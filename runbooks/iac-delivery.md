# Runbook: IaC Delivery

## Objective

Deliver infrastructure changes through an auditable plan/apply process with policy checks and drift awareness.

## When to use

- Terraform/OpenTofu module or environment updates.
- Any infrastructure delivery path where state integrity and policy compliance are required.

## Inputs required

- `iacTool`
- `workspaceOrEnvironment`
- `stateBackend`
- `planScope`
- `policyChecks`
- `rollbackPlan`

## Steps

1. Initialize environment and validate state backend/lock behavior.
2. Run formatting and validation checks.
3. Generate and review plan output.
4. Execute policy/security/cost controls.
5. Approve or reject apply based on risk and policy status.
6. Run apply and capture execution evidence.
7. Run post-apply verification and drift check.

## Exit criteria

- Plan and apply evidence are attached.
- Policy checks pass or approved exceptions are documented.
- Drift status is recorded and accepted.

## Failure handling

- If validation or policy checks fail, stop and remediate before apply.
- If apply fails, halt further changes and execute rollback plan.
- If drift remains unresolved, create follow-up action before closure.

## References

- [Visual HTML Version](./iac-delivery.html)
