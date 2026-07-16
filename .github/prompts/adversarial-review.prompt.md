# Adversarial review (gate before archive)

One round **per complete change**. Equivalent to quality + security red-team gate.

## When mandatory

- Post-verify, before archive (Rule 09 Trigger C)
- Before deleting tests (Trigger F)
- Suggested for deploy/infra/CI changes (Trigger A)

## Protocol

1. Define review scope (files, behaviors, contracts).
2. Run **two blind perspectives** (quality + security if triggered).
3. Classify findings: **Confirmed** / **Suspect** / **Contradiction**.
4. Present verdict; ask before fixing Confirmed issues.
5. Re-review after fixes until `APPROVED` or user accepts `ESCALATED`.

## Focus areas

- Silent failures (health passes but dependency broken)
- Cosmetic tests (tautologies, no behavior assertion)
- Contract drift (docs vs code vs probes)
- Security (if triggered): OWASP Web/API, LLM Top 10, secrets, supply chain

## Security overlay

If auth, secrets, API, deps, deploy, or agents are in scope → apply `persona-security-guardian.md` criteria.

## Output

```
VERDICT: APPROVED | ESCALATED
Scope: ...
Confirmed: ...
Suspect: ...
Actions taken: ...
```

Save report to `changes/<name>/adversarial-review-report.md` (optional).

Role: **Implementer** (general); **Security Guardian** (overlay when triggered).
