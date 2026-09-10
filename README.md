# IT Homelab

A practical homelab built to develop hands-on IT administration, networking, virtualization, Windows Server, Linux, and troubleshooting skills.

## Goals

- Build a small business-style IT environment from the ground up
- Gain hands-on experience with Proxmox, Windows Server, Active Directory, DNS, DHCP, Group Policy, Linux, and networking
- Practice diagnosing and resolving realistic support and infrastructure issues
- Document projects and incidents in a way that can be discussed during technical interviews
- Build a public portfolio of practical IT work

## Current Environment

```text
Home LAN: 192.168.0.0/24
        |
        +-- Router/Gateway: 192.168.0.1
        |
        +-- PVE01: 192.168.0.11
            Proxmox VE (headless)
            |
            +-- DC01      Windows Server / Active Directory / DNS (planned)
            +-- CLIENT01  Windows 11 domain client (planned)
            +-- FILE01    Windows Server file services (planned)
            +-- LINUX01   Ubuntu Server (planned)
```

Future additions may include VLANs, a virtual firewall, Docker, monitoring, centralized logging, backups, Microsoft 365/Entra ID labs, and additional Windows/Linux clients.

## Current Status

- [x] Install Proxmox VE
- [x] Configure static Proxmox management networking
- [x] Validate unattended/headless operation
- [x] Troubleshoot and repair a UEFI boot failure
- [ ] Configure Proxmox repositories and updates
- [ ] Configure secondary SSD as Proxmox storage
- [ ] Deploy Windows Server VM
- [ ] Configure Active Directory Domain Services
- [ ] Configure DNS
- [ ] Deploy Windows 11 client
- [ ] Join Windows client to domain
- [ ] Create users, groups, and organizational units
- [ ] Configure Group Policy
- [ ] Deploy file server
- [ ] Configure role-based NTFS/share permissions
- [ ] Deploy Ubuntu Server
- [ ] Add network segmentation/VLANs
- [ ] Deploy Docker services
- [ ] Implement monitoring and backups

## Completed Work

### Proxmox Hypervisor Deployment

Deployed Proxmox VE on a spare physical PC using SATA SSD storage and configured the host for remote/headless administration. The hypervisor uses a static management address outside the LAN DHCP pool and is configured to automatically power on after AC power restoration.

See [Proxmox VE Installation](proxmox/installation.md).

### UEFI Boot Failure Recovery

Diagnosed a post-installation boot failure where both SATA SSDs were visible to firmware but the system could not start Proxmox normally. Used Proxmox Rescue Boot, Linux disk/partition inspection, EFI filesystem inspection, and `efibootmgr` to isolate the problem to the firmware boot path and restore a dedicated Proxmox UEFI boot entry without reinstalling the operating system.

See [Proxmox UEFI Boot Failure](troubleshooting/proxmox-uefi-boot-failure.md).

## Documentation

- [Proxmox](proxmox/)
- [Active Directory](active-directory/)
- [Networking](networking/)
- [Windows](windows/)
- [Linux](linux/)
- [Docker](docker/)
- [Troubleshooting](troubleshooting/)
- [Projects](projects/)

## Documentation Approach

For technical changes, I document:

1. Objective
2. Environment and prerequisites
3. Configuration performed
4. Validation/testing
5. Problems encountered
6. Troubleshooting process
7. Root cause
8. Resolution
9. Lessons learned

This repository intentionally focuses on the reasoning and troubleshooting behind each project rather than only recording successful configurations.

## Skills Demonstrated

As the lab develops, this repository will demonstrate experience with:

- Virtualization and VM lifecycle management
- Windows Server administration
- Active Directory Domain Services
- DNS and DHCP
- Group Policy
- Identity and access management
- NTFS and SMB permissions
- TCP/IP networking and subnetting
- VLANs, routing, NAT, and firewall policies
- Linux administration
- UEFI/EFI boot troubleshooting
- Docker and containerized services
- Monitoring and logging
- Backup and recovery
- Technical troubleshooting
- Infrastructure documentation

## Security Notes

This is a public repository. Passwords, private keys, API tokens, public-facing addressing, employer information, and other sensitive data will not be committed.
