# Project Roadmap & Timeline

## Current Status: Phase 0 (Methodology Documentation)

**Last Updated**: May 16, 2026  
**Repository Status**: Pre-deployment methodology documentation  
**Data Collection Status**: Not yet started

---

## Timeline

### Phase 0: Pre-Deployment Methodology (May 2026) ✅ IN PROGRESS
- [x] Document network topology and isolation design
- [x] Define data schema for session/command logs
- [x] Document operational constraints (uptime, hardware, regional ISP behavior)
- [x] Establish research hypotheses
- [ ] Review methodology with security research community (GitHub issues welcome)
- [ ] Finalize Cowrie configuration based on feedback

**Deliverable**: Public GitHub repository with reproducible methodology

---

### Phase 1: Deployment & Baseline Collection (June-July 2026) 🔜 PLANNED
- [ ] Deploy Cowrie on isolated VLAN in broadcast network
- [ ] Verify logging pipeline (local JSON + SFTP export)
- [ ] Collect 30+ days of baseline data
- [ ] Monitor for infrastructure issues (network instability, disk space, false positives)

**Deliverable**: Raw honeypot logs (private, not published yet)

---

### Phase 2: Data Analysis & Feature Extraction (August 2026) 🔜 PLANNED
- [ ] Build data parsing pipeline (`cowrie_parser.py`)
- [ ] Implement IP enrichment (GeoIP, ASN lookup, abuse DB checks)
- [ ] Extract session-level features (duration, command diversity, entropy)
- [ ] Map commands to MITRE ATT&CK techniques
- [ ] Classify sessions into behavioral archetypes (Scanner/Script Kiddie/Methodical Operator)

**Deliverable**: Structured dataset (sessions.csv + commands.csv)

---

### Phase 3: Dataset Publication & Research Output (September 2026) 🔜 PLANNED
- [ ] Anonymize dataset (mask residential IPs, remove internal references)
- [ ] Publish dataset to Zenodo with DOI
- [ ] Write behavioral analysis report (`R02_REPORT.md`)
- [ ] Update this repository with empirical findings
- [ ] Submit workshop paper (ACM MTD or USENIX CSET)

**Deliverable**: Public dataset (Zenodo) + analysis report (GitHub) + paper submission

---

## Expected Outputs

### Immediate (Available Now)
- ✅ Deployment methodology documentation
- ✅ Data schema specification
- ✅ Constraint and limitation documentation
- ✅ Research hypothesis framework

### After Data Collection (Q3 2026)
- 🔜 Anonymized honeypot dataset (DOI-citable)
- 🔜 ATT&CK technique frequency analysis
- 🔜 Attacker behavioral archetype classification
- 🔜 Broadcast-specific observations (if any)
- 🔜 Workshop/conference paper

---

## How to Use This Repository

### For Researchers
- **Before Deployment**: Review methodology and constraints to understand design decisions
- **During Collection**: Monitor GitHub issues for status updates
- **After Publication**: Download dataset from Zenodo link (will be added to README when available)

### For Broadcast Infrastructure Operators
- **Methodology**: Adapt this deployment approach to your own network
- **Constraints**: Compare your constraints to ours (hardware, uptime, ISP behavior)
- **Schema**: Use our data schema for your own honeypot logs (enables cross-network comparison)

### For Cybersecurity Practitioners
- **Benchmarking**: Compare your honeypot data to ours (when published) for regional/domain differences
- **ATT&CK Mapping**: Use our command→technique lookup table for your own logs

---

## Contributing

**During Phase 0 (Pre-Deployment):**
- Questions about methodology → Open a GitHub Issue
- Suggestions for schema improvements → Open a GitHub Issue or Pull Request
- Deployment experiences in similar environments → Open a Discussion

**After Phase 1 (Data Collection):**
- Issues will be locked during data collection to avoid methodology changes mid-study
- Re-opened after dataset publication for analysis feedback

---

## Transparency Commitment

This repository will maintain:
- ✅ Honest status updates (no "coming soon" promises without timelines)
- ✅ Clear documentation of what is planned vs completed
- ✅ Limitations acknowledged upfront (not hidden in footnotes)
- ✅ Null results published if hypotheses are rejected (no publication bias)

**If the deployment fails or produces zero useful data, that will be documented honestly.**

---

## Contact & Collaboration

Interested in:
- Deploying similar honeypots in other broadcast networks?
- Comparing datasets across different critical infrastructure domains?
- Extending this methodology to other protocols (RTMP, encoder APIs)?

Open a GitHub Issue tagged "collaboration" or contact directly:

Hassan El Hajj  
Independent Researcher, Beirut, Lebanon  
ORCID: 0009-0001-2590-1089

---

**Roadmap Version**: v0.1 (May 2026)  
**Next Update**: After Phase 1 deployment (planned July 2026)
