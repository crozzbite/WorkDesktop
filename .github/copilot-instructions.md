# Project governance — Copilot instructions

## Language protocol

- **Code** (variables, commits, in-repo docs): English.
- **Explanations to the user**: Spanish (team default; change if needed).
- **Dates/times**: ISO 8601.

## Global bans (NEVER / FORBIDDEN)

Apply first. No exceptions unless `docs/company/` explicitly overrides.

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

Higher wins — full detail: `docs/governance/refined-rules/hierarchy.md`

1. Identity & global bans
2. Security (OWASP, secrets, auth)
3. Architecture (patterns, layer boundaries)
4. Ecosystem & data (testing, CI/CD, contracts)
5. Cognitive (LLM gateway, RAG, agents)
6. Workflow (discovery → design → execution → verification)
7. Governance (ADRs, versioning)

## Workflow STOP conditions

Do not proceed until resolved — see `docs/governance/refined-rules/08-workflow-refined.md`:

- Feature/design change without approved spec artifacts.
- Implementation not aligned with approved design.
- Tests failing before merge.
- Adversarial review not approved before archiving a change (when process requires it).

## Tri-role protocol

See root `AGENTS.md` for Architect / Implementer / Security Guardian routing.

## Stack (customize per project)

Update these lines for each target repository:

- Package manager: **configure per repo** (document choice in `docs/company/coding-standards.md`)
- Frontend: Angular 19+ (Standalone, Signals, OnPush) — or per project
- Backend: NestJS / FastAPI — or per project
- Styling: Tailwind — or per project

## References

- Governance index: `docs/governance/00-version-index.md`
- Security: `docs/governance/refined-rules/07-security-refined.md`
- Workflow: `docs/governance/refined-rules/08-workflow-refined.md`
- Agent loops: `docs/governance/refined-rules/09-agent-loops-refined.md`
- Strict TDD: `docs/governance/refined-rules/09-strict-tdd-refined.md`
- Constraints: `docs/governance/constraints/non-negotiables.md`
- Security persona: `docs/governance/refined-rules/persona-security-guardian.md`

## Company policies

- `docs/company/security-policy.md`
- `docs/company/coding-standards.md`
- `docs/company/compliance.md`
