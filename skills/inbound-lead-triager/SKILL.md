---
name: inbound-lead-triager
description: Scores inbound lead intent, qualifies against SME ICP in under 60 seconds, and executes automated routing (calendar injection, founder alert, or self-serve nurture).
---

# Inbound Lead Triager

Triage inbound sales inquiries in under 60 seconds. Scores commercial fit and buying intent using an SME-adapted SPICED framework, then dispatches the lead into instant calendar routing, founder notification, or automated nurture.

## When to use this skill

Trigger this skill when:
- An inbound webhook fires from a web form (Tally, Typeform, HubSpot, Webflow, WordPress).
- An inbound email arrives in a sales inbox (`sales@`, `info@`, `contact@`).
- A lead submits an inquiry via direct message (LinkedIn, WhatsApp Business, Telegram).
- A sales rep or founder enters raw lead text and asks: "Score this lead", "Should I take this call?", or "Triage this inbound".

Do not use this skill for:
- Outbound cold prospecting or list building.
- Generating pre-call discovery briefs (use `call-brief-generator`).
- Writing post-call follow-ups or proposals.

---

## The 60-second speed-to-lead benchmark

Responding within 5 minutes yields a 21x higher qualification rate compared to responding at 30 minutes. After 5 minutes, conversion drops by 8x. Lean SMEs cannot staff 24/7 human SDR teams without crushing margins. This automated triage pipeline replaces full-time triage reps with an LLM evaluation step that runs in 2 to 4 seconds for under \$0.01 per lead.

```
Inbound Submission
       │ (<1 sec)
       ▼
Data Sanitization & Domain Check
       │ (<2 sec)
       ▼
LLM Scoring Engine (Fit x Intent)
       │ (<3 sec)
       ▼
Decision Matrix
 ├── Score 75-100 (Tier 1: Hot)  ──> Direct Calendar Routing + Instant Slack/SMS Alert
 ├── Score 50-74  (Tier 2: Warm) ──> Async Micro-Qualification (SMS/Email Form)
 └── Score 0-49   (Tier 3: Cold) ──> Zero-Touch Self-Serve Resource Redirect
```

---

## Core qualification matrix

The system scores inbound leads on a 100-point scale split into two equal dimensions: **ICP Fit (50 points)** and **Buying Intent (50 points)**.

### 1. ICP Fit Matrix (50 points max)

| Factor | Criteria | Points |
| :--- | :--- | :--- |
| **Email Domain** | Verified corporate domain (e.g. `@acmecorp.com`) | 15 |
| | Generic email domain with verified company name provided | 8 |
| | Disposable, student (`.edu`), or anonymous email | 0 |
| **Authority / Seniority** | C-Suite, Founder, Owner, VP, Managing Director | 15 |
| | Director, Head of Department | 10 |
| | Manager, Individual Contributor, Consultant | 5 |
| | Student, Intern, Job Seeker | 0 |
| **Company Scale** | 10 to 250 employees or \$1M to \$25M annual revenue | 15 |
| | 5 to 9 employees or \$500k to \$1M annual revenue | 10 |
| | Enterprise (500+ employees) requiring custom RFP cycles | 5 |
| | Solo operator or pre-revenue idea stage | 0 |
| **Geographic / Market Alignment** | Primary target operating region | 5 |
| | Secondary region / English-speaking international | 3 |
| | Non-serviceable geography or sanctioned market | 0 |

### 2. Buying Intent Matrix (50 points max)

| Factor | Criteria | Points |
| :--- | :--- | :--- |
| **Problem Specificity** | Cites concrete pain, lost revenue, tool failure, or bottleneck | 20 |
| | Cites general business goal ("want to grow sales", "need marketing") | 10 |
| | Blank or generic message ("call me", "test", "pricing please") | 2 |
| **Timeline / Critical Event** | Immediate deadline (this week, within 30 days, contract renewal) | 15 |
| | Mid-term timeline (next quarter, 1-3 months) | 8 |
| | Vague timeline ("exploring options", "just looking", "no rush") | 2 |
| **Budget / Investment Anchor** | Mentions existing budget, paid tool replacement, or pricing tier match | 10 |
| | Acknowledges paid engagement expectation | 5 |
| | Asks for free work, unpaid trial, or revenue share only | 0 |
| **Inbound Action Trigger** | Direct demo or discovery call request | 5 |
| | Downloaded high-intent asset (case study, calculator, audit request) | 3 |
| | General newsletter or contact form ping | 1 |

---

## Lead tiering and routing logic

