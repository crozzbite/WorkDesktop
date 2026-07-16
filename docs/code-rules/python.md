---
version: 1.0
current: true
description: Python DO / NEVER + snippets. SkullRender code rules (uv, mypy, ruff).
---

# Code Rules: Python

**Version:** 1.0 ✓ (current)

---

## NEVER

- **NEVER** use `except Exception:` (Pokemon). Catch specific exceptions (`ValueError`, `KeyError`, etc.) or re-raise after logging.
- **NEVER** use `print()` for production logging; use `logging` with levels and structured fields.
- **NEVER** leave mutable default arguments (`def f(x=[])`). Use `None` and assign inside the function.
- **NEVER** skip type hints for public functions and module boundaries; mypy strict must pass.
- **NEVER** use `# type: ignore` without a one-line comment explaining why and a ticket if possible.
- **NEVER** create God modules/classes (&gt; ~500 LOC); split by responsibility.

---

## DO

- Use **uv** for dependencies and **pyproject.toml** as single source of truth.
- Run **mypy** in strict mode; fix or annotate so it passes.
- Use **ruff** for lint and format; align line length and rules with project.
- Prefer **pathlib** over `os.path` for paths.
- Use **Pydantic** (or similar) for validation and serialization at boundaries.
- Prefer **list[str]**, **dict[str, int]** (Python 3.9+ style) over `List`, `Dict` from typing when possible.
- Use **async/await** for I/O-bound code; keep sync code sync unless there’s a clear benefit to async.
- Log with **structured fields** (e.g. `logger.info("message", extra={"user_id": uid})`) for correlation.

---

## Snippets (good practices)

**Specific exceptions, no Pokemon:**

```python
# BAD
try:
    result = risky_call()
except Exception:
    return None

# GOOD
try:
    result = risky_call()
except (ValueError, KeyError) as e:
    logger.warning("Invalid input: %s", e)
    raise
except TimeoutError:
    logger.error("Timeout in risky_call")
    return None
```

**Mutable default:**

```python
# BAD
def add_item(item, items=[]):
    items.append(item)
    return items

# GOOD
def add_item(item: str, items: list[str] | None = None) -> list[str]:
    if items is None:
        items = []
    items.append(item)
    return items
```

**Typed public function:**

```python
def get_user(user_id: str) -> User | None:
    """Return user by id or None if not found."""
    return repo.find_by_id(user_id)
```

**Structured logging:**

```python
logger.info(
    "Request completed",
    extra={"user_id": user_id, "duration_ms": duration, "correlation_id": cid},
)
```
