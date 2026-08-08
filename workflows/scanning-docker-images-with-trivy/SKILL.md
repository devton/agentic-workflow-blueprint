## agentic-workflows-blueprint.workflow.scanning-docker-images-with-trivy

### Goal

Perform comprehensive Docker image security assessment with Trivy, including vulnerabilities, misconfigurations, secrets, and SBOM generation, with clear remediation gates.

### Scope

- Applies to: local or CI-driven Docker image scans before distribution or deployment.
- Does not cover: orchestrator runtime detection and response.

### Triggers

- "Scan Docker image with Trivy"
- "Generate security report/SBOM for image"
- "Check private registry image before release"
- "Enforce critical/high CVE gate"

### Inputs

- `imageRef`: local image, tarball input, or registry image reference
- `scannerSet` (optional): `vuln`, `misconfig`, `secret`, `license`
- `severityPolicy`: severity levels considered blocking
- `outputFormat`: table, JSON, SARIF, CycloneDX, SPDX
- `registryAuthContext` (optional): credentials and target registry
- `ignorePolicy` (optional): approved ignore file/entries

### Invariants

- Scans must clearly identify scanned image digest/tag and DB timestamp.
- Blocking policy must be deterministic and explicit.
- Ignore entries must be reviewed and justified.
- Outputs must remain auditable and machine-consumable when used in gates.

### Procedure

1. **Prepare scan environment**
   - Validate Trivy installation/version and DB update strategy.
   - Ensure image is available locally or accessible from registry.

2. **Run baseline vulnerability scan**
   - Scan image with selected severities and fail policy.
   - Capture artifact in configured format.

3. **Expand scan dimensions as needed**
   - Run additional scanner sets (`misconfig`, `secret`, `license`) where required.
   - Scan Dockerfile/config paths when build-time posture must be validated.

4. **Generate SBOM**
   - Emit SBOM in CycloneDX or SPDX.
   - Link SBOM to scanned image identity.

5. **Apply policy exceptions safely**
   - Apply ignore list only through approved and traceable mechanism.
   - Re-run scan to verify resulting gate behavior.

6. **Assess private registry images**
   - Authenticate and scan protected images.
   - Ensure no credential leakage in logs/artifacts.

7. **Finalize remediation report**
   - Summarize critical/high findings, affected packages, and fixed versions.
   - Mark pass/fail according to `severityPolicy`.

### Outputs

- Trivy vulnerability report for target image.
- Extended scan report (misconfig/secret/license when enabled).
- SBOM artifact for image.
- Gate summary with blocking findings and remediation priorities.

### Review gate

- [ ] Scan identifies image reference and supports reproducibility.
- [ ] Blocking vulnerabilities are surfaced with remediation context.
- [ ] SBOM is generated and attached when required.
- [ ] Exception handling is traceable and policy-compliant.
- [ ] Final pass/fail decision matches configured severity policy.

### References

- `../../SKILL.md`
- `../scanning-containers-with-trivy-in-cicd/SKILL.md`
- `../implementing-devsecops-security-scanning/SKILL.md`
- [Interactive HTML View](./README.html)
