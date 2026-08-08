## agentic-workflows-blueprint.workflow.scanning-containers-with-trivy-in-cicd

### Goal

Implement a CI/CD-native container scanning workflow using Trivy that blocks vulnerable images and misconfigurations from promotion based on deterministic severity gates.

### Scope

- Applies to: containerized applications built and promoted through CI/CD pipelines.
- Does not cover: runtime workload protection, host IDS, or live container behavioral monitoring.

### Triggers

- "Add Trivy scan to CI/CD pipeline"
- "Block container releases with critical/high CVEs"
- "Generate SBOM and gate image promotion"
- "Scan Dockerfiles/manifests in pull requests"

### Inputs

- `cicdPlatform`: GitHub Actions, GitLab CI, Jenkins, or equivalent
- `imageRefStrategy`: image naming/tagging approach (for example commit SHA tags)
- `severityPolicy`: threshold policy for fail/pass (for example `CRITICAL,HIGH`)
- `scanTargets`: image, filesystem, and config paths to scan
- `registryContext`: source/target registry and credential model
- `exceptionPolicy` (optional): approved ignore entries and expiry rules
- `sbomFormat` (optional): CycloneDX or SPDX

### Invariants

- No image promotion to protected environments without a successful scan gate.
- Severity gate policy is explicit and versioned in pipeline configuration.
- Ignore/exception entries must include rationale and expiry.
- Scan artifacts must be retained for auditing and trend analysis.
- Trivy DB freshness/caching policy must be explicit to avoid stale assessments.

### Procedure

1. **Define policy and gate behavior**
   - Set fail thresholds (`severityPolicy`) and decide if unfixed vulnerabilities are excluded.
   - Define which branches/events require blocking scans.

2. **Build image deterministically**
   - Build image using reproducible tag strategy (for example `app:${sha}`).
   - Preserve image identifier in logs and artifacts.

3. **Run vulnerability scan on image**
   - Execute Trivy image scan with configured severity threshold and non-zero exit on violation.
   - Publish machine-readable output (JSON/SARIF) for downstream consumers.

4. **Run misconfiguration scan**
   - Execute Trivy config scan over Dockerfiles and infrastructure/config files in scope.
   - Fail pipeline when policy-threshold findings are detected.

5. **Apply exception policy**
   - Load ignore entries from managed file/config.
   - Enforce required rationale and expiry for each exception.

6. **Generate SBOM and optional secondary checks**
   - Produce SBOM in requested format.
   - Optionally rescan SBOM or run license scanner to enrich decision context.

7. **Cache and resilience controls**
   - Configure DB caching for performance while preserving freshness constraints.
   - Define offline/air-gapped fallback process when applicable.

8. **Aggregate and enforce gate**
   - Aggregate image + config scan outcomes.
   - Block push/deploy on policy violation and emit concise remediation summary.

9. **Verification and reporting**
   - Test gate with a known-vulnerable sample image.
   - Confirm clean image path passes and retains artifacts.

### Outputs

- CI/CD pipeline steps for Trivy image/config scanning.
- Vulnerability scan reports (JSON/SARIF/table as configured).
- SBOM artifact for scanned image.
- Gate decision summary (`PASS`/`FAIL`) with actionable findings.
- Exception register entries with rationale and expiration.

### Review gate

- [ ] Image scan blocks findings above configured severity threshold.
- [ ] Misconfiguration scan enforces the same policy rigor as image scan.
- [ ] SBOM is generated and retained for audited builds.
- [ ] Exception handling is traceable and time-bounded.
- [ ] Pipeline blocks promotion when gate fails.

### References

- `../../SKILL.md`
- `../implementing-devsecops-security-scanning/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
