# EvilCharts Creative Developer Reference Guide

A complete, structured developer reference guide for **EvilCharts** — beautiful, handcrafted chart components built with Next.js, shadcn/ui, Recharts, and Framer Motion.

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
- [Chart Config](#chart-config)
- [Components](#components)
- [Area Chart](#area-chart)
- [Bar Chart](#bar-chart)
- [Bar Blocks](#bar-blocks)
- [Composed Chart](#composed-chart)
- [Line Chart](#line-chart)
- [Pie Chart](#pie-chart)
- [Radar Chart](#radar-chart)
- [Radial Chart](#radial-chart)
- [Sankey Chart](#sankey-chart)
- [Background](#background)
- [Legend](#legend)
- [Tooltip](#tooltip)

---

## Introduction <a name="introduction"></a>

> EvilCharts is an open-source chart UI website built with shadcn and Recharts, beautifully designed and handcrafted.

Wait... It's not another chart library. While searching for chart components with smooth animations and dynamic data visualizations, I realized there weren't many options available. So, I decided to build my own. Built with [Recharts](https://recharts.github.io/) and [Shadcn](https://shadcn.com/ui).

## The Problem

EvilCharts is built by [Gurbinder](https://x.com/legionsdev) (Design Engineer @ Axiom.co). As a design engineer, I see ugly and common charts everywhere, and no one in the community is creating a beautiful chart library.

There are tons of chart libraries out there, but none of them are beautiful — even though they allow customization, I see that no one is customizing them and they just use them as provided via docs.

So, here I'm building a beautiful charts component website, open source, open code, it's all yours...

## Why I named it EvilCharts?

"Evil" doesn't mean malicious. It means:

- Unapologetically opinionated UI choices 
- Design-first, animated, unique designs
- Handcrafted with SVGs and [Motion](https://motion.dev) to stand out

## Built on Recharts

While finalising the design for EvilCharts, I wanted a good headless chart library. I went over tons of chart libraries and found that Recharts has amazing features and is easy to use in every project.

---

## Installation <a name="installation"></a>

> how to install evilcharts & use it in your project

EvilCharts is designed to be a plug-and-play solution for beautiful, interactive charts in your Next.js projects. You don't need any complex setup or high-end configuration—just follow these simple steps to get started with stunning data visualizations in minutes.

## Steps

  
    #### Install Recharts

    
      
        Install Recharts, the charting library that powers EvilCharts. This is a required dependency for all chart components.
        
        Learn more at the [Recharts installation guide](https://recharts.github.io/en-US/guide/installation).
      
      ```bash
npm install recharts
```
    
  
  
  
    #### Setup shadcn/ui

    
      
        Initialize shadcn/ui in your project. This sets up Tailwind CSS, theme configuration, and component structure. Skip this step if you already have shadcn/ui installed.
        
        Check out the [shadcn/ui Next.js installation docs](https://ui.shadcn.com/docs/installation/next) for more details.
      
      ```bash
npx shadcn@latest add init
```
    
  
  
  
    #### Add EvilCharts Components

    
      
        Add chart components to your project using the CLI. Replace `{chart-name}` with your desired chart type (e.g., `area-chart`, `bar-chart`, `line-chart`, `pie-chart`). The CLI will install all necessary files and dependencies.
      
      ```bash
npx shadcn@latest add @evilcharts/{chart-name}
```

---

## Chart Config <a name="chart-config"></a>

> Define labels, colors, and icons for each data series in your chart

The `chartConfig` is a central configuration object that every Evil Chart component accepts. It maps each data key in your dataset to its display metadata — **labels**, **colors** (with theme support), and optional **icons** that appear in tooltips and legends.

## Structure

```tsx
import { type ChartConfig } from "@/registry/ui/chart";

const chartConfig = {
  desktop: {
    label: "Desktop",
    icon: MonitorIcon,
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    icon: SmartphoneIcon,
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;
```

Each key in the config (e.g. `desktop`, `mobile`) must match a data key in your dataset. The config object is typed as:

```tsx
type ChartConfig = Record<
  string,
  {
    label?: React.ReactNode;
    icon?: React.ComponentType;
    colors?: {
      light?: string[];
      dark?: string[];
    };
  }
>;
```

## Properties

### label

A human-readable name displayed in tooltips and legends. Can be a string or any `React.ReactNode`.

```tsx
const chartConfig = {
  desktop: {
    label: "Desktop Users", // [!code highlight]
    // ...
  },
} satisfies ChartConfig;
```

### colors

Theme-aware color arrays. You must provide at least one theme key (`light` or `dark`). Each value is an array of CSS color strings.

**Single color** — one color per theme for a solid fill:

```tsx
colors: {
  light: ["#047857"],
  dark: ["#10b981"],
}
```

**Multiple colors** — pass an array to create gradient fills across bars, areas, and other chart elements. Colors are evenly distributed across the series:

```tsx
colors: {
  light: ["#a855f7", "#6366f1", "#3b82f6"],
  dark: ["#f43f5e", "#ec4899", "#a855f7", "#6366f1", "#3b82f6"],
}
```

You can even define different color counts per theme — the chart intelligently distributes them using the max count across all themes.

### icon

An optional React component that replaces the default color indicator in both the **tooltip** and **legend**. This is useful when you want to visually distinguish series beyond just colors.

```tsx
import { Monitor, Smartphone } from "lucide-react";

const chartConfig = {
  desktop: {
    label: "Desktop",
    icon: Monitor, // [!code highlight] [!code word:Monitor]
    colors: { light: ["#047857"], dark: ["#10b981"] },
  },
  mobile: {
    label: "Mobile",
    icon: Smartphone, // [!code highlight] [!code word:Smartphone]
    colors: { light: ["#be123c"], dark: ["#f43f5e"] },
  },
} satisfies ChartConfig;
```

When an `icon` is provided, it renders in place of the color dot/square in tooltips and the color indicator in legends. The icon is styled with `text-muted-foreground` and sized to `h-2.5 w-2.5` in tooltips and `h-3 w-3` in legends.

## How Colors Work

Under the hood, the chart config generates CSS custom properties scoped to each chart instance. For a config key `desktop` with colors `["#a855f7", "#6366f1"]`, the following CSS variables are created:

```css
--color-desktop-0: #a855f7;
--color-desktop-1: #6366f1;
```

These variables are used by chart components, tooltips, and legends to apply consistent theming. When you switch between light and dark mode, the correct set of variables is automatically applied.

### Color Distribution

When fewer colors are provided than chart segments need, colors are **evenly distributed** across slots:

- 2 colors for 4 slots: `[red, red, pink, pink]`
- 3 colors for 4 slots: `[red, pink, blue, blue]`

This means you can provide just 2-3 gradient stops and they'll smoothly span any number of data points.

## Runtime Validation

The chart config is validated at runtime. If you pass an empty `colors` object or one without a valid theme key, you'll get a clear error:

```plaintext
[EvilCharts] Invalid chart config for "desktop": colors object must
have at least one theme key (light, dark). Received empty object or
invalid keys.
```

## Examples

### Default (Labels + Colors)

A basic chart config with labels and theme-aware colors. The label appears in the tooltip and legend, while the colors control the chart fill.

#### Live Example Code

##### Example File: `ex-chart-config-default-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop", // [!code highlight]
    colors: {
      light: ["#047857"], // [!code highlight]
      dark: ["#10b981"], // [!code highlight]
    },
  },
  mobile: {
    label: "Mobile", // [!code highlight]
    colors: {
      light: ["#be123c"], // [!code highlight]
      dark: ["#f43f5e"], // [!code highlight]
    },
  },
} satisfies ChartConfig;

export function EvilExampleChartConfigDefaultBarChart() {
  return (
    <EvilBarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value: string) => value.substring(0, 3)} />
      <Legend />
      <Tooltip defaultIndex={4} />
      <Bar dataKey="desktop" variant="default" />
      <Bar dataKey="mobile" variant="default" />
    </EvilBarChart>
  );
}
```

### With Icons

Pass an `icon` component to each config entry. The icon replaces the default color indicator in both the tooltip and the legend.

#### Live Example Code

##### Example File: `ex-chart-config-icons-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";
import { Monitor, Smartphone } from "lucide-react";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    icon: Monitor, // [!code highlight]
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    icon: Smartphone, // [!code highlight]
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleChartConfigIconsBarChart() {
  return (
    <EvilBarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value: string) => value.substring(0, 3)} />
      <Legend />
      <Tooltip defaultIndex={4} />
      <Bar dataKey="desktop" variant="default" />
      <Bar dataKey="mobile" variant="default" />
    </EvilBarChart>
  );
}
```

### Gradient Colors

Pass multiple colors per theme to create gradient fills. Each color in the array represents a gradient stop that is distributed across the chart elements.

#### Live Example Code

##### Example File: `ex-gradient-colors-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#a855f7", "#6366f1", "#3b82f6"], // [!code highlight]
      dark: ["#f43f5e", "#ec4899", "#a855f7", "#6366f1", "#3b82f6"], // [!code highlight]
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#10b981", "#34d399", "#6ee7b7"], // [!code highlight]
      dark: ["#10b981", "#14b8a6", "#06b6d4"], // [!code highlight]
    },
  },
} satisfies ChartConfig;

export function EvilExampleBarChart() {
  return (
    <EvilBarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Bar dataKey="desktop" variant="default" isClickable />
      <Bar dataKey="mobile" variant="default" isClickable />
    </EvilBarChart>
  );
}
```

## API Reference

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **label** | `React.ReactNode` | - | A human-readable name for the data series. Displayed in tooltips and legends. |
| **colors** | `{ light?: string[]; dark?: string[] }` | - | Theme-aware color configuration. Must include at least one theme key (`light` or `dark`). Each key maps to an array of CSS color strings. A single color gives a solid fill; multiple colors create a gradient. |
| **icon** | `React.ComponentType` | - | An optional React component rendered in place of the default color indicator in tooltips and legends. |

---

## Components <a name="components"></a>

> List of all components provided by EvilCharts.

## Charts

<ShowcaseGrid />

---

## Area Chart <a name="area-chart"></a>

> Simple static & beautifully designed area charts

#### Live Example Code

##### Example File: `ex-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  Grid,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
      showBrush
      xDataKey="month"
      brushFormatLabel={(value) => String(value).substring(0, 3)}
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Area dataKey="desktop" variant="gradient" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Area>
      <Area dataKey="mobile" variant="gradient" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Area>
    </EvilAreaChart>
  );
}
```

## Installation


  
    
    
  
  
### CLI Installation


    ```bash
npx shadcn@latest add @evilcharts/area-chart
```
  
  
### Manual Installation


    
      
        #### Install the following dependencies:

        
          ```bash
npm install recharts
npm install motion
```
        
      
      
        #### Copy and paste the following code snippets into your project.

         
          To use the chart, first create the folder `evilcharts` and a subfolder called `charts` inside your `components` directory.
          Then, copy the following base area-chart code into a new file in that folder.
        
        
          #### Component Implementation

##### Source File: `area-chart.tsx`

```tsx
"use client";

import {
  Children,
  createContext,
  isValidElement,
  use,
  useCallback,
  useId,
  useMemo,
  useRef,
  useState,
  type ComponentProps,
  type FC,
  type ReactElement,
  type ReactNode,
} from "react";
import {
  axisValueToPercentFormatter,
  type ChartConfig,
  ChartContainer,
  getColorsCount,
  getLoadingData,
  LoadingIndicator,
} from "@/registry/ui/chart";
import {
  Area as RechartsArea,
  AreaChart as RechartsAreaChart,
  CartesianGrid,
  XAxis as RechartsXAxis,
  YAxis as RechartsYAxis,
} from "recharts";
import {
  ChartTooltip,
  ChartTooltipContent,
  type TooltipRoundness,
  type TooltipVariant,
} from "@/registry/ui/tooltip";
import { ChartLegend, ChartLegendContent, type ChartLegendVariant } from "@/registry/ui/legend";
import { EvilBrush, useEvilBrush, type EvilBrushRange } from "@/registry/ui/evil-brush";
import { ChartDot, type DotVariant } from "@/registry/ui/dot";
import { motion, useReducedMotion } from "motion/react";

// Constants
const STROKE_WIDTH = 0.8;
const LOADING_AREA_DATA_KEY = "loading";
const LOADING_ANIMATION_DURATION = 2000; // in milliseconds
const STACK_ID = "evil-stacked";
const REVEAL_DURATION = 1; // intro wipe length, in seconds
const REVEAL_EASE: [number, number, number, number] = [0, 0.7, 0.5, 1]; // intro wipe easing

type CurveType = ComponentProps<typeof RechartsArea>["type"];
type AreaDotProp = ComponentProps<typeof RechartsArea>["dot"];
type AreaActiveDotProp = ComponentProps<typeof RechartsArea>["activeDot"];
type AreaVariant = "gradient" | "gradient-reverse" | "solid" | "dotted" | "lines" | "hatched";
type StrokeVariant = "solid" | "dashed" | "animated-dashed";
type StackType = "default" | "expanded" | "stacked";

/**
 * Direction of the custom motion.dev intro reveal. Recharts' own area animation
 * is permanently disabled (it drew the line after the dots had already popped
 * in) — these reveals replace it.
 *
 * NOTE: a reveal is a per-frame animated SVG mask, so it is heavier than a
 * static chart. `"none"` opts out entirely; it is also what a device with the
 * OS "reduce motion" preference falls back to automatically.
 */
type AreaAnimationType = "none" | "left-to-right" | "right-to-left" | "center-out" | "edges-in";
type RevealAnimationType = Exclude<AreaAnimationType, "none">;

// ─────────────────────────────────────────────────────────────────────────────
// Shared context
// ─────────────────────────────────────────────────────────────────────────────

/**
 * Shared state for every part of the chart. Lifted into <EvilAreaChart /> so that
 * <Area />, <XAxis />, <Legend />, and friends can read it without prop drilling.
 * Sub-components are composed freely — the provider is the single source of truth.
 */
type AreaChartContextValue = {
  config: ChartConfig; // colors + labels for every series
  curveType: CurveType; // default curve interpolation each <Area /> inherits
  animationType: AreaAnimationType; // default intro reveal each <Area /> inherits
  isStacked: boolean; // whether areas stack on top of each other
  isExpanded: boolean; // whether the stack is normalized to 100%
  isLoading: boolean; // whether the chart shows its loading skeleton
  selectedDataKey: string | null; // currently selected series, or null when none
  selectDataKey: (dataKey: string | null) => void; // sets the selected series
};

const AreaChartContext = createContext<AreaChartContextValue | null>(null);

// Reads the chart context, throwing a helpful error when used outside <EvilAreaChart />
function useAreaChart() {
  const context = use(AreaChartContext);

  if (!context) {
    throw new Error(
      "Area chart parts (<Area />, <XAxis />, …) must be used within <EvilAreaChart />",
    );
  }

  return context;
}

// ─────────────────────────────────────────────────────────────────────────────
// Root container
// ─────────────────────────────────────────────────────────────────────────────

// Validates that every config key also exists on the data row type
type ValidateConfigKeys<TData, TConfig> = {
  [K in keyof TConfig]: K extends keyof TData ? ChartConfig[string] : never;
};

type EvilAreaChartBaseProps<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
> = {
  config: TConfig & ValidateConfigKeys<TData, TConfig>; // series colors + labels
  data: TData[]; // rows rendered by the chart
  children: ReactNode; // composed parts — <Area />, <XAxis />, <Legend />, …
  className?: string; // extra classes for the chart container
  chartProps?: ComponentProps<typeof RechartsAreaChart>; // escape hatch for the raw Recharts chart
  curveType?: CurveType; // default curve interpolation for every <Area />
  animationType?: AreaAnimationType; // default intro reveal for every <Area />
  stackType?: StackType; // how multiple areas combine
  defaultSelectedDataKey?: string | null; // series selected on first render
  onSelectionChange?: (selectedDataKey: string | null) => void; // fires when the selected series changes
  isLoading?: boolean; // shows the animated loading skeleton
  loadingPoints?: number; // number of points in the loading skeleton
  showBrush?: boolean; // renders a zoom brush below the chart
  xDataKey?: keyof TData & string; // x-axis key — only needed for the brush footer
  brushHeight?: number; // height of the brush preview in pixels
  brushFormatLabel?: (value: unknown, index: number) => string; // formats brush axis labels
  onBrushChange?: (range: EvilBrushRange) => void; // fires when the brush range changes
};

type EvilAreaChartProps<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
> = EvilAreaChartBaseProps<TData, TConfig>;

/**
 * Root of the composible area chart. Owns the data, the shared context, the
 * loading skeleton, and the optional zoom brush. Everything visual — axes,
 * grid, tooltip, legend, and the areas themselves — is composed as children,
 * so a consumer renders exactly the parts they need.
 */
export function EvilAreaChart<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
>({
  config,
  data,
  children,
  className,
  chartProps,
  curveType = "linear",
  animationType = "left-to-right",
  stackType = "default",
  defaultSelectedDataKey = null,
  onSelectionChange,
  isLoading = false,
  loadingPoints,
  showBrush = false,
  xDataKey,
  brushHeight,
  brushFormatLabel,
  onBrushChange,
}: EvilAreaChartProps<TData, TConfig>) {
  const chartId = useId().replace(/:/g, ""); // colon-free id keeps CSS/SVG selectors valid
  const [selectedDataKey, setSelectedDataKey] = useState<string | null>(defaultSelectedDataKey);
  const { loadingData, onShimmerExit } = useLoadingData(isLoading, loadingPoints);
  const { visibleData, brushProps } = useEvilBrush({ data });

  const isExpanded = stackType === "expanded";
  const isStacked = stackType === "stacked" || isExpanded;
  const displayData = showBrush && !isLoading ? visibleData : data;

  // Updates selection state and notifies the parent
  const selectDataKey = useCallback(
    (newSelectedDataKey: string | null) => {
      setSelectedDataKey(newSelectedDataKey);
      onSelectionChange?.(newSelectedDataKey);
    },
    [onSelectionChange],
  );

  const contextValue = useMemo<AreaChartContextValue>(
    () => ({
      config,
      curveType,
      animationType,
      isStacked,
      isExpanded,
      isLoading,
      selectedDataKey,
      selectDataKey,
    }),
    [
      config,
      curveType,
      animationType,
      isStacked,
      isExpanded,
      isLoading,
      selectedDataKey,
      selectDataKey,
    ],
  );

  return (
    <AreaChartContext value={contextValue}>
      <ChartContainer
        className={className}
        config={config}
        footer={
          showBrush &&
          !isLoading && (
            <EvilBrush
              data={data}
              chartConfig={config}
              xDataKey={xDataKey}
              variant="area"
              curveType={curveType}
              height={brushHeight}
              formatLabel={brushFormatLabel}
              stacked={isStacked}
              skipStyle
              className="mt-1"
              {...brushProps}
              onChange={(range) => {
                brushProps.onChange(range);
                onBrushChange?.(range);
              }}
            />
          )
        }
      >
        <LoadingIndicator isLoading={isLoading} />
        <RechartsAreaChart
          id={chartId}
          accessibilityLayer
          stackOffset={isExpanded ? "expand" : undefined}
          data={isLoading ? loadingData : displayData}
          {...chartProps}
        >
          {children}
          {isLoading && (
            <LoadingArea chartId={chartId} curveType={curveType} onShimmerExit={onShimmerExit} />
          )}
        </RechartsAreaChart>
      </ChartContainer>
    </AreaChartContext>
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Composible parts
// ─────────────────────────────────────────────────────────────────────────────

type AreaProps = {
  dataKey: string; // series key — must exist on the data and config
  variant?: AreaVariant; // fill style for this area only
  strokeVariant?: StrokeVariant; // stroke style for this area
  curveType?: CurveType; // curve interpolation — falls back to the chart default
  animationType?: AreaAnimationType; // intro reveal — falls back to the chart default
  connectNulls?: boolean; // join segments across null/missing values
  isClickable?: boolean; // lets this area be selected by clicking it
  children?: ReactNode; // optional <Dot /> and <ActiveDot /> composition
  areaProps?: ComponentProps<typeof RechartsArea>; // escape hatch for raw Recharts Area props
};

/**
 * A single area series. Each <Area /> is fully self-contained: it generates its
 * own gradient/pattern definitions under a unique id, so any number of areas —
 * each with its own variant, stroke, and clickability — can live in one chart
 * without style collisions. Compose <Dot /> and <ActiveDot /> inside it to add
 * point markers.
 */
export function Area({
  dataKey,
  variant = "gradient",
  strokeVariant = "dashed",
  curveType,
  animationType,
  connectNulls = false,
  isClickable = false,
  children,
  areaProps,
}: AreaProps) {
  const {
    config,
    curveType: defaultCurve,
    animationType: defaultAnimation,
    isStacked,
    isExpanded,
    isLoading,
    selectedDataKey,
    selectDataKey,
  } = useAreaChart();
  const id = useId().replace(/:/g, ""); // unique id scopes this area's style defs
  // Devices set to "reduce motion" skip the intro reveal entirely
  const shouldReduceMotion = useReducedMotion();

  // The root renders the skeleton area while loading, so real areas step aside
  if (isLoading) return null;

  const resolvedCurve = curveType ?? defaultCurve;

  // The reveal is an animated SVG mask — heavier than a static chart — so
  // `"none"` and the OS reduce-motion preference both opt out of it.
  const revealType: AreaAnimationType = shouldReduceMotion
    ? "none"
    : (animationType ?? defaultAnimation);
  const maskId = revealType === "none" ? undefined : `${id}-reveal-mask`;

  const isSelected = selectedDataKey === dataKey;
  const hasSelection = selectedDataKey !== null;
  const opacity = getOpacity(selectedDataKey, dataKey);
  const showUnselected = hasSelection && !isSelected;

  const { dot, activeDot } = resolveDots(children, id, dataKey, opacity.dot, maskId);

  const isAnimatedDashed = strokeVariant === "animated-dashed";
  const isDashed = strokeVariant === "dashed" || isAnimatedDashed;

  return (
    <>
      <RechartsArea
        type={resolvedCurve}
        dataKey={dataKey}
        connectNulls={connectNulls}
        fillOpacity={opacity.fill}
        strokeOpacity={opacity.stroke}
        fill={getFillPattern(variant, showUnselected, id)}
        stroke={`url(#${id}-colors-${dataKey})`}
        stackId={isStacked ? STACK_ID : undefined}
        dot={dot}
        activeDot={activeDot}
        strokeWidth={STROKE_WIDTH}
        strokeDasharray={isDashed ? "3 3" : undefined}
        // Recharts' built-in area animation is permanently disabled — it drew
        // the line after the dots had already popped in. The motion.dev reveal
        // mask drives the intro instead, wiping fill, stroke, and dots in together.
        isAnimationActive={false}
        style={{
          ...(maskId ? { mask: `url(#${maskId})` } : {}),
          ...(isClickable ? { cursor: "pointer" } : {}),
        }}
        onClick={() => {
          if (!isClickable) return;
          // Clicking the selected area clears the selection, otherwise selects it
          selectDataKey(isSelected ? null : dataKey);
        }}
        {...areaProps}
      >
        {isAnimatedDashed && !hasSelection && <AnimatedDashedStroke />}
      </RechartsArea>
      <defs>
        {revealType !== "none" && <RevealMask id={id} type={revealType} />}
        <ColorGradient id={id} dataKey={dataKey} config={config} isExpanded={isExpanded} />
        {variant === "gradient" && <GradientPattern id={id} dataKey={dataKey} />}
        {variant === "gradient-reverse" && <ReverseGradientPattern id={id} dataKey={dataKey} />}
        {variant === "solid" && <SolidPattern id={id} dataKey={dataKey} />}
        {variant === "dotted" && <DottedPattern id={id} dataKey={dataKey} />}
        {variant === "lines" && <LinesPattern id={id} dataKey={dataKey} />}
        {variant === "hatched" && <HatchedPattern id={id} dataKey={dataKey} />}
        {showUnselected && <UnselectedPattern id={id} dataKey={dataKey} />}
      </defs>
    </>
  );
}

type DotProps = {
  variant?: DotVariant; // visual style of the point marker
};

/**
 * Declares a resting point marker for the <Area /> it is composed inside.
 * It renders nothing on its own — the parent <Area /> reads its variant and
 * wires it into the Recharts dot slot.
 */
export const Dot: FC<DotProps> = () => null;

/**
 * Declares the hovered/active point marker for the <Area /> it is composed
 * inside. Like <Dot />, it is a configuration slot and renders nothing itself.
 */
export const ActiveDot: FC<DotProps> = () => null;

type XAxisProps = ComponentProps<typeof RechartsXAxis>;

/**
 * The horizontal category axis. Ships with the chart's flat default styling and
 * forwards every Recharts XAxis prop, so `dataKey`, `tickFormatter`, etc. are
 * passed straight through. Hidden automatically while the chart is loading.
 */
export function XAxis({
  tickLine = false,
  axisLine = false,
  tickMargin = 8,
  minTickGap = 8,
  ...props
}: XAxisProps) {
  const { isLoading } = useAreaChart();

  if (isLoading) return null;

  return (
    <RechartsXAxis
      tickLine={tickLine}
      axisLine={axisLine}
      tickMargin={tickMargin}
      minTickGap={minTickGap}
      {...props}
    />
  );
}

type YAxisProps = ComponentProps<typeof RechartsYAxis>;

/**
 * The vertical value axis. Forwards every Recharts YAxis prop and, when the
 * chart uses an expanded stack, formats ticks as percentages automatically.
 * Hidden automatically while the chart is loading.
 */
export function YAxis({
  tickLine = false,
  axisLine = false,
  tickMargin = 8,
  minTickGap = 8,
  width = "auto",
  tickFormatter,
  ...props
}: YAxisProps) {
  const { isLoading, isExpanded } = useAreaChart();

  if (isLoading) return null;

  return (
    <RechartsYAxis
      tickLine={tickLine}
      axisLine={axisLine}
      tickMargin={tickMargin}
      minTickGap={minTickGap}
      width={width}
      tickFormatter={isExpanded ? axisValueToPercentFormatter : tickFormatter}
      {...props}
    />
  );
}

type GridProps = ComponentProps<typeof CartesianGrid>;

/**
 * The background grid lines. Defaults to horizontal-only dashed lines and
 * forwards every Recharts CartesianGrid prop for full control.
 */
export function Grid({ vertical = false, strokeDasharray = "3 3", ...props }: GridProps) {
  return <CartesianGrid vertical={vertical} strokeDasharray={strokeDasharray} {...props} />;
}

type TooltipProps = {
  variant?: TooltipVariant; // visual style of the tooltip surface
  roundness?: TooltipRoundness; // border-radius of the tooltip
  defaultIndex?: number; // data index shown by default with no hover
  cursor?: boolean; // whether the vertical cursor line follows the pointer
};

/**
 * The hover tooltip. Reads the chart's selection from context so its content
 * dims unselected series. Hidden automatically while the chart is loading.
 */
export function Tooltip({ variant, roundness, defaultIndex, cursor = true }: TooltipProps) {
  const { isLoading, selectedDataKey } = useAreaChart();

  if (isLoading) return null;

  return (
    <ChartTooltip
      defaultIndex={defaultIndex}
      cursor={cursor ? { strokeDasharray: "3 3", strokeWidth: STROKE_WIDTH } : false}
      content={
        <ChartTooltipContent selected={selectedDataKey} roundness={roundness} variant={variant} />
      }
    />
  );
}

type LegendProps = {
  variant?: ChartLegendVariant; // visual style of the legend indicators
  align?: "left" | "center" | "right"; // horizontal placement
  verticalAlign?: "top" | "middle" | "bottom"; // vertical placement
  isClickable?: boolean; // lets each entry toggle selection of its series
};

/**
 * The series legend. When `isClickable` is set, each entry toggles selection of
 * its series, driving the shared selection state read by every <Area />.
 */
export function Legend({
  variant,
  align = "right",
  verticalAlign = "top",
  isClickable = false,
}: LegendProps) {
  const { selectedDataKey, selectDataKey } = useAreaChart();

  return (
    <ChartLegend
      verticalAlign={verticalAlign}
      align={align}
      content={
        <ChartLegendContent
          selected={selectedDataKey}
          onSelectChange={selectDataKey}
          isClickable={isClickable}
          variant={variant}
        />
      }
    />
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Selection + dot helpers
// ─────────────────────────────────────────────────────────────────────────────

// Returns fill/stroke/dot opacity — dims a series only when another is selected
const getOpacity = (selectedDataKey: string | null, dataKey: string) => {
  if (selectedDataKey === null) {
    return { fill: 0.8, stroke: 0.8, dot: 1 };
  }

  return selectedDataKey === dataKey
    ? { fill: 0.8, stroke: 0.8, dot: 1 }
    : { fill: 0.2, stroke: 0.3, dot: 0.3 };
};

// Resolves the SVG paint reference for an area's fill based on its variant
const getFillPattern = (variant: AreaVariant, showUnselected: boolean, id: string): string => {
  // A non-selected area in a clickable chart is striped to recede visually
  if (showUnselected) return `url(#${id}-unselected)`;

  return `url(#${id}-${variant})`;
};

// Pulls <Dot /> and <ActiveDot /> out of an area's children into Recharts dot slots.
// When a `maskId` is given the resting dot is wired to the intro reveal mask so it
// wipes in with the line; the active dot is always left unmasked since it only
// appears on hover, after the intro has finished.
const resolveDots = (
  children: ReactNode,
  id: string,
  dataKey: string,
  dotOpacity: number,
  maskId: string | undefined,
): { dot: AreaDotProp; activeDot: AreaActiveDotProp } => {
  let dot: AreaDotProp = false;
  let activeDot: AreaActiveDotProp = false;

  Children.forEach(children, (child) => {
    if (!isValidElement(child)) return;

    if (child.type === Dot) {
      const { variant } = (child as ReactElement<DotProps>).props;
      dot = (
        <ChartDot
          type={variant}
          dataKey={dataKey}
          chartId={id}
          fillOpacity={dotOpacity}
          maskId={maskId}
        />
      );
    }

    if (child.type === ActiveDot) {
      const { variant } = (child as ReactElement<DotProps>).props;
      activeDot = (
        <ChartDot type={variant} dataKey={dataKey} chartId={id} fillOpacity={dotOpacity} />
      );
    }
  });

  return { dot, activeDot };
};

// ─────────────────────────────────────────────────────────────────────────────
// Style definitions — one set per <Area />, scoped to its unique id
// ─────────────────────────────────────────────────────────────────────────────

type StyleProps = {
  id: string; // unique id of the owning <Area />
  dataKey: string; // series key the colors belong to
};

// Animated dashed-stroke effect, rendered as a child of the Recharts Area
const AnimatedDashedStroke = () => {
  return (
    <>
      <animate
        attributeName="stroke-dasharray"
        values="3 3; 0 3; 3 3"
        dur="1s"
        repeatCount="indefinite"
        keyTimes="0;0.5;1"
      />
      <animate
        attributeName="stroke-dashoffset"
        values="0; -6"
        dur="1s"
        repeatCount="indefinite"
        keyTimes="0;1"
      />
    </>
  );
};

// motion `originX` for each single-rect reveal — the edge the wipe grows from.
// 0 = left edge, 1 = right edge, 0.5 = centre (grows outward to both edges).
const SINGLE_REVEAL_ORIGIN: Record<Exclude<RevealAnimationType, "edges-in">, number> = {
  "left-to-right": 0,
  "right-to-left": 1,
  "center-out": 0.5,
};

/**
 * Wipe mask driven by motion.dev, played once when an <Area /> mounts. The same
 * mask is applied to the area's fill, stroke, and resting dots, so all three
 * reveal in lockstep — fixing Recharts' default, where the dots appeared before
 * the line had finished drawing.
 *
 * `maskUnits`/`maskContentUnits` are both userSpaceOnUse so every masked element
 * shares one coordinate space and the wipe edge lands at the same x on each.
 *
 * Each rect animates `scaleX` 0 → 1; `originX` decides which edge it grows from.
 * "edges-in" needs two rects — each half grows inward from an opposite edge.
 */
const RevealMask = ({ id, type }: { id: string; type: RevealAnimationType }) => {
  const reveal = {
    initial: { scaleX: 0 },
    animate: { scaleX: 1 },
    transition: { duration: REVEAL_DURATION, ease: REVEAL_EASE },
  };

  return (
    <mask
      id={`${id}-reveal-mask`}
      maskUnits="userSpaceOnUse"
      maskContentUnits="userSpaceOnUse"
      x="0"
      y="0"
      width="100%"
      height="100%"
    >
      {type === "edges-in" ? (
        <>
          {/* left half wipes inward from the left edge toward the centre */}
          <motion.rect
            {...reveal}
            x="0"
            y="0"
            width="50%"
            height="100%"
            fill="white"
            style={{ originX: 0 }}
          />
          {/* right half wipes inward from the right edge toward the centre */}
          <motion.rect
            {...reveal}
            x="50%"
            y="0"
            width="50%"
            height="100%"
            fill="white"
            style={{ originX: 1 }}
          />
        </>
      ) : (
        <motion.rect
          {...reveal}
          x="0"
          y="0"
          width="100%"
          height="100%"
          fill="white"
          style={{ originX: SINGLE_REVEAL_ORIGIN[type] }}
        />
      )}
    </mask>
  );
};

/**
 * Horizontal left-to-right color gradient for a series. Always rendered — every
 * fill variant, the stroke, and the dots all paint from this single gradient.
 */
const ColorGradient = ({
  id,
  dataKey,
  config,
  isExpanded,
}: StyleProps & { config: ChartConfig; isExpanded: boolean }) => {
  const colorsCount = getColorsCount(config[dataKey] ?? {});

  return (
    <linearGradient
      id={`${id}-colors-${dataKey}`}
      x1="0"
      y1="0"
      x2="1"
      y2="0"
      gradientUnits={isExpanded ? "userSpaceOnUse" : "objectBoundingBox"}
    >
      {colorsCount === 1 ? (
        <>
          <stop offset="0%" stopColor={`var(--color-${dataKey}-0)`} />
          <stop offset="100%" stopColor={`var(--color-${dataKey}-0)`} />
        </>
      ) : (
        Array.from({ length: colorsCount }, (_, index) => {
          const offset = `${(index / (colorsCount - 1)) * 100}%`;
          return (
            <stop
              key={offset}
              offset={offset}
              stopColor={`var(--color-${dataKey}-${index}, var(--color-${dataKey}-0))`}
            />
          );
        })
      )}
    </linearGradient>
  );
};

/** Gradient fill that fades from visible at the top to transparent at the bottom. */
const GradientPattern = ({ id, dataKey }: StyleProps) => {
  return (
    <>
      <linearGradient id={`${id}-vertical-fade`} x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%" stopColor="white" stopOpacity={0.1} />
        <stop offset="100%" stopColor="white" stopOpacity={0} />
      </linearGradient>
      <mask id={`${id}-gradient-mask`}>
        <rect width="100%" height="100%" fill={`url(#${id}-vertical-fade)`} />
      </mask>
      <pattern id={`${id}-gradient`} patternUnits="userSpaceOnUse" width="100%" height="100%">
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-colors-${dataKey})`}
          mask={`url(#${id}-gradient-mask)`}
        />
      </pattern>
    </>
  );
};

/** Gradient fill that fades from transparent at the top to visible at the bottom. */
const ReverseGradientPattern = ({ id, dataKey }: StyleProps) => {
  return (
    <>
      <linearGradient id={`${id}-vertical-fade-reverse`} x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%" stopColor="white" stopOpacity={0} />
        <stop offset="100%" stopColor="white" stopOpacity={0.1} />
      </linearGradient>
      <mask id={`${id}-gradient-reverse-mask`}>
        <rect width="100%" height="100%" fill={`url(#${id}-vertical-fade-reverse)`} />
      </mask>
      <pattern
        id={`${id}-gradient-reverse`}
        patternUnits="userSpaceOnUse"
        width="100%"
        height="100%"
      >
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-colors-${dataKey})`}
          mask={`url(#${id}-gradient-reverse-mask)`}
        />
      </pattern>
    </>
  );
};

/** Uniform low-opacity gradient fill with no vertical fade. */
const SolidPattern = ({ id, dataKey }: StyleProps) => {
  return (
    <>
      <linearGradient id={`${id}-solid-fade`} x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%" stopColor="white" stopOpacity={0.1} />
        <stop offset="100%" stopColor="white" stopOpacity={0.1} />
      </linearGradient>
      <mask id={`${id}-solid-mask`}>
        <rect width="100%" height="100%" fill={`url(#${id}-solid-fade)`} />
      </mask>
      <pattern id={`${id}-solid`} patternUnits="userSpaceOnUse" width="100%" height="100%">
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-colors-${dataKey})`}
          mask={`url(#${id}-solid-mask)`}
        />
      </pattern>
    </>
  );
};

/** Diagonal-line texture fill, masked from the series color gradient. */
const LinesPattern = ({ id, dataKey }: StyleProps) => {
  return (
    <>
      <pattern
        id={`${id}-lines-texture`}
        patternUnits="userSpaceOnUse"
        width="5"
        height="5"
        patternTransform="rotate(45)"
      >
        <line x1="0" y1="0" x2="0" y2="5" stroke="white" strokeWidth="1" />
      </pattern>
      <mask id={`${id}-lines-mask`}>
        <rect width="100%" height="100%" fill={`url(#${id}-lines-texture)`} fillOpacity="0.3" />
      </mask>
      <pattern id={`${id}-lines`} patternUnits="userSpaceOnUse" width="100%" height="100%">
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-colors-${dataKey})`}
          mask={`url(#${id}-lines-mask)`}
        />
      </pattern>
    </>
  );
};

/** Dotted texture fill, masked from the series color gradient. */
const DottedPattern = ({ id, dataKey }: StyleProps) => {
  return (
    <>
      <pattern
        id={`${id}-dotted-texture`}
        x="0"
        y="0"
        width="6"
        height="6"
        patternUnits="userSpaceOnUse"
      >
        <circle cx="4" cy="4" r="0.5" fill="white" />
      </pattern>
      <mask id={`${id}-dotted-mask`}>
        <rect width="100%" height="100%" fill={`url(#${id}-dotted-texture)`} fillOpacity="0.5" />
      </mask>
      <pattern id={`${id}-dotted`} patternUnits="userSpaceOnUse" width="100%" height="100%">
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-colors-${dataKey})`}
          mask={`url(#${id}-dotted-mask)`}
        />
      </pattern>
    </>
  );
};

/** Hatched striped fill with a soft gradient across each stripe. */
const HatchedPattern = ({ id, dataKey }: StyleProps) => {
  return (
    <>
      <linearGradient id={`${id}-hatched-stripe`} x1="0" y1="0" x2="1" y2="0">
        <stop offset="50%" stopColor="white" stopOpacity={0.2} />
        <stop offset="50%" stopColor="white" stopOpacity={1} />
      </linearGradient>
      <pattern
        id={`${id}-hatched-texture`}
        x="0"
        y="0"
        width="20"
        height="10"
        patternUnits="userSpaceOnUse"
        overflow="visible"
        patternTransform="rotate(20)"
      >
        <rect width="20" height="10" fill={`url(#${id}-hatched-stripe)`} />
      </pattern>
      <mask id={`${id}-hatched-mask`}>
        <rect width="100%" height="100%" fill={`url(#${id}-hatched-texture)`} fillOpacity="0.2" />
      </mask>
      <pattern id={`${id}-hatched`} patternUnits="userSpaceOnUse" width="100%" height="100%">
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-colors-${dataKey})`}
          mask={`url(#${id}-hatched-mask)`}
        />
      </pattern>
    </>
  );
};

/** Diagonal-line fill used to push a non-selected area into the background. */
const UnselectedPattern = ({ id, dataKey }: StyleProps) => {
  return (
    <>
      <pattern
        id={`${id}-unselected-texture`}
        patternUnits="userSpaceOnUse"
        width="5"
        height="5"
        patternTransform="rotate(45)"
      >
        <line x1="0" y1="0" x2="0" y2="5" stroke="white" strokeWidth="1" />
      </pattern>
      <mask id={`${id}-unselected-mask`}>
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-unselected-texture)`}
          fillOpacity="0.3"
        />
      </mask>
      <pattern id={`${id}-unselected`} patternUnits="userSpaceOnUse" width="100%" height="100%">
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-colors-${dataKey})`}
          mask={`url(#${id}-unselected-mask)`}
        />
      </pattern>
    </>
  );
};

// ─────────────────────────────────────────────────────────────────────────────
// Loading skeleton
// ─────────────────────────────────────────────────────────────────────────────

// Builds bell-curve eased gradient stops for the loading shimmer
const generateEasedGradientStops = (
  steps: number = 17,
  minOpacity: number = 0.05,
  maxOpacity: number = 0.9,
) => {
  return Array.from({ length: steps }, (_, i) => {
    const t = i / (steps - 1); // 0 to 1
    // Sine-based bell curve easing: peaks at center (t=0.5), smooth falloff at edges
    const eased = Math.sin(t * Math.PI) ** 2;
    const opacity = minOpacity + eased * (maxOpacity - minOpacity);
    return { offset: `${(t * 100).toFixed(0)}%`, opacity: Number(opacity.toFixed(3)) };
  });
};

/**
 * Hook to manage loading data with pixel-perfect shimmer synchronization.
 *
 * Uses motion.dev's onUpdate callback to ensure chart data is only regenerated
 * when the shimmer has completely exited the visible area. This eliminates
 * timing drift issues from setTimeout/setInterval.
 */
export function useLoadingData(isLoading: boolean, loadingPoints: number = 14) {
  const [loadingDataKey, setLoadingDataKey] = useState(false);

  // Callback fired by motion.dev when the shimmer exits the visible area
  const onShimmerExit = useCallback(() => {
    if (isLoading) {
      setLoadingDataKey((prev) => !prev);
    }
  }, [isLoading]);

  const loadingData = useMemo(
    () => getLoadingData(loadingPoints),
    // loadingDataKey toggle triggers re-computation when the shimmer exits
    // eslint-disable-next-line react-hooks/exhaustive-deps
    [loadingPoints, loadingDataKey],
  );

  return { loadingData, onShimmerExit };
}

/**
 * The skeleton area shown while the chart is loading. Rendered by the root in
 * place of the real areas, paired with its own masked shimmer pattern.
 */
const LoadingArea = ({
  chartId,
  curveType,
  onShimmerExit,
}: {
  chartId: string;
  curveType: CurveType;
  onShimmerExit: () => void;
}) => {
  return (
    <>
      <RechartsArea
        type={curveType}
        dataKey={LOADING_AREA_DATA_KEY}
        fillOpacity={0.05}
        fill="currentColor"
        stroke="currentColor"
        strokeOpacity={0.5}
        isAnimationActive={false}
        legendType="none"
        tooltipType="none"
        activeDot={false}
        dot={false}
        style={{ mask: `url(#${chartId}-loading-mask)` }}
      />
      <defs>
        <LoadingPattern chartId={chartId} onShimmerExit={onShimmerExit} />
      </defs>
    </>
  );
};

/**
 * Animated shimmer pattern for the loading skeleton.
 *
 * The visible chart area is normalized to 0-1, the shimmer gradient has width 1,
 * and the pattern is 3x wide so the shimmer has buffer on both sides. The motion
 * rect travels x from -1 to 2; onShimmerExit fires as it crosses x=1, letting the
 * data swap happen while the shimmer is off-screen for a seamless loop.
 */
const LoadingPattern = ({
  chartId,
  onShimmerExit,
}: {
  chartId: string;
  onShimmerExit: () => void;
}) => {
  const gradientStops = generateEasedGradientStops();

  // 1 (left buffer) + 1 (visible) + 1 (right buffer)
  const patternWidth = 3;
  const startX = -1;
  const endX = 2;

  // Tracks the last x value to detect the exit threshold crossing
  const lastXRef = useRef(startX);

  return (
    <>
      <linearGradient id={`${chartId}-loading-gradient`} x1="0" y1="0" x2="1" y2="0">
        {gradientStops.map(({ offset, opacity }) => (
          <stop key={offset} offset={offset} stopColor="white" stopOpacity={opacity} />
        ))}
      </linearGradient>
      <pattern
        id={`${chartId}-loading-pattern`}
        patternUnits="objectBoundingBox"
        patternContentUnits="objectBoundingBox"
        patternTransform="rotate(25)"
        width={patternWidth}
        height="1"
        x="0"
        y="0"
      >
        <motion.rect
          y="0"
          width="1"
          height="1"
          fill={`url(#${chartId}-loading-gradient)`}
          initial={{ x: startX }}
          animate={{ x: endX }}
          transition={{
            duration: LOADING_ANIMATION_DURATION / 1000,
            ease: "linear",
            repeat: Infinity,
            repeatType: "loop",
          }}
          onUpdate={(latest) => {
            const xValue = typeof latest.x === "number" ? latest.x : startX;
            const lastX = lastXRef.current;

            // Fire once per loop, when the shimmer fully exits the visible area
            if (xValue >= 1 && lastX < 1) {
              onShimmerExit();
            }

            lastXRef.current = xValue;
          }}
        />
      </pattern>
      <mask id={`${chartId}-loading-mask`} maskUnits="userSpaceOnUse">
        <rect width="100%" height="100%" fill={`url(#${chartId}-loading-pattern)`} />
      </mask>
    </>
  );
};
```
        
      
       
        #### Now, Let's add the chart component to your project.

        
          These Components are required by the chart component to render the chart. Make a folder called `ui` inside the `evilcharts` folder and paste the code there.

          Below is main chart component.
        
        
          #### Component Implementation

##### Source File: `chart.tsx`

```tsx
"use client";

import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

// Format: { THEME_NAME: CSS_SELECTOR }
const THEMES = { light: "", dark: ".dark" } as const;

type ThemeKey = keyof typeof THEMES;

// All Keys are optional at first
type ThemeColorsBase = {
  [K in ThemeKey]?: string[];
};

// Require at least one theme key
type AtLeastOneThemeColor = {
  [K in ThemeKey]: Required<Pick<ThemeColorsBase, K>> & Partial<Omit<ThemeColorsBase, K>>;
}[ThemeKey];

const VALID_THEME_KEYS = Object.keys(THEMES) as ThemeKey[];

// Validation for chart config colors at runtime
function validateChartConfigColors(config: ChartConfig): void {
  for (const [key, value] of Object.entries(config)) {
    if (value.colors) {
      const hasValidThemeKey = VALID_THEME_KEYS.some(
        (themeKey) => value.colors?.[themeKey] !== undefined,
      );

      if (!hasValidThemeKey) {
        throw new Error(
          `[EvilCharts] Invalid chart config for "${key}": colors object must have at least one theme key (${VALID_THEME_KEYS.join(", ")}). Received empty object or invalid keys.`,
        );
      }
    }
  }
}

export type ChartConfig = Record<
  string,
  {
    label?: React.ReactNode;
    icon?: React.ComponentType;
    colors?: AtLeastOneThemeColor;
  }
>;

interface ChartContextProps {
  config: ChartConfig;
}

const ChartContext = React.createContext<ChartContextProps | null>(null);

export function useChart() {
  const context = React.useContext(ChartContext);

  if (!context) {
    throw new Error("useChart must be used within a <ChartContainer />");
  }

  return context;
}

interface ChartContainerProps
  extends
    Omit<React.ComponentProps<"div">, "children">,
    Pick<
      React.ComponentProps<typeof RechartsPrimitive.ResponsiveContainer>,
      | "initialDimension"
      | "aspect"
      | "debounce"
      | "minHeight"
      | "minWidth"
      | "maxHeight"
      | "height"
      | "width"
      | "onResize"
      | "children"
    > {
  config: ChartConfig;
  innerResponsiveContainerStyle?: React.ComponentProps<
    typeof RechartsPrimitive.ResponsiveContainer
  >["style"];
  /** Optional content rendered below the chart (e.g. EvilBrush) */
  footer?: React.ReactNode;
}

function ChartContainer({
  id,
  config,
  initialDimension = { width: 320, height: 200 },
  className,
  children,
  footer,
  ...props
}: Readonly<ChartContainerProps>) {
  const uniqueId = React.useId();
  const chartId = `chart-${id ?? uniqueId.replace(/:/g, "")}`;

  // Validate chart config at runtime
  validateChartConfigColors(config);

  return (
    <ChartContext.Provider value={{ config }}>
      <div
        data-slot="chart"
        data-chart={chartId}
        className={cn(
          "min-h-0 w-full flex-1",
          "[&_.recharts-cartesian-axis-tick_text]:fill-muted-foreground [&_.recharts-cartesian-grid_line[stroke='#ccc']]:stroke-border/50 [&_.recharts-curve.recharts-tooltip-cursor]:stroke-border [&_.recharts-polar-grid_[stroke='#ccc']]:stroke-border [&_.recharts-radial-bar-background-sector]:fill-muted [&_.recharts-rectangle.recharts-tooltip-cursor]:fill-muted [&_.recharts-reference-line_[stroke='#ccc']]:stroke-border relative flex flex-col justify-center text-xs [&_.recharts-dot[stroke='#fff']]:stroke-transparent [&_.recharts-layer]:outline-hidden [&_.recharts-sector]:outline-hidden [&_.recharts-sector[stroke='#fff']]:stroke-transparent [&_.recharts-surface]:outline-hidden",
          !footer && "aspect-video",
          className,
        )}
        {...props}
      >
        <ChartStyle id={chartId} config={config} />
        <RechartsPrimitive.ResponsiveContainer
          className="min-h-0 w-full flex-1"
          initialDimension={initialDimension}
        >
          {children}
        </RechartsPrimitive.ResponsiveContainer>
        {footer}
      </div>
    </ChartContext.Provider>
  );
}

function LoadingIndicator({ isLoading }: { isLoading: boolean }) {
  if (!isLoading) {
    return null;
  }

  return (
    <div className="pointer-events-none absolute inset-0 z-20 flex items-center justify-center">
      <div className="text-primary bg-background flex items-center justify-center gap-2 rounded-md border px-2 py-0.5 text-sm">
        <div className="border-border border-t-primary h-3 w-3 animate-spin rounded-full border" />
        <span>Loading</span>
      </div>
    </div>
  );
}

// Distribute colors evenly across slots, extra slots go to last color(s)
// Example: 2 colors for 4 slots → [red, red, pink, pink]
// Example: 3 colors for 4 slots → [red, pink, blue, blue]
function distributeColors(colorsArray: string[], maxCount: number): string[] {
  const availableCount = colorsArray.length;
  if (availableCount >= maxCount) {
    return colorsArray.slice(0, maxCount);
  }

  const result: string[] = [];
  const baseSlots = Math.floor(maxCount / availableCount);
  const extraSlots = maxCount % availableCount;

  // First (availableCount - extraSlots) colors get baseSlots each
  // Last extraSlots colors get (baseSlots + 1) each
  for (let colorIdx = 0; colorIdx < availableCount; colorIdx++) {
    const isExtraColor = colorIdx >= availableCount - extraSlots;
    const slotsForThisColor = baseSlots + (isExtraColor ? 1 : 0);
    for (let j = 0; j < slotsForThisColor; j++) {
      result.push(colorsArray[colorIdx]);
    }
  }

  return result;
}

const ChartStyle = ({ id, config }: { id: string; config: ChartConfig }) => {
  const colorConfig = Object.entries(config).filter(([, config]) => config.colors);

  if (!colorConfig.length) {
    return null;
  }

  const generateCssVars = (theme: keyof typeof THEMES) =>
    colorConfig
      .flatMap(([key, itemConfig]) => {
        const colorsArray = itemConfig.colors?.[theme];
        if (!colorsArray || !Array.isArray(colorsArray) || colorsArray.length === 0) {
          return [];
        }

        // Get max count across all themes for this key
        const maxCount = getColorsCount(itemConfig);

        // Distribute colors evenly across all required slots
        const distributedColors = distributeColors(colorsArray, maxCount);

        return distributedColors.map((color, index) => `  --color-${key}-${index}: ${color};`);
      })
      .filter(Boolean)
      .join("\n");

  const css = Object.entries(THEMES)
    .map(
      ([theme, prefix]) =>
        `${prefix} [data-chart=${id}] {\n${generateCssVars(theme as keyof typeof THEMES)}\n}`,
    )
    .join("\n");

  return <style dangerouslySetInnerHTML={{ __html: css }} />;
};

// Helper to extract item config from a payload.
export function getPayloadConfigFromPayload(config: ChartConfig, payload: unknown, key: string) {
  if (typeof payload !== "object" || payload === null) {
    return undefined;
  }

  const payloadPayload =
    "payload" in payload && typeof payload.payload === "object" && payload.payload !== null
      ? payload.payload
      : undefined;

  let configLabelKey: string = key;

  if (key in payload && typeof payload[key as keyof typeof payload] === "string") {
    configLabelKey = payload[key as keyof typeof payload] as string;
  } else if (
    payloadPayload &&
    key in payloadPayload &&
    typeof payloadPayload[key as keyof typeof payloadPayload] === "string"
  ) {
    configLabelKey = payloadPayload[key as keyof typeof payloadPayload] as string;
  }

  return configLabelKey in config ? config[configLabelKey] : config[key];
}

// Format values to percent for expanded charts
function axisValueToPercentFormatter(value: number) {
  return `${Math.round(value * 100).toFixed(0)}%`;
}

// Get max colors count across all themes for a config entry
function getColorsCount(config: ChartConfig[string]): number {
  if (!config.colors) return 1;
  const counts = VALID_THEME_KEYS.map((theme) => config.colors?.[theme]?.length ?? 0);
  return Math.max(...counts, 1);
}

// Generate random loading data for skeleton/loading state
// min/max represent percentage of the range (0-100), defaults to 20-80 for realistic look
export const getLoadingData = (points: number = 10, min: number = 0, max: number = 70) => {
  const range = max - min;
  return Array.from({ length: points }, () => ({
    loading: Math.floor(Math.random() * range) + min,
  }));
};

export {
  ChartContainer,
  ChartStyle,
  axisValueToPercentFormatter,
  LoadingIndicator,
  getColorsCount,
};
```
        
      
       
        #### Now, We need to add sub components.

        
          Create a file called `tooltip.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `tooltip.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import type { NameType, ValueType } from "recharts/types/component/DefaultTooltipContent";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type TooltipRoundness = "sm" | "md" | "lg" | "xl";
type TooltipVariant = "default" | "frosted-glass";

const roundnessMap: Record<TooltipRoundness, string> = {
  sm: "rounded-sm",
  md: "rounded-md",
  lg: "rounded-lg",
  xl: "rounded-xl",
};

const variantMap: Record<TooltipVariant, string> = {
  default: "bg-background",
  "frosted-glass": "bg-background/70 backdrop-blur-sm",
};

function ChartTooltipContent({
  active,
  payload,
  className,
  indicator = "dot",
  hideLabel = false,
  hideIndicator = false,
  label,
  labelFormatter,
  labelClassName,
  formatter,
  nameKey,
  labelKey,
  selected,
  roundness = "lg",
  variant = "default",
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip> &
  React.ComponentProps<"div"> & {
    hideLabel?: boolean;
    hideIndicator?: boolean;
    indicator?: "line" | "dot" | "dashed";
    nameKey?: string;
    labelKey?: string;
    selected?: string | null;
    roundness?: TooltipRoundness;
    variant?: TooltipVariant;
  } & Omit<
    RechartsPrimitive.DefaultTooltipContentProps<ValueType, NameType>,
    "accessibilityLayer"
  >) {
  const { config } = useChart();

  const tooltipLabel = React.useMemo(() => {
    if (hideLabel || !payload?.length) {
      return null;
    }

    const [item] = payload;
    const key = `${labelKey ?? item?.dataKey ?? item?.name ?? "value"}`;
    const itemConfig = getPayloadConfigFromPayload(config, item, key);
    const value =
      !labelKey && typeof label === "string" ? (config[label]?.label ?? label) : itemConfig?.label;

    if (labelFormatter) {
      return (
        <div className={cn("font-medium", labelClassName)}>{labelFormatter(value, payload)}</div>
      );
    }

    if (!value) {
      return null;
    }

    return <div className={cn("font-medium", labelClassName)}>{value}</div>;
  }, [label, labelFormatter, payload, hideLabel, labelClassName, config, labelKey]);

  if (!active || !payload?.length) {
    // Empty tooltip - to prevent position getting 0.0 so it doesnt animate tooltip every time from 0.0 origin
    return <span className="p-4" />;
  }

  const nestLabel = payload.length === 1 && indicator !== "dot";

  return (
    <div
      className={cn(
        "border-border/50 grid min-w-32 items-start gap-1.5 border px-2.5 py-1.5 text-xs shadow-xl",
        roundnessMap[roundness],
        variantMap[variant],
        className,
      )}
    >
      {!nestLabel ? tooltipLabel : null}
      <div className="grid gap-1.5">
        {payload
          .filter((item) => item.type !== "none")
          .map((item, index) => {
            // For pie charts, item.name contains the sector name (e.g., "chrome")
            // For radial charts, the name is in item.payload[nameKey]
            // For other charts, item.name or item.dataKey contains the series name
            const payloadName =
              nameKey && item.payload
                ? (item.payload as Record<string, unknown>)[nameKey]
                : undefined;
            const key = `${payloadName ?? item.name ?? item.dataKey ?? "value"}`;
            const itemConfig = getPayloadConfigFromPayload(config, item, key);

            // Get colors count for this item to determine gradient vs solid
            const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

            return (
              <div
                key={index}
                className={cn(
                  "[&>svg]:text-muted-foreground flex w-full flex-wrap items-stretch gap-2 [&>svg]:h-2.5 [&>svg]:w-2.5",
                  indicator === "dot" && "items-center",
                  selected != null && selected !== item.dataKey && "opacity-30",
                )}
              >
                {formatter && item?.value !== undefined && item.name ? (
                  formatter(item.value, item.name, item, index, item.payload)
                ) : (
                  <>
                    {itemConfig?.icon ? (
                      <itemConfig.icon />
                    ) : (
                      !hideIndicator && (
                        <div
                          className={cn("shrink-0 rounded-[2px]", {
                            "h-2.5 w-2.5": indicator === "dot",
                            "w-1": indicator === "line",
                            "w-0 border-[1.5px] border-dashed bg-transparent!":
                              indicator === "dashed",
                            "my-0.5": nestLabel && indicator === "dashed",
                          })}
                          style={getIndicatorColorStyle(key, colorsCount)}
                        />
                      )
                    )}
                    <div
                      className={cn(
                        "flex flex-1 justify-between gap-4 leading-none",
                        nestLabel ? "items-end" : "items-center",
                      )}
                    >
                      <div className="grid gap-1.5">
                        {nestLabel ? tooltipLabel : null}
                        <span className="text-muted-foreground">
                          {itemConfig?.label ?? item.name}
                        </span>
                      </div>
                      {item.value != null && (
                        <span className="text-foreground font-mono font-medium tabular-nums">
                          {typeof item.value === "number"
                            ? item.value.toLocaleString()
                            : String(item.value)}
                        </span>
                      )}
                    </div>
                  </>
                )}
              </div>
            );
          })}
      </div>
    </div>
  );
}

function getIndicatorColorStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { background: `var(--color-${dataKey}-0)` };
  }

  // Multiple colors: create linear gradient with evenly distributed stops
  const stops = Array.from({ length: colorsCount }, (_, index) => {
    const offset = (index / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${index}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

const ChartTooltip = ({
  animationDuration = 200,
  ...props
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip>) => (
  <RechartsPrimitive.Tooltip animationDuration={animationDuration} {...props} />
);

export { ChartTooltip, ChartTooltipContent };
export type { TooltipRoundness, TooltipVariant };
```
        
        
          Now, create another file called `legend.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `legend.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type ChartLegendVariant =
  | "square"
  | "circle"
  | "circle-outline"
  | "rounded-square"
  | "rounded-square-outline"
  | "vertical-bar"
  | "horizontal-bar";

function ChartLegendContent({
  className,
  hideIcon = false,
  nameKey,
  payload,
  verticalAlign,
  align = "right",
  selected,
  onSelectChange,
  isClickable,
  variant = "rounded-square",
}: React.ComponentProps<"div"> & {
  hideIcon?: boolean;
  nameKey?: string;
  selected?: string | null;
  isClickable?: boolean;
  onSelectChange?: (selected: string | null) => void;
  variant?: ChartLegendVariant;
} & RechartsPrimitive.DefaultLegendContentProps) {
  const { config } = useChart();

  if (!payload?.length) {
    return null;
  }

  return (
    <div
      className={cn(
        "flex items-center gap-4 select-none",
        align === "left" && "justify-start",
        align === "center" && "justify-center",
        align === "right" && "justify-end",
        verticalAlign === "top" ? "pb-4" : "pt-4",
        className,
      )}
    >
      {payload
        .filter((item) => item.type !== "none")
        .map((item) => {
          // For pie charts, item.value contains the sector name (e.g., "chrome")
          // For radial charts, the name is in item.payload[nameKey]
          // For other charts, item.dataKey contains the series name (e.g., "desktop")
          const payloadName =
            nameKey && item.payload
              ? (item.payload as Record<string, unknown>)[nameKey]
              : undefined;
          const key = `${payloadName ?? item.value ?? item.dataKey ?? "value"}`;
          const itemConfig = getPayloadConfigFromPayload(config, item, key);
          const isSelected = selected === null || selected === key;

          // Get colors count for this item to determine gradient vs solid
          const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

          return (
            <div
              key={key}
              className={cn(
                "[&>svg]:text-muted-foreground flex items-center gap-1.5 transition-opacity [&>svg]:h-3 [&>svg]:w-3",
                !isSelected && "opacity-30",
                isClickable && "cursor-pointer",
              )}
              onClick={() => {
                if (!isClickable) return;

                onSelectChange?.(selected === key ? null : key);
              }}
            >
              {itemConfig?.icon && !hideIcon ? (
                <itemConfig.icon />
              ) : (
                <LegendIndicator
                  variant={variant}
                  dataKey={key}
                  colorsCount={colorsCount}
                />
              )}
              {itemConfig?.label}
            </div>
          );
        })}
    </div>
  );
}

// ---------------------------------------------------------------------------
// Legend indicator — each variant gets its own branch so future variants
// can diverge freely in markup & style.
// ---------------------------------------------------------------------------

function LegendIndicator({
  variant,
  dataKey,
  colorsCount,
}: {
  variant: ChartLegendVariant;
  dataKey: string;
  colorsCount: number;
}) {
  const fillStyle = getLegendFillStyle(dataKey, colorsCount);
  const outlineStyle = getLegendOutlineStyle(dataKey, colorsCount);

  switch (variant) {
    case "square":
      return <div className="h-2 w-2 shrink-0" style={fillStyle} />;

    case "circle":
      return <div className="h-2 w-2 shrink-0 rounded-full" style={fillStyle} />;

    case "circle-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-full p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "vertical-bar":
      return <div className="h-3 w-1 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "horizontal-bar":
      return <div className="h-1 w-3 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "rounded-square-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-[3px] p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "rounded-square":
    default:
      return <div className="h-2 w-2 shrink-0 rounded-[2px]" style={fillStyle} />;
  }
}

// ---------------------------------------------------------------------------
// Style helpers
// ---------------------------------------------------------------------------

/** Solid fill / gradient background for filled variants. */
function getLegendFillStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { backgroundColor: `var(--color-${dataKey}-0)` };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

/**
 * Outline style for stroke variants.
 * Uses background + mask-composite to punch out the center, leaving only the
 * "border" visible. Works with both solid colors and gradients, and respects
 * border-radius — unlike plain `border-color`.
 */
function getLegendOutlineStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  const maskStyle: React.CSSProperties = {
    WebkitMask:
      "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    WebkitMaskComposite: "xor",
    mask: "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    maskComposite: "exclude",
  };

  if (colorsCount <= 1) {
    return {
      backgroundColor: `var(--color-${dataKey}-0)`,
      ...maskStyle,
    };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return {
    background: `linear-gradient(to right, ${stops})`,
    ...maskStyle,
  };
}

const ChartLegend = RechartsPrimitive.Legend;

export { ChartLegend, ChartLegendContent, type ChartLegendVariant };
```
        
        
          Finally, create last file called `dot.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `dot.tsx`

```tsx
import { cn } from "@/lib/utils";
import * as React from "react";

export type DotVariant = "default" | "border" | "colored-border";

type ChartDotProps = {
  cx?: number;
  cy?: number;
  dataKey: string;
  chartId: string;
  className?: string;
  fillOpacity?: number;
  type?: DotVariant;
  /** Optional SVG <mask> id — lets the dot share an area's intro reveal wipe. */
  maskId?: string;
};

const ChartDot = React.memo(function ChartDot({
  cx,
  cy,
  dataKey,
  chartId,
  className,
  fillOpacity = 1,
  type = "default",
  maskId,
}: ChartDotProps) {
  const dotId = React.useId().replace(/:/g, "");
  const gradientUrl = `url(#${chartId}-colors-${String(dataKey)})`;

  if (cx === undefined || cy === undefined) return null;

  switch (type) {
    case "border":
      return (
        <PrimaryBorderDot
          cx={cx}
          cy={cy}
          dotId={dotId}
          fillOpacity={fillOpacity}
          gradientUrl={gradientUrl}
          className={className}
          maskId={maskId}
        />
      );
    case "colored-border":
      return (
        <ColoredBorderDot
          cx={cx}
          cy={cy}
          dotId={dotId}
          fillOpacity={fillOpacity}
          gradientUrl={gradientUrl}
          className={className}
          maskId={maskId}
        />
      );
    default:
      return (
        <DefaultDot
          cx={cx}
          cy={cy}
          dotId={dotId}
          fillOpacity={fillOpacity}
          gradientUrl={gradientUrl}
          className={className}
          maskId={maskId}
        />
      );
  }
});

type DotVariantProps = {
  cx: number;
  cy: number;
  dotId: string;
  fillOpacity: number;
  gradientUrl: string;
  className?: string;
  maskId?: string;
};

const DefaultDot = React.memo(
  ({ cx, cy, dotId, fillOpacity, gradientUrl, className, maskId }: DotVariantProps) => {
    const r = 3;
    return (
      <g className={className} mask={maskId ? `url(#${maskId})` : undefined}>
        <defs>
          <clipPath id={`dot-clip-${dotId}`}>
            <circle cx={cx} cy={cy} r={r} />
          </clipPath>
        </defs>
        {/* Full-width gradient rectangle clipped to dot shape */}
        <rect
          x="0"
          y={cy - r}
          width="100%"
          height={r * 2}
          fill={gradientUrl}
          fillOpacity={fillOpacity}
          clipPath={`url(#dot-clip-${dotId})`}
        />
      </g>
    );
  },
);

DefaultDot.displayName = "DefaultDot";

const PrimaryBorderDot = React.memo(
  ({ cx, cy, dotId, fillOpacity, gradientUrl, className, maskId }: DotVariantProps) => {
    const r = 6;
    const strokeWidth = 5;
    return (
      <g className={cn(className, "text-background")} mask={maskId ? `url(#${maskId})` : undefined}>
        <defs>
          <clipPath id={`dot-clip-${dotId}`}>
            <circle cx={cx} cy={cy} r={r} />
          </clipPath>
        </defs>
        {/* Background stroke (border) */}
        <circle cx={cx} cy={cy} r={r} fill="currentColor" />
        {/* Inner gradient circle clipped */}
        <rect
          x="0"
          y={cy - (r - strokeWidth / 2)}
          width="100%"
          height={(r - strokeWidth / 2) * 2}
          fill={gradientUrl}
          fillOpacity={fillOpacity}
          clipPath={`url(#dot-clip-inner-${dotId})`}
        />
        <defs>
          <clipPath id={`dot-clip-inner-${dotId}`}>
            <circle cx={cx} cy={cy} r={r - strokeWidth / 2} />
          </clipPath>
        </defs>
      </g>
    );
  },
);

PrimaryBorderDot.displayName = "PrimaryBorderDot";

const ColoredBorderDot = React.memo(
  ({ cx, cy, dotId, fillOpacity, gradientUrl, className, maskId }: DotVariantProps) => {
    const r = 3;
    const strokeWidth = 1;
    return (
      <g className={cn(className, "text-background")} mask={maskId ? `url(#${maskId})` : undefined}>
        <defs>
          <clipPath id={`dot-clip-${dotId}`}>
            <circle cx={cx} cy={cy} r={r + strokeWidth / 2} />
          </clipPath>
        </defs>
        {/* Gradient stroke (border) via clipped rect */}
        <rect
          x="0"
          y={cy - r - strokeWidth / 2}
          width="100%"
          height={(r + strokeWidth / 2) * 2}
          fill={gradientUrl}
          fillOpacity={fillOpacity}
          clipPath={`url(#dot-clip-${dotId})`}
        />
        {/* Inner solid fill */}
        <circle cx={cx} cy={cy} r={r - strokeWidth / 2} fill="currentColor" />
      </g>
    );
  },
);

ColoredBorderDot.displayName = "ColoredBorderDot";

export { ChartDot };
```
        
      
    
  


## Usage

The area chart is composible. `<EvilAreaChart>` is the container, and you compose only the parts you need — `<Grid>`, `<XAxis>`, `<YAxis>`, `<Legend>`, `<Tooltip>`, and one or more `<Area>` — as its children. Each `<Area>` carries its own `variant`, `strokeVariant`, and `isClickable`, so a single chart can mix fill styles, stroke styles, and make only some series interactive.

```tsx
import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/components/evilcharts/charts/area-chart";
```

```tsx
<EvilAreaChart data={data} config={chartConfig} stackType="stacked">
  <Grid />
  <XAxis dataKey="month" />
  <YAxis />
  <Legend isClickable />
  <Tooltip />
  <Area dataKey="desktop" variant="gradient" strokeVariant="dashed" isClickable>
    <Dot variant="border" />
    <ActiveDot variant="colored-border" />
  </Area>
  <Area dataKey="mobile" variant="hatched" strokeVariant="solid" isClickable>
    <ActiveDot variant="colored-border" />
  </Area>
</EvilAreaChart>
```

### Interactive Selection

Add `isClickable` to any `<Area>` (and to `<Legend>`) to make those series selectable. Use the `onSelectionChange` callback on `<EvilAreaChart>` to handle selection events:

```tsx
<EvilAreaChart
  data={data}
  config={chartConfig}
  onSelectionChange={(selectedDataKey) => {
    if (selectedDataKey) {
      console.log("Selected:", selectedDataKey);
    } else {
      console.log("Deselected");
    }
  }}
>
  <XAxis dataKey="month" />
  <Legend isClickable />
  <Tooltip />
  <Area dataKey="desktop" variant="gradient" isClickable />
  <Area dataKey="mobile" variant="gradient" isClickable />
</EvilAreaChart>
```

### Loading State

#### Live Example Code

##### Example File: `ex-loading-state-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={[]} // if isLoading is true, pass empty array → i.e isLoading ? [] : data
      config={chartConfig}
      className="h-full w-full p-4"
      isLoading={true} // [!code highlight]
      stackType="stacked"
      curveType="bump"
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area dataKey="desktop" variant="gradient" isClickable>
        <ActiveDot variant="default" />
      </Area>
      <Area dataKey="mobile" variant="gradient" isClickable>
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    The area chart supports loading state. You can pass the `isLoading` prop to the chart to show the loading state. You can also pass `curveType` to customize the curve type of the loading state. Here, im using `curveType='bump'` to make the loading state more realistic.
  


## Examples

Below are some examples of the area chart with different `variants`. where you can change the `stackType`, `curveType`, `strokeVariant` & `areaVariant`. 

### Gradient Colors

#### Live Example Code

##### Example File: `ex-gradient-colors-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  Grid,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["red", "orange", "rosybrown", "purple", "blue"], // [!code highlight]
      dark: ["red", "orange", "rosybrown", "purple", "blue"], // [!code highlight]
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["gray"],
      dark: ["gray"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Area dataKey="desktop" variant="gradient" isClickable>
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Area>
      <Area dataKey="mobile" variant="gradient" isClickable>
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-gradient-colors-bump-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  Grid,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["red", "orange", "rosybrown", "purple", "blue"], // [!code highlight]
      dark: ["red", "orange", "rosybrown", "purple", "blue"], // [!code highlight]
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["gray"],
      dark: ["gray"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
      curveType="bump"
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Area dataKey="desktop" variant="gradient" isClickable>
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Area>
      <Area dataKey="mobile" variant="gradient" isClickable>
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```

### Curve Types

#### Live Example Code

##### Example File: `ex-bump-curve-type-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
      curveType="bump" // [!code highlight]
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area dataKey="desktop" variant="gradient" isClickable>
        <Dot variant="default" />
        <ActiveDot variant="default" />
      </Area>
      <Area dataKey="mobile" variant="gradient" isClickable>
        <Dot variant="default" />
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-step-curve-type-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
      curveType="step" // [!code highlight]
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area dataKey="desktop" variant="gradient" isClickable>
        <Dot variant="default" />
        <ActiveDot variant="default" />
      </Area>
      <Area dataKey="mobile" variant="gradient" isClickable>
        <Dot variant="default" />
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-monotoney-curve-type-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
      curveType="monotoneY" // [!code highlight]
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area dataKey="desktop" variant="gradient" isClickable>
        <Dot variant="default" />
        <ActiveDot variant="default" />
      </Area>
      <Area dataKey="mobile" variant="gradient" isClickable>
        <Dot variant="default" />
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```

### Stack Types

#### Live Example Code

##### Example File: `ex-default-type-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="default" // [!code highlight]
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area dataKey="desktop" variant="gradient" isClickable>
        <ActiveDot variant="default" />
      </Area>
      <Area dataKey="mobile" variant="gradient" isClickable>
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-stacked-type-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked" // [!code highlight]
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area dataKey="desktop" variant="gradient" isClickable>
        <ActiveDot variant="default" />
      </Area>
      <Area dataKey="mobile" variant="gradient" isClickable>
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-expanded-type-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="expanded" // [!code highlight]
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area dataKey="desktop" variant="gradient" isClickable>
        <ActiveDot variant="default" />
      </Area>
      <Area dataKey="mobile" variant="gradient" isClickable>
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```

### Stroke Variants

#### Live Example Code

##### Example File: `ex-solid-stroke-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area
        dataKey="desktop"
        variant="gradient"
        strokeVariant="solid" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
      <Area
        dataKey="mobile"
        variant="gradient"
        strokeVariant="solid" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-dashed-stroke-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area
        dataKey="desktop"
        variant="gradient"
        strokeVariant="dashed" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
      <Area
        dataKey="mobile"
        variant="gradient"
        strokeVariant="dashed" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-animated-dashed-stroke-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area
        dataKey="desktop"
        variant="gradient"
        strokeVariant="animated-dashed" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
      <Area
        dataKey="mobile"
        variant="gradient"
        strokeVariant="animated-dashed" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```

### Area Variants

#### Live Example Code

##### Example File: `ex-gradient-area-variant-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area
        dataKey="desktop"
        variant="gradient" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
      <Area
        dataKey="mobile"
        variant="gradient" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-gradient-reverse-area-variant-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area
        dataKey="desktop"
        variant="gradient-reverse" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
      <Area
        dataKey="mobile"
        variant="gradient-reverse" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-solid-area-variant-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area
        dataKey="desktop"
        variant="solid" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
      <Area
        dataKey="mobile"
        variant="solid" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-dotted-area-variant-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area
        dataKey="desktop"
        variant="dotted" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
      <Area
        dataKey="mobile"
        variant="dotted" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-lines-area-variant-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area
        dataKey="desktop"
        variant="lines" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
      <Area
        dataKey="mobile"
        variant="lines" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-hatched-area-variant-area-chart.tsx`

```tsx
"use client";

import {
  EvilAreaChart,
  Area,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
} from "@/registry/charts/area-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 245 },
  { month: "February", desktop: 876, mobile: 654 },
  { month: "March", desktop: 512, mobile: 387 },
  { month: "April", desktop: 629, mobile: 521 },
  { month: "May", desktop: 458, mobile: 412 },
  { month: "June", desktop: 781, mobile: 598 },
  { month: "July", desktop: 394, mobile: 312 },
  { month: "August", desktop: 925, mobile: 743 },
  { month: "September", desktop: 647, mobile: 489 },
  { month: "October", desktop: 532, mobile: 476 },
  { month: "November", desktop: 803, mobile: 687 },
  { month: "December", desktop: 271, mobile: 198 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleAreaChart() {
  return (
    <EvilAreaChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked"
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Area
        dataKey="desktop"
        variant="hatched" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
      <Area
        dataKey="mobile"
        variant="hatched" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Area>
    </EvilAreaChart>
  );
}
```

## API Reference

The chart is composed of several parts. The props below are grouped by the component they belong to.

### EvilAreaChart


The root container. It owns the data, the shared selection state, the loading skeleton, and the optional brush. Everything visual is composed as its children.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **data*** | `TData[]` | - | Data used to display the chart — an array of objects where each object is a data point (`TData extends Record`). |
| **config** | - | - | " required>     Defines the chart's series. Each key matches a data key, with a color or color array. |
| **children*** | `ReactNode` | - | The composed chart parts — ``, ``, ``, ``, ``, and one or more ``. |
| **className** | `string` | - | Additional CSS classes for the chart container. |
| **curveType** | - | - | Default curve interpolation inherited by every ``. |
| **animationType** | - | - | Direction of the custom intro reveal inherited by every ``. `"none"` disables it — devices with the OS reduce-motion preference fall back to `"none"` automatically. |
| **stackType** | - | - | How multiple areas combine — independent, stacked, or normalized to 100%. |
| **defaultSelectedDataKey** | `string | null` | `null` | The series selected on first render. |
| **onSelectionChange** | - | - | void">     Fires when a series is selected or deselected via a clickable `` or ``. |
| **isLoading** | `boolean` | `false` | Shows the animated loading skeleton. |
| **loadingPoints** | `number` | `14` | Number of points in the loading skeleton. |
| **showBrush** | `boolean` | `false` | Renders a zoom brush below the chart. |
| **xDataKey** | `keyof TData & string` | - | X-axis key — only needed by the brush footer. |
| **brushHeight** | `number` | - | Height of the brush preview in pixels. |
| **brushFormatLabel** | - | - | string">     Formats the brush axis labels. |
| **onBrushChange** | - | - | void">     Fires when the brush selection range changes. |
| **chartProps** | - | - | ">     Escape hatch forwarded to the underlying Recharts AreaChart. See the Recharts AreaChart documentation. |



### Area


A single area series. Each `<Area />` is self-contained and generates its own gradient/pattern definitions, so a chart can hold any number of areas — each with its own variant, stroke, and clickability.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **dataKey*** | `string` | - | The series key. Must exist on both the data rows and the chart `config`. |
| **variant** | - | - | The visual style of the area fill. Applies to this area only. |
| **strokeVariant** | - | - | The stroke style for this area. |
| **curveType** | - | - | The curve interpolation for this area. Falls back to the chart's `curveType` when omitted. |
| **animationType** | - | - | The intro reveal animation for this area. Falls back to the chart's `animationType` when omitted. |
| **connectNulls** | `boolean` | `false` | Whether to connect line segments across null or missing values. |
| **isClickable** | `boolean` | `false` | Lets this area be selected by clicking it. When any area is selected, unselected areas become semi-transparent. |
| **children** | `ReactNode` | - | Optional `` and `` composition that adds point markers to this area. |
| **areaProps** | - | - | ">     Escape hatch for raw props forwarded to the underlying Recharts Area component. |



### Dot and ActiveDot


Point markers composed inside an `<Area />`. `<Dot />` is the resting marker; `<ActiveDot />` is the hovered marker. They render nothing on their own — the parent `<Area />` reads their `variant`.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style of the point marker. |



### XAxis and YAxis


The category and value axes. Both ship with the chart's flat default styling and forward every Recharts axis prop, so `dataKey`, `tickFormatter`, `tickMargin`, etc. pass straight through. They are hidden automatically while the chart is loading, and `<YAxis />` formats ticks as percentages when `stackType="expanded"`.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **dataKey** | `string` | - | The data key for the axis values. |
| **…axisProps** | - | - | Every other Recharts XAxis / YAxis prop is forwarded as-is. See the Recharts XAxis and Recharts YAxis documentation. |



### Grid


The background grid lines. Defaults to horizontal-only dashed lines and forwards every Recharts CartesianGrid prop.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **…gridProps** | - | - | Every Recharts CartesianGrid prop is forwarded as-is. See the Recharts CartesianGrid documentation. |



### Tooltip


The hover tooltip. It reads the chart's selection state so its content dims unselected series.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style of the tooltip surface. |
| **roundness** | - | - | Controls the border-radius of the tooltip. |
| **defaultIndex** | `number` | - | When set, the tooltip is visible by default at the specified data point index. |
| **cursor** | `boolean` | `true` | Whether the vertical cursor line follows the pointer on hover. |



### Legend


The series legend. When `isClickable` is set, each entry toggles selection of its series.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style of the legend indicators. |
| **align** | - | - | Horizontal placement of the legend. |
| **verticalAlign** | - | - | Vertical placement of the legend. |
| **isClickable** | `boolean` | `false` | Lets each legend entry toggle selection of its series. |

---

## Bar Chart <a name="bar-chart"></a>

> Simple static & beautifully designed bar charts

#### Live Example Code

##### Example File: `ex-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleBarChart() {
  return (
    <EvilBarChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      xDataKey="month"
      showBrush
      brushFormatLabel={(value) => String(value).substring(0, 3)}
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Bar dataKey="desktop" variant="default" isClickable />
      <Bar dataKey="mobile" variant="default" isClickable />
    </EvilBarChart>
  );
}
```

## Installation


  
    
    
  
  
### CLI Installation


    ```bash
npx shadcn@latest add @evilcharts/bar-chart
```
  
  
### Manual Installation


    
      
        #### Install the following dependencies:

        
          ```bash
npm install recharts
npm install motion
```
        
      
      
        #### Copy and paste the following code snippets into your project.

         
          To use the chart, first create the folder `evilcharts` and a subfolder called `charts` inside your `components` directory.
          Then, copy the following base bar-chart code into a new file in that folder.
        
        
          #### Component Implementation

##### Source File: `bar-chart.tsx`

```tsx
"use client";

import {
  type ChartConfig,
  ChartContainer,
  getColorsCount,
  getLoadingData,
  LoadingIndicator,
} from "@/registry/ui/chart";
import { EvilBrush, useEvilBrush, type EvilBrushRange } from "@/registry/ui/evil-brush";
import {
  ChartTooltip,
  ChartTooltipContent,
  type TooltipRoundness,
  type TooltipVariant,
} from "@/registry/ui/tooltip";
import { ChartLegend, ChartLegendContent, type ChartLegendVariant } from "@/registry/ui/legend";
import { ChartBackground, type BackgroundVariant } from "@/registry/ui/background";
import {
  createContext,
  use,
  useCallback,
  useId,
  useMemo,
  useRef,
  useState,
  type ComponentProps,
  type ReactNode,
} from "react";
import {
  Bar as RechartsBar,
  BarChart as RechartsBarChart,
  CartesianGrid,
  Rectangle,
  ReferenceLine,
  XAxis as RechartsXAxis,
  YAxis as RechartsYAxis,
} from "recharts";
import { RectRadius } from "recharts/types/shape/Rectangle";
import { motion, useReducedMotion } from "motion/react";

// Constants
const DEFAULT_BAR_RADIUS = 2;
const LOADING_BAR_DATA_KEY = "loading";
const LOADING_ANIMATION_DURATION = 2000; // in milliseconds
const STACK_ID = "evil-stacked";
const BAR_GROW_DURATION = 0.5; // per-bar grow-in length, in seconds
const BAR_STAGGER = 0.05; // delay between consecutive bars, in seconds
const REVEAL_EASE: [number, number, number, number] = [0, 0.7, 0.5, 1]; // grow-in easing

type BarVariant = "default" | "hatched" | "duotone" | "duotone-reverse" | "gradient" | "stripped";
type StackType = "default" | "stacked" | "percent";
type BarLayout = "vertical" | "horizontal";

/**
 * Order in which bars grow into view. Recharts' own bar animation is permanently
 * disabled — every bar instead grows from its baseline (bottom for vertical
 * layout, left for horizontal), and this controls the stagger sequence.
 *
 * NOTE: the grow-in is a per-frame animation, so it is heavier than a static
 * chart. `"none"` opts out entirely; it is also what a device with the OS
 * "reduce motion" preference falls back to automatically.
 */
type BarAnimationType = "none" | "left-to-right" | "right-to-left" | "center-out" | "edges-in";

// ─────────────────────────────────────────────────────────────────────────────
// Shared context
// ─────────────────────────────────────────────────────────────────────────────

/**
 * Shared state for every part of the chart. Lifted into <EvilBarChart /> so that
 * <Bar />, <XAxis />, <Legend />, and friends can read it without prop drilling.
 * Sub-components are composed freely — the provider is the single source of truth.
 */
type BarChartContextValue = {
  config: ChartConfig; // colors + labels for every series
  isStacked: boolean; // whether bars stack on top of each other
  isHorizontal: boolean; // whether bars are laid out horizontally
  isLoading: boolean; // whether the chart shows its loading skeleton
  barRadius: number; // default corner radius each <Bar /> inherits
  animationType: BarAnimationType; // default grow-in order each <Bar /> inherits
  introStartedAt: number; // timestamp the chart mounted — anchors the one-shot grow-in
  dataLength: number; // number of rows currently rendered
  selectedDataKey: string | null; // currently selected series, or null when none
  selectDataKey: (dataKey: string | null) => void; // sets the selected series
  isMouseInChart: boolean; // whether the pointer is currently over the chart
};

const BarChartContext = createContext<BarChartContextValue | null>(null);

// Reads the chart context, throwing a helpful error when used outside <EvilBarChart />
function useBarChart() {
  const context = use(BarChartContext);

  if (!context) {
    throw new Error(
      "Bar chart parts (<Bar />, <XAxis />, …) must be used within <EvilBarChart />",
    );
  }

  return context;
}

// ─────────────────────────────────────────────────────────────────────────────
// Root container
// ─────────────────────────────────────────────────────────────────────────────

// Validates that every config key also exists on the data row type
type ValidateConfigKeys<TData, TConfig> = {
  [K in keyof TConfig]: K extends keyof TData ? ChartConfig[string] : never;
};

type EvilBarChartBaseProps<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
> = {
  config: TConfig & ValidateConfigKeys<TData, TConfig>; // series colors + labels
  data: TData[]; // rows rendered by the chart
  children: ReactNode; // composed parts — <Bar />, <XAxis />, <Legend />, …
  className?: string; // extra classes for the chart container
  chartProps?: ComponentProps<typeof RechartsBarChart>; // escape hatch for the raw Recharts chart
  stackType?: StackType; // how multiple bars combine
  layout?: BarLayout; // orientation of the bars
  barRadius?: number; // default corner radius for every <Bar />
  animationType?: BarAnimationType; // default grow-in order for every <Bar />
  barGap?: number; // gap between bars within the same category
  barCategoryGap?: number; // gap between categories of bars
  backgroundVariant?: BackgroundVariant; // background pattern drawn behind the chart
  defaultSelectedDataKey?: string | null; // series selected on first render
  onSelectionChange?: (selectedDataKey: string | null) => void; // fires when the selected series changes
  isLoading?: boolean; // shows the animated loading skeleton
  loadingBars?: number; // number of bars in the loading skeleton
  showBrush?: boolean; // renders a zoom brush below the chart
  xDataKey?: keyof TData & string; // x-axis key — only needed for the brush footer
  brushHeight?: number; // height of the brush preview in pixels
  brushFormatLabel?: (value: unknown, index: number) => string; // formats brush axis labels
  onBrushChange?: (range: EvilBrushRange) => void; // fires when the brush range changes
};

type EvilBarChartProps<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
> = EvilBarChartBaseProps<TData, TConfig>;

/**
 * Root of the composible bar chart. Owns the data, the shared context, the
 * loading skeleton, and the optional zoom brush. Everything visual — axes,
 * grid, tooltip, legend, and the bars themselves — is composed as children,
 * so a consumer renders exactly the parts they need.
 */
export function EvilBarChart<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
>({
  config,
  data,
  children,
  className,
  chartProps,
  stackType = "default",
  layout = "vertical",
  barRadius = DEFAULT_BAR_RADIUS,
  animationType = "left-to-right",
  barGap,
  barCategoryGap,
  backgroundVariant,
  defaultSelectedDataKey = null,
  onSelectionChange,
  isLoading = false,
  loadingBars,
  showBrush = false,
  xDataKey,
  brushHeight,
  brushFormatLabel,
  onBrushChange,
}: EvilBarChartProps<TData, TConfig>) {
  const chartId = useId().replace(/:/g, ""); // colon-free id keeps CSS/SVG selectors valid
  // Anchors the grow-in to a fixed moment so it plays exactly once — re-renders
  // and Recharts' bar remounts read elapsed time from here instead of replaying.
  // Lazy useState stamps the time once, on the initial render only.
  const [introStartedAt] = useState(() => Date.now());
  const [selectedDataKey, setSelectedDataKey] = useState<string | null>(defaultSelectedDataKey);
  const [isMouseInChart, setIsMouseInChart] = useState(false);
  const { loadingData, onShimmerExit } = useLoadingData(isLoading, loadingBars);
  const { visibleData, brushProps } = useEvilBrush({ data });

  const isStacked = stackType === "stacked" || stackType === "percent";
  const isHorizontal = layout === "horizontal";
  const displayData = showBrush && !isLoading ? visibleData : data;

  // Updates selection state and notifies the parent
  const selectDataKey = useCallback(
    (newSelectedDataKey: string | null) => {
      setSelectedDataKey(newSelectedDataKey);
      onSelectionChange?.(newSelectedDataKey);
    },
    [onSelectionChange],
  );

  const contextValue = useMemo<BarChartContextValue>(
    () => ({
      config,
      isStacked,
      isHorizontal,
      isLoading,
      barRadius,
      animationType,
      introStartedAt,
      dataLength: displayData.length,
      selectedDataKey,
      selectDataKey,
      isMouseInChart,
    }),
    [
      config,
      isStacked,
      isHorizontal,
      isLoading,
      barRadius,
      animationType,
      introStartedAt,
      displayData.length,
      selectedDataKey,
      selectDataKey,
      isMouseInChart,
    ],
  );

  return (
    <BarChartContext value={contextValue}>
      <ChartContainer
        className={className}
        config={config}
        footer={
          showBrush &&
          !isLoading && (
            <EvilBrush
              data={data}
              chartConfig={config}
              xDataKey={xDataKey}
              variant="bar"
              barRadius={barRadius}
              height={brushHeight}
              formatLabel={brushFormatLabel}
              stacked={isStacked}
              skipStyle
              className="mt-1"
              {...brushProps}
              onChange={(range) => {
                brushProps.onChange(range);
                onBrushChange?.(range);
              }}
            />
          )
        }
      >
        <LoadingIndicator isLoading={isLoading} />
        <RechartsBarChart
          id={chartId}
          accessibilityLayer
          layout={isHorizontal ? "vertical" : "horizontal"}
          data={isLoading ? loadingData : displayData}
          barGap={barGap}
          barCategoryGap={barCategoryGap}
          stackOffset={stackType === "percent" ? "expand" : undefined}
          onMouseEnter={() => setIsMouseInChart(true)}
          onMouseLeave={() => setIsMouseInChart(false)}
          {...chartProps}
        >
          {backgroundVariant && <ChartBackground variant={backgroundVariant} />}
          <ReferenceLine color="white" />
          {children}
          {isLoading && <LoadingBar chartId={chartId} onShimmerExit={onShimmerExit} />}
        </RechartsBarChart>
      </ChartContainer>
    </BarChartContext>
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Composible parts
// ─────────────────────────────────────────────────────────────────────────────

type BarProps = {
  dataKey: string; // series key — must exist on the data and config
  variant?: BarVariant; // fill style for this bar only
  radius?: number; // corner radius — falls back to the chart default
  animationType?: BarAnimationType; // grow-in order — falls back to the chart default
  isClickable?: boolean; // lets this bar be selected by clicking it
  enableHoverHighlight?: boolean; // dims this bar while another bar is hovered
  glowing?: boolean; // applies a soft outer glow to this bar
  bufferBar?: boolean; // renders the last data point as a hatched "buffer" bar
  barProps?: ComponentProps<typeof RechartsBar>; // escape hatch for raw Recharts Bar props
};

/**
 * A single bar series. Each <Bar /> is fully self-contained: it generates its
 * own gradient/pattern definitions under a unique id, so any number of bars —
 * each with its own variant, radius, glow, and clickability — can live in one
 * chart without style collisions.
 */
export function Bar({
  dataKey,
  variant = "default",
  radius,
  animationType,
  isClickable = false,
  enableHoverHighlight = false,
  glowing = false,
  bufferBar = false,
  barProps,
}: BarProps) {
  const {
    config,
    isStacked,
    isHorizontal,
    isLoading,
    barRadius: defaultRadius,
    animationType: defaultAnimation,
    introStartedAt,
    dataLength,
    selectedDataKey,
    selectDataKey,
    isMouseInChart,
  } = useBarChart();
  const id = useId().replace(/:/g, ""); // unique id scopes this bar's style defs
  // Devices set to "reduce motion" skip the grow-in animation entirely
  const shouldReduceMotion = useReducedMotion();

  // The root renders the skeleton bar while loading, so real bars step aside
  if (isLoading) return null;

  const resolvedRadius = radius ?? defaultRadius;
  const isSelected = selectedDataKey === dataKey;

  // The grow-in is a per-frame animation — heavier than a static chart — so
  // `"none"` and the OS reduce-motion preference both opt out of it.
  const revealType: BarAnimationType = shouldReduceMotion
    ? "none"
    : (animationType ?? defaultAnimation);

  const customBarProps = {
    id,
    dataKey,
    variant,
    barRadius: resolvedRadius,
    glowing,
    bufferBar,
    isClickable,
    enableHoverHighlight,
    isMouseInChart,
    isHorizontal,
    introStartedAt,
    selectedDataKey,
    dataLength,
    onClick: () => {
      if (!isClickable) return;
      // Clicking the selected bar clears the selection, otherwise selects it
      selectDataKey(isSelected ? null : dataKey);
    },
  };

  return (
    <>
      <RechartsBar
        dataKey={dataKey}
        stackId={isStacked ? STACK_ID : undefined}
        fill={`url(#${id}-colors-${dataKey})`}
        radius={resolvedRadius}
        // Recharts' built-in bar animation is permanently disabled — every bar
        // instead grows in from its baseline via the staggered motion.dev shape.
        isAnimationActive={false}
        style={isClickable || enableHoverHighlight ? { cursor: "pointer" } : undefined}
        shape={(props: unknown) => (
          <CustomBar {...(props as BarShapeProps)} {...customBarProps} animationType={revealType} />
        )}
        activeBar={(props: unknown) => (
          // The active (hovered) bar must never re-run the grow-in animation
          <CustomBar {...(props as BarShapeProps)} {...customBarProps} animationType="none" />
        )}
        {...barProps}
      />
      <defs>
        <ColorGradient id={id} dataKey={dataKey} config={config} />
        {variant === "hatched" && <HatchedPattern id={id} dataKey={dataKey} />}
        {variant === "duotone" && <DuotonePattern id={id} dataKey={dataKey} config={config} />}
        {variant === "duotone-reverse" && (
          <DuotoneReversePattern id={id} dataKey={dataKey} config={config} />
        )}
        {variant === "gradient" && <GradientPattern id={id} dataKey={dataKey} />}
        {variant === "stripped" && <StrippedPattern id={id} dataKey={dataKey} />}
        {bufferBar && <BufferHatchedPattern id={id} dataKey={dataKey} />}
        {glowing && <GlowFilter id={id} dataKey={dataKey} />}
      </defs>
    </>
  );
}

type XAxisProps = ComponentProps<typeof RechartsXAxis>;

/**
 * The category axis. Ships with the chart's flat default styling and forwards
 * every Recharts XAxis prop, so `dataKey`, `tickFormatter`, etc. are passed
 * straight through. Hidden automatically while the chart is loading. Resolves
 * its axis type from the chart layout — categorical when vertical, numeric
 * when the bars run horizontally.
 */
export function XAxis({
  tickLine = false,
  axisLine = false,
  tickMargin = 8,
  minTickGap = 8,
  type,
  ...props
}: XAxisProps) {
  const { isLoading, isHorizontal } = useBarChart();

  if (isLoading) return null;

  return (
    <RechartsXAxis
      tickLine={tickLine}
      axisLine={axisLine}
      tickMargin={tickMargin}
      minTickGap={minTickGap}
      type={type ?? (isHorizontal ? "number" : "category")}
      {...props}
    />
  );
}

type YAxisProps = ComponentProps<typeof RechartsYAxis>;

/**
 * The value axis. Forwards every Recharts YAxis prop and resolves its axis type
 * from the chart layout — numeric when vertical, categorical when the bars run
 * horizontally. Hidden automatically while the chart is loading.
 */
export function YAxis({
  tickLine = false,
  axisLine = false,
  tickMargin = 8,
  minTickGap = 8,
  width = "auto",
  type,
  ...props
}: YAxisProps) {
  const { isLoading, isHorizontal } = useBarChart();

  if (isLoading) return null;

  return (
    <RechartsYAxis
      tickLine={tickLine}
      axisLine={axisLine}
      tickMargin={tickMargin}
      minTickGap={minTickGap}
      width={width}
      type={type ?? (isHorizontal ? "category" : "number")}
      {...props}
    />
  );
}

type GridProps = ComponentProps<typeof CartesianGrid>;

/**
 * The background grid lines. Defaults to dashed lines aligned to the value
 * axis based on the chart layout, and forwards every Recharts CartesianGrid
 * prop for full control.
 */
export function Grid({ strokeDasharray = "3 3", vertical, horizontal, ...props }: GridProps) {
  const { isHorizontal } = useBarChart();

  return (
    <CartesianGrid
      strokeDasharray={strokeDasharray}
      vertical={vertical ?? isHorizontal}
      horizontal={horizontal ?? !isHorizontal}
      {...props}
    />
  );
}

type TooltipProps = {
  variant?: TooltipVariant; // visual style of the tooltip surface
  roundness?: TooltipRoundness; // border-radius of the tooltip
  defaultIndex?: number; // data index shown by default with no hover
};

/**
 * The hover tooltip. Reads the chart's selection from context so its content
 * dims unselected series. Hidden automatically while the chart is loading.
 */
export function Tooltip({ variant, roundness, defaultIndex }: TooltipProps) {
  const { isLoading, selectedDataKey } = useBarChart();

  if (isLoading) return null;

  return (
    <ChartTooltip
      cursor={false}
      defaultIndex={defaultIndex}
      content={
        <ChartTooltipContent selected={selectedDataKey} roundness={roundness} variant={variant} />
      }
    />
  );
}

type LegendProps = {
  variant?: ChartLegendVariant; // visual style of the legend indicators
  align?: "left" | "center" | "right"; // horizontal placement
  verticalAlign?: "top" | "middle" | "bottom"; // vertical placement
  isClickable?: boolean; // lets each entry toggle selection of its series
};

/**
 * The series legend. When `isClickable` is set, each entry toggles selection of
 * its series, driving the shared selection state read by every <Bar />.
 */
export function Legend({
  variant,
  align = "right",
  verticalAlign = "top",
  isClickable = false,
}: LegendProps) {
  const { selectedDataKey, selectDataKey } = useBarChart();

  return (
    <ChartLegend
      verticalAlign={verticalAlign}
      align={align}
      content={
        <ChartLegendContent
          selected={selectedDataKey}
          onSelectChange={selectDataKey}
          isClickable={isClickable}
          variant={variant}
        />
      }
    />
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Custom bar shape
// ─────────────────────────────────────────────────────────────────────────────

// Raw geometry Recharts hands to a custom bar shape
type BarShapeProps = {
  x?: number;
  y?: number;
  width?: number;
  height?: number;
  fill?: string;
  fillOpacity?: number;
  dataKey?: string;
  index?: number;
  [key: string]: unknown;
};

// Per-series config the <Bar /> threads into every CustomBar render
type CustomBarProps = {
  id: string;
  dataKey: string;
  variant: BarVariant;
  barRadius: number;
  glowing?: boolean;
  bufferBar?: boolean;
  isClickable?: boolean;
  enableHoverHighlight?: boolean;
  isMouseInChart?: boolean;
  isHorizontal?: boolean;
  animationType?: BarAnimationType;
  introStartedAt?: number;
  selectedDataKey?: string | null;
  isActive?: boolean;
  dataLength?: number;
  onClick?: () => void;
} & BarShapeProps;

/**
 * Custom bar shape. Renders the visible bar painted by the owning <Bar />'s
 * variant pattern, with an invisible full-height rectangle behind it to keep
 * the whole column hoverable and clickable.
 */
const CustomBar = (props: CustomBarProps) => {
  const {
    x = 0,
    y = 0,
    width = 0,
    height = 0,
    id,
    dataKey,
    variant,
    barRadius,
    glowing,
    bufferBar,
    isClickable,
    enableHoverHighlight,
    isMouseInChart,
    isHorizontal = false,
    animationType = "none",
    introStartedAt = 0,
    selectedDataKey,
    isActive,
    dataLength = 0,
    onClick,
  } = props;

  const index = typeof props.index === "number" ? props.index : -1;
  const isLastBar = bufferBar && dataLength > 0 && index === dataLength - 1;
  const isStripped = variant === "stripped";
  const grow = getBarGrowAnimation(animationType, index, dataLength, isHorizontal, introStartedAt);

  const fill = isLastBar
    ? `url(#${id}-buffer-hatched-${dataKey})`
    : getVariantFill(variant, id, dataKey);
  const filter = glowing ? `url(#${id}-bar-glow-${dataKey})` : undefined;

  const fillOpacity = getBarOpacity({
    isClickable,
    selectedDataKey,
    dataKey,
    enableHoverHighlight,
    isMouseInChart,
    isActive,
  });
  const cursorStyle = isClickable || enableHoverHighlight ? { cursor: "pointer" } : undefined;

  // Stripped bars round only their top corners; every other variant rounds all four
  const radius: RectRadius = isStripped ? [barRadius, barRadius, 0, 0] : barRadius;

  // The visible, painted bar — plus the stripped variant's solid top strip
  const visibleBar = (
    <>
      <Rectangle
        x={x}
        y={y}
        width={width}
        opacity={fillOpacity}
        height={Math.max(0, height - 3)}
        radius={radius}
        fill={fill}
        filter={filter}
        stroke={isLastBar ? `url(#${id}-colors-${dataKey})` : undefined}
        strokeWidth={isLastBar ? 1 : undefined}
      />
      {isStripped && (
        <Rectangle
          x={x}
          y={y - 4}
          width={width}
          height={2}
          radius={1}
          fill={`url(#${id}-colors-${dataKey})`}
        />
      )}
    </>
  );

  return (
    <g style={cursorStyle} onClick={onClick}>
      {/* Full-height invisible rect keeps the whole column hoverable/clickable */}
      <Rectangle {...props} fill="transparent" />
      {/* The painted bar grows in from its baseline; the hit rect above stays put */}
      {grow ? (
        <motion.g
          initial={grow.initial}
          animate={grow.animate}
          transition={grow.transition}
          style={grow.style}
        >
          {visibleBar}
        </motion.g>
      ) : (
        visibleBar
      )}
    </g>
  );
};

/**
 * Builds the motion.dev grow-in animation for a single bar, or returns `null`
 * when the bar should render statically (`"none"`, reduced motion, an unknown
 * index, or — crucially — once the bar has already finished growing).
 *
 * Every bar grows from its baseline — `scaleY` from the bottom for vertical
 * layout, `scaleX` from the left for horizontal — and `animationType` decides
 * the stagger order, so the chart fills in one bar at a time.
 *
 * The intro is anchored to `introStartedAt` (stamped once when the chart
 * mounts) rather than to component mount. Recharts remounts every bar whenever
 * the chart re-renders — e.g. on hover-highlight — so a mount-based animation
 * would replay endlessly. Reading elapsed time instead makes it a true
 * one-shot: a bar past its window renders static, and a bar caught mid-grow
 * resumes from the progress it should already be at.
 */
const getBarGrowAnimation = (
  animationType: BarAnimationType,
  index: number,
  dataLength: number,
  isHorizontal: boolean,
  introStartedAt: number,
) => {
  if (animationType === "none" || index < 0 || dataLength <= 0) return null;

  const lastIndex = dataLength - 1;
  const center = lastIndex / 2;

  // How many bars this one waits behind before it starts growing
  let step: number;
  switch (animationType) {
    case "right-to-left":
      step = lastIndex - index;
      break;
    case "center-out":
      step = Math.abs(index - center);
      break;
    case "edges-in":
      step = center - Math.abs(index - center);
      break;
    default: // left-to-right
      step = index;
  }

  const startMs = step * BAR_STAGGER * 1000;
  const durationMs = BAR_GROW_DURATION * 1000;
  const endMs = startMs + durationMs;
  const elapsed = Date.now() - introStartedAt;

  // Already finished — render static so re-renders/remounts can't replay it
  if (elapsed >= endMs) return null;

  // Resume from wherever this bar should already be: 0 before it starts,
  // partway through if a remount caught it mid-grow.
  const from = elapsed <= startMs ? 0 : (elapsed - startMs) / durationMs;
  const transition = {
    duration: (endMs - Math.max(elapsed, startMs)) / 1000,
    ease: REVEAL_EASE,
    delay: Math.max(0, startMs - elapsed) / 1000,
  };

  // Horizontal bars grow rightward from the left edge, vertical from the bottom
  return isHorizontal
    ? { initial: { scaleX: from }, animate: { scaleX: 1 }, transition, style: { originX: 0 } }
    : { initial: { scaleY: from }, animate: { scaleY: 1 }, transition, style: { originY: 1 } };
};

// ─────────────────────────────────────────────────────────────────────────────
// Selection + fill helpers
// ─────────────────────────────────────────────────────────────────────────────

// Resolves the SVG paint reference for a bar's fill based on its variant
const getVariantFill = (variant: BarVariant, id: string, dataKey: string): string => {
  switch (variant) {
    case "hatched":
      return `url(#${id}-hatched-${dataKey})`;
    case "duotone":
      return `url(#${id}-duotone-${dataKey})`;
    case "duotone-reverse":
      return `url(#${id}-duotone-reverse-${dataKey})`;
    case "gradient":
      return `url(#${id}-gradient-${dataKey})`;
    case "stripped":
      return `url(#${id}-stripped-${dataKey})`;
    default:
      return `url(#${id}-colors-${dataKey})`;
  }
};

// Computes bar opacity from the click selection and hover-highlight state
const getBarOpacity = ({
  isClickable,
  selectedDataKey,
  dataKey,
  enableHoverHighlight,
  isMouseInChart,
  isActive,
}: {
  isClickable?: boolean;
  selectedDataKey?: string | null;
  dataKey: string;
  enableHoverHighlight?: boolean;
  isMouseInChart?: boolean;
  isActive?: boolean;
}) => {
  const isSelectedDataKey = selectedDataKey === null || selectedDataKey === dataKey;
  const clickOpacity = isClickable && selectedDataKey !== null ? (isSelectedDataKey ? 1 : 0.3) : 1;

  // While hovering, the hovered bar keeps its click opacity and the rest dim further
  if (enableHoverHighlight && isMouseInChart) {
    return isActive ? clickOpacity : clickOpacity * 0.3;
  }

  return clickOpacity;
};

// ─────────────────────────────────────────────────────────────────────────────
// Style definitions — one set per <Bar />, scoped to its unique id
// ─────────────────────────────────────────────────────────────────────────────

type StyleProps = {
  id: string; // unique id of the owning <Bar />
  dataKey: string; // series key the colors belong to
};

/**
 * Vertical top-to-bottom color gradient for a series. Always rendered — every
 * fill variant and the buffer-bar stroke paint from this single gradient.
 */
const ColorGradient = ({
  id,
  dataKey,
  config,
}: StyleProps & { config: ChartConfig }) => {
  const colorsCount = getColorsCount(config[dataKey] ?? {});

  return (
    <linearGradient id={`${id}-colors-${dataKey}`} x1="0" y1="0" x2="0" y2="1">
      {colorsCount === 1 ? (
        <>
          <stop offset="0%" stopColor={`var(--color-${dataKey}-0)`} />
          <stop offset="100%" stopColor={`var(--color-${dataKey}-0)`} />
        </>
      ) : (
        Array.from({ length: colorsCount }, (_, index) => {
          const offset = `${(index / (colorsCount - 1)) * 100}%`;
          return (
            <stop
              key={offset}
              offset={offset}
              stopColor={`var(--color-${dataKey}-${index}, var(--color-${dataKey}-0))`}
            />
          );
        })
      )}
    </linearGradient>
  );
};

/** Diagonal hatched-stripe fill, masked from the series color gradient. */
const HatchedPattern = ({ id, dataKey }: StyleProps) => {
  return (
    <>
      <pattern
        id={`${id}-hatched-mask-pattern`}
        x="0"
        y="0"
        width="5"
        height="5"
        patternUnits="userSpaceOnUse"
        patternTransform="rotate(-45)"
      >
        <rect width="5" height="5" fill="white" fillOpacity={0.3} />
        <rect width="1.5" height="5" fill="white" fillOpacity={1} />
      </pattern>
      <mask id={`${id}-hatched-mask-${dataKey}`}>
        <rect width="100%" height="100%" fill={`url(#${id}-hatched-mask-pattern)`} />
      </mask>
      <pattern
        id={`${id}-hatched-${dataKey}`}
        patternUnits="userSpaceOnUse"
        width="100%"
        height="100%"
      >
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-colors-${dataKey})`}
          mask={`url(#${id}-hatched-mask-${dataKey})`}
        />
      </pattern>
    </>
  );
};

/** Hatched diagonal lines with no background fill, used for the buffer bar. */
const BufferHatchedPattern = ({ id, dataKey }: StyleProps) => {
  return (
    <>
      <pattern
        id={`${id}-buffer-hatched-mask-pattern`}
        x="0"
        y="0"
        width="5"
        height="5"
        patternUnits="userSpaceOnUse"
        patternTransform="rotate(-45)"
      >
        <rect width="5" height="5" fill="black" fillOpacity={0} />
        <rect width="1" height="5" fill="white" fillOpacity={1} />
      </pattern>
      <mask id={`${id}-buffer-hatched-mask-${dataKey}`}>
        <rect width="100%" height="100%" fill={`url(#${id}-buffer-hatched-mask-pattern)`} />
      </mask>
      <pattern
        id={`${id}-buffer-hatched-${dataKey}`}
        patternUnits="userSpaceOnUse"
        width="100%"
        height="100%"
      >
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-colors-${dataKey})`}
          mask={`url(#${id}-buffer-hatched-mask-${dataKey})`}
        />
      </pattern>
    </>
  );
};

/** Two-tone fill — a half-faded, half-solid split applied per bar bounding box. */
const DuotonePattern = ({ id, dataKey, config }: StyleProps & { config: ChartConfig }) => {
  const colorsCount = getColorsCount(config[dataKey] ?? {});

  return (
    <>
      <linearGradient
        id={`${id}-duotone-mask-gradient-${dataKey}`}
        gradientUnits="objectBoundingBox"
        x1="0"
        y1="0"
        x2="1"
        y2="0"
      >
        <stop offset="50%" stopColor="white" stopOpacity={0.4} />
        <stop offset="50%" stopColor="white" stopOpacity={1} />
      </linearGradient>
      <linearGradient
        id={`${id}-duotone-colors-${dataKey}`}
        gradientUnits="objectBoundingBox"
        x1="0"
        y1="0"
        x2="0"
        y2="1"
      >
        {colorsCount === 1 ? (
          <>
            <stop offset="0%" stopColor={`var(--color-${dataKey}-0)`} />
            <stop offset="100%" stopColor={`var(--color-${dataKey}-0)`} />
          </>
        ) : (
          Array.from({ length: colorsCount }, (_, index) => {
            const offset = `${(index / (colorsCount - 1)) * 100}%`;
            return (
              <stop
                key={offset}
                offset={offset}
                stopColor={`var(--color-${dataKey}-${index}, var(--color-${dataKey}-0))`}
              />
            );
          })
        )}
      </linearGradient>
      <mask id={`${id}-duotone-mask-${dataKey}`} maskContentUnits="objectBoundingBox">
        <rect
          x="0"
          y="0"
          width="1"
          height="1"
          fill={`url(#${id}-duotone-mask-gradient-${dataKey})`}
        />
      </mask>
      <pattern
        id={`${id}-duotone-${dataKey}`}
        patternUnits="objectBoundingBox"
        patternContentUnits="objectBoundingBox"
        width="1"
        height="1"
      >
        <rect
          x="0"
          y="0"
          width="1"
          height="1"
          fill={`url(#${id}-duotone-colors-${dataKey})`}
          mask={`url(#${id}-duotone-mask-${dataKey})`}
        />
      </pattern>
    </>
  );
};

/** Two-tone fill with the solid and faded halves reversed from `duotone`. */
const DuotoneReversePattern = ({ id, dataKey, config }: StyleProps & { config: ChartConfig }) => {
  const colorsCount = getColorsCount(config[dataKey] ?? {});

  return (
    <>
      <linearGradient
        id={`${id}-duotone-reverse-mask-gradient-${dataKey}`}
        gradientUnits="objectBoundingBox"
        x1="0"
        y1="0"
        x2="1"
        y2="0"
      >
        <stop offset="50%" stopColor="white" stopOpacity={1} />
        <stop offset="50%" stopColor="white" stopOpacity={0.4} />
      </linearGradient>
      <linearGradient
        id={`${id}-duotone-reverse-colors-${dataKey}`}
        gradientUnits="objectBoundingBox"
        x1="0"
        y1="0"
        x2="0"
        y2="1"
      >
        {colorsCount === 1 ? (
          <>
            <stop offset="0%" stopColor={`var(--color-${dataKey}-0)`} />
            <stop offset="100%" stopColor={`var(--color-${dataKey}-0)`} />
          </>
        ) : (
          Array.from({ length: colorsCount }, (_, index) => {
            const offset = `${(index / (colorsCount - 1)) * 100}%`;
            return (
              <stop
                key={offset}
                offset={offset}
                stopColor={`var(--color-${dataKey}-${index}, var(--color-${dataKey}-0))`}
              />
            );
          })
        )}
      </linearGradient>
      <mask id={`${id}-duotone-reverse-mask-${dataKey}`} maskContentUnits="objectBoundingBox">
        <rect
          x="0"
          y="0"
          width="1"
          height="1"
          fill={`url(#${id}-duotone-reverse-mask-gradient-${dataKey})`}
        />
      </mask>
      <pattern
        id={`${id}-duotone-reverse-${dataKey}`}
        patternUnits="objectBoundingBox"
        patternContentUnits="objectBoundingBox"
        width="1"
        height="1"
      >
        <rect
          x="0"
          y="0"
          width="1"
          height="1"
          fill={`url(#${id}-duotone-reverse-colors-${dataKey})`}
          mask={`url(#${id}-duotone-reverse-mask-${dataKey})`}
        />
      </pattern>
    </>
  );
};

/** Gradient fill that fades the series color from solid at the top to clear. */
const GradientPattern = ({ id, dataKey }: StyleProps) => {
  return (
    <>
      <linearGradient id={`${id}-gradient-mask-gradient`} x1="0" y1="0" x2="0" y2="1">
        <stop offset="20%" stopColor="white" stopOpacity={1} />
        <stop offset="90%" stopColor="white" stopOpacity={0} />
      </linearGradient>
      <mask id={`${id}-gradient-mask-${dataKey}`}>
        <rect width="100%" height="100%" fill={`url(#${id}-gradient-mask-gradient)`} />
      </mask>
      <pattern
        id={`${id}-gradient-${dataKey}`}
        patternUnits="userSpaceOnUse"
        width="100%"
        height="100%"
      >
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-colors-${dataKey})`}
          mask={`url(#${id}-gradient-mask-${dataKey})`}
        />
      </pattern>
    </>
  );
};

/** Low-opacity body fill, paired with a solid top strip drawn by CustomBar. */
const StrippedPattern = ({ id, dataKey }: StyleProps) => {
  return (
    <>
      <linearGradient id={`${id}-stripped-mask-gradient`} x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%" stopColor="white" stopOpacity={0.2} />
        <stop offset="100%" stopColor="white" stopOpacity={0.2} />
      </linearGradient>
      <mask id={`${id}-stripped-mask-${dataKey}`}>
        <rect width="100%" height="100%" fill={`url(#${id}-stripped-mask-gradient)`} />
      </mask>
      <pattern
        id={`${id}-stripped-${dataKey}`}
        patternUnits="userSpaceOnUse"
        width="100%"
        height="100%"
      >
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-colors-${dataKey})`}
          mask={`url(#${id}-stripped-mask-${dataKey})`}
        />
      </pattern>
    </>
  );
};

/** Soft outer-glow filter applied to a glowing bar. */
const GlowFilter = ({ id, dataKey }: StyleProps) => {
  return (
    <filter id={`${id}-bar-glow-${dataKey}`} x="-100%" y="-100%" width="300%" height="300%">
      <feGaussianBlur in="SourceGraphic" stdDeviation="8" result="blur" />
      <feColorMatrix
        in="blur"
        type="matrix"
        values="1 0 0 0 0
                0 1 0 0 0
                0 0 1 0 0
                0 0 0 0.5 0"
        result="glow"
      />
      <feMerge>
        <feMergeNode in="glow" />
        <feMergeNode in="SourceGraphic" />
      </feMerge>
    </filter>
  );
};

// ─────────────────────────────────────────────────────────────────────────────
// Loading skeleton
// ─────────────────────────────────────────────────────────────────────────────

// Builds bell-curve eased gradient stops for the loading shimmer
const generateEasedGradientStops = (
  steps: number = 17,
  minOpacity: number = 0.05,
  maxOpacity: number = 0.9,
) => {
  return Array.from({ length: steps }, (_, i) => {
    const t = i / (steps - 1); // 0 to 1
    // Sine-based bell curve easing: peaks at center (t=0.5), smooth falloff at edges
    const eased = Math.sin(t * Math.PI) ** 2;
    const opacity = minOpacity + eased * (maxOpacity - minOpacity);
    return { offset: `${(t * 100).toFixed(0)}%`, opacity: Number(opacity.toFixed(3)) };
  });
};

/**
 * Hook to manage loading data with pixel-perfect shimmer synchronization.
 *
 * Uses motion.dev's onUpdate callback to ensure chart data is only regenerated
 * when the shimmer has completely exited the visible area. This eliminates
 * timing drift issues from setTimeout/setInterval.
 */
export function useLoadingData(isLoading: boolean, loadingBars: number = 12) {
  const [loadingDataKey, setLoadingDataKey] = useState(false);

  // Callback fired by motion.dev when the shimmer exits the visible area
  const onShimmerExit = useCallback(() => {
    if (isLoading) {
      setLoadingDataKey((prev) => !prev);
    }
  }, [isLoading]);

  const loadingData = useMemo(
    () => getLoadingData(loadingBars, 20, 80),
    // loadingDataKey toggle triggers re-computation when the shimmer exits
    // eslint-disable-next-line react-hooks/exhaustive-deps
    [loadingBars, loadingDataKey],
  );

  return { loadingData, onShimmerExit };
}

/**
 * The skeleton bar shown while the chart is loading. Rendered by the root in
 * place of the real bars, paired with its own masked shimmer pattern.
 */
const LoadingBar = ({
  chartId,
  onShimmerExit,
}: {
  chartId: string;
  onShimmerExit: () => void;
}) => {
  return (
    <>
      <RechartsBar
        dataKey={LOADING_BAR_DATA_KEY}
        fill="currentColor"
        fillOpacity={0.15}
        radius={DEFAULT_BAR_RADIUS}
        isAnimationActive={false}
        legendType="none"
        style={{ mask: `url(#${chartId}-loading-mask)` }}
      />
      <defs>
        <LoadingBarPattern chartId={chartId} onShimmerExit={onShimmerExit} />
      </defs>
    </>
  );
};

/**
 * Animated shimmer pattern for the loading skeleton.
 *
 * The visible chart area is normalized to 0-1, the shimmer gradient has width 1,
 * and the pattern is 3x wide so the shimmer has buffer on both sides. The motion
 * rect travels x from -1 to 2; onShimmerExit fires as it crosses x=1, letting the
 * data swap happen while the shimmer is off-screen for a seamless loop.
 */
const LoadingBarPattern = ({
  chartId,
  onShimmerExit,
}: {
  chartId: string;
  onShimmerExit: () => void;
}) => {
  const gradientStops = generateEasedGradientStops();

  // 1 (left buffer) + 1 (visible) + 1 (right buffer)
  const patternWidth = 3;
  const startX = -1;
  const endX = 2;

  // Tracks the last x value to detect the exit threshold crossing
  const lastXRef = useRef(startX);

  return (
    <>
      <linearGradient id={`${chartId}-loading-mask-gradient`} x1="0" y1="0" x2="1" y2="0">
        {gradientStops.map(({ offset, opacity }) => (
          <stop key={offset} offset={offset} stopColor="white" stopOpacity={opacity} />
        ))}
      </linearGradient>
      <pattern
        id={`${chartId}-loading-mask-pattern`}
        patternUnits="objectBoundingBox"
        patternContentUnits="objectBoundingBox"
        patternTransform="rotate(25)"
        width={patternWidth}
        height="1"
        x="0"
        y="0"
      >
        <motion.rect
          y="0"
          width="1"
          height="1"
          fill={`url(#${chartId}-loading-mask-gradient)`}
          initial={{ x: startX }}
          animate={{ x: endX }}
          transition={{
            duration: LOADING_ANIMATION_DURATION / 1000,
            ease: "linear",
            repeat: Infinity,
            repeatType: "loop",
          }}
          onUpdate={(latest) => {
            const xValue = typeof latest.x === "number" ? latest.x : startX;
            const lastX = lastXRef.current;

            // Fire once per loop, when the shimmer fully exits the visible area
            if (xValue >= 1 && lastX < 1) {
              onShimmerExit();
            }

            lastXRef.current = xValue;
          }}
        />
      </pattern>
      <mask id={`${chartId}-loading-mask`} maskUnits="userSpaceOnUse">
        <rect width="100%" height="100%" fill={`url(#${chartId}-loading-mask-pattern)`} />
      </mask>
    </>
  );
};
```
        
      
       
        #### Now, Let's add the chart component to your project.

        
          These Components are required by the chart component to render the chart. Make a folder called `ui` inside the `evilcharts` folder and paste the code there.

          Below is main chart component.
        
        
          #### Component Implementation

##### Source File: `chart.tsx`

```tsx
"use client";

import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

// Format: { THEME_NAME: CSS_SELECTOR }
const THEMES = { light: "", dark: ".dark" } as const;

type ThemeKey = keyof typeof THEMES;

// All Keys are optional at first
type ThemeColorsBase = {
  [K in ThemeKey]?: string[];
};

// Require at least one theme key
type AtLeastOneThemeColor = {
  [K in ThemeKey]: Required<Pick<ThemeColorsBase, K>> & Partial<Omit<ThemeColorsBase, K>>;
}[ThemeKey];

const VALID_THEME_KEYS = Object.keys(THEMES) as ThemeKey[];

// Validation for chart config colors at runtime
function validateChartConfigColors(config: ChartConfig): void {
  for (const [key, value] of Object.entries(config)) {
    if (value.colors) {
      const hasValidThemeKey = VALID_THEME_KEYS.some(
        (themeKey) => value.colors?.[themeKey] !== undefined,
      );

      if (!hasValidThemeKey) {
        throw new Error(
          `[EvilCharts] Invalid chart config for "${key}": colors object must have at least one theme key (${VALID_THEME_KEYS.join(", ")}). Received empty object or invalid keys.`,
        );
      }
    }
  }
}

export type ChartConfig = Record<
  string,
  {
    label?: React.ReactNode;
    icon?: React.ComponentType;
    colors?: AtLeastOneThemeColor;
  }
>;

interface ChartContextProps {
  config: ChartConfig;
}

const ChartContext = React.createContext<ChartContextProps | null>(null);

export function useChart() {
  const context = React.useContext(ChartContext);

  if (!context) {
    throw new Error("useChart must be used within a <ChartContainer />");
  }

  return context;
}

interface ChartContainerProps
  extends
    Omit<React.ComponentProps<"div">, "children">,
    Pick<
      React.ComponentProps<typeof RechartsPrimitive.ResponsiveContainer>,
      | "initialDimension"
      | "aspect"
      | "debounce"
      | "minHeight"
      | "minWidth"
      | "maxHeight"
      | "height"
      | "width"
      | "onResize"
      | "children"
    > {
  config: ChartConfig;
  innerResponsiveContainerStyle?: React.ComponentProps<
    typeof RechartsPrimitive.ResponsiveContainer
  >["style"];
  /** Optional content rendered below the chart (e.g. EvilBrush) */
  footer?: React.ReactNode;
}

function ChartContainer({
  id,
  config,
  initialDimension = { width: 320, height: 200 },
  className,
  children,
  footer,
  ...props
}: Readonly<ChartContainerProps>) {
  const uniqueId = React.useId();
  const chartId = `chart-${id ?? uniqueId.replace(/:/g, "")}`;

  // Validate chart config at runtime
  validateChartConfigColors(config);

  return (
    <ChartContext.Provider value={{ config }}>
      <div
        data-slot="chart"
        data-chart={chartId}
        className={cn(
          "min-h-0 w-full flex-1",
          "[&_.recharts-cartesian-axis-tick_text]:fill-muted-foreground [&_.recharts-cartesian-grid_line[stroke='#ccc']]:stroke-border/50 [&_.recharts-curve.recharts-tooltip-cursor]:stroke-border [&_.recharts-polar-grid_[stroke='#ccc']]:stroke-border [&_.recharts-radial-bar-background-sector]:fill-muted [&_.recharts-rectangle.recharts-tooltip-cursor]:fill-muted [&_.recharts-reference-line_[stroke='#ccc']]:stroke-border relative flex flex-col justify-center text-xs [&_.recharts-dot[stroke='#fff']]:stroke-transparent [&_.recharts-layer]:outline-hidden [&_.recharts-sector]:outline-hidden [&_.recharts-sector[stroke='#fff']]:stroke-transparent [&_.recharts-surface]:outline-hidden",
          !footer && "aspect-video",
          className,
        )}
        {...props}
      >
        <ChartStyle id={chartId} config={config} />
        <RechartsPrimitive.ResponsiveContainer
          className="min-h-0 w-full flex-1"
          initialDimension={initialDimension}
        >
          {children}
        </RechartsPrimitive.ResponsiveContainer>
        {footer}
      </div>
    </ChartContext.Provider>
  );
}

function LoadingIndicator({ isLoading }: { isLoading: boolean }) {
  if (!isLoading) {
    return null;
  }

  return (
    <div className="pointer-events-none absolute inset-0 z-20 flex items-center justify-center">
      <div className="text-primary bg-background flex items-center justify-center gap-2 rounded-md border px-2 py-0.5 text-sm">
        <div className="border-border border-t-primary h-3 w-3 animate-spin rounded-full border" />
        <span>Loading</span>
      </div>
    </div>
  );
}

// Distribute colors evenly across slots, extra slots go to last color(s)
// Example: 2 colors for 4 slots → [red, red, pink, pink]
// Example: 3 colors for 4 slots → [red, pink, blue, blue]
function distributeColors(colorsArray: string[], maxCount: number): string[] {
  const availableCount = colorsArray.length;
  if (availableCount >= maxCount) {
    return colorsArray.slice(0, maxCount);
  }

  const result: string[] = [];
  const baseSlots = Math.floor(maxCount / availableCount);
  const extraSlots = maxCount % availableCount;

  // First (availableCount - extraSlots) colors get baseSlots each
  // Last extraSlots colors get (baseSlots + 1) each
  for (let colorIdx = 0; colorIdx < availableCount; colorIdx++) {
    const isExtraColor = colorIdx >= availableCount - extraSlots;
    const slotsForThisColor = baseSlots + (isExtraColor ? 1 : 0);
    for (let j = 0; j < slotsForThisColor; j++) {
      result.push(colorsArray[colorIdx]);
    }
  }

  return result;
}

const ChartStyle = ({ id, config }: { id: string; config: ChartConfig }) => {
  const colorConfig = Object.entries(config).filter(([, config]) => config.colors);

  if (!colorConfig.length) {
    return null;
  }

  const generateCssVars = (theme: keyof typeof THEMES) =>
    colorConfig
      .flatMap(([key, itemConfig]) => {
        const colorsArray = itemConfig.colors?.[theme];
        if (!colorsArray || !Array.isArray(colorsArray) || colorsArray.length === 0) {
          return [];
        }

        // Get max count across all themes for this key
        const maxCount = getColorsCount(itemConfig);

        // Distribute colors evenly across all required slots
        const distributedColors = distributeColors(colorsArray, maxCount);

        return distributedColors.map((color, index) => `  --color-${key}-${index}: ${color};`);
      })
      .filter(Boolean)
      .join("\n");

  const css = Object.entries(THEMES)
    .map(
      ([theme, prefix]) =>
        `${prefix} [data-chart=${id}] {\n${generateCssVars(theme as keyof typeof THEMES)}\n}`,
    )
    .join("\n");

  return <style dangerouslySetInnerHTML={{ __html: css }} />;
};

// Helper to extract item config from a payload.
export function getPayloadConfigFromPayload(config: ChartConfig, payload: unknown, key: string) {
  if (typeof payload !== "object" || payload === null) {
    return undefined;
  }

  const payloadPayload =
    "payload" in payload && typeof payload.payload === "object" && payload.payload !== null
      ? payload.payload
      : undefined;

  let configLabelKey: string = key;

  if (key in payload && typeof payload[key as keyof typeof payload] === "string") {
    configLabelKey = payload[key as keyof typeof payload] as string;
  } else if (
    payloadPayload &&
    key in payloadPayload &&
    typeof payloadPayload[key as keyof typeof payloadPayload] === "string"
  ) {
    configLabelKey = payloadPayload[key as keyof typeof payloadPayload] as string;
  }

  return configLabelKey in config ? config[configLabelKey] : config[key];
}

// Format values to percent for expanded charts
function axisValueToPercentFormatter(value: number) {
  return `${Math.round(value * 100).toFixed(0)}%`;
}

// Get max colors count across all themes for a config entry
function getColorsCount(config: ChartConfig[string]): number {
  if (!config.colors) return 1;
  const counts = VALID_THEME_KEYS.map((theme) => config.colors?.[theme]?.length ?? 0);
  return Math.max(...counts, 1);
}

// Generate random loading data for skeleton/loading state
// min/max represent percentage of the range (0-100), defaults to 20-80 for realistic look
export const getLoadingData = (points: number = 10, min: number = 0, max: number = 70) => {
  const range = max - min;
  return Array.from({ length: points }, () => ({
    loading: Math.floor(Math.random() * range) + min,
  }));
};

export {
  ChartContainer,
  ChartStyle,
  axisValueToPercentFormatter,
  LoadingIndicator,
  getColorsCount,
};
```
        
      
       
        #### Now, We need to add sub components.

        
          Create a file called `tooltip.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `tooltip.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import type { NameType, ValueType } from "recharts/types/component/DefaultTooltipContent";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type TooltipRoundness = "sm" | "md" | "lg" | "xl";
type TooltipVariant = "default" | "frosted-glass";

const roundnessMap: Record<TooltipRoundness, string> = {
  sm: "rounded-sm",
  md: "rounded-md",
  lg: "rounded-lg",
  xl: "rounded-xl",
};

const variantMap: Record<TooltipVariant, string> = {
  default: "bg-background",
  "frosted-glass": "bg-background/70 backdrop-blur-sm",
};

function ChartTooltipContent({
  active,
  payload,
  className,
  indicator = "dot",
  hideLabel = false,
  hideIndicator = false,
  label,
  labelFormatter,
  labelClassName,
  formatter,
  nameKey,
  labelKey,
  selected,
  roundness = "lg",
  variant = "default",
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip> &
  React.ComponentProps<"div"> & {
    hideLabel?: boolean;
    hideIndicator?: boolean;
    indicator?: "line" | "dot" | "dashed";
    nameKey?: string;
    labelKey?: string;
    selected?: string | null;
    roundness?: TooltipRoundness;
    variant?: TooltipVariant;
  } & Omit<
    RechartsPrimitive.DefaultTooltipContentProps<ValueType, NameType>,
    "accessibilityLayer"
  >) {
  const { config } = useChart();

  const tooltipLabel = React.useMemo(() => {
    if (hideLabel || !payload?.length) {
      return null;
    }

    const [item] = payload;
    const key = `${labelKey ?? item?.dataKey ?? item?.name ?? "value"}`;
    const itemConfig = getPayloadConfigFromPayload(config, item, key);
    const value =
      !labelKey && typeof label === "string" ? (config[label]?.label ?? label) : itemConfig?.label;

    if (labelFormatter) {
      return (
        <div className={cn("font-medium", labelClassName)}>{labelFormatter(value, payload)}</div>
      );
    }

    if (!value) {
      return null;
    }

    return <div className={cn("font-medium", labelClassName)}>{value}</div>;
  }, [label, labelFormatter, payload, hideLabel, labelClassName, config, labelKey]);

  if (!active || !payload?.length) {
    // Empty tooltip - to prevent position getting 0.0 so it doesnt animate tooltip every time from 0.0 origin
    return <span className="p-4" />;
  }

  const nestLabel = payload.length === 1 && indicator !== "dot";

  return (
    <div
      className={cn(
        "border-border/50 grid min-w-32 items-start gap-1.5 border px-2.5 py-1.5 text-xs shadow-xl",
        roundnessMap[roundness],
        variantMap[variant],
        className,
      )}
    >
      {!nestLabel ? tooltipLabel : null}
      <div className="grid gap-1.5">
        {payload
          .filter((item) => item.type !== "none")
          .map((item, index) => {
            // For pie charts, item.name contains the sector name (e.g., "chrome")
            // For radial charts, the name is in item.payload[nameKey]
            // For other charts, item.name or item.dataKey contains the series name
            const payloadName =
              nameKey && item.payload
                ? (item.payload as Record<string, unknown>)[nameKey]
                : undefined;
            const key = `${payloadName ?? item.name ?? item.dataKey ?? "value"}`;
            const itemConfig = getPayloadConfigFromPayload(config, item, key);

            // Get colors count for this item to determine gradient vs solid
            const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

            return (
              <div
                key={index}
                className={cn(
                  "[&>svg]:text-muted-foreground flex w-full flex-wrap items-stretch gap-2 [&>svg]:h-2.5 [&>svg]:w-2.5",
                  indicator === "dot" && "items-center",
                  selected != null && selected !== item.dataKey && "opacity-30",
                )}
              >
                {formatter && item?.value !== undefined && item.name ? (
                  formatter(item.value, item.name, item, index, item.payload)
                ) : (
                  <>
                    {itemConfig?.icon ? (
                      <itemConfig.icon />
                    ) : (
                      !hideIndicator && (
                        <div
                          className={cn("shrink-0 rounded-[2px]", {
                            "h-2.5 w-2.5": indicator === "dot",
                            "w-1": indicator === "line",
                            "w-0 border-[1.5px] border-dashed bg-transparent!":
                              indicator === "dashed",
                            "my-0.5": nestLabel && indicator === "dashed",
                          })}
                          style={getIndicatorColorStyle(key, colorsCount)}
                        />
                      )
                    )}
                    <div
                      className={cn(
                        "flex flex-1 justify-between gap-4 leading-none",
                        nestLabel ? "items-end" : "items-center",
                      )}
                    >
                      <div className="grid gap-1.5">
                        {nestLabel ? tooltipLabel : null}
                        <span className="text-muted-foreground">
                          {itemConfig?.label ?? item.name}
                        </span>
                      </div>
                      {item.value != null && (
                        <span className="text-foreground font-mono font-medium tabular-nums">
                          {typeof item.value === "number"
                            ? item.value.toLocaleString()
                            : String(item.value)}
                        </span>
                      )}
                    </div>
                  </>
                )}
              </div>
            );
          })}
      </div>
    </div>
  );
}

function getIndicatorColorStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { background: `var(--color-${dataKey}-0)` };
  }

  // Multiple colors: create linear gradient with evenly distributed stops
  const stops = Array.from({ length: colorsCount }, (_, index) => {
    const offset = (index / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${index}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

const ChartTooltip = ({
  animationDuration = 200,
  ...props
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip>) => (
  <RechartsPrimitive.Tooltip animationDuration={animationDuration} {...props} />
);

export { ChartTooltip, ChartTooltipContent };
export type { TooltipRoundness, TooltipVariant };
```
        
        
          Now, create another file called `legend.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `legend.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type ChartLegendVariant =
  | "square"
  | "circle"
  | "circle-outline"
  | "rounded-square"
  | "rounded-square-outline"
  | "vertical-bar"
  | "horizontal-bar";

function ChartLegendContent({
  className,
  hideIcon = false,
  nameKey,
  payload,
  verticalAlign,
  align = "right",
  selected,
  onSelectChange,
  isClickable,
  variant = "rounded-square",
}: React.ComponentProps<"div"> & {
  hideIcon?: boolean;
  nameKey?: string;
  selected?: string | null;
  isClickable?: boolean;
  onSelectChange?: (selected: string | null) => void;
  variant?: ChartLegendVariant;
} & RechartsPrimitive.DefaultLegendContentProps) {
  const { config } = useChart();

  if (!payload?.length) {
    return null;
  }

  return (
    <div
      className={cn(
        "flex items-center gap-4 select-none",
        align === "left" && "justify-start",
        align === "center" && "justify-center",
        align === "right" && "justify-end",
        verticalAlign === "top" ? "pb-4" : "pt-4",
        className,
      )}
    >
      {payload
        .filter((item) => item.type !== "none")
        .map((item) => {
          // For pie charts, item.value contains the sector name (e.g., "chrome")
          // For radial charts, the name is in item.payload[nameKey]
          // For other charts, item.dataKey contains the series name (e.g., "desktop")
          const payloadName =
            nameKey && item.payload
              ? (item.payload as Record<string, unknown>)[nameKey]
              : undefined;
          const key = `${payloadName ?? item.value ?? item.dataKey ?? "value"}`;
          const itemConfig = getPayloadConfigFromPayload(config, item, key);
          const isSelected = selected === null || selected === key;

          // Get colors count for this item to determine gradient vs solid
          const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

          return (
            <div
              key={key}
              className={cn(
                "[&>svg]:text-muted-foreground flex items-center gap-1.5 transition-opacity [&>svg]:h-3 [&>svg]:w-3",
                !isSelected && "opacity-30",
                isClickable && "cursor-pointer",
              )}
              onClick={() => {
                if (!isClickable) return;

                onSelectChange?.(selected === key ? null : key);
              }}
            >
              {itemConfig?.icon && !hideIcon ? (
                <itemConfig.icon />
              ) : (
                <LegendIndicator
                  variant={variant}
                  dataKey={key}
                  colorsCount={colorsCount}
                />
              )}
              {itemConfig?.label}
            </div>
          );
        })}
    </div>
  );
}

// ---------------------------------------------------------------------------
// Legend indicator — each variant gets its own branch so future variants
// can diverge freely in markup & style.
// ---------------------------------------------------------------------------

function LegendIndicator({
  variant,
  dataKey,
  colorsCount,
}: {
  variant: ChartLegendVariant;
  dataKey: string;
  colorsCount: number;
}) {
  const fillStyle = getLegendFillStyle(dataKey, colorsCount);
  const outlineStyle = getLegendOutlineStyle(dataKey, colorsCount);

  switch (variant) {
    case "square":
      return <div className="h-2 w-2 shrink-0" style={fillStyle} />;

    case "circle":
      return <div className="h-2 w-2 shrink-0 rounded-full" style={fillStyle} />;

    case "circle-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-full p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "vertical-bar":
      return <div className="h-3 w-1 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "horizontal-bar":
      return <div className="h-1 w-3 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "rounded-square-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-[3px] p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "rounded-square":
    default:
      return <div className="h-2 w-2 shrink-0 rounded-[2px]" style={fillStyle} />;
  }
}

// ---------------------------------------------------------------------------
// Style helpers
// ---------------------------------------------------------------------------

/** Solid fill / gradient background for filled variants. */
function getLegendFillStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { backgroundColor: `var(--color-${dataKey}-0)` };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

/**
 * Outline style for stroke variants.
 * Uses background + mask-composite to punch out the center, leaving only the
 * "border" visible. Works with both solid colors and gradients, and respects
 * border-radius — unlike plain `border-color`.
 */
function getLegendOutlineStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  const maskStyle: React.CSSProperties = {
    WebkitMask:
      "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    WebkitMaskComposite: "xor",
    mask: "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    maskComposite: "exclude",
  };

  if (colorsCount <= 1) {
    return {
      backgroundColor: `var(--color-${dataKey}-0)`,
      ...maskStyle,
    };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return {
    background: `linear-gradient(to right, ${stops})`,
    ...maskStyle,
  };
}

const ChartLegend = RechartsPrimitive.Legend;

export { ChartLegend, ChartLegendContent, type ChartLegendVariant };
```
        
      
    
  


## Usage

The bar chart is composible. `<EvilBarChart>` is the container, and you compose only the parts you need — `<Grid>`, `<XAxis>`, `<YAxis>`, `<Legend>`, `<Tooltip>`, and one or more `<Bar>` — as its children. Each `<Bar>` carries its own `variant`, `radius`, `glowing`, `bufferBar`, and `isClickable`, so a single chart can mix fill styles and make only some series interactive.

```tsx
import {
  EvilBarChart,
  Bar,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
} from "@/components/evilcharts/charts/bar-chart";
```

```tsx
<EvilBarChart data={data} config={chartConfig} stackType="default">
  <Grid />
  <XAxis dataKey="month" />
  <Legend isClickable />
  <Tooltip />
  <Bar dataKey="desktop" variant="default" isClickable />
  <Bar dataKey="mobile" variant="hatched" isClickable />
</EvilBarChart>
```

### Interactive Selection

Add `isClickable` to any `<Bar>` (and to `<Legend>`) to make those series selectable. Use the `onSelectionChange` callback on `<EvilBarChart>` to handle selection events:

```tsx
<EvilBarChart
  data={data}
  config={chartConfig}
  onSelectionChange={(selectedDataKey) => {
    if (selectedDataKey) {
      console.log("Selected:", selectedDataKey);
    } else {
      console.log("Deselected");
    }
  }}
>
  <XAxis dataKey="month" />
  <Legend isClickable />
  <Tooltip />
  <Bar dataKey="desktop" variant="default" isClickable />
  <Bar dataKey="mobile" variant="default" isClickable />
</EvilBarChart>
```

### Loading State

#### Live Example Code

##### Example File: `ex-loading-state-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleBarChart() {
  return (
    <EvilBarChart
      data={[]} // if isLoading is true, pass empty array → i.e isLoading ? [] : data
      config={chartConfig}
      className="h-full w-full p-4"
      isLoading={true} // [!code highlight]
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend />
      <Tooltip />
      <Bar dataKey="desktop" variant="default" />
      <Bar dataKey="mobile" variant="default" />
    </EvilBarChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    The bar chart supports loading state with a shimmer animation. You can pass the `isLoading` prop to the chart to show the loading state while your data is being fetched.
  


### Buffer Bar

" name="ex-buffer-bar-chart"  />
> [!NOTE]
 
  > [!NOTE]

    When a `<Bar>` has `bufferBar` set, its last data point is rendered with a diagonal lines (hatched) pattern while the rest stay solid. This is useful for indicating projected, estimated, or incomplete data at the end of a series — commonly seen in financial charts and forecasting dashboards.
  


## Examples

Below are some examples of the bar chart with different `variants`. Each `<Bar>` carries its own `variant`, and the chart-wide `stackType` and `layout` shape the rest.

### Hover Highlight

" name="ex-hover-highlight-bar-chart"  />
> [!NOTE]
 
  > [!NOTE]

    The hover highlight feature dims other bars when you hover over a bar, making it easier to focus on specific data points. Set the `enableHoverHighlight` prop on each `<Bar>` to enable this feature.
  


### Gradient Colors

#### Live Example Code

##### Example File: `ex-gradient-colors-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#a855f7", "#6366f1", "#3b82f6"], // [!code highlight]
      dark: ["#f43f5e", "#ec4899", "#a855f7", "#6366f1", "#3b82f6"], // [!code highlight]
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#10b981", "#34d399", "#6ee7b7"], // [!code highlight]
      dark: ["#10b981", "#14b8a6", "#06b6d4"], // [!code highlight]
    },
  },
} satisfies ChartConfig;

export function EvilExampleBarChart() {
  return (
    <EvilBarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Bar dataKey="desktop" variant="default" isClickable />
      <Bar dataKey="mobile" variant="default" isClickable />
    </EvilBarChart>
  );
}
```

### Bar Variants

#### Live Example Code

##### Example File: `ex-default-variant-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleBarChart() {
  return (
    <EvilBarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Bar
        dataKey="desktop"
        variant="default" // [!code highlight]
        isClickable
      />
      <Bar
        dataKey="mobile"
        variant="default" // [!code highlight]
        isClickable
      />
    </EvilBarChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-hatched-variant-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleBarChart() {
  return (
    <EvilBarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Bar
        dataKey="desktop"
        variant="hatched" // [!code highlight]
        isClickable
      />
      <Bar
        dataKey="mobile"
        variant="hatched" // [!code highlight]
        isClickable
      />
    </EvilBarChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-duotone-variant-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleBarChart() {
  return (
    <EvilBarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Bar
        dataKey="desktop"
        variant="duotone" // [!code highlight]
        isClickable
      />
      <Bar
        dataKey="mobile"
        variant="duotone" // [!code highlight]
        isClickable
      />
    </EvilBarChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-duotone-reverse-variant-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleBarChart() {
  return (
    <EvilBarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Bar
        dataKey="desktop"
        variant="duotone-reverse" // [!code highlight]
        isClickable
      />
      <Bar
        dataKey="mobile"
        variant="duotone-reverse" // [!code highlight]
        isClickable
      />
    </EvilBarChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-gradient-variant-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleBarChart() {
  return (
    <EvilBarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Bar
        dataKey="desktop"
        variant="gradient" // [!code highlight]
        isClickable
      />
      <Bar
        dataKey="mobile"
        variant="gradient" // [!code highlight]
        isClickable
      />
    </EvilBarChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-stripped-variant-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleBarChart() {
  return (
    <EvilBarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Bar
        dataKey="desktop"
        variant="stripped" // [!code highlight]
        isClickable
      />
      <Bar
        dataKey="mobile"
        variant="stripped" // [!code highlight]
        isClickable
      />
    </EvilBarChart>
  );
}
```

### Stack Types

#### Live Example Code

##### Example File: `ex-stacked-type-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleBarChart() {
  return (
    <EvilBarChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="stacked" // [!code highlight]
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Bar dataKey="desktop" variant="default" isClickable />
      <Bar dataKey="mobile" variant="default" isClickable />
    </EvilBarChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-percent-type-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleBarChart() {
  return (
    <EvilBarChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      stackType="percent" // [!code highlight]
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Bar dataKey="desktop" variant="default" isClickable />
      <Bar dataKey="mobile" variant="default" isClickable />
    </EvilBarChart>
  );
}
```

### Horizontal Layout

#### Live Example Code

##### Example File: `ex-horizontal-layout-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, YAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 186 },
  { month: "February", desktop: 305 },
  { month: "March", desktop: 237 },
  { month: "April", desktop: 173 },
  { month: "May", desktop: 209 },
  { month: "June", desktop: 214 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#2563eb"],
      dark: ["#3b82f6"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleBarChart() {
  return (
    <EvilBarChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      layout="horizontal" // [!code highlight]
    >
      <Grid />
      <YAxis
        dataKey="month"
        tickFormatter={(value) => value.substring(0, 3)} // [!code highlight]
      />
      <Legend />
      <Tooltip />
      <Bar dataKey="desktop" variant="default" />
    </EvilBarChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    Use `layout="horizontal"` on `<EvilBarChart>` to display bars horizontally. In horizontal mode, the `<YAxis>` shows categories while the `<XAxis>` shows values — pass a `tickFormatter` to `<YAxis>` for category formatting.
  


### Glowing Bars

 - desktop" name="ex-glowing-desktop-bar-chart"  />
 - mobile" name="ex-glowing-mobile-bar-chart"  />

## API Reference

The chart is composed of several parts. The props below are grouped by the component they belong to.

### EvilBarChart


The root container. It owns the data, the shared selection state, the loading skeleton, and the optional brush. Everything visual is composed as its children.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **data*** | `TData[]` | - | Data used to display the chart. An array of objects where each object represents a data point (`TData extends Record`). |
| **config** | - | - | " required>     Configuration object that defines the chart's series. Each key should match a data key in your data array, with a corresponding color or color array. |
| **children*** | `ReactNode` | - | The composed chart parts — ``, ``, ``, ``, ``, and one or more ``. |
| **className** | `string` | - | Additional CSS classes to apply to the chart container. |
| **stackType** | - | - | Controls how multiple bars combine. `"default"` renders bars side by side, `"stacked"` stacks them on top of each other, and `"percent"` normalizes them to show percentage distribution. |
| **layout** | - | - | The orientation of the bars. `"vertical"` displays bars vertically, `"horizontal"` displays bars horizontally — in which case the `` shows categories and the `` shows values. |
| **barRadius** | `number` | `2` | The default corner radius for every ``, in pixels. Each `` may override it locally with its own `radius` prop. |
| **animationType** | - | - | Order in which bars grow into view, inherited by every ``. Each bar grows from its baseline (bottom for vertical layout, left for horizontal). `"none"` disables it — devices with the OS reduce-motion preference fall back to `"none"` automatically. |
| **barGap** | `number` | - | The gap between bars in the same category (when using multiple series). |
| **barCategoryGap** | `number` | - | The gap between different categories of bars. |
| **backgroundVariant** | `BackgroundVariant` | - | Background pattern variant to display behind the chart. |
| **defaultSelectedDataKey** | `string | null` | `null` | The data key that should be selected by default. |
| **onSelectionChange** | - | - | void">     Callback fired when a series is selected or deselected — by clicking a clickable `` or `` entry. Receives the selected data key, or `null` when deselected. |
| **isLoading** | `boolean` | `false` | Shows a loading skeleton animation with a shimmer effect when data is being fetched. |
| **loadingBars** | `number` | `12` | Number of bars to display in the loading skeleton. |
| **showBrush** | `boolean` | `false` | When enabled, displays a brush control below the chart for selecting and zooming into a range of data. |
| **xDataKey** | `keyof TData & string` | - | The data key used for the x-axis. Only needed by the brush footer — the axis itself reads its key from ``. |
| **brushHeight** | `number` | - | The height of the brush preview area in pixels. |
| **brushFormatLabel** | - | - | string">     Custom formatter for the brush axis labels. |
| **onBrushChange** | - | - | void">     Callback invoked when the user changes the brush selection range. |
| **chartProps** | - | - | ">     Additional props forwarded to the underlying Recharts BarChart component. Read the Recharts BarChart documentation for available props. |



### Bar


A single bar series. Each `<Bar />` is self-contained and generates its own gradient/pattern definitions, so a chart can hold any number of bars — each with its own variant, radius, glow, and clickability.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **dataKey*** | `string` | - | The series key. Must exist on both the data rows and the chart `config`. |
| **variant** | - | - | The visual style of the bar fill. Applies to this bar only. |
| **radius** | `number` | - | The corner radius of this bar, in pixels. Falls back to the chart's `barRadius` when omitted. |
| **animationType** | - | - | The grow-in order for this bar series. Falls back to the chart's `animationType` when omitted. |
| **isClickable** | `boolean` | `false` | Lets this bar be selected by clicking it. When any bar is selected, unselected bars become semi-transparent. |
| **enableHoverHighlight** | `boolean` | `false` | When enabled, hovering over a bar dims this bar while another is hovered, making it easier to focus on specific data points. |
| **glowing** | `boolean` | `false` | Applies a soft outer glow to this bar series. |
| **bufferBar** | `boolean` | `false` | When enabled, renders this series' last data point with a diagonal lines (hatched) pattern while the rest stay solid. Useful for indicating projected or incomplete data at the end of a series. |
| **barProps** | - | - | ">     Escape hatch for raw props forwarded to the underlying Recharts Bar component. |



### XAxis and YAxis


The category and value axes. Both ship with the chart's flat default styling and forward every Recharts axis prop, so `dataKey`, `tickFormatter`, `tickMargin`, etc. pass straight through. They are hidden automatically while the chart is loading, and each resolves its axis `type` from the chart `layout` — categorical or numeric — unless you set `type` explicitly.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **dataKey** | `string` | - | The data key for the axis values. |
| **…axisProps** | - | - | Every other Recharts XAxis / YAxis prop is forwarded as-is. Read the Recharts XAxis and Recharts YAxis documentation for available props. |



### Grid


The background grid lines. Defaults to dashed lines aligned to the value axis based on the chart layout, and forwards every Recharts CartesianGrid prop.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **…gridProps** | - | - | Every Recharts CartesianGrid prop is forwarded as-is. Read the Recharts CartesianGrid documentation for available props. |



### Tooltip


The hover tooltip. It reads the chart's selection state so its content dims unselected series.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style of the tooltip surface. |
| **roundness** | - | - | Controls the border-radius of the tooltip. |
| **defaultIndex** | `number` | - | When set, the tooltip is visible by default at the specified data point index. |



### Legend


The series legend. When `isClickable` is set, each entry toggles selection of its series.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style of the legend indicators. |
| **align** | - | - | Horizontal placement of the legend. |
| **verticalAlign** | - | - | Vertical placement of the legend. |
| **isClickable** | `boolean` | `false` | Lets each legend entry toggle selection of its series. |

---

## Bar Blocks <a name="bar-blocks"></a>

> Simple static & beautifully designed bar charts

## Monospace Bar Chart

#### Live Example Code

##### Example File: `b-monospace-bar-chart.tsx`

```tsx
"use client";

import { type ChartConfig, ChartContainer } from "@/registry/ui/chart";
import { Bar, BarChart, Rectangle, XAxis } from "recharts";
import { motion, AnimatePresence } from "motion/react";

const chartData = [
  { month: "January", desktop: 342 },
  { month: "February", desktop: 876 },
  { month: "March", desktop: 512 },
  { month: "April", desktop: 629 },
  { month: "May", desktop: 458 },
  { month: "June", desktop: 781 },
  { month: "July", desktop: 394 },
  { month: "August", desktop: 925 },
  { month: "September", desktop: 647 },
  { month: "October", desktop: 532 },
  { month: "November", desktop: 803 },
  { month: "December", desktop: 271 },
  { month: "January", desktop: 342 },
  { month: "February", desktop: 876 },
  { month: "March", desktop: 512 },
  { month: "April", desktop: 629 },
  { month: "May", desktop: 458 },
  { month: "June", desktop: 781 },
  { month: "July", desktop: 394 },
  { month: "August", desktop: 925 },
  { month: "September", desktop: 647 },
  { month: "October", desktop: 532 },
  { month: "November", desktop: 803 },
  { month: "December", desktop: 271 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#18181b"],
      dark: ["#fafafa"],
    },
  },
} satisfies ChartConfig;

export function EvilMonospaceBarChart() {
  return (
    <div className="flex h-full flex-col p-4">
      <div className="flex flex-row justify-between">
        <div className="flex flex-row">
          <div className="flex flex-col gap-2">
            <span className="text-muted-foreground font-mono text-xs">{"[$] Total Sales"}</span>
            <span className="text-primary font-mono text-3xl">
              <span className="text-muted-foreground text-xl font-normal">$</span>
              <span className="tracking-tighter">14,340</span>
            </span>
          </div>
          <hr className="mx-4 h-full border-l border-dashed" />
          <div className="flex flex-col gap-2">
            <span className="text-muted-foreground font-mono text-xs">{"[⬆] Top Month"}</span>
            <span className="text-primary font-mono text-3xl">
              <span className="tracking-tighter">June</span>
            </span>
          </div>
        </div>
        <div className="flex flex-col justify-end gap-1">
          <span className="text-muted-foreground font-mono text-[10px]">
            {"// X-AXIS: "}
            <span className="text-primary">MONTHS</span>
          </span>
          <span className="text-muted-foreground font-mono text-[10px]">
            {"// Y-AXIS: "}
            <span className="text-primary">SALES</span>
          </span>
        </div>
      </div>
      <hr className="my-4 border-t border-dashed" />
      <ChartContainer config={chartConfig}>
        <BarChart accessibilityLayer data={chartData}>
          <XAxis
            dataKey="month"
            tickLine={false}
            tickMargin={10}
            axisLine={false}
            tickFormatter={(value) => value.slice(0, 3)}
          />
          {Object.keys(chartConfig).map((key) => (
            <Bar
              key={key}
              dataKey={key}
              fill={`var(--color-${key}-0)`}
              shape={BarShape}
              activeBar={BarShape}
            />
          ))}
        </BarChart>
      </ChartContainer>
    </div>
  );
}

interface BarProps {
  index?: number;
  value?: number | [number, number];
  x?: number;
  y?: number;
  width?: number;
  height?: number;
  fill?: string;
  isActive?: boolean;
}

// Scale factor: collapsed = thin line, expanded = full width
const COLLAPSED_SCALE = 0.1; // [!code highlight]

const BarShape = (props: BarProps) => {
  const { fill, x, y, width, height, index, value, isActive } = props;

  const xPos = Number(x || 0);
  const yPos = Number(y || 0);
  const realWidth = Number(width || 0);
  const realHeight = Number(height || 0);

  // Center position for the bar
  const centerX = xPos + realWidth / 2;
  const centerY = yPos + realHeight / 2;

  return (
    <>
      <Rectangle {...props} fill="transparent" />

      <AnimatePresence>
        <motion.rect
          key={`bar-${index}`}
          x={xPos}
          y={yPos}
          width={realWidth}
          height={realHeight}
          fill={fill}
          initial={{ scaleX: isActive ? COLLAPSED_SCALE : 1 }}
          animate={{ scaleX: isActive ? 1 : COLLAPSED_SCALE }}
          exit={{ scaleX: COLLAPSED_SCALE }}
          transition={{ type: "spring", stiffness: 200, damping: 25 }}
          style={{
            transformOrigin: `${centerX}px ${centerY}px`,
            transformBox: "fill-box",
          }}
        />
      </AnimatePresence>
      {isActive && (
        <AnimatePresence>
          <motion.text
            className="font-mono"
            key={`text-${index}`}
            initial={{ opacity: 0, y: -10, filter: "blur(3px)" }}
            animate={{ opacity: 1, y: 0, filter: "blur(0px)" }}
            exit={{ opacity: 0, y: -10, filter: "blur(3px)" }}
            transition={{ duration: 0.2 }}
            x={centerX}
            y={yPos - 5}
            textAnchor="middle"
            fill={fill}
            style={{ pointerEvents: "none" }}
          >
            {value}
          </motion.text>
        </AnimatePresence>
      )}
    </>
  );
};
```
```bash
npx shadcn@latest add @evilcharts/monospace-bar-chart
```

## Hover Trace Bar Chart

#### Live Example Code

##### Example File: `b-hover-trace-bar-chart.tsx`

```tsx
"use client";

import {
  Bar,
  BarChart,
  Rectangle,
  ReferenceLine,
  Tooltip,
  XAxis,
  type BarShapeProps,
  type CartesianViewBox,
} from "recharts";
import { type ChartConfig, ChartContainer } from "@/registry/ui/chart";
import { useMotionValueEvent, useSpring } from "motion/react";
import NumberFlow from "@number-flow/react";
import * as React from "react";

const CHART_MARGIN = 38;

const chartData = [
  { month: "January", desktop: 342 },
  { month: "February", desktop: 676 },
  { month: "March", desktop: 512 },
  { month: "April", desktop: 629 },
  { month: "May", desktop: 458 },
  { month: "June", desktop: 781 },
  { month: "July", desktop: 394 },
  { month: "August", desktop: 924 },
  { month: "September", desktop: 647 },
  { month: "October", desktop: 532 },
  { month: "November", desktop: 803 },
  { month: "December", desktop: 271 },
  { month: "January", desktop: 342 },
  { month: "February", desktop: 876 },
  { month: "March", desktop: 512 },
  { month: "April", desktop: 629 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#18181b"],
      dark: ["#fafafa"],
    },
  },
} satisfies ChartConfig;

export function EvilHoverTraceBarChart() {
  const [activeIndex, setActiveIndex] = React.useState<number | null>(null);

  const maxData = React.useMemo(
    () =>
      chartData.reduce(
        (max, item, index) =>
          item.desktop > max.value ? { index, month: item.month, value: item.desktop } : max,
        { index: 0, month: chartData[0].month, value: chartData[0].desktop },
      ),
    [],
  );

  const selectedData =
    activeIndex != null && chartData[activeIndex]
      ? {
          index: activeIndex,
          month: chartData[activeIndex].month,
          value: chartData[activeIndex].desktop,
        }
      : maxData;

  const valueSpring = useSpring(selectedData.value, {
    stiffness: 110,
    damping: 20,
  });
  const [springValue, setSpringValue] = React.useState(selectedData.value);

  const handleBarHover = React.useCallback(
    (index: number) => {
      setActiveIndex(index);
      valueSpring.set(chartData[index]?.desktop ?? maxData.value);
    },
    [maxData.value, valueSpring],
  );

  useMotionValueEvent(valueSpring, "change", (latest) => {
    setSpringValue(Math.round(latest));
  });

  return (
    <div className="flex h-full flex-col p-4">
      <div className="mb-4 flex items-end justify-between">
        <div className="space-y-1">
          <p className="text-muted-foreground font-mono text-xs">{"[desktop] Value"}</p>
          <p className="text-primary font-mono text-3xl tracking-tighter">
            <NumberFlow
              value={selectedData.value}
              format={{ style: "currency", currency: "USD", currencyDisplay: "narrowSymbol" }}
            />
          </p>
        </div>

        <div className="space-y-1 text-right">
          <p className="text-muted-foreground font-mono text-[10px]">{"[month]"}</p>
          <p className="text-primary font-mono text-xs">{selectedData.month}</p>
        </div>
      </div>

      <ChartContainer config={chartConfig}>
        <BarChart
          accessibilityLayer
          data={chartData}
          margin={{ left: CHART_MARGIN }}
          onMouseMove={(state) => {
            if (state?.activeTooltipIndex != null) {
              handleBarHover(Number(state.activeTooltipIndex));
            }
          }}
          onMouseLeave={() => {
            setActiveIndex(null);
            valueSpring.set(maxData.value);
          }}
        >
          <XAxis
            dataKey="month"
            tickLine={false}
            tickMargin={10}
            axisLine={false}
            tickFormatter={(value: string) => value.slice(0, 3)}
          />

          <Tooltip cursor={false} content={() => null} />

          <Bar
            dataKey="desktop"
            fill="var(--color-desktop-0)"
            radius={4}
            shape={(props: BarShapeProps) => (
              <HoverTraceBarShape {...props} highlightedIndex={selectedData.index} />
            )}
            activeBar={(props: BarShapeProps) => (
              <HoverTraceBarShape {...props} highlightedIndex={selectedData.index} />
            )}
          />

          <ReferenceLine
            y={springValue}
            stroke="var(--foreground)"
            strokeDasharray="3 3"
            label={<HoverTraceLabel value={selectedData.value} />}
          />
        </BarChart>
      </ChartContainer>
    </div>
  );
}

interface HoverTraceLabelProps {
  viewBox?: CartesianViewBox;
  value: number;
}

const HoverTraceLabel = ({ viewBox, value }: HoverTraceLabelProps) => {
  const x = viewBox?.x ?? 0;
  const y = viewBox?.y ?? 0;
  const formattedValue = value.toLocaleString();
  const width = formattedValue.length * 8 + 12;

  return (
    <>
      <rect
        x={x - CHART_MARGIN}
        y={y - 9}
        width={width}
        height={18}
        fill="var(--foreground)"
        rx={4}
      />
      <text
        className="font-mono text-[11px]"
        fontWeight={600}
        x={x - CHART_MARGIN + 7}
        y={y + 4}
        fill="var(--background)"
      >
        {formattedValue}
      </text>
      <ellipse cx={"99.5%"} cy={y} rx={3} ry={3} fill="var(--foreground)" />
    </>
  );
};

type HoverTraceBarShapeProps = BarShapeProps & {
  highlightedIndex: number;
};

const HoverTraceBarShape = (props: HoverTraceBarShapeProps) => {
  const { x, y, width, height, fill, index, isActive, highlightedIndex } = props;

  const fillOpacity = isActive || index === highlightedIndex ? 1 : 0.2;

  return (
    <g>
      <Rectangle {...props} fill="transparent" pointerEvents="all" />
      <Rectangle
        x={x}
        y={y}
        width={width}
        height={height}
        radius={4}
        fill={fill}
        fillOpacity={fillOpacity}
        stroke={isActive ? "var(--foreground)" : undefined}
        strokeOpacity={isActive ? 0.35 : undefined}
        strokeWidth={isActive ? 1 : undefined}
        className="transition-opacity duration-200"
      />
    </g>
  );
};
```
```bash
npx shadcn@latest add @evilcharts/hover-trace-bar-chart
```

## Grid Bar Chart

#### Live Example Code

##### Example File: `b-grid-bar-chart.tsx`

```tsx
"use client";

import { type ChartConfig, ChartContainer } from "@/registry/ui/chart";
import { Bar, BarChart, XAxis } from "recharts";
const SQUARE_SIZE = 10;
const GAP = 2;
const CELL_SIZE = SQUARE_SIZE + GAP;

const chartData = [
  { month: "January", desktop: 186 },
  { month: "February", desktop: 305 },
  { month: "March", desktop: 237 },
  { month: "April", desktop: 273 },
  { month: "May", desktop: 209 },
  { month: "June", desktop: 346 },
  { month: "July", desktop: 181 },
  { month: "August", desktop: 392 },
  { month: "September", desktop: 298 },
  { month: "October", desktop: 215 },
  { month: "November", desktop: 327 },
  { month: "December", desktop: 162 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#18181b"],
      dark: ["#fafafa"],
    },
  },
} satisfies ChartConfig;

interface GridBarProps {
  x?: number;
  y?: number;
  width?: number;
  height?: number;
  fill?: string;
}

// Renders ghost squares filling the entire column height — used as the Bar background.
// recharts passes y = chart top, height = full chart area height, so y + height = the
// shared bottom baseline, which perfectly aligns with the data squares in GridBarShape.
const GridBarBackground = (props: GridBarProps) => {
  const { fill, x, y, width, height } = props;

  const xPos = Number(x ?? 0);
  const yPos = Number(y ?? 0);
  const realWidth = Number(width ?? 0);
  const realHeight = Number(height ?? 0);

  if (realHeight <= 0) return null;

  const numSquares = Math.floor(realHeight / CELL_SIZE);
  const squareSize = Math.min(SQUARE_SIZE, Math.max(2, realWidth - 2));
  const squareX = xPos + Math.floor((realWidth - squareSize) / 2);
  const bottomY = yPos + realHeight;

  return (
    <>
      {Array.from({ length: numSquares }, (_, i) => {
        const squareY = bottomY - (i + 1) * CELL_SIZE + GAP;
        return (
          <rect
            className="dark:opacity-[0.1]"
            key={i}
            x={squareX}
            y={squareY}
            width={squareSize}
            height={squareSize}
            fill={fill}
          />
        );
      })}
    </>
  );
};

const GridBarShape = (props: GridBarProps) => {
  const { fill, x, y, width, height } = props;

  const xPos = Number(x ?? 0);
  const yPos = Number(y ?? 0);
  const realWidth = Number(width ?? 0);
  const realHeight = Number(height ?? 0);

  if (realHeight <= 0) return null;

  const numSquares = Math.max(1, Math.floor(realHeight / CELL_SIZE));
  const squareSize = Math.min(SQUARE_SIZE, Math.max(2, realWidth - 2));
  const squareX = xPos + Math.floor((realWidth - squareSize) / 2);
  const bottomY = yPos + realHeight;

  return (
    <>
      {Array.from({ length: numSquares }, (_, i) => {
        const squareY = bottomY - (i + 1) * CELL_SIZE + GAP;
        return (
          <rect
            key={i}
            x={squareX}
            y={squareY}
            width={squareSize}
            height={squareSize}
            fill={fill}
          />
        );
      })}
    </>
  );
};

export function EvilGridBarChart() {
  const total = chartData.reduce((sum, item) => sum + item.desktop, 0);
  const maxData = chartData.reduce(
    (max, item, index) =>
      item.desktop > max.value ? { index, month: item.month, value: item.desktop } : max,
    { index: 0, month: chartData[0].month, value: chartData[0].desktop },
  );

  return (
    <div className="flex h-full flex-col p-4">
      <div className="flex flex-row justify-between">
        <div className="flex flex-row">
          <div className="flex flex-col gap-2">
            <span className="text-muted-foreground font-mono text-xs">{"[Σ] Total"}</span>
            <span className="text-primary font-mono text-3xl tracking-tighter">
              {total.toLocaleString()}
            </span>
          </div>
          <hr className="mx-4 h-full border-l border-dashed" />
          <div className="flex flex-col gap-2">
            <span className="text-muted-foreground font-mono text-xs">{"[⬆] Peak"}</span>
            <span className="text-primary font-mono text-3xl tracking-tighter">
              {maxData.month.slice(0, 3)}
            </span>
          </div>
        </div>
        <div className="flex flex-col justify-end gap-1">
          <span className="text-muted-foreground font-mono text-[10px]">
            {"// CELL: "}
            <span className="text-primary">10x10px</span>
          </span>
          <span className="text-muted-foreground font-mono text-[10px]">
            {"// TYPE: "}
            <span className="text-primary">GRID</span>
          </span>
        </div>
      </div>
      <hr className="my-4 border-t border-dashed" />
      <ChartContainer config={chartConfig}>
        <BarChart accessibilityLayer data={chartData}>
          <XAxis
            dataKey="month"
            tickLine={false}
            tickMargin={10}
            axisLine={false}
            tickFormatter={(value: string) => value.slice(0, 3)}
          />
          {Object.keys(chartConfig).map((key) => (
            <Bar
              key={key}
              dataKey={key}
              fill={`var(--color-${key}-0)`}
              background={GridBarBackground}
              shape={GridBarShape}
              activeBar={GridBarShape}
            />
          ))}
        </BarChart>
      </ChartContainer>
    </div>
  );
}
```
```bash
npx shadcn@latest add @evilcharts/grid-bar-chart
```

## Isometric Bar Chart

#### Live Example Code

##### Example File: `b-isometric-bar-chart.tsx`

```tsx
"use client";

import * as React from "react";
import { Bar, BarChart, XAxis, YAxis } from "recharts";
import { motion } from "motion/react";
import { type ChartConfig, ChartContainer } from "@/registry/ui/chart";
import { ChartTooltip, ChartTooltipContent } from "@/registry/ui/tooltip";

const chartData = [
  { month: "January", revenue: 28 },
  { month: "February", revenue: 34 },
  { month: "March", revenue: 22 },
  { month: "April", revenue: 41 },
  { month: "May", revenue: 47 },
  { month: "June", revenue: 31 },
  { month: "July", revenue: 38 },
];

const chartConfig = {
  revenue: {
    label: "Revenue",
    colors: {
      light: ["#18181b"],
      dark: ["#fafafa"],
    },
  },
} satisfies ChartConfig;

const DX = 10;
const DY = 10;

const BEVEL_OPACITY = 0.55;

const FILLED = true;

const DIRECTION: "left" | "right" = "right";

const HIGHLIGHT_COLOR = "#22c55e";
const HIGHLIGHT_COLOR_DARK = "#15803d";

interface ShapeProps {
  x?: number;
  y?: number;
  width?: number;
  height?: number;
  index?: number;
  payload?: { month: string; revenue: number };
}

function IsoBar({
  x,
  y,
  width,
  height,
  index,
  payload,
  maxValue,
  idPrefix,
}: ShapeProps & { maxValue: number; idPrefix: string }) {
  const bx = Number(x ?? 0);
  const by = Number(y ?? 0);
  const bw = Number(width ?? 0);
  const bh = Number(height ?? 0);

  if (bh <= 0) return null;

  const highlight = payload?.revenue === maxValue;
  const dx = DIRECTION === "left" ? -DX : DX;
  const sideX = DIRECTION === "left" ? bx : bx + bw;
  const topPoints = `${bx},${by} ${bx + bw},${by} ${bx + bw + dx},${by - DY} ${bx + dx},${by - DY}`;
  const sidePoints = `${sideX},${by} ${sideX + dx},${by - DY} ${sideX + dx},${by + bh - DY} ${sideX},${by + bh}`;

  // Gradient/pattern ids are namespaced per chart instance so multiple
  // charts on the same page don't share (and clobber) each other's <defs>.
  const url = (name: string) => `url(#${idPrefix}-${name})`;

  const strokeColor = highlight ? HIGHLIGHT_COLOR_DARK : "var(--color-accent)";

  const frontFill = FILLED
    ? highlight
      ? url("iso-front-accent")
      : url("iso-front-base")
    : "none";
  const topFill = FILLED
    ? highlight
      ? url("iso-top-accent")
      : url("iso-top-base")
    : "none";
  const rightFill = FILLED
    ? highlight
      ? url("iso-right-accent")
      : url("iso-right-base")
    : "none";
  const hatchFill = highlight ? url("iso-hatch-accent") : url("iso-hatch-base");

  return (
    <motion.g
      initial={{ scaleY: 0, opacity: 0 }}
      animate={{ scaleY: 1, opacity: 1 }}
      transition={{
        duration: 0.7,
        delay: (index ?? 0) * 0.08,
        ease: [0.16, 1, 0.3, 1],
      }}
      style={{ transformBox: "fill-box", transformOrigin: "50% 100%" }}
    >
      <polygon
        points={sidePoints}
        fill={rightFill}
        stroke={strokeColor}
        strokeWidth={FILLED ? 0 : 1}
      />
      <polygon
        points={topPoints}
        fill={topFill}
        stroke={strokeColor}
        strokeWidth={FILLED ? 0 : 1}
      />
      <rect
        x={bx}
        y={by}
        width={bw}
        height={bh}
        fill={frontFill}
        stroke={strokeColor}
        strokeWidth={FILLED ? 0 : 1}
      />
      {FILLED && (
        <rect x={bx} y={by} width={bw} height={bh} fill={hatchFill} />
      )}
      {FILLED && highlight && (
        <rect x={bx} y={by} width={2} height={bh} fill="rgba(0,0,0,0.15)" />
      )}
    </motion.g>
  );
}

function IsoBarDefs({ idPrefix }: { idPrefix: string }) {
  return (
    <defs>
      <linearGradient id={`${idPrefix}-iso-front-base`} x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%" stopColor="var(--color-accent)" stopOpacity={1} />
        <stop offset="100%" stopColor="var(--color-accent)" stopOpacity={0.8} />
      </linearGradient>
      <linearGradient id={`${idPrefix}-iso-top-base`} x1="0" y1="0" x2="1" y2="0">
        <stop offset="0%" stopColor="var(--color-accent)" stopOpacity={BEVEL_OPACITY} />
        <stop offset="100%" stopColor="var(--color-accent)" stopOpacity={BEVEL_OPACITY * 0.9} />
      </linearGradient>
      <linearGradient id={`${idPrefix}-iso-right-base`} x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%" stopColor="var(--color-accent)" stopOpacity={BEVEL_OPACITY * 0.7} />
        <stop offset="100%" stopColor="var(--color-accent)" stopOpacity={BEVEL_OPACITY * 0.55} />
      </linearGradient>

      <linearGradient id={`${idPrefix}-iso-front-accent`} x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%" stopColor={HIGHLIGHT_COLOR} stopOpacity={1} />
        <stop offset="100%" stopColor={HIGHLIGHT_COLOR_DARK} stopOpacity={0.95} />
      </linearGradient>
      <linearGradient id={`${idPrefix}-iso-top-accent`} x1="0" y1="0" x2="1" y2="0">
        <stop offset="0%" stopColor={HIGHLIGHT_COLOR} stopOpacity={BEVEL_OPACITY + 0.15} />
        <stop offset="100%" stopColor={HIGHLIGHT_COLOR} stopOpacity={BEVEL_OPACITY} />
      </linearGradient>
      <linearGradient id={`${idPrefix}-iso-right-accent`} x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%" stopColor={HIGHLIGHT_COLOR_DARK} stopOpacity={BEVEL_OPACITY + 0.05} />
        <stop offset="100%" stopColor={HIGHLIGHT_COLOR_DARK} stopOpacity={BEVEL_OPACITY * 0.7} />
      </linearGradient>

      <pattern
        id={`${idPrefix}-iso-hatch-base`}
        patternUnits="userSpaceOnUse"
        width="6"
        height="6"
        patternTransform="rotate(45)"
      >
        <line x1="0" y1="0" x2="0" y2="6" stroke="currentColor" strokeWidth="1" strokeOpacity="0.15" />
      </pattern>
      <pattern
        id={`${idPrefix}-iso-hatch-accent`}
        patternUnits="userSpaceOnUse"
        width="6"
        height="6"
        patternTransform="rotate(45)"
      >
        <line x1="0" y1="0" x2="0" y2="6" stroke={HIGHLIGHT_COLOR_DARK} strokeWidth="1" strokeOpacity="0.15" />
      </pattern>
    </defs>
  );
}

export function EvilIsometricBarChart() {
  // Namespaces this instance's <defs> ids so several charts can coexist on a page.
  const idPrefix = React.useId().replace(/:/g, "");

  const maxValue = React.useMemo(
    () => chartData.reduce((m, d) => (d.revenue > m ? d.revenue : m), 0),
    [],
  );
  const total = chartData.reduce((sum, d) => sum + d.revenue, 0);
  const peak = chartData.find((d) => d.revenue === maxValue)!;

  return (
    <div className="flex h-full w-full flex-col p-4">
      <div className="flex flex-row justify-between">
        <div className="flex flex-row">
          <div className="flex flex-col gap-2">
            <span className="text-muted-foreground font-mono text-xs">{"[$] Total"}</span>
            <span className="text-primary font-mono text-3xl">
              <span className="text-muted-foreground text-xl font-normal">$</span>
              <span className="tracking-tighter">{total}K</span>
            </span>
          </div>
          <hr className="mx-4 h-full border-l border-dashed" />
          <div className="flex flex-col gap-2">
            <span className="text-muted-foreground font-mono text-xs">{"[⬆] Peak"}</span>
            <span className="text-primary font-mono text-3xl tracking-tighter">
              {peak.month.slice(0, 3)}
            </span>
          </div>
        </div>
        <div className="flex flex-col justify-end gap-1">
          <span className="text-muted-foreground font-mono text-[10px]">
            {"// PROJECTION: "}
            <span className="text-primary">ISOMETRIC</span>
          </span>
          <span className="text-muted-foreground font-mono text-[10px]">
            {"// HIGHLIGHT: "}
            <span className="text-primary">MAX</span>
          </span>
        </div>
      </div>
      <hr className="my-4 border-t border-dashed" />
      <ChartContainer config={chartConfig}>
        <BarChart
          accessibilityLayer
          data={chartData}
          margin={{ top: 30, right: 30, left: 0, bottom: 0 }}
          barCategoryGap="25%"
        >
          <IsoBarDefs idPrefix={idPrefix} />
          <XAxis
            dataKey="month"
            tickLine={false}
            tickMargin={10}
            axisLine={false}
            tickFormatter={(value: string) => value.slice(0, 3)}
          />
          <YAxis hide domain={[0, "dataMax + 10"]} />
          <ChartTooltip
            cursor={false}
            content={
              <ChartTooltipContent
                formatter={(value, name) => (
                  <div className="flex flex-1 items-center gap-2">
                    <div
                      className="size-2.5 shrink-0 rounded-[2px]"
                      style={{ background: "var(--color-revenue-0)" }}
                    />
                    <span className="text-muted-foreground flex-1 capitalize">{name}</span>
                    <span className="text-foreground font-mono font-medium tabular-nums">
                      ${value}K
                    </span>
                  </div>
                )}
              />
            }
          />
          <Bar
            dataKey="revenue"
            isAnimationActive={false}
            shape={(props: unknown) => (
              <IsoBar {...(props as ShapeProps)} maxValue={maxValue} idPrefix={idPrefix} />
            )}
          />
        </BarChart>
      </ChartContainer>
    </div>
  );
}
```
```bash
npx shadcn@latest add @evilcharts/isometric-bar-chart
```

---

## Composed Chart <a name="composed-chart"></a>

> Simple static & beautifully designed composed charts combining bars and lines

#### Live Example Code

##### Example File: `ex-composed-chart.tsx`

```tsx
"use client";

import {
  EvilComposedChart,
  Bar,
  Line,
  XAxis,
  Grid,
  Tooltip,
  Legend,
  ActiveDot,
  Dot,
} from "@/registry/charts/composed-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", revenue: 4200, profit: 1800 },
  { month: "February", revenue: 5800, profit: 2400 },
  { month: "March", revenue: 4100, profit: 1600 },
  { month: "April", revenue: 6200, profit: 2800 },
  { month: "May", revenue: 5400, profit: 2200 },
  { month: "June", revenue: 7800, profit: 3400 },
  { month: "July", revenue: 6100, profit: 2600 },
  { month: "August", revenue: 8200, profit: 3800 },
  { month: "September", revenue: 5900, profit: 2500 },
  { month: "October", revenue: 6800, profit: 3000 },
  { month: "November", revenue: 7200, profit: 3200 },
  { month: "December", revenue: 9100, profit: 4200 },
];

const chartConfig = {
  revenue: {
    label: "Revenue",
    colors: {
      light: ["#3b82f6"],
      dark: ["#6A5ACD"],
    },
  },
  profit: {
    label: "Profit",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleComposedChart() {
  return (
    <EvilComposedChart
      className="h-full w-full p-4"
      xDataKey="month"
      data={data}
      config={chartConfig}
      showBrush
      brushFormatLabel={(value) => String(value).substring(0, 3)}
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Bar dataKey="revenue" isClickable />
      <Line dataKey="profit" isClickable>
        <ActiveDot variant="colored-border" />
        <Dot variant="default" />
      </Line>
    </EvilComposedChart>
  );
}
```

## Installation


  
    
    
  
  
### CLI Installation


    ```bash
npx shadcn@latest add @evilcharts/composed-chart
```
  
  
### Manual Installation


    
      
        #### Install the following dependencies:

        
          ```bash
npm install recharts
npm install motion
```
        
      
      
        #### Copy and paste the following code snippets into your project.

         
          To use the chart, first create the folder `evilcharts` and a subfolder called `charts` inside your `components` directory.
          Then, copy the following base composed-chart code into a new file in that folder.
        
        
          #### Component Implementation

##### Source File: `composed-chart.tsx`

```tsx
"use client";

import {
  type ChartConfig,
  ChartContainer,
  getColorsCount,
  getLoadingData,
  LoadingIndicator,
} from "@/registry/ui/chart";
import { EvilBrush, useEvilBrush, type EvilBrushRange } from "@/registry/ui/evil-brush";
import { ChartLegend, ChartLegendContent, type ChartLegendVariant } from "@/registry/ui/legend";
import {
  ChartTooltip,
  ChartTooltipContent,
  type TooltipRoundness,
  type TooltipVariant,
} from "@/registry/ui/tooltip";
import { ChartDot, type DotVariant } from "@/registry/ui/dot";
import {
  Children,
  createContext,
  isValidElement,
  use,
  useCallback,
  useId,
  useMemo,
  useRef,
  useState,
  type ComponentProps,
  type FC,
  type ReactElement,
  type ReactNode,
} from "react";
import {
  Bar as RechartsBar,
  CartesianGrid,
  ComposedChart as RechartsComposedChart,
  Line as RechartsLine,
  XAxis as RechartsXAxis,
  YAxis as RechartsYAxis,
} from "recharts";
import { motion, useReducedMotion } from "motion/react";

// Constants
const STROKE_WIDTH = 2;
const DEFAULT_BAR_RADIUS = 4;
const LOADING_DATA_KEY = "loading";
const LOADING_ANIMATION_DURATION = 2000; // in milliseconds
const REVEAL_DURATION = 1; // line intro wipe length, in seconds
const REVEAL_EASE: [number, number, number, number] = [0, 0.7, 0.5, 1]; // intro easing
const BAR_GROW_DURATION = 0.5; // per-bar grow-in length, in seconds
const BAR_STAGGER = 0.05; // delay between consecutive bars, in seconds

type CurveType = ComponentProps<typeof RechartsLine>["type"];
type LineDotProp = ComponentProps<typeof RechartsLine>["dot"];
type LineActiveDotProp = ComponentProps<typeof RechartsLine>["activeDot"];
type StrokeVariant = "solid" | "dashed" | "animated-dashed";
type BarVariant = "default" | "hatched" | "duotone" | "duotone-reverse" | "gradient" | "stripped";

/**
 * Direction of the custom motion.dev intro. Recharts' own animation is
 * permanently disabled — lines wipe in along this direction, while bars grow up
 * from their baseline staggered in this same order.
 *
 * NOTE: the intro is a per-frame animation, heavier than a static chart.
 * `"none"` opts out — as does a device with the OS "reduce motion" preference.
 */
type ComposedAnimationType =
  | "none"
  | "left-to-right"
  | "right-to-left"
  | "center-out"
  | "edges-in";
type RevealAnimationType = Exclude<ComposedAnimationType, "none">;

// ─────────────────────────────────────────────────────────────────────────────
// Shared context
// ─────────────────────────────────────────────────────────────────────────────

/**
 * Shared state for every part of the chart. Lifted into <EvilComposedChart /> so
 * that <Bar />, <Line />, <XAxis />, <Legend />, and friends can read it without
 * prop drilling. Sub-components are composed freely — the provider is the single
 * source of truth.
 */
type ComposedChartContextValue = {
  config: ChartConfig; // colors + labels for every bar and line series
  curveType: CurveType; // default curve interpolation each <Line /> inherits
  animationType: ComposedAnimationType; // default intro each <Bar />/<Line /> inherits
  introStartedAt: number; // timestamp the chart mounted — anchors the one-shot intro
  dataLength: number; // number of rows currently rendered
  isLoading: boolean; // whether the chart shows its loading skeleton
  hoveredIndex: number | null; // data index currently hovered, or null when none
  selectedDataKey: string | null; // currently selected series, or null when none
  selectDataKey: (dataKey: string | null) => void; // sets the selected series
};

const ComposedChartContext = createContext<ComposedChartContextValue | null>(null);

// Reads the chart context, throwing a helpful error when used outside <EvilComposedChart />
function useComposedChart() {
  const context = use(ComposedChartContext);

  if (!context) {
    throw new Error(
      "Composed chart parts (<Bar />, <Line />, <XAxis />, …) must be used within <EvilComposedChart />",
    );
  }

  return context;
}

// ─────────────────────────────────────────────────────────────────────────────
// Root container
// ─────────────────────────────────────────────────────────────────────────────

// Validates that every config key also exists on the data row type
type ValidateConfigKeys<TData, TConfig> = {
  [K in keyof TConfig]: K extends keyof TData ? ChartConfig[string] : never;
};

type EvilComposedChartBaseProps<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
> = {
  config: TConfig & ValidateConfigKeys<TData, TConfig>; // series colors + labels for bars and lines
  data: TData[]; // rows rendered by the chart
  children: ReactNode; // composed parts — <Bar />, <Line />, <XAxis />, <Legend />, …
  className?: string; // extra classes for the chart container
  chartProps?: ComponentProps<typeof RechartsComposedChart>; // escape hatch for the raw Recharts chart
  curveType?: CurveType; // default curve interpolation for every <Line />
  animationType?: ComposedAnimationType; // default intro for every <Bar /> and <Line />
  barGap?: number; // gap between bars sharing a category
  barCategoryGap?: number; // gap between bar categories
  defaultSelectedDataKey?: string | null; // series selected on first render
  onSelectionChange?: (selectedDataKey: string | null) => void; // fires when the selected series changes
  isLoading?: boolean; // shows the animated loading skeleton
  loadingBars?: number; // number of bars in the loading skeleton
  showBrush?: boolean; // renders a zoom brush below the chart
  xDataKey?: keyof TData & string; // x-axis key — only needed for the brush footer
  brushHeight?: number; // height of the brush preview in pixels
  brushFormatLabel?: (value: unknown, index: number) => string; // formats brush axis labels
  onBrushChange?: (range: EvilBrushRange) => void; // fires when the brush range changes
};

type EvilComposedChartProps<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
> = EvilComposedChartBaseProps<TData, TConfig>;

/**
 * Root of the composible composed chart. Owns the data, the shared context, the
 * loading skeleton, and the optional zoom brush. Everything visual — axes, grid,
 * tooltip, legend, and the bars and lines themselves — is composed as children,
 * so a consumer renders exactly the parts they need.
 */
export function EvilComposedChart<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
>({
  config,
  data,
  children,
  className,
  chartProps,
  curveType = "linear",
  animationType = "left-to-right",
  barGap,
  barCategoryGap,
  defaultSelectedDataKey = null,
  onSelectionChange,
  isLoading = false,
  loadingBars,
  showBrush = false,
  xDataKey,
  brushHeight,
  brushFormatLabel,
  onBrushChange,
}: EvilComposedChartProps<TData, TConfig>) {
  const chartId = useId().replace(/:/g, ""); // colon-free id keeps CSS/SVG selectors valid
  // Anchors the intro to a fixed moment so it plays exactly once — re-renders
  // and Recharts remounts read elapsed time from here instead of replaying.
  const [introStartedAt] = useState(() => Date.now());
  const [selectedDataKey, setSelectedDataKey] = useState<string | null>(defaultSelectedDataKey);
  const [hoveredIndex, setHoveredIndex] = useState<number | null>(null);
  const { loadingData, onShimmerExit } = useLoadingData(isLoading, loadingBars);
  const { visibleData, brushProps } = useEvilBrush({ data });

  const displayData = showBrush && !isLoading ? visibleData : data;

  // Updates selection state and notifies the parent
  const selectDataKey = useCallback(
    (newSelectedDataKey: string | null) => {
      setSelectedDataKey(newSelectedDataKey);
      onSelectionChange?.(newSelectedDataKey);
    },
    [onSelectionChange],
  );

  const contextValue = useMemo<ComposedChartContextValue>(
    () => ({
      config,
      curveType,
      animationType,
      introStartedAt,
      dataLength: displayData.length,
      isLoading,
      hoveredIndex,
      selectedDataKey,
      selectDataKey,
    }),
    [
      config,
      curveType,
      animationType,
      introStartedAt,
      displayData.length,
      isLoading,
      hoveredIndex,
      selectedDataKey,
      selectDataKey,
    ],
  );

  return (
    <ComposedChartContext value={contextValue}>
      <ChartContainer
        className={className}
        config={config}
        footer={
          showBrush &&
          !isLoading && (
            <EvilBrush
              data={data}
              chartConfig={config}
              xDataKey={xDataKey}
              variant="area"
              curveType={curveType}
              height={brushHeight}
              formatLabel={brushFormatLabel}
              skipStyle
              className="mt-1"
              {...brushProps}
              onChange={(range) => {
                brushProps.onChange(range);
                onBrushChange?.(range);
              }}
            />
          )
        }
      >
        <LoadingIndicator isLoading={isLoading} />
        <RechartsComposedChart
          id={chartId}
          accessibilityLayer
          data={isLoading ? loadingData : displayData}
          barGap={barGap}
          barCategoryGap={barCategoryGap}
          onMouseLeave={() => setHoveredIndex(null)}
          {...chartProps}
        >
          {children}
          {isLoading && (
            <LoadingBar chartId={chartId} barRadius={DEFAULT_BAR_RADIUS} onShimmerExit={onShimmerExit} />
          )}
        </RechartsComposedChart>
      </ChartContainer>
    </ComposedChartContext>
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Composible parts
// ─────────────────────────────────────────────────────────────────────────────

type BarProps = {
  dataKey: string; // series key — must exist on the data and config
  variant?: BarVariant; // fill style for this bar only
  radius?: number; // corner radius of the bar in pixels
  glow?: boolean; // applies a soft neon glow to this bar
  animationType?: ComposedAnimationType; // grow-in order — falls back to the chart default
  isClickable?: boolean; // lets this bar be selected by clicking it
  enableHoverHighlight?: boolean; // dims this bar when another column is hovered
  barProps?: ComponentProps<typeof RechartsBar>; // escape hatch for raw Recharts Bar props
};

/**
 * A single bar series. Each <Bar /> is fully self-contained: it generates its
 * own gradient/pattern definitions under a unique id, so any number of bars —
 * each with its own variant, glow, and clickability — can live in one chart
 * without style collisions.
 */
export function Bar({
  dataKey,
  variant = "default",
  radius = DEFAULT_BAR_RADIUS,
  glow = false,
  animationType,
  isClickable = false,
  enableHoverHighlight = false,
  barProps,
}: BarProps) {
  const {
    config,
    animationType: defaultAnimation,
    introStartedAt,
    dataLength,
    isLoading,
    hoveredIndex,
    selectedDataKey,
    selectDataKey,
  } = useComposedChart();
  const id = useId().replace(/:/g, ""); // unique id scopes this bar's style defs
  // Devices set to "reduce motion" skip the grow-in animation entirely
  const shouldReduceMotion = useReducedMotion();

  // The root renders the skeleton bar while loading, so real bars step aside
  if (isLoading) return null;

  const isSelected = selectedDataKey === null || selectedDataKey === dataKey;
  const filter = glow ? `url(#${id}-glow)` : undefined;

  // The grow-in is a per-frame animation — heavier than a static chart — so
  // `"none"` and the OS reduce-motion preference both opt out of it.
  const revealType: ComposedAnimationType = shouldReduceMotion
    ? "none"
    : (animationType ?? defaultAnimation);

  return (
    <>
      <RechartsBar
        dataKey={dataKey}
        fill={`url(#${id}-bar-colors)`}
        radius={radius}
        // Recharts' built-in bar animation is permanently disabled — every bar
        // instead grows in from its baseline via the staggered motion.dev shape.
        isAnimationActive={false}
        style={isClickable || enableHoverHighlight ? { cursor: "pointer" } : undefined}
        shape={(props: unknown) => {
          const barShapeProps = props as BarShapeProps;
          const index = typeof barShapeProps.index === "number" ? barShapeProps.index : -1;

          return (
            <CustomBar
              {...barShapeProps}
              id={id}
              variant={variant}
              barRadius={radius}
              filter={filter}
              fillOpacity={getBarOpacity({
                isClickable,
                isSelected,
                selectedDataKey,
                enableHoverHighlight,
                hoveredIndex,
                index,
              })}
              isClickable={isClickable}
              enableHoverHighlight={enableHoverHighlight}
              animationType={revealType}
              dataLength={dataLength}
              introStartedAt={introStartedAt}
              onClick={() => {
                if (!isClickable) return;
                selectDataKey(selectedDataKey === dataKey ? null : dataKey);
              }}
            />
          );
        }}
        {...barProps}
      />
      <defs>
        <VerticalColorGradient id={id} dataKey={dataKey} config={config} />
        {variant === "hatched" && <HatchedPattern id={id} dataKey={dataKey} />}
        {variant === "duotone" && <DuotonePattern id={id} dataKey={dataKey} config={config} />}
        {variant === "duotone-reverse" && (
          <DuotoneReversePattern id={id} dataKey={dataKey} config={config} />
        )}
        {variant === "gradient" && <GradientPattern id={id} dataKey={dataKey} />}
        {variant === "stripped" && <StrippedPattern id={id} dataKey={dataKey} />}
        {glow && <BarGlowFilter id={id} />}
      </defs>
    </>
  );
}

type LineProps = {
  dataKey: string; // series key — must exist on the data and config
  strokeVariant?: StrokeVariant; // stroke style for this line only
  curveType?: CurveType; // curve interpolation — falls back to the chart default
  animationType?: ComposedAnimationType; // intro reveal — falls back to the chart default
  connectNulls?: boolean; // join segments across null/missing values
  glow?: boolean; // applies a soft neon glow to this line
  isClickable?: boolean; // lets this line be selected by clicking it
  children?: ReactNode; // optional <Dot /> and <ActiveDot /> composition
  lineProps?: ComponentProps<typeof RechartsLine>; // escape hatch for raw Recharts Line props
};

/**
 * A single line series. Each <Line /> is fully self-contained: it generates its
 * own color gradient and glow filter under a unique id, so any number of lines —
 * each with its own stroke, curve, glow, and clickability — can live in one chart
 * without style collisions. Compose <Dot /> and <ActiveDot /> inside it to add
 * point markers.
 */
export function Line({
  dataKey,
  strokeVariant = "solid",
  curveType,
  animationType,
  connectNulls = false,
  glow = false,
  isClickable = false,
  children,
  lineProps,
}: LineProps) {
  const {
    config,
    curveType: defaultCurve,
    animationType: defaultAnimation,
    isLoading,
    selectedDataKey,
    selectDataKey,
  } = useComposedChart();
  const id = useId().replace(/:/g, ""); // unique id scopes this line's style defs
  // Devices set to "reduce motion" skip the intro reveal entirely
  const shouldReduceMotion = useReducedMotion();

  // The root renders the skeleton bar while loading, so real lines step aside
  if (isLoading) return null;

  const resolvedCurve = curveType ?? defaultCurve;

  // The reveal is an animated SVG mask — heavier than a static chart — so
  // `"none"` and the OS reduce-motion preference both opt out of it.
  const revealType: ComposedAnimationType = shouldReduceMotion
    ? "none"
    : (animationType ?? defaultAnimation);
  const maskId = revealType === "none" ? undefined : `${id}-reveal-mask`;

  const opacity = getOpacity(selectedDataKey, dataKey);
  const hasSelection = selectedDataKey !== null;
  const filter = glow ? `url(#${id}-glow)` : undefined;

  const { dot, activeDot } = resolveDots(children, id, dataKey, opacity.dot, maskId);

  const isAnimatedDashed = strokeVariant === "animated-dashed";
  const isDashed = strokeVariant === "dashed" || isAnimatedDashed;

  const handleLineClick = () => {
    if (!isClickable) return;
    selectDataKey(selectedDataKey === dataKey ? null : dataKey);
  };

  return (
    <>
      {isClickable && (
        <RechartsLine
          type={resolvedCurve}
          dataKey={dataKey}
          connectNulls={connectNulls}
          stroke="transparent"
          strokeWidth={20}
          dot={false}
          activeDot={false}
          isAnimationActive={false}
          legendType="none"
          tooltipType="none"
          style={{ cursor: "pointer" }}
          onClick={handleLineClick}
        />
      )}
      <RechartsLine
        type={resolvedCurve}
        dataKey={dataKey}
        connectNulls={connectNulls}
        strokeOpacity={opacity.stroke}
        stroke={`url(#${id}-line-colors-${dataKey})`}
        filter={filter}
        dot={dot}
        activeDot={activeDot}
        strokeWidth={STROKE_WIDTH}
        strokeDasharray={isDashed ? "5 5" : undefined}
        // Recharts' built-in line animation is permanently disabled — the
        // motion.dev reveal mask drives the intro, wiping stroke and dots in together.
        isAnimationActive={false}
        style={{
          ...(maskId ? { mask: `url(#${maskId})` } : {}),
          ...(isClickable ? { cursor: "pointer", pointerEvents: "none" } : {}),
        }}
        {...lineProps}
      >
        {isAnimatedDashed && !hasSelection && <AnimatedDashedStroke />}
      </RechartsLine>
      <defs>
        {revealType !== "none" && <RevealMask id={id} type={revealType} />}
        <HorizontalColorGradient id={id} dataKey={dataKey} config={config} />
        {glow && <LineGlowFilter id={id} />}
      </defs>
    </>
  );
}

type DotProps = {
  variant?: DotVariant; // visual style of the point marker
};

/**
 * Declares a resting point marker for the <Line /> it is composed inside.
 * It renders nothing on its own — the parent <Line /> reads its variant and
 * wires it into the Recharts dot slot.
 */
export const Dot: FC<DotProps> = () => null;

/**
 * Declares the hovered/active point marker for the <Line /> it is composed
 * inside. Like <Dot />, it is a configuration slot and renders nothing itself.
 */
export const ActiveDot: FC<DotProps> = () => null;

type XAxisProps = ComponentProps<typeof RechartsXAxis>;

/**
 * The horizontal category axis. Ships with the chart's flat default styling and
 * forwards every Recharts XAxis prop, so `dataKey`, `tickFormatter`, etc. are
 * passed straight through. Hidden automatically while the chart is loading.
 */
export function XAxis({
  tickLine = false,
  axisLine = false,
  tickMargin = 8,
  minTickGap = 8,
  ...props
}: XAxisProps) {
  const { isLoading } = useComposedChart();

  if (isLoading) return null;

  return (
    <RechartsXAxis
      tickLine={tickLine}
      axisLine={axisLine}
      tickMargin={tickMargin}
      minTickGap={minTickGap}
      {...props}
    />
  );
}

type YAxisProps = ComponentProps<typeof RechartsYAxis>;

/**
 * The vertical value axis. Forwards every Recharts YAxis prop straight through.
 * Hidden automatically while the chart is loading.
 */
export function YAxis({
  tickLine = false,
  axisLine = false,
  tickMargin = 8,
  minTickGap = 8,
  width = "auto",
  ...props
}: YAxisProps) {
  const { isLoading } = useComposedChart();

  if (isLoading) return null;

  return (
    <RechartsYAxis
      tickLine={tickLine}
      axisLine={axisLine}
      tickMargin={tickMargin}
      minTickGap={minTickGap}
      width={width}
      {...props}
    />
  );
}

type GridProps = ComponentProps<typeof CartesianGrid>;

/**
 * The background grid lines. Defaults to horizontal-only dashed lines and
 * forwards every Recharts CartesianGrid prop for full control.
 */
export function Grid({ vertical = false, strokeDasharray = "3 3", ...props }: GridProps) {
  return <CartesianGrid vertical={vertical} strokeDasharray={strokeDasharray} {...props} />;
}

type TooltipProps = {
  variant?: TooltipVariant; // visual style of the tooltip surface
  roundness?: TooltipRoundness; // border-radius of the tooltip
  defaultIndex?: number; // data index shown by default with no hover
  cursor?: boolean; // whether the vertical cursor line follows the pointer
};

/**
 * The hover tooltip. Reads the chart's selection from context so its content
 * dims unselected series. Hidden automatically while the chart is loading.
 */
export function Tooltip({ variant, roundness, defaultIndex, cursor = true }: TooltipProps) {
  const { isLoading, selectedDataKey } = useComposedChart();

  if (isLoading) return null;

  return (
    <ChartTooltip
      defaultIndex={defaultIndex}
      cursor={cursor ? { strokeDasharray: "3 3", strokeWidth: STROKE_WIDTH } : false}
      content={
        <ChartTooltipContent selected={selectedDataKey} roundness={roundness} variant={variant} />
      }
    />
  );
}

type LegendProps = {
  variant?: ChartLegendVariant; // visual style of the legend indicators
  align?: "left" | "center" | "right"; // horizontal placement
  verticalAlign?: "top" | "middle" | "bottom"; // vertical placement
  isClickable?: boolean; // lets each entry toggle selection of its series
};

/**
 * The series legend. When `isClickable` is set, each entry toggles selection of
 * its series, driving the shared selection state read by every <Bar /> and <Line />.
 */
export function Legend({
  variant,
  align = "right",
  verticalAlign = "top",
  isClickable = false,
}: LegendProps) {
  const { selectedDataKey, selectDataKey } = useComposedChart();

  return (
    <ChartLegend
      verticalAlign={verticalAlign}
      align={align}
      content={
        <ChartLegendContent
          selected={selectedDataKey}
          onSelectChange={selectDataKey}
          isClickable={isClickable}
          variant={variant}
        />
      }
    />
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Selection + dot helpers
// ─────────────────────────────────────────────────────────────────────────────

// Returns stroke/dot opacity for a line — dims a series only when another is selected
const getOpacity = (selectedDataKey: string | null, dataKey: string) => {
  if (selectedDataKey === null) {
    return { stroke: 1, dot: 1 };
  }

  return selectedDataKey === dataKey ? { stroke: 1, dot: 1 } : { stroke: 0.3, dot: 0.3 };
};

// Returns the fill opacity for a bar, accounting for both selection and hover state
const getBarOpacity = ({
  isClickable,
  isSelected,
  selectedDataKey,
  enableHoverHighlight,
  hoveredIndex,
  index,
}: {
  isClickable: boolean;
  isSelected: boolean;
  selectedDataKey: string | null;
  enableHoverHighlight: boolean;
  hoveredIndex: number | null;
  index: number;
}) => {
  const clickOpacity = isClickable && selectedDataKey !== null ? (isSelected ? 1 : 0.3) : 1;

  if (enableHoverHighlight && hoveredIndex !== null) {
    return hoveredIndex === index ? clickOpacity : clickOpacity * 0.3;
  }

  return clickOpacity;
};

// Pulls <Dot /> and <ActiveDot /> out of a line's children into Recharts dot slots.
// When a `maskId` is given the resting dot is wired to the intro reveal mask so it
// wipes in with the line; the active dot is always left unmasked since it only
// appears on hover, after the intro has finished.
const resolveDots = (
  children: ReactNode,
  id: string,
  dataKey: string,
  dotOpacity: number,
  maskId: string | undefined,
): { dot: LineDotProp; activeDot: LineActiveDotProp } => {
  let dot: LineDotProp = false;
  let activeDot: LineActiveDotProp = false;

  Children.forEach(children, (child) => {
    if (!isValidElement(child)) return;

    if (child.type === Dot) {
      const { variant } = (child as ReactElement<DotProps>).props;
      dot = (
        <ChartDot
          type={variant}
          dataKey={dataKey}
          chartId={`${id}-line`}
          fillOpacity={dotOpacity}
          maskId={maskId}
        />
      );
    }

    if (child.type === ActiveDot) {
      const { variant } = (child as ReactElement<DotProps>).props;
      activeDot = (
        <ChartDot type={variant} dataKey={dataKey} chartId={`${id}-line`} fillOpacity={dotOpacity} />
      );
    }
  });

  return { dot, activeDot };
};

// ─────────────────────────────────────────────────────────────────────────────
// Custom bar shape
// ─────────────────────────────────────────────────────────────────────────────

// Props Recharts passes to a bar's custom shape renderer
type BarShapeProps = {
  x?: number;
  y?: number;
  width?: number;
  height?: number;
  fill?: string;
  fillOpacity?: number;
  dataKey?: string;
  index?: number;
  background?: {
    x?: number;
    y?: number;
    width?: number;
    height?: number;
  };
  [key: string]: unknown;
};

type CustomBarProps = {
  id: string; // unique id of the owning <Bar />
  variant: BarVariant; // fill style of the bar
  barRadius: number; // corner radius of the bar
  filter?: string; // optional glow filter reference
  isClickable?: boolean; // whether the bar is selectable by click
  enableHoverHighlight?: boolean; // whether hovering a column dims the others
  animationType?: ComposedAnimationType; // grow-in order for this bar
  dataLength?: number; // total bars in the series — drives the stagger
  introStartedAt?: number; // chart-mount timestamp anchoring the one-shot grow-in
  onClick?: () => void; // fired when a clickable bar is clicked
} & BarShapeProps;

// Renders a single bar rectangle with its variant fill, glow, and hit area
const CustomBar = ({
  x = 0,
  y = 0,
  width = 0,
  height = 0,
  fillOpacity = 1,
  background,
  index = -1,
  id,
  variant,
  barRadius,
  filter,
  isClickable,
  enableHoverHighlight,
  animationType = "none",
  dataLength = 0,
  introStartedAt = 0,
  onClick,
}: CustomBarProps) => {
  const cursorStyle = isClickable || enableHoverHighlight ? { cursor: "pointer" } : undefined;
  const hitAreaX = background?.x ?? x;
  const hitAreaY = background?.y ?? y;
  const hitAreaWidth = background?.width ?? width;
  const hitAreaHeight = background?.height ?? height;

  // motion.dev grow-in props for this bar — an empty object once it has finished
  const grow = getBarGrowAnimation(animationType, index, dataLength, introStartedAt) ?? {};

  const getFill = () => {
    switch (variant) {
      case "hatched":
        return `url(#${id}-hatched)`;
      case "duotone":
        return `url(#${id}-duotone)`;
      case "duotone-reverse":
        return `url(#${id}-duotone-reverse)`;
      case "gradient":
        return `url(#${id}-gradient)`;
      case "stripped":
        return `url(#${id}-stripped)`;
      default:
        return `url(#${id}-bar-colors)`;
    }
  };

  // Full-height invisible rect — keeps the column hoverable even mid grow-in
  const hitArea = enableHoverHighlight ? (
    <rect x={hitAreaX} y={hitAreaY} width={hitAreaWidth} height={hitAreaHeight} fill="transparent" />
  ) : null;

  if (variant === "stripped") {
    return (
      <g style={cursorStyle} onClick={onClick}>
        <motion.g
          {...grow}
          filter={filter}
          opacity={fillOpacity}
          className="transition-opacity duration-200"
        >
          <rect x={x} y={y} width={width} height={height} fill={getFill()} />
          <rect x={x} y={y} width={width} height={2} fill={`url(#${id}-bar-colors)`} />
        </motion.g>
        {hitArea}
      </g>
    );
  }

  return (
    <g style={cursorStyle} onClick={onClick}>
      <motion.g {...grow}>
        <rect
          x={x}
          y={y}
          width={width}
          height={height}
          rx={barRadius}
          ry={barRadius}
          fill={getFill()}
          opacity={fillOpacity}
          filter={filter}
          className="transition-opacity duration-200"
        />
      </motion.g>
      {hitArea}
    </g>
  );
};

/**
 * Builds the motion.dev grow-in animation for a single bar, or returns `null`
 * when it should render statically (`"none"`, an unknown index, or — crucially —
 * once the bar has already finished growing).
 *
 * The intro is anchored to `introStartedAt` (stamped once when the chart mounts)
 * rather than to component mount. Recharts remounts every bar whenever the chart
 * re-renders, so a mount-based animation would replay endlessly; reading elapsed
 * time instead makes it a true one-shot — a bar past its window renders static,
 * a bar caught mid-grow resumes from where it should already be.
 */
const getBarGrowAnimation = (
  animationType: ComposedAnimationType,
  index: number,
  dataLength: number,
  introStartedAt: number,
) => {
  if (animationType === "none" || index < 0 || dataLength <= 0) return null;

  const lastIndex = dataLength - 1;
  const center = lastIndex / 2;

  // How many bars this one waits behind before it starts growing
  let step: number;
  switch (animationType) {
    case "right-to-left":
      step = lastIndex - index;
      break;
    case "center-out":
      step = Math.abs(index - center);
      break;
    case "edges-in":
      step = center - Math.abs(index - center);
      break;
    default: // left-to-right
      step = index;
  }

  const startMs = step * BAR_STAGGER * 1000;
  const durationMs = BAR_GROW_DURATION * 1000;
  const endMs = startMs + durationMs;
  const elapsed = Date.now() - introStartedAt;

  // Already finished — render static so re-renders/remounts can't replay it
  if (elapsed >= endMs) return null;

  // Resume from wherever this bar should already be (0 before it starts)
  const from = elapsed <= startMs ? 0 : (elapsed - startMs) / durationMs;

  return {
    initial: { scaleY: from },
    animate: { scaleY: 1 },
    transition: {
      duration: (endMs - Math.max(elapsed, startMs)) / 1000,
      ease: REVEAL_EASE,
      delay: Math.max(0, startMs - elapsed) / 1000,
    },
    style: { originY: 1 }, // grow upward from the baseline
  };
};

// motion `originX` for each single-rect line reveal — the edge the wipe grows from
const SINGLE_REVEAL_ORIGIN: Record<Exclude<RevealAnimationType, "edges-in">, number> = {
  "left-to-right": 0,
  "right-to-left": 1,
  "center-out": 0.5,
};

/**
 * Wipe mask driven by motion.dev, played once when a <Line /> mounts. The same
 * mask is applied to the line's stroke and its resting dots so both reveal in
 * lockstep, replacing Recharts' built-in animation.
 */
const RevealMask = ({ id, type }: { id: string; type: RevealAnimationType }) => {
  const reveal = {
    initial: { scaleX: 0 },
    animate: { scaleX: 1 },
    transition: { duration: REVEAL_DURATION, ease: REVEAL_EASE },
  };

  return (
    <mask
      id={`${id}-reveal-mask`}
      maskUnits="userSpaceOnUse"
      maskContentUnits="userSpaceOnUse"
      x="0"
      y="0"
      width="100%"
      height="100%"
    >
      {type === "edges-in" ? (
        <>
          {/* left half wipes inward from the left edge toward the centre */}
          <motion.rect
            {...reveal}
            x="0"
            y="0"
            width="50%"
            height="100%"
            fill="white"
            style={{ originX: 0 }}
          />
          {/* right half wipes inward from the right edge toward the centre */}
          <motion.rect
            {...reveal}
            x="50%"
            y="0"
            width="50%"
            height="100%"
            fill="white"
            style={{ originX: 1 }}
          />
        </>
      ) : (
        <motion.rect
          {...reveal}
          x="0"
          y="0"
          width="100%"
          height="100%"
          fill="white"
          style={{ originX: SINGLE_REVEAL_ORIGIN[type] }}
        />
      )}
    </mask>
  );
};

// ─────────────────────────────────────────────────────────────────────────────
// Style definitions — one set per <Bar /> / <Line />, scoped to its unique id
// ─────────────────────────────────────────────────────────────────────────────

type StyleProps = {
  id: string; // unique id of the owning series
  dataKey: string; // series key the colors belong to
};

// Animated dashed-stroke effect, rendered as a child of the Recharts Line
const AnimatedDashedStroke = () => {
  return (
    <>
      <animate
        attributeName="stroke-dasharray"
        values="5 5; 0 5; 5 5"
        dur="1s"
        repeatCount="indefinite"
        keyTimes="0;0.5;1"
      />
      <animate
        attributeName="stroke-dashoffset"
        values="0; -10"
        dur="1s"
        repeatCount="indefinite"
        keyTimes="0;1"
      />
    </>
  );
};

/** Vertical top-to-bottom color gradient — the fill source for every bar variant. */
const VerticalColorGradient = ({
  id,
  dataKey,
  config,
}: StyleProps & { config: ChartConfig }) => {
  const colorsCount = getColorsCount(config[dataKey] ?? {});

  return (
    <linearGradient id={`${id}-bar-colors`} x1="0" y1="0" x2="0" y2="1">
      {colorsCount === 1 ? (
        <>
          <stop offset="0%" stopColor={`var(--color-${dataKey}-0)`} />
          <stop offset="100%" stopColor={`var(--color-${dataKey}-0)`} />
        </>
      ) : (
        Array.from({ length: colorsCount }, (_, index) => {
          const offset = `${(index / (colorsCount - 1)) * 100}%`;
          return (
            <stop
              key={offset}
              offset={offset}
              stopColor={`var(--color-${dataKey}-${index}, var(--color-${dataKey}-0))`}
            />
          );
        })
      )}
    </linearGradient>
  );
};

/** Horizontal left-to-right color gradient — the stroke source for a line series. */
const HorizontalColorGradient = ({
  id,
  dataKey,
  config,
}: StyleProps & { config: ChartConfig }) => {
  const colorsCount = getColorsCount(config[dataKey] ?? {});

  return (
    <linearGradient id={`${id}-line-colors-${dataKey}`} x1="0" y1="0" x2="1" y2="0">
      {colorsCount === 1 ? (
        <>
          <stop offset="0%" stopColor={`var(--color-${dataKey}-0)`} />
          <stop offset="100%" stopColor={`var(--color-${dataKey}-0)`} />
        </>
      ) : (
        Array.from({ length: colorsCount }, (_, index) => {
          const offset = `${(index / (colorsCount - 1)) * 100}%`;
          return (
            <stop
              key={offset}
              offset={offset}
              stopColor={`var(--color-${dataKey}-${index}, var(--color-${dataKey}-0))`}
            />
          );
        })
      )}
    </linearGradient>
  );
};

/** Hatched diagonal-stripe fill for a bar, masked from the series color gradient. */
const HatchedPattern = ({ id }: StyleProps) => {
  return (
    <>
      <pattern
        id={`${id}-hatched-mask-pattern`}
        x="0"
        y="0"
        width="5"
        height="5"
        patternUnits="userSpaceOnUse"
        patternTransform="rotate(-45)"
      >
        <rect width="5" height="5" fill="white" fillOpacity={0.3} />
        <rect width="1.5" height="5" fill="white" fillOpacity={1} />
      </pattern>
      <mask id={`${id}-hatched-mask`}>
        <rect width="100%" height="100%" fill={`url(#${id}-hatched-mask-pattern)`} />
      </mask>
      <pattern id={`${id}-hatched`} patternUnits="userSpaceOnUse" width="100%" height="100%">
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-bar-colors)`}
          mask={`url(#${id}-hatched-mask)`}
        />
      </pattern>
    </>
  );
};

/** Two-tone fill that splits each bar into a light and a full-strength half. */
const DuotonePattern = ({ id, dataKey, config }: StyleProps & { config: ChartConfig }) => {
  const colorsCount = getColorsCount(config[dataKey] ?? {});

  return (
    <>
      <linearGradient
        id={`${id}-duotone-mask-gradient`}
        gradientUnits="objectBoundingBox"
        x1="0"
        y1="0"
        x2="1"
        y2="0"
      >
        <stop offset="50%" stopColor="white" stopOpacity={0.4} />
        <stop offset="50%" stopColor="white" stopOpacity={1} />
      </linearGradient>
      <linearGradient
        id={`${id}-duotone-colors`}
        gradientUnits="objectBoundingBox"
        x1="0"
        y1="0"
        x2="0"
        y2="1"
      >
        {colorsCount === 1 ? (
          <>
            <stop offset="0%" stopColor={`var(--color-${dataKey}-0)`} />
            <stop offset="100%" stopColor={`var(--color-${dataKey}-0)`} />
          </>
        ) : (
          Array.from({ length: colorsCount }, (_, index) => {
            const offset = `${(index / (colorsCount - 1)) * 100}%`;
            return (
              <stop
                key={offset}
                offset={offset}
                stopColor={`var(--color-${dataKey}-${index}, var(--color-${dataKey}-0))`}
              />
            );
          })
        )}
      </linearGradient>
      <mask id={`${id}-duotone-mask`} maskContentUnits="objectBoundingBox">
        <rect x="0" y="0" width="1" height="1" fill={`url(#${id}-duotone-mask-gradient)`} />
      </mask>
      <pattern
        id={`${id}-duotone`}
        patternUnits="objectBoundingBox"
        patternContentUnits="objectBoundingBox"
        width="1"
        height="1"
      >
        <rect
          x="0"
          y="0"
          width="1"
          height="1"
          fill={`url(#${id}-duotone-colors)`}
          mask={`url(#${id}-duotone-mask)`}
        />
      </pattern>
    </>
  );
};

/** Two-tone fill mirrored from `duotone` — the full-strength half comes first. */
const DuotoneReversePattern = ({ id, dataKey, config }: StyleProps & { config: ChartConfig }) => {
  const colorsCount = getColorsCount(config[dataKey] ?? {});

  return (
    <>
      <linearGradient
        id={`${id}-duotone-reverse-mask-gradient`}
        gradientUnits="objectBoundingBox"
        x1="0"
        y1="0"
        x2="1"
        y2="0"
      >
        <stop offset="50%" stopColor="white" stopOpacity={1} />
        <stop offset="50%" stopColor="white" stopOpacity={0.4} />
      </linearGradient>
      <linearGradient
        id={`${id}-duotone-reverse-colors`}
        gradientUnits="objectBoundingBox"
        x1="0"
        y1="0"
        x2="0"
        y2="1"
      >
        {colorsCount === 1 ? (
          <>
            <stop offset="0%" stopColor={`var(--color-${dataKey}-0)`} />
            <stop offset="100%" stopColor={`var(--color-${dataKey}-0)`} />
          </>
        ) : (
          Array.from({ length: colorsCount }, (_, index) => {
            const offset = `${(index / (colorsCount - 1)) * 100}%`;
            return (
              <stop
                key={offset}
                offset={offset}
                stopColor={`var(--color-${dataKey}-${index}, var(--color-${dataKey}-0))`}
              />
            );
          })
        )}
      </linearGradient>
      <mask id={`${id}-duotone-reverse-mask`} maskContentUnits="objectBoundingBox">
        <rect
          x="0"
          y="0"
          width="1"
          height="1"
          fill={`url(#${id}-duotone-reverse-mask-gradient)`}
        />
      </mask>
      <pattern
        id={`${id}-duotone-reverse`}
        patternUnits="objectBoundingBox"
        patternContentUnits="objectBoundingBox"
        width="1"
        height="1"
      >
        <rect
          x="0"
          y="0"
          width="1"
          height="1"
          fill={`url(#${id}-duotone-reverse-colors)`}
          mask={`url(#${id}-duotone-reverse-mask)`}
        />
      </pattern>
    </>
  );
};

/** Gradient fill for a bar that fades from visible at the top toward transparent. */
const GradientPattern = ({ id }: StyleProps) => {
  return (
    <>
      <linearGradient id={`${id}-gradient-mask-gradient`} x1="0" y1="0" x2="0" y2="1">
        <stop offset="20%" stopColor="white" stopOpacity={1} />
        <stop offset="90%" stopColor="white" stopOpacity={0} />
      </linearGradient>
      <mask id={`${id}-gradient-mask`}>
        <rect width="100%" height="100%" fill={`url(#${id}-gradient-mask-gradient)`} />
      </mask>
      <pattern id={`${id}-gradient`} patternUnits="userSpaceOnUse" width="100%" height="100%">
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-bar-colors)`}
          mask={`url(#${id}-gradient-mask)`}
        />
      </pattern>
    </>
  );
};

/** Low-opacity gradient fill paired with the solid top strip drawn by `CustomBar`. */
const StrippedPattern = ({ id }: StyleProps) => {
  return (
    <>
      <linearGradient id={`${id}-stripped-mask-gradient`} x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%" stopColor="white" stopOpacity={0.4} />
        <stop offset="100%" stopColor="white" stopOpacity={0.1} />
      </linearGradient>
      <mask id={`${id}-stripped-mask`}>
        <rect width="100%" height="100%" fill={`url(#${id}-stripped-mask-gradient)`} />
      </mask>
      <pattern id={`${id}-stripped`} patternUnits="userSpaceOnUse" width="100%" height="100%">
        <rect
          width="100%"
          height="100%"
          fill={`url(#${id}-bar-colors)`}
          mask={`url(#${id}-stripped-mask)`}
        />
      </pattern>
    </>
  );
};

/** Soft outer-glow filter applied to a glowing bar. */
const BarGlowFilter = ({ id }: { id: string }) => {
  return (
    <filter id={`${id}-glow`} x="-100%" y="-100%" width="300%" height="300%">
      <feGaussianBlur in="SourceGraphic" stdDeviation="8" result="blur" />
      <feColorMatrix
        in="blur"
        type="matrix"
        values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 0.5 0"
        result="glow"
      />
      <feMerge>
        <feMergeNode in="glow" />
        <feMergeNode in="SourceGraphic" />
      </feMerge>
    </filter>
  );
};

/** Soft outer-glow filter applied to a glowing line. */
const LineGlowFilter = ({ id }: { id: string }) => {
  return (
    <filter id={`${id}-glow`} x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceGraphic" stdDeviation="10" result="blur" />
      <feColorMatrix
        in="blur"
        type="matrix"
        values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 2 0"
        result="glow"
      />
      <feMerge>
        <feMergeNode in="glow" />
        <feMergeNode in="SourceGraphic" />
      </feMerge>
    </filter>
  );
};

// ─────────────────────────────────────────────────────────────────────────────
// Loading skeleton
// ─────────────────────────────────────────────────────────────────────────────

// Builds bell-curve eased gradient stops for the loading shimmer
const generateEasedGradientStops = (
  steps: number = 17,
  minOpacity: number = 0.05,
  maxOpacity: number = 0.9,
) => {
  return Array.from({ length: steps }, (_, i) => {
    const t = i / (steps - 1); // 0 to 1
    // Sine-based bell curve easing: peaks at center (t=0.5), smooth falloff at edges
    const eased = Math.sin(t * Math.PI) ** 2;
    const opacity = minOpacity + eased * (maxOpacity - minOpacity);
    return { offset: `${(t * 100).toFixed(0)}%`, opacity: Number(opacity.toFixed(3)) };
  });
};

/**
 * Hook to manage loading data with pixel-perfect shimmer synchronization.
 *
 * Uses motion.dev's onUpdate callback to ensure chart data is only regenerated
 * when the shimmer has completely exited the visible area. This eliminates
 * timing drift issues from setTimeout/setInterval.
 */
export function useLoadingData(isLoading: boolean, loadingBars: number = 12) {
  const [loadingDataKey, setLoadingDataKey] = useState(false);

  // Callback fired by motion.dev when the shimmer exits the visible area
  const onShimmerExit = useCallback(() => {
    if (isLoading) {
      setLoadingDataKey((prev) => !prev);
    }
  }, [isLoading]);

  const loadingData = useMemo(
    () => getLoadingData(loadingBars, 20, 80),
    // loadingDataKey toggle triggers re-computation when the shimmer exits
    // eslint-disable-next-line react-hooks/exhaustive-deps
    [loadingBars, loadingDataKey],
  );

  return { loadingData, onShimmerExit };
}

/**
 * The skeleton bar shown while the chart is loading. Rendered by the root in
 * place of the real bars and lines, paired with its own masked shimmer pattern.
 */
const LoadingBar = ({
  chartId,
  barRadius,
  onShimmerExit,
}: {
  chartId: string;
  barRadius: number;
  onShimmerExit: () => void;
}) => {
  return (
    <>
      <RechartsBar
        dataKey={LOADING_DATA_KEY}
        fill="currentColor"
        fillOpacity={0.15}
        radius={barRadius}
        isAnimationActive={false}
        legendType="none"
        style={{ mask: `url(#${chartId}-loading-mask)` }}
      />
      <defs>
        <LoadingPattern chartId={chartId} onShimmerExit={onShimmerExit} />
      </defs>
    </>
  );
};

/**
 * Animated shimmer pattern for the loading skeleton.
 *
 * The visible chart area is normalized to 0-1, the shimmer gradient has width 1,
 * and the pattern is 3x wide so the shimmer has buffer on both sides. The motion
 * rect travels x from -1 to 2; onShimmerExit fires as it crosses x=1, letting the
 * data swap happen while the shimmer is off-screen for a seamless loop.
 */
const LoadingPattern = ({
  chartId,
  onShimmerExit,
}: {
  chartId: string;
  onShimmerExit: () => void;
}) => {
  const gradientStops = generateEasedGradientStops();

  // 1 (left buffer) + 1 (visible) + 1 (right buffer)
  const patternWidth = 3;
  const startX = -1;
  const endX = 2;

  // Tracks the last x value to detect the exit threshold crossing
  const lastXRef = useRef(startX);

  return (
    <>
      <linearGradient id={`${chartId}-loading-gradient`} x1="0" y1="0" x2="1" y2="0">
        {gradientStops.map(({ offset, opacity }) => (
          <stop key={offset} offset={offset} stopColor="white" stopOpacity={opacity} />
        ))}
      </linearGradient>
      <pattern
        id={`${chartId}-loading-pattern`}
        patternUnits="objectBoundingBox"
        patternContentUnits="objectBoundingBox"
        patternTransform="rotate(25)"
        width={patternWidth}
        height="1"
        x="0"
        y="0"
      >
        <motion.rect
          y="0"
          width="1"
          height="1"
          fill={`url(#${chartId}-loading-gradient)`}
          initial={{ x: startX }}
          animate={{ x: endX }}
          transition={{
            duration: LOADING_ANIMATION_DURATION / 1000,
            ease: "linear",
            repeat: Infinity,
            repeatType: "loop",
          }}
          onUpdate={(latest) => {
            const xValue = typeof latest.x === "number" ? latest.x : startX;
            const lastX = lastXRef.current;

            // Fire once per loop, when the shimmer fully exits the visible area
            if (xValue >= 1 && lastX < 1) {
              onShimmerExit();
            }

            lastXRef.current = xValue;
          }}
        />
      </pattern>
      <mask id={`${chartId}-loading-mask`} maskUnits="userSpaceOnUse">
        <rect width="100%" height="100%" fill={`url(#${chartId}-loading-pattern)`} />
      </mask>
    </>
  );
};
```
        
      
       
        #### Now, Let's add the chart component to your project.

        
          These Components are required by the chart component to render the chart. Make a folder called `ui` inside the `evilcharts` folder and paste the code there.

          Below is main chart component.
        
        
          #### Component Implementation

##### Source File: `chart.tsx`

```tsx
"use client";

import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

// Format: { THEME_NAME: CSS_SELECTOR }
const THEMES = { light: "", dark: ".dark" } as const;

type ThemeKey = keyof typeof THEMES;

// All Keys are optional at first
type ThemeColorsBase = {
  [K in ThemeKey]?: string[];
};

// Require at least one theme key
type AtLeastOneThemeColor = {
  [K in ThemeKey]: Required<Pick<ThemeColorsBase, K>> & Partial<Omit<ThemeColorsBase, K>>;
}[ThemeKey];

const VALID_THEME_KEYS = Object.keys(THEMES) as ThemeKey[];

// Validation for chart config colors at runtime
function validateChartConfigColors(config: ChartConfig): void {
  for (const [key, value] of Object.entries(config)) {
    if (value.colors) {
      const hasValidThemeKey = VALID_THEME_KEYS.some(
        (themeKey) => value.colors?.[themeKey] !== undefined,
      );

      if (!hasValidThemeKey) {
        throw new Error(
          `[EvilCharts] Invalid chart config for "${key}": colors object must have at least one theme key (${VALID_THEME_KEYS.join(", ")}). Received empty object or invalid keys.`,
        );
      }
    }
  }
}

export type ChartConfig = Record<
  string,
  {
    label?: React.ReactNode;
    icon?: React.ComponentType;
    colors?: AtLeastOneThemeColor;
  }
>;

interface ChartContextProps {
  config: ChartConfig;
}

const ChartContext = React.createContext<ChartContextProps | null>(null);

export function useChart() {
  const context = React.useContext(ChartContext);

  if (!context) {
    throw new Error("useChart must be used within a <ChartContainer />");
  }

  return context;
}

interface ChartContainerProps
  extends
    Omit<React.ComponentProps<"div">, "children">,
    Pick<
      React.ComponentProps<typeof RechartsPrimitive.ResponsiveContainer>,
      | "initialDimension"
      | "aspect"
      | "debounce"
      | "minHeight"
      | "minWidth"
      | "maxHeight"
      | "height"
      | "width"
      | "onResize"
      | "children"
    > {
  config: ChartConfig;
  innerResponsiveContainerStyle?: React.ComponentProps<
    typeof RechartsPrimitive.ResponsiveContainer
  >["style"];
  /** Optional content rendered below the chart (e.g. EvilBrush) */
  footer?: React.ReactNode;
}

function ChartContainer({
  id,
  config,
  initialDimension = { width: 320, height: 200 },
  className,
  children,
  footer,
  ...props
}: Readonly<ChartContainerProps>) {
  const uniqueId = React.useId();
  const chartId = `chart-${id ?? uniqueId.replace(/:/g, "")}`;

  // Validate chart config at runtime
  validateChartConfigColors(config);

  return (
    <ChartContext.Provider value={{ config }}>
      <div
        data-slot="chart"
        data-chart={chartId}
        className={cn(
          "min-h-0 w-full flex-1",
          "[&_.recharts-cartesian-axis-tick_text]:fill-muted-foreground [&_.recharts-cartesian-grid_line[stroke='#ccc']]:stroke-border/50 [&_.recharts-curve.recharts-tooltip-cursor]:stroke-border [&_.recharts-polar-grid_[stroke='#ccc']]:stroke-border [&_.recharts-radial-bar-background-sector]:fill-muted [&_.recharts-rectangle.recharts-tooltip-cursor]:fill-muted [&_.recharts-reference-line_[stroke='#ccc']]:stroke-border relative flex flex-col justify-center text-xs [&_.recharts-dot[stroke='#fff']]:stroke-transparent [&_.recharts-layer]:outline-hidden [&_.recharts-sector]:outline-hidden [&_.recharts-sector[stroke='#fff']]:stroke-transparent [&_.recharts-surface]:outline-hidden",
          !footer && "aspect-video",
          className,
        )}
        {...props}
      >
        <ChartStyle id={chartId} config={config} />
        <RechartsPrimitive.ResponsiveContainer
          className="min-h-0 w-full flex-1"
          initialDimension={initialDimension}
        >
          {children}
        </RechartsPrimitive.ResponsiveContainer>
        {footer}
      </div>
    </ChartContext.Provider>
  );
}

function LoadingIndicator({ isLoading }: { isLoading: boolean }) {
  if (!isLoading) {
    return null;
  }

  return (
    <div className="pointer-events-none absolute inset-0 z-20 flex items-center justify-center">
      <div className="text-primary bg-background flex items-center justify-center gap-2 rounded-md border px-2 py-0.5 text-sm">
        <div className="border-border border-t-primary h-3 w-3 animate-spin rounded-full border" />
        <span>Loading</span>
      </div>
    </div>
  );
}

// Distribute colors evenly across slots, extra slots go to last color(s)
// Example: 2 colors for 4 slots → [red, red, pink, pink]
// Example: 3 colors for 4 slots → [red, pink, blue, blue]
function distributeColors(colorsArray: string[], maxCount: number): string[] {
  const availableCount = colorsArray.length;
  if (availableCount >= maxCount) {
    return colorsArray.slice(0, maxCount);
  }

  const result: string[] = [];
  const baseSlots = Math.floor(maxCount / availableCount);
  const extraSlots = maxCount % availableCount;

  // First (availableCount - extraSlots) colors get baseSlots each
  // Last extraSlots colors get (baseSlots + 1) each
  for (let colorIdx = 0; colorIdx < availableCount; colorIdx++) {
    const isExtraColor = colorIdx >= availableCount - extraSlots;
    const slotsForThisColor = baseSlots + (isExtraColor ? 1 : 0);
    for (let j = 0; j < slotsForThisColor; j++) {
      result.push(colorsArray[colorIdx]);
    }
  }

  return result;
}

const ChartStyle = ({ id, config }: { id: string; config: ChartConfig }) => {
  const colorConfig = Object.entries(config).filter(([, config]) => config.colors);

  if (!colorConfig.length) {
    return null;
  }

  const generateCssVars = (theme: keyof typeof THEMES) =>
    colorConfig
      .flatMap(([key, itemConfig]) => {
        const colorsArray = itemConfig.colors?.[theme];
        if (!colorsArray || !Array.isArray(colorsArray) || colorsArray.length === 0) {
          return [];
        }

        // Get max count across all themes for this key
        const maxCount = getColorsCount(itemConfig);

        // Distribute colors evenly across all required slots
        const distributedColors = distributeColors(colorsArray, maxCount);

        return distributedColors.map((color, index) => `  --color-${key}-${index}: ${color};`);
      })
      .filter(Boolean)
      .join("\n");

  const css = Object.entries(THEMES)
    .map(
      ([theme, prefix]) =>
        `${prefix} [data-chart=${id}] {\n${generateCssVars(theme as keyof typeof THEMES)}\n}`,
    )
    .join("\n");

  return <style dangerouslySetInnerHTML={{ __html: css }} />;
};

// Helper to extract item config from a payload.
export function getPayloadConfigFromPayload(config: ChartConfig, payload: unknown, key: string) {
  if (typeof payload !== "object" || payload === null) {
    return undefined;
  }

  const payloadPayload =
    "payload" in payload && typeof payload.payload === "object" && payload.payload !== null
      ? payload.payload
      : undefined;

  let configLabelKey: string = key;

  if (key in payload && typeof payload[key as keyof typeof payload] === "string") {
    configLabelKey = payload[key as keyof typeof payload] as string;
  } else if (
    payloadPayload &&
    key in payloadPayload &&
    typeof payloadPayload[key as keyof typeof payloadPayload] === "string"
  ) {
    configLabelKey = payloadPayload[key as keyof typeof payloadPayload] as string;
  }

  return configLabelKey in config ? config[configLabelKey] : config[key];
}

// Format values to percent for expanded charts
function axisValueToPercentFormatter(value: number) {
  return `${Math.round(value * 100).toFixed(0)}%`;
}

// Get max colors count across all themes for a config entry
function getColorsCount(config: ChartConfig[string]): number {
  if (!config.colors) return 1;
  const counts = VALID_THEME_KEYS.map((theme) => config.colors?.[theme]?.length ?? 0);
  return Math.max(...counts, 1);
}

// Generate random loading data for skeleton/loading state
// min/max represent percentage of the range (0-100), defaults to 20-80 for realistic look
export const getLoadingData = (points: number = 10, min: number = 0, max: number = 70) => {
  const range = max - min;
  return Array.from({ length: points }, () => ({
    loading: Math.floor(Math.random() * range) + min,
  }));
};

export {
  ChartContainer,
  ChartStyle,
  axisValueToPercentFormatter,
  LoadingIndicator,
  getColorsCount,
};
```
        
      
       
        #### Now, We need to add sub components.

        
          Create a file called `tooltip.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `tooltip.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import type { NameType, ValueType } from "recharts/types/component/DefaultTooltipContent";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type TooltipRoundness = "sm" | "md" | "lg" | "xl";
type TooltipVariant = "default" | "frosted-glass";

const roundnessMap: Record<TooltipRoundness, string> = {
  sm: "rounded-sm",
  md: "rounded-md",
  lg: "rounded-lg",
  xl: "rounded-xl",
};

const variantMap: Record<TooltipVariant, string> = {
  default: "bg-background",
  "frosted-glass": "bg-background/70 backdrop-blur-sm",
};

function ChartTooltipContent({
  active,
  payload,
  className,
  indicator = "dot",
  hideLabel = false,
  hideIndicator = false,
  label,
  labelFormatter,
  labelClassName,
  formatter,
  nameKey,
  labelKey,
  selected,
  roundness = "lg",
  variant = "default",
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip> &
  React.ComponentProps<"div"> & {
    hideLabel?: boolean;
    hideIndicator?: boolean;
    indicator?: "line" | "dot" | "dashed";
    nameKey?: string;
    labelKey?: string;
    selected?: string | null;
    roundness?: TooltipRoundness;
    variant?: TooltipVariant;
  } & Omit<
    RechartsPrimitive.DefaultTooltipContentProps<ValueType, NameType>,
    "accessibilityLayer"
  >) {
  const { config } = useChart();

  const tooltipLabel = React.useMemo(() => {
    if (hideLabel || !payload?.length) {
      return null;
    }

    const [item] = payload;
    const key = `${labelKey ?? item?.dataKey ?? item?.name ?? "value"}`;
    const itemConfig = getPayloadConfigFromPayload(config, item, key);
    const value =
      !labelKey && typeof label === "string" ? (config[label]?.label ?? label) : itemConfig?.label;

    if (labelFormatter) {
      return (
        <div className={cn("font-medium", labelClassName)}>{labelFormatter(value, payload)}</div>
      );
    }

    if (!value) {
      return null;
    }

    return <div className={cn("font-medium", labelClassName)}>{value}</div>;
  }, [label, labelFormatter, payload, hideLabel, labelClassName, config, labelKey]);

  if (!active || !payload?.length) {
    // Empty tooltip - to prevent position getting 0.0 so it doesnt animate tooltip every time from 0.0 origin
    return <span className="p-4" />;
  }

  const nestLabel = payload.length === 1 && indicator !== "dot";

  return (
    <div
      className={cn(
        "border-border/50 grid min-w-32 items-start gap-1.5 border px-2.5 py-1.5 text-xs shadow-xl",
        roundnessMap[roundness],
        variantMap[variant],
        className,
      )}
    >
      {!nestLabel ? tooltipLabel : null}
      <div className="grid gap-1.5">
        {payload
          .filter((item) => item.type !== "none")
          .map((item, index) => {
            // For pie charts, item.name contains the sector name (e.g., "chrome")
            // For radial charts, the name is in item.payload[nameKey]
            // For other charts, item.name or item.dataKey contains the series name
            const payloadName =
              nameKey && item.payload
                ? (item.payload as Record<string, unknown>)[nameKey]
                : undefined;
            const key = `${payloadName ?? item.name ?? item.dataKey ?? "value"}`;
            const itemConfig = getPayloadConfigFromPayload(config, item, key);

            // Get colors count for this item to determine gradient vs solid
            const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

            return (
              <div
                key={index}
                className={cn(
                  "[&>svg]:text-muted-foreground flex w-full flex-wrap items-stretch gap-2 [&>svg]:h-2.5 [&>svg]:w-2.5",
                  indicator === "dot" && "items-center",
                  selected != null && selected !== item.dataKey && "opacity-30",
                )}
              >
                {formatter && item?.value !== undefined && item.name ? (
                  formatter(item.value, item.name, item, index, item.payload)
                ) : (
                  <>
                    {itemConfig?.icon ? (
                      <itemConfig.icon />
                    ) : (
                      !hideIndicator && (
                        <div
                          className={cn("shrink-0 rounded-[2px]", {
                            "h-2.5 w-2.5": indicator === "dot",
                            "w-1": indicator === "line",
                            "w-0 border-[1.5px] border-dashed bg-transparent!":
                              indicator === "dashed",
                            "my-0.5": nestLabel && indicator === "dashed",
                          })}
                          style={getIndicatorColorStyle(key, colorsCount)}
                        />
                      )
                    )}
                    <div
                      className={cn(
                        "flex flex-1 justify-between gap-4 leading-none",
                        nestLabel ? "items-end" : "items-center",
                      )}
                    >
                      <div className="grid gap-1.5">
                        {nestLabel ? tooltipLabel : null}
                        <span className="text-muted-foreground">
                          {itemConfig?.label ?? item.name}
                        </span>
                      </div>
                      {item.value != null && (
                        <span className="text-foreground font-mono font-medium tabular-nums">
                          {typeof item.value === "number"
                            ? item.value.toLocaleString()
                            : String(item.value)}
                        </span>
                      )}
                    </div>
                  </>
                )}
              </div>
            );
          })}
      </div>
    </div>
  );
}

function getIndicatorColorStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { background: `var(--color-${dataKey}-0)` };
  }

  // Multiple colors: create linear gradient with evenly distributed stops
  const stops = Array.from({ length: colorsCount }, (_, index) => {
    const offset = (index / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${index}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

const ChartTooltip = ({
  animationDuration = 200,
  ...props
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip>) => (
  <RechartsPrimitive.Tooltip animationDuration={animationDuration} {...props} />
);

export { ChartTooltip, ChartTooltipContent };
export type { TooltipRoundness, TooltipVariant };
```
        
        
          Now, create another file called `legend.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `legend.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type ChartLegendVariant =
  | "square"
  | "circle"
  | "circle-outline"
  | "rounded-square"
  | "rounded-square-outline"
  | "vertical-bar"
  | "horizontal-bar";

function ChartLegendContent({
  className,
  hideIcon = false,
  nameKey,
  payload,
  verticalAlign,
  align = "right",
  selected,
  onSelectChange,
  isClickable,
  variant = "rounded-square",
}: React.ComponentProps<"div"> & {
  hideIcon?: boolean;
  nameKey?: string;
  selected?: string | null;
  isClickable?: boolean;
  onSelectChange?: (selected: string | null) => void;
  variant?: ChartLegendVariant;
} & RechartsPrimitive.DefaultLegendContentProps) {
  const { config } = useChart();

  if (!payload?.length) {
    return null;
  }

  return (
    <div
      className={cn(
        "flex items-center gap-4 select-none",
        align === "left" && "justify-start",
        align === "center" && "justify-center",
        align === "right" && "justify-end",
        verticalAlign === "top" ? "pb-4" : "pt-4",
        className,
      )}
    >
      {payload
        .filter((item) => item.type !== "none")
        .map((item) => {
          // For pie charts, item.value contains the sector name (e.g., "chrome")
          // For radial charts, the name is in item.payload[nameKey]
          // For other charts, item.dataKey contains the series name (e.g., "desktop")
          const payloadName =
            nameKey && item.payload
              ? (item.payload as Record<string, unknown>)[nameKey]
              : undefined;
          const key = `${payloadName ?? item.value ?? item.dataKey ?? "value"}`;
          const itemConfig = getPayloadConfigFromPayload(config, item, key);
          const isSelected = selected === null || selected === key;

          // Get colors count for this item to determine gradient vs solid
          const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

          return (
            <div
              key={key}
              className={cn(
                "[&>svg]:text-muted-foreground flex items-center gap-1.5 transition-opacity [&>svg]:h-3 [&>svg]:w-3",
                !isSelected && "opacity-30",
                isClickable && "cursor-pointer",
              )}
              onClick={() => {
                if (!isClickable) return;

                onSelectChange?.(selected === key ? null : key);
              }}
            >
              {itemConfig?.icon && !hideIcon ? (
                <itemConfig.icon />
              ) : (
                <LegendIndicator
                  variant={variant}
                  dataKey={key}
                  colorsCount={colorsCount}
                />
              )}
              {itemConfig?.label}
            </div>
          );
        })}
    </div>
  );
}

// ---------------------------------------------------------------------------
// Legend indicator — each variant gets its own branch so future variants
// can diverge freely in markup & style.
// ---------------------------------------------------------------------------

function LegendIndicator({
  variant,
  dataKey,
  colorsCount,
}: {
  variant: ChartLegendVariant;
  dataKey: string;
  colorsCount: number;
}) {
  const fillStyle = getLegendFillStyle(dataKey, colorsCount);
  const outlineStyle = getLegendOutlineStyle(dataKey, colorsCount);

  switch (variant) {
    case "square":
      return <div className="h-2 w-2 shrink-0" style={fillStyle} />;

    case "circle":
      return <div className="h-2 w-2 shrink-0 rounded-full" style={fillStyle} />;

    case "circle-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-full p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "vertical-bar":
      return <div className="h-3 w-1 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "horizontal-bar":
      return <div className="h-1 w-3 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "rounded-square-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-[3px] p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "rounded-square":
    default:
      return <div className="h-2 w-2 shrink-0 rounded-[2px]" style={fillStyle} />;
  }
}

// ---------------------------------------------------------------------------
// Style helpers
// ---------------------------------------------------------------------------

/** Solid fill / gradient background for filled variants. */
function getLegendFillStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { backgroundColor: `var(--color-${dataKey}-0)` };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

/**
 * Outline style for stroke variants.
 * Uses background + mask-composite to punch out the center, leaving only the
 * "border" visible. Works with both solid colors and gradients, and respects
 * border-radius — unlike plain `border-color`.
 */
function getLegendOutlineStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  const maskStyle: React.CSSProperties = {
    WebkitMask:
      "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    WebkitMaskComposite: "xor",
    mask: "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    maskComposite: "exclude",
  };

  if (colorsCount <= 1) {
    return {
      backgroundColor: `var(--color-${dataKey}-0)`,
      ...maskStyle,
    };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return {
    background: `linear-gradient(to right, ${stops})`,
    ...maskStyle,
  };
}

const ChartLegend = RechartsPrimitive.Legend;

export { ChartLegend, ChartLegendContent, type ChartLegendVariant };
```
        
        
          Finally, create a file called `dot.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `dot.tsx`

```tsx
import { cn } from "@/lib/utils";
import * as React from "react";

export type DotVariant = "default" | "border" | "colored-border";

type ChartDotProps = {
  cx?: number;
  cy?: number;
  dataKey: string;
  chartId: string;
  className?: string;
  fillOpacity?: number;
  type?: DotVariant;
  /** Optional SVG <mask> id — lets the dot share an area's intro reveal wipe. */
  maskId?: string;
};

const ChartDot = React.memo(function ChartDot({
  cx,
  cy,
  dataKey,
  chartId,
  className,
  fillOpacity = 1,
  type = "default",
  maskId,
}: ChartDotProps) {
  const dotId = React.useId().replace(/:/g, "");
  const gradientUrl = `url(#${chartId}-colors-${String(dataKey)})`;

  if (cx === undefined || cy === undefined) return null;

  switch (type) {
    case "border":
      return (
        <PrimaryBorderDot
          cx={cx}
          cy={cy}
          dotId={dotId}
          fillOpacity={fillOpacity}
          gradientUrl={gradientUrl}
          className={className}
          maskId={maskId}
        />
      );
    case "colored-border":
      return (
        <ColoredBorderDot
          cx={cx}
          cy={cy}
          dotId={dotId}
          fillOpacity={fillOpacity}
          gradientUrl={gradientUrl}
          className={className}
          maskId={maskId}
        />
      );
    default:
      return (
        <DefaultDot
          cx={cx}
          cy={cy}
          dotId={dotId}
          fillOpacity={fillOpacity}
          gradientUrl={gradientUrl}
          className={className}
          maskId={maskId}
        />
      );
  }
});

type DotVariantProps = {
  cx: number;
  cy: number;
  dotId: string;
  fillOpacity: number;
  gradientUrl: string;
  className?: string;
  maskId?: string;
};

const DefaultDot = React.memo(
  ({ cx, cy, dotId, fillOpacity, gradientUrl, className, maskId }: DotVariantProps) => {
    const r = 3;
    return (
      <g className={className} mask={maskId ? `url(#${maskId})` : undefined}>
        <defs>
          <clipPath id={`dot-clip-${dotId}`}>
            <circle cx={cx} cy={cy} r={r} />
          </clipPath>
        </defs>
        {/* Full-width gradient rectangle clipped to dot shape */}
        <rect
          x="0"
          y={cy - r}
          width="100%"
          height={r * 2}
          fill={gradientUrl}
          fillOpacity={fillOpacity}
          clipPath={`url(#dot-clip-${dotId})`}
        />
      </g>
    );
  },
);

DefaultDot.displayName = "DefaultDot";

const PrimaryBorderDot = React.memo(
  ({ cx, cy, dotId, fillOpacity, gradientUrl, className, maskId }: DotVariantProps) => {
    const r = 6;
    const strokeWidth = 5;
    return (
      <g className={cn(className, "text-background")} mask={maskId ? `url(#${maskId})` : undefined}>
        <defs>
          <clipPath id={`dot-clip-${dotId}`}>
            <circle cx={cx} cy={cy} r={r} />
          </clipPath>
        </defs>
        {/* Background stroke (border) */}
        <circle cx={cx} cy={cy} r={r} fill="currentColor" />
        {/* Inner gradient circle clipped */}
        <rect
          x="0"
          y={cy - (r - strokeWidth / 2)}
          width="100%"
          height={(r - strokeWidth / 2) * 2}
          fill={gradientUrl}
          fillOpacity={fillOpacity}
          clipPath={`url(#dot-clip-inner-${dotId})`}
        />
        <defs>
          <clipPath id={`dot-clip-inner-${dotId}`}>
            <circle cx={cx} cy={cy} r={r - strokeWidth / 2} />
          </clipPath>
        </defs>
      </g>
    );
  },
);

PrimaryBorderDot.displayName = "PrimaryBorderDot";

const ColoredBorderDot = React.memo(
  ({ cx, cy, dotId, fillOpacity, gradientUrl, className, maskId }: DotVariantProps) => {
    const r = 3;
    const strokeWidth = 1;
    return (
      <g className={cn(className, "text-background")} mask={maskId ? `url(#${maskId})` : undefined}>
        <defs>
          <clipPath id={`dot-clip-${dotId}`}>
            <circle cx={cx} cy={cy} r={r + strokeWidth / 2} />
          </clipPath>
        </defs>
        {/* Gradient stroke (border) via clipped rect */}
        <rect
          x="0"
          y={cy - r - strokeWidth / 2}
          width="100%"
          height={(r + strokeWidth / 2) * 2}
          fill={gradientUrl}
          fillOpacity={fillOpacity}
          clipPath={`url(#dot-clip-${dotId})`}
        />
        {/* Inner solid fill */}
        <circle cx={cx} cy={cy} r={r - strokeWidth / 2} fill="currentColor" />
      </g>
    );
  },
);

ColoredBorderDot.displayName = "ColoredBorderDot";

export { ChartDot };
```
        
      
    
  


## Usage

The composed chart is composible. `<EvilComposedChart>` is the container, and you compose only the parts you need — `<Grid>`, `<XAxis>`, `<YAxis>`, `<Legend>`, `<Tooltip>`, and one or more `<Bar>` and `<Line>` — as its children. Each `<Bar>` carries its own `variant`, `glow`, and `isClickable`, and each `<Line>` its own `strokeVariant`, `curveType`, `glow`, and `isClickable`, so a single chart can freely mix bar and line styles.

```tsx
import {
  EvilComposedChart,
  Bar,
  Line,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/components/evilcharts/charts/composed-chart";
```

```tsx
const chartConfig = {
  revenue: {
    label: "Revenue",
    colors: { light: ["#3b82f6"], dark: ["#6A5ACD"] },
  },
  profit: {
    label: "Profit",
    colors: { light: ["#10b981"], dark: ["#34d399"] },
  },
} satisfies ChartConfig;

<EvilComposedChart xDataKey="month" data={data} config={chartConfig}>
  <Grid />
  <XAxis dataKey="month" />
  <YAxis />
  <Legend isClickable />
  <Tooltip />
  <Bar dataKey="revenue" variant="gradient" isClickable />
  <Line dataKey="profit" strokeVariant="dashed" isClickable>
    <Dot variant="default" />
    <ActiveDot variant="colored-border" />
  </Line>
</EvilComposedChart>
```

### Interactive Selection

Add `isClickable` to any `<Bar>` or `<Line>` (and to `<Legend>`) to make those series selectable. Use the `onSelectionChange` callback on `<EvilComposedChart>` to handle selection events:

```tsx
<EvilComposedChart
  data={data}
  config={chartConfig}
  onSelectionChange={(selectedDataKey) => {
    if (selectedDataKey) {
      console.log("Selected:", selectedDataKey);
    } else {
      console.log("Deselected");
    }
  }}
>
  <XAxis dataKey="month" />
  <Legend isClickable />
  <Tooltip />
  <Bar dataKey="revenue" isClickable />
  <Line dataKey="profit" isClickable />
</EvilComposedChart>
```

### Loading State

#### Live Example Code

##### Example File: `ex-loading-state-composed-chart.tsx`

```tsx
"use client";

import {
  EvilComposedChart,
  Bar,
  Line,
  XAxis,
  Grid,
  Tooltip,
  Legend,
} from "@/registry/charts/composed-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", revenue: 4200, profit: 1800 },
  { month: "February", revenue: 5800, profit: 2400 },
  { month: "March", revenue: 4100, profit: 1600 },
  { month: "April", revenue: 6200, profit: 2800 },
  { month: "May", revenue: 5400, profit: 2200 },
  { month: "June", revenue: 7800, profit: 3400 },
];

const chartConfig = {
  revenue: {
    label: "Revenue",
    colors: {
      light: ["#3b82f6"],
      dark: ["#6A5ACD"],
    },
  },
  profit: {
    label: "Profit",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleComposedChart() {
  return (
    <EvilComposedChart
      isLoading // [!code highlight]
      className="h-full w-full p-4"
      xDataKey="month"
      data={data}
      config={chartConfig}
    >
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend />
      <Tooltip />
      <Bar dataKey="revenue" />
      <Line dataKey="profit" />
    </EvilComposedChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    The composed chart supports loading state with a shimmer animation. You can pass the `isLoading` prop to the chart to show the loading state while your data is being fetched.
  


## Examples

Below are some examples of the composed chart with different `variants`. You can customize each `<Bar>` with a `variant`, and each `<Line>` with a `strokeVariant`, `curveType`, and other properties.

### Gradient Colors

#### Live Example Code

##### Example File: `ex-gradient-colors-composed-chart.tsx`

```tsx
"use client";

import {
  EvilComposedChart,
  Bar,
  Line,
  XAxis,
  Grid,
  Tooltip,
  Legend,
} from "@/registry/charts/composed-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", revenue: 4200, profit: 1800 },
  { month: "February", revenue: 5800, profit: 2400 },
  { month: "March", revenue: 4100, profit: 1600 },
  { month: "April", revenue: 6200, profit: 2800 },
  { month: "May", revenue: 5400, profit: 2200 },
  { month: "June", revenue: 7800, profit: 3400 },
  { month: "July", revenue: 6100, profit: 2600 },
  { month: "August", revenue: 8200, profit: 3800 },
  { month: "September", revenue: 5900, profit: 2500 },
  { month: "October", revenue: 6800, profit: 3000 },
  { month: "November", revenue: 7200, profit: 3200 },
  { month: "December", revenue: 9100, profit: 4200 },
];

const chartConfig = {
  revenue: {
    label: "Revenue",
    colors: {
      light: ["#f43f5e", "#ec4899", "#a855f7", "#6366f1", "#3b82f6"], // [!code highlight]
      dark: ["#f43f5e", "#ec4899", "#a855f7", "#6366f1", "#3b82f6"], // [!code highlight]
    },
  },
  profit: {
    label: "Profit",
    colors: {
      light: ["#10b981", "#14b8a6", "#06b6d4"], // [!code highlight]
      dark: ["#10b981", "#14b8a6", "#06b6d4"], // [!code highlight]
    },
  },
} satisfies ChartConfig;

export function EvilExampleComposedChart() {
  return (
    <EvilComposedChart className="h-full w-full p-4" xDataKey="month" data={data} config={chartConfig}>
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Bar dataKey="revenue" isClickable />
      <Line dataKey="profit" isClickable />
    </EvilComposedChart>
  );
}
```

### Bar Variants

" name="ex-hatched-variant-composed-chart"  />
" name="ex-duotone-variant-composed-chart"  />
" name="ex-gradient-variant-composed-chart"  />
" name="ex-stripped-variant-composed-chart"  />

### Line Stroke Variants

" name="ex-dashed-stroke-composed-chart"  />
" name="ex-animated-dashed-stroke-composed-chart"  />

### Curve Types

" name="ex-bump-curve-composed-chart"  />

### Line Dots

 and <ActiveDot />" name="ex-dots-composed-chart"  />
> [!NOTE]
 
  > [!NOTE]

    The composed chart supports dots on lines. Compose a `<Dot>` for the resting marker and an `<ActiveDot>` for the marker that appears on hover, inside a `<Line>`. Available variants: `default`, `border`, `colored-border`.
  


### Hover Highlight

" name="ex-hover-highlight-composed-chart"  />
> [!NOTE]
 
  > [!NOTE]

    The hover highlight feature dims other bars when you hover over a bar, making it easier to focus on specific data points. Set the `enableHoverHighlight` prop on a `<Bar>` to enable this feature.
  


### Glowing Effects

 and <Line glow />" name="ex-glowing-composed-chart"  />
> [!NOTE]
 
  > [!NOTE]

    Add a subtle glow effect to a series with the `glow` prop on a `<Bar>` or `<Line>`. Each glowing series renders its own scoped glow filter.
  


## API Reference

The chart is composed of several parts. The props below are grouped by the component they belong to.

### EvilComposedChart


The root container. It owns the data, the shared selection state, the loading skeleton, and the optional brush. Everything visual is composed as its children.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **data*** | `TData[]` | - | Data used to display the chart. An array of objects where each object represents a data point (`TData extends Record`). |
| **config** | - | - | " required>     Configuration object that defines every bar and line series. Each key should match a data key in your data array, with a corresponding color or color array. |
| **children*** | `ReactNode` | - | The composed chart parts — ``, ``, ``, ``, ``, and one or more `` and ``. |
| **className** | `string` | - | Additional CSS classes to apply to the chart container. |
| **curveType** | - | - | The default curve interpolation inherited by every ``. Each `` may override it locally. |
| **animationType** | - | - | Default intro inherited by every `` and `` — lines wipe in along this direction, bars grow up from their baseline staggered in this order. `"none"` disables it; devices with the OS reduce-motion preference fall back to `"none"` automatically. |
| **barGap** | `number` | - | The gap between bars sharing the same category. |
| **barCategoryGap** | `number` | - | The gap between different categories of bars. |
| **defaultSelectedDataKey** | `string | null` | `null` | The data key that should be selected by default. |
| **onSelectionChange** | - | - | void">     Callback fired when a series is selected or deselected — by clicking a clickable ``, ``, or `` entry. Receives the selected data key, or `null` when deselected. |
| **isLoading** | `boolean` | `false` | Shows a loading skeleton animation with a shimmer effect when data is being fetched. |
| **loadingBars** | `number` | `12` | Number of bars to display in the loading skeleton. |
| **showBrush** | `boolean` | `false` | When enabled, displays a brush control below the chart for selecting and zooming into a range of data. |
| **xDataKey** | `keyof TData & string` | - | The data key used for the x-axis. Only needed by the brush footer — the axis itself reads its key from ``. |
| **brushHeight** | `number` | - | The height of the brush preview area in pixels. |
| **brushFormatLabel** | - | - | string">     Custom formatter for the brush axis labels. |
| **onBrushChange** | - | - | void">     Callback invoked when the user changes the brush selection range. |
| **chartProps** | - | - | ">     Additional props forwarded to the underlying Recharts ComposedChart component. Read the Recharts ComposedChart documentation for available props. |



### Bar


A single bar series. Each `<Bar />` is self-contained and generates its own gradient/pattern definitions, so a chart can hold any number of bars — each with its own variant, glow, and clickability.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **dataKey*** | `string` | - | The series key. Must exist on both the data rows and the chart `config`. |
| **variant** | - | - | The visual style of the bar fill. Applies to this bar only. |
| **radius** | `number` | `4` | The corner radius of the bar in pixels. |
| **animationType** | - | - | The grow-in order for this bar series. Falls back to the chart's `animationType` when omitted. |
| **glow** | `boolean` | `false` | Applies a soft outer neon glow to this bar. |
| **isClickable** | `boolean` | `false` | Lets this bar be selected by clicking it. When any series is selected, unselected series become semi-transparent. |
| **enableHoverHighlight** | `boolean` | `false` | When enabled, hovering over a column dims this bar everywhere else, making it easier to focus on specific data points. |
| **barProps** | - | - | ">     Escape hatch for raw props forwarded to the underlying Recharts Bar component. |



### Line


A single line series. Each `<Line />` is self-contained and generates its own color gradient and glow filter, so a chart can hold any number of lines — each with its own stroke, curve, glow, and clickability.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **dataKey*** | `string` | - | The series key. Must exist on both the data rows and the chart `config`. |
| **strokeVariant** | - | - | The stroke style for this line. |
| **curveType** | - | - | The curve interpolation for this line. Falls back to the chart's `curveType` when omitted. |
| **animationType** | - | - | The intro reveal direction for this line. Falls back to the chart's `animationType` when omitted. |
| **connectNulls** | `boolean` | `false` | Whether to connect line segments across null or missing values. |
| **glow** | `boolean` | `false` | Applies a soft outer neon glow to this line. |
| **isClickable** | `boolean` | `false` | Lets this line be selected by clicking it. When any series is selected, unselected series become semi-transparent. |
| **children** | `ReactNode` | - | Optional `` and `` composition that adds point markers to this line. |
| **lineProps** | - | - | ">     Escape hatch for raw props forwarded to the underlying Recharts Line component. |



### Dot and ActiveDot


Point markers composed inside a `<Line />`. `<Dot />` is the resting marker; `<ActiveDot />` is the hovered marker. They render nothing on their own — the parent `<Line />` reads their `variant`.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style of the point marker. |



### XAxis and YAxis


The category and value axes. Both ship with the chart's flat default styling and forward every Recharts axis prop, so `dataKey`, `tickFormatter`, `tickMargin`, etc. pass straight through. They are hidden automatically while the chart is loading.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **dataKey** | `string` | - | The data key for the axis values. |
| **…axisProps** | - | - | Every other Recharts XAxis / YAxis prop is forwarded as-is. Read the Recharts XAxis and Recharts YAxis documentation for available props. |



### Grid


The background grid lines. Defaults to horizontal-only dashed lines and forwards every Recharts CartesianGrid prop.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **…gridProps** | - | - | Every Recharts CartesianGrid prop is forwarded as-is. Read the Recharts CartesianGrid documentation for available props. |



### Tooltip


The hover tooltip. It reads the chart's selection state so its content dims unselected series.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style of the tooltip surface. |
| **roundness** | - | - | Controls the border-radius of the tooltip. |
| **defaultIndex** | `number` | - | When set, the tooltip is visible by default at the specified data point index. |
| **cursor** | `boolean` | `true` | Whether the vertical cursor line follows the pointer on hover. |



### Legend


The series legend. When `isClickable` is set, each entry toggles selection of its series.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style of the legend indicators. |
| **align** | - | - | Horizontal placement of the legend. |
| **verticalAlign** | - | - | Vertical placement of the legend. |
| **isClickable** | `boolean` | `false` | Lets each legend entry toggle selection of its series. |

---

## Line Chart <a name="line-chart"></a>

> Simple static & beautifully designed line charts

#### Live Example Code

##### Example File: `ex-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      showBrush
      xDataKey="month"
      brushFormatLabel={(value) => String(value).substring(0, 3)}
    >
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```

## Installation


  
    
    
  
  
### CLI Installation


    ```bash
npx shadcn@latest add @evilcharts/line-chart
```
  
  
### Manual Installation


    
      
        #### Install the following dependencies:

        
          ```bash
npm install recharts
npm install motion
```
        
      
      
        #### Copy and paste the following code snippets into your project.

         
          To use the chart, first create the folder `evilcharts` and a subfolder called `charts` inside your `components` directory.
          Then, copy the following base line-chart code into a new file in that folder.
        
        
          #### Component Implementation

##### Source File: `line-chart.tsx`

```tsx
"use client";

import {
  type ChartConfig,
  ChartContainer,
  getColorsCount,
  getLoadingData,
  LoadingIndicator,
} from "@/registry/ui/chart";
import {
  CartesianGrid,
  Curve,
  Line as RechartsLine,
  LineChart as RechartsLineChart,
  XAxis as RechartsXAxis,
  YAxis as RechartsYAxis,
  type CurveProps,
} from "recharts";
import {
  ChartTooltip,
  ChartTooltipContent,
  type TooltipRoundness,
  type TooltipVariant,
} from "@/registry/ui/tooltip";
import { EvilBrush, useEvilBrush, type EvilBrushRange } from "@/registry/ui/evil-brush";
import { ChartLegend, ChartLegendContent, type ChartLegendVariant } from "@/registry/ui/legend";
import { ChartDot, type DotVariant } from "@/registry/ui/dot";
import {
  Children,
  createContext,
  isValidElement,
  use,
  useCallback,
  useId,
  useMemo,
  useRef,
  useState,
  type ComponentProps,
  type FC,
  type ReactElement,
  type ReactNode,
} from "react";
import { motion, useReducedMotion } from "motion/react";

// Constants
const STROKE_WIDTH = 1;
const LOADING_LINE_DATA_KEY = "loading";
const LOADING_ANIMATION_DURATION = 2000; // in milliseconds
const REVEAL_DURATION = 1; // intro wipe length, in seconds
const REVEAL_EASE: [number, number, number, number] = [0, 0.7, 0.5, 1]; // intro wipe easing

type CurveType = ComponentProps<typeof RechartsLine>["type"];
type LineDotProp = ComponentProps<typeof RechartsLine>["dot"];
type LineActiveDotProp = ComponentProps<typeof RechartsLine>["activeDot"];
type StrokeVariant = "solid" | "dashed" | "animated-dashed";

/**
 * Direction of the custom motion.dev intro reveal. Recharts' own line animation
 * is permanently disabled (it drew the line after the dots had already popped
 * in) — these reveals replace it.
 *
 * NOTE: a reveal is a per-frame animated SVG mask, so it is heavier than a
 * static chart. `"none"` opts out entirely; it is also what a device with the
 * OS "reduce motion" preference falls back to automatically.
 */
type LineAnimationType = "none" | "left-to-right" | "right-to-left" | "center-out" | "edges-in";
type RevealAnimationType = Exclude<LineAnimationType, "none">;

// ─────────────────────────────────────────────────────────────────────────────
// Shared context
// ─────────────────────────────────────────────────────────────────────────────

/**
 * Shared state for every part of the chart. Lifted into <EvilLineChart /> so that
 * <Line />, <XAxis />, <Legend />, and friends can read it without prop drilling.
 * Sub-components are composed freely — the provider is the single source of truth.
 */
type LineChartContextValue = {
  config: ChartConfig; // colors + labels for every series
  curveType: CurveType; // default curve interpolation each <Line /> inherits
  animationType: LineAnimationType; // default intro reveal each <Line /> inherits
  isLoading: boolean; // whether the chart shows its loading skeleton
  selectedDataKey: string | null; // currently selected series, or null when none
  selectDataKey: (dataKey: string | null) => void; // sets the selected series
};

const LineChartContext = createContext<LineChartContextValue | null>(null);

// Reads the chart context, throwing a helpful error when used outside <EvilLineChart />
function useLineChart() {
  const context = use(LineChartContext);

  if (!context) {
    throw new Error(
      "Line chart parts (<Line />, <XAxis />, …) must be used within <EvilLineChart />",
    );
  }

  return context;
}

// ─────────────────────────────────────────────────────────────────────────────
// Root container
// ─────────────────────────────────────────────────────────────────────────────

// Validates that every config key also exists on the data row type
type ValidateConfigKeys<TData, TConfig> = {
  [K in keyof TConfig]: K extends keyof TData ? ChartConfig[string] : never;
};

type EvilLineChartBaseProps<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
> = {
  config: TConfig & ValidateConfigKeys<TData, TConfig>; // series colors + labels
  data: TData[]; // rows rendered by the chart
  children: ReactNode; // composed parts — <Line />, <XAxis />, <Legend />, …
  className?: string; // extra classes for the chart container
  chartProps?: ComponentProps<typeof RechartsLineChart>; // escape hatch for the raw Recharts chart
  curveType?: CurveType; // default curve interpolation for every <Line />
  animationType?: LineAnimationType; // default intro reveal for every <Line />
  defaultSelectedDataKey?: string | null; // series selected on first render
  onSelectionChange?: (selectedDataKey: string | null) => void; // fires when the selected series changes
  isLoading?: boolean; // shows the animated loading skeleton
  loadingPoints?: number; // number of points in the loading skeleton
  showBrush?: boolean; // renders a zoom brush below the chart
  xDataKey?: keyof TData & string; // x-axis key — only needed for the brush footer
  brushHeight?: number; // height of the brush preview in pixels
  brushFormatLabel?: (value: unknown, index: number) => string; // formats brush axis labels
  onBrushChange?: (range: EvilBrushRange) => void; // fires when the brush range changes
};

type EvilLineChartProps<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
> = EvilLineChartBaseProps<TData, TConfig>;

/**
 * Root of the composible line chart. Owns the data, the shared context, the
 * loading skeleton, and the optional zoom brush. Everything visual — axes,
 * grid, tooltip, legend, and the lines themselves — is composed as children,
 * so a consumer renders exactly the parts they need.
 */
export function EvilLineChart<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
>({
  config,
  data,
  children,
  className,
  chartProps,
  curveType = "linear",
  animationType = "left-to-right",
  defaultSelectedDataKey = null,
  onSelectionChange,
  isLoading = false,
  loadingPoints,
  showBrush = false,
  xDataKey,
  brushHeight,
  brushFormatLabel,
  onBrushChange,
}: EvilLineChartProps<TData, TConfig>) {
  const chartId = useId().replace(/:/g, ""); // colon-free id keeps CSS/SVG selectors valid
  const [selectedDataKey, setSelectedDataKey] = useState<string | null>(defaultSelectedDataKey);
  const { loadingData, onShimmerExit } = useLoadingData(isLoading, loadingPoints);
  const { visibleData, brushProps } = useEvilBrush({ data });

  const displayData = showBrush && !isLoading ? visibleData : data;

  // Updates selection state and notifies the parent
  const selectDataKey = useCallback(
    (newSelectedDataKey: string | null) => {
      setSelectedDataKey(newSelectedDataKey);
      onSelectionChange?.(newSelectedDataKey);
    },
    [onSelectionChange],
  );

  const contextValue = useMemo<LineChartContextValue>(
    () => ({
      config,
      curveType,
      animationType,
      isLoading,
      selectedDataKey,
      selectDataKey,
    }),
    [config, curveType, animationType, isLoading, selectedDataKey, selectDataKey],
  );

  return (
    <LineChartContext value={contextValue}>
      <ChartContainer
        className={className}
        config={config}
        footer={
          showBrush &&
          !isLoading && (
            <EvilBrush
              data={data}
              chartConfig={config}
              xDataKey={xDataKey}
              variant="line"
              curveType={curveType}
              height={brushHeight}
              formatLabel={brushFormatLabel}
              skipStyle
              className="mt-1"
              {...brushProps}
              onChange={(range) => {
                brushProps.onChange(range);
                onBrushChange?.(range);
              }}
            />
          )
        }
      >
        <LoadingIndicator isLoading={isLoading} />
        <RechartsLineChart
          id={chartId}
          accessibilityLayer
          data={isLoading ? loadingData : displayData}
          {...chartProps}
        >
          {children}
          {isLoading && <LoadingLine chartId={chartId} curveType={curveType} onShimmerExit={onShimmerExit} />}
        </RechartsLineChart>
      </ChartContainer>
    </LineChartContext>
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Composible parts
// ─────────────────────────────────────────────────────────────────────────────

type LineProps = {
  dataKey: string; // series key — must exist on the data and config
  strokeVariant?: StrokeVariant; // stroke style for this line only
  curveType?: CurveType; // curve interpolation — falls back to the chart default
  animationType?: LineAnimationType; // intro reveal — falls back to the chart default
  connectNulls?: boolean; // join segments across null/missing values
  isClickable?: boolean; // lets this line be selected by clicking it
  glowing?: boolean; // applies a soft outer glow to this line
  enableBufferLine?: boolean; // renders this line's last segment as a dashed buffer
  children?: ReactNode; // optional <Dot /> and <ActiveDot /> composition
  lineProps?: ComponentProps<typeof RechartsLine>; // escape hatch for raw Recharts Line props
};

/**
 * A single line series. Each <Line /> is fully self-contained: it generates its
 * own gradient and glow definitions under a unique id, so any number of lines —
 * each with its own stroke, glow, and clickability — can live in one chart
 * without style collisions. Compose <Dot /> and <ActiveDot /> inside it to add
 * point markers.
 */
export function Line({
  dataKey,
  strokeVariant = "solid",
  curveType,
  animationType,
  connectNulls = false,
  isClickable = false,
  glowing = false,
  enableBufferLine = false,
  children,
  lineProps,
}: LineProps) {
  const {
    config,
    curveType: defaultCurve,
    animationType: defaultAnimation,
    isLoading,
    selectedDataKey,
    selectDataKey,
  } = useLineChart();
  const id = useId().replace(/:/g, ""); // unique id scopes this line's style defs
  // Devices set to "reduce motion" skip the intro reveal entirely
  const shouldReduceMotion = useReducedMotion();

  // The root renders the skeleton line while loading, so real lines step aside
  if (isLoading) return null;

  const resolvedCurve = curveType ?? defaultCurve;

  // The reveal is an animated SVG mask — heavier than a static chart — so
  // `"none"` and the OS reduce-motion preference both opt out of it.
  const revealType: LineAnimationType = shouldReduceMotion
    ? "none"
    : (animationType ?? defaultAnimation);
  const maskId = revealType === "none" ? undefined : `${id}-reveal-mask`;

  const isSelected = selectedDataKey === dataKey;
  const hasSelection = selectedDataKey !== null;
  const opacity = getOpacity(selectedDataKey, dataKey);

  const { dot, activeDot } = resolveDots(children, id, dataKey, opacity.dot, maskId);

  const isAnimatedDashed = strokeVariant === "animated-dashed";
  const isDashed = strokeVariant === "dashed" || isAnimatedDashed;

  return (
    <>
      <g key={dataKey}>
        {isClickable && (
          <RechartsLine
            type={resolvedCurve}
            dataKey={dataKey}
            connectNulls={connectNulls}
            stroke="transparent"
            strokeWidth={15}
            dot={false}
            activeDot={false}
            isAnimationActive={false}
            legendType="none"
            tooltipType="none"
            style={{ cursor: "pointer" }}
            onClick={() => selectDataKey(isSelected ? null : dataKey)}
          />
        )}
        <RechartsLine
          type={resolvedCurve}
          dataKey={dataKey}
          connectNulls={connectNulls}
          strokeOpacity={opacity.stroke}
          stroke={`url(#${id}-colors-${dataKey})`}
          filter={glowing ? `url(#${id}-glow-${dataKey})` : undefined}
          dot={dot}
          activeDot={activeDot}
          strokeWidth={STROKE_WIDTH}
          strokeDasharray={getStrokeDasharray(enableBufferLine, isDashed)}
          shape={enableBufferLine ? bufferLineShape : undefined}
          // Recharts' built-in line animation is permanently disabled — it drew
          // the line after the dots had already popped in. The motion.dev reveal
          // mask drives the intro instead, wiping stroke and dots in together.
          isAnimationActive={false}
          style={{
            ...(maskId ? { mask: `url(#${maskId})` } : {}),
            ...(isClickable ? { cursor: "pointer" } : {}),
          }}
          onClick={() => {
            if (!isClickable) return;
            // Clicking the selected line clears the selection, otherwise selects it
            selectDataKey(isSelected ? null : dataKey);
          }}
          {...lineProps}
        >
          {isAnimatedDashed && !hasSelection && <AnimatedDashedStroke />}
        </RechartsLine>
      </g>
      <defs>
        {revealType !== "none" && <RevealMask id={id} type={revealType} />}
        <ColorGradient id={id} dataKey={dataKey} config={config} />
        {glowing && <GlowFilter id={id} dataKey={dataKey} />}
      </defs>
    </>
  );
}

type DotProps = {
  variant?: DotVariant; // visual style of the point marker
};

/**
 * Declares a resting point marker for the <Line /> it is composed inside.
 * It renders nothing on its own — the parent <Line /> reads its variant and
 * wires it into the Recharts dot slot.
 */
export const Dot: FC<DotProps> = () => null;

/**
 * Declares the hovered/active point marker for the <Line /> it is composed
 * inside. Like <Dot />, it is a configuration slot and renders nothing itself.
 */
export const ActiveDot: FC<DotProps> = () => null;

type XAxisProps = ComponentProps<typeof RechartsXAxis>;

/**
 * The horizontal category axis. Ships with the chart's flat default styling and
 * forwards every Recharts XAxis prop, so `dataKey`, `tickFormatter`, etc. are
 * passed straight through. Hidden automatically while the chart is loading.
 */
export function XAxis({
  tickLine = false,
  axisLine = false,
  tickMargin = 8,
  minTickGap = 8,
  ...props
}: XAxisProps) {
  const { isLoading } = useLineChart();

  if (isLoading) return null;

  return (
    <RechartsXAxis
      tickLine={tickLine}
      axisLine={axisLine}
      tickMargin={tickMargin}
      minTickGap={minTickGap}
      {...props}
    />
  );
}

type YAxisProps = ComponentProps<typeof RechartsYAxis>;

/**
 * The vertical value axis. Ships with the chart's flat default styling and
 * forwards every Recharts YAxis prop. Hidden automatically while the chart is
 * loading.
 */
export function YAxis({
  tickLine = false,
  axisLine = false,
  tickMargin = 8,
  minTickGap = 8,
  width = "auto",
  ...props
}: YAxisProps) {
  const { isLoading } = useLineChart();

  if (isLoading) return null;

  return (
    <RechartsYAxis
      tickLine={tickLine}
      axisLine={axisLine}
      tickMargin={tickMargin}
      minTickGap={minTickGap}
      width={width}
      {...props}
    />
  );
}

type GridProps = ComponentProps<typeof CartesianGrid>;

/**
 * The background grid lines. Defaults to horizontal-only dashed lines and
 * forwards every Recharts CartesianGrid prop for full control.
 */
export function Grid({ vertical = false, strokeDasharray = "3 3", ...props }: GridProps) {
  return <CartesianGrid vertical={vertical} strokeDasharray={strokeDasharray} {...props} />;
}

type TooltipProps = {
  variant?: TooltipVariant; // visual style of the tooltip surface
  roundness?: TooltipRoundness; // border-radius of the tooltip
  defaultIndex?: number; // data index shown by default with no hover
  cursor?: boolean; // whether the vertical cursor line follows the pointer
};

/**
 * The hover tooltip. Reads the chart's selection from context so its content
 * dims unselected series. Hidden automatically while the chart is loading.
 */
export function Tooltip({ variant, roundness, defaultIndex, cursor = true }: TooltipProps) {
  const { isLoading, selectedDataKey } = useLineChart();

  if (isLoading) return null;

  return (
    <ChartTooltip
      defaultIndex={defaultIndex}
      cursor={cursor ? { strokeDasharray: "3 3", strokeWidth: STROKE_WIDTH } : false}
      content={
        <ChartTooltipContent selected={selectedDataKey} roundness={roundness} variant={variant} />
      }
    />
  );
}

type LegendProps = {
  variant?: ChartLegendVariant; // visual style of the legend indicators
  align?: "left" | "center" | "right"; // horizontal placement
  verticalAlign?: "top" | "middle" | "bottom"; // vertical placement
  isClickable?: boolean; // lets each entry toggle selection of its series
};

/**
 * The series legend. When `isClickable` is set, each entry toggles selection of
 * its series, driving the shared selection state read by every <Line />.
 */
export function Legend({
  variant,
  align = "right",
  verticalAlign = "top",
  isClickable = false,
}: LegendProps) {
  const { selectedDataKey, selectDataKey } = useLineChart();

  return (
    <ChartLegend
      verticalAlign={verticalAlign}
      align={align}
      content={
        <ChartLegendContent
          selected={selectedDataKey}
          onSelectChange={selectDataKey}
          isClickable={isClickable}
          variant={variant}
        />
      }
    />
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Selection + dot helpers
// ─────────────────────────────────────────────────────────────────────────────

// Returns stroke/dot opacity — dims a series only when another is selected
const getOpacity = (selectedDataKey: string | null, dataKey: string) => {
  if (selectedDataKey === null) {
    return { stroke: 1, dot: 1 };
  }

  return selectedDataKey === dataKey ? { stroke: 1, dot: 1 } : { stroke: 0.3, dot: 0.3 };
};

// Resolves a line's stroke-dasharray — the buffer line manages its own dashes
const getStrokeDasharray = (enableBufferLine: boolean, isDashed: boolean) => {
  if (enableBufferLine) return undefined;

  return isDashed ? "5 5" : undefined;
};

// Pulls <Dot /> and <ActiveDot /> out of a line's children into Recharts dot slots.
// When a `maskId` is given the resting dot is wired to the intro reveal mask so it
// wipes in with the line; the active dot is always left unmasked since it only
// appears on hover, after the intro has finished.
const resolveDots = (
  children: ReactNode,
  id: string,
  dataKey: string,
  dotOpacity: number,
  maskId: string | undefined,
): { dot: LineDotProp; activeDot: LineActiveDotProp } => {
  let dot: LineDotProp = false;
  let activeDot: LineActiveDotProp = false;

  Children.forEach(children, (child) => {
    if (!isValidElement(child)) return;

    if (child.type === Dot) {
      const { variant } = (child as ReactElement<DotProps>).props;
      dot = (
        <ChartDot
          type={variant}
          dataKey={dataKey}
          chartId={id}
          fillOpacity={dotOpacity}
          maskId={maskId}
        />
      );
    }

    if (child.type === ActiveDot) {
      const { variant } = (child as ReactElement<DotProps>).props;
      activeDot = (
        <ChartDot type={variant} dataKey={dataKey} chartId={id} fillOpacity={dotOpacity} />
      );
    }
  });

  return { dot, activeDot };
};

// ─────────────────────────────────────────────────────────────────────────────
// Buffer line
// ─────────────────────────────────────────────────────────────────────────────

// Buffer line shape — renders the last segment as dashed while the rest stays solid.
// Renders a single <Curve> and uses a ref callback to measure the actual SVG path
// length via getTotalLength() + getPointAtLength(), then sets stroke-dasharray
// imperatively. Works correctly with any curve type (linear, natural, monotone, etc.).
type CurvePoint = NonNullable<NonNullable<CurveProps["points"]>[number]>;
type DrawableCurvePoint = CurvePoint & { x: number; y: number };

const isDrawableCurvePoint = (point: CurvePoint): point is DrawableCurvePoint => {
  return typeof point.x === "number" && typeof point.y === "number";
};

const BUFFER_DASH_SIZE = 4;
const BUFFER_GAP_SIZE = 3;

// Binary-search the path to find the length at which path.x ≈ targetX,
// using the browser's native getPointAtLength for exact curve measurement.
const findLengthAtX = (path: SVGPathElement, totalLength: number, targetX: number): number => {
  let lo = 0;
  let hi = totalLength;
  // ~0.5px precision is more than enough for a dasharray split
  while (hi - lo > 0.5) {
    const mid = (lo + hi) / 2;
    const pt = path.getPointAtLength(mid);
    if (pt.x < targetX) lo = mid;
    else hi = mid;
  }
  return (lo + hi) / 2;
};

const bufferLineShape = (props: CurveProps) => {
  const { points, ...rest } = props;

  if (!points || points.length < 2) {
    return <Curve {...props} />;
  }

  const drawablePoints = points.filter(isDrawableCurvePoint);

  if (drawablePoints.length < 2) {
    return <Curve {...props} />;
  }

  // x coordinate of the second-to-last point — where solid meets dashed
  const splitX = drawablePoints[drawablePoints.length - 2].x;

  // Ref callback runs synchronously during React commit (before browser paint),
  // so there's no visible flash of an un-dashed line.
  const gRef = (g: SVGGElement | null) => {
    if (!g) return;
    const path = g.querySelector("path");
    if (!path) return;

    const totalLength = path.getTotalLength();
    const solidLength = findLengthAtX(path, totalLength, splitX);
    const lastSegmentLength = totalLength - solidLength;

    // Build dasharray: solid run, then repeating dash-gap for the buffer segment
    const reps = Math.ceil(lastSegmentLength / (BUFFER_DASH_SIZE + BUFFER_GAP_SIZE)) + 1;
    const dashedPart = Array.from(
      { length: reps },
      () => `${BUFFER_DASH_SIZE} ${BUFFER_GAP_SIZE}`,
    ).join(" ");

    path.setAttribute("stroke-dasharray", `${solidLength} 0 ${dashedPart}`);
  };

  return (
    <g ref={gRef}>
      <Curve {...rest} points={drawablePoints} />
    </g>
  );
};

// ─────────────────────────────────────────────────────────────────────────────
// Style definitions — one set per <Line />, scoped to its unique id
// ─────────────────────────────────────────────────────────────────────────────

type StyleProps = {
  id: string; // unique id of the owning <Line />
  dataKey: string; // series key the style belongs to
};

// Animated dashed-stroke effect, rendered as a child of the Recharts Line
const AnimatedDashedStroke = () => {
  return (
    <>
      <animate
        attributeName="stroke-dasharray"
        values="5 5; 0 5; 5 5"
        dur="1s"
        repeatCount="indefinite"
        keyTimes="0;0.5;1"
      />
      <animate
        attributeName="stroke-dashoffset"
        values="0; -10"
        dur="1s"
        repeatCount="indefinite"
        keyTimes="0;1"
      />
    </>
  );
};

// motion `originX` for each single-rect reveal — the edge the wipe grows from.
// 0 = left edge, 1 = right edge, 0.5 = centre (grows outward to both edges).
const SINGLE_REVEAL_ORIGIN: Record<Exclude<RevealAnimationType, "edges-in">, number> = {
  "left-to-right": 0,
  "right-to-left": 1,
  "center-out": 0.5,
};

/**
 * Wipe mask driven by motion.dev, played once when a <Line /> mounts. The same
 * mask is applied to the line's stroke and its resting dots, so both reveal in
 * lockstep — fixing Recharts' default, where the dots appeared before the line
 * had finished drawing.
 *
 * `maskUnits`/`maskContentUnits` are both userSpaceOnUse so every masked element
 * shares one coordinate space and the wipe edge lands at the same x on each.
 *
 * Each rect animates `scaleX` 0 → 1; `originX` decides which edge it grows from.
 * "edges-in" needs two rects — each half grows inward from an opposite edge.
 */
const RevealMask = ({ id, type }: { id: string; type: RevealAnimationType }) => {
  const reveal = {
    initial: { scaleX: 0 },
    animate: { scaleX: 1 },
    transition: { duration: REVEAL_DURATION, ease: REVEAL_EASE },
  };

  return (
    <mask
      id={`${id}-reveal-mask`}
      maskUnits="userSpaceOnUse"
      maskContentUnits="userSpaceOnUse"
      x="0"
      y="0"
      width="100%"
      height="100%"
    >
      {type === "edges-in" ? (
        <>
          {/* left half wipes inward from the left edge toward the centre */}
          <motion.rect
            {...reveal}
            x="0"
            y="0"
            width="50%"
            height="100%"
            fill="white"
            style={{ originX: 0 }}
          />
          {/* right half wipes inward from the right edge toward the centre */}
          <motion.rect
            {...reveal}
            x="50%"
            y="0"
            width="50%"
            height="100%"
            fill="white"
            style={{ originX: 1 }}
          />
        </>
      ) : (
        <motion.rect
          {...reveal}
          x="0"
          y="0"
          width="100%"
          height="100%"
          fill="white"
          style={{ originX: SINGLE_REVEAL_ORIGIN[type] }}
        />
      )}
    </mask>
  );
};

/**
 * Horizontal left-to-right color gradient for a series. Always rendered — the
 * line's stroke and its dots all paint from this single gradient.
 */
const ColorGradient = ({ id, dataKey, config }: StyleProps & { config: ChartConfig }) => {
  const colorsCount = getColorsCount(config[dataKey] ?? {});

  return (
    <linearGradient id={`${id}-colors-${dataKey}`} x1="0" y1="0" x2="1" y2="0">
      {colorsCount === 1 ? (
        <>
          <stop offset="0%" stopColor={`var(--color-${dataKey}-0)`} />
          <stop offset="100%" stopColor={`var(--color-${dataKey}-0)`} />
        </>
      ) : (
        Array.from({ length: colorsCount }, (_, index) => {
          const offset = `${(index / (colorsCount - 1)) * 100}%`;
          return (
            <stop
              key={offset}
              offset={offset}
              stopColor={`var(--color-${dataKey}-${index}, var(--color-${dataKey}-0))`}
            />
          );
        })
      )}
    </linearGradient>
  );
};

/** Soft outer glow filter applied to a glowing line. */
const GlowFilter = ({ id, dataKey }: StyleProps) => {
  return (
    <filter id={`${id}-glow-${dataKey}`} x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceGraphic" stdDeviation="10" result="blur" />
      <feColorMatrix
        in="blur"
        type="matrix"
        values="1 0 0 0 0
                0 1 0 0 0
                0 0 1 0 0
                0 0 0 2 0"
        result="glow"
      />
      <feMerge>
        <feMergeNode in="glow" />
        <feMergeNode in="SourceGraphic" />
      </feMerge>
    </filter>
  );
};

// ─────────────────────────────────────────────────────────────────────────────
// Loading skeleton
// ─────────────────────────────────────────────────────────────────────────────

// Builds bell-curve eased gradient stops for the loading shimmer
const generateEasedGradientStops = (
  steps: number = 17,
  minOpacity: number = 0.05,
  maxOpacity: number = 0.9,
) => {
  return Array.from({ length: steps }, (_, i) => {
    const t = i / (steps - 1); // 0 to 1
    // Sine-based bell curve easing: peaks at center (t=0.5), smooth falloff at edges
    const eased = Math.sin(t * Math.PI) ** 2;
    const opacity = minOpacity + eased * (maxOpacity - minOpacity);
    return { offset: `${(t * 100).toFixed(0)}%`, opacity: Number(opacity.toFixed(3)) };
  });
};

/**
 * Hook to manage loading data with pixel-perfect shimmer synchronization.
 *
 * Uses motion.dev's onUpdate callback to ensure chart data is only regenerated
 * when the shimmer has completely exited the visible area. This eliminates
 * timing drift issues from setTimeout/setInterval.
 */
export function useLoadingData(isLoading: boolean, loadingPoints: number = 14) {
  const [loadingDataKey, setLoadingDataKey] = useState(false);

  // Callback fired by motion.dev when the shimmer exits the visible area
  const onShimmerExit = useCallback(() => {
    if (isLoading) {
      setLoadingDataKey((prev) => !prev);
    }
  }, [isLoading]);

  const loadingData = useMemo(
    () => getLoadingData(loadingPoints),
    // loadingDataKey toggle triggers re-computation when the shimmer exits
    // eslint-disable-next-line react-hooks/exhaustive-deps
    [loadingPoints, loadingDataKey],
  );

  return { loadingData, onShimmerExit };
}

/**
 * The skeleton line shown while the chart is loading. Rendered by the root in
 * place of the real lines, paired with its own masked shimmer pattern.
 */
const LoadingLine = ({
  chartId,
  curveType,
  onShimmerExit,
}: {
  chartId: string;
  curveType: CurveType;
  onShimmerExit: () => void;
}) => {
  return (
    <>
      <RechartsLine
        type={curveType}
        dataKey={LOADING_LINE_DATA_KEY}
        min={0}
        max={100}
        stroke="currentColor"
        strokeOpacity={0.5}
        isAnimationActive={false}
        legendType="none"
        tooltipType="none"
        activeDot={false}
        dot={false}
        strokeWidth={STROKE_WIDTH}
        style={{ mask: `url(#${chartId}-loading-mask)` }}
      />
      <defs>
        <LoadingPattern chartId={chartId} onShimmerExit={onShimmerExit} />
      </defs>
    </>
  );
};

/**
 * Animated shimmer pattern for the loading skeleton.
 *
 * The visible chart area is normalized to 0-1, the shimmer gradient has width 1,
 * and the pattern is 3x wide so the shimmer has buffer on both sides. The motion
 * rect travels x from -1 to 2; onShimmerExit fires as it crosses x=1, letting the
 * data swap happen while the shimmer is off-screen for a seamless loop.
 */
const LoadingPattern = ({
  chartId,
  onShimmerExit,
}: {
  chartId: string;
  onShimmerExit: () => void;
}) => {
  const gradientStops = generateEasedGradientStops();

  // 1 (left buffer) + 1 (visible) + 1 (right buffer)
  const patternWidth = 3;
  const startX = -1;
  const endX = 2;

  // Tracks the last x value to detect the exit threshold crossing
  const lastXRef = useRef(startX);

  return (
    <>
      <linearGradient id={`${chartId}-loading-gradient`} x1="0" y1="0" x2="1" y2="0">
        {gradientStops.map(({ offset, opacity }) => (
          <stop key={offset} offset={offset} stopColor="white" stopOpacity={opacity} />
        ))}
      </linearGradient>
      <pattern
        id={`${chartId}-loading-pattern`}
        patternUnits="objectBoundingBox"
        patternContentUnits="objectBoundingBox"
        patternTransform="rotate(25)"
        width={patternWidth}
        height="1"
        x="0"
        y="0"
      >
        <motion.rect
          y="0"
          width="1"
          height="1"
          fill={`url(#${chartId}-loading-gradient)`}
          initial={{ x: startX }}
          animate={{ x: endX }}
          transition={{
            duration: LOADING_ANIMATION_DURATION / 1000,
            ease: "linear",
            repeat: Infinity,
            repeatType: "loop",
          }}
          onUpdate={(latest) => {
            const xValue = typeof latest.x === "number" ? latest.x : startX;
            const lastX = lastXRef.current;

            // Fire once per loop, when the shimmer fully exits the visible area
            if (xValue >= 1 && lastX < 1) {
              onShimmerExit();
            }

            lastXRef.current = xValue;
          }}
        />
      </pattern>
      <mask id={`${chartId}-loading-mask`} maskUnits="userSpaceOnUse">
        <rect width="100%" height="100%" fill={`url(#${chartId}-loading-pattern)`} />
      </mask>
    </>
  );
};
```
        
      
       
        #### Now, Let's add the chart component to your project.

        
          These Components are required by the chart component to render the chart. Make a folder called `ui` inside the `evilcharts` folder and paste the code there.

          Below is main chart component.
        
        
          #### Component Implementation

##### Source File: `chart.tsx`

```tsx
"use client";

import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

// Format: { THEME_NAME: CSS_SELECTOR }
const THEMES = { light: "", dark: ".dark" } as const;

type ThemeKey = keyof typeof THEMES;

// All Keys are optional at first
type ThemeColorsBase = {
  [K in ThemeKey]?: string[];
};

// Require at least one theme key
type AtLeastOneThemeColor = {
  [K in ThemeKey]: Required<Pick<ThemeColorsBase, K>> & Partial<Omit<ThemeColorsBase, K>>;
}[ThemeKey];

const VALID_THEME_KEYS = Object.keys(THEMES) as ThemeKey[];

// Validation for chart config colors at runtime
function validateChartConfigColors(config: ChartConfig): void {
  for (const [key, value] of Object.entries(config)) {
    if (value.colors) {
      const hasValidThemeKey = VALID_THEME_KEYS.some(
        (themeKey) => value.colors?.[themeKey] !== undefined,
      );

      if (!hasValidThemeKey) {
        throw new Error(
          `[EvilCharts] Invalid chart config for "${key}": colors object must have at least one theme key (${VALID_THEME_KEYS.join(", ")}). Received empty object or invalid keys.`,
        );
      }
    }
  }
}

export type ChartConfig = Record<
  string,
  {
    label?: React.ReactNode;
    icon?: React.ComponentType;
    colors?: AtLeastOneThemeColor;
  }
>;

interface ChartContextProps {
  config: ChartConfig;
}

const ChartContext = React.createContext<ChartContextProps | null>(null);

export function useChart() {
  const context = React.useContext(ChartContext);

  if (!context) {
    throw new Error("useChart must be used within a <ChartContainer />");
  }

  return context;
}

interface ChartContainerProps
  extends
    Omit<React.ComponentProps<"div">, "children">,
    Pick<
      React.ComponentProps<typeof RechartsPrimitive.ResponsiveContainer>,
      | "initialDimension"
      | "aspect"
      | "debounce"
      | "minHeight"
      | "minWidth"
      | "maxHeight"
      | "height"
      | "width"
      | "onResize"
      | "children"
    > {
  config: ChartConfig;
  innerResponsiveContainerStyle?: React.ComponentProps<
    typeof RechartsPrimitive.ResponsiveContainer
  >["style"];
  /** Optional content rendered below the chart (e.g. EvilBrush) */
  footer?: React.ReactNode;
}

function ChartContainer({
  id,
  config,
  initialDimension = { width: 320, height: 200 },
  className,
  children,
  footer,
  ...props
}: Readonly<ChartContainerProps>) {
  const uniqueId = React.useId();
  const chartId = `chart-${id ?? uniqueId.replace(/:/g, "")}`;

  // Validate chart config at runtime
  validateChartConfigColors(config);

  return (
    <ChartContext.Provider value={{ config }}>
      <div
        data-slot="chart"
        data-chart={chartId}
        className={cn(
          "min-h-0 w-full flex-1",
          "[&_.recharts-cartesian-axis-tick_text]:fill-muted-foreground [&_.recharts-cartesian-grid_line[stroke='#ccc']]:stroke-border/50 [&_.recharts-curve.recharts-tooltip-cursor]:stroke-border [&_.recharts-polar-grid_[stroke='#ccc']]:stroke-border [&_.recharts-radial-bar-background-sector]:fill-muted [&_.recharts-rectangle.recharts-tooltip-cursor]:fill-muted [&_.recharts-reference-line_[stroke='#ccc']]:stroke-border relative flex flex-col justify-center text-xs [&_.recharts-dot[stroke='#fff']]:stroke-transparent [&_.recharts-layer]:outline-hidden [&_.recharts-sector]:outline-hidden [&_.recharts-sector[stroke='#fff']]:stroke-transparent [&_.recharts-surface]:outline-hidden",
          !footer && "aspect-video",
          className,
        )}
        {...props}
      >
        <ChartStyle id={chartId} config={config} />
        <RechartsPrimitive.ResponsiveContainer
          className="min-h-0 w-full flex-1"
          initialDimension={initialDimension}
        >
          {children}
        </RechartsPrimitive.ResponsiveContainer>
        {footer}
      </div>
    </ChartContext.Provider>
  );
}

function LoadingIndicator({ isLoading }: { isLoading: boolean }) {
  if (!isLoading) {
    return null;
  }

  return (
    <div className="pointer-events-none absolute inset-0 z-20 flex items-center justify-center">
      <div className="text-primary bg-background flex items-center justify-center gap-2 rounded-md border px-2 py-0.5 text-sm">
        <div className="border-border border-t-primary h-3 w-3 animate-spin rounded-full border" />
        <span>Loading</span>
      </div>
    </div>
  );
}

// Distribute colors evenly across slots, extra slots go to last color(s)
// Example: 2 colors for 4 slots → [red, red, pink, pink]
// Example: 3 colors for 4 slots → [red, pink, blue, blue]
function distributeColors(colorsArray: string[], maxCount: number): string[] {
  const availableCount = colorsArray.length;
  if (availableCount >= maxCount) {
    return colorsArray.slice(0, maxCount);
  }

  const result: string[] = [];
  const baseSlots = Math.floor(maxCount / availableCount);
  const extraSlots = maxCount % availableCount;

  // First (availableCount - extraSlots) colors get baseSlots each
  // Last extraSlots colors get (baseSlots + 1) each
  for (let colorIdx = 0; colorIdx < availableCount; colorIdx++) {
    const isExtraColor = colorIdx >= availableCount - extraSlots;
    const slotsForThisColor = baseSlots + (isExtraColor ? 1 : 0);
    for (let j = 0; j < slotsForThisColor; j++) {
      result.push(colorsArray[colorIdx]);
    }
  }

  return result;
}

const ChartStyle = ({ id, config }: { id: string; config: ChartConfig }) => {
  const colorConfig = Object.entries(config).filter(([, config]) => config.colors);

  if (!colorConfig.length) {
    return null;
  }

  const generateCssVars = (theme: keyof typeof THEMES) =>
    colorConfig
      .flatMap(([key, itemConfig]) => {
        const colorsArray = itemConfig.colors?.[theme];
        if (!colorsArray || !Array.isArray(colorsArray) || colorsArray.length === 0) {
          return [];
        }

        // Get max count across all themes for this key
        const maxCount = getColorsCount(itemConfig);

        // Distribute colors evenly across all required slots
        const distributedColors = distributeColors(colorsArray, maxCount);

        return distributedColors.map((color, index) => `  --color-${key}-${index}: ${color};`);
      })
      .filter(Boolean)
      .join("\n");

  const css = Object.entries(THEMES)
    .map(
      ([theme, prefix]) =>
        `${prefix} [data-chart=${id}] {\n${generateCssVars(theme as keyof typeof THEMES)}\n}`,
    )
    .join("\n");

  return <style dangerouslySetInnerHTML={{ __html: css }} />;
};

// Helper to extract item config from a payload.
export function getPayloadConfigFromPayload(config: ChartConfig, payload: unknown, key: string) {
  if (typeof payload !== "object" || payload === null) {
    return undefined;
  }

  const payloadPayload =
    "payload" in payload && typeof payload.payload === "object" && payload.payload !== null
      ? payload.payload
      : undefined;

  let configLabelKey: string = key;

  if (key in payload && typeof payload[key as keyof typeof payload] === "string") {
    configLabelKey = payload[key as keyof typeof payload] as string;
  } else if (
    payloadPayload &&
    key in payloadPayload &&
    typeof payloadPayload[key as keyof typeof payloadPayload] === "string"
  ) {
    configLabelKey = payloadPayload[key as keyof typeof payloadPayload] as string;
  }

  return configLabelKey in config ? config[configLabelKey] : config[key];
}

// Format values to percent for expanded charts
function axisValueToPercentFormatter(value: number) {
  return `${Math.round(value * 100).toFixed(0)}%`;
}

// Get max colors count across all themes for a config entry
function getColorsCount(config: ChartConfig[string]): number {
  if (!config.colors) return 1;
  const counts = VALID_THEME_KEYS.map((theme) => config.colors?.[theme]?.length ?? 0);
  return Math.max(...counts, 1);
}

// Generate random loading data for skeleton/loading state
// min/max represent percentage of the range (0-100), defaults to 20-80 for realistic look
export const getLoadingData = (points: number = 10, min: number = 0, max: number = 70) => {
  const range = max - min;
  return Array.from({ length: points }, () => ({
    loading: Math.floor(Math.random() * range) + min,
  }));
};

export {
  ChartContainer,
  ChartStyle,
  axisValueToPercentFormatter,
  LoadingIndicator,
  getColorsCount,
};
```
        
      
       
        #### Now, We need to add sub components.

        
          Create a file called `tooltip.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `tooltip.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import type { NameType, ValueType } from "recharts/types/component/DefaultTooltipContent";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type TooltipRoundness = "sm" | "md" | "lg" | "xl";
type TooltipVariant = "default" | "frosted-glass";

const roundnessMap: Record<TooltipRoundness, string> = {
  sm: "rounded-sm",
  md: "rounded-md",
  lg: "rounded-lg",
  xl: "rounded-xl",
};

const variantMap: Record<TooltipVariant, string> = {
  default: "bg-background",
  "frosted-glass": "bg-background/70 backdrop-blur-sm",
};

function ChartTooltipContent({
  active,
  payload,
  className,
  indicator = "dot",
  hideLabel = false,
  hideIndicator = false,
  label,
  labelFormatter,
  labelClassName,
  formatter,
  nameKey,
  labelKey,
  selected,
  roundness = "lg",
  variant = "default",
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip> &
  React.ComponentProps<"div"> & {
    hideLabel?: boolean;
    hideIndicator?: boolean;
    indicator?: "line" | "dot" | "dashed";
    nameKey?: string;
    labelKey?: string;
    selected?: string | null;
    roundness?: TooltipRoundness;
    variant?: TooltipVariant;
  } & Omit<
    RechartsPrimitive.DefaultTooltipContentProps<ValueType, NameType>,
    "accessibilityLayer"
  >) {
  const { config } = useChart();

  const tooltipLabel = React.useMemo(() => {
    if (hideLabel || !payload?.length) {
      return null;
    }

    const [item] = payload;
    const key = `${labelKey ?? item?.dataKey ?? item?.name ?? "value"}`;
    const itemConfig = getPayloadConfigFromPayload(config, item, key);
    const value =
      !labelKey && typeof label === "string" ? (config[label]?.label ?? label) : itemConfig?.label;

    if (labelFormatter) {
      return (
        <div className={cn("font-medium", labelClassName)}>{labelFormatter(value, payload)}</div>
      );
    }

    if (!value) {
      return null;
    }

    return <div className={cn("font-medium", labelClassName)}>{value}</div>;
  }, [label, labelFormatter, payload, hideLabel, labelClassName, config, labelKey]);

  if (!active || !payload?.length) {
    // Empty tooltip - to prevent position getting 0.0 so it doesnt animate tooltip every time from 0.0 origin
    return <span className="p-4" />;
  }

  const nestLabel = payload.length === 1 && indicator !== "dot";

  return (
    <div
      className={cn(
        "border-border/50 grid min-w-32 items-start gap-1.5 border px-2.5 py-1.5 text-xs shadow-xl",
        roundnessMap[roundness],
        variantMap[variant],
        className,
      )}
    >
      {!nestLabel ? tooltipLabel : null}
      <div className="grid gap-1.5">
        {payload
          .filter((item) => item.type !== "none")
          .map((item, index) => {
            // For pie charts, item.name contains the sector name (e.g., "chrome")
            // For radial charts, the name is in item.payload[nameKey]
            // For other charts, item.name or item.dataKey contains the series name
            const payloadName =
              nameKey && item.payload
                ? (item.payload as Record<string, unknown>)[nameKey]
                : undefined;
            const key = `${payloadName ?? item.name ?? item.dataKey ?? "value"}`;
            const itemConfig = getPayloadConfigFromPayload(config, item, key);

            // Get colors count for this item to determine gradient vs solid
            const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

            return (
              <div
                key={index}
                className={cn(
                  "[&>svg]:text-muted-foreground flex w-full flex-wrap items-stretch gap-2 [&>svg]:h-2.5 [&>svg]:w-2.5",
                  indicator === "dot" && "items-center",
                  selected != null && selected !== item.dataKey && "opacity-30",
                )}
              >
                {formatter && item?.value !== undefined && item.name ? (
                  formatter(item.value, item.name, item, index, item.payload)
                ) : (
                  <>
                    {itemConfig?.icon ? (
                      <itemConfig.icon />
                    ) : (
                      !hideIndicator && (
                        <div
                          className={cn("shrink-0 rounded-[2px]", {
                            "h-2.5 w-2.5": indicator === "dot",
                            "w-1": indicator === "line",
                            "w-0 border-[1.5px] border-dashed bg-transparent!":
                              indicator === "dashed",
                            "my-0.5": nestLabel && indicator === "dashed",
                          })}
                          style={getIndicatorColorStyle(key, colorsCount)}
                        />
                      )
                    )}
                    <div
                      className={cn(
                        "flex flex-1 justify-between gap-4 leading-none",
                        nestLabel ? "items-end" : "items-center",
                      )}
                    >
                      <div className="grid gap-1.5">
                        {nestLabel ? tooltipLabel : null}
                        <span className="text-muted-foreground">
                          {itemConfig?.label ?? item.name}
                        </span>
                      </div>
                      {item.value != null && (
                        <span className="text-foreground font-mono font-medium tabular-nums">
                          {typeof item.value === "number"
                            ? item.value.toLocaleString()
                            : String(item.value)}
                        </span>
                      )}
                    </div>
                  </>
                )}
              </div>
            );
          })}
      </div>
    </div>
  );
}

function getIndicatorColorStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { background: `var(--color-${dataKey}-0)` };
  }

  // Multiple colors: create linear gradient with evenly distributed stops
  const stops = Array.from({ length: colorsCount }, (_, index) => {
    const offset = (index / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${index}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

const ChartTooltip = ({
  animationDuration = 200,
  ...props
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip>) => (
  <RechartsPrimitive.Tooltip animationDuration={animationDuration} {...props} />
);

export { ChartTooltip, ChartTooltipContent };
export type { TooltipRoundness, TooltipVariant };
```
        
        
          Now, create another file called `legend.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `legend.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type ChartLegendVariant =
  | "square"
  | "circle"
  | "circle-outline"
  | "rounded-square"
  | "rounded-square-outline"
  | "vertical-bar"
  | "horizontal-bar";

function ChartLegendContent({
  className,
  hideIcon = false,
  nameKey,
  payload,
  verticalAlign,
  align = "right",
  selected,
  onSelectChange,
  isClickable,
  variant = "rounded-square",
}: React.ComponentProps<"div"> & {
  hideIcon?: boolean;
  nameKey?: string;
  selected?: string | null;
  isClickable?: boolean;
  onSelectChange?: (selected: string | null) => void;
  variant?: ChartLegendVariant;
} & RechartsPrimitive.DefaultLegendContentProps) {
  const { config } = useChart();

  if (!payload?.length) {
    return null;
  }

  return (
    <div
      className={cn(
        "flex items-center gap-4 select-none",
        align === "left" && "justify-start",
        align === "center" && "justify-center",
        align === "right" && "justify-end",
        verticalAlign === "top" ? "pb-4" : "pt-4",
        className,
      )}
    >
      {payload
        .filter((item) => item.type !== "none")
        .map((item) => {
          // For pie charts, item.value contains the sector name (e.g., "chrome")
          // For radial charts, the name is in item.payload[nameKey]
          // For other charts, item.dataKey contains the series name (e.g., "desktop")
          const payloadName =
            nameKey && item.payload
              ? (item.payload as Record<string, unknown>)[nameKey]
              : undefined;
          const key = `${payloadName ?? item.value ?? item.dataKey ?? "value"}`;
          const itemConfig = getPayloadConfigFromPayload(config, item, key);
          const isSelected = selected === null || selected === key;

          // Get colors count for this item to determine gradient vs solid
          const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

          return (
            <div
              key={key}
              className={cn(
                "[&>svg]:text-muted-foreground flex items-center gap-1.5 transition-opacity [&>svg]:h-3 [&>svg]:w-3",
                !isSelected && "opacity-30",
                isClickable && "cursor-pointer",
              )}
              onClick={() => {
                if (!isClickable) return;

                onSelectChange?.(selected === key ? null : key);
              }}
            >
              {itemConfig?.icon && !hideIcon ? (
                <itemConfig.icon />
              ) : (
                <LegendIndicator
                  variant={variant}
                  dataKey={key}
                  colorsCount={colorsCount}
                />
              )}
              {itemConfig?.label}
            </div>
          );
        })}
    </div>
  );
}

// ---------------------------------------------------------------------------
// Legend indicator — each variant gets its own branch so future variants
// can diverge freely in markup & style.
// ---------------------------------------------------------------------------

function LegendIndicator({
  variant,
  dataKey,
  colorsCount,
}: {
  variant: ChartLegendVariant;
  dataKey: string;
  colorsCount: number;
}) {
  const fillStyle = getLegendFillStyle(dataKey, colorsCount);
  const outlineStyle = getLegendOutlineStyle(dataKey, colorsCount);

  switch (variant) {
    case "square":
      return <div className="h-2 w-2 shrink-0" style={fillStyle} />;

    case "circle":
      return <div className="h-2 w-2 shrink-0 rounded-full" style={fillStyle} />;

    case "circle-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-full p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "vertical-bar":
      return <div className="h-3 w-1 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "horizontal-bar":
      return <div className="h-1 w-3 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "rounded-square-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-[3px] p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "rounded-square":
    default:
      return <div className="h-2 w-2 shrink-0 rounded-[2px]" style={fillStyle} />;
  }
}

// ---------------------------------------------------------------------------
// Style helpers
// ---------------------------------------------------------------------------

/** Solid fill / gradient background for filled variants. */
function getLegendFillStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { backgroundColor: `var(--color-${dataKey}-0)` };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

/**
 * Outline style for stroke variants.
 * Uses background + mask-composite to punch out the center, leaving only the
 * "border" visible. Works with both solid colors and gradients, and respects
 * border-radius — unlike plain `border-color`.
 */
function getLegendOutlineStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  const maskStyle: React.CSSProperties = {
    WebkitMask:
      "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    WebkitMaskComposite: "xor",
    mask: "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    maskComposite: "exclude",
  };

  if (colorsCount <= 1) {
    return {
      backgroundColor: `var(--color-${dataKey}-0)`,
      ...maskStyle,
    };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return {
    background: `linear-gradient(to right, ${stops})`,
    ...maskStyle,
  };
}

const ChartLegend = RechartsPrimitive.Legend;

export { ChartLegend, ChartLegendContent, type ChartLegendVariant };
```
        
        
          Finally, create last file called `dot.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `dot.tsx`

```tsx
import { cn } from "@/lib/utils";
import * as React from "react";

export type DotVariant = "default" | "border" | "colored-border";

type ChartDotProps = {
  cx?: number;
  cy?: number;
  dataKey: string;
  chartId: string;
  className?: string;
  fillOpacity?: number;
  type?: DotVariant;
  /** Optional SVG <mask> id — lets the dot share an area's intro reveal wipe. */
  maskId?: string;
};

const ChartDot = React.memo(function ChartDot({
  cx,
  cy,
  dataKey,
  chartId,
  className,
  fillOpacity = 1,
  type = "default",
  maskId,
}: ChartDotProps) {
  const dotId = React.useId().replace(/:/g, "");
  const gradientUrl = `url(#${chartId}-colors-${String(dataKey)})`;

  if (cx === undefined || cy === undefined) return null;

  switch (type) {
    case "border":
      return (
        <PrimaryBorderDot
          cx={cx}
          cy={cy}
          dotId={dotId}
          fillOpacity={fillOpacity}
          gradientUrl={gradientUrl}
          className={className}
          maskId={maskId}
        />
      );
    case "colored-border":
      return (
        <ColoredBorderDot
          cx={cx}
          cy={cy}
          dotId={dotId}
          fillOpacity={fillOpacity}
          gradientUrl={gradientUrl}
          className={className}
          maskId={maskId}
        />
      );
    default:
      return (
        <DefaultDot
          cx={cx}
          cy={cy}
          dotId={dotId}
          fillOpacity={fillOpacity}
          gradientUrl={gradientUrl}
          className={className}
          maskId={maskId}
        />
      );
  }
});

type DotVariantProps = {
  cx: number;
  cy: number;
  dotId: string;
  fillOpacity: number;
  gradientUrl: string;
  className?: string;
  maskId?: string;
};

const DefaultDot = React.memo(
  ({ cx, cy, dotId, fillOpacity, gradientUrl, className, maskId }: DotVariantProps) => {
    const r = 3;
    return (
      <g className={className} mask={maskId ? `url(#${maskId})` : undefined}>
        <defs>
          <clipPath id={`dot-clip-${dotId}`}>
            <circle cx={cx} cy={cy} r={r} />
          </clipPath>
        </defs>
        {/* Full-width gradient rectangle clipped to dot shape */}
        <rect
          x="0"
          y={cy - r}
          width="100%"
          height={r * 2}
          fill={gradientUrl}
          fillOpacity={fillOpacity}
          clipPath={`url(#dot-clip-${dotId})`}
        />
      </g>
    );
  },
);

DefaultDot.displayName = "DefaultDot";

const PrimaryBorderDot = React.memo(
  ({ cx, cy, dotId, fillOpacity, gradientUrl, className, maskId }: DotVariantProps) => {
    const r = 6;
    const strokeWidth = 5;
    return (
      <g className={cn(className, "text-background")} mask={maskId ? `url(#${maskId})` : undefined}>
        <defs>
          <clipPath id={`dot-clip-${dotId}`}>
            <circle cx={cx} cy={cy} r={r} />
          </clipPath>
        </defs>
        {/* Background stroke (border) */}
        <circle cx={cx} cy={cy} r={r} fill="currentColor" />
        {/* Inner gradient circle clipped */}
        <rect
          x="0"
          y={cy - (r - strokeWidth / 2)}
          width="100%"
          height={(r - strokeWidth / 2) * 2}
          fill={gradientUrl}
          fillOpacity={fillOpacity}
          clipPath={`url(#dot-clip-inner-${dotId})`}
        />
        <defs>
          <clipPath id={`dot-clip-inner-${dotId}`}>
            <circle cx={cx} cy={cy} r={r - strokeWidth / 2} />
          </clipPath>
        </defs>
      </g>
    );
  },
);

PrimaryBorderDot.displayName = "PrimaryBorderDot";

const ColoredBorderDot = React.memo(
  ({ cx, cy, dotId, fillOpacity, gradientUrl, className, maskId }: DotVariantProps) => {
    const r = 3;
    const strokeWidth = 1;
    return (
      <g className={cn(className, "text-background")} mask={maskId ? `url(#${maskId})` : undefined}>
        <defs>
          <clipPath id={`dot-clip-${dotId}`}>
            <circle cx={cx} cy={cy} r={r + strokeWidth / 2} />
          </clipPath>
        </defs>
        {/* Gradient stroke (border) via clipped rect */}
        <rect
          x="0"
          y={cy - r - strokeWidth / 2}
          width="100%"
          height={(r + strokeWidth / 2) * 2}
          fill={gradientUrl}
          fillOpacity={fillOpacity}
          clipPath={`url(#dot-clip-${dotId})`}
        />
        {/* Inner solid fill */}
        <circle cx={cx} cy={cy} r={r - strokeWidth / 2} fill="currentColor" />
      </g>
    );
  },
);

ColoredBorderDot.displayName = "ColoredBorderDot";

export { ChartDot };
```
        
      
    
  


## Usage

The line chart is composible. `<EvilLineChart>` is the container, and you compose only the parts you need — `<Grid>`, `<XAxis>`, `<YAxis>`, `<Legend>`, `<Tooltip>`, and one or more `<Line>` — as its children. Each `<Line>` carries its own `strokeVariant`, `curveType`, `glowing`, `enableBufferLine`, and `isClickable`, so a single chart can mix stroke styles and make only some series interactive.

```tsx
import {
  EvilLineChart,
  Line,
  XAxis,
  YAxis,
  Grid,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/components/evilcharts/charts/line-chart";
```

```tsx
<EvilLineChart data={data} config={chartConfig} curveType="monotone">
  <Grid />
  <XAxis dataKey="month" />
  <YAxis />
  <Legend isClickable />
  <Tooltip />
  <Line dataKey="desktop" strokeVariant="solid" isClickable>
    <Dot variant="border" />
    <ActiveDot variant="colored-border" />
  </Line>
  <Line dataKey="mobile" strokeVariant="dashed" glowing>
    <ActiveDot variant="default" />
  </Line>
</EvilLineChart>
```

### Interactive Selection

Add `isClickable` to any `<Line>` (and to `<Legend>`) to make those series selectable. Use the `onSelectionChange` callback on `<EvilLineChart>` to handle selection events:

```tsx
<EvilLineChart
  data={data}
  config={chartConfig}
  onSelectionChange={(selectedDataKey) => {
    if (selectedDataKey) {
      console.log("Selected:", selectedDataKey);
    } else {
      console.log("Deselected");
    }
  }}
>
  <XAxis dataKey="month" />
  <Legend isClickable />
  <Tooltip />
  <Line dataKey="desktop" strokeVariant="solid" isClickable />
  <Line dataKey="mobile" strokeVariant="solid" isClickable />
</EvilLineChart>
```

### Loading State

#### Live Example Code

##### Example File: `ex-loading-state-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  YAxis,
  Legend,
  Tooltip,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart
      data={[]} // if isLoading is true, pass empty array → i.e isLoading ? [] : data
      config={chartConfig}
      className="h-full w-full p-4"
      isLoading={true} // [!code highlight]
      curveType="bump"
    >
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <ActiveDot variant="default" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <ActiveDot variant="default" />
      </Line>
    </EvilLineChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    The line chart supports loading state. You can pass the `isLoading` prop to the chart to show the loading state. You can also pass `curveType` to customize the curve type of the loading state. Here, im using `curveType='bump'` to make the loading state more realistic.
  


### Buffer Line

#### Live Example Code

##### Example File: `ex-buffer-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      showBrush
      xDataKey="month"
    >
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line
        dataKey="desktop"
        strokeVariant="solid"
        enableBufferLine // [!code highlight]
        isClickable
      >
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line
        dataKey="mobile"
        strokeVariant="solid"
        enableBufferLine // [!code highlight]
        isClickable
      >
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    When `enableBufferLine` is enabled, the last segment of each line is rendered as a dashed/dotted pattern while the rest of the line stays solid. This is useful for indicating projected, estimated, or incomplete data at the end of a series — commonly seen in financial charts and forecasting dashboards.
  


## Examples

Below are some examples of the line chart with different `variants`. where you can change the `curveType` & `strokeVariant`. 

### Gradient Colors

#### Live Example Code

##### Example File: `ex-gradient-colors-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["red", "orange", "rosybrown", "purple", "blue"], // [!code highlight]
      dark: ["red", "orange", "rosybrown", "purple", "blue"], // [!code highlight]
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["gray"],
      dark: ["gray"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Line>
    </EvilLineChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-gradient-colors-bump-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["red", "orange", "rosybrown", "purple", "blue"],
      dark: ["red", "orange", "rosybrown", "purple", "blue"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["gray"],
      dark: ["gray"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      curveType="bump" // [!code highlight]
    >
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Line>
    </EvilLineChart>
  );
}
```

### Curve Types

#### Live Example Code

##### Example File: `ex-bump-curve-type-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  YAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      curveType="bump" // [!code highlight]
    >
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="default" />
        <ActiveDot variant="default" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="default" />
        <ActiveDot variant="default" />
      </Line>
    </EvilLineChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-step-curve-type-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  YAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      curveType="step" // [!code highlight]
    >
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="default" />
        <ActiveDot variant="default" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="default" />
        <ActiveDot variant="default" />
      </Line>
    </EvilLineChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-monotoney-curve-type-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  YAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart
      data={data}
      config={chartConfig}
      className="h-full w-full p-4"
      curveType="monotoneY" // [!code highlight]
    >
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="default" />
        <ActiveDot variant="default" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="default" />
        <ActiveDot variant="default" />
      </Line>
    </EvilLineChart>
  );
}
```

### Stroke Variants

#### Live Example Code

##### Example File: `ex-solid-stroke-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  YAxis,
  Legend,
  Tooltip,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Line
        dataKey="desktop"
        strokeVariant="solid" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Line>
      <Line
        dataKey="mobile"
        strokeVariant="solid" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Line>
    </EvilLineChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-dashed-stroke-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  YAxis,
  Legend,
  Tooltip,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Line
        dataKey="desktop"
        strokeVariant="dashed" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Line>
      <Line
        dataKey="mobile"
        strokeVariant="dashed" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Line>
    </EvilLineChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-animated-dashed-stroke-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  YAxis,
  Legend,
  Tooltip,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <YAxis dataKey="desktop" />
      <Legend isClickable />
      <Tooltip />
      <Line
        dataKey="desktop"
        strokeVariant="animated-dashed" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Line>
      <Line
        dataKey="mobile"
        strokeVariant="animated-dashed" // [!code highlight]
        isClickable
      >
        <ActiveDot variant="default" />
      </Line>
    </EvilLineChart>
  );
}
```

### Glowing Lines

#### Live Example Code

##### Example File: `ex-glowing-desktop-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["red", "orange", "rosybrown", "purple", "blue"],
      dark: ["red", "orange", "rosybrown", "purple", "blue"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["gray"],
      dark: ["gray"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line
        dataKey="desktop"
        strokeVariant="solid"
        glowing // [!code highlight]
        isClickable
      >
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Line>
    </EvilLineChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-glowing-mobile-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line
        dataKey="mobile"
        strokeVariant="solid"
        glowing // [!code highlight]
        isClickable
      >
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```


## API Reference

The chart is composed of several parts. The props below are grouped by the component they belong to.

### EvilLineChart


The root container. It owns the data, the shared selection state, the loading skeleton, and the optional brush. Everything visual is composed as its children.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **data*** | `TData[]` | - | Data used to display the chart. An array of objects where each object represents a data point (`TData extends Record`). |
| **config** | - | - | " required>     Configuration object that defines the chart's series. Each key should match a data key in your data array, with a corresponding color or color array. |
| **children*** | `ReactNode` | - | The composed chart parts — ``, ``, ``, ``, ``, and one or more ``. |
| **className** | `string` | - | Additional CSS classes to apply to the chart container. |
| **curveType** | - | - | The default curve interpolation inherited by every ``. Each `` may override it locally. |
| **animationType** | - | - | Direction of the custom intro reveal inherited by every ``. `"none"` disables it — devices with the OS reduce-motion preference fall back to `"none"` automatically. |
| **defaultSelectedDataKey** | `string | null` | `null` | The data key that should be selected by default. |
| **onSelectionChange** | - | - | void">     Callback fired when a series is selected or deselected — by clicking a clickable `` or `` entry. Receives the selected data key, or `null` when deselected. |
| **isLoading** | `boolean` | `false` | Shows a loading skeleton animation with a shimmer effect when data is being fetched. |
| **loadingPoints** | `number` | `14` | Number of data points to display in the loading skeleton. |
| **showBrush** | `boolean` | `false` | When enabled, displays a brush control below the chart for selecting and zooming into a range of data. |
| **xDataKey** | `keyof TData & string` | - | The data key used for the x-axis. Only needed by the brush footer — the axis itself reads its key from ``. |
| **brushHeight** | `number` | - | The height of the brush preview area in pixels. |
| **brushFormatLabel** | - | - | string">     Custom formatter for the brush axis labels. |
| **onBrushChange** | - | - | void">     Callback invoked when the user changes the brush selection range. |
| **chartProps** | - | - | ">     Additional props forwarded to the underlying Recharts LineChart component. Read the Recharts LineChart documentation for available props. |



### Line


A single line series. Each `<Line />` is self-contained and generates its own gradient and glow definitions, so a chart can hold any number of lines — each with its own stroke, glow, and clickability.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **dataKey*** | `string` | - | The series key. Must exist on both the data rows and the chart `config`. |
| **strokeVariant** | - | - | The visual style for this line's stroke. Applies to this line only. |
| **curveType** | - | - | The curve interpolation for this line. Falls back to the chart's `curveType` when omitted. |
| **animationType** | - | - | The intro reveal animation for this line. Falls back to the chart's `animationType` when omitted. |
| **connectNulls** | `boolean` | `false` | Whether to connect line segments across null or missing values. |
| **isClickable** | `boolean` | `false` | Lets this line be selected by clicking it. When any line is selected, unselected lines become semi-transparent. |
| **glowing** | `boolean` | `false` | Applies a soft outer glow to this line. |
| **enableBufferLine** | `boolean` | `false` | Renders this line's last segment as a dashed buffer while the rest stays solid. Useful for indicating projected or incomplete data at the end of a series. |
| **children** | `ReactNode` | - | Optional `` and `` composition that adds point markers to this line. |
| **lineProps** | - | - | ">     Escape hatch for raw props forwarded to the underlying Recharts Line component. |



### Dot and ActiveDot


Point markers composed inside a `<Line />`. `<Dot />` is the resting marker; `<ActiveDot />` is the hovered marker. They render nothing on their own — the parent `<Line />` reads their `variant`.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style of the point marker. |



### XAxis and YAxis


The category and value axes. Both ship with the chart's flat default styling and forward every Recharts axis prop, so `dataKey`, `tickFormatter`, `tickMargin`, etc. pass straight through. They are hidden automatically while the chart is loading.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **dataKey** | `string` | - | The data key for the axis values. |
| **…axisProps** | - | - | Every other Recharts XAxis / YAxis prop is forwarded as-is. Read the Recharts XAxis and Recharts YAxis documentation for available props. |



### Grid


The background grid lines. Defaults to horizontal-only dashed lines and forwards every Recharts CartesianGrid prop.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **…gridProps** | - | - | Every Recharts CartesianGrid prop is forwarded as-is. Read the Recharts CartesianGrid documentation for available props. |



### Tooltip


The hover tooltip. It reads the chart's selection state so its content dims unselected series.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style of the tooltip surface. |
| **roundness** | - | - | Controls the border-radius of the tooltip. |
| **defaultIndex** | `number` | - | When set, the tooltip is visible by default at the specified data point index. |
| **cursor** | `boolean` | `true` | Whether the vertical cursor line follows the pointer on hover. |



### Legend


The series legend. When the chart is clickable, each entry toggles selection of its series.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style of the legend indicators. |
| **align** | - | - | Horizontal placement of the legend. |
| **verticalAlign** | - | - | Vertical placement of the legend. |
| **isClickable** | `boolean` | `false` | Lets each legend entry toggle selection of its series. |

---

## Pie Chart <a name="pie-chart"></a>

> Simple static & beautifully designed pie charts with donut, gradient, and glow effects

#### Live Example Code

##### Example File: `ex-pie-chart.tsx`

```tsx
"use client";

import { EvilPieChart, Pie, Tooltip, Legend } from "@/registry/charts/pie-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
  { browser: "edge", visitors: 173 },
  { browser: "other", visitors: 90 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  safari: {
    label: "Safari",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
  firefox: {
    label: "Firefox",
    colors: {
      light: ["#f59e0b"],
      dark: ["#fbbf24"],
    },
  },
  edge: {
    label: "Edge",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  other: {
    label: "Other",
    colors: {
      light: ["#6b7280"],
      dark: ["#9ca3af"],
    },
  },
} satisfies ChartConfig;

export function EvilExamplePieChart() {
  return (
    <EvilPieChart
      className="h-full w-full p-4"
      data={data}
      dataKey="visitors"
      nameKey="browser"
      config={chartConfig}
    >
      <Legend isClickable />
      <Tooltip />
      <Pie isClickable />
    </EvilPieChart>
  );
}
```

## Installation


  
    
    
  
  
### CLI Installation


    ```bash
npx shadcn@latest add @evilcharts/pie-chart
```
  
  
### Manual Installation


    
      
        #### Install the following dependencies:

        
          ```bash
npm install recharts
```
        
      
      
        #### Copy and paste the following code snippets into your project.

         
          To use the chart, first create the folder `evilcharts` and a subfolder called `charts` inside your `components` directory.
          Then, copy the following base pie-chart code into a new file in that folder.
        
        
          #### Component Implementation

##### Source File: `pie-chart.tsx`

```tsx
"use client";

import {
  type ChartConfig,
  ChartContainer,
  getColorsCount,
  LoadingIndicator,
} from "@/registry/ui/chart";
import { ChartLegend, ChartLegendContent, type ChartLegendVariant } from "@/registry/ui/legend";
import {
  ChartTooltip,
  ChartTooltipContent,
  type TooltipRoundness,
  type TooltipVariant,
} from "@/registry/ui/tooltip";
import { ChartBackground, type BackgroundVariant } from "@/registry/ui/background";
import {
  Children,
  createContext,
  isValidElement,
  use,
  useCallback,
  useId,
  useMemo,
  useState,
  type ComponentProps,
  type FC,
  type ReactElement,
  type ReactNode,
} from "react";
import {
  LabelList as RechartsLabelList,
  Pie as RechartsPie,
  PieChart as RechartsPieChart,
  Sector,
  type PieSectorShapeProps,
} from "recharts";
import { motion } from "motion/react";

// Constants
const LOADING_SECTORS = 5;
const LOADING_ANIMATION_DURATION = 2000; // full loading cycle duration in milliseconds
const DEFAULT_INNER_RADIUS = 0;
const DEFAULT_OUTER_RADIUS = "80%";
const DEFAULT_CORNER_RADIUS = 0;
const DEFAULT_PADDING_ANGLE = 0;
const DEFAULT_START_ANGLE = 0;
const DEFAULT_END_ANGLE = 360;
// Stable empty-array reference so the `glowingSectors` default doesn't change every render
const EMPTY_GLOWING_SECTORS: string[] = [];

type LabelListProps = ComponentProps<typeof RechartsLabelList>;

// ─────────────────────────────────────────────────────────────────────────────
// Shared context
// ─────────────────────────────────────────────────────────────────────────────

/**
 * Shared state for every part of the chart. Lifted into <EvilPieChart /> so that
 * <Pie />, <Tooltip />, <Legend />, and friends can read it without prop drilling.
 * Sub-components are composed freely — the provider is the single source of truth.
 */
type PieChartContextValue = {
  config: ChartConfig; // colors + labels for every sector
  data: Record<string, unknown>[]; // rows rendered by the chart
  dataKey: string; // key holding each sector's numeric value
  nameKey: string; // key holding each sector's name
  isLoading: boolean; // whether the chart shows its loading skeleton
  selectedSector: string | null; // currently selected sector name, or null when none
  selectSector: (sectorName: string | null) => void; // sets the selected sector
};

const PieChartContext = createContext<PieChartContextValue | null>(null);

// Reads the chart context, throwing a helpful error when used outside <EvilPieChart />
function usePieChart() {
  const context = use(PieChartContext);

  if (!context) {
    throw new Error(
      "Pie chart parts (<Pie />, <Tooltip />, …) must be used within <EvilPieChart />",
    );
  }

  return context;
}

// ─────────────────────────────────────────────────────────────────────────────
// Root container
// ─────────────────────────────────────────────────────────────────────────────

type EvilPieChartProps<TData extends Record<string, unknown>> = {
  config: ChartConfig; // sector colors + labels
  data: TData[]; // rows rendered by the chart
  dataKey: keyof TData & string; // key holding each sector's numeric value
  nameKey: keyof TData & string; // key holding each sector's name
  children: ReactNode; // composed parts — <Pie />, <Tooltip />, <Legend />, …
  className?: string; // extra classes for the chart container
  chartProps?: ComponentProps<typeof RechartsPieChart>; // escape hatch for the raw Recharts chart
  defaultSelectedSector?: string | null; // sector selected on first render
  onSelectionChange?: (selection: { dataKey: string; value: number } | null) => void; // fires when the selected sector changes
  isLoading?: boolean; // shows the animated loading skeleton
};

/**
 * Root of the composible pie chart. Owns the data, the shared context, and the
 * loading skeleton. Everything visual — the pie itself, tooltip, legend, and an
 * optional background — is composed as children, so a consumer renders exactly
 * the parts they need.
 */
export function EvilPieChart<TData extends Record<string, unknown>>({
  config,
  data,
  dataKey,
  nameKey,
  children,
  className,
  chartProps,
  defaultSelectedSector = null,
  onSelectionChange,
  isLoading = false,
}: EvilPieChartProps<TData>) {
  const [selectedSector, setSelectedSector] = useState<string | null>(defaultSelectedSector);

  // Updates selection state and notifies the parent with the sector's value
  const selectSector = useCallback(
    (sectorName: string | null) => {
      setSelectedSector(sectorName);

      if (sectorName === null) {
        onSelectionChange?.(null);
        return;
      }

      const selectedItem = data.find((item) => (item[nameKey] as string) === sectorName);

      if (selectedItem) {
        onSelectionChange?.({ dataKey: sectorName, value: selectedItem[dataKey] as number });
      }
    },
    [data, dataKey, nameKey, onSelectionChange],
  );

  const contextValue = useMemo<PieChartContextValue>(
    () => ({
      config,
      data,
      dataKey,
      nameKey,
      isLoading,
      selectedSector,
      selectSector,
    }),
    [config, data, dataKey, nameKey, isLoading, selectedSector, selectSector],
  );

  return (
    <PieChartContext value={contextValue}>
      <ChartContainer className={className} config={config}>
        <LoadingIndicator isLoading={isLoading} />
        <RechartsPieChart id="evil-charts-pie-chart" accessibilityLayer {...chartProps}>
          {children}
        </RechartsPieChart>
      </ChartContainer>
    </PieChartContext>
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Composible parts
// ─────────────────────────────────────────────────────────────────────────────

type PieProps = {
  variant?: PieVariant; // fill style for the pie's sectors
  innerRadius?: number | string; // inner radius — set above 0 for a donut
  outerRadius?: number | string; // outer radius of the pie
  cornerRadius?: number; // border-radius of each sector in pixels
  paddingAngle?: number; // gap between sectors in degrees — negative overlaps them
  startAngle?: number; // angle the pie starts drawing from
  endAngle?: number; // angle the pie stops drawing at
  isClickable?: boolean; // lets sectors be selected by clicking them
  glowingSectors?: string[]; // sector names that render with a soft outer glow
  children?: ReactNode; // optional <Label /> composition for sector labels
  pieProps?: Omit<ComponentProps<typeof RechartsPie>, "data" | "dataKey" | "nameKey">; // escape hatch for raw Recharts Pie props
};

/**
 * The pie series. Self-contained: it generates its own radial color gradients
 * and glow filters under a unique id, so any number of pies — each with its own
 * shape and clickability — can live on one page without style collisions. While
 * the chart is loading it renders an animated skeleton in place of the data.
 * Compose <Label /> inside it to draw labels on each sector.
 */
export function Pie({
  variant = "gradient",
  innerRadius = DEFAULT_INNER_RADIUS,
  outerRadius = DEFAULT_OUTER_RADIUS,
  cornerRadius = DEFAULT_CORNER_RADIUS,
  paddingAngle = DEFAULT_PADDING_ANGLE,
  startAngle = DEFAULT_START_ANGLE,
  endAngle = DEFAULT_END_ANGLE,
  isClickable = false,
  glowingSectors = EMPTY_GLOWING_SECTORS,
  children,
  pieProps,
}: PieProps) {
  const { config, data, dataKey, nameKey, isLoading, selectedSector, selectSector } = usePieChart();
  const id = useId().replace(/:/g, ""); // unique id scopes this pie's style defs

  if (isLoading) {
    return (
      <RechartsPie
        data={LOADING_PIE_DATA}
        dataKey="value"
        nameKey="name"
        innerRadius={innerRadius}
        outerRadius={outerRadius}
        cornerRadius={cornerRadius}
        paddingAngle={paddingAngle}
        startAngle={startAngle}
        endAngle={endAngle}
        strokeWidth={0}
        isAnimationActive={false}
        shape={(props) => <AnimatedLoadingSector {...props} />}
      />
    );
  }

  const label = resolveLabel(children, dataKey);

  const preparedData = data.map((item) => ({
    ...item,
    fill: `url(#${id}-colors-${item[nameKey] as string})`,
  }));

  return (
    <>
      <RechartsPie
        data={preparedData}
        dataKey={dataKey}
        nameKey={nameKey}
        innerRadius={innerRadius}
        outerRadius={outerRadius}
        cornerRadius={cornerRadius}
        paddingAngle={paddingAngle}
        startAngle={startAngle}
        endAngle={endAngle}
        strokeWidth={0}
        isAnimationActive
        style={isClickable ? { cursor: "pointer" } : undefined}
        onClick={(_, index) => {
          if (!isClickable) return;
          const clickedName = data[index]?.[nameKey] as string;
          // Clicking the selected sector clears the selection, otherwise selects it
          selectSector(selectedSector === clickedName ? null : clickedName);
        }}
        shape={(props: PieSectorShapeProps) => {
          const sectorName = data[props.index ?? 0]?.[nameKey] as string;
          const isGlowing = glowingSectors.includes(sectorName);
          const isDimmed =
            isClickable && selectedSector !== null && selectedSector !== sectorName;

          return (
            <Sector
              {...props}
              fill={`url(#${id}-colors-${sectorName})`}
              filter={isGlowing ? `url(#${id}-glow-${sectorName})` : undefined}
              stroke={paddingAngle < 0 ? "var(--background)" : "none"}
              strokeWidth={paddingAngle < 0 ? 5 : 0}
              opacity={isDimmed ? 0.3 : 1}
              className="transition-opacity duration-200"
            />
          );
        }}
        {...pieProps}
      >
        {label}
      </RechartsPie>
      <defs>
        <RadialColorGradient id={id} config={config} variant={variant} />
        {glowingSectors.length > 0 && (
          <GlowFilter id={id} glowingSectors={glowingSectors} />
        )}
      </defs>
    </>
  );
}

type LabelProps = {
  dataKey?: string; // data key for the label text — defaults to the pie's value key
  labelListProps?: Omit<LabelListProps, "dataKey">; // escape hatch for raw Recharts LabelList props
};

/**
 * Declares per-sector labels for the <Pie /> it is composed inside. It renders
 * nothing on its own — the parent <Pie /> reads its props and wires them into a
 * Recharts LabelList drawn over the sectors.
 */
export const Label: FC<LabelProps> = () => null;

type TooltipProps = {
  variant?: TooltipVariant; // visual style of the tooltip surface
  roundness?: TooltipRoundness; // border-radius of the tooltip
  defaultIndex?: number; // sector index shown by default with no hover
};

/**
 * The hover tooltip. Hidden automatically while the chart is loading.
 */
export function Tooltip({ variant, roundness, defaultIndex }: TooltipProps) {
  const { isLoading, nameKey } = usePieChart();

  if (isLoading) return null;

  return (
    <ChartTooltip
      defaultIndex={defaultIndex}
      content={
        <ChartTooltipContent
          nameKey={nameKey}
          hideLabel
          roundness={roundness}
          variant={variant}
        />
      }
    />
  );
}

type LegendProps = {
  variant?: ChartLegendVariant; // visual style of the legend indicators
  align?: "left" | "center" | "right"; // horizontal placement
  verticalAlign?: "top" | "middle" | "bottom"; // vertical placement
  isClickable?: boolean; // lets each entry toggle selection of its sector
};

/**
 * The sector legend. When `isClickable` is set, each entry toggles selection of
 * its sector, driving the shared selection state read by the <Pie />.
 */
export function Legend({
  variant,
  align = "center",
  verticalAlign = "bottom",
  isClickable = false,
}: LegendProps) {
  const { nameKey, selectedSector, selectSector } = usePieChart();

  return (
    <ChartLegend
      verticalAlign={verticalAlign}
      align={align}
      content={
        <ChartLegendContent
          selected={selectedSector}
          onSelectChange={selectSector}
          isClickable={isClickable}
          nameKey={nameKey}
          variant={variant}
        />
      }
    />
  );
}

type BackgroundProps = {
  variant?: BackgroundVariant; // background pattern style
};

/**
 * An optional decorative pattern drawn behind the pie. Compose it before the
 * <Pie /> so it sits underneath the sectors.
 */
export function Background({ variant = "dots" }: BackgroundProps) {
  return <ChartBackground variant={variant} />;
}

// ─────────────────────────────────────────────────────────────────────────────
// Label helper
// ─────────────────────────────────────────────────────────────────────────────

// Pulls a <Label /> out of a pie's children into a Recharts LabelList element
const resolveLabel = (children: ReactNode, valueKey: string): ReactNode => {
  let label: ReactNode = null;

  Children.forEach(children, (child) => {
    if (!isValidElement(child) || child.type !== Label) return;

    const { dataKey, labelListProps } = (child as ReactElement<LabelProps>).props;

    label = (
      <RechartsLabelList
        dataKey={dataKey ?? valueKey}
        stroke="none"
        fontSize={12}
        fontWeight={500}
        fill="currentColor"
        className="fill-background"
        {...labelListProps}
      />
    );
  });

  return label;
};

// ─────────────────────────────────────────────────────────────────────────────
// Style definitions — one set per <Pie />, scoped to its unique id
// ─────────────────────────────────────────────────────────────────────────────

type PieVariant = "gradient";

/**
 * Radial-style color gradients, one per sector. Each sector's fill paints from
 * the gradient that matches its name, supporting both single and multi-color
 * config entries.
 */
const RadialColorGradient = ({
  id,
  config,
}: {
  id: string; // unique id of the owning <Pie />
  config: ChartConfig; // sector colors the gradients are built from
  variant: PieVariant; // fill style — currently always a diagonal color gradient
}) => {
  return (
    <>
      {Object.entries(config).map(([sectorKey, sectorConfig]) => {
        const colorsCount = getColorsCount(sectorConfig);

        return (
          <linearGradient
            key={`${id}-colors-${sectorKey}`}
            id={`${id}-colors-${sectorKey}`}
            x1="0"
            y1="0"
            x2="1"
            y2="1"
          >
            {colorsCount === 1 ? (
              <>
                <stop offset="0%" stopColor={`var(--color-${sectorKey}-0)`} />
                <stop offset="100%" stopColor={`var(--color-${sectorKey}-0)`} />
              </>
            ) : (
              Array.from({ length: colorsCount }, (_, index) => {
                const offset = `${(index / (colorsCount - 1)) * 100}%`;
                return (
                  <stop
                    key={offset}
                    offset={offset}
                    stopColor={`var(--color-${sectorKey}-${index}, var(--color-${sectorKey}-0))`}
                  />
                );
              })
            )}
          </linearGradient>
        );
      })}
    </>
  );
};

/** Soft outer-glow SVG filter, one per glowing sector. */
const GlowFilter = ({
  id,
  glowingSectors,
}: {
  id: string; // unique id of the owning <Pie />
  glowingSectors: string[]; // sector names that should glow
}) => {
  return (
    <>
      {glowingSectors.map((sectorName) => (
        <filter
          key={`${id}-glow-${sectorName}`}
          id={`${id}-glow-${sectorName}`}
          x="-100%"
          y="-100%"
          width="300%"
          height="300%"
        >
          <feGaussianBlur in="SourceGraphic" stdDeviation="8" result="blur" />
          <feColorMatrix
            in="blur"
            type="matrix"
            values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 0.5 0"
            result="glow"
          />
          <feMerge>
            <feMergeNode in="glow" />
            <feMergeNode in="SourceGraphic" />
          </feMerge>
        </filter>
      ))}
    </>
  );
};

// ─────────────────────────────────────────────────────────────────────────────
// Loading skeleton
// ─────────────────────────────────────────────────────────────────────────────

// Equal-sized sectors used to render the circular pulsing loading skeleton
const LOADING_PIE_DATA = Array.from({ length: LOADING_SECTORS }, (_, i) => ({
  name: `loading${i}`,
  value: 100 / LOADING_SECTORS,
}));

/**
 * A single skeleton sector shown while the chart is loading. Each sector pulses
 * with a staggered delay, producing a wave that travels around the pie.
 */
const AnimatedLoadingSector = (props: ComponentProps<typeof Sector> & { index?: number }) => {
  const { index = 0, ...sectorProps } = props;

  // Staggered delay so the pulse sweeps around the circle
  const delay = (index / LOADING_SECTORS) * (LOADING_ANIMATION_DURATION / 1000);

  return (
    <motion.g
      initial={{ opacity: 0.15 }}
      animate={{ opacity: [0.15, 0.5, 0.15] }}
      transition={{
        duration: LOADING_ANIMATION_DURATION / 1000,
        delay,
        repeat: Infinity,
        ease: "easeInOut",
      }}
    >
      <Sector {...sectorProps} fill="currentColor" />
    </motion.g>
  );
};
```
        
      
       
        #### Now, Let's add the chart component to your project.

        
          These Components are required by the chart component to render the chart. Make a folder called `ui` inside the `evilcharts` folder and paste the code there.

          Below is main chart component.
        
        
          #### Component Implementation

##### Source File: `chart.tsx`

```tsx
"use client";

import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

// Format: { THEME_NAME: CSS_SELECTOR }
const THEMES = { light: "", dark: ".dark" } as const;

type ThemeKey = keyof typeof THEMES;

// All Keys are optional at first
type ThemeColorsBase = {
  [K in ThemeKey]?: string[];
};

// Require at least one theme key
type AtLeastOneThemeColor = {
  [K in ThemeKey]: Required<Pick<ThemeColorsBase, K>> & Partial<Omit<ThemeColorsBase, K>>;
}[ThemeKey];

const VALID_THEME_KEYS = Object.keys(THEMES) as ThemeKey[];

// Validation for chart config colors at runtime
function validateChartConfigColors(config: ChartConfig): void {
  for (const [key, value] of Object.entries(config)) {
    if (value.colors) {
      const hasValidThemeKey = VALID_THEME_KEYS.some(
        (themeKey) => value.colors?.[themeKey] !== undefined,
      );

      if (!hasValidThemeKey) {
        throw new Error(
          `[EvilCharts] Invalid chart config for "${key}": colors object must have at least one theme key (${VALID_THEME_KEYS.join(", ")}). Received empty object or invalid keys.`,
        );
      }
    }
  }
}

export type ChartConfig = Record<
  string,
  {
    label?: React.ReactNode;
    icon?: React.ComponentType;
    colors?: AtLeastOneThemeColor;
  }
>;

interface ChartContextProps {
  config: ChartConfig;
}

const ChartContext = React.createContext<ChartContextProps | null>(null);

export function useChart() {
  const context = React.useContext(ChartContext);

  if (!context) {
    throw new Error("useChart must be used within a <ChartContainer />");
  }

  return context;
}

interface ChartContainerProps
  extends
    Omit<React.ComponentProps<"div">, "children">,
    Pick<
      React.ComponentProps<typeof RechartsPrimitive.ResponsiveContainer>,
      | "initialDimension"
      | "aspect"
      | "debounce"
      | "minHeight"
      | "minWidth"
      | "maxHeight"
      | "height"
      | "width"
      | "onResize"
      | "children"
    > {
  config: ChartConfig;
  innerResponsiveContainerStyle?: React.ComponentProps<
    typeof RechartsPrimitive.ResponsiveContainer
  >["style"];
  /** Optional content rendered below the chart (e.g. EvilBrush) */
  footer?: React.ReactNode;
}

function ChartContainer({
  id,
  config,
  initialDimension = { width: 320, height: 200 },
  className,
  children,
  footer,
  ...props
}: Readonly<ChartContainerProps>) {
  const uniqueId = React.useId();
  const chartId = `chart-${id ?? uniqueId.replace(/:/g, "")}`;

  // Validate chart config at runtime
  validateChartConfigColors(config);

  return (
    <ChartContext.Provider value={{ config }}>
      <div
        data-slot="chart"
        data-chart={chartId}
        className={cn(
          "min-h-0 w-full flex-1",
          "[&_.recharts-cartesian-axis-tick_text]:fill-muted-foreground [&_.recharts-cartesian-grid_line[stroke='#ccc']]:stroke-border/50 [&_.recharts-curve.recharts-tooltip-cursor]:stroke-border [&_.recharts-polar-grid_[stroke='#ccc']]:stroke-border [&_.recharts-radial-bar-background-sector]:fill-muted [&_.recharts-rectangle.recharts-tooltip-cursor]:fill-muted [&_.recharts-reference-line_[stroke='#ccc']]:stroke-border relative flex flex-col justify-center text-xs [&_.recharts-dot[stroke='#fff']]:stroke-transparent [&_.recharts-layer]:outline-hidden [&_.recharts-sector]:outline-hidden [&_.recharts-sector[stroke='#fff']]:stroke-transparent [&_.recharts-surface]:outline-hidden",
          !footer && "aspect-video",
          className,
        )}
        {...props}
      >
        <ChartStyle id={chartId} config={config} />
        <RechartsPrimitive.ResponsiveContainer
          className="min-h-0 w-full flex-1"
          initialDimension={initialDimension}
        >
          {children}
        </RechartsPrimitive.ResponsiveContainer>
        {footer}
      </div>
    </ChartContext.Provider>
  );
}

function LoadingIndicator({ isLoading }: { isLoading: boolean }) {
  if (!isLoading) {
    return null;
  }

  return (
    <div className="pointer-events-none absolute inset-0 z-20 flex items-center justify-center">
      <div className="text-primary bg-background flex items-center justify-center gap-2 rounded-md border px-2 py-0.5 text-sm">
        <div className="border-border border-t-primary h-3 w-3 animate-spin rounded-full border" />
        <span>Loading</span>
      </div>
    </div>
  );
}

// Distribute colors evenly across slots, extra slots go to last color(s)
// Example: 2 colors for 4 slots → [red, red, pink, pink]
// Example: 3 colors for 4 slots → [red, pink, blue, blue]
function distributeColors(colorsArray: string[], maxCount: number): string[] {
  const availableCount = colorsArray.length;
  if (availableCount >= maxCount) {
    return colorsArray.slice(0, maxCount);
  }

  const result: string[] = [];
  const baseSlots = Math.floor(maxCount / availableCount);
  const extraSlots = maxCount % availableCount;

  // First (availableCount - extraSlots) colors get baseSlots each
  // Last extraSlots colors get (baseSlots + 1) each
  for (let colorIdx = 0; colorIdx < availableCount; colorIdx++) {
    const isExtraColor = colorIdx >= availableCount - extraSlots;
    const slotsForThisColor = baseSlots + (isExtraColor ? 1 : 0);
    for (let j = 0; j < slotsForThisColor; j++) {
      result.push(colorsArray[colorIdx]);
    }
  }

  return result;
}

const ChartStyle = ({ id, config }: { id: string; config: ChartConfig }) => {
  const colorConfig = Object.entries(config).filter(([, config]) => config.colors);

  if (!colorConfig.length) {
    return null;
  }

  const generateCssVars = (theme: keyof typeof THEMES) =>
    colorConfig
      .flatMap(([key, itemConfig]) => {
        const colorsArray = itemConfig.colors?.[theme];
        if (!colorsArray || !Array.isArray(colorsArray) || colorsArray.length === 0) {
          return [];
        }

        // Get max count across all themes for this key
        const maxCount = getColorsCount(itemConfig);

        // Distribute colors evenly across all required slots
        const distributedColors = distributeColors(colorsArray, maxCount);

        return distributedColors.map((color, index) => `  --color-${key}-${index}: ${color};`);
      })
      .filter(Boolean)
      .join("\n");

  const css = Object.entries(THEMES)
    .map(
      ([theme, prefix]) =>
        `${prefix} [data-chart=${id}] {\n${generateCssVars(theme as keyof typeof THEMES)}\n}`,
    )
    .join("\n");

  return <style dangerouslySetInnerHTML={{ __html: css }} />;
};

// Helper to extract item config from a payload.
export function getPayloadConfigFromPayload(config: ChartConfig, payload: unknown, key: string) {
  if (typeof payload !== "object" || payload === null) {
    return undefined;
  }

  const payloadPayload =
    "payload" in payload && typeof payload.payload === "object" && payload.payload !== null
      ? payload.payload
      : undefined;

  let configLabelKey: string = key;

  if (key in payload && typeof payload[key as keyof typeof payload] === "string") {
    configLabelKey = payload[key as keyof typeof payload] as string;
  } else if (
    payloadPayload &&
    key in payloadPayload &&
    typeof payloadPayload[key as keyof typeof payloadPayload] === "string"
  ) {
    configLabelKey = payloadPayload[key as keyof typeof payloadPayload] as string;
  }

  return configLabelKey in config ? config[configLabelKey] : config[key];
}

// Format values to percent for expanded charts
function axisValueToPercentFormatter(value: number) {
  return `${Math.round(value * 100).toFixed(0)}%`;
}

// Get max colors count across all themes for a config entry
function getColorsCount(config: ChartConfig[string]): number {
  if (!config.colors) return 1;
  const counts = VALID_THEME_KEYS.map((theme) => config.colors?.[theme]?.length ?? 0);
  return Math.max(...counts, 1);
}

// Generate random loading data for skeleton/loading state
// min/max represent percentage of the range (0-100), defaults to 20-80 for realistic look
export const getLoadingData = (points: number = 10, min: number = 0, max: number = 70) => {
  const range = max - min;
  return Array.from({ length: points }, () => ({
    loading: Math.floor(Math.random() * range) + min,
  }));
};

export {
  ChartContainer,
  ChartStyle,
  axisValueToPercentFormatter,
  LoadingIndicator,
  getColorsCount,
};
```
        
      
       
        #### Now, We need to add sub components.

        
          Create a file called `tooltip.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `tooltip.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import type { NameType, ValueType } from "recharts/types/component/DefaultTooltipContent";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type TooltipRoundness = "sm" | "md" | "lg" | "xl";
type TooltipVariant = "default" | "frosted-glass";

const roundnessMap: Record<TooltipRoundness, string> = {
  sm: "rounded-sm",
  md: "rounded-md",
  lg: "rounded-lg",
  xl: "rounded-xl",
};

const variantMap: Record<TooltipVariant, string> = {
  default: "bg-background",
  "frosted-glass": "bg-background/70 backdrop-blur-sm",
};

function ChartTooltipContent({
  active,
  payload,
  className,
  indicator = "dot",
  hideLabel = false,
  hideIndicator = false,
  label,
  labelFormatter,
  labelClassName,
  formatter,
  nameKey,
  labelKey,
  selected,
  roundness = "lg",
  variant = "default",
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip> &
  React.ComponentProps<"div"> & {
    hideLabel?: boolean;
    hideIndicator?: boolean;
    indicator?: "line" | "dot" | "dashed";
    nameKey?: string;
    labelKey?: string;
    selected?: string | null;
    roundness?: TooltipRoundness;
    variant?: TooltipVariant;
  } & Omit<
    RechartsPrimitive.DefaultTooltipContentProps<ValueType, NameType>,
    "accessibilityLayer"
  >) {
  const { config } = useChart();

  const tooltipLabel = React.useMemo(() => {
    if (hideLabel || !payload?.length) {
      return null;
    }

    const [item] = payload;
    const key = `${labelKey ?? item?.dataKey ?? item?.name ?? "value"}`;
    const itemConfig = getPayloadConfigFromPayload(config, item, key);
    const value =
      !labelKey && typeof label === "string" ? (config[label]?.label ?? label) : itemConfig?.label;

    if (labelFormatter) {
      return (
        <div className={cn("font-medium", labelClassName)}>{labelFormatter(value, payload)}</div>
      );
    }

    if (!value) {
      return null;
    }

    return <div className={cn("font-medium", labelClassName)}>{value}</div>;
  }, [label, labelFormatter, payload, hideLabel, labelClassName, config, labelKey]);

  if (!active || !payload?.length) {
    // Empty tooltip - to prevent position getting 0.0 so it doesnt animate tooltip every time from 0.0 origin
    return <span className="p-4" />;
  }

  const nestLabel = payload.length === 1 && indicator !== "dot";

  return (
    <div
      className={cn(
        "border-border/50 grid min-w-32 items-start gap-1.5 border px-2.5 py-1.5 text-xs shadow-xl",
        roundnessMap[roundness],
        variantMap[variant],
        className,
      )}
    >
      {!nestLabel ? tooltipLabel : null}
      <div className="grid gap-1.5">
        {payload
          .filter((item) => item.type !== "none")
          .map((item, index) => {
            // For pie charts, item.name contains the sector name (e.g., "chrome")
            // For radial charts, the name is in item.payload[nameKey]
            // For other charts, item.name or item.dataKey contains the series name
            const payloadName =
              nameKey && item.payload
                ? (item.payload as Record<string, unknown>)[nameKey]
                : undefined;
            const key = `${payloadName ?? item.name ?? item.dataKey ?? "value"}`;
            const itemConfig = getPayloadConfigFromPayload(config, item, key);

            // Get colors count for this item to determine gradient vs solid
            const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

            return (
              <div
                key={index}
                className={cn(
                  "[&>svg]:text-muted-foreground flex w-full flex-wrap items-stretch gap-2 [&>svg]:h-2.5 [&>svg]:w-2.5",
                  indicator === "dot" && "items-center",
                  selected != null && selected !== item.dataKey && "opacity-30",
                )}
              >
                {formatter && item?.value !== undefined && item.name ? (
                  formatter(item.value, item.name, item, index, item.payload)
                ) : (
                  <>
                    {itemConfig?.icon ? (
                      <itemConfig.icon />
                    ) : (
                      !hideIndicator && (
                        <div
                          className={cn("shrink-0 rounded-[2px]", {
                            "h-2.5 w-2.5": indicator === "dot",
                            "w-1": indicator === "line",
                            "w-0 border-[1.5px] border-dashed bg-transparent!":
                              indicator === "dashed",
                            "my-0.5": nestLabel && indicator === "dashed",
                          })}
                          style={getIndicatorColorStyle(key, colorsCount)}
                        />
                      )
                    )}
                    <div
                      className={cn(
                        "flex flex-1 justify-between gap-4 leading-none",
                        nestLabel ? "items-end" : "items-center",
                      )}
                    >
                      <div className="grid gap-1.5">
                        {nestLabel ? tooltipLabel : null}
                        <span className="text-muted-foreground">
                          {itemConfig?.label ?? item.name}
                        </span>
                      </div>
                      {item.value != null && (
                        <span className="text-foreground font-mono font-medium tabular-nums">
                          {typeof item.value === "number"
                            ? item.value.toLocaleString()
                            : String(item.value)}
                        </span>
                      )}
                    </div>
                  </>
                )}
              </div>
            );
          })}
      </div>
    </div>
  );
}

function getIndicatorColorStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { background: `var(--color-${dataKey}-0)` };
  }

  // Multiple colors: create linear gradient with evenly distributed stops
  const stops = Array.from({ length: colorsCount }, (_, index) => {
    const offset = (index / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${index}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

const ChartTooltip = ({
  animationDuration = 200,
  ...props
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip>) => (
  <RechartsPrimitive.Tooltip animationDuration={animationDuration} {...props} />
);

export { ChartTooltip, ChartTooltipContent };
export type { TooltipRoundness, TooltipVariant };
```
        
        
          Now, create another file called `legend.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `legend.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type ChartLegendVariant =
  | "square"
  | "circle"
  | "circle-outline"
  | "rounded-square"
  | "rounded-square-outline"
  | "vertical-bar"
  | "horizontal-bar";

function ChartLegendContent({
  className,
  hideIcon = false,
  nameKey,
  payload,
  verticalAlign,
  align = "right",
  selected,
  onSelectChange,
  isClickable,
  variant = "rounded-square",
}: React.ComponentProps<"div"> & {
  hideIcon?: boolean;
  nameKey?: string;
  selected?: string | null;
  isClickable?: boolean;
  onSelectChange?: (selected: string | null) => void;
  variant?: ChartLegendVariant;
} & RechartsPrimitive.DefaultLegendContentProps) {
  const { config } = useChart();

  if (!payload?.length) {
    return null;
  }

  return (
    <div
      className={cn(
        "flex items-center gap-4 select-none",
        align === "left" && "justify-start",
        align === "center" && "justify-center",
        align === "right" && "justify-end",
        verticalAlign === "top" ? "pb-4" : "pt-4",
        className,
      )}
    >
      {payload
        .filter((item) => item.type !== "none")
        .map((item) => {
          // For pie charts, item.value contains the sector name (e.g., "chrome")
          // For radial charts, the name is in item.payload[nameKey]
          // For other charts, item.dataKey contains the series name (e.g., "desktop")
          const payloadName =
            nameKey && item.payload
              ? (item.payload as Record<string, unknown>)[nameKey]
              : undefined;
          const key = `${payloadName ?? item.value ?? item.dataKey ?? "value"}`;
          const itemConfig = getPayloadConfigFromPayload(config, item, key);
          const isSelected = selected === null || selected === key;

          // Get colors count for this item to determine gradient vs solid
          const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

          return (
            <div
              key={key}
              className={cn(
                "[&>svg]:text-muted-foreground flex items-center gap-1.5 transition-opacity [&>svg]:h-3 [&>svg]:w-3",
                !isSelected && "opacity-30",
                isClickable && "cursor-pointer",
              )}
              onClick={() => {
                if (!isClickable) return;

                onSelectChange?.(selected === key ? null : key);
              }}
            >
              {itemConfig?.icon && !hideIcon ? (
                <itemConfig.icon />
              ) : (
                <LegendIndicator
                  variant={variant}
                  dataKey={key}
                  colorsCount={colorsCount}
                />
              )}
              {itemConfig?.label}
            </div>
          );
        })}
    </div>
  );
}

// ---------------------------------------------------------------------------
// Legend indicator — each variant gets its own branch so future variants
// can diverge freely in markup & style.
// ---------------------------------------------------------------------------

function LegendIndicator({
  variant,
  dataKey,
  colorsCount,
}: {
  variant: ChartLegendVariant;
  dataKey: string;
  colorsCount: number;
}) {
  const fillStyle = getLegendFillStyle(dataKey, colorsCount);
  const outlineStyle = getLegendOutlineStyle(dataKey, colorsCount);

  switch (variant) {
    case "square":
      return <div className="h-2 w-2 shrink-0" style={fillStyle} />;

    case "circle":
      return <div className="h-2 w-2 shrink-0 rounded-full" style={fillStyle} />;

    case "circle-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-full p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "vertical-bar":
      return <div className="h-3 w-1 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "horizontal-bar":
      return <div className="h-1 w-3 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "rounded-square-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-[3px] p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "rounded-square":
    default:
      return <div className="h-2 w-2 shrink-0 rounded-[2px]" style={fillStyle} />;
  }
}

// ---------------------------------------------------------------------------
// Style helpers
// ---------------------------------------------------------------------------

/** Solid fill / gradient background for filled variants. */
function getLegendFillStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { backgroundColor: `var(--color-${dataKey}-0)` };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

/**
 * Outline style for stroke variants.
 * Uses background + mask-composite to punch out the center, leaving only the
 * "border" visible. Works with both solid colors and gradients, and respects
 * border-radius — unlike plain `border-color`.
 */
function getLegendOutlineStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  const maskStyle: React.CSSProperties = {
    WebkitMask:
      "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    WebkitMaskComposite: "xor",
    mask: "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    maskComposite: "exclude",
  };

  if (colorsCount <= 1) {
    return {
      backgroundColor: `var(--color-${dataKey}-0)`,
      ...maskStyle,
    };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return {
    background: `linear-gradient(to right, ${stops})`,
    ...maskStyle,
  };
}

const ChartLegend = RechartsPrimitive.Legend;

export { ChartLegend, ChartLegendContent, type ChartLegendVariant };
```
        
      
    
  


## Usage

The pie chart is composible. `<EvilPieChart>` is the container, and you compose only the parts you need — `<Legend>`, `<Tooltip>`, `<Background>`, and one `<Pie>` — as its children. The `<Pie>` carries its own shape props (`innerRadius`, `paddingAngle`, `cornerRadius`, …), its `isClickable` flag, and `glowingSectors`, so a single chart can mix any combination of those.

```tsx
import {
  EvilPieChart,
  Pie,
  Label,
  Tooltip,
  Legend,
  Background,
} from "@/components/evilcharts/charts/pie-chart";
```

```tsx
const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: { light: ["#3b82f6"], dark: ["#60a5fa"] },
  },
  safari: {
    label: "Safari",
    colors: { light: ["#10b981"], dark: ["#34d399"] },
  },
  firefox: {
    label: "Firefox",
    colors: { light: ["#f59e0b"], dark: ["#fbbf24"] },
  },
} satisfies ChartConfig;
```

```tsx
<EvilPieChart data={data} dataKey="visitors" nameKey="browser" config={chartConfig}>
  <Legend isClickable />
  <Tooltip />
  <Pie isClickable innerRadius={60} paddingAngle={4} cornerRadius={8}>
    <Label />
  </Pie>
</EvilPieChart>
```

### Interactive Selection

Add `isClickable` to the `<Pie>` (and to `<Legend>`) to make sectors selectable. Use the `onSelectionChange` callback on `<EvilPieChart>` to handle selection events:

```tsx
<EvilPieChart
  data={data}
  dataKey="visitors"
  nameKey="browser"
  config={chartConfig}
  onSelectionChange={(selection) => {
    if (selection) {
      console.log("Selected:", selection.dataKey, "Value:", selection.value);
    } else {
      console.log("Deselected");
    }
  }}
>
  <Legend isClickable />
  <Tooltip />
  <Pie isClickable />
</EvilPieChart>
```

### Loading State

#### Live Example Code

##### Example File: `ex-loading-state-pie-chart.tsx`

```tsx
"use client";

import { EvilPieChart, Pie, Tooltip, Legend } from "@/registry/charts/pie-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
  { browser: "edge", visitors: 173 },
  { browser: "other", visitors: 90 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  safari: {
    label: "Safari",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
  firefox: {
    label: "Firefox",
    colors: {
      light: ["#f59e0b"],
      dark: ["#fbbf24"],
    },
  },
  edge: {
    label: "Edge",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  other: {
    label: "Other",
    colors: {
      light: ["#6b7280"],
      dark: ["#9ca3af"],
    },
  },
} satisfies ChartConfig;

export function EvilExamplePieChart() {
  return (
    <EvilPieChart
      className="h-full w-full p-4"
      data={data}
      dataKey="visitors"
      nameKey="browser"
      config={chartConfig}
      isLoading // [!code highlight]
    >
      <Legend isClickable />
      <Tooltip />
      <Pie isClickable />
    </EvilPieChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    The pie chart supports loading state with a placeholder animation. You can pass the `isLoading` prop to the chart to show the loading state while your data is being fetched.
  


## Examples

Below are some examples of the pie chart with different configurations. You can customize the `innerRadius`, `paddingAngle`, `cornerRadius`, and other properties.

### Gradient Colors

#### Live Example Code

##### Example File: `ex-gradient-colors-pie-chart.tsx`

```tsx
"use client";

import { EvilPieChart, Pie, Tooltip, Legend } from "@/registry/charts/pie-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
  { browser: "edge", visitors: 173 },
  { browser: "other", visitors: 90 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: {
      light: ["#93c5fd", "#3b82f6", "#2563eb", "#1d4ed8", "#1e40af"], // [!code highlight]
      dark: ["#bfdbfe", "#60a5fa", "#3b82f6", "#2563eb", "#1d4ed8"], // [!code highlight]
    },
  },
  safari: {
    label: "Safari",
    colors: {
      light: ["#6ee7b7", "#10b981", "#059669", "#047857", "#065f46"], // [!code highlight]
      dark: ["#a7f3d0", "#34d399", "#10b981", "#059669", "#047857"], // [!code highlight]
    },
  },
  firefox: {
    label: "Firefox",
    colors: {
      light: ["#fcd34d", "#f59e0b", "#d97706", "#b45309", "#92400e"], // [!code highlight]
      dark: ["#fde68a", "#fbbf24", "#f59e0b", "#d97706", "#b45309"], // [!code highlight]
    },
  },
  edge: {
    label: "Edge",
    colors: {
      light: ["#c4b5fd", "#8b5cf6", "#7c3aed", "#6d28d9", "#5b21b6"], // [!code highlight]
      dark: ["#ddd6fe", "#a78bfa", "#8b5cf6", "#7c3aed", "#6d28d9"], // [!code highlight]
    },
  },
  other: {
    label: "Other",
    colors: {
      light: ["#d1d5db", "#9ca3af", "#6b7280", "#4b5563", "#374151"], // [!code highlight]
      dark: ["#e5e7eb", "#d1d5db", "#9ca3af", "#6b7280", "#4b5563"], // [!code highlight]
    },
  },
} satisfies ChartConfig;

export function EvilExamplePieChart() {
  return (
    <EvilPieChart
      className="h-full w-full p-4"
      data={data}
      dataKey="visitors"
      nameKey="browser"
      config={chartConfig}
    >
      <Legend isClickable />
      <Tooltip />
      <Pie isClickable />
    </EvilPieChart>
  );
}
```

### Donut Chart

#### Live Example Code

##### Example File: `ex-donut-pie-chart.tsx`

```tsx
"use client";

import { EvilPieChart, Pie, Tooltip, Legend } from "@/registry/charts/pie-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
  { browser: "edge", visitors: 173 },
  { browser: "other", visitors: 90 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  safari: {
    label: "Safari",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
  firefox: {
    label: "Firefox",
    colors: {
      light: ["#f59e0b"],
      dark: ["#fbbf24"],
    },
  },
  edge: {
    label: "Edge",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  other: {
    label: "Other",
    colors: {
      light: ["#6b7280"],
      dark: ["#9ca3af"],
    },
  },
} satisfies ChartConfig;

export function EvilExamplePieChart() {
  return (
    <EvilPieChart
      className="h-full w-full p-4"
      data={data}
      dataKey="visitors"
      nameKey="browser"
      config={chartConfig}
    >
      <Legend isClickable />
      <Tooltip />
      <Pie
        isClickable
        innerRadius={60} // [!code highlight]
      />
    </EvilPieChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    Set `innerRadius` to a value greater than 0 to create a donut chart. The inner radius creates the hole in the center of the pie.
  


### Padded Sectors

#### Live Example Code

##### Example File: `ex-padded-pie-chart.tsx`

```tsx
"use client";

import { EvilPieChart, Pie, Tooltip, Legend } from "@/registry/charts/pie-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
  { browser: "edge", visitors: 173 },
  { browser: "other", visitors: 90 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  safari: {
    label: "Safari",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
  firefox: {
    label: "Firefox",
    colors: {
      light: ["#f59e0b"],
      dark: ["#fbbf24"],
    },
  },
  edge: {
    label: "Edge",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  other: {
    label: "Other",
    colors: {
      light: ["#6b7280"],
      dark: ["#9ca3af"],
    },
  },
} satisfies ChartConfig;

export function EvilExamplePieChart() {
  return (
    <EvilPieChart
      className="h-full w-full p-4"
      data={data}
      dataKey="visitors"
      nameKey="browser"
      config={chartConfig}
    >
      <Legend isClickable />
      <Tooltip />
      <Pie
        isClickable
        innerRadius={30} // [!code highlight]
        paddingAngle={4} // [!code highlight]
        cornerRadius={8} // [!code highlight]
      />
    </EvilPieChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    Use `paddingAngle` to add space between sectors and `cornerRadius` to round the corners of each sector. Combine with `innerRadius` for a modern donut look.
  


#### Live Example Code

##### Example File: `ex-overlapping-padded-pie-chart.tsx`

```tsx
"use client";

import { EvilPieChart, Pie, Tooltip, Legend } from "@/registry/charts/pie-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
  { browser: "edge", visitors: 173 },
  { browser: "other", visitors: 90 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  safari: {
    label: "Safari",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
  firefox: {
    label: "Firefox",
    colors: {
      light: ["#f59e0b"],
      dark: ["#fbbf24"],
    },
  },
  edge: {
    label: "Edge",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  other: {
    label: "Other",
    colors: {
      light: ["#6b7280"],
      dark: ["#9ca3af"],
    },
  },
} satisfies ChartConfig;

export function EvilExamplePieChart() {
  return (
    <EvilPieChart
      className="h-full w-full p-4"
      data={data}
      dataKey="visitors"
      nameKey="browser"
      config={chartConfig}
    >
      <Legend isClickable />
      <Tooltip />
      <Pie innerRadius={60} paddingAngle={-25} cornerRadius={99} />
    </EvilPieChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    Use a negative `paddingAngle` to create overlapping sectors with a high `cornerRadius` for a petal-like effect. Combine with `innerRadius` for a flower-shaped donut chart.
  


### Labels

#### Live Example Code

##### Example File: `ex-labels-pie-chart.tsx`

```tsx
"use client";

import { EvilPieChart, Pie, Label, Tooltip, Legend } from "@/registry/charts/pie-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
  { browser: "edge", visitors: 173 },
  { browser: "other", visitors: 90 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  safari: {
    label: "Safari",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
  firefox: {
    label: "Firefox",
    colors: {
      light: ["#f59e0b"],
      dark: ["#fbbf24"],
    },
  },
  edge: {
    label: "Edge",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  other: {
    label: "Other",
    colors: {
      light: ["#6b7280"],
      dark: ["#9ca3af"],
    },
  },
} satisfies ChartConfig;

export function EvilExamplePieChart() {
  return (
    <EvilPieChart
      className="h-full w-full p-4"
      data={data}
      dataKey="visitors"
      nameKey="browser"
      config={chartConfig}
    >
      <Legend isClickable />
      <Tooltip />
      <Pie isClickable innerRadius={30} paddingAngle={4} cornerRadius={8}>
        <Label // [!code highlight]
        />
      </Pie>
    </EvilPieChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    Enable `showLabels` to display labels on each sector. You can customize the label with `labelKey` to show different data, and use `labelListProps` for additional customization.
  


### Glowing Sectors

#### Live Example Code

##### Example File: `ex-glowing-pie-chart.tsx`

```tsx
"use client";

import { EvilPieChart, Pie, Tooltip, Legend } from "@/registry/charts/pie-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
  { browser: "edge", visitors: 173 },
  { browser: "other", visitors: 90 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  safari: {
    label: "Safari",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
  firefox: {
    label: "Firefox",
    colors: {
      light: ["#f59e0b"],
      dark: ["#fbbf24"],
    },
  },
  edge: {
    label: "Edge",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  other: {
    label: "Other",
    colors: {
      light: ["#6b7280"],
      dark: ["#9ca3af"],
    },
  },
} satisfies ChartConfig;

export function EvilExamplePieChart() {
  return (
    <EvilPieChart
      className="h-full w-full p-4"
      data={data}
      dataKey="visitors"
      nameKey="browser"
      config={chartConfig}
    >
      <Legend isClickable />
      <Tooltip />
      <Pie
        isClickable
        glowingSectors={["chrome", "safari"]} // [!code highlight]
      />
    </EvilPieChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    Add a subtle glow effect to specific sectors using `glowingSectors` prop. Pass an array of sector names (the values from your `nameKey` field) to specify which sectors should glow.
  


## API Reference

The chart is composed of several parts. The props below are grouped by the component they belong to.

### EvilPieChart


The root container. It owns the data, the shared selection state, and the loading skeleton. Everything visual is composed as its children.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **data*** | `TData[]` | - | Data used to display the chart. An array of objects where each object represents a sector (`TData extends Record`). |
| **dataKey*** | `keyof TData & string` | - | The key from your data objects to use for sector values (typically numeric values that determine sector size). |
| **nameKey*** | `keyof TData & string` | - | The key from your data objects to use for sector names (typically string values used for labels and legend). |
| **config*** | `ChartConfig` | - | Configuration object that defines the chart's sectors. Each key should match a value from your `nameKey` field, with corresponding colors. |
| **children*** | `ReactNode` | - | The composed chart parts — ``, ``, ``, and one ``. |
| **className** | `string` | - | Additional CSS classes to apply to the chart container. |
| **defaultSelectedSector** | `string | null` | `null` | The sector name that should be selected by default. |
| **onSelectionChange** | - | - | void">     Callback fired when a sector is selected or deselected — by clicking a clickable `` sector or `` entry. Receives an object with `dataKey` (sector name) and `value` (sector value), or `null` when deselected. |
| **isLoading** | `boolean` | `false` | Shows a loading placeholder animation when data is being fetched. |
| **chartProps** | - | - | ">     Additional props forwarded to the underlying Recharts PieChart component. Read the Recharts PieChart documentation for available props. |



### Pie


The pie series. Self-contained — it generates its own radial color gradients and glow filters, so any number of pies can live on one page without style collisions. Compose a `<Label />` inside it to draw labels on each sector.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **innerRadius** | `number | string` | `0` | The inner radius of the pie. Set to a value greater than 0 to create a donut chart. Can be a number (pixels) or percentage string. |
| **outerRadius** | `number | string` | - | The outer radius of the pie. Can be a number (pixels) or percentage string. |
| **cornerRadius** | `number` | `0` | The border radius for the corners of each sector in pixels. |
| **paddingAngle** | `number` | `0` | The angle of padding between sectors in degrees. Use a negative value to create overlapping sectors. |
| **startAngle** | `number` | `0` | The starting angle of the pie in degrees (0 is 3 o'clock, 90 is 12 o'clock). |
| **endAngle** | `number` | `360` | The ending angle of the pie in degrees. Set to less than 360 for a partial pie. |
| **isClickable** | `boolean` | `false` | Enables interactive clicking on sectors to select/deselect them. When a sector is selected, the others dim. |
| **glowingSectors** | `string[]` | `[]` | Array of sector names (values from your `nameKey` field) that should have a glowing effect applied. Creates a smooth outer glow around the specified sectors. |
| **children** | `ReactNode` | - | Optional `` composition that draws labels on each sector. |
| **pieProps** | - | - | , "data" | "dataKey" | "nameKey">'>     Escape hatch for raw props forwarded to the underlying Recharts Pie component. Read the Recharts Pie documentation for available props. |



### Label


Per-sector labels composed inside a `<Pie />`. It renders nothing on its own — the parent `<Pie />` reads its props and draws a label list over the sectors.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **dataKey** | `string` | - | The key from your data objects to use for label text. Falls back to the chart's `dataKey` when omitted. |
| **labelListProps** | - | - | , "dataKey">'>     Escape hatch for raw props forwarded to the underlying Recharts LabelList component. Read the Recharts LabelList documentation for available props. |



### Tooltip


The hover tooltip. Hidden automatically while the chart is loading.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style of the tooltip surface. |
| **roundness** | - | - | Controls the border-radius of the tooltip. |
| **defaultIndex** | `number` | - | When set, the tooltip is visible by default at the specified sector index. |



### Legend


The sector legend. When `isClickable` is set, each entry toggles selection of its sector.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style of the legend indicators. |
| **align** | - | - | Horizontal placement of the legend. |
| **verticalAlign** | - | - | Vertical placement of the legend. |
| **isClickable** | `boolean` | `false` | Lets each legend entry toggle selection of its sector. |



### Background


An optional decorative pattern drawn behind the pie. Compose it before the `<Pie />` so it sits underneath the sectors.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | `BackgroundVariant` | - | The background pattern style. |

---

## Radar Chart <a name="radar-chart"></a>

> Beautiful radar charts with filled and lines variants, gradient colors, and glow effects

#### Live Example Code

##### Example File: `ex-radar-chart.tsx`

```tsx
"use client";

import {
  EvilRadarChart,
  Radar,
  PolarGrid,
  PolarAngleAxis,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/registry/charts/radar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { skill: "JavaScript", desktop: 186, mobile: 80 },
  { skill: "TypeScript", desktop: 305, mobile: 200 },
  { skill: "React", desktop: 237, mobile: 120 },
  { skill: "Node.js", desktop: 173, mobile: 190 },
  { skill: "CSS", desktop: 209, mobile: 130 },
  { skill: "Python", desktop: 214, mobile: 140 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleRadarChart() {
  return (
    <EvilRadarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <PolarGrid />
      <PolarAngleAxis dataKey="skill" />
      <Legend isClickable />
      <Tooltip />
      <Radar
        dataKey="desktop"
        variant="filled" // [!code highlight]
        isClickable
      >
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Radar>
      <Radar dataKey="mobile" variant="filled" isClickable>
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Radar>
    </EvilRadarChart>
  );
}
```

## Installation


  
    
    
  
  
### CLI Installation


    ```bash
npx shadcn@latest add @evilcharts/radar-chart
```
  
  
### Manual Installation


    
      
        #### Install the following dependencies:

        
          ```bash
npm install recharts
```
        
      
      
        #### Copy and paste the following code snippets into your project.

         
          To use the chart, first create the folder `evilcharts` and a subfolder called `charts` inside your `components` directory.
          Then, copy the following base radar-chart code into a new file in that folder.
        
        
          #### Component Implementation

##### Source File: `radar-chart.tsx`

```tsx
"use client";

import {
  type ChartConfig,
  ChartContainer,
  getColorsCount,
  LoadingIndicator,
} from "@/registry/ui/chart";
import {
  ChartTooltip,
  ChartTooltipContent,
  type TooltipRoundness,
  type TooltipVariant,
} from "@/registry/ui/tooltip";
import { ChartLegend, ChartLegendContent, type ChartLegendVariant } from "@/registry/ui/legend";
import { ChartBackground, type BackgroundVariant } from "@/registry/ui/background";
import { ChartDot, type DotVariant } from "@/registry/ui/dot";
import {
  Children,
  createContext,
  isValidElement,
  use,
  useCallback,
  useEffect,
  useId,
  useMemo,
  useState,
  type ComponentProps,
  type FC,
  type ReactElement,
  type ReactNode,
} from "react";
import {
  PolarAngleAxis as RechartsPolarAngleAxis,
  PolarGrid as RechartsPolarGrid,
  PolarRadiusAxis as RechartsPolarRadiusAxis,
  Radar as RechartsRadar,
  RadarChart as RechartsRadarChart,
} from "recharts";

// Constants
const STROKE_WIDTH = 1;
const DEFAULT_FILL_OPACITY = 0.3;
const LOADING_POINTS = 6;
const LOADING_ANIMATION_DURATION = 1500; // in milliseconds
const LOADING_RADAR_DATA_KEY = "value";

type RadarVariant = "filled" | "lines";

// ─────────────────────────────────────────────────────────────────────────────
// Shared context
// ─────────────────────────────────────────────────────────────────────────────

/**
 * Shared state for every part of the chart. Lifted into <EvilRadarChart /> so that
 * <Radar />, <PolarAngleAxis />, <Legend />, and friends can read it without prop
 * drilling. Sub-components are composed freely — the provider is the single source
 * of truth.
 */
type RadarChartContextValue = {
  config: ChartConfig; // colors + labels for every series
  isLoading: boolean; // whether the chart shows its loading skeleton
  selectedDataKey: string | null; // currently selected series, or null when none
  selectDataKey: (dataKey: string | null) => void; // sets the selected series
};

const RadarChartContext = createContext<RadarChartContextValue | null>(null);

// Reads the chart context, throwing a helpful error when used outside <EvilRadarChart />
function useRadarChart() {
  const context = use(RadarChartContext);

  if (!context) {
    throw new Error(
      "Radar chart parts (<Radar />, <PolarAngleAxis />, …) must be used within <EvilRadarChart />",
    );
  }

  return context;
}

// ─────────────────────────────────────────────────────────────────────────────
// Root container
// ─────────────────────────────────────────────────────────────────────────────

// Validates that every config key also exists on the data row type
type ValidateConfigKeys<TData, TConfig> = {
  [K in keyof TConfig]: K extends keyof TData ? ChartConfig[string] : never;
};

type EvilRadarChartBaseProps<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
> = {
  config: TConfig & ValidateConfigKeys<TData, TConfig>; // series colors + labels
  data: TData[]; // rows rendered by the chart
  children: ReactNode; // composed parts — <Radar />, <PolarGrid />, <Legend />, …
  className?: string; // extra classes for the chart container
  chartProps?: ComponentProps<typeof RechartsRadarChart>; // escape hatch for the raw Recharts chart
  backgroundVariant?: BackgroundVariant; // background pattern drawn behind the chart
  defaultSelectedDataKey?: string | null; // series selected on first render
  onSelectionChange?: (selectedDataKey: string | null) => void; // fires when the selected series changes
  isLoading?: boolean; // shows the animated loading skeleton
  loadingPoints?: number; // number of points in the loading skeleton
};

type EvilRadarChartProps<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
> = EvilRadarChartBaseProps<TData, TConfig>;

/**
 * Root of the composible radar chart. Owns the data, the shared context, and the
 * loading skeleton. Everything visual — the polar grid, axes, tooltip, legend, and
 * the radars themselves — is composed as children, so a consumer renders exactly
 * the parts they need.
 */
export function EvilRadarChart<
  TData extends Record<string, unknown>,
  TConfig extends Record<string, ChartConfig[string]>,
>({
  config,
  data,
  children,
  className,
  chartProps,
  backgroundVariant,
  defaultSelectedDataKey = null,
  onSelectionChange,
  isLoading = false,
  loadingPoints,
}: EvilRadarChartProps<TData, TConfig>) {
  const chartId = useId().replace(/:/g, ""); // colon-free id keeps CSS/SVG selectors valid
  const [selectedDataKey, setSelectedDataKey] = useState<string | null>(defaultSelectedDataKey);
  const loadingData = useLoadingData(isLoading, loadingPoints);

  // Updates selection state and notifies the parent
  const selectDataKey = useCallback(
    (newSelectedDataKey: string | null) => {
      setSelectedDataKey(newSelectedDataKey);
      onSelectionChange?.(newSelectedDataKey);
    },
    [onSelectionChange],
  );

  const contextValue = useMemo<RadarChartContextValue>(
    () => ({
      config,
      isLoading,
      selectedDataKey,
      selectDataKey,
    }),
    [config, isLoading, selectedDataKey, selectDataKey],
  );

  return (
    <RadarChartContext value={contextValue}>
      <ChartContainer className={className} config={config}>
        <LoadingIndicator isLoading={isLoading} />
        <RechartsRadarChart id={chartId} data={isLoading ? loadingData : data} {...chartProps}>
          {backgroundVariant && <ChartBackground variant={backgroundVariant} />}
          {children}
          {isLoading && <LoadingRadar />}
        </RechartsRadarChart>
      </ChartContainer>
    </RadarChartContext>
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Composible parts
// ─────────────────────────────────────────────────────────────────────────────

type RadarProps = {
  dataKey: string; // series key — must exist on the data and config
  variant?: RadarVariant; // fill style for this radar only
  fillOpacity?: number; // opacity of the filled area when `variant="filled"`
  isGlowing?: boolean; // adds a soft outer glow around this radar
  isClickable?: boolean; // lets this radar be selected by clicking it
  children?: ReactNode; // optional <Dot /> and <ActiveDot /> composition
  radarProps?: Omit<ComponentProps<typeof RechartsRadar>, "dataKey">; // escape hatch for raw Recharts Radar props
};

/**
 * A single radar series. Each <Radar /> is fully self-contained: it generates its
 * own stroke/fill gradients and glow filter under a unique id, so any number of
 * radars — each with its own variant, opacity, and clickability — can live in one
 * chart without style collisions. Compose <Dot /> and <ActiveDot /> inside it to
 * add point markers.
 */
export function Radar({
  dataKey,
  variant = "filled",
  fillOpacity = DEFAULT_FILL_OPACITY,
  isGlowing = false,
  isClickable = false,
  children,
  radarProps,
}: RadarProps) {
  const { config, isLoading, selectedDataKey, selectDataKey } = useRadarChart();
  const id = useId().replace(/:/g, ""); // unique id scopes this radar's style defs

  // The root renders the skeleton radar while loading, so real radars step aside
  if (isLoading) return null;

  const isSelected = selectedDataKey === null || selectedDataKey === dataKey;
  const opacity = isClickable && !isSelected ? 0.2 : 1;
  const isFilled = variant === "filled";

  const { dot, activeDot } = resolveDots(children, id, dataKey, opacity);

  return (
    <>
      <RechartsRadar
        dataKey={dataKey}
        stroke={`url(#${id}-radar-stroke-${dataKey})`}
        strokeOpacity={opacity}
        strokeWidth={STROKE_WIDTH}
        fill={isFilled ? `url(#${id}-radar-fill-${dataKey})` : "none"}
        fillOpacity={isFilled ? fillOpacity * opacity : 0}
        dot={dot}
        activeDot={activeDot}
        filter={isGlowing ? `url(#${id}-radar-glow-${dataKey})` : undefined}
        className="transition-opacity duration-200"
        style={isClickable ? { cursor: "pointer" } : undefined}
        onClick={() => {
          if (!isClickable) return;
          // Clicking the selected radar clears the selection, otherwise selects it
          selectDataKey(selectedDataKey === dataKey ? null : dataKey);
        }}
        {...radarProps}
      />
      <defs>
        <ColorGradient id={id} dataKey={dataKey} config={config} />
        <StrokeGradient id={id} dataKey={dataKey} config={config} />
        {isFilled && <FillGradient id={id} dataKey={dataKey} config={config} />}
        {isGlowing && <GlowFilter id={id} dataKey={dataKey} />}
      </defs>
    </>
  );
}

type DotProps = {
  variant?: DotVariant; // visual style of the point marker
};

/**
 * Declares a resting point marker for the <Radar /> it is composed inside.
 * It renders nothing on its own — the parent <Radar /> reads its variant and
 * wires it into the Recharts dot slot.
 */
export const Dot: FC<DotProps> = () => null;

/**
 * Declares the hovered/active point marker for the <Radar /> it is composed
 * inside. Like <Dot />, it is a configuration slot and renders nothing itself.
 */
export const ActiveDot: FC<DotProps> = () => null;

type PolarGridProps = ComponentProps<typeof RechartsPolarGrid>;

/**
 * The polar grid lines. Defaults to a dashed polygon grid and forwards every
 * Recharts PolarGrid prop, so `gridType`, `polarRadius`, etc. pass straight through.
 */
export function PolarGrid({
  gridType = "polygon",
  stroke = "currentColor",
  strokeOpacity = 0.2,
  strokeDasharray = "3 4",
  ...props
}: PolarGridProps) {
  return (
    <RechartsPolarGrid
      gridType={gridType}
      stroke={stroke}
      strokeOpacity={strokeOpacity}
      strokeDasharray={strokeDasharray}
      {...props}
    />
  );
}

type PolarAngleAxisProps = ComponentProps<typeof RechartsPolarAngleAxis>;

/**
 * The angular category axis — the labels around the chart's perimeter. Ships
 * with the chart's flat default styling and forwards every Recharts
 * PolarAngleAxis prop. Hidden automatically while the chart is loading.
 */
export function PolarAngleAxis({
  tick = { fill: "currentColor", fontSize: 12 },
  tickLine = false,
  ...props
}: PolarAngleAxisProps) {
  const { isLoading } = useRadarChart();

  if (isLoading) return null;

  return <RechartsPolarAngleAxis tick={tick} tickLine={tickLine} {...props} />;
}

type PolarRadiusAxisProps = ComponentProps<typeof RechartsPolarRadiusAxis>;

/**
 * The radial value axis — the scale running from the center outward. Forwards
 * every Recharts PolarRadiusAxis prop. Hidden automatically while the chart is
 * loading.
 */
export function PolarRadiusAxis({
  tick = { fill: "currentColor", fontSize: 10 },
  tickLine = false,
  axisLine = false,
  ...props
}: PolarRadiusAxisProps) {
  const { isLoading } = useRadarChart();

  if (isLoading) return null;

  return (
    <RechartsPolarRadiusAxis tick={tick} tickLine={tickLine} axisLine={axisLine} {...props} />
  );
}

type TooltipProps = {
  variant?: TooltipVariant; // visual style of the tooltip surface
  roundness?: TooltipRoundness; // border-radius of the tooltip
  defaultIndex?: number; // data index shown by default with no hover
};

/**
 * The hover tooltip. Reads the chart's selection from context so its content
 * dims unselected series. Hidden automatically while the chart is loading.
 */
export function Tooltip({ variant, roundness, defaultIndex }: TooltipProps) {
  const { isLoading, selectedDataKey } = useRadarChart();

  if (isLoading) return null;

  return (
    <ChartTooltip
      defaultIndex={defaultIndex}
      cursor={false}
      content={
        <ChartTooltipContent selected={selectedDataKey} roundness={roundness} variant={variant} />
      }
    />
  );
}

type LegendProps = {
  variant?: ChartLegendVariant; // visual style of the legend indicators
  align?: "left" | "center" | "right"; // horizontal placement
  verticalAlign?: "top" | "middle" | "bottom"; // vertical placement
  isClickable?: boolean; // lets each entry toggle selection of its series
};

/**
 * The series legend. When `isClickable` is set, each entry toggles selection of
 * its series, driving the shared selection state read by every <Radar />.
 * Hidden automatically while the chart is loading.
 */
export function Legend({
  variant,
  align = "center",
  verticalAlign = "bottom",
  isClickable = false,
}: LegendProps) {
  const { isLoading, selectedDataKey, selectDataKey } = useRadarChart();

  if (isLoading) return null;

  return (
    <ChartLegend
      verticalAlign={verticalAlign}
      align={align}
      content={
        <ChartLegendContent
          selected={selectedDataKey}
          onSelectChange={selectDataKey}
          isClickable={isClickable}
          variant={variant}
        />
      }
    />
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Dot helpers
// ─────────────────────────────────────────────────────────────────────────────

type RadarDotProp = ComponentProps<typeof RechartsRadar>["dot"];
type RadarActiveDotProp = ComponentProps<typeof RechartsRadar>["activeDot"];

// Pulls <Dot /> and <ActiveDot /> out of a radar's children into Recharts dot slots
const resolveDots = (
  children: ReactNode,
  id: string,
  dataKey: string,
  dotOpacity: number,
): { dot: RadarDotProp; activeDot: RadarActiveDotProp } => {
  let dot: RadarDotProp = false;
  let activeDot: RadarActiveDotProp = false;

  Children.forEach(children, (child) => {
    if (!isValidElement(child)) return;

    if (child.type === Dot) {
      const { variant } = (child as ReactElement<DotProps>).props;
      dot = <ChartDot type={variant} dataKey={dataKey} chartId={id} fillOpacity={dotOpacity} />;
    }

    if (child.type === ActiveDot) {
      const { variant } = (child as ReactElement<DotProps>).props;
      activeDot = (
        <ChartDot type={variant} dataKey={dataKey} chartId={id} fillOpacity={dotOpacity} />
      );
    }
  });

  return { dot, activeDot };
};

// ─────────────────────────────────────────────────────────────────────────────
// Style definitions — one set per <Radar />, scoped to its unique id
// ─────────────────────────────────────────────────────────────────────────────

type StyleProps = {
  id: string; // unique id of the owning <Radar />
  dataKey: string; // series key the styles belong to
  config: ChartConfig; // colors + labels for every series
};

type ColorStopsProps = {
  dataKey: string; // series key the stops belong to
  colorsCount: number; // number of color steps in the gradient
  opacities?: number[]; // optional per-stop opacity ramp
};

// Emits one <stop> per color, falling back to a flat gradient for single colors
const ColorStops = ({ dataKey, colorsCount, opacities }: ColorStopsProps) => {
  if (colorsCount === 1) {
    return (
      <>
        <stop offset="0%" stopColor={`var(--color-${dataKey}-0)`} stopOpacity={opacities?.[0]} />
        <stop
          offset="100%"
          stopColor={`var(--color-${dataKey}-0)`}
          stopOpacity={opacities?.[opacities.length - 1]}
        />
      </>
    );
  }

  return (
    <>
      {Array.from({ length: colorsCount }, (_, index) => {
        const offset = `${(index / (colorsCount - 1)) * 100}%`;
        return (
          <stop
            key={offset}
            offset={offset}
            stopColor={`var(--color-${dataKey}-${index}, var(--color-${dataKey}-0))`}
            stopOpacity={opacities?.[index]}
          />
        );
      })}
    </>
  );
};

/**
 * Horizontal left-to-right color gradient for a series. Always rendered — the
 * radar's dots paint from this single gradient.
 */
const ColorGradient = ({ id, dataKey, config }: StyleProps) => {
  const colorsCount = getColorsCount(config[dataKey] ?? {});

  return (
    <linearGradient id={`${id}-colors-${dataKey}`} x1="0" y1="0" x2="1" y2="0">
      <ColorStops dataKey={dataKey} colorsCount={colorsCount} />
    </linearGradient>
  );
};

/** Diagonal color gradient used for the radar's outline stroke. */
const StrokeGradient = ({ id, dataKey, config }: StyleProps) => {
  const colorsCount = getColorsCount(config[dataKey] ?? {});

  return (
    <linearGradient id={`${id}-radar-stroke-${dataKey}`} x1="0" y1="0" x2="1" y2="1">
      <ColorStops dataKey={dataKey} colorsCount={colorsCount} />
    </linearGradient>
  );
};

/** Radial color gradient used for the radar's filled area, fading toward the edge. */
const FillGradient = ({ id, dataKey, config }: StyleProps) => {
  const colorsCount = getColorsCount(config[dataKey] ?? {});
  const opacities =
    colorsCount === 1 ? [0.8, 0.3] : Array.from({ length: colorsCount }, (_, i) => (i === 0 ? 0.8 : 0.3));

  return (
    <radialGradient id={`${id}-radar-fill-${dataKey}`} cx="50%" cy="50%" r="50%">
      <ColorStops dataKey={dataKey} colorsCount={colorsCount} opacities={opacities} />
    </radialGradient>
  );
};

/** Soft outer glow filter applied to a radar when `isGlowing` is set. */
const GlowFilter = ({ id, dataKey }: Pick<StyleProps, "id" | "dataKey">) => {
  return (
    <filter id={`${id}-radar-glow-${dataKey}`} x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceGraphic" stdDeviation="4" result="blur" />
      <feColorMatrix
        in="blur"
        type="matrix"
        values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 0.6 0"
        result="glow"
      />
      <feMerge>
        <feMergeNode in="glow" />
        <feMergeNode in="SourceGraphic" />
      </feMerge>
    </filter>
  );
};

// ─────────────────────────────────────────────────────────────────────────────
// Loading skeleton
// ─────────────────────────────────────────────────────────────────────────────

// Builds a fresh set of randomized loading points for the skeleton radar
const generateLoadingData = (points: number) => {
  const categories = ["A", "B", "C", "D", "E", "F"];

  return categories.slice(0, points).map((category) => ({
    skill: category,
    [LOADING_RADAR_DATA_KEY]: 30 + Math.random() * 70,
  }));
};

/**
 * Hook that regenerates the loading skeleton data on a fixed interval, so the
 * skeleton radar keeps animating between shapes while the chart is loading.
 */
export function useLoadingData(isLoading: boolean, loadingPoints: number = LOADING_POINTS) {
  const [refreshKey, setRefreshKey] = useState(0);

  useEffect(() => {
    if (!isLoading) return;

    const interval = setInterval(() => {
      setRefreshKey((prev) => prev + 1);
    }, LOADING_ANIMATION_DURATION);

    return () => clearInterval(interval);
  }, [isLoading]);

  const loadingData = useMemo(
    () => generateLoadingData(loadingPoints),
    // refreshKey toggle triggers re-computation each animation cycle
    // eslint-disable-next-line react-hooks/exhaustive-deps
    [loadingPoints, refreshKey],
  );

  return loadingData;
}

/**
 * The skeleton radar shown while the chart is loading. Rendered by the root in
 * place of the real radars, it animates between randomized shapes.
 */
const LoadingRadar = () => {
  return (
    <RechartsRadar
      dataKey={LOADING_RADAR_DATA_KEY}
      stroke="currentColor"
      strokeOpacity={0.3}
      strokeWidth={2}
      fill="currentColor"
      fillOpacity={0.1}
      dot={false}
      isAnimationActive
      animationDuration={LOADING_ANIMATION_DURATION}
      animationEasing="ease-in-out"
    />
  );
};
```
        
      
       
        #### Now, Let's add the chart component to your project.

        
          These Components are required by the chart component to render the chart. Make a folder called `ui` inside the `evilcharts` folder and paste the code there.

          Below is main chart component.
        
        
          #### Component Implementation

##### Source File: `chart.tsx`

```tsx
"use client";

import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

// Format: { THEME_NAME: CSS_SELECTOR }
const THEMES = { light: "", dark: ".dark" } as const;

type ThemeKey = keyof typeof THEMES;

// All Keys are optional at first
type ThemeColorsBase = {
  [K in ThemeKey]?: string[];
};

// Require at least one theme key
type AtLeastOneThemeColor = {
  [K in ThemeKey]: Required<Pick<ThemeColorsBase, K>> & Partial<Omit<ThemeColorsBase, K>>;
}[ThemeKey];

const VALID_THEME_KEYS = Object.keys(THEMES) as ThemeKey[];

// Validation for chart config colors at runtime
function validateChartConfigColors(config: ChartConfig): void {
  for (const [key, value] of Object.entries(config)) {
    if (value.colors) {
      const hasValidThemeKey = VALID_THEME_KEYS.some(
        (themeKey) => value.colors?.[themeKey] !== undefined,
      );

      if (!hasValidThemeKey) {
        throw new Error(
          `[EvilCharts] Invalid chart config for "${key}": colors object must have at least one theme key (${VALID_THEME_KEYS.join(", ")}). Received empty object or invalid keys.`,
        );
      }
    }
  }
}

export type ChartConfig = Record<
  string,
  {
    label?: React.ReactNode;
    icon?: React.ComponentType;
    colors?: AtLeastOneThemeColor;
  }
>;

interface ChartContextProps {
  config: ChartConfig;
}

const ChartContext = React.createContext<ChartContextProps | null>(null);

export function useChart() {
  const context = React.useContext(ChartContext);

  if (!context) {
    throw new Error("useChart must be used within a <ChartContainer />");
  }

  return context;
}

interface ChartContainerProps
  extends
    Omit<React.ComponentProps<"div">, "children">,
    Pick<
      React.ComponentProps<typeof RechartsPrimitive.ResponsiveContainer>,
      | "initialDimension"
      | "aspect"
      | "debounce"
      | "minHeight"
      | "minWidth"
      | "maxHeight"
      | "height"
      | "width"
      | "onResize"
      | "children"
    > {
  config: ChartConfig;
  innerResponsiveContainerStyle?: React.ComponentProps<
    typeof RechartsPrimitive.ResponsiveContainer
  >["style"];
  /** Optional content rendered below the chart (e.g. EvilBrush) */
  footer?: React.ReactNode;
}

function ChartContainer({
  id,
  config,
  initialDimension = { width: 320, height: 200 },
  className,
  children,
  footer,
  ...props
}: Readonly<ChartContainerProps>) {
  const uniqueId = React.useId();
  const chartId = `chart-${id ?? uniqueId.replace(/:/g, "")}`;

  // Validate chart config at runtime
  validateChartConfigColors(config);

  return (
    <ChartContext.Provider value={{ config }}>
      <div
        data-slot="chart"
        data-chart={chartId}
        className={cn(
          "min-h-0 w-full flex-1",
          "[&_.recharts-cartesian-axis-tick_text]:fill-muted-foreground [&_.recharts-cartesian-grid_line[stroke='#ccc']]:stroke-border/50 [&_.recharts-curve.recharts-tooltip-cursor]:stroke-border [&_.recharts-polar-grid_[stroke='#ccc']]:stroke-border [&_.recharts-radial-bar-background-sector]:fill-muted [&_.recharts-rectangle.recharts-tooltip-cursor]:fill-muted [&_.recharts-reference-line_[stroke='#ccc']]:stroke-border relative flex flex-col justify-center text-xs [&_.recharts-dot[stroke='#fff']]:stroke-transparent [&_.recharts-layer]:outline-hidden [&_.recharts-sector]:outline-hidden [&_.recharts-sector[stroke='#fff']]:stroke-transparent [&_.recharts-surface]:outline-hidden",
          !footer && "aspect-video",
          className,
        )}
        {...props}
      >
        <ChartStyle id={chartId} config={config} />
        <RechartsPrimitive.ResponsiveContainer
          className="min-h-0 w-full flex-1"
          initialDimension={initialDimension}
        >
          {children}
        </RechartsPrimitive.ResponsiveContainer>
        {footer}
      </div>
    </ChartContext.Provider>
  );
}

function LoadingIndicator({ isLoading }: { isLoading: boolean }) {
  if (!isLoading) {
    return null;
  }

  return (
    <div className="pointer-events-none absolute inset-0 z-20 flex items-center justify-center">
      <div className="text-primary bg-background flex items-center justify-center gap-2 rounded-md border px-2 py-0.5 text-sm">
        <div className="border-border border-t-primary h-3 w-3 animate-spin rounded-full border" />
        <span>Loading</span>
      </div>
    </div>
  );
}

// Distribute colors evenly across slots, extra slots go to last color(s)
// Example: 2 colors for 4 slots → [red, red, pink, pink]
// Example: 3 colors for 4 slots → [red, pink, blue, blue]
function distributeColors(colorsArray: string[], maxCount: number): string[] {
  const availableCount = colorsArray.length;
  if (availableCount >= maxCount) {
    return colorsArray.slice(0, maxCount);
  }

  const result: string[] = [];
  const baseSlots = Math.floor(maxCount / availableCount);
  const extraSlots = maxCount % availableCount;

  // First (availableCount - extraSlots) colors get baseSlots each
  // Last extraSlots colors get (baseSlots + 1) each
  for (let colorIdx = 0; colorIdx < availableCount; colorIdx++) {
    const isExtraColor = colorIdx >= availableCount - extraSlots;
    const slotsForThisColor = baseSlots + (isExtraColor ? 1 : 0);
    for (let j = 0; j < slotsForThisColor; j++) {
      result.push(colorsArray[colorIdx]);
    }
  }

  return result;
}

const ChartStyle = ({ id, config }: { id: string; config: ChartConfig }) => {
  const colorConfig = Object.entries(config).filter(([, config]) => config.colors);

  if (!colorConfig.length) {
    return null;
  }

  const generateCssVars = (theme: keyof typeof THEMES) =>
    colorConfig
      .flatMap(([key, itemConfig]) => {
        const colorsArray = itemConfig.colors?.[theme];
        if (!colorsArray || !Array.isArray(colorsArray) || colorsArray.length === 0) {
          return [];
        }

        // Get max count across all themes for this key
        const maxCount = getColorsCount(itemConfig);

        // Distribute colors evenly across all required slots
        const distributedColors = distributeColors(colorsArray, maxCount);

        return distributedColors.map((color, index) => `  --color-${key}-${index}: ${color};`);
      })
      .filter(Boolean)
      .join("\n");

  const css = Object.entries(THEMES)
    .map(
      ([theme, prefix]) =>
        `${prefix} [data-chart=${id}] {\n${generateCssVars(theme as keyof typeof THEMES)}\n}`,
    )
    .join("\n");

  return <style dangerouslySetInnerHTML={{ __html: css }} />;
};

// Helper to extract item config from a payload.
export function getPayloadConfigFromPayload(config: ChartConfig, payload: unknown, key: string) {
  if (typeof payload !== "object" || payload === null) {
    return undefined;
  }

  const payloadPayload =
    "payload" in payload && typeof payload.payload === "object" && payload.payload !== null
      ? payload.payload
      : undefined;

  let configLabelKey: string = key;

  if (key in payload && typeof payload[key as keyof typeof payload] === "string") {
    configLabelKey = payload[key as keyof typeof payload] as string;
  } else if (
    payloadPayload &&
    key in payloadPayload &&
    typeof payloadPayload[key as keyof typeof payloadPayload] === "string"
  ) {
    configLabelKey = payloadPayload[key as keyof typeof payloadPayload] as string;
  }

  return configLabelKey in config ? config[configLabelKey] : config[key];
}

// Format values to percent for expanded charts
function axisValueToPercentFormatter(value: number) {
  return `${Math.round(value * 100).toFixed(0)}%`;
}

// Get max colors count across all themes for a config entry
function getColorsCount(config: ChartConfig[string]): number {
  if (!config.colors) return 1;
  const counts = VALID_THEME_KEYS.map((theme) => config.colors?.[theme]?.length ?? 0);
  return Math.max(...counts, 1);
}

// Generate random loading data for skeleton/loading state
// min/max represent percentage of the range (0-100), defaults to 20-80 for realistic look
export const getLoadingData = (points: number = 10, min: number = 0, max: number = 70) => {
  const range = max - min;
  return Array.from({ length: points }, () => ({
    loading: Math.floor(Math.random() * range) + min,
  }));
};

export {
  ChartContainer,
  ChartStyle,
  axisValueToPercentFormatter,
  LoadingIndicator,
  getColorsCount,
};
```
        
      
       
        #### Now, We need to add sub components.

        
          Create a file called `tooltip.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `tooltip.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import type { NameType, ValueType } from "recharts/types/component/DefaultTooltipContent";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type TooltipRoundness = "sm" | "md" | "lg" | "xl";
type TooltipVariant = "default" | "frosted-glass";

const roundnessMap: Record<TooltipRoundness, string> = {
  sm: "rounded-sm",
  md: "rounded-md",
  lg: "rounded-lg",
  xl: "rounded-xl",
};

const variantMap: Record<TooltipVariant, string> = {
  default: "bg-background",
  "frosted-glass": "bg-background/70 backdrop-blur-sm",
};

function ChartTooltipContent({
  active,
  payload,
  className,
  indicator = "dot",
  hideLabel = false,
  hideIndicator = false,
  label,
  labelFormatter,
  labelClassName,
  formatter,
  nameKey,
  labelKey,
  selected,
  roundness = "lg",
  variant = "default",
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip> &
  React.ComponentProps<"div"> & {
    hideLabel?: boolean;
    hideIndicator?: boolean;
    indicator?: "line" | "dot" | "dashed";
    nameKey?: string;
    labelKey?: string;
    selected?: string | null;
    roundness?: TooltipRoundness;
    variant?: TooltipVariant;
  } & Omit<
    RechartsPrimitive.DefaultTooltipContentProps<ValueType, NameType>,
    "accessibilityLayer"
  >) {
  const { config } = useChart();

  const tooltipLabel = React.useMemo(() => {
    if (hideLabel || !payload?.length) {
      return null;
    }

    const [item] = payload;
    const key = `${labelKey ?? item?.dataKey ?? item?.name ?? "value"}`;
    const itemConfig = getPayloadConfigFromPayload(config, item, key);
    const value =
      !labelKey && typeof label === "string" ? (config[label]?.label ?? label) : itemConfig?.label;

    if (labelFormatter) {
      return (
        <div className={cn("font-medium", labelClassName)}>{labelFormatter(value, payload)}</div>
      );
    }

    if (!value) {
      return null;
    }

    return <div className={cn("font-medium", labelClassName)}>{value}</div>;
  }, [label, labelFormatter, payload, hideLabel, labelClassName, config, labelKey]);

  if (!active || !payload?.length) {
    // Empty tooltip - to prevent position getting 0.0 so it doesnt animate tooltip every time from 0.0 origin
    return <span className="p-4" />;
  }

  const nestLabel = payload.length === 1 && indicator !== "dot";

  return (
    <div
      className={cn(
        "border-border/50 grid min-w-32 items-start gap-1.5 border px-2.5 py-1.5 text-xs shadow-xl",
        roundnessMap[roundness],
        variantMap[variant],
        className,
      )}
    >
      {!nestLabel ? tooltipLabel : null}
      <div className="grid gap-1.5">
        {payload
          .filter((item) => item.type !== "none")
          .map((item, index) => {
            // For pie charts, item.name contains the sector name (e.g., "chrome")
            // For radial charts, the name is in item.payload[nameKey]
            // For other charts, item.name or item.dataKey contains the series name
            const payloadName =
              nameKey && item.payload
                ? (item.payload as Record<string, unknown>)[nameKey]
                : undefined;
            const key = `${payloadName ?? item.name ?? item.dataKey ?? "value"}`;
            const itemConfig = getPayloadConfigFromPayload(config, item, key);

            // Get colors count for this item to determine gradient vs solid
            const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

            return (
              <div
                key={index}
                className={cn(
                  "[&>svg]:text-muted-foreground flex w-full flex-wrap items-stretch gap-2 [&>svg]:h-2.5 [&>svg]:w-2.5",
                  indicator === "dot" && "items-center",
                  selected != null && selected !== item.dataKey && "opacity-30",
                )}
              >
                {formatter && item?.value !== undefined && item.name ? (
                  formatter(item.value, item.name, item, index, item.payload)
                ) : (
                  <>
                    {itemConfig?.icon ? (
                      <itemConfig.icon />
                    ) : (
                      !hideIndicator && (
                        <div
                          className={cn("shrink-0 rounded-[2px]", {
                            "h-2.5 w-2.5": indicator === "dot",
                            "w-1": indicator === "line",
                            "w-0 border-[1.5px] border-dashed bg-transparent!":
                              indicator === "dashed",
                            "my-0.5": nestLabel && indicator === "dashed",
                          })}
                          style={getIndicatorColorStyle(key, colorsCount)}
                        />
                      )
                    )}
                    <div
                      className={cn(
                        "flex flex-1 justify-between gap-4 leading-none",
                        nestLabel ? "items-end" : "items-center",
                      )}
                    >
                      <div className="grid gap-1.5">
                        {nestLabel ? tooltipLabel : null}
                        <span className="text-muted-foreground">
                          {itemConfig?.label ?? item.name}
                        </span>
                      </div>
                      {item.value != null && (
                        <span className="text-foreground font-mono font-medium tabular-nums">
                          {typeof item.value === "number"
                            ? item.value.toLocaleString()
                            : String(item.value)}
                        </span>
                      )}
                    </div>
                  </>
                )}
              </div>
            );
          })}
      </div>
    </div>
  );
}

function getIndicatorColorStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { background: `var(--color-${dataKey}-0)` };
  }

  // Multiple colors: create linear gradient with evenly distributed stops
  const stops = Array.from({ length: colorsCount }, (_, index) => {
    const offset = (index / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${index}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

const ChartTooltip = ({
  animationDuration = 200,
  ...props
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip>) => (
  <RechartsPrimitive.Tooltip animationDuration={animationDuration} {...props} />
);

export { ChartTooltip, ChartTooltipContent };
export type { TooltipRoundness, TooltipVariant };
```
        
        
          Now, create another file called `legend.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `legend.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type ChartLegendVariant =
  | "square"
  | "circle"
  | "circle-outline"
  | "rounded-square"
  | "rounded-square-outline"
  | "vertical-bar"
  | "horizontal-bar";

function ChartLegendContent({
  className,
  hideIcon = false,
  nameKey,
  payload,
  verticalAlign,
  align = "right",
  selected,
  onSelectChange,
  isClickable,
  variant = "rounded-square",
}: React.ComponentProps<"div"> & {
  hideIcon?: boolean;
  nameKey?: string;
  selected?: string | null;
  isClickable?: boolean;
  onSelectChange?: (selected: string | null) => void;
  variant?: ChartLegendVariant;
} & RechartsPrimitive.DefaultLegendContentProps) {
  const { config } = useChart();

  if (!payload?.length) {
    return null;
  }

  return (
    <div
      className={cn(
        "flex items-center gap-4 select-none",
        align === "left" && "justify-start",
        align === "center" && "justify-center",
        align === "right" && "justify-end",
        verticalAlign === "top" ? "pb-4" : "pt-4",
        className,
      )}
    >
      {payload
        .filter((item) => item.type !== "none")
        .map((item) => {
          // For pie charts, item.value contains the sector name (e.g., "chrome")
          // For radial charts, the name is in item.payload[nameKey]
          // For other charts, item.dataKey contains the series name (e.g., "desktop")
          const payloadName =
            nameKey && item.payload
              ? (item.payload as Record<string, unknown>)[nameKey]
              : undefined;
          const key = `${payloadName ?? item.value ?? item.dataKey ?? "value"}`;
          const itemConfig = getPayloadConfigFromPayload(config, item, key);
          const isSelected = selected === null || selected === key;

          // Get colors count for this item to determine gradient vs solid
          const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

          return (
            <div
              key={key}
              className={cn(
                "[&>svg]:text-muted-foreground flex items-center gap-1.5 transition-opacity [&>svg]:h-3 [&>svg]:w-3",
                !isSelected && "opacity-30",
                isClickable && "cursor-pointer",
              )}
              onClick={() => {
                if (!isClickable) return;

                onSelectChange?.(selected === key ? null : key);
              }}
            >
              {itemConfig?.icon && !hideIcon ? (
                <itemConfig.icon />
              ) : (
                <LegendIndicator
                  variant={variant}
                  dataKey={key}
                  colorsCount={colorsCount}
                />
              )}
              {itemConfig?.label}
            </div>
          );
        })}
    </div>
  );
}

// ---------------------------------------------------------------------------
// Legend indicator — each variant gets its own branch so future variants
// can diverge freely in markup & style.
// ---------------------------------------------------------------------------

function LegendIndicator({
  variant,
  dataKey,
  colorsCount,
}: {
  variant: ChartLegendVariant;
  dataKey: string;
  colorsCount: number;
}) {
  const fillStyle = getLegendFillStyle(dataKey, colorsCount);
  const outlineStyle = getLegendOutlineStyle(dataKey, colorsCount);

  switch (variant) {
    case "square":
      return <div className="h-2 w-2 shrink-0" style={fillStyle} />;

    case "circle":
      return <div className="h-2 w-2 shrink-0 rounded-full" style={fillStyle} />;

    case "circle-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-full p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "vertical-bar":
      return <div className="h-3 w-1 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "horizontal-bar":
      return <div className="h-1 w-3 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "rounded-square-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-[3px] p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "rounded-square":
    default:
      return <div className="h-2 w-2 shrink-0 rounded-[2px]" style={fillStyle} />;
  }
}

// ---------------------------------------------------------------------------
// Style helpers
// ---------------------------------------------------------------------------

/** Solid fill / gradient background for filled variants. */
function getLegendFillStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { backgroundColor: `var(--color-${dataKey}-0)` };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

/**
 * Outline style for stroke variants.
 * Uses background + mask-composite to punch out the center, leaving only the
 * "border" visible. Works with both solid colors and gradients, and respects
 * border-radius — unlike plain `border-color`.
 */
function getLegendOutlineStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  const maskStyle: React.CSSProperties = {
    WebkitMask:
      "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    WebkitMaskComposite: "xor",
    mask: "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    maskComposite: "exclude",
  };

  if (colorsCount <= 1) {
    return {
      backgroundColor: `var(--color-${dataKey}-0)`,
      ...maskStyle,
    };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return {
    background: `linear-gradient(to right, ${stops})`,
    ...maskStyle,
  };
}

const ChartLegend = RechartsPrimitive.Legend;

export { ChartLegend, ChartLegendContent, type ChartLegendVariant };
```
        
      
    
  


## Usage

The radar chart is a composible compound component. `<EvilRadarChart />` is the root container — every visual part (`<PolarGrid />`, `<PolarAngleAxis />`, `<Tooltip />`, `<Legend />`, and the `<Radar />` series) is composed as a child, so you render exactly what you need.

```tsx
import {
  EvilRadarChart,
  Radar,
  PolarGrid,
  PolarAngleAxis,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/components/evilcharts/charts/radar-chart";
```

```tsx
const data = [
  { skill: "JavaScript", desktop: 186, mobile: 80 },
  { skill: "TypeScript", desktop: 305, mobile: 200 },
  { skill: "React", desktop: 237, mobile: 120 },
  { skill: "Node.js", desktop: 173, mobile: 190 },
  { skill: "CSS", desktop: 209, mobile: 130 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: { light: ["#3b82f6"], dark: ["#60a5fa"] },
  },
  mobile: {
    label: "Mobile",
    colors: { light: ["#10b981"], dark: ["#34d399"] },
  },
} satisfies ChartConfig;

<EvilRadarChart data={data} config={chartConfig}>
  <PolarGrid />
  <PolarAngleAxis dataKey="skill" />
  <Legend />
  <Tooltip />
  <Radar dataKey="desktop" variant="filled">
    <Dot variant="colored-border" />
    <ActiveDot variant="default" />
  </Radar>
  <Radar dataKey="mobile" variant="filled" />
</EvilRadarChart>
```

### Interactive Selection

Set `isClickable` on a `<Radar />` to let it be selected by clicking, and on `<Legend />` to let legend entries toggle selection. Use the root's `onSelectionChange` callback to react to selection events:

```tsx
<EvilRadarChart
  data={data}
  config={chartConfig}
  onSelectionChange={(selectedDataKey) => {
    if (selectedDataKey) {
      console.log("Selected:", selectedDataKey);
    } else {
      console.log("Deselected");
    }
  }}
>
  <PolarGrid />
  <PolarAngleAxis dataKey="skill" />
  <Legend isClickable />
  <Tooltip />
  <Radar dataKey="desktop" variant="filled" isClickable />
  <Radar dataKey="mobile" variant="filled" isClickable />
</EvilRadarChart>
```

### Loading State

#### Live Example Code

##### Example File: `ex-loading-state-radar-chart.tsx`

```tsx
"use client";

import {
  EvilRadarChart,
  Radar,
  PolarGrid,
  PolarAngleAxis,
  Tooltip,
  Legend,
} from "@/registry/charts/radar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleRadarChart() {
  return (
    <EvilRadarChart
      data={[]} // if isLoading is true, pass empty array → i.e isLoading ? [] : data
      config={chartConfig}
      className="h-full w-full p-4"
      isLoading={true} // [!code highlight]
    >
      <PolarGrid />
      <PolarAngleAxis dataKey="skill" />
      <Legend />
      <Tooltip />
      <Radar dataKey="desktop" variant="filled" />
      <Radar dataKey="mobile" variant="filled" />
    </EvilRadarChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    The radar chart supports loading state with animated data. You can pass the `isLoading` prop to the root to show a loading animation while your data is being fetched.
  


```tsx
<EvilRadarChart data={[]} config={chartConfig} isLoading>
  <PolarGrid />
  <PolarAngleAxis dataKey="skill" />
  <Legend />
  <Tooltip />
  <Radar dataKey="desktop" variant="filled" />
  <Radar dataKey="mobile" variant="filled" />
</EvilRadarChart>
```

## Examples

Below are some examples of the radar chart with different configurations.

### Lines Variant

#### Live Example Code

##### Example File: `ex-lines-variant-radar-chart.tsx`

```tsx
"use client";

import {
  EvilRadarChart,
  Radar,
  PolarGrid,
  PolarAngleAxis,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/registry/charts/radar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { skill: "JavaScript", desktop: 186, mobile: 80 },
  { skill: "TypeScript", desktop: 305, mobile: 200 },
  { skill: "React", desktop: 237, mobile: 120 },
  { skill: "Node.js", desktop: 173, mobile: 190 },
  { skill: "CSS", desktop: 209, mobile: 130 },
  { skill: "Python", desktop: 214, mobile: 140 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleRadarChart() {
  return (
    <EvilRadarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <PolarGrid />
      <PolarAngleAxis dataKey="skill" />
      <Legend />
      <Tooltip />
      <Radar
        dataKey="desktop"
        variant="lines" // [!code highlight]
      >
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Radar>
      <Radar dataKey="mobile" variant="lines">
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Radar>
    </EvilRadarChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    Set `variant="lines"` to show only the outline without fill. This is useful for comparing multiple datasets more clearly.
  


### Circle Grid

#### Live Example Code

##### Example File: `ex-circle-grid-radar-chart.tsx`

```tsx
"use client";

import {
  EvilRadarChart,
  Radar,
  PolarGrid,
  PolarAngleAxis,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/registry/charts/radar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { skill: "JavaScript", desktop: 186, mobile: 80 },
  { skill: "TypeScript", desktop: 305, mobile: 200 },
  { skill: "React", desktop: 237, mobile: 120 },
  { skill: "Node.js", desktop: 173, mobile: 190 },
  { skill: "CSS", desktop: 209, mobile: 130 },
  { skill: "Python", desktop: 214, mobile: 140 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleRadarChart() {
  return (
    <EvilRadarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <PolarGrid
        gridType="circle" // [!code highlight]
      />
      <PolarAngleAxis dataKey="skill" />
      <Legend />
      <Tooltip />
      <Radar dataKey="desktop" variant="filled">
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Radar>
      <Radar dataKey="mobile" variant="filled">
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Radar>
    </EvilRadarChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    Set `gridType="circle"` to use circular grid lines instead of the default polygon grid.
  


### Gradient Colors

#### Live Example Code

##### Example File: `ex-gradient-colors-radar-chart.tsx`

```tsx
"use client";

import {
  EvilRadarChart,
  Radar,
  PolarGrid,
  PolarAngleAxis,
  Tooltip,
  Legend,
  Dot,
  ActiveDot,
} from "@/registry/charts/radar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { skill: "JavaScript", desktop: 186, mobile: 80 },
  { skill: "TypeScript", desktop: 305, mobile: 200 },
  { skill: "React", desktop: 237, mobile: 120 },
  { skill: "Node.js", desktop: 173, mobile: 190 },
  { skill: "CSS", desktop: 209, mobile: 130 },
  { skill: "Python", desktop: 214, mobile: 140 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#6366f1", "#a855f7", "#ec4899"], // Indigo -> Purple -> Pink // [!code highlight]
      dark: ["red", "orange", "pink"], // [!code highlight]
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#14b8a6", "#06b6d4", "#3b82f6"], // Teal -> Cyan -> Blue // [!code highlight]
      dark: ["#2dd4bf", "#22d3ee", "#60a5fa"], // [!code highlight]
    },
  },
} satisfies ChartConfig;

export function EvilExampleRadarChart() {
  return (
    <EvilRadarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <PolarGrid />
      <PolarAngleAxis dataKey="skill" />
      <Legend />
      <Tooltip />
      <Radar dataKey="desktop" variant="filled">
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Radar>
      <Radar dataKey="mobile" variant="filled">
        <Dot variant="colored-border" />
        <ActiveDot variant="default" />
      </Radar>
    </EvilRadarChart>
  );
}
```

### Glowing Radars

" name="ex-glowing-radar-chart"  />
> [!NOTE]
 
  > [!NOTE]

    Add a subtle glow effect to a radar by setting `isGlowing` on its `<Radar />`. Each radar controls its own glow independently.
  



## API Reference

The radar chart is composed of a root container and a set of composible parts. Each part is documented in its own section below.

### EvilRadarChart


The root container. Owns the data, the shared context, and the loading skeleton. All other parts must be rendered as its children.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **data*** | `TData[]` | - | Data used to display the chart. An array of objects where each object represents a data point on the radar (`TData extends Record`). |
| **config** | - | - | " required>     Configuration object that defines the chart's radar series. Each key should match a numeric data key in your data array, with corresponding colors and labels. |
| **children*** | `ReactNode` | - | The composed parts of the chart — ``, ``, ``, ``, ``, and one or more `` series. |
| **className** | `string` | - | Additional CSS classes to apply to the chart container. |
| **backgroundVariant** | `BackgroundVariant` | - | Background pattern variant to display behind the chart. |
| **defaultSelectedDataKey** | `string | null` | `null` | The radar series selected on first render. |
| **onSelectionChange** | - | - | void">     Callback fired when a radar is selected or deselected. Receives the selected data key, or `null` when deselected. |
| **isLoading** | `boolean` | `false` | Shows a loading animation with animated data when data is being fetched. |
| **loadingPoints** | `number` | `6` | Number of points rendered in the loading skeleton radar. |
| **chartProps** | - | - | ">     Additional props to pass to the underlying Recharts RadarChart component. Read the Recharts RadarChart documentation for available props. |



### Radar


A single radar series. Each `<Radar />` is self-contained — it generates its own gradients and glow filter under a unique id, so multiple radars never collide on styles. Compose `<Dot />` and `<ActiveDot />` inside it to add point markers.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **dataKey*** | `string` | - | The series key to render. Must exist on both the data and the config. |
| **variant** | - | - | The visual style for this radar. `"filled"` shows a filled area, `"lines"` shows only the outline. |
| **fillOpacity** | `number` | `0.3` | The opacity of the filled area when using `variant="filled"`. |
| **isGlowing** | `boolean` | `false` | Adds a soft outer glow around this radar. Each radar controls its own glow independently. |
| **isClickable** | `boolean` | `false` | Enables interactive clicking on this radar to select/deselect it. When a radar is selected, unselected radars become semi-transparent. |
| **children** | `ReactNode` | - | Optional `` and `` composition for point markers on this radar. |
| **radarProps** | - | - | , "dataKey">'>     Additional props to pass to the underlying Recharts Radar component. Read the Recharts Radar documentation for available props. |



### Dot / ActiveDot


Configuration slots composed inside a `<Radar />`. `<Dot />` styles the resting point markers; `<ActiveDot />` styles the hovered/active marker. They render nothing on their own.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style for the point marker. |



### PolarGrid


The polar grid lines. Defaults to a dashed polygon grid and forwards every Recharts PolarGrid prop.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **gridType** | - | - | The shape of the grid lines. `"polygon"` creates angular grid lines, `"circle"` creates circular grid lines. |
| **...props** | - | - | ">     All other props are forwarded to the underlying Recharts PolarGrid component. Read the Recharts PolarGrid documentation for available props. |



### PolarAngleAxis


The angular category axis — the labels around the chart's perimeter. Hidden automatically while the chart is loading.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **dataKey** | `string` | - | The key from your data objects to use for the angle axis labels (e.g., categories, skills, months). |
| **...props** | - | - | ">     All other props are forwarded to the underlying Recharts PolarAngleAxis component. Read the Recharts PolarAngleAxis documentation for available props. |



### PolarRadiusAxis


The radial value axis — the scale running from the center outward. Hidden automatically while the chart is loading.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **...props** | - | - | ">     All props are forwarded to the underlying Recharts PolarRadiusAxis component. Read the Recharts PolarRadiusAxis documentation for available props. |



### Tooltip


The hover tooltip. Reads the chart's selection so its content dims unselected series. Hidden automatically while the chart is loading.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | Controls the visual style of the tooltip. |
| **roundness** | - | - | Controls the border-radius of the tooltip. |
| **defaultIndex** | `number` | - | When set, the tooltip will be visible by default at the specified data point index. |



### Legend


The series legend. When `isClickable` is set, each entry toggles selection of its series. Hidden automatically while the chart is loading.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style variant for the legend indicators. |
| **align** | - | - | Horizontal placement of the legend. |
| **verticalAlign** | - | - | Vertical placement of the legend. |
| **isClickable** | `boolean` | `false` | Lets each legend entry toggle selection of its series, driving the shared selection state read by every ``. |

---

## Radial Chart <a name="radial-chart"></a>

> Beautiful radial bar charts with full and semi-circle variants, gradient colors, and glow effects

#### Live Example Code

##### Example File: `ex-radial-chart.tsx`

```tsx
"use client";

import { EvilRadialChart, RadialBar, Tooltip, Legend } from "@/registry/charts/radial-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
  { browser: "edge", visitors: 173 },
  { browser: "other", visitors: 90 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  safari: {
    label: "Safari",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
  firefox: {
    label: "Firefox",
    colors: {
      light: ["#f59e0b"],
      dark: ["#fbbf24"],
    },
  },
  edge: {
    label: "Edge",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  other: {
    label: "Other",
    colors: {
      light: ["#6b7280"],
      dark: ["#9ca3af"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleRadialChart() {
  return (
    <EvilRadialChart
      className="h-full w-full p-4"
      data={data}
      nameKey="browser"
      config={chartConfig}
      variant="full" // [!code highlight]
    >
      <Legend isClickable />
      <Tooltip />
      <RadialBar dataKey="visitors" isClickable />
    </EvilRadialChart>
  );
}
```

## Installation


  
    
    
  
  
### CLI Installation


    ```bash
npx shadcn@latest add @evilcharts/radial-chart
```
  
  
### Manual Installation


    
      
        #### Install the following dependencies:

        
          ```bash
npm install recharts
```
        
      
      
        #### Copy and paste the following code snippets into your project.

         
          To use the chart, first create the folder `evilcharts` and a subfolder called `charts` inside your `components` directory.
          Then, copy the following base radial-chart code into a new file in that folder.
        
        
          #### Component Implementation

##### Source File: `radial-chart.tsx`

```tsx
"use client";

import {
  type ChartConfig,
  ChartContainer,
  getColorsCount,
  LoadingIndicator,
} from "@/registry/ui/chart";
import {
  ChartTooltip,
  ChartTooltipContent,
  type TooltipRoundness,
  type TooltipVariant,
} from "@/registry/ui/tooltip";
import { ChartLegend, ChartLegendContent, type ChartLegendVariant } from "@/registry/ui/legend";
import { ChartBackground, type BackgroundVariant } from "@/registry/ui/background";
import {
  createContext,
  use,
  useCallback,
  useEffect,
  useId,
  useMemo,
  useState,
  type ComponentProps,
  type ReactNode,
} from "react";
import {
  RadialBar as RechartsRadialBar,
  RadialBarChart as RechartsRadialBarChart,
  Sector,
  type SectorProps,
} from "recharts";
import { TypedDataKey } from "recharts/types/util/typedDataKey";

// Constants
const DEFAULT_INNER_RADIUS = "30%";
const DEFAULT_OUTER_RADIUS = "100%";
const DEFAULT_CORNER_RADIUS = 5;
const DEFAULT_BAR_SIZE = 14;
const LOADING_BARS = 5;
// Stable empty-array reference so the `glowingBars` default doesn't change every render
const EMPTY_GLOWING_BARS: string[] = [];
const LOADING_ANIMATION_DURATION = 1500; // in milliseconds — interval between skeleton data changes

type RadialBarChartProps = ComponentProps<typeof RechartsRadialBarChart>;
type RadialBarRechartsProps = ComponentProps<typeof RechartsRadialBar>;

type RadialVariant = "full" | "semi";

// ─────────────────────────────────────────────────────────────────────────────
// Shared context
// ─────────────────────────────────────────────────────────────────────────────

/**
 * Shared state for every part of the chart. Lifted into <EvilRadialChart /> so
 * that <RadialBar />, <Tooltip />, and <Legend /> can read it without prop
 * drilling. Sub-components are composed freely — the provider is the single
 * source of truth.
 */
type RadialChartContextValue = {
  config: ChartConfig; // colors + labels for every bar
  nameKey: string; // data key holding each bar's name
  chartId: string; // colon-free id scoping this chart's style defs
  isLoading: boolean; // whether the chart shows its loading skeleton
  selectedBar: string | null; // currently selected bar name, or null when none
  selectBar: (barName: string | null, value?: number) => void; // sets the selected bar
};

const RadialChartContext = createContext<RadialChartContextValue | null>(null);

// Reads the chart context, throwing a helpful error when used outside <EvilRadialChart />
function useRadialChart() {
  const context = use(RadialChartContext);

  if (!context) {
    throw new Error(
      "Radial chart parts (<RadialBar />, <Tooltip />, …) must be used within <EvilRadialChart />",
    );
  }

  return context;
}

// ─────────────────────────────────────────────────────────────────────────────
// Root container
// ─────────────────────────────────────────────────────────────────────────────

type EvilRadialChartBaseProps<TData extends Record<string, unknown>> = {
  config: ChartConfig; // bar colors + labels, keyed by each bar's name
  data: TData[]; // rows rendered by the chart — one bar per row
  nameKey: keyof TData & string; // data key holding each bar's name
  children: ReactNode; // composed parts — <RadialBar />, <Tooltip />, <Legend />
  className?: string; // extra classes for the chart container
  chartProps?: RadialBarChartProps; // escape hatch for the raw Recharts chart
  variant?: RadialVariant; // arc shape — full circle or half circle
  innerRadius?: number | string; // inner radius of the radial bars
  outerRadius?: number | string; // outer radius of the radial bars
  defaultSelectedDataKey?: string | null; // bar selected on first render
  onSelectionChange?: (selection: { dataKey: string; value: number } | null) => void; // fires when the selected bar changes
  isLoading?: boolean; // shows the animated loading skeleton
  backgroundVariant?: BackgroundVariant; // background pattern behind the chart
};

type EvilRadialChartProps<TData extends Record<string, unknown>> =
  EvilRadialChartBaseProps<TData>;

/**
 * Root of the composible radial chart. Owns the data, the shared context, the
 * loading skeleton, and the chart-wide arc shape. Everything visual — the
 * tooltip, legend, and the radial bar itself — is composed as children, so a
 * consumer renders exactly the parts they need.
 */
export function EvilRadialChart<TData extends Record<string, unknown>>({
  config,
  data,
  nameKey,
  children,
  className,
  chartProps,
  variant = "full",
  innerRadius = DEFAULT_INNER_RADIUS,
  outerRadius = DEFAULT_OUTER_RADIUS,
  defaultSelectedDataKey = null,
  onSelectionChange,
  isLoading = false,
  backgroundVariant,
}: EvilRadialChartProps<TData>) {
  const chartId = useId().replace(/:/g, ""); // colon-free id keeps CSS/SVG selectors valid
  const [selectedBar, setSelectedBar] = useState<string | null>(defaultSelectedDataKey);
  const loadingData = useLoadingData(isLoading);

  const variantConfig = getVariantConfig(variant);

  // Updates selection state and notifies the parent with the bar's value
  const selectBar = useCallback(
    (barName: string | null, value?: number) => {
      setSelectedBar(barName);
      onSelectionChange?.(barName === null ? null : { dataKey: barName, value: value ?? 0 });
    },
    [onSelectionChange],
  );

  // Real bars paint from a per-name gradient; the skeleton keeps the raw rows
  const preparedData = useMemo(
    () =>
      data.map((item) => ({
        ...item,
        fill: `url(#${chartId}-radial-colors-${item[nameKey] as string})`,
      })),
    [data, nameKey, chartId],
  );

  const contextValue = useMemo<RadialChartContextValue>(
    () => ({
      config,
      nameKey,
      chartId,
      isLoading,
      selectedBar,
      selectBar,
    }),
    [config, nameKey, chartId, isLoading, selectedBar, selectBar],
  );

  return (
    <RadialChartContext value={contextValue}>
      <ChartContainer className={className} config={config}>
        <LoadingIndicator isLoading={isLoading} />
        <RechartsRadialBarChart
          id={chartId}
          data={isLoading ? loadingData : preparedData}
          innerRadius={innerRadius}
          outerRadius={outerRadius}
          startAngle={variantConfig.startAngle}
          endAngle={variantConfig.endAngle}
          cx={variantConfig.cx}
          cy={variantConfig.cy}
          {...chartProps}
        >
          {backgroundVariant && <ChartBackground variant={backgroundVariant} />}
          {children}
          {isLoading && <LoadingRadialBar />}
          <defs>
            <ColorGradientStyle config={config} chartId={chartId} />
          </defs>
        </RechartsRadialBarChart>
      </ChartContainer>
    </RadialChartContext>
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Composible parts
// ─────────────────────────────────────────────────────────────────────────────

type RadialBarProps = {
  dataKey: string; // value key — determines each bar's size
  cornerRadius?: number; // border radius of each bar's corners
  barSize?: number; // thickness of each radial bar
  showBackground?: boolean; // renders the unfilled track behind each bar
  isClickable?: boolean; // lets bars be selected by clicking them
  glowingBars?: string[]; // names of bars rendered with a soft glow
  radialBarProps?: Omit<RadialBarRechartsProps, "dataKey">; // escape hatch for raw Recharts RadialBar props
};

/**
 * The radial bar series. Each data row becomes one bar. The series generates
 * its own glow filter definitions under the chart's unique id, so glow effects
 * never collide with other charts on the page. Pass `glowingBars` to highlight
 * specific bars and `isClickable` to make bars selectable.
 */
export function RadialBar({
  dataKey,
  cornerRadius = DEFAULT_CORNER_RADIUS,
  barSize = DEFAULT_BAR_SIZE,
  showBackground = true,
  isClickable = false,
  glowingBars = EMPTY_GLOWING_BARS,
  radialBarProps,
}: RadialBarProps) {
  const { nameKey, chartId, isLoading, selectedBar, selectBar } = useRadialChart();

  // The root renders the skeleton bar while loading, so the real bar steps aside
  if (isLoading) return null;

  return (
    <>
      <RechartsRadialBar
        dataKey={dataKey as TypedDataKey<Record<string, unknown>>}
        cornerRadius={cornerRadius}
        barSize={barSize}
        background={showBackground}
        className="drop-shadow-sm"
        style={isClickable ? { cursor: "pointer" } : undefined}
        onClick={(payload, index) => {
          if (!isClickable) return;
          const entry = payload as Record<string, unknown>;
          const barName = (entry?.[nameKey] as string | undefined) ?? String(index);
          const value = Number(entry?.[dataKey] ?? 0);
          // Clicking the selected bar clears the selection, otherwise selects it
          selectBar(selectedBar === barName ? null : barName, value);
        }}
        shape={(props: SectorProps) => {
          const barName = (props as unknown as Record<string, unknown>)[nameKey] as string;
          const isGlowing = glowingBars.includes(barName);
          const isSelected = selectedBar === null || selectedBar === barName;

          return (
            <Sector
              {...props}
              filter={isGlowing ? `url(#${chartId}-radial-glow-${barName})` : undefined}
              opacity={isClickable && !isSelected ? 0.3 : 1}
              className="transition-opacity duration-200"
            />
          );
        }}
        {...radialBarProps}
      />
      <defs>
        {glowingBars.length > 0 && <GlowFilterStyle chartId={chartId} glowingBars={glowingBars} />}
      </defs>
    </>
  );
}

type TooltipProps = {
  variant?: TooltipVariant; // visual style of the tooltip surface
  roundness?: TooltipRoundness; // border-radius of the tooltip
  defaultIndex?: number; // data index shown by default with no hover
};

/**
 * The hover tooltip. Labels each bar by its name from context. Hidden
 * automatically while the chart is loading.
 */
export function Tooltip({ variant, roundness, defaultIndex }: TooltipProps) {
  const { nameKey, isLoading } = useRadialChart();

  if (isLoading) return null;

  return (
    <ChartTooltip
      defaultIndex={defaultIndex}
      cursor={false}
      content={
        <ChartTooltipContent
          nameKey={nameKey}
          hideLabel
          roundness={roundness}
          variant={variant}
        />
      }
    />
  );
}

type LegendProps = {
  variant?: ChartLegendVariant; // visual style of the legend indicators
  align?: "left" | "center" | "right"; // horizontal placement
  verticalAlign?: "top" | "middle" | "bottom"; // vertical placement
  isClickable?: boolean; // lets each entry toggle selection of its bar
};

/**
 * The bar legend. When `isClickable` is set, each entry toggles selection of
 * its bar, driving the shared selection state read by <RadialBar />. Hidden
 * automatically while the chart is loading.
 */
export function Legend({
  variant,
  align = "center",
  verticalAlign = "bottom",
  isClickable = false,
}: LegendProps) {
  const { nameKey, isLoading, selectedBar, selectBar } = useRadialChart();

  if (isLoading) return null;

  return (
    <ChartLegend
      verticalAlign={verticalAlign}
      align={align}
      content={
        <ChartLegendContent
          selected={selectedBar}
          onSelectChange={selectBar}
          isClickable={isClickable}
          nameKey={nameKey}
          variant={variant}
        />
      }
    />
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Variant helpers
// ─────────────────────────────────────────────────────────────────────────────

// Returns the angle + center configuration for the chart's arc shape
function getVariantConfig(variant: RadialVariant) {
  switch (variant) {
    case "semi":
      return { startAngle: 180, endAngle: 0, cx: "50%", cy: "70%" };
    case "full":
    default:
      return { startAngle: 90, endAngle: -270, cx: "50%", cy: "50%" };
  }
}

// ─────────────────────────────────────────────────────────────────────────────
// Style definitions — scoped to the chart's unique id
// ─────────────────────────────────────────────────────────────────────────────

/** Diagonal color gradient applied to every radial bar, one per config key. */
const ColorGradientStyle = ({
  config,
  chartId,
}: {
  config: ChartConfig;
  chartId: string;
}) => {
  return (
    <>
      {Object.entries(config).map(([dataKey, colorConfig]) => {
        const colorsCount = getColorsCount(colorConfig);

        return (
          <linearGradient
            key={`${chartId}-radial-colors-${dataKey}`}
            id={`${chartId}-radial-colors-${dataKey}`}
            x1="0"
            y1="0"
            x2="1"
            y2="1"
          >
            {colorsCount === 1 ? (
              <>
                <stop offset="0%" stopColor={`var(--color-${dataKey}-0)`} />
                <stop offset="100%" stopColor={`var(--color-${dataKey}-0)`} />
              </>
            ) : (
              Array.from({ length: colorsCount }, (_, index) => {
                const offset = `${(index / (colorsCount - 1)) * 100}%`;
                return (
                  <stop
                    key={offset}
                    offset={offset}
                    stopColor={`var(--color-${dataKey}-${index}, var(--color-${dataKey}-0))`}
                  />
                );
              })
            )}
          </linearGradient>
        );
      })}
    </>
  );
};

/** Soft outer-glow SVG filter, one per glowing bar. */
const GlowFilterStyle = ({
  chartId,
  glowingBars,
}: {
  chartId: string;
  glowingBars: string[];
}) => {
  return (
    <>
      {glowingBars.map((barName) => (
        <filter
          key={`${chartId}-radial-glow-${barName}`}
          id={`${chartId}-radial-glow-${barName}`}
          x="-100%"
          y="-100%"
          width="300%"
          height="300%"
        >
          <feGaussianBlur in="SourceGraphic" stdDeviation="6" result="blur" />
          <feColorMatrix
            in="blur"
            type="matrix"
            values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 0.6 0"
            result="glow"
          />
          <feMerge>
            <feMergeNode in="glow" />
            <feMergeNode in="SourceGraphic" />
          </feMerge>
        </filter>
      ))}
    </>
  );
};

// ─────────────────────────────────────────────────────────────────────────────
// Loading skeleton
// ─────────────────────────────────────────────────────────────────────────────

// Builds random skeleton rows with values between 40 and 100
function generateLoadingData() {
  return Array.from({ length: LOADING_BARS }, (_, i) => ({
    name: `loading${i}`,
    value: 40 + Math.random() * 60,
  }));
}

// Hook to animate the loading skeleton data at fixed intervals
function useLoadingData(isLoading: boolean) {
  const [tick, setTick] = useState(0);

  useEffect(() => {
    if (!isLoading) return;

    const interval = setInterval(() => {
      setTick((prev) => prev + 1);
    }, LOADING_ANIMATION_DURATION);

    return () => clearInterval(interval);
  }, [isLoading]);

  // Regenerate skeleton data whenever the interval ticks
  // eslint-disable-next-line react-hooks/exhaustive-deps
  const loadingData = useMemo(() => generateLoadingData(), [tick]);

  return loadingData;
}

/**
 * The skeleton bar shown while the chart is loading. Rendered by the root in
 * place of the real <RadialBar />, with animated values and a muted fill.
 */
const LoadingRadialBar = () => {
  return (
    <RechartsRadialBar
      dataKey="value"
      cornerRadius={DEFAULT_CORNER_RADIUS}
      barSize={DEFAULT_BAR_SIZE}
      background
      isAnimationActive
      animationDuration={LOADING_ANIMATION_DURATION}
      animationEasing="ease-in-out"
      shape={(props: SectorProps) => <Sector {...props} fill="currentColor" fillOpacity={0.25} />}
    />
  );
};
```
        
      
       
        #### Now, Let's add the chart component to your project.

        
          These Components are required by the chart component to render the chart. Make a folder called `ui` inside the `evilcharts` folder and paste the code there.

          Below is main chart component.
        
        
          #### Component Implementation

##### Source File: `chart.tsx`

```tsx
"use client";

import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

// Format: { THEME_NAME: CSS_SELECTOR }
const THEMES = { light: "", dark: ".dark" } as const;

type ThemeKey = keyof typeof THEMES;

// All Keys are optional at first
type ThemeColorsBase = {
  [K in ThemeKey]?: string[];
};

// Require at least one theme key
type AtLeastOneThemeColor = {
  [K in ThemeKey]: Required<Pick<ThemeColorsBase, K>> & Partial<Omit<ThemeColorsBase, K>>;
}[ThemeKey];

const VALID_THEME_KEYS = Object.keys(THEMES) as ThemeKey[];

// Validation for chart config colors at runtime
function validateChartConfigColors(config: ChartConfig): void {
  for (const [key, value] of Object.entries(config)) {
    if (value.colors) {
      const hasValidThemeKey = VALID_THEME_KEYS.some(
        (themeKey) => value.colors?.[themeKey] !== undefined,
      );

      if (!hasValidThemeKey) {
        throw new Error(
          `[EvilCharts] Invalid chart config for "${key}": colors object must have at least one theme key (${VALID_THEME_KEYS.join(", ")}). Received empty object or invalid keys.`,
        );
      }
    }
  }
}

export type ChartConfig = Record<
  string,
  {
    label?: React.ReactNode;
    icon?: React.ComponentType;
    colors?: AtLeastOneThemeColor;
  }
>;

interface ChartContextProps {
  config: ChartConfig;
}

const ChartContext = React.createContext<ChartContextProps | null>(null);

export function useChart() {
  const context = React.useContext(ChartContext);

  if (!context) {
    throw new Error("useChart must be used within a <ChartContainer />");
  }

  return context;
}

interface ChartContainerProps
  extends
    Omit<React.ComponentProps<"div">, "children">,
    Pick<
      React.ComponentProps<typeof RechartsPrimitive.ResponsiveContainer>,
      | "initialDimension"
      | "aspect"
      | "debounce"
      | "minHeight"
      | "minWidth"
      | "maxHeight"
      | "height"
      | "width"
      | "onResize"
      | "children"
    > {
  config: ChartConfig;
  innerResponsiveContainerStyle?: React.ComponentProps<
    typeof RechartsPrimitive.ResponsiveContainer
  >["style"];
  /** Optional content rendered below the chart (e.g. EvilBrush) */
  footer?: React.ReactNode;
}

function ChartContainer({
  id,
  config,
  initialDimension = { width: 320, height: 200 },
  className,
  children,
  footer,
  ...props
}: Readonly<ChartContainerProps>) {
  const uniqueId = React.useId();
  const chartId = `chart-${id ?? uniqueId.replace(/:/g, "")}`;

  // Validate chart config at runtime
  validateChartConfigColors(config);

  return (
    <ChartContext.Provider value={{ config }}>
      <div
        data-slot="chart"
        data-chart={chartId}
        className={cn(
          "min-h-0 w-full flex-1",
          "[&_.recharts-cartesian-axis-tick_text]:fill-muted-foreground [&_.recharts-cartesian-grid_line[stroke='#ccc']]:stroke-border/50 [&_.recharts-curve.recharts-tooltip-cursor]:stroke-border [&_.recharts-polar-grid_[stroke='#ccc']]:stroke-border [&_.recharts-radial-bar-background-sector]:fill-muted [&_.recharts-rectangle.recharts-tooltip-cursor]:fill-muted [&_.recharts-reference-line_[stroke='#ccc']]:stroke-border relative flex flex-col justify-center text-xs [&_.recharts-dot[stroke='#fff']]:stroke-transparent [&_.recharts-layer]:outline-hidden [&_.recharts-sector]:outline-hidden [&_.recharts-sector[stroke='#fff']]:stroke-transparent [&_.recharts-surface]:outline-hidden",
          !footer && "aspect-video",
          className,
        )}
        {...props}
      >
        <ChartStyle id={chartId} config={config} />
        <RechartsPrimitive.ResponsiveContainer
          className="min-h-0 w-full flex-1"
          initialDimension={initialDimension}
        >
          {children}
        </RechartsPrimitive.ResponsiveContainer>
        {footer}
      </div>
    </ChartContext.Provider>
  );
}

function LoadingIndicator({ isLoading }: { isLoading: boolean }) {
  if (!isLoading) {
    return null;
  }

  return (
    <div className="pointer-events-none absolute inset-0 z-20 flex items-center justify-center">
      <div className="text-primary bg-background flex items-center justify-center gap-2 rounded-md border px-2 py-0.5 text-sm">
        <div className="border-border border-t-primary h-3 w-3 animate-spin rounded-full border" />
        <span>Loading</span>
      </div>
    </div>
  );
}

// Distribute colors evenly across slots, extra slots go to last color(s)
// Example: 2 colors for 4 slots → [red, red, pink, pink]
// Example: 3 colors for 4 slots → [red, pink, blue, blue]
function distributeColors(colorsArray: string[], maxCount: number): string[] {
  const availableCount = colorsArray.length;
  if (availableCount >= maxCount) {
    return colorsArray.slice(0, maxCount);
  }

  const result: string[] = [];
  const baseSlots = Math.floor(maxCount / availableCount);
  const extraSlots = maxCount % availableCount;

  // First (availableCount - extraSlots) colors get baseSlots each
  // Last extraSlots colors get (baseSlots + 1) each
  for (let colorIdx = 0; colorIdx < availableCount; colorIdx++) {
    const isExtraColor = colorIdx >= availableCount - extraSlots;
    const slotsForThisColor = baseSlots + (isExtraColor ? 1 : 0);
    for (let j = 0; j < slotsForThisColor; j++) {
      result.push(colorsArray[colorIdx]);
    }
  }

  return result;
}

const ChartStyle = ({ id, config }: { id: string; config: ChartConfig }) => {
  const colorConfig = Object.entries(config).filter(([, config]) => config.colors);

  if (!colorConfig.length) {
    return null;
  }

  const generateCssVars = (theme: keyof typeof THEMES) =>
    colorConfig
      .flatMap(([key, itemConfig]) => {
        const colorsArray = itemConfig.colors?.[theme];
        if (!colorsArray || !Array.isArray(colorsArray) || colorsArray.length === 0) {
          return [];
        }

        // Get max count across all themes for this key
        const maxCount = getColorsCount(itemConfig);

        // Distribute colors evenly across all required slots
        const distributedColors = distributeColors(colorsArray, maxCount);

        return distributedColors.map((color, index) => `  --color-${key}-${index}: ${color};`);
      })
      .filter(Boolean)
      .join("\n");

  const css = Object.entries(THEMES)
    .map(
      ([theme, prefix]) =>
        `${prefix} [data-chart=${id}] {\n${generateCssVars(theme as keyof typeof THEMES)}\n}`,
    )
    .join("\n");

  return <style dangerouslySetInnerHTML={{ __html: css }} />;
};

// Helper to extract item config from a payload.
export function getPayloadConfigFromPayload(config: ChartConfig, payload: unknown, key: string) {
  if (typeof payload !== "object" || payload === null) {
    return undefined;
  }

  const payloadPayload =
    "payload" in payload && typeof payload.payload === "object" && payload.payload !== null
      ? payload.payload
      : undefined;

  let configLabelKey: string = key;

  if (key in payload && typeof payload[key as keyof typeof payload] === "string") {
    configLabelKey = payload[key as keyof typeof payload] as string;
  } else if (
    payloadPayload &&
    key in payloadPayload &&
    typeof payloadPayload[key as keyof typeof payloadPayload] === "string"
  ) {
    configLabelKey = payloadPayload[key as keyof typeof payloadPayload] as string;
  }

  return configLabelKey in config ? config[configLabelKey] : config[key];
}

// Format values to percent for expanded charts
function axisValueToPercentFormatter(value: number) {
  return `${Math.round(value * 100).toFixed(0)}%`;
}

// Get max colors count across all themes for a config entry
function getColorsCount(config: ChartConfig[string]): number {
  if (!config.colors) return 1;
  const counts = VALID_THEME_KEYS.map((theme) => config.colors?.[theme]?.length ?? 0);
  return Math.max(...counts, 1);
}

// Generate random loading data for skeleton/loading state
// min/max represent percentage of the range (0-100), defaults to 20-80 for realistic look
export const getLoadingData = (points: number = 10, min: number = 0, max: number = 70) => {
  const range = max - min;
  return Array.from({ length: points }, () => ({
    loading: Math.floor(Math.random() * range) + min,
  }));
};

export {
  ChartContainer,
  ChartStyle,
  axisValueToPercentFormatter,
  LoadingIndicator,
  getColorsCount,
};
```
        
      
       
        #### Now, We need to add sub components.

        
          Create a file called `tooltip.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `tooltip.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import type { NameType, ValueType } from "recharts/types/component/DefaultTooltipContent";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type TooltipRoundness = "sm" | "md" | "lg" | "xl";
type TooltipVariant = "default" | "frosted-glass";

const roundnessMap: Record<TooltipRoundness, string> = {
  sm: "rounded-sm",
  md: "rounded-md",
  lg: "rounded-lg",
  xl: "rounded-xl",
};

const variantMap: Record<TooltipVariant, string> = {
  default: "bg-background",
  "frosted-glass": "bg-background/70 backdrop-blur-sm",
};

function ChartTooltipContent({
  active,
  payload,
  className,
  indicator = "dot",
  hideLabel = false,
  hideIndicator = false,
  label,
  labelFormatter,
  labelClassName,
  formatter,
  nameKey,
  labelKey,
  selected,
  roundness = "lg",
  variant = "default",
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip> &
  React.ComponentProps<"div"> & {
    hideLabel?: boolean;
    hideIndicator?: boolean;
    indicator?: "line" | "dot" | "dashed";
    nameKey?: string;
    labelKey?: string;
    selected?: string | null;
    roundness?: TooltipRoundness;
    variant?: TooltipVariant;
  } & Omit<
    RechartsPrimitive.DefaultTooltipContentProps<ValueType, NameType>,
    "accessibilityLayer"
  >) {
  const { config } = useChart();

  const tooltipLabel = React.useMemo(() => {
    if (hideLabel || !payload?.length) {
      return null;
    }

    const [item] = payload;
    const key = `${labelKey ?? item?.dataKey ?? item?.name ?? "value"}`;
    const itemConfig = getPayloadConfigFromPayload(config, item, key);
    const value =
      !labelKey && typeof label === "string" ? (config[label]?.label ?? label) : itemConfig?.label;

    if (labelFormatter) {
      return (
        <div className={cn("font-medium", labelClassName)}>{labelFormatter(value, payload)}</div>
      );
    }

    if (!value) {
      return null;
    }

    return <div className={cn("font-medium", labelClassName)}>{value}</div>;
  }, [label, labelFormatter, payload, hideLabel, labelClassName, config, labelKey]);

  if (!active || !payload?.length) {
    // Empty tooltip - to prevent position getting 0.0 so it doesnt animate tooltip every time from 0.0 origin
    return <span className="p-4" />;
  }

  const nestLabel = payload.length === 1 && indicator !== "dot";

  return (
    <div
      className={cn(
        "border-border/50 grid min-w-32 items-start gap-1.5 border px-2.5 py-1.5 text-xs shadow-xl",
        roundnessMap[roundness],
        variantMap[variant],
        className,
      )}
    >
      {!nestLabel ? tooltipLabel : null}
      <div className="grid gap-1.5">
        {payload
          .filter((item) => item.type !== "none")
          .map((item, index) => {
            // For pie charts, item.name contains the sector name (e.g., "chrome")
            // For radial charts, the name is in item.payload[nameKey]
            // For other charts, item.name or item.dataKey contains the series name
            const payloadName =
              nameKey && item.payload
                ? (item.payload as Record<string, unknown>)[nameKey]
                : undefined;
            const key = `${payloadName ?? item.name ?? item.dataKey ?? "value"}`;
            const itemConfig = getPayloadConfigFromPayload(config, item, key);

            // Get colors count for this item to determine gradient vs solid
            const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

            return (
              <div
                key={index}
                className={cn(
                  "[&>svg]:text-muted-foreground flex w-full flex-wrap items-stretch gap-2 [&>svg]:h-2.5 [&>svg]:w-2.5",
                  indicator === "dot" && "items-center",
                  selected != null && selected !== item.dataKey && "opacity-30",
                )}
              >
                {formatter && item?.value !== undefined && item.name ? (
                  formatter(item.value, item.name, item, index, item.payload)
                ) : (
                  <>
                    {itemConfig?.icon ? (
                      <itemConfig.icon />
                    ) : (
                      !hideIndicator && (
                        <div
                          className={cn("shrink-0 rounded-[2px]", {
                            "h-2.5 w-2.5": indicator === "dot",
                            "w-1": indicator === "line",
                            "w-0 border-[1.5px] border-dashed bg-transparent!":
                              indicator === "dashed",
                            "my-0.5": nestLabel && indicator === "dashed",
                          })}
                          style={getIndicatorColorStyle(key, colorsCount)}
                        />
                      )
                    )}
                    <div
                      className={cn(
                        "flex flex-1 justify-between gap-4 leading-none",
                        nestLabel ? "items-end" : "items-center",
                      )}
                    >
                      <div className="grid gap-1.5">
                        {nestLabel ? tooltipLabel : null}
                        <span className="text-muted-foreground">
                          {itemConfig?.label ?? item.name}
                        </span>
                      </div>
                      {item.value != null && (
                        <span className="text-foreground font-mono font-medium tabular-nums">
                          {typeof item.value === "number"
                            ? item.value.toLocaleString()
                            : String(item.value)}
                        </span>
                      )}
                    </div>
                  </>
                )}
              </div>
            );
          })}
      </div>
    </div>
  );
}

function getIndicatorColorStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { background: `var(--color-${dataKey}-0)` };
  }

  // Multiple colors: create linear gradient with evenly distributed stops
  const stops = Array.from({ length: colorsCount }, (_, index) => {
    const offset = (index / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${index}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

const ChartTooltip = ({
  animationDuration = 200,
  ...props
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip>) => (
  <RechartsPrimitive.Tooltip animationDuration={animationDuration} {...props} />
);

export { ChartTooltip, ChartTooltipContent };
export type { TooltipRoundness, TooltipVariant };
```
        
        
          Now, create another file called `legend.tsx` inside the `evilcharts/ui` folder and paste the code there.
        
        
          #### Component Implementation

##### Source File: `legend.tsx`

```tsx
import { getPayloadConfigFromPayload, getColorsCount, useChart } from "@/registry/ui/chart";
import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

type ChartLegendVariant =
  | "square"
  | "circle"
  | "circle-outline"
  | "rounded-square"
  | "rounded-square-outline"
  | "vertical-bar"
  | "horizontal-bar";

function ChartLegendContent({
  className,
  hideIcon = false,
  nameKey,
  payload,
  verticalAlign,
  align = "right",
  selected,
  onSelectChange,
  isClickable,
  variant = "rounded-square",
}: React.ComponentProps<"div"> & {
  hideIcon?: boolean;
  nameKey?: string;
  selected?: string | null;
  isClickable?: boolean;
  onSelectChange?: (selected: string | null) => void;
  variant?: ChartLegendVariant;
} & RechartsPrimitive.DefaultLegendContentProps) {
  const { config } = useChart();

  if (!payload?.length) {
    return null;
  }

  return (
    <div
      className={cn(
        "flex items-center gap-4 select-none",
        align === "left" && "justify-start",
        align === "center" && "justify-center",
        align === "right" && "justify-end",
        verticalAlign === "top" ? "pb-4" : "pt-4",
        className,
      )}
    >
      {payload
        .filter((item) => item.type !== "none")
        .map((item) => {
          // For pie charts, item.value contains the sector name (e.g., "chrome")
          // For radial charts, the name is in item.payload[nameKey]
          // For other charts, item.dataKey contains the series name (e.g., "desktop")
          const payloadName =
            nameKey && item.payload
              ? (item.payload as Record<string, unknown>)[nameKey]
              : undefined;
          const key = `${payloadName ?? item.value ?? item.dataKey ?? "value"}`;
          const itemConfig = getPayloadConfigFromPayload(config, item, key);
          const isSelected = selected === null || selected === key;

          // Get colors count for this item to determine gradient vs solid
          const colorsCount = itemConfig ? getColorsCount(itemConfig) : 1;

          return (
            <div
              key={key}
              className={cn(
                "[&>svg]:text-muted-foreground flex items-center gap-1.5 transition-opacity [&>svg]:h-3 [&>svg]:w-3",
                !isSelected && "opacity-30",
                isClickable && "cursor-pointer",
              )}
              onClick={() => {
                if (!isClickable) return;

                onSelectChange?.(selected === key ? null : key);
              }}
            >
              {itemConfig?.icon && !hideIcon ? (
                <itemConfig.icon />
              ) : (
                <LegendIndicator
                  variant={variant}
                  dataKey={key}
                  colorsCount={colorsCount}
                />
              )}
              {itemConfig?.label}
            </div>
          );
        })}
    </div>
  );
}

// ---------------------------------------------------------------------------
// Legend indicator — each variant gets its own branch so future variants
// can diverge freely in markup & style.
// ---------------------------------------------------------------------------

function LegendIndicator({
  variant,
  dataKey,
  colorsCount,
}: {
  variant: ChartLegendVariant;
  dataKey: string;
  colorsCount: number;
}) {
  const fillStyle = getLegendFillStyle(dataKey, colorsCount);
  const outlineStyle = getLegendOutlineStyle(dataKey, colorsCount);

  switch (variant) {
    case "square":
      return <div className="h-2 w-2 shrink-0" style={fillStyle} />;

    case "circle":
      return <div className="h-2 w-2 shrink-0 rounded-full" style={fillStyle} />;

    case "circle-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-full p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "vertical-bar":
      return <div className="h-3 w-1 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "horizontal-bar":
      return <div className="h-1 w-3 shrink-0 rounded-[2px]" style={fillStyle} />;

    case "rounded-square-outline":
      return (
        <div
          className="h-2.5 w-2.5 shrink-0 rounded-[3px] p-[1.5px]"
          style={outlineStyle}
        />
      );

    case "rounded-square":
    default:
      return <div className="h-2 w-2 shrink-0 rounded-[2px]" style={fillStyle} />;
  }
}

// ---------------------------------------------------------------------------
// Style helpers
// ---------------------------------------------------------------------------

/** Solid fill / gradient background for filled variants. */
function getLegendFillStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  if (colorsCount <= 1) {
    return { backgroundColor: `var(--color-${dataKey}-0)` };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return { background: `linear-gradient(to right, ${stops})` };
}

/**
 * Outline style for stroke variants.
 * Uses background + mask-composite to punch out the center, leaving only the
 * "border" visible. Works with both solid colors and gradients, and respects
 * border-radius — unlike plain `border-color`.
 */
function getLegendOutlineStyle(dataKey: string, colorsCount: number): React.CSSProperties {
  const maskStyle: React.CSSProperties = {
    WebkitMask:
      "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    WebkitMaskComposite: "xor",
    mask: "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
    maskComposite: "exclude",
  };

  if (colorsCount <= 1) {
    return {
      backgroundColor: `var(--color-${dataKey}-0)`,
      ...maskStyle,
    };
  }

  const stops = Array.from({ length: colorsCount }, (_, i) => {
    const offset = (i / (colorsCount - 1)) * 100;
    return `var(--color-${dataKey}-${i}) ${offset}%`;
  }).join(", ");

  return {
    background: `linear-gradient(to right, ${stops})`,
    ...maskStyle,
  };
}

const ChartLegend = RechartsPrimitive.Legend;

export { ChartLegend, ChartLegendContent, type ChartLegendVariant };
```
        
      
    
  


## Usage

The radial chart is composible. `<EvilRadialChart>` is the container, and you compose only the parts you need — `<Legend>`, `<Tooltip>`, and a `<RadialBar>` — as its children. The `<RadialBar>` carries its own `glowingBars` and `isClickable`, so styling and interactivity live with the series itself.

```tsx
import {
  EvilRadialChart,
  RadialBar,
  Tooltip,
  Legend,
} from "@/components/evilcharts/charts/radial-chart";
```

```tsx
const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: { light: ["#3b82f6"], dark: ["#60a5fa"] },
  },
  safari: {
    label: "Safari",
    colors: { light: ["#10b981"], dark: ["#34d399"] },
  },
  firefox: {
    label: "Firefox",
    colors: { light: ["#f59e0b"], dark: ["#fbbf24"] },
  },
} satisfies ChartConfig;
```

```tsx
<EvilRadialChart data={data} nameKey="browser" config={chartConfig} variant="full">
  <Legend isClickable />
  <Tooltip />
  <RadialBar dataKey="visitors" isClickable glowingBars={["chrome"]} />
</EvilRadialChart>
```

### Interactive Selection

Add `isClickable` to `<RadialBar>` (and to `<Legend>`) to make bars selectable. Use the `onSelectionChange` callback on `<EvilRadialChart>` to handle selection events:

```tsx
<EvilRadialChart
  data={data}
  nameKey="browser"
  config={chartConfig}
  onSelectionChange={(selection) => {
    if (selection) {
      console.log("Selected:", selection.dataKey, "Value:", selection.value);
    } else {
      console.log("Deselected");
    }
  }}
>
  <Legend isClickable />
  <Tooltip />
  <RadialBar dataKey="visitors" isClickable />
</EvilRadialChart>
```

### Loading State

#### Live Example Code

##### Example File: `ex-loading-state-radial-chart.tsx`

```tsx
"use client";

import { EvilRadialChart, RadialBar, Tooltip, Legend } from "@/registry/charts/radial-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
  { browser: "edge", visitors: 173 },
  { browser: "other", visitors: 90 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  safari: {
    label: "Safari",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
  firefox: {
    label: "Firefox",
    colors: {
      light: ["#f59e0b"],
      dark: ["#fbbf24"],
    },
  },
  edge: {
    label: "Edge",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  other: {
    label: "Other",
    colors: {
      light: ["#6b7280"],
      dark: ["#9ca3af"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleRadialChart() {
  return (
    <EvilRadialChart
      className="h-full w-full p-4"
      data={data}
      nameKey="browser"
      config={chartConfig}
      isLoading // [!code highlight]
    >
      <Legend />
      <Tooltip />
      <RadialBar dataKey="visitors" />
    </EvilRadialChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    The radial chart supports loading state with a placeholder animation. You can pass the `isLoading` prop to the chart to show the loading state while your data is being fetched.
  


## Examples

Below are some examples of the radial chart with different configurations. You can customize the `variant`, `innerRadius`, `outerRadius`, and other properties.

### Semi-Circle Variant

#### Live Example Code

##### Example File: `ex-semi-variant-radial-chart.tsx`

```tsx
"use client";

import { EvilRadialChart, RadialBar, Tooltip, Legend } from "@/registry/charts/radial-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
  { browser: "edge", visitors: 173 },
  { browser: "other", visitors: 90 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  safari: {
    label: "Safari",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
  firefox: {
    label: "Firefox",
    colors: {
      light: ["#f59e0b"],
      dark: ["#fbbf24"],
    },
  },
  edge: {
    label: "Edge",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  other: {
    label: "Other",
    colors: {
      light: ["#6b7280"],
      dark: ["#9ca3af"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleRadialChart() {
  return (
    <EvilRadialChart
      className="h-full w-full p-4"
      data={data}
      nameKey="browser"
      config={chartConfig}
      variant="semi" // [!code highlight]
    >
      <Legend />
      <Tooltip />
      <RadialBar dataKey="visitors" />
    </EvilRadialChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    Set `variant="semi"` to create a half-circle radial chart. This is useful for displaying progress or gauges in a more compact space.
  


### Gradient Colors

#### Live Example Code

##### Example File: `ex-gradient-colors-radial-chart.tsx`

```tsx
"use client";

import { EvilRadialChart, RadialBar, Tooltip, Legend } from "@/registry/charts/radial-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
  { browser: "edge", visitors: 173 },
  { browser: "other", visitors: 90 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: {
      light: ["#ff6b6b", "#feca57", "#48dbfb"], // Coral -> Gold -> Electric Blue // [!code highlight]
      dark: ["#ff7979", "#ffeaa7", "#74b9ff"], // [!code highlight]
    },
  },
  safari: {
    label: "Safari",
    colors: {
      light: ["#a29bfe", "#fd79a8", "#fdcb6e"], // Lavender -> Pink -> Sunflower // [!code highlight]
      dark: ["#b8b5ff", "#ff9ff3", "#ffeaa7"], // [!code highlight]
    },
  },
  firefox: {
    label: "Firefox",
    colors: {
      light: ["#00d2d3", "#54a0ff", "#5f27cd"], // Turquoise -> Blue -> Purple // [!code highlight]
      dark: ["#01e2e3", "#74b9ff", "#7c3aed"], // [!code highlight]
    },
  },
  edge: {
    label: "Edge",
    colors: {
      light: ["#ff9f43", "#ee5a24", "#b71540"], // Tangerine -> Vermillion -> Wine // [!code highlight]
      dark: ["#ffbe76", "#f0932b", "#e74c3c"], // [!code highlight]
    },
  },
  other: {
    label: "Other",
    colors: {
      light: ["#1dd1a1", "#10ac84", "#01a3a4"], // Mint -> Jungle -> Teal // [!code highlight]
      dark: ["#55efc4", "#00b894", "#00cec9"], // [!code highlight]
    },
  },
} satisfies ChartConfig;

export function EvilExampleRadialChart() {
  return (
    <EvilRadialChart className="h-full w-full p-4" data={data} nameKey="browser" config={chartConfig}>
      <Legend />
      <Tooltip />
      <RadialBar dataKey="visitors" />
    </EvilRadialChart>
  );
}
```

### Glowing Bars

#### Live Example Code

##### Example File: `ex-glowing-radial-chart.tsx`

```tsx
"use client";

import { EvilRadialChart, RadialBar, Tooltip, Legend } from "@/registry/charts/radial-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { browser: "chrome", visitors: 275 },
  { browser: "safari", visitors: 200 },
  { browser: "firefox", visitors: 187 },
  { browser: "edge", visitors: 173 },
  { browser: "other", visitors: 90 },
];

const chartConfig = {
  chrome: {
    label: "Chrome",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  safari: {
    label: "Safari",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
  firefox: {
    label: "Firefox",
    colors: {
      light: ["#f59e0b"],
      dark: ["#fbbf24"],
    },
  },
  edge: {
    label: "Edge",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  other: {
    label: "Other",
    colors: {
      light: ["#6b7280"],
      dark: ["#9ca3af"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleRadialChart() {
  return (
    <EvilRadialChart className="h-full w-full p-4" data={data} nameKey="browser" config={chartConfig}>
      <Legend />
      <Tooltip />
      <RadialBar
        dataKey="visitors"
        glowingBars={["chrome", "safari"]} // [!code highlight]
      />
    </EvilRadialChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    Add a subtle glow effect to specific bars using the `glowingBars` prop on `<RadialBar>`. Pass an array of bar names (the values from your `nameKey` field) to specify which bars should glow.
  



## API Reference

The chart is composed of several parts. The props below are grouped by the component they belong to.

### EvilRadialChart


The root container. It owns the data, the shared selection state, the loading skeleton, and the chart-wide arc shape. Everything visual is composed as its children.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **data*** | `TData[]` | - | Data used to display the chart. An array of objects where each object represents a radial bar (`TData extends Record`). |
| **config*** | `ChartConfig` | - | Configuration object that defines the chart's bars. Each key should match a value from your `nameKey` field, with corresponding colors. |
| **nameKey*** | `keyof TData & string` | - | The key from your data objects to use for bar names (typically string values used for labels and the legend). |
| **children*** | `ReactNode` | - | The composed chart parts — ``, ``, and a ``. |
| **className** | `string` | - | Additional CSS classes to apply to the chart container. |
| **variant** | - | - | The arc shape of the radial chart. `"full"` displays a full circle (360°), `"semi"` displays a half circle (180°). |
| **innerRadius** | `number | string` | - | The inner radius of the radial bars. Can be a number (pixels) or percentage string. |
| **outerRadius** | `number | string` | - | The outer radius of the radial bars. Can be a number (pixels) or percentage string. |
| **defaultSelectedDataKey** | `string | null` | `null` | The bar name that should be selected by default. |
| **onSelectionChange** | - | - | void">     Callback fired when a bar is selected or deselected — by clicking a clickable `` or `` entry. Receives an object with `dataKey` (bar name) and `value` (bar value), or `null` when deselected. |
| **isLoading** | `boolean` | `false` | Shows a loading placeholder animation when data is being fetched. |
| **backgroundVariant** | `BackgroundVariant` | - | Background pattern variant to display behind the chart. |
| **chartProps** | - | - | ">     Additional props forwarded to the underlying Recharts RadialBarChart component. Read the Recharts RadialBarChart documentation for available props. |



### RadialBar


The radial bar series. Each data row becomes one bar. It generates its own glow filter definitions, so glow effects never collide with other charts on the page.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **dataKey*** | `string` | - | The key from your data objects to use for bar values (typically numeric values that determine bar size). |
| **cornerRadius** | `number` | `5` | The border radius for the corners of each bar in pixels. |
| **barSize** | `number` | `14` | The thickness of each radial bar in pixels. |
| **showBackground** | `boolean` | `true` | Whether to render the background track (the unfilled portion of each bar). |
| **isClickable** | `boolean` | `false` | Enables interactive clicking on bars to select/deselect them. Unselected bars dim while a selection is active. |
| **glowingBars** | `string[]` | `[]` | Array of bar names (values from your `nameKey` field) that should have a glowing effect applied. Creates a smooth outer glow around the specified bars. |
| **radialBarProps** | - | - | , "dataKey">'>     Additional props forwarded to the underlying Recharts RadialBar component. Read the Recharts RadialBar documentation for available props. |



### Tooltip


The hover tooltip. Labels each bar by its name. Render it to show a tooltip, omit it for none.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | Controls the visual style of the tooltip. |
| **roundness** | - | - | Controls the border-radius of the tooltip. |
| **defaultIndex** | `number` | - | When set, the tooltip will be visible by default at the specified data point index. |



### Legend


The bar legend. When `isClickable` is set, each entry toggles selection of its bar. Render it to show a legend, omit it for none.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The visual style variant for the legend indicators. |
| **align** | - | - | Horizontal placement of the legend. |
| **verticalAlign** | - | - | Vertical placement of the legend. |
| **isClickable** | `boolean` | `false` | When enabled, each legend entry toggles selection of its bar, driving the shared selection state read by ``. |

---

## Sankey Chart <a name="sankey-chart"></a>

> Sankey charts for visualizing flow data with nodes and links, featuring gradient colors and glow effects

#### Live Example Code

##### Example File: `ex-sankey-chart.tsx`

```tsx
"use client";

import { EvilSankeyChart, Node, NodeLabel, Link, Tooltip } from "@/registry/charts/sankey-chart";
import type { SankeyData } from "recharts";
import { type ChartConfig } from "@/registry/ui/chart";

// Marketing funnel - user acquisition to conversions
const data: SankeyData = {
  nodes: [
    { name: "Organic" },
    { name: "PaidAds" },
    { name: "Social" },
    { name: "Landing" },
    { name: "Product" },
    { name: "Cart" },
    { name: "Purchase" },
    { name: "Bounced" },
  ],
  links: [
    { source: 0, target: 3, value: 42000 },
    { source: 1, target: 3, value: 28000 },
    { source: 2, target: 3, value: 18000 },
    { source: 3, target: 4, value: 52000 },
    { source: 3, target: 7, value: 36000 },
    { source: 4, target: 5, value: 31000 },
    { source: 4, target: 7, value: 21000 },
    { source: 5, target: 6, value: 24000 },
    { source: 5, target: 7, value: 7000 },
  ],
};

const chartConfig = {
  Organic: {
    label: "Organic Search",
    colors: {
      light: ["#059669"],
      dark: ["#34d399"],
    },
  },
  PaidAds: {
    label: "Paid Ads",
    colors: {
      light: ["#dc2626"],
      dark: ["#f87171"],
    },
  },
  Social: {
    label: "Social Media",
    colors: {
      light: ["#7c3aed"],
      dark: ["#a78bfa"],
    },
  },
  Landing: {
    label: "Landing Page",
    colors: {
      light: ["#0891b2"],
      dark: ["#22d3ee"],
    },
  },
  Product: {
    label: "Product Page",
    colors: {
      light: ["#2563eb"],
      dark: ["#60a5fa"],
    },
  },
  Cart: {
    label: "Cart",
    colors: {
      light: ["#ea580c"],
      dark: ["#fb923c"],
    },
  },
  Purchase: {
    label: "Purchase",
    colors: {
      light: ["#16a34a"],
      dark: ["#4ade80"],
    },
  },
  Bounced: {
    label: "Bounced",
    colors: {
      light: ["#f43f5e"],
      dark: ["#fb7185"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleSankeyChart() {
  return (
    <EvilSankeyChart className="h-full w-full p-4" data={data} config={chartConfig}>
      <Node isClickable>
        <NodeLabel position="outside" showValues />
      </Node>
      <Link variant="source" />
      <Tooltip />
    </EvilSankeyChart>
  );
}
```

## Installation


  
    
    
  
  
### CLI Installation


    ```bash
npx shadcn@latest add @evilcharts/sankey-chart
```
  
  
### Manual Installation


    
      
        #### Install the following dependencies:

        
          ```bash
npm install recharts motion
```
        
      
      
        #### Copy and paste the following code snippets into your project.

         
          To use the chart, first create the folder `evilcharts` and a subfolder called `charts` inside your `components` directory.
          Then, copy the following base sankey-chart code into a new file in that folder.
        
        
          #### Component Implementation

##### Source File: `sankey-chart.tsx`

```tsx
"use client";

import {
  type ChartConfig,
  ChartContainer,
  getColorsCount,
  LoadingIndicator,
} from "@/registry/ui/chart";
import {
  ChartTooltip,
  ChartTooltipContent,
  type TooltipRoundness,
  type TooltipVariant,
} from "@/registry/ui/tooltip";
import { ChartBackground, type BackgroundVariant } from "@/registry/ui/background";
import {
  Children,
  createContext,
  isValidElement,
  use,
  useCallback,
  useId,
  useMemo,
  useState,
  type FC,
  type ReactElement,
  type ReactNode,
} from "react";
import {
  Sankey as RechartsSankey,
  Layer,
  type SankeyProps,
  type SankeyNodeProps,
  type SankeyLinkProps,
  type SankeyData,
  type SankeyNode as RechartsSankeyNode,
} from "recharts";
import { motion } from "motion/react";

// Constants
const LOADING_ANIMATION_DURATION = 2000; // full loading cycle duration in milliseconds
const DEFAULT_NODE_WIDTH = 10;
const DEFAULT_NODE_PADDING = 10;
const DEFAULT_LINK_CURVATURE = 0.5;
const DEFAULT_ITERATIONS = 32;

type LinkVariant = "gradient" | "solid" | "source" | "target";
type NodeLabelPosition = "inside" | "outside";

// ─────────────────────────────────────────────────────────────────────────────
// Shared context
// ─────────────────────────────────────────────────────────────────────────────

/**
 * Shared state for every part of the chart. Lifted into <EvilSankeyChart /> so
 * that <Node />, <Link />, and <Tooltip /> can read it without prop drilling.
 * A sankey chart's data is rigid — the root passes `nodes`/`links` straight to
 * Recharts — so the parts here configure how those nodes and links render.
 */
type SankeyChartContextValue = {
  data: SankeyData; // the nodes + links rendered by the chart
  config: ChartConfig; // colors + labels keyed by node name
  chartId: string; // colon-free id scoping this chart's SVG defs
  isLoading: boolean; // whether the chart shows its loading skeleton
  selectedNode: string | null; // currently selected node name, or null when none
  selectNode: (nodeName: string | null) => void; // sets the selected node
};

const SankeyChartContext = createContext<SankeyChartContextValue | null>(null);

// Reads the chart context, throwing a helpful error when used outside <EvilSankeyChart />
function useSankeyChart() {
  const context = use(SankeyChartContext);

  if (!context) {
    throw new Error(
      "Sankey chart parts (<Node />, <Link />, <Tooltip />, …) must be used within <EvilSankeyChart />",
    );
  }

  return context;
}

// ─────────────────────────────────────────────────────────────────────────────
// Root container
// ─────────────────────────────────────────────────────────────────────────────

type EvilSankeyChartBaseProps = {
  data: SankeyData; // nodes + links rendered by the chart
  config: ChartConfig; // node colors + labels keyed by node name
  children: ReactNode; // composed parts — <Node />, <Link />, <Tooltip />, …
  className?: string; // extra classes for the chart container
  sankeyProps?: Omit<SankeyProps, "data">; // escape hatch for the raw Recharts Sankey
  nodeWidth?: number; // width of each node in pixels
  nodePadding?: number; // vertical gap between nodes in pixels
  linkCurvature?: number; // link curve amount, 0 (straight) to 1 (maximum)
  iterations?: number; // layout iterations — higher is more accurate
  sort?: boolean; // sorts nodes automatically for an optimal layout
  align?: "left" | "justify"; // horizontal node alignment strategy
  verticalAlign?: "justify" | "top"; // vertical node alignment strategy
  backgroundVariant?: BackgroundVariant; // background pattern behind the chart
  defaultSelectedNode?: string | null; // node selected on first render
  onSelectionChange?: (selection: { dataKey: string; value: number } | null) => void; // fires when the selected node changes
  isLoading?: boolean; // shows the animated loading skeleton
};

type EvilSankeyChartProps = EvilSankeyChartBaseProps;

/**
 * Root of the composible sankey chart. Owns the flow data, the shared context,
 * the layout configuration, and the loading skeleton. The visual parts — the
 * nodes, links, and tooltip — are composed as children, so a consumer renders
 * exactly the parts they need with the styling they want.
 */
export function EvilSankeyChart({
  data,
  config,
  children,
  className,
  sankeyProps,
  nodeWidth = DEFAULT_NODE_WIDTH,
  nodePadding = DEFAULT_NODE_PADDING,
  linkCurvature = DEFAULT_LINK_CURVATURE,
  iterations = DEFAULT_ITERATIONS,
  sort = true,
  align = "justify",
  verticalAlign = "justify",
  backgroundVariant,
  defaultSelectedNode = null,
  onSelectionChange,
  isLoading = false,
}: EvilSankeyChartProps) {
  const chartId = useId().replace(/:/g, ""); // colon-free id keeps CSS/SVG selectors valid
  const [selectedNode, setSelectedNode] = useState<string | null>(defaultSelectedNode);

  // Updates selection state and notifies the parent with the node's flow value
  const selectNode = useCallback(
    (nodeName: string | null) => {
      setSelectedNode(nodeName);

      if (!onSelectionChange) return;

      if (nodeName === null) {
        onSelectionChange(null);
        return;
      }

      onSelectionChange({ dataKey: nodeName, value: getNodeValue(data, nodeName) });
    },
    [onSelectionChange, data],
  );

  const contextValue = useMemo<SankeyChartContextValue>(
    () => ({ data, config, chartId, isLoading, selectedNode, selectNode }),
    [data, config, chartId, isLoading, selectedNode, selectNode],
  );

  return (
    <SankeyChartContext value={contextValue}>
      <ChartContainer className={className} config={config}>
        <LoadingIndicator isLoading={isLoading} />
        {backgroundVariant && <ChartBackground variant={backgroundVariant} />}
        {!isLoading && (
          <RechartsSankey
            id={chartId}
            data={data}
            nodeWidth={nodeWidth}
            nodePadding={nodePadding}
            linkCurvature={linkCurvature}
            iterations={iterations}
            sort={sort}
            align={align}
            verticalAlign={verticalAlign}
            {...resolveSankeyRenderers(children)}
            {...sankeyProps}
          >
            {children}
            <defs>
              <NodeColorGradients config={config} chartId={chartId} />
            </defs>
          </RechartsSankey>
        )}
        {isLoading && (
          <svg
            viewBox="0 0 500 250"
            preserveAspectRatio="xMidYMid meet"
            width="100%"
            height="100%"
            className="absolute inset-0"
          >
            <LoadingSankey />
          </svg>
        )}
      </ChartContainer>
    </SankeyChartContext>
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Composible parts
// ─────────────────────────────────────────────────────────────────────────────

type NodeProps = {
  radius?: number; // corner radius of node rectangles in pixels
  isClickable?: boolean; // lets nodes be selected by clicking them
  glow?: string[]; // node names that get a soft outer glow
  children?: ReactNode; // optional <NodeLabel /> composition
};

/**
 * Configures how the sankey nodes render. It is a configuration slot — the root
 * reads its props and wires them into the Recharts Sankey `node` renderer, so it
 * renders nothing itself. Compose a <NodeLabel /> inside it to show labels.
 */
export const Node: FC<NodeProps> = () => null;

type NodeLabelProps = {
  position?: NodeLabelPosition; // places labels inside or beside the nodes
  showValues?: boolean; // appends each node's total flow value
  valueFormatter?: (value: number) => string; // formats node values when shown
};

/**
 * Declares labels for the <Node /> it is composed inside. Like <Node />, it is a
 * configuration slot and renders nothing on its own.
 */
export const NodeLabel: FC<NodeLabelProps> = () => null;

type LinkProps = {
  variant?: LinkVariant; // coloring strategy for the link bands
  verticalPadding?: number; // shrinks link width where it meets a node
  glow?: number[]; // link indices that get a soft outer glow
};

/**
 * Configures how the sankey links render. Like <Node />, it is a configuration
 * slot read by the root and renders nothing itself. The `variant` controls how
 * each link band is colored.
 */
export const Link: FC<LinkProps> = () => null;

type TooltipProps = {
  variant?: TooltipVariant; // visual style of the tooltip surface
  roundness?: TooltipRoundness; // border-radius of the tooltip
  defaultIndex?: number; // data index shown by default with no hover
};

/**
 * The hover tooltip. Reads the chart's loading state from context and is hidden
 * automatically while the chart shows its skeleton.
 */
export function Tooltip({ variant, roundness, defaultIndex }: TooltipProps) {
  const { isLoading } = useSankeyChart();

  if (isLoading) return null;

  return (
    <ChartTooltip
      defaultIndex={defaultIndex}
      content={
        <ChartTooltipContent nameKey="name" hideLabel roundness={roundness} variant={variant} />
      }
    />
  );
}

// ─────────────────────────────────────────────────────────────────────────────
// Children resolution — turns composed <Node />/<Link /> into Sankey renderers
// ─────────────────────────────────────────────────────────────────────────────

// Sums a node's outgoing flow, falling back to incoming flow for leaf nodes
const getNodeValue = (data: SankeyData, nodeName: string): number => {
  const nodeIndex = data.nodes.findIndex((node) => node.name === nodeName);
  if (nodeIndex === -1) return 0;

  const outgoing = data.links
    .filter((link) => link.source === nodeIndex)
    .reduce((sum, link) => sum + link.value, 0);
  const incoming = data.links
    .filter((link) => link.target === nodeIndex)
    .reduce((sum, link) => sum + link.value, 0);

  return outgoing > 0 ? outgoing : incoming;
};

// Reads composed <Node /> and <Link /> children into the Sankey `node`/`link` render props
const resolveSankeyRenderers = (
  children: ReactNode,
): Pick<SankeyProps, "node" | "link"> => {
  let nodeProps: NodeProps | null = null;
  let linkProps: LinkProps | null = null;

  Children.forEach(children, (child) => {
    if (!isValidElement(child)) return;

    if (child.type === Node) {
      nodeProps = (child as ReactElement<NodeProps>).props;
    }

    if (child.type === Link) {
      linkProps = (child as ReactElement<LinkProps>).props;
    }
  });

  return {
    node: (props: SankeyNodeProps) => <SankeyNode {...props} nodeConfig={nodeProps} />,
    link: (props: SankeyLinkProps) => <SankeyLink {...props} linkConfig={linkProps} />,
  };
};

// Reads the <NodeLabel /> composed inside a <Node />, if any
const resolveNodeLabel = (children: ReactNode): NodeLabelProps | null => {
  let label: NodeLabelProps | null = null;

  Children.forEach(children, (child) => {
    if (isValidElement(child) && child.type === NodeLabel) {
      label = (child as ReactElement<NodeLabelProps>).props;
    }
  });

  return label;
};

// ─────────────────────────────────────────────────────────────────────────────
// Node renderer — draws a single sankey node from the resolved <Node /> config
// ─────────────────────────────────────────────────────────────────────────────

type SankeyNodeRendererProps = SankeyNodeProps & {
  nodeConfig: NodeProps | null; // resolved props from the composed <Node />
};

/**
 * Renders a single sankey node rectangle, plus its optional label and value.
 * The root passes one of these per node, configured from the composed <Node />.
 */
const SankeyNode = ({ x, y, width, height, payload, nodeConfig }: SankeyNodeRendererProps) => {
  const { config, chartId, data, selectedNode, selectNode } = useSankeyChart();

  const radius = nodeConfig?.radius ?? 0;
  const isClickable = nodeConfig?.isClickable ?? false;
  const glow = nodeConfig?.glow ?? [];
  const label = resolveNodeLabel(nodeConfig?.children);

  const nodeName = payload.name;
  const nodeValue = payload.value;
  const nodeIcon = (payload as RechartsSankeyNode & { icon?: ReactNode }).icon;

  const isHighlighted = isNodeConnected(data, selectedNode, nodeName);
  const isGlowing = glow.includes(nodeName);
  const hasConfigColor = nodeName in config;
  const configLabel = config[nodeName]?.label ?? nodeName;
  const dimmed = isClickable && !isHighlighted;

  const valueFormatter = label?.valueFormatter ?? ((value: number) => value.toLocaleString());
  const showValues = label?.showValues ?? false;

  const labelX = x + width / 2;
  const labelY = showValues ? y + height / 2 - 8 : y + height / 2;
  const valueY = y + height / 2 + 8;
  const outsideLabelX = x + width + 8;
  const outsideLabelY = y + height / 2;

  return (
    <Layer>
      <rect
        x={x}
        y={y}
        width={width}
        height={height}
        rx={radius}
        ry={radius}
        fill={hasConfigColor ? `url(#${chartId}-sankey-colors-${nodeName})` : "currentColor"}
        fillOpacity={dimmed ? 0.3 : 0.9}
        filter={isGlowing ? `url(#${chartId}-node-glow-${nodeName})` : undefined}
        className="transition-opacity duration-200"
        style={isClickable ? { cursor: "pointer" } : undefined}
        onClick={() => {
          if (!isClickable) return;
          selectNode(selectedNode === nodeName ? null : nodeName);
        }}
      />
      {isGlowing && (
        <defs>
          <GlowFilter chartId={chartId} name={nodeName} type="node" />
        </defs>
      )}
      {label?.position === "inside" && (
        <>
          <rect
            x={x + 1}
            y={y + 1}
            width={width - 2}
            height={height - 2}
            rx={Math.max(0, radius - 1)}
            ry={Math.max(0, radius - 1)}
            opacity={dimmed ? 0.3 : 1}
            className="fill-white/50 transition-opacity duration-200 dark:fill-black/60"
            style={{ pointerEvents: "none" }}
          />
          {nodeIcon && (
            <foreignObject
              x={labelX - 8}
              y={labelY - 30}
              width={16}
              height={16}
              opacity={dimmed ? 0.3 : 1}
              className="transition-opacity duration-200"
              style={{ pointerEvents: "none" }}
            >
              <div className="text-foreground/80 flex items-center justify-center dark:text-white/80">
                {nodeIcon}
              </div>
            </foreignObject>
          )}
          <text
            x={labelX}
            y={nodeIcon ? labelY - 4 : labelY}
            textAnchor="middle"
            dominantBaseline="middle"
            className="fill-foreground text-[10px] font-medium transition-opacity duration-200 dark:fill-white"
            opacity={dimmed ? 0.3 : 1}
            style={{ pointerEvents: "none" }}
          >
            {configLabel}
          </text>
          {showValues && (
            <text
              x={labelX}
              y={valueY}
              textAnchor="middle"
              dominantBaseline="middle"
              className="fill-foreground/60 font-mono text-xs font-medium tabular-nums transition-opacity duration-200 dark:fill-white"
              opacity={dimmed ? 0.3 : 0.6}
              style={{ pointerEvents: "none" }}
            >
              {valueFormatter(nodeValue)}
            </text>
          )}
        </>
      )}
      {label?.position === "outside" && (
        <>
          <text
            x={outsideLabelX}
            y={outsideLabelY - (showValues ? 8 : 0)}
            textAnchor="start"
            dominantBaseline="middle"
            className="fill-foreground text-xs"
            style={{ pointerEvents: "none" }}
          >
            {configLabel}
          </text>
          {showValues && (
            <text
              x={outsideLabelX}
              y={outsideLabelY + 8}
              textAnchor="start"
              dominantBaseline="middle"
              opacity={0.5}
              className="fill-foreground font-mono text-xs tabular-nums dark:fill-white"
              style={{ pointerEvents: "none" }}
            >
              {valueFormatter(nodeValue)}
            </text>
          )}
        </>
      )}
    </Layer>
  );
};

// ─────────────────────────────────────────────────────────────────────────────
// Link renderer — draws a single sankey link from the resolved <Link /> config
// ─────────────────────────────────────────────────────────────────────────────

type SankeyLinkRendererProps = SankeyLinkProps & {
  linkConfig: LinkProps | null; // resolved props from the composed <Link />
};

/**
 * Renders a single sankey link band, colored by the composed <Link /> variant.
 * Highlights the bands connected to the selected node and dims the rest.
 */
const SankeyLink = ({
  sourceX,
  targetX,
  sourceY,
  targetY,
  sourceControlX,
  targetControlX,
  linkWidth,
  index,
  payload,
  linkConfig,
}: SankeyLinkRendererProps) => {
  const { config, chartId, selectedNode } = useSankeyChart();

  const variant = linkConfig?.variant ?? "gradient";
  const verticalPadding = linkConfig?.verticalPadding ?? 0;
  const glow = linkConfig?.glow ?? [];

  const sourceName = payload.source.name;
  const targetName = payload.target.name;

  const isConnected =
    selectedNode === null || selectedNode === sourceName || selectedNode === targetName;
  const isGlowing = glow.includes(index);

  const paddedLinkWidth = Math.max(1, linkWidth - verticalPadding);
  const halfWidth = paddedLinkWidth / 2;

  const linkAreaPath = `M${sourceX},${sourceY - halfWidth}
    C${sourceControlX},${sourceY - halfWidth} ${targetControlX},${targetY - halfWidth} ${targetX},${targetY - halfWidth}
    L${targetX},${targetY + halfWidth}
    C${targetControlX},${targetY + halfWidth} ${sourceControlX},${sourceY + halfWidth} ${sourceX},${sourceY + halfWidth}
    Z`;

  return (
    <Layer>
      <defs>
        {variant === "gradient" && (
          <LinkGradient
            chartId={chartId}
            index={index}
            config={config}
            sourceName={sourceName}
            targetName={targetName}
          />
        )}
        <LinkStrokeGradient chartId={chartId} index={index} />
        {isGlowing && <GlowFilter chartId={chartId} name={String(index)} type="link" />}
      </defs>
      <path
        d={linkAreaPath}
        fill={getLinkFill(variant, chartId, index, config, sourceName, targetName)}
        fillOpacity={isConnected ? 0.4 : 0.1}
        stroke={
          selectedNode !== null && isConnected ? `url(#${chartId}-link-stroke-${index})` : "none"
        }
        strokeWidth={1}
        strokeOpacity={0.3}
        filter={isGlowing ? `url(#${chartId}-link-glow-${index})` : undefined}
        className="transition-opacity duration-200"
      />
    </Layer>
  );
};

// Checks whether a node is the selected one or directly linked to it
const isNodeConnected = (
  data: SankeyData,
  selectedNode: string | null,
  nodeName: string,
): boolean => {
  if (selectedNode === null || selectedNode === nodeName) return true;

  const selectedIdx = data.nodes.findIndex((node) => node.name === selectedNode);
  const nodeIdx = data.nodes.findIndex((node) => node.name === nodeName);

  return data.links.some(
    (link) =>
      (link.source === selectedIdx && link.target === nodeIdx) ||
      (link.source === nodeIdx && link.target === selectedIdx),
  );
};

// Resolves the SVG paint reference for a link band based on its variant
const getLinkFill = (
  variant: LinkVariant,
  chartId: string,
  index: number,
  config: ChartConfig,
  sourceName: string,
  targetName: string,
): string => {
  switch (variant) {
    case "gradient":
      return `url(#${chartId}-link-gradient-${index})`;
    case "source":
      return sourceName in config ? `url(#${chartId}-sankey-colors-${sourceName})` : "currentColor";
    case "target":
      return targetName in config ? `url(#${chartId}-sankey-colors-${targetName})` : "currentColor";
    case "solid":
    default:
      return "currentColor";
  }
};

// ─────────────────────────────────────────────────────────────────────────────
// Style definitions — SVG defs scoped to the chart's unique id
// ─────────────────────────────────────────────────────────────────────────────

/** Vertical color gradient for every configured node, painted by name. */
const NodeColorGradients = ({
  config,
  chartId,
}: {
  config: ChartConfig;
  chartId: string;
}) => {
  return (
    <>
      {Object.entries(config).map(([dataKey, nodeConfig]) => {
        const colorsCount = getColorsCount(nodeConfig);

        return (
          <linearGradient
            key={`${chartId}-sankey-colors-${dataKey}`}
            id={`${chartId}-sankey-colors-${dataKey}`}
            x1="0"
            y1="0"
            x2="0"
            y2="1"
          >
            {colorsCount === 1 ? (
              <>
                <stop offset="0%" stopColor={`var(--color-${dataKey}-0)`} />
                <stop offset="100%" stopColor={`var(--color-${dataKey}-0)`} />
              </>
            ) : (
              Array.from({ length: colorsCount }, (_, index) => {
                const offset = `${(index / (colorsCount - 1)) * 100}%`;
                return (
                  <stop
                    key={offset}
                    offset={offset}
                    stopColor={`var(--color-${dataKey}-${index}, var(--color-${dataKey}-0))`}
                  />
                );
              })
            )}
          </linearGradient>
        );
      })}
    </>
  );
};

/** Source-to-target fade gradient that fills a single gradient-variant link. */
const LinkGradient = ({
  chartId,
  index,
  config,
  sourceName,
  targetName,
}: {
  chartId: string;
  index: number;
  config: ChartConfig;
  sourceName: string;
  targetName: string;
}) => {
  const sourceColor = sourceName in config ? `var(--color-${sourceName}-0)` : "currentColor";
  const targetColor = targetName in config ? `var(--color-${targetName}-0)` : "currentColor";

  return (
    <linearGradient id={`${chartId}-link-gradient-${index}`} x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" stopColor={sourceColor} stopOpacity={0.2} />
      <stop offset="50%" stopColor={sourceColor} stopOpacity={0.5} />
      <stop offset="100%" stopColor={targetColor} stopOpacity={0.2} />
    </linearGradient>
  );
};

/** Primary-colored stroke gradient highlighting a link connected to the selection. */
const LinkStrokeGradient = ({ chartId, index }: { chartId: string; index: number }) => {
  return (
    <linearGradient id={`${chartId}-link-stroke-${index}`} x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" stopColor="var(--primary)" stopOpacity={0} />
      <stop offset="15%" stopColor="var(--primary)" stopOpacity={0.8} />
      <stop offset="50%" stopColor="var(--primary)" stopOpacity={1} />
      <stop offset="85%" stopColor="var(--primary)" stopOpacity={0.8} />
      <stop offset="100%" stopColor="var(--primary)" stopOpacity={0} />
    </linearGradient>
  );
};

/** Soft outer-glow SVG filter applied to a glowing node or link. */
const GlowFilter = ({
  chartId,
  name,
  type,
}: {
  chartId: string;
  name: string;
  type: "node" | "link";
}) => {
  return (
    <filter
      id={`${chartId}-${type}-glow-${name}`}
      x="-200%"
      y="-200%"
      width="400%"
      height="400%"
    >
      <feGaussianBlur in="SourceGraphic" stdDeviation="6" result="blur" />
      <feColorMatrix
        in="blur"
        type="matrix"
        values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 0.6 0"
        result="glow"
      />
      <feMerge>
        <feMergeNode in="glow" />
        <feMergeNode in="SourceGraphic" />
      </feMerge>
    </filter>
  );
};

// ─────────────────────────────────────────────────────────────────────────────
// Loading skeleton
// ─────────────────────────────────────────────────────────────────────────────

/**
 * The skeleton sankey shown while the chart is loading. Rendered by the root in
 * place of the real diagram — a fixed grid of pulsing nodes and links.
 */
const LoadingSankey = () => {
  const nodes = [
    { x: 30, y: 25, width: 12, height: 65, delay: 0 },
    { x: 30, y: 110, width: 12, height: 50, delay: 0.3 },
    { x: 30, y: 180, width: 12, height: 45, delay: 0.15 },
    { x: 244, y: 20, width: 12, height: 55, delay: 0.45 },
    { x: 244, y: 95, width: 12, height: 75, delay: 0.6 },
    { x: 244, y: 190, width: 12, height: 40, delay: 0.25 },
    { x: 458, y: 35, width: 12, height: 80, delay: 0.5 },
    { x: 458, y: 135, width: 12, height: 90, delay: 0.1 },
  ];

  const links = [
    { from: 0, to: 3, width: 26, delay: 0.2 },
    { from: 0, to: 4, width: 18, delay: 0.7 },
    { from: 1, to: 4, width: 24, delay: 0.4 },
    { from: 1, to: 5, width: 12, delay: 0.9 },
    { from: 2, to: 4, width: 16, delay: 0.1 },
    { from: 2, to: 5, width: 14, delay: 0.55 },
    { from: 3, to: 6, width: 22, delay: 0.35 },
    { from: 3, to: 7, width: 18, delay: 0.8 },
    { from: 4, to: 6, width: 28, delay: 0.05 },
    { from: 4, to: 7, width: 32, delay: 0.65 },
    { from: 5, to: 7, width: 16, delay: 0.45 },
  ];

  // Builds a bezier path connecting the right edge of one node to the left of another
  const getLinkPath = (fromIdx: number, toIdx: number) => {
    const from = nodes[fromIdx];
    const to = nodes[toIdx];
    const startX = from.x + from.width;
    const startY = from.y + from.height / 2;
    const endX = to.x;
    const endY = to.y + to.height / 2;
    const controlX1 = startX + (endX - startX) * 0.4;
    const controlX2 = startX + (endX - startX) * 0.6;
    return `M${startX},${startY} C${controlX1},${startY} ${controlX2},${endY} ${endX},${endY}`;
  };

  const baseDuration = LOADING_ANIMATION_DURATION / 1000;

  return (
    <>
      {links.map((link, i) => (
        <motion.path
          key={`loading-link-${link.from}-${link.to}`}
          d={getLinkPath(link.from, link.to)}
          fill="none"
          stroke="currentColor"
          strokeWidth={link.width}
          initial={{ opacity: 0.04 }}
          animate={{ opacity: [0.04, 0.14, 0.04] }}
          transition={{
            duration: baseDuration * (0.8 + (i % 3) * 0.2),
            delay: link.delay,
            repeat: Infinity,
            ease: "easeInOut",
          }}
        />
      ))}
      {nodes.map((node, i) => (
        <motion.rect
          key={`loading-node-${node.x}-${node.y}`}
          x={node.x}
          y={node.y}
          width={node.width}
          height={node.height}
          rx={2}
          fill="currentColor"
          initial={{ opacity: 0.15 }}
          animate={{ opacity: [0.15, 0.4, 0.15] }}
          transition={{
            duration: baseDuration * (0.9 + (i % 4) * 0.1),
            delay: node.delay,
            repeat: Infinity,
            ease: "easeInOut",
          }}
        />
      ))}
    </>
  );
};
```
        
      
       
        #### Now, Let's add the chart component to your project.

        
          These Components are required by the chart component to render the chart. Make a folder called `ui` inside the `evilcharts` folder and paste the code there.

          Below is main chart component.
        
        
          #### Component Implementation

##### Source File: `chart.tsx`

```tsx
"use client";

import * as RechartsPrimitive from "recharts";
import { cn } from "@/lib/utils";
import * as React from "react";

// Format: { THEME_NAME: CSS_SELECTOR }
const THEMES = { light: "", dark: ".dark" } as const;

type ThemeKey = keyof typeof THEMES;

// All Keys are optional at first
type ThemeColorsBase = {
  [K in ThemeKey]?: string[];
};

// Require at least one theme key
type AtLeastOneThemeColor = {
  [K in ThemeKey]: Required<Pick<ThemeColorsBase, K>> & Partial<Omit<ThemeColorsBase, K>>;
}[ThemeKey];

const VALID_THEME_KEYS = Object.keys(THEMES) as ThemeKey[];

// Validation for chart config colors at runtime
function validateChartConfigColors(config: ChartConfig): void {
  for (const [key, value] of Object.entries(config)) {
    if (value.colors) {
      const hasValidThemeKey = VALID_THEME_KEYS.some(
        (themeKey) => value.colors?.[themeKey] !== undefined,
      );

      if (!hasValidThemeKey) {
        throw new Error(
          `[EvilCharts] Invalid chart config for "${key}": colors object must have at least one theme key (${VALID_THEME_KEYS.join(", ")}). Received empty object or invalid keys.`,
        );
      }
    }
  }
}

export type ChartConfig = Record<
  string,
  {
    label?: React.ReactNode;
    icon?: React.ComponentType;
    colors?: AtLeastOneThemeColor;
  }
>;

interface ChartContextProps {
  config: ChartConfig;
}

const ChartContext = React.createContext<ChartContextProps | null>(null);

export function useChart() {
  const context = React.useContext(ChartContext);

  if (!context) {
    throw new Error("useChart must be used within a <ChartContainer />");
  }

  return context;
}

interface ChartContainerProps
  extends
    Omit<React.ComponentProps<"div">, "children">,
    Pick<
      React.ComponentProps<typeof RechartsPrimitive.ResponsiveContainer>,
      | "initialDimension"
      | "aspect"
      | "debounce"
      | "minHeight"
      | "minWidth"
      | "maxHeight"
      | "height"
      | "width"
      | "onResize"
      | "children"
    > {
  config: ChartConfig;
  innerResponsiveContainerStyle?: React.ComponentProps<
    typeof RechartsPrimitive.ResponsiveContainer
  >["style"];
  /** Optional content rendered below the chart (e.g. EvilBrush) */
  footer?: React.ReactNode;
}

function ChartContainer({
  id,
  config,
  initialDimension = { width: 320, height: 200 },
  className,
  children,
  footer,
  ...props
}: Readonly<ChartContainerProps>) {
  const uniqueId = React.useId();
  const chartId = `chart-${id ?? uniqueId.replace(/:/g, "")}`;

  // Validate chart config at runtime
  validateChartConfigColors(config);

  return (
    <ChartContext.Provider value={{ config }}>
      <div
        data-slot="chart"
        data-chart={chartId}
        className={cn(
          "min-h-0 w-full flex-1",
          "[&_.recharts-cartesian-axis-tick_text]:fill-muted-foreground [&_.recharts-cartesian-grid_line[stroke='#ccc']]:stroke-border/50 [&_.recharts-curve.recharts-tooltip-cursor]:stroke-border [&_.recharts-polar-grid_[stroke='#ccc']]:stroke-border [&_.recharts-radial-bar-background-sector]:fill-muted [&_.recharts-rectangle.recharts-tooltip-cursor]:fill-muted [&_.recharts-reference-line_[stroke='#ccc']]:stroke-border relative flex flex-col justify-center text-xs [&_.recharts-dot[stroke='#fff']]:stroke-transparent [&_.recharts-layer]:outline-hidden [&_.recharts-sector]:outline-hidden [&_.recharts-sector[stroke='#fff']]:stroke-transparent [&_.recharts-surface]:outline-hidden",
          !footer && "aspect-video",
          className,
        )}
        {...props}
      >
        <ChartStyle id={chartId} config={config} />
        <RechartsPrimitive.ResponsiveContainer
          className="min-h-0 w-full flex-1"
          initialDimension={initialDimension}
        >
          {children}
        </RechartsPrimitive.ResponsiveContainer>
        {footer}
      </div>
    </ChartContext.Provider>
  );
}

function LoadingIndicator({ isLoading }: { isLoading: boolean }) {
  if (!isLoading) {
    return null;
  }

  return (
    <div className="pointer-events-none absolute inset-0 z-20 flex items-center justify-center">
      <div className="text-primary bg-background flex items-center justify-center gap-2 rounded-md border px-2 py-0.5 text-sm">
        <div className="border-border border-t-primary h-3 w-3 animate-spin rounded-full border" />
        <span>Loading</span>
      </div>
    </div>
  );
}

// Distribute colors evenly across slots, extra slots go to last color(s)
// Example: 2 colors for 4 slots → [red, red, pink, pink]
// Example: 3 colors for 4 slots → [red, pink, blue, blue]
function distributeColors(colorsArray: string[], maxCount: number): string[] {
  const availableCount = colorsArray.length;
  if (availableCount >= maxCount) {
    return colorsArray.slice(0, maxCount);
  }

  const result: string[] = [];
  const baseSlots = Math.floor(maxCount / availableCount);
  const extraSlots = maxCount % availableCount;

  // First (availableCount - extraSlots) colors get baseSlots each
  // Last extraSlots colors get (baseSlots + 1) each
  for (let colorIdx = 0; colorIdx < availableCount; colorIdx++) {
    const isExtraColor = colorIdx >= availableCount - extraSlots;
    const slotsForThisColor = baseSlots + (isExtraColor ? 1 : 0);
    for (let j = 0; j < slotsForThisColor; j++) {
      result.push(colorsArray[colorIdx]);
    }
  }

  return result;
}

const ChartStyle = ({ id, config }: { id: string; config: ChartConfig }) => {
  const colorConfig = Object.entries(config).filter(([, config]) => config.colors);

  if (!colorConfig.length) {
    return null;
  }

  const generateCssVars = (theme: keyof typeof THEMES) =>
    colorConfig
      .flatMap(([key, itemConfig]) => {
        const colorsArray = itemConfig.colors?.[theme];
        if (!colorsArray || !Array.isArray(colorsArray) || colorsArray.length === 0) {
          return [];
        }

        // Get max count across all themes for this key
        const maxCount = getColorsCount(itemConfig);

        // Distribute colors evenly across all required slots
        const distributedColors = distributeColors(colorsArray, maxCount);

        return distributedColors.map((color, index) => `  --color-${key}-${index}: ${color};`);
      })
      .filter(Boolean)
      .join("\n");

  const css = Object.entries(THEMES)
    .map(
      ([theme, prefix]) =>
        `${prefix} [data-chart=${id}] {\n${generateCssVars(theme as keyof typeof THEMES)}\n}`,
    )
    .join("\n");

  return <style dangerouslySetInnerHTML={{ __html: css }} />;
};

// Helper to extract item config from a payload.
export function getPayloadConfigFromPayload(config: ChartConfig, payload: unknown, key: string) {
  if (typeof payload !== "object" || payload === null) {
    return undefined;
  }

  const payloadPayload =
    "payload" in payload && typeof payload.payload === "object" && payload.payload !== null
      ? payload.payload
      : undefined;

  let configLabelKey: string = key;

  if (key in payload && typeof payload[key as keyof typeof payload] === "string") {
    configLabelKey = payload[key as keyof typeof payload] as string;
  } else if (
    payloadPayload &&
    key in payloadPayload &&
    typeof payloadPayload[key as keyof typeof payloadPayload] === "string"
  ) {
    configLabelKey = payloadPayload[key as keyof typeof payloadPayload] as string;
  }

  return configLabelKey in config ? config[configLabelKey] : config[key];
}

// Format values to percent for expanded charts
function axisValueToPercentFormatter(value: number) {
  return `${Math.round(value * 100).toFixed(0)}%`;
}

// Get max colors count across all themes for a config entry
function getColorsCount(config: ChartConfig[string]): number {
  if (!config.colors) return 1;
  const counts = VALID_THEME_KEYS.map((theme) => config.colors?.[theme]?.length ?? 0);
  return Math.max(...counts, 1);
}

// Generate random loading data for skeleton/loading state
// min/max represent percentage of the range (0-100), defaults to 20-80 for realistic look
export const getLoadingData = (points: number = 10, min: number = 0, max: number = 70) => {
  const range = max - min;
  return Array.from({ length: points }, () => ({
    loading: Math.floor(Math.random() * range) + min,
  }));
};

export {
  ChartContainer,
  ChartStyle,
  axisValueToPercentFormatter,
  LoadingIndicator,
  getColorsCount,
};
```
        
      
    
  


## Usage

The sankey chart is a composible compound component: `<EvilSankeyChart />` is the
container, and `<Node />`, `<Link />`, and `<Tooltip />` are composed as children.
Render only the parts you need.

```tsx
import {
  EvilSankeyChart,
  Node,
  NodeLabel,
  Link,
  Tooltip,
} from "@/components/evilcharts/charts/sankey-chart";
import type { SankeyData } from "recharts";
```

```tsx
const data: SankeyData = {
  nodes: [
    { name: "Visit" },
    { name: "Direct-Favourite" },
    { name: "Page-Click" },
    { name: "Detail-Favourite" },
    { name: "Lost" },
  ],
  links: [
    { source: 0, target: 1, value: 3728 },
    { source: 0, target: 2, value: 354170 },
    { source: 2, target: 3, value: 62429 },
    { source: 2, target: 4, value: 291741 },
  ],
};

const chartConfig = {
  Visit: {
    label: "Visit",
    colors: { light: ["#3b82f6"], dark: ["#60a5fa"] },
  },
  "Page-Click": {
    label: "Page Click",
    colors: { light: ["#f59e0b"], dark: ["#fbbf24"] },
  },
  // ... more node configs
} satisfies ChartConfig;

<EvilSankeyChart data={data} config={chartConfig}>
  <Node isClickable>
    <NodeLabel position="outside" showValues />
  </Node>
  <Link variant="source" />
  <Tooltip />
</EvilSankeyChart>
```

### Interactive Selection

Set `isClickable` on `<Node />` to let nodes be selected by clicking. Use the
`onSelectionChange` callback on the root to handle selection events:

```tsx
<EvilSankeyChart
  data={data}
  config={chartConfig}
  onSelectionChange={(selection) => {
    if (selection) {
      console.log("Selected:", selection.dataKey, "Value:", selection.value);
    } else {
      console.log("Deselected");
    }
  }}
>
  <Node isClickable />
  <Link variant="source" />
  <Tooltip />
</EvilSankeyChart>
```

### Loading State

#### Live Example Code

##### Example File: `ex-loading-state-sankey-chart.tsx`

```tsx
"use client";

import { EvilSankeyChart, Node, Link, Tooltip } from "@/registry/charts/sankey-chart";
import type { SankeyData } from "recharts";
import { type ChartConfig } from "@/registry/ui/chart";

// Content distribution - how content flows to platforms
const data: SankeyData = {
  nodes: [
    { name: "BlogPosts" },
    { name: "Videos" },
    { name: "Podcasts" },
    { name: "Twitter" },
    { name: "LinkedIn" },
    { name: "YouTube" },
    { name: "Newsletter" },
  ],
  links: [
    { source: 0, target: 3, value: 12000 },
    { source: 0, target: 4, value: 8500 },
    { source: 0, target: 6, value: 15000 },
    { source: 1, target: 5, value: 28000 },
    { source: 1, target: 3, value: 4200 },
    { source: 2, target: 5, value: 9800 },
    { source: 2, target: 4, value: 3600 },
  ],
};

const chartConfig = {
  BlogPosts: {
    label: "Blog Posts",
    colors: {
      light: ["#3b82f6"],
      dark: ["#60a5fa"],
    },
  },
  Videos: {
    label: "Videos",
    colors: {
      light: ["#ef4444"],
      dark: ["#f87171"],
    },
  },
  Podcasts: {
    label: "Podcasts",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  Twitter: {
    label: "Twitter",
    colors: {
      light: ["#0ea5e9"],
      dark: ["#38bdf8"],
    },
  },
  LinkedIn: {
    label: "LinkedIn",
    colors: {
      light: ["#0077b5"],
      dark: ["#0a95d9"],
    },
  },
  YouTube: {
    label: "YouTube",
    colors: {
      light: ["#dc2626"],
      dark: ["#ef4444"],
    },
  },
  Newsletter: {
    label: "Newsletter",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleSankeyChart() {
  return (
    <EvilSankeyChart
      className="h-full w-full p-4"
      data={data}
      config={chartConfig}
      isLoading // [!code highlight]
    >
      <Node />
      <Link variant="source" />
      <Tooltip />
    </EvilSankeyChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    The sankey chart supports loading state with a placeholder animation showing nodes and links. You can pass the `isLoading` prop to `<EvilSankeyChart />` to show the loading state while your data is being fetched.
  


## Examples

Below are some examples of the sankey chart with different configurations. You can customize the `<Link />` `variant`, the root `nodeWidth`, `nodePadding`, and other properties.

### Gradient Colors

#### Live Example Code

##### Example File: `ex-gradient-colors-sankey-chart.tsx`

```tsx
"use client";

import { EvilSankeyChart, Node, NodeLabel, Link, Tooltip } from "@/registry/charts/sankey-chart";
import type { SankeyData } from "recharts";
import { type ChartConfig } from "@/registry/ui/chart";

// Budget allocation - revenue to expenses
const data: SankeyData = {
  nodes: [
    { name: "ProductSales" },
    { name: "Subscriptions" },
    { name: "Services" },
    { name: "TotalRevenue" },
    { name: "Research" },
    { name: "Marketing" },
    { name: "Operations" },
    { name: "Salaries" },
    { name: "Profit" },
  ],
  links: [
    { source: 0, target: 3, value: 450000 },
    { source: 1, target: 3, value: 320000 },
    { source: 2, target: 3, value: 180000 },
    { source: 3, target: 4, value: 185000 },
    { source: 3, target: 5, value: 142000 },
    { source: 3, target: 6, value: 198000 },
    { source: 3, target: 7, value: 285000 },
    { source: 3, target: 8, value: 140000 },
  ],
};

const chartConfig = {
  ProductSales: {
    label: "Product Sales",
    colors: {
      light: ["#86efac", "#22c55e", "#16a34a", "#15803d", "#166534"], // [!code highlight]
      dark: ["#bbf7d0", "#4ade80", "#22c55e", "#16a34a", "#15803d"], // [!code highlight]
    },
  },
  Subscriptions: {
    label: "Subscriptions",
    colors: {
      light: ["#93c5fd", "#3b82f6", "#2563eb", "#1d4ed8", "#1e40af"], // [!code highlight]
      dark: ["#bfdbfe", "#60a5fa", "#3b82f6", "#2563eb", "#1d4ed8"], // [!code highlight]
    },
  },
  Services: {
    label: "Services",
    colors: {
      light: ["#c4b5fd", "#8b5cf6", "#7c3aed", "#6d28d9", "#5b21b6"], // [!code highlight]
      dark: ["#ddd6fe", "#a78bfa", "#8b5cf6", "#7c3aed", "#6d28d9"], // [!code highlight]
    },
  },
  TotalRevenue: {
    label: "Total Revenue",
    colors: {
      light: ["#fde047", "#eab308", "#ca8a04", "#a16207", "#854d0e"], // [!code highlight]
      dark: ["#fef08a", "#facc15", "#eab308", "#ca8a04", "#a16207"], // [!code highlight]
    },
  },
  Research: {
    label: "R&D",
    colors: {
      light: ["#67e8f9", "#06b6d4", "#0891b2", "#0e7490", "#155e75"], // [!code highlight]
      dark: ["#a5f3fc", "#22d3ee", "#06b6d4", "#0891b2", "#0e7490"], // [!code highlight]
    },
  },
  Marketing: {
    label: "Marketing",
    colors: {
      light: ["#f9a8d4", "#ec4899", "#db2777", "#be185d", "#9d174d"], // [!code highlight]
      dark: ["#fbcfe8", "#f472b6", "#ec4899", "#db2777", "#be185d"], // [!code highlight]
    },
  },
  Operations: {
    label: "Operations",
    colors: {
      light: ["#fdba74", "#f97316", "#ea580c", "#c2410c", "#9a3412"], // [!code highlight]
      dark: ["#fed7aa", "#fb923c", "#f97316", "#ea580c", "#c2410c"], // [!code highlight]
    },
  },
  Salaries: {
    label: "Salaries",
    colors: {
      light: ["#5eead4", "#14b8a6", "#0d9488", "#0f766e", "#115e59"], // [!code highlight]
      dark: ["#99f6e4", "#2dd4bf", "#14b8a6", "#0d9488", "#0f766e"], // [!code highlight]
    },
  },
  Profit: {
    label: "Profit",
    colors: {
      light: ["#bef264", "#84cc16", "#65a30d", "#4d7c0f", "#3f6212"], // [!code highlight]
      dark: ["#d9f99d", "#a3e635", "#84cc16", "#65a30d", "#4d7c0f"], // [!code highlight]
    },
  },
} satisfies ChartConfig;

export function EvilExampleSankeyChart() {
  return (
    <EvilSankeyChart className="h-full w-full p-4" data={data} config={chartConfig}>
      <Node isClickable>
        <NodeLabel position="outside" showValues />
      </Node>
      <Link variant="gradient" />
      <Tooltip />
    </EvilSankeyChart>
  );
}
```


### Labeled Nodes

> [!NOTE]

  > [!NOTE]

    Display labels and values on nodes by composing a `<NodeLabel />` inside `<Node />`.
  


#### Inside Labels

#### Live Example Code

##### Example File: `ex-labeled-nodes-sankey-chart.tsx`

```tsx
"use client";

import { EvilSankeyChart, Node, NodeLabel, Link, Tooltip } from "@/registry/charts/sankey-chart";
import type { SankeyData } from "recharts";
import { type ChartConfig } from "@/registry/ui/chart";

// Sales report data - similar to the design with CRT, PPT, DMG categories
const data: SankeyData = {
  nodes: [
    { name: "CRT_L" }, // Left CRT
    { name: "PPT_L" }, // Left PPT
    { name: "DMG_L" }, // Left DMG
    { name: "PPT_M" }, // Middle PPT
    { name: "DMG_M" }, // Middle DMG
    { name: "CRT_R" }, // Right CRT
    { name: "PPT_R" }, // Right PPT
    { name: "DMG_R" }, // Right DMG
  ],
  links: [
    // From left CRT to middle nodes
    { source: 0, target: 3, value: 750 },
    { source: 0, target: 4, value: 502 },

    // From left PPT to middle nodes
    { source: 1, target: 3, value: 1500 },
    { source: 1, target: 4, value: 1498 },

    // From left DMG to middle nodes
    { source: 2, target: 3, value: 3931 },
    { source: 2, target: 4, value: 1612 },

    // From middle PPT to right nodes
    { source: 3, target: 5, value: 2000 },
    { source: 3, target: 6, value: 2091 },
    { source: 3, target: 7, value: 1840 },

    // From middle DMG to right nodes
    { source: 4, target: 5, value: 1991 },
    { source: 4, target: 7, value: 1158 },
  ],
};

const chartConfig = {
  CRT_L: {
    label: "CRT",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
  PPT_L: {
    label: "PPT",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  DMG_L: {
    label: "DMG",
    colors: {
      light: ["#06b6d4", "#8b5cf6"],
      dark: ["#22d3ee", "#a78bfa"],
    },
  },
  PPT_M: {
    label: "PPT",
    colors: {
      light: ["#8b5cf6"],
      dark: ["#a78bfa"],
    },
  },
  DMG_M: {
    label: "DMG",
    colors: {
      light: ["#06b6d4", "#8b5cf6"],
      dark: ["#22d3ee", "#a78bfa"],
    },
  },
  CRT_R: {
    label: "CRT",
    colors: {
      light: ["#10b981"],
      dark: ["#34d399"],
    },
  },
  PPT_R: {
    label: "PPT",
    colors: {
      light: ["#8b5cf6", "#10b981"],
      dark: ["#a78bfa", "#34d399"],
    },
  },
  DMG_R: {
    label: "DMG",
    colors: {
      light: ["#06b6d4", "#10b981"],
      dark: ["#22d3ee", "#34d399"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLabeledNodesSankeyChart() {
  return (
    <EvilSankeyChart
      className="h-full w-full p-4"
      data={data}
      config={chartConfig}
      nodeWidth={80}
      nodePadding={24}
    >
      <Node isClickable radius={4}>
        <NodeLabel
          position="inside" // [!code highlight]
          showValues // [!code highlight]
          valueFormatter={(value) => value.toLocaleString()}
        />
      </Node>
      <Link variant="gradient" verticalPadding={8} />
      <Tooltip />
    </EvilSankeyChart>
  );
}
```
#### Live Example Code

##### Example File: `ex-solid-labeled-nodes-sankey-chart.tsx`

```tsx
"use client";

import { EvilSankeyChart, Node, NodeLabel, Link, Tooltip } from "@/registry/charts/sankey-chart";
import type { SankeyData } from "recharts";
import { type ChartConfig } from "@/registry/ui/chart";

// Sales report data with solid colors
const data: SankeyData = {
  nodes: [
    { name: "CRT_L" }, // Left CRT
    { name: "PPT_L" }, // Left PPT
    { name: "DMG_L" }, // Left DMG
    { name: "PPT_M" }, // Middle PPT
    { name: "DMG_M" }, // Middle DMG
    { name: "CRT_R" }, // Right CRT
    { name: "PPT_R" }, // Right PPT
    { name: "DMG_R" }, // Right DMG
  ],
  links: [
    // From left CRT to middle nodes
    { source: 0, target: 3, value: 800 },
    { source: 0, target: 4, value: 502 },

    // From left PPT to middle nodes
    { source: 1, target: 3, value: 1500 },
    { source: 1, target: 4, value: 1498 },

    // From left DMG to middle nodes
    { source: 2, target: 3, value: 3931 },
    { source: 2, target: 4, value: 1612 },

    // From middle PPT to right nodes
    { source: 3, target: 5, value: 2000 },
    { source: 3, target: 6, value: 2091 },
    { source: 3, target: 7, value: 1840 },

    // From middle DMG to right nodes
    { source: 4, target: 5, value: 1991 },
    { source: 4, target: 7, value: 1158 },
  ],
};

const chartConfig = {
  CRT_L: {
    label: "CRT",
    colors: {
      light: ["#a3a3a3"], // lighter than #525252
      dark: ["#525252"],
    },
  },
  PPT_L: {
    label: "PPT",
    colors: {
      light: ["#d1b3ff"], // lighter than #8b5cf6
      dark: ["#8b5cf6"],
    },
  },
  DMG_L: {
    label: "DMG",
    colors: {
      light: ["#a3a3a3"], // lighter than #404040
      dark: ["#404040"],
    },
  },
  PPT_M: {
    label: "PPT",
    colors: {
      light: ["#c4b5fd"], // lighter than #7c3aed
      dark: ["#7c3aed"],
    },
  },
  DMG_M: {
    label: "DMG",
    colors: {
      light: ["#67e8f9"], // lighter than #06b6d4
      dark: ["#06b6d4"],
    },
  },
  CRT_R: {
    label: "CRT",
    colors: {
      light: ["#6ee7b7"], // lighter than #10b981
      dark: ["#10b981"],
    },
  },
  PPT_R: {
    label: "PPT",
    colors: {
      light: ["#a3a3a3"], // lighter than #525252
      dark: ["#525252"],
    },
  },
  DMG_R: {
    label: "DMG",
    colors: {
      light: ["#a3a3a3"], // lighter than #404040
      dark: ["#404040"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleSolidLabeledNodesSankeyChart() {
  return (
    <EvilSankeyChart
      className="h-full w-full p-4"
      data={data}
      config={chartConfig}
      nodeWidth={80}
      nodePadding={24}
    >
      <Node isClickable radius={4}>
        <NodeLabel
          position="inside" // [!code highlight]
          valueFormatter={(value) => value.toLocaleString()}
        />
      </Node>
      <Link variant="source" verticalPadding={8} />
      <Tooltip />
    </EvilSankeyChart>
  );
}
```
> [!NOTE]
 
  > [!NOTE]

    Use a larger `nodeWidth` (e.g., 80) on the root to accommodate the text. You can also use `verticalPadding` on `<Link />` to add padding where links connect to nodes.
  
  

#### Outside Labels

#### Live Example Code

##### Example File: `ex-outside-labels-sankey-chart.tsx`

```tsx
"use client";

import { EvilSankeyChart, Node, NodeLabel, Link, Tooltip } from "@/registry/charts/sankey-chart";
import type { SankeyData } from "recharts";
import { type ChartConfig } from "@/registry/ui/chart";

// Marketing funnel data
const data: SankeyData = {
  nodes: [
    { name: "Organic" },
    { name: "PaidAds" },
    { name: "Social" },
    { name: "Landing" },
    { name: "Product" },
    { name: "Cart" },
    { name: "Purchase" },
    { name: "Bounced" },
  ],
  links: [
    { source: 0, target: 3, value: 42000 },
    { source: 1, target: 3, value: 28000 },
    { source: 2, target: 3, value: 18000 },
    { source: 3, target: 4, value: 52000 },
    { source: 3, target: 7, value: 36000 },
    { source: 4, target: 5, value: 31000 },
    { source: 4, target: 7, value: 21000 },
    { source: 5, target: 6, value: 24000 },
    { source: 5, target: 7, value: 7000 },
  ],
};

const chartConfig = {
  Organic: {
    label: "Organic Search",
    colors: {
      light: ["#059669"],
      dark: ["#34d399"],
    },
  },
  PaidAds: {
    label: "Paid Ads",
    colors: {
      light: ["#dc2626"],
      dark: ["#f87171"],
    },
  },
  Social: {
    label: "Social Media",
    colors: {
      light: ["#7c3aed"],
      dark: ["#a78bfa"],
    },
  },
  Landing: {
    label: "Landing Page",
    colors: {
      light: ["#0891b2"],
      dark: ["#22d3ee"],
    },
  },
  Product: {
    label: "Product Page",
    colors: {
      light: ["#2563eb"],
      dark: ["#60a5fa"],
    },
  },
  Cart: {
    label: "Cart",
    colors: {
      light: ["#ea580c"],
      dark: ["#fb923c"],
    },
  },
  Purchase: {
    label: "Purchase",
    colors: {
      light: ["#16a34a"],
      dark: ["#4ade80"],
    },
  },
  Bounced: {
    label: "Bounced",
    colors: {
      light: ["#f43f5e"],
      dark: ["#fb7185"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleOutsideLabelsSankeyChart() {
  return (
    <EvilSankeyChart
      className="h-full w-full p-4"
      data={data}
      config={chartConfig}
      nodeWidth={8}
      nodePadding={20}
    >
      <Node isClickable radius={4}>
        <NodeLabel
          position="outside" // [!code highlight]
          showValues // [!code highlight]
          valueFormatter={(value) => value.toLocaleString()}
        />
      </Node>
      <Link variant="source" />
      <Tooltip />
    </EvilSankeyChart>
  );
}
```

### Link Variants

> [!NOTE]

  > [!NOTE]

    The sankey chart supports different link coloring strategies through the `variant` prop on `<Link />`.
  


#### Solid Links

" name="ex-solid-link-variant-sankey-chart"  />
> [!NOTE]
 
  > [!NOTE]

    Set `<Link />` `variant` to `"solid"` to use a single color for all links. This creates a clean, minimal look.
  


#### Source-colored Links

" name="ex-source-link-variant-sankey-chart"  />
> [!NOTE]
 
  > [!NOTE]

    Set `<Link />` `variant` to `"source"` to color links based on their source node. This helps trace where flows originate.
  


### Glowing Nodes

" name="ex-glowing-sankey-chart"  />
> [!NOTE]
 
  > [!NOTE]

    Add a subtle glow effect to specific nodes using the `glow` prop on `<Node />`. Pass an array of node names to specify which nodes should glow.
  



## API Reference

The sankey chart is composed of a root container and a small set of composible
parts. Render the root, then compose the parts you need as children.

### EvilSankeyChart


The root container. Owns the flow data, the layout configuration, the shared
context, and the loading skeleton.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **data*** | `SankeyData` | - | Sankey diagram data containing nodes and links. Nodes represent entities, and links represent flows between them. `SankeyData` is `{ nodes: SankeyNode[]; links: SankeyLink[] }`, where `SankeyNode = { name: string; icon?: ReactNode }` and `SankeyLink = { source: number; target: number; value: number }`. |
| **config*** | `ChartConfig` | - | Configuration object that defines the chart's nodes. Each key should match a node name from your data, with corresponding colors. |
| **children*** | `ReactNode` | - | The composed parts — ``, ``, and ``. |
| **className** | `string` | - | Additional CSS classes to apply to the chart container. |
| **nodeWidth** | `number` | `10` | The width of each node in pixels. |
| **nodePadding** | `number` | `10` | The vertical padding between nodes in pixels. |
| **linkCurvature** | `number` | `0.5` | The curvature of links between nodes. Value between 0 (straight) and 1 (maximum curve). |
| **iterations** | `number` | `32` | Number of iterations for the Sankey layout algorithm. Higher values produce better layouts but take more time. |
| **sort** | `boolean` | `true` | Whether to sort nodes automatically for optimal layout. |
| **align** | - | - | Horizontal alignment strategy for nodes. `"left"` aligns nodes to the left, `"justify"` spreads them across the width. |
| **verticalAlign** | - | - | Vertical alignment strategy for nodes. `"top"` aligns to top, `"justify"` distributes vertically. |
| **backgroundVariant** | `BackgroundVariant` | - | Background pattern variant to display behind the chart. |
| **defaultSelectedNode** | `string | null` | `null` | The node name selected on first render. |
| **onSelectionChange** | - | - | void">     Callback function that is called when a node is selected or deselected. Receives an object with `dataKey` (node name) and `value` (node value calculated from links), or `null` when deselected. Fires when a node is clicked while `` has `isClickable` set. |
| **isLoading** | `boolean` | `false` | Shows a loading placeholder animation when data is being fetched. |
| **sankeyProps** | - | - | '>     Additional props to pass to the underlying Recharts Sankey component. Read the Recharts Sankey documentation for available props. |



### Node


Configures how the sankey nodes render. Compose a `<NodeLabel />` inside it to
show labels and values.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **radius** | `number` | `0` | The corner radius of node rectangles in pixels. Set to 0 for square nodes. |
| **isClickable** | `boolean` | `false` | Enables interactive clicking on nodes to select/deselect them. Selected nodes become highlighted while unselected nodes and their links dim. |
| **glow** | `string[]` | `[]` | Array of node names that should have a glowing effect applied. Creates a smooth outer glow around the specified nodes. |
| **children** | `ReactNode` | - | Optional `` composition. |



### NodeLabel


Declares labels for the `<Node />` it is composed inside.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **position** | - | - | Position of node labels. `"inside"` shows labels inside nodes, `"outside"` shows labels beside nodes. When `` is not composed, no labels are shown. |
| **showValues** | `boolean` | `false` | Whether to display the total flow value alongside each node label. |
| **valueFormatter** | - | - | string" default="(value) => value.toLocaleString()">     Function to format node values when `showValues` is enabled. |



### Link


Configures how the sankey links render.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | The coloring strategy for links. `"gradient"` fades from source to target color, `"solid"` uses a single color, `"source"` uses the source node color, `"target"` uses the target node color. |
| **verticalPadding** | `number` | `0` | Vertical padding where links connect to nodes in pixels. Useful when using node labels. |
| **glow** | `number[]` | `[]` | Array of link indices that should have a glowing effect applied. Creates a smooth outer glow around the specified links. |



### Tooltip


The hover tooltip. Hidden automatically while the chart is loading.

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **variant** | - | - | Controls the visual style of the tooltip. |
| **roundness** | - | - | Controls the border-radius of the tooltip. |
| **defaultIndex** | `number` | - | When set, the tooltip will be visible by default at the specified data point index. |

---

## Background <a name="background"></a>

> Customize the background of your charts with various styles and patterns

## Usage

```tsx
<EvilLineChart
  xDataKey="month"
  data={data}
  chartConfig={chartConfig}
  backgroundVariant="dots"
/>
```

## Variants

### Dots

#### Live Example Code

##### Example File: `ex-bg-dots-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { ChartBackground } from "@/registry/ui/background";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <ChartBackground
        variant="dots" // [!code highlight]
      />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```

### Grid

#### Live Example Code

##### Example File: `ex-bg-grid-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { ChartBackground } from "@/registry/ui/background";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <ChartBackground
        variant="grid" // [!code highlight]
      />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```

### Cross Hatch

#### Live Example Code

##### Example File: `ex-bg-cross-hatch-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { ChartBackground } from "@/registry/ui/background";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <ChartBackground
        variant="cross-hatch" // [!code highlight]
      />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```

### Diagonal Lines

#### Live Example Code

##### Example File: `ex-bg-diagonal-lines-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { ChartBackground } from "@/registry/ui/background";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <ChartBackground
        variant="diagonal-lines" // [!code highlight]
      />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```

### Plus

#### Live Example Code

##### Example File: `ex-bg-plus-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { ChartBackground } from "@/registry/ui/background";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <ChartBackground
        variant="plus" // [!code highlight]
      />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```

### Falling Triangles

#### Live Example Code

##### Example File: `ex-bg-falling-triangles-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { ChartBackground } from "@/registry/ui/background";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <ChartBackground
        variant="falling-triangles" // [!code highlight]
      />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```

### 4-Pointed Star

#### Live Example Code

##### Example File: `ex-bg-4-pointed-star-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { ChartBackground } from "@/registry/ui/background";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <ChartBackground
        variant="4-pointed-star" // [!code highlight]
      />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```

### Tiny Checkers

#### Live Example Code

##### Example File: `ex-bg-tiny-checkers-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { ChartBackground } from "@/registry/ui/background";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <ChartBackground
        variant="tiny-checkers" // [!code highlight]
      />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```

### Overlapping Circles

#### Live Example Code

##### Example File: `ex-bg-overlapping-circles-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { ChartBackground } from "@/registry/ui/background";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <ChartBackground
        variant="overlapping-circles" // [!code highlight]
      />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```

### Wiggle Lines

#### Live Example Code

##### Example File: `ex-bg-wiggle-lines-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { ChartBackground } from "@/registry/ui/background";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <ChartBackground
        variant="wiggle-lines" // [!code highlight]
      />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```

### Bubbles

#### Live Example Code

##### Example File: `ex-bg-bubbles-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
  Dot,
  ActiveDot,
} from "@/registry/charts/line-chart";
import { ChartBackground } from "@/registry/ui/background";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <ChartBackground
        variant="bubbles" // [!code highlight]
      />
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend isClickable />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
      <Line dataKey="mobile" strokeVariant="solid" isClickable>
        <Dot variant="border" />
        <ActiveDot variant="colored-border" />
      </Line>
    </EvilLineChart>
  );
}
```

## Custom Variant

You can add your own background style by defining an SVG `<pattern>` and registering it as a new variant.

1. Add the new name to the `BackgroundVariant` union in `src/registry/ui/background.tsx`.
2. Create a pattern component that returns a `<pattern>` with your SVG shapes.
3. Register the pattern component in `PATTERN_MAP`.

Example custom pattern:

<CodeBlock
  wrapperClassName="mt-4"
  title="src/registry/ui/background.tsx"
  language="tsx"
  withWrapper
  code={`export type BackgroundVariant =
    | "dots"
    | "grid"
    | "cross-hatch"
    | "custom-pattern";
    // ...other variants

const CustomPattern = ({ id }: { id: string }) => (
    <pattern
      id={id}
      x="0"
      y="0"
      width="24"
      height="24"
      patternUnits="userSpaceOnUse"
    >
      <path
        className="text-border dark:text-border"
        d="M12 2l2.2 4.6L19 9l-4.8 2.4L12 16l-2.2-4.6L5 9l4.8-2.4L12 2z"
        fill="currentColor"
        fillOpacity="0.35"
      />
    </pattern>
);

const PATTERN_MAP: Record<BackgroundVariant, React.FC<{ id: string }>> = {
    dots: DotsPattern,
    grid: GridPattern,
    "cross-hatch": CrossHatchPattern,
    "custom-pattern": CustomPattern,
    // ...other variants
};
`}
/>

Then use your new variant in any chart:

```tsx
<EvilLineChart
  xDataKey="month"
  data={data}
  chartConfig={chartConfig}
  backgroundVariant="custom-pattern"
/>
```

> [!NOTE]

  > [!NOTE]

    Tip: You can use a custom logo as a pattern. In your pattern component, swap the SVG shape for an
    `<image>` element that points to your logo, then control size and transparency. For example:
    `<image href="/logo.svg" width="24" height="24" opacity="0.2" />`.

---

## Legend <a name="legend"></a>

> Display chart legends to identify different data series in your visualizations

## Usage

```tsx
<EvilLineChart
  xDataKey="month"
  data={data}
  chartConfig={chartConfig}
  legendVariant="circle" | "square" | "circle-outline" // and so on
/>
```

## Variants

Control the legend indicator style with the `legendVariant` prop.

### Square

#### Live Example Code

##### Example File: `ex-legend-square-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLegendSquareLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend
        variant="square" // [!code highlight]
      />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" />
      <Line dataKey="mobile" strokeVariant="solid" />
    </EvilLineChart>
  );
}
```

### Circle

#### Live Example Code

##### Example File: `ex-legend-circle-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLegendCircleLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend
        variant="circle" // [!code highlight]
      />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" />
      <Line dataKey="mobile" strokeVariant="solid" />
    </EvilLineChart>
  );
}
```

### Circle Outline

#### Live Example Code

##### Example File: `ex-legend-circle-outline-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLegendCircleOutlineLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend
        variant="circle-outline" // [!code highlight]
      />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" />
      <Line dataKey="mobile" strokeVariant="solid" />
    </EvilLineChart>
  );
}
```

### Rounded Square (Default)

#### Live Example Code

##### Example File: `ex-legend-rounded-square-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLegendRoundedSquareLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend
        variant="rounded-square" // [!code highlight]
      />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" />
      <Line dataKey="mobile" strokeVariant="solid" />
    </EvilLineChart>
  );
}
```

### Rounded Square Outline

#### Live Example Code

##### Example File: `ex-legend-rounded-square-outline-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLegendRoundedSquareOutlineLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend
        variant="rounded-square-outline" // [!code highlight]
      />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" />
      <Line dataKey="mobile" strokeVariant="solid" />
    </EvilLineChart>
  );
}
```

### Vertical Bar

#### Live Example Code

##### Example File: `ex-legend-vertical-bar-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLegendVerticalBarLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend
        variant="vertical-bar" // [!code highlight]
      />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" />
      <Line dataKey="mobile" strokeVariant="solid" />
    </EvilLineChart>
  );
}
```

### Horizontal Bar

#### Live Example Code

##### Example File: `ex-legend-horizontal-bar-line-chart.tsx`

```tsx
"use client";

import {
  EvilLineChart,
  Line,
  XAxis,
  Legend,
  Tooltip,
} from "@/registry/charts/line-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleLegendHorizontalBarLineChart() {
  return (
    <EvilLineChart data={data} config={chartConfig} className="h-full w-full p-4">
      <XAxis dataKey="month" tickFormatter={(value) => value.substring(0, 3)} />
      <Legend
        variant="horizontal-bar" // [!code highlight]
      />
      <Tooltip />
      <Line dataKey="desktop" strokeVariant="solid" />
      <Line dataKey="mobile" strokeVariant="solid" />
    </EvilLineChart>
  );
}
```

## API Reference

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **legendVariant** | - | - | Controls the style of the legend indicator icon. |
| **hideLegend** | `boolean` | `false` | When `true`, hides the legend entirely. |
| **hideIcon** | `boolean` | `false` | When `true`, hides the color indicator icon next to each legend item. Only the label text is shown. |
| **align** | - | - | Controls the horizontal alignment of the legend items. |

---

## Tooltip <a name="tooltip"></a>

> Display interactive tooltips when hovering over chart data points

## Usage

```tsx
<EvilBarChart
  xDataKey="month"
  data={data}
  tooltipRoundness="md" | "sm" | "lg" | "xl"  
  chartConfig={chartConfig}
  tooltipVariant="frosted-glass" | "default" // and so on
/>
```

## Variants

Control the visual style of the tooltip with the `tooltipVariant` prop. Accepts `"default"` or `"frosted-glass"`.

### Default

The default variant uses a solid background.

#### Live Example Code

##### Example File: `ex-tooltip-default-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleTooltipDefaultBarChart() {
  return (
    <EvilBarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value: string) => value.substring(0, 3)} />
      <Legend />
      <Tooltip
        variant="default" // [!code highlight]
        defaultIndex={4}
      />
      <Bar dataKey="desktop" variant="default" />
      <Bar dataKey="mobile" variant="default" />
    </EvilBarChart>
  );
}
```

### Frosted Glass

The frosted-glass variant applies a semi-transparent background with a backdrop blur effect for a frosted-glass look.

#### Live Example Code

##### Example File: `ex-tooltip-frosted-glass-bar-chart.tsx`

```tsx
"use client";

import { EvilBarChart, Bar, XAxis, Grid, Tooltip, Legend } from "@/registry/charts/bar-chart";
import { type ChartConfig } from "@/registry/ui/chart";

const data = [
  { month: "January", desktop: 342, mobile: 184 },
  { month: "February", desktop: 876, mobile: 491 },
  { month: "March", desktop: 512, mobile: 290 },
  { month: "April", desktop: 629, mobile: 391 },
  { month: "May", desktop: 458, mobile: 309 },
  { month: "June", desktop: 781, mobile: 449 },
  { month: "July", desktop: 394, mobile: 234 },
  { month: "August", desktop: 925, mobile: 557 },
  { month: "September", desktop: 647, mobile: 367 },
  { month: "October", desktop: 532, mobile: 357 },
  { month: "November", desktop: 803, mobile: 515 },
  { month: "December", desktop: 271, mobile: 149 },
];

const chartConfig = {
  desktop: {
    label: "Desktop",
    colors: {
      light: ["#047857"],
      dark: ["#10b981"],
    },
  },
  mobile: {
    label: "Mobile",
    colors: {
      light: ["#be123c"],
      dark: ["#f43f5e"],
    },
  },
} satisfies ChartConfig;

export function EvilExampleTooltipFrostedGlassBarChart() {
  return (
    <EvilBarChart data={data} config={chartConfig} className="h-full w-full p-4">
      <Grid />
      <XAxis dataKey="month" tickFormatter={(value: string) => value.substring(0, 3)} />
      <Legend />
      <Tooltip
        variant="frosted-glass" // [!code highlight]
        defaultIndex={4}
      />
      <Bar dataKey="desktop" variant="default" />
      <Bar dataKey="mobile" variant="default" />
    </EvilBarChart>
  );
}
```

## API Reference

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **tooltipVariant** | - | - | Controls the visual style of the tooltip. |
| **tooltipRoundness** | - | - | Controls the border-radius of the tooltip. |
| **tooltipDefaultIndex** | `number` | - | When set, the tooltip will be visible by default at the specified data point index. |
| **hideTooltip** | `boolean` | `false` | When `true`, hides the tooltip entirely. |
| **indicator** | - | - | The style of the color indicator shown next to each tooltip item. |
| **hideLabel** | `boolean` | `false` | When `true`, hides the label (e.g. month name) at the top of the tooltip. |
| **hideIndicator** | `boolean` | `false` | When `true`, hides the color indicator dot/line next to each tooltip item. |

---

