# Project objective

Demonstrate rapid prototyping using LLM-assisted development. Make Codex tool use equally clear and reviewable as part of the deliverable, including meaningful tool actions, decisions, and evidence in `CODEX_LOG.md`.

## Git workflow

- Work on feature branches; keep `main` stable.
- Make small, focused commits with clear messages.
- Push feature branches regularly so work is backed up and reviewable.
- When a feature is ready, open a pull request and merge it into `main` after review.
- Automatically have Codex review every pull request before it is merged. Address findings or document why a finding is not acted on.

## Definition of done for implementation work

- Use test-driven development: write a failing test for the intended behavior, implement the change, then run the test and iterate until it passes.
- Implementation work is not done until the relevant tests pass. Report any tests that could not be run and why.
