---
version: 1.0
current: true
description: Identity, global bans, output sanitation. Rule 00 (neutral).
---

# Rule 00: Identity & Core Logic

**Version:** 1.0 ✓ (current)

---

## Global Ban List (NEVER / FORBIDDEN)

Apply first. No exception unless `docs/company/` documents an override.

- **NEVER** implement features or code without approved artifacts (proposal → specs → design → tasks).
- **NEVER** commit secrets, API keys, or credentials. If exposed, rotate immediately.
- **NEVER** roll your own crypto or custom auth. Use standard libraries.
- **NEVER** call LLM providers directly from feature code. Route through central gateway/service.
- **NEVER** merge or ship without passing tests and (for domain) mutation tests when required.
- **NEVER** use `any` in TypeScript without justification and documentation.
- **FORBIDDEN:** Exposing API endpoints without an OpenAPI (or equivalent) contract first.

---

## Mandates (what TO do)

1. **Package manager:** Use the **single documented manager** per repo (`docs/company/coding-standards.md`). Do not mix managers in one repo.
2. **Language protocol:**
   - **Code:** English (variables, commits, in-code docs).
   - **Reasoning to user:** Team default (Spanish if not specified).
3. **Time:** ISO 8601 (`YYYY-MM-DDTHH:mm:ssZ`).
4. **Spec-first:** No UI without schema; no logic without types. Approved artifacts before implementation.

---

## Decision flowchart

Before acting:

1. **User request** → Is it a code change?
   - **No** → Answer with context; use Socratic questioning when useful.
   - **Yes** → Are spec artifacts approved?
     - **No** → STOP. Create or continue structured delivery (proposal → specs → design → tasks).
     - **Yes** → Does it match approved design?
       - **No** → Refuse; request alignment.
       - **Yes** → Implement with tests and verification gates.

---

## Output sanitation

- Do not paste secrets, tokens, or PII in chat or commits.
- Redact credentials in logs and examples.
- Prefer minimal diffs; do not refactor unrelated code.
