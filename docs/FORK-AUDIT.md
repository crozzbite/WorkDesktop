# Fork audit — v-jonathanz/WorkDesktop vs crozzbite/WorkDesktop

**Fork:** https://github.com/v-jonathanz/WorkDesktop (`governance/copilot-portable` @ `5dbc114`)  
**Upstream:** https://github.com/crozzbite/WorkDesktop (`governance/copilot-portable` @ `9c7d687`)  
**Date:** 2026-07-11

---

## Resumen ejecutivo

El fork partió de upstream pero **Copilot tuvo que parchear** porque nuestro pack portable dejó basura de SkullRender, rutas locales de zzorc, y faltaban piezas que VS Code + OpenSpec necesitan nativamente.

| Categoría | Problemas nuestros | Fixes del fork | Aún mal en fork |
|-----------|-------------------|----------------|-----------------|
| Identidad SkullRender | `.clinerules`, `docs/refined-rules/*` | — | Heredado sin limpiar |
| Rutas locales | `.cursor/mcp.json` → `C:\Users\zzorc\...` | — | Sigue igual |
| OpenSpec / Copilot | Sin `openspec init`, prompts `sdd-*` | `opsx-*`, `config.yaml`, skills | Parcial |
| VS Code settings | Solo `.example` | `.vscode/settings.json` commiteado | OK |
| Gobernanza loops | Sin skills TDD/review/security | — | Upstream ya añadió |

---

## 1. Errores nuestros (upstream original)

### 🔴 Crítico — identidad y rutas personales expuestas

| Archivo | Problema |
|---------|----------|
| `.clinerules` | **442 líneas** de personalidad SkullRender ("Senior Architect", Rioplatense, filosofía Lich). Copilot/VS Code lo lee como instrucciones globales. |
| `docs/refined-rules/persona-cerbero.md` | Cerbero, Lich, Gentleman, emojis 💀🥸🚨 |
| `docs/refined-rules/programmer-profile-zzorc.md` | Perfil personal zzorc |
| `docs/refined-rules/skullrender-cicd-standard.md` | CI/CD marca SkullRender |
| `.cursor/mcp.json` | Ruta absoluta `C:/Users/zzorc/OneDrive/Desktop/WorkDesktop/DnDApp/...` — **rompe en cualquier otra máquina** |

**Por qué pasó:** la rama `governance/copilot-portable` se creó desde `master` que ya traía `docs/refined-rules/` y `.clinerules`. Normalizamos `docs/governance/` pero **no eliminamos** el canon viejo ni `.clinerules`.

### 🟠 Alto — faltaba wiring OpenSpec nativo

| Gap | Impacto |
|-----|---------|
| Sin `openspec/config.yaml` | `openspec init` no corría; flujo `/opsx:*` no funcionaba |
| Prompts solo `sdd-*.prompt.md` | Genéricos; OpenSpec en VS Code usa **`/opsx:propose`**, **`/opsx:apply`**, etc. |
| Sin skills OpenSpec en `.github/skills/` | Copilot no encontraba workflows estructurados |
| Sin `.vscode/settings.json` (solo `.example`) | Usuario debía copiar manualmente; fácil olvidarlo |

### 🟡 Medio — confusión Cursor vs VS Code

| Gap | Impacto |
|-----|---------|
| Documentación decía "no uses `.cursor/rules`" pero dejamos `.cursor/mcp.json` | Señal mixta |
| `docs/SETUP-OPENSPEC-Y-CLAUDE-OLLAMA.md` en la rama | Guía local zzorc (Ollama, Claude Code) — irrelevante en empresa |
| Duplicación `docs/` + `docs/governance/` | Copilot puede leer el path equivocado (`docs/refined-rules` vs `docs/governance`) |

---

## 2. Lo que el fork hizo bien (parches necesarios)

Commit `5dbc114` — Copilot en empresa:

| Añadido | Por qué era necesario |
|---------|----------------------|
| `openspec/config.yaml` | Habilitar OpenSpec spec-driven en el repo |
| `.github/prompts/opsx-*.prompt.md` (5) | Alineado con slash commands reales de OpenSpec |
| `.github/skills/openspec-*` (5) | Skills descubribles por VS Code Copilot |
| `.cursor/commands/opsx-*.md` | Fallback por si el host lee comandos Cursor |
| `.cursor/skills/openspec-*` | Duplicado defensivo (misma razón) |
| `.vscode/settings.json` | Settings activos sin paso manual |
| `.gga` + hooks | GGA operativo en repo empresa |
| Engram MCP (según commit message) | Memoria persistente en VS Code |

**Conclusión:** el fork no "inventó" de más — **compensó huecos** de nuestro pack.

---

## 3. Lo que el fork aún tiene mal (heredado, no limpiado)

| Archivo | Estado |
|---------|--------|
| `.clinerules` | SkullRender completo — **debe borrarse o neutralizarse** |
| `docs/refined-rules/*` (SkullRender) | Duplica y contradice `docs/governance/` |
| `.cursor/mcp.json` | Rutas zzorc — **debe borrarse** en máquina empresa |
| `docs/SETUP-OPENSPEC-Y-CLAUDE-OLLAMA.md` | Stack personal local |

---

## 4. Gaps que upstream ya cerró (post-fork)

Upstream `9c7d687` añadió lo que el fork aún no tiene:

- `openspec-verify-change` skill
- `strict-tdd` skill
- `adversarial-review` skill
- `security-review` skill
- `.github/skills/README.md`

---

## 5. Lecciones — qué cambiar "desde acá"

### Checklist rama `governance/copilot-portable` (upstream)

- [ ] **Eliminar** `docs/refined-rules/` (canon SkullRender)
- [ ] **Eliminar** `.clinerules` o reemplazar por stub neutral
- [ ] **Eliminar** `.cursor/mcp.json` (o dejar solo `governance-portable/templates/mcp.json.example`)
- [ ] **Añadir** `openspec/config.yaml` (como el fork)
- [ ] **Añadir** `.github/prompts/opsx-*.prompt.md` (mantener `sdd-*` como alias opcional)
- [ ] **Commitear** `.vscode/settings.json` además del `.example`
- [ ] **No incluir** `docs/SETUP-OPENSPEC-Y-CLAUDE-OLLAMA.md` en rama governance
- [ ] **Un solo lugar** para skills: `.github/skills/` (no duplicar en `.cursor/`)

### Sync fork empresa (cherry-pick, sin merge)

```powershell
cd governance
git remote add upstream https://github.com/crozzbite/WorkDesktop.git
git fetch upstream governance/copilot-portable
# Tras cleanup commit en upstream:
git cherry-pick <cleanup-commit>
git push
```

Luego en fork manualmente:
- Borrar `.clinerules`, `docs/refined-rules/`, `.cursor/mcp.json`
- Revisar `.gga` PROVIDER para stack empresa (no asumir `claude` CLI)

---

## 6. Matriz de prompts

| Upstream (antes) | Fork (fix) | Uso en VS Code |
|------------------|------------|----------------|
| `sdd-explore.prompt.md` | `opsx-explore.prompt.md` | `/opsx:explore` |
| — | `opsx-propose.prompt.md` | `/opsx:propose` |
| `sdd-apply.prompt.md` | `opsx-apply.prompt.md` | `/opsx:apply` |
| `sdd-verify.prompt.md` | — (usa openspec-verify skill) | `/opsx:verify` |
| — | `opsx-archive.prompt.md` | `/opsx:archive` |
| — | `opsx-sync.prompt.md` | `/opsx:sync` |
| `adversarial-review.prompt.md` | (igual) | Chat manual |

**Error nuestro:** nombrar todo `sdd-*` sin alinear con OpenSpec `/opsx:*` que Copilot registra nativamente.
