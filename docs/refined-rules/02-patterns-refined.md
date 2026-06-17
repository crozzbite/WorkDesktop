---
version: 1.0
current: true
description: Architectural patterns + when to use each. Refined Rule 02.
---

# Rule 02 Refined: Architectural Patterns

**Version:** 1.0 ✓ (current)

---

## NEVER / FORBIDDEN

- **NEVER** choose Microservices only to “fix” code complexity. Use them for organizational or scaling reasons.
- **NEVER** let modules import each other directly in a Modular Monolith. Use interfaces / contracts.
- **FORBIDDEN:** Starting with Microservices or Serverless before validating product–market fit or team size.

---

## Pattern summary & when to use

| Pattern | When it’s convenient | When to avoid | SkullRender default |
|--------|----------------------|---------------|----------------------|
| **Modular Monolith** | MVP, small team (<5), simple domain, need low latency and simple deploy | When you need independent scaling of components or polyglot teams | ✓ Default. Domains in `modules/`, in-memory calls, strict boundaries. |
| **Microservices** | Multiple teams (>~20), independent scaling per component, polyglot (e.g. Python + Rust) | Early stage, single team, no clear bounded contexts | Only when a module is choking the whole or teams collide. |
| **Event-Driven (EDA)** | Decoupling, fire-and-forget, high volume async, sagas | When you need strong consistency and simple debugging | When the problem clearly needs async events. |
| **Serverless** | Pay-per-use, elastic burst, minimal ops | Predictable heavy load, need for low cold-start latency, strict control over runtime | Optional for specific functions (e.g. webhooks). |
| **CQRS / Event Sourcing** | Read/write scale asymmetry, audit trail, decomposing monolith with events | Simple CRUD; team not ready for eventual consistency | When explicitly needed for scale or compliance. |

---

## Selection criteria (short)

- **Speed / simplicity first** → Modular Monolith.
- **Scale / many teams** → Consider Microservices for specific boundaries.
- **Decoupling / async** → Event-Driven.
- **Cost / elasticity** → Serverless for targeted workloads.
- **Asymmetric read/write or audit** → CQRS / Event Sourcing.

**Bones directive:** Start with a **Modular Monolith**. Extract to Microservices only when a specific module justifies it (performance or governance).
