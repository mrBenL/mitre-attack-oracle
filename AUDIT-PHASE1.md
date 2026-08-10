# Phase 1 Correctness Audit: MITRE ATT&CK Oracle HUD vs ATT&CK Enterprise v19

Date: 2026-08-09
Ground truth: `mitre-attack/attack-stix-data` Enterprise bundle `enterprise-attack-19.0.json`
(x_mitre_version 19.0, modified 2026-04-28T14:00:00.188Z, 25,844 STIX objects).
No edits were made to index.html in this phase.

## Method

- Parsed the `fullMitreData` model out of index.html (15 tactics, 42 technique entries) and
  cross-checked every tactic ID, technique ID, name, and tactic-to-technique assignment
  against the STIX bundle (kill_chain_phases, revoked, x_mitre_deprecated, revoked-by
  relationships).
- Verified NIST CSF 2.0 subcategory IDs against the official NIST CPRT JSON export for
  CSF 2.0 (csrc.nist.gov/extensions/nudp, doc CSF_2_0_0).
- Verified CIS v8 safeguard numbers against the CIS Controls Navigator
  (cisecurity.org/controls/cis-controls-navigator).
- Validated all 25 Sigma rules by parsing AND converting to SPL with pySigma 1.5.0 +
  pysigma-backend-splunk (including both multi-document correlation collections).
- Compiled the YARA rule with yara-python 4.5.4 and confirmed it matches a synthetic PE.
- Manually reviewed the 9 KQL and 5 SPL queries for syntax (no offline parser exists for
  either dialect).

## Findings requiring fixes (Phase 2)

**F1. T1555.003 technique name is outdated.**
HUD: "Credentials from Password Stores: Browsers".
v19 STIX: sub-technique name is "Credentials from Web Browsers"
(combined form: "Credentials from Password Stores: Credentials from Web Browsers").

**F2. T1048.003 technique name is outdated.**
HUD: "Exfiltration Over Alternative Protocol: Unencrypted Non-C2".
v19 STIX: name is "Exfiltration Over Unencrypted Non-C2 Protocol"
(still a sub-technique of T1048 "Exfiltration Over Alternative Protocol").

**F3. The Sigma verify note cites a pySigma version that does not exist.**
The `verifyStatus` SIGMA entry says "Parses and converts to SPL with pySigma 3.1.0".
PyPI's latest release is 1.5.0; no 3.x release exists. The substantive claim is true:
all 25 rules parse and convert to SPL, reproduced in this audit with pySigma 1.5.0.
Fix is to correct the version string.

## Verified correct

**Tactics.** All 15 tactic IDs and names match v19 exactly, including the split:
TA0005 "Stealth" (shortname `stealth`) and TA0112 "Defense Impairment"
(shortname `defense-impairment`) both exist in the bundle. The intro's claim that the
split shipped in v19 on 28 Apr 2026 matches the bundle's version and modified date.

**Stealth / Defense Impairment placements.** T1027 carries kill-chain phase `stealth`.
T1685 and T1685.005 carry `defense-impairment`. All correct. The re-parenting claims in
the copy are confirmed by STIX revoked-by relationships: T1562.001 "Disable or Modify
Tools" is revoked by T1685 (T1562 "Impair Defenses" is also revoked by T1685), and
T1070.001 "Clear Windows Event Logs" is revoked by T1685.005. T1070 "Indicator Removal"
itself remains active under Stealth.

**Technique assignments.** All 42 technique entries sit under a tactic that v19 assigns
them to. Multi-tactic techniques check out (T1078, T1053.005, T1547.001, T1134, T1055).

**Revoked/deprecated.** None of the 42 techniques in the HUD is revoked or deprecated
in v19. The only revoked IDs mentioned anywhere (T1562.001, T1070.001) appear correctly,
described as historical predecessors.

**Counts in copy.** "15 tactics, 222 techniques and 475 sub-techniques" matches the
active-object counts in the v19 bundle exactly. The top bar's "42 TECHNIQUES" matches
the curated set.

**NIST CSF 2.0.** All 23 subcategory IDs used exist in the official CSF 2.0 core
(ID.AM-04, ID.RA-01, ID.RA-03, PR.AA-01/02/03/05, PR.AT-01/02, PR.DS-01/02/11,
PR.IR-01, PR.PS-01/02/04, DE.AE-02/03, DE.CM-01/03/09, RC.RP-01, RS.MI-02).
Formatting is correct throughout.

**CIS v8.** All 44 safeguard numbers used exist, and the abbreviated titles in the HUD
correspond to the official safeguard titles (spot-checked all 44, e.g. 3.14 "Log
Sensitive Data Access", 16.13 "Conduct Application Penetration Testing").

**Mitigation IDs (bonus check).** All 29 M-codes referenced in the enrichment data
exist in v19 and none is deprecated. T1098, referenced in the T1136.001 rule tags,
is active.

**Detection rules.**
- Sigma (25): all parse and convert to SPL with pySigma 1.5.0. The two correlation
  collections (T1595.002, T1087.002) parse and convert as correlations.
- YARA (1): compiles with yara-python 4.5.4 and matches a synthetic PE containing two
  of its strings, consistent with the verify note.
- KQL (9): manual review found no syntax errors. Notably the T1078 geo-velocity query
  uses the correct geo_distance_2points argument order (lon, lat, lon, lat) and calls
  prev() only after sort by, which serializes the row set.
- SPL (5): manual review found no syntax errors. The eval string concatenation with
  the "." operator in T1048.003 is valid SPL.
- BASH / ANALYST WORKFLOW entries are not detection rules and are already labeled
  unverified in the UI.

## Observations, not errors (Phase 3 candidates)

- Sub-technique naming style is inconsistent: some entries use the bare sub-technique
  name ("Vulnerability Scanning"), others use the "Parent: Sub" combined form. Both are
  defensible; consistency would be cosmetic.
- The NIST and CIS descriptive strings are abbreviated paraphrases of the official
  titles. IDs are all valid; the paraphrasing was not treated as an error.
- Sigma tags `attack.stealth` and `attack.defense_impairment` are consistent with the
  v19 tactic shortnames.
- The T1134 rule tags `attack.t1134.003` while the entry is T1134. This is deliberate
  and documented in the analyst note.
- The KQL verify note claims validation "by the Kusto language service". There is no
  offline Kusto parser, so that claim could not be reproduced here; manual review found
  no issues.
- Threat-actor attributions in the enrichment data were not audited in this phase
  (out of the Phase 1 scope) and should be treated as unverified.
