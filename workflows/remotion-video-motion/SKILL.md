## agentic-workflows-blueprint.workflow.remotion-video-motion

### Goal

Scaffold, configure, and animate React frontend components inside Remotion compositions for programmatic video creation, high-fidelity UI motion demos, micro-animations, and dynamic video rendering.

### Scope

- **Applies to**: Any React-based codebase (Next.js, Vite, Remotion CLI, Webpack, Rails React mounts, or standalone video packages).
- **Agnostic & Abstract**: Designed to work independently of specific project paths or UI frameworks.

### Triggers

- "remotion video"
- "remotion motion animation"
- "import react component into remotion"
- "setup remotion composition"
- "render remotion mp4/gif"
- "ui video demo animation"

### Inputs

- `TargetComponent`: The React frontend component to animate or showcase.
- `CompositionConfig`: Resolution (`width`, `height`), frame rate (`fps`), total frames (`durationInFrames`), and default props.
- `AnimationSpec`: Keyframe choreography, spring physics configs, and sequence timing.

### Invariants

1. **Deterministic Frame Rendering**:
   - NEVER use non-deterministic sources like `Date.now()`, `Math.random()`, or unseeded random generators directly inside frame render loops.
   - For async assets or network data, use `delayRender()` and `continueRender()` from `remotion`.

2. **Asset Path Resolution**:
   - All static media (images, fonts, videos, audio) MUST use `staticFile("path/to/asset")` or pre-fetched Data URLs.

3. **Component Decoupling (Adapter Pattern)**:
   - Target React components MUST be decoupled from global browser singletons (`window.location`, `localStorage`) and external API calls.
   - Use props or mock providers to feed component state into Remotion compositions.

4. **Spring Physics over Linear Easing**:
   - Prefer `spring({ frame, fps, config: { damping, stiffness, mass } })` from `remotion` for natural UI motion over fixed linear transitions.

5. **Tailwind CSS & Asset Compatibility**:
   - When importing Tailwind CSS components, ensure `index.css` or Tailwind directives are imported in the Remotion Root or configured via `@remotion/tailwind`.

---

### Architecture and workflow phases

```
┌────────────────────────────────────────────────────────────────────────┐
│  Phase 1: Environment & Dependency Verification                         │
│  - Check / Install `remotion`, `@remotion/cli`, `@remotion/player`     │
├────────────────────────────────────────────────────────────────────────┤
│  Phase 2: Component Decoupling & Adapter Wrapping                      │
│  - Wrap React UI in Frame-aware Props & Mock Context Providers         │
├────────────────────────────────────────────────────────────────────────┤
│  Phase 3: Root Composition & Schema Definition (Zod)                   │
│  - Define <Composition id="..." component={...} schema={z.object()}/>  │
├────────────────────────────────────────────────────────────────────────┤
│  Phase 4: Motion Choreography & Spring Physics                         │
│  - Implement `useCurrentFrame`, `useVideoConfig`, `interpolate`, `spring`│
│  - Layer elements using `<AbsoluteFill>` and `<Sequence>`              │
├────────────────────────────────────────────────────────────────────────┤
│  Phase 5: Asset & Font Preloading                                      │
│  - Preload web fonts via `@remotion/google-fonts` or `@fontsource`     │
├────────────────────────────────────────────────────────────────────────┤
│  Phase 6: Studio Preview & CLI Render Pipeline                         │
│  - Verify via Remotion Studio & Render MP4 / WebM / GIF                │
└────────────────────────────────────────────────────────────────────────┘
```

---

### Step-by-step execution guide

### Phase 1: Environment Detection & Setup

Check `package.json` for `remotion` dependencies. If not present:

```bash
npm install remotion @remotion/cli @remotion/player @remotion/media-utils zod
```

If Tailwind CSS is used in the project:
```bash
npm install @remotion/tailwind
```

---

### Phase 2: React Component Decoupling (Adapter Pattern)

Wrap the target React UI component in a pure frame-driven container so it can be controlled seamlessly by Remotion props:

