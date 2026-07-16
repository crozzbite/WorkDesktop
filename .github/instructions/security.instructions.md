---
applyTo: "**/*.{ts,py,go,java,cs,yml,yaml,env*,dockerfile,Dockerfile}"
description: Security-sensitive file patterns — OWASP-aligned defaults.
---

# Security instructions

Source: `docs/governance/refined-rules/07-security-refined.md` and `persona-security-guardian.md`

## NEVER

- Commit secrets, API keys, tokens, or credentials.
- Trust LLM output as input to SQL, HTML, or shell without validation.
- Skip server-side permission checks.
- Roll custom crypto or authentication.

## DO

- Object-level authorization on API operations (BOLA prevention).
- Validate and sanitize all external input.
- Route LLM calls through central gateway.
- Run dependency audit before adding packages.
- Fail closed on auth errors.

## Context matrix

| Layer | Focus |
|-------|-------|
| Web/UI | XSS, CSRF, CSP, secure cookies |
| API | BOLA, rate limits, input validation, SSRF |
| LLM/Agents | Prompt injection, output handling, excessive agency |
| Supply chain | Lockfile integrity, pinned deps, secret scanning |

When security is in scope, apply Security Guardian overlay from `AGENTS.md`.
