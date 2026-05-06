# Change Management Log

## Change ID
CM-2026-04-11-0400

## Date & Time
2026-04-11 | 04:00 (Local Time)

## Author
Ben

## Change Type
- [ ] Standard
- [x] Normal
- [ ] Emergency

## Affected Systems
- Cisco SG300 Switch
- VLAN Trunk Configuration
- Host Operating System Connectivity
- Virtual Machine Network Segmentation
- SERVERS VLAN (60)
- HOME VLAN (30)

## Description of Change
Configured a secondary trunk port on the Cisco SG300 switch and implemented VLAN-aware segmentation between the host operating system and virtualized lab infrastructure.

The current design allows:
- the host operating system to remain on the HOME VLAN
- virtual machines and server workloads to operate on VLAN 60 (SERVERS)

This creates logical separation between the daily-use host environment and the lab/server infrastructure while maintaining operational flexibility during the current development phase.

## Reason for Change
- Improve separation between personal usage and lab infrastructure
- Begin implementing more secure VM network segmentation
- Prepare for long-term isolation of management and server traffic
- Reduce unnecessary exposure between host OS and server environments

## Pre-Change State
- Host operating system and virtualized infrastructure were less clearly separated
- Secondary trunking behavior was not implemented
- VLAN segmentation for VM traffic was incomplete

## Implementation Steps
1. Configured second switch port as trunk port
2. Enabled VLAN tagging across trunk interface
3. Configured VLAN 60 for server and VM traffic
4. Maintained host operating system connectivity on HOME VLAN
5. Verified host and VM connectivity behavior after segmentation changes

## Post-Change State
- Secondary trunk port operational
- VLAN-aware VM segmentation functioning
- Host operating system remains on HOME VLAN
- Virtualized infrastructure isolated within SERVERS VLAN
- Improved separation between personal and lab traffic

## Validation / Testing
- Verified trunk port functionality
- Confirmed VLAN 60 connectivity for server workloads
- Confirmed host operating system retains HOME VLAN access
- Verified segmentation behavior between environments

## Impact Assessment
- Expected Impact: Improved segmentation and safer separation of workloads
- Actual Impact: Successful VLAN separation with functional connectivity maintained
- Downtime: Minimal during switch reconfiguration

## Rollback Plan
- Revert secondary trunk port to access mode
- Remove VLAN tagging from VM interfaces
- Restore previous flat or mixed connectivity model

## Current Design Notes

### Temporary Design
Current architecture intentionally keeps the host operating system connected to the HOME VLAN while virtual machines operate within VLAN 60.

### Planned Future Design
Future implementation plans include:
- moving the host operating system to Wi-Fi connectivity
- reserving physical Ethernet/VLAN trunking primarily for virtual machines and segmented infrastructure
- further reducing direct exposure between personal usage and lab/server traffic

## Security Benefits
- Better isolation between host OS and infrastructure systems
- Reduced accidental crossover between personal and lab traffic
- Improved alignment with least-privilege network segmentation principles
- Cleaner separation of trust zones

## Status
- [x] Completed
- [ ] Pending
- [ ] Failed
- [ ] Rolled Back

## Notes
This change represents a major improvement in the logical separation of the homelab environment. The transition from mixed connectivity toward dedicated VLAN-aware virtualization networking more closely aligns the lab with enterprise segmentation practices.

The current configuration is considered an intermediate phase before the host operating system is fully separated onto wireless connectivity.
