---
version: 1.0
current: true
description: FastAPI DO / NEVER + snippets. OpenAPI first, Pydantic.
---

# Code Rules: FastAPI

**Version:** 1.0 ✓ (current)

---

## NEVER

- **NEVER** expose an endpoint without a **Pydantic model** (or equivalent) for request/response and docs.
- **NEVER** use raw request body bytes or unvalidated `dict` for business logic; always validate with a schema.
- **NEVER** put business logic in route handlers; use services or use cases and keep routes thin.
- **NEVER** return arbitrary dicts; return typed Pydantic models or well-defined response models.
- **NEVER** catch broad `Exception` in route handlers and swallow; log and re-raise or return a proper HTTP exception.
- **NEVER** skip OpenAPI tags and summaries for public or team-consumed APIs.

---

## DO

- Define **OpenAPI** (via Pydantic models and FastAPI’s generation) **first**; implement routes to match.
- Use **Pydantic v2** for request/response and validation; use `model_config` and `Field()` for examples and constraints.
- Use **dependency injection** for DB, auth, and services; keep routes testable.
- Use **HTTPException** with correct status codes (400, 401, 403, 404, 422, 429, 500).
- Use **router tags** and **summary/description** for each endpoint so OpenAPI is readable.
- Use **async** for I/O-bound handlers; sync for CPU-bound only when necessary.
- Version the API in the path (e.g. `/api/v1/...`) and never break v1 in place.

---

## Snippets (good practices)

**Typed request/response and status codes:**

```python
from fastapi import APIRouter, Depends, HTTPException

router = APIRouter(prefix="/api/v1/users", tags=["users"])

class UserCreate(BaseModel):
    name: str
    email: EmailStr

class UserResponse(BaseModel):
    id: str
    name: str
    email: str

@router.post("/", response_model=UserResponse, status_code=201)
async def create_user(
    body: UserCreate,
    svc: UserService = Depends(get_user_service),
) -> UserResponse:
    user = await svc.create(body)
    return UserResponse(id=user.id, name=user.name, email=user.email)
```

**Dependency for service:**

```python
def get_user_service(db: Session = Depends(get_db)) -> UserService:
    return UserService(repo=UserRepository(db))
```

**Explicit HTTPException:**

```python
user = await svc.get_by_id(user_id)
if not user:
    raise HTTPException(status_code=404, detail="User not found")
return user
```

**Validation via Pydantic (no raw dict):**

```python
# BAD: body: dict
# GOOD: body: UserCreate
@router.post("/")
async def create(body: UserCreate) -> UserResponse:
    ...
```
