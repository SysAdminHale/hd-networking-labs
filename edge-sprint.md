# Edge Sprint 1 — Trust, Access, Observability

## Day 0 — Kickoff

## Take 1 and 2

**Goal**
Establish a lightweight, repeatable edge-learning workflow using the Beryl AX to inform HD3.0 design decisions.

**What I built**
- Created separate Admin and Guest SSIDs on the Beryl AX.
- Assigned my OldThinkPad to the Admin SSID and mobile devices to the Guest SSID.
- Verified that Guest devices could reach the internet but were blocked from the router management interface.
- Confirmed Admin and Guest SSIDs are mapped to different IP subnets
  (Admin: 192.168.8.0/24, Guest: 192.168.9.0/24).


**What surprised me**
- Access control was enforced entirely by interface/SSID, not device identity.
- Simply switching Wi-Fi networks instantly changed what infrastructure I could reach, without logging in or out.
- Subnet boundaries alone enforced strong isolation before any explicit firewall rules were touched.


**What changed in my thinking**
- Management access is about *where* you connect from, not *who* you are.
- SSIDs function like trust zones and are a clean mental model for VLAN and firewall design in HD3.0.

## Take 3 — Routing and Controlled Reachability

**Goal**
Observe how routing and firewall rules determine cross-subnet access.

**What I built**
- Examined default routing and firewall rules between Admin and Guest subnets.
- Temporarily allowed controlled access to validate policy-driven reachability.

**What surprised me**
- One firewall rule instantly reshaped trust boundaries without touching devices or credentials.

**What changed in my thinking**
- Security posture is enforced by routing and policy first; identity comes later.


