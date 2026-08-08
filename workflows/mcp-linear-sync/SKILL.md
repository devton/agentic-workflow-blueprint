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
- `changeWindow` (optional): execution window to respect during write calls
- `riskClass` (optional): risk level for execution controls
- `rollbackTicket` (optional): rollback reference for handoff and incident traceability

### Invariants

- Re-check tool schema before each distinct MCP tool usage.
- Do not run if preflight has unresolved blockers.
- Keep call sequence deterministic and log each action outcome.
- On error, return actionable remediation instead of silent retries.
- If outside `changeWindow`, stop and return `manual_intervention`.

### Procedure

1. Validate `linearMcpPlan` and unresolved blockers.
2. If `dryRun = true`, render planned calls without mutating state.
3. Validate operational controls (`changeWindow`, `riskClass`, `rollbackTicket`) from plan or explicit input.
4. Execute calls in order:
  - context fetch calls (`list_teams`, `list_projects`, `list_milestones`);
  - write calls (`create_milestone`, `create_issue`, `update_issue`).
5. Record each action with status (`success`, `failed`, `skipped`).
6. Produce a final sync report with created/updated entities, failures, and operational metadata.

### Outputs

- `syncReport` with action-by-action status.
- `createdEntities` and `updatedEntities` lists.
- `retryPlan` for failed actions (if any).

### Review gate

- No call executed without matching schema validation.
- Sync report clearly maps inputs to resulting Linear entities.
- Failures include exact next-step remediation.
- Execution respects change window constraints when provided.

### References

- `../../SKILL.md`
- `../mcp-linear-planner/SKILL.md`
- [Interactive HTML View](./README.html)