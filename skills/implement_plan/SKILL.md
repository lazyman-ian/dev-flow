---
name: implement-plan
description: Execute implementation plans with TDD and agent orchestration. Use when user says "implement plan", "execute plan", "follow the plan", "按计划实现", "执行方案". Supports direct implementation and compaction-resistant agent mode.
model: opus
context: fork
allowed-tools: [Read, Glob, Grep, Edit, Write, Bash, Task, TodoWrite]
---

# Implement Plan

Execute approved technical plans from `thoughts/shared/plans/`.

## When to Use

- `/implement_plan [plan-path]`
- "implement plan", "execute plan", "follow the plan"
- 按计划实现, 执行方案

## Execution Modes

| Mode | When to Use |
|------|-------------|
| **Direct** (default) | 1-3 tasks, quick focused work |
| **Agent Orchestration** | 4+ tasks, context preservation critical |

## Getting Started

1. Read plan completely (check existing `[x]` marks)
2. Read original ticket + all mentioned files (FULLY)
3. Create todo list for progress tracking
4. Start implementing

## Reference Menu

| Reference | Load When |
|-----------|-----------|
| `references/agent-orchestration.md` | Using agent mode (4+ tasks) |

## Direct Implementation

For small plans (≤3 tasks):

1. Implement each phase yourself
2. Run success criteria checks
3. Update checkboxes in plan
4. Pause for manual verification per phase

### Verification Flow

```
Phase N Complete - Ready for Manual Verification

Automated verification passed:
- [List automated checks]

Please perform manual verification:
- [List manual items from plan]

Let me know when ready for Phase N+1.
```

## Agent Orchestration

For larger plans (4+ tasks), use agent orchestration:

```
"I'll use agent orchestration for this plan"
```

Then follow `references/agent-orchestration.md`.

### Quick Setup

```bash
mkdir -p thoughts/handoffs/<session-name>
cat .claude/skills/implement_task/SKILL.md
```

## Resuming Work

If plan has existing `[x]` marks:
- Trust completed work is done
- Pick up from first unchecked item
- Verify previous work only if something seems off

## If Things Don't Match

```
Issue in Phase [N]:
Expected: [what the plan says]
Found: [actual situation]
Why this matters: [explanation]

How should I proceed?
```

## Resumable Agents

Check `.claude/cache/agents/agent-log.jsonl` for agent IDs.

```
Task(
  resume="<agentId>",
  prompt="Phase 2 isn't matching. Can you clarify..."
)
```
