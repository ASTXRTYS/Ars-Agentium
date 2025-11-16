# ASTXRTYS INDEX

Curated snapshots of LangChain ecosystem frameworks, memory frameworks, and production agents.

This repository is used as a **reference knowledge base** (not an active development project) and as a **DeepWiki / MCP indexing source** to study architecture patterns, integration approaches, and implementation details across the LangChain ecosystem and related technologies.

For LLM-aware tooling (Claude Code, DeepWiki agents, etc.), see `CLAUDE.md` for detailed guidance.

---

## Repository Purpose

- **Reference, not development**  
  This repo stores clean snapshots of external projects (LangChain, LangGraph, DeepAgents, LangSmith, memory frameworks, production agents). Active development should happen in the upstream repositories.

- **Architecture & pattern library**  
  Use this repo to:
  - Study how production agents structure their workflows (graphs, state management, HITL, orchestration).
  - Understand integration patterns between LangChain / LangGraph / DeepAgents / LangSmith.
  - Compare different memory architectures and frameworks.

- **Indexing surface for DeepWiki / MCP**  
  Directory layout and this README are designed to give LLMs a clear ontology of what is **canonical** framework behavior vs **third‑party extensions** vs **production examples**.

---

## Repository Structure

The repository is organized into three main categories.

### 1. LangChain Ecosystem (Core Frameworks)

These are **canonical, upstream frameworks and docs**. Prefer these for API semantics, contracts, and official patterns.

- `langchain-master/`  
  LangChain framework for building LLM applications.
- `langgraph-main/`  
  Low-level orchestration framework for stateful agents (StateGraph).
- `deepagents-master/`  
  High-level abstraction for deep agents with planning, subagents, and file system access. Built on top of LangGraph and provides `create_deep_agent()`.
- `langsmith-docs-main/`  
  LangSmith documentation.
- `langsmith-sdk-main/`  
  LangSmith SDK (Python and JavaScript).

When in doubt about how something *should* behave, defer to these core repositories.

---

### 2. Memory Frameworks (`Memory-Frameworks/`)

Located under `Memory-Frameworks/`.

These are **third‑party or extension memory frameworks**. They provide alternative or enhanced memory architectures and patterns. Treat them as a **pattern library**, not as canonical LangChain / LangGraph behavior.

- `cognee-main/`  
  ECL (Extract, Cognify, Load) pipelines for AI agent memory. An alternative/enhancement to traditional RAG patterns.
- `langmem-main/`  
  LangGraph-based memory integration framework (memory as part of graph state).
- `OpenMemory-main/`  
  Multi-language memory SDK (Python/JavaScript) with IDE integrations and dashboard.

Use this folder to compare memory architectures and to inspire new memory designs, but always cross-check APIs and contracts against the core frameworks.

---

### 3. Production Agents (`Production-Agents/`)

Located under `Production-Agents/`.

These are **opinionated, real‑world implementations** of agents and LangGraph/LangChain/DeepAgents applications. Treat them as **case studies and blueprints**, not as a single source of truth.

- `enterprise-deep-research-main/`  
  Enterprise deep research agent.
- `open_deep_research-main/`  
  Open-source deep research agent. Uses LangGraph, supports multiple LLM providers, and can be run via `uvx langgraph dev`.
- `local-deep-researcher-main/`  
  Local deep researcher implementation.
- `open-swe-main/`  
  Open-source SWE (software engineering) agent.

Use these projects to study:
- Workflow and graph patterns.
- Human-in-the-loop mechanisms.
- Multi-agent orchestration and supervisor patterns.
- LangSmith and tool integration in production contexts.

---

## How to Use This Repository

### For Humans

- **APIs and contracts**  
  Use the LangChain / LangGraph / DeepAgents / LangSmith directories when you need authoritative information about APIs, expected behavior, and supported patterns.

- **Memory architecture design**  
  Browse `Memory-Frameworks/` and `Production-Agents/` to compare different memory approaches (ECL pipelines, LangGraph-integrated memory, SDK-based memory) and how they show up in real agents.

- **Agent & graph design**  
  Explore `Production-Agents/` to see concrete graphs, supervisors, subagent patterns, interrupt flows, and state management strategies.

### For LLM Tools / DeepWiki / MCP

- **Source preference**
  - Prefer **core frameworks and LangSmith** (`langchain-master/`, `langgraph-main/`, `deepagents-master/`, `langsmith-docs-main/`, `langsmith-sdk-main/`) for API semantics, contracts, and official patterns.
  - Treat **Memory-Frameworks** as third‑party or extension pattern libraries.
  - Treat **Production-Agents** as opinionated examples and case studies.

- **Conflict resolution**
  - If a pattern in `Memory-Frameworks/` or `Production-Agents/` conflicts with core framework or LangSmith docs/SDKs, **defer to the core frameworks / LangSmith**.
  - When drawing from examples, call out when a pattern is opinionated or diverges from upstream defaults.

- **Cross-framework reasoning**
  - Use this repo to answer questions about how LangChain, LangGraph, DeepAgents, LangSmith, and memory frameworks fit together in real systems.

For Claude/agent-specific navigation and expectations, consult `CLAUDE.md`.

---

## Snapshot & Maintenance Philosophy

- **Snapshot-based**  
  Each subdirectory is a snapshot of an external project. Dependencies, build systems, and tooling are defined per-project.

- **No root-level builds**  
  There is no unified build system at the repo root. Run builds/tests only within individual project directories.

- **Git workflow**  
  - Updates should document the upstream version/commit being tracked in commit messages.
  - Prefer conventional commits, for example:  
    - `feat: update LangChain to v0.x.x`  
    - `docs: add LangSmith SDK snapshot`
  - Do not commit build artifacts (`node_modules`, `__pycache__`, etc.).

- **Development location**  
  Active development and issue tracking should happen in the original upstream repositories. This repo is a **knowledge base** for architectural reference, pattern discovery, and integration planning.

---

## Related File: `CLAUDE.md`

`CLAUDE.md` contains detailed guidance for Claude Code and other LLM tools working inside this repository, including:

- Navigation patterns (how to approach individual projects).
- Common use cases (architectural reference, pattern discovery, integration planning).
- Cross-framework insights (LangChain/LangGraph/DeepAgents relationships, memory framework patterns, HITL, multi-agent orchestration, state management).

Tools and agents should read `CLAUDE.md` alongside this `README.md` to fully understand how to interpret and use this repository.
