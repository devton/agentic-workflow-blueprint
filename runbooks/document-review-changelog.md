# Runbook: Document -> Review -> Changelog

## Objective

Execute a reliable documentation pipeline with bounded retries and clear
handoff criteria.

## When to use

- After implementation is done and evidence is available (diff, files, tests).
- When the team needs auditable documentation and concise changelog entries.

## Inputs required

- `baseBranch`
- `diffScope`
- `docTarget`
- `changelogTarget`

## Steps

1. Run workflow `document`.
2. Run workflow `review`.
3. If `reviewStatus = FAIL`:
  - pass `reviewFindings` to `document`;
  - rerun `document`;
  - rerun `review`;
  - stop after 3 total attempts.
4. If `reviewStatus = PASS`, run `changelog`.

## Exit criteria

- Documentation updated and approved.
- Changelog entry generated from approved documentation.
- No unresolved blocking review findings.

## Failure handling

- If attempt 3 still fails, stop automation and request manual intervention.
- Keep review findings attached to the handoff note.