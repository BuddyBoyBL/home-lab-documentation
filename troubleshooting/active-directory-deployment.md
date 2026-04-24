# Active Directory Deployment & Domain Join Troubleshooting

## File Name

`active-directory-deployment.md`

---

## Overview

This document outlines the deployment of an Active Directory Domain Controller using Windows Server 2022 and the process of joining a physical Windows client to the domain. It also includes troubleshooting steps encountered during setup.

---

## Environment

### Infrastructure

* Host Machine (Admin Laptop)
* Hypervisor: Hyper-V
* Domain Controller: Windows Server 2022 VM
* Client Machine: Windows 10 Pro (Physical PC)

### Network Setup

* VM connected via External Virtual Switch (Wi-Fi adapter)
* Test PC connected via home Wi-Fi network

---

## Domain Controller Setup

### Installation

* Installed Windows Server 2022 (Desktop Experience)
* Promoted server to Domain Controller

### Domain Configuration

* Domain Name: `homelab.local`
* Forest Level: Default
* DNS Role: Installed automatically with AD DS

### Post-Deployment Configuration

* Logged in using:

  * `homelab\Administrator`
* Verified AD tools:

  * Active Directory Users and Computers
  * Group Policy Management

---

## Network Configuration

### Domain Controller

* Assigned Static IP:

  * Example: `192.168.1.50`
* DNS configured to:

  * `192.168.1.50` (self)

### Hyper-V Configuration

* Created External Virtual Switch using Wi-Fi adapter
* Attached Domain Controller VM to External Switch

---

## Organizational Structure

### OUs Created

* `Users`
* `Workstations`
* `Admins`
* `Groups`

### Groups Created

* `IT_Admins`
* `Remote_Users`
* `Contractors`

### Users

* Created ~25 test users
* Assigned users to appropriate groups

---

## Group Policy Configuration

### GPO: Workstation Security Policy

Applied to: `Workstations OU`

#### Settings:

* Screen lock after 5 minutes
* Require password on resume

---

## Domain Join Process (Client PC)

### Step 1: Configure DNS

On Windows 10 client:

* Set Preferred DNS:

  * `192.168.1.50`

---

### Step 2: Connectivity Testing

```bash
ping 192.168.1.50
nslookup homelab.local
```

Expected:

* Successful ping response
* Domain resolves to DC IP

---

### Step 3: Join Domain

* Navigate to:

  * System → About → Rename this PC (Advanced)
* Select:

  * Domain: `homelab.local`

---

### Step 4: Authentication

* Username:

  * `homelab\Administrator`
* Restart system after successful join

---

### Step 5: Verification

* Login using domain account
* Verify computer appears in Active Directory
* Move computer to `Workstations OU`

---

## Troubleshooting

### Issue 1: Boot Loader Error

**Error:**

* “Boot image not found”

**Cause:**

* ISO not properly attached or incorrect boot order

**Resolution:**

* Reattach ISO
* Set DVD as first boot device

---

### Issue 2: Media Disconnected

**Cause:**

* VM not connected to a virtual switch

**Resolution:**

* Attach VM to External or Internal switch

---

### Issue 3: Domain Join Failure

**Cause:**

* DNS not pointing to Domain Controller

**Resolution:**

* Set client DNS to DC IP

---

### Issue 4: Unable to Login After Promotion

**Cause:**

* Using local account instead of domain account

**Resolution:**

* Login using:

  * `homelab\Administrator`

---

### Issue 5: DNS Delegation Warning

**Message:**

* “Delegation cannot be created”

**Cause:**

* No parent DNS zone in lab environment

**Resolution:**

* Safe to ignore

---

### Issue 6: VM Not Reachable from Physical PC

**Cause:**

* VM using Internal Switch

**Resolution:**

* Create External Switch and bind to Wi-Fi adapter

---

## Key Takeaways

* DNS is critical for Active Directory functionality
* External switch is required for physical-to-VM communication
* OUs organize objects, Groups control permissions, GPOs enforce policy
* Always validate connectivity before attempting domain join

---

## Next Steps

* Integrate Splunk for log ingestion
* Create additional GPOs (password policy, lockout policy)
* Implement VLAN segmentation using firewall (pfSense)
* Simulate attack scenarios using Kali Linux

---
