# Change Management Log

## Change ID
CM-2026-04-11-0200

## Date & Time
2026-04-11 | 02:00 (Local Time)

## Author
Ben

## Change Type
- [ ] Standard
- [x] Normal
- [ ] Emergency

## Affected Systems
- Cisco SG300 Layer 3 Switch
- Guest Network Router
- Xfinity Gateway / Modem
- Physical Network Infrastructure

## Description of Change
Relocated the Cisco Layer 3 switch and guest network router from the upstairs area to the primary workspace (bedroom). 

Established a direct physical Ethernet connection from the workspace to the Xfinity modem by running a dedicated cable between floors. Cable management improvements were implemented using wall clamps to maintain a clean and organized setup.

## Reason for Change
- Centralize network infrastructure for easier management and troubleshooting
- Enable direct access to core networking equipment during lab development
- Improve reliability and reduce dependency on indirect or temporary connections
- Prepare environment for future firewall integration and VLAN segmentation testing

## Pre-Change State
- Cisco switch and guest router were located upstairs near the modem
- Limited physical access to networking equipment during lab work
- No dedicated Ethernet run to the primary workspace
- Increased reliance on less efficient or indirect connectivity methods

## Implementation Steps
1. Powered down and safely disconnected Cisco switch and guest router
2. Physically relocated both devices to the workspace
3. Ran a long-distance Ethernet cable from workspace to modem (upstairs)
4. Secured cable using wall clamps for stability and organization
5. Reconnected switch and router to network infrastructure
6. Verified physical connectivity and link status

## Post-Change State
- Core networking equipment is now located in the workspace
- Direct Ethernet connection established between workspace and modem
- Improved accessibility for configuration, monitoring, and troubleshooting
- Network layout better aligned with lab development needs

## Validation / Testing
- Verified link lights on switch and modem
- Confirmed physical connectivity is stable
- Ensured devices power on and reconnect properly
- No immediate connectivity issues observed

## Impact Assessment
- Expected Impact: Improved workflow and network reliability
- Actual Impact: Positive; easier access to infrastructure and cleaner setup
- Downtime: Minimal during device relocation

## Rollback Plan
- Move switch and router back to original upstairs location
- Disconnect Ethernet run and restore previous network layout

## Status
- [x] Completed
- [ ] Pending
- [ ] Failed
- [ ] Rolled Back

## Notes
This change improves the physical accessibility and organization of the home lab environment. Centralizing the network hardware supports more efficient troubleshooting, faster iteration, and better alignment with future planned upgrades such as firewall deployment and VLAN segmentation.
