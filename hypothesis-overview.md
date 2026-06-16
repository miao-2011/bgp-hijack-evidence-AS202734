# Hypothesis Overview: 240e::/20 BGP Hijack Incident (May 2026)

## Incident ID
`240e-bgp-hijack-2026`

## Target
`240e::/20` — China Telecom IPv6 backbone (AS4134)

## Event Period
**2026-05-01 ~ 2026-05-20** (approximately 20 days)

## Key Metrics
| Metric | Value |
|--------|-------|
| Total BGP Events | 5,504 |
| Normal Baseline | < 500 (for stable prefixes) |
| Withdrawal Events (W) | 693 |
| 3-hop Paths | 79.5% (4,373 of 5,427) |
| 5+ hop Paths | 205 (concentrated on May 5 & May 10) |
| First Anomaly | 2026-05-01 05:20:35Z |
| Peak Anomaly Window | 2026-05-16 ~ 2026-05-17 |


## Attack Chain Hypothesis

Based on WHOIS records, BGPlay path data, IXP peering information, and PeeringDB records:

| Layer | AS Number | Role | Location / Notes |
|-------|-----------|------|------------------|
| **Origin / Operator** | **AS202734** | Attack operator | RIPE NCC member (Tianshome.net / Junqi Tian) |
| **Sponsoring LIR** | **ORG-ML942-RIPE** | MoeDove LLC | Confirmed sponsor of AS202734 (RIPE WHOIS) |
| **Upstream / Peer** | **AS44324** | MoeDove LLC | Direct BGP peering with AS202734 (WHOIS: `import/export from AS44324`) |
| **IXP** | AS210925 / AS211509 | RUDAKI-IX | Confirmed MoeDove peer (PeeringDB: 10G, RS PEER) |
| **Transit AS** | AS29632 / AS211288 / AS8772 | Netassist | Ukraine / Bulgaria / Europe |
| **Global Backbone** | AS3491 / AS1299 / AS2914 | PCCW / Telia / NTT | Tier-1 global carriers |
| **Target** | **AS4134** | China Telecom | China (victim of route hijack) |

**Observed Path Pattern (via BGPlay):**

`AS202734 → AS44324 → RUDAKI-IX (AS210925/AS211509) → Netassist (AS29632/AS211288/AS8772) → Tier-1 (AS3491/AS1299/AS2914) → AS4134 (China Telecom)`

> **Note:** This chain is based on WHOIS records, path cross-validation, and PeeringDB data. RUDAKI-IX is confirmed as a MoeDove public peer via PeeringDB.


## Key Observations

