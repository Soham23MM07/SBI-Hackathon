# AI Automation Workflows

A collection of production-ready AI automation workflows built on **n8n** and **Make.com**, covering voice/chat agents, meeting scheduling, WhatsApp bots, and video editing.

---

## Workflows

### n8n Workflows (`/n8n-workflows`) -

#### 1. Chatbot — Phone Agent & Voiceflow (`chatbot-phone-agent-voiceflow.json`)
An AI-powered phone agent integrated with Voiceflow. Features:
- RAG (Retrieval-Augmented Generation) using Qdrant vector store + OpenAI embeddings
- Google Calendar integration for appointment booking
- Order tracking via webhook
- Multi-agent routing (RAG agent, calendar agent, tracking agent)
- GPT-4o-mini powered responses

**Stack:** n8n · OpenAI · Qdrant · Google Calendar · Voiceflow

---

#### 2. Scheduling Meet (`scheduling-meet.json`)
A voice-and-text meeting scheduler powered by an AI assistant ("Angie"). Features:
- Handles both voice (speech-to-text) and text messages via Telegram
- Allowlist-based access control
- Google Calendar event creation
- Gmail integration for meeting context
- Simple memory for conversational continuity

**Stack:** n8n · OpenAI Whisper · Google Calendar · Gmail · Telegram

---

#### 3. WhatsApp Chatbot — Audio, Text & PDF (`whatsapp-chatbot-audio-text-pdf.json`)
A fully functional WhatsApp chatbot that handles multiple input types:
- **Text** messages → AI agent responds
- **Audio** messages → transcribed via Whisper, then processed
- **Images** → analyzed with vision model
- **PDFs** → extracted and fed into the AI agent
- Simple memory for context retention

**Stack:** n8n · WhatsApp Business API · OpenAI · Whisper

---

#### 4. Smart Meeting Scheduler — Zoho CRM (`smart-meeting-scheduler-zoho.json`)
Automated lead-to-meeting pipeline integrated with Zoho CRM. Features:
- Triggered by new leads in Zoho CRM
- Detects lead timezone automatically
- Checks sales rep calendar availability with buffer time
- Generates personalized meeting invites via Gemini
- Sends invites and logs meetings back to Zoho CRM
- Fallback email if no slots are available

**Stack:** n8n · Zoho CRM · Google Calendar · Google Gemini · Gmail

---

### Make.com Blueprints (`/make-blueprints`)

#### 5. Video Editing Agent (`video-editing-agent.blueprint.json`)
An AI-driven video editing automation agent built on Make.com.

**Stack:** Make.com

---

## Getting Started

### Import n8n Workflows
1. Open your n8n instance
2. Go to **Workflows → Import from File**
3. Select the `.json` file from `/n8n-workflows`
4. Configure your credentials (OpenAI, Google Calendar, etc.)
5. Activate the workflow

### Import Make.com Blueprints
1. Open Make.com
2. Go to **Scenarios → Create a new scenario**
3. Click the three-dot menu → **Import Blueprint**
4. Upload the `.blueprint.json` file from `/make-blueprints`
5. Configure your connections and activate

---

## Prerequisites

| Service | Used By |
|---|---|
| OpenAI API | All n8n workflows |
| Google Calendar API | Chatbot Phone Agent, Scheduling Meet, Smart Meeting Scheduler |
| Qdrant | Chatbot Phone Agent |
| Telegram Bot | Scheduling Meet |
| WhatsApp Business API | WhatsApp Chatbot |
| Zoho CRM | Smart Meeting Scheduler |
| Google Gemini API | Smart Meeting Scheduler |

---

## Docs

The `/docs` folder contains the project pitch deck (`pitch-deck.pdf`).

---

## License

MIT