### Tier 1: Hot Lead (Score 75 to 100)
- **Status:** High ICP fit and acute commercial pain.
- **Action:** Route immediately to closer's calendar.
- **Speed target:** < 60 seconds.
- **Automation actions:**
  1. Trigger instant personalized email/SMS with direct calendar booking link (Round-Robin or Closer).
  2. Send high-priority Slack/Telegram notification to sales channel with lead summary and phone number.
  3. Update CRM status to `Hot Inbound - Routing Triggered`.

### Tier 2: Warm Lead (Score 50 to 74)
- **Status:** Potential fit, but ambiguous budget, generic timeline, or junior contact.
- **Action:** Async qualification via automated one-question nudge.
- **Speed target:** < 3 minutes.
- **Automation actions:**
  1. Send automated reply asking 1 clarifying question (e.g. current tool stack or primary bottleneck).
  2. Post low-priority Slack notification for rep review within 4 business hours.
  3. Update CRM status to `Warm Inbound - Pending Async Qual`.

### Tier 3: Disqualified / Self-Serve (Score 0 to 49)
- **Status:** Mismatched ICP, job seeker, student, or freebie collector.
- **Action:** Zero human rep involvement. Divert to self-serve content.
- **Speed target:** Instant batch or single execution.
- **Automation actions:**
  1. Send polite automated acknowledgment routing them to documentation, free community, or YouTube video.
  2. Update CRM status to `Closed - Unqualified`.
  3. Suppress all human notifications.

---

## Execution prompt for the triage engine

Use this exact prompt within your automation node (n8n, Make, or Python runner).

```text
You are an uncompromising sales operations triager for a high-efficiency SME.
Your job is to analyze inbound lead data, score it on a 0-100 scale using the ICP Fit (50 pts) and Intent (50 pts) rubric, and output a structured JSON routing decision.

Evaluation Rules:
1. Score fit strictly. If the email domain is gmail.com/yahoo.com and no legitimate company website is listed, award 0 points for email domain.
2. Detect intent from message depth. Penalize generic one-liners. Reward specific bottleneck descriptions.
3. Be completely objective. Do not inflate scores out of politeness.
4. Output valid JSON only. Do not wrap in markdown or add conversational filler.

Rubric:
ICP Fit (0-50):
- Domain: 15 (Corporate), 8 (Generic + Company name), 0 (Burner/Unknown)
- Authority: 15 (Founder/C-Suite/Owner), 10 (Director/VP), 5 (Manager), 0 (Student/Intern/Other)
- Scale: 15 (10-250 staff / $1M-$25M), 10 (5-9 staff / $500k-$1M), 5 (Enterprise 500+), 0 (Solo/Pre-revenue)
- Region: 5 (Primary target market), 3 (Secondary market), 0 (Outside footprint)

Intent (0-50):
- Pain Specificity: 20 (Exact metric/failure cited), 10 (Broad goal), 2 (Vague/Blank)
- Timeline: 15 (Urgent/Under 30 days), 8 (1-3 months), 2 (No timeline/Vague)
- Budget Signal: 10 (Budget ready/Paid replacement), 5 (Expects paid), 0 (Wants free/equity)
- Channel Action: 5 (Demo/Call request), 3 (Audit/Calculator), 1 (General contact)

JSON Output Schema:
{
  "lead_name": "string",
  "company_name": "string",
  "total_score": integer (0-100),
  "fit_score": integer (0-50),
  "intent_score": integer (0-50),
  "tier": "TIER_1_HOT" | "TIER_2_WARM" | "TIER_3_COLD",
  "routing_action": "INSTANT_CALENDAR" | "ASYNC_QUALIFY" | "SELF_SERVE_DIVERT",
  "score_reasons": [
    "string: concise fit reason",
    "string: concise intent reason"
  ],
  "lead_summary": "1-sentence executive summary of the lead request",
  "suggested_reply": "Plain-text, anti-slop auto-response text ready to send"
}
```

---

## Anti-slop communication rules

When generating automated responses to inbound leads:

1. **Cut pleasantries.** Never write "Thanks for reaching out to our team! We hope this email finds you well." Start with what they submitted.
2. **Never use AI jargon.** Ban: "delve", "streamline", "leverage", "bespoke solutions", "game-changer", "transformative".
3. **Reference their exact words.** If they said "HubSpot sync fails on Fridays", say "Saw your note regarding HubSpot sync failing on Fridays."
4. **Offer one clean action.** Never give two booking links and three PDF attachments. One link or one direct question.
5. **Keep replies under 75 words.** Busy decision-makers read emails on phones between tasks.

