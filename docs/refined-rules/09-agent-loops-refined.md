---
version: 1.1
current: true
description: Agent loop engineering — Strict TDD, Judgment Day triggers, persistence, human-in-the-loop. Refined Rule 09.
---

# Rule 09 Refined: Agent Loop Engineering

**Version:** 1.1 ✓ (current) · **Created:** 2026-06-25 (ISO 8601)

> **Skills (already installed — do NOT reimplement):** `~/.cursor/skills/sdd-apply/strict-tdd.md`, `~/.cursor/skills/sdd-verify/strict-tdd-verify.md`, `~/.cursor/skills/judgment-day/SKILL.md`
> **Engram decision:** `workdesktop/agent-loops-jd-triggers`
> **Pilot reference:** DnDApp Phase 3.5 (`dndapp/phase-3.5-health-readiness`, engram #57)

---

## 1. Principle (why loops exist)

AI is **probabilistic, not deterministic**. Uncontrolled agent flows accumulate small errors into hallucinations and silent failures.

**SkullRender response:** design **short, controlled loops**; keep **human responsibility** for merge, deploy, and `gga`. The agent prepares; the human crosses the gate.

Three complementary loops:

| Loop | Question it answers | Owner |
|------|---------------------|-------|
| **Strict TDD** (apply) | *Does the code match the intended behavior?* | 🥸 Gentleman |
| **sdd-verify** | *Was TDD followed? Do tests and spec align?* | 🥸 Gentleman |
| **Judgment Day** | *Does the contract **lie**? (silent failures, cosmetic tests, drift)* | 🥸 Gentleman (general); 🚨 Cerbero overlays security criteria when triggered |

Cerbero **does not own** Judgment Day. Cerbero **adds** OWASP/STRIDE/supply-chain criteria when security triggers fire.

---

## 2. SDD dependency graph (extended)

```
proposal → spec → design → tasks
                              ↓
                    apply [Strict TDD micro-loop per task]
                              ↓
                         sdd-verify
                              ↓
                    judgment-day  ← gate (1× per change)
                              ↓
                         sdd-archive
                              ↓
              human: commit → PR → merge → deploy → gga
```

**Optional entry audit (Trigger A):** before apply on deploy/infra/CI scope — see §4.

**Chat invocation:** user may say `judgment day`,`JD`, `judgment-day`, `juzgar`, etc. at any time (skill trigger).

---

## 3. Strict TDD (apply phase)

**Full protocol (SkullRender canon):** `docs/refined-rules/09-strict-tdd-refined.md`

When `strict_tdd: true` and a test runner exists, each apply task follows:

**Safety net → Understand → RED → GREEN → Triangulate → Refactor**

Summary:

| Step | Name | Gate |
|------|------|------|
| 0 | **Safety net** — existing tests on modified files; stop on pre-existing failures | Do not touch code blindly |
| 1 | **Understand** — task, spec scenarios, design, patterns | Before any new test/code |
| 2 | **RED** — failing test first; no production code before test | Test written |
| 3 | **GREEN** — minimum code; run **only** the relevant test file | Test passes (executed) |
| 4 | **Triangulate** — happy path + edge cases; force real logic | Spec scenarios covered |
| 5 | **Refactor** — improve with tests still green | After each refactor step |

Operational rules (assertion quality, runners, evidence table): `~/.cursor/skills/sdd-apply/strict-tdd.md`. Verification gate: `strict-tdd-verify.md`.

**STOP:** Do not silently fall back to Standard Mode when Strict TDD is active.

---

## 4. Judgment Day — when to run

### 4.1 Trigger table

| ID | Moment | Mandatory? | Rule |
|----|--------|------------|------|
| **A** | Pre-apply / phase entry | **Always** for deploy, infra, CI paths | Gentleman **suggests** JD when risk is high; user invokes anytime something "smells wrong" |
| **B** | Post-apply (TDD batch) | Yes if Strict TDD active | Adversarial check on new/changed tests — not tautologies or ghost loops |
| **C** | Post-`sdd-verify` | **Yes — gate before archive** | **One JD per complete change** (not per apply batch) |
| **D** | Pre-PR | Conditional | Skip if `sdd/{change}/judgment-day-report` exists with `JUDGMENT: APPROVED` and PR scope ⊆ JD scope |
| **E** | Pre-merge / pre-deploy | Conditional | Cerbero may require re-JD on sensitive surface |
| **F** | Before deleting old tests | **Yes — 1 round** | Does the new contract cover what the old test **pretended** to cover? |

### 4.2 Additional valid uses (recommended)

- **Post-design (infra):** one round on probes, health/readiness, smoke contracts — can this probe lie?
- **Sabotage drill:** take down Redis (or dependency); confirm `/ready` (or equivalent) fails loudly.
- **Post-mutation / Gauntlet:** mutation passes but assertions are trivial → JD focused on assertion quality.
- **Deploy docs drift:** COMMAND-REFERENCE / checklists out of sync with manifests or code.

### 4.3 Path whitelist (Trigger A — auto-suggest)

Gentleman MUST suggest JD before apply when the change touches any of:

- `deploy/**`, `**/Dockerfile*`, `docker-compose*`, `k8s/**`, `**/helm/**`
- `.github/workflows/**`, `.gitlab-ci*`, `**/ci.yml`
- `**/health/**`, readiness/liveness probes, smoke scripts
- `docs/deployment/**` when it defines runtime contracts

User may override or invoke JD manually on any scope.

### 4.4 What NOT to JD

- Typo-only, comment-only, mechanical rename with no behavior contract.
- Tasks with no testable or reviewable behavior change.

---

## 5. Judgment Day — protocol (reference)

Follow skill `judgment-day` exactly:

- Two **blind parallel judges**; orchestrator synthesizes (Confirmed / Suspect / Contradiction).
- **Round 1:** present verdict; ask before fixing confirmed issues.
- **Theoretical warnings** → INFO only; do not block.
- Re-judge after fixes until `JUDGMENT: APPROVED` or user chooses `ESCALATED`.
- Orchestrator does **not** review code itself.

### Cerbero overlay (security mode)

When Cerbero triggers (auth, secrets, API, deps, deploy, LLM/agents):

- Same JD protocol; inject **security review criteria** (OWASP Web/API, LLM Top 10, supply chain).
- Prefer `security-review` subagent for pure security-only audits without general quality scope.

---

## 6. Persistence and commit flow

### 6.1 Engram topic keys

| Artifact | Topic key |
|----------|-----------|
| JD report (verdict) | `sdd/{change}/judgment-day-report` |
| Apply progress (TDD evidence) | `sdd/{change}/apply-progress` |
| Verify report | `sdd/{change}/verify-report` |

Save JD report via `mem_save` with `topic_key: sdd/{change}/judgment-day-report` after terminal state (`APPROVED` or `ESCALATED`).

### 6.2 No commit SHA in JD scope

Do **not** tie JD validity to commit SHA. Re-running JD after every commit with no meaningful change is waste.

**Valid flow:**

```
JD → commit → more work (no automatic re-JD)
                    ↓
     user: "voy a commitear estos cambios"
                    ↓
     Gentleman: "¿Querés Judgment Day antes de continuar?"  ← reminder only, not a block
                    ↓
              user decides
```

Gentleman **reminds**; user **decides**. No forced re-JD on empty commits.

### 6.3 Pre-PR idempotency

```
IF mem_search finds sdd/{change}/judgment-day-report
   AND verdict == JUDGMENT: APPROVED
   AND PR diff scope ⊆ JD scope
THEN skip pre-PR JD
ELSE run JD (or user invokes manually)
```

---

## 7. STOP conditions (additions to Rule 08)

- **STOP** before `/sdd-archive` if post-verify JD for the **complete change** has not reached `JUDGMENT: APPROVED` (unless user explicitly escalates and accepts risk).
- **STOP** before deleting tests without Trigger **F** (one-round JD on contract coverage).
- **STOP** declaring merge-ready on deploy/infra/CI changes if Trigger **A** was skipped **and** Gentleman flagged high silent-failure risk — offer JD first.

Human retains merge, deploy, push, and `gga`. Agents prepare artifacts; humans cross gates.

---

## 8. Persona responsibilities

| Persona | Agent loops duty |
|---------|------------------|
| 💀 Lich | Canon, tradeoffs, when a loop is over-engineering; resolves conflicts |
| 🥸 Gentleman | Runs Strict TDD enforcement, verify, **Judgment Day (general)**, suggests Trigger A, **commit reminder** (§6.2), pre-PR check |
| 🚨 Cerbero | Olfateo (secrets, audit, gga); **security overlay** on JD; veto on merge/deploy; `security-review` when security-only |

---

## 9. Reference pilot (DnDApp Phase 3.5)

Proven pattern (not stack-specific):

1. JD Round 1 — audit deploy/health/tests for silent failures.
2. Strict TDD — implement confirmed fixes (`/ready`, shared Redis config, contract specs).
3. JD Round 2–3 — re-judge until `APPROVED` for the silent-failure class.

Lesson: `/health` and smoke can pass while Redis is broken. JD finds the lie; TDD writes an honest contract.

---

*Rule 09 wires existing Gentle AI skills into SkullRender governance. Implementation lives in skills and subagents; this rule defines **when** and **who**, not **how** to reimplement.*
