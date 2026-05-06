# Home Lab Network Topology

## Overview

This home lab is designed to simulate a small enterprise environment with segmented networks, centralized logging, identity management, and a future isolated malware analysis zone.

The current build is in a **segmentation and connectivity validation phase**, with a working firewall, defined VLAN architecture, and ongoing switch configuration. The legacy household network remains active while the lab environment is being brought online.

The goal is to gain hands-on experience in:
- IT support and troubleshooting
- system administration (Active Directory)
- SOC operations (Splunk, logging, monitoring)
- network segmentation and firewalling
- defensive security engineering

---

## Physical Topology (Current State)
                                       INTERNET
                                           │
                             ┌──────────────────────────┐
                             │ Xfinity Gateway / Modem │
                             │ (Legacy Network Active) │
                             └────────────┬─────────────┘
                                          │
                    Ethernet Run (Upstairs → Basement)
                                          │
                               WAN        │
                          ┌───────────────▼───────────────┐
                          │       pfSense Firewall        │
                          │      (Qotom Mini PC)          │
                          │   VLAN Gateways + Routing     │
                          └───────────────┬───────────────┘
                                          │
                                     LAN / TRUNK
                                          │
                          ┌───────────────▼───────────────┐
                          │       Cisco L2 Switch         │
                          │     VLAN Distribution Layer   │
                          └──────┬────────┬────────┬──────┬────────┬────────┘
                                 │        │        │      │        │
                              VLAN 99  VLAN 60  VLAN 30 VLAN 20 VLAN 40 VLAN 50
                             Management Servers   Home    Test     IoT   Sandbox
                               Admin     Core     Users    Lab    Untrusted Isolated
                                         Services


---

## VLAN Architecture

| VLAN | Name        | Subnet            | Purpose |
|------|------------|------------------|--------|
| 99   | Management | 192.168.99.0/24  | Infrastructure control plane |
| 60   | Servers    | 192.168.60.0/24  | Core services (AD, Splunk, NAS) |
| 30   | Home       | 192.168.30.0/24  | Controlled home network |
| 20   | Test       | 192.168.20.0/24  | Lab clients and endpoints |
| 40   | IoT        | 192.168.40.0/24  | Low-trust devices |
| 50   | Sandbox    | 192.168.50.0/24  | Malware / high-risk testing |

---

## Network Roles

### Management (VLAN 99)
- Admin laptop
- Switch management interface
- Firewall management interface

Restricted to administrative access only.

---

### Servers (VLAN 60)
- Domain Controller (Active Directory / DNS)
- Splunk Server (SIEM)
- QNAP NAS (storage / backup)

Provides centralized services for the lab environment.

---

### Home (VLAN 30)
- TP-Link access point (temporary)
- Smart TV (temporary placement)
- Future migration of family devices

This network is a controlled transition away from the legacy flat network.

---

### Test / Lab Clients (VLAN 20)
- Test PC (Dell Inspiron)
- Windows client VM
- Kali Linux VM
- Test printer
- Test phone (planned)

Used for:
- endpoint testing
- domain integration
- log generation and analysis

---

### IoT (VLAN 40)
- Smart bulbs
- Air purifier
- Ring doorbell
- Wi-Fi extender
- IoT router/AP

Internet-only network with no internal trust.

---

### Sandbox (VLAN 50)
- Reserved for malware analysis

Design intent:
- no internet initially
- no access to internal networks
- future INetSim integration

---

## Virtualization Layer

Host: Admin Laptop (Hypervisor)

Virtual Machines:
- Domain Controller → VLAN 60
- Splunk Server → VLAN 60
- Windows Client → VLAN 20
- Kali Linux → VLAN 20
- Sandbox VM (future) → VLAN 50

---

## Current State

- VLANs configured on pfSense
- IP addressing plan implemented
- QNAP NAS integrated into server VLAN
- Basic firewall rules applied
- Cisco switch configuration in progress
- Connectivity testing ongoing
- Legacy network (192.168.1.0/24) still active upstairs

---

## Design Principles

- Segmentation based on trust level
- Default deny between VLANs
- Centralized control via firewall
- Separation of servers and clients
- IoT treated as untrusted
- Sandbox treated as hostile

---

## Summary

This topology represents a **build-in-progress enterprise-style network** with:

- Dedicated firewall (pfSense)
- VLAN-based segmentation
- Centralized services (AD, Splunk, NAS)
- Isolated lab and testing environment
- Planned malware analysis zone

The environment will continue evolving as switch configuration, firewall rules, and monitoring systems are finalized.
