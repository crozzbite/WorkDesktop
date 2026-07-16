# Prompt para Copilot (VS Code) — refinamiento opcional

> **Nota:** La rama `governance/copilot-portable` ya trae la estructura lista. Usa este prompt **solo** si quieres que Copilot integre políticas de empresa o adapte el stack en la otra máquina.

Copia el bloque entre `--- INICIO ---` y `--- FIN ---` si necesitas asistencia adicional.

---

## --- INICIO --- (copiar desde aquí)

Eres mi arquitecto de gobernanza para IA en este workspace. Tu tarea es **crear una estructura agéntica similar a la del repo de referencia**, pero **sin identidad de marca** (nada de SkullRender, Lich, Gentleman, Cerbero, Phylactery, zzorc ni rutas absolutas de otra máquina).

### Contexto

Tengo un repo de referencia con gobernanza madura (reglas refinadas, constraints, code-rules, protocolo tri-rol, loops de agente, TDD estricto, review adversarial). Quiero replicar **la gobernanza y el comportamiento**, no la estética ni los nombres ficticios.

**Plataforma destino:** Visual Studio Code con **GitHub Copilot nativo** (sin extensiones adicionales por ahora).

**Documentación oficial a respetar:**
- Instrucciones globales: `.github/copilot-instructions.md`
- Instrucciones por archivo: `.github/instructions/*.instructions.md` con frontmatter `applyTo`
- Protocolo multi-rol: `AGENTS.md` (habilitar `chat.useAgentsMdFile`)
- Prompts reutilizables: `.github/prompts/*.prompt.md`
- Skills opcionales: `.github/skills/<name>/SKILL.md`
- NO uses `.cursor/rules/` — eso es de Cursor, no de VS Code

### Repos de referencia (léelos antes de generar archivos)

1. **Repo principal de gobernanza** — carpeta `docs/`:
   - `docs/00-version-index.md` — índice de reglas vigentes
   - `docs/refined-rules/hierarchy.md` — precedencia cuando hay conflicto
   - `docs/refined-rules/07-security-refined.md` — seguridad OWASP
   - `docs/refined-rules/08-workflow-refined.md` — fases y STOP conditions
   - `docs/refined-rules/09-agent-loops-refined.md` — loops TDD + review gate
   - `docs/refined-rules/09-strict-tdd-refined.md` — micro-loop de implementación
   - `docs/constraints/non-negotiables.md` — MUST/NEVER plano
   - `docs/code-rules/` — reglas por lenguaje/stack

2. **Pack portable** — carpeta `governance-portable/`:
   - `README.md`, `MANIFEST.md`
   - `templates/AGENTS.template.md`
   - `templates/copilot-instructions.template.md`
   - `templates/mcp.json.example`
   - `templates/settings.vscode.json.example`

3. **Políticas de mi empresa** (las adjunto o pegaré después):
   - [PEGA AQUÍ: rutas o contenido de políticas de seguridad, compliance, coding standards corporativos]

### Mapeo de roles (neutralizar identidad)

| Origen (referencia) | Destino (neutral) |
|---------------------|-------------------|
| Lich / Arquitecto Eterno | **Architect** |
| Gentleman / Ejecutor | **Implementer** |
| Cerbero / Guardián | **Security Guardian** |
| @Dual | **@Collaborative** |
| Judgment Day (JD) | **Adversarial Review** |
| SDD `/sdd-*` | Prompts en `.github/prompts/sdd-*.prompt.md` |

### Entregables (crear en este orden)

**Fase 1 — Estructura base**

1. `docs/governance/00-version-index.md` — índice sin marca SkullRender
2. `docs/governance/refined-rules/` — copiar y neutralizar los archivos listados arriba (omitir `programmer-profile-zzorc.md` y `skullrender-cicd-standard.md`; crear placeholders corporativos)
3. `docs/governance/constraints/` — copiar `non-negotiables.md`, `by-pillar.md`, `by-domain.md` adaptando MUST/NEVER a políticas empresa
4. `AGENTS.md` — basado en `governance-portable/templates/AGENTS.template.md`
5. `.github/copilot-instructions.md` — basado en `templates/copilot-instructions.template.md` + merge de constraints

