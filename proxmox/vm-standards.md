# Virtual Machine Standards

This document records the conventions used for virtual machines in the homelab.

## Naming Convention

Initial planned systems:

| Hostname | Role | Operating System |
|---|---|---|
| DC01 | Domain Controller / DNS | Windows Server |
| CLIENT01 | Domain workstation | Windows 11 |
| FILE01 | File Server | Windows Server |
| LINUX01 | General Linux server | Ubuntu Server |

## Resource Allocation

Record CPU, memory, disk, and networking decisions for each VM as the environment is built.

## Networking

- Default virtual bridge: TBD
- VLAN strategy: TBD
- IP addressing strategy: TBD

## Snapshot Policy

Snapshots are useful before major configuration changes but are not treated as backups.

Document:

- Why a snapshot was taken
- When it can be removed
- Whether a separate backup exists for important systems

## General Principles

- Assign only the resources a VM needs
- Use consistent hostnames
- Document static IP assignments
- Keep credentials and secrets out of GitHub
- Take snapshots before risky configuration changes
- Test recovery, not just backup creation
