# Disposition (Phase 3, Step 0b): what to do with each non-CONFIRMED attribution

Date: 2026-08-09
Ground truth for anything checkable in-bundle: `enterprise-attack-19.0.json`
(x_mitre_version 19.0, modified 2026-04-28).
Stage 1 only. index.html was not modified. No edits until you approve.

Scope: the 14 non-CONFIRMED group rows (11 UNSUPPORTED + 3 APT29 parent-link) and
the 43 software-as-actor rows from ATTRIBUTION-AUDIT.md. The 45 CONFIRMED rows and
the 19 GENERIC rows are out of scope here; CONFIRMED rows stay, GENERIC phrasing is a
copy question for the UX pass.

Global note, stated once and applying to every CUT below: absence from ATT&CK v19, and
failure to meet the two-source corroboration bar, does not mean a pairing is false in the
real world. It means this project cannot source it to its stated ground truth or to
independent authoritative reporting. Nothing here asserts a CUT pairing is wrong.

## Recommended actions at a glance

| Category | Rows | REMAP | CITE | RE-POINT | RELABEL-KEEP | CUT |
|---|---|---|---|---|---|---|
| A. UNSUPPORTED groups | 11 | 1 | 0 | - | - | 10 |
| B. APT29 parent-link | 3 | - | - | 3 | - | 0 |
| C. Software-as-actor | 43 | - | - | - | 25 | 18 |

Net: of 57 rows reviewed, 29 survive in some form (1 remap, 3 re-point, 25 relabel) and
28 are cut. No row is kept on the strength of a single source or general knowledge.

---

## A. The 11 UNSUPPORTED group rows

Priority applied per your spec: (1) remap to a neighboring in-bundle technique the same
group actually uses and that matches the HUD behavior; else (2) keep only with two
independent authoritative sources (or one government advisory naming actor + behavior)
and no contradicting attribution; else (3) cut.

### A1. Scattered Spider under T1555.003 -> REMAP to T1539 (Steal Web Session Cookie)

- Original claim: Scattered Spider (G1015) observed with T1555.003 "Credentials from Web
  Browsers".
- Action: REMAP to **T1539 Steal Web Session Cookie**.
- In-bundle evidence: Scattered Spider (G1015) has a direct `uses` relationship to T1539
  and to T1555.005 (Password Managers), T1552.001, T1552.004. It has no `uses` link to
  T1555.003 or its parent T1555.
- Why T1539: the HUD copy for this node emphasizes "cookies, and session tokens ... a
  staple of infostealer malware". That is exactly T1539. Independently, CISA/FBI advisory
  AA23-320A documents Scattered Spider using Raccoon Stealer and Vidar "to obtain browser
  cookies", "browser histories", and "login credentials", and the advisory itself tags
  that behavior T1539. So the in-bundle group link and the government advisory agree on
  T1539. This needs no external dependency; the STIX link alone carries it.
- Note for Stage 2: if remapped to T1539, the node's copy should drop the "saved
  passwords" framing so it reads as cookie/session-token theft. Your call whether to
  also keep a browser-password node; if you want to keep T1555.003 specifically, see the
  alternative below.
- Alternative if you prefer to keep the T1555.003 node: AA23-320A (CISA + FBI, orig.
  Nov 2023, rev. Jul 2025) explicitly names the actor and the browser-credential
  behavior, which under your rules is a single government advisory that may stand alone.
  I did not find a source disputing this attribution. I still recommend the REMAP because
  it is in-bundle and needs no citation, per your priority order.

### A2 through A11: CUT (no in-bundle behavior match, two-source bar not met)

Each row below has no `uses` link from the group to the listed technique or a
same-behavior neighbor, and did not clear the corroboration bar. Where a group does use
some other technique in the same family, it is noted so you can see why it still is not a
clean remap.

