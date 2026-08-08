# Runbook: Network Change

## Objective

Execute network changes with pre-checks, post-checks, and rollback controls that reduce operational risk.

## When to use

- Routing, DNS, firewall, segmentation, peering, or load balancer changes.
- Any change where connectivity or security boundaries can be affected.

## Inputs required

- `environment`
- `changeWindow`
- `networkIntent`
- `affectedServices`
- `verificationCommands`
- `rollbackPlan`

## Steps

1. Capture current topology and baseline connectivity metrics.
2. Validate approvals, maintenance window, and communication path.
3. Run pre-change verification commands and store outputs.
4. Apply changes incrementally with checkpoint pauses.
5. Run post-change verification and compare with baseline.
6. If validation fails, execute rollback and communicate incident status.
7. Publish closure note with residual risk and monitoring follow-up.

## Exit criteria

- All critical connectivity checks pass.
- Security boundaries remain compliant.
- Rollback path is either unneeded (success) or executed successfully (failure case).

## Failure handling

- Stop on first critical verification failure.
- Trigger rollback before starting additional changes.
- Escalate to incident protocol if rollback fails.

## References

- [Visual HTML Version](./network-change.html)
