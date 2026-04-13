---
title: "Inside Hermes Agent: How a Self-Improving AI Agent Actually Works"
source: "https://mranand.substack.com/p/inside-hermes-agent-how-a-self-improving"
author:
  - "[[Mr. Ånand]]"
published: 2026-04-03
created: 2026-04-13
description: "April'26 - Issue #74"
tags:
  - "clippings"
---
## Introduction

[Hermes Agent](https://hermes-agent.nousresearch.com/) is an open-source AI agent built by [Nous Research](https://nousresearch.com/). Unlike OpenClaw, which is built around multi-agent orchestration, Hermes is a single agent that gets more capable the longer it runs, not through configuration updates, but through actual use.

Most agents recall what happened, but Hermes goes one step further: it extracts what worked, writes it as a reusable skill, and loads it the next time a similar problem comes up. The learning loop runs on its own, and because the memory architecture is cache-aware, it does not keep growing your token bill as the agent learns more.

This article breaks down the learning loop, the four-layer memory system, the gateway, agent loop internals, terminal backends, skills, tools, scheduled automations, session persistence, and running Hermes Agent at scale with [Nebius Token Factory](https://tokenfactory.nebius.com/).

**I have also written OpenClaw breakdown in past - [Read here](https://generativeai.pub/inside-openclaw-how-a-persistent-ai-agent-actually-works-44a2aa5cc1d9?sk=59db37766db1ce1b4b1a22198de48b4c)!**

## The Learning Loop

Agents like OpenClaw maintain context across sessions and route it through a central hub, which works well for simple use cases, but there is a gap between storing what happened and storing what worked. Hermes is designed around that gap, where completed workflows get turned into reusable procedures the agent can follow next time without retracing the same steps.

That is what the learning loop does: a closed feedback cycle running underneath every session where memory, skills, and session search are all outputs of the same continuous process.

![](https://substackcdn.com/image/fetch/$s_!_OGL!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7b56b0b9-9fa7-483a-955e-0e3749553621_1920x1080.png)

The loop has four moving parts, and each one fires at a different point in the cycle. Let’s see how they connect.

---

### Agent-Curated Memory with Periodic Nudges

Most agents either log everything and retrieve nothing useful, or log nothing at all and start fresh every session. Hermes avoids both by giving the agent itself the responsibility to decide what is worth keeping, and it does this through a mechanism called a periodic nudge.

At set intervals during a session, the agent receives an internal system-level prompt asking it to look back at what happened and evaluate whether anything is worth persisting to memory. This fires without user input, and the agent scans recent activity and writes it to its memory files if anything crosses the threshold of being useful in a future session.

![](https://substackcdn.com/image/fetch/$s_!ynCc!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F087593b6-1e89-4457-8762-7c2fd30a13ac_1920x1080.png)

The result is that memory stays curated rather than becoming a dump of every interaction.

### Autonomous Skill Creation

When the agent completes a task, it checks whether the path it took is worth documenting. The triggers are specific: five or more tool calls, recovery from an error, a user correction, or a non-obvious workflow that worked.

If the check passes, it creates a skill file in `~/.hermes/skills/`, not a log entry but a reusable instruction set the agent can follow in future sessions without retracing the original steps.

```markup
---
name: my-skill
description: Brief description of what this skill does
version: 1.0.0
platforms: [macos, linux]     # Optional — restrict to specific OS platforms
metadata:
  hermes:
    tags: [python, automation]
    category: devops
    fallback_for_toolsets: [web]    # Optional — conditional activation (see below)
    requires_toolsets: [terminal]   # Optional — conditional activation (see below)
---
```

Each skill file contains a name, a short description, the steps involved, and any tool calls or file references that are part of the workflow, and the structure follows the [agentskills.io](https://agentskills.io/specification) open standard, which makes skills portable across compatible agents.

### Skill Self-Improvement

Skills are not frozen once written, and the agent continues using them, updating them when it finds a better path mid-execution.

There are six skill actions available through the `skill_manage` tool:

`create`, `patch`, `edit`, `delete`, `write_file`, and `remove_file`.

The agent defaults to `patch` for most updates, making targeted changes by passing in only the old string and the replacement, which means only the changed text appears in the tool call rather than the full skill content.

The preference for `patch` over `edit` is both a correctness and efficiency decision, because a full rewrite risks breaking what was already working, while a patch corrects only what changed and is more token-efficient to execute.

### FTS5 Session Search with LLM Summarization

Every session is written into a SQLite archive and indexed using **[FTS5](https://hermes-agent.nousresearch.com/docs/user-guide/sessions?_highlight=fts5#fts5-query-syntax)**, so the agent searches past context rather than loading entire old sessions into the window. Retrieved results go through LLM summarization before being injected, so only what is relevant to the current task comes in.

![](https://substackcdn.com/image/fetch/$s_!TdUh!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fede02c61-94ce-46c1-a72b-45a67b3f5faa_1920x1080.png)

This layer handles episodic memory, what happened and when, and it is deliberately separate from the skills layer, which handles procedural memory, how to do things. The two work together but answer different questions, and keeping them in separate stores rather than mixing everything into one is a design decision that carries through to how the full memory system is structured, which is what the next section covers.

---

## The Multi-Level Memory System

Mixing everything into one memory store is why most agent memory systems become unreliable over time, so Hermes separates it into four distinct layers, each with a specific job, a specific location on disk, and a specific moment when it gets read.

### Prompt Memory MEMORY.md and USER.md

This is the always-on layer, the context that loads at the start of every session without the agent having to ask for it. Both files live in `~/.hermes/memories/` and get injected directly into the system prompt before the first message is processed.

The total character limit across both is 3,575, intentionally tight to force curation over accumulation. The agent manages them through the memory tool with three operations: add, replace, or remove.

![](https://substackcdn.com/image/fetch/$s_!osZD!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff5d8261d-0745-4c8d-86b5-48d803b431e1_1920x1080.png)

One important detail: edits to `MEMORY.md` or `USER.md` made during a session takes effect only in the next session, not mid-conversation.

### Session Search SQLite + FTS5

The distinction worth understanding here is when the agent reaches for session search versus prompt memory.

- Prompt memory is always-on, meaning it loads without the agent deciding to load it.
- Session search is a deliberate retrieval step; the agent runs a query against the SQLite archive when it determines that past context is relevant to the current task.

The practical boundary is permanence. If something is important enough to be present in every future conversation, it belongs in `MEMORY.md` or `USER.md`. If it is useful only when a specific topic comes up again, it remains in the session archive and can be retrieved on demand. The agent makes that judgment during the periodic nudge, deciding which layer a piece of information belongs in rather than defaulting everything into one place.

### Skills Procedural Memory

The creation side of skills we saw in Section 1, so the focus here is on how they are stored and how the agent loads them without blowing up the token budget.

All skills stay in `~/.hermes/skills/` as individual markdown files. On a fresh install, the bundled skills from the repository are copied into this directory, and any skills created by the agent or installed from the Skills Hub are added alongside them. The loading strategy follows a progressive disclosure pattern: by default, the system prompts include only the skill name and short summaries., and the full content of a skill loads only when the agent determines it is relevant to the current task.

![](https://substackcdn.com/image/fetch/$s_!BVy5!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F13c13356-5ef3-4de4-ab5b-dc68e1cf1c92_1920x1080.png)

This keeps token usage flat regardless of the number of skills that exist. An agent with 200 skills pays roughly the same context cost as one with 40, because the detailed content only enters the context when it is actually needed.

### Honcho Layer User Modeling

The three layers so far all require the agent to write something down actively. The fourth works differently, where instead of waiting for an explicit write, it builds a picture of you passively across sessions, tracking preferences, communication style, and domain knowledge as they shift over time. This is Honcho, an optional user modeling layer that sits above the rest.

It uses a dialectic modeling approach, modeling both you and the agent in relation to each other across 12 identity layers.

![](https://substackcdn.com/image/fetch/$s_!1Jn9!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8521e127-96ab-4c3f-bfbb-e0476c4423c3_1920x1080.png)

Image reference: https://hermes-agent.nousresearch.com/docs/user-guide/features/honcho#works-alongside-built-in-memory

It is optional, and for most task-specific or automation setups, the other three layers are enough. Where it earns its overhead is when you use Hermes as a daily personal assistant and want responses that feel calibrated to how you actually work.

The gateway, covered next, is what makes all four layers accessible across platforms without losing context when you switch.

## The Gateway

The learning loop and memory system are only useful if the agent is reachable when you need it, and that is what the Gateway handles. It is a persistent background service that keeps Hermes running and connected across every platform you have paired with it, so the agent is not something you launch when you need it but something that is always on and waiting.

### Platform Adapters and Session Routing

Hermes connects to CLI, Telegram, Discord, Slack, WhatsApp, Signal, and Email, each with its own adapter but all feeding into a shared session routing layer. A conversation started on Telegram can continue in the terminal because the session is tied to an ID, not the platform.

![](https://substackcdn.com/image/fetch/$s_!hezC!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F390e18ca-64c9-4115-b221-8db7def91b00_1920x1080.png)

Telegram goes a step further with Project Conversations, where Private Chat Topics let you run isolated workflows inside a single chat, each topic carrying its own skill bindings and session context.

The `gateway/` directory handles five things: messaging, session routing, delivery, pairing, and cron ticking.

- Pairing is how a new platform gets associated with your agent instance, and
- Cron ticking is how scheduled automations get triggered at the right time and routed back out to the right platform.

The whole thing runs as a system service, started with `hermes gateway`, which means it keeps running in the background even after you close your terminal.

---

### What This Enables vs. Other Agent Architectures

In OpenClaw, the gateway handles delivery, and that is where its responsibility ends. Skill creation, memory writes, and scheduled automation outputs all go through separate mechanisms that are not connected to the gateway itself.

In Hermes, the gateway is part of the same loop. An incoming message can trigger skill creation, a scheduled automation writes its output back through the same gateway layer, and cross-platform continuity works because session routing is wired into the system, not bolted on separately.

![](https://substackcdn.com/image/fetch/$s_!ux53!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F59aa7f78-308b-4b58-9837-573cd4309268_1920x1080.png)

With the gateway handling external communication and the memory system handling context persistence, the next piece to understand is what happens inside a single turn, from the moment a message arrives to the moment a response goes back out.

## The Agent Loop

Every message that reaches Hermes, whether from the CLI, Telegram, or any other connected platform, goes through the same synchronous orchestration engine implemented in `run_agent.py`. This is where memory, skills, tools, and the gateway all meet and execute together in a defined sequence.

### Turn Lifecycle

When a message arrives, the agent generates a task ID and either loads a cached system prompt or builds a fresh one from the memory layer, skill index, and any relevant context files. Before the API call goes out, a preflight compression check runs to confirm the conversation history is not approaching the context limit.

![](https://substackcdn.com/image/fetch/$s_!MP96!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3480101b-1cd3-4700-814f-0a89426c813f_1567x882.png)

If the model returns tool calls, the agent executes them, appends the results, and loops back for another API call. Once the model returns final text, the session persists to SQLite, and the response goes out through the gateway.

### Compression as Consolidation

When the pre-flight check flags a long conversation, a sentinel triggers before any hard context limits are hit. An auxiliary model scans the full conversation, extracts what is worth keeping into memory within the 3,575-character limit, and summarizes the middle turns rather than dropping them.

**[Lineage](https://hermes-agent.nousresearch.com/docs/developer-guide/session-storage?_highlight=lineage#lineage)**, the reference chain connecting summarized turns back to the original conversation, is preserved in SQLite so the agent can trace earlier context even after a compression event.

### Prompt Caching

Hermes builds its system prompt from stable sources, so the prefix stays consistent across turns, and most API providers cache it, reducing latency and cost on subsequent turns. Three things break that cache: switching models mid-session, changing memory files, or changing context files. Any of these forces a full reprocess, which is why the provider you use in production matters.

And if that provider goes down mid-session, the agent loop does not stall. You can configure an ordered list of inference providers in `config.yaml`, and Hermes falls through to the next one automatically without interrupting the turn lifecycle. The session continues, the context stays intact, and the failure never surfaces to the user.

## Terminal Backends

The gateway handles communication, but the terminal backend is what determines where the actual work happens. Hermes gives you six options, so the agent runs where the work actually is.

### The 6 Backends

The backends cover the full range of deployment contexts:

- **Local** runs directly on your machine,
- **Docker** adds a container layer for isolation without leaving your environment, and
- **SSH** lets the agent execute on a remote server entirely, which is useful when the work involves files, databases, or services that live there.
- **Daytona and Modal** are both serverless, meaning the execution environment hibernates when idle and spins back up when needed, which matters a lot for cost if you are running Hermes continuously but with uneven usage.
- **Singularity** is the option for HPC and research environments where Docker is not available or not permitted.

The practical choice between them comes down to what you are doing: Local for personal use, where isolation is not a concern, Docker when you want the agent sandboxed from your host system, SSH when the work is on a server, Singularity for HPC clusters, and Modal or Daytona when you want serverless execution that costs almost nothing during idle periods.

### Container Hardening and Zero Telemetry

When running with Docker, Hermes applies a read-only root filesystem, drops Linux capabilities, and provides namespace isolation by default. These are architectural defaults, not optional settings, so the agent cannot write outside its designated directories or escalate privileges.

Zero telemetry works the same way. Nothing leaves your machine by design, not as a privacy toggle but as a built-in property of how the agent operates.

## Skills and Tools

Tools and skills are both part of how Hermes extends its capabilities, but they operate at different levels, and it is worth being clear about the distinction before getting into either.

1. **Tools** are what the agent can call, individual capabilities like running a terminal command, searching the web, or generating an image.
2. **Skills** are what the agent knows how to do with those tools, reusable procedures that chain tool calls together into a workflow the agent has already figured out once and written down.

### Built-in Tools

The 40+ built-in tools cover five broad categories:

1. **Execution tools** handle terminal commands and code running.
2. **Web tools** cover search and browser automation.
3. **Media tools** include vision, image generation, and text-to-speech.
4. **Coordination tools** handle subagent delegation and multi-model reasoning, which lets the agent spin up isolated subagents for parallel workstreams or route specific tasks to a model better suited for them.
5. The last category covers memory and planning tools, which are how the agent interacts with its own memory layer programmatically during a session.

On the model side, Hermes connects to 400+ models through the Nous Portal as a single provider endpoint, and Hugging Face is supported as a first-class provider with full HF Inference API integration, a built-in model picker, and a setup wizard.

![](https://substackcdn.com/image/fetch/$s_!W3qw!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe0bc3be4-b7f6-44ec-941d-5c42d554d928_1920x1080.png)

It also supports the MCP capability, and for developers who want to go further, Hermes exposes four plugin hooks: `pre_llm_call`, `post_llm_call`, `on_session_start`, and `on_session_end`. These let you build on top of Hermes, adding custom logic at specific points in the agent loop, without forking the codebase or touching anything internal.

### Skills System

Hermes ships with 40+ bundled skills covering areas like MLOps, GitHub workflows, research, and productivity tasks. These get copied into `~/.hermes/skills/` on a fresh install alongside any skills the agent creates autonomously or that you install from the Skills Hub.

```markup
~/.hermes/skills/                  # Single source of truth
├── mlops/                         # Category directory
│   ├── axolotl/
│   │   ├── SKILL.md               # Main instructions (required)
│   │   ├── references/            # Additional docs
│   │   ├── templates/             # Output formats
│   │   ├── scripts/               # Helper scripts callable from the skill
│   │   └── assets/                # Supplementary files
│   └── vllm/
│       └── SKILL.md
├── devops/
│   └── deploy-k8s/                # Agent-created skill
│       ├── SKILL.md
│       └── references/
├── .hub/                          # Skills Hub state
│   ├── lock.json
│   ├── quarantine/
│   └── audit.log
└── .bundled_manifest              # Tracks seeded bundled skills
```

The storage format follows the **[agentskills.io](https://agentskills.io/specification)** open standard, so skills are portable across compatible agents and shareable without any conversion step. Only names and summaries load by default, full content comes in on demand.

## Scheduled Automations

Hermes handles recurring tasks as first-class agent tasks, not shell scripts or cron jobs that happen to call an AI. When you schedule your message, Hermes parses the instruction, stores the job in the `cron/` directory, and the gateway’s cron ticking handles the rest.

When the scheduled time arrives, the agent loop runs the task with full access to memory and skills, then routes the output through the gateway to wherever you specified. It is the same pipeline as any interactive session, just triggered by a clock tick instead of a message

## Session Persistence

Everything covered so far, the learning loop, memory layers, gateway routing, agent loop, and scheduled automations, depends on state surviving beyond a single session. That durability comes from a SQLite database managed by `hermes_state.py`, a portable file-based store with no external server dependency.

After each turn, the agent writes the conversation, tool calls, and results into the database, indexed via FTS5 for on-demand retrieval.

![](https://substackcdn.com/image/fetch/$s_!294a!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2a4eb626-a893-4b0c-8966-93e6dfcc5ca9_1920x1080.png)

Raw transcripts go to JSONL files, cron definitions sit separately on disk, and WAL mode allows concurrent reads with a single writer, which keeps things reliable when multiple sessions are running in parallel.

That is the architecture end-to-end. Everything above runs fine locally, but if you want to run Hermes-4-405B without managing the infrastructure yourself, that is where Nebius Token Factory comes in.

Let’s see how that works.

## Why I Picked Nebius?

**[Nebius Token Factory](https://docs.tokenfactory.nebius.com/quickstart)** is an inference platform built specifically for model inference, with powerful open-source models like Qwen 3.5, Nemotron Super, and GLM-5 available as managed endpoints.

For Hermes agent you can run Hermes-4-405B, the same model from Nous Research or choose other models from Nebius, without managing GPU provisioning, load balancing, or cold starts yourself.

![](https://substackcdn.com/image/fetch/$s_!MY-H!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F994cb621-8180-4145-9439-07aa0fd3b8c2_1920x1080.png)

What makes it more than just a hosted endpoint is what sits around the inference layer. Nebius logs your chat completions, and you can pipe those directly into [Data Lab](https://docs.tokenfactory.nebius.com/data-lab/overview) to build a fine-tuning dataset from your actual Hermes sessions. From there, you fine-tune on Token Factory and deploy the checkpoint as your own custom endpoint. If your Hermes setup handles a specific domain, support, research, or coding, you end up with a model tuned on exactly how you use it, not a generic base.

There is also observability built in. You can monitor request volume, latency, and token usage across your sessions, which matters when Hermes is running scheduled automations or serving multiple tasks for user, and you need to know when something is slow or breaking.

### Configuring Hermes to Use Hermes-4-405B

The configuration is a few lines. Set your API key, then run `hermes model` to open the model configuration, select custom OpenAI-compatible endpoint, and set the base URL and model ID:

```markup
export NEBIUS_API_KEY=your_key_here
hermes model
# Select: Custom OpenAI-compatible endpoint
# Base URL: <https://api.tokenfactory.nebius.com/v1/>
# Model: NousResearch/Hermes-4-405B
```

Hermes-4-405B has a 128K context window, enough for long sessions without hitting compression early, as the pre-flight check has more room to work with before the sentinel triggers.

### When Nebius Is Worth It (And When It Is Not)

Running a 405B model yourself means managing GPU provisioning, load balancing across inference workers, and cold-start behavior for idle sessions. Managed endpoints on Nebius remove all of that from your side.

That said, managed endpoints are not always the right call. If you are running Hermes locally as a personal assistant, pointing it at a smaller local model works fine and costs less.

Nebius Token Factory fits at multiple points in Hermes Agent setup. If you are still evaluating whether any models fits your workflow, the Nebius playground lets you test and compare models before committing to anything. Once you are ready, it covers the production side too: concurrent sessions, guaranteed uptime for scheduled automations, and managed infrastructure without provisioning GPUs yourself.

## Conclusion

That covers the full architecture. A lot of moving parts for something that starts with a single message, but each one is there for a reason: the agent should get better at your work specifically, not just at work in general. The preference for patch over edit, the four memory layers, the prompt caching strategy, every one of those decisions is about keeping it accurate and cheap to run over time, not just capable out of the box.

![](https://substackcdn.com/image/fetch/$s_!-_RJ!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcaa04c27-cb01-4725-9a18-666139d58e58_1920x1080.png)

This is not a lightweight setup you spin up for a quick task. It is infrastructure you run and maintain, and if your use case is narrow and short-lived, that overhead will feel like overkill.

But if you are building something you want to use daily, across platforms, handling tasks that repeat and evolve, Hermes is worth it. Where OpenClaw gives you modularity and orchestration across multiple agents, Hermes gives you a single agent that earns context over time, and whether you are still testing it out or running it as a service for multiple users, Nebius Token Factory is the straightforward path to get there without managing the infrastructure yourself. Which one makes sense depends entirely on what you are actually trying to build.

If you want to go deeper, here is everything referenced in this article:

- **[Hermes Agent Docs](https://hermes-agent.nousresearch.com/docs)**
- **[Hermes Agent GitHub](https://github.com/NousResearch/hermes-agent)**
- **[Nebius Token Factory Docs](https://docs.tokenfactory.nebius.com/quickstart)**
- **[agentskills.io](https://agentskills.io/specification)**

---

**Sukriya🙏🏼 See You Again Next Week! Till Then Keep Learning and Sharing Knowledge with Your Network**