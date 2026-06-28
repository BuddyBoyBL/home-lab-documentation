# Cisco SG300 Switch Configuration (Current State) — UPDATED

## Overview

This document defines the **current, actual configuration** of the Cisco SG300-52 switch within the homelab environment.

The switch operates as a **Layer 2 access switch with dual trunk uplinks** to segment the network. One trunk goes to the firewall; the second trunk provides direct Ethernet access to the personal laptop.

All inter-VLAN routing is handled by OPNsense.

This document serves as the source of truth for:
- VLAN to port assignments
- Dual trunk configuration
- Current segmentation layout
- Device placement and access patterns

---

## Core Design

### Architecture Model
- **Switch** = Layer 2 (VLAN segmentation only)
- **Firewall (OPNsense)** = Layer 3 (routing, NAT, DHCP, DNS)
- **Dual trunk links** carry all VLAN traffic
  - GE1: Trunk to firewall (all VLANs tagged)
  - GE2: Trunk to personal laptop (all VLANs tagged)

### Design Philosophy

```
Personal Laptop:
  ├── WiFi: Home VLAN (192.168.30.x) — Personal use, internet access
  └── Ethernet (GE2 trunk): Access to all VLANs
      ├── Can access VM infrastructure (VLAN 60, 20)
      ├── Can access sandbox for testing (VLAN 50)
      └── All traffic routed through firewall policy

Infrastructure VMs:
  ├── Test VLAN (20): Lab VMs — Windows 11 test systems
  ├── Servers VLAN (60): Server infrastructure — AD, Splunk, future services
  └── Sandbox VLAN (50): Isolated testing — Malware analysis, high-risk work
```

---

## Trunk Configuration

| Port | Mode | Uplink Destination | VLANs Carried | Description |
|------|------|-------------------|---------------|-------------|
| GE1 | Trunk | OPNsense Firewall | 20, 30, 40, 50, 60, 99 | Primary firewall uplink (all VLANs tagged) |
| GE2 | Trunk | Personal Laptop | 20, 30, 40, 50, 60, 99 | Direct Ethernet access to lab VLANs + internet |

### Allowed VLANs on Both Trunks
- VLAN 20 (Test)
- VLAN 30 (Home)
- VLAN 40 (IoT)
- VLAN 50 (Sandbox)
- VLAN 60 (Servers)
- VLAN 99 (Management)

---

## VLAN to Port Mapping

### VLAN 99 — Management (192.168.99.0/24)

| Ports | Mode | Notes |
|-------|------|-------|
| GE3–GE5 | Access (Untagged) | Admin/management devices |
| GE1, GE2 | Tagged (Trunk) | Firewall + Laptop uplinks |

**Current Devices:**
- Cisco switch management interface (192.168.99.2)
- Reserved for future admin tools

---

### VLAN 60 — Servers (192.168.60.0/24)

| Ports | Mode | Notes |
|-------|------|-------|
| GE6–GE10 | Access (Untagged) | Server devices (reserved for physical servers) |
| GE1, GE2 | Tagged (Trunk) | Firewall + Laptop uplinks |

**Current Devices (via Trunk):**
- Domain Controller VM (running on personal laptop hardware, accessed via VLAN 60 tag)
- Splunk Server VM (running on personal laptop hardware, accessed via VLAN 60 tag)
- QNAP NAS (physical storage)

**Access Pattern:**
- VMs run on personal laptop but are tagged to VLAN 60
- Laptop's trunk (GE2) carries VLAN 60 frames
- VM software sees traffic as VLAN 60, routes through firewall
- Firewall applies VLAN 60 policies (e.g., inter-VLAN access controls)

---

### VLAN 20 — Test (192.168.20.0/24)

| Ports | Mode | Notes |
|-------|------|-------|
| GE11–GE20 | Access (Untagged) | Test/Lab systems (reserved for physical devices) |
| GE1, GE2 | Tagged (Trunk) | Firewall + Laptop uplinks |

**Current Devices (via Trunk):**
- Windows 11 test VMs (running on personal laptop hardware, tagged to VLAN 20)
- Lab testing systems
- Experimentation environment

