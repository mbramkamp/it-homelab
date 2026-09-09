# IT Homelab

A practical homelab built to develop hands-on IT administration, networking, virtualization, Windows Server, Linux, and troubleshooting skills.

## Goals

- Build a small business-style IT environment from the ground up
- Gain hands-on experience with Proxmox, Windows Server, Active Directory, DNS, DHCP, Group Policy, Linux, and networking
- Practice diagnosing and resolving realistic support and infrastructure issues
- Document projects and incidents in a way that can be discussed during technical interviews
- Build a public portfolio of practical IT work

## Planned Environment

```text
Physical Homelab Server
└── Proxmox VE
    ├── DC01      Windows Server / Active Directory / DNS
    ├── CLIENT01  Windows 11 domain client
    ├── FILE01    Windows Server file services
    └── LINUX01   Ubuntu Server
```

Future additions may include VLANs, a virtual firewall, Docker, monitoring, centralized logging, backups, Microsoft 365/Entra ID labs, and additional Windows/Linux clients.

## Current Status

- [ ] Install Proxmox VE
- [ ] Configure Proxmox networking and storage
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
- Docker and containerized services
- Monitoring and logging
- Backup and recovery
- Technical troubleshooting
- Infrastructure documentation

## Security Notes

This is a public repository. Passwords, private keys, API tokens, real public IP addresses, employer information, and other sensitive data will not be committed.
