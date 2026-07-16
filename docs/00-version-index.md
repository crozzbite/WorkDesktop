# SkullRender Rules – Version Index

> **Purpose:** Track which rule sets are current (✓) so we can compare and roll back if needed.  
> **Updated:** 2026-06-25 (ISO 8601).

---

## Version convention

- **version:** Semantic (e.g. `1.0`). Bump when we change meaning or scope of a rule set.
- **✓ (current):** Only the rule files we actively use for new work. One version per scope is current.
- **Location:** `docs/` = refined/current; `WorkSpace/.gemini/` = legacy reference (do not edit for "current" work).

---

## Refined rules (architecture, process, constraints)

> **MIGRATED (2026-07-15):** `docs/refined-rules/` was **removed** in commit `f6ba229`
> ("remove SkullRender leaks" — fork-audit cleanup). Live canon is
> **`docs/governance/refined-rules/`** and its index `docs/governance/00-version-index.md`.
> SkullRender-specific files (pillars matrix, programmer profile, persona Cerbero, CI/CD standard)
> have no neutral equivalent — recover from git history if needed.

| File | Version | Current | Description |
|------|---------|---------|-------------|
| `docs/refined-rules/*` (all rows previously here) | — | removed | See `docs/governance/refined-rules/` + git history (`f6ba229^`) |
| `docs/governance/refined-rules/10-enterprise-agent-ruleset.md` | 1.2 | ✓ | Enterprise Agent Ruleset (portable): 8 adopted rules (RACI ownership) + master priority |
| `docs/governance/glossary.md` | 1.0 | ✓ | Acronym/term glossary with DO/DON'T examples; lookup source for RULE_001 |

---

## Code rules (languages & tools – DO / NEVER + snippets)

| File | Version | Current | Description |
|------|---------|---------|-------------|
| `docs/code-rules/00-index.md`   | 1.0 | ✓ | Code rules index and versioning |
| `docs/code-rules/typescript.md` | 1.0 | ✓ | TypeScript: DO / NEVER + snippets |
| `docs/code-rules/python.md`     | 1.0 | ✓ | Python: DO / NEVER + snippets |
| `docs/code-rules/angular.md`    | 1.1 | ✓ | Angular 19+: DO / NEVER + snippets; `inject()` not constructor DI |
| `docs/code-rules/fastapi.md`    | 1.0 | ✓ | FastAPI: DO / NEVER + snippets |
| `docs/code-rules/tailwind.md`   | 1.0 | ✓ | Tailwind (SkullRender): DO / NEVER + snippets |

---

## Constraints (extracted MUST/NEVER by view)

| File                                  | Version | Current | Description |
|-----|---------|---------|-------------|
| `docs/constraints/non-negotiables.md` | 1.0 | ✓ | Flat list MUST / NEVER / FORBIDDEN |
| `docs/constraints/by-pillar.md`       | 1.0 | ✓ | By pillar (Bones, Brain, Shield, etc.) |
| `docs/constraints/by-domain.md`       | 1.0 | ✓ | By domain (front, back, API, DB, testing, etc.) |
| `docs/constraints/economy.md`         | 1.0 | ✓ | Cost / LLM / SaaS (from Phylactery-Bridge) |

---

## Supporting docs (no version; reference only)

| File | Description |
|------|-------------|
| `docs/00-comparison-index.md` | Plan summary, sources, comparison with internet rules |
| `docs/prompt-engineering-notes.md` | Prompt engineering sources and practices |

---

## Legacy (previous – not current)

| Location | Note |
|----------|------|
| `WorkSpace/.gemini/skullrender-rules.md` | Master index (reference) |
| `WorkSpace/.gemini/SkullRender-rules-00-identity.md` … `-10-governance.md` | Original 00–10; keep for comparison. **Current** equivalent is `docs/refined-rules/` + `docs/code-rules/`. |

---

**Rule of thumb:** If it has **✓** in the table above, it's the version we use for new work. When we publish a new batch of rules, bump versions and move ✓ to the new set.
