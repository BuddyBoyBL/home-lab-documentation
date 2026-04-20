# Router Overview

## Purpose

This section documents the router strategy for the home lab environment.

The router setup is being designed to support:

- wireless segmentation
- guest device isolation
- IoT separation
- troubleshooting and fallback connectivity
- a cleaner separation between wireless access and firewall enforcement

This documentation will be updated as the switch and OPNsense firewall are fully deployed.

---

## Current Router Plan

The home lab uses two routers with different roles.

### 1. TP-Link Archer A54
**Primary role:** multi-SSID wireless device for segmented networks

Planned use:
- guest wireless
- IoT wireless
- possible lab wireless later if needed

Current status:
- firmware updated
- strong admin password configured
- remote management disabled
- base hardened configuration saved
- SSID naming cleaned up to avoid exposing router details

Notes:
- admin access is intended to stay wired
- the main test PC is also intended to stay physically connected
- final VLAN mapping will be completed later when the Layer 3 switch and OPNsense firewall are in place

---

### 2. Secondary Router
**Primary role:** troubleshooting, backup connectivity, and possible IoT/testing use

Planned use:
- troubleshooting network issues
- temporary isolated network access
- testing device behavior outside the primary segmented setup
- possible containment zone for less trusted devices

Notes:
- this device is not intended to become the main long-term control point
- it should remain separate in purpose from the primary segmented wireless device

---

## Design Philosophy

The long-term network design is intended to shift control away from consumer router security features and toward a more structured model:

- **OPNsense firewall** handles policy enforcement
- **Layer 3 switch** handles VLAN structure and traffic paths
- **routers/access points** provide wireless access for assigned segments

This means the routers are not the final security core of the environment.  
They are supporting devices within a larger segmented design.

---

## Planned Network Roles

The following network structure is being considered:

- **admin**  
  wired only for stronger control and lower wireless exposure

- **lab**  
  systems used for testing, monitoring, and security tooling

- **guest**  
  devices that should have internet access but no visibility into internal systems

- **iot**  
  less trusted smart devices and similar equipment

- **isolated testing**  
  a future high-restriction segment for malware analysis or risky experiments

---

## Security Decisions Made So Far

The following hardening steps have already been completed on the TP-Link router:

- updated firmware from the official vendor source
- set a strong admin password
- disabled remote access
- removed model-identifying naming from SSIDs
- saved hardened base configuration for later deployment

Additional design choices:

- keep admin systems wired where possible
- keep the main test PC wired where possible
- reduce unnecessary wireless exposure for sensitive systems

---

## What Will Be Configured Later

The following items are intentionally being deferred until the Layer 3 switch and OPNsense firewall are ready:

- final VLAN IDs
- SSID-to-VLAN mapping
- DHCP design
- AP mode vs router mode final decision
- inter-network access rules
- firewall policy enforcement
- logging and monitoring integration

This prevents rework and keeps the topology aligned with the final design.

---

## Next Steps

1. Complete OPNsense firewall setup on the mini PC
2. Bring the Layer 3 switch into the final topology
3. Define VLAN IDs and subnet plan
4. Assign SSIDs to the proper VLANs
5. Decide final operational mode for each router
6. Document port assignments and network behavior
7. Add logging/monitoring visibility where relevant

---

## Status

**Current phase:** router preparation and planning

The router hardware is being hardened and documented first.  
Final segmentation and policy enforcement will be completed after the firewall and switch are active.
