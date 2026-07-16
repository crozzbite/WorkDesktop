---
version: 1.0
current: true
description: Workflow phases + SDLC alignment + STOPs. Rule 08 (neutral).
---

# Rule 08: Workflow

**Version:** 1.0 ✓ (current)

---

## STOP conditions (do not proceed until resolved)

- **STOP** if there are no approved spec artifacts (proposal, specs, design) and the ask is a feature or design change.
- **STOP** if implementation work runs **without an active change folder**. Convention: `changes/<name>/` or `openspec/changes/<name>/` with `proposal.md`, `specs/`, `design.md`, `tasks.md`.
- **STOP** if implementing without aligning to **tasks.md** for the active change.
- **STOP** if implementation does not match approved design.
- **STOP** before merge if tests fail or (for domain) mutation score is below the bar.
- **STOP** before archive if **adversarial review** for the complete change has not reached `APPROVED` (see Rule 09). User may accept `ESCALATED` explicitly.
- **STOP** before deleting tests without a one-round adversarial review on contract coverage (Rule 09, Trigger F).

---

## Changes and tasks (reference)

- **Change:** One unit of work. Directory: `changes/<name>/` (or OpenSpec equivalent).
- **Tasks:** Implementation checklist in `tasks.md`. Complete tasks in order; update checkboxes.
- **Sprints:** Projects may phase work inside `tasks.md` or archive completed changes by date.

---

## Phase 1: Discovery

1. Identify stakeholders (ISO 42010).
2. Capture needs with Socratic questioning.
3. Define quality attributes (ISO 25010).

**Deliverable:** Problem statement and priorities in proposal.

---

## Phase 2: Design

1. Spec workflow: proposal → specs (OpenAPI/JSON Schema) → design.
2. Choose architecture pattern (Rule 02).
3. Define domain model (DDD).
4. Diagram flows and boundaries (Mermaid).

**Deliverable:** proposal, specs, design, ADR if needed.

---

## Phase 3: Execution

When **Strict TDD** is active, each task follows `09-strict-tdd-refined.md`:

**Safety net → Understand → RED → GREEN → Triangulate → Refactor**

1. Domain logic without framework imports in domain layer.
2. Application / use cases.
3. Adapters (controllers, repos, external APIs).
4. Gauntlet: tests, mutation, type check.

**Deliverable:** Code passing Gauntlet and matching design.

---

## Phase 4: Verification

1. **Verify:** Spec alignment, Gauntlet, Strict TDD compliance.
2. **Adversarial review:** One round per complete change before archive (Rule 09).
3. **Security:** OWASP checks by context; Security Guardian overlay when triggered.
4. **Archive & merge:** Human merges when gates are satisfied.

---

## SDLC alignment

| SDLC phase | Workflow phase |
|------------|----------------|
| Planning / Discovery | Phase 1 |
| Design | Phase 2 |
| Development | Phase 3 |
| Testing | Phase 3 + 4 |
| Deployment | After Phase 4 |
| Maintenance | ADRs, fitness functions |
