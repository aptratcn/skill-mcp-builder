# 🔌 MCP Builder

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Skill](https://img.shields.io/badge/Skill-MCP_Builder-blue.svg)]()

> **Build production-ready MCP servers in hours, not weeks.**

## The Problem

MCP is the new standard for AI agent tools — but building servers is painful:

- 📖 **Spec is 100+ pages** — Hours of reading before writing a single line
- 🐛 **Error handling is non-obvious** — Timeouts, streaming, partial failures all behave differently
- 🔒 **Security is an afterthought** — Most tutorials skip auth, input validation, and rate limiting
- 🔀 **Dual-language chaos** — Need both TypeScript AND Python patterns? Good luck finding both

**Result:** Your first MCP server takes 3-5 days instead of 3-5 hours.

## The Solution

MCP Builder gives you **battle-tested patterns** so you ship servers, not read specs:

| What | What You Get |
|------|-------------|
| 🏗️ **Architecture** | TypeScript + Python boilerplates with all best practices |
| 📊 **CRUD + Pagination** | Production patterns that handle real data volumes |
| ⏳ **Long-running Ops** | Streaming, progress reporting, cancellation |
| 🛡️ **Security Checklist** | Input validation, rate limiting, auth patterns |
| 🚀 **Deployment** | One-command setup for Claude Code, Cursor, Gemini CLI |
| 🔗 **Error Handling** | Graceful degradation, retry logic, user-friendly messages |

## Quick Start

```bash
# Via Vercel Skills CLI
npx skills add aptratcn/skill-mcp-builder

# Or copy directly
cp SKILL.md ~/.claude/skills/mcp-builder/
```

## Why MCP?

Model Context Protocol is the **USB-C of AI tools** — one standard, infinite integrations. Every agent tool you build works across Claude Code, Codex, Cursor, Gemini CLI, and 40+ more.

```
Without MCP:  Tool → Claude Code only
With MCP:     Tool → Claude Code + Cursor + Codex + Gemini + 40+ more
```

## What You'll Build

### TypeScript Server (5 min)
```typescript
import { McpServer } from "@modelcontextprotocol/sdk";

const server = new McpServer({ name: "my-tool", version: "1.0.0" });

server.tool("search", { query: z.string() }, async ({ query }) => {
  const results = await searchDatabase(query);
  return { content: [{ type: "text", text: JSON.stringify(results) }] };
});
```

### Python Server (5 min)
```python
from mcp.server import Server

server = Server("my-tool")

@server.tool("search")
async def search(query: str) -> str:
    results = await search_database(query)
    return json.dumps(results)
```

## Real Impact

| Metric | Without MCP Builder | With MCP Builder |
|--------|--------------------|--------------------|
| Time to first server | 3-5 days | 3-5 hours |
| Security coverage | Maybe 30% | 90%+ (checklist) |
| Error handling | Ad-hoc | Systematic patterns |
| Cross-framework compat | Manual testing | Verified patterns |

## Related Skills

- [**MCP Security Audit**](https://github.com/aptratcn/skill-mcp-security-audit) — Audit your MCP servers for security issues
- [**Token Budget Guard**](https://github.com/aptratcn/token-budget-guard) — Optimize MCP token usage
- [**Skill Health Monitor**](https://github.com/aptratcn/skill-health-monitor) — Monitor your skill collection health

---

*Part of [Awesome Zero-Dependency Skills](https://github.com/aptratcn/awesome-zero-dependency-skills)*
