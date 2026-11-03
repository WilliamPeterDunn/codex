# Complete v1 delivery backlog

This backlog converts the v1 product brief and architecture into sequenced, implementable work for product and engineering. It is a plan, not a statement that the app exists. User-facing stories retain their fuller context and acceptance criteria in `USER_STORIES.md`; the IDs below provide traceability.

## Scope and ordering

- **MUST** means required for the self-hosted v1 prototype. Implement in dependency order; all MUST items are in the v1 target.
- **DISCOVERY** means non-blocking user research. Keep the current English interface and English/isiZulu fixtures while research is pending; update scope only from validated findings.
- Anything explicitly deferred below is not part of this backlog.
- Search covers title, creator display name, and description, with language and format filters. **Topic search is deferred**: it appears in the brief's reader workflow but not in the v1 prototype scope or architecture routes/data fields.
- Start implementation work test-first: add a failing test for each behavior before implementation, then run the relevant checks and record evidence in `CODEX_LOG.md`.

### User story traceability

| Existing story | Delivery items |
| --- | --- |
| US-01 Find stories | READ-01 |
| US-02 Read and switch versions | READ-02, MEDIA-03 |
| US-03 Publish a story | PUB-01–PUB-03, MEDIA-01 |
| US-04 Add a translation | PUB-04 |
| US-05 Upload and serve media | MEDIA-01–MEDIA-03 |
| US-06 Manage creator account | AUTH-01–AUTH-03 |
| US-07 Report and unpublish | MOD-01–MOD-02 |

Cross-cutting foundation, accessibility, demo, moderation operations, deployment, and quality work are represented by the remaining items.

## Discovery items — non-blocking

### DISC-01 — Validate the first community and language pair

**Outcome:** Intended storytellers/readers inform the first community and language pair without making the demo wait for research.

**Acceptance:** Record who was consulted, the selected or unresolved community/language pair, and resulting product changes; keep English UI and English/isiZulu sample fixtures until a decision is recorded.

### DISC-02 — Validate safeguarding, reuse wording, and media needs

**Outcome:** Capture user understanding and needs around stories involving other people, attribution/reuse choices, media formats, and connectivity limits.

**Acceptance:** Record findings and proposed changes for human review; do not imply a checkbox establishes consent or ownership, and do not silently change v1 limits or license wording.

## Delivery backlog — MUST

### Epic 1: Foundation and data boundaries

#### FND-01 — Bootstrap the self-hosted app

**Depends on:** —

**Outcome:** A runnable Next.js App Router and TypeScript app using a pinned Node.js 24 LTS patch image and locked dependencies. Public library/detail pages render on the server; client components handle filters, media controls, and uploads; Server Actions handle story edits/publication/reports; Route Handlers stream uploads and serve media.

**Acceptance:** The app builds and starts in the planned container; configuration is separated from source; setup commands and required environment values are documented without committing secrets.

#### FND-02 — Add SQLite persistence and migrations

**Depends on:** FND-01

**Outcome:** Versioned migrations run before the app serves requests, with SQLite and media rooted on the persistent `/app/data` volume.

**Acceptance:** A clean database migrates to the current schema; a fixture database migrates without losing records; an app restart retains database and media data.

#### FND-03 — Model works, versions, profiles, reports, and media assets

**Depends on:** FND-02

**Outcome:** Persist stable creator-owned works, language-specific versions, public profiles, private reports, and generated media asset metadata.

**Acceptance:** Enforce one version per work/language; each version stores its own format, body/media reference, translator/review fields, attribution, and explicit license/custom permission; reject publishing without required terms; keep private auth fields out of public DTOs.

#### FND-04 — Enforce public-read and owner-write boundaries

**Depends on:** FND-03

**Outcome:** Shared data-access functions enforce authorization at every server entry point.

**Acceptance:** Public queries return published versions only; owners can manage their drafts and versions; cross-owner operations are denied; an unpublished version and its assets fail public reads on the next request; logs omit passwords, session tokens, private account email, and private story content; tests cover allow and deny cases.

#### FND-05 — Establish CI and local quality checks

**Depends on:** FND-01

**Outcome:** Every pull request runs automated checks without production data or credentials.

**Acceptance:** CI runs typecheck, lint, Vitest unit/integration tests against temporary SQLite data, Playwright browser tests, and a Docker image build; failures block a green result; no Vercel preview or production data is used.

### Epic 2: Creator identity and account security

#### AUTH-01 — Sign up and sign in with username and password

**Depends on:** FND-03, FND-04

**Outcome:** Better Auth username and email/password plugins use the SQLite adapter and `better-sqlite3`; retain its default scrypt password hashing, a private account email, and separate public display name.

**Acceptance:** Signup collects username, private email, display name, and password, does not automatically sign in, and gives generic duplicate-account responses; sign-in accepts username/password only; email remains unverified and is not used for messaging/recovery; generic errors do not reveal account existence; username availability lookup is disabled; enforce 5 sign-ins per IP/minute and 3 signups per IP/hour with database-backed limits.

