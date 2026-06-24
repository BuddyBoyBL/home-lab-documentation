# VLAN Trunking & Docking Station Troubleshooting

## Overview

This document outlines the troubleshooting process for resolving VLAN trunking and VM segmentation issues within the homelab environment.

The goal was to allow multiple VLANs to traverse a single Ethernet connection from the laptop to the switch while assigning VMs to separate VLANs for segmentation and testing.

The issue was ultimately determined to be a combination of:

- Incorrect switch/VLAN configuration
- Docking station hardware limitations related to VLAN-tagged traffic

---

## Objective

- Configure a trunk port between laptop and switch
- Allow VMs to communicate on separate VLANs
- Keep host OS separated from lab traffic
- Simulate enterprise-style VLAN segmentation using a single physical connection

---

# Initial Design

## Intended Architecture

- Laptop connected to switch via single Ethernet connection
- Switch port configured as trunk
- VMs assigned to separate VLANs
- Host OS primarily connected through Wi-Fi/home network
- VLAN segmentation handled through pfSense and managed switch

---

# Initial Problems Encountered

## Issue 1 — No Internet / Network Connectivity

### Symptoms
- Laptop lost internet connectivity during VLAN testing
- VM traffic was inconsistent
- Trunk configuration did not behave as expected

---

## Troubleshooting Steps Performed

### Verified VLAN Access Ports
- Tested direct VLAN access ports
- Confirmed VLAN-specific ports worked correctly

### Firewall Validation
- Reviewed pfSense rules
- Confirmed firewall rules were functioning properly

### Backup Device Testing
- Connected backup laptop to test connectivity
- Determined issue was isolated to trunk path

---

## Root Cause Identified

### Incorrect PVID Configuration

The switch port PVID was configured to a reserved/incorrect VLAN, causing traffic handling issues.

---

## Resolution

### Temporary Fix
- Set PVID to VLAN 60 temporarily
- Restored connectivity

### Notes
- This is not the final intended security configuration
- Future plan is to use a dedicated management/native VLAN

---

# Issue 2 — VLAN Traffic Not Reaching VMs

## Symptoms
- Trunk port passed incomplete traffic
- VMs were unable to communicate correctly on assigned VLANs
- Segmentation behaved inconsistently

---

## Troubleshooting Steps Performed

### Docking Station Testing
Attempted:
- Power cycling docking station
- Power cycling laptop
- Driver updates
- Windows network/security setting adjustments
- Multiple reconnection attempts

### Community Research
Reviewed:
- Reddit discussions
- Docking station VLAN tagging issues
- Windows VLAN tagging limitations
- Similar enterprise NIC behavior reports

---

## Findings

### Key Discovery
The docking station Ethernet interface was unable to reliably pass VLAN-tagged (802.1Q) traffic.

This created:
- inconsistent VLAN communication
- failed VM segmentation
- unreliable trunk behavior

---

# Final Resolution

## Hardware Change
Replaced docking station Ethernet connection with:

- USB-C to Ethernet adapter

---

## VLAN Configuration Corrections

Updated switch configuration:

- Trunk port VLANs set to tagged
- VM interfaces assigned to correct VLANs
- VLAN segmentation verified operational

---

# Final Working Configuration

## Host OS
- Temporarily connected through home VLAN
- Future plan:
  - move Host OS fully to Wi-Fi
  - isolate management/lab traffic further

---

## VMs
Successfully segmented onto proper VLANs:

- Server VLAN
- Lab VLAN
- Management VLAN
- Additional planned VLANs

---

# Outcome

## Successful Results
- VLAN trunk operational
- VM segmentation functioning
- Tagged VLAN traffic working correctly
- pfSense routing functioning properly
- Switch trunk configuration validated

---

# Key Lessons Learned

- Docking station NICs may not properly support 802.1Q VLAN tagging
- Link connectivity does not guarantee tagged traffic support
- PVID misconfiguration can silently break VLAN communication
- Isolating variables (firewall vs switch vs hardware) is critical
- USB-C Ethernet adapters may provide better VLAN compatibility than some docks

---

# Future Improvements

## Planned Security Hardening
- Move Host OS off trunked Ethernet
- Use Wi-Fi for personal/home connectivity
- Reserve Ethernet only for:
  - management
  - server access
  - lab infrastructure

---

## Additional Future Goals
- Dedicated management VLAN
- Improved firewall segmentation
- Additional VLAN isolation
- Formalized network hardening policies

---

# Status

**Resolved — VLAN segmentation and trunking operational**
