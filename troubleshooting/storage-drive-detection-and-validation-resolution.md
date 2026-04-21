# Storage Drive Detection and Validation Resolution

## Overview
This entry documents the resolution of an initial drive detection issue and the validation of multiple HDDs using systematic troubleshooting. The process confirmed that the external drive reader was not the root cause and identified one healthy drive and one degraded drive.

---

## Objective
- Identify why a drive was not being recognized
- Validate functionality of the external drive reader
- Determine health status of multiple drives
- Assign appropriate roles based on drive condition

---

## Initial Issue
A hard drive was not recognized when connected via:
- External HDD reader (with power supply)
- Docking station

---

## Step 1 — Disk Management Verification

### Actions
- Opened **Disk Management**
- Observed drive listed but not initialized
- Initialized disk and allocated full capacity
- Created a new simple volume
- Formatted the drive

### Result
- Drive became accessible and usable

### Conclusion
- Issue was **not hardware failure**
- Drive required initialization before use

---

## Step 2 — External Drive Reader Validation

### Actions
- Tested a second known-working drive using:
  - Same external reader
  - Same docking station

### Result
- Second drive was detected and functioned normally

### Conclusion
- External drive reader and docking station are **fully functional**
- No adapter or connection issues present

---

## Step 3 — Drive Health Validation

### Tools Used
- CrystalDiskInfo
- CHKDSK (`/f /r`)

---

### Drive 1 — Healthy

#### Results
- CrystalDiskInfo: **Good**
- CHKDSK: Completed successfully within expected time
- No bad sectors detected

#### Classification
- **Healthy**

#### Assigned Role
- Primary storage
- Suitable for lab data and log storage

---

### Drive 2 — Degraded

#### Results
- CrystalDiskInfo: **Caution**
- SMART indicators showed:
  - Reallocated sectors
  - Pending sectors
  - Uncorrectable sectors

#### Classification
- **Degrading**

#### Assigned Role
- Not used for primary storage
- Not used for backups
- Reserved for:
  - Temporary storage
  - Testing scenarios
  - Failure simulation

---

## Root Cause Analysis

### Original Detection Issue
- Drive was not initialized
- Disk existed but was not accessible in Windows

### Confirmed Non-Issues
- External drive reader: functioning correctly
- Docking station: functioning correctly
- Cables and connections: not the cause

---

## Key Takeaways

- Drives may appear “undetected” when they are simply **uninitialized**
- Disk Management is a critical first step in storage troubleshooting
- External adapters should be validated using known-good hardware
- SMART data provides early warning of physical degradation
- Formatting prepares the disk but does not validate hardware health

---

## Final Outcome

- One drive validated as **healthy and usable**
- One drive identified as **degrading and restricted**
- External drive reader confirmed as **fully operational**
- Standard validation workflow successfully applied

---

## Next Steps

- Use healthy drive for primary storage
- Use external drives for backup strategy
- Avoid using degraded drive for critical data
- Implement RAID in the future for redundancy (not as backup)

---

## Project Impact

This process demonstrates:
- Structured troubleshooting methodology
- Hardware validation workflow
- Proper storage role assignment
- Real-world disk failure identification

This strengthens the reliability and documentation quality of the homelab environment.
