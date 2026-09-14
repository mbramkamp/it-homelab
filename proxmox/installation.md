# Proxmox VE Installation

## Objective

Install Proxmox VE on a spare physical PC and establish the virtualization platform for the homelab. The host is intended to run headless and be administered remotely through the Proxmox web interface and shell.

## Hardware

| Component | Specification |
|---|---|
| CPU | Intel(R) Core(TM) i7-4790K CPU @ 4.00GHz |
| RAM | 12 GB |
| Primary storage | ~223.6 GiB SATA SSD, serial `18060624002002` |
| Secondary storage | ~223.6 GiB SATA SSD, serial `19061924001945` |
| Network adapter | Intel Ethernet using the `e1000e` driver |
| Firmware | ASRock UEFI |

## Installation Media

- Proxmox VE installation/version recorded during setup: `pve-manager/9.2.2/b9984c6d90a4bd80`
- Initial kernel: `7.0.2-6-pve`
- USB creation tool: Rufus
- Installation date: 2026-09-10
- Boot mode: UEFI

The Proxmox ISO was written to a USB flash drive as bootable installation media. An initial attempt failed because the flash drive was visible to firmware but was not correctly bootable. Recreating the installation media allowed the Proxmox installer to start successfully.

## Network Configuration

| Setting | Value |
|---|---|
| Hostname | `pve01.home.arpa` |
| Management IP | `192.168.0.11/24` |
| Default gateway | `192.168.0.1` |
| DNS server | `192.168.0.1` |
| LAN subnet | `192.168.0.0/24` |
| Router DHCP pool | `192.168.0.100-192.168.0.199` |

The Proxmox management address was intentionally selected outside the router's DHCP pool so the hypervisor has a predictable static management address without overlapping dynamically assigned client addresses.

### USER TODO — Explain the Networking Decision

In your own words, answer:

> Why is a static IP useful for a server/hypervisor, and why did I choose an address outside the DHCP pool?

**Your answer:**

TBD

## Storage Configuration

### Proxmox system SSD

Proxmox is installed on the SATA SSD with serial `18060624002002`. The installation uses an EFI System Partition plus the default LVM-based layout:

```text
System SSD
├── EFI System Partition (~1 GiB, FAT32)
└── LVM PV (/dev/sdb3 when documented)
    └── VG "pve"
        ├── LV "root"  ~65.5 GiB
        ├── LV "swap"    8 GiB
        └── thin pool "data" ~129.85 GiB
            └── exposed by Proxmox as local-lvm
```

The `pve` volume group also retained approximately 16 GiB of unallocated capacity when inspected.

### Secondary VM storage SSD

The second SATA SSD, serial `19061924001945`, was initially identified as `/dev/sda` and contained a single NTFS filesystem labeled `Gamer Time`. Its identity was verified by model, serial number, filesystem, and contents before any destructive operation was performed.

After confirming that the existing data was no longer required, the disk was wiped through **pve01 > Disks > Disks** in the Proxmox web interface. It was then configured as dedicated LVM-Thin storage named `lab-vm-storage`.

The resulting storage stack is:

```text
Secondary SSD (~223.6 GiB)
└── LVM Physical Volume (PV)
    └── Volume Group (VG) "lab-vm-storage"
        └── LVM thin pool "lab-vm-storage" (~218.97 GiB)
            └── future VM and container virtual disks
```

The intent is to use this SSD primarily for lab VM/CT disks while keeping the Proxmox operating system and its existing local storage on the first SSD.

LVM-Thin was selected because it provides block storage suitable for Proxmox VM disks while supporting thin provisioning and snapshots. A virtual disk's configured maximum size therefore does not necessarily consume that full amount of physical SSD capacity immediately. Actual thin-pool utilization must still be monitored because thin provisioning can overcommit physical capacity.

The secondary SSD is **not a backup**. A separate backup strategy is still required for data or VM configurations that need to survive disk or host failure.

> Note: Linux device names such as `/dev/sda` and `/dev/sdb` changed after the SATA drives were physically rearranged. Physical disks should not be identified permanently by `sdX` names alone; model, serial number, UUID, and storage metadata provide safer identification.

### LVM validation

The underlying configuration was inspected from the Proxmox shell with:

```bash
pvs
vgs
lvs
```

The resulting LVM hierarchy was:

```text
PV /dev/sda
└── VG lab-vm-storage (~223.57 GiB)
    └── thin LV lab-vm-storage (~218.97 GiB)
        Data usage: 0.00%
        Metadata usage: 0.74%

PV /dev/sdb3
└── VG pve (~222.00 GiB)
    ├── LV root (~65.50 GiB)
    ├── LV swap (8.00 GiB)
    └── thin LV data (~129.85 GiB)
```

