# Cloudflare Incident Triage Assistant

A small incident-triage assistant built on **Cloudflare Workers**, **Workers AI**, and **Durable Objects**.  
It’s designed to mirror how engineering teams investigate real production issues (deploy regressions, slowdowns, outages) by guiding a structured line of questioning and next steps.

---

## What it does

You describe an incident in plain language. The assistant responds in a consistent engineering format:

- **Summary** (what’s happening)
- **Likely causes** (what commonly explains this pattern)
- **Next steps** (what to check / change next)
- **Questions to ask** (what information is needed to confirm root cause)

The goal is practical triage and clearer debugging, not “chatbot-style” answers.

---

## Key features

- **Workers AI (Llama)** used to generate the triage response
- **Durable Objects** used to keep short-term context per incident session
- **API-first design** via `POST /chat`
- **Browser-friendly (CORS-safe)** so a simple frontend can call the API
- Multiple sessions supported (e.g. `demo`, `demo2`) to run parallel investigations

---

## Architecture (high level)

### Frontend (static HTML)
- Lightweight HTML + JavaScript UI
- Sends incident messages to the backend using `POST /chat`
- Uses a session name so each incident thread keeps its own context

The frontend is intentionally minimal so the focus stays on the Cloudflare backend primitives.

### Worker (API layer)
- Exposes `/chat`
- Validates requests and handles CORS
- Routes requests to a session-specific Durable Object

### Durable Object (ChatSession)
- Stores recent conversation turns for that session
- Preserves context across multi-step investigations
- Avoids shared-state conflicts between different incident threads

### Workers AI
- Receives the current message + recent context
- Returns a structured triage response

---

## Why Cloudflare

This project is intentionally built on Cloudflare to demonstrate:

- **Low-latency, global execution** with Workers
- **Stateful workflows without a database** using Durable Objects
- **LLM inference on-platform** using Workers AI

This combination is a good fit for internal engineering tools that need to be fast, simple to operate, and easy to extend.

---

## Quick flow

1. User enters an incident description in the frontend
2. Frontend sends `POST /chat` to the Worker
3. Worker forwards the request to a Durable Object for that session
4. Durable Object loads recent context and calls Workers AI
5. Response is returned as JSON and shown in the UI

---

## Possible next improvements

- Authentication / access control for team usage
- Incident severity tagging (e.g. P0–P3)
- Integrations with logs/metrics sources
- Streaming responses (SSE)
- A richer UI (formatting, history, export)
