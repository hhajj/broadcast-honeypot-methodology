# Sanitized Network Topology
[Internet] → [Edge Firewall] → [Broadcast Core Switch]
                                  ├── VLAN 10: Production (Encoders, Playout, NLE)
                                  └── VLAN 50: Isolated Research → [Cowrie Instance]

## Isolation Principles
- No shared routing tables between VLAN 50 and production
- Zero SPAN/mirroring on production switches
- Local JSON/Syslog logging → SFTP export to air-gapped archive
- All device models, firmware, and internal IPs anonymized
