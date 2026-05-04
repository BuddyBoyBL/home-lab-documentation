# Major Milestone: Firewall, Switch, Active Directory, and M365 Lab Setup

## Summary
Reached a major milestone in the home lab by successfully bringing core infrastructure online, including the firewall, switch, Active Directory environment, and Microsoft 365 test environment. The lab is now transitioning from initial build-out into active use and scenario-based practice.

## Firewall Deployment (OPNsense)

### Issue Resolved
Initial installation issues were traced to a **third-party hard drive**. After replacing it with a more reliable drive (with increased storage capacity), installation was successful.

### Current State
- OPNsense installed and accessible via UI
- strong baseline configurations applied
- base configuration backed up

### Network Setup
- initial VLANs created
- basic segmentation implemented
- basic firewall rules applied

## Switch Integration

### Issue Encountered
- initial configuration resulted in a **system lockout**

### Resolution
- reconfigured switch access
- successfully reconnected and restored functionality

### Current State
- switch is now integrated with the firewall
- VLAN and segmentation foundation is in place

## Active Directory Setup

### Current State
- domain controller configured
- test PC joined to the domain
- multiple users created

### Policy Work
- began implementing **basic group policies**
- establishing user and system management structure

## Microsoft 365 Lab Environment

### Setup
- created a **trial Microsoft 365 environment**
- built a test structure with **~10 users**
- users organized across different departments

### Focus Areas
- user lockout scenarios
- MFA implementation
- principle of least privilege based on roles

## Help Desk Simulation

### ServiceNow Integration
- using ServiceNow to practice:
  - ticket workflow
  - incident tracking
  - documentation

### Goal
- simulate real help desk scenarios
- connect user issues to backend systems (AD, M365, endpoints)

## Virtual Machine Environment

### Created Systems
- Windows 11 VM
- Windows 11 Pro VM

### Purpose
- simulate user environments
- practice troubleshooting scenarios
- test policies and configurations

## Current Direction

The lab is now shifting from build phase to **practice and validation phase**:

- practice real-world support scenarios
- validate connectivity across all systems
- reinforce AD, M365, and endpoint workflows
- begin preparing for log ingestion and monitoring

## Next Phase

### Logging and Monitoring
- configure Splunk fully
- validate log intake
- begin building queries and alerts

### Documentation
- revisit and refine documentation
- ensure all configurations are clearly recorded
- align repo structure with actual lab state

## Future Plans

- implement endpoint detection (e.g., security tooling)
- expand and refine firewall rules
- implement DNS filtering
- set up cloud storage and backup solutions
- create SOPs (standard operating procedures)
- restructure and refine GitHub repo based on completed work

## Key Takeaway
The lab has moved from initial setup into a functional multi-system environment. Core infrastructure is now in place, and the focus is shifting toward real-world usage, security monitoring, and operational practice.
