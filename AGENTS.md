# Repository instructions

## Read the relevant project docs

- Product scope and constraints: `IDEA.md`, `USER_STORIES.md`, `BACKLOG.md`.
- Local setup and tooling: `DEVELOPMENT.md`.
- Current system design: `ARCHITECTURE.md`.
- Record meaningful Codex actions, decisions, and verification in `CODEX_LOG.md` without secrets or private user data.
- Large task planning: `Plans.md`. Architecture changes: `guides/Architecture.md`. Code reviews: `Code_review.md`.

## Commands

- Use the Node version pinned in `.nvmrc`; install locked dependencies with `npm ci` once the app scaffold exists.
- Run `git diff --check` and `npx --yes markdownlint-cli2@0.23.3 "**/*.md"` for documentation changes.
- After the app scaffold exists, use its npm scripts for lint, typecheck, tests, and build. Follow `DEVELOPMENT.md` and run relevant tests.
- Docker Engine and Compose run in Ubuntu WSL. From PowerShell, prefix Docker commands with `wsl -d Ubuntu --`.

## Git and implementation rules

- Work on feature branches; make focused commits and push them for review.
- Open a PR and request Codex review. Address findings or document why they are not acted on.
- Merge to `main` only when the user explicitly requests it.
- For implementation, use TDD: add a failing test, implement, then run relevant tests. Report tests that could not run and why.
- Keep the single-instance SQLite and local-media architecture unless an architecture change is proposed and reviewed.
- Never commit secrets, real user data, or private story content. Never use production data in tests or CI.
