# Governance Portable Pack

> **Recomendado:** clona la rama **`governance/copilot-portable`** — ya viene normalizada. Este pack es documentación complementaria.

Paquete para migrar **gobernanza y estructura agéntica** a otra máquina con **GitHub Copilot en VS Code** (sin extensiones extra).

## Qué incluye este pack

| Origen (esta máquina) | Destino (otra máquina, Copilot VS Code) |
|-----------------------|----------------------------------------|
| `.cursor/rules/*.mdc` | `.github/copilot-instructions.md` + `.github/instructions/*.instructions.md` |
| `AGENTS.md` | `AGENTS.md` (plantilla neutral en `templates/`) |
| `docs/refined-rules/` | `docs/governance/` (renombrar y neutralizar identidad) |
| `docs/code-rules/` | `.github/instructions/` por stack (`typescript.instructions.md`, etc.) |
| `docs/constraints/` | Sección en `copilot-instructions.md` o `docs/governance/constraints/` |
| `.cursor/mcp.json` | `.vscode/mcp.json` o configuración MCP de VS Code (por proyecto) |
| `WorkSpace/.agents/skills/` | `.github/skills/<nombre>/SKILL.md` (skills nativas de VS Code) |
| Comandos `/sdd-*` (chat) | `.github/prompts/sdd-*.prompt.md` |

## Qué NO migrar (identidad SkullRender)

No copies tal cual a la otra máquina:

- Nombres/personas: Lich, Gentleman, Cerbero, Phylactery, SkullRender
- `docs/refined-rules/00-identity-refined.md` (reescribir con políticas de empresa)
- `docs/refined-rules/programmer-profile-zzorc.md` (crear perfil de empresa)
- `docs/refined-rules/skullrender-cicd-standard.md` (adaptar a CI/CD corporativo)
- Rutas absolutas `C:\Users\zzorc\...` y referencias a `WorkSpace/`
- Engram / memoria persistente (opcional; no es nativo de Copilot)

## Repos a clonar o copiar

1. **WorkDesktop** (este repo) — canon de gobernanza en `docs/` + este pack en `governance-portable/`
2. **WorkSpace** (opcional) — skills y workflows en `.agents/`; filtrar solo los genéricos (OpenSpec, TDD, seguridad OWASP, etc.)

## Pasos rápidos en la otra máquina

1. `git clone -b governance/copilot-portable <REPO_URL> governance`
2. Leer `CLONE.md` en la raíz del repo.
3. Abrir en VS Code con Copilot activo.
4. Copiar `.vscode/settings.json.example` → `.vscode/settings.json`.
5. Completar `docs/company/*.md` con políticas de empresa.
6. *(Opcional)* Usar `PROMPT-COPILOT-SETUP.md` solo si quieres que Copilot refine políticas empresa.

## Settings recomendados (VS Code)

Ver `templates/settings.vscode.json.example`. Mínimo:

```json
{
  "chat.useAgentsMdFile": true,
  "chat.useCustomizationsInParentRepositories": true
}
```

## MCP

- En Cursor: `.cursor/mcp.json`
- En VS Code Copilot Agent: configurar MCP por workspace (ver plantilla `templates/mcp.json.example`)
- Sin MCP: la gobernanza en Markdown sigue funcionando; solo pierdes herramientas externas (Angular CLI, Engram, etc.)

## Archivo principal para Copilot

**`PROMPT-COPILOT-SETUP.md`** — cópialo completo al chat de la otra computadora.
