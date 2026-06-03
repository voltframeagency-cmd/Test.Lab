# Lumen WebGL Liquid Metal Button Dissection Report

This document dissects the reverse-engineered replication of the **Lumen Liquid Metal Button** sandbox. It outlines the design tokens, WebGL parameters, 3D layering mechanics, and interactive speed dynamics.

## Location References
- Replicated Codebase: [index.html](index.html)
- Local Server script: [server.js](server.js)

---

## 1. Design Tokens & Visual Hierarchy

The interface implements a monochromatic dashboard layout tailored for dark-mode SaaS showcases.

### Color Systems
| CSS Token | Hex/RGB Value | Purpose |
|---|---|---|
| `--bg-primary` | `#05060b` | Base application background |
| `--bg-secondary` | `#0c0e18` | Panels and header borders |
| `--card-bg` | `rgba(13, 16, 29, 0.6)` | Blended backdrop-blur containers |
| `--accent` | `#3b82f6` | Primary action indicator |
| `--accent-glow` | `rgba(59, 130, 246, 0.25)` | Slider glow and focus rings |
| `--text-primary` | `#f8fafc` | High-contrast body titles |
| `--text-secondary` | `#94a3b8` | Subheadings and label text |

### Typography
- Display Title: `Space Grotesk` (weights 400, 700) for sharp structural layout labels.
- Body Copy: `Inter` (weights 300 to 700) for readability.

---

## 2. Kinematic 3D Layering Structure

To decouple the WebGL shader from rigid sizing constraints, the button utilizes a 4-layer 3D stacking structure using CSS perspective.

```
[Layer 4: Click Trigger]       - translateZ(25px) - Handles clicks & ripple animations
[Layer 1: Text Label]          - translateZ(20px) - Text with drop shadow
[Layer 2: Inner Plate]         - translateZ(10px) - Dark cover creating the rim boundary
[Layer 3: WebGL Canvas]        - translateZ(0px)  - Liquid metal background shader
```

### Stacking Breakdown

1. **Perspective Container (`1000px`)**:
   Enables 3D space rendering. Coordinates the rotations on cursor interaction.
2. **Base Layer — WebGL Canvas (`translateZ(0px)`)**:
   Renders `@paper-design/shaders` liquid metal rendering on canvas. Stretches dynamically to 100% of the parent container size.
3. **Rim Cover Layer (`translateZ(10px)`)**:
   A dark inner cover positioned `3px` inside the boundary. Creates the glowing WebGL rim boundary look.
4. **Interactive Text Layer (`translateZ(20px)`)**:
   Maintains text visibility by floating above the interactive planes. Uses text shadow to ensure contrast.
5. **Trigger Layer (`translateZ(25px)`)**:
   Maintains mouse handlers. Serves as the mounting coordinate system for radial click ripple elements.

---

## 3. WebGL Uniform Configurations

The liquid metal simulation depends on uniform parameters processed each frame by the GPU.

```typescript
const activeUniforms = {
  u_repetition: 4.0,   // Wave density
  u_softness: 0.50,    // Gradient blur edge
  u_shiftRed: 0.30,    // Chromatic aberration red shift
  u_shiftBlue: 0.30,   // Chromatic aberration blue shift
  u_scale: 8.0,        // Zoom scale of texture noise
  u_distortion: 0.0,   // Noise wave distortion factor
  u_angle: 45.0,       // Wave propagation direction angle
}
```

---

## 4. Motion Choreography & Speeds

The button transitions through three distinct operational states to provide real-time tactile feedback.

### Speed States
| Operational State | Speed Value | Kinematic Behavior |
|---|---|---|
| **Idle** | `0.6` | Slow wave flow to maintain ambient movement |
| **Hover** | `1.0` | Waves accelerate. Button rotates to `rotateX(8deg) rotateY(-8deg)` |
| **Click Spike** | `2.4` | Instantly spikes speed for `300ms`. Generates a radial ripple |

### Click Ripple Animation
- Duration: `600ms`
- Timing Curve: `cubic-bezier(0.1, 0.8, 0.3, 1)`
- Behavior: Spreads outwards, scaling from 0 to 4 times the initial radius while fading from `0.6` opacity to `0`.