#### AUTH-02 — Manage public profile and own stories

**Depends on:** AUTH-01, FND-04

**Outcome:** Creators can update their display name and use a dashboard to manage only their own works and versions.

**Acceptance:** Owner actions succeed and cross-owner actions fail; public pages, search, DTOs, and logs omit email, username, password, and session secrets; public identity can be a display name or pseudonym.

#### AUTH-03 — Change password and revoke sessions

**Depends on:** AUTH-01

**Outcome:** Signed-in users can change their password safely.

**Acceptance:** A successful password change revokes other active sessions; invalid credentials do not change the account; tests verify session behavior and ensure passwords/tokens do not enter logs.

### Epic 3: Story publishing and translations

#### PUB-01 — Create and edit a story draft

**Depends on:** AUTH-02, FND-03

**Outcome:** A creator can enter title, description, language, one primary format, content/media reference, and applicable descriptive text.

**Acceptance:** Drafts persist across restart; inputs are validated at server boundaries; only the owner can edit; one version has text in SQLite or one uploaded media file; transcript/caption/alt-text fields are available where applicable.

#### PUB-02 — Set attribution and reuse terms per version

**Depends on:** PUB-01

**Outcome:** For each version, the creator explicitly chooses one of the six Creative Commons Attribution 4.0 International licenses (CC BY, CC BY-SA, CC BY-ND, CC BY-NC, CC BY-NC-SA, or CC BY-NC-ND) or writes a custom permission statement. No license is preselected.

**Acceptance:** Plain-language explanations and a link to the full license deed or custom permission are displayed; store the selected license identifier/URL or custom statement per version; terms are never inherited silently; required terms prevent publication when absent; interface does not claim selection establishes consent or ownership.

#### PUB-03 — Preview and publish a version

**Depends on:** PUB-01, PUB-02, MEDIA-01

**Outcome:** Creator can preview the public presentation and deliberately publish a complete version.

**Acceptance:** Preview matches the public metadata/media layout; published versions become visible to anonymous readers; drafts remain unavailable from public page, search, and media endpoints; failed validation leaves the version unpublished.

#### PUB-04 — Add a linked translation

**Depends on:** PUB-01, PUB-02

**Outcome:** A story owner can add a separate language version with translator credit and status.

**Acceptance:** Store BCP-47 language, translator credit, translation/review status, format/content, and independent attribution/reuse terms; reject duplicate work/language; published versions link to the source and each other; drafts remain private; translations are not generated automatically; a translation is not publicly orphaned if its source is unavailable.

### Epic 4: Media upload and delivery

#### MEDIA-01 — Validate and store uploads

**Depends on:** FND-02, AUTH-02

**Outcome:** Creators can upload JPEG/PNG/WebP, MP3/WAV, MP4/WebM, and PDF files to their story versions.

**Acceptance:** Enforce 40 MiB per file, 20 files and 200 MiB per account, 200 MiB aggregate temporary-upload capacity, and 1 GiB reserved host disk; count streamed bytes independently of request headers; validate supported type; reserve quota/temporary space during upload; use generated IDs; atomically move validated files and save metadata; return clear limit errors.

#### MEDIA-02 — Clean up failed and interrupted uploads

**Depends on:** MEDIA-01

**Outcome:** Incomplete upload state does not accumulate or become public.

**Acceptance:** Failed/interrupted uploads remove temporary files and reservations and can restart from the beginning; startup removes stale temporary files/reservations; concurrent reservations do not overrun limits; tests cover interrupted and disk-full cases.

#### MEDIA-03 — Serve protected media and document downloads

**Depends on:** FND-04, MEDIA-01

**Outcome:** Media is served only by generated asset ID after visibility/ownership checks.

**Acceptance:** Draft assets are owner-only; public assets require a published version; unpublishing blocks subsequent requests; reject traversal, symlink, and arbitrary-path access; send `Cache-Control: no-store`, `X-Content-Type-Options: nosniff`, and safe content headers; PDFs download as attachments; valid audio/video ranges return `206` and invalid ranges return `416`.

### Epic 5: Reader experience

#### READ-01 — Browse, search, and filter the published library

**Depends on:** FND-04, PUB-03

**Outcome:** Anonymous readers can find published works in the library.

**Acceptance:** `/stories` supports URL-backed `q`, `language`, and `format`; text search covers title, creator display name, and description; filters combine; results/counts exclude drafts and unpublished versions; empty and clear-filter states work; reads are dynamic/no-store so publication changes appear on the next request; private account fields never appear.

#### READ-02 — View a story and switch versions

**Depends on:** PUB-04, MEDIA-03

**Outcome:** Readers can engage with a selected version and retain source/translation context.

**Acceptance:** `/stories/[slug]?lang=…` presents title, creator, language, format, attribution, reuse terms, and media; available translations show language, translator, and review status; switching identifies the current version and returns to source; unavailable language is handled without implying a translation; page reads are dynamic/no-store; unpublished pages and media are denied on the next request.

