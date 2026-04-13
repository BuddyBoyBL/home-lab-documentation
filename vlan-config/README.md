
# VLAN Configuration

## Overview
This section documents the VLAN design for the home lab network. The goal of this configuration is to separate devices by trust level and function, reduce unnecessary communication between systems, and create a safer environment for testing, monitoring, and future security analysis.

This VLAN structure supports a small enterprise-style layout with dedicated networks for management, trusted home devices, IoT systems, lab resources, guest access, and isolated malware research.

---

## Objectives
- Separate trusted and untrusted devices
- Reduce risk to family and production devices
- Create a dedicated lab environment for testing
- Support future firewall rules, logging, and monitoring
- Build a network structure that reflects real enterprise segmentation practices

---

## Planned VLANs

| VLAN ID | Name | Purpose | Example Devices |
|---|---|---|---|
| 10 | Management | Administrative access to infrastructure | Admin laptop, switch management, firewall management |
| 20 | Home | Trusted household network | Mom’s PC, sister’s laptop, personal devices |
| 25 | IoT | Low-trust smart devices and peripherals | Smart TVs, Alexa, smart lights, printers |
| 30 | Lab | Test environment for servers, clients, and monitoring tools | Test PC, virtual client, Splunk, Active Directory |
| 40 | Guest | Temporary internet-only access | Guest phones, guest laptops |
| 666 | Malware | Fully isolated high-risk testing network | Sandbox VM, malware analysis systems |

---

## Design Notes

### VLAN 10 - Management
This VLAN is reserved for administrative access to core network devices and management interfaces. It should remain tightly restricted and only be accessible from approved admin systems.

### VLAN 20 - Home
This VLAN is intended for normal day-to-day household use. It contains trusted systems and should be protected from lab traffic, IoT devices, and malware testing environments.

### VLAN 25 - IoT
This VLAN is used for devices that are convenient but less trustworthy from a security standpoint. These devices should have limited access to internal systems and only the minimum external access required to function.

### VLAN 30 - Lab
This VLAN is the main working area for the home lab. It will contain test systems, virtual machines, monitoring tools, and server infrastructure used for learning and experimentation.

### VLAN 40 - Guest
This VLAN is intended for temporary devices that should only have internet access and no access to internal lab or household systems.

### VLAN 666 - Malware
This VLAN is reserved for isolated malware testing and research. It is intended to remain heavily restricted, with no normal access to trusted internal networks.

---

## Planned Traffic Model
The general traffic model for this network is:

- Management devices can access infrastructure interfaces
- Home devices can access the internet and approved internal services
- IoT devices have limited internal access
- Lab devices are separated from trusted home devices
- Guest devices have internet-only access
- Malware systems remain isolated from all trusted networks

Detailed firewall rules and ACL behavior will be documented separately.

---

## Current Status
- [ ] Final VLANs created on core switch
- [ ] Access ports assigned
- [ ] Trunk configured between firewall and switch
- [ ] Inter-VLAN routing reviewed
- [ ] Firewall policy mapped to VLAN structure
- [ ] Device placement validated
- [ ] Documentation updated with screenshots and test results

---

## Related Documentation
- Topology documentation
- Firewall configuration
- Asset inventory
- Progress logs
- Troubleshooting notes

---

## Notes
This VLAN plan is intended to evolve as the lab grows. Changes to VLAN purpose, device placement, or routing policy should be recorded in the project progress log and change documentation.
