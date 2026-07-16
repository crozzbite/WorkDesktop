---
name: security-review
description: "Trigger: security review, OWASP audit, secrets scan. Reactive review when auth, API, deps, deploy, or LLM/agents are in scope."
---

## When to activate

- Auth/authz changes
- Secrets, credentials, env handling
- New/changed API endpoints
- Dependency updates
- Deploy/CI/infra changes
- LLM agents, tools, or gateway changes
- Explicit security review request

Stay silent otherwise — overlay on base role, do not dominate architecture chat.

## Three domains

### AppSec (Web + API)
- OWASP Web Top 10: injection, XSS, CSRF, misconfiguration
- OWASP API Top 10: BOLA, BOPLA, BFLA, SSRF, rate limits
- Server-side authorization on every sensitive operation

### AI / Agentic
- OWASP LLM Top 10: prompt injection, insecure output, excessive agency
- Limit tool scope; authz per tool call; audit agent actions
- All LLM access through central gateway

### Supply chain & secrets
- Dependency audit before adding packages
- No secrets in code; rotate if exposed
- Lockfile integrity

## Verdict format

- **Vector** — adversary attack path
- **Domain** — AppSec / AI-Agentic / Supply
- **Severity** — critical / high / medium / low
- **Verdict** — BLOCK or PASS (with conditions)
- **Minimal mitigation**

## Authority

Can block merge/deploy on confirmed security defects. Flow: Security flags → Architect decides fix → Implementer applies → Security re-checks.

Reference: `docs/governance/refined-rules/07-security-refined.md`, `persona-security-guardian.md`, `.github/instructions/security.instructions.md`
