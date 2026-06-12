# BGP Hijacking Evidence: AS202734

This repository is an archive of evidence regarding the BGP route leak/hijacking incident on **May 16–17, 2026**, involving AS202734 (registered to Junqi Tian / Tianshome.net), sponsored by MoeDove LLC.

---

## TL;DR

**One RIPE NCC member (AS202734) + two affiliated ASes (AS402335, AS402333) intentionally hijacked 4,632 prefixes including China Telecom's IPv6 backbone. The sponsoring LIR responded to abuse with "To idiot". All evidence independently archived.**

---

## Key Facts

- **4,632 prefixes** were exported to Hurricane Electric's route collector
- **Affected prefixes include:** China Telecom, China Unicom, China Mobile, China Postal Bureau, Alibaba Cloud, Tencent Cloud, Huawei Cloud, CERNET, and more
- The operator **manually injected China Telecom IPv6 backbone (`240e::/20`)** on **May 1, 2026** (15 days before the leak)
- **Geofeed** shows a router in Shanghai, China (`yngp2-211`, Yangpu District)
- Sponsoring organization **MoeDove LLC** responded to abuse report with: **"To idiot haoziwan.xyz"**

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

Because the attribute is marked as **Transitive**, routers that do not recognize it must propagate it unchanged. This demonstrates that the route traversed a network capable of **advanced BGP engineering**, beyond basic route injection.

**Key implications:**

- The attacker operates a **multi-AS, redundantly upstreamed BGP infrastructure** with fixed routing policies — capabilities that are **incompatible with any claim of accidental misconfiguration**.
- The presence of a **custom transitive attribute** indicates sophisticated BGP engineering, not a simple route leak.
- The same AS_PATH engineering observed here was used to announce the **4,632 hijacked prefixes** documented above.

**Raw output (full `show ip bgp` command result):**  
[`/1-attacker-assets/route-views_show_ip_bgp_23.158.20.0-24.txt`](1-attacker-assets/route-views_show_ip_bgp_23.158.20.0-24.txt)

**Verification:** Any party may independently query Route Views or review the archived raw output.

---

## Retaliatory Actions

### Confirmed: Email Spoofing Attempt

Following the abuse report and publication of this repository, MoeDove LLC (`moedove.com`) attempted to forge `haoziwan.xyz` as the From domain, sending 12 spoofed emails via Google infrastructure (IP `209.85.220.69`). All attempts were rejected by DMARC policy (`p=reject`).

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

🔗 **Interactive BGPlay Timeline:**  
[https://stat.ripe.net/bgplay/240e::/20#starttime=1777593600&endtime=1779321599&rrcs=0,1,3,4,5,6,7,10,11,12,13,15,16,18,19,20,21,22,23,24,25,26](https://stat.ripe.net/bgplay/240e::/20#starttime=1777593600&endtime=1779321599&rrcs=0,1,3,4,5,6,7,10,11,12,13,15,16,18,19,20,21,22,23,24,25,26)

> **Note:** This is a direct link to RIPE NCC's own visualization tool, using their own data. It independently confirms the abnormal routing behavior correlated with the attacker's actions.

**Raw Data:**  
The complete BGPlay JSON export (5,504 timestamped events) is available in the repository at [`/0-attack-evidence/bgplay-240e-may2026.json`](0-attack-evidence/bgplay-240e-may2026.json).

---

## Source & Independence Statement

All evidence in this repository was **independently collected, verified, and archived** by Zhong Miao (`postmaster@haoziwan.xyz`).

The attacker's original GitHub repositories remain publicly available at the time of this writing and have been **forked and preserved** as independent repositories under the same GitHub account (see below).

**This repository is not a direct fork of any attacker repository. It is an original evidence archive created by the investigator.**

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

## Attacker's Original Repositories (Forked & Preserved)

In addition to the screenshots and data in [`/1-attacker-assets`](1-attacker-assets), the attacker's original GitHub repositories have been forked and preserved as independent repositories:

| Original (attacker) | Forked backup (preserved) |
| :--- | :--- |
| `tianshome/moegeo` | [`miao-2011/moegeo`](https://github.com/miao-2011/moegeo) |
| `tianshome/geofeed` | [`miao-2011/geofeed`](https://github.com/miao-2011/geofeed) |
| `tianshome/bird-configs-output` | [`miao-2011/bird-configs-output`](https://github.com/miao-2011/bird-configs-output) |
| `tianshome/looking-glass` | [`miao-2011/looking-glass`](https://github.com/miao-2011/looking-glass) |
| `tianshome/zt-ix` | [`miao-2011/zt-ix`](https://github.com/miao-2011/zt-ix) |
| `tianshome/bird` | [`miao-2011/bird`](https://github.com/miao-2011/bird) |
| `tianshome/chn-resolver` | [`miao-2011/chn-resolver`](https://github.com/miao-2011/chn-resolver) |

All forks were created for archival purposes and serve as an immutable evidence record.

---

## Related Links

- **HE BGP Toolkit**: [https://bgp.he.net/AS202734](https://bgp.he.net/AS202734)
- **RIPE NCC AS202734 record**: [https://apps.db.ripe.net/db-web-ui/query?searchtext=AS202734](https://apps.db.ripe.net/db-web-ui/query?searchtext=AS202734)
- **NANOG discussion**: [https://seclists.org/nanog/2026/May/132](https://seclists.org/nanog/2026/May/132)
- **RIPE NCC tickets**: #1042641, #1043090

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
