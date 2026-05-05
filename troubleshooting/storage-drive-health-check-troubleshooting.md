# Title: Storage Drive Health Check and Troubleshooting for Used WD 1TB HDD

## Overview
This document tracks the troubleshooting and health validation process for a used Western Digital 1TB HDD (WD10EZEX) that was not initially recognized and later showed signs of degradation during testing.

## Objective
- Validate functionality of a used HDD before integrating it into the homelab
- Determine whether the drive is reliable enough for log storage or general lab use
- Practice real-world disk troubleshooting and health analysis

## Initial Issue
The drive was not recognized when connected through:
- an external HDD reader with external power
- a SATA cable

## Initial Hypotheses
- insufficient or incorrect power delivery
- faulty SATA or USB interface
- drive hardware failure
- adapter compatibility issue

## Step 1 — Hardware Connection Testing

### Actions Taken
- Connected the drive using an external HDD reader with USB and external power
- Connected the drive using a direct SATA cable

### Result
The drive eventually became detectable through the external adapter.

### Conclusion
The issue was likely related to:
- connection inconsistency
- adapter behavior

At this stage, the drive did not appear to be completely dead.

## Step 2 — SMART Health Check

### Tool Used
- CrystalDiskInfo

### Key Findings
- Health Status: **Caution**
- Temperature: **Normal** at about **36°C**

### Critical SMART Attributes

| Attribute | Status | Meaning |
|---|---|---|
| Reallocated Sectors | Present (103) | Drive has already replaced bad sectors |
| Pending Sectors | Present (2) | Unstable sectors may fail |
| Uncorrectable Sectors | Present (26) | Data loss has already occurred |
| Reported Errors | Present | Read/write failures were detected |

### Interpretation
The drive is functional, but degrading.

There is evidence of physical media wear.

## Step 3 — Disk Preparation

### Actions Taken
Used DiskPart to:
- remove all partitions, including the recovery partition
- reinitialize the disk as GPT
- create a new simple volume

### Reasoning
- remove unknown previous configurations
- establish a clean baseline for testing

## Step 4 — CHKDSK Scan

### Command Used
```powershell
chkdsk D: /f /r /x
