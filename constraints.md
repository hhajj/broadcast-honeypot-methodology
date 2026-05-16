# Operational & Regional Constraints
## 1. Uptime Requirement (≥99.9%)
Active scanning, packet mirroring, or routing modifications prohibited. Deception operates passively on isolated infrastructure.

## 2. Hardware Limitation
Single managed switch per site. Forces honeypot deployment on spare isolated VLAN IP, not inline/tap architecture.

## 3. Regional ISP Behavior (Lebanon)
Widespread CGNAT & dynamic IP reallocation. Geolocation accuracy limited to ASN/country level. BGP shifts during instability introduce latency artifacts.

## 4. Protocol Reality
Production hosts RTMP, HLS, encoder APIs, SSH/RDP. Honeypot targets SSH/Telnet but logs cross-protocol correlation when source IPs match known broadcast scanner pools.
