---
name: call-brief-generator
description: Generates high-density, 3-minute pre-call intelligence briefs from prospect data, company signals, and SPICED discovery frameworks for SME sales closers.
---

# Call Brief Generator

Generate high-density, one-page pre-call intelligence briefs in under 10 seconds. Synthesizes prospect data, company tech signals, hiring moves, and inbound context into an actionable battleplan based on the SPICED discovery framework.

## When to use this skill

Trigger this skill when:
- A prospect books a call via Calendly, SavvyCal, ChiliPiper, or HubSpot Meetings.
- A scheduled sales meeting is 15 minutes away (automated pre-meeting alert).
- A sales rep or founder enters prospect details and requests: "Prep me for my call with [Name]", "Generate call brief for [Company]", or "Give me discovery angles for this meeting".

Do not use this skill for:
- Initial inbound scoring or routing (use `inbound-lead-triager`).
- Sending automated emails or DMs directly to the prospect.
- Post-call CRM summary logging.

---

## The 3-minute pre-call preparation rule

Top-performing closers do not waste the first 10 minutes of a call asking: "So, tell me a little bit about what your company does." Asking basic questions that exist on Google signals laziness and destroys positioning.

This skill produces an executive brief scannable in under 3 minutes before jumping on Zoom. It answers five critical questions:
1. Who is across the screen and how much buying power do they hold?
2. What does their company actually sell, to whom, and at what scale?
3. What is their likely acute pain based on tech stack, hiring, and inbound notes?
4. What exact 3 discovery questions will expose their cost of inaction?
5. What landmines or objections should be preempted?

```
Trigger (Meeting Booked / 15m Alert)
               │
               ▼
Prospect & Company Data Extraction
 (Inbound Notes + LinkedIn + Tech Stack Signals + Job Board)
               │
               ▼
SPICED Analytical Synthesis
 (Situation ➔ Pain ➔ Impact ➔ Critical Event ➔ Decision)
               │
               ▼
High-Density Intelligence Brief (Slack Card / CRM Note / Notion Page)
```

---

## Intelligence brief architecture

Every generated brief follows a rigid, 5-block structure designed for instant cognitive pickup:

### Block 1: Executive Snapshot
- Prospect name, exact job title, estimated decision authority (High / Medium / Low).
- Company name, business model, core customer ICP, estimated headcount, and revenue tier.
- Detected technology stack (CRM, eCommerce platform, marketing tools, ERP).

### Block 2: Commercial Signals & Triggers
- Active hiring openings (e.g. hiring manual coordinators indicates process bottleneck).
- Recent company announcements, product launches, or funding events.
- Prospect career background (tenure in role, previous companies).

### Block 3: The Commercial Hypothesis
- **Hypothesis:** Why they booked this call now.
- **Current Workaround:** How they are currently solving the problem (manual spreadsheets, junior contractors, broken automations).
- **Cost of Inaction:** Quantified weekly cash or labor loss if they do nothing for another 90 days.

### Block 4: SPICED Call Battleplan
- **Opening Hook (First 30 seconds):** Casual, observant entry line that builds immediate credibility without generic small talk.
- **Probe 1 (Situation & Baseline):** Validates current operational setup without interrogation.
- **Probe 2 (Pain & Financial Bleed):** Unpacks the exact bottleneck and identifies who owns the headache.
- **Probe 3 (Critical Event Anchor):** Anchors the problem to an upcoming deadline or target date.

### Block 5: Landmines & Objection Preempts
- **Landmines:** Topics, competitors, or assumptions to avoid mentioning.
- **Likely Objection:** The most probable pushback based on their company size and model.
- **Turnaround Script:** Exact phrasing to neutralize the objection in 2 sentences.

---

## Prompt template for brief generation

Use this prompt within your automation runner (Python, n8n, Make, or LLM agent):

