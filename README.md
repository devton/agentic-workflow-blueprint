# Agentic Workflows Blueprint

This folder provides a reusable blueprint to scaffold an agent-oriented
documentation/workflow structure in any repository.

## What to customize first

- `projectSlug`: the target repository name.
- `baseBranch`: integration branch (`main`, `develop`, etc.).
- `techStack`: short stack description.
- `constraints`: hard rules that cannot be violated.
- `workflowsWanted`: the workflow ids you want to scaffold.

## How to use

1. Read `SKILL.md` to understand the required contract format.
2. Create the project entry skill (`skills/<projectSlug>/SKILL.md`).
3. Create references (`routing-matrix.md`, `role-contracts.md`).
4. Create workflows under `skills/<projectSlug>/workflows/`.
5. Link everything from the root instruction file (`AGENTS.md`, etc.).
6. Verify all links and workflow ids.

## Included example workflows

This blueprint already includes three example workflows under `workflows/`:

- `document`: builds/update docs from the implementation diff.
- `review`: validates document output and returns pass/fail feedback.
- `changelog`: writes a concise changelog entry after review passes.

## Example chained flow (document -> review -> changelog)

Use this flow to demonstrate orchestration behavior:

1. Run `document`.
2. Run `review`.
3. If `review` fails:
  - rerun `document` with review feedback;
  - rerun `review`;
  - repeat up to 3 attempts total.
4. If review passes, run `changelog`.

Pseudo-flow:

```text
attempt = 1
while attempt <= 3:
  doc = run(document)
  review = run(review, input=doc)
  if review.pass:
    run(changelog, input=doc)
    break
  attempt += 1
```

## Goal of these examples

The objective here is to show a practical, composable workflow pattern. Teams
can keep this blueprint as reference and adjust naming, files, and gates to
their own project standards.