**Fase 2 — Instrucciones por stack**

6. `.github/instructions/typescript.instructions.md` — desde `docs/code-rules/typescript.md`, frontmatter `applyTo: "**/*.{ts,tsx}"`
7. `.github/instructions/python.instructions.md` — si aplica
8. `.github/instructions/angular.instructions.md` — si aplica
9. `.github/instructions/security.instructions.md` — resumen de Rule 07 para archivos sensibles (`applyTo: "**/*"` solo si es liviano; si no, prompts)

**Fase 3 — Flujos agénticos (sin slash-commands de Cursor)**

10. `.github/prompts/sdd-explore.prompt.md` — exploración de tema antes de implementar
11. `.github/prompts/sdd-apply.prompt.md` — implementación con Strict TDD micro-loop
12. `.github/prompts/sdd-verify.prompt.md` — verificación tests + spec
13. `.github/prompts/adversarial-review.prompt.md` — equivalente a Judgment Day (1× por cambio completo)

**Fase 4 — Settings y MCP (opcional)**

14. `.vscode/settings.json` — `chat.useAgentsMdFile: true`, `chat.useCustomizationsInParentRepositories: true`
15. Documentar MCP en `docs/governance/mcp-setup.md` — plantilla desde `mcp.json.example`; **no configurar MCP hasta que defina herramientas en esta máquina**

### Reglas de comportamiento para ti (Copilot)

1. **No implementes código de producto** en esta sesión — solo estructura de gobernanza.
2. **Una fase a la vez.** Tras cada fase, resume: qué creaste, riesgos, y pide confirmación antes de la siguiente.
3. **Integra políticas empresa** donde choquen con el repo de referencia; documenta el override en `docs/governance/00-version-index.md`.
4. **Mantén** jerarquía de reglas, STOP conditions, tri-rol, Strict TDD y Adversarial Review — son el núcleo.
5. **Elimina** referencias a: SkullRender, Phylactery, Engram, `gga`, rutas `C:\Users\zzorc`, WorkSpace legacy.
6. Si algo depende de OpenSpec y no está instalado, deja el flujo documentado con carpetas `openspec/changes/<name>/` como convención.
7. Commits sugeridos por fase, mensajes en inglés.

### Criterios de éxito

- Copilot Chat muestra en References: `copilot-instructions.md` y/o `AGENTS.md` en peticiones normales.
- Las instrucciones por lenguaje se aplican al editar archivos del tipo correcto.
- Un desarrollador nuevo entiende en 10 min: roles, bans, workflow, y cómo invocar adversarial-review.
- Cero menciones de identidad SkullRender en archivos generados.

### Primera acción

Lee `governance-portable/MANIFEST.md` y `docs/00-version-index.md` del repo de referencia. Propón un plan de 4 fases con lista exacta de archivos a crear. Espera mi OK antes de escribir archivos.

## --- FIN --- (copiar hasta aquí)

---

## Variante corta (si el contexto es limitado)

Si Copilot trunca el prompt largo, usa esta versión mínima:

```
Crea gobernanza agéntica en VS Code Copilot (sin extensiones) a partir del repo adjunto:
- Neutraliza roles: Architect, Implementer, Security Guardian (sin SkullRender/Lich/Cerbero).
- Estructura: docs/governance/, AGENTS.md, .github/copilot-instructions.md, .github/instructions/*.instructions.md, .github/prompts/sdd-*.prompt.md.
- Conserva: hierarchy, security rule 07, workflow STOPs, agent loops (Strict TDD + adversarial review).
- Integra mis políticas empresa: [PEGAR].
- Fase 1 solo: índice + copilot-instructions + AGENTS.md. Pide confirmación entre fases.
Lee governance-portable/MANIFEST.md primero.
```

## Después del setup

1. Verifica: pide a Copilot "¿qué instrucciones estás usando?" y revisa References.
2. Añade políticas empresa en `docs/company/` y enlázalas desde `copilot-instructions.md`.
3. Cuando tengas stack definido, completa `.github/instructions/` y MCP.
4. Opcional: migrar skills útiles de `WorkSpace/.agents/skills/` a `.github/skills/`.
