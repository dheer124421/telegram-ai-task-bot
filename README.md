# 🤖 AI-Powered Telegram Task Management Bot

A Telegram bot that lets users create ClickUp tasks using plain text or voice messages. It understands the request, extracts structured task data using an LLM, validates it, creates the task in ClickUp, and replies with a confirmation link.

## 🎯 Problem it solves

Manually opening a task manager to log a quick task breaks flow. This bot lets you just type or speak a task naturally in Telegram, and it handles the rest — extraction, validation, creation, and confirmation.

## 🖼️ Demo

> *(Add a short screen recording: send a voice note → bot replies with the created ClickUp task link)*

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
