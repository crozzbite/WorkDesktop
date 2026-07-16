# Governance Rules — Version Index

> **Purpose:** Track which rule sets are current (✓) for portable / Copilot VS Code usage.
> **Updated:** 2026-07-10 (ISO 8601).

---

## Version convention

- **version:** Semantic (e.g. `1.0`). Bump when meaning or scope changes.
- **✓ (current):** Active for new work on this branch.
- **Location:** `docs/governance/` on branch `governance/copilot-portable`.

---

## Refined rules

| File | Version | Current | Description |
|------|---------|---------|-------------|
| `refined-rules/00-identity-refined.md` | 1.0 | ✓ | Identity, global bans, output sanitation |
| `refined-rules/hierarchy.md` | 1.0 | ✓ | Precedence when rules conflict |
| `refined-rules/02-patterns-refined.md` | 1.0 | ✓ | Architectural patterns |
| `refined-rules/05-cognitive-refined.md` | 1.0 | ✓ | LLM gateway, RAG, agents |
| `refined-rules/07-security-refined.md` | 1.0 | ✓ | Security context matrix |
| `refined-rules/08-workflow-refined.md` | 1.0 | ✓ | Workflow phases + STOP conditions |
| `refined-rules/09-agent-loops-refined.md` | 1.0 | ✓ | Agent loops: TDD, verify, adversarial review |
| `refined-rules/09-strict-tdd-refined.md` | 1.0 | ✓ | Strict TDD apply protocol |
| `refined-rules/persona-security-guardian.md` | 1.0 | ✓ | Security Guardian persona |
| `refined-rules/10-enterprise-agent-ruleset.md` | 1.2 | ✓ | Enterprise Agent Ruleset (portable): confidentiality, context, RACI ownership, scope, evidence + master priority |
| `glossary.md` | 1.0 | ✓ | Acronym/term glossary with DO/DON'T examples; lookup source for RULE_001 |

---

## Code rules

| File | Version | Current | Description |
|------|---------|---------|-------------|
| `code-rules/00-index.md` | 1.0 | ✓ | Code rules index |
| `code-rules/typescript.md` | 1.0 | ✓ | TypeScript DO / NEVER |
| `code-rules/python.md` | 1.0 | ✓ | Python DO / NEVER |
| `code-rules/angular.md` | 1.0 | ✓ | Angular DO / NEVER |
| `code-rules/fastapi.md` | 1.0 | ✓ | FastAPI DO / NEVER |
| `code-rules/tailwind.md` | 1.0 | ✓ | Tailwind DO / NEVER |

Copilot file-based rules: `.github/instructions/*.instructions.md`

## Agent skills (VS Code)

| Path | Description |
|------|-------------|
| `.github/skills/README.md` | Index |
| `.github/skills/openspec-*` | OpenSpec workflow (6 skills) |
| `.github/skills/strict-tdd` | Strict TDD apply micro-loop |
| `.github/skills/adversarial-review` | Quality gate before archive |
| `.github/skills/security-review` | OWASP overlay when security triggers |

---

## Constraints

| File | Version | Current | Description |
|------|---------|---------|-------------|
| `constraints/non-negotiables.md` | 1.0 | ✓ | Flat MUST / NEVER / FORBIDDEN |
| `constraints/by-pillar.md` | 1.0 | ✓ | By pillar view |
| `constraints/by-domain.md` | 1.0 | ✓ | By domain view |
| `constraints/economy.md` | 1.0 | ✓ | Cost / LLM / SaaS |

---

## Company policies (fill in on target machine)

| File | Status | Description |
|------|--------|-------------|
| `../company/security-policy.md` | placeholder | Enterprise security policy |
| `../company/coding-standards.md` | placeholder | Stack, package manager, style |
| `../company/compliance.md` | placeholder | Regulatory / internal compliance |
| `../company/cicd-standard.md` | placeholder | CI/CD and deploy gates |

---

## Sync from `master`

This branch is derived from `master` governance canon. When updating:

1. Cherry-pick or merge doc changes from `master`.
2. Re-neutralize identity-specific content (no brand personas, no personal profiles).
3. Bump versions in this index when meaning changes.