---

## Production examples

### Example 1: Tier 1 (Hot Inbound)

#### Input
```json
{
  "name": "Marcus Vance",
  "email": "marcus@vancemanufacturing.com",
  "company": "Vance Industrial Supply",
  "headcount": "35",
  "message": "Our warehouse order tracking breaks every time our Shopify dropship feed syncs with NetSuite. Losing around 4 hours a day in manual order entry right now. Need this fixed before our Q4 inventory rush starts on November 1st. Looking to book a discovery call this week.",
  "source": "Website Demo Request"
}
```

#### Output
```json
{
  "lead_name": "Marcus Vance",
  "company_name": "Vance Industrial Supply",
  "total_score": 92,
  "fit_score": 45,
  "intent_score": 47,
  "tier": "TIER_1_HOT",
  "routing_action": "INSTANT_CALENDAR",
  "score_reasons": [
    "Corporate domain, manufacturing SME with 35 employees in core target market (+45 fit)",
    "Acute technical pain (Shopify to NetSuite sync), 4 hrs/day quantified waste, hard Nov 1 deadline (+47 intent)"
  ],
  "lead_summary": "Owner of a 35-person industrial supplier losing 4 hrs/day on Shopify-NetSuite sync; requires fix before Nov 1 peak.",
  "suggested_reply": "Marcus,\n\nSaw your note on the Shopify to NetSuite sync breaking order tracking. We handle this exact workflow bottleneck.\n\nGrab 15 minutes directly on my calendar here so we can review your schema and fix this before November: [Direct Calendar Link]\n\nBest,\nAmir"
}
```

#### Slack alert payload (Generated for Sales Channel)
```text
🔥 HOT LEAD: Marcus Vance | Vance Industrial Supply (35 FTEs)
Score: 92/100 (Fit: 45 | Intent: 47)
Pain: Shopify <-> NetSuite sync breaking, wasting 4 hrs/day.
Deadline: Nov 1 (Q4 rush).
Action: Calendar link sent via email.
Phone/Email: marcus@vancemanufacturing.com
```

---

### Example 2: Tier 2 (Warm Inbound)

#### Input
```json
{
  "name": "Sarah Jenkins",
  "email": "sarah.growth@gmail.com",
  "company": "BrightPath Consulting",
  "headcount": "8",
  "message": "Interested in learning more about your automation services for agencies. What are your pricing packages?",
  "source": "General Contact Form"
}
```

#### Output
```json
{
  "lead_name": "Sarah Jenkins",
  "company_name": "BrightPath Consulting",
  "total_score": 58,
  "fit_score": 28,
  "intent_score": 30,
  "tier": "TIER_2_WARM",
  "routing_action": "ASYNC_QUALIFY",
  "score_reasons": [
    "Gmail address used, but legitimate 8-person consultancy named (+28 fit)",
    "Generic pricing inquiry without stated workflow pain or timeline (+30 intent)"
  ],
  "lead_summary": "Agency operator asking for pricing packages without stating specific workflow bottlenecks.",
  "suggested_reply": "Sarah,\n\nGot your note about automation for BrightPath.\n\nOur custom implementations typically run between $3,500 and $7,500 depending on the workflows involved.\n\nWhat is the single biggest bottleneck your team spends manual hours on each week right now?\n\nBest,\nAmir"
}
```

---

### Example 3: Tier 3 (Cold / Disqualified)

#### Input
```json
{
  "name": "Alex Kumar",
  "email": "alexk99@gmail.com",
  "company": "N/A",
  "headcount": "1",
  "message": "Hi, I am a university student writing a paper on AI sales tools. Can I get a free trial account or have an interview with your founder for 30 minutes?",
  "source": "Footer Contact Form"
}
```

#### Output
```json
{
  "lead_name": "Alex Kumar",
  "company_name": "N/A",
  "total_score": 10,
  "fit_score": 5,
  "intent_score": 5,
  "tier": "TIER_3_COLD",
  "routing_action": "SELF_SERVE_DIVERT",
  "score_reasons": [
    "Individual student, zero business headcount, personal email (+5 fit)",
    "Academic inquiry seeking free trial and founder interview, zero commercial intent (+5 intent)"
  ],
  "lead_summary": "Student requesting free account and founder interview for academic paper.",
  "suggested_reply": "Alex,\n\nThanks for reaching out. We do not offer free student accounts or founder interviews due to client workload.\n\nYou can review our public technical case studies and architectural breakdowns on our resource hub here: [Public Hub Link]\n\nBest of luck with your research."
}
```
