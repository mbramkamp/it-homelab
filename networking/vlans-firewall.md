# VLAN and Firewall Lab

## Objective

Segment the homelab into logical networks and control traffic between them with explicit firewall policies.

## Planned Segments

- Management
- Servers
- User workstations
- Isolated lab/IoT systems

Final design: TBD

## Concepts to Practice

- 802.1Q VLAN tagging
- Access vs trunk ports
- Inter-VLAN routing
- Default gateways
- Stateful firewall rules
- NAT
- Least-privilege network access
- Packet capture and traffic analysis

## Validation

For each VLAN, document which other networks/services it should and should not reach, then test those assumptions.

## Practice Incidents

- [ ] Wrong VLAN assignment
- [ ] Missing firewall rule
- [ ] DNS allowed but application traffic blocked
- [ ] Client can reach gateway but not another subnet
- [ ] Asymmetric or unexpected routing

## What I Learned

TBD
