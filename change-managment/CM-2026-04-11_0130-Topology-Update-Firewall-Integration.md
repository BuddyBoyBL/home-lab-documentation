# Change Management Log

## Change ID
CM-2026-04-11-0130

## Date & Time
2026-04-11 | 01:30 (Local Time)

## Author
Ben

## Change Type
- [ ] Standard
- [x] Normal
- [ ] Emergency

## Affected Systems
- Network Topology
- pfSense Firewall (Planned Integration)
- Cisco L3 Switch
- Test PC (Dell Inspiron 660s)
- Lab Network Segment

## Description of Change
Updated the network topology to reflect a direct physical connection between the test PC and the Cisco Layer 3 switch. 

Removed the previous design assumption where the test network was routed through a secondary router. The updated design prepares for direct VLAN-based segmentation and future firewall integration.

## Reason for Change
- Simplify network architecture
- Align lab design with enterprise best practices (switch-based segmentation instead of router dependency)
- Prepare for pfSense firewall insertion as the primary control point
- Enable more accurate VLAN testing and traffic visibility

## Pre-Change State
- Test PC was planned to connect through a secondary router
- Network segmentation relied partially on router-based separation
- Firewall not yet integrated into topology

## Implementation Steps
1. Reviewed physical distance and confirmed feasibility of direct Ethernet connection (~78 ft)
2. Planned use of 100 ft Ethernet cable for connection to switch
3. Updated topology design to remove intermediate router
4. Adjusted network flow to:
   Internet → Firewall (planned) → L3 Switch → VLANs → Endpoints

## Post-Change State
- Test PC will connect directly to Cisco L3 switch
- VLAN segmentation handled at switch level
- Network prepared for centralized firewall control (pfSense)
- Cleaner and more scalable architecture

## Validation / Testing
- Physical feasibility confirmed (distance + cabling plan)
- Logical topology reviewed and aligned with intended VLAN structure
- No connectivity testing yet (pending hardware setup)

## Impact Assessment
- Expected Impact: Improved network clarity and segmentation control
- Actual Impact: None (design-level change, no live disruption)
- Downtime: None

## Rollback Plan
- Reintroduce secondary router into topology if direct VLAN configuration fails
- Revert to previous routing structure for test network

## Status
- [x] Completed
- [ ] Pending
- [ ] Failed
- [ ] Rolled Back

## Notes
This change represents a shift from a hybrid router-based design to a switch-centric architecture, which better reflects enterprise network design principles. This also sets the foundation for future firewall rules, IDS/IPS integration, and traffic monitoring.
