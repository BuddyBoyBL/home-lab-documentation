# Change Management Log

## Change ID
CM-2026-06-24-0300

## Date & Time
2026-06-24 | 03:00 (Local Time)

## Author
Ben

## Change Type
- [ ] Standard
- [x] Normal
- [ ] Emergency

## Affected Systems
- OPNsense Firewall (firewall rules)
- Domain Controller (WIN-MLT4OCA4COK)
- Hyper-V VM Integration Services
- Windows Time Service (w32time) on DC
- Active Directory Firewall Ports Alias (AD_PORTS)

## Description of Change

Two critical infrastructure changes were implemented to resolve persistent Active Directory domain join failures:

### Change 1: Firewall Rule Updates — Added Missing RPC Ports

Updated the `AD_PORTS` firewall alias and associated rules to include **RPC Endpoint Mapper (port 135)** and the **Dynamic RPC high-port range (49152–65535)**, which are required for Active Directory domain join, logon, and leave operations.

**Previous AD_PORTS alias:**
```
53, 88, 389, 445, 464, 3268
```

**Updated AD_PORTS alias:**
```
all
```

### Change 2: Domain Controller Time Synchronization Configuration

Reconfigured the Domain Controller to use external NTP servers (pool.ntp.org, time.windows.com) instead of relying on **Hyper-V Integration Services time synchronization**, which was causing clock drift and "not synchronized" warnings.

## Reason for Change

### Root Cause of Domain Join Failures

After systematic troubleshooting spanning two weeks, domain authentication was failing with:
```
The security database on the server does not have a computer account 
for this workstation trust relationship.
```

This error persisted despite:
- ✓ DC being healthy (dcdiag passing all tests)
- ✓ Network path working (DNS resolving correctly)
- ✓ Client secure channel testing as healthy
- ✓ Kerberos and LDAP ports (88, 389) being reachable
- ✓ Individual AD services appearing functional

**Root Cause Identified:** Active Directory domain operations (join, login, leave) depend on **Remote Procedure Call (RPC)** for machine account password negotiation and Kerberos completion. The firewall was blocking:

1. **Port 135** (RPC Endpoint Mapper) — tells clients where to connect for RPC services
2. **Ports 49152–65535** (Dynamic RPC range) — carries the actual RPC conversations

RPC is the **invisible critical dependency** that no single port test reveals. Port 88 could succeed, but RPC negotiations still fail on the high-port dynamic range.

**Secondary Issue:** DC clock was flagged "not synchronized" because **Hyper-V Integration Services** was overriding w32time, preventing the DC from syncing to a reliable external source. This caused intermittent Kerberos failures when clock drift exceeded 5 minutes.

## Pre-Change State

### Firewall Configuration
```
OPNsense Rule: TEST_NET → 192.168.60.20
  Ports: 53, 88, 389, 445, 464, 3268 (AD_PORTS alias)
  Missing: Port 135, ports 49152-65535
  
Result: Domain joins failed with "RPC server unavailable"
        Secure channel repairs failed
        User logins rejected with trust error
```

### Domain Controller Time Sync
```
w32tm /query /status output:
  Leap Indicator: 3 (not synchronized)
  Source: VM IC Time Synchronization Provider
  Last Successful Sync: [stale, hours old]
  
Result: Clock drifting, intermittent Kerberos failures
        DC unable to maintain time authority
```

## Implementation Steps

### Step 1: Update Firewall Rules in OPNsense

1. Navigated to **Firewall → Aliases → Ports**
2. Located `AD_PORTS` alias
3. Added port 135 and port range 49152-65535 to existing ports
4. Updated alias to:
   ```
   53, 88, 135, 389, 445, 464, 3268, 49152-65535
   ```
5. Saved alias configuration
6. Navigated to **Firewall → Rules** for each affected interface (TEST, servers, etc.)
7. Verified rules referenced the updated AD_PORTS alias
8. Clicked **Apply** to activate changes
9. Validated firewall rules were active in the status pane

### Step 2: Fix Domain Controller Time Synchronization

**On the DC VM (via Hyper-V host):**

1. Opened **Hyper-V Manager** on host BLUE2
2. Right-clicked **AD** VM → **Settings**
3. Navigated to **Management → Integration Services**
4. **Unchecked "Time synchronization"** to stop Hyper-V override

**On the Domain Controller (PowerShell as Administrator):**

1. Configured w32time to use external NTP sources:
   ```powershell
   w32tm /config /manualpeerlist:"time.windows.com,0x8 pool.ntp.org,0x8" /syncfromflags:manual /reliable:yes /update
   ```

2. Restarted w32time service:
   ```powershell
   Restart-Service w32time
   ```

