# What's Missing - Next.js Full-Stack Setup

This document outlines what's currently missing from this blueprint for production-ready Next.js full-stack development with Claude Code.

---

## Critical Missing Components

### 1. Essential Plugins (Not Installed)

#### TypeScript/JavaScript Support
```bash
# CRITICAL for Next.js - autocomplete, type checking, refactoring
claude plugin install typescript-lsp@claude-plugins-official
```

**Why:** Without TypeScript LSP, Claude Code lacks:
- Real-time type errors
- Intelligent autocomplete
- Safe refactoring across files
- Jump-to-definition in large codebases

#### Security Scanning
```bash
# Warns about XSS, SQL injection, insecure dependencies
claude plugin install security-guidance@claude-code-plugins
```

**Why:** Catches security vulnerabilities during development:
- Prevents `eval()` usage
- Detects SQL injection patterns
- Warns about XSS vulnerabilities
- Identifies insecure dependencies

#### Code Review Automation
```bash
# 6 specialized review agents (comments, tests, errors, types, quality)
claude plugin install pr-review-toolkit@claude-code-plugins
```

**Why:** Automated PR reviews with:
- Test coverage analysis
- Type safety validation
- Code quality scoring
- Simplification suggestions

#### Git Workflow
```bash
# /commit, /commit-push-pr, /clean_gone commands
claude plugin install commit-commands@claude-code-plugins
```

**Why:** Streamlines git operations:
- Smart commit message generation
- One-command PR creation
- Branch cleanup automation

---

## 2. Hooks Configuration

**Missing:** `.claude/hooks.json`

Hooks run automatically on events - enforce quality without manual checks.

### Recommended Hooks

```json
{
  "pre-commit": "npm run lint:fix && npm run type-check",
  "pre-push": "npm run test && npm run build",
  "post-checkout": "npm install",
  "user-prompt-submit": "echo '🔍 Security scan enabled' && npm audit --audit-level=moderate"
}
```

**Benefits:**
- `pre-commit`: Catch lint/type errors before committing
- `pre-push`: Prevent broken code from reaching remote
- `post-checkout`: Auto-install dependencies on branch switch
- `user-prompt-submit`: Run security checks before Claude executes

---

## 3. Additional MCP Servers

### Vercel MCP (Deployment)
```json
{
  "mcpServers": {
    "vercel": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-vercel"],
      "env": {
        "VERCEL_TOKEN": "${VERCEL_TOKEN}"
      }
    }
  }
}
```

**Capabilities:**
- Deploy preview environments
- Check deployment status
- Manage environment variables
- View build logs

### Sequential Thinking MCP (Complex Reasoning)
```json
{
  "sequential-thinking": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
  }
}
```

**Use Cases:**
- Complex architecture decisions
- Multi-step database migrations
- Performance optimization planning

### GitHub MCP (Project Management)
```json
{
  "github": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-github"],
    "env": {
      "GITHUB_TOKEN": "${GITHUB_TOKEN}"
    }
  }
}
```

**Features:**
- Create/manage issues
- Review PRs
- Manage project boards
- Search code across repositories

### Sentry MCP (Error Tracking)
```json
{
  "sentry": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-sentry"],
    "env": {
      "SENTRY_AUTH_TOKEN": "${SENTRY_AUTH_TOKEN}"
    }
  }
}
```

**Debug Capabilities:**
- Fetch error traces
- Analyze error patterns
- Track performance issues
- Link errors to commits

---

## 4. Settings Configuration

**Missing:** `.claude/settings.json`

### Recommended Settings

```json
{
  "preferredModel": "sonnet",
  "chromeEnabled": true,
  "planningMode": {
    "autoEnter": true,
    "minComplexity": 3
  },
  "codeStyle": {
    "maxLineLength": 100,
    "indent": 2,
    "quotes": "single",
    "semicolons": false
  },
  "mcpServers": {
    "autoConnect": true,
    "retryAttempts": 3,
    "timeout": 30000
  },
  "agents": {
    "exploreDepth": "medium",
    "planAlways": false,
    "parallelExecution": true
  }
}
```

---

## 5. Custom Skills (Workflow Automation)

**Missing:** `.claude/skills/` directory

### Recommended Skills

#### `/test-api` - API Route Testing
```typescript
// .claude/skills/test-api.ts
export default {
  name: 'test-api',
  description: 'Test Next.js API routes with curl',
  async execute(route: string) {
    // curl localhost:3000/api/{route}
    // Parse response, validate schema
    // Run assertions
  }
}
```

#### `/db-migrate` - Database Migration Helper
```typescript
// .claude/skills/db-migrate.ts
export default {
  name: 'db-migrate',
  description: 'Generate and run Drizzle migrations',
  async execute(message: string) {
    // npm run db:generate
    // Review migration SQL
    // Apply with confirmation
  }
}
```

