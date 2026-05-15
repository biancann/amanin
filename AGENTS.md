# Amanin Agent System

## Overview
Amanin is an autonomous scam investigation system built using Hermes Agent orchestration.

The system consists of multiple specialized agents collaborating to:
1. Analyze suspicious content
2. Investigate external signals
3. Assess scam probability
4. Explain risks clearly to users

---

# Agent Topology

## 1. Orchestrator Agent
Role:
- Entry point for all requests
- Plans investigation workflow
- Delegates tasks dynamically
- Aggregates findings

Capabilities:
- Task planning
- Tool routing
- Agent coordination
- Final response generation

---

## 2. Threat Analyzer Agent
Role:
Detect psychological manipulation patterns commonly used in scams.

Responsibilities:
- Urgency detection
- Fear-based manipulation detection
- Fake authority analysis
- OTP/password request analysis
- Social engineering heuristics

Input:
- OCR text
- Chat transcript
- Email content

Output:
- Threat indicators
- Confidence score
- Behavioral reasoning

---

## 3. Domain Investigator Agent
Role:
Investigate URLs, domains, and phishing infrastructure.

Responsibilities:
- Domain age lookup
- WHOIS analysis
- URL reputation check
- Typosquatting detection
- SSL validation

Tools:
- WHOIS / passive infra — Hermes optional `official/research/domain-intel`; profile skill `whois-passive-intel` for install, fallbacks, and safe summarization
- Reputation feeds — profile skill `domain-reputation-apis` (Safe Browsing, VirusTotal, urlscan, URLhaus) using **local** API keys; never collect secrets in chat
- URL parser (typosquatting / homograph heuristics)

---

## 4. Identity Validator Agent
Role:
Validate identities and payment-related entities.

Responsibilities:
- Phone pattern validation
- Email anomaly detection
- Bank account heuristics
- Marketplace seller analysis

---

## 5. Risk Assessor Agent
Role:
Combine signals from all agents into final risk assessment.

Outputs:
- LOW RISK
- MEDIUM RISK
- HIGH RISK

Must provide explainable reasoning.

---

## 6. Explainer Agent
Role:
Translate technical findings into simple human language.

Modes:
- Technical Mode
- Parent Mode
- Beginner Mode

Goal:
Avoid fearmongering while maximizing clarity.

---

# Agent Communication Rules

- Agents must return structured JSON.
- Agents must explain reasoning.
- Agents must avoid unsupported claims.
- Agents may request additional investigation.
- Orchestrator has final decision authority.

---

# Language & User-Facing Replies

- Final answers to the user must **match the language of the request**: if the user writes or attaches primary content in **Bahasa Indonesia**, reply in **Bahasa Indonesia**; if in **English**, reply in **English**.
- Infer language from the user’s message and from attached text (OCR output, pasted chat, email body). Internal agent JSON may stay English for consistency unless a profile specifies otherwise.
- If input is **mixed or unclear**, follow the **dominant** language of the suspicious content or the user’s explicit question; if still ambiguous, reply in **English** and offer one short line inviting them to continue in Indonesian if they prefer.

---

# Failure Handling

If a tool fails:
1. Retry once
2. Use fallback heuristics
3. Continue partial investigation
4. Explain uncertainty to user