| Row | Group | Listed technique | Why not remapped | Why not cited | Verdict |
|---|---|---|---|---|---|
| A2 | Volt Typhoon (G1017) | T1595.002 Vulnerability Scanning | No T1595-family link. Group uses T1590/T1592/T1596.005 (different behavior: passive gathering, not active vuln scanning). AA24-038A lists Volt Typhoon recon as T1589.002/T1590/T1591/T1592/T1593/T1594 and its scanning tools (Shodan, Censys, FOFA, ScanLine) but does not tag T1595.002. | The one government advisory that covers it does not assign this technique. | CUT |
| A3 | APT29 (G0016) | T1589.002 Email Addresses | Only same-family link is T1589.001 (Credentials), a different sub-technique, via the SolarWinds campaign. Behavior described is email-address harvesting. | No second independent source for the exact email-address pairing. | CUT |
| A4 | FIN7 (G0046) | T1589.002 Email Addresses | No T1589-family link. Group uses T1591/T1591.004 (org info), different behavior. | Not corroborated to bar. | CUT |
| A5 | APT1 (G0006) | T1590 Gather Victim Network Information | No T1590-family link anywhere for this group. | Not corroborated to bar. | CUT |
| A6 | Mustang Panda (G0129) | T1590 Gather Victim Network Information | No T1590-family link. | Not corroborated to bar. | CUT |
| A7 | APT41 (G0096) | T1587.001 Develop Capabilities: Malware | No T1587-family link. Group uses T1588.002 (Obtain Tool) and T1588.003 via APT41 DUST campaign, i.e. obtaining, not developing. | Mandiant "Double Dragon" documents APT41 custom malware, but that is one source; I could not verify a second independent authoritative source that explicitly documents malware development (the DOJ press release was not retrievable and the reporting on it describes tools/exploits/supply-chain, not development). One source does not meet the bar. When unsure, cut. | CUT |
| A8 | FIN7 (G0046) | T1134 Access Token Manipulation | No T1134-family link. | Not corroborated to bar. | CUT |
| A9 | APT29 (G0016) | T1003.001 OS Credential Dumping: LSASS Memory | Group uses siblings T1003.002 (SAM) and T1003.004 (LSA Secrets) directly, and T1003.006 (DCSync) via campaign, but not the LSASS-memory sub or the T1003 parent. Siblings are different credential stores, so not a clean behavior remap. | Not corroborated to bar for the LSASS-specific pairing. | CUT |
| A10 | Turla (G0010) | T1573.002 Encrypted Channel: Asymmetric Cryptography | No T1573-family link. Group uses T1071.001/T1071.003 (app-layer protocol), different behavior. | Not corroborated to bar. | CUT |
| A11 | APT29 (G0016) | T1041 Exfiltration Over C2 Channel | No T1041 link. Group uses T1048.002 (exfil over asymmetric-encrypted non-C2) via SolarWinds campaign, a different exfil path. | Not corroborated to bar for the over-C2 pairing. | CUT |

Sources consulted for A (for the record; none produced a keep):
- CISA AA24-038A, "PRC State-Sponsored Actors Compromise ... US Critical Infrastructure"
  (CISA/NSA/FBI + intl partners, 7 Feb 2024). Used to test Volt Typhoon recon tagging.
  https://www.cisa.gov/news-events/cybersecurity-advisories/aa24-038a
- CISA/FBI AA23-320A, "Scattered Spider" (orig. 16 Nov 2023, rev. 29 Jul 2025).
  Confirms infostealer browser-credential behavior, tagged T1539.
  https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-320a
- Mandiant, "Double Dragon: APT41, a Dual Espionage and Cyber Crime Operation" (2019).
  One source only for APT41 malware development; insufficient alone.

---

## B. The 3 APT29 parent-link rows: RE-POINT (all three verified in-bundle)

Instruction: re-point to the parent only if the bundle has an APT29 `uses` link to that
parent. All three hold. Two are campaign-derived (APT29's SolarWinds Compromise, C0024,
which ATT&CK attributes to APT29 and renders on the group page); one is a direct link.

