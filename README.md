Enterprise-Grade Security Operations & Network Simulation Lab
Project Overview
This repository documents the architecture, implementation, and management of a multi-zone enterprise network simulation. The lab is designed to facilitate high-fidelity security research, malware analysis, and security operations center (SOC) workflow optimization.

By utilizing a dedicated hardware stack including pfSense, Cisco Layer 3 switching, and Windows Server 2022, this environment enforces strict network segmentation (VLANs) and centralized telemetry ingestion (Splunk).

Strategic Objectives
Hardened Perimeter Defense: Deployment of pfSense with Suricata IDS/IPS for deep packet inspection and edge security.

Network Segmentation & Zero Trust: Implementation of a 6-zone VLAN architecture to isolate malware and untrusted IoT devices from the production core.

Identity & Access Management (IAM): Centralized management of users, groups, and GPOs via Windows Server 2022 Active Directory.

Enterprise Observability: Full-stack log aggregation from network nodes and endpoints into a Splunk SIEM for real-time detection engineering.

Hardware Specifications
Perimeter Gateway: Qotom Q305p Mini PC (Intel Core i5) running pfSense.

Core Switching: Cisco Catalyst 3560 Series L3 Switch (Inter-VLAN routing & ACLs).

Identity/Management Server: Lenovo T480 (Windows Server 2022 / Hyper-V).

SOC Endpoint: Windows 10 Pro (Hardened with Sysmon and Wireshark).

Physical Media: 100ft Cat 6 UTP (ANSI/TIA-568.2-D compliant) for direct L2 visibility.

Logical Network Architecture
Plaintext
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
Security Zone Definitions
VLAN 10 (Management): Critical infrastructure access; restricted to Admin physical ports.

VLAN 20 (Home): Trusted user environment for daily operations.

VLAN 25 (IoT): Untrusted smart devices; isolated from all internal zones.

VLAN 30 (Lab/SOC): Primary testing environment; direct switch connectivity for SPAN/Mirror port packet ingestion.

VLAN 40 (Guest): Time-limited, temporary wireless access.

VLAN 666 (Malware): Maximum isolation; no transit paths to internal zones or WAN.

Governance & Documentation
This project follows a professional Change Management framework. All infrastructure modifications are tracked via audit logs and version-controlled pull requests.

Change Management: Formal audit trail of network and server modifications.

Troubleshooting: Root-cause analysis and resolution logs for hardware/software incidents.

Procedures: Standard Operating Procedures (SOPs) for environment scaling and incident response.

Implementation Status
Phase 1: Physical & Logical Core: ✅ VLAN Schema defined; pfSense/Cisco integration verified.

Phase 2: Identity & Endpoints: 🔄 Active Directory promotion and Test PC domain-joining (In Progress).

Phase 3: Visibility & Monitoring: ⏳ Splunk Universal Forwarder deployment and Suricata rule tuning.

Status: Active Build Phase

Last Major Update: April 2026 - Topology re-routed for direct L2 switch integration of the SOC node.
