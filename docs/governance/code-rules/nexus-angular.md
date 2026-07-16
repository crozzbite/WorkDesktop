---
version: 1.0
current: true
description: Nexus Architecture + Clean/DDD en Angular. Path aliases, tokens, providers por capa, use cases, conectores. Origen DnDApp.
---

# Code Rules: Nexus Angular (Clean + DDD)

**Version:** 1.0 ✓ (current)  
**Origen:** Buenas prácticas extraídas del repo DnDApp (tasks 1.0–4.2). Aplicar en proyectos que sigan Nexus Architecture.

---

## DO (Nexus / Clean)

- **Path aliases:** Configurar en `tsconfig.json`: `@domain/*`, `@core/*`, `@shared/*`, `@features/*`, `@assets/*`. Dominio e infraestructura se importan por alias; el dominio no importa rutas de `core/infrastructure`.
- **Contratos en dominio:** Interfaces de repositorios y servicios en `domain/repositories/`, `domain/services/`; implementaciones en `core/infrastructure/` o `core/services/`. Inyección por **InjectionToken** (ej. `REPO_TOKEN`, `AUTH_CRYPT_SERVICE_TOKEN`).
- **app.config:** Separar providers por capa: `repositoryProviders`, `securityProviders` (y opcionalmente `coreProviders`). Usar `provideHttpClient(withFetch(), withInterceptors([]))` desde el inicio para poder añadir interceptores sin cambiar la firma.
- **Use cases:** Un archivo por caso de uso; inyectar repos/servicios por token; exponer un método `execute(...)`. Sin lógica de UI en el use case.
- **Conectores HTTP:** Base abstracta con TTL (sliding), retry con backoff, `shareReplay({ bufferSize: 1, refCount: true })`; hook `getHeaders()` para firma/identidad. URL sin doble barra: `baseUrl.replace(/\/$/, '')`, `endpoint.replace(/^\//, '')`.
- **Persistencia local (ej. sesión):** Payload versionado `{ version, data }`; merge con campos de identidad fijados al final (id, createdAt, updatedAt) para que el cliente no sobrescriba; `deleteSession` en el contrato; key centralizada con `key(id)`.

---

## NEVER (Nexus)

- **NEVER** hacer que el dominio importe Angular, HttpClient o rutas de infraestructura. El dominio solo contiene interfaces, modelos y (si aplica) excepciones.
- **NEVER** inyectar implementaciones concretas en use cases; siempre inyectar por token de interfaz.
- **NEVER** omitir `withInterceptors([])` en `provideHttpClient` si el proyecto tendrá interceptores de seguridad; dejarlo preparado desde Fase 1–2.

---

## Referencias

- **Workflow:** `WorkSpace/.agents/workflows/nexus-build-from-tasks.md` (checklist Fases 1–7).
- **Snippets:** `WorkSpace/.agents/snippets/nexus/` (tsconfig.paths, repository.interface, use-case, app.config.armor, base-connector); `snippets/defense/path-normalize.snippet.ts`.
- **Skill Defense:** `WorkSpace/.agents/skills/armor-defense-patterns/SKILL.md`.
