
# Active Directory & Identity Management (IAM)

## Overview
This component serves as the **Centralized Control Plane** for the home lab environment. By implementing a Windows Server 2022 Domain Controller, I have transitioned the network from a collection of isolated workgroups into a managed Enterprise Domain.

## Infrastructure
* **Domain Controller**: Windows Server 2022 (Bare metal on Lenovo hardware).
* **Domain Name**: `HL-DC01` (Internal-only).
* **Primary Roles**: AD DS (Active Directory Domain Services), DNS, DHCP.
* **Clients**:
    * **Windows 11 Pro (VM)**: Administrative and testing workstation.
    * **Windows 10 Pro (Physical)**: Dedicated test PC for software deployment and log generation.

## Project Goals
1. **Centralized Authentication**: Implement a single identity source for all lab users and service accounts.
2. **Security Hardening**: Utilize Group Policy Objects (GPOs) to enforce security baselines across all domain-joined endpoints.
3. **Audit Visibility**: Configure Advanced Audit Policies to capture process creations, account logons, and PowerShell activity.
4. **Log Aggregation**: Standardize the delivery of Windows Event Logs to the Splunk SIEM via the Splunk Universal Forwarder.

## Group Policy Implementation (GPO)
The following GPOs have been enforced to maintain a high security posture:
* **Account Lockout Policy**: Prevents brute-force attacks by locking accounts after 5 failed attempts.
* **Firewall Configuration**: Centrally manages Windows Firewall rules to allow management traffic only from VLAN 10.
* **Sysmon Deployment**: Ensures the Sysmon service is running with a customized configuration file for deep endpoint visibility.

## Logging & Monitoring
To support the **SOC/SIEM** goals of this project, this Domain Controller is configured to send the following telemetry to Splunk:
* **Event ID 4624/4625**: Successful/Failed logons.
* **Event ID 4688**: Process creation (with command-line auditing enabled).
* **PowerShell Operational Logs**: Monitoring for encoded commands and suspicious script execution.

---
> **Portfolio Note**: This setup demonstrates proficiency in **Windows Server Administration** and **Identity Management**, specifically focusing on how AD serves as a telemetry source for security operations.
