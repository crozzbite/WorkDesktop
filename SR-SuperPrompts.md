# SR-SuperPrompts

Kit de prompts ordenados para llevar un proyecto de **cero hasta entrega**, con **énfasis en arquitectura**, alineado al **modelo C4** (vistas por zoom), **ADRs**, y **consideraciones de seguridad estilo OWASP**. Pensado para usarse junto al **dual Lich / Gentleman**, las **reglas de SkullRender**, y **OpenSpec / SDD** (`/sdd-*`): el chat ayuda a pensar y redactar; los artefactos versionados ganan ante divergencias.

**Ubicación:** raíz de `WorkDesktop` (fuera de carpetas de producto/repo anidados).

---

## Cómo usar esto bien (disciplina mínima)

1. **Orden** — Ejecutá los prompts **del 1 al 25** salvo que marques explícitamente **N/A** y el motivo.
2. **Uno por sesión corta** — Mejor que un dump gigante de 25 prompts: **un número → resultado → artefacto** antes del siguiente.
3. **Dos voces SkullRender**  
   - **Lich (@Lich):** alcance canon, límites de sistema, seguridad alta, ADRs, vistas C4, tradeoffs irreversibles.  
   - **Gentleman (@Gentleman):** bajar resultado a formato de cambio (`/sdd-*`), gates, checklist de verificación, convenciones de repo.
4. **OpenSpec gana después de congelar fase** — Lo que escribís en specs/design **reemplaza** notas del chat en conflicto.
5. **Traspaso explícito** — Tras cada bloque importante, pegá la salida donde corresponda (proposal, delta spec, design, ADR, tasks).
6. **OWASP no es decoración** — Cada hallazgo debería terminar en **control + cómo verificarlo** (test, revisión, checklist), no solo en texto.
7. **Skills vía MCP (SkullRender)** — Cada bloque numerado incluye una línea **MCP SkullRender**: el asistente debe usar el servidor **`user-skullrender-skills`** y llamar **`skills_get`** con los nombres indicados **antes** de redactar la respuesta (orden sugerido = orden de la lista). Si no está claro qué skill aplica, usar antes **`skills_search`** con el tema del prompt. Si el MCP no está disponible en el entorno, declararlo en una línea y seguir con canon `docs/` + reglas del workspace.
8. **Stack opcional** — Si el proyecto es **Angular** (SkullRender), añadí manualmente al mensaje: además `angular-architecture` o `angular-core` según el prompt (C4/componentes/forms). Para **Python API** puede añadirse `django-drf` o `pytest` en prompts de entrega/verificación según aplique.

### Enlace con SDD (referencia rápida)

| Zona de prompts | Comando SDD típico | Qué materializar |
|-----------------|--------------------|------------------|
| 1–7 | `/sdd-propose`, `/sdd-spec` | Intención, alcance, requisitos, escenarios |
| 8–16 | `/sdd-design` | C4, contenedores, componentes críticos, contratos |
| 17–22 | Spec/design + criterios de `/sdd-verify` | OWASP aplicado, privacidad, ops, gate de revisión |
| 23–25 | `/sdd-tasks`, convenciones de implementación | Plan por slices, DoD, reglas cortas de código seguro |

### Skills MCP por prompt (`user-skullrender-skills` → `skills_get`)

Referencia única (cada prompt repetido abajo trae la misma lista en línea para copiar/pegar aislado).

| # | `skills_get` (orden sugerido) |
|---|------------------------------|
| 1 | `saas-product-manager`, `openspec`, `pensamiento-socratico` |
| 2–7 | `openspec`, `pensamiento-socratico` |
| 8 | `uc-patterns`, `openspec` |
| 9 | `owasp-asvs`, `app-security` |
| 10–11 | `uc-patterns`, `openspec` |
| 12 | `phylactery-lich`, `openspec`, `pensamiento-socratico` |
| 13–15 | `phylactery-lich`, `openspec` |
| 16 | `phylactery-lich`, `uc-patterns` |
| 17 | `owasp-asvs`, `app-security` |
| 18 | `gdpr-compliance`, `app-security` |
| 19 | `uc-patterns`, `openspec` |
| 20 | `uc-patterns`, `sentry`, `openspec` |
| 21 | `app-security`, `owasp-asvs`, `owasp-llm` |
| 22 | `openspec-verify-change`, `app-security`, `phylactery-lich` |
| 23 | `openspec`, `github-pr`, `openspec-apply-change` |
| 24 | `openspec-verify-change`, `app-security`, `playwright` |
| 25 | `app-security`, `clean-code`, `owasp-asvs` |

---

## Referencia corta: C4, ADR, OWASP

### Modelo C4 (zoom arquitectónico)

