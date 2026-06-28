# Network Design Overview

## Summary
This document outlines the overall network design for the home lab, including segmentation strategy, VLAN structure, and traffic flow. The goal is to create a small enterprise-style environment that supports security, testing, and operational learning.

## Design Goals
- separate devices based on trust level and function
- protect household devices from lab and test environments
- support monitoring, logging, and future detection work
- create a realistic enterprise-style network structure
- allow safe testing, including isolated environments

## Network Segmentation Strategy
The network is segmented using VLANs to enforce separation between different types of systems. Each segment is designed with a specific purpose and level of trust.

## VLAN Structure

| VLAN ID | Name       | Purpose                                  | Example Devices |
|--------|------------|------------------------------------------|-----------------|
| 10     | Management | Administrative access to infrastructure  | Admin laptop, switch UI, firewall UI |
| 20     | Home       | Trusted household network                | Personal devices, family systems |
| 25     | IoT        | Low-trust smart devices                  | Smart TVs, Alexa, smart lights, printers |
| 30     | Lab        | Testing and server environment           | Test PC, VMs, Splunk, Active Directory |
| 40     | Guest      | Internet-only temporary access           | Guest phones, laptops |
| 666    | Malware    | Isolated high-risk testing environment   | Sandbox VMs, malware analysis systems |

## VLAN Purpose and Design

### VLAN 10 – Management
Reserved for administrative access to network infrastructure.  
This VLAN is tightly controlled and only accessible from approved systems.

### VLAN 20 – Home
Used for trusted day-to-day devices.  
This network is protected from lab activity, IoT devices, and testing environments.

### VLAN 25 – IoT
Contains devices that are functional but less secure.  
Access is restricted to limit exposure to internal systems.

### VLAN 30 – Lab
Primary working environment for the home lab.  
Includes servers, monitoring tools, virtual machines, and test systems.

### VLAN 40 – Guest
Designed for temporary users.  
Restricted to internet-only access with no internal network visibility.

### VLAN 666 – Malware
Dedicated isolated network for high-risk testing.  
No direct access to trusted networks is intended.

## Traffic Model (High-Level)

- management systems can access infrastructure interfaces
- home devices access the internet and approved internal services
- IoT devices have restricted internal communication
- lab systems are isolated from trusted home devices
- guest devices have internet-only access
- malware environment remains isolated from all trusted networks

Detailed firewall rules and ACLs are documented separately.

## Current Implementation Status
- VLANs created on core switch
- access ports assigned
- trunk configured between firewall and switch
- inter-VLAN routing reviewed
- firewall policies aligned with VLAN structure
- device placement validated

## Why This Design Matters
This structure reflects real-world network segmentation practices and supports:
- reduced attack surface
- safer experimentation
- controlled communication between systems
- improved monitoring and visibility

It also allows the lab to grow without needing to redesign the entire network.

## Related Documentation
- topology diagrams
- firewall configuration
- asset inventory
- progress logs
- troubleshooting notes

## Notes
This design will evolve as the lab expands. Changes to segmentation, device placement, or routing behavior should be documented in progress and change management entries.
