# SkullRender Rules – Version Index

> **Purpose:** Track which rule sets are current (✓) so we can compare and roll back if needed.  
> **Updated:** 2025-03-15 (ISO 8601).

---

## Version convention

- **version:** Semantic (e.g. `1.0`). Bump when we change meaning or scope of a rule set.
- **✓ (current):** Only the rule files we actively use for new work. One version per scope is current.
- **Location:** `docs/` = refined/current; `WorkSpace/.gemini/` = legacy reference (do not edit for "current" work).

---

## Refined rules (architecture, process, constraints)

| File | Version | Current | Description |
|------|---------|---------|-------------|
| `docs/refined-rules/00-identity-refined.md`  | 1.0 | ✓ | Identity, Global Ban List, Output Sanitation |
| `docs/refined-rules/01-10-pillars-matrix.md` | 1.0 | ✓ | Matrix Rule × Pillar × Priority |
| `docs/refined-rules/hierarchy.md`            | 1.0 | ✓ | Explicit precedence when rules conflict |
| `docs/refined-rules/02-patterns-refined.md`  | 1.0 | ✓ | Architectural patterns + when to use each |
| `docs/refined-rules/05-cognitive-refined.md` | 1.0 | ✓ | Cognitive layer + LangSmith observability |
| `docs/refined-rules/07-security-refined.md`  | 1.0 | ✓ | Security + context matrix (Web/API/LLM/Agentic) |
| `docs/refined-rules/08-workflow-refined.md`  | 1.0 | ✓ | Workflow phases + SDLC alignment + STOPs |
| `docs/refined-rules/programmer-profile-zzorc.md` | 1.0 | ✓ | Perfil de personalidad de zzorc (pacto samurai-espada, FODA, Ley del Entierro, comportamientos activos) |

---

## Code rules (languages & tools – DO / NEVER + snippets)

| File | Version | Current | Description |
|------|---------|---------|-------------|
| `docs/code-rules/00-index.md`   | 1.0 | ✓ | Code rules index and versioning |
| `docs/code-rules/typescript.md` | 1.0 | ✓ | TypeScript: DO / NEVER + snippets |
| `docs/code-rules/python.md`     | 1.0 | ✓ | Python: DO / NEVER + snippets |
| `docs/code-rules/angular.md`    | 1.0 | ✓ | Angular 19+: DO / NEVER + snippets |
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
