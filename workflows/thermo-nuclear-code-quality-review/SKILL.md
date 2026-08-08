---
name: thermo-nuclear-code-quality-review
description: Run an extremely strict maintainability review for abstraction quality, giant files, and spaghetti-condition growth.
---

## agentic-workflows-blueprint.workflow.thermo-nuclear-code-quality-review

### Goal

Perform an unusually strict review focused on implementation quality, maintainability, abstraction quality, and codebase health.

### Scope

- **Applies to**: Any code change or pull request diff across the codebase.
- **Does not cover**: Direct code modification or executing fixes (use `thermo-fix`).

### Triggers

- "Run thermo-nuclear review"
- "/thermo-nuclear-code-quality-review"
- "Deep code quality audit"
- "Harsh maintainability review"

### Inputs

- `diffScope`: Files or commit range to review.
- `existingRootDoc`: Main instruction file (`AGENTS.md`, `CLAUDE.md`, etc.).

### Invariants

1. **Be ambitious about structural simplification**: Search for "code judo" moves that make implementation simpler, smaller, and cleaner.
2. **1000 line limit**: Do not let a PR push a file past 1k lines without strong justification.
3. **No random spaghetti growth**: Reject ad-hoc conditionals or one-off branches inserted into unrelated flows.
4. **Clean design over working code**: Do not approve merely because tests pass if the structure is messier.
5. **No silent fallbacks**: Ensure boundary cleanliness and explicit error/state contracts.

### Procedure

1. Identify all changed files in `diffScope`.
2. Evaluate against the **Non-Negotiable Additional Standards**:
   - Structural simplification & Code Judo opportunities.
   - Decomposition of files approaching 1k lines.
   - Elimination of ad-hoc conditionals or leaky abstractions.
   - Type contract and boundary cleanliness.
3. Produce a **Severity-Ranked Finding Table**:
   - **Blocker**: Structural regressions, 1k+ line file growth, severe spaghetti.
   - **High**: Major maintainability or boundary issues.
   - **Medium**: Moderate cleanup or abstraction improvements.
   - **Low**: Informational nits.
4. Output specific, actionable remedies for every Blocker and High finding.

### Outputs

- Severity-ranked finding table appended to `walkthrough.md` or conversation output.
- Final verdict: **APPROVED** or **FINDINGS REMAIN**.

### Review gate

- No unaddressed Blocker findings.
- No unaddressed High findings.
- Implementation preserves architecture contracts.

### References

- `../../SKILL.md`
- `../thermo-fix/SKILL.md`
- `../radioactive/SKILL.md`
- [Interactive HTML View](./README.html)