This validated the relationship between the physical disks, LVM physical volumes, volume groups, logical volumes, and the thin pools presented to Proxmox for VM storage.

## Headless Configuration

The host was prepared for headless operation by:

- Verifying the Proxmox web interface was reachable remotely
- Verifying the Proxmox host shell was accessible through the web interface
- Removing the installation USB and testing an unattended reboot
- Confirming both SATA SSDs are detected
- Configuring ASRock UEFI to automatically power the computer on after AC power is restored
- Confirming Proxmox returns to `192.168.0.11` after reboot

## Repository Configuration and Initial Updates

The default Proxmox enterprise repositories require a paid subscription. Because this system is a non-production homelab, the enterprise Proxmox and Ceph repository entries were disabled and the `pve-no-subscription` repository was enabled through the Proxmox web interface.

Repository connectivity was validated with:

```bash
apt update
```

APT successfully retrieved package metadata from Debian 13 (Trixie), Debian Security, and the Proxmox `pve-no-subscription` repository without repository errors. At that point, 135 installed packages had available upgrades.

Before changing the host, the proposed upgrade was inspected with:

```bash
apt list --upgradable
apt -s full-upgrade
```

The `-s` option simulated the full upgrade without making changes. After reviewing the proposed package operations, the host was upgraded with:

```bash
apt full-upgrade
```

The upgrade included Proxmox management packages, security/library updates, QEMU components, firmware, and a newer Proxmox kernel.

### Post-upgrade validation

Immediately after the upgrade but before rebooting:

```text
pve-manager: 9.2.18
running kernel: 7.0.2-6-pve
```

This demonstrated that installing a new kernel does not replace the kernel currently running in memory. A reboot is required to load it.

Because this host previously experienced a UEFI boot failure, the firmware boot entry was checked before rebooting:

```bash
efibootmgr
```

The result confirmed that the repaired `proxmox` UEFI entry still existed, was first in `BootOrder`, and was the entry used for the current boot.

After rebooting, the host returned normally and validation showed:

```text
pve-manager/9.2.18/614bede5d65599c6
running kernel: 7.0.14-16-pve
```

This confirmed that the package upgrade succeeded, the new kernel loaded successfully, networking returned, remote management remained available, and the repaired UEFI boot configuration survived the update/reboot cycle.

## Validation

- [x] Proxmox installed successfully
- [x] Host boots without the installer USB
- [x] Proxmox web interface is reachable remotely
- [x] Management network connectivity works
- [x] Static management IP persists after reboot
- [x] Remote host shell works
- [x] Both SATA SSDs are detected
- [x] Host can operate without monitor or keyboard
- [x] Firmware configured to power on after AC power restoration
- [x] Package repositories configured for homelab use
- [x] APT repository connectivity validated
- [x] Initial full system upgrade completed
- [x] New Proxmox kernel validated after reboot
- [x] Secondary SSD identified by serial before destructive changes
- [x] Secondary SSD wiped and configured as dedicated LVM-Thin storage
- [x] LVM configuration validated with `pvs`, `vgs`, and `lvs`
- [ ] DNS resolution formally tested/documented

## Problems Encountered

### Installation USB did not initially boot

The motherboard detected the USB flash drive, but attempting to boot from it returned `Reboot and Select proper Boot device`. The installation media was recreated correctly and subsequently booted into the Proxmox installer.

### Proxmox SSD stopped booting normally

After relocating/rearranging hardware, the system returned `Reboot and Select proper Boot device`. Proxmox Rescue Boot successfully loaded the existing installation, demonstrating that the operating system and data were intact. Further investigation identified a missing dedicated UEFI boot entry. See [Proxmox UEFI Boot Failure](../troubleshooting/proxmox-uefi-boot-failure.md).

## USER TODO — What I Learned

Write this section yourself. Aim for 3-6 sentences. Some questions to answer:

- What is the difference between an SSD being detected and actually being bootable?
- What did you learn about UEFI/EFI booting?
- Why shouldn't you assume `/dev/sda` always refers to the same physical disk?
- What did you learn about static IP addressing?
- What did you learn about Linux package repositories and `apt`?
- Why did the running kernel remain at `7.0.2-6-pve` until the server rebooted?
- How would you explain a PV, VG, LV, and LVM thin pool?
- Why does thin provisioning require capacity monitoring?
- What part of the process do you now understand better than before?

**Your answer:**

TBD

## Next Steps

- Upload operating-system ISOs
- Create VM standards
- Deploy `DC01` as the first Windows Server VM
- Develop a separate backup strategy for important lab workloads