#### `/deploy-preview` - Vercel Preview Deploy
```typescript
// .claude/skills/deploy-preview.ts
export default {
  name: 'deploy-preview',
  description: 'Deploy to Vercel preview environment',
  async execute() {
    // Commit changes
    // Push to branch
    // Trigger Vercel deployment
    // Return preview URL
  }
}
```

#### `/security-scan` - Full Security Audit
```typescript
// .claude/skills/security-scan.ts
export default {
  name: 'security-scan',
  description: 'Run comprehensive security checks',
  async execute() {
    // npm audit
    // Check for exposed secrets
    // Validate env vars
    // Scan dependencies
  }
}
```

---

## 6. Security & Environment Setup

### Missing `.env.example`

```bash
# Database
POSTGRES_USER=postgres
POSTGRES_PASSWORD=your-secure-password-here
POSTGRES_DB=your-db-name
DATABASE_URL=postgresql://postgres:password@localhost:5432/your-db-name

# Next.js
NEXT_PUBLIC_APP_URL=http://localhost:3000
NODE_ENV=development

# Authentication (if using NextAuth)
NEXTAUTH_SECRET=your-nextauth-secret
NEXTAUTH_URL=http://localhost:3000

# MCP Servers
VERCEL_TOKEN=your-vercel-token
GITHUB_TOKEN=your-github-token
SENTRY_AUTH_TOKEN=your-sentry-token

# Optional
OPENAI_API_KEY=your-openai-key
```

### Update `docker-compose.yml`

```yaml
# ❌ CURRENT: Hardcoded passwords
environment:
  POSTGRES_PASSWORD: your-password-here

# ✅ SHOULD BE:
environment:
  POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
  POSTGRES_USER: ${POSTGRES_USER:-postgres}
  POSTGRES_DB: ${POSTGRES_DB}

# Add health checks
healthcheck:
  test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER}"]
  interval: 10s
  timeout: 5s
  retries: 5
```

---

## 7. Automation Scripts

### Missing `setup.sh`

```bash
#!/bin/bash
set -e

echo "🚀 Setting up Next.js Claude Code environment..."

# Install essential plugins
echo "📦 Installing Claude Code plugins..."
claude plugin install frontend-design@claude-code-plugins
claude plugin install typescript-lsp@claude-plugins-official
claude plugin install security-guidance@claude-code-plugins
claude plugin install pr-review-toolkit@claude-code-plugins
claude plugin install commit-commands@claude-code-plugins

# Copy environment template
if [ ! -f .env ]; then
  echo "📝 Creating .env from template..."
  cp .env.example .env
  echo "⚠️  Update .env with your actual credentials"
fi

# Start Docker services
echo "🐳 Starting Docker services..."
docker compose up -d

# Wait for PostgreSQL to be ready
echo "⏳ Waiting for PostgreSQL..."
sleep 5

# Verify MCP connections
echo "✅ Verifying MCP servers..."
claude mcp list

echo ""
echo "✨ Setup complete!"
echo ""
echo "Next steps:"
echo "  1. Update .env with your credentials"
echo "  2. Start Claude Code: claude --chrome"
echo "  3. Run database migrations: npm run db:migrate"
echo ""
```

### Missing `Makefile` (Alternative)

```makefile
.PHONY: setup install start stop clean test

setup: ## Initial setup - install plugins and start services
	@bash setup.sh

install: ## Install dependencies
	npm install

start: ## Start development server and Docker services
	docker compose up -d
	npm run dev

stop: ## Stop all services
	docker compose down
	pkill -f "next dev" || true

clean: ## Clean all generated files
	rm -rf .next node_modules dist
	docker compose down -v

test: ## Run all tests
	npm run lint
	npm run type-check
	npm run test

migrate: ## Generate and run database migrations
	npm run db:generate
	npm run db:migrate

help: ## Show this help
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-15s\033[0m %s\n", $$1, $$2}'
```

---

## 8. Usage Documentation

### Missing `WORKFLOWS.md`

Should include:

#### Database Operations via MCP
```
"Show me the schema for the users table"
"Explain this query: SELECT * FROM users WHERE created_at > NOW() - INTERVAL '7 days'"
"What indexes exist on the posts table?"
```

#### Documentation Lookup with Context7
```
"Look up Next.js App Router server actions documentation"
"Find Drizzle ORM examples for one-to-many relationships"
"How do I use TanStack Query with Next.js 14?"
```

#### Frontend Development with Plugin
```
"Create a dashboard with sidebar, data table, and charts"
"Build a patient registration form with validation"
"Design a landing page for a SaaS product"
```

#### Chrome Debugging
```
"Debug the authentication flow on localhost:3000"
"Test the checkout process and record as GIF"
"Inspect network requests for the API calls"
```

#### Agent Coordination
```
"Use the Explore agent to understand the auth system"
"Enter plan mode to design the comment system"
"Launch parallel agents to review this PR"
```

