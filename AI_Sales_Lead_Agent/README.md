# AI Sales Lead Qualification & Follow-Up Agent

> **End-to-end agentic AI solution combining n8n workflow automation, Google Gemini LLM, Google Sheets CRM, and Gmail — scoring, routing, and following up on every sales lead automatically.**

[![Workflow](https://img.shields.io/badge/Tool-n8n%20Automation-EA4B71?logo=n8n&logoColor=white)](https://n8n.io/)
[![LLM](https://img.shields.io/badge/LLM-Google%20Gemini-4285F4?logo=google&logoColor=white)](https://aistudio.google.com/)
[![CRM](https://img.shields.io/badge/CRM-Google%20Sheets-34A853?logo=googlesheets&logoColor=white)](https://sheets.google.com/)
[![Email](https://img.shields.io/badge/Email-Gmail%20Automation-D14836?logo=gmail&logoColor=white)](https://gmail.com/)

## Project Overview

This project was developed to build a **fully automated AI sales lead qualification agent** that scores every incoming lead the moment it arrives, routes it by tier, and sends the right follow-up email — instantly, without any manual triage.

The solution brings together a live intake form, an LLM scoring pipeline, a Google Sheets mini-CRM, and three automated Gmail responses into one end-to-end agentic workflow built entirely in n8n.

### Objectives

- Capture every incoming sales enquiry through a structured intake form
- Score each lead 0–100 using Google Gemini LLM with a stated reason
- Classify leads as Hot (70–100), Warm (40–69), or Cold (0–39)
- Log every lead, score, reason, and tier automatically to Google Sheets
- Send personalized follow-up emails instantly based on lead tier
- Ensure zero missed hot leads, day or night, with no manual steps

## Agent Workflow

```text
Contact Us Form (n8n Form Trigger)
        |
        v
Google Gemini LLM
Score 0–100 + Tier Classification (Hot / Warm / Cold)
        |
        v
Structured Output Parser
Extracts score, reason, tier as clean fields
        |
        v
Google Sheets Mini-CRM
Logs every lead automatically
        |
        v
Switch Node (Tier Routing)
   /         |         \
HOT         WARM       COLD
Personalized  Nurture   Thank-You
Email +       Email     Note
Rep Alert
```

## Technology Stack

| Tool | Role |
|---|---|
| **n8n** | Workflow automation — orchestrates every step end-to-end |
| **Form Trigger** | Captures each "Contact Us" enquiry |
| **Google Gemini LLM** | Scores the lead 0–100 and classifies the tier |
| **Structured Output Parser** | Extracts score, reason & tier as clean, structured data |
| **Google Sheets** | Mini-CRM — every lead, score, reason and tier logged |
| **Gmail** | Sends the personalized, nurture or thank-you email |
| **Google Cloud OAuth** | Secures n8n's connection to Sheets & Gmail |

## Input Form Fields

| Field | Type | Description |
|---|---|---|
| Full Name | Text Input | Contact name of the lead |
| Company Name | Text Input | Organization the lead represents |
| Business Need | Text Area | What problem they're trying to solve |
| Budget Range | Dropdown | Under $1,000 / $1,000–$10,000 / $10,000–$50,000 / Over $50,000 / Not sure yet |
| Timeline | Dropdown | Immediate / Within 1 month / 1–3 months / Just exploring |
| Email | Email (required) | Used for the automated follow-up |

## Lead Classification & Routing Rules

| Tier | Score Range | Criteria | Follow-Up Action |
|---|---|---|---|
| 🔴 **Hot** | 70–100 | Confirmed budget, short timeline, clear need | Personalized email sent within seconds + rep alerted |
| 🟡 **Warm** | 40–69 | Real interest but budget or timeline unclear | Nurture email with helpful next steps |
| 🔵 **Cold** | 0–39 | Vague need, no budget or timeline stated | Polite, low-effort thank-you note |

Every score comes with a short, stated reason from the LLM.

## AI Agent Design

### Lead Scoring Agent (Google Gemini)
- Reads the lead's business need, budget and timeline
- Weighs how well the need matches what we offer
- Considers urgency based on the stated timeline
- Produces a 0–100 score with a one-line reason
- Classifies the lead as Hot, Warm or Cold

### Routing & Response Agent (n8n Switch Node)
- Reads the tier assigned by the Scoring Agent
- Decides which of the three response paths to trigger
- Selects the matching email template for that tier
- Hands off to Gmail to send the reply instantly
- Requires no human decision at any point

## LLM Prompt (Google Gemini)

```
A new sales lead submitted this form:
Name: {{ $json['full name'] }}
Company: {{ $json['company name'] }}
Business Need: {{ $json['business need'] }}
Budget Range: {{ $json['budget range'] }}
Timeline: {{ $json['timeline'] }}

Score this lead from 0 to 100 based on how likely they are to become a
paying customer. Give one short sentence explaining why. Then classify them
as Hot (70-100), Warm (40-69), or Cold (0-39).
```

**Structured Output:**
```json
{ "score": 55, "reason": "They have a solid budget but the timeline is unclear", "tier": "Warm" }
```

## n8n Workflow Nodes

| Node | Purpose |
|---|---|
| **On Form Submission** | Triggers the workflow on every form submit |
| **Basic LLM Chain** | Sends the prompt to Google Gemini and receives the scored output |
| **Structured Output Parser** | Extracts score, reason & tier as separate clean fields |
| **Google Sheets — Append Row** | Logs the full lead record to the mini-CRM spreadsheet |
| **Switch** | Routes by Hot / Warm / Cold tier |
| **Gmail — Hot** | Sends instant personalized email to hot leads |
| **Gmail — Warm** | Sends nurture email to warm leads |
| **Gmail — Cold** | Sends polite thank-you to cold leads |

## Google Sheets Mini-CRM

The workflow automatically appends a row to the spreadsheet on every submission:

| Column | Source |
|---|---|
| Timestamp | Form submission time |
| Name | Full Name field |
| Company | Company Name field |
| Need | Business Need field |
| Budget | Budget Range field |
| Timeline | Timeline field |
| Email | Email field |
| Score | LLM output |
| Reason | LLM output |
| Tier | LLM output (Hot / Warm / Cold) |

## Live Results & Proof

**Test Run — Real Example**
- Submitted a live test lead through the actual form
- Google Gemini returned: **Score 95, Tier: Hot**
- Reason: *strong budget, immediate timeline, clear business need*
- Row appended automatically to the Google Sheet CRM
- Hot-tier email sent and received within seconds

**What This Confirms**
- Full pipeline works end to end: Form → AI → Sheet → Email
- Tier routing correctly matches the score to Hot / Warm / Cold
- Zero manual steps needed after the form is submitted
- Verified across multiple test submissions with different inputs

## Key Business Results

- **100%** of leads scored automatically — no manual review needed
- **0–100** scoring scale with a stated reason per lead
- **Zero** missed hot leads — agent runs 24/7
- **Instant** personalized outreach for hot leads
- **Full lead history** kept in one live Google Sheet
- **3 automated email paths** — Hot, Warm, Cold

## Business Value

### Operational Benefits
- Faster follow-up on every single lead
- No hot lead ever waits for a rep to notice it
- Consistent, criteria-based lead scoring
- One centralized, always-current mini-CRM

### Strategic Benefits
- Higher conversion rate on high-intent leads
- Reps spend time where it actually pays off
- Better visibility into the top of the sales pipeline
- A lead-handling process that scales with enquiry volume

## Target Users

| User | Benefit |
|---|---|
| Sales Reps | Spend time only on leads worth chasing |
| Sales Managers | Get a live, centralised view of every lead |
| Marketing Teams | See which campaigns produce the strongest leads |
| Founders & SMB Owners | Never miss a hot lead, even without a sales team |
| Operations Teams | Rely on one consistent, automated intake process |

## Challenges Faced & Resolutions

| Challenge | Resolution |
|---|---|
| Groq's LLM model was deprecated mid-build | Switched to Google Gemini, confirmed via live testing |
| Google Cloud OAuth consent screen setup | Registered a Google Cloud project and added test users |
| Google Sheets/Gmail API access | Enabled the Sheets & Gmail APIs and reconnected OAuth |
| Case-sensitive mismatch broke lead routing | Traced the Switch node and corrected Hot/Warm/Cold casing |
| Moving from local to always-on setup | Migrated the workflow to n8n Cloud for public availability |

## Key Learnings

**Technical**
- Hands-on experience designing an agentic workflow in n8n
- Learned to connect an LLM to a live automation pipeline
- Practical experience with Google Cloud OAuth & API scopes
- Learned to debug a multi-step workflow node by node

**Process**
- Small details like text casing can break automated logic
- Testing early and often catches issues before they compound
- Free-tier tools are enough to build a genuinely working agent
- Clear documentation makes a project easy to explain and repeat

## Future Enhancements

**Near-Term**
- Add a Slack alert for the sales rep on every Hot lead
- Let the AI write a fully custom reply per lead
- Add basic error alerts if any workflow step fails

**Longer-Term**
- Connect to a real CRM (HubSpot, Salesforce) instead of Sheets
- Track reply and conversion rates per tier over time
- Support multiple languages for international leads

## Repository Structure

```text
Internship-AI-Sales-Lead-Agent/
├── AI_Sales_Lead_Agent/
│   ├── Sales_Lead_Qualification_Agent.json   # n8n workflow export (importable)
│   └── steps.txt                              # Step-by-step setup guide
└── README.md
```

## How to Import the Workflow

1. Open your n8n instance (`localhost:5678` or n8n Cloud)
2. Click **"+"** → **"Import from file"**
3. Select `Sales_Lead_Qualification_Agent.json`
4. Add your own credentials:
   - Google Gemini API key (from [aistudio.google.com](https://aistudio.google.com/))
   - Google Sheets OAuth (via Google Cloud Console)
   - Gmail OAuth (same Google Cloud project)
5. Create your **"Lead Qualification CRM"** Google Sheet with columns:
   `Timestamp, Name, Company, Need, Budget, Timeline, Email, Score, Reason, Tier`
6. Click **"Execute Workflow"** and test with a sample form submission

> ⚠️ **Note:** This workflow is case-sensitive. Ensure tier values in the Switch node match exactly: `hot`, `warm`, `cold` (lowercase).

## Conclusion

This project goes from proposal to a fully working, tested system — turning a simple form submission into an AI-scored, automatically routed, and instantly followed-up sales lead, end to end. Built entirely with free-tier tools and deployed on n8n Cloud.

## Author

**Sneha Shree M U** — AI Sales Lead Qualification & Follow-Up Agent — Internship Project, Sep 2026

---

📫 [LinkedIn](https://www.linkedin.com/in/sneha-shree-mu/) | [Portfolio](https://shreesneha056-gif.github.io/portfolio_website/) | [GitHub](https://github.com/shreesneha056-gif)
