## agentic-workflows-blueprint.workflow.implementing-rbac-hardening-for-kubernetes

### Goal

Harden Kubernetes RBAC by enforcing least privilege, reducing cluster-admin sprawl, and establishing repeatable access audits with remediation gates.

### Scope

- Applies to: Kubernetes RBAC roles, bindings, service accounts, and identity integration patterns.
- Does not cover: non-Kubernetes IAM systems except where required for Kubernetes authentication context.

### Triggers

- "Harden Kubernetes RBAC"
- "Audit cluster-admin and overprivileged bindings"
- "Apply least-privilege roles and service account controls"
- "Integrate OIDC identity governance for Kubernetes access"

### Inputs

- `clusterContext`: target cluster and environment
- `identityModel`: users/groups/service accounts and IdP integration assumptions
- `namespaceOwnershipMap`: team/service ownership by namespace
- `criticalWorkloads`: workloads with elevated operational risk
- `auditEvidencePath` (optional): destination for audit artifacts
- `rollbackPlan`: rollback strategy for breaking RBAC changes

### Invariants

- Least privilege is mandatory: grant only required verbs/resources.
- Namespace-scoped Role/RoleBinding is preferred over broad cluster-wide grants.
- Cluster-admin bindings require explicit justification and periodic review.
- Service accounts must be dedicated per workload where feasible.
- Token auto-mount and secret-access permissions are minimized by default.

### Procedure

1. **RBAC inventory and risk classification**
   - Enumerate ClusterRoleBindings and RoleBindings with subjects and role refs.
   - Classify privileges by risk (escalation, secret access, execution paths).

2. **Identify high-risk patterns**
   - Detect cluster-admin sprawl and overprivileged service accounts.
   - Detect default service account usage in workloads.
   - Detect dangerous permissions (`secrets`, `pods/exec`, `serviceaccounts/token`, RBAC write grants).

3. **Design least-privilege target model**
   - Replace broad cluster roles with namespace-scoped roles where possible.
   - Define dedicated service accounts per workload and restrict token behavior.
   - Map user/group access to operational responsibilities.

4. **Apply hardening changes**
   - Create/update roles and bindings with minimal verbs/resources.
   - Remove or reduce broad bindings in staged manner.
   - Apply OIDC claim mapping guidance where user auth federation is in scope.

5. **Validate authorization behavior**
   - Verify intended allowed actions still work for each principal class.
   - Verify blocked high-risk actions fail as expected.
   - Capture evidence from authorization checks/tools.

6. **Operational rollout and rollback guard**
   - Rollout progressively by namespace or team boundary.
   - Trigger rollback immediately for production-blocking access regressions.

7. **Audit closure**
   - Publish final access matrix, exceptions, and remediation backlog for deferred items.

### Outputs

- RBAC hardening manifests and binding diffs.
- Access audit report with high-risk findings and resolved status.
- Post-change authorization validation evidence.
- Exception register for remaining elevated permissions with owner and expiry.

### Review gate

- [ ] Cluster-admin and other high-risk bindings are minimized and justified.
- [ ] Namespace-scoped least-privilege roles are applied where feasible.
- [ ] Service account usage avoids default or overbroad permissions.
- [ ] Authorization validation confirms intended allow/deny behavior.
- [ ] Exceptions include owner, rationale, and sunset timeline.

### References

- `../../SKILL.md`
- `../os-platform/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
