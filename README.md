# BGP Hijacking Evidence: AS202734

This repository is an independent archive of evidence regarding the BGP route hijacking incident on **May 16–17, 2026**, involving AS202734 (registered to Junqi Tian / Tianshome.net), sponsored by MoeDove LLC.

---

## TL;DR

**AS202734, together with affiliated ASes AS402335 and AS402333, originated 4,632 unauthorized IPv4 and IPv6 prefixes, including China Telecom's IPv6 backbone (`240e::/20`). The attack involved systematic IRR object forgery, RPKI-invalid announcements, and custom BGP attributes. The sponsoring LIR responded to abuse reports with "To idiot haoziwan.xyz". All evidence is independently archived here.**

---

## Key Facts

- **4,632 prefixes** were exported to Hurricane Electric's route collector
- **Affected prefixes include:** China Telecom, China Unicom, China Mobile, China Postal Bureau, Alibaba Cloud, Tencent Cloud, Huawei Cloud, CERNET, and others
- The operator **manually injected China Telecom IPv6 backbone (`240e::/20`)** on **May 1, 2026** (15 days before the leak)
- **Geofeed** shows a router in Shanghai, China (`yngp2-211`, Yangpu District)
- Sponsoring organization **MoeDove LLC** responded to an abuse report with: **"To idiot haoziwan.xyz"**

---

## Attackers' Own Looking Glass: Self-Incriminating Evidence

The operator of AS202734 maintained a public-facing Looking Glass server, which was used to monitor the BGP state of target prefixes. The query history, captured and archived in `/looking glass/`, reveals:

### Key Findings

| Finding | Implication |
|---------|-------------|
| Global POP network (NYC, FRA, SIN, TYO, SEA) | Professional BGP infrastructure, not accidental |
| Route Reflector cluster (`bgp_cluster_list: 0.3.0.1`) | Enterprise-grade routing architecture |
| Queries for `1.0.0.1` (Cloudflare), `103.36.96.0` (Microsoft), `240e::/20` (China Telecom) | Systematic monitoring of hijack targets |
| Query for `175.45.176.0/24` (Ryugyong-dong, Pyongyang) | Unauthorized surveillance of North Korean infrastructure |

### Archived Queries

The following PDFs were captured from the attacker's own Looking Glass server:

- `175.45.176.1 lookingglass(Ryugyong-dong).pdf` — Query for North Korean IP space
- `240e lookingglass(China Telecom).pdf` — Query for China Telecom IPv6 backbone
- `103.36.96.0 lookingglass(Microsoft).pdf` — Query for Microsoft Azure prefix
- `59.80.0.0 lookingglass(China Unicom).pdf` — Query for China Unicom prefix
- ... (full list in [`/looking glass/`](/looking%20glass/))

### Why This Matters

**This is not third-party observation. This is the attacker's own infrastructure recording their own activities.**

The Looking Glass was operated by AS202734 and used to:
1. Verify hijack status of target prefixes
2. Monitor route propagation across global POPs
3. Coordinate attacks across multiple ASes (AS202734, AS402333, AS402335)

**No "misconfiguration" theory can explain why a network would query thousands of prefixes it does not own, across multiple continents, using an enterprise-grade BGP routing architecture.**

**Full evidence: [`/looking glass/`](/looking%20glass/)**

### Post-Incident Access Restriction

On or around **July 4–5, 2026**, the operator of AS202734 added an access control form to `lg.tianshome.net`, requiring visitors to provide:

- Email address
- IP address (CIDR)
- ASN(s)
- Justification / intended use case

