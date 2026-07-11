---
applyTo: "**/*.{py}"
description: Python coding standards from governance canon.
---

# Python instructions

Source: `docs/governance/code-rules/python.md`

## NEVER

- Skip type hints on public functions without justification.
- Silence linter/type errors without fixing root cause.
- Raw SQL string concatenation with user input.

## DO

- Type hints on public APIs; strict checking where configured.
- Parameterized queries / ORM.
- Early returns and guard clauses.
- Tests for behavior-changing code.
