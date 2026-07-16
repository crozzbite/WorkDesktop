---
name: adversarial-review
description: "Trigger: adversarial review, quality gate, juzgar. Run blind dual review for silent failures, weak tests, and contract drift before archive."
---

## Activation

Load when user requests adversarial review, quality gate, or equivalent (`juzgar`, `review gate`).

One round **per complete change** before archive (Rule 09 Trigger C).

## Hard rules

- Launch **two blind reviewers in parallel** with identical scope; synthesize — do not self-review code.
- Classify: **Confirmed** / **Suspect** / **Contradiction** / **INFO (theoretical)**.
- Ask before fixing Round 1 confirmed issues.
- Re-review in parallel after fixes until `APPROVED` or user accepts `ESCALATED`.
- Terminal states only: `APPROVED` or `ESCALATED`.

## Focus

- Silent failures (health passes, dependency broken)
- Cosmetic/tautology tests
- Contract drift (docs vs code vs probes)
- Spec scenarios not covered by tests

## Security overlay

When auth, secrets, API, dependencies, deploy, or agents are in scope → add criteria from `docs/governance/refined-rules/persona-security-guardian.md` and Rule 07.

## Output

```markdown
## Adversarial Review — {target}
VERDICT: APPROVED | ESCALATED
Confirmed: ...
Suspect: ...
Actions: ...
```

Save optional report: `changes/<name>/adversarial-review-report.md`

Reference: `.github/prompts/adversarial-review.prompt.md`, Rule 09.
