# GSAP v3.15.0 — Creative Developer Toolbox

> Compiled from the official `greensock/GSAP` repository type definitions and source code.
> GSAP is a robust JavaScript toolset that turns developers into animation superheroes. Build high-performance animations that work in every major browser.

---

## Table of Contents

- [Installation](#installation)
- [Core API — gsap](#core-api--gsap)
- [Tweens — gsap.to / from / fromTo / set](#tweens)
- [Timelines — gsap.timeline()](#timelines)
- [Animation Controls (Shared)](#animation-controls-shared)
- [Tween-Specific Methods](#tween-specific-methods)
- [Timeline-Specific Methods](#timeline-specific-methods)
- [Context & MatchMedia](#context--matchmedia)
- [Ticker](#ticker)
- [Config & Defaults](#config--defaults)
- [Performance Helpers — quickTo / quickSetter](#performance-helpers)
- [Registering Plugins, Effects & Eases](#registering-plugins-effects--eases)
- [CSS Properties (CSSPlugin)](#css-properties-cssplugin)
- [Easing](#easing)
- [Utility Functions — gsap.utils](#utility-functions--gsaputils)
- [ScrollTrigger Plugin](#scrolltrigger-plugin)
- [ScrollSmoother Plugin](#scrollsmoother-plugin)
- [Flip Plugin](#flip-plugin)
- [Observer Plugin](#observer-plugin)
- [Draggable](#draggable)
- [SplitText](#splittext)
- [MotionPathPlugin](#motionpathplugin)
- [TextPlugin](#textplugin)
- [DrawSVGPlugin](#drawsvgplugin)
- [Other Plugins](#other-plugins)
- [TypeScript Types Reference](#typescript-types-reference)
- [React Integration — @gsap/react](#react-integration--gsapreact)
- [Licensing](#licensing)

---

## Installation

### npm / yarn / pnpm

```bash
npm install gsap
yarn add gsap
pnpm add gsap
```

### ES Module Import

```js
import gsap from "gsap";

// Individual plugins
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { Flip } from "gsap/Flip";
import { Observer } from "gsap/Observer";
import { ScrollSmoother } from "gsap/ScrollSmoother";
import { Draggable } from "gsap/Draggable";
import { MotionPathPlugin } from "gsap/MotionPathPlugin";
import { TextPlugin } from "gsap/TextPlugin";
import { SplitText } from "gsap/SplitText";
import { DrawSVGPlugin } from "gsap/DrawSVGPlugin";
import { MorphSVGPlugin } from "gsap/MorphSVGPlugin";
import { ScrambleTextPlugin } from "gsap/ScrambleTextPlugin";
import { ScrollToPlugin } from "gsap/ScrollToPlugin";
import { EasePack } from "gsap/EasePack";
import { CustomEase } from "gsap/CustomEase";

// Register plugins
gsap.registerPlugin(ScrollTrigger, Flip, Draggable);
```

### CDN

```html
<script src="https://cdn.jsdelivr.net/npm/gsap@3.15.0/dist/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.15.0/dist/ScrollTrigger.min.js"></script>
```

### Package Exports

| Path | Format |
|---|---|
| `gsap` (default) | ESM: `esm/index.js`, CJS: `dist/gsap.js` |
| `gsap/ScrollTrigger` | ESM: `esm/ScrollTrigger.js` |
| `gsap/Flip` | ESM: `esm/Flip.js` |
| `gsap/all` | All exports bundled |
| `gsap/dist/*.js` | UMD/CJS bundles |
| `gsap/types/*` | TypeScript definitions |

---

## Core API — gsap

### Tweens

#### `gsap.to(targets, vars)` → `Tween`

Creates a tween animating TO the given values.

```js
gsap.to(".box", {
  x: 100,
  duration: 1,
  ease: "power2.out",
  stagger: 0.1,
  onComplete: () => console.log("done")
});
```

#### `gsap.from(targets, vars)` → `Tween`

Creates a tween animating FROM the given values to the element's current state.

```js
gsap.from(".box", {
  opacity: 0,
  y: 50,
  duration: 0.8,
  ease: "back.out(1.7)"
});
```

#### `gsap.fromTo(targets, fromVars, toVars)` → `Tween`

Creates a tween animating FROM the first set of values TO the second.

```js
gsap.fromTo(".box",
  { opacity: 0, scale: 0.5 },
  { opacity: 1, scale: 1, duration: 1, ease: "elastic.out(1, 0.5)" }
);
```

#### `gsap.set(targets, vars)` → `Tween`

Immediately sets properties (zero-duration tween).

```js
gsap.set(".box", { x: 100, y: 50, opacity: 0 });
```

#### TweenVars — Full Property Reference

| Property | Type | Description |
|---|---|---|
| `duration` | `number \| FunctionBased` | Duration in seconds |
| `delay` | `number \| FunctionBased` | Delay before start |
| `ease` | `EaseString \| EaseFunction` | Easing curve |
| `repeat` | `number` | Number of repeats (-1 = infinite) |
| `repeatDelay` | `number` | Delay between repeats |
| `yoyo` | `boolean` | Reverse on each repeat |
| `easeReverse` | `boolean \| EaseString \| EaseFunction` | Ease for reverse direction (replaces deprecated `yoyoEase`) |
| `stagger` | `number \| StaggerVars` | Stagger delay between targets |
| `overwrite` | `"auto" \| boolean` | Overwrite conflicting tweens |
| `immediateRender` | `boolean` | Render immediately on creation |
| `lazy` | `boolean` | Delay render until next tick |
| `paused` | `boolean` | Start paused |
| `reversed` | `boolean` | Start reversed |
| `inherit` | `boolean` | Inherit parent timeline defaults |
| `id` | `string \| number` | Unique identifier |
| `data` | `any` | Custom data attachment |
| `startAt` | `TweenVars` | Starting values |
| `endArray` | `any[]` | End values for array-based tweens |
| `keyframes` | `TweenVars[] \| object` | Keyframe animation |
| `runBackwards` | `boolean` | Internal: run animation in reverse |
| `repeatRefresh` | `boolean` | Recalculate values on each repeat |

#### Callback Properties

| Property | Type | Description |
|---|---|---|
| `onComplete` | `Callback` | Fires when animation completes |
| `onCompleteParams` | `any[]` | Params for onComplete |
| `onStart` | `Callback` | Fires when animation begins |
| `onStartParams` | `any[]` | Params for onStart |
| `onUpdate` | `Callback` | Fires on every frame |
| `onUpdateParams` | `any[]` | Params for onUpdate |
| `onRepeat` | `Callback` | Fires on each repeat |
| `onRepeatParams` | `any[]` | Params for onRepeat |
| `onReverseComplete` | `Callback` | Fires when reversed animation completes |
| `onReverseCompleteParams` | `any[]` | Params for onReverseComplete |
| `onInterrupt` | `Callback` | Fires when animation is interrupted |
| `onInterruptParams` | `any[]` | Params for onInterrupt |
| `callbackScope` | `object` | Scope for callbacks |

#### StaggerVars

```js
gsap.to(".boxes", {
  x: 100,
  stagger: {
    each: 0.1,        // Time between each start
    amount: 1,         // Total stagger time
    from: "center",    // "start" | "center" | "end" | "edges" | "random" | number | [x, y]
    grid: "auto",      // "auto" | [rows, cols]
    axis: "y",         // "x" | "y"
    ease: "power1.inOut",
    repeat: -1,
    yoyo: true,
    easeReverse: "power2.in"
  }
});
```

---

### Timelines

#### `gsap.timeline(vars?)` → `Timeline`

Creates a sequencing container for animations.

```js
const tl = gsap.timeline({
  defaults: { duration: 1, ease: "power2.out" },
  paused: true,
  repeat: -1,
  yoyo: true,
  repeatDelay: 0.5,
  smoothChildTiming: true,
  autoRemoveChildren: false,
  delay: 0.5,
  onComplete: () => console.log("all done")
});

tl.to(".box1", { x: 100 })
  .to(".box2", { y: 50 }, "-=0.5")    // overlap by 0.5s
  .to(".box3", { scale: 2 }, "<")      // at start of previous tween
  .to(".box4", { rotation: 360 }, ">") // at end of previous tween
  .to(".box5", { opacity: 0 }, "+=1"); // 1s after end
```

#### Position Parameter

| Syntax | Description |
|---|---|
| `1` | Absolute time (1 second) |
| `"+=1"` | 1 second after the end |
| `"-=0.5"` | 0.5 seconds before the end |
| `"<"` | Start of previous tween |
| `">"` | End of previous tween |
| `"<0.5"` | 0.5s after previous tween starts |
| `">-0.2"` | 0.2s before previous tween ends |
| `"myLabel"` | At the label position |
| `"myLabel+=1"` | 1s after the label |

#### TimelineVars

| Property | Type | Description |
|---|---|---|
| `defaults` | `TweenVars` | Default values for child tweens |
| `autoRemoveChildren` | `boolean` | Remove completed children |
| `smoothChildTiming` | `boolean` | Smooth repositioning |
| `delay` | `number` | Initial delay |
| *(inherits AnimationVars)* | | All animation + callback properties |

---

### Animation Controls (Shared)

These methods exist on both `Tween` and `Timeline` instances (inherited from `Animation`):

#### Playback

```js
anim.play();              // Play from current position
anim.play(1.5);           // Play from 1.5s
anim.pause();             // Pause at current position
anim.pause(2);            // Pause at 2s
anim.resume();            // Resume in current direction
anim.resume(1);           // Resume from 1s
anim.reverse();           // Play in reverse
anim.reverse(0.5);        // Reverse from 0.5s
anim.restart();           // Restart from beginning
anim.restart(true);       // Restart including delay
anim.seek(1);             // Jump to 1s
anim.seek("myLabel");     // Jump to label
```

#### Position & Progress

```js
// Getter/Setter pattern
anim.progress();          // Get (0-1)
anim.progress(0.5);       // Set to 50%
anim.time();              // Get current time
anim.time(2);             // Set to 2s
anim.totalProgress();     // Including repeats
anim.totalProgress(0.75); // Set total progress
anim.totalTime();         // Including repeats
anim.totalTime(5);        // Set total time
anim.duration();          // Get duration
anim.duration(3);         // Set duration
anim.totalDuration();     // Including repeats
anim.totalDuration(8);    // Set total duration
anim.delay();             // Get delay
anim.delay(1);            // Set delay
anim.startTime();         // Get start time in parent
anim.startTime(2);        // Set start time
anim.endTime();           // Get end time
anim.endTime(true);       // Include repeats
```

#### Repeat & Yoyo

```js
anim.repeat();            // Get repeat count
anim.repeat(3);           // Set to 3 repeats
anim.repeatDelay();       // Get repeat delay
anim.repeatDelay(0.5);    // Set repeat delay
anim.yoyo();              // Get yoyo state
anim.yoyo(true);          // Enable yoyo
anim.iteration();         // Get current iteration
anim.iteration(1);        // Set to iteration 1
```

#### State

```js
anim.isActive();          // Is actively animating?
anim.paused();            // Is paused?
anim.paused(true);        // Set paused
anim.reversed();          // Is reversed?
anim.reversed(true);      // Set reversed
anim.timeScale();         // Get time scale
anim.timeScale(2);        // Double speed
```

#### Lifecycle

```js
anim.kill();              // Kill and release for GC
anim.revert();            // Revert targets to pre-animation state
anim.invalidate();        // Force recalculation of values

// Event callbacks
anim.eventCallback("onComplete", myFunc);
anim.eventCallback("onComplete");  // Get callback

// Promise-based
anim.then(() => console.log("done"));
await anim;               // Awaitable
```

#### Properties

| Property | Type | Description |
|---|---|---|
| `data` | `any` | Custom data |
| `parent` | `Timeline \| null` | Parent timeline |
| `scrollTrigger` | `ScrollTrigger` | Associated ScrollTrigger |

---

### Tween-Specific Methods

```js
// Kill specific properties or targets
tween.kill();                     // Kill entire tween
tween.kill(myObject);             // Kill tween for specific target
tween.kill(null, "x,y");          // Kill specific properties
tween.kill(myObject, "opacity");  // Kill specific property of specific target

// Redirect an active tween's property
tween.resetTo("x", 200);
tween.resetTo("x", 200, 100, true); // value, start, startIsRelative

// Get targets
tween.targets();    // Returns target array
tween.ratio;        // Current eased progress ratio
```

---

### Timeline-Specific Methods

```js
// Adding children
tl.to(".box", { x: 100 }, "+=1");
tl.from(".box", { y: 50 }, "<");
tl.fromTo(".box", { scale: 0 }, { scale: 1 }, 1);
tl.set(".box", { opacity: 1 }, "myLabel");
tl.add(otherTimeline, 2);
tl.add("myLabel", 3);
tl.add([tween1, tween2, "label"], "<");

// Labels
tl.addLabel("intro", 0);
tl.removeLabel("intro");     // Returns time of removed label
tl.currentLabel();            // Get nearest label at or before now
tl.currentLabel("myLabel");   // Jump to label
tl.nextLabel();               // Next label from current time
tl.nextLabel(2);              // Next label after 2s
tl.previousLabel();           // Previous label
tl.previousLabel(2);          // Previous label before 2s

// Pauses
tl.addPause(2, myCallback, ["param1"]);
tl.removePause(2);

// Function calls
tl.call(myFunc, ["param"], "myLabel");

// Clearing
tl.clear();        // Remove all children
tl.clear(true);    // Including labels

// Removing children
tl.remove(myTween);
tl.remove([myTween, "myLabel"]);

// Querying
tl.getById("myTween");
tl.getChildren();                          // All children
tl.getChildren(true, true, true, 0.5);     // nested, tweens, timelines, ignoreBeforeTime
tl.getTweensOf(".myClass");
tl.getTweensOf(myElem, true);              // Only active ones
tl.recent();                               // Most recently added child

// Shifting
tl.shiftChildren(1);                  // Shift all by 1s
tl.shiftChildren(1, true);            // Including labels
tl.shiftChildren(1, true, 2);         // Ignore children before 2s

// Tweening playhead
tl.tweenTo("myLabel");                // Tween to label
tl.tweenTo(5, { ease: "power2.in" }); // Tween to 5s with options
tl.tweenFromTo("label1", "label2");   // Tween between labels
```

---

### Context & MatchMedia

#### `gsap.context(func?, scope?)` → `Context`

Records animations for easy cleanup. Essential for React, Vue, etc.

```js
const ctx = gsap.context((self) => {
  gsap.to(".box", { x: 100 });
  gsap.to(".circle", { rotation: 360 });

  // Named functions
  self.add("animate", () => {
    gsap.to(".box", { y: 50 });
  });
}, containerRef);  // Scope: selectors resolve within this element

// Call named functions later
ctx.animate();

// Cleanup — reverts all animations created within
ctx.revert();

// Or kill without reverting
ctx.kill();
ctx.kill(true);  // Revert before killing

// Ignore context tracking temporarily
ctx.ignore(() => {
  gsap.to(".permanent", { opacity: 1 }); // Not tracked
});

// Clear all recorded animations without killing context
ctx.clear();
```

#### Context Properties

| Property | Type | Description |
|---|---|---|
| `isReverted` | `boolean` | Whether context has been reverted |
| `conditions` | `Conditions` | MatchMedia conditions |
| `queries` | `object` | Raw media query results |
| `selector` | `Function` | Scoped selector function |

#### `gsap.matchMedia(scope?)` → `MatchMedia`

Responsive animations with automatic cleanup on breakpoint changes.

```js
const mm = gsap.matchMedia();

mm.add("(min-width: 800px)", (context) => {
  // Desktop animations — auto-reverted when breakpoint exits
  gsap.to(".box", { x: 200 });
  return () => { /* optional cleanup */ };
});

mm.add("(max-width: 799px)", (context) => {
  // Mobile animations
  gsap.to(".box", { x: 50 });
});

// Named conditions
mm.add({
  isDesktop: "(min-width: 800px)",
  isMobile: "(max-width: 799px)",
  reduceMotion: "(prefers-reduced-motion: reduce)"
}, (context) => {
  const { isDesktop, isMobile, reduceMotion } = context.conditions;
  if (isDesktop) gsap.to(".box", { x: 200 });
  if (reduceMotion) gsap.set(".box", { x: 200 }); // No animation
});

// Cleanup
mm.revert();
mm.kill();

// Force refresh
gsap.matchMediaRefresh();
```

---

### Ticker

The global heartbeat that drives all animations.

```js
// Add a callback
gsap.ticker.add((time, deltaTime, frame, elapsed) => {
  // Fires on every tick (~60fps)
});

// Run once
gsap.ticker.add(callback, true);

// Prioritize (run before others)
gsap.ticker.add(callback, false, true);

// Remove
gsap.ticker.remove(callback);

// Control FPS
gsap.ticker.fps(30);  // Cap at 30fps

// Lag smoothing
gsap.ticker.lagSmoothing(500, 33);  // threshold, adjustedLag
gsap.ticker.lagSmoothing(false);    // Disable

// Manual control
gsap.ticker.sleep();
gsap.ticker.wake();
gsap.ticker.tick();

// Properties
gsap.ticker.time;       // Current time
gsap.ticker.frame;      // Current frame
gsap.ticker.deltaRatio(); // Delta ratio (1 at 60fps)
gsap.ticker.deltaRatio(30); // Relative to specific fps
```

---

### Config & Defaults

```js
// Global config
gsap.config({
  autoSleep: 120,           // Seconds of inactivity before sleep
  force3D: "auto",          // "auto" | true | false — use 3D transforms
  nullTargetWarn: true,     // Warn if target is null
  resistance: 100,          // For physics-based tweens
  units: {                  // Default units
    x: "px",
    y: "px",
    rotation: "deg",
    left: "%"
  }
});

// Global tween defaults
gsap.defaults({
  ease: "power2.out",
  duration: 0.5
});

// Global values
gsap.version;          // "3.15.0"
gsap.globalTimeline;   // The root timeline
gsap.effects;          // Registered effects map
```

---

### Performance Helpers

#### `gsap.quickSetter(targets, property, unit?)` → `Function`

Ultra-fast property setter, bypassing the tween overhead.

```js
const setX = gsap.quickSetter("#cursor", "x", "px");
const setY = gsap.quickSetter("#cursor", "y", "px");
const setRotation = gsap.quickSetter(".arrow", "rotation", "deg");

window.addEventListener("mousemove", (e) => {
  setX(e.clientX);
  setY(e.clientY);
  setRotation(e.clientX * 0.1);
});
```

#### `gsap.quickTo(target, property, vars?)` → `QuickToFunc`

Reusable function that performantly animates to new values.

```js
const xTo = gsap.quickTo("#cursor", "x", { duration: 0.8, ease: "power3" });
const yTo = gsap.quickTo("#cursor", "y", { duration: 0.8, ease: "power3" });

window.addEventListener("mousemove", (e) => {
  xTo(e.clientX);
  yTo(e.clientY);
});

// Access underlying tween
xTo.tween;
```

---

### Registering Plugins, Effects & Eases

```js
// Register plugins
gsap.registerPlugin(ScrollTrigger, Flip, Draggable);

// Register reusable effects
gsap.registerEffect({
  name: "fade",
  effect: (targets, config) => {
    return gsap.to(targets, {
      duration: config.duration,
      opacity: 0,
      y: config.y || 20,
      ease: config.ease
    });
  },
  defaults: { duration: 1, y: 20, ease: "power2.out" },
  extendTimeline: true  // Enables tl.fade(".box")
});

// Use registered effects
gsap.effects.fade(".box");
gsap.effects.fade(".box", { duration: 2 });

// On timelines (when extendTimeline: true)
tl.fade(".box", { duration: 0.5 }, "+=0.3");

// Register custom eases
gsap.registerEase("myEase", (progress) => {
  return progress * progress; // Quadratic
});

// Use it
gsap.to(".box", { x: 100, ease: "myEase" });
```

---

### Other Core Methods

```js
// Delayed function call
const delayed = gsap.delayedCall(1, myFunc, ["param"]);
delayed.kill(); // Cancel

// Export root timeline
const exportedTL = gsap.exportRoot();
const exportedTL = gsap.exportRoot(vars, includeDelayedCalls);

// Get property
gsap.getProperty(element, "x");         // Returns value
gsap.getProperty(element, "x", "px");   // With unit
const getter = gsap.getProperty(element);
getter("x", "px");

// Get tweens of target
gsap.getTweensOf(".box");
gsap.getTweensOf(".box", true); // Only active

// Get by ID
gsap.getById("myTween");

// Check if animating
gsap.isTweening("#box");

// Kill tweens of target
gsap.killTweensOf(".myClass");
gsap.killTweensOf(myObject, "opacity,x");
gsap.killTweensOf(".myClass", null, true); // Only active

// Parse ease
const easeFn = gsap.parseEase("power2.inOut");
const allEases = gsap.parseEase();  // Get ease map

// Manual root update
gsap.ticker.remove(gsap.updateRoot);
gsap.updateRoot(20); // Manually set root time to 20s
```

---

## CSS Properties (CSSPlugin)

CSSPlugin is automatically included with the core. GSAP provides shorthand transforms that are significantly more performant than CSS transforms.

### GSAP Transform Shorthands

| Property | Type | Description |
|---|---|---|
| `x` | `TweenValue` | Horizontal translation (translateX) |
| `y` | `TweenValue` | Vertical translation (translateY) |
| `z` | `TweenValue` | Z-axis translation (translateZ) |
| `xPercent` | `TweenValue` | Horizontal translation in % |
| `yPercent` | `TweenValue` | Vertical translation in % |
| `rotation` / `rotate` | `TweenValue` | 2D rotation (degrees) |
| `rotationX` / `rotateX` | `TweenValue` | 3D X-axis rotation |
| `rotationY` / `rotateY` | `TweenValue` | 3D Y-axis rotation |
| `rotationZ` / `rotateZ` | `TweenValue` | 3D Z-axis rotation |
| `scale` | `TweenValue` | Uniform scale |
| `scaleX` | `TweenValue` | Horizontal scale |
| `scaleY` | `TweenValue` | Vertical scale |
| `skewX` | `TweenValue` | Horizontal skew |
| `skewY` | `TweenValue` | Vertical skew |
| `transformOrigin` | `TweenValue` | Transform origin |
| `autoAlpha` | `TweenValue` | opacity + visibility:hidden at 0 |
| `alpha` | `TweenValue` | Alias for opacity |
| `svgOrigin` | `TweenValue` | SVG-specific origin |
| `smoothOrigin` | `BooleanValue` | Smooth origin transitions |
| `translateX` / `translateY` / `translateZ` | `TweenValue` | CSS translate aliases |

### Common CSS Properties

All standard CSS properties can be animated directly:

```js
gsap.to(".box", {
  // GSAP shorthands
  x: 100,
  y: 50,
  rotation: 45,
  scale: 1.5,
  autoAlpha: 0.8,

  // Standard CSS (camelCase)
  backgroundColor: "#ff0000",
  borderRadius: "50%",
  boxShadow: "0 10px 30px rgba(0,0,0,0.3)",
  fontSize: "24px",
  width: "200px",
  height: "200px",
  opacity: 0.5,
  filter: "blur(10px)",
  clipPath: "circle(50%)",
  perspective: 500,

  // Relative values
  x: "+=100",     // Add 100 to current
  rotation: "-=45", // Subtract 45

  // Random values
  x: "random(-100, 100)",
  y: "random(-50, 50, 10)", // Snapped to 10

  // Function-based values
  x: (i, target, targets) => i * 50,
  rotation: (i) => gsap.utils.random(0, 360),

  duration: 1,
  ease: "power2.out"
});
```

---

## Easing

### Built-in Eases

| Ease | Variants | Description |
|---|---|---|
| `"none"` | — | Linear, no easing |
| `"power1"` | `.in` `.out` `.inOut` | Subtle easing (equivalent to Quad) |
| `"power2"` | `.in` `.out` `.inOut` | Moderate easing (equivalent to Cubic) |
| `"power3"` | `.in` `.out` `.inOut` | Strong easing (equivalent to Quart) |
| `"power4"` | `.in` `.out` `.inOut` | Extra strong (equivalent to Quint/Strong) |
| `"back"` | `.in` `.out` `.inOut` | Overshoots target |
| `"bounce"` | `.in` `.out` `.inOut` | Bouncing effect |
| `"circ"` | `.in` `.out` `.inOut` | Circular motion |
| `"elastic"` | `.in` `.out` `.inOut` | Spring-like bounce |
| `"expo"` | `.in` `.out` `.inOut` | Exponential curve |
| `"sine"` | `.in` `.out` `.inOut` | Sinusoidal curve |

Default direction is `.out` when none is specified (e.g., `"power2"` = `"power2.out"`).

### Configurable Eases

```js
// Back — overshoot amount
ease: "back.out(1.7)"      // Default overshoot is 1.7
Back.easeOut.config(3)      // More overshoot

// Elastic — amplitude & period
ease: "elastic.out(1, 0.3)"  // amplitude, period
Elastic.easeOut.config(2, 0.5)

// Stepped — discrete steps
SteppedEase.config(5)       // 5 steps
```

### EasePack Extras (requires EasePack import)

```js
import { EasePack, ExpoScaleEase, RoughEase, SlowMo } from "gsap/EasePack";
gsap.registerPlugin(EasePack);

// ExpoScaleEase — matches zoom animations
ExpoScaleEase.config(1, 5, "power2.inOut")

// RoughEase — organic/rough motion
RoughEase.config({
  strength: 1,
  points: 20,
  taper: "none",     // "in" | "out" | "both" | "none"
  randomize: true,
  clamp: false,
  template: "power1.out"
})

// SlowMo — slow middle section
SlowMo.config(0.7, 0.7, false)  // linearRatio, power, yoyoMode
```

### CustomEase (requires separate import)

```js
import { CustomEase } from "gsap/CustomEase";
gsap.registerPlugin(CustomEase);

CustomEase.create("myCustomEase",
  "M0,0 C0.126,0.382 0.282,0.674 0.44,0.822 0.632,1.002 0.818,1.001 1,1"
);

gsap.to(".box", { x: 100, ease: "myCustomEase" });
```

### CustomBounce & CustomWiggle

```js
import { CustomBounce } from "gsap/CustomBounce";
import { CustomWiggle } from "gsap/CustomWiggle";

CustomBounce.create("myBounce", {
  strength: 0.6,
  squash: 3,
  squashID: "myBounce-squash"
});

CustomWiggle.create("myWiggle", {
  wiggles: 8,
  type: "uniform"   // "uniform" | "random" | "easeOut" | etc.
});
```

---

## Utility Functions — gsap.utils

### `clamp(min, max, value?)` → `number | Function`

```js
gsap.utils.clamp(0, 100, 105);    // 100
gsap.utils.clamp(0, 100, -5);     // 0

const clamper = gsap.utils.clamp(0, 100);
clamper(150);  // 100
```

### `distribute(config)` → `FunctionBasedValue`

```js
gsap.to(".boxes", {
  y: gsap.utils.distribute({
    base: 0,
    amount: 200,
    from: "center",
    grid: "auto",
    axis: "y",
    ease: "power1.inOut"
  })
});
```

### `getUnit(value)` → `string`

```js
gsap.utils.getUnit("50%");    // "%"
gsap.utils.getUnit("100px");  // "px"
```

### `interpolate(start, end, progress?)` → `T | Function`

```js
gsap.utils.interpolate(0, 500, 0.5);                  // 250
gsap.utils.interpolate("red", "blue", 0.5);            // Blended color
gsap.utils.interpolate({ x: 0 }, { x: 100 }, 0.5);    // { x: 50 }
gsap.utils.interpolate([100, 50, 500], 0.5);           // 50

const lerp = gsap.utils.interpolate(0, 100);
lerp(0.75);  // 75
```

### `mapRange(inMin, inMax, outMin, outMax, value?)` → `number | Function`

```js
gsap.utils.mapRange(-10, 10, 100, 200, 0);  // 150

const mapper = gsap.utils.mapRange(0, 100, 0, 250);
mapper(50);  // 125
```

### `normalize(inMin, inMax, value?)` → `number | Function`

```js
gsap.utils.normalize(0, 100, 50);  // 0.5

const norm = gsap.utils.normalize(-10, 10);
norm(0);  // 0.5
```

### `pipe(...fns)` → `Function`

```js
const transform = gsap.utils.pipe(
  gsap.utils.clamp(0, 100),
  gsap.utils.normalize(0, 100),
  gsap.utils.mapRange(0, 1, -200, 200)
);
transform(150);  // 200
```

### `random(min, max, snap?, returnFunction?)` → `number | Function`

```js
gsap.utils.random(-100, 100);           // Random number
gsap.utils.random(0, 500, 5);           // Snapped to 5
gsap.utils.random(["red", "green"]);     // Random element

const rnd = gsap.utils.random(-200, 500, 10, true);
rnd();  // Random, snapped to 10
```

### `selector(scope)` → `SelectorFunc`

```js
const q = gsap.utils.selector("#myContainer");
gsap.to(q(".box"), { x: 100 });
```

### `shuffle(array)` → `T[]`

```js
gsap.utils.shuffle([1, 2, 3, 4, 5]);  // In-place shuffle
```

### `snap(config, value?)` → `number | Function`

```js
gsap.utils.snap(10, 23.5);            // 20
gsap.utils.snap([100, 50, 500], 65);   // 50
gsap.utils.snap({ values: [0, 100, 300], radius: 20 }, 30.5); // 30.5 (outside radius)
gsap.utils.snap({ increment: 500, radius: 150 }, 310);         // 310 (outside radius)

const snap5 = gsap.utils.snap(5);
snap5(12);  // 10

// Point2D snapping
gsap.utils.snap(
  { values: [{x:0, y:0}, {x:10, y:10}], radius: 5 },
  { x: 8, y: 8 }
);  // {x: 10, y: 10}
```

### `splitColor(color, hsl?)` → `number[]`

```js
gsap.utils.splitColor("red");                      // [255, 0, 0]
gsap.utils.splitColor("rgba(204, 153, 51, 0.5)");  // [204, 153, 51, 0.5]
gsap.utils.splitColor("#6fb936", true);             // [94, 55, 47] (HSL)
```

### `toArray(value, scope?, leaveStrings?)` → `T[]`

```js
gsap.utils.toArray(".myClass");        // Array of elements
gsap.utils.toArray(myElement);         // [myElement]
gsap.utils.toArray($(".class"));       // jQuery → Array
gsap.utils.toArray([".a", ".b"]);      // Flatten selectors
gsap.utils.toArray(nodeList);          // NodeList → Array
```

### `unitize(fn, unit?)` → `Function`

```js
const clampPx = gsap.utils.unitize(gsap.utils.clamp(0, 100), "px");
clampPx(132);  // "100px"

gsap.to(".box", {
  x: 1000,
  modifiers: {
    x: gsap.utils.unitize(gsap.utils.wrap(0, window.innerWidth), "px")
  }
});
```

### `wrap(value1, value2, index?)` → `T | Function`

```js
gsap.utils.wrap(0, 100, 150);         // 50 (wraps around)
gsap.utils.wrap(["a", "b", "c"], 5);   // "c"

const wrapper = gsap.utils.wrap(["red", "green", "blue"]);
wrapper(4);  // "green"
```

### `wrapYoyo(value1, value2, index?)` → `T | Function`

Like `wrap()` but bounces back instead of jumping to start.

```js
gsap.utils.wrapYoyo(0, 100, 150);  // 50 (bounces back)
```

### `checkPrefix(property)` → `string`

```js
gsap.utils.checkPrefix("filter");  // "filter", "WebkitFilter", or "MozFilter"
```

---

## ScrollTrigger Plugin

### Import & Register

```js
import { ScrollTrigger } from "gsap/ScrollTrigger";
gsap.registerPlugin(ScrollTrigger);
```

### Basic Usage

```js
// Attach to a tween
gsap.to(".box", {
  x: 500,
  scrollTrigger: {
    trigger: ".box",
    start: "top center",
    end: "bottom center",
    scrub: true,
    markers: true
  }
});

// Standalone
ScrollTrigger.create({
  trigger: ".section",
  start: "top top",
  end: "+=500",
  pin: true,
  pinSpacing: true,
  onEnter: (self) => console.log("entered"),
  onLeave: (self) => console.log("left"),
  onEnterBack: (self) => console.log("entered back"),
  onLeaveBack: (self) => console.log("left back")
});
```

### ScrollTrigger.Vars — Full Property Reference

| Property | Type | Description |
|---|---|---|
| `trigger` | `DOMTarget` | Element that triggers the animation |
| `start` | `string \| number \| Function` | When to start (e.g., `"top center"`, `"top 80%"`) |
| `end` | `string \| number \| Function` | When to end |
| `endTrigger` | `DOMTarget` | Separate element for end position |
| `scrub` | `boolean \| number` | Link animation to scroll (number = smoothing seconds) |
| `pin` | `boolean \| DOMTarget` | Pin the trigger element |
| `pinSpacing` | `boolean \| string` | Add spacing for pinned element |
| `pinSpacer` | `DOMTarget` | Custom spacer element |
| `pinReparent` | `boolean` | Reparent pin for z-index issues |
| `pinType` | `"fixed" \| "transform"` | Pin implementation method |
| `pinnedContainer` | `DOMTarget` | Container for pinned element |
| `anticipatePin` | `number` | Smoothen pin start |
| `snap` | `number \| number[] \| "labels" \| "labelsDirectional" \| SnapFunc \| SnapVars` | Snap to positions |
| `markers` | `boolean \| MarkersVars` | Show debug markers |
| `toggleActions` | `string` | Four actions: `"onEnter onLeave onEnterBack onLeaveBack"` |
| `toggleClass` | `string \| ToggleClassVars` | Toggle a CSS class |
| `scroller` | `DOMTarget \| Window` | Custom scroll container |
| `horizontal` | `boolean` | Horizontal scrolling |
| `containerAnimation` | `Animation` | For horizontal scroll containers |
| `once` | `boolean` | Only trigger once |
| `invalidateOnRefresh` | `boolean` | Invalidate on refresh |
| `fastScrollEnd` | `boolean \| number` | Handle fast scroll at end |
| `preventOverlaps` | `boolean \| string \| Callback` | Prevent overlapping triggers |
| `refreshPriority` | `number` | Refresh order priority |
| `id` | `string` | Unique identifier |
| `immediateRender` | `boolean` | Render immediately |

#### toggleActions Values

Each action: `"play"`, `"pause"`, `"resume"`, `"reverse"`, `"restart"`, `"reset"`, `"complete"`, `"none"`

```js
toggleActions: "play pause resume reverse"
//              onEnter onLeave onEnterBack onLeaveBack
```

#### SnapVars

```js
snap: {
  snapTo: 0.25,          // Snap to quarter increments
  // or: [0, 0.25, 0.5, 0.75, 1]
  // or: "labels" | "labelsDirectional"
  // or: (value, self) => Math.round(value * 4) / 4
  duration: { min: 0.2, max: 0.6 },
  delay: 0,
  ease: "power1.inOut",
  inertia: true,
  directional: true,
  onStart: (self) => {},
  onInterrupt: (self) => {},
  onComplete: (self) => {}
}
```

#### Callbacks

| Callback | Signature | Description |
|---|---|---|
| `onEnter` | `(self) => any` | Scrolling forward past start |
| `onLeave` | `(self) => any` | Scrolling forward past end |
| `onEnterBack` | `(self) => any` | Scrolling backward past end |
| `onLeaveBack` | `(self) => any` | Scrolling backward past start |
| `onUpdate` | `(self) => any` | Every frame while active |
| `onToggle` | `(self) => any` | When active state changes |
| `onRefresh` | `(self) => any` | On refresh |
| `onRefreshInit` | `(self) => any` | Before refresh |
| `onSnapComplete` | `(self) => any` | When snap completes |
| `onScrubComplete` | `(self) => any` | When scrub tween completes |
| `onKill` | `(self) => any` | When killed |

### ScrollTrigger Instance Properties

| Property | Type | Description |
|---|---|---|
| `animation` | `Animation` | Associated animation |
| `direction` | `number` | 1 (forward) or -1 (backward) |
| `end` | `number` | End scroll position (px) |
| `start` | `number` | Start scroll position (px) |
| `progress` | `number` | Current progress (0-1) |
| `isActive` | `boolean` | Currently between start/end |
| `pin` | `Element` | Pinned element |
| `scroller` | `Element \| Window` | Scroll container |
| `trigger` | `Element` | Trigger element |
| `vars` | `Vars` | Configuration object |

### ScrollTrigger Instance Methods

```js
const st = ScrollTrigger.create({ ... });

st.disable();                  // Disable
st.disable(true);              // Disable and revert
st.disable(true, true);       // Revert, allow animation to finish
st.enable();                   // Re-enable
st.enable(true, true);        // Reset, refresh
st.kill();                     // Permanently remove
st.kill(true);                 // Reset before killing
st.refresh();                  // Recalculate start/end
st.scroll();                   // Get scroll position
st.scroll(500);                // Set scroll position
st.tweenTo(500);               // Animate scroll position
st.endAnimation();             // Force to natural end
st.getVelocity();              // Scroll velocity (px/s)
st.getTween();                 // Get scrub tween
st.getTween(true);             // Get snap tween
st.getTrailing();              // Preceding triggers
st.getTrailing("name");        // Filter by preventOverlaps name
st.labelToScroll("myLabel");   // Label → scroll position
st.next();                     // Next trigger in order
st.previous();                 // Previous trigger
st.update();                   // Force update
```

### ScrollTrigger Static Methods

```js
ScrollTrigger.create(vars);
ScrollTrigger.batch(".items", {
  interval: 0.1,
  batchMax: 3,
  onEnter: (elements, triggers) => gsap.to(elements, { opacity: 1, stagger: 0.15 }),
  onLeave: (elements, triggers) => gsap.set(elements, { opacity: 0 })
});
ScrollTrigger.defaults({ toggleActions: "play none none reverse" });
ScrollTrigger.config({
  limitCallbacks: true,
  syncInterval: 40,
  autoRefreshEvents: "resize,load,visibilitychange,DOMContentLoaded",
  ignoreMobileResize: true
});
ScrollTrigger.refresh();
ScrollTrigger.refresh(true);           // Safe mode
ScrollTrigger.update();
ScrollTrigger.getAll();
ScrollTrigger.getById("myId");
ScrollTrigger.killAll();
ScrollTrigger.killAll(true);           // Allow listeners
ScrollTrigger.disable();
ScrollTrigger.disable(true, true);     // Reset, kill
ScrollTrigger.enable();
ScrollTrigger.addEventListener("scrollStart", fn);
ScrollTrigger.addEventListener("scrollEnd", fn);
ScrollTrigger.addEventListener("refresh", fn);
ScrollTrigger.addEventListener("refreshInit", fn);
ScrollTrigger.addEventListener("matchMedia", fn);
ScrollTrigger.addEventListener("revert", fn);
ScrollTrigger.removeEventListener("scrollEnd", fn);
ScrollTrigger.isInViewport(element, 0.2);          // 20% visible
ScrollTrigger.isInViewport(element, 0, true);      // Horizontal
ScrollTrigger.isScrolling();
ScrollTrigger.maxScroll(window);
ScrollTrigger.maxScroll(element, true);             // Horizontal
ScrollTrigger.positionInViewport(element, "center"); // 0-1
ScrollTrigger.normalizeScroll(true);
ScrollTrigger.normalizeScroll({ ... });
ScrollTrigger.observe({ ...observerVars });
ScrollTrigger.saveStyles(".panel, #logo");
ScrollTrigger.clearScrollMemory();
ScrollTrigger.clearMatchMedia();
ScrollTrigger.scrollerProxy(".container", { ... });
ScrollTrigger.snapDirectional(5);
ScrollTrigger.sort();
ScrollTrigger.sort(customSortFunc);
ScrollTrigger.getScrollFunc(window);
ScrollTrigger.getScrollFunc(element, true); // Horizontal
```

---

## ScrollSmoother Plugin

> Requires Club GSAP membership.

### Import & Register

```js
import { ScrollSmoother } from "gsap/ScrollSmoother";
gsap.registerPlugin(ScrollTrigger, ScrollSmoother);
```

### Usage

```js
const smoother = ScrollSmoother.create({
  wrapper: "#smooth-wrapper",
  content: "#smooth-content",
  smooth: 1.5,           // Seconds to catch up
  smoothTouch: 0.1,      // Touch smoothing
  effects: true,          // Enable data-speed/data-lag
  effectsPrefix: "data-", // Prefix for attributes
  effectsPadding: 50,     // Extra padding for effects
  normalizeScroll: true,  // Normalize on mobile
  ignoreMobileResize: true,
  ease: "expo",
  speed: 1,
  autoResize: true,
  onUpdate: (self) => {},
  onStop: (self) => {},
  onFocusIn: (self, event) => {}
});
```

### HTML Structure

```html
<div id="smooth-wrapper">
  <div id="smooth-content">
    <!-- Content with parallax -->
    <img data-speed="0.5" src="..." />       <!-- Slower than scroll -->
    <div data-speed="1.5">Fast element</div>  <!-- Faster than scroll -->
    <div data-lag="0.5">Lagging element</div> <!-- Delayed response -->
  </div>
</div>
```

### Instance Methods

```js
smoother.scrollTo("#target", true, "center center");  // target, smooth, position
smoother.scrollTop(500);           // Jump to position
smoother.scrollTop();              // Get position
smoother.content();                // Get content element
smoother.content("#new-content");  // Set content element
smoother.wrapper();                // Get wrapper element
smoother.wrapper("#new-wrapper");  // Set wrapper element
smoother.smooth();                 // Get smoothing value
smoother.smooth(2);                // Set smoothing
smoother.paused();                 // Get paused state
smoother.paused(true);             // Pause scrolling
smoother.offset("#target", "center center"); // Get scroll offset
smoother.getVelocity();            // Scroll velocity (px/s)
smoother.effects(".box", { speed: 0.5, lag: 0.3 });  // Apply programmatic effects
smoother.effects();                // Get effect ScrollTriggers
smoother.refresh();
smoother.refresh(true, true);      // Soft, force
smoother.kill();
```

### Static Methods

```js
ScrollSmoother.create(vars);
ScrollSmoother.get();       // Get current instance
ScrollSmoother.refresh();   // Same as ScrollTrigger.refresh()
```

---

## Flip Plugin

### Import & Register

```js
import { Flip } from "gsap/Flip";
gsap.registerPlugin(Flip);
```

### Basic FLIP Workflow

```js
// 1. Capture the current state
const state = Flip.getState(".boxes, .container", {
  props: "backgroundColor,color,borderRadius",
  simple: true  // Skip measuring transforms if unnecessary
});

// 2. Make your DOM changes
container.classList.toggle("reorder");
newParent.appendChild(box);

// 3. Animate from old → new
Flip.from(state, {
  duration: 0.8,
  ease: "power2.inOut",
  stagger: 0.05,
  absolute: true,     // Use absolute positioning during animation
  scale: true,         // Animate scale instead of width/height
  fade: true,          // Fade entering/leaving elements
  nested: true,        // Handle nested FLIP elements
  prune: true,         // Remove targets that didn't change
  spin: 1,             // Spin during flip (number or boolean)
  zIndex: 1000,        // Z-index during animation
  targets: ".specific", // Only animate specific targets
  toggleClass: "flipping",
  absoluteOnLeave: true,
  onEnter: (elements) => gsap.fromTo(elements, { opacity: 0 }, { opacity: 1 }),
  onLeave: (elements) => gsap.to(elements, { opacity: 0 }),
  onComplete: () => console.log("flip complete")
});
```

### Static Methods

```js
// Get state
const state = Flip.getState(targets, { props: "color", simple: true });
const state = Flip.getState(targets, "color"); // Shorthand for props

// Animate
Flip.from(state, vars);   // From old state → current
Flip.to(state, vars);     // From current → captured state

// Fit one element to another
Flip.fit("#el1", "#el2", {
  scale: true,
  absolute: true,
  duration: 1,
  ease: "power2"
});
Flip.fit(element, flipState);

// Coordinate system conversion
Flip.convertCoordinates(fromElement, toElement);        // Returns Matrix2D
Flip.convertCoordinates(fromElement, toElement, point); // Returns Point2D

// Query state
Flip.isFlipping("#box");
Flip.getByTarget("#box");  // Get active flip timeline

// Cleanup
Flip.killFlipsOf(".box");
Flip.killFlipsOf(".box", false); // Don't complete

// Positioning
Flip.makeAbsolute(".boxes"); // Set absolute position preserving layout
```

### FlipState Methods

```js
const state = Flip.getState(targets);
state.add(otherState);                    // Merge states
state.clear();                            // Clear state data
state.compare(otherState);                // Returns { changed, unchanged, enter, leave }
state.update();                           // Update to current positions
state.update(true);                       // Soft update
state.fit(otherState, scale, nested);     // Fit to another state
state.recordInlineStyles();
state.interrupt();
state.getProperty(element, "x");
state.getElementState(element);
state.makeAbsolute();
```

### FlipBatch

```js
const batch = Flip.batch("myBatch");

batch.add({
  getState: (self) => Flip.getState(targets),
  loadState: (done) => { /* async load */ done(); },
  setState: (self) => container.classList.toggle("active"),
  animate: (self) => Flip.from(self.state, { ease: "power1.inOut" }),
  onEnter: (elements) => console.log("entering", elements),
  onLeave: (elements) => console.log("leaving", elements),
  onStart: (self) => {},
  onComplete: (self) => {},
  once: true
});

batch.run();                // Execute all steps
batch.run(true);            // Skip getState
batch.getState();           // Run getState on all actions
batch.getState(true);       // Merge results into batch.state
batch.getStateById("box1"); // Find state by data-flip-id
batch.clear();              // Flush state
batch.clear(true);          // State only, keep actions
batch.remove(action);
batch.kill();
```

---

## Observer Plugin

### Import & Register

```js
import { Observer } from "gsap/Observer";
gsap.registerPlugin(Observer);
```

### Usage

```js
const observer = Observer.create({
  target: window,           // DOMTarget | Window | Document
  type: "wheel,touch,pointer", // Event types to listen for
  tolerance: 10,            // Minimum movement threshold
  dragMinimum: 3,           // Minimum drag distance
  lockAxis: true,           // Lock to dominant axis
  wheelSpeed: -1,           // Invert wheel direction
  preventDefault: true,
  capture: false,
  debounce: true,
  allowClicks: true,
  lineHeight: 16,

  // Direction callbacks
  onUp: (self) => {},
  onDown: (self) => {},
  onLeft: (self) => {},
  onRight: (self) => {},

  // Interaction callbacks
  onChange: (self) => {},
  onChangeX: (self) => {},
  onChangeY: (self) => {},
  onToggleX: (self) => {},
  onToggleY: (self) => {},
  onClick: (self) => {},
  onPress: (self) => {},
  onRelease: (self) => {},
  onMove: (self) => {},
  onHover: (self) => {},
  onHoverEnd: (self) => {},
  onWheel: (self) => {},
  onStop: (self) => {},
  onStopDelay: 0.25,

  // Drag callbacks
  onDrag: (self) => {},
  onDragStart: (self) => {},
  onDragEnd: (self) => {},
  onLockAxis: (self) => {},

  // Lifecycle
  onEnable: (self) => {},
  onDisable: (self) => {},

  // Filtering
  id: "myObserver",
  ignore: ".no-observe",
  ignoreCheck: (event, isTouchOrPointer) => false
});
```

### Instance Properties & Methods

```js
observer.deltaX;        // Delta since last event
observer.deltaY;
observer.velocityX;     // Current velocity
observer.velocityY;
observer.x;             // Current position
observer.y;
observer.startX;        // Drag start position
observer.startY;
observer.event;         // Latest event
observer.target;        // Observed element
observer.isDragging;
observer.isEnabled;
observer.isPressed;
observer.axis;          // Locked axis ("x" | "y" | null)

observer.enable();
observer.disable();
observer.kill();
observer.scrollX();     // Get horizontal scroll
observer.scrollX(100);  // Set horizontal scroll
observer.scrollY();     // Get vertical scroll
observer.scrollY(100);  // Set vertical scroll
```

### Static Methods

```js
Observer.create(vars);
Observer.getAll();
Observer.getById("myId");
Observer.isTouch;        // 0 = no touch, 1 = touch, 2 = touch only
Observer.eventTypes;     // Available event types
```

---

## Draggable

### Import & Register

```js
import { Draggable } from "gsap/Draggable";
gsap.registerPlugin(Draggable);
```

### Usage

```js
const [draggable] = Draggable.create(".box", {
  type: "x,y",          // "x" | "y" | "x,y" | "rotation" | "scroll" | "scrollTop" | "scrollLeft" | "top" | "left" | "top,left"
  bounds: "#container",  // DOMTarget | { top, left, width, height } | { minX, maxX, minY, maxY } | { minRotation, maxRotation }
  inertia: true,         // Requires InertiaPlugin for throw behavior

  // Interaction
  trigger: ".handle",    // Only drag from this sub-element
  cursor: "grab",
  activeCursor: "grabbing",
  dragClickables: true,
  allowContextMenu: false,
  allowNativeTouchScrolling: true,
  minimumMovement: 1,

  // Physics
  edgeResistance: 0.65,
  dragResistance: 0,
  resistance: 0,
  throwResistance: 0,
  overshootTolerance: 0,
  maxDuration: 0.5,
  minDuration: 0.2,

  // Axis
  lockAxis: true,
  force3D: true,
  zIndexBoost: true,
  autoScroll: 1,         // Speed of auto-scroll near edges

  // Snapping
  snap: [0, 90, 180, 270],        // Array of snap points
  snap: (value) => Math.round(value / 45) * 45,  // Function
  snap: {
    x: [0, 100, 200],
    y: (val) => Math.round(val / 50) * 50,
    rotation: [0, 90, 180, 270],
    points: [{ x: 0, y: 0 }, { x: 100, y: 100 }],
    radius: 20
  },
  liveSnap: true,        // Snap while dragging (not just on release)

  // Callbacks
  onClick: function() {},
  onDragStart: function() {},
  onDrag: function() {},
  onDragEnd: function() {},
  onPress: function() {},
  onRelease: function() {},
  onMove: function() {},
  onThrowComplete: function() {},
  onThrowUpdate: function() {},
  onLockAxis: function(e) {},
  onPressInit: function() {},

  // Clickable test
  clickableTest: function(element) { return true; }
});
```

### Instance Properties

| Property | Type | Description |
|---|---|---|
| `x`, `y` | `number` | Current position |
| `rotation` | `number` | Current rotation |
| `deltaX`, `deltaY` | `number` | Change since last event |
| `startX`, `startY` | `number` | Start of current drag |
| `endX`, `endY` | `number` | Projected end position (with inertia) |
| `endRotation` | `number` | Projected end rotation |
| `pointerX`, `pointerY` | `number` | Current pointer position |
| `isDragging` | `boolean` | Currently being dragged |
| `isPressed` | `boolean` | Currently pressed |
| `isThrowing` | `boolean` | Currently in inertia throw |
| `lockAxis` | `boolean` | Axis locked |
| `minX`, `maxX`, `minY`, `maxY` | `number` | Current bounds |
| `minRotation`, `maxRotation` | `number` | Rotation bounds |
| `target` | `HTMLElement \| SVGElement` | Target element |
| `tween` | `Tween` | Active tween (during throw) |
| `autoScroll` | `number` | Auto-scroll speed |

### Instance Methods

```js
draggable.enable();
draggable.disable();
draggable.enabled();             // Get state
draggable.enabled(true);         // Set state
draggable.kill();
draggable.update();              // Sync position
draggable.update(true);          // Apply bounds
draggable.update(true, true);    // Sticky
draggable.applyBounds("#container");
draggable.applyBounds({ minX: 0, maxX: 500 });
draggable.startDrag(event);
draggable.startDrag(event, true); // Align to pointer
draggable.endDrag(event);
draggable.getDirection("start");   // Direction from start
draggable.getDirection("velocity");// Direction from velocity
draggable.getDirection(otherElem); // Direction relative to element
draggable.hitTest(otherElem, 20);  // Overlap test
draggable.timeSinceDrag();         // Seconds since last drag
draggable.addEventListener("drag", fn);
draggable.removeEventListener("drag", fn);
```

### Static Methods

```js
Draggable.create(targets, vars);    // Returns Draggable[]
Draggable.get(target);              // Get instance
Draggable.hitTest(el1, el2, "50%"); // Static overlap test
Draggable.timeSinceDrag();
Draggable.zIndex;                   // Global z-index counter
```

---

## SplitText

> Requires Club GSAP membership.

### Import & Register

```js
import { SplitText } from "gsap/SplitText";
gsap.registerPlugin(SplitText);
```

### Usage

```js
const split = new SplitText(".text", {
  type: "words,chars,lines",  // What to split into
  mask: "lines",               // "lines" | "words" | "chars" — wraps in overflow:hidden containers
  tag: "span",                 // HTML tag for split elements
  wordsClass: "word",
  charsClass: "char++",        // ++ adds incrementing number: char1, char2...
  linesClass: "line",
  aria: "auto",                // "auto" | "hidden" | "none" — accessibility
  autoSplit: true,             // Re-split on resize/font load
  smartWrap: true,             // Better line detection
  deepSlice: true,             // Handle nested elements
  propIndex: true,             // Set --char-index, --word-index CSS vars
  reduceWhiteSpace: true,
  specialChars: ["ñ", /[^\x00-\x7F]/],
  wordDelimiter: " ",          // or RegExp or { delimiter, replaceWith }
  ignore: ".skip-this",
  prepareText: (text, element) => text.trim(),
  overwrite: true,
  onSplit: (self) => {},
  onRevert: (self) => {}
});

// Or use static create method
const split = SplitText.create(".text", { type: "chars,words" });

// Access split elements
split.chars;      // Array of character elements
split.words;      // Array of word elements
split.lines;      // Array of line elements
split.masks;      // Array of mask wrapper elements
split.elements;   // Original target elements
split.isSplit;    // Boolean — is currently split?

// Animate
gsap.from(split.chars, {
  opacity: 0,
  y: 50,
  stagger: 0.02,
  ease: "back.out(1.7)"
});

// Re-split with different options
split.split({ type: "lines,chars" });

// Revert to original HTML
split.revert();

// Stop watching for resizes/font loads
split.kill();
```

---

## MotionPathPlugin

### Import & Register

```js
import { MotionPathPlugin } from "gsap/MotionPathPlugin";
gsap.registerPlugin(MotionPathPlugin);
```

### Usage

```js
gsap.to(".rocket", {
  duration: 5,
  motionPath: {
    path: "#myPath",          // SVG path element or data string
    align: "#myPath",         // Align to path
    alignOrigin: [0.5, 0.5],  // Alignment origin
    autoRotate: true,          // Rotate along path (or angle offset)
    curviness: 1.25,
    start: 0,                  // Progress start (0-1)
    end: 1,                    // Progress end (0-1)
    offsetX: 0,
    offsetY: 0,
    relative: false,
    resolution: 12,            // Measurement precision
    type: "cubic",
    useRadians: false,
    fromCurrent: false
  },
  ease: "none"
});

// Array of points
gsap.to(".ball", {
  motionPath: [
    { x: 100, y: 0 },
    { x: 200, y: 100 },
    { x: 300, y: 0 }
  ]
});
```

### Static Methods

```js
MotionPathPlugin.arrayToRawPath(points, { curviness: 0.5 });
MotionPathPlugin.cacheRawPathMeasurements(rawPath, resolution);
MotionPathPlugin.convertCoordinates(fromEl, toEl);
MotionPathPlugin.convertCoordinates(fromEl, toEl, point);
MotionPathPlugin.convertToPath("circle");           // SVG shape → path
MotionPathPlugin.convertToPath(svgElement, true);   // Swap in DOM
MotionPathPlugin.getAlignMatrix(fromEl, toEl);
MotionPathPlugin.getGlobalMatrix(element);
MotionPathPlugin.getGlobalMatrix(element, true);    // Inverse
MotionPathPlugin.getPositionOnPath(rawPath, 0.5);   // {x, y}
MotionPathPlugin.getPositionOnPath(rawPath, 0.5, true); // {x, y, angle}
MotionPathPlugin.getRawPath(pathElement);
MotionPathPlugin.getRelativePosition(fromEl, toEl, fromOrigin, toOrigin);
MotionPathPlugin.pointsToSegment(points, curviness);
MotionPathPlugin.rawPathToString(rawPath);
MotionPathPlugin.sliceRawPath(rawPath, start, end);
MotionPathPlugin.stringToRawPath("M0,0 C100,20 300,50 400,0...");
```

---

## TextPlugin

### Import & Register

```js
import { TextPlugin } from "gsap/TextPlugin";
gsap.registerPlugin(TextPlugin);
```

### Usage

```js
gsap.to(".text", {
  duration: 2,
  text: "New text content",         // Simple string
  ease: "none"
});

// Advanced options
gsap.to(".text", {
  duration: 2,
  text: {
    value: "New text content",
    type: "diff",           // "diff" (default) — character-by-character replacement
    delimiter: " ",          // Split by words
    speed: 1,                // Relative speed
    newClass: "new-char",    // Class for incoming text
    oldClass: "old-char",    // Class for outgoing text
    rtl: false,              // Right-to-left
    padSpace: false,         // Keep space during transition
    preserveSpaces: false
  },
  ease: "none"
});
```

---

## DrawSVGPlugin

> Requires Club GSAP membership.

### Import & Register

```js
import { DrawSVGPlugin } from "gsap/DrawSVGPlugin";
gsap.registerPlugin(DrawSVGPlugin);
```

### Usage

```js
// Draw from nothing to full
gsap.from(".line", { drawSVG: 0 });

// Draw from 0% to 50%
gsap.to(".line", { drawSVG: "0% 50%" });

// Specific range
gsap.to(".line", { drawSVG: "20% 80%" });

// Boolean
gsap.from(".line", { drawSVG: false }); // Same as 0

// Static helpers
DrawSVGPlugin.getLength(element);     // Total stroke length
DrawSVGPlugin.getPosition(element);   // Current [start, end]
```

---

## Other Plugins

### MorphSVGPlugin

> Requires Club GSAP membership.

```js
import { MorphSVGPlugin } from "gsap/MorphSVGPlugin";

gsap.to("#circle", {
  morphSVG: "#star",
  duration: 1,
  ease: "power2.inOut"
});
```

### ScrambleTextPlugin

> Requires Club GSAP membership.

```js
import { ScrambleTextPlugin } from "gsap/ScrambleTextPlugin";

gsap.to(".text", {
  scrambleText: {
    text: "FINAL TEXT",
    chars: "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
    speed: 0.5,
    revealDelay: 0.5,
    tweenLength: false,
    delimiter: " "
  }
});
```

### ScrollToPlugin

```js
import { ScrollToPlugin } from "gsap/ScrollToPlugin";

gsap.to(window, {
  scrollTo: { y: "#section2", offsetY: 50 },
  duration: 1
});
```

### InertiaPlugin

> Requires Club GSAP membership. Powers Draggable throw behavior.

### Physics2DPlugin / PhysicsPropsPlugin

> Requires Club GSAP membership. Physics-based animations.

### EaselPlugin

For Adobe Animate (Canvas/EaselJS) integration.

### PixiPlugin

For PixiJS integration.

### CSSRulePlugin

For animating CSS rules (pseudo-elements, etc.).

---

## TypeScript Types Reference

### Core Types

```typescript
type DOMTarget = Element | string | null | Window | ArrayLike<Element | string | Window | null>;
type TweenTarget = string | object | null;
type Callback = (...args: any[]) => any;
type Position = number | string;
type EaseString = "none" | "power1" | "power1.in" | "power1.out" | "power1.inOut"
  | "power2" | "power2.in" | "power2.out" | "power2.inOut"
  | "power3" | "power3.in" | "power3.out" | "power3.inOut"
  | "power4" | "power4.in" | "power4.out" | "power4.inOut"
  | "back" | "back.in" | "back.out" | "back.inOut"
  | "bounce" | "bounce.in" | "bounce.out" | "bounce.inOut"
  | "circ" | "circ.in" | "circ.out" | "circ.inOut"
  | "elastic" | "elastic.in" | "elastic.out" | "elastic.inOut"
  | "expo" | "expo.in" | "expo.out" | "expo.inOut"
  | "sine" | "sine.in" | "sine.out" | "sine.inOut"
  | ({} & string);  // Also accepts custom ease names

type FunctionBasedValue<T> = (index: number, target: any, targets: any[]) => T;
type NumberValue = number | FunctionBasedValue<number>;
type StringValue = string | FunctionBasedValue<string>;
type BooleanValue = boolean | FunctionBasedValue<boolean>;
type TweenValue = NumberValue | StringValue;
type CallbackType = "onComplete" | "onInterrupt" | "onRepeat" | "onReverseComplete" | "onStart" | "onUpdate";
type TickerCallback = (time: number, deltaTime: number, frame: number, elapsed: number) => void | null;

type Point2D = { x: number; y: number };
type SVGPathValue = string | SVGPathElement;

interface QuickToFunc {
  (value: number, start?: number, startIsRelative?: boolean): core.Tween;
  tween: core.Tween;
}
```

### Animation Hierarchy

```
gsap.core.Animation     (base class — shared controls)
  ├── gsap.core.Tween   (single property animation)
  └── gsap.core.Timeline (sequencing container)
```

### Global Type Aliases

```typescript
type GSAPAnimation = gsap.core.Animation;
type GSAPTimeline = gsap.core.Timeline;
type GSAPTween = gsap.core.Tween;
type GSAPTweenVars = gsap.TweenVars;
type GSAPTimelineVars = gsap.TimelineVars;
type GSAPTweenTarget = gsap.TweenTarget;
type GSAPCallback = gsap.Callback;
type GSAPPlugin = gsap.Plugin;
type GSAPTickerCallback = gsap.TickerCallback;
type GSAPStaggerVars = gsap.StaggerVars;
type GSAPDraggableVars = Draggable.Vars;
type GSAPDistributeConfig = gsap.utils.DistributeConfig;
type GSAP = typeof gsap;
```

---

## React Integration — @gsap/react

### Installation

```bash
npm install gsap @gsap/react
```

### useGSAP Hook

```jsx
import { useGSAP } from "@gsap/react";
import gsap from "gsap";

gsap.registerPlugin(useGSAP);

function MyComponent() {
  const container = useRef(null);

  useGSAP(() => {
    // All animations created here are automatically cleaned up
    gsap.to(".box", { x: 100, rotation: 360 });

    // ScrollTrigger-based animations are also cleaned up
    gsap.to(".panel", {
      y: -200,
      scrollTrigger: {
        trigger: ".panel",
        start: "top bottom",
        end: "top top",
        scrub: true
      }
    });
  }, { scope: container }); // Scope selectors to container

  return (
    <div ref={container}>
      <div className="box">Animated</div>
      <div className="panel">Scrolling</div>
    </div>
  );
}
```

### With Dependencies

```jsx
useGSAP(() => {
  gsap.to(".box", { x: endX });
}, { dependencies: [endX], scope: container });
```

### Manual Context (without @gsap/react)

```jsx
useEffect(() => {
  const ctx = gsap.context(() => {
    gsap.to(".box", { x: 100 });
  }, containerRef);

  return () => ctx.revert(); // Cleanup
}, []);
```

---

## Licensing

### Standard (No-Charge) License

Free for most use cases including commercial websites. Includes:
- GSAP Core (gsap.to, gsap.from, gsap.fromTo, gsap.set, gsap.timeline)
- CSSPlugin (auto-included)
- ScrollTrigger
- Observer
- Draggable
- MotionPathPlugin
- TextPlugin
- ScrollToPlugin
- EasePack

### Club GSAP (Paid)

Premium plugins requiring a paid license:
- ScrollSmoother
- SplitText
- DrawSVGPlugin
- MorphSVGPlugin
- ScrambleTextPlugin
- InertiaPlugin
- Flip
- CustomEase / CustomBounce / CustomWiggle
- GSDevTools
- MotionPathHelper
- Physics2DPlugin / PhysicsPropsPlugin

> Full license details: https://gsap.com/standard-license

---

> **Repository**: https://github.com/greensock/GSAP
> **Documentation**: https://gsap.com/docs/v3/
> **Version**: 3.15.0
> **Maintainer**: Jack Doyle (GreenSock)