No depende de un “paper” único; es un enfoque popularizado por **Simon Brown** para documentar en capas: **Context** → **Container** → **Component** → **Code** (este último solo donde aporta). Sitio de referencia: [c4model.com](https://c4model.com/).

### ADR

**ADR** = **Architecture Decision Record** (registro de decisión de arquitectura). Documenta contexto, decisión, alternativas y consecuencias para decisiones que cuestan cambiar después.

### OWASP

Orientación práctica: **amenazas y controles** (p.ej. **OWASP Top 10** aplicado **a tu** diseño). Objetivo: que cada prompt de seguridad cierre en **superficie de ataque**, **control** y **evidencia** de verificación.

---

## Lista numerada — para qué sirve cada prompt

| # | Propósito |
|---|-----------|
| 1 | Charter de producto: problema, usuarios, valor, KPIs, no-objetivos |
| 2 | Restricciones duras vs supuestos etiquetados |
| 3 | Alcance IN/OUT por fase / releases |
| 4 | NFRs priorizados con tensiones resueltas explícitamente |
| 5 | Stakeholders y vetos/decisiones reservadas |
| 6 | Journeys críticos y criterios de aceptación medibles |
| 7 | Modelo de dominio ligero (agregados, invariantes, transacciones vs eventual) |
| 8 | C4 **Context**: externos, zonas de confianza |
| 9 | Threat model inicial OWASP-friendly (trust boundaries + activos) |
| 10 | C4 **Containers**: apps, datos, comunicación,failure modes |
| 11 | Contratos de integración, idempotencia, versionado |
| 12 | Stack inicial mínimo: decidir YA vs diferir |
| 13 | **ADR**: runtime/plataforma/hosting/CI baseline |
| 14 | **ADR**: modelo de datos, fuente de verdad, retención, migraciones |
| 15 | **ADR**: seguridad base AuthN/Z, secretos, sesiones si aplica |
| 16 | C4 **Components** solo en hotspots costosos/de riesgo |
| 17 | OWASP Top 10 mapeado a controles concretos **en esta arquitectura** |
| 18 | Clasificación de datos, minimización de logs/monitoring |
| 19 | Ops: deploy, rollback, backups, dependencias externas con SLA |
| 20 | Observabilidad: SLIs mínimos, trazas/métricas/logs alineados a journeys |
| 21 | Threat model **post‑diseño** (abuso de API, cadena de suministro, etc.) |
| 22 | Gate de revisión: coherencia C4 + NFR + OWASP + coste |
| 23 | Plan de entrega por **slices verticales** end-to-end |
| 24 | DoD del primer incremento (hardening, tests, ADRs obligatorios) |
| 25 | Reglas cortas de implementación segura (patrones permitidos/prohibidos) |

---

## Prompts (copiar y pegar en orden)

### 1 — Charter de producto

Ayudá a escribir un charter de producto: problema, usuarios, propuesta de valor, competidores o alternativas, KPIs de éxito a 3 y 12 meses, y no-objetivos explícitos.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `saas-product-manager`, `openspec`, `pensamiento-socratico`.

### 2 — Restricciones y supuestos

Lista restricciones duras versus preferencias y supuestos; etiqueta cada supuesto como válido hasta evidencia contraria. Incluye compliance y dependencias organizacionales.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `openspec`, `pensamiento-socratico`.

### 3 — Alcance por fases

Definí IN y OUT por release. Para cada ítem fuera de alcance, explicá qué riesgo o coste evita aplazar.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `openspec`, `pensamiento-socratico`.

### 4 — NFRs priorizados

Prioriza NFRs con justificación breve por flujo crítico. Si hay tensiones entre consistencia, latencia y coste, decidí política inicial y documentá el tradeoff.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `openspec`, `pensamiento-socratico`.

### 5 — Mapa de stakeholders

Lista stakeholders y qué necesitan saber o decidir sobre arquitectura. Indicá quién puede vetar qué tipo de decisión.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `openspec`, `pensamiento-socratico`.

### 6 — Journeys y criterios

Describí tres a siete user journeys end-to-end. Para cada uno: entrada, camino feliz, fallos esperados y criterios de aceptación medibles.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `openspec`, `pensamiento-socratico`.

### 7 — Modelo de dominio (ligero)

Extraé conceptos de dominio, agregados e invariantes. Marcá límites transaccionales y qué puede ser eventual sin violar invariantes críticas.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `openspec`, `pensamiento-socratico`.

### 8 — C4 Context (con trust boundaries)

Describí el nivel Context del modelo C4: actores externos, sistemas colindantes y trust boundaries claras: quién confía en qué y qué datos cruzan cada frontera.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `uc-patterns`, `openspec`.

### 9 — Threat model inicial (OWASP-aligned)

Hacé un threat model inicial breve usando STRIDE solo como guía: top riesgos sobre confidencialidad, integridad y disponibilidad; activos sensibles y controles mínimos en frontera.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `owasp-asvs`, `app-security`.

### 10 — C4 Containers

Describí el nivel Container del modelo C4: cada contenedor, su responsabilidad, datos que posee, comunicación síncrona o asíncrona, y failure modes típicos.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `uc-patterns`, `openspec`.

### 11 — Contratos e integraciones

Definí contratos externos e internos: versionado de esquemas, modelo de errores, idempotencia, reintentos con backoff y límites de tasa conocidos de terceros.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `uc-patterns`, `openspec`.

### 12 — Decisión tecnológica mínima

Proponé un stack inicial solo para arrancar: qué decidimos ya y qué dejamos explícitamente diferido con el criterio para reabrirlo.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `phylactery-lich`, `openspec`, `pensamiento-socratico`.

### 13 — ADR: runtime / plataforma

Escribí un ADR sobre elección de runtime, plataforma de despliegue y pipeline CI base. Incluye contexto, decisión, opciones evaluadas, consecuencias y criterios de reversión.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `phylactery-lich`, `openspec`.

### 14 — ADR: modelo de datos

Escribí un ADR sobre persistencia, consistencia, retención de datos y estrategia de migraciones.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `phylactery-lich`, `openspec`.

### 15 — ADR: seguridad base (AuthN / AuthZ / secretos)

Escribí un ADR de seguridad base: modelo de identidad y autorización, gestión y rotación de secretos, MFA si aplica, y uso de sesiones o tokens según corresponda.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `phylactery-lich`, `openspec`.

### 16 — C4 Components (solo hotspots)

Profundizá el nivel Component del C4 únicamente en contenedores de alto riesgo o coste. Evitá diagramar todo el sistema en detalle superficial.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `phylactery-lich`, `uc-patterns`.

### 17 — OWASP Top 10 aplicado (no genérico)

Mapeá el OWASP Top 10 a elementos concretos de esta arquitectura: para cada riesgo, control propuesto, responsable sugerido y evidencia de verificación esperada.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `owasp-asvs`, `app-security`.

### 18 — Privacidad y datagrade

Clasificación de datos, retención, minimización de recolección, y qué puede loguearse o monitorearse sin filtraciones de PII o secretos.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `gdpr-compliance`, `app-security`.

### 19 — Operación y resiliencia

Diseño operativo: despliegue y rollback, configuración por entorno, backups y restores con objetivos razonables, y dependencias externas con SLA e impacto si fallan.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `uc-patterns`, `openspec`.

### 20 — Observabilidad

Lista SLIs por journey crítico e instrumentación mínima de trazas, métricas y logs alineadas a esos SLIs; propone alertas iniciales con umbrales y anti-ruido.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `uc-patterns`, `sentry`, `openspec`.

### 21 — Threat model post-arquitectura

Revisá el threat model tras definir contenedores y contratos: nuevas superficies de ataque, abuso de APIs, control de acceso roto, y riesgos de cadena de suministro; mitigaciones y pruebas previstas.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `app-security`, `owasp-asvs`, `owasp-llm`.

### 22 — Gate de revisión (C4 + NFR + OWASP)

Auditá inconsistencias entre diagramas, decisiones, riesgos, NFRs y presupuesto operativo; entregá una lista cerrada de acciones antes de implementar código.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `openspec-verify-change`, `app-security`, `phylactery-lich`.

### 23 — Plan de entrega por slices verticales

Armá milestones por slices verticales end-to-end con dependencias técnicas y riesgos explícitos, evitando entregar solo por capas aisladas.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `openspec`, `github-pr`, `openspec-apply-change`.

### 24 — Definition of Done del primer incremento

Definí DoD técnico: hardening baseline alineado a OWASP donde aplique, tests mínimos de flujos críticos y ADRs requeridos para decisiones grandes.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `openspec-verify-change`, `app-security`, `playwright`.

### 25 — Reglas cortas para implementación segura

Lista patrones permitidos y prohibidos desde la perspectiva OWASP-aligned: entrada y validación, autorización por defecto deny, gestión de dependencias y secretos, y logging seguro.

**MCP SkullRender:** En `user-skullrender-skills`, llamar `skills_get` antes de responder, en orden: `app-security`, `clean-code`, `owasp-asvs`.

---

## Notas finales

- Si un paso **no aplica**, documentá una línea **N/A + razón** y seguí; así el hilo auditoría conserva consistencia.
- Para proyectos muy pequeños, podés fusionar pasos relacionados (**8–10**, **21–22**), pero **no** elimines el espíritu de **trust boundary + verificación**.
- Mantener **inglés** en identificadores, commits y documentación dentro del código repos según política SkullRender; este archivo puede vivir como guía interna del equipo en el idioma que elijan para operar.
