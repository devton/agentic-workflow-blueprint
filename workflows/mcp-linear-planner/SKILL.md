## agentic-workflows-blueprint.workflow.mcp-linear-planner

### Goal

Validate MCP prerequisites and produce a deterministic execution plan for
Linear operations.

### Scope

- Applies to: preparing any Linear MCP integration run.
- Does not cover: executing create/update actions in Linear.

### Triggers

- "Plan Linear sync via MCP"
- "Prepare MCP workflow for Linear"
- "Validate Linear MCP readiness"

### Inputs

- `mcpServer` (default: `user-Linear`)
- `intent` (create project items, update statuses, organize milestones)
- `projectContext` (optional ids/names already known)
- `baseBranch` (optional, for linking implementation scope)

### Invariants

- Always inspect tool schema/descriptor before calling any MCP tool.
- If server exposes `mcp_auth`, run auth before operational calls when needed.
- Keep parameter names aligned with tool schema (no guessed fields).
- Produce a written action plan before mutating external state.

### Procedure

1. List/inspect available Linear MCP tools and required arguments.
2. Verify auth readiness; if blocked, route to auth and retry.
3. Resolve target team/project context (or mark as required input).
4. Build an operation plan:
  - ordered tool calls;
  - payload shape per call;
  - idempotency/safety notes.
5. Emit a plan artifact to be consumed by `mcp-linear-sync`.

### Outputs

- `linearMcpPlan` with ordered steps and payload schemas.
- `preflightChecklist` (`tools`, `auth`, `context`) marked pass/fail.
- `blockingItems` list when execution cannot continue.

### Review gate

- Tool schema was explicitly checked before planned calls.
- Auth path is defined (or confirmed not required).
- Plan has deterministic call order and parameter mapping.

### References

- `../../SKILL.md`
- `../mcp-linear-sync/SKILL.md`

