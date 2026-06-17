# Code Rules – Index

**Version:** 1.0 ✓ (current)

Code-level rules: **what to do** and **what never to do** with languages and tools, plus short snippets. These close the gap with “qué hacer en código” so the agent and the team have explicit DO/NEVER and examples.

---

## Version and current set

| File | Version | Current | Scope |
|------|---------|---------|--------|
| `00-index.md` | 1.0 | ✓ | This index |
| `typescript.md` | 1.0 | ✓ | TypeScript (strict, no any, naming) |
| `python.md` | 1.0 | ✓ | Python (uv, mypy, ruff, no Pokemon) |
| `angular.md` | 1.0 | ✓ | Angular 19+ (Standalone, Signals, OnPush) |
| `fastapi.md` | 1.0 | ✓ | FastAPI (Pydantic, OpenAPI first) |
| `tailwind.md` | 1.0 | ✓ | Tailwind (SkullRender aesthetic) |
| `nexus-angular.md` | 1.0 | ✓ | Nexus Architecture + Clean/DDD en Angular (path aliases, tokens, use cases, conectores) |

---

## How to use

- **When writing TS:** Apply `typescript.md` + `angular.md` (if Angular). If the project follows **Nexus Architecture**, also apply `nexus-angular.md`.
- **When writing Python:** Apply `python.md` + `fastapi.md` (if API).
- **When styling:** Apply `tailwind.md`.
- All code rules are **additive** to the refined rules (00–08) and constraints; NEVER/DO here are as binding as in `docs/constraints/non-negotiables.md`.

---

## Cross-cutting (all code)

- **NEVER** `any` in TypeScript without justification and a comment.
- **NEVER** `except Exception:` in Python; catch specific exceptions.
- **NEVER** `!important` in CSS; fix specificity and layers.
- **DO** use strict types, small functions, and clear names (see per-language files).
