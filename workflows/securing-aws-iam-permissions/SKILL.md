## agentic-workflows-blueprint.workflow.securing-aws-iam-permissions

### Goal

Harden AWS IAM permissions with least-privilege controls, boundary guardrails, key-risk reduction, and continuous monitoring backed by auditable evidence.

### Scope

- Applies to: IAM users, roles, groups, policies, boundaries, and related account-level guardrails.
- Does not cover: non-AWS identity systems or application-layer authorization logic.

### Triggers

- "Harden AWS IAM permissions"
- "Reduce wildcard IAM policies"
- "Implement permission boundaries and MFA enforcement"
- "Remediate IAM Access Analyzer/Security Hub findings"

### Inputs

- `awsAccountScope`: account or organization scope in review
- `identityInventory`: users, roles, groups, and critical workloads
- `cloudTrailWindow`: time window for access pattern analysis
- `severityPolicy`: risk classification and remediation priorities
- `changeWindow` (optional): execution window for production-impacting changes
- `rollbackPlan`: rollback path for policy/binding regressions

### Invariants

- Least privilege is mandatory; wildcard resources/actions require explicit exception.
- Human access requires strong authentication controls (MFA and constrained session posture).
- Long-lived access keys are minimized and rotated on policy.
- Permission boundaries and organizational guardrails must prevent privilege escalation.
- All IAM hardening actions must be traceable with before/after evidence.

### Procedure

1. **Inventory and baseline**
   - Export credential report and enumerate IAM entities, policies, and key age.
   - Identify stale users/roles/keys and high-risk principals.

2. **Analyze effective permissions**
   - Use IAM Access Analyzer and policy simulation to detect external access and overbroad grants.
   - Generate policy recommendations from CloudTrail activity where possible.

3. **Scope permissions**
   - Replace wildcard ARNs and broad actions with specific resource/action sets.
   - Add policy conditions (MFA, source constraints, context conditions) where appropriate.

4. **Apply boundaries and guardrails**
   - Create/apply permission boundaries for developer/operator roles.
   - Enforce org-level constraints (for example SCP guardrails) where managed centrally.

5. **Reduce credential risk**
   - Disable/rotate long-lived access keys and migrate workloads to role-based temporary credentials.
   - Validate break-glass and emergency access paths remain controlled.

6. **Enable continuous monitoring**
   - Configure AWS Config/Security Hub/EventBridge controls for IAM-risk events.
   - Alert on root usage, policy tampering, and high-risk identity changes.

7. **Validate and stage rollout**
   - Test critical workflows against updated permissions.
   - Roll out progressively with rollback checkpoints.

8. **Close with evidence**
   - Produce findings summary, resolved items, exceptions, and follow-up actions.

### Outputs

- IAM hardening assessment report with prioritized findings.
- Updated/scoped IAM policies and boundary configurations.
- Credential rotation/deactivation log.
- Continuous monitoring rule set and alerting checklist.
- Exception register with owners and review deadlines.

### Review gate

- [ ] High-risk wildcard policies are removed or explicitly justified.
- [ ] Permission boundaries/guardrails are enforced on target roles.
- [ ] Long-lived key exposure risk is reduced and documented.
- [ ] MFA/session safeguards are enforced for human access.
- [ ] Monitoring detects high-risk IAM events with actionable routing.

### References

- `../../SKILL.md`
- `../iac/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
