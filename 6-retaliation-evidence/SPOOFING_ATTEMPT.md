# Email Spoofing Attempt by MoeDove LLC (May 19, 2026)

## Evidence Files
- `cloudflare-dmarc-moedove-spoofing-haoziwan-2026-05-19.png`
- `cloudflare-dmarc-moedove-spoofing-haoziwan-2026-05-19.pdf`

## Important Dates

| Event | Date |
|-------|------|
| Spoofing attack (emails sent) | **May 19, 2026** |
| Evidence archived (screenshot captured) | June 10, 2026 |

> **Note**: The filenames reflect the **actual attack date** (May 19, 2026), not the screenshot/archival date (June 10, 2026). This maintains accurate chronological evidence.

## What This Shows

**Source**: Cloudflare Dashboard → Email Routing → DMARC Management

| Field | Value | Meaning |
|-------|-------|---------|
| From: Domain | `haoziwan.xyz` | The email **forged** the investigator's domain, pretending to be from the investigator |
| Source IP | `209.85.220.69` | Google mail server (US) |
| Sending Domain | **`moedove.com`** | **Actual sender = MoeDove LLC** (the attacker) |
| SPF | `Fail` | Attacker does not have permission to send on behalf of haoziwan.xyz |
| DKIM | `Fail` | Attacker lacks the investigator's DKIM private key |
| DMARC Fails | `12` | All 12 emails were rejected by the investigator's DMARC policy |
| Reporter | `google.com` | Data from official Google DMARC reports |

## Significance

1. **Immediate timing**: The spoofing attempt occurred within **hours** of the attacker's "To idiot" reply (May 19, 03:28 UTC) — see `/2-communication/` for that email.

2. **Clear attribution**: The `Sending Domain` field identifies **MoeDove LLC (`moedove.com`)** as the entity that **forged** `haoziwan.xyz` in the From header, attempting to make emails appear as if they originated from the investigator's domain.

3. **Failed attack**: All 12 emails were rejected because the investigator's DMARC policy is set to `p=reject`. No spoofed email was delivered.

4. **Official data**: The underlying DMARC reports come from **Google**, not from the investigator's own systems.

## Technical Summary

The attacker (MoeDove LLC / moedove.com) used Google's email infrastructure (IP `209.85.220.69`) to send 12 emails **forging `haoziwan.xyz` as the From domain**, attempting to impersonate the investigator. All attempts failed SPF, DKIM, and DMARC validation due to the investigator's properly configured email authentication policies.

## Evidence Integrity

- Screenshot captured from Cloudflare Dashboard on **June 10, 2026** (logged-in session, URL visible in browser)
- Underlying DMARC reports originated from **Google** (reporter: `google.com`)
- No data was modified or redacted; full context preserved for independent verification
