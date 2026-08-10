# Governance repo — Copilot / VS Code (portable)

Ramas: `governance/vscode-copilot-ready` (preferida) · `governance/copilot-portable` (base).

Cursor = autoría / limpieza. VS Code + GitHub Copilot = consumo portable.

## Active source of truth (SoT)

**Operativo (viaja con el clone):**

1. `AGENTS.md` — tri-rol (Architect / Implementer / Security Guardian)
2. `.github/copilot-instructions.md` + `.github/instructions/` + `.github/prompts/` + `.github/skills/`
3. `docs/governance/` — canon (`00-version-index.md`, refined-rules, constraints, code-rules)
4. `docs/company/` — políticas empresa (placeholders hasta P1)
5. `.vscode/settings.json.example` + `governance-portable/templates/mcp.json.example`

**No operativo / no ship path:**

- `docs/archive/`, handoffs con paths locales, notes de OneDrive / `C:\Users\…`
- Product folders (`Check-U/`, etc.)
- SkullRender-Agents, office-accelerator, packs de personalidad (Lich / Gentleman / Cerbero)
- GGA como requisito
- `.cursor/rules/` (Cursor-only)
- `.vscode/mcp.json` personalizado con rutas absolutas (usa la plantilla)

## Clone

```bash
git clone -b governance/vscode-copilot-ready https://github.com/crozzbite/WorkDesktop.git governance
cd governance
```

> Repo **PUBLIC** — https://github.com/crozzbite/WorkDesktop  
> Consola: [`docs/SETUP-COMPANY-CONSOLE.md`](docs/SETUP-COMPANY-CONSOLE.md) · Tools: [`docs/SETUP-TOOLS.md`](docs/SETUP-TOOLS.md)

Fallback:

```bash
git clone -b governance/copilot-portable https://github.com/crozzbite/WorkDesktop.git governance
```

## VS Code setup (native Copilot)

1. Open the folder in VS Code.
2. Sign in with **company** GitHub Copilot.
3. Copy `.vscode/settings.json.example` → `.vscode/settings.json`.
4. Optional MCP: copy `governance-portable/templates/mcp.json.example` → `.vscode/mcp.json` (no absolute user paths).
5. Confirm settings:
   - `chat.useAgentsMdFile`: true
   - `chat.useCustomizationsInParentRepositories`: true
   - `github.copilot.chat.codeGeneration.instructions` → `docs/governance/constraints/non-negotiables.md`
6. Fill `docs/company/` when real policies exist (P1).

### References gate (smoke)

In Copilot Chat, References should include at least:

- `AGENTS.md`
- `.github/copilot-instructions.md`
- preferably `docs/governance/00-version-index.md` and/or `non-negotiables.md`

If that fails, fix settings/scope before skills or MCP.

## What this branch contains

| Path | Purpose |
|------|---------|
| `AGENTS.md` | Tri-role protocol |
| `.github/copilot-instructions.md` | Global Copilot instructions |
| `.github/instructions/` | Stack rules (`applyTo`), incl. Azure |
| `.github/prompts/` | SDD + adversarial review (`opsx-*` optional / OpenSpec) |
| `.github/skills/` | Core: strict-tdd, adversarial-review, security-review; OpenSpec optional |
| `docs/governance/` | Canon |
| `docs/company/` | Enterprise policy placeholders |
| `governance-portable/templates/mcp.json.example` | Portable MCP (engram, context7, azure) |

## Review substitute (no GGA in portable path)

Use repo skills/prompts: `adversarial-review`, `security-review`, plus the client machine’s native review tool. GGA may remain installed on an authoring PC; it is **not** part of company setup.

## Customize on the other machine

1. Edit `docs/company/*.md`.
2. Update stack in `.github/copilot-instructions.md` if needed.
3. Optional: Engram / Azure MCP / OpenSpec — see SETUP-TOOLS.

## Sync from `master`

Cherry-pick selective commits; re-neutralize identity-specific content. **Never merge** `master` ↔ portable/ready branches.
