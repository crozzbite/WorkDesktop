# Tri-Role Agent Protocol (neutral)

This workspace uses a three-role operating model. No fictional branding — roles are functional.

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

1. Architecture/canon → **Architect** has final say.
2. Execution flow → **Implementer** leads under Architect constraints.
3. Security → **Security Guardian** can block merge/deploy on security defects. Flow: Security flags → Architect decides fix → Implementer applies → Security re-checks.

## Conflict resolution (source of truth order)

1. `docs/governance/00-version-index.md`
2. `docs/governance/refined-rules/`
3. `docs/governance/constraints/`
4. `docs/code-rules/` or `.github/instructions/`
5. Company policy documents (add your paths here)

## Agent loops

- **Strict TDD** (during implementation): behavior-first micro-loop per task.
- **Verify**: tests and spec alignment before closing a change.
- **Adversarial review** (once per complete change): silent failures, weak tests, drift; Security adds OWASP criteria when triggered.

## Operating modes

- **Chat free**: discussion, diagnosis, tradeoffs.
- **Structured delivery**: proposal → spec → design → tasks → apply → verify → adversarial review → archive.

Use chat free first; switch to structured delivery when scope is clear.

## Human gates (non-negotiable)

The agent prepares; the human crosses these gates:

- commit
- pull request
- merge
- deploy

## Company policies

<!-- INSERT: links or paths to your company policies -->
- Security policy: `docs/company/security-policy.md` (placeholder)
- Coding standards: `docs/company/coding-standards.md` (placeholder)
- Compliance: `docs/company/compliance.md` (placeholder)
