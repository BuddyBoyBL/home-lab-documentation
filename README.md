# Home Lab Documentation

## Overview
This repository documents my personal home lab environment designed to simulate an enterprise network with segmentation, monitoring, and security controls.

## Objectives
- Build a segmented network using VLANs
- Implement firewall rules and access control
- Deploy SIEM for log monitoring
- Simulate real-world cybersecurity scenarios

## Environment
- Comcast Modem (Bridge Mode Planned)
- Layer 3 Switch
- Windows 10 Pro Host (Hyper-V)
- Kali Linux VM
- Windows Server (Active Directory)
- Splunk SIEM

## Network Architecture
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

## VLAN Structure
- VLAN 10 – Management
- VLAN 20 – Home/Users
- VLAN 30 – Lab/Servers
- VLAN 666 – Malware Sandbox (Isolated)

## Security Controls
- Network segmentation
- Firewall rules
- Least privilege model
- Log monitoring with Splunk

## Key Features
- Malware sandbox environment
- Phishing simulation lab (GoPhish)
- Log analysis and alerting
- Active Directory domain setup

## Documentation Sections
- Network Topology
- VLAN Configuration
- Firewall Rules
- SIEM Setup
- Procedures & Troubleshooting

## Status
In progress – actively building and documenting
