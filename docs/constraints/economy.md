---
version: 1.0
current: true
description: Cost / LLM / SaaS rules. Based on Phylactery-Bridge.
---

# Economy (Cost, LLM, SaaS)

**Version:** 1.0 ✓ (current)

Reference project: **Phylactery-Bridge** (SaaS, B2Prosumer, Deliberations as unit of consumption).

---

## MUST

- **Track usage** per user or session for any LLM-backed product (e.g. tokens per ValidationSession / Deliberation).
- **Enforce limits** per plan (e.g. 5 Deliberations/month free, 80 Plus, 300 Pro).
- **Route all LLM traffic** through the Gateway so every token is countable and (optionally) billable.
- **Optimize prompts and model choice** for unit economics (e.g. high margin >60% via efficient token use).
- **Define a clear unit of consumption** (e.g. “Deliberation”, “request”, “active user”) and document it in specs.

---

## NEVER

- Expose LLM features without **usage limits** or **tracking** in a commercial/SaaS context.
- Allow unbounded token consumption per user without rate limits or caps.
- Bypass the Gateway for “quick” LLM calls (cost and observability get lost).

---

## Good practices (from Phylactery-Bridge)

- **Unit economics:** High margin from efficient system prompts and controlled context size.
- **Pricing tiers:** Free (trial) → Plus → Pro with clear Deliberation/request quotas.
- **Cost tracking in architecture:** e.g. “Every token is counted and billed to the ValidationSession” (TECHNICAL_ARCHITECTURE.md).
- **Database:** e.g. `tokensUsed`, `usageLastResetDate` (or equivalent) for billing and limits.
- **MRR targets** and break-even planned per phase (e.g. Q2 beta, Q3 launch, Q4 scale); document in business/architecture docs.

---

When adding new SaaS or LLM-backed products, align with these rules and reference Phylactery-Bridge for patterns (BFF, job queue, usage in DB, Stripe for payments).
