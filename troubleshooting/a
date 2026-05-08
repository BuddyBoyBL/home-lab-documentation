# DHCP Conflict Troubleshooting – OPNsense + Cisco SG300

## Overview
This document outlines the troubleshooting process used to diagnose and resolve DHCP failures across multiple VLANs in a homelab environment using OPNsense and a Cisco SG300-52 managed switch.

The root cause was ultimately identified as a legacy `Dnsmasq DNS & DHCP` service still running alongside Kea DHCP on OPNsense, causing DHCP socket conflicts.

---

# Environment

## Firewall
- OPNsense
- Kea DHCP enabled
- Multiple VLAN interfaces configured

## Switch
- Cisco SG300-52 Managed Switch

## VLAN Layout

| VLAN | Purpose | Subnet |
|---|---|---|
| 20 | Test | 192.168.20.0/24 |
| 30 | Home | 192.168.30.0/24 |
| 40 | IoT | 192.168.40.0/24 |
| 50 | Sandbox | 192.168.50.0/24 |
| 60 | Servers | 192.168.60.0/24 |
| 99 | Management | 192.168.99.0/24 |

---

# Initial Symptoms

## DHCP Failure
Devices connected through the switch:
- Failed to obtain DHCP leases
- Received APIPA addresses (`169.254.x.x`)
- Had no default gateway assigned

## Static IP Behavior
When static IPs were manually configured:
- Devices could communicate
- Servers were reachable
- VLAN routing partially functioned

## Direct Firewall Connection
When directly connected to the firewall:
- DHCP worked normally
- Proper gateway assigned
- Correct subnet assigned

---

# Troubleshooting Process

## 1. Verified Client Configuration

Commands used:

```cmd
ipconfig
ipconfig /release
ipconfig /renew
```

Observed:
- APIPA addresses
- DHCP renew failures
- Missing default gateway

---

## 2. Verified Switch Layer 2 Functionality

Checked:
- MAC address tables
- VLAN membership tables
- Trunk ports
- Tagged/Untagged assignments
- PVID assignments

Findings:
- MAC addresses learned correctly
- Devices appeared on expected VLANs
- Trunk ports showed tagged VLAN membership

Conclusion:
- Physical connectivity and Layer 2 switching were functioning.

---

## 3. Tested Multiple Ports

Verified:
- Multiple switch access ports
- Different VLAN assignments
- Direct firewall connectivity

Result:
- DHCP consistently failed through switch
- Static addressing still worked

---

## 4. Rebooted Infrastructure

Performed:
- Switch reboot
- Firewall service restarts

Result:
- Problem persisted

---

## 5. Investigated Potential Causes

Considered:
- VLAN mismatch
- Trunk misconfiguration
- PVID errors
- WAN/LAN cable swap
- DHCP relay issues
- MAC table corruption
- Rogue DHCP server
- OPNsense parent interface issues

---

# Critical Discovery

## DHCP Logs

Reviewed:
- `Services > Kea DHCP > Log File`

Observed repeated errors:

```text
DHCPSRV_OPEN_SOCKET_FAIL
Address already in use - is another DHCP server running?
```

Additional warnings:

```text
Failed to open socket on interface
```

---

# Root Cause

## Legacy Dnsmasq DHCP Service Conflict

A legacy `Dnsmasq DNS & DHCP` service was still enabled from the original OPNsense setup.

This conflicted with:
- Kea DHCP
- VLAN interface bindings
- DHCP socket assignments

As a result:
- Kea DHCP could not bind to interfaces
- DHCP broadcasts failed
- Clients received APIPA addresses

---

# Resolution

## Disabled Legacy DHCP Service

Disabled:
- `Dnsmasq DNS & DHCP`

After disabling:
- Kea DHCP successfully bound to VLAN interfaces
- DHCP leases began working
- VLAN clients received correct IP addresses
- Wi-Fi hotspot functionality returned

---

# Key Lessons Learned

## Check Logs Earlier
The logs identified the actual problem much faster than topology troubleshooting.

---

## APIPA Addresses Matter
`169.254.x.x` addresses strongly indicate:
- DHCP failure
- Not necessarily switching failure

---

## Static IP Success Is Important Evidence
If static IPs work:
- Layer 2 is probably functional
- Routing may be functional
- DHCP becomes primary suspect

---

## MAC Learning Helps Isolate Problems
If MAC addresses appear in switch tables:
- The switch sees the device
- Physical connectivity exists
- Traffic is reaching the switch

---

## DHCP Conflicts Can Mimic VLAN Problems
A DHCP service conflict can appear like:
- broken VLANs
- trunking issues
- routing problems
- gateway failures
- switch misconfiguration

even when VLAN configuration is correct.

---

# Final Diagnosis

## Actual Root Cause
Conflicting DHCP services on OPNsense prevented Kea DHCP from binding to VLAN interfaces.

## Specifically
Legacy `Dnsmasq DNS & DHCP` remained enabled alongside Kea DHCP.

---

# Outcome

After removing the DHCP conflict:
- VLAN DHCP worked correctly
- Management VLAN connectivity functioned
- Wireless connectivity returned
- Dynamic IP assignment stabilized
- Trunked VLAN communication functioned as expected
