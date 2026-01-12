# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

dev-flow-plugin is a Claude Code plugin providing unified development workflow automation: planning → coding → commit → PR → release. It supports iOS (Swift), Android (Kotlin), and Web (TypeScript) platforms.

## Build & Development

```bash
# MCP Server
cd mcp-server
npm install
npm run bundle    # Bundle to scripts/mcp-server.cjs (required for plugin)
npm run build     # TypeScript compile to dist/ (for development)
npm run dev       # Run with ts-node

# No tests currently configured
```

## Architecture

### Plugin Structure

```
.claude-plugin/plugin.json  # Plugin manifest (v3.6.2)
.mcp.json                   # MCP server config → scripts/mcp-server.cjs
skills/                     # 7 skills (SKILL.md + references/)
commands/                   # 20 command definitions
agents/                     # 12 agent prompts
hooks/hooks.json            # 3 hooks (SessionStart, PreCompact, PostToolUse)
templates/thoughts/schema/  # JSON schemas for meta-iterate outputs
```

### MCP Server (mcp-server/src/)

Single-file bundle architecture using `@modelcontextprotocol/sdk`:

| Module | Purpose |
|--------|---------|
| `index.ts` | Server entry, 14 MCP tools |
| `detector.ts` | Project type detection (ios/android/web) |
| `git/workflow.ts` | Git status, phase detection |
| `git/build-control.ts` | PR draft/ready, change analysis |
| `git/version.ts` | Version info, release notes |
| `platforms/ios.ts` | SwiftLint, SwiftFormat integration |
| `platforms/android.ts` | ktlint, ktfmt integration |
| `continuity/` | Ledgers, reasoning, branch management |

### Key MCP Tools

| Tool | Tokens | Purpose |
|------|--------|---------|
| `dev_status` | ~30 | Quick status: `PHASE\|✅0\|next` |
| `dev_flow` | ~100 | Full status table |
| `dev_config` | ~50 | Platform commands (lint/format/build) |
| `dev_ledger` | ~50 | Task continuity management |
| `dev_defaults` | ~20 | Auto-infer scope from changes |

### Workflow Phases

```
IDLE → DEVELOPING → READY_TO_PUSH → WAITING_QA → PR_OPEN → READY_TO_RELEASE
```

### Skill Architecture Pattern

Skills use Reference File Architecture for progressive loading:

```
skill/
├── SKILL.md           # < 150 lines, frontmatter + overview
└── references/        # Detailed docs loaded on demand
```

Required frontmatter:
```yaml
---
name: skill-name
description: What it does. Use when "[triggers]", "[中文触发词]".
allowed-tools: [specific, tools, only]
---
```

## Key Patterns

### Continuity System

- **Ledgers**: `thoughts/ledgers/CONTINUITY_CLAUDE-*.md` - Track task state across sessions
- **Reasoning**: `.git/claude/commits/<hash>/reasoning.md` - Document commit decisions
- Both stored in git for persistence

### Hook Integration

- `SessionStart`: Load active ledger context
- `PreCompact`: Backup transcript before context compaction
- `PostToolUse(Bash)`: Dev workflow reminders

### Agent Orchestration

Agents in `agents/` are spawned via Task tool for complex operations:
- `plan-agent.md` - Create implementation plans
- `implement-agent.md` - TDD execution
- `code-reviewer.md` - PR review
- `evaluate/diagnose/propose/apply/verify-agent.md` - Meta-iterate cycle

## Conventions

- Commit messages: `type(scope): subject` (Conventional Commits)
- Scope auto-inferred via `dev_defaults(action="scope")`
- All `/dev-flow:*` commands use MCP tools internally
- Platform detection is automatic based on project files
