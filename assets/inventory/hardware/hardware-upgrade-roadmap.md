# Hardware Upgrade Roadmap

## Overview

At this stage, the home lab does not have many urgent hardware gaps left. Most of the remaining work is implementation, configuration, and stabilization rather than replacement. The current hardware is sufficient to build the initial lab design.

## Current Assessment

The present focus is to get the network online, segmented, documented, and stable before making additional purchases. Most supporting hardware is already good enough for the current phase.

## Upgrade Priorities

### 1. Xfinity Gateway / Modem
**Priority:** No current upgrade needed

The Xfinity gateway is not a spending priority right now. Its main future role is to serve as the ISP handoff device. The most useful future adjustment is enabling **Bridge Mode** so routing and firewall responsibilities can be handed off to pfSense.

**Plan:**
- Keep current hardware
- Use as ISP handoff
- Enable Bridge Mode later when pfSense becomes the primary router

---

### 2. Mini PC Firewall
**Priority:** Current plan is correct

The firewall mini PC should remain in service as long as it meets current needs. The right order is:

1. Keep using the current box
2. Upgrade RAM only if real performance pressure appears
3. Replace the firewall hardware only if it becomes a real bottleneck

This avoids over-investing in a budget firewall platform before the rest of the lab is fully operational.

**Plan:**
- Deploy current firewall hardware first
- Monitor performance
- Upgrade only if usage justifies it

---

### 3. Ethernet Run Through the Wall
**Priority:** Not urgent

The current Ethernet run already works and is reasonably out of the way. Running cable through the wall is a long-term cleanliness and permanence upgrade, not a current functional need.

**Plan:**
- Keep current cable path for now
- Revisit later if a cleaner permanent installation is needed

---

### 4. Cisco Switch
**Priority:** No upgrade needed

The Cisco SG300-class switch is still a strong fit for the project. It supports the VLAN and segmentation work needed for the current lab design.

**Plan:**
- Keep the current switch
- Continue using it as the core switching layer
- Replace only if it fails or becomes a speed/port bottleneck

---

### 5. VLAN-Capable Wireless Access Point
**Priority:** Main future upgrade

This remains the clearest future hardware addition. A VLAN-capable AP will support multiple SSIDs tied to segmented networks such as family, guest, and IoT.

**Plan:**
- Add later when ready to extend VLAN design to wireless
- Use it to support segmented Wi-Fi access

---

### 6. Test PC
**Priority:** Keep as-is

The current test PC remains valuable. It serves both as a test endpoint and as a troubleshooting console. There is no practical reason to replace it at this stage.

**Plan:**
- Keep current test PC
- Continue using it for lab validation and troubleshooting

---

### 7. Admin Laptop
**Priority:** No new purchase needed

The current laptop already meets the need for administration, troubleshooting, and documentation. Buying another admin laptop now would mostly duplicate function rather than add capability.

**Plan:**
- Continue using the current admin laptop
- Reassess only if it becomes a real bottleneck later

---

### 8. Docking Station, Monitors, Wi-Fi Extender, and Cat6 Cables
**Priority:** No action needed

These supporting components are already sufficient for the current stage of the project. They do not need to be replaced before the core network is built and stabilized.

**Plan:**
- Keep current supporting hardware in place
- Document and organize later as part of asset inventory expansion

## Practical Summary

### Near-Term / Meaningful
- No required hardware upgrades beyond implementation

### Later
- VLAN-capable wireless access point

### Conditional
- Mini PC firewall RAM upgrade, only if performance pressure appears
- Better firewall mini PC, only if the current system becomes limiting

### Long-Term / Optional
- In-wall Ethernet drop
- Larger external storage for logs and family backup
- MFA hardware key
- Second ISP line or separate service path

## Conclusion

There is no major forgotten hardware upgrade at this stage. The current environment is sufficient to begin building the lab properly. The priority now is not buying more hardware. The priority is getting the current design online, segmented, documented, and stable.

The main future hardware addition that still stands out is a **VLAN-capable wireless access point**. Everything else is either already sufficient or correctly deferred.
