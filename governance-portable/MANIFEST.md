# Manifest — archivos de gobernanza

Inventario de lo que ya está versionado en WorkDesktop y qué hacer en destino.

## Ya en git (WorkDesktop)

### Reglas Cursor (origen → reemplazar en destino)

| Archivo | Acción en destino |
|---------|-------------------|
| `.cursor/rules/skullrender-lich.mdc` | **No copiar.** Reemplazar por `copilot-instructions.md` neutral |
| `.cursor/rules/persona-cerbero.mdc` | Neutralizar → rol Security Guardian en `AGENTS.md` |
| `.cursor/rules/programmer-profile-zzorc.mdc` | **No copiar.** Crear perfil corporativo |
| `.cursor/mcp.json` | Adaptar rutas → `templates/mcp.json.example` |

### Protocolo agéntico

| Archivo | Acción en destino |
|---------|-------------------|
| `AGENTS.md` | Usar `templates/AGENTS.template.md` como base |

### Canon de gobernanza (`docs/`)

| Archivo | Neutralizar | Prioridad |
|---------|-------------|-----------|
| `docs/00-version-index.md` | Sí (quitar SkullRender) | Alta |
| `docs/refined-rules/hierarchy.md` | Mínima | Alta |
| `docs/refined-rules/07-security-refined.md` | Mínima | Alta |
| `docs/refined-rules/08-workflow-refined.md` | Sustituir OpenSpec/SDD por equivalente corporativo si aplica | Alta |
| `docs/refined-rules/09-agent-loops-refined.md` | Renombrar roles; mantener loops TDD + review gate | Alta |
| `docs/refined-rules/09-strict-tdd-refined.md` | Mínima | Media |
| `docs/refined-rules/02-patterns-refined.md` | Mínima | Media |
| `docs/refined-rules/05-cognitive-refined.md` | Adaptar gateway LLM a stack empresa | Media |
| `docs/refined-rules/persona-cerbero.md` | Renombrar a security-guardian | Media |
| `docs/refined-rules/00-identity-refined.md` | **Reescribir** con políticas empresa | Alta |
| `docs/refined-rules/programmer-profile-zzorc.md` | **Omitir** | — |
| `docs/refined-rules/skullrender-cicd-standard.md` | **Reescribir** CI/CD empresa | Media |
| `docs/constraints/*` | Revisar MUST/NEVER vs políticas empresa | Alta |
| `docs/code-rules/*` | Mover a `.github/instructions/` | Alta |

## Fuera de git (WorkSpace — copia manual)

| Ruta | Contenido útil | Destino sugerido |
|------|----------------|------------------|
| `WorkSpace/.agents/skills/openspec*/` | Flujo SDD/OpenSpec | `.github/skills/openspec/` |
| `WorkSpace/.agents/skills/judgment-day/` | Review adversarial | `.github/skills/adversarial-review/` |
| `WorkSpace/.agents/skills/app-security/` | OWASP | `.github/skills/security-review/` |
| `WorkSpace/.agents/workflows/genesis-protocol.md` | Solo si haces greenfield | `docs/governance/genesis.md` |
| `WorkSpace/.gemini/skullrender-rules.md` | Legacy | **No copiar** |

## Orden sugerido de implementación

1. `docs/governance/` + índice de versiones
2. `.github/copilot-instructions.md`
3. `AGENTS.md`
4. `.github/instructions/` por lenguaje
5. `.github/prompts/` para flujos SDD
6. `.github/skills/` (opcional, 2–3 skills críticos)
7. MCP (último; depende del stack en la otra máquina)
