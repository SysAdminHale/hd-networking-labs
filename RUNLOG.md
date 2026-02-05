# RUNLOG — Edge / Beryl AX

Operational run log for edge router configuration, security hardening, and validation.

---

## 2026-02-04 — Task 1: Baseline Router Reset and Initial Access

**Context**  
Prepared the Beryl AX for structured configuration by resetting to a known-good baseline.

**Actions**  
- Performed factory reset of the router.
- Confirmed administrative access via LuCI.
- Verified default LAN addressing and DHCP functionality.
- Confirmed WAN connectivity and basic internet access.

**Verification**  
- Router reachable at 192.168.8.1.
- LAN clients successfully received IP addresses.
- Internet access functional.

**Current State**  
Router operating from a clean baseline with no custom firewall or wireless policies applied.

---

## 2026-02-04 — Task 2: Wireless SSID Separation and Subnet Mapping

**Context**  
Established logical separation between administrative and guest wireless access.

**Actions**  
- Created distinct SSIDs for administrative and guest access.
- Mapped SSIDs to separate IP subnets:
  - Admin: 192.168.8.0/24
  - Guest: 192.168.9.0/24
- Verified correct DHCP behavior per subnet.
- Confirmed internet access from both SSIDs.

**Verification**  
- Admin and Guest clients received IPs from correct subnets.
- Guest devices could reach the internet but not the router management interface by default.

**Notes**  
Initial access control was enforced implicitly via SSID-to-subnet boundaries.

**Current State**  
SSID-based segmentation established and functioning.

---

## 2026-02-04 — Task 3: Controlled Reachability and Firewall Experimentation

**Context**  
Explored how routing and firewall rules affect cross-subnet and management access.

**Actions**  
- Examined default routing and firewall behavior between Admin and Guest subnets.
- Temporarily added a narrowly scoped firewall rule allowing a single Guest IP to access the router admin interface.
- Verified access was limited to the specific IP, port, and zone pairing.
- Removed the temporary rule to restore isolation.

**Verification**  
- Confirmed controlled access behaved as expected.
- Confirmed Guest isolation was immediately restored after rule removal.

**Notes / Lessons Learned**  
- Firewall rules can safely override zone isolation when narrowly scoped.
- Access control can be expressed as short-lived intent rather than permanent policy.

**Current State**  
Guest isolation restored; no experimental rules remain in place.

---

## 2026-02-04 — Task 4: Admin Jump-Host Hardening (Complete)

**Context**  
Hardened router administrative access to eliminate reliance on SSID trust and reduce attack surface.

**Actions**  
- Designated **t490 (192.168.8.246)** as the sole administrative jump host.
- Reserved static DHCP lease for t490.
- Explicitly left **LAN zone INPUT policy = accept** to avoid brittle, global restrictions.
- Implemented two firewall traffic rules:
  - **ALLOW**: t490 → router admin services (TCP 80 / 443 / 22).
  - **DENY**: all other LAN clients → router admin services.
- Recreated and secured wireless SSIDs using WPA2/WPA3.
- Verified behavior across both Admin and Guest SSIDs.

**Verification**  
- Confirmed t490 can access LuCI admin UI successfully.
- Confirmed all other LAN devices are blocked from admin UI, regardless of SSID.
- Verified normal LAN and WAN traffic remains unaffected.
- Verified configuration persists across wireless restarts and router reboot.

**Notes / Lessons Learned**  
- Attempting to restrict admin access via LAN zone INPUT policy (`reject`) resulted in loss of access and required reset.
- Traffic-rule-based enforcement provides precise control without risking self-lockout.
- Administrative authority is now identity-bound (jump host), not SSID-bound.

**Current State**  
Router admin access is intentionally restricted to t490 only.  
This behavior is expected, documented, and reusable as a standard pattern.

---

**Status**  
Edge router baseline configuration and administrative hardening complete.
