# Sample Documentation for RAG Agent

This is a sample document to demonstrate the RAG (Retrieval-Augmented Generation) capabilities of Docker cagent.

## Getting Started

Docker cagent is a powerful multi-agent runtime that lets you create and run intelligent AI agents. Each agent can have specialized knowledge, tools, and capabilities.

## Key Concepts

### Agents
Agents are AI-powered assistants defined in YAML configuration files. Each agent has:
- A model (the LLM to use)
- A description (what the agent does)
- Instructions (how the agent should behave)
- Optional toolsets (MCP tools the agent can use)
- Optional sub_agents (other agents it can delegate to)

### Models
Models are configured in the `models` section of your YAML file. Supported providers include:
- OpenAI (gpt-4o, gpt-4o-mini, etc.)
- Anthropic (claude-sonnet-4, claude-opus-4, etc.)
- Google (gemini-2.5-flash, gemini-2.5-pro, etc.)
- Docker Model Runner (local models)

### MCP Tools
MCP (Model Context Protocol) tools extend agent capabilities. Common tools include:
- File system access (read/write files)
- Web search (DuckDuckGo)
- Fetch (retrieve web pages)
- Shell (execute commands)

### RAG (Retrieval-Augmented Generation)
RAG allows agents to search through your documents to find relevant information before responding. This grounds responses in your specific knowledge base.

## Common Commands

```bash
# Run an agent
cagent run my-agent.yaml

# Run with a specific starting agent
cagent run multi-agent.yaml -a specific-agent

# Generate a new agent
cagent new

# Pull an agent from Docker Hub
cagent pull creek/pirate

# Push an agent to Docker Hub
cagent push my-agent.yaml username/my-agent
```

## Troubleshooting

### API Key Issues
Make sure you've exported the correct API key for your chosen provider:
```bash
export OPENAI_API_KEY=your_key
export ANTHROPIC_API_KEY=your_key
```

### MCP Tool Errors
Ensure Docker Desktop is running and MCP Toolkit is enabled for containerized MCP servers.

### Model Not Found
Check that the model name in your YAML matches the provider's model naming convention.
