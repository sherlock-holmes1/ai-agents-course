# AI Agents Course

A comprehensive learning repository for building AI agents using Google's Agent Development Kit (ADK) and Gemini models.

## Overview

This course provides hands-on examples of progressively complex AI agent patterns, from basic single agents to sophisticated multi-agent systems with memory and state persistence. Each lesson builds on previous concepts to demonstrate production-ready agent architectures.

## What You'll Learn

- **Agent Orchestration**: Sequential, parallel, and iterative agent workflows
- **Tool Integration**: Custom functions, agent-as-tool patterns, and external integrations via MCP
- **State Management**: Session handling and long-term memory systems
- **Human-in-the-Loop**: Pausable workflows with approval gates
- **Production Patterns**: Retry logic, error handling, and persistent storage

## Course Structure

### Lesson 1: Agent Composition Patterns
Learn fundamental agent orchestration strategies including hierarchical agents, sequential execution, parallel processing, and iterative refinement loops.

### Lesson 2: Tool Integration
Explore custom function tools, code executors, MCP (Model Context Protocol) integration, and resumable applications with human approval workflows.

### Lesson 3: State Management
Master session management with both in-memory and persistent storage, plus long-term memory systems for cross-session information retrieval.

## Getting Started

```bash
# Install dependencies
pip install -r requirements.txt

# Run an example agent
python lesson1/my_agent/agent.py
```

## Prerequisites

- Python 3.12+
- Google API key (set as `GOOGLE_API_KEY` environment variable)

## Technology Stack

- **Google ADK**: Agent Development Kit for building agentic applications
- **Google Gemini**: LLM model (gemini-2.5-flash-lite)
- **Python**: Core programming language

## Repository Structure

```
ai-agents-course/
├── lesson1/          # Agent composition patterns
│   ├── my_agent/     # Hierarchical agent coordination
│   ├── seq_agent/    # Sequential execution
│   ├── para_agent/   # Parallel execution
│   └── loop_agent/   # Iterative refinement
├── lesson2/          # Tool integration
│   ├── agent_tools/  # Custom functions & code executor
│   └── agent_tools2/ # MCP & resumable workflows
├── lesson3/          # State management
│   ├── agent_sessions/  # Session persistence
│   └── agent_memory/    # Long-term memory
└── lesson4/          # (Coming soon)
```

## Documentation

For detailed architecture and development patterns, see [CLAUDE.md](CLAUDE.md).
