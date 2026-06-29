# 🤖 AI-Powered Telegram Task Management Bot

A Telegram bot that lets users create ClickUp tasks using plain text or voice messages. It understands the request, extracts structured task data using an LLM, validates it, creates the task in ClickUp, and replies with a confirmation link.

## 🎯 Problem it solves

Manually opening a task manager to log a quick task breaks flow. This bot lets you just type or speak a task naturally in Telegram, and it handles the rest — extraction, validation, creation, and confirmation.

## 🖼️ Demo

Architecture Diagram

<img width="802" height="687" alt="Screenshot 2026-06-29 155556" src="https://github.com/user-attachments/assets/65193923-c5f2-449b-a48f-41daa44e6774" />


Workflow Image
<img width="1865" height="870" alt="image" src="https://github.com/user-attachments/assets/c9e8c367-af49-4a9d-983f-5c004cf06908" />

Clickup-demo
<img width="1511" height="761" alt="image" src="https://github.com/user-attachments/assets/d2fbbc53-3fb4-4816-8cf2-62d0567c6fc4" />

Text-demo
<img width="942" height="442" alt="text-demo" src="https://github.com/user-attachments/assets/0ad77784-5b3d-46b9-9c36-1d042207efcc" />

Image-demo
<img width="996" height="552" alt="image-demo" src="https://github.com/user-attachments/assets/d493738f-f752-4ed0-b60e-5436f3507540" />

video-demo
<img width="960" height="551" alt="video-demo" src="https://github.com/user-attachments/assets/5394dd9d-d992-4bb0-a22f-1a9eb545e18b" />

Voice-demo
<img width="943" height="516" alt="voice-demo" src="https://github.com/user-attachments/assets/6a684711-dc6f-4cf9-8d02-c8d9407f4112" />

## 🏗️ How it works

```
Telegram message (text or voice)
   → Switch node (routes by message type)
   → [Voice] Groq Whisper API (speech-to-text)
   → Google Gemini (extracts task name, description, priority, status, assignee)
   → Validation logic (rejects incomplete requests)
   → Priority mapping (AI priority → ClickUp-compatible value)
   → ClickUp API (creates task)
   → Telegram reply (sends back task URL)
```

## 🛠️ Tech Stack

| Component | Tool |
|---|---|
| Orchestration | n8n (self-hosted via Docker) |
| Messaging | Telegram Bot API |
| NLU / Extraction | Google Gemini |
| Speech-to-Text | Groq Whisper API |
| Task Management | ClickUp API |
| Logic / Parsing | JavaScript (n8n Code nodes) |
| Local Tunneling | ngrok (for webhook testing) |

## ✨ What I built

- Designed and implemented the complete n8n workflow from scratch, self-hosted via Docker.
- Integrated Telegram with n8n using webhooks, tunneled locally through ngrok during development.
- Built separate routing for text, voice, photo, and video messages using Switch nodes.
- Integrated Gemini to extract structured task fields (name, description, priority, status, assignee) from natural language.
- Implemented validation logic that returns an "Invalid Task Message" response for incomplete requests instead of creating bad tasks.
- Integrated Groq Whisper to transcribe voice messages, reusing the same downstream AI pipeline as text input.
- Connected the workflow to ClickUp's API to create tasks and return the generated task URL.
- Added a priority-mapping layer converting AI-generated priorities into ClickUp-compatible values.
- Implemented graceful error handling for unsupported media (photos/videos).

## 📈 Outcome

- Built an end-to-end automation that handles both text and voice task requests through one shared AI pipeline.
- Eliminated manual task entry by converting natural language directly into structured ClickUp tasks.
- Delivered immediate feedback to users via the returned ClickUp task URL.
- Integrated 5 systems (Telegram, n8n, Gemini, Groq, ClickUp) into a single automated workflow.

## 🚀 How to run this

1. Import `workflow.json` into your self-hosted n8n instance.
2. Set up credentials for: Telegram Bot API, Google Gemini, Groq Whisper, ClickUp API.
3. Expose your local n8n webhook using ngrok (or deploy n8n to a public server) and register the webhook URL with your Telegram bot.
4. Activate the workflow and message your bot.

## 🧠 What I learned

Hands-on experience building multi-modal input handling (text + voice), LLM-based structured data extraction, validation logic design, and chaining 5 different APIs into one reliable automated pipeline.

## 🔮 Possible improvements

- Add support for task editing/updates via follow-up messages
- Add multi-user support with per-user ClickUp workspace mapping
- Add a confirmation step before task creation for ambiguous requests
