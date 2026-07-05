# Saathi Conversational Scheduling Agent (Proof of Concept)

A voice-and-text conversational AI agent ("Angie") that can check calendars, pull contacts, manage tasks, read emails, and schedule meetings — all through natural conversation. Built and validated on **Telegram** as a proof-of-concept messaging layer, this agent is designed to run on **WhatsApp** in the SBI Saathi deployment, acting as the direct conversational front-end for one of Saathi's core agentic capabilities: closing the loop between customer engagement and a confirmed, scheduled follow-up.

> **Note on Telegram:** Telegram was used here to validate the agent's reasoning, tool-use, and voice-handling logic quickly during development. In the SBI Saathi production system, this same agent runs on **WhatsApp Business API** (matching the rest of the Saathi customer-facing experience), and its task/contact/calendar tools connect to SBI's internal systems rather than the general-purpose tools shown here.

## How It Fits Into the Saathi Agentic System

This is a **tool-using sub-agent** inside Saathi's broader multi-agent architecture, not a standalone chatbot. When a customer conversation (handled by Saathi's orchestrator and specialist agents — Saathi Swagat, Saathi Disha, Saathi Bhavishya) reaches a point where a follow-up, advisor call, or in-branch appointment is needed, this scheduling agent is invoked to check availability, confirm details, log the action, and **send the confirmed meeting details straight back to the customer on WhatsApp** — as a natural continuation of the same conversation.

## What It Does

1. **Listen for Incoming Messages** — Telegram trigger listens for new incoming messages (WhatsApp trigger in production)
2. **Allow-List Check** — Verifies the sender's chat ID against an authorized allow-list before proceeding; unauthorized senders are blocked at this step
3. **Voice or Text Detection** — Checks whether the incoming message has text content; if empty, it's treated as a voice message
4. **Voice Handling** — If voice: fetches the voice file from Telegram, then transcribes it to text using OpenAI's speech-to-text
5. **AI Agent Reasoning ("Angie")** — The transcribed or typed message is passed to the core AI Agent, which uses an OpenAI chat model with a rolling conversation memory (last 10 messages) to understand intent and decide what action is needed. The agent's instructions include specific rules: filter out promotional emails when summarizing inbox messages, always include sender/date/subject/summary when reporting on emails, default to "today" if no date is specified, and only surface calendar events relevant to the actual question asked (not unrelated future events)
6. **Tool Use** — Based on intent, the agent calls the relevant tool:
   - **Tasks** (Baserow) — retrieves or logs action items from a tasks database
   - **Contacts** (Baserow) — looks up contact information (emails, phone numbers) for the meeting/follow-up
   - **Google Calendar** — fetches relevant calendar events within a specified date range to check availability
   - **Gmail** — retrieves recent messages when the agent needs email context to answer a question
7. **Response Delivery** — Sends the agent's final response — including confirmed meeting details or summarized information — back to the customer on the same channel (Telegram in PoC, WhatsApp in production)

## Architecture

```
WhatsApp Message (Text or Voice)
            │
            ▼
     Allow-List Check (authorized senders only)
            │
            ▼
     Message Has Text?
        │        │
       No        Yes
        │          │
  Get Voice File   │
        │          │
  Transcribe        │
  (OpenAI STT)      │
        └────┬─────┘
             ▼
      Angie — Saathi Scheduling Agent
   (OpenAI Chat Model + 10-message memory)
             │
   ┌─────┬───┴───┬─────────┐
   ▼     ▼       ▼         ▼
 Tasks Contacts Calendar  Gmail
(Baserow)(Baserow)(Google) (inbox)
   │     │       │         │
   └─────┴───┬───┴─────────┘
             ▼
   Response / Confirmed Meeting Details
   sent back to customer on WhatsApp
```

## Tech Stack

| Component | Service Used (PoC) | Purpose | SBI Saathi Production Equivalent |
|---|---|---|---|
| Automation Engine | n8n | Orchestrates the full agent flow | Same |
| Messaging Channel | Telegram | Receives/sends messages during testing | WhatsApp Business API |
| Speech-to-Text | OpenAI | Transcribes incoming voice messages | Same, or Sarvam AI for vernacular voice support |
| Conversational AI | OpenAI Chat Model | Powers the agent's reasoning and tool selection | Same or SBI-approved LLM |
| Memory | n8n Buffer Window Memory (last 10 messages) | Maintains recent conversation context | Same, backed by persistent storage (e.g. Postgres/pgvector) for longer-term memory |
| Tasks & Contacts Database | Baserow | Stores and retrieves task and contact records | SBI internal CRM/task system |
| Calendar | Google Calendar | Checks availability, retrieves relevant events | SBI advisor scheduling system |
| Email | Gmail | Retrieves relevant email context (filters out promotional mail) | SBI's official communication system |

## Workflow Screenshot

*(Add your n8n canvas screenshot here)*

`![Scheduling Agent Workflow](path/to/scheduling-agent-screenshot.png)`

## Status

Working proof-of-concept — validated end-to-end on Telegram with voice and text input, allow-list authentication, calendar/task/contact lookups, and Gmail-based context retrieval.

## Setup Steps

1. **Connect Integrations** — Authenticate the messaging channel (Telegram in PoC / WhatsApp in production), OpenAI, Baserow (Tasks & Contacts tables), Google Calendar, and Gmail within n8n.
2. **Import Workflow** — In n8n, go to Workflows → Import, and paste the provided workflow JSON.
3. **Configure Allow-List** — Set the authorized chat ID(s)/sender(s) permitted to interact with the agent.
4. **Set Up Baserow Tables** — Create the Tasks and Contacts tables in Baserow (or point to existing ones) and update the database/table IDs in the workflow.
5. **Verify Tool Access** — Confirm Google Calendar and Gmail credentials are correctly scoped and authenticated.
6. **Test Voice & Text Paths** — Send both a voice note and a text message to confirm both paths route correctly to the agent.

**⚠️ Do not commit actual API keys, bot tokens, Baserow database IDs tied to production data, or OAuth credentials to this repository.** Configure all integrations as n8n credentials within your own instance.

## Notes

This agent represents the scheduling/follow-up arm of Saathi's agentic system — the point where a conversation naturally converts into a confirmed, real-world next step, with the outcome communicated back to the customer in the same WhatsApp thread they started in. Baserow is used here as a lightweight PoC database for tasks and contacts; in production this would be replaced by SBI's internal CRM and directory systems.
