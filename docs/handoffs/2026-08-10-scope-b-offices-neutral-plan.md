# Scope B — Offices SAE/SAEp neutros (plan)

> **Status:** PLAN (authoring, local until Fase 1 artifacts). Not Capa A SoT.  
> **Depends on:** Capa A = `governance/vscode-copilot-ready`.  
> **Updated:** 2026-09-11 (Fase 4 commit/push closed after SAE PR merge)  
> **JD:** Round 1 REQUEST_CHANGES → corrections below applied before Fase 1.

## Goal

Offices de etapa en **VS Code + Copilot**, **sin** packs Lich / Gentleman / Cerbero.  
- **Capa A** = reglas / tri-rol (`AGENTS.md`, `.github/…`)  
- **Capa B** = topología offices + MCP (`Office*`, `SKFLOW_ROOT`)

## Agreement

- Packs → **omitidos** (`enable_packs: false`, `inject_pack: false` siempre).
- Accelerator: `enable_rules: false` (BYO = Capa A). `.cursor/rules/office-runtime.mdc` **sigue emitiéndose**; es residuo Cursor — **no** SoT Copilot.
- **Fase 4 = B2** (una sola fuente). No mergear YAML Legion en `vscode-copilot-ready`.
- **Ids canónicos Scope B = `Office*`** (scaffold). `Saep*` / `experto_*` = árbol legacy de SkullRender-Agents — **no** SoT de Fase 1.

## Fase 0 — Contrato (congelado)

| Decisión | Valor |
|----------|--------|
| Cookbook | **`sdlc-8-stages`** (MVP más chico = `minimal-triad` solo si se abre change explícito) |
| `id_prefix` | `Office` |
| `enable_packs` | `false` |
| `enable_rules` | `false` |
| Ids esperados | `OfficeFacade`, `OfficePmo`, `OfficeScope`, `OfficeArchitecture`, `OfficeExperience`, `OfficeEngineering`, `OfficeQuality`, `OfficeDeploy`, `OfficeProduction`, `OfficeImprove` |

Mapa (mínimo, no opcional):

| Office* | Rol Capa A |
|---------|------------|
| Facade + Pmo | Collaborative / Architect (alcance) |
| Scope, Architecture | Architect |
| Experience, Engineering | Implementer |
| Quality | Implementer verify + Security overlay |
| Deploy, Production, Improve | Implementer (+ Security en release) |

## Path policy (Fase 2 + 4) — hard

**Nunca** paths de máquina en git (`C:\Users\…`), ni en esta PC.

| Qué | Dónde |
|-----|--------|
| Plantilla MCP | `mcp.json.example` (sin absolutos) |
| Offices root | Preferir script + `${workspaceFolder}` (folder B) |
| Precedencia | Si existe **`SKFLOW_ROOT`** en env → gana. Si no → root derivado del workspace folder B / script. Nunca mezclar silenciosamente. |
| CLI | Relativo al repo B o `SKFLOW_AGENTS_CLI` |
| Secretos | Env / secret store — no en `mcp.json` |

**Anti-pattern:** `mcp.json` con absolutos; MCP apuntando al default de Agents (Saep*/packs) y llamar eso “neutro”.

## Dual-root / fail-loud (JD fix)

| Abrir en VS Code | Offices MCP | Gobernanza tri-rol |
|------------------|-------------|---------------------|
| Solo Capa A | **No** disponibles (fail-loud / doc) | Sí |
| Solo Capa B (fuente offices) | Sí (`Office*`) | No (salvo multi-root) |
| **Multi-root A + B** (recomendado smoke) | Sí | Sí |

Receta: `.code-workspace` con dos folders (nombres estables) o doc “Open Folder B para MCP; A para SoT”.  
MCP debe anclarse al folder **B** (`${workspaceFolder:Name}` o script en B), no al folder A.

## B2 packaging vs `out/**` gitignore (JD fix)

Accelerator ignora `out/**`. Por tanto:

- Fase 1: generar en `out/legion-neutral` (local, gitignored) — OK para autoría.
- Fase 4: **promover** a layout versionado fuera de `out/` (p.ej. `dist/legion-neutral/` un-ignored, o tarball/release, o repo sidecar) + `scripts/mcp-offices.*` + `mcp.json.example`.
- Clone de accelerator **sin** promote ≠ producto B2.

