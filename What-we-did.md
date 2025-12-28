# What We Did - 2025-12-27

## Claude Code Plugins

### Installed Plugins ✅

#### 1. Frontend Design Plugin
- **Source**: `frontend-design@claude-code-plugins`
- **Scope**: User (available for all projects)
- **Auto-activates**: When building UI components, pages, or applications
- **Provides**:
  - Bold aesthetic choices (avoiding generic AI designs)
  - Distinctive typography and color palettes
  - Production-grade animations and visual details
  - Context-aware design thinking
- **Usage**: Simply describe frontend needs - plugin auto-activates
  - Example: "Create a patient dashboard for clinical trials"
  - Example: "Build a trial search interface with filters"

### Available Official Plugins (Not Yet Installed)

#### 🔒 Security & Code Quality
- **security-guidance** - Warns about security issues (XSS, SQL injection, eval, etc.) - **CRITICAL for medical app**
- **code-review** - 5 parallel agents for PR review with confidence scoring
- **pr-review-toolkit** - 6 specialized review agents (comments, tests, errors, types, quality, simplification)

#### ⚡ Git Workflow
- **commit-commands** - `/commit`, `/commit-push-pr`, `/clean_gone` commands

#### 🛠️ Development
- **feature-dev** - 7-phase feature development workflow with code-explorer, code-architect, code-reviewer agents
- **typescript-lsp** - TypeScript/JavaScript language server support
- **pyright-lsp** - Python language server support

#### 🎓 Learning & Hooks
- **hookify** - Create custom behavior-blocking hooks
- **learning-output-style** - Interactive learning mode
- **explanatory-output-style** - Educational insights
- **ralph-wiggum** - Self-referential AI loops for iterative development

#### 📦 Development Tools
- **plugin-dev** - Plugin development toolkit
- **agent-sdk-dev** - Claude Agent SDK development kit

### Plugin Marketplaces Configured
- `claude-code-plugins` (anthropics/claude-code) - 13 plugins
- `claude-plugins-official` (anthropics/claude-plugins-official) - 23 plugins (includes LSP servers)

## MCP Server Setup

### PostgreSQL MCP
- Already running via Docker on port 8082 (see `docker-compose.yml` lines 108-119)
- Container: `clinical-trial-matcher-postgres-mcp-1`
- Endpoint: `http://localhost:8082/sse`
- Provides: Database queries, schema inspection, EXPLAIN analysis

### Context7 MCP
- Configured to run via npx (auto-installs on demand)
- Provides: Library documentation lookup for Vercel AI, Drizzle, TanStack Query, NextAuth, etc.

### Chrome Integration (Using This, Not Puppeteer)
- Extension: "Claude in Chrome" (already installed)
- Native host: Running (`--chrome-native-host` process verified)
- Provides: Live debugging, design verification, authenticated web apps, session recording
- Usage: Start with `claude --chrome` or enable by default via `/chrome` command
- **Note**: Using Claude Code's native Chrome integration instead of Puppeteer MCP for better debugging and real browser control

### Configuration
Created project-specific MCP config at `.claude/mcp_settings.json`:
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

Updated `.gitignore` to:
- Ignore `.claude/*` session data
- Keep `mcp_settings.json` in version control

## How to Use

**Start session with all features:**
```bash
claude --chrome
```

**Or enable Chrome by default:**
- Run `/chrome` command in any session
- Select "Enable by default"