3. Forced resync:
   ```powershell
   w32tm /resync /rediscover
   ```

4. Verified time sync status:
   ```powershell
   w32tm /query /status
   Get-Date
   ```

**Expected result after changes:**
```
Leap Indicator: 0 (no warning)
Source: pool.ntp.org,0x8 (external NTP, not Hyper-V)
Last Successful Sync Time: [current, within last minute]
```

### Step 3: Clean VM Rejoin

After firewall and time fixes were applied:

1. Deleted existing computer objects from AD
2. On client VM: Removed from domain → workgroup
3. On client VM: Rejoined domain with clean `Add-Computer` command
4. Verified domain join succeeded

## Post-Change State

### Firewall Configuration
```
OPNsense Rule: TEST_NET → 192.168.60.20
  Ports: 53, 88, 135, 389, 445, 464, 3268, 49152-65535 (updated AD_PORTS)
  
Result: Domain joins succeed
        User logins succeed
        RPC conversations complete successfully
```

### Domain Controller Time Sync
```
w32tm /query /status output:
  Leap Indicator: 0 (no warning)
  Source: pool.ntp.org,0x8 (external NTP)
  Last Successful Sync Time: 2026-06-24 02:32:07 AM (current)
  Stratum: 3 (secondary reference - syncd by NTP)
  
Result: Clock stable and synchronized to external authority
        DC is authoritative time source for domain
```

## Validation / Testing

### Test 1: RPC Connectivity (Port 135)
```powershell
Test-NetConnection 192.168.60.20 -Port 135
# Result: TcpTestSucceeded: True
```

### Test 2: Full RPC Conversation
```powershell
Get-WmiObject -ComputerName 192.168.60.20 -Class Win32_ComputerSystem -Credential homelab\Administrator
# Result: Returns system information (RPC working end-to-end)
```

### Test 3: Domain Join Success
```powershell
# On client after firewall change:
Add-Computer -DomainName "homelab.local" -Credential homelab\Administrator -Restart
# Result: Computer successfully joined domain
```

### Test 4: Domain Login Success
```
Login Screen:
  Username: homelab\will.smith
  Password: [credentials]
  
Result: User successfully logged in to domain
```

### Test 5: Time Sync Verification
```powershell
# On DC:
w32tm /query /status
# Leap Indicator: 0 (no warning) ✓
# Source: pool.ntp.org,0x8 ✓
```

### Test 6: Secure Channel Health
```powershell
# On client:
Test-ComputerSecureChannel -Verbose
# Result: True
# "The secure channel between the local computer and the domain homelab.local is in good condition."
```

## Impact Assessment

### Expected Impact
- Firewall: Unblock RPC-dependent AD operations (domain join, login, leave)
- Time Sync: Stabilize DC clock and eliminate Kerberos time-based rejections

### Actual Impact
- **Domain join now succeeds** on both physical PC and VM
- **User login succeeds** with valid domain credentials
- **No more trust relationship errors** after clean rejoin
- **No more RPC server unavailable messages**
- **DC clock stable** at Leap Indicator 0 (no warning)
- **Domain authentication reliable** across power cycles

### Downtime
- Firewall rule updates: No downtime (rule application is immediate)
- Time sync fix: Brief (only during w32time service restart on DC, ~10 seconds)

## Affected Users/Systems

| System | Impact |
|--------|--------|
| DESKTOP-70JQB6F (VM) | Domain login now works |
| DESKTOP-SNB7KKD (Physical PC) | Domain login now works |
| WIN-MLT4OCA4COK (DC) | Time now synchronized; no longer "not synchronized" |
| All domain users | Can now authenticate successfully |
| Firewall operations | AD-dependent traffic no longer blocked |

## Rollback Plan

### Rollback for Firewall Changes

If domain operations break after this change:

1. In OPNsense: **Firewall → Aliases → Ports**
2. Edit `AD_PORTS` alias to remove ports 135 and 49152-65535
3. Revert to original: `53, 88, 389, 445, 464, 3268`
4. Click **Apply**
5. Clear DNS cache on clients: `ipconfig /flushdns`
6. Attempt domain join again

**Time to rollback:** ~2 minutes

### Rollback for Time Sync Changes

If DC clock becomes problematic after this change:

1. On DC: Revert to Hyper-V time sync:
   ```powershell
   w32tm /config /syncfromflags:domhierarchy /update
   Restart-Service w32time
   ```

2. In Hyper-V Manager: Re-enable "Time synchronization" on AD VM
3. Monitor `w32tm /query /status` to verify behavior

**Time to rollback:** ~5 minutes

## Security Considerations

