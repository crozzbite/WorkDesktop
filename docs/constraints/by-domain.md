---
version: 1.0
current: true
description: Constraints by domain (front, back, API, DB, testing, etc.).
---

# Constraints by Domain

**Version:** 1.0 ✓ (current)

---

## Frontend (UI, Angular, Tailwind)

- Angular 19+: Standalone Components only; NEVER NgModules.
- State: Signals (and SignalStore where needed); avoid manual RxJS subscriptions for new code.
- Change detection: OnPush.
- NEVER `any`; NEVER `!important` to fix specificity.
- Use Tailwind for styling; follow SkullRender aesthetic (see code-rules/tailwind.md).
- Security: sanitize user input (e.g. DOMPurify); secure storage for tokens (e.g. httpOnly cookies preferred).

---

## Backend (Python, FastAPI)

- Use **uv** for dependencies; **pyproject.toml**; **mypy** strict; **ruff** for lint/format.
- NEVER Pokemon exception handling (`except Exception:`); catch specific exceptions.
- No God Objects: single responsibility; refactor if > ~500 LOC in one place.
- Domain layer: pure logic, no framework imports.
- See code-rules/python.md and fastapi.md.

---

## API (REST, OpenAPI)

- Contract first: define OpenAPI (or equivalent) before implementing.
- NEVER expose endpoints without a spec.
- Status codes: 200, 201, 204, 400, 401, 403, 404, 429, 500 used consistently.
- Versioning: e.g. `/api/v1/`; breaking changes → new version.
- Apply OWASP API Top 10 (see Rule 07 refined); rate limit and validate input.

---

## Database

- Core/financial/audit data: ACID-compliant (e.g. Postgres).
- CAP: prefer CP for core domain; AP for search/cache when needed.
- Use ORM or parameterized queries; NEVER raw string concatenation for SQL.
- Least privilege: app user must not have DROP or broad admin.

---

## Testing

- Pyramid: unit (majority), integration, E2E (minority).
- Domain: 100% coverage target; mutation testing required.
- MUST pass before merge; MUST NOT decrease coverage without explicit decision.
- Tools: pytest (backend), Jest (frontend); Playwright for E2E when needed.

---

## CI/CD

- Pre-commit: lint, format, type check.
- PR: unit + mutation (blocker).
- Merge: deploy to staging; E2E on critical paths.
- IaC: no manual server config; repo as source of truth where possible.

---

## Security (cross-cutting)

- Web: OWASP Web Top 10, CSP, secure cookies, CORS.
- API: OWASP API Top 10, BOLA, authz, rate limit.
- LLM: OWASP LLM Top 10, prompt injection, output validation, PII.
- Agentic: LLM + scope of tools, authz on every action.
