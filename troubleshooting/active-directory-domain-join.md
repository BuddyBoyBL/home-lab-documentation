Active Directory Domain Join — Complete Troubleshooting Saga

Overview

This document captures the complete troubleshooting journey of a multi-week Active Directory domain join failure affecting both Hyper-V VMs and a physical Windows 10 PC. It details the systematic elimination of false leads, identification of the true root cause (missing RPC ports), and the operational lessons learned.


Scope: Domain join, Kerberos authentication, firewall rule analysis, Hyper-V checkpoint impact

Outcome: Successful domain authentication on both physical and virtual clients; RPC ports identified as critical


Initial Symptom

Error Message:

The security database on the server does not have a computer account 
for this workstation trust relationship.

Affected Machines:


DESKTOP-70JQB6F (Hyper-V VM, Windows 10)
DESKTOP-SNB7KKD (Physical PC, Windows 10)


Frequency: Persistent, survives rejoin attempts and secure channel repairs

First Occurrence: June 18, 2026 after initial domain join attempt


Environment

ComponentValueDomainhomelab.localDC HostnameWIN-MLT4OCA4COKDC OSWindows Server 2022DC IP192.168.60.20 (VLAN 60 / Servers)VM HypervisorHyper-V on BLUE2FirewallOPNsense @ 192.168.1.1SwitchCisco SG300 @ 192.168.99.2


Phase 1 — Initial Troubleshooting (False Leads)

Suspected Cause #1: Switch Port Configuration

Hypothesis:
VM network adapter plugged into a port not configured for VLAN 20.

Diagnostics Performed:

powershellipconfig /all  # confirmed 192.168.20.40, gateway 192.168.20.1, DNS 192.168.60.20
Test-NetConnection 192.168.60.20 -Port 88  # returned TcpTestSucceeded: True

Finding:
Client network config was correct; port 88 (Kerberos) was reachable.

Resolution Applied:
Verified Cisco SG300 Port 2 (GE2) was configured as trunk with VLAN 20T and 60T tagged. Configuration was correct.

Lesson:
VLAN trunk ports must explicitly include all required VLANs in tagged mode, not just access mode for a single VLAN.


Suspected Cause #2: Firewall Rules Not Applied

Hypothesis:
OPNsense firewall rules were configured but not committed/applied.

Diagnostics Performed:


Reviewed TEST interface rules in OPNsense GUI
Confirmed rules existed but status showed "staged, not applied"


Resolution Applied:
Clicked "Apply" button in OPNsense (Actions → Apply); confirmed status changed to active.

Added Rule:


ICMP pass rule on TEST interface (required for ping)


Lesson:
OPNsense configuration changes require explicit "Apply" step. Staged changes have no effect until committed.


Suspected Cause #3: DC Hostname DNS A Record Missing

Hypothesis:
Client couldn't resolve the DC's hostname, breaking Kerberos hostname-to-IP lookup.

Diagnostics Performed:

powershell# On client:
nslookup win-mlt4oca4cok.homelab.local
# Result: "Non-existent domain"

# On DC:
Get-DnsServerResourceRecord -ZoneName "homelab.local" -Name "win-mlt4oca4cok"
# Result: Record exists with 192.168.60.20

Finding:
Record existed on DC but client saw it as non-existent (stale negative DNS cache).

Resolution Applied:

powershell# On client:
ipconfig /flushdns
nslookup win-mlt4oca4cok.homelab.local
# Result: Now resolves to 192.168.60.20

Lesson:
DNS negative caches ("this doesn't exist") can persist even after records are added. A simple flush clears the stale cache and allows proper resolution.


Phase 2 — Clock Skew Investigation (Partial Red Herring)

Suspected Cause: DC and Client Clock Out of Sync

Initial Observation:

DC time (w32tm):   Leap Indicator: 3 (not synchronized)
DC source:         VM IC Time Synchronization Provider (Hyper-V)
Client time:       ~4 hours ahead of DC

Why This Matters:
Kerberos requires client and server clocks to be within 5 minutes. A 4-hour skew would absolutely break authentication.

Root Cause Identified:


DC's time sync was being overridden by Hyper-V Integration Services instead of pulling from a reliable external NTP source
DC was stuck trusting the hypervisor's clock instead of an authoritative time server
This caused DC to drift and flag itself "not synchronized"


Resolution Applied:

Step 1: Disable Hyper-V Time Sync on DC VM


Hyper-V Manager → AD VM Settings → Integration Services → uncheck "Time synchronization"


