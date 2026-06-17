---
version: 1.0
current: true
description: Matrix Rule × Pillar × Priority.
---

# Rules 01–10 × Pillars × Priority

**Version:** 1.0 ✓ (current)

Quick map: which rule covers which pillar and at what priority.

| Rule | Pillars | Priority | One-line constraint |
|------|---------|----------|---------------------|
| 01 Foundations | Quality (ISO 25010), Stakeholders (42010) | High | Every design identifies stakeholders and concerns; quality attributes in specs. |
| 02 Patterns | Architecture (Monolith, Micro, EDA, Serverless, CQRS) | High | Start Modular Monolith; NEVER microservices only for code complexity. |
| 03 Ecosystem | Testing, Observability, CI/CD | Critical | Tests MUST pass before merge; mutation required for domain; O11y (logs, metrics, traces). |
| 04 Data & API | DB (ACID/CAP), REST, OpenAPI | High | Contract first; NEVER expose API without spec; Postgres for core/financial. |
| 05 Cognitive | LLM Gateway, RAG, Agents, LangSmith | High | NEVER direct LLM calls; observability (LangSmith) for agent flows. |
| 06 Universal Arch | Layers (UI, Gateway, Domain, Cognitive, Infra, O11y) | High | Domain MUST NOT depend on Infrastructure. |
| 07 Security | OWASP Web/API/LLM, Zero Trust | Critical | Apply by context (Web vs API vs LLM vs Agentic); NEVER commit secrets. |
| 08 Workflow | Discovery, Design, Execution, Verification | High | STOP if OpenSpec missing; TDD in execution; verify before merge. |
| 09 Visual Logic | Diagrams, decision trees | Medium | Use for clarity; does not override 00–08. |
| 10 Governance | ADRs, versioning, fitness functions | Medium | Document major decisions; NEVER new pattern without ADR when it’s a strategic choice. |

**Pillar shorthand:** Bones = domain/types/OpenSpec. Brain = cognitive/LLM/agents. Shield = security. Ecosystem = test/O11y/CI. Governance = ADRs/versioning. Economy = cost/LLM/SaaS (see `docs/constraints/economy.md`).
