# Change Management Log

## Change ID
CM-2026-04-11-0345

## Date & Time
2026-04-11 | 03:45 (Local Time)

## Author
Ben

## Change Type
- [x] Standard
- [ ] Normal
- [ ] Emergency

## Affected Systems
- QNAP NAS Device
- Server VLAN (192.168.60.0/24)
- Storage / Backup Planning Layer

## Description of Change
The QNAP NAS device has been removed from the homelab environment and is being returned due to persistent non-functionality.

Extensive troubleshooting confirmed that the device does not initialize network communication and does not respond to reset procedures or DHCP discovery. Based on diagnostic results, the system is considered non-operational and unsuitable for integration into the lab.

## Reason for Change
- Device failed to establish network connectivity across all tested configurations
- No DHCP, ARP, or Layer 3 response detected
- Reset and recovery procedures did not resolve issue
- Hardware or firmware-level failure suspected
- Replacement or return determined to be the most effective resolution

## Pre-Change State
- QNAP NAS was designated as planned storage/backup infrastructure in SERVERS VLAN
- Intended role included:
  - file storage
  - backup repository
  - potential logging storage expansion

## Implementation Steps
1. Performed full multi-layer troubleshooting (Layer 1–Layer 3)
2. Tested multiple network configurations and subnets
3. Attempted DHCP and static IP assignment
4. Verified physical link integrity across multiple ports and cables
5. Tested reset procedures (short and extended)
6. Confirmed failure across multiple client devices
7. Validated symptoms against known hardware failure cases
8. Removed device from active network planning scope

## Post-Change State
- QNAP NAS is no longer part of homelab infrastructure
- SERVERS VLAN storage role is now unassigned
- Backup and storage planning remains open for replacement solution

## Validation / Testing
- Confirmed device non-responsiveness across all network layers
- Verified inability to obtain IP via DHCP
- Verified no ARP or broadcast presence on network
- Confirmed reset procedure non-functional

## Impact Assessment
- Expected Impact: Removal of unreliable storage component
- Actual Impact: Reduction in planned storage capacity; no disruption to active services
- Downtime: None (device was never fully integrated)

## Rollback Plan
- If required, device could be reintroduced after repair or firmware recovery (not recommended based on current findings)

## Status
- [x] Completed (Removed from environment)
- [ ] Pending
- [ ] Failed
- [ ] Rolled Back

## Notes
This change removes a non-functional storage component from the lab architecture. While this reduces planned storage capacity, it improves overall system reliability and avoids introducing unstable hardware into the SERVERS VLAN.

A replacement NAS or alternative storage solution (local VM storage, external server, or future hardware purchase) may be evaluated in a later phase of the lab build.

## Related Documentation
- Troubleshooting Report: QNAP NAS Not Detectable on Network
