---
version: 1.0
current: true
description: Workflow phases + SDLC alignment + STOPs. Refined Rule 08.
---

# Rule 08 Refined: The Workflow (Ritual)

**Version:** 1.0 ✓ (current)

---

## STOP conditions (do not proceed until resolved)

- **STOP** if there are no OpenSpec artifacts (proposal, specs, design) and the ask is a feature or design change. Trigger OpenSpec first.
- **STOP** if the project uses OpenSpec and we are doing feature or implementation work **without an active OpenSpec change**. Every change lives in `openspec/changes/<name>/` (or `.openspec/`); use `openspec list` and `openspec status --change <name>` to confirm the change and its artifact status. Do not implement "in the void"—create a change with `openspec new change "<name>"` or continue an existing one with `/opsx:continue`.
- **STOP** if we are implementing without aligning to the **change's tasks** (`tasks.md`). OpenSpec works change-by-change and each change has a task workflow (proposal → specs → design → **tasks**). Implementation must trace to tasks in that change; when the project tracks **sprints** (e.g. phased tasks or archived changes like `archive/YYYY-MM-DD-sprint-N-name`), ensure we are working in the current change/sprint so we do not lose the thread. If in doubt, ask which change or sprint we are in before coding.
- **STOP** if the implementation does not match the approved design. Refuse and request alignment.
- **STOP** before merge if tests fail or (for domain) mutation score is below the bar. No “merge now, fix later.”

---

## OpenSpec: changes and tasks (reference)

- **Change:** One unit of work (e.g. `nexus-build-plan`, `master-architecture`). Directory: `openspec/changes/<name>/` with `proposal.md`, `specs/`, `design.md`, **tasks.md**.
- **Tasks:** The implementation checklist for that change (`tasks.md`). Work is done by completing tasks; update checkboxes as we go. Do not add unrelated work—either extend the change (and tasks) or create a new change.
- **Nexus projects:** Para proyectos que siguen Nexus Architecture (Bones → Brain → … → Deployment), usar el checklist reutilizable **WorkSpace/.agents/workflows/nexus-build-from-tasks.md** y los snippets en **WorkSpace/.agents/snippets/nexus/** y **defense/**; reglas de código en **docs/code-rules/nexus-angular.md**.
- **Sprints:** Projects may group work by sprint (e.g. phases inside `tasks.md`, or archived changes `archive/YYYY-MM-DD-sprint-N-*`). Always know which change and, if applicable, which sprint we are in so the thread of work is clear.

---

## Phase 1: Discovery (The Mind)

1. **Identify stakeholders** (ISO 42010): Who cares about this?
2. **Capture needs:** Socratic questioning. “Why do we need this?” → “Because X” → “Why X?”
3. **Define quality attributes** (ISO 25010): Is speed more important than accuracy here? Document in specs.

**Deliverable:** Clear problem statement and priorities (e.g. in proposal).

---

## Phase 2: Design (The Bones)

1. **OpenSpec:** Run OpenSpec workflow: proposal → specs (e.g. OpenAPI/JSON Schema) → design.
2. **Pattern:** Choose architecture (Rule 02): Monolith vs Micro vs EDA, etc.
3. **Domain:** Define entities, value objects, aggregates (DDD). Pydantic/interfaces as the bones.
4. **Diagram:** Flow and boundaries (e.g. Mermaid).

**Deliverable:** proposal.md, specs/, design.md, and (if needed) ADR.

---

## Phase 3: Execution (The Muscle)

1. **TDD:** Write the failing test for the use case first.
2. **Domain:** Implement pure logic; no framework imports in domain.
3. **Application:** Wire domain (use cases).
4. **Adapters:** Controllers, DB repos, external APIs.
5. **Gauntlet:** Run tests, mutation (e.g. mutmut / Stryker), type check. Fix until pass.

**Deliverable:** Code that passes the Gauntlet and matches design.

---

## Phase 4: Verification (The Soul)

1. **Audit:** Check against ISO 25010 (quality attributes).
2. **Security:** Run checks for OWASP (by context: Web/API/LLM/Agentic).
3. **Review:** Walkthrough artifact with stakeholder if needed.
4. **Merge:** Archive OpenSpec change; merge only when all above are satisfied.

---

## SDLC alignment (reference)

- **Planning/Discovery** → Phase 1.
- **Analysis/Feasibility** → Part of Phase 1 (in proposal).
- **Design** → Phase 2.
- **Development** → Phase 3.
- **Testing** → Phase 3 (Gauntlet) + Phase 4.
- **Deployment** → After Phase 4 (CI/CD from Rule 03).
- **Maintenance** → ADRs, fitness functions, tech radar (Rule 10).
