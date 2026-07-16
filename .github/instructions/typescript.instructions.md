---
applyTo: "**/*.{ts,tsx}"
description: TypeScript coding standards from governance canon.
---

# TypeScript instructions

Source: `docs/governance/code-rules/typescript.md`

## NEVER

- Use `any` — prefer `unknown` and narrow, or proper types.
- Disable strict mode or `strictNullChecks` to silence errors.
- Use non-null assertion (`!`) without guard or justification comment.
- Leave unused variables, parameters, or imports.
- Encode types in names (`userList`, `nameString`).

## DO

- TypeScript **strict mode** (`strict: true`, `strictNullChecks: true`).
- Interfaces for object shapes; `type` for unions/intersections.
- `const` by default; `let` only when reassignment needed.
- camelCase for variables/functions; PascalCase for types/classes.
- Boolean names as questions: `isActive`, `hasPermission`.
- Early return and guard clauses.
- Positive condition first when both branches have logic.
- Keep functions small; extract helpers with clear names.
