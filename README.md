# Cloudflare AI Incident Assistant

An AI-powered incident analysis assistant built on **Cloudflare Workers**, **Workers AI**, and **Durable Objects**.  
This project simulates how engineering teams triage incidents during outages, regressions, or post-deployment issues.

> Built as part of a Cloudflare Software Engineering Internship application.

---

## 🚀 What This Project Does

The AI Incident Assistant helps engineers:
- Describe an incident in plain language
- Receive **structured, actionable guidance** instead of generic answers
- Maintain **conversation memory per incident/session**
- Focus on diagnosis, next steps, and clarifying questions

The assistant responds using a fixed incident-response structure:
- **Summary**
- **Likely Causes**
- **Next Steps**
- **Questions to Ask**

---

## 🧠 Key Features

- **LLM-powered reasoning** using Cloudflare Workers AI (Llama)
- **Durable Objects** for session-based memory
- **Stateless frontend** served separately from the Worker
- **CORS-safe API design**
- Multiple concurrent incident sessions (e.g. `demo`, `demo2`, etc.)

---

## 🏗 Architecture Overview


The application is split into two main parts:

### 1. Frontend (Static HTML)
- Simple HTML + JavaScript UI
- Allows users to submit incident descriptions
- Sends requests to the Worker API via `POST /chat`
- Supports multiple sessions using a session name (memory key)

The frontend is intentionally minimal to focus on backend architecture and Cloudflare primitives.

### 2. Cloudflare Worker (Backend API)
- Exposes a `/chat` endpoint
- Handles CORS safely for browser-based requests
- Routes requests to a Durable Object based on session name

### 3. Durable Object (ChatSession)
- Stores conversation history per incident/session
- Provides short-term memory across messages
- Enables multiple parallel incident investigations

### 4. Workers AI
- Uses Cloudflare Workers AI (Llama model)
- Receives structured prompts including prior conversation context
- Produces actionable, incident-focused responses


## ☁️ Why Cloudflare?

This project was intentionally built on Cloudflare to demonstrate:

- **Global, low-latency execution** using Workers
- **Stateful workflows** using Durable Objects instead of traditional databases
- **Serverless AI inference** using Workers AI
- Clear separation of frontend, API, and state

Cloudflare’s architecture makes it possible to build scalable, AI-powered developer tools without managing servers or infrastructure.


## 🔮 Future Improvements

- Authentication for team-based usage
- Severity classification for incidents
- Integration with logs or metrics APIs
- Streaming responses using Server-Sent Events
- Richer frontend UI and formatting
