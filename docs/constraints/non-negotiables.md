---
version: 1.0
current: true
description: Flat list MUST / NEVER / FORBIDDEN.
---

# Non-Negotiables (Flat List)

**Version:** 1.0 ✓ (current)

Single list for quick scan. Source: refined rules 00–10 + code rules.

---

## MUST

- Use **bun** as the only package manager.
- Write **code** in English (variables, commits, docs in code).
- Use **ISO 8601** for dates/times.
- Have **OpenSpec** (proposal → specs → design → tasks) before implementing features or design changes.
- Route **all LLM calls** through the central Gateway / LLMService.
- Define **OpenAPI (or equivalent) contract** before implementing API endpoints.
- **Validate and escape** LLM output before using it in SQL, HTML, or shell.
- **Run tests and (where required) mutation** before merge; they must pass.
- **Identify stakeholders and concerns** in architectural docs (ISO 42010).
- **Check permissions on every API request** server-side; object-level when applicable.
- Store **secrets** in env or secrets manager; never in code.
- Use **parameterized queries / ORM**; never raw concatenation for SQL.
- **Version** prompts and keep them in repo or registry.
- Apply **observability** (logs, metrics, traces); for agents, use LangSmith when using LangGraph/LangChain.
- **Track cost/usage** per user or session when offering LLM-backed SaaS.
- **Conditionals:** Prefer **positive (affirmative) conditions** in `if`: when both branches have logic, put the positive condition first so the main path is in the positive branch (SonarQube-style readability). Guard clauses like `if (!x) return;` are fine.
---

## NEVER

- Implement without OpenSpec when it’s a feature or design change.
- Use npm, yarn, or pnpm (only bun).
- Commit secrets or credentials; if committed, rotate immediately.
- Roll your own crypto or custom auth (use standard libs).
- Call LLM providers directly from feature/UI code.
- Expose API endpoints without a contract (OpenAPI first).
- Use NgModules in Angular 19+ (Standalone only).
- Use `any` in TypeScript without justification and documentation.
- Use `!important` in CSS to fix specificity.
- **Lead with a negated condition** in an `if` when both branches have logic and the main path would be clearer with the positive condition first; invert and put the affirmative case first.
- Choose Microservices only to fix code complexity.
- Let modules import each other directly in a Modular Monolith (use interfaces).
- Trust LLM output as safe input for SQL/HTML/shell.
- Merge with failing tests or (for domain) failing mutation.
- Skip server-side authorization because the UI hides the action.
- Put PII in LLM context without sanitization or policy.

---

## FORBIDDEN

- Exposing API without OpenAPI (or equivalent) contract.
- Starting with Microservices or Serverless before validating need (team/scale).
- Using LLM output as trusted input for SQL, HTML, or shell.
- Skipping permission checks because “the UI doesn’t show it.”