Step 2: Configure DC to Use External NTP

powershellw32tm /config /manualpeerlist:"time.windows.com,0x8 pool.ntp.org,0x8" /syncfromflags:manual /reliable:yes /update
Restart-Service w32time
w32tm /resync /rediscover
w32tm /query /status
# Result: Leap Indicator: 0 (no warning), synced to pool.ntp.org

Outcome:
Clock skew was real and needed fixing, but it was NOT the sole blocker. Even with perfect time sync, logins still failed. This was a necessary fix but not sufficient.

Lesson:
A domain controller should NEVER take time from its hypervisor. DCs must be authoritative time sources, configured to sync from external NTP (e.g., pool.ntp.org or time.windows.com). Hyper-V time sync fighting w32time creates deadlock.


Phase 3 — Checkpoint Damage (Major Discovery)

Suspected Cause: Hyper-V Checkpoints Rolling Back Machine Account Password

Observation:
After every power event on the VM:


Login worked momentarily
Then broke on next boot
Pattern repeated across multiple rejoin attempts


Root Cause Identified:
Hyper-V checkpoints (snapshots) were frozen on the VM, and on every boot the system could be reverted to a checkpoint. This rollback resets the VM Generation ID, which Windows AD uses as a security marker. When Generation ID changes:


Windows deliberately invalidates the machine account password (as a USN rollback protection mechanism)
DC has the current password; client has the old one from the checkpoint
Trust relationship breaks permanently until the next join


Evidence:


VM login error: "Would you like to continue from the last checkpoint or current state?"
This prompt appeared because checkpoints existed
Event 2168 in DC event log showed VM Generation ID changes


Resolution Applied:

powershell# On Hyper-V host (PowerShell as admin):
Get-VM -Name "WinProClient" | Get-VMSnapshot | Remove-VMSnapshot -IncludeChildren

# Disable automatic checkpoints going forward:
Set-VMHost -AutomaticCheckpointsEnabled $false

# Verify no snapshots remain:
Get-VM -Name "WinProClient" | Get-VMSnapshot
# Result: (no output = no snapshots)

Also Critical:


Never use "Save State" or "Pause" on domain-joined VMs
Always use "Shut Down" (clean OS shutdown) and "Start"
This ensures machine passwords stay in sync and don't get rolled back


Lesson:
Checkpoints and domain membership are incompatible. Every checkpoint revert/resume can invalidate the machine password. In a lab, disable checkpoints entirely on domain-joined machines. In production, never pause/save a domain controller.


Phase 4 — RPC Ports (THE ACTUAL ROOT CAUSE)

Suspected Cause: Firewall Blocking RPC Endpoint Mapper and Dynamic RPC Ports

The Critical Observation:
After all the fixes above (time, DNS, checkpoints), logins STILL failed with the trust error. However:

powershellTest-ComputerSecureChannel -Verbose
# Result: True
# "The secure channel between the local computer and the domain homelab.local is in good condition."

