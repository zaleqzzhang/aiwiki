---
title: "Hermes Agent Tutorial: Install & Set Up Your First Self-Improving AI (2026)"
source: "https://www.nxcode.io/resources/news/hermes-agent-tutorial-install-setup-first-agent-2026"
author:
  - "[[NxCode Team]]"
published: 2026-04-12
created: 2026-04-13
description: "Step-by-step Hermes Agent tutorial: install in 5 minutes, connect your AI model, set up Telegram/Discord bots, and create your first auto-learning skill. From zero to running agent."
tags:
  - "clippings"
---
Turn your idea into a working app — no coding required.[Start Free](https://studio.nxcode.io/?ref=article_top_hermes-agent-tutorial-install-setup-first-agent-2026&article=hermes-agent-tutorial-install-setup-first-agent-2026)

## Hermes Agent Tutorial: Install & Build Your First Self-Improving AI (2026)

**April 2026** — Most AI tools forget everything the moment you close the terminal. [Hermes Agent](https://github.com/NousResearch/hermes-agent) by Nous Research doesn't. It is an open-source AI agent that learns from every task you give it, creates reusable skills from successful workflows, and persists memory across sessions. Think of it as an AI assistant that actually gets better the more you use it.

In this tutorial, you will go from zero to a running Hermes Agent connected to your preferred AI model, with a Telegram or Discord bot, automated scheduled tasks, and your first self-created skill — all in under 30 minutes.

Here is exactly what you will build:

- A locally installed Hermes Agent (v0.8.0) connected to an LLM of your choice
- A messaging gateway that lets you interact via Telegram, Discord, or Slack
- An agent that auto-creates skill documents from complex tasks
- A cron-scheduled automated task that runs without your intervention

---

## Prerequisites

Before you begin, check that your system meets these requirements:

**Operating System:**

- Linux (Ubuntu 20.04+, Debian, Fedora, Arch)
- macOS (Intel or Apple Silicon)
- Windows via WSL2 (native Windows is not supported — [install WSL2 first](https://learn.microsoft.com/en-us/windows/wsl/install))

**Hardware:**

- 4 GB RAM minimum (8 GB+ recommended)
- 2 GB free disk space
- For local models via Ollama: a GPU with 8-16 GB+ VRAM (Apple Silicon's Metal acceleration works well, delivering 50-80 tokens/second on 7B models)

**AI Model Access (pick one):**

- An API key from OpenRouter, OpenAI, or Anthropic
- Ollama installed locally for free, offline usage
- A Nous Portal account

**Model Context Requirement:** Hermes Agent requires a model with at least **64,000 tokens of context**. Models with smaller context windows cannot maintain enough working memory for multi-step tool-calling workflows and will be rejected at startup.

---

## Step 1: Install Hermes Agent

Open your terminal and run the one-line installer:

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

That single command handles everything:

1. **Detects your OS** and installs missing system dependencies (Python 3.11+, Node.js, ripgrep, ffmpeg)
2. **Clones the repository** to `~/.hermes`
3. **Creates a Python virtual environment** and installs all Python/Node packages
4. **Registers a global `hermes` command** so you can run it from anywhere
5. **Launches the setup wizard** to configure your LLM provider

The entire process takes under 5 minutes on a typical connection.

Once the installer finishes, reload your shell:

```bash
source ~/.bashrc    # or source ~/.zshrc on macOS
```

Verify the installation:

```bash
hermes --version
# Expected output: hermes-agent v0.8.0
```

If you encounter any issues, run the built-in diagnostics:

```bash
hermes doctor
```

This checks your environment, dependencies, API connectivity, and configuration for common problems.

### Migration from OpenClaw

If you were previously using OpenClaw, the setup wizard automatically detects `~/.openclaw` and offers to migrate your configuration, skills, and memory before proceeding.

---

## Step 2: Choose & Configure Your AI Model

During installation, the setup wizard prompts you to pick an LLM provider. You can also change it later at any time:

```bash
hermes model
```

Here are the main options:

### Option A: OpenRouter (200+ Models)

OpenRouter gives you access to over 200 models including Claude, GPT-4, Gemini, and Llama — all through a single API key.

1. Sign up at [openrouter.ai](https://openrouter.ai/) and get your API key
2. Select "OpenRouter" in the setup wizard
3. Paste your API key
4. Choose a model (Claude Sonnet, GPT-4o, or Llama 3.1 405B are solid choices)

### Option B: Anthropic API (Direct)

If you want to use Claude directly:

1. Get your API key from [console.anthropic.com](https://console.anthropic.com/)
2. Select "Anthropic" in the setup wizard
3. Paste your API key
4. The wizard defaults to the latest Claude model with 200K context

### Option C: Ollama (Free, Local, Offline)

This is the zero-cost option. No API calls, no data leaving your machine.

1. Install Ollama if you haven't: `curl -fsSL https://ollama.com/install.sh | sh`
2. Pull a model with sufficient context:
```bash
ollama pull gemma4       # ~16 GB VRAM
# or
ollama pull qwen3.5      # ~11 GB VRAM
```
1. In the Hermes setup wizard, select "Custom endpoint"
2. Set the API base URL to `http://127.0.0.1:11434/v1`
3. Leave the API key blank (not required for local Ollama)

When running a local model, make sure to set its context size to at least 64K:

```bash
ollama run gemma4 --ctx-size 65536
```

### Provider Fallback Chains

Hermes supports ordered fallback provider chains. If your primary provider goes down, it automatically switches to the next one. Configure this in `~/.hermes/config.yaml`:

```yaml
fallback_providers:
  - provider: openrouter
    model: anthropic/claude-sonnet
  - provider: ollama
    model: gemma4
```

---

## Step 3: Your First Conversation

Start Hermes in the terminal:

```bash
hermes
```

You are now in an interactive chat session with your agent. Try something simple first:

```
> What's in my current directory? List files and tell me what this project does.
```

Hermes uses its built-in tools — file operations, terminal commands, web search — to explore your filesystem and respond with context. It ships with **40+ built-in tools** including:

- **File operations** — read, write, search, and edit files
- **Terminal commands** — run shell commands and capture output
- **Web search** — fetch live information from the internet
- **Code analysis** — understand codebases, find patterns, explain logic

### CLI Essentials

A few commands to know during a session:

| Command | What it does |
| --- | --- |
| `/help` | List all available commands |
| `/tools` | Show enabled tools |
| `/compress` | Summarize conversation to reduce token usage |
| `/memory` | View the agent's persistent memory |
| `/skills` | List all learned skills |
| `/quit` | End the session |

The `/compress` command is especially important for long sessions. It summarizes conversation history and reduces token usage significantly while preserving context — useful when working with models that have tighter context windows or to reduce API costs.

---

## Step 4: Set Up a Telegram or Discord Bot

This is where Hermes becomes more than a CLI tool. The **messaging gateway** lets you interact with your agent through Telegram, Discord, Slack, WhatsApp, Signal, and more — all from a single background process.

### Telegram Setup

1. Open Telegram and search for **@BotFather**
2. Send `/newbot`
3. Pick a name and username for your bot
4. Copy the bot token BotFather gives you

Now configure Hermes:

```bash
hermes gateway setup
```

Select **Telegram** when prompted and paste your bot token. To restrict access to only your account:

```bash
# Find your Telegram user ID (send a message to @userinfobot)
export TELEGRAM_ALLOWED_USERS=123456789
```

### Discord Setup

1. Go to the [Discord Developer Portal](https://discord.com/developers/applications)
2. Create a new application and add a bot
3. Copy the bot token
4. Invite the bot to your server with the appropriate permissions

Run the gateway setup:

```bash
hermes gateway setup
```

Select **Discord** and paste your token. Useful Discord-specific options:

```yaml
# In ~/.hermes/config.yaml
discord:
  require_mention: true           # Only respond when @mentioned in servers
  free_response_channels: "12345" # Channel IDs where bot responds freely
  auto_thread: true               # Create threads on @mention
```

### Start the Gateway

Run the gateway in the foreground to test:

```bash
hermes gateway
```

Once it works, install it as a persistent service:

```bash
# macOS (launchd)
hermes gateway install

# Linux (systemd user service)
hermes gateway install

# Linux (system-wide, survives reboot)
sudo hermes gateway install --system
```

The service automatically restarts if it crashes and starts at boot.

---

## Step 5: Watch It Learn — Skills & Memory

This is the feature that sets Hermes apart. After completing a complex task (typically 5+ tool calls), the agent **autonomously creates a skill** — a structured markdown document capturing what it did, how it did it, pitfalls it encountered, and verification steps.

### How Skills Work

1. You ask Hermes to do something complex — say, "Set up a Python FastAPI project with Docker, tests, and CI"
2. Hermes completes the task using multiple tools
3. It recognizes the workflow was non-trivial and creates a skill file
4. Next time you or anyone asks for a similar task, it loads that skill instead of figuring things out from scratch

Skills follow the [agentskills.io](https://agentskills.io/) standard, meaning they are portable and shareable across agents.

### Viewing and Managing Skills

```bash
hermes skills            # List all skills
hermes skills show <name> # View a specific skill
```

Skills self-improve during use. If the agent discovers a better approach while executing a skill, it updates the skill document with the improved procedure.

### Persistent Memory

Beyond skills, Hermes maintains a `MEMORY.md` file — agent-curated facts about you, your preferences, your projects, and your workflows. It writes to this file with periodic nudges to persist durable knowledge.

Session history, memory entries, and skill metadata are all stored in **SQLite with FTS5 full-text search**, so the agent can recall relevant context across sessions efficiently.

```bash
hermes memory            # View current memory
hermes memory search "docker" # Search memory for a topic
```

---

## Step 6: Set Up Automated Tasks with the Cron Scheduler

Hermes has a built-in cron scheduler that runs tasks autonomously on any schedule and delivers results to any configured platform.

### Create a Scheduled Task

You can set up tasks in natural language:

```bash
hermes cron add
```

When prompted, describe the task:

```
Every morning at 9am, check Hacker News for AI news and send me a summary on Telegram.
```

Or be more specific with a cron expression:

```bash
hermes cron add --schedule "0 9 * * *" --prompt "Check Hacker News for the top 5 AI-related stories. Summarize each in 2 sentences. Send to Telegram." --deliver telegram
```

### Manage Scheduled Tasks

```bash
hermes cron list          # Show all scheduled tasks
hermes cron remove <id>   # Delete a task
hermes cron logs <id>     # View execution history
hermes cron start         # Start the scheduler daemon
```

Each scheduled job stores a prompt, a schedule expression, and optional delivery settings. The scheduler runs as a background process and fires the agent with the stored prompt at the scheduled time.

**Practical cron job ideas:**

- Daily summary of GitHub issues in your repositories
- Weekly digest of industry news delivered to Slack
- Hourly monitoring of your production service health
- Morning briefing with calendar events and task priorities

---

## Advanced: MCP Servers & Custom Tools

Hermes supports the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/), which lets you connect it to external services — GitHub, databases, Stripe, or any tool that exposes an MCP endpoint.

### Adding an MCP Server

Edit `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  github:
    command: npx
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "ghp_your_token_here"

  filesystem:
    command: npx
    args: ["-y", "@modelcontextprotocol/server-filesystem", "/home/user/projects"]

  stripe:
    url: "https://mcp.stripe.com"
    headers:
      Authorization: "Bearer sk_live_your_key"
```

### MCP Configuration Options

Each server supports optional settings:

```yaml
mcp_servers:
  github:
    command: npx
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "ghp_xxx"
    timeout: 120          # Tool call timeout in seconds
    connect_timeout: 60   # Initial connection timeout
    tools:
      include: [list_issues, create_issue, search_code]
    prompts: false
    resources: false
```

The `tools.include` and `tools.exclude` lists let you control exactly which tools the agent can access from each MCP server — useful for security and cost control.

### Terminal Backends

Hermes supports six terminal backends that determine where the agent's shell commands execute:

| Backend | Use Case |
| --- | --- |
| **local** | Direct execution on your machine (default) |
| **docker** | Sandboxed execution in a Docker container |
| **ssh** | Remote execution on a server via SSH |
| **daytona** | Managed workspace with serverless persistence |
| **modal** | Serverless cloud sandbox |
| **singularity** | HPC-compatible container execution |

Configure in `~/.hermes/config.yaml`:

```yaml
terminal:
  backend: docker
  working_directory: /workspace
  command_timeout: 300
  env_forward:
    - HOME
    - PATH
```

Docker backend runs commands with all capabilities dropped and no privilege escalation enabled by default — ideal for running untrusted or experimental code.

---

## Troubleshooting Common Issues

### "Model context too small" Error

Hermes requires a model with at least 64K context tokens. If you see this error with a local model, increase the context size:

```bash
ollama run your-model --ctx-size 65536
```

### High Token Usage

With default tools and SOUL.md, Hermes uses approximately 6-8K input tokens per CLI turn and 15-20K per Telegram message. To reduce usage:

- Run `/compress` regularly during long sessions
- Disable unused tools with `hermes tools`
- Keep your SOUL.md concise

### Gateway Won't Connect

1. Run `hermes doctor` to check your environment
2. Verify your bot token is correct in `~/.hermes/.env`
3. Check that the gateway service is running: `hermes gateway status`
4. Review logs: `hermes gateway logs`

### Commands Blocked by Tirith

Tirith is Hermes's security module. It hard-blocks dangerous commands like piping curl output to shell (`curl | sh`). If a legitimate command is blocked, you can review and adjust Tirith rules in the configuration. This is by design — the security module protects against prompt injection attacks that try to trick the agent into running harmful commands.

### Cron Jobs Not Firing

Most cron issues fall into four categories:

1. **Timing** — Verify the cron expression and timezone in your config
2. **Delivery** — Ensure the gateway is running if delivering to Telegram/Discord
3. **Permissions** — Check that the agent has access to required tools
4. **Skill loading** — If the job references a skill, confirm the skill file exists

Run `hermes cron logs <id>` to see execution history and error details.

### Configuration File Reference

Hermes stores all settings and state under `~/.hermes/`:

| File/Directory | Purpose |
| --- | --- |
| `config.yaml` | Main configuration |
| `.env` | API keys and secrets |
| `SOUL.md` | Agent identity and personality |
| `MEMORY.md` | Persistent memory |
| `skills/` | Learned skill documents |
| `sessions/` | Conversation history |
| `cron/` | Scheduled task definitions |
| `logs/` | Application logs |

---

## What to Build Next

Now that your agent is running, here are some practical projects to try:

- **Personal research assistant** — Have Hermes monitor RSS feeds, summarize papers, and deliver daily briefings to Telegram
- **Code review bot** — Connect the GitHub MCP server and have Hermes review pull requests on a schedule
- **DevOps monitor** — Set up cron jobs to check server health, SSL certificates, and uptime, with alerts via Discord
- **Data pipeline automation** — Let Hermes fetch, clean, and transform data from APIs on a recurring schedule
- **Meeting prep agent** — Before your meetings, have Hermes pull relevant docs, past notes, and action items

### Hermes + NxCode: From Automation to Full Applications

Hermes excels at automating tasks and learning from your workflows — it gets smarter every time you use it. But when you need to go beyond scripts and build a complete web application, [NxCode's Export mode](https://nxcode.io/) is a natural companion. Generate full-stack applications with AI, export the complete source code, and iterate further. Hermes handles the automation layer; NxCode handles the application layer. Together, they cover the full spectrum from task automation to production-ready apps.

---

## Bottom Line

Hermes Agent is one of the most practical open-source AI agent projects available in 2026. The install takes 5 minutes. The learning curve is gentle — you start with CLI conversations and layer on messaging gateways, MCP servers, and cron jobs as you need them. The self-improving skills system means your agent compounds in value over time rather than starting from zero each session.

The key steps covered in this tutorial:

1. **Install** with the one-line installer
2. **Connect** your preferred AI model (cloud or local)
3. **Converse** directly in the terminal
4. **Deploy** a Telegram or Discord bot via the gateway
5. **Observe** skill creation and memory persistence
6. **Automate** recurring tasks with the cron scheduler
7. **Extend** with MCP servers and custom tools

Start with the basics, let the agent learn your workflows, and scale up from there. The [official documentation](https://hermes-agent.nousresearch.com/docs/) covers everything else you need.

---

*Sources: [Hermes Agent GitHub Repository](https://github.com/NousResearch/hermes-agent) | [Official Documentation](https://hermes-agent.nousresearch.com/docs/) | [Installation Guide](https://hermes-agent.nousresearch.com/docs/getting-started/installation/) | [Quickstart Guide](https://hermes-agent.nousresearch.com/docs/getting-started/quickstart) | [MCP Integration Docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp) | [FAQ & Troubleshooting](https://hermes-agent.nousresearch.com/docs/reference/faq)*

[Back to all news](https://www.nxcode.io/resources/news)

Enjoyed this article?