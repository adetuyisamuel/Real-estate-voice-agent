# Real Estate Voice Agent — Vapi.ai + GPT-4o + n8n

An AI-powered voice agent system for real estate brokerages. Handles both inbound and outbound calls — qualifying buyer and seller leads automatically, extracting structured data from every conversation, and routing hot leads to agents instantly via Slack and Airtable.

---

## The Problem

Real estate leads go cold fast. Studies show that leads contacted within 5 minutes are 9x more likely to convert than those contacted after 30 minutes. Most brokerages call back hours later — or miss the lead entirely outside business hours.

---

## The Solution

Two AI voice agents built on Vapi.ai handle every call automatically:

- **Inbound agent** answers every call 24/7, qualifies the lead, and collects all the data a human agent needs before calling back
- **Outbound agent** calls new leads automatically within minutes of them submitting a form or enquiry — no waiting, no manual dialling

After every call, GPT-4o analyses the transcript, extracts 12 qualification fields, scores the lead 1–10, and routes them into Airtable with a full summary. Hot leads trigger an immediate Slack alert and a follow-up call.

---

## What Happens Automatically

```
INBOUND                          OUTBOUND
Lead calls the number            New lead submits form/WhatsApp
        ↓                                ↓
Vapi inbound agent answers       n8n triggers Vapi outbound call
Qualifies with natural           Vapi outbound agent calls lead
conversation (up to 8 mins)      Qualifies and attempts booking
        ↓                                ↓
          Call ends — Vapi sends webhook to n8n
                        ↓
            GPT-4o analyses transcript
            Extracts 12 lead fields
            Scores lead 1-10
                        ↓
              Create record in Airtable
                        ↓
        Score 7+                  Score below 7
           ↓                            ↓
  Slack alert — #qualified-leads  Slack alert — #nurture-leads
  Trigger outbound follow-up call Added to nurture pipeline
```

---

## Results

- Leads contacted in under 2 minutes — 24 hours a day
- 100% of calls transcribed, analysed, and logged automatically
- Agents only call back pre-qualified leads with full context
- Zero leads lost outside business hours
- Full transcript and qualification data attached to every CRM record

---

## Tools and Stack

| Tool | Purpose |
|---|---|
| Vapi.ai | Voice AI platform — inbound and outbound calls |
| GPT-4o (OpenAI) | Transcript analysis and lead data extraction |
| n8n | Workflow automation — webhook processing and routing |
| Airtable | CRM — all leads with full qualification data |
| Slack | Real-time alerts for qualified and nurture leads |

---

## Files in This Repository

| File | Description |
|---|---|
| `real_estate_voice_agent_workflow.json` | n8n workflow — import directly |
| `vapi_assistant_config.md` | Vapi assistant prompts and settings |
| `README.md` | This file |

---

## Lead Qualification Fields Extracted

GPT-4o extracts these fields from every call transcript:

| Field | Description |
|---|---|
| Lead Type | Buyer, seller, investor, or tenant |
| Full Name | Caller's name if mentioned |
| Budget Min / Max | Price range discussed |
| Location | Area or suburb they want |
| Property Type | House, apartment, commercial, land |
| Bedrooms | Number of bedrooms required |
| Timeline | How soon they want to move |
| Pre Approved | Whether buyer has mortgage approval |
| Motivation | Main reason for buying or selling |
| Score | 1–10 intent and readiness score |
| Qualified | True if score is 7 or above |
| Next Step | Book viewing, send listings, follow up, nurture |

---

## Airtable CRM Structure

Create a base called `Real Estate CRM` with a table called `Real Estate Leads`:

| Field | Type |
|---|---|
| Lead Name | Text |
| Phone | Phone |
| Lead Type | Single select |
| GPT Score | Number |
| Qualified | Checkbox |
| Budget Min | Currency |
| Budget Max | Currency |
| Location | Text |
| Property Type | Single select |
| Bedrooms | Number |
| Timeline | Single select |
| Pre Approved | Checkbox |
| Motivation | Long text |
| Next Step | Single select |
| Call ID | Text |
| Call Type | Single select |
| Call Duration (s) | Number |
| Call Date | Date |
| Summary | Long text |
| Transcript | Long text |
| Status | Single select |

Status options: `New`, `Contacted`, `Viewing Booked`, `Offer Made`, `Converted`, `Nurture`, `Dead`

---

## How to Set Up

**Step 1 — Create your Vapi account**
- Sign up at vapi.ai
- Add a phone number in the Vapi dashboard
- Note your API key, phone number ID

**Step 2 — Create the two assistants**
- Follow the prompts and settings in `vapi_assistant_config.md`
- Create Inbound assistant and note the Assistant ID
- Create Outbound assistant and note the Assistant ID

**Step 3 — Set the webhook URL in Vapi**
- Go to Phone Number settings in Vapi
- Set Server URL to your n8n webhook URL:
  `https://YOUR_N8N_INSTANCE/webhook/vapi-call-complete`
- Enable the `end-of-call-report` event

**Step 4 — Import the n8n workflow**
- Open n8n → Workflows → Import from file
- Upload `real_estate_voice_agent_workflow.json`

**Step 5 — Update these values in n8n**
- `YOUR_AIRTABLE_BASE_ID` — in the Airtable node
- `YOUR_VAPI_API_KEY` — in the HTTP Request node
- `YOUR_VAPI_FOLLOWUP_ASSISTANT_ID` — outbound assistant ID
- `YOUR_VAPI_PHONE_NUMBER_ID` — your Vapi phone number ID
- Slack channel names — `#qualified-leads` and `#nurture-leads`

**Step 6 — Test**
- Call your Vapi phone number
- Go through a qualification conversation
- Check Airtable for the new lead record
- Check Slack for the alert

---

## Customisation

- **Connect to any real estate CRM** — replace Airtable nodes with HubSpot, Salesforce, or Follow Up Boss API calls
- **Add SMS follow-up** — trigger a Twilio SMS after every qualified call with listing links
- **Add calendar booking** — integrate Calendly API to book viewings directly during the call
- **Multi-language** — Vapi supports multiple languages — duplicate assistants for Spanish, French, etc.
- **Add call recording links** — Vapi provides recording URLs in the webhook payload — store these in Airtable

---

## About This Project

Built as part of a professional AI automation portfolio. Demonstrates end-to-end voice AI integration — from call handling and natural language conversation through to transcript analysis, lead scoring, CRM population, and multi-channel alerting.

**Tools demonstrated:** Vapi.ai · GPT-4o · n8n · Airtable · Slack · Voice AI · Transcript analysis · Lead scoring · Inbound + outbound automation
