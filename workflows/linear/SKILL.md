## agentic-workflows-blueprint.workflow.linear

### Goal

Turn a technical plan into a tracked Linear project (milestones, parent
issues, sub-issues, status updates) using the `user-Linear` MCP server.

### Scope

- Applies to: multi-step initiatives that need tracking.
- Does not cover: writing the technical plan itself.

### Triggers

- Work spans multiple PRs or milestones.
- You need acceptance criteria and progress visibility.

### Inputs

- Linear team/project IDs (discover via MCP when unknown).

### Invariants

- If MCP tools fail due to auth, run `mcp_auth` for `user-Linear`.
- Always validate tool schema before calling MCP tools.
- Prefer 3-5 milestones and a small number of parent issues per milestone.

### Procedure

1. List projects and teams, then choose the correct team/project context.
2. Create milestones.
3. Create one parent issue for the initiative.
4. Create child issues by milestone and optional actionable sub-issues.
5. Publish periodic project updates with: completed, in-progress, next.

### Outputs

- Linear project structure with milestones and linked issues that mirrors the
implementation plan.

### Review gate

- Team/project context is explicit.
- Milestones and issues are linked consistently.
- Update cadence and status visibility are defined.

### References

- `../../SKILL.md`
- `../mcp-linear-planner/SKILL.md`
- `../mcp-linear-sync/SKILL.md`