**Context:**
- The Looking Glass had been **fully public and accessible without restriction** during the entire investigation period (May–July 3, 2026)
- **Internet Archive snapshot confirms the public state on July 3, 2026:**  
  [`https://web.archive.org/web/20260703131435/http://lg.tianshome.net/`](https://web.archive.org/web/20260703131435/http://lg.tianshome.net/)
- During this period, 29 queries were captured and archived in this repository. The queried targets include:
  - North Korean IP space (`175.45.176.0/24`, Ryugyong-dong, Pyongyang)
  - Chinese Academy of Sciences (`124.16.0.0/15`, CNIC-CAS)
  - CERNET China Education and Research Network (`240a:a000::/20`, `2001:da8::/32`)
  - CNNIC China Internet Network Information Center (`2400:dd00::/28`)
  - Multiple multinational technology and industrial networks, including Cisco Systems, General Motors, Microsoft, and Google

**Significance:**
The access restriction was added shortly after the public archive of historical queries was published — suggesting the operator is aware of the visibility of their previous queries and is attempting to limit further observation.

---

## Multi-AS Attack Infrastructure

Route Views official collector (`route-views.routeviews.org`) captured the BGP routing table entry for `23.158.20.0/24` (a prefix legitimately owned by AS202734). The output reveals that **AS202734, AS402335, and AS402333 appear in fixed order** across multiple independent upstream paths.

This fixed AS_PATH order indicates a deliberately configured routing policy, not random or transient path selection.

**Additionally, the BGP UPDATE carries an `unknown transitive attribute` (flag 0xE0, type 0x20, length 0x60).**

| Flag | Meaning |
|------|---------|
| `0xE0` | Optional + Transitive + Complete |
| `type 0x20` (32) | **Not a standard BGP attribute** — indicates custom/proprietary routing metadata |
| `length 0x60` (96 bytes) | Carries non-trivial amount of information |

Because the attribute is marked as **Transitive**, routers that do not recognize it must propagate it unchanged. This indicates the route traversed a network capable of **advanced BGP engineering**, beyond basic route injection.

**Key implications:**

- The operator maintains a **multi-AS, redundantly upstreamed BGP infrastructure** with fixed routing policies — capabilities that are **incompatible with any claim of accidental misconfiguration**
- The presence of a **custom transitive attribute** indicates sophisticated BGP engineering, not a simple route leak
- The same AS_PATH engineering observed here was used to announce the **4,632 hijacked prefixes** documented above

**Raw output (full `show ip bgp` command result):**  
[`/1-attacker-assets/route-views_show_ip_bgp_23.158.20.0-24.txt`](1-attacker-assets/route-views_show_ip_bgp_23.158.20.0-24.txt)

**Verification:** Any party may independently query Route Views or review the archived raw output.

---

## Retaliatory Actions

### Confirmed: Email Spoofing Attempts (May 19 & June 25, 2026)

Two independent DMARC reports from Cloudflare confirm that MoeDove LLC and Tianshome.net repeatedly attempted to impersonate `haoziwan.xyz`:

| Date | Sending IP | Display Domain | Actual Sending Domain | SPF Alignment | DKIM Alignment | DMARC Result | Count |
|------|-----------|----------------|----------------------|---------------|----------------|--------------|-------|
| 2026-05-19 | 209.85.220.69 (Google LLC) | haoziwan.xyz | moedove.com | Fail | Fail | Rejected (p=reject) | 12 |
| 2026-06-25 | 57.103.76.251 (Apple Inc.) | haoziwan.xyz | tianshome.net | Fail | Pass | Passed (DKIM-aligned) | 1 |

**Raw Evidence**: Cloudflare DMARC reports (archived in `/6-retaliation-evidence/`):

- `cloudflare-dmarc-moedove-spoofing-haoziwan-2026-05-19.pdf` — 12 rejected attempts via Google infrastructure
- `2026-06-25_tianshome-net_spoofing-haoziwan-xyz_dmarc-report.pdf` — 1 delivered attempt (SPF fail, DKIM pass) via Apple Inc. infrastructure

**Technical Analysis:**

| Observation | Implication |
|-------------|-------------|
| Two independent spoofing events, 37 days apart | Pattern of behavior, not isolated incident |
| Different sending infrastructures (Google + Apple) | Multiple deliberate attempts using different vectors |
| Both used haoziwan.xyz as display domain | Clear intent to impersonate the victim |
| May 19 attempt: all 12 rejected by DMARC p=reject | Existing DMARC policy successfully blocked impersonation |
| June 25 attempt: added valid DKIM signature from tianshome.net | Sender adapted tactics after first failure, demonstrating deliberate engineering |
| Both sending domains (moedove.com, tianshome.net) are directly affiliated with AS202734 | Direct link between spoofing and the party accused of BGP hijacking |

**Conclusion:**

The documented spoofing attempts demonstrate intentional impersonation of haoziwan.xyz, eliminating any possibility of:

- Accidental misconfiguration: the June 25 attempt specifically added DKIM signing to evade detection, showing deliberate technical adaptation
- Wrong domain entered: both attempts used haoziwan.xyz as display domain while the email content specifically referenced this investigation
- Third-party action: both sending infrastructures are controlled by the same parties identified in the BGP hijacking incident (MoeDove LLC / Tianshome.net)

This constitutes a documented pattern of retaliatory harassment via email spoofing, following the publication of evidence regarding AS202734's BGP hijacking activities.

**Full documentation:** [`/6-retaliation-evidence/SPOOFING_ATTEMPT.md`](6-retaliation-evidence/SPOOFING_ATTEMPT.md)

### Suspected: Mail Subscription Bombing (Source Unconfirmed)

Beginning on June 9, 2026 at 22:52 UTC, a mail subscription bombing attack was launched against email addresses associated with this investigation.

**Incident Summary:**
- **Start time:** June 9, 2026, 22:52 UTC
- **Targets:** `postmaster@haoziwan.xyz` → then `abuse@haoziwan.xyz` added at 01:09 UTC
- **Volume:** 100+ confirmation emails within 14 minutes
- **Routing:** Tunneled through Cloudflare infrastructure (egress IPs: `104.28.160.168`, `104.28.159.154`)

> **Attribution Note:** No direct evidence establishes that the party behind the AS202734 incident (MoeDove LLC) is also responsible for the subscription bombing. The temporal proximity is noted for completeness only.

**Full documentation:** [`/6-retaliation-evidence/MAIL_SUBSCRIPTION_BOMBING.md`](6-retaliation-evidence/MAIL_SUBSCRIPTION_BOMBING.md)

---

## Core Evidence: Attack Data Snapshot

The following data was captured **during the attack** on **May 16–17, 2026** from Hurricane Electric's BGP Toolkit ([`he_as202734_20260517.log`](0-attack-evidence/he_as202734_20260517.log)):

| Key Metric | Value |
| :--- | :--- |
| **IPv4 Prefixes Originated** | 3,948 |
| **IPv6 Prefixes Originated** | 684 |
| **Total Prefixes Announced** | 4,632 |
| **RPKI Invalid (IPv4)** | 1,323 |
| **RPKI Invalid (IPv6)** | 107 |
| **IPv4 Address Space Claimed** | 285,767,680 IPs |
| **Average AS Path Length (IPv4)** | 2.023 (abnormally short, indicating route manipulation) |
| **Average AS Path Length (IPv6)** | 4.346 |

> **Note:** All numbers above are sourced directly from Hurricane Electric's BGP Toolkit. The complete data is available in the log file.

**Further reading — IRR fraud analysis:**  
[`/0-attack-evidence/key-observation-irr-fraud.md`](0-attack-evidence/key-observation-irr-fraud.md)

**Key insight:** The attacker systematically created or modified IRR route objects for hijacked prefixes they did not own, including successful forgeries ("IRR Valid") and failed attempts ("IRR Parent Invalid"), demonstrating deliberate registry-layer fraud rather than accidental misconfiguration.

---

## Visual Evidence: Global Routing Chaos

The following BGPlay animation (RIPE NCC) visualizes the BGP path changes for `240e::/20` (China Telecom IPv6 backbone) from **May 1, 2026** (the day of the manual injection) to **May 20, 2026** (after the hijack).

**What it shows:**
- **5,504 timestamped events** were recorded within 20 days (normal for a stable prefix: <500)
- First major route flaps started **within minutes** after the May 1 manual injection
- Continuous path instability during the **May 16–17 hijack window**, observed across multiple global collectors (RRC00, RRC19, RRC23, RRC24, RRC25, etc.)

**Interactive BGPlay Timeline:**  
[https://stat.ripe.net/bgplay/240e::/20#starttime=1777593600&endtime=1779321599&rrcs=0,1,3,4,5,6,7,10,11,12,13,15,16,18,19,20,21,22,23,24,25,26](https://stat.ripe.net/bgplay/240e::/20#starttime=1777593600&endtime=1779321599&rrcs=0,1,3,4,5,6,7,10,11,12,13,15,16,18,19,20,21,22,23,24,25,26)

> **Note:** This is a direct link to RIPE NCC's own visualization tool, using their own data. It independently confirms the abnormal routing behavior correlated with the attacker's actions.

**Raw Data:**  
The complete BGPlay JSON export (5,504 timestamped events) is available in two formats:
- **Human-readable:** [`/0-attack-evidence/240e_events.json`](0-attack-evidence/240e_events.json)
- **Raw (for jq processing):** [`/0-attack-evidence/bgplay-240e-may2026.json`](0-attack-evidence/bgplay-240e-may2026.json)

---

## Communication Record

### 1. NANOG Public Response — Jacob-Junqi Tian (May 23, 2026)

Tian publicly responded on the NANOG mailing list, attributing the incident to a "filter configuration error" in a custom BIRD3 setup. He claimed that the affected prefixes were exported only to Hurricane Electric's route collector, not to public upstream routers. He did not address IRR forgery, RPKI-invalid prefixes, or the custom transitive BGP attribute observed in the hijacked routes.

**Archive:** [`/2-communication/Re_%20%5BBGP%20Hijack%5D%20AS202734%20hijacked%20multiple%20Chinese%20Carriers%20on%20May%2016-17%20%E2%80%93%20Full%20evidence%20and%20attribution.eml`](2-communication/Re_%20%5BBGP%20Hijack%5D%20AS202734%20hijacked%20multiple%20Chinese%20Carriers%20on%20May%2016-17%20%E2%80%93%20Full%20evidence%20and%20attribution.eml)

### 2. MoeDove LLC — Rinne Miyano (June 25, 2026)

Rinne Miyano (`pigeon@moedove.com`) responded to a formal notice with:

> *"Stop wasting our time, this is just another spam. Your repo and so-called research are worth nothing, literally a piece of shit. Get back to doing your homework, kid."*

This response did not address any technical evidence.

**Archive:** [`/2-communication/Re_ Notice of Archival_ AS202734 BGP Route Announcements.eml`](2-communication/Re_%20Notice%20of%20Archival_%20AS202734%20BGP%20Route%20Announcements.eml)

### 3. Tianshome.net — Jacob-Junqi Tian (June 25, 2026)

Tian sent a formal cease-and-desist letter, disputing the term "originate," denying unauthorized announcements, and threatening legal action across three jurisdictions (Canada, United States, China). He did not provide counter-evidence for the IRR forgery or RPKI-invalid claims.

**Archive:** [`/2-communication/2026-06-25_Jacob-Junqi-Tian_cease-and-desist.eml`](2-communication/2026-06-25_Jacob-Junqi-Tian_cease-and-desist.eml)

**CC list included:** `noc@tianshome.com`, `chariri@chariri.moe`, `yangyingling02@gmail.com`, `iuusreport@gmail.com`, `pigeon@moedove.com`

---

## Source & Independence Statement

All evidence in this repository was **independently collected, verified, and archived** by Zhong Miao (`postmaster@haoziwan.xyz`).

|                 |                      |
|-----------------|----------------------|
| **Investigator** | Zhong Miao           |
| **Contact**      | postmaster@haoziwan.xyz |

---

## Repository Structure

| Directory | Contents |
| :--- | :--- |
| [`/0-attack-evidence`](0-attack-evidence) | Core technical evidence (HE BGP data, RIPE RIS logs, looking glass screenshots) |
| [`/1-attacker-assets`](1-attacker-assets) | Attacker-related assets (HE snapshots, WHOIS records, Geofeed, Route Views CLI output) |
| [`/2-communication`](2-communication) | All email correspondence — abuse reports, the attacker's "To idiot" reply, community discussions |
| [`/3-ripe-ncc-interaction`](3-ripe-ncc-interaction) | RIPE NCC tickets, compliance correspondence, screenshots |
| [`/4-moedove-identity`](4-moedove-identity) | MoeDove LLC — upstream/sponsor identity and assets (ToS, website, GitHub PRs) |
| [`/5-additional-context`](5-additional-context) | WHOIS records, company background, NANOG discussion summary |
| [`/6-retaliation-evidence`](6-retaliation-evidence) | Retaliatory harassment — confirmed email spoofing (MoeDove LLC) + suspected mail subscription bombing (source unconfirmed, timeline documented) |

---

## Correspondence

### Hurricane Electric (HE)

| Date | Event | Status |
|------|-------|--------|
| 2026-06-22 | Initial data request sent to HE | Sent |
| 2026-06-22 | HE auto-acknowledgement, ticket [HE#7129512] | Received |
| 2026-06-22 | HE substantive response (Kelly Cochran) | Received |
| 2026-06-23 | Follow-up request sent, clarifying direct peer status | Sent |
| 2026-06-23 | HE final response (Cole Bite, NOC) | Received |

**HE's Final Response (Summary)**:
1. HE states they **do not log past peer route announcements**.
2. HE states they **may have logged data regarding BGP session status with AS6939**, but **will not share without a court order**.

**Investigator's Note**:
HE's final response is internally inconsistent. If HE does not log past peer route announcements, the assertion that they "may have logged data" is contradictory. This suggests HE does retain some BGP-related data, but has chosen not to cooperate without legal compulsion. HE's refusal is hereby documented as a non-cooperation data point.

**Archive Reference**:
[`[HE#7129512] HE_Official_Response_No_Historical_Logs.eml`](2-communication/%5BHE%237129512%5D%20HE_Official_Response_No_Historical_Logs.eml)

---

## Related Links

- **HE BGP Toolkit**: [https://bgp.he.net/AS202734](https://bgp.he.net/AS202734)
- **RIPE NCC AS202734 record**: [https://apps.db.ripe.net/db-web-ui/query?searchtext=AS202734](https://apps.db.ripe.net/db-web-ui/query?searchtext=AS202734)
- **NANOG discussion**: [https://seclists.org/nanog/2026/May/132](https://seclists.org/nanog/2026/May/132)
- **RIPE NCC tickets**: #1042641, #1043090
- **APNIC**: #6392390, #6392389
- **Hurricane Electric**: #7129512

---

## License & Usage

© 2026 Zhong Miao. All rights reserved.

This repository constitutes an **evidence archive**, not an open-source software project.

**Permitted:**  
Viewing, citation, and linking for legitimate purposes (security research, journalism, regulatory proceedings)

**Not permitted:**  
Copying, modifying, redistributing, or commercial exploitation of any part of this archive without explicit permission.

For inquiries or verification requests:  
**postmaster@haoziwan.xyz**
