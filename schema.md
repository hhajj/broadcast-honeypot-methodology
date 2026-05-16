# Data Schema: Honeypot Session & Command Logs
## `sessions.csv`
| Field | Type | Notes |
|-------|------|-------|
| `session_id` | UUIDv4 | Unique per connection |
| `timestamp_utc` | ISO8601 | Millisecond precision |
| `src_ip` | IPv4 | Anonymized /24 suffix masked if residential |
| `country_code` | ISO 3166-2 | Reliable outside Lebanon |
| `asn` | STRING | Hosting/ISP identifier |
| `attacker_archetype` | ENUM | `scanner` / `credential_harvester` / `unknown` |

## `commands.csv`
| Field | Type | Notes |
|-------|------|-------|
| `command_id` | UUIDv4 | Unique per execution |
| `session_id` | UUIDv4 | FK → sessions |
| `command_raw` | TEXT | Exact input |
| `attck_techniques` | JSON ARRAY | e.g., `["T1078", "T1021"]` |
| `execution_outcome` | ENUM | `success` / `blocked` / `simulated` |
