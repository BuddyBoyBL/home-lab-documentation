## Initial Network Topology Plan

This home lab is being built to simulate a small enterprise IT and SOC environment, focusing on network segmentation, identity management, logging, and security monitoring.

### Architecture Overview
The network is structured with a clear separation between edge, core, and access layers:

- **Edge:** Xfinity Gateway (Bridge Mode) → pfSense/OPNsense Firewall  
- **Core:** Layer 3 Switch handling VLAN routing and segmentation  
- **Access:** Wireless access points (Linksys Velop, Netgear) in bridge mode  

This design allows centralized control of traffic while maintaining flexibility for testing and expansion.

---

### VLAN Segmentation Strategy
The network is divided into logical zones to enforce security boundaries:

- **VLAN 10 – Management:**  
  Administrative systems and network device interfaces. Restricted access only.

- **VLAN 20 – Corporate / Workstations:**  
  Family devices and domain-joined systems managed through Active Directory and Group Policy.

- **VLAN 30 – IoT / Peripheral Devices:**  
  Printers, smart devices, and other low-trust endpoints. Isolated from internal systems.

- **VLAN 99 – Sandbox / Malware Lab:**  
  Fully isolated environment for malware testing. No internet access and restricted communication.

---

### Core Systems (Planned)
The lab will integrate multiple security and IT systems to replicate real-world environments:

- **Active Directory:** Centralized identity and access management  
- **Splunk (SIEM):** Log aggregation and analysis (Sysmon, firewall, and network logs)  
- **Wazuh (EDR):** Endpoint monitoring and file integrity tracking  
- **Nessus:** Vulnerability scanning and reporting  
- **pfSense/OPNsense:** Firewall rules, segmentation, and traffic inspection  
- **Jira:** Ticketing system for incident tracking and workflow simulation  

---

### Objectives
- Build a segmented and secure network environment  
- Gain hands-on experience with enterprise IT and SOC tools  
- Practice log analysis, incident detection, and troubleshooting  
- Simulate real-world scenarios such as authentication failures, malware activity, and network issues  
- Document all configurations, changes, and findings for portfolio development  

---

### Current Progress
- [ ] VLAN configuration and network segmentation  
- [ ] Firewall deployment and rule configuration  
- [ ] Active Directory setup and domain joining  
- [ ] Splunk deployment and log ingestion  
- [ ] Endpoint monitoring (Sysmon / Wazuh)  
- [ ] Vulnerability scanning baseline  
- [ ] Malware sandbox environment  

---

This lab is being developed incrementally with a focus on stability, visibility, and security before introducing higher-risk testing such as malware analysis.
