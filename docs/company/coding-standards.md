# Enterprise Coding Standards (placeholder)

> Replace with your company's stack and style decisions.

## Package manager

- **Per repository:** document one manager (npm, pnpm, yarn, bun, pip/uv, etc.)
- Do not mix managers in the same repo

## Languages

| Stack | Standard |
|-------|----------|
| TypeScript | strict mode, see `.github/instructions/typescript.instructions.md` |
| Python | type hints, linter per team choice |
| Angular | 19+ Standalone, Signals, OnPush (if used) |

## Commits & PRs

- Commit messages: English, imperative mood
- PRs: tests pass, linked to change/tasks when using structured delivery

## Review gates

- Adversarial review before archive (see Rule 09)
- Security review when touching auth, secrets, APIs, dependencies, deploy
