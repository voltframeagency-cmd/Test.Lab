# Premium Pro Libraries (Cult UI Pro & React Bits Pro) Reference Guide

This reference guide documents the layouts, API schemas, Tailwind configurations, and property specifications for **Cult UI Pro Illustrations** and **React Bits Pro** blocks and components.

## Table of Contents

- [Cult UI Pro Illustrations](#cult-ui-pro-illustrations)
  - [Overview](#overview)
  - [EmptyState Setup & Props](#emptystate-setup--props)
  - [Interactive Feature Cards](#interactive-feature-cards)
  - [Waitlist Compositions](#waitlist-compositions)
- [React Bits Pro](#react-bits-pro)
  - [Overview](#overview-1)
  - [Balatro Background Shader (GLSL/Canvas)](#balatro-background-shader-glslcanvas)
  - [Variable Proximity Props & Setup](#variable-proximity-props--setup)
  - [Magnet Dock & Lanyard 3D Components](#magnet-dock--lanyard-3d-components)

---

## Cult UI Pro Illustrations <a name="cult-ui-pro-illustrations"></a>

Premium SVG and canvas-based animated illustrations designed for modern SaaS interfaces, landing pages, and interactive dashboards.

### Overview <a name="overview"></a>
Cult UI Pro Illustrations provides high-conversion direct-response design vectors that animate on load and hover. They utilize Framer Motion spring physics and tailwindcss utility classes.

#### Dependencies
- `framer-motion`
- `lucide-react`
- `clsx`, `tailwind-merge`

---

### EmptyState Setup & Props <a name="emptystate-setup--props"></a>
A dynamic empty state wrapper that cycles through illustrations with custom bounce entries.

#### Schema & Usage
```tsx
import React from 'react'
import { motion } from 'framer-motion'

export interface EmptyStateProps {
  title: string
  description: string
  actionLabel?: string
  onAction?: () => void
  illustration?: React.ReactNode
  animate?: boolean
}

export const EmptyState: React.FC<EmptyStateProps> = ({
  title,
  description,
  actionLabel,
  onAction,
  illustration,
  animate = true
}) => {
  return (
    <div className="flex flex-col items-center justify-center p-8 text-center border-2 border-dashed rounded-xl border-neutral-800 bg-neutral-950/50">
      {illustration && (
        <motion.div
          initial={animate ? { scale: 0.8, opacity: 0 } : false}
          animate={animate ? { scale: 1, opacity: 1 } : false}
          transition={{ type: "spring", stiffness: 100, damping: 15 }}
          className="mb-6"
        >
          {illustration}
        </motion.div>
      )}
      <h3 className="text-xl font-bold text-neutral-100">{title}</h3>
      <p className="max-w-xs mt-2 text-sm text-neutral-400">{description}</p>
      {actionLabel && onAction && (
        <button
          onClick={onAction}
          className="px-4 py-2 mt-6 text-sm font-semibold transition-all rounded-lg bg-emerald-500 text-neutral-950 hover:bg-emerald-400 active:scale-95"
        >
          {actionLabel}
        </button>
      )}
    </div>
  )
}
```

---

### Interactive Feature Cards <a name="interactive-feature-cards"></a>
Layouts combining responsive grids, text overlays, and interactive hover vectors.

#### Properties Table
| Prop | Type | Default | Description |
|---|---|---|---|
| `icon` | `React.ComponentType` | `undefined` | Lucide icon to display in the header |
| `title` | `string` | `""` | Primary title |
| `description` | `string` | `""` | Description block |
| `badge` | `string` | `""` | Optional status badge text |
| `illustration` | `React.ReactNode` | `undefined` | Custom SVG/Illustration render |

---

### Waitlist Compositions <a name="waitlist-compositions"></a>
Direct-response onboarding form structures featuring split layouts and side-mounted particle effects.

---

## React Bits Pro <a name="react-bits-pro"></a>

A suite of highly interactive React component blocks featuring WebGL shaders, math-derived offsets, and advanced scroll interactions.

### Overview <a name="overview-1"></a>
React Bits Pro leverages vanilla canvas contexts, WebGL fragment shaders, and Framer Motion hooks for high-fidelity animations.

---

### Balatro Background Shader (GLSL/Canvas) <a name="balatro-background-shader-glslcanvas"></a>
A canvas-based liquid gradient shader inspired by the game Balatro. It animates multiple waves using sin/cos functions and custom color vectors.

#### GLSL Fragment Shader Reference
```glsl
precision highp float;
uniform float u_time;
uniform vec2 u_resolution;
uniform vec3 u_color1;
uniform vec3 u_color2;
uniform vec3 u_color3;
uniform float u_speed;
uniform float u_distortion;

void main() {
    vec2 uv = gl_FragCoord.xy / u_resolution.xy;
    float time = u_time * u_speed;
    
    // Wave distortion logic
    float factor1 = sin(uv.x * 4.0 + time) * 0.5 + 0.5;
    float factor2 = cos(uv.y * 3.0 - time * 0.7) * 0.5 + 0.5;
    float distortion = sin(uv.x * 10.0 + uv.y * 10.0 + time) * u_distortion;
    
    vec3 mix1 = mix(u_color1, u_color2, factor1 + distortion);
    vec3 finalColor = mix(mix1, u_color3, factor2);
    
    gl_FragColor = vec4(finalColor, 1.0);
}
```

---

### Variable Proximity Props & Setup <a name="variable-proximity-props--setup"></a>
Animate character scaling or style transformations based on cursor distance.

#### Props Table
| Prop | Type | Default | Description |
|---|---|---|---|
| `label` | `string` | `""` | Text to render |
| `radius` | `number` | `200` | Interaction radius in pixels |
| `falloff` | `"linear" \| "exponential"` | `"linear"` | Scaling decay function |
| `minScale` | `number` | `1.0` | Minimum character scale at rest |
| `maxScale` | `number` | `1.8` | Maximum character scale under cursor |

---

### Magnet Dock & Lanyard 3D Components <a name="magnet-dock--lanyard-3d-components"></a>
Dynamic interface elements. `Lanyard` uses a canvas-based spring rope simulation (Verlet integration) that trails the cursor or follows gravity forces.
