# Validation Template

Complete template and guidelines for plan validation.

## Handoff Filename

`validation-<plan-name>.md`

---

## Template

```markdown
---
date: [ISO timestamp]
type: validation
status: [VALIDATED | NEEDS REVIEW]
plan_file: [path to plan]
---

# Plan Validation: [Plan Name]

## Overall Status: [VALIDATED | NEEDS REVIEW]

## Precedent Check (RAG-Judge)

**Verdict:** [PASS | FAIL]

### Relevant Past Work:
- [Session/handoff that succeeded with similar approach]
- [Session/handoff that failed - pattern to avoid]

### Gaps Identified:
- [Gap 1 from RAG-judge, if any]
- [Gap 2 from RAG-judge, if any]

(If no relevant precedent: "No similar past work found in Artifact Index")

## Tech Choices Validated

### 1. [Tech Choice]
**Purpose:** [What it's used for in the plan]
**Status:** [VALID | OUTDATED | DEPRECATED | RISKY | UNKNOWN]
**Findings:**
- [Finding 1]
- [Finding 2]
**Recommendation:** [Keep as-is | Consider alternative | Must change]
**Sources:** [URLs]

### 2. [Tech Choice]
[Same structure...]

## Summary

### Validated (Safe to Proceed):
- [Choice 1] ✓
- [Choice 2] ✓

### Needs Review:
- [Choice 3] - [Brief reason]
- [Choice 4] - [Brief reason]

### Must Change:
- [Choice 5] - [Brief reason and suggested alternative]

## Recommendations

[If NEEDS REVIEW:]
1. [Specific recommendation]
2. [Specific recommendation]

[If VALIDATED:]
All tech choices are current best practices. Plan is ready for implementation.

## For Implementation

[Notes about patterns or approaches to follow]
```

---

## Return Message Format

```
Validation Complete

Status: [VALIDATED | NEEDS REVIEW]
Handoff: [path to validation handoff]

Validated: [N] tech choices checked
Issues: [N] issues found (or "None")

[If VALIDATED:]
Plan is ready for implementation.

[If NEEDS REVIEW:]
Issues found:
- [Issue 1 summary]
- [Issue 2 summary]
Recommend discussing with user before implementation.
```

---

## Status Definitions

| Status | When to Return |
|--------|----------------|
| **VALID** | Current best practice, no issues |
| **OUTDATED** | Better alternatives exist |
| **DEPRECATED** | Should not use |
| **RISKY** | Security or stability concerns |
| **UNKNOWN** | Couldn't find enough info |

---

## Validation Thresholds

### VALIDATED
- All choices are valid, OR
- Only minor suggestions (not blockers)

### NEEDS REVIEW
- Any choice is DEPRECATED
- Any choice is RISKY (security)
- Any choice significantly OUTDATED with better alternatives
- Critical architectural concerns

---

## Standard Library Note

These don't need external validation (always valid):

**Python:**
- argparse, asyncio, json, os, pathlib, etc.

**Standard patterns:**
- REST APIs, JSON config, environment variables

**Established tools:**
- pytest, git, make

**Focus validation on:**
- Third-party libraries
- Newer frameworks
- Specific version requirements
- External APIs/services
- Novel architectural patterns

---

## Research Queries

For each tech choice, use WebSearch:

```
WebSearch("[library/pattern] best practices 2024 2025")
WebSearch("[library] vs alternatives [year]")
WebSearch("[pattern] deprecated OR recommended [year]")
```

Check for:
- Is this still recommended?
- Are there better alternatives?
- Any known deprecations?
- Security concerns?