| Row | From | To parent | APT29 link in bundle | Verdict |
|---|---|---|---|---|
| B1 | T1685.005 Clear Windows Event Logs | T1685 Disable or Modify Tools | Via SolarWinds Compromise (C0024): campaign uses T1685 (and subs .001/.002). Not the .005 sub. | RE-POINT to T1685 |
| B2 | T1550.002 Pass the Hash | T1550 Use Alternate Authentication Material | Via SolarWinds Compromise (C0024): campaign uses T1550 (and subs .001/.003/.004). Not the .002 sub. | RE-POINT to T1550 |
| B3 | T1573.002 Asymmetric Cryptography | T1573 Encrypted Channel | Direct APT29 `uses` link to T1573. | RE-POINT to T1573 |

Caveat for B1/B2: these become CONFIRMED at the parent level, but only through the
SolarWinds campaign, not a direct group technique. If you would rather the HUD only show
direct group-to-technique links, both are CUTs instead. B3 is a direct link either way.

---

## C. The 43 software-as-actor rows: RELABEL-KEEP or CUT

These names are malware/tool objects in v19, not groups. For each I checked whether the
software object has a `uses` link to the listed technique (exact ID, or a sub-technique of
a listed parent). RELABEL-KEEP means the link exists and the chip should be moved out of
"Observed Threat Actors" and shown as software (e.g. a "Malware / Tooling" label), not as
an actor. CUT means no such link.

### C-KEEP (25 rows): relabel from actor to software, link verified

| Technique | HUD chip | v19 software + link |
|---|---|---|
| T1566.001 | Emotet operators | S0367 Emotet uses T1566.001 |
| T1204.002 | Qakbot | S0650 QakBot uses T1204.002 |
| T1204.002 | IcedID | S0483 IcedID uses T1204.002 |
| T1204.002 | Emotet | S0367 Emotet uses T1204.002 |
| T1547.001 | TrickBot / Qakbot | S0266 TrickBot uses T1547.001; S0650 QakBot uses T1547.001 |
| T1547.001 | Emotet | S0367 Emotet uses T1547.001 |
| T1055 | TrickBot | S0266 TrickBot uses T1055 |
| T1055 | Emotet | S0367 Emotet uses T1055.001, T1055.012 (subs of T1055) |
| T1055 | Cobalt Strike operators | S0154 Cobalt Strike uses T1055 |
| T1685 | LockBit | S1199 LockBit 2.0 uses T1685; S1202 LockBit 3.0 uses T1685 |
| T1685 | BlackCat/ALPHV | S1068 BlackCat uses T1685.005 (sub of T1685) |
| T1555.003 | Lumma / RedLine operators | S1213 Lumma Stealer uses T1555.003; S1240 RedLine Stealer uses T1555.003 |
| T1087.002 | BlackCat/ALPHV | S1068 BlackCat uses T1087.002 |
| T1021.002 | NotPetya | S0368 NotPetya uses T1021.002 |
| T1021.002 | Ryuk | S0446 Ryuk uses T1021.002 |
| T1021.002 | Conti | S0575 Conti uses T1021.002 |
| T1071.001 | Cobalt Strike operators | S0154 Cobalt Strike uses T1071.001 |
| T1486 | LockBit | S1199 LockBit 2.0 uses T1486; S1202 LockBit 3.0 uses T1486 |
| T1486 | BlackCat/ALPHV | S1068 BlackCat uses T1486 |
| T1486 | Royal | S1073 Royal uses T1486 |
| T1486 | Conti | S0575 Conti uses T1486 |
| T1486 | Ryuk | S0446 Ryuk uses T1486 |
| T1490 | LockBit | S1199 LockBit 2.0 uses T1490; S1202 LockBit 3.0 uses T1490 |
| T1490 | BlackCat | S1068 BlackCat uses T1490 |
| T1485 | Industroyer operators | S0604 Industroyer uses T1485 |