#### READ-03 — Make reader and creator flows responsive and accessible

**Depends on:** READ-01, READ-02, PUB-01

**Outcome:** Main flows work across viewport sizes and with keyboard/screen-reader affordances.

**Acceptance:** Library, story detail, sign-in/sign-up, dashboard, publish/preview, translation, and reporting flows have responsive layouts, keyboard-accessible controls, clear labels for media/language, and visible focus; run Playwright coverage for core keyboard and narrow viewport paths.

#### READ-04 — Seed multilingual, multi-format demo content

**Depends on:** FND-03, PUB-03, MEDIA-03, READ-02

**Outcome:** A fresh demo illustrates more than one medium and language.

**Acceptance:** Seed a small amount of English and isiZulu content, include a linked translation and at least two media formats, provide clear credits/terms, and keep sample fixtures separate from real user data; empty-library state remains testable.

### Epic 6: Reporting and host moderation

#### MOD-01 — Submit a private reader report

**Depends on:** AUTH-01, FND-03, FND-04

**Outcome:** Signed-in readers can report a story or version for maintainer review.

**Acceptance:** Store reports privately; reject anonymous submissions; rate-limit to 5 per account/hour; confirm submission without exposing other reports, reporter identity, or maintainer notes; test authorization and rate limits.

#### MOD-02 — Add host-only account and report commands

**Depends on:** AUTH-01, MOD-01

**Outcome:** Maintainers can create/disable accounts, list reports, reset passwords, and unpublish versions inside the app container.

**Acceptance:** Commands are unavailable via web routes; reset uses a no-echo prompt, never a password argument, auth-library hashing, and session revocation; report output omits credentials/secrets; unpublishing preserves ownership/draft data while removing public page/search/media access.

#### MOD-03 — Document account recovery and maintainer procedure

**Depends on:** MOD-02

**Outcome:** The operator can explain the manual recovery boundary without relying on unverified email.

**Acceptance:** Operator docs identify how users contact the named maintainer through an independently known channel; reset occurs only after independent identity verification; otherwise the account is documented as unrecoverable in v1.

### Epic 7: Deployment and durable operations

#### OPS-01 — Run one app with persistent data behind HTTPS ingress

**Depends on:** FND-01, FND-02

**Outcome:** Docker Compose runs one app instance with a named volume and a host-managed TLS reverse proxy.

**Acceptance:** Bind the app only to the proxy network; configure canonical URL, trusted origins, secure cookies, and forwarded/client-IP headers only for the configured proxy; app secrets stay out of Git; public deployment uses HTTPS; no replicas or multi-host writes are configured.

#### OPS-02 — Back up and restore database and media together

**Depends on:** FND-02, MEDIA-01

**Outcome:** Operator can make a consistent off-host backup and restore it before upgrades.

**Acceptance:** Document stopping the app to pause writes/uploads, archiving the closed DB and media together, copying off-host, restoring and verifying before restart; any live backup procedure accounts for SQLite WAL; restore test covers media/database references.

#### OPS-03 — Document deployment, upgrades, and host responsibilities

**Depends on:** OPS-01, OPS-02, MOD-03

**Outcome:** Maintainer can operate the prototype on one self-hosted machine.

**Acceptance:** Document manual deploy from `main`, migration-before-serving order, backup/restore, TLS proxy configuration, disk reservation/capacity, host security updates, and known single-instance/non-durable limits; no managed backups or automated deploy are implied.

### Epic 8: End-to-end verification and release readiness

#### QA-01 — Cover critical user journeys and security boundaries

**Depends on:** AUTH-01, PUB-03, PUB-04, MEDIA-03, READ-01, READ-02, MOD-01, MOD-02

**Outcome:** Browser and integration tests demonstrate the core prototype flows without real personal data.

**Acceptance:** Playwright covers signup/sign-in, browse/search/filter, publish/preview, translation and version switching, private reports, and denial after unpublishing; integration tests cover invalid credentials, duplicate signups, rate limits, owner/cross-owner access, private-email omission, explicit license requirements, upload size/type/quota/concurrency/interruption/disk-full boundaries, byte ranges, cache behavior, and host-only commands. Use seeded test identities.

#### QA-02 — Verify deployment and recovery properties

**Depends on:** OPS-01, OPS-02, OPS-03, FND-05

**Outcome:** Release checks exercise the actual self-hosted boundary and recovery process.

**Acceptance:** Verify reverse-proxy-only app access, migration from a fixture DB, offline backup/restore, cleanup after restart, path traversal/symlink rejection, and Docker build; record command output and reviewed diffs in `CODEX_LOG.md` without secrets or private story data.

## Explicitly deferred

Topic filtering; automated translation; external translators; revision history; in-app moderation queue/status workflow; payments; analytics; cloud storage; multiple app instances/horizontal scaling; resumable uploads; managed/automated backups; production search infrastructure; non-PDF documents; and regional/legal policy determinations.
