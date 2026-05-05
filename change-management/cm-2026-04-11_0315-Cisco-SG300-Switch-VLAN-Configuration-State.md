# Change Management Log

## Change ID
CM-2026-04-11-0315

## Date & Time
2026-04-11 | 03:15 (Local Time)

## Author
Ben

## Change Type
- [x] Standard
- [ ] Normal
- [ ] Emergency

## Affected Systems
- Cisco SG300 Layer 3 Switch (configured in Layer 2 mode)
- VLAN segmentation infrastructure
- Trunk uplink to OPNsense firewall
- End device port assignments

## Description of Change
Documented and finalized the current Cisco SG300 switch configuration state within the homelab environment.

The switch operates strictly as a Layer 2 access switch, with all inter-VLAN routing and DHCP services handled externally by the OPNsense firewall. A single trunk uplink carries all VLAN traffic between the switch and firewall.

This configuration establishes the baseline segmentation and port mapping structure for the lab.

## Reason for Change
- Establish a stable reference configuration for switch behavior
- Document VLAN-to-port mapping for future troubleshooting and scaling
- Align switch architecture with firewall-based Layer 3 routing model
- Create a clear operational baseline before implementing further security hardening

## Pre-Change State
- VLAN structure existed but was not formally documented as a stable configuration state
- Port assignments were subject to adjustment during lab development
- No finalized switch configuration baseline was recorded

## Implementation Summary
1. Defined switch role as Layer 2 access device only
2. Established trunk link (GE1) to firewall with all VLANs tagged
3. Assigned VLANs to dedicated port ranges:
   - VLAN 99: Management
   - VLAN 60: Servers
   - VLAN 20: Test
   - VLAN 30: Home
   - VLAN 40: IoT
   - VLAN 50: Sandbox
4. Standardized access port behavior (untagged VLAN, PVID matching)
5. Confirmed DHCP and routing handled exclusively by OPNsense

## Post-Change State
- Switch operates as a centralized Layer 2 segmentation device
- All VLAN traffic passes through single trunk uplink to firewall
- End devices are isolated by VLAN assignment at access port level
- Routing, NAT, DHCP, and DNS handled externally by firewall
- VLAN structure is stable and operational

## Validation / Testing
- Verified trunk link between switch and firewall is stable
- Confirmed VLAN tagging across trunk is functional
- Verified DHCP assignment per VLAN scope
- Confirmed inter-VLAN routing functions through firewall policy
- Verified internet access per VLAN gateway configuration

## Impact Assessment
- Expected Impact: Stable segmented network with centralized routing control
- Actual Impact: Successful implementation of VLAN-based segmentation with functional service delivery
- Downtime: None (configuration-level deployment)

## Rollback Plan
- Restore previous switch configuration backup
- Revert VLAN-to-port assignments to pre-defined state
- Remove trunk configuration and revert to flat network (if required for recovery)

## Current Security Posture

### Strengths
- VLAN segmentation enforced at switch level
- Centralized routing through firewall improves control and visibility
- Clear separation of trust zones (Admin, Test, Home, IoT, Sandbox, Servers)
- Scalable architecture for future expansion

### Known Gaps (Future Hardening Required)
- Unused ports not yet disabled
- Port security (MAC limiting) not implemented
- VLAN 1 still present in switch configuration baseline (to be removed)
- Logging and monitoring not yet integrated into SIEM

## Future Improvements

### Switch Hardening
- Disable unused ports
- Move unused ports to isolated VLAN
- Enable port security controls
- Remove or restrict VLAN 1 usage

### Monitoring
- Forward switch logs to SIEM (Splunk)
- Enable centralized logging correlation with firewall events

### Physical Layer Improvements
- Label all switch ports physically and in documentation
- Reduce active port ranges per VLAN for tighter control

## Status
- [x] Completed
- [ ] Pending
- [ ] Failed
- [ ] Rolled Back

## Notes
This configuration represents the stable Layer 2 foundation of the home lab network. It is intentionally designed to be simple at the switch layer, with all routing and policy enforcement centralized at the firewall.

This separation of concerns (Switch = segmentation, Firewall = control plane) aligns with standard enterprise network design patterns and provides a scalable base for future security enhancements and monitoring integration.
