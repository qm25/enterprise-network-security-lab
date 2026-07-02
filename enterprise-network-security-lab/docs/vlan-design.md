# VLAN Design

## Overview

The headquarters network is segmented using Virtual Local Area Networks (VLANs) to improve network organization, reduce broadcast traffic, and enhance security. Each department is isolated into its own broadcast domain while still allowing controlled communication through Router-on-a-Stick inter-VLAN routing.

---

# VLAN Layout

| VLAN ID | Name | Purpose |
|----------|------|---------|
| 10 | SALES | Sales department workstations |
| 20 | IT | IT department workstations |
| 30 | GUEST | Guest devices |
| 99 | NATIVE / MANAGEMENT | Switch management and native trunk VLAN |

---

# Design Objectives

The VLAN architecture was designed to achieve the following goals:

- Separate departments into individual broadcast domains.
- Reduce unnecessary broadcast traffic.
- Improve network scalability.
- Increase security by isolating sensitive systems.
- Allow controlled communication through Layer 3 routing.
- Simplify network administration.

---

# Router-on-a-Stick

Inter-VLAN routing is implemented using Router-on-a-Stick on the HQ router.

A single physical GigabitEthernet interface is divided into multiple IEEE 802.1Q subinterfaces:

- VLAN 10
- VLAN 20
- VLAN 30
- VLAN 99 (Native)

Each subinterface functions as the default gateway for its respective VLAN.

---

# Trunk Configuration

802.1Q trunking is configured between:

- HQ Router ↔ Core Switch
- Core Switch ↔ Access Switch (EtherChannel)

The native VLAN is configured as VLAN 99.

---

# EtherChannel

An EtherChannel using LACP combines two physical links between SW1 and SW2.

Benefits include:

- Higher bandwidth
- Link redundancy
- Improved fault tolerance
- Better load distribution

---

# Security Design

Several security mechanisms are implemented throughout the network.

## Guest Isolation

An extended ACL prevents devices in VLAN 30 from accessing the IT department subnet while allowing normal outbound traffic.

This protects internal company resources from unauthorized guest devices.

---

## Port Security

Port security is enabled on user-facing switch ports.

Configuration includes:

- Maximum of one MAC address
- Sticky MAC learning
- Restrict violation mode

This limits unauthorized devices from connecting to the enterprise network.

---

## Secure Management

The management plane has been hardened by:

- Enabling SSH Version 2
- Disabling Telnet
- Creating local administrator accounts
- Encrypting stored passwords
- Configuring an enable secret
- Displaying a legal login banner

---

## Disabled Services

To reduce unnecessary attack surface:

- HTTP server disabled
- HTTPS server disabled
- CDP disabled
- Unused switch ports administratively shut down

---

# Design Summary

The VLAN architecture provides logical separation between departments while maintaining secure communication through controlled Layer 3 routing. The implementation follows common enterprise networking practices used in modern business environments and demonstrates both network segmentation and infrastructure hardening.
