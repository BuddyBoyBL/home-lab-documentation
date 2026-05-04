# Cisco SG300 Switch Configuration (Current State)

## Overview
This document defines the current configuration of the Cisco SG300 switch within the homelab environment.

The switch operates as a **Layer 2 access switch** with a single trunk uplink to the firewall.  
All inter-VLAN routing is handled by OPNsense.

This document serves as the **source of truth** for:
- VLAN to port assignments
- Trunk configuration
- Current segmentation layout

---

## Core Design

### Architecture Model
- Switch = Layer 2 (VLAN segmentation only)
- Firewall (OPNsense) = Layer 3 (routing, NAT, DHCP, DNS)
- Single trunk link carries all VLAN traffic

---

## Trunk Configuration

| Port | Mode  | Description |
|------|------|------------|
| GE1  | Trunk | Uplink to firewall (all VLANs tagged) |

### Allowed VLANs on Trunk
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
|------|------|------|
| GE2–GE5 | Access (Untagged) | Admin devices |
| GE1 | Tagged | Trunk |

---

### VLAN 60 — Servers (192.168.60.0/24)
| Ports | Mode | Notes |
|------|------|------|
| GE6–GE10 | Access (Untagged) | Servers (AD, Splunk, NAS planned) |
| GE1 | Tagged | Trunk |

---

### VLAN 20 — Test (192.168.20.0/24)
| Ports | Mode | Notes |
|------|------|------|
| GE11–GE20 | Access (Untagged) | Lab / testing systems |
| GE1 | Tagged | Trunk |

---

### VLAN 30 — Home (192.168.30.0/24)
| Ports | Mode | Notes |
|------|------|------|
| GE21–GE30 | Access (Untagged) | Personal/home devices |
| GE1 | Tagged | Trunk |

---

### VLAN 40 — IoT (192.168.40.0/24)
| Ports | Mode | Notes |
|------|------|------|
| GE31–GE40 | Access (Untagged) | IoT devices (low trust) |
| GE1 | Tagged | Trunk |

---

### VLAN 50 — Sandbox (192.168.50.0/24)
| Ports | Mode | Notes |
|------|------|------|
| GE41–GE45 | Access (Untagged) | Malware / isolated testing |
| GE1 | Tagged | Trunk |

---

## Port Configuration Standards

### Access Ports
- Mode: Access
- Untagged VLAN: Assigned VLAN
- PVID: Matches VLAN ID
- All other VLANs: Forbidden

---

### Trunk Port (GE1)
- Mode: Trunk
- All VLANs: Tagged
- Native VLAN: Not used for access traffic

---

## DHCP & Gateway Behavior

- DHCP handled by OPNsense (Kea DHCP)
- Each VLAN has its own DHCP scope
- Gateway per VLAN = `.1` on firewall

### Example
| VLAN | Gateway |
|------|--------|
| 99 | 192.168.99.1 |
| 60 | 192.168.60.1 |
| 30 | 192.168.30.1 |
| 40 | 192.168.40.1 |
| 50 | 192.168.50.1 |

---

## Security Model (Current State)

### Segmentation
- VLANs are isolated at Layer 2
- Inter-VLAN traffic controlled by firewall

### Sandbox VLAN (50)
- Limited port exposure (GE41–45 only)
- Designed for high-risk testing
- May have restricted or no internet access

---

## Known Working State

- VLAN segmentation functional
- Trunk link stable
- Inter-VLAN routing operational (via firewall)
- DHCP working per VLAN
- Internet access functional (when gateway configured correctly)

---

## Known Lessons Learned

- Default gateway must match VLAN subnet
- DNS does not replace routing
- Misconfigured gateway results in complete loss of internet
- Proper segmentation removes “accidental connectivity”

---

## Future Improvements (Planned)

### Switch Hardening
- Disable unused ports
- Move unused ports to unused VLAN
- Enable port security (MAC limits)
- Remove VLAN 1 from all access ports

---

### Monitoring
- Enable logging
- Forward logs to Splunk

---

### Network Enhancements
- Reduce exposed port ranges per VLAN
- Label all ports physically and logically
- Implement stricter VLAN-to-VLAN firewall rules

---

## Summary

This configuration establishes a:

- Segmented network architecture
- Clear VLAN-to-port mapping
- Centralized routing through firewall
- Scalable foundation for future services and security controls

The switch is currently in a **stable, functional state** and will remain unchanged while higher-level services are implemented.
