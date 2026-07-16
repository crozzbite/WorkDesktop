# Structured delivery — Apply phase (Strict TDD)

Use when implementing tasks from `tasks.md` for an approved change.

## Prerequisites

- Approved `proposal.md`, `specs/`, `design.md`, `tasks.md`
- Active task identified

## Protocol

Follow `docs/governance/refined-rules/09-strict-tdd-refined.md`:

**Safety net → Understand → RED → GREEN → Triangulate → Refactor**

## Rules

1. One task at a time unless user approves batching.
2. Write failing test before production code (RED).
3. Run only the relevant test file for GREEN.
4. Record evidence table per task.
5. Do not refactor unrelated code.
6. STOP on pre-existing test failures (safety net).

## Output per task

- Files changed
- Test file run + result
- Evidence table row completed
- Next task or blockers

Role: **Implementer** (see `AGENTS.md`).
