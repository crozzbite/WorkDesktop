# Agent Skills (VS Code Copilot)

Neutral skills for structured delivery on branch `governance/copilot-portable`.

## OpenSpec workflow

| Skill | Trigger |
|-------|---------|
| `openspec-explore` | Explore ideas before committing to a change |
| `openspec-propose` | Create change + all artifacts in one step |
| `openspec-apply-change` | Implement tasks from `tasks.md` |
| `openspec-verify-change` | Verify implementation vs specs/tasks |
| `openspec-archive-change` | Archive completed change |
| `openspec-sync-specs` | Sync spec artifacts |

Requires: `openspec` CLI (`npm install -g @fission-ai/openspec`).

## Governance loops

| Skill | Trigger |
|-------|---------|
| `strict-tdd` | Behavior-first micro-loop during apply |
| `adversarial-review` | Quality gate before archive (ex Judgment Day) |
| `security-review` | OWASP-focused review when security is in scope |

Canon references: `docs/governance/refined-rules/09-agent-loops-refined.md`, `09-strict-tdd-refined.md`, `07-security-refined.md`.

## Fork note

Company fork `v-jonathanz/WorkDesktop` may have an earlier subset (5 OpenSpec skills). Upstream adds `openspec-verify-change`, `strict-tdd`, `adversarial-review`, and `security-review`. Cherry-pick from `crozzbite/WorkDesktop` when syncing — never merge branches.
