# Network Addressing

## Objective

Document the homelab's IP addressing and subnet design.

## Initial Network

| Network | Purpose | Subnet | Gateway | DHCP/Static |
|---|---|---|---|---|
| Management | Proxmox management | TBD | TBD | TBD |
| Lab | Initial VM network | TBD | TBD | TBD |

## Future VLAN Plan

| VLAN | Name | Purpose | Subnet |
|---:|---|---|---|
| TBD | Users | Client systems | TBD |
| TBD | Servers | Server systems | TBD |
| TBD | Management | Infrastructure management | TBD |
| TBD | IoT/Lab | Isolated devices/testing | TBD |

## Static Assignments

| Hostname | Role | Address |
|---|---|---|
| Proxmox host | Hypervisor | TBD |
| DC01 | Domain Controller / DNS | TBD |
| FILE01 | File Server | TBD |
| LINUX01 | Linux Server | TBD |

## Notes

Do not publish sensitive public addressing or credentials. Private RFC1918 lab addressing is safe to document when useful.
