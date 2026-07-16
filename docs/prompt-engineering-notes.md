# Prompt Engineering Notes (for SkullRender Rules)

> Reference only. No version field.  
> **Updated:** 2025-03-15.

---

## 1. Constraint Engineering (Prompt Sigma)

- **Negative constraints** (what NOT to do) often work better than only positive ones: they prune the model’s output space.
- **Three-phase structure:**
  1. **Global Ban List** (start of prompt): non-negotiable NEVER/FORBIDDEN.
  2. **Local Rule Set**: what TO do (tasks, patterns).
  3. **Output Sanitation** (before execution): final constraints on format and safety.
- Use **NEVER**, **DO NOT**, **FORBIDDEN** as clear “kill switches” for bad behavior.

**Source:** https://promptsigma.com/constraint-engineering-negative-constraints-power.html

---

## 2. Cursor / Agent Rules (official)

- Rules in `.cursor/rules` as markdown; `.mdc` with frontmatter for `alwaysApply`, `globs`, `description`.
- **Priority:** Team > Project > User > Legacy.
- **Best practices:** Reference files instead of copying; keep under ~500 lines; be concrete; include examples; avoid vague or rare edge-case guidance.

**Source:** https://cursor.com/docs/rules

---

## 3. Length and position

- Very long constraint blocks can degrade performance (“lost in the middle”).
- **Critical constraints:** place at **beginning** and **end** of context; avoid burying in the middle.
- Short, dense constraint lists (~150–300 words) often work better than long prose.

---

## 4. Examples we used for “code rules”

- **cursorrules-collection:** security.mdc, clean-code.mdc (concrete DO/NEVER, bullets, no fluff).
- **SkullRender stack:** Angular 19 (Standalone, Signals), FastAPI (uv, mypy, ruff), Tailwind, bun.

Our **code-rules** section in `docs/code-rules/` brings the same style (DO/NEVER + snippets) to our stack and marks them as current (✓) in the version index.
