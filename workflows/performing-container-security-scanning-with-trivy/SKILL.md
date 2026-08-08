## agentic-workflows-blueprint.workflow.performing-container-security-scanning-with-trivy

### Goal

Run comprehensive Trivy-based container security scanning across images, filesystem sources, and configuration artifacts with policy-driven enforcement outputs.

### Scope

- Applies to: Trivy scans for vulnerabilities, misconfigurations, secrets, and license findings.
- Does not cover: replacing runtime threat detection and incident response execution.

### Triggers

- "Run full Trivy container security scan"
- "Scan image/filesystem/manifests with Trivy"
- "Generate SBOM and security findings from container artifacts"
- "Apply Trivy scanning in security verification flow"

### Inputs

- `scanTargets`: image references, filesystem paths, and config directories
- `scannerModes`: vulnerability/misconfig/secret/license scan set
- `severityPolicy`: blocking and reporting thresholds
- `outputFormats`: JSON/SARIF/table/SBOM formats
- `ciIntegration` (optional): pipeline integration mode and gate behavior
- `exceptionPolicy` (optional): approved ignores with rationale and expiry

### Invariants

- Scan mode and severity policy must be explicit and reproducible.
- Findings above gate threshold must trigger blocking status where enforcement is enabled.
- SBOM and report artifacts must map to target digest/path identity.
- Ignore entries cannot be permanent or unexplained.

### Procedure

1. **Define scan scope**
   - Confirm image and path targets plus required scanner modules.
   - Pin scan policy version and output schema expectations.

2. **Execute primary scans**
   - Run Trivy over image and filesystem targets.
   - Run config scanning for Docker/Kubernetes/IaC paths.

3. **Collect and normalize findings**
   - Export findings in machine-readable format.
   - Normalize severity and issue categories for triage workflows.

4. **Generate SBOM**
   - Emit SPDX/CycloneDX artifact linked to scan target.

5. **Enforce policy gate**
   - Apply threshold evaluation and determine pass/fail.
   - Produce actionable remediation list for blocking findings.

6. **Integrate and publish**
   - Publish artifacts to CI/security systems.
   - Add tuning notes for recurring false positives.

### Outputs

- Consolidated Trivy findings report by target and severity.
- SBOM artifact tied to scan subject.
- Gate decision output with remediation priorities.
- Exception/tuning register updates.

### Review gate

- [ ] Scan coverage includes all required target classes.
- [ ] Blocking threshold findings are enforced consistently.
- [ ] SBOM and findings artifacts are traceable and retained.
- [ ] Exception handling is justified and time-bounded.
- [ ] Output is ready for downstream triage automation.

### References

- `../../SKILL.md`
- `../scanning-docker-images-with-trivy/SKILL.md`
- `../scanning-containers-with-trivy-in-cicd/SKILL.md`
- [Interactive HTML View](./README.html)
