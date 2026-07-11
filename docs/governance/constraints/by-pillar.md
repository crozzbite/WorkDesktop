---
version: 1.0
current: true
description: Constraints grouped by pillar (Bones, Brain, Shield, etc.).
---

# Constraints by Pillar

**Version:** 1.0 ✓ (current)

---

## Bones (Domain, Types, OpenSpec)

- MUST have OpenSpec (proposal → specs → design → tasks) before implementation.
- MUST define domain entities, value objects, and contracts (Pydantic/interfaces) before coding.
- NEVER implement without design alignment.
- Domain MUST NOT depend on Infrastructure (Clean Architecture).
- NEVER let modules import each other directly in a Modular Monolith; use interfaces.

---

## Brain (Cognitive, LLM, Agents)

- MUST route all LLM calls through the central Gateway.
- MUST version prompts and keep them in repo or registry.
- MUST use structured output (e.g. JSON) for programmatic use.
- NEVER call providers directly from feature code.
- NEVER send PII to the LLM without sanitization/policy.
- MUST use LangSmith (or equivalent) for agent/LLM observability when using LangGraph/LangChain.
- Treat LLM output as untrusted: validate and escape before SQL/HTML/shell.

---

## Shield (Security)

- NEVER commit secrets; rotate if exposed.
- NEVER roll your own crypto; use standard auth libraries.
- MUST check permissions on every request; object-level when applicable.
- Apply OWASP by context: Web (UI), API (REST/GraphQL), LLM, Agentic (see Rule 07 refined).
- Fail closed: if permission check fails or is ambiguous → deny.
- Secrets: env or secrets manager only; never in code or images.

---

## Ecosystem (Testing, Observability, CI/CD)

- MUST run tests before merge; they must pass.
- MUST run mutation tests for domain code.
- MUST have observability: logs (structured), metrics, traces (e.g. OpenTelemetry).
- NEVER merge with failing tests or failing mutation bar.
- CI: lint, format, type check, test, mutation (where required). Block merge on failure.

---

## Governance (ADRs, Versioning, Evolution)

- Document major decisions (ADR) when choosing DB, framework, or pattern.
- NEVER introduce a new strategic pattern without an ADR.
- API versioning: e.g. `/api/v1/`; never break v1 in place; new version for breaking changes.
- Use fitness functions (e.g. archunit-style) to enforce layer boundaries.

---

## Economy (Cost, LLM, SaaS)

- MUST track usage/cost per user or session for LLM-backed SaaS.
- MUST enforce limits per plan (e.g. Deliberations/month).
- Unit economics: optimize token use and prompts for margin (see `economy.md`).
