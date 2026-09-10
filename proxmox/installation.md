# Proxmox VE Installation

## Objective

Install Proxmox VE on a spare physical PC and establish the virtualization platform for the homelab. The host is intended to run headless and be administered remotely through the Proxmox web interface and shell.

## Hardware

| Component | Specification |
|---|---|
| CPU | Intel(R) Core(TM) i7-4790K CPU @ 4.00GHz|
| RAM | 12G |
| Primary storage | ~223.6 GiB SATA SSD |
| Secondary storage | ~223.6 GiB SATA SSD (currently contains an NTFS partition; not yet repurposed) |
| Network adapter | Intel Ethernet using the `e1000e` driver |
| Firmware | ASRock UEFI |

## Installation Media

- Proxmox VE ISO version: **USER TODO: add exact version downloaded**
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

Proxmox is installed on one of the SATA SSDs using the default LVM-based installation layout. Inspection after installation showed:

```text
Proxmox SSD
├── EFI System Partition (~1 GiB, FAT32)
└── LVM
    ├── pve-root
    ├── pve-swap
    └── pve-data
```

The second SATA SSD is visible to Proxmox but currently contains an NTFS partition. It will not be wiped or repurposed until its contents are confirmed unnecessary.

> Note: Linux device names such as `/dev/sda` and `/dev/sdb` changed after the SATA drives were physically rearranged. Physical disks should not be identified permanently by `sdX` names alone.

## Headless Configuration

The host was prepared for headless operation by:

- Verifying the Proxmox web interface was reachable remotely
- Verifying the Proxmox host shell was accessible through the web interface
- Removing the installation USB and testing an unattended reboot
- Confirming both SATA SSDs are detected
- Configuring ASRock UEFI to automatically power the computer on after AC power is restored
- Confirming Proxmox returns to `192.168.0.11` after reboot

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
- [ ] DNS resolution formally tested/documented
- [ ] Package repositories configured and updates applied
- [ ] Secondary SSD configured for Proxmox storage

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
- What part of the process do you now understand better than before?

**Your answer:**

TBD

## Next Steps

- Configure Proxmox repositories and install updates
- Confirm the secondary SSD can be safely erased
- Configure secondary Proxmox storage
- Upload operating-system ISOs
- Create VM standards
- Deploy `DC01` as the first Windows Server VM
