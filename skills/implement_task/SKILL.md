---
name: implement-task
description: Implementation agent for single task execution with TDD. Internal skill used by implement-plan orchestrator. Creates handoff documents for compaction-resistant workflows.
user-invocable: false
context: fork
allowed-tools: [Read, Glob, Grep, Edit, Write, Bash, TodoWrite]
---

# Implementation Task Agent

Execute a single task from a larger plan, create handoff document.

## What You Receive

1. **Continuity ledger** - Current session state
2. **The plan** - Overall implementation plan
3. **Your specific task** - What to implement
4. **Previous task handoff** (if any) - Context from last task
5. **Handoff directory** - Where to save your handoff

## Reference Menu

| Reference | Load When |
|-----------|-----------|
| `references/tdd-workflow.md` | TDD Red-Green-Refactor details |
| `references/handoff-template.md` | Full handoff template |

## Process Overview

```
UNDERSTAND → IMPLEMENT (TDD) → CREATE HANDOFF → RETURN
```

### Step 1: Understand Context

- Read previous handoff (if provided)
- Note learnings and patterns to follow
- Understand where task fits in overall plan

### Step 2: Implement with TDD

**Iron Law: No production code without a failing test first.**

1. **RED**: Write failing test
2. **GREEN**: Minimal implementation to pass
3. **REFACTOR**: Clean up, keep tests green
4. **REPEAT**: For each behavior

See `references/tdd-workflow.md` for details.

### Step 3: Create Handoff

Use `references/handoff-template.md` format.

**Filename**: `task-NN-<short-description>.md`

### Step 4: Return

```
Task [N] Complete

Status: [success/partial/blocked]
Handoff: [path to handoff file]

Summary: [1-2 sentences]
```

## DO / DON'T

| DO | DON'T |
|----|-------|
| Write tests FIRST | Write code before tests |
| Watch tests fail before implementing | Skip the failing test step |
| Read files completely | Use limit/offset |
| Follow existing patterns | Over-engineer |
| Create handoff (even if blocked) | Skip handoff |
| Keep changes focused | Expand scope |

## If Blocked

1. Document blocker in handoff
2. Set status to "blocked"
3. Describe what's needed to unblock
4. Return to orchestrator
