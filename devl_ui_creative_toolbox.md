# Devl UI Creative Developer Reference Guide

A complete reference guide for **Devl UI** (devl.dev) — a premium component and template library focusing on split auth layouts and canvas-based particle/noise animation backgrounds.

## Table of Contents

### Core UI Components
- [ParticleField](#particlefield)
- [AmbientGrain](#ambientgrain)
- [AuthSplitLayout](#authsplitlayout)
- [OrbitAvatar](#orbitavatar)
- [OrbitAvatarField](#orbitavatarfield)
- [AppearancePicker](#appearancepicker)
### Showcase Compositions
- [Login Showcase](#login-showcase)
- [Inset Login Showcase](#inset-login-showcase)
- [Onboarding Showcase](#onboarding-showcase)
- [Waitlist Showcase](#waitlist-showcase)

---

## Core UI Components

### ParticleField <a name="particlefield"></a>

Custom canvas-based particle sampler. Reads target PNG images, generates coordinates array, performs spring-interpolated coordinate morphs (Fisher-Yates permutations) to transition shapes, and triggers ripple waves on keypress.

- **Relative Path**: `packages/ui/src/components/particle-field.tsx`

#### Props & API Reference

```typescript
export type ParticleFieldProps = {
  src: string;
  /** pixel step when sampling the source image. Lower = denser */
  sampleStep?: number;
  /** alpha cutoff 0-255 for including a pixel as a particle */
  threshold?: number;
  /** multiplier applied to the canvas rendering versus the sampled image */
  renderScale?: number;
  /** base dot size in device pixels */
  dotSize?: number;
  /** how strong the cursor repels dots */
  mouseForce?: number;
  /** radius around the cursor that has repelling force, in device pixels */
  mouseRadius?: number;
  /** spring constant pulling dots back to their origin */
  spring?: number;
  /** viscous damping on velocity */
  damping?: number;
  className?: string;
  /** alignment of the particle cluster inside the canvas */
  align?: "center" | "bottom";
  /** optional color override when `adaptToTheme` is false; defaults to white */
  color?: string;
  /** sample dark pixels instead of bright ones (for dark-on-light source images) */
  invert?: boolean;
  /**
   * When true (default), dot fill follows `html.dark` (light dots on dark, dark dots on light)
   * without resampling the image — only the paint color changes, so toggling theme stays smooth.
   */
  adaptToTheme?: boolean;
  /**
   * POC: parent bumps `current` on keydown (e.g. +0.12); field decays each frame and uses it
   * to add extra drift / twinkle so typing on the auth column subtly animates the figure.
   */
  typingImpulseRef?: MutableRefObject<number>;
  /**
   * When true, keeps every sampled pixel above `threshold` (skips the random luminance thinning).
   * Use for small fixed-size embeds where the default sparse falloff erases the figure.
   */
  denseParticles?: boolean;
}
```

#### Component Source Code

```tsx
"use client";

import {
  type MutableRefObject,
  useEffect,
  useRef,
  useSyncExternalStore,
} from "react";

type Particle = {
  ox: number;
  oy: number;
  x: number;
  y: number;
  vx: number;
  vy: number;
  size: number;
  alpha: number;
  phase: number;
  /** Per-particle spring multiplier (0.75..1.25) — decorrelates arrival times during a morph. */
  springJitter: number;
  /** 0 = invisible, 1 = fully painted. Eases toward 1, or toward 0 while fading. */
  appear: number;
  /** Surplus particle from a prior shape — fade out and cull. */
  fading: boolean;
};

type ParticleTarget = {
  ox: number;
  oy: number;
  size: number;
  alpha: number;
};

export type ParticleFieldProps = {
  src: string;
  /** pixel step when sampling the source image. Lower = denser */
  sampleStep?: number;
  /** alpha cutoff 0-255 for including a pixel as a particle */
  threshold?: number;
  /** multiplier applied to the canvas rendering versus the sampled image */
  renderScale?: number;
  /** base dot size in device pixels */
  dotSize?: number;
  /** how strong the cursor repels dots */
  mouseForce?: number;
  /** radius around the cursor that has repelling force, in device pixels */
  mouseRadius?: number;
  /** spring constant pulling dots back to their origin */
  spring?: number;
  /** viscous damping on velocity */
  damping?: number;
  className?: string;
  /** alignment of the particle cluster inside the canvas */
  align?: "center" | "bottom";
  /** optional color override when `adaptToTheme` is false; defaults to white */
  color?: string;
  /** sample dark pixels instead of bright ones (for dark-on-light source images) */
  invert?: boolean;
  /**
   * When true (default), dot fill follows `html.dark` (light dots on dark, dark dots on light)
   * without resampling the image — only the paint color changes, so toggling theme stays smooth.
   */
  adaptToTheme?: boolean;
  /**
   * POC: parent bumps `current` on keydown (e.g. +0.12); field decays each frame and uses it
   * to add extra drift / twinkle so typing on the auth column subtly animates the figure.
   */
  typingImpulseRef?: MutableRefObject<number>;
  /**
   * When true, keeps every sampled pixel above `threshold` (skips the random luminance thinning).
   * Use for small fixed-size embeds where the default sparse falloff erases the figure.
   */
  denseParticles?: boolean;
};

function subscribeDocumentDark(callback: () => void) {
  const el = document.documentElement;
  const mo = new MutationObserver(callback);
  mo.observe(el, { attributes: true, attributeFilter: ["class"] });
  const mq = window.matchMedia("(prefers-color-scheme: dark)");
  mq.addEventListener("change", callback);
  return () => {
    mo.disconnect();
    mq.removeEventListener("change", callback);
  };
}

function getDocumentDarkSnapshot() {
  return document.documentElement.classList.contains("dark");
}

function getServerDarkSnapshot() {
  return true;
}

const TYPING_IMPULSE_ADD = 0.14;
const TYPING_IMPULSE_CAP = 1.35;

const SUBMIT_IMPULSE_PRIMARY = 0.52;
const SUBMIT_IMPULSE_SECOND_MS = 120;
const SUBMIT_IMPULSE_SECONDARY = 0.2;

/** Add energy to `typingImpulseRef` (keyboard, preset chips, etc.). */
export function pulseParticleTypingImpulse(
  impulseRef: MutableRefObject<number>,
  amount = TYPING_IMPULSE_ADD,
) {
  impulseRef.current = Math.min(
    impulseRef.current + amount,
    TYPING_IMPULSE_CAP,
  );
}

/**
 * Stronger two-beat pulse when a form is sent — primary hit plus a quick
 * follow-up while the first is still decaying (reads like a soft “launch”).
 */
export function pulseParticleSubmitImpulse(
  impulseRef: MutableRefObject<number>,
) {
  pulseParticleTypingImpulse(impulseRef, SUBMIT_IMPULSE_PRIMARY);
  window.setTimeout(() => {
    pulseParticleTypingImpulse(impulseRef, SUBMIT_IMPULSE_SECONDARY);
  }, SUBMIT_IMPULSE_SECOND_MS);
}

/** Bump `typingImpulseRef` from a `keydown` handler (used with `ParticleField` typing POC). */
export function bumpParticleTypingImpulse(
  impulseRef: MutableRefObject<number>,
  e: Pick<KeyboardEvent, "repeat" | "metaKey" | "ctrlKey" | "altKey" | "key">,
) {
  if (e.repeat) return;
  if (e.metaKey || e.ctrlKey || e.altKey) return;
  if (e.key === "Tab" || e.key === "Escape") return;
  pulseParticleTypingImpulse(impulseRef, TYPING_IMPULSE_ADD);
}

function useDocumentDark() {
  return useSyncExternalStore(
    subscribeDocumentDark,
    getDocumentDarkSnapshot,
    getServerDarkSnapshot,
  );
}

export function ParticleField({
  src,
  sampleStep = 3,
  threshold = 50,
  renderScale = 1,
  dotSize = 1.15,
  mouseForce = 90,
  mouseRadius = 110,
  spring = 0.035,
  damping = 0.86,
  className,
  align = "center",
  color = "rgba(255, 255, 255, 0.92)",
  invert = false,
  adaptToTheme = true,
  typingImpulseRef,
  denseParticles = false,
}: ParticleFieldProps) {
  const isDark = useDocumentDark();
  const fillColorRef = useRef(color);
  fillColorRef.current = adaptToTheme
    ? isDark
      ? "rgba(255, 255, 255, 0.92)"
      : "rgba(10, 12, 16, 1)"
    : color;

  const canvasRef = useRef<HTMLCanvasElement | null>(null);
  const wrapperRef = useRef<HTMLDivElement | null>(null);
  const pointerRef = useRef({ x: -9999, y: -9999, active: false });
  const srcRef = useRef(src);
  srcRef.current = src;
  const applySrcRef = useRef<((nextSrc: string) => void) | null>(null);

  // Tuning props live in refs so the main effect can stay mounted across
  // prop changes (e.g. onboarding step tweaks threshold + dotSize + src
  // simultaneously). Rebuilding the effect would defeat the morph.
  const sampleStepRef = useRef(sampleStep);
  sampleStepRef.current = sampleStep;
  const thresholdRef = useRef(threshold);
  thresholdRef.current = threshold;
  const renderScaleRef = useRef(renderScale);
  renderScaleRef.current = renderScale;
  const dotSizeRef = useRef(dotSize);
  dotSizeRef.current = dotSize;
  const mouseForceRef = useRef(mouseForce);
  mouseForceRef.current = mouseForce;
  const mouseRadiusRef = useRef(mouseRadius);
  mouseRadiusRef.current = mouseRadius;
  const springRef = useRef(spring);
  springRef.current = spring;
  const dampingRef = useRef(damping);
  dampingRef.current = damping;
  const alignRef = useRef(align);
  alignRef.current = align;
  const invertRef = useRef(invert);
  invertRef.current = invert;
  const denseParticlesRef = useRef(denseParticles);
  denseParticlesRef.current = denseParticles;

  useEffect(() => {
    const canvas = canvasRef.current;
    const wrapper = wrapperRef.current;
    if (!canvas || !wrapper) return;

    const ctx = canvas.getContext("2d", { alpha: true });
    if (!ctx) return;

    let particles: Particle[] = [];
    let dpr = Math.min(window.devicePixelRatio || 1, 2);
    let width = 0;
    let height = 0;
    let clusterW = 0;
    let clusterH = 0;
    let offsetX = 0;
    let offsetY = 0;
    let rafId = 0;
    let time = 0;
    let destroyed = false;
    let resizeRaf = 0;
    let resizeTimer: ReturnType<typeof setTimeout> | null = null;
    let currentImage: HTMLImageElement | null = null;
    let loadToken = 0;

    const ensureCanvasSize = () => {
      const rect = wrapper.getBoundingClientRect();
      width = Math.max(1, Math.floor(rect.width));
      height = Math.max(1, Math.floor(rect.height));
      dpr = Math.min(window.devicePixelRatio || 1, 2);

      canvas.width = width * dpr;
      canvas.height = height * dpr;
      canvas.style.width = `${width}px`;
      canvas.style.height = `${height}px`;
    };

    const sampleTargets = (image: HTMLImageElement): ParticleTarget[] => {
      if (!image.width || !image.height) return [];

      const srcRatio = image.width / image.height;
      const dstRatio = width / height;

      let drawW = width;
      let drawH = height;
      if (srcRatio > dstRatio) {
        drawH = height;
        drawW = height * srcRatio;
      } else {
        drawW = width;
        drawH = width / srcRatio;
      }

      drawW *= renderScaleRef.current;
      drawH *= renderScaleRef.current;

      const sampleW = Math.max(80, Math.floor(drawW / sampleStepRef.current));
      const sampleH = Math.max(80, Math.floor(drawH / sampleStepRef.current));

      const off = document.createElement("canvas");
      off.width = sampleW;
      off.height = sampleH;
      const offCtx = off.getContext("2d", { willReadFrequently: true });
      if (!offCtx) return [];
      offCtx.drawImage(image, 0, 0, sampleW, sampleH);
      const data = offCtx.getImageData(0, 0, sampleW, sampleH).data;

      const cellW = drawW / sampleW;
      const cellH = drawH / sampleH;

      clusterW = drawW;
      clusterH = drawH;
      offsetX = (width - clusterW) / 2;
      offsetY =
        alignRef.current === "bottom"
          ? height - clusterH - Math.min(40, height * 0.04)
          : (height - clusterH) / 2;

      const thresholdV = thresholdRef.current;
      const invertV = invertRef.current;
      const denseV = denseParticlesRef.current;
      const dotSizeV = dotSizeRef.current;

      const targets: ParticleTarget[] = [];
      for (let y = 0; y < sampleH; y++) {
        for (let x = 0; x < sampleW; x++) {
          const idx = (y * sampleW + x) * 4;
          const r = data[idx];
          const g = data[idx + 1];
          const b = data[idx + 2];
          const a = data[idx + 3];
          const rawBrightness = (r + g + b) / 3;
          const brightness = invertV ? 255 - rawBrightness : rawBrightness;
          if (a < 200 || brightness < thresholdV) continue;

          const lum = brightness / 255;
          if (!denseV) {
            // density falloff: skip some mid-gray pixels to keep dots sparse (hero layouts)
            const keep =
              lum > 0.8
                ? true
                : lum > 0.5
                  ? Math.random() < 0.85
                  : lum > 0.25
                    ? Math.random() < 0.55
                    : Math.random() < 0.28;
            if (!keep) continue;
          }

          const px = (offsetX + x * cellW + cellW / 2) * dpr;
          const py = (offsetY + y * cellH + cellH / 2) * dpr;

          targets.push({
            ox: px,
            oy: py,
            size: (dotSizeV + lum * 0.9) * dpr,
            alpha: 0.35 + lum * 0.6,
          });
        }
      }
      return targets;
    };

    const randomSpringJitter = () => 0.9 + Math.random() * 0.2;

    const buildFresh = (image: HTMLImageElement) => {
      if (!image.width || !image.height) return;
      ensureCanvasSize();
      const targets = sampleTargets(image);
      particles = targets.map((t) => ({
        ox: t.ox,
        oy: t.oy,
        x: t.ox + (Math.random() - 0.5) * 40,
        y: t.oy + (Math.random() - 0.5) * 40,
        vx: 0,
        vy: 0,
        size: t.size,
        alpha: t.alpha,
        phase: Math.random() * Math.PI * 2,
        springJitter: randomSpringJitter(),
        appear: 1,
        fading: false,
      }));
    };

    // In-place Fisher–Yates — used to decorrelate the raster scan order of
    // both the current particle array and the freshly sampled targets so
    // morph transitions don't sweep in a visible diagonal line.
    const shuffleIndices = (n: number): number[] => {
      const arr = new Array<number>(n);
      for (let i = 0; i < n; i++) arr[i] = i;
      for (let i = n - 1; i > 0; i--) {
        const j = (Math.random() * (i + 1)) | 0;
        const tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
      }
      return arr;
    };

    // Re-home existing particles onto a new figure so they spring-migrate
    // instead of snapping.
    //
    // Pairing particles↔targets by raster index (the naive approach) created
    // a very visible sweep line: both images are sampled top-left →
    // bottom-right, so every row's density delta moved particles in lockstep.
    // We randomly permute both arrays before pairing so motion is spatially
    // decorrelated. Surplus particles just fade in place; new particles
    // spawn on a small ring around their destination.
    const morphTo = (image: HTMLImageElement) => {
      if (!image.width || !image.height) return;
      if (particles.length === 0) {
        buildFresh(image);
        return;
      }
      ensureCanvasSize();
      const targets = sampleTargets(image);

      const n = particles.length;
      const m = targets.length;
      const matched = Math.min(n, m);
      const pOrder = shuffleIndices(n);
      const tOrder = shuffleIndices(m);

      for (let k = 0; k < matched; k++) {
        const p = particles[pOrder[k]];
        const t = targets[tOrder[k]];
        p.ox = t.ox;
        p.oy = t.oy;
        p.size = t.size;
        p.alpha = t.alpha;
        p.fading = false;
        p.springJitter = randomSpringJitter();
      }

      // Surplus particles just fade in place — the spring keeps them near
      // their previous home while alpha eases to 0. No outward kick: that
      // read as "explosion" rather than "settle".
      for (let k = matched; k < n; k++) {
        particles[pOrder[k]].fading = true;
      }

      // Extras: spawn new particles on a small random ring around each
      // target so the incoming cloud reads as a soft gather, not a sweep.
      for (let k = matched; k < m; k++) {
        const t = targets[tOrder[k]];
        const angle = Math.random() * Math.PI * 2;
        const dist = (20 + Math.random() * 40) * dpr;
        particles.push({
          ox: t.ox,
          oy: t.oy,
          x: t.ox + Math.cos(angle) * dist,
          y: t.oy + Math.sin(angle) * dist,
          vx: 0,
          vy: 0,
          size: t.size,
          alpha: t.alpha,
          phase: Math.random() * Math.PI * 2,
          springJitter: randomSpringJitter(),
          appear: 0,
          fading: false,
        });
      }
    };

    const render = () => {
      if (destroyed) return;
      time += 0.016;
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = fillColorRef.current;

      const mouseForceV = mouseForceRef.current;
      const mouseRadiusV = mouseRadiusRef.current;
      const springV = springRef.current;
      const dampingV = dampingRef.current;

      const px = pointerRef.current.x * dpr;
      const py = pointerRef.current.y * dpr;
      const mr = mouseRadiusV * dpr;
      const mr2 = mr * mr;

      let typing = typingImpulseRef?.current ?? 0;
      if (typingImpulseRef && typing > 1e-4) {
        typingImpulseRef.current *= 0.93;
      }
      const typingBoost = 1 + typing * 10;
      const rippleCx = (offsetX + clusterW * 0.5) * dpr;
      const rippleCy = (offsetY + clusterH * 0.48) * dpr;

      let writeIdx = 0;
      for (let i = 0; i < particles.length; i++) {
        const p = particles[i];

        const dxo = p.ox - p.x;
        const dyo = p.oy - p.y;
        const s = springV * p.springJitter;
        p.vx += dxo * s;
        p.vy += dyo * s;

        if (pointerRef.current.active) {
          const dx = p.x - px;
          const dy = p.y - py;
          const d2 = dx * dx + dy * dy;
          if (d2 < mr2 && d2 > 0.0001) {
            const d = Math.sqrt(d2);
            const force = (1 - d / mr) * mouseForceV;
            p.vx += (dx / d) * force * 0.04;
            p.vy += (dy / d) * force * 0.04;
          }
        }

        const drift = Math.sin(time * 0.8 + p.phase) * 0.08;
        p.vx += drift * 0.05 * typingBoost;
        p.vy += Math.cos(time * 0.9 + p.phase) * 0.04 * typingBoost;

        if (typing > 1e-4) {
          p.vx += (Math.random() - 0.5) * typing * 2.8;
          p.vy += (Math.random() - 0.5) * typing * 2.8;
          const rdx = p.x - rippleCx;
          const rdy = p.y - rippleCy;
          const rd = Math.sqrt(rdx * rdx + rdy * rdy) + 0.5;
          const ripple = (typing * 22 * dpr) / rd;
          p.vx += (rdx / rd) * ripple * 0.018;
          p.vy += (rdy / rd) * ripple * 0.018;
        }

        p.vx *= dampingV;
        p.vy *= dampingV;
        p.x += p.vx;
        p.y += p.vy;

        const appearTarget = p.fading ? 0 : 1;
        p.appear += (appearTarget - p.appear) * 0.08;

        if (p.fading && p.appear < 0.02) {
          // cull from particles[] as part of the render pass
          continue;
        }

        const twinkle =
          0.85 +
          Math.sin(time * (1.4 + typing * 2.2) + p.phase) *
            (0.15 + typing * 0.35);
        ctx.globalAlpha = p.alpha * p.appear * twinkle;
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
        ctx.fill();

        if (writeIdx !== i) particles[writeIdx] = p;
        writeIdx++;
      }
      if (writeIdx !== particles.length) particles.length = writeIdx;
      ctx.globalAlpha = 1;
      rafId = requestAnimationFrame(render);
    };

    const onPointerMove = (e: PointerEvent) => {
      const rect = wrapper.getBoundingClientRect();
      pointerRef.current.x = e.clientX - rect.left;
      pointerRef.current.y = e.clientY - rect.top;
      pointerRef.current.active = true;
    };
    const onPointerLeave = () => {
      pointerRef.current.active = false;
      pointerRef.current.x = -9999;
      pointerRef.current.y = -9999;
    };

    const ro = new ResizeObserver(() => {
      if (resizeRaf) cancelAnimationFrame(resizeRaf);
      resizeRaf = requestAnimationFrame(() => {
        if (resizeTimer) clearTimeout(resizeTimer);
        // Drag-resizing can fire continuously; debounce expensive resampling.
        resizeTimer = setTimeout(() => {
          if (currentImage) buildFresh(currentImage);
        }, 120);
      });
    });

    const loadAndApply = (nextSrc: string, asMorph: boolean) => {
      const token = ++loadToken;
      const image = new Image();
      image.crossOrigin = "anonymous";
      image.decoding = "async";
      image.onload = () => {
        if (destroyed || token !== loadToken) return;
        currentImage = image;
        if (asMorph) morphTo(image);
        else buildFresh(image);
      };
      image.src = nextSrc;
    };

    applySrcRef.current = (nextSrc: string) => loadAndApply(nextSrc, true);

    // Start render loop + resize observer up-front — drawing an empty
    // particle array is a no-op, and starting eagerly avoids races where a
    // second load (e.g. parallel src-change effect) supersedes the initial
    // load's onload before it had a chance to kick off the RAF.
    ro.observe(wrapper);
    rafId = requestAnimationFrame(render);

    loadAndApply(srcRef.current, false);

    wrapper.addEventListener("pointermove", onPointerMove);
    wrapper.addEventListener("pointerleave", onPointerLeave);

    return () => {
      destroyed = true;
      cancelAnimationFrame(rafId);
      if (resizeRaf) cancelAnimationFrame(resizeRaf);
      if (resizeTimer) clearTimeout(resizeTimer);
      ro.disconnect();
      wrapper.removeEventListener("pointermove", onPointerMove);
      wrapper.removeEventListener("pointerleave", onPointerLeave);
      applySrcRef.current = null;
    };
    // Tuning props (threshold, dotSize, align, etc.) are read from refs,
    // so we intentionally don't include them here — re-running this effect
    // would tear down the canvas and particle state, defeating morphs.
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);

  // Morph into a new source without tearing down the particle system. The
  // main effect installs `applySrcRef`; we invoke it when `src` changes so
  // existing particles spring-migrate to the new shape instead of snapping.
  const lastAppliedSrcRef = useRef(src);
  useEffect(() => {
    if (lastAppliedSrcRef.current === src) return;
    lastAppliedSrcRef.current = src;
    applySrcRef.current?.(src);
  }, [src]);

  return (
    <div
      ref={wrapperRef}
      className={className}
      style={{ position: "relative", width: "100%", height: "100%" }}
    >
      <canvas
        ref={canvasRef}
        style={{ display: "block", width: "100%", height: "100%" }}
      />
    </div>
  );
}
```
---

### AmbientGrain <a name="ambientgrain"></a>

A noise grain SVG filter backdrop animating fractal noise parameters to create dynamic texture overlay.

- **Relative Path**: `packages/ui/src/components/ambient-grain.tsx`

#### Props & API Reference

```typescript
type AmbientGrainProps = {
  /** Marketing pages use a slightly stronger grain than the in-app shell */
  variant?: "marketing" | "app";
  className?: string;
}
```

#### Component Source Code

```tsx
"use client";

import { cn } from "../lib/utils";

type AmbientGrainProps = {
  /** Marketing pages use a slightly stronger grain than the in-app shell */
  variant?: "marketing" | "app";
  className?: string;
};

export function AmbientGrain({
  variant = "marketing",
  className,
}: AmbientGrainProps) {
  return (
    <div
      aria-hidden
      className={cn(
        "pointer-events-none absolute inset-0 z-0 mix-blend-multiply dark:mix-blend-overlay",
        variant === "marketing"
          ? "opacity-[0.22] dark:opacity-[0.35]"
          : "opacity-[0.14] dark:opacity-[0.28]",
        "[background-image:radial-gradient(1200px_600px_at_50%_-10%,rgba(0,0,0,0.07),transparent_60%),radial-gradient(800px_500px_at_80%_40%,rgba(0,0,0,0.035),transparent_60%)]",
        "dark:[background-image:radial-gradient(1200px_600px_at_50%_-10%,rgba(255,255,255,0.08),transparent_60%),radial-gradient(800px_500px_at_80%_40%,rgba(255,255,255,0.04),transparent_60%)]",
        className,
      )}
    />
  );
}
```
---

### AuthSplitLayout <a name="authsplitlayout"></a>

Premium split-screen authentication layout. Left side renders a custom background layer (e.g. ParticleField or AmbientGrain), right side contains form fields.

- **Relative Path**: `packages/ui/src/components/ui/auth-split-layout.tsx`

#### Props & API Reference

```typescript
type AuthSplitLayoutProps = {
  left: ReactNode;
  right: ReactNode;
  className?: string;
  frameClassName?: string;
  leftClassName?: string;
  rightClassName?: string;
}
```

#### Component Source Code

```tsx
import type { ReactNode } from "react";
import { cn } from "../../lib/utils";
import { ThemeToggle } from "../theme-toggle";

type AuthSplitLayoutProps = {
  left: ReactNode;
  right: ReactNode;
  className?: string;
  frameClassName?: string;
  leftClassName?: string;
  rightClassName?: string;
};

export function AuthSplitLayout({
  left,
  right,
  className,
  frameClassName,
  leftClassName,
  rightClassName,
}: AuthSplitLayoutProps) {
  return (
    <div
      className={cn(
        "relative h-svh overflow-hidden bg-background text-foreground 2xl:flex 2xl:h-auto 2xl:min-h-svh 2xl:items-center 2xl:justify-center 2xl:overflow-visible 2xl:p-6",
        className,
      )}
    >
      <ThemeToggle className="absolute top-6 right-6 z-30" />
      <div
        className={cn(
          "relative mx-auto flex h-full w-full max-w-[1600px]",
          "2xl:h-auto 2xl:min-h-0 2xl:w-[min(94vw,calc(92svh*16/9))] 2xl:max-w-none 2xl:aspect-[16/9]",
          "2xl:overflow-hidden 2xl:rounded-2xl 2xl:border 2xl:border-border/70 2xl:bg-background/95 2xl:shadow-[0_25px_80px_-24px_rgba(0,0,0,0.75)]",
          frameClassName,
        )}
      >
        <div
          className={cn(
            "relative hidden flex-1 overflow-hidden border-border/60 border-r lg:block",
            leftClassName,
          )}
        >
          {left}
        </div>
        <div
          className={cn(
            "relative flex w-full flex-col items-center justify-center overflow-y-auto px-6 py-10 lg:w-[560px] lg:px-14",
            rightClassName,
          )}
        >
          {right}
        </div>
      </div>
    </div>
  );
}
```
---

### OrbitAvatar <a name="orbitavatar"></a>

Animated orbiting avatar component.

- **Relative Path**: `packages/ui/src/components/orbit-avatar.tsx`

#### Props & API Reference

```typescript
export type OrbitAvatarProps = {
  seed: string;
  size?: number;
  /** "auto" (default) tints dots with the seeded palette; "mono" uses currentColor. */
  tone?: OrbitAvatarTone;
  /** Draw a faint filled circle background (useful when standalone). */
  backdrop?: boolean;
  className?: string;
  style?: CSSProperties;
  title?: string;
}
```

#### Component Source Code

```tsx
import type { CSSProperties } from "react";
import { cn } from "../lib/utils";
import {
  AVATAR_DOT_TONES,
  AVATAR_TONE_COUNT,
  hashAvatarSeed,
} from "../lib/avatar-tones";

export type OrbitRing = {
  radius: number;
  dotCount: number;
  rotation: number;
  dotSize: number;
  /** Angular range `[start, end]` (radians) to leave empty for an irregular arc. */
  arcGap?: [number, number];
  alpha: number;
  /** Radians per second, signed. Only used by the animated canvas variant. */
  speed: number;
};

export type OrbitPlanet = {
  ringIndex: number;
  angle: number;
  size: number;
};

export type OrbitSpec = {
  seed: string;
  toneIndex: number;
  core: { size: number; alpha: number };
  rings: OrbitRing[];
  planets: OrbitPlanet[];
  /** Sprinkle a handful of tiny off-ring stars for extra character. */
  stars: Array<{ x: number; y: number; size: number; alpha: number }>;
};

function mulberry32(seed: number): () => number {
  let s = seed >>> 0;
  return () => {
    s = (s + 0x6d2b79f5) >>> 0;
    let t = s;
    t = Math.imul(t ^ (t >>> 15), t | 1);
    t ^= t + Math.imul(t ^ (t >>> 7), t | 61);
    return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
  };
}

/**
 * Pure, deterministic geometry for a seeded orbit avatar.
 * Coordinate space is a normalized 100x100 viewbox centered at (50,50).
 */
export function orbitGeometry(seed: string): OrbitSpec {
  const h = hashAvatarSeed(seed || "orbit");
  const rand = mulberry32(h);
  const toneIndex = h % AVATAR_TONE_COUNT;

  const ringCount = 2 + Math.floor(rand() * 3); // 2..4
  const rings: OrbitRing[] = [];
  const innerRadius = 15 + rand() * 5;
  const outerRadius = 40 + rand() * 6;
  const step = (outerRadius - innerRadius) / Math.max(1, ringCount - 1);

  for (let i = 0; i < ringCount; i++) {
    const radius = innerRadius + step * i + (rand() - 0.5) * 2.2;
    const circumference = 2 * Math.PI * radius;
    // target ~4.5 units of arc per dot, with per-ring jitter
    const spacing = 3.8 + rand() * 2.4;
    const dotCount = Math.max(
      6,
      Math.min(24, Math.round(circumference / spacing)),
    );
    const rotation = rand() * Math.PI * 2;
    const dotSize = 0.95 + rand() * 0.75;
    // keep a high floor so rings stay legible on both light and dark backgrounds;
    // only the outermost ring softens slightly.
    const alphaBase = 0.72 + rand() * 0.22;
    const alpha = Math.max(0.58, alphaBase - i * 0.04);
    // direction + speed (radians/sec); adjacent rings go opposite ways
    const dir = i % 2 === 0 ? 1 : -1;
    const speed = dir * (0.03 + rand() * 0.08);

    let arcGap: [number, number] | undefined;
    if (rand() < 0.55) {
      const gapStart = rand() * Math.PI * 2;
      const gapSize = Math.PI * (0.18 + rand() * 0.28);
      arcGap = [gapStart, gapStart + gapSize];
    }

    rings.push({
      radius,
      dotCount,
      rotation,
      dotSize,
      arcGap,
      alpha,
      speed,
    });
  }

  const planetCount = 1 + Math.floor(rand() * 3); // 1..3
  const planets: OrbitPlanet[] = [];
  for (let i = 0; i < planetCount; i++) {
    planets.push({
      ringIndex: Math.floor(rand() * rings.length),
      angle: rand() * Math.PI * 2,
      size: 2.4 + rand() * 1.8,
    });
  }

  const starCount = 2 + Math.floor(rand() * 4); // 2..5
  const stars: OrbitSpec["stars"] = [];
  for (let i = 0; i < starCount; i++) {
    // place beyond the outermost ring, inside the 100x100 box
    const a = rand() * Math.PI * 2;
    const r = outerRadius + 4 + rand() * 6;
    stars.push({
      x: 50 + Math.cos(a) * r,
      y: 50 + Math.sin(a) * r,
      size: 0.45 + rand() * 0.5,
      alpha: 0.45 + rand() * 0.35,
    });
  }

  return {
    seed,
    toneIndex,
    core: { size: 1.8 + rand() * 1.2, alpha: 0.9 + rand() * 0.08 },
    rings,
    planets,
    stars,
  };
}

/**
 * Walk a ring and emit (x, y) for each dot, skipping dots that fall inside
 * the optional arc gap. Shared by SVG and canvas renderers.
 */
export function forEachRingDot(
  ring: OrbitRing,
  rotationDelta: number,
  visit: (x: number, y: number, angle: number) => void,
): void {
  const step = (Math.PI * 2) / ring.dotCount;
  for (let i = 0; i < ring.dotCount; i++) {
    const angle = ring.rotation + rotationDelta + i * step;
    if (ring.arcGap) {
      // normalize the dot angle to [0, 2pi)
      const norm = ((angle % (Math.PI * 2)) + Math.PI * 2) % (Math.PI * 2);
      const [gs, ge] = ring.arcGap;
      const gsN = ((gs % (Math.PI * 2)) + Math.PI * 2) % (Math.PI * 2);
      const geN = ((ge % (Math.PI * 2)) + Math.PI * 2) % (Math.PI * 2);
      const inGap =
        gsN <= geN
          ? norm >= gsN && norm <= geN
          : norm >= gsN || norm <= geN;
      if (inGap) continue;
    }
    visit(50 + Math.cos(angle) * ring.radius, 50 + Math.sin(angle) * ring.radius, angle);
  }
}

export type OrbitAvatarTone = "auto" | "mono";

export type OrbitAvatarProps = {
  seed: string;
  size?: number;
  /** "auto" (default) tints dots with the seeded palette; "mono" uses currentColor. */
  tone?: OrbitAvatarTone;
  /** Draw a faint filled circle background (useful when standalone). */
  backdrop?: boolean;
  className?: string;
  style?: CSSProperties;
  title?: string;
};

export function OrbitAvatar({
  seed,
  size = 24,
  tone = "auto",
  backdrop = false,
  className,
  style,
  title,
}: OrbitAvatarProps) {
  const spec = orbitGeometry(seed);
  const toneClass =
    tone === "auto" ? AVATAR_DOT_TONES[spec.toneIndex] : undefined;

  return (
    <span
      aria-hidden={title ? undefined : true}
      role={title ? "img" : undefined}
      aria-label={title}
      className={cn(
        "inline-block shrink-0 align-middle leading-none",
        toneClass,
        className,
      )}
      style={{ width: size, height: size, ...style }}
    >
      <svg
        viewBox="0 0 100 100"
        width={size}
        height={size}
        xmlns="http://www.w3.org/2000/svg"
      >
        {backdrop ? (
          <circle cx="50" cy="50" r="48" fill="currentColor" opacity={0.12} />
        ) : null}

        {spec.stars.map((s, i) => (
          <circle
            key={`s${i}`}
            cx={s.x}
            cy={s.y}
            r={s.size}
            fill="currentColor"
            opacity={s.alpha}
          />
        ))}

        {spec.rings.map((ring, ringIndex) => {
          const dots: Array<{ x: number; y: number; k: number }> = [];
          forEachRingDot(ring, 0, (x, y, angle) => {
            dots.push({ x, y, k: angle });
          });
          return (
            <g key={`r${ringIndex}`}>
              {dots.map((d, j) => (
                <circle
                  key={j}
                  cx={d.x}
                  cy={d.y}
                  r={ring.dotSize}
                  fill="currentColor"
                  opacity={ring.alpha}
                />
              ))}
            </g>
          );
        })}

        {spec.planets.map((p, i) => {
          const ring = spec.rings[p.ringIndex];
          if (!ring) return null;
          const x = 50 + Math.cos(p.angle + ring.rotation) * ring.radius;
          const y = 50 + Math.sin(p.angle + ring.rotation) * ring.radius;
          return (
            <circle
              key={`p${i}`}
              cx={x}
              cy={y}
              r={p.size}
              fill="currentColor"
              opacity={0.95}
            />
          );
        })}

        <circle
          cx="50"
          cy="50"
          r={spec.core.size}
          fill="currentColor"
          opacity={spec.core.alpha}
        />
      </svg>
    </span>
  );
}
```
---

### OrbitAvatarField <a name="orbitavatarfield"></a>

Form field layout showcasing multiple orbiting avatars.

- **Relative Path**: `packages/ui/src/components/orbit-avatar-field.tsx`

#### Props & API Reference

```typescript
export type OrbitAvatarFieldProps = {
  seed: string;
  size?: number;
  tone?: OrbitAvatarTone;
  backdrop?: boolean;
  className?: string;
  style?: CSSProperties;
  title?: string;
}
```

#### Component Source Code

```tsx
"use client";

import { type CSSProperties, useEffect, useRef } from "react";
import { cn } from "../lib/utils";
import { AVATAR_DOT_TONES } from "../lib/avatar-tones";
import {
  forEachRingDot,
  orbitGeometry,
  type OrbitAvatarTone,
} from "./orbit-avatar";

export type OrbitAvatarFieldProps = {
  seed: string;
  size?: number;
  tone?: OrbitAvatarTone;
  backdrop?: boolean;
  className?: string;
  style?: CSSProperties;
  title?: string;
};

/**
 * Animated canvas variant of `OrbitAvatar`. Rings rotate slowly, dots twinkle,
 * the core pulses gently. Intended for hero surfaces (app logo, room header).
 * Respects `prefers-reduced-motion` by painting a single static frame.
 */
export function OrbitAvatarField({
  seed,
  size = 32,
  tone = "auto",
  backdrop = false,
  className,
  style,
  title,
}: OrbitAvatarFieldProps) {
  const canvasRef = useRef<HTMLCanvasElement | null>(null);
  const spec = orbitGeometryCached(seed);
  const toneClass =
    tone === "auto" ? AVATAR_DOT_TONES[spec.toneIndex] : undefined;

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const ctx = canvas.getContext("2d", { alpha: true });
    if (!ctx) return;

    const dpr = Math.min(window.devicePixelRatio || 1, 2);
    canvas.width = size * dpr;
    canvas.height = size * dpr;
    canvas.style.width = `${size}px`;
    canvas.style.height = `${size}px`;

    const prefersReduce = window.matchMedia(
      "(prefers-reduced-motion: reduce)",
    ).matches;

    let rafId = 0;
    let start = 0;
    let destroyed = false;

    const scale = size / 100;

    const resolveCurrentColor = (): string => {
      const cs = getComputedStyle(canvas);
      return cs.color || "#ffffff";
    };

    const paint = (t: number) => {
      if (destroyed) return;
      const color = resolveCurrentColor();
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.save();
      ctx.scale(dpr * scale, dpr * scale);
      ctx.fillStyle = color;

      if (backdrop) {
        ctx.globalAlpha = 0.12;
        ctx.beginPath();
        ctx.arc(50, 50, 48, 0, Math.PI * 2);
        ctx.fill();
      }

      for (const s of spec.stars) {
        const tw = 0.7 + Math.sin(t * 1.2 + (s.x + s.y)) * 0.3;
        ctx.globalAlpha = s.alpha * tw;
        ctx.beginPath();
        ctx.arc(s.x, s.y, s.size, 0, Math.PI * 2);
        ctx.fill();
      }

      spec.rings.forEach((ring, ri) => {
        const delta = ring.speed * t;
        forEachRingDot(ring, delta, (x, y, angle) => {
          const tw = 0.85 + Math.sin(t * 1.6 + angle * 3 + ri) * 0.15;
          ctx.globalAlpha = ring.alpha * tw;
          ctx.beginPath();
          ctx.arc(x, y, ring.dotSize, 0, Math.PI * 2);
          ctx.fill();
        });
      });

      for (const p of spec.planets) {
        const ring = spec.rings[p.ringIndex];
        if (!ring) continue;
        const a = p.angle + ring.rotation + ring.speed * t;
        const x = 50 + Math.cos(a) * ring.radius;
        const y = 50 + Math.sin(a) * ring.radius;
        ctx.globalAlpha = 0.95;
        ctx.beginPath();
        ctx.arc(x, y, p.size, 0, Math.PI * 2);
        ctx.fill();
      }

      const pulse = 1 + Math.sin(t * 2.1) * 0.08;
      ctx.globalAlpha = spec.core.alpha;
      ctx.beginPath();
      ctx.arc(50, 50, spec.core.size * pulse, 0, Math.PI * 2);
      ctx.fill();

      ctx.restore();
      ctx.globalAlpha = 1;
    };

    const loop = (now: number) => {
      if (destroyed) return;
      if (!start) start = now;
      const t = (now - start) / 1000;
      paint(t);
      rafId = requestAnimationFrame(loop);
    };

    if (prefersReduce) {
      paint(0);
    } else {
      rafId = requestAnimationFrame(loop);
    }

    return () => {
      destroyed = true;
      cancelAnimationFrame(rafId);
    };
  }, [spec, size, backdrop]);

  return (
    <span
      aria-hidden={title ? undefined : true}
      role={title ? "img" : undefined}
      aria-label={title}
      className={cn(
        "inline-block shrink-0 align-middle leading-none",
        toneClass,
        className,
      )}
      style={{ width: size, height: size, ...style }}
    >
      <canvas ref={canvasRef} style={{ display: "block" }} />
    </span>
  );
}

// Tiny LRU to avoid re-running the geometry loop on every render for the same seed.
const geometryCache = new Map<string, ReturnType<typeof orbitGeometry>>();
const GEOMETRY_CACHE_LIMIT = 256;

function orbitGeometryCached(seed: string) {
  const hit = geometryCache.get(seed);
  if (hit) return hit;
  const spec = orbitGeometry(seed);
  if (geometryCache.size >= GEOMETRY_CACHE_LIMIT) {
    const first = geometryCache.keys().next().value;
    if (first !== undefined) geometryCache.delete(first);
  }
  geometryCache.set(seed, spec);
  return spec;
}
```
---

### AppearancePicker <a name="appearancepicker"></a>

Theme selector component.

- **Relative Path**: `packages/ui/src/components/appearance-picker.tsx`

#### Props & API Reference

```typescript
export interface AppearancePickerProps {
  /**
   * Called after an optimistic local change with the field that
   * changed. Implementations should PATCH /v1/me/preferences. Errors
   * should be surfaced by the caller (toast / log) — the picker does
   * not attempt to revert on failure (by convention a failed
   * persistence is surfaced but UI stays on the user's chosen value
   * since the local cache is already consistent).
   */
  onPersist: (
    input:
      | { themeMode: OrbitThemeMode }
      | { themePalette: OrbitThemePalette },
  ) => void | Promise<void>;
}
```

#### Component Source Code

```tsx
"use client";

import {
  ORBIT_THEME_MODES,
  type OrbitThemeMode,
  type OrbitThemePalette,
} from "../themes/types.ts";
import { PALETTES } from "../themes/palettes.ts";
import { useTheme, type OrbitThemePreference } from "./theme-provider.tsx";

export interface AppearancePickerProps {
  /**
   * Called after an optimistic local change with the field that
   * changed. Implementations should PATCH /v1/me/preferences. Errors
   * should be surfaced by the caller (toast / log) — the picker does
   * not attempt to revert on failure (by convention a failed
   * persistence is surfaced but UI stays on the user's chosen value
   * since the local cache is already consistent).
   */
  onPersist: (
    input:
      | { themeMode: OrbitThemeMode }
      | { themePalette: OrbitThemePalette },
  ) => void | Promise<void>;
}

const MODE_LABELS: Record<OrbitThemePreference, string> = {
  light: "Light",
  dark: "Dark",
  system: "System",
};

/**
 * Neutral surface tokens used by the miniature app chrome shown in
 * each preview card. Raw hexes rather than CSS vars so each card
 * can render in a *specific* scheme regardless of what's currently
 * applied to `<html>` — the palette cards respect the user's
 * currently-resolved mode, the mode cards show their own scheme.
 */
const SURFACES = {
  light: {
    bg: "#fafafa",
    sidebar: "#f4f4f5",
    sidebarBorder: "#e4e4e7",
    card: "#ffffff",
    border: "#e4e4e7",
    muted: "#d4d4d8",
    mutedSoft: "#e4e4e7",
  },
  dark: {
    bg: "#0a0a0a",
    sidebar: "#101012",
    sidebarBorder: "#27272a",
    card: "#141416",
    border: "#27272a",
    muted: "#3f3f46",
    mutedSoft: "#27272a",
  },
} as const;

type Scheme = keyof typeof SURFACES;

export function AppearancePicker({ onPersist }: AppearancePickerProps) {
  const { preference, palette, setPreference, setPalette, resolved } =
    useTheme();

  return (
    <div className="space-y-10">
      <section>
        <div className="mb-3 text-[11px] uppercase tracking-[0.15em] text-muted-foreground">
          Mode
        </div>
        <div className="grid max-w-xl grid-cols-3 gap-3">
          {ORBIT_THEME_MODES.map((m) => (
            <button
              key={m}
              type="button"
              data-selected={preference === m}
              onClick={() => {
                setPreference(m);
                void onPersist({ themeMode: m });
              }}
              className="group overflow-hidden rounded-xl border border-border bg-card text-left transition-colors hover:border-ring focus:outline-none focus-visible:ring-2 focus-visible:ring-ring data-[selected=true]:border-primary"
              aria-pressed={preference === m}
            >
              <ModePreviewCard mode={m} accent={PALETTES[palette].swatch[0]} />
              <div className="flex items-center justify-between px-3 py-2 text-xs">
                <span>{MODE_LABELS[m]}</span>
                {preference === m ? (
                  <span className="text-primary" aria-hidden>
                    ●
                  </span>
                ) : null}
              </div>
            </button>
          ))}
        </div>
      </section>

      <section>
        <div className="mb-3 text-[11px] uppercase tracking-[0.15em] text-muted-foreground">
          Palette
        </div>
        <div className="grid max-w-xl grid-cols-3 gap-3">
          {Object.values(PALETTES).map((p) => (
            <button
              key={p.id}
              type="button"
              data-selected={palette === p.id}
              onClick={() => {
                setPalette(p.id);
                void onPersist({ themePalette: p.id });
              }}
              className="group overflow-hidden rounded-xl border border-border bg-card text-left transition-colors hover:border-ring focus:outline-none focus-visible:ring-2 focus-visible:ring-ring data-[selected=true]:border-primary"
              aria-pressed={palette === p.id}
            >
              <MiniAppChrome
                scheme={resolved}
                accent={p.swatch[0]}
                accentSoft={p.swatch[1]}
              />
              <div className="flex items-center justify-between px-3 py-2 text-xs">
                <span className="flex items-center gap-2">
                  {p.name}
                  {palette === p.id ? (
                    <span className="text-primary" aria-hidden>
                      ●
                    </span>
                  ) : null}
                </span>
                <span className="flex gap-1" aria-hidden>
                  <span
                    className="size-2 rounded-full"
                    style={{ background: p.swatch[0] }}
                  />
                  <span
                    className="size-2 rounded-full"
                    style={{ background: p.swatch[1] }}
                  />
                </span>
              </div>
            </button>
          ))}
        </div>
      </section>
    </div>
  );
}

/**
 * Mode preview. Renders the mini-app in its own scheme (light card
 * → light chrome, dark card → dark chrome, system card → split).
 * Uses the user's currently-selected palette accent so the preview
 * feels personal instead of a generic neutral.
 */
function ModePreviewCard({
  mode,
  accent,
}: {
  mode: OrbitThemeMode;
  accent: string;
}) {
  if (mode === "system") {
    return (
      <div aria-hidden className="relative h-[108px] w-full overflow-hidden">
        <div className="absolute inset-0" style={{ clipPath: "polygon(0 0, 100% 0, 0 100%)" }}>
          <MiniAppChrome scheme="light" accent={accent} accentSoft={accent} />
        </div>
        <div className="absolute inset-0" style={{ clipPath: "polygon(100% 0, 100% 100%, 0 100%)" }}>
          <MiniAppChrome scheme="dark" accent={accent} accentSoft={accent} />
        </div>
      </div>
    );
  }
  return <MiniAppChrome scheme={mode} accent={accent} accentSoft={accent} />;
}

/**
 * Miniature Orbit app chrome: left rail with workspace dot + two
 * dim items + one accent-highlighted active item, main area with a
 * card containing a title row, a body line, and a primary button.
 * Uses `scheme` to pick neutral surfaces and `accent` / `accentSoft`
 * for the palette's primary + softer variant.
 */
function MiniAppChrome({
  scheme,
  accent,
  accentSoft,
}: {
  scheme: Scheme;
  accent: string;
  accentSoft: string;
}) {
  const s = SURFACES[scheme];
  return (
    <div
      aria-hidden
      className="flex h-[108px] w-full"
      style={{ background: s.bg }}
    >
      {/* Rail */}
      <div
        className="flex w-[26px] flex-col items-center gap-2 border-r py-2"
        style={{ background: s.sidebar, borderColor: s.sidebarBorder }}
      >
        <span
          className="size-3 rounded-md"
          style={{ background: accent }}
        />
        <span
          className="mt-1 size-2 rounded-sm"
          style={{ background: s.muted }}
        />
        <span
          className="size-2 rounded-sm"
          style={{ background: accentSoft, opacity: 0.75 }}
        />
        <span
          className="size-2 rounded-sm"
          style={{ background: s.mutedSoft }}
        />
      </div>

      {/* Main area */}
      <div className="flex min-w-0 flex-1 flex-col gap-2 p-2.5">
        {/* Title row */}
        <div className="flex items-center gap-1.5">
          <span
            className="h-1.5 w-14 rounded-full"
            style={{ background: s.muted }}
          />
          <span
            className="ml-auto h-1.5 w-5 rounded-full"
            style={{ background: s.mutedSoft }}
          />
        </div>

        {/* Card */}
        <div
          className="flex flex-1 flex-col justify-between rounded-md border p-2"
          style={{ background: s.card, borderColor: s.border }}
        >
          <div className="flex flex-col gap-1">
            <span
              className="h-1 w-16 rounded-full"
              style={{ background: s.muted }}
            />
            <span
              className="h-1 w-10 rounded-full"
              style={{ background: s.mutedSoft }}
            />
          </div>
          <div className="flex items-center gap-1.5">
            <span
              className="flex h-3.5 items-center justify-center rounded px-1.5 text-[7px] font-semibold text-white"
              style={{ background: accent }}
            >
              Go
            </span>
            <span
              className="h-1 flex-1 rounded-full"
              style={{ background: accentSoft, opacity: 0.5 }}
            />
          </div>
        </div>
      </div>
    </div>
  );
}
```
---

## Showcase Compositions

### Login Showcase <a name="login-showcase"></a>

Auth layout showcasing Split Layout auth with welcome.png particle fields.

- **Relative Path**: `apps/www/src/pages/showcase/login.tsx`

#### Showcase Page Source Code

```tsx
import { type FormEvent, useRef, useState } from "react";
import { Button } from "@orbit/ui/button";
import { Input } from "@orbit/ui/input";
import { Label } from "@orbit/ui/label";
import { Separator } from "@orbit/ui/separator";
import { Kbd } from "@orbit/ui/kbd";
import {
  bumpParticleTypingImpulse,
  pulseParticleSubmitImpulse,
} from "@orbit/ui/particle-field";
import { AuthShell, useAuthTypingImpulse } from "./auth-shell";

export function LoginShowcasePage() {
  return (
    <AuthShell variant="welcome">
      <LoginForm />
    </AuthShell>
  );
}

function LoginForm() {
  return (
    <>
      <div className="absolute top-6 left-6 flex items-center gap-2 font-mono text-sm lg:hidden">
        <span className="inline-block h-2 w-2 rounded-full bg-foreground" />
        <span className="tracking-[0.2em] uppercase">Sean's scratch pad</span>
      </div>

      <div className="w-full max-w-lg">
        <div className="font-mono text-[11px] text-muted-foreground uppercase tracking-[0.3em]">
          Welcome back
        </div>
        <h1 className="mt-2 font-heading text-3xl leading-tight">
          Enter your orbit
        </h1>
        <p className="mt-2 text-muted-foreground text-sm">Sign in to continue.</p>

        <MagicLinkForm />
        <OrSeparator />
        <OAuthButtons />
      </div>
    </>
  );
}

function OrSeparator() {
  return (
    <div className="my-6 flex items-center gap-3">
      <Separator className="flex-1" />
      <span className="font-mono text-[10px] text-muted-foreground uppercase tracking-[0.3em]">
        or
      </span>
      <Separator className="flex-1" />
    </div>
  );
}

function MagicLinkForm() {
  const formRef = useRef<HTMLFormElement>(null);
  const typingImpulse = useAuthTypingImpulse();
  const [email, setEmail] = useState("");
  const [sentTo, setSentTo] = useState<string | null>(null);
  const [pending, setPending] = useState(false);

  const onSubmit = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    if (!email.trim()) return;
    pulseParticleSubmitImpulse(typingImpulse);
    setPending(true);
    window.setTimeout(() => {
      setSentTo(email.trim().toLowerCase());
      setPending(false);
    }, 600);
  };

  return (
    <>
      <form
        ref={formRef}
        onSubmit={onSubmit}
        onKeyDown={(e) => bumpParticleTypingImpulse(typingImpulse, e)}
        className="mt-8 flex flex-col gap-4"
      >
        <div className="flex flex-col gap-1.5">
          <Label htmlFor="magic-link-email">Email</Label>
          <Input
            id="magic-link-email"
            type="email"
            required
            placeholder="you@example.com"
            autoComplete="email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
            nativeInput
          />
        </div>

        <Button type="submit" size="lg" loading={pending} className="mt-2">
          Send sign-in link
        </Button>
        {!sentTo ? (
          <p className="text-center text-muted-foreground text-xs">
            <Kbd className="font-mono">⌘↵</Kbd> to submit
          </p>
        ) : null}
      </form>

      {sentTo ? (
        <div className="mt-4 rounded-lg border border-border/70 bg-background/40 px-3 py-2 text-muted-foreground text-sm">
          Link sent to <span className="text-foreground">{sentTo}</span>. Open
          your inbox to continue.
        </div>
      ) : null}
    </>
  );
}

function OAuthButtons() {
  return (
    <div className="flex flex-col gap-2">
      <Button variant="outline" size="lg" type="button">
        <GoogleIcon />
        Continue with Google
      </Button>
      <Button variant="outline" size="lg" type="button">
        <AppleIcon />
        Continue with Apple
      </Button>
    </div>
  );
}

function GoogleIcon() {
  return (
    <svg viewBox="0 0 24 24" aria-hidden="true" className="size-4">
      <path
        fill="currentColor"
        d="M21.35 11.1H12v2.98h5.35c-.23 1.4-1.64 4.1-5.35 4.1-3.22 0-5.85-2.67-5.85-5.95s2.63-5.95 5.85-5.95c1.84 0 3.07.78 3.77 1.45l2.57-2.5C16.71 3.8 14.59 2.9 12 2.9 6.97 2.9 2.9 6.97 2.9 12s4.07 9.1 9.1 9.1c5.26 0 8.74-3.69 8.74-8.89 0-.6-.06-1.05-.14-1.51Z"
      />
    </svg>
  );
}

function AppleIcon() {
  return (
    <svg viewBox="0 0 24 24" aria-hidden="true" className="size-4">
      <path
        fill="currentColor"
        d="M16.37 1.43c.06 1.2-.39 2.37-1.17 3.2-.8.85-2.08 1.5-3.28 1.41-.09-1.19.5-2.37 1.21-3.13.8-.88 2.16-1.52 3.24-1.48ZM20.5 17.33c-.55 1.27-.82 1.84-1.53 2.96-.99 1.57-2.39 3.53-4.12 3.54-1.54.02-1.94-1-4.03-.99-2.1.01-2.54 1-4.08.98-1.73-.02-3.06-1.78-4.05-3.35-2.77-4.4-3.06-9.56-1.35-12.31 1.21-1.95 3.12-3.1 4.91-3.1 1.82 0 2.97.99 4.47.99 1.46 0 2.35-1 4.45-1 1.59 0 3.27.86 4.47 2.36-3.93 2.15-3.29 7.76 1.06 9.92Z"
      />
    </svg>
  );
}
```
---

### Inset Login Showcase <a name="inset-login-showcase"></a>

Auth layout showcasing inset form structure.

- **Relative Path**: `apps/www/src/pages/showcase/inset-login.tsx`

#### Showcase Page Source Code

```tsx
import { type FormEvent, useEffect, useRef, useState } from "react";
import {
  ChevronLeftIcon,
  EyeIcon,
  EyeOffIcon,
  MailIcon,
  ShuffleIcon,
} from "lucide-react";
import { Button } from "@orbit/ui/button";
import { Field, FieldLabel } from "@orbit/ui/field";
import { Input } from "@orbit/ui/input";
import {
  InputGroup,
  InputGroupAddon,
  InputGroupInput,
} from "@orbit/ui/input-group";
import { Separator } from "@orbit/ui/separator";

export function InsetLoginShowcasePage() {
  return (
    <div className="relative min-h-svh bg-background text-foreground">
      <PageBackdrop />
      <div className="relative mx-auto flex min-h-svh max-w-[1440px] items-stretch p-4 md:p-10 lg:p-16">
        <div className="relative grid w-full grid-cols-1 overflow-hidden rounded-3xl border border-border bg-card shadow-2xl shadow-foreground/10 lg:grid-cols-[1fr_minmax(440px,560px)]">
          <LeftPanel />
          <RightPanel />
        </div>
      </div>
    </div>
  );
}

function PageBackdrop() {
  return (
    <div
      aria-hidden
      className="pointer-events-none absolute inset-0"
      style={{
        background: [
          "radial-gradient(60% 50% at 10% 10%, color-mix(in srgb, var(--primary) 10%, transparent), transparent 65%)",
          "radial-gradient(50% 50% at 90% 100%, color-mix(in srgb, var(--foreground) 6%, transparent), transparent 65%)",
        ].join(", "),
      }}
    />
  );
}

type Palette = {
  name: string;
  colors: [
    [number, number, number],
    [number, number, number],
    [number, number, number],
    [number, number, number],
  ];
};

const PALETTES: Palette[] = [
  {
    name: "Slate",
    colors: [
      [0.04, 0.04, 0.07],
      [0.13, 0.13, 0.17],
      [0.22, 0.22, 0.30],
      [0.06, 0.06, 0.10],
    ],
  },
  {
    name: "Aurora",
    colors: [
      [0.05, 0.08, 0.16],
      [0.10, 0.32, 0.48],
      [0.42, 0.20, 0.58],
      [0.06, 0.10, 0.18],
    ],
  },
  {
    name: "Sunset",
    colors: [
      [0.10, 0.05, 0.10],
      [0.62, 0.22, 0.28],
      [0.50, 0.38, 0.20],
      [0.18, 0.05, 0.12],
    ],
  },
  {
    name: "Forest",
    colors: [
      [0.04, 0.09, 0.07],
      [0.08, 0.32, 0.26],
      [0.10, 0.20, 0.32],
      [0.05, 0.11, 0.10],
    ],
  },
  {
    name: "Plum",
    colors: [
      [0.10, 0.05, 0.14],
      [0.50, 0.16, 0.50],
      [0.26, 0.10, 0.42],
      [0.08, 0.04, 0.12],
    ],
  },
  {
    name: "Cyber",
    colors: [
      [0.04, 0.06, 0.12],
      [0.06, 0.42, 0.52],
      [0.55, 0.10, 0.38],
      [0.05, 0.05, 0.12],
    ],
  },
  {
    name: "Ember",
    colors: [
      [0.10, 0.04, 0.04],
      [0.62, 0.26, 0.10],
      [0.45, 0.10, 0.10],
      [0.10, 0.05, 0.05],
    ],
  },
  {
    name: "Ice",
    colors: [
      [0.05, 0.07, 0.12],
      [0.20, 0.32, 0.55],
      [0.32, 0.42, 0.58],
      [0.07, 0.09, 0.16],
    ],
  },
];

function LeftPanel() {
  const [paletteIndex, setPaletteIndex] = useState(1);
  const palette = PALETTES[paletteIndex];

  const shuffle = () => {
    setPaletteIndex((current) => {
      let next = current;
      while (next === current) {
        next = Math.floor(Math.random() * PALETTES.length);
      }
      return next;
    });
  };

  return (
    <div className="relative hidden overflow-hidden bg-[#0a0a0c] lg:block">
      <MeshShader palette={palette} />
      <div
        aria-hidden
        className="pointer-events-none absolute inset-0"
        style={{
          background:
            "radial-gradient(120% 80% at 50% 50%, transparent 35%, rgba(0,0,0,0.55) 100%)",
        }}
      />

      <div className="relative flex h-full flex-col justify-between p-12">
        <div className="flex items-start justify-between">
          <a
            href="#"
            className="inline-flex w-fit items-center gap-1.5 rounded-md bg-black/20 px-2 py-1 font-mono text-[11px] text-white/65 uppercase tracking-[0.2em] backdrop-blur-sm transition-colors hover:text-white"
          >
            <ChevronLeftIcon className="size-3.5" />
            Back
          </a>
          <ShuffleButton onClick={shuffle} paletteName={palette.name} />
        </div>

        <div className="max-w-md">
          <h2
            className="font-heading font-semibold text-3xl leading-tight md:text-4xl"
            style={{ textShadow: "0 1px 24px rgba(0,0,0,0.55)" }}
          >
            <span className="text-white">Orbit.</span>
            <br />
            <span className="text-white/55">The SaaS starter</span>{" "}
            <span className="text-white">you want</span>{" "}
            <span className="text-white/55">and need.</span>
          </h2>
          <p
            className="mt-5 max-w-sm text-sm text-white/65 leading-relaxed"
            style={{ textShadow: "0 1px 16px rgba(0,0,0,0.55)" }}
          >
            Auth, billing, emails — the boring week, already wired up.
          </p>
        </div>
      </div>
    </div>
  );
}

function ShuffleButton({
  onClick,
  paletteName,
}: {
  onClick: () => void;
  paletteName: string;
}) {
  return (
    <button
      type="button"
      onClick={onClick}
      className="group inline-flex items-center gap-2 rounded-md border border-white/10 bg-black/30 px-2.5 py-1.5 text-white/75 backdrop-blur-sm transition-colors hover:border-white/20 hover:bg-black/40 hover:text-white focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-white/30"
    >
      <ShuffleIcon className="size-3.5 transition-transform group-hover:rotate-12" />
      <span className="font-mono text-[10px] uppercase tracking-[0.2em]">
        {paletteName}
      </span>
    </button>
  );
}

const VERT_SRC = `
attribute vec2 a_position;
void main() {
  gl_Position = vec4(a_position, 0.0, 1.0);
}`;

const FRAG_SRC = `
precision mediump float;
uniform vec2 u_resolution;
uniform float u_time;
uniform vec3 u_c0;
uniform vec3 u_c1;
uniform vec3 u_c2;
uniform vec3 u_c3;

void main() {
  vec2 uv = gl_FragCoord.xy / u_resolution;
  uv.y = 1.0 - uv.y;
  float t = u_time * 0.00015;

  vec2 p0 = vec2(0.30 + sin(t * 0.70) * 0.25, 0.25 + cos(t * 0.60) * 0.20);
  vec2 p1 = vec2(0.75 + cos(t * 0.50) * 0.20, 0.70 + sin(t * 0.80) * 0.20);
  vec2 p2 = vec2(0.50 + sin(t * 0.40 + 1.0) * 0.30, 0.50 + cos(t * 0.70) * 0.25);
  vec2 p3 = vec2(0.20 + cos(t * 0.55) * 0.20, 0.85 + sin(t * 0.45) * 0.15);

  float r = 0.55;
  float d0 = pow(1.0 - smoothstep(0.0, r, distance(uv, p0)), 1.4);
  float d1 = pow(1.0 - smoothstep(0.0, r, distance(uv, p1)), 1.4);
  float d2 = pow(1.0 - smoothstep(0.0, r, distance(uv, p2)), 1.4);
  float d3 = pow(1.0 - smoothstep(0.0, r, distance(uv, p3)), 1.4);

  float total = d0 + d1 + d2 + d3 + 0.0001;
  vec3 col = (u_c0 * d0 + u_c1 * d1 + u_c2 * d2 + u_c3 * d3) / total;

  // grain
  float n = fract(sin(dot(gl_FragCoord.xy, vec2(12.9898, 78.233))) * 43758.5453);
  col += (n - 0.5) * 0.025;

  gl_FragColor = vec4(col, 1.0);
}`;

function MeshShader({ palette }: { palette: Palette }) {
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const targetRef = useRef(palette.colors);
  const currentRef = useRef(palette.colors.map((c) => [...c]) as Palette["colors"]);

  useEffect(() => {
    targetRef.current = palette.colors;
  }, [palette]);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const gl = canvas.getContext("webgl", { antialias: false, alpha: false });
    if (!gl) return;

    const compile = (type: number, src: string) => {
      const sh = gl.createShader(type);
      if (!sh) return null;
      gl.shaderSource(sh, src);
      gl.compileShader(sh);
      if (!gl.getShaderParameter(sh, gl.COMPILE_STATUS)) {
        gl.deleteShader(sh);
        return null;
      }
      return sh;
    };

    const vs = compile(gl.VERTEX_SHADER, VERT_SRC);
    const fs = compile(gl.FRAGMENT_SHADER, FRAG_SRC);
    if (!vs || !fs) return;

    const prog = gl.createProgram();
    if (!prog) return;
    gl.attachShader(prog, vs);
    gl.attachShader(prog, fs);
    gl.linkProgram(prog);
    if (!gl.getProgramParameter(prog, gl.LINK_STATUS)) return;
    gl.useProgram(prog);

    const buf = gl.createBuffer();
    gl.bindBuffer(gl.ARRAY_BUFFER, buf);
    gl.bufferData(
      gl.ARRAY_BUFFER,
      new Float32Array([-1, -1, 1, -1, -1, 1, -1, 1, 1, -1, 1, 1]),
      gl.STATIC_DRAW,
    );
    const posLoc = gl.getAttribLocation(prog, "a_position");
    gl.enableVertexAttribArray(posLoc);
    gl.vertexAttribPointer(posLoc, 2, gl.FLOAT, false, 0, 0);

    const uRes = gl.getUniformLocation(prog, "u_resolution");
    const uTime = gl.getUniformLocation(prog, "u_time");
    const uC0 = gl.getUniformLocation(prog, "u_c0");
    const uC1 = gl.getUniformLocation(prog, "u_c1");
    const uC2 = gl.getUniformLocation(prog, "u_c2");
    const uC3 = gl.getUniformLocation(prog, "u_c3");

    const resize = () => {
      const dpr = Math.min(window.devicePixelRatio || 1, 2);
      const w = Math.max(1, Math.floor(canvas.clientWidth * dpr));
      const h = Math.max(1, Math.floor(canvas.clientHeight * dpr));
      if (canvas.width !== w || canvas.height !== h) {
        canvas.width = w;
        canvas.height = h;
        gl.viewport(0, 0, w, h);
      }
    };

    const ro = new ResizeObserver(resize);
    ro.observe(canvas);
    resize();

    let raf = 0;
    const start = performance.now();
    const tick = (now: number) => {
      const t = now - start;
      // ease current toward target (~600ms feel)
      const k = 0.06;
      for (let i = 0; i < 4; i++) {
        for (let j = 0; j < 3; j++) {
          currentRef.current[i][j] +=
            (targetRef.current[i][j] - currentRef.current[i][j]) * k;
        }
      }
      gl.uniform2f(uRes, canvas.width, canvas.height);
      gl.uniform1f(uTime, t);
      const c = currentRef.current;
      gl.uniform3f(uC0, c[0][0], c[0][1], c[0][2]);
      gl.uniform3f(uC1, c[1][0], c[1][1], c[1][2]);
      gl.uniform3f(uC2, c[2][0], c[2][1], c[2][2]);
      gl.uniform3f(uC3, c[3][0], c[3][1], c[3][2]);
      gl.drawArrays(gl.TRIANGLES, 0, 6);
      raf = requestAnimationFrame(tick);
    };
    raf = requestAnimationFrame(tick);

    return () => {
      cancelAnimationFrame(raf);
      ro.disconnect();
      gl.deleteProgram(prog);
      gl.deleteShader(vs);
      gl.deleteShader(fs);
      gl.deleteBuffer(buf);
    };
  }, []);

  return (
    <canvas
      ref={canvasRef}
      aria-hidden
      className="absolute inset-0 block h-full w-full"
    />
  );
}

function RightPanel() {
  return (
    <div className="relative flex min-h-[680px] flex-col overflow-y-auto bg-card text-foreground">
      <div className="mx-auto flex w-full max-w-md flex-1 flex-col px-6 py-10 lg:py-14">
        <BrandMark />
        <Heading />
        <OAuthRow />
        <MagicLinkButton />
        <OrSeparator />
        <SignInForm />
        <FooterLinks />
      </div>
    </div>
  );
}

function BrandMark() {
  return (
    <div className="flex items-center justify-center">
      <OrbitMark />
    </div>
  );
}

function OrbitMark() {
  // Always dark backdrop on the chip so the gradient sphere reads in light + dark.
  return (
    <svg
      viewBox="0 0 64 64"
      aria-hidden
      className="size-10"
      xmlns="http://www.w3.org/2000/svg"
      fill="none"
    >
      <rect width="64" height="64" rx="14" fill="url(#orbit-bg)" />
      <g transform="translate(32 32) rotate(-25)">
        <path
          d="M -23 0 A 23 8 0 0 1 23 0"
          stroke="#ffffff"
          strokeOpacity="0.55"
          strokeWidth="1.8"
          fill="none"
          strokeLinecap="round"
        />
      </g>
      <circle cx="32" cy="32" r="20" fill="url(#orbit-glow)" />
      <circle cx="32" cy="32" r="13" fill="url(#orbit-sphere)" />
      <g transform="translate(32 32) rotate(-25)">
        <path
          d="M 23 0 A 23 8 0 0 1 -23 0"
          stroke="#ffffff"
          strokeOpacity="0.8"
          strokeWidth="1.8"
          fill="none"
          strokeLinecap="round"
        />
      </g>
      <defs>
        <radialGradient id="orbit-bg" cx="50%" cy="15%" r="95%">
          <stop offset="0%" stopColor="#1d1d2c" />
          <stop offset="100%" stopColor="#0a0a0f" />
        </radialGradient>
        <radialGradient id="orbit-glow" cx="50%" cy="50%" r="50%">
          <stop offset="0%" stopColor="#ffffff" stopOpacity="0.28" />
          <stop offset="100%" stopColor="#ffffff" stopOpacity="0" />
        </radialGradient>
        <linearGradient id="orbit-sphere" x1="20%" y1="20%" x2="80%" y2="80%">
          <stop offset="0%" stopColor="#ffffff" />
          <stop offset="60%" stopColor="#b9b9d2" />
          <stop offset="100%" stopColor="#5a5a78" />
        </linearGradient>
      </defs>
    </svg>
  );
}

function Heading() {
  return (
    <div className="mt-12 flex flex-col gap-1.5">
      <h1 className="font-heading font-semibold text-2xl tracking-tight">
        Welcome back
      </h1>
      <p className="text-muted-foreground text-sm">
        Sign in to your account to continue.
      </p>
    </div>
  );
}

function OAuthRow() {
  return (
    <div className="mt-6 grid grid-cols-2 gap-2.5">
      <OAuthButton icon={<GitHubIcon />} label="GitHub" />
      <OAuthButton icon={<GoogleIcon />} label="Google" badge="Last used" />
    </div>
  );
}

function OAuthButton({
  icon,
  label,
  badge,
}: {
  icon: React.ReactNode;
  label: string;
  badge?: string;
}) {
  return (
    <div className="relative">
      {badge ? (
        <span className="-top-2 absolute right-2 z-10 whitespace-nowrap rounded-sm border border-border bg-muted px-1.5 py-0.5 font-mono text-[9px] text-muted-foreground uppercase tracking-[0.15em]">
          {badge}
        </span>
      ) : null}
      <Button variant="outline" size="lg" className="w-full">
        <span className="size-4 shrink-0 [&_svg]:size-4">{icon}</span>
        <span className="truncate whitespace-nowrap">{label}</span>
      </Button>
    </div>
  );
}

function MagicLinkButton() {
  return (
    <Button variant="outline" size="lg" className="mt-2.5 w-full">
      <MailIcon />
      Sign in with Magic Link
    </Button>
  );
}

function OrSeparator() {
  return (
    <div className="my-6 flex items-center gap-3">
      <Separator className="flex-1" />
      <span className="text-[11px] text-muted-foreground">Or</span>
      <Separator className="flex-1" />
    </div>
  );
}

function SignInForm() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [reveal, setReveal] = useState(false);
  const [pending, setPending] = useState(false);

  const onSubmit = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    if (!email.trim() || !password) return;
    setPending(true);
    window.setTimeout(() => setPending(false), 800);
  };

  return (
    <form onSubmit={onSubmit} className="flex flex-col gap-4">
      <Field>
        <FieldLabel htmlFor="login-email">
          Email
          <span className="text-muted-foreground">*</span>
        </FieldLabel>
        <Input
          id="login-email"
          type="email"
          required
          placeholder="Enter your email"
          autoComplete="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          nativeInput
        />
      </Field>

      <Field>
        <FieldLabel htmlFor="login-password">
          Password
          <span className="text-muted-foreground">*</span>
        </FieldLabel>
        <InputGroup>
          <InputGroupInput
            id="login-password"
            type={reveal ? "text" : "password"}
            required
            placeholder="••••••••"
            autoComplete="current-password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            nativeInput
          />
          <InputGroupAddon align="inline-end">
            <button
              type="button"
              onClick={() => setReveal((v) => !v)}
              aria-label={reveal ? "Hide password" : "Show password"}
              className="cursor-pointer rounded p-1 text-muted-foreground transition-colors hover:text-foreground focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring/40"
            >
              {reveal ? (
                <EyeOffIcon className="size-4" />
              ) : (
                <EyeIcon className="size-4" />
              )}
            </button>
          </InputGroupAddon>
        </InputGroup>
      </Field>

      <Button type="submit" size="lg" loading={pending} className="mt-2">
        Sign in
      </Button>
    </form>
  );
}

function FooterLinks() {
  return (
    <div className="mt-5 flex items-baseline justify-between gap-4 text-sm">
      <p className="whitespace-nowrap text-muted-foreground">
        Don't have an account?{" "}
        <a href="#" className="text-foreground hover:underline">
          Sign up
        </a>
      </p>
      <a
        href="#"
        className="whitespace-nowrap text-muted-foreground hover:text-foreground hover:underline"
      >
        Forgot password?
      </a>
    </div>
  );
}

function GitHubIcon() {
  return (
    <svg viewBox="0 0 24 24" aria-hidden fill="currentColor">
      <path d="M12 .5C5.7.5.6 5.6.6 11.9c0 5 3.3 9.3 7.8 10.8.6.1.8-.2.8-.5v-2c-3.2.7-3.9-1.4-3.9-1.4-.5-1.3-1.3-1.7-1.3-1.7-1-.7.1-.7.1-.7 1.2.1 1.8 1.2 1.8 1.2 1 1.8 2.7 1.3 3.4 1 .1-.8.4-1.3.8-1.6-2.5-.3-5.2-1.3-5.2-5.7 0-1.3.5-2.3 1.2-3.1-.1-.3-.5-1.5.1-3.1 0 0 1-.3 3.3 1.2 1-.3 2-.4 3-.4s2 .1 3 .4c2.3-1.5 3.3-1.2 3.3-1.2.6 1.6.2 2.8.1 3.1.8.8 1.2 1.9 1.2 3.1 0 4.4-2.7 5.4-5.2 5.7.4.4.8 1.1.8 2.2v3.3c0 .3.2.6.8.5 4.5-1.5 7.8-5.8 7.8-10.8C23.4 5.6 18.3.5 12 .5Z" />
    </svg>
  );
}

function GoogleIcon() {
  return (
    <svg viewBox="0 0 24 24" aria-hidden>
      <path
        fill="#EA4335"
        d="M12 5c1.7 0 3.2.6 4.4 1.7l3.3-3.3C17.7 1.5 15 .5 12 .5 7.3.5 3.3 3.2 1.4 7.1l3.8 3c.9-2.8 3.5-4.6 6.8-4.6Z"
      />
      <path
        fill="#34A853"
        d="M23.5 12.3c0-.8-.1-1.6-.2-2.3H12v4.4h6.5c-.3 1.5-1.1 2.7-2.4 3.6l3.7 2.9c2.2-2 3.7-5 3.7-8.6Z"
      />
      <path
        fill="#FBBC05"
        d="M5.2 14.1c-.2-.7-.4-1.4-.4-2.1s.1-1.4.4-2.1l-3.8-3C.5 8.7 0 10.3 0 12s.5 3.3 1.4 4.7l3.8-2.6Z"
      />
      <path
        fill="#4285F4"
        d="M12 23.5c3.2 0 5.9-1.1 7.9-2.9l-3.7-2.9c-1 .7-2.4 1.2-4.2 1.2-3.3 0-6-2-6.9-4.6l-3.8 3C3.3 20.8 7.3 23.5 12 23.5Z"
      />
    </svg>
  );
}
```
---

### Onboarding Showcase <a name="onboarding-showcase"></a>

Auth layout showcasing user onboarding flows.

- **Relative Path**: `apps/www/src/pages/showcase/onboarding.tsx`

#### Showcase Page Source Code

```tsx
import { type FormEvent, useState } from "react";
import { Button } from "@orbit/ui/button";
import { Input } from "@orbit/ui/input";
import { Label } from "@orbit/ui/label";
import {
  bumpParticleTypingImpulse,
  pulseParticleSubmitImpulse,
} from "@orbit/ui/particle-field";
import { AuthShell, useAuthTypingImpulse } from "./auth-shell";

const STEPS = ["Workspace", "Invite", "Ready"] as const;

export function OnboardingShowcasePage() {
  return (
    <AuthShell variant="onboarding">
      <OnboardingFlow />
    </AuthShell>
  );
}

function OnboardingFlow() {
  const [step, setStep] = useState(0);
  const [workspace, setWorkspace] = useState("");
  const [invitees, setInvitees] = useState<string[]>([]);
  const [pendingEmail, setPendingEmail] = useState("");
  const typingImpulse = useAuthTypingImpulse();

  const next = () => {
    pulseParticleSubmitImpulse(typingImpulse);
    setStep((s) => Math.min(s + 1, STEPS.length - 1));
  };
  const back = () => setStep((s) => Math.max(s - 1, 0));

  return (
    <div
      className="w-full max-w-lg"
      onKeyDown={(e) => bumpParticleTypingImpulse(typingImpulse, e)}
    >
      <Stepper step={step} />

      {step === 0 ? (
        <WorkspaceStep
          value={workspace}
          onChange={setWorkspace}
          onSubmit={(e) => {
            e.preventDefault();
            if (!workspace.trim()) return;
            next();
          }}
        />
      ) : null}

      {step === 1 ? (
        <InviteStep
          invitees={invitees}
          pending={pendingEmail}
          onPendingChange={setPendingEmail}
          onAdd={(e) => {
            e.preventDefault();
            const trimmed = pendingEmail.trim().toLowerCase();
            if (!trimmed) return;
            if (invitees.includes(trimmed)) {
              setPendingEmail("");
              return;
            }
            setInvitees((prev) => [...prev, trimmed]);
            setPendingEmail("");
          }}
          onRemove={(email) =>
            setInvitees((prev) => prev.filter((e) => e !== email))
          }
          onContinue={next}
          onBack={back}
        />
      ) : null}

      {step === 2 ? (
        <ReadyStep workspace={workspace || "Untitled"} count={invitees.length} />
      ) : null}
    </div>
  );
}

function Stepper({ step }: { step: number }) {
  return (
    <div className="flex items-center gap-2 font-mono text-[10px] text-muted-foreground uppercase tracking-[0.3em]">
      <span>
        Step {String(step + 1).padStart(2, "0")} / {STEPS.length}
      </span>
      <div className="ml-2 flex items-center gap-1.5">
        {STEPS.map((_, i) => (
          <span
            key={i}
            className={`h-1.5 rounded-full transition-all ${
              i === step
                ? "w-5 bg-foreground"
                : i < step
                  ? "w-1.5 bg-foreground/70"
                  : "w-1.5 bg-foreground/20"
            }`}
          />
        ))}
      </div>
    </div>
  );
}

function WorkspaceStep({
  value,
  onChange,
  onSubmit,
}: {
  value: string;
  onChange: (v: string) => void;
  onSubmit: (e: FormEvent) => void;
}) {
  return (
    <>
      <div className="mt-8 font-mono text-[11px] text-muted-foreground uppercase tracking-[0.3em]">
        Name your workspace
      </div>
      <h1 className="mt-2 font-heading text-3xl leading-tight">
        What are we calling it?
      </h1>
      <p className="mt-2 text-muted-foreground text-sm">
        You can change this later in settings.
      </p>

      <form onSubmit={onSubmit} className="mt-8 flex flex-col gap-4">
        <div className="flex flex-col gap-1.5">
          <Label htmlFor="onboarding-workspace">Workspace name</Label>
          <Input
            id="onboarding-workspace"
            placeholder="Acme inc."
            autoComplete="off"
            value={value}
            onChange={(e) => onChange(e.target.value)}
            nativeInput
          />
        </div>
        <Button type="submit" size="lg" disabled={!value.trim()} className="mt-2">
          Continue
        </Button>
      </form>
    </>
  );
}

function InviteStep({
  invitees,
  pending,
  onPendingChange,
  onAdd,
  onRemove,
  onContinue,
  onBack,
}: {
  invitees: string[];
  pending: string;
  onPendingChange: (v: string) => void;
  onAdd: (e: FormEvent) => void;
  onRemove: (email: string) => void;
  onContinue: () => void;
  onBack: () => void;
}) {
  return (
    <>
      <div className="mt-8 font-mono text-[11px] text-muted-foreground uppercase tracking-[0.3em]">
        Bring people with you
      </div>
      <h1 className="mt-2 font-heading text-3xl leading-tight">
        Invite teammates
      </h1>
      <p className="mt-2 text-muted-foreground text-sm">
        Optional — you can add anyone later.
      </p>

      <form onSubmit={onAdd} className="mt-8 flex gap-2">
        <Input
          type="email"
          placeholder="colleague@example.com"
          autoComplete="off"
          value={pending}
          onChange={(e) => onPendingChange(e.target.value)}
          nativeInput
        />
        <Button type="submit" variant="outline">
          Add
        </Button>
      </form>

      {invitees.length > 0 ? (
        <ul className="mt-5 flex flex-col gap-1.5">
          {invitees.map((email) => (
            <li
              key={email}
              className="flex items-center justify-between rounded-md border border-border/70 bg-background/40 px-3 py-2 text-sm"
            >
              <span className="truncate text-foreground/85">{email}</span>
              <button
                type="button"
                onClick={() => onRemove(email)}
                className="font-mono text-[10px] text-muted-foreground uppercase tracking-[0.2em] transition-colors hover:text-foreground"
              >
                Remove
              </button>
            </li>
          ))}
        </ul>
      ) : (
        <div className="mt-5 rounded-md border border-dashed border-border/70 bg-background/30 px-3 py-6 text-center text-muted-foreground text-xs">
          No invites yet. Add a few or skip — totally fine.
        </div>
      )}

      <div className="mt-8 flex items-center justify-between">
        <Button variant="ghost" type="button" onClick={onBack}>
          Back
        </Button>
        <Button type="button" size="lg" onClick={onContinue}>
          {invitees.length === 0
            ? "Skip for now"
            : `Send ${invitees.length} invite${invitees.length === 1 ? "" : "s"}`}
        </Button>
      </div>
    </>
  );
}

function ReadyStep({
  workspace,
  count,
}: {
  workspace: string;
  count: number;
}) {
  return (
    <>
      <div className="mt-8 font-mono text-[11px] text-muted-foreground uppercase tracking-[0.3em]">
        You're set
      </div>
      <h1 className="mt-2 font-heading text-3xl leading-tight">
        Welcome to {workspace}.
      </h1>
      <p className="mt-3 text-muted-foreground text-sm leading-relaxed">
        {count === 0
          ? "Quiet for now — when you're ready, invite people from settings."
          : `We've sent ${count} invite${count === 1 ? "" : "s"}. They'll show up here once accepted.`}
      </p>

      <div className="mt-8 grid grid-cols-3 gap-2">
        <FactCard label="Workspace" value={workspace} />
        <FactCard label="Members" value={String(count + 1)} />
        <FactCard label="Plan" value="Free" />
      </div>

      <Button size="lg" className="mt-8 w-full" type="button">
        Take me in
      </Button>
    </>
  );
}

function FactCard({ label, value }: { label: string; value: string }) {
  return (
    <div className="rounded-md border border-border/70 bg-background/40 px-3 py-3">
      <div className="font-mono text-[10px] text-muted-foreground uppercase tracking-[0.25em]">
        {label}
      </div>
      <div className="mt-1 truncate font-heading text-sm">{value}</div>
    </div>
  );
}
```
---

### Waitlist Showcase <a name="waitlist-showcase"></a>

Waitlist showcase showing email submission forms.

- **Relative Path**: `apps/www/src/pages/showcase/waitlist.tsx`

#### Showcase Page Source Code

```tsx
import { useRef } from "react";
import { ParticleField } from "@orbit/ui/particle-field";
import emptyRoomSrc from "@/assets/figures/empty-room.png";

export function WaitlistShowcasePage() {
  const typingImpulse = useRef(0);
  return (
    <div className="relative h-dvh w-dvw overflow-hidden bg-background">
      <ParticleField
        src={emptyRoomSrc}
        sampleStep={3}
        threshold={38}
        dotSize={0.95}
        renderScale={1}
        align="center"
        typingImpulseRef={typingImpulse}
      />
      <div
        aria-hidden
        className="pointer-events-none absolute inset-0"
        style={{
          background:
            "radial-gradient(1200px 800px at 50% 55%, transparent 40%, color-mix(in srgb, var(--background) 85%, transparent) 95%)",
        }}
      />
      <div
        aria-hidden
        className="pointer-events-none absolute inset-x-0 bottom-0 h-[46%]"
        style={{
          background:
            "linear-gradient(to bottom, transparent 0%, color-mix(in srgb, var(--background) 55%, transparent) 38%, color-mix(in srgb, var(--background) 88%, transparent) 70%, var(--background) 100%)",
        }}
      />
      <div
        aria-hidden
        className="pointer-events-none absolute inset-x-0 bottom-0 h-[32%]"
        style={{
          background:
            "radial-gradient(420px 220px at 50% 78%, color-mix(in srgb, var(--background) 85%, transparent) 0%, transparent 70%)",
        }}
      />
      <div className="absolute inset-x-0 bottom-0 flex flex-col items-center gap-4 px-6 pb-16 text-center">
        <div
          className="pointer-events-none font-mono text-[11px] uppercase tracking-[0.3em] text-foreground/55"
          style={{ textShadow: "0 1px 16px rgba(0,0,0,0.7)" }}
        >
          Invite-only, for now
        </div>
        <h1
          className="pointer-events-none max-w-xl font-heading text-3xl leading-tight md:text-4xl"
          style={{ textShadow: "0 1px 24px rgba(0,0,0,0.65)" }}
        >
          This room's full.
        </h1>
        <p
          className="pointer-events-none max-w-md text-sm leading-relaxed text-foreground/70"
          style={{ textShadow: "0 1px 16px rgba(0,0,0,0.7)" }}
        >
          Join the waitlist and we'll email you when there's space.
        </p>
        <a
          href="#"
          className="mt-2 inline-flex h-10 items-center justify-center rounded-md bg-foreground px-5 font-medium text-background text-sm transition-opacity hover:opacity-90"
        >
          Join the waitlist
        </a>
      </div>
    </div>
  );
}
```
---
