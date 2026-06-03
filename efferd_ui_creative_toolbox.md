# Efferd UI Creative Developer Toolbox Reference Guide

A unified developer reference guide for **Efferd UI** (efferd.com / legacy.efferd.com / sv-efferd.pages.dev) — a premium set of copy-paste marketing blocks and utility UI components designed for landing pages, forms, and applications. This guide contains complete source code, installation CLI commands, and dependencies for both **React (Next.js / Radix UI / Motion)** and **Svelte (SvelteKit / Bits UI / Motion Svelte)** versions of each block.

## Table of Contents

- [UI Components & Utilities](#ui)
  - [Dashed Line](#dashed-line)
  - [Decor Icon](#decor-icon)
  - [Floating Paths](#floating-paths)
  - [Full Width Divider](#full-width-divider)
  - [Grid Filler](#grid-filler)
  - [Grid Pattern](#grid-pattern)
  - [Marquee](#marquee)
  - [Mask Line](#mask-line)
  - [Particles](#particles)
  - [Portal](#portal)
  - [Progressive Blur](#progressive-blur)
- [Auth Blocks](#auth)
  - [Auth 1](#auth-1)
  - [Auth 2](#auth-2)
  - [Auth 3](#auth-3)
  - [Auth 4](#auth-4)
  - [Auth 5](#auth-5)
  - [Auth Divider](#auth-divider)
- [Blog Blocks](#blog)
  - [Blog 1](#blog-1)
  - [Blog 2](#blog-2)
  - [Blog 3](#blog-3)
- [Contact Blocks](#contact)
  - [Contact 1](#contact-1)
  - [Contact 2](#contact-2)
  - [Contact 3](#contact-3)
  - [Contact 4](#contact-4)
  - [Contact 5](#contact-5)
- [CTA Blocks](#cta)
  - [Cta 1](#cta-1)
  - [Cta 2](#cta-2)
  - [Cta 3](#cta-3)
  - [Cta 4](#cta-4)
  - [Cta 5](#cta-5)
- [FAQ Blocks](#faq)
  - [Faq 1](#faq-1)
  - [Faq 2](#faq-2)
  - [Faq 3](#faq-3)
  - [Faq 4](#faq-4)
  - [Faq 5](#faq-5)
- [Feature Blocks](#feature)
  - [Feature 1](#feature-1)
  - [Feature 2](#feature-2)
  - [Feature 3](#feature-3)
  - [Feature 4](#feature-4)
  - [Feature 5](#feature-5)
- [Footer Blocks](#footer)
  - [Footer 1](#footer-1)
  - [Footer 2](#footer-2)
  - [Footer 3](#footer-3)
  - [Footer 4](#footer-4)
  - [Footer 5](#footer-5)
  - [Footer 6](#footer-6)
- [Form Blocks](#form)
  - [Form 1](#form-1)
  - [Form 2](#form-2)
  - [Form 3](#form-3)
- [Header Blocks](#header)
  - [Header 1](#header-1)
  - [Header 2](#header-2)
  - [Header 3](#header-3)
  - [Header 4](#header-4)
  - [Header 5](#header-5)
- [Hero Blocks](#hero)
  - [Hero 1](#hero-1)
  - [Hero 2](#hero-2)
  - [Hero 3](#hero-3)
- [Image Gallery Blocks](#image-gallery)
  - [Image Gallery 1](#image-gallery-1)
- [Logo Cloud Blocks](#logo-cloud)
  - [Logo Cloud 1](#logo-cloud-1)
  - [Logo Cloud 2](#logo-cloud-2)
  - [Logo Cloud 3](#logo-cloud-3)
  - [Logo Cloud 4](#logo-cloud-4)
  - [Logo Cloud 5](#logo-cloud-5)
- [Not Found Blocks](#not-found)
  - [Not Found 1](#not-found-1)
  - [Not Found 2](#not-found-2)
- [Pricing Blocks](#pricing)
  - [Pricing 1](#pricing-1)
  - [Pricing 2](#pricing-2)
  - [Pricing 3](#pricing-3)
  - [Pricing 4](#pricing-4)
- [Testimonial Blocks](#testimonial)
  - [Testimonial 1](#testimonial-1)
  - [Testimonial 2](#testimonial-2)
  - [Testimonial 3](#testimonial-3)
  - [Testimonial 4](#testimonial-4)
  - [Testimonial 5](#testimonial-5)
- [Integration Blocks](#integration)
  - [Integration 1](#integration-1)
  - [Integration 2](#integration-2)
  - [Integration 3](#integration-3)
  - [Integration 4](#integration-4)
  - [Integration 5](#integration-5)

---

## UI Components & Utilities <a name="ui"></a>

### Dashed Line <a name="dashed-line"></a>

A premium dashed line block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `dashed-line`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/dashed-line.json
```

##### Component Source Code

###### File: `src/lib/components/ui/dashed-line/dashed-line.svelte`

```svelte
<script lang="ts">
	import { cn, type WithElementRef } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";

	let {
		ref = $bindable(null),
		class: className,
		"aria-hidden": ariaHidden = "true",
		...restProps
	}: WithElementRef<HTMLAttributes<HTMLDivElement>> = $props();
</script>

<div
	bind:this={ref}
	aria-hidden={ariaHidden}
	data-slot="dashed-line"
	class={cn("absolute border-collapse border border-dashed", className)}
	{...restProps}
></div>

```

###### File: `src/lib/components/ui/dashed-line/index.ts`

```ts
import DashedLine from "./dashed-line.svelte";

export { DashedLine };

```

---

### Decor Icon <a name="decor-icon"></a>

A premium decor icon block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `decor-icon`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/decor-icon.json
```

##### Component Source Code

###### File: `src/lib/components/ui/decor-icon/decor-icon.svelte`

```svelte
<script lang="ts" module>
	import { cn } from "$lib/utils";
	import type { SVGAttributes } from "svelte/elements";
	import { type VariantProps, tv } from "tailwind-variants";

	export const decorIconVariants = tv({
		base: "pointer-events-none absolute z-1 size-5 shrink-0 stroke-muted-foreground stroke-1",
		variants: {
			position: {
				"top-left":
					"top-0 left-0 -translate-x-[calc(50%+0.5px)] -translate-y-[calc(50%+0.5px)]",
				"top-right":
					"top-0 right-0 translate-x-[calc(50%+0.5px)] -translate-y-[calc(50%+0.5px)]",
				"bottom-right":
					"right-0 bottom-0 translate-x-[calc(50%+0.5px)] translate-y-[calc(50%+0.5px)]",
				"bottom-left":
					"bottom-0 left-0 -translate-x-[calc(50%+0.5px)] translate-y-[calc(50%+0.5px)]"
			}
		},
		defaultVariants: {
			position: "top-left"
		}
	});

	type DecorIconVariant = SVGAttributes<SVGAElement> & VariantProps<typeof decorIconVariants>;
</script>

<script lang="ts">
	let { position = "top-left", class: className, ...props }: DecorIconVariant = $props();
</script>

<svg
	aria-hidden="true"
	class={cn(decorIconVariants({ position }), className)}
	fill="none"
	stroke="currentColor"
	stroke-linecap="round"
	stroke-linejoin="round"
	viewBox="0 0 24 24"
	xmlns="http://www.w3.org/2000/svg"
>
	<path d="M5 12h14" />
	<path d="M12 5v14" />
</svg>

```

###### File: `src/lib/components/ui/decor-icon/index.ts`

```ts
import DecorIcon from "./decor-icon.svelte";

export { DecorIcon };

```

---

### Floating Paths <a name="floating-paths"></a>

A premium floating paths block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `floating-paths`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/floating-paths.json
```

##### Dependencies

- **NPM Packages**:
  - `motion-sv`

##### Component Source Code

###### File: `src/lib/components/ui/floating-paths/floating-paths.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import { motion } from "motion-sv";
	import type { HTMLAttributes } from "svelte/elements";

	type FloatingPath = {
		id: number;
		d: string;
		opacity: number;
		width: number;
		duration: number;
	};

	type FloatingPathsProps = HTMLAttributes<HTMLDivElement> & {
		class?: string;
		position: number;
	};

	let { class: className = "", position, ...restProps }: FloatingPathsProps = $props();

	const paths = $derived.by<FloatingPath[]>(() =>
		Array.from({ length: 36 }, (_, index) => ({
			id: index,
			d: `M-${380 - index * 5 * position} -${189 + index * 6}C-${380 - index * 5 * position} -${189 + index * 6} -${312 - index * 5 * position} ${216 - index * 6} ${152 - index * 5 * position} ${343 - index * 6}C${616 - index * 5 * position} ${470 - index * 6} ${684 - index * 5 * position} ${875 - index * 6} ${684 - index * 5 * position} ${875 - index * 6}`,
			opacity: 0.1 + index * 0.03,
			width: 0.5 + index * 0.03,
			duration: 20 + (index % 10)
		}))
	);
</script>

<div
	aria-hidden="true"
	class={cn("pointer-events-none absolute inset-0", className)}
	{...restProps}
>
	<svg class="h-full w-full text-primary" fill="none" viewBox="0 0 696 316">
		<title>Background Paths</title>

		{#each paths as path (path.id)}
			<motion.path
				animate={{ pathLength: 1, opacity: [0.3, 0.6, 0.3], pathOffset: [0, 1, 0] }}
				d={path.d}
				initial={{ pathLength: 0.3, opacity: 0.6 }}
				stroke="currentColor"
				stroke-opacity={path.opacity}
				stroke-width={path.width}
				transition={{
					duration: path.duration,
					repeat: Number.POSITIVE_INFINITY,
					ease: "linear"
				}}
			/>
		{/each}
	</svg>
</div>

```

###### File: `src/lib/components/ui/floating-paths/index.ts`

```ts
import Root from "./floating-paths.svelte";

export {
	Root,
	//
	Root as FloatingPaths
};

```

---

### Full Width Divider <a name="full-width-divider"></a>

A premium full width divider block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `full-width-divider`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/full-width-divider.json
```

##### Component Source Code

###### File: `src/lib/components/ui/full-width-divider/full-width-divider.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils.js";
	import type { HTMLAttributes } from "svelte/elements";

	type FullWidthDividerProps = HTMLAttributes<HTMLDivElement> & {
		contained?: boolean;
		position?: "top" | "bottom";
	};

	let {
		class: className,
		contained = false,
		position,
		"aria-hidden": ariaHidden = "true",
		...rest
	}: FullWidthDividerProps = $props();
</script>

<div
	aria-hidden={ariaHidden}
	data-contained={contained}
	data-position={position}
	class={cn(
		"pointer-events-none absolute h-px bg-border",
		// full-bleed (default)
		"data-[contained=false]:left-1/2 data-[contained=false]:w-screen data-[contained=false]:-translate-x-1/2",
		// contained
		"data-[contained=true]:inset-x-0 data-[contained=true]:w-full",
		// position
		position && "data-[position=bottom]:-bottom-px data-[position=top]:-top-px",
		className
	)}
	{...rest}
></div>

```

###### File: `src/lib/components/ui/full-width-divider/index.ts`

```ts
import FullWidthDivider from "./full-width-divider.svelte";
export { FullWidthDivider };

```

---

### Grid Filler <a name="grid-filler"></a>

A premium grid filler block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `grid-filler`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/grid-filler.json
```

##### Component Source Code

###### File: `src/lib/components/ui/grid-filler/grid-filler.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";

	type GridFillerProps = HTMLAttributes<HTMLDivElement> & {
		/**
		 * The number of items in the grid.
		 */
		totalItems: number;
		/**
		 * Number of columns for small screens.
		 */
		smColumns?: number;
		/**
		 * Number of columns for medium screens.
		 */
		mdColumns?: number;
		/**
		 * Number of columns for large screens.
		 */
		lgColumns?: number;
	};

	let {
		totalItems,
		smColumns = 2,
		mdColumns = 0,
		lgColumns = 0,
		class: className = "",
		...rest
	}: GridFillerProps = $props();

	let actualMdColumns = $derived(mdColumns ?? smColumns);
	let actualLgColumns = $derived(lgColumns ?? actualMdColumns);

	// We need enough iterations to cover the maximum possible remainder.
	// For N columns, the remainder can be at most N-1.
	let maxFillers = $derived(Math.max(smColumns, actualMdColumns, actualLgColumns) - 1);
</script>

{#each { length: maxFillers } as _, i}
	{@const neededSm = (smColumns - (totalItems % smColumns)) % smColumns}
	{@const neededMd = (actualMdColumns - (totalItems % actualMdColumns)) % actualMdColumns}
	{@const neededLg = (actualLgColumns - (totalItems % actualLgColumns)) % actualLgColumns}
	{@const showSm = i < neededSm ? "sm:block" : "sm:hidden"}
	{@const showMd = i < neededMd ? "md:block" : "md:hidden"}
	{@const showLg = i < neededLg ? "lg:block" : "lg:hidden"}

	<!-- {#if showSm === 'sm:hidden' && showMd === 'md:hidden' && showLg === 'lg:hidden'}
		<div></div>
	{/if} -->
	<div
		class={cn("pointer-events-none hidden", showSm, showMd, showLg, className)}
		{...rest}
	></div>
{/each}

```

###### File: `src/lib/components/ui/grid-filler/index.ts`

```ts
import GridFiller from "./grid-filler.svelte";
export { GridFiller };

```

---

### Grid Pattern <a name="grid-pattern"></a>

A premium grid pattern block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `grid-pattern`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/grid-pattern.json
```

##### Component Source Code

###### File: `src/lib/components/ui/grid-pattern/grid-pattern.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import type { SVGAttributes } from "svelte/elements";

	interface GridPatternProps extends SVGAttributes<SVGSVGElement> {
		width?: number;
		height?: number;
		/**
		 * The x-offset of the pattern
		 */
		x?: number;
		/**
		 * The y-offset of the pattern
		 */
		y?: number;
		/**
		 * Array of [x, y] coordinates for highlighted squares
		 */
		squares?: Array<[x: number, y: number]>;
		/**
		 * Stroke dash array for dashed lines
		 */
		strokeDashArray?: string;
		class?: string;
	}

	let {
		width = 40,
		height = 40,
		x = -1,
		y = -1,
		strokeDashArray = "0",
		squares,
		class: className,
		...props
	}: GridPatternProps = $props();

	const id = $props.id();
</script>

<svg
	aria-hidden="true"
	class={cn(
		"pointer-events-none absolute inset-0 h-full w-full fill-gray-400/30 stroke-gray-400/30",
		className
	)}
	{...props}
>
	<defs>
		<pattern {id} {width} {height} patternUnits="userSpaceOnUse" {x} {y}>
			<path d={`M.5 ${height}V.5H${width}`} fill="none" stroke-dasharray={strokeDashArray} />
		</pattern>
	</defs>
	<rect width="100%" height="100%" stroke-width={0} fill={`url(#${id})`} />
	{#if squares}
		<svg {x} {y} class="overflow-visible">
			{#each squares as [squareX, squareY], i (`${squareX}-${squareY}-${i}`)}
				<rect
					stroke-width="0"
					width={width - 1}
					height={height - 1}
					x={squareX * width + 1}
					y={squareY * height + 1}
				/>
			{/each}
		</svg>
	{/if}
</svg>

```

###### File: `src/lib/components/ui/grid-pattern/index.ts`

```ts
export { default as GridPattern } from "./grid-pattern.svelte";

```

---

### Marquee <a name="marquee"></a>

A premium marquee block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `marquee`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/marquee.json
```

##### Component Source Code

###### File: `src/lib/components/magic/marquee/marquee.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import type { Snippet } from "svelte";

	interface MarqueeProps {
		children: Snippet;
		class?: string;
		reverse?: boolean;
		pauseOnHover?: boolean;
		vertical?: boolean;
		repeat?: number;
		[key: string]: any;
	}

	let {
		children,
		class: className = "",
		reverse = false,
		pauseOnHover = false,
		vertical = false,
		repeat = 4,
		...rest
	}: MarqueeProps = $props();
</script>

<div
	{...rest}
	class={cn(
		"group flex gap-(--gap) overflow-hidden p-2 [--duration:40s] [--gap:1rem]",
		{
			"flex-row": !vertical,
			"flex-col": vertical
		},
		className
	)}
>
	{#each Array(repeat).fill(0) as _, i (i)}
		<div
			class={cn("flex shrink-0 justify-around gap-(--gap)", {
				"animate-marquee flex-row": !vertical,
				"animate-marquee-vertical flex-col": vertical,
				"group-hover:paused": pauseOnHover,
				"direction-[reverse]": reverse
			})}
		>
			{@render children?.()}
		</div>
	{/each}
</div>

```

###### File: `src/lib/components/magic/marquee/index.ts`

```ts
import Marquee from "./marquee.svelte";
export { Marquee };

```

---

### Mask Line <a name="mask-line"></a>

A premium mask line block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `mask-line`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/mask-line.json
```

##### Component Source Code

###### File: `src/lib/components/ui/mask-line/mask-line.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";

	type MaskLineProps = HTMLAttributes<HTMLDivElement> & {
		orientation?: "vertical" | "horizontal";
		class?: string;
	};
	let { orientation = "vertical", class: className = "", ...props }: MaskLineProps = $props();
</script>

<div
	aria-hidden="true"
	class={cn(
		"absolute bg-foreground/20",
		orientation === "vertical" && "-inset-y-1/2 w-px mask-t-from-80% mask-b-from-80%",
		orientation === "horizontal" && "-inset-x-1/2 h-px mask-r-from-80% mask-l-from-80%",
		className
	)}
	{...props}
></div>

```

###### File: `src/lib/components/ui/mask-line/index.ts`

```ts
import MaskLine from "./mask-line.svelte";

export { MaskLine };

```

---

### Particles <a name="particles"></a>

A premium particles block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `particles`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/particles.json
```

##### Component Source Code

###### File: `src/lib/components/magic/particles/particles.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils.js";
	import { untrack } from "svelte";
	import type { HTMLAttributes } from "svelte/elements";

	type RgbColor = [red: number, green: number, blue: number];

	type Point = {
		x: number;
		y: number;
	};

	type CanvasSize = {
		w: number;
		h: number;
	};

	type Circle = {
		x: number;
		y: number;
		translateX: number;
		translateY: number;
		size: number;
		alpha: number;
		targetAlpha: number;
		dx: number;
		dy: number;
		magnetism: number;
	};

	type ParticlesProps = HTMLAttributes<HTMLDivElement> & {
		class?: string;
		quantity?: number;
		staticity?: number;
		ease?: number;
		size?: number;
		refresh?: boolean;
		color?: string;
		vx?: number;
		vy?: number;
	};

	let {
		class: classProp,
		class: className = "",
		quantity = 100,
		staticity = 50,
		ease = 50,
		size = 0.4,
		refresh = true,
		color = "#ffffff",
		vx = 0,
		vy = 0,
		...restProps
	}: ParticlesProps = $props();

	let canvasRef = $state<HTMLCanvasElement | null>(null);
	let canvasContainerRef = $state<HTMLDivElement | null>(null);
	let context = $state<CanvasRenderingContext2D | null>(null);
	let mouse = $state<Point>({ x: 0, y: 0 });
	let canvasSize = $state<CanvasSize>({ w: 0, h: 0 });

	let circles: Circle[] = [];
	let frameId: number | null = null;

	const rgb = $derived(hexToRgb(color));
	const normalizedEase = $derived(Math.max(ease, 1));
	const normalizedStaticity = $derived(Math.max(staticity, 1));
	const normalizedQuantity = $derived(Math.max(0, Math.floor(quantity)));
	const normalizedSize = $derived(Math.max(size, 0));

	function hexToRgb(hex: string): RgbColor {
		const normalizedHex = hex.trim().replace(/^#/, "");
		const expandedHex =
			normalizedHex.length === 3
				? normalizedHex
						.split("")
						.map((char) => `${char}${char}`)
						.join("")
				: normalizedHex;

		if (!/^[0-9a-fA-F]{6}$/.test(expandedHex)) {
			return [255, 255, 255];
		}

		const hexInt = Number.parseInt(expandedHex, 16);

		return [(hexInt >> 16) & 255, (hexInt >> 8) & 255, hexInt & 255];
	}

	function createCircle(particleSize: number = normalizedSize): Circle {
		return {
			x: Math.floor(Math.random() * canvasSize.w),
			y: Math.floor(Math.random() * canvasSize.h),
			translateX: 0,
			translateY: 0,
			size: Math.floor(Math.random() * 2) + particleSize,
			alpha: 0,
			targetAlpha: Number.parseFloat((Math.random() * 0.6 + 0.1).toFixed(1)),
			dx: (Math.random() - 0.5) * 0.1,
			dy: (Math.random() - 0.5) * 0.1,
			magnetism: 0.1 + Math.random() * 4
		};
	}

	function seedParticles(
		particleQuantity: number = normalizedQuantity,
		particleSize: number = normalizedSize
	): void {
		circles = Array.from({ length: particleQuantity }, () => createCircle(particleSize));
	}

	function resizeCanvas(): void {
		if (!canvasContainerRef || !canvasRef || !context) {
			return;
		}

		canvasSize = {
			w: canvasContainerRef.offsetWidth,
			h: canvasContainerRef.offsetHeight
		};

		const devicePixelRatio = window.devicePixelRatio || 1;

		canvasRef.width = canvasSize.w * devicePixelRatio;
		canvasRef.height = canvasSize.h * devicePixelRatio;
		canvasRef.style.width = `${canvasSize.w}px`;
		canvasRef.style.height = `${canvasSize.h}px`;
		context.setTransform(devicePixelRatio, 0, 0, devicePixelRatio, 0, 0);

		seedParticles();
		renderParticles();
	}

	function clearContext(): void {
		if (!context) {
			return;
		}

		context.clearRect(0, 0, canvasSize.w, canvasSize.h);
	}

	function drawCircle(circle: Circle, colorValue: RgbColor = rgb): void {
		if (!context) {
			return;
		}

		const [red, green, blue] = colorValue;

		context.beginPath();
		context.arc(
			circle.x + circle.translateX,
			circle.y + circle.translateY,
			circle.size,
			0,
			Math.PI * 2
		);
		context.fillStyle = `rgba(${red}, ${green}, ${blue}, ${circle.alpha})`;
		context.fill();
	}

	function renderParticles(colorValue: RgbColor = rgb): void {
		clearContext();

		for (const circle of circles) {
			drawCircle(circle, colorValue);
		}
	}

	function remapValue(
		value: number,
		start1: number,
		end1: number,
		start2: number,
		end2: number
	): number {
		const remapped = ((value - start1) * (end2 - start2)) / (end1 - start1) + start2;
		return Math.max(remapped, 0);
	}

	function updateMousePosition(event: MouseEvent): void {
		if (!canvasRef) {
			return;
		}

		const rect = canvasRef.getBoundingClientRect();
		const x = event.clientX - rect.left - canvasSize.w / 2;
		const y = event.clientY - rect.top - canvasSize.h / 2;
		const inside =
			x < canvasSize.w / 2 &&
			x > -canvasSize.w / 2 &&
			y < canvasSize.h / 2 &&
			y > -canvasSize.h / 2;

		mouse = inside ? { x, y } : { x: 0, y: 0 };
	}

	function resetMousePosition(): void {
		mouse = { x: 0, y: 0 };
	}

	function animate(): void {
		clearContext();

		for (let index = 0; index < circles.length; index += 1) {
			const circle = circles[index];
			const edgeDistances = [
				circle.x + circle.translateX - circle.size,
				canvasSize.w - circle.x - circle.translateX - circle.size,
				circle.y + circle.translateY - circle.size,
				canvasSize.h - circle.y - circle.translateY - circle.size
			];
			const closestEdge = Math.min(...edgeDistances);
			const edgeAlpha = Number.parseFloat(remapValue(closestEdge, 0, 20, 0, 1).toFixed(2));

			if (edgeAlpha > 1) {
				circle.alpha = Math.min(circle.alpha + 0.02, circle.targetAlpha);
			} else {
				circle.alpha = circle.targetAlpha * edgeAlpha;
			}

			circle.x += circle.dx + vx;
			circle.y += circle.dy + vy;
			circle.translateX +=
				(mouse.x / (normalizedStaticity / circle.magnetism) - circle.translateX) /
				normalizedEase;
			circle.translateY +=
				(mouse.y / (normalizedStaticity / circle.magnetism) - circle.translateY) /
				normalizedEase;

			if (
				circle.x < -circle.size ||
				circle.x > canvasSize.w + circle.size ||
				circle.y < -circle.size ||
				circle.y > canvasSize.h + circle.size
			) {
				circles[index] = createCircle();
			}

			drawCircle(circles[index]);
		}

		frameId = window.requestAnimationFrame(animate);
	}

	$effect(() => {
		if (!canvasRef || !canvasContainerRef) {
			return;
		}

		const nextContext = canvasRef.getContext("2d");

		if (!nextContext) {
			return;
		}

		context = nextContext;

		const resizeObserver = new ResizeObserver(() => {
			untrack(() => {
				resizeCanvas();
			});
		});

		resizeObserver.observe(canvasContainerRef);
		window.addEventListener("mousemove", updateMousePosition);
		window.addEventListener("blur", resetMousePosition);

		untrack(() => {
			resizeCanvas();
			frameId = window.requestAnimationFrame(animate);
		});

		return () => {
			resizeObserver.disconnect();
			window.removeEventListener("mousemove", updateMousePosition);
			window.removeEventListener("blur", resetMousePosition);

			if (frameId !== null) {
				window.cancelAnimationFrame(frameId);
				frameId = null;
			}

			clearContext();
			circles = [];
			context = null;
		};
	});

	$effect(() => {
		const particleQuantity = normalizedQuantity;
		const particleSize = normalizedSize;
		refresh;

		if (!context) {
			return;
		}

		untrack(() => {
			seedParticles(particleQuantity, particleSize);
			renderParticles();
		});
	});

	$effect(() => {
		const currentRgb = rgb;

		if (!context) {
			return;
		}

		untrack(() => {
			renderParticles(currentRgb);
		});
	});
</script>

<div
	bind:this={canvasContainerRef}
	aria-hidden="true"
	class={cn(classProp, className)}
	{...restProps}
>
	<canvas bind:this={canvasRef} class="size-full"></canvas>
</div>

<style>
	.size-full {
		width: 100%;
		height: 100%;
	}
</style>

```

###### File: `src/lib/components/magic/particles/index.ts`

```ts
import Particles from "./particles.svelte";
export { Particles };

```

---

### Portal <a name="portal"></a>

A premium portal block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `portal`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/portal.json
```

##### Dependencies

- **NPM Packages**:
  - `bits-ui`

##### Component Source Code

###### File: `src/lib/components/ui/portal/portal.svelte`

```svelte
<!-- Portal.svelte -->
<script lang="ts">
	import { Portal } from "bits-ui";
	import { cn } from "$lib/utils";
	import { onMount } from "svelte";
	import type { Snippet } from "svelte";

	type Props = {
		class?: string;
		children?: Snippet;
	};

	let { class: className, children }: Props = $props();

	let mounted = $state(false);

	onMount(() => {
		mounted = true;

		const originalOverflow = window.getComputedStyle(document.body).overflow;
		const scrollbarWidth = window.innerWidth - document.documentElement.clientWidth;
		const originalPaddingRight = document.body.style.paddingRight;

		document.body.style.overflow = "hidden";
		if (scrollbarWidth > 0) {
			document.body.style.paddingRight = `${scrollbarWidth}px`;
		}

		return () => {
			document.body.style.overflow = originalOverflow;
			document.body.style.paddingRight = originalPaddingRight;
		};
	});
</script>

{#if mounted}
	<Portal to="body">
		<div class={cn("fixed inset-0 isolate z-40 flex flex-col", className)}>
			{@render children?.()}
		</div>
	</Portal>
{/if}

```

###### File: `src/lib/components/ui/portal/portal-backdrop.svelte`

```svelte
<!-- PortalBackdrop.svelte -->
<script lang="ts">
	import { cn } from "$lib/utils";
	import type { Snippet } from "svelte";

	type Props = {
		class?: string;
		state?: "open" | "closed";
		children?: Snippet;
	};

	let { class: className, state = "open", children }: Props = $props();
</script>

<div
	data-state={state}
	class={cn(
		"fixed inset-0 -z-1 bg-background/95 backdrop-blur-sm duration-500 data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:animate-in data-[state=open]:fade-in-0 supports-backdrop-filter:bg-background/60",
		className
	)}
>
	{@render children?.()}
</div>

```

###### File: `src/lib/components/ui/portal/index.ts`

```ts
import Portal from "./portal.svelte";
import PortalBackdrop from "./portal-backdrop.svelte";

export { Portal, PortalBackdrop };

```

---

### Progressive Blur <a name="progressive-blur"></a>

A premium progressive blur block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `progressive-blur`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/progressive-blur.json
```

##### Component Source Code

###### File: `src/lib/components/ui/progressive-blur/progressive-blur.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";

	const GRADIENT_ANGLES = {
		top: 0,
		right: 90,
		bottom: 180,
		left: 270
	};
	type ProgressiveBlurProps = {
		direction?: keyof typeof GRADIENT_ANGLES;
		blurLayers?: number;
		class?: string;
		blurIntensity?: number;
	};

	let {
		direction = "bottom",
		blurLayers = 8,
		class: className = "",
		blurIntensity = 0.25
	}: ProgressiveBlurProps = $props();

	let layers = $derived(Math.max(blurLayers, 2));
	let segmentSize = $derived(1 / (blurLayers + 1));
</script>

<div class={cn("relative", className)}>
	{#each { length: layers } as _, index}
		{@const angle = GRADIENT_ANGLES[direction]}
		{@const gradientStops = [
			index * segmentSize,
			(index + 1) * segmentSize,
			(index + 2) * segmentSize,
			(index + 3) * segmentSize
		].map(
			(pos, posIndex) =>
				`rgba(255, 255, 255, ${posIndex === 1 || posIndex === 2 ? 1 : 0}) ${pos * 100}%`
		)}
		{@const gradient = `linear-gradient(${angle}deg, ${gradientStops.join(", ")})`}
		<div
			class="pointer-events-none absolute inset-0 rounded-[inherit]"
			style="mask-image: {gradient};
  -webkit-mask-image: {gradient};
  backdrop-filter: blur({index * blurIntensity}px); z-index: {index * 10};"
		></div>
	{/each}
</div>

<!-- {#snippet progressiveBlur({
	direction = 'bottom',
	blurLayers = 8,
	class: _class = '',
	blurIntensity = 0.25
}: ProgressiveBlurProps)}
	{@const layers = Math.max(blurLayers, 2)}
	{@const segmentSize = 1 / (blurLayers + 1)}
	<div class={cn('relative', _class)}>
		{#each { length: layers } as _, index}
			{@const angle = GRADIENT_ANGLES[direction]}
			{@const gradientStops = [
				index * segmentSize,
				(index + 1) * segmentSize,
				(index + 2) * segmentSize,
				(index + 3) * segmentSize
			].map(
				(pos, posIndex) =>
					`rgba(255, 255, 255, ${posIndex === 1 || posIndex === 2 ? 1 : 0}) ${pos * 100}%`
			)}
			{@const gradient = `linear-gradient(${angle}deg, ${gradientStops.join(', ')})`}
			<div
				class="pointer-events-none absolute inset-0 rounded-[inherit]"
				style="mask-image: {gradient};
  -webkit-mask-image: {gradient};
  backdrop-filter: blur({index * blurIntensity}px); z-index: {index * 10};"
			></div>
		{/each}
	</div>
{/snippet} -->

```

###### File: `src/lib/components/ui/progressive-blur/index.ts`

```ts
import ProgressiveBlur from "./progressive-blur.svelte";
export { ProgressiveBlur };

```

---

## Auth Blocks <a name="auth"></a>

### Auth 1 <a name="auth-1"></a>

A premium auth 1 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `auth-1`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/auth-1.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`
  - `https://magicui.design/r/particles.json`

##### Component Source Code

###### File: `registry/blocks/auth/1/auth-page.tsx`

```tsx
import { ChevronLeftIcon } from "lucide-react";
import type React from "react";
import { Logo } from "@/components/logo";
import { Button } from "@/components/ui/button";
// https://magicui.design/docs/components/particles
import { Particles } from "@/components/ui/particles";

export function AuthPage() {
  return (
    <div className="relative w-full md:h-screen md:overflow-hidden">
      <Particles
        className="absolute inset-0"
        color="#666666"
        ease={20}
        quantity={120}
      />
      <div className="relative mx-auto flex min-h-screen max-w-5xl flex-col justify-center px-4">
        <Button asChild className="absolute top-4 left-4" variant="ghost">
          <a href="#">
            <ChevronLeftIcon />
            Home
          </a>
        </Button>

        <div className="mx-auto space-y-4 sm:w-sm">
          <Logo className="h-6" />
          <div className="flex flex-col space-y-1">
            <h1 className="font-bold text-2xl tracking-wide">
              Sign In or Join Now!
            </h1>
            <p className="text-base text-muted-foreground">
              login or create your efferd account.
            </p>
          </div>
          <div className="space-y-2">
            <Button className="w-full" size="lg" type="button">
              <GoogleIcon />
              Continue with Google
            </Button>
            <Button className="w-full" size="lg" type="button">
              <GithubIcon />
              Continue with GitHub
            </Button>
          </div>
          <p className="mt-8 text-muted-foreground text-sm">
            By clicking continue, you agree to our{" "}
            <a
              className="underline underline-offset-4 hover:text-primary"
              href="#"
            >
              Terms of Service
            </a>{" "}
            and{" "}
            <a
              className="underline underline-offset-4 hover:text-primary"
              href="#"
            >
              Privacy Policy
            </a>
            .
          </p>
        </div>
      </div>
    </div>
  );
}

const GoogleIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 24 24" {...props}>
    <g>
      <path d="M12.479,14.265v-3.279h11.049c0.108,0.571,0.164,1.247,0.164,1.979c0,2.46-0.672,5.502-2.84,7.669   C18.744,22.829,16.051,24,12.483,24C5.869,24,0.308,18.613,0.308,12S5.869,0,12.483,0c3.659,0,6.265,1.436,8.223,3.307L18.392,5.62   c-1.404-1.317-3.307-2.341-5.913-2.341C7.65,3.279,3.873,7.171,3.873,12s3.777,8.721,8.606,8.721c3.132,0,4.916-1.258,6.059-2.401   c0.927-0.927,1.537-2.251,1.777-4.059L12.479,14.265z" />
    </g>
  </svg>
);

const GithubIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 1024 1024" {...props}>
    <path
      clipRule="evenodd"
      d="M8 0C3.58 0 0 3.58 0 8C0 11.54 2.29 14.53 5.47 15.59C5.87 15.66 6.02 15.42 6.02 15.21C6.02 15.02 6.01 14.39 6.01 13.72C4 14.09 3.48 13.23 3.32 12.78C3.23 12.55 2.84 11.84 2.5 11.65C2.22 11.5 1.82 11.13 2.49 11.12C3.12 11.11 3.57 11.7 3.72 11.94C4.44 13.15 5.59 12.81 6.05 12.6C6.12 12.08 6.33 11.73 6.56 11.53C4.78 11.33 2.92 10.64 2.92 7.58C2.92 6.71 3.23 5.99 3.74 5.43C3.66 5.23 3.38 4.41 3.82 3.31C3.82 3.31 4.49 3.1 6.02 4.13C6.66 3.95 7.34 3.86 8.02 3.86C8.7 3.86 9.38 3.95 10.02 4.13C11.55 3.09 12.22 3.31 12.22 3.31C12.66 4.41 12.38 5.23 12.3 5.43C12.81 5.99 13.12 6.7 13.12 7.58C13.12 10.65 11.25 11.33 9.47 11.53C9.76 11.78 10.01 12.26 10.01 13.01C10.01 14.08 10 14.94 10 15.21C10 15.42 10.15 15.67 10.55 15.59C13.71 14.53 16 11.53 16 8C16 3.58 12.42 0 8 0Z"
      fill="currentColor"
      fillRule="evenodd"
      transform="scale(64)"
    />
  </svg>
);

```

###### File: `components/logo.tsx`

```tsx
import type React from "react";

export const LogoIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 24 24" {...props}>
    <path
      d="M 15.03125 23.851562 C 14.117188 23.609375 13.417969 23 13 22.101562 C 12.808594 21.691406 12.808594 21.613281 12.808594 18.375 C 12.808594 15.066406 12.808594 15.066406 13.097656 14.484375 C 13.429688 13.8125 13.898438 13.351562 14.585938 13.027344 C 15.074219 12.800781 15.074219 12.800781 18.386719 12.800781 C 21.699219 12.800781 21.699219 12.800781 22.28125 13.089844 C 22.953125 13.421875 23.414062 13.890625 23.738281 14.578125 C 23.964844 15.066406 23.964844 15.066406 23.988281 18.140625 C 24.015625 20.769531 24 21.285156 23.875 21.710938 C 23.5625 22.789062 22.769531 23.582031 21.699219 23.863281 C 20.964844 24.042969 15.746094 24.042969 15.03125 23.851562 Z M 21.679688 22.390625 C 21.863281 22.304688 22.117188 22.085938 22.246094 21.917969 C 22.480469 21.613281 22.480469 21.613281 22.480469 18.375 C 22.480469 15.136719 22.480469 15.136719 22.238281 14.824219 C 21.757812 14.1875 21.792969 14.195312 18.386719 14.195312 C 15.667969 14.195312 15.308594 14.214844 15.066406 14.34375 C 14.691406 14.542969 14.359375 14.960938 14.246094 15.355469 C 14.144531 15.753906 14.132812 20.847656 14.246094 21.328125 C 14.386719 21.9375 14.847656 22.398438 15.441406 22.519531 C 15.625 22.554688 17.027344 22.582031 18.5625 22.574219 C 21.105469 22.554688 21.375 22.539062 21.679688 22.390625 Z M 21.679688 22.390625 "
      opacity="50%"
    />
    <path d="M 2.859375 23.085938 C 2.089844 22.84375 1.324219 22.15625 0.976562 21.398438 C 0.792969 20.996094 0.785156 20.882812 0.785156 17.636719 C 0.785156 14.28125 0.785156 14.28125 1.019531 13.785156 C 1.296875 13.1875 1.855469 12.609375 2.441406 12.324219 C 2.875 12.105469 2.875 12.105469 6.195312 12.078125 C 9.4375 12.054688 9.523438 12.0625 10.03125 12.246094 C 10.71875 12.507812 11.441406 13.21875 11.730469 13.933594 C 11.9375 14.457031 11.9375 14.472656 11.9375 17.636719 C 11.9375 21.136719 11.9375 21.128906 11.371094 21.953125 C 11.03125 22.433594 10.550781 22.800781 9.925781 23.035156 C 9.480469 23.199219 9.261719 23.207031 6.335938 23.199219 C 4.035156 23.199219 3.128906 23.164062 2.859375 23.085938 Z M 2.859375 23.085938 " />
    <path d="M 13.898438 11.695312 C 13.226562 11.4375 12.503906 10.703125 12.25 10.023438 C 12.070312 9.519531 12.058594 9.433594 12.085938 6.195312 C 12.113281 2.929688 12.113281 2.867188 12.3125 2.476562 C 12.636719 1.824219 13.070312 1.386719 13.707031 1.074219 C 14.289062 0.785156 14.289062 0.785156 17.644531 0.785156 C 20.90625 0.785156 21.007812 0.792969 21.410156 0.976562 C 21.984375 1.238281 22.65625 1.890625 22.933594 2.476562 C 23.179688 2.964844 23.179688 2.964844 23.179688 6.316406 C 23.179688 9.667969 23.179688 9.667969 22.890625 10.25 C 22.578125 10.886719 22.140625 11.324219 21.488281 11.644531 C 21.097656 11.84375 21.042969 11.84375 17.734375 11.863281 C 14.492188 11.878906 14.359375 11.878906 13.898438 11.695312 Z M 13.898438 11.695312 " />
    <path d="M 2.214844 11.078125 C 1.324219 10.824219 0.609375 10.207031 0.199219 9.3125 C 0 8.894531 0 8.832031 0 5.574219 C 0 2.265625 0 2.265625 0.234375 1.769531 C 0.53125 1.132812 1.132812 0.535156 1.769531 0.238281 C 2.265625 0.00390625 2.265625 0.00390625 5.578125 0.00390625 C 8.886719 0.00390625 8.886719 0.00390625 9.386719 0.238281 C 10.019531 0.535156 10.621094 1.132812 10.917969 1.769531 C 11.152344 2.265625 11.152344 2.265625 11.152344 5.574219 C 11.152344 8.882812 11.152344 8.882812 10.925781 9.371094 C 10.605469 10.058594 10.144531 10.53125 9.472656 10.859375 C 8.886719 11.148438 8.886719 11.148438 5.75 11.164062 C 3.441406 11.183594 2.507812 11.15625 2.214844 11.078125 Z M 8.898438 9.605469 C 9.238281 9.425781 9.613281 8.988281 9.699219 8.683594 C 9.734375 8.554688 9.757812 7.117188 9.757812 5.488281 C 9.757812 2.179688 9.769531 2.207031 9.132812 1.726562 C 8.820312 1.484375 8.820312 1.484375 5.804688 1.457031 C 4.148438 1.4375 2.65625 1.457031 2.5 1.484375 C 2.34375 1.507812 2.066406 1.683594 1.882812 1.855469 C 1.375 2.335938 1.34375 2.632812 1.421875 5.976562 C 1.480469 8.816406 1.480469 8.816406 1.726562 9.128906 C 2.214844 9.773438 2.316406 9.789062 5.75 9.773438 C 8.285156 9.753906 8.660156 9.738281 8.898438 9.605469 Z M 8.898438 9.605469 " />
  </svg>
);

export const Logo = (props: React.ComponentProps<"svg">) => (
  <svg
    fill="currentColor"
    viewBox="0 0 90 24"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <defs>
      <clipPath id="a">
        <path d="M12.785 12.781h11.172V24H12.785Zm0 0" />
      </clipPath>
      <clipPath id="b">
        <path d="M.008 0h11.148v11.2H.008Zm0 0" />
      </clipPath>
    </defs>
    <g clipPath="url(#a)">
      <path
        d="M15.008 23.855c-.91-.242-1.61-.851-2.028-1.75-.19-.41-.19-.488-.19-3.73 0-3.309 0-3.309.288-3.89a3 3 0 0 1 1.485-1.458c.488-.226.488-.226 3.792-.226 3.31 0 3.31 0 3.887.289.672.332 1.133.8 1.453 1.488.227.488.227.488.25 3.563.028 2.632.012 3.148-.113 3.574-.309 1.078-1.102 1.87-2.168 2.152-.734.18-5.941.18-6.656-.012m6.637-1.46c.18-.086.433-.305.562-.473.234-.305.234-.305.234-3.547 0-3.238 0-3.238-.242-3.55-.476-.637-.441-.63-3.844-.63-2.71 0-3.07.02-3.312.149-.375.199-.707.617-.82 1.011-.098.399-.11 5.497 0 5.977.14.61.601 1.07 1.195 1.191.184.036 1.582.063 3.113.055 2.54-.02 2.809-.035 3.114-.183m0 0"
        opacity="50%"
      />
    </g>
    <path d="M2.86 23.09c-.766-.242-1.532-.93-1.88-1.688-.183-.402-.187-.515-.187-3.765 0-3.356 0-3.356.23-3.852.278-.598.836-1.176 1.422-1.46.43-.22.43-.22 3.746-.247 3.235-.023 3.32-.015 3.829.168.683.262 1.406.973 1.695 1.688.207.523.207.539.207 3.703 0 3.504 0 3.496-.567 4.32-.34.48-.82.848-1.44 1.082-.446.164-.665.172-3.583.164-2.297 0-3.203-.035-3.473-.113M13.879 11.695c-.672-.258-1.395-.992-1.645-1.672-.18-.503-.191-.59-.164-3.832.028-3.265.028-3.328.223-3.718.324-.653.758-1.09 1.395-1.403.578-.289.578-.289 3.93-.289 3.253 0 3.355.008 3.757.192.57.261 1.242.914 1.52 1.5.246.488.246.488.246 3.84 0 3.355 0 3.355-.29 3.937a2.9 2.9 0 0 1-1.398 1.395c-.39.199-.445.199-3.746.218-3.238.016-3.371.016-3.828-.168m0 0" />
    <g clipPath="url(#b)">
      <path d="M2.219 11.078c-.89-.254-1.602-.871-2.012-1.765-.2-.418-.2-.481-.2-3.743 0-3.308 0-3.308.235-3.804A3.34 3.34 0 0 1 1.773.234C2.27 0 2.27 0 5.574 0c3.301 0 3.301 0 3.801.234.633.297 1.23.895 1.527 1.532.235.496.235.496.235 3.804 0 3.313 0 3.313-.227 3.801-.32.688-.777 1.16-1.45 1.488-.585.29-.585.29-3.714.305-2.305.02-3.234-.008-3.527-.086m6.668-1.473c.34-.18.715-.617.8-.921.036-.13.06-1.567.06-3.2 0-3.308.01-3.28-.626-3.761-.312-.243-.312-.243-3.32-.27-1.653-.02-3.145 0-3.297.027-.156.024-.434.2-.617.372-.508.48-.54.777-.461 4.12.058 2.844.058 2.844.304 3.157.489.644.59.66 4.016.644 2.531-.02 2.902-.035 3.14-.168m0 0" />
    </g>
    <path d="m33.688 17.254-1.622-2.125 3.325-2.57a1.6 1.6 0 0 0-.696-.434 3 3 0 0 0-.957-.145q-.626.001-1.148.266a2.75 2.75 0 0 0-.903.707q-.379.445-.59 1.047c-.14.402-.206.84-.206 1.313q0 .655.246 1.257c.164.403.394.758.68 1.063q.432.46 1.019.734a3 3 0 0 0 1.27.278c.84 0 1.57-.243 2.199-.723q.942-.72 1.152-2.008l3.375.367a7.6 7.6 0 0 1-.879 2.387q-.614 1.053-1.504 1.797-.891.75-2.027 1.14a7.5 7.5 0 0 1-2.445.395 6.6 6.6 0 0 1-2.606-.512 6.1 6.1 0 0 1-2.039-1.402 6.3 6.3 0 0 1-1.32-2.11q-.474-1.223-.473-2.663-.001-1.44.473-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.177-.499 2.59-.5 2.195 0 3.777.973c1.059.644 1.855 1.582 2.398 2.804ZM47.973 6.684H46.69q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.254-4.239q1.26-1.296 3.48-1.297h1.23ZM54.723 6.684H53.44q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.258-4.239q1.255-1.296 3.476-1.297h1.23ZM61.21 17.254l-1.62-2.125 3.324-2.57a1.6 1.6 0 0 0-.695-.434 3 3 0 0 0-.953-.145q-.632.001-1.153.266-.52.26-.902.707-.38.445-.59 1.047-.206.603-.207 1.313 0 .655.246 1.257.252.605.68 1.063.433.46 1.023.734.586.277 1.266.278c.84 0 1.57-.243 2.2-.723q.94-.72 1.151-2.008l3.375.367a7.6 7.6 0 0 1-.878 2.387q-.614 1.053-1.504 1.797-.891.75-2.028 1.14A7.5 7.5 0 0 1 61.5 22a6.6 6.6 0 0 1-2.605-.512 6.1 6.1 0 0 1-2.04-1.402 6.3 6.3 0 0 1-1.32-2.11q-.473-1.223-.472-2.663-.001-1.44.472-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.178-.499 2.59-.5 2.199 0 3.781.973c1.055.644 1.852 1.582 2.395 2.804ZM76.805 12.348h-.758q-1.413 0-2.121.855c-.469.567-.703 1.29-.703 2.16v6.32h-3.559V8.942h3.164v2.41h.05c.317-.855.75-1.5 1.31-1.925q.838-.645 1.964-.645h.653ZM89.992 3.328v12.063c0 .89-.152 1.742-.457 2.543a6.6 6.6 0 0 1-1.285 2.113 6 6 0 0 1-1.988 1.43q-1.161.522-2.551.523a5.9 5.9 0 0 1-2.496-.523 6.1 6.1 0 0 1-1.965-1.43 6.8 6.8 0 0 1-1.293-2.125 7.3 7.3 0 0 1-.473-2.637q0-1.335.461-2.555a6.5 6.5 0 0 1 1.282-2.125 6.3 6.3 0 0 1 1.921-1.44 5.4 5.4 0 0 1 2.407-.54q.527 0 1.007.117.487.122 1.008.25v3.54a5 5 0 0 0-.851-.286 4 4 0 0 0-.953-.105q-1.285 0-2.106.879-.825.876-.824 2.293 0 1.417.809 2.296a2.65 2.65 0 0 0 2.015.875q1.359-.001 2.172-.875.811-.881.813-2.218V3.328Zm0 0" />
  </svg>
);

```

#### Svelte Implementation

- **Registry Key**: `auth-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/auth-one.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:particles`

##### Component Source Code

###### File: `src/lib/components/efferd/auth/auth-one.svelte`

```svelte
<script lang="ts">
	import { ChevronLeftIcon } from "@lucide/svelte";
	import { Particles } from "$lib/components/magic/particles";
	import { Button } from "$lib/components/ui/button";
	import GithubLogo from "$lib/svgs/github.svelte";
	import GoogleLogo from "$lib/svgs/google.svelte";
	import Logo from "$lib/svgs/logo.svelte";
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";

	type AuthOneProps = HTMLAttributes<HTMLDivElement> & {
		class?: string;
		homeHref?: string;
		termsHref?: string;
		privacyHref?: string;
	};

	let {
		class: className = "",
		homeHref = "/",
		termsHref = "/",
		privacyHref = "/",
		...restProps
	}: AuthOneProps = $props();
</script>

<div class={cn("relative w-full md:h-screen md:overflow-hidden", className)} {...restProps}>
	<Particles class="absolute inset-0" color="#666666" ease={20} quantity={120} />

	<div class="relative mx-auto flex min-h-screen max-w-5xl flex-col justify-center px-8">
		<Button class="absolute top-4 left-4" href={homeHref} variant="ghost">
			<ChevronLeftIcon data-icon="inline-start" />
			Home
		</Button>

		<div class="mx-auto w-full max-w-sm space-y-4">
			<Logo class="h-5 w-auto" />

			<div class="flex flex-col space-y-1">
				<h1 class="text-2xl font-bold tracking-wide">Sign In or Join Now!</h1>
				<p class="text-base text-muted-foreground">login or create your efferd account.</p>
			</div>

			<div class="space-y-2">
				<Button class="w-full" type="button">
					<GoogleLogo data-icon="inline-start" />
					Continue with Google
				</Button>

				<Button class="w-full" type="button">
					<GithubLogo data-icon="inline-start" />
					Continue with GitHub
				</Button>
			</div>

			<p class="mt-8 text-sm text-muted-foreground">
				By clicking continue, you agree to our
				<a class="underline underline-offset-4 hover:text-primary" href={termsHref}
					>Terms of Service</a
				>
				and
				<a class="underline underline-offset-4 hover:text-primary" href={privacyHref}
					>Privacy Policy</a
				>.
			</p>
		</div>
	</div>
</div>

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

###### File: `src/lib/svgs/google.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg fill="currentColor" viewBox="0 0 24 24" {...props}>
	<g>
		<path
			d="M12.479,14.265v-3.279h11.049c0.108,0.571,0.164,1.247,0.164,1.979c0,2.46-0.672,5.502-2.84,7.669C18.744,22.829,16.051,24,12.483,24C5.869,24,0.308,18.613,0.308,12S5.869,0,12.483,0c3.659,0,6.265,1.436,8.223,3.307L18.392,5.62c-1.404-1.317-3.307-2.341-5.913-2.341C7.65,3.279,3.873,7.171,3.873,12s3.777,8.721,8.606,8.721c3.132,0,4.916-1.258,6.059-2.401c0.927-0.927,1.537-2.251,1.777-4.059L12.479,14.265z"
		/>
	</g>
</svg>

```

###### File: `src/lib/svgs/github.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg fill="currentColor" viewBox="0 0 1024 1024" {...props}>
	<path
		clip-rule="evenodd"
		d="M8 0C3.58 0 0 3.58 0 8C0 11.54 2.29 14.53 5.47 15.59C5.87 15.66 6.02 15.42 6.02 15.21C6.02 15.02 6.01 14.39 6.01 13.72C4 14.09 3.48 13.23 3.32 12.78C3.23 12.55 2.84 11.84 2.5 11.65C2.22 11.5 1.82 11.13 2.49 11.12C3.12 11.11 3.57 11.7 3.72 11.94C4.44 13.15 5.59 12.81 6.05 12.6C6.12 12.08 6.33 11.73 6.56 11.53C4.78 11.33 2.92 10.64 2.92 7.58C2.92 6.71 3.23 5.99 3.74 5.43C3.66 5.23 3.38 4.41 3.82 3.31C3.82 3.31 4.49 3.1 6.02 4.13C6.66 3.95 7.34 3.86 8.02 3.86C8.7 3.86 9.38 3.95 10.02 4.13C11.55 3.09 12.22 3.31 12.22 3.31C12.66 4.41 12.38 5.23 12.3 5.43C12.81 5.99 13.12 6.7 13.12 7.58C13.12 10.65 11.25 11.33 9.47 11.53C9.76 11.78 10.01 12.26 10.01 13.01C10.01 14.08 10 14.94 10 15.21C10 15.42 10.15 15.67 10.55 15.59C13.71 14.53 16 11.53 16 8C16 3.58 12.42 0 8 0Z"
		fill="currentColor"
		fill-rule="evenodd"
		transform="scale(64)"
	/>
</svg>

```

---

### Auth 2 <a name="auth-2"></a>

A premium auth 2 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `auth-2`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/auth-2.json
```

##### Dependencies

- **NPM Packages**:
  - `motion`
- **Registry Dependencies**:
  - `button`
  - `input-group`

##### Component Source Code

###### File: `registry/blocks/auth/2/auth-page.tsx`

```tsx
"use client";

import { AtSignIcon, ChevronLeftIcon } from "lucide-react";
import type React from "react";
import { Logo } from "@/components/logo";
import { Button } from "@/components/ui/button";
import {
  InputGroup,
  InputGroupAddon,
  InputGroupInput,
} from "@/components/ui/input-group";
import { FloatingPaths } from "./floating-paths";

export function AuthPage() {
  return (
    <main className="relative md:h-screen md:overflow-hidden lg:grid lg:grid-cols-2">
      <div className="relative hidden h-full flex-col border-r bg-secondary p-10 lg:flex dark:bg-secondary/20">
        <div className="absolute inset-0 bg-gradient-to-b from-transparent via-transparent to-background" />
        <Logo className="mr-auto h-5" />

        <div className="z-10 mt-auto">
          <blockquote className="space-y-2">
            <p className="text-xl">
              &ldquo;This Platform has helped me to save time and serve my
              clients faster than ever before.&rdquo;
            </p>
            <footer className="font-mono font-semibold text-sm">
              ~ Ali Hassan
            </footer>
          </blockquote>
        </div>
        <div className="absolute inset-0">
          <FloatingPaths position={1} />
          <FloatingPaths position={-1} />
        </div>
      </div>
      <div className="relative flex min-h-screen flex-col justify-center p-4">
        <div
          aria-hidden
          className="-z-10 absolute inset-0 isolate opacity-60 contain-strict"
        >
          <div className="-translate-y-87.5 absolute top-0 right-0 h-320 w-140 rounded-full bg-[radial-gradient(68.54%_68.72%_at_55.02%_31.46%,--theme(--color-foreground/.06)_0,hsla(0,0%,55%,.02)_50%,--theme(--color-foreground/.01)_80%)]" />
          <div className="absolute top-0 right-0 h-320 w-60 rounded-full bg-[radial-gradient(50%_50%_at_50%_50%,--theme(--color-foreground/.04)_0,--theme(--color-foreground/.01)_80%,transparent_100%)] [translate:5%_-50%]" />
          <div className="-translate-y-87.5 absolute top-0 right-0 h-320 w-60 rounded-full bg-[radial-gradient(50%_50%_at_50%_50%,--theme(--color-foreground/.04)_0,--theme(--color-foreground/.01)_80%,transparent_100%)]" />
        </div>
        <Button asChild className="absolute top-7 left-5" variant="ghost">
          <a href="#">
            <ChevronLeftIcon />
            Home
          </a>
        </Button>
        <div className="mx-auto space-y-4 sm:w-sm">
          <Logo className="h-5 lg:hidden" />
          <div className="flex flex-col space-y-1">
            <h1 className="font-bold text-2xl tracking-wide">
              Sign In or Join Now!
            </h1>
            <p className="text-base text-muted-foreground">
              login or create your efferd account.
            </p>
          </div>
          <div className="space-y-2">
            <Button className="w-full" size="lg" type="button">
              <GoogleIcon />
              Continue with Google
            </Button>
            <Button className="w-full" size="lg" type="button">
              <AppleIcon />
              Continue with Apple
            </Button>
            <Button className="w-full" size="lg" type="button">
              <GithubIcon />
              Continue with GitHub
            </Button>
          </div>

          <div className="flex w-full items-center justify-center">
            <div className="h-px w-full bg-border" />
            <span className="px-2 text-muted-foreground text-xs">OR</span>
            <div className="h-px w-full bg-border" />
          </div>

          <form className="space-y-2">
            <p className="text-start text-muted-foreground text-xs">
              Enter your email address to sign in or create an account
            </p>
            <InputGroup>
              <InputGroupInput
                placeholder="your.email@example.com"
                type="email"
              />
              <InputGroupAddon>
                <AtSignIcon />
              </InputGroupAddon>
            </InputGroup>

            <Button className="w-full" type="button">
              Continue With Email
            </Button>
          </form>
          <p className="mt-8 text-muted-foreground text-sm">
            By clicking continue, you agree to our{" "}
            <a
              className="underline underline-offset-4 hover:text-primary"
              href="#"
            >
              Terms of Service
            </a>{" "}
            and{" "}
            <a
              className="underline underline-offset-4 hover:text-primary"
              href="#"
            >
              Privacy Policy
            </a>
            .
          </p>
        </div>
      </div>
    </main>
  );
}

const GoogleIcon = (props: React.ComponentProps<"svg">) => (
  <svg
    fill="currentColor"
    viewBox="0 0 24 24"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <g>
      <path d="M12.479,14.265v-3.279h11.049c0.108,0.571,0.164,1.247,0.164,1.979c0,2.46-0.672,5.502-2.84,7.669   C18.744,22.829,16.051,24,12.483,24C5.869,24,0.308,18.613,0.308,12S5.869,0,12.483,0c3.659,0,6.265,1.436,8.223,3.307L18.392,5.62   c-1.404-1.317-3.307-2.341-5.913-2.341C7.65,3.279,3.873,7.171,3.873,12s3.777,8.721,8.606,8.721c3.132,0,4.916-1.258,6.059-2.401   c0.927-0.927,1.537-2.251,1.777-4.059L12.479,14.265z" />
    </g>
  </svg>
);

function AppleIcon({
  fill = "currentColor",
  ...props
}: React.ComponentProps<"svg">) {
  return (
    <svg fill={fill} viewBox="0 0 24 24" {...props}>
      <g id="_Group_2">
        <g id="_Group_3">
          <path
            d="M18.546,12.763c0.024-1.87,1.004-3.597,2.597-4.576c-1.009-1.442-2.64-2.323-4.399-2.378    c-1.851-0.194-3.645,1.107-4.588,1.107c-0.961,0-2.413-1.088-3.977-1.056C6.122,5.927,4.25,7.068,3.249,8.867    c-2.131,3.69-0.542,9.114,1.5,12.097c1.022,1.461,2.215,3.092,3.778,3.035c1.529-0.063,2.1-0.975,3.945-0.975    c1.828,0,2.364,0.975,3.958,0.938c1.64-0.027,2.674-1.467,3.66-2.942c0.734-1.041,1.299-2.191,1.673-3.408    C19.815,16.788,18.548,14.879,18.546,12.763z"
            id="_Path_"
          />
          <path
            d="M15.535,3.847C16.429,2.773,16.87,1.393,16.763,0c-1.366,0.144-2.629,0.797-3.535,1.829    c-0.895,1.019-1.349,2.351-1.261,3.705C13.352,5.548,14.667,4.926,15.535,3.847z"
            id="_Path_2"
          />
        </g>
      </g>
    </svg>
  );
}

const GithubIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 1024 1024" {...props}>
    <path
      clipRule="evenodd"
      d="M8 0C3.58 0 0 3.58 0 8C0 11.54 2.29 14.53 5.47 15.59C5.87 15.66 6.02 15.42 6.02 15.21C6.02 15.02 6.01 14.39 6.01 13.72C4 14.09 3.48 13.23 3.32 12.78C3.23 12.55 2.84 11.84 2.5 11.65C2.22 11.5 1.82 11.13 2.49 11.12C3.12 11.11 3.57 11.7 3.72 11.94C4.44 13.15 5.59 12.81 6.05 12.6C6.12 12.08 6.33 11.73 6.56 11.53C4.78 11.33 2.92 10.64 2.92 7.58C2.92 6.71 3.23 5.99 3.74 5.43C3.66 5.23 3.38 4.41 3.82 3.31C3.82 3.31 4.49 3.1 6.02 4.13C6.66 3.95 7.34 3.86 8.02 3.86C8.7 3.86 9.38 3.95 10.02 4.13C11.55 3.09 12.22 3.31 12.22 3.31C12.66 4.41 12.38 5.23 12.3 5.43C12.81 5.99 13.12 6.7 13.12 7.58C13.12 10.65 11.25 11.33 9.47 11.53C9.76 11.78 10.01 12.26 10.01 13.01C10.01 14.08 10 14.94 10 15.21C10 15.42 10.15 15.67 10.55 15.59C13.71 14.53 16 11.53 16 8C16 3.58 12.42 0 8 0Z"
      fill="currentColor"
      fillRule="evenodd"
      transform="scale(64)"
    />
  </svg>
);

```

###### File: `registry/blocks/auth/2/floating-paths.tsx`

```tsx
import { motion } from "motion/react";

export function FloatingPaths({ position }: { position: number }) {
  const paths = Array.from({ length: 36 }, (_, i) => ({
    id: i,
    d: `M-${380 - i * 5 * position} -${189 + i * 6}C-${
      380 - i * 5 * position
    } -${189 + i * 6} -${312 - i * 5 * position} ${216 - i * 6} ${
      152 - i * 5 * position
    } ${343 - i * 6}C${616 - i * 5 * position} ${470 - i * 6} ${
      684 - i * 5 * position
    } ${875 - i * 6} ${684 - i * 5 * position} ${875 - i * 6}`,
    color: `rgba(15,23,42,${0.1 + i * 0.03})`,
    width: 0.5 + i * 0.03,
  }));

  return (
    <div className="pointer-events-none absolute inset-0">
      <svg
        className="h-full w-full text-slate-950 dark:text-white"
        fill="none"
        viewBox="0 0 696 316"
      >
        <title>Background Paths</title>
        {paths.map((path) => (
          <motion.path
            animate={{
              pathLength: 1,
              opacity: [0.3, 0.6, 0.3],
              pathOffset: [0, 1, 0],
            }}
            d={path.d}
            initial={{ pathLength: 0.3, opacity: 0.6 }}
            key={path.id}
            stroke="currentColor"
            strokeOpacity={0.1 + path.id * 0.03}
            strokeWidth={path.width}
            transition={{
              duration: 20 + Math.random() * 10,
              repeat: Number.POSITIVE_INFINITY,
              ease: "linear",
            }}
          />
        ))}
      </svg>
    </div>
  );
}

```

###### File: `components/logo.tsx`

```tsx
import type React from "react";

export const LogoIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 24 24" {...props}>
    <path
      d="M 15.03125 23.851562 C 14.117188 23.609375 13.417969 23 13 22.101562 C 12.808594 21.691406 12.808594 21.613281 12.808594 18.375 C 12.808594 15.066406 12.808594 15.066406 13.097656 14.484375 C 13.429688 13.8125 13.898438 13.351562 14.585938 13.027344 C 15.074219 12.800781 15.074219 12.800781 18.386719 12.800781 C 21.699219 12.800781 21.699219 12.800781 22.28125 13.089844 C 22.953125 13.421875 23.414062 13.890625 23.738281 14.578125 C 23.964844 15.066406 23.964844 15.066406 23.988281 18.140625 C 24.015625 20.769531 24 21.285156 23.875 21.710938 C 23.5625 22.789062 22.769531 23.582031 21.699219 23.863281 C 20.964844 24.042969 15.746094 24.042969 15.03125 23.851562 Z M 21.679688 22.390625 C 21.863281 22.304688 22.117188 22.085938 22.246094 21.917969 C 22.480469 21.613281 22.480469 21.613281 22.480469 18.375 C 22.480469 15.136719 22.480469 15.136719 22.238281 14.824219 C 21.757812 14.1875 21.792969 14.195312 18.386719 14.195312 C 15.667969 14.195312 15.308594 14.214844 15.066406 14.34375 C 14.691406 14.542969 14.359375 14.960938 14.246094 15.355469 C 14.144531 15.753906 14.132812 20.847656 14.246094 21.328125 C 14.386719 21.9375 14.847656 22.398438 15.441406 22.519531 C 15.625 22.554688 17.027344 22.582031 18.5625 22.574219 C 21.105469 22.554688 21.375 22.539062 21.679688 22.390625 Z M 21.679688 22.390625 "
      opacity="50%"
    />
    <path d="M 2.859375 23.085938 C 2.089844 22.84375 1.324219 22.15625 0.976562 21.398438 C 0.792969 20.996094 0.785156 20.882812 0.785156 17.636719 C 0.785156 14.28125 0.785156 14.28125 1.019531 13.785156 C 1.296875 13.1875 1.855469 12.609375 2.441406 12.324219 C 2.875 12.105469 2.875 12.105469 6.195312 12.078125 C 9.4375 12.054688 9.523438 12.0625 10.03125 12.246094 C 10.71875 12.507812 11.441406 13.21875 11.730469 13.933594 C 11.9375 14.457031 11.9375 14.472656 11.9375 17.636719 C 11.9375 21.136719 11.9375 21.128906 11.371094 21.953125 C 11.03125 22.433594 10.550781 22.800781 9.925781 23.035156 C 9.480469 23.199219 9.261719 23.207031 6.335938 23.199219 C 4.035156 23.199219 3.128906 23.164062 2.859375 23.085938 Z M 2.859375 23.085938 " />
    <path d="M 13.898438 11.695312 C 13.226562 11.4375 12.503906 10.703125 12.25 10.023438 C 12.070312 9.519531 12.058594 9.433594 12.085938 6.195312 C 12.113281 2.929688 12.113281 2.867188 12.3125 2.476562 C 12.636719 1.824219 13.070312 1.386719 13.707031 1.074219 C 14.289062 0.785156 14.289062 0.785156 17.644531 0.785156 C 20.90625 0.785156 21.007812 0.792969 21.410156 0.976562 C 21.984375 1.238281 22.65625 1.890625 22.933594 2.476562 C 23.179688 2.964844 23.179688 2.964844 23.179688 6.316406 C 23.179688 9.667969 23.179688 9.667969 22.890625 10.25 C 22.578125 10.886719 22.140625 11.324219 21.488281 11.644531 C 21.097656 11.84375 21.042969 11.84375 17.734375 11.863281 C 14.492188 11.878906 14.359375 11.878906 13.898438 11.695312 Z M 13.898438 11.695312 " />
    <path d="M 2.214844 11.078125 C 1.324219 10.824219 0.609375 10.207031 0.199219 9.3125 C 0 8.894531 0 8.832031 0 5.574219 C 0 2.265625 0 2.265625 0.234375 1.769531 C 0.53125 1.132812 1.132812 0.535156 1.769531 0.238281 C 2.265625 0.00390625 2.265625 0.00390625 5.578125 0.00390625 C 8.886719 0.00390625 8.886719 0.00390625 9.386719 0.238281 C 10.019531 0.535156 10.621094 1.132812 10.917969 1.769531 C 11.152344 2.265625 11.152344 2.265625 11.152344 5.574219 C 11.152344 8.882812 11.152344 8.882812 10.925781 9.371094 C 10.605469 10.058594 10.144531 10.53125 9.472656 10.859375 C 8.886719 11.148438 8.886719 11.148438 5.75 11.164062 C 3.441406 11.183594 2.507812 11.15625 2.214844 11.078125 Z M 8.898438 9.605469 C 9.238281 9.425781 9.613281 8.988281 9.699219 8.683594 C 9.734375 8.554688 9.757812 7.117188 9.757812 5.488281 C 9.757812 2.179688 9.769531 2.207031 9.132812 1.726562 C 8.820312 1.484375 8.820312 1.484375 5.804688 1.457031 C 4.148438 1.4375 2.65625 1.457031 2.5 1.484375 C 2.34375 1.507812 2.066406 1.683594 1.882812 1.855469 C 1.375 2.335938 1.34375 2.632812 1.421875 5.976562 C 1.480469 8.816406 1.480469 8.816406 1.726562 9.128906 C 2.214844 9.773438 2.316406 9.789062 5.75 9.773438 C 8.285156 9.753906 8.660156 9.738281 8.898438 9.605469 Z M 8.898438 9.605469 " />
  </svg>
);

export const Logo = (props: React.ComponentProps<"svg">) => (
  <svg
    fill="currentColor"
    viewBox="0 0 90 24"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <defs>
      <clipPath id="a">
        <path d="M12.785 12.781h11.172V24H12.785Zm0 0" />
      </clipPath>
      <clipPath id="b">
        <path d="M.008 0h11.148v11.2H.008Zm0 0" />
      </clipPath>
    </defs>
    <g clipPath="url(#a)">
      <path
        d="M15.008 23.855c-.91-.242-1.61-.851-2.028-1.75-.19-.41-.19-.488-.19-3.73 0-3.309 0-3.309.288-3.89a3 3 0 0 1 1.485-1.458c.488-.226.488-.226 3.792-.226 3.31 0 3.31 0 3.887.289.672.332 1.133.8 1.453 1.488.227.488.227.488.25 3.563.028 2.632.012 3.148-.113 3.574-.309 1.078-1.102 1.87-2.168 2.152-.734.18-5.941.18-6.656-.012m6.637-1.46c.18-.086.433-.305.562-.473.234-.305.234-.305.234-3.547 0-3.238 0-3.238-.242-3.55-.476-.637-.441-.63-3.844-.63-2.71 0-3.07.02-3.312.149-.375.199-.707.617-.82 1.011-.098.399-.11 5.497 0 5.977.14.61.601 1.07 1.195 1.191.184.036 1.582.063 3.113.055 2.54-.02 2.809-.035 3.114-.183m0 0"
        opacity="50%"
      />
    </g>
    <path d="M2.86 23.09c-.766-.242-1.532-.93-1.88-1.688-.183-.402-.187-.515-.187-3.765 0-3.356 0-3.356.23-3.852.278-.598.836-1.176 1.422-1.46.43-.22.43-.22 3.746-.247 3.235-.023 3.32-.015 3.829.168.683.262 1.406.973 1.695 1.688.207.523.207.539.207 3.703 0 3.504 0 3.496-.567 4.32-.34.48-.82.848-1.44 1.082-.446.164-.665.172-3.583.164-2.297 0-3.203-.035-3.473-.113M13.879 11.695c-.672-.258-1.395-.992-1.645-1.672-.18-.503-.191-.59-.164-3.832.028-3.265.028-3.328.223-3.718.324-.653.758-1.09 1.395-1.403.578-.289.578-.289 3.93-.289 3.253 0 3.355.008 3.757.192.57.261 1.242.914 1.52 1.5.246.488.246.488.246 3.84 0 3.355 0 3.355-.29 3.937a2.9 2.9 0 0 1-1.398 1.395c-.39.199-.445.199-3.746.218-3.238.016-3.371.016-3.828-.168m0 0" />
    <g clipPath="url(#b)">
      <path d="M2.219 11.078c-.89-.254-1.602-.871-2.012-1.765-.2-.418-.2-.481-.2-3.743 0-3.308 0-3.308.235-3.804A3.34 3.34 0 0 1 1.773.234C2.27 0 2.27 0 5.574 0c3.301 0 3.301 0 3.801.234.633.297 1.23.895 1.527 1.532.235.496.235.496.235 3.804 0 3.313 0 3.313-.227 3.801-.32.688-.777 1.16-1.45 1.488-.585.29-.585.29-3.714.305-2.305.02-3.234-.008-3.527-.086m6.668-1.473c.34-.18.715-.617.8-.921.036-.13.06-1.567.06-3.2 0-3.308.01-3.28-.626-3.761-.312-.243-.312-.243-3.32-.27-1.653-.02-3.145 0-3.297.027-.156.024-.434.2-.617.372-.508.48-.54.777-.461 4.12.058 2.844.058 2.844.304 3.157.489.644.59.66 4.016.644 2.531-.02 2.902-.035 3.14-.168m0 0" />
    </g>
    <path d="m33.688 17.254-1.622-2.125 3.325-2.57a1.6 1.6 0 0 0-.696-.434 3 3 0 0 0-.957-.145q-.626.001-1.148.266a2.75 2.75 0 0 0-.903.707q-.379.445-.59 1.047c-.14.402-.206.84-.206 1.313q0 .655.246 1.257c.164.403.394.758.68 1.063q.432.46 1.019.734a3 3 0 0 0 1.27.278c.84 0 1.57-.243 2.199-.723q.942-.72 1.152-2.008l3.375.367a7.6 7.6 0 0 1-.879 2.387q-.614 1.053-1.504 1.797-.891.75-2.027 1.14a7.5 7.5 0 0 1-2.445.395 6.6 6.6 0 0 1-2.606-.512 6.1 6.1 0 0 1-2.039-1.402 6.3 6.3 0 0 1-1.32-2.11q-.474-1.223-.473-2.663-.001-1.44.473-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.177-.499 2.59-.5 2.195 0 3.777.973c1.059.644 1.855 1.582 2.398 2.804ZM47.973 6.684H46.69q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.254-4.239q1.26-1.296 3.48-1.297h1.23ZM54.723 6.684H53.44q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.258-4.239q1.255-1.296 3.476-1.297h1.23ZM61.21 17.254l-1.62-2.125 3.324-2.57a1.6 1.6 0 0 0-.695-.434 3 3 0 0 0-.953-.145q-.632.001-1.153.266-.52.26-.902.707-.38.445-.59 1.047-.206.603-.207 1.313 0 .655.246 1.257.252.605.68 1.063.433.46 1.023.734.586.277 1.266.278c.84 0 1.57-.243 2.2-.723q.94-.72 1.151-2.008l3.375.367a7.6 7.6 0 0 1-.878 2.387q-.614 1.053-1.504 1.797-.891.75-2.028 1.14A7.5 7.5 0 0 1 61.5 22a6.6 6.6 0 0 1-2.605-.512 6.1 6.1 0 0 1-2.04-1.402 6.3 6.3 0 0 1-1.32-2.11q-.473-1.223-.472-2.663-.001-1.44.472-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.178-.499 2.59-.5 2.199 0 3.781.973c1.055.644 1.852 1.582 2.395 2.804ZM76.805 12.348h-.758q-1.413 0-2.121.855c-.469.567-.703 1.29-.703 2.16v6.32h-3.559V8.942h3.164v2.41h.05c.317-.855.75-1.5 1.31-1.925q.838-.645 1.964-.645h.653ZM89.992 3.328v12.063c0 .89-.152 1.742-.457 2.543a6.6 6.6 0 0 1-1.285 2.113 6 6 0 0 1-1.988 1.43q-1.161.522-2.551.523a5.9 5.9 0 0 1-2.496-.523 6.1 6.1 0 0 1-1.965-1.43 6.8 6.8 0 0 1-1.293-2.125 7.3 7.3 0 0 1-.473-2.637q0-1.335.461-2.555a6.5 6.5 0 0 1 1.282-2.125 6.3 6.3 0 0 1 1.921-1.44 5.4 5.4 0 0 1 2.407-.54q.527 0 1.007.117.487.122 1.008.25v3.54a5 5 0 0 0-.851-.286 4 4 0 0 0-.953-.105q-1.285 0-2.106.879-.825.876-.824 2.293 0 1.417.809 2.296a2.65 2.65 0 0 0 2.015.875q1.359-.001 2.172-.875.811-.881.813-2.218V3.328Zm0 0" />
  </svg>
);

```

#### Svelte Implementation

- **Registry Key**: `auth-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/auth-two.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:auth-divider`
  - `local:decor-icon`
  - `input-group`

##### Component Source Code

###### File: `src/lib/components/efferd/auth/auth-two.svelte`

```svelte
<script lang="ts">
	import { AtSignIcon } from "@lucide/svelte";
	import { Button } from "$lib/components/ui/button";
	import { AuthDivider } from "$lib/components/ui/auth-divider";
	import { DecorIcon } from "$lib/components/ui/decor-icon";
	import { InputGroup, InputGroupAddon, InputGroupInput } from "$lib/components/ui/input-group";
	import GithubLogo from "$lib/svgs/github.svelte";
	import GoogleLogo from "$lib/svgs/google.svelte";
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";

	type AuthTwoProps = HTMLAttributes<HTMLDivElement> & {
		class?: string;
		termsHref?: string;
		privacyHref?: string;
		emailPlaceholder?: string;
	};

	let {
		class: className = "",
		termsHref = "/",
		privacyHref = "/",
		emailPlaceholder = "your.email@example.com",
		...restProps
	}: AuthTwoProps = $props();
</script>

<div
	class={cn(
		"relative flex h-screen w-full items-center justify-center overflow-hidden px-6 md:px-8",
		className
	)}
	{...restProps}
>
	<div
		class={cn(
			"relative flex w-full max-w-sm flex-col justify-between p-6 md:p-8",
			"dark:bg-[radial-gradient(50%_80%_at_20%_0%,--theme(--color-foreground/.1),transparent)]"
		)}
	>
		<div class="absolute -inset-y-6 -left-px w-px bg-border"></div>
		<div class="absolute -inset-y-6 -right-px w-px bg-border"></div>
		<div class="absolute -inset-x-6 -top-px h-px bg-border"></div>
		<div class="absolute -inset-x-6 -bottom-px h-px bg-border"></div>

		<DecorIcon position="top-left" />
		<DecorIcon position="bottom-right" />

		<div class="w-full max-w-sm animate-in space-y-8">
			<div class="flex flex-col space-y-1">
				<h1 class="text-2xl font-bold tracking-wide">Join Now!</h1>
				<p class="text-base text-muted-foreground">Login or create your efferd account.</p>
			</div>

			<div class="space-y-4">
				<form class="space-y-2">
					<InputGroup>
						<InputGroupInput placeholder={emailPlaceholder} type="email" />
						<InputGroupAddon align="inline-start">
							<AtSignIcon />
						</InputGroupAddon>
					</InputGroup>

					<Button class="w-full" size="sm" type="button">Continue With Email</Button>
				</form>

				<AuthDivider>OR</AuthDivider>

				<div class="grid grid-cols-2 gap-2">
					<Button class="w-full" type="button" variant="outline">
						<GoogleLogo data-icon="inline-start" />
						Google
					</Button>

					<Button class="w-full" type="button" variant="outline">
						<GithubLogo data-icon="inline-start" />
						GitHub
					</Button>
				</div>
			</div>

			<p class="text-sm text-muted-foreground">
				By clicking continue, you agree to our
				<a class="underline underline-offset-4 hover:text-primary" href={termsHref}
					>Terms of Service</a
				>
				and
				<a class="underline underline-offset-4 hover:text-primary" href={privacyHref}
					>Privacy Policy</a
				>.
			</p>
		</div>
	</div>
</div>

```

###### File: `src/lib/svgs/google.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg fill="currentColor" viewBox="0 0 24 24" {...props}>
	<g>
		<path
			d="M12.479,14.265v-3.279h11.049c0.108,0.571,0.164,1.247,0.164,1.979c0,2.46-0.672,5.502-2.84,7.669C18.744,22.829,16.051,24,12.483,24C5.869,24,0.308,18.613,0.308,12S5.869,0,12.483,0c3.659,0,6.265,1.436,8.223,3.307L18.392,5.62c-1.404-1.317-3.307-2.341-5.913-2.341C7.65,3.279,3.873,7.171,3.873,12s3.777,8.721,8.606,8.721c3.132,0,4.916-1.258,6.059-2.401c0.927-0.927,1.537-2.251,1.777-4.059L12.479,14.265z"
		/>
	</g>
</svg>

```

###### File: `src/lib/svgs/github.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg fill="currentColor" viewBox="0 0 1024 1024" {...props}>
	<path
		clip-rule="evenodd"
		d="M8 0C3.58 0 0 3.58 0 8C0 11.54 2.29 14.53 5.47 15.59C5.87 15.66 6.02 15.42 6.02 15.21C6.02 15.02 6.01 14.39 6.01 13.72C4 14.09 3.48 13.23 3.32 12.78C3.23 12.55 2.84 11.84 2.5 11.65C2.22 11.5 1.82 11.13 2.49 11.12C3.12 11.11 3.57 11.7 3.72 11.94C4.44 13.15 5.59 12.81 6.05 12.6C6.12 12.08 6.33 11.73 6.56 11.53C4.78 11.33 2.92 10.64 2.92 7.58C2.92 6.71 3.23 5.99 3.74 5.43C3.66 5.23 3.38 4.41 3.82 3.31C3.82 3.31 4.49 3.1 6.02 4.13C6.66 3.95 7.34 3.86 8.02 3.86C8.7 3.86 9.38 3.95 10.02 4.13C11.55 3.09 12.22 3.31 12.22 3.31C12.66 4.41 12.38 5.23 12.3 5.43C12.81 5.99 13.12 6.7 13.12 7.58C13.12 10.65 11.25 11.33 9.47 11.53C9.76 11.78 10.01 12.26 10.01 13.01C10.01 14.08 10 14.94 10 15.21C10 15.42 10.15 15.67 10.55 15.59C13.71 14.53 16 11.53 16 8C16 3.58 12.42 0 8 0Z"
		fill="currentColor"
		fill-rule="evenodd"
		transform="scale(64)"
	/>
</svg>

```

---

### Auth 3 <a name="auth-3"></a>

A premium auth 3 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `auth-three`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/auth-three.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:auth-divider`
  - `input-group`

##### Component Source Code

###### File: `src/lib/components/efferd/auth/auth-three.svelte`

```svelte
<script lang="ts">
	import { AtSignIcon } from "@lucide/svelte";
	import { Button } from "$lib/components/ui/button";
	import { AuthDivider } from "$lib/components/ui/auth-divider";
	import { InputGroup, InputGroupAddon, InputGroupInput } from "$lib/components/ui/input-group";
	import GithubLogo from "$lib/svgs/github.svelte";
	import GoogleLogo from "$lib/svgs/google.svelte";
	import Logo from "$lib/svgs/logo.svelte";
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";

	type AuthThreeProps = HTMLAttributes<HTMLDivElement> & {
		class?: string;
		logoHref?: string;
		privacyHref?: string;
		termsHref?: string;
		emailPlaceholder?: string;
	};

	let {
		class: className = "",
		logoHref = "/",
		privacyHref = "/",
		termsHref = "/",
		emailPlaceholder = "your.email@example.com",
		...restProps
	}: AuthThreeProps = $props();
</script>

<div class={cn("relative w-full overflow-hidden md:h-screen", className)} {...restProps}>
	<div
		class="relative mx-auto flex min-h-screen w-full max-w-sm flex-col justify-between p-6 md:p-8"
	>
		<div class="flex justify-center">
			<a href={logoHref}>
				<Logo class="h-4.5 w-auto" />
			</a>
		</div>

		<div class="w-full animate-in space-y-4 duration-600 fade-in slide-in-from-bottom-4">
			<div class="flex flex-col space-y-1">
				<h1 class="text-2xl font-bold tracking-wide">Join Now!</h1>
				<p class="text-base text-muted-foreground">Login or create your efferd account.</p>
			</div>

			<form class="space-y-2">
				<InputGroup>
					<InputGroupInput placeholder={emailPlaceholder} type="email" />
					<InputGroupAddon align="inline-start">
						<AtSignIcon />
					</InputGroupAddon>
				</InputGroup>

				<Button class="w-full" size="sm" type="button">Continue With Email</Button>
			</form>

			<AuthDivider>OR CONTINUE WITH</AuthDivider>

			<div class="space-y-2">
				<Button class="w-full" type="button" variant="outline">
					<GoogleLogo data-icon="inline-start" />
					Google
				</Button>

				<Button class="w-full" type="button" variant="outline">
					<GithubLogo data-icon="inline-start" />
					GitHub
				</Button>
			</div>
		</div>

		<p class="text-center text-sm text-muted-foreground">
			This site is protected by reCAPTCHA and the Google
			<a class="underline underline-offset-4 hover:text-primary" href={privacyHref}
				>Privacy Policy</a
			>
			and
			<a class="underline underline-offset-4 hover:text-primary" href={termsHref}
				>Terms of Service</a
			>
			apply.
		</p>
	</div>
</div>

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

###### File: `src/lib/svgs/google.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg fill="currentColor" viewBox="0 0 24 24" {...props}>
	<g>
		<path
			d="M12.479,14.265v-3.279h11.049c0.108,0.571,0.164,1.247,0.164,1.979c0,2.46-0.672,5.502-2.84,7.669C18.744,22.829,16.051,24,12.483,24C5.869,24,0.308,18.613,0.308,12S5.869,0,12.483,0c3.659,0,6.265,1.436,8.223,3.307L18.392,5.62c-1.404-1.317-3.307-2.341-5.913-2.341C7.65,3.279,3.873,7.171,3.873,12s3.777,8.721,8.606,8.721c3.132,0,4.916-1.258,6.059-2.401c0.927-0.927,1.537-2.251,1.777-4.059L12.479,14.265z"
		/>
	</g>
</svg>

```

###### File: `src/lib/svgs/github.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg fill="currentColor" viewBox="0 0 1024 1024" {...props}>
	<path
		clip-rule="evenodd"
		d="M8 0C3.58 0 0 3.58 0 8C0 11.54 2.29 14.53 5.47 15.59C5.87 15.66 6.02 15.42 6.02 15.21C6.02 15.02 6.01 14.39 6.01 13.72C4 14.09 3.48 13.23 3.32 12.78C3.23 12.55 2.84 11.84 2.5 11.65C2.22 11.5 1.82 11.13 2.49 11.12C3.12 11.11 3.57 11.7 3.72 11.94C4.44 13.15 5.59 12.81 6.05 12.6C6.12 12.08 6.33 11.73 6.56 11.53C4.78 11.33 2.92 10.64 2.92 7.58C2.92 6.71 3.23 5.99 3.74 5.43C3.66 5.23 3.38 4.41 3.82 3.31C3.82 3.31 4.49 3.1 6.02 4.13C6.66 3.95 7.34 3.86 8.02 3.86C8.7 3.86 9.38 3.95 10.02 4.13C11.55 3.09 12.22 3.31 12.22 3.31C12.66 4.41 12.38 5.23 12.3 5.43C12.81 5.99 13.12 6.7 13.12 7.58C13.12 10.65 11.25 11.33 9.47 11.53C9.76 11.78 10.01 12.26 10.01 13.01C10.01 14.08 10 14.94 10 15.21C10 15.42 10.15 15.67 10.55 15.59C13.71 14.53 16 11.53 16 8C16 3.58 12.42 0 8 0Z"
		fill="currentColor"
		fill-rule="evenodd"
		transform="scale(64)"
	/>
</svg>

```

---

### Auth 4 <a name="auth-4"></a>

A premium auth 4 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `auth-four`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/auth-four.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:auth-divider`
  - `local:full-width-divider`
  - `input-group`

##### Component Source Code

###### File: `src/lib/components/efferd/auth/auth-four.svelte`

```svelte
<script lang="ts">
	import { AtSignIcon } from "@lucide/svelte";
	import { Button } from "$lib/components/ui/button";
	import { AuthDivider } from "$lib/components/ui/auth-divider";
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import { InputGroup, InputGroupAddon, InputGroupInput } from "$lib/components/ui/input-group";
	import GoogleLogo from "$lib/svgs/google.svelte";
	import Logo from "$lib/svgs/logo.svelte";
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";

	type AuthFourProps = HTMLAttributes<HTMLDivElement> & {
		class?: string;
		logoHref?: string;
		privacyHref?: string;
		termsHref?: string;
		emailPlaceholder?: string;
		emailAriaLabel?: string;
	};

	let {
		class: className = "",
		logoHref = "/",
		privacyHref = "/",
		termsHref = "/",
		emailPlaceholder = "your.email@example.com",
		emailAriaLabel = "Email address",
		...restProps
	}: AuthFourProps = $props();
</script>

<div class={cn("relative w-full overflow-hidden px-4 md:h-screen", className)} {...restProps}>
	<div
		class="relative mx-auto flex min-h-screen w-full max-w-sm flex-col justify-center border-x *:px-6"
	>
		<div class="flex flex-col space-y-6">
			<a aria-label="Home" href={logoHref}>
				<Logo class="h-4.5 w-auto" />
			</a>

			<div class="space-y-1">
				<h1 class="text-xl font-semibold tracking-wide">Hey, welcome!</h1>
				<p class="text-base text-muted-foreground">
					Log in or sign up. It only takes a moment.
				</p>
			</div>
		</div>

		<div class="relative my-6 flex size-full flex-col gap-4 py-8">
			<FullWidthDivider position="top" />

			<Button class="w-full" type="button" variant="outline">
				<GoogleLogo aria-hidden="true" data-icon="inline-start" />
				Continue with Google
			</Button>

			<AuthDivider>OR CONTINUE WITH EMAIL</AuthDivider>

			<form class="space-y-2">
				<InputGroup>
					<InputGroupInput
						aria-label={emailAriaLabel}
						placeholder={emailPlaceholder}
						type="email"
					/>
					<InputGroupAddon align="inline-start">
						<AtSignIcon />
					</InputGroupAddon>
				</InputGroup>

				<Button class="w-full" size="sm" type="submit">Continue With Email</Button>
			</form>

			<FullWidthDivider position="bottom" />
		</div>

		<p class="text-center text-sm text-muted-foreground">
			This site is protected by reCAPTCHA and the Google
			<a class="underline underline-offset-4 hover:text-primary" href={privacyHref}
				>Privacy Policy</a
			>
			and
			<a class="underline underline-offset-4 hover:text-primary" href={termsHref}
				>Terms of Service</a
			>
			apply.
		</p>
	</div>
</div>

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

###### File: `src/lib/svgs/google.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg fill="currentColor" viewBox="0 0 24 24" {...props}>
	<g>
		<path
			d="M12.479,14.265v-3.279h11.049c0.108,0.571,0.164,1.247,0.164,1.979c0,2.46-0.672,5.502-2.84,7.669C18.744,22.829,16.051,24,12.483,24C5.869,24,0.308,18.613,0.308,12S5.869,0,12.483,0c3.659,0,6.265,1.436,8.223,3.307L18.392,5.62c-1.404-1.317-3.307-2.341-5.913-2.341C7.65,3.279,3.873,7.171,3.873,12s3.777,8.721,8.606,8.721c3.132,0,4.916-1.258,6.059-2.401c0.927-0.927,1.537-2.251,1.777-4.059L12.479,14.265z"
		/>
	</g>
</svg>

```

---

### Auth 5 <a name="auth-5"></a>

A premium auth 5 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `auth-five`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/auth-five.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:auth-divider`
  - `local:floating-paths`
  - `input-group`

##### Component Source Code

###### File: `src/lib/components/efferd/auth/auth-five.svelte`

```svelte
<script lang="ts">
	import { AtSignIcon, ChevronLeftIcon } from "@lucide/svelte";
	import { Button } from "$lib/components/ui/button";
	import { AuthDivider } from "$lib/components/ui/auth-divider";
	import { FloatingPaths } from "$lib/components/ui/floating-paths";
	import { InputGroup, InputGroupAddon, InputGroupInput } from "$lib/components/ui/input-group";
	import AppleLogo from "$lib/svgs/apple.svelte";
	import GithubLogo from "$lib/svgs/github.svelte";
	import GoogleLogo from "$lib/svgs/google.svelte";
	import Logo from "$lib/svgs/logo.svelte";
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";

	type AuthFiveProps = HTMLAttributes<HTMLElement> & {
		class?: string;
		homeHref?: string;
		logoHref?: string;
		termsHref?: string;
		privacyHref?: string;
		emailPlaceholder?: string;
		quoteText?: string;
		quoteAuthor?: string;
	};

	let {
		class: className = "",
		homeHref = "/",
		logoHref = "/",
		termsHref = "/",
		privacyHref = "/",
		emailPlaceholder = "your.email@example.com",
		quoteText = '"This Platform has helped me to save time and serve my clients faster than ever before."',
		quoteAuthor = "~ Ali Hassan",
		...restProps
	}: AuthFiveProps = $props();
</script>

<main
	class={cn("relative md:h-screen md:overflow-hidden lg:grid lg:grid-cols-2", className)}
	{...restProps}
>
	<div
		class="relative hidden h-full flex-col border-r bg-secondary p-10 lg:flex dark:bg-secondary/20"
	>
		<div
			class="absolute inset-0 bg-linear-to-b from-transparent via-transparent to-background"
		></div>

		<a aria-label="Home" class="relative z-10 mr-auto" href={logoHref}>
			<Logo class="h-4.5 w-auto" />
		</a>

		<div class="z-10 mt-auto">
			<blockquote class="space-y-2">
				<p class="text-xl">{quoteText}</p>
				<footer class="font-mono text-sm font-semibold">{quoteAuthor}</footer>
			</blockquote>
		</div>

		<div class="absolute inset-0">
			<FloatingPaths position={1} />
			<FloatingPaths position={-1} />
		</div>
	</div>

	<div class="relative flex min-h-screen flex-col justify-center px-8">
		<div aria-hidden="true" class="absolute inset-0 isolate -z-10 opacity-60 contain-strict">
			<div
				class="absolute top-0 right-0 h-320 w-140 -translate-y-87.5 rounded-full bg-[radial-gradient(68.54%_68.72%_at_55.02%_31.46%,--theme(--color-foreground/.06)_0,hsla(0,0%,55%,.02)_50%,--theme(--color-foreground/.01)_80%)]"
			></div>
			<div
				class="absolute top-0 right-0 h-320 w-60 [translate:5%_-50%] rounded-full bg-[radial-gradient(50%_50%_at_50%_50%,--theme(--color-foreground/.04)_0,--theme(--color-foreground/.01)_80%,transparent_100%)]"
			></div>
			<div
				class="absolute top-0 right-0 h-320 w-60 -translate-y-87.5 rounded-full bg-[radial-gradient(50%_50%_at_50%_50%,--theme(--color-foreground/.04)_0,--theme(--color-foreground/.01)_80%,transparent_100%)]"
			></div>
		</div>

		<Button class="absolute top-7 left-5" href={homeHref} variant="ghost">
			<ChevronLeftIcon data-icon="inline-start" />
			Home
		</Button>

		<div class="mx-auto w-full max-w-sm space-y-4">
			<a aria-label="Home" class="w-fit lg:hidden" href={logoHref}>
				<Logo class="h-4.5 w-auto" />
			</a>

			<div class="flex flex-col space-y-1">
				<h1 class="text-2xl font-bold tracking-wide">Sign In or Join Now!</h1>
				<p class="text-base text-muted-foreground">login or create your efferd account.</p>
			</div>

			<div class="space-y-2">
				<Button class="w-full" type="button">
					<GoogleLogo data-icon="inline-start" />
					Continue with Google
				</Button>

				<Button class="w-full" type="button">
					<AppleLogo data-icon="inline-start" />
					Continue with Apple
				</Button>

				<Button class="w-full" type="button">
					<GithubLogo data-icon="inline-start" />
					Continue with GitHub
				</Button>
			</div>

			<AuthDivider>OR</AuthDivider>

			<form class="space-y-2">
				<p class="text-start text-xs text-muted-foreground">
					Enter your email address to sign in or create an account
				</p>

				<InputGroup>
					<InputGroupInput placeholder={emailPlaceholder} type="email" />
					<InputGroupAddon align="inline-start">
						<AtSignIcon />
					</InputGroupAddon>
				</InputGroup>

				<Button class="w-full" type="button">Continue With Email</Button>
			</form>

			<p class="mt-8 text-sm text-muted-foreground">
				By clicking continue, you agree to our
				<a class="underline underline-offset-4 hover:text-primary" href={termsHref}
					>Terms of Service</a
				>
				and
				<a class="underline underline-offset-4 hover:text-primary" href={privacyHref}
					>Privacy Policy</a
				>.
			</p>
		</div>
	</div>
</main>

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

###### File: `src/lib/svgs/google.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg fill="currentColor" viewBox="0 0 24 24" {...props}>
	<g>
		<path
			d="M12.479,14.265v-3.279h11.049c0.108,0.571,0.164,1.247,0.164,1.979c0,2.46-0.672,5.502-2.84,7.669C18.744,22.829,16.051,24,12.483,24C5.869,24,0.308,18.613,0.308,12S5.869,0,12.483,0c3.659,0,6.265,1.436,8.223,3.307L18.392,5.62c-1.404-1.317-3.307-2.341-5.913-2.341C7.65,3.279,3.873,7.171,3.873,12s3.777,8.721,8.606,8.721c3.132,0,4.916-1.258,6.059-2.401c0.927-0.927,1.537-2.251,1.777-4.059L12.479,14.265z"
		/>
	</g>
</svg>

```

###### File: `src/lib/svgs/github.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg fill="currentColor" viewBox="0 0 1024 1024" {...props}>
	<path
		clip-rule="evenodd"
		d="M8 0C3.58 0 0 3.58 0 8C0 11.54 2.29 14.53 5.47 15.59C5.87 15.66 6.02 15.42 6.02 15.21C6.02 15.02 6.01 14.39 6.01 13.72C4 14.09 3.48 13.23 3.32 12.78C3.23 12.55 2.84 11.84 2.5 11.65C2.22 11.5 1.82 11.13 2.49 11.12C3.12 11.11 3.57 11.7 3.72 11.94C4.44 13.15 5.59 12.81 6.05 12.6C6.12 12.08 6.33 11.73 6.56 11.53C4.78 11.33 2.92 10.64 2.92 7.58C2.92 6.71 3.23 5.99 3.74 5.43C3.66 5.23 3.38 4.41 3.82 3.31C3.82 3.31 4.49 3.1 6.02 4.13C6.66 3.95 7.34 3.86 8.02 3.86C8.7 3.86 9.38 3.95 10.02 4.13C11.55 3.09 12.22 3.31 12.22 3.31C12.66 4.41 12.38 5.23 12.3 5.43C12.81 5.99 13.12 6.7 13.12 7.58C13.12 10.65 11.25 11.33 9.47 11.53C9.76 11.78 10.01 12.26 10.01 13.01C10.01 14.08 10 14.94 10 15.21C10 15.42 10.15 15.67 10.55 15.59C13.71 14.53 16 11.53 16 8C16 3.58 12.42 0 8 0Z"
		fill="currentColor"
		fill-rule="evenodd"
		transform="scale(64)"
	/>
</svg>

```

###### File: `src/lib/svgs/apple.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { fill = "currentColor", ...props }: SVGAttributes<SVGSVGElement> & { fill?: string } =
		$props();
</script>

<svg {fill} viewBox="0 0 24 24" {...props}>
	<g id="_Group_2">
		<g id="_Group_3">
			<path
				d="M18.546,12.763c0.024-1.87,1.004-3.597,2.597-4.576c-1.009-1.442-2.64-2.323-4.399-2.378 c-1.851-0.194-3.645,1.107-4.588,1.107c-0.961,0-2.413-1.088-3.977-1.056C6.122,5.927,4.25,7.068,3.249,8.867 c-2.131,3.69-0.542,9.114,1.5,12.097c1.022,1.461,2.215,3.092,3.778,3.035c1.529-0.063,2.1-0.975,3.945-0.975 c1.828,0,2.364,0.975,3.958,0.938c1.64-0.027,2.674-1.467,3.66-2.942c0.734-1.041,1.299-2.191,1.673-3.408 C19.815,16.788,18.548,14.879,18.546,12.763z"
				id="_Path_"
			/>
			<path
				d="M15.535,3.847C16.429,2.773,16.87,1.393,16.763,0c-1.366,0.144-2.629,0.797-3.535,1.829 c-0.895,1.019-1.349,2.351-1.261,3.705C13.352,5.548,14.667,4.926,15.535,3.847z"
				id="_Path_2"
			/>
		</g>
	</g>
</svg>

```

---

### Auth Divider <a name="auth-divider"></a>

A premium auth divider block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `auth-divider`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/auth-divider.json
```

##### Component Source Code

###### File: `src/lib/components/ui/auth-divider/auth-divider.svelte`

```svelte
<script lang="ts">
	import { cn, type WithElementRef } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";

	let {
		ref = $bindable(null),
		class: className,
		children,
		...restProps
	}: WithElementRef<HTMLAttributes<HTMLDivElement>> = $props();
</script>

<div bind:this={ref} class={cn("relative flex w-full items-center", className)} {...restProps}>
	<div class="w-full border-t"></div>
	<div class="flex w-max justify-center px-2 text-xs text-nowrap text-muted-foreground">
		{@render children?.()}
	</div>
	<div class="w-full border-t"></div>
</div>

```

###### File: `src/lib/components/ui/auth-divider/index.ts`

```ts
import Root from "./auth-divider.svelte";

export {
	Root,
	//
	Root as AuthDivider
};

```

---

## Blog Blocks <a name="blog"></a>

### Blog 1 <a name="blog-1"></a>

A premium blog 1 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `blog-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/blog-one.json
```

##### Dependencies

- **Registry Dependencies**:
  - `local:full-width-divider`

##### Component Source Code

###### File: `src/lib/components/efferd/blogs/blog-one/blog-one.svelte`

```svelte
<script>
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import BlogCard from "./blog-card.svelte";
	const blogs = [
		{
			title: "The New Design",
			date: "May 20 2025",
			description: "What everyone new to the field should know, and how we can help.",
			href: "/"
		},
		{
			title: "Letter Club",
			date: "Aug 14 2025",
			description: "An ode to the slow web.",
			href: "/"
		},
		{
			title: "Have the Coffee",
			date: "Sep 19 2025",
			description: "Carve space out for oppurtunity.",
			href: "/"
		},
		{
			title: "Shadcn UI",
			date: "Oct 12 2025",
			description: "Building modern applications with reusable components.",
			href: "/"
		},
		{
			title: "Fesgin",
			date: "Nov 23 2025",
			description: "Exploring the intersection of design and development.",
			href: "/"
		}
	];
</script>

<div class="mx-auto flex min-h-screen w-full max-w-3xl flex-col justify-start md:border-x">
	<div class="space-y-2 px-4 py-8 md:py-12">
		<h1 class="text-2xl font-semibold tracking-wide md:text-4xl">Latest Blogs</h1>
		<p class="text-sm text-muted-foreground">
			Discover the latest trends and insights in the world of design and technology.
		</p>
	</div>

	<div class="relative">
		<FullWidthDivider />
		<div class="divide-y">
			{#each blogs as blog}
				<BlogCard {...blog} />
			{/each}
		</div>
		<FullWidthDivider />
	</div>
</div>

```

###### File: `src/lib/components/efferd/blogs/blog-one/blog-card.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";
	type BlogCard = {
		title: string;
		date: string;
		description: string;
	};

	type Props = BlogCard & HTMLAttributes<HTMLAnchorElement>;

	let { title, date, description, ...props }: Props = $props();
</script>

<a
	class={cn(
		"group flex h-24 w-full flex-col justify-center gap-y-1 p-4 hover:cursor-pointer hover:bg-accent/30 active:bg-accent dark:active:bg-accent/50"
	)}
	{...props}
>
	<div class="relative flex items-end justify-center gap-2">
		<h3 class="text-lg font-medium whitespace-nowrap text-foreground md:text-xl">
			{title}
		</h3>
		<span class="mb-1.5 w-full border-b-2 border-dashed"></span>
		<span
			class="font-mono text-xs whitespace-nowrap text-muted-foreground uppercase group-hover:text-foreground md:text-sm"
		>
			{date}
		</span>
	</div>
	<div
		class="max-w-sm text-sm text-muted-foreground group-hover:text-foreground md:max-w-full md:text-base"
	>
		{description}
	</div>
</a>

```

---

### Blog 2 <a name="blog-2"></a>

A premium blog 2 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `blog-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/blog-two.json
```

##### Dependencies

- **Registry Dependencies**:
  - `local:full-width-divider`
  - `local:grid-filler`

##### Component Source Code

###### File: `src/lib/components/efferd/blogs/blog-two/blog-two.svelte`

```svelte
<script lang="ts">
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import { GridFiller } from "$lib/components/ui/grid-filler";
	import BlogCard from "./blog-card.svelte";

	type BlogType = {
		title: string;
		date: string;
		description: string;
		category: string;
		author: string;
		href: string;
	};

	const blogs: BlogType[] = [
		{
			title: "The New Design Principles for Modern Web Apps",
			date: "May 20 2025",
			category: "Design",
			author: "Sarah Chen",
			description:
				"We dive deep into modern UI/UX fundamentals and explore how small changes can make a massive impact on user retention.",
			href: "/"
		},
		{
			title: "Letter Club: An Ode to the Slow Web",
			date: "Aug 14 2025",
			category: "Design",
			author: "Mike Allyn",
			description:
				"In a world of instant gratification, we explore the beauty of thoughtful, long-form content and meaningful connections over time.",
			href: "/"
		},
		{
			title: "Carve Out Space for Opportunity and Coffee",
			date: "Sep 19 2025",
			category: "Productivity",
			author: "Jessica Doi",
			description:
				"Taking a break is work. Learn how simple rituals like a morning coffee can boost your creativity and productivity.",
			href: "/"
		},
		{
			title: "Building Modern Applications with Shadcn UI Components",
			date: "Oct 12 2025",
			category: "Design",
			author: "Tom Cook",
			description:
				"A comprehensive guide to leveraging Shadcn UI to build accessible, customizable, and beautiful user interfaces with incredible speed.",
			href: "/"
		},
		{
			title: "Fesgin: Bridging The Gap Between Design and Code",
			date: "Nov 23 2025",
			category: "Design",
			author: "David Park",
			description:
				"How designers and developers can collaborate more effectively to bridge the gap between creative vision and technical implementation.",
			href: "/"
		},
		{
			title: "The Art of Simplicity in User Interface Design",
			date: "Dec 05 2025",
			category: "Minimalism",
			author: "Emma Wilson",
			description:
				"Discover how minimalism in design leads to clearer communication and a more intuitive user experience, focusing on what matters.",
			href: "/"
		},
		{
			title: "Why Web Performance Matters For Your Business Growth",
			date: "Jan 18 2026",
			category: "Engineering",
			author: "Chris Martin",
			description:
				"We discuss techniques for improving web performance, from lazy loading to code splitting, ensuring your application runs smoothly.",
			href: "/"
		},
		{
			title: "Practicing Digital Well-being in an Always-On World",
			date: "Feb 02 2026",
			category: "Lifestyle",
			author: "Olivia Kim",
			description:
				"Strategies for maintaining a healthy relationship with digital tools, setting boundaries, and ensuring technology serves us rather than consumes us.",
			href: "/"
		}
	];
</script>

<div class="mx-auto w-full max-w-5xl py-4 lg:border-x">
	<div class="space-y-2 px-4 py-8 md:py-12">
		<h1 class="text-2xl font-semibold tracking-wide md:text-4xl">Latest Blogs</h1>
		<p class="text-sm text-muted-foreground">
			Discover the latest trends and insights in the world of design and technology.
		</p>
	</div>
	<FullWidthDivider contained={true} />
	<div class="grid grid-cols-1 gap-px bg-border sm:grid-cols-2 md:grid-cols-3">
		{#each blogs as blog (blog.title)}
			<BlogCard {...blog} />
		{/each}
		<GridFiller
			class="bg-background"
			mdColumns={3}
			smColumns={4}
			lgColumns={3}
			totalItems={blogs.length}
		/>
	</div>
	<FullWidthDivider contained={true} />
</div>

```

###### File: `src/lib/components/efferd/blogs/blog-two/blog-card.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";

	type BlogCard = {
		title: string;
		date: string;
		description: string;
		category: string;
		author: string;
		href: string;
	};

	type Props = BlogCard & HTMLAttributes<HTMLAnchorElement>;

	let {
		title,
		date,
		description,
		category,
		author,
		class: className = "",
		...props
	}: Props = $props();
</script>

<a
	class={cn(
		"group w-full bg-background px-6 py-12 text-muted-foreground hover:cursor-pointer hover:text-foreground active:bg-accent md:px-8 active:dark:bg-accent/50",
		className
	)}
	{...props}
>
	<h3 class="mb-3 line-clamp-2 text-lg font-medium text-foreground md:text-xl">
		{title}
	</h3>
	<div class="mb-3 flex items-center gap-2">
		<span class="text-xs text-muted-foreground group-hover:text-foreground">
			{category}
		</span>
		<div class="inline-flex size-1 rounded-full bg-muted-foreground"></div>
		<span class="text-xs text-muted-foreground group-hover:text-foreground">
			{date}
		</span>
	</div>
	<p
		class="mb-8 line-clamp-3 text-sm tracking-wide text-muted-foreground group-hover:text-foreground"
	>
		{description}
	</p>
	<div class="flex items-center gap-1.5">
		by
		<span
			class="font-mono text-xs font-medium text-foreground/80 group-hover:text-foreground md:text-sm"
		>
			{author}
		</span>
	</div>
</a>

```

---

### Blog 3 <a name="blog-3"></a>

A premium blog 3 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `blog-three`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/blog-three.json
```

##### Dependencies

- **Registry Dependencies**:
  - `local:full-width-divider`
  - `aspect-ratio`

##### Component Source Code

###### File: `src/lib/components/efferd/blogs/blog-three/blog-three.svelte`

```svelte
<script lang="ts">
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import BlogCard from "./blog-card.svelte";

	type BlogType = {
		title: string;
		href: string;
		description: string;
		author: string;
		createdAt: string;
		readTime: string;
		image: string;
	};

	const blogs: BlogType[] = [
		{
			title: "Design Systems That Scale",
			href: "/",
			description:
				"Learn how to build and maintain scalable design systems that empower teams to move faster while staying consistent.",
			image: "https://storage.efferd.com/creative/beams.webp",
			createdAt: "2025-08-25",
			author: "Ava Mitchell",
			readTime: "7 min read"
		},
		{
			title: "The Psychology of Color in UI",
			href: "/",
			description:
				"Explore how different colors influence user perception, emotion, and conversion in digital product design.",
			image: "https://storage.efferd.com/creative/plasma.webp",
			createdAt: "2025-07-14",
			author: "Liam Carter",
			readTime: "5 min read"
		},
		{
			title: "Microinteractions That Delight",
			href: "/",
			description:
				"Discover how subtle animations and interactions can enhance usability and bring joy to your users.",
			image: "https://storage.efferd.com/creative/ripple-grid.webp",
			createdAt: "2025-06-30",
			author: "Sophia Kim",
			readTime: "6 min read"
		},
		{
			title: "Accessibility Beyond Compliance",
			href: "/",
			description:
				"Practical steps to make your UI accessible, not just legally compliant, but genuinely inclusive for everyone.",
			image: "https://storage.efferd.com/creative/silk.webp",
			createdAt: "2025-06-18",
			author: "Ethan Rodriguez",
			readTime: "8 min read"
		},
		{
			title: "Dark Mode Done Right",
			href: "/",
			description:
				"Tips and tricks to design beautiful and functional dark mode experiences that users will love.",
			image: "https://storage.efferd.com/creative/dark-veil.webp",
			createdAt: "2025-05-20",
			author: "Maya Chen",
			readTime: "4 min read"
		},
		{
			title: "Typography That Speaks",
			href: "/",
			description:
				"How to select and pair typefaces that enhance readability, hierarchy, and brand personality.",
			image: "https://storage.efferd.com/creative/threads.webp",
			createdAt: "2025-05-02",
			author: "Noah Patel",
			readTime: "9 min read"
		},
		{
			title: "The Future of UI Animation",
			href: "/",
			description:
				"From motion guidelines to advanced prototyping-discover where UI animation is headed in 2025.",
			image: "https://storage.efferd.com/creative/hyperspeed.webp",
			createdAt: "2025-04-15",
			author: "Chloe Ramirez",
			readTime: "10 min read"
		},
		{
			title: "Minimalism vs Maximalism",
			href: "/",
			description:
				"A deep dive into two opposing design philosophies and how to decide which fits your product.",
			image: "https://storage.efferd.com/creative/pixel-blast.webp",
			createdAt: "2025-04-01",
			author: "Benjamin Scott",
			readTime: "6 min read"
		},
		{
			title: "Designing for Mobile-First",
			href: "/",
			description:
				"Best practices for mobile-first design, from layout decisions to performance optimization.",
			image: "https://storage.efferd.com/creative/floating-lines.webp",
			createdAt: "2025-03-22",
			author: "Isabella White",
			readTime: "7 min read"
		},
		{
			title: "Figma Hacks for Power Users",
			href: "/",
			description:
				"Hidden features, shortcuts, and workflows in Figma that can dramatically speed up your design process.",
			image: "https://storage.efferd.com/creative/color-bends.webp",
			createdAt: "2025-03-09",
			author: "James Walker",
			readTime: "5 min read"
		},
		{
			title: "Designing With AI Tools",
			href: "/",
			description:
				"A practical look at how AI tools are shaping UI/UX workflows-from ideation to final delivery.",
			image: "https://storage.efferd.com/creative/light-rays.webp",
			createdAt: "2025-02-28",
			author: "Olivia Brooks",
			readTime: "8 min read"
		},
		{
			title: "The Art of Prototyping",
			href: "/",
			description:
				"How to create prototypes that effectively communicate your ideas and speed up stakeholder feedback.",
			image: "https://storage.efferd.com/creative/orb.webp",
			createdAt: "2025-02-14",
			author: "Daniel Green",
			readTime: "6 min read"
		}
	];
</script>

<div class="mx-auto w-full max-w-5xl grow">
	<div class="space-y-1 px-4 py-8 md:px-6">
		<h1 class="text-2xl font-semibold tracking-wide md:text-4xl">Blog Section</h1>
		<p class="text-sm text-muted-foreground md:text-base">
			Discover the latest trends and insights in the world of design and technology.
		</p>
	</div>

	<FullWidthDivider contained={true} />

	<div class="grid gap-2 p-4 md:grid-cols-2 lg:grid-cols-3">
		{#each blogs as blog}
			<BlogCard {...blog} />
		{/each}
	</div>
</div>

```

###### File: `src/lib/components/efferd/blogs/blog-three/blog-card.svelte`

```svelte
<script lang="ts">
	import { AspectRatio } from "$lib/components/ui/aspect-ratio";
	import { cn } from "$lib/utils";
	import type { HTMLAnchorAttributes } from "svelte/elements";

	type BlogCard = {
		title: string;
		href: string;
		description: string;
		author: string;
		createdAt: string;
		readTime: string;
		image: string;
	};

	type Props = BlogCard & HTMLAnchorAttributes;

	const fallbackImage = "https://placehold.co/640x360?text=fallback-image";

	let {
		title,
		href,
		description,
		author,
		createdAt,
		readTime,
		image,
		class: className = "",
		...props
	}: Props = $props();

	let imageSrc = $state("");

	$effect(() => {
		imageSrc = image;
	});

	function handleImageError() {
		if (imageSrc !== fallbackImage) {
			imageSrc = fallbackImage;
		}
	}
</script>

<a
	{href}
	class={cn(
		"group flex flex-col gap-2 rounded-xl p-3 transition-colors hover:bg-muted/50 active:bg-muted",
		className
	)}
	{...props}
>
	<AspectRatio
		ratio={16 / 9}
		class="overflow-hidden rounded-xl shadow-md outline outline-offset-3 outline-border/50"
	>
		<img
			src={imageSrc}
			alt={title}
			loading="lazy"
			class="h-full w-full object-cover transition-transform duration-500 group-hover:scale-105"
			onerror={handleImageError}
		/>
	</AspectRatio>

	<div class="space-y-2 px-2 pb-2">
		<div
			class="flex flex-wrap items-center gap-2 text-[11px] text-muted-foreground transition-colors group-hover:text-foreground sm:text-xs"
		>
			<p>by {author}</p>
			<div class="size-1 rounded-full bg-current"></div>
			<p>{createdAt}</p>
			<div class="size-1 rounded-full bg-current"></div>
			<p>{readTime}</p>
		</div>

		<h2 class="line-clamp-2 text-lg font-semibold">{title}</h2>
		<p
			class="line-clamp-3 text-sm text-muted-foreground transition-colors group-active:text-foreground"
		>
			{description}
		</p>
	</div>
</a>

```

---

## Contact Blocks <a name="contact"></a>

### Contact 1 <a name="contact-1"></a>

A premium contact 1 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `contact-1`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/contact-1.json
```

##### Component Source Code

###### File: `registry/blocks/contact/1/contact.tsx`

```tsx
"use client";

import { type LucideIcon, Mail, MapPin, Phone } from "lucide-react";
import type React from "react";
import { cn } from "@/lib/utils";

const APP_EMAIL = "mail@example.com";
const APP_PHONE = "+92 300 1234567";
const APP_PHONE_2 = "+92 321 9876543";

export function Contact() {
  const socialLinks = [
    {
      icon: GithubIcon,
      href: "#",
      label: "GitHub",
    },
    {
      icon: XIcon,
      href: "#",
      label: "Twitter",
    },
  ];

  return (
    <div className="mx-auto h-full min-h-screen max-w-5xl lg:border-x">
      <div className="flex grow flex-col justify-center px-4 py-18 md:items-center">
        <h1 className="font-bold text-4xl md:text-5xl">Contact Us</h1>
        <p className="mb-5 text-base text-muted-foreground">
          Contact the support team at efferd.
        </p>
      </div>
      <BorderSeparator />
      <div className="grid md:grid-cols-3">
        <Box
          description="We respond to all emails within 24 hours."
          icon={Mail}
          title="Email"
        >
          <a
            className="font-medium font-mono text-sm tracking-wide hover:underline"
            href={`mailto:${APP_EMAIL}`}
          >
            {APP_EMAIL}
          </a>
        </Box>
        <Box
          description="Drop by our office for a chat."
          icon={MapPin}
          title="Office"
        >
          <span className="font-medium font-mono text-sm tracking-wide">
            Office # 100, 101 Second Floor Kohinoor 1, Faisalabad, Pakistan
          </span>
        </Box>
        <Box
          className="border-b-0 md:border-r-0"
          description="We're available Mon-Fri, 9am-5pm."
          icon={Phone}
          title="Phone"
        >
          <div>
            <a
              className="block font-medium font-mono text-sm tracking-wide hover:underline"
              href={`tel:${APP_PHONE}`}
            >
              {APP_PHONE}
            </a>
            <a
              className="block font-medium font-mono text-sm tracking-wide hover:underline"
              href={`tel:${APP_PHONE_2}`}
            >
              {APP_PHONE_2}
            </a>
          </div>
        </Box>
      </div>
      <BorderSeparator />
      <div className="z-1 flex h-full flex-col items-center justify-center gap-4 py-24">
        <h2 className="text-center font-medium text-2xl text-muted-foreground tracking-tight md:text-3xl">
          Find us <span className="text-foreground">online</span>
        </h2>
        <div className="flex flex-wrap items-center gap-2">
          {socialLinks.map((link) => (
            <a
              className="flex items-center gap-x-2 rounded-full border bg-card px-3 py-1.5 shadow hover:bg-accent"
              href={link.href}
              key={link.label}
              rel="noopener noreferrer"
              target="_blank"
            >
              <link.icon className="size-3.5 text-muted-foreground" />
              <span className="font-medium font-mono text-xs tracking-wide">
                {link.label}
              </span>
            </a>
          ))}
        </div>
      </div>
    </div>
  );
}

function BorderSeparator({ className }: React.ComponentProps<"div">) {
  return (
    <div className={cn("absolute inset-x-0 h-px w-full border-b", className)} />
  );
}

type ContactBox = React.ComponentProps<"div"> & {
  icon: LucideIcon;
  title: string;
  description: string;
};

function Box({
  title,
  description,
  className,
  children,
  ...props
}: ContactBox) {
  return (
    <div
      className={cn(
        "flex flex-col justify-between border-b md:border-r md:border-b-0",
        className
      )}
    >
      <div className="flex items-center gap-x-3 border-b bg-secondary/50 p-4 dark:bg-secondary/20">
        <props.icon className="size-5 text-muted-foreground" strokeWidth={1} />
        <h2 className="font-heading font-medium text-lg tracking-wider">
          {title}
        </h2>
      </div>
      <div className="flex items-center gap-x-2 p-4 py-12">{children}</div>
      <div className="border-t p-4">
        <p className="text-muted-foreground text-sm">{description}</p>
      </div>
    </div>
  );
}

const GithubIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 1024 1024" {...props}>
    <path
      clipRule="evenodd"
      d="M8 0C3.58 0 0 3.58 0 8C0 11.54 2.29 14.53 5.47 15.59C5.87 15.66 6.02 15.42 6.02 15.21C6.02 15.02 6.01 14.39 6.01 13.72C4 14.09 3.48 13.23 3.32 12.78C3.23 12.55 2.84 11.84 2.5 11.65C2.22 11.5 1.82 11.13 2.49 11.12C3.12 11.11 3.57 11.7 3.72 11.94C4.44 13.15 5.59 12.81 6.05 12.6C6.12 12.08 6.33 11.73 6.56 11.53C4.78 11.33 2.92 10.64 2.92 7.58C2.92 6.71 3.23 5.99 3.74 5.43C3.66 5.23 3.38 4.41 3.82 3.31C3.82 3.31 4.49 3.1 6.02 4.13C6.66 3.95 7.34 3.86 8.02 3.86C8.7 3.86 9.38 3.95 10.02 4.13C11.55 3.09 12.22 3.31 12.22 3.31C12.66 4.41 12.38 5.23 12.3 5.43C12.81 5.99 13.12 6.7 13.12 7.58C13.12 10.65 11.25 11.33 9.47 11.53C9.76 11.78 10.01 12.26 10.01 13.01C10.01 14.08 10 14.94 10 15.21C10 15.42 10.15 15.67 10.55 15.59C13.71 14.53 16 11.53 16 8C16 3.58 12.42 0 8 0Z"
      fill="currentColor"
      fillRule="evenodd"
      transform="scale(64)"
    />
  </svg>
);

const XIcon = (props: React.ComponentProps<"svg">) => (
  <svg
    fill="currentColor"
    viewBox="0 0 24 24"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <path d="m18.9,1.153h3.682l-8.042,9.189,9.46,12.506h-7.405l-5.804-7.583-6.634,7.583H.469l8.6-9.831L0,1.153h7.593l5.241,6.931,6.065-6.931Zm-1.293,19.494h2.039L6.482,3.239h-2.19l13.314,17.408Z" />
  </svg>
);

```

#### Svelte Implementation

- **Registry Key**: `contact-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/contact-one.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `local:full-width-divider`

##### Component Source Code

###### File: `src/lib/components/efferd/contact/contact-one.svelte`

```svelte
<script lang="ts">
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import { cn } from "$lib/utils";
	import { Mail, MapPin, Phone } from "@lucide/svelte";

	const data = [
		{
			title: "Call Us Today!",
			value: "+1 (555) 123-4567",
			icon: Phone
		},
		{
			title: "Send an Email",
			value: "mail@example.com",
			icon: Mail
		},
		{
			title: "Visit Our Office",
			value: "100 Smith Street, VIC",
			icon: MapPin
		}
	];
</script>

<div class="mx-auto max-w-4xl">
	<h2 class="mb-6 text-lg font-medium md:text-2xl">Have Questions? Get in Touch!</h2>
	<div class="relative">
		<FullWidthDivider position="top"></FullWidthDivider>
		<div class="grid gap-px overflow-hidden bg-border px-px md:grid-cols-3">
			{#each data as item}
				{@const Icon = item.icon}
				<div class="flex items-center gap-3 bg-background p-2 shadow-xs">
					<div
						class={cn(
							"flex size-12 shrink-0 items-center justify-center rounded-lg bg-muted/50",
							"[&_svg]:size-4 [&_svg]:text-muted-foreground"
						)}
					>
						<Icon />
					</div>
					<div class={cn("flex flex-col gap-y-0.5")}>
						<h2 class="text-sm">{item.title}</h2>
						<p class="text-xs text-muted-foreground">{item.value}</p>
					</div>
				</div>
			{/each}
		</div>
		<FullWidthDivider position="bottom"></FullWidthDivider>
	</div>
</div>

```

---

### Contact 2 <a name="contact-2"></a>

A premium contact 2 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `contact-2`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/contact-2.json
```

##### Component Source Code

###### File: `registry/blocks/contact/2/contact-card.tsx`

```tsx
import { type LucideIcon, PlusIcon } from "lucide-react";
import type React from "react";
import { cn } from "@/lib/utils";

type ContactInfoProps = React.ComponentProps<"div"> & {
  icon: LucideIcon;
  label: string;
  value: string;
};

type ContactCardProps = React.ComponentProps<"div"> & {
  // Content props
  title?: string;
  description?: string;
  contactInfo?: ContactInfoProps[];
  formSectionClassName?: string;
};

export function ContactCard({
  title = "Contact With Us",
  description = "If you have any questions regarding our Services or need help, please fill out the form here. We do our best to respond within 1 business day.",
  contactInfo,
  className,
  formSectionClassName,
  children,
  ...props
}: ContactCardProps) {
  return (
    <div
      className={cn(
        "relative grid h-full w-full border md:grid-cols-2 lg:grid-cols-3",
        className
      )}
      {...props}
    >
      <PlusIcon
        className="-top-[12.5px] -left-[12.5px] absolute h-6 w-6"
        strokeWidth={1}
      />
      <PlusIcon
        className="-top-[12.5px] -right-[12.5px] absolute h-6 w-6"
        strokeWidth={1}
      />
      <PlusIcon
        className="-bottom-[12.5px] -left-[12.5px] absolute h-6 w-6"
        strokeWidth={1}
      />
      <PlusIcon
        className="-right-[12.5px] -bottom-[12.5px] absolute h-6 w-6"
        strokeWidth={1}
      />

      <div className="col-span-1 flex flex-col justify-between bg-secondary/50 lg:col-span-2 dark:bg-background">
        <div className="relative h-full space-y-4 px-4 py-8 md:p-8">
          <h1 className="font-semibold text-3xl md:text-4xl lg:text-5xl">
            {title}
          </h1>
          <p className="max-w-xl text-muted-foreground text-sm md:text-base lg:text-lg">
            {description}
          </p>
          <div className="grid gap-4 md:grid md:grid-cols-2 lg:grid-cols-3">
            {contactInfo?.map((info) => (
              <ContactInfo key={info.label} {...info} />
            ))}
          </div>
        </div>
      </div>
      <div
        className={cn(
          "col-span-1 flex h-full w-full items-center border-t bg-card px-4 py-8 md:border-t-0 md:border-l dark:bg-card/50",
          formSectionClassName
        )}
      >
        {children}
      </div>
    </div>
  );
}

function ContactInfo({
  icon: Icon,
  label,
  value,
  className,
  ...props
}: ContactInfoProps) {
  return (
    <div className={cn("flex items-center gap-3 py-3", className)} {...props}>
      <div className="rounded-lg border bg-card p-3 shadow">
        <Icon className="h-5 w-5" />
      </div>
      <div>
        <p className="font-medium">{label}</p>
        <p className="text-muted-foreground text-xs">{value}</p>
      </div>
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `contact-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/contact-two.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `src/lib/components/efferd/contact/contact-two.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import { Button } from "$lib/components/ui/button";
	import XLogo from "$lib/svgs/x.svelte";
	import { Mail, Users, type Icon as IconType } from "@lucide/svelte";
	import type { Component } from "svelte";

	const APP_EMAIL = "mail@example.com";

	type Data = {
		title: string;
		description: string;
		icon: typeof IconType | Component;
		href: string;
		label: string;
	};
	let data: Data[] = [
		{
			title: "Email Us",
			description: "We respond to all emails within 24 hours.",
			icon: Mail,
			href: `mailto:${APP_EMAIL}`,
			label: APP_EMAIL
		},
		{
			title: "Send us DM",
			description: "Send us a direct message on X for quick answers.",
			icon: XLogo,
			href: "/",
			label: "@efferdui"
		},
		{
			title: "Join the community",
			description: "Join our community to connect with other users.",
			icon: Users,
			href: "/",
			label: "Join Discord"
		}
	];
</script>

<div class="mx-auto max-w-4xl">
	<div class="mb-12 flex max-w-md flex-col justify-center gap-2">
		<h1 class="text-2xl font-bold md:text-3xl">Contact Us</h1>
		<p class="text-base text-muted-foreground">
			We&apos;re here to help and answer any question you might have, We look forward to
			hearing from you.
		</p>
	</div>
	<div
		class="grid gap-0.5 overflow-hidden rounded-lg bg-muted p-0.5 md:grid-cols-3 dark:bg-muted/50"
	>
		{#each data as item}
			{@const Icon = item.icon}
			<div class="flex flex-col gap-3 rounded-lg bg-background px-6 py-6 shadow-xs">
				<div
					class={cn(
						"flex items-center gap-x-2",
						"[&_svg]:size-4 [&_svg]:text-muted-foreground"
					)}
				>
					<Icon />
					<h2 class="text-sm">{item.title}</h2>
				</div>
				<p class="text-sm text-muted-foreground">{item.description}</p>
				<div class="mt-1 flex items-center gap-x-2">
					<Button variant="link" href={item.href}>
						{item.label}
					</Button>
				</div>
			</div>
		{/each}
	</div>
</div>

```

###### File: `src/lib/svgs/x.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<!-- <svg fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" {...props}>
	<path
		d="m18.9,1.153h3.682l-8.042,9.189,9.46,12.506h-7.405l-5.804-7.583-6.634,7.583H.469l8.6-9.831L0,1.153h7.593l5.241,6.931,6.065-6.931Zm-1.293,19.494h2.039L6.482,3.239h-2.19l13.314,17.408Z"
	/>
</svg> -->
<svg fill="none" viewBox="0 0 1200 1227" {...props}
	><path
		fill="currentColor"
		d="M714.163 519.284 1160.89 0h-105.86L667.137 450.887 357.328 0H0l468.492 681.821L0 1226.37h105.866l409.625-476.152 327.181 476.152H1200L714.137 519.284h.026ZM569.165 687.828l-47.468-67.894-377.686-540.24h162.604l304.797 435.991 47.468 67.894 396.2 566.721H892.476L569.165 687.854v-.026Z"
	/></svg
>

```

---

### Contact 3 <a name="contact-3"></a>

A premium contact 3 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `contact-three`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/contact-three.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `local:full-width-divider`

##### Component Source Code

###### File: `src/lib/components/efferd/contact/contact-three.svelte`

```svelte
<script lang="ts">
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import GithubLogo from "$lib/svgs/github.svelte";
	import XLogo from "$lib/svgs/x.svelte";
	import { cn } from "$lib/utils";
	import { Mail, MapPin, Phone, type Icon as IconType } from "@lucide/svelte";
	import type { Component } from "svelte";

	const APP_EMAIL = "mail@example.com";
	const APP_PHONE = "+92 300 1234567";
	const APP_PHONE_2 = "+92 321 9876543";

	type ContactItem = {
		title: string;
		description: string;
		icon: typeof IconType;
		content: {
			type: "link" | "text";
			label: string;
			href?: string;
		}[];
	};

	type SocialLink = {
		icon: Component;
		href: string;
		label: string;
	};

	const contactItems: ContactItem[] = [
		{
			title: "Email",
			description: "We respond to all emails within 24 hours.",
			icon: Mail,
			content: [
				{
					type: "link",
					label: APP_EMAIL,
					href: `mailto:${APP_EMAIL}`
				}
			]
		},
		{
			title: "Office",
			description: "Drop by our office for a chat.",
			icon: MapPin,
			content: [
				{
					type: "text",
					label: "Office # 123, Main Street, Texas, USA"
				}
			]
		},
		{
			title: "Phone",
			description: "We're available Mon-Fri, 9am-5pm.",
			icon: Phone,
			content: [
				{
					type: "link",
					label: APP_PHONE,
					href: `tel:${APP_PHONE}`
				},
				{
					type: "link",
					label: APP_PHONE_2,
					href: `tel:${APP_PHONE_2}`
				}
			]
		}
	];

	const socialLinks: SocialLink[] = [
		{
			icon: GithubLogo,
			href: "/",
			label: "GitHub"
		},
		{
			icon: XLogo,
			href: "/",
			label: "Twitter"
		}
	];
</script>

<div class="relative mx-auto min-h-screen max-w-5xl overflow-x-clip border-x">
	<div class="flex grow flex-col justify-center px-4 py-18 md:items-center">
		<h1 class="text-4xl font-bold md:text-5xl">Contact Us</h1>
		<p class="mb-5 text-base text-muted-foreground">Contact the support team at efferd.</p>
	</div>

	<FullWidthDivider />

	<div class="grid md:grid-cols-3">
		{#each contactItems as item, index}
			{@const Icon = item.icon}
			<div
				class={cn(
					"flex flex-col justify-between border-b md:border-r md:border-b-0",
					index === contactItems.length - 1 && "border-b-0 md:border-r-0"
				)}
			>
				<div
					class={cn(
						"flex items-center gap-x-3 border-b bg-secondary/50 p-4 dark:bg-secondary/20",
						"[&_svg]:size-5 [&_svg]:stroke-[1.5] [&_svg]:text-muted-foreground"
					)}
				>
					<Icon />
					<h2 class="text-lg font-medium tracking-wider">{item.title}</h2>
				</div>

				<div class="flex items-center gap-x-2 p-4 py-12">
					<div>
						{#each item.content as entry}
							{#if entry.type === "link"}
								<a
									class="block font-mono text-sm font-medium tracking-wide hover:underline"
									href={entry.href}
								>
									{entry.label}
								</a>
							{:else}
								<span class="font-mono text-sm font-medium tracking-wide">
									{entry.label}
								</span>
							{/if}
						{/each}
					</div>
				</div>

				<div class="border-t p-4">
					<p class="text-sm text-muted-foreground">{item.description}</p>
				</div>
			</div>
		{/each}
	</div>

	<FullWidthDivider />

	<div class="z-1 flex h-full flex-col items-center justify-center gap-4 py-24">
		<h2
			class="text-center text-2xl font-medium tracking-tight text-muted-foreground md:text-3xl"
		>
			Find us <span class="text-foreground">online</span>
		</h2>
		<div class="flex flex-wrap items-center gap-2">
			{#each socialLinks as link}
				{@const Icon = link.icon}
				<a
					class="flex items-center gap-x-2 rounded-full border bg-card px-3 py-1.5 shadow hover:bg-accent"
					href={link.href}
					rel="noopener noreferrer"
					target="_blank"
				>
					<Icon class="size-3.5 text-muted-foreground" />
					<span class="font-mono text-xs font-medium tracking-wide">{link.label}</span>
				</a>
			{/each}
		</div>
	</div>
</div>

```

###### File: `src/lib/svgs/github.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg fill="currentColor" viewBox="0 0 1024 1024" {...props}>
	<path
		clip-rule="evenodd"
		d="M8 0C3.58 0 0 3.58 0 8C0 11.54 2.29 14.53 5.47 15.59C5.87 15.66 6.02 15.42 6.02 15.21C6.02 15.02 6.01 14.39 6.01 13.72C4 14.09 3.48 13.23 3.32 12.78C3.23 12.55 2.84 11.84 2.5 11.65C2.22 11.5 1.82 11.13 2.49 11.12C3.12 11.11 3.57 11.7 3.72 11.94C4.44 13.15 5.59 12.81 6.05 12.6C6.12 12.08 6.33 11.73 6.56 11.53C4.78 11.33 2.92 10.64 2.92 7.58C2.92 6.71 3.23 5.99 3.74 5.43C3.66 5.23 3.38 4.41 3.82 3.31C3.82 3.31 4.49 3.1 6.02 4.13C6.66 3.95 7.34 3.86 8.02 3.86C8.7 3.86 9.38 3.95 10.02 4.13C11.55 3.09 12.22 3.31 12.22 3.31C12.66 4.41 12.38 5.23 12.3 5.43C12.81 5.99 13.12 6.7 13.12 7.58C13.12 10.65 11.25 11.33 9.47 11.53C9.76 11.78 10.01 12.26 10.01 13.01C10.01 14.08 10 14.94 10 15.21C10 15.42 10.15 15.67 10.55 15.59C13.71 14.53 16 11.53 16 8C16 3.58 12.42 0 8 0Z"
		fill="currentColor"
		fill-rule="evenodd"
		transform="scale(64)"
	/>
</svg>

```

###### File: `src/lib/svgs/x.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<!-- <svg fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" {...props}>
	<path
		d="m18.9,1.153h3.682l-8.042,9.189,9.46,12.506h-7.405l-5.804-7.583-6.634,7.583H.469l8.6-9.831L0,1.153h7.593l5.241,6.931,6.065-6.931Zm-1.293,19.494h2.039L6.482,3.239h-2.19l13.314,17.408Z"
	/>
</svg> -->
<svg fill="none" viewBox="0 0 1200 1227" {...props}
	><path
		fill="currentColor"
		d="M714.163 519.284 1160.89 0h-105.86L667.137 450.887 357.328 0H0l468.492 681.821L0 1226.37h105.866l409.625-476.152 327.181 476.152H1200L714.137 519.284h.026ZM569.165 687.828l-47.468-67.894-377.686-540.24h162.604l304.797 435.991 47.468 67.894 396.2 566.721H892.476L569.165 687.854v-.026Z"
	/></svg
>

```

---

### Contact 4 <a name="contact-4"></a>

A premium contact 4 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `contact-four`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/contact-four.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `field`
  - `input`
  - `textarea`

##### Component Source Code

###### File: `src/lib/components/efferd/contact/contact-four.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { Field, FieldGroup, FieldLabel } from "$lib/components/ui/field";
	import { Input } from "$lib/components/ui/input";
	import { Textarea } from "$lib/components/ui/textarea";
	import { cn } from "$lib/utils";
	import { Icon as LucideIcon, MailIcon, PhoneIcon } from "@lucide/svelte";

	type ContactInfoItem = {
		icon: typeof LucideIcon;
		label: string;
		value: string;
		href: string;
	};

	const contactInfo: ContactInfoItem[] = [
		{
			icon: MailIcon,
			label: "Email",
			value: "mail@example.com",
			href: "mailto:mail@example.com"
		},
		{
			icon: PhoneIcon,
			label: "Phone",
			value: "+92 312 1234567",
			href: "tel:+923121234567"
		}
	];

	let fullName = $state("");
	let email = $state("");
	let phone = $state("");
	let message = $state("");
</script>

<div
	class="relative mx-auto grid h-full w-full max-w-4xl rounded-2xl border md:grid-cols-[1fr_0.70fr]"
>
	<div class="col-span-1 flex flex-col space-y-4 p-8 lg:p-10">
		<h1 class="text-2xl font-medium tracking-wide md:text-3xl">Contact With Us</h1>
		<p class="max-w-md text-sm leading-relaxed text-muted-foreground md:text-base">
			If you have any questions regarding our Services or need help, please fill out the form
			here.
		</p>
		<p class="max-w-md text-xs leading-relaxed text-muted-foreground md:text-sm">
			We do our best to respond within 1 business day.
		</p>
		<div class="grid gap-4">
			{#each contactInfo as info}
				{@const Icon = info.icon}
				<div class={cn("flex items-center gap-3 py-3")}>
					<div class="rounded-lg border bg-card p-3 shadow-xs [&_svg]:size-5">
						<Icon />
					</div>
					<div>
						<p class="font-medium">{info.label}</p>
						<a class="text-xs text-muted-foreground hover:underline" href={info.href}>
							{info.value}
						</a>
					</div>
				</div>
			{/each}
		</div>
	</div>

	<div class="col-span-1 flex items-center border-t p-8 md:border-t-0 md:border-l">
		<form class="w-full">
			<FieldGroup>
				<Field>
					<FieldLabel for="full-name">Full name</FieldLabel>
					<Input
						id="full-name"
						type="text"
						placeholder="John Doe"
						autocomplete="off"
						bind:value={fullName}
					/>
				</Field>

				<Field>
					<FieldLabel for="email">Email</FieldLabel>
					<Input
						id="email"
						type="email"
						placeholder="johndoe@example.com"
						autocomplete="off"
						bind:value={email}
					/>
				</Field>

				<Field>
					<FieldLabel for="phone">Phone</FieldLabel>
					<Input
						id="phone"
						type="tel"
						placeholder="+1 (555) 123-4567"
						autocomplete="off"
						bind:value={phone}
					/>
				</Field>

				<Field>
					<FieldLabel for="message">Message</FieldLabel>
					<Textarea
						id="message"
						placeholder="Your message"
						autocomplete="off"
						bind:value={message}
					/>
				</Field>
			</FieldGroup>

			<Button class="mt-8 w-full" type="submit">Submit</Button>
		</form>
	</div>
</div>

```

---

### Contact 5 <a name="contact-5"></a>

A premium contact 5 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `contact-five`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/contact-five.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:decor-icon`
  - `field`
  - `input`
  - `textarea`

##### Component Source Code

###### File: `src/lib/components/efferd/contact/contact-five.svelte`

```svelte
<script lang="ts">
	import { DecorIcon } from "$lib/components/ui/decor-icon";
	import { Mail, Phone, type Icon as IconType } from "@lucide/svelte";
	import ContactForm from "./contact-form.svelte";
	import { cn } from "$lib/utils";

	type ContactItem = {
		title: string;
		value: string;
		icon: typeof IconType;
	};

	const data: ContactItem[] = [
		{
			title: "Call Us Today!",
			value: "+1 (555) 123-4567",
			icon: Phone
		},
		{
			title: "Send an Email",
			value: "mail@example.com",
			icon: Mail
		}
	];
</script>

<div class="relative mx-auto w-full max-w-lg border">
	<div class="border-b px-6 py-8">
		<div class="mb-8 flex flex-col gap-2">
			<h1 class="text-xl font-semibold md:text-2xl">Get in touch</h1>
			{" "}
			<p class="text-sm text-muted-foreground">
				Have a question, feedback, or want to collaborate? <br /> We'd love to hear from you.
			</p>
		</div>

		<div class="grid gap-2 md:grid-cols-2">
			{#each data as item}
				{@const Icon = item.icon}
				<div class="flex items-center gap-4 p-2">
					<div class="[&_svg]:size-5 [&_svg]:text-muted-foreground">
						<Icon />
					</div>
					<div class={cn("flex flex-col gap-y-0.5")}>
						<h2 class="text-sm">{item.title}</h2>
						<p class="text-xs text-muted-foreground">{item.value}</p>
					</div>
				</div>
			{/each}
		</div>
	</div>

	<div class="px-6 py-8">
		<div class="mb-8 flex flex-col gap-1.5">
			<h2 class="text-xl font-medium">Send a message</h2>
			{" "}
			<p class="text-sm text-muted-foreground">
				Fill out the form below and our team will get back to you shortly.
			</p>
		</div>
		<ContactForm />
	</div>
	<DecorIcon position="top-left" />
	<DecorIcon position="top-right" />
	<DecorIcon position="bottom-left" />
	<DecorIcon position="bottom-right" />
</div>

```

###### File: `src/lib/components/efferd/contact/contact-form.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { Input } from "$lib/components/ui/input";
	import { Textarea } from "$lib/components/ui/textarea";

	import { Field, FieldGroup, FieldLabel } from "$lib/components/ui/field";
</script>

<form class="w-full">
	<FieldGroup>
		<div class="grid grid-cols-2 gap-4">
			<Field>
				<FieldLabel for="first-name">First name</FieldLabel>
				<Input autocomplete="off" id="first-name" placeholder="John" />
			</Field>
			<Field>
				<FieldLabel for="last-name">Last name</FieldLabel>
				<Input autocomplete="off" id="last-name" placeholder="Doe" />
			</Field>
		</div>
		<Field>
			<FieldLabel for="email">Email</FieldLabel>
			<Input autocomplete="off" id="email" placeholder="johndoe@example.com" type="email" />
		</Field>
		<Field>
			<FieldLabel for="phone">Phone</FieldLabel>
			<Input autocomplete="off" id="phone" placeholder="+1 (555) 123-4567" type="tel" />
		</Field>
		<Field>
			<FieldLabel for="message">Message</FieldLabel>
			<Textarea autocomplete="off" id="message" placeholder="Your message" />
		</Field>
	</FieldGroup>
	<Button class="mt-8 w-full" type="button">Submit</Button>
</form>

```

---

## CTA Blocks <a name="cta"></a>

### Cta 1 <a name="cta-1"></a>

A premium cta 1 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `cta-1`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/cta-1.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `registry/blocks/cta/1/cta.tsx`

```tsx
import { Button } from "@/components/ui/button";

export function CallToAction() {
  return (
    <div className="relative mx-auto flex w-full max-w-3xl flex-col justify-between border-x md:flex-row">
      <div className="-translate-x-1/2 -top-px pointer-events-none absolute left-1/2 w-screen border-t" />
      <div className="border-b p-4 md:border-b-0">
        <h2 className="text-center font-bold text-lg md:text-left md:text-2xl">
          Let your plans shape the future.
        </h2>
      </div>
      <div className="flex items-center justify-center gap-2 p-4 md:border-l">
        <Button variant="secondary">Contact Sales</Button>
        <Button>Get Started</Button>
      </div>
      <div className="-translate-x-1/2 -bottom-px pointer-events-none absolute left-1/2 w-screen border-b" />
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `cta-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/cta-one.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`
  - `local:full-width-divider`

##### Component Source Code

###### File: `src/lib/components/efferd/cta/cta-one.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
</script>

<div class="relative mx-auto flex w-full max-w-3xl flex-col justify-between border-x md:flex-row">
	<FullWidthDivider position="top" />

	<div class="border-b p-4 md:border-b-0">
		<h2 class="text-center text-lg font-bold md:text-left md:text-2xl">
			Let your plans shape the future.
		</h2>
	</div>

	<div class="flex items-center justify-center gap-2 p-4 md:border-l">
		<Button variant="secondary">Contact Sales</Button>
		<Button>Get Started</Button>
	</div>

	<FullWidthDivider position="bottom" />
</div>

```

---

### Cta 2 <a name="cta-2"></a>

A premium cta 2 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `cta-2`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/cta-2.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `registry/blocks/cta/2/cta.tsx`

```tsx
import { ArrowRightIcon } from "lucide-react";
import { Button } from "@/components/ui/button";

export function CallToAction() {
  return (
    <div className="relative mx-auto flex w-full max-w-3xl flex-col justify-between border-x">
      <div className="-translate-x-1/2 -top-px pointer-events-none absolute left-1/2 w-screen border-t" />
      <div className="border-b px-2 py-8">
        <h2 className="text-center font-semibold text-lg md:text-2xl">
          Plan the present. Build the future.
        </h2>
        <p className="text-balance text-center text-muted-foreground text-sm md:text-base">
          Start your journey today by clicking the button below.
        </p>
      </div>
      <div className="flex items-center justify-center gap-2 bg-secondary/80 p-4 dark:bg-secondary/40">
        <Button variant="outline">Contact Sales</Button>
        <Button>
          Get Started <ArrowRightIcon />
        </Button>
      </div>
      <div className="-translate-x-1/2 -bottom-px pointer-events-none absolute left-1/2 w-screen border-b" />
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `cta-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/cta-two.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:full-width-divider`

##### Component Source Code

###### File: `src/lib/components/efferd/cta/cta-two.svelte`

```svelte
<script lang="ts">
	import { ArrowRightIcon } from "@lucide/svelte";
	import { Button } from "$lib/components/ui/button";
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
</script>

<div class="relative mx-auto flex w-full max-w-3xl flex-col justify-between border-x">
	<FullWidthDivider position="top" />

	<div class="border-b px-2 py-8">
		<h2 class="text-center text-lg font-semibold md:text-2xl">
			Plan the present. Build the future.
		</h2>
		<p class="text-center text-sm text-balance text-muted-foreground md:text-base">
			Start your journey today by clicking the button below.
		</p>
	</div>

	<div class="flex items-center justify-center gap-2 bg-secondary/80 p-4 dark:bg-secondary/40">
		<Button variant="outline">Contact Sales</Button>
		<Button>
			Get Started
			<ArrowRightIcon data-icon="inline-end" />
		</Button>
	</div>

	<FullWidthDivider position="bottom" />
</div>

```

---

### Cta 3 <a name="cta-3"></a>

A premium cta 3 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `cta-3`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/cta-3.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `registry/blocks/cta/3/cta.tsx`

```tsx
import { ArrowRightIcon, PlusIcon } from "lucide-react";
import { Button } from "@/components/ui/button";

export function CallToAction() {
  return (
    <div className="relative mx-auto flex w-full max-w-3xl flex-col justify-between gap-y-4 border-y px-4 py-8 dark:bg-[radial-gradient(35%_80%_at_25%_0%,--theme(--color-foreground/.08),transparent)]">
      <PlusIcon
        className="absolute top-[-12.5px] left-[-11.5px] z-1 size-6"
        strokeWidth={1}
      />
      <PlusIcon
        className="absolute top-[-12.5px] right-[-11.5px] z-1 size-6"
        strokeWidth={1}
      />
      <PlusIcon
        className="absolute bottom-[-12.5px] left-[-11.5px] z-1 size-6"
        strokeWidth={1}
      />
      <PlusIcon
        className="absolute right-[-11.5px] bottom-[-12.5px] z-1 size-6"
        strokeWidth={1}
      />

      <div className="-inset-y-6 pointer-events-none absolute left-0 w-px border-l" />
      <div className="-inset-y-6 pointer-events-none absolute right-0 w-px border-r" />

      <div className="-z-10 absolute top-0 left-1/2 h-full border-l border-dashed" />

      <h2 className="text-center font-semibold text-xl md:text-3xl">
        Start for Free Today!
      </h2>
      <p className="text-balance text-center font-medium text-muted-foreground text-sm md:text-base">
        Begin your 6-day free trial today to fully explore and experience all
        the features and benefits we offer.
      </p>

      <div className="flex items-center justify-center gap-2">
        <Button variant="outline">Contact Sales</Button>
        <Button>
          Get Started <ArrowRightIcon />
        </Button>
      </div>
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `cta-three`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/cta-three.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:decor-icon`

##### Component Source Code

###### File: `src/lib/components/efferd/cta/cta-three.svelte`

```svelte
<script lang="ts">
	import { ArrowRightIcon } from "@lucide/svelte";
	import { Button } from "$lib/components/ui/button";
	import { DecorIcon } from "$lib/components/ui/decor-icon";
</script>

<div
	class="relative mx-auto flex w-full max-w-3xl flex-col justify-between gap-y-4 border-y px-4 py-8 dark:bg-[radial-gradient(35%_80%_at_25%_0%,--theme(--color-foreground/.08),transparent)]"
>
	<DecorIcon class="size-4" position="top-left" />
	<DecorIcon class="size-4" position="top-right" />
	<DecorIcon class="size-4" position="bottom-left" />
	<DecorIcon class="size-4" position="bottom-right" />

	<div class="pointer-events-none absolute -inset-y-6 -left-px w-px border-l"></div>
	<div class="pointer-events-none absolute -inset-y-6 -right-px w-px border-r"></div>

	<div class="absolute top-0 left-1/2 -z-10 h-full border-l border-dashed"></div>

	<h2 class="text-center text-xl font-semibold md:text-3xl">Start for Free Today!</h2>
	<p class="text-center text-sm font-medium text-balance text-muted-foreground md:text-base">
		Begin your 6-day free trial today to fully explore and experience all the features and
		benefits we offer.
	</p>

	<div class="flex items-center justify-center gap-2">
		<Button variant="outline">Contact Sales</Button>
		<Button>
			Get Started
			<ArrowRightIcon data-icon="inline-end" />
		</Button>
	</div>
</div>

```

---

### Cta 4 <a name="cta-4"></a>

A premium cta 4 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `cta-4`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/cta-4.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `registry/blocks/cta/4/cta.tsx`

```tsx
import { ArrowRightIcon, CreditCardIcon } from "lucide-react";
import { Button } from "@/components/ui/button";

export function CallToAction() {
  return (
    <div className="relative mx-auto flex w-full max-w-3xl flex-col justify-between gap-y-6 rounded-4xl border bg-card px-4 py-8 shadow-sm md:py-10 dark:bg-card/50">
      <div className="space-y-2">
        <h2 className="text-center font-semibold text-lg tracking-tight md:text-2xl">
          Let your plans shape the future.
        </h2>
        <p className="text-balance text-center text-muted-foreground text-sm md:text-base">
          Start your free trial today. No credit card{" "}
          <CreditCardIcon className="inline-block size-4" /> required.
        </p>
      </div>
      <div className="flex items-center justify-center gap-2">
        <Button className="shadow" variant="secondary">
          Contact Sales
        </Button>
        <Button className="shadow">
          Get Started <ArrowRightIcon />
        </Button>
      </div>
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `cta-four`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/cta-four.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `src/lib/components/efferd/cta/cta-four.svelte`

```svelte
<script lang="ts">
	import { ArrowRightIcon, CreditCardIcon } from "@lucide/svelte";
	import { Button } from "$lib/components/ui/button";
</script>

<div
	class="relative mx-auto flex w-full max-w-3xl flex-col justify-between gap-y-6 rounded-4xl border bg-card px-4 py-8 shadow-sm md:py-10 dark:bg-card/50"
>
	<div class="space-y-2">
		<h2 class="text-center text-lg font-semibold tracking-tight md:text-2xl">
			Let your plans shape the future.
		</h2>
		<p class="text-center text-sm text-balance text-muted-foreground md:text-base">
			Start your free trial today. No credit card
			<CreditCardIcon class="inline-block size-4" />
			required.
		</p>
	</div>

	<div class="flex items-center justify-center gap-2">
		<Button class="shadow" variant="secondary">Contact Sales</Button>
		<Button class="shadow">
			Get Started
			<ArrowRightIcon data-icon="inline-end" />
		</Button>
	</div>
</div>

```

---

### Cta 5 <a name="cta-5"></a>

A premium cta 5 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `cta-5`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/cta-5.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`
  - `input-group`

##### Component Source Code

###### File: `registry/blocks/cta/5/cta.tsx`

```tsx
import { ArrowRightIcon, AtSignIcon } from "lucide-react";
import { Button } from "@/components/ui/button";
import {
  InputGroup,
  InputGroupAddon,
  InputGroupInput,
} from "@/components/ui/input-group";

export function CallToAction() {
  return (
    <div className="relative mx-auto flex w-full max-w-3xl flex-col justify-between gap-y-6 border-x bg-secondary/80 px-2 py-8 md:px-4 dark:bg-secondary/40">
      <div className="-translate-x-1/2 -top-px pointer-events-none absolute left-1/2 w-screen border-t" />

      <div className="space-y-1">
        <h2 className="text-center font-semibold text-2xl tracking-tight md:text-4xl">
          Subscripe to our newsletter
        </h2>
        <p className="text-balance text-center text-muted-foreground text-sm md:text-base">
          Get the latest updates and insights delivered right to your inbox.
        </p>
      </div>
      <div className="flex items-center justify-center gap-2">
        <InputGroup className="max-w-[280px] bg-card">
          <InputGroupInput placeholder="Enter your email" />
          <InputGroupAddon>
            <AtSignIcon />
          </InputGroupAddon>
        </InputGroup>

        <Button>
          Subscribe <ArrowRightIcon />
        </Button>
      </div>
      <div className="flex items-center justify-center gap-2">
        <p className="text-muted-foreground text-sm">
          Written by{" "}
          <span className="font-medium text-foreground">real humans</span> (we
          swear).
        </p>
        <div className="-space-x-[0.45rem] flex">
          <img
            alt="Avatar 01"
            className="rounded-full ring-1 ring-background"
            height={24}
            src="https://mynaui.com/avatars/avatar-01.jpg"
            width={24}
          />
          <img
            alt="Avatar 02"
            className="rounded-full ring-1 ring-background"
            height={24}
            src="https://mynaui.com/avatars/avatar-02.jpg"
            width={24}
          />
          <img
            alt="Avatar 03"
            className="rounded-full ring-1 ring-background"
            height={24}
            src="https://mynaui.com/avatars/avatar-03.jpg"
            width={24}
          />
          <img
            alt="Avatar 04"
            className="rounded-full ring-1 ring-background"
            height={24}
            src="https://mynaui.com/avatars/avatar-04.jpg"
            width={24}
          />
        </div>
      </div>

      <div className="-translate-x-1/2 -bottom-px pointer-events-none absolute left-1/2 w-screen border-b" />
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `cta-five`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/cta-five.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:full-width-divider`
  - `input-group`

##### Component Source Code

###### File: `src/lib/components/efferd/cta/cta-five.svelte`

```svelte
<script lang="ts">
	import { ArrowRightIcon, AtSignIcon } from "@lucide/svelte";
	import { Button } from "$lib/components/ui/button";
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import { InputGroup, InputGroupAddon, InputGroupInput } from "$lib/components/ui/input-group";

	const avatars = [
		{
			alt: "Avatar 01",
			src: "https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?q=80&w=72"
		},
		{
			alt: "Avatar 02",
			src: "https://images.unsplash.com/photo-1485206412256-701ccc5b93ca?q=80&w=72"
		},
		{
			alt: "Avatar 03",
			src: "https://images.unsplash.com/photo-1527980965255-d3b416303d12?q=80&w=72"
		},
		{
			alt: "Avatar 04",
			src: "https://images.unsplash.com/photo-1610216705422-caa3fcb6d158?q=80&w=72"
		}
	];
</script>

<div
	class="relative mx-auto flex w-full max-w-3xl flex-col justify-between gap-y-6 border-x bg-secondary/80 px-2 py-8 md:px-4 dark:bg-secondary/40"
>
	<FullWidthDivider position="top" />

	<div class="space-y-1">
		<h2 class="text-center text-2xl font-semibold tracking-tight md:text-4xl">
			Subscripe to our newsletter
		</h2>
		<p class="text-center text-sm text-balance text-muted-foreground md:text-base">
			Get the latest updates and insights delivered right to your inbox.
		</p>
	</div>

	<div class="flex items-center justify-center gap-2">
		<InputGroup class="max-w-[280px] bg-card">
			<InputGroupInput placeholder="Enter your email" />
			<InputGroupAddon>
				<AtSignIcon data-icon="inline-start" />
			</InputGroupAddon>
		</InputGroup>

		<Button>
			Subscribe
			<ArrowRightIcon data-icon="inline-end" />
		</Button>
	</div>

	<div class="flex items-center justify-center gap-2">
		<p class="text-sm text-muted-foreground">
			Written by <span class="font-medium text-foreground">real humans</span> (we swear).
		</p>
		<div class="flex -space-x-[0.45rem] *:rounded-full *:ring-2 *:ring-background">
			{#each avatars as avatar}
				<img
					alt={avatar.alt}
					height="24"
					src={avatar.src}
					width="24"
					class="size-6 object-cover"
				/>
			{/each}
		</div>
	</div>

	<FullWidthDivider position="bottom" />
</div>

```

---

## FAQ Blocks <a name="faq"></a>

### Faq 1 <a name="faq-1"></a>

A premium faq 1 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `faqs-1`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/faqs-1.json
```

##### Dependencies

- **Registry Dependencies**:
  - `accordion`

##### Component Source Code

###### File: `registry/blocks/faqs/1/faqs-section.tsx`

```tsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";

export function FaqsSection() {
  return (
    <div className="mx-auto w-full max-w-3xl space-y-7 px-4 pt-16">
      <div className="space-y-2">
        <h2 className="font-semibold text-3xl md:text-4xl">
          Frequently Asked Questions
        </h2>
        <p className="max-w-2xl text-muted-foreground">
          Here are some common questions and answers that you might encounter
          when using Efferd. If you don't find the answer you're looking for,
          feel free to reach out.
        </p>
      </div>
      <Accordion
        className="-space-y-px w-full rounded-lg bg-card shadow dark:bg-card/50"
        collapsible
        defaultValue="item-1"
        type="single"
      >
        {questions.map((item) => (
          <AccordionItem
            className="relative border-x first:rounded-t-lg first:border-t last:rounded-b-lg last:border-b"
            key={item.id}
            value={item.id}
          >
            <AccordionTrigger className="px-4 py-4 text-[15px] leading-6 hover:no-underline">
              {item.title}
            </AccordionTrigger>
            <AccordionContent className="px-4 pb-4 text-muted-foreground">
              {item.content}
            </AccordionContent>
          </AccordionItem>
        ))}
      </Accordion>
      <p className="text-muted-foreground">
        Can't find what you're looking for? Contact our{" "}
        <a className="text-primary hover:underline" href="#">
          customer support team
        </a>
      </p>
    </div>
  );
}

const questions = [
  {
    id: "item-1",
    title: "What is Efferd?",
    content:
      "Efferd is a collection of beautifully crafted Shadcn UI blocks and components, designed to help developers build modern websites with ease.",
  },
  {
    id: "item-2",
    title: "Who can benefit from Efferd?",
    content:
      "Efferd is built for founders, product teams, and agencies that want to accelerate idea validation and delivery.",
  },
  {
    id: "item-3",
    title: "What features does Efferd include?",
    content:
      "Efferd offers a collaborative workspace where you can design and build beautiful web applications, with reusable UI blocks, deployment automation, and comprehensive analytics all in one place. With Efferd, you can streamline your team’s workflow and deliver high-quality websites quickly and efficiently.",
  },
  {
    id: "item-4",
    title: "Can I customize components in Efferd?",
    content:
      "Yes. Efferd offers editable design systems and code scaffolding so you can tailor blocks to your brand and workflow.",
  },
  {
    id: "item-5",
    title: "Does Efferd integrate with my existing tools?",
    content:
      "Efferd connects with popular source control, design tools, and cloud providers to fit into your current stack.",
  },
  {
    id: "item-6",
    title: "How do I get support while using Efferd?",
    content:
      "You can access detailed docs, community forums, and dedicated customer success channels for help at any time.",
  },
  {
    id: "item-7",
    title: "How do I get started with Efferd?",
    content:
      "You can access detailed docs, community forums, and dedicated customer success channels for help at any time.",
  },
];

```

#### Svelte Implementation

- **Registry Key**: `faq-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/faq-one.json
```

##### Dependencies

- **Registry Dependencies**:
  - `accordion`

##### Component Source Code

###### File: `src/lib/components/efferd/faqs/faq-one.svelte`

```svelte
<script lang="ts">
	import {
		Accordion,
		AccordionContent,
		AccordionItem,
		AccordionTrigger
	} from "$lib/components/ui/accordion";

	type Question = {
		id: string;
		title: string;
		content: string;
	};

	const questions: Question[] = [
		{
			id: "item-1",
			title: "What is Efferd?",
			content:
				"Efferd is a collection of beautifully crafted Shadcn UI blocks and components, designed to help developers build modern websites with ease."
		},
		{
			id: "item-2",
			title: "Who can benefit from Efferd?",
			content:
				"Efferd is built for founders, product teams, and agencies that want to accelerate idea validation and delivery."
		},
		{
			id: "item-3",
			title: "What features does Efferd include?",
			content:
				"Efferd offers a collaborative workspace where you can design and build beautiful web applications, with reusable UI blocks, deployment automation, and comprehensive analytics all in one place. With Efferd, you can streamline your team’s workflow and deliver high-quality websites quickly and efficiently."
		},
		{
			id: "item-4",
			title: "Can I customize components in Efferd?",
			content:
				"Yes. Efferd offers editable design systems and code scaffolding so you can tailor blocks to your brand and workflow."
		},
		{
			id: "item-5",
			title: "Does Efferd integrate with my existing tools?",
			content:
				"Efferd connects with popular source control, design tools, and cloud providers to fit into your current stack."
		},
		{
			id: "item-6",
			title: "How do I get support while using Efferd?",
			content:
				"You can access detailed docs, community forums, and dedicated customer success channels for help at any time."
		},
		{
			id: "item-7",
			title: "How do I get started with Efferd?",
			content:
				"You can access detailed docs, community forums, and dedicated customer success channels for help at any time."
		}
	];
</script>

<div class="mx-auto w-full max-w-2xl space-y-7 px-4">
	<div class="space-y-2">
		<h2 class="text-3xl font-semibold md:text-4xl">Frequently Asked Questions</h2>
		<p class="max-w-2xl text-muted-foreground">
			Here are some common questions and answers that you might encounter when using Efferd.
			If you don't find the answer you're looking for, feel free to reach out.
		</p>
	</div>

	<Accordion type="single" class="rounded-lg border">
		{#each questions as item (item.id)}
			<AccordionItem value={item.id} class="px-4">
				<AccordionTrigger
					class="py-4 hover:no-underline focus-visible:underline focus-visible:ring-0"
				>
					{item.title}
				</AccordionTrigger>
				<AccordionContent class="text-muted-foreground">
					{item.content}
				</AccordionContent>
			</AccordionItem>
		{/each}
	</Accordion>

	<p class="text-muted-foreground">
		Can't find what you're looking for? Contact our
		<a class="text-primary hover:underline" href="mailto:support@efferd.com">
			customer support team
		</a>
	</p>
</div>

```

---

### Faq 2 <a name="faq-2"></a>

A premium faq 2 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `faqs-2`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/faqs-2.json
```

##### Dependencies

- **Registry Dependencies**:
  - `accordion`

##### Component Source Code

###### File: `registry/blocks/faqs/2/faqs-section.tsx`

```tsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";

export function FaqsSection() {
  return (
    <div className="mx-auto min-h-screen w-full max-w-5xl lg:border-x">
      <div className="mx-4 grid h-[calc(100vh-3.5rem)] grid-cols-1 border-x md:mx-0 md:grid-cols-2 md:border-x-0">
        <div className="space-y-4 px-4 pt-12 pb-4 md:border-r">
          <h2 className="font-black text-3xl md:text-4xl">FAQs</h2>
          <p className="text-muted-foreground">
            Here are some common questions and answers that you might encounter
            when using Efferd.
          </p>
        </div>
        <div className="place-content-center">
          <Accordion collapsible defaultValue="item-1" type="single">
            {questions.map((item) => (
              <AccordionItem
                className="first:border-t last:border-b data-[state=open]:bg-card"
                key={item.id}
                value={item.id}
              >
                <AccordionTrigger className="px-4 py-4 text-[15px] leading-6 hover:no-underline">
                  {item.title}
                </AccordionTrigger>
                <AccordionContent className="px-4 pb-4 text-muted-foreground">
                  {item.content}
                </AccordionContent>
              </AccordionItem>
            ))}
          </Accordion>
        </div>
      </div>
      <div className="flex h-14 items-center justify-center border-t">
        <p className="text-muted-foreground">
          Can't find what you're looking for?{" "}
          <a className="text-primary hover:underline" href="#">
            Contact Us
          </a>
        </p>
      </div>
    </div>
  );
}

const questions = [
  {
    id: "item-1",
    title: "What is Efferd?",
    content:
      "Efferd is a collection of beautifully crafted Shadcn UI blocks and components, designed to help developers build modern websites with ease.",
  },
  {
    id: "item-2",
    title: "Who can benefit from Efferd?",
    content:
      "Efferd is built for founders, product teams, and agencies that want to accelerate idea validation and delivery.",
  },
  {
    id: "item-3",
    title: "What features does Efferd include?",
    content:
      "Efferd offers a collaborative workspace where you can design and build beautiful web applications, with reusable UI blocks, deployment automation, and comprehensive analytics all in one place. With Efferd, you can streamline your team’s workflow and deliver high-quality websites quickly and efficiently.",
  },
  {
    id: "item-4",
    title: "Can I customize components in Efferd?",
    content:
      "Yes. Efferd offers editable design systems and code scaffolding so you can tailor blocks to your brand and workflow.",
  },
  {
    id: "item-5",
    title: "Does Efferd integrate with my existing tools?",
    content:
      "Efferd connects with popular source control, design tools, and cloud providers to fit into your current stack.",
  },
  {
    id: "item-6",
    title: "How do I get support while using Efferd?",
    content:
      "You can access detailed docs, community forums, and dedicated customer success channels for help at any time.",
  },
  {
    id: "item-7",
    title: "How do I get started with Efferd?",
    content:
      "You can access detailed docs, community forums, and dedicated customer success channels for help at any time.",
  },
];

```

#### Svelte Implementation

- **Registry Key**: `faq-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/faq-two.json
```

##### Dependencies

- **Registry Dependencies**:
  - `accordion`

##### Component Source Code

###### File: `src/lib/components/efferd/faqs/faq-two.svelte`

```svelte
<script lang="ts">
	import {
		Accordion,
		AccordionContent,
		AccordionItem,
		AccordionTrigger
	} from "$lib/components/ui/accordion";

	type Question = {
		id: string;
		title: string;
		content: string;
	};

	const questions: Question[] = [
		{
			id: "item-1",
			title: "What is Efferd?",
			content:
				"Efferd is a collection of beautifully crafted Shadcn UI blocks and components, designed to help developers build modern websites with ease."
		},
		{
			id: "item-2",
			title: "Who can benefit from Efferd?",
			content:
				"Efferd is built for founders, product teams, and agencies that want to accelerate idea validation and delivery."
		},
		{
			id: "item-3",
			title: "What features does Efferd include?",
			content:
				"Efferd offers a collaborative workspace where you can design and build beautiful web applications, with reusable UI blocks, deployment automation, and comprehensive analytics all in one place. With Efferd, you can streamline your team’s workflow and deliver high-quality websites quickly and efficiently."
		},
		{
			id: "item-4",
			title: "Can I customize components in Efferd?",
			content:
				"Yes. Efferd offers editable design systems and code scaffolding so you can tailor blocks to your brand and workflow."
		},
		{
			id: "item-5",
			title: "Does Efferd integrate with my existing tools?",
			content:
				"Efferd connects with popular source control, design tools, and cloud providers to fit into your current stack."
		},
		{
			id: "item-6",
			title: "How do I get support while using Efferd?",
			content:
				"You can access detailed docs, community forums, and dedicated customer success channels for help at any time."
		},
		{
			id: "item-7",
			title: "How do I get started with Efferd?",
			content:
				"You can access detailed docs, community forums, and dedicated customer success channels for help at any time."
		}
	];
</script>

<div class="mx-auto min-h-screen w-full max-w-5xl lg:border-x">
	<div
		class="mx-4 grid h-[calc(100vh-3.5rem)] grid-cols-1 border-x md:mx-0 md:grid-cols-2 md:border-x-0"
	>
		<div class="space-y-4 px-4 pt-12 pb-4 md:border-r">
			<h2 class="text-3xl font-black md:text-4xl">FAQs</h2>
			<p class="text-muted-foreground">
				Here are some common questions and answers that you might encounter when using
				Efferd.
			</p>
		</div>

		<div class="place-content-center">
			<Accordion type="single" class="rounded-none border-x-0 border-y">
				{#each questions as item (item.id)}
					<AccordionItem value={item.id} class="px-4">
						<AccordionTrigger
							class="py-4 hover:no-underline focus-visible:underline focus-visible:ring-0"
						>
							{item.title}
						</AccordionTrigger>
						<AccordionContent class="pb-4 text-muted-foreground">
							{item.content}
						</AccordionContent>
					</AccordionItem>
				{/each}
			</Accordion>
		</div>
	</div>

	<div class="flex h-14 items-center justify-center border-t">
		<p class="text-muted-foreground">
			Can't find what you're looking for?
			<a class="text-primary hover:underline" href="mailto:support@efferd.com">
				Contact Us
			</a>
		</p>
	</div>
</div>

```

---

### Faq 3 <a name="faq-3"></a>

A premium faq 3 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `faqs-3`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/faqs-3.json
```

##### Dependencies

- **Registry Dependencies**:
  - `accordion`

##### Component Source Code

###### File: `registry/blocks/faqs/3/faqs-section.tsx`

```tsx
import { PlusIcon } from "lucide-react";
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";

export function FaqsSection() {
  return (
    <section className="mx-auto grid min-h-screen w-full max-w-5xl grid-cols-1 md:grid-cols-2 lg:border-x">
      <div className="px-4 pt-12 pb-6">
        <div className="space-y-5">
          <h2 className="text-balance font-bold text-4xl md:text-6xl lg:font-black">
            Frequently Asked Questions
          </h2>
          <p className="text-muted-foreground">
            Quick answers to common questions about Efferd. Open any question to
            learn more.
          </p>
          <p className="text-muted-foreground">
            {"Can't find what you're looking for? "}
            <a className="text-primary hover:underline" href="#">
              Contact Us
            </a>
          </p>
        </div>
      </div>
      <div className="relative place-content-center">
        {/* vertical guide line */}
        <div
          aria-hidden="true"
          className="pointer-events-none absolute inset-y-0 left-3 h-full w-px bg-border"
        />

        <Accordion collapsible type="single">
          {faqs.map((item) => (
            <AccordionItem
              className="group relative border-b pl-5 first:border-t last:border-b"
              key={item.id}
              value={item.id}
            >
              {/*  plus */}
              <PlusIcon
                aria-hidden="true"
                className="-bottom-[5.5px] -translate-x-1/2 pointer-events-none absolute left-[12.5px] size-2.5 text-muted-foreground group-last:hidden"
              />

              <AccordionTrigger className="px-4 py-4 text-[15px] leading-6 hover:no-underline">
                {item.title}
              </AccordionTrigger>

              <AccordionContent className="px-4 pb-4 text-muted-foreground">
                {item.content}
              </AccordionContent>
            </AccordionItem>
          ))}
        </Accordion>
      </div>
    </section>
  );
}

const faqs = [
  {
    id: "item-1",
    title: "What is Efferd?",
    content:
      "Efferd is a collection of beautifully crafted Shadcn UI blocks and components, designed to help developers build modern websites with ease.",
  },
  {
    id: "item-2",
    title: "Who can benefit from Efferd?",
    content:
      "Efferd is built for founders, product teams, and agencies that want to accelerate idea validation and delivery.",
  },
  {
    id: "item-3",
    title: "What features does Efferd include?",
    content:
      "Efferd offers a collaborative workspace where you can design and build beautiful web applications, with reusable UI blocks, deployment automation, and comprehensive analytics all in one place. With Efferd, you can streamline your team’s workflow and deliver high-quality websites quickly and efficiently.",
  },
  {
    id: "item-4",
    title: "Can I customize components in Efferd?",
    content:
      "Yes. Efferd offers editable design systems and code scaffolding so you can tailor blocks to your brand and workflow.",
  },
  {
    id: "item-5",
    title: "Does Efferd integrate with my existing tools?",
    content:
      "Efferd connects with popular source control, design tools, and cloud providers to fit into your current stack.",
  },
  {
    id: "item-6",
    title: "How do I get support while using Efferd?",
    content:
      "You can access detailed docs, community forums, and dedicated customer success channels for help at any time.",
  },
  {
    id: "item-7",
    title: "How do I get started with Efferd?",
    content:
      "You can access detailed docs, community forums, and dedicated customer success channels for help at any time.",
  },
];

```

#### Svelte Implementation

- **Registry Key**: `faq-three`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/faq-three.json
```

##### Dependencies

- **Registry Dependencies**:
  - `accordion`
  - `local:decor-icon`

##### Component Source Code

###### File: `src/lib/components/efferd/faqs/faq-three.svelte`

```svelte
<script lang="ts">
	import {
		Accordion,
		AccordionContent,
		AccordionItem,
		AccordionTrigger
	} from "$lib/components/ui/accordion";
	import { DecorIcon } from "$lib/components/ui/decor-icon";

	type FaqItem = {
		id: string;
		title: string;
		content: string;
	};

	const faqs: FaqItem[] = [
		{
			id: "item-1",
			title: "What is Efferd?",
			content:
				"Efferd is a collection of beautifully crafted Shadcn UI blocks and components, designed to help developers build modern websites with ease."
		},
		{
			id: "item-2",
			title: "Who can benefit from Efferd?",
			content:
				"Efferd is built for founders, product teams, and agencies that want to accelerate idea validation and delivery."
		},
		{
			id: "item-3",
			title: "What features does Efferd include?",
			content:
				"Efferd offers a collaborative workspace where you can design and build beautiful web applications, with reusable UI blocks, deployment automation, and comprehensive analytics all in one place. With Efferd, you can streamline your team's workflow and deliver high-quality websites quickly and efficiently."
		},
		{
			id: "item-4",
			title: "Can I customize components in Efferd?",
			content:
				"Yes. Efferd offers editable design systems and code scaffolding so you can tailor blocks to your brand and workflow."
		},
		{
			id: "item-5",
			title: "Does Efferd integrate with my existing tools?",
			content:
				"Efferd connects with popular source control, design tools, and cloud providers to fit into your current stack."
		},
		{
			id: "item-6",
			title: "How do I get support while using Efferd?",
			content:
				"You can access detailed docs, community forums, and dedicated customer success channels for help at any time."
		},
		{
			id: "item-7",
			title: "How do I get started with Efferd?",
			content:
				"You can access detailed docs, community forums, and dedicated customer success channels for help at any time."
		}
	];
</script>

<section class="mx-auto grid min-h-screen w-full max-w-5xl grid-cols-1 md:grid-cols-2 lg:border-x">
	<div class="px-4 pt-12 pb-6">
		<div class="space-y-5">
			<h2 class="text-4xl font-bold text-balance md:text-6xl lg:font-black">
				Frequently Asked Questions
			</h2>
			<p class="text-muted-foreground">
				Quick answers to common questions about Efferd. Open any question to learn more.
			</p>
			<p class="text-muted-foreground">
				Can't find what you're looking for?
				<a class="text-primary hover:underline" href="mailto:support@efferd.com">
					Contact Us
				</a>
			</p>
		</div>
	</div>

	<div class="relative place-content-center">
		<div
			aria-hidden="true"
			class="pointer-events-none absolute inset-y-0 left-3 h-full w-px bg-border"
		></div>

		<Accordion type="single" class="rounded-none border-x-0 border-y">
			{#each faqs as item (item.id)}
				<AccordionItem value={item.id} class="group relative pl-5">
					<DecorIcon
						position="bottom-left"
						class="left-[13px] size-3 group-last:hidden"
					/>

					<AccordionTrigger
						class="px-4 py-4 hover:no-underline focus-visible:underline focus-visible:ring-0"
					>
						{item.title}
					</AccordionTrigger>

					<AccordionContent class="px-4 pb-4 text-muted-foreground">
						{item.content}
					</AccordionContent>
				</AccordionItem>
			{/each}
		</Accordion>
	</div>
</section>

```

---

### Faq 4 <a name="faq-4"></a>

A premium faq 4 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `faqs-4`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/faqs-4.json
```

##### Dependencies

- **Registry Dependencies**:
  - `accordion`
  - `button`

##### Component Source Code

###### File: `registry/blocks/faqs/4/faqs-section.tsx`

```tsx
"use client";
import { CreditCard, Feather, LayoutGrid, LifeBuoy, Power } from "lucide-react";
import React from "react";
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";

const categories = [
  { icon: LayoutGrid, id: "all", label: "All Topics" },
  { icon: Power, id: "getting-started", label: "Getting Started" },
  { icon: Feather, id: "features", label: "Features" },
  { icon: CreditCard, id: "billing", label: "Billing" },
  { icon: LifeBuoy, id: "support", label: "Support" },
];

export function FaqsSection() {
  const [activeCategory, setActiveCategory] = React.useState("all");

  const filtered = faqs.filter((faq) => {
    const matchesCategory =
      activeCategory === "all" || faq.category === activeCategory;
    return matchesCategory;
  });

  const currentCategory = React.useMemo(
    () => categories.find((cat) => cat.id === activeCategory),
    [activeCategory]
  );

  return (
    <section className="mx-auto min-h-screen w-full max-w-5xl">
      <div className="flex flex-col items-center justify-center gap-4 px-4 py-16">
        <h2 className="text-balance font-black font-mono text-4xl md:text-5xl lg:font-black">
          FaQs
        </h2>
        <p className="text-muted-foreground">Your questions answered here.</p>
      </div>
      <div className="relative grid min-h-full grid-cols-1 py-12 md:grid-cols-3">
        <div className="flex h-full items-start justify-center border-b pb-2 md:border-b-0">
          <div className="flex w-max flex-wrap items-start justify-start gap-2 md:flex-col md:justify-center">
            {categories.map((cat) => (
              <Button
                className={cn(
                  "border border-transparent",
                  activeCategory === cat.id && "border-border"
                )}
                key={cat.id}
                onClick={() => setActiveCategory(cat.id)}
                variant={activeCategory === cat.id ? "outline" : "ghost"}
              >
                <cat.icon className="size-4" />
                {cat.label}
              </Button>
            ))}
          </div>
        </div>
        <div className="col-span-2 space-y-5 px-4 py-5">
          {currentCategory && (
            <div className="flex items-center gap-2">
              <currentCategory.icon className="size-4" />
              <span className="font-medium">{currentCategory.label}</span>
            </div>
          )}
          <Accordion
            className="space-y-2"
            collapsible
            defaultValue="1"
            type="single"
          >
            {filtered.map((item) => (
              <AccordionItem
                className="border-b-0"
                key={item.id}
                value={item.id.toString()}
              >
                <AccordionTrigger className="border bg-card px-4 shadow hover:no-underline">
                  {item.title}
                </AccordionTrigger>

                <AccordionContent className="px-4 py-4">
                  {item.content}
                </AccordionContent>
              </AccordionItem>
            ))}
          </Accordion>
        </div>
      </div>
    </section>
  );
}

const faqs = [
  {
    id: 1,
    category: "getting-started",
    title: "How do I create my first project?",
    content:
      'Click the "New Project" button in your dashboard, choose a template or start from scratch, customize your project name and settings, and you\'ll be ready to start building in seconds.',
  },
  {
    id: 2,
    category: "getting-started",
    title: "What are the system requirements?",
    content:
      "Efferd works on any modern web browser including Chrome, Firefox, Safari, and Edge. No special software installation is required—just visit our platform and log in.",
  },
  {
    id: 3,
    category: "features",
    title: "Can I use Efferd for team collaboration?",
    content:
      "Absolutely! Invite team members, set role-based permissions, leave comments on components, and track changes in real-time. Our collaboration features are built for teams of all sizes.",
  },
  {
    id: 4,
    category: "features",
    title: "Is there a component library?",
    content:
      "Yes, Efferd includes a comprehensive library of pre-built, customizable components. You can also create your own reusable components and share them across your projects.",
  },
  {
    id: 5,
    category: "features",
    title: "Do you support custom integrations?",
    content:
      "We support integrations with GitHub, GitLab, Figma, Slack, and major cloud providers. For custom integrations, contact our support team to discuss your needs.",
  },
  {
    id: 6,
    category: "billing",
    title: "What payment methods do you accept?",
    content:
      "We accept all major credit cards, PayPal, and bank transfers for annual plans. Invoicing is available for enterprise customers.",
  },
  {
    id: 7,
    category: "billing",
    title: "Can I change my plan anytime?",
    content:
      "Yes, you can upgrade or downgrade your plan at any time. Changes take effect immediately, and we'll prorate your billing accordingly.",
  },
  {
    id: 8,
    category: "support",
    title: "How do I report a bug?",
    content:
      "Use the in-app feedback button or email support@efferd.com with details about the issue. Our team typically responds within 24 hours.",
  },
  {
    id: 9,
    category: "support",
    title: "Do you offer training or onboarding?",
    content:
      "We provide video tutorials, documentation, and live webinars. Premium plans include personalized onboarding sessions with our support team.",
  },
];

```

#### Svelte Implementation

- **Registry Key**: `faq-four`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/faq-four.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `accordion`

##### Component Source Code

###### File: `src/lib/components/efferd/faqs/faq-four.svelte`

```svelte
<script lang="ts">
	import {
		Accordion,
		AccordionContent,
		AccordionItem,
		AccordionTrigger
	} from "$lib/components/ui/accordion";
	import { Button } from "$lib/components/ui/button";
	import {
		CreditCardIcon,
		FeatherIcon,
		LayoutGridIcon,
		LifeBuoyIcon,
		PowerIcon,
		type Icon
	} from "@lucide/svelte";
	import type { Component } from "svelte";

	type CategoryId = "all" | "getting-started" | "features" | "billing" | "support";

	type Category = {
		id: CategoryId;
		label: string;
		icon: Component | typeof Icon;
	};

	type Faq = {
		id: number;
		category: Exclude<CategoryId, "all">;
		title: string;
		content: string;
	};

	const categories: Category[] = [
		{
			icon: LayoutGridIcon,
			id: "all",
			label: "All Topics"
		},
		{
			icon: PowerIcon,
			id: "getting-started",
			label: "Getting Started"
		},
		{
			icon: FeatherIcon,
			id: "features",
			label: "Features"
		},
		{
			icon: CreditCardIcon,
			id: "billing",
			label: "Billing"
		},
		{
			icon: LifeBuoyIcon,
			id: "support",
			label: "Support"
		}
	];

	const faqs: Faq[] = [
		{
			id: 1,
			category: "getting-started",
			title: "How do I create my first project?",
			content:
				'Click the "New Project" button in your dashboard, choose a template or start from scratch, customize your project name and settings, and you\'ll be ready to start building in seconds.'
		},
		{
			id: 2,
			category: "getting-started",
			title: "What are the system requirements?",
			content:
				"Efferd works on any modern web browser including Chrome, Firefox, Safari, and Edge. No special software installation is required, just visit our platform and log in."
		},
		{
			id: 3,
			category: "features",
			title: "Can I use Efferd for team collaboration?",
			content:
				"Absolutely! Invite team members, set role-based permissions, leave comments on components, and track changes in real-time. Our collaboration features are built for teams of all sizes."
		},
		{
			id: 4,
			category: "features",
			title: "Is there a component library?",
			content:
				"Yes, Efferd includes a comprehensive library of pre-built, customizable components. You can also create your own reusable components and share them across your projects."
		},
		{
			id: 5,
			category: "features",
			title: "Do you support custom integrations?",
			content:
				"We support integrations with GitHub, GitLab, Figma, Slack, and major cloud providers. For custom integrations, contact our support team to discuss your needs."
		},
		{
			id: 6,
			category: "billing",
			title: "What payment methods do you accept?",
			content:
				"We accept all major credit cards, PayPal, and bank transfers for annual plans. Invoicing is available for enterprise customers."
		},
		{
			id: 7,
			category: "billing",
			title: "Can I change my plan anytime?",
			content:
				"Yes, you can upgrade or downgrade your plan at any time. Changes take effect immediately, and we'll prorate your billing accordingly."
		},
		{
			id: 8,
			category: "support",
			title: "How do I report a bug?",
			content:
				"Use the in-app feedback button or email support@efferd.com with details about the issue. Our team typically responds within 24 hours."
		},
		{
			id: 9,
			category: "support",
			title: "Do you offer training or onboarding?",
			content:
				"We provide video tutorials, documentation, and live webinars. Premium plans include personalized onboarding sessions with our support team."
		}
	];

	let activeCategory = $state<CategoryId>("all");

	let filteredFaqs = $derived(
		faqs.filter((faq) => activeCategory === "all" || faq.category === activeCategory)
	);

	let currentCategory = $derived(categories.find((category) => category.id === activeCategory));
</script>

<section class="mx-auto min-h-screen w-full max-w-5xl">
	<div class="flex flex-col items-center justify-center gap-4 px-4 py-16">
		<h2 class="font-mono text-4xl font-black text-balance md:text-5xl lg:font-black">FaQs</h2>
		<p class="text-muted-foreground">Your questions answered here.</p>
	</div>

	<div class="relative grid min-h-full grid-cols-1 py-12 md:grid-cols-3">
		<div class="flex h-full items-start justify-center border-b pb-2 md:border-b-0">
			<div
				class="flex w-max flex-wrap items-start justify-start gap-2 md:flex-col md:justify-center"
			>
				{#each categories as category (category.id)}
					{@const CategoryIcon = category.icon}
					<Button
						variant={activeCategory === category.id ? "outline" : "ghost"}
						onclick={() => (activeCategory = category.id)}
					>
						<CategoryIcon class="size-4" />
						{category.label}
					</Button>
				{/each}
			</div>
		</div>

		<div class="col-span-2 space-y-5 px-4 py-5">
			{#if currentCategory}
				{@const CurrentCategoryIcon = currentCategory.icon}
				<div class="flex items-center gap-2">
					<CurrentCategoryIcon class="size-4" />
					<span class="font-medium">{currentCategory.label}</span>
				</div>
			{/if}

			<Accordion type="single" class="space-y-2">
				{#each filteredFaqs as item (item.id)}
					<AccordionItem value={item.id.toString()} class="border-b-0">
						<AccordionTrigger
							class="bg-muted px-4 py-3 hover:no-underline dark:bg-muted/50"
						>
							{item.title}
						</AccordionTrigger>

						<AccordionContent class="p-4">
							{item.content}
						</AccordionContent>
					</AccordionItem>
				{/each}
			</Accordion>
		</div>
	</div>
</section>

```

---

### Faq 5 <a name="faq-5"></a>

A premium faq 5 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `faqs-5`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/faqs-5.json
```

##### Dependencies

- **Registry Dependencies**:
  - `accordion`
  - `button`
  - `input-group`
  - `empty`

##### Component Source Code

###### File: `registry/blocks/faqs/5/faqs-section.tsx`

```tsx
"use client";
import { Search, SearchSlash } from "lucide-react";
import React from "react";
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";
import { Button } from "@/components/ui/button";
import {
  Empty,
  EmptyContent,
  EmptyHeader,
  EmptyMedia,
  EmptyTitle,
} from "@/components/ui/empty";
import {
  InputGroup,
  InputGroupAddon,
  InputGroupInput,
} from "@/components/ui/input-group";
import { cn } from "@/lib/utils";

export function FaqsSection() {
  const [searchTerm, setSearchTerm] = React.useState("");
  const [activeCategory, setActiveCategory] = React.useState("all");

  const categories = [
    { id: "all", label: "All" },
    { id: "getting-started", label: "Getting Started" },
    { id: "features", label: "Features" },
    { id: "billing", label: "Billing" },
    { id: "support", label: "Support" },
  ];

  const filtered = faqs.filter((faq) => {
    const matchesCategory =
      activeCategory === "all" || faq.category === activeCategory;
    const matchesSearch =
      faq.title.toLowerCase().includes(searchTerm.toLowerCase()) ||
      faq.content.toLowerCase().includes(searchTerm.toLowerCase());
    return matchesCategory && matchesSearch;
  });

  return (
    <div className="mx-auto min-h-screen w-full max-w-3xl md:border-x">
      <div className="px-4 py-16 lg:px-6">
        <h1 className="mb-4 font-semibold text-3xl md:text-4xl">
          Frequently Asked Questions
        </h1>
        <p className="mb-8 max-w-2xl text-muted-foreground">
          Find answers to common questions about Efferd. Can't find what you're
          looking for? Our support team is here to help.
        </p>

        <InputGroup className="max-w-sm">
          <InputGroupInput
            onChange={(e) => setSearchTerm(e.target.value)}
            placeholder="Search FAQs..."
            value={searchTerm}
          />
          <InputGroupAddon>
            <Search />
          </InputGroupAddon>
        </InputGroup>
      </div>

      <div className="absolute inset-x-0 h-px bg-border" />

      <div className="flex flex-wrap gap-1 border-b px-4 md:gap-3">
        {categories.map((cat) => (
          <button
            className="flex flex-col"
            key={cat.id}
            onClick={() => setActiveCategory(cat.id)}
            type="button"
          >
            <span
              className={cn(
                "p-1 text-muted-foreground text-sm hover:text-primary md:p-2 md:text-base",
                activeCategory === cat.id && "text-primary"
              )}
            >
              {cat.label}
            </span>
            {activeCategory === cat.id && (
              <span className="h-0.5 w-full rounded-full bg-primary" />
            )}
          </button>
        ))}
      </div>

      <Accordion
        className="space-y-2 px-4 py-12 lg:px-6"
        collapsible
        defaultValue="item-1"
        type="single"
      >
        {filtered.map((faq) => (
          <AccordionItem
            className="rounded-md border bg-card/20 shadow outline-none last:border-b has-focus-visible:border-ring data-[state=open]:bg-card"
            key={faq.id}
            value={faq.id.toString()}
          >
            <AccordionTrigger className="px-4 hover:no-underline focus-visible:ring-0">
              {faq.title}
            </AccordionTrigger>
            <AccordionContent className="px-4 pt-2 pb-4 text-muted-foreground">
              {faq.content}
            </AccordionContent>
          </AccordionItem>
        ))}
      </Accordion>

      {filtered.length === 0 && (
        <Empty>
          <EmptyHeader>
            <EmptyMedia variant="icon">
              <Search />
            </EmptyMedia>
            <EmptyTitle>No FAQs found matching your search.</EmptyTitle>
          </EmptyHeader>
          <EmptyContent>
            <Button onClick={() => setSearchTerm("")} variant="outline">
              <SearchSlash />
              Clear search
            </Button>
          </EmptyContent>
        </Empty>
      )}

      <div className="flex items-center px-4 py-6 lg:px-6">
        <p className="text-muted-foreground">
          Can't find what you're looking for?{" "}
          <a className="text-primary hover:underline" href="#">
            Contact Us
          </a>
        </p>
      </div>
    </div>
  );
}

const faqs = [
  {
    id: 1,
    category: "getting-started",
    title: "How do I create my first project?",
    content:
      'Click the "New Project" button in your dashboard, choose a template or start from scratch, customize your project name and settings, and you\'ll be ready to start building in seconds.',
  },
  {
    id: 2,
    category: "getting-started",
    title: "What are the system requirements?",
    content:
      "Efferd works on any modern web browser including Chrome, Firefox, Safari, and Edge. No special software installation is required—just visit our platform and log in.",
  },
  {
    id: 3,
    category: "features",
    title: "Can I use Efferd for team collaboration?",
    content:
      "Absolutely! Invite team members, set role-based permissions, leave comments on components, and track changes in real-time. Our collaboration features are built for teams of all sizes.",
  },
  {
    id: 4,
    category: "features",
    title: "Is there a component library?",
    content:
      "Yes, Efferd includes a comprehensive library of pre-built, customizable components. You can also create your own reusable components and share them across your projects.",
  },
  {
    id: 5,
    category: "features",
    title: "Do you support custom integrations?",
    content:
      "We support integrations with GitHub, GitLab, Figma, Slack, and major cloud providers. For custom integrations, contact our support team to discuss your needs.",
  },
  {
    id: 6,
    category: "billing",
    title: "What payment methods do you accept?",
    content:
      "We accept all major credit cards, PayPal, and bank transfers for annual plans. Invoicing is available for enterprise customers.",
  },
  {
    id: 7,
    category: "billing",
    title: "Can I change my plan anytime?",
    content:
      "Yes, you can upgrade or downgrade your plan at any time. Changes take effect immediately, and we'll prorate your billing accordingly.",
  },
  {
    id: 8,
    category: "support",
    title: "How do I report a bug?",
    content:
      "Use the in-app feedback button or email support@efferd.com with details about the issue. Our team typically responds within 24 hours.",
  },
  {
    id: 9,
    category: "support",
    title: "Do you offer training or onboarding?",
    content:
      "We provide video tutorials, documentation, and live webinars. Premium plans include personalized onboarding sessions with our support team.",
  },
];

```

#### Svelte Implementation

- **Registry Key**: `faq-five`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/faq-five.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `accordion`
  - `local:full-width-divider`
  - `input-group`
  - `empty`

##### Component Source Code

###### File: `src/lib/components/efferd/faqs/faq-five.svelte`

```svelte
<script lang="ts">
	import {
		Accordion,
		AccordionContent,
		AccordionItem,
		AccordionTrigger
	} from "$lib/components/ui/accordion";
	import { Button } from "$lib/components/ui/button";
	import {
		Empty,
		EmptyContent,
		EmptyHeader,
		EmptyMedia,
		EmptyTitle
	} from "$lib/components/ui/empty";
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import { InputGroup, InputGroupAddon, InputGroupInput } from "$lib/components/ui/input-group";
	import { cn } from "$lib/utils";
	import { SearchIcon, SearchSlashIcon } from "@lucide/svelte";

	type CategoryId = "all" | "getting-started" | "features" | "billing" | "support";

	type Category = {
		id: CategoryId;
		label: string;
	};

	type Faq = {
		id: number;
		category: Exclude<CategoryId, "all">;
		title: string;
		content: string;
	};

	const categories: Category[] = [
		{ id: "all", label: "All" },
		{ id: "getting-started", label: "Getting Started" },
		{ id: "features", label: "Features" },
		{ id: "billing", label: "Billing" },
		{ id: "support", label: "Support" }
	];

	const faqs: Faq[] = [
		{
			id: 1,
			category: "getting-started",
			title: "How do I create my first project?",
			content:
				'Click the "New Project" button in your dashboard, choose a template or start from scratch, customize your project name and settings, and you\'ll be ready to start building in seconds.'
		},
		{
			id: 2,
			category: "getting-started",
			title: "What are the system requirements?",
			content:
				"Efferd works on any modern web browser including Chrome, Firefox, Safari, and Edge. No special software installation is required, just visit our platform and log in."
		},
		{
			id: 3,
			category: "features",
			title: "Can I use Efferd for team collaboration?",
			content:
				"Absolutely! Invite team members, set role-based permissions, leave comments on components, and track changes in real-time. Our collaboration features are built for teams of all sizes."
		},
		{
			id: 4,
			category: "features",
			title: "Is there a component library?",
			content:
				"Yes, Efferd includes a comprehensive library of pre-built, customizable components. You can also create your own reusable components and share them across your projects."
		},
		{
			id: 5,
			category: "features",
			title: "Do you support custom integrations?",
			content:
				"We support integrations with GitHub, GitLab, Figma, Slack, and major cloud providers. For custom integrations, contact our support team to discuss your needs."
		},
		{
			id: 6,
			category: "billing",
			title: "What payment methods do you accept?",
			content:
				"We accept all major credit cards, PayPal, and bank transfers for annual plans. Invoicing is available for enterprise customers."
		},
		{
			id: 7,
			category: "billing",
			title: "Can I change my plan anytime?",
			content:
				"Yes, you can upgrade or downgrade your plan at any time. Changes take effect immediately, and we'll prorate your billing accordingly."
		},
		{
			id: 8,
			category: "support",
			title: "How do I report a bug?",
			content:
				"Use the in-app feedback button or email support@efferd.com with details about the issue. Our team typically responds within 24 hours."
		},
		{
			id: 9,
			category: "support",
			title: "Do you offer training or onboarding?",
			content:
				"We provide video tutorials, documentation, and live webinars. Premium plans include personalized onboarding sessions with our support team."
		}
	];

	let searchTerm = $state("");
	let activeCategory = $state<CategoryId>("all");

	let filteredFaqs = $derived(
		faqs.filter((faq) => {
			const normalizedSearch = searchTerm.trim().toLowerCase();
			const matchesCategory = activeCategory === "all" || faq.category === activeCategory;
			const matchesSearch =
				faq.title.toLowerCase().includes(normalizedSearch) ||
				faq.content.toLowerCase().includes(normalizedSearch);

			return matchesCategory && matchesSearch;
		})
	);
</script>

<div class="mx-auto min-h-screen w-full max-w-3xl md:border-x">
	<div class="px-4 py-16 lg:px-6">
		<h1 class="mb-4 text-3xl font-semibold md:text-4xl">Frequently Asked Questions</h1>
		<p class="mb-8 max-w-2xl text-muted-foreground">
			Find answers to common questions about Efferd. Can't find what you're looking for? Our
			support team is here to help.
		</p>

		<InputGroup class="max-w-sm">
			<InputGroupInput bind:value={searchTerm} placeholder="Search FAQs..." />
			<InputGroupAddon>
				<SearchIcon data-icon="inline-start" />
			</InputGroupAddon>
		</InputGroup>
	</div>

	<FullWidthDivider contained={true} />

	<div class="flex flex-wrap gap-1 border-b px-4 md:gap-3">
		{#each categories as category (category.id)}
			<Button
				type="button"
				variant="ghost"
				aria-pressed={activeCategory === category.id}
				class={cn(
					"relative h-auto rounded-none px-1 py-2 text-sm text-muted-foreground shadow-none hover:text-primary md:px-2 md:text-base",
					activeCategory === category.id && "text-primary"
				)}
				onclick={() => (activeCategory = category.id)}
			>
				{category.label}
				{#if activeCategory === category.id}
					<span class="absolute inset-x-0 bottom-0 h-0.5 rounded-full bg-primary"></span>
				{/if}
			</Button>
		{/each}
	</div>

	<Accordion type="single" class="space-y-2 px-4 py-12 lg:px-6">
		{#each filteredFaqs as faq (faq.id)}
			<AccordionItem
				value={faq.id.toString()}
				class="rounded-lg border px-4 py-0 shadow-xs last:border-b"
			>
				<AccordionTrigger class="py-3 hover:no-underline">
					{faq.title}
				</AccordionTrigger>
				<AccordionContent class="pt-2 pb-4 text-muted-foreground">
					{faq.content}
				</AccordionContent>
			</AccordionItem>
		{/each}
	</Accordion>

	{#if filteredFaqs.length === 0}
		<Empty class="px-4 pb-12 lg:px-6">
			<EmptyHeader>
				<EmptyMedia variant="icon">
					<SearchIcon />
				</EmptyMedia>
				<EmptyTitle>No FAQs found matching your search.</EmptyTitle>
			</EmptyHeader>
			<EmptyContent>
				<Button variant="outline" onclick={() => (searchTerm = "")}>
					<SearchSlashIcon data-icon="inline-start" />
					Clear search
				</Button>
			</EmptyContent>
		</Empty>
	{/if}

	<div class="flex items-center px-4 py-6 lg:px-6">
		<p class="text-muted-foreground">
			Can't find what you're looking for?
			<a class="text-primary hover:underline" href="mailto:support@efferd.com">
				Contact Us
			</a>
		</p>
	</div>
</div>

```

---

## Feature Blocks <a name="feature"></a>

### Feature 1 <a name="feature-1"></a>

A premium feature 1 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `feature-1`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/feature-1.json
```

##### Dependencies

- **NPM Packages**:
  - `motion`
- **Registry Dependencies**:
  - `https://magicui.design/r/grid-pattern.json`

##### Component Source Code

###### File: `registry/blocks/feature/1/feature-card.tsx`

```tsx
import type React from "react";

// https://magicui.design/docs/components/grid-pattern
import { GridPattern } from "@/components/ui/grid-pattern";

import { cn } from "@/lib/utils";

type FeatureType = {
  title: string;
  icon: React.ComponentType<React.SVGProps<SVGSVGElement>>;
  description: string;
};

type FeatureCardPorps = React.ComponentProps<"div"> & {
  feature: FeatureType;
};

export function FeatureCard({
  feature,
  className,
  ...props
}: FeatureCardPorps) {
  return (
    <div className={cn("relative overflow-hidden p-6", className)} {...props}>
      <div className="-mt-2 -ml-20 pointer-events-none absolute top-0 left-1/2 size-full [mask-image:radial-gradient(farthest-side_at_top,white,transparent)]">
        <GridPattern
          className="absolute inset-0 size-full stroke-foreground/20"
          height={40}
          width={40}
          x={5}
        />
      </div>
      <feature.icon
        aria-hidden
        className="size-6 text-foreground/75"
        strokeWidth={1}
      />
      <h3 className="mt-10 text-sm md:text-base">{feature.title}</h3>
      <p className="relative z-20 mt-2 font-light text-muted-foreground text-xs">
        {feature.description}
      </p>
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `feature-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/feature-one.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`

##### Component Source Code

###### File: `src/lib/components/efferd/features/feature-one.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import { ActivityIcon, GlobeIcon, ShieldCheckIcon, ZapIcon, type Icon } from "@lucide/svelte";

	type Feature = {
		title: string;
		icon: typeof Icon;
		description: string;
	};

	const features: Feature[] = [
		{
			title: "Lightning Fast",
			icon: ZapIcon,
			description: "Blazing fast edge performance."
		},
		{
			title: "Secure by Design",
			icon: ShieldCheckIcon,
			description: "Security by design, zero config."
		},
		{
			title: "Real-time Sync",
			icon: ActivityIcon,
			description: "Real-time sync across devices."
		},
		{
			title: "Global Scale",
			icon: GlobeIcon,
			description: "Instant global deployment."
		}
	];
</script>

<div class="mx-auto grid max-w-5xl grid-cols-2 gap-4 py-4 md:grid-cols-4">
	{#each features as feature, index (feature.title)}
		{@const FeatureIcon = feature.icon}
		<div
			class={cn(
				"relative flex flex-col items-center justify-center p-2",
				"after:absolute after:inset-y-0 after:right-0 after:h-full after:w-px after:bg-linear-to-b after:from-foreground/6 after:via-foreground/25 after:to-foreground/6",
				"[&_svg]:size-6 [&_svg]:text-muted-foreground",
				{
					"after:hidden": index === features.length - 1,
					"after:hidden after:md:block": index === 1
				}
			)}
		>
			<FeatureIcon />
			<h3 class="mt-4 text-center text-xs font-medium md:text-sm lg:text-base">
				{feature.title}
			</h3>
			<p class="mt-1 text-center text-[10px] text-muted-foreground md:text-xs">
				{feature.description}
			</p>
		</div>
	{/each}
</div>

```

---

### Feature 2 <a name="feature-2"></a>

A premium feature 2 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `feature-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/feature-two.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `local:dashed-line`
  - `local:decor-icon`

##### Component Source Code

###### File: `src/lib/components/efferd/features/feature-two.svelte`

```svelte
<script lang="ts">
	import { DecorIcon } from "$lib/components/ui/decor-icon";
	import { DashedLine } from "$lib/components/ui/dashed-line";
	import { CommandIcon, HistoryIcon, SquareDashedIcon, type Icon } from "@lucide/svelte";

	type Feature = {
		title: string;
		icon: typeof Icon;
		description: string;
	};

	const features: Feature[] = [
		{
			title: "Auto-Save Everything",
			icon: HistoryIcon,
			description: "Write without worry, every time."
		},
		{
			title: "Drag-and-Drop Blocks",
			icon: SquareDashedIcon,
			description: "Rearrange sections with the block editor."
		},
		{
			title: "Keyboard Shortcuts",
			icon: CommandIcon,
			description: "Speed up your workflow with quick keys."
		}
	];

	const frameLines = [
		"-top-[1.5px] right-3 left-3",
		"top-3 -right-[1.5px] bottom-3",
		"top-3 bottom-3 -left-[1.5px]",
		"right-3 -bottom-[1.5px] left-3"
	] as const;
</script>

<div class="mx-auto max-w-5xl">
	<h2 class="mb-5 text-center text-2xl font-medium md:text-3xl">Ensuring your speedy workflow</h2>

	<div class="relative">
		<DecorIcon class="size-6 stroke-border stroke-2" position="top-left" />
		<DecorIcon class="size-6 stroke-border stroke-2" position="top-right" />
		<DecorIcon class="size-6 stroke-border stroke-2" position="bottom-left" />
		<DecorIcon class="size-6 stroke-border stroke-2" position="bottom-right" />

		{#each frameLines as lineClass}
			<DashedLine class={lineClass} />
		{/each}

		<div class="grid grid-cols-1 md:grid-cols-3">
			{#each features as feature (feature.title)}
				{@const FeatureIcon = feature.icon}
				<div
					class="group relative p-8 [&_svg]:size-7 [&_svg]:mask-b-from-0% [&_svg]:text-muted-foreground"
				>
					<FeatureIcon />
					<h3 class="mt-4 text-lg font-medium">{feature.title}</h3>
					<p class="text-sm leading-relaxed text-muted-foreground">
						{feature.description}
					</p>

					<DashedLine
						class="right-5 bottom-0 left-5 group-last:hidden md:top-5 md:right-0 md:bottom-5 md:left-full"
					/>
				</div>
			{/each}
		</div>
	</div>
</div>

```

---

### Feature 3 <a name="feature-3"></a>

A premium feature 3 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `feature-three`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/feature-three.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `local:full-width-divider`

##### Component Source Code

###### File: `src/lib/components/efferd/features/feature-three.svelte`

```svelte
<script lang="ts">
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import { cn } from "$lib/utils";
	import { ActivityIcon, GlobeIcon, ShieldCheckIcon, ZapIcon, type Icon } from "@lucide/svelte";

	type Feature = {
		title: string;
		icon: typeof Icon;
		description: string;
	};

	const features: Feature[] = [
		{
			title: "Lightning Fast",
			icon: ZapIcon,
			description: "Blazing fast performance with edge network optimizations."
		},
		{
			title: "Secure by Design",
			icon: ShieldCheckIcon,
			description: "Enterprise-grade security, zero configuration required."
		},
		{
			title: "Real-time Sync",
			icon: ActivityIcon,
			description: "Real-time data sync across all devices efficiently."
		},
		{
			title: "Global Scale",
			icon: GlobeIcon,
			description: "Instant global deployment to 35+ regions worldwide."
		}
	];
</script>

<div class="mx-auto min-h-screen w-full max-w-5xl place-content-center space-y-12 border-x py-4">
	<div class="relative grid grid-cols-1 gap-px bg-border md:grid-cols-2 lg:grid-cols-4">
		<FullWidthDivider position="top" />

		{#each features as feature (feature.title)}
			{@const FeatureIcon = feature.icon}
			<div
				class={cn(
					"relative flex flex-col justify-between overflow-hidden bg-background p-4 md:p-6"
				)}
			>
				<div
					class="relative z-10 flex items-center pt-4 pb-6 [&_svg]:size-5 [&_svg]:text-primary"
				>
					<FeatureIcon />
				</div>

				<div class="relative z-10 space-y-2">
					<h3 class="text-lg font-medium text-foreground">{feature.title}</h3>
					<p class="text-xs leading-relaxed text-muted-foreground">
						{feature.description}
					</p>
				</div>
			</div>
		{/each}

		<FullWidthDivider position="bottom" />
	</div>
</div>

```

---

### Feature 4 <a name="feature-4"></a>

A premium feature 4 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `feature-four`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/feature-four.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `local:decor-icon`

##### Component Source Code

###### File: `src/lib/components/efferd/features/feature-four/feature-four.svelte`

```svelte
<script lang="ts">
	import {
		FileTextIcon,
		LayoutDashboardIcon,
		ShieldCheckIcon,
		TerminalIcon,
		type Icon
	} from "@lucide/svelte";
	import FeatureCard from "./feature-card.svelte";

	type Feature = {
		title: string;
		icon: typeof Icon;
		description: string;
	};

	const features: Feature[] = [
		{
			title: "Interactive Dashboard",
			icon: LayoutDashboardIcon,
			description: "Visualize your data with drag-and-drop widgets."
		},
		{
			title: "Instant API",
			icon: TerminalIcon,
			description: "Auto-generate REST and GraphQL APIs instantly."
		},
		{
			title: "Role-Based Access",
			icon: ShieldCheckIcon,
			description: "Secure resources with granular permission controls."
		},
		{
			title: "Audit Trails",
			icon: FileTextIcon,
			description: "Track every change with comprehensive logs."
		}
	];
</script>

<div
	class="mx-auto flex min-h-screen w-full max-w-5xl flex-col justify-center gap-12 px-4 py-12 md:px-8"
>
	<div class="mx-auto max-w-2xl space-y-2 text-center">
		<h2 class="text-3xl font-medium tracking-tight md:text-5xl">Build apps faster</h2>
		<p class="text-sm leading-relaxed text-muted-foreground md:text-base">
			The complete platform for secure, scalable apps. You code, we handle the rest.
		</p>
	</div>

	<div class="grid grid-cols-1 gap-8 md:grid-cols-2 lg:grid-cols-4">
		{#each features as feature (feature.title)}
			<FeatureCard {feature} />
		{/each}
	</div>
</div>

```

###### File: `src/lib/components/efferd/features/feature-four/feature-card.svelte`

```svelte
<script lang="ts">
	import { DecorIcon } from "$lib/components/ui/decor-icon";
	import { cn } from "$lib/utils";
	import type { Icon } from "@lucide/svelte";
	import type { HTMLAttributes } from "svelte/elements";

	type Feature = {
		title: string;
		icon: typeof Icon;
		description: string;
	};

	type Props = HTMLAttributes<HTMLDivElement> & {
		feature: Feature;
	};

	let { feature, class: className, ...props }: Props = $props();
	let FeatureIcon: typeof Icon = $derived(feature.icon);
</script>

<div
	class={cn(
		"relative flex flex-col justify-between gap-6 bg-background px-6 pt-8 pb-6 shadow-xs",
		"dark:bg-[radial-gradient(50%_80%_at_25%_0%,--theme(--color-foreground/.1),transparent)]",
		className
	)}
	{...props}
>
	<div class="absolute -inset-y-4 -left-px w-px bg-border"></div>
	<div class="absolute -inset-y-4 -right-px w-px bg-border"></div>
	<div class="absolute -inset-x-4 -top-px h-px bg-border"></div>
	<div class="absolute -right-4 -bottom-px -left-4 h-px bg-border"></div>

	<DecorIcon class="size-3.5" position="top-left" />

	<div
		class={cn(
			"relative z-10 flex w-fit items-center justify-center rounded-lg border bg-muted/20 p-3",
			"[&_svg]:size-5 [&_svg]:stroke-[1.5] [&_svg]:text-foreground"
		)}
	>
		<FeatureIcon />
	</div>

	<div class="relative z-10 space-y-2">
		<h3 class="text-base font-medium text-foreground">{feature.title}</h3>
		<p class="text-xs leading-relaxed text-muted-foreground">{feature.description}</p>
	</div>
</div>

```

---

### Feature 5 <a name="feature-5"></a>

A premium feature 5 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `feature-five`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/feature-five.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `local:grid-pattern`

##### Component Source Code

###### File: `src/lib/components/efferd/features/feature-five/feature-five.svelte`

```svelte
<script lang="ts">
	import {
		CpuIcon,
		FingerprintIcon,
		PencilIcon,
		Settings2Icon,
		SparklesIcon,
		ZapIcon,
		type Icon
	} from "@lucide/svelte";
	import FeatureCard from "./feature-card.svelte";

	type Feature = {
		title: string;
		icon: typeof Icon;
		description: string;
	};

	const features: Feature[] = [
		{
			title: "Faaast",
			icon: ZapIcon,
			description: "It supports an entire helping developers and innovate."
		},
		{
			title: "Powerful",
			icon: CpuIcon,
			description: "It supports an entire helping developers and businesses."
		},
		{
			title: "Security",
			icon: FingerprintIcon,
			description: "It supports an helping developers businesses."
		},
		{
			title: "Customization",
			icon: PencilIcon,
			description: "It supports helping developers and businesses innovate."
		},
		{
			title: "Control",
			icon: Settings2Icon,
			description: "It supports helping developers and businesses innovate."
		},
		{
			title: "Built for AI",
			icon: SparklesIcon,
			description: "It supports helping developers and businesses innovate."
		}
	];
</script>

<div class="mx-auto w-full max-w-5xl space-y-8">
	<div class="mx-auto max-w-3xl text-center">
		<h2 class="text-2xl font-medium text-balance md:text-4xl lg:text-5xl">
			Power. Speed. Control.
		</h2>
		<p class="mt-4 text-sm text-balance text-muted-foreground md:text-base">
			Everything you need to build fast, secure, scalable apps.
		</p>
	</div>

	<div class="overflow-hidden rounded-lg border">
		<div class="grid grid-cols-1 gap-px bg-border sm:grid-cols-2 md:grid-cols-3">
			{#each features as feature (feature.title)}
				<FeatureCard {feature} />
			{/each}
		</div>
	</div>
</div>

```

###### File: `src/lib/components/efferd/features/feature-five/feature-card.svelte`

```svelte
<script lang="ts">
	import { GridPattern } from "$lib/components/ui/grid-pattern";
	import { cn } from "$lib/utils";
	import type { Icon } from "@lucide/svelte";
	import type { HTMLAttributes } from "svelte/elements";

	type Feature = {
		title: string;
		icon: typeof Icon;
		description: string;
	};

	type Props = HTMLAttributes<HTMLDivElement> & {
		feature: Feature;
	};

	let { feature, class: className, ...props }: Props = $props();
	let FeatureIcon: typeof Icon = $derived(feature.icon);
</script>

<div class={cn("relative overflow-hidden bg-background p-6", className)} {...props}>
	<div
		class="pointer-events-none absolute top-0 left-1/2 -mt-2 -ml-20 size-full mask-[radial-gradient(farthest-side_at_top,white,transparent)]"
	>
		<GridPattern
			class="absolute inset-0 size-full stroke-foreground/20"
			height={40}
			width={40}
			x={20}
		/>
	</div>

	<div class="[&_svg]:size-6 [&_svg]:text-foreground/75">
		<FeatureIcon />
	</div>
	<h3 class="mt-10 text-sm md:text-base">{feature.title}</h3>
	<p class="relative z-20 mt-2 text-xs font-light text-muted-foreground">
		{feature.description}
	</p>
</div>

```

---

## Footer Blocks <a name="footer"></a>

### Footer 1 <a name="footer-1"></a>

A premium footer 1 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `footer-1`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/footer-1.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `registry/blocks/footer/1/footer.tsx`

```tsx
import {
  FacebookIcon,
  GithubIcon,
  InstagramIcon,
  LinkedinIcon,
  TwitterIcon,
  YoutubeIcon,
} from "lucide-react";
import { Logo } from "@/components/logo";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";

export function Footer() {
  const company = [
    {
      title: "About Us",
      href: "#",
    },
    {
      title: "Careers",
      href: "#",
    },
    {
      title: "Brand assets",
      href: "#",
    },
    {
      title: "Privacy Policy",
      href: "#",
    },
    {
      title: "Terms of Service",
      href: "#",
    },
  ];

  const resources = [
    {
      title: "Blog",
      href: "#",
    },
    {
      title: "Help Center",
      href: "#",
    },
    {
      title: "Contact Support",
      href: "#",
    },
    {
      title: "Community",
      href: "#",
    },
    {
      title: "Security",
      href: "#",
    },
  ];

  const socialLinks = [
    {
      icon: FacebookIcon,
      link: "#",
    },
    {
      icon: GithubIcon,
      link: "#",
    },
    {
      icon: InstagramIcon,
      link: "#",
    },
    {
      icon: LinkedinIcon,
      link: "#",
    },
    {
      icon: TwitterIcon,
      link: "#",
    },
    {
      icon: YoutubeIcon,
      link: "#",
    },
  ];
  return (
    <footer className="relative">
      <div
        className={cn(
          "mx-auto max-w-5xl lg:border-x",
          "dark:bg-[radial-gradient(35%_80%_at_30%_0%,--theme(--color-foreground/.1),transparent)]"
        )}
      >
        <div className="absolute inset-x-0 h-px w-full bg-border" />
        <div className="grid max-w-5xl grid-cols-6 gap-6 p-4">
          <div className="col-span-6 flex flex-col gap-4 pt-5 md:col-span-4">
            <a className="w-max" href="#">
              <Logo className="h-5" />
            </a>
            <p className="max-w-sm text-balance font-mono text-muted-foreground text-sm">
              A comprehensive financial technology platform.
            </p>
            <div className="flex gap-2">
              {socialLinks.map((item, index) => (
                <Button
                  key={`social-${item.link}-${index}`}
                  size="icon-sm"
                  variant="outline"
                >
                  <a href={item.link} target="_blank">
                    <item.icon className="size-3.5" />
                  </a>
                </Button>
              ))}
            </div>
          </div>
          <div className="col-span-3 w-full md:col-span-1">
            <span className="text-muted-foreground text-xs">Resources</span>
            <div className="mt-2 flex flex-col gap-2">
              {resources.map(({ href, title }) => (
                <a
                  className="w-max text-sm hover:underline"
                  href={href}
                  key={title}
                >
                  {title}
                </a>
              ))}
            </div>
          </div>
          <div className="col-span-3 w-full md:col-span-1">
            <span className="text-muted-foreground text-xs">Company</span>
            <div className="mt-2 flex flex-col gap-2">
              {company.map(({ href, title }) => (
                <a
                  className="w-max text-sm hover:underline"
                  href={href}
                  key={title}
                >
                  {title}
                </a>
              ))}
            </div>
          </div>
        </div>
        <div className="absolute inset-x-0 h-px w-full bg-border" />
        <div className="flex max-w-4xl flex-col justify-between gap-2 py-4">
          <p className="text-center font-light text-muted-foreground text-sm">
            &copy; {new Date().getFullYear()} efferd, All rights reserved
          </p>
        </div>
      </div>
    </footer>
  );
}

```

###### File: `components/logo.tsx`

```tsx
import type React from "react";

export const LogoIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 24 24" {...props}>
    <path
      d="M 15.03125 23.851562 C 14.117188 23.609375 13.417969 23 13 22.101562 C 12.808594 21.691406 12.808594 21.613281 12.808594 18.375 C 12.808594 15.066406 12.808594 15.066406 13.097656 14.484375 C 13.429688 13.8125 13.898438 13.351562 14.585938 13.027344 C 15.074219 12.800781 15.074219 12.800781 18.386719 12.800781 C 21.699219 12.800781 21.699219 12.800781 22.28125 13.089844 C 22.953125 13.421875 23.414062 13.890625 23.738281 14.578125 C 23.964844 15.066406 23.964844 15.066406 23.988281 18.140625 C 24.015625 20.769531 24 21.285156 23.875 21.710938 C 23.5625 22.789062 22.769531 23.582031 21.699219 23.863281 C 20.964844 24.042969 15.746094 24.042969 15.03125 23.851562 Z M 21.679688 22.390625 C 21.863281 22.304688 22.117188 22.085938 22.246094 21.917969 C 22.480469 21.613281 22.480469 21.613281 22.480469 18.375 C 22.480469 15.136719 22.480469 15.136719 22.238281 14.824219 C 21.757812 14.1875 21.792969 14.195312 18.386719 14.195312 C 15.667969 14.195312 15.308594 14.214844 15.066406 14.34375 C 14.691406 14.542969 14.359375 14.960938 14.246094 15.355469 C 14.144531 15.753906 14.132812 20.847656 14.246094 21.328125 C 14.386719 21.9375 14.847656 22.398438 15.441406 22.519531 C 15.625 22.554688 17.027344 22.582031 18.5625 22.574219 C 21.105469 22.554688 21.375 22.539062 21.679688 22.390625 Z M 21.679688 22.390625 "
      opacity="50%"
    />
    <path d="M 2.859375 23.085938 C 2.089844 22.84375 1.324219 22.15625 0.976562 21.398438 C 0.792969 20.996094 0.785156 20.882812 0.785156 17.636719 C 0.785156 14.28125 0.785156 14.28125 1.019531 13.785156 C 1.296875 13.1875 1.855469 12.609375 2.441406 12.324219 C 2.875 12.105469 2.875 12.105469 6.195312 12.078125 C 9.4375 12.054688 9.523438 12.0625 10.03125 12.246094 C 10.71875 12.507812 11.441406 13.21875 11.730469 13.933594 C 11.9375 14.457031 11.9375 14.472656 11.9375 17.636719 C 11.9375 21.136719 11.9375 21.128906 11.371094 21.953125 C 11.03125 22.433594 10.550781 22.800781 9.925781 23.035156 C 9.480469 23.199219 9.261719 23.207031 6.335938 23.199219 C 4.035156 23.199219 3.128906 23.164062 2.859375 23.085938 Z M 2.859375 23.085938 " />
    <path d="M 13.898438 11.695312 C 13.226562 11.4375 12.503906 10.703125 12.25 10.023438 C 12.070312 9.519531 12.058594 9.433594 12.085938 6.195312 C 12.113281 2.929688 12.113281 2.867188 12.3125 2.476562 C 12.636719 1.824219 13.070312 1.386719 13.707031 1.074219 C 14.289062 0.785156 14.289062 0.785156 17.644531 0.785156 C 20.90625 0.785156 21.007812 0.792969 21.410156 0.976562 C 21.984375 1.238281 22.65625 1.890625 22.933594 2.476562 C 23.179688 2.964844 23.179688 2.964844 23.179688 6.316406 C 23.179688 9.667969 23.179688 9.667969 22.890625 10.25 C 22.578125 10.886719 22.140625 11.324219 21.488281 11.644531 C 21.097656 11.84375 21.042969 11.84375 17.734375 11.863281 C 14.492188 11.878906 14.359375 11.878906 13.898438 11.695312 Z M 13.898438 11.695312 " />
    <path d="M 2.214844 11.078125 C 1.324219 10.824219 0.609375 10.207031 0.199219 9.3125 C 0 8.894531 0 8.832031 0 5.574219 C 0 2.265625 0 2.265625 0.234375 1.769531 C 0.53125 1.132812 1.132812 0.535156 1.769531 0.238281 C 2.265625 0.00390625 2.265625 0.00390625 5.578125 0.00390625 C 8.886719 0.00390625 8.886719 0.00390625 9.386719 0.238281 C 10.019531 0.535156 10.621094 1.132812 10.917969 1.769531 C 11.152344 2.265625 11.152344 2.265625 11.152344 5.574219 C 11.152344 8.882812 11.152344 8.882812 10.925781 9.371094 C 10.605469 10.058594 10.144531 10.53125 9.472656 10.859375 C 8.886719 11.148438 8.886719 11.148438 5.75 11.164062 C 3.441406 11.183594 2.507812 11.15625 2.214844 11.078125 Z M 8.898438 9.605469 C 9.238281 9.425781 9.613281 8.988281 9.699219 8.683594 C 9.734375 8.554688 9.757812 7.117188 9.757812 5.488281 C 9.757812 2.179688 9.769531 2.207031 9.132812 1.726562 C 8.820312 1.484375 8.820312 1.484375 5.804688 1.457031 C 4.148438 1.4375 2.65625 1.457031 2.5 1.484375 C 2.34375 1.507812 2.066406 1.683594 1.882812 1.855469 C 1.375 2.335938 1.34375 2.632812 1.421875 5.976562 C 1.480469 8.816406 1.480469 8.816406 1.726562 9.128906 C 2.214844 9.773438 2.316406 9.789062 5.75 9.773438 C 8.285156 9.753906 8.660156 9.738281 8.898438 9.605469 Z M 8.898438 9.605469 " />
  </svg>
);

export const Logo = (props: React.ComponentProps<"svg">) => (
  <svg
    fill="currentColor"
    viewBox="0 0 90 24"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <defs>
      <clipPath id="a">
        <path d="M12.785 12.781h11.172V24H12.785Zm0 0" />
      </clipPath>
      <clipPath id="b">
        <path d="M.008 0h11.148v11.2H.008Zm0 0" />
      </clipPath>
    </defs>
    <g clipPath="url(#a)">
      <path
        d="M15.008 23.855c-.91-.242-1.61-.851-2.028-1.75-.19-.41-.19-.488-.19-3.73 0-3.309 0-3.309.288-3.89a3 3 0 0 1 1.485-1.458c.488-.226.488-.226 3.792-.226 3.31 0 3.31 0 3.887.289.672.332 1.133.8 1.453 1.488.227.488.227.488.25 3.563.028 2.632.012 3.148-.113 3.574-.309 1.078-1.102 1.87-2.168 2.152-.734.18-5.941.18-6.656-.012m6.637-1.46c.18-.086.433-.305.562-.473.234-.305.234-.305.234-3.547 0-3.238 0-3.238-.242-3.55-.476-.637-.441-.63-3.844-.63-2.71 0-3.07.02-3.312.149-.375.199-.707.617-.82 1.011-.098.399-.11 5.497 0 5.977.14.61.601 1.07 1.195 1.191.184.036 1.582.063 3.113.055 2.54-.02 2.809-.035 3.114-.183m0 0"
        opacity="50%"
      />
    </g>
    <path d="M2.86 23.09c-.766-.242-1.532-.93-1.88-1.688-.183-.402-.187-.515-.187-3.765 0-3.356 0-3.356.23-3.852.278-.598.836-1.176 1.422-1.46.43-.22.43-.22 3.746-.247 3.235-.023 3.32-.015 3.829.168.683.262 1.406.973 1.695 1.688.207.523.207.539.207 3.703 0 3.504 0 3.496-.567 4.32-.34.48-.82.848-1.44 1.082-.446.164-.665.172-3.583.164-2.297 0-3.203-.035-3.473-.113M13.879 11.695c-.672-.258-1.395-.992-1.645-1.672-.18-.503-.191-.59-.164-3.832.028-3.265.028-3.328.223-3.718.324-.653.758-1.09 1.395-1.403.578-.289.578-.289 3.93-.289 3.253 0 3.355.008 3.757.192.57.261 1.242.914 1.52 1.5.246.488.246.488.246 3.84 0 3.355 0 3.355-.29 3.937a2.9 2.9 0 0 1-1.398 1.395c-.39.199-.445.199-3.746.218-3.238.016-3.371.016-3.828-.168m0 0" />
    <g clipPath="url(#b)">
      <path d="M2.219 11.078c-.89-.254-1.602-.871-2.012-1.765-.2-.418-.2-.481-.2-3.743 0-3.308 0-3.308.235-3.804A3.34 3.34 0 0 1 1.773.234C2.27 0 2.27 0 5.574 0c3.301 0 3.301 0 3.801.234.633.297 1.23.895 1.527 1.532.235.496.235.496.235 3.804 0 3.313 0 3.313-.227 3.801-.32.688-.777 1.16-1.45 1.488-.585.29-.585.29-3.714.305-2.305.02-3.234-.008-3.527-.086m6.668-1.473c.34-.18.715-.617.8-.921.036-.13.06-1.567.06-3.2 0-3.308.01-3.28-.626-3.761-.312-.243-.312-.243-3.32-.27-1.653-.02-3.145 0-3.297.027-.156.024-.434.2-.617.372-.508.48-.54.777-.461 4.12.058 2.844.058 2.844.304 3.157.489.644.59.66 4.016.644 2.531-.02 2.902-.035 3.14-.168m0 0" />
    </g>
    <path d="m33.688 17.254-1.622-2.125 3.325-2.57a1.6 1.6 0 0 0-.696-.434 3 3 0 0 0-.957-.145q-.626.001-1.148.266a2.75 2.75 0 0 0-.903.707q-.379.445-.59 1.047c-.14.402-.206.84-.206 1.313q0 .655.246 1.257c.164.403.394.758.68 1.063q.432.46 1.019.734a3 3 0 0 0 1.27.278c.84 0 1.57-.243 2.199-.723q.942-.72 1.152-2.008l3.375.367a7.6 7.6 0 0 1-.879 2.387q-.614 1.053-1.504 1.797-.891.75-2.027 1.14a7.5 7.5 0 0 1-2.445.395 6.6 6.6 0 0 1-2.606-.512 6.1 6.1 0 0 1-2.039-1.402 6.3 6.3 0 0 1-1.32-2.11q-.474-1.223-.473-2.663-.001-1.44.473-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.177-.499 2.59-.5 2.195 0 3.777.973c1.059.644 1.855 1.582 2.398 2.804ZM47.973 6.684H46.69q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.254-4.239q1.26-1.296 3.48-1.297h1.23ZM54.723 6.684H53.44q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.258-4.239q1.255-1.296 3.476-1.297h1.23ZM61.21 17.254l-1.62-2.125 3.324-2.57a1.6 1.6 0 0 0-.695-.434 3 3 0 0 0-.953-.145q-.632.001-1.153.266-.52.26-.902.707-.38.445-.59 1.047-.206.603-.207 1.313 0 .655.246 1.257.252.605.68 1.063.433.46 1.023.734.586.277 1.266.278c.84 0 1.57-.243 2.2-.723q.94-.72 1.151-2.008l3.375.367a7.6 7.6 0 0 1-.878 2.387q-.614 1.053-1.504 1.797-.891.75-2.028 1.14A7.5 7.5 0 0 1 61.5 22a6.6 6.6 0 0 1-2.605-.512 6.1 6.1 0 0 1-2.04-1.402 6.3 6.3 0 0 1-1.32-2.11q-.473-1.223-.472-2.663-.001-1.44.472-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.178-.499 2.59-.5 2.199 0 3.781.973c1.055.644 1.852 1.582 2.395 2.804ZM76.805 12.348h-.758q-1.413 0-2.121.855c-.469.567-.703 1.29-.703 2.16v6.32h-3.559V8.942h3.164v2.41h.05c.317-.855.75-1.5 1.31-1.925q.838-.645 1.964-.645h.653ZM89.992 3.328v12.063c0 .89-.152 1.742-.457 2.543a6.6 6.6 0 0 1-1.285 2.113 6 6 0 0 1-1.988 1.43q-1.161.522-2.551.523a5.9 5.9 0 0 1-2.496-.523 6.1 6.1 0 0 1-1.965-1.43 6.8 6.8 0 0 1-1.293-2.125 7.3 7.3 0 0 1-.473-2.637q0-1.335.461-2.555a6.5 6.5 0 0 1 1.282-2.125 6.3 6.3 0 0 1 1.921-1.44 5.4 5.4 0 0 1 2.407-.54q.527 0 1.007.117.487.122 1.008.25v3.54a5 5 0 0 0-.851-.286 4 4 0 0 0-.953-.105q-1.285 0-2.106.879-.825.876-.824 2.293 0 1.417.809 2.296a2.65 2.65 0 0 0 2.015.875q1.359-.001 2.172-.875.811-.881.813-2.218V3.328Zm0 0" />
  </svg>
);

```

#### Svelte Implementation

- **Registry Key**: `footer-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/footer-one.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `src/lib/components/efferd/footer/footer-one.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import GithubLogo from "$lib/svgs/github.svelte";
	import Logo from "$lib/svgs/logo.svelte";
	import XLogo from "$lib/svgs/x.svelte";
	import type { Component } from "svelte";

	type NavLink = {
		href: string;
		label: string;
	};

	type SocialLink = {
		href: string;
		label: string;
		icon: Component;
	};

	const navLinks: NavLink[] = [
		{ href: "/", label: "Features" },
		{ href: "/", label: "Blog" },
		{ href: "/", label: "About" },
		{ href: "/", label: "Contact" },
		{ href: "/", label: "Licence" },
		{ href: "/", label: "Privacy" }
	];

	const socialLinks: SocialLink[] = [
		{
			href: "https://x.com",
			label: "X",
			icon: XLogo
		},
		{
			href: "https://github.com",
			label: "Github",
			icon: GithubLogo
		}
	];

	const currentYear = new Date().getFullYear();
</script>

<footer class="mx-auto max-w-5xl *:px-4 *:md:px-6">
	<div class="flex flex-col gap-6 py-6">
		<div class="flex items-center justify-between">
			<div class="flex items-center gap-2">
				<Logo class="h-4.5 w-auto" />
			</div>

			<div class="flex items-center">
				{#each socialLinks as social (social.label)}
					{@const SocialIcon = social.icon}
					<Button
						href={social.href}
						aria-label={social.label}
						size="icon-sm"
						variant="ghost"
						rel="noreferrer"
						target="_blank"
					>
						<SocialIcon class="size-4" />
					</Button>
				{/each}
			</div>
		</div>

		<nav>
			<ul class="flex flex-wrap gap-4 text-sm font-medium text-muted-foreground md:gap-6">
				{#each navLinks as link (link.label)}
					<li>
						<a class="hover:text-foreground" href={link.href}>
							{link.label}
						</a>
					</li>
				{/each}
			</ul>
		</nav>
	</div>

	<div
		class="flex items-center justify-between gap-4 border-t py-4 text-sm text-muted-foreground"
	>
		<p>&copy; {currentYear} efferd</p>

		<p class="inline-flex items-center gap-1">
			<span>Built by</span>
			<a
				aria-label="x/twitter"
				class="inline-flex items-center gap-1 text-foreground/80 hover:text-foreground hover:underline"
				href="https://x.com/Sikandar_Bhide"
				rel="noreferrer"
				target="_blank"
			>
				<img
					alt="bhide svelte"
					class="size-4 rounded-full"
					height="16"
					src="https://avatars.githubusercontent.com/u/93428946?v=4"
					width="16"
				/>
				Bhide Svelte
			</a>
		</p>
	</div>
</footer>

```

###### File: `src/lib/svgs/github.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg fill="currentColor" viewBox="0 0 1024 1024" {...props}>
	<path
		clip-rule="evenodd"
		d="M8 0C3.58 0 0 3.58 0 8C0 11.54 2.29 14.53 5.47 15.59C5.87 15.66 6.02 15.42 6.02 15.21C6.02 15.02 6.01 14.39 6.01 13.72C4 14.09 3.48 13.23 3.32 12.78C3.23 12.55 2.84 11.84 2.5 11.65C2.22 11.5 1.82 11.13 2.49 11.12C3.12 11.11 3.57 11.7 3.72 11.94C4.44 13.15 5.59 12.81 6.05 12.6C6.12 12.08 6.33 11.73 6.56 11.53C4.78 11.33 2.92 10.64 2.92 7.58C2.92 6.71 3.23 5.99 3.74 5.43C3.66 5.23 3.38 4.41 3.82 3.31C3.82 3.31 4.49 3.1 6.02 4.13C6.66 3.95 7.34 3.86 8.02 3.86C8.7 3.86 9.38 3.95 10.02 4.13C11.55 3.09 12.22 3.31 12.22 3.31C12.66 4.41 12.38 5.23 12.3 5.43C12.81 5.99 13.12 6.7 13.12 7.58C13.12 10.65 11.25 11.33 9.47 11.53C9.76 11.78 10.01 12.26 10.01 13.01C10.01 14.08 10 14.94 10 15.21C10 15.42 10.15 15.67 10.55 15.59C13.71 14.53 16 11.53 16 8C16 3.58 12.42 0 8 0Z"
		fill="currentColor"
		fill-rule="evenodd"
		transform="scale(64)"
	/>
</svg>

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

###### File: `src/lib/svgs/x.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<!-- <svg fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" {...props}>
	<path
		d="m18.9,1.153h3.682l-8.042,9.189,9.46,12.506h-7.405l-5.804-7.583-6.634,7.583H.469l8.6-9.831L0,1.153h7.593l5.241,6.931,6.065-6.931Zm-1.293,19.494h2.039L6.482,3.239h-2.19l13.314,17.408Z"
	/>
</svg> -->
<svg fill="none" viewBox="0 0 1200 1227" {...props}
	><path
		fill="currentColor"
		d="M714.163 519.284 1160.89 0h-105.86L667.137 450.887 357.328 0H0l468.492 681.821L0 1226.37h105.866l409.625-476.152 327.181 476.152H1200L714.137 519.284h.026ZM569.165 687.828l-47.468-67.894-377.686-540.24h162.604l304.797 435.991 47.468 67.894 396.2 566.721H892.476L569.165 687.854v-.026Z"
	/></svg
>

```

---

### Footer 2 <a name="footer-2"></a>

A premium footer 2 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `footer-2`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/footer-2.json
```

##### Dependencies

- **NPM Packages**:
  - `motion`

##### Component Source Code

###### File: `registry/blocks/footer/2/footer.tsx`

```tsx
"use client";
import {
  FacebookIcon,
  InstagramIcon,
  LinkedinIcon,
  YoutubeIcon,
} from "lucide-react";
import { motion, useReducedMotion } from "motion/react";
import type React from "react";
import type { ComponentProps, ReactNode } from "react";
import { Logo } from "@/components/logo";

type FooterLink = {
  title: string;
  href: string;
  icon?: React.ComponentType<{ className?: string }>;
};

type FooterSection = {
  label: string;
  links: FooterLink[];
};

const footerLinks: FooterSection[] = [
  {
    label: "Product",
    links: [
      { title: "Features", href: "#" },
      { title: "Pricing", href: "#" },
      { title: "Testimonials", href: "#" },
      { title: "Integration", href: "#" },
    ],
  },
  {
    label: "Company",
    links: [
      { title: "FAQs", href: "#" },
      { title: "About Us", href: "#" },
      { title: "Privacy Policy", href: "#" },
      { title: "Terms of Services", href: "#" },
    ],
  },
  {
    label: "Resources",
    links: [
      { title: "Blog", href: "#" },
      { title: "Changelog", href: "#" },
      { title: "Brand", href: "#" },
      { title: "Help", href: "#" },
    ],
  },
  {
    label: "Social Links",
    links: [
      { title: "Facebook", href: "#", icon: FacebookIcon },
      { title: "Instagram", href: "#", icon: InstagramIcon },
      { title: "Youtube", href: "#", icon: YoutubeIcon },
      { title: "LinkedIn", href: "#", icon: LinkedinIcon },
    ],
  },
];

export function Footer() {
  return (
    <footer className="relative mx-auto flex w-full max-w-5xl flex-col items-center justify-center rounded-t-4xl border-t bg-[radial-gradient(35%_128px_at_50%_0%,theme(backgroundColor.white/8%),transparent)] px-4 py-6 md:rounded-t-6xl md:px-6">
      <div className="-translate-x-1/2 -translate-y-1/2 absolute top-0 right-1/2 left-1/2 h-px w-1/3 rounded-full bg-foreground/20 blur" />

      <div className="grid w-full gap-8 xl:grid-cols-3 xl:gap-8">
        <AnimatedContainer className="space-y-4">
          <Logo className="h-4" />
          <p className="mt-8 text-muted-foreground text-sm md:mt-0">
            &copy; {new Date().getFullYear()} efferd, All rights reserved
          </p>
        </AnimatedContainer>

        <div className="mt-10 grid grid-cols-2 gap-8 md:grid-cols-4 xl:col-span-2 xl:mt-0">
          {footerLinks.map((section, index) => (
            <AnimatedContainer delay={0.1 + index * 0.1} key={section.label}>
              <div className="mb-10 md:mb-0">
                <h3 className="text-xs">{section.label}</h3>
                <ul className="mt-4 space-y-2 text-muted-foreground text-sm">
                  {section.links.map((link) => (
                    <li key={link.title}>
                      <a
                        className="inline-flex items-center transition-all duration-300 hover:text-foreground"
                        href={link.href}
                        key={`${section.label}-${link.title}`}
                      >
                        {link.icon && <link.icon className="me-1 size-4" />}
                        {link.title}
                      </a>
                    </li>
                  ))}
                </ul>
              </div>
            </AnimatedContainer>
          ))}
        </div>
      </div>
    </footer>
  );
}

type ViewAnimationProps = {
  delay?: number;
  className?: ComponentProps<typeof motion.div>["className"];
  children: ReactNode;
};

function AnimatedContainer({
  className,
  delay = 0.1,
  children,
}: ViewAnimationProps) {
  const shouldReduceMotion = useReducedMotion();

  if (shouldReduceMotion) {
    return children;
  }

  return (
    <motion.div
      className={className}
      initial={{ filter: "blur(4px)", translateY: -8, opacity: 0 }}
      transition={{ delay, duration: 0.8 }}
      viewport={{ once: true }}
      whileInView={{ filter: "blur(0px)", translateY: 0, opacity: 1 }}
    >
      {children}
    </motion.div>
  );
}

```

###### File: `components/logo.tsx`

```tsx
import type React from "react";

export const LogoIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 24 24" {...props}>
    <path
      d="M 15.03125 23.851562 C 14.117188 23.609375 13.417969 23 13 22.101562 C 12.808594 21.691406 12.808594 21.613281 12.808594 18.375 C 12.808594 15.066406 12.808594 15.066406 13.097656 14.484375 C 13.429688 13.8125 13.898438 13.351562 14.585938 13.027344 C 15.074219 12.800781 15.074219 12.800781 18.386719 12.800781 C 21.699219 12.800781 21.699219 12.800781 22.28125 13.089844 C 22.953125 13.421875 23.414062 13.890625 23.738281 14.578125 C 23.964844 15.066406 23.964844 15.066406 23.988281 18.140625 C 24.015625 20.769531 24 21.285156 23.875 21.710938 C 23.5625 22.789062 22.769531 23.582031 21.699219 23.863281 C 20.964844 24.042969 15.746094 24.042969 15.03125 23.851562 Z M 21.679688 22.390625 C 21.863281 22.304688 22.117188 22.085938 22.246094 21.917969 C 22.480469 21.613281 22.480469 21.613281 22.480469 18.375 C 22.480469 15.136719 22.480469 15.136719 22.238281 14.824219 C 21.757812 14.1875 21.792969 14.195312 18.386719 14.195312 C 15.667969 14.195312 15.308594 14.214844 15.066406 14.34375 C 14.691406 14.542969 14.359375 14.960938 14.246094 15.355469 C 14.144531 15.753906 14.132812 20.847656 14.246094 21.328125 C 14.386719 21.9375 14.847656 22.398438 15.441406 22.519531 C 15.625 22.554688 17.027344 22.582031 18.5625 22.574219 C 21.105469 22.554688 21.375 22.539062 21.679688 22.390625 Z M 21.679688 22.390625 "
      opacity="50%"
    />
    <path d="M 2.859375 23.085938 C 2.089844 22.84375 1.324219 22.15625 0.976562 21.398438 C 0.792969 20.996094 0.785156 20.882812 0.785156 17.636719 C 0.785156 14.28125 0.785156 14.28125 1.019531 13.785156 C 1.296875 13.1875 1.855469 12.609375 2.441406 12.324219 C 2.875 12.105469 2.875 12.105469 6.195312 12.078125 C 9.4375 12.054688 9.523438 12.0625 10.03125 12.246094 C 10.71875 12.507812 11.441406 13.21875 11.730469 13.933594 C 11.9375 14.457031 11.9375 14.472656 11.9375 17.636719 C 11.9375 21.136719 11.9375 21.128906 11.371094 21.953125 C 11.03125 22.433594 10.550781 22.800781 9.925781 23.035156 C 9.480469 23.199219 9.261719 23.207031 6.335938 23.199219 C 4.035156 23.199219 3.128906 23.164062 2.859375 23.085938 Z M 2.859375 23.085938 " />
    <path d="M 13.898438 11.695312 C 13.226562 11.4375 12.503906 10.703125 12.25 10.023438 C 12.070312 9.519531 12.058594 9.433594 12.085938 6.195312 C 12.113281 2.929688 12.113281 2.867188 12.3125 2.476562 C 12.636719 1.824219 13.070312 1.386719 13.707031 1.074219 C 14.289062 0.785156 14.289062 0.785156 17.644531 0.785156 C 20.90625 0.785156 21.007812 0.792969 21.410156 0.976562 C 21.984375 1.238281 22.65625 1.890625 22.933594 2.476562 C 23.179688 2.964844 23.179688 2.964844 23.179688 6.316406 C 23.179688 9.667969 23.179688 9.667969 22.890625 10.25 C 22.578125 10.886719 22.140625 11.324219 21.488281 11.644531 C 21.097656 11.84375 21.042969 11.84375 17.734375 11.863281 C 14.492188 11.878906 14.359375 11.878906 13.898438 11.695312 Z M 13.898438 11.695312 " />
    <path d="M 2.214844 11.078125 C 1.324219 10.824219 0.609375 10.207031 0.199219 9.3125 C 0 8.894531 0 8.832031 0 5.574219 C 0 2.265625 0 2.265625 0.234375 1.769531 C 0.53125 1.132812 1.132812 0.535156 1.769531 0.238281 C 2.265625 0.00390625 2.265625 0.00390625 5.578125 0.00390625 C 8.886719 0.00390625 8.886719 0.00390625 9.386719 0.238281 C 10.019531 0.535156 10.621094 1.132812 10.917969 1.769531 C 11.152344 2.265625 11.152344 2.265625 11.152344 5.574219 C 11.152344 8.882812 11.152344 8.882812 10.925781 9.371094 C 10.605469 10.058594 10.144531 10.53125 9.472656 10.859375 C 8.886719 11.148438 8.886719 11.148438 5.75 11.164062 C 3.441406 11.183594 2.507812 11.15625 2.214844 11.078125 Z M 8.898438 9.605469 C 9.238281 9.425781 9.613281 8.988281 9.699219 8.683594 C 9.734375 8.554688 9.757812 7.117188 9.757812 5.488281 C 9.757812 2.179688 9.769531 2.207031 9.132812 1.726562 C 8.820312 1.484375 8.820312 1.484375 5.804688 1.457031 C 4.148438 1.4375 2.65625 1.457031 2.5 1.484375 C 2.34375 1.507812 2.066406 1.683594 1.882812 1.855469 C 1.375 2.335938 1.34375 2.632812 1.421875 5.976562 C 1.480469 8.816406 1.480469 8.816406 1.726562 9.128906 C 2.214844 9.773438 2.316406 9.789062 5.75 9.773438 C 8.285156 9.753906 8.660156 9.738281 8.898438 9.605469 Z M 8.898438 9.605469 " />
  </svg>
);

export const Logo = (props: React.ComponentProps<"svg">) => (
  <svg
    fill="currentColor"
    viewBox="0 0 90 24"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <defs>
      <clipPath id="a">
        <path d="M12.785 12.781h11.172V24H12.785Zm0 0" />
      </clipPath>
      <clipPath id="b">
        <path d="M.008 0h11.148v11.2H.008Zm0 0" />
      </clipPath>
    </defs>
    <g clipPath="url(#a)">
      <path
        d="M15.008 23.855c-.91-.242-1.61-.851-2.028-1.75-.19-.41-.19-.488-.19-3.73 0-3.309 0-3.309.288-3.89a3 3 0 0 1 1.485-1.458c.488-.226.488-.226 3.792-.226 3.31 0 3.31 0 3.887.289.672.332 1.133.8 1.453 1.488.227.488.227.488.25 3.563.028 2.632.012 3.148-.113 3.574-.309 1.078-1.102 1.87-2.168 2.152-.734.18-5.941.18-6.656-.012m6.637-1.46c.18-.086.433-.305.562-.473.234-.305.234-.305.234-3.547 0-3.238 0-3.238-.242-3.55-.476-.637-.441-.63-3.844-.63-2.71 0-3.07.02-3.312.149-.375.199-.707.617-.82 1.011-.098.399-.11 5.497 0 5.977.14.61.601 1.07 1.195 1.191.184.036 1.582.063 3.113.055 2.54-.02 2.809-.035 3.114-.183m0 0"
        opacity="50%"
      />
    </g>
    <path d="M2.86 23.09c-.766-.242-1.532-.93-1.88-1.688-.183-.402-.187-.515-.187-3.765 0-3.356 0-3.356.23-3.852.278-.598.836-1.176 1.422-1.46.43-.22.43-.22 3.746-.247 3.235-.023 3.32-.015 3.829.168.683.262 1.406.973 1.695 1.688.207.523.207.539.207 3.703 0 3.504 0 3.496-.567 4.32-.34.48-.82.848-1.44 1.082-.446.164-.665.172-3.583.164-2.297 0-3.203-.035-3.473-.113M13.879 11.695c-.672-.258-1.395-.992-1.645-1.672-.18-.503-.191-.59-.164-3.832.028-3.265.028-3.328.223-3.718.324-.653.758-1.09 1.395-1.403.578-.289.578-.289 3.93-.289 3.253 0 3.355.008 3.757.192.57.261 1.242.914 1.52 1.5.246.488.246.488.246 3.84 0 3.355 0 3.355-.29 3.937a2.9 2.9 0 0 1-1.398 1.395c-.39.199-.445.199-3.746.218-3.238.016-3.371.016-3.828-.168m0 0" />
    <g clipPath="url(#b)">
      <path d="M2.219 11.078c-.89-.254-1.602-.871-2.012-1.765-.2-.418-.2-.481-.2-3.743 0-3.308 0-3.308.235-3.804A3.34 3.34 0 0 1 1.773.234C2.27 0 2.27 0 5.574 0c3.301 0 3.301 0 3.801.234.633.297 1.23.895 1.527 1.532.235.496.235.496.235 3.804 0 3.313 0 3.313-.227 3.801-.32.688-.777 1.16-1.45 1.488-.585.29-.585.29-3.714.305-2.305.02-3.234-.008-3.527-.086m6.668-1.473c.34-.18.715-.617.8-.921.036-.13.06-1.567.06-3.2 0-3.308.01-3.28-.626-3.761-.312-.243-.312-.243-3.32-.27-1.653-.02-3.145 0-3.297.027-.156.024-.434.2-.617.372-.508.48-.54.777-.461 4.12.058 2.844.058 2.844.304 3.157.489.644.59.66 4.016.644 2.531-.02 2.902-.035 3.14-.168m0 0" />
    </g>
    <path d="m33.688 17.254-1.622-2.125 3.325-2.57a1.6 1.6 0 0 0-.696-.434 3 3 0 0 0-.957-.145q-.626.001-1.148.266a2.75 2.75 0 0 0-.903.707q-.379.445-.59 1.047c-.14.402-.206.84-.206 1.313q0 .655.246 1.257c.164.403.394.758.68 1.063q.432.46 1.019.734a3 3 0 0 0 1.27.278c.84 0 1.57-.243 2.199-.723q.942-.72 1.152-2.008l3.375.367a7.6 7.6 0 0 1-.879 2.387q-.614 1.053-1.504 1.797-.891.75-2.027 1.14a7.5 7.5 0 0 1-2.445.395 6.6 6.6 0 0 1-2.606-.512 6.1 6.1 0 0 1-2.039-1.402 6.3 6.3 0 0 1-1.32-2.11q-.474-1.223-.473-2.663-.001-1.44.473-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.177-.499 2.59-.5 2.195 0 3.777.973c1.059.644 1.855 1.582 2.398 2.804ZM47.973 6.684H46.69q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.254-4.239q1.26-1.296 3.48-1.297h1.23ZM54.723 6.684H53.44q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.258-4.239q1.255-1.296 3.476-1.297h1.23ZM61.21 17.254l-1.62-2.125 3.324-2.57a1.6 1.6 0 0 0-.695-.434 3 3 0 0 0-.953-.145q-.632.001-1.153.266-.52.26-.902.707-.38.445-.59 1.047-.206.603-.207 1.313 0 .655.246 1.257.252.605.68 1.063.433.46 1.023.734.586.277 1.266.278c.84 0 1.57-.243 2.2-.723q.94-.72 1.151-2.008l3.375.367a7.6 7.6 0 0 1-.878 2.387q-.614 1.053-1.504 1.797-.891.75-2.028 1.14A7.5 7.5 0 0 1 61.5 22a6.6 6.6 0 0 1-2.605-.512 6.1 6.1 0 0 1-2.04-1.402 6.3 6.3 0 0 1-1.32-2.11q-.473-1.223-.472-2.663-.001-1.44.472-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.178-.499 2.59-.5 2.199 0 3.781.973c1.055.644 1.852 1.582 2.395 2.804ZM76.805 12.348h-.758q-1.413 0-2.121.855c-.469.567-.703 1.29-.703 2.16v6.32h-3.559V8.942h3.164v2.41h.05c.317-.855.75-1.5 1.31-1.925q.838-.645 1.964-.645h.653ZM89.992 3.328v12.063c0 .89-.152 1.742-.457 2.543a6.6 6.6 0 0 1-1.285 2.113 6 6 0 0 1-1.988 1.43q-1.161.522-2.551.523a5.9 5.9 0 0 1-2.496-.523 6.1 6.1 0 0 1-1.965-1.43 6.8 6.8 0 0 1-1.293-2.125 7.3 7.3 0 0 1-.473-2.637q0-1.335.461-2.555a6.5 6.5 0 0 1 1.282-2.125 6.3 6.3 0 0 1 1.921-1.44 5.4 5.4 0 0 1 2.407-.54q.527 0 1.007.117.487.122 1.008.25v3.54a5 5 0 0 0-.851-.286 4 4 0 0 0-.953-.105q-1.285 0-2.106.879-.825.876-.824 2.293 0 1.417.809 2.296a2.65 2.65 0 0 0 2.015.875q1.359-.001 2.172-.875.811-.881.813-2.218V3.328Zm0 0" />
  </svg>
);

```

#### Svelte Implementation

- **Registry Key**: `footer-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/footer-two.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `src/lib/components/efferd/footer/footer-two.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { cn } from "$lib/utils";
	import Logo from "$lib/svgs/logo.svelte";
	import XLogo from "$lib/svgs/x.svelte";
	import {
		FacebookIcon,
		GithubIcon,
		InstagramIcon,
		LinkedinIcon,
		YoutubeIcon
	} from "@lucide/svelte";
	import type { Component } from "svelte";

	type FooterLink = {
		title: string;
		href: string;
	};

	type SocialLink = {
		link: string;
		icon: Component;
		label: string;
	};

	const company: FooterLink[] = [
		{
			title: "About Us",
			href: "/"
		},
		{
			title: "Careers",
			href: "/"
		},
		{
			title: "Brand assets",
			href: "/"
		},
		{
			title: "Privacy Policy",
			href: "/"
		},
		{
			title: "Terms of Service",
			href: "/"
		}
	];

	const resources: FooterLink[] = [
		{
			title: "Blog",
			href: "/"
		},
		{
			title: "Help Center",
			href: "/"
		},
		{
			title: "Contact Support",
			href: "/"
		},
		{
			title: "Community",
			href: "/"
		},
		{
			title: "Security",
			href: "/"
		}
	];

	const socialLinks: SocialLink[] = [
		{
			icon: FacebookIcon,
			link: "https://facebook.com",
			label: "Facebook"
		},
		{
			icon: GithubIcon,
			link: "https://github.com",
			label: "Github"
		},
		{
			icon: InstagramIcon,
			link: "https://instagram.com",
			label: "Instagram"
		},
		{
			icon: LinkedinIcon,
			link: "https://linkedin.com",
			label: "LinkedIn"
		},
		{
			icon: XLogo,
			link: "https://x.com",
			label: "X"
		},
		{
			icon: YoutubeIcon,
			link: "https://youtube.com",
			label: "YouTube"
		}
	];

	const currentYear = new Date().getFullYear();
</script>

<footer class="relative">
	<div
		class={cn(
			"mx-auto max-w-5xl lg:border-x",
			"dark:bg-[radial-gradient(35%_80%_at_15%_0%,--theme(--color-foreground/.1),transparent)]"
		)}
	>
		<div class="absolute inset-x-0 h-px w-full bg-border"></div>

		<div class="grid max-w-5xl grid-cols-6 gap-6 p-4">
			<div class="col-span-6 flex flex-col gap-4 pt-5 md:col-span-4">
				<a class="w-max" href="/">
					<Logo class="h-5 w-auto" />
				</a>
				<p class="max-w-sm text-sm text-balance text-muted-foreground">
					Beautify your app with efferd.
				</p>
				<div class="flex gap-2">
					{#each socialLinks as item, index (`social-${item.link}-${index}`)}
						{@const SocialIcon = item.icon}
						<Button
							href={item.link}
							aria-label={item.label}
							rel="noreferrer"
							size="icon-sm"
							target="_blank"
							variant="outline"
						>
							<SocialIcon class="size-4" />
						</Button>
					{/each}
				</div>
			</div>

			<div class="col-span-3 w-full md:col-span-1">
				<span class="text-xs text-muted-foreground">Resources</span>
				<div class="mt-2 flex flex-col gap-2">
					{#each resources as item (item.title)}
						<a class="w-max text-sm hover:underline" href={item.href}>
							{item.title}
						</a>
					{/each}
				</div>
			</div>

			<div class="col-span-3 w-full md:col-span-1">
				<span class="text-xs text-muted-foreground">Company</span>
				<div class="mt-2 flex flex-col gap-2">
					{#each company as item (item.title)}
						<a class="w-max text-sm hover:underline" href={item.href}>
							{item.title}
						</a>
					{/each}
				</div>
			</div>
		</div>

		<div class="absolute inset-x-0 h-px w-full bg-border"></div>

		<div class="flex max-w-4xl flex-col justify-between gap-2 py-4">
			<p class="text-center text-sm font-light text-muted-foreground">
				&copy; {currentYear} efferd, All rights reserved
			</p>
		</div>
	</div>
</footer>

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

###### File: `src/lib/svgs/x.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<!-- <svg fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" {...props}>
	<path
		d="m18.9,1.153h3.682l-8.042,9.189,9.46,12.506h-7.405l-5.804-7.583-6.634,7.583H.469l8.6-9.831L0,1.153h7.593l5.241,6.931,6.065-6.931Zm-1.293,19.494h2.039L6.482,3.239h-2.19l13.314,17.408Z"
	/>
</svg> -->
<svg fill="none" viewBox="0 0 1200 1227" {...props}
	><path
		fill="currentColor"
		d="M714.163 519.284 1160.89 0h-105.86L667.137 450.887 357.328 0H0l468.492 681.821L0 1226.37h105.866l409.625-476.152 327.181 476.152H1200L714.137 519.284h.026ZM569.165 687.828l-47.468-67.894-377.686-540.24h162.604l304.797 435.991 47.468 67.894 396.2 566.721H892.476L569.165 687.854v-.026Z"
	/></svg
>

```

---

### Footer 3 <a name="footer-3"></a>

A premium footer 3 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `footer-3`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/footer-3.json
```

##### Component Source Code

###### File: `registry/blocks/footer/3/footer.tsx`

```tsx
"use client";
import { ArrowRight } from "lucide-react";
import { cn } from "@/lib/utils";

export function Footer() {
  return (
    <footer className="border-t bg-[radial-gradient(35%_128px_at_50%_0%,theme(backgroundColor.white/8%),transparent)]">
      <div className="relative mx-auto max-w-5xl px-4">
        <div className="relative grid grid-cols-1 border-x md:grid-cols-4 md:divide-x">
          <div>
            <SocialCard className="border-t-0" href="#" title="Facebook" />
            <LinksGroup
              links={[
                { title: "Pricing", href: "#" },
                { title: "Testimonials", href: "#" },
                { title: "FAQs", href: "#" },
                { title: "Contact Us", href: "#" },
                { title: "Blog", href: "#" },
              ]}
              title="About Us"
            />
          </div>
          <div>
            <SocialCard href="#" title="Youtube" />
            <LinksGroup
              links={[
                { title: "Help Center", href: "#" },
                { title: "Terms", href: "#" },
                { title: "Privacy", href: "#" },
                { title: "Security", href: "#" },
                { title: "Cookie Policy", href: "#" },
              ]}
              title="Support"
            />
          </div>

          <div>
            <SocialCard href="#" title="Twitter" />
            <LinksGroup
              links={[
                { title: "Forum", href: "#" },
                { title: "Events", href: "#" },
                { title: "Partners", href: "#" },
                { title: "Affiliates", href: "#" },
                { title: "Career", href: "#" },
              ]}
              title="Community"
            />
          </div>
          <div>
            <SocialCard href="#" title="Instagram" />
            <LinksGroup
              links={[
                { title: "Investors", href: "#" },
                { title: "Terms of Use", href: "#" },
                { title: "Privacy Policy", href: "#" },
                { title: "Cookie Policy", href: "#" },
                { title: "Legal", href: "#" },
              ]}
              title="Press"
            />
          </div>
        </div>
      </div>
      <div className="flex justify-center border-t p-3">
        <p className="text-muted-foreground text-xs">
          &copy; {new Date().getFullYear()} efferd, All rights reserved
        </p>
      </div>
    </footer>
  );
}

type LinksGroupProps = {
  title: string;
  links: { title: string; href: string }[];
};
function LinksGroup({ title, links }: LinksGroupProps) {
  return (
    <div className="p-2">
      <h3 className="mt-2 mb-4 font-medium text-foreground/75 text-xs uppercase tracking-wider">
        {title}
      </h3>
      <ul>
        {links.map((link) => (
          <li key={link.title}>
            <a
              className="text-muted-foreground text-xs hover:text-foreground"
              href={link.href}
            >
              {link.title}
            </a>
          </li>
        ))}
      </ul>
    </div>
  );
}

function SocialCard({
  title,
  href,
  className,
}: {
  title: string;
  href: string;
  className?: string;
}) {
  return (
    <a
      className={cn(
        "flex items-center justify-between border-y p-2 text-sm hover:bg-accent hover:text-accent-foreground md:border-t-0",
        className
      )}
      href={href}
    >
      <span className="font-medium">{title}</span>
      <ArrowRight className="h-4 w-4 transition-colors" />
    </a>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `footer-three`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/footer-three.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
  - `motion-sv`

##### Component Source Code

###### File: `src/lib/components/efferd/footer/footer-three.svelte`

```svelte
<script lang="ts">
	import AnimatedContainer from "$lib/components/efferd/footer/AnimatedContainer.svelte";
	import { cn } from "$lib/utils";
	import Logo from "$lib/svgs/logo.svelte";
	import {
		FacebookIcon,
		InstagramIcon,
		LinkedinIcon,
		YoutubeIcon,
		type Icon
	} from "@lucide/svelte";

	type FooterLink = {
		title: string;
		href: string;
		icon?: typeof Icon;
	};

	type FooterSection = {
		label: string;
		links: FooterLink[];
	};

	const footerLinks: FooterSection[] = [
		{
			label: "Product",
			links: [
				{ title: "Features", href: "/" },
				{ title: "Pricing", href: "/" },
				{ title: "Testimonials", href: "/" },
				{ title: "Integration", href: "/" }
			]
		},
		{
			label: "Company",
			links: [
				{ title: "FAQs", href: "/" },
				{ title: "About Us", href: "/" },
				{ title: "Privacy Policy", href: "/" },
				{ title: "T&S", href: "/" }
			]
		},
		{
			label: "Resources",
			links: [
				{ title: "Blog", href: "/" },
				{ title: "Changelog", href: "/" },
				{ title: "Brand", href: "/" },
				{ title: "Help", href: "/" }
			]
		},
		{
			label: "Social Links",
			links: [
				{
					title: "Facebook",
					href: "https://facebook.com",
					icon: FacebookIcon
				},
				{
					title: "Instagram",
					href: "https://instagram.com",
					icon: InstagramIcon
				},
				{
					title: "Youtube",
					href: "https://youtube.com",
					icon: YoutubeIcon
				},
				{
					title: "LinkedIn",
					href: "https://linkedin.com",
					icon: LinkedinIcon
				}
			]
		}
	];

	const currentYear = new Date().getFullYear();
</script>

<footer
	class={cn(
		"md:rounded-t-6xl relative mx-auto flex w-full max-w-5xl flex-col items-center justify-center rounded-t-4xl border-t px-6 md:px-8",
		"dark:bg-[radial-gradient(35%_128px_at_50%_0%,--theme(--color-foreground/.1),transparent)]"
	)}
>
	<div
		class="absolute top-0 right-1/2 left-1/2 h-px w-1/3 -translate-x-1/2 -translate-y-1/2 rounded-full bg-foreground/20 blur"
	></div>

	<div class="grid w-full gap-8 py-6 md:py-8 lg:grid-cols-3 lg:gap-8">
		<AnimatedContainer class="space-y-4">
			<Logo class="h-4 w-auto" />
			<p class="mt-8 text-sm text-muted-foreground md:mt-0">Beautify your app with efferd.</p>
		</AnimatedContainer>

		<div class="mt-10 grid grid-cols-2 gap-8 md:grid-cols-4 lg:col-span-2 lg:mt-0">
			{#each footerLinks as section, index (section.label)}
				<AnimatedContainer delay={0.1 + index * 0.1}>
					<div class="mb-10 md:mb-0">
						<h3 class="text-xs">{section.label}</h3>
						<ul class="mt-4 space-y-2 text-sm text-muted-foreground">
							{#each section.links as link (`${section.label}-${link.title}`)}
								<li>
									<a
										class="inline-flex items-center duration-250 hover:text-foreground [&_svg]:me-1 [&_svg]:size-4"
										href={link.href}
										rel={link.icon ? "noreferrer" : undefined}
										target={link.icon ? "_blank" : undefined}
									>
										{#if link.icon}
											{@const LinkIcon = link.icon}
											<LinkIcon />
										{/if}
										{link.title}
									</a>
								</li>
							{/each}
						</ul>
					</div>
				</AnimatedContainer>
			{/each}
		</div>
	</div>

	<div class="h-px w-full bg-linear-to-r via-border"></div>

	<div class="flex w-full items-center justify-center py-4">
		<p class="text-sm text-muted-foreground">
			&copy; {currentYear} efferd, All rights reserved
		</p>
	</div>
</footer>

```

###### File: `src/lib/components/efferd/footer/AnimatedContainer.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import { motion } from "motion-sv";
	import type { Snippet } from "svelte";

	type Props = {
		class?: string;
		delay?: number;
		children?: Snippet;
	};

	let { class: className, delay = 0.1, children }: Props = $props();
</script>

<motion.div
	class={cn(className)}
	initial={{ filter: "blur(4px)", translateY: -8, opacity: 0 }}
	transition={{ delay, duration: 0.8 }}
	whileInView={{ filter: "blur(0px)", translateY: 0, opacity: 1 }}
>
	{@render children?.()}
</motion.div>

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

---

### Footer 4 <a name="footer-4"></a>

A premium footer 4 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `footer-4`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/footer-4.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `registry/blocks/footer/4/footer.tsx`

```tsx
"use client";

import {
  FacebookIcon,
  InstagramIcon,
  LinkedinIcon,
  TwitterIcon,
} from "lucide-react";
import { Button } from "@/components/ui/button";

const footerLinks = [
  {
    title: "Company",
    links: [
      { href: "#", label: "The Linomore Blog" },
      { href: "#", label: "Engineering Blog" },
      { href: "#", label: "Marketplace" },
      { href: "#", label: "What’s New" },
      { href: "#", label: "About" },
      { href: "#", label: "Press" },
      { href: "#", label: "Careers" },
      { href: "#", label: "Link in Bio" },
      { href: "#", label: "Social Good" },
    ],
  },
  {
    title: "Community",
    links: [
      { href: "#", label: "Linktree for Enterprise" },
      { href: "#", label: "2023 Creator Report" },
      { href: "#", label: "2022 Creator Report" },
      { href: "#", label: "Charities" },
      { href: "#", label: "What’s Trending" },
      { href: "#", label: "Creator Profile Directory" },
      { href: "#", label: "Explore Templates" },
    ],
  },
  {
    title: "Support",
    links: [
      { href: "#", label: "Help Topics" },
      { href: "#", label: "Getting Started" },
      { href: "#", label: "Linoree Pro" },
      { href: "#", label: "Features & How-tos" },
      { href: "#", label: "FAQs" },
      { href: "#", label: "Report a Violation" },
    ],
  },
  {
    title: "Legal",
    links: [
      { href: "#", label: "Terms & Conditions" },
      { href: "#", label: "Privacy Notice" },
      { href: "#", label: "Cookie Notice" },
      { href: "#", label: "Trust Center" },
      { href: "#", label: "Cookie Preferences" },
      { href: "#", label: "Transparency Report" },
      { href: "#", label: "Law Enforcement Access Policy" },
    ],
  },
];

const socialLinks = [
  { icon: FacebookIcon, href: "#" },
  { icon: InstagramIcon, href: "#" },
  { icon: LinkedinIcon, href: "#" },
  { icon: TwitterIcon, href: "#" },
];

export function Footer() {
  return (
    <footer className="border-t">
      <div className="mx-auto max-w-6xl px-4 lg:px-6">
        {/* Grid container with headings and links */}
        <div className="grid grid-cols-2 gap-8 py-8 md:grid-cols-4">
          {footerLinks.map((item) => (
            <div key={item.title}>
              <h3 className="mb-4 text-xs">{item.title}</h3>
              <ul className="space-y-2 text-muted-foreground text-sm">
                {item.links.map((link) => (
                  <li key={link.label}>
                    <a className="hover:text-foreground" href={link.href}>
                      {link.label}
                    </a>
                  </li>
                ))}
              </ul>
            </div>
          ))}
        </div>
        <div className="h-px bg-border" />
        {/* Social Buttons + App Links */}
        <div className="flex flex-wrap items-center justify-between gap-4 py-5">
          <div className="flex items-center gap-2">
            {socialLinks.map(({ icon: Icon, href }, index) => (
              <Button
                asChild
                key={`social-${href}-${index}`} // More descriptive prefix
                size="icon-sm"
                variant="outline"
              >
                <a href={href}>
                  <Icon />
                </a>
              </Button>
            ))}
          </div>

          <div className="flex gap-4">
            <Button asChild className="h-11">
              <a href="#">
                <PlayStoreIcon className="size-5" />
                <div className="flex flex-col items-start justify-center pr-2 text-left">
                  <span className="font-light text-[10px] leading-none tracking-tighter">
                    GET IT ON
                  </span>
                  <p className="font-bold text-base leading-none">
                    Google Play
                  </p>
                </div>
              </a>
            </Button>

            <Button asChild className="h-11">
              <a href="#">
                <AppleIcon className="size-5" />
                <div className="flex flex-col items-start justify-center pr-2 text-left">
                  <span className="text-[10px] leading-none tracking-tighter">
                    Download on the
                  </span>
                  <p className="font-bold text-base leading-none">App Store</p>
                </div>
              </a>
            </Button>
          </div>
        </div>
        <div className="h-px bg-border" />
        <div className="py-4 text-center text-muted-foreground text-xs">
          <p>&copy; {new Date().getFullYear()} efferd, All rights reserved</p>
        </div>
      </div>
    </footer>
  );
}

function PlayStoreIcon({
  fill = "currentColor",
  ...props
}: React.ComponentProps<"svg">) {
  return (
    <svg fill={fill} viewBox="0 0 24 24" {...props}>
      <path d="m21.762,9.942L4.67.378c-.721-.466-1.635-.504-2.393-.099-.768.411-1.246,1.208-1.246,2.08v19.282c0,.872.477,1.668,1.246,2.079.755.404,1.668.37,2.393-.098l17.092-9.564c.756-.423,1.207-1.192,1.207-2.058s-.451-1.635-1.207-2.058Zm-5.746-1.413l-2.36,2.36L5.302,2.534l10.714,5.995ZM2.604,21.906V2.094l9.941,9.906L2.604,21.906Zm2.698-.439l8.355-8.355,2.36,2.36-10.714,5.995Zm15.692-8.78l-3.552,1.987-2.674-2.674,2.674-2.674,3.552,1.987c.363.203.402.548.402.686s-.039.483-.402.686Z" />
    </svg>
  );
}

function AppleIcon({
  fill = "currentColor",
  ...props
}: React.ComponentProps<"svg">) {
  return (
    <svg fill={fill} viewBox="0 0 24 24" {...props}>
      <g id="_Group_2">
        <g id="_Group_3">
          <path
            d="M18.546,12.763c0.024-1.87,1.004-3.597,2.597-4.576c-1.009-1.442-2.64-2.323-4.399-2.378    c-1.851-0.194-3.645,1.107-4.588,1.107c-0.961,0-2.413-1.088-3.977-1.056C6.122,5.927,4.25,7.068,3.249,8.867    c-2.131,3.69-0.542,9.114,1.5,12.097c1.022,1.461,2.215,3.092,3.778,3.035c1.529-0.063,2.1-0.975,3.945-0.975    c1.828,0,2.364,0.975,3.958,0.938c1.64-0.027,2.674-1.467,3.66-2.942c0.734-1.041,1.299-2.191,1.673-3.408    C19.815,16.788,18.548,14.879,18.546,12.763z"
            id="_Path_"
          />
          <path
            d="M15.535,3.847C16.429,2.773,16.87,1.393,16.763,0c-1.366,0.144-2.629,0.797-3.535,1.829    c-0.895,1.019-1.349,2.351-1.261,3.705C13.352,5.548,14.667,4.926,15.535,3.847z"
            id="_Path_2"
          />
        </g>
      </g>
    </svg>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `footer-four`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/footer-four.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`

##### Component Source Code

###### File: `src/lib/components/efferd/footer/footer-four.svelte`

```svelte
<script lang="ts">
	import LinkGroup from "$lib/components/efferd/footer/LinkGroup.svelte";
	import { cn } from "$lib/utils";
	import { ArrowRightIcon } from "@lucide/svelte";

	type LinkItem = {
		title: string;
		href: string;
	};

	type FooterColumn = {
		social: {
			title: string;
			href: string;
		};
		group: {
			title: string;
			links: LinkItem[];
		};
	};

	const columns: FooterColumn[] = [
		{
			social: { title: "Facebook", href: "https://facebook.com" },
			group: {
				title: "About Us",
				links: [
					{ title: "Pricing", href: "/" },
					{ title: "Testimonials", href: "/" },
					{ title: "FAQs", href: "/" },
					{ title: "Contact Us", href: "/" },
					{ title: "Blog", href: "/" }
				]
			}
		},
		{
			social: { title: "Youtube", href: "https://youtube.com" },
			group: {
				title: "Support",
				links: [
					{ title: "Help Center", href: "/" },
					{ title: "Terms", href: "/" },
					{ title: "Privacy", href: "/" },
					{ title: "Security", href: "/" },
					{ title: "Cookie Policy", href: "/" }
				]
			}
		},
		{
			social: { title: "Twitter", href: "https://x.com" },
			group: {
				title: "Community",
				links: [
					{ title: "Forum", href: "/" },
					{ title: "Events", href: "/" },
					{ title: "Partners", href: "/" },
					{ title: "Affiliates", href: "/" },
					{ title: "Career", href: "/" }
				]
			}
		},
		{
			social: { title: "Instagram", href: "https://instagram.com" },
			group: {
				title: "Press",
				links: [
					{ title: "Investors", href: "/" },
					{ title: "Terms of Use", href: "/" },
					{ title: "Privacy Policy", href: "/" },
					{ title: "Cookie Policy", href: "/" },
					{ title: "Legal", href: "/" }
				]
			}
		}
	];

	const currentYear = new Date().getFullYear();
</script>

<footer
	class={cn(
		"border-t",
		"dark:bg-[radial-gradient(35%_128px_at_50%_0%,--theme(--color-foreground/.08),transparent)]"
	)}
>
	<div class="relative mx-auto max-w-5xl px-4">
		<div class="relative grid grid-cols-1 border-x md:grid-cols-4 md:divide-x">
			{#each columns as column, index (column.social.title)}
				<div>
					<a
						href={column.social.href}
						class={cn(
							"flex items-center justify-between border-y p-2 text-sm hover:bg-muted md:border-t-0 dark:hover:bg-muted/50",
							index === 0 && "border-t-0"
						)}
					>
						<span class="font-medium">{column.social.title}</span>
						<ArrowRightIcon class="size-4" />
					</a>
					<LinkGroup title={column.group.title} links={column.group.links} />
				</div>
			{/each}
		</div>
	</div>

	<div class="flex justify-center border-t p-3">
		<p class="text-xs text-muted-foreground">
			&copy; {currentYear} efferd, All rights reserved
		</p>
	</div>
</footer>

```

###### File: `src/lib/components/efferd/footer/LinkGroup.svelte`

```svelte
<script lang="ts">
	type LinkItem = {
		title: string;
		href: string;
	};

	type Props = {
		title: string;
		links: LinkItem[];
	};

	let { title, links }: Props = $props();
</script>

<div class="p-2">
	<h3 class="mt-2 mb-4 text-xs font-medium tracking-wider text-foreground/75 uppercase">
		{title}
	</h3>
	<ul class="space-y-1.5">
		{#each links as link (link.title)}
			<li>
				<a class="text-xs text-muted-foreground hover:text-foreground" href={link.href}>
					{link.title}
				</a>
			</li>
		{/each}
	</ul>
</div>

```

---

### Footer 5 <a name="footer-5"></a>

A premium footer 5 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `footer-5`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/footer-5.json
```

##### Dependencies

- **NPM Packages**:
  - `motion`
- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `registry/blocks/footer/5/sticky-footer.tsx`

```tsx
"use client";
import {
  FacebookIcon,
  InstagramIcon,
  LinkedinIcon,
  YoutubeIcon,
} from "lucide-react";
import { motion, useReducedMotion } from "motion/react";
import type React from "react";
import { Logo } from "@/components/logo";
import { Button } from "@/components/ui/button";

type FooterLink = {
  title: string;
  href: string;
  icon?: React.ComponentType<{ className?: string }>;
};
type FooterLinkGroup = {
  label: string;
  links: FooterLink[];
};

export function StickyFooter() {
  return (
    <footer
      className="relative h-[560px] w-full border-t"
      style={{ clipPath: "polygon(0% 0, 100% 0%, 100% 100%, 0 100%)" }}
    >
      <div className="fixed bottom-0 h-[560px] w-full">
        <div className="sticky top-[calc(100vh-560px)] h-full overflow-y-auto">
          <div className="relative flex size-full flex-col justify-between gap-5 px-4">
            <div
              aria-hidden
              className="absolute inset-0 isolate z-0 opacity-50 contain-strict dark:opacity-100"
            >
              <div className="-translate-y-87.5 -rotate-45 absolute top-0 left-0 h-320 w-140 rounded-full bg-[radial-gradient(68.54%_68.72%_at_55.02%_31.46%,--theme(--color-foreground/.06)_0,hsla(0,0%,55%,.02)_50%,--theme(--color-foreground/.01)_80%)]" />
              <div className="-rotate-45 absolute top-0 left-0 h-320 w-60 rounded-full bg-[radial-gradient(50%_50%_at_50%_50%,--theme(--color-foreground/.04)_0,--theme(--color-foreground/.01)_80%,transparent_100%)] [translate:5%_-50%]" />
              <div className="-translate-y-87.5 -rotate-45 absolute top-0 left-0 h-320 w-60 rounded-full bg-[radial-gradient(50%_50%_at_50%_50%,--theme(--color-foreground/.04)_0,--theme(--color-foreground/.01)_80%,transparent_100%)]" />
            </div>
            <div className="flex flex-col gap-8 pt-12 md:flex-row">
              <AnimatedContainer className="w-full min-w-2xs max-w-sm space-y-4">
                <Logo className="h-5" />
                <p className="mt-8 text-muted-foreground text-sm md:mt-0">
                  Innovative fintech empowering businesses with seamless
                  payments, lending, and financial infrastructure worldwide.
                </p>
                <div className="flex gap-2">
                  {socialLinks.map((link, index) => (
                    <Button
                      key={`social-${link.href}-${index}`}
                      size="icon-sm"
                      variant="outline"
                    >
                      <link.icon className="size-4" />
                    </Button>
                  ))}
                </div>
              </AnimatedContainer>
              {footerLinkGroups.map((group, index) => (
                <AnimatedContainer
                  className="w-full"
                  delay={0.1 + index * 0.1}
                  key={group.label}
                >
                  <div className="mb-10 md:mb-0">
                    <h3 className="text-sm uppercase">{group.label}</h3>
                    <ul className="mt-4 space-y-2 text-muted-foreground text-sm md:text-xs lg:text-sm">
                      {group.links.map((link) => (
                        <li key={link.title}>
                          <a
                            className="inline-flex items-center transition-all duration-300 hover:text-foreground"
                            href={link.href}
                          >
                            {link.icon && <link.icon className="me-1 size-4" />}
                            {link.title}
                          </a>
                        </li>
                      ))}
                    </ul>
                  </div>
                </AnimatedContainer>
              ))}
            </div>
            <div className="flex flex-col items-center justify-between gap-2 border-t py-4 text-muted-foreground text-sm md:flex-row">
              <p>
                &copy; {new Date().getFullYear()} efferd, All rights reserved.
              </p>
              <a className="hover:text-foreground" href="#">
                License
              </a>
            </div>
          </div>
        </div>
      </div>
    </footer>
  );
}

const socialLinks = [
  { title: "Facebook", href: "#", icon: FacebookIcon },
  { title: "Instagram", href: "#", icon: InstagramIcon },
  { title: "Youtube", href: "#", icon: YoutubeIcon },
  { title: "LinkedIn", href: "#", icon: LinkedinIcon },
];

const footerLinkGroups: FooterLinkGroup[] = [
  {
    label: "Product",
    links: [
      { title: "Payments", href: "#" },
      { title: "Cards & Issuing", href: "#" },
      { title: "Lending & Credit", href: "#" },
      { title: "Wealth Management", href: "#" },
      { title: "Insurance", href: "#" },
      { title: "Crypto Wallets", href: "#" },
      { title: "FX & Currency Exchange", href: "#" },
      { title: "Treasury Management", href: "#" },
      { title: "Merchant Services", href: "#" },
      { title: "Point of Sale", href: "#" },
      { title: "Embedded Finance", href: "#" },
      { title: "Open Banking API", href: "#" },
      { title: "SDKs & Integrations", href: "#" },
      { title: "Pricing", href: "#" },
    ],
  },
  {
    label: "Solutions",
    links: [
      { title: "Startups", href: "#" },
      { title: "Enterprises", href: "#" },
      { title: "Marketplaces", href: "#" },
      { title: "Freelancers", href: "#" },
      { title: "E-commerce", href: "#" },
      { title: "Banks & Credit Unions", href: "#" },
      { title: "Investment Platforms", href: "#" },
      { title: "Insurance Providers", href: "#" },
      { title: "Payment Gateways", href: "#" },
      { title: "Government & Public Sector", href: "#" },
      { title: "Nonprofits", href: "#" },
      { title: "Education", href: "#" },
      { title: "Healthcare", href: "#" },
    ],
  },
  {
    label: "Resources",
    links: [
      { title: "Blog", href: "#" },
      { title: "Case Studies", href: "#" },
      { title: "Documentation", href: "#" },
      { title: "API Reference", href: "#" },
      { title: "Developer Tools", href: "#" },
      { title: "Guides & Tutorials", href: "#" },
      { title: "Whitepapers", href: "#" },
      { title: "Reports & Research", href: "#" },
      { title: "Events & Webinars", href: "#" },
      { title: "E-books", href: "#" },
      { title: "Community Forum", href: "#" },
      { title: "Release Notes", href: "#" },
      { title: "System Status", href: "#" },
    ],
  },
  {
    label: "Company",
    links: [
      { title: "About Us", href: "#" },
      { title: "Leadership", href: "#" },
      { title: "Careers", href: "#" },
      { title: "Press", href: "#" },
      { title: "Sustainability", href: "#" },
      { title: "Diversity & Inclusion", href: "#" },
      { title: "Investor Relations", href: "#" },
      { title: "Partners", href: "#" },
      { title: "Legal & Compliance", href: "#" },
      { title: "Privacy Policy", href: "#" },
      { title: "Cookie Policy", href: "#" },
      { title: "Terms of Service", href: "#" },
      { title: "AML & KYC Policy", href: "#" },
      { title: "Regulatory Disclosures", href: "#" },
    ],
  },
];

type AnimatedContainerProps = React.ComponentProps<typeof motion.div> & {
  children?: React.ReactNode;
  delay?: number;
};

function AnimatedContainer({
  delay = 0.1,
  children,
  ...props
}: AnimatedContainerProps) {
  const shouldReduceMotion = useReducedMotion();

  if (shouldReduceMotion) {
    return children;
  }

  return (
    <motion.div
      initial={{ filter: "blur(4px)", translateY: -8, opacity: 0 }}
      transition={{ delay, duration: 0.8 }}
      viewport={{ once: true }}
      whileInView={{ filter: "blur(0px)", translateY: 0, opacity: 1 }}
      {...props}
    >
      {children}
    </motion.div>
  );
}

```

###### File: `components/logo.tsx`

```tsx
import type React from "react";

export const LogoIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 24 24" {...props}>
    <path
      d="M 15.03125 23.851562 C 14.117188 23.609375 13.417969 23 13 22.101562 C 12.808594 21.691406 12.808594 21.613281 12.808594 18.375 C 12.808594 15.066406 12.808594 15.066406 13.097656 14.484375 C 13.429688 13.8125 13.898438 13.351562 14.585938 13.027344 C 15.074219 12.800781 15.074219 12.800781 18.386719 12.800781 C 21.699219 12.800781 21.699219 12.800781 22.28125 13.089844 C 22.953125 13.421875 23.414062 13.890625 23.738281 14.578125 C 23.964844 15.066406 23.964844 15.066406 23.988281 18.140625 C 24.015625 20.769531 24 21.285156 23.875 21.710938 C 23.5625 22.789062 22.769531 23.582031 21.699219 23.863281 C 20.964844 24.042969 15.746094 24.042969 15.03125 23.851562 Z M 21.679688 22.390625 C 21.863281 22.304688 22.117188 22.085938 22.246094 21.917969 C 22.480469 21.613281 22.480469 21.613281 22.480469 18.375 C 22.480469 15.136719 22.480469 15.136719 22.238281 14.824219 C 21.757812 14.1875 21.792969 14.195312 18.386719 14.195312 C 15.667969 14.195312 15.308594 14.214844 15.066406 14.34375 C 14.691406 14.542969 14.359375 14.960938 14.246094 15.355469 C 14.144531 15.753906 14.132812 20.847656 14.246094 21.328125 C 14.386719 21.9375 14.847656 22.398438 15.441406 22.519531 C 15.625 22.554688 17.027344 22.582031 18.5625 22.574219 C 21.105469 22.554688 21.375 22.539062 21.679688 22.390625 Z M 21.679688 22.390625 "
      opacity="50%"
    />
    <path d="M 2.859375 23.085938 C 2.089844 22.84375 1.324219 22.15625 0.976562 21.398438 C 0.792969 20.996094 0.785156 20.882812 0.785156 17.636719 C 0.785156 14.28125 0.785156 14.28125 1.019531 13.785156 C 1.296875 13.1875 1.855469 12.609375 2.441406 12.324219 C 2.875 12.105469 2.875 12.105469 6.195312 12.078125 C 9.4375 12.054688 9.523438 12.0625 10.03125 12.246094 C 10.71875 12.507812 11.441406 13.21875 11.730469 13.933594 C 11.9375 14.457031 11.9375 14.472656 11.9375 17.636719 C 11.9375 21.136719 11.9375 21.128906 11.371094 21.953125 C 11.03125 22.433594 10.550781 22.800781 9.925781 23.035156 C 9.480469 23.199219 9.261719 23.207031 6.335938 23.199219 C 4.035156 23.199219 3.128906 23.164062 2.859375 23.085938 Z M 2.859375 23.085938 " />
    <path d="M 13.898438 11.695312 C 13.226562 11.4375 12.503906 10.703125 12.25 10.023438 C 12.070312 9.519531 12.058594 9.433594 12.085938 6.195312 C 12.113281 2.929688 12.113281 2.867188 12.3125 2.476562 C 12.636719 1.824219 13.070312 1.386719 13.707031 1.074219 C 14.289062 0.785156 14.289062 0.785156 17.644531 0.785156 C 20.90625 0.785156 21.007812 0.792969 21.410156 0.976562 C 21.984375 1.238281 22.65625 1.890625 22.933594 2.476562 C 23.179688 2.964844 23.179688 2.964844 23.179688 6.316406 C 23.179688 9.667969 23.179688 9.667969 22.890625 10.25 C 22.578125 10.886719 22.140625 11.324219 21.488281 11.644531 C 21.097656 11.84375 21.042969 11.84375 17.734375 11.863281 C 14.492188 11.878906 14.359375 11.878906 13.898438 11.695312 Z M 13.898438 11.695312 " />
    <path d="M 2.214844 11.078125 C 1.324219 10.824219 0.609375 10.207031 0.199219 9.3125 C 0 8.894531 0 8.832031 0 5.574219 C 0 2.265625 0 2.265625 0.234375 1.769531 C 0.53125 1.132812 1.132812 0.535156 1.769531 0.238281 C 2.265625 0.00390625 2.265625 0.00390625 5.578125 0.00390625 C 8.886719 0.00390625 8.886719 0.00390625 9.386719 0.238281 C 10.019531 0.535156 10.621094 1.132812 10.917969 1.769531 C 11.152344 2.265625 11.152344 2.265625 11.152344 5.574219 C 11.152344 8.882812 11.152344 8.882812 10.925781 9.371094 C 10.605469 10.058594 10.144531 10.53125 9.472656 10.859375 C 8.886719 11.148438 8.886719 11.148438 5.75 11.164062 C 3.441406 11.183594 2.507812 11.15625 2.214844 11.078125 Z M 8.898438 9.605469 C 9.238281 9.425781 9.613281 8.988281 9.699219 8.683594 C 9.734375 8.554688 9.757812 7.117188 9.757812 5.488281 C 9.757812 2.179688 9.769531 2.207031 9.132812 1.726562 C 8.820312 1.484375 8.820312 1.484375 5.804688 1.457031 C 4.148438 1.4375 2.65625 1.457031 2.5 1.484375 C 2.34375 1.507812 2.066406 1.683594 1.882812 1.855469 C 1.375 2.335938 1.34375 2.632812 1.421875 5.976562 C 1.480469 8.816406 1.480469 8.816406 1.726562 9.128906 C 2.214844 9.773438 2.316406 9.789062 5.75 9.773438 C 8.285156 9.753906 8.660156 9.738281 8.898438 9.605469 Z M 8.898438 9.605469 " />
  </svg>
);

export const Logo = (props: React.ComponentProps<"svg">) => (
  <svg
    fill="currentColor"
    viewBox="0 0 90 24"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <defs>
      <clipPath id="a">
        <path d="M12.785 12.781h11.172V24H12.785Zm0 0" />
      </clipPath>
      <clipPath id="b">
        <path d="M.008 0h11.148v11.2H.008Zm0 0" />
      </clipPath>
    </defs>
    <g clipPath="url(#a)">
      <path
        d="M15.008 23.855c-.91-.242-1.61-.851-2.028-1.75-.19-.41-.19-.488-.19-3.73 0-3.309 0-3.309.288-3.89a3 3 0 0 1 1.485-1.458c.488-.226.488-.226 3.792-.226 3.31 0 3.31 0 3.887.289.672.332 1.133.8 1.453 1.488.227.488.227.488.25 3.563.028 2.632.012 3.148-.113 3.574-.309 1.078-1.102 1.87-2.168 2.152-.734.18-5.941.18-6.656-.012m6.637-1.46c.18-.086.433-.305.562-.473.234-.305.234-.305.234-3.547 0-3.238 0-3.238-.242-3.55-.476-.637-.441-.63-3.844-.63-2.71 0-3.07.02-3.312.149-.375.199-.707.617-.82 1.011-.098.399-.11 5.497 0 5.977.14.61.601 1.07 1.195 1.191.184.036 1.582.063 3.113.055 2.54-.02 2.809-.035 3.114-.183m0 0"
        opacity="50%"
      />
    </g>
    <path d="M2.86 23.09c-.766-.242-1.532-.93-1.88-1.688-.183-.402-.187-.515-.187-3.765 0-3.356 0-3.356.23-3.852.278-.598.836-1.176 1.422-1.46.43-.22.43-.22 3.746-.247 3.235-.023 3.32-.015 3.829.168.683.262 1.406.973 1.695 1.688.207.523.207.539.207 3.703 0 3.504 0 3.496-.567 4.32-.34.48-.82.848-1.44 1.082-.446.164-.665.172-3.583.164-2.297 0-3.203-.035-3.473-.113M13.879 11.695c-.672-.258-1.395-.992-1.645-1.672-.18-.503-.191-.59-.164-3.832.028-3.265.028-3.328.223-3.718.324-.653.758-1.09 1.395-1.403.578-.289.578-.289 3.93-.289 3.253 0 3.355.008 3.757.192.57.261 1.242.914 1.52 1.5.246.488.246.488.246 3.84 0 3.355 0 3.355-.29 3.937a2.9 2.9 0 0 1-1.398 1.395c-.39.199-.445.199-3.746.218-3.238.016-3.371.016-3.828-.168m0 0" />
    <g clipPath="url(#b)">
      <path d="M2.219 11.078c-.89-.254-1.602-.871-2.012-1.765-.2-.418-.2-.481-.2-3.743 0-3.308 0-3.308.235-3.804A3.34 3.34 0 0 1 1.773.234C2.27 0 2.27 0 5.574 0c3.301 0 3.301 0 3.801.234.633.297 1.23.895 1.527 1.532.235.496.235.496.235 3.804 0 3.313 0 3.313-.227 3.801-.32.688-.777 1.16-1.45 1.488-.585.29-.585.29-3.714.305-2.305.02-3.234-.008-3.527-.086m6.668-1.473c.34-.18.715-.617.8-.921.036-.13.06-1.567.06-3.2 0-3.308.01-3.28-.626-3.761-.312-.243-.312-.243-3.32-.27-1.653-.02-3.145 0-3.297.027-.156.024-.434.2-.617.372-.508.48-.54.777-.461 4.12.058 2.844.058 2.844.304 3.157.489.644.59.66 4.016.644 2.531-.02 2.902-.035 3.14-.168m0 0" />
    </g>
    <path d="m33.688 17.254-1.622-2.125 3.325-2.57a1.6 1.6 0 0 0-.696-.434 3 3 0 0 0-.957-.145q-.626.001-1.148.266a2.75 2.75 0 0 0-.903.707q-.379.445-.59 1.047c-.14.402-.206.84-.206 1.313q0 .655.246 1.257c.164.403.394.758.68 1.063q.432.46 1.019.734a3 3 0 0 0 1.27.278c.84 0 1.57-.243 2.199-.723q.942-.72 1.152-2.008l3.375.367a7.6 7.6 0 0 1-.879 2.387q-.614 1.053-1.504 1.797-.891.75-2.027 1.14a7.5 7.5 0 0 1-2.445.395 6.6 6.6 0 0 1-2.606-.512 6.1 6.1 0 0 1-2.039-1.402 6.3 6.3 0 0 1-1.32-2.11q-.474-1.223-.473-2.663-.001-1.44.473-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.177-.499 2.59-.5 2.195 0 3.777.973c1.059.644 1.855 1.582 2.398 2.804ZM47.973 6.684H46.69q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.254-4.239q1.26-1.296 3.48-1.297h1.23ZM54.723 6.684H53.44q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.258-4.239q1.255-1.296 3.476-1.297h1.23ZM61.21 17.254l-1.62-2.125 3.324-2.57a1.6 1.6 0 0 0-.695-.434 3 3 0 0 0-.953-.145q-.632.001-1.153.266-.52.26-.902.707-.38.445-.59 1.047-.206.603-.207 1.313 0 .655.246 1.257.252.605.68 1.063.433.46 1.023.734.586.277 1.266.278c.84 0 1.57-.243 2.2-.723q.94-.72 1.151-2.008l3.375.367a7.6 7.6 0 0 1-.878 2.387q-.614 1.053-1.504 1.797-.891.75-2.028 1.14A7.5 7.5 0 0 1 61.5 22a6.6 6.6 0 0 1-2.605-.512 6.1 6.1 0 0 1-2.04-1.402 6.3 6.3 0 0 1-1.32-2.11q-.473-1.223-.472-2.663-.001-1.44.472-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.178-.499 2.59-.5 2.199 0 3.781.973c1.055.644 1.852 1.582 2.395 2.804ZM76.805 12.348h-.758q-1.413 0-2.121.855c-.469.567-.703 1.29-.703 2.16v6.32h-3.559V8.942h3.164v2.41h.05c.317-.855.75-1.5 1.31-1.925q.838-.645 1.964-.645h.653ZM89.992 3.328v12.063c0 .89-.152 1.742-.457 2.543a6.6 6.6 0 0 1-1.285 2.113 6 6 0 0 1-1.988 1.43q-1.161.522-2.551.523a5.9 5.9 0 0 1-2.496-.523 6.1 6.1 0 0 1-1.965-1.43 6.8 6.8 0 0 1-1.293-2.125 7.3 7.3 0 0 1-.473-2.637q0-1.335.461-2.555a6.5 6.5 0 0 1 1.282-2.125 6.3 6.3 0 0 1 1.921-1.44 5.4 5.4 0 0 1 2.407-.54q.527 0 1.007.117.487.122 1.008.25v3.54a5 5 0 0 0-.851-.286 4 4 0 0 0-.953-.105q-1.285 0-2.106.879-.825.876-.824 2.293 0 1.417.809 2.296a2.65 2.65 0 0 0 2.015.875q1.359-.001 2.172-.875.811-.881.813-2.218V3.328Zm0 0" />
  </svg>
);

```

#### Svelte Implementation

- **Registry Key**: `footer-five`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/footer-five.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `src/lib/components/efferd/footer/footer-five.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import AppleLogo from "$lib/svgs/apple.svelte";
	import PlayStoreLogo from "$lib/svgs/play-store.svelte";
	import XLogo from "$lib/svgs/x.svelte";
	import { FacebookIcon, InstagramIcon, LinkedinIcon } from "@lucide/svelte";
	import type { Component } from "svelte";

	type FooterLink = {
		href: string;
		label: string;
	};

	type FooterSection = {
		title: string;
		links: FooterLink[];
	};

	type SocialLink = {
		href: string;
		icon: Component;
		label: string;
	};

	const footerLinks: FooterSection[] = [
		{
			title: "Company",
			links: [
				{ href: "/", label: "Engineering Blog" },
				{ href: "/", label: "Marketplace" },
				{ href: "/", label: "What's New" },
				{ href: "/", label: "About" },
				{ href: "/", label: "Press" },
				{ href: "/", label: "Careers" },
				{ href: "/", label: "Social Good" }
			]
		},
		{
			title: "Community",
			links: [
				{ href: "/", label: "Linktree for Enterprise" },
				{ href: "/", label: "2023 Creator Report" },
				{ href: "/", label: "2022 Creator Report" },
				{ href: "/", label: "Charities" },
				{ href: "/", label: "What's Trending" },
				{ href: "/", label: "Creator Profile Directory" },
				{ href: "/", label: "Explore Templates" }
			]
		},
		{
			title: "Support",
			links: [
				{ href: "/", label: "Help Topics" },
				{ href: "/", label: "Getting Started" },
				{ href: "/", label: "Linoree Pro" },
				{ href: "/", label: "Features & How-tos" },
				{ href: "/", label: "FAQs" },
				{ href: "/", label: "Report a Violation" }
			]
		},
		{
			title: "Legal",
			links: [
				{ href: "/", label: "Terms & Conditions" },
				{ href: "/", label: "Privacy Notice" },
				{ href: "/", label: "Cookie Notice" },
				{ href: "/", label: "Trust Center" },
				{ href: "/", label: "Cookie Preferences" },
				{ href: "/", label: "Transparency Report" },
				{ href: "/", label: "Law Enforcement Access Policy" }
			]
		}
	];

	const socialLinks: SocialLink[] = [
		{
			icon: FacebookIcon,
			href: "https://facebook.com",
			label: "Facebook"
		},
		{
			icon: InstagramIcon,
			href: "https://instagram.com",
			label: "Instagram"
		},
		{
			icon: LinkedinIcon,
			href: "https://linkedin.com",
			label: "LinkedIn"
		},
		{
			icon: XLogo,
			href: "https://x.com",
			label: "X"
		}
	];

	const currentYear = new Date().getFullYear();
</script>

<footer class="border-t">
	<div class="mx-auto max-w-6xl px-4 lg:px-6">
		<div class="grid grid-cols-2 gap-8 py-8 md:grid-cols-4">
			{#each footerLinks as item (item.title)}
				<div>
					<h3 class="mb-4 text-xs">{item.title}</h3>
					<ul class="space-y-2 text-sm text-muted-foreground">
						{#each item.links as link (link.label)}
							<li>
								<a class="hover:text-foreground" href={link.href}>
									{link.label}
								</a>
							</li>
						{/each}
					</ul>
				</div>
			{/each}
		</div>

		<div class="h-px bg-border"></div>

		<div class="flex flex-wrap items-center justify-between gap-4 py-5">
			<div class="flex items-center gap-2">
				{#each socialLinks as item, index (`social-${item.href}-${index}`)}
					{@const SocialIcon = item.icon}
					<Button
						href={item.href}
						aria-label={item.label}
						size="icon-sm"
						variant="outline"
					>
						<SocialIcon class="size-4" />
					</Button>
				{/each}
			</div>

			<div class="flex gap-4">
				<Button
					href="https://play.google.com/store"
					class="h-11"
					target="_blank"
					rel="noreferrer"
				>
					<PlayStoreLogo class="size-5" />
					<div class="flex flex-col items-start justify-center pr-2 text-left">
						<span class="text-[10px] leading-none font-light tracking-tighter">
							GET IT ON
						</span>
						<p class="text-base leading-none font-bold">Google Play</p>
					</div>
				</Button>

				<Button
					href="https://www.apple.com/app-store/"
					class="h-11"
					target="_blank"
					rel="noreferrer"
				>
					<AppleLogo class="size-5" />
					<div class="flex flex-col items-start justify-center pr-2 text-left">
						<span class="text-[10px] leading-none tracking-tighter"
							>Download on the</span
						>
						<p class="text-base leading-none font-bold">App Store</p>
					</div>
				</Button>
			</div>
		</div>

		<div class="h-px bg-border"></div>

		<div class="py-4 text-center text-xs text-muted-foreground">
			<p>&copy; {currentYear} efferd, All rights reserved</p>
		</div>
	</div>
</footer>

```

###### File: `src/lib/svgs/apple.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { fill = "currentColor", ...props }: SVGAttributes<SVGSVGElement> & { fill?: string } =
		$props();
</script>

<svg {fill} viewBox="0 0 24 24" {...props}>
	<g id="_Group_2">
		<g id="_Group_3">
			<path
				d="M18.546,12.763c0.024-1.87,1.004-3.597,2.597-4.576c-1.009-1.442-2.64-2.323-4.399-2.378 c-1.851-0.194-3.645,1.107-4.588,1.107c-0.961,0-2.413-1.088-3.977-1.056C6.122,5.927,4.25,7.068,3.249,8.867 c-2.131,3.69-0.542,9.114,1.5,12.097c1.022,1.461,2.215,3.092,3.778,3.035c1.529-0.063,2.1-0.975,3.945-0.975 c1.828,0,2.364,0.975,3.958,0.938c1.64-0.027,2.674-1.467,3.66-2.942c0.734-1.041,1.299-2.191,1.673-3.408 C19.815,16.788,18.548,14.879,18.546,12.763z"
				id="_Path_"
			/>
			<path
				d="M15.535,3.847C16.429,2.773,16.87,1.393,16.763,0c-1.366,0.144-2.629,0.797-3.535,1.829 c-0.895,1.019-1.349,2.351-1.261,3.705C13.352,5.548,14.667,4.926,15.535,3.847z"
				id="_Path_2"
			/>
		</g>
	</g>
</svg>

```

###### File: `src/lib/svgs/play-store.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { fill = "currentColor", ...props }: SVGAttributes<SVGSVGElement> & { fill?: string } =
		$props();
</script>

<svg {fill} viewBox="0 0 24 24" {...props}>
	<path
		d="m21.762,9.942L4.67.378c-.721-.466-1.635-.504-2.393-.099-.768.411-1.246,1.208-1.246,2.08v19.282c0,.872.477,1.668,1.246,2.079.755.404,1.668.37,2.393-.098l17.092-9.564c.756-.423,1.207-1.192,1.207-2.058s-.451-1.635-1.207-2.058Zm-5.746-1.413l-2.36,2.36L5.302,2.534l10.714,5.995ZM2.604,21.906V2.094l9.941,9.906L2.604,21.906Zm2.698-.439l8.355-8.355,2.36,2.36-10.714,5.995Zm15.692-8.78l-3.552,1.987-2.674-2.674,2.674-2.674,3.552,1.987c.363.203.402.548.402.686s-.039.483-.402.686Z"
	/>
</svg>

```

###### File: `src/lib/svgs/x.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<!-- <svg fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" {...props}>
	<path
		d="m18.9,1.153h3.682l-8.042,9.189,9.46,12.506h-7.405l-5.804-7.583-6.634,7.583H.469l8.6-9.831L0,1.153h7.593l5.241,6.931,6.065-6.931Zm-1.293,19.494h2.039L6.482,3.239h-2.19l13.314,17.408Z"
	/>
</svg> -->
<svg fill="none" viewBox="0 0 1200 1227" {...props}
	><path
		fill="currentColor"
		d="M714.163 519.284 1160.89 0h-105.86L667.137 450.887 357.328 0H0l468.492 681.821L0 1226.37h105.866l409.625-476.152 327.181 476.152H1200L714.137 519.284h.026ZM569.165 687.828l-47.468-67.894-377.686-540.24h162.604l304.797 435.991 47.468 67.894 396.2 566.721H892.476L569.165 687.854v-.026Z"
	/></svg
>

```

---

### Footer 6 <a name="footer-6"></a>

A premium footer 6 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `footer-six`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/footer-six.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
  - `motion-sv`
- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `src/lib/components/efferd/footer/footer-six.svelte`

```svelte
<script lang="ts">
	import AnimatedContainer from "$lib/components/efferd/footer/AnimatedContainer.svelte";
	import { Button } from "$lib/components/ui/button";
	import Logo from "$lib/svgs/logo.svelte";
	import {
		FacebookIcon,
		InstagramIcon,
		LinkedinIcon,
		YoutubeIcon,
		type Icon
	} from "@lucide/svelte";

	type FooterLink = {
		title: string;
		href: string;
		icon?: typeof Icon;
	};

	type FooterLinkGroup = {
		label: string;
		links: FooterLink[];
	};

	type SocialLink = {
		title: string;
		href: string;
		icon: typeof Icon;
	};

	const socialLinks: SocialLink[] = [
		{
			title: "Facebook",
			href: "https://facebook.com",
			icon: FacebookIcon
		},
		{
			title: "Instagram",
			href: "https://instagram.com",
			icon: InstagramIcon
		},
		{
			title: "Youtube",
			href: "https://youtube.com",
			icon: YoutubeIcon
		},
		{
			title: "LinkedIn",
			href: "https://linkedin.com",
			icon: LinkedinIcon
		}
	];

	const footerLinkGroups: FooterLinkGroup[] = [
		{
			label: "Product",
			links: [
				{ title: "Payments", href: "/" },
				{ title: "Cards & Issuing", href: "/" },
				{ title: "Lending & Credit", href: "/" },
				{ title: "Wealth Management", href: "/" },
				{ title: "Insurance", href: "/" },
				{ title: "Crypto Wallets", href: "/" },
				{ title: "Treasury Management", href: "/" },
				{ title: "Merchant Services", href: "/" },
				{ title: "Point of Sale", href: "/" },
				{ title: "Embedded Finance", href: "/" },
				{ title: "Open Banking API", href: "/" },
				{ title: "SDKs & Integrations", href: "/" },
				{ title: "Pricing", href: "/" }
			]
		},
		{
			label: "Resources",
			links: [
				{ title: "Blog", href: "/" },
				{ title: "Case Studies", href: "/" },
				{ title: "Documentation", href: "/" },
				{ title: "API Reference", href: "/" },
				{ title: "Developer Tools", href: "/" },
				{ title: "Whitepapers", href: "/" },
				{ title: "Reports & Research", href: "/" },
				{ title: "Events & Webinars", href: "/" },
				{ title: "E-books", href: "/" },
				{ title: "Community Forum", href: "/" },
				{ title: "Release Notes", href: "/" },
				{ title: "System Status", href: "/" }
			]
		},
		{
			label: "Company",
			links: [
				{ title: "About Us", href: "/" },
				{ title: "Leadership", href: "/" },
				{ title: "Careers", href: "/" },
				{ title: "Press", href: "/" },
				{ title: "Sustainability", href: "/" },
				{ title: "Diversity & Inclusion", href: "/" },
				{ title: "Investor Relations", href: "/" },
				{ title: "Partners", href: "/" },
				{ title: "Legal & Compliance", href: "/" },
				{ title: "Privacy Policy", href: "/" },
				{ title: "Cookie Policy", href: "/" },
				{ title: "Terms of Service", href: "/" },
				{ title: "AML & KYC Policy", href: "/" }
			]
		}
	];

	const currentYear = new Date().getFullYear();
</script>

<footer
	class="relative h-(--footer-height) w-full border-t [--footer-height:520px]"
	style="clip-path: polygon(0% 0, 100% 0%, 100% 100%, 0 100%);"
>
	<div class="fixed bottom-0 h-(--footer-height) w-full">
		<div class="sticky top-[calc(100vh-var(--footer-height))] h-full overflow-y-auto">
			<div
				aria-hidden="true"
				class="absolute inset-0 isolate z-0 opacity-50 contain-strict dark:opacity-60"
			>
				<div
					class="absolute top-0 left-0 h-320 w-140 -translate-y-87.5 -rotate-45 rounded-full bg-[radial-gradient(68.54%_68.72%_at_55.02%_31.46%,--theme(--color-foreground/.06)_0,hsla(0,0%,55%,.02)_50%,--theme(--color-foreground/.01)_80%)]"
				></div>
				<div
					class="absolute top-0 left-0 h-320 w-60 [translate:5%_-50%] -rotate-45 rounded-full bg-[radial-gradient(50%_50%_at_50%_50%,--theme(--color-foreground/.04)_0,--theme(--color-foreground/.01)_80%,transparent_100%)]"
				></div>
				<div
					class="absolute top-0 left-0 h-320 w-60 -translate-y-87.5 -rotate-45 rounded-full bg-[radial-gradient(50%_50%_at_50%_50%,--theme(--color-foreground/.04)_0,--theme(--color-foreground/.01)_80%,transparent_100%)]"
				></div>
			</div>

			<div class="relative mx-auto flex size-full max-w-6xl flex-col justify-between gap-5">
				<div class="grid grid-cols-1 gap-8 px-4 pt-12 md:grid-cols-2 lg:grid-cols-4">
					<AnimatedContainer class="w-full space-y-4">
						<Logo class="h-5 w-auto" />
						<p class="mt-8 text-sm text-muted-foreground md:mt-0">
							Beautifully crafted shadcn blocks by efferd.
						</p>
						<div class="flex gap-2">
							{#each socialLinks as link, index (`social-${link.href}-${index}`)}
								{@const SocialIcon = link.icon}
								<Button
									href={link.href}
									aria-label={link.title}
									size="icon-sm"
									variant="outline"
								>
									<SocialIcon class="size-4" />
								</Button>
							{/each}
						</div>
					</AnimatedContainer>

					{#each footerLinkGroups as group, index (group.label)}
						<AnimatedContainer class="w-full" delay={0.1 + index * 0.1}>
							<div class="mb-10 md:mb-0">
								<h3 class="text-sm uppercase">{group.label}</h3>
								<ul
									class="mt-4 space-y-2 text-sm text-muted-foreground md:text-xs lg:text-sm"
								>
									{#each group.links as link (link.title)}
										<li>
											<a
												class="inline-flex items-center hover:text-foreground [&_svg]:me-1 [&_svg]:size-4"
												href={link.href}
											>
												{#if link.icon}
													{@const LinkIcon = link.icon}
													<LinkIcon />
												{/if}
												{link.title}
											</a>
										</li>
									{/each}
								</ul>
							</div>
						</AnimatedContainer>
					{/each}
				</div>

				<div
					class="flex flex-col items-center justify-between gap-2 border-t p-4 text-sm text-muted-foreground md:flex-row"
				>
					<p>&copy; {currentYear} efferd, All rights reserved.</p>
					<a class="hover:text-foreground" href="/">License</a>
				</div>
			</div>
		</div>
	</div>
</footer>

```

###### File: `src/lib/components/efferd/footer/AnimatedContainer.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import { motion } from "motion-sv";
	import type { Snippet } from "svelte";

	type Props = {
		class?: string;
		delay?: number;
		children?: Snippet;
	};

	let { class: className, delay = 0.1, children }: Props = $props();
</script>

<motion.div
	class={cn(className)}
	initial={{ filter: "blur(4px)", translateY: -8, opacity: 0 }}
	transition={{ delay, duration: 0.8 }}
	whileInView={{ filter: "blur(0px)", translateY: 0, opacity: 1 }}
>
	{@render children?.()}
</motion.div>

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

---

## Form Blocks <a name="form"></a>

### Form 1 <a name="form-1"></a>

A premium form 1 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `form-1`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/form-1.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`
  - `button-group`
  - `field`
  - `input`
  - `input-group`
  - `label`

##### Component Source Code

###### File: `registry/blocks/form/1/create-workspace-form.tsx`

```tsx
import { CircleCheckIcon } from "lucide-react";
import { Logo } from "@/components/logo";
import { Button } from "@/components/ui/button";
import { ButtonGroup, ButtonGroupText } from "@/components/ui/button-group";
import {
  Field,
  FieldDescription,
  FieldGroup,
  FieldLabel,
} from "@/components/ui/field";
import { Input } from "@/components/ui/input";
import {
  InputGroup,
  InputGroupAddon,
  InputGroupInput,
} from "@/components/ui/input-group";
import { Label } from "@/components/ui/label";

export function CreateWorkspaceForm() {
  return (
    <div className="w-full max-w-md rounded-xl border bg-background shadow-sm">
      <div className="flex flex-col items-center justify-center gap-6 rounded-t-xl border-b bg-card/60 py-12">
        <Logo className="h-5" />
        <div className="flex flex-col items-center space-y-1">
          <h2 className="font-medium text-2xl">Create your workspace</h2>
          <a className="text-muted-foreground underline" href="#">
            What is a workspace?
          </a>
        </div>
      </div>
      <FieldGroup className="p-4">
        <Field className="gap-2">
          <FieldLabel htmlFor="name">Workspace Name</FieldLabel>
          <Input autoComplete="off" id="name" placeholder="e.g., Acme, Inc." />
          <FieldDescription>
            This is the name of your workspace on Efferd.
          </FieldDescription>
        </Field>
        <Field className="gap-2">
          <FieldLabel htmlFor="slug">Workspace Slug</FieldLabel>
          <ButtonGroup>
            <ButtonGroupText asChild>
              <Label htmlFor="slug">efferd.com/</Label>
            </ButtonGroupText>
            <InputGroup>
              <InputGroupInput id="slug" placeholder="e.g., acme" />
              <InputGroupAddon align="inline-end">
                <CircleCheckIcon />
              </InputGroupAddon>
            </InputGroup>
          </ButtonGroup>
          <FieldDescription>
            This is your workspace's unique slug on Efferd.
          </FieldDescription>
        </Field>
      </FieldGroup>
      <div className="rounded-b-xl border-t bg-card/60 p-4">
        <Button className="w-full" type="submit">
          Create Workspace
        </Button>
      </div>
    </div>
  );
}

```

###### File: `components/logo.tsx`

```tsx
import type React from "react";

export const LogoIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 24 24" {...props}>
    <path
      d="M 15.03125 23.851562 C 14.117188 23.609375 13.417969 23 13 22.101562 C 12.808594 21.691406 12.808594 21.613281 12.808594 18.375 C 12.808594 15.066406 12.808594 15.066406 13.097656 14.484375 C 13.429688 13.8125 13.898438 13.351562 14.585938 13.027344 C 15.074219 12.800781 15.074219 12.800781 18.386719 12.800781 C 21.699219 12.800781 21.699219 12.800781 22.28125 13.089844 C 22.953125 13.421875 23.414062 13.890625 23.738281 14.578125 C 23.964844 15.066406 23.964844 15.066406 23.988281 18.140625 C 24.015625 20.769531 24 21.285156 23.875 21.710938 C 23.5625 22.789062 22.769531 23.582031 21.699219 23.863281 C 20.964844 24.042969 15.746094 24.042969 15.03125 23.851562 Z M 21.679688 22.390625 C 21.863281 22.304688 22.117188 22.085938 22.246094 21.917969 C 22.480469 21.613281 22.480469 21.613281 22.480469 18.375 C 22.480469 15.136719 22.480469 15.136719 22.238281 14.824219 C 21.757812 14.1875 21.792969 14.195312 18.386719 14.195312 C 15.667969 14.195312 15.308594 14.214844 15.066406 14.34375 C 14.691406 14.542969 14.359375 14.960938 14.246094 15.355469 C 14.144531 15.753906 14.132812 20.847656 14.246094 21.328125 C 14.386719 21.9375 14.847656 22.398438 15.441406 22.519531 C 15.625 22.554688 17.027344 22.582031 18.5625 22.574219 C 21.105469 22.554688 21.375 22.539062 21.679688 22.390625 Z M 21.679688 22.390625 "
      opacity="50%"
    />
    <path d="M 2.859375 23.085938 C 2.089844 22.84375 1.324219 22.15625 0.976562 21.398438 C 0.792969 20.996094 0.785156 20.882812 0.785156 17.636719 C 0.785156 14.28125 0.785156 14.28125 1.019531 13.785156 C 1.296875 13.1875 1.855469 12.609375 2.441406 12.324219 C 2.875 12.105469 2.875 12.105469 6.195312 12.078125 C 9.4375 12.054688 9.523438 12.0625 10.03125 12.246094 C 10.71875 12.507812 11.441406 13.21875 11.730469 13.933594 C 11.9375 14.457031 11.9375 14.472656 11.9375 17.636719 C 11.9375 21.136719 11.9375 21.128906 11.371094 21.953125 C 11.03125 22.433594 10.550781 22.800781 9.925781 23.035156 C 9.480469 23.199219 9.261719 23.207031 6.335938 23.199219 C 4.035156 23.199219 3.128906 23.164062 2.859375 23.085938 Z M 2.859375 23.085938 " />
    <path d="M 13.898438 11.695312 C 13.226562 11.4375 12.503906 10.703125 12.25 10.023438 C 12.070312 9.519531 12.058594 9.433594 12.085938 6.195312 C 12.113281 2.929688 12.113281 2.867188 12.3125 2.476562 C 12.636719 1.824219 13.070312 1.386719 13.707031 1.074219 C 14.289062 0.785156 14.289062 0.785156 17.644531 0.785156 C 20.90625 0.785156 21.007812 0.792969 21.410156 0.976562 C 21.984375 1.238281 22.65625 1.890625 22.933594 2.476562 C 23.179688 2.964844 23.179688 2.964844 23.179688 6.316406 C 23.179688 9.667969 23.179688 9.667969 22.890625 10.25 C 22.578125 10.886719 22.140625 11.324219 21.488281 11.644531 C 21.097656 11.84375 21.042969 11.84375 17.734375 11.863281 C 14.492188 11.878906 14.359375 11.878906 13.898438 11.695312 Z M 13.898438 11.695312 " />
    <path d="M 2.214844 11.078125 C 1.324219 10.824219 0.609375 10.207031 0.199219 9.3125 C 0 8.894531 0 8.832031 0 5.574219 C 0 2.265625 0 2.265625 0.234375 1.769531 C 0.53125 1.132812 1.132812 0.535156 1.769531 0.238281 C 2.265625 0.00390625 2.265625 0.00390625 5.578125 0.00390625 C 8.886719 0.00390625 8.886719 0.00390625 9.386719 0.238281 C 10.019531 0.535156 10.621094 1.132812 10.917969 1.769531 C 11.152344 2.265625 11.152344 2.265625 11.152344 5.574219 C 11.152344 8.882812 11.152344 8.882812 10.925781 9.371094 C 10.605469 10.058594 10.144531 10.53125 9.472656 10.859375 C 8.886719 11.148438 8.886719 11.148438 5.75 11.164062 C 3.441406 11.183594 2.507812 11.15625 2.214844 11.078125 Z M 8.898438 9.605469 C 9.238281 9.425781 9.613281 8.988281 9.699219 8.683594 C 9.734375 8.554688 9.757812 7.117188 9.757812 5.488281 C 9.757812 2.179688 9.769531 2.207031 9.132812 1.726562 C 8.820312 1.484375 8.820312 1.484375 5.804688 1.457031 C 4.148438 1.4375 2.65625 1.457031 2.5 1.484375 C 2.34375 1.507812 2.066406 1.683594 1.882812 1.855469 C 1.375 2.335938 1.34375 2.632812 1.421875 5.976562 C 1.480469 8.816406 1.480469 8.816406 1.726562 9.128906 C 2.214844 9.773438 2.316406 9.789062 5.75 9.773438 C 8.285156 9.753906 8.660156 9.738281 8.898438 9.605469 Z M 8.898438 9.605469 " />
  </svg>
);

export const Logo = (props: React.ComponentProps<"svg">) => (
  <svg
    fill="currentColor"
    viewBox="0 0 90 24"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <defs>
      <clipPath id="a">
        <path d="M12.785 12.781h11.172V24H12.785Zm0 0" />
      </clipPath>
      <clipPath id="b">
        <path d="M.008 0h11.148v11.2H.008Zm0 0" />
      </clipPath>
    </defs>
    <g clipPath="url(#a)">
      <path
        d="M15.008 23.855c-.91-.242-1.61-.851-2.028-1.75-.19-.41-.19-.488-.19-3.73 0-3.309 0-3.309.288-3.89a3 3 0 0 1 1.485-1.458c.488-.226.488-.226 3.792-.226 3.31 0 3.31 0 3.887.289.672.332 1.133.8 1.453 1.488.227.488.227.488.25 3.563.028 2.632.012 3.148-.113 3.574-.309 1.078-1.102 1.87-2.168 2.152-.734.18-5.941.18-6.656-.012m6.637-1.46c.18-.086.433-.305.562-.473.234-.305.234-.305.234-3.547 0-3.238 0-3.238-.242-3.55-.476-.637-.441-.63-3.844-.63-2.71 0-3.07.02-3.312.149-.375.199-.707.617-.82 1.011-.098.399-.11 5.497 0 5.977.14.61.601 1.07 1.195 1.191.184.036 1.582.063 3.113.055 2.54-.02 2.809-.035 3.114-.183m0 0"
        opacity="50%"
      />
    </g>
    <path d="M2.86 23.09c-.766-.242-1.532-.93-1.88-1.688-.183-.402-.187-.515-.187-3.765 0-3.356 0-3.356.23-3.852.278-.598.836-1.176 1.422-1.46.43-.22.43-.22 3.746-.247 3.235-.023 3.32-.015 3.829.168.683.262 1.406.973 1.695 1.688.207.523.207.539.207 3.703 0 3.504 0 3.496-.567 4.32-.34.48-.82.848-1.44 1.082-.446.164-.665.172-3.583.164-2.297 0-3.203-.035-3.473-.113M13.879 11.695c-.672-.258-1.395-.992-1.645-1.672-.18-.503-.191-.59-.164-3.832.028-3.265.028-3.328.223-3.718.324-.653.758-1.09 1.395-1.403.578-.289.578-.289 3.93-.289 3.253 0 3.355.008 3.757.192.57.261 1.242.914 1.52 1.5.246.488.246.488.246 3.84 0 3.355 0 3.355-.29 3.937a2.9 2.9 0 0 1-1.398 1.395c-.39.199-.445.199-3.746.218-3.238.016-3.371.016-3.828-.168m0 0" />
    <g clipPath="url(#b)">
      <path d="M2.219 11.078c-.89-.254-1.602-.871-2.012-1.765-.2-.418-.2-.481-.2-3.743 0-3.308 0-3.308.235-3.804A3.34 3.34 0 0 1 1.773.234C2.27 0 2.27 0 5.574 0c3.301 0 3.301 0 3.801.234.633.297 1.23.895 1.527 1.532.235.496.235.496.235 3.804 0 3.313 0 3.313-.227 3.801-.32.688-.777 1.16-1.45 1.488-.585.29-.585.29-3.714.305-2.305.02-3.234-.008-3.527-.086m6.668-1.473c.34-.18.715-.617.8-.921.036-.13.06-1.567.06-3.2 0-3.308.01-3.28-.626-3.761-.312-.243-.312-.243-3.32-.27-1.653-.02-3.145 0-3.297.027-.156.024-.434.2-.617.372-.508.48-.54.777-.461 4.12.058 2.844.058 2.844.304 3.157.489.644.59.66 4.016.644 2.531-.02 2.902-.035 3.14-.168m0 0" />
    </g>
    <path d="m33.688 17.254-1.622-2.125 3.325-2.57a1.6 1.6 0 0 0-.696-.434 3 3 0 0 0-.957-.145q-.626.001-1.148.266a2.75 2.75 0 0 0-.903.707q-.379.445-.59 1.047c-.14.402-.206.84-.206 1.313q0 .655.246 1.257c.164.403.394.758.68 1.063q.432.46 1.019.734a3 3 0 0 0 1.27.278c.84 0 1.57-.243 2.199-.723q.942-.72 1.152-2.008l3.375.367a7.6 7.6 0 0 1-.879 2.387q-.614 1.053-1.504 1.797-.891.75-2.027 1.14a7.5 7.5 0 0 1-2.445.395 6.6 6.6 0 0 1-2.606-.512 6.1 6.1 0 0 1-2.039-1.402 6.3 6.3 0 0 1-1.32-2.11q-.474-1.223-.473-2.663-.001-1.44.473-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.177-.499 2.59-.5 2.195 0 3.777.973c1.059.644 1.855 1.582 2.398 2.804ZM47.973 6.684H46.69q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.254-4.239q1.26-1.296 3.48-1.297h1.23ZM54.723 6.684H53.44q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.258-4.239q1.255-1.296 3.476-1.297h1.23ZM61.21 17.254l-1.62-2.125 3.324-2.57a1.6 1.6 0 0 0-.695-.434 3 3 0 0 0-.953-.145q-.632.001-1.153.266-.52.26-.902.707-.38.445-.59 1.047-.206.603-.207 1.313 0 .655.246 1.257.252.605.68 1.063.433.46 1.023.734.586.277 1.266.278c.84 0 1.57-.243 2.2-.723q.94-.72 1.151-2.008l3.375.367a7.6 7.6 0 0 1-.878 2.387q-.614 1.053-1.504 1.797-.891.75-2.028 1.14A7.5 7.5 0 0 1 61.5 22a6.6 6.6 0 0 1-2.605-.512 6.1 6.1 0 0 1-2.04-1.402 6.3 6.3 0 0 1-1.32-2.11q-.473-1.223-.472-2.663-.001-1.44.472-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.178-.499 2.59-.5 2.199 0 3.781.973c1.055.644 1.852 1.582 2.395 2.804ZM76.805 12.348h-.758q-1.413 0-2.121.855c-.469.567-.703 1.29-.703 2.16v6.32h-3.559V8.942h3.164v2.41h.05c.317-.855.75-1.5 1.31-1.925q.838-.645 1.964-.645h.653ZM89.992 3.328v12.063c0 .89-.152 1.742-.457 2.543a6.6 6.6 0 0 1-1.285 2.113 6 6 0 0 1-1.988 1.43q-1.161.522-2.551.523a5.9 5.9 0 0 1-2.496-.523 6.1 6.1 0 0 1-1.965-1.43 6.8 6.8 0 0 1-1.293-2.125 7.3 7.3 0 0 1-.473-2.637q0-1.335.461-2.555a6.5 6.5 0 0 1 1.282-2.125 6.3 6.3 0 0 1 1.921-1.44 5.4 5.4 0 0 1 2.407-.54q.527 0 1.007.117.487.122 1.008.25v3.54a5 5 0 0 0-.851-.286 4 4 0 0 0-.953-.105q-1.285 0-2.106.879-.825.876-.824 2.293 0 1.417.809 2.296a2.65 2.65 0 0 0 2.015.875q1.359-.001 2.172-.875.811-.881.813-2.218V3.328Zm0 0" />
  </svg>
);

```

#### Svelte Implementation

> [!NOTE]
> Svelte version is not available in the open-source repository.

---

### Form 2 <a name="form-2"></a>

A premium form 2 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `form-2`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/form-2.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`
  - `field`
  - `input`
  - `input-group`
  - `textarea`

##### Component Source Code

###### File: `registry/blocks/form/2/submit-project-form.tsx`

```tsx
import { PlusIcon } from "lucide-react";
import { Button } from "@/components/ui/button";
import {
  Field,
  FieldDescription,
  FieldGroup,
  FieldLabel,
} from "@/components/ui/field";
import { Input } from "@/components/ui/input";
import {
  InputGroup,
  InputGroupAddon,
  InputGroupInput,
} from "@/components/ui/input-group";
import { Textarea } from "@/components/ui/textarea";

export function SubmitProjectForm() {
  return (
    <div className="relative w-full max-w-md bg-background p-4">
      <div className="-left-px -inset-y-6 absolute w-px bg-border" />
      <div className="-right-px -inset-y-6 absolute w-px bg-border" />
      <div className="-top-px -inset-x-6 absolute h-px bg-border" />
      <div className="-bottom-px -inset-x-6 absolute h-px bg-border" />
      <PlusIcon
        className="-left-[12.5px] -top-[12.5px] absolute size-6"
        strokeWidth={0.5}
      />
      <PlusIcon
        className="-right-[12.5px] -bottom-[12.5px] absolute size-6"
        strokeWidth={0.5}
      />

      <div className="rounded-md border border-border/60 p-[2px]">
        <div className="items-left flex flex-col justify-center gap-1 rounded-md border bg-card p-4 shadow-xs">
          <h2 className="font-medium text-xl">Submit Project</h2>
          <p className="text-muted-foreground text-sm">
            Submit your open source project for review.
          </p>
        </div>
      </div>
      <FieldGroup className="p-4">
        <Field className="gap-2">
          <FieldLabel htmlFor="name">Project Name</FieldLabel>
          <Input autoComplete="off" id="name" placeholder="e.g., shadcn/ui" />
          <FieldDescription>
            This is your public display name. Must be between 3 and 10
            characters. Must only contain letters, numbers, and underscores.
          </FieldDescription>
        </Field>
        <Field className="gap-2">
          <FieldLabel htmlFor="github">GitHub Repository URL</FieldLabel>
          <InputGroup>
            <InputGroupAddon>
              <GithubIcon />
            </InputGroupAddon>
            <InputGroupInput
              id="github"
              placeholder="e.g., https://github.com/shadcn/ui"
            />
          </InputGroup>
          <FieldDescription>
            The URL of your GitHub repository.
          </FieldDescription>
        </Field>
        <Field className="gap-2">
          <FieldLabel htmlFor="description">Project Description</FieldLabel>
          <Textarea
            id="description"
            placeholder="A brief description of your project..."
          />
          <FieldDescription>
            Provide a detailed description of your project.
          </FieldDescription>
        </Field>
      </FieldGroup>
      <div className="p-2">
        <Button className="w-full" type="submit">
          Submit Project
        </Button>
      </div>
    </div>
  );
}

const GithubIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 1024 1024" {...props}>
    <path
      clipRule="evenodd"
      d="M8 0C3.58 0 0 3.58 0 8C0 11.54 2.29 14.53 5.47 15.59C5.87 15.66 6.02 15.42 6.02 15.21C6.02 15.02 6.01 14.39 6.01 13.72C4 14.09 3.48 13.23 3.32 12.78C3.23 12.55 2.84 11.84 2.5 11.65C2.22 11.5 1.82 11.13 2.49 11.12C3.12 11.11 3.57 11.7 3.72 11.94C4.44 13.15 5.59 12.81 6.05 12.6C6.12 12.08 6.33 11.73 6.56 11.53C4.78 11.33 2.92 10.64 2.92 7.58C2.92 6.71 3.23 5.99 3.74 5.43C3.66 5.23 3.38 4.41 3.82 3.31C3.82 3.31 4.49 3.1 6.02 4.13C6.66 3.95 7.34 3.86 8.02 3.86C8.7 3.86 9.38 3.95 10.02 4.13C11.55 3.09 12.22 3.31 12.22 3.31C12.66 4.41 12.38 5.23 12.3 5.43C12.81 5.99 13.12 6.7 13.12 7.58C13.12 10.65 11.25 11.33 9.47 11.53C9.76 11.78 10.01 12.26 10.01 13.01C10.01 14.08 10 14.94 10 15.21C10 15.42 10.15 15.67 10.55 15.59C13.71 14.53 16 11.53 16 8C16 3.58 12.42 0 8 0Z"
      fill="currentColor"
      fillRule="evenodd"
      transform="scale(64)"
    />
  </svg>
);

```

#### Svelte Implementation

> [!NOTE]
> Svelte version is not available in the open-source repository.

---

### Form 3 <a name="form-3"></a>

A premium form 3 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `form-3`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/form-3.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`
  - `button-group`
  - `field`
  - `input`
  - `input-group`

##### Component Source Code

###### File: `registry/blocks/form/3/settings-form.tsx`

```tsx
import { AtSign, EyeIcon, LockIcon, PhoneIcon } from "lucide-react";
import { Button } from "@/components/ui/button";
import { ButtonGroup } from "@/components/ui/button-group";
import {
  Field,
  FieldContent,
  FieldDescription,
  FieldGroup,
  FieldLabel,
  FieldLegend,
  FieldSeparator,
  FieldSet,
} from "@/components/ui/field";
import { Input } from "@/components/ui/input";
import {
  InputGroup,
  InputGroupAddon,
  InputGroupButton,
  InputGroupInput,
} from "@/components/ui/input-group";

export function SettingsForm() {
  return (
    <div className="mx-auto w-full max-w-4xl">
      <FieldSet>
        <FieldLegend className="text-4xl">Account Settings</FieldLegend>
        <FieldDescription>
          Manage account and your personal information.
        </FieldDescription>
        <FieldSeparator />
        <FieldGroup className="gap-5">
          <div className="rounded-xl border border-border/60 p-0.5">
            <Field
              className="rounded-xl border bg-card px-4 py-8 shadow-xs dark:bg-card/50"
              orientation="responsive"
            >
              <FieldContent>
                <FieldLabel htmlFor="name">Your Name</FieldLabel>
                <FieldDescription>
                  Please enter a display name you are comfortable with.
                </FieldDescription>
              </FieldContent>
              <FieldContent>
                <ButtonGroup className="w-full">
                  <Input id="name" placeholder="Enter Your Name" />
                  <Button variant="outline">Save</Button>
                </ButtonGroup>
                <FieldDescription>Max 32 characters</FieldDescription>
              </FieldContent>
            </Field>
          </div>
          <div className="rounded-xl border border-border/60 p-0.5">
            <Field
              className="rounded-xl border bg-card px-4 py-8 shadow-xs dark:bg-card/50"
              orientation="responsive"
            >
              <FieldContent>
                <FieldLabel htmlFor="lastName">Your Email</FieldLabel>
                <FieldDescription>
                  Please enter a valid email address.
                </FieldDescription>
              </FieldContent>
              <FieldContent>
                <ButtonGroup className="w-full">
                  <InputGroup>
                    <InputGroupAddon>
                      <AtSign />
                    </InputGroupAddon>
                    <InputGroupInput
                      placeholder="Enter Your Email"
                      type="email"
                    />
                  </InputGroup>
                  <Button variant="outline">Save</Button>
                </ButtonGroup>
                <FieldDescription>Should be unique</FieldDescription>
              </FieldContent>
            </Field>
          </div>
          <div className="rounded-xl border border-border/60 p-0.5">
            <Field
              className="rounded-xl border bg-card px-4 py-8 shadow-xs dark:bg-card/50"
              orientation="responsive"
            >
              <FieldContent>
                <FieldLabel htmlFor="lastName">Your Password</FieldLabel>
                <FieldDescription>
                  please create a strong password
                </FieldDescription>
              </FieldContent>
              <FieldContent>
                <ButtonGroup className="w-full">
                  <InputGroup>
                    <InputGroupAddon>
                      <LockIcon />
                    </InputGroupAddon>
                    <InputGroupInput placeholder="XXxxXXX" type="password" />
                    <InputGroupAddon align="inline-end">
                      <InputGroupButton>
                        <EyeIcon />
                      </InputGroupButton>
                    </InputGroupAddon>
                  </InputGroup>
                  <Button variant="outline">Save</Button>
                </ButtonGroup>
                <FieldDescription>
                  Min 8 characters, Max 16 characters
                </FieldDescription>
              </FieldContent>
            </Field>
          </div>
          <div className="rounded-xl border border-border/60 p-0.5">
            <Field
              className="rounded-xl border bg-card px-4 py-8 shadow-xs dark:bg-card/50"
              orientation="responsive"
            >
              <FieldContent>
                <FieldLabel htmlFor="lastName">Your Phone Number</FieldLabel>
                <FieldDescription>
                  please provide your phone number
                </FieldDescription>
              </FieldContent>
              <FieldContent>
                <ButtonGroup className="w-full">
                  <InputGroup>
                    <InputGroupAddon>
                      <PhoneIcon />
                    </InputGroupAddon>
                    <InputGroupInput placeholder="+91 1234567890" type="tel" />
                  </InputGroup>
                  <Button variant="outline">Save</Button>
                </ButtonGroup>
                <FieldDescription>Min 10 characters</FieldDescription>
              </FieldContent>
            </Field>
          </div>
        </FieldGroup>
      </FieldSet>
    </div>
  );
}

```

#### Svelte Implementation

> [!NOTE]
> Svelte version is not available in the open-source repository.

---

## Header Blocks <a name="header"></a>

### Header 1 <a name="header-1"></a>

A premium header 1 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `header-1`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/header-1.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `registry/blocks/header/1/header.tsx`

```tsx
"use client";
import React from "react";
import { createPortal } from "react-dom";
import { Logo } from "@/components/logo";
import { MenuToggleIcon } from "@/components/menu-toggle-icon";
import { Button, buttonVariants } from "@/components/ui/button";
import { useScroll } from "@/hooks/use-scroll";
import { cn } from "@/lib/utils";

export function Header() {
  const [open, setOpen] = React.useState(false);
  const scrolled = useScroll(10);

  const links = [
    {
      label: "Features",
      href: "#",
    },
    {
      label: "Pricing",
      href: "#",
    },
    {
      label: "About",
      href: "#",
    },
  ];

  React.useEffect(() => {
    if (open) {
      document.body.style.overflow = "hidden";
    } else {
      document.body.style.overflow = "";
    }
    return () => {
      document.body.style.overflow = "";
    };
  }, [open]);

  return (
    <header
      className={cn("sticky top-0 z-50 w-full border-transparent border-b", {
        "border-border bg-background/95 backdrop-blur-lg supports-[backdrop-filter]:bg-background/50":
          scrolled,
      })}
    >
      <nav className="mx-auto flex h-14 w-full max-w-5xl items-center justify-between px-4">
        <div className="rounded-md p-2 hover:bg-accent">
          <Logo className="h-4" />
        </div>
        <div className="hidden items-center gap-2 md:flex">
          {links.map((link, i) => (
            <a
              className={buttonVariants({ variant: "ghost" })}
              href={link.href}
              key={i}
            >
              {link.label}
            </a>
          ))}
          <Button variant="outline">Sign In</Button>
          <Button>Get Started</Button>
        </div>
        <Button
          aria-controls="mobile-menu"
          aria-expanded={open}
          aria-label="Toggle menu"
          className="md:hidden"
          onClick={() => setOpen(!open)}
          size="icon"
          variant="outline"
        >
          <MenuToggleIcon className="size-5" duration={300} open={open} />
        </Button>
      </nav>
      <MobileMenu className="flex flex-col justify-between gap-2" open={open}>
        <div className="grid gap-y-2">
          {links.map((link) => (
            <a
              className={buttonVariants({
                variant: "ghost",
                className: "justify-start",
              })}
              href={link.href}
              key={link.label}
            >
              {link.label}
            </a>
          ))}
        </div>
        <div className="flex flex-col gap-2">
          <Button className="w-full bg-transparent" variant="outline">
            Sign In
          </Button>
          <Button className="w-full">Get Started</Button>
        </div>
      </MobileMenu>
    </header>
  );
}

type MobileMenuProps = React.ComponentProps<"div"> & {
  open: boolean;
};

function MobileMenu({ open, children, className, ...props }: MobileMenuProps) {
  if (!open || typeof window === "undefined") {
    return null;
  }

  return createPortal(
    <div
      className={cn(
        "bg-background/95 backdrop-blur-lg supports-[backdrop-filter]:bg-background/50",
        "fixed top-14 right-0 bottom-0 left-0 z-40 flex flex-col overflow-hidden border-y md:hidden"
      )}
      id="mobile-menu"
    >
      <div
        className={cn(
          "data-[slot=open]:zoom-in-97 ease-out data-[slot=open]:animate-in",
          "size-full p-4",
          className
        )}
        data-slot={open ? "open" : "closed"}
        {...props}
      >
        {children}
      </div>
    </div>,
    document.body
  );
}

```

###### File: `components/logo.tsx`

```tsx
import type React from "react";

export const LogoIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 24 24" {...props}>
    <path
      d="M 15.03125 23.851562 C 14.117188 23.609375 13.417969 23 13 22.101562 C 12.808594 21.691406 12.808594 21.613281 12.808594 18.375 C 12.808594 15.066406 12.808594 15.066406 13.097656 14.484375 C 13.429688 13.8125 13.898438 13.351562 14.585938 13.027344 C 15.074219 12.800781 15.074219 12.800781 18.386719 12.800781 C 21.699219 12.800781 21.699219 12.800781 22.28125 13.089844 C 22.953125 13.421875 23.414062 13.890625 23.738281 14.578125 C 23.964844 15.066406 23.964844 15.066406 23.988281 18.140625 C 24.015625 20.769531 24 21.285156 23.875 21.710938 C 23.5625 22.789062 22.769531 23.582031 21.699219 23.863281 C 20.964844 24.042969 15.746094 24.042969 15.03125 23.851562 Z M 21.679688 22.390625 C 21.863281 22.304688 22.117188 22.085938 22.246094 21.917969 C 22.480469 21.613281 22.480469 21.613281 22.480469 18.375 C 22.480469 15.136719 22.480469 15.136719 22.238281 14.824219 C 21.757812 14.1875 21.792969 14.195312 18.386719 14.195312 C 15.667969 14.195312 15.308594 14.214844 15.066406 14.34375 C 14.691406 14.542969 14.359375 14.960938 14.246094 15.355469 C 14.144531 15.753906 14.132812 20.847656 14.246094 21.328125 C 14.386719 21.9375 14.847656 22.398438 15.441406 22.519531 C 15.625 22.554688 17.027344 22.582031 18.5625 22.574219 C 21.105469 22.554688 21.375 22.539062 21.679688 22.390625 Z M 21.679688 22.390625 "
      opacity="50%"
    />
    <path d="M 2.859375 23.085938 C 2.089844 22.84375 1.324219 22.15625 0.976562 21.398438 C 0.792969 20.996094 0.785156 20.882812 0.785156 17.636719 C 0.785156 14.28125 0.785156 14.28125 1.019531 13.785156 C 1.296875 13.1875 1.855469 12.609375 2.441406 12.324219 C 2.875 12.105469 2.875 12.105469 6.195312 12.078125 C 9.4375 12.054688 9.523438 12.0625 10.03125 12.246094 C 10.71875 12.507812 11.441406 13.21875 11.730469 13.933594 C 11.9375 14.457031 11.9375 14.472656 11.9375 17.636719 C 11.9375 21.136719 11.9375 21.128906 11.371094 21.953125 C 11.03125 22.433594 10.550781 22.800781 9.925781 23.035156 C 9.480469 23.199219 9.261719 23.207031 6.335938 23.199219 C 4.035156 23.199219 3.128906 23.164062 2.859375 23.085938 Z M 2.859375 23.085938 " />
    <path d="M 13.898438 11.695312 C 13.226562 11.4375 12.503906 10.703125 12.25 10.023438 C 12.070312 9.519531 12.058594 9.433594 12.085938 6.195312 C 12.113281 2.929688 12.113281 2.867188 12.3125 2.476562 C 12.636719 1.824219 13.070312 1.386719 13.707031 1.074219 C 14.289062 0.785156 14.289062 0.785156 17.644531 0.785156 C 20.90625 0.785156 21.007812 0.792969 21.410156 0.976562 C 21.984375 1.238281 22.65625 1.890625 22.933594 2.476562 C 23.179688 2.964844 23.179688 2.964844 23.179688 6.316406 C 23.179688 9.667969 23.179688 9.667969 22.890625 10.25 C 22.578125 10.886719 22.140625 11.324219 21.488281 11.644531 C 21.097656 11.84375 21.042969 11.84375 17.734375 11.863281 C 14.492188 11.878906 14.359375 11.878906 13.898438 11.695312 Z M 13.898438 11.695312 " />
    <path d="M 2.214844 11.078125 C 1.324219 10.824219 0.609375 10.207031 0.199219 9.3125 C 0 8.894531 0 8.832031 0 5.574219 C 0 2.265625 0 2.265625 0.234375 1.769531 C 0.53125 1.132812 1.132812 0.535156 1.769531 0.238281 C 2.265625 0.00390625 2.265625 0.00390625 5.578125 0.00390625 C 8.886719 0.00390625 8.886719 0.00390625 9.386719 0.238281 C 10.019531 0.535156 10.621094 1.132812 10.917969 1.769531 C 11.152344 2.265625 11.152344 2.265625 11.152344 5.574219 C 11.152344 8.882812 11.152344 8.882812 10.925781 9.371094 C 10.605469 10.058594 10.144531 10.53125 9.472656 10.859375 C 8.886719 11.148438 8.886719 11.148438 5.75 11.164062 C 3.441406 11.183594 2.507812 11.15625 2.214844 11.078125 Z M 8.898438 9.605469 C 9.238281 9.425781 9.613281 8.988281 9.699219 8.683594 C 9.734375 8.554688 9.757812 7.117188 9.757812 5.488281 C 9.757812 2.179688 9.769531 2.207031 9.132812 1.726562 C 8.820312 1.484375 8.820312 1.484375 5.804688 1.457031 C 4.148438 1.4375 2.65625 1.457031 2.5 1.484375 C 2.34375 1.507812 2.066406 1.683594 1.882812 1.855469 C 1.375 2.335938 1.34375 2.632812 1.421875 5.976562 C 1.480469 8.816406 1.480469 8.816406 1.726562 9.128906 C 2.214844 9.773438 2.316406 9.789062 5.75 9.773438 C 8.285156 9.753906 8.660156 9.738281 8.898438 9.605469 Z M 8.898438 9.605469 " />
  </svg>
);

export const Logo = (props: React.ComponentProps<"svg">) => (
  <svg
    fill="currentColor"
    viewBox="0 0 90 24"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <defs>
      <clipPath id="a">
        <path d="M12.785 12.781h11.172V24H12.785Zm0 0" />
      </clipPath>
      <clipPath id="b">
        <path d="M.008 0h11.148v11.2H.008Zm0 0" />
      </clipPath>
    </defs>
    <g clipPath="url(#a)">
      <path
        d="M15.008 23.855c-.91-.242-1.61-.851-2.028-1.75-.19-.41-.19-.488-.19-3.73 0-3.309 0-3.309.288-3.89a3 3 0 0 1 1.485-1.458c.488-.226.488-.226 3.792-.226 3.31 0 3.31 0 3.887.289.672.332 1.133.8 1.453 1.488.227.488.227.488.25 3.563.028 2.632.012 3.148-.113 3.574-.309 1.078-1.102 1.87-2.168 2.152-.734.18-5.941.18-6.656-.012m6.637-1.46c.18-.086.433-.305.562-.473.234-.305.234-.305.234-3.547 0-3.238 0-3.238-.242-3.55-.476-.637-.441-.63-3.844-.63-2.71 0-3.07.02-3.312.149-.375.199-.707.617-.82 1.011-.098.399-.11 5.497 0 5.977.14.61.601 1.07 1.195 1.191.184.036 1.582.063 3.113.055 2.54-.02 2.809-.035 3.114-.183m0 0"
        opacity="50%"
      />
    </g>
    <path d="M2.86 23.09c-.766-.242-1.532-.93-1.88-1.688-.183-.402-.187-.515-.187-3.765 0-3.356 0-3.356.23-3.852.278-.598.836-1.176 1.422-1.46.43-.22.43-.22 3.746-.247 3.235-.023 3.32-.015 3.829.168.683.262 1.406.973 1.695 1.688.207.523.207.539.207 3.703 0 3.504 0 3.496-.567 4.32-.34.48-.82.848-1.44 1.082-.446.164-.665.172-3.583.164-2.297 0-3.203-.035-3.473-.113M13.879 11.695c-.672-.258-1.395-.992-1.645-1.672-.18-.503-.191-.59-.164-3.832.028-3.265.028-3.328.223-3.718.324-.653.758-1.09 1.395-1.403.578-.289.578-.289 3.93-.289 3.253 0 3.355.008 3.757.192.57.261 1.242.914 1.52 1.5.246.488.246.488.246 3.84 0 3.355 0 3.355-.29 3.937a2.9 2.9 0 0 1-1.398 1.395c-.39.199-.445.199-3.746.218-3.238.016-3.371.016-3.828-.168m0 0" />
    <g clipPath="url(#b)">
      <path d="M2.219 11.078c-.89-.254-1.602-.871-2.012-1.765-.2-.418-.2-.481-.2-3.743 0-3.308 0-3.308.235-3.804A3.34 3.34 0 0 1 1.773.234C2.27 0 2.27 0 5.574 0c3.301 0 3.301 0 3.801.234.633.297 1.23.895 1.527 1.532.235.496.235.496.235 3.804 0 3.313 0 3.313-.227 3.801-.32.688-.777 1.16-1.45 1.488-.585.29-.585.29-3.714.305-2.305.02-3.234-.008-3.527-.086m6.668-1.473c.34-.18.715-.617.8-.921.036-.13.06-1.567.06-3.2 0-3.308.01-3.28-.626-3.761-.312-.243-.312-.243-3.32-.27-1.653-.02-3.145 0-3.297.027-.156.024-.434.2-.617.372-.508.48-.54.777-.461 4.12.058 2.844.058 2.844.304 3.157.489.644.59.66 4.016.644 2.531-.02 2.902-.035 3.14-.168m0 0" />
    </g>
    <path d="m33.688 17.254-1.622-2.125 3.325-2.57a1.6 1.6 0 0 0-.696-.434 3 3 0 0 0-.957-.145q-.626.001-1.148.266a2.75 2.75 0 0 0-.903.707q-.379.445-.59 1.047c-.14.402-.206.84-.206 1.313q0 .655.246 1.257c.164.403.394.758.68 1.063q.432.46 1.019.734a3 3 0 0 0 1.27.278c.84 0 1.57-.243 2.199-.723q.942-.72 1.152-2.008l3.375.367a7.6 7.6 0 0 1-.879 2.387q-.614 1.053-1.504 1.797-.891.75-2.027 1.14a7.5 7.5 0 0 1-2.445.395 6.6 6.6 0 0 1-2.606-.512 6.1 6.1 0 0 1-2.039-1.402 6.3 6.3 0 0 1-1.32-2.11q-.474-1.223-.473-2.663-.001-1.44.473-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.177-.499 2.59-.5 2.195 0 3.777.973c1.059.644 1.855 1.582 2.398 2.804ZM47.973 6.684H46.69q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.254-4.239q1.26-1.296 3.48-1.297h1.23ZM54.723 6.684H53.44q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.258-4.239q1.255-1.296 3.476-1.297h1.23ZM61.21 17.254l-1.62-2.125 3.324-2.57a1.6 1.6 0 0 0-.695-.434 3 3 0 0 0-.953-.145q-.632.001-1.153.266-.52.26-.902.707-.38.445-.59 1.047-.206.603-.207 1.313 0 .655.246 1.257.252.605.68 1.063.433.46 1.023.734.586.277 1.266.278c.84 0 1.57-.243 2.2-.723q.94-.72 1.151-2.008l3.375.367a7.6 7.6 0 0 1-.878 2.387q-.614 1.053-1.504 1.797-.891.75-2.028 1.14A7.5 7.5 0 0 1 61.5 22a6.6 6.6 0 0 1-2.605-.512 6.1 6.1 0 0 1-2.04-1.402 6.3 6.3 0 0 1-1.32-2.11q-.473-1.223-.472-2.663-.001-1.44.472-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.178-.499 2.59-.5 2.199 0 3.781.973c1.055.644 1.852 1.582 2.395 2.804ZM76.805 12.348h-.758q-1.413 0-2.121.855c-.469.567-.703 1.29-.703 2.16v6.32h-3.559V8.942h3.164v2.41h.05c.317-.855.75-1.5 1.31-1.925q.838-.645 1.964-.645h.653ZM89.992 3.328v12.063c0 .89-.152 1.742-.457 2.543a6.6 6.6 0 0 1-1.285 2.113 6 6 0 0 1-1.988 1.43q-1.161.522-2.551.523a5.9 5.9 0 0 1-2.496-.523 6.1 6.1 0 0 1-1.965-1.43 6.8 6.8 0 0 1-1.293-2.125 7.3 7.3 0 0 1-.473-2.637q0-1.335.461-2.555a6.5 6.5 0 0 1 1.282-2.125 6.3 6.3 0 0 1 1.921-1.44 5.4 5.4 0 0 1 2.407-.54q.527 0 1.007.117.487.122 1.008.25v3.54a5 5 0 0 0-.851-.286 4 4 0 0 0-.953-.105q-1.285 0-2.106.879-.825.876-.824 2.293 0 1.417.809 2.296a2.65 2.65 0 0 0 2.015.875q1.359-.001 2.172-.875.811-.881.813-2.218V3.328Zm0 0" />
  </svg>
);

```

###### File: `components/menu-toggle-icon.tsx`

```tsx
"use client";
import type React from "react";
import { cn } from "@/lib/utils";

type MenuToggleProps = React.ComponentProps<"svg"> & {
  open: boolean;
  duration?: number;
};

export function MenuToggleIcon({
  open,
  className,
  fill = "none",
  stroke = "currentColor",
  strokeWidth = 2.5,
  strokeLinecap = "round",
  strokeLinejoin = "round",
  duration = 500,
  ...props
}: MenuToggleProps) {
  return (
    <svg
      className={cn(
        "transition-transform ease-in-out",
        open && "-rotate-45",
        className
      )}
      fill={fill}
      stroke={stroke}
      strokeLinecap={strokeLinecap}
      strokeLinejoin={strokeLinejoin}
      strokeWidth={strokeWidth}
      style={{
        transitionDuration: `${duration}ms`,
      }}
      viewBox="0 0 32 32"
      {...props}
    >
      <path
        className={cn(
          "transition-all ease-in-out",
          open
            ? "[stroke-dasharray:20_300] [stroke-dashoffset:-32.42px]"
            : "[stroke-dasharray:12_63]"
        )}
        d="M27 10 13 10C10.8 10 9 8.2 9 6 9 3.5 10.8 2 13 2 15.2 2 17 3.8 17 6L17 26C17 28.2 18.8 30 21 30 23.2 30 25 28.2 25 26 25 23.8 23.2 22 21 22L7 22"
        style={{
          transitionDuration: `${duration}ms`,
        }}
      />
      <path d="M7 16 27 16" />
    </svg>
  );
}

```

###### File: `hooks/use-scroll.ts`

```ts
"use client";
import React from "react";

export function useScroll(threshold: number) {
  const [scrolled, setScrolled] = React.useState(false);

  const onScroll = React.useCallback(() => {
    setScrolled(window.scrollY > threshold);
  }, [threshold]);

  React.useEffect(() => {
    window.addEventListener("scroll", onScroll);
    return () => window.removeEventListener("scroll", onScroll);
  }, [onScroll]);

  // also check on first load
  React.useEffect(() => {
    onScroll();
  }, [onScroll]);

  return scrolled;
}

```

#### Svelte Implementation

- **Registry Key**: `header-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/header-one.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:portal`

##### Component Source Code

###### File: `src/lib/components/efferd/header/header-one/header-one.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { createScroll } from "$lib/hooks/use-scroll.svelte";
	import Logo from "$lib/svgs/logo.svelte";
	import { cn } from "$lib/utils";
	import MobileNav from "./mobile-nav.svelte";
	import { navLinks } from "./nav-links";

	let scroll = createScroll(10);
</script>

<header
	class={cn(
		"sticky top-0 z-50 w-full border-b border-transparent",
		scroll.scrolled &&
			"border-border bg-background/95 backdrop-blur-sm supports-backdrop-filter:bg-background/50"
	)}
>
	<nav class="mx-auto flex h-14 w-full max-w-5xl items-center justify-between px-4">
		<a class="w-fit rounded-md p-2 hover:bg-muted dark:hover:bg-muted/50" href="/">
			<Logo class="h-4" />
		</a>
		<div class="hidden items-center gap-2 md:flex">
			{#each navLinks as link}
				<Button href={link.href} size="sm" variant="ghost">
					{link.label}
				</Button>
			{/each}
			<Button size="sm" variant="outline">Sign In</Button>
			<Button size="sm">Get Started</Button>
		</div>
		<MobileNav />
	</nav>
</header>

```

###### File: `src/lib/components/efferd/header/header-one/mobile-nav.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { Portal, PortalBackdrop } from "$lib/components/ui/portal";
	import { cn } from "$lib/utils";
	import { MenuIcon, XIcon } from "@lucide/svelte";
	import { navLinks } from "./nav-links";
	import { fade } from "svelte/transition";

	let open = $state(false);
</script>

<div class="md:hidden">
	<Button
		aria-controls="mobile-menu"
		aria-expanded={open}
		aria-label="Toggle menu"
		class="md:hidden"
		onclick={() => (open = !open)}
		size="icon"
		variant="outline"
	>
		{#if open}
			<XIcon class="size-4.5" />
		{:else}
			<MenuIcon class="size-4.5" />
		{/if}
	</Button>
	{#if open}
		<Portal class="top-14">
			<PortalBackdrop />
			<div
				class={cn(
					"ease-out data-[slot=open]:animate-in data-[slot=open]:zoom-in-97",
					"size-full p-4"
				)}
				out:fade|global={{ duration: 150 }}
				data-slot={open ? "open" : "closed"}
			>
				<div class="grid gap-y-2">
					{#each navLinks as link}
						<Button class="justify-start" variant="ghost" href={link.href}>
							{link.label}
						</Button>
					{/each}
				</div>
				<div class="mt-12 flex flex-col gap-2">
					<Button class="w-full" variant="outline">Sign In</Button>
					<Button class="w-full">Get Started</Button>
				</div>
			</div>
		</Portal>
	{/if}
</div>

```

###### File: `src/lib/components/efferd/header/header-one/nav-links.ts`

```ts
export let navLinks = [
	{
		label: "Features",
		href: "/"
	},
	{
		label: "Pricing",
		href: "/"
	},
	{
		label: "About",
		href: "/"
	}
];

```

###### File: `src/lib/hooks/use-scroll.svelte.ts`

```ts
class ScrollState {
	#scrolled = $state(false);

	readonly #downThreshold: number;
	readonly #upThreshold: number;

	constructor(downThreshold: number, upThreshold?: number) {
		this.#downThreshold = downThreshold;
		this.#upThreshold = upThreshold ?? downThreshold / 2;

		$effect(() => {
			const handleScroll = () => {
				const y = window.scrollY;
				// Hysteresis Logic: Only update scrolled state when crossing thresholds in the appropriate direction
				this.#scrolled = this.#scrolled
					? y > this.#upThreshold // currently scrolled → only reset below upThreshold
					: y > this.#downThreshold; // currently not scrolled → only set above downThreshold
			};

			window.addEventListener("scroll", handleScroll, { passive: true });
			handleScroll();

			return () => window.removeEventListener("scroll", handleScroll);
		});
	}

	get scrolled(): boolean {
		return this.#scrolled;
	}
}

export function createScroll(downThreshold: number, upThreshold?: number): ScrollState {
	return new ScrollState(downThreshold, upThreshold);
}

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

---

### Header 2 <a name="header-2"></a>

A premium header 2 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `header-2`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/header-2.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `registry/blocks/header/2/header.tsx`

```tsx
"use client";
import React from "react";
import { createPortal } from "react-dom";
import { Logo } from "@/components/logo";
import { MenuToggleIcon } from "@/components/menu-toggle-icon";
import { Button, buttonVariants } from "@/components/ui/button";
import { useScroll } from "@/hooks/use-scroll";
import { cn } from "@/lib/utils";

export function Header() {
  const [open, setOpen] = React.useState(false);
  const scrolled = useScroll(10);

  const links = [
    {
      label: "Features",
      href: "#",
    },
    {
      label: "Pricing",
      href: "#",
    },
    {
      label: "About",
      href: "#",
    },
  ];

  React.useEffect(() => {
    if (open) {
      // Disable scroll
      document.body.style.overflow = "hidden";
    } else {
      // Re-enable scroll
      document.body.style.overflow = "";
    }

    // Cleanup when component unmounts (important for Next.js)
    return () => {
      document.body.style.overflow = "";
    };
  }, [open]);

  return (
    <header
      className={cn(
        "sticky top-0 z-50 mx-auto w-full max-w-4xl border-transparent border-b md:rounded-md md:border md:transition-all md:ease-out",
        {
          "border-border bg-background/95 backdrop-blur-lg supports-[backdrop-filter]:bg-background/50 md:top-4 md:max-w-3xl md:shadow":
            scrolled,
        }
      )}
    >
      <nav
        className={cn(
          "flex h-14 w-full items-center justify-between px-4 md:h-12 md:transition-all md:ease-out",
          {
            "md:px-2": scrolled,
          }
        )}
      >
        <a className="rounded-md p-2 hover:bg-accent" href="#">
          <Logo className="h-4" />
        </a>
        <div className="hidden items-center gap-2 md:flex">
          {links.map((link, i) => (
            <a
              className={buttonVariants({ variant: "ghost" })}
              href={link.href}
              key={i}
            >
              {link.label}
            </a>
          ))}
          <Button variant="outline">Sign In</Button>
          <Button>Get Started</Button>
        </div>
        <Button
          className="md:hidden"
          onClick={() => setOpen(!open)}
          size="icon"
          variant="outline"
        >
          <MenuToggleIcon className="size-5" duration={300} open={open} />
        </Button>
      </nav>

      <MobileMenu className="flex flex-col justify-between gap-2" open={open}>
        <div className="grid gap-y-2">
          {links.map((link) => (
            <a
              className={buttonVariants({
                variant: "ghost",
                className: "justify-start",
              })}
              href={link.href}
              key={link.label}
            >
              {link.label}
            </a>
          ))}
        </div>
        <div className="flex flex-col gap-2">
          <Button className="w-full bg-transparent" variant="outline">
            Sign In
          </Button>
          <Button className="w-full">Get Started</Button>
        </div>
      </MobileMenu>
    </header>
  );
}

type MobileMenuProps = React.ComponentProps<"div"> & {
  open: boolean;
};

function MobileMenu({ open, children, className, ...props }: MobileMenuProps) {
  if (!open || typeof window === "undefined") {
    return null;
  }

  return createPortal(
    <div
      className={cn(
        "bg-background/95 backdrop-blur-lg supports-[backdrop-filter]:bg-background/50",
        "fixed top-14 right-0 bottom-0 left-0 z-40 flex flex-col overflow-hidden border-y md:hidden"
      )}
      id="mobile-menu"
    >
      <div
        className={cn(
          "data-[slot=open]:zoom-in-97 ease-out data-[slot=open]:animate-in",
          "size-full p-4",
          className
        )}
        data-slot={open ? "open" : "closed"}
        {...props}
      >
        {children}
      </div>
    </div>,
    document.body
  );
}

```

###### File: `components/logo.tsx`

```tsx
import type React from "react";

export const LogoIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 24 24" {...props}>
    <path
      d="M 15.03125 23.851562 C 14.117188 23.609375 13.417969 23 13 22.101562 C 12.808594 21.691406 12.808594 21.613281 12.808594 18.375 C 12.808594 15.066406 12.808594 15.066406 13.097656 14.484375 C 13.429688 13.8125 13.898438 13.351562 14.585938 13.027344 C 15.074219 12.800781 15.074219 12.800781 18.386719 12.800781 C 21.699219 12.800781 21.699219 12.800781 22.28125 13.089844 C 22.953125 13.421875 23.414062 13.890625 23.738281 14.578125 C 23.964844 15.066406 23.964844 15.066406 23.988281 18.140625 C 24.015625 20.769531 24 21.285156 23.875 21.710938 C 23.5625 22.789062 22.769531 23.582031 21.699219 23.863281 C 20.964844 24.042969 15.746094 24.042969 15.03125 23.851562 Z M 21.679688 22.390625 C 21.863281 22.304688 22.117188 22.085938 22.246094 21.917969 C 22.480469 21.613281 22.480469 21.613281 22.480469 18.375 C 22.480469 15.136719 22.480469 15.136719 22.238281 14.824219 C 21.757812 14.1875 21.792969 14.195312 18.386719 14.195312 C 15.667969 14.195312 15.308594 14.214844 15.066406 14.34375 C 14.691406 14.542969 14.359375 14.960938 14.246094 15.355469 C 14.144531 15.753906 14.132812 20.847656 14.246094 21.328125 C 14.386719 21.9375 14.847656 22.398438 15.441406 22.519531 C 15.625 22.554688 17.027344 22.582031 18.5625 22.574219 C 21.105469 22.554688 21.375 22.539062 21.679688 22.390625 Z M 21.679688 22.390625 "
      opacity="50%"
    />
    <path d="M 2.859375 23.085938 C 2.089844 22.84375 1.324219 22.15625 0.976562 21.398438 C 0.792969 20.996094 0.785156 20.882812 0.785156 17.636719 C 0.785156 14.28125 0.785156 14.28125 1.019531 13.785156 C 1.296875 13.1875 1.855469 12.609375 2.441406 12.324219 C 2.875 12.105469 2.875 12.105469 6.195312 12.078125 C 9.4375 12.054688 9.523438 12.0625 10.03125 12.246094 C 10.71875 12.507812 11.441406 13.21875 11.730469 13.933594 C 11.9375 14.457031 11.9375 14.472656 11.9375 17.636719 C 11.9375 21.136719 11.9375 21.128906 11.371094 21.953125 C 11.03125 22.433594 10.550781 22.800781 9.925781 23.035156 C 9.480469 23.199219 9.261719 23.207031 6.335938 23.199219 C 4.035156 23.199219 3.128906 23.164062 2.859375 23.085938 Z M 2.859375 23.085938 " />
    <path d="M 13.898438 11.695312 C 13.226562 11.4375 12.503906 10.703125 12.25 10.023438 C 12.070312 9.519531 12.058594 9.433594 12.085938 6.195312 C 12.113281 2.929688 12.113281 2.867188 12.3125 2.476562 C 12.636719 1.824219 13.070312 1.386719 13.707031 1.074219 C 14.289062 0.785156 14.289062 0.785156 17.644531 0.785156 C 20.90625 0.785156 21.007812 0.792969 21.410156 0.976562 C 21.984375 1.238281 22.65625 1.890625 22.933594 2.476562 C 23.179688 2.964844 23.179688 2.964844 23.179688 6.316406 C 23.179688 9.667969 23.179688 9.667969 22.890625 10.25 C 22.578125 10.886719 22.140625 11.324219 21.488281 11.644531 C 21.097656 11.84375 21.042969 11.84375 17.734375 11.863281 C 14.492188 11.878906 14.359375 11.878906 13.898438 11.695312 Z M 13.898438 11.695312 " />
    <path d="M 2.214844 11.078125 C 1.324219 10.824219 0.609375 10.207031 0.199219 9.3125 C 0 8.894531 0 8.832031 0 5.574219 C 0 2.265625 0 2.265625 0.234375 1.769531 C 0.53125 1.132812 1.132812 0.535156 1.769531 0.238281 C 2.265625 0.00390625 2.265625 0.00390625 5.578125 0.00390625 C 8.886719 0.00390625 8.886719 0.00390625 9.386719 0.238281 C 10.019531 0.535156 10.621094 1.132812 10.917969 1.769531 C 11.152344 2.265625 11.152344 2.265625 11.152344 5.574219 C 11.152344 8.882812 11.152344 8.882812 10.925781 9.371094 C 10.605469 10.058594 10.144531 10.53125 9.472656 10.859375 C 8.886719 11.148438 8.886719 11.148438 5.75 11.164062 C 3.441406 11.183594 2.507812 11.15625 2.214844 11.078125 Z M 8.898438 9.605469 C 9.238281 9.425781 9.613281 8.988281 9.699219 8.683594 C 9.734375 8.554688 9.757812 7.117188 9.757812 5.488281 C 9.757812 2.179688 9.769531 2.207031 9.132812 1.726562 C 8.820312 1.484375 8.820312 1.484375 5.804688 1.457031 C 4.148438 1.4375 2.65625 1.457031 2.5 1.484375 C 2.34375 1.507812 2.066406 1.683594 1.882812 1.855469 C 1.375 2.335938 1.34375 2.632812 1.421875 5.976562 C 1.480469 8.816406 1.480469 8.816406 1.726562 9.128906 C 2.214844 9.773438 2.316406 9.789062 5.75 9.773438 C 8.285156 9.753906 8.660156 9.738281 8.898438 9.605469 Z M 8.898438 9.605469 " />
  </svg>
);

export const Logo = (props: React.ComponentProps<"svg">) => (
  <svg
    fill="currentColor"
    viewBox="0 0 90 24"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <defs>
      <clipPath id="a">
        <path d="M12.785 12.781h11.172V24H12.785Zm0 0" />
      </clipPath>
      <clipPath id="b">
        <path d="M.008 0h11.148v11.2H.008Zm0 0" />
      </clipPath>
    </defs>
    <g clipPath="url(#a)">
      <path
        d="M15.008 23.855c-.91-.242-1.61-.851-2.028-1.75-.19-.41-.19-.488-.19-3.73 0-3.309 0-3.309.288-3.89a3 3 0 0 1 1.485-1.458c.488-.226.488-.226 3.792-.226 3.31 0 3.31 0 3.887.289.672.332 1.133.8 1.453 1.488.227.488.227.488.25 3.563.028 2.632.012 3.148-.113 3.574-.309 1.078-1.102 1.87-2.168 2.152-.734.18-5.941.18-6.656-.012m6.637-1.46c.18-.086.433-.305.562-.473.234-.305.234-.305.234-3.547 0-3.238 0-3.238-.242-3.55-.476-.637-.441-.63-3.844-.63-2.71 0-3.07.02-3.312.149-.375.199-.707.617-.82 1.011-.098.399-.11 5.497 0 5.977.14.61.601 1.07 1.195 1.191.184.036 1.582.063 3.113.055 2.54-.02 2.809-.035 3.114-.183m0 0"
        opacity="50%"
      />
    </g>
    <path d="M2.86 23.09c-.766-.242-1.532-.93-1.88-1.688-.183-.402-.187-.515-.187-3.765 0-3.356 0-3.356.23-3.852.278-.598.836-1.176 1.422-1.46.43-.22.43-.22 3.746-.247 3.235-.023 3.32-.015 3.829.168.683.262 1.406.973 1.695 1.688.207.523.207.539.207 3.703 0 3.504 0 3.496-.567 4.32-.34.48-.82.848-1.44 1.082-.446.164-.665.172-3.583.164-2.297 0-3.203-.035-3.473-.113M13.879 11.695c-.672-.258-1.395-.992-1.645-1.672-.18-.503-.191-.59-.164-3.832.028-3.265.028-3.328.223-3.718.324-.653.758-1.09 1.395-1.403.578-.289.578-.289 3.93-.289 3.253 0 3.355.008 3.757.192.57.261 1.242.914 1.52 1.5.246.488.246.488.246 3.84 0 3.355 0 3.355-.29 3.937a2.9 2.9 0 0 1-1.398 1.395c-.39.199-.445.199-3.746.218-3.238.016-3.371.016-3.828-.168m0 0" />
    <g clipPath="url(#b)">
      <path d="M2.219 11.078c-.89-.254-1.602-.871-2.012-1.765-.2-.418-.2-.481-.2-3.743 0-3.308 0-3.308.235-3.804A3.34 3.34 0 0 1 1.773.234C2.27 0 2.27 0 5.574 0c3.301 0 3.301 0 3.801.234.633.297 1.23.895 1.527 1.532.235.496.235.496.235 3.804 0 3.313 0 3.313-.227 3.801-.32.688-.777 1.16-1.45 1.488-.585.29-.585.29-3.714.305-2.305.02-3.234-.008-3.527-.086m6.668-1.473c.34-.18.715-.617.8-.921.036-.13.06-1.567.06-3.2 0-3.308.01-3.28-.626-3.761-.312-.243-.312-.243-3.32-.27-1.653-.02-3.145 0-3.297.027-.156.024-.434.2-.617.372-.508.48-.54.777-.461 4.12.058 2.844.058 2.844.304 3.157.489.644.59.66 4.016.644 2.531-.02 2.902-.035 3.14-.168m0 0" />
    </g>
    <path d="m33.688 17.254-1.622-2.125 3.325-2.57a1.6 1.6 0 0 0-.696-.434 3 3 0 0 0-.957-.145q-.626.001-1.148.266a2.75 2.75 0 0 0-.903.707q-.379.445-.59 1.047c-.14.402-.206.84-.206 1.313q0 .655.246 1.257c.164.403.394.758.68 1.063q.432.46 1.019.734a3 3 0 0 0 1.27.278c.84 0 1.57-.243 2.199-.723q.942-.72 1.152-2.008l3.375.367a7.6 7.6 0 0 1-.879 2.387q-.614 1.053-1.504 1.797-.891.75-2.027 1.14a7.5 7.5 0 0 1-2.445.395 6.6 6.6 0 0 1-2.606-.512 6.1 6.1 0 0 1-2.039-1.402 6.3 6.3 0 0 1-1.32-2.11q-.474-1.223-.473-2.663-.001-1.44.473-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.177-.499 2.59-.5 2.195 0 3.777.973c1.059.644 1.855 1.582 2.398 2.804ZM47.973 6.684H46.69q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.254-4.239q1.26-1.296 3.48-1.297h1.23ZM54.723 6.684H53.44q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.258-4.239q1.255-1.296 3.476-1.297h1.23ZM61.21 17.254l-1.62-2.125 3.324-2.57a1.6 1.6 0 0 0-.695-.434 3 3 0 0 0-.953-.145q-.632.001-1.153.266-.52.26-.902.707-.38.445-.59 1.047-.206.603-.207 1.313 0 .655.246 1.257.252.605.68 1.063.433.46 1.023.734.586.277 1.266.278c.84 0 1.57-.243 2.2-.723q.94-.72 1.151-2.008l3.375.367a7.6 7.6 0 0 1-.878 2.387q-.614 1.053-1.504 1.797-.891.75-2.028 1.14A7.5 7.5 0 0 1 61.5 22a6.6 6.6 0 0 1-2.605-.512 6.1 6.1 0 0 1-2.04-1.402 6.3 6.3 0 0 1-1.32-2.11q-.473-1.223-.472-2.663-.001-1.44.472-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.178-.499 2.59-.5 2.199 0 3.781.973c1.055.644 1.852 1.582 2.395 2.804ZM76.805 12.348h-.758q-1.413 0-2.121.855c-.469.567-.703 1.29-.703 2.16v6.32h-3.559V8.942h3.164v2.41h.05c.317-.855.75-1.5 1.31-1.925q.838-.645 1.964-.645h.653ZM89.992 3.328v12.063c0 .89-.152 1.742-.457 2.543a6.6 6.6 0 0 1-1.285 2.113 6 6 0 0 1-1.988 1.43q-1.161.522-2.551.523a5.9 5.9 0 0 1-2.496-.523 6.1 6.1 0 0 1-1.965-1.43 6.8 6.8 0 0 1-1.293-2.125 7.3 7.3 0 0 1-.473-2.637q0-1.335.461-2.555a6.5 6.5 0 0 1 1.282-2.125 6.3 6.3 0 0 1 1.921-1.44 5.4 5.4 0 0 1 2.407-.54q.527 0 1.007.117.487.122 1.008.25v3.54a5 5 0 0 0-.851-.286 4 4 0 0 0-.953-.105q-1.285 0-2.106.879-.825.876-.824 2.293 0 1.417.809 2.296a2.65 2.65 0 0 0 2.015.875q1.359-.001 2.172-.875.811-.881.813-2.218V3.328Zm0 0" />
  </svg>
);

```

###### File: `components/menu-toggle-icon.tsx`

```tsx
"use client";
import type React from "react";
import { cn } from "@/lib/utils";

type MenuToggleProps = React.ComponentProps<"svg"> & {
  open: boolean;
  duration?: number;
};

export function MenuToggleIcon({
  open,
  className,
  fill = "none",
  stroke = "currentColor",
  strokeWidth = 2.5,
  strokeLinecap = "round",
  strokeLinejoin = "round",
  duration = 500,
  ...props
}: MenuToggleProps) {
  return (
    <svg
      className={cn(
        "transition-transform ease-in-out",
        open && "-rotate-45",
        className
      )}
      fill={fill}
      stroke={stroke}
      strokeLinecap={strokeLinecap}
      strokeLinejoin={strokeLinejoin}
      strokeWidth={strokeWidth}
      style={{
        transitionDuration: `${duration}ms`,
      }}
      viewBox="0 0 32 32"
      {...props}
    >
      <path
        className={cn(
          "transition-all ease-in-out",
          open
            ? "[stroke-dasharray:20_300] [stroke-dashoffset:-32.42px]"
            : "[stroke-dasharray:12_63]"
        )}
        d="M27 10 13 10C10.8 10 9 8.2 9 6 9 3.5 10.8 2 13 2 15.2 2 17 3.8 17 6L17 26C17 28.2 18.8 30 21 30 23.2 30 25 28.2 25 26 25 23.8 23.2 22 21 22L7 22"
        style={{
          transitionDuration: `${duration}ms`,
        }}
      />
      <path d="M7 16 27 16" />
    </svg>
  );
}

```

###### File: `hooks/use-scroll.ts`

```ts
"use client";
import React from "react";

export function useScroll(threshold: number) {
  const [scrolled, setScrolled] = React.useState(false);

  const onScroll = React.useCallback(() => {
    setScrolled(window.scrollY > threshold);
  }, [threshold]);

  React.useEffect(() => {
    window.addEventListener("scroll", onScroll);
    return () => window.removeEventListener("scroll", onScroll);
  }, [onScroll]);

  // also check on first load
  React.useEffect(() => {
    onScroll();
  }, [onScroll]);

  return scrolled;
}

```

#### Svelte Implementation

- **Registry Key**: `header-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/header-two.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:portal`

##### Component Source Code

###### File: `src/lib/components/efferd/header/header-two/header-two.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { createScroll } from "$lib/hooks/use-scroll.svelte";
	import Logo from "$lib/svgs/logo.svelte";
	import { cn } from "$lib/utils";
	import MobileNav from "./mobile-nav.svelte";
	import { navLinks } from "./nav-links";
	let scroll = createScroll(10);
</script>

<header
	class={cn(
		"sticky top-0 z-50 mx-auto w-full max-w-4xl border-b border-transparent md:rounded-md md:border md:transition-all md:ease-out",
		scroll.scrolled &&
			"border-border bg-background/95 backdrop-blur-sm supports-backdrop-filter:bg-background/50 md:top-2 md:max-w-3xl md:shadow"
	)}
>
	<nav
		class={cn(
			"flex h-14 w-full items-center justify-between px-4 md:h-12 md:transition-all md:ease-out",
			scroll.scrolled && "md:px-2"
		)}
	>
		<a class="rounded-md p-2 hover:bg-muted dark:hover:bg-muted/50" href="/">
			<Logo class="h-4" />
		</a>
		<div class="hidden items-center gap-2 md:flex">
			<div>
				{#each navLinks as { label, href }}
					<Button size="sm" variant="ghost" {href}>
						{label}
					</Button>
				{/each}
			</div>
			<Button size="sm" variant="outline">Sign In</Button>
			<Button size="sm">Get Started</Button>
		</div>
		<MobileNav />
	</nav>
</header>

```

###### File: `src/lib/components/efferd/header/header-two/mobile-nav.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { Portal, PortalBackdrop } from "$lib/components/ui/portal";
	import { cn } from "$lib/utils";
	import { MenuIcon, XIcon } from "@lucide/svelte";
	import { navLinks } from "./nav-links";
	let open = $state(false);
</script>

<div class="md:hidden">
	<Button
		aria-controls="mobile-menu"
		aria-expanded={open}
		aria-label="Toggle menu"
		class="md:hidden"
		size="icon"
		variant="outline"
		onclick={() => (open = !open)}
	>
		{#if open}
			<XIcon class="size-4.5" />
		{:else}
			<MenuIcon class="size-4.5" />
		{/if}
	</Button>
	{#if open}
		<Portal class="top-14">
			<PortalBackdrop />
			<div
				class={cn(
					"ease-out data-[slot=open]:animate-in data-[slot=open]:zoom-in-97",
					"size-full p-4"
				)}
				data-slot={open ? "open" : "closed"}
			>
				<div class="grid gap-y-2">
					{#each navLinks as link, i}
						<Button class="justify-start" variant="ghost" href={link.href}>
							{link.label}
						</Button>
					{/each}
				</div>
				<div class="mt-12 flex flex-col gap-2">
					<Button class="w-full" variant="outline">Sign In</Button>
					<Button class="w-full">Get Started</Button>
				</div>
			</div>
		</Portal>
	{/if}
</div>

```

###### File: `src/lib/components/efferd/header/header-two/nav-links.ts`

```ts
let navLinks = [
	{
		label: "Features",
		href: "/"
	},
	{
		label: "Pricing",
		href: "/"
	},
	{
		label: "About",
		href: "/"
	}
];

export { navLinks };

```

###### File: `src/lib/hooks/use-scroll.svelte.ts`

```ts
class ScrollState {
	#scrolled = $state(false);

	readonly #downThreshold: number;
	readonly #upThreshold: number;

	constructor(downThreshold: number, upThreshold?: number) {
		this.#downThreshold = downThreshold;
		this.#upThreshold = upThreshold ?? downThreshold / 2;

		$effect(() => {
			const handleScroll = () => {
				const y = window.scrollY;
				// Hysteresis Logic: Only update scrolled state when crossing thresholds in the appropriate direction
				this.#scrolled = this.#scrolled
					? y > this.#upThreshold // currently scrolled → only reset below upThreshold
					: y > this.#downThreshold; // currently not scrolled → only set above downThreshold
			};

			window.addEventListener("scroll", handleScroll, { passive: true });
			handleScroll();

			return () => window.removeEventListener("scroll", handleScroll);
		});
	}

	get scrolled(): boolean {
		return this.#scrolled;
	}
}

export function createScroll(downThreshold: number, upThreshold?: number): ScrollState {
	return new ScrollState(downThreshold, upThreshold);
}

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

---

### Header 3 <a name="header-3"></a>

A premium header 3 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `header-3`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/header-3.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`
  - `navigation-menu`

##### Component Source Code

###### File: `registry/blocks/header/3/header.tsx`

```tsx
"use client";
import {
  BarChart,
  CodeIcon,
  FileText,
  GlobeIcon,
  Handshake,
  HelpCircle,
  LayersIcon,
  Leaf,
  type LucideIcon,
  PlugIcon,
  RotateCcw,
  Shield,
  Star,
  UserPlusIcon,
  Users,
} from "lucide-react";
import React from "react";
import { createPortal } from "react-dom";
import { Logo } from "@/components/logo";
import { MenuToggleIcon } from "@/components/menu-toggle-icon";
import { Button } from "@/components/ui/button";
import {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuItem,
  NavigationMenuLink,
  NavigationMenuList,
  NavigationMenuTrigger,
} from "@/components/ui/navigation-menu";
import { useScroll } from "@/hooks/use-scroll";
import { cn } from "@/lib/utils";

type LinkItem = {
  title: string;
  href: string;
  icon: LucideIcon;
  description?: string;
};

export function Header() {
  const [open, setOpen] = React.useState(false);
  const scrolled = useScroll(10);

  React.useEffect(() => {
    if (open) {
      document.body.style.overflow = "hidden";
    } else {
      document.body.style.overflow = "";
    }
    return () => {
      document.body.style.overflow = "";
    };
  }, [open]);

  return (
    <header
      className={cn("sticky top-0 z-50 w-full border-transparent border-b", {
        "border-border bg-background/95 backdrop-blur-lg supports-[backdrop-filter]:bg-background/50":
          scrolled,
      })}
    >
      <nav className="mx-auto flex h-14 w-full max-w-5xl items-center justify-between px-4">
        <div className="flex items-center gap-5">
          <a className="rounded-md p-2 hover:bg-accent" href="#">
            <Logo className="h-4" />
          </a>
          <NavigationMenu className="hidden md:flex">
            <NavigationMenuList>
              <NavigationMenuItem>
                <NavigationMenuTrigger className="bg-transparent">
                  Product
                </NavigationMenuTrigger>
                <NavigationMenuContent className="bg-muted/50 p-1 pr-1.5 dark:bg-background">
                  <ul className="grid w-lg grid-cols-2 gap-2 rounded-md border bg-popover p-2 shadow">
                    {productLinks.map((item, i) => (
                      <li key={i}>
                        <ListItem {...item} />
                      </li>
                    ))}
                  </ul>
                  <div className="p-2">
                    <p className="text-muted-foreground text-sm">
                      Interested?{" "}
                      <a
                        className="font-medium text-foreground hover:underline"
                        href="#"
                      >
                        Schedule a demo
                      </a>
                    </p>
                  </div>
                </NavigationMenuContent>
              </NavigationMenuItem>
              <NavigationMenuItem>
                <NavigationMenuTrigger className="bg-transparent">
                  Company
                </NavigationMenuTrigger>
                <NavigationMenuContent className="bg-muted/50 p-1 pr-1.5 pb-1.5 dark:bg-background">
                  <div className="grid w-lg grid-cols-2 gap-2">
                    <ul className="space-y-2 rounded-md border bg-popover p-2 shadow">
                      {companyLinks.map((item, i) => (
                        <li key={i}>
                          <ListItem {...item} />
                        </li>
                      ))}
                    </ul>
                    <ul className="space-y-2 p-3">
                      {companyLinks2.map((item, i) => (
                        <li key={i}>
                          <NavigationMenuLink
                            className="flex-row items-center gap-x-2"
                            href={item.href}
                          >
                            <item.icon className="size-4 text-foreground" />
                            <span className="font-medium">{item.title}</span>
                          </NavigationMenuLink>
                        </li>
                      ))}
                    </ul>
                  </div>
                </NavigationMenuContent>
              </NavigationMenuItem>
              <NavigationMenuLink asChild className="px-4">
                <a className="rounded-md p-2 hover:bg-accent" href="#">
                  Pricing
                </a>
              </NavigationMenuLink>
            </NavigationMenuList>
          </NavigationMenu>
        </div>
        <div className="hidden items-center gap-2 md:flex">
          <Button variant="outline">Sign In</Button>
          <Button>Get Started</Button>
        </div>
        <Button
          aria-controls="mobile-menu"
          aria-expanded={open}
          aria-label="Toggle menu"
          className="md:hidden"
          onClick={() => setOpen(!open)}
          size="icon"
          variant="outline"
        >
          <MenuToggleIcon className="size-5" duration={300} open={open} />
        </Button>
      </nav>
      <MobileMenu
        className="flex flex-col justify-between gap-2 overflow-y-auto"
        open={open}
      >
        <NavigationMenu className="max-w-full">
          <div className="flex w-full flex-col gap-y-2">
            <span className="text-sm">Product</span>
            {productLinks.map((link) => (
              <ListItem key={link.title} {...link} />
            ))}
            <span className="text-sm">Company</span>
            {companyLinks.map((link) => (
              <ListItem key={link.title} {...link} />
            ))}
            {companyLinks2.map((link) => (
              <ListItem key={link.title} {...link} />
            ))}
          </div>
        </NavigationMenu>
        <div className="flex flex-col gap-2">
          <Button className="w-full bg-transparent" variant="outline">
            Sign In
          </Button>
          <Button className="w-full">Get Started</Button>
        </div>
      </MobileMenu>
    </header>
  );
}

type MobileMenuProps = React.ComponentProps<"div"> & {
  open: boolean;
};

function MobileMenu({ open, children, className, ...props }: MobileMenuProps) {
  if (!open || typeof window === "undefined") {
    return null;
  }

  return createPortal(
    <div
      className={cn(
        "bg-background/95 backdrop-blur-lg supports-[backdrop-filter]:bg-background/50",
        "fixed top-14 right-0 bottom-0 left-0 z-40 flex flex-col overflow-hidden border-y md:hidden"
      )}
      id="mobile-menu"
    >
      <div
        className={cn(
          "data-[slot=open]:zoom-in-97 ease-out data-[slot=open]:animate-in",
          "size-full p-4",
          className
        )}
        data-slot={open ? "open" : "closed"}
        {...props}
      >
        {children}
      </div>
    </div>,
    document.body
  );
}

function ListItem({
  title,
  description,
  icon: Icon,
  className,
  href,
  ...props
}: React.ComponentProps<typeof NavigationMenuLink> & LinkItem) {
  return (
    <NavigationMenuLink
      className={cn("w-full flex-row gap-x-2", className)}
      {...props}
      asChild
    >
      <a href={href}>
        <div className="flex aspect-square size-12 items-center justify-center rounded-md border bg-background/40 shadow-sm">
          <Icon className="size-5 text-foreground" />
        </div>
        <div className="flex flex-col items-start justify-center">
          <span className="font-medium">{title}</span>
          <span className="text-muted-foreground text-xs">{description}</span>
        </div>
      </a>
    </NavigationMenuLink>
  );
}

const productLinks: LinkItem[] = [
  {
    title: "Website Builder",
    href: "#",
    description: "Create responsive websites with ease",
    icon: GlobeIcon,
  },
  {
    title: "Cloud Platform",
    href: "#",
    description: "Deploy and scale apps in the cloud",
    icon: LayersIcon,
  },
  {
    title: "Team Collaboration",
    href: "#",
    description: "Tools to help your teams work better together",
    icon: UserPlusIcon,
  },
  {
    title: "Analytics",
    href: "#",
    description: "Track and analyze your website traffic",
    icon: BarChart,
  },
  {
    title: "Integrations",
    href: "#",
    description: "Connect your apps and services",
    icon: PlugIcon,
  },
  {
    title: "API",
    href: "#",
    description: "Build custom integrations with our API",
    icon: CodeIcon,
  },
];

const companyLinks: LinkItem[] = [
  {
    title: "About Us",
    href: "#",
    description: "Learn more about our story and team",
    icon: Users,
  },
  {
    title: "Customer Stories",
    href: "#",
    description: "See how we’ve helped our clients succeed",
    icon: Star,
  },
  {
    title: "Partnerships",
    href: "#",
    icon: Handshake,
    description: "Collaborate with us for mutual growth",
  },
];

const companyLinks2: LinkItem[] = [
  {
    title: "Terms of Service",
    href: "#",
    icon: FileText,
  },
  {
    title: "Privacy Policy",
    href: "#",
    icon: Shield,
  },
  {
    title: "Refund Policy",
    href: "#",
    icon: RotateCcw,
  },
  {
    title: "Blog",
    href: "#",
    icon: Leaf,
  },
  {
    title: "Help Center",
    href: "#",
    icon: HelpCircle,
  },
];

```

###### File: `components/logo.tsx`

```tsx
import type React from "react";

export const LogoIcon = (props: React.ComponentProps<"svg">) => (
  <svg fill="currentColor" viewBox="0 0 24 24" {...props}>
    <path
      d="M 15.03125 23.851562 C 14.117188 23.609375 13.417969 23 13 22.101562 C 12.808594 21.691406 12.808594 21.613281 12.808594 18.375 C 12.808594 15.066406 12.808594 15.066406 13.097656 14.484375 C 13.429688 13.8125 13.898438 13.351562 14.585938 13.027344 C 15.074219 12.800781 15.074219 12.800781 18.386719 12.800781 C 21.699219 12.800781 21.699219 12.800781 22.28125 13.089844 C 22.953125 13.421875 23.414062 13.890625 23.738281 14.578125 C 23.964844 15.066406 23.964844 15.066406 23.988281 18.140625 C 24.015625 20.769531 24 21.285156 23.875 21.710938 C 23.5625 22.789062 22.769531 23.582031 21.699219 23.863281 C 20.964844 24.042969 15.746094 24.042969 15.03125 23.851562 Z M 21.679688 22.390625 C 21.863281 22.304688 22.117188 22.085938 22.246094 21.917969 C 22.480469 21.613281 22.480469 21.613281 22.480469 18.375 C 22.480469 15.136719 22.480469 15.136719 22.238281 14.824219 C 21.757812 14.1875 21.792969 14.195312 18.386719 14.195312 C 15.667969 14.195312 15.308594 14.214844 15.066406 14.34375 C 14.691406 14.542969 14.359375 14.960938 14.246094 15.355469 C 14.144531 15.753906 14.132812 20.847656 14.246094 21.328125 C 14.386719 21.9375 14.847656 22.398438 15.441406 22.519531 C 15.625 22.554688 17.027344 22.582031 18.5625 22.574219 C 21.105469 22.554688 21.375 22.539062 21.679688 22.390625 Z M 21.679688 22.390625 "
      opacity="50%"
    />
    <path d="M 2.859375 23.085938 C 2.089844 22.84375 1.324219 22.15625 0.976562 21.398438 C 0.792969 20.996094 0.785156 20.882812 0.785156 17.636719 C 0.785156 14.28125 0.785156 14.28125 1.019531 13.785156 C 1.296875 13.1875 1.855469 12.609375 2.441406 12.324219 C 2.875 12.105469 2.875 12.105469 6.195312 12.078125 C 9.4375 12.054688 9.523438 12.0625 10.03125 12.246094 C 10.71875 12.507812 11.441406 13.21875 11.730469 13.933594 C 11.9375 14.457031 11.9375 14.472656 11.9375 17.636719 C 11.9375 21.136719 11.9375 21.128906 11.371094 21.953125 C 11.03125 22.433594 10.550781 22.800781 9.925781 23.035156 C 9.480469 23.199219 9.261719 23.207031 6.335938 23.199219 C 4.035156 23.199219 3.128906 23.164062 2.859375 23.085938 Z M 2.859375 23.085938 " />
    <path d="M 13.898438 11.695312 C 13.226562 11.4375 12.503906 10.703125 12.25 10.023438 C 12.070312 9.519531 12.058594 9.433594 12.085938 6.195312 C 12.113281 2.929688 12.113281 2.867188 12.3125 2.476562 C 12.636719 1.824219 13.070312 1.386719 13.707031 1.074219 C 14.289062 0.785156 14.289062 0.785156 17.644531 0.785156 C 20.90625 0.785156 21.007812 0.792969 21.410156 0.976562 C 21.984375 1.238281 22.65625 1.890625 22.933594 2.476562 C 23.179688 2.964844 23.179688 2.964844 23.179688 6.316406 C 23.179688 9.667969 23.179688 9.667969 22.890625 10.25 C 22.578125 10.886719 22.140625 11.324219 21.488281 11.644531 C 21.097656 11.84375 21.042969 11.84375 17.734375 11.863281 C 14.492188 11.878906 14.359375 11.878906 13.898438 11.695312 Z M 13.898438 11.695312 " />
    <path d="M 2.214844 11.078125 C 1.324219 10.824219 0.609375 10.207031 0.199219 9.3125 C 0 8.894531 0 8.832031 0 5.574219 C 0 2.265625 0 2.265625 0.234375 1.769531 C 0.53125 1.132812 1.132812 0.535156 1.769531 0.238281 C 2.265625 0.00390625 2.265625 0.00390625 5.578125 0.00390625 C 8.886719 0.00390625 8.886719 0.00390625 9.386719 0.238281 C 10.019531 0.535156 10.621094 1.132812 10.917969 1.769531 C 11.152344 2.265625 11.152344 2.265625 11.152344 5.574219 C 11.152344 8.882812 11.152344 8.882812 10.925781 9.371094 C 10.605469 10.058594 10.144531 10.53125 9.472656 10.859375 C 8.886719 11.148438 8.886719 11.148438 5.75 11.164062 C 3.441406 11.183594 2.507812 11.15625 2.214844 11.078125 Z M 8.898438 9.605469 C 9.238281 9.425781 9.613281 8.988281 9.699219 8.683594 C 9.734375 8.554688 9.757812 7.117188 9.757812 5.488281 C 9.757812 2.179688 9.769531 2.207031 9.132812 1.726562 C 8.820312 1.484375 8.820312 1.484375 5.804688 1.457031 C 4.148438 1.4375 2.65625 1.457031 2.5 1.484375 C 2.34375 1.507812 2.066406 1.683594 1.882812 1.855469 C 1.375 2.335938 1.34375 2.632812 1.421875 5.976562 C 1.480469 8.816406 1.480469 8.816406 1.726562 9.128906 C 2.214844 9.773438 2.316406 9.789062 5.75 9.773438 C 8.285156 9.753906 8.660156 9.738281 8.898438 9.605469 Z M 8.898438 9.605469 " />
  </svg>
);

export const Logo = (props: React.ComponentProps<"svg">) => (
  <svg
    fill="currentColor"
    viewBox="0 0 90 24"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <defs>
      <clipPath id="a">
        <path d="M12.785 12.781h11.172V24H12.785Zm0 0" />
      </clipPath>
      <clipPath id="b">
        <path d="M.008 0h11.148v11.2H.008Zm0 0" />
      </clipPath>
    </defs>
    <g clipPath="url(#a)">
      <path
        d="M15.008 23.855c-.91-.242-1.61-.851-2.028-1.75-.19-.41-.19-.488-.19-3.73 0-3.309 0-3.309.288-3.89a3 3 0 0 1 1.485-1.458c.488-.226.488-.226 3.792-.226 3.31 0 3.31 0 3.887.289.672.332 1.133.8 1.453 1.488.227.488.227.488.25 3.563.028 2.632.012 3.148-.113 3.574-.309 1.078-1.102 1.87-2.168 2.152-.734.18-5.941.18-6.656-.012m6.637-1.46c.18-.086.433-.305.562-.473.234-.305.234-.305.234-3.547 0-3.238 0-3.238-.242-3.55-.476-.637-.441-.63-3.844-.63-2.71 0-3.07.02-3.312.149-.375.199-.707.617-.82 1.011-.098.399-.11 5.497 0 5.977.14.61.601 1.07 1.195 1.191.184.036 1.582.063 3.113.055 2.54-.02 2.809-.035 3.114-.183m0 0"
        opacity="50%"
      />
    </g>
    <path d="M2.86 23.09c-.766-.242-1.532-.93-1.88-1.688-.183-.402-.187-.515-.187-3.765 0-3.356 0-3.356.23-3.852.278-.598.836-1.176 1.422-1.46.43-.22.43-.22 3.746-.247 3.235-.023 3.32-.015 3.829.168.683.262 1.406.973 1.695 1.688.207.523.207.539.207 3.703 0 3.504 0 3.496-.567 4.32-.34.48-.82.848-1.44 1.082-.446.164-.665.172-3.583.164-2.297 0-3.203-.035-3.473-.113M13.879 11.695c-.672-.258-1.395-.992-1.645-1.672-.18-.503-.191-.59-.164-3.832.028-3.265.028-3.328.223-3.718.324-.653.758-1.09 1.395-1.403.578-.289.578-.289 3.93-.289 3.253 0 3.355.008 3.757.192.57.261 1.242.914 1.52 1.5.246.488.246.488.246 3.84 0 3.355 0 3.355-.29 3.937a2.9 2.9 0 0 1-1.398 1.395c-.39.199-.445.199-3.746.218-3.238.016-3.371.016-3.828-.168m0 0" />
    <g clipPath="url(#b)">
      <path d="M2.219 11.078c-.89-.254-1.602-.871-2.012-1.765-.2-.418-.2-.481-.2-3.743 0-3.308 0-3.308.235-3.804A3.34 3.34 0 0 1 1.773.234C2.27 0 2.27 0 5.574 0c3.301 0 3.301 0 3.801.234.633.297 1.23.895 1.527 1.532.235.496.235.496.235 3.804 0 3.313 0 3.313-.227 3.801-.32.688-.777 1.16-1.45 1.488-.585.29-.585.29-3.714.305-2.305.02-3.234-.008-3.527-.086m6.668-1.473c.34-.18.715-.617.8-.921.036-.13.06-1.567.06-3.2 0-3.308.01-3.28-.626-3.761-.312-.243-.312-.243-3.32-.27-1.653-.02-3.145 0-3.297.027-.156.024-.434.2-.617.372-.508.48-.54.777-.461 4.12.058 2.844.058 2.844.304 3.157.489.644.59.66 4.016.644 2.531-.02 2.902-.035 3.14-.168m0 0" />
    </g>
    <path d="m33.688 17.254-1.622-2.125 3.325-2.57a1.6 1.6 0 0 0-.696-.434 3 3 0 0 0-.957-.145q-.626.001-1.148.266a2.75 2.75 0 0 0-.903.707q-.379.445-.59 1.047c-.14.402-.206.84-.206 1.313q0 .655.246 1.257c.164.403.394.758.68 1.063q.432.46 1.019.734a3 3 0 0 0 1.27.278c.84 0 1.57-.243 2.199-.723q.942-.72 1.152-2.008l3.375.367a7.6 7.6 0 0 1-.879 2.387q-.614 1.053-1.504 1.797-.891.75-2.027 1.14a7.5 7.5 0 0 1-2.445.395 6.6 6.6 0 0 1-2.606-.512 6.1 6.1 0 0 1-2.039-1.402 6.3 6.3 0 0 1-1.32-2.11q-.474-1.223-.473-2.663-.001-1.44.473-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.177-.499 2.59-.5 2.195 0 3.777.973c1.059.644 1.855 1.582 2.398 2.804ZM47.973 6.684H46.69q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.254-4.239q1.26-1.296 3.48-1.297h1.23ZM54.723 6.684H53.44q-.601 0-.968.277-.366.277-.368.98v1h2.618v3.25h-2.618v9.493h-3.347V8.864c0-1.962.418-3.372 1.258-4.239q1.255-1.296 3.476-1.297h1.23ZM61.21 17.254l-1.62-2.125 3.324-2.57a1.6 1.6 0 0 0-.695-.434 3 3 0 0 0-.953-.145q-.632.001-1.153.266-.52.26-.902.707-.38.445-.59 1.047-.206.603-.207 1.313 0 .655.246 1.257.252.605.68 1.063.433.46 1.023.734.586.277 1.266.278c.84 0 1.57-.243 2.2-.723q.94-.72 1.151-2.008l3.375.367a7.6 7.6 0 0 1-.878 2.387q-.614 1.053-1.504 1.797-.891.75-2.028 1.14A7.5 7.5 0 0 1 61.5 22a6.6 6.6 0 0 1-2.605-.512 6.1 6.1 0 0 1-2.04-1.402 6.3 6.3 0 0 1-1.32-2.11q-.473-1.223-.472-2.663-.001-1.44.472-2.676a6.2 6.2 0 0 1 1.332-2.125 6.2 6.2 0 0 1 2.043-1.387q1.178-.499 2.59-.5 2.199 0 3.781.973c1.055.644 1.852 1.582 2.395 2.804ZM76.805 12.348h-.758q-1.413 0-2.121.855c-.469.567-.703 1.29-.703 2.16v6.32h-3.559V8.942h3.164v2.41h.05c.317-.855.75-1.5 1.31-1.925q.838-.645 1.964-.645h.653ZM89.992 3.328v12.063c0 .89-.152 1.742-.457 2.543a6.6 6.6 0 0 1-1.285 2.113 6 6 0 0 1-1.988 1.43q-1.161.522-2.551.523a5.9 5.9 0 0 1-2.496-.523 6.1 6.1 0 0 1-1.965-1.43 6.8 6.8 0 0 1-1.293-2.125 7.3 7.3 0 0 1-.473-2.637q0-1.335.461-2.555a6.5 6.5 0 0 1 1.282-2.125 6.3 6.3 0 0 1 1.921-1.44 5.4 5.4 0 0 1 2.407-.54q.527 0 1.007.117.487.122 1.008.25v3.54a5 5 0 0 0-.851-.286 4 4 0 0 0-.953-.105q-1.285 0-2.106.879-.825.876-.824 2.293 0 1.417.809 2.296a2.65 2.65 0 0 0 2.015.875q1.359-.001 2.172-.875.811-.881.813-2.218V3.328Zm0 0" />
  </svg>
);

```

###### File: `components/menu-toggle-icon.tsx`

```tsx
"use client";
import type React from "react";
import { cn } from "@/lib/utils";

type MenuToggleProps = React.ComponentProps<"svg"> & {
  open: boolean;
  duration?: number;
};

export function MenuToggleIcon({
  open,
  className,
  fill = "none",
  stroke = "currentColor",
  strokeWidth = 2.5,
  strokeLinecap = "round",
  strokeLinejoin = "round",
  duration = 500,
  ...props
}: MenuToggleProps) {
  return (
    <svg
      className={cn(
        "transition-transform ease-in-out",
        open && "-rotate-45",
        className
      )}
      fill={fill}
      stroke={stroke}
      strokeLinecap={strokeLinecap}
      strokeLinejoin={strokeLinejoin}
      strokeWidth={strokeWidth}
      style={{
        transitionDuration: `${duration}ms`,
      }}
      viewBox="0 0 32 32"
      {...props}
    >
      <path
        className={cn(
          "transition-all ease-in-out",
          open
            ? "[stroke-dasharray:20_300] [stroke-dashoffset:-32.42px]"
            : "[stroke-dasharray:12_63]"
        )}
        d="M27 10 13 10C10.8 10 9 8.2 9 6 9 3.5 10.8 2 13 2 15.2 2 17 3.8 17 6L17 26C17 28.2 18.8 30 21 30 23.2 30 25 28.2 25 26 25 23.8 23.2 22 21 22L7 22"
        style={{
          transitionDuration: `${duration}ms`,
        }}
      />
      <path d="M7 16 27 16" />
    </svg>
  );
}

```

###### File: `hooks/use-scroll.ts`

```ts
"use client";
import React from "react";

export function useScroll(threshold: number) {
  const [scrolled, setScrolled] = React.useState(false);

  const onScroll = React.useCallback(() => {
    setScrolled(window.scrollY > threshold);
  }, [threshold]);

  React.useEffect(() => {
    window.addEventListener("scroll", onScroll);
    return () => window.removeEventListener("scroll", onScroll);
  }, [onScroll]);

  // also check on first load
  React.useEffect(() => {
    onScroll();
  }, [onScroll]);

  return scrolled;
}

```

#### Svelte Implementation

- **Registry Key**: `header-three`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/header-three.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `navigation-menu`
  - `local:portal`

##### Component Source Code

###### File: `src/lib/components/efferd/header/header-three/header-three.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { createScroll } from "$lib/hooks/use-scroll.svelte";
	import Logo from "$lib/svgs/logo.svelte";
	import { cn } from "$lib/utils";
	import DesktopNav from "./desktop-nav.svelte";
	import MobileNav from "./mobile-nav.svelte";

	let scroll = createScroll(50);
</script>

<header
	class={cn(
		"sticky top-0 z-50 w-full border-b border-transparent",
		scroll.scrolled &&
			"border-border bg-background/95 backdrop-blur-sm supports-backdrop-filter:bg-background/50"
	)}
>
	<nav class="mx-auto flex h-14 w-full max-w-5xl items-center justify-between px-4">
		<div class="flex items-center gap-5">
			<a class="rounded-lg px-3 py-2.5 hover:bg-muted dark:hover:bg-muted/50" href="/">
				<Logo class="h-4" />
			</a>
			<DesktopNav />
		</div>
		<div class="hidden items-center gap-2 md:flex">
			<Button variant="outline">Sign In</Button>
			<Button>Get Started</Button>
		</div>
		<MobileNav />
	</nav>
</header>

```

###### File: `src/lib/components/efferd/header/header-three/desktop-nav.svelte`

```svelte
<script lang="ts">
	import * as NavigationMenu from "$lib/components/ui/navigation-menu/index";
	import type { HTMLAttributes } from "svelte/elements";
	import LinkItem from "./link-item.svelte";
	import { companyLinks, companyLinks2, productLinks } from "./nav-links";

	type ListItemProps = HTMLAttributes<HTMLAnchorElement> & {
		title: string;
		href: string;
		content: string;
	};
</script>

<NavigationMenu.Root class="hidden md:flex">
	<NavigationMenu.List>
		<NavigationMenu.Item>
			<NavigationMenu.Trigger class="bg-transparent">Product</NavigationMenu.Trigger>
			<NavigationMenu.Content class="NavigationMenuContent p-0">
				<div class="bg-muted/50 p-1 pr-1.5 dark:bg-background">
					<div
						class="grid w-lg grid-cols-2 gap-2 rounded-lg border bg-popover p-2 shadow"
					>
						{#each productLinks as item, i}
							<NavigationMenu.Link>
								<LinkItem {...item} />
							</NavigationMenu.Link>
						{/each}
					</div>
					<div class="p-2">
						<p class="text-sm text-muted-foreground">
							Interested?{" "}
							<a class="font-medium text-foreground hover:underline" href="/">
								Schedule a demo
							</a>
						</p>
					</div>
				</div>
			</NavigationMenu.Content>
		</NavigationMenu.Item>
		<NavigationMenu.Item>
			<NavigationMenu.Trigger class="bg-transparent">Company</NavigationMenu.Trigger>
			<NavigationMenu.Content class="p-0">
				<div class="bg-muted/50 p-1 pr-1.5 pb-1.5 dark:bg-background">
					<div class="grid w-lg grid-cols-2 gap-2">
						<div class="space-y-2 rounded-lg border bg-popover p-2 shadow">
							{#each companyLinks as item, i}
								<NavigationMenu.Link>
									<LinkItem {...item} />
								</NavigationMenu.Link>
							{/each}
						</div>
						<div class="space-y-2 p-3">
							{#each companyLinks2 as item, i}
								{@const IconComp = item.icon}
								<NavigationMenu.Link>
									<div class="flex items-center gap-2">
										<IconComp />
										{item.label}
									</div>
								</NavigationMenu.Link>
							{/each}
						</div>
					</div>
				</div>
			</NavigationMenu.Content>
		</NavigationMenu.Item>
		<NavigationMenu.Item>
			<NavigationMenu.Link class="rounded-md p-2 px-4 hover:bg-accent">
				{#snippet child({ props })}
					<a href="/docs" {...props}>Docs</a>
				{/snippet}
			</NavigationMenu.Link>
		</NavigationMenu.Item>
	</NavigationMenu.List>
</NavigationMenu.Root>

```

###### File: `src/lib/components/efferd/header/header-three/mobile-nav.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { Portal, PortalBackdrop } from "$lib/components/ui/portal";
	import { cn } from "$lib/utils";
	import { MenuIcon, XIcon } from "@lucide/svelte";
	import LinkItem from "./link-item.svelte";
	import { productLinks, companyLinks, companyLinks2 } from "./nav-links";

	let open = $state(false);
</script>

<div class="md:hidden">
	<Button
		aria-controls="mobile-menu"
		aria-expanded={open}
		aria-label="Toggle menu"
		class="md:hidden"
		onclick={() => (open = !open)}
		size="icon"
		variant="outline"
	>
		<div class={cn("transition-all", open ? "scale-100 opacity-100" : "scale-0 opacity-0")}>
			<XIcon />
		</div>
		<div
			class={cn(
				"absolute transition-all",
				open ? "scale-0 opacity-0" : "scale-100 opacity-100"
			)}
		>
			<MenuIcon />
		</div>
	</Button>
	{#if open}
		<Portal class="top-14">
			<PortalBackdrop />
			<div
				class={cn(
					"size-full overflow-y-auto p-4",
					"ease-out data-[slot=open]:animate-in data-[slot=open]:zoom-in-97"
				)}
				data-slot={open ? "open" : "closed"}
			>
				<div class="flex w-full flex-col gap-y-2">
					<span class="text-sm">Product</span>
					{#each productLinks as link}
						<LinkItem
							class="rounded-lg p-2 active:bg-muted dark:active:bg-muted/50"
							{...link}
						/>
					{/each}
					<span class="text-sm">Company</span>
					{#each companyLinks as link}
						<LinkItem
							class="rounded-lg p-2 active:bg-muted dark:active:bg-muted/50"
							{...link}
						/>
					{/each}
					{#each companyLinks2 as link}
						<LinkItem
							class="rounded-lg p-2 active:bg-muted dark:active:bg-muted/50"
							{...link}
						/>
					{/each}
				</div>
				<div class="mt-5 flex flex-col gap-2">
					<Button class="w-full" variant="outline">Sign In</Button>
					<Button class="w-full">Get Started</Button>
				</div>
			</div>
		</Portal>
	{/if}
</div>

```

###### File: `src/lib/components/efferd/header/header-three/link-item.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";
	import type { LinkItemType } from "./types";
	import type { Component } from "svelte";
	import type { Icon } from "@lucide/svelte";

	type Props = LinkItemType & HTMLAttributes<HTMLAnchorElement>;
	let { label, description, icon, href, class: className, ...props }: Props = $props();

	// i have added Svelte Component, Lucide Icon so you can have custom-icon.svelte and lucide icon both
	let IconComponent: Component | typeof Icon = $derived(icon);
</script>

<a class={cn("flex items-center gap-x-2", className)} {href} {...props}>
	<div
		class={cn(
			"flex aspect-square size-9 items-center justify-center rounded-md border bg-card text-sm shadow-sm",
			"[&_svg:not([class*='size-'])]:size-4 [&_svg:not([class*='size-'])]:text-foreground"
		)}
	>
		<IconComponent />
	</div>
	<div class="flex flex-col items-start justify-center">
		<span class="font-medium">{label}</span>
		<span class="line-clamp-2 text-xs text-muted-foreground">
			{description}
		</span>
	</div>
</a>

```

###### File: `src/lib/components/efferd/header/header-three/nav-links.ts`

```ts
import {
	FileTextIcon,
	GlobeIcon,
	LayersIcon,
	PlugIcon,
	ShieldIcon,
	UserPlusIcon,
	UsersIcon,
	StarIcon,
	HandshakeIcon,
	CodeIcon,
	RotateCcwIcon,
	LeafIcon,
	HandHelpingIcon,
	ChartArea
} from "@lucide/svelte";

import type { LinkItemType } from "./types";

let productLinks: LinkItemType[] = [
	{
		label: "Website Builder",
		href: "/",
		description: "Create responsive websites with ease",
		icon: GlobeIcon
	},
	{
		label: "Cloud Platform",
		href: "/",
		description: "Deploy and scale apps in the cloud",
		icon: LayersIcon
	},
	{
		label: "Team Collaboration",
		href: "/",
		description: "Tools to help your teams work better together",
		icon: UserPlusIcon
	},
	{
		label: "Analytics",
		href: "/",
		description: "Track and analyze your website traffic",
		icon: ChartArea
	},
	{
		label: "Integrations",
		href: "/",
		description: "Connect your apps and services",
		icon: PlugIcon
	},
	{
		label: "API",
		href: "/",
		description: "Build custom integrations with our API",
		icon: CodeIcon
	}
];

let companyLinks: LinkItemType[] = [
	{
		label: "About Us",
		href: "/",
		description: "Learn more about our story and team",
		icon: UsersIcon
	},
	{
		label: "Customer Stories",
		href: "/",
		description: "See how we've helped our clients succeed",
		icon: StarIcon
	},
	{
		label: "Partnerships",
		href: "/",
		icon: HandshakeIcon,
		description: "Collaborate with us for mutual growth"
	}
];

let companyLinks2: LinkItemType[] = [
	{
		label: "Terms of Service",
		href: "/",
		icon: FileTextIcon
	},
	{
		label: "Privacy Policy",
		href: "/",
		icon: ShieldIcon
	},
	{
		label: "Refund Policy",
		href: "/",
		icon: RotateCcwIcon
	},
	{
		label: "Blog",
		href: "/",
		icon: LeafIcon
	},
	{
		label: "Help Center",
		href: "/",
		icon: HandHelpingIcon
	}
];

export { productLinks, companyLinks, companyLinks2 };

```

###### File: `src/lib/components/efferd/header/header-three/types.ts`

```ts
import type { Icon } from "@lucide/svelte";
import type { Component } from "svelte";

export type LinkItemType = {
	label: string;
	description?: string;
	icon: typeof Icon | Component;
	href: string;
};

```

###### File: `src/lib/hooks/use-scroll.svelte.ts`

```ts
class ScrollState {
	#scrolled = $state(false);

	readonly #downThreshold: number;
	readonly #upThreshold: number;

	constructor(downThreshold: number, upThreshold?: number) {
		this.#downThreshold = downThreshold;
		this.#upThreshold = upThreshold ?? downThreshold / 2;

		$effect(() => {
			const handleScroll = () => {
				const y = window.scrollY;
				// Hysteresis Logic: Only update scrolled state when crossing thresholds in the appropriate direction
				this.#scrolled = this.#scrolled
					? y > this.#upThreshold // currently scrolled → only reset below upThreshold
					: y > this.#downThreshold; // currently not scrolled → only set above downThreshold
			};

			window.addEventListener("scroll", handleScroll, { passive: true });
			handleScroll();

			return () => window.removeEventListener("scroll", handleScroll);
		});
	}

	get scrolled(): boolean {
		return this.#scrolled;
	}
}

export function createScroll(downThreshold: number, upThreshold?: number): ScrollState {
	return new ScrollState(downThreshold, upThreshold);
}

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

---

### Header 4 <a name="header-4"></a>

A premium header 4 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `header-four`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/header-four.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:portal`

##### Component Source Code

###### File: `src/lib/components/efferd/header/header-four/header-four.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { createScroll } from "$lib/hooks/use-scroll.svelte";
	import Logo from "$lib/svgs/logo.svelte";
	import { cn } from "$lib/utils";
	import MobileNav from "./mobile-nav.svelte";
	import { navLinks } from "./nav-links";
	let scroll = createScroll(10);
</script>

<header
	class={cn(
		"sticky top-2 z-50 mx-auto flex h-11 w-[92svw] max-w-4xl items-center justify-between rounded-lg border bg-background/95 px-1 shadow-sm backdrop-blur-sm transition-[max-width] duration-300 ease-out supports-backdrop-filter:bg-background/50",
		scroll.scrolled &&
			"border-border bg-background/95 backdrop-blur-sm supports-backdrop-filter:bg-background/50 md:top-2 md:max-w-3xl md:shadow"
	)}
>
	<nav
		class={cn(
			"flex h-11 w-full items-center justify-between md:h-11  md:transition-all md:ease-out"
		)}
	>
		<a
			class="rounded-md border border-border p-2 hover:bg-muted dark:hover:bg-muted/50"
			href="/"
		>
			<Logo class="h-4" />
		</a>
		<div class="hidden items-center gap-2 md:flex">
			<div>
				{#each navLinks as { label, href }}
					<Button size="sm" variant="ghost" {href}>
						{label}
					</Button>
				{/each}
			</div>
		</div>
		<div>
			<Button size="sm" variant="ghost">Login</Button>
			<Button size="sm">Sign Up</Button>
		</div>
		<MobileNav />
	</nav>
</header>

```

###### File: `src/lib/components/efferd/header/header-four/mobile-nav.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { Portal, PortalBackdrop } from "$lib/components/ui/portal";
	import { cn } from "$lib/utils";
	import { MenuIcon, XIcon } from "@lucide/svelte";
	import { navLinks } from "./nav-links";
	let open = $state(false);
</script>

<div class="md:hidden">
	<Button
		aria-controls="mobile-menu"
		aria-expanded={open}
		aria-label="Toggle menu"
		class="size-8 md:hidden"
		size="icon"
		variant="secondary"
		onclick={() => (open = !open)}
	>
		{#if open}
			<XIcon class="size-4.5" />
		{:else}
			<MenuIcon class="size-4.5" />
		{/if}
	</Button>
	{#if open}
		<Portal class="top-14">
			<PortalBackdrop />
			<div
				class={cn(
					"ease-out data-[slot=open]:animate-in data-[slot=open]:zoom-in-97",
					"size-full px-6 py-4"
				)}
				data-slot={open ? "open" : "closed"}
			>
				<ul class="grid gap-y-4">
					{#each navLinks as link, i}
						<li>
							<a href={link.href} class="text-xl font-medium">
								{link.label}
							</a>
						</li>
					{/each}
				</ul>
				<div class="mt-4 grid grid-cols-2 gap-2">
					<Button variant="outline">Sign In</Button>
					<Button>Get Started</Button>
				</div>
			</div>
		</Portal>
	{/if}
</div>

```

###### File: `src/lib/components/efferd/header/header-four/nav-links.ts`

```ts
let navLinks = [
	{
		label: "Home",
		href: "/"
	},
	{
		label: "Features",
		href: "/"
	},
	{
		label: "Pricing",
		href: "/"
	},
	{
		label: "About",
		href: "/"
	}
];

export { navLinks };

```

###### File: `src/lib/hooks/use-scroll.svelte.ts`

```ts
class ScrollState {
	#scrolled = $state(false);

	readonly #downThreshold: number;
	readonly #upThreshold: number;

	constructor(downThreshold: number, upThreshold?: number) {
		this.#downThreshold = downThreshold;
		this.#upThreshold = upThreshold ?? downThreshold / 2;

		$effect(() => {
			const handleScroll = () => {
				const y = window.scrollY;
				// Hysteresis Logic: Only update scrolled state when crossing thresholds in the appropriate direction
				this.#scrolled = this.#scrolled
					? y > this.#upThreshold // currently scrolled → only reset below upThreshold
					: y > this.#downThreshold; // currently not scrolled → only set above downThreshold
			};

			window.addEventListener("scroll", handleScroll, { passive: true });
			handleScroll();

			return () => window.removeEventListener("scroll", handleScroll);
		});
	}

	get scrolled(): boolean {
		return this.#scrolled;
	}
}

export function createScroll(downThreshold: number, upThreshold?: number): ScrollState {
	return new ScrollState(downThreshold, upThreshold);
}

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

---

### Header 5 <a name="header-5"></a>

A premium header 5 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `header-five`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/header-five.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `navigation-menu`
  - `local:portal`

##### Component Source Code

###### File: `src/lib/components/efferd/header/header-five/header-five.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { createScroll } from "$lib/hooks/use-scroll.svelte";
	import Logo from "$lib/svgs/logo.svelte";
	import { cn } from "$lib/utils";
	import DesktopNav from "./desktop-nav.svelte";
	import MobileNav from "./mobile-nav.svelte";
	import { navLinks } from "./nav-links";
	let scroll = createScroll(10);
</script>

<header
	class={cn(
		"sticky top-0 z-50 mx-auto w-full max-w-5xl border-b border-border  shadow-sm backdrop-blur-sm transition-[max-width] duration-300 ease-out supports-backdrop-filter:bg-background/50 md:transition-all md:ease-out",
		scroll.scrolled &&
			"rounded-full border border-border bg-background/95 backdrop-blur-sm supports-backdrop-filter:bg-background/50 md:top-2 md:max-w-4xl md:shadow"
	)}
>
	<nav
		class={cn(
			"flex h-14 w-full items-center justify-between px-4 md:h-14 md:transition-all md:ease-out",
			scroll.scrolled && "md:h-12 md:px-2"
		)}
	>
		<div class="flex items-center gap-2">
			<a class="rounded-md p-2 hover:bg-muted dark:hover:bg-muted/50" href="/">
				<Logo class="h-4" />
			</a>
			<div aria-hidden="true" class="h-6 shrink-0 bg-border md:w-px"></div>
			<DesktopNav />
		</div>
		<div>
			<Button size="sm" class="rounded-full" variant="outline">Sign In</Button>
			<Button size="sm" class="rounded-full">Get Started</Button>
		</div>
		<MobileNav />
	</nav>
</header>

```

###### File: `src/lib/components/efferd/header/header-five/desktop-nav.svelte`

```svelte
<script lang="ts">
	import * as NavigationMenu from "$lib/components/ui/navigation-menu/index";
	import LinkItem from "./link-item.svelte";
	import { companyLinks, companyLinks2, productLinks } from "./nav-links";
</script>

<NavigationMenu.Root class="hidden md:flex">
	<NavigationMenu.List>
		<NavigationMenu.Item>
			<NavigationMenu.Trigger class="bg-transparent">Product</NavigationMenu.Trigger>
			<NavigationMenu.Content class="p-0!">
				<div class="m-0 bg-muted/50 p-1 pr-1.5 dark:bg-background">
					<div
						class="grid w-64 grid-cols-1 gap-2 rounded-lg border bg-popover p-2 shadow"
					>
						{#each productLinks as item, i}
							<NavigationMenu.Link>
								<LinkItem {...item} />
							</NavigationMenu.Link>
						{/each}
					</div>
					<div class="p-2">
						<p class="text-sm text-muted-foreground">
							Interested?{" "}
							<a class="font-medium text-foreground hover:underline" href="/">
								Schedule a demo
							</a>
						</p>
					</div>
				</div>
			</NavigationMenu.Content>
		</NavigationMenu.Item>
		<NavigationMenu.Item>
			<NavigationMenu.Trigger class="bg-transparent">Company</NavigationMenu.Trigger>
			<NavigationMenu.Content class="p-0">
				<div class="bg-muted/50 p-1 pr-1.5 pb-1.5 dark:bg-background">
					<div class="grid w-sm grid-cols-2 gap-2">
						<div class="space-y-2 rounded-lg border bg-popover p-2 shadow">
							{#each companyLinks as item, i}
								<NavigationMenu.Link>
									<LinkItem {...item} />
								</NavigationMenu.Link>
							{/each}
						</div>
						<div class="space-y-2 p-3">
							{#each companyLinks2 as item, i}
								{@const IconComp = item.icon}
								<NavigationMenu.Link>
									<div class="flex items-center gap-2">
										<IconComp />
										{item.label}
									</div>
								</NavigationMenu.Link>
							{/each}
						</div>
					</div>
				</div>
			</NavigationMenu.Content>
		</NavigationMenu.Item>
		<NavigationMenu.Item>
			<NavigationMenu.Link class="rounded-md p-2 px-4 hover:bg-accent">
				{#snippet child({ props })}
					<a href="/docs" {...props}>Docs</a>
				{/snippet}
			</NavigationMenu.Link>
		</NavigationMenu.Item>
	</NavigationMenu.List>
</NavigationMenu.Root>

```

###### File: `src/lib/components/efferd/header/header-five/mobile-nav.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { Portal, PortalBackdrop } from "$lib/components/ui/portal";
	import { cn } from "$lib/utils";
	import { MenuIcon, XIcon } from "@lucide/svelte";
	import { navLinks } from "./nav-links";
	let open = $state(false);
</script>

<div class="md:hidden">
	<Button
		aria-controls="mobile-menu"
		aria-expanded={open}
		aria-label="Toggle menu"
		class="md:hidden"
		size="icon"
		variant="outline"
		onclick={() => (open = !open)}
	>
		{#if open}
			<XIcon class="size-4.5" />
		{:else}
			<MenuIcon class="size-4.5" />
		{/if}
	</Button>
	{#if open}
		<Portal class="top-14">
			<PortalBackdrop />
			<div
				class={cn(
					"ease-out data-[slot=open]:animate-in data-[slot=open]:zoom-in-97",
					"size-full p-4"
				)}
				data-slot={open ? "open" : "closed"}
			>
				<div class="grid gap-y-2">
					{#each navLinks as link, i}
						<Button class="justify-start" variant="ghost" href={link.href}>
							{link.label}
						</Button>
					{/each}
				</div>
				<div class="mt-12 flex flex-col gap-2">
					<Button class="w-full" variant="outline">Sign In</Button>
					<Button class="w-full">Get Started</Button>
				</div>
			</div>
		</Portal>
	{/if}
</div>

```

###### File: `src/lib/components/efferd/header/header-five/link-item.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";
	import type { LinkItemType } from "./types";
	import type { Component } from "svelte";
	import type { Icon } from "@lucide/svelte";

	type Props = LinkItemType & HTMLAttributes<HTMLAnchorElement>;
	let { label, description, icon, href, class: className, ...props }: Props = $props();

	// i have added Svelte Component, Lucide Icon so you can have custom-icon.svelte and lucide icon both
	let IconComponent: Component | typeof Icon = $derived(icon);
</script>

<a class={cn("flex items-center gap-x-2", className)} {href} {...props}>
	<div
		class={cn(
			"flex aspect-square size-9 items-center justify-center rounded-lg border bg-card shadow-xs outline outline-offset-2 outline-border/60 [&_svg]:size-4 [&_svg]:text-foreground"
		)}
	>
		<IconComponent />
	</div>
	<div class="flex flex-col items-start justify-center">
		<span class="text-xs font-medium">{label}</span>
		<span class="line-clamp-1 text-[10px] text-muted-foreground">
			{description}
		</span>
	</div>
</a>

```

###### File: `src/lib/components/efferd/header/header-five/nav-links.ts`

```ts
import {
	FileTextIcon,
	GlobeIcon,
	LayersIcon,
	PlugIcon,
	ShieldIcon,
	UserPlusIcon,
	UsersIcon,
	StarIcon,
	HandshakeIcon,
	CodeIcon,
	RotateCcwIcon,
	LeafIcon,
	HandHelpingIcon,
	ChartArea
} from "@lucide/svelte";

import type { LinkItemType } from "./types";

let productLinks: LinkItemType[] = [
	{
		label: "Website Builder",
		href: "/",
		description: "Create responsive websites with ease",
		icon: GlobeIcon
	},
	{
		label: "Cloud Platform",
		href: "/",
		description: "Deploy and scale apps in the cloud",
		icon: LayersIcon
	},
	{
		label: "Team Collaboration",
		href: "/",
		description: "Tools to help your teams work better together",
		icon: UserPlusIcon
	},
	{
		label: "Analytics",
		href: "/",
		description: "Track and analyze your website traffic",
		icon: ChartArea
	}
];

let companyLinks: LinkItemType[] = [
	{
		label: "About Us",
		href: "/",
		description: "Learn more about our story and team",
		icon: UsersIcon
	},
	{
		label: "Customer Stories",
		href: "/",
		description: "See how we've helped our clients succeed",
		icon: StarIcon
	},
	{
		label: "Partnerships",
		href: "/",
		icon: HandshakeIcon,
		description: "Collaborate with us for mutual growth"
	}
];

let companyLinks2: LinkItemType[] = [
	{
		label: "Terms of Service",
		href: "/",
		icon: FileTextIcon
	},
	{
		label: "Privacy Policy",
		href: "/",
		icon: ShieldIcon
	},
	{
		label: "Refund Policy",
		href: "/",
		icon: RotateCcwIcon
	}
];

export { productLinks, companyLinks, companyLinks2 };

let navLinks = [
	{
		label: "Features",
		href: "/"
	},
	{
		label: "Pricing",
		href: "/"
	},
	{
		label: "About",
		href: "/"
	}
];

export { navLinks };

```

###### File: `src/lib/components/efferd/header/header-five/types.ts`

```ts
import type { Icon } from "@lucide/svelte";
import type { Component } from "svelte";

export type LinkItemType = {
	label: string;
	description?: string;
	icon: typeof Icon | Component;
	href: string;
};

```

###### File: `src/lib/hooks/use-scroll.svelte.ts`

```ts
class ScrollState {
	#scrolled = $state(false);

	readonly #downThreshold: number;
	readonly #upThreshold: number;

	constructor(downThreshold: number, upThreshold?: number) {
		this.#downThreshold = downThreshold;
		this.#upThreshold = upThreshold ?? downThreshold / 2;

		$effect(() => {
			const handleScroll = () => {
				const y = window.scrollY;
				// Hysteresis Logic: Only update scrolled state when crossing thresholds in the appropriate direction
				this.#scrolled = this.#scrolled
					? y > this.#upThreshold // currently scrolled → only reset below upThreshold
					: y > this.#downThreshold; // currently not scrolled → only set above downThreshold
			};

			window.addEventListener("scroll", handleScroll, { passive: true });
			handleScroll();

			return () => window.removeEventListener("scroll", handleScroll);
		});
	}

	get scrolled(): boolean {
		return this.#scrolled;
	}
}

export function createScroll(downThreshold: number, upThreshold?: number): ScrollState {
	return new ScrollState(downThreshold, upThreshold);
}

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

---

## Hero Blocks <a name="hero"></a>

### Hero 1 <a name="hero-1"></a>

A premium hero 1 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `hero-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/hero-one.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:logo-cloud-three`
  - `local:portal`

##### Component Source Code

###### File: `src/lib/components/efferd/hero/hero-one/preview.svelte`

```svelte
<script lang="ts">
	import HeaderOne from "$lib/components/efferd/header/header-one/header-one.svelte";
	import Hero from "./hero.svelte";
	import LogoSection from "./logo-section.svelte";
</script>

<div class="flex min-h-screen flex-col">
	<HeaderOne />
	<main class="grow">
		<Hero />
		<LogoSection />
	</main>
</div>

```

###### File: `src/lib/components/efferd/hero/hero-one/hero.svelte`

```svelte
<script lang="ts">
	import { ArrowRight, PhoneCall, Rocket } from "@lucide/svelte";
	import { Button } from "$lib/components/ui/button";
</script>

<section class="relative -top-14 mx-auto w-full max-w-5xl px-4">
	<div
		aria-hidden="true"
		class="absolute inset-0 isolate hidden overflow-hidden contain-strict lg:block"
	>
		<div
			class="absolute inset-0 top-0 isolate -z-20 bg-[radial-gradient(35%_80%_at_49%_0%,color-mix(in_oklab,var(--foreground)_8%,transparent),transparent)] contain-strict"
		></div>
	</div>

	<div
		aria-hidden="true"
		class="absolute inset-0 mx-auto hidden min-h-screen w-full max-w-5xl lg:block"
	>
		<div
			class="absolute inset-y-0 left-0 z-10 h-full w-px bg-foreground/15 [mask-image:linear-gradient(to_bottom,transparent_0%,black_20%,black_80%,transparent_100%)]"
		></div>
		<div
			class="absolute inset-y-0 right-0 z-10 h-full w-px bg-foreground/15 [mask-image:linear-gradient(to_bottom,transparent_0%,black_20%,black_80%,transparent_100%)]"
		></div>
	</div>

	<div class="relative flex flex-col items-center justify-center gap-5 pt-46 pb-10">
		<div aria-hidden="true" class="absolute inset-0 top-14 -z-10 size-full overflow-hidden">
			<div
				class="absolute inset-y-0 left-4 w-px bg-linear-to-b from-transparent via-border to-border md:left-4"
			></div>
			<div
				class="absolute inset-y-0 right-4 w-px bg-linear-to-b from-transparent via-border to-border md:right-4"
			></div>
			<div
				class="absolute inset-y-0 left-8 w-px bg-linear-to-b from-transparent via-border/50 to-border/50 md:left-12"
			></div>
			<div
				class="absolute inset-y-0 right-8 w-px bg-linear-to-b from-transparent via-border/50 to-border/50 md:right-12"
			></div>
		</div>

		<a
			class="group mx-auto flex w-fit animate-in items-center gap-3 rounded-full border bg-card px-3 py-1 shadow transition-all delay-500 duration-500 ease-out fill-mode-backwards slide-in-from-bottom-10 fade-in"
			href="/"
		>
			<Rocket class="size-3 text-muted-foreground" />
			<span class="text-xs">shipped new features!</span>
			<span class="block h-5 border-l"></span>
			<ArrowRight class="size-3 duration-150 ease-out group-hover:translate-x-1" />
		</a>

		<h1
			class="animate-in text-center text-4xl tracking-tight text-balance delay-100 duration-500 ease-out fill-mode-backwards slide-in-from-bottom-10 [text-shadow:0_0_50px_color-mix(in_oklab,var(--foreground)_20%,transparent)] fade-in md:text-5xl lg:text-6xl"
		>
			Building Teams Help
			<br />
			You Scale and Lead
		</h1>

		<p
			class="mx-auto max-w-md animate-in text-center text-base tracking-wider text-foreground/80 delay-200 duration-500 ease-out fill-mode-backwards slide-in-from-bottom-10 fade-in sm:text-lg md:text-xl"
		>
			Conecting you with world-class talent
			<br />
			to scale, innovate and lead
		</p>

		<div
			class="flex animate-in flex-row flex-wrap items-center justify-center gap-3 pt-2 delay-300 duration-500 ease-out fill-mode-backwards slide-in-from-bottom-10 fade-in"
		>
			<Button class="rounded-full" href="/" size="lg" variant="secondary">
				<PhoneCall class="size-4" />
				Book a Call
			</Button>
			<Button class="rounded-full" href="/" size="lg">
				Get started
				<ArrowRight class="size-4" />
			</Button>
		</div>
	</div>
</section>

```

###### File: `src/lib/components/efferd/hero/hero-one/logo-section.svelte`

```svelte
<script lang="ts">
	import LogoCloudThree from "$lib/components/efferd/logo-cloud/three/logo-cloud-three.svelte";
</script>

<section class="relative space-y-4 border-t pt-6 pb-10">
	<h2 class="text-center text-lg font-medium tracking-tight text-muted-foreground md:text-xl">
		Trusted by <span class="text-foreground">experts</span>
	</h2>
	<div class="relative z-10 mx-auto max-w-4xl">
		<LogoCloudThree />
	</div>
</section>

```

###### File: `src/lib/components/efferd/header/header-one/header-one.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { createScroll } from "$lib/hooks/use-scroll.svelte";
	import Logo from "$lib/svgs/logo.svelte";
	import { cn } from "$lib/utils";
	import MobileNav from "./mobile-nav.svelte";
	import { navLinks } from "./nav-links";

	let scroll = createScroll(10);
</script>

<header
	class={cn(
		"sticky top-0 z-50 w-full border-b border-transparent",
		scroll.scrolled &&
			"border-border bg-background/95 backdrop-blur-sm supports-backdrop-filter:bg-background/50"
	)}
>
	<nav class="mx-auto flex h-14 w-full max-w-5xl items-center justify-between px-4">
		<a class="w-fit rounded-md p-2 hover:bg-muted dark:hover:bg-muted/50" href="/">
			<Logo class="h-4" />
		</a>
		<div class="hidden items-center gap-2 md:flex">
			{#each navLinks as link}
				<Button href={link.href} size="sm" variant="ghost">
					{link.label}
				</Button>
			{/each}
			<Button size="sm" variant="outline">Sign In</Button>
			<Button size="sm">Get Started</Button>
		</div>
		<MobileNav />
	</nav>
</header>

```

###### File: `src/lib/components/efferd/header/header-one/mobile-nav.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { Portal, PortalBackdrop } from "$lib/components/ui/portal";
	import { cn } from "$lib/utils";
	import { MenuIcon, XIcon } from "@lucide/svelte";
	import { navLinks } from "./nav-links";
	import { fade } from "svelte/transition";

	let open = $state(false);
</script>

<div class="md:hidden">
	<Button
		aria-controls="mobile-menu"
		aria-expanded={open}
		aria-label="Toggle menu"
		class="md:hidden"
		onclick={() => (open = !open)}
		size="icon"
		variant="outline"
	>
		{#if open}
			<XIcon class="size-4.5" />
		{:else}
			<MenuIcon class="size-4.5" />
		{/if}
	</Button>
	{#if open}
		<Portal class="top-14">
			<PortalBackdrop />
			<div
				class={cn(
					"ease-out data-[slot=open]:animate-in data-[slot=open]:zoom-in-97",
					"size-full p-4"
				)}
				out:fade|global={{ duration: 150 }}
				data-slot={open ? "open" : "closed"}
			>
				<div class="grid gap-y-2">
					{#each navLinks as link}
						<Button class="justify-start" variant="ghost" href={link.href}>
							{link.label}
						</Button>
					{/each}
				</div>
				<div class="mt-12 flex flex-col gap-2">
					<Button class="w-full" variant="outline">Sign In</Button>
					<Button class="w-full">Get Started</Button>
				</div>
			</div>
		</Portal>
	{/if}
</div>

```

###### File: `src/lib/components/efferd/header/header-one/nav-links.ts`

```ts
export let navLinks = [
	{
		label: "Features",
		href: "/"
	},
	{
		label: "Pricing",
		href: "/"
	},
	{
		label: "About",
		href: "/"
	}
];

```

###### File: `src/lib/hooks/use-scroll.svelte.ts`

```ts
class ScrollState {
	#scrolled = $state(false);

	readonly #downThreshold: number;
	readonly #upThreshold: number;

	constructor(downThreshold: number, upThreshold?: number) {
		this.#downThreshold = downThreshold;
		this.#upThreshold = upThreshold ?? downThreshold / 2;

		$effect(() => {
			const handleScroll = () => {
				const y = window.scrollY;
				// Hysteresis Logic: Only update scrolled state when crossing thresholds in the appropriate direction
				this.#scrolled = this.#scrolled
					? y > this.#upThreshold // currently scrolled → only reset below upThreshold
					: y > this.#downThreshold; // currently not scrolled → only set above downThreshold
			};

			window.addEventListener("scroll", handleScroll, { passive: true });
			handleScroll();

			return () => window.removeEventListener("scroll", handleScroll);
		});
	}

	get scrolled(): boolean {
		return this.#scrolled;
	}
}

export function createScroll(downThreshold: number, upThreshold?: number): ScrollState {
	return new ScrollState(downThreshold, upThreshold);
}

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

---

### Hero 2 <a name="hero-2"></a>

A premium hero 2 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `hero-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/hero-two.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:decor-icon`
  - `local:full-width-divider`
  - `local:logo-cloud-two`
  - `local:portal`

##### Component Source Code

###### File: `src/lib/components/efferd/hero/hero-two/preview.svelte`

```svelte
<script lang="ts">
	import HeaderTwo from "$lib/components/efferd/header/header-two/header-two.svelte";
	import Hero from "./hero.svelte";
	import LogoSection from "./logo-section.svelte";
</script>

<div
	class="relative flex min-h-screen flex-col overflow-hidden px-4 supports-[overflow:clip]:overflow-clip"
>
	<HeaderTwo />
	<main
		class="relative mx-auto max-w-4xl grow before:absolute before:-inset-y-14 before:-left-px before:w-px before:bg-border after:absolute after:-inset-y-14 after:-right-px after:w-px after:bg-border"
	>
		<Hero />
		<LogoSection />
	</main>
</div>

```

###### File: `src/lib/components/efferd/hero/hero-two/hero.svelte`

```svelte
<script lang="ts">
	import { ArrowRight, PhoneCall } from "@lucide/svelte";
	import { Button } from "$lib/components/ui/button";
	import { DecorIcon } from "$lib/components/ui/decor-icon";
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
</script>

<section>
	<div
		class="relative flex flex-col items-center justify-center gap-5 px-4 py-12 md:px-4 md:py-24 lg:py-28"
	>
		<div aria-hidden="true" class="absolute inset-0 -z-10 size-full overflow-hidden">
			<div
				class="absolute -inset-x-20 inset-y-0 z-0 rounded-full bg-[radial-gradient(ellipse_at_center,color-mix(in_oklab,var(--foreground)_10%,transparent),transparent,transparent)] blur-[50px]"
			></div>
			<div
				class="absolute inset-y-0 left-4 w-px bg-linear-to-b from-transparent via-border to-border md:left-8"
			></div>
			<div
				class="absolute inset-y-0 right-4 w-px bg-linear-to-b from-transparent via-border to-border md:right-8"
			></div>
			<div
				class="absolute inset-y-0 left-8 w-px bg-linear-to-b from-transparent via-border/50 to-border/50 md:left-12"
			></div>
			<div
				class="absolute inset-y-0 right-8 w-px bg-linear-to-b from-transparent via-border/50 to-border/50 md:right-12"
			></div>
		</div>

		<a
			class="group mx-auto flex w-fit animate-in items-center gap-3 rounded-sm border bg-card p-1 shadow transition-all delay-500 duration-500 ease-out fill-mode-backwards slide-in-from-bottom-10 fade-in"
			href="/"
		>
			<div class="rounded-xs border bg-card px-1.5 py-0.5 shadow-sm">
				<p class="font-mono text-xs">NOW</p>
			</div>

			<span class="text-xs">accepting new client projects</span>
			<span class="block h-5 border-l"></span>

			<div class="pr-1">
				<ArrowRight
					class="size-3 -translate-x-0.5 duration-150 ease-out group-hover:translate-x-0.5"
				/>
			</div>
		</a>

		<h1
			class="max-w-2xl animate-in text-center text-3xl text-balance text-foreground delay-100 duration-500 ease-out fill-mode-backwards slide-in-from-bottom-10 fade-in md:text-5xl lg:text-6xl"
		>
			Building Digital Experiences That Drive Growth
		</h1>

		<p
			class="animate-in text-center text-sm tracking-wider text-muted-foreground delay-200 duration-500 ease-out fill-mode-backwards slide-in-from-bottom-10 fade-in sm:text-lg"
		>
			We help brands scale faster through design,
			<br />
			development and strategic execution.
		</p>

		<div
			class="flex w-fit animate-in items-center justify-center gap-3 pt-2 delay-300 duration-500 ease-out fill-mode-backwards slide-in-from-bottom-10 fade-in"
		>
			<Button href="/" variant="outline">
				<PhoneCall class="size-4" />
				Book a Call
			</Button>
			<Button href="/">
				Get started
				<ArrowRight class="size-4" />
			</Button>
		</div>
	</div>

	<div class="relative">
		<DecorIcon class="size-4" position="top-left" />
		<DecorIcon class="size-4" position="top-right" />
		<DecorIcon class="size-4" position="bottom-left" />
		<DecorIcon class="size-4" position="bottom-right" />

		<FullWidthDivider class="-top-px" />
		<div class="overflow-hidden *:pointer-events-none *:aspect-video *:select-none">
			<img
				alt="light app screen"
				class="dark:hidden"
				height="auto"
				src="https://storage.efferd.com/screen/dashboard-light.webp"
				width="auto"
			/>
			<img
				alt="dark app screen"
				class="hidden dark:block"
				height="auto"
				src="https://storage.efferd.com/screen/dashboard-dark.webp"
				width="auto"
			/>
		</div>
		<FullWidthDivider class="-bottom-px" />
	</div>
</section>

```

###### File: `src/lib/components/efferd/hero/hero-two/logo-section.svelte`

```svelte
<script lang="ts">
	import LogoCloudTwo from "$lib/components/efferd/logo-cloud/two/logo-cloud-two.svelte";
	import { DecorIcon } from "$lib/components/ui/decor-icon";
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
</script>

<section class="mb-12">
	<h2
		class="py-6 text-center text-lg font-medium tracking-tight text-muted-foreground md:text-xl"
	>
		Trusted by <span class="text-foreground">experts</span>
	</h2>
	<div class="relative *:border-0">
		<DecorIcon class="size-4" position="top-left" />
		<DecorIcon class="size-4" position="top-right" />
		<DecorIcon class="size-4" position="bottom-left" />
		<DecorIcon class="size-4" position="bottom-right" />

		<FullWidthDivider class="-top-px" />
		<LogoCloudTwo />
		<FullWidthDivider class="-bottom-px" />
	</div>
</section>

```

###### File: `src/lib/components/efferd/header/header-two/header-two.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { createScroll } from "$lib/hooks/use-scroll.svelte";
	import Logo from "$lib/svgs/logo.svelte";
	import { cn } from "$lib/utils";
	import MobileNav from "./mobile-nav.svelte";
	import { navLinks } from "./nav-links";
	let scroll = createScroll(10);
</script>

<header
	class={cn(
		"sticky top-0 z-50 mx-auto w-full max-w-4xl border-b border-transparent md:rounded-md md:border md:transition-all md:ease-out",
		scroll.scrolled &&
			"border-border bg-background/95 backdrop-blur-sm supports-backdrop-filter:bg-background/50 md:top-2 md:max-w-3xl md:shadow"
	)}
>
	<nav
		class={cn(
			"flex h-14 w-full items-center justify-between px-4 md:h-12 md:transition-all md:ease-out",
			scroll.scrolled && "md:px-2"
		)}
	>
		<a class="rounded-md p-2 hover:bg-muted dark:hover:bg-muted/50" href="/">
			<Logo class="h-4" />
		</a>
		<div class="hidden items-center gap-2 md:flex">
			<div>
				{#each navLinks as { label, href }}
					<Button size="sm" variant="ghost" {href}>
						{label}
					</Button>
				{/each}
			</div>
			<Button size="sm" variant="outline">Sign In</Button>
			<Button size="sm">Get Started</Button>
		</div>
		<MobileNav />
	</nav>
</header>

```

###### File: `src/lib/components/efferd/header/header-two/mobile-nav.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { Portal, PortalBackdrop } from "$lib/components/ui/portal";
	import { cn } from "$lib/utils";
	import { MenuIcon, XIcon } from "@lucide/svelte";
	import { navLinks } from "./nav-links";
	let open = $state(false);
</script>

<div class="md:hidden">
	<Button
		aria-controls="mobile-menu"
		aria-expanded={open}
		aria-label="Toggle menu"
		class="md:hidden"
		size="icon"
		variant="outline"
		onclick={() => (open = !open)}
	>
		{#if open}
			<XIcon class="size-4.5" />
		{:else}
			<MenuIcon class="size-4.5" />
		{/if}
	</Button>
	{#if open}
		<Portal class="top-14">
			<PortalBackdrop />
			<div
				class={cn(
					"ease-out data-[slot=open]:animate-in data-[slot=open]:zoom-in-97",
					"size-full p-4"
				)}
				data-slot={open ? "open" : "closed"}
			>
				<div class="grid gap-y-2">
					{#each navLinks as link, i}
						<Button class="justify-start" variant="ghost" href={link.href}>
							{link.label}
						</Button>
					{/each}
				</div>
				<div class="mt-12 flex flex-col gap-2">
					<Button class="w-full" variant="outline">Sign In</Button>
					<Button class="w-full">Get Started</Button>
				</div>
			</div>
		</Portal>
	{/if}
</div>

```

###### File: `src/lib/components/efferd/header/header-two/nav-links.ts`

```ts
let navLinks = [
	{
		label: "Features",
		href: "/"
	},
	{
		label: "Pricing",
		href: "/"
	},
	{
		label: "About",
		href: "/"
	}
];

export { navLinks };

```

###### File: `src/lib/hooks/use-scroll.svelte.ts`

```ts
class ScrollState {
	#scrolled = $state(false);

	readonly #downThreshold: number;
	readonly #upThreshold: number;

	constructor(downThreshold: number, upThreshold?: number) {
		this.#downThreshold = downThreshold;
		this.#upThreshold = upThreshold ?? downThreshold / 2;

		$effect(() => {
			const handleScroll = () => {
				const y = window.scrollY;
				// Hysteresis Logic: Only update scrolled state when crossing thresholds in the appropriate direction
				this.#scrolled = this.#scrolled
					? y > this.#upThreshold // currently scrolled → only reset below upThreshold
					: y > this.#downThreshold; // currently not scrolled → only set above downThreshold
			};

			window.addEventListener("scroll", handleScroll, { passive: true });
			handleScroll();

			return () => window.removeEventListener("scroll", handleScroll);
		});
	}

	get scrolled(): boolean {
		return this.#scrolled;
	}
}

export function createScroll(downThreshold: number, upThreshold?: number): ScrollState {
	return new ScrollState(downThreshold, upThreshold);
}

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

---

### Hero 3 <a name="hero-3"></a>

A premium hero 3 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `hero-three`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/hero-three.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:logo-cloud-five`
  - `navigation-menu`
  - `local:portal`

##### Component Source Code

###### File: `src/lib/components/efferd/hero/hero-three/preview.svelte`

```svelte
<script lang="ts">
	import HeaderThree from "$lib/components/efferd/header/header-three/header-three.svelte";
	import Hero from "./hero.svelte";
	import LogoSection from "./logo-section.svelte";
</script>

<div class="flex min-h-screen flex-col">
	<HeaderThree />
	<main class="grow">
		<Hero />
		<LogoSection />
	</main>
</div>

```

###### File: `src/lib/components/efferd/hero/hero-three/hero.svelte`

```svelte
<script lang="ts">
	import { ArrowRight, PhoneCall } from "@lucide/svelte";
	import { Button } from "$lib/components/ui/button";
</script>

<section class="mx-auto w-full max-w-5xl overflow-hidden pt-16">
	<div aria-hidden="true" class="absolute inset-0 size-full overflow-hidden">
		<div
			class="absolute inset-0 isolate -z-10 bg-[radial-gradient(20%_80%_at_20%_0%,color-mix(in_oklab,var(--foreground)_10%,transparent),transparent)]"
		></div>
	</div>

	<div class="relative z-10 flex max-w-2xl flex-col gap-5 px-4">
		<a
			class="group flex w-fit animate-in items-center gap-3 rounded-sm border bg-card p-1 shadow-xs transition-all delay-500 duration-500 ease-out fill-mode-backwards slide-in-from-bottom-10 fade-in"
			href="/"
		>
			<div class="rounded-xs border bg-card px-1.5 py-0.5 shadow-sm">
				<p class="font-mono text-xs">NOW</p>
			</div>

			<span class="text-xs">accepting new client projects</span>
			<span class="block h-5 border-l"></span>

			<div class="pr-1">
				<ArrowRight
					class="size-3 -translate-x-0.5 duration-150 ease-out group-hover:translate-x-0.5"
				/>
			</div>
		</a>

		<h1
			class="animate-in text-4xl leading-tight font-medium text-balance text-foreground delay-100 duration-500 ease-out fill-mode-backwards slide-in-from-bottom-10 fade-in md:text-5xl"
		>
			Building Digital Experiences That Drive Growth
		</h1>

		<p
			class="animate-in text-sm tracking-wider text-muted-foreground delay-200 duration-500 ease-out fill-mode-backwards slide-in-from-bottom-10 fade-in sm:text-lg md:text-xl"
		>
			We help brands scale faster through design, development
			<br />
			and strategic execution.
		</p>

		<div
			class="flex w-fit animate-in items-center justify-center gap-3 pt-2 delay-300 duration-500 ease-out fill-mode-backwards slide-in-from-bottom-10 fade-in"
		>
			<Button href="/" variant="outline">
				<PhoneCall class="size-4" />
				Book a Call
			</Button>
			<Button href="/">
				Get started
				<ArrowRight class="size-4" />
			</Button>
		</div>
	</div>

	<div class="relative">
		<div
			class="absolute -inset-x-20 inset-y-0 -translate-y-1/3 scale-120 rounded-full bg-[radial-gradient(ellipse_at_center,color-mix(in_oklab,var(--foreground)_10%,transparent),transparent,transparent)] blur-[50px]"
		></div>
		<div
			class="relative mt-8 -mr-56 animate-in overflow-hidden [mask-image:linear-gradient(to_bottom,black_0%,black_60%,transparent_100%)] px-2 delay-100 duration-1000 ease-out fill-mode-backwards slide-in-from-bottom-5 fade-in sm:mt-12 sm:mr-0 md:mt-20"
		>
			<div
				class="relative mx-auto max-w-5xl overflow-hidden rounded-lg border bg-background p-2 shadow-xl ring-1 inset-shadow-2xs ring-card inset-shadow-foreground/10 dark:inset-shadow-xs dark:inset-shadow-foreground/20"
			>
				<img
					alt="app screen"
					class="z-2 aspect-video rounded-lg border dark:hidden"
					height="1080"
					src="https://storage.efferd.com/screen/dashboard-light.webp"
					width="1920"
				/>
				<img
					alt="app screen"
					class="hidden aspect-video rounded-lg bg-background dark:block"
					height="1080"
					src="https://storage.efferd.com/screen/dashboard-dark.webp"
					width="1920"
				/>
			</div>
		</div>
	</div>
</section>

```

###### File: `src/lib/components/efferd/hero/hero-three/logo-section.svelte`

```svelte
<script lang="ts">
	import LogoCloudFive from "$lib/components/efferd/logo-cloud/five/logo-cloud-five.svelte";
</script>

<section class="mx-auto h-full max-w-3xl space-y-4 px-4 py-10 md:px-8">
	<h2 class="text-center text-lg font-medium tracking-tight text-muted-foreground md:text-xl">
		Trusted by <span class="text-foreground">experts</span>
	</h2>
	<LogoCloudFive />
</section>

```

###### File: `src/lib/components/efferd/header/header-three/header-three.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { createScroll } from "$lib/hooks/use-scroll.svelte";
	import Logo from "$lib/svgs/logo.svelte";
	import { cn } from "$lib/utils";
	import DesktopNav from "./desktop-nav.svelte";
	import MobileNav from "./mobile-nav.svelte";

	let scroll = createScroll(50);
</script>

<header
	class={cn(
		"sticky top-0 z-50 w-full border-b border-transparent",
		scroll.scrolled &&
			"border-border bg-background/95 backdrop-blur-sm supports-backdrop-filter:bg-background/50"
	)}
>
	<nav class="mx-auto flex h-14 w-full max-w-5xl items-center justify-between px-4">
		<div class="flex items-center gap-5">
			<a class="rounded-lg px-3 py-2.5 hover:bg-muted dark:hover:bg-muted/50" href="/">
				<Logo class="h-4" />
			</a>
			<DesktopNav />
		</div>
		<div class="hidden items-center gap-2 md:flex">
			<Button variant="outline">Sign In</Button>
			<Button>Get Started</Button>
		</div>
		<MobileNav />
	</nav>
</header>

```

###### File: `src/lib/components/efferd/header/header-three/desktop-nav.svelte`

```svelte
<script lang="ts">
	import * as NavigationMenu from "$lib/components/ui/navigation-menu/index";
	import type { HTMLAttributes } from "svelte/elements";
	import LinkItem from "./link-item.svelte";
	import { companyLinks, companyLinks2, productLinks } from "./nav-links";

	type ListItemProps = HTMLAttributes<HTMLAnchorElement> & {
		title: string;
		href: string;
		content: string;
	};
</script>

<NavigationMenu.Root class="hidden md:flex">
	<NavigationMenu.List>
		<NavigationMenu.Item>
			<NavigationMenu.Trigger class="bg-transparent">Product</NavigationMenu.Trigger>
			<NavigationMenu.Content class="NavigationMenuContent p-0">
				<div class="bg-muted/50 p-1 pr-1.5 dark:bg-background">
					<div
						class="grid w-lg grid-cols-2 gap-2 rounded-lg border bg-popover p-2 shadow"
					>
						{#each productLinks as item, i}
							<NavigationMenu.Link>
								<LinkItem {...item} />
							</NavigationMenu.Link>
						{/each}
					</div>
					<div class="p-2">
						<p class="text-sm text-muted-foreground">
							Interested?{" "}
							<a class="font-medium text-foreground hover:underline" href="/">
								Schedule a demo
							</a>
						</p>
					</div>
				</div>
			</NavigationMenu.Content>
		</NavigationMenu.Item>
		<NavigationMenu.Item>
			<NavigationMenu.Trigger class="bg-transparent">Company</NavigationMenu.Trigger>
			<NavigationMenu.Content class="p-0">
				<div class="bg-muted/50 p-1 pr-1.5 pb-1.5 dark:bg-background">
					<div class="grid w-lg grid-cols-2 gap-2">
						<div class="space-y-2 rounded-lg border bg-popover p-2 shadow">
							{#each companyLinks as item, i}
								<NavigationMenu.Link>
									<LinkItem {...item} />
								</NavigationMenu.Link>
							{/each}
						</div>
						<div class="space-y-2 p-3">
							{#each companyLinks2 as item, i}
								{@const IconComp = item.icon}
								<NavigationMenu.Link>
									<div class="flex items-center gap-2">
										<IconComp />
										{item.label}
									</div>
								</NavigationMenu.Link>
							{/each}
						</div>
					</div>
				</div>
			</NavigationMenu.Content>
		</NavigationMenu.Item>
		<NavigationMenu.Item>
			<NavigationMenu.Link class="rounded-md p-2 px-4 hover:bg-accent">
				{#snippet child({ props })}
					<a href="/docs" {...props}>Docs</a>
				{/snippet}
			</NavigationMenu.Link>
		</NavigationMenu.Item>
	</NavigationMenu.List>
</NavigationMenu.Root>

```

###### File: `src/lib/components/efferd/header/header-three/mobile-nav.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { Portal, PortalBackdrop } from "$lib/components/ui/portal";
	import { cn } from "$lib/utils";
	import { MenuIcon, XIcon } from "@lucide/svelte";
	import LinkItem from "./link-item.svelte";
	import { productLinks, companyLinks, companyLinks2 } from "./nav-links";

	let open = $state(false);
</script>

<div class="md:hidden">
	<Button
		aria-controls="mobile-menu"
		aria-expanded={open}
		aria-label="Toggle menu"
		class="md:hidden"
		onclick={() => (open = !open)}
		size="icon"
		variant="outline"
	>
		<div class={cn("transition-all", open ? "scale-100 opacity-100" : "scale-0 opacity-0")}>
			<XIcon />
		</div>
		<div
			class={cn(
				"absolute transition-all",
				open ? "scale-0 opacity-0" : "scale-100 opacity-100"
			)}
		>
			<MenuIcon />
		</div>
	</Button>
	{#if open}
		<Portal class="top-14">
			<PortalBackdrop />
			<div
				class={cn(
					"size-full overflow-y-auto p-4",
					"ease-out data-[slot=open]:animate-in data-[slot=open]:zoom-in-97"
				)}
				data-slot={open ? "open" : "closed"}
			>
				<div class="flex w-full flex-col gap-y-2">
					<span class="text-sm">Product</span>
					{#each productLinks as link}
						<LinkItem
							class="rounded-lg p-2 active:bg-muted dark:active:bg-muted/50"
							{...link}
						/>
					{/each}
					<span class="text-sm">Company</span>
					{#each companyLinks as link}
						<LinkItem
							class="rounded-lg p-2 active:bg-muted dark:active:bg-muted/50"
							{...link}
						/>
					{/each}
					{#each companyLinks2 as link}
						<LinkItem
							class="rounded-lg p-2 active:bg-muted dark:active:bg-muted/50"
							{...link}
						/>
					{/each}
				</div>
				<div class="mt-5 flex flex-col gap-2">
					<Button class="w-full" variant="outline">Sign In</Button>
					<Button class="w-full">Get Started</Button>
				</div>
			</div>
		</Portal>
	{/if}
</div>

```

###### File: `src/lib/components/efferd/header/header-three/link-item.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";
	import type { LinkItemType } from "./types";
	import type { Component } from "svelte";
	import type { Icon } from "@lucide/svelte";

	type Props = LinkItemType & HTMLAttributes<HTMLAnchorElement>;
	let { label, description, icon, href, class: className, ...props }: Props = $props();

	// i have added Svelte Component, Lucide Icon so you can have custom-icon.svelte and lucide icon both
	let IconComponent: Component | typeof Icon = $derived(icon);
</script>

<a class={cn("flex items-center gap-x-2", className)} {href} {...props}>
	<div
		class={cn(
			"flex aspect-square size-9 items-center justify-center rounded-md border bg-card text-sm shadow-sm",
			"[&_svg:not([class*='size-'])]:size-4 [&_svg:not([class*='size-'])]:text-foreground"
		)}
	>
		<IconComponent />
	</div>
	<div class="flex flex-col items-start justify-center">
		<span class="font-medium">{label}</span>
		<span class="line-clamp-2 text-xs text-muted-foreground">
			{description}
		</span>
	</div>
</a>

```

###### File: `src/lib/components/efferd/header/header-three/nav-links.ts`

```ts
import {
	FileTextIcon,
	GlobeIcon,
	LayersIcon,
	PlugIcon,
	ShieldIcon,
	UserPlusIcon,
	UsersIcon,
	StarIcon,
	HandshakeIcon,
	CodeIcon,
	RotateCcwIcon,
	LeafIcon,
	HandHelpingIcon,
	ChartArea
} from "@lucide/svelte";

import type { LinkItemType } from "./types";

let productLinks: LinkItemType[] = [
	{
		label: "Website Builder",
		href: "/",
		description: "Create responsive websites with ease",
		icon: GlobeIcon
	},
	{
		label: "Cloud Platform",
		href: "/",
		description: "Deploy and scale apps in the cloud",
		icon: LayersIcon
	},
	{
		label: "Team Collaboration",
		href: "/",
		description: "Tools to help your teams work better together",
		icon: UserPlusIcon
	},
	{
		label: "Analytics",
		href: "/",
		description: "Track and analyze your website traffic",
		icon: ChartArea
	},
	{
		label: "Integrations",
		href: "/",
		description: "Connect your apps and services",
		icon: PlugIcon
	},
	{
		label: "API",
		href: "/",
		description: "Build custom integrations with our API",
		icon: CodeIcon
	}
];

let companyLinks: LinkItemType[] = [
	{
		label: "About Us",
		href: "/",
		description: "Learn more about our story and team",
		icon: UsersIcon
	},
	{
		label: "Customer Stories",
		href: "/",
		description: "See how we've helped our clients succeed",
		icon: StarIcon
	},
	{
		label: "Partnerships",
		href: "/",
		icon: HandshakeIcon,
		description: "Collaborate with us for mutual growth"
	}
];

let companyLinks2: LinkItemType[] = [
	{
		label: "Terms of Service",
		href: "/",
		icon: FileTextIcon
	},
	{
		label: "Privacy Policy",
		href: "/",
		icon: ShieldIcon
	},
	{
		label: "Refund Policy",
		href: "/",
		icon: RotateCcwIcon
	},
	{
		label: "Blog",
		href: "/",
		icon: LeafIcon
	},
	{
		label: "Help Center",
		href: "/",
		icon: HandHelpingIcon
	}
];

export { productLinks, companyLinks, companyLinks2 };

```

###### File: `src/lib/components/efferd/header/header-three/types.ts`

```ts
import type { Icon } from "@lucide/svelte";
import type { Component } from "svelte";

export type LinkItemType = {
	label: string;
	description?: string;
	icon: typeof Icon | Component;
	href: string;
};

```

###### File: `src/lib/hooks/use-scroll.svelte.ts`

```ts
class ScrollState {
	#scrolled = $state(false);

	readonly #downThreshold: number;
	readonly #upThreshold: number;

	constructor(downThreshold: number, upThreshold?: number) {
		this.#downThreshold = downThreshold;
		this.#upThreshold = upThreshold ?? downThreshold / 2;

		$effect(() => {
			const handleScroll = () => {
				const y = window.scrollY;
				// Hysteresis Logic: Only update scrolled state when crossing thresholds in the appropriate direction
				this.#scrolled = this.#scrolled
					? y > this.#upThreshold // currently scrolled → only reset below upThreshold
					: y > this.#downThreshold; // currently not scrolled → only set above downThreshold
			};

			window.addEventListener("scroll", handleScroll, { passive: true });
			handleScroll();

			return () => window.removeEventListener("scroll", handleScroll);
		});
	}

	get scrolled(): boolean {
		return this.#scrolled;
	}
}

export function createScroll(downThreshold: number, upThreshold?: number): ScrollState {
	return new ScrollState(downThreshold, upThreshold);
}

```

###### File: `src/lib/svgs/logo.svelte`

```svelte
<script lang="ts">
	import type { SVGAttributes } from "svelte/elements";

	let { ...props }: SVGAttributes<SVGSVGElement> = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	class="size-4"
	viewBox="0 0 24 24"
	fill="none"
	xmlns:xlink="http://www.w3.org/1999/xlink"
	role="img"
	color="currentColor"
	{...props}
>
	<path
		d="M12 21H12H12C16.2426 21 18.364 21 19.682 19.682C21 18.364 21 16.2426 21 12V12V12C21 7.75735 21 5.63604 19.682 4.31802C18.364 3 16.2426 3 12 3C7.75736 3 5.63604 3 4.31802 4.31802C3 5.63604 3 7.75736 3 12C3 16.2426 3 18.364 4.31802 19.682C5.63604 21 7.75735 21 12 21Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
	<path
		opacity="0.4"
		d="M15.7275 9.36502C16 9.8998 16 10.5999 16 12C16 13.4001 16 14.1002 15.7275 14.635C15.4878 15.1054 15.1054 15.4878 14.635 15.7275C14.1002 16 13.4001 16 12 16C10.5999 16 9.8998 16 9.36502 15.7275C8.89462 15.4878 8.51217 15.1054 8.27248 14.635C8 14.1002 8 13.4001 8 12C8 10.5999 8 9.8998 8.27248 9.36502C8.51217 8.89462 8.89462 8.51217 9.36502 8.27248C9.8998 8 10.5999 8 12 8C13.4001 8 14.1002 8 14.635 8.27248C15.1054 8.51217 15.4878 8.89462 15.7275 9.36502Z"
		stroke="currentColor"
		stroke-width="1.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	></path>
</svg>

```

---

## Image Gallery Blocks <a name="image-gallery"></a>

### Image Gallery 1 <a name="image-gallery-1"></a>

A premium image gallery 1 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `image-gallery-1`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/image-gallery-1.json
```

##### Dependencies

- **NPM Packages**:
  - `motion`
- **Registry Dependencies**:
  - `aspect-ratio`

##### Component Source Code

###### File: `registry/blocks/image-gallery/1/image-gallery.tsx`

```tsx
import { LazyImage } from "./lazy-image";

export function ImageGallery() {
  return (
    <div className="relative flex min-h-screen w-full flex-col items-center justify-center px-4 py-10">
      <div className="mx-auto grid w-full max-w-5xl grid-cols-1 gap-4 sm:grid-cols-2 md:grid-cols-4 md:gap-6">
        {Array.from({ length: 4 }).map((_, col) => (
          <div className="grid gap-4" key={col}>
            {/* biome-ignore lint/nursery/noShadow: false positive */}
            {Array.from({ length: 8 }).map((_, index) => {
              const isPortrait = Math.random() > 0.5;
              const width = isPortrait ? 1080 : 1920;
              const height = isPortrait ? 1920 : 1080;
              const ratio = isPortrait ? 9 / 16 : 16 / 9;

              return (
                <LazyImage
                  alt={`Image ${col}-${index}`}
                  fallback={`https://placehold.co/${width}x${height}/`}
                  inView={true}
                  key={`${col}-${index}`}
                  ratio={ratio}
                  src={`https://picsum.photos/seed/${col}-${index}/${width}/${height}`}
                />
              );
            })}
          </div>
        ))}
      </div>
    </div>
  );
}

```

###### File: `registry/blocks/image-gallery/1/lazy-image.tsx`

```tsx
"use client";

import { useInView } from "motion/react";
import React from "react";
import { AspectRatio } from "@/components/ui/aspect-ratio";
import { cn } from "@/lib/utils";

type LazyImageProps = {
  alt: string;
  src: string;
  className?: string;
  AspectRatioClassName?: string;
  /** URL of the fallback image. default: undefined */
  fallback?: string;
  /** The ratio of the image. */
  ratio: number;
  /** Whether the image should only load when it is in view. default: false */
  inView?: boolean;
};

export function LazyImage({
  alt,
  src,
  ratio,
  fallback,
  inView = false,
  className,
  AspectRatioClassName,
}: LazyImageProps) {
  const ref = React.useRef<HTMLDivElement | null>(null);
  const imgRef = React.useRef<HTMLImageElement | null>(null);
  const isInView = useInView(ref, { once: true });

  const [imgSrc, setImgSrc] = React.useState<string | undefined>(
    inView ? undefined : src
  );
  const [isLoading, setIsLoading] = React.useState(true);

  const handleError = () => {
    if (fallback) {
      setImgSrc(fallback);
    }
    setIsLoading(false);
  };

  const handleLoad = React.useCallback(() => {
    setIsLoading(false);
  }, []);

  // Load image only when inView
  React.useEffect(() => {
    if (inView && isInView && !imgSrc) {
      setImgSrc(src);
    }
  }, [inView, isInView, src, imgSrc]);

  // Handle cached images instantly
  React.useEffect(() => {
    if (imgRef.current?.complete) {
      handleLoad();
    }
  }, [handleLoad]);

  return (
    <AspectRatio
      className={cn(
        "relative size-full overflow-hidden rounded-lg border bg-accent/30",
        AspectRatioClassName
      )}
      ratio={ratio}
      ref={ref}
    >
      {imgSrc && (
        // biome-ignore lint/nursery/useImageSize: dynamic image size
        <img
          alt={alt}
          className={cn(
            "size-full rounded-md object-cover transition-opacity duration-500",
            isLoading ? "opacity-0" : "opacity-100",
            className
          )}
          decoding="async"
          fetchPriority={inView ? "high" : "low"}
          loading="lazy"
          onError={handleError}
          onLoad={handleLoad}
          ref={imgRef}
          role="presentation" // Changed from "img" to "presentation" since it's decorative
          src={imgSrc}
        />
      )}
    </AspectRatio>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `image-gallery-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/image-gallery-one.json
```

##### Dependencies

- **NPM Packages**:
  - `motion-sv`
- **Registry Dependencies**:
  - `aspect-ratio`

##### Component Source Code

###### File: `src/lib/components/efferd/image-gallery/image-gallery.svelte`

```svelte
<script lang="ts">
	import LazyImage from "./lazy-image.svelte";

	type GalleryImage = {
		id: string;
		alt: string;
		src: string;
		fallback: string;
		ratio: number;
	};

	function getOrientation(column: number, index: number) {
		const seed = Math.sin((column + 1) * 97 + (index + 1) * 131) * 10_000;
		return seed - Math.floor(seed) > 0.5;
	}

	function createImage(column: number, index: number): GalleryImage {
		const isPortrait = getOrientation(column, index);
		const width = isPortrait ? 1080 : 1920;
		const height = isPortrait ? 1920 : 1080;

		return {
			id: `${column}-${index}`,
			alt: `Image ${column}-${index}`,
			fallback: `https://placehold.co/${width}x${height}/`,
			ratio: isPortrait ? 9 / 16 : 16 / 9,
			src: `https://picsum.photos/seed/${column}-${index}/${width}/${height}`
		};
	}

	const columns = Array.from({ length: 4 }, (_, column) =>
		Array.from({ length: 8 }, (_, index) => createImage(column, index))
	);
</script>

<section class="relative flex min-h-screen w-full flex-col items-center justify-center px-4 py-10">
	<div
		class="mx-auto grid w-full max-w-5xl grid-cols-1 gap-4 sm:grid-cols-2 md:grid-cols-4 md:gap-6"
	>
		{#each columns as column, columnIndex (columnIndex)}
			<div class="grid gap-4">
				{#each column as image (image.id)}
					<LazyImage
						alt={image.alt}
						containerClassName="rounded-xl"
						fallback={image.fallback}
						inView={true}
						ratio={image.ratio}
						src={image.src}
					/>
				{/each}
			</div>
		{/each}
	</div>
</section>

```

###### File: `src/lib/components/efferd/image-gallery/lazy-image.svelte`

```svelte
<script lang="ts">
	import { AspectRatio } from "$lib/components/ui/aspect-ratio";
	import { cn } from "$lib/utils";
	import { useInView } from "motion-sv";

	type LazyImageProps = {
		alt: string;
		src: string;
		class?: string;
		containerClassName?: string;
		fallback?: string;
		ratio: number;
		inView?: boolean;
	};

	let {
		alt,
		src,
		class: className,
		containerClassName,
		fallback,
		ratio,
		inView = false
	}: LazyImageProps = $props();

	let containerRef = $state<HTMLElement | null>(null);
	let imageRef = $state<HTMLImageElement | null>(null);
	let fallbackSource = $state<string | undefined>(undefined);
	let isLoading = $state(false);
	let lastSource: string | undefined = undefined;
	let revealStartFrame: number | null = null;
	let revealCommitFrame: number | null = null;

	let view = useInView(
		() => containerRef as HTMLElement,
		() => ({ once: true }) as any
	);

	let imageSource = $derived(
		fallbackSource ?? (inView ? (view.isInView ? src : undefined) : src)
	);

	function handleError() {
		if (fallback && imageSource !== fallback) {
			fallbackSource = fallback;
			return;
		}

		cancelReveal();
		isLoading = false;
	}

	function handleLoad() {
		scheduleReveal();
	}

	function cancelReveal() {
		if (revealStartFrame !== null) {
			cancelAnimationFrame(revealStartFrame);
			revealStartFrame = null;
		}

		if (revealCommitFrame !== null) {
			cancelAnimationFrame(revealCommitFrame);
			revealCommitFrame = null;
		}
	}

	function scheduleReveal() {
		cancelReveal();

		// The image needs to be painted once at opacity-0 before we flip it to opacity-100.
		revealStartFrame = requestAnimationFrame(() => {
			revealStartFrame = null;
			revealCommitFrame = requestAnimationFrame(() => {
				revealCommitFrame = null;
				isLoading = false;
			});
		});
	}

	$effect(() => {
		src;
		fallback;
		fallbackSource = undefined;
	});

	$effect(() => {
		if (imageSource !== lastSource) {
			cancelReveal();
			lastSource = imageSource;
			isLoading = Boolean(imageSource);
		}
	});

	$effect(() => {
		if (!imageSource || !imageRef?.complete || !isLoading) {
			return;
		}

		scheduleReveal();
	});

	$effect(() => {
		return () => {
			cancelReveal();
		};
	});
</script>

<AspectRatio
	bind:ref={containerRef}
	class={cn("relative size-full overflow-hidden border bg-accent/30", containerClassName)}
	{ratio}
>
	{#if imageSource}
		<img
			bind:this={imageRef}
			{alt}
			class={cn(
				"size-full object-cover transition-opacity duration-500",
				isLoading ? "opacity-0" : "opacity-100",
				className
			)}
			decoding="async"
			fetchpriority={inView ? "high" : "low"}
			loading="lazy"
			onerror={handleError}
			onload={handleLoad}
			src={imageSource}
		/>
	{/if}
</AspectRatio>

```

---

## Logo Cloud Blocks <a name="logo-cloud"></a>

### Logo Cloud 1 <a name="logo-cloud-1"></a>

A premium logo cloud 1 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `logo-cloud-1`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/logo-cloud-1.json
```

##### Component Source Code

###### File: `registry/blocks/logo-cloud/1/logo-cloud.tsx`

```tsx
import { cn } from "@/lib/utils";

type Logo = {
  src: string;
  alt: string;
  width?: number;
  height?: number;
};

type LogoCloudProps = React.ComponentProps<"div"> & {
  logos: Logo[];
};

export function LogoCloud({ logos, className, ...props }: LogoCloudProps) {
  return (
    <div
      className={cn(
        "mx-auto grid max-w-3xl grid-cols-2 rounded-lg bg-border shadow md:grid-cols-4",
        className
      )}
      {...props}
    >
      {logos.map((logo) => (
        <div
          className="flex items-center justify-center rounded-lg border bg-background p-8"
          key={logo.alt}
        >
          <img
            alt={logo.alt}
            className="pointer-events-none block h-4 select-none md:h-5 dark:brightness-0 dark:invert"
            height={logo.height ?? "auto"}
            loading="lazy"
            src={logo.src}
            width={logo.width ?? "auto"}
          />
        </div>
      ))}
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `logo-cloud-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/logo-cloud-one.json
```

##### Component Source Code

###### File: `src/lib/components/efferd/logo-cloud/logo-cloud-one.svelte`

```svelte
<script lang="ts">
	const logos = [
		{
			src: "https://storage.efferd.com/logo/nvidia-wordmark.svg",
			alt: "Nvidia Logo"
		},
		{
			src: "https://storage.efferd.com/logo/supabase-wordmark.svg",
			alt: "Supabase Logo"
		},
		{
			src: "https://storage.efferd.com/logo/openai-wordmark.svg",
			alt: "OpenAI Logo"
		},
		{
			src: "https://storage.efferd.com/logo/turso-wordmark.svg",
			alt: "Turso Logo"
		},
		{
			src: "https://storage.efferd.com/logo/vercel-wordmark.svg",
			alt: "Vercel Logo"
		},
		{
			src: "https://storage.efferd.com/logo/github-wordmark.svg",
			alt: "GitHub Logo"
		},
		{
			src: "https://storage.efferd.com/logo/claude-wordmark.svg",
			alt: "Claude AI Logo"
		},
		{
			src: "https://storage.efferd.com/logo/clerk-wordmark.svg",
			alt: "Clerk Logo"
		}
	];
</script>

<div class="grid grid-cols-2 rounded-lg bg-border shadow md:grid-cols-4">
	{#each logos as logo, i}
		<div class="flex items-center justify-center rounded-lg border bg-background p-8">
			<img
				alt={logo.alt}
				class="pointer-events-none block h-4 select-none md:h-5 dark:brightness-0 dark:invert"
				height="auto"
				loading="lazy"
				src={logo.src}
				width="auto"
			/>
		</div>
	{/each}
</div>

```

---

### Logo Cloud 2 <a name="logo-cloud-2"></a>

A premium logo cloud 2 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `logo-cloud-2`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/logo-cloud-2.json
```

##### Component Source Code

###### File: `registry/blocks/logo-cloud/2/logo-cloud.tsx`

```tsx
import { PlusIcon } from "lucide-react";
import { cn } from "@/lib/utils";

type Logo = {
  src: string;
  alt: string;
  width?: number;
  height?: number;
};

type LogoCloudProps = React.ComponentProps<"div">;

export function LogoCloud({ className, ...props }: LogoCloudProps) {
  return (
    <div
      className={cn("grid grid-cols-2 border md:grid-cols-4", className)}
      {...props}
    >
      <LogoCard
        className="relative border-r border-b bg-secondary dark:bg-secondary/30"
        logo={{
          src: "https://svgl.app/library/nvidia-wordmark-light.svg",
          alt: "Nvidia Logo",
        }}
      >
        <PlusIcon
          className="-right-[12.5px] -bottom-[12.5px] absolute z-10 size-6"
          strokeWidth={1}
        />
      </LogoCard>

      <LogoCard
        className="border-b md:border-r"
        logo={{
          src: "https://svgl.app/library/supabase_wordmark_light.svg",
          alt: "Supabase Logo",
        }}
      />

      <LogoCard
        className="relative border-r border-b md:bg-secondary dark:md:bg-secondary/30"
        logo={{
          src: "https://svgl.app/library/github_wordmark_light.svg",
          alt: "GitHub Logo",
        }}
      >
        <PlusIcon
          className="-right-[12.5px] -bottom-[12.5px] absolute z-10 size-6"
          strokeWidth={1}
        />
        <PlusIcon
          className="-bottom-[12.5px] -left-[12.5px] absolute z-10 hidden size-6 md:block"
          strokeWidth={1}
        />
      </LogoCard>

      <LogoCard
        className="relative border-b bg-secondary md:bg-background dark:bg-secondary/30 md:dark:bg-background"
        logo={{
          src: "https://svgl.app/library/openai_wordmark_light.svg",
          alt: "OpenAI Logo",
        }}
      />

      <LogoCard
        className="relative border-r border-b bg-secondary md:border-b-0 md:bg-background dark:bg-secondary/30 md:dark:bg-background"
        logo={{
          src: "https://svgl.app/library/turso-wordmark-light.svg",
          alt: "Turso Logo",
        }}
      >
        <PlusIcon
          className="-right-[12.5px] -bottom-[12.5px] md:-left-[12.5px] absolute z-10 size-6 md:hidden"
          strokeWidth={1}
        />
      </LogoCard>

      <LogoCard
        className="border-b bg-background md:border-r md:border-b-0 md:bg-secondary dark:md:bg-secondary/30"
        logo={{
          src: "https://svgl.app/library/clerk-wordmark-light.svg",
          alt: "Clerk Logo",
        }}
      />

      <LogoCard
        className="border-r"
        logo={{
          src: "https://svgl.app/library/claude-ai-wordmark-icon_light.svg",
          alt: "Claude AI Logo",
        }}
      />

      <LogoCard
        className="bg-secondary dark:bg-secondary/30"
        logo={{
          src: "https://svgl.app/library/vercel_wordmark.svg",
          alt: "Vercel Logo",
        }}
      />
    </div>
  );
}

type LogoCardProps = React.ComponentProps<"div"> & {
  logo: Logo;
};

function LogoCard({ logo, className, children, ...props }: LogoCardProps) {
  return (
    <div
      className={cn(
        "flex items-center justify-center bg-background px-4 py-8 md:p-8",
        className
      )}
      {...props}
    >
      <img
        alt={logo.alt}
        className="pointer-events-none h-4 select-none md:h-5 dark:brightness-0 dark:invert"
        height={logo.height || "auto"}
        src={logo.src}
        width={logo.width || "auto"}
      />
      {children}
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `logo-cloud-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/logo-cloud-two.json
```

##### Dependencies

- **Registry Dependencies**:
  - `local:decor-icon`

##### Component Source Code

###### File: `src/lib/components/efferd/logo-cloud/two/logo-cloud-two.svelte`

```svelte
<script lang="ts">
	import { DecorIcon } from "$lib/components/ui/decor-icon";
	import LogoCard from "./logo-card.svelte";
</script>

<div class="grid grid-cols-2 border md:grid-cols-4">
	<LogoCard
		class="relative border-r border-b bg-secondary dark:bg-secondary/30"
		logo={{
			src: "https://storage.efferd.com/logo/nvidia-wordmark.svg",
			alt: "Nvidia Logo"
		}}
	>
		<DecorIcon class="z-10" position="bottom-right" />
	</LogoCard>

	<LogoCard
		class="border-b md:border-r"
		logo={{
			src: "https://storage.efferd.com/logo/supabase-wordmark.svg",
			alt: "Supabase Logo"
		}}
	/>

	<LogoCard
		class="relative border-r border-b md:bg-secondary dark:md:bg-secondary/30"
		logo={{
			src: "https://storage.efferd.com/logo/github-wordmark.svg",
			alt: "GitHub Logo"
		}}
	>
		<DecorIcon class="z-10" position="bottom-right" />
		<DecorIcon class="z-10 hidden md:block" position="bottom-left" />
	</LogoCard>

	<LogoCard
		class="relative border-b bg-secondary md:bg-background dark:bg-secondary/30 md:dark:bg-background"
		logo={{
			src: "https://storage.efferd.com/logo/openai-wordmark.svg",
			alt: "OpenAI Logo"
		}}
	/>

	<LogoCard
		class="relative border-r border-b bg-secondary md:border-b-0 md:bg-background dark:bg-secondary/30 md:dark:bg-background"
		logo={{
			src: "https://storage.efferd.com/logo/turso-wordmark.svg",
			alt: "Turso Logo"
		}}
	>
		<DecorIcon class="z-10 md:hidden" position="bottom-right" />
	</LogoCard>

	<LogoCard
		class="border-b bg-background md:border-r md:border-b-0 md:bg-secondary dark:md:bg-secondary/30"
		logo={{
			src: "https://storage.efferd.com/logo/clerk-wordmark.svg",
			alt: "Clerk Logo"
		}}
	/>

	<LogoCard
		class="border-r"
		logo={{
			src: "https://storage.efferd.com/logo/claude-wordmark.svg",
			alt: "Claude AI Logo"
		}}
	/>

	<LogoCard
		class="bg-secondary dark:bg-secondary/30"
		logo={{
			src: "https://storage.efferd.com/logo/vercel-wordmark.svg",
			alt: "Vercel Logo"
		}}
	/>
</div>

```

###### File: `src/lib/components/efferd/logo-cloud/two/logo-card.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";
	import type { Snippet } from "svelte";
	import type { HTMLAttributes } from "svelte/elements";

	type Logo = {
		src: string;
		alt: string;
	};
	type LogoCardProps = HTMLAttributes<HTMLDivElement> & {
		logo: Logo;
		class?: string;
		children?: Snippet;
	};

	let { logo, class: className = "", children, ...props }: LogoCardProps = $props();
</script>

<div
	class={cn("flex items-center justify-center bg-background px-4 py-8 md:p-8", className)}
	{...props}
>
	<img
		alt={logo.alt}
		class="pointer-events-none h-4 select-none md:h-5 dark:brightness-0 dark:invert"
		height="auto"
		src={logo.src}
		width="auto"
	/>
	{@render children?.()}
</div>

```

---

### Logo Cloud 3 <a name="logo-cloud-3"></a>

A premium logo cloud 3 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `logo-cloud-3`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/logo-cloud-3.json
```

##### Dependencies

- **Registry Dependencies**:
  - `https://motion-primitives.com/c/infinite-slider.json`

##### Component Source Code

###### File: `registry/blocks/logo-cloud/3/logo-cloud.tsx`

```tsx
// https://motion-primitives.com/docs/infinite-slider
import { InfiniteSlider } from "@/components/ui/infinite-slider";

import { cn } from "@/lib/utils";

type Logo = {
  src: string;
  alt: string;
  width?: number;
  height?: number;
};

type LogoCloudProps = React.ComponentProps<"div"> & {
  logos: Logo[];
};

export function LogoCloud({ className, logos, ...props }: LogoCloudProps) {
  return (
    <div
      {...props}
      className={cn(
        "overflow-hidden py-4 [mask-image:linear-gradient(to_right,transparent,black,transparent)]",
        className
      )}
    >
      <InfiniteSlider gap={42} reverse speed={80} speedOnHover={25}>
        {logos.map((logo) => (
          <img
            alt={logo.alt}
            className="pointer-events-none h-4 select-none md:h-5 dark:brightness-0 dark:invert"
            height={logo.height || "auto"}
            key={`logo-${logo.alt}`}
            loading="lazy"
            src={logo.src}
            width={logo.width || "auto"}
          />
        ))}
      </InfiniteSlider>
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `logo-cloud-three`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/logo-cloud-three.json
```

##### Dependencies

- **Registry Dependencies**:
  - `local:marquee`

##### Component Source Code

###### File: `src/lib/components/efferd/logo-cloud/three/logo-cloud-three.svelte`

```svelte
<script lang="ts">
	import { Marquee } from "$lib/components/magic/marquee";

	const logos = [
		{
			src: "https://storage.efferd.com/logo/nvidia-wordmark.svg",
			alt: "Nvidia Logo"
		},
		{
			src: "https://storage.efferd.com/logo/supabase-wordmark.svg",
			alt: "Supabase Logo"
		},
		{
			src: "https://storage.efferd.com/logo/openai-wordmark.svg",
			alt: "OpenAI Logo"
		},
		{
			src: "https://storage.efferd.com/logo/turso-wordmark.svg",
			alt: "Turso Logo"
		},
		{
			src: "https://storage.efferd.com/logo/vercel-wordmark.svg",
			alt: "Vercel Logo"
		},
		{
			src: "https://storage.efferd.com/logo/github-wordmark.svg",
			alt: "GitHub Logo"
		},
		{
			src: "https://storage.efferd.com/logo/claude-wordmark.svg",
			alt: "Claude AI Logo"
		},
		{
			src: "https://storage.efferd.com/logo/clerk-wordmark.svg",
			alt: "Clerk Logo"
		}
	];
</script>

<div class="overflow-hidden mask-[linear-gradient(to_right,transparent,black,transparent)] py-4">
	<Marquee>
		{#each logos as logo}
			<img
				alt={logo.alt}
				class="pointer-events-none h-4 select-none md:h-5 dark:brightness-0 dark:invert"
				height="auto"
				loading="lazy"
				src={logo.src}
				width="auto"
			/>
		{/each}
	</Marquee>
</div>

```

---

### Logo Cloud 4 <a name="logo-cloud-4"></a>

A premium logo cloud 4 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `logo-cloud-4`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/logo-cloud-4.json
```

##### Dependencies

- **Registry Dependencies**:
  - `https://motion-primitives.com/c/infinite-slider.json`
  - `https://motion-primitives.com/c/progressive-blur.json`

##### Component Source Code

###### File: `registry/blocks/logo-cloud/4/logo-cloud.tsx`

```tsx
// https://motion-primitives.com/docs/infinite-slider
import { InfiniteSlider } from "@/components/ui/infinite-slider";
// https://motion-primitives.com/docs/progressive-blur
import { ProgressiveBlur } from "@/components/ui/progressive-blur";
import { cn } from "@/lib/utils";

type Logo = {
  src: string;
  alt: string;
  width?: number;
  height?: number;
};

type LogoCloudProps = React.ComponentProps<"div"> & {
  logos: Logo[];
};

export function LogoCloud({ className, logos, ...props }: LogoCloudProps) {
  return (
    <div
      className={cn(
        "relative mx-auto max-w-3xl border bg-gradient-to-r from-secondary via-transparent to-secondary py-6",
        className
      )}
      {...props}
    >
      <InfiniteSlider gap={42} reverse speed={60} speedOnHover={20}>
        {logos.map((logo) => (
          <img
            alt={logo.alt}
            className="pointer-events-none h-4 select-none md:h-5 dark:brightness-0 dark:invert"
            height="auto"
            key={`logo-${logo.alt}`}
            loading="lazy"
            src={logo.src}
            width="auto"
          />
        ))}
      </InfiniteSlider>

      <ProgressiveBlur
        blurIntensity={1}
        className="pointer-events-none absolute top-0 left-0 h-full w-[100px] md:w-[160px]"
        direction="left"
      />
      <ProgressiveBlur
        blurIntensity={1}
        className="pointer-events-none absolute top-0 right-0 h-full w-[100px] md:w-[160px]"
        direction="right"
      />
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `logo-cloud-four`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/logo-cloud-four.json
```

##### Dependencies

- **Registry Dependencies**:
  - `local:marquee`
  - `local:progressive-blur`

##### Component Source Code

###### File: `src/lib/components/efferd/logo-cloud/four/logo-cloud-four.svelte`

```svelte
<script lang="ts">
	import { Marquee } from "$lib/components/magic/marquee";
	import { ProgressiveBlur } from "$lib/components/ui/progressive-blur";

	const logos = [
		{
			src: "https://storage.efferd.com/logo/nvidia-wordmark.svg",
			alt: "Nvidia Logo"
		},
		{
			src: "https://storage.efferd.com/logo/supabase-wordmark.svg",
			alt: "Supabase Logo"
		},
		{
			src: "https://storage.efferd.com/logo/openai-wordmark.svg",
			alt: "OpenAI Logo"
		},
		{
			src: "https://storage.efferd.com/logo/turso-wordmark.svg",
			alt: "Turso Logo"
		},
		{
			src: "https://storage.efferd.com/logo/vercel-wordmark.svg",
			alt: "Vercel Logo"
		},
		{
			src: "https://storage.efferd.com/logo/github-wordmark.svg",
			alt: "GitHub Logo"
		},
		{
			src: "https://storage.efferd.com/logo/claude-wordmark.svg",
			alt: "Claude AI Logo"
		},
		{
			src: "https://storage.efferd.com/logo/clerk-wordmark.svg",
			alt: "Clerk Logo"
		}
	];
</script>

<div
	class="relative border-x border-y bg-linear-to-r from-secondary/50 via-transparent to-secondary/50 py-6"
>
	<Marquee class="flex items-center gap-20" style="--duration: 30s; --gap: 2rem;">
		{#each logos as logo}
			<img
				alt={logo.alt}
				class="pointer-events-none h-4 select-none md:h-5 dark:brightness-0 dark:invert"
				height="auto"
				loading="lazy"
				src={logo.src}
				width="auto"
			/>
		{/each}
	</Marquee>

	<ProgressiveBlur
		blurIntensity={1}
		class="pointer-events-none absolute top-0 left-0 h-full w-[100px] md:w-[160px]"
		direction="left"
	/>
	<ProgressiveBlur
		blurIntensity={1}
		class="pointer-events-none absolute top-0 right-0 h-full w-[100px] md:w-[160px]"
		direction="right"
	/>
</div>

```

---

### Logo Cloud 5 <a name="logo-cloud-5"></a>

A premium logo cloud 5 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `logo-cloud-five`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/logo-cloud-five.json
```

##### Component Source Code

###### File: `src/lib/components/efferd/logo-cloud/five/logo-cloud-five.svelte`

```svelte
<script lang="ts">
	const logos = [
		{
			src: "https://storage.efferd.com/logo/vercel-wordmark.svg",
			alt: "Vercel Logo"
		},
		{
			src: "https://storage.efferd.com/logo/supabase-wordmark.svg",
			alt: "Supabase Logo"
		},
		{
			src: "https://storage.efferd.com/logo/openai-wordmark.svg",
			alt: "OpenAI Logo"
		},
		{
			src: "https://storage.efferd.com/logo/dub-wordmark.svg",
			alt: "Dub Logo"
		},
		{
			src: "https://storage.efferd.com/logo/turso-wordmark.svg",
			alt: "Turso Logo"
		},

		{
			src: "https://storage.efferd.com/logo/github-wordmark.svg",
			alt: "GitHub Logo"
		},
		{
			src: "https://storage.efferd.com/logo/claude-wordmark.svg",
			alt: "Claude AI Logo"
		},
		{
			src: "https://storage.efferd.com/logo/nvidia-wordmark.svg",
			alt: "Nvidia Logo"
		},
		{
			src: "https://storage.efferd.com/logo/clerk-wordmark.svg",
			alt: "Clerk Logo"
		},
		{
			src: "https://storage.efferd.com/logo/bolt-wordmark.svg",
			alt: "Bolt Logo"
		},

		{
			src: "https://storage.efferd.com/logo/stripe-wordmark.svg",
			alt: "Stripe Logo"
		}
	];
</script>

<div
	class="relative flex flex-wrap items-center justify-center gap-x-10 gap-y-8 py-6 sm:gap-x-12 sm:gap-y-12"
>
	{#each logos as logo}
		<img
			alt={logo.alt}
			class="pointer-events-none h-5 w-fit select-none dark:brightness-0 dark:invert"
			height="auto"
			loading="lazy"
			src={logo.src}
			width="auto"
		/>
	{/each}
</div>

```

---

## Not Found Blocks <a name="not-found"></a>

### Not Found 1 <a name="not-found-1"></a>

A premium not found 1 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `not-found-1`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/not-found-1.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`
  - `empty`

##### Component Source Code

###### File: `registry/blocks/not-found/1/not-found.tsx`

```tsx
import { Compass, Home } from "lucide-react";
import { Button } from "@/components/ui/button";
import {
  Empty,
  EmptyContent,
  EmptyDescription,
  EmptyHeader,
  EmptyTitle,
} from "@/components/ui/empty";

export function NotFoundPage() {
  return (
    <div className="flex w-full items-center justify-center">
      <div className="flex h-screen items-center border-x">
        <div>
          <div className="absolute inset-x-0 h-px bg-border" />
          <Empty>
            <EmptyHeader>
              <EmptyTitle className="font-black font-mono text-8xl">
                404
              </EmptyTitle>
              <EmptyDescription className="text-nowrap">
                The page you're looking for might have been <br />
                moved or doesn't exist.
              </EmptyDescription>
            </EmptyHeader>
            <EmptyContent>
              <div className="flex gap-2">
                <Button asChild>
                  <a href="#">
                    <Home /> Go Home
                  </a>
                </Button>

                <Button asChild variant="outline">
                  <a href="#">
                    <Compass /> Explore
                  </a>
                </Button>
              </div>
            </EmptyContent>
          </Empty>
          <div className="absolute inset-x-0 h-px bg-border" />
        </div>
      </div>
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `not-found-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/not-found-one.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:full-width-divider`
  - `empty`

##### Component Source Code

###### File: `src/lib/components/efferd/not-found/not-found-one.svelte`

```svelte
<script lang="ts">
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import * as Empty from "$lib/components/ui/empty/index";
	import { Button } from "$lib/components/ui/button";
	import { CompassIcon, HomeIcon } from "@lucide/svelte";
</script>

<div class="flex w-full items-center justify-center overflow-hidden">
	<div class="flex h-screen items-center border-x">
		<div>
			<FullWidthDivider />
			<Empty.Root>
				<Empty.Header>
					<Empty.Title class="font-mono text-8xl font-black">404</Empty.Title>
					<Empty.Description class="text-nowrap">
						The page you're looking for might have been <br />
						moved or doesn't exist.
					</Empty.Description>
				</Empty.Header>
				<Empty.Content>
					<div class="flex gap-2">
						<Button href="/">
							<HomeIcon data-icon="inline-start" />
							Go Home
						</Button>

						<Button href="/" variant="outline">
							<CompassIcon data-icon="inline-start" />
							Explore
						</Button>
					</div>
				</Empty.Content>
			</Empty.Root>
			<FullWidthDivider />
		</div>
	</div>
</div>

```

---

### Not Found 2 <a name="not-found-2"></a>

A premium not found 2 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `not-found-2`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/not-found-2.json
```

##### Dependencies

- **NPM Packages**:
  - `motion`
- **Registry Dependencies**:
  - `button`
  - `empty`

##### Component Source Code

###### File: `registry/blocks/not-found/2/not-found.tsx`

```tsx
"use client";

import { Compass, Home } from "lucide-react";
import { motion } from "motion/react";
import { Button } from "@/components/ui/button";
import {
  Empty,
  EmptyContent,
  EmptyDescription,
  EmptyHeader,
  EmptyTitle,
} from "@/components/ui/empty";

const PRIMARY_ORB_HORIZONTAL_OFFSET = 40;
const PRIMARY_ORB_VERTICAL_OFFSET = 20;

export function NotFoundPage() {
  return (
    <div className="relative flex min-h-screen w-full items-center justify-center overflow-hidden bg-[radial-gradient(circle_at_center,rgba(99,102,241,0.1),transparent_70%)] text-[var(--foreground)]">
      <div
        aria-hidden={true}
        className="-z-10 absolute inset-0 overflow-hidden"
      >
        <motion.div
          animate={{
            x: [
              0,
              PRIMARY_ORB_HORIZONTAL_OFFSET,
              -PRIMARY_ORB_HORIZONTAL_OFFSET,
              0,
            ],
            y: [
              0,
              PRIMARY_ORB_VERTICAL_OFFSET,
              -PRIMARY_ORB_VERTICAL_OFFSET,
              0,
            ],
            rotate: [0, 10, -10, 0],
          }}
          className="absolute top-1/2 left-1/3 size-90 rounded-full bg-gradient-to-tr from-purple-500/20 to-blue-500/20 blur-3xl"
          transition={{
            repeat: Number.POSITIVE_INFINITY,
            duration: 4,
            ease: "easeInOut",
          }}
        />
        <motion.div
          animate={{
            x: [
              0,
              -PRIMARY_ORB_HORIZONTAL_OFFSET,
              PRIMARY_ORB_HORIZONTAL_OFFSET,
              0,
            ],
            y: [
              0,
              -PRIMARY_ORB_VERTICAL_OFFSET,
              PRIMARY_ORB_VERTICAL_OFFSET,
              0,
            ],
          }}
          className="absolute right-1/4 bottom-1/3 size-120 rounded-full bg-gradient-to-br from-indigo-400/10 to-pink-400/10 blur-3xl"
          transition={{
            repeat: Number.POSITIVE_INFINITY,
            duration: 4,
            ease: "easeInOut",
          }}
        />
      </div>

      <Empty>
        <EmptyHeader>
          <EmptyTitle className="font-extrabold text-8xl">404</EmptyTitle>
          <EmptyDescription className="text-nowrap">
            The page you're looking for might have been <br />
            moved or doesn't exist.
          </EmptyDescription>
        </EmptyHeader>
        <EmptyContent>
          <div className="flex gap-2">
            <Button asChild>
              <a href="#">
                <Home /> Go Home
              </a>
            </Button>

            <Button asChild variant="outline">
              <a href="#">
                <Compass /> Explore
              </a>
            </Button>
          </div>
        </EmptyContent>
      </Empty>
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `not-found-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/not-found-two.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:full-width-divider`
  - `empty`

##### Component Source Code

###### File: `src/lib/components/efferd/not-found/not-found-two.svelte`

```svelte
<script lang="ts">
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import * as Empty from "$lib/components/ui/empty/index";
	import { Button } from "$lib/components/ui/button";
	import { CompassIcon, HomeIcon } from "@lucide/svelte";
</script>

<div class="relative flex min-h-screen w-full items-center justify-center overflow-hidden">
	<Empty.Root>
		<Empty.Header>
			<Empty.Title class="mask-b-from-20% mask-b-to-80% text-9xl font-extrabold"
				>404</Empty.Title
			>
			<Empty.Description class="-mt-8 text-nowrap text-foreground/80">
				The page you're looking for might have been <br />
				moved or doesn't exist.
			</Empty.Description>
		</Empty.Header>
		<Empty.Content>
			<div class="flex gap-2">
				<Button href="/">
					<HomeIcon data-icon="inline-start" />
					Go Home
				</Button>

				<Button href="/" variant="outline">
					<CompassIcon data-icon="inline-start" />{" "}
					Explore
				</Button>
			</div>
		</Empty.Content>
	</Empty.Root>
</div>

```

---

## Pricing Blocks <a name="pricing"></a>

### Pricing 1 <a name="pricing-1"></a>

A premium pricing 1 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `pricing-1`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/pricing-1.json
```

##### Dependencies

- **NPM Packages**:
  - `motion`
- **Registry Dependencies**:
  - `button`
  - `tooltip`

##### Component Source Code

###### File: `registry/blocks/pricing/1/pricing-section.tsx`

```tsx
"use client";
import { CheckCircleIcon, StarIcon } from "lucide-react";
import Link from "next/link";
import React from "react";
import { Button } from "@/components/ui/button";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/tooltip";
import { cn } from "@/lib/utils";
import { type FREQUENCY, FrequencyToggle } from "./frequency-toggle";

export type Plan = {
  name: string;
  info: string;
  price: {
    monthly: number;
    yearly: number;
  };
  features: {
    text: string;
    tooltip?: string;
  }[];
  btn: {
    text: string;
    href: string;
  };
  highlighted?: boolean;
};

interface PricingSectionProps extends React.ComponentProps<"div"> {
  plans: Plan[];
  heading: string;
  description?: string;
}

export function PricingSection({
  plans,
  heading,
  description,
  ...props
}: PricingSectionProps) {
  const [frequency, setFrequency] = React.useState<"monthly" | "yearly">(
    "monthly"
  );

  return (
    <div
      className={cn(
        "flex w-full flex-col items-center justify-center space-y-7 p-4",
        props.className
      )}
      {...props}
    >
      <div className="mx-auto max-w-xl space-y-2">
        <h2 className="text-center font-bold text-2xl tracking-tight md:text-3xl lg:font-extrabold lg:text-4xl">
          {heading}
        </h2>
        {description && (
          <p className="text-center text-muted-foreground text-sm md:text-base">
            {description}
          </p>
        )}
      </div>

      <FrequencyToggle frequency={frequency} setFrequency={setFrequency} />
      <div className="mx-auto grid w-full max-w-4xl grid-cols-1 gap-6 md:grid-cols-3">
        {plans.map((plan) => (
          <PricingCard frequency={frequency} key={plan.name} plan={plan} />
        ))}
      </div>
    </div>
  );
}

type PricingCardProps = React.ComponentProps<"div"> & {
  plan: Plan;
  frequency?: FREQUENCY;
};

export function PricingCard({
  plan,
  className,
  frequency = "monthly",
  ...props
}: PricingCardProps) {
  return (
    <div
      className={cn(
        "relative flex w-full flex-col rounded-lg border shadow",
        plan.highlighted && "scale-105",
        className
      )}
      key={plan.name}
      {...props}
    >
      <div
        className={cn(
          "rounded-t-lg border-b p-4",
          plan.highlighted && "bg-card dark:bg-card/80"
        )}
      >
        <div className="absolute top-2 right-2 z-10 flex items-center gap-2">
          {plan.highlighted && (
            <p className="flex items-center gap-1 rounded-md border bg-background px-2 py-0.5 text-xs">
              <StarIcon className="h-3 w-3 fill-current" />
              Popular
            </p>
          )}
          {frequency === "yearly" && (
            <p className="flex items-center gap-1 rounded-md border bg-primary px-2 py-0.5 text-primary-foreground text-xs">
              {Math.round(
                ((plan.price.monthly * 12 - plan.price.yearly) /
                  plan.price.monthly /
                  12) *
                  100
              )}
              % off
            </p>
          )}
        </div>

        <div className="font-medium text-lg">{plan.name}</div>
        <p className="font-normal text-muted-foreground text-sm">{plan.info}</p>
        <h3 className="mt-6 mb-2 flex items-end gap-1">
          <span className="font-extrabold text-3xl">
            ${plan.price[frequency]}
          </span>
          <span className="text-muted-foreground">
            {plan.name !== "Free"
              ? `/${frequency === "monthly" ? "month" : "year"}`
              : ""}
          </span>
        </h3>
      </div>
      <div
        className={cn(
          "space-y-4 px-4 pt-6 pb-8 text-muted-foreground text-sm",
          plan.highlighted && "bg-muted/10"
        )}
      >
        {plan.features.map((feature, index) => (
          <div className="flex items-center gap-2" key={index}>
            <CheckCircleIcon className="h-4 w-4 text-foreground" />
            <TooltipProvider>
              <Tooltip delayDuration={0}>
                <TooltipTrigger asChild>
                  <p
                    className={cn(feature.tooltip && "cursor-pointer border-b")}
                  >
                    {feature.text}
                  </p>
                </TooltipTrigger>
                {feature.tooltip && (
                  <TooltipContent>
                    <p>{feature.tooltip}</p>
                  </TooltipContent>
                )}
              </Tooltip>
            </TooltipProvider>
          </div>
        ))}
      </div>
      <div
        className={cn(
          "mt-auto w-full rounded-b-lg border-t p-3",
          plan.highlighted && "bg-card dark:bg-card/80"
        )}
      >
        <Button
          asChild
          className="w-full"
          variant={plan.highlighted ? "default" : "outline"}
        >
          <Link href={plan.btn.href}>{plan.btn.text}</Link>
        </Button>
      </div>
    </div>
  );
}

```

###### File: `registry/blocks/pricing/1/frequency-toggle.tsx`

```tsx
"use client";
import { motion } from "motion/react";
import type React from "react";
import { cn } from "@/lib/utils";

export type FREQUENCY = "monthly" | "yearly";

type FrequencyToggleProps = React.ComponentProps<"div"> & {
  frequency: FREQUENCY;
  setFrequency: React.Dispatch<React.SetStateAction<FREQUENCY>>;
  frequencies?: FREQUENCY[];
};

export function FrequencyToggle({
  frequency,
  setFrequency,
  frequencies = ["monthly", "yearly"],
  ...props
}: FrequencyToggleProps) {
  return (
    <div
      className={cn(
        "mx-auto flex w-fit rounded-full border bg-card p-1 shadow",
        props.className
      )}
      {...props}
    >
      {frequencies.map((freq) => (
        <button
          className="relative px-4 py-1 text-sm capitalize"
          key={freq}
          onClick={() => setFrequency(freq)}
        >
          <span className="relative z-10">{freq}</span>
          {frequency === freq && (
            <motion.span
              className="absolute inset-0 z-10 rounded-full bg-background mix-blend-difference dark:bg-foreground"
              layoutId="frequency"
              transition={{ type: "spring", duration: 0.4 }}
            />
          )}
        </button>
      ))}
    </div>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `pricing-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/pricing-one.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:decor-icon`

##### Component Source Code

###### File: `src/lib/components/efferd/pricing/pricing-one.svelte`

```svelte
<script lang="ts">
	import { ShieldCheck } from "@lucide/svelte";
	import { Button } from "$lib/components/ui/button";
	import { DecorIcon } from "$lib/components/ui/decor-icon";
	import { cn } from "$lib/utils";

	type Plan = {
		name: string;
		description: string;
		originalPrice: string;
		discount: string;
		price: string;
		cta: string;
		featured?: boolean;
	};

	const plans: Plan[] = [
		{
			name: "Monthly",
			description: "Best value for growing businesses!",
			originalPrice: "$8.99",
			discount: "11% off",
			price: "7.99",
			cta: "Start Your Journey"
		},
		{
			name: "Yearly",
			description: "Unlock savings with an annual commitment!",
			originalPrice: "$8.99",
			discount: "22% off",
			price: "6.99",
			cta: "Get Started Now",
			featured: true
		}
	];
</script>

<section class="w-full space-y-5">
	<div class="mx-auto max-w-lg">
		<div class="flex justify-center">
			<div class="rounded-md border px-4 py-1 text-sm">Pricing</div>
		</div>

		<h2 class="mt-4 text-center text-2xl font-bold tracking-tight md:text-3xl">
			Pricing Based on Your Success
		</h2>

		<p class="mt-2 text-center text-sm text-muted-foreground md:text-base">
			We offer a single price for all our services. We believe that pricing is a critical
			component of any successful business.
		</p>
	</div>

	<div class="mx-auto w-full max-w-2xl space-y-2">
		<div class="relative grid border bg-background p-4 shadow-xs md:grid-cols-2">
			<DecorIcon class="size-3" position="top-left" />
			<DecorIcon class="size-3" position="top-right" />
			<DecorIcon class="size-3" position="bottom-left" />
			<DecorIcon class="size-3" position="bottom-right" />

			{#each plans as plan}
				<div
					class={cn(
						"w-full",
						plan.featured
							? "relative rounded-md border bg-card p-4 shadow dark:bg-card/80"
							: "px-4 pt-5 pb-4"
					)}
				>
					<div class="space-y-1">
						<div class="flex items-center justify-between gap-3">
							<h3 class="leading-none font-semibold">{plan.name}</h3>

							<div class="flex items-center gap-x-1">
								<span class="text-sm text-muted-foreground line-through">
									{plan.originalPrice}
								</span>
								<span
									class={cn(
										"inline-flex items-center rounded-md border px-2 py-0.5 text-xs font-medium",
										plan.featured
											? "bg-primary text-primary-foreground shadow-xs"
											: "bg-secondary text-secondary-foreground"
									)}
								>
									{plan.discount}
								</span>
							</div>
						</div>

						<p class="text-sm text-muted-foreground">{plan.description}</p>
					</div>

					<div class="mt-10 space-y-4">
						<div class="flex items-end gap-0.5 text-xl text-muted-foreground">
							<span>$</span>
							<span
								class="-mb-0.5 text-4xl font-extrabold tracking-tighter text-foreground"
							>
								{plan.price}
							</span>
							<span>/month</span>
						</div>

						<Button
							class="w-full"
							href="/"
							variant={plan.featured ? "default" : "outline"}
						>
							{plan.cta}
						</Button>
					</div>
				</div>
			{/each}
		</div>

		<div class="flex items-center justify-center gap-x-2 text-sm text-muted-foreground">
			<ShieldCheck class="size-4" />
			<span>Access to all features with no hidden fees</span>
		</div>
	</div>
</section>

```

---

### Pricing 2 <a name="pricing-2"></a>

A premium pricing 2 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `pricing-2`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/pricing-2.json
```

##### Component Source Code

###### File: `components/pricing-card.tsx`

```tsx
import type React from "react";
import { cn } from "@/lib/utils";

function Card({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      className={cn(
        "relative w-full max-w-xs rounded-xl border bg-card dark:bg-transparent",
        "p-1.5 shadow-sm backdrop-blur-xl",
        className
      )}
      {...props}
    />
  );
}

function Header({
  className,
  children,
  glassEffect = true,
  ...props
}: React.ComponentProps<"div"> & {
  glassEffect?: boolean;
}) {
  return (
    <div
      className={cn(
        "relative mb-4 rounded-xl border bg-muted/50 p-4 shadow",
        className
      )}
      {...props}
    >
      {/* Top glass gradient */}
      {glassEffect && (
        <div
          aria-hidden="true"
          className="-z-10 absolute inset-x-0 top-0 h-48 rounded-[inherit]"
          style={{
            background:
              "linear-gradient(180deg, rgba(255,255,255,0.07) 0%, rgba(255,255,255,0.03) 40%, rgba(0,0,0,0) 100%)",
          }}
        />
      )}
      {children}
    </div>
  );
}

function Plan({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      className={cn("mb-8 flex items-center justify-between", className)}
      {...props}
    />
  );
}

function Description({ className, ...props }: React.ComponentProps<"p">) {
  return (
    <p className={cn("text-muted-foreground text-xs", className)} {...props} />
  );
}

function PlanName({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      className={cn(
        "flex items-center gap-2 font-medium text-muted-foreground text-sm [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    />
  );
}

function Badge({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <span
      className={cn(
        "rounded-full border border-foreground/20 px-2 py-0.5 text-foreground/80 text-xs",
        className
      )}
      {...props}
    />
  );
}

function Price({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div className={cn("mb-3 flex items-end gap-1", className)} {...props} />
  );
}

function MainPrice({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <span
      className={cn("font-extrabold text-3xl tracking-tight", className)}
      {...props}
    />
  );
}

function Period({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <span
      className={cn("pb-1 text-foreground/80 text-sm", className)}
      {...props}
    />
  );
}

function OriginalPrice({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <span
      className={cn(
        "mr-1 ml-auto text-lg text-muted-foreground line-through",
        className
      )}
      {...props}
    />
  );
}

function Body({ className, ...props }: React.ComponentProps<"div">) {
  return <div className={cn("space-y-6 p-3", className)} {...props} />;
}

function List({ className, ...props }: React.ComponentProps<"ul">) {
  return <ul className={cn("space-y-3", className)} {...props} />;
}

function ListItem({ className, ...props }: React.ComponentProps<"li">) {
  return (
    <li
      className={cn(
        "flex items-start gap-3 text-muted-foreground text-sm",
        className
      )}
      {...props}
    />
  );
}

function Separator({
  children = "Upgrade to access",
  className,
  ...props
}: React.ComponentProps<"div"> & {
  children?: string;
  className?: string;
}) {
  return (
    <div
      className={cn(
        "flex items-center gap-3 text-muted-foreground text-sm",
        className
      )}
      {...props}
    >
      <span className="h-[1px] flex-1 bg-muted-foreground/40" />
      <span className="shrink-0 text-muted-foreground">{children}</span>
      <span className="h-[1px] flex-1 bg-muted-foreground/40" />
    </div>
  );
}

export {
  Card,
  Header,
  Description,
  Plan,
  PlanName,
  Badge,
  Price,
  MainPrice,
  Period,
  OriginalPrice,
  Body,
  List,
  ListItem,
  Separator,
};

```

#### Svelte Implementation

- **Registry Key**: `pricing-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/pricing-two.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`
  - `local:full-width-divider`

##### Component Source Code

###### File: `src/lib/components/efferd/pricing/pricing-two.svelte`

```svelte
<script lang="ts">
	import { Check } from "@lucide/svelte";
	import { Button } from "$lib/components/ui/button";
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";

	type PricingPlan = {
		name: string;
		price: string;
		period?: string;
		description: string;
		href?: string;
		featuresTitle: string;
		features: string[];
		isPopular?: boolean;
	};

	const pricingPlans: PricingPlan[] = [
		{
			name: "STARTER",
			price: "Free",
			description: "For early-stage startups",
			featuresTitle: "FREE, FOREVER:",
			features: ["10 customers", "10 documents", "10 invoices", "Auto-updated taxes"],
			href: "/"
		},
		{
			name: "SCALE",
			isPopular: true,
			href: "/",
			price: "$8",
			period: "month",
			description: "For fast-growing teams",
			featuresTitle: "EVERYTHING IN STARTER, PLUS:",
			features: [
				"20 customers",
				"25 documents",
				"30 invoices",
				"Auto-updated taxes",
				"Cloud Sync"
			]
		}
	];
</script>

<section class="mx-auto min-h-screen max-w-5xl place-content-center border-x py-4">
	<div class="relative">
		<FullWidthDivider position="top" />
		<FullWidthDivider position="bottom" />

		<div class="grid grid-cols-1 gap-px bg-border md:grid-cols-2 lg:grid-cols-4">
			<div class="flex flex-col bg-background p-8 md:col-span-2">
				<p class="mb-6 text-sm tracking-wider text-muted-foreground uppercase">PRICING</p>
				<h1 class="text-3xl leading-tight font-bold md:text-5xl">
					Pricing that doesn&apos;t suck
				</h1>
			</div>

			{#each pricingPlans as plan}
				<div class="flex flex-col bg-background *:px-4 *:py-6">
					<div class="border-b">
						<p class="mb-6 text-sm tracking-wider text-muted-foreground uppercase">
							{plan.name}
						</p>

						<div class="mb-2 flex items-baseline gap-2">
							<h2 class="text-4xl font-bold">{plan.price}</h2>
							{#if plan.period}
								<span class="text-xs text-muted-foreground">/ {plan.period}</span>
							{/if}
						</div>

						<p class="mb-8 line-clamp-1 text-muted-foreground">{plan.description}</p>

						<Button
							class="w-full"
							href={plan.href}
							variant={plan.isPopular ? "default" : "outline"}
						>
							Get started
						</Button>
					</div>

					<div class="space-y-3 text-sm text-muted-foreground">
						<p class="mb-6 text-xs uppercase">{plan.featuresTitle}</p>

						{#each plan.features as feature}
							<p class="flex items-center gap-2 text-foreground/80">
								<Check class="size-4" />
								{feature}
							</p>
						{/each}
					</div>
				</div>
			{/each}
		</div>
	</div>
</section>

```

---

### Pricing 3 <a name="pricing-3"></a>

A premium pricing 3 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `pricing-3`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/pricing-3.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `registry/blocks/pricing/3/pricing-section.tsx`

```tsx
"use client";

import { Briefcase, Building, CheckCircle2, Users } from "lucide-react";
import * as PricingCard from "@/components/pricing-card";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";

export function PricingSection() {
  const plans = [
    {
      icon: <Users />,
      description: "Perfect for individuals",
      name: "Basic",
      price: "Free",
      variant: "outline",
      features: [
        "Automated Meeting Scheduling",
        "Basic Calendar Sync",
        "Daily Schedule Overview",
        "Email Reminders",
        "Task Management",
        "24/7 Customer Support",
        "Single User Access",
        "Basic Reporting",
        "Mobile App Access",
      ],
    },
    {
      icon: <Briefcase />,
      description: "Ideal for small teams",
      name: "Pro",
      badge: "Popular",
      price: "$29",
      original: "$39",
      period: "/month",
      variant: "default",
      features: [
        "All Basic Plan Features",
        "Advanced Calendar Integrations",
        "Customizable Notifications",
        "Priority Support",
        "Analytics and Insights",
        "Group Scheduling",
        "Multiple User Roles",
        "Advanced Reporting",
        "Custom Branding Options",
      ],
    },
    {
      icon: <Building />,
      name: "Enterprise",
      description: "Perfect for large scale companies",
      price: "$99",
      original: "$129",
      period: "/month",
      variant: "outline",
      features: [
        "All Pro Plan Features",
        "Dedicated Account Manager",
        "Custom Integrations",
        "Advanced Security Features",
        "Team Collaboration Tools",
        "Onboarding and Training",
        "Unlimited Users",
        "API Access with Higher Limits",
        "Advanced Audit Logs",
      ],
    },
  ];

  return (
    <section className="mx-auto grid w-full max-w-4xl gap-4 p-6 md:grid-cols-3">
      {plans.map((plan, index) => (
        <PricingCard.Card
          className={cn("w-full max-w-full", index === 1 && "md:scale-105")}
          key={plan.name}
        >
          <PricingCard.Header>
            <PricingCard.Plan>
              <PricingCard.PlanName>
                {plan.icon}
                <span className="text-muted-foreground">{plan.name}</span>
              </PricingCard.PlanName>
              {plan.badge && (
                <PricingCard.Badge>{plan.badge}</PricingCard.Badge>
              )}
            </PricingCard.Plan>
            <PricingCard.Price>
              <PricingCard.MainPrice>{plan.price}</PricingCard.MainPrice>
              <PricingCard.Period>{plan.period}</PricingCard.Period>
              {plan.original && (
                <PricingCard.OriginalPrice className="ml-auto">
                  {plan.original}
                </PricingCard.OriginalPrice>
              )}
            </PricingCard.Price>
            <Button
              className={cn("w-full font-semibold")}
              variant={plan.variant as "outline" | "default"}
            >
              Get Started
            </Button>
          </PricingCard.Header>

          <PricingCard.Body>
            <PricingCard.Description>
              {plan.description}
            </PricingCard.Description>
            <PricingCard.List>
              {plan.features.map((item) => (
                <PricingCard.ListItem className="text-xs" key={item}>
                  <CheckCircle2
                    aria-hidden="true"
                    className="h-4 w-4 text-foreground"
                  />
                  <span>{item}</span>
                </PricingCard.ListItem>
              ))}
            </PricingCard.List>
          </PricingCard.Body>
        </PricingCard.Card>
      ))}
    </section>
  );
}

```

###### File: `components/pricing-card.tsx`

```tsx
import type React from "react";
import { cn } from "@/lib/utils";

function Card({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      className={cn(
        "relative w-full max-w-xs rounded-xl border bg-card dark:bg-transparent",
        "p-1.5 shadow-sm backdrop-blur-xl",
        className
      )}
      {...props}
    />
  );
}

function Header({
  className,
  children,
  glassEffect = true,
  ...props
}: React.ComponentProps<"div"> & {
  glassEffect?: boolean;
}) {
  return (
    <div
      className={cn(
        "relative mb-4 rounded-xl border bg-muted/50 p-4 shadow",
        className
      )}
      {...props}
    >
      {/* Top glass gradient */}
      {glassEffect && (
        <div
          aria-hidden="true"
          className="-z-10 absolute inset-x-0 top-0 h-48 rounded-[inherit]"
          style={{
            background:
              "linear-gradient(180deg, rgba(255,255,255,0.07) 0%, rgba(255,255,255,0.03) 40%, rgba(0,0,0,0) 100%)",
          }}
        />
      )}
      {children}
    </div>
  );
}

function Plan({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      className={cn("mb-8 flex items-center justify-between", className)}
      {...props}
    />
  );
}

function Description({ className, ...props }: React.ComponentProps<"p">) {
  return (
    <p className={cn("text-muted-foreground text-xs", className)} {...props} />
  );
}

function PlanName({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      className={cn(
        "flex items-center gap-2 font-medium text-muted-foreground text-sm [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    />
  );
}

function Badge({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <span
      className={cn(
        "rounded-full border border-foreground/20 px-2 py-0.5 text-foreground/80 text-xs",
        className
      )}
      {...props}
    />
  );
}

function Price({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div className={cn("mb-3 flex items-end gap-1", className)} {...props} />
  );
}

function MainPrice({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <span
      className={cn("font-extrabold text-3xl tracking-tight", className)}
      {...props}
    />
  );
}

function Period({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <span
      className={cn("pb-1 text-foreground/80 text-sm", className)}
      {...props}
    />
  );
}

function OriginalPrice({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <span
      className={cn(
        "mr-1 ml-auto text-lg text-muted-foreground line-through",
        className
      )}
      {...props}
    />
  );
}

function Body({ className, ...props }: React.ComponentProps<"div">) {
  return <div className={cn("space-y-6 p-3", className)} {...props} />;
}

function List({ className, ...props }: React.ComponentProps<"ul">) {
  return <ul className={cn("space-y-3", className)} {...props} />;
}

function ListItem({ className, ...props }: React.ComponentProps<"li">) {
  return (
    <li
      className={cn(
        "flex items-start gap-3 text-muted-foreground text-sm",
        className
      )}
      {...props}
    />
  );
}

function Separator({
  children = "Upgrade to access",
  className,
  ...props
}: React.ComponentProps<"div"> & {
  children?: string;
  className?: string;
}) {
  return (
    <div
      className={cn(
        "flex items-center gap-3 text-muted-foreground text-sm",
        className
      )}
      {...props}
    >
      <span className="h-[1px] flex-1 bg-muted-foreground/40" />
      <span className="shrink-0 text-muted-foreground">{children}</span>
      <span className="h-[1px] flex-1 bg-muted-foreground/40" />
    </div>
  );
}

export {
  Card,
  Header,
  Description,
  Plan,
  PlanName,
  Badge,
  Price,
  MainPrice,
  Period,
  OriginalPrice,
  Body,
  List,
  ListItem,
  Separator,
};

```

#### Svelte Implementation

- **Registry Key**: `pricing-three`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/pricing-three.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `src/lib/components/efferd/pricing/pricing-three.svelte`

```svelte
<script lang="ts">
	import { Briefcase, Building, Users, type Icon as IconType } from "@lucide/svelte";
	import PricingCard from "$lib/components/efferd/pricing/pricing-card.svelte";
	import { cn } from "$lib/utils";
	import type { Component } from "svelte";

	type Plan = {
		icon: typeof IconType | Component;
		description: string;
		name: string;
		price: string;
		variant: "outline" | "default";
		features: string[];
		badge?: string;
		original?: string;
		period?: string;
		href?: string;
	};

	const plans: Plan[] = [
		{
			icon: Users,
			description: "Perfect for individuals",
			name: "Basic",
			price: "Free",
			variant: "outline",
			features: [
				"Automated Meeting Scheduling",
				"Basic Calendar Sync",
				"Daily Schedule Overview",
				"Email Reminders",
				"Task Management",
				"24/7 Customer Support",
				"Single User Access",
				"Basic Reporting",
				"Mobile App Access"
			]
		},
		{
			icon: Briefcase,
			description: "Ideal for small teams",
			name: "Pro",
			badge: "Popular",
			price: "$29",
			original: "$39",
			period: "/month",
			variant: "default",
			features: [
				"All Basic Plan Features",
				"Advanced Calendar Integrations",
				"Customizable Notifications",
				"Priority Support",
				"Analytics and Insights",
				"Group Scheduling",
				"Multiple User Roles",
				"Advanced Reporting",
				"Custom Branding Options"
			]
		},
		{
			icon: Building,
			name: "Enterprise",
			description: "Perfect for large scale companies",
			price: "$99",
			original: "$129",
			period: "/month",
			variant: "outline",
			features: [
				"All Pro Plan Features",
				"Dedicated Account Manager",
				"Custom Integrations",
				"Advanced Security Features",
				"Team Collaboration Tools",
				"Onboarding and Training",
				"Unlimited Users",
				"API Access with Higher Limits",
				"Advanced Audit Logs"
			]
		}
	];
</script>

<section class="w-full">
	<div class="mx-auto mb-4 max-w-md space-y-2">
		<div class="flex justify-center">
			<div class="rounded-md border px-4 py-1 text-sm">Pricing</div>
		</div>

		<h2
			class="text-center text-2xl font-bold tracking-tight md:text-3xl lg:text-4xl lg:font-extrabold"
		>
			Plans that Scale with You
		</h2>

		<p class="text-center text-sm text-muted-foreground md:text-base">
			Whether you&apos;re just starting out or growing fast, our flexible pricing has you
			covered.
		</p>
	</div>

	<div class="mx-auto grid w-full max-w-4xl gap-4 p-6 md:grid-cols-3">
		{#each plans as plan, index}
			<PricingCard
				class={cn("w-full max-w-full", index === 1 && "md:scale-105")}
				icon={plan.icon}
				name={plan.name}
				price={plan.price}
				description={plan.description}
				features={plan.features}
				period={plan.period}
				original={plan.original}
				badge={plan.badge}
				buttonVariant={plan.variant}
				featured={index === 1}
				href={plan.href}
			/>
		{/each}
	</div>
</section>

```

###### File: `src/lib/components/efferd/pricing/pricing-card.svelte`

```svelte
<script lang="ts">
	import { CheckCircle2, type Icon as IconType } from "@lucide/svelte";
	import { Button, type ButtonVariant } from "$lib/components/ui/button";
	import { cn } from "$lib/utils";
	import type { Component } from "svelte";
	import type { HTMLAttributes } from "svelte/elements";

	type PricingCardProps = HTMLAttributes<HTMLDivElement> & {
		icon: typeof IconType | Component;
		name: string;
		price: string;
		description: string;
		features: string[];
		period?: string;
		original?: string;
		badge?: string;
		href?: string;
		featured?: boolean;
		buttonLabel?: string;
		buttonVariant?: Extract<ButtonVariant, "default" | "outline">;
	};

	let {
		icon,
		name,
		price,
		description,
		features,
		period,
		original,
		badge,
		href = "/",
		featured = false,
		buttonLabel = "Get Started",
		buttonVariant = "outline",
		class: className,
		...restProps
	}: PricingCardProps = $props();
	let Icon = $derived(icon);
</script>

<div
	class={cn("relative w-full max-w-xs rounded-xl border bg-background p-1", className)}
	{...restProps}
>
	<!-- {@const Icon = icon} -->

	<div class={cn("relative mb-4 rounded-xl border p-4", featured && "bg-card shadow-xs")}>
		<div class="mb-8 flex items-center justify-between">
			<div
				class="flex items-center gap-2 text-sm font-medium [&_svg:not([class*='size-'])]:size-4"
			>
				<Icon />
				<span>{name}</span>
			</div>

			{#if badge}
				<span class="rounded-full border bg-background px-3 py-1 text-xs shadow-xs"
					>{badge}</span
				>
			{/if}
		</div>

		<div class="mb-3 flex items-end gap-1">
			<span class="text-3xl font-extrabold tracking-tight">{price}</span>

			{#if period}
				<span class="pb-1 text-sm text-muted-foreground">{period}</span>
			{/if}

			{#if original}
				<span class="ml-auto text-lg text-muted-foreground line-through">{original}</span>
			{/if}
		</div>

		<Button class="w-full font-semibold" {href} variant={buttonVariant}>
			{buttonLabel}
		</Button>
	</div>

	<div class="space-y-6 p-3">
		<p class="text-xs text-muted-foreground">{description}</p>

		<ul class="space-y-3">
			{#each features as feature}
				<li class="flex items-start gap-3 text-xs text-muted-foreground">
					<CheckCircle2 aria-hidden="true" class="size-4 text-foreground" />
					<span>{feature}</span>
				</li>
			{/each}
		</ul>
	</div>
</div>

```

---

### Pricing 4 <a name="pricing-4"></a>

A premium pricing 4 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `pricing-4`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/pricing-4.json
```

##### Dependencies

- **NPM Packages**:
  - `motion`
- **Registry Dependencies**:
  - `button`
  - `badge`

##### Component Source Code

###### File: `registry/blocks/pricing/4/pricing-section.tsx`

```tsx
"use client";
import { PlusIcon, ShieldCheckIcon } from "lucide-react";
import { motion } from "motion/react";
import { Badge } from "@/components/ui/badge";
import { Button } from "@/components/ui/button";

export function PricingSection() {
  return (
    <section className="mx-auto w-full max-w-5xl space-y-5" id="pricing">
      <motion.div
        className="mx-auto max-w-xl space-y-5"
        initial={{ opacity: 0, y: 20 }}
        transition={{ duration: 0.8, delay: 0.1, ease: [0.16, 1, 0.3, 1] }}
        viewport={{ once: true }}
        whileInView={{ opacity: 1, y: 0 }}
      >
        <div className="flex justify-center">
          <div className="rounded-md border px-4 py-1 text-sm">Pricing</div>
        </div>
        <h2 className="mt-5 text-center font-bold text-2xl tracking-tight md:text-3xl lg:text-4xl">
          Pricing Based on Your Success
        </h2>
        <p className="mt-5 text-center text-muted-foreground text-sm md:text-base">
          We offer a single price for all our services. We believe that pricing
          is a critical component of any successful business.
        </p>
      </motion.div>

      <motion.div
        className="mx-auto w-full max-w-2xl space-y-2"
        initial={{ opacity: 0, y: 20 }}
        transition={{ duration: 0.8, delay: 0.2, ease: [0.16, 1, 0.3, 1] }}
        viewport={{ once: true }}
        whileInView={{ opacity: 1, y: 0 }}
      >
        <div className="relative grid border bg-background p-4 md:grid-cols-2">
          <PlusIcon
            className="-top-[10.5px] -left-[10.5px] absolute size-5 text-foreground"
            strokeWidth={1}
          />
          <PlusIcon
            className="-top-[10.5px] -right-[10.5px] absolute size-5 text-foreground"
            strokeWidth={1}
          />
          <PlusIcon
            className="-bottom-[10.5px] -left-[10.5px] absolute size-5 text-foreground"
            strokeWidth={1}
          />
          <PlusIcon
            className="-right-[10.5px] -bottom-[10.5px] absolute size-5 text-foreground"
            strokeWidth={1}
          />

          <div className="w-full px-4 pt-5 pb-4">
            <div className="space-y-1">
              <div className="flex items-center justify-between">
                <h3 className="font-semibold leading-none">Monthly</h3>
                <div className="flex items-center gap-x-1">
                  <span className="text-muted-foreground text-sm line-through">
                    $8.99
                  </span>
                  <Badge variant="secondary">11% off</Badge>
                </div>
              </div>
              <p className="text-muted-foreground text-sm">
                Best value for growing businesses!
              </p>
            </div>
            <div className="mt-10 space-y-4">
              <div className="flex items-end gap-0.5 text-muted-foreground text-xl">
                <span>$</span>
                <span className="-mb-0.5 font-extrabold text-4xl text-foreground tracking-tighter md:text-4xl">
                  7.99
                </span>
                <span>/month</span>
              </div>
              <Button asChild className="w-full" variant="outline">
                <a href="#">Start Your Journey</a>
              </Button>
            </div>
          </div>
          <div className="relative w-full rounded-md border bg-card p-4 shadow dark:bg-card/80">
            <div className="space-y-1">
              <div className="flex items-center justify-between">
                <h3 className="font-semibold leading-none">Yearly</h3>
                <div className="flex items-center gap-x-1">
                  <span className="text-muted-foreground text-sm line-through">
                    $8.99
                  </span>
                  <Badge>22% off</Badge>
                </div>
              </div>
              <p className="text-muted-foreground text-sm">
                Unlock savings with an annual commitment!
              </p>
            </div>
            <div className="mt-10 space-y-4">
              <div className="flex items-end text-muted-foreground text-xl">
                <span>$</span>
                <span className="-mb-0.5 font-extrabold text-4xl text-foreground tracking-tighter md:text-4xl">
                  6.99
                </span>
                <span>/month</span>
              </div>
              <Button asChild className="w-full">
                <a href="#">Get Started Now</a>
              </Button>
            </div>
          </div>
        </div>

        <div className="flex items-center justify-center gap-x-2 text-muted-foreground text-sm">
          <ShieldCheckIcon className="size-4" />
          <span>Access to all features with no hidden fees</span>
        </div>
      </motion.div>
    </section>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `pricing-four`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/pricing-four.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
  - `@number-flow/svelte`
  - `motion-sv`
- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `src/lib/components/efferd/pricing/pricing-four.svelte`

```svelte
<script lang="ts">
	import NumberFlow from "@number-flow/svelte";
	import { CircleCheck, Star } from "@lucide/svelte";
	import { AnimatePresence, createLayoutMotion } from "motion-sv";
	import FrequencyToggle from "$lib/components/efferd/pricing/frequency-toggle.svelte";
	import { Button } from "$lib/components/ui/button";
	import { cn } from "$lib/utils";
	import type { FREQUENCY } from "./frequency-toggle.svelte";

	type Plan = {
		name: string;
		info: string;
		price: {
			monthly: number;
			yearly: number;
		};
		features: string[];
		btn: {
			text: string;
			href: string;
		};
		highlighted?: boolean;
	};

	type PlanBadge =
		| {
				id: "popular";
				label: string;
				variant: "neutral";
		  }
		| {
				id: "discount";
				label: string;
				variant: "primary";
		  };

	const plans: Plan[] = [
		{
			name: "Basic",
			info: "For most individuals",
			price: {
				monthly: 7,
				yearly: 6
			},
			features: [
				"Up to 3 Blog posts",
				"Up to 3 Transcriptions",
				"Up to 3 Posts stored",
				"Markdown support",
				"Community support",
				"AI powered suggestions"
			],
			btn: {
				text: "Start Your Free Trial",
				href: "/"
			}
		},
		{
			highlighted: true,
			name: "Pro",
			info: "For small businesses",
			price: {
				monthly: 17,
				yearly: 14
			},
			features: [
				"Up to 500 Blog Posts",
				"Up to 500 Transcriptions",
				"Up to 500 Posts stored",
				"Unlimited Markdown support",
				"SEO optimization tools",
				"Priority support",
				"AI powered suggestions"
			],
			btn: {
				text: "Get started",
				href: "/"
			}
		},
		{
			name: "Business",
			info: "For large organizations",
			price: {
				monthly: 49,
				yearly: 40
			},
			features: [
				"Unlimited Blog Posts",
				"Unlimited Transcriptions",
				"Unlimited Posts stored",
				"Unlimited Markdown support",
				"SEO optimization tools",
				"Priority support",
				"AI powered suggestions"
			],
			btn: {
				text: "Contact team",
				href: "/"
			}
		}
	];

	const layout = createLayoutMotion();

	let frequency = $state<FREQUENCY>("monthly");

	const setFrequency = layout.update.with((nextFrequency: FREQUENCY) => {
		frequency = nextFrequency;
	});

	function getDiscount(plan: Plan) {
		if (plan.price.monthly <= plan.price.yearly) {
			return 0;
		}

		return Math.round(((plan.price.monthly - plan.price.yearly) / plan.price.monthly) * 100);
	}

	function getBadges(plan: Plan, activeFrequency: FREQUENCY): PlanBadge[] {
		const badges: PlanBadge[] = [];

		if (plan.highlighted) {
			badges.push({
				id: "popular",
				label: "Popular",
				variant: "neutral"
			});
		}

		if (activeFrequency === "yearly") {
			const discount = getDiscount(plan);

			if (discount > 0) {
				badges.push({
					id: "discount",
					label: `${discount}% off`,
					variant: "primary"
				});
			}
		}

		return badges;
	}
</script>

<section class="flex w-full flex-col items-center justify-center space-y-7 p-4">
	<div class="mx-auto max-w-xl space-y-2">
		<h2
			class="text-center text-2xl font-bold tracking-tight md:text-3xl lg:text-4xl lg:font-extrabold"
		>
			Plans that Scale with You
		</h2>

		<p class="text-center text-sm text-muted-foreground md:text-base">
			Whether you&apos;re just starting out or growing fast, our flexible pricing has you
			covered with no hidden costs.
		</p>
	</div>

	<FrequencyToggle {frequency} {setFrequency} />

	<div class="mx-auto grid w-full max-w-4xl grid-cols-1 gap-6 md:grid-cols-3">
		{#each plans as plan (plan.name)}
			<div
				class={cn(
					"relative flex w-full flex-col overflow-hidden rounded-lg border shadow-xs",
					plan.highlighted && "scale-105"
				)}
			>
				<div class={cn("border-b p-4", plan.highlighted && "bg-card dark:bg-card/80")}>
					<layout.div
						class="absolute top-2 right-2 z-10 flex items-center gap-2"
						layout
						layoutDependency={frequency}
					>
						<AnimatePresence initial={false}>
							{#each getBadges(plan, frequency) as badge (badge.id)}
								<layout.div
									animate={{ opacity: 1, scale: 1, y: 0 }}
									class={cn(
										"flex items-center gap-1 rounded-md border px-2 py-0.5 text-xs",
										badge.variant === "primary"
											? "bg-primary text-primary-foreground"
											: "bg-background"
									)}
									exit={{ opacity: 0, scale: 0.95, y: -4 }}
									initial={{ opacity: 0, scale: 0.95, y: -4 }}
									layout
									transition={{ duration: badge.id === "popular" ? 0.1 : 0.15 }}
								>
									{#if badge.id === "popular"}
										<Star class="size-3 fill-current" />
									{/if}
									{badge.label}
								</layout.div>
							{/each}
						</AnimatePresence>
					</layout.div>

					<div class="text-lg font-medium">{plan.name}</div>
					<p class="text-sm font-normal text-muted-foreground">{plan.info}</p>

					<h3 class="mt-6 mb-1 flex w-max items-end gap-1">
						<!-- tabular-nums text-3xl font-extrabold  -->
						<NumberFlow
							class="text-3xl font-extrabold [&::part(suffix)]:text-base [&::part(suffix)]:font-normal [&::part(suffix)]:text-muted-foreground"
							format={{
								style: "currency",
								currency: "USD",
								notation: "compact"
							}}
							suffix="/month"
							value={plan.price[frequency]}
						/>
					</h3>

					<p class="mb-2 text-xs font-normal text-muted-foreground">billed {frequency}</p>
				</div>

				<div
					class={cn(
						"space-y-3 px-4 pt-6 pb-8 text-sm text-muted-foreground",
						plan.highlighted && "bg-muted/10"
					)}
				>
					{#each plan.features as feature}
						<div class="flex items-center gap-2">
							<CircleCheck class="size-3.5 text-foreground" />
							<p>{feature}</p>
						</div>
					{/each}
				</div>

				<div
					class={cn(
						"mt-auto w-full border-t p-3",
						plan.highlighted && "bg-card dark:bg-card/80"
					)}
				>
					<Button
						class="w-full"
						href={plan.btn.href}
						variant={plan.highlighted ? "default" : "outline"}
					>
						{plan.btn.text}
					</Button>
				</div>
			</div>
		{/each}
	</div>
</section>

```

###### File: `src/lib/components/efferd/pricing/frequency-toggle.svelte`

```svelte
<script lang="ts" module>
	export type FREQUENCY = "monthly" | "yearly";
</script>

<script lang="ts">
	import { cn } from "$lib/utils";
	import { createLayoutMotion } from "motion-sv";
	import type { HTMLAttributes } from "svelte/elements";

	type FREQUENCY = "monthly" | "yearly";

	type FrequencyToggleProps = HTMLAttributes<HTMLDivElement> & {
		frequency: FREQUENCY;
		setFrequency: (frequency: FREQUENCY) => void;
		frequencies?: FREQUENCY[];
	};

	const layout = createLayoutMotion();

	let {
		frequency,
		setFrequency,
		frequencies = ["monthly", "yearly"],
		class: className,
		...restProps
	}: FrequencyToggleProps = $props();

	const selectFrequency = layout.update.with((nextFrequency: FREQUENCY) => {
		if (nextFrequency !== frequency) {
			setFrequency(nextFrequency);
		}
	});
</script>

<div
	class={cn("mx-auto flex w-fit rounded-xl border bg-card p-1 shadow-xs", className)}
	{...restProps}
>
	{#each frequencies as freq}
		<button
			aria-pressed={frequency === freq}
			class="relative px-4 py-1 text-sm capitalize"
			onclick={() => selectFrequency(freq)}
			type="button"
		>
			<span class="relative z-10">{freq}</span>

			{#if frequency === freq}
				<layout.span
					class="absolute inset-0 z-10 rounded-xl bg-background mix-blend-difference dark:bg-foreground"
					layoutId="frequency-toggle"
					transition={{ type: "spring", duration: 0.4 }}
				/>
			{/if}
		</button>
	{/each}
</div>

```

---

## Testimonial Blocks <a name="testimonial"></a>

### Testimonial 1 <a name="testimonial-1"></a>

A premium testimonial 1 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `testimonials-1`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/testimonials-1.json
```

##### Dependencies

- **NPM Packages**:
  - `motion`

##### Component Source Code

###### File: `registry/blocks/testimonials/1/testimonials-columns.tsx`

```tsx
"use client";
import { motion } from "motion/react";
import React from "react";

export type Testimonial = {
  text: string;
  image: string;
  name: string;
  role: string;
};

export const TestimonialsColumn = (props: {
  className?: string;
  testimonials: Testimonial[];
  duration?: number;
}) => (
  <div className={props.className}>
    <motion.div
      animate={{
        translateY: "-50%",
      }}
      className="flex flex-col gap-6 pb-6"
      transition={{
        duration: props.duration || 10,
        repeat: Number.POSITIVE_INFINITY,
        ease: "linear",
        repeatType: "loop",
      }}
    >
      {[
        ...new Array(2).fill(0).map((_, index) => (
          <React.Fragment key={`column-${index}`}>
            {props.testimonials.map(({ text, image, name, role }) => (
              <div
                className="w-full max-w-xs rounded-3xl border bg-card p-8 shadow-lg dark:bg-card/20 dark:shadow-foreground/10"
                key={name}
              >
                <div>{text}</div>
                <div className="mt-5 flex items-center gap-2">
                  <img
                    alt={name}
                    className="h-10 w-10 rounded-full"
                    height={40}
                    src={image}
                    width={40}
                  />
                  <div className="flex flex-col">
                    <div className="font-medium leading-5 tracking-tight">
                      {name}
                    </div>
                    <div className="leading-5 tracking-tight opacity-60">
                      {role}
                    </div>
                  </div>
                </div>
              </div>
            ))}
          </React.Fragment>
        )),
      ]}
    </motion.div>
  </div>
);

```

#### Svelte Implementation

- **Registry Key**: `testimonial-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/testimonial-one.json
```

##### Dependencies

- **Registry Dependencies**:
  - `avatar`

##### Component Source Code

###### File: `src/lib/components/efferd/testimonial/testimonial-one.svelte`

```svelte
<script lang="ts">
	import { Avatar, AvatarFallback, AvatarImage } from "$lib/components/ui/avatar";
</script>

<figure class="mx-auto flex w-full max-w-lg flex-col items-center justify-center">
	<div class="mb-8 flex items-center gap-1">
		<!-- <VercelIcon aria-hidden="true" class="size-6" /> -->
		<svg preserveAspectRatio="xMidYMid" viewBox="0 0 256 222" aria-hidden="true" class="size-6">
			<path d="m128 0 128 221.705H0z" fill="currentColor" />
		</svg>
		<span class="text-lg font-medium">Vercel</span>
	</div>

	<blockquote class="text-center text-xl leading-tight tracking-tight sm:text-2xl md:text-3xl">
		&quot;<span class="font-medium">Svelte Efferd</span> is tooo good.&quot;
	</blockquote>

	<div
		class="mx-auto my-5 h-px w-full max-w-sm bg-border mask-[linear-gradient(to_right,transparent,black,transparent)]"
	></div>

	<figcaption class="flex flex-col items-center gap-5">
		<div class="space-y-0.5 text-center">
			<cite class="text-xl font-medium text-foreground not-italic"> Guillermo Rauch </cite>
			<div class="text-lg text-muted-foreground">CEO, Vercel</div>
		</div>

		<Avatar class="size-12 rounded-full border object-cover">
			<AvatarImage
				alt="Guillermo Rauch's profile picture"
				src="https://github.com/rauchg.png"
			/>
			<AvatarFallback>GR</AvatarFallback>
		</Avatar>
	</figcaption>
</figure>

```

---

### Testimonial 2 <a name="testimonial-2"></a>

A premium testimonial 2 block component for modern landing pages and interfaces.

#### React Implementation

- **Registry Key**: `testimonials-2`

##### Installation
```bash
npx shadcn@latest add https://efferd.com/r/testimonials-2.json
```

##### Dependencies

- **NPM Packages**:
  - `motion`
- **Registry Dependencies**:
  - `https://magicui.design/r/grid-pattern.json`

##### Component Source Code

###### File: `registry/blocks/testimonials/2/testimonials-section.tsx`

```tsx
"use client";
import { motion } from "motion/react";

// https://magicui.design/docs/components/grid-pattern
import { GridPattern } from "@/components/ui/grid-pattern";

type Testimonial = {
  name: string;
  role: string;
  image: string;
  company: string;
  quote: string;
};

const testimonials: Testimonial[] = [
  {
    quote:
      "Multi Techno transformed the way we manage our operations. Their ERP system is reliable, scalable, and truly easy to use.",
    name: "Ali Khan",
    role: "HR Manager",
    company: "Pak Mission Society",
    image: "https://randomuser.me/api/portraits/men/21.jpg",
  },
  {
    quote:
      "Their ERP platform streamlined our business processes. What impressed me most is their dedication to client success and support.",
    name: "Sara Ahmed",
    role: "CEO",
    company: "Galaxy Five Home",
    image: "https://randomuser.me/api/portraits/women/22.jpg",
  },
  {
    quote:
      "They took time to understand our unique requirements and delivered a system that fits seamlessly into daily operations.",
    name: "Imran Hussain",
    role: "Manager",
    company: "Al-Tayyab Foods",
    image: "https://randomuser.me/api/portraits/men/23.jpg",
  },
  {
    quote:
      "From onboarding to ongoing support, the Multi Techno team has been responsive, professional, and incredibly easy to work with.",
    name: "Fatima Noor",
    role: "Director",
    company: "Shafiqe Foods",
    image: "https://randomuser.me/api/portraits/women/24.jpg",
  },
  {
    quote:
      "Their collaborative approach makes us feel like partners, not just clients. Every strategy session brings new value to our business.",
    name: "Usman Raza",
    role: "CTO",
    company: "NextGen Solutions",
    image: "https://randomuser.me/api/portraits/men/25.jpg",
  },
  {
    quote:
      "We rely on their ERP to manage critical operations. The platform is intuitive, and the support team is always proactive.",
    name: "Ayesha Siddiqui",
    role: "Product Lead",
    company: "Bright Future Tech",
    image: "https://randomuser.me/api/portraits/women/26.jpg",
  },
  {
    quote:
      "Multi Techno gave us better visibility across departments. The insights and efficiency gains have been game-changing for our company.",
    name: "Bilal Sheikh",
    role: "Operations Head",
    company: "Metro Logistics",
    image: "https://randomuser.me/api/portraits/men/27.jpg",
  },
  {
    quote:
      "The ERP system brought structure to our finance operations. It’s user-friendly and perfectly tailored to our organizational needs.",
    name: "Nadia Karim",
    role: "Finance Manager",
    company: "Alpha Traders",
    image: "https://randomuser.me/api/portraits/women/28.jpg",
  },
  {
    quote:
      "Dependable, efficient, and forward-thinking. Multi Techno has become a trusted partner in helping us scale confidently worldwide.",
    name: "Omar Farooq",
    role: "Managing Director",
    company: "VisionX Global",
    image: "https://randomuser.me/api/portraits/men/29.jpg",
  },
  {
    quote:
      "Their attention to detail and continuous improvements keep us ahead of the curve. Working with them feels effortless every time.",
    name: "Sana Iqbal",
    role: "Head of Strategy",
    company: "BlueWave Consulting",
    image: "https://randomuser.me/api/portraits/women/30.jpg",
  },
  {
    quote:
      "We’ve tested other ERPs, but nothing matched their level of customization and hands-on support. Highly recommend their services.",
    name: "Hamza Tariq",
    role: "Operations Manager",
    company: "Green Valley Farms",
    image: "https://randomuser.me/api/portraits/men/31.jpg",
  },
  {
    quote:
      "Multi Techno’s system made our business smarter, not harder. The partnership has been valuable for both efficiency and growth.",
    name: "Mehwish Zafar",
    role: "COO",
    company: "Skyline Apparel",
    image: "https://randomuser.me/api/portraits/women/32.jpg",
  },
];

export function TestimonialsSection() {
  return (
    <section className="relative w-full px-4 pt-10 pb-20">
      <div className="mx-auto max-w-5xl space-y-8">
        <div className="flex flex-col gap-2">
          <h1 className="text-balance font-semibold text-3xl tracking-wide md:text-4xl lg:text-5xl xl:font-bold">
            Real Results, Real Voices
          </h1>
          <p className="text-muted-foreground text-sm md:text-base lg:text-lg">
            See how businesses are thriving with our ERP — real stories, real
            impact, real growth.
          </p>
        </div>
        <div className="relative grid grid-cols-1 gap-2 sm:grid-cols-2 lg:grid-cols-3">
          {testimonials.map(({ name, role, company, quote, image }, index) => (
            <motion.div
              className="relative grid grid-cols-[auto_1fr] gap-x-3 overflow-hidden rounded-lg border bg-card p-4 shadow dark:bg-card/50"
              initial={{ filter: "blur(4px)", translateY: -5, opacity: 0 }}
              key={name}
              transition={{ delay: 0.05 * index, duration: 0.5 }}
              viewport={{ once: true }}
              whileInView={{
                filter: "blur(0px)",
                translateY: 0,
                opacity: 1,
              }}
            >
              <div className="-mt-2 -ml-20 pointer-events-none absolute top-0 left-1/2 size-full [mask-image:radial-gradient(farthest-side_at_top,white,transparent)]">
                <GridPattern
                  className="absolute inset-0 size-full stroke-border"
                  height={25}
                  width={25}
                  x={-12}
                  y={4}
                />
              </div>
              <img
                alt={name}
                className="rounded-full"
                height={36}
                loading="lazy"
                src={image}
                width={36}
              />
              <div>
                <div className="-mt-0.5 -space-y-0.5">
                  <p className="text-sm md:text-base">{name}</p>
                  <span className="block font-light text-[11px] text-muted-foreground tracking-tight">
                    {role} at {company}
                  </span>
                </div>
                <blockquote className="mt-3">
                  <p className="font-light text-foreground text-sm tracking-wide">
                    {quote}
                  </p>
                </blockquote>
              </div>
            </motion.div>
          ))}
        </div>
      </div>
    </section>
  );
}

```

#### Svelte Implementation

- **Registry Key**: `testimonial-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/testimonial-two.json
```

##### Dependencies

- **Registry Dependencies**:
  - `avatar`
  - `local:mask-line`

##### Component Source Code

###### File: `src/lib/components/efferd/testimonial/testimonial-two.svelte`

```svelte
<script>
	import { Avatar, AvatarFallback, AvatarImage } from "$lib/components/ui/avatar";
	import MaskLine from "$lib/components/ui/mask-line/mask-line.svelte";
</script>

<figure
	class="mx-auto flex w-full max-w-lg flex-col items-center justify-center md:grid md:grid-cols-[auto_1fr]"
>
	<div class="relative">
		<!-- Vertical lines  -->
		<MaskLine class="left-0" orientation="vertical" />
		<MaskLine class="right-0" orientation="vertical" />
		<!-- Horizontal lines  -->
		<MaskLine class="top-0 md:w-xl" orientation="horizontal" />
		<MaskLine class="bottom-0 md:w-xl" orientation="horizontal" />

		<Avatar
			class="size-24 rounded-none mask-[radial-gradient(circle,black_60%,transparent)] *:rounded-none md:size-32"
		>
			<AvatarImage alt="Shadcn's profile picture" src="https://github.com/shadcn.png" />
			<AvatarFallback>SH</AvatarFallback>
		</Avatar>
	</div>
	<figcaption class="space-y-4 p-8 text-center md:p-6 md:text-left">
		<blockquote class="text-lg leading-tight tracking-tight text-muted-foreground">
			&quot;<span class="font-medium text-foreground">Svelte Efferd</span> is so polished I might
			just retire. The ecosystem is in safe hands.&quot;
		</blockquote>

		<div>
			<cite class="text-xs font-medium text-foreground not-italic"> Shadcn </cite>
			<div class="text-[10px] text-muted-foreground">Founder, shadcn/ui</div>
		</div>
	</figcaption>
</figure>

```

---

### Testimonial 3 <a name="testimonial-3"></a>

A premium testimonial 3 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `testimonial-three`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/testimonial-three.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `avatar`
  - `local:decor-icon`

##### Component Source Code

###### File: `src/lib/components/efferd/testimonial/testimonial-three.svelte`

```svelte
<script lang="ts">
	import TestimonialCard from "./testimonial-card.svelte";

	type Testimonial = {
		quote: string;
		name: string;
		role: string;
		company: string;
		image: string;
	};
	let testimonials: Testimonial[] = [
		{
			quote: "We just acquired Efferd for 3 gazillion dollars. We're calling it iEfferd. It's our best product yet.",
			image: "https://unavatar.io/x/tim_cook",
			name: "Tim Cook",
			role: "CEO",
			company: "Apple"
		},
		{
			quote: "I'm considering shipping Efferd components with Prime delivery. 2-day shipping on beautiful UIs? Done.",
			image: "https://unavatar.io/x/JeffBezos",
			name: "Jeff Bezos",
			role: "Founder",
			company: "Amazon"
		},
		{
			quote: "We're rewriting OpenAI's entire frontend in Efferd. The AGI told us it's the only logical choice.",
			image: "https://unavatar.io/x/sama",
			name: "Sam Altman",
			role: "CEO",
			company: "OpenAI"
		}
	];
</script>

<div class="mx-auto -mt-10 grid w-full max-w-5xl gap-8 md:grid-cols-3 md:gap-6">
	{#each testimonials as testimonial, index}
		<TestimonialCard {index} {testimonial} />
	{/each}
</div>

```

###### File: `src/lib/components/efferd/testimonial/testimonial-card.svelte`

```svelte
<script lang="ts">
	import { Avatar, AvatarFallback, AvatarImage } from "$lib/components/ui/avatar";
	import { DecorIcon } from "$lib/components/ui/decor-icon";
	import { cn } from "$lib/utils";
	import { QuoteIcon } from "@lucide/svelte";
	import type { HTMLAttributes } from "svelte/elements";

	type Testimonial = {
		quote: string;
		name: string;
		role: string;
		company: string;
		image: string;
	};

	type Props = HTMLAttributes<HTMLElement> & {
		testimonial: Testimonial;
		class?: string;
		index?: number;
	};

	let { testimonial, class: className = "", index = 0, ...props }: Props = $props();
</script>

<figure
	class={cn(
		"relative flex flex-col justify-between gap-6 px-8 pt-8 pb-6 shadow-xs md:translate-y-[calc(3rem*var(--t-card-index))]",
		"dark:bg-[radial-gradient(50%_80%_at_25%_0%,--theme(--color-foreground/.1),transparent)]",
		className
	)}
	style={`--t-card-index: ${index}`}
	{...props}
>
	<div class="absolute -inset-y-4 -left-px w-px bg-border"></div>
	<div class="absolute -inset-y-4 -right-px w-px bg-border"></div>
	<div class="absolute -inset-x-4 -top-px h-px bg-border"></div>
	<div class="absolute -right-4 -bottom-px -left-4 h-px bg-border"></div>
	<DecorIcon class="size-3.5" position="top-left" />

	<blockquote class="flex gap-4">
		<QuoteIcon aria-hidden="true" class="size-6 shrink-0 stroke-1" />

		<p class="flex-1 text-base leading-relaxed font-normal text-muted-foreground">
			{testimonial.quote}
		</p>
	</blockquote>

	<figcaption class="flex items-center gap-3">
		<Avatar
			class="size-10 rounded-full ring-2 ring-border ring-offset-2 ring-offset-background transition-shadow group-hover:ring-foreground/20"
		>
			<AvatarImage alt={`${testimonial.name}'s profile picture`} src={testimonial.image} />
			<AvatarFallback>{testimonial.name.charAt(0)}</AvatarFallback>
		</Avatar>
		<div class="flex flex-col">
			<cite class="text-sm font-medium text-foreground not-italic">
				{testimonial.name}
			</cite>
			<p class="text-xs text-muted-foreground">
				{testimonial.role}, <span class="text-foreground/80">{testimonial.company}</span>
			</p>
		</div>
	</figcaption>
</figure>

```

---

### Testimonial 4 <a name="testimonial-4"></a>

A premium testimonial 4 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `testimonial-four`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/testimonial-four.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `local:full-width-divider`

##### Component Source Code

###### File: `src/lib/components/efferd/testimonial/testimonial-four/testimonial-four.svelte`

```svelte
<script lang="ts">
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import TestimonialCard from "./testimonial-card.svelte";

	type Testimonial = {
		quote: string;
		name: string;
		role: string;
		company?: string;
	};

	const testimonials: Testimonial[] = [
		{
			quote: "Efferd is so polished I might just retire and become a full-time potato farmer. The ecosystem is in safe hands.",
			name: "Shadcn",
			role: "Founder",
			company: "Shadcn UI"
		},
		{
			quote: "Efferd is why I still have hair. No more pulling it out over centering divs or fighting with CSS grid.",
			name: "Guillermo Rauch",
			role: "CEO",
			company: "Vercel"
		},

		{
			quote: "I tried to buy Efferd but they wouldn't sell. So I just bought Twitter instead to complain about it.",
			name: "Elon Musk",
			role: "CEO",
			company: "X.com"
		}
	];
</script>

<section class="relative mx-auto min-h-screen w-full max-w-4xl place-content-center border-x">
	<FullWidthDivider />
	<div class="grid md:grid-cols-[2fr_1px_1fr]">
		<div class="divide-y">
			{#each testimonials.slice(0, 2) as testimonial}
				<TestimonialCard {testimonial} />
			{/each}
		</div>
		<div class="h-px bg-border md:h-auto"></div>
		<div class="flex items-center">
			<TestimonialCard testimonial={testimonials[2] as Testimonial} />
		</div>
	</div>
	<FullWidthDivider />
</section>

```

###### File: `src/lib/components/efferd/testimonial/testimonial-four/testimonial-card.svelte`

```svelte
<script lang="ts">
	import { QuoteIcon } from "@lucide/svelte";

	type Testimonial = {
		quote: string;
		name: string;
		role: string;
		company?: string;
	};
	let {
		testimonial
	}: {
		testimonial: Testimonial;
	} = $props();
</script>

<figure class="p-6 md:p-8">
	<QuoteIcon aria-hidden="true" class="mb-4 size-12 stroke-1 text-muted-foreground" />

	<blockquote class="mb-6 text-base font-normal text-foreground md:text-lg">
		&quot;{testimonial.quote}&quot;
	</blockquote>

	<figcaption class="flex flex-col gap-0.5">
		<cite class="text-lg font-medium text-foreground not-italic">
			{testimonial.name}
		</cite>
		<p class="text-sm text-muted-foreground">
			{testimonial.role}
			{testimonial.company && `, ${testimonial.company}`}
		</p>
	</figcaption>
</figure>

```

---

### Testimonial 5 <a name="testimonial-5"></a>

A premium testimonial 5 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `testimonial-five`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/testimonial-five.json
```

##### Dependencies

- **Registry Dependencies**:
  - `avatar`
  - `local:full-width-divider`
  - `local:grid-filler`
  - `local:grid-pattern`

##### Component Source Code

###### File: `src/lib/components/efferd/testimonial/testimonial-five/testimonial-five.svelte`

```svelte
<script lang="ts">
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import { GridFiller } from "$lib/components/ui/grid-filler";
	import TestimonialCard from "./testimonial-card.svelte";

	type Testimonial = {
		name: string;
		role: string;
		image: string;
		company?: string;
		quote: string;
	};

	const testimonials: Testimonial[] = [
		{
			quote: "Efferd is so polished I might just retire and become a full-time potato farmer. The ecosystem is in safe hands.",
			image: "https://github.com/shadcn.png",
			name: "Shadcn",
			role: "Founder",
			company: "Shadcn UI"
		},
		{
			quote: "Efferd is why I still have hair. No more pulling it out over centering divs or fighting with CSS grid.",
			image: "https://github.com/rauchg.png",
			name: "Guillermo Rauch",
			role: "CEO",
			company: "Vercel"
		},
		{
			quote: "I tried to buy Efferd but they wouldn't sell. So I just bought Twitter instead to complain about it.",
			image: "https://unavatar.io/x/elonmusk",
			name: "Elon Musk",
			role: "CEO",
			company: "X.com"
		},
		{
			quote: "We just acquired Efferd for 3 gazillion dollars. We're calling it iEfferd. It's our best product yet.",
			image: "https://unavatar.io/x/tim_cook",
			name: "Tim Cook",
			role: "CEO",
			company: "Apple"
		},
		{
			quote: "I'm considering shipping Efferd components with Prime delivery. 2-day shipping on beautiful UIs? Done.",
			image: "https://unavatar.io/x/JeffBezos",
			name: "Jeff Bezos",
			role: "Founder",
			company: "Amazon"
		},
		{
			quote: "We're rewriting OpenAI's entire frontend in Efferd. The AGI told us it's the only logical choice.",
			image: "https://unavatar.io/x/sama",
			name: "Sam Altman",
			role: "CEO",
			company: "OpenAI"
		},
		{
			quote: "We processed 100 petabytes of data to find the perfect UI library. The algorithm returned 'Efferd' with 99.9% confidence.",
			image: "https://unavatar.io/x/sundarpichai",
			name: "Sundar Pichai",
			role: "CEO",
			company: "Google"
		},
		{
			quote: "Our links might 404 sometimes, but thanks to Efferd, at least the 404 page looks absolutely stunning.",
			image: "https://github.com/steven-tey.png",
			name: "Steven Tey",
			role: "Founder",
			company: "Dub.co"
		},
		{
			quote: "It's so fast, I finished my UI sprint before my next meeting even started. Open source for the win.",
			image: "https://unavatar.io/x/peer_rich",
			name: "Peer Richelsen",
			role: "Co-Founder",
			company: "Cal.com"
		},
		{
			quote: "21st.dev brings in 100k users daily just to see Efferd. We got into YC solely because of this UI library. And yes, we're rich now.",
			image: "https://github.com/serafimcloud.png",
			name: "Serafim",
			role: "Founder",
			company: "21st Labs."
		},
		{
			quote: "I posted a video on Efferd components and it got more views than my cat video. That's statistically impossible.",
			image: "https://github.com/TheOrcDev.png",
			name: "OrcDev",
			role: "Youtuber"
		}
	];
</script>

<div class="mx-auto min-h-screen max-w-5xl space-y-8 overflow-x-clip border-x py-6">
	<div class="flex flex-col gap-2 px-4 md:px-6">
		<h1 class="text-3xl font-semibold tracking-wide text-balance md:text-4xl xl:font-bold">
			Real Results, Real Voices
		</h1>
		<p class="text-sm text-muted-foreground md:text-base lg:text-lg">
			Used by thousands of developers to build beautiful, accessible, and performant web
			applications.
		</p>
	</div>
	<div class="relative grid grid-cols-1 gap-px bg-border sm:grid-cols-2 lg:grid-cols-3">
		<FullWidthDivider position="top" />
		{#each testimonials as testimonial}
			<TestimonialCard {testimonial} />
		{/each}

		<GridFiller
			class="bg-background"
			lgColumns={3}
			smColumns={2}
			totalItems={testimonials.length}
		/>
		<FullWidthDivider position="bottom" />
	</div>
</div>

```

###### File: `src/lib/components/efferd/testimonial/testimonial-five/testimonial-card.svelte`

```svelte
<script lang="ts">
	import { Avatar, AvatarFallback, AvatarImage } from "$lib/components/ui/avatar";
	import { GridPattern } from "$lib/components/ui/grid-pattern";
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";
	type Testimonial = {
		name: string;
		role: string;
		image: string;
		company?: string;
		quote: string;
	};

	type Props = HTMLAttributes<HTMLElement> & {
		testimonial: Testimonial;
		class?: string;
	};
	let { testimonial, class: className = "", ...props }: Props = $props();
</script>

<div
	class={cn(
		"relative grid grid-cols-[auto_1fr] gap-x-3 overflow-hidden bg-background p-4",
		className
	)}
	{...props}
>
	<div
		class="pointer-events-none absolute top-0 left-1/2 -mt-2 -ml-20 size-full mask-[radial-gradient(farthest-side_at_top,white,transparent)]"
	>
		<GridPattern
			class="absolute inset-0 size-full stroke-border"
			height={25}
			width={25}
			x={-12}
			y={4}
		/>
	</div>

	<Avatar class="size-8 rounded-full">
		<AvatarImage alt={`${testimonial.name}'s profile picture`} src={testimonial.image} />
		<AvatarFallback>{testimonial.name.charAt(0)}</AvatarFallback>
	</Avatar>
	<figure>
		<figcaption class="-mt-0.5 -space-y-0.5">
			<cite class="text-sm not-italic md:text-base">{testimonial.name}</cite>
			<span class="block text-[11px] font-light tracking-tight text-muted-foreground">
				{testimonial.role}
				{testimonial.company && `, ${testimonial.company}`}
			</span>
		</figcaption>
		<blockquote class="mt-3">
			<p class="text-sm tracking-wide text-foreground/80">{testimonial.quote}</p>
		</blockquote>
	</figure>
</div>

```

---

## Integration Blocks <a name="integration"></a>

### Integration 1 <a name="integration-1"></a>

A premium integration 1 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `integration-one`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/integration-one.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `src/lib/components/efferd/integrations/integration-one.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { cn } from "$lib/utils";
	import { ArrowUpRightIcon } from "@lucide/svelte";

	type Integration = {
		src: string;
		name: string;
		description: string;
		isInvertable?: boolean;
	};

	const data: Integration[] = [
		{
			src: "https://storage.efferd.com/logo/vercel.svg",
			name: "Vercel",
			description: "Amet praesentium deserunt ex commodi tempore fuga....",
			isInvertable: true
		},
		{
			src: "https://storage.efferd.com/logo/openai.svg",
			name: "OpenAI",
			description: "Amet praesentium deserunt ex commodi tempore fuga....",
			isInvertable: true
		},
		{
			src: "https://storage.efferd.com/logo/supabase.svg",
			name: "Supabase",
			description: "Amet praesentium deserunt ex commodi tempore fuga...."
		},
		{
			src: "https://storage.efferd.com/logo/notion.svg",
			name: "Notion",
			description: "Amet praesentium deserunt ex commodi tempore fuga...."
		}
	];
</script>

<div
	class={cn(
		"mx-auto grid max-w-5xl gap-1 overflow-hidden rounded-md bg-secondary p-1 sm:grid-cols-2 lg:grid-cols-4 dark:bg-secondary/50"
	)}
>
	{#each data as item (item.name)}
		<div
			class={cn(
				"group relative flex flex-col justify-between gap-2 rounded-md bg-background p-6 shadow-sm"
			)}
		>
			<img
				alt={item.name}
				class={cn(
					"pointer-events-none size-8 shrink-0 object-contain select-none",
					item.isInvertable && "dark:invert"
				)}
				height="32"
				src={item.src}
				width="32"
			/>

			<div class="space-y-1">
				<h3 class="font-semibold">{item.name}</h3>
				<p class="text-xs text-muted-foreground md:text-sm">
					{item.description}
				</p>
			</div>
		</div>
	{/each}

	<div class="relative flex items-center justify-center p-1 sm:col-span-2 lg:col-span-4">
		<Button href="/" class="group text-xs" size="sm" variant="link">
			View all integrations
			<ArrowUpRightIcon data-icon="inline-end" />
		</Button>
	</div>
</div>

```

---

### Integration 2 <a name="integration-2"></a>

A premium integration 2 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `integration-two`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/integration-two.json
```

##### Dependencies

- **Registry Dependencies**:
  - `local:decor-icon`

##### Component Source Code

###### File: `src/lib/components/efferd/integrations/integration-two.svelte`

```svelte
<script lang="ts">
	import { DecorIcon } from "$lib/components/ui/decor-icon";
	import IntegrationCardTwo from "./integration-card-two.svelte";

	type DecorPosition = "top-left" | "top-right" | "bottom-left" | "bottom-right";

	type Integration = {
		src: string;
		name: string;
		description: string;
		isInvertable?: boolean;
		decorPosition?: DecorPosition;
	};

	const data: Integration[] = [
		{
			src: "https://storage.efferd.com/logo/vercel.svg",
			name: "Vercel",
			description: "Amet praesentium deserunt ex commodi tempore fuga voluptatem....",
			isInvertable: true
		},
		{
			src: "https://storage.efferd.com/logo/openai.svg",
			name: "OpenAI",
			description: "Amet praesentium deserunt ex commodi tempore fuga voluptatem....",
			isInvertable: true,
			decorPosition: "bottom-left"
		},
		{
			src: "https://storage.efferd.com/logo/supabase.svg",
			name: "Supabase",
			description: "Amet praesentium deserunt ex commodi tempore fuga voluptatem...."
		},
		{
			src: "https://storage.efferd.com/logo/github.svg",
			name: "GitHub",
			description: "Amet praesentium deserunt ex commodi tempore fuga voluptatem....",
			isInvertable: true
		},
		{
			src: "https://storage.efferd.com/logo/notion.svg",
			name: "Notion",
			description: "Amet praesentium deserunt ex commodi tempore fuga voluptatem...."
		},
		{
			src: "https://storage.efferd.com/logo/gmail.svg",
			name: "Gmail",
			description: "Amet praesentium deserunt ex commodi tempore fuga voluptatem....",
			decorPosition: "top-left"
		}
	];
</script>

<div class="relative mx-auto max-w-5xl border">
	<div class="grid grid-cols-2 gap-px bg-border md:grid-cols-3">
		{#each data as item (item.name)}
			<IntegrationCardTwo integration={item} />
		{/each}
	</div>

	<DecorIcon position="top-left" />
	<DecorIcon position="top-right" />
	<DecorIcon position="bottom-left" />
	<DecorIcon position="bottom-right" />
</div>

```

###### File: `src/lib/components/efferd/integrations/integration-card-two.svelte`

```svelte
<script lang="ts">
	import { DecorIcon } from "$lib/components/ui/decor-icon";
	import { cn } from "$lib/utils";
	import type { HTMLAttributes } from "svelte/elements";

	type DecorPosition = "top-left" | "top-right" | "bottom-left" | "bottom-right";

	type Integration = {
		src: string;
		name: string;
		description: string;
		isInvertable?: boolean;
		decorPosition?: DecorPosition;
	};

	type Props = HTMLAttributes<HTMLDivElement> & {
		integration: Integration;
	};

	let { integration, class: className, ...props }: Props = $props();
</script>

<div
	class={cn(
		"relative flex flex-col items-start gap-4 bg-background p-4 text-start md:p-6 md:even:bg-background/75",
		className
	)}
	{...props}
>
	<img
		alt={integration.name}
		class={cn(
			"pointer-events-none size-8 shrink-0 object-contain select-none",
			integration.isInvertable && "dark:invert"
		)}
		height="32"
		src={integration.src}
		width="32"
	/>

	<div class="space-y-1">
		<h3 class="font-semibold">{integration.name}</h3>
		<p class="text-xs text-muted-foreground md:text-sm">
			{integration.description}
		</p>
	</div>

	{#if integration.decorPosition}
		<DecorIcon position={integration.decorPosition} />
	{/if}
</div>

```

---

### Integration 3 <a name="integration-3"></a>

A premium integration 3 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `integration-three`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/integration-three.json
```

##### Dependencies

- **Registry Dependencies**:
  - `button`
  - `local:full-width-divider`

##### Component Source Code

###### File: `src/lib/components/efferd/integrations/integration-three.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { FullWidthDivider } from "$lib/components/ui/full-width-divider";
	import { cn } from "$lib/utils";
	import IntegrationCardThree from "./integration-card-three.svelte";

	type LogoType = {
		src: string;
		alt: string;
	};

	type TileData = {
		row: number;
		col: number;
		logo?: LogoType;
	};

	const tiles: TileData[] = [
		{
			row: 0,
			col: 1,
			logo: {
				src: "https://storage.efferd.com/logo/vercel.svg",
				alt: "Vercel Logo"
			}
		},
		{
			row: 0,
			col: 3,
			logo: {
				src: "https://storage.efferd.com/logo/openai.svg",
				alt: "OpenAI Logo"
			}
		},
		{ row: 1, col: 0 },
		{
			row: 1,
			col: 2,
			logo: {
				src: "https://storage.efferd.com/logo/cursor.svg",
				alt: "Cursor Logo"
			}
		},
		{
			row: 1,
			col: 4,
			logo: {
				src: "https://storage.efferd.com/logo/v0.svg",
				alt: "V0 Logo"
			}
		},
		{
			row: 2,
			col: 1,
			logo: {
				src: "https://storage.efferd.com/logo/planetscale.svg",
				alt: "Planetscale Logo"
			}
		},
		{ row: 2, col: 3 },
		{ row: 3, col: 0 },
		{
			row: 3,
			col: 2,
			logo: {
				src: "https://storage.efferd.com/logo/base-ui.svg",
				alt: "Base UI Logo"
			}
		},
		{
			row: 3,
			col: 4,
			logo: {
				src: "https://storage.efferd.com/logo/copilot.svg",
				alt: "Copilot Logo"
			}
		},
		{
			row: 4,
			col: 1,
			logo: {
				src: "https://storage.efferd.com/logo/github.svg",
				alt: "GitHub Logo"
			}
		},
		{
			row: 4,
			col: 3,
			logo: {
				src: "https://storage.efferd.com/logo/dub.svg",
				alt: "Dub Logo"
			}
		}
	];
</script>

<div
	class="relative mx-auto grid max-w-4xl grid-cols-1 gap-12 border-x md:grid-cols-2 md:items-center"
>
	<FullWidthDivider class="-top-px" />

	<div class="p-4 md:p-6">
		<div class="space-y-4">
			<h2 class="text-3xl font-medium tracking-tight text-foreground sm:text-4xl">
				Connect with your favorite tools
			</h2>
			<p class="text-sm text-muted-foreground md:text-base">
				Connect your favorite tools with our growing library of integrations.
			</p>
			<Button size="sm" href="/">Explore integrations</Button>
		</div>
	</div>

	<div class="place-items-end">
		<div class="relative size-80">
			<div
				class={cn(
					"absolute inset-0 size-full",
					"bg-[linear-gradient(to_right,theme(--color-border)_1px,transparent_1px),linear-gradient(to_bottom,theme(--color-border)_1px,transparent_1px)]",
					"bg-size-[64px_64px]",
					"mask-[radial-gradient(ellipse_at_center,black,black,transparent)]"
				)}
			></div>

			{#each tiles as tile (`${tile.row}_${tile.col}`)}
				<IntegrationCardThree {...tile} />
			{/each}
		</div>
	</div>

	<FullWidthDivider class="-bottom-px" />
</div>

```

###### File: `src/lib/components/efferd/integrations/integration-card-three.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";

	type Logo = {
		src: string;
		alt: string;
	};

	type Props = {
		row: number;
		col: number;
		logo?: Logo;
	};

	let { row, col, logo }: Props = $props();
</script>

<div
	class={cn("absolute flex size-16 items-center justify-center", logo && "bg-secondary/40")}
	style:left={`${col * 64}px`}
	style:top={`${row * 64}px`}
>
	{#if logo}
		<img
			alt={logo.alt}
			class="pointer-events-none size-8 object-contain p-1 select-none dark:invert"
			height="40"
			src={logo.src}
			width="40"
		/>
	{/if}
</div>

```

---

### Integration 4 <a name="integration-4"></a>

A premium integration 4 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `integration-four`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/integration-four.json
```

##### Component Source Code

###### File: `src/lib/components/efferd/integrations/integration-four.svelte`

```svelte
<script lang="ts">
	import IntegrationCardFour from "./integration-card-four.svelte";

	type LogoType = {
		src: string;
		alt: string;
		isInvertable?: boolean;
	};

	type TileData = {
		row: number;
		col: number;
		logo?: LogoType;
	};

	const tiles: TileData[] = [
		{
			row: 0,
			col: 1
		},
		{
			row: 0,
			col: 3,
			logo: {
				src: "https://storage.efferd.com/logo/notion.svg",
				alt: "Notion Logo"
			}
		},
		{ row: 1, col: 0 },
		{
			row: 1,
			col: 2,
			logo: {
				src: "https://storage.efferd.com/logo/cursor.svg",
				alt: "Cursor Logo",
				isInvertable: true
			}
		},
		{
			row: 1,
			col: 4,
			logo: {
				src: "https://storage.efferd.com/logo/vercel.svg",
				alt: "Vercel Logo",
				isInvertable: true
			}
		},
		{
			row: 2,
			col: 1,
			logo: {
				src: "https://storage.efferd.com/logo/planetscale.svg",
				alt: "Planetscale Logo",
				isInvertable: true
			}
		},
		{
			row: 2,
			col: 3,
			logo: {
				src: "https://storage.efferd.com/logo/gmail.svg",
				alt: "Gmail Logo"
			}
		},
		{ row: 3, col: 0 },
		{
			row: 3,
			col: 2,
			logo: {
				src: "https://storage.efferd.com/logo/supabase.svg",
				alt: "Supabase Logo"
			}
		},
		{
			row: 3,
			col: 4,
			logo: {
				src: "https://storage.efferd.com/logo/canva.svg",
				alt: "Canva Logo"
			}
		},
		{
			row: 4,
			col: 1,
			logo: {
				src: "https://storage.efferd.com/logo/adobe.svg",
				alt: "Adobe Logo"
			}
		},
		{
			row: 4,
			col: 3,
			logo: {
				src: "https://storage.efferd.com/logo/polar.svg",
				alt: "Polar Logo"
			}
		}
	];
</script>

<div class="mx-auto grid max-w-5xl grid-cols-1 gap-12 p-4 md:grid-cols-2 md:items-center">
	<div class="max-w-xl space-y-5">
		<h2 class="text-3xl font-medium tracking-tight text-foreground sm:text-4xl md:text-5xl">
			Seamless Integration
		</h2>
		<p class="text-lg leading-8 text-muted-foreground">
			Integrate with over 100+ tools and platforms to streamline your workflow and boost
			productivity.
		</p>
	</div>

	<div class="place-items-end">
		<div
			class="relative size-90 mask-[radial-gradient(ellipse_at_center,black,black,transparent)]"
		>
			{#each tiles as tile (`${tile.row}_${tile.col}`)}
				<IntegrationCardFour {...tile} />
			{/each}
		</div>
	</div>
</div>

```

###### File: `src/lib/components/efferd/integrations/integration-card-four.svelte`

```svelte
<script lang="ts">
	import { cn } from "$lib/utils";

	type LogoType = {
		src: string;
		alt: string;
		isInvertable?: boolean;
	};

	type Props = {
		row: number;
		col: number;
		logo?: LogoType;
	};

	let { row, col, logo }: Props = $props();
</script>

<div
	class={cn(
		"absolute flex size-18 items-center justify-center rounded-md border",
		logo ? "bg-card shadow-xs dark:bg-card/60" : "bg-secondary/30 dark:bg-background"
	)}
	style:left={`${col * 72}px`}
	style:top={`${row * 72}px`}
>
	{#if logo}
		<img
			alt={logo.alt}
			class={cn(
				"pointer-events-none size-8 object-contain p-1 select-none",
				logo.isInvertable && "dark:invert"
			)}
			height="40"
			src={logo.src}
			width="40"
		/>
	{/if}
</div>

```

---

### Integration 5 <a name="integration-5"></a>

A premium integration 5 block component for modern landing pages and interfaces.

#### React Implementation

> [!NOTE]
> React version is not available in the open-source repository.

#### Svelte Implementation

- **Registry Key**: `integration-five`

##### Installation
```bash
pnpm dlx shadcn-svelte@latest add https://sv-efferd.pages.dev/r/integration-five.json
```

##### Dependencies

- **NPM Packages**:
  - `@lucide/svelte`
- **Registry Dependencies**:
  - `button`

##### Component Source Code

###### File: `src/lib/components/efferd/integrations/integration-five.svelte`

```svelte
<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { cn } from "$lib/utils";
	import { ArrowUpRightIcon } from "@lucide/svelte";

	type Integration = {
		src?: string;
		name: string;
		isInvertable?: boolean;
	};

	const data: Integration[] = [
		{
			name: "Empty 1"
		},
		{
			name: "Vercel",
			src: "https://storage.efferd.com/logo/vercel.svg",
			isInvertable: true
		},
		{
			name: "OpenAI",
			src: "https://storage.efferd.com/logo/openai.svg",
			isInvertable: true
		},
		{
			src: "https://storage.efferd.com/logo/supabase.svg",
			name: "Supabase"
		},
		{
			name: "GitHub",
			src: "https://storage.efferd.com/logo/github.svg",
			isInvertable: true
		},
		{
			name: "Notion",
			src: "https://storage.efferd.com/logo/notion.svg"
		},
		{
			name: "Gmail",
			src: "https://storage.efferd.com/logo/gmail.svg"
		},
		{
			name: "Google Maps",
			src: "https://storage.efferd.com/logo/google-maps.svg"
		},
		{
			name: "Neon",
			src: "https://storage.efferd.com/logo/neon.svg"
		},
		{
			name: "Empty 2"
		}
	];
</script>

<div class="flex flex-col items-center justify-center gap-6 py-24">
	<div class="max-w-xl space-y-2 px-4 text-center">
		<h2 class="text-4xl font-semibold tracking-tight">All types of integration</h2>
		<p class="text-base text-muted-foreground md:text-lg">
			Connect your favourite apps and services easily
		</p>
	</div>

	<div class="flex flex-col justify-center rounded-full border bg-secondary dark:bg-secondary/10">
		<div class="flex items-center justify-center -space-x-4 mask-r-from-90 mask-l-from-90 p-1">
			{#each data as item (item.name)}
				<div
					class={cn(
						"relative z-0 transition-transform",
						item.src && "hover:z-10 hover:scale-110"
					)}
				>
					<div
						class="flex size-12 items-center justify-center overflow-hidden rounded-full border bg-card shadow-sm md:size-16"
					>
						{#if item.src}
							<img
								alt={item.name}
								class={cn(
									"pointer-events-auto size-5 object-contain select-none md:size-6",
									item.isInvertable && "dark:invert"
								)}
								height="24"
								src={item.src}
								width="24"
							/>
						{/if}
					</div>
				</div>
			{/each}
		</div>
	</div>

	<Button href="/" class="rounded-full px-5">
		See all integrations
		<ArrowUpRightIcon data-icon="inline-end" />
	</Button>
</div>

```

---
