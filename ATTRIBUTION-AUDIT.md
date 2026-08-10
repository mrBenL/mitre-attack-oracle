# Attribution Audit (Phase 3, Step 0): techEnrich actors vs ATT&CK Enterprise v19

Date: 2026-08-09
Ground truth: `enterprise-attack-19.0.json` (x_mitre_version 19.0, modified 2026-04-28).
Read-only audit. index.html was not modified.

## Method

- Extracted all 121 actor strings from the `techEnrich` map (42 techniques).
- Resolved each string against every active intrusion-set, campaign, malware, and tool
  object in the bundle, matching on canonical names and all aliases (`aliases` +
  `x_mitre_aliases`), case-insensitively, including 0/o spelling variants (Cl0p/Clop)
  and word-prefix forms (Sandworm -> Sandworm Team, Lumma -> Lumma Stealer).
- A pairing is CONFIRMED when the bundle contains an active `uses` relationship from
  the group to the exact technique ID or one of its sub-techniques, either directly or
  from a campaign attributed to that group (`attributed-to`). Campaign-derived
  confirmations are labeled as such, mirroring how attack.mitre.org renders group pages.
- UNSUPPORTED means the group exists in v19 but no such `uses` link exists.
- NONEXISTENT (as actor) means no group or campaign matches; most of these resolve to a
  malware/tool object instead, which is noted.
- GENERIC means the string is a descriptive phrase, not a named actor, and cannot be
  checked against STIX.

Important caveat, in both directions: a pairing being absent from ATT&CK does not prove
it false in the real world. It only means ATT&CK v19 does not document it. Nothing in
this report says an UNSUPPORTED claim is wrong; it says it is not sourceable to the
bundle this project treats as ground truth. Equally, no row here was upgraded from
general knowledge; only the bundle was consulted.

## Summary

| Verdict | Count |
|---|---|
| CONFIRMED | 45 |
| UNSUPPORTED | 14 |
| NONEXISTENT (as actor) | 43 |
| GENERIC | 19 |
| Total | 121 |

Patterns worth knowing before deciding dispositions:

- Every ransomware-family name used as an actor (LockBit, BlackCat/ALPHV, Royal, Conti,
  Ryuk, Black Basta, Cl0p, NotPetya) exists in v19 only as a malware object, not as a
  group. ATT&CK models these as software used by groups (e.g. Wizard Spider G0102 uses
  Ryuk and Conti). The same applies to botnet/loader families (Emotet, TrickBot, QakBot,
  IcedID) and to Cobalt Strike (S0154, software). Where the software object itself has a
  `uses` link to the technique, the row notes it; that is software-to-technique, not an
  actor attribution.
- 'Sandworm' resolves to Sandworm Team (G0034) and both its rows are CONFIRMED; the HUD
  name is just short of the canonical form.
- Several APT29 rows are CONFIRMED only via the SolarWinds Compromise campaign (C0024),
  which ATT&CK attributes to APT29.
- 14 rows are UNSUPPORTED: real v19 groups with no documented link to the technique they
  are listed under. Three of them (APT29 on T1685.005, T1550.002, T1573.002) do have a
  documented link to the parent technique, noted per row.

## Interpretation: what each bucket means for a learner

The value of this audit is not a perfect score. It is the method: every claim was forced
to trace to a named object and relationship in the ground-truth bundle, and the failures
sort into instructive categories. That gap-spotting discipline is what this section
teaches.

**CONFIRMED is solid ground.** The group exists in v19 and ATT&CK documents a uses
relationship to the exact technique or a sub-technique of it. These claims carry a
citation you can hand to anyone: a STIX relationship in the official bundle. Treat them
as the baseline standard every attribution in a teaching tool should meet. Confirmed
means sourced, not that unconfirmed pairings are false; some real tradecraft simply
isn't recorded in the bundle.

**UNSUPPORTED is a v19 gap, not a verdict on reality.** The actor is a real, documented
v19 group; the specific pairing is what the bundle does not record. ATT&CK only documents
what public reporting lets it cite, so real tradecraft can be missing. Absence from
ATT&CK does not equal false in the real world. It does mean a teaching tool cannot
present the pairing as documented, which is why these rows needed independent
corroboration or a cut.

**NONEXISTENT (as actor) is a taxonomy mismatch, not a factual error about threats.**
Groups use malware; ATT&CK models the groups (intrusion sets) and the malware (software)
as separate object types. "LockBit uses T1486" fails as an actor claim because LockBit is
a software object, while the same behavior is documented as a malware-uses-technique
link. The fix is relabeling to the right object type, not deleting the knowledge.

