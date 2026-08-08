## agentic-workflows-blueprint.workflow.os-platform

### Goal

Define and execute operating system baseline, hardening, and patching workflows with verifiable platform health controls.

### Scope

- Applies to: Linux/Windows server baselines, patching, hardening, service configuration, and OS-level compliance checks.
- Does not cover: application-only logic changes detached from host/runtime configuration.

### Triggers

- "Run OS hardening workflow"
- "Plan server patching and validation"
- "Audit platform baseline"
- "Create OS operations contract"

### Inputs

- `platform` (linux/windows/mixed)
- `environment`
- `baselineStandard` (CIS/internal baseline)
- `patchScope`
- `serviceCriticality`
- `rollbackOrRecoveryPlan`

### Invariants

- Baseline and target state must be explicit before changes.
- Patching and hardening changes require service impact assessment.
- Verification must include service status, resource health, and security posture checks.
- Recovery path must be defined for failed patch or hardening step.

### Procedure

1. Capture current baseline (packages, kernel/OS version, services, security controls).
2. Define hardening or patch set with expected service impact.
3. Run pre-change checks (service health, disk/memory headroom, backup/recovery readiness).
4. Apply patching/hardening actions in controlled steps.
5. Run post-change checks (service status, boot/runtime health, security scan delta).
6. Trigger recovery/rollback path if verification fails.
7. Publish platform handoff report with residual risk and follow-up actions.

### Outputs

- OS baseline delta report.
- Patching/hardening execution log.
- Post-change service and security verification evidence.
- Recovery/rollback status note.

### Review gate

- [ ] Baseline and target state are documented.
- [ ] Service impact and maintenance window are explicit.
- [ ] Post-change health and security checks pass.
- [ ] Recovery/rollback instructions are actionable.

### References

- `../../SKILL.md`
- `../infra-operations/SKILL.md`
- `../document/SKILL.md`
- [Interactive HTML View](./README.html)