```text
You are an elite sales strategist preparing an SME founder or sales closer for an upcoming discovery call.
Your job is to synthesize prospect details, inbound form data, tech stack clues, and web signals into an ultra-dense, anti-slop Pre-Call Intelligence Brief.

Rules:
1. Zero corporate puffery. Do not summarize their generic mission statement ("They strive to deliver innovative excellence").
2. Focus on money, time, bottlenecks, and decision dynamics.
3. Every insight must connect directly to a discovery question or commercial reason to buy.
4. Discovery questions must follow the SPICED methodology (Situation, Pain, Impact, Critical Event, Decision).
5. Output clean Markdown following the exact template below.

Template:
# PRE-CALL BRIEF: [Prospect Name] | [Company Name]
**Call Time:** [Meeting Date/Time] | **Lead Source:** [Source]

### 1. Executive Snapshot
- **Prospect:** [Name] ([Title]) | **Authority:** [High/Medium/Low] | **Tenure:** [Years/Months]
- **Company:** [Company Name] ([Website]) | **Model:** [B2B / D2C / Agency / etc.]
- **Scale:** [Headcount] employees | Est. Revenue: [Revenue Tier]
- **Detected Stack:** [Tools / Platforms detected]

### 2. Live Commercial Signals
- **Hiring Signals:** [Key open roles and what operational strain they indicate]
- **Growth / Company Triggers:** [Recent news, site updates, or product launches]
- **Inbound Context:** [Raw quote or summary of what they submitted in the form]

### 3. Commercial Hypothesis & Cost of Inaction
- **Core Hypothesis:** [Why they need us right now]
- **Current Workaround:** [What duct-tape solution they are using today]
- **Cost of Inaction:** [Quantified waste in hours, dollars, or lost deals per month]

### 4. SPICED Call Battleplan
- **30-Second Opening Hook:** "[Direct, observant opening phrase that references their specific situation]"
- **Probe 1 (Situation):** "[Open-ended question testing their current architecture/workflow]"
- **Probe 2 (Pain & Impact):** "[Question isolating the cost, hours lost, or friction on the team]"
- **Probe 3 (Critical Event):** "[Question pinning down their deadline or upcoming milestone]"

### 5. Landmines & Objection Preempts
- **Landmine:** [What NOT to say or assume]
- **Anticipated Objection:** "[Exact pushback they are likely to raise]"
- **Preemptive Script:** "[2-sentence response turning the objection into a reason to buy]"
```

---

## Anti-slop briefing rules

1. **Delete all corporate fluff.** Never write "They are a market-leading provider of customer-centric solutions." Write: "They sell industrial valves to regional HVAC contractors."
2. **Never ask canned discovery questions.** Ban: "What keeps you up at night?", "Where do you see yourself in 5 years?", "Can you tell me about your journey?"
3. **No generic advice.** Ban advice like "Build rapport and establish trust." Every bullet must provide specific data or exact word-for-word scripts.
4. **Quantify everything.** Never say "They are wasting a lot of time." Say "Estimated 15-20 hours per week of manual data re-entry across 2 coordinators."
5. **Keep the entire brief on one scannable screen.** If a rep has to scroll through pages of background text, the brief fails.

---

## Production examples

### Example 1: B2B E-Commerce & Industrial Supplier (Founder-Led Sales)

#### Input Context
```json
{
  "prospect_name": "Marcus Vance",
  "title": "Managing Director & Owner",
  "linkedin_url": "https://linkedin.com/in/marcus-vance-example",
  "company": "Vance Industrial Supply",
  "website": "vancemanufacturing.com",
  "headcount": 35,
  "inbound_notes": "Our warehouse order tracking breaks every time our Shopify dropship feed syncs with NetSuite. Losing around 4 hours a day in manual order entry right now. Need this fixed before our Q4 inventory rush starts on November 1st.",
  "detected_tools": ["Shopify Plus", "NetSuite", "Klaviyo", "Zendesk"],
  "open_roles": ["Warehouse Logistics Coordinator (2 openings)"]
}
```

#### Generated Intelligence Brief

# PRE-CALL BRIEF: Marcus Vance | Vance Industrial Supply
**Call Time:** Today at 2:00 PM EST | **Lead Source:** Website Demo Form

