# Home Lab Network Topology

## Overview
This home lab is designed to simulate a small enterprise environment with segmented networks, centralized logging, identity management, and an isolated malware analysis zone. The goal is to build practical experience in IT support, system administration, SOC operations, and defensive security engineering.

---

## Physical Topology

```text
                                           INTERNET
                                               │
                                 ┌──────────────────────────┐
                                 │ Xfinity Gateway / Modem │
                                 │ (Bridge Mode preferred) │
                                 └────────────┬─────────────┘
                                              │
                                   WAN        │
                              ┌───────────────▼───────────────┐
                              │       pfSense Firewall        │
                              │      + Suricata IDS/IPS       │
                              └───────────────┬───────────────┘
                                              │
                                         LAN / TRUNK
                                              │
                              ┌───────────────▼───────────────┐
                              │       Cisco L3 Switch         │
                              │   VLANs / Inter-VLAN paths    │
                              └──────┬────────┬────────┬──────┬────────┬────────┘
                                     │        │        │      │        │
                                 VLAN 10   VLAN 20  VLAN 25 VLAN 30 VLAN 40  VLAN 666
                                Management   Home      IoT    Lab/SOC Guest    Malware
                                   Admin    Trusted Untrusted Servers/Test Temp  Isolated
