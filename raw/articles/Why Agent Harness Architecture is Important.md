---
title: "Why Agent Harness Architecture is Important"
source: "https://contextua.dev/why-agent-harness-architecture-is-important/"
author:
  - "[[Tom Foster]]"
published: 2026-03-08
created: 2026-04-13
description: "In my previous post on Specification-Driven Development, I argued that managing an LLM's unpredictability requires discipline, and that a well-formed specification is the most powerful context you can provide. The specification defines the what. The engineer designs the verification strategy. The LLM handles the how.But there's a layer beneath"
tags:
  - "clippings"
---
In my previous post on [Specification-Driven Development](https://contextua.dev/specification-driven-development/), I argued that managing an LLM's unpredictability requires discipline, and that a well-formed specification is the most powerful context you can provide. The specification defines the *what*. The engineer designs the verification strategy. The LLM handles the *how*.

But there's a layer beneath the specification that determines whether any of this actually works in practice: the **agent harness**.

The harness is the infrastructure that decides how the model receives context, which tools it can reach, how it recovers from failure, and whether its work persists across sessions. Two agents given the same specification will produce wildly different results if their harnesses differ in how they manage state, scope tool access, or handle the inevitable moment when the context window fills up.

As AI coding agents mature into daily-use engineering tools, understanding harness architecture has become essential. Not because you need to build one from scratch, but because the harness you choose shapes the ceiling of what your agent can accomplish, and the floor of what it can reliably repeat.

## What Is an Agent Harness?

An agent harness is the structured environment that sits between the language model and the real world. It manages the loop: the model observes, reasons, acts, and verifies. The harness orchestrates that cycle, providing the model with tools, enforcing permissions, persisting state across sessions, and compacting context when the window fills up.

Without a harness, you have a stateless model that forgets everything between turns. With one, you have something that can navigate a repository, run tests, fix what it broke, and pick up where it left off tomorrow morning.

The interesting question is not *whether* you need a harness, but how different architectural choices in harness design lead to fundamentally different agent capabilities and failure modes.

## Two Families of Harness Design

An analysis of five top agent harnesses (Claude Code, Codex, Gemini, Mistral Vibe and OpenCode) reveals a simple truth: they aren't all unique. They actually boil down to two schools of thought. The difference? One prioritizes the model’s internal logic, while the other leans on the structure of the environment itself.

### The Reasoning-First Family: Claude Code and Mistral Vibe 2.0

Claude Code and Mistral Vibe 2.0 share a common architectural conviction: trust the model, keep the scaffolding minimal, and invest in mechanisms that sustain the model's reasoning over long horizons.

Both implement what amounts to a master while-loop. Claude Code uses a simple "Gather, Take action, Verify, Repeat" cycle. Mistral Vibe 2.0 runs an equivalent loop through its middleware pipeline. In both cases, the harness avoids prescribing *how* the model should solve a problem. It provides tools, sets boundaries, and gets out of the way.

The deeper similarity lies in how they handle the central challenge of long-running agents: **context decay**. Both harnesses recognise that an agent's effectiveness degrades as its context window fills with stale tool outputs, failed attempts, and accumulated history.

Claude Code addresses this through a dual-agent architecture: an Initializer Agent sets up the environment and defines acceptance criteria in structured files (`claude-progress.txt`, `feature_list.json`), while a Coding Agent picks up work in subsequent sessions by reading those files and the git log. Context compaction runs automatically, and a hierarchy of `CLAUDE.md` files provides persistent project knowledge that survives across sessions and even across machines.

Mistral Vibe 2.0 takes a similar approach through different mechanisms. Its middleware pipeline includes an auto-compaction layer that proactively summarises conversation history as it approaches token limits. A persistent todo system tracks task state. And its "project-aware" startup automatically scans the file structure and git status, eliminating the manual orientation step that Anthropic's approach requires the coding agent to perform explicitly.

Both harnesses support subagent delegation, spawning independent agent instances with their own context windows for specific tasks like codebase exploration or architectural review. Claude Code goes further with Agent Teams, where multiple agents coordinate as peers through an asynchronous messaging system rather than strict parent-child delegation. Vibe 2.0 currently supports a single subagent type, though its architecture is designed for extension.

The philosophical alignment extends to safety: both enforce tool permissions through explicit approval levels and support deterministic hooks that fire at defined points in the agent lifecycle. Claude Code's hook system (PreToolUse, PostToolUse, SessionStart) and Vibe's trust-folder validation both serve the same purpose, providing a deterministic "firewall" around the model's probabilistic decisions.

Where they diverge is in response formatting and error recovery. Vibe 2.0 enforces a "structure-first" output style (tables, ASCII trees) optimised for terminal readability, while Claude Code favours conciseness with GitHub-flavoured markdown. More significantly, Vibe's middleware provides diff-based feedback when a `search_replace` operation fails, using sequence matching to show the model *why* its edit didn't land. This "strict edits, forgiving feedback" philosophy is a notable refinement that helps the model self-correct without wasting turns.

### The Environment-First Family: OpenAI Codex CLI and Gemini CLI

OpenAI's Codex CLI and Google's Gemini CLI approach the problem from the opposite direction. Rather than primarily trusting the model and providing minimal scaffolding, they invest heavily in making the *environment* legible, observable, and mechanically enforced.

OpenAI's "harness engineering" philosophy, demonstrated through a project that produced approximately one million lines of entirely agent-generated code, is built on the premise that the bottleneck in agentic performance is infrastructure, not intelligence. When an agent fails, the corrective action isn't a better prompt; it's identifying the missing environmental capability and making it legible to the agent.

This manifests in several distinctive architectural choices. The repository itself is reorganised for agent consumption through progressive disclosure: a lightweight `AGENTS.md` file acts as a high-level map, directing the agent to deeper documentation only as needed. Custom, agent-generated linters enforce architectural boundaries mechanically, not through instructions the model might ignore. Boundary validation using schema libraries (like Zod) ensures that the agent's code parses data shapes at boundaries rather than guessing structures that might hallucinate. And a system of "garbage collection" agents runs continuously in the background, scanning for architectural drift and opening cleanup pull requests automatically.

The Codex CLI's technical implementation reflects this philosophy. Built predominantly in Rust, it operates through a Codex App Server that communicates via bidirectional JSON-RPC. This client-server architecture allows the same agent logic to drive the terminal CLI, VS Code extensions, and a desktop application through a single source of truth. The protocol is structured around Items, Turns, and Threads, primitives that enable conversation persistence, forking, and resumption. The harness also supports three distinct permission modes (Auto, Approval, Read-Only) and platform-specific sandboxing through macOS Seatbelt and Linux Landlock.

Gemini CLI shares this environment-first orientation but applies it most distinctively to evaluation and observability. Where OpenAI focuses on making the repository agent-legible, Google focuses on making the agent's behaviour *measurable*. The Gemini API introduces "Thought Signatures" for its thinking models (Gemini 3 and 2.5 series), encrypted objects that represent the model's internal reasoning state immediately before a tool call. Any client using function calling with these models encounters them, but the Gemini CLI harness is designed to take full advantage of the mechanism. By requiring developers to pass these signatures back when submitting tool results, the API maintains reasoning continuity across multi-step tool interactions. If a developer constructs a conversation history without the correct signatures, the API returns a hard 400 error.

Gemini CLI's integration with GitHub Actions transforms it into a persistent repository teammate for issue triage, PR review, and on-demand collaboration. Its conversation checkpointing enables multi-day debugging sessions. And the Vertex AI evaluation service provides enterprise-grade trajectory analysis, measuring not just *what* the agent said, but whether it called the correct tools in the correct order with valid arguments.

A critical insight from research into long-context performance, widely cited in the harness engineering community, is the performance degradation that occurs when context utilisation exceeds roughly 40% of the window. This finding has direct implications for harness design: overloading agents with documentation, tool schemas, or accumulated history makes them *less* reliable, not more. It's a data point that validates the reasoning-first family's emphasis on aggressive context compaction.

### OpenCode: The Model-Agnostic Bridge

OpenCode occupies a unique position as a model-agnostic, open-source framework that can orchestrate over 75 different model providers. It draws from both families: its extended ReAct execution pipeline includes pre-check compaction, optional self-critique, and staged context management (reasoning-first characteristics), while its ecosystem of plugins provides isolation through devcontainers and granular permission enforcement through `opencode.json` (environment-first characteristics).

OpenCode's most interesting contribution is its identification of what it calls "The Harness Problem": the insight that the mechanical interface between a model's intent and the physical file modification is frequently the primary bottleneck, not the model's intelligence. This led to the development of Hashline, a referential editing system that tags every line of code with a unique content hash. Instead of requiring the model to reproduce exact original text (as `str_replace` does) or output a precise diff format (as `apply_patch` does), the model simply references line hashes. If the file changes between read and edit, the mismatched hashes prevent corruption automatically.

The performance data is striking. Models that struggled with conventional edit tools saw dramatic improvements: Grok Code Fast went from a 6.7% success rate with patches to 68.3% with Hashline, a 10x improvement from changing nothing about the model, only the harness. This is perhaps the strongest empirical evidence that harness architecture matters as much as model capability.

## Does Open Source Matter?

Four of the five harnesses examined are open source: Gemini CLI, Mistral Vibe 2.0, OpenCode CLI, and Codex CLI. Claude Code is the sole proprietary tool in this group.

For individual developers, the licensing distinction may feel academic; you use whichever tool makes you most productive. But for enterprise adoption, particularly in regulated European environments, the distinction carries real weight.

Open-source harnesses offer three concrete advantages. First, **auditability**: you can inspect exactly what the harness sends to the model, how it handles your code, and where data flows. For organisations subject to GDPR or operating in sectors where code is genuinely a trade secret, this isn't optional. Second, **portability**: an open-source, model-agnostic harness like OpenCode means you're not locked into a single model provider. If your CTO decides to move from one provider to another, or to run models locally for data sovereignty reasons, the harness adapts. Third, **community-driven improvement**: the Hashline innovation emerged from the OpenCode community, not from a vendor's product roadmap. When contributors use different models and encounter different failure modes, the fixes benefit everyone.

The counter-argument is integration quality. Claude Code's tight coupling with Anthropic's models, particularly the "Code Mode" pattern for MCP interaction (an approach explored independently by both Cloudflare and Anthropic, which reduces tool context usage by up to 98.7%), produces optimisations that are difficult to replicate in a model-agnostic framework. The same applies to Gemini's Thought Signatures, which are a model API feature deeply tied to the reasoning architecture of the Gemini model family. Proprietary harnesses can exploit model-specific capabilities that open-source alternatives cannot.

The pragmatic answer is that this isn't an either/or choice. Many teams will use a proprietary harness (Claude Code, Codex CLI) for daily development where deep model integration maximises productivity, while maintaining an open-source option (OpenCode, Gemini CLI) for use cases where data sovereignty, model flexibility, or auditability take priority.

## Connecting Back to Specification-Driven Development

In my SDD post, I described the developer's evolving role: from line-by-line coder to system architect and verifier. The harness is what makes verification *mechanical* rather than manual.

Consider the SDD workflow: create a specification, generate an implementation plan, execute the plan, and verify with evidence. Each step depends on harness capabilities that vary significantly across tools:

**State persistence** determines whether your specification can span multiple sessions. Claude Code's `CLAUDE.md` hierarchy and progress files, Vibe's persistent todo system, and Codex's thread persistence all solve this differently. Without it, every session starts from zero, and your specification becomes a document the agent reads once and may partially forget as context fills up.

**Verification loops** determine whether "verify with evidence" means anything. Anthropic's approach uses browser automation (Puppeteer) to validate changes from a user's perspective. Vibe runs tests through its bash tool with a "Break Loops" rule that forces escalation after two consecutive failures. Gemini's trajectory metrics can verify not just that the tests passed, but that the agent arrived at the solution through a reasonable sequence of actions.

**Context management** determines the practical ceiling on specification complexity. Research into long-context reasoning, and practitioner experience across the industry, suggests that agent reliability degrades significantly once context utilisation exceeds roughly 40% of the window. Claude Code's MCP "Code Mode" pattern addresses this by keeping tool definitions off the context window entirely. Vibe's proactive compaction summarises history before it becomes a problem. These aren't implementation details; they define the maximum complexity of specification a given harness can reliably execute.

The harness, in other words, is where the SDD philosophy meets engineering reality. A specification is only as good as the infrastructure that can hold it in context, execute against it, and verify the result.

## What Practitioners Should Know

If you're choosing a harness for your team, the architecture matters more than the marketing. Here's what I'd pay attention to:

**How does it handle sessions longer than thirty minutes?** This is where most harnesses diverge sharply. If your typical task is a quick refactor, any harness will do. If you're building features that require sustained multi-file reasoning, you need robust context compaction, progress persistence, and ideally subagent delegation.

**How does it recover from failed edits?** The Harness Problem is real. A model that correctly understands a change but can't mechanically apply it will burn tokens on retry loops. Vibe's diff-based feedback and OpenCode's Hashline represent genuinely different approaches to the same fundamental bottleneck.

**Where does your code go?** Claude Code operates local-first with optional remote supervision. Codex supports both local and cloud execution. Vibe supports fully local operation, including with locally-hosted models via the Devstral family. If you're in a regulated environment or working with sensitive codebases, the data flow architecture of your harness is a security decision, not just a convenience preference.

**Can it grow with your team?** Agent Teams (Claude Code), the Ralph Loop pattern (Codex), and OMO's multi-agent orchestration (OpenCode) represent the leading edge of multi-agent coordination. If you're planning to scale beyond a single developer working with a single agent, the harness's coordination model will determine your ceiling.

## The Harness Is the New Build System

Two decades ago, build systems were a solved problem, or so we thought. Then CI/CD transformed them into the critical infrastructure that determined how fast teams could ship. The agent harness is at a similar inflection point.

Today, harnesses are still young enough that switching costs are manageable and the landscape is shifting rapidly. But the architectural decisions being made now (how state persists, how context is managed, how multiple agents coordinate) will compound over time. The teams that understand these trade-offs, rather than simply adopting whichever tool has the best demo, will be the ones that extract the most value from their agents.

The specification tells the agent what to build. The harness determines whether it can.