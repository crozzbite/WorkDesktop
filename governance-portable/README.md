# Governance Portable Pack

> **Recomendado:** clona la rama **`governance/copilot-portable`** — ya viene normalizada. Este pack es documentación complementaria.

Paquete para migrar **gobernanza y estructura agéntica** a otra máquina con **GitHub Copilot en VS Code** (sin extensiones extra).

## Qué incluye este pack

| Origen (esta máquina / rama portable) | Destino (otra máquina, Copilot VS Code) |
|-----------------------|----------------------------------------|
| (histórico) `.cursor/rules/*.mdc` | Ya reemplazado por `.github/copilot-instructions.md` + `.github/instructions/` |
| `AGENTS.md` | `AGENTS.md` (plantilla en `templates/` si regeneras) |
| `docs/governance/` | Canon vivo — clonar la rama tal cual |
| `docs/governance/code-rules/` | También expuesto vía `.github/instructions/` |
| `docs/governance/constraints/` | Referenciado desde copilot-instructions |
| MCP plantilla | `.vscode/mcp.json` desde `templates/mcp.json.example` |
| `.github/skills/` | Ya en la rama |
| Prompts | Preferir `.github/prompts/sdd-*.prompt.md`; `opsx-*` si hay OpenSpec |

## Qué NO migrar

No copies tal cual a la otra máquina:

- Nombres/personas branded: Lich, Gentleman, Cerbero, Phylactery, SkullRender (histórico en `docs/archive/`)
- Plugin Engram / Azure de **Cursor** — usa MCP nativo (`engram setup vscode-copilot`, `azd coding-agent config`)
- Rutas absolutas `C:\Users\…` y referencias a `WorkSpace/`
- `docs/archive/*` como instrucciones activas

## Repos a clonar o copiar

1. **WorkDesktop** (este repo) — canon de gobernanza en `docs/` + este pack en `governance-portable/`
2. **WorkSpace** (opcional) — skills y workflows en `.agents/`; filtrar solo los genéricos (OpenSpec, TDD, seguridad OWASP, etc.)
3. **[office-accelerator](https://github.com/crozzbite/office-accelerator)** (opcional, Cursor) — fórmula IaC-like de offices pack-free + reglas neutras opcionales (`enable_rules`)

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
