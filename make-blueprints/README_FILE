# Automated B-Roll Sourcing Pipeline

An automated workflow that takes a raw, unedited video and outputs a ready-to-use B-roll shot list — no manual transcript scrubbing or stock-site searching required.

## What It Does

Editors normally spend hours transcribing footage, picking out visual cues, and manually searching stock sites for matching B-roll. This automation does all of that in one pass:

1. **Input** — Upload a raw voiceover/video file
2. **Compression** — Compresses the raw video to speed up processing
3. **Transcription** — Automatically transcribes the full voiceover
4. **Keyword Extraction** — Identifies the best contextual keywords/phrases from the transcript
5. **Stock Footage Fetch** — Searches multiple stock footage sites and pulls matching clips based on those keywords
6. **Output** — Generates a sheet with direct links to each clip, mapped to the exact timeline timestamp where it should be used

## Tech Stack & Integrations

| Component | Service Used | Purpose |
|---|---|---|
| Automation Engine | Make.com | Orchestrates the full workflow end-to-end |
| Transcription | OpenAI (ChatGPT/Whisper API) | Converts raw VO audio into text transcript |
| Keyword Extraction | OpenAI (ChatGPT API) | Analyzes transcript to identify best contextual keywords |
| Video Compression | *[your compressor tool name]* | Compresses raw video before processing to reduce upload/processing time |
| Stock Footage Sourcing | *[stock site names, e.g. Pexels/Pixabay/Storyblocks APIs]* | Fetches matching B-roll clips based on extracted keywords |
| Authentication | Google OAuth 2.0 | Authenticates and authorizes Google Drive access for file upload/approval flow |
| Output Storage | Google Sheets / Google Drive | Stores the final shot list with clip links and timeline mapping |

## Status

Working prototype — tested successfully on sample footage.

## Input / Output Example

*(Add screenshots here — one showing the raw input video/transcript, one showing the final output sheet with clip links and timestamps)*

`![Input Example](path/to/input-screenshot.png)`

`![Output Example](path/to/output-screenshot.png)`

## Setup

This project requires API keys/credentials for:
- OpenAI (transcription + keyword extraction)
- Your chosen stock footage provider(s)
- Google OAuth (Drive access)

**⚠️ Do not commit actual API keys or credentials to this repository.** Store them in a `.env` file excluded via `.gitignore`, or as connection credentials within the Make.com scenario itself.

## How to Use

1. Import the Make.com scenario blueprint into your own Make.com account
2. Connect your OpenAI, stock footage provider, and Google accounts under the scenario's connections
3. Upload a raw video/VO file to trigger the workflow
4. Retrieve the generated shot-list sheet from the connected Google Drive/Sheets output

## Notes

Built as a side automation project to speed up video editing workflows by removing manual B-roll research.
