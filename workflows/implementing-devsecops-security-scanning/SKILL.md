## agentic-workflows-blueprint.workflow.implementing-devsecops-security-scanning

### Goal

Implement and operationalize a full DevSecOps security scanning pipeline that combines secrets detection, SAST, SCA, container scanning, optional DAST, and deterministic security gates in CI/CD.

### Scope

- Applies to: repositories and delivery pipelines that need repeatable automated security controls before merge/deploy.
- Does not cover: replacing manual penetration testing, threat modeling workshops, or production incident response.

### Triggers

- "Set up DevSecOps scanning in CI/CD"
- "Integrate SAST, SCA, and DAST"
- "Add security gates for pull requests"
- "Shift-left security pipeline implementation"

### Inputs

- `cicdPlatform`: GitHub Actions, GitLab CI, Jenkins, or Azure DevOps
- `targetBranches`: branches that enforce security checks (for example `main`, `develop`)
- `severityPolicy`: pass/fail thresholds (for example block on `CRITICAL,HIGH`)
- `stagingUrl` (optional): required when enabling DAST
- `containerBuildContext` (optional): image build path/tag strategy
- `iacPaths` (optional): Terraform/CloudFormation/Kubernetes paths for config scanning
- `repoSecurityContext`: branch protection requirements, code owner policy, and approval gates

### Invariants

- Security scanning must be deterministic and reproducible from repository state.
- Secrets detection runs before deeper scans and blocks immediately on confirmed secret leaks.
- Merge/deploy gates must be explicit and based on configured severity policy.
- Findings and artifacts must be preserved for auditability (JSON/SARIF/SBOM as configured).
- DAST is optional in PR flow but mandatory for release hardening flows when `stagingUrl` exists.
- This workflow augments, not replaces, manual offensive testing for business logic risk.

### Procedure

1. **Establish policy and execution boundaries**
   - Confirm `targetBranches`, `severityPolicy`, and which scan stages are mandatory per branch.
   - Define fail-fast behavior for secrets and critical findings.
   - Define artifact retention and report destinations.

2. **Implement secrets detection (Gitleaks)**
   - Add a secrets scanning stage that runs on pull requests and branch pushes.
   - Configure allowlist and custom secret patterns where needed.
   - Ensure pipeline exits non-zero on confirmed credential leaks.

3. **Implement SAST (Semgrep)**
   - Configure Semgrep rulesets (`security-audit`, `owasp`, and organization custom rules).
   - Emit machine-readable output (JSON/SARIF) for reviewer visibility and trend tracking.
   - Fail according to `severityPolicy`.

4. **Implement SCA and IaC scanning (Trivy)**
   - Scan repository dependencies/filesystem (`trivy fs`) for vulnerable packages and secrets.
   - Scan infrastructure/config artifacts (`trivy config`) when `iacPaths` are present.
   - Publish scan outputs as CI artifacts and enforce fail thresholds.

5. **Implement container image scanning + SBOM**
   - Build target image(s) with deterministic tags (for example commit SHA).
   - Scan image(s) with Trivy using the same severity policy.
   - Generate SBOM (CycloneDX or SPDX) and publish as artifact.

6. **Implement DAST stage (OWASP ZAP) when applicable**
   - If `stagingUrl` exists, add baseline DAST for PR/release candidate verification.
   - For periodic deep coverage, add scheduled full scans outside fast PR checks.
   - Normalize DAST results to the same gate semantics used by other stages.

7. **Create aggregate security gate**
   - Add a gate job that consolidates outcomes from secrets, SAST, SCA, container, and optional DAST.
   - Block merge/deploy when any mandatory stage fails policy.
   - Produce a concise gate summary for reviewers.

8. **Enforce branch protection and ownership controls**
   - Configure required status checks matching all mandatory stages.
   - Enforce branch update requirements and workflow/codeowner review controls.
   - Verify bypass paths are disabled unless explicitly approved.

9. **Shift-left developer feedback loop**
   - Add local pre-commit hooks (for example Gitleaks and Semgrep).
   - Provide quick-start local commands to reproduce CI findings before push.
   - Document remediation patterns for common findings.

10. **Verification and closure**
   - Run controlled tests with seeded known-bad examples (dummy secret, vulnerable dependency) to verify gates trigger correctly.
   - Confirm expected pass behavior on clean baseline.
   - Record final configuration and open risk exceptions.

### Outputs

- CI/CD security pipeline configuration in repository workflow files.
- Policy baseline document with severity thresholds and gate behavior.
- Scan artifacts (Semgrep, Trivy fs/config/image, optional ZAP outputs, SBOM).
- Aggregate security gate report with pass/fail rationale.
- Branch protection checklist and local developer feedback instructions.

### Review gate

- [ ] Secrets scanning blocks confirmed credential leaks.
- [ ] SAST, SCA, and container scans run automatically on configured branches.
- [ ] Gate behavior matches `severityPolicy` and blocks critical/high findings as configured.
- [ ] Optional DAST is implemented when `stagingUrl` is available or explicitly deferred with rationale.
- [ ] SBOM is generated and stored for audited builds.
- [ ] Branch protection requires security checks before merge.
- [ ] Developers can reproduce core checks locally with documented commands.

### References

- `../../SKILL.md`
- `../review/SKILL.md`
- `../document/SKILL.md`
- [Interactive HTML View](./README.html)
