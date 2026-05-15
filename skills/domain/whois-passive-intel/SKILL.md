---
name: whois-passive-intel
description: Install and use Hermes optional skill official/research/domain-intel for passive WHOIS, RDAP-style registration signals, DNS, SSL, and subdomain hints via certificate transparency. No API keys. Use for domain age, registrar, nameservers, cert SANs, and bulk passive checks when investigating phishing or typosquats.
version: 1.0.0
author: Amanin profile
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [WHOIS, DNS, SSL, OSINT, Phishing, Domains]
    related_skills: [domain-intel, domain-reputation-apis]
---

# WHOIS and passive domain intelligence

## When to use this skill

- User or content mentions a **suspicious domain** and you need **registration age**, **registrar**, **nameservers**, **DNS records**, **TLS certificate** details, or **CT-based subdomain hints**.
- You want **passive** infrastructure context **without API keys** (Hermes `domain-intel` optional skill).

## Install (Hermes hub)

```bash
hermes skills install official/research/domain-intel
```

Verify:

```bash
hermes skills list | grep -i domain-intel
```

If the optional skill is unavailable, fall back to **terminal** only when appropriate:

- `whois example.com` (if the `whois` client exists in the environment)
- `dig +short NS example.com` / `dig +short TXT example.com`

Treat CLI WHOIS/RDAP output as **unverified context**; explain uncertainty to the user (see `SAFETY.MD`).

## How this fits Amanin

- **Domain Investigator** uses this path for **technical** signals (age, DNS, SSL), not for “this site is malicious” certainty.
- Pair with **`domain-reputation-apis`** when you need vendor **reputation** verdicts (often API-keyed).

## Output hygiene

- Do **not** paste raw PII from WHOIS into user chat; summarize (e.g. “recently registered”, “privacy-protected WHOIS”) in the user’s language.
- Never claim guilt from WHOIS alone; combine with behavior, URL shape, and optional reputation API results.
