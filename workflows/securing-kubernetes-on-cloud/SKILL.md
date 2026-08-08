## agentic-workflows-blueprint.workflow.securing-kubernetes-on-cloud

### Goal

Harden managed Kubernetes clusters on major cloud providers by enforcing pod security, workload identity, network segmentation, RBAC constraints, image admission controls, and runtime monitoring.

### Scope

- Applies to: EKS, AKS, GKE, or equivalent managed Kubernetes platforms.
- Does not cover: non-Kubernetes compute orchestration models.

### Triggers

- "Secure managed Kubernetes cluster"
- "Harden EKS/AKS/GKE for production"
- "Enforce workload identity and pod security standards"
- "Add cloud-native Kubernetes security guardrails"

### Inputs

- `provider`: EKS, AKS, GKE (or mapped equivalent)
- `clusterContext`: cluster name/environment/region
- `namespaceSecurityMap`: namespace-by-namespace target security posture
- `identityStrategy`: IRSA, Workload Identity, Managed Identity mapping plan
- `networkPolicyStrategy`: default-deny and explicit allow-list model
- `admissionPolicyStrategy` (optional): Kyverno/Gatekeeper policy requirements
- `runtimeMonitoringStrategy` (optional): Falco/eBPF/security telemetry integration
- `rollbackPlan`: rollback for blocking policy changes

### Invariants

- Production namespaces require strict pod security posture with staged rollout.
- Static cloud credentials in pods are disallowed when workload identity is available.
- East-west traffic must be segmented by least privilege.
- RBAC access must avoid broad cluster-level grants for non-admin identities.
- Admission controls and runtime monitoring must complement static hardening.

### Procedure

1. **Security baseline assessment**
   - Inventory cluster security posture (PSA labels, RBAC, service accounts, network policies, admission controls).
   - Identify provider-specific identity and control-plane constraints.

2. **Enforce pod security standards**
   - Apply namespace policy map (`audit/warn/enforce`) with staged rollout.
   - Remediate non-compliant workloads before strict enforcement.

3. **Implement workload identity**
   - Bind Kubernetes service accounts to cloud IAM identities (IRSA/GKE WI/AKS MI).
   - Remove static secrets/credentials used for cloud API access.

4. **Segment network traffic**
   - Apply default-deny and explicit allow-list network policies.
   - Validate service dependencies and DNS/control-plane paths.

5. **Harden RBAC**
   - Reduce overprivileged roles/bindings.
   - Align namespace roles to team/service responsibilities.

6. **Apply image admission controls**
   - Restrict allowed registries and require immutable digest references.
   - Enforce policy failures before workload admission.

7. **Enable runtime monitoring**
   - Deploy runtime threat detection and benchmark tooling as defined.
   - Route alerts with severity-driven triage paths.

8. **Validate and operationalize**
   - Execute security and functional validation checks after each hardening phase.
   - Publish final risk register, exceptions, and follow-up remediation work.

### Outputs

- Cluster hardening plan and execution evidence by control area.
- Namespace security policy and compliance report.
- Workload identity migration report.
- Network/RBAC/admission control validation results.
- Runtime monitoring enablement and alert routing summary.

### Review gate

- [ ] Pod security policies are applied with staged enforcement and evidence.
- [ ] Workload identity replaces static cloud credentials in target workloads.
- [ ] Network and RBAC controls enforce least-privilege boundaries.
- [ ] Admission controls enforce trusted registry/digest policy.
- [ ] Runtime monitoring is active with documented triage flow.

### References

- `../../SKILL.md`
- `../implementing-pod-security-admission-controller/SKILL.md`
- `../implementing-network-policies-for-kubernetes/SKILL.md`
- `../implementing-rbac-hardening-for-kubernetes/SKILL.md`
- [Interactive HTML View](./README.html)
