# Planning large tasks

Use this workflow when a task spans multiple stories, system areas, or reviewable commits.

1. Read `IDEA.md`, `USER_STORIES.md`, `BACKLOG.md`, and relevant sections of `ARCHITECTURE.md`.
2. State the user outcome, scope, non-goals, acceptance criteria, dependencies, security or data risks, and decisions needed.
3. Identify decisions that block implementation. Resolve them with the user; record non-blocking research separately.
4. Define affected interfaces, data migrations, key tests and docs, and rollout or rollback steps.
5. Break work into ordered, independently reviewable tasks. Prefer vertical slices that include behavior, tests, and documentation.
6. Delegate bounded tasks with clear inputs, outputs, and file ownership. Use separate Git worktrees when agents will edit concurrently; if sharing one checkout, assign disjoint files and coordinate first.
7. Keep backlog IDs and story references traceable. Update `BACKLOG.md` when scope, dependencies, or acceptance criteria change.
8. Implement on a feature branch, follow TDD, and record meaningful tool use, decisions, and verification in `CODEX_LOG.md`.
9. Open a PR and request Codex review. Never merge without the user's explicit request.

Do not claim a plan is complete while blocking decisions or required acceptance criteria remain unresolved.
