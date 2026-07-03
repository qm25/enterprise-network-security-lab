# IP Addressing Table

## HQ VLANs (on R1, via router-on-a-stick)

| VLAN | Name | Subnet | Gateway | DHCP Range | Purpose |
|---|---|---|---|---|---|
| 10 | SALES | 192.168.10.0/24 | 192.168.10.1 | .11–.254 | Sales department hosts |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 | .11–.254 | IT department hosts |
| 30 | GUEST | 192.168.30.0/24 | 192.168.30.1 | .11–.254 | Guest / untrusted devices |
| 99 | NATIVE/MGMT | 192.168.99.0/24 | 192.168.99.1 | N/A (static) | Trunk native VLAN + switch management |

## Switch Management IPs (VLAN 99 SVIs)

| Device | IP Address |
|---|---|
| SW1 | 192.168.99.2 |
| SW2 | 192.168.99.3 |

## Branch LAN

| Network | Subnet | Gateway | DHCP Range |
|---|---|---|---|
| Branch LAN (VLAN 1) | 192.168.40.0/24 | 192.168.40.1 | .11–.254 |

| Device | IP Address |
|---|---|
| SW3 (VLAN 1 SVI) | 192.168.40.2 |

## WAN Links (Point-to-Point Serial)

| Link | Subnet | R0 (DCE) | Remote Router (DTE) |
|---|---|---|---|
| R0 ↔ R1 | 10.10.10.0/30 | 10.10.10.1 (Se0/0/0) | R1: 10.10.10.2 (Se0/1/0) |
| R0 ↔ R2 | 10.10.20.0/30 | 10.10.20.1 (Se0/0/1) | R2: 10.10.20.2 (Se0/1/0) |

## NAT

| Item | Value |
|---|---|
| NAT type | PAT (overload) |
| Inside networks | 192.168.10.0/24, 192.168.20.0/24, 192.168.30.0/24 |
| Outside interface | R1 Serial0/1/0 |
| Public pool | 200.100.100.1–200.100.100.6/29 (simulated) |

## Design Notes
- VLAN 99 was chosen as the native/trunk VLAN rather than leaving the default VLAN 1 as native, to avoid VLAN hopping attacks that rely on native VLAN mismatch — this also doubles as the out-of-band management network for switch access.
- DHCP exclusions (.1–.10) were reserved on every subnet for static assignments (gateways, servers, management addresses), leaving .11 onward for dynamic client leases.
- Point-to-point /30 subnets were used on WAN links to avoid wasting address space, standard practice for router-to-router links.
