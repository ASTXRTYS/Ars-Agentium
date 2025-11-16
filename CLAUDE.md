# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

The INDEX repository (`ASTXRTYS/index`) is a curated reference collection of AI framework source code and production agent implementations. It serves as a knowledge base for understanding architecture patterns, integration approaches, and implementation details across the LangChain ecosystem and related technologies.

**This is primarily a reference repository** - not an active development project. Individual subdirectories contain complete snapshots of external projects.

## Repository Structure

The repository is organized into three main categories:

### 1. LangChain Ecosystem (Core Frameworks)
- **`langchain-master/`** - LangChain framework for building LLM applications
  - Has its own `CLAUDE.md` with development guidelines
  - Python-based, uses `uv` for package management
  - Build: `uv sync`, Test: `make test`, Lint: `make lint`
- **`langgraph-main/`** - Low-level orchestration framework for stateful agents
  - Python-based, workflow graph construction
- **`deepagents-master/`** - High-level abstraction for deep agents with planning, subagents, and file system access
  - Built on top of LangGraph
  - Provides `create_deep_agent()` for rapid agent development
- **`langsmith-docs-main/`** - LangSmith documentation
- **`langsmith-sdk-main/`** - LangSmith SDK (Python and JavaScript)

### 2. Memory Frameworks
Located in `Memory-Frameworks/`:
- **`cognee-main/`** - ECL (Extract, Cognify, Load) pipelines for AI agent memory
  - Alternative/enhancement to RAG patterns
- **`langmem-main/`** - LangChain memory integration framework
  - LangGraph-based memory management
- **`OpenMemory-main/`** - Multi-language memory SDK (Python/JavaScript)
  - Includes IDE integrations and dashboard

### 3. Production Agents
Located in `Production-Agents/`:
- **`enterprise-deep-research-main/`** - Enterprise deep research agent
- **`open_deep_research-main/`** - Open-source deep research agent
  - Has its own `CLAUDE.md` describing its architecture
  - Uses LangGraph, supports multiple LLM providers
  - Run: `uvx langgraph dev`
- **`local-deep-researcher-main/`** - Local deep researcher implementation
- **`open-swe-main/`** - Open-source SWE (Software Engineering) agent

## Working with This Repository

### Navigation Pattern

When exploring a specific project:
1. Check if the subdirectory has its own `CLAUDE.md` - follow those guidelines for project-specific work
2. Read the subdirectory's `README.md` for project overview and setup
3. Each project is self-contained with its own dependencies, build system, and tooling

### Common Use Cases

**Architectural Reference**
- Study how production agents structure their workflows (graph patterns, state management)
- Understand integration patterns between LangChain/LangGraph/DeepAgents
- Compare memory framework approaches

**Code Pattern Discovery**
- Find examples of interrupt() usage in LangGraph workflows
- Study middleware patterns in LangChain agents
- Reference tool calling and multi-agent orchestration patterns

**Integration Planning**
- Understand how memory frameworks integrate with agent workflows
- Study MCP (Model Context Protocol) server implementations
- Compare different LLM provider integration approaches

### Repository Maintenance

**Git Workflow**
```bash
# Check current state
git status

# This repo tracks snapshots of external projects
# Commits should reflect major updates to tracked frameworks
git log --oneline
```

**Updating Snapshots**
When updating a framework snapshot:
1. Document the version/commit being tracked in commit messages
2. Use conventional commit format: `feat: update LangChain to v0.x.x` or `docs: add LangSmith SDK snapshot`
3. Avoid committing node_modules, __pycache__, or other build artifacts

## Key Architectural Insights

### LangChain/LangGraph/DeepAgents Relationship

**Abstraction Layers (Low to High):**
1. **LangGraph** (StateGraph) - Full control, custom nodes/edges, maximum flexibility
2. **LangChain** (`create_agent`) - Pre-built agent loop, middleware customization, less boilerplate
3. **DeepAgents** (`create_deep_agent`) - Highest-level, includes planning/file ops/subagents out of the box

**Choose based on needs:**
- Need custom workflow control? → StateGraph (LangGraph)
- Need standard agent with hooks? → create_agent (LangChain)
- Need full-featured deep agent quickly? → create_deep_agent (DeepAgents)

### Memory Framework Patterns

Each framework takes a different approach:
- **cognee**: ECL pipeline pattern (replace RAG)
- **langmem**: LangGraph integration (memory as graph state)
- **OpenMemory**: Multi-platform SDK approach

Study these for different memory architecture patterns.

## Cross-Framework Patterns to Study

### Human-in-the-Loop (HITL)
- LangGraph: `interrupt()` mechanism
- DeepAgents: Built-in approval middleware
- Research agents: Iterative refinement patterns

### Multi-Agent Orchestration
- Supervisor patterns (enterprise-deep-research, open-swe)
- Subagent spawning (deepagents)
- Parallel agent execution (open_deep_research)

### State Management
- Persistent state across interrupts
- Replay behavior and checkpointing
- State serialization patterns

## Notes for Development

- **Do not run builds at repository root** - there is no root-level build system
- **Individual projects are independent** - dependencies and tooling vary by subdirectory
- **Reference, don't modify** - This is a knowledge base; active development should happen in original upstream repositories
- **Check git status** - Unstaged changes may indicate exploration work in progress

## Quick Reference

| Framework | Primary Language | Entry Point | Key Concept |
|-----------|------------------|-------------|-------------|
| LangChain | Python | Various | LLM application framework |
| LangGraph | Python | StateGraph | Workflow orchestration |
| DeepAgents | Python | create_deep_agent | High-level agent abstraction |
| cognee | Python | ECL pipelines | Memory via extraction |
| langmem | Python | Memory nodes | LangGraph memory integration |
| open_deep_research | Python | deep_researcher | Configurable research agent |

---

**Repository Philosophy:** Maintain clean snapshots of key AI frameworks for architectural reference, pattern discovery, and integration planning.
