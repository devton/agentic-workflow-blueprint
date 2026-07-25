---
name: changelog-generator
description: Generate customer-facing changelog entries from git commits and PR diffs.
---

## agentic-workflows-blueprint.workflow.changelog-generator

### Goal

Analyze session git commits and generate user-facing release notes categorized by features, fixes, and improvements.

### Scope

- **Applies to**: Post-implementation delivery and PR completion.
- **Does not cover**: Internal dev notes or technical refactor logs (keep those in code comments).

### Triggers

- "/changelog-generator"
- "Generate user changelog"
- "Summarize release notes"

### Inputs

- `baseBranch`: Target integration branch (`main`, `develop`).
- `commits`: Git commits in session.

### Invariants

1. **User Perspective**: Write for end users, not backend implementation details.
2. **Clear Categorization**: Group into Features ✨, Fixes 🐛, Improvements 🔧.

### Procedure

1. Run `git log` against `baseBranch` to inspect session commits.
2. Filter and categorize user-visible changes.
3. Transform technical language into clear benefit-driven descriptions.
4. Output changelog section into `walkthrough.md`.

### Outputs

- User-facing Changelog entry in `walkthrough.md`.

### Review Gate

- Changelog reflects user-facing impact accurately.

### References

- `../../SKILL.md`
- `../radioactive/SKILL.md`
- `../changelog/SKILL.md`