**Access Pattern:**
- Test VMs run on personal laptop hypervisor
- Laptop trunk (GE2) tags VM traffic to VLAN 20
- Firewall sees them as VLAN 20 clients
- Can access servers (VLAN 60) via firewall rules
- Isolated from sandbox (VLAN 50)

---

### VLAN 30 — Home (192.168.30.0/24)

| Ports | Mode | Notes |
|-------|------|-------|
| GE21–GE30 | Access (Untagged) | Personal/household devices |
| GE1, GE2 | Tagged (Trunk) | Firewall + Laptop uplinks |

**Current Devices:**
- Personal laptop (192.168.30.x) — **WiFi connection (wireless)**
  - Used for daily personal work
  - Internet browsing
  - Productivity work
  - Does NOT connect directly via Ethernet trunk to VLAN 30
- Reserved for household devices (printers, tablets, phones)

**Access Pattern:**
- Laptop connects via WiFi (not switch)
- WiFi broadcasts VLAN 30 SSID
- Gets DHCP on 192.168.30.x
- Can access internet, home VLAN resources
- Cannot directly access lab infrastructure (separate from Ethernet trunk)

---

### VLAN 40 — IoT (192.168.40.0/24)

| Ports | Mode | Notes |
|-------|------|-------|
| GE31–GE40 | Access (Untagged) | IoT devices (low trust) |
| GE1, GE2 | Tagged (Trunk) | Firewall + Laptop uplinks |

**Current Devices:** None connected (reserved for future IoT)

**Firewall Policy (Low Trust):**
- No inter-VLAN access to servers, test, management
- Limited outbound (NTP, DNS only)
- Completely isolated

---

### VLAN 50 — Sandbox (192.168.50.0/24)

| Ports | Mode | Notes |
|-------|------|-------|
| GE41–GE45 | Access (Untagged) | Malware/isolated testing |
| GE1, GE2 | Tagged (Trunk) | Firewall + Laptop uplinks |

**Current Devices (via Trunk):** Available for malware/security testing

**Access Pattern:**
- Malware/sandbox VMs can run on laptop, tagged to VLAN 50
- Completely isolated from all other VLANs
- Blocked inbound from all sources
- Outbound to internet optional (controlled by firewall)
- No access to servers or test VMs

**Firewall Policy (Isolated):**
- Sandbox → Everything: Blocked
- Everything → Sandbox: Blocked
- Malicious content contained at network level

---

## Port Configuration Standards

### Access Ports (GE3–GE45)

| Setting | Value |
|---------|-------|
| **Port Mode** | Access |
| **Untagged VLAN** | Assigned VLAN |
| **PVID** | Matches VLAN ID |
| **All other VLANs** | Forbidden |

### Trunk Ports (GE1, GE2)

| Setting | Value |
|---------|-------|
| **Port Mode** | Trunk |
| **All VLANs** | Tagged (20, 30, 40, 50, 60, 99) |
| **Native VLAN** | Not used (all traffic tagged) |
| **PVID** | Not applicable (all tagged) |

---

## Device Access Patterns & VLAN Flow

### Scenario 1: Personal Laptop — WiFi Connection

```
Laptop (WiFi)
    ↓
WiFi AP (192.168.30.0/24 broadcast)
    ↓
Receives DHCP: 192.168.30.x
    ↓
Uses Gateway: 192.168.30.1 (OPNsense)
    ↓
Access Level:
  ✓ Internet
  ✓ Home VLAN resources
  ✗ Lab VMs (separate network)
  ✗ Servers, Test, Sandbox
```

**Use Case:** Personal work, browsing, productivity. No interaction with lab infrastructure.

---

### Scenario 2: Personal Laptop — Ethernet Access to Lab (via GE2 Trunk)

```
Laptop Ethernet Adapter → GE2 Trunk
    ↓
Trunk tags frames with VLAN ID (depends on adapter config)
    ↓
Possible VLANs:
  - VLAN 20 (Test): Access test VMs
  - VLAN 60 (Servers): Access domain controller, Splunk
  - VLAN 50 (Sandbox): Test malware analysis
    ↓
Traffic routed through firewall
    ↓
Firewall applies policies based on source/dest VLAN
```

