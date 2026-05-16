# Data Schema: Honeypot Session & Command Logs

## Overview
This schema captures both raw honeypot events and derived behavioral features for attacker classification and ATT&CK technique mapping.

---

## `sessions.csv` — Session-Level Records

| Field | Type | Description |
|-------|------|-------------|
| `session_id` | UUID | Primary key, unique per SSH/Telnet connection |
| `src_ip` | IPv4 | Source IP address (last octet masked if residential) |
| `src_country` | ISO 3166-1 alpha-2 | Country code via GeoIP lookup |
| `src_asn` | String | Autonomous System Number (ISP/hosting provider identifier) |
| `src_organization` | String | Organization name from IP enrichment API |
| `is_tor_exit` | Boolean | True if source IP is a Tor exit node |
| `is_known_malicious` | Boolean | True if flagged in AbuseIPDB or similar reputation service |
| `timestamp_start` | ISO 8601 | Session start time (UTC, millisecond precision) |
| `timestamp_end` | ISO 8601 | Session end time (UTC) |
| `duration_seconds` | Float | Total session duration |
| `n_login_attempts` | Integer | Number of authentication attempts |
| `n_successful_logins` | Integer | Number of successful authentications (typically 0 or 1) |
| `credentials_tried` | JSON Array | List of `{"username": "...", "password": "..."}` pairs |
| `n_commands` | Integer | Total commands executed after successful login |
| `unique_commands` | Integer | Number of distinct commands executed |
| `mean_time_between_commands_ms` | Float | Average inter-command delay (human vs automated indicator) |
| `command_diversity_entropy` | Float | Shannon entropy of command sequence (0-1 normalized) |
| `max_directory_depth` | Integer | Deepest filesystem path reached via `cd` commands |
| `files_downloaded` | JSON Array | List of `{"filename": "...", "url": "...", "method": "wget|curl"}` |
| `lateral_movement_indicators` | Boolean | True if session contains `ping`, `nc`, `ssh`, or network scan commands |
| `attacker_archetype` | Enum | `Scanner` / `Script_Kiddie` / `Methodical_Operator` / `Unclassified` (see classification methodology below) |

---

## `commands.csv` — Command-Level Records

| Field | Type | Description |
|-------|------|-------------|
| `command_id` | UUID | Primary key, unique per command execution |
| `session_id` | UUID | Foreign key → `sessions.session_id` |
| `sequence_number` | Integer | Command order within session (1-indexed) |
| `timestamp_utc` | ISO 8601 | Command execution time (millisecond precision) |
| `command_raw` | Text | Exact command input as typed by attacker |
| `command_normalized` | String | Command name only (first token, e.g., `ls` from `ls -la /etc`) |
| `attck_techniques` | JSON Array | List of MITRE ATT&CK technique IDs (e.g., `["T1033", "T1083"]`) |
| `attck_tactics` | JSON Array | List of MITRE ATT&CK tactic names (e.g., `["Discovery", "Collection"]`) |
| `execution_outcome` | Enum | `success` / `command_not_found` / `permission_denied` / `simulated` |
| `output_length_bytes` | Integer | Length of command output returned to attacker |

---

## Attacker Archetype Classification Methodology

Sessions are classified into archetypes using feature vectors and k-means clustering (k=3):

| Archetype | Behavioral Signature |
|-----------|---------------------|
| **Scanner** | High speed (>5 login attempts/second), zero post-login interaction, duration <30 seconds, credential spray pattern |
| **Script_Kiddie** | Moderate speed, runs known exploit commands (`wget` malware URLs, automated scripts), attempts file download, duration 1-10 minutes |
| **Methodical_Operator** | Slow typing (>2 seconds mean inter-command delay), systematic directory traversal, reconnaissance commands (`whoami`, `uname`, `find`), duration >10 minutes, low credential variety |
| **Unclassified** | Does not fit clear archetype pattern or insufficient data for classification |

Classification validated via manual inspection of 20 randomly sampled sessions per cluster.

---

## ATT&CK Technique Mapping

Commands are mapped to MITRE ATT&CK Enterprise techniques using a manually curated lookup table:

| Command Example | ATT&CK Technique | Tactic |
|----------------|-----------------|--------|
| `whoami`, `id` | T1033 (System Owner/User Discovery) | Discovery |
| `wget http://...`, `curl -O ...` | T1105 (Ingress Tool Transfer) | Command and Control |
| `cat /etc/passwd`, `cat /etc/shadow` | T1003.008 (OS Credential Dumping: /etc/passwd) | Credential Access |
| `find / -name "*.conf"` | T1083 (File and Directory Discovery) | Discovery |
| `nmap`, `ping`, `nc` | T1018 (Remote System Discovery) | Discovery |

Full mapping table published as `attck_command_map.yaml` in repository.

---

## Data Anonymization & Privacy

- **IP Addresses**: Last octet masked (e.g., `192.168.1.x`) if source is residential ISP; hosting/cloud IPs published unmasked
- **Internal Network References**: All internal IP addresses, hostnames, and organizational identifiers removed or replaced with placeholders
- **Credentials**: Actual passwords retained for frequency analysis (common patterns like `admin/admin`, `root/password`), but no personally identifiable passwords (names, email addresses) included
- **Ethical Consideration**: All connections are to a honeypot (fake system), not production infrastructure — no real user data is captured

---

## Schema Version

**v0.1** — Pre-deployment specification (May 2026)  
**Status**: Subject to minor refinement after initial data collection  
**Backward Compatibility**: Maintained via schema versioning in exported CSV headers
