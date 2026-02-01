# Edge Sprint 1 — Trust, Access, Observability

## Day 0 — Kickoff

**Goal**

Establish a lightweight, repeatable edge-learning workflow using the Beryl AX to inform HD3.0 design decisions.

**What I built**
- Created separate Admin and Guest SSIDs on the Beryl AX.
- Assigned my OldThinkPad to the Admin SSID and mobile devices to the Guest SSID.
- Verified that Guest devices could reach the internet but were blocked from the router management interface.

**What surprised me**
- Access control was enforced entirely by interface/SSID, not device identity.
- Switching Wi-Fi networks instantly changed what resources I could reach, without logging in or out.

**What changed in my thinking**
- Management access is about *where* you connect from, not *who* you are.
- SSIDs function like trust zones and are a clean mental model for VLAN and firewall design in HD3.0.


