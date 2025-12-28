# What We Did

## Claude Code Plugins

### Installed
- **frontend-design** - Production-grade UI design, auto-activates for frontend work

### Available (Not Installed)
- **security-guidance** - Block security vulnerabilities (XSS, SQL injection, eval)
- **commit-commands** - `/commit`, `/commit-push-pr` commands
- **pr-review-toolkit** - 6 specialized PR review agents
- **feature-dev** - 7-phase feature workflow

## MCP Servers

### Context7
- Library documentation lookup
- Usage: Ask about Vercel AI, Drizzle, TanStack Query, NextAuth

### PostgreSQL (via Docker)
- Database queries, schema inspection
- Running on port 8082
- Config in `docker-compose.yml`

## Chrome Integration

- Extension: "Claude in Chrome" (install from Chrome Web Store)
- Usage: `claude --chrome`
- Features: Live debugging, real browser control

## Configuration

**File**: `.claude/mcp_settings.json`
```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upshift-dev/context7-mcp-server"]
    },
    "postgres": {
      "type": "sse",
      "url": "http://localhost:8082/sse"
    }
  }
}
```

## How to Use

1. Install plugin: `claude plugin install frontend-design@claude-code-plugins`
2. Start PostgreSQL MCP: `docker compose up -d`
3. Copy `.claude/mcp_settings.json` to your project
4. Start Claude: `claude --chrome`
