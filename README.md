# Next.js Claude Code Blueprint

---

## Why This Repo?

I tried 10+ repositories (Agent OS, various boilerplates, community configs) to set up Claude Code for Next.js development. **None satisfied me.**

The problem: Too many agents, slash commands, and generalized docs. They duplicate what Claude Code already does (like `/create-feature` that launches 3 subagents to explore code, when Claude already does this when you say "create feature"). Bloated configs eat your context window and fight against defaults.

**This Repo's Philosophy:**
> **Only add what's needed for agentic coding with Next.js. Nothing else.**

Claude Code can't provide Next.js-specific development workflow configs because it's framework-agnostic. This repo fills that gap.

We add:
- ✅ Next.js-specific MCP servers (PostgreSQL, Context7 for Next.js docs)
- ✅ Plugins for Next.js development (frontend-design)
- ✅ Docker configs for local Next.js dev environment

We DON'T add:
- ❌ What Claude Code already has (explore agents, planning, Task tool)
- ❌ Generalized configs that work for any language
- ❌ Custom slash commands that duplicate defaults

**Clean. Minimal. Works.**

---

## What's Included

```
.claude/
└── mcp_settings.json       # PostgreSQL MCP + Context7 MCP

docker-compose.yml          # PostgreSQL MCP server
What-we-did.md             # Setup log
```

---

## Quick Start

### 1. Install Claude Code Plugin
```bash
claude plugin install frontend-design@claude-code-plugins
```

### 2. Start PostgreSQL MCP (Optional)
```bash
docker compose up -d
```

### 3. Copy Config
```bash
cp .claude/mcp_settings.json your-project/.claude/
```

### 4. Restart Claude Code
```bash
claude --chrome
```

Done.

---

## What's Configured

**MCP Servers:**
- PostgreSQL (database queries, schema inspection)
- Context7 (library documentation lookup)

**Plugins:**
- frontend-design (production-grade UI, auto-activates)

**Chrome Integration:**
- Live debugging, real browser control

---

## What's NOT Included (By Design)

- ❌ Custom explore agents (Claude Code has Explore built-in)
- ❌ Dozens of slash commands (use Claude's defaults)
- ❌ Complex agent orchestration (use Task tool)
- ❌ Generic skills that add noise

If Claude Code can do it by default, we don't recreate it.

---

## Philosophy

**Simple > Complex**
**Minimal > Maximal**
**Works > Theoretical**

This is not a framework. It's a clean starting point.

---

## What's Missing?

This is a minimal starting point. For production Next.js full-stack development, see:

**[📋 MISSING.md](./MISSING.md)** - Complete guide on what to add next

**Quick Overview:**
- TypeScript LSP plugin (critical for Next.js development)
- Security scanning plugin
- Hooks configuration for automated quality gates
- Additional MCP servers (Vercel, GitHub, Sentry)
- Custom skills for workflow automation
- CI/CD setup
- Testing configuration

**Implementation Priority:**
1. **Phase 1 (Critical):** TypeScript LSP, security plugin, .env setup, hooks
2. **Phase 2 (High Value):** Automation scripts, more MCP servers, workflows doc
3. **Phase 3 (Enhancement):** Custom skills, example app, CI/CD

See [MISSING.md](./MISSING.md) for detailed setup instructions and rationale.

---

## Contribute

Found something essential that's missing? Open an issue.

Adding unnecessary complexity? No thanks.
