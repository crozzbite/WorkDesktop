---
version: 1.0
current: true
description: Tailwind DO / NEVER + snippets. Follow project style system; define in spec if missing.
---

# Code Rules: Tailwind CSS

**Version:** 1.0 ✓ (current)

---

## NEVER

- **NEVER** use `!important` to override Tailwind or fix specificity. Fix the order or use `@layer` and proper cascade.
- **NEVER** inline long class strings in templates without extraction; use `@apply` in a component style block or a shared class for repeated patterns.
- **NEVER** use arbitrary values (`w-[137px]`) when a design token or theme value exists; prefer config and semantic names.
- **NEVER** mix Tailwind with heavy custom CSS that duplicates Tailwind (e.g. custom flex + gap); use Tailwind utilities first.
- **NEVER** leave unused custom CSS or dead Tailwind classes; prune regularly.

---

## DO

- Use **Tailwind** as the primary styling system; **follow the style system defined in each project** (design tokens, palette, typography in `tailwind.config` or project docs). If the project has **no defined style system**, ask so it can be **defined in the corresponding spec** (OpenSpec specs or design) before inventing tokens—do not assume a default palette.
- Prefer **utility-first**; extract to a component or `@apply` only when the same combination repeats 3+ times.
- Use **design tokens** from the project's style system in config (colors, spacing, typography) so the UI is consistent with the spec.
- Use **responsive** and **state** variants (`md:`, `hover:`, `dark:`) instead of custom media queries when possible.
- Keep **minimalismo funcional**: only the classes needed; no decorative clutter.
- Use **CSS layers** if you need to override Tailwind with custom rules: `@layer components { ... }`.

---

## Snippets (good practices)

**Semantic color from the project's style system:**

Use the tokens defined in the project (e.g. in specs or `tailwind.config`). Example if the spec defines `bone`, `ink`, `accident`:

```html
<div class="bg-bone text-ink border border-accident">
  ...
</div>
```

In `tailwind.config`: define the project's tokens in `theme.extend.colors` (and spacing/typography) as per the project style spec.

**Repeated pattern → component class:**

```html
<!-- Repeated 3+ times: extract -->
<button class="btn-primary">Submit</button>
```

```css
@layer components {
  .btn-primary {
    @apply rounded-lg bg-accident px-4 py-2 text-white hover:opacity-90;
  }
}
```

**Responsive without !important:**

```html
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  ...
</div>
```

**No arbitrary one-offs when token exists:**

```html
<!-- BAD: w-[137px] -->
<!-- GOOD if 36 is in theme: w-36 or use theme spacing -->
<div class="w-36">...</div>
```

---

Follow the project's style spec (e.g. minimal, high contrast, defined palette). Prefer clarity and consistency over one-off pixel values.
