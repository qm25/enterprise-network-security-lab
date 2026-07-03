# VLAN Design

## Why Segment Into VLANs at All

A flat network (everyone on one broadcast domain) means every device can see and potentially reach every other device. That's a problem for two reasons: performance (broadcast traffic scales with every host on the segment) and security (a compromised or untrusted device has a direct path to sensitive systems). Splitting HQ into VLANs solves both, and lets each department's traffic be controlled independently at Layer 3.

## The Three HQ VLANs

**VLAN 10 — SALES**
Sales staff have no legitimate need to reach IT infrastructure or admin systems. Keeping them on their own VLAN means their traffic never has to be inspected against IT-specific rules — it's isolated by default, not by exception.

**VLAN 20 — IT**
IT holds the more sensitive systems in this design (in a real deployment: servers, monitoring tools, admin consoles). This VLAN is the one every other VLAN's access into is restricted, since it represents the highest-value target on the network.

**VLAN 30 — GUEST**
Guest devices are the least trusted by definition — visitors, personal devices, anything not managed by the organization. This VLAN is explicitly blocked from reaching IT (VLAN 20) via an extended ACL on R1, while still being allowed general internet access. This models a very common real-world requirement: guests get internet, nothing else.

## Why Router-on-a-Stick Instead of a Layer 3 Switch

A Layer 3 switch would be the more typical production choice for inter-VLAN routing at scale, since it routes in hardware (ASIC) rather than relying on a router's CPU. Router-on-a-stick was used here deliberately because:
- It's the standard way CCNA curriculum teaches inter-VLAN routing, and this project is meant to demonstrate that understanding directly.
- It keeps NAT, DHCP, and ACLs consolidated on a single device (R1), which simplifies the topology for a lab environment without changing the underlying VLAN/trunking concepts.
- The trade-off (all inter-VLAN traffic funnels through one router link) is a legitimate real-world limitation worth being able to explain — at higher traffic volumes, a Layer 3 switch or distributed routing would be the better design.

## Native VLAN Choice (VLAN 99)

Leaving VLAN 1 as the native/trunk VLAN is a common misconfiguration — VLAN 1 is enabled by default on every trunk and switch port, making it a predictable target for VLAN hopping attacks (double-tagging exploits rely on attacker traffic matching the native VLAN of a trunk). Using a dedicated, non-default VLAN (99) as native, and disabling VLAN 1 wherever unused, removes that default assumption an attacker could otherwise rely on.

## Branch Site — No VLANs

The Branch site (SW3, R2) intentionally has no VLAN segmentation — it's a single small office LAN with no differentiated trust levels between devices in this design. This is a realistic scope decision: not every site needs the same complexity, and over-engineering a two-PC branch office with multiple VLANs would add operational overhead without a corresponding security or performance benefit.
