Change Management Log

Change ID
CM-2026-04-23-2000-AD-Client-Join

Date & Time
2026-04-23 | 20:00 (Local Time)

Author
Ben

Change Type
Standard
Normal
Emergency

Affected Systems
Physical Test PC (Windows 10 Pro)
Domain Controller (HL-DC01)

Description of Change
Configured physical test PC to join Active Directory domain.

Reason for Change
Validate domain functionality
Enable centralized authentication
Prepare for policy enforcement testing

Pre-Change State
Test PC in workgroup
DNS pointing to router

Implementation Steps
Configured DNS to Domain Controller IP
Validated connectivity (ping + nslookup)
Initiated domain join
Authenticated with domain admin account
Restarted system

Post-Change State
Test PC joined to domain
Computer object created in AD
Ready for GPO application

Validation / Testing
Verified domain login
Confirmed device appears in AD
Moved system to Workstations OU

Impact Assessment
Expected Impact: Domain integration
Actual Impact: Successful join
Downtime: Required restart

Rollback Plan
Remove device from domain
Reset DNS to router

Status
Completed
Pending
Failed
Rolled Back

Notes
This confirms proper DNS and AD functionality.

Evidence
/assets/screenshots/domain-join
