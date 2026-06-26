---
version: 1.0
current: true
description: Strict TDD Mode — SDD apply protocol (Safety net → Understand → RED → GREEN → Triangulate → Refactor). Companion to Rule 09.
---

# Strict TDD Mode — SDD Apply Protocol

**Version:** 1.0 ✓ (current) · **Created:** 2026-06-25 (ISO 8601)

> **Parent rule:** `docs/refined-rules/09-agent-loops-refined.md` (when to run, who owns, JD relationship)
> **Operational enforcement (do NOT duplicate in code):** `~/.cursor/skills/sdd-apply/strict-tdd.md`, `~/.cursor/skills/sdd-verify/strict-tdd-verify.md`
> **Phase:** `/sdd-apply` — each task in `tasks.md` runs this cycle when `strict_tdd: true` and a test runner exists

---

## Idea central

TDD **no** se trata de “escribir tests”. Se trata de **diseñar software desde el comportamiento esperado**.

El test define el **contrato**; el código de producción viene **después**. En modo Strict TDD no hay implementación “a ciegas” ni fallback silencioso a Standard Mode.

---

## Cuándo está activo

```
IF strict_tdd: true (openspec/config.yaml, sdd-init, or orchestrator injection)
   AND test runner detected for the project
THEN → Strict TDD Mode on every assigned apply task
ELSE → Standard apply (this protocol does NOT apply)
```

Resolución de `strict_tdd`: ver skill `sdd-init` y Rule 09 §2.

---

## Ciclo obligatorio (por tarea)

Cuando Strict TDD está activo, **cada tarea** del agente sigue **estrictamente** este ciclo:

### 0. Safety net (Red de seguridad)

**Solo si se modifican archivos existentes** (archivos nuevos → `N/A` en evidencia).

1. Correr los tests actuales de los archivos que se van a tocar.
2. Capturar baseline: `{N} tests passing`.
3. Si **alguno falla** → **STOP**. Reportar como **falla preexistente** al orchestrator. **No** seguir tocando código a ciegas ni “arreglar de paso” fallos viejos sin acuerdo.

> La safety net demuestra que no rompiste lo que ya funcionaba antes de empezar el RED.

### 1. Understand (Comprensión)

Antes de escribir **cualquier** test o código nuevo:

1. Leer la **tarea** (`tasks.md`).
2. Leer los **escenarios de aceptación** en spec (son el contrato).
3. Leer el **diseño** (decisiones que limitan el enfoque).
4. Leer **código y tests existentes** (patrones del repo).
5. Elegir **capa de test** adecuada (unit → integration → e2e según capacidades del proyecto).

### 2. RED (Fallo)

1. Escribir **primero** un test que **falle**.
2. El test describe el **comportamiento esperado** derivado del spec — no detalles de implementación.
3. **No** escribir código de producción antes del test.
4. El test debe referenciar código que **aún no existe** o comportamiento **aún no implementado** (garantía de fallo real).

**GATE:** no pasar a GREEN hasta que el test RED esté escrito.

### 3. GREEN (Paso)

1. Escribir el **mínimo** código de producción para que el test pase.
2. “Fake It” es válido aquí (valores hardcodeados) **solo** como paso intermedio hacia Triangulate.
3. **Ejecutar** el test → debe **PASS**.
4. Ejecutar **únicamente** el archivo de test relevante, **no** toda la suite (la suite completa corre en `sdd-verify`).

**GATE:** no pasar a Triangulate/Refactor sin GREEN confirmado por ejecución.

### 4. Triangulate (Triangulación)

**Obligatorio por defecto.** Buscar robustez antes de dar la tarea por cerrada.

1. Añadir casos con **inputs/outputs distintos** (happy path + al menos un edge case).
2. Cubrir **todos los escenarios del spec** aplicables a esta tarea.
3. Si Fake It en GREEN ya no alcanza → **generalizar** a lógica real (ese es el punto).
4. Mínimo: **2 casos** por comportamiento (uno no trivial + uno que ejerce otro camino).

**Skip triangulate** solo si **todas** se cumplen:

- Tarea puramente estructural (config, constante, export de tipo).
- Un solo output posible (sin branching).
- Se anota explícitamente en la tabla de evidencia: `Triangulation skipped: {reason}`.

**GATE:** escenarios del spec cubiertos antes de Refactor.

### 5. Refactor

1. Mejorar código **sin cambiar comportamiento** (nombres, extracción, duplicación, pure functions donde aplique).
2. **Ejecutar tests después de cada paso** de refactor → deben seguir verdes.
3. Si un refactor rompe tests → revertir ese paso; refactor más pequeño.

**Regla Boy Scout:** dejar el código más limpio que como lo encontraste, con tests verdes.

---

## Las tres leyes (no negociables)

1. **No** escribir código de producción hasta tener un test que falle.
2. **No** escribir más test del necesario para fallar.
3. **No** escribir más código del necesario para pasar el test.

---

## Evidencia obligatoria (apply → verify)

Al cerrar apply, el artefacto `apply-progress` **debe** incluir la tabla **TDD Cycle Evidence**:

| Task | Test File | Layer | Safety Net | RED | GREEN | TRIANGULATE | REFACTOR |
|------|-----------|-------|------------|-----|-------|-------------|----------|

`sdd-verify` con `strict-tdd-verify.md` **rechaza** apply sin tabla completa o con pasos falsos (p. ej. GREEN sin ejecución, tests que no existen).

Detalle de calidad de assertions, patrones prohibidos y approval testing para refactors: ver skill `strict-tdd.md` (Secciones *Assertion Quality*, *Approval Testing*).

---

## Relación con Judgment Day

| Strict TDD | Judgment Day |
|------------|--------------|
| Diseña contratos **honestos** durante apply | Pregunta si el contrato **miente** (tests cosméticos, fallos silenciosos) |
| Safety net = no romper lo existente | Trigger F = no borrar tests viejos sin cubrir el contrato falso |
| Post-apply Trigger B (Rule 09) | Post-verify gate obligatorio (Rule 09 §4) |

Orden típico en cambios riesgosos (piloto DnDApp Phase 3.5): **JD diagnóstico → Strict TDD sobre fixes confirmados → JD re-juicio**.

---

## STOP (Strict TDD)

- **STOP** apply task si Safety net encuentra fallas preexistentes — escalar, no parchear en silencio.
- **STOP** declarar tarea completa sin RED escrito primero.
- **STOP** declarar GREEN sin haber **ejecutado** el test.
- **STOP** fallback silencioso a Standard Mode cuando Strict TDD está activo — reportar fallo o bloqueo explícito.

---

*SkullRender canon for Strict TDD. Execution details and language-specific runners live in `~/.cursor/skills/sdd-apply/strict-tdd.md` — this document is the **what and why**; the skill is the **how**.*
