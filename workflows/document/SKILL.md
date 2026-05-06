## agentic-workflows-blueprint.workflow.document

### Goal

Create or update project documentation from the implementation evidence
(primarily git diff and related files).

### Scope

- Applies to: documenting completed implementation work.
- Does not cover: code changes unrelated to documentation.

### Triggers

- "Document this change"
- "Create task documentation from diff"
- "Write implementation notes for handoff"

### Inputs

- `baseBranch`
- `diffScope` (commit range, PR diff, or working tree)
- `docTarget` (destination file path)
- `docStyle` (technical, product-facing, mixed)
- `reviewFeedback` (optional, when rerunning after review fail)

### Invariants

- Use evidence from actual code changes.
- Keep facts aligned with diff and existing behavior.
- Do not invent decisions or results not present in evidence.
- If rerun from review feedback, address each reported issue explicitly.

### Procedure

1. Inspect `diffScope` and collect relevant evidence.
2. Draft/update `docTarget` with objective, context, behavior, and validation.
3. If `reviewFeedback` exists, apply all required corrections.
4. Produce a final documentation output ready for review.

### Outputs

- Updated documentation file at `docTarget`.
- A short "what changed" note for reviewer handoff.

### Review gate

- [ ] Every major statement is grounded in implementation evidence.
- [ ] Known limitations and assumptions are explicit.
- [ ] The document is coherent for the intended audience.

### References

- `../../SKILL.md`
- `../review/SKILL.md`
- `../changelog/SKILL.md`
