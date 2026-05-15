---
name: scam-verification
description: Verify whether a message, link, image, or claim is a scam, phishing attempt, or fraud. Covers SMS, WhatsApp, social media, email, and website verification.
triggers:
  - User shares suspicious message, SMS, screenshot, or link
  - User asks "is this real?", "is this a scam?", "tolong cek ini"
  - Claims of prizes, government aid, urgent bank issues, or account verification
  - Messages with URLs from unknown senders
---

# Scam Verification

## Workflow

### 1. Initial Visual/Text Analysis
- Read the message carefully. Extract all text.
- Flag urgency words: "SEGERA", "TERAKHIR", "BURUAN", "AKAN DIBLOKIR", "PESAN TERAKHIR"
- Flag monetary lures: prize winnings, government aid (BSU, BLT, bansos), unexpected refunds
- Check sender identity: is it a personal number, unknown email, or unofficial domain?
- Check language quality: excessive abbreviations, inconsistent branding, poor grammar

### 2. Domain & URL Investigation
Prefer **web_search** / **web_extract** (and skill/domain tools when enabled) for URLs. Use **terminal** only when the deployment allows it (e.g. local CLI); on restricted gateways (e.g. Telegram-only toolsets) rely on web + vision and explain limits.

```bash
# When terminal is available — probe suspicious links before visiting in browser
# Check DNS resolution
dig +short <domain>
host <domain>

# Check WHOIS for registration info
whois <domain> | grep -iE "Domain Name|Registrar|Creation Date|Name Server|Status"

# Fetch headers only (safe — does not load page content)
curl -sI "https://<url>" | head -20

# Check HTTP status. 502/403/redirect chains on scam domains are common.
```

Red flags:
- URL shorteners (`tiny.cc`, `bit.ly`) hiding final destination
- Domains using `.my.id`, `.web.id`, `.co.id` instead of `.go.id` for government claims
- Recently registered domains (check Creation Date)
- Cloudflare or other CDN fronting on obviously fake sites

### 3. Cross-Reference Official Sources
- For Indonesian government programs (BSU, BLT, Bansos): check **bsu.kemnaker.go.id**, **kemensos.go.id**, **bpjs.go.id**
- Look for official press releases or news from trusted outlets (detik, kompas, liputan6)
- Search for domain name + "penipuan" / "scam" / "phishing"

### 4. Image Analysis
If user provides screenshot:
- Use `vision_analyze` to extract all text and describe visual elements
- Check for official logos used without authorization
- Look for UI inconsistencies (non-standard message bubbles, fake app interfaces)

### 5. Response Format (Telegram/mobile)

Structure:
1. **Direct verdict** — bold scam/legitimate/uncertain label at top
2. **Evidence table** — 2-3 column markdown table of red flags + explanations
3. **Safe actions** — numbered list of concrete steps ("Jangan klik", "Hapus pesan", "Blokir")
4. **Official alternative** — if applicable, provide the real website or verified channel

Formatting rules:
- Use real Markdown lists (`-` or `1.`) not manual bullets
- One compact line per step (avoid multi-line templates per step)
- No horizontal rules (`---`)
- Use `[text](url)` for links, avoid raw URLs when possible
- Keep tables simple; prefer key: value pairs on small screens

### 6. Tone Rules
- Calm, clear, reassuring, evidence-driven
- Never shame the user for almost being scammed
- If risk is HIGH: be direct but not alarmist
- If uncertain: admit uncertainty honestly and suggest safe default actions

## Pitfalls

- **Do not** open suspicious links in browser directly. Use `curl -I` or `dig` first.
- **Do not** rely solely on visual appearance — scammers clone official graphics well.
- **Do not** forget to verify which ministry/agency actually runs a program. Scammers often attribute programs to wrong ministries (e.g., BSU is Kemnaker, not Kemensos).
- **Do not** provide long narrative explanations. Users on mobile need concise verdicts.

## References

- `references/indonesian-government-scam-indicators.md` — domain rules, official portals, common scam templates
