DMVPN PHASE 3 - DUAL HUB FAILOVER LAB
=====================================

OVERVIEW
--------
This lab demonstrates a Cisco DMVPN Phase 3 design with:
- Dual Hub architecture (R1 Primary, R2 Secondary)
- Three Spoke routers (R3, R4, R5)
- Dynamic spoke-to-spoke tunnel formation
- Automatic hub failover capability

No IPsec or routing protocols are used in this lab to focus on DMVPN control-plane behavior.

---

TOPOLOGY
--------
Underlay Network: 10.0.100.0/24
Overlay Network: 172.16.100.0/24

R1 (Hub1)     - 10.0.100.1
R2 (Hub2)     - 10.0.100.2
R3 (Spoke1)   - 10.0.100.3
R4 (Spoke2)   - 10.0.100.4
R5 (Spoke3)   - 10.0.100.5

---

KEY TECHNOLOGIES
----------------
- GRE Multipoint (mGRE)
- NHRP (Next Hop Resolution Protocol)
- DMVPN Phase 3
- Dual Hub Redundancy
- Spoke-to-Spoke Dynamic Tunnels

---

HOW DMVPN WORKS
---------------

1. SPOKE REGISTRATION
   Spokes register with both hubs using NHRP.

2. INITIAL TRAFFIC FLOW
   Traffic initially flows via hub.

3. NHRP REDIRECT
   Hub instructs spokes to avoid hub for transit traffic.

4. SHORTCUT CREATION
   Spokes dynamically learn each other's NBMA addresses.

5. DIRECT TUNNEL
   Spoke-to-spoke GRE tunnel is formed automatically.

---

HUB FAILOVER BEHAVIOR
---------------------
If R1 (primary hub) fails:
- Spokes detect NHRP timeout
- Spokes automatically re-register with R2
- DMVPN remains operational without manual intervention

---

KEY COMMANDS
------------
HUB:
- ip nhrp redirect

SPOKES:
- ip nhrp shortcut
- ip nhrp nhs (dual hub configuration)

---

VERIFICATION COMMANDS
---------------------
show ip interface brief
show ip nhrp
show dmvpn
ping 172.16.100.X

---

EXPECTED BEHAVIOR
-----------------
- All tunnels come UP/UP
- Spokes register with both hubs
- First traffic goes via hub
- Then spoke-to-spoke direct tunnel forms
- Failover works automatically when primary hub is down
