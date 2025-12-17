# Cloudflare Incident Triage Assistant

An incident triage assistant built using **Cloudflare Workers**, **Workers AI**, and **Durable Objects**.  
The project explores how engineering teams investigate outages, regressions, and post-deployment performance issues in a structured and repeatable way.

---

## Overview

This tool provides a lightweight workflow to support early-stage incident analysis.  
Instead of returning generic chatbot-style answers, it guides engineers through a clear triage process.

Engineers can:

- Describe an incident in plain language  
- Receive structured guidance rather than open-ended responses  
- Maintain short-term context per incident using session-based memory  
- Focus on diagnosis, investigation steps, and follow-up questions  

The assistant is designed to **support reasoning and investigation**, not replace existing debugging or monitoring tools.

---

## Key Features

- Structured incident analysis generated using **Cloudflare Workers AI (Llama)**
- Session-based memory implemented with **Durable Objects**
- Serverless API built on **Cloudflare Workers**
- CORS-safe request handling for browser-based clients
- Supports multiple concurrent incident sessions (e.g. `demo`, `demo2`)

---

## Architecture

The application is intentionally split into clearly separated components to reflect a real-world production setup.

### Frontend (Static HTML)

- Lightweight HTML + JavaScript interface  
- Sends incident descriptions to the backend using `POST /chat`  
- Uses a session name so each incident thread maintains its own context  

The frontend is deliberately minimal to keep the focus on backend architecture and Cloudflare primitives.

---

### Worker (API Layer)

- Exposes a `/chat` endpoint  
- Handles request validation and CORS  
- Routes each request to a session-specific Durable Object  

---

### Durable Object (ChatSession)

- Stores recent conversation turns for a single incident  
- Preserves context across multi-step investigations  
- Prevents shared-state conflicts between parallel sessions  

---

### Workers AI

- Receives the current incident message along with recent context  
- Returns a structured triage response:
  - Summary  
  - Likely causes  
  - Next steps  
  - Questions to ask  

---

## Why Cloudflare

This project is intentionally built on Cloudflare to demonstrate:

- **Low-latency global execution** using Workers  
- **Stateful workflows without a database** using Durable Objects  
- **On-platform LLM inference** via Workers AI  

This architecture is well-suited for internal engineering tools that need to be fast, simple to operate, and easy to extend without managing infrastructure.

---

## Request Flow (High Level)

1. A user enters an incident description in the frontend  
2. The frontend sends a `POST /chat` request to the Worker  
3. The Worker forwards the request to a Durable Object based on session name  
4. The Durable Object loads recent context and calls Workers AI  
5. A structured response is returned as JSON and displayed in the UI  

---

## Possible Next Improvements

- Authentication and access control for team usage  
- Incident severity tagging (e.g. P0–P3)  
- Integration with logs or metrics systems  
- Streaming responses (SSE)  
- Richer UI (history view, exports, formatting)  
