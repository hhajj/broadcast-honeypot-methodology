# Operational & Regional Constraints

## 1. Uptime Requirement (≥99.9%)

**Constraint**: Broadcast playout operates 24/7 with contractual SLA commitments. Any network modification that risks service interruption is prohibited.

**Impact on Honeypot Design**:
- ✗ Cannot deploy inline/tap honeypot that intercepts production traffic
- ✗ Cannot use SPAN/port mirroring on production switch (resource contention risk)
- ✗ Cannot perform active scanning of production network for correlation
- ✓ Must deploy on isolated VLAN with zero routing to production systems

**Mitigation**: Honeypot operates passively on dedicated infrastructure. Correlation with production logs performed offline via manual log export.

---

## 2. Hardware Limitation (Single Managed Switch Per Site)

**Constraint**: Resource-constrained broadcast infrastructure. One managed switch per facility (no redundant switching fabric, no separate security monitoring network).

**Impact on Honeypot Design**:
- ✗ Cannot deploy dedicated honeypot switch with full network tap
- ✗ Cannot implement multiple honeypot nodes in distributed architecture
- ✓ Must use VLAN isolation on single physical switch
- ✓ Honeypot shares switch backplane with production (but isolated at Layer 2/3)

**Implication for Research**: This constraint makes the deployment more representative of typical broadcast/OT environments (resource-limited, single-point topologies) than enterprise security research labs with arbitrary hardware budgets.

---

## 3. Regional ISP Behavior (Lebanon)

### 3a. Widespread CGNAT (Carrier-Grade NAT)

**Constraint**: Most Lebanese ISPs use CGNAT, aggregating hundreds of customers behind single public IP addresses.

**Impact on Data**:
- ✗ Cannot reliably attribute sessions to individual attackers based on source IP alone
- ✗ Geolocation precision limited (IP may geolocate to ISP headquarters, not actual attacker location)
- ✓ Can use ASN, organization name, and temporal patterns for clustering

**Mitigation**: Document CGNAT prevalence in dataset metadata. Use session behavioral features (command patterns, timing) for clustering rather than IP-based attribution.

### 3b. Dynamic IP Reallocation

**Constraint**: Residential IP addresses reassigned frequently (daily or weekly) by ISPs due to IPv4 scarcity.

**Impact on Data**:
- ✗ Cannot track persistent attackers across multiple days using source IP
- ✗ Session duration metrics may conflate multiple distinct attackers on same IP
- ✓ Can analyze per-session behavior independently

**Mitigation**: Treat each session as independent. Do not assume temporal correlation between sessions from same IP unless behavioral fingerprinting strongly suggests same attacker.

### 3c. BGP Route Instability

**Constraint**: Lebanon's internet connectivity experiences periodic BGP route changes, ISP peering disputes, and submarine cable maintenance affecting latency/uptime.

**Impact on Data**:
- ✗ Session timeouts or connection drops may be network-driven, not attacker-driven
- ✗ Latency spikes may appear in inter-command timing metrics
- ✓ Can log external network stability metrics for correlation

**Mitigation**: Collect concurrent uptime/latency data from external monitoring service (e.g., ping honeypot IP from international probe). Filter sessions during documented network instability periods.

---

## 4. Protocol & Technology Reality

### 4a. Production Protocols

**Context**: Broadcast infrastructure uses domain-specific protocols not present in typical IT environments:
- **RTMP** (Real-Time Messaging Protocol) for video ingest
- **HLS/MPEG-DASH** for adaptive streaming output
- **Proprietary APIs** for encoder control (vendor-specific REST/SOAP endpoints)
- **Standard Protocols**: SSH, RDP, SNMP for management

**Impact on Honeypot**:
- Cowrie supports only SSH/Telnet (not RTMP, HLS, or proprietary APIs)
- Honeypot captures management-layer attacks, not media-layer attacks
- Missing protocol coverage is a research limitation (documented in publications)

**Future Work**: Extend to fake RTMP endpoint or encoder web UI honeypot (out of scope for initial deployment).

### 4b. Cross-Protocol Correlation Opportunity

**Opportunity**: When source IPs in honeypot logs match IPs in production Wazuh/Netdata logs (IT Project 04), can identify attackers simultaneously probing multiple broadcast services.

**Limitation**: Correlation performed offline due to network isolation; no real-time blocking or response.

---

## 5. Ethical & Legal Constraints

### 5a. No Active Engagement

**Constraint**: No active hacking-back, payload execution, or attacker tracking beyond passive logging.

**Rationale**: Legal uncertainty around active defense in Lebanon; risk of escalation; research ethics.

**Impact**: Honeypot is purely observational (logs behavior, does not retaliate or gather attacker intelligence beyond what they voluntarily provide).

### 5b. Data Privacy

**Constraint**: Even though honeypot targets malicious actors, must still respect data minimization principles.

**Impact on Data**:
- Passwords are logged (to study credential patterns) but personally identifiable passwords (names, email addresses) are filtered before publication
- Source IPs are partially anonymized (last octet masked) for residential networks
- No packet payloads beyond Cowrie's session logs (no deep packet inspection)

---

## Summary Table

| Constraint Category | Specific Limitation | Design Mitigation |
|---------------------|---------------------|-------------------|
| Operational | ≥99.9% uptime requirement | Passive isolated deployment, zero production interference |
| Hardware | Single managed switch | VLAN isolation, no inline tap |
| Regional ISP | CGNAT IP aggregation | Behavioral clustering, not IP-based attribution |
| Regional ISP | Dynamic IP reallocation | Per-session analysis, no cross-day tracking |
| Regional ISP | BGP route instability | External network monitoring for filtering |
| Protocol | Limited to SSH/Telnet | Document limitation, plan future extension |
| Ethical | No active engagement | Passive logging only |

---

**Document Version**: v0.1 (May 2026)  
**Status**: Pre-deployment constraint documentation
