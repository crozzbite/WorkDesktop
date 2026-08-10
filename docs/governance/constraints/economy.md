---
version: 1.0
current: true
description: Cost / LLM / SaaS rules for usage-metered products.
---

# Economy (Cost, LLM, SaaS)

**Version:** 1.0 ✓ (current)

Reference pattern: usage-metered SaaS (B2Prosumer or B2B) with a clear unit of consumption (e.g. session, request, deliberation).

---

## MUST

- **Track usage** per user or session for any LLM-backed product (e.g. tokens per session / request).
- **Enforce limits** per plan (document quotas in specs and company pricing docs).
- **Route all LLM traffic** through the Gateway so every token is countable and (optionally) billable.
- **Optimize prompts and model choice** for unit economics (efficient token use).
- **Define a clear unit of consumption** (e.g. “request”, “active user”, product-specific unit) and document it in specs.

---

## NEVER

- Expose LLM features without **usage limits** or **tracking** in a commercial/SaaS context.
- Allow unbounded token consumption per user without rate limits or caps.
- Bypass the Gateway for “quick” LLM calls (cost and observability get lost).

---

## Good practices

- **Unit economics:** High margin from efficient system prompts and controlled context size.
- **Pricing tiers:** Free (trial) → paid tiers with clear quotas.
- **Cost tracking in architecture:** Every token counted against the billing/session entity.
- **Database:** e.g. `tokensUsed`, `usageLastResetDate` (or equivalent) for billing and limits.
- **MRR targets** and break-even planned per phase; document in business/architecture docs.

---

When adding new SaaS or LLM-backed products, align with these rules and prefer patterns such as BFF, job queue, usage in DB, and a standard payments provider (e.g. Stripe) where applicable.
