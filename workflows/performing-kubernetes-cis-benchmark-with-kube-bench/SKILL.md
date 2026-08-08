## agentic-workflows-blueprint.workflow.performing-kubernetes-cis-benchmark-with-kube-bench

### Goal

Assess Kubernetes cluster security posture against CIS benchmark controls using kube-bench and produce remediation-prioritized evidence for hardening programs.

### Scope

- Applies to: Kubernetes control plane, worker, etcd, and policy benchmark assessments.
- Does not cover: replacing runtime threat detection or policy enforcement controls.

### Triggers

- "Run Kubernetes CIS benchmark"
- "Assess cluster hardening with kube-bench"
- "Generate compliance evidence for Kubernetes security controls"
- "Track benchmark drift over time"

### Inputs

- `clusterContext`: cluster/provider/version context
- `benchmarkProfile`: target profile (generic, EKS, GKE, AKS, etc.)
- `executionMode`: node/binary run or Kubernetes job mode
- `severityPolicy`: mapping for FAIL/WARN prioritization
- `reportTarget` (optional): destination for JSON/JUnit/text output
- `remediationCapacity` (optional): team constraints for scheduled fixes

### Invariants

- Benchmark profile must match cluster platform/version context.
- Results must be preserved in machine-readable format for trend analysis.
- FAIL findings are prioritized before WARN findings unless justified.
- Remediation recommendations must map to specific CIS control IDs.

### Procedure

1. **Prepare benchmark run**
   - Select benchmark profile and execution mode.
   - Validate required permissions and host/cluster access.

2. **Execute kube-bench**
   - Run full benchmark and optionally component-specific targets.
   - Capture raw output and structured report formats.

3. **Classify findings**
   - Separate PASS/FAIL/WARN outcomes and map by benchmark section.
   - Identify high-risk controls affecting auth, API server, kubelet, and RBAC posture.

4. **Prioritize remediation**
   - Rank findings by security impact and operational risk.
   - Propose sequenced remediation plan with ownership and expected blast radius.

5. **Validate remediations**
   - Re-run impacted controls after configuration changes.
   - Confirm delta improvements and absence of regressions.

6. **Operationalize continuous assessment**
   - Schedule periodic benchmark runs for drift detection.
   - Publish trend metrics and unresolved findings backlog.

### Outputs

- kube-bench result artifacts (text/JSON/JUnit as configured).
- CIS findings summary with FAIL/WARN prioritization.
- Remediation plan mapped to control IDs.
- Re-test evidence for resolved controls.

### Review gate

- [ ] Benchmark profile aligns with platform/version.
- [ ] Structured report artifacts are generated and retained.
- [ ] FAIL findings have remediation owner and plan.
- [ ] Re-validation confirms critical fixes are effective.
- [ ] Continuous benchmark cadence is defined.

### References

- `../../SKILL.md`
- `../securing-kubernetes-on-cloud/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
