# MITRE ATT&CK Oracle HUD

Interactive MITRE ATT&CK Enterprise explorer — 14 tactics and 42 techniques
mapped to NIST CSF 2.0 and CIS Controls v8, with reference Sigma/KQL/SPL
detection logic and analyst notes for every technique.

**Live:** https://mrbenl.github.io/mitre-attack-oracle/

## About

A three-layer teaching reference for adversary behavior:

- **Tactics** — the 14 strategic objectives of the ATT&CK matrix, phase-coded
  from pre-attack through action-on-objective
- **Techniques** — 42 techniques with platforms, observed threat actors,
  data sources, and primary mitigations
- **Forensic Vault** — reference Sigma, KQL, and SPL detection rules with
  analyst notes on tuning and false-positive sources

Every technique is mapped to NIST CSF 2.0 subcategories and CIS Controls v8
safeguards.

## Framework baseline

| Framework | Version this project maps to |
|---|---|
| MITRE ATT&CK Enterprise | **v18.1** (28 Oct 2025 – 27 Apr 2026) |
| NIST CSF | 2.0 |
| CIS Controls | v8 |

### Known divergence from current ATT&CK

ATT&CK **v19** (28 Apr 2026) split the Defense Evasion tactic. This project
deliberately stays on v18.1 rather than tracking that restructure, so three
things differ from the current catalog:

- **TA0005** is presented as *Defense Evasion*; in v19 it is renamed **Stealth**
- **TA0112 Defense Impairment** (v19, 18 techniques) is not represented here
- Two techniques were revoked in v19 and have superseding IDs:

  | Here (v18.1) | v19 replacement |
  |---|---|
  | `T1562.001` Impair Defenses: Disable or Modify Tools | `T1685` Disable or Modify Tools (TA0112) |
  | `T1070.001` Indicator Removal: Clear Windows Event Logs | `T1685.005` Clear Windows Event Logs (TA0112) |

Revocation is a taxonomy change, not a behavioral one — the adversary
techniques and the detection logic for them are unaffected. Only the
identifiers moved.

## Validation status

The detection content is verified to differing depths. Be aware of which is
which before deploying anything here.

| Content | State |
|---|---|
| 25 Sigma rules | Validated with **pySigma 3.1.0**; all 25 parse and convert to SPL |
| 9 KQL queries | Parsed with the **Kusto language service** against declared table schemas — tables, columns, function names and arity all resolve |
| 5 SPL queries | **Not machine-validated** — no public SPL parser exists |
| 1 YARA rule | **Not machine-validated** |

No query in this repository has been executed against a live Splunk, Sentinel
or Defender tenant. Thresholds are illustrative starting points, not tuned
values, and several rules contain placeholders (for example the TLS rule's
`Zeek_ssl_CL` table) that must be pointed at your own data sources.

## Credits

**Architect:** Ben Luthy — CISSP, CISM, CDPSE · [LinkedIn](https://www.linkedin.com/in/benjaminluthy/)

**Builder:** Claude AI
