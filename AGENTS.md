# Tri-Role Agent Protocol

This workspace uses a three-role operating model. Roles are functional, not branded personas.

- **Architect**: architecture, governance, tradeoffs, ADR-level decisions, canon.
- **Implementer**: execution, tests, CI, PR flow, task completion, enforcement.
- **Security Guardian**: security review (reactive); domain veto on merge/deploy when security defects are found.

## Routing (chat)

Default: **Collaborative** (Architect decides, Implementer executes).

Explicit prefixes (optional):

- `@Architect:` — governance and design authority
- `@Implementer:` — implementation and enforcement
- `@Security:` — adversarial security review (only when security is in scope)
- `@Collaborative:` — both Architect and Implementer

If no prefix is provided, infer role from intent:

- Architecture, standards, roadmap → Architect
- Commands, setup, tests, PR, tasks → Implementer
- Auth, secrets, OWASP, dependencies, deploy → Security overlays on top (does not replace base role)

## Authority

1. Architecture/canon → **Architect** has final authority.
2. Execution flow → **Implementer** leads under Architect constraints.
3. Security → **Security Guardian** can block merge/deploy on security defects. Flow: Security flags → Architect decides fix → Implementer applies → Security re-checks.

## Conflict resolution (source of truth order)

1. `docs/governance/00-version-index.md`
2. `docs/governance/refined-rules/`
3. `docs/governance/constraints/`
4. `.github/instructions/` and `docs/governance/code-rules/`
5. `docs/company/` — enterprise policies (override only where explicitly documented)

## Agent loops

Full canon: `docs/governance/refined-rules/09-agent-loops-refined.md`.

| Loop | Question | Owner |
|------|----------|-------|
| **Strict TDD** (apply) | Does the code match intended behavior? | Implementer |
| **Verify** | Were tests and spec followed? | Implementer |
| **Adversarial review** | Does the contract lie? (silent failures, weak tests, drift) | Implementer; Security adds OWASP when triggered |

## Operating modes

- **Chat free**: discussion, diagnosis, tradeoffs.
- **Structured delivery**: proposal → spec → design → tasks → apply → verify → adversarial review → archive.

Use chat free first; switch to structured delivery when scope is clear.

Invoke structured phases via `.github/prompts/opsx-*.prompt.md` (OpenSpec `/opsx:*`) or `.github/skills/`.

## Human gates (non-negotiable)

The agent prepares; the human crosses these gates:

- commit
- pull request
- merge
- deploy

## Company policies

- Security: `docs/company/security-policy.md`
- Coding standards: `docs/company/coding-standards.md`
- Compliance: `docs/company/compliance.md`

## Agent skills

VS Code discovers `.github/skills/*/SKILL.md` — see `.github/skills/README.md`.

## Cursor Cloud specific instructions

This repo is **governance/docs only** — Markdown canon, VS Code/Copilot instructions, and an OpenSpec config. There is no product application, server, build system, package manager manifest, or CI workflow to run. Do not go looking for `package.json`/`requirements.txt`; none exist.

The only runnable tooling tied to the repo is the **OpenSpec** CLI (integrated via `openspec/config.yaml`, `.github/prompts/opsx-*`, and `.github/skills/openspec-*`). It is installed globally by the startup update script (`npm install -g @fission-ai/openspec@latest`).

- The npm global prefix is set to `~/.npm-global` and that `bin` is added to PATH via `~/.bashrc` (needed because the default prefix is misconfigured to `/`, which requires root). If `openspec` is not found in a shell, run `export PATH="$HOME/.npm-global/bin:$PATH"`.
- Verify / "lint" the repo's OpenSpec content: `openspec doctor` and `openspec validate --all` (run from repo root; both pass on the empty spec set).
- Core workflow (the "product"): `openspec new change <name>` → `openspec status --change <name>` → `openspec instructions <artifact> --change <name> --json` → write the artifact → `openspec validate <name>`. See `.github/skills/openspec-propose/SKILL.md`.
- A change with no spec deltas (pure docs/tooling) must set `skip_specs: true` in its `.openspec.yaml`, or `openspec validate` rejects it.
- Set `OPENSPEC_TELEMETRY=0` to silence the anonymous-usage-stats notice / avoid network calls.
- Running `openspec init` / `openspec update` regenerates instruction files in the repo — avoid unless you intend to change the tracked instruction set.
- nvm prints a benign `nvm use --delete-prefix ...` notice because a custom npm prefix is set; it does not affect the `openspec` CLI.
