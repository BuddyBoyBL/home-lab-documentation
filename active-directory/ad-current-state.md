# Active Directory – Current State (Homelab)

## File Name

`ad-current-state.md`

---

## Overview

This document captures the **current operational state** of the Active Directory environment in the homelab. It reflects completed configuration for the Domain Controller, organizational structure, networking, and initial Group Policy.

---

## Environment Summary

### Platform

* Hypervisor: Hyper-V
* Domain Controller: Windows Server 2022 (Desktop Experience)
* Domain: `homelab.local`

### Endpoints

* Physical Test PC (Windows 10 Pro)
* Additional VMs planned (Windows 11, Kali Linux)

---

## Domain Controller Configuration

* Hostname: `HL-DC01`
* Role: Domain Controller + DNS Server
* Domain: `homelab.local`
* Login format:

  * `homelab\Administrator`

### Networking

* Connected via **External Virtual Switch** (Wi-Fi)
* Static IP assigned (example):

  * `192.168.1.50`
* DNS configured to:

  * `192.168.1.50` (self)

---

## Active Directory Structure

### Organizational Units (OUs)

```plaintext
homelab.local
│
├── Users
├── Workstations
├── Admins
├── Groups
```

---

### Users

* ~25 test user accounts created
* Stored in:

  * `Users OU`

---

### Admin Accounts

* Admin-specific users stored in:

  * `Admins OU`

---

### Security Groups

Stored in:

* `Groups OU`

Groups created:

* `IT_Admins`
* `Remote_Users`
* `Contractors`

---

## Group Policy

### GPO: Workstation Security Policy

* Linked to: `Workstations OU`

#### Configurations:

* Screen lock timeout: 5 minutes
* Password required on resume

---

## Client Integration (Current State)

### Test PC (Physical)

#### Network Configuration

* Connected to home Wi-Fi
* DNS manually set to:

  * `192.168.1.50` (Domain Controller)

#### Connectivity Validation

```bash
ping 192.168.1.50
nslookup homelab.local
```

Expected:

* Successful ping response
* Domain resolves to DC

---

## Domain Join Status

* Test PC is prepared for domain join
* Domain join process:

  * `homelab.local`
  * Auth via `homelab\Administrator`

Post-join:

* Computer object will be moved to:

  * `Workstations OU`

---

## Key Concepts Implemented

* Active Directory Domain Services (AD DS)
* DNS-based domain resolution
* OU-based organization
* Security group-based access control
* GPO enforcement for endpoint behavior

---

## Known Working Configuration

* Domain Controller operational
* DNS resolving domain correctly
* External switch allows VM ↔ physical device communication
* GPO linked and ready for enforcement
* AD structure organized for scalability

---

## Known Limitations (Current Stage)

* No VLAN segmentation yet
* No firewall rules (pfSense pending)
* No centralized logging (Splunk pending)
* Minimal GPOs implemented

---

## Next Steps

* Complete domain join for physical test PC
* Add additional client systems (VM + physical)
* Implement password and account lockout policies
* Begin log collection and analysis (Splunk)
* Introduce network segmentation using firewall and VLANs
* Integrate Kali Linux for security testing scenarios

---

## Notes

* DNS is critical for all domain operations
* External switch required for physical client communication
* OUs are used for organization, GPO targeting, and scaling
* Groups are used strictly for permission management

---
