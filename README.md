# BGP Hijacking Evidence: AS202734

This repository is an archive of evidence regarding the BGP route leak/hijacking incident on **May 16–17, 2026**, involving AS202734 (registered to Junqi Tian / Tianshome.net), sponsored by MoeDove LLC.

## Key Facts

- **4,622 prefixes** were exported to Hurricane Electric's route collector
- Affected prefixes include: China Telecom, China Unicom, China Mobile, China Postal Bureau, Alibaba Cloud, Tencent Cloud, Huawei Cloud, CERNET, etc.
- The operator manually injected China Telecom IPv6 backbone (`240e::/20`) on **May 1, 2026** (15 days before the leak)
- Geofeed shows a router in Shanghai, China (yngp2-211, Yangpu District)
- Sponsoring organization MoeDove LLC responded to abuse report with: **"To idiot haoziwan.xyz"**

## Source

All evidence in this repository was **independently collected, verified, and archived** by Zhong Miao (`postmaster@haoziwan.xyz`).

The attacker's original GitHub repositories remain publicly available at the time of this writing and have been **forked and preserved** as independent repositories under the same GitHub account (see below).

This repository is **not** a direct fork of any attacker repository. It is an original evidence archive created by the investigator.

## Repository Structure

| Directory | Contents |
| :--- | :--- |
| `/0-attack-evidence` | Core technical evidence (RIPE RIS logs, HE BGP data, looking glass screenshots) |
| `/1-attacker-assets` | Additional attacker-related screenshots and data (HE snapshots, etc.) |
| `/2-communication` | All email correspondence — abuse reports, the attacker's "To idiot" reply, community discussions |
| `/3-ripe-ncc-interaction` | RIPE NCC tickets, compliance correspondence, screenshots |
| `/4-3rd-party-responses` | Official replies from Alibaba Cloud, Huawei Cloud, CNCERT, etc. (partial/pending) |
| `/5-additional-context` | WHOIS records, company background, NANOG discussion summary |

## Attacker's Original Repositories (Forked & Preserved)

In addition to the screenshots and data in `/1-attacker-assets`, the attacker's original GitHub repositories have been forked and preserved as independent repositories:

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

## Related Links

- **HE BGP Toolkit**: https://bgp.he.net/AS202734
- **RIPE NCC AS202734 record**: https://apps.db.ripe.net/db-web-ui/query?searchtext=AS202734
- **NANOG discussion**: [https://seclists.org/nanog/2026/May/132](https://seclists.org/nanog/2026/May/132)
- **RIPE NCC tickets**: #1042641, #1043090

## License

© 2026 Zhong Miao. All rights reserved.

This is an **evidence archive**, not an open-source project.

- You may **view, link to, and cite** this repository for legitimate purposes (research, journalism, legal proceedings).
- You may **NOT copy, modify, redistribute, or commercially exploit** any part of this evidence without permission.

For inquiries: `postmaster@haoziwan.xyz`
