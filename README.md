# Space Engineers Mod Report Workspace

Private workspace for preparing Space Engineers mod bug reports and sanitized evidence packages.

This repository is for my own server/project work unless I explicitly make a specific report public.

## What belongs here

- Draft bug reports before sending them to mod authors.
- Sanitized log excerpts.
- Mod-specific notes.
- Reproduction steps.
- Test results and rollback notes.

## What does not belong here

- Raw server logs with private data.
- Full save files unless reviewed.
- Tokens, credentials, remote admin data, IPs, private paths, or player identifiers.
- Unreviewed config dumps.

## Workflow

1. Put raw logs in a local folder outside Git or in `private-do-not-upload/`.
2. Extract only relevant lines.
3. Sanitize the excerpt.
4. Write a report in `reports/outgoing/`.
5. Attach sanitized logs from `logs/safe-to-share/` when sending the report.
