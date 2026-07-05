# Saathi Conversational Scheduling Agent (Proof of Concept)

A voice-and-text conversational AI agent that can check calendars, pull contacts, manage tasks, read emails, and schedule meetings — all through natural conversation. Built and validated on **Telegram** as a proof-of-concept messaging layer, this agent is designed to run on **WhatsApp** in the SBI Saathi deployment, acting as the direct conversational front-end for one of Saathi's core agentic capabilities: closing the loop between customer engagement and a confirmed, scheduled follow-up.

> **Note on Telegram:** Telegram was used here to validate the agent's reasoning, tool-use, and voice-handling logic quickly during development. In the SBI Saathi production system, this same agent runs on **WhatsApp Business API** (matching the rest of the Saathi customer-facing experience), and its calendar/contact/task tools connect to SBI's internal systems rather than the general-purpose tools shown here.

## How It Fits Into the Saathi Agentic System

This is not a standalone chatbot — it's a **tool-using sub-agent** inside Saathi's broader multi-agent architecture. When a customer conversation (handled by Saathi's orchestrator and specialist agents — e.g. Saathi Swagat, Saathi Disha, Saathi Bhavishya) reaches a point where a follow-up, advisor call, or in-branch appointment is needed, this scheduling agent is invoked to:

1. Check real availability (calendar)
2. Confirm who the meeting is with (contacts)
3. Log the action item (tasks)
4. Check relevant email threads if needed (inbox)
5. Book the meeting and **send the confirmed meeting details straight back to the customer on WhatsApp** — date, time, and any relevant link or next step — as a natural continuation of the same conversation the customer was already having with Saathi.

This means a customer never has to leave the WhatsApp chat, call a number, or wait for a callback — the same conversational thread that identified their need also books and confirms their appointment.

## What It Does

1. **Listen for Incoming Messages** — Triggers on new incoming messages (WhatsApp in production; Telegram in this PoC)
2. **Allow-List Check** — Verifies the sender is an authorized/known user before proceeding
3. **Input Type Detection** — Determines whether the message is voice or text
4. **Voice Handling** — If voice: fetches the voice file and transcribes it to text (speech-to-text)
5. **AI Agent Reasoning** — The transcribed or typed message is passed to the core AI Agent, which uses an OpenAI chat model with conversational memory to understand intent and decide what action is needed
6. **Tool Use** — Based on intent, the agent calls the relevant tool:
   - **Tasks** — retrieves or logs action items
   - **Contacts** — looks up who the meeting/follow-up is with
   - **Calendar** — checks availability and books the meeting
   - **Email** — retrieves relevant message context if needed
7. **Response Delivery** — Sends the final response — including confirmed meeting details when a booking is made — back to the customer on the same channel

## Architecture

```
WhatsApp Message (Text or Voice)
            │
            ▼
     Allow-List Check
            │
            ▼
     Voice or Text?
        │        │
     Voice      Text
        │          │
  Transcribe       │
  (Speech-to-Text) │
        └────┬─────┘
             ▼
      Saathi Scheduling Agent
   (OpenAI Chat Model + Memory)
             │
   ┌─────┬───┴───┬─────────┐
   ▼     ▼       ▼         ▼
 Tasks Contacts Calendar  Email
   │     │       │         │
   └─────┴───┬───┴─────────┘
             ▼
   Confirmed Meeting Details
   sent back to customer on WhatsApp
```

## Tech Stack

| Component | Service Used (PoC) | Purpose | SBI Saathi Production Equivalent |
|---|---|---|---|
| Automation Engine | n8n | Orchestrates the full agent flow | Same |
| Messaging Channel | Telegram | Receives/sends messages during testing | WhatsApp Business API |
| Speech-to-Text | OpenAI (Whisper) | Transcribes incoming voice messages | Same, or Sarvam AI for vernacular voice support |
| Conversational AI | OpenAI Chat Model | Powers the agent's reasoning and tool selection | Same or SBI-approved LLM |
| Memory | n8n Simple Memory | Maintains context across the conversation | Same, backed by persistent storage (e.g. Postgres/pgvector) |
| Tasks | Task management tool | Logs and retrieves action items | SBI internal task/CRM system |
| Contacts | Contacts tool | Identifies who a meeting/follow-up is with | SBI customer/advisor directory |
| Calendar | Google Calendar | Checks availability, books meetings | SBI advisor scheduling system |
| Email | Gmail | Retrieves relevant email context | SBI's official communication system |

## Workflow Screenshot
<img width="1722" height="473" alt="Screenshot 2026-07-05 020809" src="https://github.com/user-attachments/assets/2052c7f4-3875-4496-9e47-cf9b541c053a" />





## Status

Working proof-of-concept — validated end-to-end on Telegram with voice and text input, calendar booking, and tool-based reasoning.

## Setup Steps

1. **Connect Integrations** — Authenticate the messaging channel (Telegram in PoC / WhatsApp in production), OpenAI, Calendar, Contacts, Tasks, and Email within n8n.
2. **Import Workflow** — In n8n, go to Workflows → Import, and paste the provided workflow JSON.
3. **Configure Allow-List** — Set the list of authorized users permitted to interact with the agent.
4. **Verify Tool Access** — Confirm Calendar, Contacts, Tasks, and Email tools are correctly scoped and authenticated.
5. **Test Voice & Text Paths** — Send both a voice note and a text message to confirm both paths route correctly to the agent.

**⚠️ Do not commit actual API keys, bot tokens, or OAuth credentials to this repository.** Configure all integrations as n8n credentials within your own instance.

## Notes

This agent represents the scheduling/follow-up arm of Saathi's agentic system — the point where a conversation naturally converts into a confirmed, real-world next step, with the outcome communicated back to the customer in the same WhatsApp thread they started in.
