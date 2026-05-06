# Runbook: MCP Linear Sync

## Objective

Execute Linear updates through MCP with preflight checks and deterministic
action logs.

## When to use

- When project tracking must be created/updated in Linear from workflow output.
- When external-state mutations need repeatable operational steps.

## Inputs required

- `mcpServer` (default: `user-Linear`)
- `intent`
- `projectContext` (team/project/milestone references, if known)
- `issuePayloads`

## Steps

1. Run workflow `mcp-linear-planner`.
2. Confirm preflight checklist:
  - tool schema checked;
  - auth ready (`mcp_auth` path defined if required);
  - project/team context resolved.
3. Run workflow `mcp-linear-sync`.
4. Review `syncReport`:
  - success items;
  - failures with remediation;
  - skipped items and reason.

## Exit criteria

- All required entities were created/updated in Linear, or
- A complete retry/remediation plan is produced for pending failures.

## Failure handling

- Do not retry blindly.
- Re-check schema and payload mapping before rerunning failed actions.
- If auth/context is missing, resolve blocker first and rerun intake.

