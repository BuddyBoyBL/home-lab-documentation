Change Management Log

Change ID
CM-2026-04-23-2000-NET-External-Switch

Date & Time
2026-04-23 | 20:00 (Local Time)

Author
Ben

Change Type
Standard
Normal
Emergency

Affected Systems
Hyper-V Virtual Switch Manager
Host Network Adapter (Wi-Fi)
Domain Controller VM

Description of Change
Created and configured an external virtual switch to allow communication between the Domain Controller VM and physical devices on the home network.

Reason for Change
Enable VM-to-physical communication
Allow domain join from physical test PC
Bridge lab environment to real network

Pre-Change State
VM connected to internal switch
No communication with physical devices

Implementation Steps
Opened Hyper-V Virtual Switch Manager
Created External Virtual Switch
Bound switch to Wi-Fi adapter
Enabled host sharing
Attached VM network adapter to external switch

Post-Change State
VM receives IP from home network
VM reachable from physical devices

Validation / Testing
Confirmed IP assignment via ipconfig
Tested ping from test PC to VM
Verified DNS resolution

Impact Assessment
Expected Impact: Network connectivity between VM and physical devices
Actual Impact: Successful communication established
Downtime: Brief network interruption during switch creation

Rollback Plan
Reassign VM to internal switch
Remove external switch

Status
Completed
Pending
Failed
Rolled Back

Notes
This change is required for physical client domain integration.

Evidence
/assets/screenshots/networking
