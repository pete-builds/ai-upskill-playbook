# AI Upskill Playbook

This is my AI upskill origin story, one layer at a time.

I learn best by doing. Over the past few months I've been building a self-hosted AI stack from scratch, one service at a time, learning how the pieces connect along the way. Each section below is a real layer in that stack today.

There are many ways to upskill in AI. This is just the path I took. Whether you're looking to break into an emerging field, trying to support your business, or trying to do better in your current job, this can help get you going. This guide is probably best for someone with an IT background, but if you're curious and willing to learn, anyone can do it.

This is not comprehensive. I'm just getting started and there's so much more to learn. This is everything I've set up so far, and I'll keep updating it as I go. If you've been down this path before, I'd love to hear what I've missed.

### What you'll learn

By the end of this playbook, you'll understand how to:

- Set up an AI-native development environment with an AI coding assistant
- Run always-on infrastructure and containers on your own hardware
- Deploy and manage local LLMs on consumer hardware
- Route multiple AI providers through a single API gateway
- Build agentic workflows that delegate tasks to specialized AI agents
- Give your AI tools direct access to services via MCP servers
- Automate workflows connecting AI models, APIs, and databases
- Ship real web applications and monitor your entire stack

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
| 12 | [Build a Web App](#12-build-a-web-app) | Ship a real project with your AI coding assistant | Prove the stack works by building something |

### Keep It Running

| # | Layer | What | Why |
|---|-------|------|-----|
| 13 | [Monitoring + Infrastructure](#13-monitoring--infrastructure) | Uptime Kuma, Watchtower, Caddy, Tailscale | Keep it all running and reachable |

New to some of these terms? See the [Vocabulary](#vocabulary) at the bottom.

---

## Getting Started

You don't need all thirteen layers. Start with 1-3 and you'll already be ahead of most people. Each layer is optional. Skip what doesn't interest you, come back to it later.

The real unlock is **[Claude Code](https://code.claude.com/docs/en/overview) + a Linux box + [Docker](https://docs.docker.com/engine/install/)**. Once you have those three, building the rest is just conversations with your AI coding assistant.

---

## 1. Claude Code

[Claude Code](https://code.claude.com/docs/en/overview) is an AI coding assistant that runs in your terminal. It can read your files, write code, run commands, and iterate on problems with you. This is layer one because it accelerates everything that comes after. You're not just following tutorials anymore. You have a coding assistant that knows the docs.

- Get the [$20/month Claude Pro subscription](https://claude.ai/pricing). I know, nobody wants another subscription. Don't let that be the thing that stops you. The value you get back in productivity is not even close. This is the highest-leverage $20 you'll spend.
- Install [Claude Code](https://code.claude.com/docs/en/overview) (npm, requires Node.js)
- Connect to Anthropic API (direct key or API gateway)
- Learn the core loop: describe what you want, review what it does, iterate
- **Use Sonnet for everyday tasks** (quick edits, file searches, simple scripts). **Switch to Opus** (`/model opus`) for complex code, architecture decisions, debugging, and writing. Opus thinks deeper but costs more context.
- Learn [plan mode](https://code.claude.com/docs/en/common-workflows#use-plan-mode-for-safe-code-analysis) (`shift+tab`): Claude researches your codebase and proposes a plan before writing any code. Great for understanding unfamiliar projects or planning big changes.
- Set up [GitHub CLI](https://cli.github.com/) and [GitHub MCP](https://github.com/github/github-mcp-server) for version control and repo management from day one
- Customize your [statusline](https://code.claude.com/docs/en/interactive-mode#status-bar) (context usage, model, git status, session metrics). I built a [custom one](https://github.com/pete-builds/claude-code-statusline) with weather, billing tier, and battery.
- Create a [`CLAUDE.md`](https://code.claude.com/docs/en/memory) project file for persistent instructions
- Use `/init` to scaffold new projects
- Learn [slash commands](https://code.claude.com/docs/en/interactive-mode#built-in-commands): `/compact`, `/clear`, `/model`, `/cost`
- Set up [hooks](https://code.claude.com/docs/en/hooks) for session start, tool calls, and notifications
- Build [custom skills](https://code.claude.com/docs/en/skills) (slash commands) for repeatable workflows
- Create [custom subagents](https://code.claude.com/docs/en/sub-agents) to delegate specialized tasks

### Organize your workspace

Claude Code gets its value from proximity to files. Structure your workspace so the right context is always nearby.

```
workspace/
├── CLAUDE.md              # persistent instructions, agent routing, rules
├── Work/
│   ├── Projects/
│   │   ├── project-a/
│   │   └── project-b/
│   ├── Meeting-Notes/
│   ├── Issues/
│   ├── Documentation/
│   └── Scripts/
└── Personal/
    ├── Homelab/
    ├── Side-Projects/
    └── Notes/
```

Each folder can have its own `CLAUDE.md` with context specific to that area. When Claude works in a folder, it picks up those instructions automatically. The context lives with the work itself.

### Managing token usage

Tokens cost money and context windows fill up fast. A lot of what's in this playbook helps reduce burn without thinking about it.

- **Use the right model.** Sonnet for everyday tasks, Opus only when you need deep thinking. This alone cuts costs significantly.
- **Agentic workflows** isolate context. Each agent only reads what it needs instead of loading your entire workspace into one conversation.
- **MCP servers** let Claude call APIs directly instead of you pasting terminal output back and forth. Fewer tokens in, faster results out.
- **Workspace organization** means Claude finds what it needs quickly instead of searching through unrelated files.
- **Session resume files** give your agents a summary to pick up from instead of re-explaining the full history every session.
- **Subagents** get their own context windows, so research and parallel tasks don't eat into your main conversation.
- **`/compact`** summarizes your conversation in place when context gets heavy. **`/clear`** starts fresh.
- **Plan mode** (`shift+tab`) lets Claude research before writing, so you don't burn tokens on code that misses the mark.

**Key concept:** Claude Code isn't autocomplete. It's a coding assistant that can SSH into servers, deploy containers, write tests, and debug production issues. You just need to give it the right context and tools. Read [common workflows](https://code.claude.com/docs/en/common-workflows) and [best practices](https://code.claude.com/docs/en/best-practices) to see what's possible.

**Security note:** Claude Code can run commands on your machine. Be aware of [prompt injection](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/) risks: malicious content in files, repos, or web pages could try to trick Claude into running harmful commands. Review what Claude is doing before approving tool calls, especially with unfamiliar code. Never blindly consume unofficial code from repos you haven't reviewed, including the ones linked in this playbook. Read it first.

## 2. Linux Box

You need one computer that stays on. It doesn't need to be powerful. A used mini PC, an old laptop, or a NUC will do. Install [Ubuntu Server](https://ubuntu.com/download/server) or Debian.

- Any x86 machine with 8GB RAM and a 256GB SSD
- Ubuntu Server or Debian (headless, no desktop environment needed)
- SSH access configured (key-based, no passwords)
- Static IP or DHCP reservation on your router
- [Tailscale](https://tailscale.com/) for remote access without port forwarding
- Give Claude Code SSH access to your Linux box (key-based). This lets Claude deploy containers, check logs, and manage services directly from your laptop. It's a game changer.
- This is your foundation. Everything else is a container on this box.

## 3. Docker + Portainer

[Docker](https://docs.docker.com/engine/install/) lets you run applications in isolated containers. [Portainer](https://www.portainer.io/) gives you a web UI to manage them instead of memorizing CLI commands.

- Install [Docker Engine](https://docs.docker.com/engine/install/) (not Docker Desktop)
- Install [Portainer CE](https://docs.portainer.io/start/install-ce) as your first container
- Learn [docker-compose](https://docs.docker.com/compose/): one YAML file per service
- Understand volumes (persistent data) vs bind mounts
- Understand networking: bridge, host, and container DNS
- [Watchtower](https://github.com/containrrr/watchtower) for automatic image updates (optional, be careful in production)
- Once you have these two, installing new services goes from "follow a 47-step guide" to "paste a docker-compose file and click deploy"

## 4. Agentic Workflows

This is where Claude Code goes from "smart assistant" to "team of specialists." Instead of one conversation handling everything, you build purpose-built agents that each own a domain. See the official docs on [custom subagents](https://code.claude.com/docs/en/sub-agents) and [skills](https://code.claude.com/docs/en/skills).

- **What is an agent?** A [skill](https://code.claude.com/docs/en/skills) (slash command) that spawns a [subagent](https://code.claude.com/docs/en/sub-agents) with its own persona, context docs, tools, and SOPs
- **Why agents?** Context isolation. Each agent reads only what it needs, so it doesn't get confused by unrelated information.
- **Agent routing:** a table in [`CLAUDE.md`](https://code.claude.com/docs/en/memory) that maps topics to the right agent automatically
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

- Create a skill file in `.claude/commands/your-agent.md` (see [skills docs](https://code.claude.com/docs/en/skills))
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

[Model Context Protocol (MCP)](https://code.claude.com/docs/en/mcp) servers give Claude Code direct access to your services. Instead of copy-pasting between terminals, you just ask Claude to do it and it calls the API directly. See the [MCP server registry](https://registry.modelcontextprotocol.io/) for pre-built servers.

- **What is MCP?** A protocol that lets AI assistants call external tools via structured API
- **Transports:** SSE (network, accessible from any machine) vs stdio (local binary)
- **Build pattern:** Python + [FastMCP](https://github.com/jlowin/fastmcp) + httpx + Docker
- **Deploy as containers** on your Linux box, register with `claude mcp add`
- **Example servers you can build:**
  - Infrastructure management ([Portainer MCP](https://github.com/portainer/portainer-mcp))
  - [GitHub MCP](https://github.com/github/github-mcp-server) (PRs, issues, code search)
  - Workflow execution (n8n)
  - Home automation, DNS, network tools
  - Calendar, email, and other integrations
- Each MCP server = a set of tools Claude can call on demand
- SSE transport recommended. One server, accessible from any machine on your network.

## 6. LiteLLM

[LiteLLM](https://github.com/BerriAI/litellm) sits between your apps and AI model providers. Point everything at one URL, and LiteLLM routes to OpenAI, Anthropic, local models, or whatever you configure.

- Unified OpenAI-compatible API for 100+ model providers
- One endpoint, swap models without changing client code
- Track usage and costs across all providers
- Route between cloud APIs and local [Ollama](https://ollama.com/) models
- Load balancing and fallback between providers
- Set up your API connection once. Every tool downstream just talks to LiteLLM.

## 7. Local Models

Run open-source LLMs on your own hardware with [Ollama](https://ollama.com/). A Mac Mini with Apple Silicon is surprisingly capable, but any machine with a decent GPU works.

- [Ollama](https://ollama.com/) for model management and inference
- Apple Silicon (M-series) for surprisingly good local performance
- Models: [Llama](https://ollama.com/library/llama3.2), [Mistral](https://ollama.com/library/mistral), [Qwen](https://ollama.com/library/qwen2.5), [DeepSeek](https://ollama.com/library/deepseek-r1), [Gemma](https://ollama.com/library/gemma2), and more
- Free, private, and always available
- Register with LiteLLM so all your tools can use them
- Won't replace Claude for complex work, but perfect for bulk tasks, embeddings, and experimentation
- Run coding assistants, chat models, and embedding models side by side

## 8. SearXNG

[SearXNG](https://github.com/searxng/searxng) is a private metasearch engine that aggregates results from Google, Bing, DuckDuckGo, and dozens of other sources without tracking you.

- Self-hosted, no API keys needed
- Aggregates results from 70+ search engines
- JSON API for programmatic access
- Feed it to n8n workflows, LLM agents, and Perplexica
- Gives your AI tools and workflows a way to search the web
- Privacy-first: no tracking, no profiling, no ads

## 9. n8n

[n8n](https://n8n.io/) is a workflow automation platform. Think Zapier, but self-hosted and free. Drag and drop nodes to connect APIs, databases, AI models, and triggers.

- Visual workflow builder (no code required, but code is available when you need it)
- AI agent nodes with tool calling and memory
- Webhook triggers for real-time integrations
- Schedule-based workflows for recurring tasks
- Built-in MCP server: expose workflows as tools for Claude Code
- Build RAG pipelines: feed documents to an LLM so it can answer questions about your own data
- Connect to LiteLLM, Ollama, SearXNG, and everything else in your stack
- This is where your stack starts doing things on its own

## 10. Open WebUI

[Open WebUI](https://github.com/open-webui/open-webui) gives you a ChatGPT-style interface for all your models, local and remote. Point it at Ollama and LiteLLM and everyone in your house can use AI without a subscription.

- Web-based chat interface (looks and feels like ChatGPT)
- Connect to Ollama (local models) and LiteLLM (cloud models)
- Multi-user support with accounts
- Conversation history and search
- Document upload and RAG (retrieval-augmented generation)
- The "front door" layer. Everything you built powers what feels like a simple chat app.

## 11. Perplexica

[Perplexica](https://github.com/ItzCrazyKns/Perplexica) is a self-hosted alternative to Perplexity AI. It uses SearXNG for web search and your local/cloud models for synthesis.

- AI-powered research and answer engine
- Uses your SearXNG instance for web search (no API keys)
- Routes through your LLM stack (local or cloud models)
- Multiple search modes: general, academic, writing, Wolfram Alpha
- Self-hosted. Your searches stay on your network.

## 12. Build a Web App

The best way to prove your stack works is to build something real with it. Claude Code + Docker + your Linux box = you can go from idea to deployed webapp in one session.

### Build an agent first

Before you start coding, build a dedicated agent (see [Agentic Workflows](#4-agentic-workflows)) that owns the full lifecycle of your webapps. Give it a context doc with your deploy target, stack preferences, and the practices below. This keeps every project consistent and saves you from re-explaining the same things every session.

### Practices to bake into your agent

- **Use latest stable versions.** Web search to verify current versions of languages, frameworks, and base images before scaffolding.
- **Dockerize from the start.** Every app gets a `Dockerfile` and `docker-compose.yml`. No exceptions.
- **Session resume files.** Each project gets a `SESSION-RESUME.md` with stack, status, decisions, and deploy info so your agent can pick up where it left off.
- **Never deploy secrets to git.** API keys, tokens, and `.env` files stay on the server. Use environment variables in your compose file.
- **Deploy via git pull, not file copy.** Push to GitHub, pull on the server, rebuild. Keeps the server's working tree clean.
- **Security baselines.** CSRF protection, rate limiting, input validation. Your agent should enforce these by default.
- **Health checks.** Add a `/healthz` endpoint and a Docker `HEALTHCHECK` so monitoring can verify the app is running.

### Ship it

- **Start simple.** Pick a problem you actually have.
- **Example projects:**
  - [Phantom Paste](https://github.com/pete-builds/phantom-paste): zero-knowledge ephemeral pastebin (Go + SQLite + vanilla JS)
  - [Model Arena](https://github.com/pete-builds/open-model-arena): blind AI model comparison with ELO leaderboard (Python/FastAPI + SQLite)
- **The workflow:** Describe it to Claude Code, iterate on the code, Dockerize it, deploy to your Linux box
- **Push to GitHub.** Public repos build your portfolio.
- **[Tailscale Funnel](https://tailscale.com/kb/1223/funnel)** for sharing without exposing your network
- You'll learn more building one real thing than reading ten tutorials

## 13. Monitoring + Infrastructure

Once you have services running, you need to keep them running and make them accessible.

- **[Uptime Kuma](https://github.com/louislam/uptime-kuma):** monitor all your services, get alerts when something goes down
- **[Watchtower](https://github.com/containrrr/watchtower):** automatically pull and redeploy updated container images
- **[Caddy](https://caddyserver.com/):** reverse proxy with automatic HTTPS (TLS certificates)
- **[Tailscale](https://tailscale.com/):** mesh VPN for secure remote access without port forwarding
- **[Homepage](https://github.com/gethomepage/homepage)/[Homarr](https://github.com/ajnart/homarr):** dashboard to see everything at a glance

---

## Vocabulary

Terms you'll run into as you work through this playbook.

| Term | What It Means |
|------|---------------|
| **LLM** | Large Language Model. The AI models that power tools like ChatGPT, Claude, and Llama. |
| **Agent** | An AI assistant with a specific role, its own context, and access to tools. |
| **Subagent** | An agent spawned by another agent to handle a specific subtask. |
| **MCP** | Model Context Protocol. A standard that lets AI tools call external services directly. |
| **SSE** | Server-Sent Events. A transport method for MCP servers that works over the network. |
| **RAG** | Retrieval-Augmented Generation. Feeding relevant documents to an LLM so it can answer questions about them. |
| **Prompt injection** | A security risk where malicious text tricks an AI into doing something unintended. |
| **Context window** | The amount of text an LLM can process in a single conversation. Measured in tokens. |
| **Token** | A chunk of text (roughly a word or part of a word) that LLMs use to process language. |
| **Inference** | The process of running a prompt through a model and getting a response. |
| **Embedding** | A numerical representation of text that captures meaning. Used for search and similarity. |
| **API gateway** | A single endpoint that routes requests to multiple backend services or model providers. |
| **Docker container** | A lightweight, isolated environment that runs an application with all its dependencies. |
| **docker-compose** | A tool for defining and running multi-container Docker applications using a YAML file. |
| **Volume** | Persistent storage for Docker containers. Data survives container restarts. |
| **Reverse proxy** | A server that sits in front of your services, handles HTTPS, and routes traffic to the right container. |
| **Webhook** | A URL that triggers an action when another service sends it a request. |
| **SSH** | Secure Shell. A protocol for securely connecting to and running commands on remote machines. |
| **Headless** | A server running without a monitor, keyboard, or desktop environment. Managed entirely via SSH. |
| **Self-hosted** | Running software on your own hardware instead of paying for a cloud service. |

---

## About

Built and maintained by [pete-builds](https://github.com/pete-builds). This playbook reflects a real stack running in production on a home network, not a theoretical setup guide. It's a living document. Do your own research, dig into the docs, and make it yours.

Suggestions, corrections, and "you're missing X" welcome via [Issues](../../issues).
