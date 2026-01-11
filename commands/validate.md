---
description: Validate plan tech choices against current best practices
---

# /validate - Plan Validation

Validate technical plan choices against 2024-2025 best practices.

## Usage

```bash
/validate thoughts/shared/plans/YYYY-MM-DD-feature.md
```

## Process

1. **Extract Tech Choices** - Libraries, patterns, APIs from plan
2. **Check Precedent** - RAG-Judge for similar past work
3. **Research** - WebSearch for each choice
4. **Assess** - VALID | OUTDATED | DEPRECATED | RISKY | UNKNOWN
5. **Handoff** - Create validation report

## Output

| Status | Meaning |
|--------|---------|
| VALIDATED | Safe to implement |
| NEEDS REVIEW | Issues found, discuss first |

## Validation Handoff

Created at: `thoughts/handoffs/<session>/validation-<plan-name>.md`
