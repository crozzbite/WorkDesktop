# Tooling setup — otra máquina

Enlaces oficiales y comandos para replicar el stack de herramientas que usas aquí, adaptado a **VS Code + Copilot** en la rama `governance/copilot-portable`.

> **Repo público:** https://github.com/crozzbite/WorkDesktop — clone sin cuenta personal.  
> **Consola lista para copiar:** [`docs/SETUP-COMPANY-CONSOLE.md`](SETUP-COMPANY-CONSOLE.md)

---

## 1. Base (obligatorio)

| Herramienta | Para qué | Instalación |
|-------------|----------|-------------|
| **Git** | Clone de rama governance | https://git-scm.com/download/win |
| **VS Code** | IDE | https://code.visualstudio.com |
| **GitHub Copilot** | Agente (cuenta **empresa**) | Plan/licencia corporativa en VS Code |

### Clone de esta rama (sin `gh auth`)

```powershell
git clone -b governance/copilot-portable https://github.com/crozzbite/WorkDesktop.git governance
cd governance
New-Item -ItemType Directory -Force -Path .vscode
Copy-Item .vscode\settings.json.example .vscode\settings.json
```

Ver todos los bloques en [`SETUP-COMPANY-CONSOLE.md`](SETUP-COMPANY-CONSOLE.md).

---

## 2. Engram (memoria persistente) — recomendado

| Recurso | URL |
|---------|-----|
| **Repo** | https://github.com/Gentleman-Programming/engram |
| **Instalación (todas las plataformas)** | https://github.com/Gentleman-Programming/engram/blob/main/docs/INSTALLATION.md |
| **Releases (binario Windows)** | https://github.com/Gentleman-Programming/engram/releases |
| **Setup por agente** | https://github.com/Gentleman-Programming/engram/blob/main/docs/AGENT-SETUP.md |
| **Docs (mintlify)** | https://gentleman-programming-engram.mintlify.app |

### Windows — opciones de install

**A) Go install (recomendado, sin falsos positivos de AV):**

```powershell
go install github.com/Gentleman-Programming/engram/cmd/engram@latest
# Asegura %USERPROFILE%\go\bin en PATH
engram version
```

**B) Binario precompilado:** descarga `engram_*_windows_amd64.zip` desde [Releases](https://github.com/Gentleman-Programming/engram/releases).

### Conectar con VS Code Copilot

```powershell
engram setup vscode-copilot
```

Eso escribe MCP + instrucciones en `%APPDATA%\Code\User\` (ver [INSTALLATION.md → Windows Config Paths](https://github.com/Gentleman-Programming/engram/blob/main/docs/INSTALLATION.md#windows-config-paths)).

Reinicia VS Code después del setup.

Datos locales: `%USERPROFILE%\.engram\` (override con `ENGRAM_DATA_DIR`).

---

## 3. GGA — Gentleman Guardian Angel (review pre-commit)

| Recurso | URL |
|---------|-----|
| **Repo** | https://github.com/Gentleman-Programming/gentleman-guardian-angel |
| **Comandos** | https://github.com/Gentleman-Programming/gentleman-guardian-angel/blob/main/docs/commands.md |

### Install global (Windows — Git Bash o WSL)

```bash
git clone https://github.com/Gentleman-Programming/gentleman-guardian-angel.git
cd gentleman-guardian-angel
bash install.sh
```

O vía **gentle-ai** (instala `gga` global):

```powershell
irm https://raw.githubusercontent.com/Gentleman-Programming/gentle-ai/main/scripts/install.ps1 | iex
gentle-ai install --component gga
```

### Por repo (después de clonar governance o cualquier proyecto)

```powershell
cd governance
gga init
gga install
```

---

## 4. Gentle AI (orquestador del stack)

| Recurso | URL |
|---------|-----|
| **Repo** | https://github.com/Gentleman-Programming/gentle-ai |
| **Componentes (gga, engram, skills…)** | https://github.com/Gentleman-Programming/gentle-ai/blob/main/docs/components.md |

### Windows (PowerShell)

```powershell
irm https://raw.githubusercontent.com/Gentleman-Programming/gentle-ai/main/scripts/install.ps1 | iex
gentle-ai --version
```

Instala componentes según necesidad:

```powershell
gentle-ai install --component engram
gentle-ai install --component gga
```

---

## 5. OpenSpec (entrega estructurada — opcional pero alineado con gobernanza)

| Recurso | URL |
|---------|-----|
| **Repo** | https://github.com/Fission-AI/OpenSpec |
| **npm** | https://www.npmjs.com/package/@fission-ai/openspec |
| **Docs** | https://github.com/Fission-AI/OpenSpec/blob/main/docs/README.md |
| **Herramientas soportadas (incl. VS Code)** | https://github.com/Fission-AI/OpenSpec/blob/main/docs/supported-tools.md |

### Install (Node 20.19+)

```powershell
npm install -g @fission-ai/openspec@latest
# o con bun:
# bun add -g @fission-ai/openspec@latest

cd governance
openspec init
openspec update
```

Flujo en chat: `/opsx:explore` → `/opsx:propose` → `/opsx:apply` → `/opsx:archive`.

---

## 6. Runtime / package managers (según proyecto)

| Herramienta | URL | Notas |
|-------------|-----|-------|
| **Bun** | https://bun.sh | Usado en tu máquina principal; en governance es configurable vía `docs/company/coding-standards.md` |
| **Node.js** | https://nodejs.org | Requerido por OpenSpec (20.19+) |

---

## 7. Stack local opcional (tu máquina principal — no obligatorio en empresa)

Solo si quieres terminal AI local como aquí:

| Herramienta | URL |
|-------------|-----|
| **Ollama** | https://ollama.com |
| **Claude Code** | https://docs.anthropic.com/en/docs/claude-code |
| **OpenCode** | https://github.com/sst/opencode |

Guía local (referencia en `master`, no en esta rama): `docs/SETUP-OPENSPEC-Y-CLAUDE-OLLAMA.md`.

---

## 8. Orden sugerido de setup (otra máquina)

```
1. git clone (público, sin cuenta personal)
2. Abrir carpeta en VS Code + Copilot empresa
3. .vscode/settings.json
4. engram install → engram setup vscode-copilot
5. (opcional) openspec init
6. (opcional) gga init + gga install
7. Completar docs/company/*.md
```

---

## 9. Prompt Copilot post-setup (con herramientas)

Pega en Copilot Chat después del clone:

```
Repo clonado: governance/copilot-portable (rama independiente de master, nunca mergear).

Herramientas que voy a usar:
- GitHub Copilot en VS Code (instrucciones en .github/ y AGENTS.md)
- Engram (engram setup vscode-copilot)
- [opcional] OpenSpec, GGA

Ayúdame a:
1. Verificar que Copilot carga copilot-instructions.md y AGENTS.md (References).
2. Completar docs/company/ con mis políticas: [PEGAR].
3. Confirmar que Engram MCP responde (mem_context o equivalente).
4. Una fase a la vez; sin código de producto.

Lee CLONE.md y docs/SETUP-TOOLS.md primero.
```

---

## Sync entre ramas (recordatorio)

```
master  ──cherry-pick──►  governance/copilot-portable
         NUNCA merge
```

Cuando actualices reglas en `master`, cherry-pick + re-normalizar en governance.
