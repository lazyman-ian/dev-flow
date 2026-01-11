# TDD Workflow Reference

Test-Driven Development workflow for implementation agents.

## Iron Law

**No production code without a failing test first.**

---

## Red-Green-Refactor Cycle

### RED - Write Failing Test First

1. Read necessary files completely (no limit/offset)
2. Write a test that describes the desired behavior
3. Run the test and **verify it fails**
   - Confirm it fails for the RIGHT reason (missing functionality)
   - If it passes immediately, you're testing existing behavior - fix test

### GREEN - Minimal Implementation

4. Write the **simplest code** that makes the test pass
5. Run the test and **verify it passes**
   - Don't add features beyond what test requires
   - Don't refactor yet

### REFACTOR - Clean Up

6. Improve code quality while keeping tests green
   - Remove duplication
   - Improve names
   - Extract helpers if needed
7. Run tests again to confirm still passing

### REPEAT

8. Continue cycle for each behavior in your task

### QUALITY CHECK

9. Run code quality checks (if qlty configured):
   ```bash
   qlty check --fix
   ```

---

## TDD Guidelines

| DO | DON'T |
|----|-------|
| Write test BEFORE implementation | Write code first then test |
| One test per behavior | Multiple behaviors in one test |
| Clear test names | Vague test names |
| Use real code | Over-mock |
| Hard to test = design problem | Force tests onto bad design |

If you wrote code first, **DELETE IT** and start with test.

---

## Editing Tool Selection

| Tool | Best For | Speed |
|------|----------|-------|
| **morph-apply** | Large files (>500 lines), batch edits | 10,500 tokens/sec |
| **Claude Edit** | Small files already read, precise edits | Standard |

### Using morph-apply (recommended for large files)

```bash
uv run python -m runtime.harness scripts/morph_apply.py \
    --file "src/auth.ts" \
    --instruction "I will add null check for user" \
    --code_edit "// ... existing code ...
if (!user) throw new Error('User not found');
// ... existing code ..."
```

**Key pattern:** Use `// ... existing code ...` markers to show where changes go.

---

## Implementation Guidelines

- Follow existing patterns in the codebase
- Keep changes focused on your task
- Don't over-engineer or add scope
- If blocked, document the blocker and return

---

## TDD Verification Checklist

```markdown
## TDD Verification
- [ ] Tests written BEFORE implementation
- [ ] Each test failed first (RED), then passed (GREEN)
- [ ] Tests run: [command] → [N] passing, [M] failing
- [ ] Refactoring kept tests green
```