## Pack-free = contrato, no esperanza (JD fix)

- Runtime puede listar `skflow_packs_*` → **política: no llamar**; smoke debe fallar si identity se resuelve con pack.
- Success asserts (obligatorios):
  1. `skflow_agents_list` ids ⊆ set `Office*` del scaffold (no `experto_*` como SoT).
  2. Resolve de `OfficeArchitecture` (o Scope) con `inject_pack: false`.
  3. Grep ship artifacts: cero `C:\Users`, cero pack ids branded en manifests promovidos.
  4. Abrir **solo A** → tools offices ausentes o documentado; no inventar offices.

## Phases

### Fase 1 — Scaffold (esta PC) ← DONE (local)
- [x] `params.vsc-neutral.yaml` (contrato Fase 0)
- [x] `bun run scaffold … --out ./out/legion-neutral` → 10 manifests `Office*`
- [x] Tests accelerator OK; `personality_pack_default: false` en todos
- [x] `.cursor/rules/office-runtime.mdc` (+ snippet MCP) puede existir como residuo Cursor — **no** SoT Copilot; BYO = `RULES.BYO.md` → usar Capa A

### Fase 2 — MCP VS Code (esta PC) ← DONE (local)
- [x] Wrapper `scripts/mcp-offices.ps1` + template `templates/mcp.vscode.json.example` (sin absolutos)
- [x] Precedencia: `SKFLOW_ROOT` env gana; else `out/legion-neutral`
- [x] Agents CLI: `SKFLOW_AGENTS_CLI` o sibling `../SkullRender-Agents/bundle/cli.js`
- [x] Smoke `scripts/smoke-offices.ps1` — Office*×10 + **AgentsManager loadAll=10** (YAML parse gate)
- [x] Fix accelerator `yaml-lite`: single-line strings with `:` quoted (OfficePmo was unloadable)
- [x] Dual-root recipe: `vsc-a-plus-b.code-workspace` + doc `docs/SCOPE-B-VSC-MCP.md`
- [x] Política inject_pack false / no packs como SoT documentada
- [ ] Cursor IDE MCP (`user-skullrender-agents`) aún apunta al root **legacy** Saep* — no es el smoke de VS Code offices-neutral; re-apuntar SKFLOW_ROOT solo si quieres el mismo root aquí
- [ ] Validación manual en VS Code Copilot: tools `skflow_*` tras Open Folder offices-B (humano)

### Fase 3 — Consumo Copilot ← DONE (smoke humano VS Code)
- [x] Prompt + checklist: `office-accelerator/docs/SCOPE-B-FASE3-SMOKE.md`
- [x] Prefer multi-root `vsc-a-plus-b.code-workspace` / open offices-B
- [x] Smoke B: `smoke-offices.ps1` → Office*×10 + loadAll=10; MCP `offices-neutral` en `.vscode/mcp.json`
- [x] FAIL previo = solo Capa A (fail-loud esperado); reintento en B = PASS ~9/10

### Fase 4 — Empaquetar B2 ← DONE (merged 2026-09-05)
- [x] Promote `dist/legion-neutral/` (shipped; outside `out/**`)
- [x] Scripts prefer `dist` then `out`; MCP template sin absolutos
- [x] README accelerator + Agents: install/use/verify/deploy + LLM SETUP PROMPT
- [x] Human: commit + push `office-accelerator` and `SkullRender-Agents` (PR #1 merged each)

### Fase 5 — PC trabajo
- [ ] Clone siblings + paste README setup prompts in Copilot
- [ ] Smoke = asserts Success

## Out of scope
- Packs / personalidades  
- Adapter `.github/instructions` accelerator v2  
- P1 `docs/company`  
- GGA  

## Success
1. Lista offices = set `Office*` del scaffold (sin packs como SoT)  
2. `inject_pack: false` resolve OK + brief validate  
3. Capa A intacta (no Legion YAML merge; References OK)  
4. Clone sin reescribir paths versionados  
5. Fail-loud si solo A (offices no disponibles)  
