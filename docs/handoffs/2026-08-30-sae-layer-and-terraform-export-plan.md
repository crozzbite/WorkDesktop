# SAE layer + Terraform export accelerator (plan)

> **Status:** PLAN (authoring). Not Capa A SoT (Source of Truth / fuente de verdad).
> **Depends on:** [`2026-08-10-scope-b-offices-neutral-plan.md`](2026-08-10-scope-b-offices-neutral-plan.md) — Fases 1-3 DONE.
> **Created:** 2026-08-30
> **Repos in scope:** `crozzbite/SkullRender-Agents`, `crozzbite/office-accelerator`, `crozzbite/WorkDesktop`.

## Vocabulary

| Sigla | Full name | Meaning here |
|-------|-----------|--------------|
| SAEp | Sub Agente Experto Principal | Stage office. Owns the stage handoff to PMO. Ids `Office*` in the neutral scaffold, `Saep*` in the legacy branded tree. |
| SAE | Sub Agente Experto | Expert subagent **under** one SAEp. Reports to its SAEp, never to PMO. **Does not exist yet.** |
| PR | Pull Request | Human gate. Agent prepares, human merges. |
| IaC | Infrastructure as Code | Terraform in Fase C. |
| MCP | Model Context Protocol | `skflow_*` tools exposed by `SkullRender-Agents`. |

## Problem

The Scope B scaffold emits **10 SAEp manifests only** (`OfficeFacade`, `OfficePmo`, plus 8 stage
offices). The SAE layer is anticipated but unimplemented:

- `office-accelerator/src/scaffold.ts` has `buildFacade`, `buildPmo`, `buildStage`. No `buildSae`.
- `cookbooks/sdlc-8-stages.yaml` declares `stages:` only. No SAE catalog.
- Every emitted SAEp carries the placeholder line
  `"Keep Saes under this office reporting here (if any)."` — the `(if any)` is currently always *none*.
- `SkullRender-Agents` `AgentsManager` loads a flat id map. Nothing validates the
  SAE → SAEp → PMO reporting chain, so a malformed SAE would load silently.

Good news: the versioned contract is already SAE-ready.
`office-accelerator/schemas/identity.schema.json` (`$id: office-accelerator://schemas/identity/v1`)
already allows `"office": "sae"`. **No schema version bump is required** for Fase B.

## Current state verified 2026-08-30

| Repo | Branch | vs origin | Tests | Uncommitted |
|------|--------|-----------|-------|-------------|
| `SkullRender-Agents` | `master` @ `e454ff1` | in sync | 10 pass / 0 fail | yes — see Fase A |
| `office-accelerator` | `master` @ `fb7cb30` | in sync | 7 pass / 0 fail | yes — see Fase A |

Tooling on this authoring PC: `gh` 2.91.0 authenticated as `crozzbite`
(scopes `gist, read:org, repo, workflow, write:packages`). **Terraform is not installed.**

---

## Decisions frozen 2026-08-30

| # | Decision | Value |
|---|----------|-------|
| D1 | PR granularity | **Bundled.** One PR per repo carrying Fase A + Fase B together. Fase A does not ship early. |
| D2 | SAE catalog | **Full 15-SAE catalog** as proposed in B0. |
| D3 | SAE id shape | `{id_prefix}Sae{Suffix}` (e.g. `OfficeSaeContracts`) — honours `id_prefix` and keeps the existing `Office*` smoke gate valid. |
| D4 | `yaml-lite` | **Option B** — extend `parseSimpleYaml` to two-level map-of-lists, parser test first. |
| D5 | Terraform location | New branch on `WorkDesktop`: **`accelerator/terraform-github-governance`**, cut from `master`. Not `governance/copilot-portable`. |

## Fase A — Pending WIP (ships inside the Fase B PRs, per D1)

Work already written and green on disk. Unrelated to the SAE layer, but bundled by decision D1.

### A1 — `SkullRender-Agents`: pack-free tool advertisement

