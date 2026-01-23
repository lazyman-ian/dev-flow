---
name: meta-iterate
description: Analyzes session performance and iterates agent/skill/rule prompts for self-improvement. This skill should be used when user says "improve prompts", "analyze sessions", "self-improve", "discover skills", "compound learnings", "learn from sessions", "优化工作流", "迭代agent", "分析session", "发现新skill", "积累经验", "学习总结", "提取规则". Triggers on /meta-iterate, 自我改进, 会话分析, prompt优化.
model: opus
context: fork
allowed-tools: [Read, Glob, Grep, Write, Edit, Bash, WebSearch, Task, TaskCreate, TaskUpdate, TaskList, AskUserQuestion]
---

# meta-iterate - Self-Improvement Workflow

Analyze Claude Code session performance and iterate on agent/skill/rule prompts for continuous improvement.

## When to Use

- Periodically evaluate workflow effectiveness
- After noticing repeated issues
- When prompted by session-end reminder (every 10 sessions)
- To proactively improve Claude capabilities
- To compound learnings into permanent artifacts

## Commands

| Command | Purpose |
|---------|---------|
| `/meta-iterate` | Run full 5-phase workflow |
| `/meta-iterate evaluate` | Only evaluate sessions |
| `/meta-iterate discover` | Discover new skill opportunities |
| `/meta-iterate compound` | Transform learnings into skills/rules |
| `/meta-iterate diagnose` | Only diagnose issues |
| `/meta-iterate propose` | Only generate proposals |
| `/meta-iterate apply` | Apply approved changes |
| `/meta-iterate verify` | Verify improvement effects |

## Workflow

```
evaluate → discover/compound (optional) → diagnose → propose → [approve] → apply → verify
```

| Phase | Input | Output | Agent |
|-------|-------|--------|-------|
| **evaluate** | Braintrust logs | `EVAL-<date>.json` | evaluate-agent |
| **discover** | Evaluation data | `DISCOVER-<date>.md` | (built-in) |
| **compound** | Learnings files | Artifacts proposal | (built-in) |
| **diagnose** | Evaluation | `DIAG-<date>.md` | diagnose-agent |
| **propose** | Diagnosis | `PROP-<date>.md` | propose-agent |
| **apply** | Proposals + approval | Component files | apply-agent |
| **verify** | Post-change sessions | `ITER-NNN.md` | verify-agent |

## Options

| Option | Default | Description |
|--------|---------|-------------|
| `--recent N` | 10 | Number of sessions to analyze |
| `--target PATH` | all | Specific component to focus on |
| `--type TYPE` | all | agent, skill, rule, or all |
| `--threshold N` | 70 | Score threshold for recommendations |

## Local Mode (No External Dependencies)

When Braintrust is unavailable (missing Python modules, API issues, network), use local data sources.

### Quick Check

```bash
# Test Braintrust availability
uv run python -c "import requests" 2>/dev/null && echo "BRAINTRUST_OK" || echo "LOCAL_MODE"
```

### Local Data Sources

| Source | Location | Contains | Extraction |
|--------|----------|----------|------------|
| Session JSONL | `~/.claude/projects/<proj>/*.jsonl` | Full transcripts | `jq` parsing |
| Session index | `~/.claude/projects/<proj>/session-index` | Metadata | `jq .sessions` |
| Stats cache | `~/.claude/projects/<proj>/stats-cache` | Token usage | `jq .sessions` |
| Commit reasoning | `.git/claude/commits/*/reasoning.md` | Decisions | Read directly |
| Ledgers | `thoughts/ledgers/` | Task completion | Read directly |

### Local Workflow

```
[local evaluate] -> diagnose -> propose -> [approve] -> apply -> verify
```

The evaluate phase uses local session JSONL files instead of Braintrust API.
Mark evaluation with `data_quality: "local"` to indicate limited data fidelity.

## Examples

```bash
# Full workflow
/meta-iterate

# Evaluate recent 20 sessions
/meta-iterate evaluate --recent 20

# Discover new skill opportunities
/meta-iterate discover

# Compound learnings into permanent artifacts
/meta-iterate compound

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
| Compound | `thoughts/proposals/COMPOUND-YYYY-MM-DD.md` | Markdown |
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
| `braintrust_analyze.py` | Session data source (primary) |
| Local JSONL parsing | Session data source (fallback) |

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `requests module not installed` | Use Local Mode OR run `uv pip install requests` |
| `aiohttp not found` | Use Local Mode OR run `uv pip install aiohttp` |
| Braintrust API timeout | Use Local Mode (local files don't require network) |
| Empty evaluation results | Check `~/.claude/projects/` for session files |

**Tip**: Local Mode provides ~80% of evaluation quality for most use cases.

## References

- `references/compound-learnings.md` - Detailed process for transforming learnings into artifacts

## Quick Reference

```bash
# Weekly check (full workflow)
/meta-iterate

# Find new skill opportunities
/meta-iterate discover

# Transform learnings into permanent artifacts
/meta-iterate compound

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
