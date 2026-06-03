# Bklit UI Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **Bklit UI** (ui.bklit.com) — a premium, high-performance neubrutalist SVG React chart library designed with Visx, Framer Motion (Motion), Tailwind CSS, and TypeScript.

## Table of Contents

- [Charts](#charts)
  - [Area Chart](#area-chart)
  - [Bar Chart](#bar-chart)
  - [Candlestick Chart](#candlestick-chart)
  - [Choropleth Chart](#choropleth-chart)
  - [Composed Chart](#composed-chart)
  - [Funnel Chart](#funnel-chart)
  - [Gauge](#gauge-chart)
  - [Line Chart](#line-chart)
  - [Live Line Chart](#live-line-chart)
  - [Pie Chart](#pie-chart)
  - [Profit/Loss Line](#profit-loss-line)
  - [Radar Chart](#radar-chart)
  - [Ring Chart](#ring-chart)
  - [Sankey Chart](#sankey-chart)
  - [Scatter Chart](#scatter-chart)
- [Utility](#utility)
  - [Installation](#installation)
  - [Skills](#skills)
  - [Theming](#theming)
  - [X Axis](#x-axis)
  - [Y Axis](#y-axis)
  - [Custom Indicator](#custom-indicator)
  - [Grid](#grid)
  - [Legend](#legend)
  - [Tooltip](#tooltip)
  - [useChart Hook](#use-chart)

---

## Charts <a name="charts"></a>

### Area Chart <a name="area-chart"></a>

A composable area chart with gradient fills, tooltips, and hover interactions

- **Category**: Charts
- **Registry Key**: `area-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/area-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `@visx/curve@4.0.1-alpha.0`
  - `@visx/gradient@4.0.1-alpha.0`
  - `@visx/shape@4.0.1-alpha.0`
  - `motion`
- **Local Registry Dependencies**:
  - `@bklit/chart-context`
  - `@bklit/chart-animation`
  - `@bklit/chart-series`
  - `@bklit/grid`
  - `@bklit/x-axis`
  - `@bklit/chart-tooltip`
  - `@bklit/utils`

#### How to Use

##### Example 1

```tsx
import { AreaChart, Area, Grid, XAxis, ChartTooltip } from "@bklitui/ui/charts";

const data = [
  { date: new Date("2025-01-01"), revenue: 12000, costs: 8500 },
  { date: new Date("2025-01-02"), revenue: 13500, costs: 9200 },
  // ... more data
];

export default function RevenueChart() {
  return (
    <AreaChart data={data}>
      <Grid horizontal />
      <Area dataKey="revenue" fill="var(--chart-line-primary)" />
      <Area dataKey="costs" fill="var(--chart-line-secondary)" />
      <XAxis />
      <ChartTooltip />
    </AreaChart>
  );
}
```

##### Example 2

```tsx
<Area
  dataKey="visitors"
  dashFromIndex={5}
  dashArray="6,4"
  fill="var(--chart-line-primary)"
  fillOpacity={0.35}
/>
```

##### Example 3

```tsx
import {
  AreaChart,
  Area,
  ChartTooltip,
  ChartMarkers,
  MarkerTooltipContent,
  useActiveMarkers,
  type ChartMarker,
} from "@bklitui/ui/charts";

const markers: ChartMarker[] = [
  {
    date: new Date("2025-01-05"),
    icon: "🚀",
    title: "v1.2.0 Released",
    description: "New chart animations",
  },
];

function MyChart({ data }) {
  return (
    <AreaChart data={data}>
      <Area dataKey="revenue" fill="var(--chart-line-primary)" />
      <ChartMarkers items={markers} />
      <ChartTooltip>
        <MarkerContent markers={markers} />
      </ChartTooltip>
    </AreaChart>
  );
}

function MarkerContent({ markers }) {
  const activeMarkers = useActiveMarkers(markers);
  if (activeMarkers.length === 0) return null;
  return <MarkerTooltipContent markers={activeMarkers} />;
}
```

##### Example 4

```tsx
import {
  AreaChart,
  Area,
  Grid,
  XAxis,
  ChartTooltip,
  SegmentBackground,
  SegmentLineFrom,
  SegmentLineTo,
} from "@bklitui/ui/charts";

<AreaChart data={data}>
  <Grid horizontal />
  <Area dataKey="revenue" fill="var(--chart-line-primary)" />
  <SegmentBackground />
  <SegmentLineFrom />
  <SegmentLineTo />
  <XAxis />
  <ChartTooltip />
</AreaChart>
```

##### Example 5

```tsx
import { useChart } from "@bklitui/ui/charts";

function SelectionStats({ onSelectionChange }) {
  const { selection, data, xAccessor } = useChart();

  useEffect(() => {
    if (!selection?.active) {
      onSelectionChange(null);
      return;
    }

    const startPoint = data[selection.startIndex];
    const endPoint = data[selection.endIndex];
    onSelectionChange({ startPoint, endPoint });
  }, [selection, data, xAccessor, onSelectionChange]);

  return null;
}
```

#### Props & API Reference

##### `AreaChart` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `data` | `Record<string, unknown>[]` | required | Array of data points |
| `xDataKey` | `string` | `"date"` | Key in data for x-axis values |
| `margin` | `Partial<Margin>` | `{ top: 40, right: 40, bottom: 40, left: 40 }` | Chart margins |
| `animationDuration` | `number` | `1100` | Animation duration in ms |
| `aspectRatio` | `string` | `"2 / 1"` | CSS aspect ratio |
| `className` | `string` | `""` | Additional CSS class |

##### `Area` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dataKey` | `string` | required | Key in data for y values |
| `fill` | `string` | `var(--chart-line-primary)` | Gradient fill color |
| `fillOpacity` | `number` | `0.4` | Fill opacity at the top |
| `stroke` | `string` | Same as `fill` | Line stroke color |
| `strokeWidth` | `number` | `2` | Line stroke width |
| `curve` | `CurveFactory` | `curveMonotoneX` | D3 curve function |
| `animate` | `boolean` | `true` | Enable grow animation |
| `showLine` | `boolean` | `true` | Show stroke line on top |
| `showHighlight` | `boolean` | `true` | Show highlight on hover |
| `gradientToOpacity` | `number` | `0` | Opacity at bottom of gradient |
| `fadeEdges` | `boolean` | `false` | Fade area fill at left/right edges |
| `showMarkers` | `boolean` | `false` | Render scatter-style ring markers at each point |
| `markers` | `SeriesPointMarkerStyle` | — | Marker styling (same options as [`Scatter`](/docs/components/scatter-chart)) |

##### `PatternArea` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dataKey` | `string` | required | Key in data for y values |
| `fill` | `string` | required | Fill color or pattern URL (e.g. `url(#pattern-id)`) |
| `curve` | `CurveFactory` | `curveMonotoneX` | D3 curve function |

##### `Grid` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `horizontal` | `boolean` | `true` | Show horizontal lines |
| `vertical` | `boolean` | `false` | Show vertical lines |
| `numTicksRows` | `number` | `5` | Number of horizontal lines |
| `numTicksColumns` | `number` | `10` | Number of vertical lines |
| `stroke` | `string` | `var(--chart-grid)` | Line color |
| `strokeDasharray` | `string` | `"4,4"` | Dash pattern |

##### `XAxis` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `numTicks` | `number` | `5` | Number of tick labels when `tickMode` is `"domain"` |
| `tickerHalfWidth` | `number` | `50` | Fade radius for labels |
| `tickMode` | `"domain" \| "data"` | `"domain"` | `"domain"` for evenly spaced ticks; `"data"` for one label per row |

##### `ChartTooltip` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showDatePill` | `boolean` | `true` | Show animated date ticker |
| `showCrosshair` | `boolean` | `true` | Show vertical crosshair |
| `showDots` | `boolean` | `true` | Show dots on series |
| `content` | `(props) => ReactNode` | - | Custom content renderer |
| `rows` | `(point) => TooltipRow[]` | - | Custom row generator |

##### `Dashed tail` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dashFromIndex` | `number` | — | Inclusive data index where the dashed tail begins |
| `dashArray` | `string` | `"6,4"` | SVG `stroke-dasharray` pattern for the tail segment |

##### `ChartMarkers Props` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `items` | `ChartMarker[]` | required | Array of markers |
| `size` | `number` | `28` | Marker circle size |
| `showLines` | `boolean` | `true` | Show vertical guide lines |
| `animate` | `boolean` | `true` | Animate markers on entrance |

##### `SegmentBackground` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `fill` | `string` | `var(--chart-segment-background)` | Fill color for the selected region |

##### `SegmentLineFrom / SegmentLineTo` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `stroke` | `string` | `var(--chart-segment-line)` | Line color |
| `strokeWidth` | `number` | `1` | Line width |
| `variant` | `"dashed" \| "solid" \| "gradient"` | `"dashed"` | Line style |

#### Component Source Code

##### File: `src/charts/area-chart.tsx`

```tsx
"use client";

import { ParentSize } from "@visx/responsive";
import type { Transition } from "motion/react";
import {
  Children,
  isValidElement,
  type ReactNode,
  useMemo,
  useRef,
} from "react";
import { cn } from "@/lib/utils";
import { Area, type AreaProps } from "./area";
import type { LineConfig, Margin } from "./chart-context";
import { PatternArea } from "./pattern-area";
import { TimeSeriesChartInner } from "./time-series-chart-shell";

export interface AreaChartProps {
  /** Data array - each item should have a date field and numeric values */
  data: Record<string, unknown>[];
  /** Key in data for the x-axis (date). Default: "date" */
  xDataKey?: string;
  /** Chart margins */
  margin?: Partial<Margin>;
  /** Animation duration in milliseconds. Default: 1100 */
  animationDuration?: number;
  /** CSS easing for clip-reveal. Default: cubic-bezier(0.85, 0, 0.15, 1) */
  animationEasing?: string;
  /** Motion enter transition (spring or cubic-bezier tween). */
  enterTransition?: Transition;
  /** Signature of motion URL state — triggers reveal replay when it changes. */
  revealSignature?: string;
  /** Aspect ratio as "width / height". Default: "2 / 1" */
  aspectRatio?: string;
  /** Additional class name for the container */
  className?: string;
  /** Child components (Area, Grid, ChartTooltip, etc.) */
  children: ReactNode;
}

const DEFAULT_MARGIN: Margin = { top: 40, right: 40, bottom: 40, left: 40 };

function extractAreaConfigs(children: ReactNode): LineConfig[] {
  const configs: LineConfig[] = [];

  Children.forEach(children, (child) => {
    if (!isValidElement(child)) {
      return;
    }

    const childType = child.type as {
      displayName?: string;
      name?: string;
    };
    const componentName =
      typeof child.type === "function"
        ? childType.displayName || childType.name || ""
        : "";

    const props = child.props as AreaProps | undefined;
    const isPatternArea =
      componentName === "PatternArea" || child.type === PatternArea;
    const isAreaComponent =
      componentName === "Area" ||
      child.type === Area ||
      (props &&
        typeof props.dataKey === "string" &&
        props.dataKey.length > 0 &&
        !isPatternArea);

    if (isAreaComponent && props?.dataKey) {
      configs.push({
        dataKey: props.dataKey,
        stroke: props.stroke || props.fill || "var(--chart-line-primary)",
        strokeWidth: props.strokeWidth || 2,
      });
    }
  });

  return configs;
}

interface ChartInnerProps {
  width: number;
  height: number;
  data: Record<string, unknown>[];
  xDataKey: string;
  margin: Margin;
  animationDuration: number;
  animationEasing?: string;
  enterTransition?: Transition;
  revealSignature?: string;
  children: ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
}

function ChartInner({
  width,
  height,
  data,
  xDataKey,
  margin,
  animationDuration,
  animationEasing,
  enterTransition,
  revealSignature,
  children,
  containerRef,
}: ChartInnerProps) {
  const lines = useMemo(() => extractAreaConfigs(children), [children]);

  return (
    <TimeSeriesChartInner
      animationDuration={animationDuration}
      animationEasing={animationEasing}
      clipPathId="chart-area-grow-clip"
      containerRef={containerRef}
      data={data}
      enterTransition={enterTransition}
      height={height}
      lines={lines}
      margin={margin}
      revealSignature={revealSignature}
      width={width}
      xDataKey={xDataKey}
    >
      {children}
    </TimeSeriesChartInner>
  );
}

export function AreaChart({
  data,
  xDataKey = "date",
  margin: marginProp,
  animationDuration = 1100,
  animationEasing,
  enterTransition,
  revealSignature,
  aspectRatio = "2 / 1",
  className = "",
  children,
}: AreaChartProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const margin = { ...DEFAULT_MARGIN, ...marginProp };

  return (
    <div
      className={cn("relative w-full", className)}
      ref={containerRef}
      style={{ aspectRatio, touchAction: "none" }}
    >
      <ParentSize debounceTime={10}>
        {({ width, height }) => (
          <ChartInner
            animationDuration={animationDuration}
            animationEasing={animationEasing}
            containerRef={containerRef}
            data={data}
            enterTransition={enterTransition}
            height={height}
            margin={margin}
            revealSignature={revealSignature}
            width={width}
            xDataKey={xDataKey}
          >
            {children}
          </ChartInner>
        )}
      </ParentSize>
    </div>
  );
}

export { Area, type AreaProps } from "./area";

export default AreaChart;
```

##### File: `src/charts/time-series-chart-shell.tsx`

```tsx
"use client";

import { scaleLinear, scaleTime } from "@visx/scale";
import { bisector, extent } from "d3-array";
import type { Transition } from "motion/react";
import {
  Children,
  cloneElement,
  isValidElement,
  memo,
  type ReactElement,
  type ReactNode,
  useCallback,
  useEffect,
  useMemo,
  useState,
} from "react";
import { DEFAULT_ANIMATION_EASING } from "./animation";
import { ChartProvider, type LineConfig, type Margin } from "./chart-context";
import { isGradientDefComponent, isPatternDefComponent } from "./chart-defs";
import { shortDateFmt } from "./chart-formatters";
import { ChartRevealClip } from "./chart-reveal-clip";
import {
  decimateTimeSeries,
  maxRenderPointsForWidth,
} from "./decimate-time-series";
import {
  computeSeriesBarRevealClipPadding,
  computeSeriesBarWidth,
} from "./series-bar-layout";
import { useStaticChartPreview } from "./static-chart-preview-context";
import { useChartInteraction } from "./use-chart-interaction";

function collectNumericExtents(
  data: Record<string, unknown>[],
  dataKeys: string[]
) {
  let minValue = Number.POSITIVE_INFINITY;
  let maxValue = Number.NEGATIVE_INFINITY;

  for (const d of data) {
    for (const key of dataKeys) {
      const value = d[key];
      if (typeof value === "number") {
        if (value < minValue) {
          minValue = value;
        }
        if (value > maxValue) {
          maxValue = value;
        }
      }
    }
  }

  if (minValue === Number.POSITIVE_INFINITY) {
    return { minValue: 0, maxValue: 100 };
  }

  return { minValue, maxValue };
}

function resolveTimeSeriesYDomain(
  data: Record<string, unknown>[],
  dataKeys: string[],
  yScaleDomainMax: number | undefined
): [number, number] {
  if (yScaleDomainMax != null && yScaleDomainMax > 0) {
    return [0, yScaleDomainMax * 1.1];
  }

  const { minValue, maxValue } = collectNumericExtents(data, dataKeys);

  if (minValue >= 0) {
    const top = maxValue <= 0 ? 100 : maxValue * 1.1;
    return [0, top];
  }

  const padding = (maxValue - minValue) * 0.05 || 1;
  return [minValue - padding, maxValue + padding];
}

/** Markers render after the interaction overlay so they stay clickable. */
export function isPostOverlayComponent(child: ReactElement): boolean {
  const childType = child.type as {
    displayName?: string;
    name?: string;
    __isChartMarkers?: boolean;
  };

  if (childType.__isChartMarkers) {
    return true;
  }

  const componentName =
    typeof child.type === "function"
      ? childType.displayName || childType.name || ""
      : "";

  return componentName === "ChartMarkers" || componentName === "MarkerGroup";
}

function ensureChildKey(child: ReactElement, index: number): ReactElement {
  if (child.key != null) {
    return child;
  }
  return cloneElement(child, { key: `chart-child-${index}` });
}

export interface TimeSeriesChartInnerProps {
  width: number;
  height: number;
  data: Record<string, unknown>[];
  xDataKey: string;
  margin: Margin;
  animationDuration: number;
  animationEasing?: string;
  enterTransition?: Transition;
  /** Signature of motion URL state — triggers reveal replay when it changes. */
  revealSignature?: string;
  children: ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
  /** Series keys driving y-domain and tooltip (Line / Area / SeriesBar configs). */
  lines: LineConfig[];
  /** SVG clipPath id for grow animation. */
  clipPathId: string;
  /** Optional ComposedChart bar layout (forwarded into context). */
  composedBarDataKeys?: string[];
  composedBarSize?: number;
  composedMaxBarSize?: number;
  composedBarGap?: number;
  composedStacked?: boolean;
  composedStackOffsets?: Map<number, Map<string, number>>;
  composedStackGap?: number;
  /** When set, drives the y-axis max instead of scanning `lines` (e.g. stacked bar totals). */
  yScaleDomainMax?: number;
}

export function TimeSeriesChartInner(props: TimeSeriesChartInnerProps) {
  const { width, height } = props;
  if (width < 10 || height < 10) {
    return null;
  }
  return <TimeSeriesChartCore {...props} />;
}

const TimeSeriesChartCore = memo(function TimeSeriesChartCore({
  width,
  height,
  data,
  xDataKey,
  margin,
  animationDuration,
  animationEasing = DEFAULT_ANIMATION_EASING,
  enterTransition,
  revealSignature = "",
  children,
  containerRef,
  lines,
  clipPathId,
  composedBarDataKeys,
  composedBarSize,
  composedMaxBarSize,
  composedBarGap,
  composedStacked,
  composedStackOffsets,
  composedStackGap,
  yScaleDomainMax,
}: TimeSeriesChartInnerProps) {
  const staticPreview = useStaticChartPreview();
  const [isLoaded, setIsLoaded] = useState(staticPreview);
  const [revealEpoch, setRevealEpoch] = useState(0);

  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  const xAccessor = useCallback(
    (d: Record<string, unknown>): Date => {
      const value = d[xDataKey];
      return value instanceof Date ? value : new Date(value as string | number);
    },
    [xDataKey]
  );

  const bisectDate = useMemo(
    () => bisector<Record<string, unknown>, Date>((d) => xAccessor(d)).left,
    [xAccessor]
  );

  const xScale = useMemo(() => {
    const timeExtent = extent(data, (d) => xAccessor(d).getTime());
    const minTime = timeExtent[0] ?? 0;
    const maxTime = timeExtent[1] ?? minTime;

    return scaleTime({
      range: [0, innerWidth],
      domain: [minTime, maxTime],
    });
  }, [innerWidth, data, xAccessor]);

  const renderData = useMemo(() => {
    const valueKeys = lines.map((line) => line.dataKey);
    return decimateTimeSeries(
      data,
      maxRenderPointsForWidth(innerWidth),
      valueKeys
    );
  }, [data, innerWidth, lines]);

  const columnWidth = useMemo(() => {
    if (data.length < 2) {
      return 0;
    }
    return innerWidth / (data.length - 1);
  }, [innerWidth, data.length]);

  const yScale = useMemo(() => {
    const dataKeys = lines.map((line) => line.dataKey);
    const domain = resolveTimeSeriesYDomain(data, dataKeys, yScaleDomainMax);

    return scaleLinear({
      range: [innerHeight, 0],
      domain,
      nice: true,
    });
  }, [innerHeight, data, lines, yScaleDomainMax]);

  const dateLabels = useMemo(
    () => data.map((d) => shortDateFmt.format(xAccessor(d))),
    [data, xAccessor]
  );

  // biome-ignore lint/correctness/useExhaustiveDependencies: revealSignature
  useEffect(() => {
    if (staticPreview) {
      setIsLoaded(true);
      return;
    }

    setRevealEpoch((n) => n + 1);
    setIsLoaded(false);
    const timer = setTimeout(() => {
      setIsLoaded(true);
    }, animationDuration);
    return () => clearTimeout(timer);
  }, [animationDuration, revealSignature, staticPreview]);

  const canInteract = isLoaded;

  const {
    tooltipData,
    setTooltipData,
    selection,
    clearSelection,
    interactionHandlers,
    interactionStyle,
  } = useChartInteraction({
    xScale,
    yScale,
    data,
    lines,
    margin,
    xAccessor,
    bisectDate,
    canInteract,
  });

  const defsChildren: ReactElement[] = [];
  const preOverlayChildren: ReactElement[] = [];
  const postOverlayChildren: ReactElement[] = [];

  Children.forEach(children, (child, index) => {
    if (!isValidElement(child)) {
      return;
    }

    const keyedChild = ensureChildKey(child, index);

    if (isGradientDefComponent(keyedChild)) {
      defsChildren.push(keyedChild);
    } else if (isPatternDefComponent(keyedChild)) {
      // Keep pattern defs in the plot <g> (same as main) — hoisting breaks url(#id) fills.
      preOverlayChildren.push(keyedChild);
    } else if (isPostOverlayComponent(keyedChild)) {
      postOverlayChildren.push(keyedChild);
    } else {
      preOverlayChildren.push(keyedChild);
    }
  });

  const contextValue = useMemo(
    () => ({
      data,
      renderData,
      xScale,
      yScale,
      width,
      height,
      innerWidth,
      innerHeight,
      margin,
      columnWidth,
      tooltipData,
      setTooltipData,
      containerRef,
      lines,
      isLoaded,
      animationDuration,
      animationEasing,
      enterTransition,
      revealEpoch,
      xAccessor,
      dateLabels,
      selection,
      clearSelection,
      composedBarDataKeys,
      composedBarSize,
      composedMaxBarSize,
      composedBarGap,
      composedStacked,
      composedStackOffsets,
      composedStackGap,
    }),
    [
      data,
      renderData,
      xScale,
      yScale,
      width,
      height,
      innerWidth,
      innerHeight,
      margin,
      columnWidth,
      tooltipData,
      setTooltipData,
      containerRef,
      lines,
      isLoaded,
      animationDuration,
      animationEasing,
      enterTransition,
      revealEpoch,
      xAccessor,
      dateLabels,
      selection,
      clearSelection,
      composedBarDataKeys,
      composedBarSize,
      composedMaxBarSize,
      composedBarGap,
      composedStacked,
      composedStackOffsets,
      composedStackGap,
    ]
  );

  // Single shared reveal clip for every series. Replaces the per-<Line> /
  // per-<Area> `<ChartRevealClip>` motion.rects: one motion-driven attribute
  // animation instead of N, with all series referencing the same `<clipPath>`.
  // The wipe semantics (left-to-right unveil of static path geometry) are
  // identical to the previous per-series clips.
  // animationDuration === 0 truly disables the reveal (no clipPath wrapper),
  // so consumers can opt out without having to also pass enterTransition.
  const showReveal =
    !staticPreview &&
    renderData.length > 1 &&
    innerWidth > 0 &&
    animationDuration > 0;
  // If the consumer didn't pass an explicit enterTransition, derive one from
  // animationDuration so clipRevealTransition picks up the override instead
  // of falling back to its 1100ms default.
  const effectiveEnterTransition: Transition = enterTransition ?? {
    type: "tween",
    duration: animationDuration / 1000,
  };

  const revealClipPadding = useMemo(() => {
    if (!composedBarDataKeys?.length) {
      return 0;
    }
    const barWidth = computeSeriesBarWidth({
      innerWidth,
      dataLength: data.length,
      columnWidth,
      seriesCount: composedBarDataKeys.length,
      composedBarSize,
      composedMaxBarSize,
      composedBarGap,
      stacked: composedStacked,
    });
    return computeSeriesBarRevealClipPadding({
      barWidth,
      seriesCount: composedBarDataKeys.length,
      gap: composedBarGap,
      stacked: composedStacked,
    });
  }, [
    columnWidth,
    composedBarDataKeys,
    composedBarGap,
    composedBarSize,
    composedMaxBarSize,
    composedStacked,
    data.length,
    innerWidth,
  ]);

  return (
    <ChartProvider value={contextValue}>
      <svg aria-hidden="true" height={height} width={width}>
        <defs>
          {defsChildren}
          {showReveal ? (
            <ChartRevealClip
              clipPathId={clipPathId}
              enterTransition={effectiveEnterTransition}
              height={innerHeight + 20}
              padding={revealClipPadding}
              revealEpoch={revealEpoch}
              targetWidth={innerWidth}
            />
          ) : null}
        </defs>

        <rect fill="transparent" height={height} width={width} x={0} y={0} />

        <g
          {...interactionHandlers}
          style={interactionStyle}
          transform={`translate(${margin.left},${margin.top})`}
        >
          <rect
            fill="transparent"
            height={innerHeight}
            width={innerWidth}
            x={0}
            y={0}
          />

          {showReveal ? (
            <g clipPath={`url(#${clipPathId})`}>{preOverlayChildren}</g>
          ) : (
            preOverlayChildren
          )}
          {postOverlayChildren}
        </g>
      </svg>
    </ChartProvider>
  );
});
```

##### File: `src/charts/series-bar-layout.ts`

```tsx
export function computeSeriesBarWidth(input: {
  innerWidth: number;
  dataLength: number;
  columnWidth: number;
  seriesCount: number;
  composedBarSize?: number;
  composedMaxBarSize?: number;
  composedBarGap?: number;
  stacked?: boolean;
}): number {
  const {
    innerWidth,
    dataLength,
    columnWidth,
    seriesCount,
    composedBarSize,
    composedMaxBarSize,
    composedBarGap = 4,
    stacked = false,
  } = input;

  const gap = composedBarGap;
  const groupCount = stacked ? 1 : Math.max(1, seriesCount);
  let slot = columnWidth;
  if (slot <= 0) {
    slot = dataLength < 2 ? innerWidth : innerWidth / (dataLength - 1);
  }

  let width =
    composedBarSize ??
    Math.min(slot * 0.88, composedMaxBarSize ?? Number.POSITIVE_INFINITY);
  if (composedMaxBarSize != null) {
    width = Math.min(width, composedMaxBarSize);
  }
  if (groupCount > 1) {
    const maxGroup = slot * 0.92;
    const needed = groupCount * width + (groupCount - 1) * gap;
    if (needed > maxGroup && maxGroup > 0) {
      width = Math.max(4, (maxGroup - (groupCount - 1) * gap) / groupCount);
    }
  }

  return Math.max(2, width);
}

/** Half-width of the bar group at each x — used to pad reveal clips. */
export function computeSeriesBarRevealClipPadding(input: {
  barWidth: number;
  seriesCount: number;
  gap?: number;
  stacked?: boolean;
}): number {
  const { barWidth, seriesCount, gap = 4, stacked = false } = input;

  if (stacked || seriesCount <= 1) {
    return Math.ceil(barWidth / 2);
  }

  const groupWidth = seriesCount * barWidth + (seriesCount - 1) * gap;
  return Math.ceil(groupWidth / 2);
}
```

##### File: `src/charts/area.tsx`

```tsx
"use client";

import { curveMonotoneX } from "@visx/curve";
import { AreaClosed, LinePath } from "@visx/shape";

// CurveFactory type - simplified version compatible with visx
// biome-ignore lint/suspicious/noExplicitAny: d3 curve factory type
type CurveFactory = any;

import { useCallback, useId, useRef } from "react";
import { AreaGradientDefs } from "./area-gradient-defs";
import { chartCssVars, useChartStable } from "./chart-context";
import { type FadeEdges, resolveFadeSides } from "./fade-edges";
import {
  resolveDashTailBounds,
  usePathStrokeMetrics,
} from "./path-stroke-utils";
import { SeriesDashTailOverlay } from "./series-dash-tail-overlay";
import { SeriesHighlightLayer } from "./series-highlight-layer";
import { SeriesHoverDim } from "./series-hover-dim";
import { SeriesMarkers } from "./series-markers";
import type { SeriesPointMarkerStyle } from "./series-point-marker";

export interface AreaProps {
  /** Key in data to use for y values */
  dataKey: string;
  /** Fill color for the area gradient start. Default: var(--chart-line-primary) */
  fill?: string;
  /** Fill opacity at the top of the area. Default: 0.4 */
  fillOpacity?: number;
  /** Stroke color for the line. Default: same as fill */
  stroke?: string;
  /** Stroke width. Default: 2 */
  strokeWidth?: number;
  /** Curve function. Default: curveMonotoneX */
  curve?: CurveFactory;
  /** Whether to animate the area. Default: true */
  animate?: boolean;
  /** Whether to show the stroke line. Default: true */
  showLine?: boolean;
  /** Whether to show highlight segment on hover. Default: true */
  showHighlight?: boolean;
  /** Gradient opacity at bottom (0 = fully transparent). Default: 0 */
  gradientToOpacity?: number;
  /**
   * Fade the area fill (and stroke) toward transparent at the chart edges.
   * - `true` fades both edges, `false` disables the fade entirely.
   * - `"left"` / `"right"` fades only that side — useful when the opposite
   *   edge butts up against another element you don't want to fade into.
   * Default: false
   */
  fadeEdges?: FadeEdges;
  /** Render scatter-style circle markers at each data point. Default: false */
  showMarkers?: boolean;
  /** Marker styling (same options as Scatter). */
  markers?: SeriesPointMarkerStyle;
  /**
   * Data index from which the line stroke becomes dashed (inclusive).
   * Useful for projecting incomplete periods, e.g. dashed from yesterday through today.
   */
  dashFromIndex?: number;
  /** Dash pattern for the tail segment when `dashFromIndex` is set. Default: "6,4" */
  dashArray?: string;
}

export function Area({
  dataKey,
  fill = chartCssVars.linePrimary,
  fillOpacity = 0.4,
  stroke,
  strokeWidth = 2,
  curve = curveMonotoneX,
  animate = true,
  showLine = true,
  showHighlight = true,
  gradientToOpacity = 0,
  fadeEdges = false,
  showMarkers = false,
  markers,
  dashFromIndex,
  dashArray = "6,4",
}: AreaProps) {
  // Stable slice only: hover state lives inside `<SeriesHoverDim>` and
  // `<SeriesHighlightLayer>` so this component (and its expensive
  // <SeriesDashTailOverlay> child) does not re-render on cursor motion.
  // The reveal-clip is now a single shared clipPath at the chart-shell
  // level (`time-series-chart-shell.tsx`); we no longer render a per-area
  // `<ChartRevealClip>` or read `revealEpoch` here.
  const {
    data,
    renderData,
    xScale,
    yScale,
    innerHeight,
    innerWidth,
    xAccessor,
  } = useChartStable();

  const pathRef = useRef<SVGPathElement>(null);
  const { pathLength, pathD } = usePathStrokeMetrics(pathRef, [
    renderData,
    innerWidth,
    dashFromIndex,
    showLine,
  ]);

  // Unique IDs for this area
  const uniqueId = useId();
  const gradientId = `area-gradient-${dataKey}-${uniqueId}`;
  const strokeGradientId = `area-stroke-gradient-${dataKey}-${uniqueId}`;
  const edgeMaskId = `area-edge-mask-${dataKey}-${uniqueId}`;
  const edgeGradientId = `${edgeMaskId}-gradient`;

  const isPatternFill = fill.startsWith("url(");
  const showAreaFill = isPatternFill || fillOpacity > 0;
  const areaFill = isPatternFill ? fill : `url(#${gradientId})`;

  // Resolved stroke color (defaults to fill; pattern URLs need a real color)
  const resolvedStroke =
    stroke || (isPatternFill ? chartCssVars.linePrimary : fill);

  const getY = useCallback(
    (d: Record<string, unknown>) => {
      const value = d[dataKey];
      return typeof value === "number" ? (yScale(value) ?? 0) : 0;
    },
    [dataKey, yScale]
  );

  const hasDashTail = resolveDashTailBounds(dashFromIndex, data.length);
  // The stroke gradient is only emitted when at least one edge fades, so fall
  // back to the resolved solid color otherwise — avoids an invalid url(#...).
  const fadeSides = resolveFadeSides(fadeEdges);
  const strokePaint = fadeSides.any
    ? `url(#${strokeGradientId})`
    : resolvedStroke;
  const highlightEnabled = showHighlight && showLine;

  return (
    <>
      <AreaGradientDefs
        edgeGradientId={edgeGradientId}
        edgeMaskId={edgeMaskId}
        fadeEdges={fadeEdges}
        fill={fill}
        fillOpacity={fillOpacity}
        gradientId={gradientId}
        gradientToOpacity={gradientToOpacity}
        innerHeight={innerHeight}
        innerWidth={innerWidth}
        isPatternFill={isPatternFill}
        resolvedStroke={resolvedStroke}
        strokeGradientId={strokeGradientId}
      />

      <SeriesHoverDim dimOpacity={0.6} enabled={showHighlight}>
        {/* Area fill */}
        {showAreaFill ? (
          <g
            mask={
              fadeSides.any && !isPatternFill
                ? `url(#${edgeMaskId})`
                : undefined
            }
          >
            <AreaClosed
              curve={curve}
              data={renderData}
              fill={areaFill}
              x={(d) => xScale(xAccessor(d)) ?? 0}
              y={getY}
              yScale={yScale}
            />
          </g>
        ) : null}

        {/* Stroke line on top of area */}
        {showLine ? (
          <>
            <LinePath
              curve={curve}
              data={renderData}
              innerRef={pathRef}
              stroke={hasDashTail ? "transparent" : strokePaint}
              strokeLinecap="round"
              strokeWidth={strokeWidth}
              x={(d) => xScale(xAccessor(d)) ?? 0}
              y={getY}
            />
            <SeriesDashTailOverlay
              dashArray={dashArray}
              dashFromIndex={dashFromIndex}
              data={data}
              innerHeight={innerHeight}
              innerWidth={innerWidth}
              pathD={pathD}
              pathLength={pathLength}
              stroke={strokePaint}
              strokeWidth={strokeWidth}
              xAccessor={xAccessor}
              xScale={xScale}
            />
          </>
        ) : null}
      </SeriesHoverDim>

      {/* Highlight segment on hover — isolated hover subscriber. */}
      <SeriesHighlightLayer
        enabled={highlightEnabled}
        height={innerHeight}
        pathRef={pathRef}
        stroke={resolvedStroke}
        strokeWidth={strokeWidth}
      />

      {showMarkers ? (
        <SeriesMarkers
          animate={animate}
          dataKey={dataKey}
          {...markers}
          fill={markers?.fill ?? resolvedStroke}
          stroke={markers?.stroke ?? markers?.fill ?? resolvedStroke}
        />
      ) : null}
    </>
  );
}

Area.displayName = "Area";

export default Area;
```

##### File: `src/charts/pattern-area.tsx`

```tsx
"use client";

import { curveMonotoneX } from "@visx/curve";
import { AreaClosed } from "@visx/shape";
import { useChartStable } from "./chart-context";

// biome-ignore lint/suspicious/noExplicitAny: d3 curve factory type
type CurveFactory = any;

export interface PatternAreaProps {
  /** Key in data to use for y values */
  dataKey: string;
  /** Fill color or pattern URL (e.g. `url(#pattern-id)`) */
  fill: string;
  /** Curve function. Default: curveMonotoneX */
  curve?: CurveFactory;
  /** @deprecated Pattern fill is not clip-revealed; only the stroke `Area` animates. */
  animate?: boolean;
}

/**
 * Filled area using an SVG pattern (`url(#id)`).
 * Pair with `PatternLines` in `AreaChart` children and an `Area` with `fillOpacity={0}` for the stroke line.
 */
export function PatternArea({
  dataKey,
  fill,
  curve = curveMonotoneX,
}: PatternAreaProps) {
  const { data, xScale, yScale, xAccessor } = useChartStable();

  return (
    <AreaClosed
      curve={curve}
      data={data}
      fill={fill}
      x={(d) => xScale(xAccessor(d)) ?? 0}
      y={(d) => {
        const v = d[dataKey];
        return typeof v === "number" ? (yScale(v) ?? 0) : 0;
      }}
      yScale={yScale}
    />
  );
}

PatternArea.displayName = "PatternArea";

export default PatternArea;
```

---

### Bar Chart <a name="bar-chart"></a>

A composable bar chart with spring animations, stacked bars, horizontal orientation, and grouped series support

- **Category**: Charts
- **Registry Key**: `bar-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/bar-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `@visx/gradient@4.0.1-alpha.0`
  - `@visx/pattern@4.0.1-alpha.0`
  - `@visx/shape@4.0.1-alpha.0`
  - `motion`
- **Local Registry Dependencies**:
  - `@bklit/chart-context`
  - `@bklit/chart-animation`
  - `@bklit/grid`
  - `@bklit/chart-tooltip`
  - `@bklit/utils`

#### How to Use

##### Example 1

```tsx
import {
  BarChart,
  Bar,
  BarXAxis,
  Grid,
  ChartTooltip,
} from "@bklitui/ui/charts";

const data = [
  { month: "Jan", revenue: 12000, profit: 4500 },
  { month: "Feb", revenue: 15500, profit: 5200 },
  // ... more data
];

export default function RevenueChart() {
  return (
    <BarChart data={data} xDataKey="month">
      <Grid horizontal />
      <Bar dataKey="revenue" fill="var(--chart-line-primary)" lineCap="round" />
      <Bar
        dataKey="profit"
        fill="var(--chart-line-secondary)"
        lineCap="round"
      />
      <BarXAxis />
      <ChartTooltip />
    </BarChart>
  );
}
```

##### Example 2

```tsx
<Bar dataKey="revenue" animationType="grow" />
```

##### Example 3

```tsx
<Bar dataKey="revenue" animationType="fade" />
```

##### Example 4

```tsx
// Override automatic stagger with custom delay
<Bar dataKey="revenue" staggerDelay={0.02} />

// No stagger (all bars animate together)
<Bar dataKey="revenue" staggerDelay={0} />
```

##### Example 5

```tsx
// Rounded caps (default) - full rounding
<Bar dataKey="revenue" lineCap="round" />

// Square caps - no rounding
<Bar dataKey="revenue" lineCap="butt" />

// Custom radius in pixels
<Bar dataKey="revenue" lineCap={4} />
<Bar dataKey="revenue" lineCap={8} />
```

#### Props & API Reference

##### `BarChart` Props

| Prop                | Type                         | Default                                        | Description                                         |
| ------------------- | ---------------------------- | ---------------------------------------------- | --------------------------------------------------- |
| `data`              | `Record<string, unknown>[]`  | required                                       | Array of data points                                |
| `xDataKey`          | `string`                     | `"name"`                                       | Key in data for categorical axis values             |
| `margin`            | `Partial<Margin>`            | `{ top: 40, right: 40, bottom: 40, left: 40 }` | Chart margins                                       |
| `animationDuration` | `number`                     | `1100`                                         | Animation duration in ms                            |
| `aspectRatio`       | `string`                     | `"2 / 1"`                                      | CSS aspect ratio                                    |
| `barGap`            | `number`                     | `0.2`                                          | Gap between bar groups (0-1 fraction of band width) |
| `barWidth`          | `number`                     | -                                              | Fixed bar width in pixels (auto-sizes if not set)   |
| `orientation`       | `"vertical" \| "horizontal"` | `"vertical"`                                   | Bar chart orientation                               |
| `stacked`           | `boolean`                    | `false`                                        | Stack bars instead of grouping them                 |
| `stackGap`          | `number`                     | `0`                                            | Gap between stacked bar segments in pixels          |
| `className`         | `string`                     | `""`                                           | Additional CSS class                                |

##### `Bar` Props

| Prop            | Type                          | Default                     | Description                                             |
| --------------- | ----------------------------- | --------------------------- | ------------------------------------------------------- |
| `dataKey`       | `string`                      | required                    | Key in data for values                                  |
| `fill`          | `string`                      | `var(--chart-line-primary)` | Bar fill color (can be gradient/pattern url)            |
| `stroke`        | `string`                      | -                           | Tooltip dot color. Use when fill is a gradient/pattern  |
| `lineCap`       | `"round" \| "butt" \| number` | `"round"`                   | Bar end cap style or custom radius                      |
| `animate`       | `boolean`                     | `true`                      | Enable animation                                        |
| `animationType` | `"grow" \| "fade"`            | `"grow"`                    | Animation style                                         |
| `fadedOpacity`  | `number`                      | `0.3`                       | Opacity when another bar is hovered                     |
| `staggerDelay`  | `number`                      | auto                        | Delay between bars (auto-calculated based on bar count) |
| `stackGap`      | `number`                      | `0`                         | Gap between stacked bars in pixels                      |

##### `BarXAxis` Props

| Prop              | Type      | Default | Description                          |
| ----------------- | --------- | ------- | ------------------------------------ |
| `tickerHalfWidth` | `number`  | `50`    | Width of ticker for fade calculation |
| `showAllLabels`   | `boolean` | `false` | Show all labels (may crowd)          |
| `maxLabels`       | `number`  | `12`    | Maximum labels to show               |

##### `BarYAxis` Props

| Prop            | Type      | Default | Description            |
| --------------- | --------- | ------- | ---------------------- |
| `showAllLabels` | `boolean` | `true`  | Show all labels        |
| `maxLabels`     | `number`  | `20`    | Maximum labels to show |

##### `Grid` Props

| Prop             | Type      | Default | Description                               |
| ---------------- | --------- | ------- | ----------------------------------------- |
| `horizontal`     | `boolean` | `true`  | Show horizontal grid lines                |
| `vertical`       | `boolean` | `false` | Show vertical grid lines                  |
| `fadeHorizontal` | `boolean` | `true`  | Fade horizontal lines at left/right edges |
| `fadeVertical`   | `boolean` | `false` | Fade vertical lines at top/bottom edges   |

#### Component Source Code

##### File: `src/charts/bar-chart.tsx`

```tsx
"use client";

import { localPoint } from "@visx/event";
import { ParentSize } from "@visx/responsive";
import { scaleBand, scaleLinear } from "@visx/scale";
import type { Transition } from "motion/react";
import {
  Children,
  isValidElement,
  memo,
  type ReactElement,
  type ReactNode,
  useCallback,
  useEffect,
  useMemo,
  useRef,
  useState,
} from "react";
import { cn } from "@/lib/utils";
import { DEFAULT_ANIMATION_EASING } from "./animation";
import type { BarProps } from "./bar";
import {
  ChartProvider,
  type LineConfig,
  type Margin,
  type TooltipData,
} from "./chart-context";
import { isGradientDefComponent, isPatternDefComponent } from "./chart-defs";
import { shortDateFmt } from "./chart-formatters";
import { useScheduledTooltip } from "./use-scheduled-tooltip";

export type BarOrientation = "vertical" | "horizontal";

export interface BarChartProps {
  /** Data array - each item should have an x-axis key and numeric values */
  data: Record<string, unknown>[];
  /** Key in data for the categorical axis. Default: "name" */
  xDataKey?: string;
  /** Chart margins */
  margin?: Partial<Margin>;
  /** Animation duration in milliseconds. Default: 1100 */
  animationDuration?: number;
  /** CSS easing for bar grow transitions. */
  animationEasing?: string;
  /** Motion enter transition (spring or cubic-bezier tween). */
  enterTransition?: Transition;
  /** Signature of motion URL state — triggers enter replay when it changes. */
  revealSignature?: string;
  /** Aspect ratio as "width / height". Default: "2 / 1" */
  aspectRatio?: string;
  /** Additional class name for the container */
  className?: string;
  /** Gap between bar groups as a fraction of band width (0-1). Default: 0.2 */
  barGap?: number;
  /** Fixed bar width in pixels. If not set, bars auto-size to fill the band. */
  barWidth?: number;
  /** Bar chart orientation. Default: "vertical" */
  orientation?: BarOrientation;
  /** Whether to stack bars instead of grouping them. Default: false */
  stacked?: boolean;
  /** Gap between stacked bar segments in pixels. Default: 0 */
  stackGap?: number;
  /** Child components (Bar, Grid, ChartTooltip, etc.) */
  children: ReactNode;
}

const DEFAULT_MARGIN: Margin = { top: 40, right: 40, bottom: 40, left: 40 };

// Extract bar configs from children synchronously
function extractBarConfigs(children: ReactNode): LineConfig[] {
  const configs: LineConfig[] = [];

  Children.forEach(children, (child) => {
    if (!isValidElement(child)) {
      return;
    }

    const childType = child.type as {
      displayName?: string;
      name?: string;
    };
    const componentName =
      typeof child.type === "function"
        ? childType.displayName || childType.name || ""
        : "";

    const props = child.props as BarProps | undefined;
    const isBarComponent =
      componentName === "Bar" ||
      (props && typeof props.dataKey === "string" && props.dataKey.length > 0);

    if (isBarComponent && props?.dataKey) {
      // Use stroke for tooltip dot color if provided, otherwise fall back to fill
      // This allows gradient/pattern fills to have a solid dot color
      const dotColor =
        props.stroke || props.fill || "var(--chart-line-primary)";
      configs.push({
        dataKey: props.dataKey,
        stroke: dotColor,
        strokeWidth: 0,
      });
    }
  });

  return configs;
}

// Check if a component should render after the mouse overlay
function isPostOverlayComponent(child: ReactElement): boolean {
  const childType = child.type as {
    displayName?: string;
    name?: string;
    __isChartMarkers?: boolean;
  };

  if (childType.__isChartMarkers) {
    return true;
  }

  const componentName =
    typeof child.type === "function"
      ? childType.displayName || childType.name || ""
      : "";

  return componentName === "ChartMarkers" || componentName === "MarkerGroup";
}

interface ChartInnerProps {
  width: number;
  height: number;
  data: Record<string, unknown>[];
  xDataKey: string;
  margin: Margin;
  animationDuration: number;
  animationEasing: string;
  enterTransition?: Transition;
  revealSignature?: string;
  barGap: number;
  barWidthProp?: number;
  orientation: BarOrientation;
  stacked: boolean;
  stackGap: number;
  children: ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
}

function ChartInner(props: ChartInnerProps) {
  const { width, height } = props;
  if (width < 10 || height < 10) {
    return null;
  }
  return <ChartCore {...props} />;
}

const ChartCore = memo(function ChartCore({
  width,
  height,
  data,
  xDataKey,
  margin,
  animationDuration,
  animationEasing,
  enterTransition,
  revealSignature = "",
  barGap,
  barWidthProp,
  orientation,
  stacked,
  stackGap,
  children,
  containerRef,
}: ChartInnerProps) {
  const { tooltipData, setTooltipData, scheduleTooltip, clearTooltip } =
    useScheduledTooltip<TooltipData>();
  const [isLoaded, setIsLoaded] = useState(false);
  const [revealEpoch, setRevealEpoch] = useState(0);
  const hoveredBarIndex = tooltipData?.index ?? null;

  const isHorizontal = orientation === "horizontal";

  // Extract bar configs synchronously from children
  const lines = useMemo(() => extractBarConfigs(children), [children]);

  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  // Category accessor function - returns string for categorical scale
  const categoryAccessor = useCallback(
    (d: Record<string, unknown>): string => {
      const value = d[xDataKey];
      if (value instanceof Date) {
        return shortDateFmt.format(value);
      }
      return String(value ?? "");
    },
    [xDataKey]
  );

  // For compatibility with ChartContext, provide a Date-based xAccessor
  const xAccessorDate = useCallback(
    (d: Record<string, unknown>): Date => {
      const value = d[xDataKey];
      if (value instanceof Date) {
        return value;
      }
      return new Date();
    },
    [xDataKey]
  );

  // Category scale (band) - for the categorical axis
  const categoryScale = useMemo(() => {
    const domain = data.map((d) => categoryAccessor(d));
    const range: [number, number] = isHorizontal
      ? [0, innerHeight]
      : [0, innerWidth];
    return scaleBand<string>({
      range,
      domain,
      padding: barGap,
    });
  }, [innerWidth, innerHeight, data, categoryAccessor, barGap, isHorizontal]);

  // Band width for bars - use prop if provided, otherwise use scale's bandwidth
  const bandWidth = barWidthProp ?? categoryScale.bandwidth();

  // Compute max value considering stacking
  const maxValue = useMemo(() => {
    if (stacked) {
      // For stacked bars, sum all values at each data point
      let max = 0;
      for (const d of data) {
        let sum = 0;
        for (const line of lines) {
          const value = d[line.dataKey];
          if (typeof value === "number") {
            sum += value;
          }
        }
        if (sum > max) {
          max = sum;
        }
      }
      return max || 100;
    }
    // For grouped bars, find max single value
    let max = 0;
    for (const line of lines) {
      for (const d of data) {
        const value = d[line.dataKey];
        if (typeof value === "number" && value > max) {
          max = value;
        }
      }
    }
    return max || 100;
  }, [data, lines, stacked]);

  // Value scale (linear) - for the value axis
  const valueScale = useMemo(() => {
    const range = isHorizontal ? [0, innerWidth] : [innerHeight, 0];
    return scaleLinear({
      range,
      domain: [0, maxValue * 1.1],
      nice: true,
    });
  }, [innerWidth, innerHeight, maxValue, isHorizontal]);

  // Compute stack offsets for stacked bars
  const stackOffsets = useMemo(() => {
    if (!stacked) {
      return undefined;
    }
    const offsets = new Map<number, Map<string, number>>();
    for (let i = 0; i < data.length; i++) {
      const d = data[i];
      if (!d) {
        continue;
      }
      const pointOffsets = new Map<string, number>();
      let cumulative = 0;
      for (const line of lines) {
        pointOffsets.set(line.dataKey, cumulative);
        const value = d[line.dataKey];
        if (typeof value === "number") {
          cumulative += value;
        }
      }
      offsets.set(i, pointOffsets);
    }
    return offsets;
  }, [data, lines, stacked]);

  // Column width for tooltip indicator
  const columnWidth = useMemo(() => {
    if (data.length < 1) {
      return 0;
    }
    return isHorizontal ? innerHeight / data.length : innerWidth / data.length;
  }, [innerWidth, innerHeight, data.length, isHorizontal]);

  // Pre-compute labels for ticker animation
  const dateLabels = useMemo(
    () => data.map((d) => categoryAccessor(d)),
    [data, categoryAccessor]
  );

  // Create a fake time scale for compatibility with ChartContext
  const fakeTimeScale = useMemo(() => {
    const now = Date.now();
    const start = now - data.length * 24 * 60 * 60 * 1000;
    const scale = {
      ...categoryScale,
      domain: () => [new Date(start), new Date(now)],
      range: () => [0, innerWidth] as [number, number],
      invert: (x: number) => new Date(start + (x / innerWidth) * (now - start)),
      copy: () => scale,
    };
    return scale;
  }, [categoryScale, innerWidth, data.length]);

  // Animation timing — replay when motion settings change
  // biome-ignore lint/correctness/useExhaustiveDependencies: revealSignature
  useEffect(() => {
    setRevealEpoch((n) => n + 1);
    setIsLoaded(false);
    const timer = setTimeout(() => {
      setIsLoaded(true);
    }, animationDuration);
    return () => clearTimeout(timer);
  }, [animationDuration, revealSignature]);

  // Mouse move handler
  const handleMouseMove = useCallback(
    (event: React.MouseEvent<SVGGElement>) => {
      const point = localPoint(event);
      if (!point) {
        return;
      }

      const pos = isHorizontal ? point.y - margin.top : point.x - margin.left;

      // Find which band the mouse is over
      const bandIndex = Math.floor(pos / columnWidth);
      const clampedIndex = Math.max(0, Math.min(data.length - 1, bandIndex));
      const d = data[clampedIndex];

      if (!d) {
        return;
      }

      // Calculate positions for each bar
      const yPositions: Record<string, number> = {};
      const xPositions: Record<string, number> = {};
      const barPos = categoryScale(categoryAccessor(d)) ?? 0;

      if (isHorizontal) {
        // Horizontal bars: dots at end of bar (x = value), centered vertically in band
        const seriesCount = lines.length;
        const groupGap = seriesCount > 1 ? 4 : 0;
        const individualBarHeight =
          seriesCount > 0
            ? (bandWidth - groupGap * (seriesCount - 1)) / seriesCount
            : bandWidth;

        if (stacked) {
          // Stacked horizontal: all bars same y, x at cumulative end
          let cumulative = 0;
          for (const line of lines) {
            const value = d[line.dataKey];
            if (typeof value === "number") {
              cumulative += value;
              xPositions[line.dataKey] = valueScale(cumulative) ?? 0;
              yPositions[line.dataKey] = barPos + bandWidth / 2;
            }
          }
        } else {
          // Grouped horizontal: each bar at its own y position
          lines.forEach((line, idx) => {
            const value = d[line.dataKey];
            if (typeof value === "number") {
              xPositions[line.dataKey] = valueScale(value) ?? 0;
              yPositions[line.dataKey] =
                barPos +
                idx * (individualBarHeight + groupGap) +
                individualBarHeight / 2;
            }
          });
        }
      } else if (stacked) {
        // Vertical stacked bars
        let cumulative = 0;
        let seriesIdx = 0;
        for (const line of lines) {
          const value = d[line.dataKey];
          if (typeof value === "number") {
            cumulative += value;
            const gapOffset = seriesIdx * stackGap;
            yPositions[line.dataKey] =
              (valueScale(cumulative) ?? 0) - gapOffset;
            seriesIdx++;
          }
        }
      } else {
        // Vertical grouped bars
        const seriesCount = lines.length;
        const groupGap = seriesCount > 1 ? 4 : 0;
        const individualBarWidth =
          seriesCount > 0
            ? (bandWidth - groupGap * (seriesCount - 1)) / seriesCount
            : bandWidth;

        lines.forEach((line, idx) => {
          const value = d[line.dataKey];
          if (typeof value === "number") {
            yPositions[line.dataKey] = valueScale(value) ?? 0;
            xPositions[line.dataKey] =
              barPos +
              idx * (individualBarWidth + groupGap) +
              individualBarWidth / 2;
          }
        });
      }

      // Tooltip position: for horizontal, position at max bar end; for vertical, center of band
      let tooltipX: number;
      if (isHorizontal) {
        // Position tooltip at the end of the longest bar
        const maxX = Math.max(...Object.values(xPositions), 0);
        tooltipX = maxX;
      } else {
        tooltipX = barPos + bandWidth / 2;
      }

      scheduleTooltip({
        point: d,
        index: clampedIndex,
        x: tooltipX,
        yPositions,
        xPositions: Object.keys(xPositions).length > 0 ? xPositions : undefined,
      });
    },
    [
      categoryScale,
      valueScale,
      data,
      lines,
      margin.left,
      margin.top,
      categoryAccessor,
      columnWidth,
      bandWidth,
      isHorizontal,
      stacked,
      stackGap,
      scheduleTooltip,
    ]
  );

  const handleMouseLeave = useCallback(() => {
    clearTooltip();
  }, [clearTooltip]);

  const canInteract = isLoaded;

  // Separate children into defs, pre-overlay, and post-overlay
  const defsChildren: ReactElement[] = [];
  const preOverlayChildren: ReactElement[] = [];
  const postOverlayChildren: ReactElement[] = [];

  Children.forEach(children, (child) => {
    if (!isValidElement(child)) {
      return;
    }

    if (isGradientDefComponent(child)) {
      defsChildren.push(child);
    } else if (isPatternDefComponent(child)) {
      preOverlayChildren.push(child);
    } else if (isPostOverlayComponent(child)) {
      postOverlayChildren.push(child);
    } else {
      preOverlayChildren.push(child);
    }
  });

  const contextValue = {
    data,
    renderData: data,
    xScale: fakeTimeScale as unknown as ReturnType<
      typeof import("@visx/scale").scaleTime<number>
    >,
    yScale: valueScale,
    width,
    height,
    innerWidth,
    innerHeight,
    margin,
    columnWidth,
    tooltipData,
    setTooltipData,
    containerRef,
    lines,
    isLoaded,
    animationDuration,
    animationEasing,
    enterTransition,
    revealEpoch,
    xAccessor: xAccessorDate,
    dateLabels,
    // Bar-specific properties
    barScale: categoryScale,
    bandWidth,
    hoveredBarIndex,
    barXAccessor: categoryAccessor,
    orientation,
    stacked,
    stackOffsets,
  };

  return (
    <ChartProvider value={contextValue}>
      <svg aria-hidden="true" height={height} width={width}>
        {/* Gradient and pattern definitions */}
        {defsChildren.length > 0 && <defs>{defsChildren}</defs>}

        <rect fill="transparent" height={height} width={width} x={0} y={0} />

        {/* biome-ignore lint/a11y/noStaticElementInteractions: Chart interaction area */}
        <g
          onMouseLeave={canInteract ? handleMouseLeave : undefined}
          onMouseMove={canInteract ? handleMouseMove : undefined}
          style={{ cursor: canInteract ? "crosshair" : "default" }}
          transform={`translate(${margin.left},${margin.top})`}
        >
          {/* Background rect for mouse event detection */}
          <rect
            fill="transparent"
            height={innerHeight}
            width={innerWidth}
            x={0}
            y={0}
          />

          {/* SVG children rendered before markers */}
          {preOverlayChildren}

          {/* Markers rendered last so they're on top for interaction */}
          {postOverlayChildren}
        </g>
      </svg>
    </ChartProvider>
  );
});

export function BarChart({
  data,
  xDataKey = "name",
  margin: marginProp,
  animationDuration = 1100,
  animationEasing = DEFAULT_ANIMATION_EASING,
  enterTransition,
  revealSignature,
  aspectRatio = "2 / 1",
  className = "",
  barGap = 0.2,
  barWidth,
  orientation = "vertical",
  stacked = false,
  stackGap = 0,
  children,
}: BarChartProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const margin = { ...DEFAULT_MARGIN, ...marginProp };

  return (
    <div
      className={cn("relative w-full", className)}
      ref={containerRef}
      style={{ aspectRatio }}
    >
      <ParentSize debounceTime={10}>
        {({ width, height }) => (
          <ChartInner
            animationDuration={animationDuration}
            animationEasing={animationEasing}
            barGap={barGap}
            barWidthProp={barWidth}
            containerRef={containerRef}
            data={data}
            enterTransition={enterTransition}
            height={height}
            margin={margin}
            orientation={orientation}
            revealSignature={revealSignature}
            stacked={stacked}
            stackGap={stackGap}
            width={width}
            xDataKey={xDataKey}
          >
            {children}
          </ChartInner>
        )}
      </ParentSize>
    </div>
  );
}

BarChart.displayName = "BarChart";

export default BarChart;
```

##### File: `src/charts/bar.tsx`

```tsx
"use client";

import type { scaleBand } from "@visx/scale";
import type { Transition } from "motion/react";
import { motion } from "motion/react";
import { memo, useId, useMemo } from "react";
import { chartCssVars, useChart, useChartStable } from "./chart-context";
import { transitionWithDelay } from "./motion-utils";

type ScaleBand<Domain extends { toString(): string }> = ReturnType<
  typeof scaleBand<Domain>
>;

export type BarLineCap = "round" | "butt" | number;
export type BarAnimationType = "grow" | "fade";

export interface BarProps {
  /** Key in data to use for y values */
  dataKey: string;
  /** Fill color for the bar. Can be a color, gradient url, or pattern url. Default: var(--chart-line-primary) */
  fill?: string;
  /** Color for tooltip dot. Use when fill is a gradient/pattern. Default: uses fill value */
  stroke?: string;
  /** Line cap style for bar ends: "round", "butt", or a number for custom radius. Default: "round" */
  lineCap?: BarLineCap;
  /** Whether to animate the bars. Default: true */
  animate?: boolean;
  /** Animation type: "grow" (height) or "fade" (opacity + blur). Default: "grow" */
  animationType?: BarAnimationType;
  /** Opacity when not hovered (when another bar is hovered). Default: 0.3 */
  fadedOpacity?: number;
  /** Stagger delay between bars in seconds. Auto-calculated if not provided. */
  staggerDelay?: number;
  /** Gap between stacked bars in pixels. Default: 0 */
  stackGap?: number;
  /** Gap between grouped bars in pixels. Default: 4 */
  groupGap?: number;
}

interface BarInnerProps extends BarProps {
  barScale: ScaleBand<string>;
  bandWidth: number;
  barXAccessor: (d: Record<string, unknown>) => string;
}

interface AnimatedBarProps {
  x: number;
  y: number;
  width: number;
  height: number;
  fill: string;
  rx: number;
  ry: number;
  index: number;
  isFaded: boolean;
  animationType: BarAnimationType;
  innerHeight: number;
  fadedOpacity: number;
  staggerDelay: number;
  enterTransition?: Transition;
  revealEpoch: number;
  isHorizontal: boolean;
}

function AnimatedBar({
  x,
  y,
  width,
  height,
  fill,
  rx,
  ry,
  index,
  isFaded,
  animationType,
  innerHeight,
  fadedOpacity,
  staggerDelay,
  enterTransition,
  revealEpoch,
  isHorizontal,
}: AnimatedBarProps) {
  const enterAnim = transitionWithDelay(enterTransition, index * staggerDelay);

  if (animationType === "fade") {
    return (
      <motion.rect
        animate={{
          opacity: isFaded ? fadedOpacity : 1,
          filter: "blur(0px)",
        }}
        fill={fill}
        height={height}
        initial={{ opacity: 0, filter: "blur(2px)" }}
        key={`fade-${index}-${revealEpoch}`}
        rx={rx}
        ry={ry}
        transition={enterAnim}
        width={width}
        x={x}
        y={y}
      />
    );
  }

  const initial = isHorizontal
    ? { width: 0, height, x: 0, y }
    : { width, height: 0, x, y: innerHeight };
  const target = isHorizontal
    ? { width, height, x: 0, y }
    : { width, height, x, y };

  return (
    <g
      opacity={isFaded ? fadedOpacity : 1}
      style={{ transition: "opacity 0.15s ease-in-out" }}
    >
      <motion.rect
        animate={target}
        fill={fill}
        initial={initial}
        key={`grow-${index}-${revealEpoch}`}
        rx={rx}
        ry={ry}
        transition={enterAnim}
      />
    </g>
  );
}

const BarInner = memo(function BarInner({
  dataKey,
  fill = chartCssVars.linePrimary,
  lineCap = "round",
  animate = true,
  animationType = "grow",
  fadedOpacity = 0.3,
  staggerDelay,
  stackGap = 0,
  groupGap = 4,
  barScale,
  bandWidth,
  barXAccessor,
}: BarInnerProps) {
  const {
    data,
    yScale,
    innerHeight,
    isLoaded,
    hoveredBarIndex,
    lines,
    orientation,
    stacked,
    stackOffsets,
    animationDuration,
    enterTransition,
    revealEpoch = 0,
  } = useChart();

  // Calculate stagger delay automatically if not provided
  // Total animation duration is ~1200ms, with 40% for stagger spread and 60% for bar animation
  const totalAnimDuration = animationDuration || 1100;
  const staggerSpread = totalAnimDuration * 0.4; // 40% of time for stagger spread
  const calculatedStaggerDelay =
    staggerDelay ?? (data.length > 1 ? staggerSpread / 1000 / data.length : 0);
  const uniqueId = useId();

  const isHorizontal = orientation === "horizontal";

  // Find the index of this bar series among all bar series
  const seriesIndex = useMemo(() => {
    const idx = lines.findIndex((l) => l.dataKey === dataKey);
    return idx >= 0 ? idx : 0;
  }, [lines, dataKey]);

  const seriesCount = lines.length;
  const isLastSeries = seriesIndex === seriesCount - 1;

  // Calculate the width for each bar within a group (for non-stacked)
  const barWidth = useMemo(() => {
    if (!bandWidth || seriesCount === 0) {
      return 0;
    }
    if (stacked) {
      // Stacked bars use full band width
      return bandWidth;
    }
    // Leave a gap between grouped bars (controlled by groupGap prop)
    const effectiveGroupGap = seriesCount > 1 ? groupGap : 0;
    return (bandWidth - effectiveGroupGap * (seriesCount - 1)) / seriesCount;
  }, [bandWidth, seriesCount, stacked, groupGap]);

  // Calculate corner radius based on lineCap
  const cornerRadius = useMemo(() => {
    if (typeof lineCap === "number") {
      return lineCap;
    }
    if (lineCap === "round" && barWidth) {
      return Math.min(barWidth / 2, 8);
    }
    return 0;
  }, [lineCap, barWidth]);

  return (
    <g className={`bar-series-${uniqueId}`}>
      {data.map((d, i) => {
        const value = d[dataKey];
        if (typeof value !== "number") {
          return null;
        }

        const categoryValue = barXAccessor(d);
        const bandPos = barScale(categoryValue) ?? 0;

        let x: number;
        let y: number;
        let barHeight: number;
        let barW: number;

        if (isHorizontal) {
          // Horizontal bars: category on y-axis, value on x-axis
          const valuePos = yScale(value) ?? 0;
          barW = valuePos; // Width is the value position (grows from left)
          barHeight = barWidth;

          if (stacked && stackOffsets) {
            const offset = stackOffsets.get(i)?.get(dataKey) ?? 0;
            x = yScale(offset) ?? 0;
            barW = valuePos - x;
            // Apply stack gap for horizontal: shift right and reduce width
            const gapOffset = seriesIndex * stackGap;
            x += gapOffset;
            if (!isLastSeries && stackGap > 0) {
              barW = Math.max(0, barW - stackGap);
            }
          } else {
            x = 0;
            // For grouped bars, offset y position
            const effectiveGroupGap = seriesCount > 1 ? groupGap : 0;
            y = bandPos + seriesIndex * (barWidth + effectiveGroupGap);
          }
          y = stacked
            ? bandPos
            : bandPos +
              seriesIndex * (barWidth + (seriesCount > 1 ? groupGap : 0));
        } else {
          // Vertical bars: category on x-axis, value on y-axis
          const valuePos = yScale(value) ?? 0;
          barHeight = innerHeight - valuePos;
          barW = barWidth;

          if (stacked && stackOffsets) {
            const offset = stackOffsets.get(i)?.get(dataKey) ?? 0;
            const offsetY = yScale(offset) ?? innerHeight;
            // Apply stack gap: shift up and reduce height
            const gapOffset = seriesIndex * stackGap;
            y = offsetY - barHeight - gapOffset;
            // Reduce height slightly for non-last bars to create visual gap
            if (!isLastSeries && stackGap > 0) {
              barHeight = Math.max(0, barHeight - stackGap);
            }
          } else {
            y = valuePos;
            // For grouped bars, offset x position
            const effectiveGroupGap = seriesCount > 1 ? groupGap : 0;
            x = bandPos + seriesIndex * (barWidth + effectiveGroupGap);
          }
          x = stacked
            ? bandPos
            : bandPos +
              seriesIndex * (barWidth + (seriesCount > 1 ? groupGap : 0));
        }

        const isFaded = hoveredBarIndex !== null && hoveredBarIndex !== i;

        // Use categoryValue as key since it's the unique identifier from data
        const barKey = `bar-${dataKey}-${categoryValue}`;

        // Apply rounded corners:
        // - For non-stacked: always apply
        // - For stacked with gap: apply to all bars
        // - For stacked without gap: only apply to the last series
        const applyRounding = !stacked || stackGap > 0 || isLastSeries;
        const effectiveRx = applyRounding ? cornerRadius : 0;
        const effectiveRy = applyRounding ? cornerRadius : 0;

        if (animate && !isLoaded) {
          return (
            <AnimatedBar
              animationType={animationType}
              enterTransition={enterTransition}
              fadedOpacity={fadedOpacity}
              fill={fill}
              height={barHeight}
              index={i}
              innerHeight={innerHeight}
              isFaded={isFaded}
              isHorizontal={isHorizontal}
              key={barKey}
              revealEpoch={revealEpoch}
              rx={effectiveRx}
              ry={effectiveRy}
              staggerDelay={calculatedStaggerDelay}
              width={barW}
              x={x}
              y={y}
            />
          );
        }

        // Static bar after animation completes
        return (
          <rect
            fill={fill}
            height={barHeight}
            key={barKey}
            opacity={isFaded ? fadedOpacity : 1}
            rx={effectiveRx}
            ry={effectiveRy}
            style={{
              cursor: "default",
              transition: "opacity 0.15s ease-in-out",
            }}
            width={barW}
            x={x}
            y={y}
          />
        );
      })}
    </g>
  );
});

export function Bar(props: BarProps) {
  const { barScale, bandWidth, barXAccessor } = useChartStable();

  if (!(barScale && bandWidth && barXAccessor)) {
    console.warn("Bar component must be used within a BarChart");
    return null;
  }

  return (
    <BarInner
      {...props}
      bandWidth={bandWidth}
      barScale={barScale}
      barXAccessor={barXAccessor}
    />
  );
}

Bar.displayName = "Bar";

export default Bar;
```

##### File: `src/charts/bar-x-axis.tsx`

```tsx
"use client";

import { motion } from "motion/react";
import { memo, useEffect, useMemo, useState } from "react";
import { createPortal } from "react-dom";
import { cn } from "@/lib/utils";
import { useChart, useChartStable } from "./chart-context";

export interface BarXAxisProps {
  /** Width of the date ticker box for fade calculation. Default: 50 */
  tickerHalfWidth?: number;
  /** Whether to show all labels or skip some for dense data. Default: false */
  showAllLabels?: boolean;
  /** Maximum number of labels to show. Default: 12 */
  maxLabels?: number;
}

interface BarXAxisLabelProps {
  label: string;
  x: number;
  crosshairX: number | null;
  isHovering: boolean;
  tickerHalfWidth: number;
}

function BarXAxisLabel({
  label,
  x,
  crosshairX,
  isHovering,
  tickerHalfWidth,
}: BarXAxisLabelProps) {
  const fadeBuffer = 20;
  const fadeRadius = tickerHalfWidth + fadeBuffer;

  let opacity = 1;
  if (isHovering && crosshairX !== null) {
    const distance = Math.abs(x - crosshairX);
    if (distance < tickerHalfWidth) {
      opacity = 0;
    } else if (distance < fadeRadius) {
      opacity = (distance - tickerHalfWidth) / fadeBuffer;
    }
  }

  // Zero-width container approach for perfect centering
  return (
    <div
      className="absolute"
      style={{
        left: x,
        bottom: 12,
        width: 0,
        display: "flex",
        justifyContent: "center",
      }}
    >
      <motion.span
        animate={{ opacity }}
        className={cn("whitespace-nowrap text-chart-label text-xs")}
        initial={{ opacity: 1 }}
        transition={{ duration: 0.4, ease: "easeInOut" }}
      >
        {label}
      </motion.span>
    </div>
  );
}

export function BarXAxis(props: BarXAxisProps) {
  const { containerRef, barScale } = useChartStable();
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    setMounted(true);
  }, []);

  const container = containerRef.current;
  if (!(mounted && container)) {
    return null;
  }

  if (!barScale) {
    return null;
  }

  return <BarXAxisInner {...props} container={container} />;
}

const BarXAxisInner = memo(function BarXAxisInner({
  tickerHalfWidth = 50,
  showAllLabels = false,
  maxLabels = 12,
  container,
}: BarXAxisProps & { container: HTMLDivElement }) {
  const { margin, tooltipData, barScale, bandWidth, barXAccessor, data } =
    useChart();

  // Generate labels for each bar
  const labelsToShow = useMemo(() => {
    if (!(barScale && bandWidth && barXAccessor)) {
      return [];
    }

    const allLabels = data.map((d) => {
      const label = barXAccessor(d);
      const bandX = barScale(label) ?? 0;
      // Center the label under the bar group
      const x = bandX + bandWidth / 2 + margin.left;
      return { label, x };
    });

    // If showAllLabels is true or we have fewer than maxLabels, show all
    if (showAllLabels || allLabels.length <= maxLabels) {
      return allLabels;
    }

    // Otherwise, skip some labels to avoid crowding
    const step = Math.ceil(allLabels.length / maxLabels);
    return allLabels.filter((_, i) => i % step === 0);
  }, [
    barScale,
    bandWidth,
    barXAccessor,
    data,
    margin.left,
    showAllLabels,
    maxLabels,
  ]);

  const isHovering = tooltipData !== null;
  const crosshairX = tooltipData ? tooltipData.x + margin.left : null;

  return createPortal(
    <div className="pointer-events-none absolute inset-0">
      {labelsToShow.map((item) => (
        <BarXAxisLabel
          crosshairX={crosshairX}
          isHovering={isHovering}
          key={`${item.label}-${item.x}`}
          label={item.label}
          tickerHalfWidth={tickerHalfWidth}
          x={item.x}
        />
      ))}
    </div>,
    container
  );
});

BarXAxis.displayName = "BarXAxis";

export default BarXAxis;
```

##### File: `src/charts/bar-y-axis.tsx`

```tsx
"use client";

import { motion } from "motion/react";
import { memo, useEffect, useMemo, useState } from "react";
import { createPortal } from "react-dom";
import { cn } from "@/lib/utils";
import { useChart, useChartStable } from "./chart-context";

export interface BarYAxisProps {
  /** Whether to show all labels or skip some for dense data. Default: true */
  showAllLabels?: boolean;
  /** Maximum number of labels to show. Default: 20 */
  maxLabels?: number;
}

interface BarYAxisLabelProps {
  label: string;
  y: number;
  bandHeight: number;
  isHovered: boolean;
}

function BarYAxisLabel({
  label,
  y,
  bandHeight,
  isHovered,
}: BarYAxisLabelProps) {
  return (
    <div
      className="absolute right-0 flex items-center justify-end pr-2"
      style={{
        top: y,
        height: bandHeight,
      }}
    >
      <motion.span
        animate={{
          opacity: isHovered ? 1 : 0.7,
          color: isHovered
            ? "var(--foreground)"
            : "var(--chart-label, var(--color-zinc-500))",
        }}
        className={cn("truncate whitespace-nowrap text-right text-xs")}
        initial={{
          opacity: 0.7,
          color: "var(--chart-label, var(--color-zinc-500))",
        }}
        style={{ maxWidth: 70 }}
        transition={{ duration: 0.15 }}
      >
        {label}
      </motion.span>
    </div>
  );
}

export function BarYAxis(props: BarYAxisProps) {
  const { containerRef, barScale } = useChartStable();
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    setMounted(true);
  }, []);

  const container = containerRef.current;
  if (!(mounted && container)) {
    return null;
  }

  if (!barScale) {
    return null;
  }

  return <BarYAxisInner {...props} container={container} />;
}

const BarYAxisInner = memo(function BarYAxisInner({
  showAllLabels = true,
  maxLabels = 20,
  container,
}: BarYAxisProps & { container: HTMLDivElement }) {
  const { margin, barScale, bandWidth, barXAccessor, data, hoveredBarIndex } =
    useChart();

  // Generate labels for each bar
  const labelsToShow = useMemo(() => {
    if (!(barScale && bandWidth && barXAccessor)) {
      return [];
    }

    const allLabels = data.map((d, i) => {
      const label = barXAccessor(d);
      const bandY = barScale(label) ?? 0;
      // Center the label vertically within the band
      const y = bandY + margin.top;
      return { label, y, bandHeight: bandWidth, index: i };
    });

    // If showAllLabels is true or we have fewer than maxLabels, show all
    if (showAllLabels || allLabels.length <= maxLabels) {
      return allLabels;
    }

    // Otherwise, skip some labels to avoid crowding
    const step = Math.ceil(allLabels.length / maxLabels);
    return allLabels.filter((_, i) => i % step === 0);
  }, [
    barScale,
    bandWidth,
    barXAccessor,
    data,
    margin.top,
    showAllLabels,
    maxLabels,
  ]);

  return createPortal(
    <div
      className="pointer-events-none absolute top-0 bottom-0"
      style={{
        left: 0,
        width: margin.left,
      }}
    >
      {labelsToShow.map((item) => (
        <BarYAxisLabel
          bandHeight={item.bandHeight}
          isHovered={hoveredBarIndex === item.index}
          key={`${item.label}-${item.y}`}
          label={item.label}
          y={item.y}
        />
      ))}
    </div>,
    container
  );
});

BarYAxis.displayName = "BarYAxis";

export default BarYAxis;
```

---

### Candlestick Chart <a name="candlestick-chart"></a>

A composable OHLC candlestick chart with gradients, patterns, tooltips, and hover interactions

- **Category**: Charts
- **Registry Key**: `candlestick-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/candlestick-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `@visx/scale@4.0.1-alpha.0`
  - `@visx/responsive@4.0.1-alpha.0`
  - `d3-array`
  - `motion`
- **Local Registry Dependencies**:
  - `@bklit/chart-context`
  - `@bklit/chart-animation`
  - `@bklit/grid`
  - `@bklit/x-axis`
  - `@bklit/y-axis`
  - `@bklit/chart-tooltip`
  - `@bklit/utils`

#### How to Use

```tsx
import {
  CandlestickChart,
  Candlestick,
  Grid,
  ChartTooltip,
  XAxis,
} from "@bklitui/ui/charts";

const ohlcData = [
  { date: new Date("2025-01-01"), open: 100, high: 108, low: 96, close: 104 },
  { date: new Date("2025-01-02"), open: 104, high: 112, low: 101, close: 109 },
  // ... more OHLC points
];

function OHLCChart() {
  return (
    <CandlestickChart
      data={ohlcData}
      margin={{ top: 16, right: 16, bottom: 40, left: 16 }}
      style={{ height: 320 }}
    >
      <Grid horizontal />
      <Candlestick fadedOpacity={0.25} />
      <ChartTooltip content={MyOHLCTooltipContent} />
      <XAxis />
    </CandlestickChart>
  );
}
```

#### Component Source Code

##### File: `src/charts/candlestick-chart.tsx`

```tsx
"use client";

import { ParentSize } from "@visx/responsive";
import { scaleLinear, scaleTime } from "@visx/scale";
import { bisector } from "d3-array";
import type { Transition } from "motion/react";
import {
  Children,
  isValidElement,
  memo,
  type ReactElement,
  type ReactNode,
  useCallback,
  useEffect,
  useMemo,
  useRef,
  useState,
} from "react";
import { cn } from "@/lib/utils";
import { ChartProvider, type LineConfig, type Margin } from "./chart-context";
import { shortDateFmt } from "./chart-formatters";
import {
  decimateOhlcData,
  maxRenderPointsForWidth,
} from "./decimate-time-series";
import { useChartInteraction } from "./use-chart-interaction";

export interface OHLCDataPoint {
  date: Date;
  open: number;
  high: number;
  low: number;
  close: number;
}

export interface CandlestickChartProps {
  /** OHLC data array */
  data: OHLCDataPoint[];
  /** Key in data for the x-axis (date). Default: "date" */
  xDataKey?: string;
  /** Chart margins */
  margin?: Partial<Margin>;
  /** Animation duration in milliseconds. Default: 1500 */
  animationDuration?: number;
  /** Motion enter transition (spring or cubic-bezier tween). */
  enterTransition?: Transition;
  /** Signature of motion URL state — triggers enter replay when it changes. */
  revealSignature?: string;
  /** Aspect ratio as "width / height". Default: "2 / 1" */
  aspectRatio?: string;
  /** Additional class name for the container */
  className?: string;
  /** Inline styles for the container (e.g. { height: 320 }) */
  style?: React.CSSProperties;
  /** Gap between candles as fraction of slot width (0–1). Default: 0.2. Ignored when candleWidth is set. */
  candleGap?: number;
  /** Fixed candle body width in pixels. If set, overrides candleGap. */
  candleWidth?: number;
  /** When set, xScale uses this domain instead of deriving from data. Use with brush so main chart and strip share the same scale. */
  xDomain?: [Date, Date];
  /** When xDomain is set, use this as the number of slots for scale padding (e.g. full data length). */
  xDomainSlotCount?: number;
  /** Child components (Candlestick, Grid, XAxis, YAxis, ChartTooltip, etc.) */
  children: ReactNode;
}

const DEFAULT_MARGIN: Margin = { top: 40, right: 40, bottom: 40, left: 40 };

interface ChartInnerProps {
  width: number;
  height: number;
  data: Record<string, unknown>[];
  xDataKey: string;
  margin: Margin;
  animationDuration: number;
  enterTransition?: Transition;
  revealSignature?: string;
  candleGap: number;
  candleWidthProp?: number;
  xDomain?: [Date, Date];
  xDomainSlotCount?: number;
  children: ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
}

function ChartInner(props: ChartInnerProps) {
  const { width, height } = props;
  if (width < 10 || height < 10) {
    return null;
  }
  return <ChartCore {...props} />;
}

const ChartCore = memo(function ChartCore({
  width,
  height,
  data,
  xDataKey,
  margin,
  animationDuration,
  enterTransition,
  revealSignature = "",
  candleGap,
  candleWidthProp,
  xDomain,
  xDomainSlotCount,
  children,
  containerRef,
}: ChartInnerProps) {
  const [isLoaded, setIsLoaded] = useState(false);
  const [revealEpoch, setRevealEpoch] = useState(0);

  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  const xAccessor = useCallback(
    (d: Record<string, unknown>): Date => {
      const value = d[xDataKey];
      return value instanceof Date ? value : new Date(value as string | number);
    },
    [xDataKey]
  );

  const bisectDate = useMemo(
    () => bisector<Record<string, unknown>, Date>((d) => xAccessor(d)).left,
    [xAccessor]
  );

  const slotCount =
    xDomain && xDomainSlotCount != null ? xDomainSlotCount : data.length;
  const slotWidth = innerWidth / Math.max(slotCount, 1);
  const xScale = useMemo(() => {
    const minTime = xDomain
      ? xDomain[0].getTime()
      : Math.min(...data.map((d) => xAccessor(d).getTime()));
    const maxTime = xDomain
      ? xDomain[1].getTime()
      : Math.max(...data.map((d) => xAccessor(d).getTime()));
    const padding = slotWidth / 2;
    return scaleTime({
      range: [padding, innerWidth - padding],
      domain: [minTime, maxTime],
    });
  }, [innerWidth, data, xAccessor, slotWidth, xDomain]);

  const yScale = useMemo(() => {
    let minVal = Number.POSITIVE_INFINITY;
    let maxVal = Number.NEGATIVE_INFINITY;
    for (const d of data) {
      const low = d.low as number | undefined;
      const high = d.high as number | undefined;
      if (typeof low === "number" && low < minVal) {
        minVal = low;
      }
      if (typeof high === "number" && high > maxVal) {
        maxVal = high;
      }
    }
    if (minVal === Number.POSITIVE_INFINITY) {
      minVal = 0;
    }
    if (maxVal === Number.NEGATIVE_INFINITY) {
      maxVal = 100;
    }
    const padding = (maxVal - minVal) * 0.05 || 1;
    return scaleLinear({
      range: [innerHeight, 0],
      domain: [minVal - padding, maxVal + padding],
      nice: true,
    });
  }, [innerHeight, data]);

  const columnWidth = slotWidth;
  const bandWidth = candleWidthProp ?? slotWidth * (1 - candleGap);

  const lines: LineConfig[] = useMemo(
    () => [
      { dataKey: "close", stroke: "var(--chart-line-primary)", strokeWidth: 0 },
    ],
    []
  );

  const renderData = useMemo(
    () => decimateOhlcData(data, maxRenderPointsForWidth(innerWidth)),
    [data, innerWidth]
  );

  const dateLabels = useMemo(
    () => data.map((d) => shortDateFmt.format(xAccessor(d))),
    [data, xAccessor]
  );

  // biome-ignore lint/correctness/useExhaustiveDependencies: revealSignature
  useEffect(() => {
    setRevealEpoch((n) => n + 1);
    setIsLoaded(false);
    const timer = setTimeout(() => setIsLoaded(true), animationDuration);
    return () => clearTimeout(timer);
  }, [animationDuration, revealSignature]);

  const {
    tooltipData,
    setTooltipData,
    selection,
    clearSelection,
    interactionHandlers,
    interactionStyle,
  } = useChartInteraction({
    xScale,
    yScale,
    data,
    lines,
    margin,
    xAccessor,
    bisectDate,
    canInteract: isLoaded,
  });

  const hoveredCandleIndex = tooltipData?.index ?? null;

  const isDefsComponent = (child: ReactElement): boolean => {
    const displayName =
      (child.type as { displayName?: string })?.displayName ||
      (child.type as { name?: string })?.name ||
      "";
    return (
      displayName.includes("Gradient") ||
      displayName.includes("Pattern") ||
      displayName === "LinearGradient" ||
      displayName === "RadialGradient" ||
      displayName === "Lines" ||
      displayName === "PatternLines"
    );
  };

  const defsChildren: ReactElement[] = [];
  const restChildren: ReactElement[] = [];
  Children.forEach(children, (child) => {
    if (!isValidElement(child)) {
      return;
    }
    if (isDefsComponent(child)) {
      defsChildren.push(child);
    } else {
      restChildren.push(child);
    }
  });

  const contextValue = {
    data,
    renderData,
    xScale,
    yScale,
    width,
    height,
    innerWidth,
    innerHeight,
    margin,
    columnWidth,
    tooltipData,
    setTooltipData,
    containerRef,
    lines,
    isLoaded,
    animationDuration,
    enterTransition,
    revealEpoch,
    xAccessor,
    dateLabels,
    selection: selection ?? null,
    clearSelection,
    bandWidth,
    hoveredCandleIndex,
  };

  return (
    <ChartProvider value={contextValue}>
      <svg aria-hidden="true" height={height} width={width}>
        <defs>
          {/* Default vertical gradients for positive/negative candles (chart-1 / chart-5) */}
          <linearGradient id="candlestick-positive" x1="0" x2="0" y1="1" y2="0">
            <stop offset="0%" stopColor="var(--chart-1)" />
            <stop offset="100%" stopColor="var(--chart-1)" />
          </linearGradient>
          <linearGradient id="candlestick-negative" x1="0" x2="0" y1="1" y2="0">
            <stop offset="0%" stopColor="var(--chart-5)" />
            <stop offset="100%" stopColor="var(--chart-5)" />
          </linearGradient>
          {defsChildren}
        </defs>
        <rect fill="transparent" height={height} width={width} x={0} y={0} />
        <g
          {...interactionHandlers}
          style={interactionStyle}
          transform={`translate(${margin.left},${margin.top})`}
        >
          <rect
            fill="transparent"
            height={innerHeight}
            width={innerWidth}
            x={0}
            y={0}
          />
          {restChildren}
        </g>
      </svg>
    </ChartProvider>
  );
});

export function CandlestickChart({
  data,
  xDataKey = "date",
  margin: marginProp,
  animationDuration = 1100,
  enterTransition,
  revealSignature,
  aspectRatio = "2 / 1",
  className = "",
  style,
  candleGap = 0.2,
  candleWidth,
  xDomain,
  xDomainSlotCount,
  children,
}: CandlestickChartProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const margin = { ...DEFAULT_MARGIN, ...marginProp };
  const dataAsRecords = data as unknown as Record<string, unknown>[];

  return (
    <div
      className={cn("relative w-full", className)}
      ref={containerRef}
      style={{ aspectRatio, touchAction: "none", ...style }}
    >
      <ParentSize debounceTime={10}>
        {({ width, height }) => (
          <ChartInner
            animationDuration={animationDuration}
            candleGap={candleGap}
            candleWidthProp={candleWidth}
            containerRef={containerRef}
            data={dataAsRecords}
            enterTransition={enterTransition}
            height={height}
            margin={margin}
            revealSignature={revealSignature}
            width={width}
            xDataKey={xDataKey}
            xDomain={xDomain}
            xDomainSlotCount={xDomainSlotCount}
          >
            {children}
          </ChartInner>
        )}
      </ParentSize>
    </div>
  );
}

CandlestickChart.displayName = "CandlestickChart";

export default CandlestickChart;
```

##### File: `src/charts/candlestick.tsx`

```tsx
"use client";

import type { Transition } from "motion/react";
import { motion } from "motion/react";
import { memo, useMemo } from "react";
import { useChart } from "./chart-context";
import { transitionWithDelay } from "./motion-utils";

const DEFAULT_POSITIVE = "url(#candlestick-positive)";
const DEFAULT_NEGATIVE = "url(#candlestick-negative)";

const SOLID_POSITIVE = "var(--chart-1)";
const SOLID_NEGATIVE = "var(--chart-5)";
const WICK_WIDTH = 1.5;

export interface CandlestickProps {
  /** Whether to animate the candlesticks. Default: true */
  animate?: boolean;
  /** Fill for positive (close >= open) candles. Color or url(#gradient). Default: --chart-1 */
  positiveFill?: string;
  /** Fill for negative candles. Color or url(#gradient). Default: --chart-5 */
  negativeFill?: string;
  /** Optional pattern URL for body only (e.g. url(#pattern)). When set, body is drawn solid first, then pattern overlaid and masked to the body rect. */
  bodyPatternPositive?: string;
  /** Optional pattern URL for negative candle body. */
  bodyPatternNegative?: string;
  /** Inner border width on the body (drawn inside so it does not expand the shape). Default: 0 (off). */
  insideStrokeWidth?: number;
  /** Opacity when another candle is hovered. Default: 0.3 */
  fadedOpacity?: number;
  /** Dim non-hovered candles on hover. Default: true */
  showHoverFade?: boolean;
}

interface CandleGeometry {
  time: number;
  centerX: number;
  bodyTop: number;
  bodyHeight: number;
  bodyLeft: number;
  candleWidth: number;
  wickTop: number;
  wickHeight: number;
  wickLeft: number;
  bodySolidFill: string;
  wickFill: string;
  bodyPattern?: string;
  insideStrokeWidth: number;
}

function getSolidColor(isPositive: boolean): string {
  return isPositive ? SOLID_POSITIVE : SOLID_NEGATIVE;
}

function computeGeometries(
  renderData: Record<string, unknown>[],
  xScale: (value: Date) => number | undefined,
  yScale: (value: number) => number | undefined,
  xAccessor: (d: Record<string, unknown>) => Date,
  candleWidth: number,
  positiveFill: string,
  negativeFill: string,
  bodyPatternPositive: string | undefined,
  bodyPatternNegative: string | undefined,
  insideStrokeWidth: number
): CandleGeometry[] {
  return renderData.map((d) => {
    const date = xAccessor(d);
    const open = d.open as number;
    const high = d.high as number;
    const low = d.low as number;
    const close = d.close as number;
    const centerX = xScale(date) ?? 0;
    const yHigh = yScale(high) ?? 0;
    const yLow = yScale(low) ?? 0;
    const yOpen = yScale(open) ?? 0;
    const yClose = yScale(close) ?? 0;
    const bodyTop = Math.min(yOpen, yClose);
    const bodyHeight = Math.abs(yClose - yOpen) || 1;
    const bodyLeft = centerX - candleWidth / 2;
    const wickTop = Math.min(yHigh, yLow);
    const wickHeight = Math.abs(yLow - yHigh) || 1;
    const isPositive = close >= open;
    const fill = isPositive ? positiveFill : negativeFill;
    const bodyPattern = isPositive ? bodyPatternPositive : bodyPatternNegative;
    const hasPatternOverlay = Boolean(bodyPattern);
    const bodySolidFill = hasPatternOverlay ? getSolidColor(isPositive) : fill;

    return {
      time: date.getTime(),
      centerX,
      bodyTop,
      bodyHeight,
      bodyLeft,
      candleWidth,
      wickTop,
      wickHeight,
      wickLeft: centerX - WICK_WIDTH / 2,
      bodySolidFill,
      wickFill: hasPatternOverlay ? bodySolidFill : fill,
      bodyPattern: hasPatternOverlay ? bodyPattern : undefined,
      insideStrokeWidth,
    };
  });
}

const CandlestickBody = memo(function CandlestickBody({
  geometry,
}: {
  geometry: CandleGeometry;
}) {
  const {
    wickLeft,
    wickTop,
    wickHeight,
    wickFill,
    bodyLeft,
    bodyTop,
    bodyHeight,
    candleWidth,
    bodySolidFill,
    bodyPattern,
    insideStrokeWidth,
  } = geometry;

  return (
    <>
      <rect
        fill={wickFill}
        height={wickHeight}
        width={WICK_WIDTH}
        x={wickLeft}
        y={wickTop}
      />
      <rect
        fill={bodySolidFill}
        height={bodyHeight}
        rx={1}
        ry={1}
        stroke={bodySolidFill}
        strokeWidth={1}
        width={candleWidth}
        x={bodyLeft}
        y={bodyTop}
      />
      {bodyPattern ? (
        <rect
          fill={bodyPattern}
          height={bodyHeight}
          rx={1}
          ry={1}
          width={candleWidth}
          x={bodyLeft}
          y={bodyTop}
        />
      ) : null}
      {insideStrokeWidth > 0 ? (
        <rect
          fill="none"
          height={bodyHeight - insideStrokeWidth}
          rx={1}
          ry={1}
          stroke={bodySolidFill}
          strokeWidth={insideStrokeWidth}
          width={candleWidth - insideStrokeWidth}
          x={bodyLeft + insideStrokeWidth / 2}
          y={bodyTop + insideStrokeWidth / 2}
        />
      ) : null}
    </>
  );
});

const CandlestickBodies = memo(function CandlestickBodies({
  geometries,
}: {
  geometries: CandleGeometry[];
}) {
  return (
    <>
      {geometries.map((geometry) => (
        <g key={geometry.time}>
          <CandlestickBody geometry={geometry} />
        </g>
      ))}
    </>
  );
});

interface AnimatedCandleProps {
  geometry: CandleGeometry;
  delay: number;
  enterTransition: Transition;
  revealEpoch: number;
}

function AnimatedCandle({
  geometry,
  delay,
  enterTransition,
  revealEpoch,
}: AnimatedCandleProps) {
  const t = transitionWithDelay(enterTransition, delay);
  const bodyOrigin = `${geometry.centerX}px ${geometry.bodyTop + geometry.bodyHeight / 2}px`;
  const wickCenterY = geometry.wickTop + geometry.wickHeight / 2;

  return (
    <motion.g
      animate={{ opacity: 1 }}
      initial={{ opacity: 0 }}
      key={`candle-enter-${geometry.time}-${revealEpoch}`}
      style={{ transformOrigin: `${geometry.centerX}px ${wickCenterY}px` }}
      transition={{ ...t, opacity: { duration: 0.15 } }}
    >
      <motion.rect
        animate={{ scaleY: 1 }}
        fill={geometry.wickFill}
        height={geometry.wickHeight}
        initial={{ scaleY: 0 }}
        style={{ transformOrigin: `${geometry.centerX}px ${wickCenterY}px` }}
        transition={t}
        width={WICK_WIDTH}
        x={geometry.wickLeft}
        y={geometry.wickTop}
      />
      <motion.rect
        animate={{ scaleY: 1 }}
        fill={geometry.bodySolidFill}
        height={geometry.bodyHeight}
        initial={{ scaleY: 0 }}
        rx={1}
        ry={1}
        stroke={geometry.bodySolidFill}
        strokeWidth={1}
        style={{ transformOrigin: bodyOrigin }}
        transition={t}
        width={geometry.candleWidth}
        x={geometry.bodyLeft}
        y={geometry.bodyTop}
      />
      {geometry.bodyPattern ? (
        <motion.rect
          animate={{ scaleY: 1 }}
          fill={geometry.bodyPattern}
          height={geometry.bodyHeight}
          initial={{ scaleY: 0 }}
          rx={1}
          ry={1}
          style={{ transformOrigin: bodyOrigin }}
          transition={t}
          width={geometry.candleWidth}
          x={geometry.bodyLeft}
          y={geometry.bodyTop}
        />
      ) : null}
    </motion.g>
  );
}

export function Candlestick({
  animate = true,
  positiveFill = DEFAULT_POSITIVE,
  negativeFill = DEFAULT_NEGATIVE,
  bodyPatternPositive,
  bodyPatternNegative,
  insideStrokeWidth = 0,
  fadedOpacity = 0.3,
  showHoverFade = true,
}: CandlestickProps) {
  const {
    data,
    xScale,
    yScale,
    xAccessor,
    animationDuration,
    enterTransition,
    revealEpoch = 0,
    isLoaded,
    bandWidth,
    columnWidth,
    hoveredCandleIndex,
  } = useChart();

  const candleWidth = Math.min(bandWidth ?? columnWidth * 0.8, columnWidth);

  const geometries = useMemo(
    () =>
      computeGeometries(
        data,
        xScale,
        yScale,
        xAccessor,
        candleWidth,
        positiveFill,
        negativeFill,
        bodyPatternPositive,
        bodyPatternNegative,
        insideStrokeWidth
      ),
    [
      data,
      xScale,
      yScale,
      xAccessor,
      candleWidth,
      positiveFill,
      negativeFill,
      bodyPatternPositive,
      bodyPatternNegative,
      insideStrokeWidth,
    ]
  );

  const hoveredTime = useMemo(() => {
    if (hoveredCandleIndex == null) {
      return null;
    }
    const point = data[hoveredCandleIndex];
    return point ? xAccessor(point).getTime() : null;
  }, [hoveredCandleIndex, data, xAccessor]);

  const highlightGeometry = useMemo(() => {
    if (hoveredCandleIndex == null) {
      return null;
    }
    const point = data[hoveredCandleIndex];
    if (!point) {
      return null;
    }
    return (
      computeGeometries(
        [point],
        xScale,
        yScale,
        xAccessor,
        candleWidth,
        positiveFill,
        negativeFill,
        bodyPatternPositive,
        bodyPatternNegative,
        insideStrokeWidth
      )[0] ?? null
    );
  }, [
    hoveredCandleIndex,
    data,
    xScale,
    yScale,
    xAccessor,
    candleWidth,
    positiveFill,
    negativeFill,
    bodyPatternPositive,
    bodyPatternNegative,
    insideStrokeWidth,
  ]);

  const defaultEnter: Transition = {
    type: "spring",
    duration: 0.8,
    bounce: 0.15,
  };
  const enter = enterTransition ?? defaultEnter;
  const staggerDelayMs =
    data.length > 0 ? (animationDuration * 0.6) / data.length : 0;

  if (animate && !isLoaded) {
    return (
      <g className="chart-candlesticks">
        {geometries.map((geometry, index) => (
          <AnimatedCandle
            delay={(index * staggerDelayMs) / 1000}
            enterTransition={enter}
            geometry={geometry}
            key={geometry.time}
            revealEpoch={revealEpoch}
          />
        ))}
      </g>
    );
  }

  const dimBase = showHoverFade && hoveredTime !== null;

  return (
    <g className="chart-candlesticks">
      <g
        opacity={dimBase ? fadedOpacity : 1}
        style={{ transition: "opacity 0.15s ease-in-out" }}
      >
        <CandlestickBodies geometries={geometries} />
      </g>
      {highlightGeometry ? (
        <g>
          <CandlestickBody geometry={highlightGeometry} />
        </g>
      ) : null}
    </g>
  );
}

Candlestick.displayName = "Candlestick";

export default Candlestick;
```

---

### Choropleth Chart <a name="choropleth-chart"></a>

A composable geographic map chart for visualizing data across regions with interactive tooltips, zoom controls, and pattern support

- **Category**: Charts
- **Registry Key**: `choropleth-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/choropleth-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `@types/geojson`
  - `@visx/geo@4.0.1-alpha.0`
  - `@visx/responsive@4.0.1-alpha.0`
  - `@visx/zoom@4.0.1-alpha.0`
  - `d3-geo`
  - `topojson-client`
  - `motion`
- **Local Registry Dependencies**:
  - `@bklit/chart-animation`
  - `@bklit/utils`
  - `@bklit/chart-utils`
  - `@bklit/chart-tooltip`

#### How to Use

```tsx
import { ChoroplethChart, ChoroplethFeatureComponent, ChoroplethGraticule, ChoroplethTooltip } from "@bklitui/ui/charts";
import * as topojson from "topojson-client";

// Load your GeoJSON data (from TopoJSON or direct GeoJSON)
const geojson = topojson.feature(topology, topology.objects.countries);

export default function WorldMap() {
  return (
    <ChoroplethChart data={geojson} aspectRatio="16 / 9">
      <ChoroplethGraticule />
      <ChoroplethFeatureComponent fill="var(--chart-1)" />
      <ChoroplethTooltip />
    </ChoroplethChart>
  );
}
```

#### Props & API Reference

##### `ChoroplethChart` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `data` | `FeatureCollection` | required | GeoJSON FeatureCollection with geographic features |
| `margin` | `Partial<Margin>` | `{ top: 0, right: 0, bottom: 0, left: 0 }` | Chart margins |
| `animationDuration` | `number` | `800` | Animation duration in ms |
| `aspectRatio` | `string` | `"16 / 9"` | CSS aspect ratio |
| `scale` | `number` | auto | Projection scale (auto-calculated from width if not set) |
| `center` | `[number, number]` | `[0, 20]` | Center coordinates [longitude, latitude] |
| `translate` | `[number, number]` | auto | Translate offset [x, y] |
| `zoomEnabled` | `boolean` | `false` | Enable zoom and pan interactions |
| `zoomMin` | `number` | `0.5` | Minimum zoom scale |
| `zoomMax` | `number` | `4` | Maximum zoom scale |
| `initialZoom` | `TransformMatrix` | identity | Initial zoom transform |
| `className` | `string` | `""` | Additional CSS class |

##### `ChoroplethFeatureComponent` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `fill` | `string` | - | Fill color for all features (overrides getFeatureColor) |
| `stroke` | `string` | `"var(--chart-grid)"` | Stroke color for borders |
| `strokeWidth` | `number` | `0.5` | Border stroke width |
| `fadedOpacity` | `number` | `0.4` | Opacity when another feature is hovered |
| `getFeatureColor` | `(feature, index) => string` | - | Custom color function |
| `patterns` | `ReactNode` | - | Pattern definitions using `@visx/pattern` components |
| `getFeaturePattern` | `(feature, index) => string \| null` | - | Return pattern ID for a feature |

##### `ChoroplethGraticule` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `stroke` | `string` | `"rgba(255,255,255,0.1)"` | Line color |
| `strokeWidth` | `number` | `0.5` | Line width |
| `step` | `[number, number]` | `[10, 10]` | Grid step intervals [longitude, latitude] in degrees |

##### `ChoroplethTooltip` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `content` | `(props) => ReactNode` | - | Custom tooltip renderer |
| `formatValue` | `(value) => string` | `toLocaleString` | Value formatter |
| `getFeatureName` | `(feature, index) => string` | - | Custom name getter |
| `getFeatureValue` | `(feature, index) => number` | - | Value getter for display |
| `valueLabel` | `string` | `"Value"` | Label for the value row |
| `className` | `string` | `""` | Additional CSS class |

#### Component Source Code

##### File: `src/charts/choropleth/choropleth-chart.tsx`

```tsx
"use client";

import { Mercator } from "@visx/geo";
import { ParentSize } from "@visx/responsive";
import type { TransformMatrix } from "@visx/zoom";
import { Zoom } from "@visx/zoom";
import type { FeatureCollection, Geometry } from "geojson";
import type { Transition } from "motion/react";
import React, {
  type ReactNode,
  useCallback,
  useEffect,
  useRef,
  useState,
} from "react";
import { cn } from "@/lib/utils";
import {
  type ChoroplethFeatureProperties,
  ChoroplethProvider,
  type ChoroplethTooltipData,
  ChoroplethZoomContext,
  type Margin,
  type ZoomInstance,
} from "./choropleth-context";

export interface ChoroplethChartProps {
  /** GeoJSON FeatureCollection data */
  data: FeatureCollection<Geometry, ChoroplethFeatureProperties>;
  /** Chart margins */
  margin?: Partial<Margin>;
  /** Animation duration in milliseconds. Default: 800 */
  animationDuration?: number;
  /** Motion enter transition (spring or cubic-bezier tween). */
  enterTransition?: Transition;
  /** Signature of motion URL state — triggers enter replay when it changes. */
  revealSignature?: string;
  /** Aspect ratio as "width / height". Default: "16 / 9" */
  aspectRatio?: string;
  /** Projection scale. If not provided, auto-calculated based on width */
  scale?: number;
  /** Center coordinates [longitude, latitude]. Default: [0, 20] */
  center?: [number, number];
  /** Translate offset [x, y]. If not provided, auto-calculated to center */
  translate?: [number, number];
  /** Enable zoom and pan. Default: false */
  zoomEnabled?: boolean;
  /** Minimum zoom scale. Default: 0.5 */
  zoomMin?: number;
  /** Maximum zoom scale. Default: 4 */
  zoomMax?: number;
  /** Initial zoom transform */
  initialZoom?: TransformMatrix;
  /** Additional class name for the container */
  className?: string;
  /** Child components (ChoroplethFeature, ChoroplethGraticule, ChoroplethTooltip) */
  children: ReactNode;
}

const DEFAULT_MARGIN: Margin = { top: 0, right: 0, bottom: 0, left: 0 };

// Known SVG component displayNames
const SVG_COMPONENT_NAMES = new Set([
  "ChoroplethFeature",
  "ChoroplethGraticule",
  "ChoroplethTooltip",
]);

// HTML elements that should render in overlay layer
const HTML_ELEMENTS = new Set(["div", "span", "button", "p", "a"]);

// Separate children into SVG and overlay layers
function separateChildren(children: ReactNode): {
  svgChildren: React.ReactNode[];
  overlayChildren: React.ReactNode[];
} {
  const childArray = React.Children.toArray(children);
  const svgChildren: React.ReactNode[] = [];
  const overlayChildren: React.ReactNode[] = [];

  for (const child of childArray) {
    if (!React.isValidElement(child)) {
      svgChildren.push(child);
      continue;
    }

    // Check if it's a known SVG component by displayName
    const displayName =
      typeof child.type === "function"
        ? (child.type as { displayName?: string }).displayName
        : null;

    if (displayName && SVG_COMPONENT_NAMES.has(displayName)) {
      svgChildren.push(child);
    } else if (typeof child.type === "string") {
      // Native elements - HTML goes to overlay, SVG stays in SVG
      if (HTML_ELEMENTS.has(child.type)) {
        overlayChildren.push(child);
      } else {
        svgChildren.push(child);
      }
    } else {
      // Unknown React components (like ZoomControls) go to overlay
      overlayChildren.push(child);
    }
  }

  return { svgChildren, overlayChildren };
}

const DEFAULT_INITIAL_ZOOM: TransformMatrix = {
  scaleX: 1,
  scaleY: 1,
  translateX: 0,
  translateY: 0,
  skewX: 0,
  skewY: 0,
};

function ChoroplethChartInner({
  data,
  width,
  height,
  margin,
  animationDuration,
  enterTransition,
  revealSignature = "",
  scale: scaleProp,
  center,
  translate: translateProp,
  zoomEnabled,
  zoomMin,
  zoomMax,
  initialZoom,
  children,
}: {
  data: FeatureCollection<Geometry, ChoroplethFeatureProperties>;
  width: number;
  height: number;
  margin: Margin;
  animationDuration: number;
  enterTransition?: Transition;
  revealSignature?: string;
  scale?: number;
  center: [number, number];
  translate?: [number, number];
  zoomEnabled: boolean;
  zoomMin: number;
  zoomMax: number;
  initialZoom: TransformMatrix;
  children: ReactNode;
}) {
  const containerRef = useRef<HTMLDivElement>(null);
  const [isLoaded, setIsLoaded] = useState(false);
  const [revealEpoch, setRevealEpoch] = useState(0);
  const [hoveredFeatureIndex, setHoveredFeatureIndex] = useState<number | null>(
    null
  );
  const [tooltipData, setTooltipData] = useState<ChoroplethTooltipData | null>(
    null
  );

  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  // Auto-calculate scale based on width if not provided
  const scale = scaleProp ?? (innerWidth / 630) * 100;

  // Auto-calculate translate to center if not provided
  const translate = translateProp ?? [
    innerWidth / 2 + margin.left,
    innerHeight / 2 + margin.top + 50,
  ];

  // biome-ignore lint/correctness/useExhaustiveDependencies: revealSignature
  useEffect(() => {
    setRevealEpoch((n) => n + 1);
    setIsLoaded(false);
    const timeout = setTimeout(() => {
      setIsLoaded(true);
    }, animationDuration);
    return () => clearTimeout(timeout);
  }, [animationDuration, revealSignature]);

  const handleMouseLeave = useCallback(() => {
    setHoveredFeatureIndex(null);
    setTooltipData(null);
  }, []);

  if (width < 10 || height < 10) {
    return null;
  }

  return (
    <Mercator
      center={center}
      data={data.features}
      scale={scale}
      translate={translate as [number, number]}
    >
      {(mercator) => {
        // Project geo coordinates to screen coordinates using the projection
        const projectPoint = (
          coords: [number, number]
        ): [number, number] | null => {
          const projected = mercator.projection(coords);
          if (!projected) {
            return null;
          }
          return projected as [number, number];
        };

        const contextValue = {
          features: data.features,
          featureCollection: data,
          pathGenerator: (feature: (typeof data.features)[0]) =>
            mercator.path(feature) ?? undefined,
          // biome-ignore lint/suspicious/noExplicitAny: GeoJSON types are complex
          rawPathGenerator: (geo: any) => mercator.path(geo),
          projectPoint,
          width,
          height,
          innerWidth,
          innerHeight,
          margin,
          hoveredFeatureIndex,
          setHoveredFeatureIndex,
          tooltipData,
          setTooltipData,
          containerRef,
          isLoaded,
          animationDuration,
          enterTransition,
          revealEpoch,
        };

        // Separate SVG children from HTML overlay children
        const { svgChildren, overlayChildren } = separateChildren(children);

        const svgContent = (zoom?: ZoomInstance<SVGSVGElement>) => (
          <ChoroplethZoomContext.Provider value={{ zoom: zoom ?? null }}>
            <ChoroplethProvider value={contextValue}>
              <div className="relative h-full w-full" ref={containerRef}>
                <svg
                  aria-hidden="true"
                  height={height}
                  onMouseLeave={handleMouseLeave}
                  ref={zoom?.containerRef}
                  style={{
                    cursor: zoom?.isDragging ? "grabbing" : "grab",
                    touchAction: "none",
                  }}
                  width={width}
                >
                  <g
                    style={{
                      transition: zoom?.isDragging
                        ? "none"
                        : "transform 0.18s ease-out",
                    }}
                    transform={zoom ? zoom.toString() : undefined}
                  >
                    {svgChildren}
                  </g>
                </svg>
                {/* HTML overlay layer for controls, legends, etc. */}
                {overlayChildren}
              </div>
            </ChoroplethProvider>
          </ChoroplethZoomContext.Provider>
        );

        if (zoomEnabled) {
          return (
            <Zoom<SVGSVGElement>
              height={height}
              initialTransformMatrix={initialZoom}
              scaleXMax={zoomMax}
              scaleXMin={zoomMin}
              scaleYMax={zoomMax}
              scaleYMin={zoomMin}
              wheelDelta={(event) => {
                // Reduce zoom sensitivity (default is too aggressive)
                const scale = event.deltaY > 0 ? 0.95 : 1.05;
                return { scaleX: scale, scaleY: scale };
              }}
              width={width}
            >
              {(zoom) => svgContent(zoom)}
            </Zoom>
          );
        }

        return svgContent();
      }}
    </Mercator>
  );
}

export function ChoroplethChart({
  data,
  margin: marginProp,
  animationDuration = 800,
  enterTransition,
  revealSignature,
  aspectRatio = "16 / 9",
  scale,
  center = [0, 20],
  translate,
  zoomEnabled = false,
  zoomMin = 0.5,
  zoomMax = 4,
  initialZoom = DEFAULT_INITIAL_ZOOM,
  className = "",
  children,
}: ChoroplethChartProps) {
  const margin = { ...DEFAULT_MARGIN, ...marginProp };

  return (
    <div className={cn("relative w-full", className)} style={{ aspectRatio }}>
      <ParentSize>
        {({ width, height }) =>
          width > 0 && height > 0 ? (
            <ChoroplethChartInner
              animationDuration={animationDuration}
              center={center}
              data={data}
              enterTransition={enterTransition}
              height={height}
              initialZoom={initialZoom}
              margin={margin}
              revealSignature={revealSignature}
              scale={scale}
              translate={translate}
              width={width}
              zoomEnabled={zoomEnabled}
              zoomMax={zoomMax}
              zoomMin={zoomMin}
            >
              {children}
            </ChoroplethChartInner>
          ) : null
        }
      </ParentSize>
    </div>
  );
}

ChoroplethChart.displayName = "ChoroplethChart";

export default ChoroplethChart;
```

##### File: `src/charts/choropleth/choropleth-context.tsx`

```tsx
"use client";

import type { ProvidedZoom, TransformMatrix } from "@visx/zoom";
import type { Feature, FeatureCollection, Geometry } from "geojson";
import type { Transition } from "motion/react";
import {
  createContext,
  type Dispatch,
  type RefObject,
  type SetStateAction,
  useContext,
} from "react";

// ZoomState from visx/zoom that includes isDragging
interface ZoomState {
  initialTransformMatrix: TransformMatrix;
  transformMatrix: TransformMatrix;
  isDragging: boolean;
}

// Combined type from visx Zoom children prop
export type ZoomInstance<E extends Element> = ProvidedZoom<E> & ZoomState;

// Zoom context to share zoom controls with child components
interface ChoroplethZoomContextValue {
  zoom: ZoomInstance<SVGSVGElement> | null;
}

export const ChoroplethZoomContext = createContext<ChoroplethZoomContextValue>({
  zoom: null,
});

export function useChoroplethZoom() {
  return useContext(ChoroplethZoomContext);
}

export interface Margin {
  top: number;
  right: number;
  bottom: number;
  left: number;
}

export interface ChoroplethFeatureProperties {
  name?: string;
  id?: string | number;
  [key: string]: unknown;
}

export type ChoroplethFeature = Feature<Geometry, ChoroplethFeatureProperties>;

export interface ChoroplethTooltipData {
  featureIndex: number;
  x: number;
  y: number;
  feature: ChoroplethFeature;
}

export interface ChoroplethContextValue {
  // Geo data
  features: ChoroplethFeature[];
  featureCollection: FeatureCollection<Geometry, ChoroplethFeatureProperties>;

  // Projection function (returns path string)
  pathGenerator: (feature: ChoroplethFeature) => string | undefined;

  // Raw path function for graticule (accepts any geo object)
  // biome-ignore lint/suspicious/noExplicitAny: GeoJSON types are complex
  rawPathGenerator: (geo: any) => string | null;

  // Project geo coordinates to screen coordinates
  projectPoint: (coords: [number, number]) => [number, number] | null;

  // Dimensions
  width: number;
  height: number;
  innerWidth: number;
  innerHeight: number;
  margin: Margin;

  // Hover state
  hoveredFeatureIndex: number | null;
  setHoveredFeatureIndex: (index: number | null) => void;

  // Tooltip
  tooltipData: ChoroplethTooltipData | null;
  setTooltipData: Dispatch<SetStateAction<ChoroplethTooltipData | null>>;
  containerRef: RefObject<HTMLDivElement | null>;

  // Animation
  isLoaded: boolean;
  animationDuration: number;
  enterTransition?: Transition;
  revealEpoch: number;
}

const ChoroplethContext = createContext<ChoroplethContextValue | null>(null);

export function ChoroplethProvider({
  children,
  value,
}: {
  children: React.ReactNode;
  value: ChoroplethContextValue;
}) {
  return (
    <ChoroplethContext.Provider value={value}>
      {children}
    </ChoroplethContext.Provider>
  );
}

export function useChoropleth(): ChoroplethContextValue {
  const context = useContext(ChoroplethContext);
  if (!context) {
    throw new Error("useChoropleth must be used within a ChoroplethProvider");
  }
  return context;
}

// CSS variables for choropleth theming
export const choroplethCssVars = {
  feature1: "var(--chart-1)",
  feature2: "var(--chart-2)",
  feature3: "var(--chart-3)",
  feature4: "var(--chart-4)",
  feature5: "var(--chart-5)",
  stroke: "var(--chart-grid)",
  background: "var(--background)",
};

// Default colors array for cycling through features
export const defaultChoroplethColors = [
  "var(--chart-1)",
  "var(--chart-2)",
  "var(--chart-3)",
  "var(--chart-4)",
  "var(--chart-5)",
];
```

##### File: `src/charts/choropleth/choropleth-feature.tsx`

```tsx
"use client";

import { geoCentroid } from "d3-geo";
import { motion } from "motion/react";
import { useCallback, useMemo } from "react";
import { transitionWithDelay } from "../motion-utils";
import {
  type ChoroplethFeature as ChoroplethFeatureType,
  defaultChoroplethColors,
  useChoropleth,
} from "./choropleth-context";

export interface ChoroplethFeatureProps {
  /** Fill color for all features (overrides getFeatureColor). Default: uses getFeatureColor or chart-1 */
  fill?: string;
  /** Stroke color for feature borders. Default: var(--chart-grid) */
  stroke?: string;
  /** Stroke width for feature borders. Default: 0.5 */
  strokeWidth?: number;
  /** Opacity when another feature is hovered. Default: 0.4 */
  fadedOpacity?: number;
  /** Custom function to get feature fill color */
  getFeatureColor?: (feature: ChoroplethFeatureType, index: number) => string;
  /** Pattern definitions to render in defs. Use @visx/pattern components (PatternLines, PatternCircles, etc.) */
  patterns?: React.ReactNode;
  /** Return pattern ID for a feature, or null/undefined to use solid fill */
  getFeaturePattern?: (
    feature: ChoroplethFeatureType,
    index: number
  ) => string | null | undefined;
}

interface AnimatedFeaturePathProps {
  path: string;
  fill: string;
  stroke: string;
  strokeWidth: number;
  isFaded: boolean;
  isHighlighted: boolean;
  fadedOpacity: number;
  animationDuration: number;
  index: number;
  totalFeatures: number;
  onMouseEnter: () => void;
  onMouseLeave: () => void;
}

function AnimatedFeaturePath({
  path,
  fill,
  stroke,
  strokeWidth,
  isFaded,
  isHighlighted,
  fadedOpacity,
  animationDuration,
  index,
  totalFeatures,
  onMouseEnter,
  onMouseLeave,
}: AnimatedFeaturePathProps) {
  const { enterTransition, revealEpoch } = useChoropleth();
  const staggerDelaySec =
    ((index / totalFeatures) * animationDuration * 0.5) / 1000;
  const enterAnim = transitionWithDelay(enterTransition, staggerDelaySec);

  // Calculate target opacity - slightly boost highlighted features
  const getTargetOpacity = () => {
    if (isFaded) {
      return fadedOpacity;
    }
    if (isHighlighted) {
      return 1;
    }
    return 0.85;
  };
  const targetOpacity = getTargetOpacity();

  return (
    <motion.path
      animate={{ opacity: targetOpacity }}
      className="cursor-pointer"
      d={path}
      fill={fill}
      initial={{ opacity: 0 }}
      key={`feature-${index}-${revealEpoch}`}
      onMouseEnter={onMouseEnter}
      onMouseLeave={onMouseLeave}
      stroke={stroke}
      strokeWidth={strokeWidth}
      transition={{
        opacity: { duration: 0.18, ease: "easeOut" },
        default: enterAnim,
      }}
    />
  );
}

export function ChoroplethFeature({
  fill,
  stroke = "var(--background)",
  strokeWidth = 0.5,
  fadedOpacity = 0.4,
  getFeatureColor,
  patterns,
  getFeaturePattern,
}: ChoroplethFeatureProps) {
  const {
    features,
    pathGenerator,
    projectPoint,
    hoveredFeatureIndex,
    setHoveredFeatureIndex,
    setTooltipData,
    animationDuration,
    width,
    height,
  } = useChoropleth();

  // Pre-calculate centroids for all features (only recalculates when features change)
  const featureCentroids = useMemo(() => {
    return features.map((feature) => {
      try {
        const centroid = geoCentroid(feature);
        if (
          centroid &&
          !Number.isNaN(centroid[0]) &&
          !Number.isNaN(centroid[1])
        ) {
          const projected = projectPoint(centroid as [number, number]);
          if (projected) {
            // Clamp to chart bounds with padding
            const padding = 60;
            const x = Math.max(
              padding,
              Math.min(width - padding, projected[0])
            );
            const y = Math.max(
              padding,
              Math.min(height - padding, projected[1])
            );
            return { x, y };
          }
        }
      } catch {
        // Some geometries may not have valid centroids
      }
      return null;
    });
  }, [features, projectPoint, width, height]);

  // Get color for a feature
  const getFeatureColorFn = useCallback(
    (feature: ChoroplethFeatureType, index: number): string => {
      if (fill) {
        return fill;
      }
      if (getFeatureColor) {
        return getFeatureColor(feature, index);
      }
      // Default: use chart colors cycling through
      return (
        defaultChoroplethColors[index % defaultChoroplethColors.length] ??
        "var(--chart-1)"
      );
    },
    [fill, getFeatureColor]
  );

  // Check if any element is hovered
  const isAnyHovered = hoveredFeatureIndex !== null;

  return (
    <g className="choropleth-features">
      {/* Pattern definitions */}
      {patterns && <defs>{patterns}</defs>}

      {/* Feature paths */}
      {features.map((feature, index) => {
        const path = pathGenerator(feature);

        // Skip if path is empty or undefined
        if (!path || path.trim() === "") {
          return null;
        }

        const isHighlighted = hoveredFeatureIndex === index;
        const isFaded = isAnyHovered && !isHighlighted;

        // Get pre-calculated centroid for tooltip positioning
        const centroid = featureCentroids[index];

        const handleMouseEnter = () => {
          setHoveredFeatureIndex(index);
          setTooltipData({
            featureIndex: index,
            x: centroid?.x ?? width / 2,
            y: centroid?.y ?? height / 2,
            feature,
          });
        };

        const handleMouseLeave = () => {
          setHoveredFeatureIndex(null);
          setTooltipData(null);
        };

        // Determine fill (pattern URL or solid color)
        let featureFill: string;
        const patternId = getFeaturePattern?.(feature, index);
        if (patternId) {
          featureFill = `url(#${patternId})`;
        } else {
          featureFill = getFeatureColorFn(feature, index);
        }

        return (
          <AnimatedFeaturePath
            animationDuration={animationDuration}
            fadedOpacity={fadedOpacity}
            fill={featureFill}
            index={index}
            isFaded={isFaded}
            isHighlighted={isHighlighted}
            key={`feature-${feature.properties?.name ?? feature.properties?.id ?? feature.id ?? path}`}
            onMouseEnter={handleMouseEnter}
            onMouseLeave={handleMouseLeave}
            path={path}
            stroke={stroke}
            strokeWidth={strokeWidth}
            totalFeatures={features.length}
          />
        );
      })}
    </g>
  );
}

ChoroplethFeature.displayName = "ChoroplethFeature";

export default ChoroplethFeature;
```

##### File: `src/charts/choropleth/choropleth-graticule.tsx`

```tsx
"use client";

import { Graticule } from "@visx/geo";
import { useChoropleth } from "./choropleth-context";

export interface ChoroplethGraticuleProps {
  /** Stroke color for graticule lines. Default: rgba(255,255,255,0.1) */
  stroke?: string;
  /** Stroke width for graticule lines. Default: 0.5 */
  strokeWidth?: number;
  /** Step intervals for graticule lines [longitude, latitude] in degrees. Default: [10, 10] */
  step?: [number, number];
}

export function ChoroplethGraticule({
  stroke = "rgba(255,255,255,0.1)",
  strokeWidth = 0.5,
  step,
}: ChoroplethGraticuleProps) {
  const { rawPathGenerator } = useChoropleth();

  return (
    <Graticule
      graticule={(g) => rawPathGenerator(g) || ""}
      step={step}
      stroke={stroke}
      strokeWidth={strokeWidth}
    />
  );
}

ChoroplethGraticule.displayName = "ChoroplethGraticule";

export default ChoroplethGraticule;
```

##### File: `src/charts/choropleth/choropleth-tooltip.tsx`

```tsx
"use client";

import { intFmt } from "../chart-formatters";
import { TooltipBox } from "../tooltip/tooltip-box";
import { TooltipContent, type TooltipRow } from "../tooltip/tooltip-content";
import {
  type ChoroplethFeature,
  useChoropleth,
  useChoroplethZoom,
} from "./choropleth-context";

export interface ChoroplethTooltipProps {
  /** Custom content renderer for feature tooltips */
  content?: (props: {
    feature: ChoroplethFeature;
    index: number;
  }) => React.ReactNode;
  /** Value formatter function */
  formatValue?: (value: number) => string;
  /** Get the display name for a feature. Default: uses feature.properties.name */
  getFeatureName?: (feature: ChoroplethFeature, index: number) => string;
  /** Get the value for a feature (for display in tooltip) */
  getFeatureValue?: (
    feature: ChoroplethFeature,
    index: number
  ) => number | undefined;
  /** Label for the value row. Default: "Value" */
  valueLabel?: string;
  /** Custom class name */
  className?: string;
}

export function ChoroplethTooltip({
  content,
  formatValue = intFmt,
  getFeatureName,
  getFeatureValue,
  valueLabel = "Value",
  className = "",
}: ChoroplethTooltipProps) {
  const { tooltipData, containerRef, width, height, features } =
    useChoropleth();
  const { zoom } = useChoroplethZoom();

  if (!tooltipData) {
    return null;
  }

  // Apply zoom transform to centroid position
  let x = tooltipData.x;
  let y = tooltipData.y;

  if (zoom) {
    // Apply the zoom transform matrix to the tooltip position
    const transformed = zoom.applyToPoint({ x, y });
    x = transformed.x;
    y = transformed.y;
  }

  const feature = features[tooltipData.featureIndex];
  if (!feature) {
    return null;
  }

  // Get feature name
  const featureName = getFeatureName
    ? getFeatureName(feature, tooltipData.featureIndex)
    : (feature.properties?.name ?? `Feature ${tooltipData.featureIndex}`);

  // Custom content
  if (content) {
    return (
      <TooltipBox
        className={className}
        containerHeight={height}
        containerRef={containerRef}
        containerWidth={width}
        visible
        x={x}
        y={y}
      >
        {content({ feature, index: tooltipData.featureIndex })}
      </TooltipBox>
    );
  }

  // Default tooltip with optional value
  const value = getFeatureValue?.(feature, tooltipData.featureIndex);
  const rows: TooltipRow[] =
    value === undefined
      ? []
      : [
          {
            color: "var(--chart-1)",
            label: valueLabel,
            value: formatValue(value),
          },
        ];

  return (
    <TooltipBox
      className={className}
      containerHeight={height}
      containerRef={containerRef}
      containerWidth={width}
      visible
      x={x}
      y={y}
    >
      <TooltipContent rows={rows} title={featureName} />
    </TooltipBox>
  );
}

ChoroplethTooltip.displayName = "ChoroplethTooltip";

export default ChoroplethTooltip;
```

##### File: `src/charts/choropleth/index.ts`

```tsx
export type { TransformMatrix } from "@visx/zoom";
export { ChoroplethChart, type ChoroplethChartProps } from "./choropleth-chart";
export {
  type ChoroplethContextValue,
  type ChoroplethFeature,
  type ChoroplethFeatureProperties,
  ChoroplethProvider,
  type ChoroplethTooltipData,
  choroplethCssVars,
  defaultChoroplethColors,
  type Margin,
  useChoropleth,
  useChoroplethZoom,
} from "./choropleth-context";
export {
  ChoroplethFeature as ChoroplethFeatureComponent,
  type ChoroplethFeatureProps,
} from "./choropleth-feature";
export {
  ChoroplethGraticule,
  type ChoroplethGraticuleProps,
} from "./choropleth-graticule";
export {
  ChoroplethTooltip,
  type ChoroplethTooltipProps,
} from "./choropleth-tooltip";
```

---

### Composed Chart <a name="composed-chart"></a>

Mix SeriesBar, Line, and Area on one shared time axis (Recharts ComposedChart–style)

- **Category**: Charts
- **Registry Key**: `composed-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/composed-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `@visx/curve@4.0.1-alpha.0`
  - `@visx/scale@4.0.1-alpha.0`
  - `@visx/shape@4.0.1-alpha.0`
  - `@visx/responsive@4.0.1-alpha.0`
  - `d3-array`
  - `motion`
- **Local Registry Dependencies**:
  - `@bklit/chart-context`
  - `@bklit/grid`
  - `@bklit/x-axis`
  - `@bklit/y-axis`
  - `@bklit/chart-tooltip`
  - `@bklit/utils`
  - `@bklit/line-chart`
  - `@bklit/area-chart`

#### How to Use

```tsx
import {
  Area,
  ComposedChart,
  Grid,
  Line,
  SeriesBar,
  XAxis,
  ChartTooltip,
} from "@bklitui/ui/charts";
import { curveCatmullRom } from "@visx/curve";
import { composedDemoData } from "@/lib/composed-demo-data";

const smooth = curveCatmullRom.alpha(0.42);

export default function Example() {
  return (
    <ComposedChart
      aspectRatio="2 / 1"
      barGap={0}
      data={composedDemoData}
      maxBarSize={32}
      xDataKey="date"
    >
      <Grid horizontal />
      <Area
        curve={smooth}
        dataKey="runRate"
        fill="var(--chart-4)"
        fillOpacity={0.32}
      />
      <SeriesBar dataKey="units" fill="var(--chart-3)" radius={4} />
      <Line curve={smooth} dataKey="revenue" stroke="var(--chart-1)" />
      <ChartTooltip showCrosshair={false} />
      <XAxis numTicks={8} />
    </ComposedChart>
  );
}
```

#### Component Source Code

##### File: `src/charts/composed-chart.tsx`

```tsx
"use client";

import { ParentSize } from "@visx/responsive";
import type { Transition } from "motion/react";
import {
  Children,
  isValidElement,
  type ReactElement,
  type ReactNode,
  useMemo,
  useRef,
} from "react";
import { cn } from "@/lib/utils";
import { Area, type AreaProps } from "./area";
import type { LineConfig, Margin } from "./chart-context";
import { Line, type LineProps } from "./line";
import { SeriesBar, type SeriesBarProps } from "./series-bar";
import { TimeSeriesChartInner } from "./time-series-chart-shell";

export interface ComposedChartProps {
  /** Data array — each row typically has a date and multiple numeric series */
  data: Record<string, unknown>[];
  /** Key for the x-axis (time). Default: "date" */
  xDataKey?: string;
  margin?: Partial<Margin>;
  animationDuration?: number;
  animationEasing?: string;
  enterTransition?: Transition;
  /** Signature of motion URL state — triggers reveal replay when it changes. */
  revealSignature?: string;
  aspectRatio?: string;
  className?: string;
  children: ReactNode;
  /** Target bar width in px (Recharts-style `barSize`). */
  barSize?: number;
  /** Maximum bar width in px (`maxBarSize`). */
  maxBarSize?: number;
  /** Gap between grouped `SeriesBar` series in px. Default: 4 */
  barGap?: number;
  /** Stack `SeriesBar` segments in child order at each x (line/area are not stacked). */
  stacked?: boolean;
  /** Gap in px between stacked segments. Default: 0 */
  stackGap?: number;
}

const DEFAULT_MARGIN: Margin = { top: 40, right: 40, bottom: 40, left: 40 };

function getChildComponentName(child: ReactElement): string {
  const childType = child.type as { displayName?: string; name?: string };
  return typeof child.type === "function"
    ? childType.displayName || childType.name || ""
    : "";
}

function upsertLineConfig(lines: LineConfig[], config: LineConfig): void {
  const index = lines.findIndex((line) => line.dataKey === config.dataKey);
  if (index === -1) {
    lines.push(config);
    return;
  }
  // Area+Line pairs share a dataKey — keep the later config (Line over Area).
  lines[index] = config;
}

function tryAppendSeriesBar(
  child: ReactElement,
  lines: LineConfig[],
  barDataKeys: string[]
): boolean {
  const name = getChildComponentName(child);
  if (!(child.type === SeriesBar || name === "SeriesBar")) {
    return false;
  }
  const props = child.props as SeriesBarProps;
  if (!props.dataKey) {
    return true;
  }
  barDataKeys.push(props.dataKey);
  upsertLineConfig(lines, {
    dataKey: props.dataKey,
    stroke: props.stroke || props.fill || "var(--chart-line-primary)",
    strokeWidth: 0,
  });
  return true;
}

function tryAppendLine(child: ReactElement, lines: LineConfig[]): boolean {
  const name = getChildComponentName(child);
  if (!(child.type === Line || name === "Line")) {
    return false;
  }
  const props = child.props as LineProps;
  if (props.dataKey) {
    upsertLineConfig(lines, {
      dataKey: props.dataKey,
      stroke: props.stroke || "var(--chart-line-primary)",
      strokeWidth: props.strokeWidth ?? 2.5,
    });
  }
  return true;
}

function tryAppendArea(child: ReactElement, lines: LineConfig[]): boolean {
  const name = getChildComponentName(child);
  if (!(child.type === Area || name === "Area")) {
    return false;
  }
  const props = child.props as AreaProps;
  if (props.dataKey) {
    upsertLineConfig(lines, {
      dataKey: props.dataKey,
      stroke: props.stroke || props.fill || "var(--chart-line-primary)",
      strokeWidth: props.strokeWidth ?? 2,
    });
  }
  return true;
}

function extractComposedSeries(children: ReactNode): {
  lines: LineConfig[];
  barDataKeys: string[];
} {
  const lines: LineConfig[] = [];
  const barDataKeys: string[] = [];

  Children.forEach(children, (child) => {
    if (!isValidElement(child)) {
      return;
    }
    if (tryAppendSeriesBar(child, lines, barDataKeys)) {
      return;
    }
    if (tryAppendLine(child, lines)) {
      return;
    }
    tryAppendArea(child, lines);
  });

  return { lines, barDataKeys };
}

function computeComposedYScaleDomainMax(
  data: Record<string, unknown>[],
  lines: LineConfig[],
  barDataKeys: string[]
): number | undefined {
  const barSet = new Set(barDataKeys);
  let max = 0;
  for (const d of data) {
    let barSum = 0;
    for (const k of barDataKeys) {
      const v = d[k];
      if (typeof v === "number") {
        barSum += v;
      }
    }
    let rowMaxOther = 0;
    for (const line of lines) {
      if (barSet.has(line.dataKey)) {
        continue;
      }
      const v = d[line.dataKey];
      if (typeof v === "number") {
        rowMaxOther = Math.max(rowMaxOther, v);
      }
    }
    max = Math.max(max, barSum, rowMaxOther);
  }
  return max > 0 ? max : undefined;
}

interface ChartInnerProps {
  width: number;
  height: number;
  data: Record<string, unknown>[];
  xDataKey: string;
  margin: Margin;
  animationDuration: number;
  animationEasing?: string;
  enterTransition?: Transition;
  revealSignature?: string;
  children: ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
  barSize?: number;
  maxBarSize?: number;
  barGap?: number;
  stacked?: boolean;
  stackGap?: number;
}

function ChartInner({
  width,
  height,
  data,
  xDataKey,
  margin,
  animationDuration,
  animationEasing,
  enterTransition,
  revealSignature,
  children,
  containerRef,
  barSize,
  maxBarSize,
  barGap,
  stacked = false,
  stackGap = 0,
}: ChartInnerProps) {
  const { lines, barDataKeys } = useMemo(
    () => extractComposedSeries(children),
    [children]
  );

  const composedStackOffsets = useMemo(() => {
    if (!(stacked && barDataKeys.length > 0)) {
      return undefined;
    }
    const offsets = new Map<number, Map<string, number>>();
    for (let i = 0; i < data.length; i++) {
      const d = data[i];
      if (!d) {
        continue;
      }
      const pointOffsets = new Map<string, number>();
      let cumulative = 0;
      for (const key of barDataKeys) {
        pointOffsets.set(key, cumulative);
        const v = d[key];
        if (typeof v === "number") {
          cumulative += v;
        }
      }
      offsets.set(i, pointOffsets);
    }
    return offsets;
  }, [data, barDataKeys, stacked]);

  const yScaleDomainMax = useMemo(
    () =>
      stacked && barDataKeys.length > 0
        ? computeComposedYScaleDomainMax(data, lines, barDataKeys)
        : undefined,
    [data, lines, barDataKeys, stacked]
  );

  return (
    <TimeSeriesChartInner
      animationDuration={animationDuration}
      animationEasing={animationEasing}
      clipPathId="composed-chart-grow-clip"
      composedBarDataKeys={barDataKeys.length > 0 ? barDataKeys : undefined}
      composedBarGap={barGap}
      composedBarSize={barSize}
      composedMaxBarSize={maxBarSize}
      composedStacked={stacked}
      composedStackGap={stackGap}
      composedStackOffsets={composedStackOffsets}
      containerRef={containerRef}
      data={data}
      enterTransition={enterTransition}
      height={height}
      lines={lines}
      margin={margin}
      revealSignature={revealSignature}
      width={width}
      xDataKey={xDataKey}
      yScaleDomainMax={yScaleDomainMax}
    >
      {children}
    </TimeSeriesChartInner>
  );
}

export function ComposedChart({
  data,
  xDataKey = "date",
  margin: marginProp,
  animationDuration = 1100,
  animationEasing,
  enterTransition,
  revealSignature,
  aspectRatio = "2 / 1",
  className = "",
  children,
  barSize,
  maxBarSize,
  barGap = 4,
  stacked = false,
  stackGap = 0,
}: ComposedChartProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const margin = { ...DEFAULT_MARGIN, ...marginProp };

  return (
    <div
      className={cn("relative w-full", className)}
      ref={containerRef}
      style={{ aspectRatio, touchAction: "none" }}
    >
      <ParentSize debounceTime={10}>
        {({ width, height }) => (
          <ChartInner
            animationDuration={animationDuration}
            animationEasing={animationEasing}
            barGap={barGap}
            barSize={barSize}
            containerRef={containerRef}
            data={data}
            enterTransition={enterTransition}
            height={height}
            margin={margin}
            maxBarSize={maxBarSize}
            revealSignature={revealSignature}
            stacked={stacked}
            stackGap={stackGap}
            width={width}
            xDataKey={xDataKey}
          >
            {children}
          </ChartInner>
        )}
      </ParentSize>
    </div>
  );
}

ComposedChart.displayName = "ComposedChart";

export default ComposedChart;
```

##### File: `src/charts/series-bar.tsx`

```tsx
"use client";

import type { Transition } from "motion/react";
import { motion } from "motion/react";
import { useMemo } from "react";
import { chartCssVars, useChart } from "./chart-context";
import { transitionWithDelay } from "./motion-utils";
import { computeSeriesBarWidth } from "./series-bar-layout";

function computeSeriesBarLayout(input: {
  stacked: boolean;
  composedStackOffsets: Map<number, Map<string, number>> | undefined;
  rowIndex: number;
  dataKey: string;
  value: number;
  yScale: (n: number) => number | undefined;
  innerHeight: number;
  xCenter: number;
  barWidth: number;
  seriesCount: number;
  gap: number;
  seriesIndex: number;
  stackGap: number;
  isLastSeries: boolean;
  radius: number;
}): {
  barLeft: number;
  barHeight: number;
  effectiveRadius: number;
  valueY: number;
} {
  const {
    stacked,
    composedStackOffsets,
    rowIndex,
    dataKey,
    value,
    yScale,
    innerHeight,
    xCenter,
    barWidth,
    seriesCount,
    gap,
    seriesIndex,
    stackGap,
    isLastSeries,
    radius,
  } = input;

  if (stacked && composedStackOffsets) {
    const offset = composedStackOffsets.get(rowIndex)?.get(dataKey) ?? 0;
    const valuePos = yScale(value) ?? 0;
    let barHeight = innerHeight - valuePos;
    const offsetY = yScale(offset) ?? innerHeight;
    const gapOffset = seriesIndex * stackGap;
    const valueY = offsetY - barHeight - gapOffset;
    if (!isLastSeries && stackGap > 0) {
      barHeight = Math.max(0, barHeight - stackGap);
    }
    const barLeft = xCenter - barWidth / 2;
    const applyRounding = stackGap > 0 || isLastSeries;
    return {
      barLeft,
      barHeight,
      effectiveRadius: applyRounding ? radius : 0,
      valueY,
    };
  }

  const groupWidth =
    seriesCount * barWidth + (seriesCount > 1 ? (seriesCount - 1) * gap : 0);
  const valueY = yScale(value) ?? innerHeight;
  return {
    barLeft: xCenter - groupWidth / 2 + seriesIndex * (barWidth + gap),
    barHeight: innerHeight - valueY,
    effectiveRadius: radius,
    valueY,
  };
}

export interface SeriesBarProps {
  /** Key in data for bar height (y value) */
  dataKey: string;
  /** Fill color. Default: var(--chart-line-primary) */
  fill?: string;
  /** Tooltip dot color when fill is gradient/pattern. Default: fill */
  stroke?: string;
  /** Corner radius for bar top corners. Default: 0 (square tops, similar to Bar lineCap="butt") */
  radius?: number;
  /** Animate grow from baseline. Default: true */
  animate?: boolean;
  /** Opacity for non-hovered bars when another point is hovered (matches BarChart). Default: 0.3 */
  fadedOpacity?: number;
}

export function SeriesBar({
  dataKey,
  fill = chartCssVars.linePrimary,
  radius = 0,
  animate = true,
  fadedOpacity = 0.3,
}: SeriesBarProps) {
  const {
    data,
    xScale,
    yScale,
    xAccessor,
    innerHeight,
    innerWidth,
    columnWidth,
    isLoaded,
    animationDuration,
    enterTransition,
    revealEpoch = 0,
    barScale,
    composedBarDataKeys,
    composedBarSize,
    composedMaxBarSize,
    composedBarGap,
    composedStacked,
    composedStackOffsets,
    composedStackGap,
    tooltipData,
  } = useChart();

  const barKeys = useMemo(() => {
    if (composedBarDataKeys && composedBarDataKeys.length > 0) {
      return composedBarDataKeys;
    }
    return [dataKey];
  }, [composedBarDataKeys, dataKey]);

  const seriesIndex = useMemo(() => {
    const idx = barKeys.indexOf(dataKey);
    return idx >= 0 ? idx : 0;
  }, [barKeys, dataKey]);

  const n = barKeys.length;
  const gap = composedBarGap ?? 4;
  const stackGap = composedStackGap ?? 0;

  const stacked =
    Boolean(composedStacked) &&
    composedStackOffsets != null &&
    composedBarDataKeys != null &&
    composedBarDataKeys.length > 0;

  const isLastSeries = seriesIndex === n - 1;

  const barWidth = useMemo(
    () =>
      computeSeriesBarWidth({
        innerWidth,
        dataLength: data.length,
        columnWidth,
        seriesCount: n,
        composedBarSize,
        composedMaxBarSize,
        composedBarGap: gap,
        stacked,
      }),
    [
      columnWidth,
      composedBarSize,
      composedMaxBarSize,
      data.length,
      gap,
      innerWidth,
      n,
      stacked,
    ]
  );

  const totalAnimDuration = animationDuration || 1100;
  const staggerSpread = totalAnimDuration * 0.4;
  const calculatedStaggerDelay =
    data.length > 1 ? staggerSpread / 1000 / data.length : 0;
  if (barScale) {
    console.warn(
      "SeriesBar is for time-based ComposedChart / LineChart context. Use Bar inside BarChart for categorical x."
    );
    return null;
  }

  const hoveredIndex = tooltipData?.index ?? null;

  return (
    <g className="series-bar">
      {data.map((d, i) => {
        const value = d[dataKey];
        if (typeof value !== "number") {
          return null;
        }

        const xCenter = xScale(xAccessor(d)) ?? 0;

        const { barLeft, valueY, barHeight, effectiveRadius } =
          computeSeriesBarLayout({
            stacked,
            composedStackOffsets,
            rowIndex: i,
            dataKey,
            value,
            yScale,
            innerHeight,
            xCenter,
            barWidth,
            seriesCount: n,
            gap,
            seriesIndex,
            stackGap,
            isLastSeries,
            radius,
          });

        const categoryLabel = String(xAccessor(d).getTime());
        const isFaded = hoveredIndex !== null && hoveredIndex !== i;

        if (animate && !isLoaded) {
          return (
            <SeriesBarRect
              barHeight={barHeight}
              barWidth={barWidth}
              calculatedStaggerDelay={calculatedStaggerDelay}
              enterTransition={enterTransition}
              fadedOpacity={fadedOpacity}
              fill={fill}
              index={i}
              innerHeight={innerHeight}
              isFaded={isFaded}
              key={`${dataKey}-${categoryLabel}-${revealEpoch}`}
              radius={effectiveRadius}
              revealEpoch={revealEpoch}
              x={barLeft}
              y={valueY}
            />
          );
        }

        return (
          <motion.rect
            animate={{ opacity: isFaded ? fadedOpacity : 1 }}
            fill={fill}
            height={barHeight}
            key={`${dataKey}-${categoryLabel}`}
            rx={effectiveRadius}
            ry={effectiveRadius}
            transition={{ opacity: { duration: 0.12 } }}
            width={barWidth}
            x={barLeft}
            y={valueY}
          />
        );
      })}
    </g>
  );
}

SeriesBar.displayName = "SeriesBar";

interface SeriesBarRectProps {
  x: number;
  y: number;
  barWidth: number;
  barHeight: number;
  fill: string;
  radius: number;
  index: number;
  innerHeight: number;
  calculatedStaggerDelay: number;
  enterTransition?: Transition;
  revealEpoch: number;
  isFaded: boolean;
  fadedOpacity: number;
}

function SeriesBarRect({
  x,
  y,
  barWidth,
  barHeight,
  fill,
  radius,
  index,
  innerHeight,
  calculatedStaggerDelay,
  enterTransition,
  revealEpoch,
  isFaded,
  fadedOpacity,
}: SeriesBarRectProps) {
  const enterAnim = transitionWithDelay(
    enterTransition,
    index * calculatedStaggerDelay
  );

  return (
    <motion.rect
      animate={{
        height: barHeight,
        y,
        opacity: isFaded ? fadedOpacity : 1,
      }}
      fill={fill}
      initial={{ height: 0, y: innerHeight, opacity: 1 }}
      key={`series-bar-${index}-${revealEpoch}`}
      rx={radius}
      ry={radius}
      transition={enterAnim}
      width={barWidth}
      x={x}
    />
  );
}

export default SeriesBar;
```

##### File: `src/charts/series-bar-layout.ts`

```tsx
export function computeSeriesBarWidth(input: {
  innerWidth: number;
  dataLength: number;
  columnWidth: number;
  seriesCount: number;
  composedBarSize?: number;
  composedMaxBarSize?: number;
  composedBarGap?: number;
  stacked?: boolean;
}): number {
  const {
    innerWidth,
    dataLength,
    columnWidth,
    seriesCount,
    composedBarSize,
    composedMaxBarSize,
    composedBarGap = 4,
    stacked = false,
  } = input;

  const gap = composedBarGap;
  const groupCount = stacked ? 1 : Math.max(1, seriesCount);
  let slot = columnWidth;
  if (slot <= 0) {
    slot = dataLength < 2 ? innerWidth : innerWidth / (dataLength - 1);
  }

  let width =
    composedBarSize ??
    Math.min(slot * 0.88, composedMaxBarSize ?? Number.POSITIVE_INFINITY);
  if (composedMaxBarSize != null) {
    width = Math.min(width, composedMaxBarSize);
  }
  if (groupCount > 1) {
    const maxGroup = slot * 0.92;
    const needed = groupCount * width + (groupCount - 1) * gap;
    if (needed > maxGroup && maxGroup > 0) {
      width = Math.max(4, (maxGroup - (groupCount - 1) * gap) / groupCount);
    }
  }

  return Math.max(2, width);
}

/** Half-width of the bar group at each x — used to pad reveal clips. */
export function computeSeriesBarRevealClipPadding(input: {
  barWidth: number;
  seriesCount: number;
  gap?: number;
  stacked?: boolean;
}): number {
  const { barWidth, seriesCount, gap = 4, stacked = false } = input;

  if (stacked || seriesCount <= 1) {
    return Math.ceil(barWidth / 2);
  }

  const groupWidth = seriesCount * barWidth + (seriesCount - 1) * gap;
  return Math.ceil(groupWidth / 2);
}
```

##### File: `src/charts/time-series-chart-shell.tsx`

```tsx
"use client";

import { scaleLinear, scaleTime } from "@visx/scale";
import { bisector, extent } from "d3-array";
import type { Transition } from "motion/react";
import {
  Children,
  cloneElement,
  isValidElement,
  memo,
  type ReactElement,
  type ReactNode,
  useCallback,
  useEffect,
  useMemo,
  useState,
} from "react";
import { DEFAULT_ANIMATION_EASING } from "./animation";
import { ChartProvider, type LineConfig, type Margin } from "./chart-context";
import { isGradientDefComponent, isPatternDefComponent } from "./chart-defs";
import { shortDateFmt } from "./chart-formatters";
import { ChartRevealClip } from "./chart-reveal-clip";
import {
  decimateTimeSeries,
  maxRenderPointsForWidth,
} from "./decimate-time-series";
import {
  computeSeriesBarRevealClipPadding,
  computeSeriesBarWidth,
} from "./series-bar-layout";
import { useStaticChartPreview } from "./static-chart-preview-context";
import { useChartInteraction } from "./use-chart-interaction";

function collectNumericExtents(
  data: Record<string, unknown>[],
  dataKeys: string[]
) {
  let minValue = Number.POSITIVE_INFINITY;
  let maxValue = Number.NEGATIVE_INFINITY;

  for (const d of data) {
    for (const key of dataKeys) {
      const value = d[key];
      if (typeof value === "number") {
        if (value < minValue) {
          minValue = value;
        }
        if (value > maxValue) {
          maxValue = value;
        }
      }
    }
  }

  if (minValue === Number.POSITIVE_INFINITY) {
    return { minValue: 0, maxValue: 100 };
  }

  return { minValue, maxValue };
}

function resolveTimeSeriesYDomain(
  data: Record<string, unknown>[],
  dataKeys: string[],
  yScaleDomainMax: number | undefined
): [number, number] {
  if (yScaleDomainMax != null && yScaleDomainMax > 0) {
    return [0, yScaleDomainMax * 1.1];
  }

  const { minValue, maxValue } = collectNumericExtents(data, dataKeys);

  if (minValue >= 0) {
    const top = maxValue <= 0 ? 100 : maxValue * 1.1;
    return [0, top];
  }

  const padding = (maxValue - minValue) * 0.05 || 1;
  return [minValue - padding, maxValue + padding];
}

/** Markers render after the interaction overlay so they stay clickable. */
export function isPostOverlayComponent(child: ReactElement): boolean {
  const childType = child.type as {
    displayName?: string;
    name?: string;
    __isChartMarkers?: boolean;
  };

  if (childType.__isChartMarkers) {
    return true;
  }

  const componentName =
    typeof child.type === "function"
      ? childType.displayName || childType.name || ""
      : "";

  return componentName === "ChartMarkers" || componentName === "MarkerGroup";
}

function ensureChildKey(child: ReactElement, index: number): ReactElement {
  if (child.key != null) {
    return child;
  }
  return cloneElement(child, { key: `chart-child-${index}` });
}

export interface TimeSeriesChartInnerProps {
  width: number;
  height: number;
  data: Record<string, unknown>[];
  xDataKey: string;
  margin: Margin;
  animationDuration: number;
  animationEasing?: string;
  enterTransition?: Transition;
  /** Signature of motion URL state — triggers reveal replay when it changes. */
  revealSignature?: string;
  children: ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
  /** Series keys driving y-domain and tooltip (Line / Area / SeriesBar configs). */
  lines: LineConfig[];
  /** SVG clipPath id for grow animation. */
  clipPathId: string;
  /** Optional ComposedChart bar layout (forwarded into context). */
  composedBarDataKeys?: string[];
  composedBarSize?: number;
  composedMaxBarSize?: number;
  composedBarGap?: number;
  composedStacked?: boolean;
  composedStackOffsets?: Map<number, Map<string, number>>;
  composedStackGap?: number;
  /** When set, drives the y-axis max instead of scanning `lines` (e.g. stacked bar totals). */
  yScaleDomainMax?: number;
}

export function TimeSeriesChartInner(props: TimeSeriesChartInnerProps) {
  const { width, height } = props;
  if (width < 10 || height < 10) {
    return null;
  }
  return <TimeSeriesChartCore {...props} />;
}

const TimeSeriesChartCore = memo(function TimeSeriesChartCore({
  width,
  height,
  data,
  xDataKey,
  margin,
  animationDuration,
  animationEasing = DEFAULT_ANIMATION_EASING,
  enterTransition,
  revealSignature = "",
  children,
  containerRef,
  lines,
  clipPathId,
  composedBarDataKeys,
  composedBarSize,
  composedMaxBarSize,
  composedBarGap,
  composedStacked,
  composedStackOffsets,
  composedStackGap,
  yScaleDomainMax,
}: TimeSeriesChartInnerProps) {
  const staticPreview = useStaticChartPreview();
  const [isLoaded, setIsLoaded] = useState(staticPreview);
  const [revealEpoch, setRevealEpoch] = useState(0);

  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  const xAccessor = useCallback(
    (d: Record<string, unknown>): Date => {
      const value = d[xDataKey];
      return value instanceof Date ? value : new Date(value as string | number);
    },
    [xDataKey]
  );

  const bisectDate = useMemo(
    () => bisector<Record<string, unknown>, Date>((d) => xAccessor(d)).left,
    [xAccessor]
  );

  const xScale = useMemo(() => {
    const timeExtent = extent(data, (d) => xAccessor(d).getTime());
    const minTime = timeExtent[0] ?? 0;
    const maxTime = timeExtent[1] ?? minTime;

    return scaleTime({
      range: [0, innerWidth],
      domain: [minTime, maxTime],
    });
  }, [innerWidth, data, xAccessor]);

  const renderData = useMemo(() => {
    const valueKeys = lines.map((line) => line.dataKey);
    return decimateTimeSeries(
      data,
      maxRenderPointsForWidth(innerWidth),
      valueKeys
    );
  }, [data, innerWidth, lines]);

  const columnWidth = useMemo(() => {
    if (data.length < 2) {
      return 0;
    }
    return innerWidth / (data.length - 1);
  }, [innerWidth, data.length]);

  const yScale = useMemo(() => {
    const dataKeys = lines.map((line) => line.dataKey);
    const domain = resolveTimeSeriesYDomain(data, dataKeys, yScaleDomainMax);

    return scaleLinear({
      range: [innerHeight, 0],
      domain,
      nice: true,
    });
  }, [innerHeight, data, lines, yScaleDomainMax]);

  const dateLabels = useMemo(
    () => data.map((d) => shortDateFmt.format(xAccessor(d))),
    [data, xAccessor]
  );

  // biome-ignore lint/correctness/useExhaustiveDependencies: revealSignature
  useEffect(() => {
    if (staticPreview) {
      setIsLoaded(true);
      return;
    }

    setRevealEpoch((n) => n + 1);
    setIsLoaded(false);
    const timer = setTimeout(() => {
      setIsLoaded(true);
    }, animationDuration);
    return () => clearTimeout(timer);
  }, [animationDuration, revealSignature, staticPreview]);

  const canInteract = isLoaded;

  const {
    tooltipData,
    setTooltipData,
    selection,
    clearSelection,
    interactionHandlers,
    interactionStyle,
  } = useChartInteraction({
    xScale,
    yScale,
    data,
    lines,
    margin,
    xAccessor,
    bisectDate,
    canInteract,
  });

  const defsChildren: ReactElement[] = [];
  const preOverlayChildren: ReactElement[] = [];
  const postOverlayChildren: ReactElement[] = [];

  Children.forEach(children, (child, index) => {
    if (!isValidElement(child)) {
      return;
    }

    const keyedChild = ensureChildKey(child, index);

    if (isGradientDefComponent(keyedChild)) {
      defsChildren.push(keyedChild);
    } else if (isPatternDefComponent(keyedChild)) {
      // Keep pattern defs in the plot <g> (same as main) — hoisting breaks url(#id) fills.
      preOverlayChildren.push(keyedChild);
    } else if (isPostOverlayComponent(keyedChild)) {
      postOverlayChildren.push(keyedChild);
    } else {
      preOverlayChildren.push(keyedChild);
    }
  });

  const contextValue = useMemo(
    () => ({
      data,
      renderData,
      xScale,
      yScale,
      width,
      height,
      innerWidth,
      innerHeight,
      margin,
      columnWidth,
      tooltipData,
      setTooltipData,
      containerRef,
      lines,
      isLoaded,
      animationDuration,
      animationEasing,
      enterTransition,
      revealEpoch,
      xAccessor,
      dateLabels,
      selection,
      clearSelection,
      composedBarDataKeys,
      composedBarSize,
      composedMaxBarSize,
      composedBarGap,
      composedStacked,
      composedStackOffsets,
      composedStackGap,
    }),
    [
      data,
      renderData,
      xScale,
      yScale,
      width,
      height,
      innerWidth,
      innerHeight,
      margin,
      columnWidth,
      tooltipData,
      setTooltipData,
      containerRef,
      lines,
      isLoaded,
      animationDuration,
      animationEasing,
      enterTransition,
      revealEpoch,
      xAccessor,
      dateLabels,
      selection,
      clearSelection,
      composedBarDataKeys,
      composedBarSize,
      composedMaxBarSize,
      composedBarGap,
      composedStacked,
      composedStackOffsets,
      composedStackGap,
    ]
  );

  // Single shared reveal clip for every series. Replaces the per-<Line> /
  // per-<Area> `<ChartRevealClip>` motion.rects: one motion-driven attribute
  // animation instead of N, with all series referencing the same `<clipPath>`.
  // The wipe semantics (left-to-right unveil of static path geometry) are
  // identical to the previous per-series clips.
  // animationDuration === 0 truly disables the reveal (no clipPath wrapper),
  // so consumers can opt out without having to also pass enterTransition.
  const showReveal =
    !staticPreview &&
    renderData.length > 1 &&
    innerWidth > 0 &&
    animationDuration > 0;
  // If the consumer didn't pass an explicit enterTransition, derive one from
  // animationDuration so clipRevealTransition picks up the override instead
  // of falling back to its 1100ms default.
  const effectiveEnterTransition: Transition = enterTransition ?? {
    type: "tween",
    duration: animationDuration / 1000,
  };

  const revealClipPadding = useMemo(() => {
    if (!composedBarDataKeys?.length) {
      return 0;
    }
    const barWidth = computeSeriesBarWidth({
      innerWidth,
      dataLength: data.length,
      columnWidth,
      seriesCount: composedBarDataKeys.length,
      composedBarSize,
      composedMaxBarSize,
      composedBarGap,
      stacked: composedStacked,
    });
    return computeSeriesBarRevealClipPadding({
      barWidth,
      seriesCount: composedBarDataKeys.length,
      gap: composedBarGap,
      stacked: composedStacked,
    });
  }, [
    columnWidth,
    composedBarDataKeys,
    composedBarGap,
    composedBarSize,
    composedMaxBarSize,
    composedStacked,
    data.length,
    innerWidth,
  ]);

  return (
    <ChartProvider value={contextValue}>
      <svg aria-hidden="true" height={height} width={width}>
        <defs>
          {defsChildren}
          {showReveal ? (
            <ChartRevealClip
              clipPathId={clipPathId}
              enterTransition={effectiveEnterTransition}
              height={innerHeight + 20}
              padding={revealClipPadding}
              revealEpoch={revealEpoch}
              targetWidth={innerWidth}
            />
          ) : null}
        </defs>

        <rect fill="transparent" height={height} width={width} x={0} y={0} />

        <g
          {...interactionHandlers}
          style={interactionStyle}
          transform={`translate(${margin.left},${margin.top})`}
        >
          <rect
            fill="transparent"
            height={innerHeight}
            width={innerWidth}
            x={0}
            y={0}
          />

          {showReveal ? (
            <g clipPath={`url(#${clipPathId})`}>{preOverlayChildren}</g>
          ) : (
            preOverlayChildren
          )}
          {postOverlayChildren}
        </g>
      </svg>
    </ChartProvider>
  );
});
```

---

### Funnel Chart <a name="funnel-chart"></a>

An animated funnel chart with multi-layer halo rings, hover interactions, and staggered entrance animations

- **Category**: Charts
- **Registry Key**: `funnel-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/funnel-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
- **Local Registry Dependencies**:
  - `@bklit/chart-animation`
  - `@bklit/utils`
  - `@bklit/chart-utils`

#### How to Use

```tsx
import { FunnelChart } from "@bklitui/ui/charts";

const data = [
  { label: "Visitors", value: 12400, displayValue: "12.4k" },
  { label: "Leads", value: 6800, displayValue: "6.8k" },
  { label: "Qualified", value: 3200, displayValue: "3.2k" },
  { label: "Proposals", value: 1500, displayValue: "1.5k" },
  { label: "Closed", value: 620, displayValue: "620" },
];

export default function SalesFunnel() {
  return (
    <FunnelChart
      data={data}
      color="var(--chart-1)"
      layers={3}
    />
  );
}
```

#### Props & API Reference

##### `FunnelChart` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `data` | `FunnelStage[]` | required | Array of funnel stages |
| `orientation` | `"horizontal" \| "vertical"` | `"horizontal"` | Layout direction |
| `color` | `string` | `"var(--chart-1)"` | Default color for all segments |
| `layers` | `number` | `3` | Number of concentric halo rings per segment |
| `edges` | `"curved" \| "straight"` | `"curved"` | Edge style for segment shapes |
| `gap` | `number` | `4` | Gap between segments in pixels |
| `staggerDelay` | `number` | `0.12` | Stagger delay between segment animations (seconds) |
| `showPercentage` | `boolean` | `true` | Show percentage badges |
| `showValues` | `boolean` | `true` | Show value labels |
| `showLabels` | `boolean` | `true` | Show stage name labels |
| `formatPercentage` | `(pct: number) => string` | rounds to integer | Custom percentage formatter |
| `formatValue` | `(value: number) => string` | locale string | Custom value formatter |
| `labelLayout` | `"spread" \| "grouped"` | `"spread"` | How labels are arranged within each segment |
| `labelOrientation` | `"vertical" \| "horizontal"` | auto | Stack direction for grouped labels |
| `labelAlign` | `"center" \| "start" \| "end"` | `"center"` | Alignment of grouped labels |
| `hoveredIndex` | `number \| null` | - | Controlled hover state (segment index) |
| `onHoverChange` | `(index: number \| null) => void` | - | Callback when hover state changes |
| `grid` | `boolean \| GridConfig` | `false` | Background bands and grid lines |
| `renderPattern` | `(id: string, color: string) => ReactNode` | - | Custom SVG pattern for the innermost ring |
| `className` | `string` | - | Additional CSS class |
| `style` | `CSSProperties` | - | Additional inline styles |

#### Component Source Code

##### File: `src/charts/funnel-chart.tsx`

```tsx
"use client";

import type { Transition } from "motion/react";
import { motion, useSpring, useTransform } from "motion/react";
import {
  type CSSProperties,
  type ReactNode,
  useCallback,
  useEffect,
  useRef,
  useState,
} from "react";
import { cn } from "@/lib/utils";
import { useMountProgress } from "./use-mount-progress";

// ─── Public types ───────────────────────────────────────────────────

export interface FunnelGradientStop {
  offset: string | number;
  color: string;
}

export interface FunnelStage {
  label: string;
  value: number;
  displayValue?: string;
  /** Override the chart-level color for this segment */
  color?: string;
  /**
   * Apply a linear gradient to this segment.
   * Provide an array of color stops, e.g. `[{ offset: "0%", color: "#8B5CF6" }, { offset: "100%", color: "#3B82F6" }]`.
   * When set, this takes priority over the segment and chart-level `color` for the innermost ring.
   * Outer halo rings use the first stop color as their solid color.
   */
  gradient?: FunnelGradientStop[];
}

export interface FunnelChartProps {
  data: FunnelStage[];
  orientation?: "horizontal" | "vertical";
  color?: string;
  layers?: number;
  className?: string;
  style?: CSSProperties;
  showPercentage?: boolean;
  showValues?: boolean;
  showLabels?: boolean;
  /** Controlled hover state — index of the hovered segment */
  hoveredIndex?: number | null;
  /** Callback when hover state changes */
  onHoverChange?: (index: number | null) => void;
  formatPercentage?: (pct: number) => string;
  formatValue?: (value: number) => string;
  /** Stagger delay between segments in seconds. Default 0.12 */
  staggerDelay?: number;
  /** Framer Motion transition for segment enter animation */
  enterTransition?: Transition;
  /** Gap between segments in pixels. Default 4 */
  gap?: number;
  /**
   * Render a visx pattern definition. Receives a unique `id` string per segment
   * and the resolved `color`. Return a `<PatternLines>` (or any visx pattern)
   * inside an SVG `<defs>`. The component will use `fill="url(#id)"` on the
   * innermost ring while keeping outer halo rings as solid color.
   */
  renderPattern?: (id: string, color: string) => ReactNode;
  /** Edge style for the funnel segments. Default "curved" */
  edges?: "curved" | "straight";
  /**
   * Controls how segment labels (value, percentage, stage name) are arranged.
   * - "spread": Value/percentage/label are spread apart (top/center/bottom for horizontal,
   *   left/center/right for vertical). This is the default.
   * - "grouped": All label items stack together in a tight group.
   *
   * When "grouped", use `labelOrientation` and `labelAlign` for full control.
   */
  labelLayout?: "spread" | "grouped";
  /**
   * Stack direction of the label group. Only applies when `labelLayout="grouped"`.
   * - "vertical": Items stack top-to-bottom. Default for horizontal funnels.
   * - "horizontal": Items stack left-to-right. Default for vertical funnels.
   */
  labelOrientation?: "vertical" | "horizontal";
  /**
   * Where the label group sits within the segment cell.
   * - "center" (default), "start", "end"
   * For horizontal funnel: start=top, end=bottom.
   * For vertical funnel: start=left, end=right.
   */
  labelAlign?: "center" | "start" | "end";
  /** Grid configuration. Pass `true` for default bands + lines, or an object for fine control. */
  grid?:
    | boolean
    | {
        /** Show alternating background bands behind each segment. Default true */
        bands?: boolean;
        /** Color of the background bands. Default "var(--color-muted)" */
        bandColor?: string;
        /** Show grid lines at each gap between segments. Default true */
        lines?: boolean;
        /** Color of the grid lines. Default "var(--chart-grid)" */
        lineColor?: string;
        /** Opacity of the grid lines. Default 1 */
        lineOpacity?: number;
        /** Width of the grid lines in pixels. Default 1 */
        lineWidth?: number;
      };
}

// ─── Defaults ───────────────────────────────────────────────────────

import { intFmt } from "./chart-formatters";

const fmtPct = (p: number) => `${Math.round(p)}%`;
const fmtVal = intFmt;

const hoverSpring = { stiffness: 300, damping: 24 };

// ─── SVG helpers ────────────────────────────────────────────────────

/**
 * Builds a single segment path for one stage in the funnel.
 * Each segment is a smooth trapezoid-like shape transitioning from
 * the height of the current norm to the next norm.
 */
function hSegmentPath(
  normStart: number,
  normEnd: number,
  segW: number,
  H: number,
  layerScale: number,
  straight = false
) {
  const my = H / 2;
  const h0 = normStart * H * 0.44 * layerScale;
  const h1 = normEnd * H * 0.44 * layerScale;

  if (straight) {
    return `M 0 ${my - h0} L ${segW} ${my - h1} L ${segW} ${my + h1} L 0 ${my + h0} Z`;
  }

  const cx = segW * 0.55;
  const top = `M 0 ${my - h0} C ${cx} ${my - h0}, ${segW - cx} ${my - h1}, ${segW} ${my - h1}`;
  const bot = `L ${segW} ${my + h1} C ${segW - cx} ${my + h1}, ${cx} ${my + h0}, 0 ${my + h0}`;
  return `${top} ${bot} Z`;
}

function vSegmentPath(
  normStart: number,
  normEnd: number,
  segH: number,
  W: number,
  layerScale: number,
  straight = false
) {
  const mx = W / 2;
  const w0 = normStart * W * 0.44 * layerScale;
  const w1 = normEnd * W * 0.44 * layerScale;

  if (straight) {
    return `M ${mx - w0} 0 L ${mx - w1} ${segH} L ${mx + w1} ${segH} L ${mx + w0} 0 Z`;
  }

  const cy = segH * 0.55;
  const left = `M ${mx - w0} 0 C ${mx - w0} ${cy}, ${mx - w1} ${segH - cy}, ${mx - w1} ${segH}`;
  const right = `L ${mx + w1} ${segH} C ${mx + w1} ${segH - cy}, ${mx + w0} ${cy}, ${mx + w0} 0`;
  return `${left} ${right} Z`;
}

// ─── Animated Segment ───────────────────────────────────────────────

function HRing({
  d,
  color,
  fill,
  opacity,
  hovered,
  ringIndex,
  totalRings,
}: {
  d: string;
  color: string;
  fill?: string;
  opacity: number;
  hovered: boolean;
  ringIndex: number;
  totalRings: number;
}) {
  // Outer rings get progressively more hover expansion and a softer spring
  const extraScale = 1 + (ringIndex / Math.max(totalRings - 1, 1)) * 0.12;
  const ringSpring = {
    stiffness: 300 - ringIndex * 60,
    damping: 24 - ringIndex * 3,
  };
  const scaleY = useSpring(1, ringSpring);

  useEffect(() => {
    scaleY.set(hovered ? extraScale : 1);
  }, [hovered, scaleY, extraScale]);

  return (
    <motion.path
      d={d}
      fill={fill ?? color}
      opacity={opacity}
      style={{ scaleY, transformOrigin: "center center" }}
    />
  );
}

function HSegment({
  index,
  normStart,
  normEnd,
  segW,
  fullH,
  color,
  layers,
  staggerDelay,
  enterTransition,
  hovered,
  dimmed,
  renderPattern,
  straight,
  gradientStops,
}: {
  index: number;
  normStart: number;
  normEnd: number;
  segW: number;
  fullH: number;
  color: string;
  layers: number;
  staggerDelay: number;
  enterTransition?: Transition;
  hovered: boolean;
  dimmed: boolean;
  renderPattern?: (id: string, color: string) => ReactNode;
  straight: boolean;
  gradientStops?: FunnelGradientStop[];
}) {
  const patternId = `funnel-h-pattern-${index}`;
  const gradientId = `funnel-h-grad-${index}`;
  const mountProgress = useMountProgress(
    enterTransition,
    index * staggerDelay,
    index
  );
  const entranceScaleX = useTransform(mountProgress, [0, 1], [0, 1]);
  const entranceScaleY = useTransform(mountProgress, [0, 1], [0, 1]);

  // Dim spring: reduces opacity when another segment is hovered
  const dimOpacity = useSpring(1, hoverSpring);

  useEffect(() => {
    dimOpacity.set(dimmed ? 0.4 : 1);
  }, [dimmed, dimOpacity]);

  const rings = Array.from({ length: layers }, (_, l) => {
    const scale = 1 - (l / layers) * 0.35;
    const opacity = 0.18 + (l / (layers - 1 || 1)) * 0.65;
    return {
      d: hSegmentPath(normStart, normEnd, segW, fullH, scale, straight),
      opacity,
    };
  });

  return (
    <motion.div
      className="pointer-events-none relative shrink-0 overflow-visible"
      style={{
        width: segW,
        height: fullH,
        zIndex: hovered ? 10 : 1,
        opacity: dimOpacity,
      }}
    >
      {/* Entrance animation wrapper: grows from left-center */}
      <motion.div
        className="absolute inset-0 overflow-visible"
        style={{
          scaleX: entranceScaleX,
          scaleY: entranceScaleY,
          transformOrigin: "left center",
        }}
      >
        <svg
          aria-hidden="true"
          className="absolute inset-0 h-full w-full overflow-visible"
          preserveAspectRatio="none"
          role="presentation"
          viewBox={`0 0 ${segW} ${fullH}`}
        >
          <defs>
            {gradientStops && (
              <linearGradient id={gradientId} x1="0" x2="1" y1="0" y2="0">
                {gradientStops.map((stop) => (
                  <stop
                    key={`${stop.offset}-${stop.color}`}
                    offset={
                      typeof stop.offset === "number"
                        ? `${stop.offset * 100}%`
                        : stop.offset
                    }
                    stopColor={stop.color}
                  />
                ))}
              </linearGradient>
            )}
            {renderPattern?.(patternId, color)}
          </defs>
          {rings.map((r, i) => {
            const isInnermost = i === rings.length - 1;
            let ringFill: string | undefined;
            if (isInnermost && renderPattern) {
              ringFill = `url(#${patternId})`;
            } else if (isInnermost && gradientStops) {
              ringFill = `url(#${gradientId})`;
            }
            const ringKey = `h-ring-${r.opacity.toFixed(2)}`;
            return (
              <HRing
                color={color}
                d={r.d}
                fill={ringFill}
                hovered={hovered}
                key={ringKey}
                opacity={r.opacity}
                ringIndex={i}
                totalRings={layers}
              />
            );
          })}
        </svg>
      </motion.div>
    </motion.div>
  );
}

function VRing({
  d,
  color,
  fill,
  opacity,
  hovered,
  ringIndex,
  totalRings,
}: {
  d: string;
  color: string;
  fill?: string;
  opacity: number;
  hovered: boolean;
  ringIndex: number;
  totalRings: number;
}) {
  // Outer rings get progressively more hover expansion and a softer spring
  const extraScale = 1 + (ringIndex / Math.max(totalRings - 1, 1)) * 0.12;
  const ringSpring = {
    stiffness: 300 - ringIndex * 60,
    damping: 24 - ringIndex * 3,
  };
  const scaleX = useSpring(1, ringSpring);

  useEffect(() => {
    scaleX.set(hovered ? extraScale : 1);
  }, [hovered, scaleX, extraScale]);

  return (
    <motion.path
      d={d}
      fill={fill ?? color}
      opacity={opacity}
      style={{ scaleX, transformOrigin: "center center" }}
    />
  );
}

function VSegment({
  index,
  normStart,
  normEnd,
  segH,
  fullW,
  color,
  layers,
  staggerDelay,
  enterTransition,
  hovered,
  dimmed,
  renderPattern,
  straight,
  gradientStops,
}: {
  index: number;
  normStart: number;
  normEnd: number;
  segH: number;
  fullW: number;
  color: string;
  layers: number;
  staggerDelay: number;
  enterTransition?: Transition;
  hovered: boolean;
  dimmed: boolean;
  renderPattern?: (id: string, color: string) => ReactNode;
  straight: boolean;
  gradientStops?: FunnelGradientStop[];
}) {
  const patternId = `funnel-v-pattern-${index}`;
  const gradientId = `funnel-v-grad-${index}`;
  const mountProgress = useMountProgress(
    enterTransition,
    index * staggerDelay,
    index
  );
  const entranceScaleY = useTransform(mountProgress, [0, 1], [0, 1]);
  const entranceScaleX = useTransform(mountProgress, [0, 1], [0, 1]);

  // Dim spring: reduces opacity when another segment is hovered
  const dimOpacity = useSpring(1, hoverSpring);

  useEffect(() => {
    dimOpacity.set(dimmed ? 0.4 : 1);
  }, [dimmed, dimOpacity]);

  const rings = Array.from({ length: layers }, (_, l) => {
    const scale = 1 - (l / layers) * 0.35;
    const opacity = 0.18 + (l / (layers - 1 || 1)) * 0.65;
    return {
      d: vSegmentPath(normStart, normEnd, segH, fullW, scale, straight),
      opacity,
    };
  });

  return (
    <motion.div
      className="pointer-events-none relative shrink-0 overflow-visible"
      style={{
        width: fullW,
        height: segH,
        zIndex: hovered ? 10 : 1,
        opacity: dimOpacity,
      }}
    >
      {/* Entrance animation wrapper: grows from top-center */}
      <motion.div
        className="absolute inset-0 overflow-visible"
        style={{
          scaleY: entranceScaleY,
          scaleX: entranceScaleX,
          transformOrigin: "center top",
        }}
      >
        <svg
          aria-hidden="true"
          className="absolute inset-0 h-full w-full overflow-visible"
          preserveAspectRatio="none"
          role="presentation"
          viewBox={`0 0 ${fullW} ${segH}`}
        >
          <defs>
            {gradientStops && (
              <linearGradient id={gradientId} x1="0" x2="0" y1="0" y2="1">
                {gradientStops.map((stop) => (
                  <stop
                    key={`${stop.offset}-${stop.color}`}
                    offset={
                      typeof stop.offset === "number"
                        ? `${stop.offset * 100}%`
                        : stop.offset
                    }
                    stopColor={stop.color}
                  />
                ))}
              </linearGradient>
            )}
            {renderPattern?.(patternId, color)}
          </defs>
          {rings.map((r, i) => {
            const isInnermost = i === rings.length - 1;
            let ringFill: string | undefined;
            if (isInnermost && renderPattern) {
              ringFill = `url(#${patternId})`;
            } else if (isInnermost && gradientStops) {
              ringFill = `url(#${gradientId})`;
            }
            const ringKey = `v-ring-${r.opacity.toFixed(2)}`;
            return (
              <VRing
                color={color}
                d={r.d}
                fill={ringFill}
                hovered={hovered}
                key={ringKey}
                opacity={r.opacity}
                ringIndex={i}
                totalRings={layers}
              />
            );
          })}
        </svg>
      </motion.div>
    </motion.div>
  );
}

// ─── Label overlay ──────────────────────────────────────────────────

function SegmentLabel({
  stage,
  pct,
  isHorizontal,
  showValues,
  showPercentage,
  showLabels,
  formatPercentage,
  formatValue,
  index,
  staggerDelay,
  layout = "spread",
  orientation,
  align = "center",
}: {
  stage: FunnelStage;
  pct: number;
  isHorizontal: boolean;
  showValues: boolean;
  showPercentage: boolean;
  showLabels: boolean;
  formatPercentage: (p: number) => string;
  formatValue: (v: number) => string;
  index: number;
  staggerDelay: number;
  layout?: "spread" | "grouped";
  orientation?: "vertical" | "horizontal";
  align?: "center" | "start" | "end";
}) {
  const display = stage.displayValue ?? formatValue(stage.value);

  const valueEl = showValues && (
    <span className="whitespace-nowrap font-semibold text-foreground text-sm">
      {display}
    </span>
  );
  const pctEl = showPercentage && (
    <span className="rounded-full bg-foreground px-3 py-1 font-bold text-background text-xs shadow-sm">
      {formatPercentage(pct)}
    </span>
  );
  const labelEl = showLabels && (
    <span className="whitespace-nowrap font-medium text-muted-foreground text-xs">
      {stage.label}
    </span>
  );

  // ── Spread layout (default): items pushed to edges with center element ──
  if (layout === "spread") {
    return (
      <motion.div
        animate={{ opacity: 1 }}
        className={cn(
          "absolute inset-0 flex",
          isHorizontal ? "flex-col items-center" : "flex-row items-center"
        )}
        initial={{ opacity: 0 }}
        transition={{
          delay: index * staggerDelay + 0.25,
          duration: 0.35,
          ease: "easeOut",
        }}
      >
        {isHorizontal ? (
          <>
            <div className="flex h-[16%] items-end justify-center pb-1">
              {valueEl}
            </div>
            <div className="flex flex-1 items-center justify-center">
              {pctEl}
            </div>
            <div className="flex h-[16%] items-start justify-center pt-1">
              {labelEl}
            </div>
          </>
        ) : (
          <>
            <div className="flex w-[16%] items-center justify-end pr-2">
              {valueEl}
            </div>
            <div className="flex flex-1 items-center justify-center">
              {pctEl}
            </div>
            <div className="flex w-[16%] items-center justify-start pl-2">
              {labelEl}
            </div>
          </>
        )}
      </motion.div>
    );
  }

  // ── Grouped layout: items stacked tightly together ──
  const resolvedOrientation =
    orientation ?? (isHorizontal ? "vertical" : "horizontal");
  const isVerticalStack = resolvedOrientation === "vertical";

  // Map align to flexbox alignment on the cross axes
  const justifyMap = {
    start: "justify-start",
    center: "justify-center",
    end: "justify-end",
  } as const;
  const itemsMap = {
    start: "items-start",
    center: "items-center",
    end: "items-end",
  } as const;

  // The outer container uses the chart orientation to position the group,
  // and the inner group uses the label orientation for stacking.
  return (
    <motion.div
      animate={{ opacity: 1 }}
      className={cn(
        "absolute inset-0 flex",
        // For horizontal funnel, align controls vertical placement
        // For vertical funnel, align controls horizontal placement
        isHorizontal
          ? cn("flex-col items-center", justifyMap[align])
          : cn("flex-row items-center", justifyMap[align])
      )}
      initial={{ opacity: 0 }}
      style={{
        padding: isHorizontal ? "8% 0" : "0 8%",
      }}
      transition={{
        delay: index * staggerDelay + 0.25,
        duration: 0.35,
        ease: "easeOut",
      }}
    >
      <div
        className={cn(
          "flex gap-1.5",
          isVerticalStack
            ? cn("flex-col", itemsMap[isHorizontal ? "center" : align])
            : cn("flex-row", itemsMap.center)
        )}
      >
        {valueEl}
        {pctEl}
        {labelEl}
      </div>
    </motion.div>
  );
}

// ─── Main Component ─────────────────────────────────────────────────

export function FunnelChart({
  data,
  orientation = "horizontal",
  color = "var(--chart-1)",
  layers = 3,
  className,
  style,
  showPercentage = true,
  showValues = true,
  showLabels = true,
  hoveredIndex: hoveredIndexProp,
  onHoverChange,
  formatPercentage = fmtPct,
  formatValue = fmtVal,
  staggerDelay = 0.12,
  enterTransition,
  gap = 4,
  renderPattern,
  edges = "curved",
  labelLayout = "spread",
  labelOrientation,
  labelAlign = "center",
  grid: gridProp = false,
}: FunnelChartProps) {
  const ref = useRef<HTMLDivElement>(null);
  const [sz, setSz] = useState({ w: 0, h: 0 });
  const [internalHoveredIndex, setInternalHoveredIndex] = useState<
    number | null
  >(null);

  const isControlled = hoveredIndexProp !== undefined;
  const hoveredIndex = isControlled ? hoveredIndexProp : internalHoveredIndex;
  const setHoveredIndex = useCallback(
    (index: number | null) => {
      if (isControlled) {
        onHoverChange?.(index);
      } else {
        setInternalHoveredIndex(index);
      }
    },
    [isControlled, onHoverChange]
  );

  const measure = useCallback(() => {
    if (!ref.current) {
      return;
    }
    const { width: w, height: h } = ref.current.getBoundingClientRect();
    if (w > 0 && h > 0) {
      setSz({ w, h });
    }
  }, []);

  useEffect(() => {
    measure();
    const ro = new ResizeObserver(measure);
    if (ref.current) {
      ro.observe(ref.current);
    }
    return () => ro.disconnect();
  }, [measure]);

  if (!data.length) {
    return null;
  }

  const first = data[0];
  if (!first) {
    return null;
  }
  const max = first.value;
  const n = data.length;
  const norms = data.map((d) => d.value / max);
  const horiz = orientation === "horizontal";
  const { w: W, h: H } = sz;

  const totalGap = gap * (n - 1);
  const segW = (W - (horiz ? totalGap : 0)) / n;
  const segH = (H - (horiz ? 0 : totalGap)) / n;

  // Resolve grid config
  const gridEnabled = gridProp !== false;
  const gridCfg = typeof gridProp === "object" ? gridProp : {};
  const showBands = gridEnabled && (gridCfg.bands ?? true);
  const bandColor = gridCfg.bandColor ?? "var(--color-muted)";
  const showGridLines = gridEnabled && (gridCfg.lines ?? true);
  const gridLineColor = gridCfg.lineColor ?? "var(--chart-grid)";
  const gridLineOpacity = gridCfg.lineOpacity ?? 1;
  const gridLineWidth = gridCfg.lineWidth ?? 1;

  return (
    <div
      className={cn("relative w-full select-none overflow-visible", className)}
      ref={ref}
      style={{
        aspectRatio: horiz ? "2.2 / 1" : "1 / 1.8",
        ...style,
      }}
    >
      {W > 0 && H > 0 && (
        <>
          {/* Grid layer: background bands + grid lines */}
          {gridEnabled && (
            <svg
              aria-hidden="true"
              className="pointer-events-none absolute inset-0 h-full w-full"
              preserveAspectRatio="none"
              role="presentation"
              viewBox={`0 0 ${W} ${H}`}
            >
              {/* Background bands — alternating on even segments */}
              {showBands &&
                data.map((stage, i) => {
                  if (i % 2 !== 0) {
                    return null;
                  }
                  if (horiz) {
                    const x = (segW + gap) * i;
                    return (
                      <rect
                        fill={bandColor}
                        height={H}
                        key={`band-${stage.label}`}
                        width={segW}
                        x={x}
                        y={0}
                      />
                    );
                  }
                  const y = (segH + gap) * i;
                  return (
                    <rect
                      fill={bandColor}
                      height={segH}
                      key={`band-${stage.label}`}
                      width={W}
                      x={0}
                      y={y}
                    />
                  );
                })}
            </svg>
          )}

          {/* Segments container — overflow-visible so hover scale is not clipped */}
          <div
            className={cn(
              "absolute inset-0 flex overflow-visible",
              horiz ? "flex-row" : "flex-col"
            )}
            style={{ gap }}
          >
            {data.map((stage, i) => {
              const normStart = norms[i] ?? 0;
              const normEnd = norms[Math.min(i + 1, n - 1)] ?? 0;
              const firstStop = stage.gradient?.[0];
              const segColor = firstStop
                ? firstStop.color
                : (stage.color ?? color);

              return horiz ? (
                <HSegment
                  color={segColor}
                  dimmed={hoveredIndex !== null && hoveredIndex !== i}
                  enterTransition={enterTransition}
                  fullH={H}
                  gradientStops={stage.gradient}
                  hovered={hoveredIndex === i}
                  index={i}
                  key={stage.label}
                  layers={layers}
                  normEnd={normEnd}
                  normStart={normStart}
                  renderPattern={renderPattern}
                  segW={segW}
                  staggerDelay={staggerDelay}
                  straight={edges === "straight"}
                />
              ) : (
                <VSegment
                  color={segColor}
                  dimmed={hoveredIndex !== null && hoveredIndex !== i}
                  enterTransition={enterTransition}
                  fullW={W}
                  gradientStops={stage.gradient}
                  hovered={hoveredIndex === i}
                  index={i}
                  key={stage.label}
                  layers={layers}
                  normEnd={normEnd}
                  normStart={normStart}
                  renderPattern={renderPattern}
                  segH={segH}
                  staggerDelay={staggerDelay}
                  straight={edges === "straight"}
                />
              );
            })}
          </div>

          {/* Grid lines — rendered above segments so they're visible */}
          {gridEnabled && showGridLines && (
            <svg
              aria-hidden="true"
              className="pointer-events-none absolute inset-0 h-full w-full"
              preserveAspectRatio="none"
              role="presentation"
              viewBox={`0 0 ${W} ${H}`}
            >
              {Array.from({ length: n - 1 }, (_, i) => {
                const idx = i + 1;
                const gridKey = `grid-${idx}`;
                if (horiz) {
                  const x = segW * idx + gap * i + gap / 2;
                  return (
                    <line
                      key={gridKey}
                      stroke={gridLineColor}
                      strokeOpacity={gridLineOpacity}
                      strokeWidth={gridLineWidth}
                      x1={x}
                      x2={x}
                      y1={0}
                      y2={H}
                    />
                  );
                }
                const y = segH * idx + gap * i + gap / 2;
                return (
                  <line
                    key={gridKey}
                    stroke={gridLineColor}
                    strokeOpacity={gridLineOpacity}
                    strokeWidth={gridLineWidth}
                    x1={0}
                    x2={W}
                    y1={y}
                    y2={y}
                  />
                );
              })}
            </svg>
          )}

          {/* Label overlays — one per segment, positioned over each segment cell.
              These are the hover triggers for each segment. */}
          {data.map((stage, i) => {
            const pct = (stage.value / max) * 100;
            const posStyle: CSSProperties = horiz
              ? {
                  left: (segW + gap) * i,
                  width: segW,
                  top: 0,
                  height: H,
                }
              : {
                  top: (segH + gap) * i,
                  height: segH,
                  left: 0,
                  width: W,
                };

            const isDimmed = hoveredIndex !== null && hoveredIndex !== i;

            return (
              <motion.div
                animate={{ opacity: isDimmed ? 0.4 : 1 }}
                className="absolute cursor-pointer"
                key={`lbl-${stage.label}`}
                onMouseEnter={() => setHoveredIndex(i)}
                onMouseLeave={() => setHoveredIndex(null)}
                style={{ ...posStyle, zIndex: 20 }}
                transition={{ type: "spring", stiffness: 300, damping: 24 }}
              >
                <SegmentLabel
                  align={labelAlign}
                  formatPercentage={formatPercentage}
                  formatValue={formatValue}
                  index={i}
                  isHorizontal={horiz}
                  layout={labelLayout}
                  orientation={labelOrientation}
                  pct={pct}
                  showLabels={showLabels}
                  showPercentage={showPercentage}
                  showValues={showValues}
                  stage={stage}
                  staggerDelay={staggerDelay}
                />
              </motion.div>
            );
          })}
        </>
      )}
    </div>
  );
}
```

---

### Gauge <a name="gauge-chart"></a>

Notch-based radial gauge with PieCenter-style NumberFlow, theme fills, optional patterns and arc gradients, and responsive sizing

- **Category**: Charts
- **Registry Key**: `gauge-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/gauge-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `@visx/responsive@4.0.1-alpha.0`
  - `@visx/pattern@4.0.1-alpha.0`
  - `@number-flow/react`
  - `motion`
  - `d3-shape`
- **Local Registry Dependencies**:
  - `@bklit/utils`

#### How to Use

```tsx
import { Gauge, PatternLines } from "@bklitui/ui/charts";

export default function RevenueGauge() {
  return (
    <Gauge
      value={66}
      centerValue={428_000}
      spacing={25}
      inactiveFillOpacity={0.4}
      defaultLabel="ARR run rate"
      formatOptions={{
        style: "currency",
        currency: "USD",
        maximumFractionDigits: 0,
      }}
    />
  );
}
```

#### Props & API Reference

##### `Gauge` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `value` | `number` | required | Arc fill **0–100** |
| `centerValue` | `number` | required | Center statistic (NumberFlow) |
| `totalNotches` | `number` | `40` | Notch count |
| `spacing` | `number` | `25` | **%** of the arc used as gaps between notches |
| `notchLengthPercent` | `number` | `100` | Radial notch depth as **%** of default (5–100); lower = shorter notches |
| `notchCornerRadius` | `number` | `0` | Corner fillet in **px** (0 = sharp); large values clamp toward a capsule shape |
| `uniformWidth` | `boolean` | `false` | Rectangular notches vs tapered |
| `startAngle` / `endAngle` | `number` | `135` / `405` | Arc sweep in degrees |
| `useGradient` | `boolean` | `false` | Per-notch color ramp along the arc |
| `activeGradient` | `[string, string]` | lime → emerald | Hex stops for active notches when `useGradient` |
| `inactiveGradient` | `[string, string]` | same as active | Hex stops for inactive notches when `useGradient` |
| `activeFill` / `inactiveFill` | `string` | `chart-1` / `chart-background` | Solid, CSS color, or `url(#patternId)` |
| `activeFillOpacity` / `inactiveFillOpacity` | `number` | `1` / `0.8` | SVG `fill-opacity` (0–1) for active / track notches |
| `defaultLabel` | `string` | `"Total"` | Center label |
| `formatOptions` | `ChartStatFlowFormat` | standard | NumberFlow format |
| `prefix` / `suffix` | `string` | - | Center prefix / suffix |
| `width` / `height` | `number` | - | Fixed size; omit for responsive |
| `minWidth` | `number` | `300` | Min width (px) when responsive |
| `className` | `string` | - | Root wrapper |
| `children` | `ReactNode` | - | Defs (`Pattern*`, `*Gradient`, …) |

#### Component Source Code

##### File: `src/charts/chart-stat-flow.tsx`

```tsx
"use client";

import NumberFlow from "@number-flow/react";
import type { ReactNode } from "react";
import { cn } from "@/lib/utils";

/** Subset of `Intl.NumberFormatOptions` supported by NumberFlow */
export interface ChartStatFlowFormat {
  notation?: "standard" | "compact";
  compactDisplay?: "short" | "long";
  minimumFractionDigits?: number;
  maximumFractionDigits?: number;
  minimumIntegerDigits?: number;
  minimumSignificantDigits?: number;
  maximumSignificantDigits?: number;
  style?: "decimal" | "percent" | "currency";
  currency?: string;
  currencyDisplay?: "symbol" | "narrowSymbol" | "code" | "name";
  unit?: string;
  unitDisplay?: "short" | "long" | "narrow";
}

export const defaultChartStatFlowFormat: ChartStatFlowFormat = {
  notation: "standard",
  maximumFractionDigits: 0,
};

export interface ChartStatFlowProps {
  value: number;
  label: string;
  formatOptions?: ChartStatFlowFormat;
  prefix?: string;
  suffix?: string;
  valueClassName?: string;
  labelClassName?: string;
  icon?: ReactNode;
}

/**
 * Shared value + label stack using NumberFlow (same layout as pie / ring centers).
 * Parent should provide flex alignment and sizing when needed.
 */
export function ChartStatFlow({
  value,
  label,
  formatOptions = defaultChartStatFlowFormat,
  prefix,
  suffix,
  valueClassName = "text-2xl font-bold",
  labelClassName = "text-xs",
  icon,
}: ChartStatFlowProps) {
  return (
    <>
      {icon ? (
        <div className="mb-2 flex h-12 w-12 items-center justify-center rounded-full bg-muted/50">
          {icon}
        </div>
      ) : null}
      <span className={cn("text-foreground tabular-nums", valueClassName)}>
        <NumberFlow
          format={formatOptions}
          prefix={prefix}
          suffix={suffix}
          value={value}
          willChange
        />
      </span>
      <span className={cn("mt-0.5 text-chart-label", labelClassName)}>
        {label}
      </span>
    </>
  );
}

ChartStatFlow.displayName = "ChartStatFlow";
```

##### File: `src/charts/pie-context.tsx`

```tsx
"use client";

import type { Transition } from "motion/react";
import { createContext, type RefObject, useContext } from "react";

// CSS variable references for pie chart theming
export const pieCssVars = {
  background: "var(--chart-background)",
  foreground: "var(--chart-foreground)",
  foregroundMuted: "var(--chart-foreground-muted)",
  label: "var(--chart-label)",
  // Default slice colors from chart palette
  slice1: "var(--chart-1)",
  slice2: "var(--chart-2)",
  slice3: "var(--chart-3)",
  slice4: "var(--chart-4)",
  slice5: "var(--chart-5)",
};

// Default slice color palette
export const defaultPieColors = [
  pieCssVars.slice1,
  pieCssVars.slice2,
  pieCssVars.slice3,
  pieCssVars.slice4,
  pieCssVars.slice5,
];

export interface PieData {
  /** Display label for the slice */
  label: string;
  /** Value for the slice (determines slice size relative to total) */
  value: number;
  /** Optional color override - falls back to palette */
  color?: string;
  /** Optional fill override for patterns/gradients (e.g., "url(#patternId)") */
  fill?: string;
}

/** Arc data computed by visx Pie */
export interface PieArcData {
  data: PieData;
  index: number;
  startAngle: number;
  endAngle: number;
  padAngle: number;
  value: number;
}

export interface PieContextValue {
  // Data
  data: PieData[];
  arcs: PieArcData[];

  // Dimensions
  size: number;
  center: number;
  outerRadius: number;
  innerRadius: number;
  padAngle: number;
  cornerRadius: number;

  // Hover effect
  hoverOffset: number;

  // Hover state
  hoveredIndex: number | null;
  setHoveredIndex: (index: number | null) => void;

  // Animation state
  animationKey: number;
  isLoaded: boolean;
  enterTransition?: Transition;
  enterStaggerScale: number;

  // Container ref for portals
  containerRef: RefObject<HTMLDivElement | null>;

  // Computed values
  totalValue: number;

  // Get color for a slice index
  getColor: (index: number) => string;

  // Get fill for a slice index (supports patterns/gradients)
  getFill: (index: number) => string;
}

const PieContext = createContext<PieContextValue | null>(null);

export function PieProvider({
  children,
  value,
}: {
  children: React.ReactNode;
  value: PieContextValue;
}) {
  return <PieContext.Provider value={value}>{children}</PieContext.Provider>;
}

export function usePie(): PieContextValue {
  const context = useContext(PieContext);
  if (!context) {
    throw new Error(
      "usePie must be used within a PieProvider. " +
        "Make sure your component is wrapped in <PieChart>."
    );
  }
  return context;
}

export default PieContext;
```

##### File: `src/charts/pie-center-shell.tsx`

```tsx
"use client";

import { pie as d3Pie } from "d3-shape";
import { useCallback, useEffect, useMemo, useRef, useState } from "react";
import { PieCenter, type PieCenterProps } from "./pie-center";
import {
  defaultPieColors,
  type PieArcData,
  type PieContextValue,
  type PieData,
  PieProvider,
} from "./pie-context";

const SHELL_HOVER_OFFSET = 10;

export type PieCenterShellProps = Omit<PieCenterProps, "children"> & {
  /** Value shown with NumberFlow (same role as pie total when not hovering) */
  centerValue: number;
  /** Square reference size for pie context (matches `PieChart` `size`) */
  contextSize: number;
  /** Inner radius in px — must be > 0 so `PieCenter` renders */
  innerRadiusPx: number;
  /**
   * When true (default), the first paint uses `0` then updates to `centerValue`
   * on the next frame so NumberFlow can run an entrance transition. Subsequent
   * `centerValue` updates animate as usual.
   */
  animateEntrance?: boolean;
};

/**
 * Renders {@link PieCenter} with a minimal {@link PieProvider} so you can reuse
 * the same center layout as a donut pie without mounting slices or a full {@link PieChart}.
 */
export function PieCenterShell({
  centerValue,
  contextSize,
  innerRadiusPx,
  animateEntrance = true,
  ...pieCenterProps
}: PieCenterShellProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const introStartedRef = useRef(false);

  const [flowTotal, setFlowTotal] = useState(() =>
    animateEntrance ? 0 : centerValue
  );

  useEffect(() => {
    if (!animateEntrance) {
      setFlowTotal(centerValue);
      return;
    }

    if (!introStartedRef.current) {
      introStartedRef.current = true;
      setFlowTotal(0);
      let innerRaf = 0;
      const outerRaf = requestAnimationFrame(() => {
        innerRaf = requestAnimationFrame(() => setFlowTotal(centerValue));
      });
      return () => {
        cancelAnimationFrame(outerRaf);
        cancelAnimationFrame(innerRaf);
        introStartedRef.current = false;
      };
    }

    setFlowTotal(centerValue);
  }, [animateEntrance, centerValue]);

  const data: PieData[] = useMemo(
    () => [{ label: "_pieCenterShell", value: Math.max(flowTotal, 0) }],
    [flowTotal]
  );

  const totalValue = flowTotal;

  const arcs = useMemo((): PieArcData[] => {
    const v = data[0]?.value ?? 0;
    if (v > 0) {
      const pieGenerator = d3Pie<PieData>()
        .value((d) => d.value)
        .startAngle(-Math.PI / 2)
        .endAngle((3 * Math.PI) / 2)
        .padAngle(0)
        .sort(null);
      const computed = pieGenerator(data);
      return computed.map((arc, index) => ({
        data: arc.data,
        index,
        startAngle: arc.startAngle,
        endAngle: arc.endAngle,
        padAngle: arc.padAngle,
        value: arc.value,
      })) as PieArcData[];
    }
    const d0 = data[0];
    if (!d0) {
      return [];
    }
    return [
      {
        data: d0,
        index: 0,
        startAngle: -Math.PI / 2,
        endAngle: (3 * Math.PI) / 2,
        padAngle: 0,
        value: 0,
      },
    ];
  }, [data]);

  const getColor = useCallback((index: number) => {
    return defaultPieColors[index % defaultPieColors.length] as string;
  }, []);

  const getFill = useCallback(
    (index: number) => {
      const item = data[index];
      if (item?.fill) {
        return item.fill;
      }
      return getColor(index);
    },
    [data, getColor]
  );

  const center = contextSize / 2;
  const outerRadius = center - SHELL_HOVER_OFFSET;

  const contextValue: PieContextValue = useMemo(
    () => ({
      data,
      arcs,
      size: contextSize,
      center,
      outerRadius,
      innerRadius: innerRadiusPx,
      padAngle: 0,
      cornerRadius: 0,
      hoverOffset: SHELL_HOVER_OFFSET,
      hoveredIndex: null,
      setHoveredIndex: () => undefined,
      animationKey: 0,
      isLoaded: true,
      enterStaggerScale: 1,
      containerRef,
      totalValue,
      getColor,
      getFill,
    }),
    [
      data,
      arcs,
      contextSize,
      center,
      outerRadius,
      innerRadiusPx,
      totalValue,
      getColor,
      getFill,
    ]
  );

  return (
    <PieProvider value={contextValue}>
      <PieCenter {...pieCenterProps} />
    </PieProvider>
  );
}

PieCenterShell.displayName = "PieCenterShell";
```

##### File: `src/charts/pie-center.tsx`

```tsx
"use client";

import type { ReactNode } from "react";
import { cn } from "@/lib/utils";
import {
  chartCenterContainerClassName,
  chartCenterLabelClassName,
  chartCenterValueClassName,
} from "./chart-center-typography";
import {
  ChartStatFlow,
  type ChartStatFlowFormat,
  defaultChartStatFlowFormat,
} from "./chart-stat-flow";
import { usePie } from "./pie-context";

export interface PieCenterProps {
  /** Label shown below the value. Default: "Total" when not hovering */
  defaultLabel?: string;
  /** Format options for NumberFlow. Default: standard notation */
  formatOptions?: ChartStatFlowFormat;
  /** Custom render function for complete control over center content */
  children?: (props: {
    value: number;
    label: string;
    isHovered: boolean;
    data: { label: string; value: number; color?: string; fill?: string };
  }) => ReactNode;
  /** Additional class name for the container */
  className?: string;
  /** Class name for the value text. Scales with center size via container queries. */
  valueClassName?: string;
  /** Class name for the label text. Scales with center size via container queries. */
  labelClassName?: string;
  /** Prefix to show before the number (e.g., "$") */
  prefix?: string;
  /** Suffix to show after the number (e.g., "%") */
  suffix?: string;
}

/**
 * PieCenter displays content in the center of a donut/pie chart.
 *
 * This component renders as pure HTML (not inside SVG foreignObject) to avoid
 * Safari's WebKit bug #23113 where HTML content with CSS transforms/opacity
 * inside foreignObject renders at incorrect positions.
 *
 * The parent PieChart uses CSS Grid stacking to overlay this HTML content
 * on top of the SVG slices.
 */
export function PieCenter({
  defaultLabel = "Total",
  formatOptions = defaultChartStatFlowFormat,
  children,
  className = "",
  valueClassName = chartCenterValueClassName,
  labelClassName = chartCenterLabelClassName,
  prefix,
  suffix,
}: PieCenterProps) {
  const { data, hoveredIndex, totalValue, innerRadius } = usePie();

  const hoveredData = hoveredIndex === null ? null : data[hoveredIndex];
  const displayValue = hoveredData ? hoveredData.value : totalValue;
  const displayLabel = hoveredData ? hoveredData.label : defaultLabel;

  // Calculate center area size based on inner radius
  // Leave some padding so text doesn't touch the inner edge
  const centerSize = innerRadius * 2 - 16;

  // Don't render if there's no inner radius (solid pie, not donut)
  if (innerRadius <= 0) {
    return null;
  }

  // If custom render function is provided, use it
  if (children && hoveredData) {
    return (
      <div
        className={cn(
          chartCenterContainerClassName,
          "flex items-center justify-center",
          className
        )}
        style={{ width: centerSize, height: centerSize }}
      >
        {children({
          value: displayValue,
          label: displayLabel,
          isHovered: hoveredIndex !== null,
          data: hoveredData,
        })}
      </div>
    );
  }

  // Default center content with NumberFlow animations
  // Now renders as pure HTML, avoiding Safari's foreignObject bugs
  return (
    <div
      className={cn(
        chartCenterContainerClassName,
        "flex flex-col items-center justify-center text-center",
        className
      )}
      style={{ width: centerSize, height: centerSize }}
    >
      <ChartStatFlow
        formatOptions={formatOptions}
        label={displayLabel}
        labelClassName={labelClassName}
        prefix={prefix}
        suffix={suffix}
        value={displayValue}
        valueClassName={valueClassName}
      />
    </div>
  );
}

PieCenter.displayName = "PieCenter";

export default PieCenter;
```

##### File: `src/charts/gauge.tsx`

```tsx
"use client";

import { ParentSize } from "@visx/responsive";
import { motion, type Transition, useReducedMotion } from "motion/react";
import {
  Children,
  Fragment,
  isValidElement,
  type ReactElement,
  type ReactNode,
  useId,
  useMemo,
} from "react";
import { cn } from "@/lib/utils";
import {
  type ChartStatFlowFormat,
  defaultChartStatFlowFormat,
} from "./chart-stat-flow";
import { PieCenterShell } from "./pie-center-shell";

function isDefsComponent(child: ReactElement): boolean {
  const typeLabel =
    (child.type as { displayName?: string })?.displayName ||
    (child.type as { name?: string })?.name ||
    "";
  return (
    typeLabel.includes("Gradient") ||
    typeLabel.includes("Pattern") ||
    typeLabel === "LinearGradient" ||
    typeLabel === "RadialGradient" ||
    typeLabel === "Lines" ||
    typeLabel === "PatternLines" ||
    typeLabel === "Circles" ||
    typeLabel === "Hexagons" ||
    typeLabel === "Waves"
  );
}

function collectDefsElements(nodes: ReactNode): ReactElement[] {
  const out: ReactElement[] = [];
  Children.forEach(nodes, (child) => {
    if (!isValidElement(child)) {
      return;
    }
    if (child.type === Fragment) {
      out.push(
        ...collectDefsElements(
          (child.props as { children?: ReactNode }).children
        )
      );
      return;
    }
    if (isDefsComponent(child)) {
      out.push(child);
    }
  });
  return out;
}

function interpolateHex(
  color1: string,
  color2: string,
  factor: number
): string {
  const hex = (c: string) => Number.parseInt(c, 16);
  const r1 = hex(color1.slice(1, 3));
  const g1 = hex(color1.slice(3, 5));
  const b1 = hex(color1.slice(5, 7));
  const r2 = hex(color2.slice(1, 3));
  const g2 = hex(color2.slice(3, 5));
  const b2 = hex(color2.slice(5, 7));

  const r = Math.round(r1 + (r2 - r1) * factor);
  const g = Math.round(g1 + (g2 - g1) * factor);
  const b = Math.round(b1 + (b2 - b1) * factor);

  return `#${r.toString(16).padStart(2, "0")}${g.toString(16).padStart(2, "0")}${b.toString(16).padStart(2, "0")}`;
}

const DEFAULT_ACTIVE_GRADIENT: readonly [string, string] = [
  "#bef264",
  "#10b981",
];

const DEFAULT_ACTIVE_FILL_OPACITY = 1;
const DEFAULT_INACTIVE_FILL_OPACITY = 0.8;

const DEFAULT_NOTCH_ENTER_TRANSITION: Transition = {
  type: "spring",
  stiffness: 300,
  damping: 20,
};

export interface GaugeProps {
  /** Fill level 0–100 */
  value: number;
  /** Number of arc notches */
  totalNotches?: number;
  /** Percentage of the arc reserved for gaps between notches */
  spacing?: number;
  /**
   * Corner fillet radius for each notch corner (pixels). **0** = sharp corners;
   * higher values read more rounded; geometry clamps so large values approach a
   * capsule / near-circular silhouette.
   */
  notchCornerRadius?: number;
  /** `true` = rectangular notches; `false` = tapered toward the center */
  uniformWidth?: boolean;
  startAngle?: number;
  endAngle?: number;
  useGradient?: boolean;
  /**
   * When `useGradient` is true, active notch colors interpolate along the arc
   * between these hex stops (default lime → emerald).
   */
  activeGradient?: readonly [string, string];
  /**
   * When `useGradient` is true, inactive notch colors interpolate between these
   * hex stops. Defaults to {@link activeGradient} when omitted.
   */
  inactiveGradient?: readonly [string, string];
  /** Value passed to {@link PieCenterShell} / NumberFlow */
  centerValue: number;
  defaultLabel?: string;
  prefix?: string;
  suffix?: string;
  formatOptions?: ChartStatFlowFormat;
  /**
   * Inactive / track notch fill — CSS color or `url(#patternId)` (define patterns
   * in `children`).
   */
  inactiveFill?: string;
  /**
   * Active notch fill — CSS color or `url(#patternId)`.
   * When set, overrides solid / gradient active fills for that layer.
   */
  activeFill?: string;
  /**
   * SVG `fill-opacity` for inactive / track notches (0–1).
   * Default **0.8** (foreground default is **1**).
   */
  inactiveFillOpacity?: number;
  /**
   * SVG `fill-opacity` for active notches (0–1). Default **1**.
   */
  activeFillOpacity?: number;
  /**
   * `PatternLines`, gradients, etc. — rendered inside `<defs>` (same convention
   * as `PieChart` children).
   */
  children?: ReactNode;
  className?: string;
  /**
   * Explicit pixel size. When omitted, the gauge fills its parent; give the
   * parent a size (e.g. `min-w-[300px]` + aspect box) for responsive layouts.
   */
  width?: number;
  height?: number;
  /** Minimum width (px) when using the built-in responsive wrapper. Default 300 */
  minWidth?: number;
  /**
   * Radial depth of notches as a **%** of the built-in default (outer 42% /
   * inner 28% of `size`). **100** = full length; lower values pull the inner
   * edge toward the outer arc. Clamped **5–100**.
   */
  notchLengthPercent?: number;
  /** Framer Motion transition for notch enter animation (opacity / scale). */
  enterTransition?: Transition;
  /** Scales notch stagger delays relative to default timing (1 = reference). */
  enterStaggerScale?: number;
}

interface GaugeInnerProps extends Omit<GaugeProps, "className" | "minWidth"> {
  width: number;
  height: number;
}

function GaugeInner({
  value,
  totalNotches = 40,
  spacing = 25,
  notchCornerRadius = 0,
  uniformWidth = false,
  width,
  height,
  startAngle = 135,
  endAngle = 405,
  useGradient = false,
  activeGradient,
  inactiveGradient,
  centerValue,
  defaultLabel = "Total",
  prefix,
  suffix,
  formatOptions = defaultChartStatFlowFormat,
  inactiveFill,
  activeFill,
  inactiveFillOpacity,
  activeFillOpacity,
  children,
  notchLengthPercent = 100,
  enterTransition,
  enterStaggerScale = 1,
}: GaugeInnerProps) {
  const prefersReducedMotion = useReducedMotion();
  const themeActiveGradientId = `gauge-theme-active-${useId().replace(/:/g, "")}`;
  const defsChildren = useMemo(() => collectDefsElements(children), [children]);

  const notchTransition: Transition = prefersReducedMotion
    ? { duration: 0 }
    : (enterTransition ?? DEFAULT_NOTCH_ENTER_TRANSITION);

  const stagger = Math.max(0.25, Math.min(2.5, enterStaggerScale));

  const hasCustomInactive =
    inactiveFill !== undefined && inactiveFill.length > 0;
  const hasCustomActive = activeFill !== undefined && activeFill.length > 0;

  const resolvedActiveFillOpacity =
    activeFillOpacity ?? DEFAULT_ACTIVE_FILL_OPACITY;
  const resolvedInactiveFillOpacity =
    inactiveFillOpacity ?? DEFAULT_INACTIVE_FILL_OPACITY;

  const size = Math.min(width, height);
  const centerX = width / 2;
  const centerY = height / 2;
  const outerRadius = size * 0.42;
  const innerRadiusBase = size * 0.28;
  const defaultRadialDepth = outerRadius - innerRadiusBase;
  const depthFactor = Math.min(100, Math.max(5, notchLengthPercent)) / 100;
  const notchLength = defaultRadialDepth * depthFactor;
  const innerRadius = outerRadius - notchLength;

  const activeNotches = Math.round((value / 100) * totalNotches);

  const totalAngle = endAngle - startAngle;
  const availableAngle = totalAngle * (1 - spacing / 100);
  const notchAngle = totalNotches > 0 ? availableAngle / totalNotches : 0;
  const gapDen = totalNotches - 1 > 0 ? totalNotches - 1 : 1;
  const gapAngle = (totalAngle * (spacing / 100)) / gapDen;

  const activeGrad0 = activeGradient?.[0] ?? DEFAULT_ACTIVE_GRADIENT[0];
  const activeGrad1 = activeGradient?.[1] ?? DEFAULT_ACTIVE_GRADIENT[1];
  const inactiveGrad0 = inactiveGradient?.[0] ?? activeGrad0;
  const inactiveGrad1 = inactiveGradient?.[1] ?? activeGrad1;
  const useThemePaletteGradient = useGradient && activeGradient === undefined;

  const notches = useMemo(() => {
    return Array.from({ length: totalNotches }, (_, i) => {
      const angle = startAngle + i * (notchAngle + gapAngle) + notchAngle / 2;
      const radians = (angle * Math.PI) / 180;

      const notchWidth = notchAngle * 0.8;
      const halfWidth = (notchWidth * Math.PI) / 180 / 2;

      const x1 = centerX + Math.cos(radians - halfWidth) * outerRadius;
      const y1 = centerY + Math.sin(radians - halfWidth) * outerRadius;
      const x2 = centerX + Math.cos(radians + halfWidth) * outerRadius;
      const y2 = centerY + Math.sin(radians + halfWidth) * outerRadius;

      let x3: number;
      let y3: number;
      let x4: number;
      let y4: number;

      if (uniformWidth) {
        const perpX = Math.cos(radians);
        const perpY = Math.sin(radians);
        x3 = x2 - perpX * notchLength;
        y3 = y2 - perpY * notchLength;
        x4 = x1 - perpX * notchLength;
        y4 = y1 - perpY * notchLength;
      } else {
        x3 = centerX + Math.cos(radians + halfWidth) * innerRadius;
        y3 = centerY + Math.sin(radians + halfWidth) * innerRadius;
        x4 = centerX + Math.cos(radians - halfWidth) * innerRadius;
        y4 = centerY + Math.sin(radians - halfWidth) * innerRadius;
      }

      const denom = totalNotches > 1 ? totalNotches - 1 : 1;
      const gradientFactor = i / denom;
      const gradientColor =
        useGradient && !useThemePaletteGradient
          ? interpolateHex(activeGrad0, activeGrad1, gradientFactor)
          : "var(--chart-1)";

      return {
        index: i,
        points: { x1, y1, x2, y2, x3, y3, x4, y4 },
        isActive: i < activeNotches,
        gradientColor,
      };
    });
  }, [
    totalNotches,
    notchAngle,
    gapAngle,
    centerX,
    centerY,
    outerRadius,
    innerRadius,
    activeNotches,
    startAngle,
    uniformWidth,
    notchLength,
    activeGrad0,
    activeGrad1,
    useGradient,
    useThemePaletteGradient,
  ]);

  const createNotchPath = (
    points: {
      x1: number;
      y1: number;
      x2: number;
      y2: number;
      x3: number;
      y3: number;
      x4: number;
      y4: number;
    },
    cornerRadiusPx: number,
    radialDepth: number
  ) => {
    const { x1, y1, x2, y2, x3, y3, x4, y4 } = points;

    const lerp = (a: number, b: number, t: number) => a + (b - a) * t;
    const dist = (ax: number, ay: number, bx: number, by: number) =>
      Math.hypot(bx - ax, by - ay);

    const d12 = dist(x1, y1, x2, y2);
    const d23 = dist(x2, y2, x3, y3);
    const d34 = dist(x3, y3, x4, y4);
    const d41 = dist(x4, y4, x1, y1);

    if (cornerRadiusPx <= 0) {
      return `M ${x1} ${y1} L ${x2} ${y2} L ${x3} ${y3} L ${x4} ${y4} Z`;
    }

    const minEdge = Math.min(d12, d23, d34, d41);
    const cr = Math.min(
      cornerRadiusPx,
      radialDepth * 0.48,
      d12 * 0.49,
      d23 * 0.49,
      d34 * 0.49,
      d41 * 0.49,
      minEdge * 0.49
    );

    const r1 = Math.min(cr / d12, 0.49);
    const r2 = Math.min(cr / d23, 0.49);
    const r3 = Math.min(cr / d34, 0.49);
    const r4 = Math.min(cr / d41, 0.49);

    const p1a = { x: lerp(x1, x4, r4), y: lerp(y1, y4, r4) };
    const p1b = { x: lerp(x1, x2, r1), y: lerp(y1, y2, r1) };
    const p2a = { x: lerp(x2, x1, r1), y: lerp(y2, y1, r1) };
    const p2b = { x: lerp(x2, x3, r2), y: lerp(y2, y3, r2) };
    const p3a = { x: lerp(x3, x2, r2), y: lerp(y3, y2, r2) };
    const p3b = { x: lerp(x3, x4, r3), y: lerp(y3, y4, r3) };
    const p4a = { x: lerp(x4, x3, r3), y: lerp(y4, y3, r3) };
    const p4b = { x: lerp(x4, x1, r4), y: lerp(y4, y1, r4) };

    return `M ${p1a.x} ${p1a.y} Q ${x1} ${y1} ${p1b.x} ${p1b.y} L ${p2a.x} ${p2a.y} Q ${x2} ${y2} ${p2b.x} ${p2b.y} L ${p3a.x} ${p3a.y} Q ${x3} ${y3} ${p3b.x} ${p3b.y} L ${p4a.x} ${p4a.y} Q ${x4} ${y4} ${p4b.x} ${p4b.y} Z`;
  };

  const bgFillSolid = "var(--chart-background)";
  const activeFillSolid = "var(--chart-1)";

  const denom = totalNotches > 1 ? totalNotches - 1 : 1;

  const resolveBgFill = (notchIndex: number) => {
    if (hasCustomInactive) {
      return inactiveFill as string;
    }
    if (useThemePaletteGradient) {
      return bgFillSolid;
    }
    if (useGradient) {
      return interpolateHex(inactiveGrad0, inactiveGrad1, notchIndex / denom);
    }
    return bgFillSolid;
  };

  const resolveActiveFill = (notch: (typeof notches)[number]) => {
    if (hasCustomActive) {
      return activeFill as string;
    }
    if (useThemePaletteGradient) {
      return `url(#${themeActiveGradientId})`;
    }
    if (useGradient) {
      return notch.gradientColor;
    }
    return activeFillSolid;
  };

  return (
    <div className="relative" style={{ height, width }}>
      <svg
        aria-hidden="true"
        className="overflow-visible"
        height={height}
        viewBox={`0 0 ${width} ${height}`}
        width={width}
      >
        {defsChildren.length > 0 || useThemePaletteGradient ? (
          <defs>
            {useThemePaletteGradient ? (
              <linearGradient
                id={themeActiveGradientId}
                x1="0%"
                x2="100%"
                y1="0%"
                y2="0%"
              >
                <stop offset="0%" stopColor="var(--chart-1)" />
                <stop offset="100%" stopColor="var(--chart-5)" />
              </linearGradient>
            ) : null}
            {defsChildren}
          </defs>
        ) : null}
        {notches.map((notch) => (
          <motion.path
            animate={{ opacity: 1, scale: 1 }}
            d={createNotchPath(notch.points, notchCornerRadius, notchLength)}
            fill={resolveBgFill(notch.index)}
            fillOpacity={resolvedInactiveFillOpacity}
            initial={{ opacity: 0, scale: 0 }}
            key={`bg-${notch.index}`}
            style={{
              transformOrigin: `${centerX}px ${centerY}px`,
            }}
            transition={{
              ...notchTransition,
              delay: notch.index * 0.015 * stagger,
            }}
          />
        ))}

        {notches
          .filter((n) => n.isActive)
          .map((notch) => (
            <motion.path
              animate={{ opacity: 1, scale: 1 }}
              d={createNotchPath(notch.points, notchCornerRadius, notchLength)}
              fill={resolveActiveFill(notch)}
              fillOpacity={resolvedActiveFillOpacity}
              initial={{ opacity: 0, scale: 0 }}
              key={`active-${notch.index}`}
              style={{
                transformOrigin: `${centerX}px ${centerY}px`,
              }}
              transition={{
                ...notchTransition,
                delay: (0.3 + notch.index * 0.02) * stagger,
              }}
            />
          ))}
      </svg>

      <div
        className="pointer-events-none absolute inset-0 flex flex-col items-center justify-center"
        style={{ paddingTop: size * 0.08 }}
      >
        <PieCenterShell
          centerValue={centerValue}
          contextSize={size}
          defaultLabel={defaultLabel}
          formatOptions={formatOptions}
          innerRadiusPx={Math.max(size * 0.2, 52)}
          prefix={prefix}
          suffix={suffix}
        />
      </div>
    </div>
  );
}

export function Gauge({
  width: widthProp,
  height: heightProp,
  className,
  minWidth = 300,
  ...props
}: GaugeProps) {
  if (widthProp != null && heightProp != null) {
    return (
      <div className={cn("relative inline-flex max-w-full", className)}>
        <GaugeInner height={heightProp} width={widthProp} {...props} />
      </div>
    );
  }

  return (
    <div
      className={cn("relative w-full max-w-full", className)}
      style={{ minWidth }}
    >
      <div className="mx-auto aspect-[21/16] w-full max-w-[560px]">
        <ParentSize debounceTime={10}>
          {({ width, height }) =>
            width > 0 && height > 0 ? (
              <GaugeInner height={height} width={width} {...props} />
            ) : null
          }
        </ParentSize>
      </div>
    </div>
  );
}

Gauge.displayName = "Gauge";
```

##### File: `src/charts/chart-center-typography.ts`

```tsx
/**
 * Fluid typography for pie / ring / gauge center labels.
 *
 * Uses CSS container query units (`cqw`) so values scale with the center
 * hole — not the viewport — which keeps stat text readable on small charts.
 */
export const chartCenterContainerClassName =
  "@container/chart-center size-full min-w-0";

/** Primary stat — ~22% of center width, clamped between text-sm and text-3xl. */
export const chartCenterValueClassName =
  "font-bold tabular-nums leading-none text-[clamp(0.75rem,22cqw,1.875rem)]";

/** Supporting label — ~9% of center width, clamped between 10px and text-xs. */
export const chartCenterLabelClassName =
  "max-w-full truncate leading-tight text-[clamp(0.625rem,9cqw,0.75rem)]";
```

---

### Line Chart <a name="line-chart"></a>

A composable line chart with tooltips, markers, and hover interactions

- **Category**: Charts
- **Registry Key**: `line-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/line-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `@visx/curve@4.0.1-alpha.0`
  - `@visx/shape@4.0.1-alpha.0`
  - `motion`
- **Local Registry Dependencies**:
  - `@bklit/chart-context`
  - `@bklit/chart-animation`
  - `@bklit/chart-series`
  - `@bklit/grid`
  - `@bklit/x-axis`
  - `@bklit/chart-tooltip`
  - `@bklit/utils`

#### How to Use

##### Example 1

```tsx
import { LineChart, Line, Grid, XAxis, ChartTooltip } from "@bklitui/ui/charts";

const data = [
  { date: new Date("2025-01-01"), users: 1200 },
  { date: new Date("2025-01-02"), users: 1350 },
  // ...
];

export default function SimpleChart() {
  return (
    <LineChart data={data}>
      <Grid horizontal />
      <Line dataKey="users" />
      <XAxis />
      <ChartTooltip />
    </LineChart>
  );
}
```

##### Example 2

```tsx
<Grid
  highlightRowValues={[0]}
  highlightRowStroke="var(--foreground)"
  highlightRowStrokeOpacity={0.35}
  horizontal
/>
```

##### Example 3

```tsx
<Line
  dataKey="visitors"
  dashFromIndex={5}
  dashArray="6,4"
  stroke="var(--chart-line-primary)"
/>
```

##### Example 4

```tsx
import { LineChart, Line, ChartTooltip, ChartMarkers, MarkerTooltipContent, useActiveMarkers, type ChartMarker } from "@bklitui/ui/charts";

const markers: ChartMarker[] = [
  {
    date: new Date("2025-01-05"),
    icon: "🚀",
    title: "v1.2.0 Released",
    description: "New chart animations",
  },
  {
    date: new Date("2025-01-05"), // Same day - will stack!
    icon: "🐛",
    title: "Bug Fix",
    description: "Fixed tooltip positioning",
  },
];

function MyChart({ data }) {
  return (
    <LineChart data={data}>
      <Line dataKey="users" />
      <ChartMarkers items={markers} />
      <ChartTooltip>
        <MarkerContent markers={markers} />
      </ChartTooltip>
    </LineChart>
  );
}

// Use the hook to get markers for the hovered date
function MarkerContent({ markers }) {
  const activeMarkers = useActiveMarkers(markers);
  if (activeMarkers.length === 0) return null;
  return <MarkerTooltipContent markers={activeMarkers} />;
}
```

##### Example 5

```tsx
import { LineChart, Line, Grid, XAxis, ChartTooltip, SegmentBackground, SegmentLineFrom, SegmentLineTo } from "@bklitui/ui/charts";

<LineChart data={data}>
  <Grid horizontal />
  <Line dataKey="users" />
  <SegmentBackground />
  <SegmentLineFrom />
  <SegmentLineTo />
  <XAxis />
  <ChartTooltip />
</LineChart>
```

##### Example 6

```tsx
import { useChart } from "@bklitui/ui/charts";

function SelectionStats({ onSelectionChange }) {
  const { selection, data, xAccessor } = useChart();

  useEffect(() => {
    if (!selection?.active) {
      onSelectionChange(null);
      return;
    }

    const startPoint = data[selection.startIndex];
    const endPoint = data[selection.endIndex];
    // Compute and report stats...
    onSelectionChange({ startPoint, endPoint });
  }, [selection, data, xAccessor, onSelectionChange]);

  return null;
}
```

#### Props & API Reference

##### `LineChart` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `data` | `Record<string, unknown>[]` | required | Array of data points |
| `xDataKey` | `string` | `"date"` | Key in data for x-axis values |
| `margin` | `Partial<Margin>` | `{ top: 40, right: 40, bottom: 40, left: 40 }` | Chart margins |
| `animationDuration` | `number` | `1100` | Animation duration in ms |
| `aspectRatio` | `string` | `"2 / 1"` | CSS aspect ratio |
| `className` | `string` | `""` | Additional CSS class |

##### `Line` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dataKey` | `string` | required | Key in data for y values |
| `stroke` | `string` | `var(--chart-line-primary)` | Line color |
| `strokeWidth` | `number` | `2.5` | Line width |
| `curve` | `CurveFactory` | `curveNatural` | D3 curve function |
| `animate` | `boolean` | `true` | Enable grow animation |
| `fadeEdges` | `boolean` | `true` | Fade line at edges |
| `showHighlight` | `boolean` | `true` | Show highlight on hover |
| `showMarkers` | `boolean` | `false` | Render scatter-style ring markers at each point |
| `markers` | `SeriesPointMarkerStyle` | — | Marker styling (same options as [`Scatter`](/docs/components/scatter-chart)) |

##### `Grid` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `horizontal` | `boolean` | `true` | Show horizontal lines |
| `vertical` | `boolean` | `false` | Show vertical lines |
| `numTicksRows` | `number` | `5` | Number of horizontal lines |
| `numTicksColumns` | `number` | `10` | Number of vertical lines |
| `stroke` | `string` | `var(--chart-grid)` | Line color |
| `strokeDasharray` | `string` | `"4,4"` | Dash pattern |
| `highlightRowValues` | `number[]` | — | Draw emphasized horizontal lines at specific y values (e.g. `[0]` for break-even) |
| `highlightRowStroke` | `string` | `var(--chart-foreground-muted)` | Stroke for highlighted rows |
| `highlightRowStrokeOpacity` | `number` | `1` | Opacity for highlighted rows |
| `highlightRowStrokeWidth` | `number` | `1` | Width for highlighted rows |
| `highlightRowStrokeDasharray` | `string` | `"0"` | Dash pattern for highlighted rows (`"0"` = solid) |

##### `XAxis` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `numTicks` | `number` | `6` | Number of tick labels |
| `tickerHalfWidth` | `number` | `50` | Fade radius for labels |
| `tickMode` | `"domain" \| "data"` | `"domain"` | `"domain"` for evenly spaced ticks; `"data"` for one label per row |

##### `ChartTooltip` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showDatePill` | `boolean` | `true` | Show animated date ticker |
| `showCrosshair` | `boolean` | `true` | Show vertical crosshair |
| `showDots` | `boolean` | `true` | Show dots on lines |
| `indicatorColor` | `string \| (point) => string` | — | Crosshair and dot color; use a function for value-based colors |
| `content` | `(props) => ReactNode` | - | Custom content renderer |
| `rows` | `(point) => TooltipRow[]` | - | Custom row generator |

##### `Dashed tail` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dashFromIndex` | `number` | — | Inclusive data index where the dashed tail begins |
| `dashArray` | `string` | `"6,4"` | SVG `stroke-dasharray` pattern for the tail segment |

##### `ChartMarkers Props` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `items` | `ChartMarker[]` | required | Array of markers |
| `size` | `number` | `28` | Marker circle size |
| `showLines` | `boolean` | `true` | Show vertical guide lines |
| `animate` | `boolean` | `true` | Animate markers on entrance |

##### `SegmentBackground` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `fill` | `string` | `var(--chart-segment-background)` | Fill color for the selected region |

##### `SegmentLineFrom / SegmentLineTo` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `stroke` | `string` | `var(--chart-segment-line)` | Line color |
| `strokeWidth` | `number` | `1` | Line width |
| `variant` | `"dashed" \| "solid" \| "gradient"` | `"dashed"` | Line style |

#### Component Source Code

##### File: `src/charts/line-chart.tsx`

```tsx
"use client";

import { ParentSize } from "@visx/responsive";
import type { Transition } from "motion/react";
import {
  Children,
  isValidElement,
  type ReactElement,
  type ReactNode,
  useMemo,
  useRef,
} from "react";
import { cn } from "@/lib/utils";
import type { LineConfig, Margin } from "./chart-context";
import { Line, type LineProps } from "./line";
import { TimeSeriesChartInner } from "./time-series-chart-shell";

export interface LineChartProps {
  /** Data array - each item should have a date field and numeric values */
  data: Record<string, unknown>[];
  /** Key in data for the x-axis (date). Default: "date" */
  xDataKey?: string;
  /** Chart margins */
  margin?: Partial<Margin>;
  /** Animation duration in milliseconds. Default: 1100 */
  animationDuration?: number;
  /** CSS easing for clip-reveal. Default: cubic-bezier(0.85, 0, 0.15, 1) */
  animationEasing?: string;
  enterTransition?: Transition;
  revealSignature?: string;
  /** Aspect ratio as "width / height". Default: "2 / 1" */
  aspectRatio?: string;
  /** Additional class name for the container */
  className?: string;
  /** Child components (Line, Grid, ChartTooltip, etc.) */
  children: ReactNode;
}

const DEFAULT_MARGIN: Margin = { top: 40, right: 40, bottom: 40, left: 40 };

/** Series renderers that carry a dataKey but must not drive the shared y-domain. */
const LINE_DOMAIN_EXCLUDED_NAMES = new Set([
  "ProfitLossLine",
  "Area",
  "SeriesBar",
  "Scatter",
  "Candlestick",
  "Bar",
  "PatternArea",
]);

function getChildComponentName(child: ReactElement) {
  const childType = child.type as { displayName?: string; name?: string };
  return typeof child.type === "function"
    ? childType.displayName || childType.name || ""
    : "";
}

function registersLineDomain(
  child: ReactElement,
  props: LineProps | undefined
) {
  if (!props?.dataKey) {
    return false;
  }

  const componentName = getChildComponentName(child);
  if (componentName === "Line" || child.type === Line) {
    return true;
  }
  if (LINE_DOMAIN_EXCLUDED_NAMES.has(componentName)) {
    return false;
  }
  // MDX / duplicate bundle instances may not share the same `Line` reference.
  return typeof props.dataKey === "string" && props.dataKey.length > 0;
}

function extractLineConfigs(children: ReactNode): LineConfig[] {
  const configs: LineConfig[] = [];

  const visit = (node: ReactNode) => {
    Children.forEach(node, (child) => {
      if (!isValidElement(child)) {
        return;
      }

      const props = child.props as LineProps | undefined;

      if (registersLineDomain(child, props) && props?.dataKey) {
        configs.push({
          dataKey: props.dataKey,
          stroke: props.stroke || "var(--chart-line-primary)",
          strokeWidth: props.strokeWidth || 2.5,
        });
        return;
      }

      const childProps = child.props as { children?: ReactNode } | undefined;
      if (childProps?.children) {
        visit(childProps.children);
      }
    });
  };

  visit(children);
  return configs;
}

interface ChartInnerProps {
  width: number;
  height: number;
  data: Record<string, unknown>[];
  xDataKey: string;
  margin: Margin;
  animationDuration: number;
  animationEasing?: string;
  enterTransition?: Transition;
  revealSignature?: string;
  children: ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
}

function ChartInner({
  width,
  height,
  data,
  xDataKey,
  margin,
  animationDuration,
  animationEasing,
  enterTransition,
  revealSignature,
  children,
  containerRef,
}: ChartInnerProps) {
  const lines = useMemo(() => extractLineConfigs(children), [children]);

  return (
    <TimeSeriesChartInner
      animationDuration={animationDuration}
      animationEasing={animationEasing}
      clipPathId="chart-grow-clip"
      containerRef={containerRef}
      data={data}
      enterTransition={enterTransition}
      height={height}
      lines={lines}
      margin={margin}
      revealSignature={revealSignature}
      width={width}
      xDataKey={xDataKey}
    >
      {children}
    </TimeSeriesChartInner>
  );
}

export function LineChart({
  data,
  xDataKey = "date",
  margin: marginProp,
  animationDuration = 1100,
  animationEasing,
  enterTransition,
  revealSignature,
  aspectRatio = "2 / 1",
  className = "",
  children,
}: LineChartProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const margin = { ...DEFAULT_MARGIN, ...marginProp };

  return (
    <div
      className={cn("relative w-full", className)}
      ref={containerRef}
      style={{ aspectRatio, touchAction: "none" }}
    >
      <ParentSize debounceTime={10}>
        {({ width, height }) => (
          <ChartInner
            animationDuration={animationDuration}
            animationEasing={animationEasing}
            containerRef={containerRef}
            data={data}
            enterTransition={enterTransition}
            height={height}
            margin={margin}
            revealSignature={revealSignature}
            width={width}
            xDataKey={xDataKey}
          >
            {children}
          </ChartInner>
        )}
      </ParentSize>
    </div>
  );
}

export { Line, type LineProps } from "./line";

export default LineChart;
```

##### File: `src/charts/time-series-chart-shell.tsx`

```tsx
"use client";

import { scaleLinear, scaleTime } from "@visx/scale";
import { bisector, extent } from "d3-array";
import type { Transition } from "motion/react";
import {
  Children,
  cloneElement,
  isValidElement,
  memo,
  type ReactElement,
  type ReactNode,
  useCallback,
  useEffect,
  useMemo,
  useState,
} from "react";
import { DEFAULT_ANIMATION_EASING } from "./animation";
import { ChartProvider, type LineConfig, type Margin } from "./chart-context";
import { isGradientDefComponent, isPatternDefComponent } from "./chart-defs";
import { shortDateFmt } from "./chart-formatters";
import { ChartRevealClip } from "./chart-reveal-clip";
import {
  decimateTimeSeries,
  maxRenderPointsForWidth,
} from "./decimate-time-series";
import {
  computeSeriesBarRevealClipPadding,
  computeSeriesBarWidth,
} from "./series-bar-layout";
import { useStaticChartPreview } from "./static-chart-preview-context";
import { useChartInteraction } from "./use-chart-interaction";

function collectNumericExtents(
  data: Record<string, unknown>[],
  dataKeys: string[]
) {
  let minValue = Number.POSITIVE_INFINITY;
  let maxValue = Number.NEGATIVE_INFINITY;

  for (const d of data) {
    for (const key of dataKeys) {
      const value = d[key];
      if (typeof value === "number") {
        if (value < minValue) {
          minValue = value;
        }
        if (value > maxValue) {
          maxValue = value;
        }
      }
    }
  }

  if (minValue === Number.POSITIVE_INFINITY) {
    return { minValue: 0, maxValue: 100 };
  }

  return { minValue, maxValue };
}

function resolveTimeSeriesYDomain(
  data: Record<string, unknown>[],
  dataKeys: string[],
  yScaleDomainMax: number | undefined
): [number, number] {
  if (yScaleDomainMax != null && yScaleDomainMax > 0) {
    return [0, yScaleDomainMax * 1.1];
  }

  const { minValue, maxValue } = collectNumericExtents(data, dataKeys);

  if (minValue >= 0) {
    const top = maxValue <= 0 ? 100 : maxValue * 1.1;
    return [0, top];
  }

  const padding = (maxValue - minValue) * 0.05 || 1;
  return [minValue - padding, maxValue + padding];
}

/** Markers render after the interaction overlay so they stay clickable. */
export function isPostOverlayComponent(child: ReactElement): boolean {
  const childType = child.type as {
    displayName?: string;
    name?: string;
    __isChartMarkers?: boolean;
  };

  if (childType.__isChartMarkers) {
    return true;
  }

  const componentName =
    typeof child.type === "function"
      ? childType.displayName || childType.name || ""
      : "";

  return componentName === "ChartMarkers" || componentName === "MarkerGroup";
}

function ensureChildKey(child: ReactElement, index: number): ReactElement {
  if (child.key != null) {
    return child;
  }
  return cloneElement(child, { key: `chart-child-${index}` });
}

export interface TimeSeriesChartInnerProps {
  width: number;
  height: number;
  data: Record<string, unknown>[];
  xDataKey: string;
  margin: Margin;
  animationDuration: number;
  animationEasing?: string;
  enterTransition?: Transition;
  /** Signature of motion URL state — triggers reveal replay when it changes. */
  revealSignature?: string;
  children: ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
  /** Series keys driving y-domain and tooltip (Line / Area / SeriesBar configs). */
  lines: LineConfig[];
  /** SVG clipPath id for grow animation. */
  clipPathId: string;
  /** Optional ComposedChart bar layout (forwarded into context). */
  composedBarDataKeys?: string[];
  composedBarSize?: number;
  composedMaxBarSize?: number;
  composedBarGap?: number;
  composedStacked?: boolean;
  composedStackOffsets?: Map<number, Map<string, number>>;
  composedStackGap?: number;
  /** When set, drives the y-axis max instead of scanning `lines` (e.g. stacked bar totals). */
  yScaleDomainMax?: number;
}

export function TimeSeriesChartInner(props: TimeSeriesChartInnerProps) {
  const { width, height } = props;
  if (width < 10 || height < 10) {
    return null;
  }
  return <TimeSeriesChartCore {...props} />;
}

const TimeSeriesChartCore = memo(function TimeSeriesChartCore({
  width,
  height,
  data,
  xDataKey,
  margin,
  animationDuration,
  animationEasing = DEFAULT_ANIMATION_EASING,
  enterTransition,
  revealSignature = "",
  children,
  containerRef,
  lines,
  clipPathId,
  composedBarDataKeys,
  composedBarSize,
  composedMaxBarSize,
  composedBarGap,
  composedStacked,
  composedStackOffsets,
  composedStackGap,
  yScaleDomainMax,
}: TimeSeriesChartInnerProps) {
  const staticPreview = useStaticChartPreview();
  const [isLoaded, setIsLoaded] = useState(staticPreview);
  const [revealEpoch, setRevealEpoch] = useState(0);

  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  const xAccessor = useCallback(
    (d: Record<string, unknown>): Date => {
      const value = d[xDataKey];
      return value instanceof Date ? value : new Date(value as string | number);
    },
    [xDataKey]
  );

  const bisectDate = useMemo(
    () => bisector<Record<string, unknown>, Date>((d) => xAccessor(d)).left,
    [xAccessor]
  );

  const xScale = useMemo(() => {
    const timeExtent = extent(data, (d) => xAccessor(d).getTime());
    const minTime = timeExtent[0] ?? 0;
    const maxTime = timeExtent[1] ?? minTime;

    return scaleTime({
      range: [0, innerWidth],
      domain: [minTime, maxTime],
    });
  }, [innerWidth, data, xAccessor]);

  const renderData = useMemo(() => {
    const valueKeys = lines.map((line) => line.dataKey);
    return decimateTimeSeries(
      data,
      maxRenderPointsForWidth(innerWidth),
      valueKeys
    );
  }, [data, innerWidth, lines]);

  const columnWidth = useMemo(() => {
    if (data.length < 2) {
      return 0;
    }
    return innerWidth / (data.length - 1);
  }, [innerWidth, data.length]);

  const yScale = useMemo(() => {
    const dataKeys = lines.map((line) => line.dataKey);
    const domain = resolveTimeSeriesYDomain(data, dataKeys, yScaleDomainMax);

    return scaleLinear({
      range: [innerHeight, 0],
      domain,
      nice: true,
    });
  }, [innerHeight, data, lines, yScaleDomainMax]);

  const dateLabels = useMemo(
    () => data.map((d) => shortDateFmt.format(xAccessor(d))),
    [data, xAccessor]
  );

  // biome-ignore lint/correctness/useExhaustiveDependencies: revealSignature
  useEffect(() => {
    if (staticPreview) {
      setIsLoaded(true);
      return;
    }

    setRevealEpoch((n) => n + 1);
    setIsLoaded(false);
    const timer = setTimeout(() => {
      setIsLoaded(true);
    }, animationDuration);
    return () => clearTimeout(timer);
  }, [animationDuration, revealSignature, staticPreview]);

  const canInteract = isLoaded;

  const {
    tooltipData,
    setTooltipData,
    selection,
    clearSelection,
    interactionHandlers,
    interactionStyle,
  } = useChartInteraction({
    xScale,
    yScale,
    data,
    lines,
    margin,
    xAccessor,
    bisectDate,
    canInteract,
  });

  const defsChildren: ReactElement[] = [];
  const preOverlayChildren: ReactElement[] = [];
  const postOverlayChildren: ReactElement[] = [];

  Children.forEach(children, (child, index) => {
    if (!isValidElement(child)) {
      return;
    }

    const keyedChild = ensureChildKey(child, index);

    if (isGradientDefComponent(keyedChild)) {
      defsChildren.push(keyedChild);
    } else if (isPatternDefComponent(keyedChild)) {
      // Keep pattern defs in the plot <g> (same as main) — hoisting breaks url(#id) fills.
      preOverlayChildren.push(keyedChild);
    } else if (isPostOverlayComponent(keyedChild)) {
      postOverlayChildren.push(keyedChild);
    } else {
      preOverlayChildren.push(keyedChild);
    }
  });

  const contextValue = useMemo(
    () => ({
      data,
      renderData,
      xScale,
      yScale,
      width,
      height,
      innerWidth,
      innerHeight,
      margin,
      columnWidth,
      tooltipData,
      setTooltipData,
      containerRef,
      lines,
      isLoaded,
      animationDuration,
      animationEasing,
      enterTransition,
      revealEpoch,
      xAccessor,
      dateLabels,
      selection,
      clearSelection,
      composedBarDataKeys,
      composedBarSize,
      composedMaxBarSize,
      composedBarGap,
      composedStacked,
      composedStackOffsets,
      composedStackGap,
    }),
    [
      data,
      renderData,
      xScale,
      yScale,
      width,
      height,
      innerWidth,
      innerHeight,
      margin,
      columnWidth,
      tooltipData,
      setTooltipData,
      containerRef,
      lines,
      isLoaded,
      animationDuration,
      animationEasing,
      enterTransition,
      revealEpoch,
      xAccessor,
      dateLabels,
      selection,
      clearSelection,
      composedBarDataKeys,
      composedBarSize,
      composedMaxBarSize,
      composedBarGap,
      composedStacked,
      composedStackOffsets,
      composedStackGap,
    ]
  );

  // Single shared reveal clip for every series. Replaces the per-<Line> /
  // per-<Area> `<ChartRevealClip>` motion.rects: one motion-driven attribute
  // animation instead of N, with all series referencing the same `<clipPath>`.
  // The wipe semantics (left-to-right unveil of static path geometry) are
  // identical to the previous per-series clips.
  // animationDuration === 0 truly disables the reveal (no clipPath wrapper),
  // so consumers can opt out without having to also pass enterTransition.
  const showReveal =
    !staticPreview &&
    renderData.length > 1 &&
    innerWidth > 0 &&
    animationDuration > 0;
  // If the consumer didn't pass an explicit enterTransition, derive one from
  // animationDuration so clipRevealTransition picks up the override instead
  // of falling back to its 1100ms default.
  const effectiveEnterTransition: Transition = enterTransition ?? {
    type: "tween",
    duration: animationDuration / 1000,
  };

  const revealClipPadding = useMemo(() => {
    if (!composedBarDataKeys?.length) {
      return 0;
    }
    const barWidth = computeSeriesBarWidth({
      innerWidth,
      dataLength: data.length,
      columnWidth,
      seriesCount: composedBarDataKeys.length,
      composedBarSize,
      composedMaxBarSize,
      composedBarGap,
      stacked: composedStacked,
    });
    return computeSeriesBarRevealClipPadding({
      barWidth,
      seriesCount: composedBarDataKeys.length,
      gap: composedBarGap,
      stacked: composedStacked,
    });
  }, [
    columnWidth,
    composedBarDataKeys,
    composedBarGap,
    composedBarSize,
    composedMaxBarSize,
    composedStacked,
    data.length,
    innerWidth,
  ]);

  return (
    <ChartProvider value={contextValue}>
      <svg aria-hidden="true" height={height} width={width}>
        <defs>
          {defsChildren}
          {showReveal ? (
            <ChartRevealClip
              clipPathId={clipPathId}
              enterTransition={effectiveEnterTransition}
              height={innerHeight + 20}
              padding={revealClipPadding}
              revealEpoch={revealEpoch}
              targetWidth={innerWidth}
            />
          ) : null}
        </defs>

        <rect fill="transparent" height={height} width={width} x={0} y={0} />

        <g
          {...interactionHandlers}
          style={interactionStyle}
          transform={`translate(${margin.left},${margin.top})`}
        >
          <rect
            fill="transparent"
            height={innerHeight}
            width={innerWidth}
            x={0}
            y={0}
          />

          {showReveal ? (
            <g clipPath={`url(#${clipPathId})`}>{preOverlayChildren}</g>
          ) : (
            preOverlayChildren
          )}
          {postOverlayChildren}
        </g>
      </svg>
    </ChartProvider>
  );
});
```

##### File: `src/charts/series-bar-layout.ts`

```tsx
export function computeSeriesBarWidth(input: {
  innerWidth: number;
  dataLength: number;
  columnWidth: number;
  seriesCount: number;
  composedBarSize?: number;
  composedMaxBarSize?: number;
  composedBarGap?: number;
  stacked?: boolean;
}): number {
  const {
    innerWidth,
    dataLength,
    columnWidth,
    seriesCount,
    composedBarSize,
    composedMaxBarSize,
    composedBarGap = 4,
    stacked = false,
  } = input;

  const gap = composedBarGap;
  const groupCount = stacked ? 1 : Math.max(1, seriesCount);
  let slot = columnWidth;
  if (slot <= 0) {
    slot = dataLength < 2 ? innerWidth : innerWidth / (dataLength - 1);
  }

  let width =
    composedBarSize ??
    Math.min(slot * 0.88, composedMaxBarSize ?? Number.POSITIVE_INFINITY);
  if (composedMaxBarSize != null) {
    width = Math.min(width, composedMaxBarSize);
  }
  if (groupCount > 1) {
    const maxGroup = slot * 0.92;
    const needed = groupCount * width + (groupCount - 1) * gap;
    if (needed > maxGroup && maxGroup > 0) {
      width = Math.max(4, (maxGroup - (groupCount - 1) * gap) / groupCount);
    }
  }

  return Math.max(2, width);
}

/** Half-width of the bar group at each x — used to pad reveal clips. */
export function computeSeriesBarRevealClipPadding(input: {
  barWidth: number;
  seriesCount: number;
  gap?: number;
  stacked?: boolean;
}): number {
  const { barWidth, seriesCount, gap = 4, stacked = false } = input;

  if (stacked || seriesCount <= 1) {
    return Math.ceil(barWidth / 2);
  }

  const groupWidth = seriesCount * barWidth + (seriesCount - 1) * gap;
  return Math.ceil(groupWidth / 2);
}
```

##### File: `src/charts/line.tsx`

```tsx
"use client";

import { curveNatural } from "@visx/curve";
import { LinePath } from "@visx/shape";

// CurveFactory type - simplified version compatible with visx
// biome-ignore lint/suspicious/noExplicitAny: d3 curve factory type
type CurveFactory = any;

import { useCallback, useId, useRef } from "react";
import { chartCssVars, useChartStable } from "./chart-context";
import {
  type FadeEdges,
  fadeGradientStops,
  resolveFadeSides,
} from "./fade-edges";
import {
  resolveDashTailBounds,
  usePathStrokeMetrics,
} from "./path-stroke-utils";
import { SeriesDashTailOverlay } from "./series-dash-tail-overlay";
import { SeriesHighlightLayer } from "./series-highlight-layer";
import { SeriesHoverDim } from "./series-hover-dim";
import { SeriesMarkers } from "./series-markers";
import type { SeriesPointMarkerStyle } from "./series-point-marker";

export interface LineProps {
  /** Key in data to use for y values */
  dataKey: string;
  /** Stroke color. Default: var(--chart-line-primary) */
  stroke?: string;
  /** Stroke width. Default: 2.5 */
  strokeWidth?: number;
  /** Curve function. Default: curveNatural */
  curve?: CurveFactory;
  /** Whether to animate the line. Default: true */
  animate?: boolean;
  /**
   * Fade the line stroke toward transparent at the chart edges.
   * - `true` fades both edges, `false` disables the fade entirely.
   * - `"left"` / `"right"` fades only that side.
   * Default: true
   */
  fadeEdges?: FadeEdges;
  /** Whether to show highlight segment on hover. Default: true */
  showHighlight?: boolean;
  /** Render scatter-style circle markers at each data point. Default: false */
  showMarkers?: boolean;
  /** Marker styling (same options as Scatter). */
  markers?: SeriesPointMarkerStyle;
  /**
   * Data index from which the line stroke becomes dashed (inclusive).
   * Useful for projecting incomplete periods, e.g. dashed from yesterday through today.
   */
  dashFromIndex?: number;
  /** Dash pattern for the tail segment when `dashFromIndex` is set. Default: "6,4" */
  dashArray?: string;
}

export function Line({
  dataKey,
  stroke = chartCssVars.linePrimary,
  strokeWidth = 2.5,
  curve = curveNatural,
  animate = true,
  fadeEdges = true,
  showHighlight = true,
  showMarkers = false,
  markers,
  dashFromIndex,
  dashArray = "6,4",
}: LineProps) {
  // Stable slice only: hover state lives inside `<SeriesHoverDim>` and
  // `<SeriesHighlightLayer>` so this component (and its expensive
  // <SeriesDashTailOverlay> child) does not re-render on cursor motion.
  // The reveal-clip is now a single shared clipPath at the chart-shell
  // level (`time-series-chart-shell.tsx`); we no longer render a per-line
  // `<ChartRevealClip>` or read `revealEpoch` here.
  const {
    data,
    renderData,
    xScale,
    yScale,
    innerHeight,
    innerWidth,
    xAccessor,
  } = useChartStable();

  const pathRef = useRef<SVGPathElement>(null);
  const { pathLength, pathD } = usePathStrokeMetrics(pathRef, [
    renderData,
    innerWidth,
    dashFromIndex,
    animate,
  ]);

  const reactId = useId();
  const gradientId = `line-gradient-${dataKey}-${reactId}`;

  const getY = useCallback(
    (d: Record<string, unknown>) => {
      const value = d[dataKey];
      return typeof value === "number" ? (yScale(value) ?? 0) : 0;
    },
    [dataKey, yScale]
  );

  const hasDashTail = resolveDashTailBounds(dashFromIndex, data.length);
  const fadeSides = resolveFadeSides(fadeEdges);
  const lineStroke = fadeSides.any ? `url(#${gradientId})` : stroke;
  const fadeStops = fadeSides.any ? fadeGradientStops(fadeSides) : null;

  return (
    <>
      {fadeStops ? (
        <defs>
          <linearGradient id={gradientId} x1="0%" x2="100%" y1="0%" y2="0%">
            {fadeStops.map((stop) => (
              <stop
                key={stop.offset}
                offset={stop.offset}
                style={{ stopColor: stroke, stopOpacity: stop.opacity }}
              />
            ))}
          </linearGradient>
        </defs>
      ) : null}

      <SeriesHoverDim dimOpacity={0.3} enabled={showHighlight}>
        <LinePath
          curve={curve}
          data={renderData}
          innerRef={pathRef}
          stroke={hasDashTail ? "transparent" : lineStroke}
          strokeLinecap="round"
          strokeWidth={strokeWidth}
          x={(d) => xScale(xAccessor(d)) ?? 0}
          y={getY}
        />

        <SeriesDashTailOverlay
          dashArray={dashArray}
          dashFromIndex={dashFromIndex}
          data={data}
          innerHeight={innerHeight}
          innerWidth={innerWidth}
          pathD={pathD}
          pathLength={pathLength}
          stroke={lineStroke}
          strokeWidth={strokeWidth}
          xAccessor={xAccessor}
          xScale={xScale}
        />
      </SeriesHoverDim>

      {showMarkers ? (
        <SeriesMarkers
          animate={animate}
          dataKey={dataKey}
          {...markers}
          fill={markers?.fill ?? stroke}
          stroke={markers?.stroke ?? markers?.fill ?? stroke}
        />
      ) : null}

      <SeriesHighlightLayer
        enabled={showHighlight}
        height={innerHeight}
        pathRef={pathRef}
        stroke={stroke}
        strokeWidth={strokeWidth}
      />
    </>
  );
}

Line.displayName = "Line";

export default Line;
```

---

### Live Line Chart <a name="live-line-chart"></a>

Real-time streaming line chart with smooth scrolling, crosshair, and animated axes

- **Category**: Charts
- **Registry Key**: `live-line-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/live-line-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `@visx/curve@4.0.1-alpha.0`
  - `@visx/scale@4.0.1-alpha.0`
  - `@visx/shape@4.0.1-alpha.0`
  - `@visx/responsive@4.0.1-alpha.0`
  - `@visx/event@4.0.1-alpha.0`
  - `d3-array`
  - `motion`
- **Local Registry Dependencies**:
  - `@bklit/chart-context`
  - `@bklit/chart-tooltip`
  - `@bklit/utils`

#### How to Use

##### Example 1

```tsx
import {
  LiveLineChart,
  LiveLine,
  ChartTooltip,
  LiveXAxis,
  LiveYAxis,
} from "@bklitui/ui/charts";

const [data, setData] = useState([]);
const [value, setValue] = useState(100);

// Append new points (e.g. from WebSocket or polling)
useEffect(() => {
  const id = setInterval(() => {
    const point = { time: Date.now() / 1000, value: fetchLatest() };
    setData((prev) => [...prev.slice(-500), point]);
    setValue(point.value);
  }, 1000);
  return () => clearInterval(id);
}, []);

<LiveLineChart data={data} value={value} window={30}>
  <LiveLine dataKey="value" stroke="var(--chart-line-primary)" formatValue={(v) => `$${v.toFixed(2)}`} />
  <ChartTooltip showDatePill={false} content={MyTooltipContent} />
  <LiveXAxis />
  <LiveYAxis position="left" formatValue={(v) => `$${v.toFixed(2)}`} />
</LiveLineChart>
```

##### Example 2

```tsx
<LiveLineChart data={data} value={value} window={20} nowOffsetUnits={1}>
  <LiveLine dataKey="value" />
  {/* ... */}
</LiveLineChart>
```

##### Example 3

```tsx
const [paused, setPaused] = useState(false);

<LiveLineChart data={data} value={value} paused={paused}>
  {/* ... */}
</LiveLineChart>
```

##### Example 4

```tsx
const momentumColors = {
  up: "var(--color-emerald-500)",
  down: "var(--color-red-500)",
  flat: "var(--color-zinc-400)",
};

<LiveLine
  dataKey="value"
  momentumColors={momentumColors}
  formatValue={(v) => `$${v.toFixed(2)}`}
/>
```

#### Props & API Reference

##### `Live Line Props` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dataKey` | `string` | required | Key in data for y values |
| `stroke` | `string` | `var(--chart-line-primary)` | Line color (ignored if `momentumColors` set) |
| `strokeWidth` | `number` | `2` | Line width |
| `fill` | `boolean` | `true` | Show gradient fill under curve |
| `pulse` | `boolean` | `true` | Show pulsing live dot |
| `dotSize` | `number` | `4` | Radius of the live dot |
| `badge` | `boolean` | `true` | Show value badge at live tip |
| `formatValue` | `(v: number) => string` | - | Formatter for badge (and optional tooltip) |
| `momentumColors` | `MomentumColors` | - | `{ up, down, flat }` colors by trend |

##### `LiveLineChart Props` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `data` | `LiveLinePoint[]` | required | Streaming points `{ time, value }` |
| `value` | `number` | required | Latest value (for smooth interpolation) |
| `dataKey` | `string` | `"value"` | Key for value field in context |
| `window` | `number` | `30` | Visible time window (seconds) |
| `numXTicks` | `number` | `5` | Number of X-axis ticks |
| `nowOffsetUnits` | `number` | `0` | Leading offset in X-tick units |
| `exaggerate` | `boolean` | `false` | Tighter Y-axis range |
| `lerpSpeed` | `number` | `0.08` | Y-range interpolation speed (0–1) |
| `margin` | `Partial<Margin>` | - | Chart margins |
| `paused` | `boolean` | `false` | Freeze chart scrolling |

#### Component Source Code

##### File: `src/charts/live-line-chart.tsx`

```tsx
"use client";

import { localPoint } from "@visx/event";
import { ParentSize } from "@visx/responsive";
import { scaleLinear, scaleTime } from "@visx/scale";
import { bisector } from "d3-array";
import {
  Children,
  isValidElement,
  memo,
  type ReactNode,
  startTransition,
  useCallback,
  useEffect,
  useMemo,
  useRef,
  useState,
} from "react";
import { cn } from "@/lib/utils";
import {
  ChartProvider,
  type LineConfig,
  type Margin,
  type TooltipData,
} from "./chart-context";
import { hmsTimeFmt } from "./chart-formatters";
import type { LiveLineProps } from "./live-line";

// ---------------------------------------------------------------------------
// Types
// ---------------------------------------------------------------------------

export interface LiveLinePoint {
  time: number;
  value: number;
}

export interface LiveLineChartProps {
  /** Streaming data — array of { time: unixSeconds, value } */
  data: LiveLinePoint[];
  /** Latest value (smoothly interpolated to) */
  value: number;
  /** Key used for the value field in context data. Default: "value" */
  dataKey?: string;
  /** Visible time window in seconds. Default: 30 */
  window?: number;
  /** Number of X-axis ticks (used to compute leading offset). Default: 5 */
  numXTicks?: number;
  /** Leading offset in X-tick units (0 = now at right edge). Default: 0 */
  nowOffsetUnits?: number;
  /** Tight Y-axis. Default: false */
  exaggerate?: boolean;
  /** Interpolation speed (0–1). Default: 0.08 */
  lerpSpeed?: number;
  /** Chart margins */
  margin?: Partial<Margin>;
  /** Freeze chart scrolling. Default: false */
  paused?: boolean;
  /** Child components (LiveLine, Grid, ChartTooltip, LiveXAxis, LiveYAxis, etc.) */
  children: ReactNode;
  className?: string;
  style?: React.CSSProperties;
}

// ---------------------------------------------------------------------------
// Helpers
// ---------------------------------------------------------------------------

const LERP_SPEED = 0.08;
const DEFAULT_MARGIN: Margin = { top: 24, right: 16, bottom: 32, left: 16 };
/** React commit interval for the live animation loop (~30fps). */
const LIVE_FRAME_COMMIT_MS = 32;

interface AnimFrame {
  now: number;
  yMin: number;
  yMax: number;
  displayValue: number;
}

function computeTargetRange(
  data: LiveLinePoint[],
  value: number,
  exaggerate: boolean
) {
  if (data.length === 0) {
    return { yMin: 0, yMax: 100 };
  }
  let min = Number.POSITIVE_INFINITY;
  let max = Number.NEGATIVE_INFINITY;
  for (const d of data) {
    if (d.value < min) {
      min = d.value;
    }
    if (d.value > max) {
      max = d.value;
    }
  }
  if (value < min) {
    min = value;
  }
  if (value > max) {
    max = value;
  }
  const rawRange = max - min;
  const paddingFactor = exaggerate ? 0.03 : 0.15;
  const rangePad = rawRange * paddingFactor || (exaggerate ? 0.04 : 10);
  return { yMin: min - rangePad, yMax: max + rangePad };
}

function nextAnimFrame(
  prev: AnimFrame,
  targetRange: { yMin: number; yMax: number },
  targetValue: number,
  speed: number,
  isPaused: boolean
): AnimFrame {
  const nextNow = isPaused ? prev.now : Date.now();
  const nextYMin =
    targetRange.yMin < prev.yMin
      ? targetRange.yMin
      : prev.yMin + (targetRange.yMin - prev.yMin) * speed;
  const nextYMax =
    targetRange.yMax > prev.yMax
      ? targetRange.yMax
      : prev.yMax + (targetRange.yMax - prev.yMax) * speed;
  const nextValue =
    prev.displayValue + (targetValue - prev.displayValue) * speed;
  return {
    now: nextNow,
    yMin: nextYMin,
    yMax: nextYMax,
    displayValue: nextValue,
  };
}

function interpolateAtTime(
  points: LiveLinePoint[],
  timeSec: number
): number | null {
  if (points.length === 0) {
    return null;
  }
  const firstPt = points[0] as LiveLinePoint;
  const lastPt = points.at(-1) as LiveLinePoint;
  if (timeSec <= firstPt.time) {
    return firstPt.value;
  }
  if (timeSec >= lastPt.time) {
    return lastPt.value;
  }
  let lo = 0;
  let hi = points.length - 1;
  while (hi - lo > 1) {
    const mid = Math.floor((lo + hi) / 2);
    const midPt = points[mid];
    if (midPt && midPt.time <= timeSec) {
      lo = mid;
    } else {
      hi = mid;
    }
  }
  const p1 = points[lo];
  if (!p1) {
    return null;
  }
  const p2 = points[hi];
  if (!p2) {
    return null;
  }
  const dt = p2.time - p1.time;
  if (dt === 0) {
    return p1.value;
  }
  const t = (timeSec - p1.time) / dt;
  return p1.value + (p2.value - p1.value) * t;
}

const bisectTime = bisector<LiveLinePoint, number>((d) => d.time).left;

function extractLiveLineConfigs(children: ReactNode): LineConfig[] {
  const configs: LineConfig[] = [];
  Children.forEach(children, (child) => {
    if (!isValidElement(child)) {
      return;
    }
    const childType = child.type as { displayName?: string; name?: string };
    const name =
      typeof child.type === "function"
        ? childType.displayName || childType.name || ""
        : "";
    const props = child.props as LiveLineProps | undefined;
    if (
      (name === "LiveLine" || (props && "dataKey" in props)) &&
      props?.dataKey
    ) {
      configs.push({
        dataKey: props.dataKey,
        stroke: props.stroke || "var(--chart-line-primary)",
        strokeWidth: props.strokeWidth || 2,
      });
    }
  });
  return configs;
}

// ---------------------------------------------------------------------------
// Inner chart
// ---------------------------------------------------------------------------

function liveTooltipKey(
  tooltip: TooltipData | null,
  dataKey: string
): string | null {
  if (!tooltip) {
    return null;
  }
  return `${Math.round(tooltip.x)}:${Math.round(tooltip.yPositions[dataKey] ?? 0)}`;
}

function resolveLiveTooltip(
  cursorX: number | null,
  innerWidth: number,
  innerHeight: number,
  frame: AnimFrame,
  leadingMs: number,
  windowMs: number,
  xTickUnitMs: number,
  data: LiveLinePoint[],
  dataKey: string
): TooltipData | null {
  if (cursorX === null || innerWidth <= 0 || innerHeight <= 0) {
    return null;
  }

  const domainEndMs = frame.now + leadingMs;
  const xScaleNext = scaleTime({
    domain: [new Date(domainEndMs - windowMs), new Date(domainEndMs)],
    range: [0, innerWidth],
  });
  const yScaleNext = scaleLinear({
    domain: [frame.yMin, frame.yMax],
    range: [innerHeight, 0],
    nice: true,
  });
  const timeMs = xScaleNext.invert(cursorX).getTime();
  const timeSec = timeMs / 1000;
  const visible = data.filter((p) => p.time >= (domainEndMs - windowMs) / 1000);
  visible.push({ time: frame.now / 1000, value: frame.displayValue });
  visible.push({
    time: (frame.now + xTickUnitMs) / 1000,
    value: frame.displayValue,
  });
  const val = interpolateAtTime(visible, timeSec);
  if (val === null) {
    return null;
  }

  return {
    point: { date: new Date(timeMs), [dataKey]: val },
    index: 0,
    x: cursorX,
    yPositions: { [dataKey]: yScaleNext(val) ?? 0 },
  };
}

function shouldCommitLiveUpdates(
  now: number,
  lastFrameCommit: number,
  tooltipKey: string | null,
  lastTooltipKey: string | null
): { commitFrame: boolean; commitTooltip: boolean } {
  const commitFrame = now - lastFrameCommit >= LIVE_FRAME_COMMIT_MS;
  const commitTooltip = tooltipKey !== lastTooltipKey;
  return { commitFrame, commitTooltip };
}

interface InnerProps {
  data: LiveLinePoint[];
  value: number;
  dataKey: string;
  windowSecs: number;
  numXTicks: number;
  nowOffsetUnits: number;
  exaggerate: boolean;
  lerpSpeed: number;
  margin: Margin;
  paused: boolean;
  width: number;
  height: number;
  containerRef: React.RefObject<HTMLDivElement | null>;
  children: ReactNode;
}

function LiveLineChartInner(props: InnerProps) {
  const { width, height, margin } = props;
  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  if (innerWidth <= 0 || innerHeight <= 0) {
    return null;
  }

  return <LiveLineChartCore {...props} />;
}

const LiveLineChartCore = memo(function LiveLineChartCore({
  data,
  value,
  dataKey,
  windowSecs,
  numXTicks,
  nowOffsetUnits,
  exaggerate,
  lerpSpeed,
  margin,
  paused,
  width,
  height,
  containerRef,
  children,
}: InnerProps) {
  const windowMs = windowSecs * 1000;
  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  // ---- Animation state ----
  const animRef = useRef<AnimFrame>({
    now: Date.now(),
    yMin: 0,
    yMax: 100,
    displayValue: value,
  });
  const [frame, setFrame] = useState<AnimFrame>({
    now: Date.now(),
    yMin: 0,
    yMax: 100,
    displayValue: value,
  });

  const pausedRef = useRef(paused);
  const dataRef = useRef(data);
  const dataKeyRef = useRef(dataKey);
  dataRef.current = data;
  dataKeyRef.current = dataKey;

  useEffect(() => {
    pausedRef.current = paused;
  }, [paused]);

  const targetRange = useMemo(
    () => computeTargetRange(data, value, exaggerate),
    [data, value, exaggerate]
  );

  const lines = useMemo(() => extractLiveLineConfigs(children), [children]);

  // Leading offset (used in rAF for tooltip)
  const xTickUnitMs = windowMs / (numXTicks - 1);
  const leadingMs = nowOffsetUnits * xTickUnitMs;

  // ---- rAF loop: update frame and tooltip in one place to avoid effect→setState loops ----
  const cursorXRef = useRef<number | null>(null);
  const [tooltipData, setTooltipData] = useState<TooltipData | null>(null);
  const lastFrameCommitRef = useRef(0);
  const lastTooltipKeyRef = useRef<string | null>(null);

  useEffect(() => {
    let raf: number;
    const tick = () => {
      const next = nextAnimFrame(
        animRef.current,
        targetRange,
        value,
        lerpSpeed,
        pausedRef.current
      );
      animRef.current = next;

      const nextTooltip = resolveLiveTooltip(
        cursorXRef.current,
        innerWidth,
        innerHeight,
        next,
        leadingMs,
        windowMs,
        xTickUnitMs,
        dataRef.current,
        dataKeyRef.current
      );
      const now = performance.now();
      const tooltipKey = liveTooltipKey(nextTooltip, dataKeyRef.current);
      const { commitFrame, commitTooltip } = shouldCommitLiveUpdates(
        now,
        lastFrameCommitRef.current,
        tooltipKey,
        lastTooltipKeyRef.current
      );

      if (!(commitFrame || commitTooltip)) {
        raf = requestAnimationFrame(tick);
        return;
      }

      if (commitFrame) {
        lastFrameCommitRef.current = now;
      }
      if (commitTooltip) {
        lastTooltipKeyRef.current = tooltipKey;
      }

      startTransition(() => {
        if (commitFrame) {
          setFrame(next);
        }
        if (commitTooltip) {
          setTooltipData(nextTooltip);
        }
      });

      raf = requestAnimationFrame(tick);
    };
    raf = requestAnimationFrame(tick);
    return () => cancelAnimationFrame(raf);
  }, [
    targetRange,
    value,
    lerpSpeed,
    leadingMs,
    windowMs,
    xTickUnitMs,
    innerWidth,
    innerHeight,
  ]);

  const domainEndMs = frame.now + leadingMs;

  // ---- Scales ----
  const xScale = useMemo(
    () =>
      scaleTime({
        domain: [new Date(domainEndMs - windowMs), new Date(domainEndMs)],
        range: [0, innerWidth],
      }),
    [domainEndMs, windowMs, innerWidth]
  );

  const yScale = useMemo(
    () =>
      scaleLinear({
        domain: [frame.yMin, frame.yMax],
        range: [innerHeight, 0],
        nice: true,
      }),
    [frame.yMin, frame.yMax, innerHeight]
  );

  // ---- Build context-compatible data ----
  // Convert LiveLinePoint[] to Record<string, unknown>[] with 2 virtual points:
  // 1. At "now" — the live tip where the dot sits
  // 2. At "now + 1 unit" — a queued point that the line fades into
  const contextData = useMemo(() => {
    const windowStart = domainEndMs - windowMs;
    let startIdx = bisectTime(data, windowStart / 1000, 0);
    if (startIdx > 0) {
      startIdx--;
    }
    const sliced = data.slice(startIdx);
    const records: Record<string, unknown>[] = sliced.map((p) => ({
      date: new Date(p.time * 1000),
      [dataKey]: p.value,
    }));
    // Virtual point 1: the "now" position (where the live dot sits)
    records.push({
      date: new Date(frame.now),
      [dataKey]: frame.displayValue,
    });
    // Virtual point 2: queued ahead (the line extends and fades into this)
    records.push({
      date: new Date(frame.now + xTickUnitMs),
      [dataKey]: frame.displayValue,
    });
    return records;
  }, [
    data,
    frame.now,
    frame.displayValue,
    domainEndMs,
    windowMs,
    dataKey,
    xTickUnitMs,
  ]);

  // ---- X accessor ----
  const xAccessor = useCallback(
    (d: Record<string, unknown>): Date =>
      d.date instanceof Date ? d.date : new Date(d.date as number),
    []
  );

  const handleMouseMove = useCallback(
    (event: React.MouseEvent<SVGGElement>) => {
      const coords = localPoint(event);
      if (!coords) {
        return;
      }
      const x = coords.x - margin.left;
      cursorXRef.current = x >= 0 && x <= innerWidth ? x : null;
    },
    [margin.left, innerWidth]
  );

  const handleMouseLeave = useCallback(() => {
    cursorXRef.current = null;
    lastTooltipKeyRef.current = null;
    setTooltipData(null);
  }, []);

  // Date labels (for ChartTooltip's DateTicker — not used in live but needed for context)
  const dateLabels = useMemo(
    () => contextData.map((d) => hmsTimeFmt.format(xAccessor(d))),
    [contextData, xAccessor]
  );

  const columnWidth = useMemo(() => {
    if (contextData.length < 2) {
      return 0;
    }
    return innerWidth / (contextData.length - 1);
  }, [innerWidth, contextData.length]);

  const contextValue = useMemo(
    () => ({
      data: contextData,
      renderData: contextData,
      xScale,
      yScale,
      width,
      height,
      innerWidth,
      innerHeight,
      margin,
      columnWidth,
      tooltipData,
      setTooltipData,
      containerRef,
      lines,
      isLoaded: true,
      animationDuration: 0,
      xAccessor,
      dateLabels,
    }),
    [
      contextData,
      xScale,
      yScale,
      width,
      height,
      innerWidth,
      innerHeight,
      margin,
      columnWidth,
      tooltipData,
      containerRef,
      lines,
      xAccessor,
      dateLabels,
    ]
  );

  return (
    <ChartProvider value={contextValue}>
      <svg
        aria-hidden="true"
        className="overflow-visible"
        height={height}
        width={width}
      >
        {/* biome-ignore lint/a11y/noStaticElementInteractions: SVG group for mouse tracking */}
        <g
          onMouseLeave={handleMouseLeave}
          onMouseMove={handleMouseMove}
          style={{ cursor: "crosshair" }}
          transform={`translate(${margin.left},${margin.top})`}
        >
          <rect
            fill="transparent"
            height={innerHeight}
            width={innerWidth}
            x={0}
            y={0}
          />
          {children}
        </g>
      </svg>
    </ChartProvider>
  );
});

// ---------------------------------------------------------------------------
// Public component
// ---------------------------------------------------------------------------

export function LiveLineChart({
  data,
  value,
  dataKey = "value",
  window: windowSecs = 30,
  numXTicks = 5,
  nowOffsetUnits = 0,
  exaggerate = false,
  lerpSpeed = LERP_SPEED,
  margin: marginProp,
  paused = false,
  children,
  className,
  style,
}: LiveLineChartProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const margin = { ...DEFAULT_MARGIN, ...marginProp };

  return (
    <div
      className={cn("relative w-full", className)}
      ref={containerRef}
      style={{ height: 300, touchAction: "none", ...style }}
    >
      <ParentSize debounceTime={10}>
        {({ width, height }) => (
          <LiveLineChartInner
            containerRef={containerRef}
            data={data}
            dataKey={dataKey}
            exaggerate={exaggerate}
            height={height}
            lerpSpeed={lerpSpeed}
            margin={margin}
            nowOffsetUnits={nowOffsetUnits}
            numXTicks={numXTicks}
            paused={paused}
            value={value}
            width={width}
            windowSecs={windowSecs}
          >
            {children}
          </LiveLineChartInner>
        )}
      </ParentSize>
    </div>
  );
}

export default LiveLineChart;
```

##### File: `src/charts/live-line.tsx`

```tsx
"use client";

import { curveMonotoneX } from "@visx/curve";

// biome-ignore lint/suspicious/noExplicitAny: d3 curve factory type
type CurveFactory = any;

import { AreaClosed, LinePath } from "@visx/shape";
import { motion } from "motion/react";
import { useCallback, useId, useMemo } from "react";
import { chartCssVars, useChart } from "./chart-context";

export type Momentum = "up" | "down" | "flat";

export interface MomentumColors {
  up: string;
  down: string;
  flat: string;
}

export function detectMomentum(
  data: Record<string, unknown>[],
  dataKey: string,
  lookback = 20
): Momentum {
  if (data.length < 5) {
    return "flat";
  }
  const start = Math.max(0, data.length - lookback);
  let min = Number.POSITIVE_INFINITY;
  let max = Number.NEGATIVE_INFINITY;
  for (let i = start; i < data.length; i++) {
    const v = data[i]?.[dataKey];
    if (typeof v === "number") {
      if (v < min) {
        min = v;
      }
      if (v > max) {
        max = v;
      }
    }
  }
  const range = max - min;
  if (range === 0) {
    return "flat";
  }
  const tailStart = Math.max(start, data.length - 5);
  const first = (data[tailStart]?.[dataKey] as number) ?? 0;
  const last = (data.at(-1)?.[dataKey] as number) ?? 0;
  const delta = last - first;
  const threshold = range * 0.12;
  if (delta > threshold) {
    return "up";
  }
  if (delta < -threshold) {
    return "down";
  }
  return "flat";
}

export interface LiveLineProps {
  /** Key in data to use for y values */
  dataKey: string;
  /** Stroke color. Default: var(--chart-line-primary) */
  stroke?: string;
  /** Stroke width. Default: 2 */
  strokeWidth?: number;
  /** Curve function. Default: curveMonotoneX */
  curve?: CurveFactory;
  /** Show gradient fill under the curve. Default: true */
  fill?: boolean;
  /** Show pulsing live dot at the right edge. Default: true */
  pulse?: boolean;
  /** Radius of the live dot. Default: 4 */
  dotSize?: number;
  /** Show value badge pill at the live tip. Default: true */
  badge?: boolean;
  /** Value label formatter for the badge */
  formatValue?: (v: number) => string;
  /**
   * When set, the line/fill color changes based on momentum direction.
   * Overrides `stroke` for the line and fill (dot always uses momentum colors).
   */
  momentumColors?: MomentumColors;
}

LiveLine.displayName = "LiveLine";

export function LiveLine({
  dataKey,
  stroke = chartCssVars.linePrimary,
  strokeWidth = 2,
  curve = curveMonotoneX,
  fill = true,
  pulse = true,
  dotSize = 4,
  badge = true,
  formatValue = (v: number) => v.toFixed(2),
  momentumColors,
}: LiveLineProps) {
  const {
    data,
    xScale,
    yScale,
    innerWidth,
    innerHeight,
    xAccessor,
    lines,
    tooltipData,
  } = useChart();

  const isScrubbing = tooltipData !== null;

  const uid = useId();
  const gradientId = `live-line-grad-${uid}`;
  const areaGradientId = `live-area-grad-${uid}`;
  const fadeId = `live-fade-${uid}`;
  const fadeMaskId = `live-fade-mask-${uid}`;

  const getX = useCallback(
    (d: Record<string, unknown>) => xScale(xAccessor(d)) ?? 0,
    [xScale, xAccessor]
  );

  const getY = useCallback(
    (d: Record<string, unknown>) => {
      const v = d[dataKey];
      return typeof v === "number" ? (yScale(v) ?? 0) : 0;
    },
    [dataKey, yScale]
  );

  // The second-to-last point is the "now" position (live tip).
  // The last point is the queued future point for the fade-out zone.
  const nowPoint = data.length >= 2 ? data.at(-2) : data.at(-1);
  const liveValue =
    nowPoint && typeof nowPoint[dataKey] === "number"
      ? (nowPoint[dataKey] as number)
      : 0;

  const liveDotX = nowPoint ? (xScale(xAccessor(nowPoint)) ?? 0) : innerWidth;
  const liveDotY = yScale(liveValue) ?? 0;

  const momentum = useMemo(
    () => detectMomentum(data, dataKey),
    [data, dataKey]
  );

  const defaultMomentumColors: MomentumColors = {
    up: "var(--chart-1)",
    down: "var(--chart-5)",
    flat: stroke,
  };
  const dotMomentumColors = momentumColors ?? defaultMomentumColors;
  const dotColor = dotMomentumColors[momentum];

  // Find the line config for this dataKey to get the resolved stroke
  const lineConfig = lines.find((l) => l.dataKey === dataKey);
  const baseStroke = lineConfig?.stroke ?? stroke;
  const resolvedStroke = momentumColors ? momentumColors[momentum] : baseStroke;

  return (
    <>
      <defs>
        <linearGradient id={gradientId} x1="0" x2="0" y1="0" y2="1">
          <stop offset="0%" stopColor={resolvedStroke} stopOpacity={1} />
          <stop offset="100%" stopColor={resolvedStroke} stopOpacity={0.6} />
        </linearGradient>
        <linearGradient id={areaGradientId} x1="0" x2="0" y1="0" y2="1">
          <stop offset="0%" stopColor={resolvedStroke} stopOpacity={0.1} />
          <stop offset="100%" stopColor={resolvedStroke} stopOpacity={0} />
        </linearGradient>
        <linearGradient id={fadeId} x1="0" x2="1" y1="0" y2="0">
          <stop offset="0%" stopColor="white" stopOpacity={0} />
          <stop offset="4%" stopColor="white" stopOpacity={1} />
          {liveDotX < innerWidth - 1 ? (
            <>
              <stop
                offset={`${(liveDotX / innerWidth) * 100}%`}
                stopColor="white"
                stopOpacity={1}
              />
              <stop offset="100%" stopColor="white" stopOpacity={0} />
            </>
          ) : (
            <stop offset="100%" stopColor="white" stopOpacity={1} />
          )}
        </linearGradient>
        <mask id={fadeMaskId}>
          <rect
            fill={`url(#${fadeId})`}
            height={innerHeight + 40}
            width={innerWidth}
            x={0}
            y={-20}
          />
        </mask>
      </defs>

      {/* Area fill */}
      {fill && data.length > 1 && (
        <g mask={`url(#${fadeMaskId})`}>
          <AreaClosed
            curve={curve}
            data={data}
            fill={`url(#${areaGradientId})`}
            strokeWidth={0}
            x={getX}
            y={getY}
            yScale={yScale}
          />
        </g>
      )}

      {/* Line */}
      {data.length > 1 && (
        <g mask={`url(#${fadeMaskId})`}>
          <LinePath
            curve={curve}
            data={data}
            stroke={`url(#${gradientId})`}
            strokeLinecap="round"
            strokeLinejoin="round"
            strokeWidth={strokeWidth}
            x={getX}
            y={getY}
          />
        </g>
      )}

      {/* Dashed horizontal line at current value */}
      <line
        opacity={0.25}
        stroke={resolvedStroke}
        strokeDasharray="4,4"
        strokeWidth={1}
        x1={0}
        x2={innerWidth}
        y1={liveDotY}
        y2={liveDotY}
      />

      {/* Live indicator (dot + badge) — dims when crosshair is active */}
      <motion.g
        animate={{ opacity: isScrubbing ? 0.25 : 1 }}
        transition={{ duration: 0.3, ease: "easeInOut" }}
      >
        {/* Pulsing dot */}
        <g>
          {pulse && (
            <circle
              cx={liveDotX}
              cy={liveDotY}
              fill="none"
              opacity={0.4}
              r={dotSize * 2}
              stroke={dotColor}
              strokeWidth={1.5}
            >
              <animate
                attributeName="r"
                dur="1.5s"
                from={String(dotSize)}
                repeatCount="indefinite"
                to={String(dotSize * 3.5)}
              />
              <animate
                attributeName="opacity"
                dur="1.5s"
                from="0.5"
                repeatCount="indefinite"
                to="0"
              />
            </circle>
          )}
          <circle
            cx={liveDotX}
            cy={liveDotY}
            fill={dotColor}
            opacity={0.1}
            r={dotSize + 2}
          />
          <circle
            cx={liveDotX}
            cy={liveDotY}
            fill={dotColor}
            r={dotSize}
            stroke={chartCssVars.background}
            strokeWidth={2}
          />
        </g>

        {/* Badge — use popover vars so text is never white-on-white */}
        {badge && (
          <g transform={`translate(${liveDotX + 12},${liveDotY})`}>
            <rect
              fill="var(--popover)"
              height={24}
              opacity={0.95}
              rx={6}
              width={formatValue(liveValue).length * 7.5 + 16}
              x={0}
              y={-12}
            />
            <text
              fill="var(--popover-foreground)"
              fontFamily="SF Mono, Menlo, Monaco, monospace"
              fontSize={11}
              fontWeight={500}
              x={8}
              y={4}
            >
              {formatValue(liveValue)}
            </text>
          </g>
        )}
      </motion.g>
    </>
  );
}

export default LiveLine;
```

##### File: `src/charts/live-x-axis.tsx`

```tsx
"use client";

import { motion, useSpring } from "motion/react";
import { memo, useEffect, useMemo, useRef, useState } from "react";
import { createPortal } from "react-dom";
import { useChart, useChartStable } from "./chart-context";
import { hmsTimeFmt } from "./chart-formatters";

const TICKER_HALF_WIDTH = 50;
const FADE_BUFFER = 20;

const crosshairSpringConfig = { stiffness: 300, damping: 30 };

function labelFadeOpacity(
  labelX: number,
  crosshairX: number | null,
  isHovering: boolean
): number {
  if (!isHovering || crosshairX === null) {
    return 1;
  }
  const distance = Math.abs(labelX - crosshairX);
  if (distance < TICKER_HALF_WIDTH) {
    return 0;
  }
  if (distance < TICKER_HALF_WIDTH + FADE_BUFFER) {
    return (distance - TICKER_HALF_WIDTH) / FADE_BUFFER;
  }
  return 1;
}

export interface LiveXAxisProps {
  /** Number of time labels. Default: 5 */
  numTicks?: number;
  /** Time formatter. Default: HH:MM:SS */
  formatTime?: (t: number) => string;
}

const defaultFormatTime = (t: number) => hmsTimeFmt.format(new Date(t));

export function LiveXAxis(props: LiveXAxisProps) {
  const { containerRef } = useChartStable();
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    setMounted(true);
  }, []);

  const container = containerRef.current;
  if (!(mounted && container)) {
    return null;
  }

  return <LiveXAxisInner {...props} container={container} />;
}

const LiveXAxisInner = memo(function LiveXAxisInner({
  numTicks = 5,
  formatTime = defaultFormatTime,
  container,
}: LiveXAxisProps & { container: HTMLDivElement }) {
  const { xScale, margin, tooltipData } = useChart();

  const domain = xScale.domain();
  const startMs = domain[0]?.getTime() ?? 0;
  const endMs = domain[1]?.getTime() ?? 0;

  const labels = useMemo(() => {
    const step = (endMs - startMs) / (numTicks - 1);
    return Array.from({ length: numTicks }, (_, i) => {
      const t = startMs + i * step;
      const x = (xScale(new Date(t)) ?? 0) + margin.left;
      return { x, label: formatTime(t), stableKey: i };
    });
  }, [startMs, endMs, numTicks, xScale, margin.left, formatTime]);

  const isHovering = tooltipData !== null;
  const crosshairX = tooltipData ? tooltipData.x + margin.left : null;

  // Time pill label
  const pillLabel = useMemo(() => {
    if (!tooltipData) {
      return null;
    }
    const timeMs = xScale.invert(tooltipData.x).getTime();
    return formatTime(timeMs);
  }, [tooltipData, xScale, formatTime]);

  // Spring-animated pill position — matches TooltipIndicator's spring config
  // so the pill and crosshair line move in lockstep
  const pillX = tooltipData ? tooltipData.x + margin.left : 0;
  const animatedPillX = useSpring(pillX, crosshairSpringConfig);
  const springRef = useRef(animatedPillX);
  springRef.current = animatedPillX;

  useEffect(() => {
    springRef.current.set(pillX);
  }, [pillX]);

  return createPortal(
    <div className="pointer-events-none absolute inset-0">
      {/* Time labels */}
      {labels.map((l) => (
        <div
          className="absolute"
          key={l.stableKey}
          style={{
            left: l.x,
            bottom: 12,
            width: 0,
            display: "flex",
            justifyContent: "center",
          }}
        >
          <motion.span
            animate={{
              opacity: labelFadeOpacity(l.x, crosshairX, isHovering),
            }}
            className="whitespace-nowrap text-chart-label text-xs"
            transition={{ duration: 0.15, ease: "easeOut" }}
          >
            {l.label}
          </motion.span>
        </div>
      ))}

      {/* Time pill at crosshair — spring-animated to match crosshair line */}
      {isHovering && pillLabel && (
        <motion.div
          className="absolute z-50"
          style={{
            left: animatedPillX,
            x: "-50%",
            bottom: 4,
          }}
        >
          <div className="overflow-hidden rounded-full bg-zinc-900 px-4 py-1 text-white shadow-lg dark:bg-zinc-100 dark:text-zinc-900">
            <span className="whitespace-nowrap font-medium text-sm">
              {pillLabel}
            </span>
          </div>
        </motion.div>
      )}
    </div>,
    container
  );
});

LiveXAxis.displayName = "LiveXAxis";

export default LiveXAxis;
```

##### File: `src/charts/live-y-axis.tsx`

```tsx
"use client";

import { AnimatePresence, motion } from "motion/react";
import { memo, useEffect, useMemo, useRef, useState } from "react";
import { createPortal } from "react-dom";
import { useChartStable } from "./chart-context";

// ---------------------------------------------------------------------------
// Interval picker (inspired by liveline's pickInterval)
// Finds a "nice" step size that keeps labels ~minGap pixels apart.
// Uses hysteresis: keeps the previous interval if it still fits, preventing
// jittery step changes when the range oscillates near a boundary.
// ---------------------------------------------------------------------------

function pickNiceInterval(
  valRange: number,
  chartHeight: number,
  minGap: number,
  prevInterval: number
): number {
  if (valRange <= 0 || chartHeight <= 0) {
    return 1;
  }
  const pxPerUnit = chartHeight / valRange;

  // Keep previous interval if it still produces reasonable spacing
  if (prevInterval > 0) {
    const px = prevInterval * pxPerUnit;
    if (px >= minGap * 0.5 && px <= minGap * 3) {
      return prevInterval;
    }
  }

  // Try multiple divisor sequences to find the best nice step
  const divisorSets = [
    [2, 2.5, 2],
    [2, 2, 2.5],
    [2.5, 2, 2],
  ];
  let best = Number.POSITIVE_INFINITY;
  for (const divs of divisorSets) {
    let span = 10 ** Math.ceil(Math.log10(valRange));
    let i = 0;
    let d = divs[i % 3] ?? 2;
    while ((span / d) * pxPerUnit >= minGap) {
      span /= d;
      i++;
      d = divs[i % 3] ?? 2;
    }
    if (span < best) {
      best = span;
    }
  }
  return best === Number.POSITIVE_INFINITY ? valRange / 5 : best;
}

// ---------------------------------------------------------------------------
// Edge fade: labels near the top/bottom of the chart area fade out
// ---------------------------------------------------------------------------

const EDGE_FADE_PX = 28;

function edgeOpacity(y: number, chartHeight: number): number {
  const fromEdge = Math.min(y, chartHeight - y);
  if (fromEdge >= EDGE_FADE_PX) {
    return 1;
  }
  if (fromEdge <= 0) {
    return 0;
  }
  return fromEdge / EDGE_FADE_PX;
}

// ---------------------------------------------------------------------------
// Component
// ---------------------------------------------------------------------------

export interface LiveYAxisProps {
  /** Minimum pixel gap between labels. Default: 36 */
  minGap?: number;
  /** Position. Default: "left" */
  position?: "left" | "right";
  /** Value formatter */
  formatValue?: (v: number) => string;
  /** Allow decimal tick values. Default: true */
  allowDecimals?: boolean;
}

const tickSpring = { type: "spring" as const, stiffness: 180, damping: 24 };

export function LiveYAxis(props: LiveYAxisProps) {
  const { containerRef } = useChartStable();
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    setMounted(true);
  }, []);

  const container = containerRef.current;
  if (!(mounted && container)) {
    return null;
  }

  return <LiveYAxisInner {...props} container={container} />;
}

const LiveYAxisInner = memo(function LiveYAxisInner({
  minGap = 36,
  position = "left",
  formatValue = (v: number) => v.toFixed(2),
  allowDecimals = true,
  container,
}: LiveYAxisProps & { container: HTMLDivElement }) {
  const { yScale, margin, innerHeight } = useChartStable();
  const intervalRef = useRef(0);

  const domain = yScale.domain() as [number, number];
  const minVal = domain[0];
  const maxVal = domain[1];
  const valRange = maxVal - minVal;

  // Pick a nice interval with hysteresis
  const interval = useMemo(() => {
    const next = pickNiceInterval(
      valRange,
      innerHeight,
      minGap,
      intervalRef.current
    );
    intervalRef.current = next;
    return next;
  }, [valRange, innerHeight, minGap]);

  // Stabilize the tick VALUE set: only recompute which ticks exist when the
  // domain crosses an interval boundary. We quantize min/max to interval
  // boundaries so the set doesn't change on every sub-pixel lerp frame.
  const quantizedMin = interval > 0 ? Math.floor(minVal / interval) : 0;
  const quantizedMax = interval > 0 ? Math.ceil(maxVal / interval) : 0;

  // biome-ignore lint/correctness/useExhaustiveDependencies: quantized values are intentional coarse-grained deps for stability
  const stableTickValues = useMemo(() => {
    if (interval <= 0 || valRange <= 0) {
      return [];
    }
    const expandedMin = minVal - interval * 0.5;
    const expandedMax = maxVal + interval * 0.5;
    const first = Math.ceil(expandedMin / interval) * interval;
    const values: number[] = [];
    for (let v = first; v <= expandedMax; v += interval) {
      const rounded = Math.round(v * 1e10) / 1e10;
      const isDecimal = !Number.isInteger(rounded);
      if (isDecimal && !allowDecimals) {
        continue;
      }
      values.push(rounded);
    }
    return values;
  }, [
    quantizedMin,
    quantizedMax,
    interval,
    minVal,
    maxVal,
    valRange,
    allowDecimals,
  ]);

  // Pixel positions update every frame for smooth movement
  const tickData = useMemo(
    () =>
      stableTickValues
        .map((value) => {
          const y = yScale(value) ?? 0;
          return {
            value,
            y,
            label: formatValue(value),
            key: value.toPrecision(10),
            edgeAlpha: edgeOpacity(y, innerHeight),
          };
        })
        .filter((t) => t.y >= -10 && t.y <= innerHeight + 10),
    [stableTickValues, yScale, innerHeight, formatValue]
  );

  const isLeft = position === "left";

  return createPortal(
    <div className="pointer-events-none absolute inset-0">
      <div
        className="absolute overflow-hidden"
        style={{
          top: margin.top,
          height: innerHeight,
          ...(isLeft
            ? { left: 0, width: margin.left }
            : { right: 0, width: margin.right }),
        }}
      >
        <AnimatePresence initial={false}>
          {tickData.map((tick) => (
            <motion.div
              animate={{ opacity: tick.edgeAlpha, y: tick.y }}
              className="absolute w-full"
              exit={{ opacity: 0 }}
              initial={{ opacity: 0, y: tick.y }}
              key={tick.key}
              style={{
                ...(isLeft
                  ? { right: 0, paddingRight: 8, textAlign: "right" }
                  : { left: 0, paddingLeft: 8, textAlign: "left" }),
              }}
              transition={tickSpring}
            >
              <span className="whitespace-nowrap font-mono text-chart-label text-xs">
                {tick.label}
              </span>
            </motion.div>
          ))}
        </AnimatePresence>
      </div>
    </div>,
    container
  );
});

LiveYAxis.displayName = "LiveYAxis";

export default LiveYAxis;
```

---

### Pie Chart <a name="pie-chart"></a>

A composable pie and donut chart with animated slices, hover interactions, patterns, gradients, and an interactive legend

- **Category**: Charts
- **Registry Key**: `pie-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/pie-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `@number-flow/react`
  - `@visx/group@4.0.1-alpha.0`
  - `@visx/responsive@4.0.1-alpha.0`
  - `@visx/shape@4.0.1-alpha.0`
  - `d3-shape`
  - `motion`
- **Local Registry Dependencies**:
  - `@bklit/chart-animation`
  - `@bklit/utils`

#### How to Use

```tsx
import { PieChart, PieSlice } from "@bklitui/ui/charts";

const data = [
  { label: "Electronics", value: 4250, color: "#0ea5e9" },
  { label: "Clothing", value: 3120, color: "#a855f7" },
  { label: "Food", value: 2100, color: "#f59e0b" },
];

export default function SalesChart() {
  return (
    <PieChart data={data} size={280}>
      {data.map((_, index) => (
        <PieSlice key={index} index={index} />
      ))}
    </PieChart>
  );
}
```

#### Props & API Reference

##### `PieChart` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `data` | `PieData[]` | required | Array of pie data items |
| `size` | `number` | auto | Fixed size in pixels (uses parent if not set) |
| `innerRadius` | `number` | `0` | Inner radius for donut charts (0 = solid pie) |
| `padAngle` | `number` | `0` | Padding angle between slices (radians) |
| `cornerRadius` | `number` | `0` | Corner radius for rounded slice edges |
| `startAngle` | `number` | `-PI/2` | Start angle in radians (top) |
| `endAngle` | `number` | `3*PI/2` | End angle in radians (full circle) |
| `hoveredIndex` | `number \| null` | - | Controlled hover state |
| `onHoverChange` | `(index: number \| null) => void` | - | Hover state callback |
| `className` | `string` | `""` | Additional CSS class |

##### `PieSlice` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `index` | `number` | required | Index of the slice in the data array |
| `color` | `string` | from data/palette | Optional color override |
| `fill` | `string` | - | Optional fill for patterns/gradients (e.g., `url(#patternId)`) |
| `animate` | `boolean` | `true` | Enable animation on mount |
| `showGlow` | `boolean` | `true` | Show glow effect on hover |
| `hoverEffect` | `"translate" \| "grow" \| "none"` | `"translate"` | Hover animation type |
| `hoverOffset` | `number` | `10` | Distance in pixels for hover effect |

##### `PieCenter` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `defaultLabel` | `string` | `"Total"` | Label shown when not hovering |
| `formatOptions` | `NumberFlowFormat` | - | Number formatting options |
| `prefix` | `string` | - | Prefix before the value (e.g., "$") |
| `suffix` | `string` | - | Suffix after the value (e.g., "%") |
| `children` | `function` | - | Custom render function |
| `className` | `string` | `""` | Additional CSS class |

#### Component Source Code

##### File: `src/charts/pie-chart.tsx`

```tsx
"use client";

import { Group } from "@visx/group";
import { ParentSize } from "@visx/responsive";
import { pie as d3Pie } from "d3-shape";
import type { Transition } from "motion/react";
import {
  Children,
  isValidElement,
  memo,
  type ReactElement,
  type ReactNode,
  useCallback,
  useMemo,
  useRef,
  useState,
} from "react";
import { cn } from "@/lib/utils";
import {
  defaultPieColors,
  type PieArcData,
  type PieContextValue,
  type PieData,
  PieProvider,
} from "./pie-context";

/** Default hover offset in pixels */
export const DEFAULT_HOVER_OFFSET = 10;

export interface PieChartProps {
  /** Data array - each item represents a slice */
  data: PieData[];
  /** Chart size in pixels. If not provided, uses parent container size */
  size?: number;
  /** Inner radius for donut charts. Default: 0 (solid pie) */
  innerRadius?: number;
  /** Padding angle between slices in radians. Default: 0 */
  padAngle?: number;
  /** Corner radius for rounded slice edges. Default: 0 */
  cornerRadius?: number;
  /** Start angle in radians. Default: -PI/2 (top) */
  startAngle?: number;
  /** End angle in radians. Default: 3*PI/2 (full circle from top) */
  endAngle?: number;
  /** Additional class name for the container */
  className?: string;
  /** Controlled hover state - index of hovered slice */
  hoveredIndex?: number | null;
  /** Callback when hover state changes */
  onHoverChange?: (index: number | null) => void;
  /**
   * Hover offset in pixels for slice hover effects.
   * This also determines the padding around the chart to prevent clipping.
   * Default: 10
   */
  hoverOffset?: number;
  /** Child components (PieSlice, PieCenter, patterns, gradients, etc.) */
  children: ReactNode;
  /** Framer Motion transition for slice enter animation */
  enterTransition?: Transition;
  /** Scales slice stagger delays (1 = default). */
  enterStaggerScale?: number;
}

interface PieChartInnerProps {
  width: number;
  height: number;
  data: PieData[];
  innerRadius: number;
  padAngle: number;
  cornerRadius: number;
  startAngle: number;
  endAngle: number;
  hoverOffset: number;
  children: ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
  hoveredIndexProp?: number | null;
  onHoverChange?: (index: number | null) => void;
  enterTransition?: Transition;
  enterStaggerScale: number;
}

// Helper to check if a child is a PieCenter component
function isPieCenter(child: ReactNode): boolean {
  return (
    isValidElement(child) &&
    typeof child.type === "function" &&
    ((child.type as { displayName?: string }).displayName === "PieCenter" ||
      (child.type as { name?: string }).name === "PieCenter")
  );
}

// Helper to check if a component is a gradient or pattern definition
function isDefsComponent(child: ReactElement): boolean {
  const displayName =
    (child.type as { displayName?: string })?.displayName ||
    (child.type as { name?: string })?.name ||
    "";
  return (
    displayName.includes("Gradient") ||
    displayName.includes("Pattern") ||
    displayName === "LinearGradient" ||
    displayName === "RadialGradient"
  );
}

function PieChartInner(props: PieChartInnerProps) {
  const size = Math.min(props.width, props.height);

  if (size < 10) {
    return null;
  }

  return <PieChartCore {...props} />;
}

const PieChartCore = memo(function PieChartCore({
  width,
  height,
  data,
  innerRadius: innerRadiusProp,
  padAngle,
  cornerRadius,
  startAngle,
  endAngle,
  hoverOffset,
  children,
  containerRef,
  hoveredIndexProp,
  onHoverChange,
  enterTransition,
  enterStaggerScale,
}: PieChartInnerProps) {
  const [internalHoveredIndex, setInternalHoveredIndex] = useState<
    number | null
  >(null);
  const [animationKey] = useState(0);
  const [isLoaded, setIsLoaded] = useState(false);

  // Use controlled or uncontrolled hover state
  const isControlled = hoveredIndexProp !== undefined;
  const hoveredIndex = isControlled ? hoveredIndexProp : internalHoveredIndex;
  const setHoveredIndex = useCallback(
    (index: number | null) => {
      if (isControlled) {
        onHoverChange?.(index);
      } else {
        setInternalHoveredIndex(index);
      }
    },
    [isControlled, onHoverChange]
  );

  // Use the smaller dimension to ensure the chart fits
  const size = Math.min(width, height);
  const center = size / 2;

  // Calculate radii with padding based on hover offset to prevent clipping
  const padding = hoverOffset;
  const outerRadius = center - padding;
  const innerRadius = innerRadiusProp;

  // Calculate total value
  const totalValue = useMemo(
    () => data.reduce((sum, d) => sum + d.value, 0),
    [data]
  );

  // Get color for a slice index
  const getColor = useCallback(
    (index: number) => {
      const item = data[index];
      if (item?.color) {
        return item.color;
      }
      return defaultPieColors[index % defaultPieColors.length] as string;
    },
    [data]
  );

  // Get fill for a slice index (supports patterns/gradients)
  const getFill = useCallback(
    (index: number) => {
      const item = data[index];
      // Check for explicit fill (pattern/gradient URL)
      if (item?.fill) {
        return item.fill;
      }
      // Fall back to color
      return getColor(index);
    },
    [data, getColor]
  );

  // Compute arcs using d3-shape pie
  const arcs = useMemo(() => {
    const pieGenerator = d3Pie<PieData>()
      .value((d) => d.value)
      .startAngle(startAngle)
      .endAngle(endAngle)
      .padAngle(padAngle)
      .sort(null); // Maintain data order

    const computed = pieGenerator(data);

    return computed.map((arc, index) => ({
      data: arc.data,
      index,
      startAngle: arc.startAngle,
      endAngle: arc.endAngle,
      padAngle: arc.padAngle,
      value: arc.value,
    })) as PieArcData[];
  }, [data, startAngle, endAngle, padAngle]);

  // Mark as loaded after initial render
  useState(() => {
    const timer = setTimeout(() => {
      setIsLoaded(true);
    }, 100);
    return () => clearTimeout(timer);
  });

  // Separate children into categories
  const { svgChildren, centerChildren, defsChildren } = useMemo(() => {
    const svgNodes: ReactNode[] = [];
    const centerNodes: ReactNode[] = [];
    const defsNodes: ReactElement[] = [];

    Children.forEach(children, (child) => {
      if (!isValidElement(child)) {
        svgNodes.push(child);
        return;
      }

      if (isPieCenter(child)) {
        centerNodes.push(child);
      } else if (isDefsComponent(child)) {
        defsNodes.push(child);
      } else {
        svgNodes.push(child);
      }
    });

    return {
      svgChildren: svgNodes,
      centerChildren: centerNodes,
      defsChildren: defsNodes,
    };
  }, [children]);

  const contextValue: PieContextValue = {
    data,
    arcs,
    size,
    center,
    outerRadius,
    innerRadius,
    padAngle,
    cornerRadius,
    hoverOffset,
    hoveredIndex,
    setHoveredIndex,
    animationKey,
    isLoaded,
    enterTransition,
    enterStaggerScale,
    containerRef,
    totalValue,
    getColor,
    getFill,
  };

  // Use CSS Grid stacking to layer SVG and HTML content
  // This avoids Safari's foreignObject rendering bugs
  return (
    <PieProvider value={contextValue}>
      <div
        className="grid"
        style={{
          gridTemplateColumns: "1fr",
          gridTemplateRows: "1fr",
          width: size,
          height: size,
        }}
      >
        {/* SVG layer with pie slices */}
        <svg
          aria-hidden="true"
          height={size}
          style={{ gridArea: "1 / 1" }}
          width={size}
        >
          {/* Defs for patterns and gradients */}
          {defsChildren.length > 0 && <defs>{defsChildren}</defs>}

          <Group left={center} top={center}>
            {svgChildren}
          </Group>
        </svg>

        {/* HTML layer with center content - stacked on top via grid */}
        {centerChildren.length > 0 && (
          <div
            className="pointer-events-none flex items-center justify-center"
            style={{ gridArea: "1 / 1" }}
          >
            {centerChildren}
          </div>
        )}
      </div>
    </PieProvider>
  );
});

export function PieChart({
  data,
  size: fixedSize,
  innerRadius = 0,
  padAngle = 0,
  cornerRadius = 0,
  startAngle = -Math.PI / 2,
  endAngle = (3 * Math.PI) / 2,
  className = "",
  hoveredIndex,
  onHoverChange,
  hoverOffset = DEFAULT_HOVER_OFFSET,
  enterTransition,
  enterStaggerScale = 1,
  children,
}: PieChartProps) {
  const containerRef = useRef<HTMLDivElement>(null);

  // If fixed size is provided, use it directly
  if (fixedSize) {
    return (
      <div
        className={cn("relative flex items-center justify-center", className)}
        ref={containerRef}
        style={{ width: fixedSize, height: fixedSize }}
      >
        <PieChartInner
          containerRef={containerRef}
          cornerRadius={cornerRadius}
          data={data}
          endAngle={endAngle}
          enterStaggerScale={enterStaggerScale}
          enterTransition={enterTransition}
          height={fixedSize}
          hoveredIndexProp={hoveredIndex}
          hoverOffset={hoverOffset}
          innerRadius={innerRadius}
          onHoverChange={onHoverChange}
          padAngle={padAngle}
          startAngle={startAngle}
          width={fixedSize}
        >
          {children}
        </PieChartInner>
      </div>
    );
  }

  // Otherwise use ParentSize for responsive sizing
  return (
    <div
      className={cn("relative aspect-square w-full", className)}
      ref={containerRef}
    >
      <ParentSize debounceTime={10}>
        {({ width, height }) => (
          <PieChartInner
            containerRef={containerRef}
            cornerRadius={cornerRadius}
            data={data}
            endAngle={endAngle}
            enterStaggerScale={enterStaggerScale}
            enterTransition={enterTransition}
            height={height}
            hoveredIndexProp={hoveredIndex}
            hoverOffset={hoverOffset}
            innerRadius={innerRadius}
            onHoverChange={onHoverChange}
            padAngle={padAngle}
            startAngle={startAngle}
            width={width}
          >
            {children}
          </PieChartInner>
        )}
      </ParentSize>
    </div>
  );
}

PieChart.displayName = "PieChart";

export default PieChart;
```

##### File: `src/charts/pie-context.tsx`

```tsx
"use client";

import type { Transition } from "motion/react";
import { createContext, type RefObject, useContext } from "react";

// CSS variable references for pie chart theming
export const pieCssVars = {
  background: "var(--chart-background)",
  foreground: "var(--chart-foreground)",
  foregroundMuted: "var(--chart-foreground-muted)",
  label: "var(--chart-label)",
  // Default slice colors from chart palette
  slice1: "var(--chart-1)",
  slice2: "var(--chart-2)",
  slice3: "var(--chart-3)",
  slice4: "var(--chart-4)",
  slice5: "var(--chart-5)",
};

// Default slice color palette
export const defaultPieColors = [
  pieCssVars.slice1,
  pieCssVars.slice2,
  pieCssVars.slice3,
  pieCssVars.slice4,
  pieCssVars.slice5,
];

export interface PieData {
  /** Display label for the slice */
  label: string;
  /** Value for the slice (determines slice size relative to total) */
  value: number;
  /** Optional color override - falls back to palette */
  color?: string;
  /** Optional fill override for patterns/gradients (e.g., "url(#patternId)") */
  fill?: string;
}

/** Arc data computed by visx Pie */
export interface PieArcData {
  data: PieData;
  index: number;
  startAngle: number;
  endAngle: number;
  padAngle: number;
  value: number;
}

export interface PieContextValue {
  // Data
  data: PieData[];
  arcs: PieArcData[];

  // Dimensions
  size: number;
  center: number;
  outerRadius: number;
  innerRadius: number;
  padAngle: number;
  cornerRadius: number;

  // Hover effect
  hoverOffset: number;

  // Hover state
  hoveredIndex: number | null;
  setHoveredIndex: (index: number | null) => void;

  // Animation state
  animationKey: number;
  isLoaded: boolean;
  enterTransition?: Transition;
  enterStaggerScale: number;

  // Container ref for portals
  containerRef: RefObject<HTMLDivElement | null>;

  // Computed values
  totalValue: number;

  // Get color for a slice index
  getColor: (index: number) => string;

  // Get fill for a slice index (supports patterns/gradients)
  getFill: (index: number) => string;
}

const PieContext = createContext<PieContextValue | null>(null);

export function PieProvider({
  children,
  value,
}: {
  children: React.ReactNode;
  value: PieContextValue;
}) {
  return <PieContext.Provider value={value}>{children}</PieContext.Provider>;
}

export function usePie(): PieContextValue {
  const context = useContext(PieContext);
  if (!context) {
    throw new Error(
      "usePie must be used within a PieProvider. " +
        "Make sure your component is wrapped in <PieChart>."
    );
  }
  return context;
}

export default PieContext;
```

##### File: `src/charts/pie-slice.tsx`

```tsx
"use client";

import { arc as arcGenerator } from "@visx/shape";
import { motion, useSpring, useTransform } from "motion/react";
import { useEffect, useRef } from "react";
import { usePie } from "./pie-context";
import { useMountProgress } from "./use-mount-progress";

// Helper to generate arc path using d3 arc generator
function generateArcPath(
  innerRadius: number,
  outerRadius: number,
  startAngle: number,
  endAngle: number,
  cornerRadius: number,
  padAngle: number
): string {
  const generator = arcGenerator<unknown>({
    innerRadius,
    outerRadius,
    cornerRadius,
    padAngle,
  });
  return generator({ startAngle, endAngle } as unknown as null) || "";
}

// Calculate the translation offset for a slice to "pop out" along its radial axis
function getSliceOffset(
  startAngle: number,
  endAngle: number,
  distance: number
): { x: number; y: number } {
  // Calculate the midpoint angle of the slice
  const midAngle = (startAngle + endAngle) / 2;
  // In d3-shape, 0 radians is at 12 o'clock, angles increase clockwise
  // So the outward direction is: x = sin(angle), y = -cos(angle)
  return {
    x: Math.sin(midAngle) * distance,
    y: -Math.cos(midAngle) * distance,
  };
}

/** Hover effect types */
export type PieSliceHoverEffect = "translate" | "grow" | "none";

export interface PieSliceProps {
  /** Index of the slice in the data array */
  index: number;
  /** Optional color override - falls back to data color or palette */
  color?: string;
  /** Optional fill override for patterns/gradients (e.g., "url(#patternId)") */
  fill?: string;
  /** Animate the slice on mount. Default: true */
  animate?: boolean;
  /** Show glow effect on hover. Default: true */
  showGlow?: boolean;
  /**
   * Hover effect type. Default: "translate"
   * - "translate": Slice moves outward along its radial axis
   * - "grow": Slice extends its outer radius (gets longer)
   * - "none": No hover animation
   */
  hoverEffect?: PieSliceHoverEffect;
  /** Distance in pixels for hover effect (translate distance or grow amount). Defaults to PieChart's hoverOffset */
  hoverOffset?: number;
  /** Additional CSS class */
  className?: string;
}

interface AnimatedSliceTranslateProps {
  index: number;
  innerRadius: number;
  outerRadius: number;
  startAngle: number;
  endAngle: number;
  cornerRadius: number;
  padAngle: number;
  fill: string;
  color: string;
  isHovered: boolean;
  isFaded: boolean;
  animationKey: number;
  showGlow: boolean;
  hoverOffset: number;
}

function AnimatedSliceTranslate({
  index,
  innerRadius,
  outerRadius,
  startAngle,
  endAngle,
  cornerRadius,
  padAngle,
  fill,
  color,
  isHovered,
  isFaded,
  animationKey,
  showGlow,
  hoverOffset,
}: AnimatedSliceTranslateProps) {
  const {
    enterTransition,
    enterStaggerScale,
    animationKey: pieAnimationKey,
  } = usePie();
  const animationDelay = (0.1 + index * 0.08) * enterStaggerScale;
  const mountProgress = useMountProgress(
    enterTransition,
    animationDelay,
    pieAnimationKey
  );

  const animatedPath = useTransform(mountProgress, (mount) => {
    const currentEndAngle = startAngle + (endAngle - startAngle) * mount;
    if (currentEndAngle <= startAngle + 0.01) {
      return "";
    }
    return generateArcPath(
      innerRadius,
      outerRadius,
      startAngle,
      currentEndAngle,
      cornerRadius,
      padAngle
    );
  });

  const offset = getSliceOffset(startAngle, endAngle, hoverOffset);
  const glowColor = color;

  return (
    <motion.path
      animate={{
        opacity: isFaded ? 0.4 : 1,
        x: isHovered ? offset.x : 0,
        y: isHovered ? offset.y : 0,
      }}
      d={animatedPath}
      fill={fill}
      key={`slice-${animationKey}-${index}`}
      pointerEvents="none"
      style={{
        filter:
          showGlow && isHovered ? `drop-shadow(0 0 12px ${glowColor})` : "none",
      }}
      transition={{
        opacity: { duration: 0.15 },
        x: { type: "spring", stiffness: 400, damping: 25 },
        y: { type: "spring", stiffness: 400, damping: 25 },
      }}
    />
  );
}

interface AnimatedSliceGrowProps {
  index: number;
  innerRadius: number;
  outerRadius: number;
  startAngle: number;
  endAngle: number;
  cornerRadius: number;
  padAngle: number;
  fill: string;
  color: string;
  isHovered: boolean;
  isFaded: boolean;
  animationKey: number;
  showGlow: boolean;
  hoverOffset: number;
}

function AnimatedSliceGrow({
  index,
  innerRadius,
  outerRadius,
  startAngle,
  endAngle,
  cornerRadius,
  padAngle,
  fill,
  color,
  isHovered,
  isFaded,
  animationKey,
  showGlow,
  hoverOffset,
}: AnimatedSliceGrowProps) {
  const {
    enterTransition,
    enterStaggerScale,
    animationKey: pieAnimationKey,
  } = usePie();
  const animationDelay = (0.1 + index * 0.08) * enterStaggerScale;
  const mountProgress = useMountProgress(
    enterTransition,
    animationDelay,
    pieAnimationKey
  );

  const growSpring = useSpring(outerRadius, {
    stiffness: 400,
    damping: 25,
  });

  useEffect(() => {
    growSpring.set(isHovered ? outerRadius + hoverOffset : outerRadius);
  }, [isHovered, hoverOffset, outerRadius, growSpring]);

  const animatedPath = useTransform(
    [mountProgress, growSpring],
    ([mount, currentOuterRadius]) => {
      const currentEndAngle =
        startAngle + (endAngle - startAngle) * (mount as number);
      if (currentEndAngle <= startAngle + 0.01) {
        return "";
      }
      return generateArcPath(
        innerRadius,
        currentOuterRadius as number,
        startAngle,
        currentEndAngle,
        cornerRadius,
        padAngle
      );
    }
  );

  const glowColor = color;

  return (
    <motion.path
      animate={{
        opacity: isFaded ? 0.4 : 1,
      }}
      d={animatedPath}
      fill={fill}
      key={`slice-${animationKey}-${index}`}
      pointerEvents="none"
      style={{
        filter:
          showGlow && isHovered ? `drop-shadow(0 0 12px ${glowColor})` : "none",
      }}
      transition={{
        opacity: { duration: 0.15 },
      }}
    />
  );
}

export function PieSlice({
  index,
  color: colorProp,
  fill: fillProp,
  animate = true,
  showGlow = true,
  hoverEffect = "translate",
  hoverOffset: hoverOffsetProp,
}: PieSliceProps) {
  const {
    arcs,
    innerRadius,
    outerRadius,
    cornerRadius,
    hoverOffset: contextHoverOffset,
    hoveredIndex,
    setHoveredIndex,
    animationKey,
    getColor,
    getFill,
  } = usePie();

  // Use prop if provided, otherwise use context value
  const hoverOffset = hoverOffsetProp ?? contextHoverOffset;

  // Track if initial mount animation is complete
  const hasAnimated = useRef(false);
  const sliceExpandDelay = index * 0.08;

  useEffect(() => {
    if (animate && !hasAnimated.current) {
      const timeout = setTimeout(
        () => {
          hasAnimated.current = true;
        },
        (sliceExpandDelay + 0.5) * 1000
      );
      return () => clearTimeout(timeout);
    }
  }, [animate, sliceExpandDelay]);

  const arcData = arcs[index];
  if (!arcData) {
    return null;
  }

  const color = colorProp || getColor(index);
  const fill = fillProp || getFill(index);

  const isHovered = hoveredIndex === index;
  const isFaded = hoveredIndex !== null && hoveredIndex !== index;

  // Calculate values for non-animated/static paths
  const offset = getSliceOffset(
    arcData.startAngle,
    arcData.endAngle,
    hoverOffset
  );

  // Generate the static hitbox path (always uses base outer radius)
  const hitboxPath = generateArcPath(
    innerRadius,
    outerRadius,
    arcData.startAngle,
    arcData.endAngle,
    cornerRadius,
    arcData.padAngle
  );

  // Generate the visible path for grow effect
  const grownOuterRadius = isHovered ? outerRadius + hoverOffset : outerRadius;
  const grownPath = generateArcPath(
    innerRadius,
    grownOuterRadius,
    arcData.startAngle,
    arcData.endAngle,
    cornerRadius,
    arcData.padAngle
  );

  // Render animated slice based on effect type
  const renderAnimatedSlice = () => {
    if (hoverEffect === "grow") {
      return (
        <AnimatedSliceGrow
          animationKey={animationKey}
          color={color}
          cornerRadius={cornerRadius}
          endAngle={arcData.endAngle}
          fill={fill}
          hoverOffset={hoverOffset}
          index={index}
          innerRadius={innerRadius}
          isFaded={isFaded}
          isHovered={isHovered}
          outerRadius={outerRadius}
          padAngle={arcData.padAngle}
          showGlow={showGlow}
          startAngle={arcData.startAngle}
        />
      );
    }

    // Default: translate effect (also covers "none" with hoverOffset=0)
    return (
      <AnimatedSliceTranslate
        animationKey={animationKey}
        color={color}
        cornerRadius={cornerRadius}
        endAngle={arcData.endAngle}
        fill={fill}
        hoverOffset={hoverEffect === "none" ? 0 : hoverOffset}
        index={index}
        innerRadius={innerRadius}
        isFaded={isFaded}
        isHovered={isHovered}
        outerRadius={outerRadius}
        padAngle={arcData.padAngle}
        showGlow={showGlow}
        startAngle={arcData.startAngle}
      />
    );
  };

  // Render static (non-animated) slice
  const renderStaticSlice = () => {
    if (hoverEffect === "grow") {
      return (
        <motion.path
          animate={{
            opacity: isFaded ? 0.4 : 1,
            d: grownPath,
          }}
          d={hitboxPath}
          fill={fill}
          pointerEvents="none"
          style={{
            filter:
              showGlow && isHovered ? `drop-shadow(0 0 12px ${color})` : "none",
          }}
          transition={{
            opacity: { duration: 0.15 },
            d: { type: "spring", stiffness: 400, damping: 25 },
          }}
        />
      );
    }

    // Default: translate effect
    const shouldTranslate = hoverEffect !== "none" && isHovered;
    const translateX = shouldTranslate ? offset.x : 0;
    const translateY = shouldTranslate ? offset.y : 0;

    return (
      <motion.path
        animate={{
          opacity: isFaded ? 0.4 : 1,
          x: translateX,
          y: translateY,
        }}
        d={hitboxPath}
        fill={fill}
        pointerEvents="none"
        style={{
          filter:
            showGlow && isHovered ? `drop-shadow(0 0 12px ${color})` : "none",
        }}
        transition={{
          opacity: { duration: 0.15 },
          x: { type: "spring", stiffness: 400, damping: 25 },
          y: { type: "spring", stiffness: 400, damping: 25 },
        }}
      />
    );
  };

  return (
    <g style={{ cursor: "pointer" }}>
      {/* Invisible hitbox - stays in place, handles hover events */}
      {/* biome-ignore lint/a11y/noStaticElementInteractions: SVG path used as hover hitbox for visualization */}
      <path
        d={hitboxPath}
        fill="transparent"
        onMouseEnter={() => setHoveredIndex(index)}
        onMouseLeave={() => setHoveredIndex(null)}
      />

      {/* Visible slice - animates based on hover effect, no pointer events */}
      {animate ? renderAnimatedSlice() : renderStaticSlice()}
    </g>
  );
}

PieSlice.displayName = "PieSlice";

export default PieSlice;
```

##### File: `src/charts/chart-stat-flow.tsx`

```tsx
"use client";

import NumberFlow from "@number-flow/react";
import type { ReactNode } from "react";
import { cn } from "@/lib/utils";

/** Subset of `Intl.NumberFormatOptions` supported by NumberFlow */
export interface ChartStatFlowFormat {
  notation?: "standard" | "compact";
  compactDisplay?: "short" | "long";
  minimumFractionDigits?: number;
  maximumFractionDigits?: number;
  minimumIntegerDigits?: number;
  minimumSignificantDigits?: number;
  maximumSignificantDigits?: number;
  style?: "decimal" | "percent" | "currency";
  currency?: string;
  currencyDisplay?: "symbol" | "narrowSymbol" | "code" | "name";
  unit?: string;
  unitDisplay?: "short" | "long" | "narrow";
}

export const defaultChartStatFlowFormat: ChartStatFlowFormat = {
  notation: "standard",
  maximumFractionDigits: 0,
};

export interface ChartStatFlowProps {
  value: number;
  label: string;
  formatOptions?: ChartStatFlowFormat;
  prefix?: string;
  suffix?: string;
  valueClassName?: string;
  labelClassName?: string;
  icon?: ReactNode;
}

/**
 * Shared value + label stack using NumberFlow (same layout as pie / ring centers).
 * Parent should provide flex alignment and sizing when needed.
 */
export function ChartStatFlow({
  value,
  label,
  formatOptions = defaultChartStatFlowFormat,
  prefix,
  suffix,
  valueClassName = "text-2xl font-bold",
  labelClassName = "text-xs",
  icon,
}: ChartStatFlowProps) {
  return (
    <>
      {icon ? (
        <div className="mb-2 flex h-12 w-12 items-center justify-center rounded-full bg-muted/50">
          {icon}
        </div>
      ) : null}
      <span className={cn("text-foreground tabular-nums", valueClassName)}>
        <NumberFlow
          format={formatOptions}
          prefix={prefix}
          suffix={suffix}
          value={value}
          willChange
        />
      </span>
      <span className={cn("mt-0.5 text-chart-label", labelClassName)}>
        {label}
      </span>
    </>
  );
}

ChartStatFlow.displayName = "ChartStatFlow";
```

##### File: `src/charts/pie-center-shell.tsx`

```tsx
"use client";

import { pie as d3Pie } from "d3-shape";
import { useCallback, useEffect, useMemo, useRef, useState } from "react";
import { PieCenter, type PieCenterProps } from "./pie-center";
import {
  defaultPieColors,
  type PieArcData,
  type PieContextValue,
  type PieData,
  PieProvider,
} from "./pie-context";

const SHELL_HOVER_OFFSET = 10;

export type PieCenterShellProps = Omit<PieCenterProps, "children"> & {
  /** Value shown with NumberFlow (same role as pie total when not hovering) */
  centerValue: number;
  /** Square reference size for pie context (matches `PieChart` `size`) */
  contextSize: number;
  /** Inner radius in px — must be > 0 so `PieCenter` renders */
  innerRadiusPx: number;
  /**
   * When true (default), the first paint uses `0` then updates to `centerValue`
   * on the next frame so NumberFlow can run an entrance transition. Subsequent
   * `centerValue` updates animate as usual.
   */
  animateEntrance?: boolean;
};

/**
 * Renders {@link PieCenter} with a minimal {@link PieProvider} so you can reuse
 * the same center layout as a donut pie without mounting slices or a full {@link PieChart}.
 */
export function PieCenterShell({
  centerValue,
  contextSize,
  innerRadiusPx,
  animateEntrance = true,
  ...pieCenterProps
}: PieCenterShellProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const introStartedRef = useRef(false);

  const [flowTotal, setFlowTotal] = useState(() =>
    animateEntrance ? 0 : centerValue
  );

  useEffect(() => {
    if (!animateEntrance) {
      setFlowTotal(centerValue);
      return;
    }

    if (!introStartedRef.current) {
      introStartedRef.current = true;
      setFlowTotal(0);
      let innerRaf = 0;
      const outerRaf = requestAnimationFrame(() => {
        innerRaf = requestAnimationFrame(() => setFlowTotal(centerValue));
      });
      return () => {
        cancelAnimationFrame(outerRaf);
        cancelAnimationFrame(innerRaf);
        introStartedRef.current = false;
      };
    }

    setFlowTotal(centerValue);
  }, [animateEntrance, centerValue]);

  const data: PieData[] = useMemo(
    () => [{ label: "_pieCenterShell", value: Math.max(flowTotal, 0) }],
    [flowTotal]
  );

  const totalValue = flowTotal;

  const arcs = useMemo((): PieArcData[] => {
    const v = data[0]?.value ?? 0;
    if (v > 0) {
      const pieGenerator = d3Pie<PieData>()
        .value((d) => d.value)
        .startAngle(-Math.PI / 2)
        .endAngle((3 * Math.PI) / 2)
        .padAngle(0)
        .sort(null);
      const computed = pieGenerator(data);
      return computed.map((arc, index) => ({
        data: arc.data,
        index,
        startAngle: arc.startAngle,
        endAngle: arc.endAngle,
        padAngle: arc.padAngle,
        value: arc.value,
      })) as PieArcData[];
    }
    const d0 = data[0];
    if (!d0) {
      return [];
    }
    return [
      {
        data: d0,
        index: 0,
        startAngle: -Math.PI / 2,
        endAngle: (3 * Math.PI) / 2,
        padAngle: 0,
        value: 0,
      },
    ];
  }, [data]);

  const getColor = useCallback((index: number) => {
    return defaultPieColors[index % defaultPieColors.length] as string;
  }, []);

  const getFill = useCallback(
    (index: number) => {
      const item = data[index];
      if (item?.fill) {
        return item.fill;
      }
      return getColor(index);
    },
    [data, getColor]
  );

  const center = contextSize / 2;
  const outerRadius = center - SHELL_HOVER_OFFSET;

  const contextValue: PieContextValue = useMemo(
    () => ({
      data,
      arcs,
      size: contextSize,
      center,
      outerRadius,
      innerRadius: innerRadiusPx,
      padAngle: 0,
      cornerRadius: 0,
      hoverOffset: SHELL_HOVER_OFFSET,
      hoveredIndex: null,
      setHoveredIndex: () => undefined,
      animationKey: 0,
      isLoaded: true,
      enterStaggerScale: 1,
      containerRef,
      totalValue,
      getColor,
      getFill,
    }),
    [
      data,
      arcs,
      contextSize,
      center,
      outerRadius,
      innerRadiusPx,
      totalValue,
      getColor,
      getFill,
    ]
  );

  return (
    <PieProvider value={contextValue}>
      <PieCenter {...pieCenterProps} />
    </PieProvider>
  );
}

PieCenterShell.displayName = "PieCenterShell";
```

##### File: `src/charts/pie-center.tsx`

```tsx
"use client";

import type { ReactNode } from "react";
import { cn } from "@/lib/utils";
import {
  chartCenterContainerClassName,
  chartCenterLabelClassName,
  chartCenterValueClassName,
} from "./chart-center-typography";
import {
  ChartStatFlow,
  type ChartStatFlowFormat,
  defaultChartStatFlowFormat,
} from "./chart-stat-flow";
import { usePie } from "./pie-context";

export interface PieCenterProps {
  /** Label shown below the value. Default: "Total" when not hovering */
  defaultLabel?: string;
  /** Format options for NumberFlow. Default: standard notation */
  formatOptions?: ChartStatFlowFormat;
  /** Custom render function for complete control over center content */
  children?: (props: {
    value: number;
    label: string;
    isHovered: boolean;
    data: { label: string; value: number; color?: string; fill?: string };
  }) => ReactNode;
  /** Additional class name for the container */
  className?: string;
  /** Class name for the value text. Scales with center size via container queries. */
  valueClassName?: string;
  /** Class name for the label text. Scales with center size via container queries. */
  labelClassName?: string;
  /** Prefix to show before the number (e.g., "$") */
  prefix?: string;
  /** Suffix to show after the number (e.g., "%") */
  suffix?: string;
}

/**
 * PieCenter displays content in the center of a donut/pie chart.
 *
 * This component renders as pure HTML (not inside SVG foreignObject) to avoid
 * Safari's WebKit bug #23113 where HTML content with CSS transforms/opacity
 * inside foreignObject renders at incorrect positions.
 *
 * The parent PieChart uses CSS Grid stacking to overlay this HTML content
 * on top of the SVG slices.
 */
export function PieCenter({
  defaultLabel = "Total",
  formatOptions = defaultChartStatFlowFormat,
  children,
  className = "",
  valueClassName = chartCenterValueClassName,
  labelClassName = chartCenterLabelClassName,
  prefix,
  suffix,
}: PieCenterProps) {
  const { data, hoveredIndex, totalValue, innerRadius } = usePie();

  const hoveredData = hoveredIndex === null ? null : data[hoveredIndex];
  const displayValue = hoveredData ? hoveredData.value : totalValue;
  const displayLabel = hoveredData ? hoveredData.label : defaultLabel;

  // Calculate center area size based on inner radius
  // Leave some padding so text doesn't touch the inner edge
  const centerSize = innerRadius * 2 - 16;

  // Don't render if there's no inner radius (solid pie, not donut)
  if (innerRadius <= 0) {
    return null;
  }

  // If custom render function is provided, use it
  if (children && hoveredData) {
    return (
      <div
        className={cn(
          chartCenterContainerClassName,
          "flex items-center justify-center",
          className
        )}
        style={{ width: centerSize, height: centerSize }}
      >
        {children({
          value: displayValue,
          label: displayLabel,
          isHovered: hoveredIndex !== null,
          data: hoveredData,
        })}
      </div>
    );
  }

  // Default center content with NumberFlow animations
  // Now renders as pure HTML, avoiding Safari's foreignObject bugs
  return (
    <div
      className={cn(
        chartCenterContainerClassName,
        "flex flex-col items-center justify-center text-center",
        className
      )}
      style={{ width: centerSize, height: centerSize }}
    >
      <ChartStatFlow
        formatOptions={formatOptions}
        label={displayLabel}
        labelClassName={labelClassName}
        prefix={prefix}
        suffix={suffix}
        value={displayValue}
        valueClassName={valueClassName}
      />
    </div>
  );
}

PieCenter.displayName = "PieCenter";

export default PieCenter;
```

##### File: `src/charts/chart-center-typography.ts`

```tsx
/**
 * Fluid typography for pie / ring / gauge center labels.
 *
 * Uses CSS container query units (`cqw`) so values scale with the center
 * hole — not the viewport — which keeps stat text readable on small charts.
 */
export const chartCenterContainerClassName =
  "@container/chart-center size-full min-w-0";

/** Primary stat — ~22% of center width, clamped between text-sm and text-3xl. */
export const chartCenterValueClassName =
  "font-bold tabular-nums leading-none text-[clamp(0.75rem,22cqw,1.875rem)]";

/** Supporting label — ~9% of center width, clamped between 10px and text-xs. */
export const chartCenterLabelClassName =
  "max-w-full truncate leading-tight text-[clamp(0.625rem,9cqw,0.75rem)]";
```

---

### Profit/Loss Line <a name="profit-loss-line"></a>

Sign-colored line segments for profit and loss on a shared zero baseline

- **Category**: Charts
- **Registry Key**: `profit-loss-line`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/profit-loss-line.json
```

#### Dependencies

- **NPM Packages**:
  - `@visx/curve@4.0.1-alpha.0`
  - `@visx/shape@4.0.1-alpha.0`
- **Local Registry Dependencies**:
  - `@bklit/line-chart`
  - `@bklit/grid`
  - `@bklit/x-axis`
  - `@bklit/chart-tooltip`
  - `@bklit/legend`
  - `@bklit/utils`

#### How to Use

##### Example 1

```tsx
import {
  LineChart,
  Line,
  Grid,
  XAxis,
  ChartTooltip,
  ProfitLossLine,
  profitLossColor,
  resolveProfitLossTooltipLabel,
} from "@bklitui/ui/charts";
import { curveLinear } from "@visx/curve";

const data = [
  { date: new Date("2024-01-01"), pnl: 420 },
  { date: new Date("2024-01-02"), pnl: -180 },
  // ...
];

export default function ProfitLossChart() {
  return (
    <LineChart data={data}>
      <Grid highlightRowValues={[0]} horizontal />
      <Line
        curve={curveLinear}
        dataKey="pnl"
        fadeEdges={false}
        showHighlight={false}
        stroke="transparent"
        strokeWidth={0}
      />
      <ProfitLossLine dataKey="pnl" />
      <XAxis />
      <ChartTooltip
        indicatorColor={(point) => profitLossColor((point.pnl as number) ?? 0)}
        rows={(point) => {
          const value = (point.pnl as number) ?? 0;
          return [
            {
              color: profitLossColor(value),
              label: resolveProfitLossTooltipLabel(""),
              value,
            },
          ];
        }}
      />
    </LineChart>
  );
}
```

##### Example 2

```tsx
<Grid
  highlightRowValues={[0]}
  highlightRowStroke="var(--foreground)"
  highlightRowStrokeOpacity={0.35}
  horizontal
/>
```

#### Props & API Reference

##### `ProfitLossLine` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dataKey` | `string` | required | Key in data for y values |
| `xDataKey` | `string` | `"date"` | Key in data for x values |
| `strokeWidth` | `number` | `2.5` | Line width |
| `curve` | `CurveFactory` | `curveLinear` | Interpolation curve (same as `Line`) |
| `fadeEdges` | `FadeEdges` | `false` | Fade stroke toward transparent at chart edges |
| `positiveColor` | `string` | `var(--color-emerald-500)` | Color for profit segments |
| `negativeColor` | `string` | `var(--color-red-500)` | Color for loss segments |

##### `ProfitLossLegend` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `hoveredIndex` | `number \| null` | `null` | Controlled hover index |
| `onHoverChange` | `(index: number \| null) => void` | — | Hover callback |
| `align` | `"start" \| "center" \| "end"` | `"start"` | Horizontal alignment |
| `className` | `string` | — | Additional CSS class |

#### Component Source Code

##### File: `src/charts/profit-loss-line.tsx`

```tsx
"use client";

import { curveLinear } from "@visx/curve";
import { LinePath } from "@visx/shape";
import { useCallback, useId, useMemo } from "react";
import { useChart, useChartStable } from "./chart-context";
import {
  type FadeEdges,
  fadeGradientStops,
  resolveFadeSides,
} from "./fade-edges";
import { useProfitLossLegendHover } from "./profit-loss-legend-hover";
import { splitProfitLossSegments } from "./profit-loss-segments";

// CurveFactory type - simplified version compatible with visx
// biome-ignore lint/suspicious/noExplicitAny: d3 curve factory type
type CurveFactory = any;

export const PROFIT_LOSS_POSITIVE_COLOR = "var(--color-emerald-500)";
export const PROFIT_LOSS_NEGATIVE_COLOR = "var(--color-red-500)";

const LEGEND_DIM_OPACITY = 0.25;

export function profitLossColor(value: number) {
  return value >= 0 ? PROFIT_LOSS_POSITIVE_COLOR : PROFIT_LOSS_NEGATIVE_COLOR;
}

export const PROFIT_LOSS_TOOLTIP_LABEL_FALLBACK = "Profit/Loss";

export function resolveProfitLossTooltipLabel(label: string) {
  const trimmed = label.trim();
  return trimmed || PROFIT_LOSS_TOOLTIP_LABEL_FALLBACK;
}

export interface ProfitLossLineProps {
  dataKey: string;
  xDataKey?: string;
  strokeWidth?: number;
  positiveColor?: string;
  negativeColor?: string;
  /** Curve function. Default: curveLinear */
  curve?: CurveFactory;
  /**
   * Fade the line stroke toward transparent at the chart edges.
   * Default: false
   */
  fadeEdges?: FadeEdges;
}

function segmentLegendIndex(isPositive: boolean) {
  return isPositive ? 0 : 1;
}

export function ProfitLossLine({
  dataKey,
  xDataKey = "date",
  strokeWidth = 2.5,
  positiveColor = PROFIT_LOSS_POSITIVE_COLOR,
  negativeColor = PROFIT_LOSS_NEGATIVE_COLOR,
  curve = curveLinear,
  fadeEdges = false,
}: ProfitLossLineProps) {
  const { tooltipData } = useChart();
  const { hoveredIndex } = useProfitLossLegendHover();
  const { renderData, xScale, yScale, xAccessor, innerWidth } =
    useChartStable();
  const reactId = useId();
  const fadeSides = resolveFadeSides(fadeEdges);
  const fadeStops = fadeSides.any ? fadeGradientStops(fadeSides) : null;
  const positiveGradientId = `profit-loss-gradient-pos-${dataKey}-${reactId}`;
  const negativeGradientId = `profit-loss-gradient-neg-${dataKey}-${reactId}`;

  const focusedLegendIndex = useMemo(() => {
    if (hoveredIndex !== null) {
      return hoveredIndex;
    }
    if (!tooltipData) {
      return null;
    }
    const value = tooltipData.point[dataKey];
    if (typeof value !== "number") {
      return null;
    }
    return segmentLegendIndex(value >= 0);
  }, [dataKey, hoveredIndex, tooltipData]);

  const segments = useMemo(
    () =>
      splitProfitLossSegments({
        data: renderData,
        dataKey,
        xDataKey,
        xAccessor,
      }),
    [dataKey, renderData, xAccessor, xDataKey]
  );

  const getX = useCallback(
    (d: Record<string, unknown>) => xScale(xAccessor(d)) ?? 0,
    [xAccessor, xScale]
  );

  const getY = useCallback(
    (d: Record<string, unknown>) => {
      const value = d[dataKey];
      return typeof value === "number" ? (yScale(value) ?? 0) : 0;
    },
    [dataKey, yScale]
  );

  return (
    <>
      {fadeStops ? (
        <defs>
          <linearGradient
            gradientUnits="userSpaceOnUse"
            id={positiveGradientId}
            x1={0}
            x2={innerWidth}
            y1={0}
            y2={0}
          >
            {fadeStops.map((stop) => (
              <stop
                key={stop.offset}
                offset={stop.offset}
                style={{
                  stopColor: positiveColor,
                  stopOpacity: stop.opacity,
                }}
              />
            ))}
          </linearGradient>
          <linearGradient
            gradientUnits="userSpaceOnUse"
            id={negativeGradientId}
            x1={0}
            x2={innerWidth}
            y1={0}
            y2={0}
          >
            {fadeStops.map((stop) => (
              <stop
                key={stop.offset}
                offset={stop.offset}
                style={{
                  stopColor: negativeColor,
                  stopOpacity: stop.opacity,
                }}
              />
            ))}
          </linearGradient>
        </defs>
      ) : null}
      {segments.map((segment) => {
        const isDimmed =
          focusedLegendIndex !== null &&
          focusedLegendIndex !== segmentLegendIndex(segment.isPositive);
        const firstPoint = segment.data[0];
        const lastPoint = segment.data.at(-1);
        const segmentKey = `${dataKey}-${segment.isPositive ? "pos" : "neg"}-${String(firstPoint?.[xDataKey])}-${String(lastPoint?.[xDataKey])}`;
        const stroke = segment.isPositive ? positiveColor : negativeColor;
        const segmentStroke = fadeStops
          ? `url(#${segment.isPositive ? positiveGradientId : negativeGradientId})`
          : stroke;

        return (
          <g
            key={segmentKey}
            opacity={isDimmed ? LEGEND_DIM_OPACITY : 1}
            style={{ transition: "opacity 0.2s ease-in-out" }}
          >
            <LinePath
              curve={curve}
              data={segment.data}
              stroke={segmentStroke}
              strokeLinecap="round"
              strokeLinejoin="round"
              strokeWidth={strokeWidth}
              x={getX}
              y={getY}
            />
          </g>
        );
      })}
    </>
  );
}

ProfitLossLine.displayName = "ProfitLossLine";
```

##### File: `src/charts/profit-loss-segments.ts`

```tsx
type SegmentSign = "positive" | "negative";

export interface ProfitLossSegment {
  data: Record<string, unknown>[];
  isPositive: boolean;
}

function resolveSign(value: number, fallback: SegmentSign): SegmentSign {
  if (value > 0) {
    return "positive";
  }
  if (value < 0) {
    return "negative";
  }
  return fallback;
}

function findInitialSign(
  data: Record<string, unknown>[],
  dataKey: string
): SegmentSign {
  for (const row of data) {
    const value = row[dataKey];
    if (typeof value !== "number") {
      continue;
    }
    if (value > 0) {
      return "positive";
    }
    if (value < 0) {
      return "negative";
    }
  }
  return "positive";
}

function interpolateZeroCrossing(
  a: Record<string, unknown>,
  b: Record<string, unknown>,
  dataKey: string,
  xDataKey: string,
  xAccessor: (d: Record<string, unknown>) => Date
): Record<string, unknown> {
  const ya = a[dataKey] as number;
  const yb = b[dataKey] as number;
  const t = ya / (ya - yb);
  const start = xAccessor(a).getTime();
  const end = xAccessor(b).getTime();
  const crossDate = new Date(start + t * (end - start));

  return {
    ...a,
    [xDataKey]: crossDate,
    [dataKey]: 0,
  };
}

/** Split a single series into contiguous segments above/below zero. */
export function splitProfitLossSegments({
  data,
  dataKey,
  xDataKey = "date",
  xAccessor,
}: {
  data: Record<string, unknown>[];
  dataKey: string;
  xDataKey?: string;
  xAccessor: (d: Record<string, unknown>) => Date;
}): ProfitLossSegment[] {
  if (data.length === 0) {
    return [];
  }

  const segments: ProfitLossSegment[] = [];
  let currentSign = findInitialSign(data, dataKey);
  const firstPoint = data[0];
  if (!firstPoint) {
    return [];
  }
  let currentSegment: Record<string, unknown>[] = [firstPoint];

  for (let i = 0; i < data.length - 1; i++) {
    const a = data[i];
    const b = data[i + 1];
    if (!(a && b)) {
      continue;
    }
    const ya = a[dataKey] as number;
    const yb = b[dataKey] as number;

    if (
      typeof ya === "number" &&
      typeof yb === "number" &&
      ya !== 0 &&
      yb !== 0 &&
      Math.sign(ya) !== Math.sign(yb)
    ) {
      const cross = interpolateZeroCrossing(a, b, dataKey, xDataKey, xAccessor);
      currentSegment.push(cross);
      segments.push({
        data: currentSegment,
        isPositive: currentSign === "positive",
      });
      currentSegment = [cross, b];
      currentSign = resolveSign(yb, currentSign);
      continue;
    }

    currentSegment.push(b);
    if (typeof yb === "number" && yb !== 0) {
      currentSign = resolveSign(yb, currentSign);
    }
  }

  if (currentSegment.length > 0) {
    segments.push({
      data: currentSegment,
      isPositive: currentSign === "positive",
    });
  }

  return segments;
}
```

##### File: `src/charts/profit-loss-legend-hover.tsx`

```tsx
"use client";

import { createContext, type ReactNode, useContext } from "react";

interface ProfitLossLegendHoverContextValue {
  hoveredIndex: number | null;
}

const ProfitLossLegendHoverContext =
  createContext<ProfitLossLegendHoverContextValue | null>(null);

export function ProfitLossLegendHoverProvider({
  hoveredIndex,
  children,
}: {
  hoveredIndex: number | null;
  children: ReactNode;
}) {
  return (
    <ProfitLossLegendHoverContext.Provider value={{ hoveredIndex }}>
      {children}
    </ProfitLossLegendHoverContext.Provider>
  );
}

export function useProfitLossLegendHover(): ProfitLossLegendHoverContextValue {
  const context = useContext(ProfitLossLegendHoverContext);
  return context ?? { hoveredIndex: null };
}
```

##### File: `src/charts/profit-loss-legend.tsx`

```tsx
"use client";

import { cn } from "@/lib/utils";
import { Legend, LegendItem, LegendLabel, LegendMarker } from "./legend/index";
import {
  PROFIT_LOSS_NEGATIVE_COLOR,
  PROFIT_LOSS_POSITIVE_COLOR,
} from "./profit-loss-line";

export const PROFIT_LOSS_LEGEND_ITEMS = [
  { label: "Profit", value: 0, color: PROFIT_LOSS_POSITIVE_COLOR },
  { label: "Loss", value: 0, color: PROFIT_LOSS_NEGATIVE_COLOR },
] as const;

export interface ProfitLossLegendProps {
  hoveredIndex?: number | null;
  onHoverChange?: (index: number | null) => void;
  align?: "start" | "center" | "end";
  className?: string;
}

const LEGEND_ALIGN_CLASSES: Record<
  NonNullable<ProfitLossLegendProps["align"]>,
  string
> = {
  start: "justify-start",
  center: "justify-center",
  end: "justify-end",
};

export function ProfitLossLegend({
  hoveredIndex = null,
  onHoverChange,
  align = "start",
  className,
}: ProfitLossLegendProps) {
  return (
    <div
      className={cn(
        "flex w-full shrink-0 px-1 py-2",
        LEGEND_ALIGN_CLASSES[align],
        className
      )}
    >
      <Legend
        className="flex-row flex-wrap gap-4"
        hoveredIndex={hoveredIndex}
        items={[...PROFIT_LOSS_LEGEND_ITEMS]}
        onHoverChange={onHoverChange}
      >
        <LegendItem className="flex items-center gap-2">
          <LegendMarker className="h-2.5 w-2.5" />
          <LegendLabel className="text-xs" />
        </LegendItem>
      </Legend>
    </div>
  );
}
```

---

### Radar Chart <a name="radar-chart"></a>

A composable multi-series radar chart with animated polygons, hover interactions, and customizable metrics

- **Category**: Charts
- **Registry Key**: `radar-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/radar-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `@visx/group@4.0.1-alpha.0`
  - `@visx/responsive@4.0.1-alpha.0`
  - `@visx/scale@4.0.1-alpha.0`
  - `@visx/shape@4.0.1-alpha.0`
  - `d3-shape`
  - `motion`
- **Local Registry Dependencies**:
  - `@bklit/chart-animation`
  - `@bklit/utils`

#### How to Use

##### Example 1

```tsx
import { RadarChart, RadarGrid, RadarAxis, RadarLabels, RadarArea } from "@bklitui/ui/charts";

const metrics = [
  { key: "speed", label: "Speed" },
  { key: "power", label: "Power" },
  { key: "technique", label: "Technique" },
];

const data = [
  { label: "Player A", color: "#3b82f6", values: { speed: 85, power: 70, technique: 90 } },
  { label: "Player B", color: "#f59e0b", values: { speed: 65, power: 95, technique: 60 } },
];

export default function PerformanceRadar() {
  return (
    <RadarChart data={data} metrics={metrics} size={400}>
      <RadarGrid />
      <RadarAxis />
      <RadarLabels />
      {data.map((item, index) => (
        <RadarArea key={item.label} index={index} />
      ))}
    </RadarChart>
  );
}
```

##### Example 2

```tsx
import { useRadar } from "@bklitui/ui/charts";

function CustomComponent() {
  const {
    data,
    metrics,
    radius,
    hoveredIndex,
    setHoveredIndex,
    getPointPosition,
  } = useRadar();
  // ...
}
```

#### Props & API Reference

##### `RadarChart` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `data` | `RadarData[]` | required | Array of data series |
| `metrics` | `RadarMetric[]` | required | Metrics to display |
| `size` | `number` | auto | Fixed size in pixels |
| `levels` | `number` | `5` | Number of grid circles |
| `margin` | `number` | `60` | Margin around chart |
| `animate` | `boolean` | `true` | Enable animations |
| `hoveredIndex` | `number \| null` | - | Controlled hover state |
| `onHoverChange` | `(index: number \| null) => void` | - | Hover callback |
| `className` | `string` | `""` | Additional CSS class |

##### `RadarGrid` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showLabels` | `boolean` | `true` | Show level value labels |
| `className` | `string` | `""` | Additional CSS class |

##### `RadarAxis` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `className` | `string` | `""` | Additional CSS class |

##### `RadarLabels` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `offset` | `number` | `24` | Distance from chart edge |
| `fontSize` | `number` | `11` | Font size for labels |
| `interactive` | `boolean` | `false` | Enable hover effects on labels |
| `className` | `string` | `""` | Additional CSS class |

##### `RadarArea` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `index` | `number` | required | Index in the data array |
| `color` | `string` | from data | Optional color override |
| `showPoints` | `boolean` | `true` | Show data point circles |
| `showGlow` | `boolean` | `true` | Show glow effect on hover |
| `className` | `string` | `""` | Additional CSS class |

#### Component Source Code

##### File: `src/charts/radar-chart.tsx`

```tsx
"use client";

import { Group } from "@visx/group";
import { ParentSize } from "@visx/responsive";
import { scaleLinear } from "@visx/scale";
import type { Transition } from "motion/react";
import { type ReactNode, useCallback, useState } from "react";
import { cn } from "@/lib/utils";
import {
  defaultRadarColors,
  type RadarContextValue,
  type RadarData,
  type RadarMetric,
  RadarProvider,
} from "./radar-context";

export interface RadarChartProps {
  /** Data array - each item represents a data series (polygon) */
  data: RadarData[];
  /** Metrics to display on the radar */
  metrics: RadarMetric[];
  /** Chart size in pixels. If not provided, uses parent container size */
  size?: number;
  /** Number of concentric grid circles. Default: 5 */
  levels?: number;
  /** Margin around the chart. Default: 60 */
  margin?: number;
  /** Enable animations. Default: true */
  animate?: boolean;
  /** Enter animation budget in ms. Default: 1100 */
  enterDurationMs?: number;
  /** Scales stagger timing (1 = default). */
  staggerScale?: number;
  /** Motion enter transition (spring or cubic-bezier tween). */
  enterTransition?: Transition;
  /** Changes when motion settings change — replays enter animations. */
  motionReplayKey?: string;
  /** Controlled hover state - index of hovered area */
  hoveredIndex?: number | null;
  /** Callback when hover state changes */
  onHoverChange?: (index: number | null) => void;
  /** Additional class name for the container */
  className?: string;
  /** Child components (RadarGrid, RadarAxis, RadarLabels, RadarArea) */
  children: ReactNode;
}

interface RadarChartInnerProps {
  width: number;
  height: number;
  data: RadarData[];
  metrics: RadarMetric[];
  levels: number;
  margin: number;
  animate: boolean;
  enterDurationMs: number;
  staggerScale: number;
  enterTransition?: Transition;
  motionReplayKey: string;
  children: ReactNode;
  hoveredIndexProp?: number | null;
  onHoverChange?: (index: number | null) => void;
}

function RadarChartInner({
  width,
  height,
  data,
  metrics,
  levels,
  margin,
  animate,
  enterDurationMs,
  staggerScale,
  enterTransition,
  motionReplayKey,
  children,
  hoveredIndexProp,
  onHoverChange,
}: RadarChartInnerProps) {
  const [internalHoveredIndex, setInternalHoveredIndex] = useState<
    number | null
  >(null);

  // Use controlled or uncontrolled hover state
  const isControlled = hoveredIndexProp !== undefined;
  const hoveredIndex = isControlled ? hoveredIndexProp : internalHoveredIndex;
  const setHoveredIndex = useCallback(
    (index: number | null) => {
      if (isControlled) {
        onHoverChange?.(index);
      } else {
        setInternalHoveredIndex(index);
      }
    },
    [isControlled, onHoverChange]
  );

  // Use the smaller dimension
  const size = Math.min(width, height);
  const radius = (size - margin * 2) / 2;

  // Scale for converting values (0-100) to radius
  const yScale = useCallback(
    (value: number) => {
      const scale = scaleLinear<number>({
        range: [0, radius],
        domain: [0, 100],
      });
      return scale(value) ?? 0;
    },
    [radius]
  );

  // Get angle for a metric index (rotated so first metric is at top)
  const getAngle = useCallback(
    (metricIndex: number) => {
      const step = (Math.PI * 2) / metrics.length;
      const angleOffset = -Math.PI / 2; // Rotate so first axis is at top
      return metricIndex * step + angleOffset;
    },
    [metrics.length]
  );

  // Get x,y position for a metric at a given value
  const getPointPosition = useCallback(
    (metricIndex: number, value: number) => {
      const angle = getAngle(metricIndex);
      const r = yScale(value);
      return {
        x: r * Math.cos(angle),
        y: r * Math.sin(angle),
      };
    },
    [getAngle, yScale]
  );

  // Get color for a data index
  const getColor = useCallback(
    (index: number) => {
      const item = data[index];
      if (item?.color) {
        return item.color;
      }
      return defaultRadarColors[index % defaultRadarColors.length] as string;
    },
    [data]
  );

  // Early return if dimensions not ready
  if (size < 10) {
    return null;
  }

  const contextValue: RadarContextValue = {
    data,
    metrics,
    size,
    radius,
    levels,
    hoveredIndex,
    setHoveredIndex,
    animate,
    enterDurationMs,
    staggerScale,
    enterTransition,
    motionReplayKey,
    getColor,
    getAngle,
    getPointPosition,
    yScale,
  };

  return (
    <RadarProvider value={contextValue}>
      <svg
        aria-hidden="true"
        height={size}
        style={{ overflow: "visible" }}
        width={size}
      >
        <Group left={size / 2} top={size / 2}>
          {children}
        </Group>
      </svg>
    </RadarProvider>
  );
}

export function RadarChart({
  data,
  metrics,
  size: fixedSize,
  levels = 5,
  margin = 60,
  animate = true,
  enterDurationMs = 1100,
  staggerScale = 1,
  enterTransition,
  motionReplayKey = "",
  className = "",
  hoveredIndex,
  onHoverChange,
  children,
}: RadarChartProps) {
  // If fixed size is provided, use it directly
  if (fixedSize) {
    return (
      <div
        className={cn("relative flex items-center justify-center", className)}
        style={{ width: fixedSize, height: fixedSize }}
      >
        <RadarChartInner
          animate={animate}
          data={data}
          enterDurationMs={enterDurationMs}
          enterTransition={enterTransition}
          height={fixedSize}
          hoveredIndexProp={hoveredIndex}
          levels={levels}
          margin={margin}
          metrics={metrics}
          motionReplayKey={motionReplayKey}
          onHoverChange={onHoverChange}
          staggerScale={staggerScale}
          width={fixedSize}
        >
          {children}
        </RadarChartInner>
      </div>
    );
  }

  // Otherwise use ParentSize for responsive sizing
  return (
    <div className={cn("relative aspect-square w-full", className)}>
      <ParentSize debounceTime={10}>
        {({ width, height }) => (
          <RadarChartInner
            animate={animate}
            data={data}
            enterDurationMs={enterDurationMs}
            enterTransition={enterTransition}
            height={height}
            hoveredIndexProp={hoveredIndex}
            levels={levels}
            margin={margin}
            metrics={metrics}
            motionReplayKey={motionReplayKey}
            onHoverChange={onHoverChange}
            staggerScale={staggerScale}
            width={width}
          >
            {children}
          </RadarChartInner>
        )}
      </ParentSize>
    </div>
  );
}

export default RadarChart;
```

##### File: `src/charts/radar-context.tsx`

```tsx
"use client";

import type { Transition } from "motion/react";
import { createContext, useContext } from "react";

// CSS variable references for radar chart theming
export const radarCssVars = {
  background: "var(--chart-background)",
  foreground: "var(--chart-foreground)",
  foregroundMuted: "var(--chart-foreground-muted)",
  label: "var(--chart-label, oklch(0.65 0.01 260))",
  grid: "var(--chart-grid)",
  border: "var(--border)",
  // Default radar colors from chart palette
  area1: "var(--chart-1)",
  area2: "var(--chart-2)",
  area3: "var(--chart-3)",
  area4: "var(--chart-4)",
  area5: "var(--chart-5)",
};

// Default radar color palette
export const defaultRadarColors = [
  radarCssVars.area1,
  radarCssVars.area2,
  radarCssVars.area3,
  radarCssVars.area4,
  radarCssVars.area5,
];

export interface RadarMetric {
  /** Unique key for the metric */
  key: string;
  /** Display label for the metric */
  label: string;
}

export interface RadarData {
  /** Display label for this data series */
  label: string;
  /** Color for this data series (defaults to chart-1 through chart-5) */
  color?: string;
  /** Metric values (key -> value, normalized 0-100) */
  values: Record<string, number>;
}

export interface RadarContextValue {
  // Data
  data: RadarData[];
  metrics: RadarMetric[];

  // Dimensions
  size: number;
  radius: number;
  levels: number;

  // Hover state
  hoveredIndex: number | null;
  setHoveredIndex: (index: number | null) => void;

  // Animation
  animate: boolean;
  /** Total enter animation budget in ms */
  enterDurationMs: number;
  /** Scales stagger delays between grid / campaigns / metrics */
  staggerScale: number;
  /** Motion enter transition (spring or cubic-bezier tween). */
  enterTransition?: Transition;
  /** Changes when motion settings change — replays enter animations. */
  motionReplayKey: string;

  // Computed helpers
  getColor: (index: number) => string;
  getAngle: (metricIndex: number) => number;
  getPointPosition: (
    metricIndex: number,
    value: number
  ) => { x: number; y: number };
  yScale: (value: number) => number;
}

const RadarContext = createContext<RadarContextValue | null>(null);

export function RadarProvider({
  children,
  value,
}: {
  children: React.ReactNode;
  value: RadarContextValue;
}) {
  return (
    <RadarContext.Provider value={value}>{children}</RadarContext.Provider>
  );
}

export function useRadar(): RadarContextValue {
  const context = useContext(RadarContext);
  if (!context) {
    throw new Error(
      "useRadar must be used within a RadarProvider. " +
        "Make sure your component is wrapped in <RadarChart>."
    );
  }
  return context;
}

export default RadarContext;
```

##### File: `src/charts/radar-area.tsx`

```tsx
"use client";

import type { MotionValue } from "motion/react";
import { motion, useTransform } from "motion/react";
import { useMemo } from "react";
import { radarCssVars, useRadar } from "./radar-context";
import { useMountProgress } from "./use-mount-progress";

export interface RadarAreaProps {
  /** Index of this area in the data array */
  index: number;
  /** Optional color override */
  color?: string;
  /** Show data point circles. Default: true */
  showPoints?: boolean;
  /** Show stroke outline on the polygon. Default: true */
  showStroke?: boolean;
  /** Show glow effect on hover. Default: true */
  showGlow?: boolean;
  /** Additional class name */
  className?: string;
}

function getStrokeWidth(isHovered: boolean): number {
  return isHovered ? 3 : 2;
}

function RadarPoint({
  mountProgress,
  target,
  color,
  isHovered,
  metricKey,
}: {
  mountProgress: MotionValue<number>;
  target: { x: number; y: number };
  color: string;
  isHovered: boolean;
  metricKey: string;
}) {
  const cx = useTransform(mountProgress, (t) => target.x * t);
  const cy = useTransform(mountProgress, (t) => target.y * t);

  return (
    <motion.circle
      cx={cx}
      cy={cy}
      fill={color}
      key={metricKey}
      r={isHovered ? 6 : 4}
      stroke={radarCssVars.background}
      strokeWidth={2}
      transition={{
        r: { type: "spring", stiffness: 300, damping: 20 },
      }}
    />
  );
}

export function RadarArea({
  index,
  color: colorProp,
  showPoints = true,
  showStroke = true,
  showGlow = true,
  className = "",
}: RadarAreaProps) {
  const {
    data,
    metrics,
    levels,
    hoveredIndex,
    setHoveredIndex,
    animate,
    enterDurationMs,
    staggerScale,
    enterTransition,
    motionReplayKey,
    getColor,
    getPointPosition,
  } = useRadar();

  const durationFactor = enterDurationMs / 1100;
  const areaData = data[index];

  const targetPositions = useMemo(() => {
    if (!areaData) {
      return metrics.map(() => ({ x: 0, y: 0 }));
    }
    return metrics.map((metric, i) => {
      const value = areaData.values[metric.key] ?? 0;
      return getPointPosition(i, value);
    });
  }, [metrics, areaData, getPointPosition]);

  const gridStagger = 0.08 * staggerScale * durationFactor;
  const campaignBaseDelay = (levels * gridStagger + 0.2) * durationFactor;
  const campaignStagger = 0.15 * staggerScale * durationFactor;
  const animationDelay = campaignBaseDelay + index * campaignStagger;

  const mountProgress = useMountProgress(
    enterTransition,
    animationDelay,
    `${motionReplayKey}-${index}`
  );

  const animatedPositions = useTransform(mountProgress, (t) =>
    targetPositions.map((p) => ({ x: p.x * t, y: p.y * t }))
  );

  const pathD = useTransform(animatedPositions, (positions) => {
    if (positions.length === 0) {
      return "";
    }
    return `M ${positions.map((p) => `${p.x},${p.y}`).join(" L ")} Z`;
  });

  if (!areaData) {
    return null;
  }

  const color = colorProp || getColor(index);
  const isHovered = hoveredIndex === index;
  const isOtherHovered = hoveredIndex !== null && hoveredIndex !== index;

  return (
    <motion.g
      animate={{
        opacity: isOtherHovered ? 0.3 : 1,
        scale: isHovered ? 1.05 : 1,
      }}
      className={className}
      initial={{ opacity: animate ? 0 : 1 }}
      onMouseEnter={() => setHoveredIndex(index)}
      onMouseLeave={() => setHoveredIndex(null)}
      style={{ transformOrigin: "0px 0px", cursor: "pointer" }}
      transition={{
        opacity: { duration: 0.15 },
        scale: { type: "spring", stiffness: 400, damping: 25 },
      }}
    >
      <motion.path
        animate={{
          fillOpacity: isHovered ? 0.35 : 0.15,
          strokeWidth: showStroke ? getStrokeWidth(isHovered) : 0,
        }}
        d={pathD}
        fill={color}
        stroke={showStroke ? color : "none"}
        strokeLinejoin="round"
        style={{
          filter:
            showGlow && isHovered ? `drop-shadow(0 0 12px ${color})` : "none",
        }}
        transition={{
          fillOpacity: { duration: 0.2 },
          strokeWidth: { duration: 0.2 },
        }}
      />

      {showPoints &&
        metrics.map((metric, i) => {
          const target = targetPositions[i];
          if (!target) {
            return null;
          }
          return (
            <RadarPoint
              color={color}
              isHovered={isHovered}
              key={metric.key}
              metricKey={metric.key}
              mountProgress={mountProgress}
              target={target}
            />
          );
        })}
    </motion.g>
  );
}

RadarArea.displayName = "RadarArea";

export default RadarArea;
```

##### File: `src/charts/radar-axis.tsx`

```tsx
"use client";

import { motion } from "motion/react";
import { radarCssVars, useRadar } from "./radar-context";

export interface RadarAxisProps {
  /** Additional class name */
  className?: string;
}

export function RadarAxis({ className = "" }: RadarAxisProps) {
  const { metrics, radius, getAngle, animate } = useRadar();

  // Animation delay base
  const axisBaseDelay = 0;

  return (
    <g className={className}>
      {metrics.map((metric, i) => {
        const angle = getAngle(i);
        const targetX = radius * Math.cos(angle);
        const targetY = radius * Math.sin(angle);

        return (
          <motion.line
            animate={{ x2: targetX, y2: targetY }}
            initial={animate ? { x2: 0, y2: 0 } : { x2: targetX, y2: targetY }}
            key={`axis-${metric.key}`}
            stroke={radarCssVars.border}
            strokeOpacity={0.6}
            strokeWidth={1}
            transition={{
              type: "spring",
              stiffness: 80,
              damping: 15,
              mass: 1,
              delay: animate ? axisBaseDelay + i * 0.05 : 0,
            }}
            x1={0}
            y1={0}
          />
        );
      })}
    </g>
  );
}

RadarAxis.displayName = "RadarAxis";

export default RadarAxis;
```

##### File: `src/charts/radar-grid.tsx`

```tsx
"use client";

import { scaleLinear } from "@visx/scale";
import { LineRadial } from "@visx/shape";
import { motion } from "motion/react";
import { transitionWithDelay } from "./motion-utils";
import { radarCssVars, useRadar } from "./radar-context";

export interface RadarGridProps {
  /** Show level value labels. Default: true */
  showLabels?: boolean;
  /** Additional class name */
  className?: string;
}

export function RadarGrid({
  showLabels = true,
  className = "",
}: RadarGridProps) {
  const {
    metrics,
    radius,
    levels,
    animate,
    enterTransition,
    staggerScale,
    enterDurationMs,
    motionReplayKey,
  } = useRadar();

  const durationFactor = enterDurationMs / 1100;
  const gridStagger = 0.08 * staggerScale * durationFactor;

  // Generate angles for the radial lines (one per metric)
  const degrees = 360;
  const angles = [...new Array(metrics.length + 1)].map((_, i) => ({
    angle: i * (degrees / metrics.length) + degrees / metrics.length / 2,
  }));

  // Radial scale for converting degrees to radians
  const radialScale = scaleLinear<number>({
    range: [0, Math.PI * 2],
    domain: [degrees, 0],
  });

  const labelDelay = levels * gridStagger * 0.5;

  return (
    <g className={className}>
      {/* Concentric grid circles */}
      {[...new Array(levels)].map((_, i) => {
        const targetRadius = ((i + 1) * radius) / levels;
        return (
          <motion.g
            animate={{ scale: 1, opacity: 1 }}
            initial={
              animate ? { scale: 0, opacity: 0 } : { scale: 1, opacity: 1 }
            }
            // biome-ignore lint/suspicious/noArrayIndexKey: Static grid levels
            key={`grid-${i}-${motionReplayKey}`}
            style={{ transformOrigin: "0px 0px" }}
            transition={
              animate
                ? transitionWithDelay(enterTransition, i * gridStagger, {
                    type: "spring",
                    stiffness: 100,
                    damping: 15,
                    mass: 1,
                  })
                : undefined
            }
          >
            <LineRadial
              angle={(d) => radialScale(d.angle) ?? 0}
              data={angles}
              fill="none"
              radius={targetRadius}
              stroke={radarCssVars.border}
              strokeLinecap="round"
              strokeOpacity={0.6}
              strokeWidth={1}
            />
          </motion.g>
        );
      })}

      {/* Grid level labels */}
      {showLabels &&
        [...new Array(levels)].map((_, i) => (
          <motion.g
            animate={{ opacity: 1 }}
            initial={animate ? { opacity: 0 } : { opacity: 1 }}
            // biome-ignore lint/suspicious/noArrayIndexKey: Static grid levels
            key={`level-label-${i}-${motionReplayKey}`}
            transition={
              animate
                ? transitionWithDelay(
                    enterTransition,
                    labelDelay + i * 0.06 * durationFactor
                  )
                : undefined
            }
          >
            <text
              dominantBaseline="middle"
              fill={radarCssVars.foregroundMuted}
              fontSize={9}
              textAnchor="start"
              x={4}
              y={-((i + 1) * radius) / levels}
            >
              {((i + 1) * 100) / levels}
            </text>
          </motion.g>
        ))}
    </g>
  );
}

RadarGrid.displayName = "RadarGrid";

export default RadarGrid;
```

##### File: `src/charts/radar-labels.tsx`

```tsx
"use client";

import { motion } from "motion/react";
import { radarCssVars, useRadar } from "./radar-context";

export interface RadarLabelsProps {
  /** Distance from the chart edge. Default: 24 */
  offset?: number;
  /** Font size for labels. Default: 11 */
  fontSize?: number;
  /** Enable interactive hover on labels. Default: false */
  interactive?: boolean;
  /** Additional class name */
  className?: string;
}

export function RadarLabels({
  offset = 24,
  fontSize = 11,
  interactive = false,
  className = "",
}: RadarLabelsProps) {
  const { metrics, radius, levels, getAngle, animate } = useRadar();

  // Label animation delay (starts after grid begins)
  const gridStagger = 0.08;
  const labelDelay = levels * gridStagger * 0.5;

  const labelRadius = radius + offset;

  return (
    <g className={className}>
      {metrics.map((metric, i) => {
        const angle = getAngle(i);
        const x = labelRadius * Math.cos(angle);
        const y = labelRadius * Math.sin(angle);

        return (
          <motion.g
            animate={{ opacity: 1, x, y }}
            initial={
              animate ? { opacity: 0, x: 0, y: 0 } : { opacity: 1, x, y }
            }
            key={`label-${metric.key}`}
            transition={{
              opacity: {
                duration: 0.5,
                delay: animate ? labelDelay + i * 0.08 : 0,
              },
              x: { type: "spring", stiffness: 80, damping: 15 },
              y: { type: "spring", stiffness: 80, damping: 15 },
            }}
          >
            <text
              className={
                interactive
                  ? "cursor-pointer transition-opacity duration-150 hover:opacity-100"
                  : ""
              }
              dominantBaseline="middle"
              fontSize={fontSize}
              fontWeight={500}
              opacity={interactive ? 0.8 : 1}
              style={{ fill: radarCssVars.label }}
              textAnchor="middle"
              x={0}
              y={0}
            >
              {metric.label}
            </text>
          </motion.g>
        );
      })}
    </g>
  );
}

RadarLabels.displayName = "RadarLabels";

export default RadarLabels;
```

---

### Ring Chart <a name="ring-chart"></a>

A composable multi-ring progress chart with animated arcs, hover interactions, and a reusable legend component

- **Category**: Charts
- **Registry Key**: `ring-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/ring-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `@visx/group@4.0.1-alpha.0`
  - `@visx/responsive@4.0.1-alpha.0`
  - `@visx/shape@4.0.1-alpha.0`
  - `@number-flow/react`
  - `motion`
- **Local Registry Dependencies**:
  - `@bklit/chart-animation`
  - `@bklit/utils`

#### How to Use

```tsx
import { RingChart, Ring, RingCenter } from "@bklitui/ui/charts";

const data = [
  { label: "Organic", value: 4250, maxValue: 5000, color: "#0ea5e9" },
  { label: "Paid", value: 3120, maxValue: 5000, color: "#a855f7" },
  { label: "Email", value: 2100, maxValue: 5000, color: "#f59e0b" },
];

export default function SessionsChart() {
  return (
    <RingChart data={data} size={300}>
      {data.map((item, index) => (
        <Ring key={item.label} index={index} />
      ))}
      <RingCenter defaultLabel="Total Sessions" />
    </RingChart>
  );
}
```

#### Props & API Reference

##### `RingChart` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `data` | `RingData[]` | required | Array of ring data items |
| `size` | `number` | auto | Fixed size in pixels (uses parent if not set) |
| `strokeWidth` | `number` | `12` | Width of each ring |
| `ringGap` | `number` | `6` | Gap between rings |
| `baseInnerRadius` | `number` | `60` | Inner radius of the innermost ring |
| `hoveredIndex` | `number \| null` | - | Controlled hover state |
| `onHoverChange` | `(index: number \| null) => void` | - | Hover state callback |
| `className` | `string` | `""` | Additional CSS class |

##### `Ring` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `index` | `number` | required | Index of the ring in the data array |
| `color` | `string` | from data/palette | Optional color override |
| `animate` | `boolean` | `true` | Enable animation on mount |
| `showGlow` | `boolean` | `true` | Show glow effect on hover |
| `lineCap` | `"round" \| "butt"` | `"round"` | Line cap style for ring ends |

##### `RingCenter` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `defaultLabel` | `string` | `"Total"` | Label shown when not hovering |
| `formatValue` | `(value: number) => string` | `toLocaleString()` | Format function for values |
| `children` | `function` | - | Custom render function |
| `className` | `string` | `""` | Additional CSS class |

#### Component Source Code

##### File: `src/charts/ring-chart.tsx`

```tsx
"use client";

import { Group } from "@visx/group";
import { ParentSize } from "@visx/responsive";
import type { Transition } from "motion/react";
import {
  Children,
  isValidElement,
  memo,
  type ReactNode,
  useCallback,
  useEffect,
  useMemo,
  useRef,
  useState,
} from "react";
import { cn } from "@/lib/utils";
import {
  defaultRingColors,
  type RingContextValue,
  type RingData,
  RingProvider,
} from "./ring-context";

export interface RingChartProps {
  /** Data array - each item represents a ring */
  data: RingData[];
  /** Chart size in pixels. If not provided, uses parent container size */
  size?: number;
  /** Stroke width of each ring. Default: 12 */
  strokeWidth?: number;
  /** Gap between rings. Default: 6 */
  ringGap?: number;
  /** Inner radius of the innermost ring. Default: 60 */
  baseInnerRadius?: number;
  /** Animation duration in milliseconds. Default: 1100 */
  animationDuration?: number;
  /** Additional class name for the container */
  className?: string;
  /** Controlled hover state - index of hovered ring */
  hoveredIndex?: number | null;
  /** Callback when hover state changes */
  onHoverChange?: (index: number | null) => void;
  /** Start angle in radians. Default: -PI/2 (top) */
  startAngle?: number;
  /** End angle in radians. Default: 3*PI/2 (full circle) */
  endAngle?: number;
  /** Framer Motion transition for ring enter animation */
  enterTransition?: Transition;
  /** Scales ring stagger delays (1 = default). */
  enterStaggerScale?: number;
  /** Child components (Ring, RingCenter, etc.) */
  children: ReactNode;
}

interface RingChartInnerProps {
  width: number;
  height: number;
  data: RingData[];
  strokeWidth: number;
  ringGap: number;
  baseInnerRadius: number;
  children: ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
  hoveredIndexProp?: number | null;
  onHoverChange?: (index: number | null) => void;
  startAngle: number;
  endAngle: number;
  enterTransition?: Transition;
  enterStaggerScale: number;
}

// Helper to check if a child is a RingCenter component
function isRingCenter(child: ReactNode): boolean {
  return (
    isValidElement(child) &&
    typeof child.type === "function" &&
    ((child.type as { displayName?: string }).displayName === "RingCenter" ||
      child.type.name === "RingCenter")
  );
}

function RingChartInner(props: RingChartInnerProps) {
  const size = Math.min(props.width, props.height);

  if (size < 10) {
    return null;
  }

  return <RingChartCore {...props} />;
}

const RingChartCore = memo(function RingChartCore({
  width,
  height,
  data,
  strokeWidth: strokeWidthProp,
  ringGap: ringGapProp,
  baseInnerRadius: baseInnerRadiusProp,
  children,
  containerRef,
  hoveredIndexProp,
  onHoverChange,
  startAngle,
  endAngle,
  enterTransition,
  enterStaggerScale,
}: RingChartInnerProps) {
  const [internalHoveredIndex, setInternalHoveredIndex] = useState<
    number | null
  >(null);
  const [animationKey] = useState(0);
  const [isLoaded, setIsLoaded] = useState(false);

  // Use controlled or uncontrolled hover state
  const isControlled = hoveredIndexProp !== undefined;
  const hoveredIndex = isControlled ? hoveredIndexProp : internalHoveredIndex;
  const setHoveredIndex = useCallback(
    (index: number | null) => {
      if (isControlled) {
        onHoverChange?.(index);
      } else {
        setInternalHoveredIndex(index);
      }
    },
    [isControlled, onHoverChange]
  );

  // Use the smaller dimension to ensure the chart fits
  const size = Math.min(width, height);
  const center = size / 2;

  // Calculate scaled dimensions to fit within the available space
  // The outermost ring needs to fit within the chart with some padding
  const ringCount = data.length;
  const padding = 8; // Padding from edge
  const availableRadius = center - padding;

  // Calculate the "design" outer radius (what we'd need at 1:1 scale)
  const designOuterRadius =
    baseInnerRadiusProp +
    (ringCount - 1) * (strokeWidthProp + ringGapProp) +
    strokeWidthProp;

  // Scale factor to fit within available space
  const scale = Math.min(1, availableRadius / designOuterRadius);

  // Apply scaling to all dimensions
  const strokeWidth = strokeWidthProp * scale;
  const ringGap = ringGapProp * scale;
  const baseInnerRadius = baseInnerRadiusProp * scale;

  // Calculate total value
  const totalValue = useMemo(
    () => data.reduce((sum, d) => sum + d.value, 0),
    [data]
  );

  // Get color for a ring index
  const getColor = useCallback(
    (index: number) => {
      const item = data[index];
      if (item?.color) {
        return item.color;
      }
      return defaultRingColors[index % defaultRingColors.length] as string;
    },
    [data]
  );

  // Get ring radii for an index
  const getRingRadii = useCallback(
    (index: number) => {
      const innerRadius = baseInnerRadius + index * (strokeWidth + ringGap);
      const outerRadius = innerRadius + strokeWidth;
      return { innerRadius, outerRadius };
    },
    [baseInnerRadius, strokeWidth, ringGap]
  );

  // biome-ignore lint/correctness/useExhaustiveDependencies: enterTransition
  useEffect(() => {
    setIsLoaded(false);
    const timer = setTimeout(() => {
      setIsLoaded(true);
    }, 100);
    return () => clearTimeout(timer);
  }, [enterTransition, enterStaggerScale]);

  // Separate SVG children (rings) from HTML children (RingCenter)
  // This avoids Safari's foreignObject positioning bugs (WebKit #23113)
  const { svgChildren, centerChildren } = useMemo(() => {
    const svgNodes: ReactNode[] = [];
    const centerNodes: ReactNode[] = [];

    Children.forEach(children, (child) => {
      if (isRingCenter(child)) {
        centerNodes.push(child);
      } else {
        svgNodes.push(child);
      }
    });

    return { svgChildren: svgNodes, centerChildren: centerNodes };
  }, [children]);

  const contextValue: RingContextValue = {
    data,
    size,
    center,
    strokeWidth,
    ringGap,
    baseInnerRadius,
    hoveredIndex,
    setHoveredIndex,
    animationKey,
    isLoaded,
    enterTransition,
    enterStaggerScale,
    containerRef,
    totalValue,
    getColor,
    getRingRadii,
    startAngle,
    endAngle,
  };

  // Use CSS Grid stacking to layer SVG and HTML content
  // This avoids Safari's foreignObject rendering bugs where HTML content
  // inside SVG foreignObject renders at wrong positions when it has a RenderLayer
  return (
    <RingProvider value={contextValue}>
      <div
        className="grid"
        style={{
          gridTemplateColumns: "1fr",
          gridTemplateRows: "1fr",
          width: size,
          height: size,
        }}
      >
        {/* SVG layer with rings */}
        <svg
          aria-hidden="true"
          height={size}
          style={{ gridArea: "1 / 1" }}
          width={size}
        >
          <Group left={center} top={center}>
            {svgChildren}
          </Group>
        </svg>

        {/* HTML layer with center content - stacked on top via grid */}
        {centerChildren.length > 0 && (
          <div
            className="pointer-events-none flex items-center justify-center"
            style={{ gridArea: "1 / 1" }}
          >
            {centerChildren}
          </div>
        )}
      </div>
    </RingProvider>
  );
});

export function RingChart({
  data,
  size: fixedSize,
  strokeWidth = 12,
  ringGap = 6,
  baseInnerRadius = 60,
  className = "",
  hoveredIndex,
  onHoverChange,
  startAngle = -Math.PI / 2,
  endAngle = (3 * Math.PI) / 2,
  enterTransition,
  enterStaggerScale = 1,
  children,
}: RingChartProps) {
  const containerRef = useRef<HTMLDivElement>(null);

  // If fixed size is provided, use it directly
  if (fixedSize) {
    return (
      <div
        className={cn("relative flex items-center justify-center", className)}
        ref={containerRef}
        style={{ width: fixedSize, height: fixedSize }}
      >
        <RingChartInner
          baseInnerRadius={baseInnerRadius}
          containerRef={containerRef}
          data={data}
          endAngle={endAngle}
          enterStaggerScale={enterStaggerScale}
          enterTransition={enterTransition}
          height={fixedSize}
          hoveredIndexProp={hoveredIndex}
          onHoverChange={onHoverChange}
          ringGap={ringGap}
          startAngle={startAngle}
          strokeWidth={strokeWidth}
          width={fixedSize}
        >
          {children}
        </RingChartInner>
      </div>
    );
  }

  // Otherwise use ParentSize for responsive sizing
  return (
    <div
      className={cn("relative aspect-square w-full", className)}
      ref={containerRef}
    >
      <ParentSize debounceTime={10}>
        {({ width, height }) => (
          <RingChartInner
            baseInnerRadius={baseInnerRadius}
            containerRef={containerRef}
            data={data}
            endAngle={endAngle}
            enterStaggerScale={enterStaggerScale}
            enterTransition={enterTransition}
            height={height}
            hoveredIndexProp={hoveredIndex}
            onHoverChange={onHoverChange}
            ringGap={ringGap}
            startAngle={startAngle}
            strokeWidth={strokeWidth}
            width={width}
          >
            {children}
          </RingChartInner>
        )}
      </ParentSize>
    </div>
  );
}

export default RingChart;
```

##### File: `src/charts/ring-context.tsx`

```tsx
"use client";

import type { Transition } from "motion/react";
import { createContext, type RefObject, useContext } from "react";

// CSS variable references for ring chart theming
export const ringCssVars = {
  background: "var(--chart-background)",
  foreground: "var(--chart-foreground)",
  foregroundMuted: "var(--chart-foreground-muted)",
  label: "var(--chart-label)",
  ringBackground: "var(--chart-ring-background)",
  // Default ring colors from chart palette
  ring1: "var(--chart-1)",
  ring2: "var(--chart-2)",
  ring3: "var(--chart-3)",
  ring4: "var(--chart-4)",
  ring5: "var(--chart-5)",
};

// Default ring color palette
export const defaultRingColors = [
  ringCssVars.ring1,
  ringCssVars.ring2,
  ringCssVars.ring3,
  ringCssVars.ring4,
  ringCssVars.ring5,
];

export interface RingData {
  /** Display label for the ring */
  label: string;
  /** Current value */
  value: number;
  /** Maximum value (determines progress percentage) */
  maxValue: number;
  /** Optional color override - falls back to palette */
  color?: string;
}

export interface RingContextValue {
  // Data
  data: RingData[];

  // Dimensions
  size: number;
  center: number;
  strokeWidth: number;
  ringGap: number;
  baseInnerRadius: number;

  // Hover state
  hoveredIndex: number | null;
  setHoveredIndex: (index: number | null) => void;

  // Animation state
  animationKey: number;
  isLoaded: boolean;
  enterTransition?: Transition;
  enterStaggerScale: number;

  // Container ref for portals
  containerRef: RefObject<HTMLDivElement | null>;

  // Computed values
  totalValue: number;

  // Get color for a ring index
  getColor: (index: number) => string;

  // Get ring radii for an index
  getRingRadii: (index: number) => { innerRadius: number; outerRadius: number };

  // Arc angle range
  startAngle: number;
  endAngle: number;
}

const RingContext = createContext<RingContextValue | null>(null);

export function RingProvider({
  children,
  value,
}: {
  children: React.ReactNode;
  value: RingContextValue;
}) {
  return <RingContext.Provider value={value}>{children}</RingContext.Provider>;
}

export function useRing(): RingContextValue {
  const context = useContext(RingContext);
  if (!context) {
    throw new Error(
      "useRing must be used within a RingProvider. " +
        "Make sure your component is wrapped in <RingChart>."
    );
  }
  return context;
}

export default RingContext;
```

##### File: `src/charts/ring.tsx`

```tsx
"use client";

import { Arc, arc as arcGenerator } from "@visx/shape";
import { motion, useTransform } from "motion/react";
import { useEffect, useRef } from "react";
import { ringCssVars, useRing } from "./ring-context";
import { useMountProgress } from "./use-mount-progress";

// Helper to generate arc path using d3 arc generator
function generateArcPath(
  innerRadius: number,
  outerRadius: number,
  startAngle: number,
  endAngle: number,
  cornerRadius: number
): string {
  const generator = arcGenerator<unknown>({
    innerRadius,
    outerRadius,
    cornerRadius,
  });
  return generator({ startAngle, endAngle } as unknown as null) || "";
}

export type RingLineCap = "round" | "butt";

export interface RingProps {
  /** Index of the ring in the data array */
  index: number;
  /** Optional color override - falls back to data color or palette */
  color?: string;
  /** Animate the progress arc. Default: true */
  animate?: boolean;
  /** Show glow effect on hover. Default: true */
  showGlow?: boolean;
  /** Line cap style for ring ends. Default: "round" */
  lineCap?: RingLineCap;
}

interface AnimatedProgressArcProps {
  index: number;
  innerRadius: number;
  outerRadius: number;
  progress: number;
  color: string;
  isHovered: boolean;
  isFaded: boolean;
  isPushedOut: boolean;
  animationKey: number;
  showGlow: boolean;
  lineCap: RingLineCap;
  startAngle: number;
  arcRange: number;
}

function AnimatedProgressArc({
  index,
  innerRadius,
  outerRadius,
  progress,
  color,
  isHovered,
  isFaded,
  isPushedOut,
  animationKey,
  showGlow,
  lineCap,
  startAngle,
  arcRange,
}: AnimatedProgressArcProps) {
  const {
    enterTransition,
    enterStaggerScale,
    animationKey: ringAnimationKey,
  } = useRing();
  const targetEndAngle = startAngle + arcRange * progress;
  const cornerRadius =
    lineCap === "round" ? (outerRadius - innerRadius) / 2 : 0;

  // Progress arc delay - starts after background rings expand
  const progressDelay = (0.6 + index * 0.1) * enterStaggerScale;
  const mountProgress = useMountProgress(
    enterTransition,
    progressDelay,
    ringAnimationKey
  );

  const animatedPath = useTransform(mountProgress, (v) => {
    const currentEndAngle = startAngle + (targetEndAngle - startAngle) * v;
    if (currentEndAngle <= startAngle + 0.01) {
      return "";
    }
    return generateArcPath(
      innerRadius,
      outerRadius,
      startAngle,
      currentEndAngle,
      cornerRadius
    );
  });

  // Calculate scale: hovered ring scales up, outer rings pushed out
  const getScale = () => {
    if (isHovered) {
      return 1.03;
    }
    if (isPushedOut) {
      return 1.02;
    }
    return 1;
  };

  return (
    <motion.path
      animate={{
        opacity: isFaded ? 0.4 : 1,
        scale: getScale(),
      }}
      d={animatedPath}
      fill={color}
      key={`progress-${animationKey}`}
      style={{
        transformOrigin: "center",
        filter:
          showGlow && isHovered ? `drop-shadow(0 0 12px ${color})` : "none",
      }}
      transition={{
        opacity: { duration: 0.15 },
        scale: { type: "spring", stiffness: 400, damping: 25 },
      }}
    />
  );
}

export function Ring({
  index,
  color: colorProp,
  animate = true,
  showGlow = true,
  lineCap = "round",
}: RingProps) {
  const {
    data,
    hoveredIndex,
    setHoveredIndex,
    animationKey,
    enterStaggerScale,
    getColor,
    getRingRadii,
    startAngle: ctxStartAngle,
    endAngle: ctxEndAngle,
  } = useRing();

  const arcRange = ctxEndAngle - ctxStartAngle;

  const hasAnimated = useRef(false);
  const ringExpandDelay = index * 0.08 * enterStaggerScale;

  useEffect(() => {
    if (animate && !hasAnimated.current) {
      const timeout = setTimeout(
        () => {
          hasAnimated.current = true;
        },
        (ringExpandDelay + 0.3) * 1000
      );
      return () => clearTimeout(timeout);
    }
  }, [animate, ringExpandDelay]);

  const ringData = data[index];
  if (!ringData) {
    return null;
  }

  const { innerRadius, outerRadius } = getRingRadii(index);
  const color = colorProp || getColor(index);
  const progress = ringData.value / ringData.maxValue;

  const isHovered = hoveredIndex === index;
  const isFaded = hoveredIndex !== null && hoveredIndex !== index;
  // Ring is pushed out when a ring with lower index (inner ring) is hovered
  const isPushedOut = hoveredIndex !== null && hoveredIndex < index;

  // Only apply delay on initial mount, not on hover changes
  const shouldDelay = animate && !hasAnimated.current;

  // Calculate scale for background and progress arcs
  const getScale = () => {
    if (isHovered) {
      return 1.03;
    }
    if (isPushedOut) {
      return 1.02;
    }
    return 1;
  };

  return (
    // biome-ignore lint/a11y/noStaticElementInteractions: SVG group for hover interaction
    <g
      onMouseEnter={() => setHoveredIndex(index)}
      onMouseLeave={() => setHoveredIndex(null)}
      style={{ cursor: "pointer" }}
    >
      {/* Background track */}
      <Arc
        cornerRadius={lineCap === "round" ? (outerRadius - innerRadius) / 2 : 0}
        endAngle={ctxEndAngle}
        innerRadius={innerRadius}
        outerRadius={outerRadius}
        startAngle={ctxStartAngle}
      >
        {({ path }) => (
          <motion.path
            animate={{
              scale: animate ? getScale() : 1,
              opacity: isFaded ? 0.3 : 1,
            }}
            d={path(null) || ""}
            fill={ringCssVars.ringBackground}
            initial={animate ? { scale: 0 } : { scale: 1 }}
            key={`bg-${animationKey}`}
            style={{ transformOrigin: "center" }}
            transition={{
              scale: {
                type: "spring",
                stiffness: 400,
                damping: 25,
                delay: shouldDelay ? ringExpandDelay : 0,
              },
              opacity: { duration: 0.15 },
            }}
          />
        )}
      </Arc>

      {/* Animated Progress arc */}
      {animate ? (
        <AnimatedProgressArc
          animationKey={animationKey}
          arcRange={arcRange}
          color={color}
          index={index}
          innerRadius={innerRadius}
          isFaded={isFaded}
          isHovered={isHovered}
          isPushedOut={isPushedOut}
          lineCap={lineCap}
          outerRadius={outerRadius}
          progress={progress}
          showGlow={showGlow}
          startAngle={ctxStartAngle}
        />
      ) : (
        <motion.path
          animate={{
            opacity: isFaded ? 0.4 : 1,
            scale: getScale(),
          }}
          d={generateArcPath(
            innerRadius,
            outerRadius,
            ctxStartAngle,
            ctxStartAngle + arcRange * progress,
            lineCap === "round" ? (outerRadius - innerRadius) / 2 : 0
          )}
          fill={color}
          style={{
            transformOrigin: "center",
            filter:
              showGlow && isHovered ? `drop-shadow(0 0 12px ${color})` : "none",
          }}
          transition={{
            opacity: { duration: 0.15 },
            scale: { type: "spring", stiffness: 400, damping: 25 },
          }}
        />
      )}
    </g>
  );
}

Ring.displayName = "Ring";

export default Ring;
```

##### File: `src/charts/chart-stat-flow.tsx`

```tsx
"use client";

import NumberFlow from "@number-flow/react";
import type { ReactNode } from "react";
import { cn } from "@/lib/utils";

/** Subset of `Intl.NumberFormatOptions` supported by NumberFlow */
export interface ChartStatFlowFormat {
  notation?: "standard" | "compact";
  compactDisplay?: "short" | "long";
  minimumFractionDigits?: number;
  maximumFractionDigits?: number;
  minimumIntegerDigits?: number;
  minimumSignificantDigits?: number;
  maximumSignificantDigits?: number;
  style?: "decimal" | "percent" | "currency";
  currency?: string;
  currencyDisplay?: "symbol" | "narrowSymbol" | "code" | "name";
  unit?: string;
  unitDisplay?: "short" | "long" | "narrow";
}

export const defaultChartStatFlowFormat: ChartStatFlowFormat = {
  notation: "standard",
  maximumFractionDigits: 0,
};

export interface ChartStatFlowProps {
  value: number;
  label: string;
  formatOptions?: ChartStatFlowFormat;
  prefix?: string;
  suffix?: string;
  valueClassName?: string;
  labelClassName?: string;
  icon?: ReactNode;
}

/**
 * Shared value + label stack using NumberFlow (same layout as pie / ring centers).
 * Parent should provide flex alignment and sizing when needed.
 */
export function ChartStatFlow({
  value,
  label,
  formatOptions = defaultChartStatFlowFormat,
  prefix,
  suffix,
  valueClassName = "text-2xl font-bold",
  labelClassName = "text-xs",
  icon,
}: ChartStatFlowProps) {
  return (
    <>
      {icon ? (
        <div className="mb-2 flex h-12 w-12 items-center justify-center rounded-full bg-muted/50">
          {icon}
        </div>
      ) : null}
      <span className={cn("text-foreground tabular-nums", valueClassName)}>
        <NumberFlow
          format={formatOptions}
          prefix={prefix}
          suffix={suffix}
          value={value}
          willChange
        />
      </span>
      <span className={cn("mt-0.5 text-chart-label", labelClassName)}>
        {label}
      </span>
    </>
  );
}

ChartStatFlow.displayName = "ChartStatFlow";
```

##### File: `src/charts/ring-center.tsx`

```tsx
"use client";

import type { ReactNode } from "react";
import { cn } from "@/lib/utils";
import {
  chartCenterContainerClassName,
  chartCenterLabelClassName,
  chartCenterValueClassName,
} from "./chart-center-typography";
import {
  ChartStatFlow,
  type ChartStatFlowFormat,
  defaultChartStatFlowFormat,
} from "./chart-stat-flow";
import { useRing } from "./ring-context";

export interface RingCenterProps {
  /** Label shown below the value. Default: "Total" when not hovering */
  defaultLabel?: string;
  /** Format options for NumberFlow. Default: standard notation */
  formatOptions?: ChartStatFlowFormat;
  /** Custom render function for complete control over center content */
  children?: (props: {
    value: number;
    label: string;
    isHovered: boolean;
    data: { label: string; value: number; maxValue: number; color?: string };
  }) => ReactNode;
  /** Additional class name for the container */
  className?: string;
  /** Class name for the value text. Scales with center size via container queries. */
  valueClassName?: string;
  /** Class name for the label text. Scales with center size via container queries. */
  labelClassName?: string;
  /** Prefix to show before the number (e.g., "$") */
  prefix?: string;
  /** Suffix to show after the number (e.g., "%") */
  suffix?: string;
}

/**
 * RingCenter displays content in the center of the ring chart.
 *
 * This component renders as pure HTML (not inside SVG foreignObject) to avoid
 * Safari's WebKit bug #23113 where HTML content with CSS transforms/opacity
 * inside foreignObject renders at incorrect positions.
 *
 * The parent RingChart uses CSS Grid stacking to overlay this HTML content
 * on top of the SVG rings.
 */
export function RingCenter({
  defaultLabel = "Total",
  formatOptions = defaultChartStatFlowFormat,
  children,
  className = "",
  valueClassName = chartCenterValueClassName,
  labelClassName = chartCenterLabelClassName,
  prefix,
  suffix,
}: RingCenterProps) {
  const { data, hoveredIndex, totalValue, baseInnerRadius } = useRing();

  const hoveredData = hoveredIndex === null ? null : data[hoveredIndex];
  const displayValue = hoveredData ? hoveredData.value : totalValue;
  const displayLabel = hoveredData ? hoveredData.label : defaultLabel;

  // Calculate center area size based on scaled baseInnerRadius
  // Leave some padding so text doesn't touch the inner ring
  const centerSize = baseInnerRadius * 2 - 16;

  // If custom render function is provided, use it
  if (children && hoveredData) {
    return (
      <div
        className={cn(
          chartCenterContainerClassName,
          "flex items-center justify-center",
          className
        )}
        style={{ width: centerSize, height: centerSize }}
      >
        {children({
          value: displayValue,
          label: displayLabel,
          isHovered: hoveredIndex !== null,
          data: hoveredData,
        })}
      </div>
    );
  }

  // Default center content with NumberFlow animations
  // Now renders as pure HTML, avoiding Safari's foreignObject bugs
  return (
    <div
      className={cn(
        chartCenterContainerClassName,
        "flex flex-col items-center justify-center text-center",
        className
      )}
      style={{ width: centerSize, height: centerSize }}
    >
      <ChartStatFlow
        formatOptions={formatOptions}
        label={displayLabel}
        labelClassName={labelClassName}
        prefix={prefix}
        suffix={suffix}
        value={displayValue}
        valueClassName={valueClassName}
      />
    </div>
  );
}

RingCenter.displayName = "RingCenter";

export default RingCenter;
```

##### File: `src/charts/chart-center-typography.ts`

```tsx
/**
 * Fluid typography for pie / ring / gauge center labels.
 *
 * Uses CSS container query units (`cqw`) so values scale with the center
 * hole — not the viewport — which keeps stat text readable on small charts.
 */
export const chartCenterContainerClassName =
  "@container/chart-center size-full min-w-0";

/** Primary stat — ~22% of center width, clamped between text-sm and text-3xl. */
export const chartCenterValueClassName =
  "font-bold tabular-nums leading-none text-[clamp(0.75rem,22cqw,1.875rem)]";

/** Supporting label — ~9% of center width, clamped between 10px and text-xs. */
export const chartCenterLabelClassName =
  "max-w-full truncate leading-tight text-[clamp(0.625rem,9cqw,0.75rem)]";
```

---

### Sankey Chart <a name="sankey-chart"></a>

A composable sankey diagram for visualizing flow between nodes with animated links and interactive tooltips

- **Category**: Charts
- **Registry Key**: `sankey-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/sankey-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `@visx/event@4.0.1-alpha.0`
  - `@visx/gradient@4.0.1-alpha.0`
  - `@visx/pattern@4.0.1-alpha.0`
  - `@visx/responsive@4.0.1-alpha.0`
  - `@visx/sankey@4.0.1-alpha.0`
  - `d3-sankey`
  - `motion`
- **Local Registry Dependencies**:
  - `@bklit/chart-animation`
  - `@bklit/utils`
  - `@bklit/chart-utils`
  - `@bklit/chart-tooltip`

#### How to Use

```tsx
import { SankeyChart, SankeyNode, SankeyLink, SankeyTooltip } from "@bklitui/ui/charts";

const data = {
  nodes: [
    { name: "Organic Search", category: "source" },
    { name: "Homepage", category: "landing" },
    { name: "Converted", category: "outcome" },
  ],
  links: [
    { source: 0, target: 1, value: 100 },
    { source: 1, target: 2, value: 80 },
  ],
};

export default function FlowDiagram() {
  return (
    <SankeyChart data={data}>
      <SankeyLink />
      <SankeyNode lineCap={4} />
      <SankeyTooltip />
    </SankeyChart>
  );
}
```

#### Props & API Reference

##### `SankeyChart` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `data` | `SankeyData` | required | Object with `nodes` and `links` arrays |
| `margin` | `Partial<Margin>` | `{ top: 40, right: 180, bottom: 40, left: 180 }` | Chart margins |
| `animationDuration` | `number` | `1100` | Animation duration in ms |
| `aspectRatio` | `string` | `"2 / 1"` | CSS aspect ratio |
| `nodeWidth` | `number` | `16` | Width of nodes in pixels |
| `nodePadding` | `number` | `24` | Vertical padding between nodes |
| `className` | `string` | `""` | Additional CSS class |

##### `SankeyNode` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `fill` | `string` | - | Fill color for all nodes |
| `lineCap` | `number` | `4` | Corner radius for nodes |
| `fadedOpacity` | `number` | `0.4` | Opacity when another element is hovered |
| `showLabels` | `boolean` | `true` | Show node name and value labels |
| `getNodeColor` | `(node, index) => string` | - | Custom color function |

##### `SankeyLink` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `stroke` | `string` | - | Solid stroke color (overrides gradient) |
| `strokeOpacity` | `number` | `0.7` | Link opacity |
| `fadedOpacity` | `number` | `0.1` | Opacity when another element is hovered |
| `useGradient` | `boolean` | `true` | Use gradient from source to target node color |
| `getNodeColor` | `(node, index) => string` | - | Custom node color function for gradients |
| `getLinkColor` | `(link, index) => string` | - | Custom link color (overrides gradient) |
| `patterns` | `ReactNode` | - | Pattern definitions using `@visx/pattern` components |
| `getLinkPattern` | `(link, index) => string \| null` | - | Return pattern ID for a link, or null for gradient |

##### `SankeyTooltip` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `nodeContent` | `(props) => ReactNode` | - | Custom node tooltip renderer |
| `linkContent` | `(props) => ReactNode` | - | Custom link tooltip renderer |
| `formatValue` | `(value) => string` | `toLocaleString` | Value formatter |
| `className` | `string` | `""` | Additional CSS class |

#### Component Source Code

##### File: `src/charts/sankey/sankey-chart.tsx`

```tsx
"use client";

import { localPoint } from "@visx/event";
import { ParentSize } from "@visx/responsive";
import { sankey, sankeyCenter, sankeyLinkHorizontal } from "@visx/sankey";
import type { Transition } from "motion/react";
import {
  memo,
  type ReactNode,
  useCallback,
  useEffect,
  useMemo,
  useRef,
  useState,
} from "react";
import { cn } from "@/lib/utils";
import {
  type Margin,
  type SankeyLinkDatum,
  type SankeyNodeDatum,
  SankeyProvider,
  type SankeyTooltipData,
} from "./sankey-context";

export interface SankeyData {
  nodes: SankeyNodeDatum[];
  links: SankeyLinkDatum[];
}

export interface SankeyChartProps {
  /** Sankey data with nodes and links */
  data: SankeyData;
  /** Chart margins */
  margin?: Partial<Margin>;
  /** Animation duration in milliseconds. Default: 1100 */
  animationDuration?: number;
  /** Motion enter transition (spring or cubic-bezier tween). */
  enterTransition?: Transition;
  /** Signature of motion URL state — triggers enter replay when it changes. */
  revealSignature?: string;
  /** Aspect ratio as "width / height". Default: "2 / 1" */
  aspectRatio?: string;
  /** Node width in pixels. Default: 16 */
  nodeWidth?: number;
  /** Node padding in pixels. Default: 24 */
  nodePadding?: number;
  /** Additional class name for the container */
  className?: string;
  /** Child components (SankeyNode, SankeyLink, SankeyTooltip) */
  children: ReactNode;
}

const DEFAULT_MARGIN: Margin = { top: 40, right: 180, bottom: 40, left: 180 };

interface SankeyChartInnerProps {
  data: SankeyData;
  width: number;
  height: number;
  margin: Margin;
  animationDuration: number;
  enterTransition?: Transition;
  revealSignature?: string;
  nodeWidth: number;
  nodePadding: number;
  children: ReactNode;
}

function SankeyChartInner(props: SankeyChartInnerProps) {
  const { width, height } = props;

  if (width < 10 || height < 10) {
    return null;
  }

  return <SankeyChartCore {...props} />;
}

const SankeyChartCore = memo(function SankeyChartCore({
  data,
  width,
  height,
  margin,
  animationDuration,
  enterTransition,
  revealSignature = "",
  nodeWidth,
  nodePadding,
  children,
}: SankeyChartInnerProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const [isLoaded, setIsLoaded] = useState(false);
  const [revealEpoch, setRevealEpoch] = useState(0);
  const [hoveredNodeIndex, setHoveredNodeIndex] = useState<number | null>(null);
  const [hoveredLinkIndex, setHoveredLinkIndex] = useState<number | null>(null);
  const [tooltipData, setTooltipData] = useState<SankeyTooltipData | null>(
    null
  );
  const [mousePos, setMousePos] = useState<{ x: number; y: number } | null>(
    null
  );

  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  // biome-ignore lint/correctness/useExhaustiveDependencies: revealSignature
  useEffect(() => {
    setRevealEpoch((n) => n + 1);
    setIsLoaded(false);
    const timeout = setTimeout(() => {
      setIsLoaded(true);
    }, animationDuration);
    return () => clearTimeout(timeout);
  }, [animationDuration, revealSignature]);

  const sankeyGenerator = useMemo(() => {
    return sankey<SankeyNodeDatum, SankeyLinkDatum>()
      .nodeWidth(nodeWidth)
      .nodePadding(nodePadding)
      .nodeAlign(sankeyCenter)
      .extent([
        [0, 0],
        [innerWidth, innerHeight],
      ]);
  }, [innerWidth, innerHeight, nodeWidth, nodePadding]);

  const graph = useMemo(() => {
    const clonedData = {
      nodes: data.nodes.map((node) => ({ ...node })),
      links: data.links.map((link) => ({ ...link })),
    };
    return sankeyGenerator(clonedData);
  }, [data, sankeyGenerator]);

  const createPath = useCallback(
    // biome-ignore lint/suspicious/noExplicitAny: d3-sankey types are complex
    (link: any) => {
      try {
        const pathGenerator = sankeyLinkHorizontal();
        return pathGenerator(link) || "";
      } catch {
        return "";
      }
    },
    []
  );

  const handleMouseMove = useCallback((event: React.MouseEvent) => {
    const point = localPoint(event);
    if (point) {
      setMousePos({ x: point.x, y: point.y });
    }
  }, []);

  const handleMouseLeave = useCallback(() => {
    setHoveredNodeIndex(null);
    setHoveredLinkIndex(null);
    setTooltipData(null);
    setMousePos(null);
  }, []);

  const contextValue = {
    graph,
    nodes: graph.nodes,
    links: graph.links,
    width,
    height,
    innerWidth,
    innerHeight,
    margin,
    hoveredNodeIndex,
    hoveredLinkIndex,
    setHoveredNodeIndex,
    setHoveredLinkIndex,
    tooltipData,
    setTooltipData,
    containerRef,
    isLoaded,
    animationDuration,
    enterTransition,
    revealEpoch,
    mousePos,
    createPath,
  };

  return (
    <SankeyProvider value={contextValue}>
      <div className="relative h-full w-full" ref={containerRef}>
        <svg
          aria-hidden="true"
          height={height}
          onMouseLeave={handleMouseLeave}
          onMouseMove={handleMouseMove}
          width={width}
        >
          <g transform={`translate(${margin.left},${margin.top})`}>
            {children}
          </g>
        </svg>
      </div>
    </SankeyProvider>
  );
});

export function SankeyChart({
  data,
  margin: marginProp,
  animationDuration = 1100,
  enterTransition,
  revealSignature,
  aspectRatio = "2 / 1",
  nodeWidth = 16,
  nodePadding = 24,
  className = "",
  children,
}: SankeyChartProps) {
  const margin = { ...DEFAULT_MARGIN, ...marginProp };

  return (
    <div className={cn("relative w-full", className)} style={{ aspectRatio }}>
      <ParentSize>
        {({ width, height }) => (
          <SankeyChartInner
            animationDuration={animationDuration}
            data={data}
            enterTransition={enterTransition}
            height={height}
            margin={margin}
            nodePadding={nodePadding}
            nodeWidth={nodeWidth}
            revealSignature={revealSignature}
            width={width}
          >
            {children}
          </SankeyChartInner>
        )}
      </ParentSize>
    </div>
  );
}

SankeyChart.displayName = "SankeyChart";

export default SankeyChart;
```

##### File: `src/charts/sankey/sankey-context.tsx`

```tsx
"use client";

import type { SankeyGraph, SankeyLink, SankeyNode } from "d3-sankey";
import type { Transition } from "motion/react";
import {
  createContext,
  type Dispatch,
  type RefObject,
  type SetStateAction,
  useContext,
} from "react";

export interface Margin {
  top: number;
  right: number;
  bottom: number;
  left: number;
}

export interface SankeyNodeDatum {
  name: string;
  category?: "source" | "landing" | "outcome";
  [key: string]: unknown;
}

export interface SankeyLinkDatum {
  source: number;
  target: number;
  value: number;
  [key: string]: unknown;
}

export interface SankeyTooltipData {
  type: "node" | "link";
  nodeIndex?: number;
  linkIndex?: number;
  x: number;
  y: number;
  data: SankeyNodeDatum | SankeyLinkDatum;
}

export interface SankeyContextValue {
  // Layout data
  graph: SankeyGraph<SankeyNodeDatum, SankeyLinkDatum>;
  nodes: SankeyNode<SankeyNodeDatum, SankeyLinkDatum>[];
  links: SankeyLink<SankeyNodeDatum, SankeyLinkDatum>[];

  // Dimensions
  width: number;
  height: number;
  innerWidth: number;
  innerHeight: number;
  margin: Margin;

  // Hover state
  hoveredNodeIndex: number | null;
  hoveredLinkIndex: number | null;
  setHoveredNodeIndex: (index: number | null) => void;
  setHoveredLinkIndex: (index: number | null) => void;

  // Tooltip
  tooltipData: SankeyTooltipData | null;
  setTooltipData: Dispatch<SetStateAction<SankeyTooltipData | null>>;
  containerRef: RefObject<HTMLDivElement | null>;

  // Animation
  isLoaded: boolean;
  animationDuration: number;
  /** Motion enter transition (spring or cubic-bezier tween). */
  enterTransition?: Transition;
  /** Increments when enter animation should replay. */
  revealEpoch: number;

  // Mouse position for dynamic tooltips
  mousePos: { x: number; y: number } | null;

  // Link path generator
  createPath: (link: SankeyLink<SankeyNodeDatum, SankeyLinkDatum>) => string;
}

const SankeyContext = createContext<SankeyContextValue | null>(null);

export function SankeyProvider({
  children,
  value,
}: {
  children: React.ReactNode;
  value: SankeyContextValue;
}) {
  return (
    <SankeyContext.Provider value={value}>{children}</SankeyContext.Provider>
  );
}

export function useSankey(): SankeyContextValue {
  const context = useContext(SankeyContext);
  if (!context) {
    throw new Error("useSankey must be used within a SankeyProvider");
  }
  return context;
}

// CSS variables for sankey theming
export const sankeyCssVars = {
  background: "var(--chart-background)",
  foreground: "var(--chart-foreground)",
  nodePrimary: "var(--chart-line-primary)",
  nodeSecondary: "var(--chart-line-secondary)",
  linkColor: "var(--chart-foreground-muted, hsl(0, 0%, 50%))",
};
```

##### File: `src/charts/sankey/sankey-node.tsx`

```tsx
"use client";

import type { SankeyNode as SankeyNodeType } from "d3-sankey";
import { motion } from "motion/react";
import { useCallback, useMemo } from "react";
import { intFmt } from "../chart-formatters";
import { transitionWithDelay } from "../motion-utils";
import {
  type SankeyLinkDatum,
  type SankeyNodeDatum,
  useSankey,
} from "./sankey-context";

// Helper to get node index from link source/target
type NodeOrIndex = SankeyNodeType<SankeyNodeDatum, SankeyLinkDatum> | number;

function getNodeIndex(nodeOrIndex: NodeOrIndex): number | undefined {
  if (typeof nodeOrIndex === "number") {
    return nodeOrIndex;
  }
  return nodeOrIndex.index;
}

export interface SankeyNodeProps {
  /** Fill color for nodes. Default: uses theme colors */
  fill?: string;
  /** Corner radius for nodes. Default: 4 */
  lineCap?: number;
  /** Opacity when another node/link is hovered. Default: 0.4 */
  fadedOpacity?: number;
  /** Show node labels. Default: true */
  showLabels?: boolean;
  /** Custom node color function */
  getNodeColor?: (
    node: SankeyNodeType<SankeyNodeDatum, SankeyLinkDatum>,
    index: number
  ) => string;
}

interface AnimatedNodeProps {
  x: number;
  y: number;
  width: number;
  height: number;
  fill: string;
  rx: number;
  index: number;
  totalNodes: number;
  isFaded: boolean;
  fadedOpacity: number;
  animationDuration: number;
  onMouseEnter: () => void;
  onMouseLeave: () => void;
  name: string;
  value: number;
  isLeftSide: boolean;
  showLabels: boolean;
}

function AnimatedNode({
  x,
  y,
  width,
  height,
  fill,
  rx,
  index,
  totalNodes,
  isFaded,
  fadedOpacity,
  animationDuration,
  onMouseEnter,
  onMouseLeave,
  name,
  value,
  isLeftSide,
  showLabels,
}: AnimatedNodeProps) {
  const { enterTransition, revealEpoch } = useSankey();

  const nodeAnimDuration = animationDuration * 0.6;
  const staggerDelaySec =
    ((index / totalNodes) * nodeAnimDuration * 0.4) / 1000;
  const nameLabelDelaySec =
    staggerDelaySec + (nodeAnimDuration * 0.6 * 0.3) / 1000;
  const valueLabelDelaySec = nameLabelDelaySec + 0.06;

  const nodeEnter = transitionWithDelay(enterTransition, staggerDelaySec);
  const nameEnter = transitionWithDelay(enterTransition, nameLabelDelaySec);
  const valueEnter = transitionWithDelay(enterTransition, valueLabelDelaySec);
  const nameLabelX = isLeftSide ? x - 12 : x + width + 12;
  const valueLabelX = isLeftSide ? x - 12 : x + width + 12;
  const nodeOpacity = isFaded ? fadedOpacity : 1;
  const nameOpacity = isFaded ? fadedOpacity : 1;
  const valueOpacity = isFaded ? fadedOpacity * 0.8 : 0.6;

  return (
    <motion.g
      onMouseEnter={onMouseEnter}
      onMouseLeave={onMouseLeave}
      style={{ cursor: "pointer" }}
    >
      <motion.rect
        animate={{ opacity: nodeOpacity, scaleY: 1 }}
        fill={fill}
        height={height}
        initial={{ opacity: 0, scaleY: 0 }}
        key={`node-${index}-${revealEpoch}`}
        rx={rx}
        ry={rx}
        style={{ originY: 0.5 }}
        transition={nodeEnter}
        width={width}
        x={x}
        y={y}
      />
      {showLabels && (
        <>
          <motion.text
            animate={{ opacity: nameOpacity, x: nameLabelX }}
            className="fill-foreground font-medium text-[13px]"
            dy="0.35em"
            initial={{ opacity: 0, x: isLeftSide ? x + 8 : x + width - 8 }}
            key={`name-${index}-${revealEpoch}`}
            textAnchor={isLeftSide ? "end" : "start"}
            transition={nameEnter}
            y={y + height / 2}
          >
            {name}
          </motion.text>
          <motion.text
            animate={{ opacity: valueOpacity, x: valueLabelX }}
            className="fill-foreground text-[11px]"
            dy="0.35em"
            initial={{ opacity: 0, x: isLeftSide ? x + 8 : x + width - 8 }}
            key={`value-${index}-${revealEpoch}`}
            textAnchor={isLeftSide ? "end" : "start"}
            transition={valueEnter}
            y={y + height / 2 + 16}
          >
            {intFmt(value)} sessions
          </motion.text>
        </>
      )}
    </motion.g>
  );
}

export function SankeyNode({
  fill,
  lineCap = 4,
  fadedOpacity = 0.4,
  showLabels = true,
  getNodeColor: getNodeColorProp,
}: SankeyNodeProps) {
  const {
    nodes,
    links,
    width,
    margin,
    hoveredNodeIndex,
    hoveredLinkIndex,
    setHoveredNodeIndex,
    setTooltipData,
    animationDuration,
  } = useSankey();

  // Default colors using CSS variables
  const defaultColors = useMemo(
    () => [
      "var(--chart-1)",
      "var(--chart-2)",
      "var(--chart-3)",
      "var(--chart-4)",
      "var(--chart-5)",
    ],
    []
  );

  // Get color for a node
  const getColor = useCallback(
    (
      node: SankeyNodeType<SankeyNodeDatum, SankeyLinkDatum>,
      index: number
    ): string => {
      if (fill) {
        return fill;
      }
      if (getNodeColorProp) {
        return getNodeColorProp(node, index);
      }

      return defaultColors[index % defaultColors.length] ?? "var(--chart-1)";
    },
    [fill, getNodeColorProp, defaultColors]
  );

  // Check if a node is connected to the hovered element
  const isNodeConnected = useCallback(
    (nodeIndex: number) => {
      if (hoveredNodeIndex !== null) {
        if (hoveredNodeIndex === nodeIndex) {
          return true;
        }
        return links.some((link) => {
          const sIdx = getNodeIndex(link.source as NodeOrIndex);
          const tIdx = getNodeIndex(link.target as NodeOrIndex);
          return (
            (sIdx === hoveredNodeIndex && tIdx === nodeIndex) ||
            (tIdx === hoveredNodeIndex && sIdx === nodeIndex)
          );
        });
      }
      if (hoveredLinkIndex !== null) {
        const link = links[hoveredLinkIndex];
        if (!link) {
          return false;
        }
        const sIdx = getNodeIndex(link.source as NodeOrIndex);
        const tIdx = getNodeIndex(link.target as NodeOrIndex);
        return sIdx === nodeIndex || tIdx === nodeIndex;
      }
      return false;
    },
    [hoveredNodeIndex, hoveredLinkIndex, links]
  );

  const isAnyHovered = hoveredNodeIndex !== null || hoveredLinkIndex !== null;
  const innerWidth = width - margin.left - margin.right;

  return (
    <g className="sankey-nodes">
      {nodes.map((node, index) => {
        const nodeX = node.x0 ?? 0;
        const nodeY = node.y0 ?? 0;
        const nodeWidth = (node.x1 ?? 0) - nodeX;
        const nodeHeight = (node.y1 ?? 0) - nodeY;

        const isConnected = isNodeConnected(index);
        const isFaded = isAnyHovered && !isConnected;
        const isLeftSide = nodeX < innerWidth / 2;

        let displayValue = 0;
        for (const l of links) {
          const sIdx = getNodeIndex(l.source as NodeOrIndex);
          const tIdx = getNodeIndex(l.target as NodeOrIndex);
          if (node.category === "source" && sIdx === index) {
            displayValue += l.value;
          } else if (node.category !== "source" && tIdx === index) {
            displayValue += l.value;
          }
        }

        const handleMouseEnter = () => {
          setHoveredNodeIndex(index);
          setTooltipData({
            type: "node",
            nodeIndex: index,
            x: 0,
            y: 0,
            data: node,
          });
        };

        const handleMouseLeave = () => {
          setHoveredNodeIndex(null);
          setTooltipData(null);
        };

        return (
          <AnimatedNode
            animationDuration={animationDuration}
            fadedOpacity={fadedOpacity}
            fill={getColor(node, index)}
            height={nodeHeight}
            index={index}
            isFaded={isFaded}
            isLeftSide={isLeftSide}
            key={`node-${node.name}`}
            name={node.name}
            onMouseEnter={handleMouseEnter}
            onMouseLeave={handleMouseLeave}
            rx={lineCap}
            showLabels={showLabels}
            totalNodes={nodes.length}
            value={displayValue}
            width={nodeWidth}
            x={nodeX}
            y={nodeY}
          />
        );
      })}
    </g>
  );
}

SankeyNode.displayName = "SankeyNode";

export default SankeyNode;
```

##### File: `src/charts/sankey/sankey-link.tsx`

```tsx
"use client";

import type {
  SankeyLink as SankeyLinkType,
  SankeyNode as SankeyNodeType,
} from "d3-sankey";
import { motion, useTransform } from "motion/react";
import { useCallback, useLayoutEffect, useMemo, useRef, useState } from "react";
import { useMountProgress } from "../use-mount-progress";
import {
  type SankeyLinkDatum,
  type SankeyNodeDatum,
  useSankey,
} from "./sankey-context";

// Helper to get node index from link source/target
type NodeOrIndex = SankeyNodeType<SankeyNodeDatum, SankeyLinkDatum> | number;

function getNodeIndex(nodeOrIndex: NodeOrIndex): number | undefined {
  if (typeof nodeOrIndex === "number") {
    return nodeOrIndex;
  }
  return nodeOrIndex.index;
}

function getNodeObject(
  nodeOrIndex: NodeOrIndex
): SankeyNodeType<SankeyNodeDatum, SankeyLinkDatum> | null {
  if (typeof nodeOrIndex === "number") {
    return null;
  }
  return nodeOrIndex;
}

// Default node color palette using CSS variables
const defaultColors = [
  "var(--chart-1)",
  "var(--chart-2)",
  "var(--chart-3)",
  "var(--chart-4)",
  "var(--chart-5)",
];

function getDefaultNodeColor(
  node: SankeyNodeType<SankeyNodeDatum, SankeyLinkDatum>
): string {
  const index = node.index ?? 0;
  return defaultColors[index % defaultColors.length] ?? "var(--chart-1)";
}

export interface SankeyLinkProps {
  /** Stroke color for links (overrides gradient). Default: uses gradient */
  stroke?: string;
  /** Stroke opacity. Default: 0.5 */
  strokeOpacity?: number;
  /** Opacity when another link/node is hovered. Default: 0.1 */
  fadedOpacity?: number;
  /** Use gradient from source to target color. Default: true */
  useGradient?: boolean;
  /** Custom function to get node color (for gradient) */
  getNodeColor?: (
    node: SankeyNodeType<SankeyNodeDatum, SankeyLinkDatum>,
    index: number
  ) => string;
  /** Custom link color function (overrides gradient) */
  getLinkColor?: (
    link: SankeyLinkType<SankeyNodeDatum, SankeyLinkDatum>,
    index: number
  ) => string;
  /** Pattern definitions to render in defs. Use @visx/pattern components (PatternLines, PatternCircles, etc.) */
  patterns?: React.ReactNode;
  /** Return pattern ID for a link, or null/undefined to use gradient/solid color */
  getLinkPattern?: (
    link: SankeyLinkType<SankeyNodeDatum, SankeyLinkDatum>,
    index: number
  ) => string | null | undefined;
}

interface AnimatedLinkProps {
  path: string;
  width: number;
  stroke: string;
  strokeOpacity: number;
  index: number;
  totalLinks: number;
  isFaded: boolean;
  isHighlighted: boolean;
  fadedOpacity: number;
  animationDuration: number;
  onMouseEnter: () => void;
  onMouseLeave: () => void;
}

function AnimatedLink({
  path,
  width,
  stroke,
  strokeOpacity,
  index,
  totalLinks,
  isFaded,
  isHighlighted,
  fadedOpacity,
  animationDuration,
  onMouseEnter,
  onMouseLeave,
}: AnimatedLinkProps) {
  const { enterTransition, revealEpoch } = useSankey();
  const pathRef = useRef<SVGPathElement>(null);
  const [pathLength, setPathLength] = useState(0);

  // Links animate during the last 80% of total duration, starting at 20%
  const linkStartDelay = animationDuration * 0.2;
  const linkAnimDuration = animationDuration * 0.8;
  const staggerDelaySeconds =
    (linkStartDelay + (index / totalLinks) * linkAnimDuration * 0.4) / 1000;

  useLayoutEffect(() => {
    if (pathRef.current) {
      const length = pathRef.current.getTotalLength();
      setPathLength(length);
    }
  });

  const progress = useMountProgress(
    enterTransition,
    staggerDelaySeconds,
    `${revealEpoch}-${index}`
  );
  const strokeDashoffset = useTransform(progress, [0, 1], [pathLength, 0]);

  // Calculate target opacity
  const getTargetOpacity = () => {
    if (isFaded) {
      return fadedOpacity;
    }
    if (isHighlighted) {
      return Math.min(1, strokeOpacity * 1.3);
    }
    return strokeOpacity;
  };
  const targetOpacity = getTargetOpacity();

  // Dasharray for path reveal
  const dashArray = pathLength > 0 ? `${pathLength} ${pathLength}` : "none";

  // Ensure opacity values are always numbers
  const initialOpacity = strokeOpacity ?? 0.5;
  const animatedOpacity = targetOpacity ?? initialOpacity;

  return (
    <motion.path
      animate={{ opacity: animatedOpacity }}
      d={path}
      fill="none"
      initial={{ opacity: initialOpacity }}
      onMouseEnter={onMouseEnter}
      onMouseLeave={onMouseLeave}
      ref={pathRef}
      stroke={stroke}
      strokeDasharray={dashArray}
      strokeWidth={Math.max(1, width)}
      style={{
        cursor: "pointer",
        strokeDashoffset,
      }}
      transition={{ duration: 0.18, ease: "easeOut" }}
    />
  );
}

export function SankeyLink({
  stroke,
  strokeOpacity = 0.5,
  fadedOpacity = 0.1,
  useGradient = true,
  getNodeColor,
  getLinkColor,
  patterns,
  getLinkPattern,
}: SankeyLinkProps) {
  const {
    links,
    hoveredNodeIndex,
    hoveredLinkIndex,
    setHoveredLinkIndex,
    setTooltipData,
    animationDuration,
    createPath,
  } = useSankey();

  // Get color for a node (for gradients)
  const getNodeColorFn = useCallback(
    (node: SankeyNodeType<SankeyNodeDatum, SankeyLinkDatum>): string => {
      if (getNodeColor) {
        return getNodeColor(node, node.index ?? 0);
      }
      return getDefaultNodeColor(node);
    },
    [getNodeColor]
  );

  // Get color for a link (solid color, when not using gradient)
  const getLinkColorFn = useCallback(
    (link: SankeyLinkType<SankeyNodeDatum, SankeyLinkDatum>, index: number) => {
      if (getLinkColor) {
        return getLinkColor(link, index);
      }
      return stroke || "var(--chart-line-primary)";
    },
    [getLinkColor, stroke]
  );

  // Check if any element is hovered
  const isAnyHovered = hoveredNodeIndex !== null || hoveredLinkIndex !== null;

  // Build gradient definitions for all links
  const gradientDefs = useMemo(() => {
    if (!useGradient || stroke || getLinkColor) {
      return null;
    }

    return links.map((link, index) => {
      const sourceNode = getNodeObject(link.source as NodeOrIndex);
      const targetNode = getNodeObject(link.target as NodeOrIndex);

      // Always define a gradient so `url(#...)` never points to a missing id.
      // Use fallback colors if nodes can't be resolved
      const sourceColor = sourceNode
        ? getNodeColorFn(sourceNode)
        : "var(--chart-1)";
      const targetColor = targetNode
        ? getNodeColorFn(targetNode)
        : "var(--chart-1)";
      const gradientId = `link-gradient-${index}`;

      // Get absolute x positions for gradient
      // Use userSpaceOnUse to avoid issues with horizontal links (where bounding box has zero height)
      const x1 = sourceNode?.x1 ?? 0;
      const x2 = targetNode?.x0 ?? 100;

      return (
        <linearGradient
          gradientUnits="userSpaceOnUse"
          id={gradientId}
          key={gradientId}
          x1={x1}
          x2={x2}
          y1="0"
          y2="0"
        >
          <stop offset="0%" stopColor={sourceColor} stopOpacity={1} />
          <stop offset="100%" stopColor={targetColor} stopOpacity={1} />
        </linearGradient>
      );
    });
  }, [links, useGradient, stroke, getLinkColor, getNodeColorFn]);

  return (
    <g className="sankey-links">
      {/* Pattern and gradient definitions */}
      <defs>
        {patterns}
        {gradientDefs}
      </defs>

      {/* Links */}
      {links.map((link, index) => {
        const path = createPath(link);
        const linkWidth = link.width ?? 1;

        // Skip if path is empty
        if (!path || path.trim() === "") {
          return null;
        }

        const sIdx = getNodeIndex(link.source as NodeOrIndex);
        const tIdx = getNodeIndex(link.target as NodeOrIndex);

        // Use fallback indices if we can't resolve
        const sourceIdx =
          sIdx ?? (typeof link.source === "number" ? link.source : -1);
        const targetIdx =
          tIdx ?? (typeof link.target === "number" ? link.target : -1);

        const isHighlighted =
          hoveredLinkIndex === index ||
          hoveredNodeIndex === sourceIdx ||
          hoveredNodeIndex === targetIdx;
        const isFaded = isAnyHovered && !isHighlighted;

        const handleMouseEnter = () => {
          setHoveredLinkIndex(index);
          setTooltipData({
            type: "link",
            linkIndex: index,
            x: 0,
            y: 0,
            data: link,
          });
        };

        const handleMouseLeave = () => {
          setHoveredLinkIndex(null);
          setTooltipData(null);
        };

        // Determine stroke color (pattern URL, gradient URL, or solid color)
        let linkStroke: string;
        const patternId = getLinkPattern?.(link, index);
        if (patternId) {
          // Use pattern fill
          linkStroke = `url(#${patternId})`;
        } else if (useGradient && !stroke && !getLinkColor) {
          // Use gradient
          linkStroke = `url(#link-gradient-${index})`;
        } else {
          linkStroke = getLinkColorFn(link, index);
        }

        return (
          <AnimatedLink
            animationDuration={animationDuration}
            fadedOpacity={fadedOpacity}
            index={index}
            isFaded={isFaded}
            isHighlighted={isHighlighted}
            key={`link-${sourceIdx}-${targetIdx}-${link.width ?? link.value ?? ""}`}
            onMouseEnter={handleMouseEnter}
            onMouseLeave={handleMouseLeave}
            path={path}
            stroke={linkStroke}
            strokeOpacity={strokeOpacity}
            totalLinks={links.length}
            width={linkWidth}
          />
        );
      })}
    </g>
  );
}

SankeyLink.displayName = "SankeyLink";

export default SankeyLink;
```

##### File: `src/charts/sankey/sankey-tooltip.tsx`

```tsx
"use client";

import type { SankeyLink, SankeyNode } from "d3-sankey";
import { intFmt } from "../chart-formatters";
import { TooltipBox } from "../tooltip/tooltip-box";
import { TooltipContent, type TooltipRow } from "../tooltip/tooltip-content";
import {
  type SankeyLinkDatum,
  type SankeyNodeDatum,
  useSankey,
} from "./sankey-context";

// Helper to get node name from link source/target
type NodeOrIndex = SankeyNode<SankeyNodeDatum, SankeyLinkDatum> | number;

function getNodeName(nodeOrIndex: NodeOrIndex, fallbackIndex: number): string {
  if (typeof nodeOrIndex === "number") {
    return `Node ${nodeOrIndex}`;
  }
  return nodeOrIndex.name ?? `Node ${fallbackIndex}`;
}

export interface SankeyTooltipProps {
  /** Custom content renderer for node tooltips */
  nodeContent?: (props: {
    node: SankeyNode<SankeyNodeDatum, SankeyLinkDatum>;
    index: number;
  }) => React.ReactNode;
  /** Custom content renderer for link tooltips */
  linkContent?: (props: {
    link: SankeyLink<SankeyNodeDatum, SankeyLinkDatum>;
    index: number;
  }) => React.ReactNode;
  /** Value formatter function */
  formatValue?: (value: number) => string;
  /** Custom class name */
  className?: string;
}

export function SankeyTooltip({
  nodeContent,
  linkContent,
  formatValue = intFmt,
  className = "",
}: SankeyTooltipProps) {
  const {
    tooltipData,
    containerRef,
    width,
    height,
    margin,
    nodes,
    links,
    mousePos,
  } = useSankey();

  if (!tooltipData) {
    return null;
  }

  // Use mouse position if available, otherwise fallback to anchor point
  const x = mousePos ? mousePos.x : tooltipData.x + margin.left;
  const y = mousePos ? mousePos.y : tooltipData.y + margin.top;

  // Render node tooltip
  if (tooltipData.type === "node" && tooltipData.nodeIndex !== undefined) {
    const node = nodes[tooltipData.nodeIndex];
    if (!node) {
      return null;
    }

    // Calculate total value flowing through this node
    const totalValue = node.value ?? 0;

    // Custom content
    if (nodeContent) {
      return (
        <TooltipBox
          className={className}
          containerHeight={height}
          containerRef={containerRef}
          containerWidth={width}
          visible
          x={x}
          y={y}
        >
          {nodeContent({ node, index: tooltipData.nodeIndex })}
        </TooltipBox>
      );
    }

    // Default node tooltip
    const rows: TooltipRow[] = [
      {
        color: "var(--chart-line-primary)",
        label: "Sessions",
        value: formatValue(totalValue),
      },
    ];

    return (
      <TooltipBox
        className={className}
        containerHeight={height}
        containerRef={containerRef}
        containerWidth={width}
        visible
        x={x}
        y={y}
      >
        <TooltipContent rows={rows} title={node.name} />
      </TooltipBox>
    );
  }

  // Render link tooltip
  if (tooltipData.type === "link" && tooltipData.linkIndex !== undefined) {
    const link = links[tooltipData.linkIndex];
    if (!link) {
      return null;
    }

    // Get source and target names
    const sourceName = getNodeName(
      link.source as NodeOrIndex,
      tooltipData.linkIndex
    );
    const targetName = getNodeName(
      link.target as NodeOrIndex,
      tooltipData.linkIndex
    );

    // Custom content
    if (linkContent) {
      return (
        <TooltipBox
          className={className}
          containerHeight={height}
          containerRef={containerRef}
          containerWidth={width}
          visible
          x={x}
          y={y}
        >
          {linkContent({ link, index: tooltipData.linkIndex })}
        </TooltipBox>
      );
    }

    // Default link tooltip
    const rows: TooltipRow[] = [
      {
        color: "var(--chart-foreground-muted)",
        label: "Flow",
        value: formatValue(link.value),
      },
    ];

    return (
      <TooltipBox
        className={className}
        containerHeight={height}
        containerRef={containerRef}
        containerWidth={width}
        visible
        x={x}
        y={y}
      >
        <TooltipContent rows={rows} title={`${sourceName} → ${targetName}`} />
      </TooltipBox>
    );
  }

  return null;
}

SankeyTooltip.displayName = "SankeyTooltip";

export default SankeyTooltip;
```

##### File: `src/charts/sankey/index.ts`

```tsx
export {
  SankeyChart,
  type SankeyChartProps,
  type SankeyData,
} from "./sankey-chart";
export {
  type Margin,
  type SankeyContextValue,
  type SankeyLinkDatum,
  type SankeyNodeDatum,
  SankeyProvider,
  type SankeyTooltipData,
  sankeyCssVars,
  useSankey,
} from "./sankey-context";
export { SankeyLink, type SankeyLinkProps } from "./sankey-link";
export { SankeyNode, type SankeyNodeProps } from "./sankey-node";
export { SankeyTooltip, type SankeyTooltipProps } from "./sankey-tooltip";
```

---

### Scatter Chart <a name="scatter-chart"></a>

A composable time-series scatter chart with offset rings, hover dimming, and animated enter

- **Category**: Charts
- **Registry Key**: `scatter-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/scatter-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `d3-array`
  - `d3-scale`
  - `motion`
  - `react-use-measure`
- **Local Registry Dependencies**:
  - `@bklit/chart-context`
  - `@bklit/chart-animation`
  - `@bklit/chart-series`
  - `@bklit/grid`
  - `@bklit/x-axis`
  - `@bklit/chart-tooltip`
  - `@bklit/utils`

#### How to Use

##### Example 1

```tsx
import { ScatterChart, Scatter, Grid, XAxis, ChartTooltip } from "@bklitui/ui/charts";

const data = [
  { date: new Date("2025-01-01"), users: 1200 },
  { date: new Date("2025-02-01"), users: 1350 },
  { date: new Date("2025-03-01"), users: 1100 },
];

export default function SimpleScatter() {
  return (
    <ScatterChart data={data}>
      <Grid horizontal />
      <Scatter dataKey="users" />
      <XAxis />
      <ChartTooltip />
    </ScatterChart>
  );
}
```

##### Example 2

```tsx
<ScatterChart data={data}>
  <Grid horizontal />
  <Scatter dataKey="sessions" />
  <Scatter dataKey="conversions" />
  <XAxis />
  <ChartTooltip />
</ScatterChart>
```

##### Example 3

```tsx
<Scatter dataKey="users" radius={6} strokeWidth={2} ringGap={2} />
```

##### Example 4

```tsx
<Scatter
  dataKey="users"
  fadeOnHover
  inactiveOpacity={0.5}
  inactiveBlur={2}
  showActiveHighlight
/>
```

##### Example 5

```tsx
<Scatter dataKey="users" strokeWidth={0} yGradient />

// Custom stops
<Scatter
  dataKey="users"
  strokeWidth={0}
  yGradient={{ from: "var(--color-red-500)", to: "var(--color-emerald-500)" }}
/>
```

#### Props & API Reference

##### `ScatterChart` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `data` | `Record<string, unknown>[]` | required | Rows with a date (or `xDataKey`) and numeric series fields |
| `xDataKey` | `string` | `"date"` | Field used for the time x-axis |
| `margin` | `Partial<Margin>` | `40` all sides | Chart margins |
| `animationDuration` | `number` | `1100` | Enter animation duration (ms) |
| `enterTransition` | `Transition` | line-chart default | Motion tween for enter |
| `revealSignature` | `string` | — | Change to replay enter animation |
| `aspectRatio` | `string` | `"2 / 1"` | Container aspect ratio |

##### `Scatter` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dataKey` | `string` | required | Y value field |
| `fill` | `string` | series palette | Inner dot fill |
| `stroke` | `string` | same as `fill` | Outer ring color |
| `strokeWidth` | `number` | `2` | Outer ring width (0 disables) |
| `ringGap` | `number` | `2` | Gap between fill and ring (px) |
| `radius` | `number` | `5` | Inner dot radius (px) |
| `fadeOnHover` | `boolean` | `true` | Dim/blur non-active points on hover |
| `inactiveOpacity` | `number` | `0.5` | Opacity for dimmed points |
| `inactiveBlur` | `number` | `2` | Blur (px) for dimmed points |
| `showActiveHighlight` | `boolean` | `true` | Scale up the active point |
| `yGradient` | `boolean \| { from?: string; to?: string }` | — | Color dots by y-position (default red → green) |

#### Component Source Code

##### File: `src/charts/scatter-chart.tsx`

```tsx
"use client";

import type { Transition } from "motion/react";
import {
  Children,
  isValidElement,
  type ReactNode,
  useMemo,
  useRef,
} from "react";
import useMeasure from "react-use-measure";
import { cn } from "@/lib/utils";
import { DEFAULT_CHART_ENTER_TRANSITION } from "./animation";
import {
  defaultScatterColors,
  type LineConfig,
  type Margin,
} from "./chart-context";
import { Scatter, type ScatterProps } from "./scatter";
import { ScatterChartInner } from "./scatter-chart-shell";

export interface ScatterChartProps {
  /** Data array — each item should have a date field and numeric values */
  data: Record<string, unknown>[];
  /** Key in data for the x-axis (date). Default: "date" */
  xDataKey?: string;
  /** Chart margins */
  margin?: Partial<Margin>;
  /** Animation duration in milliseconds. Default: 1100 */
  animationDuration?: number;
  /** CSS easing for clip-reveal. Default: cubic-bezier(0.85, 0, 0.15, 1) */
  animationEasing?: string;
  enterTransition?: Transition;
  revealSignature?: string;
  /** Aspect ratio as "width / height". Default: "2 / 1" */
  aspectRatio?: string;
  /** Additional class name for the container */
  className?: string;
  /** Child components (Scatter, Grid, ChartTooltip, XAxis, etc.) */
  children: ReactNode;
}

const DEFAULT_MARGIN: Margin = { top: 40, right: 40, bottom: 40, left: 40 };

function extractScatterConfigs(children: ReactNode): LineConfig[] {
  const configs: LineConfig[] = [];
  let seriesIndex = 0;

  Children.forEach(children, (child) => {
    if (!isValidElement(child)) {
      return;
    }

    const childType = child.type as {
      displayName?: string;
      name?: string;
    };
    const componentName =
      typeof child.type === "function"
        ? childType.displayName || childType.name || ""
        : "";

    const props = child.props as ScatterProps | undefined;
    const isScatterComponent =
      componentName === "Scatter" ||
      child.type === Scatter ||
      (props && typeof props.dataKey === "string" && props.dataKey.length > 0);

    if (isScatterComponent && props?.dataKey) {
      const seriesColor =
        defaultScatterColors[seriesIndex % defaultScatterColors.length] ??
        defaultScatterColors[0];
      configs.push({
        dataKey: props.dataKey,
        stroke: props.fill || props.stroke || seriesColor,
        strokeWidth: props.radius ?? 5,
      });
      seriesIndex += 1;
    }
  });

  return configs;
}

interface ChartInnerProps {
  width: number;
  height: number;
  data: Record<string, unknown>[];
  xDataKey: string;
  margin: Margin;
  animationDuration: number;
  animationEasing?: string;
  enterTransition?: Transition;
  revealSignature?: string;
  children: ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
}

function ChartInner({
  width,
  height,
  data,
  xDataKey,
  margin,
  animationDuration,
  animationEasing,
  enterTransition,
  revealSignature,
  children,
  containerRef,
}: ChartInnerProps) {
  const lines = useMemo(() => extractScatterConfigs(children), [children]);

  return (
    <ScatterChartInner
      animationDuration={animationDuration}
      animationEasing={animationEasing}
      containerRef={containerRef}
      data={data}
      enterTransition={enterTransition}
      height={height}
      lines={lines}
      margin={margin}
      revealSignature={revealSignature}
      width={width}
      xDataKey={xDataKey}
    >
      {children}
    </ScatterChartInner>
  );
}

export function ScatterChart({
  data,
  xDataKey = "date",
  margin: marginProp,
  animationDuration = 1100,
  animationEasing,
  enterTransition = DEFAULT_CHART_ENTER_TRANSITION,
  revealSignature,
  aspectRatio = "2 / 1",
  className = "",
  children,
}: ScatterChartProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const margin = { ...DEFAULT_MARGIN, ...marginProp };
  const [measureRef, bounds] = useMeasure({ debounce: 10 });

  const setContainerRef = (node: HTMLDivElement | null) => {
    containerRef.current = node;
    measureRef(node);
  };

  const width = bounds.width ?? 0;
  const height = bounds.height ?? 0;

  return (
    <div
      className={cn("relative w-full", className)}
      ref={setContainerRef}
      style={{ aspectRatio, touchAction: "none" }}
    >
      {width > 0 && height > 0 ? (
        <ChartInner
          animationDuration={animationDuration}
          animationEasing={animationEasing}
          containerRef={containerRef}
          data={data}
          enterTransition={enterTransition}
          height={height}
          margin={margin}
          revealSignature={revealSignature}
          width={width}
          xDataKey={xDataKey}
        >
          {children}
        </ChartInner>
      ) : null}
    </div>
  );
}

ScatterChart.displayName = "ScatterChart";

export { Scatter, type ScatterProps } from "./scatter";

export default ScatterChart;
```

##### File: `src/charts/scatter-chart-shell.tsx`

```tsx
"use client";

import { bisector } from "d3-array";
import { scaleLinear, scaleTime } from "d3-scale";
import type { Transition } from "motion/react";
import {
  Children,
  isValidElement,
  type ReactElement,
  type ReactNode,
  useCallback,
  useEffect,
  useMemo,
  useState,
} from "react";
import { DEFAULT_ANIMATION_EASING } from "./animation";
import {
  type ChartContextValue,
  ChartProvider,
  type LineConfig,
  type Margin,
} from "./chart-context";
import { isGradientDefComponent, isPatternDefComponent } from "./chart-defs";
import { shortDateFmt } from "./chart-formatters";
import { isPostOverlayComponent } from "./time-series-chart-shell";
import { useScatterChartInteraction } from "./use-scatter-chart-interaction";

export interface ScatterChartInnerProps {
  width: number;
  height: number;
  data: Record<string, unknown>[];
  xDataKey: string;
  margin: Margin;
  animationDuration: number;
  animationEasing?: string;
  enterTransition?: Transition;
  revealSignature?: string;
  children: ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
  lines: LineConfig[];
}

export function ScatterChartInner({
  width,
  height,
  data,
  xDataKey,
  margin,
  animationDuration,
  animationEasing = DEFAULT_ANIMATION_EASING,
  enterTransition,
  revealSignature = "",
  children,
  containerRef,
  lines,
}: ScatterChartInnerProps) {
  const [isLoaded, setIsLoaded] = useState(false);
  const [revealEpoch, setRevealEpoch] = useState(0);

  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  const xAccessor = useCallback(
    (d: Record<string, unknown>): Date => {
      const value = d[xDataKey];
      return value instanceof Date ? value : new Date(value as string | number);
    },
    [xDataKey]
  );

  const bisectDate = useMemo(
    () => bisector<Record<string, unknown>, Date>((d) => xAccessor(d)).left,
    [xAccessor]
  );

  const xRangePadding = useMemo(() => {
    if (lines.length === 0) {
      return 12;
    }
    const maxRadius = Math.max(...lines.map((line) => line.strokeWidth ?? 5));
    return maxRadius + 10;
  }, [lines]);

  const xScale = useMemo(() => {
    const dates = data.map((d) => xAccessor(d));
    const minTime = Math.min(...dates.map((d) => d.getTime()));
    const maxTime = Math.max(...dates.map((d) => d.getTime()));

    return scaleTime<number>()
      .range([
        xRangePadding,
        Math.max(xRangePadding, innerWidth - xRangePadding),
      ])
      .domain([minTime, maxTime]);
  }, [innerWidth, data, xAccessor, xRangePadding]);

  const columnWidth = useMemo(() => {
    if (data.length < 2) {
      return 0;
    }
    return innerWidth / (data.length - 1);
  }, [innerWidth, data.length]);

  const yScale = useMemo(() => {
    let maxValue = 0;
    for (const line of lines) {
      for (const d of data) {
        const value = d[line.dataKey];
        if (typeof value === "number" && value > maxValue) {
          maxValue = value;
        }
      }
    }

    if (maxValue === 0) {
      maxValue = 100;
    }

    return scaleLinear<number>()
      .range([innerHeight, 0])
      .domain([0, maxValue * 1.1])
      .nice();
  }, [innerHeight, data, lines]);

  const dateLabels = useMemo(
    () => data.map((d) => shortDateFmt.format(xAccessor(d))),
    [data, xAccessor]
  );

  // biome-ignore lint/correctness/useExhaustiveDependencies: revealSignature
  useEffect(() => {
    setRevealEpoch((n) => n + 1);
    setIsLoaded(false);
    const timer = setTimeout(() => {
      setIsLoaded(true);
    }, animationDuration);
    return () => clearTimeout(timer);
  }, [animationDuration, revealSignature]);

  const canInteract = isLoaded;

  const {
    tooltipData,
    setTooltipData,
    selection,
    clearSelection,
    interactionHandlers,
    interactionStyle,
  } = useScatterChartInteraction({
    xScale,
    yScale,
    data,
    lines,
    margin,
    xAccessor,
    bisectDate,
    canInteract,
  });

  if (width < 10 || height < 10) {
    return null;
  }

  const defsChildren: ReactElement[] = [];
  const preOverlayChildren: ReactElement[] = [];
  const postOverlayChildren: ReactElement[] = [];

  Children.forEach(children, (child) => {
    if (!isValidElement(child)) {
      return;
    }

    if (isGradientDefComponent(child)) {
      defsChildren.push(child);
    } else if (isPatternDefComponent(child)) {
      preOverlayChildren.push(child);
    } else if (isPostOverlayComponent(child)) {
      postOverlayChildren.push(child);
    } else {
      preOverlayChildren.push(child);
    }
  });

  const contextValue: ChartContextValue = {
    data,
    renderData: data,
    xScale: xScale as ChartContextValue["xScale"],
    yScale: yScale as ChartContextValue["yScale"],
    width,
    height,
    innerWidth,
    innerHeight,
    margin,
    columnWidth,
    tooltipData,
    setTooltipData,
    containerRef,
    lines,
    isLoaded,
    animationDuration,
    animationEasing,
    enterTransition,
    revealEpoch,
    xAccessor,
    dateLabels,
    selection,
    clearSelection,
  };

  return (
    <ChartProvider value={contextValue}>
      <svg
        aria-hidden="true"
        className="overflow-visible"
        height={height}
        width={width}
      >
        {defsChildren.length > 0 && <defs>{defsChildren}</defs>}

        <rect fill="transparent" height={height} width={width} x={0} y={0} />

        <g
          {...interactionHandlers}
          style={interactionStyle}
          transform={`translate(${margin.left},${margin.top})`}
        >
          <rect
            fill="transparent"
            height={innerHeight}
            width={innerWidth}
            x={0}
            y={0}
          />

          {preOverlayChildren}
          {postOverlayChildren}
        </g>
      </svg>
    </ChartProvider>
  );
}
```

##### File: `src/charts/time-series-chart-shell.tsx`

```tsx
"use client";

import { scaleLinear, scaleTime } from "@visx/scale";
import { bisector, extent } from "d3-array";
import type { Transition } from "motion/react";
import {
  Children,
  cloneElement,
  isValidElement,
  memo,
  type ReactElement,
  type ReactNode,
  useCallback,
  useEffect,
  useMemo,
  useState,
} from "react";
import { DEFAULT_ANIMATION_EASING } from "./animation";
import { ChartProvider, type LineConfig, type Margin } from "./chart-context";
import { isGradientDefComponent, isPatternDefComponent } from "./chart-defs";
import { shortDateFmt } from "./chart-formatters";
import { ChartRevealClip } from "./chart-reveal-clip";
import {
  decimateTimeSeries,
  maxRenderPointsForWidth,
} from "./decimate-time-series";
import {
  computeSeriesBarRevealClipPadding,
  computeSeriesBarWidth,
} from "./series-bar-layout";
import { useStaticChartPreview } from "./static-chart-preview-context";
import { useChartInteraction } from "./use-chart-interaction";

function collectNumericExtents(
  data: Record<string, unknown>[],
  dataKeys: string[]
) {
  let minValue = Number.POSITIVE_INFINITY;
  let maxValue = Number.NEGATIVE_INFINITY;

  for (const d of data) {
    for (const key of dataKeys) {
      const value = d[key];
      if (typeof value === "number") {
        if (value < minValue) {
          minValue = value;
        }
        if (value > maxValue) {
          maxValue = value;
        }
      }
    }
  }

  if (minValue === Number.POSITIVE_INFINITY) {
    return { minValue: 0, maxValue: 100 };
  }

  return { minValue, maxValue };
}

function resolveTimeSeriesYDomain(
  data: Record<string, unknown>[],
  dataKeys: string[],
  yScaleDomainMax: number | undefined
): [number, number] {
  if (yScaleDomainMax != null && yScaleDomainMax > 0) {
    return [0, yScaleDomainMax * 1.1];
  }

  const { minValue, maxValue } = collectNumericExtents(data, dataKeys);

  if (minValue >= 0) {
    const top = maxValue <= 0 ? 100 : maxValue * 1.1;
    return [0, top];
  }

  const padding = (maxValue - minValue) * 0.05 || 1;
  return [minValue - padding, maxValue + padding];
}

/** Markers render after the interaction overlay so they stay clickable. */
export function isPostOverlayComponent(child: ReactElement): boolean {
  const childType = child.type as {
    displayName?: string;
    name?: string;
    __isChartMarkers?: boolean;
  };

  if (childType.__isChartMarkers) {
    return true;
  }

  const componentName =
    typeof child.type === "function"
      ? childType.displayName || childType.name || ""
      : "";

  return componentName === "ChartMarkers" || componentName === "MarkerGroup";
}

function ensureChildKey(child: ReactElement, index: number): ReactElement {
  if (child.key != null) {
    return child;
  }
  return cloneElement(child, { key: `chart-child-${index}` });
}

export interface TimeSeriesChartInnerProps {
  width: number;
  height: number;
  data: Record<string, unknown>[];
  xDataKey: string;
  margin: Margin;
  animationDuration: number;
  animationEasing?: string;
  enterTransition?: Transition;
  /** Signature of motion URL state — triggers reveal replay when it changes. */
  revealSignature?: string;
  children: ReactNode;
  containerRef: React.RefObject<HTMLDivElement | null>;
  /** Series keys driving y-domain and tooltip (Line / Area / SeriesBar configs). */
  lines: LineConfig[];
  /** SVG clipPath id for grow animation. */
  clipPathId: string;
  /** Optional ComposedChart bar layout (forwarded into context). */
  composedBarDataKeys?: string[];
  composedBarSize?: number;
  composedMaxBarSize?: number;
  composedBarGap?: number;
  composedStacked?: boolean;
  composedStackOffsets?: Map<number, Map<string, number>>;
  composedStackGap?: number;
  /** When set, drives the y-axis max instead of scanning `lines` (e.g. stacked bar totals). */
  yScaleDomainMax?: number;
}

export function TimeSeriesChartInner(props: TimeSeriesChartInnerProps) {
  const { width, height } = props;
  if (width < 10 || height < 10) {
    return null;
  }
  return <TimeSeriesChartCore {...props} />;
}

const TimeSeriesChartCore = memo(function TimeSeriesChartCore({
  width,
  height,
  data,
  xDataKey,
  margin,
  animationDuration,
  animationEasing = DEFAULT_ANIMATION_EASING,
  enterTransition,
  revealSignature = "",
  children,
  containerRef,
  lines,
  clipPathId,
  composedBarDataKeys,
  composedBarSize,
  composedMaxBarSize,
  composedBarGap,
  composedStacked,
  composedStackOffsets,
  composedStackGap,
  yScaleDomainMax,
}: TimeSeriesChartInnerProps) {
  const staticPreview = useStaticChartPreview();
  const [isLoaded, setIsLoaded] = useState(staticPreview);
  const [revealEpoch, setRevealEpoch] = useState(0);

  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  const xAccessor = useCallback(
    (d: Record<string, unknown>): Date => {
      const value = d[xDataKey];
      return value instanceof Date ? value : new Date(value as string | number);
    },
    [xDataKey]
  );

  const bisectDate = useMemo(
    () => bisector<Record<string, unknown>, Date>((d) => xAccessor(d)).left,
    [xAccessor]
  );

  const xScale = useMemo(() => {
    const timeExtent = extent(data, (d) => xAccessor(d).getTime());
    const minTime = timeExtent[0] ?? 0;
    const maxTime = timeExtent[1] ?? minTime;

    return scaleTime({
      range: [0, innerWidth],
      domain: [minTime, maxTime],
    });
  }, [innerWidth, data, xAccessor]);

  const renderData = useMemo(() => {
    const valueKeys = lines.map((line) => line.dataKey);
    return decimateTimeSeries(
      data,
      maxRenderPointsForWidth(innerWidth),
      valueKeys
    );
  }, [data, innerWidth, lines]);

  const columnWidth = useMemo(() => {
    if (data.length < 2) {
      return 0;
    }
    return innerWidth / (data.length - 1);
  }, [innerWidth, data.length]);

  const yScale = useMemo(() => {
    const dataKeys = lines.map((line) => line.dataKey);
    const domain = resolveTimeSeriesYDomain(data, dataKeys, yScaleDomainMax);

    return scaleLinear({
      range: [innerHeight, 0],
      domain,
      nice: true,
    });
  }, [innerHeight, data, lines, yScaleDomainMax]);

  const dateLabels = useMemo(
    () => data.map((d) => shortDateFmt.format(xAccessor(d))),
    [data, xAccessor]
  );

  // biome-ignore lint/correctness/useExhaustiveDependencies: revealSignature
  useEffect(() => {
    if (staticPreview) {
      setIsLoaded(true);
      return;
    }

    setRevealEpoch((n) => n + 1);
    setIsLoaded(false);
    const timer = setTimeout(() => {
      setIsLoaded(true);
    }, animationDuration);
    return () => clearTimeout(timer);
  }, [animationDuration, revealSignature, staticPreview]);

  const canInteract = isLoaded;

  const {
    tooltipData,
    setTooltipData,
    selection,
    clearSelection,
    interactionHandlers,
    interactionStyle,
  } = useChartInteraction({
    xScale,
    yScale,
    data,
    lines,
    margin,
    xAccessor,
    bisectDate,
    canInteract,
  });

  const defsChildren: ReactElement[] = [];
  const preOverlayChildren: ReactElement[] = [];
  const postOverlayChildren: ReactElement[] = [];

  Children.forEach(children, (child, index) => {
    if (!isValidElement(child)) {
      return;
    }

    const keyedChild = ensureChildKey(child, index);

    if (isGradientDefComponent(keyedChild)) {
      defsChildren.push(keyedChild);
    } else if (isPatternDefComponent(keyedChild)) {
      // Keep pattern defs in the plot <g> (same as main) — hoisting breaks url(#id) fills.
      preOverlayChildren.push(keyedChild);
    } else if (isPostOverlayComponent(keyedChild)) {
      postOverlayChildren.push(keyedChild);
    } else {
      preOverlayChildren.push(keyedChild);
    }
  });

  const contextValue = useMemo(
    () => ({
      data,
      renderData,
      xScale,
      yScale,
      width,
      height,
      innerWidth,
      innerHeight,
      margin,
      columnWidth,
      tooltipData,
      setTooltipData,
      containerRef,
      lines,
      isLoaded,
      animationDuration,
      animationEasing,
      enterTransition,
      revealEpoch,
      xAccessor,
      dateLabels,
      selection,
      clearSelection,
      composedBarDataKeys,
      composedBarSize,
      composedMaxBarSize,
      composedBarGap,
      composedStacked,
      composedStackOffsets,
      composedStackGap,
    }),
    [
      data,
      renderData,
      xScale,
      yScale,
      width,
      height,
      innerWidth,
      innerHeight,
      margin,
      columnWidth,
      tooltipData,
      setTooltipData,
      containerRef,
      lines,
      isLoaded,
      animationDuration,
      animationEasing,
      enterTransition,
      revealEpoch,
      xAccessor,
      dateLabels,
      selection,
      clearSelection,
      composedBarDataKeys,
      composedBarSize,
      composedMaxBarSize,
      composedBarGap,
      composedStacked,
      composedStackOffsets,
      composedStackGap,
    ]
  );

  // Single shared reveal clip for every series. Replaces the per-<Line> /
  // per-<Area> `<ChartRevealClip>` motion.rects: one motion-driven attribute
  // animation instead of N, with all series referencing the same `<clipPath>`.
  // The wipe semantics (left-to-right unveil of static path geometry) are
  // identical to the previous per-series clips.
  // animationDuration === 0 truly disables the reveal (no clipPath wrapper),
  // so consumers can opt out without having to also pass enterTransition.
  const showReveal =
    !staticPreview &&
    renderData.length > 1 &&
    innerWidth > 0 &&
    animationDuration > 0;
  // If the consumer didn't pass an explicit enterTransition, derive one from
  // animationDuration so clipRevealTransition picks up the override instead
  // of falling back to its 1100ms default.
  const effectiveEnterTransition: Transition = enterTransition ?? {
    type: "tween",
    duration: animationDuration / 1000,
  };

  const revealClipPadding = useMemo(() => {
    if (!composedBarDataKeys?.length) {
      return 0;
    }
    const barWidth = computeSeriesBarWidth({
      innerWidth,
      dataLength: data.length,
      columnWidth,
      seriesCount: composedBarDataKeys.length,
      composedBarSize,
      composedMaxBarSize,
      composedBarGap,
      stacked: composedStacked,
    });
    return computeSeriesBarRevealClipPadding({
      barWidth,
      seriesCount: composedBarDataKeys.length,
      gap: composedBarGap,
      stacked: composedStacked,
    });
  }, [
    columnWidth,
    composedBarDataKeys,
    composedBarGap,
    composedBarSize,
    composedMaxBarSize,
    composedStacked,
    data.length,
    innerWidth,
  ]);

  return (
    <ChartProvider value={contextValue}>
      <svg aria-hidden="true" height={height} width={width}>
        <defs>
          {defsChildren}
          {showReveal ? (
            <ChartRevealClip
              clipPathId={clipPathId}
              enterTransition={effectiveEnterTransition}
              height={innerHeight + 20}
              padding={revealClipPadding}
              revealEpoch={revealEpoch}
              targetWidth={innerWidth}
            />
          ) : null}
        </defs>

        <rect fill="transparent" height={height} width={width} x={0} y={0} />

        <g
          {...interactionHandlers}
          style={interactionStyle}
          transform={`translate(${margin.left},${margin.top})`}
        >
          <rect
            fill="transparent"
            height={innerHeight}
            width={innerWidth}
            x={0}
            y={0}
          />

          {showReveal ? (
            <g clipPath={`url(#${clipPathId})`}>{preOverlayChildren}</g>
          ) : (
            preOverlayChildren
          )}
          {postOverlayChildren}
        </g>
      </svg>
    </ChartProvider>
  );
});
```

##### File: `src/charts/series-bar-layout.ts`

```tsx
export function computeSeriesBarWidth(input: {
  innerWidth: number;
  dataLength: number;
  columnWidth: number;
  seriesCount: number;
  composedBarSize?: number;
  composedMaxBarSize?: number;
  composedBarGap?: number;
  stacked?: boolean;
}): number {
  const {
    innerWidth,
    dataLength,
    columnWidth,
    seriesCount,
    composedBarSize,
    composedMaxBarSize,
    composedBarGap = 4,
    stacked = false,
  } = input;

  const gap = composedBarGap;
  const groupCount = stacked ? 1 : Math.max(1, seriesCount);
  let slot = columnWidth;
  if (slot <= 0) {
    slot = dataLength < 2 ? innerWidth : innerWidth / (dataLength - 1);
  }

  let width =
    composedBarSize ??
    Math.min(slot * 0.88, composedMaxBarSize ?? Number.POSITIVE_INFINITY);
  if (composedMaxBarSize != null) {
    width = Math.min(width, composedMaxBarSize);
  }
  if (groupCount > 1) {
    const maxGroup = slot * 0.92;
    const needed = groupCount * width + (groupCount - 1) * gap;
    if (needed > maxGroup && maxGroup > 0) {
      width = Math.max(4, (maxGroup - (groupCount - 1) * gap) / groupCount);
    }
  }

  return Math.max(2, width);
}

/** Half-width of the bar group at each x — used to pad reveal clips. */
export function computeSeriesBarRevealClipPadding(input: {
  barWidth: number;
  seriesCount: number;
  gap?: number;
  stacked?: boolean;
}): number {
  const { barWidth, seriesCount, gap = 4, stacked = false } = input;

  if (stacked || seriesCount <= 1) {
    return Math.ceil(barWidth / 2);
  }

  const groupWidth = seriesCount * barWidth + (seriesCount - 1) * gap;
  return Math.ceil(groupWidth / 2);
}
```

##### File: `src/charts/scatter.tsx`

```tsx
"use client";

import { useId } from "react";
import { useChartStable } from "./chart-context";
import { SeriesMarkers, type SeriesMarkersProps } from "./series-markers";

export interface ScatterProps extends Omit<SeriesMarkersProps, "animate"> {
  /** Whether to animate points with clip reveal. Default: true */
  animate?: boolean;
  /**
   * Color each dot by its vertical position using a chart-space linear gradient.
   * Lower values use `from`; higher values use `to`. Default stops: red (bottom) → green (top).
   */
  yGradient?: boolean | { from?: string; to?: string };
}

const DEFAULT_Y_GRADIENT_FROM = "var(--color-red-500)";
const DEFAULT_Y_GRADIENT_TO = "var(--color-emerald-500)";

export function Scatter({
  dataKey,
  fill,
  stroke,
  strokeWidth = 2,
  ringGap = 2,
  outlineWidth = 0,
  outlineColor,
  radius = 5,
  animate = true,
  fadeOnHover = true,
  inactiveOpacity = 0.5,
  inactiveBlur = 2,
  enterBlur = 2,
  showActiveHighlight = true,
  yGradient,
}: ScatterProps) {
  const { innerHeight } = useChartStable();

  const yGradientConfig = (() => {
    if (!yGradient) {
      return null;
    }
    if (yGradient === true) {
      return { from: DEFAULT_Y_GRADIENT_FROM, to: DEFAULT_Y_GRADIENT_TO };
    }
    return {
      from: yGradient.from ?? DEFAULT_Y_GRADIENT_FROM,
      to: yGradient.to ?? DEFAULT_Y_GRADIENT_TO,
    };
  })();

  const yGradientId = `scatter-y-gradient-${useId().replace(/:/g, "")}`;
  const gradientFill = yGradientConfig ? `url(#${yGradientId})` : undefined;

  const resolvedFill = gradientFill ?? fill;
  const resolvedStroke = stroke ?? (gradientFill ? gradientFill : undefined);

  return (
    <>
      {yGradientConfig ? (
        <defs>
          <linearGradient
            gradientUnits="userSpaceOnUse"
            id={yGradientId}
            x1={0}
            x2={0}
            y1={innerHeight}
            y2={0}
          >
            <stop offset="0%" stopColor={yGradientConfig.from} />
            <stop offset="100%" stopColor={yGradientConfig.to} />
          </linearGradient>
        </defs>
      ) : null}
      <SeriesMarkers
        animate={animate}
        dataKey={dataKey}
        enterBlur={enterBlur}
        fadeOnHover={fadeOnHover}
        fill={resolvedFill}
        inactiveBlur={inactiveBlur}
        inactiveOpacity={inactiveOpacity}
        outlineColor={outlineColor}
        outlineWidth={outlineWidth}
        radius={radius}
        ringGap={ringGap}
        showActiveHighlight={showActiveHighlight}
        stroke={resolvedStroke}
        strokeWidth={strokeWidth}
      />
    </>
  );
}

Scatter.displayName = "Scatter";

export default Scatter;
```

##### File: `src/charts/use-scatter-chart-interaction.ts`

```tsx
"use client";

import type { ScaleLinear, ScaleTime } from "d3-scale";
import { useCallback, useRef, useState } from "react";
import type { LineConfig, Margin, TooltipData } from "./chart-context";
import { localPointFromSvg } from "./scatter-svg";
import type { ChartSelection } from "./use-chart-interaction";
import { useScheduledTooltip } from "./use-scheduled-tooltip";

type XScale = ScaleTime<number, number>;
type YScale = ScaleLinear<number, number>;

interface UseScatterChartInteractionParams {
  xScale: XScale;
  yScale: YScale;
  data: Record<string, unknown>[];
  lines: LineConfig[];
  margin: Margin;
  xAccessor: (d: Record<string, unknown>) => Date;
  bisectDate: (
    data: Record<string, unknown>[],
    date: Date,
    lo: number
  ) => number;
  canInteract: boolean;
}

interface ScatterChartInteractionResult {
  tooltipData: TooltipData | null;
  setTooltipData: React.Dispatch<React.SetStateAction<TooltipData | null>>;
  selection: ChartSelection | null;
  clearSelection: () => void;
  interactionHandlers: {
    onMouseMove?: (event: React.MouseEvent<SVGGElement>) => void;
    onMouseLeave?: () => void;
    onMouseDown?: (event: React.MouseEvent<SVGGElement>) => void;
    onMouseUp?: () => void;
    onTouchStart?: (event: React.TouchEvent<SVGGElement>) => void;
    onTouchMove?: (event: React.TouchEvent<SVGGElement>) => void;
    onTouchEnd?: () => void;
  };
  interactionStyle: React.CSSProperties;
}

export function useScatterChartInteraction({
  xScale,
  yScale,
  data,
  lines,
  margin,
  xAccessor,
  bisectDate,
  canInteract,
}: UseScatterChartInteractionParams): ScatterChartInteractionResult {
  const [selection, setSelection] = useState<ChartSelection | null>(null);
  const {
    tooltipData,
    setTooltipData,
    scheduleTooltip,
    clearTooltip,
    resetTooltipDedupe,
  } = useScheduledTooltip<TooltipData>();

  const isDraggingRef = useRef(false);
  const dragStartXRef = useRef<number>(0);

  const resolveTooltipFromX = useCallback(
    (pixelX: number): TooltipData | null => {
      const x0 = xScale.invert(pixelX);
      const index = bisectDate(data, x0, 1);
      const d0 = data[index - 1];
      const d1 = data[index];

      if (!d0) {
        return null;
      }

      let d = d0;
      let finalIndex = index - 1;
      if (d1) {
        const d0Time = xAccessor(d0).getTime();
        const d1Time = xAccessor(d1).getTime();
        if (x0.getTime() - d0Time > d1Time - x0.getTime()) {
          d = d1;
          finalIndex = index;
        }
      }

      const yPositions: Record<string, number> = {};
      for (const line of lines) {
        const value = d[line.dataKey];
        if (typeof value === "number") {
          yPositions[line.dataKey] = yScale(value) ?? 0;
        }
      }

      return {
        point: d,
        index: finalIndex,
        x: xScale(xAccessor(d)) ?? 0,
        yPositions,
      };
    },
    [xScale, yScale, data, lines, xAccessor, bisectDate]
  );

  const resolveIndexFromX = useCallback(
    (pixelX: number): number => {
      const x0 = xScale.invert(pixelX);
      const index = bisectDate(data, x0, 1);
      const d0 = data[index - 1];
      const d1 = data[index];
      if (!d0) {
        return 0;
      }
      if (d1) {
        const d0Time = xAccessor(d0).getTime();
        const d1Time = xAccessor(d1).getTime();
        if (x0.getTime() - d0Time > d1Time - x0.getTime()) {
          return index;
        }
      }
      return index - 1;
    },
    [xScale, data, xAccessor, bisectDate]
  );

  const getChartX = useCallback(
    (
      event: React.MouseEvent<SVGGElement> | React.TouchEvent<SVGGElement>,
      touchIndex = 0
    ): number | null => {
      const svg = event.currentTarget.ownerSVGElement;
      let clientX: number;
      let clientY: number;

      if ("touches" in event) {
        const touch = event.touches[touchIndex];
        if (!touch) {
          return null;
        }
        clientX = touch.clientX;
        clientY = touch.clientY;
      } else {
        clientX = event.clientX;
        clientY = event.clientY;
      }

      const point = localPointFromSvg(svg, clientX, clientY);
      if (!point) {
        return null;
      }
      return point.x - margin.left;
    },
    [margin.left]
  );

  const handleMouseMove = useCallback(
    (event: React.MouseEvent<SVGGElement>) => {
      const chartX = getChartX(event);
      if (chartX === null) {
        return;
      }

      if (isDraggingRef.current) {
        const startX = Math.min(dragStartXRef.current, chartX);
        const endX = Math.max(dragStartXRef.current, chartX);
        setSelection({
          startX,
          endX,
          startIndex: resolveIndexFromX(startX),
          endIndex: resolveIndexFromX(endX),
          active: true,
        });
        return;
      }

      const tooltip = resolveTooltipFromX(chartX);
      if (tooltip) {
        scheduleTooltip(tooltip);
      }
    },
    [getChartX, resolveTooltipFromX, resolveIndexFromX, scheduleTooltip]
  );

  const handleMouseLeave = useCallback(() => {
    clearTooltip();
    if (isDraggingRef.current) {
      isDraggingRef.current = false;
    }
    setSelection(null);
  }, [clearTooltip]);

  const handleMouseDown = useCallback(
    (event: React.MouseEvent<SVGGElement>) => {
      const chartX = getChartX(event);
      if (chartX === null) {
        return;
      }
      isDraggingRef.current = true;
      dragStartXRef.current = chartX;
      clearTooltip();
      setSelection(null);
    },
    [getChartX, clearTooltip]
  );

  const handleMouseUp = useCallback(() => {
    if (isDraggingRef.current) {
      isDraggingRef.current = false;
    }
    setSelection(null);
  }, []);

  const handleTouchStart = useCallback(
    (event: React.TouchEvent<SVGGElement>) => {
      if (event.touches.length === 1) {
        event.preventDefault();
        const chartX = getChartX(event, 0);
        if (chartX === null) {
          return;
        }
        const tooltip = resolveTooltipFromX(chartX);
        if (tooltip) {
          scheduleTooltip(tooltip);
        }
      } else if (event.touches.length === 2) {
        event.preventDefault();
        resetTooltipDedupe();
        clearTooltip();
        const x0 = getChartX(event, 0);
        const x1 = getChartX(event, 1);
        if (x0 === null || x1 === null) {
          return;
        }
        const startX = Math.min(x0, x1);
        const endX = Math.max(x0, x1);
        setSelection({
          startX,
          endX,
          startIndex: resolveIndexFromX(startX),
          endIndex: resolveIndexFromX(endX),
          active: true,
        });
      }
    },
    [
      getChartX,
      resolveTooltipFromX,
      resolveIndexFromX,
      scheduleTooltip,
      resetTooltipDedupe,
      clearTooltip,
    ]
  );

  const handleTouchMove = useCallback(
    (event: React.TouchEvent<SVGGElement>) => {
      if (event.touches.length === 1) {
        event.preventDefault();
        const chartX = getChartX(event, 0);
        if (chartX === null) {
          return;
        }
        const tooltip = resolveTooltipFromX(chartX);
        if (tooltip) {
          scheduleTooltip(tooltip);
        }
      } else if (event.touches.length === 2) {
        event.preventDefault();
        const x0 = getChartX(event, 0);
        const x1 = getChartX(event, 1);
        if (x0 === null || x1 === null) {
          return;
        }
        const startX = Math.min(x0, x1);
        const endX = Math.max(x0, x1);
        setSelection({
          startX,
          endX,
          startIndex: resolveIndexFromX(startX),
          endIndex: resolveIndexFromX(endX),
          active: true,
        });
      }
    },
    [getChartX, resolveTooltipFromX, resolveIndexFromX, scheduleTooltip]
  );

  const handleTouchEnd = useCallback(() => {
    clearTooltip();
    setSelection(null);
  }, [clearTooltip]);

  const clearSelection = useCallback(() => {
    setSelection(null);
  }, []);

  const interactionHandlers = canInteract
    ? {
        onMouseMove: handleMouseMove,
        onMouseLeave: handleMouseLeave,
        onMouseDown: handleMouseDown,
        onMouseUp: handleMouseUp,
        onTouchStart: handleTouchStart,
        onTouchMove: handleTouchMove,
        onTouchEnd: handleTouchEnd,
      }
    : {};

  const interactionStyle: React.CSSProperties = {
    cursor: canInteract ? "crosshair" : "default",
    touchAction: "none",
  };

  return {
    tooltipData,
    setTooltipData,
    selection,
    clearSelection,
    interactionHandlers,
    interactionStyle,
  };
}
```

##### File: `src/charts/scatter-svg.ts`

```tsx
/** Map viewport coordinates to SVG user space (no @visx/event). */
export function localPointFromSvg(
  svg: SVGSVGElement | null,
  clientX: number,
  clientY: number
): { x: number; y: number } | null {
  if (!svg) {
    return null;
  }

  const point = svg.createSVGPoint();
  point.x = clientX;
  point.y = clientY;

  const matrix = svg.getScreenCTM();
  if (!matrix) {
    return null;
  }

  const transformed = point.matrixTransform(matrix.inverse());
  return { x: transformed.x, y: transformed.y };
}
```

---

## Utility <a name="utility"></a>

### Installation <a name="installation"></a>

How to install and configure Bklit UI in your project

- **Category**: Utility
- **Registry Key**: `installation`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/installation.json
```

---

### Skills <a name="skills"></a>

Give AI agents project-aware context for Bklit charts in your application.

- **Category**: Utility
- **Registry Key**: `skills`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/skills.json
```

---

### Theming <a name="theming"></a>

Theme your charts using CSS custom properties for consistent, customizable styling.

- **Category**: Utility
- **Registry Key**: `theming`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/theming.json
```

#### How to Use

##### Example 1

```tsx
import { chartCssVars } from "@bklitui/ui/charts";

// In your component
<rect fill={chartCssVars.indicatorColor} />
<line stroke={chartCssVars.crosshair} />
```

##### Example 2

```tsx
<motion.rect
  fill="var(--chart-indicator-color)"
  height={2}
  width={barWidth}
  x={barX}
/>
```

##### Example 3

```tsx
import { chartCssVars } from "@bklitui/ui/charts";

<motion.rect
  fill={chartCssVars.indicatorColor}
  height={2}
  width={barWidth}
  x={barX}
/>
```

---

### X Axis <a name="x-axis"></a>

Date labels for the horizontal axis in line and area charts

- **Category**: Utility
- **Registry Key**: `x-axis`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/x-axis.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@bklit/chart-context`
  - `@bklit/utils`

#### How to Use

```tsx
import { LineChart, Line, XAxis, ChartTooltip } from "@bklitui/ui/charts";

<LineChart data={data}>
  <Line dataKey="value" />
  <XAxis />
  <ChartTooltip />
</LineChart>
```

#### Props & API Reference

##### `Props` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `numTicks` | `number` | `5` | Number of ticks to show (including first and last) |
| `tickerHalfWidth` | `number` | `50` | Width of the date ticker box for fade calculation when tooltip is visible |

#### Component Source Code

##### File: `src/charts/x-axis.tsx`

```tsx
"use client";

import { memo, useEffect, useMemo, useState } from "react";
import { createPortal } from "react-dom";
import { cn } from "@/lib/utils";
import { useChart, useChartStable } from "./chart-context";
import { shortDateFmt } from "./chart-formatters";

export interface XAxisProps {
  /** Number of ticks to show (including first and last). Default: 5. Used when `tickMode` is `"domain"`. */
  numTicks?: number;
  /** Width of the date ticker box for fade calculation. Default: 50 */
  tickerHalfWidth?: number;
  /**
   * `"domain"` — evenly spaced ticks across the time domain (default).
   * `"data"` — one label per data row at its x value (better with sparse or monthly bars).
   */
  tickMode?: "domain" | "data";
}

interface XAxisLabelProps {
  label: string;
  x: number;
  crosshairX: number | null;
  isHovering: boolean;
  tickerHalfWidth: number;
}

function XAxisLabel({
  label,
  x,
  crosshairX,
  isHovering,
  tickerHalfWidth,
}: XAxisLabelProps) {
  const fadeBuffer = 20;
  const fadeRadius = tickerHalfWidth + fadeBuffer;

  let opacity = 1;
  if (isHovering && crosshairX !== null) {
    const distance = Math.abs(x - crosshairX);
    if (distance < tickerHalfWidth) {
      opacity = 0;
    } else if (distance < fadeRadius) {
      opacity = (distance - tickerHalfWidth) / fadeBuffer;
    }
  }

  // Zero-width container approach for perfect centering
  // The wrapper is positioned exactly at x with width:0
  // The inner span overflows and is centered via text-align
  return (
    <div
      className="absolute"
      style={{
        left: x,
        bottom: 12,
        width: 0,
        display: "flex",
        justifyContent: "center",
      }}
    >
      <span
        className={cn("whitespace-nowrap text-chart-label text-xs")}
        style={{
          opacity,
          transition: "opacity 0.4s ease-in-out",
        }}
      >
        {label}
      </span>
    </div>
  );
}

export function XAxis(props: XAxisProps) {
  const { containerRef } = useChartStable();
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    setMounted(true);
  }, []);

  const container = containerRef.current;
  if (!(mounted && container)) {
    return null;
  }

  return <XAxisInner {...props} container={container} />;
}

const XAxisInner = memo(function XAxisInner({
  numTicks = 5,
  tickerHalfWidth = 50,
  tickMode = "domain",
  container,
}: XAxisProps & { container: HTMLDivElement }) {
  const { xScale, margin, tooltipData, data, xAccessor, dateLabels } =
    useChart();

  // Generate tick labels: evenly spaced along the domain, or one per data row
  const labelsToShow = useMemo(() => {
    if (tickMode === "data") {
      return data.map((d, i) => ({
        date: xAccessor(d),
        x: (xScale(xAccessor(d)) ?? 0) + margin.left,
        label: dateLabels[i] ?? shortDateFmt.format(xAccessor(d)),
      }));
    }

    const domain = xScale.domain();
    const startDate = domain[0];
    const endDate = domain[1];

    if (!(startDate && endDate)) {
      return [];
    }

    const startTime = startDate.getTime();
    const endTime = endDate.getTime();
    const timeRange = endTime - startTime;

    // Create evenly spaced dates from start to end
    const tickCount = Math.max(2, numTicks); // At least first and last
    const dates: Date[] = [];

    for (let i = 0; i < tickCount; i++) {
      const t = i / (tickCount - 1); // 0 to 1
      const time = startTime + t * timeRange;
      dates.push(new Date(time));
    }

    return dates.map((date) => ({
      date,
      x: (xScale(date) ?? 0) + margin.left,
      label: shortDateFmt.format(date),
    }));
  }, [tickMode, data, xAccessor, xScale, margin.left, dateLabels, numTicks]);

  const isHovering = tooltipData !== null;
  const crosshairX = tooltipData ? tooltipData.x + margin.left : null;

  return createPortal(
    <div className="pointer-events-none absolute inset-0">
      {labelsToShow.map((item) => (
        <XAxisLabel
          crosshairX={crosshairX}
          isHovering={isHovering}
          key={`${item.date.getTime()}-${item.x}`}
          label={item.label}
          tickerHalfWidth={tickerHalfWidth}
          x={item.x}
        />
      ))}
    </div>,
    container
  );
});

XAxis.displayName = "XAxis";

export default XAxis;
```

---

### Y Axis <a name="y-axis"></a>

Value labels for the vertical axis in line and area charts

- **Category**: Utility
- **Registry Key**: `y-axis`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/y-axis.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@bklit/chart-context`
  - `@bklit/utils`

#### How to Use

```tsx
import { LineChart, Line, XAxis, YAxis, ChartTooltip } from "@bklitui/ui/charts";

<LineChart data={data} margin={{ left: 50 }}>
  <Line dataKey="value" />
  <YAxis />
  <XAxis />
  <ChartTooltip />
</LineChart>
```

#### Props & API Reference

##### `Props` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `numTicks` | `number` | `5` | Number of ticks to show |
| `formatLargeNumbers` | `boolean` | `true` | Format values ≥1000 as "1k", "2k", etc. |

#### Component Source Code

##### File: `src/charts/y-axis.tsx`

```tsx
"use client";

import { memo, useEffect, useMemo, useState } from "react";
import { createPortal } from "react-dom";
import { useChartStable } from "./chart-context";

export interface YAxisProps {
  /** Number of ticks to show. Default: 5 */
  numTicks?: number;
  /** Format large numbers (e.g. 1000 as "1k"). Default: true */
  formatLargeNumbers?: boolean;
  /** Custom formatter for tick labels (e.g. USD). Overrides formatLargeNumbers when set. */
  formatValue?: (value: number) => string;
}

function formatLabel(
  value: number,
  formatLargeNumbers: boolean,
  formatValue?: (value: number) => string
): string {
  if (formatValue) {
    return formatValue(value);
  }
  if (formatLargeNumbers && value >= 1000) {
    return `${(value / 1000).toFixed(0)}k`;
  }
  return String(value);
}

export function YAxis(props: YAxisProps) {
  const { containerRef } = useChartStable();
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    setMounted(true);
  }, []);

  const container = containerRef.current;
  if (!(mounted && container)) {
    return null;
  }

  return <YAxisInner {...props} container={container} />;
}

const YAxisInner = memo(function YAxisInner({
  numTicks = 5,
  formatLargeNumbers = true,
  formatValue,
  container,
}: YAxisProps & { container: HTMLDivElement }) {
  const { yScale, margin } = useChartStable();

  const ticks = useMemo(() => {
    const tickValues = yScale.ticks(numTicks);
    return tickValues.map((value) => ({
      value,
      y: (yScale(value) ?? 0) + margin.top,
      label: formatLabel(value, formatLargeNumbers, formatValue),
    }));
  }, [yScale, margin.top, numTicks, formatLargeNumbers, formatValue]);

  return createPortal(
    <div
      className="pointer-events-none absolute top-0 bottom-0"
      style={{ left: 0, width: margin.left }}
    >
      {ticks.map((tick) => (
        <div
          className="absolute right-0 flex items-center justify-end pr-2"
          key={tick.value}
          style={{ top: tick.y, transform: "translateY(-50%)" }}
        >
          <span className="text-chart-label text-xs">{tick.label}</span>
        </div>
      ))}
    </div>,
    container
  );
});

YAxis.displayName = "YAxis";

export default YAxis;
```

---

### Custom Indicator <a name="custom-indicator"></a>

Create custom tooltip indicators for charts using the useChart hook

- **Category**: Utility
- **Registry Key**: `custom-indicator`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/custom-indicator.json
```

#### How to Use

##### Example 1

```tsx
<ChartTooltip 
  showCrosshair={false}  // Hides the vertical crosshair line
  showDots={false}       // Hides the dots on bars/lines
/>
```

##### Example 2

```tsx
import { motion, useSpring } from "motion/react";
import { useEffect } from "react";

function AnimatedBarLine({
  barX,
  barTopY,
  barBottomY,
  width,
  isHovered,
}: {
  barX: number;
  barTopY: number;
  barBottomY: number;
  width: number;
  isHovered: boolean;
}) {
  // Spring animations for position and opacity
  const animatedY = useSpring(barBottomY, { stiffness: 300, damping: 30 });
  const animatedOpacity = useSpring(0, { stiffness: 300, damping: 30 });

  useEffect(() => {
    // Rise to bar top when hovered, drop to bottom when not
    animatedY.set(isHovered ? barTopY : barBottomY);
    animatedOpacity.set(isHovered ? 1 : 0);
  }, [isHovered, barTopY, barBottomY, animatedY, animatedOpacity]);

  return (
    <motion.rect
      fill="var(--chart-indicator-color)"
      height={2}
      style={{
        opacity: animatedOpacity,
        y: animatedY,
      }}
      width={width}
      x={barX}
    />
  );
}
```

##### Example 3

```tsx
import { useChart } from "@bklitui/ui/charts";

function BarHorizontalLineIndicator({ data, dataKeys }) {
  const {
    barScale,        // Scale to get x position from category
    bandWidth,       // Width of each bar group
    innerHeight,     // Chart height (for bottom position)
    yScale,          // Scale to get y position from value
    hoveredBarIndex, // Which bar group is currently hovered
    margin,          // Chart margins
    containerRef,    // Ref for portal rendering
  } = useChart();
  
  // For grouped bars, divide bandWidth by number of series
  const individualBarWidth = bandWidth / dataKeys.length;
  
  // ... render indicators
}
```

##### Example 4

```tsx
import React, { useEffect } from "react";

function BarHorizontalLineIndicator({ data, dataKeys }) {
  const { barScale, bandWidth, innerHeight, margin, containerRef, hoveredBarIndex, yScale } = useChart();
  const [mounted, setMounted] = React.useState(false);

  useEffect(() => {
    setMounted(true);
  }, []);

  const container = containerRef.current;
  if (!(mounted && container && bandWidth && barScale)) {
    return null;
  }

  const { createPortal } = require("react-dom");

  // Calculate individual bar width for grouped bars
  const barCount = dataKeys.length;
  const individualBarWidth = bandWidth / barCount;

  return createPortal(
    <svg
      aria-hidden="true"
      className="pointer-events-none absolute inset-0 z-50"
      height="100%"
      width="100%"
    >
      <g transform={`translate(${margin.left},${margin.top})`}>
        {data.map((d, i) => {
          const groupX = barScale(d.month) ?? 0;
          const isHovered = hoveredBarIndex === i;

          return dataKeys.map((dataKey, barIndex) => {
            const barTopY = yScale(d[dataKey]) ?? innerHeight;
            const barX = groupX + barIndex * individualBarWidth;

            return (
              <AnimatedBarLine
                key={`${d.month}-${dataKey}`}
                barX={barX}
                barTopY={barTopY}
                barBottomY={innerHeight}
                width={individualBarWidth}
                isHovered={isHovered}
              />
            );
          });
        })}
      </g>
    </svg>,
    container
  );
}
```

##### Example 5

```tsx
import { PatternLines } from "@bklitui/ui/charts";

<BarChart data={data} xDataKey="month" barGap={0}>
  <LinearGradient id="gradient" from="var(--chart-3)" to="transparent" />
  <PatternLines
    id="diagonalPattern"
    height={6}
    width={6}
    stroke="var(--chart-4)"
    strokeWidth={1.5}
    orientation={["diagonal"]}
  />
  <Grid horizontal />
  <Bar dataKey="revenue" fill="url(#gradient)" stroke="var(--chart-3)" />
  <Bar dataKey="cost" fill="url(#diagonalPattern)" stroke="var(--chart-4)" />
  <BarXAxis />
  <ChartTooltip showCrosshair={false} showDots={false} />
  <BarHorizontalLineIndicator data={data} dataKeys={["revenue", "cost"]} />
</BarChart>
```

##### Example 6

```tsx
// Use chartCssVars or CSS variables directly
<motion.rect fill="var(--chart-indicator-color)" />
```

---

### Grid <a name="grid"></a>

A customizable grid component for charts with horizontal and vertical lines, fade effects, and dashed styling

- **Category**: Utility
- **Registry Key**: `grid`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/grid.json
```

#### Dependencies

- **NPM Packages**:
  - `@visx/grid@4.0.1-alpha.0`
- **Local Registry Dependencies**:
  - `@bklit/chart-context`

#### How to Use

##### Example 1

```tsx
import { LineChart, Line, Grid, ChartTooltip } from "@bklitui/ui/charts";

<LineChart data={data}>
  <Grid horizontal />
  <Line dataKey="value" />
  <ChartTooltip />
</LineChart>
```

##### Example 2

```tsx
<LineChart data={data}>
  <Grid
    horizontal
    vertical
    stroke="var(--border)"
    strokeOpacity={0.5}
    strokeWidth={0.5}
    strokeDasharray=""
    fadeHorizontal={false}
    fadeVertical={false}
  />
  <Line dataKey="value" />
</LineChart>
```

#### Props & API Reference

##### `Props` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `horizontal` | `boolean` | `true` | Show horizontal grid lines |
| `vertical` | `boolean` | `false` | Show vertical grid lines |
| `numTicksRows` | `number` | `5` | Number of horizontal lines |
| `numTicksColumns` | `number` | `10` | Number of vertical lines |
| `rowTickValues` | `number[]` | - | Explicit tick values for horizontal grid lines. When set, overrides `numTicksRows`. Use with Live Line Chart so grid rows align with `LiveYAxis` labels. |
| `stroke` | `string` | `var(--chart-grid)` | Line color |
| `strokeOpacity` | `number` | `1` | Line opacity |
| `strokeWidth` | `number` | `1` | Line width |
| `strokeDasharray` | `string` | `"4,4"` | Dash pattern (empty string for solid) |
| `highlightRowValues` | `number[]` | — | Emphasize horizontal lines at specific y values (e.g. `[0]` for break-even) |
| `highlightRowStroke` | `string` | `var(--chart-foreground-muted)` | Stroke for highlighted rows |
| `highlightRowStrokeOpacity` | `number` | `1` | Opacity for highlighted rows |
| `highlightRowStrokeWidth` | `number` | `1` | Width for highlighted rows |
| `highlightRowStrokeDasharray` | `string` | `"0"` | Dash pattern for highlighted rows |
| `fadeHorizontal` | `boolean` | `true` | Fade horizontal lines at left/right edges |
| `fadeVertical` | `boolean` | `false` | Fade vertical lines at top/bottom edges |

#### Component Source Code

##### File: `src/charts/grid.tsx`

```tsx
"use client";

import { GridColumns, GridRows } from "@visx/grid";
import { useId } from "react";
import { chartCssVars, useChartStable } from "./chart-context";

export interface GridProps {
  /** Show horizontal grid lines. Default: true */
  horizontal?: boolean;
  /** Show vertical grid lines. Default: false */
  vertical?: boolean;
  /** Number of horizontal grid lines. Default: 5 */
  numTicksRows?: number;
  /** Number of vertical grid lines. Default: 10 */
  numTicksColumns?: number;
  /** Explicit tick values for horizontal grid lines. Overrides numTicksRows. */
  rowTickValues?: number[];
  /** Grid line stroke color. Default: var(--chart-grid) */
  stroke?: string;
  /** Grid line stroke opacity. Default: 1 */
  strokeOpacity?: number;
  /** Grid line stroke width. Default: 1 */
  strokeWidth?: number;
  /** Grid line dash array. Default: "4,4" for dashed lines */
  strokeDasharray?: string;
  /** Horizontal row values rendered with alternate styling (e.g. zero baseline). */
  highlightRowValues?: number[];
  /** Stroke for highlighted rows. Default: var(--chart-foreground-muted) */
  highlightRowStroke?: string;
  /** Stroke opacity for highlighted rows. Default: 1 */
  highlightRowStrokeOpacity?: number;
  /** Stroke width for highlighted rows. Default: 1 */
  highlightRowStrokeWidth?: number;
  /** Dash array for highlighted rows. Default: solid line */
  highlightRowStrokeDasharray?: string;
  /** Enable horizontal fade effect on grid rows (fades at left/right). Default: true */
  fadeHorizontal?: boolean;
  /** Enable vertical fade effect on grid columns (fades at top/bottom). Default: false */
  fadeVertical?: boolean;
}

export function Grid({
  horizontal = true,
  vertical = false,
  numTicksRows = 5,
  numTicksColumns = 10,
  rowTickValues,
  stroke = chartCssVars.grid,
  strokeOpacity = 1,
  strokeWidth = 1,
  strokeDasharray = "4,4",
  highlightRowValues,
  highlightRowStroke = chartCssVars.foregroundMuted,
  highlightRowStrokeOpacity = 1,
  highlightRowStrokeWidth = 1,
  highlightRowStrokeDasharray = "0",
  fadeHorizontal = true,
  fadeVertical = false,
}: GridProps) {
  const { xScale, yScale, innerWidth, innerHeight, orientation, barScale } =
    useChartStable();

  // For bar charts, determine which scale to use for grid lines
  // Horizontal bar charts: vertical grid should use yScale (value scale)
  // Vertical bar charts: horizontal grid uses yScale (value scale)
  const isHorizontalBarChart = orientation === "horizontal" && barScale;

  // For vertical grid lines in horizontal bar charts, use yScale (the value scale)
  // For time-based charts, use xScale
  const columnScale = isHorizontalBarChart ? yScale : xScale;
  const uniqueId = useId();

  // Horizontal fade mask (for grid rows - fades left/right)
  const hMaskId = `grid-rows-fade-${uniqueId}`;
  const hGradientId = `${hMaskId}-gradient`;

  // Vertical fade mask (for grid columns - fades top/bottom)
  const vMaskId = `grid-cols-fade-${uniqueId}`;
  const vGradientId = `${vMaskId}-gradient`;

  return (
    <g className="chart-grid">
      {/* Gradient mask for horizontal grid lines - fades at left/right */}
      {horizontal && fadeHorizontal && (
        <defs>
          <linearGradient id={hGradientId} x1="0%" x2="100%" y1="0%" y2="0%">
            <stop offset="0%" style={{ stopColor: "white", stopOpacity: 0 }} />
            <stop offset="10%" style={{ stopColor: "white", stopOpacity: 1 }} />
            <stop offset="90%" style={{ stopColor: "white", stopOpacity: 1 }} />
            <stop
              offset="100%"
              style={{ stopColor: "white", stopOpacity: 0 }}
            />
          </linearGradient>
          <mask id={hMaskId}>
            <rect
              fill={`url(#${hGradientId})`}
              height={innerHeight}
              width={innerWidth}
              x="0"
              y="0"
            />
          </mask>
        </defs>
      )}

      {/* Gradient mask for vertical grid lines - fades at top/bottom */}
      {vertical && fadeVertical && (
        <defs>
          <linearGradient id={vGradientId} x1="0%" x2="0%" y1="0%" y2="100%">
            <stop offset="0%" style={{ stopColor: "white", stopOpacity: 0 }} />
            <stop offset="10%" style={{ stopColor: "white", stopOpacity: 1 }} />
            <stop offset="90%" style={{ stopColor: "white", stopOpacity: 1 }} />
            <stop
              offset="100%"
              style={{ stopColor: "white", stopOpacity: 0 }}
            />
          </linearGradient>
          <mask id={vMaskId}>
            <rect
              fill={`url(#${vGradientId})`}
              height={innerHeight}
              width={innerWidth}
              x="0"
              y="0"
            />
          </mask>
        </defs>
      )}

      {horizontal && (
        <g mask={fadeHorizontal ? `url(#${hMaskId})` : undefined}>
          <GridRows
            numTicks={rowTickValues ? undefined : numTicksRows}
            scale={yScale}
            stroke={stroke}
            strokeDasharray={strokeDasharray}
            strokeOpacity={strokeOpacity}
            strokeWidth={strokeWidth}
            tickValues={rowTickValues}
            width={innerWidth}
          />
        </g>
      )}
      {horizontal && highlightRowValues && highlightRowValues.length > 0 ? (
        <g className="chart-grid-highlight-rows">
          {highlightRowValues.map((value) => {
            const y = yScale(value);
            if (y == null || !Number.isFinite(y)) {
              return null;
            }

            return (
              <line
                key={value}
                stroke={highlightRowStroke}
                strokeDasharray={highlightRowStrokeDasharray}
                strokeOpacity={highlightRowStrokeOpacity}
                strokeWidth={highlightRowStrokeWidth}
                x1={0}
                x2={innerWidth}
                y1={y}
                y2={y}
              />
            );
          })}
        </g>
      ) : null}
      {vertical && columnScale && typeof columnScale === "function" && (
        <g mask={fadeVertical ? `url(#${vMaskId})` : undefined}>
          <GridColumns
            height={innerHeight}
            numTicks={numTicksColumns}
            scale={columnScale}
            stroke={stroke}
            strokeDasharray={strokeDasharray}
            strokeOpacity={strokeOpacity}
            strokeWidth={strokeWidth}
          />
        </g>
      )}
    </g>
  );
}

Grid.displayName = "Grid";

export default Grid;
```

---

### Legend <a name="legend"></a>

A composable legend component for charts with progress bars, markers, and customizable layouts

- **Category**: Utility
- **Registry Key**: `legend`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/legend.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`
  - `@number-flow/react`
- **Local Registry Dependencies**:
  - `@bklit/utils`
  - `@bklit/chart-utils`

#### How to Use

##### Example 1

```tsx
const data = [
  { label: "Organic", value: 4250, maxValue: 5000, color: "#0ea5e9" },
  { label: "Paid", value: 3120, maxValue: 5000, color: "#a855f7" },
];

<Legend items={data} title="Traffic Sources">
  <LegendItemComponent>
    <LegendMarker />
    <LegendLabel />
    <LegendValue />
  </LegendItemComponent>
</Legend>
```

##### Example 2

```tsx
<Legend items={revenueData}>
  <LegendItemComponent className="flex items-center gap-3">
    <LegendMarker />
    <LegendLabel className="flex-1" />
    <LegendValue 
      formatValue={(v) => `$${(v / 1000).toFixed(0)}k`}
      showPercentage
      formatPercentage={(p) => `(${p.toFixed(1)}%)`}
    />
  </LegendItemComponent>
</Legend>
```

##### Example 3

```tsx
import { useState } from "react";
import { RingChart, Ring, RingCenter, Legend, LegendItemComponent, LegendMarker, LegendLabel, LegendValue } from "@bklitui/ui/charts";

function SyncedChart() {
  const [hoveredIndex, setHoveredIndex] = useState<number | null>(null);

  return (
    <div className="flex items-center gap-8">
      <RingChart 
        data={data} 
        hoveredIndex={hoveredIndex}
        onHoverChange={setHoveredIndex}
      >
        {data.map((_, i) => <Ring key={i} index={i} />)}
        <RingCenter />
      </RingChart>

      <Legend 
        items={data}
        hoveredIndex={hoveredIndex}
        onHoverChange={setHoveredIndex}
      >
        <LegendItemComponent className="flex items-center gap-3">
          <LegendMarker />
          <LegendLabel className="flex-1" />
          <LegendValue />
        </LegendItemComponent>
      </Legend>
    </div>
  );
}
```

##### Example 4

```tsx
import { useLegend } from "@bklitui/ui/charts";

function CustomComponent() {
  const { items, hoveredIndex, setHoveredIndex } = useLegend();
  // ...
}
```

##### Example 5

```tsx
import { useLegendItem } from "@bklitui/ui/charts";

function CustomItemContent() {
  const { item, index, isHovered, isFaded, percentage } = useLegendItem();
  // ...
}
```

#### Props & API Reference

##### `Legend` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `items` | `LegendItemData[]` | required | Array of legend items |
| `hoveredIndex` | `number \| null` | - | Controlled hover state |
| `onHoverChange` | `(index: number \| null) => void` | - | Hover callback |
| `title` | `string` | - | Title above the legend |
| `titleClassName` | `string` | `"text-sm font-semibold"` | Title styling |
| `className` | `string` | `""` | Container class |

##### `LegendItemComponent` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `className` | `string` | `""` | Item container class |

##### `LegendMarker` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `className` | `string` | `"h-2.5 w-2.5"` | Size and styling |

##### `LegendLabel` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `className` | `string` | `"text-sm font-medium"` | Label styling |

##### `LegendValue` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `className` | `string` | `"text-sm tabular-nums"` | Value styling |
| `showPercentage` | `boolean` | `false` | Show percentage |
| `percentageClassName` | `string` | `"text-xs tabular-nums"` | Percentage styling |
| `formatValue` | `(value: number) => string` | `toLocaleString()` | Value formatter |
| `formatPercentage` | `(percentage: number) => string` | `${p.toFixed(0)}%` | Percentage formatter |

##### `LegendProgress` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `height` | `string` | `"h-1.5"` | Track height class |
| `trackClassName` | `string` | `""` | Track styling |
| `indicatorClassName` | `string` | `""` | Indicator styling |

#### Component Source Code

##### File: `src/charts/legend/legend.tsx`

```tsx
"use client";

import {
  cloneElement,
  isValidElement,
  type ReactElement,
  useState,
} from "react";
import { cn } from "@/lib/utils";
import {
  type LegendItemData,
  LegendItemProvider,
  LegendProvider,
} from "./legend-context";

export interface LegendProps {
  /** Legend items data */
  items: LegendItemData[];
  /** Controlled hover state */
  hoveredIndex?: number | null;
  /** Hover state change callback */
  onHoverChange?: (index: number | null) => void;
  /** Title shown above the legend */
  title?: string;
  /** Title class name */
  titleClassName?: string;
  /** Container class name */
  className?: string;
  /** Children - should contain a single LegendItem that will be mapped for each item */
  children: ReactElement;
}

export function Legend({
  items,
  hoveredIndex: controlledHoveredIndex,
  onHoverChange,
  title,
  titleClassName = "text-sm font-semibold",
  className = "",
  children,
}: LegendProps) {
  const [internalHoveredIndex, setInternalHoveredIndex] = useState<
    number | null
  >(null);

  // Controlled or uncontrolled hover state
  const isControlled = controlledHoveredIndex !== undefined;
  const hoveredIndex = isControlled
    ? controlledHoveredIndex
    : internalHoveredIndex;
  const setHoveredIndex = (index: number | null) => {
    if (isControlled) {
      onHoverChange?.(index);
    } else {
      setInternalHoveredIndex(index);
    }
  };

  const contextValue = {
    items,
    hoveredIndex,
    setHoveredIndex,
  };

  return (
    <LegendProvider value={contextValue}>
      <div className={cn("legend-container flex flex-col gap-2", className)}>
        {title && (
          <h3 className={cn("mb-1 text-legend-foreground", titleClassName)}>
            {title}
          </h3>
        )}
        {items.map((item, index) => {
          const isHovered = hoveredIndex === index;
          const isFaded = hoveredIndex !== null && hoveredIndex !== index;
          const percentage = item.maxValue
            ? (item.value / item.maxValue) * 100
            : 0;

          const itemContext = {
            item,
            index,
            isHovered,
            isFaded,
            percentage,
          };

          // Clone the child element for each item
          if (isValidElement(children)) {
            return (
              <LegendItemProvider key={item.label} value={itemContext}>
                {cloneElement(children)}
              </LegendItemProvider>
            );
          }

          return null;
        })}
      </div>
    </LegendProvider>
  );
}

Legend.displayName = "Legend";
```

##### File: `src/charts/legend/legend-context.tsx`

```tsx
"use client";

import { createContext, useContext } from "react";

// CSS variable references for legend theming
export const legendCssVars = {
  background: "var(--legend)",
  foreground: "var(--legend-foreground)",
  muted: "var(--legend-muted)",
  mutedForeground: "var(--legend-muted-foreground)",
  track: "var(--legend-track)",
};

export interface LegendItemData {
  /** Display label */
  label: string;
  /** Current value */
  value: number;
  /** Maximum value (for progress/percentage calculation) */
  maxValue?: number;
  /** Item color */
  color: string;
}

export interface LegendContextValue {
  /** All legend items */
  items: LegendItemData[];
  /** Currently hovered index */
  hoveredIndex: number | null;
  /** Set hovered index */
  setHoveredIndex: (index: number | null) => void;
}

export interface LegendItemContextValue {
  /** The current item data */
  item: LegendItemData;
  /** Index of this item */
  index: number;
  /** Whether this item is hovered */
  isHovered: boolean;
  /** Whether this item is faded (another item is hovered) */
  isFaded: boolean;
  /** Percentage value (value / maxValue * 100) */
  percentage: number;
}

const LegendContext = createContext<LegendContextValue | null>(null);
const LegendItemContext = createContext<LegendItemContextValue | null>(null);

export function LegendProvider({
  children,
  value,
}: {
  children: React.ReactNode;
  value: LegendContextValue;
}) {
  return (
    <LegendContext.Provider value={value}>{children}</LegendContext.Provider>
  );
}

export function LegendItemProvider({
  children,
  value,
}: {
  children: React.ReactNode;
  value: LegendItemContextValue;
}) {
  return (
    <LegendItemContext.Provider value={value}>
      {children}
    </LegendItemContext.Provider>
  );
}

export function useLegend(): LegendContextValue {
  const context = useContext(LegendContext);
  if (!context) {
    throw new Error("useLegend must be used within a <Legend> component.");
  }
  return context;
}

export function useLegendItem(): LegendItemContextValue {
  const context = useContext(LegendItemContext);
  if (!context) {
    throw new Error(
      "useLegendItem must be used within a <LegendItem> component."
    );
  }
  return context;
}
```

##### File: `src/charts/legend/legend-item.tsx`

```tsx
"use client";

import type { ReactNode } from "react";
import { cn } from "@/lib/utils";
import { useLegend, useLegendItem } from "./legend-context";

export interface LegendItemProps {
  /** Container class name */
  className?: string;
  /** Children components (LegendMarker, LegendLabel, LegendValue, LegendProgress) */
  children: ReactNode;
}

export function LegendItem({ className = "", children }: LegendItemProps) {
  const { setHoveredIndex } = useLegend();
  const { index, isHovered } = useLegendItem();

  return (
    // biome-ignore lint/a11y/noNoninteractiveElementInteractions: Legend item hover interaction
    // biome-ignore lint/a11y/noStaticElementInteractions: Legend item hover interaction
    <div
      className={cn(
        "cursor-pointer rounded-lg px-2 py-1.5 transition-all duration-150 ease-out",
        isHovered && "bg-legend-muted",
        className
      )}
      data-hovered={isHovered ? "" : undefined}
      onMouseEnter={() => setHoveredIndex(index)}
      onMouseLeave={() => setHoveredIndex(null)}
    >
      {children}
    </div>
  );
}

LegendItem.displayName = "LegendItem";
```

##### File: `src/charts/legend/legend-label.tsx`

```tsx
"use client";

import { cn } from "@/lib/utils";
import { useLegendItem } from "./legend-context";

export interface LegendLabelProps {
  /** Label class name. Default: "text-sm font-medium" */
  className?: string;
}

export function LegendLabel({
  className = "text-sm font-medium",
}: LegendLabelProps) {
  const { item } = useLegendItem();

  return (
    <span className={cn("text-legend-foreground", className)}>
      {item.label}
    </span>
  );
}

LegendLabel.displayName = "LegendLabel";
```

##### File: `src/charts/legend/legend-marker.tsx`

```tsx
"use client";

import { cn } from "@/lib/utils";
import { useLegendItem } from "./legend-context";

export interface LegendMarkerProps {
  /** Marker size class. Default: "h-2.5 w-2.5" */
  className?: string;
}

export function LegendMarker({ className = "h-2.5 w-2.5" }: LegendMarkerProps) {
  const { item } = useLegendItem();

  // Note: backgroundColor must remain inline style as item.color is dynamic data
  return (
    <div
      className={cn("shrink-0 rounded-full", className)}
      style={{ backgroundColor: item.color }}
    />
  );
}

LegendMarker.displayName = "LegendMarker";
```

##### File: `src/charts/legend/legend-progress.tsx`

```tsx
"use client";

import { Progress } from "@base-ui/react/progress";
import { cn } from "@/lib/utils";
import { useLegendItem } from "./legend-context";

export interface LegendProgressProps {
  /** Track class name */
  trackClassName?: string;
  /** Indicator class name */
  indicatorClassName?: string;
  /** Track height. Default: "h-1.5" */
  height?: string;
}

export function LegendProgress({
  trackClassName = "",
  indicatorClassName = "",
  height = "h-1.5",
}: LegendProgressProps) {
  const { item } = useLegendItem();

  if (!item.maxValue) {
    return null;
  }

  // Note: item.color must remain inline style as it's dynamic data
  return (
    <Progress.Root max={item.maxValue} value={item.value}>
      <Progress.Track
        className={cn(
          "w-full overflow-hidden rounded-full bg-legend-track",
          height,
          trackClassName
        )}
      >
        <Progress.Indicator
          className={cn(
            "h-full rounded-full transition-all duration-500",
            indicatorClassName
          )}
          style={{ backgroundColor: item.color }}
        />
      </Progress.Track>
    </Progress.Root>
  );
}

LegendProgress.displayName = "LegendProgress";
```

##### File: `src/charts/legend/legend-value.tsx`

```tsx
"use client";

import { cn } from "@/lib/utils";
import { intFmt } from "../chart-formatters";
import { useLegendItem } from "./legend-context";

export interface LegendValueProps {
  /** Value class name. Default: "text-sm tabular-nums" */
  className?: string;
  /** Show percentage alongside value. Default: false */
  showPercentage?: boolean;
  /** Percentage class name. Default: "text-xs tabular-nums" */
  percentageClassName?: string;
  /** Format function for the value. Default: toLocaleString() */
  formatValue?: (value: number) => string;
  /** Format function for percentage. Default: (p) => `${p.toFixed(0)}%` */
  formatPercentage?: (percentage: number) => string;
}

export function LegendValue({
  className = "text-sm tabular-nums",
  showPercentage = false,
  percentageClassName = "text-xs tabular-nums",
  formatValue = intFmt,
  formatPercentage = (p) => `${p.toFixed(0)}%`,
}: LegendValueProps) {
  const { item, percentage } = useLegendItem();

  return (
    <span
      className={cn(
        "flex items-center gap-2 text-legend-muted-foreground",
        className
      )}
    >
      <span>{formatValue(item.value)}</span>
      {showPercentage && item.maxValue && (
        <span className={percentageClassName}>
          {formatPercentage(percentage)}
        </span>
      )}
    </span>
  );
}

LegendValue.displayName = "LegendValue";
```

##### File: `src/charts/legend/index.ts`

```tsx
// Legend context and hooks

// Legend components
export { Legend, type LegendProps } from "./legend";
export {
  type LegendContextValue,
  type LegendItemContextValue,
  type LegendItemData,
  legendCssVars,
  useLegend,
  useLegendItem,
} from "./legend-context";
export { LegendItem, type LegendItemProps } from "./legend-item";
export { LegendLabel, type LegendLabelProps } from "./legend-label";
export { LegendMarker, type LegendMarkerProps } from "./legend-marker";
export { LegendProgress, type LegendProgressProps } from "./legend-progress";
export { LegendValue, type LegendValueProps } from "./legend-value";
```

---

### Tooltip <a name="tooltip"></a>

An interactive tooltip component for charts with crosshair, dots, date picker, and customizable content

- **Category**: Utility
- **Registry Key**: `tooltip`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/tooltip.json
```

#### How to Use

```tsx
import { LineChart, Line, Grid, ChartTooltip } from "@bklitui/ui/charts";

<LineChart data={data}>
  <Grid horizontal />
  <Line dataKey="value" />
  <ChartTooltip />
</LineChart>
```

#### Props & API Reference

##### `Props` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showDatePill` | `boolean` | `true` | Show animated date ticker at bottom |
| `showCrosshair` | `boolean` | `true` | Show vertical crosshair line |
| `showDots` | `boolean` | `true` | Show dots on data points |
| `indicatorColor` | `string \| (point) => string` | — | Crosshair and dot color; pass a function for dynamic colors |
| `content` | `(props) => ReactNode` | - | Custom content renderer |
| `rows` | `(point) => TooltipRow[]` | - | Custom row generator |
| `children` | `ReactNode` | - | Additional content (e.g., markers) |
| `className` | `string` | `""` | Additional CSS class |

---

### useChart Hook <a name="use-chart"></a>

Access chart state and context for building custom chart components

- **Category**: Utility
- **Registry Key**: `use-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.bklit.com/r/use-chart.json
```

#### How to Use

##### Example 1

```tsx
import { useChart } from "@bklitui/ui/charts";

function MyCustomComponent() {
  const { tooltipData, hoveredBarIndex, yScale, innerHeight } = useChart();
  // ... use chart state
}
```

##### Example 2

```tsx
// Correct: Inside a chart
<BarChart data={data} xDataKey="month">
  <MyCustomComponent /> {/* useChart works here */}
</BarChart>

// Error: Outside a chart
<MyCustomComponent /> {/* useChart will throw */}
```

##### Example 3

```tsx
interface TooltipData {
  point: Record<string, unknown>;  // The data point being hovered
  index: number;                    // Index in the data array
  x: number;                        // X position in pixels
  yPositions: Record<string, number>;  // Y positions keyed by dataKey
  xPositions?: Record<string, number>; // X positions (grouped bars)
}
```

##### Example 4

```tsx
function HoverIndicator() {
  const { tooltipData, innerHeight, margin } = useChart();
  
  if (!tooltipData) return null;
  
  return (
    <div 
      style={{ 
        position: 'absolute',
        left: tooltipData.x + margin.left,
        top: margin.top,
        height: innerHeight,
      }}
    >
      {/* Custom indicator */}
    </div>
  );
}
```

##### Example 5

```tsx
function BarOverlay() {
  const { barScale, bandWidth, hoveredBarIndex, data } = useChart();
  
  if (!barScale || hoveredBarIndex === null) return null;
  
  const hoveredData = data[hoveredBarIndex];
  const x = barScale(hoveredData.category);
  
  return (
    <rect x={x} width={bandWidth} /* ... */ />
  );
}
```

##### Example 6

```tsx
function CustomMarker({ value, category }) {
  const { yScale, barScale, innerHeight } = useChart();
  
  const y = yScale(value);
  const x = barScale?.(category) ?? 0;
  
  return (
    <circle cx={x} cy={y} r={5} fill="red" />
  );
}
```

---