Closes the plan requirement *"Pack-free = contrato, no esperanza"*.

| File | Change |
|------|--------|
| `src/mcp-tools.ts` | **new** — `listMcpTools()` builds the tool list; `skflow_packs_*` advertised **only** when `packs/` holds YAML |
| `src/mcp-tools.test.ts` | **new** — 2 tests: omits packs tools without `packs/`, advertises them with |
| `src/mcp-server.ts` | drops the 77-line hardcoded tool array, imports `listMcpTools` |
| `ARCHITECTURE.md`, `README.md`, `.gitignore` | doc + hygiene |
| `LICENSE` | untracked, add |

### A2 — `office-accelerator`: non-portable path guard

Closes the plan requirement *"Path policy (Fase 2 + 4) — hard"*.

| File | Change |
|------|--------|
| `src/scaffold.ts` | `posixRel()` — throws on absolute or `Users/` paths; `scaffold-meta.json` now stores paths relative to `outDir` |
| `tests/scaffold.test.ts` | test: meta lists POSIX paths relative to `outDir`, no machine users |
| `scripts/smoke-offices.ps1` | absolute-path grep widened from `manifests/*.yaml` to the whole SKFLOW tree (`yaml/json/md/txt/mdc`) — catches `scaffold-meta.json` |
| `dist/legion-neutral/scaffold-meta.json` | regenerated with relative paths |
| `README.md`, `.gitignore`, `LICENSE` | doc + hygiene |

### Fase A exit criteria

- [ ] `bun test` green in both repos (currently 10/10 and 7/7)
- [ ] `scripts/smoke-offices.ps1` PASS against `dist/legion-neutral`
- [ ] Zero `C:\Users` in tracked files of either repo

---

## Fase B — Implement the SAE layer

### B0 — The SAE catalog *(decided: full catalog, D2)*

Ids follow D3, `{id_prefix}Sae{Suffix}`, so with the default prefix `Office` the
`OfficeArchitecture` roster is `OfficeSaeContracts` and `OfficeSaeDataModel`.

| SAEp (stage) | Proposed SAE |
|--------------|--------------|
| `OfficeScope` | `SaeResearch`, `SaeRequirements` |
| `OfficeArchitecture` | `SaeContracts`, `SaeDataModel` |
| `OfficeExperience` | `SaeUx`, `SaeAccessibility` |
| `OfficeEngineering` | `SaeBackend`, `SaeFrontend` |
| `OfficeQuality` | `SaeTestStrategy`, `SaeSecurityReview` |
| `OfficeDeploy` | `SaePipeline`, `SaeIaC` |
| `OfficeProduction` | `SaeObservability`, `SaeIncident` |
| `OfficeImprove` | `SaeAutomation` |

**Accepted cost:** 15 SAEs on top of 10 SAEp means 25 manifests in `skflow_agents_list`, which is
context the PMO pays for on every resolve. Mitigated by the SAEs living in an opt-in cookbook, so
the 10-office formula stays available for cheap runs.

### B1 — Cookbook carries the catalog (additive, non-breaking)

Keep `sdlc-8-stages.yaml` untouched so the shipped `dist/legion-neutral` and its 7 tests stay
valid. Add SAEs as a new opt-in cookbook:

```yaml
# cookbooks/sdlc-8-stages-saes.yaml
id: sdlc-8-stages-saes
description: Facade + PMO + eight SDLC stage offices, each with expert subagents (pack-free)
stages: [scope, architecture, experience, engineering, quality, deploy, production, improve]
saes:
  architecture: [contracts, data_model]
  engineering: [backend, frontend]
```

`saes:` maps a stage key to SAE keys. A stage absent from `saes:` emits no SAE, so the
`(if any)` placeholder finally tells the truth.

