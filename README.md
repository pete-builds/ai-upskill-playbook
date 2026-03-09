# AI Upskill Playbook

*By [pete-builds](https://github.com/pete-builds) · Last updated March 2026*

**What I learned building a self-hosted AI stack from scratch, one layer at a time.**

Over the past few months I've been building a self-hosted AI stack from scratch, one service at a time, trying to understand how the pieces connect. Each section below represents a real layer in that stack today.

There are many ways to upskill in AI. This is simply the path I took. If you're trying to break into the field, support a business, or level up in your current role, this might give you a useful starting point. It will probably feel most natural if you come from an IT background, but anyone curious and willing to learn can follow along.

This isn't meant to be comprehensive. I'm still learning and there's a lot more to explore. What you see here is everything I've built so far, and I'll keep updating it as the stack evolves.

### What you'll end up with

```
You (laptop)
 │
 ├── Claude Code ──── AI coding assistant in your terminal
 │    ├── Agents ──── Specialized AI assistants (infra, webapps, MCP, career)
 │    └── MCP ─────── Direct access to GitHub, Portainer, n8n, Plex, etc.
 │
 └── Linux Box ────── Self-hosted AI business infrastructure
      │
      ├── LiteLLM ──── Unified API gateway (one endpoint, any model)
      │    ├── Cloud ── OpenAI, Anthropic, Gemini
      │    └── Local ── Ollama (Llama, Mistral, DeepSeek, Qwen)
      │
      ├── n8n ──────── Workflow automation (RAG, webhooks, scheduled jobs)
      ├── SearXNG ──── Private web search for AI tools
      ├── Open WebUI ── Chat interface for all models
      ├── Perplexica ── AI-powered research engine
      ├── Your Apps ─── Webapps you build and deploy
      └── Monitoring ── Uptime Kuma, Watchtower, Caddy, Tailscale
```

### What you'll learn

By the end of this playbook, you'll understand how to:

- Set up an AI-native development environment with an AI coding assistant
- Build agentic workflows that delegate tasks to specialized AI agents
- Give your AI tools direct access to services via MCP servers
- Run always-on infrastructure and containers on your own hardware
- Deploy and manage local LLMs on consumer hardware
- Route multiple AI providers through a single API gateway
- Automate workflows connecting AI models, APIs, and databases
- Ship real web applications and monitor your entire stack

---

## The Stack

### Part I: Get Going

Everything in this section runs on your laptop. No server, no hardware, no infrastructure. Just install Claude Code and start building.

| # | Layer | What | Why |
|---|-------|------|-----|
| 1 | [Claude Code](#1-claude-code) | AI-powered CLI dev environment | Your building speed multiplier. Everything else gets easier with this. |
| 2 | [Agentic Workflows](#2-agentic-workflows) | Specialized AI agents with routing and memory | Stop prompting, start delegating |
| 3 | [MCP — Connecting Tools](#3-mcp--connecting-tools) | Give Claude direct access to services | Cloud-hosted integrations, no server needed |

### Part II: Self-Hosted AI Business Infrastructure

This is where you build production-grade tooling. A dedicated Linux box running Docker becomes your AI operations platform: the same stack businesses use to deliver automations, control costs, and ship products. Run it in production or use it as a learning sandbox to gain hands-on experience with the tools teams deploy at scale.

| # | Layer | What | Why |
|---|-------|------|-----|
| 4 | [Linux Box](#4-linux-box) | A dedicated machine running Linux | Your always-on platform for everything below |
| 5 | [Docker + Portainer](#5-docker--portainer) | Containers and a management UI | Install anything without breaking everything |
| 6 | [MCP — Building Your Own](#6-mcp--building-your-own) | Build custom MCP tools | Give Claude direct access to your homelab services |
| 7 | [LiteLLM](#7-litellm) | Unified API gateway for LLMs | One endpoint, any model |
| 8 | [Local LLMs](#8-local-llms) | Ollama on your Mac, PC, or GPU box | Run models with zero API costs |
| 9 | [SearXNG](#9-searxng) | Private metasearch engine | Give your AI tools access to the web |
| 10 | [n8n](#10-n8n) | Workflow automation platform | Connect everything to everything |
| 11 | [Open WebUI](#11-open-webui) | Chat interface for local + remote models | A front door for everyone else |
| 12 | [Perplexica](#12-perplexica) | AI-powered search (self-hosted Perplexity) | Deep research without subscriptions |
| 13 | [Build a Web App](#13-build-a-web-app) | Ship a real project with your AI coding assistant | Prove the stack works by building something |
| 14 | [Monitoring + Infrastructure](#14-monitoring--infrastructure) | Uptime Kuma, Watchtower, Caddy, Tailscale | Keep it all running and reachable |

New to some of these terms? See the [Vocabulary](#vocabulary) at the bottom.

---

## Getting Started

You don't need a server to get started. Layers 1-3 run entirely on your laptop and will already change how you work. Once you're ready to go deeper, Part II walks you through setting up your own infrastructure.

The real unlock is **[Claude Code](https://code.claude.com/docs/en/overview) + agents + [MCP servers](https://code.claude.com/docs/en/mcp)**. Once you have those three, you're not just using AI. You're building with it.

---

## 1. Claude Code

[Claude Code](https://code.claude.com/docs/en/overview) is an AI coding assistant that runs in your terminal. It can read your files, write code, run commands, and iterate on problems with you. This is layer one because it accelerates everything that comes after. You're not just following tutorials anymore. You have a coding assistant that knows the docs.

- Get the [$20/month Claude Pro subscription](https://claude.com/pricing). I know, nobody wants another subscription. Don't let that be the thing that stops you. The value you get back in productivity is not even close. This is the highest-leverage $20 you'll spend.
- Install [Claude Code](https://code.claude.com/docs/en/overview) in your terminal, VS Code, or whatever IDE you prefer (npm, requires Node.js)
- Connect to Anthropic API (direct key or API gateway)
- Learn the core loop: describe what you want, review what it does, iterate
- **Use Sonnet for everyday tasks** (quick edits, file searches, simple scripts). **Switch to Opus** (`/model opus`) for complex code, architecture decisions, debugging, and writing. Opus thinks deeper but costs more context.
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

**Security note:** Claude Code can run commands on your machine. Be aware of [prompt injection](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/mitigate-jailbreaks) risks: malicious content in files, repos, or web pages could try to trick Claude into running harmful commands. Review what Claude is doing before approving tool calls, especially with unfamiliar code. Never blindly consume unofficial code from repos you haven't reviewed, including the ones linked in this playbook. Read it first.

> **✓ Checkpoint: Claude Code**
>
> **You should now be able to:**
> - Edit code, run commands, and iterate on problems without leaving your terminal
> - Switch models (`/model opus` for deep work, back to Sonnet for quick tasks)
>
> **Test it:**
> Run `claude` in a project directory. Ask it to read a file, explain what it does, and make a small improvement. Review the changes and approve them.
>
> **Next unlock:**
> Build specialized agents that delegate work instead of handling everything in one conversation.

## 2. Agentic Workflows

This is where Claude Code goes from "smart assistant" to "team of specialists." Instead of one conversation handling everything, you build purpose-built agents that each own a domain. No server needed. This all runs on your laptop. See the official docs on [custom subagents](https://code.claude.com/docs/en/sub-agents) and [skills](https://code.claude.com/docs/en/skills).

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

There are two ways to build an agent. Start with the easy way.

**The easy way: just ask Claude Code to build it for you.**

Tell Claude what you want the agent to do, what domain it covers, and what tools it should have access to. Claude will ask you clarifying questions, then create the skill file, write the context doc, and update your `CLAUDE.md` routing table. You don't need to know the file structure or syntax. Just describe what you need and iterate.

```
I want to build an agent called Tank that manages my homelab infrastructure.
It should check container health, view logs, and deploy services on my
Linux server. Create everything it needs to work.
```

Claude handles the rest. It creates `.claude/commands/tank.md`, writes a context doc, adds the routing entry, and sets up a trigger phrase. You review what it built, test it, and refine from there.

**The manual way: build each piece yourself.**

If you want to understand what's under the hood, or if you prefer to write your own files:

- Create a skill file in `.claude/commands/your-agent.md` (see [skills docs](https://code.claude.com/docs/en/skills))
- Write a context doc with system knowledge, SOPs, and constraints
- Define a trigger phrase and startup sequence
- Add it to the routing table in `CLAUDE.md`
- The agent reads its context doc every time it's invoked, so it stays current

See [Appendix A](#appendix-a-prompts-and-templates) for copy-paste templates of each file.

### Workflow Principles

Bake these into every agent. Tell Claude to add them to each agent's skill file, or add them manually.

- **Plan before building:** for 3+ step tasks, outline the plan first
- **Explain before acting:** say what's about to happen before running tools
- **Verify before done:** run it, check the output, confirm it works
- **Self-improvement loop:** log corrections in a lessons file, review at session start

> **✓ Checkpoint: Agentic Workflows**
>
> **You should now be able to:**
> - Route topics to specialized agents using a trigger phrase and skill invocation
> - Isolate context so each agent only reads what it needs
>
> **Test it:**
> Create a simple agent with a skill file in `.claude/commands/`, a context doc, and an entry in your `CLAUDE.md` routing table. Invoke it with the trigger phrase and verify it reads its context.
>
> **Next unlock:**
> Give your agents direct access to external services via cloud-hosted MCP servers.

## 3. MCP — Connecting Tools

[Model Context Protocol (MCP)](https://code.claude.com/docs/en/mcp) servers give Claude Code direct access to external services. Instead of copy-pasting between terminals, you just ask Claude to do it and it calls the API directly.

You don't need a server to use MCP. Many MCP servers are cloud-hosted or run locally on your laptop. Start with these:

- **[GitHub MCP](https://github.com/github/github-mcp-server):** PRs, issues, code search, repo management
- **[Google Calendar](https://console.cloud.google.com/):** schedule, check availability, create events
- **[Gmail](https://console.cloud.google.com/):** read, search, draft emails
- **[Anthropic's built-in connectors](https://code.claude.com/docs/en/mcp):** many integrations available out of the box

Each MCP server = a set of tools Claude can call on demand. Register them with `claude mcp add` and they're available in every conversation. See the [MCP server registry](https://registry.modelcontextprotocol.io/) for more pre-built servers.

**What is MCP?** A protocol that lets AI assistants call external tools via structured API. Once you have a Linux box (Part II), you can also [build and self-host your own](#6-mcp--building-your-own).

> **✓ Checkpoint: MCP — Connecting Tools**
>
> **You should now be able to:**
> - Register cloud-hosted MCP servers (GitHub, Gmail, Calendar) with `claude mcp add`
> - Ask Claude to interact with those services directly (read GitHub issues, check your calendar)
>
> **Test it:**
> Register GitHub MCP, then ask Claude to list your recent pull requests or search code in a repo. Verify it calls the API instead of you copy-pasting terminal output.
>
> **Next unlock:**
> Set up a Linux box to self-host infrastructure and run your own services.

**Need templates?** See [Appendix A: Prompts and Templates](#appendix-a-prompts-and-templates) for copy-paste-ready examples.

---

## Part II: Self-Hosted Infrastructure

This is where you build your AI business stack. The infrastructure in Part II isn't just for experimentation. These are production-grade tools used by businesses to deliver AI-powered automations, control costs, and ship real products. LiteLLM gives you the same team management and cost tracking that enterprises use to govern AI spending. n8n powers mission-critical workflows in companies worldwide. Local models let you cut API costs to near zero for high-volume tasks. Together, this stack becomes your AI operations platform: a toolkit for building automations that save time and money for you, your customers, and stakeholders.

If you're looking to work in AI in 2026, this is the environment you'd be expected to understand, build in, and ship from. Docker, API gateways, workflow automation, local models, and AI-assisted development are showing up in job descriptions and team stacks as baseline expectations. Even if you never run production services, having this stack in your own sandbox gives you hands-on experience with the tools and language that teams use at scale. That positions you to consult, build for clients, or step into roles that require this expertise.

## 4. Linux Box

Ready to go deeper? Everything from here on runs on a server. That server can be anything: a $5/month cloud VPS, a five-year-old PC from your closet, a used mini PC, or a NUC. If it turns on and stays on, it works. I'm using a small form factor PC with a Synology NAS for backups.

**Never installed Linux?** That's fine. Ask Claude Code: "Help me install Ubuntu Server headless on a PC. Walk me through it step by step." It will guide you through creating a bootable USB, the installer options, and initial setup. You don't need to know Linux to start. You just need to be willing to follow the steps.

A cloud VPS (DigitalOcean, Hetzner, Linode) works too if you don't want hardware at home. You'll skip the physical setup and go straight to SSH. Everything else in this playbook works the same either way.

- Any x86 machine with 8GB RAM and a 256GB SSD, or a cloud VPS
- [Ubuntu Server](https://ubuntu.com/download/server) or Debian (headless, no desktop environment needed)
- SSH access configured (key-based, no passwords)
- Static IP or DHCP reservation on your router (or use the VPS IP)
- A NAS or external drive for backups (optional but smart)
- [Tailscale](https://tailscale.com/) for remote access without port forwarding
- Give Claude Code SSH access to your Linux box (key-based). This lets Claude deploy containers, check logs, and manage services directly from your laptop. It's a game changer.
- This is your foundation. Everything else is a container on this box.

### Build Your Sysadmin Agent

Every good business needs a sysadmin. Do you know how to manage a Linux server, troubleshoot Docker containers, and keep services running? Not yet, you don't. But instead of hiring a sysadmin or asking Claude manually every time something breaks, you're going to build a sysadmin agent. This is one of the most important steps in the entire playbook because it cuts the time it takes to manage your infrastructure to a fraction of what it would normally be.

Go back to [Building Your First Agent](#building-your-first-agent) and create an agent that manages your Linux box. Give it SSH access, tell it about your services, and let it handle health checks, log viewing, container restarts, and deployments. You built the agent framework in Part I. Now you give it a real job.

**Do it responsibly.** An AI agent with SSH access to your server is powerful, but it can also break things. Use key-based auth with a dedicated key you can revoke. Restrict sudo to specific commands. Prefer MCP tools (like Portainer) over raw SSH when possible, since APIs have structured permissions. Always review what the agent is doing before approving destructive operations. See [Appendix B: Building Tank](#appendix-b-case-study--building-tank-the-homelab-sysadmin-agent) for a full walkthrough of how to build a sysadmin agent safely.

> **✓ Checkpoint: Linux Box**
>
> **You should now be able to:**
> - SSH into a dedicated Linux machine with key-based authentication
> - Give Claude Code SSH access so it can deploy and manage services remotely
>
> **Test it:**
> From your laptop, ask Claude to SSH into your Linux box, check disk space, and show running processes. Verify the output.
>
> **Next unlock:**
> Install Docker and Portainer to run containers instead of installing everything bare-metal.

## 5. Docker + Portainer

[Docker](https://docs.docker.com/engine/install/) lets you run applications in isolated containers. [Portainer](https://www.portainer.io/) gives you a web UI to manage them instead of memorizing CLI commands.

- Install [Docker Engine](https://docs.docker.com/engine/install/) (not Docker Desktop)
- Install [Portainer CE](https://docs.portainer.io/start/install-ce) as your first container
- Learn [docker-compose](https://docs.docker.com/compose/): one YAML file per service
- Understand volumes (persistent data) vs bind mounts
- Understand networking: bridge, host, and container DNS
- [Watchtower](https://github.com/containrrr/watchtower) for automatic image updates (note: archived Dec 2025, no longer maintained - use with caution)
- Once you have these two, installing new services goes from "follow a 47-step guide" to "paste a docker-compose file and click deploy"

> **✓ Checkpoint: Docker + Portainer**
>
> **You should now be able to:**
> - Deploy containers from docker-compose files via Portainer's web UI
> - Understand volumes (persistent data) and container networking
>
> **Test it:**
> Deploy a simple service (nginx, hello-world) using a docker-compose file. Check logs, restart the container, and verify data persists across restarts.
>
> **Next unlock:**
> Build your own MCP servers to give Claude direct access to your homelab services.

## 6. MCP — Building Your Own

Once you have a Linux box and Docker, you can build your own MCP servers and give Claude Code direct access to your homelab services. This is where MCP goes from "connect to cloud tools" to "control your entire infrastructure."

- **Build pattern:** Python + [FastMCP](https://github.com/PrefectHQ/fastmcp) + httpx + Docker
- **Transports:** SSE (network, accessible from any machine) vs stdio (local binary)
- **Deploy as containers** on your Linux box, register with `claude mcp add`
- **Example servers you can build:**
  - Infrastructure management ([Portainer MCP](https://github.com/portainer/portainer-mcp))
  - Workflow execution (n8n)
  - Home automation, DNS, network tools
  - Media management, monitoring, and more
- SSE transport recommended. One server, accessible from any machine on your network.

> **✓ Checkpoint: MCP — Building Your Own**
>
> **You should now be able to:**
> - Build a custom MCP server using FastMCP + Docker + SSE transport
> - Register it with Claude Code and call its tools from any conversation
>
> **Test it:**
> Build a simple MCP server with 1-2 tools (e.g., check container status, read a file). Deploy as a container, register with `claude mcp add`, and invoke a tool.
>
> **Next unlock:**
> Set up LiteLLM to route between cloud APIs and local models from one endpoint.

## 7. LiteLLM

[LiteLLM](https://github.com/BerriAI/litellm) sits between your apps and AI model providers. Point everything at one URL, and LiteLLM routes to OpenAI, Anthropic, local models, or whatever you configure.

That's the surface-level pitch. But the real value isn't just the unified API. It's the organizational infrastructure that comes with it. LiteLLM turns AI model access from "everyone has their own API keys" into a managed service you control. Track costs, enforce limits, route intelligently, and change backends without breaking anything.

### Core Features

**Unified API**: OpenAI-compatible endpoint for 100+ model providers. One endpoint, swap models without changing client code.

**Cost Tracking**: Log every request and calculate costs in real time across all providers. Know exactly what each user, team, or workflow is spending.

**Fallback Routing**: Try one model, automatically retry with another if it fails. Chain free → local → paid for maximum efficiency.

**Team Key Management**: Issue virtual keys instead of distributing real API keys. Track attribution, enforce budgets, revoke access in one place.

**Rate Limiting**: Cap requests per minute/hour to prevent runaway costs from broken workflows.

**Model Aliasing**: Give models friendly names (`fast`, `smart`). Change the backend without changing client code.

### Why This Matters for Teams

**Before LiteLLM**: Every person gets their own API key, or everyone shares one key. No central visibility, no cost control, billing chaos.

**With LiteLLM**: One master key on the gateway. Virtual keys for every user, team, or service. All usage tracked and attributed centrally. Set budgets, enforce limits, swap providers transparently.

### Key Use Cases

**Budget Control**: Set spending limits per user or team. Get alerts before hitting your credit card limit.

**Intelligent Fallback**: Try free models first (Groq, Gemini), fall back to local models (Ollama), then paid models (Claude, GPT-4) only when needed. 99% of requests cost nothing.

**Model Testing**: A/B test new models by routing 10% of traffic to them. Compare quality and cost before switching everyone over.

**Prevent Runaway Costs**: Rate limit workflows. A broken script can't burn through $500 before you notice.

**Zero Vendor Lock-In**: Abstract away provider details. Clients call `smart`, you decide whether that's GPT-4o, Claude Opus, or Llama 70B. Switch anytime.

> **✓ Checkpoint: LiteLLM**
>
> **You should now be able to:**
> - Point apps at LiteLLM's unified endpoint and route to OpenAI, Anthropic, or Ollama
> - Track usage and costs across all providers
>
> **Test it:**
> Deploy LiteLLM, configure one cloud provider and one local Ollama model. Send a request to each via LiteLLM's OpenAI-compatible API and verify responses.
>
> **Next unlock:**
> Run local models with Ollama for free, private inference.

## 8. Local LLMs

Let's be honest: a local model running on consumer hardware won't come close to the latest Claude or GPT for complex reasoning, code generation, or deep analysis. That's not the point. Running at least one local model is still worth doing, and here's why.

**It's free.** Once the model is downloaded, every request costs nothing. For high-volume tasks like summarization, classification, embeddings, or bulk processing, that matters. A broken n8n workflow that fires 10,000 requests overnight costs you $0 against a local model.

**It's private.** Nothing leaves your network. Sensitive data, internal documents, customer information: process it locally and it never touches a third-party API.

**It's always available.** No rate limits, no outages, no API keys expiring at 2 AM. Your local model is up when your server is up.

**It's a fallback.** Register local models with LiteLLM and they become the last resort in your fallback chain. Cloud API down? Local model picks up the request automatically.

**It's a learning experience.** Understanding how models run, what hardware they need, how quantization affects quality, and how inference speed scales with model size teaches you things you can't learn from API calls alone. If you're building a career in AI infrastructure, you need to understand what's happening on the other side of the API.

### Getting started

Install [Ollama](https://ollama.com/) on any Mac, Linux box, or Windows PC with a GPU. It's a one-line install on Mac and Linux. Ollama handles downloading, managing, and serving models. Pull a model, run it, done.

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

**Apple Silicon** (M1/M2/M3/M4) uses unified memory, which means the GPU shares your system RAM. A Mac Mini with 16GB can comfortably run 7-8B models. 32GB opens up the 13-14B range. This is surprisingly good for the price.

**NVIDIA GPUs** use dedicated VRAM. An RTX 3060 (12GB VRAM) handles 7B models well. An RTX 3090 or 4090 (24GB VRAM) gives you room for larger models.

**Quantization** compresses models to use less memory at a small quality cost. A 70B model that normally needs 140GB of memory can run in ~40GB when quantized to 4-bit. Ollama handles this automatically with different model tags (e.g., `llama3.1:70b-q4_0`).

Start with a 7-8B model. It's the sweet spot for most consumer hardware: fast enough to be useful, smart enough for most automation tasks, and small enough to leave room for other services.

- [Ollama](https://ollama.com/) for model management and inference
- Models: [Llama](https://ollama.com/library/llama3.2), [Mistral](https://ollama.com/library/mistral), [Qwen](https://ollama.com/library/qwen2.5), [DeepSeek](https://ollama.com/library/deepseek-r1), [Gemma](https://ollama.com/library/gemma2), and more
- Register with LiteLLM so all your tools can use them
- Run coding assistants, chat models, and embedding models side by side

> **✓ Checkpoint: Local Models**
>
> **You should now be able to:**
> - Run open-source LLMs locally with Ollama (Llama, Mistral, Qwen, DeepSeek)
> - Register them with LiteLLM so all your tools can access them
>
> **Test it:**
> Pull a model with `ollama pull`, run a prompt with `ollama run`, then query the same model through LiteLLM's API. Compare speed and quality to cloud models.
>
> **Next unlock:**
> Deploy SearXNG to give your AI tools private web search capability.

## 9. SearXNG

[SearXNG](https://github.com/searxng/searxng) is a private metasearch engine that aggregates results from Google, Bing, DuckDuckGo, and dozens of other sources without tracking you.

**Where it fits in the stack:** SearXNG is the search backend for your entire AI infrastructure. It doesn't use AI itself. It returns raw search results via a JSON API that other tools consume. Think of it as the "eyes" of your stack: n8n workflows query it for web data, Perplexica uses it as its search engine, and any custom tool you build can hit the API for real-time web results. Without it, your AI tools are blind to the internet.

- Self-hosted, no API keys needed
- Aggregates results from 70+ search engines
- JSON API for programmatic access
- Feed it to n8n workflows, LLM agents, and Perplexica
- Gives your AI tools and workflows a way to search the web
- Privacy-first: no tracking, no profiling, no ads

> **✓ Checkpoint: SearXNG**
>
> **You should now be able to:**
> - Search the web via SearXNG's JSON API without tracking or API keys
> - Point AI tools and workflows at your self-hosted search instance
>
> **Test it:**
> Deploy SearXNG, run a search query via the web UI, then hit the JSON API endpoint with curl or your browser. Verify results from multiple engines.
>
> **Next unlock:**
> Build automated workflows in n8n that connect AI, search, APIs, and triggers.

## 10. n8n

[n8n](https://n8n.io/) is a workflow automation platform. Think Zapier, but self-hosted and free. Drag and drop nodes to connect APIs, databases, AI models, and triggers.

This is where your stack starts solving real problems for real people. Everything you've built so far (LiteLLM, local models, SearXNG, MCP) becomes the backend for automations that save time and money. n8n is how you ship those automations as products.

**Why n8n matters for your business.** If you're consulting, freelancing, or building AI services for clients, n8n is how you deliver. A client says "I need my support emails triaged automatically" or "I want a daily report from three data sources." You don't write a custom app. You build an n8n workflow, connect it to their systems, and it runs. That's a billable deliverable you can build in hours, not weeks. Learning n8n turns your AI stack into a service you can sell.

**Real problems n8n solves:**
- A small business wants AI to categorize incoming leads and route hot ones to sales immediately
- A team needs daily summaries pulled from GitHub, Slack, and their project tracker
- A client wants their internal docs searchable by AI so new hires can self-serve answers
- A nonprofit needs donation receipts generated and emailed automatically
- You need your own infrastructure monitored with plain-English alerts instead of raw log output

Every one of these is an n8n workflow backed by the stack you've already built.

- Visual workflow builder (no code required, but code is available when you need it)
- AI agent nodes with tool calling and memory
- Webhook triggers for real-time integrations
- Schedule-based workflows for recurring tasks
- Built-in MCP server: expose workflows as tools for Claude Code
- Build RAG pipelines: feed documents to an LLM so it can answer questions about your own data
- Connect to LiteLLM, Ollama, SearXNG, and everything else in your stack

### n8n Workflow Examples

Here are five real workflows that show what you can build with n8n + LiteLLM. Each one demonstrates a different capability. Copy these patterns and adapt them to your needs.

#### 1. Webhook to AI Summary to Slack

**What it does:** Real-time alert processing. Any service sends a webhook with JSON data, an LLM summarizes it in plain English, result posts to Slack.

**Why it's useful:** Turn noisy system alerts into readable notifications. Monitoring tools, CI/CD pipelines, and application errors all speak JSON. Humans prefer sentences.

**Key nodes:**
- Webhook (trigger)
- HTTP Request to LiteLLM
- Slack (send message)

**How it connects to LiteLLM:**
```
POST http://your-litellm-server:4000/v1/chat/completions
Headers: Authorization: Bearer sk-litellm-master-key
Body:
{
  "model": "gemini/gemini-2.0-flash-exp",
  "messages": [
    {"role": "system", "content": "Summarize alerts in one clear sentence."},
    {"role": "user", "content": "{{ $json.alert_data }}"}
  ]
}
```

**What AI capability it demonstrates:** JSON-to-text transformation. The model reads structured data and explains what's happening.

**Practical use case:** Uptime Kuma sends a webhook when a service goes down. n8n catches it, sends the JSON to Gemini Flash (free via LiteLLM), posts "nix1 went offline at 2:34 PM, last seen 3 minutes ago" to your Slack channel.

#### 2. Daily Report Generator

**What it does:** Scheduled workflow that pulls data from multiple APIs (GitHub, Uptime Kuma, Portainer), sends it all to an LLM, generates a summary report, emails it to you.

**Why it's useful:** Replace manual status checks with automated morning briefings. Infrastructure status, active GitHub issues, container health, all in one readable email.

**Key nodes:**
- Schedule Trigger (cron: 0 8 * * *)
- HTTP Request x3 (GitHub API, Uptime Kuma API, Portainer API)
- HTTP Request to LiteLLM
- Send Email

**How it connects to LiteLLM:**
```
POST http://your-litellm-server:4000/v1/chat/completions
Body:
{
  "model": "gpt-4o-mini",
  "messages": [
    {"role": "system", "content": "You are a daily infrastructure reporter. Summarize the data below into a clear status report with bullet points."},
    {"role": "user", "content": "GitHub Issues: {{ $json.github }}\nUptime: {{ $json.uptime }}\nContainers: {{ $json.portainer }}"}
  ]
}
```

**What AI capability it demonstrates:** Multi-source data synthesis. The model reads from different systems and produces one coherent summary.

**Practical use case:** Every morning at 8 AM, you get an email titled "Daily Homelab Status" with sections for GitHub activity, service uptime, and container health. No manual checking required.

#### 3. RAG Pipeline (Searchable Knowledge Base)

**What it does:** Upload documents (PDFs, markdown, text files), extract and chunk the text, generate embeddings, store them in a vector database, query them with natural language questions.

**Why it's useful:** Make internal documentation, manuals, and notes searchable with AI. Ask "How do I reset the Portainer password?" and get the answer from your own docs.

**Key nodes:**
- Manual Trigger or Webhook (for document upload)
- Read Binary File
- Document Splitter
- Embeddings (OpenAI, Ollama, or via LiteLLM)
- Vector Store (Pinecone, Chroma, Qdrant, or Postgres with pgvector)
- AI Agent (for querying)

**How it connects to LiteLLM:**

For embeddings:
```
POST http://your-litellm-server:4000/v1/embeddings
Body:
{
  "model": "text-embedding-3-small",
  "input": "{{ $json.chunk }}"
}
```

For retrieval query:
```
1. Convert user question to embedding (same endpoint)
2. Search vector store for similar chunks
3. Send chunks + question to LLM:

POST http://your-litellm-server:4000/v1/chat/completions
Body:
{
  "model": "gpt-4o",
  "messages": [
    {"role": "system", "content": "Answer questions based only on the provided context."},
    {"role": "user", "content": "Context: {{ $json.retrieved_chunks }}\n\nQuestion: {{ $json.user_question }}"}
  ]
}
```

**What AI capability it demonstrates:** Retrieval-Augmented Generation. The model answers questions by referencing your actual documents instead of making things up.

**Practical use case:** Drop all your homelab setup notes into n8n. When you forget how you configured something six months ago, ask the RAG workflow instead of searching through files.

#### 4. Multi-Model Evaluation

**What it does:** Send the same prompt to three different models (e.g., GPT-4o, Claude Sonnet, Llama 3.2 local), collect all responses, compare outputs side by side, log to a spreadsheet.

**Why it's useful:** Figure out which model is best for your task. Test if a cheaper or local model can match the quality of expensive cloud models.

**Key nodes:**
- Manual Trigger or Webhook
- HTTP Request x3 (one per model, run in parallel)
- Merge (combine results)
- Google Sheets (log comparison)

**How it connects to LiteLLM:**

All three requests point to the same LiteLLM endpoint, just different models:

```
Request 1:
POST http://your-litellm-server:4000/v1/chat/completions
Body: { "model": "gpt-4o", "messages": [...] }

Request 2:
POST http://your-litellm-server:4000/v1/chat/completions
Body: { "model": "claude-sonnet-4", "messages": [...] }

Request 3:
POST http://your-litellm-server:4000/v1/chat/completions
Body: { "model": "ollama/llama3.2", "messages": [...] }
```

**What AI capability it demonstrates:** A/B/C testing for LLMs. Quantify quality, speed, and cost across models.

**Practical use case:** You're writing a script to summarize support tickets. Run 20 real tickets through GPT-4o, Claude Sonnet, and local Llama. Compare outputs. If Llama matches quality, switch to it and pay nothing.

#### 5. AI-Powered Triage System

**What it does:** Incoming requests (email, webhook, form submission) get sent to an LLM for categorization. Based on the category, route to different workflows: urgent to Slack, questions to support queue, bugs to GitHub issues.

**Why it's useful:** Automate first-level routing without writing classification rules. The LLM decides where things go.

**Key nodes:**
- Email Trigger or Webhook
- HTTP Request to LiteLLM (for classification)
- Switch (route based on category)
- Slack, GitHub, Email nodes (downstream actions)

**How it connects to LiteLLM:**

```
POST http://your-litellm-server:4000/v1/chat/completions
Body:
{
  "model": "gemini/gemini-2.0-flash-exp",
  "messages": [
    {"role": "system", "content": "Classify this request into one of: URGENT, QUESTION, BUG, FEATURE_REQUEST, SPAM. Return only the category."},
    {"role": "user", "content": "{{ $json.email_body }}"}
  ]
}
```

Parse the response (e.g., "URGENT"), then route:

```
Switch node:
- If category == "URGENT" → send to Slack
- If category == "QUESTION" → add to support queue
- If category == "BUG" → create GitHub issue
- If category == "SPAM" → archive
```

**What AI capability it demonstrates:** Zero-shot classification. No training data, no fine-tuning. Just tell the model what categories exist and it figures out which one fits.

**Practical use case:** Support inbox gets flooded. n8n intercepts every message, Claude or Gemini categorizes it, urgent issues hit Slack immediately, everything else gets filed correctly.

### Tips for Building n8n Workflows

- **Start with a webhook trigger.** Test manually before connecting real data sources.
- **Use LiteLLM for flexibility.** Swap models without rewriting workflows. Start with Gemini Flash (free), upgrade to GPT-4o if you need higher quality.
- **Log everything.** Add a "Write to Google Sheets" or "Append to File" node at the end so you can review what the workflow did.
- **Error handling matters.** Use n8n's error workflows to catch API failures and retry logic.
- **Keep prompts in one place.** Use n8n's "Set" node to define your system prompt as a variable at the top of the workflow. Makes it easy to iterate.
- **LiteLLM fallback groups.** Configure `smart` and `fast` groups in LiteLLM. If your primary model is down, the workflow keeps running.

These five workflows cover the most common patterns. Once you've built them, you'll understand how to connect n8n, LiteLLM, and your AI stack. Everything else is just variations.

> **✓ Checkpoint: n8n**
>
> **You should now be able to:**
> - Build visual workflows connecting AI models, APIs, webhooks, and schedules
> - Expose workflows as MCP tools for Claude Code to invoke
>
> **Test it:**
> Create a simple workflow with a webhook trigger and an LLM node. Send a request to the webhook URL and verify the workflow executes and returns a response.
>
> **Next unlock:**
> Deploy Open WebUI to give everyone a ChatGPT-style interface for your models.

## 11. Open WebUI

[Open WebUI](https://github.com/open-webui/open-webui) gives you a ChatGPT-style interface for all your models, local and remote. Point it at Ollama and LiteLLM and everyone in your house can use AI without a subscription.

- Web-based chat interface (looks and feels like ChatGPT)
- Connect to Ollama (local models) and LiteLLM (cloud models)
- Multi-user support with accounts
- Conversation history and search
- Document upload and RAG (retrieval-augmented generation)
- The "front door" layer. Everything you built powers what feels like a simple chat app.

> **✓ Checkpoint: Open WebUI**
>
> **You should now be able to:**
> - Provide a web-based chat interface for local (Ollama) and cloud (LiteLLM) models
> - Create accounts, save conversation history, and upload documents for RAG
>
> **Test it:**
> Deploy Open WebUI, connect to Ollama and LiteLLM, create an account, and chat with a local model. Upload a document and ask questions about it.
>
> **Next unlock:**
> Set up Perplexica for AI-powered research using your search and LLM stack.

## 12. Perplexica

[Perplexica](https://github.com/ItzCrazyKns/Perplexica) is a self-hosted alternative to Perplexity AI. It uses SearXNG for web search and your local/cloud models for synthesis.

**Where it fits in the stack:** If SearXNG is the search backend (raw results, JSON API, no AI), Perplexica is the research frontend (takes a question, searches via SearXNG, synthesizes an answer using your LLMs, cites sources). SearXNG serves n8n workflows and programmatic tools. Perplexica serves you when you want a researched answer. They work together: SearXNG does the searching, Perplexica does the thinking.

- AI-powered research and answer engine
- Uses your SearXNG instance for web search (no API keys)
- Routes through your LLM stack via LiteLLM (local or cloud models)
- Multiple search modes: general, academic, writing, Wolfram Alpha
- Self-hosted. Your searches stay on your network.

> **✓ Checkpoint: Perplexica**
>
> **You should now be able to:**
> - Run AI-powered research queries that search the web and synthesize answers
> - Use your SearXNG instance and LLM stack without external subscriptions
>
> **Test it:**
> Deploy Perplexica, run a research query, and verify it pulls results from SearXNG, processes them with your LLM, and returns a synthesized answer with sources.
>
> **Next unlock:**
> Build and deploy a real webapp with Claude Code from idea to production.

## 13. Build a Web App

The best way to prove your stack works is to build something real with it. Claude Code + Docker + your Linux box = you can go from idea to deployed webapp in one session.

### Build an agent first

Before you start coding, build a dedicated agent (see [Agentic Workflows](#2-agentic-workflows)) that owns the full lifecycle of your webapps. Give it a context doc with your deploy target, stack preferences, and the practices below. This keeps every project consistent and saves you from re-explaining the same things every session.

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

> **✓ Checkpoint: Build a Web App**
>
> **You should now be able to:**
> - Go from idea to deployed webapp in one session with Claude Code
> - Dockerize, push to GitHub, deploy via git pull, and expose via Tailscale Funnel
>
> **Test it:**
> Build a simple webapp (form submission, data display, API integration), containerize it, deploy to your Linux box, and verify it's accessible on your network. Push the code to GitHub.
>
> **Next unlock:**
> Monitor your stack and make services accessible from anywhere.

## 14. Monitoring + Infrastructure

Once you have services running, you need to keep them running and make them accessible.

- **[Uptime Kuma](https://github.com/louislam/uptime-kuma):** monitor all your services, get alerts when something goes down
- **[Watchtower](https://github.com/containrrr/watchtower):** automatically pull and redeploy updated container images (archived, no longer maintained)
- **[Caddy](https://caddyserver.com/):** reverse proxy with automatic HTTPS (TLS certificates)
- **[Tailscale](https://tailscale.com/):** mesh VPN for secure remote access without port forwarding
- **[Homepage](https://github.com/gethomepage/homepage)/[Homarr](https://github.com/homarr-labs/homarr):** dashboard to see everything at a glance

> **✓ Checkpoint: Monitoring + Infrastructure**
>
> **You should now be able to:**
> - Monitor service uptime with Uptime Kuma and get alerts when things break
> - Access your stack securely from anywhere with Tailscale (VPN mesh network)
>
> **Test it:**
> Deploy Uptime Kuma, add monitors for 3-5 services, and verify status checks pass. Set up Tailscale, connect from your phone or another device, and access a service.
>
> **Next unlock:**
> You've built a complete self-hosted AI stack. Start experimenting, build more apps, or explore evaluation frameworks and vector databases.

---

## What You've Built

If you've worked through this entire playbook, here's what you now have:

**An AI coding assistant** that reads your files, writes code, runs commands, deploys containers, and iterates with you in real time. Not autocomplete. A collaborator.

**A team of specialized agents** that each own a domain: infrastructure, webapps, career, configuration, architecture. You route tasks by topic and each agent carries its own context, tools, and memory. You're delegating, not prompting.

**Direct tool access via MCP** so your AI assistant can interact with GitHub, Google Calendar, Gmail, Portainer, n8n, Plex, and any service you build a connector for. No copy-paste. No switching windows.

**A dedicated Linux server** running Docker, managed through Portainer, accessible from anywhere via Tailscale. Your always-on platform for self-hosted services.

**Custom MCP servers** you built yourself, giving Claude Code direct control over your homelab infrastructure. SSE transport, containerized, registered with one command.

**A unified AI gateway (LiteLLM)** that sits between every tool and every model provider. One endpoint. Virtual keys for every user and workflow. Cost tracking, budget enforcement, fallback routing, model aliasing. You control who uses what, how much they spend, and which models serve which requests.

**Local LLMs running on your own hardware** via Ollama, registered through LiteLLM, available to every tool in your stack. Free, private, and always on.

**A private search engine (SearXNG)** that feeds web results to your AI tools and workflows without tracking, API keys, or subscriptions.

**An automation platform (n8n)** that connects everything. Webhooks trigger AI workflows. Scheduled jobs generate reports. RAG pipelines make your documents searchable. Triage systems classify and route requests. Your stack does things on its own while you sleep.

**A chat interface (Open WebUI)** where anyone in your household or team can talk to any model, local or cloud, without their own subscription.

**An AI research engine (Perplexica)** that searches the web via SearXNG and synthesizes answers using your LLM stack. Self-hosted Perplexity, no subscription required.

**Real web applications** you built and deployed, running in Docker on your server, accessible via Tailscale, pushed to GitHub. Proof that the stack works.

**Monitoring and infrastructure** that keeps everything running: uptime checks, automatic updates, reverse proxy with HTTPS, VPN access, and a dashboard to see it all at a glance.

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
      ├── Your Apps ───── Webapps you built and deployed
      │
      └── Monitoring ──── Uptime Kuma, Caddy, Tailscale
```

This is not a theoretical diagram. Every line represents a real connection running on real hardware. You built it, you understand it, and you own it.

---

## Appendix A: Prompts and Templates

Copy-paste-ready examples to get you building.

### Claude Code Starter Prompts

Use these examples to learn how to communicate effectively with Claude Code.

**Building Projects:**
```
Help me build a task tracker web app with:
- Python/FastAPI backend
- SQLite database
- Vanilla JS frontend (no frameworks)
- Docker deployment
```

```
Scaffold a new React app with TypeScript, Tailwind CSS, and Vite. Set up ESLint and Prettier with sensible defaults.
```

**Code Review:**
```
Review this code for security issues. Check for SQL injection, XSS, authentication bypasses, and API key exposure.
```

```
Find all places where we're making database queries without proper error handling.
```

**Infrastructure:**
```
Set up Portainer in Docker on my server at 192.168.1.100. Use docker-compose, expose on port 9000, configure with a volume for persistent data.
```

```
Create a docker-compose.yml for a standard LAMP stack with phpMyAdmin.
```

**Debugging:**
```
This error is happening when I run the app:
[paste error message]

Here's the relevant code:
[paste code or file path]

Help me fix it.
```

**Learning:**
```
Explain how the authentication flow works in this codebase. Trace the request from login to session creation.
```

**Automation:**
```
Create a bash script that backs up my .env files, archives them with timestamps, keeps only the last 10 backups, and runs via cron daily at 2am.
```

**Git Workflows:**
```
Create a pull request for my current branch. Look at the commit history and write a clear description of what changed.
```

### Agent Skill File Template

Save as `.claude/commands/your-agent.md`:

```markdown
---
name: YourAgent
description: Brief description of what this agent does
---

# YourAgent

**Trigger Phrase**: "Your trigger phrase here..."

**Context Doc**: `path/to/YOUR-AGENT.md`

## Agent Identity

You are **YourAgent**, a specialized AI assistant for [specific domain].

**Your role**: [What this agent does]

**Core principles**:
- Principle 1
- Principle 2

## Startup Sequence

1. Read context document
2. Check current state
3. Announce readiness

## Tools and Capabilities

**MCP Tools Available**:
- `mcp__service__tool`: what it does

**File Access**:
- Config: `path/to/configs/`

## Task Patterns

### Pattern 1: Common Task
**When**: User asks for X
**Steps**: 1, 2, 3
**Output**: What to return
```

### CLAUDE.md Starter Template

Save as `CLAUDE.md` in your project root:

```markdown
# Claude Memory

## Project Overview

**What this is**: [Project description]
**Tech stack**: [Languages, frameworks]

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

**Status**: [Active/Deployed/Paused]

## Project Overview

**Purpose**: What this does
**Use case**: Who uses it

## Tech Stack

**Backend**: [Framework, version]
**Frontend**: [Framework, version]
**Database**: [Type, version]

## Current Status

**What works**:
- Feature 1
- Feature 2

**In progress**:
- Current task

## Deployment Info

**Location**: [Server, port]
**Deploy command**: `docker compose up -d --build`

## Known Issues

**Issue 1**: Description
- Workaround: How to handle

## Next Steps

1. Next task
2. Following task
```

---

## Appendix B: Case Study — Building Tank, the Homelab Sysadmin Agent

This is the story of how Tank came to exist and how giving Claude Code SSH access to a remote Linux box changes everything. It's a concrete example of Claude Code + agents + MCP + SSH working together as a system.

### The Problem

You have a Linux box in the corner running Docker containers. When something breaks at 11 PM, you SSH in manually, run `docker ps`, check logs, restart containers, and troubleshoot. It works, but it's slow and you're doing the same steps every time.

You could script it, but then you're maintaining bash scripts. You could use Ansible, but now you're learning Ansible. What you actually want is to just ask "Is nix1 healthy?" and get an answer.

### The Solution: Tank

Tank is a specialized agent that lives inside Claude Code and knows how to manage homelab infrastructure. It has SSH access to your Linux box, understands Docker and Portainer, and can check system health, view logs, deploy containers, and troubleshoot issues.

You don't run Tank as a separate service. It's a subagent inside Claude Code that spawns when you ask homelab-related questions. It reads a context document with your infrastructure layout, uses Portainer MCP tools to manage containers, and SSHs into your server when it needs to run commands.

### How to Build Tank

Instead of writing these files manually, you're going to prompt Claude Code to build Tank for you. Here's how.

**Step 1: Describe what you want**

Open Claude Code and tell it what you need:

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

Claude will ask clarifying questions about your infrastructure. Answer them:
- What's your server's IP and hostname?
- What services are running?
- What username do you SSH with?
- What common tasks do you want Tank to handle?

**Step 2: Iterate on the agent**

Claude will generate the skill file and context doc. Review them and refine:

```
Update Tank's context doc to include my NAS at 192.168.1.200 and add a common task for checking SABnzbd status.
```

```
Add a security constraint: Tank should never run docker system prune without explicit confirmation.
```

**Step 3: Test and expand**

Start using Tank and add capabilities as you need them:

```
Tank needs to know how to deploy containers from docker-compose files. Add that to the skill file.
```

```
I want Tank to check if Tailscale is connected before SSHing. Add that to the startup sequence.
```

The agent evolves with you. You're not maintaining code. You're describing what you need and Claude builds it.

**Step 4: Set up SSH access**

Ask Claude Code to help you configure SSH:

```
I need to give you SSH access to my server at 192.168.1.100. Generate an ed25519 key, show me how to add it to the server, and configure ~/.ssh/config so you can connect easily.
```

Claude will generate the key, show you the `ssh-copy-id` command, and create the SSH config entry. Test it by asking:

```
SSH into my server and run `docker ps` to verify the connection works.
```

**Step 5: Register Portainer MCP**

Instead of manually figuring out the Portainer MCP syntax, ask Claude:

```
Help me register Portainer MCP. My Portainer instance is at 192.168.1.100:9443. Show me how to get an API token and register it with Claude Code.
```

Claude will walk you through:
1. Generating a Portainer API token
2. Running the `claude mcp add` command with the correct syntax
3. Testing that Tank can use `mcp__portainer__*` tools

### Security Model

Giving an AI assistant SSH access to your infrastructure sounds risky. Here's how to do it safely.

**What Tank CAN do:**
- Read-only system checks (`df -h`, `docker ps`, `systemctl status`)
- View logs (`docker logs`, `journalctl`)
- Restart containers via Portainer MCP (user approval required)
- Deploy containers from git repos (user approval required)

**What Tank CANNOT do without explicit confirmation:**
- Destructive operations (`rm -rf`, `docker system prune`)
- Modify production data
- Change firewall rules or networking
- Install system packages or update the OS

**How it's enforced:**

1. **SSH key scope**: The SSH key Tank uses is dedicated to homelab management. If compromised, you revoke one key, not your entire system.

2. **User approval prompts**: Claude Code asks permission before running commands in your configured permission mode. Risky operations always require approval.

3. **Sudo restrictions**: Don't give the Claude Code SSH user passwordless sudo for everything. If you need sudo, grant it only for specific commands:

```bash
# /etc/sudoers.d/claude-homelab
your-username ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart *
your-username ALL=(ALL) NOPASSWD: /usr/bin/docker restart *
```

4. **Prefer MCP over SSH**: Portainer MCP is safer than raw `docker` commands because it goes through Portainer's API with structured permissions. Use MCP tools first, SSH only when needed.

5. **Logging and auditing**: All SSH commands run by Claude Code appear in your shell history and system logs. You can audit what happened.

### Real Examples

**Example 1: System Health Check**

You: "Is my server healthy?"

Tank:
- SSHs to your server
- Runs `df -h`, `docker ps`, `uptime`
- Checks for containers in unhealthy state
- Reports back: "Server is healthy. Disk usage 42%, all 8 containers running, uptime 23 days."

**Example 2: Troubleshooting a Down Service**

You: "My web app isn't loading"

Tank:
- Uses Portainer MCP to check the container status
- Container is running but logs show port binding error
- Identifies that the port is already in use
- Suggests: "Port conflict detected. Another process is using port 3000. Restart the container or check what's using the port with `lsof -i :3000`."

**Example 3: Deploying a New Container**

You: "Deploy Uptime Kuma on my server"

Tank:
1. SSHs to your server and creates `~/docker/uptime-kuma/` directory
2. Writes a `docker-compose.yml` file
3. Runs `docker compose up -d`
4. Verifies the container started successfully
5. Reports: "Uptime Kuma deployed at http://192.168.1.100:3001"

All of this happens in one conversation. You don't switch windows, paste commands, or manually verify steps. You just talk to Tank.

### Why This Matters

This is the unlock. Claude Code isn't just a coding assistant anymore. It's an operator sitting next to you, managing your infrastructure while you focus on higher-level problems.

The power isn't in the automation. You could script all of this. The power is in the conversational interface and the context awareness. Tank knows your infrastructure layout, your conventions, your stack. It doesn't just run commands. It understands what you're trying to do and figures out the steps.

You go from "let me SSH in and check the logs" to "Tank, why is this slow?" and get a real answer.

**What you've learned:**
- How to give Claude Code SSH access safely
- How to build a specialized agent with real infrastructure knowledge
- How agents + MCP + SSH combine into something greater than the sum of parts
- Security model for AI-assisted infrastructure management

**Next step**: Build your own infrastructure agent. Start simple (health checks and log viewing), then expand as you get comfortable with the model.

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

## Up Next

Things I haven't built yet but plan to explore.

- **Evaluation frameworks.** As you start shipping AI-powered apps, you need a way to measure if your models are actually getting better. Tools like [Braintrust](https://www.braintrust.dev/) and [promptfoo](https://www.promptfoo.dev/) let you run evals against prompts and models systematically instead of eyeballing outputs.
- **Vector databases.** n8n handles basic RAG, but scaling up means choosing a dedicated vector store. [Chroma](https://www.trychroma.com/), [Pinecone](https://www.pinecone.io/), and [pgvector](https://github.com/pgvector/pgvector) are the main options depending on whether you want self-hosted, managed, or built into Postgres.

---

## About

Built and maintained by [pete-builds](https://github.com/pete-builds). This playbook reflects a real stack running in production on a home network, not a theoretical setup guide. It's a living document. Do your own research, dig into the docs, and make it yours.

Suggestions, corrections, and "you're missing X" welcome via [Issues](../../issues).
