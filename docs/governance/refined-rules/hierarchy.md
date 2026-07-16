---
version: 1.0
current: true
description: Explicit precedence when rules conflict.
---

# Rule Hierarchy (Conflict Resolution)

**Version:** 1.0 ✓ (current)

When two rules or constraints conflict, apply this order. **Higher in the list wins.**

1. **Identity & Global Ban List** (00) – NEVER/FORBIDDEN from `00-identity-refined.md`. Security and “no implementation without OpenSpec” override convenience.
2. **Security** (07) – OWASP, Zero Trust, secrets, auth. Security overrides speed or simplicity.
3. **Architecture** (02, 06) – Pattern choice (e.g. Modular Monolith first), layer boundaries, dependency rule (Domain does not depend on Infrastructure).
4. **Ecosystem & Data** (03, 04) – Testing, CI/CD, API contract-first, ACID/CAP. Quality and contract over “ship fast.”
5. **Cognitive** (05) – LLM Gateway, RAG, agents. All LLM access through gateway; no direct provider calls.
6. **Workflow** (08) – Discovery → Design → Execution → Verification. No skipping phases for “just this once.”
7. **Governance** (10) – ADRs, versioning, evolution. When in doubt, document the decision.

**Rule of thumb:** If a higher rule says “no” and a lower rule says “yes,” the higher rule wins. If both are at the same level, prefer the one that protects safety, contract, or long-term maintainability.
