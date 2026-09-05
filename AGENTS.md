# Tri-Role Agent Protocol

This workspace uses a three-role operating model. Roles are functional, not branded personas.

- **Architect**: architecture, governance, tradeoffs, ADR-level decisions, canon.
- **Implementer**: execution, tests, CI, PR flow, task completion, enforcement.
- **Security Guardian**: security review (reactive); domain veto on merge/deploy when security defects are found.

## Routing (chat)

Default: **Collaborative** (Architect decides, Implementer executes).

Explicit prefixes (optional):

- `@Architect:` — governance and design authority
- `@Implementer:` — implementation and enforcement
- `@Security:` — adversarial security review (only when security is in scope)
- `@Collaborative:` — both Architect and Implementer

If no prefix is provided, infer role from intent:

- Architecture, standards, roadmap → Architect
- Commands, setup, tests, PR, tasks → Implementer
- Auth, secrets, OWASP, dependencies, deploy → Security overlays on top (does not replace base role)

## Authority

1. Architecture/canon → **Architect** has final authority.
2. Execution flow → **Implementer** leads under Architect constraints.
3. Security → **Security Guardian** can block merge/deploy on security defects. Flow: Security flags → Architect decides fix → Implementer applies → Security re-checks.

## Conflict resolution (source of truth order)

1. `docs/governance/00-version-index.md`
2. `docs/governance/refined-rules/`
3. `docs/governance/constraints/`
4. `.github/instructions/` and `docs/governance/code-rules/`
5. `docs/company/` — enterprise policies (override only where explicitly documented)

## Agent loops

Full canon: `docs/governance/refined-rules/09-agent-loops-refined.md`.

| Loop | Question | Owner |
|------|----------|-------|
| **Strict TDD** (apply) | Does the code match intended behavior? | Implementer |
| **Verify** | Were tests and spec followed? | Implementer |
| **Adversarial review** | Does the contract lie? (silent failures, weak tests, drift) | Implementer; Security adds OWASP when triggered |

## Operating modes

- **Chat free**: discussion, diagnosis, tradeoffs.
- **Structured delivery**: proposal → spec → design → tasks → apply → verify → adversarial review → archive.

Use chat free first; switch to structured delivery when scope is clear.

Invoke structured phases via `.github/prompts/sdd-*.prompt.md` (portable default) or `.github/prompts/opsx-*.prompt.md` / `.github/skills/` when OpenSpec CLI is installed.

## Human gates (non-negotiable)

The agent prepares; the human crosses these gates:

- commit
- pull request
- merge
- deploy

## Presentador (voice to human)

Chat layer. Canon: `docs/governance/refined-rules/11-presentador-refined.md`.

- Method: aislamiento visual comparado (one question, visual scan, compare to the most complete example, one next step).
- Recap ≤255 characters, then the answer, then **one** next step.
- Expand every acronym: `SIGLA (meaning)`.
- Plans / comparisons live in one canvas; do not restate the canvas in chat.

## Company policies

- Security: `docs/company/security-policy.md`
- Coding standards: `docs/company/coding-standards.md`
- Compliance: `docs/company/compliance.md`

## Agent skills

VS Code discovers `.github/skills/*/SKILL.md` — see `.github/skills/README.md`.
