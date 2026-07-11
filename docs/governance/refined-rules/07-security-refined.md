---
version: 1.0
current: true
description: Security + context matrix (Web/API/LLM/Agentic). Refined Rule 07.
---

# Rule 07 Refined: Security (Context Matrix + Testing Logic)

**Version:** 1.0 ✓ (current)

---

## NEVER / FORBIDDEN

- **NEVER** commit secrets. Rotate immediately if one is exposed.
- **NEVER** roll your own crypto or custom auth. Use standard libraries.
- **NEVER** trust LLM output as input to SQL, HTML, or shell without validation/escaping.
- **FORBIDDEN:** Skipping server-side permission checks because “the UI hides the button.” Every API must verify the caller’s permission.

---

## When to apply which security rules (context matrix)

Not every control applies to every layer. Use this mapping:

| Context | Primary focus | Apply these |
|---------|----------------|-------------|
| **Web (UI)** | OWASP Web Top 10 | Injection, Broken Auth, XSS (CSP, encoding), Sensitive Data Exposure, CSRF (cookies sameSite). CORS, HTTPS, secure cookies. |
| **API (REST/GraphQL)** | OWASP **API** Top 10 | BOLA, Broken Auth, Broken Object Property Authorization, Unrestricted Resource Consumption, Broken Function Level Authorization, Sensitive Business Flows, SSRF, Misconfiguration, Inventory, Unsafe Consumption of APIs. Rate limit, input validation, object-level checks. |
| **LLM (chat, RAG, generation)** | OWASP **LLM** Top 10 | LLM01 Prompt Injection, LLM02 Insecure Output, LLM04 DoS, LLM06 Sensitive Disclosure, LLM07 Insecure Plugin, LLM09 Overreliance. Delimiters, output validation, PII sanitization. |
| **Agentic (agents that act)** | LLM + execution | All LLM Top 10 plus: LLM08 Excessive Agency (limit scope of tools), authz on every tool/API call, audit of actions. Treat agent as privileged; verify every downstream call. |

**Rule of thumb:** If it’s a web page → Web. If it’s an API endpoint → API. If it calls an LLM → LLM. If the LLM triggers tools/APIs/DB → Agentic.

---

## Testing logic (security)

- **Fail closed:** If the permission check fails or is ambiguous → deny access. Prefer `if (!hasPermission) deny` over “allow by default.”
- **Every endpoint:** Must enforce authorization on the server. Hiding the button is UX, not security.
- **Object-level:** Not only “is the user authenticated?” but “can this user access *this* resource?” (e.g. `WHERE user_id = auth.uid() AND id = :requestedId`).
- **Example test cases:** “User A cannot GET /orders of User B.” “Request with ‘ignore previous instructions’ in body is rejected or sanitized.” “Invalid token returns 401.”

---

## Zero Trust & secrets

- **Identity as perimeter:** No implicit trust for “internal” network. Service-to-service with token/identity.
- **Least privilege:** DB and service accounts with minimal required permissions (e.g. app user cannot `DROP TABLE`).
- **Secrets:** Dev = `.env.local` (gitignored). Prod = Parameter Store / Vault / platform secrets. Never in code or in images.
