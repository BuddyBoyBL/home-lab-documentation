# Backup Notes

## Overview
This document outlines the current backup approach for the homelab environment.

The strategy follows the **3-2-1 backup principle**:
- 3 copies of data
- 2 different storage types
- 1 offsite/cloud backup

---

## Backup Locations

### Local Backups
- External HDD (2TB)
- Internal HDD (1TB via reader)
- Backup Laptop (~1.5TB available)

### Cloud / Offsite
- GitHub (configs + documentation)
- 1Password (secure storage for critical credentials/config references)

---

## What Is Backed Up

- OPNsense firewall configuration (XML)
- Cisco switch configurations (running + startup)
- GitHub repository (local clone)
- Lab state information (IPs, VLANs, hostnames)

---

## Backup Method

- Manual backups before major changes
- Files stored on:
  - External drive
  - Internal drive
  - Backup laptop (if needed)
- Repository cloned locally for redundancy

---

## Key Notes

- Backups exist on **multiple physical devices**
- One backup copy can be kept **offline**
- Sensitive information is stored securely in **1Password**
- Focus is on protecting **configurations over bulk data**

---
