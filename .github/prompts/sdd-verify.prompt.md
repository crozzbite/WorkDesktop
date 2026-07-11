# Structured delivery — Verify phase

Use after apply batches, before adversarial review.

## Checks

1. All tasks in scope marked complete in `tasks.md`
2. Tests pass (full suite for touched modules)
3. Spec scenarios covered by tests
4. Strict TDD evidence table complete (if active)
5. No secrets or debug code left behind
6. Design alignment — implementation matches approved design

## Output

- Verify report: pass/fail per check
- Gaps list (if any)
- Recommendation: proceed to adversarial review or return to apply

Role: **Implementer**.

Reference: `docs/governance/refined-rules/09-agent-loops-refined.md` §3.