**Use Case:** Administrative access to lab infrastructure. Can manage VMs, access Splunk logs, run tests.

---

### Scenario 3: VMs Running on Personal Laptop

```
Hypervisor (Personal Laptop Hardware)
    ├── VM 1: Domain Controller (VLAN 60 tag)
    │   ↓
    │   Laptop NIC → GE2 Trunk (tagged VLAN 60)
    │   ↓
    │   Firewall sees: 192.168.60.x server
    │
    ├── VM 2: Splunk Server (VLAN 60 tag)
    │   ↓
    │   Laptop NIC → GE2 Trunk (tagged VLAN 60)
    │   ↓
    │   Firewall sees: 192.168.60.x server
    │
    └── VM 3: Test Lab VM (VLAN 20 tag)
        ↓
        Laptop NIC → GE2 Trunk (tagged VLAN 20)
        ↓
        Firewall sees: 192.168.20.x test client
        ↓
        Can access servers via firewall rules:
          ✓ VLAN 20 → VLAN 60 (allowed)
          ✗ VLAN 20 → VLAN 40 (blocked)
          ✗ VLAN 20 → VLAN 50 (blocked)
```

**Use Case:** Infrastructure running on laptop hypervisor. VLAN tagging provides network-level segmentation and policy enforcement.

---

## DHCP & Gateway Behavior

### DHCP Server: OPNsense (Kea DHCP)

| VLAN | Subnet | Gateway | DHCP Scope | DNS |
|------|--------|---------|-----------|-----|
| 99 | 192.168.99.0/24 | 192.168.99.1 | 99.50–99.200 | OPNsense |
| 60 | 192.168.60.0/24 | 192.168.60.1 | 60.50–60.200 | OPNsense |
| 20 | 192.168.20.0/24 | 192.168.20.1 | 20.50–20.200 | OPNsense |
| 30 | 192.168.30.0/24 | 192.168.30.1 | 30.50–30.200 | OPNsense |
| 40 | 192.168.40.0/24 | 192.168.40.1 | 40.50–40.200 | OPNsense |
| 50 | 192.168.50.0/24 | 192.168.50.1 | 50.50–50.200 | OPNsense |

### Key Points

- **Personal Laptop WiFi:** Gets 192.168.30.x via Home VLAN DHCP
- **Personal Laptop Ethernet (GE2):** Can request DHCP on any VLAN it's tagged to; typically admin uses static or specific VLAN
- **VMs on Laptop:** Get DHCP from OPNsense based on their VLAN tag (60 for servers, 20 for test, 50 for sandbox)
- **Each VLAN has independent gateway** = routing separation enforced by firewall

---

## Security Model (Current State)

### Segmentation

**Physical Layer:**
- VLAN 99 (Management): GE3–GE5 + Trunks
- VLAN 60 (Servers): GE6–GE10 + Trunks
- VLAN 20 (Test): GE11–GE20 + Trunks
- VLAN 30 (Home): GE21–GE30 + Trunks
- VLAN 40 (IoT): GE31–GE40 + Trunks
- VLAN 50 (Sandbox): GE41–GE45 + Trunks
- **GE46–GE52:** Unused/disabled

**Network Layer (Firewall):**
- Inter-VLAN traffic routed/blocked based on policy
- Test (20) can access Servers (60): ✓ Allowed (testing)
- Test (20) cannot access Sandbox (50): ✓ Blocked (isolation)
- IoT (40) isolated from all: ✓ Blocked (low trust)
- Sandbox (50) completely isolated: ✓ Blocked (malware containment)

### Laptop as Admin Bridge

**Via WiFi (Home VLAN):**
- Standard user access to internet
- No direct lab access
- Separate administrative domain

**Via Ethernet Trunk (GE2):**
- Can connect to any VLAN with proper tagging
- Administrative access to infrastructure
- Manages VMs, accesses services
- Subject to firewall policies like any other device

---

## Known Working State

