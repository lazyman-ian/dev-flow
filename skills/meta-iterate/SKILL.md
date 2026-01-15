---
name: meta-iterate
description: Self-improvement workflow analyzing session performance to iterate agent/skill/rule prompts. Use when user says "improve prompts", "analyze sessions", "self-improve", "优化工作流", "迭代 agent", "分析 session 质量".
model: opus
context: fork
allowed-tools: [Read, Glob, Grep, Write, Edit, Bash, WebSearch, Task, TodoWrite]
---

# meta-iterate - Self-Improvement Workflow

Analyze Claude Code session performance and iterate on agent/skill/rule prompts for continuous improvement.

## When to Use

- Periodically evaluate workflow effectiveness
- After noticing repeated issues
- When prompted by session-end reminder (every 10 sessions)
- To proactively improve Claude capabilities

## Commands

| Command | Purpose |
|---------|---------|
| `/meta-iterate` | Run full 5-phase workflow |
| `/meta-iterate evaluate` | Only evaluate sessions |
| `/meta-iterate discover` | Discover new skill opportunities |
| `/meta-iterate diagnose` | Only diagnose issues |
| `/meta-iterate propose` | Only generate proposals |
| `/meta-iterate apply` | Apply approved changes |
| `/meta-iterate verify` | Verify improvement effects |

## Workflow

```
evaluate → discover (optional) → diagnose → propose → [approve] → apply → verify
```

| Phase | Input | Output | Agent |
|-------|-------|--------|-------|
| **evaluate** | Braintrust logs | `EVAL-<date>.json` | evaluate-agent |
| **discover** | Evaluation data | `DISCOVER-<date>.md` | (built-in) |
| **diagnose** | Evaluation | `DIAG-<date>.md` | diagnose-agent |
| **propose** | Diagnosis | `PROP-<date>.md` | propose-agent |
| **apply** | Proposals + approval | Component files | apply-agent |
| **verify** | Post-change sessions | `ITER-NNN.md` | verify-agent |

**discover** analyzes tool usage patterns to suggest new skills (merged from `/discover-skills`).

## Options

| Option | Default | Description |
|--------|---------|-------------|
| `--recent N` | 10 | Number of sessions to analyze |
| `--target PATH` | all | Specific component to focus on |
| `--type TYPE` | all | agent, skill, rule, or all |
| `--threshold N` | 70 | Score threshold for recommendations |

## Examples

```bash
# Full workflow
/meta-iterate

# Evaluate recent 20 sessions
/meta-iterate evaluate --recent 20

# Discover new skill opportunities (merged from /discover-skills)
/meta-iterate discover

# Focus on specific agent
/meta-iterate --target agents/plan-agent.md

# Apply specific proposals
/meta-iterate apply --proposals PROP-2026-01-10.md

# Verify after improvements
/meta-iterate verify
```

## Output Files

| Phase | Location | Format |
|-------|----------|--------|
| Evaluate | `thoughts/evaluations/EVAL-YYYY-MM-DD.json` | JSON |
| Discover | `thoughts/discoveries/DISCOVER-YYYY-MM-DD.md` | Markdown |
| Diagnose | `thoughts/diagnoses/DIAG-YYYY-MM-DD.md` | Markdown |
| Propose | `thoughts/proposals/PROP-YYYY-MM-DD.md` | Markdown |
| Apply | Component files + `thoughts/iterations/ITER-NNN.md` | Markdown |
| Verify | `thoughts/iterations/ITER-NNN.md` (updated) | Markdown |

## Human Review Gate

**Phase 4 (APPLY) requires explicit approval:**

1. Review proposals in `thoughts/proposals/`
2. Confirm which changes to apply
3. Backups saved to `thoughts/backups/`

This ensures human oversight on all prompt changes.

## Integration with dev-flow

| dev-flow Component | Usage |
|--------------------|-------|
| `dev_ledger` | Track iteration tasks |
| `dev_reasoning` | Record iteration decisions |
| `braintrust_analyze.py` | Session data source |

## Quick Reference

```bash
# Weekly check (full workflow)
/meta-iterate

# Find new skill opportunities
/meta-iterate discover

# After noticing issues
/meta-iterate evaluate --recent 5
/meta-iterate diagnose

# Apply improvements
/meta-iterate propose
# Review proposals...
/meta-iterate apply

# Verify after 5+ new sessions
/meta-iterate verify
```
