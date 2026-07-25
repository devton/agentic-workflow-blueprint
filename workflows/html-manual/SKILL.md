---
name: html-manual
description: Standalone single-file interactive visual HTML manual generator for skills, workflows, reference docs, and runbooks using Tailwind CSS CDN and glassmorphism styling.
---

# "agentic-workflows-blueprint.workflow.html-manual"

## Goal
Generate standalone, single-file interactive HTML visual manuals (`README.html` for skills/workflows and `<filename>.html` for references/runbooks) that mirror markdown contracts using Tailwind CSS CDN, dark mode, and glassmorphic styling for human readability.

---

## Scope
- **Applies to**: Any skill (`SKILL.md`), workflow contract, reference document (`.md`), or runbook (`.md`) in any project.
- **Does not cover**: Building full web applications or dynamic client-side JS SPAs.

---

## Triggers
- "Generate HTML manual"
- "Create README.html for skill"
- "Build visual documentation"
- "Generate HTML version of runbook or reference"
- "/html-manual [target path]"

---

## Inputs
- `targetPath`: Path to the source markdown file (`SKILL.md`, `routing-matrix.md`, `agent-role-system.md`, etc.).
- `projectTitle`: Human-readable name of the project or workflow.

---

## Invariants (Guardrails)
1. **Single-File Self-Contained HTML**: Output must be 100% self-contained in one `.html` file using Tailwind CSS CDN (`<script src="https://cdn.tailwindcss.com"></script>`).
2. **Sleek Dark Mode Aesthetic**: Use dark background (`bg-zinc-950 text-zinc-100`), Jakarta Sans font, mono font for code, and glassmorphic cards (`background: rgba(24, 24, 27, 0.65); backdrop-filter: blur(12px); border: 1px solid rgba(255, 255, 255, 0.08)`).
3. **Exact Filename Convention**:
   - For skill/workflow directories: Output filename MUST be `README.html` in the same folder as `SKILL.md`.
   - For reference or runbook files (`<name>.md`): Output filename MUST be `<name>.html` matching the exact base name in the same folder (e.g., `routing-matrix.md` ➔ `routing-matrix.html`, `agent-role-system.md` ➔ `agent-role-system.html`).
4. **Reference Link Back**: Every source `.md` file MUST link to its `.html` companion under `## References` (e.g. `[Interactive HTML View](./README.html)` or `[Visual HTML Version](./routing-matrix.html)`).
5. **Exact Content Sync**: The HTML version must faithfully reflect all goals, scopes, procedures, invariants, and tables from the source markdown file.

---

## Procedure

1. **Parse Source Markdown**:
   - Read the target `.md` file (`SKILL.md`, runbook, or reference file).
   - Extract title, goal, scope, invariants, procedure steps, tables, and references.

2. **Determine Output Path**:
   - If target is `SKILL.md`: Output path is `README.html` in the same directory.
   - If target is `<name>.md`: Output path is `<name>.html` in the same directory.

3. **Generate HTML Document**:
   - Build HTML boilerplate with Tailwind CSS CDN script and Jakarta Sans font.
   - Construct Header Banner with icon, project title, and navigation shortcuts.
   - Construct Executive Summary / Goal Card with glassmorphism CSS.
   - Construct Scope & Invariants Grid with color-coded borders (green for scope, blue/purple/amber for invariants).
   - Construct Procedure Steps / Tables with custom badges (`GET`, `POST`, `DELETE`, `Phase 1`, `Phase 2`, etc.).
   - Construct Review Gate Checklist with checkbox indicators.

4. **Link HTML in Source Markdown**:
   - Open source `.md` file.
   - Add `[Interactive HTML View](./README.html)` (or `./<name>.html`) under `## References`.

5. **Verification**:
   - Verify the generated `.html` file exists and contains valid HTML structure.

---

## Outputs
- Single-file `<filename>.html` or `README.html` saved alongside the source markdown file.
- Updated source `.md` file with reference link.

---

## Review Gate
- [ ] Output HTML is self-contained single file with Tailwind CSS CDN?
- [ ] Filename matches convention (`README.html` for skills, `<name>.html` for references/runbooks)?
- [ ] Dark mode & glassmorphic styling applied?
- [ ] Source `.md` file updated with link under `## References`?

---

## References
- [Main Skill](../../SKILL.md)
- [Interactive HTML View](./README.html)
