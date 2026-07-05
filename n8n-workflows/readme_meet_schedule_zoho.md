# Saathi Lead Scheduling & Follow-Up Automation (Proof of Concept)

An automated workflow that turns a new lead into a fully scheduled, personalized meeting — with zero manual coordination. Built and tested against **Zoho CRM** as a stand-in integration, this demonstrates the exact automation pattern Saathi would run against **SBI's own CRM and calendar systems** once authenticated for production use.

> **Note on Zoho:** Zoho CRM is used here purely as a proof-of-concept environment to validate the workflow logic end-to-end (webhook triggers, calendar conflict resolution, AI-personalized outreach, and CRM logging). In an SBI deployment, this same workflow would authenticate directly against SBI's internal CRM and scheduling systems, with Zoho replaced by SBI's equivalent lead/customer data source.

## Why This Matters for Saathi

A large part of financial inclusion isn't just answering customer questions — it's making sure interested leads (e.g., a customer flagged for a loan product, insurance follow-up, or dormant account reactivation) actually get a scheduled human or advisor touchpoint without falling through the cracks. This automation shows how Saathi can move a lead from "flagged as interested" to "meeting booked and confirmed" with no manual back-and-forth.

## What It Does

1. **Lead Capture & Retrieval** — Triggers automatically when a new lead is created in the CRM (via webhook). Pulls full lead details for processing.
2. **Workflow Configuration** — Applies configurable scheduling rules: meeting duration, buffer time, working hours, and how many days ahead to search for slots.
3. **Sales Rep & Availability Processing** — Identifies the assigned rep, detects the lead's timezone, checks the rep's real calendar availability, and generates conflict-free time slots based on the configured rules.
4. **Slot Check (Branch):**
   - **If slots are available** → proceeds to meeting creation
   - **If no slots are available** → sends a polite fallback "no availability" email, prompting the lead to share preferred times instead
5. **Zoom Meeting Creation** — Authenticates with Zoom, creates a scheduled meeting at the earliest available slot, and generates the join link.
6. **AI Email & CRM Logging** — Generates a personalized meeting invite email (via AI) with the top time-slot options and meeting link, sends it via Gmail, and logs the scheduled meeting back into the CRM for tracking.

## Architecture

```
CRM Webhook (New Lead)
        │
        ▼
Workflow Configuration (duration, buffers, hours, search window)
        │
        ▼
Sales Rep & Availability Processing
 (Get rep details → Detect timezone → Check calendar → Find open slots)
        │
        ▼
   Slots Available? ──── No ──▶ Send "No Availability" Fallback Email
        │
       Yes
        │
        ▼
Create Zoom Meeting (auth → generate link)
        │
        ▼
AI-Generated Personalized Invite ──▶ Send via Gmail ──▶ Log Meeting in CRM
```

## Tech Stack

| Component | Service Used (PoC) | Purpose | SBI Production Equivalent |
|---|---|---|---|
| Automation Engine | n8n | Orchestrates the full lead-to-meeting flow | Same |
| CRM / Lead Source | Zoho CRM | Triggers on new lead, stores meeting outcome | SBI's internal CRM/lead system |
| Calendar | Google Calendar | Checks rep availability, finds open slots | SBI advisor/rep calendar system |
| Meeting Platform | Zoom | Creates and hosts the scheduled meeting | SBI's approved meeting platform |
| Email | Gmail | Sends personalized meeting invites and fallback messages | SBI's official email/communication channel |
| AI Personalization | Google Gemini | Generates the personalized invite copy and time-slot options | Same or SBI-approved LLM provider |

## Workflow Screenshots

<img width="1387" height="496" alt="Screenshot 2026-07-05 020042" src="https://github.com/user-attachments/assets/566c95c8-66d4-4102-b634-f0be2bfbf7f4" />


## Status

Working proof-of-concept — validated end-to-end against Zoho CRM, Google Calendar, Zoom, Gmail, and Gemini.

## Setup Steps

1. **Connect Integrations** — Authenticate Zoho CRM, Google Calendar, Gmail, Zoom, and Gemini within n8n.
2. **Import Workflow** — In n8n, go to Workflows → Import, and paste the provided workflow JSON.
3. **Update Configuration** — In the Workflow Configuration node, set meeting duration, buffers, working hours, and days to search.
4. **Adjust Timezone Logic** — In the Detect Lead Timezone code node, edit or expand the country/state → timezone mappings as needed.
5. **Verify Calendar Access** — Ensure each sales rep has a correct calendar ID and that events are fetchable.
6. **Add CRM Webhook** — Configure the CRM to fire a webhook to this workflow whenever a new lead is created.

**⚠️ Do not commit actual API keys, OAuth tokens, or CRM credentials to this repository.** Configure all integrations as n8n credentials within your own instance.

## Notes

This automation is a proof-of-concept demonstrating Saathi's ability to close the loop between AI-driven engagement and real-world scheduling. Zoho, Zoom, and Gmail are placeholder integrations used to validate the logic; the production version would connect directly into SBI's CRM, calendar, and communication infrastructure.
