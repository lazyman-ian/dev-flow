---
description: Implement plan with TDD and agent orchestration
---

Implement an approved plan using Test-Driven Development and optional agent orchestration.

## Instructions

### 1. Read Plan

```bash
cat thoughts/shared/plans/<plan-file>.md
```

Check for existing progress (checkmarks `[x]`).

### 2. Choose Mode

| Tasks | Mode | Description |
|-------|------|-------------|
| 1-3 | Direct | Implement yourself |
| 4+ | Orchestration | Delegate to agents |

### 3. Direct Implementation

For each phase:

#### 3a. TDD Cycle (Red-Green-Refactor)

**RED** - Write failing test:
```swift
func testUserLogin() {
    // Test expected behavior
}
```
Run test → Verify it FAILS

**GREEN** - Minimal implementation:
```swift
func login() -> Bool {
    // Simplest code to pass
}
```
Run test → Verify it PASSES

**REFACTOR** - Clean up:
- Remove duplication
- Improve names
- Keep tests green

#### 3b. Verify Phase

```bash
make fix && make check
```

Update plan checkboxes:
```
- [x] Phase 1 complete
```

#### 3c. Pause for Manual Verification

```
Phase 1 Complete - Ready for Manual Verification

Automated passed:
- [x] make check
- [x] make test

Please verify manually:
- [ ] Feature works in UI
- [ ] No regressions

Let me know when ready for Phase 2.
```

### 4. Agent Orchestration

For larger plans (4+ tasks):

#### 4a. Setup

```bash
mkdir -p thoughts/handoffs/<session-name>
```

#### 4b. Spawn Agents

For each task:
```
Task(
  subagent_type="general-purpose",
  model="opus",
  prompt="""
  # Implementation Task Agent

  [implement_task SKILL.md content]

  ## Your Task
  Task [N] of [Total]: [Description]

  ## Previous Handoff
  [Content or "First task"]

  ## Handoff Directory
  thoughts/handoffs/<session>/
  """
)
```

#### 4c. Process Results

Read handoff → Update ledger → Continue or handle blocker

### 5. Update Ledger

```
dev_ledger(action="update", content="Phase N complete")
```

### 6. Commit When Ready

```
/dev commit
```

## TDD Guidelines

- Write test BEFORE implementation - no exceptions
- One test per behavior
- If code written first → DELETE and start with test
- Hard to test = design problem

## Recovery After Compaction

1. Read ledger (auto-loaded)
2. List handoffs: `ls thoughts/handoffs/<session>/`
3. Read last handoff
4. Continue from next uncompleted task

## Examples

```bash
/dev implement thoughts/shared/plans/2025-01-10-auth.md
/dev implement phase 2        # Continue specific phase
/dev implement                 # Resume current plan
```

## Handoff Chain

```
task-01-setup.md
    ↓ (read by agent 2)
task-02-models.md
    ↓ (read by agent 3)
task-03-api.md
    ↓
...
```

Each handoff preserves context across compactions.
