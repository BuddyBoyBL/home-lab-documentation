# Cisco SG300 Switch Configuration (Current State - Hybrid Host Trunk Update)

## Overview

This document defines the current configuration of the Cisco SG300 switch within the homelab environment.

The switch operates as a **Layer 2 access switch** with trunk uplinks to both the firewall and a virtualization host.

All inter-VLAN routing is handled by OPNsense.

This document serves as the **source of truth** for:

* VLAN to port assignments
* Trunk configuration
* Current segmentation layout

---

## Core Design

### Architecture Model

* Switch = Layer 2 (VLAN segmentation only)
* Firewall (OPNsense) = Layer 3 (routing, NAT, DHCP, DNS)
* GE1 = Primary trunk to firewall
* GE2 = Hybrid trunk for virtualization host (host + VMs)

---

## Trunk Configuration

| Port | Mode  | Description                           |
| ---- | ----- | ------------------------------------- |
| GE1  | Trunk | Uplink to firewall (all VLANs tagged) |
| GE2  | Trunk | Hybrid port for hypervisor host       |

---

### GE1 (Firewall Trunk)

Tagged VLANs:

* VLAN 20 (Test)
* VLAN 30 (Home)
* VLAN 40 (IoT)
* VLAN 50 (Sandbox)
* VLAN 60 (Servers)
* VLAN 99 (Management)

---

### GE2 (Hybrid Host Trunk)

| VLAN    | Mode              | Purpose                            |
| ------- | ----------------- | ---------------------------------- |
| VLAN 30 | Untagged (PVID)   | Host OS (laptop personal use)      |
| VLAN 60 | Tagged            | Virtual Machines (AD, Splunk, NAS) |
| VLAN 99 | Tagged (optional) | Admin VMs (if used)                |

---

## VLAN to Port Mapping

### VLAN 99 — Management (192.168.99.0/24)

| Ports   | Mode              | Notes         |
| ------- | ----------------- | ------------- |
| GE3–GE5 | Access (Untagged) | Admin devices |
| GE1     | Tagged            | Trunk         |
| GE2     | Tagged (optional) | Admin VMs     |

---

### VLAN 60 — Servers (192.168.60.0/24)

| Ports    | Mode              | Notes               |
| -------- | ----------------- | ------------------- |
| GE6–GE10 | Access (Untagged) | Physical servers    |
| GE2      | Tagged            | Virtualized servers |
| GE1      | Tagged            | Trunk               |

---

### VLAN 20 — Test (192.168.20.0/24)

| Ports     | Mode              | Notes                 |
| --------- | ----------------- | --------------------- |
| GE11–GE20 | Access (Untagged) | Lab / testing systems |
| GE1       | Tagged            | Trunk                 |

---

### VLAN 30 — Home (192.168.30.0/24)

| Ports     | Mode              | Notes            |
| --------- | ----------------- | ---------------- |
| GE21–GE30 | Access (Untagged) | Home devices     |
| GE2       | Untagged (PVID)   | Host OS (laptop) |
| GE1       | Tagged            | Trunk            |

---

### VLAN 40 — IoT (192.168.40.0/24)

| Ports     | Mode              | Notes       |
| --------- | ----------------- | ----------- |
| GE31–GE40 | Access (Untagged) | IoT devices |
| GE1       | Tagged            | Trunk       |

---

### VLAN 50 — Sandbox (192.168.50.0/24)

| Ports     | Mode              | Notes           |
| --------- | ----------------- | --------------- |
| GE41–GE45 | Access (Untagged) | Malware testing |
| GE1       | Tagged            | Trunk           |

---

## Port Configuration Standards

### Access Ports

* Mode: Access
* Untagged VLAN: Assigned VLAN
* PVID: Matches VLAN ID
* All other VLANs: Forbidden

---

### Trunk Ports

#### GE1 (Firewall)

* Mode: Trunk
* All VLANs: Tagged

#### GE2 (Hybrid Host)

* Mode: Trunk
* Native VLAN (PVID): VLAN 30
* Tagged VLANs: 60 (Servers), optional 99 (Management)

---

## DHCP & Gateway Behavior

* DHCP handled by OPNsense (Kea DHCP)
* Each VLAN has its own DHCP scope
* Gateway per VLAN = `.1` on firewall

### Example

| VLAN | Gateway      |
| ---- | ------------ |
| 99   | 192.168.99.1 |
| 60   | 192.168.60.1 |
| 30   | 192.168.30.1 |
| 40   | 192.168.40.1 |
| 50   | 192.168.50.1 |

---

## Security Model (Current State)

### Segmentation

* VLANs are isolated at Layer 2
* Inter-VLAN routing controlled by firewall

---

### Hybrid Host Consideration (GE2)

This port carries both:

* Personal traffic (untagged VLAN 30)
* Infrastructure traffic (tagged VLAN 60)

Implications:

```text
Single physical device participates in multiple trust zones
```

Mitigation strategy:

* Keep management access restricted to VLAN 99
* Control VM access via firewall rules
* Avoid exposing host OS to management VLAN

---

### Sandbox VLAN (50)

* Limited ports (GE41–45 only)
* Intended for high-risk activity
* May be isolated from internal network

---

## Known Working State

* VLAN segmentation functional
* Trunk links stable (GE1, GE2)
* Hypervisor VLAN tagging functional
* Inter-VLAN routing operational
* DHCP functional per VLAN
* Internet access functional

---

## Known Lessons Learned

* Default gateway must match VLAN subnet
* DNS does not replace routing
* VLAN tagging must match hypervisor configuration
* Trunk ports should only be used when required
* Misconfigured PVID leads to silent failures

---

## Future Improvements (Planned)

### Switch Hardening

* Disable unused ports
* Remove VLAN 1 from all access ports
* Apply "Forbidden" VLAN rules where appropriate
* Implement port security (MAC limits)

---

### Monitoring

* Enable switch logging
* Forward logs to Splunk

---

### Network Enhancements

* Reduce exposed port ranges per VLAN
* Label ports physically and logically
* Implement stricter firewall segmentation rules
* Separate management VM access if needed

---

## Summary

This configuration establishes:

* A segmented multi-VLAN architecture
* Hybrid host networking via trunked hypervisor port
* Centralized routing and policy enforcement via firewall
* A scalable and realistic enterprise-style lab environment

The switch remains in a **stable operational state**, with controlled expansion into virtualization-aware networking.

```
---

# 🧠 What changed (for your awareness)

- GE2 is now:
  - Trunk instead of access
  - VLAN 30 = untagged
  - VLAN 60 = tagged
- Management ports shifted from GE2–5 → GE3–5
- Added **hybrid host model explanation** (this is strong for GitHub)

---

# 🚀 Next recommendation (important)

Before merging this branch:

- Test:
  - Host OS internet (VLAN 30)
  - VM connectivity (VLAN 60)
  - VM → firewall → internet
- Confirm hypervisor tagging works cleanly

---

If you want next, I can help you:
- Configure your **hypervisor networking exactly**
- Or build a **diagram that matches this doc** (would look great in your repo)
```

