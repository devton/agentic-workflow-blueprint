---
name: html-manual
description: Standalone single-file interactive visual HTML manual generator for skills, workflows, reference docs, and runbooks using Tailwind CSS CDN, Light/Dark Mode theme switcher, recursive sidebar file tree, and organic Mermaid graph mesh.
---

## agentic-workflows-blueprint.workflow.html-manual

## Goal
Generate standalone, single-file interactive HTML visual manuals (`README.html` for skills/workflows and `<filename>.html` for references/runbooks) that mirror markdown contracts using Tailwind CSS CDN, Light/Dark mode theme switcher (persisted via `sessionStorage`), recursive sidebar file tree explorer, and organic Mermaid graph network mesh for maximum human readability and navigation.

---

## Scope
- **Applies to**: Any skill (`SKILL.md`), workflow contract, reference document (`.md`), or runbook (`.md`) in any agentic workflow skill system.
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
2. **Light/Dark Mode Theme Switcher (`sessionStorage`)**: Every page MUST feature an interactive Theme Toggle button (`☀️ Light Mode` / `🌙 Dark Mode`) in the header that persists the user's preference in `sessionStorage.getItem('apix_theme')` across page navigations.
3. **Ultra-Minimalist Aesthetic (Light & Dark Tones)**: Light mode uses clean light tones (`bg-gray-50 text-gray-800`), white cards (`bg-white border-gray-200/80 shadow-2xs`). Dark mode uses sleek dark tones (`dark:bg-gray-950 dark:text-gray-100`), dark cards (`dark:bg-gray-900 dark:border-gray-800`).
4. **1 Card Per Row (Full Width Stack)**: Every card (Overview, Triggers, Guardrails, Procedures, Review Gate) MUST occupy 100% width (`w-full` / single column) so long descriptions, lists, and tables have maximum room without squeezing.
5. **Overview & Contextual Sub-Graph Open by Default (`<details open>`)**: The `Goal & System Overview` and `Contextual Dependency Sub-Graph` cards MUST open automatically (`<details open>`) when any manual is loaded so the user gets instant context. All other detail sections remain closed by default (`<details>`).
6. **Flat Unnested Hierarchy (No Card Inside Card)**: Content inside sections (tables, code, lists) MUST render directly on the flat card background without adding extra nested card borders.
7. **Recursive Sidebar File Tree Explorer**: Every page MUST include an interactive left sidebar file tree (`w-80 border-r border-gray-200 bg-white dark:bg-gray-900`) with collapsible folder nodes (`📁 reference/`, `📁 workflows/`) and file links (`📄 SKILL.md`). The folder containing the active file auto-expands (`open`) and the active document is highlighted.
8. **Organic Skill Mesh Graph (`GRAPH.html`)**: The root skill directory MUST include a dedicated `GRAPH.html` page rendering a pure organic network mesh map based strictly on true direct markdown cross-links between skills (no artificial hub nodes, no subgraph boxes).
9. **Compact Horizontal Flow (`graph LR`) with Close-up Zoom & RankSpacing**: Diagrams MUST use a horizontal flow (`graph LR`) with reduced canvas height (`h-[600px]` for `GRAPH.html`, `h-[340px]` for sub-graphs), tight `rankSpacing`, close-up initial zoom (`zoom(1.25)` / `zoom(1.35)`), and `svg-pan-zoom` so nodes are projected forward smoothly without vertical waste.
10. **Mermaid Diagram Sanitization**: Any ```mermaid block MUST have diacritics/accents removed from node labels/identifiers, and unquoted labels enclosed in double quotes (e.g. `node["Label Text"]`) to prevent syntax parsing errors.
11. **Syntax Highlighting (Prism.js)**: Code blocks MUST use Prism.js for clean syntax highlighting across Ruby, TypeScript, JSON, Bash, and HTML.
12. **Visual File Trees**: Folder/file tree outputs (`├──`, `└──`, paths) MUST be parsed into clean interactive file tree components with folder `📁` and file `📄` icons.
13. **Exact Filename Convention**:
   - For skill/workflow directories: Output filename MUST be `README.html` in the same folder as `SKILL.md`.
   - For reference or runbook files (`<name>.md`): Output filename MUST be `<name>.html` matching the exact base name in the same folder (e.g., `routing-matrix.md` ➔ `routing-matrix.html`).
14. **Reference Link Back**: Every source `.md` file MUST link to its `.html` companion under `## References` (e.g. `[Interactive HTML View](./README.html)` or `[Visual HTML Version](./routing-matrix.html)`).

---

## Procedure

1. **Parse Source Markdown**:
   - Read the target `.md` file (`SKILL.md`, runbook, or reference file).
   - Extract title, goal, scope, invariants, procedure steps, tables, metadata frontmatter, and references.

2. **Build Theme Switcher & Head Scripts**:
   - Add inline theme script in `<head>` reading `sessionStorage.getItem('apix_theme')`.
   - Add Tailwind CDN script with `darkMode: 'class'`.
   - Add Prism.js and Mermaid + `svg-pan-zoom` scripts.

3. **Construct UI Component Stack**:
   - Build Theme Toggle button (`☀️ Light Mode` / `🌙 Dark Mode`) in header.
   - Build Recursive Sidebar File Tree Explorer with auto-expanding active folder.
   - Construct `Goal & System Overview` card (`<details open>`).
   - Construct `Contextual Dependency Sub-Graph` card (`<details open>`) with `svg-pan-zoom` controls.
   - Construct flat full-width cards for Triggers, Guardrails, Procedures, and Review Gate (`<details>` closed by default).

4. **Sanitize Diagrams & Code Blocks**:
   - Run accent transliteration on Mermaid diagram labels.
   - Wrap unquoted node labels in double quotes (`node["Label Text"]`).
   - Render ASCII directory trees into visual file tree components.

5. **Generate System Graph (`GRAPH.html`)**:
   - Extract real cross-reference links between all markdown files in the skill tree.
   - Generate `GRAPH.html` with an organic Mermaid network mesh (`graph LR`, `svg-pan-zoom`).

6. **Link HTML in Source Markdown**:
   - Add `[Interactive HTML View](./README.html)` (or `./<name>.html`) under `## References` in source `.md` files.

---

## Outputs
- Single-file `<filename>.html` or `README.html` saved alongside the source markdown file.
- `GRAPH.html` in root skill directory with organic mesh map.
- Updated source `.md` file with reference link.

---

## Review gate
- [ ] Output HTML is self-contained single file with Tailwind CSS CDN?
- [ ] Theme toggle switches between Light and Dark mode and persists in `sessionStorage`?
- [ ] Overview and Contextual Sub-Graph are open by default?
- [ ] Recursive sidebar file tree auto-expands active folder?
- [ ] Organic `GRAPH.html` generated with real link connections and `svg-pan-zoom` controls?
- [ ] Source `.md` file updated with link under `## References`?

---

## References
- [Interactive HTML View](./README.html)
- [System Architecture Graph](../../GRAPH.html)
