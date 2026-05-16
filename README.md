# Broadcast Infrastructure Honeypot Methodology Stub

## Purpose
Documents deployment methodology, constraints, and data schema for a live cyber deception environment on an IP-based broadcast network in Lebanon. Enables reproducible research and domain-specific behavioral analysis.

## Context
- **Environment**: Operational broadcast IP network (600+ endpoints, 24/7 playout)
- **Honeypot**: Cowrie SSH/Telnet on isolated VLAN (no production routing)
- **Geography**: Lebanon (CGNAT-heavy, regional ISP topology)
- **Uptime**: ≥99.9% (prohibits active scanning/mirroring on production)

## Repository Contents
| File | Description |
|------|-------------|
| `topology.md` | Sanitized network diagram & isolation design |
| `constraints.md` | Operational & regional constraints |
| `schema.md` | Structured session/command log schema |
| `preliminary_observations.md` | Baseline findings & hypotheses |
| `CITATION.cff` | Machine-readable citation metadata |

## Status
`v0.1` — Baseline collection phase. Methodology locked. Dataset publication pending 30+ days telemetry.

## Contact
Hassan El Hajj | Independent Researcher, Beirut, Lebanon | ORCID: 0009-0001-2590-1089
