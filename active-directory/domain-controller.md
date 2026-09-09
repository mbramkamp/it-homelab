# Domain Controller Build

## Objective

Deploy the first Windows Server domain controller and build an Active Directory environment representing a small business.

## Planned Role

**Hostname:** `DC01`

Planned services:

- Active Directory Domain Services (AD DS)
- DNS

DHCP may initially be added to the Windows environment or implemented elsewhere depending on the networking design.

## VM Configuration

| Setting | Value |
|---|---|
| vCPU | TBD |
| Memory | TBD |
| Disk | TBD |
| Network | TBD |
| Windows Server Version | TBD |

## Domain Design

- AD DNS domain: TBD
- NetBIOS name: TBD
- Organizational unit structure: TBD

## Build Notes

Document:

1. Windows Server installation
2. Hostname configuration
3. Static IP configuration
4. AD DS role installation
5. Domain/forest creation
6. DNS configuration
7. Post-installation validation

## Validation

- [ ] DC01 resolves its own DNS records
- [ ] AD DS services are running
- [ ] DNS is functioning
- [ ] A test user can be created
- [ ] A Windows client can locate the domain
- [ ] A Windows client can join the domain

## Problems Encountered

TBD

## What I Learned

TBD
