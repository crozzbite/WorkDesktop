---
name: strict-tdd
description: "Trigger: strict TDD, behavior-first apply. Micro-loop per task when strict_tdd is active and a test runner exists."
---

## When active

```
IF strict_tdd: true AND test runner detected
THEN apply this protocol per task
ELSE standard apply
```

Full canon: `docs/governance/refined-rules/09-strict-tdd-refined.md`

## Cycle (per task)

```
Safety net → Understand → RED → GREEN → Triangulate → Refactor
```

| Step | Gate |
|------|------|
| 0 Safety net | Stop on pre-existing failures in touched files |
| 1 Understand | Task, spec scenarios, design |
| 2 RED | Failing test before production code |
| 3 GREEN | Minimum code; run **only** relevant test file |
| 4 Triangulate | Happy path + edge cases; force real logic |
| 5 Refactor | Tests stay green after each step |

## Three laws

1. Do NOT write production code before a failing test
2. Do NOT write more test than needed to fail
3. Do NOT write more code than needed to pass

## Evidence table (required)

| Task | Test File | RED | GREEN | Triangulate | Refactor |
|------|-----------|-----|-------|-------------|----------|

## Banned assertions

- Tautologies (`expect(true).toBe(true)`)
- Type-only checks without behavior
- Empty collection asserts without setup context
- CSS class asserts (test behavior, not styling)

## STOP

- Never skip RED
- Never silently fall back to standard mode when Strict TDD is active
- Never declare done without executed test evidence

Reference: `.github/prompts/sdd-apply.prompt.md`