```tsx
// src/video/adapters/MyComponentAdapter.tsx
import React from 'react';
import { useCurrentFrame, useVideoConfig, spring, interpolate } from 'remotion';
import { MyTargetComponent } from '../../components/MyTargetComponent';

export interface AdapterProps {
  title: string;
  subtitle?: string;
  highlightColor?: string;
}

export const MyComponentAdapter: React.FC<AdapterProps> = ({ title, subtitle, highlightColor = '#3b82f6' }) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  // Smooth entrance spring
  const scale = spring({
    frame,
    fps,
    config: { damping: 12, stiffness: 100, mass: 0.5 },
  });

  // Opacity fade-in
  const opacity = interpolate(frame, [0, 15], [0, 1], {
    extrapolateLeft: 'clamp',
    extrapolateRight: 'clamp',
  });

  return (
    <div style={{ opacity, transform: `scale(${scale})` }}>
      <MyTargetComponent title={title} subtitle={subtitle} accentColor={highlightColor} />
    </div>
  );
};
```

---

### Phase 3: Root Composition & Zod Schema Definition

Register the composition in Remotion's root entrypoint (e.g., `src/video/Root.tsx` or `remotion/Root.tsx`):

```tsx
// src/video/Root.tsx
import React from 'react';
import { Composition } from 'remotion';
import { z } from 'zod';
import { MyComponentAdapter } from './adapters/MyComponentAdapter';
import '../index.css'; // Import global Tailwind or CSS styles

export const myComponentSchema = z.object({
  title: z.string(),
  subtitle: z.string().optional(),
  highlightColor: z.string().optional(),
});

export const RemotionRoot: React.FC = () => {
  return (
    <>
      <Composition
        id="MyComponentDemo"
        component={MyComponentAdapter}
        durationInFrames={150} // 5 seconds at 30 fps
        fps={30}
        width={1920}
        height={1080}
        schema={myComponentSchema}
        defaultProps={{
          title: 'Automated Finance',
          subtitle: 'Multi-chain DEX & Web3 Automations',
          highlightColor: '#8b5cf6',
        }}
      />
    </>
  );
};
```

---

### Phase 4: Motion Choreography & Sequences

Structure multi-element animations using `<Sequence>` and `<AbsoluteFill>`:

```tsx
import { AbsoluteFill, Sequence, spring, useCurrentFrame, useVideoConfig } from 'remotion';

export const MotionShowcase: React.FC = () => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  const titleSlide = spring({ frame, fps, config: { damping: 15 } });

  return (
    <AbsoluteFill className="bg-slate-950 text-white justify-center items-center">
      {/* Intro Sequence (Frames 0 to 60) */}
      <Sequence from={0} durationInFrames={60}>
        <div style={{ transform: `translateY(${(1 - titleSlide) * 50}px)` }}>
          <h1 className="text-5xl font-bold">Introducing Feature</h1>
        </div>
      </Sequence>

      {/* Main Component Sequence (Frames 45 onwards) */}
      <Sequence from={45}>
        <MyComponentAdapter title="Live Product Demo" />
      </Sequence>
    </AbsoluteFill>
  );
};
```

---

### Phase 5: Studio Preview & Production Rendering

1. **Launch Studio for Interactive Editing**:
   ```bash
   npx remotion studio src/video/Root.tsx
   ```

2. **Render Production Video (MP4)**:
   ```bash
   npx remotion render MyComponentDemo out/video.mp4
   ```

3. **Render Animated GIF / WebM**:
   ```bash
   npx remotion render MyComponentDemo out/demo.gif
   ```

---

### Review gate

- [ ] All Remotion dependencies resolution cleanly without version conflicts.
- [ ] React UI components render in Remotion Studio without missing styles or unhandled runtime exceptions.
- [ ] Frames render deterministically (`npx remotion render` produces clean output without missing assets).
- [ ] Props are strongly typed with Zod schema (`myComponentSchema`).
- [ ] Motion springs and interpolations execute smoothly at target FPS (30 or 60).

### References

- [Remotion Official Documentation](https://www.remotion.dev/docs)
- [Remotion Motion Blur & Transitions](https://www.remotion.dev/docs/transitions)
- [Workflow Blueprint SKILL](../../SKILL.md)
- [Interactive HTML View](./README.html)
