# Loading UI Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **Loading UI** (loading-ui.com) — a set of high-aesthetic loaders, progress rings, bobbing dots, swirling spinners, and text shimmer effects built with Tailwind CSS, Framer Motion, and TypeScript.

## Table of Contents

- [Analyzing image](#analyzing-image)
- [Arc](#arc)
- [Bars](#bars)
- [Bobbing dots](#bobbing-dots)
- [Bouncing dots](#bouncing-dots)
- [Classic](#classic)
- [Clock ring](#clock-ring)
- [Comet spinner](#comet-spinner)
- [Concentric ring](#concentric-ring)
- [Dash ring](#dash-ring)
- [Diamond](#diamond)
- [Dots ring](#dots-ring)
- [Dots](#dots)
- [Dual arc](#dual-arc)
- [Fade arc](#fade-arc)
- [Infinity](#infinity)
- [Morphing infinity](#morphing-infinity)
- [Orbit ring](#orbit-ring)
- [Pulsating dots](#pulsating-dots)
- [Pulse dot](#pulse-dot)
- [Pulse](#pulse)
- [Quarter ring](#quarter-ring)
- [Ring](#ring)
- [Ripple](#ripple)
- [Satellite ring](#satellite-ring)
- [Skeleton](#skeleton)
- [Spiral](#spiral)
- [Spokes](#spokes)
- [Swirling](#swirling)
- [Terminal](#terminal)
- [Text blink](#text-blink)
- [Text dots](#text-dots)
- [Text shimmer wave](#text-shimmer-wave)
- [Text shimmer](#text-shimmer)
- [Triple dot spinner](#triple-dot-spinner)
- [Twin orbit](#twin-orbit)
- [Typing](#typing)
- [Wandering eyes](#wandering-eyes)
- [Wave](#wave)

---

### Analyzing image <a name="analyzing-image"></a>

A visual placeholder for image analysis or computer-vision style loading.

- **Registry Key**: `analyzing-image`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/analyzing-image.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

##### Example 1

```tsx
import { AnalyzingImage } from "@/components/loading-ui/analyzing-image";
```

##### Example 2

```tsx
<AnalyzingImage className="size-16" />
```

#### Component Source Code

##### File: `registry/components/loading-ui/analyzing-image.tsx`

```tsx
"use client";

import { motion } from "motion/react";

import { cn } from "@/lib/utils";

import type { Transition } from "motion/react";

const transition: Transition = {
  duration: 2.5,
  ease: [0.175, 0.885, 0.32, 1],
  times: [0, 0.6, 0.6, 1],
  repeat: Infinity,
  repeatType: "mirror",
  repeatDelay: 0.2,
};

function AnalyzingImage({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      role="status"
      aria-label="Analyzing image"
      className={cn("relative isolate shrink-0", className)}
      {...props}
    >
      <motion.div
        initial={{
          clipPath: "inset(0% 0% 0% 0%)",
        }}
        animate={{
          clipPath: [
            "inset(0% 0% 0% 0%)",
            "inset(0% 105% 0% 0%)",
            "inset(0% 105% 0% 0%)",
            "inset(0% 0% 0% 0%)",
          ],
        }}
        transition={transition}
        className="absolute inset-0 z-10 bg-[var(--loading-ui-analyzing-image-background,var(--background))]"
      >
        <svg
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
          className="size-full"
        >
          <path
            d="M4.27209 20.7279L10.8686 14.1314C11.2646 13.7354 11.4627 13.5373 11.691 13.4632C11.8918 13.3979 12.1082 13.3979 12.309 13.4632C12.5373 13.5373 12.7354 13.7354 13.1314 14.1314L19.6839 20.6839M14 15L16.8686 12.1314C17.2646 11.7354 17.4627 11.5373 17.691 11.4632C17.8918 11.3979 18.1082 11.3979 18.309 11.4632C18.5373 11.5373 18.7354 11.7354 19.1314 12.1314L22 15M10 9C10 10.1046 9.10457 11 8 11C6.89543 11 6 10.1046 6 9C6 7.89543 6.89543 7 8 7C9.10457 7 10 7.89543 10 9ZM6.8 21H17.2C18.8802 21 19.7202 21 20.362 20.673C20.9265 20.3854 21.3854 19.9265 21.673 19.362C22 18.7202 22 17.8802 22 16.2V7.8C22 6.11984 22 5.27976 21.673 4.63803C21.3854 4.07354 20.9265 3.6146 20.362 3.32698C19.7202 3 18.8802 3 17.2 3H6.8C5.11984 3 4.27976 3 3.63803 3.32698C3.07354 3.6146 2.6146 4.07354 2.32698 4.63803C2 5.27976 2 6.11984 2 7.8V16.2C2 17.8802 2 18.7202 2.32698 19.362C2.6146 19.9265 3.07354 20.3854 3.63803 20.673C4.27976 21 5.11984 21 6.8 21Z"
            stroke="currentColor"
            strokeWidth="1.5"
            strokeLinecap="round"
            strokeLinejoin="round"
          />
        </svg>
      </motion.div>
      <motion.div
        initial={{ transform: "translateX(1400%)" }}
        animate={{
          transform: [
            "translateX(1400%)",
            "translateX(-80%)",
            "translateX(-80%)",
            "translateX(1400%)",
          ],
        }}
        transition={transition}
        className="absolute z-10 h-full w-[7%] rounded-full bg-current"
      />
      <svg
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        className="absolute inset-0 size-full"
      >
        <path
          d="M6.8 21H17.2C18.8802 21 19.7202 21 20.362 20.673C20.9265 20.3854 21.3854 19.9265 21.673 19.362C22 18.7202 22 17.8802 22 16.2V7.8C22 6.11984 22 5.27976 21.673 4.63803C21.3854 4.07354 20.9265 3.6146 20.362 3.32698C19.7202 3 18.8802 3 17.2 3H6.8C5.11984 3 4.27976 3 3.63803 3.32698C3.07354 3.6146 2.6146 4.07354 2.32698 4.63803C2 5.27976 2 6.11984 2 7.8V16.2C2 17.8802 2 18.7202 2.32698 19.362C2.6146 19.9265 3.07354 20.3854 3.63803 20.673C4.27976 21 5.11984 21 6.8 21Z"
          stroke="currentColor"
          strokeWidth="1.5"
          strokeLinecap="round"
          strokeLinejoin="round"
        />
        <rect x="6" y="19" width="1" height="1" fill="currentColor" />
        <rect x="7" y="18" width="1" height="1" fill="currentColor" />
        <rect x="7" y="19" width="3" height="1" fill="currentColor" />
        <rect x="9" y="18" width="1" height="1" fill="currentColor" />
        <rect x="14" y="19" width="3" height="1" fill="currentColor" />
        <rect x="15" y="18" width="1" height="1" fill="currentColor" />
        <rect x="5" y="18" width="2" height="1" fill="currentColor" />
        <rect x="5" y="17" width="1" height="1" fill="currentColor" />
        <rect x="10" y="19" width="1" height="1" fill="currentColor" />
        <rect x="7" y="17" width="1" height="1" fill="currentColor" />
        <rect x="11" y="19" width="1" height="1" fill="currentColor" />
        <rect x="10" y="18" width="1" height="1" fill="currentColor" />
        <rect x="17" y="19" width="1" height="1" fill="currentColor" />
        <rect x="15" y="4" width="2" height="1" fill="currentColor" />
        <rect x="3" y="9" width="1" height="3" fill="currentColor" />
        <rect x="4" y="10" width="1" height="2" fill="currentColor" />
        <rect x="6" y="9" width="1" height="1" fill="currentColor" />
        <rect x="15" y="5" width="1" height="1" fill="currentColor" />
        <rect x="20" y="8" width="1" height="3" fill="currentColor" />
        <rect x="19" y="9" width="1" height="1" fill="currentColor" />
        <rect x="7" y="13" width="1" height="1" fill="currentColor" />
        <rect x="9" y="11" width="1" height="1" fill="currentColor" />
        <rect x="16" y="12" width="1" height="2" fill="currentColor" />
        <rect x="13" y="14" width="1" height="1" fill="currentColor" />
        <rect x="12" y="11" width="1" height="1" fill="currentColor" />
        <rect x="10" y="9" width="1" height="1" fill="currentColor" />
        <rect x="10" y="15" width="1" height="1" fill="currentColor" />
        <rect x="10" y="13" width="1" height="1" fill="currentColor" />
        <rect x="15" y="9" width="1" height="1" fill="currentColor" />
        <rect x="13" y="10" width="1" height="1" fill="currentColor" />
        <rect x="12" y="14" width="1" height="1" fill="currentColor" />
        <rect x="5" y="4" width="3" height="1" fill="currentColor" />
        <rect x="6" y="5" width="1" height="1" fill="currentColor" />
        <rect x="7" y="14" width="1" height="2" fill="currentColor" />
        <rect x="6" y="14" width="3" height="1" fill="currentColor" />
        <rect x="16" y="8" width="1" height="1" fill="currentColor" />
        <rect x="8" y="9" width="1" height="1" fill="currentColor" />
        <rect x="20" y="16" width="1" height="1" fill="currentColor" />
        <rect x="12" y="12" width="1" height="1" fill="currentColor" />
        <rect x="8" y="8" width="1" height="1" fill="currentColor" />
        <rect x="14" y="12" width="1" height="1" fill="currentColor" />
        <rect x="17" y="16" width="2" height="1" fill="currentColor" />
        <rect x="14" y="17" width="1" height="1" fill="currentColor" />
        <rect x="11" y="5" width="3" height="1" fill="currentColor" />
        <rect x="12" y="4" width="1" height="1" fill="currentColor" />
        <rect x="12" y="7" width="1" height="1" fill="currentColor" />
        <rect x="7" y="11" width="1" height="1" fill="currentColor" />
        <rect x="15" y="15" width="1" height="1" fill="currentColor" />
        <rect x="11" y="11" width="1" height="1" fill="currentColor" />
        <rect x="13" y="9" width="1" height="1" fill="currentColor" />
        <rect x="12" y="15" width="1" height="1" fill="currentColor" />
        <rect x="9" y="12" width="2" height="1" fill="currentColor" />
        <rect x="19" y="13" width="2" height="1" fill="currentColor" />
        <rect x="9" y="6" width="1" height="1" fill="currentColor" />
        <rect x="20" y="4" width="1" height="1" fill="currentColor" />
        <rect x="19" y="4" width="1" height="1" fill="currentColor" />
        <rect x="3" y="15" width="1" height="2" fill="currentColor" />
        <rect x="3" y="19" width="1" height="1" fill="currentColor" />
      </svg>
      <span className="sr-only">Analyzing image</span>
    </div>
  );
}

export { AnalyzingImage };
```

---

### Arc <a name="arc"></a>

A curved indeterminate progress arc for loading and pending UI.

- **Registry Key**: `arc`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/arc.json
```

#### How to Use

##### Example 1

```tsx
import { Arc } from "@/components/loading-ui/arc";
```

##### Example 2

```tsx
<Arc />
```

#### Component Source Code

##### File: `registry/components/loading-ui/arc.tsx`

```tsx
import { cn } from "@/lib/utils";

function Arc({ className, style, ...props }: React.ComponentProps<"div">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-arc-spin {
          to {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <div
        className={cn(
          "rounded-full border-[5px] border-current/10 border-t-current",
          className,
        )}
        style={{
          animationName: "loading-ui-arc-spin",
          animationDuration: "var(--duration, 1s)",
          animationTimingFunction: "linear",
          animationIterationCount: "infinite",
          ...style,
        }}
        {...props}
      />
    </>
  );
}

export { Arc };
```

---

### Bars <a name="bars"></a>

A bar-style activity indicator for lists, players, and dense layouts.

- **Registry Key**: `bars`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/bars.json
```

#### How to Use

##### Example 1

```tsx
import { Bars } from "@/components/loading-ui/bars";
```

##### Example 2

```tsx
<Bars className="h-12 w-16" />
```

#### Component Source Code

##### File: `registry/components/loading-ui/bars.tsx`

```tsx
import { cn } from "@/lib/utils";

function Bars({
  className,
  bars = 3,
  ...props
}: React.ComponentProps<"span"> & { bars?: number }) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-wave-bars {
          0%,
          100% {
            transform: scaleY(1);
            opacity: 0.5;
          }

          50% {
            transform: scaleY(0.6);
            opacity: 1;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("inline-flex items-stretch gap-[5%]", className)}
        {...props}
      >
        {Array.from({ length: bars }, (_, index) => (
          <span
            key={index}
            aria-hidden="true"
            className="inline-block h-full rounded-[1px] bg-current"
            style={{
              width: `${100 / bars}%`,
              animation:
                "loading-ui-wave-bars var(--duration, 1.2s) ease-in-out infinite",
              animationDelay: `calc(var(--delay, 0.2s) * ${index})`,
            }}
          />
        ))}
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { Bars };
```

---

### Bobbing dots <a name="bobbing-dots"></a>

Motion-driven dots that bounce in phase—playful loading for creative tools, media, and lighthearted flows.

- **Registry Key**: `bobbing-dots`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/bobbing-dots.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

##### Example 1

```tsx
import { BobbingDots } from "@/components/loading-ui/bobbing-dots";
```

##### Example 2

```tsx
<BobbingDots />
```

#### Component Source Code

##### File: `registry/components/loading-ui/bobbing-dots.tsx`

```tsx
"use client";

import { motion } from "motion/react";
import { cn } from "@/lib/utils";

function BobbingDots({
  className,
  dots = 3,
  duration = 1,
  ...props
}: React.ComponentProps<"span"> & { dots?: number; duration?: number }) {
  const transition = (index: number) => ({
    duration,
    repeat: Infinity,
    repeatType: "loop" as const,
    delay: index * 0.2,
    ease: "easeInOut" as const,
  });

  return (
    <span
      role="status"
      className={cn("inline-flex items-center gap-[12%]", className)}
      {...props}
    >
      {Array.from({ length: dots }, (_, index) => (
        <motion.span
          key={index}
          aria-hidden="true"
          initial={{ y: 0 }}
          animate={{ y: [0, "0.625em", 0] }}
          transition={transition(index)}
          className="inline-block aspect-square grow rounded-full bg-current shadow-sm"
        />
      ))}
      <span className="sr-only">Loading</span>
    </span>
  );
}

export { BobbingDots };
```

---

### Bouncing dots <a name="bouncing-dots"></a>

Elastic scale-and-opacity dots—bouncy inline feedback for queues, ingests, and playful waits.

- **Registry Key**: `bouncing-dots`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/bouncing-dots.json
```

#### How to Use

##### Example 1

```tsx
import { BouncingDots } from "@/components/loading-ui/bouncing-dots";
```

##### Example 2

```tsx
<BouncingDots />
```

#### Component Source Code

##### File: `registry/components/loading-ui/bouncing-dots.tsx`

```tsx
import { cn } from "@/lib/utils";

function BouncingDots({
  className,
  dots = 3,
  ...props
}: React.ComponentProps<"span"> & { dots?: number }) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-bouncing-dots {
          0%,
          100% {
            transform: scale(0.8);
            opacity: 0.5;
          }

          50% {
            transform: scale(1.2);
            opacity: 1;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("inline-flex items-center gap-[12%]", className)}
        {...props}
      >
        {Array.from({ length: dots }, (_, index) => (
          <span
            key={index}
            aria-hidden="true"
            className="inline-block aspect-square grow rounded-full bg-current"
            style={{
              animation:
                "loading-ui-bouncing-dots var(--duration, 1.4s) ease-in-out infinite",
              animationDelay: `calc(var(--delay, 0.2s) * ${index})`,
            }}
          />
        ))}
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { BouncingDots };
```

---

### Classic <a name="classic"></a>

A familiar spinner style that fits most apps and form contexts.

- **Registry Key**: `classic`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/classic.json
```

#### How to Use

##### Example 1

```tsx
import { Classic } from "@/components/loading-ui/classic";
```

##### Example 2

```tsx
<Classic />
```

#### Component Source Code

##### File: `registry/components/loading-ui/classic.tsx`

```tsx
import { cn } from "@/lib/utils";

type ClassicProps = Omit<React.ComponentProps<"span">, "children">;

function Classic({ className, ...props }: ClassicProps) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-classic-fade {
          0% {
            opacity: 1;
          }

          100% {
            opacity: 0.15;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("box-border inline-block size-5", className)}
        {...props}
      >
        <span
          aria-hidden="true"
          className="relative top-1/2 left-1/2 block size-full"
        >
          {Array.from({ length: 12 }, (_, index) => (
            <span
              key={index}
              className="absolute top-[-3.9%] left-[-10%] block h-[8%] w-[24%] rounded-(--radius) bg-current"
              style={{
                transform: `rotate(${index * 30}deg) translate(146%)`,
                animation:
                  "loading-ui-classic-fade var(--duration, 1.2s) linear infinite",
                animationDelay: `calc(var(--duration, 1.2s) / 12 * ${index - 12})`,
              }}
            />
          ))}
        </span>
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { Classic };
```

---

### Clock ring <a name="clock-ring"></a>

A clock-hand style ring for scheduling, sync, and time-bound wait states.

- **Registry Key**: `clock-ring`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/clock-ring.json
```

#### How to Use

##### Example 1

```tsx
import { ClockRing } from "@/components/loading-ui/clock-ring";
```

##### Example 2

```tsx
<ClockRing />
```

#### Component Source Code

##### File: `registry/components/loading-ui/clock-ring.tsx`

```tsx
import { cn } from "@/lib/utils";

function ClockRing({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-clock-ring-rotation {
          0% {
            transform: rotate(0deg);
          }

          100% {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "relative inline-block rounded-full border-2 border-current",
          className,
        )}
        style={{
          animation:
            "loading-ui-clock-ring-rotation var(--duration, 1.5s) linear infinite",
        }}
        {...props}
      >
        <span
          aria-hidden="true"
          className="absolute top-0 left-1/2 bg-current"
          style={{
            width: "6.25%",
            height: "50%",
            transform: "translateX(-50%)",
          }}
        />
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { ClockRing };
```

---

### Comet spinner <a name="comet-spinner"></a>

A rotating comet with a bright head and sweeping tail—great for energetic throughput states.

- **Registry Key**: `comet-spinner`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/comet-spinner.json
```

#### How to Use

##### Example 1

```tsx
import { CometSpinner } from "@/components/loading-ui/comet-spinner";
```

##### Example 2

```tsx
<CometSpinner />
```

#### Component Source Code

##### File: `registry/components/loading-ui/comet-spinner.tsx`

```tsx
import { cn } from "@/lib/utils";

const SHADOW_ANIMATION = "loading-ui-comet-shadow";
const ROTATION_ANIMATION = "loading-ui-comet-rotation";

type CometSpinnerProps = React.ComponentProps<"span"> & {
  headScale?: number;
  radiusScale?: number;
};

function clamp(value: number, min: number, max: number) {
  return Math.min(Math.max(value, min), max);
}

function CometSpinner({
  className,
  style,
  headScale = 0.2,
  radiusScale = 0.83,
  ...props
}: CometSpinnerProps) {
  const safeHeadScale = clamp(headScale, 0.08, 0.35);
  const safeRadiusScale = clamp(radiusScale, 0.3, 1.1);
  const cometStyle = {
    ...style,
    "--loading-ui-comet-head": `${(safeHeadScale * 100).toFixed(2)}cqmin`,
    "--loading-ui-comet-radius": `${(safeRadiusScale * 100).toFixed(2)}cqmin`,
  } as React.CSSProperties;

  return (
    <>
      <style>{`
        @keyframes ${SHADOW_ANIMATION} {
          0% {
            box-shadow:
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2),
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2.1),
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2.2),
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2.3),
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2.385);
          }

          5%,
          95% {
            box-shadow:
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2),
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2.1),
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2.2),
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2.3),
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2.385);
          }

          10%,
          59% {
            box-shadow:
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2),
              calc(var(--loading-ui-comet-radius) * -0.105) calc(var(--loading-ui-comet-radius) * -0.994) 0 calc(var(--loading-ui-comet-head) * -2.1),
              calc(var(--loading-ui-comet-radius) * -0.208) calc(var(--loading-ui-comet-radius) * -0.978) 0 calc(var(--loading-ui-comet-head) * -2.2),
              calc(var(--loading-ui-comet-radius) * -0.308) calc(var(--loading-ui-comet-radius) * -0.95) 0 calc(var(--loading-ui-comet-head) * -2.3),
              calc(var(--loading-ui-comet-radius) * -0.358) calc(var(--loading-ui-comet-radius) * -0.934) 0 calc(var(--loading-ui-comet-head) * -2.385);
          }

          20% {
            box-shadow:
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2),
              calc(var(--loading-ui-comet-radius) * -0.407) calc(var(--loading-ui-comet-radius) * -0.913) 0 calc(var(--loading-ui-comet-head) * -2.1),
              calc(var(--loading-ui-comet-radius) * -0.669) calc(var(--loading-ui-comet-radius) * -0.743) 0 calc(var(--loading-ui-comet-head) * -2.2),
              calc(var(--loading-ui-comet-radius) * -0.808) calc(var(--loading-ui-comet-radius) * -0.588) 0 calc(var(--loading-ui-comet-head) * -2.3),
              calc(var(--loading-ui-comet-radius) * -0.902) calc(var(--loading-ui-comet-radius) * -0.41) 0 calc(var(--loading-ui-comet-head) * -2.385);
          }

          38% {
            box-shadow:
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2),
              calc(var(--loading-ui-comet-radius) * -0.454) calc(var(--loading-ui-comet-radius) * -0.892) 0 calc(var(--loading-ui-comet-head) * -2.1),
              calc(var(--loading-ui-comet-radius) * -0.777) calc(var(--loading-ui-comet-radius) * -0.629) 0 calc(var(--loading-ui-comet-head) * -2.2),
              calc(var(--loading-ui-comet-radius) * -0.934) calc(var(--loading-ui-comet-radius) * -0.358) 0 calc(var(--loading-ui-comet-head) * -2.3),
              calc(var(--loading-ui-comet-radius) * -0.988) calc(var(--loading-ui-comet-radius) * -0.108) 0 calc(var(--loading-ui-comet-head) * -2.385);
          }

          100% {
            box-shadow:
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2),
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2.1),
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2.2),
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2.3),
              0 calc(var(--loading-ui-comet-radius) * -1) 0 calc(var(--loading-ui-comet-head) * -2.385);
          }
        }

        @keyframes ${ROTATION_ANIMATION} {
          0% {
            transform: rotate(0deg);
          }

          100% {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <span
        role="status"
        aria-label="Loading"
        className={cn(
          "@container-[size] relative inline-flex aspect-square items-center justify-center align-middle",
          className,
        )}
        style={cometStyle}
        {...props}
      >
        <span
          aria-hidden="true"
          className="absolute inset-0 rounded-full"
          style={{
            animation: `${SHADOW_ANIMATION} var(--duration, 1.7s) infinite var(--easing, ease), ${ROTATION_ANIMATION} var(--duration, 1.7s) infinite var(--easing, ease)`,
            transform: "translateZ(0)",
          }}
        />
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { CometSpinner };
```

---

### Concentric ring <a name="concentric-ring"></a>

Layered rings for a deep, technical loading look.

- **Registry Key**: `concentric-ring`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/concentric-ring.json
```

#### How to Use

##### Example 1

```tsx
import { ConcentricRing } from "@/components/loading-ui/concentric-ring";
```

##### Example 2

```tsx
<ConcentricRing />
```

#### Component Source Code

##### File: `registry/components/loading-ui/concentric-ring.tsx`

```tsx
import { cn } from "@/lib/utils";

function ConcentricRing({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-concentric-ring-rotation {
          0% {
            transform: rotate(0deg);
          }

          100% {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("relative inline-block", className)}
        style={{
          animation:
            "loading-ui-concentric-ring-rotation var(--duration, 1s) linear infinite",
        }}
        {...props}
      >
        <span
          aria-hidden="true"
          className="absolute inset-0 rounded-full border-2 border-current"
          style={{ opacity: 0.25 }}
        />
        <span
          aria-hidden="true"
          className="absolute top-1/2 left-1/2 rounded-full border-2 border-transparent border-b-current"
          style={{
            width: "83.333%",
            height: "83.333%",
            transform: "translate(-50%, -50%)",
          }}
        />
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { ConcentricRing };
```

---

### Dash ring <a name="dash-ring"></a>

An SVG dash-stroke ring with morphing length—strong for streaming and indeterminate progress.

- **Registry Key**: `dash-ring`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/dash-ring.json
```

#### How to Use

##### Example 1

```tsx
import { DashRing } from "@/components/loading-ui/dash-ring";
```

##### Example 2

```tsx
<DashRing />
```

#### Component Source Code

##### File: `registry/components/loading-ui/dash-ring.tsx`

```tsx
import { cn } from "@/lib/utils";

type DashRingProps = React.ComponentProps<"svg">;

function DashRing({ className, ...props }: DashRingProps) {
  return (
    <svg
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      xmlns="http://www.w3.org/2000/svg"
      role="status"
      className={cn(className)}
      {...props}
    >
      <circle
        cx="12"
        cy="12"
        r="9.5"
        opacity="0.1"
        strokeWidth="2"
        strokeLinecap="round"
      />
      <circle cx="12" cy="12" r="9.5" strokeWidth="2" strokeLinecap="round">
        <animateTransform
          attributeName="transform"
          type="rotate"
          from="0 12 12"
          to="360 12 12"
          dur="2s"
          repeatCount="indefinite"
        />
        <animate
          attributeName="stroke-dasharray"
          values="0 150;42 150;42 150"
          keyTimes="0;0.5;1"
          dur="1.5s"
          repeatCount="indefinite"
        />
        <animate
          attributeName="stroke-dashoffset"
          values="0;-16;-59"
          keyTimes="0;0.5;1"
          dur="1.5s"
          repeatCount="indefinite"
        />
      </circle>
    </svg>
  );
}

export { DashRing };
```

---

### Diamond <a name="diamond"></a>

An 8-bit diamond loader with pixel-stepped opacity for retro-flavored waiting states.

- **Registry Key**: `diamond`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/diamond.json
```

#### How to Use

##### Example 1

```tsx
import { Diamond } from "@/components/loading-ui/diamond";
```

##### Example 2

```tsx
<Diamond />
```

#### Component Source Code

##### File: `registry/components/loading-ui/diamond.tsx`

```tsx
import * as React from "react";
import { cn } from "@/lib/utils";

const Diamond = React.forwardRef<
  SVGSVGElement,
  React.ComponentProps<"svg">
>(({ className, ...props }, ref) => {
  return (
    <svg
      ref={ref}
      viewBox="0 0 20 20"
      fill="currentColor"
      className={cn("size-4", className)}
      role="status"
      aria-label="Loading"
      {...props}
    >
      <style>
        {`
            @keyframes spin-pixel {
              0% { opacity: 0; }
              1% { opacity: 1; }
              100% { opacity: 0; }
            }
            .pixel-1 { animation: spin-pixel 0.8s ease-in-out 0s infinite; }
            .pixel-2 { animation: spin-pixel 0.8s ease-in-out 0.1s infinite; }
            .pixel-3 { animation: spin-pixel 0.8s ease-in-out 0.2s infinite; }
            .pixel-4 { animation: spin-pixel 0.8s ease-in-out 0.3s infinite; }
            .pixel-5 { animation: spin-pixel 0.8s ease-in-out 0.4s infinite; }
            .pixel-6 { animation: spin-pixel 0.8s ease-in-out 0.5s infinite; }
            .pixel-7 { animation: spin-pixel 0.8s ease-in-out 0.6s infinite; }
            .pixel-8 { animation: spin-pixel 0.8s ease-in-out 0.7s infinite; }
          `}
      </style>
      {/* Top */}
      <rect className="pixel-1" x="8" y="0" width="4" height="4" />
      {/* Top Right */}
      <rect className="pixel-2" x="12" y="4" width="4" height="4" />
      {/* Right */}
      <rect className="pixel-3" x="16" y="8" width="4" height="4" />
      {/* Bottom Right */}
      <rect className="pixel-4" x="12" y="12" width="4" height="4" />
      {/* Bottom */}
      <rect className="pixel-5" x="8" y="16" width="4" height="4" />
      {/* Bottom Left */}
      <rect className="pixel-6" x="4" y="12" width="4" height="4" />
      {/* Left */}
      <rect className="pixel-7" x="0" y="8" width="4" height="4" />
      {/* Top Left */}
      <rect className="pixel-8" x="4" y="4" width="4" height="4" />
    </svg>
  );
});

Diamond.displayName = "Diamond";

export { Diamond };
```

---

### Dots ring <a name="dots-ring"></a>

A ring of animated dots that scales with the parent container.

- **Registry Key**: `dots-ring`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/dots-ring.json
```

#### How to Use

##### Example 1

```tsx
import { DotsRing } from "@/components/loading-ui/dots-ring";
```

##### Example 2

```tsx
<DotsRing />
```

#### Component Source Code

##### File: `registry/components/loading-ui/dots-ring.tsx`

```tsx
import { cn } from "@/lib/utils";

type DotsRingProps = React.ComponentProps<"span"> & {
  dots?: number;
  dotScale?: number;
  radiusScale?: number;
};

function clamp(value: number, min: number, max: number) {
  return Math.min(Math.max(value, min), max);
}

function DotsRing({
  className,
  style,
  dots = 8,
  dotScale = 0.16,
  radiusScale = 0.34,
  ...props
}: DotsRingProps) {
  const dotCount = Math.max(4, Math.floor(dots));
  const safeDotScale = clamp(dotScale, 0.2, 0.4);
  const safeRadiusScale = clamp(radiusScale, 0, 0.5 - safeDotScale / 2);

  return (
    <>
      <style>{`
        @keyframes loading-ui-dots-ring-pulse {
          0%,
          100% {
            opacity: 0.25;
            transform: scale(0.65);
          }

          12.5% {
            opacity: 1;
            transform: scale(1);
          }

          25% {
            opacity: 0.75;
            transform: scale(0.85);
          }

          50% {
            opacity: 0.35;
            transform: scale(0.7);
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "@container-[size] relative inline-flex aspect-square items-center justify-center",
          className,
        )}
        style={style}
        {...props}
      >
        <span aria-hidden="true" className="relative block size-full">
          {Array.from({ length: dotCount }, (_, index) => {
            const angle = (index / dotCount) * Math.PI * 2;
            const x = `${(Math.sin(angle) * safeRadiusScale * 100).toFixed(2)}cqmin`;
            const y = `${(-Math.cos(angle) * safeRadiusScale * 100).toFixed(2)}cqmin`;

            return (
              <span
                key={index}
                className="absolute top-1/2 left-1/2"
                style={{
                  width: `calc(${safeDotScale} * 100cqmin)`,
                  height: `calc(${safeDotScale} * 100cqmin)`,
                  transform: `translate(-50%, -50%) translate(${x}, ${y})`,
                }}
              >
                <span
                  className="block size-full rounded-full bg-current"
                  style={{
                    animation:
                      "loading-ui-dots-ring-pulse var(--duration, 1s) linear infinite",
                    animationDelay: `calc(var(--duration, 1s) / ${dotCount} * ${index - dotCount})`,
                  }}
                />
              </span>
            );
          })}
        </span>
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { DotsRing };
```

---

### Dots <a name="dots"></a>

A classic blinking-dot row—perfect beside copy, in composers, and anywhere “typing” should read instantly.

- **Registry Key**: `dots`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/dots.json
```

#### How to Use

##### Example 1

```tsx
import { Dots } from "@/components/loading-ui/dots";
```

##### Example 2

```tsx
<Dots />
```

#### Component Source Code

##### File: `registry/components/loading-ui/dots.tsx`

```tsx
import { cn } from "@/lib/utils";

function Dots({
  className,
  dots = 3,
  ...props
}: React.ComponentProps<"span"> & { dots?: number }) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-dots-blink {
          0%,
          100% {
            opacity: 0.2;
          }

          20% {
            opacity: 1;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "inline-flex items-center justify-center gap-[12%]",
          className,
        )}
        {...props}
      >
        {Array.from({ length: dots }, (_, index) => (
          <span
            key={index}
            data-slot="dot"
            aria-hidden="true"
            style={{
              animation:
                "loading-ui-dots-blink var(--duration, 1.4s) infinite both",
              animationDelay: `calc(var(--delay, 0.2s) * ${index})`,
            }}
            className="aspect-square grow rounded-full bg-current"
          />
        ))}
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { Dots };
```

---

### Dual arc <a name="dual-arc"></a>

Two complementary arcs for a balanced indeterminate progress feel.

- **Registry Key**: `dual-arc`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/dual-arc.json
```

#### How to Use

##### Example 1

```tsx
import { DualArc } from "@/components/loading-ui/dual-arc";
```

##### Example 2

```tsx
<DualArc />
```

#### Component Source Code

##### File: `registry/components/loading-ui/dual-arc.tsx`

```tsx
import { cn } from "@/lib/utils";

function DualArc({ className, style, ...props }: React.ComponentProps<"div">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-dual-arc-spin {
          to {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <div
        className={cn(
          "rounded-full border-[5px] border-transparent border-y-current",
          className,
        )}
        style={{
          animationName: "loading-ui-dual-arc-spin",
          animationDuration: "var(--duration, 1s)",
          animationTimingFunction: "linear",
          animationIterationCount: "infinite",
          ...style,
        }}
        {...props}
      />
    </>
  );
}

export { DualArc };
```

---

### Fade arc <a name="fade-arc"></a>

A gradient arc spinner with a soft trailing fade for polished loading states.

- **Registry Key**: `fade-arc`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/fade-arc.json
```

#### How to Use

##### Example 1

```tsx
import { FadeArc } from "@/components/loading-ui/fade-arc";
```

##### Example 2

```tsx
<FadeArc />
```

#### Component Source Code

##### File: `registry/components/loading-ui/fade-arc.tsx`

```tsx
import { useId } from "react";

import { cn } from "@/lib/utils";

function FadeArc({ className, style, ...props }: React.ComponentProps<"svg">) {
  const baseId = useId().replace(/:/g, "");
  const leadingGradientId = `${baseId}-leading`;
  const trailingGradientId = `${baseId}-trailing`;

  return (
    <>
      <style>{`
        @keyframes loading-ui-fade-arc-spin {
          to {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <svg
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        role="status"
        className={cn(className)}
        style={{
          animationName: "loading-ui-fade-arc-spin",
          animationDuration: "var(--duration, 1s)",
          animationTimingFunction: "linear",
          animationIterationCount: "infinite",
          ...style,
        }}
        {...props}
      >
        <defs>
          <linearGradient
            id={leadingGradientId}
            x1="50%"
            x2="50%"
            y1="5.271%"
            y2="91.793%"
          >
            <stop offset="0%" stopColor="currentColor" />
            <stop offset="100%" stopColor="currentColor" stopOpacity="0.55" />
          </linearGradient>
          <linearGradient
            id={trailingGradientId}
            x1="50%"
            x2="50%"
            y1="15.24%"
            y2="87.15%"
          >
            <stop offset="0%" stopColor="currentColor" stopOpacity="0" />
            <stop offset="100%" stopColor="currentColor" stopOpacity="0.55" />
          </linearGradient>
        </defs>
        <g fill="none">
          <path
            d="M8.749.021a1.5 1.5 0 0 1 .497 2.958A7.5 7.5 0 0 0 3 10.375a7.5 7.5 0 0 0 7.5 7.5v3c-5.799 0-10.5-4.7-10.5-10.5C0 5.23 3.726.865 8.749.021"
            fill={`url(#${leadingGradientId})`}
            transform="translate(1.5 1.625)"
          />
          <path
            d="M15.392 2.673a1.5 1.5 0 0 1 2.119-.115A10.48 10.48 0 0 1 21 10.375c0 5.8-4.701 10.5-10.5 10.5v-3a7.5 7.5 0 0 0 5.007-13.084a1.5 1.5 0 0 1-.115-2.118"
            fill={`url(#${trailingGradientId})`}
            transform="translate(1.5 1.625)"
          />
        </g>
      </svg>
    </>
  );
}

export { FadeArc };
```

---

### Infinity <a name="infinity"></a>

An infinity-path animation for long-running or continuous loading.

- **Registry Key**: `infinity`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/infinity.json
```

#### How to Use

##### Example 1

```tsx
import { InfinityLoop } from "@/components/loading-ui/infinity";
```

##### Example 2

```tsx
<InfinityLoop className="h-12 w-16" />
```

#### Component Source Code

##### File: `registry/components/loading-ui/infinity.tsx`

```tsx
import { cn } from "@/lib/utils";

function InfinityLoop({ className, ...props }: React.ComponentProps<"svg">) {
  return (
    <svg
      viewBox="0 0 100 100"
      preserveAspectRatio="xMidYMid"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
      className={cn(className)}
      {...props}
    >
      <title>Loading...</title>
      <style>{`
        @keyframes loading-ui-infinity-dash {
          to {
            stroke-dashoffset: 256.58892822265625;
          }
        }
      `}</style>
      <path
        d="M24.3 30C11.4 30 5 43.3 5 50s6.4 20 19.3 20c19.3 0 32.1-40 51.4-40C88.6 30 95 43.3 95 50s-6.4 20-19.3 20C56.4 70 43.6 30 24.3 30z"
        stroke="currentColor"
        strokeWidth="10"
        strokeLinecap="round"
        strokeDasharray="205.271142578125 51.317785644531256"
        style={{
          transform: "scale(0.8)",
          transformOrigin: "50px 50px",
          animationName: "loading-ui-infinity-dash",
          animationDuration: "var(--duration, 2s)",
          animationTimingFunction: "linear",
          animationIterationCount: "infinite",
        }}
      />
    </svg>
  );
}

export { InfinityLoop };
```

---

### Morphing infinity <a name="morphing-infinity"></a>

A circle-to-infinity morph with shimmering rotating copy.

- **Registry Key**: `morphing-infinity`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/morphing-infinity.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

##### Example 1

```tsx
import { MorphingInfinity } from "@/components/loading-ui/morphing-infinity";
```

##### Example 2

```tsx
<MorphingInfinity />
```

#### Component Source Code

##### File: `registry/components/loading-ui/morphing-infinity.tsx`

```tsx
"use client";

import { forwardRef, type ComponentProps } from "react";
import { motion } from "motion/react";

import { cn } from "@/lib/utils";

const circleA =
  "M 12 8 C 14.21 8 16 9.79 16 12 C 16 14.21 14.21 16 12 16 C 9.79 16 8 14.21 8 12 C 8 9.79 9.79 8 12 8 Z";

const infinity =
  "M 12 12 C 14 8.5 19 8.5 19 12 C 19 15.5 14 15.5 12 12 C 10 8.5 5 8.5 5 12 C 5 15.5 10 15.5 12 12 Z";

const circleB =
  "M 12 16 C 14.21 16 16 14.21 16 12 C 16 9.79 14.21 8 12 8 C 9.79 8 8 9.79 8 12 C 8 14.21 9.79 16 12 16 Z";

const MorphingInfinity = forwardRef<SVGSVGElement, ComponentProps<"svg">>(
  (props, ref) => {
    return (
      <svg
        ref={ref}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={1.5}
        strokeLinecap="round"
        strokeLinejoin="round"
        role="status"
        aria-label="Loading"
        {...props}
      >
        <motion.path
          animate={{
            d: [circleA, infinity, circleB, infinity, circleA],
          }}
          transition={{
            d: {
              duration: 6,
              ease: "easeInOut",
              repeat: Infinity,
              times: [0, 0.25, 0.5, 0.75, 1.0],
            },
          }}
        />
      </svg>
    );
  },
);

MorphingInfinity.displayName = "MorphingInfinity";

export { MorphingInfinity };
```

---

### Orbit ring <a name="orbit-ring"></a>

A ring with orbiting elements for a dynamic loading indicator.

- **Registry Key**: `orbit-ring`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/orbit-ring.json
```

#### How to Use

##### Example 1

```tsx
import { OrbitRing } from "@/components/loading-ui/orbit-ring";
```

##### Example 2

```tsx
<OrbitRing />
```

#### Component Source Code

##### File: `registry/components/loading-ui/orbit-ring.tsx`

```tsx
import { cn } from "@/lib/utils";

function OrbitRing({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-orbit-ring-rotation {
          0% {
            transform: rotate(0deg);
          }

          100% {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("relative inline-block", className)}
        style={{
          animation:
            "loading-ui-orbit-ring-rotation var(--duration, 1s) linear infinite",
        }}
        {...props}
      >
        <span
          aria-hidden="true"
          className="absolute inset-0 rounded-full border-2 border-current"
          style={{ opacity: 0.25 }}
        />
        <span
          aria-hidden="true"
          className="absolute top-1/2 left-1/2 rounded-full border-2 border-transparent border-b-current"
          style={{
            width: "116.667%",
            height: "116.667%",
            transform: "translate(-50%, -50%)",
          }}
        />
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { OrbitRing };
```

---

### Pulsating dots <a name="pulsating-dots"></a>

Dots with scale and opacity motion for a lively wait state.

- **Registry Key**: `pulsating-dots`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/pulsating-dots.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

##### Example 1

```tsx
import { PulsatingDots } from "@/components/loading-ui/pulsating-dots";
```

##### Example 2

```tsx
<PulsatingDots className="w-16" />
```

#### Component Source Code

##### File: `registry/components/loading-ui/pulsating-dots.tsx`

```tsx
"use client";

import { motion } from "motion/react";
import { cn } from "@/lib/utils";

function PulsatingDots({
  className,
  dots = 3,
  duration = 1,
  ...props
}: React.ComponentProps<"span"> & { dots?: number; duration?: number }) {
  const dotCount = Number.isFinite(dots) ? Math.max(1, Math.floor(dots)) : 3;

  return (
    <span
      role="status"
      className={cn("inline-flex items-center justify-center", className)}
      {...props}
    >
      <span aria-hidden="true" className="inline-flex w-full gap-[16%]">
        {Array.from({ length: dotCount }, (_, index) => (
          <motion.span
            key={index}
            className="inline-block aspect-square grow rounded-full bg-current"
            animate={{
              scale: [1, 1.5, 1],
              opacity: [0.5, 1, 0.5],
            }}
            transition={{
              duration,
              ease: "easeInOut",
              repeat: Infinity,
              delay: index * 0.3,
            }}
          />
        ))}
      </span>
      <span className="sr-only">Loading</span>
    </span>
  );
}

export { PulsatingDots };
```

---

### Pulse dot <a name="pulse-dot"></a>

A single pulsing dot for chat and assistant UIs—the same minimal cue ChatGPT shows while a reply is generating.

- **Registry Key**: `pulse-dot`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/pulse-dot.json
```

#### How to Use

##### Example 1

```tsx
import { PulseDot } from "@/components/loading-ui/pulse-dot";
```

##### Example 2

```tsx
<PulseDot />
```

#### Component Source Code

##### File: `registry/components/loading-ui/pulse-dot.tsx`

```tsx
import { cn } from "@/lib/utils";

function PulseDot({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-pulse-dot {
          0%,
          100% {
            transform: scale(1);
            opacity: 0.8;
          }

          50% {
            transform: scale(1.5);
            opacity: 1;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("inline-block rounded-full bg-current", className)}
        style={{
          animation:
            "loading-ui-pulse-dot var(--duration, 1.2s) ease-in-out infinite",
        }}
        {...props}
      >
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { PulseDot };
```

---

### Pulse <a name="pulse"></a>

A soft scaling ring for calm, breathable loading and standby states.

- **Registry Key**: `pulse`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/pulse.json
```

#### How to Use

##### Example 1

```tsx
import { Pulse } from "@/components/loading-ui/pulse";
```

##### Example 2

```tsx
<Pulse />
```

#### Component Source Code

##### File: `registry/components/loading-ui/pulse.tsx`

```tsx
import { cn } from "@/lib/utils";

function Pulse({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-thin-pulse {
          0%,
          100% {
            transform: scale(0.95);
            opacity: 0.8;
          }

          50% {
            transform: scale(1.05);
            opacity: 0.4;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("relative inline-block shrink-0", className)}
        {...props}
      >
        <span
          aria-hidden="true"
          className="absolute inset-0 rounded-full border-2 border-current"
          style={{
            animation:
              "loading-ui-thin-pulse var(--duration, 1.5s) ease-in-out infinite",
          }}
        />
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { Pulse };
```

---

### Quarter ring <a name="quarter-ring"></a>

A quarter-arc ring for compact spinners when space is tight but motion still matters.

- **Registry Key**: `quarter-ring`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/quarter-ring.json
```

#### How to Use

##### Example 1

```tsx
import { QuarterRing } from "@/components/loading-ui/quarter-ring";
```

##### Example 2

```tsx
<QuarterRing />
```

#### Component Source Code

##### File: `registry/components/loading-ui/quarter-ring.tsx`

```tsx
import { cn } from "@/lib/utils";

function QuarterRing({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-quarter-ring-rotation {
          0% {
            transform: rotate(0deg);
          }

          100% {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "inline-block rounded-full border-t-[3px] border-r-[3px] border-t-current border-r-transparent",
          className,
        )}
        style={{
          animation:
            "loading-ui-quarter-ring-rotation var(--duration, 1s) linear infinite",
        }}
        {...props}
      >
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { QuarterRing };
```

---

### Ring <a name="ring"></a>

A circular indeterminate loader for inline actions, page transitions, and general pending states.

- **Registry Key**: `ring`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/ring.json
```

#### How to Use

##### Example 1

```tsx
import { Ring } from "@/components/loading-ui/ring";
```

##### Example 2

```tsx
<Ring />
```

#### Component Source Code

##### File: `registry/components/loading-ui/ring.tsx`

```tsx
import { cn } from "@/lib/utils";

function Ring({ className, style, ...props }: React.ComponentProps<"svg">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-ring-spin {
          to {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <svg
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        className={cn(className)}
        style={{
          animationName: "loading-ui-ring-spin",
          animationDuration: "var(--duration, 1s)",
          animationTimingFunction: "linear",
          animationIterationCount: "infinite",
          ...style,
        }}
        {...props}
      >
        <path
          d="M21 12.0004C20.9999 13.901 20.3981 15.7528 19.2809 17.2904C18.1637 18.8279 16.5885 19.9723 14.7809 20.5596C12.9733 21.1469 11.0262 21.1468 9.21864 20.5594C7.41109 19.9721 5.83588 18.8276 4.71876 17.29C3.60165 15.7523 2.99999 13.9005 3 11.9999C3.00001 10.0993 3.60171 8.24755 4.71884 6.70994C5.83598 5.17233 7.4112 4.02785 9.21877 3.44052C11.0263 2.85319 12.9734 2.85316 14.781 3.44044"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
        />
      </svg>
    </>
  );
}

export { Ring };
```

---

### Ripple <a name="ripple"></a>

Concentric expanding ripples for radar-like, soft indeterminate progress.

- **Registry Key**: `ripple`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/ripple.json
```

#### How to Use

##### Example 1

```tsx
import { Ripple } from "@/components/loading-ui/ripple";
```

##### Example 2

```tsx
<Ripple />
```

#### Component Source Code

##### File: `registry/components/loading-ui/ripple.tsx`

```tsx
import { cn } from "@/lib/utils";

function Ripple({ className, ...props }: React.ComponentProps<"svg">) {
  return (
    <svg
      viewBox="0 0 44 44"
      fill="none"
      stroke="currentColor"
      xmlns="http://www.w3.org/2000/svg"
      className={cn(className)}
      {...props}
    >
      <title>Loading...</title>
      <g fill="none" fillRule="evenodd" strokeWidth="2">
        <circle cx="22" cy="22" r="1">
          <animate
            attributeName="r"
            begin="0s"
            calcMode="spline"
            dur="1.8s"
            keySplines="0.165, 0.84, 0.44, 1"
            keyTimes="0; 1"
            repeatCount="indefinite"
            values="1; 20"
          />
          <animate
            attributeName="stroke-opacity"
            begin="0s"
            calcMode="spline"
            dur="1.8s"
            keySplines="0.3, 0.61, 0.355, 1"
            keyTimes="0; 1"
            repeatCount="indefinite"
            values="1; 0"
          />
        </circle>
        <circle cx="22" cy="22" r="1">
          <animate
            attributeName="r"
            begin="-0.9s"
            calcMode="spline"
            dur="1.8s"
            keySplines="0.165, 0.84, 0.44, 1"
            keyTimes="0; 1"
            repeatCount="indefinite"
            values="1; 20"
          />
          <animate
            attributeName="stroke-opacity"
            begin="-0.9s"
            calcMode="spline"
            dur="1.8s"
            keySplines="0.3, 0.61, 0.355, 1"
            keyTimes="0; 1"
            repeatCount="indefinite"
            values="1; 0"
          />
        </circle>
      </g>
    </svg>
  );
}

export { Ripple };
```

---

### Satellite ring <a name="satellite-ring"></a>

A ring with a satellite dot that orbits the edge for propagation-style loading states.

- **Registry Key**: `satellite-ring`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/satellite-ring.json
```

#### How to Use

##### Example 1

```tsx
import { SatelliteRing } from "@/components/loading-ui/satellite-ring";
```

##### Example 2

```tsx
<SatelliteRing />
```

#### Component Source Code

##### File: `registry/components/loading-ui/satellite-ring.tsx`

```tsx
import { cn } from "@/lib/utils";

function SatelliteRing({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-satellite-ring-rotation {
          0% {
            transform: rotate(0deg);
          }

          100% {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "relative inline-block rounded-full border-2 border-current/25",
          className,
        )}
        style={{
          animation:
            "loading-ui-satellite-ring-rotation var(--duration, 1.5s) linear infinite",
        }}
        {...props}
      >
        <span
          aria-hidden="true"
          className="absolute top-0 left-0 rounded-full bg-current"
          style={{
            width: "33.333%",
            height: "33.333%",
            transform: "translate(-50%, 50%)",
          }}
        />
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { SatelliteRing };
```

---

### Skeleton <a name="skeleton"></a>

A skeleton block placeholder for content that is still loading.

- **Registry Key**: `skeleton`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/skeleton.json
```

#### How to Use

##### Example 1

```tsx
import { Skeleton } from "@/components/loading-ui/skeleton";
```

##### Example 2

```tsx
<Skeleton className="h-5 w-[160px]" />
```

#### Component Source Code

##### File: `registry/components/loading-ui/skeleton.tsx`

```tsx
import { cn } from "@/lib/utils";

function Skeleton({ className, style, ...props }: React.ComponentProps<"div">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-skeleton-pulse {
          50% {
            opacity: 0.5;
          }
        }
      `}</style>
      <div
        data-slot="skeleton"
        className={cn("bg-muted rounded-md", className)}
        style={{
          animationName: "loading-ui-skeleton-pulse",
          animationDuration: "var(--duration, 2s)",
          animationTimingFunction: "cubic-bezier(0.4, 0, 0.6, 1)",
          animationIterationCount: "infinite",
          ...style,
        }}
        {...props}
      />
    </>
  );
}

export { Skeleton };
```

---

### Spiral <a name="spiral"></a>

A spiral motion path for a distinctive busy state.

- **Registry Key**: `spiral`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/spiral.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

##### Example 1

```tsx
import { Spiral } from "@/components/loading-ui/spiral";
```

##### Example 2

```tsx
<Spiral />
```

#### Component Source Code

##### File: `registry/components/loading-ui/spiral.tsx`

```tsx
"use client";

import { motion } from "motion/react";
import { cn } from "@/lib/utils";

function Spiral({
  dots = 8,
  radius = 31.25,
  className,
  ...props
}: React.ComponentProps<"span"> & { dots?: number; radius?: number }) {
  return (
    <span
      role="status"
      className={cn("relative inline-block", className)}
      {...props}
    >
      {Array.from({ length: dots }, (_, index) => {
        const angle = (index / dots) * (2 * Math.PI);
        const x = `${50 + radius * Math.cos(angle)}%`;
        const y = `${50 + radius * Math.sin(angle)}%`;

        return (
          <motion.span
            key={index}
            aria-hidden="true"
            className="absolute inline-block rounded-full bg-current"
            style={{
              left: x,
              top: y,
              translate: "-50% -50%",
              width: `${150 / dots}%`,
              height: `${150 / dots}%`,
            }}
            animate={{
              scale: [0, 1, 0],
              opacity: [0, 1, 0],
            }}
            transition={{
              duration: 1.5,
              repeat: Infinity,
              delay: (index / dots) * 1.5,
              ease: "easeInOut",
            }}
          />
        );
      })}
      <span className="sr-only">Loading</span>
    </span>
  );
}

export { Spiral };
```

---

### Spokes <a name="spokes"></a>

A segmented spinner for lightweight indeterminate loading states.

- **Registry Key**: `spokes`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/spokes.json
```

#### How to Use

##### Example 1

```tsx
import { Spokes } from "@/components/loading-ui/spokes";
```

##### Example 2

```tsx
<Spokes />
```

#### Component Source Code

##### File: `registry/components/loading-ui/spokes.tsx`

```tsx
import { cn } from "@/lib/utils";

function Spokes({ className, style, ...props }: React.ComponentProps<"svg">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-spokes-spin {
          to {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <svg
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        className={cn(className)}
        style={{
          animationName: "loading-ui-spokes-spin",
          animationDuration: "var(--duration, 1s)",
          animationTimingFunction: "linear",
          animationIterationCount: "infinite",
          ...style,
        }}
        {...props}
      >
        <path
          d="M12 2V6M16.2 7.8L19.1 4.9M18 12H22M16.2 16.2L19.1 19.1M12 18V22M4.9 19.1L7.8 16.2M2 12H6M4.9 4.9L7.8 7.8"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
        />
      </svg>
    </>
  );
}

export { Spokes };
```

---

### Swirling <a name="swirling"></a>

A fluid swirl effect for high-motion loading affordances.

- **Registry Key**: `swirling`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/swirling.json
```

#### How to Use

##### Example 1

```tsx
import { Swirling } from "@/components/loading-ui/swirling";
```

##### Example 2

```tsx
<Swirling />
```

#### Component Source Code

##### File: `registry/components/loading-ui/swirling.tsx`

```tsx
"use client";

function Swirling(props: React.ComponentProps<"svg">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-swirling-spin {
          to {
            transform: rotate(360deg);
          }
        }

        @keyframes loading-ui-swirling-dash {
          0% {
            stroke-dasharray: 1, 800;
            stroke-dashoffset: 0;
          }
          50% {
            stroke-dasharray: 400, 400;
            stroke-dashoffset: -200px;
          }
          100% {
            stroke-dasharray: 800, 1;
            stroke-dashoffset: -800px;
          }
        }

        .loading-ui-swirling-circle {
          transform-origin: center;
          animation:
            loading-ui-swirling-dash var(--duration, 1.5s) ease-in-out infinite alternate,
            loading-ui-swirling-spin calc(var(--duration, 1.5s) * 1.333333) linear infinite;
        }
      `}</style>
      <svg viewBox="0 0 800 800" xmlns="http://www.w3.org/2000/svg" {...props}>
        <circle
          className="loading-ui-swirling-circle"
          cx="400"
          cy="400"
          r="200"
          fill="none"
          stroke="currentColor"
          strokeLinecap="round"
          strokeWidth="50"
        />
      </svg>
    </>
  );
}

export { Swirling };
```

---

### Terminal <a name="terminal"></a>

A terminal or CLI style loader for developer-facing experiences.

- **Registry Key**: `terminal`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/terminal.json
```

#### How to Use

##### Example 1

```tsx
import { Terminal } from "@/components/loading-ui/terminal";
```

##### Example 2

```tsx
<Terminal prompt="$" />
```

#### Component Source Code

##### File: `registry/components/loading-ui/terminal.tsx`

```tsx
import { cn } from "@/lib/utils";

type TerminalProps = React.ComponentProps<"span"> & {
  prompt?: string;
};

function Terminal({ className, prompt = ">", style, ...props }: TerminalProps) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-terminal-blink {
          0%,
          100% {
            opacity: 1;
          }

          50% {
            opacity: 0;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "inline-flex items-center gap-[0.25em] font-mono",
          className,
        )}
        style={style}
        {...props}
      >
        <span aria-hidden="true">{prompt}</span>
        <span
          aria-hidden="true"
          className="inline-block w-[0.5em] bg-current"
          style={{
            height: "1em",
            animation:
              "loading-ui-terminal-blink var(--duration, 1s) step-end infinite",
          }}
        />
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { Terminal };
```

---

### Text blink <a name="text-blink"></a>

A blinking text treatment for in-place loading copy.

- **Registry Key**: `text-blink`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/text-blink.json
```

#### How to Use

##### Example 1

```tsx
import { TextBlink } from "@/components/loading-ui/text-blink";
```

##### Example 2

```tsx
<TextBlink>Thinking</TextBlink>
```

#### Component Source Code

##### File: `registry/components/loading-ui/text-blink.tsx`

```tsx
import { cn } from "@/lib/utils";

type TextBlinkProps = Omit<React.ComponentProps<"span">, "children"> & {
  children: React.ReactNode;
  as?: React.ElementType;
  minOpacity?: number;
};

function TextBlink({
  children,
  as: Component = "p",
  className,
  minOpacity = 0.45,
  style,
  ...props
}: TextBlinkProps) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-text-blink {
          0%,
          100% {
            opacity: 1;
          }

          50% {
            opacity: var(--loading-ui-text-blink-opacity);
          }
        }
      `}</style>
      <Component
        className={cn("inline-block font-medium", className)}
        style={
          {
            ...style,
            "--loading-ui-text-blink-opacity": minOpacity,
            animation:
              "loading-ui-text-blink var(--duration, 2s) ease-in-out infinite",
          } as React.CSSProperties
        }
        {...props}
      >
        {children}
      </Component>
    </>
  );
}

export { TextBlink };
```

---

### Text dots <a name="text-dots"></a>

Trailing or inline dots for ongoing status text.

- **Registry Key**: `text-dots`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/text-dots.json
```

#### How to Use

##### Example 1

```tsx
import { TextDots } from "@/components/loading-ui/text-dots";
```

##### Example 2

```tsx
<TextDots>Thinking</TextDots>
```

#### Component Source Code

##### File: `registry/components/loading-ui/text-dots.tsx`

```tsx
import { cn } from "@/lib/utils";

type TextDotsProps = React.ComponentProps<"span"> & {
  dots?: number;
};

function TextDots({
  className,
  children,
  dots = 3,
  style,
  ...props
}: TextDotsProps) {
  const dotCount = Number.isFinite(dots) ? Math.max(1, Math.floor(dots)) : 3;

  return (
    <>
      <style>{`
        @keyframes loading-ui-text-dots {
          0%,
          100% {
            opacity: 0;
          }

          50% {
            opacity: 1;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("inline-flex items-center", className)}
        style={style}
        {...props}
      >
        <span>{children}</span>
        <span aria-hidden="true" className="inline-flex">
          {Array.from({ length: dotCount }, (_, index) => (
            <span
              key={index}
              style={{
                animation:
                  "loading-ui-text-dots var(--duration, 1.4s) infinite",
                animationDelay: `calc(var(--delay, 0.2s) * ${index + 1})`,
              }}
            >
              .
            </span>
          ))}
        </span>
      </span>
    </>
  );
}

export { TextDots };
```

---

### Text shimmer wave <a name="text-shimmer-wave"></a>

A wave of shimmer along text for animated placeholder copy.

- **Registry Key**: `text-shimmer-wave`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/text-shimmer-wave.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

##### Example 1

```tsx
import { TextShimmerWave } from "@/components/loading-ui/text-shimmer-wave";
```

##### Example 2

```tsx
<TextShimmerWave>Thinking</TextShimmerWave>
```

#### Component Source Code

##### File: `registry/components/loading-ui/text-shimmer-wave.tsx`

```tsx
"use client";

import { type JSX } from "react";
import { motion, type Transition } from "motion/react";
import { cn } from "@/lib/utils";

export type TextShimmerWaveProps = {
  children: string;
  as?: React.ElementType;
  className?: string;
  duration?: number;
  baseColor?: string;
  shimmerColor?: string;
  zDistance?: number;
  xDistance?: number;
  yDistance?: number;
  spread?: number;
  scaleDistance?: number;
  rotateYDistance?: number;
  transition?: Transition;
  style?: React.CSSProperties;
};

export function TextShimmerWave({
  children,
  as: Component = "p",
  className,
  duration = 1,
  baseColor,
  shimmerColor,
  zDistance = 10,
  xDistance = 2,
  yDistance = -2,
  spread = 1,
  scaleDistance = 1.1,
  rotateYDistance = 10,
  transition,
  style,
}: TextShimmerWaveProps) {
  const MotionComponent = motion.create(
    Component as keyof JSX.IntrinsicElements,
  );

  return (
    <MotionComponent
      className={cn("relative inline-block [perspective:500px]", className)}
      style={
        {
          ...style,
          "--base-color":
            baseColor ?? "color-mix(in oklab, currentColor 55%, transparent)",
          "--base-gradient-color": shimmerColor ?? "currentColor",
        } as React.CSSProperties
      }
    >
      {children.split("").map((char, i) => {
        const delay = (i * duration * (1 / spread)) / children.length;

        return (
          <motion.span
            key={i}
            className={cn(
              "inline-block whitespace-pre [transform-style:preserve-3d]",
            )}
            initial={{
              translateZ: 0,
              scale: 1,
              rotateY: 0,
              color: "var(--base-color)",
            }}
            animate={{
              translateZ: [0, zDistance, 0],
              translateX: [0, xDistance, 0],
              translateY: [0, yDistance, 0],
              scale: [1, scaleDistance, 1],
              rotateY: [0, rotateYDistance, 0],
              color: [
                "var(--base-color)",
                "var(--base-gradient-color)",
                "var(--base-color)",
              ],
            }}
            transition={{
              duration: duration,
              repeat: Infinity,
              repeatDelay: (children.length * 0.05) / spread,
              delay,
              ease: "easeInOut",
              ...transition,
            }}
          >
            {char}
          </motion.span>
        );
      })}
    </MotionComponent>
  );
}
```

---

### Text shimmer <a name="text-shimmer"></a>

A shimmer sweep across label text for premium loading states.

- **Registry Key**: `text-shimmer`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/text-shimmer.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

##### Example 1

```tsx
import { TextShimmer } from "@/components/loading-ui/text-shimmer";
```

##### Example 2

```tsx
<TextShimmer>Thinking</TextShimmer>
```

#### Component Source Code

##### File: `registry/components/loading-ui/text-shimmer.tsx`

```tsx
"use client";

import React, { useMemo, type JSX } from "react";
import { motion } from "motion/react";
import { cn } from "@/lib/utils";

export type TextShimmerProps = {
  children: string;
  as?: React.ElementType;
  className?: string;
  duration?: number;
  spread?: number;
  baseColor?: string;
  shimmerColor?: string;
  style?: React.CSSProperties;
};

function TextShimmerComponent({
  children,
  as: Component = "p",
  className,
  duration = 2,
  spread = 2,
  baseColor,
  shimmerColor,
  style,
}: TextShimmerProps) {
  const MotionComponent = motion.create(
    Component as keyof JSX.IntrinsicElements,
  );

  const dynamicSpread = useMemo(() => {
    return children.length * spread;
  }, [children, spread]);

  return (
    <MotionComponent
      className={cn(
        "relative inline-block bg-size-[250%_100%,auto] bg-clip-text",
        "[-webkit-text-fill-color:transparent]",
        "[background-repeat:no-repeat,padding-box] [--bg:linear-gradient(90deg,#0000_calc(50%-var(--spread)),var(--base-gradient-color),#0000_calc(50%+var(--spread)))]",
        className,
      )}
      initial={{ backgroundPosition: "100% center" }}
      animate={{ backgroundPosition: "0% center" }}
      transition={{
        repeat: Infinity,
        duration,
        ease: "linear",
      }}
      style={
        {
          ...style,
          "--spread": `${dynamicSpread}px`,
          "--base-color":
            baseColor ?? "color-mix(in oklab, currentColor 55%, transparent)",
          "--base-gradient-color": shimmerColor ?? "currentColor",
          backgroundImage: `var(--bg), linear-gradient(var(--base-color), var(--base-color))`,
        } as React.CSSProperties
      }
    >
      {children}
    </MotionComponent>
  );
}

export const TextShimmer = React.memo(TextShimmerComponent);
```

---

### Triple dot spinner <a name="triple-dot-spinner"></a>

Three dots arranged in a compact rotating spinner for buttons and list rows.

- **Registry Key**: `triple-dot-spinner`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/triple-dot-spinner.json
```

#### How to Use

##### Example 1

```tsx
import { TripleDotSpinner } from "@/components/loading-ui/triple-dot-spinner";
```

##### Example 2

```tsx
<span className="grid size-5 place-items-center">
  <TripleDotSpinner className="size-1.5" />
</span>
```

#### Component Source Code

##### File: `registry/components/loading-ui/triple-dot-spinner.tsx`

```tsx
import { cn } from "@/lib/utils";

function TripleDotSpinner({
  className,
  ...props
}: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-triple-dot-rotation {
          0% {
            transform: rotate(0deg);
          }

          100% {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("relative inline-block shrink-0", className)}
        {...props}
      >
        <span
          aria-hidden="true"
          className="absolute inset-0"
          style={{
            animation:
              "loading-ui-triple-dot-rotation var(--duration, 2s) ease-in-out infinite",
          }}
        >
          <span className="absolute top-1/2 left-1/2 size-full -translate-x-1/2 -translate-y-1/2 rounded-full bg-current" />
          <span className="absolute top-1/2 left-1/2 size-full -translate-x-[200%] -translate-y-1/2 rounded-full bg-current" />
          <span className="absolute top-1/2 left-1/2 size-full translate-x-full -translate-y-1/2 rounded-full bg-current" />
        </span>
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { TripleDotSpinner };
```

---

### Twin orbit <a name="twin-orbit"></a>

Two orbiting markers for a balanced circular loading animation.

- **Registry Key**: `twin-orbit`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/twin-orbit.json
```

#### How to Use

##### Example 1

```tsx
import { TwinOrbit } from "@/components/loading-ui/twin-orbit";
```

##### Example 2

```tsx
<TwinOrbit />
```

#### Component Source Code

##### File: `registry/components/loading-ui/twin-orbit.tsx`

```tsx
import { cn } from "@/lib/utils";

function TwinOrbit({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-twin-orbit-rotate {
          100% {
            transform: rotate(360deg) translate(155%);
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "relative inline-block aspect-square rounded-full bg-current",
          className,
        )}
        {...props}
      >
        <span
          aria-hidden="true"
          className="absolute inset-0 rounded-full bg-current"
          style={{
            transform: "rotate(0deg) translate(155%)",
            animation:
              "loading-ui-twin-orbit-rotate var(--duration, 1s) ease infinite",
          }}
        />
        <span
          aria-hidden="true"
          className="absolute inset-0 rounded-full bg-current"
          style={{
            transform: "rotate(0deg) translate(155%)",
            animation:
              "loading-ui-twin-orbit-rotate var(--duration, 1s) ease infinite",
            animationDelay: "calc(var(--duration, 1s) / 2)",
          }}
        />
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { TwinOrbit };
```

---

### Typing <a name="typing"></a>

Vertical wave dots that read like keystrokes—chat, search, and co-editing surfaces.

- **Registry Key**: `typing`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/typing.json
```

#### How to Use

##### Example 1

```tsx
import { Typing } from "@/components/loading-ui/typing";
```

##### Example 2

```tsx
<Typing />
```

#### Component Source Code

##### File: `registry/components/loading-ui/typing.tsx`

```tsx
import { cn } from "@/lib/utils";

function Typing({
  className,
  dots = 3,
  ...props
}: React.ComponentProps<"span"> & { dots?: number }) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-typing {
          0%,
          100% {
            transform: translateY(0);
            opacity: 0.5;
          }

          50% {
            transform: translateY(-50%);
            opacity: 1;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("inline-flex items-center gap-[12%]", className)}
        {...props}
      >
        {Array.from({ length: dots }, (_, index) => (
          <span
            key={index}
            aria-hidden="true"
            className="inline-block aspect-square grow rounded-full bg-current"
            style={{
              animation: "loading-ui-typing var(--duration, 1s) infinite",
              animationDelay: `calc(var(--delay, 160ms) * ${index})`,
            }}
          />
        ))}
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { Typing };
```

---

### Wandering eyes <a name="wandering-eyes"></a>

A playful pair of animated eyes for expressive or brand-led loading states.

- **Registry Key**: `wandering-eyes`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/wandering-eyes.json
```

#### How to Use

##### Example 1

```tsx
import { WanderingEyes } from "@/components/loading-ui/wandering-eyes";
```

##### Example 2

```tsx
<WanderingEyes className="h-12 w-[108px]" />
```

#### Component Source Code

##### File: `registry/components/loading-ui/wandering-eyes.tsx`

```tsx
import { cn } from "@/lib/utils";

type WanderingEyesProps = React.ComponentProps<"span"> & {
  eyeScale?: number;
  gapScale?: number;
  pupilScale?: number;
  blinkScale?: number;
  travelScale?: number;
};

function clamp(value: number, min: number, max: number) {
  return Math.min(Math.max(value, min), max);
}

function WanderingEyes({
  className,
  style,
  eyeScale = 0.62,
  gapScale = 0.09,
  pupilScale = 0.32,
  blinkScale = 0.375,
  travelScale = 0.3125,
  ...props
}: WanderingEyesProps) {
  const safeEyeScale = clamp(eyeScale, 0.28, 0.7);
  const safeGapScale = clamp(gapScale, 0.04, 0.3);
  const safePupilScale = clamp(pupilScale, 0.12, 0.45);
  const safeBlinkScale = clamp(blinkScale, 0.15, 1);
  const safeTravelScale = clamp(travelScale, 0.08, 0.5);
  const eyesStyle = {
    ...style,
    "--loading-ui-wandering-eyes-eye": `${(safeEyeScale * 100).toFixed(2)}cqmin`,
    "--loading-ui-wandering-eyes-gap": `${(safeGapScale * 100).toFixed(2)}cqmin`,
    "--loading-ui-wandering-eyes-pupil-scale": `${safePupilScale}`,
    "--loading-ui-wandering-eyes-blink": `${safeBlinkScale}`,
    "--loading-ui-wandering-eyes-travel-scale": `${safeTravelScale}`,
  } as React.CSSProperties;

  return (
    <>
      <style>{`
        @keyframes loading-ui-wandering-eyes-move {
          0%,
          10% {
            background-position: 0 0;
          }

          13%,
          40% {
            background-position: calc(var(--loading-ui-wandering-eyes-eye) * var(--loading-ui-wandering-eyes-travel-scale) * -1) 0;
          }

          43%,
          70% {
            background-position: calc(var(--loading-ui-wandering-eyes-eye) * var(--loading-ui-wandering-eyes-travel-scale)) 0;
          }

          73%,
          90% {
            background-position: 0 calc(var(--loading-ui-wandering-eyes-eye) * var(--loading-ui-wandering-eyes-travel-scale));
          }

          93%,
          100% {
            background-position: 0 0;
          }
        }

        @keyframes loading-ui-wandering-eyes-blink {
          0%,
          10%,
          12%,
          20%,
          22%,
          40%,
          42%,
          60%,
          62%,
          70%,
          72%,
          90%,
          92%,
          98%,
          100% {
            height: var(--loading-ui-wandering-eyes-eye);
          }

          11%,
          21%,
          41%,
          61%,
          71%,
          91%,
          99% {
            height: calc(var(--loading-ui-wandering-eyes-eye) * var(--loading-ui-wandering-eyes-blink));
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "@container-[size] relative inline-flex aspect-9/4 items-center justify-center align-middle [--eye-color:color-mix(in_srgb,currentColor_16%,transparent)] [--pupil-color:currentColor]",
          className,
        )}
        style={eyesStyle}
        {...props}
      >
        <span
          aria-hidden="true"
          className="inline-flex items-center justify-center gap-(--loading-ui-wandering-eyes-gap)"
        >
          {Array.from({ length: 2 }, (_, index) => (
            <span
              key={index}
              className="inline-block rounded-full"
              style={{
                width: "var(--loading-ui-wandering-eyes-eye)",
                height: "var(--loading-ui-wandering-eyes-eye)",
                backgroundColor: "var(--eye-color)",
                backgroundImage:
                  "radial-gradient(circle calc(var(--loading-ui-wandering-eyes-eye) * var(--loading-ui-wandering-eyes-pupil-scale)), var(--pupil-color) 100%, transparent 0)",
                backgroundRepeat: "no-repeat",
                animation:
                  "loading-ui-wandering-eyes-move var(--duration, 10s) infinite, loading-ui-wandering-eyes-blink var(--duration, 10s) infinite",
              }}
            />
          ))}
        </span>
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { WanderingEyes };
```

---

### Wave <a name="wave"></a>

A wave of bars for audio, sync, or progress-adjacent loading.

- **Registry Key**: `wave`

#### Installation

```bash
npx shadcn@latest add https://loading-ui.com/r/wave.json
```

#### How to Use

##### Example 1

```tsx
import { Wave } from "@/components/loading-ui/wave";
```

##### Example 2

```tsx
<Wave className="h-12 w-24" />
```

#### Component Source Code

##### File: `registry/components/loading-ui/wave.tsx`

```tsx
import { cn } from "@/lib/utils";

const WAVE_BAR_HEIGHTS = ["50%", "75%", "100%", "75%", "50%"] as const;

function Wave({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-wave {
          0%,
          100% {
            transform: scaleY(1);
          }

          50% {
            transform: scaleY(0.6);
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("inline-flex items-center gap-[2.5%]", className)}
        {...props}
      >
        {WAVE_BAR_HEIGHTS.map((height, index) => (
          <span
            key={index}
            aria-hidden="true"
            className="inline-block rounded-full bg-current"
            style={{
              width: "12.5%",
              height,
              animation:
                "loading-ui-wave var(--duration, 1s) ease-in-out infinite",
              animationDelay: `calc(var(--delay, 100ms) * ${index})`,
            }}
          />
        ))}
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { Wave };
```

---
