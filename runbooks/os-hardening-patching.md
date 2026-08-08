# Runbook: OS Hardening and Patching

## Objective

Apply operating system hardening and patching with service safety checks and recovery readiness.

## When to use

- Linux/Windows baseline hardening.
- Security patch cycles on production or shared infrastructure.

## Inputs required

- `platform`
- `environment`
- `baselineStandard`
- `patchScope`
- `serviceDependencies`
- `recoveryPlan`

## Steps

1. Record current baseline (versions, services, security controls).
2. Validate maintenance window and dependency owners.
3. Run pre-change health checks and backup/recovery validation.
4. Apply patching/hardening in controlled order.
5. Run post-change service, performance, and security checks.
6. Trigger recovery if any blocking regression is detected.
7. Publish execution report and follow-up monitoring tasks.

## Exit criteria

- Services remain healthy after patching/hardening.
- Security controls meet expected baseline.
- Recovery path is validated.

## Failure handling

- Pause rollout on first critical service regression.
- Execute recovery plan before continuing.
- Escalate through incident flow if recovery fails.

## References

- [Visual HTML Version](./os-hardening-patching.html)
