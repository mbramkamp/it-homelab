# Incident: Proxmox UEFI Boot Failure

**Date:** 2026-09-10  
**Affected system:** `pve01`  
**Impact:** Hypervisor could not boot normally from its SSD  
**Status:** Resolved

## Problem

After Proxmox VE had been successfully installed, booted, remotely administered, and rebooted without the installation USB, the server later stopped booting normally. Firmware displayed:

```text
Reboot and Select proper Boot device
or Insert Boot Media in selected Boot device and press a key
```

Because the host never reached Proxmox, its management interface at `192.168.0.11` was also unreachable.

## Symptoms

- Both SATA SSDs were detected by ASRock UEFI.
- Selecting either SATA SSD as a boot device resulted in the same boot-device error.
- The Ethernet NIC still showed link/activity lights, but the host did not respond to ARP or ping because Proxmox had not actually booted.
- Proxmox Rescue Boot from the installer USB successfully loaded the installed Proxmox system.
- Once Rescue Boot loaded the installed system, `192.168.0.11` and the Proxmox web interface worked normally.

## Troubleshooting Process

### 1. Verified physical disk detection

ASRock UEFI detected two SATA SSDs on SATA ports. This reduced the likelihood of a disconnected SATA data cable, missing power, or completely failed drive.

**Conclusion:** Disk detection alone does not prove that firmware has a valid boot path to an operating system.

### 2. Checked firmware boot behavior

The one-time boot menu was tested with the available SATA boot options. Both returned the boot-device error. CSM/storage boot settings were also inspected while investigating whether the installation expected legacy BIOS or UEFI booting.

**Conclusion:** The failure was occurring before Proxmox/Linux loaded.

### 3. Used Proxmox Rescue Boot

The Proxmox installer USB was inserted and Rescue Boot was used rather than reinstalling the operating system.

Rescue Boot successfully loaded the existing Proxmox installation.

**Conclusion:** The Proxmox installation, root filesystem, and network configuration were intact. Reinstallation was unnecessary. The problem was isolated to the normal firmware/bootloader path.

### 4. Inspected disk layout

The following command was used:

```bash
lsblk -o NAME,SIZE,FSTYPE,PARTTYPE,MOUNTPOINTS
```

The running system showed a Proxmox disk containing:

- A ~1 GiB FAT32 EFI System Partition mounted at `/boot/efi`
- An LVM physical volume containing `pve-root`, `pve-swap`, and `pve-data`

The other SATA SSD contained an NTFS partition.

The SATA disks had previously been physically rearranged, which also demonstrated that Linux names such as `/dev/sda` and `/dev/sdb` should not be treated as permanent physical-disk identities.

### 5. Inspected Proxmox boot-tool state

```bash
proxmox-boot-tool status
```

returned:

```text
E: /etc/kernel/proxmox-boot-uuids does not exist.
```

Additional inspection was performed before making changes rather than assuming `proxmox-boot-tool` was responsible for this installation's boot path.

### 6. Verified the EFI System Partition

```bash
findmnt /boot/efi
ls -R /boot/efi/EFI
```

The EFI System Partition was mounted and contained both fallback and Proxmox EFI files, including:

```text
EFI/BOOT/BOOTx64.efi
EFI/proxmox/grubx64.efi
EFI/proxmox/shimx64.efi
```

**Conclusion:** EFI boot files existed on disk.

### 7. Inspected UEFI NVRAM boot entries

```bash
efibootmgr -v
```

The firmware boot list contained generic hard-drive and USB entries but no dedicated `proxmox` UEFI entry pointing to the installed Proxmox EFI loader.

## Root Cause

The Proxmox installation and EFI files were intact, but the system firmware did not have a dedicated UEFI NVRAM boot entry pointing to the Proxmox EFI loader. As a result, normal firmware booting failed even though Rescue Boot could locate and start the installed operating system.

## Resolution

A new UEFI boot entry was created from the running Rescue Boot environment:

```bash
efibootmgr --create --disk /dev/sdb --part 2 --label "proxmox" --loader '\EFI\proxmox\shimx64.efi'
```

> `/dev/sdb` and partition `2` were correct for the disk layout at the time of repair. `sdX` device names should always be verified before reusing commands like this on another system.

The resulting firmware configuration showed:

```text
Boot0000* proxmox ... /File(\EFI\proxmox\shimx64.efi)
BootOrder: 0000,...
```

The server was then shut down, the installer USB was removed, and the system was powered on normally.

## Validation

- [x] Server booted without the Proxmox installer USB
- [x] Rescue Boot was no longer required
- [x] `192.168.0.11` became reachable after boot
- [x] Proxmox web management interface became available
- [x] `proxmox` was first in the UEFI boot order

## Prevention / Improvement

- Identify disks by model, serial number, filesystem UUID, or partition UUID when possible instead of relying solely on `/dev/sdX` names.
- Verify firmware boot mode and UEFI entries when troubleshooting a system that detects its disk but cannot boot it.
- Avoid reinstalling an operating system until determining whether the installed filesystems are actually damaged.
- Keep bootable installation/recovery media available for headless infrastructure systems.

## USER TODO — What I Learned

Write this part yourself. Try to explain the incident without looking at the sections above word-for-word.

Questions to consider:

1. Why did the Ethernet lights initially make the problem look like a networking issue?
2. What evidence proved that Proxmox itself was still intact?
3. What is an EFI System Partition?
4. What does a UEFI boot entry do?
5. Why was repairing the boot entry preferable to reinstalling Proxmox?
6. Why can `/dev/sda` and `/dev/sdb` change?

**Your answer:**

TBD

## USER TODO — Interview Summary

Fill this out in your own words. Keep each section to roughly 1-3 sentences.

**Situation:**  
TBD

**Task:**  
TBD

**Action:**  
TBD

**Result:**  
TBD

### Interview Practice

After completing the STAR section, practice answering this question aloud without reading the document:

> Tell me about a time you diagnosed a system that would not boot. How did you isolate the problem and resolve it?
