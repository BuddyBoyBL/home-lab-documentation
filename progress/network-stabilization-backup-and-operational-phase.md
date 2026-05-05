# Progress Update: Network Stabilization, Backup, and Transition to Operations

## Summary
Reached a stable point in the home lab where core infrastructure is functioning correctly. Focus is now shifting from setup into backup, validation, and operational practice.

## Network Infrastructure Status

### Cisco Switch
- fully operational
- VLANs configured
- ports properly assigned
- segmentation working as intended

### Firewall (OPNsense)
- successfully deployed after resolving earlier hardware issues
- stable storage now in place
- VLAN segmentation configured
- baseline firewall rules implemented

## Key Milestone
Core network components are now:
- connected
- configured
- stable

This marks the transition from **build phase → operational phase**.

## Backup Plan

### Immediate Actions
- back up switch configuration
- back up firewall configuration
- store backups on external hard drive

### Purpose
- protect against configuration loss
- allow quick recovery if systems reset or fail
- create a repeatable baseline for future changes

## Next Phase: User and Help Desk Scenarios

### Microsoft 365 Lab
- continue using trial environment
- expand user-based testing scenarios

### Active Directory Integration
- ensure AD is fully connected to the network
- validate domain connectivity from test systems

### Practice Scenarios
- user lockouts
- password resets
- remote desktop troubleshooting
- general help desk workflows

## Upcoming Phase: Logging and Monitoring

### Splunk Preparation
- ensure Splunk server is running
- begin forwarding logs from:
  - endpoints
  - network devices (if applicable)
- validate log ingestion

### Next Steps After Ingestion
- begin writing queries
- build basic alerts
- test detection scenarios

## Current Direction
The lab is now focused on:
- validating configurations
- practicing real-world scenarios
- preparing for monitoring and detection work

## Future Follow-Up
- refine firewall rules beyond baseline
- expand endpoint monitoring
- improve documentation across all components
- ensure all systems are properly integrated and communicating

## Key Takeaway
The lab has moved into a functional state where real-world workflows can be practiced. With infrastructure stabilized, the focus is now on operational use, validation, and monitoring.
