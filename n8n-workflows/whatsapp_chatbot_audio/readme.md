# Saathi WhatsApp Multi-Modal Assistant

A WhatsApp-native conversational agent built for **SBI Saathi** that understands and responds to customers in **text, voice, image, and document** formats — and replies back in whichever mode fits best (text or audio).

## Why This Matters for Saathi

Rural and semi-urban banking customers often face literacy or comfort barriers with text-heavy banking apps. This automation lets a customer simply **send a voice note, a photo, or a PDF on WhatsApp** — no app download, no typing required — and get a clear, spoken or written response back. It's the input layer that makes Saathi's "Your Bank Knows, Saathi Acts" vernacular-first philosophy actually usable in the real world.

## What It Does

The workflow listens on a WhatsApp trigger and automatically detects what kind of message the customer sent, then routes it down the right processing path:

| Input Type | Processing Steps |
|---|---|
| **Text** | Passed directly to the AI Agent |
| **Voice** | Get audio URL → Download audio → Transcribe (via OpenAI Whisper) → Pass transcript to AI Agent |
| **Image** | Get image URL → Download image → Analyze image (via OpenAI vision) → Pass analysis to AI Agent |
| **Document** | Validates it's a PDF → Get file URL → Download file → Extract text from PDF → Pass extracted content to AI Agent |
| **Unsupported format** | Sends a fallback "not supported" message back to the user |

Once the AI Agent processes the request (using an OpenAI chat model with conversational memory to maintain context across the chat), the workflow decides how to reply:

- **If the incoming message was audio** → generates an audio response (OpenAI), fixes the audio MIME type for WhatsApp compatibility, and sends it back as a voice note
- **Otherwise** → sends a standard text message back

## Architecture

```
WhatsApp Trigger
      │
      ▼
Input Type Router (Text / Voice / Image / Document / Unsupported)
      │
      ├─ Text ──────────────┐
      ├─ Voice → Transcribe ─┤
      ├─ Image → Analyze ────┼──▶ AI Agent (OpenAI Chat Model + Memory)
      ├─ Document → Extract ─┘         │
      └─ Unsupported → Fallback msg    ▼
                              From audio to audio? (router)
                                   │              │
                              Yes ▼          No ▼
                        Generate Audio      Send Text
                        Response → Fix       Message
                        MIME → Send Audio
```

## Tech Stack

| Component | Service Used | Purpose |
|---|---|---|
| Automation Engine | n8n | Orchestrates the full multi-modal conversation flow |
| Messaging Channel | WhatsApp Business API | Receives and sends customer messages (text/voice/image/document) |
| Transcription | OpenAI (Whisper) | Converts voice notes to text |
| Image Understanding | OpenAI (Vision) | Analyzes and describes submitted images |
| Document Parsing | PDF Extraction (n8n native) | Extracts readable text from PDF documents |
| Conversational AI | OpenAI Chat Model | Powers the AI Agent's responses |
| Memory | n8n Simple Memory | Maintains conversation context across turns |
| Voice Response | OpenAI (Audio Generation) | Converts text responses back into spoken audio when needed |

## Workflow Screenshots

*(Add your n8n canvas screenshots here — input-type router view and the response/audio-generation view)*

`![Input Routing](path/to/input-routing-screenshot.png)`

`![Response Generation](path/to/response-generation-screenshot.png)`

## Status

Working prototype — tested across text, voice, image, and document inputs on WhatsApp.

## Setup

This workflow requires:
- A WhatsApp Business API connection (via n8n's WhatsApp node)
- OpenAI API key (chat, transcription, vision, and audio generation)

**⚠️ Do not commit actual API keys or WhatsApp credentials to this repository.** Configure them as n8n credentials within your own instance, not hardcoded in the workflow JSON before export/sharing.

## How to Use

1. Import the `.json` workflow file into your n8n instance
2. Connect your WhatsApp Business API and OpenAI credentials under each node
3. Activate the workflow
4. Message the connected WhatsApp number with text, a voice note, an image, or a PDF to trigger a response

## Notes

This is the multi-modal input/output layer for the broader Saathi agentic system, enabling customers to interact naturally regardless of literacy level or preferred communication format.
