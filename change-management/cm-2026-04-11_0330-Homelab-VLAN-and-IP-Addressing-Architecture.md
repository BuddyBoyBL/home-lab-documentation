# Change Management Log

## Change ID
CM-2026-04-11-0330

## Date & Time
2026-04-11 | 03:30 (Local Time)

## Author
Ben

## Change Type
- [x] Standard
- [ ] Normal
- [ ] Emergency

## Affected Systems
- Homelab VLAN Architecture
- IP Addressing Scheme
- Firewall Routing Policies (OPNsense)
- Switch VLAN Mapping

## Description of Change
Defined and documented the current homelab IP addressing scheme and VLAN segmentation architecture.

This configuration establishes the foundational network design for all routing, firewall policy enforcement, and device segmentation within the environment.

All VLANs are routed centrally through the firewall, with strict segmentation between trust zones.

## Reason for Change
- Establish consistent and scalable IP addressing structure
- Define security zones through VLAN segmentation
- Provide a clear reference model for firewall and switch configuration
- Support long-term expansion of lab infrastructure and services

## Pre-Change State
- VLANs existed but were not formally standardized in documentation
- IP assignments were not centrally defined
- Network segmentation model was implied rather than explicitly documented

## Implementation Summary

### VLAN Structure Defined
- MGMT → 192.168.99.0/24
- SERVERS → 192.168.60.0/24
- TEST → 192.168.20.0/24
- HOME → 192.168.30.0/24
- IOT → 192.168.40.0/24
- SANDBOX → 192.168.50.0/24
- WAN → 10.0.0.0/24

### Server Assignments Standardized
- Splunk → 192.168.60.10
- Active Directory → 192.168.60.20
- NAS → 192.168.60.30

### Design Principles Established
- VLAN-based security segmentation
- Least privilege access model
- Controlled inter-VLAN communication via firewall
- Centralized routing at firewall layer

## Post-Change State
- Fully defined VLAN-to-subnet mapping exists
- Server IP addressing is standardized and documented
- Network zones are clearly separated by trust level
- Traffic flow model is explicitly defined for operational use

## Validation / Testing
- Confirmed VLAN structure aligns with switch configuration
- Verified firewall routing model supports defined subnets
- Ensured server IP assignments do not conflict
- Confirmed segmentation model is consistent with current firewall rules

## Impact Assessment
- Expected Impact: Stable and scalable network architecture foundation
- Actual Impact: Improved clarity and alignment across switch, firewall, and documentation layers
- Downtime: None (design-level implementation)

## Rollback Plan
- Revert VLAN definitions in documentation
- Restore previous ad-hoc IP assignments if required
- Remove standardized addressing scheme references

## Security Model Summary

### Segmentation Model
- MGMT → Full administrative access (restricted by policy)
- SERVERS → Core infrastructure zone
- TEST → Controlled experimentation environment
- HOME → General user network
- IOT → Restricted/untrusted devices
- SANDBOX → Fully isolated high-risk environment

### Traffic Flow Model
```text
MGMT → All VLANs (administration access)

TEST → AD, Logging (Splunk), Internet

HOME → Internet only (future IoT interaction optional)

IOT → Internet only (restricted internal access)

SERVERS → Limited outbound access (updates/services)

SANDBOX → Fully isolated environment (no internal access)
