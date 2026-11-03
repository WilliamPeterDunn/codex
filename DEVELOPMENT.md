# Development setup

This repository is still at the planning stage; the Next.js app scaffold and package scripts have not been created yet. This document records the toolchain and test approach for the first implementation pass.

## Local prerequisites

- **Node.js:** Use the exact LTS patch in `.nvmrc` (`24.22.1`). Verify with `node --version`. Use npm and commit the generated `package-lock.json` when the app is scaffolded.
- **Docker Engine with Compose V2:** Installed in the existing Ubuntu 24.04 WSL 2 distribution (Docker Engine 29.9.2, Compose plugin 5.7.0). Docker Desktop is not required. Run Docker commands from Ubuntu WSL; from PowerShell, prefix them with `wsl -d Ubuntu --`. The Windows repository is available inside Ubuntu at `/mnt/c/Projects/personal/codex`.
- **Git and GitHub:** Work on feature branches and open pull requests. The user will explicitly request any merge to `main`.

Docker images and containers use the Ubuntu WSL virtual disk. Check usage with `docker system df`; remove unused images with `docker image prune` when space is needed. Do not prune volumes, which may contain persistent app data.

Keep local configuration in an ignored `.env.local`, create an `.env.example` containing names and safe defaults only, and use seeded test identities. Never use production data or put credentials in CI, fixtures, logs, or `CODEX_LOG.md`.

## Test approach

- **Vitest:** Unit tests for validation and domain logic, plus integration tests against a temporary SQLite database for migrations, ownership, publication visibility, reports, and media metadata.
- **Playwright:** Browser end-to-end tests for account flows, publishing, browsing, version switching, reporting, and media access. Use it for async Server Component user journeys.
- Follow TDD: write a failing test for each behavior before implementation, then run the affected tests and the full relevant suite.

At app bootstrap, add npm scripts for lint, typecheck, unit/integration tests, Playwright, and production build. CI should run those scripts against temporary SQLite data and build the Docker image; do not use real user data or deployment secrets.

## CI and review already configured

GitHub Actions currently runs Gitleaks on full Git history and markdownlint-cli2 on pushes and pull requests. CI does not yet run app lint, typecheck, tests, or Docker build because there is no app scaffold or package manifest. Native Codex review is enabled and has been verified on a newly opened pull request; the review trigger may need a manual `@codex review` for later commits, depending on its Codex settings.

Once app checks exist, configure the `main` branch rules to require the passing CI status checks. Keep the merge action manual: only merge after the user explicitly requests it.
