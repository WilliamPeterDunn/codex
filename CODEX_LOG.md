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

## 2026-11-03 — Add commit CI checks

- **Task:** Run a secrets scan and Markdown linter on pushes and pull requests, and account for native Codex review in the workflow design.
- **Codex tools used:** Inspected repository files and Git state; checked official Gitleaks, markdownlint-cli2, and OpenAI Codex documentation; added a GitHub Actions workflow, Markdown lint configuration, repository review rules, and semantic subheadings to `USER_STORIES.md`.
- **Human direction:** Add a secrets scan and linter to commit CI; consider native Codex features and other external CI tools.
- **Decisions recorded:** Use Gitleaks and markdownlint-cli2 as the two deterministic CI checks, pin actions by commit SHA, scan full Git history, and limit workflow token permissions to read-only. Native Codex automatic PR review requires a connected repository and an enabled Codex settings toggle; this session cannot change that setting. A manual `@codex review` request received no bot response, and no `OPENAI_API_KEY` repository secret is configured for a separate Codex GitHub Action, so the action was not added. The new `AGENTS.md` review rules guide native Codex review once enabled. Markdownlint keeps line-length checks off for the existing prose style and allows repeated subheadings in separate stories; repeated story labels were changed from bold text to real headings.
- **Review evidence:** The workflow triggers on pushes and pull requests; linter targets Markdown because the repository currently contains planning documents only. Local `markdownlint-cli2@0.23.3 "**/*.md"` passed: 6 files, 0 issues. GitHub CI passed both Secrets scan and Markdown lint on push and pull request. No application tests are part of this tooling-only change.

## 2026-11-03 — Verify native Codex automatic review

- **Task:** Verify the native Codex GitHub review after automatic review was enabled in Codex settings.
- **Codex tools used:** Created a small documentation-only PR to trigger and observe the native review alongside the existing CI checks.
- **Human direction:** Enable automatic review and try it.
- **Review evidence:** [PR #6](https://github.com/WilliamPeterDunn/codex/pull/6) triggered a native Codex review on open; it completed with no findings. Secrets scan and Markdown lint also passed on the PR.

## 2026-11-03 — Record manual merge preference

- **Task:** Preserve the user's instruction that they will request merges to `main` manually.
- **Decision recorded:** Open and review pull requests as usual, but never merge without the user's explicit request.
- **Review evidence:** Updated the Git workflow rule in `AGENTS.md`; this preference change remains in a pull request until the user chooses to merge it.

## 2026-11-03 — Record implementation preflight

- **Task:** Resolve repository-side implementation prerequisites on the existing manual-merge preference PR.
- **Codex tools used:** Inspected the open PR, architecture, and local tool availability; added the exact Node patch pin and `DEVELOPMENT.md`; checked the setup guide with markdownlint.
- **Human direction:** Handle outstanding setup matters as commits on the open PR, then merge to `main`.
- **Decisions recorded:** Pin local Node to 24.22.1; use npm; document Docker Desktop/Compose as a required host prerequisite, Vitest for unit/SQLite integration tests, Playwright for browser flows, the CI checks that exist today, and CI/branch-protection work to add when the app scaffold exists. Keep automatic merges disabled; the user's explicit merge request authorizes this merge.
- **Review evidence:** Local markdownlint and `git diff --check` passed. `.nvmrc` pins Node 24.22.1, but the current shell remains on 24.13.0: winget verified the installer and then requested administrator elevation, so the install was cancelled. Docker Compose is not available in the current shell; installing Docker Desktop requires host-level setup. The app scaffold and app-level test/build jobs remain future bootstrap work.

## 2026-11-03 — Install Docker Compose in WSL

- **Task:** Install Docker Compose using a lighter setup than Docker Desktop.
- **Codex tools used:** Checked WSL 2 and Ubuntu 24.04 availability; consulted Docker's official Engine and Compose installation docs; installed Docker Engine, CLI, Buildx, and Compose V2 from Docker's signed Ubuntu apt repository; verified a container run.
- **Human direction:** Install Compose; prefer a lighter option because disk space is limited.
- **Decision recorded:** Use Docker Engine inside the existing Ubuntu WSL 2 distro rather than Docker Desktop. This avoids installing Desktop while preserving Docker Compose compatibility.
- **Review evidence:** `docker compose version` reported 5.7.0; Docker Engine reported 29.9.2; `docker run --rm hello-world` succeeded. Removed the test image after verification. Updated `DEVELOPMENT.md`; the PR is left open for user-requested merge.

## 2026-11-03 — Set the v1 reuse license choices

- **Task:** Resolve the open product choice for reuse terms before implementation.
- **Human decision:** Offer all six Creative Commons Attribution 4.0 International licenses (CC BY, CC BY-SA, CC BY-ND, CC BY-NC, CC BY-NC-SA, and CC BY-NC-ND) plus a custom permission statement. Require an explicit choice per story version and do not preselect or silently inherit terms.
- **Codex tools used:** Updated `USER_STORIES.md`, `BACKLOG.md`, and `ARCHITECTURE.md` to make the choice consistent across product behavior, acceptance criteria, and data model.
- **Review evidence:** Reviewed the six license identifiers against Creative Commons' official license overview; documentation lint and whitespace checks run for this change.