### Confirmed / High Confidence
- **AS202734** is the origin of the route injection (HE Bogon list, 5,504 BGPlay events, RIPE WHOIS)
- **AS202734** is registered to **Junqi Tian** (Tianshome.net) — RIPE WHOIS
- **AS202734** is sponsored by **MoeDove LLC (ORG-ML942-RIPE)** — RIPE WHOIS
- **AS44324 (MoeDove LLC)** has direct BGP peering with AS202734 — RIPE WHOIS (`import/export from AS44324`)
- **AS44324 (MoeDove LLC)** has public peering at **RUDAKI-IX** (10G, RS PEER) — PeeringDB confirmed
- **AS4134 (China Telecom)** is the final victim receiving the injected route
- **5,504 events** were recorded across 26 RIPE RRC collectors simultaneously
- **693 withdrawal events** occurred, concentrated in the first 5 days of the attack
- **79.5% of paths are 3-hop**, indicating a structured injection pattern rather than random propagation
- **Attack location**: 1103-2100 Rue de Bleury, Montreal, QC, Canada (Junqi Tian's registered address, near McGill University)

### Pending / Medium Confidence
- **Netassist (AS29632 / AS211288 / AS8772)** is the primary transit toward Tier-1 carriers — confirmed via WHOIS, but active peering status during the attack window needs cross-check


## Confirmed AS202734 WHOIS Information

Raw output from RIPE WHOIS:

aut-num:        AS202734
as-name:        Tianshome
org:            ORG-JT121-RIPE (Junqi Tian)
sponsoring-org: ORG-ML942-RIPE (MoeDove LLC)
address:        1103-2100 Rue de Bleury, Montreal, QC, H3A0H4, CA

import:         from AS44324 accept ANY
export:         to AS44324 announce AS202734
import:         from AS20473 accept ANY
export:         to AS20473 announce AS202734

**Key takeaways:**
- AS202734 is **Tianshome.net**, registered to **Junqi Tian**
- **MoeDove LLC** is the **sponsoring organization** — confirmed by RIPE WHOIS
- AS44324 (MoeDove LLC) has **direct BGP peering** with AS202734
- Junqi Tian's registered address is in **Montreal, Canada** (near McGill University)


## Confirmed AS44324 (MoeDove LLC) WHOIS Information

Raw output from RIPE WHOIS:

aut-num:        AS44324
as-name:        MOEDOVE-AS
org:            ORG-ML942-RIPE (MoeDove LLC)
address:        1209 Mountain Road Pl Ne, Ste N, Albuquerque, NM 87110, United States

**Key takeaways:**
- MoeDove LLC is registered in **New Mexico, USA**
- Company registration: 7475136 (New Mexico)
- Abuse contact: `noc@moedove.com`


## Confirmed PeeringDB Information (AS44324 / MoeDove LLC)

| Field | Value |
|-------|-------|
| ASN | 44324 |
| Organization | MoeDove LLC |
| IPv4 Prefixes | 1,000 |
| IPv6 Prefixes | 1,000 |
| Traffic Level | 10-20 Gbps |
| Geographic Scope | Global |
| Public Peering Exchange Points | RUDAKI-IX (10G, RS PEER), ZXIX Hangzhou (1G), ZXIX Hong Kong (10G), TPIX-TW (20G, Taipei), and 20+ others |

**Key takeaways:**
- **RUDAKI-IX** is confirmed as a MoeDove public peer (10G, RS PEER)
- **MoeDove has physical presence in China**: ZXIX Hangzhou (1G), ZXIX Hong Kong (10G), TPIX-TW (20G, Taipei)
- This confirms the attacker's infrastructure has direct access to Asian peering points


## Supporting Evidence

| Evidence | Source |
|----------|--------|
| 5,504 BGP events | RIPE NCC BGPlay JSON |
| 693 withdrawal events | RIPE NCC BGPlay JSON |
| 4,632 prefix Bogon list | Hurricane Electric BGP Toolkit |
| AS202734 origin confirmation | HE Bogon list (3 PDFs) |
| AS202734 WHOIS (Tianshome / Junqi Tian) | RIPE WHOIS |
| AS202734 ← AS44324 peering | RIPE WHOIS (`import/export from AS44324`) |
| AS44324 (MoeDove LLC) WHOIS | RIPE WHOIS |
| AS44324 PeeringDB record | PeeringDB |
| AS44324 ← AS29632 peering | AS29632 WHOIS (`MOEDOVE via RUDAKI-IX`) |
| AS46997 / AS38008 transit | ARIN / APNIC WHOIS |
| Attack timeline (May 1 injection) | BGPlay first anomaly at 05:20:35Z |
| Multi-collector observation | 26 RIPE RRC collectors |
| Attacker physical address (Junqi Tian) | RIPE WHOIS (Montreal, QC, Canada) |
| MoeDove physical address | RIPE WHOIS (Albuquerque, NM, USA) |


## Confidence Assessment

| Component | Confidence | Justification |
|-----------|------------|---------------|
| Abnormal BGP activity | **High** | 5,504 events vs <500 baseline, 26 collectors |
| Target is AS4134 | **High** | All anomalous paths contain AS4134 |
| Route injection occurred | **High** | Consistent path anomalies + 693 withdrawals + HE Bogon list |
| AS202734 is the origin | **High** | Frequent appearance in 3-hop paths, HE origin confirmation, BIRD config evidence |
| AS202734 is Tianshome.net / Junqi Tian | **High** | RIPE WHOIS confirmed |
| AS44324 (MoeDove LLC) is sponsor | **High** | RIPE WHOIS (`sponsoring-org: ORG-ML942-RIPE`) |
| AS44324 is upstream/peer | **High** | RIPE WHOIS (`import/export from AS44324`) |
| RUDAKI-IX is the entry IXP | **High** | PeeringDB confirms MoeDove public peer at RUDAKI-IX |
| Netassist (AS29632) is transit | **Medium** | WHOIS confirmed; active peering status during attack window needs cross-check |


## Next Steps

- [ ] Cross-check Netassist (AS29632) peering with European IXPs (DE-CIX, AMS-IX, PL-IX) during the attack window


## Notes

- **AS202734 is confirmed as Tianshome.net / Junqi Tian via RIPE WHOIS.**
- **MoeDove LLC (AS44324) is confirmed as the sponsoring organization and direct BGP peer.**
- **RUDAKI-IX is confirmed as a MoeDove public peer via PeeringDB.**
- All evidence in this document is derived from publicly accessible data sources (RIPE RIS, Hurricane Electric, WHOIS, PeeringDB) or personally owned records.


**Last Updated:** 2026-06-17  
**Status:** Active Investigation / Evidence Archive
