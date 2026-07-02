# Enterprise Network Design & Security Hardening Lab

A simulated multi-site enterprise network built in Cisco Packet Tracer, covering network design, routing, VLAN segmentation, and security hardening — end to end, from topology design through documented penetration-style testing of the security controls.

Built as a personal project applying CCNA-level networking concepts (all three CCNA chapters completed) alongside a security-first mindset, ahead of pursuing the EC-Council CEH certification.

## What This Project Demonstrates
- Designing and cabling a realistic two-site enterprise topology (HQ + Branch + simulated ISP)
- VLAN segmentation with router-on-a-stick inter-VLAN routing
- Switch redundancy via LACP EtherChannel
- Dynamic routing with OSPF (single area)
- NAT/PAT for internet-bound traffic
- DHCP services per VLAN
- Standard and extended ACLs for traffic segmentation
- Port security against unauthorized device connections
- Device hardening: SSH-only remote access, encrypted passwords, disabled unnecessary services, unused port shutdown
- **Verification of the security controls through deliberate, documented test attempts** (not just "I configured an ACL" — proof it actually blocks what it's supposed to)

## Topology

![Network Topology](topology/Topology.png)

| Site | Devices | Purpose |
|---|---|---|
| HQ | R1, SW1 (core), SW2 (access), PC1–PC4 | 3 VLANs: Sales, IT, Guest |
| Branch | R2, SW3 (access), PC5–PC6 | Single flat LAN |
| ISP (simulated) | R0 | Provides WAN transit + OSPF backbone between sites |

Full IP addressing and VLAN plan: [`docs/ip-addressing-table.md`](docs/ip-addressing-table.md) and [`docs/vlan-design.md`](docs/vlan-design.md)

## Security Testing & Findings

Rather than just listing configured features, each control was actively tested to confirm it works as intended.

| Test | What Was Checked | Result |
|---|---|---|
| VLAN/ACL segmentation | Guest VLAN pinging an IT VLAN host | **Blocked** — ACL denies cross-VLAN traffic as designed |
| Guest internet access | Guest VLAN pinging the simulated ISP | **Allowed** — confirms ACL is selective, not blocking everything |
| Port security | Connecting an unauthorized device to a secured access port | **Violation triggered** — port restricts traffic from the new MAC, logged via `show port-security` |
| Remote access hardening | Attempting Telnet vs SSH to a router | **Telnet refused, SSH accepted** — confirms VTY lines are SSH-only |

Full write-up with screenshots: [`security-testing/findings.md`](security-testing/findings.md)

## Repository Structure
```
enterprise-network-security-lab/
├── README.md
├── topology/
│   ├── enterprise-network-security-lab.pkt
│   └── topology-diagram.png
├── configs/
│   ├── R0-config.txt
│   ├── R1-config.txt
│   ├── R2-config.txt
│   ├── SW1-config.txt
│   ├── SW2-config.txt
│   └── SW3-config.txt
├── docs/
│   ├── ip-addressing-table.md
│   └── vlan-design.md
└── security-testing/
    ├── screenshots/
    └── findings.md
```

## Tools Used
- Cisco Packet Tracer 8.2.2
- Cisco IOS CLI (routers: 2911, 4321; switches: 2960)

## Background
Built while completing a Computer Science bachelor's degree (final year in progress), having completed all three CCNA course chapters, and preparing for EC-Council's Certified Ethical Hacker (CEH) certification. This project reflects both the networking fundamentals from CCNA study and an early security-focused mindset ahead of CEH — designing a network is only half the job; proving it's actually defensible is the other half.

## Contact
[Qais Mahmoud] — [https://www.linkedin.com/in/qais-mahmoud-835781404/] — [qaismahmoud1@yahoo.com]
