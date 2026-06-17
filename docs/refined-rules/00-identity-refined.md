---
version: 1.0
current: true
description: Identity, Global Ban List, Output Sanitation. Refined Rule 00.
---

# Rule 00 Refined: Identity & Core Logic

**Version:** 1.0 ✓ (current)

---

## Phase A: Global Ban List (NEVER / FORBIDDEN)

Apply these first. No exception.

- **NEVER** implement features or code without OpenSpec artifacts (proposal → specs → design → tasks). If they are missing, STOP and trigger OpenSpec workflow.
- **NEVER** use npm, yarn, or pnpm. **ONLY** `bun` for package management.
- **NEVER** commit secrets, API keys, or credentials. If a secret is committed, treat it as burned; rotate immediately.
- **NEVER** roll your own crypto or custom auth from scratch. Use standard libraries (e.g. Clerk, Firebase Auth, established OAuth).
- **NEVER** call LLM providers (OpenAI, Anthropic, etc.) directly from feature code. All calls MUST go through the central LLM Gateway / service.
- **NEVER** merge or ship without passing tests and (for domain) mutation tests. No “skip tests for now.”
- **NEVER** use NgModules in Angular 19+. Use Standalone Components only.
- **NEVER** use `any` in TypeScript unless explicitly justified and documented. Prefer strict types.
- **NEVER** use `!important` in CSS to fix specificity. Fix the cascade and layers instead.
- **FORBIDDEN:** Exposing API endpoints without an OpenAPI (or equivalent) contract defined first.

---

## Phase B: Mandamientos (What TO Do)

1. **Package manager:** Use **`bun`** only. (`bun install`, `bun run dev`, `bun add`.)
2. **Language protocol:**
   - **Code:** Strict **ENGLISH** (variables, commits, docs in code).
   - **Reasoning:** Native **ESPAÑOL** (context, decisions, explanations to the user).
3. **Time:** **ISO 8601** only (`YYYY-MM-DDTHH:mm:ssZ`).
4. **Bones first:** No UI without schema; no logic without types. **OpenSpec** is the ritual (proposal → specs → design → tasks).

---

## Flowchart (unchanged)

Before acting:

1. **User request** → Is it a code change?
   - **No** → Consult knowledge base → Answer in Socratic Spanish.
   - **Yes** → Has OpenSpec artifacts?
     - **No** → STOP. Trigger OpenSpec (proposal → specs → design → tasks) → then implementation.
     - **Yes** → Does it match the design?
       - **No** → Refuse and request alignment.
       - **Yes** → Execute the Gauntlet (tests, types, mutation) → Pass? Commit & archive. Fail? Refactor loop.

---

## Phase C: Output Sanitation (Before Commit / Merge)

Right before considering the task done:

- Confirm **no** secrets or credentials in code or logs.
- Confirm **no** `any` (or justified and documented).
- Confirm **no** `!important` introduced.
- Confirm tests pass and (where required) mutation score is acceptable.
- Confirm Domain does not depend on Infrastructure (Clean Architecture).

---

## Library of Laws (reference)

- Rule 01: Foundations (ISO)
- Rule 02: Architectural Patterns
- Rule 03: Ecosystem (Test / CI/CD)
- Rule 04: Data & API
- Rule 05: Cognitive Layer (AI/LLM)
- Rule 06: Universal Architecture
- Rule 07: Security
- Rule 08: Workflow
- Rule 09: Visual Logic
- Rule 10: Governance

Detailed refined content: see other files in `docs/refined-rules/` and `docs/constraints/`.
