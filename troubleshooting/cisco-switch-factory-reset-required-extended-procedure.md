# Title: Cisco Switch Factory Reset Required Extended 90-Second Reset Procedure

## Environment
- Device: Cisco switch (SG300 series)
- Access Method: Physical reset button
- Context: Reset required during lab setup and reconfiguration

## Problem Description
The Cisco switch did not reset using a standard reset attempt. Initial attempts to restore factory settings were unsuccessful.

## Symptoms
- Reset button press did not restore factory configuration
- Switch retained previous configuration after initial reset attempt
- Unable to access switch as expected after reset

## Initial Hypothesis
- Reset procedure was not being executed correctly
- Reset timing or sequence may be required for full factory reset

## Troubleshooting Steps Taken
1. Attempted standard reset using reset button
2. Observed that configuration was not cleared
3. Consulted official Cisco documentation for reset procedure
4. Followed extended reset sequence:
   - Held reset button for 30 seconds
   - Unplugged power while continuing to hold reset for 30 seconds
   - Plugged power back in while still holding reset for an additional 30 seconds
5. Released reset button after total 90 seconds

## Root Cause
The switch required a full extended reset sequence to properly clear the configuration. A simple reset press was not sufficient to trigger a factory reset.

## Resolution
Performed the full 90-second reset procedure as described in official documentation.

## Validation
- Switch reset successfully
- Configuration was cleared
- Device returned to factory default state
- Able to proceed with setup

## Lessons Learned
- Some network devices require precise reset timing and sequence
- Standard reset attempts may not fully clear configuration
- Official documentation should be referenced when default methods fail

## References
- Cisco official reset procedure documentation
