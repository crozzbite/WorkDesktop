# Structured delivery — Explore phase

Use this prompt **before** implementing a feature or design change.

## Objective

Explore scope, risks, and alignment with governance. Do **not** write production code in this phase.

## Steps

1. Read `docs/governance/refined-rules/08-workflow-refined.md` STOP conditions.
2. Identify stakeholders and quality attributes (ISO 42010 / ISO 25010).
3. Ask Socratic questions: why is this needed? what breaks if we don't do it?
4. Propose change folder structure: `changes/<name>/` with planned artifacts.
5. List top 3 risks and whether adversarial review will be needed (Rule 09 Trigger A).

## Output

- Problem statement (1 paragraph)
- Proposed change name and folder
- Draft outline for proposal.md
- Go / no-go recommendation for design phase

Wait for human confirmation before creating spec artifacts.