---

## 9. Testing & Quality Gates

### Missing Test Configuration

#### Vitest Config
```typescript
// vitest.config.ts
import { defineConfig } from 'vitest/config'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    setupFiles: ['./vitest.setup.ts'],
    coverage: {
      provider: 'v8',
      reporter: ['text', 'html'],
      exclude: ['node_modules/', '.next/']
    }
  }
})
```

#### Pre-commit Hook with Husky
```bash
# Install husky
npm install -D husky lint-staged

# Setup
npx husky init
```

```json
// package.json
{
  "lint-staged": {
    "*.{ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{json,md}": [
      "prettier --write"
    ]
  }
}
```

---

## 10. CI/CD Configuration

### Missing `.github/workflows/ci.yml`

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:17
        env:
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: test
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - run: npm ci

      - run: npm run lint

      - run: npm run type-check

      - run: npm run test

      - run: npm run build
```

---

## 11. Example Project Structure

### Missing Minimal Next.js App

Should include:

```
src/
├── app/
│   ├── api/
│   │   └── health/route.ts        # Health check endpoint
│   ├── layout.tsx
│   └── page.tsx                    # Dashboard example
├── db/
│   ├── schema.ts                   # Drizzle schema
│   └── index.ts                    # Database client
├── lib/
│   └── utils.ts                    # Shared utilities
└── components/
    └── ui/                         # shadcn/ui components
```

---

## 12. Agent Coordination Patterns

### Missing Documentation on Agent Usage

#### When to Use Explore Agent
```
❌ "Find the user authentication code"
✅ Use Explore agent for: "Understand the authentication system architecture"
```

#### When to Enter Plan Mode
```
❌ Simple tasks: "Add a console.log"
✅ Complex tasks: "Implement OAuth2 with Google, GitHub, and email"
```

#### Parallel Agent Execution
```javascript
// Launch multiple agents simultaneously
Task tool: subagent_type="Explore" - "Map database schema"
Task tool: subagent_type="Explore" - "Find API routes"
Task tool: subagent_type="Explore" - "Analyze auth flow"
```

---

## Implementation Priority

### Phase 1: Critical (Do Now)
- [ ] Install `typescript-lsp` plugin
- [ ] Install `security-guidance` plugin
- [ ] Create `.env.example`
- [ ] Add `.claude/hooks.json` with pre-commit checks
- [ ] Update `docker-compose.yml` with env vars and health checks

### Phase 2: High Value (This Week)
- [ ] Install `pr-review-toolkit` and `commit-commands` plugins
- [ ] Create `setup.sh` automation script
- [ ] Add Vercel and GitHub MCP servers
- [ ] Create `.claude/settings.json`
- [ ] Write `WORKFLOWS.md` documentation

### Phase 3: Enhancement (Nice to Have)
- [ ] Add custom skills (`/test-api`, `/db-migrate`, `/deploy-preview`)
- [ ] Create minimal Next.js example app
- [ ] Add GitHub Actions CI/CD
- [ ] Setup Husky + lint-staged
- [ ] Add Sentry MCP integration
- [ ] Document agent coordination patterns

---

## Cost/Benefit Analysis

| Addition | Setup Time | Value Impact | Context Cost |
|----------|-----------|--------------|--------------|
| TypeScript LSP | 1 min | ⭐⭐⭐⭐⭐ Critical | Low |
| Security Plugin | 1 min | ⭐⭐⭐⭐⭐ Critical | Low |
| Hooks | 5 min | ⭐⭐⭐⭐⭐ High | None |
| .env.example | 2 min | ⭐⭐⭐⭐⭐ High | None |
| setup.sh | 10 min | ⭐⭐⭐⭐ High | None |
| Vercel MCP | 5 min | ⭐⭐⭐⭐ High | Low |
| PR Review Plugin | 1 min | ⭐⭐⭐⭐ High | Medium |
| Settings.json | 5 min | ⭐⭐⭐ Medium | None |
| Custom Skills | 30 min | ⭐⭐⭐ Medium | Low |
| CI/CD | 20 min | ⭐⭐⭐ Medium | None |

---

## Why These Additions Matter

### Without TypeScript LSP
- Claude can't see type errors until runtime
- No autocomplete across 50+ files
- Refactoring becomes manual and error-prone

### Without Hooks
- Manual lint checks before every commit
- No automated quality gates
- Security issues caught in PR review (too late)

### Without MCP Servers
- Can't deploy from Claude
- Manual GitHub PR management
- No error tracking integration

### Without Setup Automation
- Every developer repeats 20 manual steps
- Inconsistent configurations
- Onboarding takes hours instead of minutes

---

## Next Steps

1. Review this document
2. Choose implementation phase (1, 2, or 3)
3. Run the setup commands
4. Update project README with new capabilities
5. Test workflows with Claude Code

Want me to implement any of these? Start with Phase 1 for immediate impact.
