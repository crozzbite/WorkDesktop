---
version: 1.0
current: true
description: Strict TDD apply protocol. Companion to Rule 09 (neutral).
---

# Strict TDD — Apply Protocol

**Version:** 1.0 ✓ (current)

> **Parent:** `docs/governance/refined-rules/09-agent-loops-refined.md`
> **Phase:** apply — each task in `tasks.md` when `strict_tdd: true` and a test runner exists

---

## Core idea

TDD is not "writing tests". It is **designing software from expected behavior**.

The test defines the **contract**; production code comes **after**. No blind implementation; no silent fallback to standard mode.

---

## When active

```
IF strict_tdd: true (project config or orchestrator)
   AND test runner detected
THEN → Strict TDD on every apply task
ELSE → Standard apply (this protocol does not apply)
```

---

## Mandatory cycle (per task)

| Step | Name | Action |
|------|------|--------|
| 0 | **Safety net** | Run existing tests on touched files; stop on pre-existing failures |
| 1 | **Understand** | Read task, spec scenarios, design, patterns |
| 2 | **RED** | Write failing test first; no production code before test |
| 3 | **GREEN** | Minimum code to pass; run **only** the relevant test file |
| 4 | **Triangulate** | Happy path + edge cases; force real logic |
| 5 | **Refactor** | Improve structure; tests stay green after each step |

---

## Assertion quality

- Tests must assert **behavior**, not implementation details.
- Avoid tautologies (`expect(true).toBe(true)`).
- Cover spec scenarios explicitly.
- Document evidence: which test file was run and result.

---

## Evidence table (per task)

| Task | Test file | RED | GREEN | Triangulate | Refactor |
|------|-----------|-----|-------|-------------|----------|
| … | … | ✓/✗ | ✓/✗ | ✓/✗ | ✓/✗ |

---

## STOP

- Do not skip RED.
- Do not implement multiple tasks in one TDD cycle without explicit user approval.
- Do not declare done without executed test evidence.
