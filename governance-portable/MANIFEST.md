# Manifest — estado actual (governance/copilot-portable)

Inventario post-migración tri-role. La rama **ya** trae canon bajo `docs/governance/`. Este archivo es checklist de destino / sync, no un plan de mover `docs/refined-rules/` otra vez.

## Ya listo en esta rama (no regenerar)

| Path | Notas |
|------|-------|
| `AGENTS.md` | Tri-role Architect / Implementer / Security Guardian |
| `docs/governance/**` | Canon + constraints + code-rules + `00-version-index.md` |
| `.github/copilot-instructions.md` | Instrucciones globales Copilot |
| `.github/instructions/*.instructions.md` | Stack + security |
| `.github/prompts/sdd-*.prompt.md` | Flujo portable preferido (sin OpenSpec CLI) |
| `.github/prompts/opsx-*.prompt.md` | Solo si instalas OpenSpec CLI |
| `.github/skills/**` | OpenSpec + strict-tdd + adversarial/security review |
| `docs/company/*.md` | Placeholders — completar en la máquina destino |
| `CLONE.md` / `docs/SETUP-TOOLS.md` | Clone + tooling |

## Stubs / archive (no usar como SoT)

| Path | Acción |
|------|--------|
| `docs/00-version-index.md` | Redirect → `docs/governance/00-version-index.md` |
| `docs/code-rules/README.md` | Redirect → `docs/governance/code-rules/` |
| `docs/constraints/README.md` | Redirect → `docs/governance/constraints/` |
| `docs/archive/SR-SuperPrompts.md` | Histórico branded — no copiar a empresa |
| `SR-SuperPrompts.md` (raíz) | Stub redirect |

## No migrar a la otra máquina

| Origen | Motivo |
|--------|--------|
| `.cursor/rules/` | Cursor-only; esta rama no los usa |
| Plugin Azure / Engram de Cursor | Instalar MCP nativo en VS Code (ver `templates/mcp.json.example`) |
| `skullrender-agents` / `skullrender-skills` MCP | Marca + rutas locales |
| `docs/archive/*` | Referencia histórica |

## Destino — completar en la otra PC

1. Completar `docs/company/*.md`
2. Ajustar stack en `.github/copilot-instructions.md`
3. Copiar/adaptar `templates/mcp.json.example` → `.vscode/mcp.json` (o User MCP de VS Code)
4. Azure: `azd coding-agent config` + sección Azure en `SETUP-TOOLS.md`
5. Engram (opcional): `engram setup vscode-copilot`

## Sync desde `master`

Cherry-pick selectivo → re-neutralizar. **Nunca merge** `master` ↔ `governance/copilot-portable`.
