# Security Testing — Findings

Each control below was configured, then actively tested from the affected devices to confirm it behaves as intended, rather than assumed to work simply because it was configured. Screenshots referenced are in `security-testing/screenshots/`.

---

## Test 1 — VLAN Segmentation & ACL Enforcement

**What was tested:** Whether the Guest VLAN (VLAN 30) can reach the IT VLAN (VLAN 20), and whether it can still reach the outside network.

**Commands used (from PC4, Guest VLAN):**
```
ping 192.168.20.11
ping 10.10.10.1
```

**Result:**
- Ping to the IT VLAN host: **failed** (Request timed out) — see `screenshots/acl-guest-blocked.png`
- Ping to the simulated internet (R0): **succeeded** — see `screenshots/acl-guest-internet-allowed.png`

**Why it matters:** This confirms the extended ACL on R1 (`GUEST_RESTRICT`) is selectively blocking traffic — Guest devices aren't fully sandboxed, they still get functional internet access, but cannot pivot into the IT segment. Without this control, any device connecting to the Guest network (including untrusted visitor devices) would have a direct path into internal IT systems.

---

## Test 2 — Port Security Violation

**What was tested:** Whether connecting an unrecognized device to a port already secured with a sticky MAC address triggers a violation and restricts traffic.

**Commands used (on SW2):**
```
show port-security interface fastEthernet 0/1
```

**Result:** After disconnecting the authorized device and connecting a different device to the same port, the security violation counter incremented and traffic from the new MAC address was restricted rather than forwarded — see `screenshots/port-security-violation.png`.

**Why it matters:** Port security prevents a common physical-access attack: someone unplugging a legitimate device and plugging in their own laptop to gain network access from a trusted switchport. Without this control, physical access to an office jack would be equivalent to full network access.

---

## Test 3 — Remote Management Hardening (SSH-Only Access)

**What was tested:** Whether Telnet access to network devices is disabled, and whether SSH functions correctly as the replacement.

**Commands used (from a PC):**
```
telnet 192.168.10.1
ssh -l admin 192.168.10.1
```

**Result:**
- Telnet attempt: **refused** — see `screenshots/telnet-refused.png`
- SSH attempt: **succeeded**, prompted for credentials, logged in cleanly — see `screenshots/ssh-success.png`

**Why it matters:** Telnet sends credentials and session data in plaintext, making it trivial to intercept on a shared network segment. Restricting VTY lines to SSH-only (`transport input ssh`) ensures management traffic is encrypted, closing off a common and historically well-exploited attack vector against network infrastructure.

---

## Test 4 — Routing & Redundancy Verification

**What was tested:** Whether OSPF successfully formed neighbor relationships across the WAN, and whether the EtherChannel link between the HQ core and access switches was active and load-balancing.

**Commands used:**
```
show ip ospf neighbor          (on R1)
show etherchannel summary      (on SW1)
```

**Result:**
- R1 shows R0 as a FULL OSPF neighbor — see `screenshots/ospf-neighbors.png`
- SW1's Port-channel1 shows both member links in an active "SU" (in use) state — see `screenshots/etherchannel-summary.png`

**Why it matters:** These aren't security controls, but they validate that the underlying network is actually functioning as designed — dynamic routing is converging correctly, and the switch-to-switch link has redundancy (if one physical link in the EtherChannel fails, traffic continues over the other without a full outage).

---

## Summary

| Control | Verified Working |
|---|---|
| VLAN segmentation (Guest → IT) | ✅ |
| Guest → Internet access retained | ✅ |
| Port security violation handling | ✅ |
| SSH-only remote access | ✅ |
| OSPF neighbor formation | ✅ |
| EtherChannel redundancy | ✅ |

All controls were tested from the perspective of an end device on the network, not just verified via configuration review — this matches the mindset of confirming a control actually stops the behavior it's meant to stop, rather than assuming a correct-looking config is sufficient.
