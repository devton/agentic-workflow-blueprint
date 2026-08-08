## agentic-workflows-blueprint.workflow.implementing-network-policies-for-kubernetes

### Goal

Implement Kubernetes NetworkPolicies that enforce zero-trust segmentation with deterministic validation and rollback-safe rollout controls.

### Scope

- Applies to: namespace/workload ingress-egress policy design and enforcement in Kubernetes clusters with compatible CNI.
- Does not cover: perimeter firewall management outside Kubernetes networking scope.

### Triggers

- "Implement Kubernetes NetworkPolicies"
- "Apply default deny and allow-list traffic"
- "Segment workloads with least-privilege network access"
- "Block metadata endpoint and lateral movement"

### Inputs

- `clusterContext`: target cluster and environment tier
- `targetNamespaces`: namespaces in scope
- `cniProvider`: Calico, Cilium, Antrea, or equivalent
- `serviceFlowMap`: expected workload-to-workload and workload-to-external traffic matrix
- `changeWindow` (optional): maintenance window for production rollout
- `rollbackPlan`: rollback procedure for policy changes

### Invariants

- Default-deny stance is the baseline for protected namespaces unless explicitly exempted.
- Allow rules must be minimal and tied to explicit service flows.
- DNS and control-plane dependencies must be explicitly preserved.
- Cloud metadata endpoints must remain blocked for untrusted workloads.
- Validation commands must prove both blocked and allowed paths before closure.

### Procedure

1. **Baseline and discovery**
   - Inventory namespace/workload labels and communication dependencies.
   - Build/verify `serviceFlowMap` before writing rules.

2. **Apply default-deny foundation**
   - Create ingress+egress deny-all policy for each target namespace.
   - Confirm workloads are isolated before selective allow rules.

3. **Allow mandatory platform traffic**
   - Add DNS egress and any platform-critical control traffic explicitly.
   - Validate service discovery continues to operate.

4. **Implement application-specific allow rules**
   - Define podSelector/namespaceSelector rules for allowed app flows.
   - Restrict ports/protocols to least-privilege values only.

5. **Add cross-namespace and egress controls**
   - Allow only required observability and shared-service ingress.
   - Restrict external egress with explicit CIDR/port allow-list.

6. **Block metadata endpoint access**
   - Enforce egress exclusions for known cloud metadata addresses.
   - Validate SSRF-style metadata access is blocked.

7. **Validate behavior**
   - Run connectivity tests proving denied paths fail.
   - Run connectivity tests proving required paths succeed.
   - Capture test outputs as evidence.

8. **Rollout and rollback readiness**
   - Promote policies by environment with monitored checkpoints.
   - If a critical dependency breaks, execute rollback immediately.

### Outputs

- Versioned NetworkPolicy manifests per namespace/application.
- Validation report with blocked/allowed traffic evidence.
- Change summary including known exceptions and risk notes.
- Rollback execution note (if used) or rollback readiness confirmation.

### Review gate

- [ ] Default-deny policy exists for each protected namespace.
- [ ] All allow rules map to explicit documented service flows.
- [ ] DNS and required platform dependencies remain functional.
- [ ] Metadata endpoint access is blocked where required.
- [ ] Validation evidence proves both deny and allow outcomes.

### References

- `../../SKILL.md`
- `../network-engineering/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
