# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Google ADK (Agent Development Kit) learning repository containing progressive lessons on building AI agents with Google's Gemini models. The repository demonstrates agent patterns from basic single agents to complex multi-agent workflows with memory and persistence.

## Technology Stack

- **Framework**: Google ADK (Agent Development Kit)
- **Models**: Google Gemini (gemini-2.5-flash-lite)
- **Python Version**: 3.12
- **Key Dependencies**: google-adk, ipython

## Development Commands

### Environment Setup
```bash
# Install dependencies at project root
pip install -r requirements.txt

# Or install dependencies for individual lessons
cd lesson1  # or lesson2, lesson3
pip install -r requirements.txt
```

### Running Agents
```bash
# Agents are Python scripts that can be run directly
python lesson1/my_agent/agent.py
python lesson2/agent_tools/agent.py
python lesson3/agent_sessions/agent.py

# Most agent scripts have commented-out asyncio.run() calls at the bottom
# Uncomment the relevant section to run the agent
```

## Architecture Overview

### Lesson 1: Agent Composition Patterns

**Location**: `lesson1/`

Core agent orchestration patterns:
- **my_agent**: Root agent coordinating sub-agents as tools using `AgentTool`
- **seq_agent**: Sequential agents executing tasks in order using `SequentialAgent`
- **para_agent**: Parallel agents executing tasks concurrently using `ParallelAgent`
- **loop_agent**: Iterative refinement using `LoopAgent` with exit conditions

Key pattern: Agents can be wrapped in `AgentTool` to become callable tools for other agents, enabling hierarchical agent architectures.

### Lesson 2: Tool Integration

**Location**: `lesson2/`

- **agent_tools**: Demonstrates custom function tools (fee lookup, exchange rates) and agent-as-tool pattern with code executor
- **agent_tools2**: MCP (Model Context Protocol) integration and resumable apps with human-in-the-loop approval workflows

Critical concept: Tools can pause execution via `ToolContext.request_confirmation()` for human approval, then resume with `invocation_id`.

### Lesson 3: State Management

**Location**: `lesson3/`

State persistence strategies:
- **agent_sessions**: Session management with `InMemorySessionService` (temporary) and `DatabaseSessionService` (SQLite persistence)
- **agent_memory**: Long-term memory with `InMemoryMemoryService`, `load_memory`/`preload_memory` tools, and automatic memory callbacks

Architecture note: Sessions track conversation history within a single interaction; Memory enables cross-session information retrieval.

### Lesson 4

**Location**: `lesson4/` - Empty directory for future content

## Key ADK Patterns

### Agent Types
- `Agent`: Basic LLM agent
- `LlmAgent`: Extended agent with additional features
- `SequentialAgent`: Executes sub-agents in sequence
- `ParallelAgent`: Executes sub-agents concurrently
- `LoopAgent`: Executes sub-agents repeatedly until exit condition

### Tool Integration
```python
# Function as tool
tools=[function_name]

# Agent as tool
tools=[AgentTool(agent_instance)]

# With tool context for resumability
def my_tool(param: str, tool_context: ToolContext) -> dict:
    tool_context.request_confirmation(hint="...", payload={...})
```

### State Services
- `InMemorySessionService`: Volatile session storage
- `DatabaseSessionService`: Persistent session storage (SQLite)
- `InMemoryMemoryService`: Long-term memory storage
- `Runner`: Orchestrates agent execution with session/memory services

### Retry Configuration
All agents use consistent retry configuration:
```python
retry_config = types.HttpRetryOptions(
    attempts=5,
    exp_base=7,
    initial_delay=1,
    http_status_codes=[429, 500, 503, 504]
)
```

## Common Patterns

### Agent Instruction Design
- Explicit step-by-step instructions with numbered workflows
- Clear exit conditions for loop agents ("APPROVED" keyword pattern)
- Tool usage mandates ("you MUST call...")

### State Key Pattern
- `output_key` parameter stores agent results in session state
- State values accessible in instructions via template: `{state_key_name}`

### Resumability Pattern
1. Tool requests confirmation via `tool_context.request_confirmation()`
2. Agent execution pauses with `adk_request_confirmation` event
3. Human provides approval decision
4. Agent resumes with same `invocation_id`

### Memory Pattern
1. Add sessions to memory: `memory_service.add_session_to_memory(session)`
2. Provide `preload_memory` tool to agent for automatic memory loading
3. Use `after_agent_callback` for automatic memory persistence

## Environment Variables
- `GOOGLE_GENAI_USE_VERTEXAI="FALSE"`: Use Gemini API instead of Vertex AI
- `GOOGLE_API_KEY`: Required for Google Gemini API access (typically set in Kaggle secrets)
