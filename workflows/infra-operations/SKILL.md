## agentic-workflows-blueprint.workflow.infra-operations

### Goal

Run infrastructure operations with controlled execution, operational evidence, and clear incident-safe handoffs.

### Scope

- Applies to: operational infrastructure changes across compute, storage, network, runtime services, and platform dependencies.
- Does not cover: product-only code refactors with no operational impact.

### Triggers

- "Run this infrastructure change"
- "Prepare infra change runbook"
- "Operate rollout with rollback checkpoints"
- "Create infra operations contract"

### Inputs

- `environment`
- `changeRequest` (ticket, owner, objective)
- `changeWindow`
- `serviceDependencies`
- `healthChecks`
- `rollbackPlan`

### Invariants

- No production mutation without pre-change health baseline.
- Every step must emit evidence (command output, dashboard snapshots, or logs).
- Rollback checkpoints are mandatory for high-risk steps.
- Incident escalation path must be known before execution.

### Procedure

1. Validate readiness: approvals, maintenance window, dependency ownership, and communication channel.
2. Capture baseline health and capacity metrics.
3. Execute change in incremental phases with explicit checkpoints.
4. Run health checks after each phase.
5. If any checkpoint fails, trigger rollback and incident communication.
6. Finalize post-change summary with completed actions, open risks, and next monitoring actions.

### Outputs

- Infra operations execution log.
- Checkpoint results and health evidence.
- Rollback or success closure note.
- Handoff note for `document` and `review`.

### Review gate

- [ ] Change window, owner, and escalation path are explicit.
- [ ] Baseline and post-change evidence are attached.
- [ ] Rollback checkpoints are complete.
- [ ] Handoff contains residual risk and monitoring instructions.

### References

- `../../SKILL.md`
- `../network-engineering/SKILL.md`
- `../document/SKILL.md`
- [Interactive HTML View](./README.html)
