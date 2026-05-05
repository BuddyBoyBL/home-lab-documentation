# Firewall Platform, Router Hardening, and Access Security Update

## Summary
Made progress across multiple parts of the home lab, including firewall platform planning, router hardening, account security, hardware simplification, and storage troubleshooting preparation.

## Firewall Platform Update

### pfSense Reconsidered
I started re-evaluating **pfSense** as the planned firewall platform after realizing that the offline installation path I expected to use was no longer available in the way I originally planned.

This pushed me to look more seriously at **OPNsense** as the better option for the lab moving forward.

### OPNsense Installation Issue
While shifting toward OPNsense, I ran into a problem with **partitioning the drive** during installation.

## Current Suspected Causes
At this point, the issue appears to possibly involve one of the following:
- keyboard input not being recognized correctly during installation
- the need to connect the drive to the admin laptop with an adapter for inspection or preparation
- a possible issue related to the storage media itself

## Current Troubleshooting Direction
To keep narrowing down the cause, I identified a low-cost SATA cable with external power that can be used to test whether the issue is:
- the hard drive reader
- or the hard drive itself

This should help isolate the problem before moving further with firewall deployment.

## Router Hardening Update

### Completed Hardening Steps
On the new router, I completed several initial hardening actions:

- changed the default admin password
- changed access point and network naming so router-identifying information is not exposed
- reset passwords as part of securing the setup
- updated the router firmware
- began planning SSID naming structure

### Current Approach
I am intentionally waiting to do more advanced wireless and segmentation-related configuration until the **switch** and **firewall** are fully in place. This should reduce rework and make the final setup cleaner.

## Hardware Simplification Update

### Netgear Router Temporarily Removed from Plan
For now, I am leaving the **Netgear router** out of the active homelab setup in order to keep the environment less complicated until the rest of the infrastructure is integrated.

**Reason:**
- reduce moving parts during the current build stage
- keep the setup easier to troubleshoot
- focus first on the core network, firewall, and switch integration

This is a temporary simplification, not a permanent removal from future planning.

## Access Security Update

### Yubico NFC Passkey Added
I received a **Yubico NFC passkey** that I plan to use as a backup passkey for important websites.

**Planned purpose:**
- add stronger account security for important services
- provide a backup authentication method
- improve long-term access security as the lab and related accounts expand

**Current note:**
- this is part of the broader security hardening approach around account protection, not just the physical lab setup

## Why This Matters
This update reflects several important themes in the lab build process:

- adapting when the original firewall plan becomes less practical
- troubleshooting hardware problems methodically instead of guessing
- hardening network equipment early
- simplifying the environment when complexity would slow progress
- improving account security alongside infrastructure security

Together, these changes support a more realistic and maintainable build process.

## Current Position
- pfSense is no longer the preferred direction
- OPNsense is currently the leading firewall option
- firewall installation is blocked by a storage/input troubleshooting issue
- router baseline hardening has been completed
- the Netgear router is being left out for now to reduce complexity
- a Yubico NFC passkey has been added for backup account security use
- a new SATA cable with external power has been identified for additional hard drive testing

## Follow-Up Items
- test another keyboard during firewall installation
- use the SATA cable with external power to help determine whether the issue is the drive reader or the hard drive
- inspect or prepare the drive from the admin laptop if needed
- continue OPNsense installation troubleshooting
- finalize SSID naming plan
- keep the active network design simplified until the switch and firewall are fully integrated
- begin using the Yubico NFC passkey for important websites where appropriate

## Key Takeaway
The lab is continuing to move forward through a mix of troubleshooting, simplification, and hardening. Instead of forcing the original plan, I am adjusting the build based on what is practical, secure, and supportable with the current hardware and budget.
