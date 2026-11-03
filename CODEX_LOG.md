# Codex development log

This log records meaningful Codex-assisted work so the development process is reviewable alongside the prototype. It captures tool categories and decisions, not private prompts, credentials, or user data.

## 2026-11-03 — First product brief

- **Task:** Turn the initial storytelling and translation idea into a prototype brief.
- **Codex tools used:** Inspected project notes with a shell command; edited `IDEA.md` and `AGENTS.md` with a patch tool.
- **Human direction:** Keep rapid prototyping and visible Codex tool use equally important in the deliverable.
- **Decisions recorded:** Frame storytellers, readers, and translators as primary users; outline reader and storyteller workflows; recommend a responsive web app with demo data; defer accounts, durable media storage, and automated translation; document open questions for validation.
- **Review evidence:** See the current diff for `IDEA.md` and `AGENTS.md`. No application code or verification commands were part of this documentation task.

## 2026-11-03 — TDD definition of done

- **Task:** Make test-driven development the standing completion rule for implementation work.
- **Codex tools used:** Updated `AGENTS.md` with a patch tool.
- **Human direction:** Write a failing test first, implement, rerun tests, and iterate until they pass.
- **Decision recorded:** Implementation work is complete only when relevant tests pass; report tests that could not be run and why.
- **Review evidence:** See the current diff for `AGENTS.md`. This documentation-only change does not require running the application test suite.

## 2026-11-03 — Automatic Codex PR review

- **Task:** Require Codex review on every pull request before merge.
- **Codex tools used:** Updated `AGENTS.md` with a patch tool.
- **Human direction:** Codex must review all PRs automatically.
- **Decision recorded:** Review findings should be addressed or have the reason for not acting on them documented.
- **Review evidence:** See the current diff for `AGENTS.md`.

## 2026-11-03 — Initial application architecture

- **Task:** Save the architecture plan for the storytelling platform.
- **Codex tools used:** Inspected the repository and product brief with shell commands; researched current official Next.js, Supabase, Vercel, Vitest, and Playwright documentation; delegated architecture drill-down and adversarial review to Codex subagents; added `ARCHITECTURE.md` with a patch tool.
- **Human decisions:** Next.js App Router with TypeScript, Vercel, Supabase, Google OAuth, creator accounts, owner-managed translations, self-publishing with reporting, 40 MiB upload limit, service-console moderation, signed-in reporting, and in-place edits for v1.
- **Review decisions:** Isolate PR previews and CI from production data and credentials; use private owner-scoped media storage and short-lived published-only URLs; enforce RLS, test authorization allow/deny paths, pin/isolate the currently beta Supabase SSR integration, and assign a maintainer for reports.
- **Review evidence:** `ARCHITECTURE.md` contains the selected design, acceptance tests, assumptions, and direct links to official documentation. The adversarial review was completed before this plan was saved.

## 2026-11-03 — Reduce prototype hosting scope

- **Task:** Revise the prototype architecture for self-hosted Docker deployment.
- **Codex tools used:** Researched official Next.js, Supabase self-hosting, Better Auth, and SQLite documentation; updated `ARCHITECTURE.md`, `IDEA.md`, and this log with a patch tool.
- **Initial direction:** Use one Next.js app container, SQLite, uploaded media on a persistent host volume, and Google OAuth; Google OAuth was later removed at the user's direction.
- **Scope changes:** Remove Vercel, Supabase, Postgres, hosted storage, and PR preview environments. Keep creator publishing and reports; operate moderation with host-only commands. The single-instance design accepts manual backup/restore and no horizontal scaling.
- **Review evidence:** The revised architecture and product brief describe the new self-hosted boundary; documentation diff checked before commit.

## 2026-11-03 — Replace OAuth with local username/password

- **Task:** Remove Google OAuth and keep account access local to the Docker app.
- **Codex tools used:** Checked Better Auth's official username, password, and SQLite documentation; updated `ARCHITECTURE.md`, `IDEA.md`, and this log with a patch tool.
- **Human direction:** Use a username/password login form instead of OAuth.
- **Decision recorded:** Better Auth's username plugin requires an email at signup; use a private, unverified account email while keeping the login form username/password only. No verification or recovery email in v1.
- **Review evidence:** A Codex subagent challenged backup consistency, disk abuse, media range handling, proxy trust, password recovery, brute-force and username enumeration, and non-resumable uploads. The architecture now includes an offline DB/media backup and restore test, upload quotas and temporary-space reservations, `206`/`416` range behavior with no-store responses, uncached story reads, proxy-only access, local password change/reset, database-backed rate limits, private-email omission tests, and explicit retry-on-failure scope. Password reset requires independent out-of-band identity verification; unverified email is not used for recovery. Final adversarial review found no remaining blockers.

## 2026-11-03 — Define v1 user stories

- **Task:** Continue product planning by translating the prototype brief and architecture into user stories.
- **Codex tools used:** Inspected `IDEA.md`, `ARCHITECTURE.md`, and repository history; created a documentation feature branch; delegated story-detail drafting across reader discovery/versioning, creator publishing/translations, and media/accounts/reporting; consolidated and edited the acceptance criteria into `USER_STORIES.md`.
- **Human direction:** Identify story titles first, then fan out agents to populate each story’s detail.
- **Decisions recorded:** The seven stories cover discovery, reading/version switching, publishing/terms, linked translations, media handling, creator accounts, and reports/host unpublishing. Each includes a user need, value, acceptance criteria, and scope or edge cases consistent with the v1 architecture.
- **Review evidence:** Compare `USER_STORIES.md` with the source constraints in `IDEA.md` and `ARCHITECTURE.md`; this planning-only change does not alter application code or require running the application test suite.

## 2026-11-03 — Build complete v1 delivery backlog

- **Task:** Convert the user story catalog and self-hosted architecture into an implementation-ready, sequenced backlog.
- **Codex tools used:** Inspected `USER_STORIES.md`, `IDEA.md`, `ARCHITECTURE.md`, project instructions, and Git history; created a documentation feature branch; drafted and reviewed `BACKLOG.md`; checked whitespace and scope traceability.
- **Human decisions:** Include full v1 delivery work for product and implementers; keep the user story catalog and add a separate backlog; defer topic search to match the architecture; track open user validation as non-blocking discovery; retain English UI and English/isiZulu samples until validation.
- **Decisions recorded:** Added stable backlog IDs, MUST/DISCOVERY priorities, dependency order, acceptance criteria, test work, and operations/security/deployment tasks. The backlog chooses a host-managed TLS reverse proxy, consistent with the architecture options, and records architecture-deferred capabilities separately.
- **Review evidence:** `BACKLOG.md` is cross-referenced against the user stories and architecture. `git diff --check` is the documentation check; no application code changed, so application tests were not run.