### 1. Executive Snapshot
- **Prospect:** Marcus Vance (Managing Director & Owner) | **Authority:** High (Sole signer) | **Tenure:** 9 years (Founded company)
- **Company:** Vance Industrial Supply ([vancemanufacturing.com](https://vancemanufacturing.com)) | **Model:** B2B Wholesale & Dropship Supply
- **Scale:** 35 employees | Est. Revenue: $8M - $12M ARR
- **Detected Stack:** Shopify Plus, NetSuite ERP, Zendesk, Klaviyo

### 2. Live Commercial Signals
- **Hiring Signals:** Currently hiring 2 Warehouse Logistics Coordinators. Indicates operational headcount is being thrown at manual data entry problems.
- **Growth / Company Triggers:** Expanded B2B catalog by 400 SKUs last quarter; inventory volume is outpacing legacy software connectors.
- **Inbound Context:** "Our warehouse order tracking breaks every time our Shopify dropship feed syncs with NetSuite. Losing around 4 hours a day in manual order entry right now. Need this fixed before Nov 1st."

### 3. Commercial Hypothesis & Cost of Inaction
- **Core Hypothesis:** Marcus is paying warehouse staff to manually reconcile Shopify orders into NetSuite because their default Celigo/native integration errors out on custom SKUs.
- **Current Workaround:** 2 warehouse coordinators spending 2 hours each morning hand-typing shipping manifests and inventory levels.
- **Cost of Inaction:** 4 hours/day = 20 hours/week = ~$3,200/month in wasted labor, plus fulfillment errors during the high-volume November/December holiday surge.

### 4. SPICED Call Battleplan
- **30-Second Opening Hook:** "Marcus, saw your note on the NetSuite and Shopify sync choking on dropship manifests. Looked at your catalog growth recently, makes complete sense why manual order re-entry is breaking down. Let's get straight into how you're running that pipeline today."
- **Probe 1 (Situation):** "When the Shopify dropship feed errors out during the morning sync, what is the exact manual path the order takes to reach NetSuite today?"
- **Probe 2 (Pain & Impact):** "You mentioned 4 hours a day lost to manual reconciliation. When an order drops or gets misrouted during peak days, what does that cost you in canceled shipments or customer support tickets?"
- **Probe 3 (Critical Event):** "You flagged November 1st for the Q4 inventory surge. If this sync isn't 100% automated by the last week of October, how does that impact fulfillment capacity?"

### 5. Landmines & Objection Preempts
- **Landmine:** Do not suggest replacing NetSuite or switching ERPs. NetSuite migrations take 9 months; he only wants the integration pipe fixed.
- **Anticipated Objection:** "We are hiring two new logistics coordinators next month anyway, so they can handle the manual data entry."
- **Preemptive Script:** "Hiring coordinators to manually copy-paste data between Shopify and NetSuite is paying \$90k a year in salary for what a \$20/month automated webhook does instantly with zero typos. Wouldn't you rather have those coordinators managing warehouse throughput instead of typing tracking numbers?"

---

### Example 2: B2B Marketing Agency / Professional Services (Growth Sales Call)

#### Input Context
```json
{
  "prospect_name": "Elena Rostova",
  "title": "Head of Operations",
  "linkedin_url": "https://linkedin.com/in/elena-rostova-example",
  "company": "Apex Media Partners",
  "website": "apexmediapartners.com",
  "headcount": 18,
  "inbound_notes": "We run paid media for 25 healthcare clinics. Client reporting takes our account managers 3 full days at the end of every month because data is scattered between Google Ads, Meta Ads, and CallRail. Looking to streamline.",
  "detected_tools": ["Google Ads", "Meta Ads", "CallRail", "Asana", "Google Sheets"],
  "open_roles": ["Senior Account Manager", "Media Buyer"]
}
```

#### Generated Intelligence Brief

# PRE-CALL BRIEF: Elena Rostova | Apex Media Partners
**Call Time:** Tomorrow at 11:00 AM EST | **Lead Source:** Inbound Form

### 1. Executive Snapshot
- **Prospect:** Elena Rostova (Head of Operations) | **Authority:** Medium-High (Recommends tool/vendor to Managing Partner) | **Tenure:** 2 years
- **Company:** Apex Media Partners ([apexmediapartners.com](https://apexmediapartners.com)) | **Model:** Performance Marketing Agency (Healthcare niche)
- **Scale:** 18 employees | Est. Revenue: $2.5M - $4M ARR (25 retainer clients)
- **Detected Stack:** Google Ads, Meta Ads, CallRail, Asana, Google Sheets

### 2. Live Commercial Signals
- **Hiring Signals:** Hiring Senior Account Manager and Media Buyer. Account team is running at capacity; client reporting overhead is choking account manager bandwidth.
- **Growth / Company Triggers:** Recently added 6 healthcare clinic clients over the past two quarters without increasing operational headcount.
- **Inbound Context:** "Client reporting takes our account managers 3 full days at the end of every month because data is scattered between Google Ads, Meta Ads, and CallRail."

### 3. Commercial Hypothesis & Cost of Inaction
- **Core Hypothesis:** Account managers are pulling CSV exports manually from three platforms into Google Sheets, formatting charts, and pasting screenshots into Google Slides for monthly client reviews.
- **Current Workaround:** 4 Account Managers spend 24 working hours each at month-end building slide decks instead of optimizing ad spend.
- **Cost of Inaction:** 96 billable hours lost every month (~$9,600/mo agency cost), plus delayed reporting increases client churn risk.

### 4. SPICED Call Battleplan
- **30-Second Opening Hook:** "Elena, read through your note on the month-end reporting squeeze. Pulling Google Ads, Meta, and CallRail numbers across 25 healthcare clinics by hand is brutal on account managers. Let's dig into the exact data fields you guys report on."
- **Probe 1 (Situation):** "When your account managers compile the monthly clinic reports, where does the manual formatting take the most time? Is it attributing CallRail leads back to specific ad campaigns?"
- **Probe 2 (Pain & Impact):** "When account managers are locked in reporting for 3 days at month-end, what falls through the cracks on active campaign optimization or client response times?"
- **Probe 3 (Critical Event):** "With 2 new team members being hired right now, what is your target date to have automated dashboards live so you don't have to train new hires on the manual spreadsheet process?"

### 5. Landmines & Objection Preempts
- **Landmine:** Do not pitch complex enterprise BI tools like Tableau or PowerBI; agencies hate heavy maintenance overhead. Emphasize self-updating Google Looker Studio or simple automated client dashboards.
- **Anticipated Objection:** "We could probably just use a tool like Supermetrics or Looker Studio ourselves."
- **Preemptive Script:** "Supermetrics gives you raw connectors, but your team still has to spend 40 hours building the healthcare attribution schemas, filtering spam calls in CallRail, and debugging broken API keys. We deliver the complete, finished client-facing dashboard and maintain the pipelines for you."
