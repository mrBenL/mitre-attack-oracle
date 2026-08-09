# MITRE ATT&CK Oracle HUD

Interactive MITRE ATT&CK Enterprise explorer — 15 tactics and 42 techniques
mapped to NIST CSF 2.0 and CIS Controls v8, with reference Sigma/KQL/SPL
detection logic and analyst notes for every technique.

**Live:** https://mrbenl.github.io/mitre-attack-oracle/

## About

A three-layer teaching reference for adversary behavior:

- **Tactics** — the 15 strategic objectives of the ATT&CK matrix, phase-coded
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
| MITRE ATT&CK Enterprise | **v19** (28 Apr 2026) |
| NIST CSF | 2.0 |
| CIS Controls | v8 |

### The v19 tactic split

ATT&CK v19 split the old **Defense Evasion** tactic in two, taking Enterprise
from 14 tactics to 15. This project tracks that split, because a course
companion teaching the retired 14-tactic model sends students into interviews
and certification exams with the wrong matrix in their head.

| v18.1 and earlier | v19 |
|---|---|
| **TA0005** Defense Evasion | **TA0005 Stealth** — hiding activity inside legitimate behavior |
| *(same tactic)* | **TA0112 Defense Impairment** — actively breaking security controls |

The distinction is operational, not cosmetic: blending in and switching the
alarms off call for different detection strategies, so they are now different
tactics.

Two techniques in this project moved as part of the split. The adversary
behavior and the detection logic are unchanged — only the identifiers moved:

| Old ID | v19 ID |
|---|---|
| `T1562.001` Impair Defenses: Disable or Modify Tools | `T1685` Disable or Modify Tools (TA0112) |
| `T1070.001` Indicator Removal: Clear Windows Event Logs | `T1685.005` Clear Windows Event Logs (TA0112) |

Sigma `tags:` were updated to match — `attack.defense_evasion` is replaced by
`attack.stealth` or `attack.defense_impairment` depending on the rule.

**Uneven coverage note.** Every other tactic here carries three techniques.
The split left **Stealth with one** (`T1027`) and **Defense Impairment with
two**, because the three techniques that were under Defense Evasion did not
divide evenly. This is a curated teaching subset, not a mirror of the full
matrix — the real v19 Enterprise catalog holds 222 techniques and 475
sub-techniques across the 15 tactics.

## Validation status

The detection content is verified to differing depths. Be aware of which is
which before deploying anything here.

| Content | State |
|---|---|
| 25 Sigma rules | Validated with **pySigma 1.5.0** + pysigma-backend-splunk 2.1.0; all 25 parse and convert to SPL. Re-run after the v19 tag change. |
| 9 KQL queries | Parsed with the **Kusto language service** against declared table schemas — tables, columns, function names and arity all resolve |
| 1 YARA rule | Compiled with **yara-python 4.5.4**; matches a synthetic PE test case |
| 5 SPL queries | **Not machine-validated** — Splunk ships no public SPL parser, making this the one dialect that cannot be checked offline |
| 2 reference workflows (bash, OSINT) | Not detection rules; not validated |

**This is surfaced in the app itself.** Every rule in the Forensic Vault
carries a verification banner stating whether a parser has checked it, so the
distinction is visible at the point of use rather than buried here.

No query in this repository has been executed against a live Splunk, Sentinel
or Defender tenant, and thresholds are illustrative starting points rather
than tuned values. Every query runs against real tables as written — there are
no placeholder table names.

## Credits

**Architect:** Ben Luthy — CISSP, CISM, CDPSE · [LinkedIn](https://www.linkedin.com/in/benjaminluthy/)

**Builder:** Claude AI
