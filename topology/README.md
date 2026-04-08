# Home Lab Network Topology

## Overview
This home lab is designed to simulate a small enterprise environment with segmented networks, centralized logging, identity management, and an isolated malware analysis zone. The goal is to build practical experience in IT support, system administration, SOC operations, and defensive security engineering.

---

## Physical Topology

```text
                    [ Internet / Xfinity ]
                            |
                            |
                 [ Xfinity Gateway - Bridge Mode ]
                            |
                            |
               [ Hardware Firewall - pfSense/OPNsense ]
                            |
                            |
                    [ Layer 3 Managed Switch ]
             _____________|_____________|_____________
            |             |             |             |
            |             |             |             |
       [Mgmt VLAN]    [Corp VLAN]    [IoT VLAN]   [Sandbox VLAN]
            |             |             |             |
            |             |             |             |
   Admin Laptop      Family PCs      Printers,      Test PC /
   Nessus Scanner    Domain PCs      IoT Devices    Malware VMs
                                      APs
