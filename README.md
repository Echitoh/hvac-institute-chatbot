# HVAC Institute Chatbot

> AI chatbot that answers prospective student questions for an HVAC training institute — deployed on Telegram for testing. Built with n8n and Gemini AI.

## The Problem

HVAC training institutes get the same questions over and over from prospective students: "When does the next cohort start?", "How much does it cost?", "Is it in-person or online?", "Do you offer financing?", "What's the job placement rate?". Staff answers each one manually, often outside business hours, and many prospects bounce when they don't get a fast reply.

This chatbot handles the first conversation 24/7 — answering common questions instantly, qualifying serious leads, and handing off to a human only when the prospect is ready to enroll or has a question the bot can't answer.

## How It Works

1. **Telegram trigger** — A prospect messages the institute's Telegram bot. The workflow fires on every incoming message.
2. **Context lookup** — The workflow loads the institute's knowledge base (courses, schedules, prices, location, FAQs) as context for the AI.
3. **AI response** — Gemini receives the prospect's message + the context + the conversation history, and generates a focused, on-brand response.
4. **Send reply** — The response is sent back via Telegram, typically within 2-3 seconds.
5. **Lead capture** — When the AI detects buying signals ("I want to sign up", "Can I pay in installments?"), the workflow saves the conversation to a Google Sheet and notifies the admissions team.
6. **Handoff** — If the prospect requests a human or asks something outside the knowledge base, the bot tells them an admissions advisor will reach out and flags the conversation for follow-up.

## Stack

- **n8n** — workflow orchestration and conversation state
- **Gemini AI** — language understanding and response generation
- **Telegram Bot API** — chat interface (currently used for testing; WhatsApp Business is the target for production)
- **Google Sheets** — qualified leads log

## Screenshots

See [`/screenshots`](./screenshots/) — Telegram conversation samples and workflow diagram.

## How to Replicate

1. Create a Telegram bot via [@BotFather](https://t.me/BotFather) and save the bot token.
2. Import [`workflow.json`](./workflow.json) into your n8n instance.
3. Configure n8n credentials:
   - **Telegram** — paste your bot token
   - **Google Gemini (PaLM)** — API key from Google AI Studio
   - **Google Sheets OAuth2** — for the leads log
4. Open the `System Prompt` node and replace the placeholder knowledge base with your institute's actual info: course catalog, dates, pricing, contact info, common objections.
5. Test by messaging your bot from any Telegram client.
6. (Optional) Migrate to WhatsApp Business API for production by swapping the trigger and send nodes — the logic stays the same.

## Why Telegram First

Telegram has zero-friction bot setup (no Meta Business verification, no template approvals), which makes it ideal for iterating on the prompt and the conversation flow. Once the bot's behavior is dialed in, the same n8n workflow ports to WhatsApp Business with minimal changes — only the trigger and send nodes get swapped.

## Customization Ideas

- Add multilingual support (Spanish + English) by detecting the prospect's message language and replying in kind.
- Plug in a vector database (Supabase pgvector, Pinecone) for richer document-based answers when the knowledge base grows beyond what fits in a prompt.
- Add appointment booking via Google Calendar so qualified leads can schedule a call directly in the chat.
- Send a daily digest of unanswered or flagged conversations to the admissions team.

---

Part of the [AI Automation portfolio](https://github.com/Echitoh) by Ezequiel Andrade.
