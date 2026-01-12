---
description: Create implementation plan through interactive research
---

Create a detailed implementation plan through research and iteration.

## Instructions

### 1. Check Input

If file path provided (e.g., `/dev-flow:plan thoughts/tickets/TASK-123.md`):
- Read the ticket/requirement file
- Begin research immediately

If no path:
```
I'll help create a detailed implementation plan.

Please provide:
1. Task/ticket description (or path to ticket file)
2. Relevant context or constraints
3. Links to related research
```

### 2. Context Gathering

Read all mentioned files **completely** (no limit/offset).

Spawn parallel research agents:
- **codebase-locator** - Find related files
- **codebase-analyzer** - Understand current implementation

```
dev_ledger(action="search", keyword="<related-term>")
```

### 3. Research Phase

After initial understanding, spawn more agents:
- Find similar implementations
- Check past decisions in thoughts/
- Research external best practices (if needed)

Present findings:
```
Based on my research:

**Current State:**
- [Discovery with file:line reference]
- [Pattern to follow]

**Design Options:**
1. [Option A] - [pros/cons]
2. [Option B] - [pros/cons]

**Open Questions:**
- [Technical uncertainty]
```

### 4. Plan Structure

Get alignment before details:
```
Proposed plan structure:

## Overview
[1-2 sentence summary]

## Phases:
1. [Phase name] - [what it accomplishes]
2. [Phase name] - [what it accomplishes]

Does this phasing make sense?
```

### 5. Write Plan

Save to `thoughts/shared/plans/YYYY-MM-DD-[ticket]-[description].md`:

```markdown
# [Feature] Implementation Plan

## Overview
[Brief description]

## Current State Analysis
[What exists, constraints discovered]

## Desired End State
[Specification + how to verify]

## What We're NOT Doing
[Out of scope items]

## Phase 1: [Name]

### Changes Required
**File**: `path/to/file.ext`
**Changes**: [Description]

### Success Criteria

#### Automated:
- [ ] Tests pass: `make test`
- [ ] Lint passes: `make check`

#### Manual:
- [ ] Feature works as expected

---

## Phase 2: [Name]
...
```

### 6. Update Ledger

```
dev_ledger(action="update", content="Plan created: [path]")
```

## Guidelines

- **Be Skeptical** - Question vague requirements
- **Be Interactive** - Get buy-in at each step
- **Be Thorough** - Include file:line references
- **No Open Questions** - Resolve all before finalizing

## Examples

```bash
/dev-flow:plan                                    # Interactive
/dev-flow:plan thoughts/tickets/TASK-123.md       # From ticket
/dev-flow:plan think deeply about auth system     # Deep analysis
```
