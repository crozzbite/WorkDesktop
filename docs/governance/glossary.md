---
version: 1.0
current: true
description: Acronym and term glossary. Official lookup source for RULE_001 (Resolve_Context). Supporting doc.
---

# Glossary — Enterprise & Repo Canon

**Version:** 1.0 ✓ (current)
**Source:** "Enterprise AI Learning Agent Dictionary" v1.0.0 (2026-07-15) + `repo_canon` additions.
**Wiring:** RULE_001 (Resolve_Context) resolves unknown acronyms against this file first.
Selected terms include **DO / DON'T** usage examples (same style as `code-rules/`).

---

## Business

| Acronym | Full name | Español | Meaning |
|---------|-----------|---------|---------|
| KPI | Key Performance Indicator | Indicador Clave de Desempeño | Metric measuring success or progress toward a goal |
| ROI | Return On Investment | Retorno de Inversión | Value obtained compared to cost incurred |
| SLA | Service Level Agreement | Acuerdo de Nivel de Servicio | Formal commitment on service time or quality |
| ETA | Estimated Time of Arrival | Tiempo Estimado de Finalización | Estimate of when something will be done |
| RCA | Root Cause Analysis | Análisis de Causa Raíz | Investigation to identify the real origin of a problem |
| PIR | Post Incident Review | Revisión Posterior al Incidente | Lessons learned after an incident |
| FY | Fiscal Year | Año Fiscal | Financial year used for business planning |
| Q | Quarter | Trimestre | One fourth of a fiscal year |

**SLA vs ETA — DO / DON'T**

- **DO:** treat an SLA as a *contractual* boundary ("restore service within 4h") and an ETA as an *estimate* ("I expect this fixed by Thursday").
- **DON'T:** promise an ETA with SLA language. An estimate that slips is normal; a missed SLA is a breach.

**RCA — DO / DON'T** *(wired to RULE_012 Root_Cause_Thinking)*

- **DO:** iterate "why" until you reach a cause that, if fixed, prevents recurrence. Symptom: "the pod crashed" → cause: "no memory limit + unbounded cache".
- **DON'T:** stop at the first plausible symptom, or name a person as the root cause. People are triggers; systems are causes.

---

## Delivery

| Acronym | Full name | Español | Meaning |
|---------|-----------|---------|---------|
| RACI | Responsible, Accountable, Consulted, Informed | Responsable, Aprobador, Consultado, Informado | Matrix defining responsibilities per activity |
| PM | Project Manager | Gerente de Proyecto | Coordinates and manages a project |
| SA | Solution Architect | Arquitecto de Soluciones | Designs the technical solution |
| SME | Subject Matter Expert | Experto en la Materia | Deep specialist in a specific topic |
| UAT | User Acceptance Testing | Pruebas de Aceptación de Usuario | End-user validation of a solution |
| MVP | Minimum Viable Product | Producto Mínimo Viable | First functional version delivering value |

**RACI — DO / DON'T** *(wired to RULE_004 Determine_Ownership)*

- **DO:** exactly one **A** per activity; at least one **R**; **C** only where expertise is required; **I** kept short.

| Activity | A | R | C | I |
|----------|---|---|---|---|
| API contract design | Architect | Implementer | Security Guardian | Team |

- **DON'T:** "Ownership: everyone" (that is ownership: no one), or two A's on one activity (shared accountability = no accountability).

**MVP — DO / DON'T**

- **DO:** ship the *smallest complete slice* that delivers real value end-to-end (one search flow working in production).
- **DON'T:** build 40% of every feature. Ten half-built features is not an MVP; it is ten open loops (Ley del Entierro).

**UAT — DO / DON'T**

- **DO:** have the *user* validate against written acceptance criteria before production sign-off.
- **DON'T:** let the developer self-certify. Passing unit tests is verification; UAT is acceptance — different gates.

---

## Cloud

