---
version: 1.0
current: true
description: Angular 19+ DO / NEVER + snippets. Standalone, Signals, OnPush.
---

# Code Rules: Angular

**Version:** 1.0 ✓ (current)

---

## NEVER

- **NEVER** use NgModules for new code. Use **Standalone Components** only.
- **NEVER** use `any`; type templates and inputs/outputs properly.
- **NEVER** use `!important` in component styles; fix specificity or use Tailwind/cascade.
- **NEVER** subscribe manually to Observables in components when you can use **Signals**, **async** pipe, or `effect()`.
- **NEVER** put business logic in components; use services or facade.
- **NEVER** use inline templates or inline styles for production components. Each component MUST have separate **`.html`**, **`.ts`**, **`.spec.ts`**, and (when needed) **`.css`** (or SCSS) files. No single-file components with `template:`/`style:` in the decorator for real features.
- **NEVER** enable `inlineTemplate` or `inlineStyle` in angular.json for production-style builds.
- **NEVER** ship a component without a **`.spec.ts`** file; every component and service has a corresponding spec.
- **NEVER** use `setTimeout`/`setInterval` without cleaning up in `ngOnDestroy` or using RxJS (e.g. `takeUntilDestroyed`).

---

## DO

- **One component = four files (minimum):** **`.html`** (template), **`.ts`** (class), **`.spec.ts`** (tests), and **`.css`** or `.scss` (styles when not using Tailwind-only). Keep template, logic, tests, and styles in separate files for maintainability and consistency.
- Use **Standalone Components** and import what you need in the component.
- Use **Signals** for component state: `signal()`, `computed()`, `effect()`.
- Use **OnPush** change detection for all components.
- Use the **new control flow**: `@if`, `@for`, `@switch` (Angular 17+).
- Use **`@defer`** for deferred loading: wrap heavy or below-the-fold content (modals, tabs, side panels, heavy lists) in `@defer` so it loads lazily and improves initial load and LCP. Use `@defer (on viewport)` or `@defer (on interaction)` when appropriate; pair with `@placeholder` and optional `@loading` for better UX.
- Use **functional guards** (`CanActivateFn`) and **functional interceptors**.
- Use **inject()** for DI in components and services.
- Keep components **dumb** when possible: inputs + outputs; logic in services.
- Write a **`.spec.ts`** for every component and service; aim for meaningful coverage.

---

## SSR: when to use and when not

- **Use SSR** when:
  - You need **SEO** (public pages, marketing, blog, product listings) or **social previews** (Open Graph, Twitter cards).
  - You care about **fast first contentful paint** for anonymous or first-time users (landing, docs, content-heavy sites).
  - The app has **public, crawlable routes** that must be indexed or shared with meaningful HTML on first response.
- **Do not use SSR** (or keep it optional/hybrid) when:
  - The app is **behind login** (dashboards, tools, admin) and there is no relevant public content to index.
  - The experience is **highly interactive** and the main win is after hydration; CSR + good caching may be enough.
  - You want to **minimize server cost and complexity** and SEO/first-paint are not requirements.
- **Hybrid:** Use SSR for public routes (home, landing, docs) and CSR or prerender-only for authenticated or tool-heavy sections. In Angular: enable SSR with `@angular/ssr` and use `provideServerRendering()` only where it adds value; avoid SSR for routes that require auth or heavy client state.

---

## Snippets (good practices)

**Component file layout (required):** One folder per component, e.g. `user-card/` with `user-card.component.ts`, `user-card.component.html`, `user-card.component.spec.ts`, `user-card.component.css` (or `.scss`). Reference them in the decorator:

```typescript
@Component({
  selector: 'app-user-card',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  templateUrl: './user-card.component.html',
  styleUrls: ['./user-card.component.css'],
})
export class UserCardComponent { ... }
```

**Standalone component with Signals and OnPush (logic in .ts; template in .html):**

```typescript
import { Component, input } from '@angular/core';

@Component({
  selector: 'app-user-card',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  templateUrl: './user-card.component.html',
  styleUrls: ['./user-card.component.css'],
})
export class UserCardComponent {
  user = input.required<{ name: string }>();
}
```

**Signal-based state:**

```typescript
readonly count = signal(0);
readonly doubled = computed(() => this.count() * 2);

increment(): void {
  this.count.update((c) => c + 1);
}
```

**Functional guard:**

```typescript
export const authGuard: CanActivateFn = (route, state) => {
  const auth = inject(AuthService);
  if (auth.isAuthenticated()) return true;
  return inject(Router).createUrlTree(['/login']);
};
```

**Inject instead of constructor DI:**

```typescript
private readonly http = inject(HttpClient);
private readonly router = inject(Router);
```
