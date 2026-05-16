# Preliminary Observations & Research Hypotheses

## Current Status: Pre-Deployment

**Collection Start Date**: Not yet deployed (planned June 2026)  
**Current Session Count**: n = 0  
**This Document**: Research hypotheses to be tested against empirical data after deployment

---

## Research Hypotheses

### H1: Scanner Indifference to Broadcast-Specific Assets
**Hypothesis**: Automated SSH scanners targeting IP ranges will engage with the honeypot at similar rates regardless of broadcast-specific banners or file artifacts, because scanners operate indiscriminately.

**Test Method**: Compare session counts and behavioral signatures between:
- Cowrie configured with generic Linux banner vs broadcast-specific banner (e.g., "encoder-backup-za1")
- Filesystem containing generic files vs broadcast-specific artifacts (`/opt/playout/`, `.rtmp_credentials`)

**Expected Finding**: No significant difference in scanner engagement rate, supporting the hypothesis that automated scanning is domain-agnostic.

---

### H2: Low Targeted Engagement Due to Broadcast Exploit Scarcity
**Hypothesis**: The honeypot will observe low rates of methodical, human-operated attack sessions because:
- No public exploit databases exist for broadcast-specific software (MagicSoft, encoder firmware)
- Broadcast infrastructure is not a high-value target for most attacker motivations (financial gain, espionage)
- Skilled attackers targeting broadcast networks conduct reconnaissance via proprietary methods not visible to honeypots

**Test Method**: Classify sessions into archetypes (Scanner / Script_Kiddie / Methodical_Operator) and measure:
- Percentage of sessions classified as Methodical_Operator
- Presence of broadcast-specific reconnaissance commands (e.g., `ps aux | grep ffmpeg`, `ls /var/streaming/`)

**Expected Finding**: <5% of sessions exhibit methodical human behavior; >80% are automated scanners.

---

### H3: CGNAT/IP Reallocation Inflates Session Duration Variance
**Hypothesis**: Lebanon's widespread use of Carrier-Grade NAT (CGNAT) and dynamic IP reallocation will introduce artifacts in session duration measurements:
- Multiple distinct attackers may appear as a single source IP (CGNAT aggregation)
- Single attackers may appear as multiple source IPs (dynamic reallocation)
- This inflates variance in session duration and inter-command timing metrics

**Test Method**: Analyze correlation between:
- Source ASN (ISPs known to use CGNAT) and session duration variance
- Geographic clustering of source IPs and behavioral similarity
- Time-of-day patterns in session activity

**Expected Finding**: Higher session duration variance for Lebanese ISP ASNs compared to cloud hosting providers or Tor exit nodes.

---

## Operational Constraints Affecting Data Collection

### Constraint 1: Passive Observation Only
- **Limitation**: No active probing or mirroring of production traffic
- **Impact on Data**: Honeypot receives only organic attacker traffic; cannot correlate with simultaneous attacks on production systems
- **Mitigation**: Cross-reference honeypot source IPs with IT Project 05 Wazuh alert logs (offline, post-collection)

### Constraint 2: Single Deployment Site
- **Limitation**: One geographic location, one ISP uplink, one network topology
- **Impact on Data**: Findings may not generalize to other broadcast networks with different ISP topologies or geographic threat landscapes
- **Mitigation**: Document constraints thoroughly; frame findings as "case study" rather than universal claims

### Constraint 3: Lebanon Regional Network Behavior
- **Limitation**: Frequent BGP route changes, power outages, ISP throttling during peak hours
- **Impact on Data**: Session timeouts or connection drops may be network-driven rather than attacker-driven
- **Mitigation**: Log network stability metrics (uptime, latency to honeypot) alongside session data; filter anomalies during analysis

---

## Baseline Expectations (Literature-Derived, Not Empirical)

Based on published honeypot studies in similar contexts (Middle East region, critical infrastructure):
- **Session volume**: Unknown (no broadcast infrastructure honeypot data exists)
- **Scanner dominance**: >80% of sessions expected to be automated scanners (consistent with SSH honeypot literature)
- **Credential patterns**: Top 10 credential pairs expected to match OWASP most common passwords list
- **Geographic distribution**: Expected concentration from China, Russia, USA, India (consistent with global SSH scan sources)

**Important**: These are expectations from general SSH honeypot literature, not predictions specific to broadcast infrastructure. Actual data may differ significantly.

---

## Post-Deployment Analysis Plan

After 30+ days of data collection:
1. Validate or reject each hypothesis using statistical tests (chi-square for categorical data, Mann-Whitney U for continuous distributions)
2. Document unexpected findings (deviations from hypotheses)
3. Publish anonymized dataset with analysis report
4. Update this document with empirical observations section

---

## Version History

**v0.1** (May 2026) — Pre-deployment hypotheses documented  
**v0.2** (Planned: July 2026) — Add empirical findings section after 30 days data collection
