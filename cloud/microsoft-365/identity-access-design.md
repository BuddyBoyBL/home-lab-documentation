# Cyber Frontier – Microsoft 365 Identity & Access Design

## Overview
This document defines the identity, access control, and role-based security model for the Cyber Frontier Microsoft 365 environment.

The goal of this design is to simulate a realistic enterprise environment using:
- Role-Based Access Control (RBAC)
- Least Privilege principles
- Segmented user roles
- Real-world administrative hierarchy

This environment is used for:
- Help desk simulation
- Security incident practice
- Identity and access management training

---

## Company Information

- **Company Name:** Cyber Frontier  
- **Location:** Seattle, Washington  
- **Domain:** cyberfrontier.com  

---

## Identity Structure

### User Categories

| Category        | Count | Naming Scheme |
|----------------|------|--------------|
| Executive      | 1    | Devil Fruit Names |
| IT Admins      | 2    | Candy Names |
| General Users  | 5    | Fruit Names |
| Contractors    | 2    | Vegetable Names |

---

## Role-Based Access Model

### Tier Structure

| Tier | Role Type          | Description |
|------|------------------|------------|
| Tier 0 | Senior Admins     | Full tenant control |
| Tier 1 | Security Admin    | Monitoring and response |
| Tier 2 | General Users     | Standard business access |
| Tier 3 | Contractors       | Restricted, scoped access |

---

## Administrative Roles

### Senior Administrators
- **Users:** Alicia Caramel, Lab Admin (Self)
- **Roles:**
  - Global Administrator
- **Access Level:**
  - Full control over tenant
  - Identity, licensing, and policy management

**Security Note:**
Admin accounts should be separated from daily-use accounts.

---

### Cybersecurity Lead

- **User:** Brian Skittles  
- **Roles:**
  - Security Administrator
  - Conditional Access Administrator
  - Security Reader  

**Restricted From:**
- Global Administrator
- Billing Administrator
- Privileged Role Administrator
- Full User Administration

**Purpose:**
- Monitor threats
- Manage security policies
- Respond to incidents

---

### IT Contractor

- **User:** Sophia Broccoli  
- **Roles:**
  - Helpdesk Administrator (Scoped)

**Permissions:**
- Reset passwords for:
  - General Users
  - Contractors  

**Restricted From:**
- Admin accounts
- Executive account

**Additional Access:**
- Printer management (local / simulated)

---

## General Users

Standard business users assigned based on department:

| User            | Department        |
|----------------|------------------|
| Daniel Apple    | Sales            |
| Jessica Orange  | Marketing        |
| Kevin Banana    | Support          |
| Laura Cherry    | HR               |
| Michael Grape   | Finance          |

**Access Level:**
- Microsoft 365 applications
- Email and collaboration tools
- No administrative privileges

---

## License Assignment

| Role Type        | License Type |
|------------------|-------------|
| Executive        | Microsoft 365 E5 |
| IT Admins        | Microsoft 365 E5 |
| Finance / HR     | Microsoft 365 E5 |
| General Users    | Microsoft 365 E3 |
| Contractors      | Microsoft 365 Business Basic |

---

## Security Groups

### Role-Based Groups
- EXEC-Leadership
- IT-Admins
- IT-Security
- Finance-Team
- HR-Team
- Sales-Team
- Marketing-Team
- Support-Team
- Contractors

### Access Control Groups
- MFA-Required
- VPN-Users
- Email-Standard
- Privileged-Access

---

## Security Controls

### Multi-Factor Authentication (MFA)

**Enabled For:**
- Administrators
- Finance
- HR
- Contractors

**Optional / Simulated Gaps:**
- Some general users (for testing phishing scenarios)

---

### Conditional Access (Planned / Recommended)

- Require MFA for privileged roles
- Restrict contractor access to sensitive resources
- Block admin portal access for non-admin users

---

## Identity Lifecycle Simulation

| Phase | Timeframe | Description |
|------|----------|------------|
| Foundation | 2015–2018 | Executive + initial IT |
| Growth | 2019–2021 | Finance, HR, Business roles |
| Expansion | 2022–2024 | Support + Contractors |

---

## Security Design Principles

### 1. Least Privilege
Users are granted only the access necessary for their role.

### 2. Role Separation
Administrative responsibilities are divided to reduce risk:
- Senior Admin → Control
- Security Admin → Monitoring
- Helpdesk → Support

### 3. Controlled Escalation
Lower roles cannot elevate privileges without approval.

### 4. Attack Surface Simulation
Environment intentionally includes:
- Users without MFA
- Role boundaries
- Limited contractor access

---

## Use Cases

This lab supports simulation of:

### Help Desk Scenarios
- Password resets
- Account lockouts
- Access requests

### Security Scenarios
- Phishing attacks (CEO / Finance)
- Account compromise
- Suspicious login detection

### Administrative Scenarios
- Role escalation requests
- Conditional access enforcement
- License changes

---

## Future Improvements

- Implement Privileged Identity Management (PIM)
- Add audit logging and alerting workflows
- Simulate endpoint management (Intune)
- Expand user base to 25+ accounts
- Integrate SIEM (Splunk / Sentinel)

---

## Summary

This environment replicates a realistic Microsoft 365 tenant using structured identity design, role-based access control, and security best practices.

It is intended to provide hands-on experience with:
- Identity management
- Access control
- Security operations
- Help desk workflows