**Blocker — verified 2026-08-30: the shape above does NOT parse.** `src/yaml-lite.ts`
`parseSimpleYaml` is documented as *"scalars, lists, one-level maps"* and the code confirms it:
inside a nested block it does `obj[key] = parseValue(rest)`, so a two-level
map-of-lists collapses. A nested key with an empty value becomes `null` and the list items that
follow are pushed onto the **parent's** `items` array. Inline flow lists do not help either —
`parseValue("[contracts, data_model]")` returns the literal string, not an array.

`office-accelerator` deliberately avoids a YAML dependency (`yaml-lite` header: *"no external
yaml dep"*), unlike `SkullRender-Agents`, which uses the `yaml` package. So this is a real fork
in the road:

| Option | Cost | Risk |
|--------|------|------|
| **A. Flatten** to top-level `saes_architecture:` block lists | zero parser change | awkward cookbook schema; stage key encoded in the field name |
| **B. Extend `parseSimpleYaml`** to two-level map-of-lists | ~20 lines + tests | regression surface across the 7 existing scaffold tests |
| **C. Adopt the `yaml` package** | smallest code | drops the deliberate zero-dependency stance |

**Decided: B** (D4), guarded by a parser-level unit test written before the change. The nested
shape is the one a human reads correctly, and the parser will need it again.

### B2 — `scaffold.ts` emits SAEs

- `SAE_META: Record<string, { suffix, display, summary, parent_stage }>` beside `STAGE_META`.
- `buildSae(prefix, saeKey, stageKey, mcp, skills)` returning `office: "sae"`,
  `reports_to: officeId(prefix, STAGE_META[stageKey].suffix)`, `handoff_owner: false`,
  `personality_pack_default: false`.
- SAE permissions are a **subset** of their SAEp's. An SAE must not hold `Task` (no
  re-delegation) — that keeps the tree from growing a third layer by accident.
- Replace the placeholder `must` line on a stage that has SAEs with the explicit roster:
  `Delegate expert work only to: <SAE ids>.`
- Reuse `forbiddenBrandHits` and `posixRel` — SAE files pass the same brand and path scans.

### B3 — Fitness functions (tests, in `tests/scaffold.test.ts`)

1. `sdlc-8-stages` still emits exactly 10 manifests and zero `office: sae` — no regression.
2. `sdlc-8-stages-saes` emits `10 + N`; every `office: sae` id is in the expected set.
3. **No orphans:** every SAE `reports_to` resolves to an emitted SAEp id.
4. **No second PMO:** no SAE reports to `OfficePmo` or to another SAE.
5. **No re-delegation:** no SAE has `Task` in `permissions.tools`.
6. `personality_pack_default: false` on every SAE; brand scan clean.
7. Every emitted SAE validates against `schemas/identity.schema.json`.

### B4 — MCP side (`SkullRender-Agents`)

- Extend `AgentsManager` with `officeTree()` returning parent → children, and make `loadAll`
  **fail loud** on an SAE whose `reports_to` is missing. Silent skip is what the adversarial
  review flags as a lying contract.
- `formatList()` renders the hierarchy so the PMO sees which SAEs belong to which SAEp.
- Decide: extend `skflow_agents_list` output, or add `skflow_office_tree`. Adding a tool is a
  cross-repo contract change and needs the consuming rules updated in the same change.
- `resolveIdentityPrompt` for an SAE must include its SAEp boundary, not just its own identity.

### B5 — Ship and smoke

- Regenerate `dist/legion-neutral` only if the catalog is adopted as the default; otherwise ship
  the new cookbook and leave `dist` alone.
- `scripts/smoke-offices.ps1`: assert `Office*` count **and** SAE count, `loadAll` total, plus
  the existing pack-free and absolute-path gates.

### Fase B exit criteria

- [ ] B0 catalog approved by human
- [ ] `yaml-lite` nested-shape question answered with a test
- [ ] All B3 fitness functions green
- [ ] `loadAll` fails loud on an orphan SAE (negative test)
- [x] PR B1 on `office-accelerator`, PR B2 on `SkullRender-Agents`, ordered: accelerator first
- [x] `docs/handoffs/2026-08-10-scope-b-offices-neutral-plan.md` Fase 4 checkbox updated