| Acronym | Full name | Español | Meaning |
|---------|-----------|---------|---------|
| IaaS | Infrastructure as a Service | Infraestructura como Servicio | Client manages OS and applications; provider manages hardware |
| PaaS | Platform as a Service | Plataforma como Servicio | Provider manages infrastructure; client deploys apps |
| SaaS | Software as a Service | Software como Servicio | Application consumed directly as a service |
| VNet | Virtual Network | Red Virtual | Private network inside a cloud platform |
| NSG | Network Security Group | Grupo de Seguridad de Red | Rule set controlling network traffic |
| DR | Disaster Recovery | Recuperación ante Desastres | Strategy to restore services after major failure |
| BCDR | Business Continuity and Disaster Recovery | Continuidad del Negocio y Recuperación ante Desastres | End-to-end strategy to keep operating through disruptions |

**IaaS / PaaS / SaaS — DO / DON'T**

- **DO:** pick by *who you want managing what*: AKS node pools = IaaS-leaning; Azure App Service = PaaS; the DnDApp offered to end users = SaaS.
- **DON'T:** default to IaaS "for control" — you inherit patching, scaling and hardening. Control is a cost, not a free feature.

---

## Artificial Intelligence

| Acronym | Full name | Español | Meaning |
|---------|-----------|---------|---------|
| AI | Artificial Intelligence | Inteligencia Artificial | Systems performing tasks associated with human intelligence |
| GenAI | Generative AI | IA Generativa | AI capable of generating new content |
| LLM | Large Language Model | Modelo de Lenguaje Grande | Model trained to understand and generate language |
| RAG | Retrieval Augmented Generation | Generación Aumentada por Recuperación | Architecture combining an LLM with external knowledge retrieval |
| MCP | Model Context Protocol | Protocolo de Contexto para Modelos | Standard connecting agents to tools and systems |
| A2A | Agent To Agent | Agente a Agente | Communication between multiple intelligent agents |
| NLP | Natural Language Processing | Procesamiento de Lenguaje Natural | Understanding and generating human language |
| NLU | Natural Language Understanding | Comprensión del Lenguaje Natural | Interpreting intent and meaning |
| ML | Machine Learning | Aprendizaje Automático | AI branch based on training from data |

**RAG — DO / DON'T** *(this is your unified-search + vector oracle architecture)*

- **DO:** retrieve relevant chunks (Pinecone / Azure AI Search), ground the LLM answer on them, and cite the source. Knowledge that changes lives in the index, not in the model.
- **DON'T:** fine-tune a model to "teach" it volatile content, or let the LLM answer from memory when an index exists (RULE_014: evidence over recall).

**MCP — DO / DON'T**

- **DO:** expose capabilities as MCP servers (engram, skullrender-agents) so any client — Cursor, Claude Code, VS Code — reuses them.
- **DON'T:** hardcode per-agent integrations. One tool wired N times is N maintenance loops.

---

## Roles

| Acronym | Full name | Español | Meaning |
|---------|-----------|---------|---------|
| CEO | Chief Executive Officer | Director General | Leads the organization |
| CTO | Chief Technology Officer | Director de Tecnología | Owns technology strategy |
| COO | Chief Operating Officer | Director de Operaciones | Owns operational execution |
| CISO | Chief Information Security Officer | Director de Seguridad de la Información | Owns security strategy |

---

## Security

| Acronym | Full name | Español | Meaning |
|---------|-----------|---------|---------|
| MFA | Multi Factor Authentication | Autenticación Multifactor | Authentication using multiple evidence factors |
| IAM | Identity And Access Management | Gestión de Identidades y Accesos | Control of users, permissions and authentication |
| RBAC | Role Based Access Control | Control de Acceso Basado en Roles | Permissions assigned by function |
| TLS | Transport Layer Security | Seguridad de la Capa de Transporte | Protocol protecting communications |
| VPN | Virtual Private Network | Red Privada Virtual | Secure channel between networks |

