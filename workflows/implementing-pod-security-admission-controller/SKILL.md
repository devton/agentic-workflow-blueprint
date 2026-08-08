## agentic-workflows-blueprint.workflow.implementing-pod-security-admission-controller

### Goal

Implement Kubernetes Pod Security Admission (PSA) with controlled policy rollout (`audit`, `warn`, `enforce`) to raise workload security posture without destabilizing operations.

### Scope

- Applies to: namespace-level Pod Security Standard enforcement and cluster-level PSA defaults.
- Does not cover: custom policy engines for rules beyond PSA baseline/restricted semantics.

### Triggers

- "Enable Pod Security Admission"
- "Migrate from PodSecurityPolicy to PSA"
- "Enforce baseline/restricted policies per namespace"
- "Audit pod security violations before enforcement"

### Inputs

- `clusterVersion`: Kubernetes version and API server config model
- `namespacePolicyMap`: desired profile per namespace (`privileged`, `baseline`, `restricted`)
- `enforcementStrategy`: rollout sequence and timing by environment
- `exemptionList` (optional): system/critical namespaces with justification
- `changeWindow` (optional): execution window for enforcement changes
- `rollbackPlan`: policy rollback and emergency exemption procedure

### Invariants

- Production namespaces should not jump directly to strict enforcement without audit evidence.
- Enforcement versions must be pinned for deterministic behavior across upgrades.
- Exemptions must be minimal, explicit, and justified.
- Any breaking enforcement change requires rollback readiness before rollout.

### Procedure

1. **Assess current workload posture**
   - Inventory namespace workloads and identify likely PSA violations.
   - If migrating from PSP, map existing controls to PSA profiles.

2. **Define target policy map**
   - Assign namespace profiles: system namespaces (`privileged` when justified), staging (`baseline`), production (`restricted` target).
   - Define pinned version labels for enforce/audit/warn.

3. **Start with audit and warn**
   - Apply `audit` and `warn` labels before `enforce` in target namespaces.
   - Collect warning/audit evidence and prioritize remediations.

4. **Remediate violating workloads**
   - Update securityContext and pod specs to meet target profile.
   - Validate restricted/baseline compliance for critical workloads.

5. **Enable enforcement progressively**
   - Apply `enforce` per namespace in staged sequence aligned to `changeWindow`.
   - Monitor admission rejections and service health after each stage.

6. **Configure cluster defaults (optional)**
   - When required, configure API server admission control defaults and controlled exemptions.
   - Validate control-plane behavior after config rollout.

7. **Verify and stabilize**
   - Confirm pod creations match expected allow/deny behavior.
   - Document residual exceptions and follow-up remediation actions.

### Outputs

- Namespace PSA label plan and applied label evidence.
- Violation/remediation report from audit and warn phases.
- Enforcement rollout report with accepted/rejected workload outcomes.
- Exemption register with owner, reason, and review date.

### Review gate

- [ ] Target policy profile per namespace is explicit and version-pinned.
- [ ] Audit/warn evidence exists before strict enforcement in sensitive namespaces.
- [ ] Violating workloads are remediated or formally excepted.
- [ ] Enforcement rollout is staged and operationally validated.
- [ ] Exemptions are justified, minimal, and tracked.

### References

- `../../SKILL.md`
- `../os-platform/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
