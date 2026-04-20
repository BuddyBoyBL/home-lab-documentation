# Firewall Platform Plan

## Summary
The firewall direction for the home lab has shifted from pfSense to OPNsense based on installation practicality and current setup needs.

## Current Direction
The lab is currently leaning toward **OPNsense** as the primary firewall platform.

## Why the Plan Changed
The original plan was to use **pfSense**, but that direction became less practical after discovering that the offline installation path I expected to use was no longer available in the way I originally planned.

Rather than forcing the build around that limitation, I began evaluating **OPNsense** as a more workable option for the environment.

## Current Blocker
Firewall deployment is currently paused due to an installation issue involving **drive partitioning**.

## Suspected Causes
The issue may be related to one or more of the following:
- keyboard input not being recognized correctly during installation
- the need to inspect or prepare the drive from the admin laptop
- a possible issue with the drive reader or the drive itself

## Current Troubleshooting Direction
The current troubleshooting path includes:
- testing another keyboard
- using a SATA cable with external power
- checking whether the problem is the drive reader or the drive itself
- preparing or inspecting the drive from the admin laptop if needed

## Why This Matters
The firewall is a central part of the long-term lab design, so it is important to choose a platform that is practical to install, support, and learn in the current environment.

## Next Steps
- continue OPNsense installation troubleshooting
- test alternate keyboard input
- test drive access with the SATA cable and external power
- prepare the storage device from the admin laptop if needed
- proceed with firewall deployment once installation issues are resolved
