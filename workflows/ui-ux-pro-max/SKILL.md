---
name: ui-ux-pro-max
description: Generate professional UI/UX design systems, palettes, typography, and responsive component guidance.
---

## agentic-workflows-blueprint.workflow.ui-ux-pro-max

### Goal

Produce a visual design system, color palette, typography hierarchy, and responsive interaction pattern for UI components.

### Scope

- **Applies to**: Web/Mobile interfaces, views, components, and pages.
- **Does not cover**: Pure backend services or API-only tasks.

### Triggers

- "/ui-ux-pro-max"
- "Generate UI design system"
- "Design frontend component"

### Inputs

- `productType`: Domain or product category.
- `techStack`: CSS framework (e.g. Tailwind CSS, CSS Modules).

### Invariants

1. **Design Excellence**: Avoid generic default colors or plain browser defaults.
2. **Responsive Breakpoints**: Use mobile-first responsive design (`min-*` inverted breakpoints).
3. **Accessibility**: Ensure high contrast, focus states, and keyboard navigation.

### Procedure

1. Analyze feature context and target users.
2. Define HSL color palette (Primary, Secondary, Accent, Background, Surface, Muted).
3. Establish font pairings and typographic scale.
4. Specify micro-animations, hover states, and dynamic visual cues.
5. Output design block to `walkthrough.md`.

### Outputs

- UX Design Specification Block in `walkthrough.md`.

### Review gate

- Palette, typography, and responsive rules defined.
- Mobile-first breakpoints verified.

### References

- `../../SKILL.md`
- `../radioactive/SKILL.md`
- [Interactive HTML View](./README.html)
