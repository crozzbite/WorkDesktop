---
version: 1.0
current: true
description: Cognitive layer + LangSmith observability. Refined Rule 05.
---

# Rule 05 Refined: The Cognitive Layer

**Version:** 1.0 ✓ (current)

---

## NEVER / FORBIDDEN

- **NEVER** call OpenAI/Anthropic (or other LLM providers) directly from feature or UI code. All calls MUST go through the central **LLM Gateway / LLMService**.
- **NEVER** put unversioned prompts in random files. Prompts are code: live in `prompts/` or a registry and are versioned.
- **NEVER** send PII to the LLM without sanitization or policy. Use DLP/heuristics before context is built.
- **FORBIDDEN:** Using LLM output as trusted input for SQL, HTML, or shell. Always validate and escape (treat as untrusted).

---

## DO

- **Gateway:** Single entry point for all LLM calls (observability, cost control, fallback, rate limiting).
- **Structured output:** Use JSON mode (or equivalent) for programmatic use; free text only for chat UX.
- **RAG flow:** Ingest → Chunk → Embed → Vector DB; Query → Embed → Search → Context + Query → LLM → Answer. Use Pinecone or Chroma; embeddings e.g. `text-embedding-3-small`. Prefer **hybrid search** (keyword + semantic): combine keyword/full-text search (e.g. BM25, classic search) with vector/semantic search so retrieval uses both exact terms and contextual meaning; merge or rerank results for better recall.
- **Agents:** LLM + tools + loop. Prefer **LangGraph** (Python) for multi-step state machines; Vercel AI SDK (TS) for frontend streaming when appropriate.
- **Memory:** Short-term = context window; long-term = vector DB; procedural = tools/skills.
- **Observability – LangSmith:** Use **LangSmith** for agent/LLM observability when using LangGraph or LangChain. It provides tracing of agent flows, tool calls, and LLM calls with minimal instrumentation.

---

## LangSmith (observability for agents)

- **Purpose:** Trace LangGraph (and LangChain) runs end-to-end: inputs, LLM calls, tool calls, outputs. Debug and evaluate agent behavior.
- **Setup (minimal):**  
  `LANGSMITH_TRACING=true`  
  `LANGSMITH_API_KEY=<your-key>`  
  Optional: `LANGSMITH_PROJECT` for project name.  
  In serverless: `LANGCHAIN_CALLBACKS_BACKGROUND=false` so traces flush before exit.
- **With LangChain:** Tracing is automatic once env is set.
- **Without LangChain:** Use `@traceable` (Python) or `traceable` (JS) and wrap the OpenAI (or other) client so calls are traced.
- **Note:** LangSmith is part of the LangChain ecosystem; we should adopt it for agent-heavy projects (e.g. Phylactery). Complement with OpenTelemetry for system-wide observability (Rule 03).

---

## AI Security (OWASP LLM)

- **Prompt injection:** Validate inputs; use delimiters for user content; heuristic checks (e.g. “ignore previous instructions”).
- **Insecure output:** Validate and escape LLM output before rendering or executing.
- **Sensitive disclosure:** DLP/sanitization before sending data to the model; no PII in prompts without policy.
