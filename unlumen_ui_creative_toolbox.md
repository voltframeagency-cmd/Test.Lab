# Unlumen UI Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **Lumen UI** (ui.unlumen.com) — copy-paste React components designed with Tailwind CSS, TypeScript, Framer Motion, and WebGL Shaders.

## Table of Contents

- [Animations](#animations)
  - [AI Chat](#ai-chat)
  - [Vercel Animate Count](#animate-count)
  - [Animate Digits](#animate-digits)
  - [Animated List](#animated-list)
  - [Apple Switch](#apple-switch)
  - [Cursor Image Trail](#cursor-image-trail)
  - [Dia Text Reveal](#dia-text-reveal)
  - [Favicon Search](#favicon-search)
  - [File Tree](#file-tree)
  - [Gooey SVG Filter](#gooey-svg-filter)
  - [Hover Expand](#hover-expand)
  - [Hover Feature Cards](#hover-feature-cards)
  - [Hover Image List](#hover-image-list)
  - [Matrix](#matrix)
  - [Motion FAQs Accordion](#motion-faqs-accordion)
  - [Pinned List](#pinned-list)
  - [Side By Side Slide](#side-by-side-slide)
  - [Smart Animate Text](#smart-animate-text)
  - [Stacked Feature Cards](#stacked-feature-cards)
  - [Team Stack](#team-stack)
  - [Tilt Card](#tilt-card)
  - [Vercel Snap Text](#vercel-snap-text)
  - [Youtube Video Ambient](#video-ambient)
  - [Video Slider](#video-slider)
- [Backgrounds & Shaders](#backgrounds-and-shaders)
  - [Aurora Bars](#aurora-bars)
  - [Gravity Stars](#gravity-stars)
  - [Pixel Background](#pixel)
  - [Pixel Liquid Background](#pixel-liquid-bg)
  - [Square Pattern](#square-pattern)
  - [Wave Background](#wave-background)
- [Icons](#icons)
  - [Cards View Icon](#cards-view-icon)
  - [Compact View Icon](#compact-view-icon)
  - [List View Icon](#list-view-icon)
  - [Sidebar Toggle Icon](#sidebar-toggle-icon)
- [Image Effects](#image-effects)
  - [Horizontal Depth Fade](#horizontal-depth-fade)
  - [Orbital Image Wheel](#orbital-image-wheel)
- [Marketing](#marketing)
  - [Aurora Card](#aurora-card)
  - [Blob Card](#blob-card)
  - [Inline Testimonials](#inline-testimonials)
  - [Vertical Marquee](#vertical-marquee)
- [Navigation](#navigation)
  - [Dock](#dock)
  - [Expandable Navbar](#expandable-navbar)
  - [Floating Navbar](#floating-navbar)
  - [Gooey Navbar Menu](#gooey-navbar-menu)
  - [Motion Navigation Menu](#motion-navigation-menu)
  - [Motion Tabs Menu](#motion-tabs-menu)
  - [Sidebar 001](#sidebar-001)
  - [Sidebar 002](#sidebar-002)

---

## Animations <a name="animations"></a>

### AI Chat <a name="ai-chat"></a>

Animated chat interface with physics-based input, typing indicator, and aurora burst on send.

- **Tier**: Pro
- **Category**: Animations

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { AiChat } from "@/components/unlumen-ui/ai-chat";

<AiChat />;
```

---

### Vercel Animate Count <a name="animate-count"></a>

A number display that animates transitions between values with a blur + slide effect.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/animate-count.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { AnimateCount } from "@/components/unlumen-ui/animate-count";

<AnimateCount>{count}</AnimateCount>;
```

#### Demo Code

```tsx
"use client";

import { useEffect, useState } from "react";
import { AnimateCount } from "@/registry/components/unlumen/animate-count";

function AutoCountUp() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    if (count >= 100) return;
    const id = setTimeout(() => setCount((c) => c + 1), 1000);
    return () => clearTimeout(id);
  }, [count]);

  return (
    <div className="flex flex-col items-center gap-1">
      <AnimateCount className="text-4xl font-bold">{count}</AnimateCount>
      <span className="text-xs text-muted-foreground">auto count-up</span>
    </div>
  );
}

export default function AnimateCountDemo() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const onKey = (e: KeyboardEvent) => {
      if (e.key === "i") setCount((c) => c + 1);
      if (e.key === "o") setCount((c) => Math.max(0, c - 1));
    };
    window.addEventListener("keydown", onKey);
    return () => window.removeEventListener("keydown", onKey);
  }, []);

  return (
    <div className="flex flex-col items-center justify-center gap-10 p-8">
      <div className="flex flex-col items-center gap-4">
        <AnimateCount className="text-6xl font-bold">{count}</AnimateCount>
        <div className="flex gap-3">
          <button
            onClick={() => setCount((c) => Math.max(0, c - 1))}
            className="rounded-md border border-border bg-muted px-4 py-2 text-sm font-medium hover:bg-muted/80 transition-colors"
          >
            − <kbd className="ml-1 text-xs text-muted-foreground">O</kbd>
          </button>
          <button
            onClick={() => setCount((c) => c + 1)}
            className="rounded-md border border-border bg-muted px-4 py-2 text-sm font-medium hover:bg-muted/80 transition-colors"
          >
            + <kbd className="ml-1 text-xs text-muted-foreground">I</kbd>
          </button>
        </div>
      </div>

      <div className="w-px h-8 bg-border" />

      <AutoCountUp />
    </div>
  );
}
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `children` | `number` | `-` | The number to display. |
| `animate?` | `boolean` | `true` | Whether to animate transitions. Set to false to disable. |
| `className?` | `string` | `-` | Additional className applied to the wrapper. |

#### Component Source Code

##### File: `registry/components/unlumen/animate-count/index.tsx`

```tsx
"use client";

import { useEffect, useState } from "react";
import { AnimatePresence, motion } from "motion/react";

import { cn } from "@/lib/utils";

export const ANIMATE_COUNT_DURATION_MS = 450;

const EASING = [0.23, 0.88, 0.26, 0.92] as const;

interface AnimateCountProps {
  children: number;
  animate?: boolean;
  className?: string;
}

function AnimateCount({
  children: count,
  animate = true,
  className,
}: AnimateCountProps) {
  const [prev, setPrev] = useState<number | null>(null);
  const [displayCount, setDisplayCount] = useState(count);

  useEffect(() => {
    if (animate) setPrev(displayCount);
    setDisplayCount(count);
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [count, animate]);

  return (
    <div
      className={cn(
        "grid place-items-center tabular-nums tracking-tight",
        "[&>*]:col-start-1 [&>*]:row-start-1",
        className,
      )}
    >
      <AnimatePresence initial={false}>
        {animate && prev !== null && prev !== displayCount && (
          <motion.div
            key={`exit-${prev}-${displayCount}`}
            aria-hidden
            initial={{ opacity: 1, filter: "blur(0px)", y: 0 }}
            animate={{ opacity: 0, filter: "blur(2px)", y: -12 }}
            transition={{
              duration: ANIMATE_COUNT_DURATION_MS / 1000,
              ease: EASING,
            }}
            onAnimationComplete={() => setPrev(null)}
          >
            {prev}
          </motion.div>
        )}
      </AnimatePresence>
      <motion.div
        key={`enter-${displayCount}`}
        initial={animate ? { opacity: 0, filter: "blur(2px)", y: 8 } : false}
        animate={{ opacity: 1, filter: "blur(0px)", y: 0 }}
        transition={{
          duration: ANIMATE_COUNT_DURATION_MS / 1000,
          ease: EASING,
        }}
      >
        {displayCount}
      </motion.div>
    </div>
  );
}

export { AnimateCount, type AnimateCountProps };
```

---

### Animate Digits <a name="animate-digits"></a>

Per-digit blur-slide animation with direction awareness — only the digits that change animate, sliding up when increasing and down when decreasing.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/animate-digits.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { AnimateDigits } from "@/components/unlumen-ui/animate-digits";

export default function Example() {
  return <AnimateDigits value="05:00" className="text-6xl font-bold" />;
}
```

#### Demo Code

```tsx
"use client";

import { useEffect, useRef, useState } from "react";
import { motion } from "motion/react";
import { AnimateDigits } from "@/registry/components/unlumen/animate-digits";
import { Button } from "@workspace/ui/components/ui/button";

function formatTime(seconds: number): string {
  const m = Math.floor(seconds / 60);
  const s = seconds % 60;
  return String(m).padStart(2, "0") + ":" + String(s).padStart(2, "0");
}

interface AnimateDigitsDemoProps {
  gap?: number;
  enterStiffness?: number;
  enterDamping?: number;
  exitStiffness?: number;
  exitDamping?: number;
  direction?: "dynamic" | "up" | "down";
  enterY?: number;
  enterBlur?: number;
  enterScale?: number;
  animationDelay?: number;
}

const SCROLL_THRESHOLD = 80;

export default function AnimateDigitsDemo({
  gap = 2,
  enterStiffness,
  enterDamping,
  exitStiffness,
  exitDamping,
  direction,
  enterY,
  enterBlur,
  enterScale,
  animationDelay,
}: AnimateDigitsDemoProps) {
  const [minutes, setMinutes] = useState(5);
  const [remaining, setRemaining] = useState<number | null>(null);
  const [running, setRunning] = useState(false);
  const [scrollDir, setScrollDir] = useState<"up" | "down" | null>(null);
  const intervalRef = useRef<ReturnType<typeof setInterval> | null>(null);
  const scrollAccum = useRef(0);
  const scrollIdleTimer = useRef<ReturnType<typeof setTimeout> | null>(null);

  const idle = !running && remaining === null;

  function start() {
    setRemaining(minutes * 60);
    setRunning(true);
  }

  function reset() {
    if (intervalRef.current) clearInterval(intervalRef.current);
    setRunning(false);
    setRemaining(null);
  }

  function handleWheel(e: React.WheelEvent) {
    if (!idle) return;
    e.preventDefault();
    setScrollDir(e.deltaY > 0 ? "down" : "up");
    if (scrollIdleTimer.current) clearTimeout(scrollIdleTimer.current);
    scrollIdleTimer.current = setTimeout(() => setScrollDir(null), 400);
    scrollAccum.current += e.deltaY;
    if (Math.abs(scrollAccum.current) >= SCROLL_THRESHOLD) {
      const steps = Math.trunc(scrollAccum.current / SCROLL_THRESHOLD);
      scrollAccum.current -= steps * SCROLL_THRESHOLD;
      setMinutes((m) => Math.max(1, Math.min(99, m - steps)));
    }
  }

  useEffect(() => {
    if (!running || remaining === null) return;
    if (remaining <= 0) {
      setRunning(false);
      return;
    }
    intervalRef.current = setInterval(() => {
      setRemaining((r) => {
        if (r === null || r <= 1) {
          clearInterval(intervalRef.current!);
          setRunning(false);
          return 0;
        }
        return r - 1;
      });
    }, 1000);
    return () => clearInterval(intervalRef.current!);
  }, [running, remaining]);

  const display =
    remaining !== null ? formatTime(remaining) : formatTime(minutes * 60);

  return (
    <div className="flex flex-col items-center justify-center gap-8 p-8">
      <span className="text-lg text-muted-foreground flex flex-col items-center gap-4 ">
        {idle ? "scroll to set time" : "Concentration"}
        <div className="h-25 w-px bg-gradient-to-b from-border" />
      </span>
      <div
        className="flex relative items-center gap-4"
        onWheel={handleWheel}
        style={{ cursor: idle ? "ns-resize" : "default" }}
      >
        <div className="flex absolute left-55 flex-col items-center gap-[5px]">
          {[0, 1, 2].map((i) => {
            const activeIndex =
              scrollDir === "up" ? 0 : scrollDir === "down" ? 2 : 1;
            const isActive = idle && activeIndex === i;
            return (
              <motion.div
                key={i}
                animate={{
                  height: isActive ? 18 : 8,
                  opacity: isActive ? 1 : 0.2,
                  y:
                    isActive && scrollDir === "up"
                      ? -2
                      : isActive && scrollDir === "down"
                        ? 2
                        : 0,
                }}
                transition={{ type: "spring", stiffness: 400, damping: 28 }}
                className="w-[3px] rounded-full bg-foreground"
              />
            );
          })}
        </div>
        <div className="flex flex-col items-center gap-3">
          <AnimateDigits
            value={display}
            gap={gap}
            className="text-7xl font-bold font-medium tracking-tighter"
            enterStiffness={enterStiffness}
            enterDamping={enterDamping}
            exitStiffness={exitStiffness}
            exitDamping={exitDamping}
            direction={direction}
            enterY={enterY}
            enterBlur={enterBlur}
            enterScale={enterScale}
            animationDelay={animationDelay}
          />
        </div>
      </div>

      {idle && (
        <Button variant="outline" size="md" onClick={start}>
          Start
        </Button>
      )}

      {(running || remaining !== null) && (
        <Button variant="destructive" size="md" onClick={reset}>
          Reset
        </Button>
      )}
    </div>
  );
}
```

#### Component Source Code

##### File: `registry/components/unlumen/animate-digits/index.tsx`

```tsx
"use client";

import { useEffect, useRef, useState } from "react";
import { AnimatePresence, motion, useSpring, useTransform } from "motion/react";

import { cn } from "@/lib/utils";

interface AnimateDigitsProps {
  value: string;
  gap?: number;
  className?: string;
  digitClassName?: string;
  enterStiffness?: number;
  enterDamping?: number;
  exitStiffness?: number;
  exitDamping?: number;
  direction?: "dynamic" | "up" | "down";
  enterY?: number;
  enterBlur?: number;
  enterScale?: number;
  animationDelay?: number;
}

interface ExitItem {
  id: number;
  char: string;
  exitY: number;
}

let _id = 0;

function DigitCell({
  char,
  isDigit,
  className,
  enterStiffness = 170,
  enterDamping = 10,
  exitStiffness = 170,
  exitDamping = 15,
  direction = "dynamic",
  enterY = 32,
  enterBlur = 52,
  enterScale = 0.7,
}: {
  char: string;
  isDigit: boolean;
  className?: string;
  enterStiffness?: number;
  enterDamping?: number;
  exitStiffness?: number;
  exitDamping?: number;
  direction?: "dynamic" | "up" | "down";
  enterY?: number;
  enterBlur?: number;
  enterScale?: number;
}) {
  const [exitQueue, setExitQueue] = useState<ExitItem[]>([]);

  const prevCharRef = useRef(char);
  const isFirstRender = useRef(true);

  // Springs — jump() snaps the spring itself (position + velocity reset)
  const springConfig = { stiffness: enterStiffness, damping: enterDamping };
  const y = useSpring(0, springConfig);
  const opacity = useSpring(1, springConfig);
  const scale = useSpring(1, springConfig);
  const blur = useSpring(0, springConfig);
  const filter = useTransform(blur, (v) => `blur(${v}px)`);

  useEffect(() => {
    if (!isDigit) return;

    const prev = prevCharRef.current;
    prevCharRef.current = char;

    if (isFirstRender.current) {
      isFirstRender.current = false;
      return;
    }

    if (char === prev || !/\d/.test(prev)) return;

    const up =
      direction === "dynamic"
        ? Number(char) > Number(prev)
        : direction === "up";

    const id = _id++;
    setExitQueue((q) => {
      const next = [...q, { id, char: prev, exitY: up ? -enterY : enterY }];
      return next.length > 3 ? next.slice(-3) : next;
    });

    y.jump(up ? enterY : -enterY);
    opacity.jump(0);
    scale.jump(enterScale);
    blur.jump(enterBlur);

    y.set(0);
    opacity.set(1);
    scale.set(1);
    blur.set(0);
  }, [
    char,
    isDigit,
    direction,
    enterY,
    enterBlur,
    enterScale,
    y,
    opacity,
    scale,
    blur,
  ]);

  if (!isDigit) {
    return <span className={className}>{char}</span>;
  }

  return (
    <div
      className={cn(
        "relative grid place-items-center [&>*]:col-start-1 [&>*]:row-start-1",
        className,
      )}
    >
      <AnimatePresence>
        {exitQueue.map(({ id, char: exitChar, exitY }) => (
          <motion.span
            key={id}
            aria-hidden
            initial={{ opacity: 1, scale: 1, filter: "blur(0px)", y: 0 }}
            animate={{ opacity: 0, scale: 0.7, filter: "blur(10px)", y: exitY }}
            transition={{
              type: "spring",
              stiffness: exitStiffness,
              damping: exitDamping,
            }}
            onAnimationComplete={() =>
              setExitQueue((q) => q.filter((item) => item.id !== id))
            }
          >
            {exitChar}
          </motion.span>
        ))}
      </AnimatePresence>
      <motion.span style={{ opacity, scale, filter, y }}>{char}</motion.span>
    </div>
  );
}

function AnimateDigits({
  value,
  gap = 2,
  className,
  digitClassName,
  enterStiffness,
  enterDamping,
  exitStiffness,
  exitDamping,
  direction,
  enterY,
  enterBlur,
  enterScale,
  animationDelay = 80,
}: AnimateDigitsProps) {
  // displayedValue is what the cells actually render — advanced one step at a time
  const [displayedValue, setDisplayedValue] = useState(value);

  const pendingQueue = useRef<string[]>([]);
  const isAnimating = useRef(false);
  const timerRef = useRef<ReturnType<typeof setTimeout> | null>(null);
  const displayedRef = useRef(value);

  const processQueue = () => {
    if (pendingQueue.current.length === 0) {
      isAnimating.current = false;
      return;
    }

    isAnimating.current = true;
    const next = pendingQueue.current.shift()!;
    displayedRef.current = next;
    setDisplayedValue(next);

    timerRef.current = setTimeout(processQueue, animationDelay);
  };

  useEffect(() => {
    if (value === displayedRef.current) return;

    pendingQueue.current.push(value);

    if (!isAnimating.current) {
      processQueue();
    }
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [value]);

  // When animationDelay changes while animating, the in-flight timer keeps the
  // old delay for its last tick — acceptable; no need to restart the queue.

  useEffect(() => {
    return () => {
      if (timerRef.current) clearTimeout(timerRef.current);
    };
  }, []);

  const chars = displayedValue.split("");

  return (
    <div
      className={cn("flex items-center tabular-nums", className)}
      style={{ gap }}
    >
      {chars.map((char, i) => (
        <DigitCell
          key={i}
          char={char}
          isDigit={/\d/.test(char)}
          className={digitClassName}
          enterStiffness={enterStiffness}
          enterDamping={enterDamping}
          exitStiffness={exitStiffness}
          exitDamping={exitDamping}
          direction={direction}
          enterY={enterY}
          enterBlur={enterBlur}
          enterScale={enterScale}
        />
      ))}
    </div>
  );
}

export { AnimateDigits, type AnimateDigitsProps };
```

---

### Animated List <a name="animated-list"></a>

A real-time push feed where new items animate in from the top with spring physics — Stripe Radar style.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/animated-list.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { AnimatedList } from "@/components/unlumen-ui/animated-list";

type Item = { id: number; message: string };

const [items, setItems] = useState<Item[]>([]);

// Prepend new items to show at top:
setItems((prev) => [newItem, ...prev].slice(0, 8));

<AnimatedList
  items={items}
  renderItem={(item) => (
    <div className="rounded-xl border p-3">{item.message}</div>
  )}
/>;
```

#### Demo Code

```tsx
"use client";

import * as React from "react";
import {
  AnimatedList,
  type AnimationType,
} from "@/registry/components/unlumen/animated-list";

type Notification = {
  id: number;
  icon: string;
  title: string;
  description: string;
  time: string;
};

const NOTIFICATIONS: Notification[] = [
  {
    id: 1,
    icon: "💳",
    title: "Payment received",
    description: "acme@corp.com · $2,400.00",
    time: "just now",
  },
  {
    id: 2,
    icon: "🚀",
    title: "Deployment succeeded",
    description: "main · production · 12s",
    time: "1m ago",
  },
  {
    id: 3,
    icon: "🔔",
    title: "New subscriber",
    description: "leo@unlumen.dev joined Pro",
    time: "2m ago",
  },
  {
    id: 4,
    icon: "🛡️",
    title: "Security alert",
    description: "New login from Paris, FR",
    time: "5m ago",
  },
  {
    id: 5,
    icon: "📦",
    title: "Order shipped",
    description: "Order #8821 is on its way",
    time: "8m ago",
  },
  {
    id: 6,
    icon: "⭐",
    title: "New review",
    description: '5 stars · "Absolutely love it"',
    time: "10m ago",
  },
];

function NotificationItem({ item }: { item: Notification }) {
  return (
    <div className="flex items-start gap-3 rounded-xl border border-foreground/10 bg-background/60 px-4 py-3 backdrop-blur-sm">
      <span className="text-xl leading-none mt-0.5">{item.icon}</span>
      <div className="flex-1 min-w-0">
        <p className="text-sm font-medium leading-none truncate">
          {item.title}
        </p>
        <p className="text-xs text-muted-foreground mt-1 truncate">
          {item.description}
        </p>
      </div>
      <span className="text-xs text-muted-foreground shrink-0 mt-0.5">
        {item.time}
      </span>
    </div>
  );
}

interface AnimatedListDemoProps {
  maxVisible?: number;
  gap?: number;
  animation?: AnimationType;
}

export const AnimatedListDemo = ({
  maxVisible = 8,
  gap = 12,
  animation = "scale",
}: AnimatedListDemoProps) => {
  const [items, setItems] = React.useState<Notification[]>(
    NOTIFICATIONS.slice(0, 3),
  );
  const counterRef = React.useRef(100);

  React.useEffect(() => {
    const pool = [...NOTIFICATIONS];
    let poolIndex = 0;

    const interval = setInterval(() => {
      const source = pool[poolIndex % pool.length];
      poolIndex++;
      const newItem: Notification = {
        ...source,
        id: counterRef.current++,
        time: "just now",
      };
      setItems((prev) => [newItem, ...prev].slice(0, maxVisible));
    }, 2000);

    return () => clearInterval(interval);
  }, [maxVisible]);

  return (
    <div className="w-full max-w-sm mx-auto p-4">
      <AnimatedList
        items={items}
        renderItem={(item) => <NotificationItem item={item} />}
        maxVisible={maxVisible}
        gap={gap}
        animation={animation}
      />
    </div>
  );
};

export default AnimatedListDemo;
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `items` | `T[]` | `-` | Array of items. Each item must have a unique `id` (string or number). Prepend new items at index 0 for top-push behavior. |
| `renderItem` | `(item: T, index: number) =&gt; React.ReactNode` | `-` | Render function called for each visible item. |
| `maxVisible?` | `number` | `8` | Maximum number of items rendered at once. Older items are removed. |
| `gap?` | `string` | `&quot;gap-3&quot;` | Tailwind gap class applied between items. |
| `className?` | `string` | `-` | Extra classes on the list container. |

#### Component Source Code

##### File: `registry/components/unlumen/animated-list/index.tsx`

```tsx
"use client";

import * as React from "react";
import { AnimatePresence, motion } from "motion/react";

import { cn } from "@/lib/utils";

export type AnimationType = "scale" | "slide" | "fade" | "bounce";

export interface AnimatedListProps<T> {
  /** index 0 = newest */
  items: T[];
  renderItem: (item: T, index: number) => React.ReactNode;
  /** @default 8 */
  maxVisible?: number;
  /** @default 12 */
  gap?: number;
  /** @default "scale" */
  animation?: AnimationType;
  className?: string;
}

function getAnimationVariants(type: AnimationType) {
  switch (type) {
    case "slide":
      return {
        initial: { opacity: 0, y: -30 },
        animate: { opacity: 1, y: 0 },
        exit: { opacity: 0, y: -20 },
      };
    case "fade":
      return {
        initial: { opacity: 0 },
        animate: { opacity: 1 },
        exit: { opacity: 0 },
      };
    case "bounce":
      return {
        initial: { opacity: 0, y: -20, scale: 0.8 },
        animate: { opacity: 1, y: 0, scale: 1 },
        exit: { opacity: 0, scale: 0.8 },
      };
    case "scale":
    default:
      return {
        initial: { opacity: 0, y: -20, scale: 0.95 },
        animate: { opacity: 1, y: 0, scale: 1 },
        exit: { opacity: 0, scale: 0.9 },
      };
  }
}

export function AnimatedList<T extends { id: string | number }>({
  items,
  renderItem,
  maxVisible = 8,
  gap = 12,
  animation = "scale",
  className,
}: AnimatedListProps<T>) {
  const visible = items.slice(0, maxVisible);
  const variants = getAnimationVariants(animation);

  return (
    <div className={cn("flex flex-col", className)} style={{ gap: `${gap}px` }}>
      <AnimatePresence mode="popLayout" initial={false}>
        {visible.map((item, index) => (
          <motion.div
            key={item.id}
            layout
            initial={variants.initial}
            animate={variants.animate}
            exit={variants.exit}
            transition={{
              type: "spring",
              stiffness: 350,
              damping: 28,
              layout: { type: "spring", stiffness: 350, damping: 28 },
            }}
          >
            {renderItem(item, index)}
          </motion.div>
        ))}
      </AnimatePresence>
    </div>
  );
}
```

---

### Apple Switch <a name="apple-switch"></a>

Animated toggle switch inspired by iOS, with draggable thumb, liquid press feedback, spring physics, size presets, and tone variants.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/apple-switch.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
"use client";

import { useState } from "react";
import { AppleSwitch } from "@/components/unlumen-ui/apple-switch";

export default function Example() {
  const [enabled, setEnabled] = useState(false);

  return (
    <AppleSwitch
      checked={enabled}
      onCheckedChange={setEnabled}
      label="Auto sync"
      description="Keep project data up to date."
    />
  );
}
```

#### Demo Code

```tsx
"use client";

import { useState } from "react";
import { AppleSwitch } from "@/registry/components/unlumen/apple-switch";
import { cn } from "@workspace/ui/lib/utils";

const tones = ["neutral", "accent"] as const;

const AppleSwitchDemo = () => {
  const [tone, setTone] = useState<(typeof tones)[number]>("neutral");
  const [active, setActive] = useState(true);
  const [inactive, setInactive] = useState(false);

  return (
    <div className="mx-auto flex w-full max-w-sm flex-col items-center gap-5 rounded-xl p-5">
      <div className="flex items-center gap-2">
        {tones.map((toneOption) => (
          <button
            key={toneOption}
            type="button"
            aria-label={`${toneOption} color`}
            aria-pressed={tone === toneOption}
            onClick={() => setTone(toneOption)}
            className={cn(
              "size-5 rounded-full border border-white/30 outline-none transition",
              "focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background",
              tone === toneOption && "ring-2 ring-foreground/70 ring-offset-2",
              toneOption === "neutral" ? "bg-[#34c759]" : "bg-foreground",
            )}
          />
        ))}
      </div>

      <div className=" gap-8 text-center">
        <div className="flex flex-col items-center gap-3">
          <AppleSwitch
            checked={active}
            onCheckedChange={setActive}
            aria-label="Active switch"
            size="md"
            tone={tone}
          />
        </div>
      </div>
    </div>
  );
};

export default AppleSwitchDemo;
export { AppleSwitchDemo };
```

#### Component Source Code

##### File: `registry/components/unlumen/apple-switch/index.tsx`

```tsx
"use client";

import { motion, useMotionValue, useSpring, useTransform } from "motion/react";
import { forwardRef, useEffect, useId, useRef, useState } from "react";
import { cn } from "@/lib/utils";

const switchSizes = {
  sm: {
    trackX: 46,
    trackY: 24,
    thumbX: 22,
    thumbY: 18,
    padding: 3,
  },
  md: {
    trackX: 62,
    trackY: 30,
    thumbX: 32,
    thumbY: 24,
    padding: 4,
  },
  lg: {
    trackX: 74,
    trackY: 36,
    thumbX: 34,
    thumbY: 28,
    padding: 5,
  },
} as const;

const switchTones = {
  neutral: {
    off: "color-mix(in srgb, var(--muted) 82%, transparent)",
    on: "#34c759",
    thumb: "#ffffff",
    glow: "color-mix(in srgb, #34c759 32%, transparent)",
  },
  accent: {
    off: "color-mix(in srgb, var(--muted) 82%, transparent)",
    on: "var(--foreground)",
    thumb: "#ffffff",
    glow: "color-mix(in srgb, var(--accent) 42%, transparent)",
  },
} as const;

const thumbSpring = {
  stiffness: 700,
  damping: 48,
  mass: 0.55,
};

const grabSpring = {
  stiffness: 500,
  damping: 25,
};

interface AppleSwitchProps
  extends Omit<
    React.ButtonHTMLAttributes<HTMLButtonElement>,
    "onChange" | "role"
  > {
  checked?: boolean;
  defaultChecked?: boolean;
  onCheckedChange?: (checked: boolean) => void;
  label?: React.ReactNode;
  description?: React.ReactNode;
  /** @default "md" */
  size?: keyof typeof switchSizes;
  /** @default "neutral" */
  tone?: keyof typeof switchTones;
  /** @default "right" */
  labelSide?: "left" | "right";
}

const clamp = (value: number, min: number, max: number) => {
  return Math.min(Math.max(value, min), max);
};

const AppleSwitch = forwardRef<HTMLButtonElement, AppleSwitchProps>(
  (
    {
      checked,
      onCheckedChange,
      label,
      description,
      size = "md",
      tone = "neutral",
      labelSide = "right",
      className,
      style,
      disabled,
      defaultChecked,
      id,
      type = "button",
      onClick,
      onPointerCancel,
      onPointerDown,
      onPointerLeave,
      onPointerMove,
      onPointerUp,
      ...props
    },
    ref,
  ) => {
    const generatedId = useId();
    const switchId = id ?? generatedId;
    const [uncontrolledChecked, setUncontrolledChecked] = useState(
      Boolean(defaultChecked),
    );
    const currentChecked = checked ?? uncontrolledChecked;
    const metrics = switchSizes[size];
    const colors = switchTones[tone];
    const thumbTravel = metrics.trackX - metrics.thumbX - metrics.padding * 2;
    const targetX = useMotionValue(currentChecked ? thumbTravel : 0);
    const thumbX = useSpring(targetX, thumbSpring);
    const grabTarget = useMotionValue(0);
    const grabProgress = useSpring(grabTarget, grabSpring);
    const thumbWidth = useTransform(
      grabProgress,
      [0, 1],
      [metrics.thumbX, metrics.thumbX + metrics.padding * 4.5],
    );
    const thumbHeight = useTransform(
      grabProgress,
      [0, 1],
      [metrics.thumbY, metrics.thumbY + metrics.padding * 2.3],
    );
    const thumbOffsetX = useTransform(
      () => thumbX.get() - (thumbWidth.get() - metrics.thumbX) / 2,
    );
    const liquidOpacity = useTransform(grabProgress, [0, 1], [0, 0.76]);
    const liquidScale = useTransform(grabProgress, [0, 1], [0.82, 1.08]);
    const thumbOpacity = useTransform(grabProgress, [0, 1], [1, 0.2]);
    const dragStartX = useRef(0);
    const dragStartThumbX = useRef(0);
    const isDragging = useRef(false);
    const activePointerId = useRef<number | null>(null);
    const suppressNextClick = useRef(false);
    const activeProgress = useTransform(thumbX, [0, thumbTravel], [0, 1]);
    const fillOpacity = useTransform(activeProgress, [0, 1], [0, 1]);
    const glowOpacity = useTransform(
      activeProgress,
      [0, 0.7, 1],
      [0, 0.18, 0.2],
    );
    const glowScale = useTransform(activeProgress, [0, 1], [0.82, 1]);

    useEffect(() => {
      if (activePointerId.current !== null) return;
      targetX.set(currentChecked ? thumbTravel : 0);
    }, [currentChecked, thumbTravel, targetX]);

    const setChecked = (next: boolean) => {
      if (next === currentChecked) {
        targetX.set(next ? thumbTravel : 0);
        return;
      }

      if (checked === undefined) {
        setUncontrolledChecked(next);
      }

      targetX.set(next ? thumbTravel : 0);
      onCheckedChange?.(next);
    };

    const handlePointerDown = (
      event: React.PointerEvent<HTMLButtonElement>,
    ) => {
      onPointerDown?.(event);
      if (event.defaultPrevented || disabled) return;
      if (event.pointerType === "mouse" && event.button !== 0) return;
      event.currentTarget.setPointerCapture(event.pointerId);
      activePointerId.current = event.pointerId;
      grabTarget.set(1);
      dragStartX.current = event.clientX;
      dragStartThumbX.current = thumbX.get();
      targetX.set(dragStartThumbX.current);
      isDragging.current = false;
    };

    const handlePointerMove = (
      event: React.PointerEvent<HTMLButtonElement>,
    ) => {
      onPointerMove?.(event);
      if (event.defaultPrevented || disabled) return;
      if (
        activePointerId.current !== null &&
        event.pointerId !== activePointerId.current
      ) {
        return;
      }
      if (!event.currentTarget.hasPointerCapture(event.pointerId)) return;

      const deltaX = event.clientX - dragStartX.current;

      if (Math.abs(deltaX) > 3) {
        isDragging.current = true;
      }

      if (!isDragging.current) return;
      event.preventDefault();

      const nextX = dragStartThumbX.current + deltaX;
      targetX.set(clamp(nextX, 0, thumbTravel));
    };

    const handlePointerUp = (event: React.PointerEvent<HTMLButtonElement>) => {
      onPointerUp?.(event);
      if (
        activePointerId.current !== null &&
        event.pointerId !== activePointerId.current
      ) {
        return;
      }
      if (event.currentTarget.hasPointerCapture(event.pointerId)) {
        event.currentTarget.releasePointerCapture(event.pointerId);
      }

      activePointerId.current = null;
      grabTarget.set(0);

      if (!isDragging.current) return;

      isDragging.current = false;
      suppressNextClick.current = true;
      setChecked(targetX.get() >= thumbTravel / 2);
    };

    const handlePointerCancel = (
      event: React.PointerEvent<HTMLButtonElement>,
    ) => {
      onPointerCancel?.(event);
      activePointerId.current = null;
      isDragging.current = false;
      grabTarget.set(0);
      targetX.set(currentChecked ? thumbTravel : 0);
    };

    const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
      onClick?.(event);
      if (event.defaultPrevented || disabled) return;

      if (suppressNextClick.current) {
        suppressNextClick.current = false;
        event.preventDefault();
        return;
      }

      setChecked(!currentChecked);
    };

    useEffect(() => {
      const stopFromWindow = () => {
        if (!isDragging.current && activePointerId.current === null) return;
        const wasDragging = isDragging.current;
        isDragging.current = false;
        activePointerId.current = null;
        grabTarget.set(0);
        if (!wasDragging) return;
        suppressNextClick.current = true;
        setChecked(targetX.get() >= thumbTravel / 2);
      };

      window.addEventListener("pointerup", stopFromWindow);
      window.addEventListener("pointercancel", stopFromWindow);
      window.addEventListener("blur", stopFromWindow);

      return () => {
        window.removeEventListener("pointerup", stopFromWindow);
        window.removeEventListener("pointercancel", stopFromWindow);
        window.removeEventListener("blur", stopFromWindow);
      };
    });

    const switchEl = (
      <button
        id={switchId}
        ref={ref}
        type={type}
        role="switch"
        aria-checked={currentChecked}
        disabled={disabled}
        onClick={handleClick}
        onPointerCancel={handlePointerCancel}
        onPointerDown={handlePointerDown}
        onPointerMove={handlePointerMove}
        onPointerUp={handlePointerUp}
        onPointerLeave={(event) => {
          onPointerLeave?.(event);
        }}
        aria-label={typeof label === "string" ? label : props["aria-label"]}
        className={cn(
          "relative inline-flex shrink-0 cursor-pointer items-center rounded-full active:cursor-grabbing",
          "border border-white/35 bg-white/10 shadow-inner backdrop-blur-md",
          "outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background",
          "disabled:cursor-not-allowed disabled:opacity-45",
          className,
        )}
        style={{
          width: metrics.trackX,
          height: metrics.trackY,
          touchAction: "pan-y",
          ...style,
        }}
        {...props}
      >
        <motion.span
          className="pointer-events-none absolute -inset-1 rounded-full blur-md"
          style={{
            backgroundColor: colors.glow,
            opacity: glowOpacity,
            scale: glowScale,
          }}
        />

        <span className="absolute inset-0 overflow-hidden rounded-full">
          <span
            className="absolute inset-0 rounded-full"
            style={{
              backgroundColor: colors.off,
              boxShadow:
                "inset 0 1px 1px rgba(255,255,255,0.34), inset 0 -1px 2px rgba(0,0,0,0.08)",
            }}
          />

          <motion.span
            className="absolute inset-0 rounded-full"
            style={{
              backgroundColor: colors.on,
              opacity: fillOpacity,
            }}
          />
        </span>

        <motion.span
          className="pointer-events-none absolute left-0 z-[9] block rounded-full"
          style={{
            width: thumbWidth,
            height: thumbHeight,
            x: thumbOffsetX,
            top: "50%",
            y: "-50%",
            marginLeft: metrics.padding,
            background:
              "color-mix(in srgb, var(--background) 82%, transparent)",
            opacity: liquidOpacity,
            scale: liquidScale,
            filter: "blur(9px)",
            backdropFilter: "blur(10px)",
            WebkitBackdropFilter: "blur(10px)",
          }}
        />

        <motion.span
          className="pointer-events-none z-10 block rounded-full"
          style={{
            width: thumbWidth,
            height: thumbHeight,
            x: thumbOffsetX,
            marginLeft: metrics.padding,
            backgroundColor: colors.thumb,
            opacity: thumbOpacity,
            backdropFilter: "blur(12px)",
            WebkitBackdropFilter: "blur(12px)",
            boxShadow:
              "0 3px 11px rgba(0,0,0,0.24), 0 1px 1px rgba(0,0,0,0.12), inset 0 1px 0 rgba(255,255,255,0.78), inset 0 -1px 1px rgba(0,0,0,0.05)",
          }}
        />
      </button>
    );

    if (!label) return switchEl;

    return (
      <label
        htmlFor={switchId}
        className={cn(
          "inline-flex cursor-pointer select-none items-center gap-3",
          disabled && "cursor-not-allowed opacity-50",
        )}
      >
        {labelSide === "left" && (
          <span className="flex flex-col gap-0.5 text-right">
            <span className="text-sm font-medium text-foreground">{label}</span>
            {description && (
              <span className="text-xs text-muted-foreground">
                {description}
              </span>
            )}
          </span>
        )}
        {switchEl}
        {labelSide === "right" && (
          <span className="flex flex-col gap-0.5">
            <span className="text-sm font-medium text-foreground">{label}</span>
            {description && (
              <span className="text-xs text-muted-foreground">
                {description}
              </span>
            )}
          </span>
        )}
      </label>
    );
  },
);

AppleSwitch.displayName = "AppleSwitch";

export { AppleSwitch, type AppleSwitchProps };
```

---

### Cursor Image Trail <a name="cursor-image-trail"></a>

A trail of images that follows the cursor with fade, scale, and rotation animations powered by Motion.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/cursor-image-trail.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { CursorImageTrail } from "@/components/unlumen-ui/cursor-image-trail";

const IMAGES = [
  "https://example.com/photo-1.jpg",
  "https://example.com/photo-2.jpg",
  "https://example.com/photo-3.jpg",
];

<CursorImageTrail images={IMAGES} className="h-[480px] w-full bg-neutral-950">
  <p className="text-white/30 select-none">Move your cursor</p>
</CursorImageTrail>;
```

#### Demo Code

```tsx
"use client";

import { CursorImageTrail } from "@/registry/components/unlumen/cursor-image-trail";

function Svg({ children }: { children: string }) {
  return (
    <span
      className="block w-full [&>svg]:h-auto [&>svg]:w-full"
      dangerouslySetInnerHTML={{ __html: children }}
    />
  );
}

const fire = `<svg width="114" height="110" viewBox="0 0 114 110" fill="none" xmlns="http://www.w3.org/2000/svg"><g clip-path="url(#cit-fire)"><path d="M82.2281 108.417L26.8392 106.903L2.3916 82.6722H18.4353L2.3916 51.6267L26.8392 62.2276L36.007 3.16547L65.0385 45.5691L81.8462 22.0956L77.2622 66.0137L105.53 51.6267L92.542 78.5076L112.406 75.1002L82.2281 108.417Z" fill="#FF5959"/><path d="M40.1991 108.087C30.5071 86.1754 34.0313 48.3148 44.1641 47.8302C54.2969 47.3456 49.8913 89.9678 45.9263 89.9678C41.9614 89.9678 57.8214 52.4837 67.073 57.1005C77.2057 62.157 57.3807 92.9174 56.059 92.9174C54.7374 92.9174 69.906 72.2713 75.8839 80.2762C80.2894 86.1754 63.989 101.766 63.5484 100.502C63.1079 99.2381 73.2406 93.7602 75.0028 97.1312C76.4126 99.828 72.8 105.559 67.0728 108.087" stroke="#FFD43E" stroke-width="2" stroke-linecap="square" stroke-linejoin="round"/><path d="M90.221 42.506C96.3376 40.5811 101.995 27.3775 96.5829 26.4706C90.7226 25.4886 95.0711 44.3303 108.911 34.5863" stroke="#FFD43E" stroke-width="2" stroke-linecap="square" stroke-linejoin="round"/><path d="M72.2378 7.35004L71.016 16.9039L76.4583 8.47263L72.2378 7.35004Z" fill="#FF4C4C"/><path d="M0.222565 72.2174L6.33408 73.3255L1.89686 68.2111L0.222565 72.2174Z" fill="#FF4C4C"/><path d="M7.7294 28.6059C10.2975 27.4497 17.074 26.6245 20.1652 35.5759" stroke="#FF4C4C" stroke-width="2" stroke-linecap="square" stroke-linejoin="round"/><path d="M61.3846 28.4892L60.5874 24.928L63.3776 24.5323L63.7762 28.4892H61.3846Z" fill="#FF4C4C"/><path d="M9.26516 3.34113L12.2774 4.56639L15.417 2.13627L15.4515 5.42498L19.9373 7.49096L15.132 8.74149L14.4768 12.7803L11.8247 9.30757L8.51847 9.52139L10.464 6.50768L9.26516 3.34113Z" fill="#FFD43E"/><path d="M108.16 17.2731L108.693 19.2825L111.165 19.9417L109.478 21.2325L110.146 24.3183L107.639 22.3537L105.296 23.5835L106.066 20.8875L104.675 19.2854L106.987 19.11L108.16 17.2731Z" fill="#FF4C4C"/></g><defs><clipPath id="cit-fire"><rect width="114" height="110" fill="white"/></clipPath></defs></svg>`;

const flag = `<svg width="81" height="86" viewBox="0 0 81 86" fill="none" xmlns="http://www.w3.org/2000/svg"><rect width="7" height="86" fill="#FFDB5C"/><path d="M7 0H81L51.1967 29L81 58H7V0Z" fill="#1AE69B"/><path fill-rule="evenodd" clip-rule="evenodd" d="M7 20H27V0H28V20L48 20V0H49V20H60.446L59.4183 21H49V41H63.5291L64.5568 42H49V58H48V42H28V58H27V42H7V41H27V21H7V20ZM48 41H28V21L48 21V41Z" fill="black"/><path d="M70 58H69V46.3234L70 47.2965V58Z" fill="black"/><path d="M70 0V10.7035L69 11.6766V0H70Z" fill="black"/></svg>`;

const book = `<svg width="100" height="100" viewBox="0 0 100 100" fill="none" xmlns="http://www.w3.org/2000/svg"><g clip-path="url(#cit-book)"><path d="M96.8567 56V86.1426H60.3714C59.7738 85.8112 58.1862 84.9582 56.2669 84.1846C54.3733 83.4214 52.0408 82.6866 50.0003 82.6865C47.9597 82.6865 45.6273 83.4214 43.7337 84.1846C41.814 84.9583 40.2256 85.8113 39.6389 86.1377C39.6335 86.1407 39.6297 86.1422 39.6282 86.1426H3.14285V56H96.8567Z" fill="#FFF3CC" stroke="#17191C" stroke-width="2"/><path d="M49.7141 17.177V75.0921L3.14285 61.3948V3.47977L49.7141 17.177Z" fill="#FFFAEB" stroke="#17191C" stroke-width="2"/><path d="M96.857 3.47977V61.3948L50.2857 75.0921V17.177L96.857 3.47977Z" fill="#FFF3CC" stroke="#17191C" stroke-width="2"/><path d="M99.2857 24.2857L99.2857 88.5715L60.4599 88.5715C60.4599 88.5715 57.0915 95.7143 50 95.7143C42.554 95.7143 39.5401 88.5715 39.5401 88.5715L0.714289 88.5714L0.714289 24.2857" stroke="#FF4C4C" stroke-width="4"/><path d="M47.8571 78.2143L7.85715 70M41.0714 80.7143L7.85715 78.2143M52.1429 78.2143L92.1429 70M58.9286 80.7143L92.1429 78.2143" stroke="#C1B89C" stroke-width="2"/><path d="M12.1429 14.2857L41.4286 22.5V25.3571L12.1429 17.1428V14.2857Z" fill="#17191C"/><path d="M12.1429 22.5L41.4286 30.7143V33.5714L12.1429 25.3571V22.5Z" fill="#17191C"/><path d="M12.1429 30.7143L41.4286 38.9286V41.7857L12.1429 33.5714V30.7143Z" fill="#17191C"/><path d="M12.1429 38.9286L41.4286 47.1428V50L12.1429 41.7857V38.9286Z" fill="#17191C"/><path d="M12.1429 47.1428L41.4286 55.3571V58.2142L12.1429 50V47.1428Z" fill="#17191C"/><path d="M87.8571 14.2857L58.5714 22.5V25.3571L87.8571 17.1428V14.2857Z" fill="#17191C"/><path d="M87.8571 22.5L58.5714 30.7143V33.5714L87.8571 25.3571V22.5Z" fill="#17191C"/><path d="M72.8571 35L58.5714 38.9285V41.7857L72.8571 37.8571V35Z" fill="#17191C"/></g><defs><clipPath id="cit-book"><rect width="100" height="100" fill="white"/></clipPath></defs></svg>`;

const magicWand = `<svg width="97" height="123" viewBox="0 0 97 123" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M25.9469 21.7842C47.9179 7.77081 69.6647 31.1264 63.4993 51.5625C58.1827 65.708 45.824 71.6809 33.2331 68.4954C14.1766 65.5759 3.97589 35.7975 25.9469 21.7842Z" fill="#FFFCDE"/><rect width="14.9731" height="94.519" transform="matrix(0.818834 -0.57403 0.57497 0.818175 28.3717 42.7875)" fill="#17191C"/><rect width="14.9747" height="23.632" transform="matrix(0.818843 -0.574017 0.574983 0.818165 28.3717 42.7882)" fill="#FFDB5C"/><rect opacity="0.3" width="3.20775" height="23.6296" transform="matrix(0.818833 -0.574031 0.574969 0.818176 31.1938 40.8113)" fill="#FF4C4C"/><rect opacity="0.3" width="3.20765" height="59.365" transform="matrix(0.818829 -0.574037 0.574962 0.81818 47.02 63.9655)" fill="white"/><path d="M11.6103 66.0577L24.0644 57.7347M13.0024 21.6215L22.6476 30.6861M50.491 13.2781L46.3357 25.4073" stroke="#353535" stroke-width="2"/><path d="M57.4559 33.2925L68.0788 24.4573" stroke="#B28EFF" stroke-width="5"/><path d="M31.472 64.5134L29.4786 71.6809" stroke="#FF6BB2" stroke-width="3"/><path d="M0.543372 40.6515C1.18502 36.1447 20.9307 40.2497 20.3213 43.9237C19.6183 48.163 -0.0982815 45.1583 0.543372 40.6515Z" fill="#FF5C5C"/><path d="M26.1985 13.2556C30.4056 11.5366 32.6106 7.51772 33.4964 0.727216C34.2692 7.12221 36.7136 10.4929 40.9851 12.6475C36.5279 15.6006 34.5905 19.4486 34.2286 26.313C33.2267 19.4244 31.6092 15.572 26.1985 13.2556Z" fill="#315EFF"/><ellipse cx="64.5614" cy="46.1964" rx="3.18935" ry="3.18557" fill="#1AE69B"/></svg>`;

const unknown = `<svg width="106" height="94" viewBox="0 0 106 94" fill="none" xmlns="http://www.w3.org/2000/svg"><g clip-path="url(#cit-unknown)"><path d="M22.5241 71.7127L33.9623 44.9464C43.2314 41.9367 47.1884 39.4954 54.7547 35L87.8688 66.3876C100.203 78.0789 106.37 83.9246 104.362 88.9623C102.354 94 93.8567 94 76.8619 94H37.237C26.2882 94 20.8137 94 18.4421 90.4074C16.0705 86.8148 18.2217 81.7808 22.5241 71.7127Z" fill="#89FAD0"/><path d="M57.6861 37.7786L54.7547 35C54.4955 35.154 54.2405 35.3056 53.9893 35.4549C46.911 39.6633 42.9138 42.0398 33.9623 44.9464L22.5241 71.7127C18.2217 81.7808 16.0705 86.8148 18.4421 90.4074C18.5333 90.5456 18.6291 90.6785 18.7297 90.8062C39.2327 77.8728 49.0775 66.4913 57.6861 37.7786Z" fill="#6BEABB"/><path d="M61.9866 94H51.0333C56.1287 83.5576 58.1153 74.3314 60.4207 63.6244C61.6597 57.8701 62.9908 51.6882 64.9461 44.66L72.4685 51.7902C70.4065 67.8348 68.1173 78.2396 61.9866 94Z" fill="#6BEABB"/><path d="M87.5564 93.9326C84.4526 94 80.8967 94 76.8619 94H75.3109C79.5335 80.7864 81.1536 72.38 82.0881 60.9083L87.8688 66.3876C89.1583 67.6099 90.3804 68.7683 91.5328 69.8685C90.5747 80.1316 89.5476 86.3421 87.5564 93.9326Z" fill="#6BEABB"/><path d="M33.2574 16.8379C43.5617 11.9164 53.5141 8.86164 61.2546 7.88046C65.1303 7.38918 68.3941 7.42503 70.85 7.96954C73.3207 8.51736 74.7972 9.53385 75.4267 10.8517C76.0562 12.1696 75.9187 13.9569 74.792 16.223C73.6719 18.4754 71.649 21.0373 68.8309 23.7431C63.2027 29.147 54.5715 34.9681 44.2673 39.8895C33.9633 44.8108 24.0114 47.8653 16.271 48.8466C12.3952 49.3378 9.13105 49.3011 6.67517 48.7566C4.20435 48.2087 2.72792 47.1924 2.09845 45.8744C1.46908 44.5564 1.60639 42.7692 2.73321 40.5032C3.85324 38.2508 5.87667 35.6896 8.69472 32.9839C14.3228 27.5801 22.9534 21.7593 33.2574 16.8379Z" fill="white" stroke="#2F3237" stroke-width="2"/><path d="M56.1174 13.1364C50.9174 2.24886 37.4228 -2.21683 26.6199 2.94281C15.817 8.10245 10.8089 21.4053 16.009 32.2928C29.0103 33.3677 48.487 24.7592 56.1174 13.1364Z" fill="#006FFF"/><ellipse cx="34.6921" cy="19.7377" rx="17.3112" ry="5.01038" transform="rotate(-30.4891 34.6921 19.7377)" fill="#002C66"/><path d="M26.9513 9.12398C19.4358 14.4922 19.0794 22.7236 20.8689 28.0919" stroke="white" stroke-width="2"/><path d="M69.0566 13.6895C65.2881 21.1119 52.3997 29.5914 47.4759 31.7576" stroke="#B9D6FC" stroke-width="2"/><path d="M33.8348 55.0765L34.2086 82.6176L9.17969 69.6886L33.8348 55.0765Z" fill="#FF9900"/><path d="M28.2083 58.4112L22.5241 71.7127C21.9134 73.1418 21.346 74.4696 20.8288 75.7061L34.2086 82.6176L33.8348 55.0765L28.2083 58.4112Z" fill="#FFB800"/><rect x="54.7821" y="55" width="20.2253" height="19.9584" transform="rotate(57.2303 54.7821 55)" fill="#1983E5"/><circle cx="79" cy="53" r="13" fill="#FF766D"/><path d="M67.448 47.0315L85.5774 64.2157C83.6479 65.3497 81.3999 66 79 66C71.8203 66 66 60.1797 66 53C66 50.8484 66.5227 48.8189 67.448 47.0315Z" fill="#D55CFF"/></g><defs><clipPath id="cit-unknown"><rect width="106" height="94" fill="white"/></clipPath></defs></svg>`;

const shapes = `<svg width="115" height="112" viewBox="0 0 115 112" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M56.0688 97.3429L62.1607 49.8717L108.402 52.3252L108.423 105.325L56.0688 97.3429Z" fill="#FFF176"/><path fill-rule="evenodd" clip-rule="evenodd" d="M96.6846 51.7035L73.0167 50.4478L75.1966 67.1232L59.6866 69.1507L56.619 93.0554L78.2174 90.232L79.6163 100.933L103.598 104.589L101.326 87.2112L108.415 86.2845L108.406 62.782L98.3054 64.1024L96.6846 51.7035ZM98.3054 64.1024L75.1966 67.1232L78.2174 90.232L101.326 87.2112L98.3054 64.1024Z" fill="#FF9DB9"/><path fill-rule="evenodd" clip-rule="evenodd" d="M93.5536 51.5375L62.1607 49.8718L61.6558 53.8064C67.3022 59.7237 71.5349 66.7937 76.1358 76.7286C80.5663 65.9404 86.0104 57.6616 93.5536 51.5375Z" fill="#FFFAEB"/><rect x="97.6248" y="6.16962" width="13.8429" height="30.5196" transform="rotate(36.2286 97.6248 6.16962)" fill="#2DE09E"/><path d="M111.557 42.1215C96.4929 36.8286 83.8714 21.7643 84.6857 2.6286C76.95 23.8 62.9644 35.2541 40.7143 38.8643C59.7249 49.0255 67.2424 57.5249 76.1357 76.7286C83.4922 58.8158 93.6429 47.8215 111.557 42.1215Z" fill="#FEE798"/><path fill-rule="evenodd" clip-rule="evenodd" d="M95.8701 31.9868C92.4921 28.4863 89.6848 24.338 87.7229 19.6844L79.5873 30.7886L90.7539 38.9699L95.8701 31.9868Z" fill="#FF4C4C"/><path d="M43.5645 40.0857C56.3249 40.0853 84.7885 20.868 86.0441 4.82151C86.2666 1.97783 83.6125 1.41637 82.2427 3.85016C79.3925 15.6573 92.6338 39.27 105.02 42.1216C114.947 44.4068 114.651 36.8624 105.021 38.9654C90.3499 42.1694 74.8121 59.2638 73.0972 75.5125C72.4389 81.7506 76.1361 80.3931 76.1361 76.7288C76.1361 64.6966 56.9999 34.3849 42.7438 36.5866C38.6789 37.2144 39.4932 40.0858 43.5645 40.0857Z" stroke="#002C66" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/><path d="M52.8412 14.183C66.4077 23.6394 66.8329 46.9338 55.302 59.2135C40.5031 74.9736 19.8973 75.2152 8.47525 64.195C-1.23706 54.8243 -4.3092 36.4825 8.47523 21.4458C19.4233 8.56907 39.9724 5.21294 52.8412 14.183Z" fill="#006FFF"/><path d="M57.4277 18.4674C56.3527 17.1395 55.1335 15.9296 53.7689 14.8668L51.1857 19.0896L55.4509 21.6988L57.4277 18.4674Z" fill="#00329C"/><path d="M12.0138 17.8856C13.6715 16.4546 15.4637 15.1909 17.3519 14.1006L16.7131 21.6509L11.7308 21.2294L12.0138 17.8856Z" fill="#00329C"/><path d="M0.409973 38.3552C0.689758 36.6417 1.11664 34.9154 1.70221 33.1917L7.75336 37.2896L4.94965 41.4296L0.409973 38.3552Z" fill="#00329C"/><path d="M3.30933 57.1969C2.3421 55.3235 1.56451 53.309 1.00671 51.1937L1.69757 50L6.02502 52.5045L3.30933 57.1969Z" fill="#00329C"/><path d="M63.5009 38.09C63.4719 39.1422 63.396 40.1951 63.2734 41.2432L60.3712 42.8132L54.1857 31.379L58.5834 29L63.5009 38.09Z" fill="#00329C"/><path d="M41.2309 12L44.5166 15.7688L34.7177 24.3117L31.4319 20.5429L41.2309 12Z" fill="#00329C"/><path d="M47.0859 39.1036L43.9105 42.9658L33.8688 34.7097L37.0442 30.8474L47.0859 39.1036Z" fill="#00329C"/><path d="M19.684 39.0225L24.3786 37.3018L19.9046 25.0959L15.21 26.8166L19.684 39.0225Z" fill="#00329C"/><path d="M53.5132 58.756L49.1857 56.2515L55.6976 45L60.025 47.5045L53.5132 58.756Z" fill="#00329C"/><path d="M62.9728 31.026C56.7159 34.889 49.3355 37.4655 40.7143 38.8643C49.6714 42.9357 55.1024 47.406 60.0871 52.2272C63.2586 45.7592 64.2505 38.0631 62.9728 31.026Z" fill="#FFBD3E"/><path d="M10.9926 52.7071C16.1164 39.7947 31.6106 37.5182 37.0503 46.5999C40.4118 52.212 42.0528 58.82 45.2532 64.5255C50.7975 74.4093 68.9771 75.7292 70.8431 86.9071C74.4567 108.554 48.4501 118.664 29.7211 106.857C12.7243 96.142 3.80541 70.8194 10.9926 52.7071Z" fill="#1AE69B"/><path d="M12.4554 85.6041C11.7413 83.9 11.1171 82.1587 10.5882 80.3951C14.1463 78.323 19.5253 75.1627 22.1857 73.5C25.3857 71.5 28.8524 67.6667 30.1857 66L39.6857 67C39.0191 67.8333 36.5857 70.4 32.1857 74C28.2236 77.2418 18.2879 82.6189 12.4554 85.6041Z" fill="#2285FF"/><path d="M24.9043 103.254C21.4868 100.263 18.5113 96.6529 16.0671 92.6654C20.9385 90.7934 33.5997 85.5542 39.6857 79C41.7813 76.7432 45.7332 72.1176 48.3323 68.3034C50.3148 70.1087 52.751 71.5973 55.283 72.997C53.284 78.9751 48.578 87.0776 41.6857 94.5C37.5012 99.0065 30.0139 101.872 24.9043 103.254Z" fill="#2285FF"/><path d="M65.3875 78.8951C68.1788 80.9956 70.2728 83.491 70.8431 86.9071C73.8628 104.996 56.1984 115.029 39.3813 110.949C45.8956 105.138 51.1848 99.0812 54.5485 95.2292C54.771 94.9744 54.9826 94.7322 55.1857 94.5C58.5241 90.6848 63.4715 83.8217 65.3875 78.8951Z" fill="#2285FF"/><path d="M58.9402 74.9682C64.5982 78.0109 69.8824 81.1527 70.8431 86.9072C71.6349 91.6506 71.0045 95.84 69.34 99.3663L56.0688 97.343L58.9402 74.9682Z" fill="#002C66"/><path d="M46.5439 66.4208C33.1952 74.6526 17.8551 73.1746 8.54877 64.2655C8.76191 60.2407 9.5551 56.3297 10.9926 52.7071C16.1164 39.7947 31.6106 37.5182 37.0503 46.5999C38.8136 49.5437 40.1035 52.7617 41.3934 55.9796C42.5625 58.8962 43.7316 61.8129 45.2532 64.5255C45.6286 65.1946 46.0618 65.8244 46.5439 66.4208Z" fill="#17191C"/><path d="M34.1857 43.418C33.5607 42.9522 32.8906 42.5573 32.1857 42.2327V64H8.56366C9.18976 64.8792 9.85944 65.4576 10.5551 66H32.1857V71.5878C32.8508 71.504 33.5178 71.3994 34.1857 71.2737V66H46.2162C45.8638 65.5299 45.5414 65.0392 45.2532 64.5255C45.1555 64.3512 45.0591 64.176 44.9641 64H34.1857V43.418Z" fill="#006EFF"/><path d="M30.1286 79.166C30.1286 85.1177 34.4396 96.3717 42.1037 95.3978C49.7677 94.4239 46.8937 83.224 45.6962 77.5428" stroke="white" stroke-width="2"/></svg>`;

const shield = `<svg width="101" height="109" viewBox="0 0 101 109" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M57.8244 54.5H101V64.7736C101 73.4307 96.826 81.5365 89.8319 86.4615L57.8244 109V54.5Z" fill="#1AE69B"/><path d="M91.4911 54.5H65.7914V78.8952H57.8244V104.848H63.7213L65.7914 103.39V78.8952H91.4911V85.1903C93.6583 83.3872 95.5021 81.2567 96.9728 78.8952H91.4911V54.5Z" fill="#0051FF"/><path d="M57.8244 14.0143H101V54.5H83.4998L83.9734 56.6266C84.7971 60.3263 82.0109 63.8429 78.2557 63.8429C74.5005 63.8429 71.7143 60.3263 72.5381 56.6266L73.0116 54.5H57.8244V40.7207L55.7185 41.1988C52.0548 42.0307 48.5725 39.2171 48.5725 35.425C48.5725 31.6329 52.0548 28.8193 55.7185 29.6512L57.8244 30.1293V14.0143Z" fill="#006FFF"/><path d="M57.8244 24.2597C64.213 22.0548 68.5132 18.9988 73.1739 14.0143H86.8692C78.1994 28.532 71.2762 38.3377 57.8244 42.5619V40.7207L55.7185 41.1988C52.0548 42.0307 48.5725 39.2171 48.5725 35.425C48.5725 31.6329 52.0548 28.8193 55.7185 29.6512L57.8244 30.1293V24.2597Z" fill="#FFCD00"/><path d="M101 36.7879V54.5H83.4998L83.9734 56.6266C84.7971 60.3263 82.0109 63.8429 78.2557 63.8429C74.5005 63.8429 71.7143 60.3263 72.5381 56.6266L73.0116 54.5H63.3978C66.9079 52.4126 70.0673 50.1724 73.1156 48.0109C81.5638 42.0204 89.1585 36.6352 101 36.7879Z" fill="#FFCD00"/><path d="M0 0H43.1756V13.8346L40.6849 13.2542C37.226 12.4481 33.9237 15.101 33.9237 18.6857C33.9237 22.2704 37.226 24.9233 40.6849 24.1172L43.1756 23.5368V39.7071H27.1627L27.7374 37.192C28.5356 33.6991 25.9086 30.3643 22.3588 30.3643C18.8089 30.3643 16.1819 33.6991 16.9801 37.192L17.5549 39.7071H0V0Z" fill="#FF4C4C"/><path d="M36.2367 45.9357C32.7143 45.9357 30.1318 49.2815 30.9917 52.7309L31.4328 54.5H15.4199V64.7736C15.4199 73.4308 19.5194 81.5365 26.3885 86.4615L57.8244 109V54.5H41.0405L41.4816 52.7309C42.3415 49.2815 39.759 45.9357 36.2367 45.9357Z" fill="#FFDB5C"/><path d="M13.3639 0H12.3359V16.0905H0V17.1286H12.3359V34.7762H0V35.8143H12.3359V39.7071H13.3639V35.8143H16.8375C16.8447 35.4608 16.8853 35.1135 16.9567 34.7762H13.3639V17.1286H30.8397V34.7762H27.7608C27.8322 35.1135 27.8729 35.4608 27.88 35.8143H30.8397V39.7071H31.8677V35.8143H43.1756V34.7762H31.8677V17.1286H34.1404C34.2445 16.7648 34.3842 16.4176 34.5549 16.0905H31.8677V0H30.8397V16.0905H13.3639V0Z" fill="black"/><path d="M41.6157 50.7953C41.3846 48.6792 39.9282 46.8606 37.9109 46.2005L37.0076 49.5145L41.6157 50.7953Z" fill="#46DC36"/><path d="M57.8244 68.8504L53.4555 71.018L55.6641 75.5574L57.8244 74.4856V68.8504Z" fill="#46DC36"/><path d="M57.8244 94.5592L53.7566 93.4286L52.4275 98.305L57.8244 99.8051V94.5592Z" fill="#46DC36"/><path d="M16.5734 72.6251L24.5097 76.875L26.8561 72.4066L15.7103 66.4381L15.4981 66.8422C15.6481 68.8215 16.0121 70.76 16.5734 72.6251Z" fill="#46DC36"/><path d="M43.8071 60.2095L46.3211 64.5839L35.4097 70.9788L32.8957 66.6044L43.8071 60.2095Z" fill="#46DC36"/><path d="M45.6067 77.8571L49.2715 81.3045L40.6724 90.6266L37.0076 87.1792L45.6067 77.8571Z" fill="#46DC36"/></svg>`;

const sandtimer = `<svg width="87" height="99" viewBox="0 0 87 99" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M86 1V25.1436L59.2549 48.75L58.4062 49.5L59.2549 50.25L86 73.8555V98H1V73.8555L27.7451 50.25L28.5938 49.5L27.7451 48.75L1 25.1436V1H86Z" fill="white" stroke="#006FFF" stroke-width="2"/><mask id="cit-st-mask" style="mask-type:alpha" maskUnits="userSpaceOnUse" x="0" y="0" width="87" height="99"><path d="M86.9999 0H0V25.5951L27.083 49.5L5.98144e-05 73.4049L5.69955e-05 99L87 99V73.4049L59.9171 49.5L86.9999 25.5951V0Z" fill="white"/></mask><g mask="url(#cit-st-mask)"><path d="M43 -6H71V22H43V-6Z" fill="#EFF7FF"/><path d="M71 22H99V50H71V22Z" fill="#EFF7FF"/><path d="M-13 -6H15V22H-13V-6Z" fill="#EFF7FF"/><path d="M15 22H43V50H15V22Z" fill="#EFF7FF"/><path d="M43 50H71V78H43V50Z" fill="#EFF7FF"/><path d="M71 78H99V106H71V78Z" fill="#EFF7FF"/><path d="M-13 50H15V78H-13V50Z" fill="#EFF7FF"/><path d="M15 78H43V106H15V78Z" fill="#EFF7FF"/><path d="M24 25.5119L41.3824 15L45.0047 20.9899L27.6224 31.5018L24 25.5119Z" fill="#E9F3FF"/><path d="M29 -0.748774L39.7258 -18L45.6704 -14.304L34.9447 2.94726L29 -0.748774Z" fill="#E9F3FF"/><path d="M13.8998 7.94172L-0.999994 -5.8656L3.75794 -11L18.6577 2.80732L13.8998 7.94172Z" fill="#E9F3FF"/><path d="M73.8998 8.94172L59 -4.8656L63.7579 -10L78.6577 3.80732L73.8998 8.94172Z" fill="#E9F3FF"/><path d="M69.9444 44.0811L54.0001 31.4944L58.3374 26L74.2817 38.5867L69.9444 44.0811Z" fill="#E9F3FF"/><path d="M46.7168 43.7164L35.932 60.9308L30 57.2144L40.7848 40L46.7168 43.7164Z" fill="#E9F3FF"/><path d="M7.12454 50L20.9626 64.8712L15.8381 69.6398L1.99999 54.7685L7.12454 50Z" fill="#E9F3FF"/><path d="M-4.62607 83L13.1723 92.791L9.79839 98.9242L-8 89.1332L-4.62607 83Z" fill="#E9F3FF"/><path d="M25 86.3347L42.4883 76L46.0496 82.0264L28.5613 92.3611L25 86.3347Z" fill="#E9F3FF"/><path d="M65.112 51L78.9893 65.8346L73.8774 70.6167L60 55.7821L65.112 51Z" fill="#E9F3FF"/><path d="M57.3875 84L75.1642 93.8302L71.7768 99.956L54 90.1258L57.3875 84Z" fill="#E9F3FF"/></g><circle cx="43.55" cy="29.6171" r="7.51357" fill="#FF9C1A"/><circle cx="25.0791" cy="15.52" r="5.32211" fill="#006FFF"/><circle cx="60.5136" cy="89.5136" r="7.51357" fill="#545963"/><rect x="61.6124" y="13.3376" width="15.6533" height="15.6533" transform="rotate(23.1986 61.6124 13.3376)" fill="#2DE09E"/><rect x="30" y="81.5684" width="15.6533" height="15.6533" fill="#006FFF"/><path d="M50.2107 39.6101L46.0047 57.5412L32.2797 43.8162L50.2107 39.6101Z" fill="#FF7B7B"/><path d="M33.707 40.1319L17.0002 32.3796L33.2216 21.7205L33.707 40.1319Z" fill="#FFDB5C"/><path d="M58.374 67.5371L52.3313 84.9353L40.1035 69.8611L58.374 67.5371Z" fill="#2DE09E"/><path d="M86 1V25.1436L59.2549 48.75L58.4062 49.5L59.2549 50.25L86 73.8555V98H1V73.8555L27.7451 50.25L28.5938 49.5L27.7451 48.75L1 25.1436V1H86Z" stroke="#006FFF" stroke-width="2"/></svg>`;

const brezel = `<svg width="104" height="91" viewBox="0 0 104 91" fill="none" xmlns="http://www.w3.org/2000/svg"><g clip-path="url(#cit-brezel)"><path fill-rule="evenodd" clip-rule="evenodd" d="M52 6.2263C73.1852 -4.31052 104 6.22629 102.054 39.7058C100.163 67.407 83.4137 84.5776 62.9084 88.4904C42.8467 92.3186 20.7479 83.0829 8.77975 61.9064C1.2557 48.5933 1.05705 33.3122 7.50569 21.5692C14.1578 9.45553 27.4199 2.04938 44.033 4.42358C46.3529 4.75512 49.992 5.40514 52 6.2263ZM62.6738 45.0981C63.5929 43.0808 64.3243 41.0203 64.8647 38.9408C66.5591 32.4213 66.3857 25.5202 63.8557 19.4471C75.1111 17.2421 85.6243 25.8865 84.7606 38.5375C84.4221 43.4963 83.4913 47.8486 82.1037 51.6312C73.019 50.7399 66.824 48.1826 62.6738 45.0981ZM38.7809 21.2374C36.6685 28.2564 37.0532 36.3119 40.0252 43.4741C40.3589 44.2784 40.7227 45.0712 41.1166 45.8515C37.2702 49.4601 31.8035 52.5927 24.3996 54.3358C24.2269 54.0465 24.0567 53.7535 23.8891 53.4568C19.0325 44.8636 19.38 35.9125 22.7174 29.8349C25.5354 24.7033 30.8011 20.9494 38.7809 21.2374ZM38.1586 67.9342C43.7009 65.5804 48.4489 62.5177 52.3727 58.9825C57.2186 62.5436 63.2344 65.3408 70.429 67.1032C67.0724 69.3845 63.3821 70.8436 59.6429 71.5571C52.6783 72.8861 45.0269 71.7487 38.1586 67.9342Z" fill="#B08361"/><path d="M37.2722 13.397L38.703 13.1999L38.3068 10.3534L36.876 10.5504C31.4468 11.2982 23.5828 13.9805 17.885 19.6903C12.0949 25.4926 8.68417 34.2884 12.0869 46.8322L12.4632 48.2195L15.2524 47.4708L14.876 46.0836C11.7307 34.4885 14.9002 26.7605 19.9353 21.7147C25.0629 16.5764 32.2693 14.086 37.2722 13.397Z" fill="#DEB290"/><path d="M84.9578 65.9655L85.7908 64.7916L83.4307 63.1344L82.5977 64.3083C71.5544 79.8706 44.2166 85.9859 24.1554 65.1021L23.1575 64.0633L21.0689 66.0486L22.0668 67.0874C43.413 89.3089 72.8901 82.9715 84.9578 65.9655Z" fill="#DEB290"/><path d="M48.6296 40.2316C45.2593 30.6526 47.5704 15.039 62.5926 11.9737C81.3704 8.14212 92.9259 19.6369 93.4074 36.4" stroke="#DEB290" stroke-width="3"/><path d="M71.5 67.0526C52.7222 62.1434 38.3982 48.7835 38.3982 32.5684C38.3982 9.97851 54.1783 1.20358 71.7407 2.39472C92.9259 3.83157 107.51 24.8983 100.63 48.8526M64.0371 19.6013C66.9259 27.3 65.2408 39.3616 62.1111 44.63M52.4815 57.4736C47.6667 61.7842 40.3241 66.5737 37.5556 68.0105M96.2963 62.2631C87.7068 79.3518 69.8645 90.865 50.6893 89.0309C40.2289 88.0304 31.0397 84.4165 23.5926 79.1077M52.4815 6.2341C50.1771 5.25869 47.3735 4.5583 44.2963 4.19747M40.4445 21.1556C18.7778 22.0316 16.3704 43.1052 24.5556 54.121C32.2593 52.2052 36.5926 50.2895 41.8571 45.9789M11.5437 67.0526C5.90489 59.0739 2.84387 49.6998 3.02734 40.2316M4.04913 31.8818C5.02287 27.6209 6.70192 23.4191 9.14816 19.3973C14.2858 9.84644 24.5842 5.31029 34.4764 4.19747M56.8148 36.8789C59.8226 47.2874 72.7037 50.2895 81.8519 51.2474C84.7407 43.5842 86.0219 36.1312 83.2963 28.7368C78 14.3684 50.5021 15.0336 56.8148 36.8789Z" stroke="#453531" stroke-width="2" stroke-linejoin="bevel"/><path d="M74.2141 61.5093L77.6265 59.7319L79.86 63.9749L76.4476 65.7523L74.2141 61.5093Z" fill="#FFFEDB"/><path d="M98.4613 30.5263L100.493 33.7818L96.4016 36.3075L94.3704 33.052L98.4613 30.5263Z" fill="#FFFEDB"/><path d="M21.2054 72.0927L19.6724 75.6078L15.2553 73.7016L16.7883 70.1866L21.2054 72.0927Z" fill="#FFFEDB"/><path d="M37.421 61.7813L33.5843 61.4421L34.0105 56.6714L37.8472 57.0106L37.421 61.7813Z" fill="#FFFEDB"/><path d="M14.5512 59.0762L12.5561 55.7986L16.6748 53.3179L18.6698 56.5955L14.5512 59.0762Z" fill="#FFFEDB"/><path d="M56.8012 83.6184L58.9268 80.4231L62.9421 83.0661L60.8165 86.2615L56.8012 83.6184Z" fill="#FFFEDB"/><path d="M44.2965 75.7452L47.9826 76.8572L46.5852 81.4406L42.8991 80.3285L44.2965 75.7452Z" fill="#FFFEDB"/><path d="M73.1852 76.6061L73.6287 72.8L78.4115 73.3515L77.968 77.1576L73.1852 76.6061Z" fill="#FFFEDB"/><path d="M56.1367 49.815L59.5624 51.5669L57.361 55.8264L53.9353 54.0746L56.1367 49.815Z" fill="#FFFEDB"/><path d="M26.0924 57.9635L29.5181 59.7153L27.3167 63.9749L23.8911 62.2231L26.0924 57.9635Z" fill="#FFFEDB"/><path d="M16.5287 31.4697L16.9897 35.2738L12.2095 35.8469L11.7485 32.0429L16.5287 31.4697Z" fill="#FFFEDB"/><path d="M5.74318 33.2086L6.20416 37.0126L1.42395 37.5858L0.962967 33.7818L5.74318 33.2086Z" fill="#FFFEDB"/><path d="M22.4281 20.3026L18.7025 21.2756L17.4798 16.6431L21.2054 15.6701L22.4281 20.3026Z" fill="#FFFEDB"/><path d="M49.5117 25.4237L45.6668 25.1943L45.9551 20.4134L49.8 20.6428L49.5117 25.4237Z" fill="#FFFEDB"/><path d="M38.449 35.3603L36.5926 32.003L40.8113 29.6947L42.6677 33.052L38.449 35.3603Z" fill="#FFFEDB"/><path d="M69.9294 52.5976L68.073 49.2404L72.2918 46.9321L74.1482 50.2893L69.9294 52.5976Z" fill="#FFFEDB"/><path d="M88.8512 44.376L88.5926 40.5531L93.3965 40.2316L93.6551 44.0545L88.8512 44.376Z" fill="#FFFEDB"/><path d="M83.2775 66.6511L86.1909 64.1446L89.3406 67.7671L86.4272 70.2736L83.2775 66.6511Z" fill="#FFFEDB"/><path d="M68.8519 5.4496L72.0365 7.60493L69.3281 11.5648L66.1434 9.40947L68.8519 5.4496Z" fill="#FFFEDB"/><path d="M27.2576 14.7643L26 11.1426L30.551 9.57894L31.8086 13.2006L27.2576 14.7643Z" fill="#FFFEDB"/><path d="M40.6176 5.4496L36.9409 4.30745L38.3761 -0.264282L42.0529 0.87787L40.6176 5.4496Z" fill="#FFFEDB"/><path d="M62.1368 9.16571L61.3063 12.9072L56.6048 11.8745L57.4353 8.13306L62.1368 9.16571Z" fill="#FFFEDB"/><path d="M30.8148 75.5374L34.226 73.7579L36.4622 77.9995L33.051 79.779L30.8148 75.5374Z" fill="#FFFEDB"/><path d="M51.7879 43.5169L55.1991 41.7374L57.4353 45.979L54.024 47.7585L51.7879 43.5169Z" fill="#FFFEDB"/><path d="M86.5534 50.2893L90.212 51.4879L88.7058 56.037L85.0473 54.8384L86.5534 50.2893Z" fill="#FFFEDB"/><path d="M100.721 53.5863L101.182 57.3903L96.4016 57.9635L95.9407 54.1595L100.721 53.5863Z" fill="#FFFEDB"/><path d="M89.5556 20.4134L92.9668 18.6339L95.203 22.8755L91.7917 24.655L89.5556 20.4134Z" fill="#FFFEDB"/><path d="M78.4115 11.8745L81.8227 10.095L84.0589 14.3366L80.6477 16.1161L78.4115 11.8745Z" fill="#FFFEDB"/></g><defs><clipPath id="cit-brezel"><rect width="104" height="91" fill="white"/></clipPath></defs></svg>`;

const ITEMS = [
  fire,
  flag,
  book,
  magicWand,
  unknown,
  shapes,
  shield,
  sandtimer,
  brezel,
].map((svg, i) => <Svg key={i}>{svg}</Svg>);

export default function CursorImageTrailDemo() {
  return (
    <CursorImageTrail
      items={ITEMS}
      itemSize={120}
      trailLength={10}
      spawnDistance={100}
      rotationRange={20}
      className="h-full w-full rounded-lg"
    />
  );
}
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `images` | `string[]` | `-` | Array of image URLs to cycle through in the trail. |
| `imageSize?` | `number` | `120` | Width and height of each trail image in pixels. |
| `trailLength?` | `number` | `8` | Maximum number of images simultaneously visible in the trail. |
| `spawnDistance?` | `number` | `80` | Minimum cursor travel in pixels before a new image is spawned. |
| `rotationRange?` | `number` | `20` | Maximum random rotation (±degrees) applied to each image. |
| `containerRef?` | `React.RefObject&lt;HTMLElement&gt;` | `-` | Optional ref to a container element. Defaults to tracking the whole window. |
| `className?` | `string` | `-` | Additional className for the outer wrapper div. |
| `children?` | `React.ReactNode` | `-` | Optional content rendered inside the trail container. |

#### Component Source Code

##### File: `registry/components/unlumen/cursor-image-trail/index.tsx`

```tsx
"use client";

import * as React from "react";
import { AnimatePresence, motion } from "motion/react";

import { cn } from "@/lib/utils";

export interface CursorImageTrailProps {
  items: React.ReactNode[];
  /** Size of each trail item in px. @default 120 */
  itemSize?: number;
  /** Max simultaneous items in the trail. @default 8 */
  trailLength?: number;
  /** Minimum cursor travel (px) before spawning a new item. @default 80 */
  spawnDistance?: number;
  /** Max random rotation applied to each item in degrees. @default 20 */
  rotationRange?: number;
  /** Render target — defaults to the whole window. */
  containerRef?: React.RefObject<HTMLElement>;
  className?: string;
  children?: React.ReactNode;
}

interface TrailItem {
  id: number;
  x: number;
  y: number;
  rotation: number;
  itemIndex: number;
}

let _id = 0;
const nextId = () => ++_id;

export function CursorImageTrail({
  items,
  itemSize = 120,
  trailLength = 8,
  spawnDistance = 80,
  rotationRange = 20,
  containerRef,
  className,
  children,
}: CursorImageTrailProps) {
  const [trail, setTrail] = React.useState<TrailItem[]>([]);
  const lastPos = React.useRef<{ x: number; y: number } | null>(null);
  const itemCounter = React.useRef(0);
  const containerElRef = React.useRef<HTMLDivElement>(null);

  React.useEffect(() => {
    const el = containerRef?.current ?? containerElRef.current ?? window;

    const onLeave = () => setTrail([]);

    const onMove = (e: Event) => {
      const mouseEvent = e as MouseEvent;
      const rect =
        containerRef?.current?.getBoundingClientRect() ??
        containerElRef.current?.getBoundingClientRect();

      const x = rect ? mouseEvent.clientX - rect.left : mouseEvent.clientX;
      const y = rect ? mouseEvent.clientY - rect.top : mouseEvent.clientY;

      if (lastPos.current) {
        const dx = x - lastPos.current.x;
        const dy = y - lastPos.current.y;
        const dist = Math.sqrt(dx * dx + dy * dy);
        if (dist < spawnDistance) return;
      }

      lastPos.current = { x, y };

      const rotation = (Math.random() * 2 - 1) * rotationRange;
      const itemIndex = itemCounter.current % items.length;
      itemCounter.current += 1;

      setTrail((prev) => {
        const next = [...prev, { id: nextId(), x, y, rotation, itemIndex }];
        return next.slice(-trailLength);
      });
    };

    el.addEventListener("mousemove", onMove);
    el.addEventListener("mouseleave", onLeave);
    return () => {
      el.removeEventListener("mousemove", onMove);
      el.removeEventListener("mouseleave", onLeave);
    };
  }, [items, spawnDistance, rotationRange, trailLength, containerRef]);

  const total = trail.length;

  return (
    <div
      ref={containerElRef}
      className={cn("relative overflow-hidden", className)}
    >
      {children}

      <AnimatePresence>
        {trail.map((item, i) => {
          const age = total - 1 - i;
          const scale = 0.6 + 0.4 * (1 - age / trailLength);

          return (
            <motion.div
              key={item.id}
              className="pointer-events-none absolute select-none"
              style={{
                left: item.x,
                top: item.y,
                width: itemSize,
                x: "-50%",
                y: "-50%",
                zIndex: i,
              }}
              initial={{
                opacity: 0,
                scale: 0.5,
                rotate: item.rotation * 1.5,
              }}
              animate={{
                opacity: 1,
                scale,
                rotate: item.rotation,
              }}
              exit={{
                opacity: 0,
                scale: 0.3,
                rotate: item.rotation * 0.5,
                filter: "blur(4px)",
              }}
              transition={{
                duration: 0.4,
                ease: [0.23, 1, 0.32, 1],
              }}
            >
              <div className="w-full [&>svg]:h-auto [&>svg]:w-full [&>img]:h-auto [&>img]:w-full">
                {items[item.itemIndex]}
              </div>
            </motion.div>
          );
        })}
      </AnimatePresence>
    </div>
  );
}
```

---

### Dia Text Reveal <a name="dia-text-reveal"></a>

Animated text reveal with a colorful gradient sweep effect, inspired by the Dia app.

- **Tier**: Pro
- **Category**: Animations

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { DiaTextReveal } from "@/components/unlumen-ui/dia-text-reveal";

<DiaTextReveal
  text="Build something beautiful."
  className="text-4xl font-bold"
/>;
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `text` | `string` | `-` | The text to animate. |
| `colors?` | `string[]` | `[&quot;#f0abfc&quot;, &quot;#f472b6&quot;, &quot;#fb923c&quot;, &quot;#facc15&quot;, &quot;#a3e635&quot;]` | Array of CSS color strings for the gradient stops. |
| `duration?` | `number` | `1.5` | Sweep animation duration in seconds. |
| `delay?` | `number` | `0` | Delay before animation starts in seconds. |
| `repeat?` | `boolean` | `false` | Whether to repeat the animation indefinitely. |
| `repeatDelay?` | `number` | `0.5` | Pause between repeats in seconds. |
| `startOnView?` | `boolean` | `true` | Whether to trigger the animation when the element enters the viewport. |
| `once?` | `boolean` | `true` | When startOnView is true, whether to only animate once. |
| `inViewMargin?` | `UseInViewOptions[&quot;margin&quot;]` | `-` | Margin for the viewport intersection observer (rootMargin). |
| `className?` | `string` | `-` | Extra classes applied to the span element. |

---

### Favicon Search <a name="favicon-search"></a>

A search input that fetches and smoothly animates the site favicon into the search bar as you type a URL.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/favicon-search.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### How to Use

```tsx
import { FaviconSearch } from "@/components/unlumen-ui/favicon-search";

<FaviconSearch
  placeholder="Enter a website URL…"
  onSearch={(value, domain) => console.log(value, domain)}
/>;
```

#### Demo Code

```tsx
"use client";

import * as React from "react";
import { motion, AnimatePresence } from "motion/react";
import { ArrowRight01Icon as ArrowRight } from "hugeicons-react";
import {
  FaviconSearch,
  extractDomain,
  getFaviconUrl,
} from "@/registry/components/unlumen/favicon-search";

export const FaviconSearchDemo = () => {
  const [value, setValue] = React.useState("");
  const [submitted, setSubmitted] = React.useState<string | null>(null);
  const [submittedDomain, setSubmittedDomain] = React.useState<string | null>(
    null,
  );

  const handleSearch = (val: string, domain: string | null) => {
    if (!domain) return;
    setSubmitted(val);
    setSubmittedDomain(domain);
  };

  const handleChange = (val: string) => {
    setValue(val);
    if (!val) {
      setSubmitted(null);
      setSubmittedDomain(null);
    }
  };

  return (
    <div className="flex min-h-[340px] w-full items-center justify-center p-8">
      <div className="w-full max-w-sm space-y-3">
        {/* Heading */}
        <div className="space-y-1">
          <h2 className="text-xl font-medium tracking-tight text-foreground">
            Enter your Website
          </h2>
        </div>

        {/* Search input */}
        <div className="space-y-2">
          <FaviconSearch
            value={value}
            onChange={handleChange}
            onSearch={handleSearch}
            placeholder="ui.shadcn.com"
            className="w-full"
          />
        </div>

        {/* Result card */}
        <AnimatePresence mode="wait">
          {submittedDomain && (
            <motion.div
              key={submittedDomain}
              initial={{ opacity: 0, y: 12, scale: 0.97 }}
              animate={{ opacity: 1, y: 0, scale: 1 }}
              exit={{ opacity: 0, y: -8, scale: 0.97 }}
              transition={{ type: "spring", stiffness: 360, damping: 28 }}
              className="rounded-xl border border-border bg-muted/40 p-4 flex items-center gap-3"
            >
              <motion.img
                src={getFaviconUrl(submittedDomain, 64)}
                alt={submittedDomain}
                width={36}
                height={36}
                className="size-9 rounded-lg object-contain shrink-0 shadow-sm"
                initial={{ scale: 0.6, opacity: 0, rotate: -8 }}
                animate={{ scale: 1, opacity: 1, rotate: 0 }}
                transition={{
                  type: "spring",
                  stiffness: 400,
                  damping: 24,
                  delay: 0.05,
                }}
              />
              <div className="flex-1 min-w-0">
                <p className="text-sm font-medium text-foreground truncate">
                  {submittedDomain}
                </p>
                <p className="text-xs text-muted-foreground mt-0.5 truncate">
                  {submitted}
                </p>
              </div>
              <ArrowRight className="size-4 text-muted-foreground shrink-0" />
            </motion.div>
          )}
        </AnimatePresence>
      </div>
    </div>
  );
};
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `value?` | `string` | `-` | Controlled input value. |
| `defaultValue?` | `string` | `&quot;&quot;` | Default value for uncontrolled mode. |
| `onChange?` | `(value: string) =&gt; void` | `-` | Called on every keystroke with the current input value. |
| `onSearch?` | `(value: string, domain: string \| null) =&gt; void` | `-` | Called when the user presses Enter. |
| `placeholder?` | `string` | `&quot;Enter a website URL…&quot;` | Input placeholder text. |
| `clearable?` | `boolean` | `true` | Show a clear (×) button when the input has content. |
| `faviconSize?` | `16 \| 32 \| 64 \| 128` | `64` | Favicon resolution requested from the Google favicon API. |
| `debounce?` | `number` | `350` | Milliseconds to wait after typing before resolving the domain. |
| `className?` | `string` | `-` | Extra className on the wrapper element. |
| `inputClassName?` | `string` | `-` | Extra className on the <input> element. |

#### Component Source Code

##### File: `registry/components/unlumen/favicon-search/index.tsx`

```tsx
"use client";

import * as React from "react";
import { motion, AnimatePresence } from "motion/react";
import { Globe, Search, X } from "lucide-react";
import { cn } from "@/lib/utils";

function extractDomain(input: string): string | null {
  if (!input.trim()) return null;
  try {
    const raw = input.includes("://") ? input : `https://${input}`;
    const url = new URL(raw);
    const host = url.hostname.replace(/^www\./, "");
    if (host.includes(".") && host.split(".").every(Boolean)) return host;
    return null;
  } catch {
    const cleaned = input.trim().replace(/^www\./, "");
    if (cleaned.includes(".") && cleaned.split(".").every(Boolean))
      return cleaned;
    return null;
  }
}

function getFaviconUrl(domain: string, size = 64): string {
  return `https://www.google.com/s2/favicons?domain=${encodeURIComponent(domain)}&sz=${size}`;
}

export interface FaviconSearchProps {
  /** Controlled value */
  value?: string;
  defaultValue?: string;
  onChange?: (value: string) => void;
  onSearch?: (value: string, domain: string | null) => void;
  placeholder?: string;
  clearable?: boolean;
  /** @default 64 */
  faviconSize?: 16 | 32 | 64 | 128;
  /** @default 350 */
  debounce?: number;
  className?: string;
  inputClassName?: string;
}

const FaviconSearch = React.forwardRef<HTMLInputElement, FaviconSearchProps>(
  (
    {
      value: controlledValue,
      defaultValue = "",
      onChange,
      onSearch,
      placeholder = "Enter a website URL…",
      clearable = true,
      faviconSize = 64,
      debounce = 350,
      className,
      inputClassName,
    },
    ref,
  ) => {
    const isControlled = controlledValue !== undefined;
    const [internalValue, setInternalValue] = React.useState(defaultValue);
    const value = isControlled ? controlledValue : internalValue;

    const [domain, setDomain] = React.useState<string | null>(null);
    const [faviconReady, setFaviconReady] = React.useState(false);
    const [faviconError, setFaviconError] = React.useState(false);
    const prevDomainRef = React.useRef<string | null>(null);

    React.useEffect(() => {
      const id = setTimeout(() => {
        const d = extractDomain(value);
        if (d !== prevDomainRef.current) {
          prevDomainRef.current = d;
          setFaviconReady(false);
          setFaviconError(false);
          setDomain(d);
        }
      }, debounce);
      return () => clearTimeout(id);
    }, [value, debounce]);

    const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
      const v = e.target.value;
      if (!isControlled) setInternalValue(v);
      onChange?.(v);
    };

    const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>) => {
      if (e.key === "Enter") {
        onSearch?.(value, domain);
      }
    };

    const handleClear = () => {
      if (!isControlled) setInternalValue("");
      onChange?.("");
      setDomain(null);
      setFaviconReady(false);
      setFaviconError(false);
      prevDomainRef.current = null;
    };

    const showFavicon = domain && faviconReady && !faviconError;

    return (
      <div
        className={cn(
          "relative flex items-center w-full max-w-md group",
          className,
        )}
      >
        <div className="pointer-events-none absolute left-3.5 flex items-center justify-center size-5">
          <AnimatePresence mode="wait">
            {showFavicon ? (
              <motion.img
                key={`favicon-${domain}`}
                src={getFaviconUrl(domain, faviconSize)}
                alt={domain}
                width={20}
                height={20}
                className="size-5 rounded-sm object-contain"
                initial={{ opacity: 0, scale: 0.5, filter: "blur(4px)" }}
                animate={{ opacity: 1, scale: 1, filter: "blur(0px)" }}
                exit={{ opacity: 0, scale: 0.5, filter: "blur(4px)" }}
                transition={{ type: "spring", stiffness: 400, damping: 28 }}
                onLoad={() => setFaviconReady(true)}
                onError={() => setFaviconError(true)}
              />
            ) : (
              <motion.span
                key="search-icon"
                initial={{ opacity: 0, scale: 0.7 }}
                animate={{ opacity: 1, scale: 1 }}
                exit={{ opacity: 0, scale: 0.7 }}
                transition={{ type: "spring", stiffness: 400, damping: 28 }}
                className="flex items-center justify-center text-muted-foreground"
              >
                {domain && !faviconError ? (
                  <Globe className="size-[18px]" />
                ) : (
                  <Search className="size-[18px]" />
                )}
              </motion.span>
            )}
          </AnimatePresence>

          {/* preload img to detect load/error before showing the animated favicon */}
          {domain && !faviconReady && !faviconError && (
            <img
              src={getFaviconUrl(domain, faviconSize)}
              alt=""
              className="sr-only absolute"
              onLoad={() => setFaviconReady(true)}
              onError={() => setFaviconError(true)}
              aria-hidden
            />
          )}
        </div>

        <input
          ref={ref}
          type="text"
          value={value}
          onChange={handleChange}
          onKeyDown={handleKeyDown}
          placeholder={placeholder}
          className={cn(
            "flex w-full rounded-xl border border-border bg-background",
            "pl-10 pr-10 py-2.5 text-sm text-foreground",
            "placeholder:text-muted-foreground",
            "outline-none ring-offset-background",
            "transition-shadow duration-200",
            "focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2",
            "hover:border-muted-foreground/50",
            inputClassName,
          )}
        />

        <AnimatePresence>
          {clearable && value.length > 0 && (
            <motion.button
              type="button"
              onClick={handleClear}
              initial={{ opacity: 0, scale: 0.7 }}
              animate={{ opacity: 1, scale: 1 }}
              exit={{ opacity: 0, scale: 0.7 }}
              transition={{ type: "spring", stiffness: 400, damping: 28 }}
              className={cn(
                "absolute right-3 flex items-center justify-center",
                "size-5 rounded-full text-muted-foreground",
                "hover:text-foreground hover:bg-muted transition-colors",
              )}
              aria-label="Clear input"
            >
              <X className="size-3.5" />
            </motion.button>
          )}
        </AnimatePresence>
      </div>
    );
  },
);
FaviconSearch.displayName = "FaviconSearch";

export { FaviconSearch, extractDomain, getFaviconUrl };
```

---

### File Tree <a name="file-tree"></a>

Animated file-tree component with spring folder expand/collapse, animated hover highlight, and automatic file-type icons.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/file-tree.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### How to Use

```tsx
import {
  FileTree,
  type FileTreeElement,
} from "@/components/unlumen-ui/file-tree";

const elements: FileTreeElement[] = [
  {
    id: "src",
    name: "src",
    type: "folder",
    children: [
      {
        id: "components",
        name: "components",
        type: "folder",
        children: [
          { id: "button", name: "button.tsx", highlight: true },
          { id: "card", name: "card.tsx", highlight: true },
        ],
      },
      { id: "page", name: "page.tsx" },
    ],
  },
  { id: "package-json", name: "package.json" },
];

export default function Example() {
  return <FileTree elements={elements} defaultOpenIds={["src"]} />;
}
```

#### Demo Code

```tsx
"use client";

import {
  FileTree,
  type FileTreeElement,
} from "@/registry/components/unlumen/file-tree";

const elements: FileTreeElement[] = [
  {
    id: "src",
    type: "folder",
    name: "src",
    children: [
      {
        id: "components",
        type: "folder",
        name: "components",
        children: [
          {
            id: "ui",
            type: "folder",
            name: "ui",
            children: [
              { id: "button", name: "button.tsx", highlight: true },
              { id: "card", name: "card.tsx", highlight: true },
              { id: "modal", name: "modal.tsx" },
              { id: "input", name: "input.tsx" },
            ],
          },
          {
            id: "layout",
            type: "folder",
            name: "layout",
            children: [
              { id: "header", name: "header.tsx" },
              { id: "footer", name: "footer.tsx" },
              { id: "sidebar", name: "sidebar.tsx" },
            ],
          },
        ],
      },
      {
        id: "app",
        type: "folder",
        name: "app",
        children: [
          { id: "layout", name: "layout.tsx", highlight: true },
          { id: "page", name: "page.tsx", highlight: true },
          {
            id: "api",
            type: "folder",
            name: "api",
            children: [
              {
                id: "users",
                type: "folder",
                name: "users",
                children: [{ id: "route", name: "route.ts" }],
              },
              {
                id: "posts",
                type: "folder",
                name: "posts",
                children: [{ id: "route", name: "route.ts" }],
              },
            ],
          },
        ],
      },
      {
        id: "lib",
        type: "folder",
        name: "lib",
        children: [
          { id: "utils", name: "utils.ts" },
          {
            id: "hooks",
            type: "folder",
            name: "hooks",
            children: [
              { id: "useAuth", name: "useAuth.ts" },
              { id: "useTheme", name: "useTheme.ts" },
            ],
          },
          {
            id: "services",
            type: "folder",
            name: "services",
            children: [
              { id: "api", name: "api.ts", highlight: true },
              { id: "auth", name: "auth.ts" },
            ],
          },
        ],
      },
      {
        id: "styles",
        type: "folder",
        name: "styles",
        children: [
          { id: "globals", name: "globals.css" },
          { id: "theme", name: "theme.css" },
        ],
      },
    ],
  },
  {
    id: "public",
    type: "folder",
    name: "public",
    children: [
      { id: "favicon", name: "favicon.ico" },
      {
        id: "images",
        type: "folder",
        name: "images",
        children: [
          { id: "logo", name: "logo.png" },
          { id: "banner", name: "banner.jpg" },
        ],
      },
    ],
  },
  {
    id: "config",
    type: "folder",
    name: "config",
    children: [
      { id: "package-json", name: "package.json" },
      { id: "tsconfig", name: "tsconfig.json" },
      { id: "nextconfig", name: "next.config.js" },
    ],
  },
  { id: "readme", name: "README.md" },
];

interface FileTreeDemoProps {
  highlightColor?: string;
  indentSize?: number;
  showIcons?: boolean;
}

export function FileTreeDemo({
  highlightColor,
  indentSize,
  showIcons,
}: FileTreeDemoProps) {
  return (
    <div className="w-64">
      <FileTree
        elements={elements}
        defaultOpenIds={["src", "app"]}
        highlightColor={highlightColor}
        indentSize={indentSize}
        showIcons={showIcons}
      />
    </div>
  );
}

export default FileTreeDemo;
```

#### Props

##### `FileTree` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `elements` | `FileTreeElement[]` | `-` | Tree nodes rendered by the component. |
| `className?` | `string` | `-` | Additional classes applied to the root element. |
| `highlightColor?` | `string` | `&quot;#f472b6&quot;` | Text color used for nodes marked with `highlight: true`. |
| `indentSize?` | `number` | `24` | Horizontal indent in pixels for nested folder content. |
| `showIcons?` | `boolean` | `true` | Whether file and folder icons are rendered. |
| `defaultOpenIds?` | `string[]` | `[]` | Folder ids that should be open on first render. |

##### `FileTreeElement` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `id` | `string` | `-` | Stable unique id used as the React key. |
| `name` | `string` | `-` | Label shown for the file or folder. |
| `type?` | `&quot;folder&quot; \| &quot;file&quot;` | `-` | Node type. Use `"folder"` for collapsible branches; omit it for files. |
| `children?` | `FileTreeElement[]` | `-` | Nested nodes rendered inside a folder. |
| `icon?` | `React.ComponentType&lt;{ className?: string }&gt;` | `-` | Custom icon component for file nodes. |
| `highlight?` | `boolean` | `-` | Marks the node as visually highlighted. |
| `defaultOpen?` | `boolean` | `-` | Whether a folder starts expanded. |

#### Component Source Code

##### File: `registry/components/unlumen/file-tree/index.tsx`

```tsx
"use client";

import * as React from "react";
import {
  Folder,
  FolderOpen,
  File,
  FileText,
  FileCode,
  FileJson,
  FileImage,
  FileCog,
} from "lucide-react";
import { AnimatePresence, motion } from "motion/react";
import { cn } from "@/lib/utils";

// ─── Types ─────────────────────────────────────────────────────────────────────

export type FileTreeElement = {
  id: string;
  name: string;
  /** Omit or set to "file" for a leaf node; "folder" renders a collapsible branch. */
  type?: "folder" | "file";
  children?: FileTreeElement[];
  /** Custom icon component (receives a `className` prop). */
  icon?: React.ComponentType<{ className?: string }>;
  /** Pink-tints the item to mark it as newly added / relevant. */
  highlight?: boolean;
  /** Whether this folder starts expanded. */
  defaultOpen?: boolean;
};

// ─── Context ───────────────────────────────────────────────────────────────────

type FileTreeCtx = {
  highlightColor: string;
  indentSize: number;
  showIcons: boolean;
  defaultOpenIds: Set<string>;
  containerRef: React.RefObject<HTMLDivElement | null>;
  highlightBounds: HighlightBounds | null;
  setHighlightBounds: React.Dispatch<
    React.SetStateAction<HighlightBounds | null>
  >;
};

type HighlightBounds = {
  top: number;
  left: number;
  width: number;
  height: number;
};

const FileTreeContext = React.createContext<FileTreeCtx | null>(null);

function useFileTree() {
  const context = React.useContext(FileTreeContext);
  if (!context) {
    throw new Error("File tree components must be used within <FileTree />");
  }
  return context;
}

type FolderCtx = {
  isOpen: boolean;
  toggle: () => void;
};

const FolderContext = React.createContext<FolderCtx | null>(null);

function useFolder() {
  const context = React.useContext(FolderContext);
  if (!context) {
    throw new Error("Folder components must be used within a folder item");
  }
  return context;
}

// ─── Icon resolution ───────────────────────────────────────────────────────────

const EXT_ICONS: Record<string, React.ComponentType<{ className?: string }>> = {
  tsx: FileCode,
  ts: FileCode,
  jsx: FileCode,
  js: FileCode,
  json: FileJson,
  md: FileText,
  mdx: FileText,
  png: FileImage,
  jpg: FileImage,
  jpeg: FileImage,
  svg: FileImage,
  webp: FileImage,
  config: FileCog,
  toml: FileCog,
  yaml: FileCog,
  yml: FileCog,
  env: FileCog,
};

function resolveFileIcon(
  name: string,
  custom?: React.ComponentType<{ className?: string }>,
): React.ComponentType<{ className?: string }> {
  if (custom) return custom;
  const ext = name.split(".").pop()?.toLowerCase() ?? "";
  return EXT_ICONS[ext] ?? File;
}

// ─── Shared highlight/collapse pieces ──────────────────────────────────────────

function FileTreeHoverHighlight({ className }: { className?: string }) {
  const { highlightBounds } = useFileTree();

  return (
    <AnimatePresence>
      {highlightBounds && (
        <motion.div
          className={className}
          initial={{ opacity: 0 }}
          animate={{
            opacity: 1,
            top: highlightBounds.top,
            left: highlightBounds.left,
            width: highlightBounds.width,
            height: highlightBounds.height,
          }}
          exit={{ opacity: 0 }}
          transition={{ type: "spring", stiffness: 500, damping: 40 }}
          style={{ position: "absolute", pointerEvents: "none", zIndex: 0 }}
        />
      )}
    </AnimatePresence>
  );
}

function useHighlightTarget() {
  const { containerRef, setHighlightBounds } = useFileTree();
  const ref = React.useRef<HTMLDivElement>(null);

  const onMouseEnter = React.useCallback(() => {
    const element = ref.current;
    const container = containerRef.current;
    if (!element || !container) return;

    const containerRect = container.getBoundingClientRect();
    const elementRect = element.getBoundingClientRect();

    setHighlightBounds({
      top: elementRect.top - containerRect.top,
      left: elementRect.left - containerRect.left,
      width: elementRect.width,
      height: elementRect.height,
    });
  }, [containerRef, setHighlightBounds]);

  return { ref, onMouseEnter };
}

function FolderIcon({
  closeIcon,
  openIcon,
}: {
  closeIcon: React.ReactNode;
  openIcon: React.ReactNode;
}) {
  const { isOpen } = useFolder();

  return (
    <span className="inline-flex shrink-0 relative size-[1.125rem]">
      <AnimatePresence initial={false} mode="popLayout">
        <motion.span
          key={isOpen ? "open" : "close"}
          className="inline-flex"
          initial={{ scale: 0.5, opacity: 0, rotate: -15 }}
          animate={{ scale: 1, opacity: 1, rotate: 0 }}
          exit={{ scale: 0.5, opacity: 0, rotate: 15 }}
          transition={{
            type: "spring",
            stiffness: 500,
            damping: 30,
            mass: 0.8,
          }}
        >
          {isOpen ? openIcon : closeIcon}
        </motion.span>
      </AnimatePresence>
    </span>
  );
}

function FolderContent({ children }: { children: React.ReactNode }) {
  const { isOpen } = useFolder();

  return (
    <AnimatePresence initial={false}>
      {isOpen && (
        <motion.div
          initial={{ height: 0, opacity: 0 }}
          animate={{ height: "auto", opacity: 1 }}
          exit={{ height: 0, opacity: 0 }}
          transition={{ type: "spring", stiffness: 500, damping: 40 }}
          style={{ overflow: "hidden" }}
        >
          {children}
        </motion.div>
      )}
    </AnimatePresence>
  );
}

// ─── Node renderers ────────────────────────────────────────────────────────────

function FileTreeFile({ node }: { node: FileTreeElement }) {
  const { highlightColor, showIcons } = useFileTree();
  const Icon = resolveFileIcon(node.name, node.icon);
  const highlightTarget = useHighlightTarget();

  return (
    <div
      ref={highlightTarget.ref}
      className="relative z-10"
      onMouseEnter={highlightTarget.onMouseEnter}
    >
      <div
        className="flex items-center gap-2 p-2 pointer-events-none"
        style={node.highlight ? { color: highlightColor } : undefined}
      >
        {showIcons && (
          <span className="inline-flex shrink-0">
            <Icon className="size-4.5" />
          </span>
        )}
        <span className="text-sm">{node.name}</span>
      </div>
    </div>
  );
}

function FileTreeFolder({ node }: { node: FileTreeElement }) {
  const { defaultOpenIds, highlightColor, indentSize, showIcons } =
    useFileTree();
  const highlightTarget = useHighlightTarget();
  const [isOpen, setIsOpen] = React.useState(
    node.defaultOpen ?? defaultOpenIds.has(node.id),
  );
  const toggle = React.useCallback(() => setIsOpen((open) => !open), []);

  return (
    <FolderContext.Provider value={{ isOpen, toggle }}>
      <div data-value={node.id} className="relative z-10">
        <button type="button" className="w-full text-start" onClick={toggle}>
          <div
            ref={highlightTarget.ref}
            onMouseEnter={highlightTarget.onMouseEnter}
          >
            <div className="flex items-center gap-2 p-2 pointer-events-none">
              {showIcons && (
                <FolderIcon
                  closeIcon={<Folder className="size-4.5" />}
                  openIcon={<FolderOpen className="size-4.5" />}
                />
              )}
              <span
                className="text-sm"
                style={node.highlight ? { color: highlightColor } : undefined}
              >
                {node.name}
              </span>
            </div>
          </div>
        </button>
        <div
          className="relative ml-6 before:absolute before:-left-2 before:inset-y-0 before:w-px before:h-full before:bg-border"
          style={indentSize !== 24 ? { marginLeft: indentSize } : undefined}
        >
          <FolderContent>
            {(node.children ?? []).map((child) => (
              <FileTreeNode key={child.id} node={child} />
            ))}
          </FolderContent>
        </div>
      </div>
    </FolderContext.Provider>
  );
}

function FileTreeNode({ node }: { node: FileTreeElement }) {
  if (node.type === "folder") {
    return <FileTreeFolder node={node} />;
  }
  return <FileTreeFile node={node} />;
}

// ─── Public API ────────────────────────────────────────────────────────────────

export type FileTreeProps = {
  elements: FileTreeElement[];
  className?: string;
  /** Highlight color for items with `highlight: true`. Defaults to pink (#f472b6). */
  highlightColor?: string;
  /** Horizontal indent per nesting level in px. Defaults to 24. */
  indentSize?: number;
  /** Whether to show file/folder icons. Defaults to true. */
  showIcons?: boolean;
  /** Folder ids that should be open on first render. */
  defaultOpenIds?: string[];
};

export function FileTree({
  elements,
  className,
  highlightColor = "#f472b6",
  indentSize = 24,
  showIcons = true,
  defaultOpenIds = [],
}: FileTreeProps) {
  const containerRef = React.useRef<HTMLDivElement>(null);
  const [highlightBounds, setHighlightBounds] =
    React.useState<HighlightBounds | null>(null);
  const defaultOpenIdSet = React.useMemo(
    () => new Set(defaultOpenIds),
    [defaultOpenIds],
  );

  return (
    <FileTreeContext.Provider
      value={{
        highlightColor,
        indentSize,
        showIcons,
        defaultOpenIds: defaultOpenIdSet,
        containerRef,
        highlightBounds,
        setHighlightBounds,
      }}
    >
      <div
        className={cn(
          "rounded-xl border border-border/60 overflow-hidden",
          className,
        )}
      >
        <div
          ref={containerRef}
          className="p-2 w-full relative isolate"
          onMouseLeave={() => setHighlightBounds(null)}
        >
          <FileTreeHoverHighlight className="rounded-lg border bg-accent/55 border-accent/45 z-0" />
          {elements.map((node) => (
            <FileTreeNode key={node.id} node={node} />
          ))}
        </div>
      </div>
    </FileTreeContext.Provider>
  );
}
```

---

### Gooey SVG Filter <a name="gooey-svg-filter"></a>

An SVG filter that creates a gooey, fluid merging effect on adjacent elements using Gaussian blur and alpha-channel color matrix manipulation.

- **Tier**: Pro
- **Category**: Animations

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import GooeySvgFilter from "@/components/unlumen-ui/gooey-svg-filter";

export default function Example() {
  return (
    <div className="relative">
      {/* Render the filter once anywhere in the tree — it's invisible */}
      <GooeySvgFilter id="gooey-filter" strength={15} />

      {/* Apply it to the container that holds the animating blobs */}
      <div style={{ filter: "url(#gooey-filter)" }}>
        <div className="w-12 h-12 rounded-full bg-black" />
        <div className="w-12 h-12 rounded-full bg-black" />
      </div>
    </div>
  );
}
```

---

### Hover Expand <a name="hover-expand"></a>

A vertical list of rows that expand on hover to reveal a full-width photograph, animated with Motion spring physics.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/hover-expand.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { HoverExpand } from "@/components/unlumen-ui/hover-expand";

const items = [
  {
    label: "Kyoto",
    sublabel: "Japan",
    description: "Ancient temples hidden among bamboo groves",
    image: "/images/kyoto.jpg",
  },
];

<HoverExpand items={items} />;
```

#### Demo Code

```tsx
"use client";

import {
  HoverExpand,
  type HoverExpandProps,
} from "@/registry/components/unlumen/hover-expand";

const ITEMS: HoverExpandProps["items"] = [
  {
    label: "Kyoto",
    sublabel: "Japan",
    description: "Ancient temples hidden among bamboo groves",
    image:
      "https://images.unsplash.com/vector-1749746338337-b3c0a35a5975?q=80&w=2242&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
    imageAlt: "Kyoto, Japan",
  },
  {
    label: "Lisbon",
    sublabel: "Portugal",
    description: "Sunlit hills and crumbling azulejo facades",
    image:
      "https://images.unsplash.com/vector-1766815276251-fa4fa3d81ce9?q=80&w=2242&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
    imageAlt: "Lisbon, Portugal",
  },
  {
    label: "Marrakech",
    sublabel: "Morocco",
    description: "A labyrinth of souks washed in saffron light",
    image:
      "https://images.unsplash.com/vector-1769292737537-e37dc3c08514?q=80&w=2282&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
    imageAlt: "Marrakech, Morocco",
  },
  {
    label: "Reykjavik",
    sublabel: "Iceland",
    description: "Aurora skies above volcanic black sand shores",
    image:
      "https://images.unsplash.com/vector-1738163099330-5c9841c97c39?q=80&w=2360&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
    imageAlt: "Reykjavik, Iceland",
  },
  {
    label: "Oaxaca",
    sublabel: "Mexico",
    description: "Mezcal smoke and murals on colonial walls",
    image:
      "https://images.unsplash.com/vector-1743446716355-85d9578a73d9?q=80&w=1800&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
    imageAlt: "Oaxaca, Mexico",
  },
];

type HoverExpandDemoProps = Pick<
  HoverExpandProps,
  "collapsedHeight" | "expandedHeight"
>;

export const HoverExpandDemo = ({
  collapsedHeight = 68,
  expandedHeight = 320,
}: HoverExpandDemoProps) => {
  return (
    <div className="flex items-center justify-center min-h-[520px] w-full p-8">
      <div className="w-full max-w-2xl">
        <HoverExpand
          items={ITEMS}
          collapsedHeight={collapsedHeight}
          expandedHeight={expandedHeight}
        />
      </div>
    </div>
  );
};
```

#### Props

##### `HoverExpand` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `items` | `HoverExpandItem[]` | `-` | Array of items to display. |
| `collapsedHeight?` | `number` | `68` | Row height in pixels when not hovered. |
| `expandedHeight?` | `number` | `320` | Row height in pixels when hovered. |
| `className?` | `string` | `-` | Additional CSS classes on the container. |

##### `HoverExpandItem` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `label` | `string` | `-` | Primary row label. |
| `image` | `string` | `-` | Image URL revealed on hover. |
| `sublabel?` | `string` | `-` | Secondary metadata shown beside the label (country, year, category, etc.). |
| `description?` | `string` | `-` | Short descriptor that slides in from the left when the row is expanded. |
| `imageAlt?` | `string` | `&quot;&quot;` | Alt text for the image. |

#### Component Source Code

##### File: `registry/components/unlumen/hover-expand/index.tsx`

```tsx
"use client";

import * as React from "react";
import { motion } from "motion/react";

import { cn } from "@/lib/utils";

export interface HoverExpandItem {
  label: string;
  /** e.g. country, year, category */
  sublabel?: string;
  image: string;
  imageAlt?: string;
  /** short descriptor shown when expanded */
  description?: string;
}

export interface HoverExpandProps {
  items: HoverExpandItem[];
  /**
   * Row height when collapsed, in pixels.
   * @default 68
   */
  collapsedHeight?: number;
  /**
   * Row height when expanded, in pixels.
   * @default 320
   */
  expandedHeight?: number;
  className?: string;
}

export function HoverExpand({
  items,
  collapsedHeight = 68,
  expandedHeight = 320,
  className,
}: HoverExpandProps) {
  const [hoveredIndex, setHoveredIndex] = React.useState<number | null>(null);

  return (
    <div className={cn("flex flex-col w-full", className)}>
      <div className="w-full border-t border-current opacity-15" />

      {items.map((item, i) => {
        const isHovered = hoveredIndex === i;
        const isOtherHovered = hoveredIndex !== null && !isHovered;

        return (
          <React.Fragment key={i}>
            <motion.div
              className="relative w-full overflow-hidden cursor-default"
              animate={{
                height: isHovered ? expandedHeight : collapsedHeight,
                opacity: isOtherHovered ? 0.38 : 1,
              }}
              transition={{
                height: {
                  type: "spring",
                  stiffness: 280,
                  damping: 32,
                  mass: 0.9,
                },
                opacity: { duration: 0.22, ease: "easeOut" },
              }}
              onHoverStart={() => setHoveredIndex(i)}
              onHoverEnd={() => setHoveredIndex(null)}
            >
              <motion.div
                className="absolute inset-0 w-full h-full"
                initial={false}
                animate={{
                  opacity: isHovered ? 1 : 0,
                  scale: isHovered ? 1 : 1.06,
                }}
                transition={{
                  opacity: { duration: 0.45, ease: [0.23, 1, 0.32, 1] },
                  scale: { duration: 0.55, ease: [0.23, 1, 0.32, 1] },
                }}
              >
                <img
                  src={item.image}
                  alt={item.imageAlt ?? ""}
                  className="w-full h-full object-cover"
                  loading="lazy"
                  decoding="async"
                />
                <div className="absolute inset-0 bg-gradient-to-t from-black/70 via-black/20 to-black/10" />
              </motion.div>

              <div className="absolute inset-0 flex items-end px-5 pb-4">
                <div className="flex w-full items-end justify-between gap-4">
                  <div className="flex items-baseline gap-3 min-w-0">
                    <motion.span
                      className="text-xs tabular-nums shrink-0 opacity-40"
                      animate={{
                        color: isHovered ? "#ffffff" : "currentColor",
                        opacity: isHovered ? 0.5 : 0.4,
                      }}
                      transition={{ duration: 0.2 }}
                    >
                      {String(i + 1).padStart(2, "0")}
                    </motion.span>

                    <motion.span
                      className="font-semibold tracking-tight truncate"
                      style={{ fontSize: "clamp(1.1rem, 2.2vw, 1.5rem)" }}
                      animate={{
                        color: isHovered ? "#ffffff" : "currentColor",
                      }}
                      transition={{ duration: 0.2 }}
                    >
                      {item.label}
                    </motion.span>

                    {item.description && (
                      <motion.span
                        className="text-sm text-white/70 truncate hidden sm:block"
                        initial={{ opacity: 0, x: -8 }}
                        animate={{
                          opacity: isHovered ? 1 : 0,
                          x: isHovered ? 0 : -8,
                        }}
                        transition={{
                          duration: 0.3,
                          delay: isHovered ? 0.12 : 0,
                          ease: [0.23, 1, 0.32, 1],
                        }}
                      >
                        — {item.description}
                      </motion.span>
                    )}
                  </div>

                  {item.sublabel && (
                    <motion.span
                      className="text-xs tracking-widest uppercase shrink-0"
                      animate={{
                        color: isHovered
                          ? "rgba(255,255,255,0.55)"
                          : "currentColor",
                        opacity: isHovered ? 1 : 0.45,
                      }}
                      transition={{ duration: 0.2 }}
                    >
                      {item.sublabel}
                    </motion.span>
                  )}
                </div>
              </div>
            </motion.div>

            <div className="w-full border-t border-current opacity-15" />
          </React.Fragment>
        );
      })}
    </div>
  );
}
```

---

### Hover Feature Cards <a name="hover-feature-cards"></a>

A responsive grid of feature cards where a description panel slides in from below on hover, with spring animation and optional image support.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/hover-feature-cards.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import {
  HoverFeatureCards,
  type HoverFeatureCard,
} from "@/components/unlumen-ui/hover-feature-cards";
import Link from "next/link";

const items: HoverFeatureCard[] = [
  {
    name: "Components",
    description:
      "Real components, not just primitives. Ready to use and customize.",
    href: "/components",
    img: "/blocks.png",
    imgLight: "/blocks-light.png",
    imgClassName: "absolute -bottom-10 left-1/2 -translate-x-1/2",
    imgWidth: 320,
    containerClassName: "h-full rounded-3xl",
    fadeBottom: true,
  },
  {
    name: "Refined UI",
    description: "Polished UI system, focused on aesthetics and smooth UX.",
    href: "/docs/ui",
    img: "/refined-ui.png",
    imgWidth: 200,
    containerClassName: "h-full rounded-3xl",
  },
];

export default function Example() {
  return (
    <HoverFeatureCards
      items={items}
      renderLink={(href, children) => <Link href={href}>{children}</Link>}
    />
  );
}
```

#### Demo Code

```tsx
"use client";

import { HoverFeatureCard } from "@/registry/components/unlumen/hover-feature-cards";

const ITEM = {
  name: "Components",
  description:
    "Real components, not just primitives. Ready to use and customize in your projects.",
  img: "/blocks.png",
  imgLight: "/blocks-light.png",
  imgClassName: "absolute -bottom-10 left-1/2 -translate-x-1/2",
  imgWidth: 320,
  containerClassName: "h-full rounded-3xl",
  fadeBottom: true,
  soon: false,
};

export default function HoverFeatureCardsDemo() {
  return (
    <div className="w-full max-w-md mx-auto px-5 py-10">
      <HoverFeatureCard item={ITEM} />
    </div>
  );
}
```

#### Props

##### `HoverFeatureCards` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `items` | `HoverFeatureCard[]` | `-` | Array of feature card data objects. |
| `renderLink?` | `(href: string, children: React.ReactNode) =&gt; React.ReactNode` | `-` | Optional render prop to wrap linked cards. Receives the href and card children. Use this to inject Next.js Link or any router link. |
| `className?` | `string` | `-` | Extra classes applied to the grid wrapper. |

##### `HoverFeatureCard (item shape)` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `name` | `string` | `-` | Card title displayed inside the illustration area. |
| `description` | `string` | `-` | Text shown in the slide-in panel on hover. |
| `href?` | `string` | `-` | Optional link target. When omitted the card is non-interactive. |
| `img?` | `string` | `-` | Image URL shown in dark mode (or always when imgLight is not set). |
| `imgLight?` | `string` | `-` | Image URL shown in light mode. |
| `imgClassName?` | `string` | `-` | Extra classes applied to the image element. |
| `imgWidth?` | `number` | `-` | Intrinsic width of the image in pixels. |
| `containerClassName?` | `string` | `-` | Extra classes applied to the illustration container. |
| `fadeBottom?` | `boolean` | `-` | When true, a gradient fades the bottom of the illustration area into the card background. |
| `soon?` | `boolean` | `-` | When true, renders a 'Coming soon' badge and disables hover interaction. |

#### Component Source Code

##### File: `registry/components/unlumen/hover-feature-cards/index.tsx`

```tsx
"use client";

import * as React from "react";
import { cn } from "@/lib/utils";
import { motion } from "motion/react";

export interface HoverFeatureCard {
  name: string;
  description: string;
  href?: string;
  img?: string;
  imgLight?: string;
  imgClassName?: string;
  imgWidth?: number;
  containerClassName?: string;
  fadeBottom?: boolean;
  soon?: boolean;
}

export interface HoverFeatureCardsProps {
  items: HoverFeatureCard[];
  className?: string;
  renderLink?: (href: string, children: React.ReactNode) => React.ReactNode;
}

function HoverFeatureCard({
  item,
  renderLink,
}: {
  item: HoverFeatureCard;
  renderLink?: HoverFeatureCardsProps["renderLink"];
}) {
  const inner = (
    <motion.div
      initial="rest"
      whileHover="hover"
      animate="rest"
      whileTap={{ scale: item.href && !item.soon ? 0.97 : 1 }}
      transition={{ type: "spring", stiffness: 300, damping: 22 }}
      variants={{ rest: { scale: 1, y: 0 } }}
      className={cn(
        "group flex flex-col w-full relative",
        item.soon
          ? "opacity-80 cursor-not-allowed"
          : item.href
            ? "cursor-pointer"
            : "",
      )}
    >
      <div
        className={cn(
          "flex flex-col rounded-3xl border h-64 z-5 bg-surface transition-colors w-full",
          !item.soon && item.href ? "hover:border-border/80" : "",
          item.soon ? "border-dashed border-border" : "border-border",
        )}
      >
        {item.soon && (
          <span className="absolute top-3 right-3 z-10 text-xs text-muted-foreground border rounded-full px-2 py-1 bg-card">
            Coming soon
          </span>
        )}

        <div
          className={cn(
            "relative w-full overflow-hidden bg-muted/10 px-5 pt-6 pb-4 flex flex-col gap-3",
            item.containerClassName,
          )}
        >
          <span
            className={cn(
              "font-medium text-xl tracking-tight",
              item.soon ? "text-muted-foreground/80" : "text-foreground",
            )}
          >
            {item.name}
          </span>

          {item.img && (
            <img
              src={item.img}
              alt={item.name}
              width={item.imgWidth ?? 200}
              height={200}
              className={cn("h-auto hidden dark:block", item.imgClassName)}
            />
          )}
          {(item.imgLight ?? item.img) && (
            <img
              src={item.imgLight ?? item.img}
              alt={item.name}
              width={item.imgWidth ?? 200}
              height={200}
              className={cn("h-auto dark:hidden", item.imgClassName)}
            />
          )}

          {item.fadeBottom && (
            <div className="pointer-events-none absolute inset-x-0 bottom-0 h-22 bg-gradient-to-t from-surface to-transparent" />
          )}
        </div>
      </div>

      <motion.div
        variants={{
          rest: { opacity: 0.2, y: -30 },
          hover: { opacity: 1, y: 0 },
        }}
        transition={{ type: "spring", stiffness: 200, damping: 15 }}
        className="overflow-hidden z-1 w-11/12 self-center"
      >
        <div className="py-3 px-5 relative border-t-0 rounded-b-3xl border">
          <div className="pointer-events-none w-[103%] bg-gradient-to-b from-background to-transparent h-12 absolute -top-1 -left-1" />
          <p className="text-sm font-base text-muted-foreground">
            {item.description}
          </p>
        </div>
      </motion.div>
    </motion.div>
  );

  if (item.href && renderLink) {
    return renderLink(item.href, inner);
  }

  return inner;
}

function HoverFeatureCards({
  items,
  className,
  renderLink,
}: HoverFeatureCardsProps) {
  return (
    <div
      className={cn("grid grid-cols-1 sm:grid-cols-2 gap-4 w-full", className)}
    >
      {items.map((item) => (
        <HoverFeatureCard key={item.name} item={item} renderLink={renderLink} />
      ))}
    </div>
  );
}

export { HoverFeatureCards, HoverFeatureCard };
```

---

### Hover Image List <a name="hover-image-list"></a>

A list of large headings that reveal an image on the left and shift right on hover, animated with Motion.

- **Tier**: Pro
- **Category**: Animations

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { HoverImageList } from "@/components/unlumen-ui/hover-image-list";

const items = [
  { label: "Design systems", image: "/images/design.jpg" },
  { label: "Prototyping", image: "/images/proto.jpg" },
  { label: "Motion & animation", image: "/images/motion.jpg" },
];

<HoverImageList items={items} />;
```

#### Props

##### `HoverImageList` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `items` | `HoverImageListItem[]` | `-` | Array of items to display. |
| `imageWidth?` | `number` | `220` | Width of the image panel in pixels. |
| `imageGap?` | `number` | `40` | Gap between the image panel and the text list in pixels. |
| `className?` | `string` | `-` | Additional CSS classes. |

##### `HoverImageListItem` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `label` | `string` | `-` | Large heading text. |
| `image` | `string` | `-` | Image URL shown when this item is hovered. |
| `imageAlt?` | `string` | `&quot;&quot;` | Alt text for the image. |

---

### Matrix <a name="matrix"></a>

An LED dot matrix display component with SVG pixels, radial glow, animated presets, and a VU meter mode.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/matrix.json
```

#### How to Use

```tsx
import {
  Matrix,
  digits,
  loader,
  wave,
  snake,
  pulse,
  chevronLeft,
  chevronRight,
  vu,
  emptyFrame,
  setPixel,
} from "@/components/unlumen-ui/matrix"

// Static pattern — render digit "7"
<Matrix rows={7} cols={5} pattern={digits[7]} size={12} gap={3} />

// Animated preset
<Matrix rows={7} cols={7} frames={loader} fps={12} size={12} gap={3} />

// VU meter mode
<Matrix
  rows={7}
  cols={8}
  mode="vu"
  levels={[0.9, 0.6, 0.8, 0.4, 0.7, 0.5, 0.8, 0.3]}
  size={12}
  gap={3}
/>
```

#### Demo Code

```tsx
"use client";

import { useEffect, useRef, useState } from "react";

import { cn } from "@workspace/ui/lib/utils";
import { UnlumenSlider } from "@workspace/ui/components/ui/unlumen-slider";
import {
  type Frame,
  Matrix,
  digits,
  emptyFrame,
  loader,
  pulse,
  setPixel,
  snake,
  wave,
} from "@/registry/components/unlumen/matrix";

const COLON: Frame = [
  [0, 0, 0],
  [0, 0, 0],
  [0, 1, 0],
  [0, 0, 0],
  [0, 1, 0],
  [0, 0, 0],
  [0, 0, 0],
];

const COLON_OFF: Frame = [
  [0, 0, 0],
  [0, 0, 0],
  [0, 0, 0],
  [0, 0, 0],
  [0, 0, 0],
  [0, 0, 0],
  [0, 0, 0],
];

function ClockDisplay({ size = 10, gap = 2 }: { size?: number; gap?: number }) {
  const [now, setNow] = useState<Date | null>(null);
  const [colonOn, setColonOn] = useState(true);

  useEffect(() => {
    setNow(new Date());
    const id = setInterval(() => {
      setNow(new Date());
      setColonOn((v) => !v);
    }, 1000);
    return () => clearInterval(id);
  }, []);

  if (!now) {
    return (
      <div style={{ height: 7 * (size + gap) - gap }} className="opacity-0" />
    );
  }

  const h = now.getHours();
  const m = now.getMinutes();
  const s = now.getSeconds();

  const parts: Array<{ type: "digit"; value: number } | { type: "colon" }> = [
    { type: "digit", value: Math.floor(h / 10) },
    { type: "digit", value: h % 10 },
    { type: "colon" },
    { type: "digit", value: Math.floor(m / 10) },
    { type: "digit", value: m % 10 },
    { type: "colon" },
    { type: "digit", value: Math.floor(s / 10) },
    { type: "digit", value: s % 10 },
  ];

  return (
    <div className="flex items-center gap-1">
      {parts.map((part, i) =>
        part.type === "colon" ? (
          <Matrix
            key={i}
            rows={7}
            cols={3}
            pattern={colonOn ? COLON : COLON_OFF}
            size={size}
            gap={gap}
            ariaLabel=":"
          />
        ) : (
          <Matrix
            key={i}
            rows={7}
            cols={5}
            pattern={digits[part.value]}
            size={size}
            gap={gap}
            ariaLabel={String(part.value)}
          />
        ),
      )}
    </div>
  );
}

const NUM_BANDS = 16;

function useVuLevels(numBands: number) {
  const [levels, setLevels] = useState<number[]>(() =>
    Array.from({ length: numBands }, (_, i) => {
      const t = i / (numBands - 1);
      return 0.15 + Math.sin(t * Math.PI) * 0.55;
    }),
  );

  const frameRef = useRef(0);

  useEffect(() => {
    const id = setInterval(() => {
      frameRef.current++;
      const f = frameRef.current;
      setLevels((prev) =>
        prev.map((v, i) => {
          const bass =
            Math.abs(Math.sin(f * 0.07 + i * 0.35)) *
            (i < numBands * 0.4 ? 0.9 : 0.5);
          const mid =
            Math.abs(Math.sin(f * 0.13 + i * 0.6)) *
            (i >= numBands * 0.3 && i < numBands * 0.7 ? 0.7 : 0.2);
          const treble =
            Math.abs(Math.sin(f * 0.21 + i * 0.9)) *
            (i >= numBands * 0.6 ? 0.6 : 0.1);
          const noise = (Math.random() - 0.5) * 0.08;
          const target = Math.max(
            0.04,
            Math.min(1, bass + mid + treble + noise),
          );
          return v + (target - v) * 0.25;
        }),
      );
    }, 60);
    return () => clearInterval(id);
  }, [numBands]);

  return levels;
}

// ─── Preset showcase ──────────────────────────────────────────────────────────

const PRESETS = [
  { name: "Loader", frames: loader },
  { name: "Wave", frames: wave },
  { name: "Snake", frames: snake },
  { name: "Pulse", frames: pulse },
] as const;

// ─── Draw pad ─────────────────────────────────────────────────────────────────

const PAD_ROWS = 7;
const PAD_COLS = 7;

function DrawPad({ size = 24, gap = 3 }: { size?: number; gap?: number }) {
  const [grid, setGrid] = useState<Frame>(() => emptyFrame(PAD_ROWS, PAD_COLS));
  const painting = useRef(false);
  const lastCell = useRef<string | null>(null);

  function toggleCell(row: number, col: number, forceOn?: boolean) {
    const key = `${row}-${col}`;
    if (key === lastCell.current) return;
    lastCell.current = key;
    setGrid((prev) => {
      const next = prev.map((r) => [...r]);
      next[row][col] =
        forceOn != null ? (forceOn ? 1 : 0) : prev[row][col] > 0 ? 0 : 1;
      return next;
    });
  }

  const cellW = size + gap;
  const svgW = PAD_COLS * cellW - gap;
  const svgH = PAD_ROWS * cellW - gap;

  function getCellFromEvent(
    e: React.MouseEvent<SVGSVGElement>,
  ): [number, number] | null {
    const rect = e.currentTarget.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;
    const col = Math.floor(x / cellW);
    const row = Math.floor(y / cellW);
    if (row < 0 || row >= PAD_ROWS || col < 0 || col >= PAD_COLS) return null;
    return [row, col];
  }

  return (
    <div className="flex flex-col items-center gap-3">
      <svg
        width={svgW}
        height={svgH}
        viewBox={`0 0 ${svgW} ${svgH}`}
        className="cursor-crosshair touch-none select-none"
        onMouseDown={(e) => {
          painting.current = true;
          lastCell.current = null;
          const cell = getCellFromEvent(e);
          if (cell) toggleCell(cell[0], cell[1]);
        }}
        onMouseMove={(e) => {
          if (!painting.current) return;
          const cell = getCellFromEvent(e);
          if (cell) toggleCell(cell[0], cell[1], true);
        }}
        onMouseUp={() => {
          painting.current = false;
          lastCell.current = null;
        }}
        onMouseLeave={() => {
          painting.current = false;
          lastCell.current = null;
        }}
      >
        {Array.from({ length: PAD_ROWS }, (_, row) =>
          Array.from({ length: PAD_COLS }, (_, col) => {
            const on = grid[row]?.[col] > 0;
            const cx = col * cellW + size / 2;
            const cy = row * cellW + size / 2;
            return (
              <circle
                key={`${row}-${col}`}
                cx={cx}
                cy={cy}
                r={(size / 2) * 0.85}
                className={cn(
                  "transition-all duration-100",
                  on
                    ? "fill-foreground drop-shadow-[0_0_4px_currentColor]"
                    : "fill-muted-foreground/20 hover:fill-muted-foreground/40",
                )}
              />
            );
          }),
        )}
      </svg>
      <button
        onClick={() => setGrid(emptyFrame(PAD_ROWS, PAD_COLS))}
        className="text-[10px] font-mono tracking-widest uppercase text-muted-foreground hover:text-foreground transition-colors"
      >
        clear
      </button>
    </div>
  );
}

// ─── Image to Frame ───────────────────────────────────────────────────────────

const IMG_MIN = 4;
const IMG_MAX = 32;
const IMG_DEFAULT = 16;

function imageToFrame(
  image: HTMLImageElement,
  rows: number,
  cols: number,
  threshold: number,
  invert: boolean,
): Frame {
  const canvas = document.createElement("canvas");
  canvas.width = cols;
  canvas.height = rows;
  const ctx = canvas.getContext("2d")!;
  ctx.imageSmoothingEnabled = true;
  ctx.imageSmoothingQuality = "high";
  ctx.drawImage(image, 0, 0, cols, rows);
  const { data } = ctx.getImageData(0, 0, cols, rows);
  const frame: Frame = [];
  for (let r = 0; r < rows; r++) {
    const row: number[] = [];
    for (let c = 0; c < cols; c++) {
      const i = (r * cols + c) * 4;
      const luma =
        (0.299 * data[i] + 0.587 * data[i + 1] + 0.114 * data[i + 2]) / 255;
      const alpha = data[i + 3] / 255;
      const value = invert ? luma : 1 - luma;
      row.push(value * alpha >= threshold ? 1 : 0);
    }
    frame.push(row);
  }
  return frame;
}

function frameToCode(frame: Frame): string {
  const inner = frame.map((row) => `  [${row.join(", ")}]`).join(",\n");
  return `const frame: Frame = [\n${inner},\n];`;
}

function ImageToFramePanel() {
  const [frame, setFrame] = useState<Frame | null>(null);
  const [rows, setRows] = useState(IMG_DEFAULT);
  const [cols, setCols] = useState(IMG_DEFAULT);
  const [threshold, setThreshold] = useState(0.5);
  const [invert, setInvert] = useState(false);
  const [isDragging, setIsDragging] = useState(false);
  const [copied, setCopied] = useState(false);
  const imageRef = useRef<HTMLImageElement | null>(null);
  const inputRef = useRef<HTMLInputElement>(null);

  function recompute(
    img: HTMLImageElement,
    r: number,
    c: number,
    t: number,
    inv: boolean,
  ) {
    setFrame(imageToFrame(img, r, c, t, inv));
  }

  function handleFile(file: File) {
    if (!file.type.startsWith("image/")) return;
    const url = URL.createObjectURL(file);
    const img = new Image();
    img.onload = () => {
      URL.revokeObjectURL(url);
      imageRef.current = img;
      recompute(img, rows, cols, threshold, invert);
    };
    img.onerror = () => URL.revokeObjectURL(url);
    img.src = url;
  }

  const pixelSize = Math.max(2, Math.floor(320 / Math.max(rows, cols)) - 1);

  return (
    <div className="flex flex-col items-center gap-5 w-full">
      {!frame ? (
        <button
          onClick={() => inputRef.current?.click()}
          onDragOver={(e) => {
            e.preventDefault();
            setIsDragging(true);
          }}
          onDragLeave={() => setIsDragging(false)}
          onDrop={(e) => {
            e.preventDefault();
            setIsDragging(false);
            const file = e.dataTransfer.files[0];
            if (file) handleFile(file);
          }}
          className={cn(
            "group flex flex-col items-center justify-center gap-3",
            "w-56 h-40 rounded-xl border-2 border-dashed",
            "transition-all duration-200 cursor-pointer",
            isDragging
              ? "border-foreground/40 bg-foreground/5"
              : "border-border hover:border-foreground/30 hover:bg-muted/30",
          )}
        >
          <svg
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            strokeWidth="1.5"
            strokeLinecap="round"
            strokeLinejoin="round"
            className={cn(
              "transition-colors duration-200",
              isDragging
                ? "text-foreground/60"
                : "text-muted-foreground group-hover:text-foreground/50",
            )}
          >
            <rect x="3" y="3" width="18" height="18" rx="2" />
            <circle cx="9" cy="9" r="2" />
            <path d="m21 15-3.086-3.086a2 2 0 0 0-2.828 0L6 21" />
          </svg>
          <span className="text-xs text-muted-foreground font-mono tracking-wide">
            {isDragging ? "drop image" : "upload image"}
          </span>
        </button>
      ) : (
        <div className="flex flex-col items-center gap-4 w-full max-w-xs">
          <Matrix
            rows={rows}
            cols={cols}
            pattern={frame}
            size={pixelSize}
            gap={1}
            ariaLabel="image to frame preview"
          />
          <div className="flex flex-col gap-2.5 w-full">
            <div className="flex gap-4">
              <UnlumenSlider
                value={rows}
                onChange={(v) => {
                  const n = v as number;
                  setRows(n);
                  if (imageRef.current)
                    recompute(imageRef.current, n, cols, threshold, invert);
                }}
                min={IMG_MIN}
                max={IMG_MAX}
                step={1}
                label="rows"
                valuePosition="right"
                className="flex-1"
              />
              <UnlumenSlider
                value={cols}
                onChange={(v) => {
                  const n = v as number;
                  setCols(n);
                  if (imageRef.current)
                    recompute(imageRef.current, rows, n, threshold, invert);
                }}
                min={IMG_MIN}
                max={IMG_MAX}
                step={1}
                label="cols"
                valuePosition="right"
                className="flex-1"
              />
            </div>
            <UnlumenSlider
              value={threshold}
              onChange={(v) => {
                const n = v as number;
                setThreshold(n);
                if (imageRef.current)
                  recompute(imageRef.current, rows, cols, n, invert);
              }}
              min={0.1}
              max={0.9}
              step={0.01}
              label="threshold"
              valuePosition="right"
              formatValue={(v) => v.toFixed(2)}
            />
            <div className="flex items-center justify-between">
              <div className="flex items-center gap-3">
                <button
                  onClick={() => {
                    const next = !invert;
                    setInvert(next);
                    if (imageRef.current)
                      recompute(imageRef.current, rows, cols, threshold, next);
                  }}
                  className={cn(
                    "flex items-center gap-1.5 text-[10px] font-mono tracking-widest uppercase transition-colors",
                    invert
                      ? "text-foreground"
                      : "text-muted-foreground hover:text-foreground/60",
                  )}
                >
                  <span
                    className={cn(
                      "inline-block w-3 h-3 rounded-full border transition-all",
                      invert
                        ? "bg-foreground border-foreground"
                        : "bg-transparent border-muted-foreground",
                    )}
                  />
                  invert
                </button>
                <button
                  onClick={() => inputRef.current?.click()}
                  className="text-[10px] font-mono tracking-widest uppercase text-muted-foreground hover:text-foreground transition-colors"
                >
                  change
                </button>
              </div>
              <button
                onClick={() => {
                  if (!frame) return;
                  navigator.clipboard.writeText(frameToCode(frame));
                  setCopied(true);
                  setTimeout(() => setCopied(false), 1800);
                }}
                className={cn(
                  "text-[10px] font-mono tracking-widest uppercase transition-colors px-2.5 py-1 rounded border",
                  copied
                    ? "border-foreground/30 text-foreground bg-foreground/5"
                    : "border-border text-muted-foreground hover:text-foreground hover:border-foreground/30",
                )}
              >
                {copied ? "copied!" : "copy frame"}
              </button>
            </div>
          </div>
        </div>
      )}
      <input
        ref={inputRef}
        type="file"
        accept="image/*"
        className="hidden"
        onChange={(e) => {
          const file = e.target.files?.[0];
          if (file) handleFile(file);
          e.target.value = "";
        }}
      />
    </div>
  );
}

// ─── Tabs ─────────────────────────────────────────────────────────────────────

type Tab = "clock" | "presets" | "vu" | "draw" | "image";

const TABS: { id: Tab; label: string }[] = [
  { id: "clock", label: "Clock" },
  { id: "presets", label: "Presets" },
  { id: "vu", label: "VU Meter" },
  { id: "draw", label: "Draw" },
  { id: "image", label: "Image" },
];

// ─── Main demo ────────────────────────────────────────────────────────────────

export default function MatrixDemo() {
  const [tab, setTab] = useState<Tab>("clock");
  const [activePreset, setActivePreset] = useState(0);
  const vuLevels = useVuLevels(NUM_BANDS);

  useEffect(() => {
    if (tab !== "presets") return;
    const id = setInterval(
      () => setActivePreset((p) => (p + 1) % PRESETS.length),
      2800,
    );
    return () => clearInterval(id);
  }, [tab]);

  return (
    <div className="flex flex-col items-center justify-center gap-8 p-8 min-h-[420px]">
      {/* Tab bar */}
      <div className="flex gap-1 rounded-lg border border-border bg-muted/40 p-1">
        {TABS.map(({ id, label }) => (
          <button
            key={id}
            onClick={() => setTab(id)}
            className={cn(
              "rounded-md px-3 py-1.5 text-xs font-medium transition-all duration-200",
              tab === id
                ? "bg-background text-foreground shadow-sm"
                : "text-muted-foreground hover:text-foreground",
            )}
          >
            {label}
          </button>
        ))}
      </div>

      {/* Panels */}
      {tab === "clock" && (
        <div className="flex flex-col items-center gap-4">
          <ClockDisplay size={10} gap={2} />
          <p className="text-[10px] font-mono tracking-widest uppercase text-muted-foreground">
            Live clock · digit matrices
          </p>
        </div>
      )}

      {tab === "presets" && (
        <div className="flex flex-col items-center gap-6">
          <div className="flex gap-8 items-end">
            {PRESETS.map((preset, i) => (
              <button
                key={preset.name}
                onClick={() => setActivePreset(i)}
                className="flex flex-col items-center gap-2 group"
              >
                <Matrix
                  rows={7}
                  cols={7}
                  frames={preset.frames}
                  fps={i === activePreset ? 10 : 6}
                  size={11}
                  gap={2}
                  className={cn(
                    "transition-opacity duration-300",
                    i === activePreset ? "opacity-100" : "opacity-100",
                  )}
                  ariaLabel={preset.name}
                />
                <span
                  className={cn(
                    "text-[9px] font-mono tracking-widest uppercase transition-colors duration-300",
                    i === activePreset
                      ? "text-foreground"
                      : "text-muted-foreground group-hover:text-foreground/60",
                  )}
                >
                  {preset.name}
                </span>
              </button>
            ))}
          </div>
          <p className="text-[10px] font-mono tracking-widest uppercase text-muted-foreground">
            Click a preset to focus
          </p>
        </div>
      )}

      {tab === "vu" && (
        <div className="flex flex-col items-center gap-4">
          <Matrix
            rows={7}
            cols={NUM_BANDS}
            mode="vu"
            levels={vuLevels}
            size={10}
            gap={2}
            ariaLabel="VU meter"
          />
          <p className="text-[10px] font-mono tracking-widest uppercase text-muted-foreground">
            Simulated audio levels
          </p>
        </div>
      )}

      {tab === "draw" && (
        <div className="flex flex-col items-center gap-4">
          <DrawPad size={22} gap={3} />
          <p className="text-[10px] font-mono tracking-widest uppercase text-muted-foreground">
            Click or drag to draw
          </p>
        </div>
      )}

      {tab === "image" && <ImageToFramePanel />}
    </div>
  );
}
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `rows` | `number` | `-` | Number of pixel rows. |
| `cols` | `number` | `-` | Number of pixel columns. |
| `pattern?` | `Frame` | `-` | A static 2D array (Frame) to display. Disables animation when set. |
| `frames?` | `Frame[]` | `-` | An array of Frames to animate through. |
| `fps?` | `number` | `12` | Animation frame rate. |
| `autoplay?` | `boolean` | `true` | Whether to start animation automatically. |
| `loop?` | `boolean` | `true` | Whether to loop the animation. |
| `size?` | `number` | `10` | Pixel diameter in pixels. |
| `gap?` | `number` | `2` | Gap between pixels in pixels. |
| `palette?` | `{ on: string; off: string }` | `{ on: &quot;currentColor&quot;, off: &quot;var(--muted-foreground)&quot; }` | Color overrides for on/off pixels. Accepts any CSS color value. |
| `brightness?` | `number` | `1` | Global brightness multiplier applied to all pixels (0–1). |
| `mode?` | `&quot;default&quot; \| &quot;vu&quot;` | `&quot;default&quot;` | Display mode. `"vu"` renders a vertical bar graph from the `levels` prop. |
| `levels?` | `number[]` | `-` | Per-column level values (0–1) used when `mode="vu"`. Length should match `cols`. |
| `onFrame?` | `(index: number) =&gt; void` | `-` | Callback fired on each frame advance, receives frame index. |
| `ariaLabel?` | `string` | `&quot;matrix display&quot;` | Accessible label for the matrix display. |
| `className?` | `string` | `-` | Additional className applied to the wrapper div. |

#### Component Source Code

##### File: `registry/components/unlumen/matrix/index.tsx`

```tsx
"use client";

import * as React from "react";
import { useEffect, useMemo, useRef, useState } from "react";

import { cn } from "@/lib/utils";

export type Frame = number[][];
type MatrixMode = "default" | "vu";

interface CellPosition {
  x: number;
  y: number;
}

interface MatrixProps extends React.HTMLAttributes<HTMLDivElement> {
  rows: number;
  cols: number;
  pattern?: Frame;
  frames?: Frame[];
  fps?: number;
  autoplay?: boolean;
  loop?: boolean;
  size?: number;
  gap?: number;
  palette?: {
    on: string;
    off: string;
  };
  brightness?: number;
  ariaLabel?: string;
  onFrame?: (index: number) => void;
  mode?: MatrixMode;
  levels?: number[];
}

function clamp(value: number): number {
  return Math.max(0, Math.min(1, value));
}

function ensureFrameSize(frame: Frame, rows: number, cols: number): Frame {
  const result: Frame = [];
  for (let r = 0; r < rows; r++) {
    const row = frame[r] || [];
    result.push([]);
    for (let c = 0; c < cols; c++) {
      result[r][c] = row[c] ?? 0;
    }
  }
  return result;
}

function useAnimation(
  frames: Frame[] | undefined,
  options: {
    fps: number;
    autoplay: boolean;
    loop: boolean;
    onFrame?: (index: number) => void;
  },
): { frameIndex: number; isPlaying: boolean } {
  const [frameIndex, setFrameIndex] = useState(0);
  const [isPlaying, setIsPlaying] = useState(options.autoplay);
  const frameIdRef = useRef<number | undefined>(undefined);
  const lastTimeRef = useRef<number>(0);
  const accumulatorRef = useRef<number>(0);

  useEffect(() => {
    if (!frames || frames.length === 0 || !isPlaying) {
      return;
    }

    const frameInterval = 1000 / options.fps;

    const animate = (currentTime: number) => {
      if (lastTimeRef.current === 0) {
        lastTimeRef.current = currentTime;
      }

      const deltaTime = currentTime - lastTimeRef.current;
      lastTimeRef.current = currentTime;
      accumulatorRef.current += deltaTime;

      if (accumulatorRef.current >= frameInterval) {
        accumulatorRef.current -= frameInterval;

        setFrameIndex((prev) => {
          const next = prev + 1;
          if (next >= frames.length) {
            if (options.loop) {
              options.onFrame?.(0);
              return 0;
            } else {
              setIsPlaying(false);
              return prev;
            }
          }
          options.onFrame?.(next);
          return next;
        });
      }

      frameIdRef.current = requestAnimationFrame(animate);
    };

    frameIdRef.current = requestAnimationFrame(animate);

    return () => {
      if (frameIdRef.current) {
        cancelAnimationFrame(frameIdRef.current);
      }
    };
  }, [frames, isPlaying, options.fps, options.loop, options.onFrame]);

  useEffect(() => {
    setFrameIndex(0);
    setIsPlaying(options.autoplay);
    lastTimeRef.current = 0;
    accumulatorRef.current = 0;
  }, [frames, options.autoplay]);

  return { frameIndex, isPlaying };
}

export function emptyFrame(rows: number, cols: number): Frame {
  return Array.from({ length: rows }, () => Array(cols).fill(0));
}

export function setPixel(
  frame: Frame,
  row: number,
  col: number,
  value: number,
): void {
  if (row >= 0 && row < frame.length && col >= 0 && col < frame[0].length) {
    frame[row][col] = value;
  }
}

export const digits: Frame[] = [
  [
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
  ],
  [
    [0, 0, 1, 0, 0],
    [0, 1, 1, 0, 0],
    [0, 0, 1, 0, 0],
    [0, 0, 1, 0, 0],
    [0, 0, 1, 0, 0],
    [0, 0, 1, 0, 0],
    [0, 1, 1, 1, 0],
  ],
  [
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [0, 0, 0, 0, 1],
    [0, 0, 0, 1, 0],
    [0, 0, 1, 0, 0],
    [0, 1, 0, 0, 0],
    [1, 1, 1, 1, 1],
  ],
  [
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [0, 0, 0, 0, 1],
    [0, 0, 1, 1, 0],
    [0, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
  ],
  [
    [0, 0, 0, 1, 0],
    [0, 0, 1, 1, 0],
    [0, 1, 0, 1, 0],
    [1, 0, 0, 1, 0],
    [1, 1, 1, 1, 1],
    [0, 0, 0, 1, 0],
    [0, 0, 0, 1, 0],
  ],
  [
    [1, 1, 1, 1, 1],
    [1, 0, 0, 0, 0],
    [1, 1, 1, 1, 0],
    [0, 0, 0, 0, 1],
    [0, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
  ],
  [
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 0],
    [1, 0, 0, 0, 0],
    [1, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
  ],
  [
    [1, 1, 1, 1, 1],
    [0, 0, 0, 0, 1],
    [0, 0, 0, 1, 0],
    [0, 0, 1, 0, 0],
    [0, 1, 0, 0, 0],
    [0, 1, 0, 0, 0],
    [0, 1, 0, 0, 0],
  ],
  [
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
  ],
  [
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 1],
    [0, 0, 0, 0, 1],
    [0, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
  ],
];

export const chevronLeft: Frame = [
  [0, 0, 0, 1, 0],
  [0, 0, 1, 0, 0],
  [0, 1, 0, 0, 0],
  [0, 0, 1, 0, 0],
  [0, 0, 0, 1, 0],
];

export const chevronRight: Frame = [
  [0, 1, 0, 0, 0],
  [0, 0, 1, 0, 0],
  [0, 0, 0, 1, 0],
  [0, 0, 1, 0, 0],
  [0, 1, 0, 0, 0],
];

export const loader: Frame[] = (() => {
  const frames: Frame[] = [];
  const size = 7;
  const center = 3;
  const radius = 2.5;

  for (let frame = 0; frame < 12; frame++) {
    const f = emptyFrame(size, size);
    for (let i = 0; i < 8; i++) {
      const angle = (frame / 12) * Math.PI * 2 + (i / 8) * Math.PI * 2;
      const x = Math.round(center + Math.cos(angle) * radius);
      const y = Math.round(center + Math.sin(angle) * radius);
      const brightness = 1 - i / 10;
      setPixel(f, y, x, Math.max(0.2, brightness));
    }
    frames.push(f);
  }

  return frames;
})();

export const pulse: Frame[] = (() => {
  const frames: Frame[] = [];
  const size = 7;
  const center = 3;

  for (let frame = 0; frame < 16; frame++) {
    const f = emptyFrame(size, size);
    const phase = (frame / 16) * Math.PI * 2;
    const intensity = (Math.sin(phase) + 1) / 2;

    setPixel(f, center, center, 1);

    const radius = Math.floor((1 - intensity) * 3) + 1;
    for (let dy = -radius; dy <= radius; dy++) {
      for (let dx = -radius; dx <= radius; dx++) {
        const dist = Math.sqrt(dx * dx + dy * dy);
        if (Math.abs(dist - radius) < 0.7) {
          setPixel(f, center + dy, center + dx, intensity * 0.6);
        }
      }
    }

    frames.push(f);
  }

  return frames;
})();

export function vu(columns: number, levels: number[]): Frame {
  const rows = 7;
  const frame = emptyFrame(rows, columns);

  for (let col = 0; col < Math.min(columns, levels.length); col++) {
    const level = Math.max(0, Math.min(1, levels[col]));
    const height = Math.floor(level * rows);

    for (let row = 0; row < rows; row++) {
      const rowFromBottom = rows - 1 - row;
      if (rowFromBottom < height) {
        let brightness = 1;
        if (row < rows * 0.3) {
          brightness = 1;
        } else if (row < rows * 0.6) {
          brightness = 0.8;
        } else {
          brightness = 0.6;
        }
        frame[row][col] = brightness;
      }
    }
  }

  return frame;
}

export const wave: Frame[] = (() => {
  const frames: Frame[] = [];
  const rows = 7;
  const cols = 7;

  for (let frame = 0; frame < 24; frame++) {
    const f = emptyFrame(rows, cols);
    const phase = (frame / 24) * Math.PI * 2;

    for (let col = 0; col < cols; col++) {
      const colPhase = (col / cols) * Math.PI * 2;
      const height = Math.sin(phase + colPhase) * 2.5 + 3.5;
      const row = Math.floor(height);

      if (row >= 0 && row < rows) {
        setPixel(f, row, col, 1);
        const frac = height - row;
        if (row > 0) setPixel(f, row - 1, col, 1 - frac);
        if (row < rows - 1) setPixel(f, row + 1, col, frac);
      }
    }

    frames.push(f);
  }

  return frames;
})();

export const snake: Frame[] = (() => {
  const frames: Frame[] = [];
  const rows = 7;
  const cols = 7;
  const path: Array<[number, number]> = [];

  let x = 0;
  let y = 0;
  let dx = 1;
  let dy = 0;

  const visited = new Set<string>();
  while (path.length < rows * cols) {
    path.push([y, x]);
    visited.add(`${y},${x}`);

    const nextX = x + dx;
    const nextY = y + dy;

    if (
      nextX >= 0 &&
      nextX < cols &&
      nextY >= 0 &&
      nextY < rows &&
      !visited.has(`${nextY},${nextX}`)
    ) {
      x = nextX;
      y = nextY;
    } else {
      const newDx = -dy;
      const newDy = dx;
      dx = newDx;
      dy = newDy;

      const nextX = x + dx;
      const nextY = y + dy;

      if (
        nextX >= 0 &&
        nextX < cols &&
        nextY >= 0 &&
        nextY < rows &&
        !visited.has(`${nextY},${nextX}`)
      ) {
        x = nextX;
        y = nextY;
      } else {
        break;
      }
    }
  }

  const snakeLength = 5;
  for (let frame = 0; frame < path.length; frame++) {
    const f = emptyFrame(rows, cols);

    for (let i = 0; i < snakeLength; i++) {
      const idx = frame - i;
      if (idx >= 0 && idx < path.length) {
        const [y, x] = path[idx];
        const brightness = 1 - i / snakeLength;
        setPixel(f, y, x, brightness);
      }
    }

    frames.push(f);
  }

  return frames;
})();

export const Matrix = React.forwardRef<HTMLDivElement, MatrixProps>(
  (
    {
      rows,
      cols,
      pattern,
      frames,
      fps = 12,
      autoplay = true,
      loop = true,
      size = 10,
      gap = 2,
      palette = {
        on: "currentColor",
        off: "var(--muted-foreground)",
      },
      brightness = 1,
      ariaLabel,
      onFrame,
      mode = "default",
      levels,
      className,
      ...props
    },
    ref,
  ) => {
    const { frameIndex } = useAnimation(frames, {
      fps,
      autoplay: autoplay && !pattern,
      loop,
      onFrame,
    });

    const currentFrame = useMemo(() => {
      if (mode === "vu" && levels && levels.length > 0) {
        return ensureFrameSize(vu(cols, levels), rows, cols);
      }

      if (pattern) {
        return ensureFrameSize(pattern, rows, cols);
      }

      if (frames && frames.length > 0) {
        return ensureFrameSize(frames[frameIndex] || frames[0], rows, cols);
      }

      return ensureFrameSize([], rows, cols);
    }, [pattern, frames, frameIndex, rows, cols, mode, levels]);

    const cellPositions = useMemo(() => {
      const positions: CellPosition[][] = [];

      for (let row = 0; row < rows; row++) {
        positions[row] = [];
        for (let col = 0; col < cols; col++) {
          positions[row][col] = {
            x: col * (size + gap),
            y: row * (size + gap),
          };
        }
      }

      return positions;
    }, [rows, cols, size, gap]);

    const svgDimensions = useMemo(() => {
      return {
        width: cols * (size + gap) - gap,
        height: rows * (size + gap) - gap,
      };
    }, [rows, cols, size, gap]);

    const isAnimating = !pattern && frames && frames.length > 0;

    return (
      <div
        ref={ref}
        role="img"
        aria-label={ariaLabel ?? "matrix display"}
        aria-live={isAnimating ? "polite" : undefined}
        className={cn("relative inline-block", className)}
        style={
          {
            "--matrix-on": palette.on,
            "--matrix-off": palette.off,
            "--matrix-gap": `${gap}px`,
            "--matrix-size": `${size}px`,
          } as React.CSSProperties
        }
        {...props}
      >
        <svg
          width={svgDimensions.width}
          height={svgDimensions.height}
          viewBox={`0 0 ${svgDimensions.width} ${svgDimensions.height}`}
          xmlns="http://www.w3.org/2000/svg"
          className="block"
          style={{ overflow: "visible" }}
        >
          <defs>
            <radialGradient id="matrix-pixel-on" cx="50%" cy="50%" r="50%">
              <stop offset="0%" stopColor="var(--matrix-on)" stopOpacity="1" />
              <stop
                offset="70%"
                stopColor="var(--matrix-on)"
                stopOpacity="0.85"
              />
              <stop
                offset="100%"
                stopColor="var(--matrix-on)"
                stopOpacity="0.6"
              />
            </radialGradient>

            <radialGradient id="matrix-pixel-off" cx="50%" cy="50%" r="50%">
              <stop
                offset="0%"
                stopColor="var(--muted-foreground)"
                stopOpacity="1"
              />
              <stop
                offset="100%"
                stopColor="var(--muted-foreground)"
                stopOpacity="0.7"
              />
            </radialGradient>

            <filter
              id="matrix-glow"
              x="-50%"
              y="-50%"
              width="200%"
              height="200%"
            >
              <feGaussianBlur stdDeviation="2" result="blur" />
              <feComposite in="SourceGraphic" in2="blur" operator="over" />
            </filter>
          </defs>

          <style>
            {`
              .matrix-pixel {
                transition: opacity 300ms ease-out, transform 150ms ease-out;
                transform-origin: center;
                transform-box: fill-box;
              }
              .matrix-pixel-active {
                filter: url(#matrix-glow);
              }
            `}
          </style>

          {currentFrame.map((row, rowIndex) =>
            row.map((value, colIndex) => {
              const pos = cellPositions[rowIndex]?.[colIndex];
              if (!pos) return null;

              const opacity = clamp(brightness * value);
              const isActive = opacity > 0.5;
              const isOn = opacity > 0.05;
              const fill = isOn
                ? "url(#matrix-pixel-on)"
                : "url(#matrix-pixel-off)";

              const scale = isActive ? 1.1 : 1;
              const radius = (size / 2) * 0.9;

              return (
                <circle
                  key={`${rowIndex}-${colIndex}`}
                  className={cn(
                    "matrix-pixel",
                    isActive && "matrix-pixel-active",
                    !isOn && "opacity-20 dark:opacity-[0.1]",
                  )}
                  cx={pos.x + size / 2}
                  cy={pos.y + size / 2}
                  r={radius}
                  fill={fill}
                  opacity={isOn ? opacity : 0.1}
                  style={{
                    transform: `scale(${scale})`,
                  }}
                />
              );
            }),
          )}
        </svg>
      </div>
    );
  },
);

Matrix.displayName = "Matrix";
```

---

### Motion FAQs Accordion <a name="motion-faqs-accordion"></a>

A clean Motion-powered FAQ accordion with spring-animated content reveal.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/motion-faqs-accordion.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { MotionAccordion } from "@/components/unlumen-ui/motion-faqs-accordion";

const items = [
  {
    question: "What is Unlumen UI?",
    answer:
      "A collection of premium components built with Motion and Tailwind CSS.",
  },
  {
    question: "Do I need to install a package?",
    answer:
      "No — every component is delivered as source code via the registry.",
  },
];

<MotionAccordion items={items} />;
```

#### Demo Code

```tsx
"use client";

import {
  MotionAccordion,
  type MotionAccordionProps,
} from "@/registry/components/unlumen/motion-faqs-accordion";

const ITEMS = [
  {
    question: "What is Unlumen UI?",
    answer:
      "Unlumen UI is a collection of premium, open-source components built with Motion and Tailwind CSS. Each component is designed to be copy-pasteable, fully typed, and production-ready.",
  },
  {
    question: "Do I need to install a package?",
    answer:
      "No. Every component is delivered as source code via the registry. You copy what you need — no runtime dependency, no black-box abstractions.",
  },
  {
    question: "Is it compatible with shadcn/ui?",
    answer:
      "Yes. Unlumen UI components sit alongside shadcn/ui primitives. They share the same Tailwind CSS variables and can be mixed freely in any project.",
  },
  {
    question: "Can I use it commercially?",
    answer:
      "Free components are MIT-licensed. Premium components require an active license. See the pricing page for details.",
  },
  {
    question: "What animation library is used?",
    answer:
      "All animations use Motion (formerly Framer Motion). Springs, layout animations, and gesture handling are first-class — no CSS keyframes required.",
  },
];

type MotionAccordionDemoProps = Pick<MotionAccordionProps, "gap">;

const MotionAccordionDemo = ({ gap = 10 }: MotionAccordionDemoProps) => {
  return (
    <div className="w-full max-w-xl mx-auto px-4">
      <MotionAccordion items={ITEMS} gap={gap} />
    </div>
  );
};

export default MotionAccordionDemo;
export { MotionAccordionDemo };
```

#### Props

##### `MotionAccordion` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `items` | `MotionAccordionItem[]` | `-` | Array of FAQ items to render. |
| `gap?` | `number` | `10` | Gap between accordion items in pixels. |
| `className?` | `string` | `-` | Extra class names applied to the wrapper. |

##### `MotionAccordionItem` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `question` | `ReactNode` | `-` | The FAQ question shown in the header row. |
| `answer` | `ReactNode` | `-` | The answer text revealed when the item is open. |

#### Component Source Code

##### File: `registry/components/unlumen/motion-faqs-accordion/index.tsx`

```tsx
"use client";

import * as React from "react";
import { motion } from "motion/react";

import { cn } from "@/lib/utils";

export interface MotionAccordionItem {
  question: React.ReactNode;
  answer: React.ReactNode;
}

export interface MotionAccordionProps {
  items: MotionAccordionItem[];
  /** @default 10 */
  gap?: number;
  className?: string;
}

function AccordionItem({
  item,
  isOpen,
  onToggle,
  itemId,
  panelId,
}: {
  item: MotionAccordionItem;
  isOpen: boolean;
  onToggle: () => void;
  itemId: string;
  panelId: string;
}) {
  const contentRef = React.useRef<HTMLDivElement>(null);
  const [contentH, setContentH] = React.useState(0);

  React.useEffect(() => {
    const el = contentRef.current;
    if (!el) return;
    const ro = new ResizeObserver(() => setContentH(el.scrollHeight));
    ro.observe(el);
    setContentH(el.scrollHeight);
    return () => ro.disconnect();
  }, []);

  return (
    <motion.div
      layout
      className={cn(
        "overflow-hidden rounded-[30px] bg-surface text-foreground shadow-xs",
        isOpen && " ",
      )}
      transition={{ type: "spring", stiffness: 280, damping: 28, mass: 0.9 }}
      animate={{ scale: isOpen ? 1 : 0.985 }}
      initial={false}
      style={{ originX: 0.5, originY: 0 }}
    >
      <button
        id={itemId}
        type="button"
        aria-controls={panelId}
        aria-expanded={isOpen}
        onClick={onToggle}
        className="flex w-full cursor-pointer select-none items-center justify-between gap-4 px-7 py-5 text-left"
      >
        <span className="text-[clamp(1.2rem,1.6vw,1.3rem)] font-medium leading-snug">
          {item.question}
        </span>

        <motion.span
          aria-hidden="true"
          initial={false}
          animate={{
            rotate: isOpen ? 180 : 0,
            scale: isOpen ? 1.05 : 1,
          }}
          transition={{ type: "spring", stiffness: 480, damping: 28 }}
          className="inline-flex size-12 shrink-0 items-center justify-center text-foreground"
        >
          {isOpen ? (
            <svg
              width="14"
              height="14"
              viewBox="0 0 14 2"
              fill="none"
              aria-hidden
            >
              <path
                d="M1 1h12"
                stroke="currentColor"
                strokeWidth="1.75"
                strokeLinecap="round"
              />
            </svg>
          ) : (
            <svg
              width="14"
              height="14"
              viewBox="0 0 14 14"
              fill="none"
              aria-hidden
            >
              <path
                d="M7 1v12M1 7h12"
                stroke="currentColor"
                strokeWidth="1.75"
                strokeLinecap="round"
              />
            </svg>
          )}
        </motion.span>
      </button>

      <motion.div
        id={panelId}
        role="region"
        aria-labelledby={itemId}
        animate={{
          height: isOpen ? contentH : 0,
          opacity: isOpen ? 1 : 0,
        }}
        initial={false}
        transition={{
          height: { type: "spring", stiffness: 340, damping: 34, mass: 0.9 },
          opacity: { duration: 0.2, ease: "easeOut" },
        }}
        style={{ overflow: "hidden" }}
      >
        <motion.div
          ref={contentRef}
          animate={{ y: isOpen ? 0 : -8 }}
          transition={{
            type: "spring",
            stiffness: 360,
            damping: 30,
            mass: 0.8,
          }}
          className="px-7 pb-7"
        >
          <p className="text-lg leading-8 font-base tracking-normal text-foreground/75">
            {item.answer}
          </p>
        </motion.div>
      </motion.div>
    </motion.div>
  );
}

export function MotionAccordion({
  items,
  gap = 10,
  className,
}: MotionAccordionProps) {
  const rawId = React.useId();
  const baseId = `accordion-${rawId.replace(/:/g, "")}`;

  const [openIndex, setOpenIndex] = React.useState<number | null>(null);

  const toggle = (i: number) => setOpenIndex((prev) => (prev === i ? null : i));

  return (
    <div className={cn("w-full", className)}>
      <div className="flex flex-col rounded-[34px] p-3 " style={{ gap }}>
        {items.map((item, i) => (
          <AccordionItem
            key={i}
            item={item}
            isOpen={openIndex === i}
            onToggle={() => toggle(i)}
            itemId={`${baseId}-trigger-${i}`}
            panelId={`${baseId}-panel-${i}`}
          />
        ))}
      </div>
    </div>
  );
}
```

---

### Pinned List <a name="pinned-list"></a>

A list of items that animate smoothly to a pinned section using Framer Motion layout animations.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/pinned-list.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### How to Use

```tsx
import { PinnedList } from "@/components/unlumen-ui/pinned-list";
import { ShoppingBag01Icon as ShoppingBag } from "hugeicons-react";

const items = [
  {
    id: "1",
    name: "Apple Store",
    subtitle: "Electronics · Closes 9:00 PM",
    icon: <ShoppingBag size={16} />,
  },
];

<PinnedList items={items} />;
```

#### Demo Code

```tsx
"use client";

import {
  BookOpen01Icon as BookOpen,
  Car01Icon as Car,
  Coffee01Icon as Coffee,
  Dumbbell01Icon as Dumbbell,
  Home01Icon as Home,
  Leaf01Icon as Leaf,
  MusicNote02Icon as Music2,
  ShoppingBag01Icon as ShoppingBag,
} from "hugeicons-react";

import {
  PinnedList,
  type PinnedListItem,
} from "@/registry/components/unlumen/pinned-list";

const ITEMS: PinnedListItem[] = [
  {
    id: "1",
    name: "Café Lumière",
    subtitle: "Coffee Shop · Opens at 7:00 AM",
    icon: <Coffee size={22} />,
  },
  {
    id: "2",
    name: "Volta Bookstore",
    subtitle: "Books & Stationery · Closes at 7:00 PM",
    icon: <BookOpen size={22} />,
  },
  {
    id: "3",
    name: "Pilates Studio",
    subtitle: "Wellness · Class at 8:30 AM",
    icon: <Dumbbell size={22} />,
  },
  {
    id: "4",
    name: "Organic Market",
    subtitle: "Fresh Produce · Sat. & Sun. mornings",
    icon: <Leaf size={22} />,
  },
  {
    id: "5",
    name: "Vinyl & Co.",
    subtitle: "Record Shop · Closes at 8:00 PM",
    icon: <Music2 size={22} />,
  },
];

const PinnedListDemo = () => {
  return (
    <div className="w-full max-w-sm mx-auto px-4">
      <PinnedList items={ITEMS} />
    </div>
  );
};

export default PinnedListDemo;
export { PinnedListDemo };
```

#### Props

##### `PinnedList` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `items` | `PinnedListItem[]` | `-` | Array of items to display in the list. |
| `className?` | `string` | `-` | Additional className for the outer wrapper. |

##### `PinnedListItem` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `id` | `string` | `-` | Unique identifier used as the motion layoutId. |
| `name` | `string` | `-` | Display name shown in bold. |
| `subtitle` | `string` | `-` | Secondary line shown below the name, e.g. "Category · Detail". |
| `icon` | `React.ReactNode` | `-` | React node rendered inside the icon container. |

#### Component Source Code

##### File: `registry/components/unlumen/pinned-list/index.tsx`

```tsx
"use client";

import * as React from "react";
import {
  motion,
  AnimatePresence,
  LayoutGroup,
  type Variants,
} from "motion/react";
import { Pin } from "lucide-react";

import { cn } from "@/lib/utils";

export interface PinnedListItem {
  id: string;
  name: string;
  /** e.g. "Category · Detail" */
  subtitle: string;
  icon: React.ReactNode;
}

export interface PinnedListProps {
  items: PinnedListItem[];
  className?: string;
}

const itemVariants: Variants = {
  hidden: { opacity: 0, scale: 0.96, y: -6 },
  visible: {
    opacity: 1,
    scale: 1,
    y: 0,
    transition: { type: "spring", stiffness: 380, damping: 20, mass: 0.8 },
  },
  exit: {
    opacity: 0,
    scale: 0.96,
    y: -4,
    transition: { duration: 0.18, ease: "easeIn" },
  },
};

function ItemCard({
  item,
  pinned,
  onToggle,
}: {
  item: PinnedListItem;
  pinned: boolean;
  onToggle: () => void;
}) {
  return (
    <motion.div
      layoutId={item.id}
      layout
      variants={itemVariants}
      initial="hidden"
      animate="visible"
      exit="exit"
      className={cn(
        "flex items-center gap-3 rounded-2xl px-3 py-3.5",
        "bg-muted text-muted-foreground/50",
        "border border-transparent",
        pinned && " bg-blue-100 dark:bg-blue-950/30",
      )}
    >
      <div
        className={cn(
          "flex h-11 w-11 flex-shrink-0 items-center justify-center rounded-xl",
          "bg-background ",
          "text-foreground/70",
        )}
      >
        {item.icon}
      </div>

      <div className="min-w-0 flex-1">
        <p className="truncate text-base font-medium leading-tight text-foreground">
          {item.name}
        </p>
        <p className="truncate text-sm text-muted-foreground">
          {item.subtitle}
        </p>
      </div>

      <button
        type="button"
        onClick={onToggle}
        aria-label={pinned ? `Unpin ${item.name}` : `Pin ${item.name}`}
        className={cn(
          "flex h-8 w-8 flex-shrink-0 items-center justify-center rounded-full",
          "transition-colors duration-200",
          pinned
            ? "bg-blue-500 text-white hover:bg-blue-600"
            : "bg-muted-foreground/10 text-muted-foreground hover:bg-muted-foreground/20",
        )}
      >
        <Pin
          size={17}
          className={cn(
            "transition-transform duration-200",
            pinned && "-rotate-45",
          )}
        />
      </button>
    </motion.div>
  );
}

const headingVariants: Variants = {
  hidden: { opacity: 0, y: -6 },
  visible: {
    opacity: 1,
    y: 0,
    transition: { type: "spring", stiffness: 400, damping: 22 },
  },
  exit: { opacity: 0, y: -4, transition: { duration: 0.15, ease: "easeIn" } },
};

export function PinnedList({ items, className }: PinnedListProps) {
  const [pinnedIds, setPinnedIds] = React.useState<Set<string>>(new Set());
  // keep section visible until item exit animations finish, then unmount to trigger section exit
  const [showPinnedSection, setShowPinnedSection] = React.useState(false);
  const pinnedLengthRef = React.useRef(0);

  const togglePin = (id: string) => {
    setPinnedIds((prev) => {
      const next = new Set(prev);
      if (next.has(id)) {
        next.delete(id);
      } else {
        next.add(id);
      }
      return next;
    });
  };

  const pinned = items.filter((i) => pinnedIds.has(i.id));
  const unpinned = items.filter((i) => !pinnedIds.has(i.id));

  pinnedLengthRef.current = pinned.length;

  const [showAllSection, setShowAllSection] = React.useState(true);
  const unpinnedLengthRef = React.useRef(unpinned.length);
  unpinnedLengthRef.current = unpinned.length;

  // show as soon as something is pinned; hiding is triggered from inner AnimatePresence onExitComplete
  React.useEffect(() => {
    if (pinned.length > 0) setShowPinnedSection(true);
  }, [pinned.length]);

  React.useEffect(() => {
    if (unpinned.length > 0) setShowAllSection(true);
  }, [unpinned.length]);

  return (
    <LayoutGroup>
      <motion.div
        layout
        className={cn("flex w-full flex-col gap-1", className)}
      >
        <AnimatePresence onExitComplete={() => setShowPinnedSection(false)}>
          {showPinnedSection && (
            <motion.div
              key="pinned-section"
              layout
              variants={headingVariants}
              initial="hidden"
              animate="visible"
              exit="exit"
              className="flex flex-col gap-1"
            >
              <motion.p
                layout="position"
                className="px-1 pb-0.5 pt-1 text-sm font-medium text-muted-foreground"
              >
                Pinned Items
              </motion.p>
              <AnimatePresence
                mode="popLayout"
                onExitComplete={() => {
                  if (pinnedLengthRef.current === 0)
                    setShowPinnedSection(false);
                }}
              >
                {pinned.map((item) => (
                  <ItemCard
                    key={item.id}
                    item={item}
                    pinned
                    onToggle={() => togglePin(item.id)}
                  />
                ))}
              </AnimatePresence>
            </motion.div>
          )}
        </AnimatePresence>

        <AnimatePresence onExitComplete={() => setShowAllSection(false)}>
          {showAllSection && (
            <motion.div
              key="all-section"
              layout
              variants={headingVariants}
              initial="hidden"
              animate="visible"
              exit="exit"
              className="flex flex-col gap-1"
            >
              <motion.p
                layout="position"
                className={cn(
                  "px-1 pb-0.5 text-sm font-medium text-muted-foreground",
                  pinned.length > 0 ? "pt-3 " : "pt-1",
                )}
              >
                All Items
              </motion.p>
              <AnimatePresence
                mode="popLayout"
                onExitComplete={() => {
                  if (unpinnedLengthRef.current === 0) setShowAllSection(false);
                }}
              >
                {unpinned.map((item) => (
                  <ItemCard
                    key={item.id}
                    item={item}
                    pinned={false}
                    onToggle={() => togglePin(item.id)}
                  />
                ))}
              </AnimatePresence>
            </motion.div>
          )}
        </AnimatePresence>
      </motion.div>
    </LayoutGroup>
  );
}
```

---

### Side By Side Slide <a name="side-by-side-slide"></a>

An image comparison slider where a spring-animated divider follows the cursor on hover, revealing before/after images.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/side-by-side-slide.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { SideBySideSlide } from "@/components/unlumen-ui/side-by-side-slide";

<SideBySideSlide
  beforeImage="/images/before.jpg"
  afterImage="/images/after.jpg"
  className="aspect-video rounded-xl"
/>;
```

#### Demo Code

```tsx
"use client";

import { SideBySideSlide } from "@/registry/components/unlumen/side-by-side-slide";

interface SideBySideSlideDemoProps {
  orientation: "horizontal" | "vertical";
  initialPosition: number;
  dividerColor: string;
  dividerWidth: number;
  showHandle: boolean;
  handleSize: number;
  handleColor: string;
  cursor: "none" | "col-resize" | "row-resize" | "pointer";
  stiffness: number;
  damping: number;
}

export default function SideBySideSlideDemo({
  orientation = "horizontal",
  initialPosition = 50,
  dividerColor = "white",
  dividerWidth = 2,
  showHandle = true,
  handleSize = 16,
  handleColor = "white",
  cursor = "none",
  stiffness = 300,
  damping = 30,
}: SideBySideSlideDemoProps) {
  return (
    <div className="flex items-center justify-center p-8">
      <div className="w-full max-w-2xl overflow-hidden rounded-xl">
        <SideBySideSlide
          beforeImage="https://urjypba3n2iozf8u.public.blob.vercel-storage.com/villa.png"
          afterImage="https://urjypba3n2iozf8u.public.blob.vercel-storage.com/villa-sketch.png"
          beforeAlt="Modern house exterior"
          afterAlt="Modern house interior"
          orientation={orientation}
          initialPosition={initialPosition}
          dividerColor={dividerColor}
          dividerWidth={dividerWidth}
          showHandle={showHandle}
          handleSize={handleSize}
          handleColor={handleColor}
          cursor={cursor}
          springOptions={{ stiffness, damping }}
          className="aspect-video"
        />
      </div>
    </div>
  );
}
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `beforeImage` | `string` | `-` | Source URL for the "before" image (left or top half). |
| `afterImage` | `string` | `-` | Source URL for the "after" image (right or bottom half). |
| `beforeAlt?` | `string` | `&quot;Before&quot;` | Alt text for the before image. |
| `afterAlt?` | `string` | `&quot;After&quot;` | Alt text for the after image. |
| `orientation?` | `&quot;horizontal&quot; \| &quot;vertical&quot;` | `&quot;horizontal&quot;` | Direction of the divider. Horizontal splits left/right; vertical splits top/bottom. |
| `initialPosition?` | `number` | `50` | Starting position of the divider as a percentage (0–100). The divider springs back to this value on mouse leave. |
| `dividerColor?` | `string` | `&quot;white&quot;` | CSS color value for the divider line. |
| `dividerWidth?` | `number` | `2` | Thickness of the divider line in pixels. |
| `dividerShadow?` | `string` | `&quot;0 0 8px rgba(0,0,0,0.3)&quot;` | CSS box-shadow applied to the divider line. |
| `showHandle?` | `boolean` | `true` | Whether to render the circular drag handle on the divider. |
| `handleSize?` | `number` | `40` | Diameter of the circular handle in pixels. |
| `handleColor?` | `string` | `&quot;white&quot;` | Background color of the circular handle. |
| `cursor?` | `&quot;none&quot; \| &quot;col-resize&quot; \| &quot;row-resize&quot; \| &quot;pointer&quot;` | `&quot;none&quot;` | CSS cursor style applied when hovering over the component. Use `none` to hide the cursor entirely. |
| `springOptions?` | `SpringOptions` | `{ stiffness: 300, damping: 30 }` | Motion spring config forwarded to `useSpring`. Controls how the divider accelerates and settles. |
| `className?` | `string` | `-` | Extra CSS classes applied to the root container. |

#### Component Source Code

##### File: `registry/components/unlumen/side-by-side-slide/index.tsx`

```tsx
"use client";

import * as React from "react";
import {
  motion,
  useMotionValue,
  useSpring,
  useTransform,
  type SpringOptions,
} from "motion/react";

import { cn } from "@/lib/utils";

export type SideBySideSlideProps = {
  /** Source URL for the "before" image (left / top). */
  beforeImage: string;
  /** Source URL for the "after" image (right / bottom). */
  afterImage: string;
  /** Alt text for the before image. */
  beforeAlt?: string;
  /** Alt text for the after image. */
  afterAlt?: string;
  /** Divider direction. */
  orientation?: "horizontal" | "vertical";
  /** Initial divider position as a percentage (0–100). */
  initialPosition?: number;
  /** CSS color of the divider line. */
  dividerColor?: string;
  /** Width (or height for vertical) of the divider in px. */
  dividerWidth?: number;
  /** Box-shadow applied to the divider. */
  dividerShadow?: string;
  /** Whether to show the circular handle on the divider. */
  showHandle?: boolean;
  /** Diameter of the handle circle in px. */
  handleSize?: number;
  /** Background color of the handle. */
  handleColor?: string;
  /** Cursor style when hovering over the component. */
  cursor?: "none" | "col-resize" | "row-resize" | "pointer";
  /** Spring configuration for the divider animation. */
  springOptions?: SpringOptions;
  /** Extra CSS classes on the root container. */
  className?: string;
};

export function SideBySideSlide({
  beforeImage,
  afterImage,
  beforeAlt = "Before",
  afterAlt = "After",
  orientation = "horizontal",
  initialPosition = 50,
  dividerColor = "white",
  dividerWidth = 2,
  dividerShadow = "0 0 8px rgba(0,0,0,0.3)",
  showHandle = true,
  handleSize = 40,
  handleColor = "white",
  cursor = "none",
  springOptions = { stiffness: 300, damping: 30 },
  className,
}: SideBySideSlideProps) {
  const ref = React.useRef<HTMLDivElement>(null);
  const isHorizontal = orientation === "horizontal";

  const raw = useMotionValue(initialPosition);
  const position = useSpring(raw, springOptions);

  const clipPath = useTransform(position, (v) =>
    isHorizontal ? `inset(0 ${100 - v}% 0 0)` : `inset(0 0 ${100 - v}% 0)`,
  );

  const dividerPos = useTransform(position, (v) => `${v}%`);

  const handleMouseMove = (e: React.MouseEvent<HTMLDivElement>) => {
    if (!ref.current) return;
    const rect = ref.current.getBoundingClientRect();
    const pct = isHorizontal
      ? ((e.clientX - rect.left) / rect.width) * 100
      : ((e.clientY - rect.top) / rect.height) * 100;
    raw.set(Math.max(0, Math.min(100, pct)));
  };

  const handleMouseLeave = () => {
    raw.set(initialPosition);
  };

  return (
    <div
      ref={ref}
      className={cn("relative select-none overflow-hidden", className)}
      style={{ cursor }}
      onMouseMove={handleMouseMove}
      onMouseLeave={handleMouseLeave}
    >
      {/* After image (bottom layer — in flow to give the container intrinsic height) */}
      {/* eslint-disable-next-line @next/next/no-img-element */}
      <img
        src={afterImage}
        alt={afterAlt}
        draggable={false}
        className="block h-full w-full object-cover"
      />

      {/* Before image (clipped top layer) */}
      <motion.div className="absolute inset-0" style={{ clipPath }}>
        {/* eslint-disable-next-line @next/next/no-img-element */}
        <img
          src={beforeImage}
          alt={beforeAlt}
          draggable={false}
          className="block h-full w-full object-cover"
        />
      </motion.div>

      {/* Divider line */}
      <motion.div
        className="absolute"
        style={
          isHorizontal
            ? {
                left: dividerPos,
                top: 0,
                bottom: 0,
                width: dividerWidth,
                x: "-50%",
                backgroundColor: dividerColor,
                boxShadow: dividerShadow,
              }
            : {
                top: dividerPos,
                left: 0,
                right: 0,
                height: dividerWidth,
                y: "-50%",
                backgroundColor: dividerColor,
                boxShadow: dividerShadow,
              }
        }
      >
        {/* Handle */}
        {showHandle && (
          <div
            className="absolute rounded-full flex items-center justify-center"
            style={
              isHorizontal
                ? {
                    width: handleSize,
                    height: handleSize,
                    top: "50%",
                    left: "50%",
                    transform: "translate(-50%, -50%)",
                    backgroundColor: handleColor,
                    boxShadow: dividerShadow,
                  }
                : {
                    width: handleSize,
                    height: handleSize,
                    left: "50%",
                    top: "50%",
                    transform: "translate(-50%, -50%)",
                    backgroundColor: handleColor,
                    boxShadow: dividerShadow,
                  }
            }
          ></div>
        )}
      </motion.div>
    </div>
  );
}
```

---

### Smart Animate Text <a name="smart-animate-text"></a>

Per-character blur-slide animation. Values update directly while changed digits and letters animate up, down, or dynamically.

- **Tier**: Pro
- **Category**: Animations

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { SmartAnimateText } from "@/components/unlumen-ui/smart-animate-text";

export default function Example() {
  return <SmartAnimateText value="mon." className="text-6xl font-bold" />;
}
```

---

### Stacked Feature Cards <a name="stacked-feature-cards"></a>

A two-column section with a sticky spring-animated hero card on the left and a stack of rotating feature cards on the right.

- **Tier**: Pro
- **Category**: Animations

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { StackedFeatureCards } from "@/components/unlumen-ui/stacked-feature-cards";
import { Leaf, Truck } from "lucide-react";

const heroCard = {
  imageSrc: "/product.png",
  imageAlt: "Product image",
  badge: "29 CHF · 250g",
  title: "Premium Blend",
  description: "A rich, full-bodied blend roasted fresh every week.",
  href: "/shop",
};

const featureCards = [
  {
    value: "01",
    title: "100% Organic",
    description: "Certified organic from seed to your cup.",
    icon: Leaf,
    cardClassName: "bg-green-50 text-green-950",
    iconClassName: "bg-green-100 text-green-700",
    rotateClassName: "rotate-[-1.2deg]",
  },
  {
    value: "02",
    title: "Free Shipping",
    description: "Orders over 40 CHF ship free in compostable packaging.",
    icon: Truck,
    cardClassName: "bg-violet-50 text-violet-950",
    iconClassName: "bg-violet-100 text-violet-700",
    rotateClassName: "rotate-[1deg]",
  },
];

export default function Example() {
  return (
    <StackedFeatureCards
      heroCard={heroCard}
      featureCards={featureCards}
      sectionTitle="Why Choose Us"
    />
  );
}
```

#### Props

##### `StackedFeatureCards` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `heroCard` | `HeroCard` | `-` | Data for the sticky hero card on the left. |
| `featureCards` | `FeatureCard[]` | `-` | Array of feature cards stacked on the right. |
| `sectionTitle?` | `string` | `-` | Optional eyebrow heading displayed above the grid. |
| `className?` | `string` | `-` | Extra classes applied to the outer section element. |

##### `HeroCard` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `imageSrc` | `string` | `-` | URL of the floating product image. |
| `imageAlt` | `string` | `-` | Alt text for the product image. |
| `badge` | `string` | `-` | Short pill label (e.g. price or weight). |
| `title` | `string` | `-` | Main heading inside the hero card. |
| `description` | `string` | `-` | Short body copy below the heading. |
| `href` | `string` | `-` | Link target for the whole card. |

##### `FeatureCard` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `value` | `string` | `-` | Monospace number label (e.g. "01"). |
| `title` | `string` | `-` | Card heading. |
| `description` | `string` | `-` | One-to-two sentence body copy. |
| `icon` | `React.ElementType` | `-` | Any React component that renders an SVG icon (e.g. a Lucide icon). |
| `cardClassName?` | `string` | `-` | Background and text color classes for the card. |
| `iconClassName?` | `string` | `-` | Background and icon color classes for the icon wrapper. |
| `rotateClassName?` | `string` | `-` | Tailwind rotate utility for the subtle tilt (e.g. "rotate-[-1.2deg]"). |

---

### Team Stack <a name="team-stack"></a>

A stacked deck of team member cards that fans out on hover (desktop) or tap (mobile), with spring-animated CTA pills and deterministic jitter for a natural pile feel.

- **Tier**: Pro
- **Category**: Animations

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { TeamStack } from "@/components/unlumen-ui/team-stack";

export default function Page() {
  return <TeamStack />;
}
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `name` | `string` | `-` | Member name displayed on the card. |
| `role` | `string` | `-` | Job title or role shown above the name. |
| `description` | `string` | `-` | Short bio shown below the name. |
| `imageSrc` | `string` | `-` | Path or URL to the member's avatar/photo. |
| `imageAlt` | `string` | `-` | Alt text for the image. |
| `isFeatured?` | `boolean` | `-` | When true, renders the card with an inverted (dark) background so it stands out as the hero card. |
| `ctaLabel?` | `string` | `-` | Label for the CTA pill. When omitted, no pill is rendered. |
| `ctaHref?` | `string` | `-` | Navigation target for the CTA pill. Also makes the entire card clickable. |

---

### Tilt Card <a name="tilt-card"></a>

A card with 3D spring tilt, optional floating image, badge and radial shine on hover.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/tilt-card.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { TiltCard } from "@/components/unlumen-ui/tilt-card";

<TiltCard
  title="Starter Kit"
  description="Everything you need to ship fast."
  price="Free"
  badgeLabel="Popular"
  imageSrc="/preview.png"
  href="/templates/starter"
/>;
```

#### Demo Code

```tsx
"use client";

import {
  TiltCard,
  type TiltCardProps,
} from "@/registry/components/unlumen/tilt-card";

type TiltCardDemoProps = Pick<TiltCardProps, "badgeVariant"> & {
  rotationFactor?: number;
};

export const TiltCardDemo = ({
  badgeVariant = "success",
  rotationFactor = 11,
}: TiltCardDemoProps) => {
  return (
    <div className="flex w-full flex-wrap gap-6 items-center justify-center p-10 max-w-xl">
      {/* Split badge + image */}
      <div className="w-full">
        <TiltCard
          title="Starter Kit"
          description="Everything you need to ship a modern SaaS product fast."
          price="Free"
          badgeLabel="Popular"
          badgeVariant={badgeVariant}
          imageSrc="https://images.unsplash.com/photo-1707978813846-dbf6c4dfbbe3?q=80&w=1065&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
          imageAlt="dashboard screenshot"
          href="#"
          tiltProps={{ rotationFactor }}
        />
      </div>

      {/* Price only badge + image */}
      <div className="w-full">
        <TiltCard
          title="Pro Template"
          description="Advanced features for power users and growing teams."
          price="$49"
          imageSrc="https://images.unsplash.com/photo-1759834857095-4e172a6d03f5?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
          imageAlt="analytics"
          href="#"
          tiltProps={{ rotationFactor }}
        />
      </div>

      {/* Split badge, warning, children */}
      <div className="w-full">
        <TiltCard
          title="Enterprise"
          description="SSO, audit logs, and dedicated infrastructure."
          price="$199"
          badgeLabel="Best value"
          badgeVariant="warning"
          href="#"
          tiltProps={{ rotationFactor }}
        >
          <ul className="text-xs text-muted-foreground space-y-1 mt-1">
            <li>✓ Unlimited seats</li>
            <li>✓ Priority support</li>
            <li>✓ Custom domain</li>
          </ul>
        </TiltCard>
      </div>
    </div>
  );
};
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `title` | `string` | `-` | Card heading. |
| `description?` | `string` | `-` | Subtitle text. |
| `price?` | `string` | `-` | Price label — shown as the left half of the split badge pill. |
| `badgeLabel?` | `string` | `-` | Secondary badge label (right half of the split pill). |
| `badgeVariant?` | `&quot;success&quot; \| &quot;warning&quot;` | `&quot;success&quot;` | Colour scheme for the right badge half. |
| `imageSrc?` | `string` | `-` | Image source URL — rendered floating bottom-right. |
| `imageAlt?` | `string` | `&quot;&quot;` | Alt text for the image. |
| `href?` | `string` | `-` | Wraps the card in an anchor tag. |
| `tiltProps?` | `Omit&lt;TiltProps, &quot;children&quot; \| &quot;className&quot;&gt;` | `-` | Props forwarded to the inner Tilt wrapper. |

#### Component Source Code

##### File: `registry/components/unlumen/tilt-card/index.tsx`

```tsx
"use client";

import * as React from "react";

import { cn } from "@/lib/utils";
import { ClippedCircle } from "@/components/unlumen-ui/clipped-circle";
import { Tilt, type TiltProps } from "@/components/unlumen-ui/tilt";

export interface TiltCardProps extends React.HTMLAttributes<HTMLDivElement> {
  title: string;
  description?: string;
  /** left half of the split badge pill; shown as a simple pill if `badgeLabel` is omitted */
  price?: string;
  /** right half of the split pill, coloured by `badgeVariant` */
  badgeLabel?: string;
  badgeVariant?: "success" | "warning";
  imageSrc?: string;
  imageAlt?: string;
  /** wraps the card in a plain `<a>` tag */
  href?: string;
  children?: React.ReactNode;
  tiltProps?: Omit<TiltProps, "children" | "className">;
}

const BADGE_LABEL_CLASSES: Record<
  NonNullable<TiltCardProps["badgeVariant"]>,
  string
> = {
  success: "bg-emerald-500/15 text-emerald-700 dark:text-emerald-300",
  warning: "bg-amber-500/20 text-amber-700 dark:text-amber-300",
};

export function TiltCard({
  title,
  description,
  price,
  badgeLabel,
  badgeVariant = "success",
  imageSrc,
  imageAlt = "",
  href,
  children,
  tiltProps,
  className,
  ...props
}: TiltCardProps) {
  const inner = (
    <Tilt
      rotationFactor={11}
      {...tiltProps}
      className={cn(
        "relative group overflow-hidden",
        "bg-background border border-border rounded-lg",
        "flex flex-col gap-4",
        "h-48 sm:h-52 md:h-56 w-full",
        "hover:shadow-lg hover:scale-105 transition-all duration-400 ease-out",
        className,
      )}
    >
      <div className="flex flex-row transition-all duration-200 justify-between px-4 sm:px-6 py-4 sm:py-5">
        <div className="flex flex-col gap-1 flex-1 mr-2">
          <h2 className="text-lg tracking-tight leading-tight font-medium">
            {title}
          </h2>
          {description && (
            <p className="text-foreground/50 text-sm">{description}</p>
          )}
          {children && <div className="mt-2">{children}</div>}
        </div>

        {price && badgeLabel ? (
          <div className="inline-flex h-fit items-center text-sm whitespace-nowrap shrink-0">
            <span className="rounded-l-full bg-secondary h-fit py-1 px-2 font-medium">
              {price}
            </span>
            <span
              className={cn(
                "rounded-r-full text-sm h-fit py-1 px-2 font-medium",
                BADGE_LABEL_CLASSES[badgeVariant],
              )}
            >
              {badgeLabel}
            </span>
          </div>
        ) : price ? (
          <span className="h-fit rounded-full bg-secondary px-3 py-1 text-sm font-medium whitespace-nowrap shrink-0">
            {price}
          </span>
        ) : null}
      </div>

      {imageSrc && (
        <img
          src={imageSrc}
          alt={imageAlt}
          width={288}
          height={224}
          loading="lazy"
          decoding="async"
          className={cn(
            "absolute z-10 top-27 w-72 -right-10",
            "rotate-[-5deg] border-border border rounded-md",
            "transition-transform duration-300 ease-out",
            "group-hover:-rotate-3 group-hover:-translate-y-1 group-hover:-translate-x-0.5",
          )}
        />
      )}

      <ClippedCircle circleClassName="bg-white" circleSize={800} />
    </Tilt>
  );

  if (href) {
    return (
      <a
        href={href}
        className="block cursor-pointer"
        {...(props as React.AnchorHTMLAttributes<HTMLAnchorElement>)}
      >
        {inner}
      </a>
    );
  }

  return <div {...props}>{inner}</div>;
}
```

---

### Vercel Snap Text <a name="vercel-snap-text"></a>

A scroll-driven text list that snaps exactly to each item. The active item scales up with an optional prefix, others fade and indent — all animated with Motion springs.

- **Tier**: Pro
- **Category**: Animations

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { VercelSnapText } from "@/components/unlumen-ui/vercel-snap-text";

<VercelSnapText
  items={["the platform", "the brand", "the future"]}
  prefix="We Design"
  className="w-full h-screen"
/>;
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `items` | `string[]` | `-` | List of strings to cycle through. |
| `prefix?` | `string` | `&quot;&quot;` | Text prepended before the active item label (e.g. "We Design"). |
| `itemHeight?` | `number` | `96` | Height of each item row in pixels. All rows share this height — it is the snap unit. |
| `stiffness?` | `number` | `200` | Spring stiffness for the track animation. Higher = snappier. |
| `damping?` | `number` | `28` | Spring damping for the track animation. Higher = less bounce. |
| `className?` | `string` | `-` | Additional CSS classes. Set a height (e.g. h-screen) on the root. |

---

### Youtube Video Ambient <a name="video-ambient"></a>

A video player with real-time ambient glow that mirrors the colors playing — just like YouTube's ambient mode.

- **Tier**: Free
- **Category**: Animations

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/video-ambient.json
```

#### How to Use

```tsx
import { VideoAmbient } from "@/components/unlumen-ui/video-ambient";

export default function Page() {
  return <VideoAmbient src="/your-video.mp4" poster="/your-poster.jpg" />;
}
```

#### Demo Code

```tsx
"use client";

import { VideoAmbient } from "@/registry/components/unlumen/video-ambient";

const VIDEO_SRC =
  "https://download.blender.org/peach/bigbuckbunny_movies/BigBuckBunny_320x180.mp4";
const VIDEO_POSTER =
  "https://peach.blender.org/wp-content/uploads/title_anouncement.jpg?x11217";

export default function VideoAmbientDemo({
  blurAmount = 70,
  intensity = 0.9,
}: {
  blurAmount?: number;
  intensity?: number;
}) {
  return (
    <div className="flex flex-col text-foreground">
      {/* Video */}
      <div className="w-full py-8 px-4">
        <VideoAmbient
          src={VIDEO_SRC}
          poster={VIDEO_POSTER}
          blurAmount={blurAmount}
          intensity={intensity}
          autoPlay
          muted
          className="max-w-5xl mx-auto"
        />
      </div>

      {/* Content below */}
      <div className="w-full max-w-5xl mx-auto px-4 pb-12">
        {/* Title */}
        <h1 className="mt-2 text-xl font-semibold leading-snug">
          Big Buck Bunny 60fps 4K — Official Blender Foundation Short Film
        </h1>

        {/* Channel row + actions */}
        <div className="flex flex-wrap items-center gap-3 mt-3">
          <div className="flex items-center gap-2.5 flex-1 min-w-0">
            <div className="w-10 h-10 rounded-full bg-orange-600 flex items-center justify-center text-white text-xs font-bold shrink-0">
              BF
            </div>
            <div className="min-w-0">
              <div className="flex items-center gap-1">
                <span className="text-sm font-medium truncate">
                  The Official Blender Foundations
                </span>
              </div>
              <p className="text-xs text-muted-foreground">1.22M subscribers</p>
            </div>
            <button className="ml-1 bg-foreground text-background text-sm font-semibold px-4 py-2 rounded-full hover:bg-foreground/80 transition-colors shrink-0">
              Subscribe
            </button>
          </div>

          <div className="flex items-center gap-2 flex-wrap">
            {/* Like / dislike */}
            <div className="flex rounded-full bg-foreground/10 divide-x divide-border overflow-hidden">
              <button className="flex items-center gap-1.5 px-4 py-2 text-sm font-medium hover:bg-foreground/20 transition-colors">
                <svg
                  className="w-4 h-4"
                  fill="currentColor"
                  viewBox="0 0 24 24"
                  aria-hidden="true"
                >
                  <path d="M1 21h4V9H1v12zm22-11c0-1.1-.9-2-2-2h-6.31l.95-4.57.03-.32c0-.41-.17-.79-.44-1.06L14.17 1 7.59 7.59C7.22 7.95 7 8.45 7 9v10c0 1.1.9 2 2 2h9c.83 0 1.54-.5 1.84-1.22l3.02-7.05c.09-.23.14-.47.14-.73v-2z" />
                </svg>
                107k
              </button>
              <button className="px-3 py-2 hover:bg-foreground/20 transition-colors">
                <svg
                  className="w-4 h-4"
                  fill="currentColor"
                  viewBox="0 0 24 24"
                  aria-hidden="true"
                >
                  <path d="M15 3H6c-.83 0-1.54.5-1.84 1.22l-3.02 7.05c-.09.23-.14.47-.14.73v2c0 1.1.9 2 2 2h6.31l-.95 4.57-.03.32c0 .41.17.79.44 1.06L9.83 23l6.59-6.59c.36-.36.58-.86.58-1.41V5c0-1.1-.9-2-2-2zm4 0v12h4V3h-4z" />
                </svg>
              </button>
            </div>

            {/* Share */}
            <button className="flex items-center gap-1.5 px-4 py-2 rounded-full bg-foreground/10 text-sm font-medium hover:bg-foreground/20 transition-colors">
              <svg
                className="w-4 h-4"
                fill="currentColor"
                viewBox="0 0 24 24"
                aria-hidden="true"
              >
                <path d="M18 16.08c-.76 0-1.44.3-1.96.77L8.91 12.7c.05-.23.09-.46.09-.7s-.04-.47-.09-.7l7.05-4.11c.54.5 1.25.81 2.04.81 1.66 0 3-1.34 3-3s-1.34-3-3-3-3 1.34-3 3c0 .24.04.47.09.7L8.04 9.81C7.5 9.31 6.79 9 6 9c-1.66 0-3 1.34-3 3s1.34 3 3 3c.79 0 1.5-.31 2.04-.81l7.12 4.16c-.05.21-.08.43-.08.65 0 1.61 1.31 2.92 2.92 2.92s2.92-1.31 2.92-2.92S19.61 16.08 18 16.08z" />
              </svg>
              Share
            </button>

            {/* Save */}
            <button className="flex items-center gap-1.5 px-4 py-2 rounded-full bg-foreground/10 text-sm font-medium hover:bg-foreground/20 transition-colors">
              <svg
                className="w-4 h-4"
                fill="none"
                stroke="currentColor"
                strokeWidth={2}
                viewBox="0 0 24 24"
                aria-hidden="true"
              >
                <path
                  strokeLinecap="round"
                  strokeLinejoin="round"
                  d="M5 5a2 2 0 012-2h10a2 2 0 012 2v16l-7-3.5L5 21V5z"
                />
              </svg>
              Save
            </button>

            {/* Clip */}
            <button className="flex items-center gap-1.5 px-4 py-2 rounded-full bg-foreground/10 text-sm font-medium hover:bg-foreground/20 transition-colors">
              <svg
                className="w-4 h-4"
                fill="none"
                stroke="currentColor"
                strokeWidth={2}
                viewBox="0 0 24 24"
                aria-hidden="true"
              >
                <path
                  strokeLinecap="round"
                  strokeLinejoin="round"
                  d="M14.121 14.121L19 19m-7-7l7-7m-7 7l-2.879 2.879M12 12L9.121 9.121m0 5.758a3 3 0 10-4.243 4.243 3 3 0 004.243-4.243zm0-5.758a3 3 0 10-4.243-4.243 3 3 0 004.243 4.243z"
                />
              </svg>
              Clip
            </button>

            {/* More */}
            <button className="w-9 h-9 flex items-center justify-center rounded-full bg-foreground/10 hover:bg-foreground/20 transition-colors">
              <svg
                className="w-5 h-5"
                fill="currentColor"
                viewBox="0 0 24 24"
                aria-hidden="true"
              >
                <path d="M12 8c1.1 0 2-.9 2-2s-.9-2-2-2-2 .9-2 2 .9 2 2 2zm0 2c-1.1 0-2 .9-2 2s.9 2 2 2 2-.9 2-2-.9-2-2-2zm0 6c-1.1 0-2 .9-2 2s.9 2 2 2 2-.9 2-2-.9-2-2-2z" />
              </svg>
            </button>
          </div>
        </div>

        {/* Description */}
        <div className="mt-4 bg-foreground/8 hover:bg-foreground/12 transition-colors rounded-xl p-4 text-sm cursor-pointer">
          <p className="font-semibold">
            23M views &nbsp;&bull;&nbsp; 11 years ago
          </p>
          <p className="text-muted-foreground mt-1 line-clamp-2">
            UHD High Frame rate version of the iconic short film by Blender, Big
            Buck Bunny. More info:{" "}
            <span className="text-blue-400">
              https://studio.blender.org/projects/b…
            </span>{" "}
            Learn more about this UHD high frame-rate version at:{" "}
            <span className="text-blue-400">
              http://bbb3d.renderfarming.net
            </span>{" "}
            …
          </p>
          <span className="font-semibold text-sm mt-0.5 inline-block">
            Show more
          </span>
        </div>
      </div>
    </div>
  );
}
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `src` | `string` | `-` | Video file URL. |
| `poster?` | `string` | `-` | Poster image shown before playback starts. |
| `blurAmount?` | `number` | `60` | Blur radius of the glow layer in pixels. |
| `intensity?` | `number` | `0.85` | Opacity of the glow layer (0–1). |
| `autoPlay?` | `boolean` | `false` | Start playback automatically on load. Must be combined with `muted` — browsers block unmuted autoplay. |
| `muted?` | `boolean` | `false` | Mute the video. Required for `autoPlay` to work in all browsers. |
| `className?` | `string` | `-` | Additional class names on the wrapper element. |

#### Component Source Code

##### File: `registry/components/unlumen/video-ambient/index.tsx`

```tsx
"use client";

import { useRef, useEffect } from "react";
import { cn } from "@/lib/utils";

interface VideoAmbientProps {
  src: string;
  poster?: string;
  blurAmount?: number;
  intensity?: number;
  autoPlay?: boolean;
  muted?: boolean;
  className?: string;
}

// Low resolution for the canvas — the blur hides any pixel detail anyway.
const CANVAS_W = 64;
const CANVAS_H = 36;

export function VideoAmbient({
  src,
  poster,
  blurAmount = 60,
  intensity = 0.85,
  autoPlay = false,
  muted = false,
  className,
}: VideoAmbientProps) {
  const videoRef = useRef<HTMLVideoElement>(null);
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const rafRef = useRef<number>(0);

  useEffect(() => {
    const video = videoRef.current;
    const canvas = canvasRef.current;
    if (!video || !canvas) return;

    const ctx = canvas.getContext("2d");
    if (!ctx) return;

    const draw = () => {
      // Only paint when the video has pixel data to show.
      if (!video.paused || video.readyState >= 2) {
        try {
          ctx.drawImage(video, 0, 0, CANVAS_W, CANVAS_H);
        } catch {
          // Ignore cross-origin canvas restrictions; video playback should continue.
        }
      }
      rafRef.current = requestAnimationFrame(draw);
    };

    rafRef.current = requestAnimationFrame(draw);

    return () => {
      cancelAnimationFrame(rafRef.current);
    };
  }, []);

  return (
    <div className={cn("relative w-full", className)}>
      {/* Glow canvas — same approach as YouTube's ambient mode.
          Canvas drawImage() runs in the normal paint cycle, so adjacent
          elements repaint automatically without composite-layer tricks. */}
      <canvas
        ref={canvasRef}
        width={CANVAS_W}
        height={CANVAS_H}
        aria-hidden
        className="absolute pointer-events-none rounded-xl"
        style={{
          inset: 0,
          width: "100%",
          height: "100%",
          filter: `blur(${blurAmount}px)`,
          opacity: intensity,
          transform: "scale(1.08)",
          zIndex: 0,
        }}
      />
      {/* Main video */}
      <video
        ref={videoRef}
        src={src}
        poster={poster}
        controls
        playsInline
        preload="metadata"
        autoPlay={autoPlay}
        muted={muted}
        className="relative w-full rounded-xl"
        style={{ zIndex: 1 }}
      />
    </div>
  );
}
```

---

### Video Slider <a name="video-slider"></a>

A custom video player with an animated scrubber, drag-to-seek with pointer capture, hover time preview, and optional edge-stretch rail effect.

- **Tier**: Pro
- **Category**: Animations

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { VideoSlider } from "@/components/unlumen-ui/video-slider";

export default function Example() {
  return (
    <VideoSlider
      src="/videos/demo.mp4"
      poster="/videos/demo-poster.jpg"
      stretchEffect
    />
  );
}
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `src` | `string` | `-` | Video source URL. |
| `poster?` | `string` | `-` | Poster image shown before playback starts. |
| `className?` | `string` | `-` | Additional classes applied to the root wrapper element. |
| `stretchEffect?` | `boolean` | `true` | Enables the elastic rail deformation when dragging beyond left/right bounds. |

---

## Backgrounds & Shaders <a name="backgrounds-and-shaders"></a>

### Aurora Bars <a name="aurora-bars"></a>

Animated bars with aurora-inspired gradient effects and smooth transitions.

- **Tier**: Free
- **Category**: Backgrounds & Shaders

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/aurora-bars.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { AuroraBars } from "@/components/unlumen-ui/aurora-bars";

<AuroraBars />;
```

#### Demo Code

```tsx
"use client";

import {
  AuroraBars,
  type AuroraBarsProps,
} from "@/registry/components/unlumen/aurora-bars";

type AuroraBarsDemo = Pick<
  AuroraBarsProps,
  "barCount" | "speed" | "gap" | "blur" | "maxHeightRatio" | "minHeightRatio"
>;

export const AuroraBarsDemo = ({
  barCount = 24,
  speed = 0.5,
  gap = 3,
  blur = 18,
  maxHeightRatio = 0.92,
  minHeightRatio = 0.18,
}: AuroraBarsDemo) => {
  return (
    <AuroraBars
      barCount={barCount}
      speed={speed}
      gap={gap}
      blur={blur}
      maxHeightRatio={maxHeightRatio}
      minHeightRatio={minHeightRatio}
      className="w-full h-full"
    />
  );
};
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `barCount?` | `number` | `24` | Number of vertical bars rendered. |
| `colors?` | `string[]` | `[&quot;#ffd6eb&quot;,&quot;#ff9acb&quot;,&quot;#ff5aa6&quot;,&quot;#ff2d78&quot;,&quot;#00000000&quot;]` | Gradient color stops applied bottom → top. |
| `maxHeightRatio?` | `number` | `0.92` | Maximum bar height as fraction of container (0–1). |
| `minHeightRatio?` | `number` | `0.18` | Minimum bar height as fraction of container (0–1). |
| `speed?` | `number` | `0.5` | Animation speed multiplier (higher = faster). |
| `gap?` | `number` | `3` | Gap between bars in pixels. |
| `blur?` | `number` | `0` | Blur applied to each bar in pixels (soft glow). |
| `background?` | `string` | `&quot;#000000&quot;` | Background color behind all bars. |
| `className?` | `string` | `-` | Additional className for the outer wrapper. |

#### Component Source Code

##### File: `registry/components/unlumen/aurora-bars/index.tsx`

```tsx
"use client";

import * as React from "react";
import { motion, useAnimationFrame } from "motion/react";

import { cn } from "@/lib/utils";

export interface AuroraBarsProps {
  /** @default 24 */
  barCount?: number;
  /** gradient color stops, bottom to top — @default ["#ff2d78", "#c04aff", "#4a6fff", "#0a1aff", "#00000000"] */
  colors?: string[];
  /** max bar height as fraction of container height — @default 0.92 */
  maxHeightRatio?: number;
  /** min bar height as fraction of container height — @default 0.18 */
  minHeightRatio?: number;
  /** undulation speed — @default 0.5 */
  speed?: number;
  /** @default 3 */
  gap?: number;
  /** px blur per bar, creates soft glow — @default 0 */
  blur?: number;
  /** @default "#000000" */
  background?: string;
  className?: string;
}

/** two sine waves per bar for organic movement */
function barHeight(
  index: number,
  total: number,
  time: number,
  minH: number,
  maxH: number,
): number {
  // Arch envelope: tallest in the centre, shorter on edges
  // arch envelope: tallest in centre, shorter on edges
  const norm = index / (total - 1);
  const arch = Math.sin(norm * Math.PI);

  const phase1 = (index / total) * Math.PI * 2;
  const phase2 = (index / total) * Math.PI * 5.3;

  const wave =
    0.5 +
    0.25 * Math.sin(time * 1.1 + phase1) +
    0.25 * Math.sin(time * 0.7 + phase2);

  const blended = arch * 0.65 + wave * 0.35;

  return minH + blended * (maxH - minH);
}

export function AuroraBars({
  barCount = 24,
  colors = ["#ffd6eb", "#ff9acb", "#ff5aa6", "#ff2d78", "#00000000"],
  maxHeightRatio = 0.92,
  minHeightRatio = 0.18,
  speed = 0.5,
  gap = 3,
  blur = 0,
  background = "#000000",
  className,
}: AuroraBarsProps) {
  const containerRef = React.useRef<HTMLDivElement>(null);
  const [heights, setHeights] = React.useState<number[]>(() =>
    Array.from({ length: barCount }, (_, i) =>
      barHeight(i, barCount, 0, minHeightRatio, maxHeightRatio),
    ),
  );

  const timeRef = React.useRef(0);

  useAnimationFrame((_, delta) => {
    timeRef.current += (delta / 1000) * speed;
    const t = timeRef.current;
    setHeights(
      Array.from({ length: barCount }, (_, i) =>
        barHeight(i, barCount, t, minHeightRatio, maxHeightRatio),
      ),
    );
  });

  const gradientStop = colors
    .map((c, i) => `${c} ${Math.round((i / (colors.length - 1)) * 100)}%`)
    .join(", ");
  const gradient = `linear-gradient(to top, ${gradientStop})`;

  return (
    <div
      ref={containerRef}
      className={cn("relative w-full h-full overflow-hidden", className)}
      style={{ background }}
    >
      <div className="absolute inset-0 flex items-end">
        {Array.from({ length: barCount }).map((_, i) => {
          const heightFraction = heights[i] ?? maxHeightRatio;
          return (
            <div
              key={i}
              className="flex-1"
              style={{
                height: "100%",
                display: "flex",
                alignItems: "flex-end",
                padding: `0 ${gap / 2}px`,
              }}
            >
              <motion.div
                style={{
                  width: "100%",
                  height: `${heightFraction * 100}%`,
                  background: gradient,
                  borderRadius: "9999px 9999px 0 0",
                  filter: `blur(${blur}px)`,
                  opacity: 0.85,
                }}
              />
            </div>
          );
        })}
      </div>

      <div
        className="absolute inset-0 pointer-events-none"
        style={{
          background:
            "radial-gradient(ellipse 90% 80% at 50% 100%, transparent 40%, #000000cc 100%)",
        }}
      />
    </div>
  );
}
```

---

### Gravity Stars <a name="gravity-stars"></a>

Interactive canvas background with gravity-driven star particles, mouse attraction/repulsion, click-to-spawn, and star collisions.

- **Tier**: Pro
- **Category**: Backgrounds & Shaders

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { GravityStarsBackground } from "@/components/unlumen-ui/gravity-stars";
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `starsCount?` | `number` | `75` | Initial number of stars. |
| `starsSize?` | `number` | `2` | Maximum radius of each star in pixels. |
| `starsOpacity?` | `number` | `0.75` | Base opacity of the stars. |
| `starsColor?` | `string` | `&quot;#ffcad4&quot;` | Color of the stars (any CSS color). |
| `glowIntensity?` | `number` | `15` | Shadow blur intensity applied to each star. |
| `glowAnimation?` | `&#x27;instant&#x27; \| &#x27;ease&#x27; \| &#x27;spring&#x27;` | `&quot;ease&quot;` | How the glow responds to mouse proximity — instant snap, smooth ease, or spring physics. |
| `movementSpeed?` | `number` | `0.3` | Base movement speed of the stars. |
| `mouseInfluence?` | `number` | `100` | Radius in pixels around the cursor that affects stars. |
| `mouseGravity?` | `&#x27;attract&#x27; \| &#x27;repel&#x27;` | `&quot;attract&quot;` | Whether the cursor attracts or repels stars. |
| `gravityStrength?` | `number` | `75` | Strength of the gravitational force applied to stars. |
| `starsInteraction?` | `boolean` | `false` | Whether stars interact with each other on collision. |
| `starsInteractionType?` | `&#x27;bounce&#x27; \| &#x27;merge&#x27;` | `&quot;bounce&quot;` | Collision behavior between stars — rigid bounce or soft merge with glow. |
| `...props?` | `React.ComponentProps&lt;&quot;div&quot;&gt;` | `-` | All other props are forwarded to the root div. |

---

### Pixel Background <a name="pixel"></a>

An interactive background featuring tiny pixel particles that ripple outward from the center on hover, then fade away.

- **Tier**: Pro
- **Category**: Backgrounds & Shaders

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { PixelBackground } from "@/components/backgrounds/pixel";

export default function MyPage() {
  return (
    <PixelBackground>
      <div className="flex items-center justify-center h-screen">
        <h1>Your content here</h1>
      </div>
    </PixelBackground>
  );
}
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `gap?` | `number` | `5` | Gap in pixels between each particle on the canvas grid. |
| `speed?` | `number` | `35` | Animation speed from 0 (stopped) to 100 (maximum). |
| `pattern?` | `&quot;center&quot; \| &quot;top&quot; \| &quot;bottom&quot; \| &quot;left&quot; \| &quot;right&quot; \| &quot;diagonal&quot; \| &quot;ascend&quot; \| &quot;edges&quot; \| &quot;spiral&quot; \| &quot;cursor&quot; \| &quot;random&quot;` | `&quot;center&quot;` | Controls the reveal animation direction and style. |
| `darkColors?` | `string` | `&quot;#2a2a2a,#3b3b3b,#525252&quot;` | Comma-separated hex colors used in dark mode. |
| `lightColors?` | `string` | `&quot;#d4d4d4,#bdbdbd,#a3a3a3&quot;` | Comma-separated hex colors used in light mode. |
| `children?` | `React.ReactNode` | `-` | Content rendered on top of the pixel canvas. |
| `className?` | `string` | `-` | Additional CSS classes for the container. |

---

### Pixel Liquid Background <a name="pixel-liquid-bg"></a>

A full Navier-Stokes fluid simulation background with pixelation, Bayer dithering, film-grain noise, and auto-demo mode. Reacts to cursor movement.

- **Tier**: Free
- **Category**: Backgrounds & Shaders

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/pixel-liquid-bg.json
```

#### Dependencies

- **NPM Packages**:
  - `three`

#### How to Use

```tsx
import { PixelLiquidBg } from "@/components/unlumen-ui/components/backgrounds/pixel-liquid-bg";

<PixelLiquidBg className="w-full h-full" />;
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `darkPalette?` | `string[]` | `[&quot;#000000&quot;, &quot;#2a0020&quot;, &quot;#8c0f60&quot;, &quot;#e8227a&quot;, &quot;#ff85b3&quot;]` | Color stops used for the fluid gradient in dark mode. Passed from transparent to opaque left to right. |
| `lightPalette?` | `string[]` | `[&quot;#ffffff&quot;, &quot;#FD96E5&quot;, &quot;#F36AC3&quot;, &quot;#FE4396&quot;, &quot;#ff85b3&quot;]` | Color stops used for the fluid gradient in light mode. |
| `pixelSize?` | `number` | `18` | Size in pixels of each block in the pixelation post-process pass. |
| `resolution?` | `number` | `0.4` | Simulation resolution multiplier (0–1). Lower values improve performance at the cost of detail. |
| `mouseForce?` | `number` | `8` | Strength of the force applied to the fluid on cursor move. |
| `cursorSize?` | `number` | `110` | Radius of the cursor's influence on the fluid simulation. |
| `autoDemo?` | `boolean` | `true` | When true, an autonomous animation drives the fluid when the cursor has been idle for more than ~1.2 s. |
| `children?` | `React.ReactNode` | `-` | Content rendered on top of the fluid canvas via `z-10`. |

#### Component Source Code

##### File: `registry/components/backgrounds/pixel-liquid-bg/index.tsx`

```tsx
"use client";

import { useEffect, useRef } from "react";
import * as THREE from "three";
import { cn } from "@/lib/utils";

/*
  PixelLiquidBg — Navier-Stokes fluid sim with Bayer dithering, pixelation,
  film-grain noise, and auto-demo mode that yields to cursor interaction.
*/

const face_vert = /* glsl */ `
attribute vec3 position;
uniform vec2 px;
uniform vec2 boundarySpace;
varying vec2 uv;
precision highp float;
void main(){
  vec3 pos = position;
  vec2 scale = 1.0 - boundarySpace * 2.0;
  pos.xy = pos.xy * scale;
  uv = vec2(0.5) + pos.xy * 0.5;
  gl_Position = vec4(pos, 1.0);
}
`;

const line_vert = /* glsl */ `
attribute vec3 position;
uniform vec2 px;
precision highp float;
varying vec2 uv;
void main(){
  vec3 pos = position;
  uv = 0.5 + pos.xy * 0.5;
  vec2 n = sign(pos.xy);
  pos.xy = abs(pos.xy) - px * 1.0;
  pos.xy *= n;
  gl_Position = vec4(pos, 1.0);
}
`;

const mouse_vert = /* glsl */ `
precision highp float;
attribute vec3 position;
attribute vec2 uv;
uniform vec2 center;
uniform vec2 scale;
uniform vec2 px;
varying vec2 vUv;
void main(){
  vec2 pos = position.xy * scale * 2.0 * px + center;
  vUv = uv;
  gl_Position = vec4(pos, 0.0, 1.0);
}
`;

const advection_frag = /* glsl */ `
precision highp float;
uniform sampler2D velocity;
uniform float dt;
uniform bool isBFECC;
uniform vec2 fboSize;
uniform vec2 px;
varying vec2 uv;
void main(){
  vec2 ratio = max(fboSize.x, fboSize.y) / fboSize;
  if(isBFECC == false){
    vec2 vel = texture2D(velocity, uv).xy;
    vec2 uv2 = uv - vel * dt * ratio;
    vec2 newVel = texture2D(velocity, uv2).xy;
    gl_FragColor = vec4(newVel, 0.0, 0.0);
  } else {
    vec2 spot_new = uv;
    vec2 vel_old = texture2D(velocity, uv).xy;
    vec2 spot_old = spot_new - vel_old * dt * ratio;
    vec2 vel_new1 = texture2D(velocity, spot_old).xy;
    vec2 spot_new2 = spot_old + vel_new1 * dt * ratio;
    vec2 error = spot_new2 - spot_new;
    vec2 spot_new3 = spot_new - error / 2.0;
    vec2 vel_2 = texture2D(velocity, spot_new3).xy;
    vec2 spot_old2 = spot_new3 - vel_2 * dt * ratio;
    vec2 newVel2 = texture2D(velocity, spot_old2).xy;
    gl_FragColor = vec4(newVel2, 0.0, 0.0);
  }
}
`;

const color_frag = /* glsl */ `
precision highp float;
uniform sampler2D velocity;
uniform sampler2D palette;
uniform sampler2D uBayer;
uniform vec4 bgColor;
uniform float uTime;
uniform vec2 uRes;
uniform float uPixelSize;

varying vec2 uv;

float hash(vec2 p) {
  return fract(sin(dot(p, vec2(127.1, 311.7))) * 43758.5453);
}

float noise(vec2 p) {
  vec2 i = floor(p);
  vec2 f = fract(p);
  vec2 u = f * f * (3.0 - 2.0 * f);
  return mix(
    mix(hash(i), hash(i + vec2(1.0, 0.0)), u.x),
    mix(hash(i + vec2(0.0, 1.0)), hash(i + vec2(1.0, 1.0)), u.x),
    u.y
  );
}

void main(){
  vec2 pixGrid = uRes / uPixelSize;
  vec2 pixUV   = (floor(uv * pixGrid) + 0.5) / pixGrid;

  vec2 vel  = texture2D(velocity, pixUV).xy;
  float len = clamp(length(vel) * 2.2, 0.0, 1.0);

  vec2 bayerUV = (mod(floor(gl_FragCoord.xy), 4.0) + 0.5) / 4.0;
  float dither  = texture2D(uBayer, bayerUV).r - 0.5;

  float noiseVal = noise(uv * 6.0 + uTime * 0.15) * 0.06 - 0.03;

  float t = clamp(len + dither * 0.12 + noiseVal, 0.0, 1.0);

  vec3 fluidColor = texture2D(palette, vec2(t, 0.5)).rgb;
  vec3 col        = mix(bgColor.rgb, fluidColor, t);

  float grain = hash(gl_FragCoord.xy + vec2(uTime * 137.0, uTime * 91.0));
  col += (grain - 0.5) * 0.085;

  float alpha = mix(bgColor.a, 1.0, t);
  gl_FragColor = vec4(clamp(col, 0.0, 1.0), alpha);
}
`;

const divergence_frag = /* glsl */ `
precision highp float;
uniform sampler2D velocity;
uniform float dt;
uniform vec2 px;
varying vec2 uv;
void main(){
  float x0 = texture2D(velocity, uv - vec2(px.x, 0.0)).x;
  float x1 = texture2D(velocity, uv + vec2(px.x, 0.0)).x;
  float y0 = texture2D(velocity, uv - vec2(0.0, px.y)).y;
  float y1 = texture2D(velocity, uv + vec2(0.0, px.y)).y;
  float divergence = (x1 - x0 + y1 - y0) / 2.0;
  gl_FragColor = vec4(divergence / dt);
}
`;

const externalForce_frag = /* glsl */ `
precision highp float;
uniform vec2 force;
uniform vec2 center;
uniform vec2 scale;
uniform vec2 px;
varying vec2 vUv;
void main(){
  vec2 circle = (vUv - 0.5) * 2.0;
  float d = 1.0 - min(length(circle), 1.0);
  d *= d;
  gl_FragColor = vec4(force * d, 0.0, 1.0);
}
`;

const poisson_frag = /* glsl */ `
precision highp float;
uniform sampler2D pressure;
uniform sampler2D divergence;
uniform vec2 px;
varying vec2 uv;
void main(){
  float p0 = texture2D(pressure, uv + vec2(px.x * 2.0, 0.0)).r;
  float p1 = texture2D(pressure, uv - vec2(px.x * 2.0, 0.0)).r;
  float p2 = texture2D(pressure, uv + vec2(0.0, px.y * 2.0)).r;
  float p3 = texture2D(pressure, uv - vec2(0.0, px.y * 2.0)).r;
  float div = texture2D(divergence, uv).r;
  float newP = (p0 + p1 + p2 + p3) / 4.0 - div;
  gl_FragColor = vec4(newP);
}
`;

const pressure_frag = /* glsl */ `
precision highp float;
uniform sampler2D pressure;
uniform sampler2D velocity;
uniform vec2 px;
uniform float dt;
varying vec2 uv;
void main(){
  float p0 = texture2D(pressure, uv + vec2(px.x, 0.0)).r;
  float p1 = texture2D(pressure, uv - vec2(px.x, 0.0)).r;
  float p2 = texture2D(pressure, uv + vec2(0.0, px.y)).r;
  float p3 = texture2D(pressure, uv - vec2(0.0, px.y)).r;
  vec2 v      = texture2D(velocity, uv).xy;
  vec2 gradP  = vec2(p0 - p1, p2 - p3) * 0.5;
  v = v - gradP * dt;
  gl_FragColor = vec4(v, 0.0, 1.0);
}
`;

const viscous_frag = /* glsl */ `
precision highp float;
uniform sampler2D velocity;
uniform sampler2D velocity_new;
uniform float v;
uniform vec2 px;
uniform float dt;
varying vec2 uv;
void main(){
  vec2 old  = texture2D(velocity, uv).xy;
  vec2 new0 = texture2D(velocity_new, uv + vec2(px.x * 2.0, 0.0)).xy;
  vec2 new1 = texture2D(velocity_new, uv - vec2(px.x * 2.0, 0.0)).xy;
  vec2 new2 = texture2D(velocity_new, uv + vec2(0.0, px.y * 2.0)).xy;
  vec2 new3 = texture2D(velocity_new, uv - vec2(0.0, px.y * 2.0)).xy;
  vec2 newv = 4.0 * old + v * dt * (new0 + new1 + new2 + new3);
  newv /= 4.0 * (1.0 + v * dt);
  gl_FragColor = vec4(newv, 0.0, 0.0);
}
`;

type Uniforms = Record<string, { value: unknown }>;

const DEFAULT_DARK_PALETTE = [
  "#000000",
  "#2a0020",
  "#8c0f60",
  "#e8227a",
  "#ff85b3",
];
const DEFAULT_LIGHT_PALETTE = [
  "#ffffff",
  "#FD96E5",
  "#F36AC3",
  "#FE4396",
  "#ff85b3",
];

function writePaletteData(data: Uint8Array, stops: string[]) {
  const arr = stops.length === 1 ? [stops[0], stops[0]] : stops;
  for (let i = 0; i < arr.length; i++) {
    const c = new THREE.Color(arr[i]);
    data[i * 4] = Math.round(c.r * 255);
    data[i * 4 + 1] = Math.round(c.g * 255);
    data[i * 4 + 2] = Math.round(c.b * 255);
    data[i * 4 + 3] = 255;
  }
}

function makePaletteTexture(stops: string[]): THREE.DataTexture {
  const arr = stops.length === 1 ? [stops[0], stops[0]] : stops;
  const w = arr.length;
  const data = new Uint8Array(w * 4);
  writePaletteData(data, arr);
  const tex = new THREE.DataTexture(data, w, 1, THREE.RGBAFormat);
  tex.magFilter = THREE.LinearFilter;
  tex.minFilter = THREE.LinearFilter;
  tex.wrapS = THREE.ClampToEdgeWrapping;
  tex.wrapT = THREE.ClampToEdgeWrapping;
  tex.generateMipmaps = false;
  tex.needsUpdate = true;
  return tex;
}

function isDarkMode() {
  return document.documentElement.classList.contains("dark");
}

function getBgColor(dark: boolean) {
  return dark ? new THREE.Vector4(0, 0, 0, 0) : new THREE.Vector4(1, 1, 1, 0);
}

function makeBayerTexture(): THREE.DataTexture {
  const raw = [
    0, 136, 34, 170, 204, 68, 238, 102, 51, 187, 17, 153, 255, 119, 221, 85,
  ];
  const data = new Uint8Array(16 * 4);
  for (let i = 0; i < 16; i++) {
    data[i * 4] = raw[i];
    data[i * 4 + 1] = raw[i];
    data[i * 4 + 2] = raw[i];
    data[i * 4 + 3] = 255;
  }
  const tex = new THREE.DataTexture(data, 4, 4, THREE.RGBAFormat);
  tex.magFilter = THREE.NearestFilter;
  tex.minFilter = THREE.NearestFilter;
  tex.wrapS = THREE.RepeatWrapping;
  tex.wrapT = THREE.RepeatWrapping;
  tex.generateMipmaps = false;
  tex.needsUpdate = true;
  return tex;
}

class CommonGL {
  width = 1;
  height = 1;
  pixelRatio = 1;
  renderer: THREE.WebGLRenderer | null = null;
  clock: THREE.Clock | null = null;
  time = 0;
  delta = 0;
  container: HTMLElement | null = null;

  init(container: HTMLElement) {
    this.container = container;
    this.pixelRatio = 1;
    this.resize();
    this.renderer = new THREE.WebGLRenderer({ antialias: false, alpha: true });
    this.renderer.autoClear = false;
    this.renderer.setClearColor(0x000000, 0);
    this.renderer.setPixelRatio(this.pixelRatio);
    this.renderer.setSize(this.width, this.height, false);
    const el = this.renderer.domElement;
    el.style.width = "100%";
    el.style.height = "100%";
    el.style.display = "block";
    this.clock = new THREE.Clock();
    this.clock.start();
  }

  resize() {
    if (!this.container) return;
    const r = this.container.getBoundingClientRect();
    this.width = Math.max(1, Math.floor(r.width));
    this.height = Math.max(1, Math.floor(r.height));
    this.renderer?.setSize(this.width, this.height, false);
  }

  update() {
    if (!this.clock) return;
    this.delta = this.clock.getDelta();
    this.time += this.delta;
  }
}

class MouseGL {
  coords = new THREE.Vector2();
  coords_old = new THREE.Vector2();
  diff = new THREE.Vector2();
  mouseMoved = false;
  isInside = false;
  isAutoActive = false;
  autoIntensity = 2.0;
  timer: ReturnType<typeof setTimeout> | null = null;
  container: HTMLElement | null = null;
  onInteract: (() => void) | null = null;

  private _move = this._onMove.bind(this);
  private _leave = () => {
    this.isInside = false;
  };
  private _touch = this._onTouch.bind(this);

  init(container: HTMLElement) {
    this.container = container;
    window.addEventListener("mousemove", this._move);
    window.addEventListener("touchmove", this._touch, { passive: true });
    window.addEventListener("touchstart", this._touch, { passive: true });
    document.addEventListener("mouseleave", this._leave);
  }

  dispose() {
    window.removeEventListener("mousemove", this._move);
    window.removeEventListener("touchmove", this._touch);
    window.removeEventListener("touchstart", this._touch);
    document.removeEventListener("mouseleave", this._leave);
  }

  private _onMove(e: MouseEvent) {
    if (!this.container) return;
    const r = this.container.getBoundingClientRect();
    this.isInside =
      e.clientX >= r.left &&
      e.clientX <= r.right &&
      e.clientY >= r.top &&
      e.clientY <= r.bottom;
    if (!this.isInside) return;
    this.onInteract?.();
    this._set(e.clientX, e.clientY);
  }

  private _onTouch(e: TouchEvent) {
    if (e.touches.length !== 1) return;
    const t = e.touches[0];
    this.onInteract?.();
    this._set(t.clientX, t.clientY);
  }

  private _set(cx: number, cy: number) {
    if (!this.container) return;
    if (this.timer) clearTimeout(this.timer);
    const r = this.container.getBoundingClientRect();
    const nx = (cx - r.left) / r.width;
    const ny = (cy - r.top) / r.height;
    this.coords.set(nx * 2 - 1, -(ny * 2 - 1));
    this.mouseMoved = true;
    this.timer = setTimeout(() => {
      this.mouseMoved = false;
    }, 100);
  }

  setNormalized(x: number, y: number) {
    this.coords.set(x, y);
    this.mouseMoved = true;
  }

  update() {
    this.diff.subVectors(this.coords, this.coords_old);
    this.coords_old.copy(this.coords);
    if (this.coords_old.x === 0 && this.coords_old.y === 0) this.diff.set(0, 0);
    if (this.isAutoActive) this.diff.multiplyScalar(this.autoIntensity);
  }
}

class ShaderPass {
  scene: THREE.Scene;
  camera: THREE.Camera;
  material: THREE.RawShaderMaterial | null = null;
  geometry: THREE.BufferGeometry | null = null;
  uniforms: Uniforms;
  output: THREE.WebGLRenderTarget | null;
  renderer: () => THREE.WebGLRenderer | null;

  constructor(
    renderer: () => THREE.WebGLRenderer | null,
    vertShader: string,
    fragShader: string,
    uniforms: Uniforms,
    output: THREE.WebGLRenderTarget | null = null,
  ) {
    this.renderer = renderer;
    this.uniforms = uniforms;
    this.output = output;
    this.scene = new THREE.Scene();
    this.camera = new THREE.Camera();
    this.material = new THREE.RawShaderMaterial({
      vertexShader: vertShader,
      fragmentShader: fragShader,
      uniforms,
    });
    this.geometry = new THREE.PlaneGeometry(2, 2);
    this.scene.add(new THREE.Mesh(this.geometry, this.material));
  }

  render(to: THREE.WebGLRenderTarget | null = this.output) {
    const r = this.renderer();
    if (!r) return;
    r.setRenderTarget(to);
    r.render(this.scene, this.camera);
    r.setRenderTarget(null);
  }

  dispose() {
    this.material?.dispose();
    this.geometry?.dispose();
  }
}

class AutoDriver {
  enabled: boolean;
  speed: number;
  resumeDelay: number;
  current = new THREE.Vector2();
  target = new THREE.Vector2();
  lastTime = performance.now();
  private _tmp = new THREE.Vector2();
  private _mouse: MouseGL;
  private _getLastInteraction: () => number;

  constructor(
    mouse: MouseGL,
    getLastInteraction: () => number,
    speed = 0.4,
    resumeDelay = 1200,
  ) {
    this._mouse = mouse;
    this._getLastInteraction = getLastInteraction;
    this.speed = speed;
    this.resumeDelay = resumeDelay;
    this.enabled = true;
    this._pickTarget();
  }

  private _pickTarget() {
    this.target.set(
      (Math.random() * 2 - 1) * 0.8,
      (Math.random() * 2 - 1) * 0.8,
    );
  }

  update() {
    if (!this.enabled) return;
    const now = performance.now();
    const idleMs = now - this._getLastInteraction();
    if (idleMs < this.resumeDelay) {
      this._mouse.isAutoActive = false;
      return;
    }
    this._mouse.isAutoActive = true;
    const dt = Math.min((now - this.lastTime) / 1000, 0.05);
    this.lastTime = now;
    const dir = this._tmp.subVectors(this.target, this.current);
    const dist = dir.length();
    if (dist < 0.02) {
      this._pickTarget();
      return;
    }
    dir.normalize();
    this.current.addScaledVector(dir, Math.min(this.speed * dt, dist));
    this._mouse.setNormalized(this.current.x, this.current.y);
  }
}

interface SimOpts {
  resolution: number;
  mouse_force: number;
  cursor_size: number;
  dt: number;
  BFECC: boolean;
  isBounce: boolean;
  isViscous: boolean;
  viscous: number;
  iterations_viscous: number;
  iterations_poisson: number;
}

class FluidSim {
  opts: SimOpts;
  fboSize = new THREE.Vector2();
  cellScale = new THREE.Vector2();
  boundarySpace = new THREE.Vector2();
  fbos: Record<string, THREE.WebGLRenderTarget | null> = {};
  gl: CommonGL;
  mouse: MouseGL;

  advection!: { pass: ShaderPass; line: THREE.LineSegments };
  externalForce!: {
    scene: THREE.Scene;
    camera: THREE.Camera;
    mesh: THREE.Mesh;
  };
  viscousPass!: {
    pass: ShaderPass;
    output0: THREE.WebGLRenderTarget | null;
    output1: THREE.WebGLRenderTarget | null;
  };
  divergencePass!: ShaderPass;
  poissonPass!: {
    pass: ShaderPass;
    output0: THREE.WebGLRenderTarget | null;
    output1: THREE.WebGLRenderTarget | null;
  };
  pressurePass!: ShaderPass;

  constructor(gl: CommonGL, mouse: MouseGL, opts: Partial<SimOpts> = {}) {
    this.gl = gl;
    this.mouse = mouse;
    this.opts = {
      resolution: 0.4,
      mouse_force: 10,
      cursor_size: 100,
      dt: 0.011,
      BFECC: true,
      isBounce: false,
      isViscous: false,
      viscous: 30,
      iterations_viscous: 32,
      iterations_poisson: 32,
      ...opts,
    };
    this._calcSize();
    this._createFBOs();
    this._createPasses();
  }

  private _r = () => this.gl.renderer;

  private _calcSize() {
    const w = Math.max(1, Math.round(this.opts.resolution * this.gl.width));
    const h = Math.max(1, Math.round(this.opts.resolution * this.gl.height));
    this.cellScale.set(1 / w, 1 / h);
    this.fboSize.set(w, h);
  }

  private _makeFBO() {
    return new THREE.WebGLRenderTarget(this.fboSize.x, this.fboSize.y, {
      type: THREE.HalfFloatType,
      depthBuffer: false,
      stencilBuffer: false,
      minFilter: THREE.LinearFilter,
      magFilter: THREE.LinearFilter,
      wrapS: THREE.ClampToEdgeWrapping,
      wrapT: THREE.ClampToEdgeWrapping,
    });
  }

  private _createFBOs() {
    for (const n of ["vel_0", "vel_1", "vel_v0", "vel_v1", "div", "p0", "p1"])
      this.fbos[n] = this._makeFBO();
  }

  private _createPasses() {
    const { fbos, cellScale, fboSize, opts, _r: r } = this;

    const advUniforms: Uniforms = {
      boundarySpace: { value: cellScale },
      px: { value: cellScale },
      fboSize: { value: fboSize },
      velocity: { value: fbos.vel_0!.texture },
      dt: { value: opts.dt },
      isBFECC: { value: true },
    };
    const advPass = new ShaderPass(
      r,
      face_vert,
      advection_frag,
      advUniforms,
      fbos.vel_1,
    );
    const bGeo = new THREE.BufferGeometry();
    bGeo.setAttribute(
      "position",
      new THREE.BufferAttribute(
        new Float32Array([
          -1, -1, 0, -1, 1, 0, -1, 1, 0, 1, 1, 0, 1, 1, 0, 1, -1, 0, 1, -1, 0,
          -1, -1, 0,
        ]),
        3,
      ),
    );
    const bMat = new THREE.RawShaderMaterial({
      vertexShader: line_vert,
      fragmentShader: advection_frag,
      uniforms: advUniforms,
    });
    const bLine = new THREE.LineSegments(bGeo, bMat);
    advPass.scene.add(bLine);
    this.advection = { pass: advPass, line: bLine };

    const efScene = new THREE.Scene();
    const efCam = new THREE.Camera();
    const efMesh = new THREE.Mesh(
      new THREE.PlaneGeometry(1, 1),
      new THREE.RawShaderMaterial({
        vertexShader: mouse_vert,
        fragmentShader: externalForce_frag,
        blending: THREE.AdditiveBlending,
        depthWrite: false,
        uniforms: {
          px: { value: cellScale },
          force: { value: new THREE.Vector2() },
          center: { value: new THREE.Vector2() },
          scale: {
            value: new THREE.Vector2(opts.cursor_size, opts.cursor_size),
          },
        },
      }),
    );
    efScene.add(efMesh);
    this.externalForce = { scene: efScene, camera: efCam, mesh: efMesh };

    const viscPass = new ShaderPass(
      r,
      face_vert,
      viscous_frag,
      {
        boundarySpace: { value: cellScale },
        velocity: { value: fbos.vel_1!.texture },
        velocity_new: { value: fbos.vel_v0!.texture },
        v: { value: opts.viscous },
        px: { value: cellScale },
        dt: { value: opts.dt },
      },
      fbos.vel_v1,
    );
    this.viscousPass = {
      pass: viscPass,
      output0: fbos.vel_v0,
      output1: fbos.vel_v1,
    };

    this.divergencePass = new ShaderPass(
      r,
      face_vert,
      divergence_frag,
      {
        boundarySpace: { value: cellScale },
        velocity: { value: fbos.vel_v0!.texture },
        px: { value: cellScale },
        dt: { value: opts.dt },
      },
      fbos.div,
    );

    const poisPass = new ShaderPass(
      r,
      face_vert,
      poisson_frag,
      {
        boundarySpace: { value: cellScale },
        pressure: { value: fbos.p0!.texture },
        divergence: { value: fbos.div!.texture },
        px: { value: cellScale },
      },
      fbos.p1,
    );
    this.poissonPass = { pass: poisPass, output0: fbos.p0, output1: fbos.p1 };

    this.pressurePass = new ShaderPass(
      r,
      face_vert,
      pressure_frag,
      {
        boundarySpace: { value: cellScale },
        pressure: { value: fbos.p0!.texture },
        velocity: { value: fbos.vel_v0!.texture },
        px: { value: cellScale },
        dt: { value: opts.dt },
      },
      fbos.vel_0,
    );
  }

  resize() {
    this._calcSize();
    for (const k in this.fbos)
      this.fbos[k]!.setSize(this.fboSize.x, this.fboSize.y);
  }

  update(time: number) {
    const { opts, mouse, fbos } = this;
    const r = this.gl.renderer;
    if (!r) return;

    this.boundarySpace.copy(
      opts.isBounce ? new THREE.Vector2() : this.cellScale,
    );

    {
      const u = this.advection.pass.uniforms;
      u.dt.value = opts.dt;
      u.isBFECC.value = opts.BFECC;
      this.advection.line.visible = opts.isBounce;
      this.advection.pass.render();
    }

    {
      const mf = opts.mouse_force;
      const cs = opts.cursor_size;
      const cx = this.cellScale.x;
      const cy = this.cellScale.y;
      const clampedX = Math.min(
        Math.max(mouse.coords.x, -1 + cs * cx * 2 + cx * 2),
        1 - cs * cx * 2 - cx * 2,
      );
      const clampedY = Math.min(
        Math.max(mouse.coords.y, -1 + cs * cy * 2 + cy * 2),
        1 - cs * cy * 2 - cy * 2,
      );
      const u = (this.externalForce.mesh.material as THREE.RawShaderMaterial)
        .uniforms;
      u.force.value.set((mouse.diff.x / 2) * mf, (mouse.diff.y / 2) * mf);
      u.center.value.set(clampedX, clampedY);
      u.scale.value.set(cs, cs);
      r.setRenderTarget(fbos.vel_1);
      r.render(this.externalForce.scene, this.externalForce.camera);
      r.setRenderTarget(null);
    }

    let velFBO: THREE.WebGLRenderTarget | null = fbos.vel_1;
    if (opts.isViscous) {
      const { pass, output0, output1 } = this.viscousPass;
      const u = pass.uniforms;
      u.v.value = opts.viscous;
      u.dt.value = opts.dt;
      let fbo_in = output0,
        fbo_out = output1;
      for (let i = 0; i < opts.iterations_viscous; i++) {
        if (i % 2 === 0) {
          fbo_in = output0;
          fbo_out = output1;
        } else {
          fbo_in = output1;
          fbo_out = output0;
        }
        u.velocity_new.value = fbo_in!.texture;
        pass.render(fbo_out);
      }
      velFBO = fbo_out;
    }

    (
      this.divergencePass.uniforms as Uniforms & {
        velocity: { value: THREE.Texture };
      }
    ).velocity.value = velFBO!.texture;
    this.divergencePass.render();

    {
      const { pass, output0, output1 } = this.poissonPass;
      let p_in = output0,
        p_out = output1;
      for (let i = 0; i < opts.iterations_poisson; i++) {
        if (i % 2 === 0) {
          p_in = output0;
          p_out = output1;
        } else {
          p_in = output1;
          p_out = output0;
        }
        pass.uniforms.pressure.value = p_in!.texture;
        pass.render(p_out);
      }
      this.pressurePass.uniforms.pressure.value = p_out!.texture;
      this.pressurePass.uniforms.velocity.value = velFBO!.texture;
    }

    this.pressurePass.render();
    void time;
  }

  dispose() {
    for (const k in this.fbos) this.fbos[k]?.dispose();
    this.advection.pass.dispose();
    this.divergencePass.dispose();
    this.pressurePass.dispose();
  }
}

export interface PixelLiquidBgProps extends React.ComponentProps<"div"> {
  darkPalette?: string[];
  lightPalette?: string[];
  /** pixelation grid size in px */
  pixelSize?: number;
  /** sim resolution multiplier 0–1; lower = faster */
  resolution?: number;
  mouseForce?: number;
  cursorSize?: number;
  /** auto-moves fluid when idle, yields to cursor */
  autoDemo?: boolean;
  children?: React.ReactNode;
}

export function PixelLiquidBg({
  darkPalette = DEFAULT_DARK_PALETTE,
  lightPalette = DEFAULT_LIGHT_PALETTE,
  pixelSize = 18,
  resolution = 0.4,
  mouseForce = 8,
  cursorSize = 110,
  autoDemo = true,
  children,
  className,
  ...props
}: PixelLiquidBgProps) {
  const mountRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const container = mountRef.current;
    if (!container) return;

    const gl = new CommonGL();
    gl.init(container);
    container.prepend(gl.renderer!.domElement);

    const mouse = new MouseGL();
    mouse.init(container);
    mouse.autoIntensity = 2.4;

    const dark = isDarkMode();
    const palette = makePaletteTexture(dark ? darkPalette : lightPalette);
    const bayerTex = makeBayerTexture();

    const sim = new FluidSim(gl, mouse, {
      resolution,
      mouse_force: mouseForce,
      cursor_size: cursorSize,
      dt: 0.008,
      BFECC: false,
      isBounce: false,
      isViscous: false,
      iterations_poisson: 8,
    });

    const outputUniforms: Uniforms = {
      velocity: { value: sim.fbos.vel_0!.texture },
      palette: { value: palette },
      uBayer: { value: bayerTex },
      bgColor: { value: getBgColor(dark) },
      uTime: { value: 0 },
      uRes: { value: new THREE.Vector2(gl.width, gl.height) },
      uPixelSize: { value: pixelSize },
      boundarySpace: { value: new THREE.Vector2() },
      px: { value: new THREE.Vector2() },
    };
    const outputScene = new THREE.Scene();
    const outputCam = new THREE.Camera();
    const outputMesh = new THREE.Mesh(
      new THREE.PlaneGeometry(2, 2),
      new THREE.RawShaderMaterial({
        vertexShader: face_vert,
        fragmentShader: color_frag,
        transparent: true,
        depthWrite: false,
        uniforms: outputUniforms,
      }),
    );
    outputScene.add(outputMesh);

    const themeObserver = new MutationObserver(() => {
      const nowDark = isDarkMode();
      const stops = nowDark ? darkPalette : lightPalette;
      writePaletteData(palette.image.data as Uint8Array, stops);
      palette.needsUpdate = true;
      const bg = getBgColor(nowDark);
      (outputUniforms.bgColor.value as THREE.Vector4).set(
        bg.x,
        bg.y,
        bg.z,
        bg.w,
      );
    });
    themeObserver.observe(document.documentElement, {
      attributes: true,
      attributeFilter: ["class"],
    });

    let lastInteraction = performance.now();
    mouse.onInteract = () => {
      lastInteraction = performance.now();
    };
    const driver = autoDemo
      ? new AutoDriver(mouse, () => lastInteraction, 0.45, 1200)
      : null;

    const handleResize = () => {
      gl.resize();
      sim.resize();
      (outputUniforms.uRes.value as THREE.Vector2).set(gl.width, gl.height);
    };
    const ro = new ResizeObserver(handleResize);
    ro.observe(container);

    let raf = 0;
    let running = true;

    const loop = () => {
      if (!running) return;
      raf = requestAnimationFrame(loop);
      driver?.update();
      mouse.update();
      gl.update();
      outputUniforms.uTime.value = gl.time;
      sim.update(gl.time);
      const r = gl.renderer;
      if (r) {
        r.setRenderTarget(null);
        r.render(outputScene, outputCam);
      }
    };
    loop();

    const onVisibility = () => {
      if (document.hidden) {
        running = false;
        cancelAnimationFrame(raf);
      } else {
        running = true;
        loop();
      }
    };
    document.addEventListener("visibilitychange", onVisibility);

    return () => {
      running = false;
      cancelAnimationFrame(raf);
      ro.disconnect();
      themeObserver.disconnect();
      document.removeEventListener("visibilitychange", onVisibility);
      mouse.dispose();
      sim.dispose();
      palette.dispose();
      bayerTex.dispose();
      (outputMesh.material as THREE.Material).dispose();
      outputMesh.geometry.dispose();
      const canvas = gl.renderer?.domElement;
      gl.renderer?.dispose();
      if (canvas?.parentNode) canvas.parentNode.removeChild(canvas);
    };
  }, [
    darkPalette,
    lightPalette,
    pixelSize,
    resolution,
    mouseForce,
    cursorSize,
    autoDemo,
  ]);

  return (
    <div
      ref={mountRef}
      className={cn(
        "relative w-full h-full overflow-hidden bg-background",
        className,
      )}
      {...props}
    >
      {children && (
        <div className="relative z-10 w-full h-full">{children}</div>
      )}
    </div>
  );
}
```

---

### Square Pattern <a name="square-pattern"></a>

An interactive grid background where squares randomly light up with colorful accents and react to hover.

- **Tier**: Pro
- **Category**: Backgrounds & Shaders

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import SquarePattern from "@/components/unlumen-ui/square-pattern";

export default function Example() {
  return (
    <div className="relative w-full h-64">
      <SquarePattern className="absolute inset-0" gridSize={10} />
    </div>
  );
}
```

---

### Wave Background <a name="wave-background"></a>

A morphing SVG wave divider with scroll-driven morphing and ambient looping animation. Use it between page sections, as a card accent, or as a full-width transition.

- **Tier**: Pro
- **Category**: Backgrounds & Shaders

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { WaveBackground } from "@/components/unlumen-ui/wave-background";
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `fill?` | `string` | `&quot;var(--foreground)&quot;` | Fill color of the SVG wave shape. Accepts any CSS color or custom property. |
| `background?` | `string` | `&quot;transparent&quot;` | Background color of the container behind the wave. |
| `direction?` | `&quot;down&quot; \| &quot;up&quot;` | `&quot;down&quot;` | Whether the wave crest faces down (fills the bottom of the container) or up (fills the top, useful for closing a section). |
| `variant?` | `&quot;scroll&quot; \| &quot;ambient&quot;` | `&quot;scroll&quot;` | `"scroll"` morphs the wave as the element enters the viewport. `"ambient"` runs a continuous sine-wave loop. |
| `height?` | `number` | `120` | Height of the wave container in pixels. |
| `speed?` | `number` | `1` | Ambient mode only — speed multiplier for the looping animation. 1 = ~4 s cycle. |
| `amplitude?` | `number` | `28` | Ambient mode only — peak-to-trough amplitude of the wave in pixels. |
| `...props?` | `React.ComponentProps&lt;&quot;div&quot;&gt;` | `-` | All other props are forwarded to the root div. |

---

## Icons <a name="icons"></a>

### Cards View Icon <a name="cards-view-icon"></a>

Four squares in a 2×2 grid that reorder via Motion layout animation when active, signaling a card view mode.

- **Tier**: Free
- **Category**: Icons

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/cards-view-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { useState } from "react";
import { CardsViewIcon } from "@/components/unlumen-ui/cards-view-icon";

export default function Example() {
  const [isActive, setIsActive] = useState(false);

  return (
    <button onClick={() => setIsActive((prev) => !prev)}>
      <CardsViewIcon isActive={isActive} />
    </button>
  );
}
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `isActive` | `boolean` | `-` | When true, the four squares reorder from [TL,TR,BL,BR] to [TR,BR,TL,BL] via Motion layout animation. |
| `className?` | `string` | `-` | Extra classes applied to the root <span> element. |

#### Component Source Code

##### File: `registry/components/icons/cards-view-icon/index.tsx`

```tsx
"use client";

import { motion } from "motion/react";

const CARD_DELAY_STEP = 0.067;

// Active permutation: TR BR / TL BL (indices in a 2×2 grid 0-3)
const ACTIVE_ORDER = [1, 3, 0, 2];
const DEFAULT_ORDER = [0, 1, 2, 3];

export interface CardsViewIconProps {
  /** Whether the icon is in its active (animated) state. */
  isActive: boolean;
  /** Extra classes applied to the root element. */
  className?: string;
}

export function CardsViewIcon({ isActive, className }: CardsViewIconProps) {
  const order = isActive ? ACTIVE_ORDER : DEFAULT_ORDER;

  return (
    <motion.span
      className={`inline-grid h-3 w-3 place-content-center grid-cols-2 gap-[2.5px] ${className ?? ""}`}
      aria-hidden="true"
    >
      {order.map((id, i) => (
        <motion.span
          key={id}
          layout
          className="size-[4px] rounded-[1px] bg-current"
          animate={isActive ? { opacity: [0.65, 1, 1] } : { opacity: 1 }}
          transition={{
            duration: 0.42,
            delay: i === 0 ? 0 : i * CARD_DELAY_STEP,
            ease: "easeOut",
            layout: {
              duration: 0.45,
              delay: i === 0 ? 0 : i * CARD_DELAY_STEP,
              ease: "easeInOut",
            },
          }}
        />
      ))}
    </motion.span>
  );
}
```

---

### Compact View Icon <a name="compact-view-icon"></a>

Six dots in a 3×2 grid that reorder via Motion layout animation when active, signaling a compact view mode.

- **Tier**: Free
- **Category**: Icons

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/compact-view-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { useState } from "react";
import { CompactViewIcon } from "@/components/unlumen-ui/compact-view-icon";

export default function Example() {
  const [isActive, setIsActive] = useState(false);

  return (
    <button onClick={() => setIsActive((prev) => !prev)}>
      <CompactViewIcon isActive={isActive} />
    </button>
  );
}
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `isActive` | `boolean` | `-` | When true, the six dots reorder from [0,1,2,3,4,5] to [1,2,5,0,3,4] via Motion layout animation. |
| `className?` | `string` | `-` | Extra classes applied to the root <span> element. |

#### Component Source Code

##### File: `registry/components/icons/compact-view-icon/index.tsx`

```tsx
"use client";

import { motion } from "motion/react";

const COMPACT_DELAY_STEP = 0.018;

// Active permutation: B C F / A D E (indices in a 3×2 grid 0-5)
const ACTIVE_ORDER = [1, 2, 5, 0, 3, 4];
const DEFAULT_ORDER = [0, 1, 2, 3, 4, 5];

export interface CompactViewIconProps {
  /** Whether the icon is in its active (animated) state. */
  isActive: boolean;
  /** Extra classes applied to the root element. */
  className?: string;
}

export function CompactViewIcon({ isActive, className }: CompactViewIconProps) {
  const order = isActive ? ACTIVE_ORDER : DEFAULT_ORDER;

  return (
    <motion.span
      className={`inline-grid h-4 w-4 place-content-center grid-cols-3 gap-[2px] ${className ?? ""}`}
      aria-hidden="true"
    >
      {order.map((id, i) => (
        <motion.span
          key={id}
          layout
          className="size-[3px] rounded-[1px] bg-current"
          animate={isActive ? { opacity: [0.65, 1, 1] } : { opacity: 1 }}
          transition={{
            duration: 0.42,
            delay: i === 0 ? 0 : i * COMPACT_DELAY_STEP,
            ease: "easeOut",
            layout: {
              duration: 0.45,
              delay: i === 0 ? 0 : i * COMPACT_DELAY_STEP,
              ease: "easeInOut",
            },
          }}
        />
      ))}
    </motion.span>
  );
}
```

---

### List View Icon <a name="list-view-icon"></a>

Three horizontal lines that shuffle vertically when active, animated with Motion spring transitions.

- **Tier**: Free
- **Category**: Icons

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/list-view-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { useState } from "react";
import { ListViewIcon } from "@/components/unlumen-ui/list-view-icon";

export default function Example() {
  const [isActive, setIsActive] = useState(false);

  return (
    <button onClick={() => setIsActive((prev) => !prev)}>
      <ListViewIcon isActive={isActive} />
    </button>
  );
}
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `isActive` | `boolean` | `-` | When true, the three lines animate to their shuffled positions. When false they return to the resting state. |
| `className?` | `string` | `-` | Extra classes applied to the root <span> element. |

#### Component Source Code

##### File: `registry/components/icons/list-view-icon/index.tsx`

```tsx
"use client";

import { motion } from "motion/react";

const LINE_DELAY_STEP = 0.02;
const LINE_STEP = 5; // 2px line + 3px gap

const LINE_TRAVEL: Record<number, number> = {
  0: LINE_STEP,
  1: LINE_STEP,
  2: -LINE_STEP * 2,
};

export interface ListViewIconProps {
  /** Whether the icon is in its active (animated) state. */
  isActive: boolean;
  /** Extra classes applied to the root element. */
  className?: string;
}

export function ListViewIcon({ isActive, className }: ListViewIconProps) {
  return (
    <motion.span
      className={`inline-flex h-4 w-4 flex-col items-center justify-center gap-[3px] ${className ?? ""}`}
      aria-hidden="true"
    >
      {[0, 1, 2].map((i) => (
        <motion.span
          key={i}
          className="h-[2px] w-3 rounded-full bg-current"
          animate={
            isActive
              ? { y: LINE_TRAVEL[i], opacity: [0.55, 1, 1] }
              : { y: 0, opacity: 0.9 }
          }
          transition={{
            duration: 0.36,
            delay: i === 0 ? 0 : i * LINE_DELAY_STEP,
            ease: "easeOut",
          }}
        />
      ))}
    </motion.span>
  );
}
```

---

### Sidebar Toggle Icon <a name="sidebar-toggle-icon"></a>

Animated SVG icon that morphs between open and closed sidebar panel states using Motion path interpolation.

- **Tier**: Free
- **Category**: Icons

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/sidebar-toggle-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { SidebarToggleIcon } from "@/components/unlumen-ui/sidebar-toggle-icon";

const [isOpen, setIsOpen] = useState(false);

<button onClick={() => setIsOpen((prev) => !prev)}>
  <SidebarToggleIcon isOpen={isOpen} />
</button>;
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `isOpen` | `boolean` | `-` | Controls which SVG path is rendered — open (wide panel) or closed (narrow column). |
| `strokeWidth?` | `number` | `1.5` | SVG stroke width applied to the outer border path. |
| `className?` | `string` | `-` | Extra classes applied to the <svg> element. |

#### Component Source Code

##### File: `registry/components/icons/sidebar-toggle-icon/index.tsx`

```tsx
"use client";

import { motion } from "motion/react";

// Outer rounded rectangle — fill + stroke, common to both states
const OUTER =
  "M11 3H13C16.7712 3 18.6569 3 19.8284 4.17157C21 5.34315 21 7.22876 21 11V13C21 16.7712 21 18.6569 19.8284 19.8284C18.6569 21 16.7712 21 13 21H11C7.2288 21 5.3431 21 4.1716 19.8284C3 18.6569 3 16.7712 3 13V11C3 7.22876 3 5.34315 4.1716 4.17157C5.3431 3 7.2288 3 11 3Z";

// Narrow left column (right edge at x = 10) — closed state
const PANEL_CLOSED =
  "M10 5.5 C10 4.793 10 4.439 9.780 4.220 C9.560 4 9.207 4 8.5 4 H8.5 C6.379 4 5.318 4 4.659 4.659 C4 5.318 4 6.379 4 8.5 V15.5 C4 17.621 4 18.682 4.659 19.341 C5.318 20 6.379 20 8.5 20 H8.5 C9.207 20 9.561 20 9.780 19.780 C10 19.561 10 19.207 10 18.5 V5.5 Z";

// Wide left panel (right edge at x = 14) — open state
const PANEL_OPEN =
  "M14 6 C14 5.057 14 4.586 13.707 4.293 C13.414 4 12.943 4 12 4 H10 C7.172 4 5.757 4 4.879 4.879 C4 5.757 4 7.172 4 10 V14 C4 16.828 4 18.243 4.879 19.121 C5.757 20 7.172 20 10 20 H12 C12.943 20 13.414 20 13.707 19.707 C14 19.414 14 18.943 14 18 V6 Z";

export interface SidebarToggleIconProps {
  /** Whether the sidebar panel is open. Controls the morphed path. */
  isOpen: boolean;
  /** SVG stroke width. @default 1.5 */
  strokeWidth?: number;
  /** Extra classes applied to the `<svg>` element. */
  className?: string;
}

export function SidebarToggleIcon({
  isOpen,
  strokeWidth = 1.5,
  className,
}: SidebarToggleIconProps) {
  return (
    <svg
      width="24"
      height="24"
      viewBox="0 0 24 24"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
      className={className}
    >
      {/* Outer border — foreground fill + stroke */}
      <path
        d={OUTER}
        fill="currentColor"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
      />

      {/* Inner panel — pure path morph, no opacity / translate tricks */}
      <motion.path
        d={isOpen ? PANEL_OPEN : PANEL_CLOSED}
        animate={{ d: isOpen ? PANEL_OPEN : PANEL_CLOSED }}
        style={{ fill: "var(--background)" }}
        transition={{ duration: 0.35, ease: [0.4, 0, 0.2, 1] }}
      />
    </svg>
  );
}
```

---

## Image Effects <a name="image-effects"></a>

### Horizontal Depth Fade <a name="horizontal-depth-fade"></a>

Scroll-driven horizontal strip with images on a single line, cinematic blur focus, and brightness falloff.

- **Tier**: Free
- **Category**: Image Effects

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/horizontal-depth-fade.json
```

#### Dependencies

- **NPM Packages**:
  - `gsap`

#### How to Use

```tsx
import { HorizontalDepthFade } from "@/components/unlumen-ui/horizontal-depth-fade";

const images = [
  { src: "/photos/1.jpg", alt: "Photo 1" },
  { src: "/photos/2.jpg", alt: "Photo 2" },
  { src: "/photos/3.jpg", alt: "Photo 3" },
  { src: "/photos/4.jpg", alt: "Photo 4" },
];

export default function Gallery() {
  return <HorizontalDepthFade images={images} />;
}
```

#### Demo Code

```tsx
"use client";

import { useEffect, useMemo, useRef, useState } from "react";
import Lenis from "lenis";

import {
  HorizontalDepthFade,
  type HorizontalDepthFadeProps,
} from "@/registry/components/unlumen/horizontal-depth-fade";

type HorizontalDepthFadeDemoProps = Pick<
  HorizontalDepthFadeProps,
  | "travel"
  | "blur"
  | "dim"
  | "brightnessBoost"
  | "darknessStrength"
  | "minSaturation"
  | "saturationStrength"
  | "focusSpread"
  | "scaleEffect"
  | "scrollSensitivity"
  | "gap"
  | "itemWidth"
  | "itemHeight"
  | "scrollLength"
> & {
  imageCount?: number;
  smooth?: boolean;
};

export default function HorizontalDepthFadeDemo({
  travel = 100,
  blur = 10,
  dim = 20,
  brightnessBoost = 55,
  darknessStrength = 1.35,
  minSaturation = 0,
  saturationStrength = 1.35,
  focusSpread = 0.14,
  scaleEffect = 0.11,
  scrollSensitivity = 0.6,
  gap = 24,
  itemWidth = 360,
  itemHeight = 460,
  scrollLength = 360,
  imageCount = 20,
  smooth = true,
}: HorizontalDepthFadeDemoProps) {
  const scrollContainerRef = useRef<HTMLDivElement>(null);
  const contentRef = useRef<HTMLDivElement>(null);
  const [previewWidth, setPreviewWidth] = useState(900);

  useEffect(() => {
    const node = scrollContainerRef.current;
    if (!node) return;

    const update = () => setPreviewWidth(node.clientWidth || 900);
    update();

    const observer = new ResizeObserver(update);
    observer.observe(node);

    return () => observer.disconnect();
  }, []);

  useEffect(() => {
    const wrapper = scrollContainerRef.current;
    const content = contentRef.current;
    if (!smooth || !wrapper || !content) return;

    const lenis = new Lenis({
      wrapper,
      content,
      smoothWheel: true,
      wheelMultiplier: 0.95,
      lerp: 0.09,
    });

    let rafId = 0;
    const raf = (time: number) => {
      lenis.raf(time);
      rafId = window.requestAnimationFrame(raf);
    };

    rafId = window.requestAnimationFrame(raf);

    return () => {
      window.cancelAnimationFrame(rafId);
      lenis.destroy();
    };
  }, [smooth]);

  const images = useMemo(() => {
    const targetWidth = Math.max(220, Math.round(itemWidth));
    const targetHeight = Math.max(260, Math.round(itemHeight));
    const qualityBias = Math.max(1, Math.round(previewWidth / 900));

    return Array.from({ length: imageCount }, (_, i) => ({
      src: `https://picsum.photos/seed/hbs${i + 1}/${targetWidth * qualityBias}/${targetHeight * qualityBias}`,
      alt: `Demo image ${i + 1}`,
    }));
  }, [imageCount, itemWidth, itemHeight, previewWidth]);

  const reloadKey = useMemo(
    () =>
      [
        travel,
        blur,
        dim,
        brightnessBoost,
        darknessStrength,
        minSaturation,
        saturationStrength,
        focusSpread,
        scaleEffect,
        scrollSensitivity,
        gap,
        itemWidth,
        itemHeight,
        scrollLength,
        imageCount,
        smooth,
      ].join("|"),
    [
      travel,
      blur,
      dim,
      brightnessBoost,
      darknessStrength,
      minSaturation,
      saturationStrength,
      focusSpread,
      scaleEffect,
      scrollSensitivity,
      gap,
      itemWidth,
      itemHeight,
      scrollLength,
      imageCount,
      smooth,
    ],
  );

  return (
    <div ref={scrollContainerRef} className="h-full w-full overflow-y-auto">
      <div ref={contentRef}>
        <div className="flex h-44 w-full items-center justify-center">
          <h1 className="text-center text-xl font-medium tracking-tight">
            Horizontal Depth Fade
          </h1>
        </div>

        <HorizontalDepthFade
          key={reloadKey}
          images={images}
          travel={travel}
          blur={blur}
          dim={dim}
          brightnessBoost={brightnessBoost}
          darknessStrength={darknessStrength}
          minSaturation={minSaturation}
          saturationStrength={saturationStrength}
          focusSpread={focusSpread}
          scaleEffect={scaleEffect}
          scrollSensitivity={scrollSensitivity}
          gap={gap}
          itemWidth={itemWidth}
          itemHeight={itemHeight}
          scrollLength={scrollLength}
          scrollContainerRef={scrollContainerRef}
        />
      </div>
    </div>
  );
}
```

#### Component Source Code

##### File: `registry/components/unlumen/horizontal-depth-fade/index.tsx`

```tsx
"use client";

import { useEffect, useRef, useState, type RefObject } from "react";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { cn } from "@/lib/utils";

gsap.registerPlugin(ScrollTrigger);

export interface HorizontalDepthFadeImage {
  src: string;
  alt?: string;
}

export interface HorizontalDepthFadeProps {
  /** Array of image sources to display in the strip. */
  images: HorizontalDepthFadeImage[];
  /** Horizontal travel amount in percent of the available strip overflow. @default 100 */
  travel?: number;
  /** Maximum blur amount (px) away from each image focus zone. @default 10 */
  blur?: number;
  /** Minimum brightness (%) away from each image focus zone. @default 20 */
  dim?: number;
  /** Extra brightness applied around the active focus zone. @default 45 */
  brightnessBoost?: number;
  /** Multiplier for out-of-focus darkening intensity. Values > 1 darken more aggressively. @default 1.35 */
  darknessStrength?: number;
  /** Minimum saturation (%) away from focus. Use `0` for full desaturation on edges. @default 0 */
  minSaturation?: number;
  /** Multiplier for desaturation intensity away from focus. Values > 1 desaturate faster. @default 1.35 */
  saturationStrength?: number;
  /** Focus zone width as normalized progress range. @default 0.16 */
  focusSpread?: number;
  /** Scale reduction amount away from focus. @default 0.09 */
  scaleEffect?: number;
  /** Scroll sensitivity multiplier. Lower values require longer scrolling. @default 0.6 */
  scrollSensitivity?: number;
  /** Gap between images. Accepts px number or CSS string. @default "1.5rem" */
  gap?: number | string;
  /** Image width in pixels. @default 360 */
  itemWidth?: number;
  /** Image height in pixels. @default 460 */
  itemHeight?: number;
  /** Height of the scroll section in viewport units. @default 280 */
  scrollLength?: number;
  /** Optional scrollable container element used as the animation scroller. */
  scrollContainerRef?: RefObject<HTMLElement | null>;
  /** Additional class name on the root element. */
  className?: string;
}

const DEFAULT_TRAVEL = 100;
const DEFAULT_BLUR = 10;
const DEFAULT_DIM = 20;
const DEFAULT_BRIGHTNESS_BOOST = 45;
const DEFAULT_DARKNESS_STRENGTH = 1.35;
const DEFAULT_MIN_SATURATION = 0;
const DEFAULT_SATURATION_STRENGTH = 1.35;
const DEFAULT_FOCUS_SPREAD = 0.16;
const DEFAULT_SCALE_EFFECT = 0.09;
const DEFAULT_SCROLL_SENSITIVITY = 0.6;
const DEFAULT_ITEM_WIDTH = 360;
const DEFAULT_ITEM_HEIGHT = 460;
const DEFAULT_SCROLL_LENGTH = 280;

function clamp(value: number, min: number, max: number) {
  return Math.min(max, Math.max(min, value));
}

function toCssLength(value: number | string | undefined, fallback: string) {
  if (typeof value === "number") return `${value}px`;
  return value ?? fallback;
}

function computeFocusIntensity(
  progress: number,
  index: number,
  total: number,
  influence = 0.2,
) {
  if (total <= 1) return 0;
  const safeInfluence = clamp(influence, 0.04, 1);
  const center = index / (total - 1);
  return clamp(Math.abs(progress - center) / safeInfluence, 0, 1);
}

function applyScrollSensitivity(progress: number, sensitivity: number) {
  const safeSensitivity = clamp(sensitivity, 0.25, 1.6);
  const exponent = 1 / safeSensitivity;
  return Math.pow(clamp(progress, 0, 1), exponent);
}

function useStripMetrics(
  viewportRef: RefObject<HTMLDivElement | null>,
  trackRef: RefObject<HTMLDivElement | null>,
) {
  const [maxShift, setMaxShift] = useState(0);

  useEffect(() => {
    const viewport = viewportRef.current;
    const track = trackRef.current;
    if (!viewport || !track) return;

    const update = () => {
      const overflow = Math.max(0, track.scrollWidth - viewport.clientWidth);
      setMaxShift(overflow);
    };

    update();

    const observer = new ResizeObserver(update);
    observer.observe(viewport);
    observer.observe(track);

    return () => observer.disconnect();
  }, [viewportRef, trackRef]);

  return maxShift;
}

export function HorizontalDepthFade({
  images,
  travel = DEFAULT_TRAVEL,
  blur = DEFAULT_BLUR,
  dim = DEFAULT_DIM,
  brightnessBoost = DEFAULT_BRIGHTNESS_BOOST,
  darknessStrength = DEFAULT_DARKNESS_STRENGTH,
  minSaturation = DEFAULT_MIN_SATURATION,
  saturationStrength = DEFAULT_SATURATION_STRENGTH,
  focusSpread = DEFAULT_FOCUS_SPREAD,
  scaleEffect = DEFAULT_SCALE_EFFECT,
  scrollSensitivity = DEFAULT_SCROLL_SENSITIVITY,
  gap = "1.5rem",
  itemWidth = DEFAULT_ITEM_WIDTH,
  itemHeight = DEFAULT_ITEM_HEIGHT,
  scrollLength = DEFAULT_SCROLL_LENGTH,
  scrollContainerRef,
  className,
}: HorizontalDepthFadeProps) {
  const sectionRef = useRef<HTMLElement>(null);
  const viewportRef = useRef<HTMLDivElement>(null);
  const trackRef = useRef<HTMLDivElement>(null);

  const boundedTravel = clamp(travel, 0, 100);
  const boundedBlur = clamp(blur, 0, 48);
  const boundedDim = clamp(dim, 0, 100);
  const boundedBrightnessBoost = clamp(brightnessBoost, 0, 120);
  const boundedDarknessStrength = clamp(darknessStrength, 0.2, 3);
  const boundedMinSaturation = clamp(minSaturation, 0, 100);
  const boundedSaturationStrength = clamp(saturationStrength, 0.2, 3);
  const boundedFocusSpread = clamp(focusSpread, 0.04, 1);
  const boundedScaleEffect = clamp(scaleEffect, 0, 0.25);
  const boundedScrollSensitivity = clamp(scrollSensitivity, 0.25, 1.6);
  const boundedScrollLength = clamp(scrollLength, 140, 600);
  const stripGap = toCssLength(gap, "1.5rem");

  const maxShift = useStripMetrics(viewportRef, trackRef);
  const effectiveShift = maxShift * (boundedTravel / 100);

  useEffect(() => {
    const section = sectionRef.current;
    const track = trackRef.current;
    if (!section || !track) return;

    const context = gsap.context(() => {
      const cards = Array.from(
        track.querySelectorAll<HTMLElement>(".hbs-item"),
      );

      const applyState = (rawProgress: number) => {
        const p = applyScrollSensitivity(rawProgress, boundedScrollSensitivity);

        gsap.set(track, {
          x: -effectiveShift * p,
        });

        cards.forEach((card, index) => {
          const intensity = computeFocusIntensity(
            p,
            index,
            cards.length,
            boundedFocusSpread,
          );
          const boostedIntensity = clamp(
            intensity * boundedDarknessStrength,
            0,
            1,
          );
          const currentBlur = boostedIntensity * boundedBlur;
          const peakBrightness = clamp(100 + boundedBrightnessBoost, 100, 220);
          const currentBrightness =
            boundedDim + (1 - boostedIntensity) * (peakBrightness - boundedDim);
          const boostedSaturationIntensity = clamp(
            intensity * boundedSaturationStrength,
            0,
            1,
          );
          const currentSaturation =
            boundedMinSaturation +
            (1 - boostedSaturationIntensity) * (100 - boundedMinSaturation);
          const currentScale = 1 - boostedIntensity * boundedScaleEffect;

          gsap.set(card, {
            filter: `blur(${currentBlur}px) brightness(${currentBrightness}%) saturate(${currentSaturation}%)`,
            scale: currentScale,
          });
        });
      };

      applyState(0);

      ScrollTrigger.create({
        trigger: section,
        scroller: scrollContainerRef?.current ?? undefined,
        start: "top top",
        end: "bottom bottom",
        scrub: true,
        onUpdate: (self) => {
          applyState(self.progress);
        },
      });
    }, sectionRef);

    ScrollTrigger.refresh();

    return () => context.revert();
  }, [
    scrollContainerRef,
    images,
    effectiveShift,
    boundedBlur,
    boundedDim,
    boundedBrightnessBoost,
    boundedDarknessStrength,
    boundedMinSaturation,
    boundedSaturationStrength,
    boundedFocusSpread,
    boundedScaleEffect,
    boundedScrollSensitivity,
  ]);

  return (
    <section
      ref={sectionRef}
      className={cn("relative w-full", className)}
      style={{ height: `${boundedScrollLength}vh` }}
    >
      <div
        ref={viewportRef}
        className="sticky top-0 h-screen w-full overflow-hidden"
      >
        <div className="flex h-full w-full items-center">
          <div
            ref={trackRef}
            className="hbs-track flex w-max items-center px-[8vw]"
            style={{ gap: stripGap }}
          >
            {images.map((img, i) => (
              <figure
                key={i}
                className="hbs-item relative z-10 m-0 shrink-0 overflow-hidden rounded-xl"
                style={{ width: itemWidth, height: itemHeight }}
              >
                <div
                  className="absolute inset-0 h-full w-full bg-cover bg-center"
                  style={{ backgroundImage: `url(${img.src})` }}
                  role="img"
                  aria-label={img.alt ?? `Image ${i + 1}`}
                />
              </figure>
            ))}
          </div>
        </div>
      </div>
    </section>
  );
}

export default HorizontalDepthFade;
```

---

### Orbital Image Wheel <a name="orbital-image-wheel"></a>

Scroll-driven half-wheel image layout pinned to the bottom area of the viewport, with GSAP focus effects and animated active captions.

- **Tier**: Free
- **Category**: Image Effects

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/orbital-image-wheel.json
```

#### Dependencies

- **NPM Packages**:
  - `gsap`

#### How to Use

```tsx
import { OrbitalImageWheel } from "@/components/unlumen-ui/orbital-image-wheel";

const images = [
  {
    src: "/photos/01.jpg",
    alt: "Capsule pack",
    label: "Capsule",
    subtitle: "Precision delivery",
  },
  {
    src: "/photos/02.jpg",
    alt: "Lab interior",
    label: "Lab",
    subtitle: "AI diagnostics",
  },
  {
    src: "/photos/03.jpg",
    alt: "Clinical scan",
    label: "Vision",
    subtitle: "Predictive screening",
  },
];

export default function Example() {
  return <OrbitalImageWheel images={images} />;
}
```

#### Demo Code

```tsx
"use client";

import { useEffect, useMemo, useRef, useState } from "react";
import Lenis from "lenis";

import {
  OrbitalImageWheel,
  type OrbitalImageWheelProps,
} from "@/registry/components/unlumen/orbital-image-wheel";

type OrbitalImageWheelDemoProps = Pick<
  OrbitalImageWheelProps,
  | "turns"
  | "blur"
  | "dim"
  | "brightnessBoost"
  | "darknessStrength"
  | "minSaturation"
  | "saturationStrength"
  | "focusSpread"
  | "scaleEffect"
  | "scrollSensitivity"
  | "itemWidth"
  | "itemHeight"
  | "wheelSize"
  | "cropRatio"
  | "scrollLength"
  | "captionOffset"
  | "showCaption"
  | "subtitleDirection"
  | "subtitleSpeed"
  | "subtitleStagger"
> & {
  imageCount?: number;
  smooth?: boolean;
};

const LABELS = [
  "Neural",
  "Capsule",
  "Vision",
  "Fusion",
  "Pulse",
  "Signal",
  "Orbit",
  "Clinic",
  "Vault",
  "Prism",
  "Quantum",
  "Nova",
];

const SUBTITLES = [
  "AI diagnostics",
  "Precision delivery",
  "Imaging stack",
  "Research pipeline",
  "Real-time monitoring",
  "Clinical workflow",
  "Predictive screening",
  "Data mesh",
  "Bioinformatics",
  "Automated QA",
  "Companion model",
  "Lab insights",
];

export default function OrbitalImageWheelDemo({
  turns = 4,
  blur = 4,
  dim = 40,
  brightnessBoost = 30,
  darknessStrength = 1.05,
  minSaturation = 55,
  saturationStrength = 0.6,
  focusSpread = 0.34,
  scaleEffect = 0.06,
  scrollSensitivity = 0.7,
  itemWidth = 220,
  itemHeight = 300,
  wheelSize = 2200,
  cropRatio = 0.75,
  scrollLength = 330,
  captionOffset = 8,
  showCaption = true,
  subtitleDirection = "top",
  subtitleSpeed = 1,
  subtitleStagger = 0.018,
  imageCount = 20,
  smooth = true,
}: OrbitalImageWheelDemoProps) {
  const scrollContainerRef = useRef<HTMLDivElement>(null);
  const contentRef = useRef<HTMLDivElement>(null);
  const [previewWidth, setPreviewWidth] = useState(900);

  useEffect(() => {
    const node = scrollContainerRef.current;
    if (!node) return;

    const update = () => setPreviewWidth(node.clientWidth || 900);
    update();

    const observer = new ResizeObserver(update);
    observer.observe(node);

    return () => observer.disconnect();
  }, []);

  useEffect(() => {
    const wrapper = scrollContainerRef.current;
    const content = contentRef.current;
    if (!smooth || !wrapper || !content) return;

    const lenis = new Lenis({
      wrapper,
      content,
      smoothWheel: true,
      wheelMultiplier: 0.95,
      lerp: 0.09,
    });

    let rafId = 0;
    const raf = (time: number) => {
      lenis.raf(time);
      rafId = window.requestAnimationFrame(raf);
    };

    rafId = window.requestAnimationFrame(raf);

    return () => {
      window.cancelAnimationFrame(rafId);
      lenis.destroy();
    };
  }, [smooth]);

  const images = useMemo(() => {
    const qualityBias = Math.max(1, Math.round(previewWidth / 900));
    const targetWidth = Math.max(220, Math.round(itemWidth));
    const targetHeight = Math.max(280, Math.round(itemHeight));

    return Array.from({ length: imageCount }, (_, i) => ({
      src: `https://picsum.photos/seed/oiw${i + 1}/${targetWidth * qualityBias}/${targetHeight * qualityBias}`,
      alt: `Orbital image ${i + 1}`,
      label: LABELS[i % LABELS.length],
      subtitle: SUBTITLES[i % SUBTITLES.length],
    }));
  }, [imageCount, itemWidth, itemHeight, previewWidth]);

  const reloadKey = useMemo(
    () =>
      [
        turns,
        blur,
        dim,
        brightnessBoost,
        darknessStrength,
        minSaturation,
        saturationStrength,
        focusSpread,
        scaleEffect,
        scrollSensitivity,
        itemWidth,
        itemHeight,
        wheelSize ?? "",
        cropRatio,
        scrollLength,
        captionOffset,
        showCaption,
        subtitleDirection,
        subtitleSpeed,
        subtitleStagger,
        imageCount,
        smooth,
      ].join("|"),
    [
      turns,
      blur,
      dim,
      brightnessBoost,
      darknessStrength,
      minSaturation,
      saturationStrength,
      focusSpread,
      scaleEffect,
      scrollSensitivity,
      itemWidth,
      itemHeight,
      wheelSize,
      cropRatio,
      scrollLength,
      captionOffset,
      showCaption,
      subtitleDirection,
      subtitleSpeed,
      subtitleStagger,
      imageCount,
      smooth,
    ],
  );

  return (
    <div ref={scrollContainerRef} className="h-full w-full overflow-y-auto">
      <div ref={contentRef}>
        <OrbitalImageWheel
          key={reloadKey}
          images={images}
          turns={turns}
          blur={blur}
          dim={dim}
          brightnessBoost={brightnessBoost}
          darknessStrength={darknessStrength}
          minSaturation={minSaturation}
          saturationStrength={saturationStrength}
          focusSpread={focusSpread}
          scaleEffect={scaleEffect}
          scrollSensitivity={scrollSensitivity}
          itemWidth={itemWidth}
          itemHeight={itemHeight}
          wheelSize={wheelSize}
          cropRatio={cropRatio}
          scrollLength={scrollLength}
          captionOffset={captionOffset}
          showCaption={showCaption}
          subtitleDirection={subtitleDirection}
          subtitleSpeed={subtitleSpeed}
          subtitleStagger={subtitleStagger}
          scrollContainerRef={scrollContainerRef}
        />
      </div>
    </div>
  );
}
```

#### Component Source Code

##### File: `registry/components/unlumen/orbital-image-wheel/index.tsx`

```tsx
"use client";

import {
  useCallback,
  useEffect,
  useLayoutEffect,
  useMemo,
  useRef,
  useState,
  type RefObject,
} from "react";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { cn } from "@/lib/utils";
import { MotionSubtitle } from "@/components/unlumen-ui/motion-subtitle";

gsap.registerPlugin(ScrollTrigger);

export interface OrbitalImageWheelImage {
  src: string;
  alt?: string;
  label?: string;
  subtitle?: string;
}

export interface OrbitalImageWheelProps {
  /** Images displayed around the wheel. */
  images: OrbitalImageWheelImage[];
  /** Number of full wheel turns during the scroll range. @default 4 */
  turns?: number;
  /** Maximum blur amount (px) away from the focus zone. @default 4 */
  blur?: number;
  /** Minimum brightness (%) away from focus. @default 40 */
  dim?: number;
  /** Extra brightness boost (%) around the active card. @default 30 */
  brightnessBoost?: number;
  /** Multiplier for out-of-focus darkening intensity. @default 1.05 */
  darknessStrength?: number;
  /** Minimum saturation (%) away from focus. @default 55 */
  minSaturation?: number;
  /** Multiplier for out-of-focus desaturation intensity. @default 0.6 */
  saturationStrength?: number;
  /** Focus zone width as normalized angular range. @default 0.34 */
  focusSpread?: number;
  /** Scale reduction amount away from focus. @default 0.06 */
  scaleEffect?: number;
  /** Scroll sensitivity multiplier. Lower values require longer scrolling. @default 0.7 */
  scrollSensitivity?: number;
  /** Card width in pixels. @default 220 */
  itemWidth?: number;
  /** Card height in pixels. @default 300 */
  itemHeight?: number;
  /** Optional fixed wheel diameter in pixels. Defaults to a responsive value based on viewport width. */
  wheelSize?: number;
  /** How much of the wheel sits below the viewport (0..1). `0.5` keeps only the top half visible. @default 0.75 */
  cropRatio?: number;
  /** Scroll section height in viewport units. @default 330 */
  scrollLength?: number;
  /** Bottom offset of the caption block in viewport units. @default 8 */
  captionOffset?: number;
  /** Show or hide the centered caption. @default true */
  showCaption?: boolean;
  /** Subtitle animation direction. @default "top" */
  subtitleDirection?: "top" | "bottom";
  /** Subtitle animation speed multiplier. @default 1 */
  subtitleSpeed?: number;
  /** Delay between subtitle character reveals in seconds. @default 0.018 */
  subtitleStagger?: number;
  /** Optional scrollable container element used as the animation scroller. */
  scrollContainerRef?: RefObject<HTMLElement | null>;
  /** Additional class name on the root element. */
  className?: string;
}

const DEFAULT_TURNS = 4;
const DEFAULT_BLUR = 4;
const DEFAULT_DIM = 40;
const DEFAULT_BRIGHTNESS_BOOST = 30;
const DEFAULT_DARKNESS_STRENGTH = 1.05;
const DEFAULT_MIN_SATURATION = 55;
const DEFAULT_SATURATION_STRENGTH = 0.6;
const DEFAULT_FOCUS_SPREAD = 0.34;
const DEFAULT_SCALE_EFFECT = 0.06;
const DEFAULT_SCROLL_SENSITIVITY = 0.7;
const DEFAULT_ITEM_WIDTH = 220;
const DEFAULT_ITEM_HEIGHT = 300;
const DEFAULT_SCROLL_LENGTH = 330;
const DEFAULT_CROP_RATIO = 0.75;
const DEFAULT_CAPTION_OFFSET = 15;
const DEFAULT_SUBTITLE_DIRECTION = "top";
const DEFAULT_SUBTITLE_SPEED = 1;
const DEFAULT_SUBTITLE_STAGGER = 0.018;

function clamp(value: number, min: number, max: number) {
  return Math.min(max, Math.max(min, value));
}

function shortestAngleDistance(a: number, b: number) {
  const full = Math.PI * 2;
  const raw = ((a - b + Math.PI) % full) - Math.PI;
  const normalized = raw < -Math.PI ? raw + full : raw;
  return Math.abs(normalized);
}

function applyScrollSensitivity(progress: number, sensitivity: number) {
  const safeSensitivity = clamp(sensitivity, 0.25, 1.6);
  const exponent = 1 / safeSensitivity;
  return Math.pow(clamp(progress, 0, 1), exponent);
}

function getFocusedImageIndexWithHysteresis(
  progress: number,
  total: number,
  turns: number,
  currentIndex: number,
  hysteresis = 0.18,
) {
  if (total <= 0 || turns <= 0) return 0;

  const phaseRaw = total * (0.25 + progress * turns);
  const phase = ((phaseRaw % total) + total) % total;

  if (currentIndex < 0) {
    return Math.round(phase) % total;
  }

  let next = currentIndex;
  let delta = phase - next;

  if (delta > total / 2) delta -= total;
  if (delta < -total / 2) delta += total;

  const threshold = 0.5 + clamp(hysteresis, 0, 0.35);

  while (delta > threshold) {
    next = (next + 1) % total;
    delta -= 1;
  }

  while (delta < -threshold) {
    next = (next - 1 + total) % total;
    delta += 1;
  }

  return next;
}

function getSnapProgressForIndex(
  index: number,
  total: number,
  turns: number,
  currentProgress: number,
) {
  if (total <= 0 || turns <= 0) return clamp(currentProgress, 0, 1);

  const safeIndex = ((index % total) + total) % total;
  const minCycle = Math.floor(-turns - 2);
  const maxCycle = Math.ceil(turns + 2);
  let nearest = clamp(currentProgress, 0, 1);
  let minDistance = Number.POSITIVE_INFINITY;

  for (let cycle = minCycle; cycle <= maxCycle; cycle += 1) {
    const progress = (safeIndex / total - 0.25 - cycle) / turns;
    if (progress < 0 || progress > 1) continue;

    const distance = Math.abs(progress - currentProgress);
    if (distance < minDistance) {
      minDistance = distance;
      nearest = progress;
    }
  }

  if (!Number.isFinite(minDistance)) {
    return clamp((safeIndex / total - 0.25) / turns, 0, 1);
  }

  return nearest;
}

function useViewportWidth(viewportRef: RefObject<HTMLDivElement | null>) {
  const [width, setWidth] = useState(1200);

  useEffect(() => {
    const viewport = viewportRef.current;
    if (!viewport) return;

    const update = () => setWidth(viewport.clientWidth || 1200);
    update();

    const observer = new ResizeObserver(update);
    observer.observe(viewport);

    return () => observer.disconnect();
  }, [viewportRef]);

  return width;
}

export function OrbitalImageWheel({
  images,
  turns = DEFAULT_TURNS,
  blur = DEFAULT_BLUR,
  dim = DEFAULT_DIM,
  brightnessBoost = DEFAULT_BRIGHTNESS_BOOST,
  darknessStrength = DEFAULT_DARKNESS_STRENGTH,
  minSaturation = DEFAULT_MIN_SATURATION,
  saturationStrength = DEFAULT_SATURATION_STRENGTH,
  focusSpread = DEFAULT_FOCUS_SPREAD,
  scaleEffect = DEFAULT_SCALE_EFFECT,
  scrollSensitivity = DEFAULT_SCROLL_SENSITIVITY,
  itemWidth = DEFAULT_ITEM_WIDTH,
  itemHeight = DEFAULT_ITEM_HEIGHT,
  wheelSize,
  cropRatio = DEFAULT_CROP_RATIO,
  scrollLength = DEFAULT_SCROLL_LENGTH,
  captionOffset = DEFAULT_CAPTION_OFFSET,
  showCaption = true,
  subtitleDirection = DEFAULT_SUBTITLE_DIRECTION,
  subtitleSpeed = DEFAULT_SUBTITLE_SPEED,
  subtitleStagger = DEFAULT_SUBTITLE_STAGGER,
  scrollContainerRef,
  className,
}: OrbitalImageWheelProps) {
  const sectionRef = useRef<HTMLElement>(null);
  const viewportRef = useRef<HTMLDivElement>(null);
  const wheelRef = useRef<HTMLDivElement>(null);
  const wheelScrollTriggerRef = useRef<ScrollTrigger | null>(null);
  const titleClickTweenRef = useRef<gsap.core.Tween | null>(null);
  const titleViewportRef = useRef<HTMLDivElement>(null);
  const titleTrackRef = useRef<HTMLDivElement>(null);
  const titleStartSpacerRef = useRef<HTMLSpanElement>(null);
  const titleEndSpacerRef = useRef<HTMLSpanElement>(null);
  const titleTrackXToRef = useRef<((value: number) => void) | null>(null);
  const [activeIndex, setActiveIndex] = useState(0);

  const viewportWidth = useViewportWidth(viewportRef);

  const boundedTurns = clamp(turns, 0.2, 4);
  const boundedBlur = clamp(blur, 0, 36);
  const boundedDim = clamp(dim, 0, 100);
  const boundedBrightnessBoost = clamp(brightnessBoost, 0, 120);
  const boundedDarknessStrength = clamp(darknessStrength, 0.2, 3);
  const boundedMinSaturation = clamp(minSaturation, 0, 100);
  const boundedSaturationStrength = clamp(saturationStrength, 0.2, 3);
  const boundedFocusSpread = clamp(focusSpread, 0.08, 0.8);
  const boundedScaleEffect = clamp(scaleEffect, 0, 0.3);
  const boundedScrollSensitivity = clamp(scrollSensitivity, 0.25, 1.6);
  const boundedItemWidth = clamp(itemWidth, 140, 520);
  const boundedItemHeight = clamp(itemHeight, 180, 620);
  const boundedCropRatio = clamp(cropRatio, 0.2, 0.8);
  const boundedScrollLength = clamp(scrollLength, 180, 700);
  const boundedCaptionOffset = clamp(captionOffset, 2, 22);
  const boundedSubtitleSpeed = clamp(subtitleSpeed, 0.3, 3);
  const boundedSubtitleStagger = clamp(subtitleStagger, 0, 0.08);
  const boundedSubtitleDirection =
    subtitleDirection === "bottom" ? "bottom" : "top";

  const responsiveWheelSize = clamp(viewportWidth * 1.65, 900, 2400);
  const boundedWheelSize = clamp(wheelSize ?? responsiveWheelSize, 700, 2600);
  const radius = boundedWheelSize / 2;
  const titleLabels = useMemo(
    () => images.map((img, i) => img.label ?? img.alt ?? `Image ${i + 1}`),
    [images],
  );
  const titleTrackLabels = useMemo(() => titleLabels, [titleLabels]);
  const activeTitleTrackIndex = Math.max(
    0,
    Math.min(activeIndex, titleTrackLabels.length - 1),
  );

  const handleTitleClick = useCallback(
    (index: number) => {
      const trigger = wheelScrollTriggerRef.current;
      if (!trigger || images.length === 0) return;

      const currentProgress = clamp(trigger.progress, 0, 1);
      const targetProgress = getSnapProgressForIndex(
        index,
        images.length,
        boundedTurns,
        currentProgress,
      );

      const scrollStart = trigger.start;
      const scrollEnd = trigger.end;
      const scrollRange = scrollEnd - scrollStart;
      if (scrollRange <= 0) return;

      const fromScroll = scrollStart + currentProgress * scrollRange;
      const toScroll = scrollStart + targetProgress * scrollRange;

      setActiveIndex(index);
      titleClickTweenRef.current?.kill();

      const proxy = { scroll: fromScroll };
      titleClickTweenRef.current = gsap.to(proxy, {
        scroll: toScroll,
        duration: 0.58,
        ease: "power3.out",
        overwrite: true,
        onUpdate: () => {
          trigger.scroll(proxy.scroll);
        },
        onComplete: () => {
          setActiveIndex(index);
        },
      });
    },
    [images.length, boundedTurns],
  );

  useEffect(() => {
    const section = sectionRef.current;
    const wheel = wheelRef.current;
    if (!section || !wheel || images.length === 0) return;

    let previousActive = -1;

    const context = gsap.context(() => {
      const cards = Array.from(
        wheel.querySelectorAll<HTMLElement>(".oiw-item"),
      );
      if (cards.length === 0) return;

      const topAnchor = -Math.PI / 2;
      const focusArc = Math.PI * boundedFocusSpread;

      const applyState = (rawProgress: number) => {
        const p = applyScrollSensitivity(rawProgress, boundedScrollSensitivity);
        const rotation = -p * boundedTurns * Math.PI * 2;
        const focusedIndex = getFocusedImageIndexWithHysteresis(
          p,
          cards.length,
          boundedTurns,
          previousActive,
        );

        cards.forEach((card, index) => {
          const base = (index / cards.length) * Math.PI * 2 - Math.PI;
          const theta = base + rotation;
          const x = Math.cos(theta) * radius;
          const y = Math.sin(theta) * radius;

          const distanceToFocus = shortestAngleDistance(theta, topAnchor);
          const focusIntensity = clamp(distanceToFocus / focusArc, 0, 1);

          const darkIntensity = clamp(
            focusIntensity * boundedDarknessStrength,
            0,
            1,
          );
          const saturationIntensity = clamp(
            focusIntensity * boundedSaturationStrength,
            0,
            1,
          );

          const currentBlur = darkIntensity * boundedBlur;
          const peakBrightness = clamp(100 + boundedBrightnessBoost, 100, 220);
          const currentBrightness =
            boundedDim + (1 - darkIntensity) * (peakBrightness - boundedDim);
          const currentSaturation =
            boundedMinSaturation +
            (1 - saturationIntensity) * (100 - boundedMinSaturation);
          const currentScale = 1 - darkIntensity * boundedScaleEffect;
          const drift = clamp(x / radius, -1, 1);
          const tilt = drift * 8;
          const depth = clamp((1 - focusIntensity) * 100, 0, 100);

          gsap.set(card, {
            x,
            y,
            xPercent: -50,
            yPercent: -50,
            z: depth,
            rotate: tilt,
            scale: currentScale,
            filter: `blur(${currentBlur}px) brightness(${currentBrightness}%) saturate(${currentSaturation}%)`,
            zIndex: Math.round(depth),
          });
        });

        if (focusedIndex !== previousActive) {
          previousActive = focusedIndex;
          setActiveIndex(focusedIndex);
        }
      };

      applyState(0);

      const trigger = ScrollTrigger.create({
        trigger: section,
        scroller: scrollContainerRef?.current ?? undefined,
        start: "top top",
        end: "bottom bottom",
        scrub: true,
        onUpdate: (self) => {
          applyState(self.progress);
        },
      });

      wheelScrollTriggerRef.current = trigger;
    }, sectionRef);

    ScrollTrigger.refresh();

    return () => context.revert();
  }, [
    scrollContainerRef,
    images,
    radius,
    boundedTurns,
    boundedBlur,
    boundedDim,
    boundedBrightnessBoost,
    boundedDarknessStrength,
    boundedMinSaturation,
    boundedSaturationStrength,
    boundedFocusSpread,
    boundedScaleEffect,
    boundedScrollSensitivity,
  ]);

  useEffect(() => {
    return () => {
      titleClickTweenRef.current?.kill();
      wheelScrollTriggerRef.current = null;
    };
  }, []);

  useLayoutEffect(() => {
    const viewport = titleViewportRef.current;
    const track = titleTrackRef.current;
    const startSpacer = titleStartSpacerRef.current;
    const endSpacer = titleEndSpacerRef.current;
    if (!viewport || !track || titleTrackLabels.length === 0) return;

    if (!titleTrackXToRef.current) {
      titleTrackXToRef.current = gsap.quickTo(track, "x", {
        duration: 0.62,
        ease: "power4.out",
        overwrite: true,
      });
    }

    const firstTitle = track.querySelector<HTMLElement>(
      `[data-title-index="0"]`,
    );
    const lastTitle = track.querySelector<HTMLElement>(
      `[data-title-index="${titleTrackLabels.length - 1}"]`,
    );

    const activeTitle = track.querySelector<HTMLElement>(
      `[data-title-index="${activeTitleTrackIndex}"]`,
    );
    if (!activeTitle || !firstTitle || !lastTitle) return;

    const viewportWidthPx = viewport.clientWidth;

    // Add edge spacers so the first and last pills can be centered.
    const startPad = Math.max(
      0,
      viewportWidthPx / 2 - firstTitle.offsetWidth / 2,
    );
    const endPad = Math.max(0, viewportWidthPx / 2 - lastTitle.offsetWidth / 2);

    if (startSpacer) {
      startSpacer.style.width = `${Math.round(startPad)}px`;
    }

    if (endSpacer) {
      endSpacer.style.width = `${Math.round(endPad)}px`;
    }

    const activeCenter = activeTitle.offsetLeft + activeTitle.offsetWidth / 2;

    let targetX = Math.round(viewportWidthPx / 2 - activeCenter);

    if (track.scrollWidth <= viewportWidthPx) {
      targetX = Math.round((viewportWidthPx - track.scrollWidth) / 2);
    } else {
      const minX = viewportWidthPx - track.scrollWidth;
      targetX = Math.round(clamp(targetX, minX, 0));
    }

    titleTrackXToRef.current(targetX);
  }, [activeTitleTrackIndex, titleTrackLabels, viewportWidth]);

  const activeImage = useMemo(() => {
    if (images.length === 0) return null;
    return images[activeIndex] ?? images[0];
  }, [images, activeIndex]);

  if (images.length === 0) {
    return null;
  }

  return (
    <section
      ref={sectionRef}
      className={cn("relative w-full", className)}
      style={{ height: `${boundedScrollLength}vh` }}
    >
      <div
        ref={viewportRef}
        className="sticky top-0 h-screen w-full overflow-hidden"
      >
        <div
          ref={wheelRef}
          className="absolute left-1/2 -translate-x-1/2"
          style={{
            width: boundedWheelSize,
            height: boundedWheelSize,
            bottom: `-${boundedWheelSize * boundedCropRatio}px`,
          }}
        >
          <div
            className="relative h-full w-full"
            style={{ perspective: "1200px" }}
          >
            {images.map((img, i) => (
              <figure
                key={i}
                className="oiw-item absolute left-1/2 top-1/2 m-0 overflow-hidden rounded-xl"
                style={{ width: boundedItemWidth, height: boundedItemHeight }}
              >
                <div
                  className="absolute inset-0 h-full w-full bg-cover bg-center"
                  style={{ backgroundImage: `url(${img.src})` }}
                  role="img"
                  aria-label={img.alt ?? img.label ?? `Image ${i + 1}`}
                />
              </figure>
            ))}
          </div>
        </div>

        {showCaption && activeImage && (
          <div
            className="pointer-events-none absolute inset-x-0 z-30 flex justify-center"
            style={{ bottom: `${boundedCaptionOffset}vh` }}
          >
            <div className="px-6 text-center">
              <MotionSubtitle
                text={activeImage.subtitle ?? activeImage.alt ?? "Visual Story"}
                direction={boundedSubtitleDirection}
                speed={boundedSubtitleSpeed}
                stagger={boundedSubtitleStagger}
                className="mb-2 text-[clamp(0.8rem,1vw,0.95rem)] tracking-[0.04em] text-foreground/45"
              />

              <div
                ref={titleViewportRef}
                className="pointer-events-auto mx-auto w-[min(92vw,760px)] overflow-hidden py-1"
                style={{
                  WebkitMaskImage:
                    "linear-gradient(to right, transparent 0%, black 14%, black 86%, transparent 100%)",
                  maskImage:
                    "linear-gradient(to right, transparent 0%, black 14%, black 86%, transparent 100%)",
                }}
              >
                <div ref={titleTrackRef} className="flex w-max items-center">
                  <span
                    ref={titleStartSpacerRef}
                    aria-hidden
                    className="block h-px shrink-0"
                  />

                  {titleTrackLabels.map((title, i) => (
                    <button
                      type="button"
                      key={`${title}-${i}`}
                      data-title-index={i}
                      onClick={() => handleTitleClick(i)}
                      aria-current={
                        i === activeTitleTrackIndex ? "true" : undefined
                      }
                      style={{
                        opacity:
                          Math.abs(i - activeTitleTrackIndex) === 0
                            ? 1
                            : Math.abs(i - activeTitleTrackIndex) === 1
                              ? 0.58
                              : Math.abs(i - activeTitleTrackIndex) === 2
                                ? 0.32
                                : 0.16,
                        transform:
                          Math.abs(i - activeTitleTrackIndex) === 0
                            ? "scale(1)"
                            : "scale(0.96)",
                      }}
                      className={cn(
                        "oiw-title-item mr-3 inline-flex shrink-0 cursor-pointer appearance-none items-center justify-center whitespace-nowrap rounded-full border border-foreground/35 px-7 py-2 text-center leading-none text-[clamp(1.05rem,2.25vw,2rem)] font-medium tracking-tight transition-[opacity,transform,color,border-color] duration-300",
                        i === activeTitleTrackIndex
                          ? "border-foreground/40 text-foreground"
                          : "border-foreground/28 text-foreground/45",
                      )}
                    >
                      {title}
                    </button>
                  ))}

                  <span
                    ref={titleEndSpacerRef}
                    aria-hidden
                    className="block h-px shrink-0"
                  />
                </div>
              </div>
            </div>
          </div>
        )}
      </div>
    </section>
  );
}

export default OrbitalImageWheel;
```

---

## Marketing <a name="marketing"></a>

### Aurora Card <a name="aurora-card"></a>

Aurora-blur pricing card with a configurable hero area, layout hooks, and customizable shader controls.

- **Tier**: Pro
- **Category**: Marketing

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { AuroraCard } from "@/components/unlumen-ui/aurora-card";

export default function Example() {
  return <AuroraCard />;
}
```

---

### Blob Card <a name="blob-card"></a>

Animated card with fluid blob header, glowing border, and configurable colors.

- **Tier**: Free
- **Category**: Marketing

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/blob-card.json
```

#### How to Use

```tsx
import { BlobCard } from "@/components/unlumen-ui/components/ui/blob-card";

<BlobCard>Your content here</BlobCard>;
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `header?` | `React.ReactNode` | `-` | Optional content rendered inside the blob header area. |
| `headerHeight?` | `number` | `224` | Height of the blob header section in pixels. |
| `lightColors?` | `string[]` | `[&quot;#ff0020&quot;, &quot;#fc0f60&quot;, &quot;#e8227a&quot;, &quot;#ff85b3&quot;]` | Color stops for the fluid blobs in light mode. |
| `darkColors?` | `string[]` | `[&quot;#8c0f60&quot;, &quot;#e8227a&quot;, &quot;#e8227a&quot;, &quot;#ff85b3&quot;]` | Color stops for the fluid blobs in dark mode. |
| `glowColors?` | `string[]` | `[&quot;#ff96a9&quot;, &quot;#e8b4f0&quot;, &quot;#ffb3c6&quot;, &quot;#d44d8a&quot;, &quot;#ff96a9&quot;]` | Color stops used for the animated glow border. |
| `children?` | `React.ReactNode` | `-` | Content rendered below the blob header. |
| `className?` | `string` | `-` | Additional CSS classes applied to the outer wrapper. |

#### Component Source Code

##### File: `registry/components/ui/blob-card/index.tsx`

```tsx
"use client";

import * as React from "react";
import { FluidBlobs } from "@/components/ui/FluidBlobs";
import { GlowEffect } from "@/components/ui/glow-effect";
import { cn } from "@/lib/utils";

export interface BlobCardProps {
  header?: React.ReactNode;
  children?: React.ReactNode;
  headerHeight?: number;
  lightColors?: string[];
  darkColors?: string[];
  glowColors?: string[];
  className?: string;
}

const DEFAULT_LIGHT = ["#ff0020", "#fc0f60", "#e8227a", "#ff85b3"];
const DEFAULT_DARK = ["#8c0f60", "#e8227a", "#e8227a", "#ff85b3"];
const DEFAULT_GLOW = ["#ff96a9", "#e8b4f0", "#ffb3c6", "#d44d8a", "#ff96a9"];

export function BlobCard({
  header,
  children,
  headerHeight = 224,
  lightColors = DEFAULT_LIGHT,
  darkColors = DEFAULT_DARK,
  glowColors = DEFAULT_GLOW,
  className,
}: BlobCardProps) {
  return (
    <div className={cn("relative w-full", className)}>
      <div className="absolute -inset-[1.5px] rounded-[21.5px] overflow-hidden z-0">
        <GlowEffect
          colors={glowColors}
          mode="rotate"
          blur="strongest"
          duration={5}
          scale={1}
        />
      </div>

      <div className="relative z-10 rounded-[20px] overflow-hidden bg-background">
        <div
          className="relative overflow-hidden rounded-t-[20px]"
          style={{ height: headerHeight }}
        >
          <FluidBlobs
            lightColors={lightColors}
            darkColors={darkColors}
            origins={[
              { x: 50, y: -55 },
              { x: 50, y: -25 },
              { x: 50, y: -25 },
              { x: 50, y: -25 },
            ]}
            margin={60}
            blur={50}
          />
          <div className="absolute inset-0 bg-gradient-to-b from-transparent via-transparent to-background pointer-events-none" />
          {header && <div className="relative z-10 p-8 pb-0">{header}</div>}
        </div>

        {children && <div>{children}</div>}
      </div>
    </div>
  );
}
```

---

### Inline Testimonials <a name="inline-testimonials"></a>

Testimonials rendered as inline flowing text with circular avatars. Hover any quote to blur the others and reveal the author's name and role.

- **Tier**: Pro
- **Category**: Marketing

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import {
  InlineTestimonials,
  type Testimonial,
} from "@/components/unlumen-ui/inline-testimonials";

const testimonials: Testimonial[] = [
  {
    id: "1",
    text: "Undoubtedly one of the best resources in the world.",
    author: {
      name: "Benji Taylor",
      role: "Head of Design @Base",
      avatar: "https://example.com/avatar.jpg",
    },
  },
];

<InlineTestimonials testimonials={testimonials} accentColor="#f97316" />;
```

#### Props

##### `InlineTestimonials` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `testimonials` | `Testimonial[]` | `-` | Array of testimonial objects to render inline. |
| `blurAmount?` | `number` | `5` | Blur filter strength (px) applied to non-hovered items. |
| `blurOpacity?` | `number` | `0.25` | Opacity of non-hovered items (0–1). |
| `accentColor?` | `string` | `&quot;#f97316&quot;` | CSS color used for the author name label on hover. |
| `avatarSize?` | `number` | `32` | Diameter of the circular avatar image in pixels. |
| `fontSize?` | `number` | `30` | Font size of the testimonial text in pixels. |
| `className?` | `string` | `-` | Extra classes on the container element. |

##### `Testimonial` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `id` | `string` | `-` | Unique identifier for the testimonial. |
| `text` | `string` | `-` | The testimonial quote text. |
| `author.name` | `string` | `-` | Author's full name, shown on hover. |
| `author.role` | `string` | `-` | Author's role and company, shown on hover. |
| `author.avatar` | `string` | `-` | URL of the author's circular avatar image. |

---

### Vertical Marquee <a name="vertical-marquee"></a>

Two columns of tweet cards scrolling vertically in opposite directions with a progressive fade at the top and bottom edges.

- **Tier**: Free
- **Category**: Marketing

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/vertical-marquee.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `react-tweet`

#### How to Use

```tsx
import { VerticalMarquee } from "@/components/unlumen-ui/vertical-marquee";

const tweetIds = [
  "1892181693611458734",
  "1886746617457354918",
  "1874122090617197017",
  "1854306627573950703",
];

export default async function Page() {
  return (
    <div className="h-[560px]">
      <VerticalMarquee tweetIds={tweetIds} />
    </div>
  );
}
```

#### Demo Code

```tsx
"use client";

import type { Tweet } from "react-tweet/api";

import {
  VerticalMarqueeClient,
  type TweetItem,
} from "@/registry/components/unlumen/vertical-marquee";

function createAvatarDataUri(seed: string, palette: [string, string, string]) {
  const [bg, skin, shirt] = palette;
  const svg = `
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 96 96" fill="none">
      <defs>
        <linearGradient id="bg" x1="0" y1="0" x2="96" y2="96" gradientUnits="userSpaceOnUse">
          <stop stop-color="${bg}" />
          <stop offset=".5" stop-color="${skin}" />
          <stop offset="1" stop-color="${shirt}" />
        </linearGradient>
      </defs>
      <rect width="96" height="96" rx="48" fill="url(#bg)" />
    </svg>
  `;

  return `data:image/svg+xml;charset=UTF-8,${encodeURIComponent(svg)}`;
}

const makeTweet = ({
  id,
  name,
  handle,
  text,
  photo,
  avatarPalette,
}: {
  id: string;
  name: string;
  handle: string;
  text: string;
  photo?: string;
  avatarPalette: [string, string, string];
}): Tweet => ({
  __typename: "Tweet",
  lang: "en",
  created_at: "2026-03-20T10:00:00.000Z",
  display_text_range: [0, text.length],
  entities: {
    hashtags: [],
    urls: [],
    user_mentions: [],
    symbols: [],
  },
  id_str: id,
  text,
  user: {
    id_str: `${id}-user`,
    name,
    profile_image_url_https: createAvatarDataUri(handle, avatarPalette),
    profile_image_shape: "Circle",
    screen_name: handle,
    verified: true,
    is_blue_verified: true,
  },
  edit_control: {
    edit_tweet_ids: [id],
    editable_until_msecs: "0",
    is_edit_eligible: false,
    edits_remaining: "0",
  },
  isEdited: false,
  isStaleEdit: false,
  favorite_count: 0,
  conversation_count: 0,
  news_action_type: "conversation",
  photos: photo
    ? [
        {
          backgroundColor: {
            red: 17,
            green: 24,
            blue: 39,
          },
          cropCandidates: [],
          expandedUrl: photo,
          url: photo,
          width: 1200,
          height: 900,
        },
      ]
    : undefined,
});

const TWEETS: TweetItem[] = [
  {
    id: "demo-1",
    tweet: makeTweet({
      id: "demo-1",
      name: "Elon Musk",
      handle: "elonmusk",
      text: "Mars is basically a fixer-upper.",
      avatarPalette: ["#22d3ee", "#2563eb", "#0f172a"],
    }),
  },
  {
    id: "demo-2",
    tweet: makeTweet({
      id: "demo-2",
      name: "Ryan Reynolds",
      handle: "vancityreynolds",
      text: "I meditate by opening one tab and closing seventeen.",
      avatarPalette: ["#f97316", "#fb7185", "#7f1d1d"],
    }),
  },
  {
    id: "demo-3",
    tweet: makeTweet({
      id: "demo-3",
      name: "Zendaya",
      handle: "zendaya",
      text: "Replying 'on my way' while still choosing socks is a universal experience.",
      avatarPalette: ["#ec4899", "#f59e0b", "#7c2d12"],
    }),
  },
  {
    id: "demo-4",
    tweet: makeTweet({
      id: "demo-4",
      name: "Keanu Reeves",
      handle: "keanureeves",
      text: "The scariest part of adulthood is how often lunch is a decision.",
      avatarPalette: ["#64748b", "#94a3b8", "#0f172a"],
    }),
  },
  {
    id: "demo-5",
    tweet: makeTweet({
      id: "demo-5",
      name: "Taylor Swift",
      handle: "taylorswift13",
      text: "I support any plan that starts with coffee and ends with cancellation.",
      avatarPalette: ["#a78bfa", "#f472b6", "#4338ca"],
    }),
  },
  {
    id: "demo-6",
    tweet: makeTweet({
      id: "demo-6",
      name: "Pedro Pascal",
      handle: "pascalispunk",
      text: "My strongest skill is nodding like I understand the group chat plan.",
      avatarPalette: ["#14b8a6", "#22c55e", "#14532d"],
    }),
  },
  {
    id: "demo-7",
    tweet: makeTweet({
      id: "demo-7",
      name: "Dua Lipa",
      handle: "dualipa",
      text: "The best productivity hack is pretending the deadline is in 14 minutes.",
      avatarPalette: ["#06b6d4", "#3b82f6", "#1e3a8a"],
    }),
  },
  {
    id: "demo-8",
    tweet: makeTweet({
      id: "demo-8",
      name: "Chris Evans",
      handle: "chrisevans",
      text: "I opened the fridge four times like the software update might finally be ready.",
      avatarPalette: ["#ef4444", "#f59e0b", "#7c2d12"],
    }),
  },
  {
    id: "demo-9",
    tweet: makeTweet({
      id: "demo-9",
      name: "Ariana Grande",
      handle: "arianagrande",
      text: "I respect every calendar reminder that starts sounding passive aggressive.",
      avatarPalette: ["#f472b6", "#c084fc", "#6d28d9"],
    }),
  },
  {
    id: "demo-10",
    tweet: makeTweet({
      id: "demo-10",
      name: "LeBron James",
      handle: "kingjames",
      text: "Hydration, discipline, and accidentally liking a message from 2019.",
      avatarPalette: ["#f59e0b", "#f97316", "#7f1d1d"],
    }),
  },
  {
    id: "demo-11",
    tweet: makeTweet({
      id: "demo-11",
      name: "Billie Eilish",
      handle: "billieeilish",
      text: "If I say 'quick call' I am lying to both of us.",
      avatarPalette: ["#84cc16", "#22c55e", "#14532d"],
    }),
  },
  {
    id: "demo-12",
    tweet: makeTweet({
      id: "demo-12",
      name: "Robert Downey Jr.",
      handle: "robertdowneyjr",
      text: "Confidence is just re-opening the same app and hoping this time it behaves.",
      avatarPalette: ["#38bdf8", "#818cf8", "#312e81"],
    }),
  },
];

interface VerticalMarqueeDemoProps {
  speed?: number;
  gap?: number;
  blurSize?: number;
  pauseOnHover?: boolean;
}

export const VerticalMarqueeDemo = ({
  speed = 20,
  gap = 16,
  blurSize = 120,
  pauseOnHover = true,
}: VerticalMarqueeDemoProps) => {
  return (
    <div className="mx-auto h-[760px] w-full max-w-5xl overflow-hidden px-4 py-2">
      <VerticalMarqueeClient
        tweets={TWEETS}
        speed={speed}
        gap={gap}
        blurSize={blurSize}
        pauseOnHover={pauseOnHover}
      />
    </div>
  );
};

export default VerticalMarqueeDemo;
```

#### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `tweetIds` | `string[]` | `-` | Array of tweet IDs to fetch and display in the scrolling columns. |
| `columns?` | `1 \| 2` | `2` | Number of scrolling columns. Both columns scroll at different speeds and in opposite directions. |
| `speed?` | `number` | `20` | Duration in seconds for one full scroll loop of the primary column. |
| `gap?` | `number` | `16` | Vertical gap between tweet cards in pixels. Also used as the horizontal gap between columns. |
| `blurSize?` | `number` | `120` | Height in pixels of the progressive fade zone at the top and bottom edges. |
| `pauseOnHover?` | `boolean` | `true` | When true, all columns smoothly decelerate to a stop while the cursor is over the component. |
| `className?` | `string` | `-` | Extra classes applied to the root container element. |
| `tweetClassName?` | `string` | `-` | Extra classes applied to each tweet card. |

#### Component Source Code

##### File: `registry/components/unlumen/vertical-marquee/index.tsx`

```tsx
"use client";
import { getTweet, type Tweet } from "react-tweet/api";

import {
  TweetNotFound,
  VerticalMarqueeClient,
} from "./vertical-marquee-client";
import type { TweetItem } from "./vertical-marquee.types";

export type { TweetItem } from "./vertical-marquee.types";

export interface VerticalMarqueeProps {
  /** Tweet IDs to display in the scrolling columns. */
  tweetIds: string[];
  /** Number of scrolling columns. @default 2 */
  columns?: 1 | 2;
  /** Scroll duration in seconds per full loop. @default 20 */
  speed?: number;
  /** Vertical gap between cards in px. @default 16 */
  gap?: number;
  /** Height of the fade zone at top and bottom in px. @default 120 */
  blurSize?: number;
  /** Smoothly decelerate to a stop when hovering. @default true */
  pauseOnHover?: boolean;
  className?: string;
  tweetClassName?: string;
}

async function getTweetItem(id: string): Promise<TweetItem> {
  const tweet = await getTweet(id).catch((error) => {
    console.error(error);
    return undefined;
  });

  return { id, tweet };
}

export async function VerticalMarquee({
  tweetIds,
  columns = 2,
  speed = 20,
  gap = 16,
  blurSize = 120,
  pauseOnHover = true,
  className,
  tweetClassName,
}: VerticalMarqueeProps) {
  if (!tweetIds.length) {
    return null;
  }

  const tweets = await Promise.all(tweetIds.map((id) => getTweetItem(id)));
  const hasResolvedTweet = tweets.some((item) => item.tweet);

  if (!hasResolvedTweet) {
    return (
      <div className="grid gap-4 sm:grid-cols-2">
        {tweetIds.slice(0, Math.min(tweetIds.length, 4)).map((id) => (
          <TweetNotFound key={id} />
        ))}
      </div>
    );
  }

  return (
    <VerticalMarqueeClient
      tweets={tweets}
      columns={columns}
      speed={speed}
      gap={gap}
      blurSize={blurSize}
      pauseOnHover={pauseOnHover}
      className={className}
      tweetClassName={tweetClassName}
    />
  );
}

export {
  MagicTweet,
  TweetBody,
  TweetHeader,
  TweetMedia,
  TweetNotFound,
  TweetSkeleton,
  VerticalMarqueeClient,
} from "./vertical-marquee-client";
```

##### File: `registry/components/unlumen/vertical-marquee/vertical-marquee-client.tsx`

```tsx
"use client";

import * as React from "react";
import { motion, useAnimationFrame, useMotionValue } from "motion/react";
import { enrichTweet, type EnrichedTweet } from "react-tweet";

import type { Tweet } from "react-tweet/api";

import { cn } from "@/lib/utils";

import type { TweetItem } from "./vertical-marquee.types";

type TweetWithLooseEntities = Tweet & {
  entities: Partial<Tweet["entities"]> | null | undefined;
};

interface TwitterIconProps {
  className?: string;
  [key: string]: unknown;
}

const Twitter = ({ className, ...props }: TwitterIconProps) => (
  <svg
    stroke="currentColor"
    fill="currentColor"
    strokeWidth="0"
    viewBox="0 0 24 24"
    height="1em"
    width="1em"
    xmlns="http://www.w3.org/2000/svg"
    className={className}
    {...props}
  >
    <g>
      <path fill="none" d="M0 0h24v24H0z" />
      <path d="M22.162 5.656a8.384 8.384 0 0 1-2.402.658A4.196 4.196 0 0 0 21.6 4c-.82.488-1.719.83-2.656 1.015a4.182 4.182 0 0 0-7.126 3.814 11.874 11.874 0 0 1-8.62-4.37 4.168 4.168 0 0 0-.566 2.103c0 1.45.738 2.731 1.86 3.481a4.168 4.168 0 0 1-1.894-.523v.052a4.185 4.185 0 0 0 3.355 4.101 4.21 4.21 0 0 1-1.89.072A4.185 4.185 0 0 0 7.97 16.65a8.394 8.394 0 0 1-6.191 1.732 11.83 11.83 0 0 0 6.41 1.88c7.693 0 11.9-6.373 11.9-11.9 0-.18-.005-.362-.013-.54a8.496 8.496 0 0 0 2.087-2.165z" />
    </g>
  </svg>
);

const Verified = ({ className, ...props }: TwitterIconProps) => (
  <svg
    aria-label="Verified Account"
    viewBox="0 0 24 24"
    className={className}
    {...props}
  >
    <g fill="currentColor">
      <path d="M22.5 12.5c0-1.58-.875-2.95-2.148-3.6.154-.435.238-.905.238-1.4 0-2.21-1.71-3.998-3.818-3.998-.47 0-.92.084-1.336.25C14.818 2.415 13.51 1.5 12 1.5s-2.816.917-3.437 2.25c-.415-.165-.866-.25-1.336-.25-2.11 0-3.818 1.79-3.818 4 0 .494.083.964.237 1.4-1.272.65-2.147 2.018-2.147 3.6 0 1.495.782 2.798 1.942 3.486-.02.17-.032.34-.032.514 0 2.21 1.708 4 3.818 4 .47 0 .92-.086 1.335-.25.62 1.334 1.926 2.25 3.437 2.25 1.512 0 2.818-.916 3.437-2.25.415.163.865.248 1.336.248 2.11 0 3.818-1.79 3.818-4 0-.174-.012-.344-.033-.513 1.158-.687 1.943-1.99 1.943-3.484zm-6.616-3.334l-4.334 6.5c-.145.217-.382.334-.625.334-.143 0-.288-.04-.416-.126l-.115-.094-2.415-2.415c-.293-.293-.293-.768 0-1.06s.768-.294 1.06 0l1.77 1.767 3.825-5.74c.23-.345.696-.436 1.04-.207.346.23.44.696.21 1.04z" />
    </g>
  </svg>
);

export const truncate = (str: string | null, length: number) => {
  if (!str || str.length <= length) return str;
  return `${str.slice(0, length - 3)}...`;
};

const Skeleton = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) => {
  return (
    <div className={cn("rounded-md bg-primary/10", className)} {...props} />
  );
};

export const TweetSkeleton = ({
  className,
  ...props
}: {
  className?: string;
  [key: string]: unknown;
}) => (
  <div
    className={cn(
      "flex size-full max-h-max min-w-72 flex-col gap-3 rounded-[28px] border border-border/60 bg-background/90 p-4 backdrop-blur-sm",
      className,
    )}
    {...props}
  >
    <div className="flex flex-row gap-3">
      <Skeleton className="size-11 shrink-0 rounded-full" />
      <Skeleton className="h-11 w-full" />
    </div>
    <Skeleton className="h-24 w-full" />
    <Skeleton className="h-44 w-full rounded-2xl" />
  </div>
);

export const TweetNotFound = ({
  className,
  ...props
}: {
  className?: string;
  [key: string]: unknown;
}) => (
  <div
    className={cn(
      "flex min-h-56 size-full flex-col items-center justify-center gap-2 rounded-[28px] border border-dashed border-border/70 bg-muted/40 p-6 text-center",
      className,
    )}
    {...props}
  >
    <h3 className="text-base font-medium">Tweet not found</h3>
    <p className="text-muted-foreground text-sm">
      This post could not be loaded.
    </p>
  </div>
);

export const TweetHeader = ({ tweet }: { tweet: EnrichedTweet }) => (
  <div className="flex flex-row items-start justify-between tracking-normal">
    <div className="flex items-center space-x-3">
      <a
        href={tweet.user.url}
        target="_blank"
        rel="noreferrer"
        className="shrink-0"
      >
        <img
          title={`Profile picture of ${tweet.user.name}`}
          alt={tweet.user.screen_name}
          height={48}
          width={48}
          src={tweet.user.profile_image_url_https}
          className="overflow-hidden rounded-full"
        />
      </a>
      <div className="flex flex-col gap-0.5">
        <a
          href={tweet.user.url}
          target="_blank"
          rel="noreferrer"
          className="text-foreground flex items-center font-medium whitespace-nowrap transition-opacity hover:opacity-80"
        >
          {truncate(tweet.user.name, 20)}
          {(tweet.user.verified || tweet.user.is_blue_verified) && (
            <Verified className="ml-1 inline size-4 text-sky-500" />
          )}
        </a>
        <div className="flex items-center space-x-1">
          <a
            href={tweet.user.url}
            target="_blank"
            rel="noreferrer"
            className="text-muted-foreground hover:text-foreground text-sm transition-colors"
          >
            @{truncate(tweet.user.screen_name, 16)}
          </a>
        </div>
      </div>
    </div>
    <a href={tweet.url} target="_blank" rel="noreferrer">
      <span className="sr-only">Link to tweet</span>
      <Twitter className="text-muted-foreground hover:text-foreground size-5 transition-transform hover:scale-105" />
    </a>
  </div>
);

export const TweetBody = ({ tweet }: { tweet: EnrichedTweet }) => (
  <div className="whitespace-pre-wrap break-words text-[15px] leading-relaxed tracking-normal">
    {tweet.entities.map((entity, idx) => {
      switch (entity.type) {
        case "url":
        case "symbol":
        case "hashtag":
        case "mention":
          return (
            <a
              key={idx}
              href={entity.href}
              target="_blank"
              rel="noopener noreferrer"
              className="text-muted-foreground hover:text-foreground text-[15px] font-normal transition-colors"
            >
              <span>{entity.text}</span>
            </a>
          );
        case "text":
          return (
            <span
              key={idx}
              className="text-foreground text-[15px] font-normal"
              dangerouslySetInnerHTML={{ __html: entity.text }}
            />
          );
        default:
          return null;
      }
    })}
  </div>
);

function getCardThumbnail(tweet: unknown) {
  const card = tweet as {
    card?: {
      binding_values?: {
        thumbnail_image_large?: {
          image_value?: {
            url?: string;
          };
        };
      };
    };
  };

  return card.card?.binding_values?.thumbnail_image_large?.image_value?.url;
}

function getBestVideoSource(tweet: EnrichedTweet) {
  const variants = tweet.video?.variants ?? [];
  const mp4Variants = variants.filter((variant) =>
    variant.src.includes(".mp4"),
  );

  return (
    mp4Variants.at(-1)?.src ?? variants.find((variant) => variant.src)?.src
  );
}

export const TweetMedia = ({ tweet }: { tweet: EnrichedTweet }) => {
  const thumbnail = getCardThumbnail(tweet);
  const videoSource = getBestVideoSource(tweet);

  if (!tweet.video && !tweet.photos?.length && !thumbnail) {
    return null;
  }

  return (
    <div className="flex flex-1 items-center justify-center">
      {tweet.video ? (
        <video
          poster={tweet.video.poster}
          autoPlay
          loop
          muted
          playsInline
          className="w-full rounded-2xl border border-border/60"
        >
          {videoSource ? <source src={videoSource} type="video/mp4" /> : null}
          Your browser does not support the video tag.
        </video>
      ) : tweet.photos?.length ? (
        <div className="relative flex w-full snap-x snap-mandatory gap-3 overflow-x-auto pb-1">
          {tweet.photos.map((photo) => (
            <img
              key={photo.url}
              src={photo.url}
              width={photo.width}
              height={photo.height}
              title={`Photo by ${tweet.user.name}`}
              alt={tweet.text}
              className="h-64 w-[88%] shrink-0 snap-center rounded-2xl border border-border/60 object-cover"
            />
          ))}
        </div>
      ) : (
        <img
          src={thumbnail}
          className="h-64 w-full rounded-2xl border border-border/60 object-cover"
          alt={tweet.text}
        />
      )}
    </div>
  );
};

function normalizeTweetEntities(tweet: Tweet): Tweet {
  const entities = (tweet as TweetWithLooseEntities).entities;

  return {
    ...tweet,
    entities: {
      hashtags: Array.isArray(entities?.hashtags) ? entities.hashtags : [],
      urls: Array.isArray(entities?.urls) ? entities.urls : [],
      user_mentions: Array.isArray(entities?.user_mentions)
        ? entities.user_mentions
        : [],
      symbols: Array.isArray(entities?.symbols) ? entities.symbols : [],
      media: Array.isArray(entities?.media) ? entities.media : undefined,
    },
  };
}

export const MagicTweet = ({
  tweet,
  className,
  ...props
}: {
  tweet: Tweet;
  className?: string;
}) => {
  const enrichedTweet = enrichTweet(normalizeTweetEntities(tweet));

  return (
    <article
      className={cn(
        "relative flex h-fit w-full flex-col gap-4 overflow-hidden rounded-[28px] border border-border/60 bg-background/95 p-5 backdrop-blur-sm",
        className,
      )}
      {...props}
    >
      <TweetHeader tweet={enrichedTweet} />
      <TweetBody tweet={enrichedTweet} />
      <TweetMedia tweet={enrichedTweet} />
    </article>
  );
};

interface ColumnProps {
  tweets: TweetItem[];
  speed: number;
  gap: number;
  reverse?: boolean;
  paused: boolean;
  tweetClassName?: string;
}

function MarqueeColumn({
  tweets,
  speed,
  gap,
  reverse = false,
  paused,
  tweetClassName,
}: ColumnProps) {
  const wrapRef = React.useRef<HTMLDivElement>(null);
  const [singleHeight, setSingleHeight] = React.useState(0);
  const y = useMotionValue(0);
  const pos = React.useRef(0);
  const currentSpeed = React.useRef(0);
  const initialized = React.useRef(false);

  React.useEffect(() => {
    const el = wrapRef.current;
    if (!el) return;

    const measure = () => {
      const height = el.scrollHeight / 2;
      if (height <= 0) return;
      setSingleHeight(height);
      if (!initialized.current) {
        pos.current = reverse ? -height : 0;
        y.set(pos.current);
        initialized.current = true;
      }
    };

    const observer = new ResizeObserver(measure);
    observer.observe(el);
    measure();

    return () => observer.disconnect();
  }, [reverse, tweets, y]);

  useAnimationFrame((_, delta) => {
    if (!singleHeight) return;

    const targetSpeed = paused ? 0 : singleHeight / speed;
    currentSpeed.current += (targetSpeed - currentSpeed.current) * 0.08;

    const step = currentSpeed.current * (delta / 1000);
    if (reverse) {
      pos.current += step;
      if (pos.current >= 0) pos.current -= singleHeight;
    } else {
      pos.current -= step;
      if (pos.current <= -singleHeight) pos.current += singleHeight;
    }

    y.set(pos.current);
  });

  const doubled = [...tweets, ...tweets];

  return (
    <div className="min-w-0 flex-1 overflow-hidden">
      <motion.div ref={wrapRef} style={{ y }}>
        {doubled.map((item, index) => (
          <div
            key={`${item.id}-${index}`}
            style={{ marginBottom: `${gap}px` }}
            className="w-full"
          >
            {item.tweet ? (
              <MagicTweet tweet={item.tweet} className={tweetClassName} />
            ) : (
              <TweetNotFound className={tweetClassName} />
            )}
          </div>
        ))}
      </motion.div>
    </div>
  );
}

export function VerticalMarqueeClient({
  tweets,
  columns = 2,
  speed = 20,
  gap = 16,
  blurSize = 120,
  pauseOnHover = true,
  className,
  tweetClassName,
}: {
  tweets: TweetItem[];
  columns?: 1 | 2;
  speed?: number;
  gap?: number;
  blurSize?: number;
  pauseOnHover?: boolean;
  className?: string;
  tweetClassName?: string;
}) {
  const [paused, setPaused] = React.useState(false);

  const mid = Math.ceil(tweets.length / 2);
  const col1 = columns === 2 ? tweets.slice(0, mid) : tweets;
  const col2 = columns === 2 ? tweets.slice(mid) : tweets;

  const mask = `linear-gradient(to bottom, transparent 0%, black ${blurSize}px, black calc(100% - ${blurSize}px), transparent 100%)`;

  return (
    <div
      className={cn("h-full w-full overflow-hidden", className)}
      style={{ maskImage: mask, WebkitMaskImage: mask }}
      onMouseEnter={() => pauseOnHover && setPaused(true)}
      onMouseLeave={() => setPaused(false)}
    >
      <div className="flex h-full" style={{ gap: `${gap}px` }}>
        <MarqueeColumn
          tweets={col1.length ? col1 : tweets}
          speed={speed}
          gap={gap}
          paused={paused}
          tweetClassName={tweetClassName}
        />
        {columns === 2 && (
          <MarqueeColumn
            tweets={col2.length ? col2 : tweets}
            speed={speed * 1.25}
            gap={gap}
            reverse
            paused={paused}
            tweetClassName={tweetClassName}
          />
        )}
      </div>
    </div>
  );
}
```

##### File: `registry/components/unlumen/vertical-marquee/vertical-marquee.types.ts`

```tsx
import type { Tweet } from "react-tweet/api";

export interface TweetItem {
  id: string;
  tweet?: Tweet;
}
```

---

## Navigation <a name="navigation"></a>

### Dock <a name="dock"></a>

macOS-style animated dock with Gaussian neighbor icon magnification driven by spring physics.

- **Tier**: Free
- **Category**: Navigation

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/dock.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
import { Dock } from "@/components/unlumen-ui/dock";

const items = [
  { icon: "🌐", label: "Browser" },
  { icon: "📁", label: "Files", href: "/files" },
];

<Dock items={items} />;
```

#### Demo Code

```tsx
"use client";

import {
  Globe02Icon as Globe,
  FolderOpenIcon as FolderOpen,
  MusicNote01Icon as Music,
  Image01Icon as Image,
  Settings01Icon as Settings,
  BubbleChatIcon as MessageCircle,
  ComputerTerminal01Icon as Terminal,
} from "hugeicons-react";

import { Dock, type DockItem } from "@/registry/components/unlumen/dock";

const ITEMS: DockItem[] = [
  { icon: <Globe />, label: "Browser" },
  { icon: <FolderOpen />, label: "Files" },
  { icon: <Terminal />, label: "Terminal" },
  { icon: <Music />, label: "Music", separator: true },
  { icon: <Image />, label: "Photos" },
  { icon: <MessageCircle />, label: "Messages", separator: true },
  { icon: <Settings />, label: "Settings" },
];

interface DockDemoProps {
  magnification?: number;
  distance?: number;
  iconSize?: number;
  gap?: number;
  alwaysShowLabels?: boolean;
  borderRadius?: number;
}

export const DockDemo = ({
  magnification,
  distance,
  iconSize,
  gap,
  alwaysShowLabels,
  borderRadius,
}: DockDemoProps) => {
  return (
    <div className="flex items-end justify-center p-16 min-h-[200px]">
      <Dock
        items={ITEMS}
        magnification={magnification}
        distance={distance}
        iconSize={iconSize}
        gap={gap}
        alwaysShowLabels={alwaysShowLabels}
        borderRadius={borderRadius}
      />
    </div>
  );
};

export default DockDemo;
```

#### Props

##### `Dock` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `items` | `DockItem[]` | `-` | Array of dock items to render. |
| `magnification?` | `number` | `2.2` | Maximum scale factor applied to the icon directly under the cursor. |
| `distance?` | `number` | `100` | Pixel radius from the cursor within which neighbor icons are magnified. |
| `iconSize?` | `number` | `48` | Base icon size in pixels (width & height). |
| `gap?` | `number` | `12` | Gap between icons in pixels. |
| `alwaysShowLabels?` | `boolean` | `false` | When true, labels are always visible beneath icons instead of appearing only on hover. |
| `springOptions?` | `SpringOptions` | `{ stiffness: 300, damping: 22, mass: 0.5 }` | Motion spring options for the scale animation. |
| `className?` | `string` | `-` | Extra classes on the dock container. |

##### `DockItem` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `icon` | `React.ReactNode` | `-` | Icon content — emoji, image, or any React node. |
| `label` | `string` | `-` | Tooltip text shown above the icon on hover. |
| `href?` | `string` | `-` | If provided, the item renders as an anchor tag. |
| `onClick?` | `() =&gt; void` | `-` | Click handler (used when href is not set). |

#### Component Source Code

##### File: `registry/components/unlumen/dock/index.tsx`

```tsx
"use client";

import * as React from "react";
import {
  motion,
  useMotionValue,
  useSpring,
  useTransform,
  type SpringOptions,
  AnimatePresence,
} from "motion/react";

import { cn } from "@/lib/utils";

export interface DockItem {
  icon: React.ReactNode;
  label: string;
  /** if provided, item renders as an `<a>` */
  href?: string;
  onClick?: () => void;
  /** renders a visual separator after this item */
  separator?: boolean;
}

export interface DockProps {
  items: DockItem[];
  /** @default 1.8 */
  magnification?: number;
  /** cursor radius (px) within which neighbors are magnified — @default 120 */
  distance?: number;
  /** @default 40 */
  iconSize?: number;
  /** @default 4 */
  gap?: number;
  /** @default 16 */
  borderRadius?: number;
  /** show labels permanently instead of on hover — @default false */
  alwaysShowLabels?: boolean;
  springOptions?: SpringOptions;
  className?: string;
}

const DEFAULT_SPRING: SpringOptions = {
  stiffness: 400,
  damping: 25,
  mass: 0.4,
};

function DockSeparator() {
  return (
    <div className="mx-1 flex items-center self-stretch">
      <div className="h-6 w-px bg-foreground/10" />
    </div>
  );
}

function DockIcon({
  item,
  mouseX,
  magnification,
  distance,
  iconSize,
  borderRadius,
  alwaysShowLabels,
  springOptions,
  onHover,
  iconRef: externalIconRef,
}: {
  item: DockItem;
  mouseX: ReturnType<typeof useMotionValue<number>>;
  magnification: number;
  distance: number;
  iconSize: number;
  borderRadius: number;
  alwaysShowLabels: boolean;
  springOptions: SpringOptions;
  onHover: (ref: React.RefObject<HTMLDivElement | null> | null) => void;
  iconRef: React.RefObject<HTMLDivElement | null>;
}) {
  const wrapperRef = React.useRef<HTMLDivElement>(null);

  const distanceFromMouse = useTransform(mouseX, (val) => {
    const el = wrapperRef.current;
    if (!el) return distance * 100;
    const rect = el.getBoundingClientRect();
    return Math.abs(val - (rect.left + rect.width / 2));
  });

  const gaussian = (d: number) =>
    (magnification - 1) * Math.exp(-(d * d) / (2 * distance * distance)) + 1;

  const widthRaw = useTransform(
    distanceFromMouse,
    (d) => iconSize * gaussian(d),
  );
  const heightRaw = useTransform(
    distanceFromMouse,
    (d) => iconSize * gaussian(d),
  );

  const width = useSpring(widthRaw, springOptions);
  const height = useSpring(heightRaw, springOptions);

  const Tag = item.href ? "a" : "button";

  return (
    // fixed height in-flow; width animates to push neighbors
    <motion.div
      ref={wrapperRef}
      className="relative flex items-end justify-center"
      style={{ width, height: iconSize }}
    >
      {/* absolute, anchored bottom so icon grows upward */}
      <motion.div
        ref={externalIconRef}
        style={{ width, height, bottom: 0 }}
        className="absolute"
      >
        <Tag
          href={item.href}
          onClick={item.onClick}
          onMouseEnter={() => onHover(externalIconRef)}
          onMouseLeave={() => onHover(null)}
          aria-label={item.label}
          style={{ borderRadius }}
          className={cn(
            "flex h-full w-full items-center justify-center",
            "text-foreground/70 transition-colors duration-150",
            "hover:bg-foreground/[0.06] hover:text-foreground",
            "focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-foreground/20",
            "[&_svg]:size-[55%]",
          )}
        >
          {item.icon}
        </Tag>
      </motion.div>

      {alwaysShowLabels && (
        <span className="mt-0.5 text-[10px] font-medium tracking-tight text-foreground/40 whitespace-nowrap pointer-events-none select-none leading-none">
          {item.label}
        </span>
      )}
    </motion.div>
  );
}

export function Dock({
  items,
  magnification = 1.8,
  distance = 120,
  iconSize = 40,
  gap = 4,
  borderRadius = 16,
  alwaysShowLabels = false,
  springOptions = DEFAULT_SPRING,
  className,
}: DockProps) {
  const mouseX = useMotionValue(Infinity);
  const dockRef = React.useRef<HTMLDivElement>(null);

  const iconRefs = React.useRef<React.RefObject<HTMLDivElement | null>[]>(
    items.map(() => React.createRef<HTMLDivElement>()),
  );

  const [hoveredIndex, setHoveredIndex] = React.useState<number | null>(null);
  const [tooltipX, setTooltipX] = React.useState(0);
  const [tooltipBottomOffset, setTooltipBottomOffset] = React.useState(0);

  React.useEffect(() => {
    if (hoveredIndex === null) return;

    let raf: number;
    const update = () => {
      const iconEl = iconRefs.current[hoveredIndex]?.current;
      const dockEl = dockRef.current;
      if (iconEl && dockEl) {
        const iconRect = iconEl.getBoundingClientRect();
        const dockRect = dockEl.getBoundingClientRect();
        setTooltipX(iconRect.left - dockRect.left + iconRect.width / 2);
        setTooltipBottomOffset(dockRect.bottom - iconRect.top);
      }
      raf = requestAnimationFrame(update);
    };
    raf = requestAnimationFrame(update);
    return () => cancelAnimationFrame(raf);
  }, [hoveredIndex]);

  const handleHover = React.useCallback(
    (ref: React.RefObject<HTMLDivElement | null> | null) => {
      if (ref === null) {
        setHoveredIndex(null);
        return;
      }
      const idx = iconRefs.current.findIndex((r) => r === ref);
      setHoveredIndex(idx >= 0 ? idx : null);
    },
    [],
  );

  return (
    <motion.div
      ref={dockRef}
      className={cn(
        "relative flex items-end overflow-visible border border-foreground/[0.08] bg-background/80 px-2 py-2 shadow-none hover:shadow-[0_0_0_1px_rgba(0,0,0,0.02),0_2px_8px_rgba(0,0,0,0.04),0_8px_24px_rgba(0,0,0,0.06)] transition-shadow duration-200 backdrop-blur-xl",
        className,
      )}
      style={{ gap, borderRadius }}
      onMouseMove={(e) => mouseX.set(e.clientX)}
      onMouseLeave={() => mouseX.set(Infinity)}
    >
      {items.map((item, i) => (
        <React.Fragment key={i}>
          <DockIcon
            item={item}
            mouseX={mouseX}
            magnification={magnification}
            distance={distance}
            iconSize={iconSize}
            borderRadius={borderRadius}
            alwaysShowLabels={alwaysShowLabels}
            springOptions={springOptions}
            onHover={handleHover}
            iconRef={iconRefs.current[i]}
          />
          {item.separator && <DockSeparator />}
        </React.Fragment>
      ))}

      {!alwaysShowLabels && (
        <AnimatePresence>
          {hoveredIndex !== null && (
            <motion.div
              key="dock-tooltip"
              layoutId="dock-tooltip"
              className="pointer-events-none absolute flex flex-col items-center z-50"
              style={{
                left: tooltipX,
                bottom: tooltipBottomOffset + 8,
                x: "-50%",
              }}
              initial={{ opacity: 0, y: 6, scale: 0.94 }}
              animate={{ opacity: 1, y: 0, scale: 1 }}
              exit={{ opacity: 0, y: 6, scale: 0.94 }}
              transition={{ duration: 0.13, ease: "easeOut" }}
            >
              <span className="rounded-md border border-foreground/10 bg-background px-2 py-1 text-sm font-medium text-foreground shadow-sm whitespace-nowrap">
                {items[hoveredIndex].label}
              </span>
              <svg
                width="8"
                height="4"
                viewBox="0 0 8 4"
                className="-mt-px text-background"
                aria-hidden
              >
                <path d="M0 0L4 4L8 0" fill="currentColor" />
              </svg>
            </motion.div>
          )}
        </AnimatePresence>
      )}
    </motion.div>
  );
}
```

---

### Expandable Navbar <a name="expandable-navbar"></a>

A full-width fixed header with velocity-highlight hover, an expanding inline dropdown that grows the navbar height, scroll hide/show, and a mobile circle-reveal overlay.

- **Tier**: Pro
- **Category**: Navigation

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { ExpandableNavbar } from "@/components/unlumen-ui/expandable-navbar";

const LINKS = [
  { label: "About", href: "/about" },
  {
    label: "Products",
    href: "#",
    children: [
      {
        label: "Analytics",
        href: "/products/analytics",
        description: "Understand your data.",
      },
      {
        label: "Automation",
        href: "/products/automation",
        description: "Streamline workflows.",
      },
    ],
  },
  { label: "Pricing", href: "/pricing" },
];

<ExpandableNavbar
  logo={<img src="/logo.svg" alt="Logo" className="h-6" />}
  links={LINKS}
  cta={{ label: "Get started", href: "/signup" }}
/>;
```

#### Props

##### `ExpandableNavbar` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `logo?` | `React.ReactNode` | `-` | Logo element rendered on the left side of the navbar. |
| `links?` | `NavLink[]` | `[]` | Navigation links. Links with `children` trigger the expanding inline dropdown. |
| `cta?` | `{ label: string; href: string }` | `-` | Optional primary CTA button shown on the right (desktop only). |
| `showThemeToggle?` | `boolean` | `true` | Show a sun/moon theme toggle button. Requires a next-themes provider. |
| `className?` | `string` | `-` | Additional className for the outer motion wrapper. |

##### `NavLink` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `label` | `string` | `-` | Text label for the link. |
| `href` | `string` | `-` | URL the link navigates to. |
| `children?` | `{ label: string; href: string; description?: string }[]` | `-` | Sub-links. When present, hovering the link expands the navbar body to reveal a grid of sub-links with staggered entry. |

---

### Floating Navbar <a name="floating-navbar"></a>

A sticky floating pill navbar with scroll-velocity spring physics, dropdown menus, mobile overlay, and optional theme toggle.

- **Tier**: Pro
- **Category**: Navigation

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import { FloatingNavbar } from "@/components/unlumen-ui/floating-navbar";

const LINKS = [
  { label: "About", href: "/about" },
  {
    label: "Products",
    href: "#",
    children: [
      {
        label: "Analytics",
        href: "/products/analytics",
        description: "Understand your data.",
      },
      {
        label: "Automation",
        href: "/products/automation",
        description: "Streamline workflows.",
      },
    ],
  },
  { label: "Pricing", href: "/pricing" },
];

<FloatingNavbar
  logo={<img src="/logo.svg" alt="Logo" className="h-6" />}
  links={LINKS}
  cta={{ label: "Get started", href: "/signup" }}
/>;
```

#### Props

##### `FloatingNavbar` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `logo?` | `React.ReactNode` | `-` | Logo element rendered on the left side of the navbar. |
| `links?` | `NavLink[]` | `[]` | Navigation links. Each link can optionally have children for a dropdown. |
| `cta?` | `{ label: string; href: string }` | `-` | Optional CTA button shown on the right. Animates in once the user scrolls past ctaThreshold. |
| `ctaThreshold?` | `number` | `80` | Scroll distance in pixels before the CTA button appears. |
| `showThemeToggle?` | `boolean` | `true` | Show a sun/moon theme toggle button. Requires a next-themes provider. |
| `className?` | `string` | `-` | Additional className for the outer wrapper. |

##### `NavLink` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `label` | `string` | `-` | Text label for the link. |
| `href` | `string` | `-` | URL the link navigates to. |
| `children?` | `{ label: string; href: string; description?: string }[]` | `-` | Optional sub-links rendered in a dropdown panel. |

---

### Gooey Navbar Menu <a name="gooey-navbar-menu"></a>

A spring-animated navbar menu with a gooey SVG-filtered background that connects the active trigger and dropdown panel.

- **Tier**: Pro
- **Category**: Navigation

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import {
  GooeyNavbarMenu,
  GooeyNavbarMenuContent,
  GooeyNavbarMenuItem,
  GooeyNavbarMenuLink,
  GooeyNavbarMenuList,
  GooeyNavbarMenuTrigger,
} from "@/components/unlumen-ui/gooey-navbar-menu";

const contentHighlightClassName =
  "bg-primary/10 rounded-lg ring-1 ring-primary/15";

export default function Example() {
  return (
    <GooeyNavbarMenu
      viewportClassName="bg-surface border-none shadow-none"
      gooeyClassName="bg-surface rounded-xl"
      gooeyStrength={12}
    >
      <GooeyNavbarMenuList>
        <GooeyNavbarMenuItem value="products">
          <GooeyNavbarMenuTrigger>Products</GooeyNavbarMenuTrigger>
          <GooeyNavbarMenuContent
            highlightClassName={contentHighlightClassName}
          >
            <div className="grid w-[400px] grid-cols-2 gap-1">
              <GooeyNavbarMenuLink href="/analytics">
                <span className="text-sm font-medium">Analytics</span>
                <span className="text-muted-foreground text-xs">
                  Understand your data flow.
                </span>
              </GooeyNavbarMenuLink>
              <GooeyNavbarMenuLink href="/automation">
                <span className="text-sm font-medium">Automation</span>
                <span className="text-muted-foreground text-xs">
                  Streamline workflows.
                </span>
              </GooeyNavbarMenuLink>
            </div>
          </GooeyNavbarMenuContent>
        </GooeyNavbarMenuItem>

        <GooeyNavbarMenuItem>
          <GooeyNavbarMenuLink
            href="/pricing"
            className="px-4 py-2 text-sm font-medium"
          >
            Pricing
          </GooeyNavbarMenuLink>
        </GooeyNavbarMenuItem>
      </GooeyNavbarMenuList>
    </GooeyNavbarMenu>
  );
}
```

#### Props

##### `GooeyNavbarMenu` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `viewport?` | `boolean` | `true` | When true, renders content inside the shared morphing viewport. The gooey bridge is only rendered when the shared viewport is enabled. |
| `viewportClassName?` | `string` | `-` | Additional classes applied to the morphing viewport. |
| `gooey?` | `boolean` | `true` | When true, renders the SVG-filtered background that connects the active trigger and shared viewport. |
| `gooeyClassName?` | `string` | `-` | Additional classes applied to the gooey background shapes. Match this to the viewport background for the cleanest merge. |
| `gooeyFilterId?` | `string` | `-` | Custom SVG filter id used by the gooey background. Useful when you need a stable id. |
| `gooeyStrength?` | `number` | `12` | Blur strength used by the gooey SVG filter. Higher values create a stronger merge between shapes. |
| `springBounce?` | `number` | `0` | Bounce used by the spring transition for the viewport, bridge, and content animations. |
| `springStiffness?` | `number` | `350` | Stiffness used by the spring transition for the viewport, bridge, and content animations. |
| `springDamping?` | `number` | `32` | Damping used by the spring transition for the viewport, bridge, and content animations. |
| `value?` | `string` | `-` | Controlled active item value. Use an empty string to close the menu. |
| `onValueChange?` | `(value: string) =&gt; void` | `-` | Callback fired when the active item value changes. |
| `className?` | `string` | `-` | Additional classes applied to the root element. |

##### `GooeyNavbarMenuList` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `className?` | `string` | `-` | Additional classes applied to the list element. |
| `highlightClassName?` | `string` | `-` | Additional classes applied to the hover highlight rendered behind trigger items. |

##### `GooeyNavbarMenuItem` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `value?` | `string` | `-` | Unique identifier for this item. Required when the item has a Trigger + Content pair. |
| `className?` | `string` | `-` | Additional classes applied to the list item. |

##### `GooeyNavbarMenuTrigger` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `className?` | `string` | `-` | Additional classes applied to the trigger button. |
| `children?` | `React.ReactNode` | `-` | Trigger content. |

##### `GooeyNavbarMenuContent` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `className?` | `string` | `-` | Additional classes applied to the content wrapper. |
| `highlightClassName?` | `string` | `-` | Additional classes applied to the hover highlight rendered behind links inside the content. |
| `innerClassName?` | `string` | `-` | Additional classes applied to the inner content container. |

##### `GooeyNavbarMenuLink` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `href?` | `string` | `-` | Destination URL for the anchor. |
| `className?` | `string` | `-` | Additional classes applied to the link. |
| `children?` | `React.ReactNode` | `-` | Link content. |

---

### Motion Navigation Menu <a name="motion-navigation-menu"></a>

A spring-animated navigation menu with a single morphing container, layout-animated active pill, and direction-aware content transitions.

- **Tier**: Free
- **Category**: Navigation

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/motion-navigation-menu.json
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
  - `lucide-react`
  - `class-variance-authority`

#### How to Use

```tsx
import {
  MotionNavigationMenu,
  MotionNavigationMenuContent,
  MotionNavigationMenuIndicator,
  MotionNavigationMenuItem,
  MotionNavigationMenuLink,
  MotionNavigationMenuList,
  MotionNavigationMenuTrigger,
} from "@/components/unlumen-ui/motion-navigation-menu";

const listHighlightClassName = "bg-primary/10 rounded-lg";
const contentHighlightClassName =
  "bg-primary/10 rounded-lg ring-1 ring-primary/15";

export default function Example() {
  return (
    <MotionNavigationMenu
      viewportClassName="bg-surface border-none shadow-none"
      springStiffness={350}
      springDamping={32}
    >
      <MotionNavigationMenuList highlightClassName={listHighlightClassName}>
        <MotionNavigationMenuItem value="products">
          <MotionNavigationMenuTrigger>Products</MotionNavigationMenuTrigger>
          <MotionNavigationMenuContent
            highlightClassName={contentHighlightClassName}
          >
            <div className="grid w-[400px] grid-cols-2 gap-1">
              <MotionNavigationMenuLink href="/analytics">
                <span className="text-sm font-medium">Analytics</span>
                <span className="text-muted-foreground text-xs">
                  Understand your data flow.
                </span>
              </MotionNavigationMenuLink>
              <MotionNavigationMenuLink href="/automation">
                <span className="text-sm font-medium">Automation</span>
                <span className="text-muted-foreground text-xs">
                  Streamline workflows.
                </span>
              </MotionNavigationMenuLink>
            </div>
          </MotionNavigationMenuContent>
        </MotionNavigationMenuItem>

        <MotionNavigationMenuIndicator />

        <MotionNavigationMenuItem>
          <MotionNavigationMenuLink
            href="/pricing"
            className="px-4 py-2 text-sm font-medium"
          >
            Pricing
          </MotionNavigationMenuLink>
        </MotionNavigationMenuItem>
      </MotionNavigationMenuList>
    </MotionNavigationMenu>
  );
}
```

#### Demo Code

```tsx
"use client";

import {
  ArrowUpRight,
  BookOpen,
  Building2,
  ChartNoAxesColumn,
  type LucideIcon,
  Rocket,
  Sparkles,
  Users,
} from "lucide-react";

import {
  MotionNavigationMenu,
  MotionNavigationMenuContent,
  MotionNavigationMenuItem,
  MotionNavigationMenuLink,
  MotionNavigationMenuList,
  MotionNavigationMenuTrigger,
} from "@/registry/components/unlumen/motion-navigation-menu";

interface MotionNavigationMenuDemoProps {
  viewport?: boolean;
  springBounce?: number;
  springStiffness?: number;
  springDamping?: number;
}

const PRODUCTS = [
  { title: "Analytics", desc: "Live funnels, cohorts, and retention." },
  { title: "Automation", desc: "Trigger workflows from events." },
  { title: "Insights", desc: "AI recommendations for next steps." },
  { title: "Reports", desc: "Share snapshots with stakeholders." },
];

const SOLUTIONS: { title: string; desc: string; icon: LucideIcon }[] = [
  {
    title: "Startups",
    desc: "Launch dashboards without building infra.",
    icon: Rocket,
  },
  {
    title: "Agencies",
    desc: "Manage every client workspace from one view.",
    icon: Users,
  },
  {
    title: "Enterprise",
    desc: "SAML, audit logs, SLAs, and permissions.",
    icon: Building2,
  },
];

const RESOURCES = [
  { title: "Documentation", desc: "Guides and API reference." },
  { title: "Changelog", desc: "What shipped this month." },
  { title: "Blog", desc: "Engineering and product notes." },
];

const highlightClassName = "dark:bg-accent bg-foreground/[0.06] rounded-lg";

export function MotionNavigationMenuDemo({
  viewport,
  springBounce,
  springStiffness,
  springDamping,
}: MotionNavigationMenuDemoProps) {
  return (
    <div className="flex min-h-full w-full items-center justify-center px-6">
      <MotionNavigationMenu
        viewport={viewport}
        viewportClassName="bg-surface border-none! rounded-xl shadow-none ring-0"
        springBounce={springBounce}
        springStiffness={springStiffness}
        springDamping={springDamping}
      >
        <MotionNavigationMenuList>
          <MotionNavigationMenuItem value="products">
            <MotionNavigationMenuTrigger>Products</MotionNavigationMenuTrigger>
            <MotionNavigationMenuContent
              highlightClassName={highlightClassName}
            >
              <div className="grid w-[500px] grid-cols-[1fr_1.25fr] gap-2">
                <MotionNavigationMenuLink
                  href="#"
                  className="bg-background/70 rounded-lg min-h-44 justify-between p-4"
                >
                  <span className="bg-background flex size-9 items-center justify-center rounded-lg border">
                    <ChartNoAxesColumn className="size-4" />
                  </span>
                  <span className="space-y-1">
                    <span className="block text-sm font-medium">
                      Command center
                    </span>
                    <span className="text-muted-foreground block text-xs">
                      Monitor product growth, workflow health, and team output.
                    </span>
                  </span>
                </MotionNavigationMenuLink>
                <div className="grid grid-cols-2 gap-0.5">
                  {PRODUCTS.map((product) => (
                    <MotionNavigationMenuLink key={product.title} href="#">
                      <span className="flex items-center justify-between gap-2 text-sm font-medium">
                        {product.title}
                        <ArrowUpRight className="size-3" />
                      </span>
                      <span className="text-muted-foreground text-xs">
                        {product.desc}
                      </span>
                    </MotionNavigationMenuLink>
                  ))}
                </div>
              </div>
            </MotionNavigationMenuContent>
          </MotionNavigationMenuItem>

          <MotionNavigationMenuItem value="solutions">
            <MotionNavigationMenuTrigger>Solutions</MotionNavigationMenuTrigger>
            <MotionNavigationMenuContent
              highlightClassName={highlightClassName}
            >
              <div className="w-[380px] space-y-1">
                <div className="text-muted-foreground px-2 py-2 text-xs font-medium">
                  Built for teams
                </div>
                {SOLUTIONS.map((solution) => (
                  <MotionNavigationMenuLink
                    key={solution.title}
                    href="#"
                    className="grid grid-cols-[auto_1fr_auto] items-center gap-3"
                  >
                    <span className="bg-transparent flex size-8 items-center justify-center rounded-lg">
                      <solution.icon className="size-4.5 text-foreground" />
                    </span>
                    <span className="space-y-0.5">
                      <span className="block text-sm font-medium">
                        For {solution.title.toLowerCase()}
                      </span>
                      <span className="text-muted-foreground block text-xs">
                        {solution.desc}
                      </span>
                    </span>
                    <span className=" text-foreground rounded-lg  px-1.5 py-0.5 text-xs">
                      View
                    </span>
                  </MotionNavigationMenuLink>
                ))}
              </div>
            </MotionNavigationMenuContent>
          </MotionNavigationMenuItem>

          <MotionNavigationMenuItem value="resources">
            <MotionNavigationMenuTrigger>Resources</MotionNavigationMenuTrigger>
            <MotionNavigationMenuContent
              highlightClassName={highlightClassName}
            >
              <div className="grid w-[460px] grid-cols-2 gap-2">
                <div className="space-y-0.5">
                  {RESOURCES.map((resource) => (
                    <MotionNavigationMenuLink key={resource.title} href="#">
                      <span className="flex items-center gap-2 text-sm font-medium">
                        <BookOpen className="size-3.5" />
                        {resource.title}
                      </span>
                      <span className="text-muted-foreground text-xs">
                        {resource.desc}
                      </span>
                    </MotionNavigationMenuLink>
                  ))}
                </div>
                <MotionNavigationMenuLink
                  href="#"
                  className="bg-background/70 min-h-44 justify-between p-4"
                >
                  <span className="flex items-center gap-2 text-sm font-medium">
                    <Sparkles className="size-4" />
                    New release
                  </span>
                  <span className="text-muted-foreground text-xs">
                    Explore the latest workflow templates and API improvements.
                  </span>
                  <span className="text-xs font-medium">Read changelog</span>
                </MotionNavigationMenuLink>
              </div>
            </MotionNavigationMenuContent>
          </MotionNavigationMenuItem>

          <MotionNavigationMenuItem>
            <MotionNavigationMenuLink
              href="#"
              className="flex h-9 items-center px-4 py-2 text-sm font-medium"
            >
              Pricing
            </MotionNavigationMenuLink>
          </MotionNavigationMenuItem>
        </MotionNavigationMenuList>
      </MotionNavigationMenu>
    </div>
  );
}

export default MotionNavigationMenuDemo;
```

#### Props

##### `MotionNavigationMenu` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `viewport?` | `boolean` | `true` | When true, renders content inside the shared morphing viewport. When false, each content panel renders absolutely below its item. |
| `viewportClassName?` | `string` | `-` | Additional classes applied to the morphing viewport. |
| `springBounce?` | `number` | `0` | Bounce used by the spring transition for the viewport and content animations. |
| `springStiffness?` | `number` | `350` | Stiffness used by the spring transition for the viewport and content animations. |
| `springDamping?` | `number` | `32` | Damping used by the spring transition for the viewport and content animations. |
| `value?` | `string` | `-` | Controlled active item value. Use an empty string to close the menu. |
| `onValueChange?` | `(value: string) =&gt; void` | `-` | Callback fired when the active item value changes. |
| `className?` | `string` | `-` | Additional classes applied to the root element. |

##### `MotionNavigationMenuList` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `className?` | `string` | `-` | Additional classes applied to the list element. |
| `highlightClassName?` | `string` | `-` | Additional classes applied to the hover highlight rendered behind trigger items. |

##### `MotionNavigationMenuItem` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `value?` | `string` | `-` | Unique identifier for this item. Required when the item has a Trigger + Content pair. |
| `className?` | `string` | `-` | Additional classes applied to the list item. |

##### `MotionNavigationMenuTrigger` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `className?` | `string` | `-` | Additional classes applied to the trigger button. |
| `children?` | `React.ReactNode` | `-` | Trigger content. |

##### `MotionNavigationMenuContent` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `className?` | `string` | `-` | Additional classes applied to the content wrapper. |
| `highlightClassName?` | `string` | `-` | Additional classes applied to the hover highlight rendered behind links inside the content. |
| `innerClassName?` | `string` | `-` | Additional classes applied to the inner content container. |

##### `MotionNavigationMenuLink` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `href?` | `string` | `-` | Destination URL for the anchor. |
| `className?` | `string` | `-` | Additional classes applied to the link. |
| `children?` | `React.ReactNode` | `-` | Link content. |

##### `MotionNavigationMenuIndicator` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `className?` | `string` | `-` | Additional classes applied to the indicator wrapper. |

#### Component Source Code

##### File: `registry/components/unlumen/motion-navigation-menu/index.tsx`

```tsx
"use client";

import * as React from "react";
import { cva } from "class-variance-authority";
import { AnimatePresence, motion } from "framer-motion";
import { ChevronDownIcon } from "lucide-react";

import { cn } from "@/lib/utils";
import {
  Highlight,
  HighlightItem,
} from "@/components/unlumen-ui/primitives/effects/highlight";

type Spring = {
  type: "spring";
  stiffness?: number;
  damping?: number;
  bounce: number;
};

type ContentRecord = {
  children: React.ReactNode;
  className?: string;
  highlightClassName?: string;
  innerClassName?: string;
};

type MotionNavigationMenuContextValue = {
  activeValue: string;
  direction: number;
  spring: Spring;
  viewport: boolean;
  viewportX: number | null;
  openValue: (value: string) => void;
  closeMenu: () => void;
  registerContent: (value: string, content: ContentRecord) => () => void;
  updateViewportPosition: () => void;
};

type MotionNavigationMenuItemContextValue = {
  value?: string;
};

const MotionNavigationMenuContext =
  React.createContext<MotionNavigationMenuContextValue | null>(null);

const MotionNavigationMenuItemContext =
  React.createContext<MotionNavigationMenuItemContextValue | null>(null);

const contentVariants = {
  initial: (direction: number) => ({ x: `${100 * direction}%`, opacity: 0 }),
  active: { x: "0%", opacity: 1 },
  exit: (direction: number) => ({ x: `${-100 * direction}%`, opacity: 0 }),
};

type MotionNavigationMenuProps = Omit<
  React.ComponentPropsWithRef<"nav">,
  "onValueChange"
> & {
  viewport?: boolean;
  viewportClassName?: string;
  springBounce?: number;
  springStiffness?: number;
  springDamping?: number;
  value?: string;
  onValueChange?: (value: string) => void;
};

function MotionNavigationMenu({
  className,
  children,
  viewport = true,
  viewportClassName,
  springBounce = 0,
  springStiffness = 350,
  springDamping = 32,
  value,
  onValueChange,
  onPointerLeave,
  onKeyDown,
  ref,
  ...props
}: MotionNavigationMenuProps) {
  const rootRef = React.useRef<HTMLElement | null>(null);
  const frameRef = React.useRef<number | null>(null);
  const lastActiveValueRef = React.useRef(value ?? "");
  const isControlled = value !== undefined;
  const [internalValue, setInternalValue] = React.useState("");
  const [direction, setDirection] = React.useState(1);
  const [viewportX, setViewportX] = React.useState<number | null>(null);
  const [contentByValue, setContentByValue] = React.useState<
    Record<string, ContentRecord>
  >({});

  const activeValue = value ?? internalValue;

  const spring = React.useMemo(
    () => ({
      type: "spring" as const,
      bounce: springBounce,
      stiffness: springStiffness,
      damping: springDamping,
    }),
    [springBounce, springStiffness, springDamping],
  );

  const getItemValues = React.useCallback(() => {
    const root = rootRef.current;

    if (!root) {
      return [];
    }

    return Array.from(
      root.querySelectorAll<HTMLElement>(
        '[data-slot="navigation-menu-item"][data-value]',
      ),
      (item) => item.dataset.value ?? "",
    ).filter(Boolean);
  }, []);

  const updateViewportPosition = React.useCallback(() => {
    if (frameRef.current !== null) {
      cancelAnimationFrame(frameRef.current);
    }

    frameRef.current = requestAnimationFrame(() => {
      const root = rootRef.current;

      if (!root) {
        return;
      }

      const rootRect = root.getBoundingClientRect();
      const activeTrigger = root.querySelector<HTMLElement>(
        '[data-slot="navigation-menu-trigger"][data-state="open"]',
      );

      if (!activeTrigger) {
        setViewportX(rootRect.width / 2);
        return;
      }

      const triggerRect = activeTrigger.getBoundingClientRect();
      const idealX = triggerRect.left - rootRect.left + triggerRect.width / 2;

      const measureEl = root.querySelector<HTMLElement>(
        '[data-slot="navigation-menu-measure"]',
      );
      const viewportEl = root.querySelector<HTMLElement>(
        '[data-slot="navigation-menu-viewport"]',
      );
      const contentWidth =
        (measureEl ? measureEl.offsetWidth : 0) ||
        (viewportEl ? viewportEl.offsetWidth : 0);
      const half = contentWidth / 2;

      if (contentWidth > 0) {
        // Find the nearest clipping ancestor to use as the boundary
        let boundary: DOMRect | null = null;
        let ancestor = root.parentElement;
        while (ancestor && ancestor !== document.body) {
          const style = window.getComputedStyle(ancestor);
          const overflow = style.overflow + style.overflowX;
          if (/hidden|clip|scroll|auto/.test(overflow)) {
            boundary = ancestor.getBoundingClientRect();
            break;
          }
          ancestor = ancestor.parentElement;
        }
        if (!boundary) {
          boundary = document.documentElement.getBoundingClientRect();
        }

        const margin = 8;
        const dropLeft = rootRect.left + idealX - half;
        const dropRight = rootRect.left + idealX + half;

        let adjustment = 0;
        if (dropLeft < boundary.left + margin) {
          adjustment = boundary.left + margin - dropLeft;
        } else if (dropRight > boundary.right - margin) {
          adjustment = boundary.right - margin - dropRight;
        }

        setViewportX(idealX + adjustment);
      } else {
        setViewportX(idealX);
      }
    });
  }, []);

  const setRootRef = React.useCallback(
    (node: HTMLElement | null) => {
      rootRef.current = node;

      if (typeof ref === "function") {
        ref(node);
      } else if (ref) {
        ref.current = node;
      }
    },
    [ref],
  );

  const setActiveValue = React.useCallback(
    (nextValue: string) => {
      if (!isControlled) {
        setInternalValue(nextValue);
      }

      onValueChange?.(nextValue);
    },
    [isControlled, onValueChange],
  );

  const openValue = React.useCallback(
    (nextValue: string) => {
      if (!nextValue || nextValue === lastActiveValueRef.current) {
        return;
      }

      const itemValues = getItemValues();
      const previousIndex = itemValues.indexOf(lastActiveValueRef.current);
      const nextIndex = itemValues.indexOf(nextValue);

      if (previousIndex !== -1 && nextIndex !== -1) {
        setDirection(nextIndex > previousIndex ? 1 : -1);
      }

      lastActiveValueRef.current = nextValue;
      setActiveValue(nextValue);
      updateViewportPosition();
    },
    [getItemValues, setActiveValue, updateViewportPosition],
  );

  const closeMenu = React.useCallback(() => {
    lastActiveValueRef.current = "";
    setActiveValue("");
    updateViewportPosition();
  }, [setActiveValue, updateViewportPosition]);

  const registerContent = React.useCallback(
    (value: string, content: ContentRecord) => {
      setContentByValue((current) => {
        const previous = current[value];

        if (
          previous?.children === content.children &&
          previous?.className === content.className &&
          previous?.innerClassName === content.innerClassName
        ) {
          return current;
        }

        return { ...current, [value]: content };
      });

      return () => {
        setContentByValue((current) => {
          if (!current[value]) {
            return current;
          }

          const next = { ...current };
          delete next[value];
          return next;
        });
      };
    },
    [],
  );

  React.useEffect(() => {
    if (value === undefined) {
      return;
    }

    if (!value) {
      lastActiveValueRef.current = "";
      return;
    }

    openValue(value);
  }, [openValue, value]);

  React.useLayoutEffect(() => {
    updateViewportPosition();
  }, [activeValue, updateViewportPosition]);

  React.useLayoutEffect(() => {
    const root = rootRef.current;

    if (!root || typeof ResizeObserver === "undefined") {
      return () => {
        if (frameRef.current !== null) {
          cancelAnimationFrame(frameRef.current);
        }
      };
    }

    const observer = new ResizeObserver(updateViewportPosition);
    observer.observe(root);

    return () => {
      observer.disconnect();

      if (frameRef.current !== null) {
        cancelAnimationFrame(frameRef.current);
      }
    };
  }, [updateViewportPosition]);

  React.useEffect(() => {
    function handlePointerDown(event: PointerEvent) {
      if (
        rootRef.current &&
        event.target instanceof Node &&
        !rootRef.current.contains(event.target)
      ) {
        closeMenu();
      }
    }

    document.addEventListener("pointerdown", handlePointerDown);
    return () => document.removeEventListener("pointerdown", handlePointerDown);
  }, [closeMenu]);

  const contextValue = React.useMemo(
    () => ({
      activeValue,
      direction,
      spring,
      viewport,
      viewportX,
      openValue,
      closeMenu,
      registerContent,
      updateViewportPosition,
    }),
    [
      activeValue,
      closeMenu,
      direction,
      openValue,
      registerContent,
      spring,
      updateViewportPosition,
      viewport,
      viewportX,
    ],
  );

  return (
    <MotionNavigationMenuContext.Provider value={contextValue}>
      <nav
        ref={setRootRef}
        data-slot="navigation-menu"
        data-viewport={viewport}
        className={cn(
          "group/navigation-menu relative flex max-w-max flex-1 items-center justify-center",
          className,
        )}
        onPointerLeave={(event) => {
          onPointerLeave?.(event);
          closeMenu();
        }}
        onKeyDown={(event) => {
          onKeyDown?.(event);

          if (event.key === "Escape") {
            closeMenu();
          }
        }}
        {...props}
      >
        {children}
        {viewport && (
          <MotionNavigationMenuViewport
            className={viewportClassName}
            contentByValue={contentByValue}
          />
        )}
      </nav>
    </MotionNavigationMenuContext.Provider>
  );
}

function MotionNavigationMenuList({
  className,
  highlightClassName,
  ...props
}: React.ComponentPropsWithRef<"ul"> & {
  highlightClassName?: string;
}) {
  return (
    <Highlight
      mode="parent"
      controlledItems
      hover
      className={cn(
        "bg-accent rounded-md pointer-events-none",
        highlightClassName,
      )}
      style={{ zIndex: -1 }}
      containerClassName="relative"
    >
      <ul
        data-slot="navigation-menu-list"
        className={cn(
          "group relative z-10 flex flex-1 list-none items-center justify-center gap-1",
          className,
        )}
        {...props}
      />
    </Highlight>
  );
}

function MotionNavigationMenuItem({
  className,
  value,
  ...props
}: React.ComponentPropsWithRef<"li"> & {
  value?: string;
}) {
  const itemContextValue = React.useMemo(() => ({ value }), [value]);

  return (
    <MotionNavigationMenuItemContext.Provider value={itemContextValue}>
      <li
        data-slot="navigation-menu-item"
        data-value={value}
        className={cn("relative", className)}
        {...props}
      />
    </MotionNavigationMenuItemContext.Provider>
  );
}

const motionNavigationMenuTriggerStyle = cva(
  "group inline-flex h-9 w-max items-center justify-center rounded-md bg-transparent px-4 py-2 text-sm font-medium hover:text-accent-foreground focus:text-accent-foreground disabled:pointer-events-none disabled:opacity-50 data-[state=open]:text-accent-foreground focus-visible:ring-ring/50 outline-none transition-colors focus-visible:ring-[3px] focus-visible:outline-1",
);

function MotionNavigationMenuTrigger({
  className,
  children,
  onPointerEnter,
  onFocus,
  onClick,
  ...props
}: React.ComponentPropsWithRef<"button">) {
  const context = React.useContext(MotionNavigationMenuContext);
  const itemContext = React.useContext(MotionNavigationMenuItemContext);
  const value = itemContext?.value;
  const isOpen = !!value && context?.activeValue === value;

  return (
    <HighlightItem asChild>
      <button
        type="button"
        data-slot="navigation-menu-trigger"
        data-state={isOpen ? "open" : "closed"}
        aria-expanded={isOpen}
        className={cn(motionNavigationMenuTriggerStyle(), "group", className)}
        onPointerEnter={(event) => {
          onPointerEnter?.(event);

          if (value) {
            context?.openValue(value);
          }
        }}
        onFocus={(event) => {
          onFocus?.(event);

          if (value) {
            context?.openValue(value);
          }
        }}
        onClick={(event) => {
          onClick?.(event);

          if (value) {
            context?.openValue(value);
          }
        }}
        {...props}
      >
        {children}{" "}
        <motion.span
          aria-hidden="true"
          animate={{
            rotate: isOpen ? 180 : 0,
            y: isOpen ? 1 : 0,
          }}
          transition={{
            type: "spring",
            stiffness: 400,
            damping: 20,
          }}
          className="relative top-0 ml-1.5 inline-flex"
        >
          <ChevronDownIcon className="size-3.5 stroke-2.5" aria-hidden="true" />
        </motion.span>
      </button>
    </HighlightItem>
  );
}

function MotionNavigationMenuContent({
  className,
  highlightClassName,
  innerClassName,
  children,
}: React.ComponentPropsWithRef<"div"> & {
  highlightClassName?: string;
  innerClassName?: string;
}) {
  const context = React.useContext(MotionNavigationMenuContext);
  const itemContext = React.useContext(MotionNavigationMenuItemContext);
  const value = itemContext?.value;
  const isOpen = !!value && context?.activeValue === value;

  React.useLayoutEffect(() => {
    if (!context || !value || !context.viewport) {
      return;
    }

    return context.registerContent(value, {
      children,
      className,
      highlightClassName,
      innerClassName,
    });
  }, [children, className, context, highlightClassName, innerClassName, value]);

  if (!context || !value || context.viewport) {
    return null;
  }

  return (
    <AnimatePresence initial={false} custom={context.direction}>
      {isOpen && (
        <motion.div
          data-slot="navigation-menu-content"
          key={value}
          custom={context.direction}
          variants={contentVariants}
          initial="initial"
          animate="active"
          exit="exit"
          transition={context.spring}
          className={cn(
            "bg-background/90 text-popover-foreground absolute top-full left-0 z-50 mt-1.5 rounded-md border p-2 pr-2.5 shadow",
            className,
          )}
        >
          <MotionNavigationMenuContentInner
            highlightClassName={highlightClassName}
            innerClassName={innerClassName}
          >
            {children}
          </MotionNavigationMenuContentInner>
        </motion.div>
      )}
    </AnimatePresence>
  );
}

function MotionNavigationMenuContentInner({
  highlightClassName,
  innerClassName,
  children,
}: {
  highlightClassName?: string;
  innerClassName?: string;
  children: React.ReactNode;
}) {
  return (
    <Highlight
      mode="parent"
      controlledItems
      hover
      className={cn(
        "bg-accent rounded-sm pointer-events-none",
        highlightClassName,
      )}
      style={{ zIndex: -1 }}
      containerClassName="relative"
    >
      <div className={cn("relative z-10", innerClassName)}>{children}</div>
    </Highlight>
  );
}

function MotionNavigationMenuViewport({
  className,
  contentByValue,
}: React.ComponentPropsWithRef<"div"> & {
  contentByValue?: Record<string, ContentRecord>;
}) {
  const context = React.useContext(MotionNavigationMenuContext);
  const measureRef = React.useRef<HTMLDivElement | null>(null);
  const [size, setSize] = React.useState({ width: 0, height: 0 });
  const [lastSize, setLastSize] = React.useState({ width: 0, height: 0 });
  const activeContent =
    context?.activeValue && contentByValue
      ? contentByValue[context.activeValue]
      : undefined;

  React.useLayoutEffect(() => {
    const node = measureRef.current;

    if (!node || !activeContent) {
      return;
    }

    const updateSize = () => {
      const rect = node.getBoundingClientRect();
      const nextSize = {
        width: rect.width,
        height: rect.height,
      };

      setSize(nextSize);

      if (nextSize.width > 0 || nextSize.height > 0) {
        setLastSize(nextSize);
      }

      context?.updateViewportPosition();
    };

    updateSize();

    if (typeof ResizeObserver === "undefined") {
      return;
    }

    const observer = new ResizeObserver(updateSize);
    observer.observe(node);

    return () => observer.disconnect();
  }, [activeContent, context]);

  const width = size.width > 0 ? size.width : lastSize.width;
  const height = size.height > 0 ? size.height : lastSize.height;

  return (
    <motion.div
      className="absolute top-full isolate z-50 flex -translate-x-1/2 justify-center"
      initial={false}
      animate={{ left: context?.viewportX ?? "50%" }}
      transition={context?.spring}
    >
      <motion.div
        data-slot="navigation-menu-viewport"
        initial={false}
        animate={{
          width: activeContent ? width : 0,
          height: activeContent ? height : 0,
          opacity: activeContent ? 1 : 0,
          scale: activeContent ? 1 : 0.95,
        }}
        transition={context?.spring}
        className={cn(
          "bg-background text-popover-foreground relative mt-1.5 overflow-hidden rounded-md border shadow backdrop-blur-md",
          className,
        )}
      >
        <AnimatePresence
          mode="popLayout"
          initial={false}
          custom={context?.direction ?? 1}
        >
          {activeContent && context?.activeValue && (
            <motion.div
              data-slot="navigation-menu-content"
              key={context.activeValue}
              custom={context.direction}
              variants={contentVariants}
              initial="initial"
              animate="active"
              exit="exit"
              transition={context.spring}
              className={cn("p-2 pr-2.5", activeContent.className)}
            >
              <MotionNavigationMenuContentInner
                highlightClassName={activeContent.highlightClassName}
                innerClassName={activeContent.innerClassName}
              >
                {activeContent.children}
              </MotionNavigationMenuContentInner>
            </motion.div>
          )}
        </AnimatePresence>
      </motion.div>

      <div
        ref={measureRef}
        aria-hidden="true"
        data-slot="navigation-menu-measure"
        className="pointer-events-none invisible absolute top-1.5 left-0 w-max"
      >
        {activeContent && (
          <div className={cn("p-2 pr-2.5", activeContent.className)}>
            <MotionNavigationMenuContentInner
              highlightClassName={activeContent.highlightClassName}
              innerClassName={activeContent.innerClassName}
            >
              {activeContent.children}
            </MotionNavigationMenuContentInner>
          </div>
        )}
      </div>
    </motion.div>
  );
}

function MotionNavigationMenuLink({
  className,
  ...props
}: React.ComponentPropsWithRef<"a">) {
  return (
    <HighlightItem asChild>
      <a
        data-slot="navigation-menu-link"
        className={cn(
          "data-[active=true]:text-accent-foreground hover:text-accent-foreground focus:text-accent-foreground focus-visible:ring-ring/50 [&_svg:not([class*='text-'])]:text-muted-foreground flex flex-col gap-1 rounded-sm p-2 text-sm transition-colors outline-none focus-visible:ring-[3px] focus-visible:outline-1 [&_svg:not([class*='size-'])]:size-4",
          className,
        )}
        {...props}
      />
    </HighlightItem>
  );
}

function MotionNavigationMenuIndicator({
  className,
  ...props
}: React.ComponentPropsWithRef<"div">) {
  return (
    <div
      data-slot="navigation-menu-indicator"
      className={cn(
        "pointer-events-none top-full z-1 flex h-1.5 items-end justify-center overflow-hidden",
        className,
      )}
      {...props}
    >
      <div className="bg-border relative top-[60%] h-2 w-2 rotate-45 rounded-tl-sm shadow-md" />
    </div>
  );
}

export {
  MotionNavigationMenu,
  MotionNavigationMenuList,
  MotionNavigationMenuItem,
  MotionNavigationMenuContent,
  MotionNavigationMenuTrigger,
  MotionNavigationMenuLink,
  MotionNavigationMenuViewport,
  MotionNavigationMenuIndicator,
  motionNavigationMenuTriggerStyle,
};
```

---

### Motion Tabs Menu <a name="motion-tabs-menu"></a>

A floating bottom tab bar with animated width expansion, direction-aware content transitions, and spring-driven tap feedback.

- **Tier**: Pro
- **Category**: Navigation

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import MotionTabsMenu from "@/components/unlumen-ui/motion-tabs-menu";

export default function Page() {
  return (
    <div className="flex min-h-screen items-center justify-center">
      <MotionTabsMenu />
    </div>
  );
}
```

---

### Sidebar 001 <a name="sidebar-001"></a>

Animated vertical sidebar with spring hover highlight, animated active bar, collapsible groups, and drag-to-resize.

- **Tier**: Free
- **Category**: Navigation

#### Installation

```bash
npx shadcn@latest add https://ui.unlumen.com/r/sidebar-001.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### How to Use

```tsx
"use client";

import { usePathname } from "next/navigation";
import {
  Sidebar001,
  Sidebar001Content,
  Sidebar001Header,
  Sidebar001Footer,
  Sidebar001Section,
  Sidebar001Group,
  Sidebar001Item,
  useSidebar001Effects,
} from "@/components/unlumen-ui/sidebar-001";
import { BookOpen, Layers } from "lucide-react";

const nav = [
  {
    label: "Getting Started",
    items: [
      { href: "/docs/introduction", label: "Introduction" },
      { href: "/docs/installation", label: "Installation" },
      { href: "/docs/theming", label: "Theming", isNew: true },
    ],
  },
];

const advanced = [
  { href: "/docs/animations", label: "Animations" },
  { href: "/docs/accessibility", label: "Accessibility" },
];

function EffectsToggle() {
  const { enabled, toggle } = useSidebar001Effects();
  return (
    <button
      onClick={toggle}
      className="text-xs text-muted-foreground hover:text-foreground transition-colors"
    >
      Effects: {enabled ? "on" : "off"}
    </button>
  );
}

export function AppSidebar() {
  const pathname = usePathname();

  return (
    <Sidebar001 defaultWidth={260}>
      <Sidebar001Header>
        <span className="text-sm font-semibold">Docs</span>
      </Sidebar001Header>

      <Sidebar001Content>
        {/* Flat section with a heading */}
        {nav.map((section) => (
          <Sidebar001Section key={section.label} label={section.label}>
            {section.items.map((item) => (
              <Sidebar001Item
                key={item.href}
                href={item.href}
                label={item.label}
                isActive={pathname === item.href}
                isNew={item.isNew}
              />
            ))}
          </Sidebar001Section>
        ))}

        {/* Collapsible group */}
        <Sidebar001Group label="Advanced" icon={<Layers />} defaultOpen>
          {advanced.map((item) => (
            <Sidebar001Item
              key={item.href}
              href={item.href}
              label={item.label}
              isActive={pathname === item.href}
            />
          ))}
        </Sidebar001Group>
      </Sidebar001Content>

      <Sidebar001Footer>
        <EffectsToggle />
      </Sidebar001Footer>
    </Sidebar001>
  );
}
```

#### Demo Code

```tsx
"use client";

import { useState } from "react";
import { Bell, BookMarked, Layers } from "lucide-react";
import {
  Sidebar001,
  Sidebar001Content,
  Sidebar001Footer,
  Sidebar001Group,
  Sidebar001Header,
  Sidebar001Item,
  Sidebar001Section,
} from "@/registry/components/unlumen/sidebar-001";

const NAV = [
  {
    label: "Getting Started",
    items: [
      { href: "/docs/introduction", label: "Introduction" },
      { href: "/docs/installation", label: "Installation" },
      { href: "/docs/theming", label: "Theming", isNew: true },
      { href: "/docs/changelog", label: "Changelog" },
      { href: "/docs/faq", label: "FAQ" },
    ],
  },
  {
    label: "Components",
    items: [
      { href: "/docs/button", label: "Button" },
      { href: "/docs/card", label: "Card" },
      { href: "/docs/input", label: "Input", isNew: true },
      { href: "/docs/tabs", label: "Tabs" },
      { href: "/docs/badge", label: "Badge" },
      { href: "/docs/avatar", label: "Avatar" },
    ],
    groups: [
      {
        label: "Overlays",
        defaultOpen: true,
        icon: <Layers />,
        items: [
          { href: "/docs/dialog", label: "Dialog" },
          { href: "/docs/drawer", label: "Drawer", isNew: true },
          { href: "/docs/popover", label: "Popover" },
          { href: "/docs/tooltip", label: "Tooltip" },
          { href: "/docs/dropdown", label: "Dropdown" },
          { href: "/docs/modal", label: "Modal" },
        ],
      },
      {
        label: "Feedback",
        defaultOpen: false,
        icon: <Bell />,
        items: [
          { href: "/docs/toast", label: "Toast", isNew: true },
          { href: "/docs/alert", label: "Alert" },
          { href: "/docs/progress", label: "Progress" },
          { href: "/docs/skeleton", label: "Skeleton" },
          { href: "/docs/spinner", label: "Spinner" },
        ],
      },
    ],
  },
  {
    label: "Utilities",
    items: [
      { href: "/docs/animations", label: "Animations" },
      { href: "/docs/colors", label: "Colors" },
      { href: "/docs/typography", label: "Typography" },
      { href: "/docs/spacing", label: "Spacing" },
    ],
  },
  {
    label: "Resources",
    items: [
      { href: "/docs/accessibility", label: "Accessibility" },
      { href: "/docs/performance", label: "Performance" },
      { href: "/docs/best-practices", label: "Best Practices" },
      { href: "/docs/examples", label: "Examples" },
    ],
  },
];

export default function Sidebar001Demo() {
  const [active, setActive] = useState("/docs/introduction");

  return (
    <div className="flex h-full w-full overflow-hidden bg-background">
      <Sidebar001>
        <Sidebar001Header>
          <div className="flex items-center gap-2">
            <BookMarked size={18} />
            <span className="text-base font-semibold text-foreground">
              Docs
            </span>
          </div>
        </Sidebar001Header>
        <Sidebar001Content>
          {NAV.map((section) => (
            <Sidebar001Section key={section.label} label={section.label}>
              {section.items.map((item) => (
                <Sidebar001Item
                  key={item.href}
                  href={item.href}
                  label={item.label}
                  isActive={active === item.href}
                  isNew={item.isNew}
                  onClick={(e) => {
                    e.preventDefault();
                    setActive(item.href);
                  }}
                />
              ))}
              {section.groups?.map((group) => (
                <Sidebar001Group
                  key={group.label}
                  label={group.label}
                  defaultOpen={group.defaultOpen}
                  icon={group.icon}
                >
                  {group.items.map((item) => (
                    <Sidebar001Item
                      key={item.href}
                      href={item.href}
                      label={item.label}
                      isActive={active === item.href}
                      isNew={item.isNew}
                      onClick={(e) => {
                        e.preventDefault();
                        setActive(item.href);
                      }}
                    />
                  ))}
                </Sidebar001Group>
              ))}
            </Sidebar001Section>
          ))}
        </Sidebar001Content>
        <Sidebar001Footer>
          <span className="text-xs text-foreground/40">v1.0.0</span>
        </Sidebar001Footer>
      </Sidebar001>

      <div className="flex-1 flex flex-col gap-4 p-8 border-l border-border/50 overflow-y-auto no-scrollbar">
        <div className="h-5 w-32 rounded bg-foreground/8" />
        <div className="h-8 w-64 rounded bg-foreground/10" />
        <div className="flex flex-col gap-2 mt-2">
          <div className="h-3 w-full rounded bg-foreground/6" />
          <div className="h-3 w-5/6 rounded bg-foreground/6" />
          <div className="h-3 w-4/6 rounded bg-foreground/6" />
        </div>
        <div className="h-24 w-full rounded-lg bg-foreground/5 mt-2" />
        <div className="flex flex-col gap-2 mt-2">
          <div className="h-3 w-full rounded bg-foreground/6" />
          <div className="h-3 w-3/4 rounded bg-foreground/6" />
        </div>
      </div>
    </div>
  );
}
```

#### Props

##### `Sidebar001` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `children` | `React.ReactNode` | `-` | Sidebar contents — Header, Content, Footer. |
| `className?` | `string` | `-` | Additional classes on the root aside element. |
| `defaultEffectsEnabled?` | `boolean` | `true` | Initial value for the motion effects toggle (persisted in localStorage). |
| `defaultWidth?` | `number` | `240` | Initial sidebar width in px. |
| `minWidth?` | `number` | `160` | Minimum drag width in px. |
| `maxWidth?` | `number` | `400` | Maximum drag width in px. |

##### `Sidebar001Header` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `children?` | `React.ReactNode` | `-` | Content rendered at the top of the sidebar. |
| `className?` | `string` | `-` | Additional classes. |

##### `Sidebar001Content` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `children` | `React.ReactNode` | `-` | Scrollable body — typically Sidebar001Section and Sidebar001Group elements. |
| `className?` | `string` | `-` | Additional classes on the scroll container. |

##### `Sidebar001Section` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `label?` | `React.ReactNode` | `-` | Section heading shown above the group of items. |
| `children` | `React.ReactNode` | `-` | Sidebar001Item elements. |
| `className?` | `string` | `-` | Additional classes. |

##### `Sidebar001Group` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `label` | `React.ReactNode` | `-` | Group heading shown in the toggle button. |
| `children` | `React.ReactNode` | `-` | Sidebar001Item elements shown when the group is open. |
| `defaultOpen?` | `boolean` | `false` | Whether the group starts expanded. |
| `icon?` | `React.ReactNode` | `-` | Optional leading icon next to the label. Omit for compact chevron-only style. |
| `className?` | `string` | `-` | Additional classes. |

##### `Sidebar001Item` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `href` | `string` | `-` | Navigation target. |
| `label` | `React.ReactNode` | `-` | Text or node shown as the link label. |
| `isActive` | `boolean` | `-` | Marks this item as the current page. |
| `isNew?` | `boolean` | `-` | Shows a small accent dot after the label. |
| `className?` | `string` | `-` | Additional classes on the anchor element. |
| `onClick?` | `React.MouseEventHandler&lt;HTMLAnchorElement&gt;` | `-` | Click handler for the anchor. |

##### `Sidebar001Footer` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `children?` | `React.ReactNode` | `-` | Content rendered at the bottom of the sidebar, above a top border. |
| `className?` | `string` | `-` | Additional classes. |

#### Component Source Code

##### File: `registry/components/unlumen/sidebar-001/index.tsx`

```tsx
"use client";

import * as React from "react";
import {
  createContext,
  memo,
  useCallback,
  useContext,
  useEffect,
  useId,
  useMemo,
  useRef,
  useState,
} from "react";
import { motion, AnimatePresence } from "motion/react";
import { ChevronRight } from "lucide-react";
import { cn } from "@/lib/utils";

const MotionChevron = motion.create(ChevronRight);

const EFFECTS_KEY = "sidebar-001-effects";

const EffectsContext = createContext<{ enabled: boolean; toggle: () => void }>({
  enabled: true,
  toggle: () => {},
});

function EffectsProvider({
  children,
  defaultEnabled = true,
}: {
  children: React.ReactNode;
  defaultEnabled?: boolean;
}) {
  const [enabled, setEnabled] = useState(() => {
    if (typeof window === "undefined") return defaultEnabled;
    const stored = localStorage.getItem(EFFECTS_KEY);
    return stored !== null ? stored === "true" : defaultEnabled;
  });

  const toggle = useCallback(() => {
    setEnabled((prev) => {
      const next = !prev;
      localStorage.setItem(EFFECTS_KEY, String(next));
      return next;
    });
  }, []);

  const value = useMemo(() => ({ enabled, toggle }), [enabled, toggle]);
  return (
    <EffectsContext.Provider value={value}>{children}</EffectsContext.Provider>
  );
}

export function useSidebar001Effects() {
  return useContext(EffectsContext);
}

// ─── Hover context ────────────────────────────────────────────────────────────

interface HoverRect {
  top: number;
  height: number;
  left: number;
}

const HoverContext = createContext<{
  hovered: string | null;
  hoverRect: HoverRect | null;
  containerRef: React.RefObject<HTMLDivElement | null>;
  setHovered: (id: string | null, rect?: HoverRect | null) => void;
}>({
  hovered: null,
  hoverRect: null,
  containerRef: { current: null },
  setHovered: () => {},
});

function HoverProvider({
  children,
  containerRef,
}: {
  children: React.ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
}) {
  const [hovered, setHoveredId] = useState<string | null>(null);
  const [hoverRect, setHoverRect] = useState<HoverRect | null>(null);

  const setHovered = useCallback(
    (id: string | null, rect?: HoverRect | null) => {
      setHoveredId(id);
      setHoverRect(rect ?? null);
    },
    [],
  );

  const value = useMemo(
    () => ({ hovered, hoverRect, containerRef, setHovered }),
    [hovered, hoverRect, containerRef, setHovered],
  );

  return (
    <HoverContext.Provider value={value}>{children}</HoverContext.Provider>
  );
}

// ─── Scroll to active ─────────────────────────────────────────────────────────

function useScrollToActive(active: boolean) {
  const ref = useRef<HTMLDivElement>(null);
  const scrolled = useRef(false);

  useEffect(() => {
    if (!active || scrolled.current || !ref.current) return;
    scrolled.current = true;
    const el = ref.current;
    const schedule =
      typeof requestIdleCallback !== "undefined"
        ? (cb: () => void) => requestIdleCallback(cb)
        : (cb: () => void) => setTimeout(cb, 100);
    const cancel =
      typeof cancelIdleCallback !== "undefined"
        ? cancelIdleCallback
        : clearTimeout;
    const id = schedule(() => {
      const viewport = el.closest("[data-scroll-viewport]");
      if (!(viewport instanceof HTMLElement)) return;
      const vpRect = viewport.getBoundingClientRect();
      const elRect = el.getBoundingClientRect();
      const offset =
        elRect.top - vpRect.top - vpRect.height / 2 + elRect.height / 2;
      if (Math.abs(offset) > 40)
        viewport.scrollBy({ top: offset, behavior: "smooth" });
    });
    return () => cancel(id as number);
  }, [active]);

  useEffect(() => {
    if (!active) scrolled.current = false;
  }, [active]);

  return ref;
}

// ─── HoverHighlight ───────────────────────────────────────────────────────────

function HoverHighlight() {
  const { hoverRect, hovered } = useContext(HoverContext);
  const { enabled } = useContext(EffectsContext);

  return (
    <AnimatePresence>
      {enabled && hovered && hoverRect && (
        <motion.div
          key="sb001-hover-bg"
          className="pointer-events-none absolute z-0 rounded-md bg-accent/50"
          style={{ right: 0 }}
          initial={false}
          animate={{
            top: hoverRect.top + 2,
            height: hoverRect.height - 4,
            left: hoverRect.left,
            opacity: 1,
          }}
          exit={{ opacity: 0 }}
          transition={{ type: "spring", stiffness: 300, damping: 30 }}
        />
      )}
    </AnimatePresence>
  );
}

// ─── Sidebar001Item ───────────────────────────────────────────────────────────

export interface Sidebar001ItemProps {
  href: string;
  label: React.ReactNode;
  isActive: boolean;
  isNew?: boolean;
  className?: string;
  onClick?: React.MouseEventHandler<HTMLAnchorElement>;
}

export const Sidebar001Item = memo(function Sidebar001Item({
  href,
  label,
  isActive,
  isNew,
  className,
  onClick,
}: Sidebar001ItemProps) {
  const { hovered, setHovered, containerRef } = useContext(HoverContext);
  const isHovered = hovered === href;
  const itemRef = useScrollToActive(isActive);

  const opacity = isActive
    ? 1
    : hovered !== null
      ? isHovered
        ? 1
        : 0.3
      : 0.55;
  const x = isActive ? 8 : isHovered ? 6 : 0;

  return (
    <div className="relative">
      {isActive && (
        <motion.span
          layoutId="sb001-active-bar"
          className="pointer-events-none absolute z-10 left-[4px] top-1/2 h-[1.8px] -translate-y-1/2 rounded-full bg-accent-pro"
          animate={{ width: 23 }}
          transition={{ type: "spring", stiffness: 800, damping: 40 }}
        />
      )}

      <motion.span
        className="pointer-events-none absolute left-0 top-1/2 -translate-y-1/2 h-px bg-foreground/50"
        animate={{ width: isActive ? 0 : isHovered ? 26 : 18 }}
        transition={{ type: "spring", stiffness: 600, damping: 30 }}
      />
      <motion.span className="pointer-events-none absolute w-[13px] left-0 top-1/4 h-px bg-foreground/30" />
      <motion.span className="pointer-events-none absolute w-[16px] left-0 top-0 h-px bg-foreground/30" />
      <motion.span className="pointer-events-none absolute w-[13px] left-0 top-3/4 h-px bg-foreground/30" />

      <motion.div
        ref={itemRef}
        animate={{ opacity, x }}
        transition={{ type: "spring", stiffness: 700, damping: 30 }}
        style={{ transformOrigin: "left center" }}
      >
        <a
          href={href}
          onClick={onClick}
          onMouseEnter={() => {
            const el = itemRef.current;
            const container = containerRef.current;
            if (el && container) {
              const elRect = el.getBoundingClientRect();
              const containerRect = container.getBoundingClientRect();
              setHovered(href, {
                top: elRect.top - containerRect.top,
                height: elRect.height,
                left: 25,
              });
            } else {
              setHovered(href);
            }
          }}
          onMouseLeave={() => setHovered(null)}
          className={cn(
            "relative flex items-center gap-2 ml-2 pl-4 py-1.5 text-sm select-none",
            className,
          )}
        >
          <span className="relative z-1 truncate">{label}</span>
          {isNew && (
            <span className="size-1.5 rounded-full bg-accent-pro shrink-0" />
          )}
        </a>
      </motion.div>
    </div>
  );
});

// ─── Sidebar001Separator ──────────────────────────────────────────────────────

export function Sidebar001Separator({
  children,
  className,
}: {
  children?: React.ReactNode;
  className?: string;
}) {
  return (
    <div
      className={cn(
        "px-0 py-3.5 mt-2 text-sm font-medium text-foreground/40",
        className,
      )}
    >
      {children}
    </div>
  );
}

// ─── Sidebar001Group ──────────────────────────────────────────────────────────

export interface Sidebar001GroupProps {
  label: React.ReactNode;
  children: React.ReactNode;
  defaultOpen?: boolean;
  icon?: React.ReactNode;
  className?: string;
}

export function Sidebar001Group({
  label,
  children,
  defaultOpen = false,
  icon,
  className,
}: Sidebar001GroupProps) {
  const [isOpen, setIsOpen] = useState(false);
  const id = useId();
  const { setHovered, containerRef } = useContext(HoverContext);
  const buttonRef = useRef<HTMLButtonElement>(null);

  useEffect(() => {
    setIsOpen(defaultOpen);
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);

  const handleMouseEnter = useCallback(() => {
    const el = buttonRef.current;
    const container = containerRef.current;
    if (el && container) {
      const elRect = el.getBoundingClientRect();
      const containerRect = container.getBoundingClientRect();
      setHovered(id, {
        top: elRect.top - containerRect.top,
        height: elRect.height,
        left: 0,
      });
    } else {
      setHovered(id);
    }
  }, [id, setHovered, containerRef]);

  const handleMouseLeave = useCallback(() => {
    setHovered(null);
  }, [setHovered]);

  return (
    <div className={cn("flex flex-col", className)}>
      <button
        ref={buttonRef}
        type="button"
        onClick={() => setIsOpen((v) => !v)}
        onMouseEnter={handleMouseEnter}
        onMouseLeave={handleMouseLeave}
        className="relative z-1 flex items-center gap-1.5 py-1.5 pr-2 select-none text-left w-full group"
      >
        {icon ? (
          <>
            <span className="shrink-0 text-foreground/35 [&_svg]:size-3.5">
              {icon}
            </span>
            <span className="text-sm text-foreground/45 group-hover:text-foreground/70 transition-colors duration-150 flex-1">
              {label}
            </span>
            <MotionChevron
              size={14}
              strokeWidth={2.5}
              className="shrink-0 text-foreground/25 mr-1"
              animate={{ rotate: isOpen ? 90 : 0 }}
              transition={{ type: "spring", stiffness: 500, damping: 30 }}
            />
          </>
        ) : (
          <>
            <MotionChevron
              size={11}
              strokeWidth={2.5}
              className="shrink-0 text-foreground/35"
              animate={{ rotate: isOpen ? 90 : 0 }}
              transition={{ type: "spring", stiffness: 500, damping: 30 }}
            />
            <span className="text-sm text-foreground/45 group-hover:text-foreground/70 transition-colors duration-150">
              {label}
            </span>
          </>
        )}
      </button>

      <AnimatePresence initial={false}>
        {isOpen && (
          <motion.div
            initial={{ height: 0, opacity: 0 }}
            animate={{ height: "auto", opacity: 1 }}
            exit={{ height: 0, opacity: 0 }}
            transition={{ type: "spring", stiffness: 420, damping: 34 }}
            style={{ overflow: "hidden" }}
          >
            <div className="flex flex-col pl-3">{children}</div>
          </motion.div>
        )}
      </AnimatePresence>
    </div>
  );
}

// ─── Sidebar001Section ────────────────────────────────────────────────────────

export function Sidebar001Section({
  label,
  children,
  className,
}: {
  label?: React.ReactNode;
  children: React.ReactNode;
  className?: string;
}) {
  return (
    <div className={cn("flex flex-col", className)}>
      {label && <Sidebar001Separator>{label}</Sidebar001Separator>}
      {children}
    </div>
  );
}

// ─── Sidebar001Content ────────────────────────────────────────────────────────

export function Sidebar001Content({
  children,
  className,
}: {
  children: React.ReactNode;
  className?: string;
}) {
  const containerRef = useContext(HoverContext).containerRef;

  return (
    <div
      className={cn("flex-1 overflow-y-auto py-4 no-scrollbar", className)}
      data-scroll-viewport
    >
      <div ref={containerRef} className="relative px-1">
        <HoverHighlight />
        {children}
      </div>
    </div>
  );
}

// ─── Sidebar001 (with resize) ─────────────────────────────────────────────────

export interface Sidebar001Props {
  children: React.ReactNode;
  className?: string;
  defaultEffectsEnabled?: boolean;
  /** Initial width in px. Default: 240 */
  defaultWidth?: number;
  /** Min resize width in px. Default: 160 */
  minWidth?: number;
  /** Max resize width in px. Default: 400 */
  maxWidth?: number;
}

export function Sidebar001({
  children,
  className,
  defaultEffectsEnabled = true,
  defaultWidth = 240,
  minWidth = 160,
  maxWidth = 400,
}: Sidebar001Props) {
  const containerRef = useRef<HTMLDivElement>(null);
  const [width, setWidth] = useState(defaultWidth);
  const dragging = useRef(false);
  const startX = useRef(0);
  const startW = useRef(0);

  const onPointerDown = useCallback(
    (e: React.PointerEvent) => {
      e.preventDefault();
      dragging.current = true;
      startX.current = e.clientX;
      startW.current = width;
      (e.target as HTMLElement).setPointerCapture(e.pointerId);
    },
    [width],
  );

  const onPointerMove = useCallback(
    (e: React.PointerEvent) => {
      if (!dragging.current) return;
      const next = Math.min(
        maxWidth,
        Math.max(minWidth, startW.current + e.clientX - startX.current),
      );
      setWidth(next);
    },
    [minWidth, maxWidth],
  );

  const onPointerUp = useCallback(() => {
    dragging.current = false;
  }, []);

  return (
    <EffectsProvider defaultEnabled={defaultEffectsEnabled}>
      <HoverProvider containerRef={containerRef}>
        <aside
          className={cn(
            "relative flex flex-col h-full shrink-0 bg-background",
            className,
          )}
          style={{ width }}
        >
          {children}

          {/* Resize handle */}
          <div
            className="absolute top-0 right-0 h-full w-1 cursor-col-resize group/handle z-20"
            onPointerDown={onPointerDown}
            onPointerMove={onPointerMove}
            onPointerUp={onPointerUp}
          >
            <div className="absolute right-0 top-0 h-full w-px bg-border/50 group-hover/handle:bg-border transition-colors duration-150" />
          </div>
        </aside>
      </HoverProvider>
    </EffectsProvider>
  );
}

// ─── Sidebar001Header ─────────────────────────────────────────────────────────

export function Sidebar001Header({
  children,
  className,
}: {
  children?: React.ReactNode;
  className?: string;
}) {
  return (
    <div className={cn("shrink-0 px-3 pt-4 pb-2", className)}>{children}</div>
  );
}

// ─── Sidebar001Footer ─────────────────────────────────────────────────────────

export function Sidebar001Footer({
  children,
  className,
}: {
  children?: React.ReactNode;
  className?: string;
}) {
  return (
    <div
      className={cn(
        "shrink-0 px-3 pb-4 pt-2 border-t border-border/50",
        className,
      )}
    >
      {children}
    </div>
  );
}
```

---

### Sidebar 002 <a name="sidebar-002"></a>

Slide-in overlay sidebar with spring animation, animated dash items, and a cursor-following video preview on hover.

- **Tier**: Pro
- **Category**: Navigation

#### Installation

> [!NOTE]
> This is a **Pro Component**. The complete registry files and installation commands are available to active Unlumen UI subscribers.

#### How to Use

```tsx
import {
  Sidebar002,
  Sidebar002Provider,
  Sidebar002Section,
  Sidebar002Item,
  useSidebar002,
} from "@/components/unlumen-ui/sidebar-002";

function Trigger() {
  const { toggle } = useSidebar002();
  return (
    <button data-sidebar002-toggle onClick={toggle}>
      Menu
    </button>
  );
}

function Layout({ children }: { children: React.ReactNode }) {
  const { open, close } = useSidebar002();
  const pathname = usePathname();

  return (
    <>
      <Sidebar002 open={open} onClose={close}>
        <Sidebar002Section label="Components" count={4}>
          <Sidebar002Item
            href="/components/button"
            label="Button"
            isActive={pathname === "/components/button"}
            preview="/previews/button.mp4"
          />
          <Sidebar002Item
            href="/components/card"
            label="Card"
            isActive={pathname === "/components/card"}
          />
        </Sidebar002Section>
      </Sidebar002>

      <Trigger />
      {children}
    </>
  );
}

export default function App() {
  return (
    <Sidebar002Provider>
      <Layout>...</Layout>
    </Sidebar002Provider>
  );
}
```

#### Props

##### `Sidebar002Provider` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `children` | `React.ReactNode` | `-` | App tree — wraps the layout that contains the sidebar and its trigger. |
| `keyboardShortcut?` | `string \| false` | `&quot;s&quot;` | Key that toggles the sidebar. Pass false to disable. |
| `edgeTrigger?` | `boolean` | `true` | Open the sidebar when the cursor hovers the left edge. |
| `edgeDelay?` | `number` | `500` | Milliseconds the cursor must sit at the left edge before opening. |

##### `Sidebar002` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `open` | `boolean` | `-` | Whether the sidebar panel is visible. |
| `onClose` | `() =&gt; void` | `-` | Called when the sidebar should close (Escape, scroll, or outside click). |
| `children` | `React.ReactNode` | `-` | Sidebar002Section elements. |
| `className?` | `string` | `-` | Additional classes on the sidebar panel. |

##### `Sidebar002Section` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `label` | `React.ReactNode` | `-` | Section heading. |
| `count?` | `number` | `-` | Optional item count shown dimly after the label. |
| `children` | `React.ReactNode` | `-` | Sidebar002Item elements. |
| `className?` | `string` | `-` | Additional classes. |

##### `Sidebar002Item` Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `href` | `string` | `-` | Navigation target. |
| `label` | `React.ReactNode` | `-` | Text or node shown as the link label. |
| `isActive` | `boolean` | `-` | Marks this item as the current page. |
| `preview?` | `string` | `-` | URL to a video shown as a cursor-following card on hover. |
| `className?` | `string` | `-` | Additional classes on the anchor element. |
| `onClick?` | `React.MouseEventHandler&lt;HTMLAnchorElement&gt;` | `-` | Click handler for the anchor. |

---
