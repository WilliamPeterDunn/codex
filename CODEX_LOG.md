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
