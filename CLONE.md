# Governance repo — Copilot / VS Code branch

This branch (`governance/copilot-portable`) is **ready to clone** on another machine. No SkullRender branding, no personal profiles, no Cursor-specific rules.

## Clone

```bash
git clone -b governance/copilot-portable https://github.com/crozzbite/WorkDesktop.git governance
cd governance
```

> Repo **PUBLIC** — clone sin cuenta personal. URL: https://github.com/crozzbite/WorkDesktop  
> Herramientas: [`docs/SETUP-TOOLS.md`](docs/SETUP-TOOLS.md) · Consola lista: [`docs/SETUP-COMPANY-CONSOLE.md`](docs/SETUP-COMPANY-CONSOLE.md)

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
5. **Tools (Engram, GGA, OpenSpec, etc.):** see [`docs/SETUP-TOOLS.md`](docs/SETUP-TOOLS.md).

## What this branch contains

| Path | Purpose |
|------|---------|
| `AGENTS.md` | Tri-role protocol (Architect / Implementer / Security Guardian) |
| `.github/copilot-instructions.md` | Global Copilot instructions |
| `.github/instructions/` | Stack-specific rules (`applyTo` frontmatter), incl. `azure.instructions.md` |
| `.github/prompts/` | Structured delivery flows (explore, apply, verify, adversarial review) |
| `docs/governance/` | Canon rules, constraints, version index |
| `docs/company/` | Placeholders for your enterprise policies |

## What this branch does NOT contain

- `.cursor/rules/` (Cursor-only; not used here)
- `.cursor/mcp.json` — removed; use `governance-portable/templates/mcp.json.example` per project
- Personal programmer profiles / `docs/refined-rules/` SkullRender canon — removed from this branch

## Customize on the other machine

1. Edit `docs/company/*.md` with real policies.
2. Update stack section in `.github/copilot-instructions.md` (package manager, frameworks).
3. Optional: add `.github/skills/` for reusable workflows.
4. Optional: configure MCP when tools are defined.

## Related: office topology accelerator (Cursor)

For **pack-free office manifests** (Facade / PMO / stage offices) as an IaC-style formula, see the separate repo:

- https://github.com/crozzbite/office-accelerator

This governance branch remains the Copilot/rules canon. The accelerator can emit optional neutral Cursor rules (`enable_rules: true|false`) or leave room for BYO rules.

## Sync from `master`

When governance evolves on `master`, **cherry-pick** selective commits into this branch and re-neutralize identity-specific content. **Never merge** `master` ↔ `governance/copilot-portable`.
