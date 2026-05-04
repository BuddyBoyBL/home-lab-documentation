# Change Management Log

## Change ID
CM-2026-04-11-0300

## Date & Time
2026-04-11 | 03:00 (Local Time)

## Author
Ben

## Change Type
- [ ] Standard
- [x] Normal
- [ ] Emergency

## Affected Systems
- OPNsense Firewall
- VLAN Infrastructure
- Core Network Segmentation
- Server, Test, Home, IoT, and Sandbox Networks

## Description of Change
Implemented initial firewall rule set within OPNsense to enforce VLAN-based segmentation and control inter-network communication.

Rules were structured per VLAN interface and follow a top-down evaluation model where first match wins. This represents the initial functional security policy layer for the home lab environment.

## Reason for Change
- Establish baseline security controls between VLANs
- Enforce separation between trust zones (Admin, Test, Home, IoT, Servers, Sandbox)
- Enable controlled communication for services such as Active Directory and logging
- Prepare infrastructure for future hardening (logging, IDS/IPS, refined rule sets)

## Pre-Change State
- VLANs defined but not actively controlled by firewall policy
- Inter-VLAN traffic behavior was not formally restricted
- No structured rule hierarchy in place

## Implementation Steps
1. Defined VLAN-specific firewall rule sets in OPNsense
2. Implemented MGMT VLAN rules for administrative access and temporary full access
3. Configured TEST VLAN rules for controlled access to:
   - Active Directory services
   - Log forwarding (Splunk)
   - WAN access
4. Applied restrictive rules for HOME VLAN to isolate from internal networks
5. Hardened IOT VLAN to restrict internal network access
6. Configured SANDBOX VLAN for isolation and containment
7. Established SERVERS VLAN rules allowing controlled management access
8. Created network, host, and port aliases for rule simplification

## Post-Change State
- VLAN segmentation is now actively enforced at firewall level
- Inter-VLAN communication is explicitly controlled via rule sets
- Administrative access paths are defined through MGMT VLAN
- IoT and Sandbox environments are logically isolated from core infrastructure
- Server environment has controlled access paths for management and services

## Validation / Testing
- Verified VLAN-to-VLAN traffic follows rule behavior
- Confirmed MGMT access to infrastructure systems
- Confirmed TEST VLAN can reach required services (AD, Splunk)
- Confirmed HOME and IOT VLANs are restricted from internal networks
- Verified implicit deny behavior on all VLAN interfaces

## Impact Assessment
- Expected Impact: Full enforcement of network segmentation and improved security posture
- Actual Impact: Successful segmentation enforcement with functional service connectivity maintained
- Downtime: None (configuration-based deployment)

## Rollback Plan
- Disable interface-level firewall rules in OPNsense
- Revert to pre-rule state where VLANs operate without enforced segmentation policies

## Current Security Posture

### Strengths
- VLAN segmentation is actively enforced
- Critical infrastructure is logically isolated
- Administrative access is restricted to MGMT VLAN
- Sandbox and IoT environments are contained

### Known Gaps (Future Hardening Required)
- MGMT VLAN rules are currently overly permissive
- SERVERS VLAN allows outbound WAN access (temporary design choice)
- WAN rules use broad allowances (not port-restricted yet)
- No logging on deny rules enabled yet
- IDS/IPS not yet integrated

## Status
- [x] Completed
- [ ] Pending
- [ ] Failed
- [ ] Rolled Back

## Notes
This change represents the transition from logical network design to enforced security policy. It establishes the foundation for all future hardening work including logging, intrusion detection, and stricter administrative access controls.

The current rule set is intentionally permissive in select areas to allow system functionality during early-stage lab development and will be progressively hardened in future iterations.
