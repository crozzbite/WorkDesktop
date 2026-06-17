# Setup: OpenSpec + Claude Code con Ollama (local)

## 1. Instalar OpenSpec

Requisito: Node.js 20.19+ (o Bun). En esta máquina se usa **bun**.

```powershell
bun add -g @fission-ai/openspec@latest
openspec --version
```

En un proyecto SkullRender (por ejemplo WorkDesktop o phylactery):

```powershell
cd C:\Users\VECTOR\Desktop\WorkDesktop
openspec init
```

Eso crea `.openspec` y genera instrucciones en `AGENTS.md`. Luego el flujo es: `/opsx:propose` → specs → design → tasks → `/opsx:apply` → `/opsx:archive`.

---

## 2. Claude Code + Ollama: por qué "Credit balance too low"

El mensaje **"Credit balance too low - Add funds"** aparece porque el CLI de Claude Code hace una comprobación de **cuenta/créditos de Anthropic** antes (o al) enviar la petición. Aunque pongas `ANTHROPIC_BASE_URL=http://localhost:11434`, esa comprobación sigue usando la lógica de Anthropic; si la API key parece “real” o hay token de sesión, intenta facturar.

### Qué estaba mal en tu configuración

- **ANTHROPIC_API_KEY="ollama"**  
  La doc oficial de Ollama pide **ANTHROPIC_API_KEY=""** (vacío). Si pones `"ollama"`, el cliente puede interpretarlo como API key y disparar la comprobación de créditos.

- **Variables de entorno y proceso**  
  Si definiste las variables en una ventana de PowerShell y luego abriste Claude Code desde **otra** (o desde Cursor), el proceso de `claude` puede no heredar esas variables. En Windows, `SetEnvironmentVariable(..., "User")` solo afecta a **nuevos** procesos que arranquen después; si Cursor ya tenía una terminal abierta o lanzó `claude` de otra forma, no vería los valores.

### Qué hacer (recomendado)

**Opción A – Mismo proceso (variables en la misma sesión)**

En la **misma** terminal donde vayas a ejecutar `claude`:

```powershell
$env:ANTHROPIC_AUTH_TOKEN = "ollama"
$env:ANTHROPIC_API_KEY = ""
$env:ANTHROPIC_BASE_URL = "http://localhost:11434"
claude --model qwen2.5-coder:14b
```

Comprueba que Ollama esté corriendo en `http://localhost:11434` (en otra terminal: `ollama list` o `ollama serve`).

**Opción B – Config aislada (evitar mezcla con cuenta Anthropic)**

Para no mezclar con tu cuenta de claude.ai:

```powershell
$env:CLAUDE_CONFIG_DIR = "$env:LOCALAPPDATA\claude-ollama"
$env:ANTHROPIC_AUTH_TOKEN = "ollama"
$env:ANTHROPIC_API_KEY = ""
$env:ANTHROPIC_BASE_URL = "http://localhost:11434"
claude --model qwen2.5-coder:14b
```

**Opción C – Comando integrado de Ollama (recomendado si tienes Ollama reciente)**

Si tu versión de Ollama es reciente (p. ej. 0.15+):

```powershell
ollama launch claude
# o con modelo concreto
ollama launch claude --model qwen2.5-coder:14b
```

Ese comando configura y lanza Claude Code contra tu Ollama local.

### Resumen de variables correctas

| Variable               | Valor                  |
|------------------------|------------------------|
| ANTHROPIC_AUTH_TOKEN   | `ollama`               |
| ANTHROPIC_API_KEY      | `""` (cadena vacía)    |
| ANTHROPIC_BASE_URL     | `http://localhost:11434` |

No uses `ANTHROPIC_API_KEY="ollama"`. Y asegúrate de arrancar `claude` en la misma sesión donde definiste las variables (o usar `ollama launch claude`).

---

## 3. MCP con las mismas skills que Cursor

El MCP **pasa la ruta de skills como argumento** (`args`: `[cli.js, mcp, ruta]`) para que funcione aunque el host (Claude Desktop / Claude Code) no inyecte bien el `env`. Así siempre carga desde WorkSpace.

### Verificar / instalar el MCP

1. **Rebuild del MCP** (si cambiaste `src/`):
   ```powershell
   cd C:\Users\VECTOR\Desktop\WorkDesktop\skullrender-mcp-skills
   bun run bundle
   ```

2. **Configurar con skills de WorkSpace** (genera el config con la ruta en `args`):
   ```powershell
   $env:SKILLS_PATH = "C:\Users\VECTOR\Desktop\WorkSpace\.agents\skills"
   node bundle\cli.js setup claude-code
   ```
   Para **Claude Desktop** (app de escritorio):
   ```powershell
   $env:SKILLS_PATH = "C:\Users\VECTOR\Desktop\WorkSpace\.agents\skills"
   node bundle\cli.js setup claude-desktop
   ```
   Eso escribe en:
   - Claude Code: `%USERPROFILE%\.claude\settings.json`
   - Claude Desktop: `%APPDATA%\Claude\claude_desktop_config.json`

3. **Probar en terminal** (debe mostrar la ruta correcta y N skills cargadas):
   ```powershell
   node "C:\Users\VECTOR\Desktop\WorkDesktop\skullrender-mcp-skills\bundle\cli.js" mcp "C:\Users\VECTOR\Desktop\WorkSpace\.agents\skills"
   ```
   Deberías ver algo como: `Skills path: C:\...\WorkSpace\.agents\skills` y `Loaded N skills`.

4. Reiniciar Claude Code / Claude Desktop. El servidor MCP cargará todos los `**/SKILL.md` desde WorkSpace.
