# Governance repo — Copilot / VS Code branch

This branch (`governance/copilot-portable`) is **ready to clone** on another machine. No SkullRender branding, no personal profiles, no Cursor-specific rules.

## Clone

```bash
git clone -b governance/copilot-portable <REPO_URL> governance
cd governance
```

Or from an existing clone:

```bash
git fetch origin
git checkout governance/copilot-portable
```

## VS Code setup (native Copilot, no extra extensions)

1. Open the folder in VS Code.
2. Ensure GitHub Copilot is signed in.
3. Copy `.vscode/settings.json.example` → `.vscode/settings.json` (or merge keys).
4. Add your company policies under `docs/company/` and link them from `.github/copilot-instructions.md`.

## What this branch contains

| Path | Purpose |
|------|---------|
| `AGENTS.md` | Tri-role protocol (Architect / Implementer / Security Guardian) |
| `.github/copilot-instructions.md` | Global Copilot instructions |
| `.github/instructions/` | Stack-specific rules (`applyTo` frontmatter) |
| `.github/prompts/` | Structured delivery flows (explore, apply, verify, adversarial review) |
| `docs/governance/` | Canon rules, constraints, version index |
| `docs/company/` | Placeholders for your enterprise policies |

## What this branch does NOT contain

- `.cursor/rules/` (Cursor-only; not used here)
- Personal programmer profiles
- Project-specific MCP servers (see `governance-portable/templates/mcp.json.example`)

## Customize on the other machine

1. Edit `docs/company/*.md` with real policies.
2. Update stack section in `.github/copilot-instructions.md` (package manager, frameworks).
3. Optional: add `.github/skills/` for reusable workflows.
4. Optional: configure MCP when tools are defined.

## Sync from `master`

When governance evolves on `master`, cherry-pick or merge selective commits into this branch and re-neutralize identity-specific content.
