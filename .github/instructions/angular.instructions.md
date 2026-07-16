---
applyTo: "**/*.{component.ts,service.ts,directive.ts,pipe.ts,routes.ts}"
description: Angular 19+ standards — Standalone, Signals, OnPush.
---

# Angular instructions

Source: `docs/governance/code-rules/angular.md`

## NEVER

- NgModules for new code — Standalone Components only.
- `any` in templates or component code.
- Business logic in components — use services/facades.
- Inline templates/styles for production components.
- Constructor DI — use `inject()` in class fields.
- Manual Observable subscriptions when Signals/async pipe/`effect()` suffice.
- Ship components without `.spec.ts`.

## DO

- One component = `.html`, `.ts`, `.spec.ts`, styles file (when needed).
- Signals: `signal()`, `computed()`, `effect()`.
- OnPush change detection.
- Control flow: `@if`, `@for`, `@switch`.
- Separate template, logic, tests, and styles.
