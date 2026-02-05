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
- Added a temporary, surgical firewall rule allowing a single Guest IP to access the router admin interface.
- Verified access worked only for the specified IP, port, and zone pairing.
- Removed the rule and confirmed Guest isolation was immediately restored.


**What surprised me**
- One firewall rule instantly reshaped trust boundaries without touching devices or credentials.
- Fine-grained firewall rules can safely override zone isolation without weakening the overall model.
- Access control can be expressed as short-lived intent, not permanent configuration.


**What changed in my thinking**
- Security posture is enforced by routing and policy first; identity comes later.
- Firewall rules are reversible decisions, not static permissions.
- Secure design includes the ability to grant, test, and revoke access cleanly.
- This pattern maps directly to jump hosts, admin VLANs, and break-glass access in HD3.0.

> Takeaway: Trust boundaries should be enforced by default, with exceptions treated as temporary experiments.

## 🔒 Task 4 — Admin Jump-Host Hardening (Complete)

**Goal**  
Restrict router administrative access to a single, designated jump host without relying on SSID trust, zone policy changes, or brittle firewall defaults.

**What I built**  
- Designated **t490 (192.168.8.246)** as the sole administrative jump host.
- Left **LAN zone INPUT = accept** to avoid fragile, global policy changes.
- Implemented two explicit firewall traffic rules:
  - **ALLOW**: t490 → router admin services (80 / 443 / 22).
  - **DENY**: all other LAN clients → router admin services.
- Verified behavior across SSIDs:
  - t490 can access the admin UI from any LAN-attached SSID.
  - All other devices are blocked from the admin UI, even when connected to the admin SSID.
- Confirmed normal LAN and internet traffic remains unaffected.
- Verified configuration survives reboot and SSID changes.

**What surprised me**  
- SSID membership alone is a weak security boundary for management access.
- Identity-based control (IP-bound jump host) is more robust than interface-based trust.
- Leaving zone policies permissive and enforcing intent through narrow rules is safer than tightening zones.
- The firewall became an explicit expression of administrative intent, not just a traffic filter.

**What changed in my thinking**  
- Management access is about *who is allowed to manage*, not merely *where* they connect.
- SSIDs define convenience and segmentation, not authority.
- Jump hosts are the correct abstraction for administrative control—even at small scale.
- Strong security comes from default reachability with explicit exceptions, not blanket restriction.
- This model scales cleanly to servers, hypervisors, and future HaleDistrict phases.

**Takeaway**  
Administrative access should be enforced by **explicit identity-bound policy**, not by SSID trust or zone-wide restrictions.  
Jump-host patterns provide strong security while remaining predictable, reversible, and resilient.

**Status**  
Task 4 complete. Admin access is now deliberate, minimal, and boring—in the best possible way.

