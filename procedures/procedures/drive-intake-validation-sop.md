# Drive Intake & Validation SOP

## Purpose
Establish a consistent process for validating all new or used storage drives before integrating them into the homelab environment. This ensures reliability, reduces risk of data loss, and standardizes storage decision-making.

---

## Scope
Applies to:
- HDDs and SSDs (internal and external)
- New, used, or repurposed drives
- All storage intended for lab, logging, or backup use

---

## Required Tools
- CrystalDiskInfo (SMART monitoring)
- Windows Disk Management
- Command Prompt (Administrator)

Optional:
- HDDScan (surface testing over USB)
- Vendor tools (SeaTools, WD Utilities via SATA)

---

## Procedure

### Step 1 — Physical Connection & Detection

**Objective:** Confirm the system recognizes the drive.

**Actions:**
- Connect drive via:
  - USB adapter (initial)
  - Direct SATA (preferred for full diagnostics)
- Open Disk Management and verify the drive appears

**Outcomes:**
- Detected → proceed  
- Not detected → troubleshoot:
  - Power supply
  - Cable/adapter
  - Port or system compatibility

---

### Step 2 — Disk Initialization & Preparation

**Objective:** Create a clean baseline.

**Actions:**
1. If prompted → Initialize Disk  
   - Select: GPT
2. If previously used:
