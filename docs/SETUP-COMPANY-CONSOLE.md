# Consola VS Code — comandos listos (máquina empresa)

Copia y pega **bloque por bloque** en la terminal integrada de VS Code (PowerShell).  
No requiere cuenta GitHub personal — el repo es **público**.

Repo: https://github.com/crozzbite/WorkDesktop  
Rama: `governance/copilot-portable`

---

## Bloque 1 — Clonar y abrir

```powershell
cd $env:USERPROFILE\Documents
git clone -b governance/copilot-portable https://github.com/crozzbite/WorkDesktop.git governance
cd governance
New-Item -ItemType Directory -Force -Path .vscode
Copy-Item .vscode\settings.json.example .vscode\settings.json
```

Luego en VS Code: **File → Open Folder** → selecciona la carpeta `governance`.

---

## Bloque 2 — Engram (memoria persistente)

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

**NO hagas esto** (traería tus memorias personales):

- Copiar `%USERPROFILE%\.engram\` desde tu PC personal
- Copiar `WorkSpace\.engram\` ni ningún `.engram.db`
- `engram cloud login` con tu cuenta personal
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

Datos empresa quedarán solo en el perfil Windows de **esa** máquina (`%USERPROFILE%\.engram\`).

1. Abre https://github.com/Gentleman-Programming/engram/releases  
2. Descarga `engram_*_windows_amd64.zip`  
3. Extrae `engram.exe` a `%USERPROFILE%\bin` y agrégalo al PATH  
4. Luego:

```powershell
engram setup vscode-copilot
```

---

## Bloque 3 — GGA (review pre-commit, opcional)

Requiere **Git Bash** o WSL en la máquina empresa:

```bash
git clone https://github.com/Gentleman-Programming/gentleman-guardian-angel.git
cd gentleman-guardian-angel
bash install.sh
```

Vuelve a PowerShell en la carpeta `governance`:

```powershell
cd $env:USERPROFILE\Documents\governance
gga init
gga install
```

### Alternativa vía gentle-ai (PowerShell)

```powershell
irm https://raw.githubusercontent.com/Gentleman-Programming/gentle-ai/main/scripts/install.ps1 | iex
gentle-ai install --component gga
cd $env:USERPROFILE\Documents\governance
gga init
gga install
```

---

## Bloque 4 — OpenSpec (opcional)

Requiere **Node.js 20.19+**: https://nodejs.org

```powershell
cd $env:USERPROFILE\Documents\governance
npm install -g @fission-ai/openspec@latest
openspec init
openspec update
```

---

## Bloque 5 — Verificar Copilot

Con la carpeta `governance` abierta en VS Code y **Copilot de la empresa** activo, pega esto en **Copilot Chat**:

```
Repo clonado: governance/copilot-portable (rama independiente de master — NUNCA mergear).

Herramientas en esta máquina:
- VS Code + GitHub Copilot (cuenta empresa)
- Engram (engram setup vscode-copilot)

Lee CLONE.md y docs/SETUP-TOOLS.md.

Ayúdame a:
1. Verificar que Copilot carga copilot-instructions.md y AGENTS.md (muestra References).
2. Completar docs/company/ con políticas empresa que te pegaré después.
3. Confirmar que Engram MCP responde.
4. Una fase a la vez; sin código de producto.
```

---

## Notas empresa

| Tema | Detalle |
|------|---------|
| Cuenta GitHub | **No necesaria** para clonar (repo público) |
| Copilot | Usa la **cuenta/licencia de tu empresa** en VS Code |
| Datos Engram | Quedan en `%USERPROFILE%\.engram\` (local, máquina empresa) |
| Repo master | No clonar/usar `master` en empresa — solo rama `governance/copilot-portable` |
