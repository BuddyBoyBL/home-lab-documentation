# Virtualization Resource Allocation Plan

## Overview

This document defines the current and planned resource allocation strategy for the homelab virtual machine environment.

The goal is to balance:
- performance
- stability
- resource efficiency
- virtualization flexibility
- long-term scalability

while operating within current hardware limits.

The host system currently has:

- **31.8 GB total RAM available**

Dynamic memory will be enabled selectively where appropriate to reduce unnecessary memory consumption while still allowing workloads to scale when needed.

---

# Current Virtual Machine Allocation Plan

| System | Current RAM Allocation | Dynamic Memory | Purpose |
|--------|------------------------|----------------|--------|
| Sandbox/Test Environment | 2 GB | Adjustable as needed | Basic testing, management validation, isolated connectivity testing |
| Splunk Server | 7 GB | Enabled | SIEM / logging platform |
| Client PC VM | 4 GB | Optional | Endpoint simulation / domain testing |
| Active Directory Domain Controller | 4 GB | Enabled if needed | Authentication, DNS, Group Policy |
| Cloud / Storage Server | 2 GB | Enabled | Lightweight storage and cloud-related services |

---

# Infrastructure Design Notes

## Sandbox / Test Environment

### Current Allocation
- 2 GB RAM

### Purpose
The sandbox environment is currently intended for:
- management testing
- connectivity validation
- configuration testing
- lightweight experimentation

### Resource Strategy
The sandbox is intentionally configured with minimal memory usage during the current phase of development.

Additional RAM will be allocated later as workloads increase or more resource-intensive testing becomes necessary.

This allows:
- better host system responsiveness
- more efficient resource distribution
- flexible scaling based on operational needs

---

## Splunk Server

### Current Configuration
- Windows deployment
- 7 GB RAM allocated
- Dynamic memory enabled

### Purpose
- SIEM operations
- log collection
- dashboard testing
- event monitoring

### Planned Future Direction
Future plans include migrating the Splunk environment from Windows to Ubuntu Server.

### Reason for Planned Migration
A Linux-based deployment is expected to provide:
- lower operating system overhead
- improved resource efficiency
- better long-term scalability
- additional Linux administration experience

---

## Client PC VM

### Current Configuration
- 4 GB RAM allocated

### Purpose
- endpoint simulation
- Active Directory testing
- Group Policy validation
- user environment testing

---

## Active Directory Domain Controller

### Current Configuration
- 4 GB RAM allocated
- Dynamic memory enabled as needed

### Purpose
- centralized authentication
- DNS services
- Group Policy management
- domain administration

---

## Cloud / Storage Server

### Current Configuration
- 2 GB RAM allocated
- Dynamic memory enabled

### Purpose
- lightweight storage
- backup planning
- cloud-related service experimentation

### Notes
Current storage-related workloads are minimal, so resource allocation remains intentionally conservative.

---

# Resource Management Philosophy

## 1. Conservative Allocation First
Virtual machines are initially assigned lower memory allocations and scaled upward only when justified by workload requirements.

---

## 2. Flexible Scaling
Dynamic memory is used where appropriate to:
- improve overall host responsiveness
- avoid unnecessary RAM consumption
- allow temporary workload spikes without permanent over-allocation

---

## 3. Segmented Infrastructure Roles
Each VM serves a dedicated operational purpose:
- logging
- authentication
- testing
- sandboxing
- storage

This mirrors enterprise infrastructure separation practices.

---

# Planned Future Improvements

## Virtualization Improvements
- More refined dynamic memory tuning
- Migration of Splunk to Ubuntu Server
- Additional segmented virtual clients
- Dedicated monitoring systems

---

## Performance Improvements
- Increased host RAM capacity if needed
- Improved storage performance
- More aggressive workload isolation
- Expanded monitoring capabilities

---

# Current Status

- [x] Initial allocation strategy defined
- [x] Virtualization segmentation strategy established
- [x] Splunk deployment active
- [x] Sandbox environment operational
- [ ] Linux migration completed
- [ ] Advanced resource tuning completed
- [ ] Long-term workload optimization finalized

---

# Notes

This allocation plan is intentionally modular and flexible. Current resource assignments prioritize:
- learning
- stability
- testing flexibility
- efficient host utilization

Resource allocations are expected to evolve as:
- services expand
- monitoring increases
- logging workloads grow
- security tooling becomes more advanced
