# Project governance — Copilot instructions (neutral)

> Place this file at `.github/copilot-instructions.md` in the target repository.
> Customize company policy paths and stack choices before committing.

## Language protocol

- **Code** (variables, commits, in-repo docs): English.
- **Explanations to the user**: Spanish (or your team default).
- **Dates/times**: ISO 8601.

## Global bans (NEVER / FORBIDDEN)

Apply first. No exceptions unless company policy explicitly overrides.

- NEVER implement features or design changes without approved artifacts (proposal → specs → design → tasks).
- NEVER commit secrets, API keys, or credentials.
- NEVER merge or ship with failing tests.
- NEVER call LLM providers directly from feature code — use the central gateway/service.
- NEVER expose API endpoints without a defined contract (OpenAPI or equivalent).
- FORBIDDEN: rolling custom crypto or auth from scratch — use standard libraries.

## MUST

- Validate and escape LLM output before SQL, HTML, or shell use.
- Check permissions on every API request server-side.
- Store secrets in env or secrets manager only.
- Use parameterized queries / ORM for SQL.
- Prefer positive (affirmative) conditions in `if` when both branches have logic.

## Rule hierarchy (when rules conflict)

Higher wins:

1. Identity & global bans
2. Security (OWASP, secrets, auth)
3. Architecture (patterns, layer boundaries)
4. Ecosystem & data (testing, CI/CD, contracts)
5. Cognitive (LLM gateway, RAG, agents)
6. Workflow (discovery → design → execution → verification)
7. Governance (ADRs, versioning)

## Workflow STOP conditions

Do not proceed until resolved:

- Feature/design change without approved spec artifacts.
- Implementation not aligned with approved design.
- Tests failing before merge.
- Adversarial review not approved before archiving a change (if your process requires it).

## Tri-role protocol

See root `AGENTS.md` for Architect / Implementer / Security Guardian routing.

## Stack (customize per project)

<!-- INSERT your company stack -->
- Package manager: (e.g. npm, pnpm, bun — pick one per repo)
- Frontend: (e.g. Angular 19+, React, etc.)
- Backend: (e.g. NestJS, FastAPI, etc.)
- Styling: (e.g. Tailwind)

## References

- Governance index: `docs/governance/00-version-index.md`
- Security: `docs/governance/refined-rules/07-security-refined.md`
- Workflow: `docs/governance/refined-rules/08-workflow-refined.md`
- Agent loops: `docs/governance/refined-rules/09-agent-loops-refined.md`
- Constraints: `docs/governance/constraints/non-negotiables.md`

## Company policies

<!-- INSERT paths -->
- Add links to internal security, compliance, and coding policy documents.
