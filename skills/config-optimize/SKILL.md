---
name: config-optimize
description: Check Claude Code releases and optimize configuration for new features. Use when user says "optimize config", "check updates", "new features", "what's new", "配置优化", "检查更新", "新功能". Runs periodically or after Claude Code updates.
model: haiku
allowed-tools: [Read, Glob, Grep, Write, Edit, Bash, WebFetch, TodoWrite]
---

# config-optimize

Automatically check Claude Code releases and optimize configuration.

## When to Use

- After Claude Code updates
- Periodically (weekly recommended)
- When prompted by session reminder
- Manual: `/config-optimize`

## Commands

| Command | Purpose |
|---------|---------|
| `/config-optimize` | Full optimization workflow |
| `/config-optimize check` | Check only (no changes) |
| `/config-optimize apply` | Apply pending proposals |

## Workflow

```
VERSION CHECK → CONFIG ANALYSIS → GAP ANALYSIS → PROPOSALS → APPLY
```

1. **Version Check**: Compare current vs last checked version
2. **Config Analysis**: Scan settings, hooks, rules, skills
3. **Gap Analysis**: Identify unused features, deprecated patterns
4. **Proposals**: Generate optimization recommendations
5. **Apply**: Apply selected changes (requires approval)

## Reference Menu

| Reference | Load When |
|-----------|-----------|
| `references/version-history.md` | Generating proposals for specific versions |
| `references/config-areas.md` | Analyzing hooks, skills, rules, env |
| `references/workflow-details.md` | Understanding full workflow details |
| `references/sources.md` | Finding official documentation sources |

## Documentation Sources

| Source | URL | Use For |
|--------|-----|---------|
| GitHub Releases | github.com/anthropics/claude-code/releases | New features |
| Official Blog | claude.ai/blog | Best practices |
| Documentation | docs.anthropic.com/en/docs/claude-code | Reference |

## Quick Check

```bash
# 1. Current version
claude --version

# 2. Last checked
cat ~/.claude/config-optimize-state.json

# 3. Fetch release notes
WebFetch("https://github.com/anthropics/claude-code/releases")
```

## State File

`~/.claude/config-optimize-state.json`:
```json
{
  "last_checked_version": "2.1.3",
  "last_check_date": "2026-01-11",
  "applied_optimizations": ["agent_type_check", "force_autoupdate"]
}
```

## Output

| File | Purpose |
|------|---------|
| `thoughts/config-optimizations/CHECK-*.md` | Gap analysis |
| `thoughts/config-optimizations/APPLY-*.md` | Applied changes |

## Integration

| Skill | Focus |
|-------|-------|
| `/config-optimize` | Config based on releases |
| `/meta-iterate` | Prompts based on sessions |

Weekly routine: `/config-optimize` then `/meta-iterate`
