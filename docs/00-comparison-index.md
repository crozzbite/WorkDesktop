# SkullRender Rules – Comparison & Plan Index

> **Purpose:** Summarize what changed, where prompt-engineering ideas came from, and how we compare to public rule sets.  
> **Updated:** 2025-03-15.

---

## 1. What we did

- **Refined** architecture/process rules (00, 02, 05, 07, 08) with explicit **prohibitions** (NEVER/DO NOT/FORBIDDEN) and **hierarchy**.
- **Extracted** constraints into flat and grouped views (non-negotiables, by-pillar, by-domain, economy).
- **Added** a **code-rules** section: per-language and per-tool DO/NEVER + snippets (TypeScript, Python, Angular, FastAPI, Tailwind).
- **Versioned** all new rule files; **✓** marks current set in `00-version-index.md`.
- **Did not modify** original rules in `WorkSpace/.gemini/`; they remain as reference.

---

## 2. Sources for prompt engineering

| Topic | Source | URL / note |
|-------|--------|------------|
| Constraint Engineering (negative constraints, 3 phases) | Prompt Sigma | https://promptsigma.com/constraint-engineering-negative-constraints-power.html |
| Cursor Rules (structure, priority, best practices) | Cursor Docs | https://cursor.com/docs/rules |
| Instruction hierarchy / “lost in the middle” | Research summary | Optimal constraint length ~150–300 words; critical at start/end |
| Real rule examples | cursorrules-collection (security, clean-code) | https://github.com/nedcodes-ok/cursorrules-collection |

---

## 3. How we compare to “internet” rules

| Dimension | Typical public rules | SkullRender (ours) |
|-----------|----------------------|---------------------|
| **Scope** | By language/framework/tool | By **phase** (identity, foundations, patterns, ecosystem, data, cognitive, arch, security, workflow, governance) + **code rules** |
| **Process** | Little or none | OpenSpec, flowchart, Gauntlet, TDD in workflow |
| **Prohibitions** | Some “never” | **Global Ban List** + MUST/NEVER in every refined rule |
| **Hierarchy** | Unwritten | Explicit in `hierarchy.md` |
| **Code-level** | Strong (snippets, DO/NEVER per tech) | **Closed gap** with `docs/code-rules/` (TS, Python, Angular, FastAPI, Tailwind) |
| **Versioning** | Rare | **Version index** + ✓ for current set |

---

## 4. Where to look for what

- **Current rules (✓):** `docs/00-version-index.md` → then the files marked ✓.
- **Identity & bans:** `docs/refined-rules/00-identity-refined.md`.
- **When to use which architecture:** `docs/refined-rules/02-patterns-refined.md`.
- **Security by context (Web/API/LLM/Agentic):** `docs/refined-rules/07-security-refined.md`.
- **Code DO/NEVER + snippets:** `docs/code-rules/00-index.md` and the per-language/per-tool files.
- **Economy (cost/LLM/SaaS):** `docs/constraints/economy.md`.
- **Prompt-engineering rationale:** `docs/prompt-engineering-notes.md`.
