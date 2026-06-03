# React Bits Pro Agency Template Dissection Report

This report reverse-engineers the design system and motion mechanics of the **React Bits Pro Agency Template**. It outlines layout tokens, WebGL wave shaders, 3D CSS entrance layers, and interactive mouse-proximity physics.

## Location References
- Target Live Site: [rbp-agency-template.vercel.app](https://rbp-agency-template.vercel.app)
- Template Documentation: [pro.reactbits.dev/docs/templates/agency-site](https://pro.reactbits.dev/docs/templates/agency-site)

---

## 1. Design System & Spacing Tokens

The template applies a high-contrast minimalist layout system. It's built for dark-mode creative studios.

### Color Tokens
| CSS Token | Value | Purpose |
|---|---|---|
| `--bg-background` | `#0a0a0a` | Global dark-mode background base |
| `--fg-foreground` | `#ffffff` | High-contrast display titles |
| `--border-accent` | `rgba(255, 255, 255, 0.1)` | Divider lines — menu borders |
| `--bg-muted` | `rgba(255, 255, 255, 0.05)` | Bento grids — card backdrops |

### Typography
- Primary Sans: `Geist Sans` (weights 300 to 600) — handles clean body copy.
- Display Serif: `Geist Mono` or standard serif (italicized) — highlights emotional action verbs.
- Font Scaling: Clamp-based viewport scales. The hero titles scale via `text-[clamp(3rem,8vw,12rem)]` — service menus scale via `text-[clamp(1.5rem,4vw,4rem)]`.

---

## 2. WebGL Wave Background Shader

The hero background uses a noise-deformed wave shader on a WebGL canvas context. It runs ambient wave animations.

### Uniform Configurations
The fragment shader handles dynamic waves using 2D Perlin noise. The GPU processes these uniforms:

```typescript
const waveUniforms = {
  waveSpeed: 0.05,        // Time scale multiplier for noise speed
  waveFrequency: 3.0,     // Wave noise detail compression
  waveAmplitude: 0.3,     // Height displacement factor of waves
  waveColor: [0.5, 0.5, 0.5], // Normalized RGB base color vector
  mouseRadius: 1.0,       // Cursor proximity repulsion radius
  enableMouseInteraction: 1 // Toggle coordinate-based deflection
}
```

### Noise Deformation Pipeline
1. **FBM (Fractional Brownian Motion) Octaves**:
   The fragment shader runs `4` noise octaves. It scales frequency and amplitude progressively.
2. **Coordinate Warping (Domain Warping)**:
   The shader offsets coordinates by noise outputs (`fbm(p + fbm(p2))`). It creates flowing, smoke-like wave patterns.
3. **Mouse Interaction Deflection**:
   The cursor position is mapped to Normalized Device Coordinates (NDC). It subtracts height from the noise wave on proximity.

---

## 3. Kinematic 3D Typography Entrance

The hero typography utilizes a perspective-origin container to create 3D folding character entries.

```
[Viewport Layer]           - perspective: 1200px
[Line Mask Containers]     - overflow: hidden
[Animated Text Spans]      - transform-origin: center bottom
                           - transform: translateY(120%) translateZ(-200px) rotateX(-90deg)
```

### Entrance Choreography
- **Initial State**: Text is pushed down (`translateY(120%)`), rotated backward (`rotateX(-90deg)`), and set deep in perspective space (`translateZ(-200px)`).
- **Masking**: Each text line is wrapped in a container that has `overflow: hidden`. It clips the rotated elements.
- **Tween Target**: GSAP animates the inner spans to `translateY(0%)`, `translateZ(0px)`, and `rotateX(0deg)`. This creates a flip-up letterpress effect.

---

## 4. Motion Choreography & Speeds

The page transitions through distinct scroll states. It coordinates micro-animations on user scroll.

### Selected Work Marquee
- Horizontal Loop: Uses CSS translations to shift text bands infinitely.
- Hover Cursor Follower: A pointer element (`w-20 h-20 rounded-full bg-foreground`) scales up on project cards. It displays "Open" inside.

### Proximity Project Card Parallax
- Aspect Ratio: Cards enforce `aspect-4/3` wrapped in `rounded-full` boundaries.
- Scale Dynamics: The child image spans are pre-scaled to `scale(1.15)`. On cursor proximity, the image translates inside the parent crop — creating a deep water-ripple parallax illusion.

### Services Hover Overlay
- Animation Trigger: Hovering list entries slides a background block (`bg-foreground`) from `translateY(101%)` to `0%`.
- Text Color Inversion: The text block duplicates inside the sliding container. It uses inverted colors (`text-background`) — creating a color-swap mask on overlap.

### FAQ Accordion Grid Row Transitions
- Height Animating: The details panel avoids absolute pixel heights. It toggles via `grid-template-rows: 0fr` (closed) to `1fr` (open) inside a transition-all duration block.
- Rotating Indicator: The plus icon (`+`) rotates `45deg` into a minus icon (`-`) on active accordion states.

### Sticky Reveal Footer
- Layout Stacking: The contact footer (`#contact`) uses `lg:sticky lg:bottom-0 lg:z-0`. The main scrollable content has `lg:relative lg:z-10 bg-background`.
- Reveal Effect: As the user scrolls past the FAQ section, the main page layer translates upwards. It uncovers the fixed footer layer underneath.
