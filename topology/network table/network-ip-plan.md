# Network IP Plan (Segmented Homelab Architecture)

## Overview
This document defines the IP addressing scheme for the homelab environment. It establishes a segmented VLAN-based architecture to improve security, organization, and scalability.

This file is the **source of truth** for all IP assignments.

---

## VLAN Architecture

| VLAN ID | Network Name | Subnet | Gateway | DHCP Range | Purpose |
|--------|-------------|--------|---------|------------|--------|
| 99 | Management | 192.168.99.0/24 | 192.168.99.1 | 192.168.99.50–100 (optional) | Infrastructure control plane |
| 60 | Servers | 192.168.60.0/24 | 192.168.60.1 | None / Optional | Core services |
| 30 | Home | 192.168.30.0/24 | 192.168.30.1 | 192.168.30.100–200 | User devices |
| 20 | Test | 192.168.20.0/24 | 192.168.20.1 | 192.168.20.100–200 | Lab/testing systems |
| 40 | IoT | 192.168.40.0/24 | 192.168.40.1 | 192.168.40.100–200 | Untrusted devices |
| 50 | Sandbox | 192.168.50.0/24 | 192.168.50.1 | None | Malware / high-risk testing |

---

## Static IP Assignments

| Device | VLAN | IP Address | Role |
|-------|------|-----------|------|
| Firewall Interfaces | All | *.1 per VLAN* | Default gateway |
| Switch Management | 99 | 192.168.99.2 | Network switch management |
| Admin Workstation | 99 | 192.168.99.10 | Secure admin access |
| Domain Controller | 60 | 192.168.60.10 | Active Directory + DNS |
| Splunk Server | 60 | 192.168.60.20 | Log aggregation |
| QNAP NAS | 60 | 192.168.60.30 | Storage / Backup |

---

## DHCP Allocation Strategy

- `.1` → Gateway  
- `.2 – .20` → Infrastructure  
- `.20 – .50` → Static assignments  
- `.100 – .200` → DHCP pool  
- `.200+` → Reserved  

---

## Legacy Network (Decommission Plan)

| Network | Range | Notes |
|--------|-------|------|
| Flat LAN | 192.168.1.0/24 | Legacy network being phased out |

### Known Legacy Assignments
- 192.168.1.1 → Gateway  
- 192.168.1.50 → Previous Domain Controller  

---

## Network Design Principles

### Segmentation by Trust
- Management → Control plane  
- Servers → Trusted services  
- Clients → Standard use  
- IoT → Low trust  
- Sandbox → Untrusted / hostile  

---

### Isolation Strategy
- Servers are isolated from sandbox environments  
- Management network is restricted to infrastructure only  
- Inter-VLAN traffic is denied by default and explicitly allowed as needed  

---

## Special Notes

### Sandbox (VLAN 50)
- No DHCP  
- Manual IP assignment only  
- No access to Server or Management VLANs  

---

### Management VLAN (99)
- Infrastructure access only  
- Not used for servers or general clients  

---

## Summary

This design provides:
- Clear segmentation
- Reduced risk of IP conflicts
- Stronger security boundaries
- Scalable network structure

All future devices must follow this plan to maintain consistency and control.