**RBAC — DO / DON'T** *(wired to non-negotiables: authz on every request)*

- **DO:** check the role **server-side on every request**, object-level when applicable (can *this* user edit *this* campaign?).
- **DON'T:** trust that the UI hid the button. Hidden ≠ forbidden — that is exactly OWASP BOLA/BFLA territory.

---

## Productivity

| Acronym | Full name | Español | Meaning |
|---------|-----------|---------|---------|
| OOF | Out Of Office | Fuera de Oficina | Temporary absence status |
| PTO | Paid Time Off | Tiempo Libre Remunerado | Paid vacation or leave |
| SOP | Standard Operating Procedure | Procedimiento Operativo Estándar | Documented instruction for executing a task |
| KB | Knowledge Base | Base de Conocimiento | Repository of documentation and learnings |

---

## Repo canon *(additions — vocabulary this repo uses daily)*

| Acronym | Full name | Español | Meaning |
|---------|-----------|---------|---------|
| ADR | Architecture Decision Record | Registro de Decisión de Arquitectura | Short doc freezing one architectural decision: context, options, decision, consequences |
| SDD | Spec-Driven Development | Desarrollo Guiado por Especificación | Workflow: proposal → spec → design → tasks → apply → verify → archive |
| TDD | Test-Driven Development | Desarrollo Guiado por Pruebas | RED (failing test) → GREEN (minimum code) → Refactor |
| OpenSpec | — | — | Tooling/convention implementing SDD: `openspec/specs/` + `openspec/changes/` |
| GGA | Gentleman Guardian Angel | — | Local pre-commit AI review tool; rules in `AGENTS.md`, config in `.gga` |
| JD | Judgment Day | Día del Juicio | Adversarial dual-review protocol: two blind reviewers + synthesis + fix + re-judge |
| DDD | Domain-Driven Design | Diseño Guiado por el Dominio | Modeling software around the business domain; domain layer free of framework imports |
| OWASP | Open Worldwide Application Security Project | — | Foundation publishing the Top 10 security risk lists (Web, API, LLM) |
| STRIDE | Spoofing, Tampering, Repudiation, Information disclosure, Denial of service, Elevation of privilege | — | Threat-modeling checklist |
| BOLA | Broken Object Level Authorization | — | OWASP API #1: accessing objects you don't own by changing an ID |
| BFLA | Broken Function Level Authorization | — | Calling admin functions as a regular user |
| PII | Personally Identifiable Information | Información Personal Identificable | Data identifying a person; never in LLM context without sanitization (non-negotiable) |
| SSR | Server-Side Rendering | Renderizado del Lado del Servidor | Rendering Angular pages on the server before sending HTML |
| CI/CD | Continuous Integration / Continuous Delivery | Integración y Entrega Continua | Automated build-test-deploy pipeline |
| AKS | Azure Kubernetes Service | — | Managed Kubernetes on Azure (deploy target) |
| GHCR | GitHub Container Registry | — | Registry storing the Docker images CI builds |
| OIDC | OpenID Connect | — | Identity layer on OAuth 2.0; CI→AKS auth without stored secrets |
| ISO 42010 | — | — | Standard: architecture descriptions must identify stakeholders and concerns |
| ISO 25010 | — | — | Standard: software quality attributes model (performance, security, maintainability…) |

**ADR — DO / DON'T** *(wired to RULE_014: state the why and the risk)*

- **DO:** record context, options with tradeoffs, the decision, and its consequences at the moment of deciding. Half a page is enough.
- **DON'T:** bury decisions in chat history or commit messages. "Why is this a modular monolith?" must be answerable without archaeology.

**JD — DO / DON'T**

- **DO:** run two *blind* parallel reviews, synthesize Confirmed / Suspect / Contradiction, fix confirmed issues, re-judge until APPROVED.
- **DON'T:** let one reviewer see the other's verdict before writing their own — that collapses the dual review into one biased review.
