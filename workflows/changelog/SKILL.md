## agentic-workflows-blueprint.workflow.changelog

### Goal

Generate a concise changelog entry after documentation review is approved.

### Scope

- Applies to: finalized changes with `reviewStatus = PASS`.
- Does not cover: publishing or release automation.

### Triggers

- "Generate changelog entry"
- Final step after `document -> review` chain succeeds

### Inputs

- `reviewStatus`
- `docTarget`
- `changeSummary`
- `changelogTarget` (file path or buffer destination)

### Invariants

- Only run when `reviewStatus = PASS`.
- Keep entries concise, factual, and user-relevant.
- Avoid internal-only noise unless explicitly requested.

### Procedure

1. Confirm review pass status.
2. Extract relevant end-user/maintainer impact from `docTarget` and
   `changeSummary`.
3. Write/update a changelog entry in `changelogTarget`.
4. Keep wording stable and easy to scan.

### Outputs

- One changelog entry ready to be stored/published later.

### Review gate

- [ ] Entry reflects approved documentation.
- [ ] Entry is concise and understandable without source diff.
- [ ] No unsupported claims.

### References

- `../../SKILL.md`
- `../document/SKILL.md`
- `../review/SKILL.md`
