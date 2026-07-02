# IP Addressing Table

## Overview

This document provides the IP addressing plan for the Enterprise Network Design & Security Hardening Lab. The network is divided into multiple VLANs at the headquarters site and a separate subnet at the branch office. Dynamic routing is provided by OSPF, while DHCP automatically assigns IP addresses to client devices.

---

## Network Addressing

| Device | Interface | IP Address | Subnet Mask | Purpose |
|---------|-----------|------------|-------------|---------|
| R0 (ISP) | Serial0/0/0* | 10.10.10.1 | 255.255.255.252 | WAN Link to HQ |
| R0 (ISP) | Serial0/0/1* | 10.10.20.1 | 255.255.255.252 | WAN Link to Branch |
| R1 (HQ) | GigabitEthernet0/0/0.10 | 192.168.10.1 | 255.255.255.0 | VLAN 10 Default Gateway |
| R1 (HQ) | GigabitEthernet0/0/0.20 | 192.168.20.1 | 255.255.255.0 | VLAN 20 Default Gateway |
| R1 (HQ) | GigabitEthernet0/0/0.30 | 192.168.30.1 | 255.255.255.0 | VLAN 30 Default Gateway |
| R1 (HQ) | GigabitEthernet0/0/0.99 | 192.168.99.1 | 255.255.255.0 | Management VLAN Gateway |
| R1 (HQ) | Serial0/0/0* | 10.10.10.2 | 255.255.255.252 | WAN Link to ISP |
| R2 (Branch) | GigabitEthernet0/0/1 | 192.168.40.1 | 255.255.255.0 | Branch LAN Gateway |
| R2 (Branch) | Serial0/0/0* | 10.10.20.2 | 255.255.255.252 | WAN Link to ISP |
| SW1 | VLAN 99 | 192.168.99.2 | 255.255.255.0 | Switch Management |
| SW2 | VLAN 99 | 192.168.99.3 | 255.255.255.0 | Switch Management |
| SW3 | VLAN 1 | 192.168.40.2 | 255.255.255.0 | Switch Management |

*Serial interface numbers may vary depending on the Packet Tracer hardware module installed (for example, Serial0/1/0 instead of Serial0/0/0).

---

## VLAN Subnets

| VLAN | Network | Gateway | Purpose |
|------|---------|---------|---------|
| VLAN 10 | 192.168.10.0/24 | 192.168.10.1 | Sales Department |
| VLAN 20 | 192.168.20.0/24 | 192.168.20.1 | IT Department |
| VLAN 30 | 192.168.30.0/24 | 192.168.30.1 | Guest Network |
| VLAN 99 | 192.168.99.0/24 | 192.168.99.1 | Management Network |
| Branch LAN | 192.168.40.0/24 | 192.168.40.1 | Branch Office |

---

## DHCP Configuration

DHCP services are configured on the routers.

The following DHCP pools are available:

- SALES_POOL
- IT_POOL
- GUEST_POOL
- BRANCH_POOL

The first ten addresses of each subnet are excluded to reserve static addresses for network infrastructure devices.

---

## Routing Summary

Dynamic routing between headquarters and the branch office is provided using OSPF Area 0.

A default route is configured toward the simulated ISP to provide Internet connectivity through NAT overload.
