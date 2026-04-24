Change Management Log

Change ID
CM-2026-04-23-2000-GPO-Workstation-Security

Date & Time
2026-04-23 | 20:00 (Local Time)

Author
Ben

Change Type
Standard
Normal
Emergency

Affected Systems
Active Directory
Workstations OU
Domain Clients

Description of Change
Created and applied a workstation security Group Policy.

Reason for Change
Improve endpoint security
Demonstrate centralized policy enforcement
Prepare for security monitoring scenarios

Pre-Change State
No enforced workstation policies

Implementation Steps
Created GPO: Workstation Security Policy
Configured screen lock timeout (5 minutes)
Enabled password requirement on resume
Linked GPO to Workstations OU

Post-Change State
All workstations receive security policy
Users required to reauthenticate after inactivity

Validation / Testing
Ran gpupdate /force on client
Observed screen lock behavior
Verified policy application using gpresult

Impact Assessment
Expected Impact: Improved endpoint security
Actual Impact: Policy successfully enforced
Downtime: None

Rollback Plan
Unlink or delete GPO
Restore default policy behavior

Status
Completed
Pending
Failed
Rolled Back

Notes
This is the first enforced policy in the environment and establishes baseline security.

Evidence
/assets/screenshots/gpo
