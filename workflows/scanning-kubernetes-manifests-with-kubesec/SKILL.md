## agentic-workflows-blueprint.workflow.scanning-kubernetes-manifests-with-kubesec

### Goal

Assess Kubernetes manifests with Kubesec to detect insecure pod/workload configurations and enforce policy thresholds before deployment.

### Scope

- Applies to: Kubernetes YAML/JSON manifests, including workloads and related deployment artifacts.
- Does not cover: live runtime behavioral threat detection after workloads are deployed.

### Triggers

- "Scan Kubernetes manifests with Kubesec"
- "Block insecure K8s manifests in CI"
- "Validate pod security controls before deploy"
- "Add static manifest security gate"

### Inputs

- `manifestPaths`: files/directories containing Kubernetes manifests
- `scorePolicy`: numeric/qualitative threshold policy (for example reject score < 0)
- `executionMode`: CLI binary, Docker, plugin, or HTTP API
- `pipelineContext` (optional): CI platform and artifact/reporting requirements
- `namespaceContext` (optional): target namespaces or environment tiers

### Invariants

- Every scanned resource must emit an explicit score and advisory list.
- Gate policy must be deterministic and documented.
- Critical insecure patterns (privileged, host PID/network, dangerous capabilities) are non-negotiable blockers.
- Results must be retained for review and remediation tracking.

### Procedure

1. **Prepare scan environment**
   - Select execution mode (local CLI, container, plugin, or API endpoint).
   - Validate tool availability and version.

2. **Define scoring and fail policy**
   - Set `scorePolicy` and advisory severity interpretation.
   - Define fail-fast behavior for critical controls.

3. **Scan manifests**
   - Run Kubesec on each target manifest.
   - Capture structured output (JSON preferred for automation).

4. **Evaluate key control categories**
   - Privilege and root controls.
   - Capabilities and host namespace usage.
   - Volume mount and filesystem hardening controls.
   - Resource limits/requests and service account hygiene.
   - Seccomp/AppArmor/SELinux posture where applicable.

5. **Enforce CI/CD gate**
   - Integrate scan step into PR/build pipeline.
   - Fail pipeline on score/policy breach.

6. **Optional admission enforcement**
   - Where required, integrate as validating webhook to enforce policy at admission.
   - Ensure fail-closed behavior is intentional and staged by environment.

7. **Report and remediation**
   - Produce actionable findings with manifest path and control IDs.
   - Generate prioritized remediation plan for blocked resources.

### Outputs

- Kubesec scan report for all manifest targets.
- Gate decision summary with score threshold outcome.
- Findings list mapped to manifest resources and remediation actions.
- Optional admission policy integration notes.

### Review gate

- [ ] All manifest targets are scanned with auditable output.
- [ ] Critical policy violations trigger blocking outcome.
- [ ] Score/advisory thresholds are clearly enforced.
- [ ] Findings include direct remediation guidance.
- [ ] Pipeline/admission integration behavior is documented.

### References

- `../../SKILL.md`
- `../implementing-network-policies-for-kubernetes/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
