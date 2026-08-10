# Agent Skills (VS Code Copilot)

Neutral skills for structured delivery (`governance/vscode-copilot-ready` / `governance/copilot-portable`).

## Core (portable — no extra CLI)

| Skill | Trigger |
|-------|---------|
| `strict-tdd` | Behavior-first micro-loop during apply |
| `adversarial-review` | Quality gate before archive (portable substitute for local-only review tools such as GGA) |
| `security-review` | OWASP-focused review when security is in scope |

Canon: `docs/governance/refined-rules/09-agent-loops-refined.md`, `09-strict-tdd-refined.md`, `07-security-refined.md`.

Pair with prompts: `.github/prompts/sdd-*.prompt.md`, `adversarial-review.prompt.md`.

## Optional (requires OpenSpec CLI)

| Skill | Trigger |
|-------|---------|
| `openspec-explore` | Explore ideas before committing to a change |
| `openspec-propose` | Create change + all artifacts in one step |
| `openspec-apply-change` | Implement tasks from `tasks.md` |
| `openspec-verify-change` | Verify implementation vs specs/tasks |
| `openspec-archive-change` | Archive completed change |
| `openspec-sync-specs` | Sync spec artifacts |

Requires: `openspec` CLI (`npm install -g @fission-ai/openspec`). Without it, use **core** skills + `sdd-*` prompts only.

## Out of scope

- Personality packs / branded experts
- GGA as a skill dependency
- Cursor-only workflows

## Fork note

Company fork `v-jonathanz/WorkDesktop` may have an earlier subset. Sync from `crozzbite/WorkDesktop` via cherry-pick — never merge portable ↔ master.
