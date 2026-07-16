---
version: 1.0
current: true
description: TypeScript DO / NEVER + snippets. SkullRender code rules.
---

# Code Rules: TypeScript

**Version:** 1.0 ✓ (current)

---

## NEVER

- **NEVER** use `any`. Use `unknown` and narrow, or define a proper type/interface.
- **NEVER** disable strict mode or `strictNullChecks` to “fix” a type error. Fix the types.
- **NEVER** use non-null assertion (`!`) without a preceding guard or a short comment justifying it.
- **NEVER** leave unused variables, parameters, or imports; remove them.
- **NEVER** encode types in names (`userList`, `nameString`); the type system already carries that.

---

## DO

- Run **TypeScript in strict mode** (`strict: true` and `strictNullChecks: true` in `tsconfig.json`); fix or annotate so the build passes. Never disable strict to silence errors—fix the types instead. (Same idea as mypy strict in Python.)
- Prefer **interfaces** for object shapes; **type** for unions/intersections/mapped types.
- Use **const** by default; **let** only when reassignment is needed.
- Names: variables and functions in **camelCase**; types/interfaces/classes **PascalCase**.
- Booleans as questions: `isActive`, `hasPermission`, `canEdit`.
- Functions as verbs: `fetchUser`, `validateInput`, `parseConfig`.
- Prefer **early return** and guard clauses to reduce nesting.
- **Conditionals:** Prefer positive condition first when both branches have logic → see **non-negotiables** (single source of truth).
- Keep functions small (e.g. &lt; 30 lines); extract helpers with clear names.

---

## Snippets (good practices)

**Typed function with explicit return:**

```typescript
function getFullName(user: { firstName: string; lastName: string }): string {
  return `${user.firstName} ${user.lastName}`;
}
```

**Avoid any – use unknown and narrow:**

```typescript
// BAD
function parse(data: any) {
  return data.value;
}

// GOOD
function parse(data: unknown): { value: string } {
  if (typeof data !== 'object' || data === null || !('value' in data)) {
    throw new Error('Invalid shape');
  }
  const { value } = data as { value: unknown };
  if (typeof value !== 'string') throw new Error('value must be string');
  return { value };
}
```

**Guard clause instead of deep nesting:**

```typescript
// GOOD
function findUser(id: string): User | null {
  if (!id) return null;
  const user = db.users.get(id);
  if (!user) return null;
  return user;
}
```

**Constants for magic numbers:**

```typescript
const MAX_RETRIES = 3;
if (retries > MAX_RETRIES) throw new Error('Max retries exceeded');
```

Conditionals: prefer positive condition first when both branches have logic → see **non-negotiables** (single source of truth).