The Contradiction:
Secure channel = healthy (NTLM-based, doesn't require RPC)

Domain login = fails with trust error (Kerberos-based, requires RPC)

Root Cause Identified:
The firewall's AD_PORTS alias was allowing:


Port 53 (DNS) ✓
Port 88 (Kerberos) ✓
Port 389 (LDAP) ✓
Port 445 (SMB) ✓
Port 464 (Kerberos password change) ✓
Port 3268 (LDAP Global Catalog) ✓


But was MISSING:


Port 135 (RPC Endpoint Mapper) ✗
Ports 49152–65535 (Dynamic RPC high-port range) ✗


Why This Breaks Domain Operations:


Add-Computer (domain join) requires RPC to negotiate machine account creation
Interactive logon requires RPC to complete Kerberos handshake
Remove-Computer (leave domain) requires RPC to clean up machine account
RPC starts negotiation on port 135, then gets redirected to a dynamic high port (49152+)
If either is blocked, the entire RPC conversation fails


Evidence:
During rejoin attempt on physical PC:

Error: "The RPC server is unavailable."

This was the first direct RPC failure message, finally confirming the issue.

Resolution Applied:

Option 1: Temporary (for testing)
Changed the OPNsense firewall rule to allow ALL traffic from TEST VLAN to 192.168.60.20 to verify the hypothesis.

Result: Domain join succeeded immediately

Option 2: Permanent (recommended)
Updated the AD_PORTS alias in OPNsense to include:

Original alias:     53, 88, 389, 445, 464, 3268
Updated alias:      53, 88, 135, 389, 445, 464, 3268, 49152-65535

Applied the rule to both TEST and physical PC network paths.

Outcome:
Domain joins, logins, and domain operations all succeeded.

Lesson:
Active Directory is fundamentally RPC-dependent. While individual AD services (DNS, Kerberos, LDAP) are visible and testable, RPC is the critical hidden dependency. Port 135 (Endpoint Mapper) + the dynamic RPC range (49152–65535) must be open between clients and the DC. This is often overlooked because Kerberos and LDAP tests pass even without RPC. Test RPC connectivity with:

powershellGet-WmiObject -ComputerName <DC-IP> -Class Win32_ComputerSystem -Credential domain\admin

If this fails with RPC error while Test-NetConnection -Port 88 succeeds, the dynamic RPC range is blocked.


Phase 5 — User Account Forced Password Change Loop

Suspected Cause: Password Change at Logon Flag Not Properly Cleared

Symptom:
After successful domain join, login prompted: "User must change password before signing in."

Every password entered was rejected, even if complex and matching confirmation.

Root Cause Identified:
The user account was created with ChangePasswordAtLogon $true flag, but the domain's password policy had conflicting requirements:


Minimum password age: 1 day (can't change password until 1 day after creation)
Password history: 24 (can't reuse recent passwords)
Minimum length: 14 characters


When forced to change at logon, Windows couldn't satisfy both "you must change it now" and "you can't change it yet" simultaneously, causing a deadlock.

Resolution Applied:

Immediate Fix:

powershell# On DC:
Set-ADUser -Identity "will.smith" -ChangePasswordAtLogon $false
Set-ADAccountPassword -Identity "will.smith" -Reset -NewPassword (ConvertTo-SecureString "NewPassword123!@ABC" -AsPlainText -Force)

This bypassed the forced-change loop by setting the password directly from the admin side.

Best Practice Going Forward:
When creating new users, do NOT set ChangePasswordAtLogon $true. Instead:

powershellNew-ADUser -Name "Test User" `
    -SamAccountName "testuser" `
    -UserPrincipalName "testuser@homelab.local" `
    -AccountPassword (ConvertTo-SecureString "TempPassword123!@" -AsPlainText -Force) `
    -Enabled $true `
    -ChangePasswordAtLogon $false

Users can manually change their password on first login via Ctrl+Alt+Delete → Change Password, which doesn't trigger the policy deadlock.

Lesson:
The forced-change-at-logon feature conflicts with minimum password age policies. For labs and development, set ChangePasswordAtLogon $false and educate users to change passwords manually. This avoids the deadlock entirely.


Phase 6 — Remote Desktop Users Group Restriction

Symptom After Successful Login

After domain join finally succeeded and RPC ports were open, the physical PC logged in cleanly, but the VM showed:

"To sign in remotely, you need the right to sign in through Remote Desktop Services. 
By default, members of the Remote Desktop Users group have this right."

Root Cause Identified:
This is NOT a trust error or authentication failure. The domain authenticated the user successfully but the VM has a local security policy restricting interactive logon to members of "Remote Desktop Users" group only. This is a feature, not a bug — a security boundary on that specific machine.

Resolution Applied:

powershell# On the VM (as local admin):
Add-LocalGroupMember -Group "Remote Desktop Users" -Member "homelab\will.smith"

Outcome:
User can now log in interactively to the VM.

Lesson:
Authentication ≠ Authorization. A user can be authenticated to the domain but still be blocked from logging in by local group membership or GPO restrictions. This is expected behavior for security-hardened machines.


Complete Diagnostic Checklist

Use this checklist to diagnose future domain join issues:

TestCommandExpected ResultIndicatesDC Healthdcdiag /test:netlogons /test:machineaccount /test:advertisingAll PASSEDDC is operationalTime Syncw32tm /query /statusLeap Indicator: 0Clock is synchronizedDNS Domainnslookup homelab.localResolves to DC IPDomain DNS worksDNS DC Hostnamenslookup win-mlt4oca4cok.homelab.localResolves to 192.168.60.20DC hostname resolvesKerberos PortTest-NetConnection <DC-IP> -Port 88TcpTestSucceeded: TrueKerberos port openRPC PortTest-NetConnection <DC-IP> -Port 135TcpTestSucceeded: TrueRPC Endpoint Mapper openRPC ConversationGet-WmiObject -ComputerName <DC-IP> -Class Win32_ComputerSystem -Credential domain\adminReturns system infoFull RPC workingSecure ChannelTest-ComputerSecureChannel -VerboseTrue + "in good condition"Machine trust is validDomain LoginLog in at login screen with domain\userReaches desktopFull authentication working


Operational Lessons for Production

1. Hyper-V Domain Controllers and Clients

Never:


Pause or Save a domain controller or domain-joined VM
Enable checkpoints on domain-joined machines
Let Hyper-V time sync control the DC's clock


Always:


Use clean Shut Down and Start for domain machines
Configure DC to sync time from external NTP (pool.ntp.org, time.windows.com)
Disable Hyper-V time synchronization on the DC VM
Boot the DC before any clients


2. Firewall Rules for Active Directory

Minimum ports required:

TCP/UDP 53      - DNS
TCP/UDP 88      - Kerberos
TCP 135         - RPC Endpoint Mapper (CRITICAL — often forgotten)
TCP/UDP 389     - LDAP
TCP 445         - SMB
TCP 464         - Kerberos password change
TCP 3268        - LDAP Global Catalog
TCP 49152-65535 - Dynamic RPC range (CRITICAL — often forgotten)

Test RPC explicitly:

powershellGet-WmiObject -ComputerName <DC-IP> -Class Win32_ComputerSystem -Credential domain\admin

3. User Account Best Practices

For new users, avoid:

powershell-ChangePasswordAtLogon $true  # Creates deadlock with password policies

Instead:

powershell-ChangePasswordAtLogon $false  # Let users change manually via Ctrl+Alt+Del

4. Troubleshooting Order

When domain joins fail:


Verify DC is healthy (dcdiag, services, AD objects)
Verify time is synchronized (DC + client within 5 min)
Verify network path: DNS, port 88, port 135, dynamic RPC range
Verify computer object exists on DC
Run Test-ComputerSecureChannel -Repair on client
Do clean rejoin: Remove-ADComputer, then Add-Computer


Do NOT repeatedly rejoin without addressing underlying network/time/firewall issues.


Root Cause Summary

PhaseSuspected CauseActual StatusImpactFixed1Switch port configCorrectNoneN/A1Firewall rules not appliedNot appliedFull block✓ Applied1DC hostname DNS recordMissing from client cacheIntermittent✓ Flushed2Clock skewReal (DC drifted 4+ hours)Kerberos breaks✓ Fixed NTP3CheckpointsExisted, rolling back trustRecurring breaks✓ Deleted4RPC ports blockedTRUE ROOT CAUSEAll domain ops fail✓ Opened5User password policyReal (deadlock)Login loop✓ Fixed6Remote Desktop groupBy designLocal restriction✓ Added group

The actual root cause was RPC ports 135 + 49152–65535 being blocked. While other issues (time, DNS, checkpoints) were real and needed fixing, they were mask the fundamental RPC problem. RPC is invisible to most AD tests but critical for all join/login/leave operations.


Testing Evidence

Physical PC — Successful Domain Login (June 24, 2026):

Test-ComputerSecureChannel -Verbose
VERBOSE: The secure channel between the local computer and the domain homelab.local is in good condition.

login as: homelab\will.smith
password: [entered]
[Desktop reached successfully]

VM — Successful Domain Authentication (after RPC ports opened):

Test-ComputerSecureChannel -Verbose
True
VERBOSE: The secure channel between the local computer and the domain homelab.local is in good condition.

[Remote Desktop group restriction message — resolved by adding user to group]
[Desktop reached successfully]


Timeline

DateEvent2026-06-10Initial DC and AD setup2026-06-18VM domain join attempted; trust error first observed2026-06-18Switch Port 2 VLAN trunk configuration fixed2026-06-18OPNsense firewall rules applied2026-06-18DC hostname DNS A record verified; client cache flushed2026-06-19DC clock skew discovered and fixed (external NTP)2026-06-19Hyper-V checkpoints deleted; auto-checkpoints disabled2026-06-19User password policy deadlock encountered and worked around2026-06-24RPC port 135 + dynamic range identified as missing2026-06-24Firewall rule updated to include RPC ports2026-06-24Domain joins and logins succeed on both physical PC and VM


Recommendations for Portfolio Documentation

This troubleshooting saga demonstrates:


Systematic Debugging: Eliminated false leads methodically, didn't jump to conclusions
RPC Dependency: Active Directory operations depend on RPC — this is often overlooked
Hyper-V + Domain Controllers: Checkpoints and time sync are incompatible with domain trust
Firewall Precision: Individual AD services can appear healthy while RPC fails silently
Policy Deadlocks: User password policies can create "must change" + "can't change" contradictions
