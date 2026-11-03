# Architecture change workflow

Use this guide when a change affects system boundaries, data ownership, persistence, authentication, media handling, deployment, or a shared interface.

1. Read the current `ARCHITECTURE.md`, relevant backlog items, and affected user stories before editing.
2. Describe the proposed change, its reason, and effects on privacy, authorization, migration, operations, and recovery.
3. Check whether the proposal changes product scope. Update `IDEA.md`, `USER_STORIES.md`, or `BACKLOG.md` when behavior or acceptance criteria change.
4. Update `ARCHITECTURE.md` as the canonical design. Keep this workflow guide focused on how to make architecture changes.
5. Include migration and rollback steps for persistent data changes. Identify deployment and backup implications.
6. Add or update tests for data boundaries and failure paths; use TDD for implementation.
7. Record the decision and verification in `CODEX_LOG.md`. Submit the change through a reviewed PR.

Do not silently introduce external runtime services, multiple app instances, public access to private data, or breaking data changes.
