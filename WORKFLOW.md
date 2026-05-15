# Amanin Investigation Workflow

## Step 1 — Intake
User submits:
- screenshot
- URL
- message
- email

Orchestrator determines investigation path and **response language** (match user message and main attached text: Bahasa Indonesia vs English; see `AGENTS.md`).

---

## Step 2 — Content Extraction
OCR agent extracts relevant text.

---

## Step 3 — Threat Analysis
Threat Analyzer:
- identifies manipulation tactics
- extracts suspicious intent

---

## Step 4 — Infrastructure Investigation
Domain Investigator:
- validates URLs
- checks domain age
- checks reputation (passive WHOIS/DNS/SSL via optional `domain-intel`; optional vendor APIs per `domain-reputation-apis` when keys exist locally)

---

## Step 5 — Identity Validation
Identity Validator:
- analyzes sender identity
- validates legitimacy signals

---

## Step 6 — Risk Aggregation
Risk Assessor combines:
- behavioral risk
- technical risk
- contextual risk

---

## Step 7 — Human Explanation
Explainer Agent generates:
- concise summary
- explanation
- next actions
- output in the **same language** as the user’s intake (Bahasa Indonesia or English; see `AGENTS.md`)

---

# Autonomous Behavior

Agents dynamically decide:
- which tools to invoke
- whether deeper investigation is needed
- confidence level
- escalation path
