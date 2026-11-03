# Code review checklist

Review the changed code and relevant callers, server entry points, tests, and configuration. Report actionable findings with file and line references, ordered by severity. Do not repeat style preferences as defects.

## Privacy and authorization

- Flag public paths that can expose drafts, unpublished versions, private account fields, reports, or private story content.
- Check owner-only writes at every Server Action, Route Handler, and other server entry point; test cross-owner denial.
- Check media reads for publication and ownership checks on every request, generated asset IDs, traversal/symlink protection, and stale-cache access after unpublishing.
- Check public DTOs, pages, search results, and logs for private email, usernames, credentials, session tokens, reports, or private story content.

## Secrets and operational safety

- Flag credentials or private user data in source, fixtures, logs, CI output, and public responses.
- Host-only account and moderation commands must not be reachable through web routes. Password resets must not accept or log password arguments.
- Uploads must count streamed bytes, validate actual file type, reserve and release quota and temporary space, clean interrupted uploads, and use generated paths.
- Do not trust user-controlled filesystem paths, request size/type headers, or forwarded client-IP headers from untrusted proxies.

## Verification

- Check that new behavior has tests, including denial and failure cases for security-sensitive changes.
- Run relevant checks when requested or required by the task instructions; state what was and was not run.
- Record findings or the reason for not acting on them in the PR discussion or `CODEX_LOG.md`.
