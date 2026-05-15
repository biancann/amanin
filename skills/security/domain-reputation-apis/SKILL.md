---
name: domain-reputation-apis
description: Query URL and domain reputation using public HTTP APIs (Google Safe Browsing Lookup v4, VirusTotal v3, urlscan.io, URLhaus) via curl or small scripts. Requires API keys in local env — never ask users to paste secrets into chat. Use after extracting a hostname or URL from a scam report.
version: 1.0.0
author: Amanin profile
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [Reputation, SafeBrowsing, VirusTotal, URLhaus, Phishing, APIs]
    related_skills: [whois-passive-intel, native-mcp]
---

# Domain and URL reputation APIs

## When to use this skill

- You need **vendor reputation** or **malware/phishing feeds** beyond passive DNS/WHOIS/SSL.
- The user shared a **URL or domain** and you can justify an evidence-backed **“known bad / suspicious / unknown”** framing (not legal accusations).

## Credentials (critical)

- Put keys in **`~/.hermes/.env`** or your host environment; reference them as **`$VAR`** in local commands only.
- **Never** ask the user to paste API keys into Telegram or other channels. Point them to local `hermes setup` / `.env` on their machine (see Hermes skills docs on secrets).

Suggested variable names (examples only — use what you actually configure):

- `GOOGLE_SAFEBROWSING_API_KEY` — [Google Safe Browsing API](https://developers.google.com/safe-browsing)
- `VIRUSTOTAL_API_KEY` — [VirusTotal API](https://docs.virustotal.com/reference/overview)
- `URLSCAN_API_KEY` — [urlscan.io API](https://urlscan.io/docs/api/)
- `URLHAUS` — optional; many URLhaus endpoints work **without** a key for limited lookups (check current docs).

## Safe Browsing Lookup API (v4) — example

Threat lists change; always follow current Google docs. Pattern:

```bash
curl -sS "https://safebrowsing.googleapis.com/v4/threatMatches:find?key=${GOOGLE_SAFEBROWSING_API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{"client":{"clientId":"amanin-hermes","clientVersion":"1.0.0"},"threatInfo":{"threatTypes":["MALWARE","SOCIAL_ENGINEERING","UNWANTED_SOFTWARE","POTENTIALLY_HARMFUL_APPLICATION"],"platformTypes":["ANY_PLATFORM"],"threatEntryTypes":["URL"],"threatEntries":[{"url":"https://example.com/path"}]}}'
```

Interpret **empty `matches`** as “no hit in queried lists,” not “guaranteed safe.”

## VirusTotal — v3 domain report (example)

```bash
curl -sS -H "x-apikey: ${VIRUSTOTAL_API_KEY}" \
  "https://www.virustotal.com/api/v3/domains/example.com"
```

Summarize **detection counts** and **last_analysis_stats**; avoid dumping full vendor JSON into chat.

## urlscan — submit or retrieve (example)

Use when you need **rendered page / redirect chain** context (still not proof of criminality). Respect **rate limits** and **terms of use**.

```bash
curl -sS -H "API-Key: ${URLSCAN_API_KEY}" -H "Content-Type: application/json" \
  -d '{"url":"https://example.com","visibility":"public"}' \
  "https://urlscan.io/api/v1/scan/"
```

## URLhaus — abuse.ch (example)

Check current URLhaus API docs for the exact path and parameters; many workflows use **payload or URL lookups** for malware distribution links.

```bash
curl -sS "https://urlhaus-api.abuse.ch/v1/url/" -d "url=https://example.com/malware.bin"
```

## MCP alternative

If you prefer tools instead of ad-hoc `curl`, configure an MCP server under `mcp_servers` in `config.yaml` and follow **`native-mcp`** (`skills/mcp/native-mcp/SKILL.md`) so discovered tools appear as `mcp_*` calls.

## Amanin-specific rules

- Reputation = **signal**, not a verdict. Combine with **Threat Analyzer** (urgency, OTP requests, impersonation) and **passive WHOIS/DNS** from **`whois-passive-intel`**.
- On API failure: retry once, then continue with heuristics and state uncertainty (`AGENTS.md` failure handling).
