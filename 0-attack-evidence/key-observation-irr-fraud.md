## Key Observation: IRR Fraud

A large number of hijacked prefixes appear as **"IRR Valid"** in the Hurricane Electric BGP Toolkit output, including prefixes belonging to China Unicom, China Telecom, Alibaba Cloud, and many others.

This incident provides a textbook example of **registry-layer fraud enabling routing-layer hijacking**.

### Why this is significant:

**1. IRR is not a security system.**
"IRR Valid" merely means a matching route object exists in a public registry — **not** that the origin ASN is authorized by the actual address holder. IRR databases are not cryptographically protected.

**2. The attacker exploited this.**
He did not merely announce hijacked prefixes. He also **created or modified IRR route objects** for prefixes he did not own — making them appear routable and legitimate to networks that filter based on IRR data.

**3. This is deliberate fraud.**
Creating route objects for prefixes you do not own is **not** a configuration error. It is an intentional act to **weaponize the registry itself** in order to bypass routing filters and evade detection.

### What the HE data reveals:

The attacker's IRR manipulation was not uniformly successful. The data shows a mix of outcomes:

| IRR Status in HE Output | Meaning | Observed Examples |
|------------------------|---------|-------------------|
| **"IRR Valid"** | Attackersucceeded in creating or modifying a route object for this prefix | China Unicom (`2408:840c::/40`), China Telecom (`240e::/20`), Baidu (`240c:4000::/22`) |
| **"IRR Parent Invalid"** | Attacker created a route object, but the parent object is missing or invalid — a partially successful/failed forgery | A large number of China Unicom prefixes (`2408:8409:b600::/42`, etc.) |
| *(blank / no IRR data)* | Attacker did not (or could not) create a route object for this prefix | Some China Telecom prefixes, various others |

**The "IRR Parent Invalid" entries are particularly revealing.** They show the attacker *tried* to create route objects for these prefixes, but the objects were incomplete or incorrectly formatted. This is evidence of **attempted fraud**, not accidental misconfiguration.

### What RPKI shows:

While the attacker could manipulate IRR, he could not manipulate RPKI. For the same hijacked prefixes:

- **RPKI status is consistently empty or Invalid**
- No valid ROA authorizing AS202734 was ever issued by the true address holders

**This is not a coincidence. The attacker attacked the weak link (IRR) and avoided the strong link (RPKI).**

### Implication:

This incident demonstrates that **IRR-based filtering alone is insufficient** to prevent BGP hijacking. Operators relying solely on IRR are effectively blind to this class of attack. **RPKI-based origin validation (ROV) is required** to provide meaningful defense.

The presence of both successful ("IRR Valid") and failed/partial ("IRR Parent Invalid") forgeries provides clear evidence of a **deliberate, systematic attempt to manipulate routing registries** — not a one-off error or accident.
