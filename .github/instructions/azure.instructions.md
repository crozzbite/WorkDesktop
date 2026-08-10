---
applyTo: "**/*.{bicep,bicepparam,tf,yml,yaml}"
---

# Azure / IaC instructions (portable)

Source of truth for governance: `docs/governance/`. Prefer Azure Well-Architected + official docs via Azure MCP when available.

## MUST

- Prefer **Azure Verified Modules (AVM)** / official module patterns over one-off resource graphs when available.
- Keep secrets in Key Vault / secret stores — **never** in Bicep/TF/params committed to git.
- Use parameterized names, locations, and SKUs; document required params in README or `main.parameters.json`.
- For AI/RAG landing zones: private endpoints, DNS, and network isolation are first-class — do not invent public-by-default shortcuts.
- Call Azure MCP best-practices / documentation tools when generating or changing Azure code (if MCP is configured).

## NEVER

- Hardcode subscription IDs, tenant IDs, or client secrets in repo files.
- Open management ports or public endpoints without an explicit ADR + Security Guardian review.
- Bypass `azd` / approved deploy pipelines for production applies when company CI/CD requires them.

## Copilot / VS Code without Cursor

- Install Azure MCP via `azd coding-agent config` or copy `governance-portable/templates/mcp.json.example`.
- Cursor Azure **plugin skills** do not travel with this repo — keep durable rules here and in `docs/company/`.
