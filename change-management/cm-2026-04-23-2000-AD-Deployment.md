Change Management Log

Change ID
CM-2026-04-23-2000-AD-Deployment

Date & Time
2026-04-23 | 20:00 (Local Time)

Author
Ben

Change Type
Standard
Normal
Emergency

Affected Systems
Domain Controller (HL-DC01)
Hyper-V Virtual Networking
Physical Test PC (Windows 10 Pro)
Active Directory Domain (homelab.local)

Description of Change
Initial deployment and configuration of Active Directory Domain Services, including Domain Controller setup, DNS configuration, and network integration with a physical client system.

Reason for Change
Establish centralized authentication and identity management
Enable domain-based user and device control
Prepare for security testing and monitoring
Create scalable infrastructure for future segmentation

Pre-Change State
No Active Directory environment
Standalone systems (workgroup)
No centralized authentication
VM isolated on internal switch

Implementation Steps
Installed Windows Server 2022 (Desktop Experience)
Promoted server to Domain Controller
Created domain: homelab.local
Configured static IP and DNS
Created external virtual switch (Wi-Fi adapter)
Connected VM to external switch
Created OUs (Users, Workstations, Admins, Groups)
Created groups (IT_Admins, Remote_Users, Contractors)
Created test users (~25)
Created and linked GPO (Workstation Security Policy)

Post-Change State
Active Directory operational
DNS resolving correctly
VM accessible from physical network
AD structure organized
GPO applied

Validation / Testing
Verified domain login
Tested DNS resolution
Tested network connectivity
Confirmed AD tools operational

Impact Assessment
Expected Impact: Centralized authentication and management
Actual Impact: Successful deployment and stable operation
Downtime: None

Rollback Plan
Demote Domain Controller
Disconnect external switch
Revert DNS settings on clients

Status
Completed
Pending
Failed
Rolled Back

Notes
This establishes the core identity system for the homelab. All future systems will integrate into this structure.

Evidence
/assets/screenshots/active-directory
