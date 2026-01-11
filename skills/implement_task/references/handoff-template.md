# Handoff Template

Template and guidelines for creating implementation handoffs.

## Filename Format

`task-NN-<short-description>.md`

- NN = zero-padded task number (01, 02, etc.)
- short-description = kebab-case summary

---

## Template

```markdown
---
date: [ISO timestamp with timezone]
task_number: [N]
task_total: [Total tasks in plan]
status: [success | partial | blocked]
---

# Task Handoff: [Task Description]

## Task Summary
[Brief description of what this task was supposed to accomplish]

## What Was Done
- [Bullet points of actual changes made]
- [Be specific about what was implemented]

## Files Modified
- `path/to/file.ts:45-67` - [What was changed]
- `path/to/other.ts:123` - [What was changed]

## Decisions Made
- [Decision 1]: [Rationale]
- [Decision 2]: [Rationale]

## Patterns/Learnings for Next Tasks
- [Patterns discovered that future tasks should follow]
- [Gotchas or important context]

## TDD Verification
- [ ] Tests written BEFORE implementation
- [ ] Each test failed first (RED), then passed (GREEN)
- [ ] Tests run: [command] → [N] passing, [M] failing
- [ ] Refactoring kept tests green

## Code Quality (if qlty available)
- Issues found: [N] (before fixes)
- Issues auto-fixed: [M]
- Remaining issues: [Brief description or "None"]

## Issues Encountered
[Problems hit and how resolved, or blockers if status is blocked]

## Next Task Context
[Brief note about what the next task should know]
```

---

## Return Message Format

After creating handoff, return:

```
Task [N] Complete

Status: [success/partial/blocked]
Handoff: [path to handoff file]

Summary: [1-2 sentence description]

[If blocked: Blocker description and what's needed to unblock]
```

---

## Status Definitions

| Status | When to Use |
|--------|-------------|
| **success** | Task fully completed, all tests pass |
| **partial** | Some work done but not complete |
| **blocked** | Cannot proceed without external input |

---

## Reading Previous Handoffs

When reading a previous task's handoff:

1. Read the handoff document completely
2. Extract key sections:
   - Files Modified (what was changed)
   - Patterns/Learnings (what to follow)
   - Next Task Context (dependencies)
3. Verify mentioned files still exist
4. Apply learnings to your implementation

### What to Look For

- **Files Modified**: May need to read for context
- **Decisions Made**: Follow consistent approaches
- **Patterns/Learnings**: Apply to your work
- **Issues Encountered**: Avoid repeating mistakes

### If Handoff Seems Stale

- Check if files mentioned still exist
- Verify patterns are still valid
- Note discrepancies in your own handoff

---

## Directory Structure

```
thoughts/handoffs/<session>/
├── task-01-setup-schema.md
├── task-02-create-endpoints.md
├── task-03-add-validation.md      ← You create this
├── task-04-write-tests.md         ← Next agent creates
└── ...
```

Each agent reads previous → does work → creates next. The chain continues.
