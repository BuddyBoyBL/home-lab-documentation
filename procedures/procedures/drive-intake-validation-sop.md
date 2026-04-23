### File Name:

`sop-drive-intake-validation.md`

---

## Title

Drive Intake & Validation SOP

---

## Purpose

Establish a consistent process for validating all new or used storage drives before integrating them into the homelab environment.

This procedure:

* Verifies drive health and reliability
* Reduces risk of data loss
* Standardizes acceptance or rejection decisions

---

## Scope

Applies to:

* HDDs and SSDs (internal and external)
* New, used, or repurposed drives
* Storage intended for:

  * Homelab systems
  * Logging
  * Backups

Does **not** cover:

* RAID configuration
* Long-term monitoring (handled separately)

---

## Prerequisites

* Windows system with admin access
* Available SATA port or USB-to-SATA adapter
* Basic familiarity with Command Prompt

---

## Required Tools

* CrystalDiskInfo (SMART monitoring)
* Windows Disk Management
* Command Prompt (Administrator)

**Optional:**

* HDDScan (USB surface testing)
* Vendor tools:

  * SeaTools (Seagate)
  * WD Utilities (Western Digital, SATA preferred)

---

## Procedure

### Step 1 — Physical Connection & Detection

**Objective:** Confirm the system recognizes the drive

**Actions:**

* Connect drive via:

  * USB adapter (initial check)
  * Direct SATA (preferred for full diagnostics)
* Open **Disk Management**
* Verify the drive appears

**Outcomes:**

* Detected → proceed
* Not detected → troubleshoot:

  * Power supply
  * Cable/adapter
  * Port/system compatibility

---

### Step 2 — Disk Initialization & Preparation

**Objective:** Create a clean baseline

**Actions:**

* If prompted → **Initialize Disk**

  * Select: **GPT**
* If previously used:

  * Delete all partitions
  * Create a new simple volume
  * Perform a **full format** (not quick)

---

### Step 3 — SMART Health Check

**Objective:** Evaluate internal drive health

**Actions:**

* Open **CrystalDiskInfo**
* Locate the drive
* Review key attributes:

  * Reallocated Sector Count
  * Current Pending Sector Count
  * Uncorrectable Sector Count
  * Power-On Hours

**Outcomes:**

* **Good health (Blue/Good status)** → proceed
* **Caution or Bad status** → reject or downgrade usage

---

### Step 4 — Surface Scan

**Objective:** Detect bad sectors

**Actions:**

* Run:

  * HDDScan (USB-compatible)
  * OR vendor tool (preferred via SATA)

**Outcomes:**

* No bad sectors → proceed
* Bad sectors found → reject or restrict usage (non-critical only)

---

### Step 5 — File System Integrity Check

**Objective:** Validate read/write reliability

Run in **Command Prompt (Admin):**

```
chkdsk X: /f /r /x
```

Replace `X:` with the drive letter

**Flags:**

* `/f` = fix errors
* `/r` = locate bad sectors
* `/x` = force dismount

**Outcomes:**

* No errors → proceed
* Errors found and fixed → monitor closely
* Unrecoverable errors → reject

---

### Step 6 — Temperature Monitoring

**Objective:** Ensure safe operating temperatures

**Actions:**

* Monitor via:

  * CrystalDiskInfo
  * External thermometer (optional)
* Observe:

  * Idle temperature
  * Load temperature during scan

**Guidelines:**

* HDD typical safe range: ~30–50°C
* Sustained high temps → improve airflow or reconsider usage

---

### Step 7 — Load / Stress Test

**Objective:** Simulate real usage

**Actions:**

* Transfer large files (10–100 GB total)
* Perform read/write operations
* Observe:

  * Speed consistency
  * Disconnects or errors

**Outcomes:**

* Stable → proceed
* Unstable behavior → reject

---

### Step 8 — Final Classification

**Objective:** Assign role based on results

**Categories:**

* **Primary Use**

  * No errors
  * Strong SMART health
* **Secondary Use**

  * Minor issues
  * Acceptable for logs or non-critical storage
* **Reject**

  * SMART warnings
  * Bad sectors
  * Instability

---

## Validation / Verification

A drive passes if:

* SMART status = Good
* No bad sectors detected
* CHKDSK completes without critical errors
* Stable under load
* Temperatures remain within safe range

---

## Troubleshooting

**Issue:** Drive not detected

* Fix:

  * Change cable/adapter
  * Try different port
  * Test on another system

**Issue:** CHKDSK not running

* Fix:

  * Ensure correct syntax
  * Run Command Prompt as Administrator
  * Verify drive letter

**Issue:** High temperature

* Fix:

  * Improve airflow
  * Reduce load duration
  * Add cooling solution

---

## Security Considerations

* Always wipe previous data before reuse
* Do not trust used drives without full validation
* Avoid storing sensitive data on borderline drives
* Maintain backups regardless of test results

---

## Notes

* USB testing is acceptable for initial checks, but SATA provides more accurate diagnostics
* Used drives are inherently less reliable—assign roles accordingly
* Temperature spikes during scans are normal but should stabilize

---

## Revision History

| Date       | Change          | Notes                         |
| ---------- | --------------- | ----------------------------- |
| 2026-04-23 | Initial version | Standardized and expanded SOP |
