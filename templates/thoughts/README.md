# Thoughts Directory

This directory contains continuity ledgers and development notes.

## Structure

```
thoughts/
├── README.md
├── ledgers/           # Task tracking ledgers
│   ├── TASK-XXX-description.md
│   └── archive/       # Completed tasks
└── shared/            # Shared documentation
    └── plans/         # Implementation plans
```

## Ledger Format

Each ledger tracks a single task:

```markdown
# Session: TASK-XXX-description
Updated: <ISO timestamp>

## Goal
What you're trying to achieve

## Constraints
- Technical limitations
- Requirements

## Key Decisions
- **Decision**: Rationale

## State
- Done:
  - [x] Completed item
- Now:
  - [→] Current work
- Next:
  - [ ] Upcoming

## Open Questions
- Unresolved issues

## Working Set
- Branch: `feature/TASK-XXX-desc`
- PR: https://github.com/...
```

## Commands

```bash
/dev ledger          # View current ledger
/dev ledger list     # List all ledgers
/dev ledger archive  # Archive completed
/dev recall "keyword" # Search history
```

## Best Practices

1. **Update regularly**: Mark checkboxes as you complete items
2. **Document decisions**: Record why, not just what
3. **Archive promptly**: Clean up after PR merge
4. **Search before starting**: Check if similar work was done before
