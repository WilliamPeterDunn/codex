# Project objective

Demonstrate rapid prototyping using LLM-assisted development. Make Codex tool use equally clear and reviewable as part of the deliverable, including meaningful tool actions, decisions, and evidence in `CODEX_LOG.md`.

## Git workflow

- Work on feature branches; keep `main` stable.
- Make small, focused commits with clear messages.
- Push feature branches regularly so work is backed up and reviewable.
- When a feature is ready, open a pull request and wait for the user to request its merge into `main`. Never merge automatically, even after review and passing checks.
- Automatically have Codex review every pull request before it is merged. Address findings or document why a finding is not acted on.

## Definition of done for implementation work

- Use test-driven development: write a failing test for the intended behavior, implement the change, then run the test and iterate until it passes.
- Implementation work is not done until the relevant tests pass. Report any tests that could not be run and why.

## Code Review Rules

### Privacy and authorization

- Flag any public path that could expose drafts, unpublished story versions, private account fields, reports, or private story content. Check that owner-only writes enforce ownership at every server entry point.
- Flag media access that can bypass publication/ownership checks, use user-controlled filesystem paths, or serve stale cached content after unpublishing.

### Secrets and operational safety

- Flag credentials or private user data in source, logs, CI output, or public DTOs. Host-only account and moderation commands must not be reachable through web routes; password resets must not accept or log password arguments.
- Flag upload handling that trusts request size/type headers, skips quota reservations/cleanup, or permits untrusted forwarded client-IP headers.
