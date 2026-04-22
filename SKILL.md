---
name: mcp-builder
version: 1.0.0
description: Build production-ready MCP servers for any AI agent. Complete guide with patterns for tools, resources, prompts, and real-world integrations. Trigger on: 'build MCP', 'create MCP server', 'MCP tool', 'add tool to agent'.
emoji: 🔌
tags: [mcp, model-context-protocol, agent-tools, integration, server-development]
---

# MCP Builder 🔌

Build production-ready MCP servers that connect AI agents to the real world.

## What is MCP?

Model Context Protocol (MCP) is the open standard for connecting AI assistants to tools, data sources, and services. It's the USB-C plug for AI — one protocol, infinite integrations.

**Supported by:** Claude Code, Codex CLI, Cursor, Gemini CLI, Zed, Windsurf, and 40+ agents.

## Core Concepts

Three primitive types:
- **Tools** — Functions the agent can call (read/write, compute, act)
- **Resources** — Data the agent can read (files, APIs, configs)
- **Prompts** — Reusable prompt templates with variables

## Quick Start: TypeScript

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new McpServer({ name: "my-server", version: "1.0.0" });

server.tool("get_weather", "Get current weather for a location",
  { location: { type: "string", description: "City name" } },
  async ({ location }) => {
    const data = await fetchWeather(location);
    return { content: [{ type: "text", text: JSON.stringify(data) }] };
  }
);

const transport = new StdioServerTransport();
await server.connect(transport);
```

## Quick Start: Python

```python
from mcp.server.fastmcp import FastMCP
mcp = FastMCP("my-server")

@mcp.tool()
def get_weather(location: str) -> str:
    """Get current weather for a location."""
    return json.dumps(fetch_weather(location))

mcp.run(transport="stdio")
```

## Tool Design Patterns

### CRUD Operations
```typescript
server.tool("create_ticket", "Create a support ticket", {
  title: { type: "string" },
  priority: { type: "string", enum: ["low", "medium", "high"] }
}, async ({ title, priority }) => {
  const ticket = await db.tickets.create({ title, priority });
  return { content: [{ type: "text", text: JSON.stringify({ id: ticket.id }) }] };
});
```

### Paginated Search
```typescript
server.tool("search_docs", "Search documentation", {
  query: { type: "string" },
  limit: { type: "number", default: 10 }
}, async ({ query, limit }) => {
  const results = await searchEngine.query(query, { limit });
  return { content: [{ type: "text", text: JSON.stringify({
    results: results.items,
    hasMore: results.hasMore
  }) }] };
});
```

## Error Handling

```typescript
server.tool("risky_operation", "Operation that might fail", {
  input: { type: "string" }
}, async ({ input }) => {
  try {
    const result = await performOperation(input);
    return { content: [{ type: "text", text: JSON.stringify(result) }] };
  } catch (error) {
    return {
      content: [{ type: "text", text: `Error: ${error.message}` }],
      isError: true
    };
  }
});
```

## Security Checklist

- [ ] Path traversal prevention
- [ ] Input validation on all tools
- [ ] Rate limiting for expensive operations
- [ ] Never expose API keys in descriptions
- [ ] Least privilege for all tools
- [ ] No shell injection from user input
- [ ] Timeout on all external calls

## Deployment

### Claude Code
```bash
claude mcp add my-server -e API_KEY=sk-xxx -- npx @scope/my-server
```

### Cursor / Gemini CLI
```json
{
  "mcpServers": {
    "my-server": {
      "command": "npx",
      "args": ["@scope/my-server"],
      "env": { "API_KEY": "sk-xxx" }
    }
  }
}
```

## Common Pitfalls

1. **Blocking the event loop** — Use async/await
2. **Huge responses** — Truncate to prevent context overflow
3. **Missing descriptions** — Good descriptions = better agent usage
4. **Inconsistent error format** — Always use `isError: true`
5. **Ignoring stdio transport** — Most agents use stdio, not HTTP

---

*Zero dependencies. Copy, paste, build.*
