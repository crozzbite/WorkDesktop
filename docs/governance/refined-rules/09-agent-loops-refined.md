---
version: 1.0
current: true
description: Agent loop engineering — Strict TDD, adversarial review, human-in-the-loop. Rule 09 (neutral).
---

# Rule 09: Agent Loop Engineering

**Version:** 1.0 ✓ (current)

---

## 1. Principle

AI is **probabilistic, not deterministic**. Uncontrolled flows accumulate errors into silent failures.

**Response:** design **short, controlled loops**; keep **human responsibility** for commit, PR, merge, and deploy. The agent prepares; the human crosses the gate.

| Loop | Question it answers | Owner |
|------|---------------------|-------|
| **Strict TDD** (apply) | Does the code match intended behavior? | Implementer |
| **Verify** | Was TDD followed? Do tests and spec align? | Implementer |
| **Adversarial review** | Does the contract **lie**? (silent failures, cosmetic tests, drift) | Implementer; Security Guardian adds OWASP when triggered |

Security Guardian **does not own** adversarial review. Security **adds** OWASP/STRIDE/supply-chain criteria when security triggers fire.

---

## 2. Structured delivery graph

```
proposal → spec → design → tasks
                              ↓
                    apply [Strict TDD micro-loop per task]
                              ↓
                            verify
                              ↓
                    adversarial-review  ← gate (1× per change)
                              ↓
                            archive
                              ↓
              human: commit → PR → merge → deploy
```

Invoke phases via `.github/prompts/` in Copilot Chat.

---

## 3. Strict TDD (apply phase)

**Full protocol:** `docs/governance/refined-rules/09-strict-tdd-refined.md`

When `strict_tdd: true` and a test runner exists:

**Safety net → Understand → RED → GREEN → Triangulate → Refactor**

| Step | Gate |
|------|------|
| 0 Safety net | Stop on pre-existing test failures |
| 1 Understand | Task, spec, design before code |
| 2 RED | Failing test first |
| 3 GREEN | Minimum code; run relevant test file |
| 4 Triangulate | Happy path + edge cases |
| 5 Refactor | Tests stay green |

**STOP:** Do not silently fall back to standard mode when Strict TDD is active.

---

## 4. Adversarial review — when to run

| ID | Moment | Mandatory? |
|----|--------|------------|
| **A** | Pre-apply / phase entry | Suggest for deploy, infra, CI paths |
| **B** | Post-apply (TDD batch) | Yes if Strict TDD active |
| **C** | Post-verify | **Yes — gate before archive** (1× per change) |
| **D** | Pre-PR | Skip if prior `APPROVED` report covers PR scope |
| **E** | Pre-merge / pre-deploy | Security may require re-review on sensitive surface |
| **F** | Before deleting tests | **Yes — 1 round** |

### Path whitelist (Trigger A — auto-suggest)

Suggest adversarial review before apply when touching:

- `deploy/**`, `Dockerfile*`, `docker-compose*`, `k8s/**`, `helm/**`
- `.github/workflows/**`, CI configs
- Health/readiness probes, smoke scripts
- Deployment docs defining runtime contracts

### What NOT to review adversarially

- Typo-only, comment-only, mechanical rename with no behavior contract.

---

## 5. Adversarial review — protocol

Use `.github/prompts/adversarial-review.prompt.md`:

- Two blind parallel perspectives; synthesize Confirmed / Suspect / Contradiction.
- Round 1: present verdict; ask before fixing confirmed issues.
- Theoretical warnings → INFO only.
- Re-review after fixes until `APPROVED` or user chooses `ESCALATED`.

### Security Guardian overlay

When security triggers fire (auth, secrets, API, deps, deploy, LLM/agents):

- Same protocol with **security criteria** (OWASP Web/API, LLM Top 10, supply chain).

---

## 6. Persistence (optional)

If using persistent memory, store:

| Artifact | Suggested key |
|----------|---------------|
| Review report | `delivery/{change}/adversarial-review-report` |
| Apply progress | `delivery/{change}/apply-progress` |
| Verify report | `delivery/{change}/verify-report` |

Do **not** tie review validity to commit SHA.

### Commit reminder

When user announces an upcoming commit with uncommitted changes, **remind** about adversarial review; user decides.

---

## 7. STOP conditions (additions to Rule 08)

- **STOP** before archive if post-verify adversarial review has not reached `APPROVED` (unless user escalates).
- **STOP** before deleting tests without Trigger **F**.
- Human retains merge, deploy, and production gates.

---

## 8. Role responsibilities

| Role | Agent loops duty |
|------|------------------|
| **Architect** | Canon, tradeoffs, when a loop is over-engineering |
| **Implementer** | Strict TDD, verify, adversarial review, commit reminder |
| **Security Guardian** | Early sniff (secrets, audit); security overlay on review; deploy veto |
