---
name: thermo-fix
description: Run a full code quality review on changed files, apply every identified fix, and iterate until approved (up to 3×).
---

## agentic-workflows-blueprint.workflow.thermo-fix

### Goal

Run a thermo-nuclear code quality review on the diff, apply every identified fix inline, verify the build, and iterate until all blockers are resolved (up to 3 cycles).

### Scope

- **Applies to**: Files modified in the current session or branch.
- **Does not cover**: Implementing new feature scope from scratch or running deployment pipelines.

### Triggers

- "/thermo-fix"
- "Run review and fix cycle"
- "Fix code quality findings"

### Inputs

- `diffScope`: Files to review (default: modified files in branch).
- `maxIterations`: Maximum review+fix cycles (default: 3).
- `buildCommand`: Project verification/build command.

### Invariants

1. **Never approve on behavior alone**: Structural regressions are blockers.
2. **Inline fix cycle**: Apply fixes in the same conversation without branching.
3. **Build verification**: Run build/syntax check after every fix cycle.
4. **No unsolicited changes**: Only fix identified quality findings.

### Procedure

1. **Identify Diff**: List all modified files.
2. **Review**: Run `thermo-nuclear-code-quality-review` to produce finding table (🔴, 🟠, 🟡, 🟢).
3. **Apply Fixes**: Fix findings top-down (🔴 → 🟠 → 🟡).
4. **Verify Build**: Run project build/check command. Fix build errors if any occur.
5. **Re-review**: Re-inspect all changed files. Stop if zero 🔴 or 🟠 remain. Otherwise repeat up to `maxIterations`.
6. **Commit**: Create a local git commit with finding summary.

### Outputs

- Resolved code findings.
- Build verification pass.
- Updated `task.md` and `walkthrough.md`.
- Local git commit.

### Review Gate

- Zero 🔴 (Blocker) findings.
- Zero 🟠 (High) findings.
- Project build/syntax checks pass.

### References

- `../../SKILL.md`
- `../thermo-nuclear-code-quality-review/SKILL.md`
- `../radioactive/SKILL.md`
