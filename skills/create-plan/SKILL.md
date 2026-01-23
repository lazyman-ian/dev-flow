---
name: create-plan
description: Creates detailed implementation plans through interactive research. This skill should be used when user says "create plan", "make a plan", "design architecture", "plan feature", "制定计划", "设计方案", "规划功能", "架构设计". Triggers on /create_plan, 实现计划, 功能规划, 技术方案.
model: opus
allowed-tools: [Read, Glob, Grep, WebSearch, Task, TaskCreate, TaskUpdate]
---

# Implementation Plan

Create detailed implementation plans through interactive, iterative research.

## When to Use

- Planning new features
- Designing architecture
- Preparing implementation strategy
- `/create_plan [ticket-file]`

## Initial Response

**If parameters provided**: Skip greeting, read files, begin research.

**If no parameters**:
```
I'll help create a detailed implementation plan.

Please provide:
1. Task/ticket description (or ticket file reference)
2. Relevant context, constraints, requirements
3. Links to related research or implementations

Tip: /create_plan thoughts/allison/tickets/eng_1234.md
```

## Process Overview

```
CONTEXT GATHER → RESEARCH → STRUCTURE → WRITE PLAN → REVIEW
```

1. **Context Gathering**: Read all files FULLY, spawn research agents
2. **Research**: Parallel sub-tasks, verify findings
3. **Structure**: Present outline, get buy-in
4. **Write**: Create plan in `thoughts/shared/plans/`
5. **Review**: Iterate until approved

## Reference Menu

| Reference | Load When |
|-----------|-----------|
| `references/process-steps.md` | Detailed step-by-step process |
| `references/plan-template.md` | Full plan template + success criteria |
| `references/guidelines.md` | Planning principles + sub-task patterns |

## Quick Reference

### Plan File Location
`thoughts/shared/plans/YYYY-MM-DD-ENG-XXXX-description.md`

### Success Criteria Format
```markdown
#### Automated Verification:
- [ ] Tests pass: `make test`
- [ ] Linting passes: `make lint`

#### Manual Verification:
- [ ] Feature works in UI
- [ ] Performance acceptable
```

### Key Agents

| Agent | Purpose |
|-------|---------|
| codebase-locator | Find related files |
| codebase-analyzer | Understand implementation |
| thoughts-locator | Find existing research |
| research-agent | External documentation |

## Core Principles

1. **Be Skeptical**: Question vague requirements, verify with code
2. **Be Interactive**: Get buy-in at each step
3. **Be Thorough**: Read files completely, include file:line refs
4. **No Open Questions**: Resolve all questions before finalizing

## Example

```
User: /create_plan thoughts/allison/tickets/eng_1478.md

[Reads ticket fully]
[Spawns parallel research tasks]
[Presents informed understanding + questions]
[Iterates with user]
[Writes plan to thoughts/shared/plans/]
```