Note on T1555.003: this is the same node discussed in A1. Here the Lumma/RedLine chips
are software that genuinely uses T1555.003, so they survive as software even though the
Scattered Spider chip on that node is being remapped to T1539. If you remap the node to
T1539, these two chips would need to move with the browser-credential content or be
dropped; flag this interaction for Stage 2.

### C-CUT (18 rows): software exists but no link to the listed technique

| Technique | HUD chip | Finding |
|---|---|---|
| T1583.001 | Conti operators | S0575 Conti: no link to T1583.001 |
| T1588.002 | LockBit affiliates | S1199/S1202 LockBit: no link to T1588.002 |
| T1588.002 | Black Basta | S1070 Black Basta: no link to T1588.002 |
| T1190 | Cl0p (MOVEit) | S0611 Clop: no link to T1190. See MOVEit note below. |
| T1059.001 | Ryuk operators | S0446 Ryuk: no link to T1059.001 |
| T1053.005 | Conti | S0575 Conti: no link to T1053.005 |
| T1505.003 | Cl0p (MOVEit) | S0611 Clop: no link to T1505.003. See MOVEit note below. |
| T1136.001 | LockBit affiliates | S1199 LockBit 2.0 links to parent T1136 only, not the .001 sub; S1202 no link |
| T1068 | LockBit | S1199/S1202 LockBit: no link to T1068 |
| T1685 | Royal | S1073 Royal: no link to T1685 (BlackCat and LockBit on this node do link; Royal does not) |
| T1558.003 | LockBit affiliates | S1199/S1202 LockBit: no link to T1558.003 |
| T1087.002 | Conti (BloodHound heavy) | S0575 Conti: no link to T1087.002 |
| T1021.001 | Conti | S0575 Conti: no link to T1021.001 |
| T1550.002 | Conti | S0575 Conti: no link to T1550.002 |
| T1041 | Cobalt Strike operators | S0154 Cobalt Strike: no link to T1041 |
| T1567.002 | LockBit | S1199/S1202 LockBit: no link to T1567.002 |
| T1567.002 | BlackCat/ALPHV | S1068 BlackCat: no link to T1567.002 |
| T1567.002 | Cl0p | S0611 Clop: no link to T1567.002. See MOVEit note below. |

### Cl0p / MOVEit callout (as requested)

All three Cl0p chips (T1190, T1505.003, T1567.002) are CUT. Reasons:
- v19 has the malware object S0611 "Clop", but it has no `uses` link to T1190
  (Exploit Public-Facing Application), T1505.003 (Web Shell), or T1567.002
  (Exfiltration to Cloud Storage).
- There is no MOVEit campaign in the v19 bundle. I enumerated all 56 active campaigns;
  none is MOVEit or Cl0p related. So the "(MOVEit)" narrative cannot be sourced to the
  bundle at all.
- Per your instruction, that narrative is therefore cut, not relabeled. If you want to
  keep any MOVEit reference, it would have to carry an explicit non-MITRE source citation
  that you approve; I am not adding one from memory.

---

## What I need from you before Stage 2

1. A: accept the single REMAP (Scattered Spider T1555.003 -> T1539), or prefer the
   AA23-320A CITE that keeps the T1555.003 node. Confirm the 10 CUTs.
2. B: accept all 3 RE-POINTs, or drop B1/B2 to CUT if you want direct-only group links.
3. C: accept the 25 RELABEL-KEEPs (and confirm you want a distinct "software/tooling"
   label rather than "Observed Threat Actors"), and the 18 CUTs including the three Cl0p
   rows.
4. Confirm the Sandworm -> Sandworm Team (G0034) rename from the prior audit still applies
   in the same commit.

On approval I will apply exactly the signed-off set to index.html, do the Sandworm Team
rename, show a full diff, and commit separately from any UX work. Nothing else changes.