---

## Fase C — Terraform GitHub governance accelerator

**Chosen scope:** Terraform stamps the *repository and its governance* into any GitHub
organization — repos, branches, protection, labels, and the canonical governance files.
It does **not** provision Azure infrastructure; that is explicitly out of scope.

### C0 — Open decisions *(block C1)*

| Decision | Options | Note |
|----------|---------|------|
| Location | new repo `crozzbite/governance-terraform-accelerator` **vs** `terraform/github-governance/` inside `WorkDesktop` on `master` | A separate repo exports cleanest; a folder is less setup |
| State backend | local `terraform.tfstate` (gitignored) **vs** `azurerm` remote backend | Local for one operator; remote once a second person applies |
| File stamping | GitHub **template repository** + protections in Terraform **vs** `github_repository_file` per governance file | Template is far less Terraform churn; per-file resources prevent drift |

### C1 — Prerequisites

- Terraform is **not installed** here. Install and pin a version
  (`.terraform-version` or `required_version >= 1.9`).
- Provider `integrations/github ~> 6`.
- **Auth is the real risk.** The current `gh` token has `read:org` only. Creating repos and
  rulesets in a company organization needs `admin:org`-class rights, delivered as a **GitHub App
  installation token or an organization fine-grained token**, exported as `GITHUB_TOKEN`.
  Never in `.tfvars`, never committed. On the company machine this must be a company credential,
  consistent with the no-personal-accounts constraint.

### C2 — Module layout

```
terraform/github-governance/
  versions.tf              # required_version, required_providers
  providers.tf             # provider "github" { owner = var.org }  — token from env
  variables.tf             # org, repos, visibility, default_branch, protection, labels
  repos.tf                 # github_repository, github_branch, github_branch_default
  rulesets.tf              # github_repository_ruleset — PR required, no force push
  labels.tf                # github_issue_label
  governance_files.tf      # github_repository_file — AGENTS.md, .github/copilot-instructions.md
  outputs.tf               # clone URLs, applied ruleset ids
  terraform.tfvars.example # no secrets
  README.md                # init / plan / apply / destroy + token setup
  .gitignore               # *.tfstate*, .terraform/, *.auto.tfvars
```

Recommendation: stamp **only** the files that must never drift (`AGENTS.md`,
`.github/copilot-instructions.md`) as `github_repository_file`, and carry the bulk via a template
repository. Managing 40 files as individual resources turns every doc edit into a Terraform apply.

### C3 — Guardrails

- `terraform plan` is agent-preparable. `terraform apply` is the **deploy human gate** — never
  run by an agent.
- `terraform destroy` against a repo with content is unacceptable; set
  `lifecycle { prevent_destroy = true }` on `github_repository`.
- Committed state would leak organization topology. `.gitignore` before the first `init`.
- The Terraform accelerator must never write to `governance/copilot-portable`. That branch is
  fed by cherry-pick only and **never merges** with `master`.

### Fase C exit criteria

- [ ] C0 decisions recorded, as an ADR if the layout is hard to reverse
- [ ] `terraform init` + `terraform validate` + `terraform fmt -check` clean
- [ ] `terraform plan` against a throwaway org or a single test repo, output reviewed
- [ ] Zero secrets and zero `C:\Users` in tracked Terraform files
- [ ] README reproduces the flow from a clean machine with no personal account
- [ ] Human applies

---

## Out of scope

- Personality packs (`PackLich`, `PackGentleman`, `PackCerbero`) — permanently out of the formula.
- Porting `azure-ai-accelerators/GPT-RAG` from Bicep to Terraform.
- Workstation bootstrap via Terraform (Engram, GGA, MCP wiring) — stays documented in
  `docs/SETUP-COMPANY-CONSOLE.md`.
- Merging `master` into `governance/copilot-portable`, in either direction, ever.

## Human gates

Commit, PR, merge and apply. The agent prepares branches, diffs, tests, evidence and
`terraform plan` output. The human crosses every gate.
