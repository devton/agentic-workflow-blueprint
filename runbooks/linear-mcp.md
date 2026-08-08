# Runbook: Linear MCP (Direct Workflow)

## Objective

Execute Linear project management through a single workflow (`linear`) with clear setup and traceable outcomes.

## When to use

- You want a straightforward Linear integration flow.
- You do not need to split preflight and execution into separate workflows.

## Inputs required

- `teamId` or team discovery criteria
- `projectId` or project discovery criteria
- initiative description and issue payloads
- `changeWindow` (optional)
- `riskClass` (optional)
- `rollbackTicket` (optional)

## Steps

1. Run workflow `linear`.
2. Confirm auth readiness (run `mcp_auth` if needed).
3. Validate tool schemas for intended MCP calls.
4. Create/update milestones and issues.
   - Include operational metadata in descriptions when provided.
5. Publish project update summary.

## Exit criteria

- Milestones and issues are linked to the intended project/team.
- A status update was published with completed/in-progress/next.
- Optional operational metadata is preserved for audit traceability.

## Failure handling

- If auth fails, stop and resolve auth first.
- If payload validation fails, correct mapping and rerun.

## References

- [Visual HTML Version](./linear-mcp.html)