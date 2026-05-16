# Network Topology (Sanitized)

```
[Internet] 
    ↓
[Edge Firewall]
    ↓
[Broadcast Core Switch]
    ├── VLAN 10: Production Network
    │   ├── Encoders (RTMP ingest, management web UIs)
    │   ├── Playout Servers (MagicSoft, automation)
    │   ├── NLE/Edit Workstations
    │   └── Streaming Origin Servers
    │
    └── VLAN 50: Isolated Research Network
        └── [Cowrie Honeypot Instance]
            - SSH: Port 22
            - Telnet: Port 23
            - Logging: Local JSON + Syslog
```

## Isolation Design Principles

### Network-Level Isolation
- **No Inter-VLAN Routing**: VLAN 50 (honeypot) cannot route to VLAN 10 (production) or vice versa
- **Dedicated VLAN**: Honeypot operates on isolated Layer 2 segment with no shared broadcast domain
- **Firewall Rules**: Edge firewall allows inbound SSH/Telnet to VLAN 50 only; all outbound from honeypot is blocked except logging

### Physical Isolation
- **No SPAN/Mirroring**: Production switch ports are NOT mirrored to honeypot (violates uptime constraint)
- **Separate Management**: Honeypot managed via out-of-band connection (SSH from internal management workstation, not production network)

### Data Isolation
- **Local Logging**: Cowrie logs written to local filesystem (`/var/log/cowrie/`)
- **Offline Export**: Logs transferred via SFTP to air-gapped archive server (manual weekly export during maintenance window)
- **No Real-Time Correlation**: Honeypot does not have real-time access to production logs; correlation performed offline post-collection

## Anonymization for Publication

All published topology diagrams and documentation:
- ✅ Generic device role names (Encoder, Playout Server) — not vendor models
- ✅ VLAN IDs changed from actual values
- ✅ Internal IP address ranges replaced with RFC 1918 examples
- ✅ No firmware versions, management interface screenshots, or configuration files
- ✅ Geographic location limited to country level (Lebanon) — no city or facility identifiers

## Why This Topology Matters for Research

**Key Constraint**: Single managed switch per site (broadcast infrastructure is resource-constrained, not enterprise data center)

**Implication**: Cannot deploy inline honeypot (e.g., network tap, transparent proxy) because:
- Only one physical switch available
- ≥99.9% uptime requirement prohibits experimental routing changes
- No budget for additional switching hardware

**Solution**: Honeypot deployed on isolated VLAN with public IP exposure, simulating a "backup encoder server" accessible from internet but not routable to production systems.

**Research Value**: This constraint-driven design is more representative of operational broadcast/OT environments than academic honeypot deployments in cloud VPCs with arbitrary network flexibility.

---

**Topology Version**: v0.1 (Pre-deployment specification, May 2026)