### Risk Assessment for Firewall Changes

**Risk Level:** Low
- RPC ports (135, 49152–65535) are required for AD to function
- These ports are only exposed to authorized AD clients (constrained by firewall rules)
- No new external exposure (still restricted to TEST VLAN → DC only)

**Mitigation:**
- RPC traffic is encrypted when used for AD operations (Kerberos + SMB signing)
- Monitor RPC connections in Splunk for anomalous activity
- Rules still restrict to specific DC IP (192.168.60.20), not entire subnets

### Risk Assessment for Time Sync Changes

**Risk Level:** Low
- External NTP sources (pool.ntp.org, time.windows.com) are industry standard
- DC has local gateway (192.168.60.1) for internet path
- Disabling Hyper-V time sync on DC is a best practice (avoids hypervisor/OS conflicts)

**Mitigation:**
- Monitor w32tm status regularly (`w32tm /query /status`)
- Verify DC remains Leap Indicator 0
- Set alerts if DC clock drifts >5 minutes from system time

## Related Documentation

- [Troubleshooting AD Domain Join — Complete Saga](../troubleshooting/troubleshooting-ad-domain-join-complete-saga.md)
- [OPNsense Firewall Rules and Policies](../firewall/opnsense-firewall-rules.md)
- [Active Directory — Current State](../active-directory/ad-current-state.md)

## Testing Evidence

**RPC Port Open and Responding:**
```
Test-NetConnection 192.168.60.20 -Port 135
ComputerName     : 192.168.60.20
RemoteAddress    : 192.168.60.20
RemotePort       : 135
InterfaceAlias   : Ethernet
SourceAddress    : 192.168.20.40
TcpTestSucceeded : True
```

**Domain Join Success After Change:**
```
PS C:\Windows\system32> Add-Computer -DomainName "homelab.local" -Credential homelab\Administrator -Restart

[Successfully rejoined domain; system restarted]

PS C:\Windows\system32> Test-ComputerSecureChannel -Verbose
VERBOSE: The secure channel between the local computer and the domain homelab.local is in good condition.
```

**DC Time Synchronized After Change:**
```
PS C:\Users\Administrator> w32tm /query /status
Leap Indicator: 0(no warning)
Stratum: 3 (secondary reference - syncd by (S)NTP)
ReferenceId: 0x450AD0AA (source IP:  69.10.208.170)
Last Successful Sync Time: 6/24/2026 2:32:07 AM
Source: pool.ntp.org,0x8
Poll Interval: 6 (64s)
```

## Change Status

- [x] Completed
- [ ] Pending
- [ ] Failed
- [ ] Rolled Back

## Verification Sign-Off

| Item | Status |
|------|--------|
| Firewall rules updated | ✓ Completed |
| Rules applied in OPNsense | ✓ Verified |
| DC time sync configured | ✓ Completed |
| DC time synchronized | ✓ Verified (Leap Indicator 0) |
| VM domain join successful | ✓ Verified |
| Physical PC domain login successful | ✓ Verified |
| User login successful | ✓ Verified |
| RPC connectivity tested | ✓ Verified |
| Secure channel healthy | ✓ Verified |

## Notes

This change represents the resolution of a **two-week troubleshooting effort** to enable Active Directory domain join. While multiple issues were discovered and fixed (switch VLAN trunking, firewall rule application, DNS cache flushing, DC clock synchronization, Hyper-V checkpoints), the **fundamental blocker was RPC ports 135 and 49152–65535 being missing from the firewall rules.**

Key Lesson: Active Directory operations depend on RPC for machine-level authentication. While individual AD services (Kerberos, LDAP, DNS) are testable and visible, RPC failures are often silent because:
- `Test-ComputerSecureChannel` (NTLM-based) passes without RPC
- Kerberos port tests pass even if RPC negotiation fails
- "RPC server unavailable" is often the only symptom, but it appears late in the join process

**Recommendation for future labs:** Always include RPC ports (135 + 49152–65535) in firewall rules that allow AD operations. Test RPC explicitly with:
```powershell
Get-WmiObject -ComputerName <DC-IP> -Class Win32_ComputerSystem -Credential domain\admin
```

This change also enforces the **operational best practice** that domain controllers should never be time-synced by hypervisors. DCs must be authoritative time sources, controlled by external reliable sources (NTP pools), not by the host OS.

## Revision History

| Date | Author | Change |
|------|--------|--------|
| 2026-06-24 | Ben | Initial documentation of firewall RPC ports and DC time sync fixes |

---

**Change Approved:** ✓ Completed and Verified  
**Date Approved:** 2026-06-24  
**Approved By:** Self (lab environment)
