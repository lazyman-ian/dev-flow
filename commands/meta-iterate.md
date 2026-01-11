---
description: Analyze session performance and iterate agent/skill/rule prompts for continuous improvement
---

# /meta-iterate - Self-Improvement Workflow

Analyze Claude Code session performance and iterate on prompts for continuous improvement.

## Auto Execute

```
1. Parse subcommand (evaluate/diagnose/propose/apply/verify)
2. Check for existing evaluation/diagnosis files
3. Launch appropriate agent based on phase
```

## Subcommands

| Command | Purpose |
|---------|---------|
| `/meta-iterate` | Run full 5-phase workflow |
| `/meta-iterate evaluate` | Evaluate recent sessions |
| `/meta-iterate diagnose` | Diagnose identified issues |
| `/meta-iterate propose` | Generate improvement proposals |
| `/meta-iterate apply` | Apply approved changes |
| `/meta-iterate verify` | Verify improvement effects |

## Options

| Option | Default | Description |
|--------|---------|-------------|
| `--recent N` | 10 | Sessions to analyze |
| `--target PATH` | all | Specific component |
| `--type TYPE` | all | agent/skill/rule |
| `--threshold N` | 70 | Score threshold |

## Workflow

```
evaluate → diagnose → propose → [approve] → apply → verify
```

Phase 4 (apply) requires explicit user approval before modifying any files.

## Output Locations

| Phase | Output |
|-------|--------|
| Evaluate | `thoughts/evaluations/EVAL-YYYY-MM-DD.json` |
| Diagnose | `thoughts/diagnoses/DIAG-YYYY-MM-DD.md` |
| Propose | `thoughts/proposals/PROP-YYYY-MM-DD.md` |
| Apply | `thoughts/iterations/ITER-NNN.md` |

## Quick Start

```bash
# Weekly check
/meta-iterate

# Focus on specific component
/meta-iterate --target agents/plan-agent.md

# After applying changes
/meta-iterate verify
```
