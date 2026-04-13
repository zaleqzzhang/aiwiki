---
title: "Model, Agent, Harness: The Three Layers Most People Collapse Into One"
source: "https://contextua.dev/model-agent-harness-the-three-layers-most-people-collapse-into-one/"
author:
  - "[[Tom Foster]]"
published: 2026-03-22
created: 2026-04-13
description: "Most people use the word \"agent\" to describe the whole thing. They point at Claude Code or Codex CLI and say \"that is an AI agent.\" This is not wrong, exactly, but it is the kind of shorthand that becomes actively unhelpful the moment you need to make an architectural"
tags:
  - "clippings"
---
Most people use the word "agent" to describe the whole thing. They point at Claude Code or Codex CLI and say "that is an AI agent." This is not wrong, exactly, but it is the kind of shorthand that becomes actively unhelpful the moment you need to make an architectural decision, debug a failure, or explain to a colleague why the same model performs brilliantly in one tool and poorly in another.

The confusion is understandable. For the past year, vendor marketing has used "agent" as an umbrella term for anything that does more than autocomplete. But underneath that umbrella sit three distinct layers, each with different responsibilities, different failure modes, and different replacement costs. Choosing the right terms isn't about splitting hairs. It is the difference between understanding your system and merely using it.

## Layer 1: The Model

The model is the raw language model. Claude Opus 4.6,GPT-5.3 Codex, Gemini 3 Flash. It is a statistical engine trained on vast quantities of text, and its sole capability is predicting what comes next in a sequence. It has no memory of previous conversations. It cannot see your filesystem. It does not know what time it is. It cannot execute a shell command or click a button. Hand it a prompt, and it will return a completion. That is the full extent of what it does.

This is the layer that gets all the attention. Benchmarks measure it. Release notes celebrate it. And to be fair, the quality of the model matters enormously. A weak engine limits everything above it. But the model alone is not an agent any more than an engine alone is a car. It is a necessary but radically insufficient component.

## Layer 2: The Agent

The agent is the model plus a reasoning loop. This is the layer that transforms a text predictor into something goal-directed. Where the model simply completes a prompt, the agent observes a situation, formulates a plan, selects a tool, acts, evaluates the result, and decides what to do next. It is the difference between answering a question and pursuing an objective.

The reasoning loop is what gives the system its characteristic behaviour. A ReAct-style loop (Reason, Act, Observe) produces a different working pattern than a plan-then-execute approach. Some agents break problems into subtasks and delegate. Others iterate in tight cycles of trial and error. The choice of reasoning strategy has at least as much impact on real-world performance as the choice of model, and arguably more, because it determines how the model's intelligence is actually applied to the problem.

Critically, the agent layer is where "intelligence" meets "intention." The model provides cognitive horsepower. The agent layer decides where to point it.

## Layer 3: The Harness

The harness is the execution platform, including tools such as Claude Code, Codex CLI, Github Copilot CLI, Gemini CLI. It is the infrastructure that gives the agent a body: filesystem access, terminal execution, permission handling, session persistence, context assembly, and safety guardrails.

When the agent decides it needs to read a file, the harness is what actually performs the cat command. When the agent determines it should run the test suite, the harness is what executes npm test and, crucially, what asks you "Allow this command?" before doing so. When the agent formulates a multi-step plan, the harness is what maintains the session state so the agent does not forget what it did three steps ago.

This is also where safety lives. If the agent reasons its way into a destructive command, a well-built harness intercepts it. If the agent enters an infinite loop of failing tests, the harness is what breaks the cycle and hands control back to the human. The model cannot police itself. The agent's reasoning loop will not spontaneously decide to stop. The harness enforces the boundaries.

## A Concrete Example: Claude Code

To make this tangible, consider what happens when you use Claude Code with Opus 4.6. The API delivers both Layer 1 and Layer 2 in a single call. The raw model weights do the prediction (Layer 1), and the agentic reasoning loop, the ReAct-style planning, tool selection, and result evaluation, is baked into the model's training and the API's tool-use interface (Layer 2). Claude Code then sits cleanly at Layer 3. It provides the filesystem access, the terminal execution, the permission prompts, the session persistence, and the context assembly. It does not do the reasoning. It gives the reasoning something to act on.

The boundary is worth noting because it is not always this clean. Claude Code also makes decisions about what context to inject, how to chunk the codebase index, and when to break the agent's loop and ask the human for input. Those decisions shape the agent's behaviour so profoundly that the harness is not purely passive infrastructure. It is an orchestrator: it does not perform the reasoning, but it determines what context the agent receives, which tools it can invoke, and when the loop should stop.

## Harness vs Framework

It is worth distinguishing between an agent harness and an agent framework, because the terms are easy to conflate. Claude Code, Codex CLI, and Gemini CLI are harnesses: complete, opinionated, ready-to-use platforms that give an agent a body and put it to work immediately. Agent frameworks are a different category entirely. They are toolkits for building your own harness.

The dominant framework today is LangGraph, the agent-focused orchestration layer within the LangChain ecosystem. It models agent behaviour as a state graph, with nodes for actions and edges for transitions, and is used in production by several hundred organisations. CrewAI is the main alternative, taking a role-based approach where each agent is defined with a role, goal, and backstory, which makes multi-agent coordination more intuitive but less granular.

What both have in common is that they give you the components: chain abstractions, tool interfaces, memory modules, retrieval patterns. But you still have to wire them together, decide on the reasoning strategy, implement the permission model, build the context assembly logic, and handle session persistence. The end result of that work is a harness, but the framework itself is not one. This is why frameworks appeal to teams building bespoke agent systems for specific domains (customer support pipelines, document processing workflows, internal tooling), while harnesses like Claude Code appeal to developers who want to sit down and start working with an agent immediately against their own codebase.

## Why the Three-Layer Distinction Matters

The practical consequence of the three-layer model is composability. You can swap the model (Layer 1) without changing the harness. You can change the reasoning strategy (Layer 2) without touching the execution infrastructure. And you can move between harnesses (Layer 3) while keeping the same model and broadly similar agentic patterns.

This is not theoretical. It is exactly what happens when a team switches Claude Code from Sonnet to Opus for a complex refactoring task, or when an organisation evaluates whether Codex CLI's sandboxed execution model better suits their security posture than Claude Code's local execution model. Each swap happens at a specific layer, and understanding which layer you are changing is the difference between a deliberate architectural decision and an uninformed gamble.

The other consequence is diagnostic. When an AI coding agent fails, the three-layer model gives you a vocabulary for asking the right question. Was the model's reasoning wrong (Layer 1)? Did the agent choose the wrong tool or pursue the wrong plan (Layer 2)? Or did the harness fail to provide adequate context, enforce the right permissions, or maintain state correctly (Layer 3)? In practice, most failures I encounter in day-to-day use are Layer 3 problems. The model is smart enough. The reasoning loop is sound. The harness simply did not assemble the right context, or lost state at the wrong moment, or presented the agent with a tool interface that led it astray.

The next time you are evaluating an "AI agent," resist the urge to compare models. Compare harnesses. The model is the engine. The harness is everything else. And everything else is where the work actually gets done.