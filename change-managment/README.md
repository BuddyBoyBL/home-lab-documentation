# Change Management & Governance

## Overview
This directory serves as the official **Audit Trail** for the Home Lab environment. In an enterprise setting, uncontrolled changes are a primary source of security vulnerabilities and network instability. 

To mimic a professional **Production Environment**, every significant modification to the network, firewall, or identity services is documented here.

---

## The Change Process
Before any change is applied to the "Production" environment (VLAN 10, 20, or the L3 Core), it must follow this lifecycle:

1. **Request**: Define the business or security need (e.g., "Mom needs access to a new wireless printer").
2. **Assessment**: Identify potential risks (e.g., "Does this open a hole from the IoT VLAN to the Home VLAN?").
3. **Testing**: Validate the change in a **Hyper-V Sandbox** or on an isolated port before global deployment.
4. **Implementation**: Apply the change and log the specific technical details.
5. **Verification**: Confirm the change works as intended and monitor **Splunk** logs for unexpected behavior.

---

## Change Log Structure
All entries are recorded in the `CHANGE_LOG.md` (or similar tracking file) using the following format:

| Field | Description |
| :--- | :--- |
| **ID** | Unique change identifier (e.g., CHG-001). |
| **Component** | The device or service affected (e.g., Cisco L3 Switch, pfSense, AD). |
| **Classification** | Standard (Low risk), Normal (Planned), or Emergency (Security patch). |
| **Description** | Technical details of the change (e.g., added ACL rule #105). |
| **Verification** | How success was confirmed (e.g., Nessus scan or Ping test). |

---

## Governance Policies
* **No "Cowboy" Coding**: No changes are made directly to the L3 Switch or Firewall without being logged first.
* **Rollback Plan**: Every major change must have a defined "Back-out" procedure (e.g., "Revert to Cisco startup-config").
* **Least Privilege**: All changes must adhere to the principle of Least Privilege to minimize the attack surface.

---

> **Portfolio Note**: This process demonstrates alignment with **ITIL (Information Technology Infrastructure Library)** standards, ensuring that all infrastructure modifications are intentional, documented, and reversible.