- ✅ Dual trunk configuration stable (GE1 to firewall, GE2 to laptop)
- ✅ VLAN segmentation functional
- ✅ VMs running on laptop hardware with VLAN tagging
- ✅ Inter-VLAN routing operational via firewall
- ✅ DHCP working per VLAN
- ✅ Internet access functional (via firewall)
- ✅ Servers VLAN (60) hosting AD + Splunk accessible
- ✅ Test VLAN (20) VMs can reach servers (20→60 allowed)
- ✅ Sandbox VLAN (50) isolated from other VLANs

---

## Known Lessons Learned

1. **Default gateway must match VLAN subnet** — DHCP and static IPs need correct gateway
2. **VLAN tagging is how VMs access segmented network** — Hypervisor NIC must be trunk-capable
3. **Laptop Ethernet is trunk, not access port** — Allows multiple VLAN access simultaneously
4. **Proper segmentation requires both switch + firewall** — Switch does Layer 2, firewall does Layer 3 policy
5. **WiFi and Ethernet are separate paths** — Laptop on WiFi = Home VLAN only; on Ethernet = all VLANs possible
6. **Firewall policies enforce inter-VLAN rules** — Switch can't prevent VLAN 20→60 access; firewall does
7. **Sandbox isolation requires firewall rules, not just VLAN separation** — Network-level containment

---

## Current Infrastructure Summary

### Physical Devices
- **Cisco SG300-52 Switch:** Core segmentation
- **OPNsense Firewall:** Routing, policy, DHCP
- **QNAP NAS:** Storage (VLAN 60)
- **Xfinity Modem/Gateway:** ISP connection
- **Personal Laptop:** Hypervisor host, admin workstation

### Virtual Infrastructure (Running on Laptop)
- **Domain Controller VM:** VLAN 60 (Servers)
- **Splunk Server VM:** VLAN 60 (Servers)
- **Test Lab VMs:** VLAN 20 (Test) — Windows 11 testing
- **Sandbox VMs:** VLAN 50 (Sandbox) — Malware analysis (when needed)

### Access Patterns
- **WiFi:** Personal use (Home VLAN) → Internet only
- **Ethernet Trunk:** Admin/infrastructure management → Lab access
- **VMs:** Firewalled access between segments (20↔60 allowed; 20/60 ↔ 50 blocked)

---

## Future Improvements (Planned)

### Switch Hardening
- [ ] Disable/isolate unused ports (GE46–GE52)
- [ ] Enable port security (MAC limiting on access ports)
- [ ] Remove VLAN 1 from active use
- [ ] Physical cable labeling

### Monitoring
- [ ] Forward switch logs to Splunk
- [ ] Centralized VLAN/port event tracking
- [ ] Alerts on port security violations

### Network Enhancements
- [ ] Second backup trunk (redundancy)
- [ ] DHCP Snooping (prevent rogue DHCP)
- [ ] Storm control (broadcast protection)
- [ ] Port ACLs (additional Layer 2 filtering)

---

## Summary

This configuration establishes a:
- **Segmented network architecture** with dual trunk design
- **Clear VLAN-to-device mapping** for VMs and physical systems
- **Centralized routing & policy** through firewall
- **Laptop as admin bridge** (WiFi for personal, Ethernet for lab)
- **Scalable foundation** for future infrastructure growth

The switch is currently **stable and fully operational**. The architecture supports:
- Development/testing (VLAN 20 + VMs)
- Core services (VLAN 60 + AD/Splunk/NAS)
- Isolated experimentation (VLAN 50 + sandbox)
- Administrative control (VLAN 99 + management)
- Personal use (VLAN 30 WiFi + internet)

All while maintaining network segmentation and firewall-enforced policy controls.

---

## Document Control

| Item | Details |
|------|---------|
| **Created** | 2026-04-11 |
| **Last Updated** | 2026-06-28 |
| **Author** | Ben |
| **Configuration Status** | Current / Actual |
| **Review Date** | 2026-09-28 (quarterly) |
| **Status** | Operational & Stable |

---

## References

- Cisco SG300 Administration Guide
- OPNsense Firewall Configuration
- Home Lab Network Topology
- Change Management Logs
- VLAN Configuration Deep Dive
- Port Configuration Mapping
