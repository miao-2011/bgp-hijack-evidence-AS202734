# Email Spoofing Attempts by MoeDove LLC and Tianshome.net

## Evidence Files

- `cloudflare-dmarc-moedove-spoofing-haoziwan-2026-05-19.png`
- `cloudflare-dmarc-moedove-spoofing-haoziwan-2026-05-19.pdf`
- `2026-06-25_tianshome-net_spoofing-haoziwan-xyz_dmarc-report.pdf`

---

## Attempt 1: May 19, 2026 (MoeDove LLC)

| Event | Date |
|-------|------|
| Spoofing attack (emails sent) | May 19, 2026 |
| Evidence archived | June 10, 2026 |

**What This Shows**

Source: Cloudflare Dashboard → Email Routing → DMARC Management

| Field | Value | Meaning |
|-------|-------|---------|
| From: Domain | haoziwan.xyz | The email forged the investigator's domain |
| Source IP | 209.85.220.69 | Google mail server (US) |
| Sending Domain | moedove.com | Actual sender = MoeDove LLC |
| SPF | Fail | Attacker not authorized to send on behalf of haoziwan.xyz |
| DKIM | Fail | No valid DKIM signature from haoziwan.xyz |
| DMARC Fails | 12 | All 12 emails rejected by DMARC policy |
| Reporter | google.com | Data from official Google DMARC reports |

**Significance**

- Immediate timing: occurred within hours of the attacker's "To idiot" reply (May 19, 03:28 UTC)
- Clear attribution: sending domain identifies MoeDove LLC as the sender
- Failed attack: all 12 rejected because DMARC policy is set to p=reject
- Official data: DMARC reports from Google, not self-reported

---

## Attempt 2: June 25, 2026 (Tianshome.net)

| Event | Date |
|-------|------|
| Spoofing attack (email sent) | June 25, 2026 |
| Evidence archived | June 26, 2026 |

**What This Shows**

Source: Cloudflare Dashboard → Email Routing → DMARC Management

| Field | Value | Meaning |
|-------|-------|---------|
| From: Domain | haoziwan.xyz | The email forged the investigator's domain |
| Source IP | 57.103.76.251 | Apple Inc. mail infrastructure (US) |
| Sending Domain | tianshome.net | Actual sender = Tianshome.net |
| SPF | Fail | Attacker not authorized to send on behalf of haoziwan.xyz |
| DKIM | Pass | Valid DKIM signature from tianshome.net |
| DMARC Result | Passed (DKIM-aligned) | DMARC passed because DKIM was valid |
| Reporter | google.com | Data from official Google DMARC reports |

**Significance**

- Follow-up attempt: 37 days after the first spoofing attempt
- Adapted tactic: added a valid DKIM signature from tianshome.net to evade detection
- Clear attribution: sending domain identifies Tianshome.net as the sender
- SPF still failed: IP not authorized in haoziwan.xyz SPF record
- Official data: DMARC reports from Google, not self-reported

---

## Combined Technical Summary

| Attempt | Date | Sending IP | IP Owner | Display Domain | Actual Sender | SPF | DKIM | DMARC | Count |
|---------|------|-----------|----------|----------------|---------------|-----|------|-------|-------|
| 1 | 2026-05-19 | 209.85.220.69 | Google LLC | haoziwan.xyz | moedove.com | Fail | Fail | Rejected | 12 |
| 2 | 2026-06-25 | 57.103.76.251 | Apple Inc. | haoziwan.xyz | tianshome.net | Fail | Pass | Passed | 1 |

The attacker (MoeDove LLC / Tianshome.net) used third-party email infrastructure (Google / Apple) to send emails forging `haoziwan.xyz` as the From domain, attempting to impersonate the investigator. The June 25 attempt added a DKIM signature from `tianshome.net` to evade detection, demonstrating deliberate technical adaptation rather than accidental misconfiguration.

Both sending domains (`moedove.com`, `tianshome.net`) are directly affiliated with AS202734, the party accused of BGP hijacking.

---

## Evidence Integrity

- Screenshots captured from Cloudflare Dashboard (logged-in session, URL visible)
- Underlying DMARC reports originated from Google (reporter: google.com)
- No data was modified or redacted; full context preserved for independent verification
