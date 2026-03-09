# AI Upskill Playbook

*By [pete-builds](https://github.com/pete-builds) · Last updated March 2026*

**A field guide to the applied AI stack, built one layer at a time.**

This playbook covers applied AI engineering: how to integrate models, build agent workflows, connect tools, automate operations, and ship real applications on infrastructure you control. It does not cover model training, neural network theory, or machine learning research. Those are different disciplines. This is the infrastructure and tooling side.

These are the skills companies are hiring for in 2026 under titles like AI engineer, AI platform engineer, and automation architect. Docker, API orchestration, model gateways, tool calling, workflow automation, RAG pipelines, local inference. If you were hired tomorrow to build or support an AI-powered operation, this is what the stack looks like.

This is not a step-by-step tutorial. It's a map. Each section introduces a technology, explains why it matters, and gives you enough context to start researching on your own. Think of it as a checklist of things worth learning, with starting points and checkpoints so you know when you've got it.

### Who this is for

- IT professionals and sysadmins looking to move into AI-adjacent roles by building on skills you already have
- Builders who want to learn AI by standing up the same tools teams use in production
- Anyone supporting business or client workflows who needs to understand how AI tooling fits together in practice

---

Over the past few months I've been building this stack one service at a time, trying to understand how the pieces connect. Each section below is a real layer in that stack today.

I'm still learning. What you see here is everything I've built so far, and I'll keep updating it as the stack evolves.

---

## The Stack

### Part I: Get Going

Everything in this section runs on your laptop. No server needed.

| # | Layer | What | Why |
|---|-------|------|-----|
| 1 | [Claude Code](#1-claude-code) | AI-powered CLI dev environment | An AI coding assistant that accelerates everything that comes after |
| 2 | [Agentic Workflows](#2-agentic-workflows) | Specialized AI agents with routing and memory | Isolate context and delegate tasks to purpose-built agents |
| 3 | [MCP — Connecting Tools](#3-mcp--connecting-tools) | Give AI tools direct access to services | Cloud-hosted integrations, no server needed |

### Part II: Self-Hosted AI Business Infrastructure

A dedicated Linux box running Docker becomes your operations platform. Run it in production or use it as a learning sandbox.

| # | Layer | What | Why |
|---|-------|------|-----|
| 4 | [Linux Box](#4-linux-box) | A dedicated machine running Linux | Always-on platform for everything below |
| 5 | [Docker + Portainer](#5-docker--portainer) | Containers and a management UI | Install anything without breaking everything |
| 6 | [MCP — Building Your Own](#6-mcp--building-your-own) | Build custom MCP tools | Give AI tools direct access to your services |
| 7 | [LiteLLM](#7-litellm) | Unified API gateway for LLMs | One endpoint, any model |
| 8 | [Local LLMs](#8-local-llms) | Ollama on your Mac, PC, or GPU box | Run models with zero API costs |
| 9 | [SearXNG](#9-searxng) | Private metasearch engine | Give your AI tools access to the web |
| 10 | [n8n](#10-n8n) | Workflow automation platform | Where it all comes together |
| 11 | [Open WebUI](#11-open-webui) | Chat interface for local + remote models | A front door for everyone else |
| 12 | [Perplexica](#12-perplexica) | AI-powered search (self-hosted Perplexity) | Deep research without subscriptions |
| 13 | [Monitoring + Infrastructure](#13-monitoring--infrastructure) | Uptime Kuma, Caddy, Tailscale | Keep it all running and reachable |

New to some of these terms? See the [Vocabulary](#vocabulary) at the bottom.

---

## Getting Started

Layers 1-3 run entirely on your laptop. Once you're ready to go deeper, Part II walks you through setting up your own infrastructure.

---

## 1. Claude Code

[Claude Code](https://code.claude.com/docs/en/overview) is an AI coding assistant that runs in your terminal. It reads your files, writes code, runs commands, and iterates on problems with you. This is layer one because it accelerates everything that comes after.

- Get the [$20/month Claude Pro subscription](https://claude.com/pricing) (includes Claude Code access)
- Install [Claude Code](https://code.claude.com/docs/en/overview) in your terminal, VS Code, or whatever IDE you prefer (npm, requires Node.js)
- Connect to Anthropic API (direct key or API gateway)
- Learn the core loop: describe what you want, review what it does, iterate
- Understand [context windows](https://docs.anthropic.com/en/docs/build-with-claude/context-windows): the amount of text a model can process in a single conversation, measured in tokens. Everything you send and receive counts against it. When it fills up, the model loses track of earlier context.
- Use Sonnet for everyday tasks (quick edits, file searches, simple scripts). Switch to Opus (`/model opus`) for complex code, architecture decisions, debugging, and writing. Opus thinks deeper but costs more context.
- Learn [plan mode](https://code.claude.com/docs/en/common-workflows#use-plan-mode-for-safe-code-analysis) (`shift+tab`): Claude researches your codebase and proposes a plan before writing any code. Great for understanding unfamiliar projects or planning big changes.
- Set up [GitHub CLI](https://cli.github.com/) and [GitHub MCP](https://github.com/github/github-mcp-server) for version control and repo management from day one
- Customize your [statusline](https://code.claude.com/docs/en/statusline) (context usage, model, git status, session metrics). I built a [custom one](https://github.com/pete-builds/claude-code-statusline) with weather, billing tier, and battery.
- Create a [`CLAUDE.md`](https://code.claude.com/docs/en/memory) project file for persistent instructions
- Use `/init` to scaffold new projects
- Learn [slash commands](https://code.claude.com/docs/en/interactive-mode#built-in-commands): `/compact`, `/clear`, `/model`, `/cost`
- Set up [hooks](https://code.claude.com/docs/en/hooks) for session start, tool calls, and notifications
- Build [custom skills](https://code.claude.com/docs/en/skills) (slash commands) for repeatable workflows
- Create [custom subagents](https://code.claude.com/docs/en/sub-agents) to delegate specialized tasks

### Organize your workspace

Claude Code works best with proximity to files. Structure your workspace so the right context is always nearby.

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

Tokens cost money and context windows fill up fast. Several patterns help:

- Use the right model. Sonnet for everyday tasks, Opus for deep thinking.
- Agentic workflows isolate context. Each agent reads only what it needs.
- MCP servers let Claude call APIs directly instead of you pasting terminal output back and forth.
- Workspace organization means Claude finds what it needs quickly.
- Session resume files let agents pick up where they left off without re-explaining history.
- Subagents get their own context windows, keeping research separate from your main conversation.
- `/compact` summarizes in place when context gets heavy. `/clear` starts fresh.
- Plan mode (`shift+tab`) lets Claude research before writing.

Read [common workflows](https://code.claude.com/docs/en/common-workflows) and [best practices](https://code.claude.com/docs/en/best-practices) to see what's possible beyond basic code editing.

Security note: Claude Code can run commands on your machine. Be aware of [prompt injection](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/mitigate-jailbreaks) risks: malicious content in files, repos, or web pages could try to trick it into running harmful commands. Review tool calls before approving, especially with unfamiliar code. Never blindly consume unofficial code from repos you haven't reviewed, including the ones linked in this playbook.

> ✓ Checkpoint: Claude Code
>
> You should now be able to:
> - Edit code, run commands, and iterate on problems without leaving your terminal
> - Switch models (`/model opus` for deep work, back to Sonnet for quick tasks)
>
> Test it:
> Run `claude` in a project directory. Ask it to read a file, explain what it does, and make a small improvement. Review the changes and approve them.
>
> Next unlock:
> Build specialized agents that delegate work instead of handling everything in one conversation.

## 2. Agentic Workflows

Instead of one conversation handling everything, you build purpose-built agents that each own a domain. No server needed. This all runs on your laptop. See the docs on [custom subagents](https://code.claude.com/docs/en/sub-agents) and [skills](https://code.claude.com/docs/en/skills).

- What is an agent? A [skill](https://code.claude.com/docs/en/skills) (slash command) that spawns a [subagent](https://code.claude.com/docs/en/sub-agents) with its own persona, context docs, tools, and SOPs
- Why agents? Context isolation. Each agent reads only what it needs.
- Agent routing: a table in [`CLAUDE.md`](https://code.claude.com/docs/en/memory) maps topics to the right agent
- Trigger phrases: each agent announces itself before working ("Checking with Tank...stand by")
- Memory files: persistent notes that agents read at session start to build on previous work

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

There are two ways to build an agent. Start with the easy way.

The easy way: ask Claude Code to build it for you.

Describe what you want the agent to do, what domain it covers, and what tools it needs. Claude will create the skill file, write the context doc, and update your `CLAUDE.md` routing table.

```
I want to build an agent called Tank that manages my homelab infrastructure.
It should check container health, view logs, and deploy services on my
Linux server. Create everything it needs to work.
```

Claude handles the rest. Review what it built, test it, and refine from there.

The manual way: build each piece yourself.

If you want to understand what's under the hood:

- Create a skill file in `.claude/commands/your-agent.md` (see [skills docs](https://code.claude.com/docs/en/skills))
- Write a context doc with system knowledge, SOPs, and constraints
- Define a trigger phrase and startup sequence
- Add it to the routing table in `CLAUDE.md`
- The agent reads its context doc every time it's invoked, so it stays current

See [Appendix A](#appendix-a-prompts-and-templates) for copy-paste templates of each file.

### Workflow Principles

Bake these into every agent. Tell Claude to add them to each agent's skill file, or add them manually.

- Plan before building: for 3+ step tasks, outline the plan first
- Explain before acting: say what's about to happen before running tools
- Verify before done: run it, check the output, confirm it works
- Self-improvement loop: log corrections in a lessons file, review at session start

> ✓ Checkpoint: Agentic Workflows
>
> You should now be able to:
> - Route topics to specialized agents using a trigger phrase and skill invocation
> - Isolate context so each agent only reads what it needs
>
> Test it:
> Create a simple agent with a skill file in `.claude/commands/`, a context doc, and an entry in your `CLAUDE.md` routing table. Invoke it with the trigger phrase and verify it reads its context.
>
> Next unlock:
> Give your agents direct access to external services via cloud-hosted MCP servers.

## 3. MCP — Connecting Tools

[Model Context Protocol (MCP)](https://modelcontextprotocol.io/docs/getting-started/intro) servers give AI tools direct access to external services via structured API calls. Many MCP servers are cloud-hosted or run locally on your laptop:

- [GitHub MCP](https://github.com/github/github-mcp-server): PRs, issues, code search, repo management
- [Google Calendar](https://console.cloud.google.com/): schedule, check availability, create events
- [Gmail](https://console.cloud.google.com/): read, search, draft emails
- [Anthropic's built-in connectors](https://code.claude.com/docs/en/mcp): many integrations available out of the box

Each MCP server is a set of tools your AI assistant can call on demand. Register them with `claude mcp add` and they're available in every conversation. See the [MCP server registry](https://registry.modelcontextprotocol.io/) for more. Once you have a Linux box (Part II), you can also [build and self-host your own](#6-mcp--building-your-own).

> ✓ Checkpoint: MCP — Connecting Tools
>
> You should now be able to:
> - Understand what the protocol is and how it's used
> - Register cloud-hosted MCP servers (GitHub, Gmail, Calendar) with `claude mcp add`
> - Ask Claude to interact with those services directly (read GitHub issues, check your calendar)
>
> Test it:
> Register GitHub MCP, then ask Claude to list your recent pull requests or search code in a repo. Verify it calls the API instead of you copy-pasting terminal output.
>
> Next unlock:
> Set up a Linux box to self-host infrastructure and run your own services.

Need templates? See [Appendix A: Prompts and Templates](#appendix-a-prompts-and-templates) for copy-paste-ready examples.

---

## Part II: Self-Hosted Infrastructure

Docker, API gateways, workflow automation, and local models are showing up in job descriptions and team stacks as baseline expectations. Even if you never run production services, having this stack in your own sandbox gives you hands-on experience with the tools teams deploy at scale.

## 4. Linux Box

Everything from here on runs on a server: a $5/month cloud VPS, a spare PC, a used mini PC, or a NUC. If it turns on and stays on, it works. A cloud VPS (DigitalOcean, Hetzner, Linode) works too. You'll skip the physical setup and go straight to SSH.

- Any x86 machine with 8GB RAM and a 256GB SSD, or a cloud VPS
- [Ubuntu Server](https://ubuntu.com/download/server) or Debian (headless, no desktop environment needed)
- SSH access configured (key-based, no passwords)
- Static IP or DHCP reservation on your router (or use the VPS IP)
- A NAS or external drive for backups (optional but smart)
- [Tailscale](https://tailscale.com/) for remote access without port forwarding
- Give your AI coding assistant SSH access (key-based) so it can deploy containers, check logs, and manage services directly from your laptop
- This is your foundation. Everything else is a container on this box.

### Build Your Sysadmin Agent

This is where the agent framework from Part I pays off. Create an agent that manages your Linux box, give it SSH access, and let it handle health checks, log viewing, container restarts, and deployments.

Do it responsibly. Use key-based auth with a dedicated key you can revoke. Restrict sudo to specific commands. Prefer MCP tools (like Portainer) over raw SSH when possible, since APIs have structured permissions. Always review what the agent is doing before approving destructive operations. See [Appendix B: Building Tank](#appendix-b-case-study--building-tank-the-homelab-sysadmin-agent) for a full walkthrough.

> ✓ Checkpoint: Linux Box
>
> You should now be able to:
> - SSH into a dedicated Linux machine with key-based authentication
> - Give your AI coding assistant SSH access to deploy and manage services remotely
>
> Test it:
> From your laptop, ask Claude to SSH into your Linux box, check disk space, and show running processes. Verify the output.
>
> Next unlock:
> Install Docker and Portainer to run containers instead of installing everything bare-metal.

## 5. Docker + Portainer

[Docker](https://docs.docker.com/engine/install/) runs applications in isolated containers. [Portainer](https://www.portainer.io/) gives you a web UI to manage them.

- Install [Docker Engine](https://docs.docker.com/engine/install/) (not Docker Desktop)
- Install [Portainer CE](https://docs.portainer.io/start/install-ce) as your first container
- Learn [docker-compose](https://docs.docker.com/compose/): one YAML file per service
- Understand volumes (persistent data) vs bind mounts
- Understand networking: bridge, host, and container DNS

> ✓ Checkpoint: Docker + Portainer
>
> You should now be able to:
> - Deploy containers from docker-compose files via Portainer's web UI
> - Understand volumes (persistent data) and container networking
>
> Test it:
> Deploy a simple service (nginx, hello-world) using a docker-compose file. Check logs, restart the container, and verify data persists across restarts.
>
> Next unlock:
> Build your own MCP servers to give Claude direct access to your homelab services.

## 6. MCP — Building Your Own

Once you have a Linux box and Docker, you can build your own MCP servers and give AI tools direct access to your services.

- Build pattern: Python + [FastMCP](https://github.com/PrefectHQ/fastmcp) + httpx + Docker
- Transports: SSE (network, accessible from any machine) vs stdio (local binary)
- Deploy as containers on your Linux box, register with `claude mcp add`
- Example servers you can build:
  - Infrastructure management ([Portainer MCP](https://github.com/portainer/portainer-mcp))
  - Workflow execution (n8n)
  - Home automation, DNS, network tools
  - Media management, monitoring, and more
- SSE transport recommended. One server, accessible from any machine on your network.

> ✓ Checkpoint: MCP — Building Your Own
>
> You should now be able to:
> - Build a custom MCP server using FastMCP + Docker + SSE transport
> - Register it with Claude Code and call its tools from any conversation
>
> Test it:
> Build a simple MCP server with 1-2 tools (e.g., check container status, read a file). Deploy as a container, register with `claude mcp add`, and invoke a tool.
>
> Next unlock:
> Set up LiteLLM to route between cloud APIs and local models from one endpoint.

## 7. LiteLLM

[LiteLLM](https://github.com/BerriAI/litellm) sits between your apps and AI model providers. Point everything at one URL, and LiteLLM routes to OpenAI, Anthropic, local models, or whatever you configure.

### Core Features

- Unified API: OpenAI-compatible endpoint for 100+ providers. Swap models without changing client code.
- Cost Tracking: Log every request, calculate costs in real time across all providers.
- Fallback Routing: Chain free → local → paid models. If one fails, the next picks up automatically.
- Team Key Management: Issue virtual keys instead of distributing real API keys. Track attribution, enforce budgets, revoke in one place.
- Rate Limiting: Cap requests per minute/hour to prevent runaway costs.
- Model Aliasing: Give models friendly names (`fast`, `smart`). Change the backend without changing client code.

> ✓ Checkpoint: LiteLLM
>
> You should now be able to:
> - Point apps at LiteLLM's unified endpoint and route to OpenAI, Anthropic, or Ollama
> - Track usage and costs across all providers
>
> Test it:
> Deploy LiteLLM, configure one cloud provider and one local Ollama model. Send a request to each via LiteLLM's OpenAI-compatible API and verify responses.
>
> Next unlock:
> Run local models with Ollama for free, private inference.

## 8. Local LLMs

A local model on consumer hardware won't match the latest Claude or GPT for complex reasoning or code generation. That's not the point. Local models are worth running for other reasons:

- Free. Once downloaded, every request costs nothing. A broken n8n workflow firing 10,000 requests overnight costs $0 against a local model.
- Private. Nothing leaves your network. Sensitive data never touches a third-party API.
- Always available. No rate limits, no outages, no API keys expiring at 2 AM.
- Fallback. Register with LiteLLM and they become the last resort in your routing chain. Cloud API down? Local model picks up automatically.
- Learning experience. Understanding how models run, what hardware they need, and how quantization affects quality teaches you things you can't learn from API calls alone.

### Getting started

Install [Ollama](https://ollama.com/) on any Mac, Linux box, or Windows PC with a GPU. One-line install on Mac and Linux.

```
ollama pull llama3.2
ollama run llama3.2
```

### Picking the right model for your hardware

The biggest factor is VRAM (GPU memory) or unified memory (Apple Silicon). Models come in different sizes, and bigger models need more memory. Here's a rough guide:

| Memory | What you can run | Examples |
|--------|-----------------|----------|
| 8 GB | Small models (1-3B parameters) | Llama 3.2 3B, Gemma 2 2B, Phi-3 Mini |
| 16 GB | Medium models (7-8B parameters) | Llama 3.1 8B, Mistral 7B, Qwen 2.5 7B |
| 32 GB | Large models (13-14B parameters) | Llama 3.1 13B, DeepSeek-R1 14B |
| 48-64 GB | Extra large models (30-70B parameters) | Llama 3.1 70B (quantized), Qwen 2.5 72B (quantized) |

Apple Silicon (M1/M2/M3/M4) uses unified memory (GPU shares system RAM). A Mac Mini with 16GB runs 7-8B models comfortably. 32GB opens up the 13-14B range.

NVIDIA GPUs use dedicated VRAM. An RTX 3060 (12GB) handles 7B models. An RTX 3090/4090 (24GB) gives room for larger models.

Quantization compresses models to use less memory at a small quality cost. A 70B model that normally needs 140GB can run in ~40GB at 4-bit quantization. Ollama handles this with model tags (e.g., `llama3.1:70b-q4_0`).

Start with a 7-8B model: fast enough to be useful, smart enough for automation tasks, small enough to leave room for other services.

- [Ollama](https://ollama.com/) for model management and inference
- Models: [Llama](https://ollama.com/library/llama3.2), [Mistral](https://ollama.com/library/mistral), [Qwen](https://ollama.com/library/qwen2.5), [DeepSeek](https://ollama.com/library/deepseek-r1), [Gemma](https://ollama.com/library/gemma2), and more
- Register with LiteLLM so all your tools can use them
- Run coding assistants, chat models, and embedding models side by side

> ✓ Checkpoint: Local Models
>
> You should now be able to:
> - Run open-source LLMs locally with Ollama (Llama, Mistral, Qwen, DeepSeek)
> - Register them with LiteLLM so all your tools can access them
>
> Test it:
> Pull a model with `ollama pull`, run a prompt with `ollama run`, then query the same model through LiteLLM's API. Compare speed and quality to cloud models.
>
> Next unlock:
> Deploy SearXNG to give your AI tools private web search capability.

## 9. SearXNG

[SearXNG](https://github.com/searxng/searxng) is a private metasearch engine that aggregates results from 70+ sources without tracking you. It's the search backend for the rest of your stack: n8n workflows query it for web data, Perplexica uses it as its search engine, and any custom tool you build can hit the JSON API for real-time web results.

### Why self-host search?

Google's API costs money and rate-limits aggressively. Bing's API requires an Azure account. SearXNG gives you unlimited programmatic search with zero API keys and zero per-query costs. When an n8n workflow fires 500 search queries overnight to build a research report, that's $0. When your AI agent needs real-time web data to answer a question, it hits your local instance with no auth, no rate limits, and no third-party tracking.

- Self-hosted, no API keys needed
- JSON API for programmatic access
- Privacy-first: no tracking, no profiling, no ads
- Aggregates Google, Bing, DuckDuckGo, Wikipedia, and 70+ other sources in one query

> ✓ Checkpoint: SearXNG
>
> You should now be able to:
> - Search the web via SearXNG's JSON API without tracking or API keys
> - Point AI tools and workflows at your self-hosted search instance
>
> Test it:
> Deploy SearXNG, run a search query via the web UI, then hit the JSON API endpoint with curl or your browser. Verify results from multiple engines.
>
> Next unlock:
> Build automated workflows in n8n that connect AI, search, APIs, and triggers.

## 10. n8n

This is the layer where everything comes together.

[n8n](https://n8n.io/) is a self-hosted workflow automation platform. Drag and drop nodes to connect APIs, databases, AI models, and triggers. Everything you've built so far (LiteLLM, local models, SearXNG, MCP) becomes the backend for automations you build here. If this playbook has a "prove it works" moment, this is it. An n8n workflow that takes a webhook, routes it through your LLM gateway, queries your search engine, and posts the result to Slack is a real, deployable automation built entirely on your stack.

- Visual workflow builder (code available when you need it)
- AI agent nodes with tool calling and memory
- Webhook triggers for real-time integrations
- Schedule-based workflows for recurring tasks
- Built-in MCP server: expose workflows as tools for AI coding assistants
- Build RAG pipelines: feed documents to an LLM so it can answer questions about your own data
- Connect to LiteLLM, Ollama, SearXNG, and everything else in your stack

### n8n Workflow Examples

Five patterns that show what you can build with n8n + LiteLLM. Copy and adapt to your needs.

All five workflows connect to LiteLLM via the same pattern:

```
POST http://your-litellm-server:4000/v1/chat/completions
Headers: Authorization: Bearer sk-litellm-master-key
Body:
{
  "model": "gemini/gemini-2.0-flash-exp",
  "messages": [
    {"role": "system", "content": "Your system prompt here."},
    {"role": "user", "content": "{{ $json.your_data }}"}
  ]
}
```

Swap `model` to route to any provider. Use `v1/embeddings` for RAG embedding steps.

#### 1. Webhook to AI Summary to Slack

Webhook trigger → HTTP Request to LiteLLM → Slack message. Any service sends JSON, an LLM summarizes it, result posts to Slack. Example: Uptime Kuma alerts get turned into "nix1 went offline at 2:34 PM" in your Slack channel.

#### 2. Daily Report Generator

Schedule trigger (cron) → pull from GitHub, Uptime Kuma, Portainer APIs → LLM synthesis → email. Every morning at 8 AM, you get a status report covering GitHub activity, service uptime, and container health.

#### 3. RAG Pipeline (Searchable Knowledge Base)

Upload documents → split into chunks → generate embeddings via LiteLLM → store in vector DB (Pinecone, Chroma, pgvector) → query with natural language. Makes internal docs searchable with AI. Key nodes: Document Splitter, Embeddings, Vector Store, AI Agent.

#### 4. Multi-Model Evaluation

Send the same prompt to 3+ models in parallel via LiteLLM (just change the `model` field), merge results, log to Google Sheets. Example: run 20 support tickets through GPT-4o, Claude Sonnet, and local Llama. If Llama matches quality, switch and pay nothing.

#### 5. AI-Powered Triage System

Email/webhook trigger → LLM classifies into categories (URGENT, QUESTION, BUG, SPAM) → Switch node routes to Slack, support queue, GitHub issues, or archive. Zero-shot classification, no training data needed.

### Tips for Building n8n Workflows

- Start with a webhook trigger. Test manually before connecting real data sources.
- Use LiteLLM for flexibility. Swap models without rewriting workflows.
- Log everything. Add a "Write to Google Sheets" or "Append to File" node so you can review what the workflow did.
- Error handling matters. Use n8n's error workflows to catch API failures.
- Keep prompts in one place. Use n8n's "Set" node to define your system prompt as a variable at the top of the workflow.
- LiteLLM fallback groups. Configure `smart` and `fast` groups so workflows keep running if your primary model is down.

These five patterns cover the most common use cases. Everything else is variations on these.

> ✓ Checkpoint: n8n
>
> You should now be able to:
> - Build visual workflows connecting AI models, APIs, webhooks, and schedules
> - Expose workflows as MCP tools for Claude Code to invoke
>
> Test it:
> Create a simple workflow with a webhook trigger and an LLM node. Send a request to the webhook URL and verify the workflow executes and returns a response.
>
> Next unlock:
> Deploy Open WebUI to give everyone a ChatGPT-style interface for your models.

## 11. Open WebUI

[Open WebUI](https://github.com/open-webui/open-webui) gives you a ChatGPT-style chat interface for all your models. Point it at Ollama and LiteLLM and anyone on your network can use AI without a subscription.

### Why self-host a chat interface?

ChatGPT and Claude subscriptions cost $20/month per person. Open WebUI gives everyone on your network access to every model in your stack (local and cloud) through one interface, with no per-seat cost. Conversations stay on your hardware. You control which models are available, who has access, and what data stays private. It's also the easiest way to let non-technical users interact with your local models without touching a terminal.

- Connect to Ollama (local models) and LiteLLM (cloud models)
- Multi-user support with accounts and conversation history
- Document upload and RAG (retrieval-augmented generation)
- Model selection per conversation: pick the right model for the task
- Runs as a single Docker container

> ✓ Checkpoint: Open WebUI
>
> You should now be able to:
> - Provide a web-based chat interface for local (Ollama) and cloud (LiteLLM) models
> - Create accounts, save conversation history, and upload documents for RAG
>
> Test it:
> Deploy Open WebUI, connect to Ollama and LiteLLM, create an account, and chat with a local model. Upload a document and ask questions about it.
>
> Next unlock:
> Set up Perplexica for AI-powered research using your search and LLM stack.

## 12. Perplexica

[Perplexica](https://github.com/ItzCrazyKns/Perplexica) is a self-hosted alternative to Perplexity AI. If SearXNG is the search backend (raw results, JSON API), Perplexica is the research frontend: takes a question, searches via SearXNG, synthesizes an answer using your LLMs, and cites sources.

### Why self-host research?

Perplexity Pro costs $20/month and you're locked into their model choices. Perplexica uses your SearXNG instance for search and your LLM stack (via LiteLLM) for synthesis. Swap models anytime. No subscription, no query limits, no data leaving your network. It's also the layer where SearXNG and LiteLLM come together visibly: you can see exactly which search results the model used and how it synthesized the answer.

- Uses your SearXNG instance for web search (no API keys)
- Routes through your LLM stack via LiteLLM
- Multiple search modes: general, academic, writing, Wolfram Alpha
- Cited sources on every answer so you can verify claims

> ✓ Checkpoint: Perplexica
>
> You should now be able to:
> - Run AI-powered research queries that search the web and synthesize answers
> - Use your SearXNG instance and LLM stack without external subscriptions
>
> Test it:
> Deploy Perplexica, run a research query, and verify it pulls results from SearXNG, processes them with your LLM, and returns a synthesized answer with sources.
>
> Next unlock:
> Monitor your stack and make services accessible from anywhere.

## 13. Monitoring + Infrastructure

- [Uptime Kuma](https://github.com/louislam/uptime-kuma): monitor services, get alerts when something goes down
- [Caddy](https://caddyserver.com/): reverse proxy with automatic HTTPS
- [Tailscale](https://tailscale.com/): mesh VPN for secure remote access without port forwarding
- [Homepage](https://github.com/gethomepage/homepage)/[Homarr](https://github.com/homarr-labs/homarr): dashboard for everything at a glance

> ✓ Checkpoint: Monitoring + Infrastructure
>
> You should now be able to:
> - Monitor service uptime with Uptime Kuma and get alerts when things break
> - Access your stack securely from anywhere with Tailscale (VPN mesh network)
>
> Test it:
> Deploy Uptime Kuma, add monitors for 3-5 services, and verify status checks pass. Set up Tailscale, connect from your phone or another device, and access a service.
>
> Next unlock:
> The stack is complete. Start building automations, explore evaluation frameworks and vector databases, or just keep going.

---

## What You've Built

If you've worked through this entire playbook, you now have an AI coding assistant, specialized agents, MCP integrations, a Linux server running Docker, a unified API gateway, local LLMs, private search, workflow automation, a chat interface, a research engine, and monitoring to keep it all running.

### How the pieces connect

```
You (laptop)
 │
 ├── Claude Code ────── AI coding assistant
 │    ├── Agents ────── Specialized AI assistants (infra, webapps, MCP, career)
 │    └── MCP ─────── GitHub, Portainer, n8n, Plex, your custom servers
 │
 └── Linux Box ─────── Always-on server running Docker
      │
      ├── LiteLLM ────── API gateway (one endpoint, any model)
      │    ├── Cloud ──── OpenAI, Anthropic, Gemini, Groq
      │    └── Local ──── Ollama (Llama, Mistral, DeepSeek, Qwen)
      │
      ├── n8n ──────────── Workflow automation
      │    └── Uses ────── LiteLLM (models), SearXNG (search), your APIs
      │
      ├── SearXNG ────── Search backend (JSON API, no AI)
      │    └── Feeds ──── n8n workflows, Perplexica, custom tools
      │
      ├── Perplexica ──── Research frontend (search + AI synthesis)
      │    └── Uses ────── SearXNG (search) + LiteLLM (models)
      │
      ├── Open WebUI ──── Chat interface for everyone
      │    └── Uses ────── Ollama (local) + LiteLLM (cloud)
      │
      └── Monitoring ──── Uptime Kuma, Caddy, Tailscale
```

Every line represents a real connection running on real hardware.

---

## Appendix A: Prompts and Templates

Copy-paste-ready templates.

### Claude Code Starter Prompts

```
Help me build a task tracker web app with:
- Python/FastAPI backend, SQLite database, vanilla JS frontend, Docker deployment
```

```
Review this code for security issues. Check for SQL injection, XSS, authentication bypasses, and API key exposure.
```

```
Set up Portainer in Docker on my server at 192.168.1.100. Use docker-compose, expose on port 9000, configure with a volume for persistent data.
```

```
Explain how the authentication flow works in this codebase. Trace the request from login to session creation.
```

### Agent Skill File Template

Save as `.claude/commands/your-agent.md`:

```markdown
---
name: YourAgent
description: Brief description of what this agent does
---

# YourAgent

Trigger Phrase: "Your trigger phrase here..."

Context Doc: `path/to/YOUR-AGENT.md`

## Agent Identity

You are YourAgent, a specialized AI assistant for [specific domain].

Your role: [What this agent does]

Core principles:
- Principle 1
- Principle 2

## Startup Sequence

1. Read context document
2. Check current state
3. Announce readiness

## Tools and Capabilities

MCP Tools Available:
- `mcp__service__tool`: what it does

File Access:
- Config: `path/to/configs/`

## Task Patterns

### Pattern 1: Common Task
When: User asks for X
Steps: 1, 2, 3
Output: What to return
```

### CLAUDE.md Starter Template

Save as `CLAUDE.md` in your project root:

```markdown
# Claude Memory

## Project Overview

What this is: [Project description]
Tech stack: [Languages, frameworks]

## Agent Routing

| Trigger Topics | Trigger Phrase | Skill |
|---|---|---|
| [Topic] | "Phrase..." | `skill-name` |

## Workflow Principles

- Plan Before Building
- Explain Before Acting
- Verify Before Done

## Project Rules

- [Rule 1]
- [Rule 2]

## Security Rules

- Never commit API keys or passwords
```

### Session Resume Template

Save as `SESSION-RESUME.md` in your project:

```markdown
# Session Resume: [Project Name]

Status: [Active/Deployed/Paused]
Stack: [Backend] / [Frontend] / [Database]
Deploy: [Server, port] / `docker compose up -d --build`

## What Works
- Feature 1

## In Progress
- Current task

## Known Issues
- Issue: Description / Workaround

## Next Steps
1. Next task
```

---

## Appendix B: Case Study — Building Tank, the Homelab Sysadmin Agent

A concrete example of agents + MCP + SSH working together as a system.

### The Problem

You have a Linux box running Docker containers. When something breaks, you SSH in manually, run `docker ps`, check logs, restart containers, and troubleshoot. It works, but it's slow and repetitive.

### The Solution: Tank

Tank is a specialized agent (subagent inside Claude Code) that manages homelab infrastructure. It has SSH access to your Linux box, understands Docker and Portainer, and can check system health, view logs, deploy containers, and troubleshoot issues. It spawns when you ask homelab-related questions, reads a context document with your infrastructure layout, and uses Portainer MCP tools or SSH as needed.

### How to Build Tank

Step 1: Describe what you want

```
I want to build a homelab infrastructure agent called Tank. It should:
- Manage Docker containers on my Linux server
- Check system health (disk, memory, containers)
- View logs and troubleshoot issues
- Have SSH access to my server at 192.168.1.100
- Use Portainer MCP tools when possible
- Be invoked with the trigger phrase "Checking with Tank...stand by"

Create the skill file, context document, and add routing to my CLAUDE.md.
```

Step 2: Iterate on the agent

Claude will generate the files and ask clarifying questions. Review and refine:

```
Update Tank's context doc to include my NAS at 192.168.1.200 and add a common task for checking SABnzbd status.
```

```
Add a security constraint: Tank should never run docker system prune without explicit confirmation.
```

Step 3: Test and expand

Start using Tank and add capabilities as you need them. The agent evolves with your needs.

Step 4: Set up SSH access

Ask Claude Code to help you configure SSH:

```
I need to give you SSH access to my server at 192.168.1.100. Generate an ed25519 key, show me how to add it to the server, and configure ~/.ssh/config so you can connect easily.
```

Step 5: Register Portainer MCP

Ask Claude to help register Portainer MCP (API token generation and `claude mcp add` syntax).

### Security Model

Giving an AI assistant SSH access to your infrastructure requires care. Here's how to do it safely.

What Tank CAN do:
- Read-only system checks (`df -h`, `docker ps`, `systemctl status`)
- View logs (`docker logs`, `journalctl`)
- Restart containers via Portainer MCP (user approval required)
- Deploy containers from git repos (user approval required)

What Tank CANNOT do without explicit confirmation:
- Destructive operations (`rm -rf`, `docker system prune`)
- Modify production data
- Change firewall rules or networking
- Install system packages or update the OS

How it's enforced:

1. Dedicated SSH key: Revoke one key if compromised, not your entire system.
2. User approval prompts: Claude Code asks permission before running commands. Risky operations always require approval.
3. Sudo restrictions: Grant passwordless sudo only for specific commands:
   ```bash
   # /etc/sudoers.d/claude-homelab
   your-username ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart *
   your-username ALL=(ALL) NOPASSWD: /usr/bin/docker restart *
   ```
4. Prefer MCP over SSH: Portainer MCP has structured permissions. Use it first, SSH when needed.
5. Logging: All SSH commands appear in shell history and system logs.

### Real Examples

"Is my server healthy?" → Tank SSHs in, runs `df -h`, `docker ps`, `uptime`, checks for unhealthy containers, reports back: "Disk usage 42%, all 8 containers running, uptime 23 days."

"My web app isn't loading" → Tank checks container status via Portainer MCP, reads logs, identifies a port conflict, suggests the fix.

"Deploy Uptime Kuma on my server" → Tank SSHs in, creates the directory, writes `docker-compose.yml`, runs `docker compose up -d`, verifies, reports the URL.

All of this happens in one conversation. The value is in the conversational interface and context awareness: the agent knows your infrastructure layout, your conventions, and your stack.

Next step: Build your own infrastructure agent. Start simple (health checks and log viewing), then expand as you get comfortable.

---

## Vocabulary

AI-specific terms used in this playbook. Standard infrastructure terms (Docker, SSH, webhooks, reverse proxy) are not covered here.

| Term | What It Means |
|------|---------------|
| LLM | Large Language Model. The AI models that power ChatGPT, Claude, Llama, etc. |
| Agent | An AI assistant with a specific role, its own context, and access to tools. |
| Subagent | An agent spawned by another agent to handle a specific subtask. |
| MCP | Model Context Protocol. A standard that lets AI tools call external services directly. |
| SSE | Server-Sent Events. A transport method for MCP servers that works over the network. |
| RAG | Retrieval-Augmented Generation. Feeding documents to an LLM so it can answer questions about them. |
| Prompt injection | A security risk where malicious text tricks an AI into doing something unintended. |
| Context window | The amount of text an LLM can process in a single conversation. Measured in tokens. |
| Token | A chunk of text (roughly a word or part of a word) that LLMs process. |
| Inference | Running a prompt through a model and getting a response. |
| Embedding | A numerical representation of text that captures meaning. Used for search and similarity. |

---

## Up Next

Things I haven't built yet but plan to explore.

- Evaluation frameworks. Tools like [Braintrust](https://www.braintrust.dev/) and [promptfoo](https://www.promptfoo.dev/) for systematic prompt and model evaluation. Right now I'm comparing models by feel. Structured evals would let me measure quality, cost, and latency across providers and make data-driven routing decisions in LiteLLM.
- Vector databases. [Chroma](https://www.trychroma.com/), [Pinecone](https://www.pinecone.io/), or [pgvector](https://github.com/pgvector/pgvector) for scaling RAG beyond n8n's built-in vector store nodes. The goal is a persistent knowledge base that agents and workflows can query across sessions.
- Multi-agent orchestration. Running agents that coordinate with each other, not just with me. Claude Code supports [teams](https://code.claude.com/docs/en/sub-agents) of agents working in parallel on shared task lists. The next step is having the infra agent and webapp agent collaborate on deploys without me as the relay.
- Fine-tuning and adapters. Training small models on domain-specific data using [Unsloth](https://github.com/unslothai/unsloth) or [Axolotl](https://github.com/axolotl-ai-cloud/axolotl). A fine-tuned 7B model that knows your infrastructure might outperform a general 70B model for routine tasks.

---

## About

Built and maintained by [pete-builds](https://github.com/pete-builds). This playbook reflects a real stack running on a home network. Do your own research, dig into the docs, and make it yours.

Suggestions and corrections welcome via [Issues](../../issues).
