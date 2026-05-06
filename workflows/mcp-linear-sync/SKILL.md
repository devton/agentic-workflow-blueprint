## agentic-workflows-blueprint.workflow.mcp-linear-sync

### Goal

Execute a controlled synchronization in Linear using the prevalidated MCP plan.

### Scope

- Applies to: creating/updating Linear milestones and issues via MCP.
- Does not cover: planning from scratch (use `mcp-linear-planner` first).

### Triggers

- "Run Linear sync"
- "Apply MCP plan to Linear"
- "Create/update issues in Linear from workflow"

### Inputs

- `linearMcpPlan` (from `mcp-linear-planner`)
- `teamId` / `projectId` (if required by selected actions)
- `issuePayloads` (titles, descriptions, parent/milestone links)
- `dryRun` (optional boolean)

### Invariants

- Re-check tool schema before each distinct MCP tool usage.
- Do not run if preflight has unresolved blockers.
- Keep call sequence deterministic and log each action outcome.
- On error, return actionable remediation instead of silent retries.

### Procedure

1. Validate `linearMcpPlan` and unresolved blockers.
2. If `dryRun = true`, render planned calls without mutating state.
3. Execute calls in order:
  - context fetch calls (`list_teams`, `list_projects`, `list_milestones`);
  - write calls (`create_milestone`, `create_issue`, `update_issue`).
4. Record each action with status (`success`, `failed`, `skipped`).
5. Produce a final sync report with created/updated entities and failures.

### Outputs

- `syncReport` with action-by-action status.
- `createdEntities` and `updatedEntities` lists.
- `retryPlan` for failed actions (if any).

### Review gate

- No call executed without matching schema validation.
- Sync report clearly maps inputs to resulting Linear entities.
- Failures include exact next-step remediation.

### References

- `../../SKILL.md`
- `../mcp-linear-planner/SKILL.md`