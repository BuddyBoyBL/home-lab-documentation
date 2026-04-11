
# Asset Management & Inventory

## Overview
This directory serves as the inventory for the physical and virtual components of the home lab. Effective asset management ensures that all devices on the network are accounted for, which is the foundational step in the **NIST Cybersecurity Framework (Identify)**.

## Physical Assets
| Asset Name | Device Type | Status | Role |
| :--- | :--- | :--- | :--- |
| **Qotom Mini PC** | Networking | Pending | Edge Firewall (pfSense) |
| **Cisco 3560** | Networking | Active | L3 Core Switch |
| **Lenovo Laptop** | Server | Active | Windows Server 2022 (AD) |
| **Admin Laptop** | Workstation | Active | Management & SIEM Host |
| **Test PC** | Endpoint | Active | Windows 10 Pro Test Node |

## Virtual Assets (Hyper-V)
* **Active Directory DC**: Windows Server 2022 Domain Controller.
* **Splunk Instance**: Centralized log aggregator.
* **Kali Linux**: Penetration testing and network auditing tools.
* **Client VM**: Windows 11 workstation for policy testing.

## Asset Identification
To maintain organization, physical assets are tagged and tracked by:
* **Hostnames**: Standardized naming convention for easy identification in logs.
* **IP Assignment**: Static IPs for infrastructure; DHCP for client devices.
* **Physical Tags**: NFC-based labeling for quick access to device documentation.

---
> **Project Status**: This inventory is updated as hardware is acquired and integrated into the production environment.
