# Kagent: Kubernetes-Native AI Agent Framework

Kagent is a Kubernetes-native platform for building, deploying, and managing AI agents at scale. It provides a unified framework for running agents built with various frameworks (Google ADK, LangGraph, CrewAI) in a production Kubernetes environment.

## Project Architecture

Kagent is a **multi-language monorepo** with three main components:

- **Go** (`go/`) - Kubernetes controller, HTTP API server, and CLI
- **Python** (`python/packages/`) - Agent runtime integrations for multiple frameworks
- **TypeScript/Next.js** (`ui/`) - Web-based management interface

## Where to Find Things

### Backend (Go)

```
go/
├── api/              # API definitions and schemas
├── cli/              # Command-line interface (kagent CLI)
├── cmd/              # Main applications (server, controller)
├── internal/
│   ├── controller/   # Kubernetes controller logic
│   ├── database/     # GORM database layer
│   ├── httpserver/   # HTTP API handlers
│   └── a2a/          # Agent-to-Agent protocol
└── pkg/              # Public packages
```

**Key Technologies:** Go 1.25.1, Gorilla Mux, GORM, Kubernetes client-go

### Python Agent Runtimes

```
python/packages/
├── kagent-adk/       # Google Agent Development Kit integration
├── kagent-langgraph/ # LangGraph integration
├── kagent-crewai/    # CrewAI integration
└── kagent-core/      # Shared utilities and A2A protocol
```

**Key Technologies:** Python 3.11+, httpx, pydantic, a2a-sdk

### Frontend (Next.js)

```
ui/
├── src/
│   ├── app/          # Next.js 15 App Router pages
│   ├── components/   # React components (organized by feature)
│   │   ├── ui/       # Radix UI wrapper components
│   │   ├── chat/     # Chat interface components
│   │   ├── sidebars/ # Navigation components
│   │   └── ...       # Other feature components
│   └── lib/          # Utilities and helpers
└── cypress/          # E2E tests
```

**Key Technologies:** Next.js 15.4.7, React 18, TypeScript 5.8.3, Tailwind CSS, Radix UI

### Standards & Skills

```
agent-os/
└── standards/        # Coding standards documentation
    ├── backend/      # Go backend patterns
    ├── frontend/     # React/Next.js patterns
    ├── global/       # Cross-language conventions
    └── testing/      # Testing best practices

.claude/
├── skills/           # Claude Code skills for domain-specific guidance
│   ├── backend-*/    # Backend development skills
│   ├── frontend-*/   # Frontend development skills
│   ├── global-*/     # Cross-cutting concerns
│   └── testing-*/    # Testing skills
├── agents/           # Specialized Claude agents
│   └── agent-os/     # Agent-OS workflow agents
└── commands/         # Custom slash commands
    └── agent-os/     # Agent-OS workflow commands
```

## Key Documentation

- **Standards**: `agent-os/standards/` - Comprehensive coding standards with examples
- **Tech Stack**: `agent-os/standards/global/tech-stack.md` - Complete technology inventory
- **Component Docs**: Look for `claude.md` files in each major directory
- **Codebase Analysis**: `go/CODEBASE_ANALYSIS.md` - Go backend patterns and architecture

## Development Workflow

### Local Development

```bash
# Backend API server
cd go && make run-server

# Frontend dev server
cd ui && npm run dev

# Python package development
cd python/packages/kagent-adk && uv pip install -e .
```

### Testing

```bash
# Go tests
cd go && go test ./...

# Python tests
cd python/packages/kagent-adk && pytest

# Frontend tests
cd ui && npm test
```

### Database

- **Development**: SQLite (zero config, file-based)
- **Production**: PostgreSQL
- **Migrations**: GORM AutoMigrate on startup

## Using the Task Finalizer Agent

When you've completed a task and need to finalize it with documentation, commits, and a pull request, use the **task-finalizer agent**:

### When to Use

The task-finalizer agent should be invoked when:
- You've finished implementing a feature or bug fix
- You want to commit changes and create a PR
- The user says phrases like "wrap this up", "finish up", or "create a PR"
- You've completed all requested work and it's ready for review

### How to Invoke

Simply tell Claude Code:
```
"Please use the task-finalizer agent to finalize this work"
```

Or trigger it naturally:
```
"I'm done with the implementation. Let's commit and create a PR."
"This looks good. Wrap it up and create a pull request."
```

### What It Does

The task-finalizer agent will:
1. **Analyze changes** - Review uncommitted and unpushed changes
2. **Update documentation** - Update CLAUDE.md files and other relevant docs
3. **Create commits** - Stage changes and create well-formatted commits
4. **Manage branches** - Create feature branches if needed
5. **Push changes** - Push commits to remote repository
6. **Create draft PR** - Generate a comprehensive pull request description

See `.claude/agents/task-finalizer.md` for detailed workflow documentation.

## Kubernetes Integration

Kagent defines custom Kubernetes resources:

- **Agent** - Defines an AI agent with its configuration
- **ModelConfig** - Configures LLM model settings
- **ToolServer** - Integrates MCP tool servers

The controller in `go/internal/controller/` watches these resources and manages their lifecycle.

## Agent-to-Agent (A2A) Protocol

The A2A protocol enables agents to communicate with each other. Implementation:

- **Go**: `go/internal/a2a/` - Server-side A2A handling
- **Python**: `python/packages/kagent-core/src/kagent/core/a2a/` - Agent runtime integration
- **Frontend**: `ui/` uses `@a2a-js/sdk` for browser-based A2A communication

## Environment Configuration

Each component uses environment variables for configuration:

**Go (`go/`):**
- `DATABASE_URL` - PostgreSQL connection string
- `SQLITE_PATH` - SQLite database file (default: `./kagent.db`)
- `PORT` - HTTP server port (default: `8080`)

**Python (`python/packages/`):**
- `KAGENT_URL` - Kagent API endpoint
- `KAGENT_NAME` - Agent name
- `KAGENT_NAMESPACE` - Agent namespace

**Frontend (`ui/`):**
- `NEXT_PUBLIC_API_URL` - Kagent API endpoint
- `NEXT_PUBLIC_USER_ID` - User identifier for development

## Contributing

1. Follow the coding standards in `agent-os/standards/`
2. Use Claude Code skills (`.claude/skills/`) for domain-specific guidance
3. Write tests for new features (see `agent-os/standards/testing/test-writing.md`)
4. Update documentation when making significant changes
5. Use the task-finalizer agent to create well-documented PRs

## Getting Help

- **Standards**: Check `agent-os/standards/` for language-specific patterns
- **Skills**: Claude Code will automatically invoke relevant skills from `.claude/skills/`
- **Commands**: Use `/help` in Claude Code to see available commands
- **Agent-OS Workflows**: Use commands in `.claude/commands/agent-os/` for structured development
