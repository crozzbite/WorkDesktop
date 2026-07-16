# CI/CD Standard (placeholder)

> Replace with your company's pipeline and deploy gates.

## Pipeline stages

1. Lint + typecheck
2. Unit tests
3. Integration tests (if applicable)
4. Security scan (dependencies, secrets, SAST)
5. Build artifact
6. Deploy to staging → production (human approval)

## Deploy gates

- Tests green
- Security scan clean or accepted risk documented
- Adversarial review `APPROVED` for behavior-changing releases (per Rule 09)

## Secrets

- CI: platform secrets store only
- Runtime: vault / parameter store / K8s secrets

## Container registry

- Document your registry and tagging policy here.
