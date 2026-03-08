# AI Upskill Playbook

One person's journey building a self-hosted AI stack from scratch, one layer at a time.

This is my origin story. Not a tutorial, not a best practices guide. Just the timeline of how I actually learned this stuff, what I built, and the order I'd do it again if I started over. Some of these layers happened in a different order the first time around, but this is how I'd approach it now. I've learned a lot along the way and I'm hoping it helps others who are curious.

---

## The Stack

### Foundation

| # | Layer | What | Why |
|---|-------|------|-----|
| 1 | [Claude Code](#1-claude-code) | AI-powered CLI dev environment | Your building speed multiplier. Everything else gets easier with this. |
| 2 | [Linux Box](#2-linux-box) | A dedicated machine running Linux | Everything else runs on this |
| 3 | [Docker + Portainer](#3-docker--portainer) | Containers and a management UI | Install anything without breaking everything |

### AI Stack

| # | Layer | What | Why |
|---|-------|------|-----|
| 4 | [Agentic Workflows](#4-agentic-workflows) | Specialized AI agents with routing and memory | Stop prompting, start delegating |
| 5 | [MCP Servers](#5-mcp-servers) | Model Context Protocol servers | Give Claude hands to control your stack |
| 6 | [LiteLLM](#6-litellm) | Unified API gateway for LLMs | One endpoint, any model |
| 7 | [Local Models](#7-local-models) | Ollama on a Mac Mini (or any GPU box) | Run models with zero API costs |
| 8 | [SearXNG](#8-searxng) | Private metasearch engine | Give your AI tools access to the web |
| 9 | [n8n](#9-n8n) | Workflow automation platform | Connect everything to everything |

### Interfaces

| # | Layer | What | Why |
|---|-------|------|-----|
| 10 | [Open WebUI](#10-open-webui) | Chat interface for local + remote models | A front door for everyone else |
| 11 | [Perplexica](#11-perplexica) | AI-powered search (self-hosted Perplexity) | Deep research without subscriptions |

### Ship Something

| # | Layer | What | Why |
|---|-------|------|-----|
| 12 | [Build a Web App](#12-build-a-web-app) | Ship a real project with your AI pair programmer | Prove the stack works by building something |

### Keep It Running

| # | Layer | What | Why |
|---|-------|------|-----|
| 13 | [Monitoring + Infrastructure](#13-monitoring--infrastructure) | Uptime Kuma, Watchtower, Caddy, Tailscale | Keep it all running and reachable |

---

## 1. Claude Code

Claude Code is an AI coding assistant that runs in your terminal. It can read your files, write code, run commands, and iterate on problems with you. This is layer one because it accelerates everything that comes after. You're not just following tutorials anymore. You have a pair programmer that knows the docs.

- Install Claude Code (npm, requires Node.js)
- Connect to Anthropic API (direct key or API gateway)
- Learn the core loop: describe what you want, review what it does, iterate
- Set up GitHub integration for version control from day one
- Customize your statusline (context usage, model, git status, session metrics)
- Create a `CLAUDE.md` project file for persistent instructions
- Use `/init` to scaffold new projects
- Learn slash commands: `/compact`, `/clear`, `/model`, `/cost`
- Set up hooks for session start, tool calls, and notifications

**Key concept:** Claude Code isn't autocomplete. It's a pair programmer that can SSH into servers, deploy containers, write tests, and debug production issues. You just need to give it the right context and tools.

## 2. Linux Box

You need one computer that stays on. It doesn't need to be powerful. A used mini PC, an old laptop, or a NUC will do. Install Ubuntu Server or Debian.

- Any x86 machine with 8GB RAM and a 256GB SSD
- Ubuntu Server or Debian (headless, no desktop environment needed)
- SSH access configured (key-based, no passwords)
- Static IP or DHCP reservation on your router
- Tailscale for remote access without port forwarding
- This is your foundation. Everything else is a container on this box.

## 3. Docker + Portainer

Docker lets you run applications in isolated containers. Portainer gives you a web UI to manage them instead of memorizing CLI commands.

- Install Docker Engine (not Docker Desktop)
- Install Portainer CE as your first container
- Learn docker-compose: one YAML file per service
- Understand volumes (persistent data) vs bind mounts
- Understand networking: bridge, host, and container DNS
- Watchtower for automatic image updates (optional, be careful in production)
- Once you have these two, installing new services goes from "follow a 47-step guide" to "paste a docker-compose file and click deploy"

## 4. Agentic Workflows

This is where Claude Code goes from "smart assistant" to "team of specialists." Instead of one conversation handling everything, you build purpose-built agents that each own a domain.

- **What is an agent?** A slash command that spawns a subagent with its own persona, context docs, tools, and SOPs
- **Why agents?** Context isolation. Each agent reads only what it needs, so it doesn't get confused by unrelated information.
- **Agent routing:** a table in `CLAUDE.md` that maps topics to the right agent automatically
- **Trigger phrases:** each agent announces itself before working ("Checking with Tank...stand by")
- **Subagent strategy:** offload research and parallel work to subagents, one task per agent
- **Memory files:** persistent notes that agents read at session start to build on previous work

### Example Agent Roster

| Agent | Domain | What It Does |
|-------|--------|-------------|
| Tank | Infrastructure | Checks containers, servers, networking, system health |
| Link | Webapps | Builds, deploys, debugs self-hosted web applications |
| Forge | MCP Servers | Designs, builds, deploys MCP servers for your services |
| Oracle | Career | Job positioning, resume strategy, organizational navigation |
| Switch | Config | Claude Code setup, LiteLLM configuration, troubleshooting |
| Architect | Design | Reviews workflows, system architecture, separation of concerns |

### Building Your First Agent

- Create a skill file in `.claude/commands/your-agent.md`
- Write a context doc with system knowledge, SOPs, and constraints
- Define a trigger phrase and startup sequence
- Add it to the routing table in `CLAUDE.md`
- The agent reads its context doc every time it's invoked, so it stays current

### Workflow Principles

- **Plan before building:** for 3+ step tasks, outline the plan first
- **Explain before acting:** say what's about to happen before running tools
- **Verify before done:** run it, check the output, confirm it works
- **Self-improvement loop:** log corrections in a lessons file, review at session start

## 5. MCP Servers

Model Context Protocol (MCP) servers give Claude Code direct access to your services. Instead of copy-pasting between terminals, you just ask Claude to do it and it calls the API directly.

- **What is MCP?** A protocol that lets AI assistants call external tools via structured API
- **Transports:** SSE (network, accessible from any machine) vs stdio (local binary)
- **Build pattern:** Python + FastMCP + httpx + Docker
- **Deploy as containers** on your Linux box, register with `claude mcp add`
- **Example servers you can build:**
  - Infrastructure management (Portainer, Uptime Kuma)
  - Workflow execution (n8n)
  - Home automation, DNS, network tools
  - GitHub (PRs, issues, code search)
  - Calendar, email, and other integrations
- Each MCP server = a set of tools Claude can call on demand
- SSE transport recommended. One server, accessible from any machine on your network.

## 6. LiteLLM

LiteLLM sits between your apps and AI model providers. Point everything at one URL, and LiteLLM routes to OpenAI, Anthropic, local models, or whatever you configure.

- Unified OpenAI-compatible API for 100+ model providers
- One endpoint, swap models without changing client code
- Track usage and costs across all providers
- Route between cloud APIs and local Ollama models
- Load balancing and fallback between providers
- Set up your API connection once. Every tool downstream just talks to LiteLLM.

## 7. Local Models

Run open-source LLMs on your own hardware with Ollama. A Mac Mini with Apple Silicon is surprisingly capable, but any machine with a decent GPU works.

- Ollama for model management and inference
- Apple Silicon (M-series) for surprisingly good local performance
- Models: Llama, Mistral, Qwen, DeepSeek, Gemma, and more
- Free, private, and always available
- Register with LiteLLM so all your tools can use them
- Won't replace Claude for complex work, but perfect for bulk tasks, embeddings, and experimentation
- Run coding assistants, chat models, and embedding models side by side

## 8. SearXNG

SearXNG is a private metasearch engine that aggregates results from Google, Bing, DuckDuckGo, and dozens of other sources without tracking you.

- Self-hosted, no API keys needed
- Aggregates results from 70+ search engines
- JSON API for programmatic access
- Feed it to n8n workflows, LLM agents, and Perplexica
- Gives your AI tools and workflows a way to search the web
- Privacy-first: no tracking, no profiling, no ads

## 9. n8n

n8n is a workflow automation platform. Think Zapier, but self-hosted and free. Drag and drop nodes to connect APIs, databases, AI models, and triggers.

- Visual workflow builder (no code required, but code is available when you need it)
- AI agent nodes with tool calling and memory
- Webhook triggers for real-time integrations
- Schedule-based workflows for recurring tasks
- Built-in MCP server: expose workflows as tools for Claude Code
- Connect to LiteLLM, Ollama, SearXNG, and everything else in your stack
- This is where your stack starts doing things on its own

## 10. Open WebUI

Open WebUI gives you a ChatGPT-style interface for all your models, local and remote. Point it at Ollama and LiteLLM and everyone in your house can use AI without a subscription.

- Web-based chat interface (looks and feels like ChatGPT)
- Connect to Ollama (local models) and LiteLLM (cloud models)
- Multi-user support with accounts
- Conversation history and search
- Document upload and RAG (retrieval-augmented generation)
- The "front door" layer. Everything you built powers what feels like a simple chat app.

## 11. Perplexica

Perplexica is a self-hosted alternative to Perplexity AI. It uses SearXNG for web search and your local/cloud models for synthesis.

- AI-powered research and answer engine
- Uses your SearXNG instance for web search (no API keys)
- Routes through your LLM stack (local or cloud models)
- Multiple search modes: general, academic, writing, Wolfram Alpha
- Self-hosted. Your searches stay on your network.

## 12. Build a Web App

The best way to prove your stack works is to build something real with it. Claude Code + Docker + your Linux box = you can go from idea to deployed webapp in one session.

- **Start simple:** Pick a problem you actually have
- **Example projects:**
  - Phantom Paste: zero-knowledge ephemeral pastebin (Go + SQLite + vanilla JS)
  - Model Arena: blind AI model comparison with ELO leaderboard (Python/FastAPI + SQLite)
- **The workflow:** Describe it to Claude Code, iterate on the code, Dockerize it, deploy to your Linux box
- **Push to GitHub.** Public repos build your portfolio.
- **Tailscale Funnel** for sharing without exposing your network
- You'll learn more building one real thing than reading ten tutorials

## 13. Monitoring + Infrastructure

Once you have services running, you need to keep them running and make them accessible.

- **Uptime Kuma:** monitor all your services, get alerts when something goes down
- **Watchtower:** automatically pull and redeploy updated container images
- **Caddy:** reverse proxy with automatic HTTPS (TLS certificates)
- **Tailscale:** mesh VPN for secure remote access without port forwarding
- **Homepage/Homarr:** dashboard to see everything at a glance

---

## Getting Started

You don't need all thirteen layers. Start with 1-3 and you'll already be ahead of most people. Each layer is optional. Skip what doesn't interest you, come back to it later.

The real unlock is **Claude Code + a Linux box + Docker**. Once you have those three, building the rest is just conversations with your AI pair programmer.

---

## About

Built and maintained by [pete-builds](https://github.com/pete-builds). This playbook reflects a real stack running in production on a home network, not a theoretical setup guide.

Suggestions and questions welcome via [Issues](../../issues).
