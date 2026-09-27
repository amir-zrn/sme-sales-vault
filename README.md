# 10 SME Sales & Revenue Skills Vault

![SME Sales Vault](assets/sme_sales_vault.png)

> **A suite of 10 modular agent skills I built to lift our SME clients' baseline sales by up to 30% without hiring more reps.**

---

### The Reality of SME Sales

Most small and mid-sized businesses don't have a lead generation problem. They have a response-time and follow-up leak.

Here is what usually happens behind the scenes:
1. An inbound lead submits a quote request.
2. The team takes 4 to 6 hours to open it. By then, the prospect already called a competitor.
3. If they don't buy immediately, the sales rep sends one follow-up email and gives up.
4. When a proposal is finally needed, it takes 3 days to write a 10-page document nobody asked for.

Everyone is trying to solve this by dumping raw prompts into a blank chat box or buying bloated CRM add-ons that nobody on the team logs into.

I packaged the entire workflow into **10 modular agent skills** that run directly inside Claude Code, Antigravity, Cursor, or your own automation backend (n8n, Python, webhooks).

---

### The Economics (Why 95% Profit Margin)

When an SME adds new sales headcount, margins take a beating: recruiter fees, base salaries, software seats, onboarding time.

With modular skills:
- The fixed business overhead (rent, warehouse, existing team) is already paid.
- The automation processes leads in under 60 seconds and runs 48-hour follow-up loops for fractions of a cent per API call.
- Every incremental deal won from saved leads flows straight to gross margin. That is how our clients saw a 30% baseline lift running at 95%+ profit margins.

---

### The 10 Skills

| # | Skill | What it actually does |
|---|---|---|
| **01** | [`inbound-lead-triager`](skills/inbound-lead-triager/SKILL.md) | Scores inbound forms in 60s using a 100-point rubric. Hot leads get routed to calendar/Slack instantly; low-intent inquiries get an async probe. |
| **02** | [`anti-slop-outreach`](skills/anti-slop-outreach/SKILL.md) | Scrapes real company receipts and writes cold emails in human, lowercase prose. Banned-word checks eliminate all AI tells. |
| **03** | [`deal-nudger`](skills/deal-nudger/SKILL.md) | Runs a 4-touch revival cadence (asset drop, friction check, 9-word Dean Jackson email, clean breakup). No "just checking in" fluff. |
| **04** | [`objection-prep`](skills/objection-prep/SKILL.md) | Armors reps with Chris Voss-style tactical empathy for pricing pushback, timeline stalls, and "we'll build it in-house" excuses. |
| **05** | [`rapid-proposal-writer`](skills/rapid-proposal-writer/SKILL.md) | Turns messy discovery call transcripts into a clean, 1-page 3-tier scope in under 3 minutes. Zero corporate filler. |
| **06** | [`dm-sequencer`](skills/dm-sequencer/SKILL.md) | Coordinates LinkedIn connection notes and multi-touch DM conversations that respect rate limits and read like peer-to-peer chats. |
| **07** | [`dead-pipeline-auditor`](skills/dead-pipeline-auditor/SKILL.md) | Scans lost and dormant deals from the last 90–180 days for job-change or renewal triggers to reopen pipeline without ad spend. |
| **08** | [`call-brief-generator`](skills/call-brief-generator/SKILL.md) | Pulls prospect headcount, tech stack, commercial signals, and estimated Cost of Inaction (COI) into a 1-page pre-call cheat sheet. |
| **09** | [`account-expander`](skills/account-expander/SKILL.md) | Watches active accounts for 80%+ capacity or value milestones and flags low-friction expansion offers before clients ask. |
| **10** | [`handraiser-writer`](skills/handraiser-writer/SKILL.md) | Writes organic LinkedIn breakdown posts with single-word comment triggers that pull qualified buyer DMs into your inbox. |

---

### How to Run These Skills

#### Option A: Claude Code / Antigravity / Cursor
Drop any skill folder into your project's `.agents/skills/` directory:
```bash
# Example: load the proposal writer into your agent workspace
cp -r skills/rapid-proposal-writer path/to/your/repo/.agents/skills/
```

#### Option B: Claude Desktop Projects
1. Create a **Claude Project** for your sales pipeline.
2. Upload the relevant `SKILL.md` file into **Project Knowledge**.
3. Paste raw notes, lead data, or discovery transcripts. Claude will strictly follow the skill's workflows and guardrails.

#### Option C: Automated Pipelines (n8n / Make / Python)
Feed your CRM webhooks (HubSpot, Airtable, Typeform) into an LLM node conditioned with the skill's JSON schema for sub-60-second triage and drafting.

---

### Anti-Slop Engineering Rules

Every skill in this repository enforces strict communication rules:
- **No buzzwords**: `delve`, `seamless`, `game-changer`, `harness`, `elevate`, `paradigm`, and `robust` are completely banned.
- **Evidence first**: Numbers, dates, and mechanisms always come before claims.
- **Lowercase peer-to-peer**: Outbound emails and social messages default to clean, human, spoken phrasing.

---

### License

MIT License. Built by [Amir Zareian](https://github.com/amir-zrn). Free to clone, customize, and run in your own business.
