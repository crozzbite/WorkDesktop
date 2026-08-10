# Consola VS Code — comandos listos (máquina empresa)

Copia y pega **bloque por bloque** en la terminal integrada de VS Code (PowerShell).  
No requiere cuenta GitHub personal — el repo es **público**.

Repo: https://github.com/crozzbite/WorkDesktop  
Rama recomendada: `governance/vscode-copilot-ready` (fallback: `governance/copilot-portable`)

**Fuera del setup empresa:** GGA (Gentleman Guardian Angel), Gentle AI como orquestador, packs de personalidad, Cursor-only, handoffs con paths de otra máquina. En esta PC el review portable es `adversarial-review` + `security-review` (+ la herramienta nativa que ya tenga la empresa).

---

## Bloque 1 — Clonar y abrir (obligatorio)

```powershell
cd $env:USERPROFILE\Documents
git clone -b governance/vscode-copilot-ready https://github.com/crozzbite/WorkDesktop.git governance
cd governance
New-Item -ItemType Directory -Force -Path .vscode
Copy-Item .vscode\settings.json.example .vscode\settings.json
Copy-Item governance-portable\templates\mcp.json.example .vscode\mcp.json
```

Luego en VS Code: **File → Open Folder** → selecciona la carpeta `governance`.

Si `governance/vscode-copilot-ready` aún no existe en remoto, usa:

```powershell
git clone -b governance/copilot-portable https://github.com/crozzbite/WorkDesktop.git governance
```

---

## Bloque 2 — Engram (memoria persistente) — opcional

Requiere **Go** instalado: https://go.dev/dl/

```powershell
go install github.com/Gentleman-Programming/engram/cmd/engram@latest
$env:Path = "$env:USERPROFILE\go\bin;$env:Path"
[Environment]::SetEnvironmentVariable("Path", "$env:USERPROFILE\go\bin;" + [Environment]::GetEnvironmentVariable("Path", "User"), "User")
engram version
engram setup vscode-copilot
```

**Reinicia VS Code** después de `engram setup vscode-copilot`.

### Memorias vacías (máquina nueva — importante)

`engram setup` **solo** registra MCP + instrucciones en VS Code. **No copia** memorias de otra PC.

Las memorias viven **locales** en esta máquina:

```
%USERPROFILE%\.engram\
```

En una instalación nueva, esa carpeta **no existe** → Engram empieza **vacío**. Correcto para empezar memorias de empresa desde cero.

**NO hagas esto** (traería memorias personales):

- Copiar `%USERPROFILE%\.engram\` desde otra PC
- Copiar cualquier `.engram.db`
- `engram cloud login` con cuenta personal
- Restaurar backup de Engram de otra máquina

**Verificar que está vacío** (después del setup):

```powershell
engram timeline
# o en Copilot Chat: pedir mem_context / mem_search — debe devolver vacío o solo sesión nueva
```

Si por error ya copiaste datos viejos, borra solo la DB local (máquina empresa) y reinicia:

```powershell
Remove-Item -Recurse -Force "$env:USERPROFILE\.engram" -ErrorAction SilentlyContinue
engram setup vscode-copilot
```

### Alternativa sin Go (binario)

1. Abre https://github.com/Gentleman-Programming/engram/releases  
2. Descarga `engram_*_windows_amd64.zip`  
3. Extrae `engram.exe` a `%USERPROFILE%\bin` y agrégalo al PATH  
4. Luego: `engram setup vscode-copilot`

---

## Bloque 3 — OpenSpec (opcional)

Requiere **Node.js 20.19+**: https://nodejs.org

```powershell
cd $env:USERPROFILE\Documents\governance
npm install -g @fission-ai/openspec@latest
openspec init
openspec update
```

Sin OpenSpec, usa prompts `sdd-*` y skills `strict-tdd` / `adversarial-review` / `security-review`.

---

## Bloque 4 — Verificar Copilot

Con la carpeta `governance` abierta en VS Code y **Copilot de la empresa** activo, pega esto en **Copilot Chat**:

```
Repo clonado: governance/vscode-copilot-ready (o copilot-portable). Rama independiente de master — NUNCA mergear.

Herramientas en esta máquina:
- VS Code + GitHub Copilot (cuenta empresa)
- MCP opcional: engram, context7, azure (desde .vscode/mcp.json o plantilla)

Lee CLONE.md.

Ayúdame a:
1. Verificar que Copilot carga AGENTS.md y .github/copilot-instructions.md (muestra References).
2. Confirmar settings: chat.useAgentsMdFile y chat.useCustomizationsInParentRepositories.
3. Confirmar que GGA NO es requisito de esta base.
4. Completar docs/company/ con políticas que te pegaré después.
5. Una fase a la vez; sin código de producto.
```

---

## Notas empresa

| Tema | Detalle |
|------|---------|
| Cuenta GitHub | **No necesaria** para clonar (repo público) |
| Copilot | Usa la **cuenta/licencia de tu empresa** en VS Code |
| Datos Engram | Quedan en `%USERPROFILE%\.engram\` (local) si lo instalas |
| GGA | **No** forma parte del setup empresa |
| Repo master | No clonar/usar `master` — solo rama portable / ready |
| SoT activa | `AGENTS.md` + `.github/` + `docs/governance/` (no archive, no handoffs) |
