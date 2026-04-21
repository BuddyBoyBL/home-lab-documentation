# Storage Overview

## Summary
This section documents the storage design, validation processes, and data management strategy for the home lab.

The goal is to simulate real-world storage practices while balancing cost, reliability, and learning value.

---

## Objectives

- Validate all storage devices before use
- Separate storage by purpose (logs, backups, testing)
- Implement safe handling of degraded hardware
- Support SIEM (Splunk) log storage and retention
- Build a scalable and well-documented storage model

---

## Current Storage Components

### Primary Systems
- **Admin Laptop (ThinkPad)**
  - ~1.5TB internal storage
  - Hosts virtual machines (AD, Splunk, client systems)

- **Test PC**
  - Used for hardware testing and storage validation
  - Will serve as a future internal storage test platform

- **Firewall Mini PC**
  - ~250GB SSD
  - Reserved for OS and minimal logging only

---

### Bulk Storage
- **4TB HDD (New)**
  - Intended for:
    - Log storage (Splunk warm/cold data)
    - General lab storage

- **Used 1TB HDD (WD10EZEX)**
  - Status: **Degrading (SMART caution)**
  - Assigned role:
    - Non-critical storage
    - Testing and failure simulation

---

### External Storage
- Multiple **2TB external drives**
- Used for:
  - Backup copies
  - Redundant storage of important data

---

## Storage Strategy

### Data Classification

| Data Type        | Storage Location         | Priority |
|------------------|--------------------------|----------|
| Active Logs      | Splunk VM / 4TB HDD      | High     |
| Archived Logs    | 4TB HDD                  | Medium   |
| Backups          | External Drives          | Critical |
| Test Data        | Used/Degraded Drives     | Low      |

---

### Key Principles

- **Do not trust a single drive**
- **Separate critical and non-critical data**
- **Validate all used drives before deployment**
- **Monitor SMART data over time**
- **Use degraded drives only for controlled scenarios**

---

## Drive Validation Process

All drives must go through:

1. SMART check (CrystalDiskInfo)
2. Surface scan (CHKDSK or HDDScan)
3. Optional vendor diagnostics (SeaTools, WD Tools)
4. Classification:
   - Healthy
   - Degrading
   - Failed

Reference:
- `troubleshooting/used-drive-validation-and-health-check.md`

---

## Current Limitations

- No RAID or redundancy at the hardware level
- Reliance on manual backup strategy
- Some drives show early signs of failure

---

## Future Improvements

- Add RAID or mirrored storage
- Introduce automated backup system
- Implement SMART monitoring alerts
- Expand storage segmentation by VLAN or system role
- Evaluate NAS or dedicated storage node

---

## Project Relevance

This storage design supports:

- SIEM logging (Splunk)
- Network monitoring
- Lab testing environments
- Real-world failure simulation

It reflects practical trade-offs between cost, reliability, and learning objectives.

---
