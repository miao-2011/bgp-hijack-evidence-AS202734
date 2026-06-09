# Mail Subscription Bombing Incident

## Summary

Beginning on June 9, 2026 at 22:52 UTC, the email address `postmaster@haoziwan.xyz` was targeted by a mail subscription bombing attack. The attack involved automated subscription requests to dozens of public mailing lists, resulting in a flood of confirmation emails.

At 01:09 UTC on June 10, 2026, a second email address — `abuse@haoziwan.xyz` — was added as a target.

## Timeline (UTC)

| Time | Event |
|------|-------|
| June 9, 22:52 | First wave begins. Target: `postmaster@haoziwan.xyz` |
| June 10, 01:00 | Second wave. Target: `postmaster@haoziwan.xyz` |
| June 10, 01:08 | Abuse report filed with Cloudflare regarding the originating IP `104.28.160.168` |
| June 10, 01:09 | Third wave begins. Targets: `postmaster@haoziwan.xyz` and `abuse@haoziwan.xyz` |

## Technical Details

- **Originating IPs observed:** `104.28.160.168`, `104.28.159.154`
- **Attack routing:** Requests tunneled through **Cloudflare infrastructure** (CDN / WARP / proxy)
- **IP owner:** Cloudflare
- **Method:** Automated HTTP POST requests to mailing list subscription endpoints (Mailman, Sympa, etc.)
- **Volume:** Approximately 60+ confirmation emails within 14 minutes (01:00–01:14 UTC)
- **Mailing Lists Affected (partial list):**
  - spce-dev@lists.sipwise.com
  - announce@openvz.org
  - dnsdist@mailman.powerdns.com
  - freifunk-bonn@lists.kbu.freifunk.net
  - africanspiritcpt@lists.host-m.co.za
  - anu.malaysia.institute@anu.edu.au
  - *(Full list available in raw email logs)*

## Mitigation

Server-side filtering rules were implemented on the receiving MTA (Postfix) to reject incoming messages matching the following patterns:

- `From: *-request@*`
- `Subject: *confirm*subscription*`

No further action was taken to confirm or interact with any of the subscription requests.

## Raw Evidence

Preserved raw email files (.eml format) are available in the `raw-emls/` subdirectory, including sample evidence with full headers.

## Chronological Note (for awareness)

The following events occurred within a similar timeframe. Whether they are causally related is unknown and not asserted here — they are presented for chronological awareness only.

- **May 2026:** Abuse report sent from `abuse@haoziwan.xyz` regarding AS202734 BGP hijacking. Response received from the other party.
- **June 9, 2026 (prior to 22:52 UTC):** This evidence repository was published, listing `postmaster@haoziwan.xyz` as a contact.
- **June 9, 2026, 22:52 UTC:** First wave of subscription bombing targeting `postmaster@haoziwan.xyz` begins.
- **June 10, 2026, 01:08 UTC:** Abuse report filed with Cloudflare regarding the originating IP `104.28.160.168`.
- **June 10, 2026, 01:09 UTC:** Second email address (`abuse@haoziwan.xyz`) is added as a target.
- **June 10, 2026, 01:09 UTC and after:** The same Cloudflare egress IPs continued to be used for subscription requests targeting both addresses.

No direct evidence establishes that the party behind the AS202734 incident is also responsible for the subscription bombing. The temporal proximity is noted for completeness.

---

*Documented: June 10, 2026*