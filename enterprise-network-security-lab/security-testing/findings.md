# Security Testing Report

## Overview
This document summarizes the verification and security validation performed after implementing the enterprise network.

The purpose of these tests is to ensure that routing, switching, security controls, and management configurations operate as intended.

---

## Test 1 — VLAN Verification

**Objective**
Verify that all required VLANs were successfully created and assigned.

**Command**
```
show vlan brief
```

**Expected Result**
- VLAN 10 present
- VLAN 20 present
- VLAN 30 present
- VLAN 99 present

**Actual Result**
All VLANs were successfully created and operational.

**Security Benefit**
Network segmentation reduces broadcast domains and separates users based on organizational roles.

---

## Test 2 — EtherChannel Verification

**Objective**
Verify that LACP EtherChannel is operational.

**Command**
```
show etherchannel summary
```

**Expected Result**
Port-channel 1 displays an "SU" state indicating a Layer 2 EtherChannel is active.

**Actual Result**
EtherChannel formed successfully between SW1 and SW2.

![EtherChannel Summary](enterprise-network-security-lab/security-testing/screenshots/etherchannel-summary.png)

**Security / Reliability Benefit**
Provides redundancy and increased bandwidth while preventing single-link failures.

---

## Test 3 — OSPF Neighbor Verification

**Objective**
Verify dynamic routing adjacency.

**Command**
```
show ip ospf neighbor
```

**Expected Result**
Routers establish FULL neighbor relationships.

**Actual Result**
OSPF adjacency successfully formed between routers.

![OSPF Neighbors](enterprise-network-security-lab/security-testing/screenshots/ospf-neighbors.png)

**Benefit**
Dynamic routing automatically exchanges network reachability information without requiring manual static routes.

---

## Test 4 — DHCP Verification

**Objective**
Verify automatic IP address assignment.

**Procedure**
Configured each PC to obtain an address using DHCP.

**Expected Result**
Each device receives an address from the appropriate subnet.

**Actual Result**
All workstations successfully received valid IP configurations.

**Benefit**
Centralized address management reduces configuration errors.

---

## Test 5 — NAT Verification

**Objective**
Verify internal addresses are translated before leaving the enterprise network.

**Command**
```
show ip nat translations
```

**Procedure**
Generated traffic by pinging the simulated ISP router.

**Expected Result**
Translation entries appear.

**Actual Result**
NAT overload translated private addresses successfully.

**Benefit**
Allows multiple internal hosts to share public address space.

---

## Test 6 — Guest VLAN ACL

**Objective**
Verify Guest users cannot access internal IT resources.

**Procedure**
From a Guest VLAN workstation:
```
ping 192.168.20.x
```

**Expected Result**
Ping fails.

**Actual Result**
Access was denied as expected.

![Guest VLAN Blocked from IT VLAN](enterprise-network-security-lab/security-testing/screenshots/acl-guest-blocked.png)

**Benefit**
Protects sensitive internal systems from guest devices.

---

## Test 7 — Guest Internet Access

**Objective**
Verify Guest users still have external connectivity.

**Procedure**
```
ping 10.10.10.1
```

**Expected Result**
Ping succeeds.

**Actual Result**
Traffic successfully reached the simulated ISP.

![Guest VLAN Internet Access Allowed](enterprise-network-security-lab/security-testing/screenshots/acl-guest-internet-allowed.png)

**Benefit**
Allows guest Internet access without exposing internal networks.

---

## Test 8 — Port Security

**Objective**
Verify unauthorized devices cannot use protected switch ports.

**Procedure**
A different workstation was connected to a secured access port.

**Command**
```
show port-security interface fastEthernet0/1
```

**Expected Result**
Violation counter increases and unauthorized traffic is restricted.

**Actual Result**
Port security detected the unauthorized MAC address and restricted traffic.

![Port Security Violation](enterprise-network-security-lab/security-testing/screenshots/port-security-violation.png)

**Benefit**
Prevents unauthorized devices from accessing the network.

---

## Test 9 — SSH Verification

**Objective**
Verify secure remote administration.

**Procedure**
Attempted Telnet followed by SSH.

**Expected Result**
- Telnet refused
- SSH successful

**Actual Result**
Telnet connections were denied while SSH login succeeded using local credentials.

![Telnet Refused]enterprise-network-security-lab/security-testing/(screenshots/telnet-refused.png)

![SSH Login Successful](enterprise-network-security-lab/security-testing/screenshots/ssh-success.png)

**Benefit**
Encrypted remote management protects administrator credentials from interception.

---

## Overall Assessment

All planned network services and security mechanisms operated successfully.

The completed implementation demonstrates:

- Enterprise VLAN segmentation
- Router-on-a-Stick inter-VLAN routing
- Dynamic routing with OSPF
- DHCP services
- NAT overload
- Layer 2 EtherChannel
- Port Security
- ACL-based network segmentation
- Secure remote management using SSH
- Infrastructure hardening through service reduction and unused port shutdown

This project demonstrates practical implementation of core CCNA enterprise networking concepts together with foundational security best practices.
