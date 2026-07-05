# SBI Saathi — Visual Documentation

This folder contains the visual assets supporting the SBI Saathi submission — the layered agentic architecture, customer journey flow, dashboard mockups, and UI screens referenced in the pitch deck and idea submission.

## Architecture Overview

**Saathi is not a single chatbot.** It is a layered agentic system where every layer has a specific job, a specific tool, and a specific output — and no layer oversteps into another's responsibility.


## The 7-Step Agentic Flow

### 01 — Customer Intelligence
The Agentic AI securely processes SBI's internal customer signals, banking activity, consent records, and eligibility criteria to proactively identify customers who can benefit from personalized banking services, government schemes, and financial products — before the customer even realizes the opportunity.
<img width="1536" height="1024" alt="ChatGPT Image Jul 1, 2026, 05_17_39 PM (1)" src="https://github.com/user-attachments/assets/63bb3b55-85ab-4b07-9c58-fd31139485dd" />



### 02 — AI Outreach Engine
Selects the best communication channel for the customer and personalizes the outreach message accordingly.
<img width="1536" height="1024" alt="ChatGPT Image Jul 1, 2026, 02_37_38 PM (1) (1)" src="https://github.com/user-attachments/assets/c28d20f8-f0c2-4f26-9b19-1ccefbb1b57b" />

### 03 — Conversation Agent
Guides the customer through a natural conversation, collects consent, and requests any required documents.
<img width="1536" height="1024" alt="ChatGPT Image Jul 3, 2026, 02_11_12 PM" src="https://github.com/user-attachments/assets/83b7cf35-f29b-4ca3-992b-c881e33f791f" />




### 04 — Document Intelligence
Verifies submitted documents, detects missing information, and recommends additional benefits the customer may be eligible for.
<img width="1536" height="1024" alt="ChatGPT Image Jul 1, 2026, 05_35_34 PM (1)" src="https://github.com/user-attachments/assets/948075dd-689a-43d4-8b36-3e7984b78d1b" />



### 05 — Decision Engine
Determines the optimal service path — self-service resolution, appointment booking, or handoff for remote processing.
<img width="1536" height="1024" alt="ChatGPT Image Jul 3, 2026, 02_27_54 PM (1)" src="https://github.com/user-attachments/assets/f0178f48-9e86-49fd-b0f2-eabacbe0d138" />




### 06 — Banking Mitra Orchestration
Assigns the verified request, along with complete customer context, to a Banking Correspondent (Banking Mitra) for seamless execution.
<img width="1536" height="1024" alt="ChatGPT Image Jul 3, 2026, 02_34_05 PM (1)" src="https://github.com/user-attachments/assets/7bbf3f5e-2c0a-423a-a220-5cba27e213b6" />



### 07 — Service Completion & Feedback
Activates the requested service, captures customer feedback, and feeds it back into the system to continuously improve future AI decisions.




## Additional Visual Assets

### whole worflow ui 
<img width="1536" height="1024" alt="ChatGPT Image Jul 3, 2026, 01_24_06 PM (1)" src="https://github.com/user-attachments/assets/64800d16-a4a4-45ce-b48c-f31d2783f19b" />



### Banking Mitra / Advisor Dashboard
Mockup of the internal dashboard used by Banking Mitras/advisors to view assigned requests, customer context, and completion status.
<img width="1536" height="1024" alt="ChatGPT Image Jul 1, 2026, 07_31_21 PM (1)" src="https://github.com/user-attachments/assets/34c0e09f-25c6-4f24-af0e-925c3fde2042" />



### main dashboard
this is how the main dashboard for a main authority would look like to look over the whole banking system automation
<img width="1536" height="1024" alt="ChatGPT Image Jul 1, 2026, 02_57_48 PM (1)" src="https://github.com/user-attachments/assets/4c3714a3-6013-4779-8d60-0394c73e5062" />



### Automation Workflow Diagrams
Visual breakdown of the proof-of-concept automations built to validate this architecture (see `n8n-workflows/` and `make-blueprints/` for the underlying builds):
- B-Roll Sourcing Pipeline
- WhatsApp Multi-Modal Chatbot
- Lead Scheduling Automation
- Telegram/WhatsApp Scheduling Agent
- Phone & Chat Agent (Voiceflow)



## Notes

These visuals represent the idea-stage architecture and proof-of-concept diagrams intended to communicate the system's design. A full production UI/UX and dashboard build would take place during the 30-day prototyping phase if selected.

## How to Use This Folder

- Reference these images alongside the main [README](../README.md) and the pitch deck for full context
- Each image is named descriptively; add new visuals here as the design evolves during prototyping