**GENERIC is unverifiable without specificity.** Phrases like "most ransomware ops" name
no actor, so no bundle object can confirm or refute them. They may be fair
characterizations, but they are editorial, and a reader should be able to tell editorial
texture from sourced attribution at a glance.

Dispositions for every non-CONFIRMED row were later decided in DISPOSITION.md and
applied to the HUD, which now renders each surviving chip with its basis visible.

## Full results

Suggested v19 name is filled only where the HUD name differs from the canonical
group name. No renames have been applied.

| # | Technique | HUD actor (as written) | v19 resolution | Verdict | Suggested v19 name | Notes |
|---|---|---|---|---|---|---|
| 1 | T1595.002 Vulnerability Scanning | APT28 (GRU) | APT28 (G0007) | CONFIRMED |  | direct uses: T1595.002 |
| 2 | T1595.002 Vulnerability Scanning | APT41 | APT41 (G0096) | CONFIRMED |  | direct uses: T1595.002 |
| 3 | T1595.002 Vulnerability Scanning | Volt Typhoon | Volt Typhoon (G1017) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. |
| 4 | T1589.002 Email Addresses | APT29 (SVR) | APT29 (G0016) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. |
| 5 | T1589.002 Email Addresses | FIN7 | FIN7 (G0046) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. |
| 6 | T1589.002 Email Addresses | Lazarus Group | Lazarus Group (G0032) | CONFIRMED |  | direct uses: T1589.002 |
| 7 | T1590 Gather Victim Network Information | APT1 (PLA 61398) | APT1 (G0006) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. |
| 8 | T1590 Gather Victim Network Information | Sandworm | Sandworm Team (G0034) | CONFIRMED | Sandworm Team (G0034) | direct uses: T1590.001 |
| 9 | T1590 Gather Victim Network Information | Mustang Panda | Mustang Panda (G0129) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. |
| 10 | T1583.001 Domains | APT29 | APT29 (G0016) | CONFIRMED |  | via attributed campaign: T1583.001 (Operation Ghost (C0023); SolarWinds Compromise (C0024)) |
| 11 | T1583.001 Domains | Scattered Spider (UNC3944) | Scattered Spider (G1015) | CONFIRMED |  | direct uses: T1583.001 |
| 12 | T1583.001 Domains | Conti operators | Conti (S0575) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0575 'Conti' (malware). |
| 13 | T1588.002 Tool | LockBit affiliates | LockBit 3.0 (S1202) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1202 'LockBit 3.0' (malware); S1199 'LockBit 2.0' (malware). |
| 14 | T1588.002 Tool | Black Basta | Black Basta (S1070) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1070 'Black Basta' (malware). |
| 15 | T1588.002 Tool | Most ransomware crews | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 16 | T1587.001 Malware | Lazarus Group | Lazarus Group (G0032) | CONFIRMED |  | direct uses: T1587.001 |
| 17 | T1587.001 Malware | APT41 | APT41 (G0096) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. |
| 18 | T1587.001 Malware | Turla | Turla (G0010) | CONFIRMED |  | direct uses: T1587.001 |
| 19 | T1566.001 Spearphishing Attachment | APT29 | APT29 (G0016) | CONFIRMED |  | direct uses: T1566.001 |
| 20 | T1566.001 Spearphishing Attachment | TA505 | TA505 (G0092) | CONFIRMED |  | direct uses: T1566.001 |
| 21 | T1566.001 Spearphishing Attachment | Emotet operators | Emotet (S0367) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0367 'Emotet' (malware). |
| 22 | T1190 Exploit Public-Facing Application | Cl0p (MOVEit) | Clop (S0611) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0611 'Clop' (malware). |
| 23 | T1190 Exploit Public-Facing Application | Volt Typhoon (Fortinet) | Volt Typhoon (G1017) | CONFIRMED |  | direct uses: T1190 |
| 24 | T1190 Exploit Public-Facing Application | APT41 | APT41 (G0096) | CONFIRMED |  | direct uses: T1190 |
| 25 | T1078 Valid Accounts | Scattered Spider | Scattered Spider (G1015) | CONFIRMED |  | direct uses: T1078 |
| 26 | T1078 Valid Accounts | APT29 | APT29 (G0016) | CONFIRMED |  | direct uses: T1078 |
| 27 | T1078 Valid Accounts | Most ransomware ops | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 28 | T1059.001 PowerShell | APT29 | APT29 (G0016) | CONFIRMED |  | direct uses: T1059.001 |
| 29 | T1059.001 PowerShell | FIN7 | FIN7 (G0046) | CONFIRMED |  | direct uses: T1059.001 |
| 30 | T1059.001 PowerShell | Turla | Turla (G0010) | CONFIRMED |  | direct uses: T1059.001 |
| 31 | T1059.001 PowerShell | Ryuk operators | Ryuk (S0446) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0446 'Ryuk' (malware). |
| 32 | T1053.005 Scheduled Task | APT29 | APT29 (G0016) | CONFIRMED |  | direct uses: T1053.005 |
| 33 | T1053.005 Scheduled Task | APT41 | APT41 (G0096) | CONFIRMED |  | direct uses: T1053.005 |
| 34 | T1053.005 Scheduled Task | Conti | Conti (S0575) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0575 'Conti' (malware). |
| 35 | T1204.002 Malicious File | Qakbot | QakBot (S0650) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0650 'QakBot' (malware). |
| 36 | T1204.002 Malicious File | IcedID | IcedID (S0483) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0483 'IcedID' (malware). |
| 37 | T1204.002 Malicious File | Emotet | Emotet (S0367) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0367 'Emotet' (malware). |
| 38 | T1547.001 Registry Run Keys / Startup Folder | TrickBot / Qakbot | TrickBot (S0266) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0266 'TrickBot' (malware); S0650 'QakBot' (malware). |
| 39 | T1547.001 Registry Run Keys / Startup Folder | APT29 | APT29 (G0016) | CONFIRMED |  | direct uses: T1547.001 |
| 40 | T1547.001 Registry Run Keys / Startup Folder | Emotet | Emotet (S0367) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0367 'Emotet' (malware). |
| 41 | T1505.003 Web Shell | HAFNIUM (Exchange) | HAFNIUM (G0125) | CONFIRMED |  | direct uses: T1505.003 |
| 42 | T1505.003 Web Shell | Cl0p (MOVEit) | Clop (S0611) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0611 'Clop' (malware). |
| 43 | T1505.003 Web Shell | Volt Typhoon | Volt Typhoon (G1017) | CONFIRMED |  | direct uses: T1505.003 |
| 44 | T1136.001 Local Account | LockBit affiliates | LockBit 3.0 (S1202) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1202 'LockBit 3.0' (malware); S1199 'LockBit 2.0' (malware). |
| 45 | T1136.001 Local Account | APT41 | APT41 (G0096) | CONFIRMED |  | direct uses: T1136.001 |
| 46 | T1136.001 Local Account | Most ransomware crews | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 47 | T1134 Access Token Manipulation | APT28 | APT28 (G0007) | CONFIRMED |  | direct uses: T1134.001 |
| 48 | T1134 Access Token Manipulation | FIN7 | FIN7 (G0046) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. |
| 49 | T1134 Access Token Manipulation | Turla | Turla (G0010) | CONFIRMED |  | direct uses: T1134.002 |
| 50 | T1068 Exploitation for Privilege Escalation | LockBit | LockBit 3.0 (S1202) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1202 'LockBit 3.0' (malware); S1199 'LockBit 2.0' (malware). |
| 51 | T1068 Exploitation for Privilege Escalation | Volt Typhoon | Volt Typhoon (G1017) | CONFIRMED |  | direct uses: T1068 |
| 52 | T1068 Exploitation for Privilege Escalation | Most ransomware ops | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 53 | T1055 Process Injection | TrickBot | TrickBot (S0266) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0266 'TrickBot' (malware). |
| 54 | T1055 Process Injection | Emotet | Emotet (S0367) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0367 'Emotet' (malware). |
| 55 | T1055 Process Injection | Cobalt Strike operators | Cobalt Strike (S0154) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0154 'Cobalt Strike' (malware). |
| 56 | T1027 Obfuscated Files or Information | Near-universal across APT + crimeware | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 57 | T1685 Disable or Modify Tools | LockBit | LockBit 3.0 (S1202) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1202 'LockBit 3.0' (malware); S1199 'LockBit 2.0' (malware). |
| 58 | T1685 Disable or Modify Tools | BlackCat/ALPHV | BlackCat (S1068) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1068 'BlackCat' (malware). |
| 59 | T1685 Disable or Modify Tools | Royal | Royal (S1073) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1073 'Royal' (malware). |
| 60 | T1685.005 Clear Windows Event Logs | APT29 | APT29 (G0016) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. A uses link to parent T1685 does exist. |
| 61 | T1685.005 Clear Windows Event Logs | Most ransomware ops | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 62 | T1003.001 LSASS Memory | APT28 | APT28 (G0007) | CONFIRMED |  | direct uses: T1003.001 |
| 63 | T1003.001 LSASS Memory | APT29 | APT29 (G0016) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. |
| 64 | T1003.001 LSASS Memory | Nearly all ransomware ops | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 65 | T1558.003 Kerberoasting | APT29 | APT29 (G0016) | CONFIRMED |  | via attributed campaign: T1558.003 (SolarWinds Compromise (C0024)) |
| 66 | T1558.003 Kerberoasting | FIN7 | FIN7 (G0046) | CONFIRMED |  | direct uses: T1558.003 |
| 67 | T1558.003 Kerberoasting | LockBit affiliates | LockBit 3.0 (S1202) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1202 'LockBit 3.0' (malware); S1199 'LockBit 2.0' (malware). |
| 68 | T1555.003 Credentials from Web Browsers | Scattered Spider | Scattered Spider (G1015) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. |
| 69 | T1555.003 Credentials from Web Browsers | Lumma / RedLine operators | Lumma Stealer (S1213) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1213 'Lumma Stealer' (malware); S1240 'RedLine Stealer' (malware). |
| 70 | T1555.003 Credentials from Web Browsers | Infostealer affiliates | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 71 | T1087.002 Domain Account | APT29 | APT29 (G0016) | CONFIRMED |  | via attributed campaign: T1087.002 (SolarWinds Compromise (C0024)) |
| 72 | T1087.002 Domain Account | BlackCat/ALPHV | BlackCat (S1068) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1068 'BlackCat' (malware). |
| 73 | T1087.002 Domain Account | Conti (BloodHound heavy) | Conti (S0575) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0575 'Conti' (malware). |
| 74 | T1018 Remote System Discovery | APT41 | APT41 (G0096) | CONFIRMED |  | direct uses: T1018 |
| 75 | T1018 Remote System Discovery | Volt Typhoon | Volt Typhoon (G1017) | CONFIRMED |  | direct uses: T1018 |
| 76 | T1018 Remote System Discovery | Most ransomware ops | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 77 | T1046 Network Service Discovery | Most ransomware ops | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 78 | T1046 Network Service Discovery | APT41 | APT41 (G0096) | CONFIRMED |  | direct uses: T1046 |
| 79 | T1046 Network Service Discovery | Volt Typhoon | Volt Typhoon (G1017) | CONFIRMED |  | direct uses: T1046 |
| 80 | T1021.001 Remote Desktop Protocol | Ransomware ops (universal) | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 81 | T1021.001 Remote Desktop Protocol | Conti | Conti (S0575) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0575 'Conti' (malware). |
| 82 | T1021.001 Remote Desktop Protocol | Scattered Spider | Scattered Spider (G1015) | CONFIRMED |  | direct uses: T1021.001 |
| 83 | T1021.002 SMB/Windows Admin Shares | NotPetya | NotPetya (S0368) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0368 'NotPetya' (malware). |
| 84 | T1021.002 SMB/Windows Admin Shares | Ryuk | Ryuk (S0446) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0446 'Ryuk' (malware). |
| 85 | T1021.002 SMB/Windows Admin Shares | Conti | Conti (S0575) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0575 'Conti' (malware). |
| 86 | T1550.002 Pass the Hash | APT28 | APT28 (G0007) | CONFIRMED |  | direct uses: T1550.002 |
| 87 | T1550.002 Pass the Hash | APT29 | APT29 (G0016) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. A uses link to parent T1550 does exist. |
| 88 | T1550.002 Pass the Hash | Conti | Conti (S0575) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0575 'Conti' (malware). |
| 89 | T1114.002 Remote Email Collection | APT29 | APT29 (G0016) | CONFIRMED |  | direct uses: T1114.002 |
| 90 | T1114.002 Remote Email Collection | HAFNIUM | HAFNIUM (G0125) | CONFIRMED |  | direct uses: T1114.002 |
| 91 | T1114.002 Remote Email Collection | Ransomware exfil crews | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 92 | T1005 Data from Local System | Near-universal | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 93 | T1560.001 Archive via Utility | APT29 | APT29 (G0016) | CONFIRMED |  | via attributed campaign: T1560.001 (SolarWinds Compromise (C0024)) |
| 94 | T1560.001 Archive via Utility | Most ransomware ops | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 95 | T1560.001 Archive via Utility | APT41 | APT41 (G0096) | CONFIRMED |  | direct uses: T1560.001 |
| 96 | T1071.001 Web Protocols | APT29 | APT29 (G0016) | CONFIRMED |  | via attributed campaign: T1071.001 (SolarWinds Compromise (C0024)) |
| 97 | T1071.001 Web Protocols | Cobalt Strike operators | Cobalt Strike (S0154) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0154 'Cobalt Strike' (malware). |
| 98 | T1071.001 Web Protocols | Most crimeware | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 99 | T1573.002 Asymmetric Cryptography | APT29 | APT29 (G0016) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. A uses link to parent T1573 does exist. |
| 100 | T1573.002 Asymmetric Cryptography | Turla | Turla (G0010) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. |
| 101 | T1573.002 Asymmetric Cryptography | Advanced adversaries | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 102 | T1105 Ingress Tool Transfer | Near-universal post-exploitation | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 103 | T1041 Exfiltration Over C2 Channel | APT29 | APT29 (G0016) | UNSUPPORTED |  | Group exists in v19 but has no uses link to this technique or its sub-techniques. |
| 104 | T1041 Exfiltration Over C2 Channel | APT41 | APT41 (G0096) | CONFIRMED |  | via attributed campaign: T1041 (C0017 (C0017)) |
| 105 | T1041 Exfiltration Over C2 Channel | Cobalt Strike operators | Cobalt Strike (S0154) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0154 'Cobalt Strike' (malware). |
| 106 | T1567.002 Exfiltration to Cloud Storage | LockBit | LockBit 3.0 (S1202) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1202 'LockBit 3.0' (malware); S1199 'LockBit 2.0' (malware). |
| 107 | T1567.002 Exfiltration to Cloud Storage | BlackCat/ALPHV | BlackCat (S1068) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1068 'BlackCat' (malware). |
| 108 | T1567.002 Exfiltration to Cloud Storage | Cl0p | Clop (S0611) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0611 'Clop' (malware). |
| 109 | T1048.003 Exfiltration Over Unencrypted Non-C2 Protocol | APT32 (OceanLotus) | APT32 (G0050) | CONFIRMED |  | direct uses: T1048.003 |
| 110 | T1048.003 Exfiltration Over Unencrypted Non-C2 Protocol | OilRig | OilRig (G0049) | CONFIRMED |  | direct uses: T1048.003 |
| 111 | T1048.003 Exfiltration Over Unencrypted Non-C2 Protocol | Low-sophistication crews | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 112 | T1486 Data Encrypted for Impact | LockBit | LockBit 3.0 (S1202) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1202 'LockBit 3.0' (malware); S1199 'LockBit 2.0' (malware). |
| 113 | T1486 Data Encrypted for Impact | BlackCat/ALPHV | BlackCat (S1068) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1068 'BlackCat' (malware). |
| 114 | T1486 Data Encrypted for Impact | Royal | Royal (S1073) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1073 'Royal' (malware). |
| 115 | T1486 Data Encrypted for Impact | Conti | Conti (S0575) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0575 'Conti' (malware). |
| 116 | T1486 Data Encrypted for Impact | Ryuk | Ryuk (S0446) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0446 'Ryuk' (malware). |
| 117 | T1490 Inhibit System Recovery | All ransomware ops | no match | GENERIC |  | Descriptive phrase, not a named actor; not checkable against STIX. |
| 118 | T1490 Inhibit System Recovery | LockBit | LockBit 3.0 (S1202) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1202 'LockBit 3.0' (malware); S1199 'LockBit 2.0' (malware). |
| 119 | T1490 Inhibit System Recovery | BlackCat | BlackCat (S1068) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S1068 'BlackCat' (malware). |
| 120 | T1485 Data Destruction | Sandworm (HermeticWiper, CaddyWiper) | Sandworm Team (G0034) | CONFIRMED | Sandworm Team (G0034) | direct uses: T1485 |
| 121 | T1485 Data Destruction | Industroyer operators | Industroyer (S0604) | NONEXISTENT (as actor) |  | No group or campaign by this name in v19; exists only as software: S0604 'Industroyer' (malware). |

## What was not done

- No edits to index.html. No attribution was added, removed, renamed, or substituted.
- UNSUPPORTED rows were not silently remapped to nearby pairings that do exist in the
  bundle. Disposition of every non-CONFIRMED row is the maintainer's call: remove,
  caveat, or keep with an explicit non-MITRE source citation.
