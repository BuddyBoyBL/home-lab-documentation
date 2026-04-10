# Progress Entry: Topology Adjustment and Test PC Integration

## Date
April 2026

## Summary
I made additional progress on my home lab build and clarified part of the physical network layout. After reviewing the setup more closely, I realized that my test PC needs to be physically connected directly to the switch instead of relying on the router path I had originally considered.

## Physical Connectivity Update
- Determined that I need a **100 ft Ethernet cable** to connect the test PC to the switch
- Measured the required distance at approximately **78 ft**
- Confirmed that a 100 ft cable will provide enough length with some extra slack for cable routing and flexibility

## Topology Change
Because of this adjustment, I need to update my network topology documentation.

### Previous Idea
- Switch connected to router
- Router used as part of the path for the test environment

### Updated Plan
- **Test PC will connect directly to the switch**
- **Test printer will be physically connected to the test PC**
- **Router will now be repurposed for guest Wi-Fi access on the test network**

This creates a cleaner separation between the wired test environment and the guest wireless portion of the lab.

## Test PC Status
The test PC has already been partially prepared for lab use.

### Installed / Configured
- Windows 10 Pro
- Wireshark
- Sysmon
- Splunk Universal Forwarder
- Splunk account preparation in progress / accounted for

## Documentation Status
- Cleaned up the topology design
- Still need to **update the actual topology documentation and diagrams** so they reflect the new direct-switch connection model

## Why This Matters
This change improves the realism and structure of the lab by making the test machine part of the wired environment directly, rather than placing unnecessary dependency on the router. It also helps better separate:
- wired test systems
- guest wireless access
- future monitoring and traffic analysis

## Next Steps
- Purchase and install the 100 ft Ethernet cable
- Physically connect the test PC to the switch
- Connect the test printer to the test PC
- Update topology documentation and diagrams
- Continue configuring monitoring and logging tools on the test system

## Key Takeaways
- Physical layout matters just as much as logical design
- Measuring distance before purchasing cable helped avoid guesswork
- Simplifying the topology makes the lab easier to document and troubleshoot
