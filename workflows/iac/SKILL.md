## agentic-workflows-blueprint.workflow.iac

### Goal

Manage Infrastructure as Code changes through deterministic plan/apply validation, policy gates, and drift-aware operations.

### Scope

- Applies to: Terraform/OpenTofu/CloudFormation-style infrastructure definitions, modules, policies, and environment state management.
- Does not cover: manual infrastructure changes that bypass IaC workflows.

### Triggers

- "Apply Terraform/OpenTofu change"
- "Review IaC plan"
- "Handle drift with Infrastructure as Code"
- "Create IaC delivery workflow"

### Inputs

- `iacTool` (Terraform, OpenTofu, etc.)
- `workspaceOrEnvironment`
- `stateBackendContext`
- `planScope`
- `policyChecks` (security/cost/compliance)
- `rollbackStrategy`

### Invariants

- No `apply` without reviewed plan output.
- Policy checks must pass or be explicitly waived with rationale.
- State operations must be serialized and auditable.
- Drift detection must be acknowledged before execution.

### Procedure

1. Initialize environment and validate backend/state lock readiness.
2. Run format/validate checks for IaC definitions.
3. Generate plan and summarize resource-level impact.
4. Run policy and security checks.
5. Obtain approval for apply scope.
6. Execute apply and capture outputs.
7. Run post-apply verification and drift snapshot.

### Outputs

- IaC validation and plan report.
- Policy/security check summary.
- Apply execution evidence.
- Drift and rollback notes for handoff.

### Review gate

- [ ] Plan is reviewed and traceable to requested change.
- [ ] Policy/security checks are passed or justified.
- [ ] Apply outputs and post-check evidence exist.
- [ ] Drift status is documented.

### References

- `../../SKILL.md`
- `../infra-operations/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
