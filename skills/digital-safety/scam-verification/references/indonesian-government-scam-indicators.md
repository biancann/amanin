# Indonesian Government Scam Indicators

## Official Domain Rules

| Claimed Entity | Real Domain Pattern | Common Fake Patterns |
|---|---|---|
| Kementerian Ketenagakerjaan (Kemnaker) | `*.kemnaker.go.id` | `*mybsu.my.id`, `*bsu.*.co.id` |
| Kementerian Sosial (Kemensos) | `*.kemensos.go.id` | `*bansos.*`, `*blt.*.web.id` |
| BPJS Ketenagakerjaan | `*.bpjs.go.id`, `bpjsketenagakerjaan.go.id` | `*bpjs*.xyz`, `*bpjs*.co.id` |
| Bank BRI/BNI/Mandiri/BCA | `*.bri.co.id`, `*.bni.co.id`, etc. | URL shorteners, lookalike domains |
| Shopee/Tokopedia | `shopee.co.id`, `tokopedia.com` | `shopee-prize.*`, `shopee-undian.*` |

Key rule: **All Indonesian government ministries use `.go.id`.** Any government program link ending in `.my.id`, `.web.id`, `.co.id`, or using a URL shortener (`tiny.cc`, `bit.ly`, `s.id`) is **not official**.

## Common Scam Templates (Indonesia)

### 1. Fake Prize / Undian
- Pattern: "SELAMAT Anda mendapat hadiah Rp XXX juta dari [Brand]"
- URL shortener + code/ID
- Red flag: Did not enter any contest

### 2. Fake Government Aid (BSU/BLT/Bansos)
- Pattern: "Kementerian X menyalurkan BSU Rp 600.000"
- Fake urgency: "Cek sekarang agar tidak terlewat"
- Often attributes program to wrong ministry (BSU is Kemnaker, not Kemensos)

### 3. Account Block / Verification
- Pattern: "Rekening Anda akan diblokir", "Verifikasi data Anda sekarang"
- Links to fake banking login pages

### 4. Package / Delivery
- Pattern: "Paket Anda tertahan", "Biaya ongkir belum dibayar"
- Fake logistics companies

## Safe Verification Commands

```bash
# Headers only — safest first probe
curl -sI "https://SUSPICIOUS_URL" | head -20

# DNS lookup
dig +short SUSPICIOUS_DOMAIN
host SUSPICIOUS_DOMAIN

# Search for scam reports
curl -s "https://www.bing.com/search?q=DOMAIN+penipuan" -H "User-Agent: Mozilla/5.0"
```

## Official Portals to Bookmark

- BSU check: https://bsu.kemnaker.go.id
- Kemensos programs: https://www.kemensos.go.id
- BPJS Ketenagakerjaan: https://www.bpjsketenagakerjaan.go.id
- Police cyber crime report: https://www.cyber.polri.go.id (if applicable)
