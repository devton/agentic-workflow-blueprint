## agentic-workflows-blueprint.workflow.performing-container-image-hardening

### Goal

Harden container images by reducing attack surface, enforcing non-root and immutable runtime constraints, and validating security posture with repeatable checks.

### Scope

- Applies to: Docker/OCI image build pipelines and runtime-ready image hardening baselines.
- Does not cover: host daemon hardening or runtime threat monitoring controls.

### Triggers

- "Harden container image for production"
- "Reduce image CVE surface"
- "Apply non-root/read-only/container hardening best practices"
- "Implement multi-stage secure Docker builds"

### Inputs

- `imageBuildContext`: Dockerfile/build context and runtime requirements
- `baseImageStrategy`: minimal/distroless/scratch policy and digest pinning approach
- `securityBaseline`: required hardening controls (non-root, read-only fs, capability drop)
- `validationPolicy`: scanner checks and pass/fail thresholds
- `runtimePlatform` (optional): Kubernetes/container runtime constraints

### Invariants

- Production images must avoid unnecessary packages and tooling.
- Images must run as non-root unless explicitly justified.
- Base images should be pinned by digest for reproducibility and trust.
- Security validation must include vulnerability and configuration checks.
- Hardening cannot break application runtime requirements without mitigation plan.

### Procedure

1. **Assess current image posture**
   - Measure size, package inventory, and current vulnerability baseline.
   - Identify unnecessary build/runtime dependencies.

2. **Apply multi-stage build design**
   - Separate builder and runtime stages.
   - Copy only required runtime artifacts into final image.

3. **Minimize runtime footprint**
   - Use slim/distroless/scratch-compatible base strategy.
   - Remove unnecessary package managers, shells, docs, and setuid/setgid binaries when feasible.

4. **Enforce identity and privilege controls**
   - Create and run as non-root user/group.
   - Configure least-privilege runtime flags and capability drops.

5. **Apply filesystem and process hardening**
   - Design for read-only root filesystem plus explicit writable mounts.
   - Add health checks and deterministic entrypoints.

6. **Pin trust anchors**
   - Pin base image by digest.
   - Track update cadence for base image refresh.

7. **Validate hardening outcomes**
   - Run vulnerability/config scans and compare before/after posture.
   - Verify runtime behavior (non-root, read-only expectations, app liveness).

8. **Publish hardening report**
   - Document posture improvements, residual risks, and deferred remediations.

### Outputs

- Hardened Dockerfile/image build spec.
- Before/after hardening report (size and vulnerability delta).
- Validation evidence for non-root, filesystem policy, and scanner results.
- Residual risk register with follow-up actions.

### Review gate

- [ ] Final image uses minimized trusted base strategy.
- [ ] Non-root and least-privilege settings are enforced.
- [ ] Read-only/root-hardening controls are validated where applicable.
- [ ] Vulnerability/config checks meet policy thresholds.
- [ ] Hardening changes preserve required application behavior.

### References

- `../../SKILL.md`
- `../scanning-docker-images-with-trivy/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
