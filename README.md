# Broadcast Infrastructure Honeypot Deployment: Methodology Documentation

## Purpose
This repository documents the deployment methodology, operational constraints, and data schema for a planned SSH/Telnet honeypot deployment on an IP-based broadcast network in Lebanon. The goal is to enable reproducible research and capture domain-specific attacker behavioral data from operational broadcast infrastructure.

## Status: Pre-Deployment Methodology
**Current Phase**: Methodology documentation (May 2026)  
**Planned Deployment**: June 2026  
**Data Collection**: 30+ days minimum before dataset publication

This documentation is being written **before** honeypot deployment to establish:
- Clear operational constraints and isolation design
- Structured data schema for behavioral analysis
- Reproducibility framework for other broadcast infrastructure operators

## Context
- **Environment**: Operational broadcast IP network (600+ endpoints, 24/7 live playout)
- **Technology**: Cowrie SSH/Telnet honeypot on isolated VLAN
- **Geography**: Lebanon (CGNAT-heavy regional ISP topology, limited geolocation precision)
- **Operational Constraint**: ≥99.9% uptime requirement prohibits active scanning or production network interference

## Repository Contents
| File | Description |
|------|-------------|
| `topology.md` | Sanitized network diagram & VLAN isolation design |
| `constraints.md` | Operational, hardware, and regional constraints |
| `schema.md` | Planned data schema for session/command logs |
| `preliminary_observations.md` | Research hypotheses and expected limitations |
| `CITATION.cff` | Machine-readable citation metadata |

## Research Contribution
This deployment addresses a gap in cyber deception research: **no published honeypot datasets exist from operational broadcast media infrastructure**. Academic honeypot studies typically deploy in cloud environments (AWS, Azure) or university networks. This deployment captures attacker behavior in the context of:
- Broadcast-specific protocols (RTMP, HLS, encoder management APIs)
- Resource-constrained infrastructure (single managed switch per site)
- Middle East regional threat landscape (Lebanon-based network)

## Planned Output
After 30+ days of data collection:
- Anonymized dataset published to Zenodo (DOI-citable)
- Behavioral analysis report on GitHub
- ATT&CK technique frequency analysis

## Contact
Hassan El Hajj  
Independent Researcher, Beirut, Lebanon  
ORCID: 0009-0001-2590-1089  
GitHub: hhajj

## License
Methodology documentation: CC BY 4.0  
Future dataset: CC0 1.0 (public domain dedication)
