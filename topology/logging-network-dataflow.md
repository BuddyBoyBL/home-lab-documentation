# Logging Network & Data Flow Architecture

## Overview
This diagram represents the network segmentation and log flow design of the homelab environment. It separates responsibilities across VLANs and defines how log data moves from endpoints to storage.

---

## Network Segmentation

- **Admin VLAN**
  - Management access (QNAP UI, configs, backups)

- **Server VLAN**
  - Splunk server (log processing)
  - QNAP NAS (log storage)
  - Domain Controller (AD logs)

- **Client VLAN(s)**
  - Endpoints generating logs

---

## Physical & Logical Topology

```text
                    ┌────────────────────────────┐
                    │        pfSense Firewall     │
                    │     (Trunk Port - VLANs)    │
                    └────────────┬───────────────┘
                                 │
                     ┌───────────┴───────────┐
                     │        Switch          │
                     │   (VLAN Segmentation) │
                     └───────┬───────┬───────┘
                             │       │
         ┌───────────────────┘       └────────────────────┐
         │                                                │

 ┌───────────────┐                          ┌────────────────────┐
 │  Client VLAN  │                          │   Admin VLAN       │
 │ (Endpoints)   │                          │ (Admin Laptop)     │
 └──────┬────────┘                          └─────────┬──────────┘
        │                                             │
        │ Logs (Forwarders)                           │ Management Access
        ▼                                             ▼

                ┌────────────────────────────┐
                │     Server VLAN            │
                │                            │
                │  ┌──────────────────────┐  │
                │  │    Splunk Server     │  │
                │  │  (Test PC - Hot)     │  │
                │  └─────────┬────────────┘  │
                │            │               │
                │   Log Forwarding           │
                │            ▼               │
                │  ┌──────────────────────┐  │
                │  │      QNAP NAS        │  │
                │  │ (Warm/Cold Storage)  │  │
                │  └──────────────────────┘  │
                │                            │
                │  ┌──────────────────────┐  │
                │  │ Domain Controller    │  │
                │  │   (AD Logs)          │  │
                │  └──────────────────────┘  │
                │                            │
                └────────────────────────────┘
