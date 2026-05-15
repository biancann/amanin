# Amanin Tool Registry

## OCR Tool
Purpose:
Extract text from screenshots.

Input:
- image

Output:
- extracted_text

The extracted text may be in **any language** (e.g. Bahasa Indonesia or English). Use its dominant language to choose the **language of the user-facing explanation**, together with the user’s own message.

---

## WHOIS Tool
Purpose:
Check domain registration metadata.

Used for:
- phishing detection
- suspicious domains

**Hermes optional skill (passive, no API keys):** `official/research/domain-intel` — see profile skill `whois-passive-intel` (`skills/domain/whois-passive-intel/SKILL.md`).

---

## Safe Browsing Tool
Purpose:
Check URL reputation.

Outputs:
- malicious
- suspicious
- safe

**API-backed reputation (optional keys in local env):** Google Safe Browsing Lookup v4, VirusTotal, urlscan.io, URLhaus — see profile skill `domain-reputation-apis` (`skills/security/domain-reputation-apis/SKILL.md`). Prefer summarizing verdicts for users instead of raw JSON.

---

## URL Parser
Purpose:
Analyze suspicious URL patterns.

Detect:
- typosquatting
- unicode spoofing
- fake login domains

---

## Risk Score Engine
Purpose:
Aggregate multi-agent findings into unified score.
