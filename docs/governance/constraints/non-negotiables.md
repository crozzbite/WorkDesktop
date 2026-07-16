---
version: 1.0
current: true
description: Flat list MUST / NEVER / FORBIDDEN (neutral).
---

# Non-Negotiables (Flat List)

**Version:** 1.0 ✓ (current)

Single list for quick scan. Enterprise overrides: `docs/company/`.

---

## MUST

- Use the **documented package manager** for the repo (see `docs/company/coding-standards.md`).
- Write **code** in English (variables, commits, docs in code).
- Use **ISO 8601** for dates/times.
- Have **approved spec artifacts** (proposal → specs → design → tasks) before feature or design implementation.
- Route **all LLM calls** through the central gateway/service.
- Define **OpenAPI (or equivalent) contract** before API endpoints.
- **Validate and escape** LLM output before SQL, HTML, or shell use.
- **Run tests** (and mutation where required) before merge.
- **Check permissions** on every API request server-side.
- Store **secrets** in env or secrets manager only.
- Use **parameterized queries / ORM** for SQL.
- Prefer **positive conditions** in `if` when both branches have logic.

---

## NEVER

- Implement features without approved spec artifacts.
- Mix package managers in one repository.
- Commit secrets or credentials.
- Roll your own crypto or custom auth.
- Call LLM providers directly from feature code.
- Expose API endpoints without a contract.
- Use `any` in TypeScript without justification.
- Trust LLM output as safe input for SQL/HTML/shell.
- Merge with failing tests.
- Skip server-side authorization because the UI hides the action.

---

## FORBIDDEN

- API without contract-first design.
- Skipping permission checks because "the UI doesn't show it."
- Using LLM output as trusted input for SQL, HTML, or shell.
