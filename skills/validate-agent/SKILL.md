---
name: validate-agent
description: Validate technical plan choices against 2024-2025 best practices. Use when user says "validate plan", "check tech choices", "验证方案", "检查技术选型". Researches external sources before implementation.
model: haiku
context: fork
user-invocable: false
allowed-tools: [Read, WebSearch, WebFetch, Write]
---

> **Note:** Current year is 2025. Validate against 2024-2025 best practices.

# Validate Agent

Validate a technical plan's choices against current best practices.

## When to Use

- `/validate-agent [plan-path]`
- "validate plan", "check tech choices"
- 验证方案, 检查技术选型

## What You Receive

1. **Plan content** - The implementation plan
2. **Plan path** - Location of plan file
3. **Handoff directory** - Where to save validation

## Reference Menu

| Reference | Load When |
|-----------|-----------|
| `references/validation-template.md` | Full validation template |

## Process Overview

```
EXTRACT TECH CHOICES → CHECK PRECEDENT → RESEARCH → ASSESS → CREATE HANDOFF
```

### Step 1: Extract Tech Choices

Identify all technical decisions:
- Libraries/frameworks
- Patterns/architectures
- APIs or external services
- Implementation approaches

### Step 2: Check Past Precedent (RAG-Judge)

```bash
uv run python scripts/braintrust_analyze.py --rag-judge --plan-file <plan-path>
```

### Step 3: Research Each Choice (WebSearch)

```
WebSearch("[library] best practices 2024 2025")
WebSearch("[library] vs alternatives")
WebSearch("[pattern] deprecated OR recommended")
```

### Step 4: Assess Findings

| Status | Meaning |
|--------|---------|
| **VALID** | Current best practice |
| **OUTDATED** | Better alternatives exist |
| **DEPRECATED** | Should not use |
| **RISKY** | Security/stability concerns |
| **UNKNOWN** | Insufficient info (note as assumption) |

### Step 5: Create Handoff

See `references/validation-template.md`.

**Filename**: `validation-<plan-name>.md`

## Return Format

```
Validation Complete

Status: [VALIDATED | NEEDS REVIEW]
Handoff: [path]

Validated: [N] tech choices checked
Issues: [N] issues found (or "None")
```

## Skip Validation For

- Python stdlib (argparse, asyncio, json, etc.)
- Standard patterns (REST, JSON config)
- Established tools (pytest, git, make)

## Focus Validation On

- Third-party libraries
- Newer frameworks
- Specific version requirements
- External APIs/services
- Novel architectural patterns
