# Tooling setup — otra máquina (VS Code + Copilot)

Enlaces y comandos para el stack **portable** en ramas `governance/vscode-copilot-ready` / `governance/copilot-portable`.

> **Repo público:** https://github.com/crozzbite/WorkDesktop — clone sin cuenta personal.  
> **Consola lista:** [`docs/SETUP-COMPANY-CONSOLE.md`](SETUP-COMPANY-CONSOLE.md)

**Roles de este doc**

| Tier | Qué |
|------|-----|
| **Empresa / portable** | Git, VS Code, GitHub Copilot, settings del repo, plantilla MCP, skills/prompts del repo |
| **Opcional portable** | Engram, Context7, Azure MCP, OpenSpec CLI |
| **Solo máquina personal** | GGA, Gentle AI orquestador, Ollama/Claude Code, paths locales — **no** requisito de migración |

---

## 1. Base (obligatorio en empresa)

| Herramienta | Para qué | Instalación |
|-------------|----------|-------------|
| **Git** | Clone de rama governance | https://git-scm.com/download/win |
| **VS Code** | IDE | https://code.visualstudio.com |
| **GitHub Copilot** | Agente (cuenta **empresa**) | Plan/licencia corporativa en VS Code |

### Clone (sin `gh auth`)

```powershell
git clone -b governance/vscode-copilot-ready https://github.com/crozzbite/WorkDesktop.git governance
cd governance
New-Item -ItemType Directory -Force -Path .vscode
Copy-Item .vscode\settings.json.example .vscode\settings.json
Copy-Item governance-portable\templates\mcp.json.example .vscode\mcp.json
```

Fallback de rama: `governance/copilot-portable`.

Ver bloques en [`SETUP-COMPANY-CONSOLE.md`](SETUP-COMPANY-CONSOLE.md).

---

## 2. Engram (memoria) — opcional portable

| Recurso | URL |
|---------|-----|
| **Repo** | https://github.com/Gentleman-Programming/engram |
| **Instalación** | https://github.com/Gentleman-Programming/engram/blob/main/docs/INSTALLATION.md |
| **Releases Windows** | https://github.com/Gentleman-Programming/engram/releases |
| **Setup por agente** | https://github.com/Gentleman-Programming/engram/blob/main/docs/AGENT-SETUP.md |

```powershell
go install github.com/Gentleman-Programming/engram/cmd/engram@latest
engram version
engram setup vscode-copilot
```

Reinicia VS Code. Datos locales: `%USERPROFILE%\.engram\` — **no** copiar desde otra PC.

---

## 3. MCP portable (Engram / Context7 / Azure) — opcional

Plantilla shippable (sin paths de usuario):

`governance-portable/templates/mcp.json.example`

```powershell
New-Item -ItemType Directory -Force -Path .vscode
Copy-Item governance-portable\templates\mcp.json.example .vscode\mcp.json
# Reinicia VS Code
```

- **No** commits de `.vscode/mcp.json` con rutas absolutas o servidores personales.
- Sustituto Azure: `azd coding-agent config` si usas Azure Developer CLI.
- Reglas durables: `.github/instructions/azure.instructions.md`

El **plugin Azure de Cursor** no se copia a VS Code.

---

## 4. OpenSpec (opcional)

| Recurso | URL |
|---------|-----|
| **Repo** | https://github.com/Fission-AI/OpenSpec |
| **npm** | https://www.npmjs.com/package/@fission-ai/openspec |

```powershell
npm install -g @fission-ai/openspec@latest
cd governance
openspec init
openspec update
```

Sin OpenSpec: usa `.github/prompts/sdd-*.prompt.md` y skills core (`strict-tdd`, `adversarial-review`, `security-review`).

---

## 5. Azure CLIs (opcional, si hay cloud)

| Recurso | URL |
|---------|-----|
| **Azure CLI** | https://learn.microsoft.com/cli/azure/install-azure-cli |
| **azd** | https://learn.microsoft.com/azure/developer/azure-developer-cli/install-azd |
| **MCP troubleshooting** | https://aka.ms/azmcp/troubleshooting |

```powershell
azd coding-agent config
```

---

## 6. Runtime (según proyecto)

| Herramienta | URL | Notas |
|-------------|-----|-------|
| **Node.js** | https://nodejs.org | OpenSpec 20.19+ |
| **Bun** | https://bun.sh | Configurable en `docs/company/coding-standards.md` |

---

## 7. Orden sugerido (máquina empresa)

```
1. git clone (rama ready / portable)
2. Abrir carpeta en VS Code + Copilot empresa
3. .vscode/settings.json (+ mcp.json desde plantilla si quieres MCP)
4. (opcional) engram setup vscode-copilot
5. (opcional) az/azd + Azure MCP
6. (opcional) openspec init
7. Completar docs/company/*.md
8. Smoke: References = AGENTS.md + copilot-instructions.md
```

**No** incluir GGA en este orden.

---

## 8. Solo máquina personal (fuera de migración)

Estas herramientas pueden existir en tu PC de autoría. **No** son requisito de la otra PC ni del ship path.

| Herramienta | Notas |
|-------------|-------|
| **GGA** (Gentleman Guardian Angel) | Review pre-commit local. Sustituto portable: skills `adversarial-review` + `security-review` + tool nativa empresa |
| **Gentle AI** | Orquestador de install personal |
| **Ollama / Claude Code / OpenCode** | Terminal AI local |
| Handoffs / paths `C:\Users\…` | Valor local; no copiar al clone empresa |

GGA (referencia, no setup empresa): https://github.com/Gentleman-Programming/gentleman-guardian-angel

---

## 9. Prompt Copilot post-setup

```
Repo: governance/vscode-copilot-ready (o copilot-portable). Nunca mergear con master.

Herramientas:
- GitHub Copilot en VS Code (AGENTS.md + .github/)
- MCP opcional: engram, context7, azure
- OpenSpec opcional

NO uses GGA como requisito.

Ayúdame a:
1. Verificar References (AGENTS.md, copilot-instructions.md).
2. Completar docs/company/: [PEGAR].
3. Confirmar MCP si está configurado.
4. Una fase a la vez; sin código de producto.

Lee CLONE.md primero.
```

---

## Sync entre ramas

```
master  ──cherry-pick──►  governance/copilot-portable / vscode-copilot-ready
         NUNCA merge
```
