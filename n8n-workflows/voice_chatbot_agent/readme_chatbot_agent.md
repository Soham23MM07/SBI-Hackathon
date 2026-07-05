# Saathi Phone & Chat Agent (Voiceflow + n8n)

A conversational AI backend that powers both a **chat widget** and a **phone-call agent** (via Twilio) for SBI Saathi — giving customers a way to check status, book appointments, and get answers to banking questions **by voice call, with no app or smartphone required.**

## Why This Matters for Saathi

A large share of rural and semi-urban customers Saathi is built for don't reliably have smartphones, data, or comfort with apps — but nearly everyone has access to a basic phone call. This automation makes Saathi reachable as an **actual inbound phone number**: a customer calls in, talks naturally, and the same AI backend that would power a WhatsApp chat now handles a live voice conversation — checking application status, booking a branch appointment, or answering a scheme question, entirely hands-free.

This is the voice-access layer that makes "Your Bank Knows, Saathi Acts" work for customers who are digitally excluded, not just digitally hesitant.

## What It Does

The system exposes three core capabilities through n8n webhooks, all orchestrated by a **Voiceflow** conversational front-end that can run as a website chat widget, voice widget, or a live phone line (via a Twilio number connected to Voiceflow):

### 1. Status / Application Tracking
Customer asks about the status of an order, application, or request (e.g. "what's the status of my loan application?"). The agent calls an external tracking API with the reference number and email, and reads the status back to the customer in natural language.

### 2. Appointment Booking
Customer asks to book a branch visit or advisor call. The agent:
- Converts the requested date/time into a valid Google Calendar format
- Creates a calendar event (with a 1-hour default duration) for the requested slot
- Confirms the booking back to the customer in conversation

### 3. Knowledge Q&A (RAG)
Customer asks a general question (e.g. about a scheme, product, or process). The agent:
- Searches a **Qdrant vector database** containing SBI's knowledge base (scheme details, FAQs, eligibility criteria, etc.)
- Uses a retrieval-augmented conversational agent (OpenAI GPT-4o-mini) to generate an accurate, grounded answer based only on the retrieved knowledge
- Responds conversationally, and can ask clarifying questions if the query is ambiguous

### 4. Knowledge Base Ingestion (Setup Pipeline)
A separate one-time/refreshable pipeline that:
- Pulls source documents from a Google Drive folder
- Splits them into chunks (token-based splitting)
- Generates embeddings (OpenAI) and stores them in the Qdrant vector collection
- Can be re-run to refresh the knowledge base whenever documents are updated

## Architecture

```
                 Customer (Phone Call via Twilio, or Chat Widget)
                                  │
                                  ▼
                            Voiceflow Agent
                    (routes intent to correct capability)
                    │             │             │
                    ▼             ▼             ▼
             Status Webhook  Appointment    Knowledge Webhook
                    │          Webhook             │
                    ▼             │                ▼
           External Tracking      ▼         Retrieval Agent
                API          Date Formatting   (GPT-4o-mini)
                    │        (LLM chain)             │
                    ▼             │                  ▼
          Status Response         ▼           Qdrant Vector Search
                    │      Google Calendar      (OpenAI Embeddings)
                    │       Event Created             │
                    ▼             │                   ▼
             Response back        ▼            Grounded Answer
             to customer   Confirmation              │
                            back to customer          ▼
                                              Response back to customer


   ── Knowledge Base Setup (separate pipeline) ──
   Google Drive Folder → Download Documents → Token Splitter
        → OpenAI Embeddings → Qdrant Vector Store (indexed)
```

## Tech Stack

| Component | Service Used | Purpose | SBI Saathi Role |
|---|---|---|---|
| Conversational Front-End | Voiceflow | Manages conversation flow, connects webhooks, powers both chat and voice interfaces | Customer-facing entry point |
| Telephony | Twilio (via Voiceflow) | Assigns a real phone number to the agent, enabling inbound phone calls | Enables no-smartphone-needed access |
| Automation Engine | n8n | Hosts the three core capability webhooks (status, appointment, RAG) | Backend orchestration |
| Conversational AI | OpenAI (GPT-4o-mini) | Powers the retrieval agent's reasoning and the appointment date-formatting logic | Core reasoning layer |
| Vector Database | Qdrant | Stores embedded knowledge base documents for retrieval | Powers Saathi's scheme/FAQ knowledge |
| Embeddings | OpenAI Embeddings | Converts documents and queries into vectors for semantic search | Enables accurate knowledge retrieval |
| Document Source | Google Drive | Source location for knowledge base documents | Where SBI scheme/FAQ docs would live |
| Scheduling | Google Calendar | Creates and manages appointment bookings | Branch visit / advisor call booking |
| Status Lookup | External Tracking API | Retrieves status for a given reference/order/application number | Application/claim status checks |

## Status

Working prototype — validated with all three capabilities (status lookup, appointment booking, RAG-based Q&A) connected through Voiceflow, deployable as both a chat widget and a Twilio phone number.

## Setup Steps

1. **Set up Qdrant** — Create a Qdrant collection to hold the knowledge base vectors
2. **Ingest Knowledge Base** — Run the document ingestion pipeline: connect a Google Drive folder containing your knowledge documents, and run the workflow to chunk, embed, and store them in Qdrant
3. **Configure Webhooks** — Deploy the three n8n webhook workflows (status tracking, appointment booking, RAG Q&A) and note their URLs
4. **Build the Voiceflow Agent** — Create a Voiceflow project with three corresponding "capture" intents, each pointing to its matching n8n webhook URL
5. **Connect Google Calendar** — Authenticate the calendar account that appointment bookings should be created against
6. **Connect a Twilio Number (optional, for phone access)** — Import a Twilio number into Voiceflow to make the agent reachable by phone call, in addition to chat
7. **Test** — Test both chat and voice paths across all three capabilities before going live

**⚠️ Do not commit actual API keys, Qdrant URLs, Twilio credentials, or OAuth tokens to this repository.** Configure all integrations as n8n credentials or environment variables within your own instance.

## Notes

This automation represents Saathi's **voice and phone accessibility layer** — proving that the same agentic backend answering WhatsApp messages can also answer a real phone call, which matters directly for reaching customers without smartphones or data access. The knowledge base (RAG) component is where SBI's actual scheme documentation (PMJJBY, PMSBY, Sukanya Samriddhi Yojana, e-Shram, etc.) would be ingested in a production deployment, replacing the generic example knowledge base used during testing.
