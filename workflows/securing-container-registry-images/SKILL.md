## agentic-workflows-blueprint.workflow.securing-container-registry-images

### Goal

Secure container image registries by enforcing vulnerability scanning, image signing, SBOM traceability, and promotion gates that prevent unsafe artifacts from deployment.

### Scope

- Applies to: image build/push/promotion controls in registries such as ECR, ACR, GCR/Artifact Registry, and equivalent platforms.
- Does not cover: runtime detection on deployed containers.

### Triggers

- "Secure container registry images"
- "Require scan and signature before image promotion"
- "Implement SBOM and registry hardening controls"
- "Audit registry for unscanned/unsigned images"

### Inputs

- `registryPlatform`: target registry provider(s)
- `repositoryScope`: repositories/tags/environments in scope
- `severityPolicy`: blocking thresholds for vulnerabilities
- `signingModel`: key-based or keyless signing approach
- `sbomPolicy`: required SBOM format and retention target
- `promotionFlow`: dev -> staging -> production image progression
- `exceptionPolicy` (optional): approved risk exceptions with expiry

### Invariants

- Images must not be promoted to protected environments without passing scan policy.
- Image authenticity must be verifiable (signature and provenance expectations).
- Mutable tag risk must be controlled (immutability or digest-based promotion).
- SBOM and scan artifacts must be retained and linked to image digests.
- Exceptions must be explicit, reviewed, and time-bounded.

### Procedure

1. **Baseline registry security posture**
   - Inventory repositories, scan settings, mutability rules, and retention policies.
   - Identify unscanned, unsigned, and high-risk images.

2. **Enforce vulnerability scanning**
   - Run Trivy and optional secondary scanner coverage (for example Grype) on target images.
   - Apply fail thresholds from `severityPolicy`.

3. **Generate and persist SBOM**
   - Generate SBOM artifacts (SPDX/CycloneDX) for promoted images.
   - Attach or store SBOM with immutable digest association.

4. **Implement signing and verification**
   - Sign images using configured signing model (keyed or keyless).
   - Verify signatures before promotion and/or admission.

5. **Harden registry controls**
   - Enable scan-on-push and tag immutability where supported.
   - Configure lifecycle and retention policies for hygiene.
   - Tighten registry IAM/RBAC access boundaries.

6. **Integrate CI/CD promotion gate**
   - Require scan + signature + SBOM checks before pushing/promoting release artifacts.
   - Block promotion on policy failures and emit remediation summary.

7. **Continuous reassessment**
   - Re-scan promoted artifacts on vulnerability DB updates.
   - Track drift between current findings and promotion-time status.

### Outputs

- Registry security posture report with prioritized gaps.
- Scan artifacts and vulnerability summary per image digest.
- Signed image verification evidence and attestation metadata.
- SBOM inventory linked to promoted artifacts.
- Promotion gate report with pass/fail and exception details.

### Review gate

- [ ] All promoted images pass vulnerability policy thresholds.
- [ ] Signature verification is enforced in promotion path.
- [ ] SBOM is generated and traceable to image digests.
- [ ] Tag immutability/digest controls prevent silent artifact replacement.
- [ ] Exceptions are documented with rationale, owner, and expiry.

### References

- `../../SKILL.md`
- `../scanning-docker-images-with-trivy/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
