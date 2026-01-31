# Docker cagent Examples

A collection of ready-to-use agent configurations for [Docker cagent](https://github.com/docker/cagent) - the Agent Builder and Runtime by Docker Engineering.

## Prerequisites

### Installation

Docker cagent comes bundled with Docker Desktop 4.49.0+. Alternatively:

```bash
# Homebrew
brew install cagent

# Or download from GitHub releases
# https://github.com/docker/cagent/releases
```

### API Keys

Set up your API keys based on which models you want to use:

```bash
# OpenAI (for GPT-4, GPT-4o, etc.)
export OPENAI_API_KEY=your_openai_key

# Anthropic (for Claude models)
export ANTHROPIC_API_KEY=your_anthropic_key

# Google (for Gemini models)
export GOOGLE_API_KEY=your_google_key
```

## Available Examples

### Basic Examples

| File | Description | Models |
|------|-------------|--------|
| `basic-agent.yaml` | Simplest possible agent configuration | OpenAI |
| `pirate-agent.yaml` | Fun personality example - speaks like a pirate | OpenAI |
| `local-agent.yaml` | Runs entirely locally using Docker Model Runner | DMR (no API key needed) |

### Advanced Examples

| File | Description | Models |
|------|-------------|--------|
| `coding-agent.yaml` | Coding assistant with file system access | Anthropic/OpenAI |
| `research-agent.yaml` | Web search and research capabilities | OpenAI + MCP tools |
| `rag-agent.yaml` | Knowledge base with document retrieval | OpenAI + RAG |

### Multi-Agent Teams

| File | Description | Models |
|------|-------------|--------|
| `dev-assistant.yaml` | Development team with coordinator, reviewer, researcher, and doc writer | OpenAI + Anthropic |
| `customer-support-team.yaml` | Support team with triage, technical, billing, and escalation agents | OpenAI + Anthropic |

## Quick Start

### 1. Run a Basic Agent

```bash
cagent run basic-agent.yaml
```

### 2. Run the Development Team

```bash
# Start with the root coordinator
cagent run dev-assistant.yaml

# Or start with a specific agent
cagent run dev-assistant.yaml -a code_reviewer
```

### 3. Run the Research Agent (with web search)

```bash
cagent run research-agent.yaml
```

### 4. Run a Local Model (no API key needed)

```bash
# Make sure Docker Model Runner is enabled in Docker Desktop
cagent run local-agent.yaml
```

### 5. Use the RAG Agent

```bash
# Add your documents to the 'documents' folder
cp your-docs/* documents/

# Run the agent
cagent run rag-agent.yaml
```

## Example Sessions

### Basic Agent

```
$ cagent run basic-agent.yaml

You: What is Docker cagent?

Agent: Docker cagent is an Agent Builder and Runtime by Docker Engineering. 
It lets you create and run intelligent AI agents, where each agent can have 
specialized knowledge, tools, and capabilities. Think of it as a way to build 
a team of virtual experts that collaborate to solve complex problems.

Key features include:
- Multi-agent architecture for specialized agents
- MCP tool ecosystem for external APIs
- Smart delegation between agents
- YAML configuration (no code required)
- Support for multiple AI providers
```

### Development Team

```
$ cagent run dev-assistant.yaml

You: Review this Python function for potential issues:
def calculate_average(numbers):
    total = 0
    for n in numbers:
        total += n
    return total / len(numbers)

Agent: I'll delegate this to our code reviewer for a thorough analysis.

[Delegating to code_reviewer...]

Code Reviewer: ## Code Review Summary

### Critical Issues 🔴
1. **Division by Zero** (Line 5)
   - `len(numbers)` could be 0 for empty lists
   - Fix: Add a check for empty input

### Improvements 🟡
2. **Use Built-in Functions**
   - Python's `sum()` is more efficient and readable
   
### Suggested Fix:
```python
def calculate_average(numbers):
    if not numbers:
        raise ValueError("Cannot calculate average of empty list")
    return sum(numbers) / len(numbers)
```

### Positive Highlights ⭐
- Clear function name
- Simple, readable logic
```

## Customizing Agents

### Change the Model

```yaml
agents:
  root:
    model: anthropic/claude-sonnet-4-20250514  # Use Claude instead
    # or
    model: openai/gpt-4o-mini  # Faster, cheaper
    # or
    model: dmr/ai/qwen3:8B  # Local model
```

### Add MCP Tools

```yaml
agents:
  root:
    model: openai/gpt-4o
    toolsets:
      # Docker MCP Gateway tools
      - type: mcp
        ref: docker:duckduckgo
      
      # Custom MCP server
      - type: mcp
        command: npx
        args: ["-y", "@modelcontextprotocol/server-filesystem", "./"]
```

### Add Sub-Agents

```yaml
agents:
  root:
    model: openai/gpt-4o
    sub_agents: [helper, specialist]
  
  helper:
    model: openai/gpt-4o-mini
    description: General helper
  
  specialist:
    model: anthropic/claude-sonnet-4-20250514
    description: Domain expert
```

## Directory Structure

```
cagent/
├── README.md                    # This file
├── basic-agent.yaml             # Simple single agent
├── pirate-agent.yaml            # Fun personality example
├── local-agent.yaml             # Docker Model Runner example
├── coding-agent.yaml            # Coding assistant
├── research-agent.yaml          # Web research agent
├── rag-agent.yaml               # Knowledge base agent
├── dev-assistant.yaml           # Development team
├── customer-support-team.yaml   # Support team
└── documents/                   # Documents for RAG
    └── sample-docs.md           # Sample documentation
```

## Useful Commands

```bash
# Run an agent
cagent run <agent.yaml>

# Run with a specific starting agent
cagent run <agent.yaml> -a <agent-name>

# Generate a new agent interactively
cagent new

# Pull an agent from Docker Hub
cagent pull <user>/<agent>

# Push your agent to Docker Hub
cagent push <agent.yaml> <user>/<agent>

# Expose agents as MCP tools
cagent mcp <agent.yaml>

# Get help
cagent --help
```

## Resources

- [Docker cagent GitHub](https://github.com/docker/cagent)
- [Official Documentation](https://github.com/docker/cagent/blob/main/USAGE.md)
- [MCP Toolkit](https://www.docker.com/products/mcp-toolkit/)
- [Docker Model Runner](https://docs.docker.com/desktop/features/model-runner/)

## License

These examples are provided under the Apache 2.0 License, same as Docker cagent.
