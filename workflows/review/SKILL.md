## agentic-workflows-blueprint.workflow.review

### Goal

Review documentation quality and correctness with deterministic pass/fail
criteria.

### Scope

- Applies to: docs produced by the `document` workflow.
- Does not cover: implementing code fixes outside documentation scope.

### Triggers

- "Review this documentation output"
- "Check if document is correct"
- Automatic review step after `document`

### Inputs

- `docTarget`
- `sourceEvidence` (diff/files used by document workflow)
- `maxDocumentRetries` (default: 3)
- `currentAttempt` (1..3)

### Invariants

- Review must be evidence-based and actionable.
- Every failure must include concrete fix instructions.
- Keep feedback concise and unambiguous.

### Procedure

1. Validate documentation claims against `sourceEvidence`.
2. Check structure clarity and required sections.
3. Return one of:
  - `PASS`: no blocking issues;
  - `FAIL`: list exact issues and required fixes.
4. If `FAIL` and `currentAttempt < maxDocumentRetries`, route back to
  `document` with feedback.
5. If `FAIL` and retry limit reached, stop the chain and mark as unresolved.

### Outputs

- `reviewStatus`: `PASS` or `FAIL`
- `reviewFindings`: concise issue list with fix instructions
- `nextAction`: `run_changelog`, `rerun_document`, or `manual_intervention`

### Review gate

- Every `FAIL` item includes specific correction guidance.
- Final verdict maps directly to a next action.
- Retry rule (`<= 3`) is enforced.

### References

- `../../SKILL.md`
- `../document/SKILL.md`

