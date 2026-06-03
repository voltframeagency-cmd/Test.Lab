# BoldKit Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **BoldKit** (boldkit.dev) — a library of bold, raw, and beautiful neubrutalist UI components, charts, hooks, and mathematical backgrounds designed with Tailwind CSS, TypeScript, Radix, and Lucide Icons.

## Table of Contents

- [Components](#components)
  - [Error Boundary](#error-boundary)
- [Hooks](#hooks)
  - [Use Theme](#use-theme)
- [Libraries](#libraries)
  - [Math Curves](#math-curves)
  - [Utils](#utils)
- [UI Components](#ui-components)
  - [Accordion](#accordion)
  - [Alert](#alert)
  - [Alert Dialog](#alert-dialog)
  - [Ascii Shapes](#ascii-shapes)
  - [Aspect Ratio](#aspect-ratio)
  - [Avatar](#avatar)
  - [Badge](#badge)
  - [Breadcrumb](#breadcrumb)
  - [Button](#button)
  - [Calendar](#calendar)
  - [Card](#card)
  - [Carousel](#carousel)
  - [Chart](#chart)
  - [Checkbox](#checkbox)
  - [Collapsible](#collapsible)
  - [Combobox](#combobox)
  - [Command](#command)
  - [Data Table](#data-table)
  - [Date Range Picker](#date-range-picker)
  - [Dialog](#dialog)
  - [Donut Chart](#donut-chart)
  - [Drawer](#drawer)
  - [Dropdown Menu](#dropdown-menu)
  - [Dropzone](#dropzone)
  - [Empty State](#empty-state)
  - [Funnel Chart](#funnel-chart)
  - [Gauge Chart](#gauge-chart)
  - [Heatmap Chart](#heatmap-chart)
  - [Hover Card](#hover-card)
  - [Input](#input)
  - [Input Otp](#input-otp)
  - [Kbd](#kbd)
  - [Label](#label)
  - [Layered Card](#layered-card)
  - [Marquee](#marquee)
  - [Math Curve Background](#math-curve-background)
  - [Math Curve Loader](#math-curve-loader)
  - [Math Curve Progress](#math-curve-progress)
  - [Pagination](#pagination)
  - [Popover](#popover)
  - [Progress](#progress)
  - [Radar Chart](#radar-chart)
  - [Radial Bar Chart](#radial-bar-chart)
  - [Radio Group](#radio-group)
  - [Rating](#rating)
  - [Sankey Chart](#sankey-chart)
  - [Scroll Area](#scroll-area)
  - [Select](#select)
  - [Separator](#separator)
  - [Shapes](#shapes)
  - [Sheet](#sheet)
  - [Sidebar](#sidebar)
  - [Skeleton](#skeleton)
  - [Slider](#slider)
  - [Sonner](#sonner)
  - [Sparkline](#sparkline)
  - [Spinner](#spinner)
  - [Stat Card](#stat-card)
  - [Stepper](#stepper)
  - [Sticker](#sticker)
  - [Switch](#switch)
  - [Table](#table)
  - [Tabs](#tabs)
  - [Tag Input](#tag-input)
  - [Textarea](#textarea)
  - [Time Picker](#time-picker)
  - [Timeline](#timeline)
  - [Toggle](#toggle)
  - [Toggle Group](#toggle-group)
  - [Tooltip](#tooltip)
  - [Tour](#tour)
  - [Tree View](#tree-view)
  - [Treemap Chart](#treemap-chart)

---

## Components <a name="components"></a>

### Error Boundary <a name="error-boundary"></a>

Error boundary wrapper with neubrutalism-styled fallback UI. Used by math-curve components to catch runtime errors gracefully.

- **Registry Name**: `@boldkit/error-boundary`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/error-boundary.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`
  - `@boldkit/card`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/default/components/ErrorBoundary.tsx`

```tsx
import { Component, type ReactNode } from 'react'
import { Button } from '@/components/ui/button'
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { AlertTriangle, RefreshCw, Home } from 'lucide-react'

interface Props {
  children: ReactNode
}

interface State {
  hasError: boolean
  error?: Error
}

export class ErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props)
    this.state = { hasError: false }
  }

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error }
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    // Log error to console in development
    if (process.env.NODE_ENV === 'development') {
      console.error('Error caught by boundary:', error, errorInfo)
    }
  }

  handleReload = () => {
    window.location.reload()
  }

  handleGoHome = () => {
    window.location.href = '/'
  }

  render() {
    if (this.state.hasError) {
      return (
        <div className="min-h-screen bg-background flex items-center justify-center p-4">
          <Card className="max-w-lg w-full">
            <CardHeader className="bg-destructive">
              <CardTitle className="flex items-center gap-3">
                <AlertTriangle className="h-6 w-6 stroke-[3]" />
                Something went wrong
              </CardTitle>
            </CardHeader>
            <CardContent className="pt-6 space-y-4">
              <p className="text-muted-foreground">
                An unexpected error occurred. This has been logged and we'll look into it.
              </p>
              {process.env.NODE_ENV === 'development' && this.state.error && (
                <pre className="bg-muted border-3 border-foreground p-4 text-sm overflow-x-auto">
                  <code>{this.state.error.message}</code>
                </pre>
              )}
              <div className="flex gap-3">
                <Button onClick={this.handleReload} className="gap-2">
                  <RefreshCw className="h-4 w-4" />
                  Reload Page
                </Button>
                <Button variant="outline" onClick={this.handleGoHome} className="gap-2">
                  <Home className="h-4 w-4" />
                  Go Home
                </Button>
              </div>
            </CardContent>
          </Card>
        </div>
      )
    }

    return this.props.children
  }
}
```

---

## Hooks <a name="hooks"></a>

### Use Theme <a name="use-theme"></a>

Theme provider hook with light/dark/system support and localStorage persistence. Used by Sonner toast for theme-aware rendering.

- **Registry Name**: `@boldkit/use-theme`
- **Type**: `registry:hook`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/use-theme.json
```

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/default/hooks/use-theme.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'

type Theme = 'light' | 'dark' | 'system'

interface ThemeProviderProps {
  children: React.ReactNode
  defaultTheme?: Theme
  storageKey?: string
}

interface ThemeProviderState {
  theme: Theme
  setTheme: (theme: Theme) => void
  resolvedTheme: 'light' | 'dark'
}

const ThemeProviderContext = React.createContext<ThemeProviderState | undefined>(
  undefined
)

export function ThemeProvider({
  children,
  defaultTheme = 'system',
  storageKey = 'boldkit-theme',
}: ThemeProviderProps) {
  const [theme, setTheme] = React.useState<Theme>(() => {
    if (typeof window !== 'undefined') {
      return (localStorage.getItem(storageKey) as Theme) || defaultTheme
    }
    return defaultTheme
  })

  const resolvedTheme = React.useMemo<'light' | 'dark'>(() => {
    if (typeof window === 'undefined') return 'light'
    if (theme === 'dark') return 'dark'
    if (theme === 'light') return 'light'
    return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light'
  }, [theme])

  React.useEffect(() => {
    const root = window.document.documentElement
    root.classList.remove('light', 'dark')
    root.classList.add(resolvedTheme)
  }, [resolvedTheme])

  const value = React.useMemo(
    () => ({
      theme,
      setTheme: (newTheme: Theme) => {
        localStorage.setItem(storageKey, newTheme)
        setTheme(newTheme)
      },
      resolvedTheme,
    }),
    [theme, resolvedTheme, storageKey]
  )

  return (
    <ThemeProviderContext.Provider value={value}>
      {children}
    </ThemeProviderContext.Provider>
  )
}

export function useTheme() {
  const context = React.useContext(ThemeProviderContext)

  if (context === undefined) {
    throw new Error('useTheme must be used within a ThemeProvider')
  }

  return context
}
```

---

## Libraries <a name="libraries"></a>

### Math Curves <a name="math-curves"></a>

Framework-agnostic math engine for parametric curve animations (rose, lissajous, butterfly, hypotrochoid, cardioid, lemniscate, fourier, spiral, heart)

- **Registry Name**: `@boldkit/math-curves`
- **Type**: `registry:lib`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/math-curves.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/default/lib/math-curves.ts`

```tsx
/**
 * math-curves.ts
 * Framework-agnostic math engine for parametric curve animations.
 * All coordinates are in [0, 100] SVG space, center at (50, 50).
 * progress: 0–1 (fraction of full curve loop)
 * detailScale: 0.52–1.0 (breathing oscillator, see getDetailScale)
 */

export type LoaderCurveKey =
  | 'rose' | 'lissajous' | 'butterfly' | 'hypotrochoid'
  | 'cardioid' | 'lemniscate' | 'fourier' | 'rose3'
  | 'astroid' | 'deltoid' | 'nephroid' | 'epicycloid'
  | 'superellipse' | 'triskelion' | 'involute'
  | 'spiral' | 'heart'

export type ProgressCurveKey =
  | 'spiral' | 'heart' | 'lissajous' | 'cardioid' | 'rose'
  | 'astroid' | 'superellipse' | 'deltoid' | 'nephroid'

export type BackgroundCurveKey =
  | 'rose' | 'lissajous' | 'fourier' | 'spiral'
  | 'triskelion' | 'involute' | 'epicycloid'

export type CurveKey = LoaderCurveKey | ProgressCurveKey | BackgroundCurveKey

type Point = { x: number; y: number }

interface CurveDefinition {
  /** t parameter range — progress 0→1 maps to tMin→tMax */
  tMin: number
  tMax: number
  /** Default breathing cycle duration in ms */
  pulseDurationMs: number
  /** Default segment count for buildPath */
  defaultSegments: number
  /** Core parametric function — returns {x, y} in [0,100] space */
  compute: (t: number, detailScale: number) => Point
  /**
   * Progress fractions (0–1 exclusive) where the curve is discontinuous.
   * buildPath will insert M (moveto) instead of L at these points.
   */
  discontinuities?: number[]
}

const CURVE_DEFS: Record<string, CurveDefinition> = {
  // 5-petal rose: r = a·cos(5θ), odd k → full flower in [0, π]
  rose: {
    tMin: 0,
    tMax: Math.PI,
    pulseDurationMs: 5000,
    defaultSegments: 300,
    compute: (t, ds) => {
      const a = 30 + ds * 8
      const r = a * Math.cos(5 * t)
      return { x: 50 + r * Math.cos(t), y: 50 + r * Math.sin(t) }
    },
  },

  // 3-petal rose: r = a·cos(3θ), odd k → full flower in [0, π]
  rose3: {
    tMin: 0,
    tMax: Math.PI,
    pulseDurationMs: 5500,
    defaultSegments: 240,
    compute: (t, ds) => {
      const a = 34 + ds * 6
      const r = a * Math.cos(3 * t)
      return { x: 50 + r * Math.cos(t), y: 50 + r * Math.sin(t) }
    },
  },

  // Lissajous figure (3:4 ratio): x = A·sin(3t + π/2), y = B·sin(4t)
  lissajous: {
    tMin: 0,
    tMax: 2 * Math.PI,
    pulseDurationMs: 4800,
    defaultSegments: 240,
    compute: (t, ds) => {
      const A = 34 + ds * 6
      const B = A * 0.86
      return { x: 50 + A * Math.sin(3 * t + Math.PI / 2), y: 50 + B * Math.sin(4 * t) }
    },
  },

  // Butterfly curve: r = e^cos(t) - 2·cos(4t) - sin^5(t/12)
  // Full closure at t = 12π (6 full rotations)
  butterfly: {
    tMin: 0,
    tMax: 12 * Math.PI,
    pulseDurationMs: 6000,
    defaultSegments: 480,
    compute: (t, ds) => {
      const r =
        Math.exp(Math.cos(t)) -
        2 * Math.cos(4 * t) -
        Math.pow(Math.sin(t / 12), 5)
      const scale = 7 + ds * 2.5
      return { x: 50 + scale * r * Math.cos(t), y: 50 + scale * r * Math.sin(t) }
    },
  },

  // Hypotrochoid (spirograph): R=5, r=3, d varies with detailScale
  // (R-r)/r = 2/3 → closes at t = 6π (3 outer rotations)
  hypotrochoid: {
    tMin: 0,
    tMax: 6 * Math.PI,
    pulseDurationMs: 5500,
    defaultSegments: 300,
    compute: (t, ds) => {
      const R = 5,
        r = 3
      const d = 3.5 + ds * 1.5 // 3.5–5.0
      const scale = 5
      const x = scale * ((R - r) * Math.cos(t) + d * Math.cos(((R - r) / r) * t))
      const y = scale * ((R - r) * Math.sin(t) - d * Math.sin(((R - r) / r) * t))
      return { x: 50 + x, y: 50 + y }
    },
  },

  // Cardioid: r = a(1 - cos t), centered in the 100×100 viewBox
  // Span on x: [50 - a, 50 + a], span on y: [50 - a, 50 + a] → fits with a ≤ 22
  cardioid: {
    tMin: 0,
    tMax: 2 * Math.PI,
    pulseDurationMs: 5000,
    defaultSegments: 300,
    compute: (t, ds) => {
      const a = 18 + ds * 4
      const r = a * (1 - Math.cos(t))
      // Offset by +a so cusp is at (50 + a, 50) and loop extends left to (50 - a, 50)
      return { x: 50 + a + r * Math.cos(t), y: 50 + r * Math.sin(t) }
    },
  },

  // Bernoulli lemniscate: x = a·cos(t)/(1+sin²t), y = a·sin(t)·cos(t)/(1+sin²t)
  lemniscate: {
    tMin: 0,
    tMax: 2 * Math.PI,
    pulseDurationMs: 4500,
    defaultSegments: 300,
    compute: (t, ds) => {
      const a = 36 + ds * 6
      const sinT = Math.sin(t)
      const denom = 1 + sinT * sinT
      return {
        x: 50 + (a * Math.cos(t)) / denom,
        y: 50 + (a * Math.sin(t) * Math.cos(t)) / denom,
      }
    },
  },

  // Fourier sum: multiple interfering harmonics
  fourier: {
    tMin: 0,
    tMax: 2 * Math.PI,
    pulseDurationMs: 6500,
    defaultSegments: 480,
    compute: (t, ds) => {
      const a1 = 20
      const a2 = 10 + ds * 6
      const a3 = 7
      const a4 = 4 + ds * 3
      const x =
        a1 * Math.cos(t) +
        a2 * Math.cos(2 * t) +
        a3 * Math.cos(3 * t) +
        a4 * Math.cos(5 * t)
      const y =
        a1 * Math.sin(t) +
        a2 * Math.sin(2 * t) -
        a3 * Math.sin(3 * t) +
        a4 * Math.sin(5 * t)
      return { x: 50 + x, y: 50 + y }
    },
  },

  // Archimedean spiral: r = b·t, progress 0→1 maps to 0 full revolutions outward
  spiral: {
    tMin: 0,
    tMax: 4 * Math.PI,
    pulseDurationMs: 5000,
    defaultSegments: 200,
    compute: (t, ds) => {
      const b = 2.5 + ds * 0.5
      const r = b * t
      return { x: 50 + r * Math.cos(t), y: 50 + r * Math.sin(t) }
    },
  },

  // Parametric heart: x = 16sin³t, y = -(13cost - 5cos2t - 2cos3t - cos4t)
  heart: {
    tMin: 0,
    tMax: 2 * Math.PI,
    pulseDurationMs: 5200,
    defaultSegments: 200,
    compute: (t, ds) => {
      const scale = 1.8 + ds * 0.4
      const x = scale * 16 * Math.pow(Math.sin(t), 3)
      const y =
        -scale *
        (13 * Math.cos(t) -
          5 * Math.cos(2 * t) -
          2 * Math.cos(3 * t) -
          Math.cos(4 * t))
      return { x: 50 + x, y: 50 + y }
    },
  },

  // Astroid (4-cusped hypocycloid): x = a·cos³t, y = a·sin³t
  astroid: {
    tMin: 0,
    tMax: 2 * Math.PI,
    pulseDurationMs: 4000,
    defaultSegments: 300,
    compute: (t, ds) => {
      const a = 38 + ds * 4
      return {
        x: 50 + a * Math.pow(Math.cos(t), 3),
        y: 50 + a * Math.pow(Math.sin(t), 3),
      }
    },
  },

  // Deltoid (3-cusped hypocycloid, R=3 r=1): x=(2cos+cos2t), y=(2sin-sin2t)
  deltoid: {
    tMin: 0,
    tMax: 2 * Math.PI,
    pulseDurationMs: 4800,
    defaultSegments: 240,
    compute: (t, ds) => {
      const scale = 11 + ds * 2
      return {
        x: 50 + scale * (2 * Math.cos(t) + Math.cos(2 * t)),
        y: 50 + scale * (2 * Math.sin(t) - Math.sin(2 * t)),
      }
    },
  },

  // Nephroid (2-cusped epicycloid, R=2 r=1): x=3cos(t)-cos(3t), y=3sin(t)-sin(3t)
  nephroid: {
    tMin: 0,
    tMax: 2 * Math.PI,
    pulseDurationMs: 5200,
    defaultSegments: 300,
    compute: (t, ds) => {
      const scale = 9 + ds * 2
      return {
        x: 50 + scale * (3 * Math.cos(t) - Math.cos(3 * t)),
        y: 50 + scale * (3 * Math.sin(t) - Math.sin(3 * t)),
      }
    },
  },

  // Epicycloid (5-cusped, R=5 r=1): x=6cos-cos6t, y=6sin-sin6t
  epicycloid: {
    tMin: 0,
    tMax: 2 * Math.PI,
    pulseDurationMs: 5000,
    defaultSegments: 300,
    compute: (t, ds) => {
      const scale = 5 + ds * 0.5
      return {
        x: 50 + scale * (6 * Math.cos(t) - Math.cos(6 * t)),
        y: 50 + scale * (6 * Math.sin(t) - Math.sin(6 * t)),
      }
    },
  },

  // Superellipse (Lamé curve): |x/a|^n + |y/a|^n = 1, n oscillates 2→4
  superellipse: {
    tMin: 0,
    tMax: 2 * Math.PI,
    pulseDurationMs: 6000,
    defaultSegments: 360,
    compute: (t, ds) => {
      const a = 36
      const n = 2 + ds * 2
      const exp = 2 / n
      const cosT = Math.cos(t)
      const sinT = Math.sin(t)
      return {
        x: 50 + a * Math.sign(cosT) * Math.pow(Math.abs(cosT), exp),
        y: 50 + a * Math.sign(sinT) * Math.pow(Math.abs(sinT), exp),
      }
    },
  },

  // Triskelion: 3 Archimedean spiral arms, 120° apart
  triskelion: {
    tMin: 0,
    tMax: 6 * Math.PI,
    pulseDurationMs: 5500,
    defaultSegments: 360,
    // Arms are discontinuous: each arm starts at center and ends at outer tip.
    // Insert M moves at 1/3 and 2/3 so arms aren't connected by straight lines.
    discontinuities: [1 / 3, 2 / 3],
    compute: (t, ds) => {
      const b = 32 + ds * 8
      const arm = Math.floor(t / (2 * Math.PI))
      const theta = t - arm * 2 * Math.PI
      const offset = (arm * 2 * Math.PI) / 3
      const r = b * (theta / (2 * Math.PI))
      return {
        x: 50 + r * Math.cos(theta + offset),
        y: 50 + r * Math.sin(theta + offset),
      }
    },
  },

  // Involute of circle: x = a(cos t + t·sin t), y = a(sin t - t·cos t)
  involute: {
    tMin: 0,
    tMax: 4 * Math.PI,
    pulseDurationMs: 5800,
    defaultSegments: 300,
    compute: (t, ds) => {
      const a = 2.5 + ds * 0.5
      return {
        x: 50 + a * (Math.cos(t) + t * Math.sin(t)),
        y: 50 + a * (Math.sin(t) - t * Math.cos(t)),
      }
    },
  },
}

/**
 * Returns {x, y} in [0, 100] coordinate space for a given curve at a progress position.
 * @param curve  - curve key (e.g. 'rose', 'lissajous')
 * @param progress - 0–1 position along the full curve loop
 * @param detailScale - breathing oscillator value 0.52–1.0, defaults to 1.0
 */
export function getPoint(curve: string, progress: number, detailScale = 1.0): Point {
  const def = CURVE_DEFS[curve]
  if (!def) return { x: 50, y: 50 }
  const t = def.tMin + progress * (def.tMax - def.tMin)
  return def.compute(t, detailScale)
}

/**
 * Returns the curve tangent angle in degrees at the given progress position.
 * Computed via finite difference: angle between (progress - ε) and (progress + ε).
 * Used to rotate the head <rect> to follow the curve direction.
 * @param curve - curve key
 * @param progress - 0–1
 * @param detailScale - defaults to 1.0
 */
export function getAngle(curve: string, progress: number, detailScale = 1.0): number {
  const EPS = 0.001
  const p1 = getPoint(curve, Math.max(0, progress - EPS), detailScale)
  const p2 = getPoint(curve, Math.min(1, progress + EPS), detailScale)
  const dx = p2.x - p1.x
  const dy = p2.y - p1.y
  // Fallback to larger epsilon when sample points are too close (e.g. spiral at progress=0)
  if (dx * dx + dy * dy < 1e-6) {
    const BIG_EPS = 0.01
    const q1 = getPoint(curve, Math.max(0, progress - BIG_EPS), detailScale)
    const q2 = getPoint(curve, Math.min(1, progress + BIG_EPS), detailScale)
    return Math.atan2(q2.y - q1.y, q2.x - q1.x) * (180 / Math.PI)
  }
  return Math.atan2(dy, dx) * (180 / Math.PI)
}

/**
 * Returns a full SVG path string (M + L commands) for the guide track.
 * Default segment counts per curve type:
 *   rose/cardioid/lemniscate/hypotrochoid = 300
 *   lissajous/butterfly/rose3 = 240 (butterfly: 480)
 *   fourier = 480
 *   spiral/heart = 200
 * @param curve - curve key
 * @param detailScale - defaults to 1.0
 * @param segments - optional override
 */
export function buildPath(curve: string, detailScale = 1.0, segments?: number): string {
  const def = CURVE_DEFS[curve]
  if (!def) return ''
  const n = segments ?? def.defaultSegments
  const discontinuities = def.discontinuities ?? []
  let d = ''
  for (let i = 0; i <= n; i++) {
    const progress = i / n
    const { x, y } = getPoint(curve, progress, detailScale)
    // Use M (moveto) at the start or at declared discontinuity points
    const isDiscontinuous = discontinuities.some(
      (dp) => Math.abs(progress - dp) < 0.5 / n
    )
    if (i === 0 || isDiscontinuous) {
      d += ` M ${x.toFixed(2)} ${y.toFixed(2)}`
    } else {
      d += ` L ${x.toFixed(2)} ${y.toFixed(2)}`
    }
  }
  return d.trim()
}

/**
 * Returns a sine-based breathing oscillator value in [0.52, 1.0].
 * Formula: 0.52 + ((sin(2π × (elapsed % pulseDurationMs) / pulseDurationMs) + 1) / 2) × 0.48
 * @param elapsed - current time in ms (e.g. performance.now())
 * @param pulseDurationMs - full breathing cycle duration
 */
export function getDetailScale(elapsed: number, pulseDurationMs: number): number {
  const phase = (elapsed % pulseDurationMs) / pulseDurationMs
  return 0.52 + ((Math.sin(2 * Math.PI * phase) + 1) / 2) * 0.48
}

/**
 * Returns the default pulse duration for a given curve key.
 * Useful for initializing the breathing animation.
 */
export function getCurvePulseDuration(curve: string): number {
  return CURVE_DEFS[curve]?.pulseDurationMs ?? 5000
}
```

---

### Utils <a name="utils"></a>

Utility functions for className merging

- **Registry Name**: `@boldkit/utils`
- **Type**: `registry:lib`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/utils.json
```

#### Dependencies

- **NPM Packages**:
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/default/lib/utils.ts`

```tsx
import { type ClassValue, clsx } from 'clsx'
import { twMerge } from 'tailwind-merge'

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

const SAFE_URL_PROTOCOLS = new Set(['http:', 'https:', 'mailto:', 'tel:'])

/** Sanitize a URL to prevent javascript: protocol injection. Returns '#' for unsafe values. */
export function safeHref(url: string | undefined): string {
  if (!url) return '#'
  if (url.startsWith('/') || url.startsWith('#')) return url
  try {
    const parsed = new URL(url, 'https://placeholder.invalid')
    return SAFE_URL_PROTOCOLS.has(parsed.protocol) ? url : '#'
  } catch {
    return '#'
  }
}

/** Sanitize a CSS value by stripping characters that could break out of a CSS declaration. */
export function sanitizeCssValue(value: string): string {
  return value.replace(/[{}<>;"'\\]/g, '')
}
```

---

## UI Components <a name="ui-components"></a>

### Accordion <a name="accordion"></a>

A vertically stacked set of interactive headings with neubrutalism styling

- **Registry Name**: `@boldkit/accordion`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/accordion.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-accordion`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/accordion-demo.tsx`

```tsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from '@/components/ui/accordion'

export default function Example() {
  return (
    <Accordion type="single" collapsible>
      <AccordionItem value="item-1">
        <AccordionTrigger>Is it accessible?</AccordionTrigger>
        <AccordionContent>
          Yes. It adheres to the WAI-ARIA design pattern.
        </AccordionContent>
      </AccordionItem>
    </Accordion>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/accordion.tsx`

```tsx
import * as React from 'react'
import * as AccordionPrimitive from '@radix-ui/react-accordion'
import { ChevronDown } from 'lucide-react'
import { cn } from '@/lib/utils'

const Accordion = AccordionPrimitive.Root

const AccordionItem = React.forwardRef<
  React.ElementRef<typeof AccordionPrimitive.Item>,
  React.ComponentPropsWithoutRef<typeof AccordionPrimitive.Item>
>(({ className, ...props }, ref) => (
  <AccordionPrimitive.Item
    ref={ref}
    className={cn('border-3 border-foreground border-b-0 last:border-b-3 shadow-[4px_4px_0px_hsl(var(--shadow-color))]', className)}
    {...props}
  />
))
AccordionItem.displayName = 'AccordionItem'

const AccordionTrigger = React.forwardRef<
  React.ElementRef<typeof AccordionPrimitive.Trigger>,
  React.ComponentPropsWithoutRef<typeof AccordionPrimitive.Trigger>
>(({ className, children, ...props }, ref) => (
  <AccordionPrimitive.Header className="flex">
    <AccordionPrimitive.Trigger
      ref={ref}
      className={cn(
        'flex flex-1 items-center justify-between bg-background py-4 px-4 font-bold uppercase tracking-wide transition-all duration-200 hover:bg-muted [&[data-state=open]]:bg-accent [&[data-state=open]>svg]:rotate-180',
        className
      )}
      {...props}
    >
      {children}
      <ChevronDown className="h-5 w-5 shrink-0 stroke-[3] transition-transform duration-200" />
    </AccordionPrimitive.Trigger>
  </AccordionPrimitive.Header>
))
AccordionTrigger.displayName = AccordionPrimitive.Trigger.displayName

const AccordionContent = React.forwardRef<
  React.ElementRef<typeof AccordionPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof AccordionPrimitive.Content>
>(({ className, children, ...props }, ref) => (
  <AccordionPrimitive.Content
    ref={ref}
    className="overflow-hidden text-sm transition-all data-[state=closed]:animate-accordion-up data-[state=open]:animate-accordion-down border-t-3 border-foreground"
    {...props}
  >
    <div className={cn('p-4', className)}>{children}</div>
  </AccordionPrimitive.Content>
))
AccordionContent.displayName = AccordionPrimitive.Content.displayName

export { Accordion, AccordionItem, AccordionTrigger, AccordionContent }
```

---

### Alert <a name="alert"></a>

Displays a callout for user attention with bold neubrutalism borders

- **Registry Name**: `@boldkit/alert`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/alert.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/alert-demo.tsx`

```tsx
import { Alert, AlertDescription, AlertTitle, AlertAction } from '@/components/ui/alert'
import { Terminal } from 'lucide-react'

export default function Example() {
  return (
    <Alert variant="warning">
      <Terminal className="h-4 w-4" />
      <AlertTitle>Session Expiring</AlertTitle>
      <AlertDescription>
        Your session will expire in 5 minutes.
      </AlertDescription>
      <AlertAction onClick={() => console.log('extend')}>
        Extend Session
      </AlertAction>
    </Alert>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/alert.tsx`

```tsx
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'

const alertVariants = cva(
  'relative w-full border-3 border-foreground p-4 shadow-[4px_4px_0px_hsl(var(--shadow-color))] animate-in fade-in-0 slide-in-from-top-2 duration-300 transition-all [&>svg~*:not([data-alert-action])]:pl-8 [&>svg~[data-alert-action]]:ml-8 [&>svg+div]:translate-y-[-3px] [&>svg]:absolute [&>svg]:left-4 [&>svg]:top-4 [&>svg]:text-foreground',
  {
    variants: {
      variant: {
        default: 'bg-background text-foreground',
        destructive: 'bg-destructive text-destructive-foreground [&>svg]:text-destructive-foreground',
        success: 'bg-success text-success-foreground',
        warning: 'bg-warning text-warning-foreground',
        info: 'bg-info text-info-foreground',
      },
    },
    defaultVariants: {
      variant: 'default',
    },
  }
)

const Alert = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement> & VariantProps<typeof alertVariants>
>(({ className, variant, ...props }, ref) => (
  <div
    ref={ref}
    role="alert"
    className={cn(alertVariants({ variant }), className)}
    {...props}
  />
))
Alert.displayName = 'Alert'

const AlertTitle = React.forwardRef<
  HTMLHeadingElement,
  React.HTMLAttributes<HTMLHeadingElement>
>(({ className, ...props }, ref) => (
  <h5
    ref={ref}
    className={cn('mb-1 font-bold uppercase tracking-wide leading-none', className)}
    {...props}
  />
))
AlertTitle.displayName = 'AlertTitle'

const AlertDescription = React.forwardRef<
  HTMLParagraphElement,
  React.HTMLAttributes<HTMLParagraphElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn('text-sm [&_p]:leading-relaxed', className)}
    {...props}
  />
))
AlertDescription.displayName = 'AlertDescription'

export interface AlertActionProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  /** Shows an inline spinner and disables the button automatically */
  loading?: boolean
}

const AlertAction = React.forwardRef<HTMLButtonElement, AlertActionProps>(
  ({ className, loading, disabled, children, ...props }, ref) => (
    <button
      ref={ref}
      data-alert-action=""
      disabled={disabled || loading}
      className={cn(
        // layout
        'mt-3 inline-flex items-center gap-1.5 max-w-full min-w-0',
        // shape — no rounded corners, thin border inheriting text color
        'rounded-none border border-current',
        // typography
        'px-4 py-1 text-xs font-bold uppercase tracking-wide',
        // transitions
        'transition-all duration-150',
        // hover
        'hover:opacity-100 hover:bg-current/10',
        // active
        'active:scale-95',
        // focus
        'focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-current focus-visible:ring-offset-1',
        // cursor + pointer-events when non-interactive
        'disabled:cursor-not-allowed disabled:pointer-events-none',
        // opacity: loading stays visible, truly-disabled fades out, default is 80%
        loading ? 'opacity-80' : disabled ? 'opacity-40' : 'opacity-80',
        className
      )}
      {...props}
    >
      {loading && (
        <span
          data-testid="alert-action-spinner"
          aria-hidden="true"
          className="h-3 w-3 shrink-0 animate-spin rounded-full border-2 border-current border-t-transparent"
        />
      )}
      <span className="inline-flex items-center gap-1.5 whitespace-nowrap overflow-hidden">{children}</span>
    </button>
  )
)
AlertAction.displayName = 'AlertAction'

export { Alert, AlertTitle, AlertDescription, AlertAction }
```

---

### Alert Dialog <a name="alert-dialog"></a>

A modal dialog for important confirmations with neubrutalism styling

- **Registry Name**: `@boldkit/alert-dialog`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/alert-dialog.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-alert-dialog`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`

#### How to Use

##### File: `registry/default/example/alert-dialog-demo.tsx`

```tsx
import {
  AlertDialog,
  AlertDialogAction,
  AlertDialogCancel,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogHeader,
  AlertDialogTitle,
  AlertDialogTrigger,
} from '@/components/ui/alert-dialog'

export default function Example() {
  return (
    <AlertDialog>
      <AlertDialogTrigger asChild>
        <Button variant="destructive">Delete</Button>
      </AlertDialogTrigger>
      <AlertDialogContent>
        <AlertDialogHeader>
          <AlertDialogTitle>Are you sure?</AlertDialogTitle>
          <AlertDialogDescription>
            This action cannot be undone.
          </AlertDialogDescription>
        </AlertDialogHeader>
        <AlertDialogFooter>
          <AlertDialogCancel>Cancel</AlertDialogCancel>
          <AlertDialogAction>Continue</AlertDialogAction>
        </AlertDialogFooter>
      </AlertDialogContent>
    </AlertDialog>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/alert-dialog.tsx`

```tsx
import * as React from 'react'
import * as AlertDialogPrimitive from '@radix-ui/react-alert-dialog'
import { cn } from '@/lib/utils'
import { buttonVariants } from '@/components/ui/button'

const AlertDialog = AlertDialogPrimitive.Root

const AlertDialogTrigger = AlertDialogPrimitive.Trigger

const AlertDialogPortal = AlertDialogPrimitive.Portal

const AlertDialogOverlay = React.forwardRef<
  React.ElementRef<typeof AlertDialogPrimitive.Overlay>,
  React.ComponentPropsWithoutRef<typeof AlertDialogPrimitive.Overlay>
>(({ className, ...props }, ref) => (
  <AlertDialogPrimitive.Overlay
    className={cn(
      'fixed inset-0 z-50 bg-black/70 data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0',
      className
    )}
    {...props}
    ref={ref}
  />
))
AlertDialogOverlay.displayName = AlertDialogPrimitive.Overlay.displayName

const AlertDialogContent = React.forwardRef<
  React.ElementRef<typeof AlertDialogPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof AlertDialogPrimitive.Content>
>(({ className, ...props }, ref) => (
  <AlertDialogPortal>
    <AlertDialogOverlay />
    <AlertDialogPrimitive.Content
      ref={ref}
      className={cn(
        'fixed left-[50%] top-[50%] z-50 grid w-full max-w-lg translate-x-[-50%] translate-y-[-50%] gap-4 border-3 border-foreground bg-background p-6 shadow-[8px_8px_0px_hsl(var(--shadow-color))] duration-200 data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[state=closed]:slide-out-to-left-1/2 data-[state=closed]:slide-out-to-top-[48%] data-[state=open]:slide-in-from-left-1/2 data-[state=open]:slide-in-from-top-[48%]',
        className
      )}
      {...props}
    />
  </AlertDialogPortal>
))
AlertDialogContent.displayName = AlertDialogPrimitive.Content.displayName

const AlertDialogHeader = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn('flex flex-col space-y-2 text-center sm:text-left', className)}
    {...props}
  />
)
AlertDialogHeader.displayName = 'AlertDialogHeader'

const AlertDialogFooter = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn(
      'flex flex-col-reverse gap-3 sm:flex-row sm:justify-end',
      className
    )}
    {...props}
  />
)
AlertDialogFooter.displayName = 'AlertDialogFooter'

const AlertDialogTitle = React.forwardRef<
  React.ElementRef<typeof AlertDialogPrimitive.Title>,
  React.ComponentPropsWithoutRef<typeof AlertDialogPrimitive.Title>
>(({ className, ...props }, ref) => (
  <AlertDialogPrimitive.Title
    ref={ref}
    className={cn('text-lg font-bold uppercase tracking-wide', className)}
    {...props}
  />
))
AlertDialogTitle.displayName = AlertDialogPrimitive.Title.displayName

const AlertDialogDescription = React.forwardRef<
  React.ElementRef<typeof AlertDialogPrimitive.Description>,
  React.ComponentPropsWithoutRef<typeof AlertDialogPrimitive.Description>
>(({ className, ...props }, ref) => (
  <AlertDialogPrimitive.Description
    ref={ref}
    className={cn('text-sm text-muted-foreground', className)}
    {...props}
  />
))
AlertDialogDescription.displayName = AlertDialogPrimitive.Description.displayName

const AlertDialogAction = React.forwardRef<
  React.ElementRef<typeof AlertDialogPrimitive.Action>,
  React.ComponentPropsWithoutRef<typeof AlertDialogPrimitive.Action>
>(({ className, ...props }, ref) => (
  <AlertDialogPrimitive.Action
    ref={ref}
    className={cn(buttonVariants(), className)}
    {...props}
  />
))
AlertDialogAction.displayName = AlertDialogPrimitive.Action.displayName

const AlertDialogCancel = React.forwardRef<
  React.ElementRef<typeof AlertDialogPrimitive.Cancel>,
  React.ComponentPropsWithoutRef<typeof AlertDialogPrimitive.Cancel>
>(({ className, ...props }, ref) => (
  <AlertDialogPrimitive.Cancel
    ref={ref}
    className={cn(buttonVariants({ variant: 'outline' }), className)}
    {...props}
  />
))
AlertDialogCancel.displayName = AlertDialogPrimitive.Cancel.displayName

export {
  AlertDialog,
  AlertDialogPortal,
  AlertDialogOverlay,
  AlertDialogTrigger,
  AlertDialogContent,
  AlertDialogHeader,
  AlertDialogFooter,
  AlertDialogTitle,
  AlertDialogDescription,
  AlertDialogAction,
  AlertDialogCancel,
}
```

---

### Ascii Shapes <a name="ascii-shapes"></a>

7 animated ASCII art components (Spiral, Rose, Wave, Vortex, Pulse, Matrix, Grid) with 5 character sets, 4 sizes, and SSR-safe static mode

- **Registry Name**: `@boldkit/ascii-shapes`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/ascii-shapes.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/ascii-shapes-demo.tsx`

```tsx
import { AsciiTorus } from "@/components/ui/ascii-shapes"

export default function AsciiShapesDemo() {
  return <AsciiTorus size="md" charset="classic" speed="normal" />
}
```

#### Component Source Code

##### File: `registry/default/ui/ascii-shapes.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'

// ============================================================================
// Types
// ============================================================================

export type AsciiSize = 'sm' | 'md' | 'lg' | 'hero'
export type AsciiCharset = 'blocks' | 'braille' | 'classic' | 'line' | 'dots'
export type AsciiSpeed = 'slow' | 'normal' | 'fast'

export interface AsciiShapeProps extends React.HTMLAttributes<HTMLPreElement> {
  size?: AsciiSize
  charset?: AsciiCharset
  color?: string
  speed?: AsciiSpeed
  animated?: boolean
  multicolor?: boolean
}

// ============================================================================
// Constants
// ============================================================================

const SIZE_MAP: Record<AsciiSize, { cols: number; rows: number }> = {
  sm:   { cols: 24,  rows: 12 },
  md:   { cols: 48,  rows: 24 },
  lg:   { cols: 72,  rows: 36 },
  hero: { cols: 120, rows: 60 },
}

const CHARSETS: Record<AsciiCharset, string[]> = {
  blocks:  [' ', '░', '▒', '▓', '█'],
  braille: [' ', '⠁', '⠃', '⠇', '⠿', '⠷'],
  classic: [' ', '.', ':', 'o', '*', '#', '@'],
  line:    [' ', '-', '/', '|', '\\', '+', 'X'],
  dots:    [' ', '.', '·', '•', '●'],
}

const SPEED_MAP: Record<AsciiSpeed, number> = {
  slow:   0.4,
  normal: 1.0,
  fast:   2.2,
}

const MULTICOLOR_PALETTE = [
  'hsl(var(--primary))',
  'hsl(var(--secondary))',
  'hsl(var(--accent))',
  'hsl(var(--warning))',
  'hsl(var(--info))',
  'hsl(var(--success))',
]

// ============================================================================
// Grid engine
// ============================================================================

function makeGrid(cols: number, rows: number): string[][] {
  return Array.from({ length: rows }, () => Array(cols).fill(' '))
}

function gridToLines(grid: string[][]): string[] {
  return grid.map((row) => row.join(''))
}

// ============================================================================
// Draw functions
// ============================================================================

function drawSpiral(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const cx = cols / 2, cy = rows / 2
  const aspect = 2.0
  const spacing = Math.min(cx * aspect, cy) * 0.35

  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      const dx = (c - cx) * aspect
      const dy = r - cy
      const dist = Math.sqrt(dx * dx + dy * dy)
      if (dist < 0.5) { grid[r][c] = chars[chars.length - 1]; continue }
      const angle = ((Math.atan2(dy, dx) + t * 0.002) % (2 * Math.PI) + 2 * Math.PI) % (2 * Math.PI)
      const armPhase = angle / (2 * Math.PI)
      const winding = Math.round(dist / spacing - armPhase)
      const nearestR = spacing * (armPhase + winding)
      const distToArm = Math.abs(dist - nearestR)
      const threshold = spacing * 0.38
      if (nearestR >= 0 && distToArm < threshold) {
        const intensity = 1 - distToArm / threshold
        grid[r][c] = chars[Math.floor(intensity * (chars.length - 1))]
      } else {
        grid[r][c] = ' '
      }
    }
  }
}

function drawRose(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const cx = cols / 2, cy = rows / 2
  const aspect = 2.0
  const k = 5
  const radius = Math.min(cx * aspect, cy) * 0.85

  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      const dx = (c - cx) * aspect
      const dy = r - cy
      const dist = Math.sqrt(dx * dx + dy * dy)
      const angle = Math.atan2(dy, dx) + t * 0.001
      const roseR = radius * Math.abs(Math.cos(k * angle))
      const distToRose = Math.abs(dist - roseR)
      const threshold = radius * 0.12
      if (distToRose < threshold) {
        const intensity = 1 - distToRose / threshold
        grid[r][c] = chars[Math.floor(intensity * (chars.length - 1))]
      } else {
        grid[r][c] = ' '
      }
    }
  }
}

function drawWave(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      const x = c / cols
      const y = r / rows
      const wave =
        0.5 * Math.sin(2 * Math.PI * (x * 2 - t * 0.001)) +
        0.3 * Math.sin(2 * Math.PI * (x * 3 - t * 0.0015)) +
        0.2 * Math.sin(2 * Math.PI * (x * 5 - t * 0.002))
      const waveY = 0.5 + wave * 0.3
      const distToWave = Math.abs(y - waveY)
      const threshold = 0.08
      if (distToWave < threshold) {
        const intensity = 1 - distToWave / threshold
        grid[r][c] = chars[Math.floor(intensity * (chars.length - 1))]
      } else {
        grid[r][c] = ' '
      }
    }
  }
}

function drawVortex(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const cx = cols / 2, cy = rows / 2
  const aspect = 2.0
  const maxR = Math.sqrt((cx * aspect) ** 2 + cy ** 2)

  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      const dx = (c - cx) * aspect
      const dy = r - cy
      const dist = Math.sqrt(dx * dx + dy * dy)
      const angle = Math.atan2(dy, dx)
      const vortexAngle = angle + dist * 0.3 - t * 0.002
      const intensity = ((Math.sin(vortexAngle * 3) + 1) / 2) * Math.exp(-dist / (maxR * 0.8))
      if (intensity > 0.08) {
        grid[r][c] = chars[Math.min(Math.floor(intensity * (chars.length - 1)), chars.length - 1)]
      } else {
        grid[r][c] = ' '
      }
    }
  }
}

function drawPulse(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const cx = cols / 2, cy = rows / 2
  const aspect = 2.0
  const speed = t * 0.02
  const ringSpacing = Math.min(cx * aspect, cy) * 0.35
  const maxR = Math.sqrt((cx * aspect) ** 2 + cy ** 2)

  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      const dx = (c - cx) * aspect
      const dy = r - cy
      const dist = Math.sqrt(dx * dx + dy * dy)
      const phase = ((dist - speed) % ringSpacing + ringSpacing) % ringSpacing
      const ringIntensity = Math.pow(Math.sin(Math.PI * phase / ringSpacing), 2)
      const fade = Math.max(0, 1 - dist / maxR)
      const intensity = ringIntensity * fade
      if (intensity > 0.05) {
        grid[r][c] = chars[Math.floor(intensity * (chars.length - 1))]
      } else {
        grid[r][c] = ' '
      }
    }
  }
}

interface ColState { offset: number; speed: number }

function makeMatrixState(cols: number): ColState[] {
  return Array.from({ length: cols }, (_, i) => ({
    offset: (i * 37 % 100),
    speed: 0.5 + (i * 13 % 10) * 0.15,
  }))
}

function drawMatrix(grid: string[][], cols: number, rows: number, t: number, chars: string[], state: ColState[]): void {
  for (let c = 0; c < cols; c++) {
    const colT = (t * 0.001 * state[c].speed + state[c].offset) % (rows * 1.8)
    for (let r = 0; r < rows; r++) {
      const distFromHead = colT - r
      if (distFromHead >= 0 && distFromHead < 1) {
        grid[r][c] = chars[chars.length - 1]
      } else if (distFromHead >= 1 && distFromHead < rows * 0.45) {
        const fade = 1 - distFromHead / (rows * 0.45)
        grid[r][c] = chars[Math.floor(fade * (chars.length - 1))]
      } else {
        grid[r][c] = ' '
      }
    }
  }
}

function drawGrid(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const cellW = 6, cellH = 3
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      const isH = r % cellH === 0
      const isV = c % cellW === 0
      if (isH && isV) {
        const wave = (Math.sin(c / cols * Math.PI * 4 + r / rows * Math.PI * 2 - t * 0.002) + 1) / 2
        grid[r][c] = chars[Math.floor(wave * (chars.length - 1))]
      } else if (isH || isV) {
        const wave = Math.sin(c / cols * Math.PI * 8 + r / rows * Math.PI * 4 - t * 0.001) * 0.3 + 0.3
        grid[r][c] = chars[Math.floor(wave * (chars.length - 1))]
      } else {
        grid[r][c] = ' '
      }
    }
  }
}

function drawTorus(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const A = t * 0.0012, B = t * 0.0007
  const cosA = Math.cos(A), sinA = Math.sin(A)
  const cosB = Math.cos(B), sinB = Math.sin(B)
  const R1 = 1.0, R2 = 2.2, K2 = 5.0
  const cx = cols / 2, cy = rows / 2
  const aspect = 0.5
  const screenScale = Math.min(cols * aspect, rows) * 0.85
  const K1 = screenScale * K2 / (K2 + R1 + R2)
  const zbuf: number[][] = Array.from({ length: rows }, () => Array(cols).fill(-Infinity))
  for (let theta = 0; theta < 2 * Math.PI; theta += 0.05) {
    const cosT = Math.cos(theta), sinT = Math.sin(theta)
    for (let phi = 0; phi < 2 * Math.PI; phi += 0.02) {
      const cosP = Math.cos(phi), sinP = Math.sin(phi)
      const ox = (R2 + R1 * cosT) * cosP
      const oy = (R2 + R1 * cosT) * sinP
      const oz = R1 * sinT
      const oy1 =  oy * cosA - oz * sinA
      const oz1 =  oy * sinA + oz * cosA
      const ox2 =  ox * cosB - oy1 * sinB
      const oy2 =  ox * sinB + oy1 * cosB
      const oz2 =  ox * Math.sin(B) + oz1 * Math.cos(B)
      const zDist = K2 - oz2
      if (zDist <= 0) continue
      const ooz = 1.0 / zDist
      const xp = Math.round(cx + K1 * ox2 * ooz)
      const yp = Math.round(cy - K1 * oy2 * ooz * aspect)
      if (xp < 0 || xp >= cols || yp < 0 || yp >= rows) continue
      const nx = cosT * cosP, ny = cosT * sinP, nz = sinT
      const ny1 =  ny * cosA - nz * sinA
      const nz1 =  ny * sinA + nz * cosA
      const nx2 =  nx * cosB - ny1 * sinB
      const ny2 =  nx * sinB + ny1 * cosB
      const nz2 =  nz1
      const L = nx2 * 0.57 + ny2 * 0.57 + nz2 * (-0.57)
      if (L > 0 && ooz > zbuf[yp][xp]) {
        zbuf[yp][xp] = ooz
        grid[yp][xp] = chars[Math.min(Math.floor(L * (chars.length - 1)), chars.length - 1)]
      }
    }
  }
}

function drawSphere(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const cx = cols / 2, cy = rows / 2
  const aspect = 2.0
  const R = Math.min(cols / (aspect * 2), rows / 2) * 0.88
  const A = t * 0.0007, B = t * 0.0004
  const cosA = Math.cos(A), sinA = Math.sin(A)
  const cosB = Math.cos(B), sinB = Math.sin(B)
  // Light from upper-right-front
  const lx = 0.577, ly = -0.577, lz = -0.577

  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      const sx = (c - cx) / (R * aspect)
      const sy = (r - cy) / R
      const d2 = sx * sx + sy * sy
      if (d2 >= 1.0) continue
      const sz = Math.sqrt(1.0 - d2)  // front-facing z (positive toward viewer)
      // Rotate surface point for texture coordinates
      const ny1 = sy * cosA - sz * sinA
      const nz1 = sy * sinA + sz * cosA
      const nx2 = sx * cosB + nz1 * sinB
      const nz2 = -sx * sinB + nz1 * cosB
      const ny2 = ny1
      // Lat/lon grid lines on rotating surface
      const lat = Math.asin(Math.max(-1, Math.min(1, ny2)))
      const lon = Math.atan2(nx2, nz2)
      const onGrid = Math.abs(Math.cos(lat * 5)) > 0.93 || Math.abs(Math.cos(lon * 8)) > 0.91
      // Lighting uses unrotated normal (sx, sy, sz)
      const L = Math.max(0, sx * lx + sy * ly + sz * lz)
      if (onGrid) {
        const intensity = 0.3 + L * 0.7
        grid[r][c] = chars[Math.min(Math.floor(intensity * (chars.length - 1)), chars.length - 1)]
      } else if (L > 0.02) {
        grid[r][c] = chars[Math.min(Math.floor(L * (chars.length - 1)), chars.length - 1)]
      }
      // dark side stays ' '
    }
  }
}

function drawCube(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const A = t * 0.0009, B = t * 0.0006
  const cosA = Math.cos(A), sinA = Math.sin(A)
  const cosB = Math.cos(B), sinB = Math.sin(B)
  const cx = cols / 2, cy = rows / 2
  const K2 = 5.0, S = 1.4, aspect = 0.5
  const scale = Math.min(cols * aspect, rows) * 0.72
  const K1 = scale * K2 / (K2 + S)
  const zbuf: number[][] = Array.from({ length: rows }, () => Array(cols).fill(-Infinity))
  const lx = 0.577, ly = -0.577, lz = -0.577

  function rot(px: number, py: number, pz: number): [number, number, number] {
    const y1 = py * cosA - pz * sinA
    const z1 = py * sinA + pz * cosA
    return [px * cosB + z1 * sinB, y1, -px * sinB + z1 * cosB]
  }

  const step = 0.04
  const faces: Array<[[number, number, number], (u: number, v: number) => [number, number, number]]> = [
    [[0,  0,  1], (u, v) => [ u*S,  v*S,  S]],
    [[0,  0, -1], (u, v) => [-u*S,  v*S, -S]],
    [[1,  0,  0], (u, v) => [ S,    v*S, -u*S]],
    [[-1, 0,  0], (u, v) => [-S,    v*S,  u*S]],
    [[0, -1,  0], (u, v) => [ u*S, -S,    v*S]],
    [[0,  1,  0], (u, v) => [ u*S,  S,   -v*S]],
  ]

  for (const [norm, ptFn] of faces) {
    const [rnx, rny, rnz] = rot(norm[0], norm[1], norm[2])
    const L = Math.max(0, rnx * lx + rny * ly + rnz * lz)
    if (L < 0.02) continue
    const ch = chars[Math.min(Math.floor(L * (chars.length - 1)), chars.length - 1)]
    for (let u = -1; u <= 1 + step * 0.5; u += step) {
      for (let v = -1; v <= 1 + step * 0.5; v += step) {
        const [rx, ry, rz] = rot(...ptFn(u, v))
        const zDist = K2 - rz
        if (zDist <= 0) continue
        const ooz = 1 / zDist
        const xp = Math.round(cx + K1 * rx * ooz)
        const yp = Math.round(cy - K1 * ry * ooz * aspect)
        if (xp >= 0 && xp < cols && yp >= 0 && yp < rows && ooz > zbuf[yp][xp]) {
          zbuf[yp][xp] = ooz
          grid[yp][xp] = ch
        }
      }
    }
  }
}

function drawDonut(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  // Faithful a1k0n donut.c algorithm — ring lies in XZ plane, tube depth = sinTheta,
  // so the hole is ALWAYS visible from any viewing angle.
  // A tilts around X-axis, B spins around Z-axis. Luminance from a1k0n's exact formula.
  const A = t * 0.00083, B = t * 0.00125
  const cosA = Math.cos(A), sinA = Math.sin(A)
  const cosB = Math.cos(B), sinB = Math.sin(B)
  const R1 = 1.0, R2 = 2.0, K2 = 5.0
  const cx = cols / 2, cy = rows / 2
  const aspect = 0.5
  const K1 = Math.min(cols * aspect, rows) * K2 / (K2 + R1 + R2) * 0.9
  const zbuf: number[][] = Array.from({ length: rows }, () => Array(cols).fill(-Infinity))

  // theta: angle around tube cross-section
  // phi:   angle around the ring center
  for (let theta = 0; theta < 2 * Math.PI; theta += 0.07) {
    const cosT = Math.cos(theta), sinT = Math.sin(theta)
    const h = R2 + R1 * cosT  // radial distance from axis to surface point

    for (let phi = 0; phi < 2 * Math.PI; phi += 0.02) {
      const cosP = Math.cos(phi), sinP = Math.sin(phi)

      // y-component after rotating by A around X-axis (before B spin)
      const ycomp = sinP * h * cosA - sinT * sinA

      // z depth after A rotation (determines perspective + occlusion)
      const zDist = sinP * h * sinA + sinT * cosA + K2
      if (zDist <= 0) continue
      const ooz = 1 / zDist

      // Project to screen (B rotates around Z in screen space)
      const xp = Math.round(cx + K1 * ooz * (cosP * h * cosB - ycomp * sinB))
      const yp = Math.round(cy - K1 * ooz * (cosP * h * sinB + ycomp * cosB) * aspect)
      if (xp < 0 || xp >= cols || yp < 0 || yp >= rows) continue

      // a1k0n's luminance: dot(rotated_normal, light_direction)
      // Produces smooth gradient across the tube with strong highlight on top
      const L = (sinT * sinA - sinP * cosT * cosA) * cosB
              - sinP * cosT * sinA
              - sinT * cosA
              - cosP * cosT * sinB

      if (L > 0 && ooz > zbuf[yp][xp]) {
        zbuf[yp][xp] = ooz
        grid[yp][xp] = chars[Math.min(Math.floor(L * (chars.length - 1)), chars.length - 1)]
      }
    }
  }
}

function drawHelix(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const cx = cols / 2, cy = rows / 2
  const aspect = 0.5
  const K2 = 6.0
  const scale = Math.min(cols * aspect, rows) * 0.5
  const K1 = scale
  const B = t * 0.0006
  const cosB = Math.cos(B), sinB = Math.sin(B)
  const zbuf: number[][] = Array.from({ length: rows }, () => Array(cols).fill(-Infinity))
  const lx = 0.707, lz = -0.707
  const helixR = 1.2, helixH = 3.8, turns = 3

  function plot(px: number, py: number, pz: number, intensity: number) {
    const rx = px * cosB + pz * sinB
    const rz = -px * sinB + pz * cosB
    const zDist = K2 - rz
    if (zDist <= 0) return
    const ooz = 1 / zDist
    const xp = Math.round(cx + K1 * rx * ooz)
    const yp = Math.round(cy - K1 * py * ooz * aspect)
    if (xp >= 0 && xp < cols && yp >= 0 && yp < rows && ooz > zbuf[yp][xp]) {
      zbuf[yp][xp] = ooz
      grid[yp][xp] = chars[Math.min(Math.floor(intensity * (chars.length - 1)), chars.length - 1)]
    }
  }

  const pStep = 0.025
  for (let phi = 0; phi < turns * 2 * Math.PI; phi += pStep) {
    const y = (phi / (turns * 2 * Math.PI) - 0.5) * helixH * 2
    // Strand 1
    const x1 = helixR * Math.cos(phi), z1 = helixR * Math.sin(phi)
    plot(x1, y, z1, Math.max(0.15, Math.cos(phi) * lx + Math.sin(phi) * lz * 0.5 + 0.55))
    // Strand 2 (π offset)
    const x2 = helixR * Math.cos(phi + Math.PI), z2 = helixR * Math.sin(phi + Math.PI)
    plot(x2, y, z2, Math.max(0.15, Math.cos(phi + Math.PI) * lx + Math.sin(phi + Math.PI) * lz * 0.5 + 0.55))
    // Rungs every half-turn
    if (phi % (Math.PI * 0.5) < pStep * 1.5) {
      for (let s = 0; s <= 14; s++) {
        const f = s / 14
        plot(x1 + (x2 - x1) * f, y, z1 + (z2 - z1) * f, 0.45)
      }
    }
  }
}

function drawTrefoilKnot(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const A = t * 0.0008, B = t * 0.0005
  const cosA = Math.cos(A), sinA = Math.sin(A)
  const cosB = Math.cos(B), sinB = Math.sin(B)
  const cx = cols / 2, cy = rows / 2
  const aspect = 0.5
  const K2 = 5.0
  const screenScale = Math.min(cols * aspect, rows) * 0.85
  const K1 = screenScale * K2 / (K2 + 3.5)
  const zbuf: number[][] = Array.from({ length: rows }, () => Array(cols).fill(-Infinity))
  const tubeR = 0.28
  const steps = 400
  const tubeSteps = 20
  const lx = 0.577, ly = 0.577, lz = -0.577
  const EPS = 0.02

  function knotPt(th: number): [number, number, number] {
    return [
      (2 + Math.cos(3 * th)) * Math.cos(2 * th),
      (2 + Math.cos(3 * th)) * Math.sin(2 * th),
      Math.sin(3 * th) * 2,
    ]
  }

  function rotYX(px: number, py: number, pz: number): [number, number, number] {
    const rx1 = px * cosA + pz * sinA
    const ry1 = py
    const rz1 = -px * sinA + pz * cosA
    return [rx1, ry1 * cosB - rz1 * sinB, ry1 * sinB + rz1 * cosB]
  }

  for (let i = 0; i < steps; i++) {
    const th = (i / steps) * 2 * Math.PI
    const [px, py, pz] = knotPt(th)
    const [px2, py2, pz2] = knotPt(th + EPS)

    let tx = px2 - px, ty = py2 - py, tz = pz2 - pz
    const tlen = Math.sqrt(tx * tx + ty * ty + tz * tz)
    tx /= tlen; ty /= tlen; tz /= tlen

    let upx = 0, upy = 0, upz = 1
    let dot = tx * upx + ty * upy + tz * upz
    let nx = upx - dot * tx, ny = upy - dot * ty, nz = upz - dot * tz
    let nlen = Math.sqrt(nx * nx + ny * ny + nz * nz)
    if (nlen < 0.001) {
      upx = 0; upy = 1; upz = 0
      dot = tx * upx + ty * upy + tz * upz
      nx = upx - dot * tx; ny = upy - dot * ty; nz = upz - dot * tz
      nlen = Math.sqrt(nx * nx + ny * ny + nz * nz)
    }
    nx /= nlen; ny /= nlen; nz /= nlen
    const bx = ty * nz - tz * ny, by = tz * nx - tx * nz, bz = tx * ny - ty * nx

    for (let j = 0; j < tubeSteps; j++) {
      const u = (j / tubeSteps) * 2 * Math.PI
      const cu = Math.cos(u), su = Math.sin(u)
      const qx = px + tubeR * (cu * nx + su * bx)
      const qy = py + tubeR * (cu * ny + su * by)
      const qz = pz + tubeR * (cu * nz + su * bz)

      const [rx, ry, rz] = rotYX(qx, qy, qz)
      const zDist = K2 - rz
      if (zDist <= 0) continue
      const ooz = 1 / zDist
      const xp = Math.round(cx + K1 * rx * ooz)
      const yp = Math.round(cy - K1 * ry * ooz * aspect)
      if (xp < 0 || xp >= cols || yp < 0 || yp >= rows) continue

      const snx = cu * nx + su * bx, sny = cu * ny + su * by, snz = cu * nz + su * bz
      const [rnx, rny, rnz] = rotYX(snx, sny, snz)
      const L = Math.max(0, rnx * lx + rny * ly + rnz * lz)

      if (ooz > zbuf[yp][xp]) {
        zbuf[yp][xp] = ooz
        const intensity = 0.15 + L * 0.85
        grid[yp][xp] = chars[Math.min(Math.floor(intensity * (chars.length - 1)), chars.length - 1)]
      }
    }
  }
}

function drawGeodesicDome(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const A = t * 0.0004, B = t * 0.00025
  const cosA = Math.cos(A), sinA = Math.sin(A)
  const cosB = Math.cos(B), sinB = Math.sin(B)
  const cx = cols / 2, cy = rows / 2
  const aspect = 0.5
  const K2 = 5.0
  const screenScale = Math.min(cols * aspect, rows) * 0.85
  const K1 = screenScale * K2 / (K2 + 1.05)
  const zbuf: number[][] = Array.from({ length: rows }, () => Array(cols).fill(-Infinity))
  const lx = 0.577, ly = 0.577, lz = -0.577

  function norm3(v: [number, number, number]): [number, number, number] {
    const len = Math.sqrt(v[0] * v[0] + v[1] * v[1] + v[2] * v[2])
    return [v[0] / len, v[1] / len, v[2] / len]
  }

  function rotYX(px: number, py: number, pz: number): [number, number, number] {
    const rx1 = px * cosA + pz * sinA
    const ry1 = py
    const rz1 = -px * sinA + pz * cosA
    return [rx1, ry1 * cosB - rz1 * sinB, ry1 * sinB + rz1 * cosB]
  }

  const phi = (1 + Math.sqrt(5)) / 2
  const rawV: [number, number, number][] = [
    [0, 1, phi], [0, -1, phi], [0, 1, -phi], [0, -1, -phi],
    [1, phi, 0], [-1, phi, 0], [1, -phi, 0], [-1, -phi, 0],
    [phi, 0, 1], [-phi, 0, 1], [phi, 0, -1], [-phi, 0, -1],
  ]
  const faces: [number, number, number][] = [
    [0,1,8],[0,8,4],[0,4,5],[0,5,9],[0,9,1],
    [1,6,8],[8,10,4],[4,2,5],[5,11,9],[9,7,1],
    [6,10,8],[10,2,4],[2,11,5],[11,7,9],[7,6,1],
    [3,6,7],[3,10,6],[3,2,10],[3,11,2],[3,7,11],
  ]

  const freq = 3

  function plotEdge(v1: [number, number, number], v2: [number, number, number]) {
    if (v1[1] < -0.3 && v2[1] < -0.3) return
    for (let s = 0; s <= 28; s++) {
      const f = s / 28
      const px = v1[0] + f * (v2[0] - v1[0])
      const py = v1[1] + f * (v2[1] - v1[1])
      const pz = v1[2] + f * (v2[2] - v1[2])
      const [rx, ry, rz] = rotYX(px, py, pz)
      const zDist = K2 - rz
      if (zDist <= 0) continue
      const ooz = 1 / zDist
      const xp = Math.round(cx + K1 * rx * ooz)
      const yp = Math.round(cy - K1 * ry * ooz * aspect)
      if (xp < 0 || xp >= cols || yp < 0 || yp >= rows) continue
      const [rnx, rny, rnz] = rotYX(px, py, pz)
      const L = Math.max(0.25, rnx * lx + rny * ly + rnz * lz)
      if (ooz > zbuf[yp][xp]) {
        zbuf[yp][xp] = ooz
        grid[yp][xp] = chars[Math.min(Math.floor(L * (chars.length - 1)), chars.length - 1)]
      }
    }
  }

  for (const [ai, bi, ci] of faces) {
    const va = norm3(rawV[ai]), vb = norm3(rawV[bi]), vc = norm3(rawV[ci])
    const pts: [number, number, number][][] = []
    for (let i = 0; i <= freq; i++) {
      pts[i] = []
      for (let j = 0; j <= freq - i; j++) {
        const k = freq - i - j
        pts[i][j] = norm3([(i * va[0] + j * vb[0] + k * vc[0]) / freq,
                            (i * va[1] + j * vb[1] + k * vc[1]) / freq,
                            (i * va[2] + j * vb[2] + k * vc[2]) / freq])
      }
    }
    for (let i = 0; i <= freq; i++) {
      for (let j = 0; j <= freq - i; j++) {
        if (j + 1 <= freq - i) plotEdge(pts[i][j], pts[i][j + 1])
        if (i + 1 <= freq && j <= freq - (i + 1)) plotEdge(pts[i][j], pts[i + 1][j])
        if (i + 1 <= freq && j >= 1) plotEdge(pts[i][j], pts[i + 1][j - 1])
      }
    }
  }
}

function drawSaturn(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const A = t * 0.0007, B = t * 0.0004
  const cosA = Math.cos(A), sinA = Math.sin(A)
  const cosB = Math.cos(B), sinB = Math.sin(B)
  const cx = cols / 2, cy = rows / 2
  const aspect = 0.5
  const K2 = 5.0
  const screenScale = Math.min(cols * aspect, rows) * 0.85
  const K1 = screenScale * K2 / (K2 + 2.5)
  const zbuf: number[][] = Array.from({ length: rows }, () => Array(cols).fill(-Infinity))
  const lx = 0.577, ly = 0.577, lz = -0.577
  const TILT = 0.47
  const sinTilt = Math.sin(TILT), cosTilt = Math.cos(TILT)

  function rotYX(px: number, py: number, pz: number): [number, number, number] {
    const rx1 = px * cosA + pz * sinA
    const ry1 = py
    const rz1 = -px * sinA + pz * cosA
    return [rx1, ry1 * cosB - rz1 * sinB, ry1 * sinB + rz1 * cosB]
  }

  function plotPoint(px: number, py: number, pz: number, nx: number, ny: number, nz: number) {
    const [rx, ry, rz] = rotYX(px, py, pz)
    const zDist = K2 - rz
    if (zDist <= 0) return
    const ooz = 1 / zDist
    const xp = Math.round(cx + K1 * rx * ooz)
    const yp = Math.round(cy - K1 * ry * ooz * aspect)
    if (xp < 0 || xp >= cols || yp < 0 || yp >= rows) return
    const [rnx, rny, rnz] = rotYX(nx, ny, nz)
    const L = Math.max(0, rnx * lx + rny * ly + rnz * lz)
    if (L > 0 && ooz > zbuf[yp][xp]) {
      zbuf[yp][xp] = ooz
      grid[yp][xp] = chars[Math.min(Math.floor(L * (chars.length - 1)), chars.length - 1)]
    }
  }

  // Planet sphere — tighter sampling for denser coverage
  for (let theta = 0; theta < 2 * Math.PI; theta += 0.04) {
    const cosT = Math.cos(theta), sinT = Math.sin(theta)
    for (let phi = 0; phi < 2 * Math.PI; phi += 0.02) {
      const cosP = Math.cos(phi), sinP = Math.sin(phi)
      const px = cosT * cosP, py = cosT * sinP, pz = sinT
      plotPoint(px, py, pz, px, py, pz)
    }
  }

  // Rings — solid swept disk sampled at many radii (not just sparse bands)
  const R_inner = 1.35, R_outer = 2.45
  const ringSamples = 40
  for (let ri = 0; ri <= ringSamples; ri++) {
    const rFrac = ri / ringSamples
    // Cassini division gap
    if (rFrac > 0.42 && rFrac < 0.58) continue
    const r = R_inner + (R_outer - R_inner) * rFrac
    // Brightness: bright B ring (inner), dark C ring hint, Cassini gap, bright A ring (outer)
    const ringBrightness = rFrac < 0.42
      ? 0.35 + rFrac * 1.1   // B ring — bright, brighter toward gap
      : 0.25 + (1 - rFrac) * 0.9  // A ring — bright at inner edge, fades outward

    for (let theta = 0; theta < 2 * Math.PI; theta += 0.01) {
      const cosT = Math.cos(theta), sinT = Math.sin(theta)
      const px = r * cosT
      const py = -r * sinT * sinTilt
      const pz =  r * sinT * cosTilt
      const [rx, ry, rz] = rotYX(px, py, pz)
      const zDist = K2 - rz
      if (zDist <= 0) continue
      const ooz = 1 / zDist
      const xp = Math.round(cx + K1 * rx * ooz)
      const yp = Math.round(cy - K1 * ry * ooz * aspect)
      if (xp < 0 || xp >= cols || yp < 0 || yp >= rows) continue
      if (ooz > zbuf[yp][xp]) {
        zbuf[yp][xp] = ooz
        grid[yp][xp] = chars[Math.min(Math.floor(ringBrightness * (chars.length - 1)), chars.length - 1)]
      }
    }
  }
}

function drawHyperboloid(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const A = t * 0.0006, B = t * 0.0003
  const cosA = Math.cos(A), sinA = Math.sin(A)
  const cosB = Math.cos(B), sinB = Math.sin(B)
  const cx = cols / 2, cy = rows / 2
  const aspect = 0.5
  const H = 1.7
  const K2 = 6.0
  const maxExtent = Math.sqrt(1 + H * H) + 0.1
  const screenScale = Math.min(cols * aspect, rows) * 0.85
  const K1 = screenScale * K2 / (K2 + maxExtent)
  const zbuf: number[][] = Array.from({ length: rows }, () => Array(cols).fill(-Infinity))
  const lx = 0.577, ly = 0.577, lz = -0.577
  const nRulings = 32
  const nPoints = 60

  function rotYX(px: number, py: number, pz: number): [number, number, number] {
    const rx1 = px * cosA + pz * sinA
    const ry1 = py
    const rz1 = -px * sinA + pz * cosA
    return [rx1, ry1 * cosB - rz1 * sinB, ry1 * sinB + rz1 * cosB]
  }

  function plotRulingPoint(px: number, py: number, pz: number) {
    const [rx, ry, rz] = rotYX(px, py, pz)
    const zDist = K2 - rz
    if (zDist <= 0) return
    const ooz = 1 / zDist
    const xp = Math.round(cx + K1 * rx * ooz)
    const yp = Math.round(cy - K1 * ry * ooz * aspect)
    if (xp < 0 || xp >= cols || yp < 0 || yp >= rows) return
    // Outward normal of hyperboloid x²+z²-y²=1 at (x,y,z) is (x,-y,z)/|(x,-y,z)|
    const nm = Math.sqrt(px * px + py * py + pz * pz)
    const [rnx, rny, rnz] = rotYX(px / nm, -py / nm, pz / nm)
    const L = Math.max(0.12, rnx * lx + rny * ly + rnz * lz)
    if (ooz > zbuf[yp][xp]) {
      zbuf[yp][xp] = ooz
      grid[yp][xp] = chars[Math.min(Math.floor(L * (chars.length - 1)), chars.length - 1)]
    }
  }

  // Two families of straight-line rulings on x²+z²-y²=1
  // Family A: P(s,v) = (cos(s)+v·sin(s), v, sin(s)-v·cos(s))
  // Family B: P(s,v) = (cos(s)-v·sin(s), v, sin(s)+v·cos(s))
  for (let k = 0; k < nRulings; k++) {
    const s = (k / nRulings) * 2 * Math.PI
    const cosS = Math.cos(s), sinS = Math.sin(s)
    for (let family = 0; family < 2; family++) {
      const dir = family === 0 ? 1 : -1
      for (let j = 0; j <= nPoints; j++) {
        const v = -H + (2 * H * j) / nPoints
        plotRulingPoint(cosS + v * dir * sinS, v, sinS - v * dir * cosS)
      }
    }
  }

  // Horizontal rings at multiple levels for structural definition
  for (let j = 0; j <= 80; j++) {
    const theta = (j / 80) * 2 * Math.PI
    for (const vy of [-H, -H * 0.75, -H * 0.5, -H * 0.25, 0, H * 0.25, H * 0.5, H * 0.75, H]) {
      const r = Math.sqrt(1 + vy * vy)
      plotRulingPoint(r * Math.cos(theta), vy, r * Math.sin(theta))
    }
  }
}

function drawDNA(grid: string[][], cols: number, rows: number, t: number, chars: string[]): void {
  const A = t * 0.0005
  const cosA = Math.cos(A), sinA = Math.sin(A)
  const cx = cols / 2, cy = rows / 2
  const aspect = 0.5
  const K2 = 6.0
  const scale = Math.min(cols * aspect, rows) * 0.52
  const K1 = scale
  const zbuf: number[][] = Array.from({ length: rows }, () => Array(cols).fill(-Infinity))
  const lx = 0.707, lz = -0.707
  const helixR = 1.0
  const helixH = 4.2
  const turns = 4.0
  const tubeR = 0.22
  const bpPerTurn = 10
  const totalSteps = Math.round(turns * bpPerTurn * 6)
  // B-DNA: strands are ~150° apart (minor groove ~120°, major groove ~240°)
  const strandOffset = (150 / 180) * Math.PI

  function rotY(px: number, py: number, pz: number): [number, number, number] {
    return [px * cosA + pz * sinA, py, -px * sinA + pz * cosA]
  }

  function plotTubeSection(px: number, py: number, pz: number,
    tx: number, ty: number, tz: number, r: number, baseBrightness: number) {
    let upx = 0, upy = 0, upz = 1
    let dot = tx * upx + ty * upy + tz * upz
    let nx = upx - dot * tx, ny = upy - dot * ty, nz = upz - dot * tz
    let nlen = Math.sqrt(nx * nx + ny * ny + nz * nz)
    if (nlen < 0.001) {
      upx = 0; upy = 1; upz = 0
      dot = tx * upx + ty * upy + tz * upz
      nx = upx - dot * tx; ny = upy - dot * ty; nz = upz - dot * tz
      nlen = Math.sqrt(nx * nx + ny * ny + nz * nz)
    }
    nx /= nlen; ny /= nlen; nz /= nlen
    const bx = ty * nz - tz * ny, by = tz * nx - tx * nz, bz = tx * ny - ty * nx

    for (let j = 0; j < 16; j++) {
      const u = (j / 16) * 2 * Math.PI
      const cu = Math.cos(u), su = Math.sin(u)
      const qx = px + r * (cu * nx + su * bx)
      const qy = py + r * (cu * ny + su * by)
      const qz = pz + r * (cu * nz + su * bz)
      const [rx, ry, rz] = rotY(qx, qy, qz)
      const zDist = K2 - rz
      if (zDist <= 0) continue
      const ooz = 1 / zDist
      const xp = Math.round(cx + K1 * rx * ooz)
      const yp = Math.round(cy - K1 * ry * ooz * aspect)
      if (xp < 0 || xp >= cols || yp < 0 || yp >= rows) continue
      const snx = cu * nx + su * bx, snz = cu * nz + su * bz
      const [rnx,, rnz] = rotY(snx, 0, snz)
      const L = Math.max(0, rnx * lx + rnz * lz)
      if (ooz > zbuf[yp][xp]) {
        zbuf[yp][xp] = ooz
        const intensity = Math.max(0.12, Math.min(1, baseBrightness * (0.3 + L * 0.7)))
        grid[yp][xp] = chars[Math.min(Math.floor(intensity * (chars.length - 1)), chars.length - 1)]
      }
    }
  }

  const stepsPerBP = 6
  const totalAngle = turns * 2 * Math.PI

  for (let i = 0; i < totalSteps; i++) {
    const f = i / totalSteps
    const y = -helixH + f * helixH * 2
    const angle = f * totalAngle
    const da = totalAngle / totalSteps

    // Tangent direction (same for both strands)
    const txRaw = -helixR * Math.sin(angle) * da
    const tyRaw = (helixH * 2) / totalSteps
    const tzRaw = helixR * Math.cos(angle) * da
    const tlen = Math.sqrt(txRaw * txRaw + tyRaw * tyRaw + tzRaw * tzRaw)
    const tx = txRaw / tlen, ty = tyRaw / tlen, tz = tzRaw / tlen

    // Strand 1
    const x1 = helixR * Math.cos(angle), z1 = helixR * Math.sin(angle)
    plotTubeSection(x1, y, z1, tx, ty, tz, tubeR, 1.0)

    // Strand 2 (B-DNA: 150° offset)
    const x2 = helixR * Math.cos(angle + strandOffset), z2 = helixR * Math.sin(angle + strandOffset)
    plotTubeSection(x2, y, z2, tx, ty, tz, tubeR, 0.72)

    // Base-pair rungs every stepsPerBP
    if (i % stepsPerBP === 0) {
      for (let s = 0; s <= 10; s++) {
        const sf = s / 10
        const rx = x1 + sf * (x2 - x1)
        const rz = z1 + sf * (z2 - z1)
        const [vrx, vry, vrz] = rotY(rx, y, rz)
        const zDist = K2 - vrz
        if (zDist <= 0) continue
        const ooz = 1 / zDist
        const xp = Math.round(cx + K1 * vrx * ooz)
        const yp = Math.round(cy - K1 * vry * ooz * aspect)
        if (xp < 0 || xp >= cols || yp < 0 || yp >= rows) continue
        if (ooz > zbuf[yp][xp]) {
          zbuf[yp][xp] = ooz
          grid[yp][xp] = chars[Math.min(Math.floor(0.42 * (chars.length - 1)), chars.length - 1)]
        }
      }
    }
  }
}

// ============================================================================
// Component factory
// ============================================================================

type DrawFn = (grid: string[][], cols: number, rows: number, t: number, chars: string[], extra?: ColState[]) => void

function makeAsciiComponent(drawFn: DrawFn, defaultCharset: AsciiCharset = 'classic') {
  return React.forwardRef<HTMLPreElement, AsciiShapeProps>(
    (
      {
        size = 'md',
        charset = defaultCharset,
        color,
        speed = 'normal',
        animated = true,
        multicolor = false,
        className,
        ...props
      },
      ref
    ) => {
      const { cols, rows } = SIZE_MAP[size]
      const chars = CHARSETS[charset]
      const speedMul = SPEED_MAP[speed]
      const matrixStateRef = React.useRef<ColState[]>(makeMatrixState(cols))
      const preRef = React.useRef<HTMLPreElement | null>(null)
      const spanRefs = React.useRef<(HTMLSpanElement | null)[]>([])

      // Compute first frame for initial paint only — never triggers re-renders
      const initialLines = React.useMemo(() => {
        matrixStateRef.current = makeMatrixState(cols)
        const g = makeGrid(cols, rows)
        drawFn(g, cols, rows, 0, chars, matrixStateRef.current)
        return gridToLines(g)
      // eslint-disable-next-line react-hooks/exhaustive-deps
      }, [cols, rows, chars])

      React.useEffect(() => {
        const pre = preRef.current
        if (!pre) return

        function writeLines(lines: string[]) {
          if (multicolor) {
            spanRefs.current.forEach((span, i) => {
              if (span) span.textContent = lines[i] ?? ''
            })
          } else {
            // eslint-disable-next-line @typescript-eslint/no-non-null-assertion
            pre!.textContent = lines.join('\n')
          }
        }

        if (!animated) {
          writeLines(initialLines)
          return
        }

        const startTime = performance.now()
        let rafId: number

        function frame(now: number) {
          const t = (now - startTime) * speedMul
          const g = makeGrid(cols, rows)
          drawFn(g, cols, rows, t, chars, matrixStateRef.current)
          writeLines(gridToLines(g))
          rafId = requestAnimationFrame(frame)
        }

        rafId = requestAnimationFrame(frame)
        return () => cancelAnimationFrame(rafId)
      // eslint-disable-next-line react-hooks/exhaustive-deps
      }, [size, charset, speed, animated, multicolor])

      function combineRef(el: HTMLPreElement | null) {
        preRef.current = el
        if (typeof ref === 'function') ref(el)
        else if (ref) (ref as React.MutableRefObject<HTMLPreElement | null>).current = el
      }

      return (
        <pre
          aria-hidden="true"
          ref={combineRef}
          className={cn(
            'inline-block border-3 border-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))] bg-background overflow-hidden',
            'font-mono text-xs leading-none tracking-tight select-none p-1',
            className
          )}
          style={multicolor ? undefined : { color: color || 'currentColor' }}
          {...props}
        >
          {multicolor
            ? initialLines.map((line, i) => (
                <React.Fragment key={i}>
                  <span
                    ref={el => { spanRefs.current[i] = el }}
                    style={{ color: MULTICOLOR_PALETTE[i % MULTICOLOR_PALETTE.length] }}
                  >{line}</span>
                  {i < initialLines.length - 1 && '\n'}
                </React.Fragment>
              ))
            : initialLines.join('\n')}
        </pre>
      )
    }
  )
}

// ============================================================================
// Named exports — 17 ASCII shape components
// ============================================================================

export const AsciiSpiral = makeAsciiComponent(drawSpiral, 'classic')
AsciiSpiral.displayName = 'AsciiSpiral'

export const AsciiRose = makeAsciiComponent(drawRose, 'braille')
AsciiRose.displayName = 'AsciiRose'

export const AsciiWave = makeAsciiComponent(drawWave, 'classic')
AsciiWave.displayName = 'AsciiWave'

export const AsciiVortex = makeAsciiComponent(drawVortex, 'blocks')
AsciiVortex.displayName = 'AsciiVortex'

export const AsciiPulse = makeAsciiComponent(drawPulse, 'dots')
AsciiPulse.displayName = 'AsciiPulse'

export const AsciiMatrix = makeAsciiComponent(
  (g, cols, rows, t, chars, state) => drawMatrix(g, cols, rows, t, chars, state!), 'classic'
)
AsciiMatrix.displayName = 'AsciiMatrix'

export const AsciiGrid = makeAsciiComponent(drawGrid, 'line')
AsciiGrid.displayName = 'AsciiGrid'

export const AsciiTorus = makeAsciiComponent(drawTorus, 'blocks')
AsciiTorus.displayName = 'AsciiTorus'

export const AsciiSphere = makeAsciiComponent(drawSphere, 'classic')
AsciiSphere.displayName = 'AsciiSphere'

export const AsciiCube = makeAsciiComponent(drawCube, 'blocks')
AsciiCube.displayName = 'AsciiCube'

export const AsciiHelix = makeAsciiComponent(drawHelix, 'braille')
AsciiHelix.displayName = 'AsciiHelix'

export const AsciiDonut = makeAsciiComponent(drawDonut, 'classic')
AsciiDonut.displayName = 'AsciiDonut'

export const AsciiTrefoilKnot = makeAsciiComponent(drawTrefoilKnot, 'blocks')
AsciiTrefoilKnot.displayName = 'AsciiTrefoilKnot'

export const AsciiGeodesicDome = makeAsciiComponent(drawGeodesicDome, 'classic')
AsciiGeodesicDome.displayName = 'AsciiGeodesicDome'

export const AsciiSaturn = makeAsciiComponent(drawSaturn, 'blocks')
AsciiSaturn.displayName = 'AsciiSaturn'

export const AsciiHyperboloid = makeAsciiComponent(drawHyperboloid, 'classic')
AsciiHyperboloid.displayName = 'AsciiHyperboloid'

export const AsciiDNA = makeAsciiComponent(drawDNA, 'braille')
AsciiDNA.displayName = 'AsciiDNA'
```

---

### Aspect Ratio <a name="aspect-ratio"></a>

Displays content within a desired ratio

- **Registry Name**: `@boldkit/aspect-ratio`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/aspect-ratio.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-aspect-ratio`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/aspect-ratio-demo.tsx`

```tsx
import { AspectRatio } from '@/components/ui/aspect-ratio'

export default function Example() {
  return (
    <AspectRatio ratio={16 / 9}>
      <img
        src="https://images.unsplash.com/photo-1588345921523-c2dcdb7f1dcd?w=800"
        alt="Photo"
        className="h-full w-full object-cover"
      />
    </AspectRatio>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/aspect-ratio.tsx`

```tsx
import * as AspectRatioPrimitive from '@radix-ui/react-aspect-ratio'

const AspectRatio = AspectRatioPrimitive.Root

export { AspectRatio }
```

---

### Avatar <a name="avatar"></a>

An image element with a fallback for representing the user

- **Registry Name**: `@boldkit/avatar`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/avatar.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-avatar`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/avatar-demo.tsx`

```tsx
import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar'

export default function Example() {
  return (
    <Avatar>
      <AvatarImage src="https://github.com/shadcn.png" alt="@shadcn" />
      <AvatarFallback>CN</AvatarFallback>
    </Avatar>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/avatar.tsx`

```tsx
import * as React from 'react'
import * as AvatarPrimitive from '@radix-ui/react-avatar'
import { cn } from '@/lib/utils'

const Avatar = React.forwardRef<
  React.ElementRef<typeof AvatarPrimitive.Root>,
  React.ComponentPropsWithoutRef<typeof AvatarPrimitive.Root>
>(({ className, ...props }, ref) => (
  <AvatarPrimitive.Root
    ref={ref}
    className={cn(
      'relative flex h-10 w-10 shrink-0 overflow-hidden border-3 border-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))] transition-all duration-200 hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none',
      className
    )}
    {...props}
  />
))
Avatar.displayName = AvatarPrimitive.Root.displayName

const AvatarImage = React.forwardRef<
  React.ElementRef<typeof AvatarPrimitive.Image>,
  React.ComponentPropsWithoutRef<typeof AvatarPrimitive.Image>
>(({ className, ...props }, ref) => (
  <AvatarPrimitive.Image
    ref={ref}
    className={cn('aspect-square h-full w-full object-cover', className)}
    {...props}
  />
))
AvatarImage.displayName = AvatarPrimitive.Image.displayName

const AvatarFallback = React.forwardRef<
  React.ElementRef<typeof AvatarPrimitive.Fallback>,
  React.ComponentPropsWithoutRef<typeof AvatarPrimitive.Fallback>
>(({ className, ...props }, ref) => (
  <AvatarPrimitive.Fallback
    ref={ref}
    className={cn(
      'flex h-full w-full items-center justify-center bg-primary text-primary-foreground font-bold uppercase',
      className
    )}
    {...props}
  />
))
AvatarFallback.displayName = AvatarPrimitive.Fallback.displayName

export { Avatar, AvatarImage, AvatarFallback }
```

---

### Badge <a name="badge"></a>

Displays a badge or a component that looks like a badge with neubrutalism styling

- **Registry Name**: `@boldkit/badge`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/badge.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/badge-demo.tsx`

```tsx
import { Badge } from '@/components/ui/badge'

export default function Example() {
  return <Badge>Badge</Badge>
}
```

#### Component Source Code

##### File: `registry/default/ui/badge.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'

const badgeVariants = cva(
  'inline-flex items-center border-2 border-foreground px-2.5 py-0.5 text-xs font-bold uppercase tracking-wide transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-ring focus:ring-offset-2 shadow-[4px_4px_0px_hsl(var(--shadow-color))] hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none',
  {
    variants: {
      variant: {
        default: 'bg-primary text-primary-foreground',
        secondary: 'bg-secondary text-secondary-foreground',
        accent: 'bg-accent text-accent-foreground',
        destructive: 'bg-destructive text-destructive-foreground',
        success: 'bg-success text-success-foreground',
        warning: 'bg-warning text-warning-foreground',
        info: 'bg-info text-info-foreground',
        outline: 'bg-background text-foreground',
      },
    },
    defaultVariants: {
      variant: 'default',
    },
  }
)

export interface BadgeProps
  extends React.HTMLAttributes<HTMLSpanElement>,
    VariantProps<typeof badgeVariants> {}

function Badge({ className, variant, ...props }: BadgeProps) {
  return (
    <span
      className={cn(badgeVariants({ variant }), className)}
      {...props}
    />
  )
}

export { Badge, badgeVariants }
```

---

### Breadcrumb <a name="breadcrumb"></a>

Displays the path to the current resource using a hierarchy of links

- **Registry Name**: `@boldkit/breadcrumb`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/breadcrumb.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-slot`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/breadcrumb-demo.tsx`

```tsx
import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from '@/components/ui/breadcrumb'

export default function Example() {
  return (
    <Breadcrumb>
      <BreadcrumbList>
        <BreadcrumbItem>
          <BreadcrumbLink href="/">Home</BreadcrumbLink>
        </BreadcrumbItem>
        <BreadcrumbSeparator />
        <BreadcrumbItem>
          <BreadcrumbLink href="/docs">Docs</BreadcrumbLink>
        </BreadcrumbItem>
        <BreadcrumbSeparator />
        <BreadcrumbItem>
          <BreadcrumbPage>Breadcrumb</BreadcrumbPage>
        </BreadcrumbItem>
      </BreadcrumbList>
    </Breadcrumb>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/breadcrumb.tsx`

```tsx
import * as React from 'react'
import { Slot } from '@radix-ui/react-slot'
import { ChevronRight, MoreHorizontal } from 'lucide-react'
import { cn } from '@/lib/utils'

const Breadcrumb = React.forwardRef<
  HTMLElement,
  React.ComponentPropsWithoutRef<'nav'> & {
    separator?: React.ReactNode
  }
>(({ ...props }, ref) => <nav ref={ref} aria-label="breadcrumb" {...props} />)
Breadcrumb.displayName = 'Breadcrumb'

const BreadcrumbList = React.forwardRef<
  HTMLOListElement,
  React.ComponentPropsWithoutRef<'ol'>
>(({ className, ...props }, ref) => (
  <ol
    ref={ref}
    className={cn(
      'flex flex-wrap items-center gap-1.5 break-words text-sm text-muted-foreground sm:gap-2.5',
      className
    )}
    {...props}
  />
))
BreadcrumbList.displayName = 'BreadcrumbList'

const BreadcrumbItem = React.forwardRef<
  HTMLLIElement,
  React.ComponentPropsWithoutRef<'li'>
>(({ className, ...props }, ref) => (
  <li
    ref={ref}
    className={cn('inline-flex items-center gap-1.5', className)}
    {...props}
  />
))
BreadcrumbItem.displayName = 'BreadcrumbItem'

const BreadcrumbLink = React.forwardRef<
  HTMLAnchorElement,
  React.ComponentPropsWithoutRef<'a'> & {
    asChild?: boolean
  }
>(({ asChild, className, ...props }, ref) => {
  const Comp = asChild ? Slot : 'a'

  return (
    <Comp
      ref={ref}
      className={cn('transition-colors hover:text-foreground font-medium', className)}
      {...props}
    />
  )
})
BreadcrumbLink.displayName = 'BreadcrumbLink'

const BreadcrumbPage = React.forwardRef<
  HTMLSpanElement,
  React.ComponentPropsWithoutRef<'span'>
>(({ className, ...props }, ref) => (
  <span
    ref={ref}
    role="link"
    aria-disabled="true"
    aria-current="page"
    className={cn('font-bold text-foreground', className)}
    {...props}
  />
))
BreadcrumbPage.displayName = 'BreadcrumbPage'

const BreadcrumbSeparator = ({
  children,
  className,
  ...props
}: React.ComponentProps<'li'>) => (
  <li
    role="presentation"
    aria-hidden="true"
    className={cn('[&>svg]:h-3.5 [&>svg]:w-3.5', className)}
    {...props}
  >
    {children ?? <ChevronRight className="stroke-[3]" />}
  </li>
)
BreadcrumbSeparator.displayName = 'BreadcrumbSeparator'

const BreadcrumbEllipsis = ({
  className,
  ...props
}: React.ComponentProps<'span'>) => (
  <span
    role="presentation"
    aria-hidden="true"
    className={cn('flex h-9 w-9 items-center justify-center', className)}
    {...props}
  >
    <MoreHorizontal className="h-4 w-4 stroke-[3]" />
    <span className="sr-only">More</span>
  </span>
)
BreadcrumbEllipsis.displayName = 'BreadcrumbEllipsis'

export {
  Breadcrumb,
  BreadcrumbList,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbPage,
  BreadcrumbSeparator,
  BreadcrumbEllipsis,
}
```

---

### Button <a name="button"></a>

Displays a button with neubrutalism styling and press-down animation

- **Registry Name**: `@boldkit/button`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/button.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-slot`
  - `class-variance-authority`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/button-demo.tsx`

```tsx
import { Button } from '@/components/ui/button'

export default function Example() {
  return <Button>Click me</Button>
}
```

#### Component Source Code

##### File: `registry/default/ui/button.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { Slot } from '@radix-ui/react-slot'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'

const buttonVariants = cva(
  'inline-flex items-center justify-center gap-2 whitespace-nowrap text-sm font-bold uppercase tracking-wide transition-all duration-200 disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0 border-3 border-foreground focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2',
  {
    variants: {
      variant: {
        default:
          'bg-primary text-primary-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))] hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none active:translate-x-[4px] active:translate-y-[4px] active:shadow-none',
        secondary:
          'bg-secondary text-secondary-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))] hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none active:translate-x-[4px] active:translate-y-[4px] active:shadow-none',
        accent:
          'bg-accent text-accent-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))] hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none active:translate-x-[4px] active:translate-y-[4px] active:shadow-none',
        destructive:
          'bg-destructive text-destructive-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))] hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none active:translate-x-[4px] active:translate-y-[4px] active:shadow-none',
        outline:
          'bg-background text-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))] hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none hover:bg-muted active:translate-x-[4px] active:translate-y-[4px] active:shadow-none',
        ghost:
          'border-transparent shadow-none hover:bg-muted hover:border-foreground',
        link: 'border-transparent shadow-none underline-offset-4 hover:underline text-primary',
        noShadow:
          'bg-primary text-primary-foreground',
        reverse:
          'bg-primary text-primary-foreground hover:translate-x-[-4px] hover:translate-y-[-4px] hover:shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
      },
      size: {
        default: 'h-11 px-5 py-2',
        sm: 'h-9 px-4 text-xs',
        lg: 'h-12 px-8 text-base',
        xl: 'h-14 px-10 text-lg',
        icon: 'h-11 w-11',
      },
      animation: {
        none: '',
        pulse: 'animate-[brutal-pulse_2s_ease-in-out_infinite]',
        bounce: 'animate-[brutal-bounce_0.5s_ease-in-out_infinite]',
        shake: 'hover:animate-[brutal-shake_0.4s_ease-in-out]',
        wiggle: 'hover:animate-[brutal-wiggle_0.3s_ease-in-out]',
        pop: 'hover:animate-[brutal-pop_0.2s_ease-out] active:scale-95',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
      animation: 'none',
    },
  }
)

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, animation, asChild = false, ...props }, ref) => {
    const Comp = asChild ? Slot : 'button'
    return (
      <Comp
        className={cn(buttonVariants({ variant, size, animation, className }))}
        ref={ref}
        {...props}
      />
    )
  }
)
Button.displayName = 'Button'

export { Button, buttonVariants }
```

---

### Calendar <a name="calendar"></a>

A date picker calendar with neubrutalism styling

- **Registry Name**: `@boldkit/calendar`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/calendar.json
```

#### Dependencies

- **NPM Packages**:
  - `react-day-picker`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`

#### How to Use

##### File: `registry/default/example/calendar-demo.tsx`

```tsx
import { Calendar } from '@/components/ui/calendar'
import { useState } from 'react'

export default function Example() {
  const [date, setDate] = useState<Date | undefined>(new Date())

  return (
    <Calendar
      mode="single"
      selected={date}
      onSelect={setDate}
    />
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/calendar.tsx`

```tsx
import * as React from 'react'
import { ChevronLeft, ChevronRight } from 'lucide-react'
import { DayPicker } from 'react-day-picker'
import { cn } from '@/lib/utils'
import { buttonVariants } from '@/components/ui/button'

export type CalendarProps = React.ComponentProps<typeof DayPicker>

function Calendar({
  className,
  classNames,
  showOutsideDays = true,
  ...props
}: CalendarProps) {
  return (
    <DayPicker
      showOutsideDays={showOutsideDays}
      className={cn(
        'p-3 border-3 border-foreground bg-background shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        className
      )}
      classNames={{
        months: 'flex flex-col sm:flex-row gap-4',
        month: 'flex flex-col gap-4',
        month_caption: 'flex justify-center pt-1 relative items-center px-10',
        caption_label: 'text-sm font-bold uppercase tracking-wide',
        nav: 'flex items-center gap-1',
        button_previous: cn(
          buttonVariants({ variant: 'outline' }),
          'h-7 w-7 bg-transparent p-0 absolute left-0 hover:translate-x-0 hover:translate-y-0 hover:scale-110'
        ),
        button_next: cn(
          buttonVariants({ variant: 'outline' }),
          'h-7 w-7 bg-transparent p-0 absolute right-0 hover:translate-x-0 hover:translate-y-0 hover:scale-110'
        ),
        month_grid: 'w-full border-collapse space-y-1',
        weekdays: 'flex',
        weekday: 'text-muted-foreground w-9 font-bold text-[0.8rem] uppercase',
        week: 'flex w-full mt-2',
        day: 'relative p-0 text-center text-sm focus-within:relative focus-within:z-20',
        day_button: cn(
          buttonVariants({ variant: 'ghost' }),
          'h-9 w-9 p-0 font-medium border-0 aria-selected:opacity-100'
        ),
        selected: 'bg-primary text-primary-foreground hover:bg-primary hover:text-primary-foreground focus:bg-primary focus:text-primary-foreground border-2 border-foreground',
        today: 'bg-accent text-accent-foreground border-2 border-foreground',
        outside: 'text-muted-foreground opacity-50 aria-selected:opacity-30',
        disabled: 'text-muted-foreground opacity-50',
        range_middle: 'aria-selected:bg-accent aria-selected:text-accent-foreground',
        hidden: 'invisible',
        ...classNames,
      }}
      components={{
        Chevron: ({ orientation }) => {
          const Icon = orientation === 'left' ? ChevronLeft : ChevronRight
          return <Icon className="h-4 w-4 stroke-[3]" />
        },
      }}
      {...props}
    />
  )
}
Calendar.displayName = 'Calendar'

export { Calendar }
```

---

### Card <a name="card"></a>

Displays a card with header, content, and footer with thick borders and shadows

- **Registry Name**: `@boldkit/card`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/card.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/card-demo.tsx`

```tsx
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'

export default function Example() {
  return (
    <Card>
      <CardHeader>
        <CardTitle>Card Title</CardTitle>
        <CardDescription>Card Description</CardDescription>
      </CardHeader>
      <CardContent>
        <p>Card Content</p>
      </CardContent>
      <CardFooter>
        <p>Card Footer</p>
      </CardFooter>
    </Card>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/card.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'

const Card = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement> & { interactive?: boolean }
>(({ className, interactive, role = 'article', ...props }, ref) => (
  <div
    ref={ref}
    role={role}
    className={cn(
      'bg-card text-card-foreground border-3 border-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))] transition-all duration-200',
      interactive && 'cursor-pointer hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none active:translate-x-[4px] active:translate-y-[4px] active:shadow-none',
      className
    )}
    {...props}
  />
))
Card.displayName = 'Card'

const CardHeader = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn('flex flex-col space-y-1.5 p-6 border-b-3 border-foreground', className)}
    {...props}
  />
))
CardHeader.displayName = 'CardHeader'

const CardTitle = React.forwardRef<
  HTMLHeadingElement,
  React.HTMLAttributes<HTMLHeadingElement>
>(({ className, ...props }, ref) => (
  <h3
    ref={ref}
    className={cn('text-xl font-bold uppercase tracking-wide leading-none', className)}
    {...props}
  />
))
CardTitle.displayName = 'CardTitle'

const CardDescription = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn('text-sm text-muted-foreground', className)}
    {...props}
  />
))
CardDescription.displayName = 'CardDescription'

const CardContent = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div ref={ref} className={cn('p-6', className)} {...props} />
))
CardContent.displayName = 'CardContent'

const CardFooter = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn('flex items-center p-6 border-t-3 border-foreground bg-muted/50', className)}
    {...props}
  />
))
CardFooter.displayName = 'CardFooter'

export { Card, CardHeader, CardFooter, CardTitle, CardDescription, CardContent }
```

---

### Carousel <a name="carousel"></a>

Carousel component with navigation, dots, and touch support built on Embla

- **Registry Name**: `@boldkit/carousel`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/carousel.json
```

#### Dependencies

- **NPM Packages**:
  - `embla-carousel-react`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`

#### How to Use

##### File: `registry/default/example/carousel-demo.tsx`

```tsx
import { Carousel, CarouselContent, CarouselItem, CarouselPrevious, CarouselNext } from '@/components/ui/carousel'

export default function Example() {
  return (
    <Carousel>
      <CarouselContent>
        <CarouselItem>Slide 1</CarouselItem>
        <CarouselItem>Slide 2</CarouselItem>
        <CarouselItem>Slide 3</CarouselItem>
      </CarouselContent>
      <CarouselPrevious />
      <CarouselNext />
    </Carousel>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/carousel.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import useEmblaCarousel, {
  type UseEmblaCarouselType,
} from 'embla-carousel-react'
import { ArrowLeft, ArrowRight } from 'lucide-react'
import { cn } from '@/lib/utils'
import { Button } from '@/components/ui/button'

type CarouselApi = UseEmblaCarouselType[1]
type UseCarouselParameters = Parameters<typeof useEmblaCarousel>
type CarouselOptions = UseCarouselParameters[0]
type CarouselPlugin = UseCarouselParameters[1]

interface CarouselProps {
  opts?: CarouselOptions
  plugins?: CarouselPlugin
  orientation?: 'horizontal' | 'vertical'
  setApi?: (api: CarouselApi) => void
}

type CarouselContextProps = {
  carouselRef: ReturnType<typeof useEmblaCarousel>[0]
  api: ReturnType<typeof useEmblaCarousel>[1]
  scrollPrev: () => void
  scrollNext: () => void
  canScrollPrev: boolean
  canScrollNext: boolean
  selectedIndex: number
  scrollSnaps: number[]
  scrollTo: (index: number) => void
} & CarouselProps

const CarouselContext = React.createContext<CarouselContextProps | null>(null)

function useCarousel() {
  const context = React.useContext(CarouselContext)

  if (!context) {
    throw new Error('useCarousel must be used within a <Carousel />')
  }

  return context
}

const Carousel = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement> & CarouselProps
>(
  (
    {
      orientation = 'horizontal',
      opts,
      setApi,
      plugins,
      className,
      children,
      ...props
    },
    ref
  ) => {
    const [carouselRef, api] = useEmblaCarousel(
      {
        ...opts,
        axis: orientation === 'horizontal' ? 'x' : 'y',
      },
      plugins
    )
    const [canScrollPrev, setCanScrollPrev] = React.useState(false)
    const [canScrollNext, setCanScrollNext] = React.useState(false)
    const [selectedIndex, setSelectedIndex] = React.useState(0)
    const [scrollSnaps, setScrollSnaps] = React.useState<number[]>([])

    const onSelect = React.useCallback((api: CarouselApi) => {
      if (!api) return

      setSelectedIndex(api.selectedScrollSnap())
      setCanScrollPrev(api.canScrollPrev())
      setCanScrollNext(api.canScrollNext())
    }, [])

    const scrollPrev = React.useCallback(() => {
      api?.scrollPrev()
    }, [api])

    const scrollNext = React.useCallback(() => {
      api?.scrollNext()
    }, [api])

    const scrollTo = React.useCallback(
      (index: number) => {
        api?.scrollTo(index)
      },
      [api]
    )

    const handleKeyDown = React.useCallback(
      (event: React.KeyboardEvent<HTMLDivElement>) => {
        if (event.key === 'ArrowLeft') {
          event.preventDefault()
          scrollPrev()
        } else if (event.key === 'ArrowRight') {
          event.preventDefault()
          scrollNext()
        }
      },
      [scrollPrev, scrollNext]
    )

    React.useEffect(() => {
      if (!api || !setApi) return
      setApi(api)
    }, [api, setApi])

    React.useEffect(() => {
      if (!api) return

      setScrollSnaps(api.scrollSnapList())
      onSelect(api)
      api.on('reInit', onSelect)
      api.on('select', onSelect)

      return () => {
        api?.off('select', onSelect)
        api?.off('reInit', onSelect)
      }
    }, [api, onSelect])

    return (
      <CarouselContext.Provider
        value={{
          carouselRef,
          api: api,
          opts,
          orientation:
            orientation || (opts?.axis === 'y' ? 'vertical' : 'horizontal'),
          scrollPrev,
          scrollNext,
          canScrollPrev,
          canScrollNext,
          selectedIndex,
          scrollSnaps,
          scrollTo,
        }}
      >
        <div
          ref={ref}
          onKeyDownCapture={handleKeyDown}
          className={cn('relative', className)}
          role="region"
          aria-roledescription="carousel"
          {...props}
        >
          {children}
        </div>
      </CarouselContext.Provider>
    )
  }
)
Carousel.displayName = 'Carousel'

const CarouselContent = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => {
  const { carouselRef, orientation } = useCarousel()

  return (
    <div ref={carouselRef} className="overflow-hidden">
      <div
        ref={ref}
        className={cn(
          'flex',
          orientation === 'horizontal' ? '-ml-4' : '-mt-4 flex-col',
          className
        )}
        {...props}
      />
    </div>
  )
})
CarouselContent.displayName = 'CarouselContent'

const CarouselItem = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => {
  const { orientation } = useCarousel()

  return (
    <div
      ref={ref}
      role="group"
      aria-roledescription="slide"
      className={cn(
        'min-w-0 shrink-0 grow-0 basis-full',
        orientation === 'horizontal' ? 'pl-4' : 'pt-4',
        className
      )}
      {...props}
    />
  )
})
CarouselItem.displayName = 'CarouselItem'

const CarouselPrevious = React.forwardRef<
  HTMLButtonElement,
  React.ComponentProps<typeof Button>
>(({ className, variant = 'outline', size = 'icon', ...props }, ref) => {
  const { orientation, scrollPrev, canScrollPrev } = useCarousel()

  return (
    <Button
      ref={ref}
      variant={variant}
      size={size}
      className={cn(
        'absolute h-10 w-10',
        // Disable translate on hover - use scale instead for carousel buttons
        'hover:translate-x-0 hover:translate-y-0 hover:scale-105 hover:shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        orientation === 'horizontal'
          ? '-left-12 top-1/2 -translate-y-1/2'
          : '-top-12 left-1/2 -translate-x-1/2 rotate-90',
        className
      )}
      disabled={!canScrollPrev}
      onClick={scrollPrev}
      {...props}
    >
      <ArrowLeft className="h-5 w-5 stroke-[3]" />
      <span className="sr-only">Previous slide</span>
    </Button>
  )
})
CarouselPrevious.displayName = 'CarouselPrevious'

const CarouselNext = React.forwardRef<
  HTMLButtonElement,
  React.ComponentProps<typeof Button>
>(({ className, variant = 'outline', size = 'icon', ...props }, ref) => {
  const { orientation, scrollNext, canScrollNext } = useCarousel()

  return (
    <Button
      ref={ref}
      variant={variant}
      size={size}
      className={cn(
        'absolute h-10 w-10',
        // Disable translate on hover - use scale instead for carousel buttons
        'hover:translate-x-0 hover:translate-y-0 hover:scale-105 hover:shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        orientation === 'horizontal'
          ? '-right-12 top-1/2 -translate-y-1/2'
          : '-bottom-12 left-1/2 -translate-x-1/2 rotate-90',
        className
      )}
      disabled={!canScrollNext}
      onClick={scrollNext}
      {...props}
    >
      <ArrowRight className="h-5 w-5 stroke-[3]" />
      <span className="sr-only">Next slide</span>
    </Button>
  )
})
CarouselNext.displayName = 'CarouselNext'

const CarouselDots = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => {
  const { selectedIndex, scrollSnaps, scrollTo } = useCarousel()

  if (scrollSnaps.length <= 1) return null

  return (
    <div
      ref={ref}
      className={cn('flex items-center justify-center gap-2 mt-4', className)}
      {...props}
    >
      {scrollSnaps.map((snap, index) => (
        <button
          key={snap}
          type="button"
          onClick={() => scrollTo(index)}
          className={cn(
            'h-3 w-3 border-2 border-foreground transition-all duration-200',
            index === selectedIndex
              ? 'bg-primary scale-110 shadow-[2px_2px_0px_hsl(var(--shadow-color))]'
              : 'bg-muted hover:bg-muted/80'
          )}
          aria-label={`Go to slide ${index + 1} of ${scrollSnaps.length}`}
          aria-current={index === selectedIndex ? 'page' : undefined}
        />
      ))}
    </div>
  )
})
CarouselDots.displayName = 'CarouselDots'

export {
  type CarouselApi,
  Carousel,
  CarouselContent,
  CarouselItem,
  CarouselPrevious,
  CarouselNext,
  CarouselDots,
  useCarousel,
}
```

---

### Chart <a name="chart"></a>

Beautiful charts with neubrutalism styling built on Recharts

- **Registry Name**: `@boldkit/chart`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/chart-demo.tsx`

```tsx
import { createChartConfig, CHART_PALETTES } from "@/components/ui/chart"

export default function ChartDemo() {
  const config = createChartConfig(["Revenue", "Expenses", "Profit"], "bold")
  return (
    <pre className="p-4 border-3 border-foreground bg-muted">
      {JSON.stringify(config, null, 2)}
    </pre>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/chart.tsx`

```tsx
import * as React from 'react'
import * as RechartsPrimitive from 'recharts'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn, sanitizeCssValue } from '@/lib/utils'

// Format: { THEME_NAME: CSS_SELECTOR }
const THEMES = { light: '', dark: '.dark' } as const

// Neubrutalism color palettes for charts
export const CHART_PALETTES = {
  bold: [
    'hsl(var(--primary))',
    'hsl(var(--secondary))',
    'hsl(var(--accent))',
    'hsl(var(--success))',
    'hsl(var(--warning))',
    'hsl(var(--info))',
  ],
  vibrant: [
    'hsl(0 84% 60%)',      // Coral red
    'hsl(174 62% 50%)',    // Teal
    'hsl(49 100% 60%)',    // Yellow
    'hsl(280 65% 60%)',    // Purple
    'hsl(145 63% 49%)',    // Green
    'hsl(212 100% 60%)',   // Blue
  ],
  pastel: [
    'hsl(0 84% 75%)',      // Light coral
    'hsl(174 62% 70%)',    // Light teal
    'hsl(49 100% 75%)',    // Light yellow
    'hsl(280 65% 75%)',    // Light purple
    'hsl(145 63% 70%)',    // Light green
    'hsl(212 100% 75%)',   // Light blue
  ],
  monochrome: [
    'hsl(var(--foreground))',
    'hsl(var(--foreground) / 0.8)',
    'hsl(var(--foreground) / 0.6)',
    'hsl(var(--foreground) / 0.4)',
    'hsl(var(--foreground) / 0.2)',
    'hsl(var(--foreground) / 0.1)',
  ],
} as const

export type ChartPalette = keyof typeof CHART_PALETTES

// Helper to get colors from a palette
export function getChartColor(palette: ChartPalette, index: number): string {
  const colors = CHART_PALETTES[palette]
  return colors[index % colors.length]
}

// Generate ChartConfig from palette
export function createChartConfig(
  keys: string[],
  labels: string[],
  palette: ChartPalette = 'bold'
): ChartConfig {
  const config: ChartConfig = {}
  keys.forEach((key, index) => {
    config[key] = {
      label: labels[index] || key,
      color: getChartColor(palette, index),
    }
  })
  return config
}

export type ChartConfig = {
  [k in string]: {
    label?: React.ReactNode
    icon?: React.ComponentType
  } & (
    | { color?: string; theme?: never }
    | { color?: never; theme: Record<keyof typeof THEMES, string> }
  )
}

type ChartContextProps = {
  config: ChartConfig
}

const ChartContext = React.createContext<ChartContextProps | null>(null)

function useChart() {
  const context = React.useContext(ChartContext)

  if (!context) {
    throw new Error('useChart must be used within a <ChartContainer />')
  }

  return context
}

const chartContainerVariants = cva(
  'flex aspect-video justify-center overflow-hidden text-xs [&_.recharts-cartesian-axis-tick_text]:fill-foreground [&_.recharts-cartesian-grid_line[stroke="#ccc"]]:stroke-muted-foreground/30 [&_.recharts-curve.recharts-tooltip-cursor]:stroke-muted-foreground [&_.recharts-polar-grid_[stroke="#ccc"]]:stroke-foreground [&_.recharts-reference-line_[stroke="#ccc"]]:stroke-foreground [&_.recharts-dot[stroke="#fff"]]:stroke-transparent [&_.recharts-sector]:outline-hidden [&_.recharts-sector[stroke="#fff"]]:stroke-foreground [&_.recharts-surface]:outline-hidden [&_.recharts-layer_path]:[fill-opacity:1] [&_.recharts-layer_path]:[stroke-width:3] [&_.recharts-layer_path]:[stroke:hsl(var(--foreground))]',
  {
    variants: {
      variant: {
        default: 'border-3 border-foreground bg-background p-4 shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        elevated: 'border-3 border-foreground bg-background p-4 shadow-[6px_6px_0px_hsl(var(--shadow-color))] hover:shadow-[8px_8px_0px_hsl(var(--shadow-color))] hover:translate-x-[-2px] hover:translate-y-[-2px] transition-all',
        flat: 'border-3 border-foreground bg-background p-4',
        filled: 'border-3 border-foreground bg-muted/30 p-4 shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        minimal: 'bg-background p-4',
        accent: 'border-3 border-foreground bg-accent/10 p-4 shadow-[4px_4px_0px_hsl(var(--accent))]',
        primary: 'border-3 border-foreground bg-primary/10 p-4 shadow-[4px_4px_0px_hsl(var(--primary))]',
      },
    },
    defaultVariants: {
      variant: 'default',
    },
  }
)

export interface ChartContainerProps
  extends React.ComponentProps<'div'>,
    VariantProps<typeof chartContainerVariants> {
  config: ChartConfig
  children: React.ComponentProps<typeof RechartsPrimitive.ResponsiveContainer>['children']
}

function ChartContainer({
  id,
  className,
  children,
  config,
  variant,
  ...props
}: ChartContainerProps) {
  const uniqueId = React.useId()
  const chartId = `chart-${id || uniqueId.replace(/:/g, '')}`

  return (
    <ChartContext.Provider value={{ config }}>
      <div
        data-slot="chart"
        data-chart={chartId}
        className={cn(chartContainerVariants({ variant }), className)}
        {...props}
      >
        <ChartStyle id={chartId} config={config} />
        <RechartsPrimitive.ResponsiveContainer>
          {children}
        </RechartsPrimitive.ResponsiveContainer>
      </div>
    </ChartContext.Provider>
  )
}

const ChartStyle = ({ id, config }: { id: string; config: ChartConfig }) => {
  const colorConfig = Object.entries(config).filter(
    ([, configItem]) => configItem.theme || configItem.color
  )

  if (!colorConfig.length) {
    return null
  }

  const safeId = id.replace(/[^a-zA-Z0-9_-]/g, '')

  return (
    <style
      dangerouslySetInnerHTML={{
        __html: Object.entries(THEMES)
          .map(
            ([theme, prefix]) => `
${prefix} [data-chart=${safeId}] {
${colorConfig
  .map(([key, itemConfig]) => {
    const color =
      itemConfig.theme?.[theme as keyof typeof itemConfig.theme] ||
      itemConfig.color
    return color ? `  --color-${sanitizeCssValue(key)}: ${sanitizeCssValue(color)};` : null
  })
  .join('\n')}
}
`
          )
          .join('\n'),
      }}
    />
  )
}

const ChartTooltip = RechartsPrimitive.Tooltip

interface ChartTooltipContentProps extends React.ComponentProps<'div'> {
  active?: boolean
  payload?: Array<{
    name?: string
    value?: number
    dataKey?: string
    color?: string
    payload?: Record<string, unknown>
    fill?: string
  }>
  label?: string
  hideLabel?: boolean
  hideIndicator?: boolean
  indicator?: 'line' | 'dot' | 'dashed'
  nameKey?: string
  labelKey?: string
  labelFormatter?: (label: unknown, payload: unknown[]) => React.ReactNode
  formatter?: (
    value: unknown,
    name: unknown,
    item: unknown,
    index: number,
    payload: unknown
  ) => React.ReactNode
  labelClassName?: string
  color?: string
}

function ChartTooltipContent({
  active,
  payload,
  className,
  indicator = 'dot',
  hideLabel = false,
  hideIndicator = false,
  label,
  labelFormatter,
  labelClassName,
  formatter,
  color,
  nameKey,
  labelKey,
}: ChartTooltipContentProps) {
  const { config } = useChart()

  const tooltipLabel = React.useMemo(() => {
    if (hideLabel || !payload?.length) {
      return null
    }

    const [item] = payload
    const key = `${labelKey || item?.dataKey || item?.name || 'value'}`
    const itemConfig = getPayloadConfigFromPayload(config, item, key)
    const value =
      !labelKey && typeof label === 'string'
        ? config[label as keyof typeof config]?.label || label
        : itemConfig?.label

    if (labelFormatter) {
      return (
        <div className={cn('font-bold uppercase tracking-wide', labelClassName)}>
          {labelFormatter(value, payload)}
        </div>
      )
    }

    if (!value) {
      return null
    }

    return <div className={cn('font-bold', labelClassName)}>{value}</div>
  }, [label, labelFormatter, payload, hideLabel, labelClassName, config, labelKey])

  if (!active || !payload?.length) {
    return null
  }

  const nestLabel = payload.length === 1 && indicator !== 'dot'

  return (
    <div
      className={cn(
        'grid min-w-[8rem] items-start gap-1.5 border-3 border-foreground bg-background px-2.5 py-1.5 text-xs shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        className
      )}
    >
      {!nestLabel ? tooltipLabel : null}
      <div className="grid gap-1.5">
        {payload.map((item, index) => {
          const key = `${nameKey || item.name || item.dataKey || 'value'}`
          const itemConfig = getPayloadConfigFromPayload(config, item, key)
          const indicatorColor = color || (item.payload as Record<string, string>)?.fill || item.color

          return (
            <div
              key={item.dataKey || index}
              className={cn(
                'flex w-full flex-wrap items-stretch gap-2 [&>svg]:h-2.5 [&>svg]:w-2.5 [&>svg]:text-muted-foreground',
                indicator === 'dot' && 'items-center'
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
                        className={cn('shrink-0 border-2 border-foreground', {
                          'h-2.5 w-2.5': indicator === 'dot',
                          'w-1': indicator === 'line',
                          'w-0 border-[1.5px] border-dashed bg-transparent':
                            indicator === 'dashed',
                          'my-0.5': nestLabel && indicator === 'dashed',
                        })}
                        style={{
                          backgroundColor: indicatorColor,
                        }}
                      />
                    )
                  )}
                  <div
                    className={cn(
                      'flex flex-1 justify-between leading-none',
                      nestLabel ? 'items-end' : 'items-center'
                    )}
                  >
                    <div className="grid gap-1.5">
                      {nestLabel ? tooltipLabel : null}
                      <span className="text-muted-foreground">
                        {itemConfig?.label || item.name}
                      </span>
                    </div>
                    {item.value !== undefined && (
                      <span className="font-mono font-bold tabular-nums text-foreground">
                        {item.value.toLocaleString()}
                      </span>
                    )}
                  </div>
                </>
              )}
            </div>
          )
        })}
      </div>
    </div>
  )
}

const ChartLegend = RechartsPrimitive.Legend

interface ChartLegendContentProps extends React.ComponentProps<'div'> {
  payload?: Array<{
    value?: string
    dataKey?: string
    color?: string
  }>
  verticalAlign?: 'top' | 'bottom'
  hideIcon?: boolean
  nameKey?: string
}

function ChartLegendContent({
  className,
  hideIcon = false,
  payload,
  verticalAlign = 'bottom',
  nameKey,
}: ChartLegendContentProps) {
  const { config } = useChart()

  if (!payload?.length) {
    return null
  }

  return (
    <div
      className={cn(
        'flex items-center justify-center gap-4 font-bold uppercase tracking-wide',
        verticalAlign === 'top' ? 'pb-3' : 'pt-3',
        className
      )}
    >
      {payload.map((item) => {
        const key = `${nameKey || item.dataKey || 'value'}`
        const itemConfig = getPayloadConfigFromPayload(config, item, key)

        return (
          <div
            key={item.value}
            className={cn(
              'flex items-center gap-1.5 [&>svg]:h-3 [&>svg]:w-3 [&>svg]:text-foreground'
            )}
          >
            {itemConfig?.icon && !hideIcon ? (
              <itemConfig.icon />
            ) : (
              <div
                className="h-3 w-3 shrink-0 border-2 border-foreground"
                style={{
                  backgroundColor: item.color,
                }}
              />
            )}
            <span className="text-sm">{itemConfig?.label}</span>
          </div>
        )
      })}
    </div>
  )
}

// Helper to extract item config from a payload.
function getPayloadConfigFromPayload(
  config: ChartConfig,
  payload: unknown,
  key: string
) {
  if (typeof payload !== 'object' || payload === null) {
    return undefined
  }

  const payloadPayload =
    'payload' in payload &&
    typeof payload.payload === 'object' &&
    payload.payload !== null
      ? payload.payload
      : undefined

  let configLabelKey: string = key

  if (
    key in payload &&
    typeof payload[key as keyof typeof payload] === 'string'
  ) {
    configLabelKey = payload[key as keyof typeof payload] as string
  } else if (
    payloadPayload &&
    key in payloadPayload &&
    typeof payloadPayload[key as keyof typeof payloadPayload] === 'string'
  ) {
    configLabelKey = payloadPayload[
      key as keyof typeof payloadPayload
    ] as string
  }

  return configLabelKey in config
    ? config[configLabelKey]
    : config[key as keyof typeof config]
}

// ──────────────────────────────────────────────────────────────────
// ChartEmpty — fallback display when a chart receives no data.
// Originally lived in src/components/ui/chart/empty.tsx, merged here so
// that synced chart-X files can do `import { ChartEmpty } from './chart'`
// without needing the split-file structure on the consumer side.
// ──────────────────────────────────────────────────────────────────

export interface ChartEmptyProps extends React.HTMLAttributes<HTMLDivElement> {
  message?: React.ReactNode
}

const ChartEmpty = React.forwardRef<HTMLDivElement, ChartEmptyProps>(
  ({ message = 'No data', className, ...props }, ref) => {
    return (
      <div
        ref={ref}
        role="status"
        aria-live="polite"
        className={cn(
          'flex min-h-[120px] w-full items-center justify-center border-3 border-dashed border-foreground/40 bg-muted/20 p-6 text-xs font-bold uppercase tracking-wide text-muted-foreground',
          className
        )}
        {...props}
      >
        {message}
      </div>
    )
  }
)
ChartEmpty.displayName = 'ChartEmpty'

export {
  ChartContainer,
  ChartTooltip,
  ChartTooltipContent,
  ChartLegend,
  ChartLegendContent,
  ChartStyle,
  ChartEmpty,
  chartContainerVariants,
}
```

---

### Checkbox <a name="checkbox"></a>

A control that allows the user to toggle between checked and not checked

- **Registry Name**: `@boldkit/checkbox`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/checkbox.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-checkbox`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/label`

#### How to Use

##### File: `registry/default/example/checkbox-demo.tsx`

```tsx
import { Checkbox } from '@/components/ui/checkbox'
import { Label } from '@/components/ui/label'

export default function Example() {
  return (
    <div className="flex items-center space-x-2">
      <Checkbox id="terms" />
      <Label htmlFor="terms">Accept terms and conditions</Label>
    </div>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/checkbox.tsx`

```tsx
import * as React from 'react'
import * as CheckboxPrimitive from '@radix-ui/react-checkbox'
import { Check } from 'lucide-react'
import { cn } from '@/lib/utils'

const Checkbox = React.forwardRef<
  React.ElementRef<typeof CheckboxPrimitive.Root>,
  React.ComponentPropsWithoutRef<typeof CheckboxPrimitive.Root>
>(({ className, ...props }, ref) => (
  <CheckboxPrimitive.Root
    ref={ref}
    className={cn(
      'peer h-5 w-5 shrink-0 border-3 border-foreground bg-background shadow-[4px_4px_0px_hsl(var(--shadow-color))] transition-all duration-200 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 data-[state=checked]:bg-primary data-[state=checked]:text-primary-foreground hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none active:translate-x-[4px] active:translate-y-[4px] active:shadow-none',
      className
    )}
    {...props}
  >
    <CheckboxPrimitive.Indicator
      className={cn('flex items-center justify-center text-current')}
    >
      <Check className="h-4 w-4 stroke-[3]" />
    </CheckboxPrimitive.Indicator>
  </CheckboxPrimitive.Root>
))
Checkbox.displayName = CheckboxPrimitive.Root.displayName

export { Checkbox }
```

---

### Collapsible <a name="collapsible"></a>

An interactive component which expands/collapses a panel

- **Registry Name**: `@boldkit/collapsible`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/collapsible.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-collapsible`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`

#### How to Use

##### File: `registry/default/example/collapsible-demo.tsx`

```tsx
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from '@/components/ui/collapsible'
import { Button } from '@/components/ui/button'
import { ChevronsUpDown } from 'lucide-react'

export default function Example() {
  return (
    <Collapsible>
      <CollapsibleTrigger asChild>
        <Button variant="outline">
          Toggle <ChevronsUpDown className="ml-2 h-4 w-4" />
        </Button>
      </CollapsibleTrigger>
      <CollapsibleContent className="mt-2">
        <div className="border-3 border-foreground p-4">
          Collapsible content here.
        </div>
      </CollapsibleContent>
    </Collapsible>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/collapsible.tsx`

```tsx
import * as React from 'react'
import * as CollapsiblePrimitive from '@radix-ui/react-collapsible'
import { cn } from '@/lib/utils'

const Collapsible = CollapsiblePrimitive.Root

const CollapsibleTrigger = React.forwardRef<
  React.ElementRef<typeof CollapsiblePrimitive.CollapsibleTrigger>,
  React.ComponentPropsWithoutRef<typeof CollapsiblePrimitive.CollapsibleTrigger>
>(({ className, ...props }, ref) => (
  <CollapsiblePrimitive.CollapsibleTrigger
    ref={ref}
    className={cn(
      'transition-all duration-150 [&[data-state=open]>svg]:rotate-180',
      className
    )}
    {...props}
  />
))
CollapsibleTrigger.displayName = CollapsiblePrimitive.CollapsibleTrigger.displayName

const CollapsibleContent = React.forwardRef<
  React.ElementRef<typeof CollapsiblePrimitive.CollapsibleContent>,
  React.ComponentPropsWithoutRef<typeof CollapsiblePrimitive.CollapsibleContent>
>(({ className, ...props }, ref) => (
  <CollapsiblePrimitive.CollapsibleContent
    ref={ref}
    className={cn(
      'overflow-hidden transition-all data-[state=closed]:animate-collapsible-up data-[state=open]:animate-collapsible-down',
      className
    )}
    {...props}
  />
))
CollapsibleContent.displayName = CollapsiblePrimitive.CollapsibleContent.displayName

export { Collapsible, CollapsibleTrigger, CollapsibleContent }
```

---

### Combobox <a name="combobox"></a>

Searchable dropdown built by composing Popover and Command with neubrutalism styling

- **Registry Name**: `@boldkit/combobox`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/combobox.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/popover`
  - `@boldkit/command`

#### How to Use

##### File: `registry/default/example/combobox-demo.tsx`

```tsx
import { useState } from 'react'
import { Check } from 'lucide-react'
import { cn } from '@/lib/utils'
import {
  Combobox, ComboboxContent, ComboboxEmpty, ComboboxGroup,
  ComboboxInput, ComboboxItem, ComboboxList, ComboboxTrigger,
} from '@/components/ui/combobox'

const frameworks = [
  { value: 'next.js', label: 'Next.js' },
  { value: 'sveltekit', label: 'SvelteKit' },
  { value: 'nuxt.js', label: 'Nuxt.js' },
]

export default function Example() {
  const [open, setOpen] = useState(false)
  const [value, setValue] = useState('')

  return (
    <Combobox open={open} onOpenChange={setOpen}>
      <ComboboxTrigger
        className="w-[200px]"
        open={open}
        value={frameworks.find(f => f.value === value)?.label}
        placeholder="Select framework..."
      />
      <ComboboxContent className="w-[200px]">
        <ComboboxInput placeholder="Search framework..." />
        <ComboboxList>
          <ComboboxEmpty>No framework found.</ComboboxEmpty>
          <ComboboxGroup>
            {frameworks.map(f => (
              <ComboboxItem
                key={f.value}
                value={f.value}
                onSelect={current => {
                  setValue(current === value ? '' : current)
                  setOpen(false)
                }}
              >
                {f.label}
                <Check
                  className={cn(
                    'ml-auto h-4 w-4 stroke-[3]',
                    value === f.value ? 'opacity-100' : 'opacity-0'
                  )}
                />
              </ComboboxItem>
            ))}
          </ComboboxGroup>
        </ComboboxList>
      </ComboboxContent>
    </Combobox>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/combobox.tsx`

```tsx
import * as React from 'react'
import { ChevronsUpDown, X } from 'lucide-react'
import { cn } from '@/lib/utils'
import { Popover, PopoverContent, PopoverTrigger } from '@/components/ui/popover'
import {
  Command,
  CommandEmpty,
  CommandGroup,
  CommandInput,
  CommandItem,
  CommandList,
  CommandSeparator,
} from '@/components/ui/command'

const Combobox = Popover

const ComboboxTrigger = React.forwardRef<
  HTMLButtonElement,
  React.ButtonHTMLAttributes<HTMLButtonElement> & {
    placeholder?: string
    value?: string
    open?: boolean
  }
>(({ className, placeholder = 'Select...', value, open, ...props }, ref) => (
  <PopoverTrigger asChild>
    <button
      ref={ref}
      role="combobox"
      aria-expanded={open}
      className={cn(
        'flex h-11 w-full items-center justify-between border-3 border-input bg-background px-4 py-2 text-sm font-medium shadow-[4px_4px_0px_hsl(var(--shadow-color))] focus:outline-none focus:translate-x-[4px] focus:translate-y-[4px] focus:shadow-none disabled:cursor-not-allowed disabled:opacity-50 transition-all duration-200',
        className
      )}
      {...props}
    >
      <span className={cn('truncate', !value && 'text-muted-foreground')}>
        {value || placeholder}
      </span>
      <ChevronsUpDown className="ml-2 h-4 w-4 shrink-0 opacity-50 stroke-[2.5]" />
    </button>
  </PopoverTrigger>
))
ComboboxTrigger.displayName = 'ComboboxTrigger'

/**
 * Multi-select trigger. Each entry in `values` has a `value` (identifier)
 * and a `label` (display text). The `onRemove` callback receives the `value`
 * of the chip that was dismissed, so you can remove it from state directly.
 */
const ComboboxMultiTrigger = React.forwardRef<
  HTMLButtonElement,
  React.ButtonHTMLAttributes<HTMLButtonElement> & {
    placeholder?: string
    values?: { value: string; label: string }[]
    open?: boolean
    onRemove?: (value: string) => void
  }
>(({ className, placeholder = 'Select...', values = [], open, onRemove, ...props }, ref) => (
  <PopoverTrigger asChild>
    <button
      ref={ref}
      role="combobox"
      aria-expanded={open}
      className={cn(
        'flex min-h-11 w-full flex-wrap items-center gap-1.5 border-3 border-input bg-background px-3 py-2 text-sm font-medium shadow-[4px_4px_0px_hsl(var(--shadow-color))] focus:outline-none focus:translate-x-[4px] focus:translate-y-[4px] focus:shadow-none disabled:cursor-not-allowed disabled:opacity-50 transition-all duration-200',
        className
      )}
      {...props}
    >
      <span className="flex flex-1 flex-wrap items-center gap-1">
        {values.length > 0 ? (
          values.map(({ value, label }) => (
            <span
              key={value}
              className="flex items-center gap-1 border-2 border-foreground bg-accent px-1.5 py-0.5 text-xs font-bold"
            >
              {label}
              <X
                className="h-3 w-3 cursor-pointer stroke-[3] hover:opacity-70"
                onClick={e => {
                  e.stopPropagation()
                  onRemove?.(value)
                }}
              />
            </span>
          ))
        ) : (
          <span className="text-muted-foreground">{placeholder}</span>
        )}
      </span>
      <ChevronsUpDown className="ml-auto h-4 w-4 shrink-0 opacity-50 stroke-[2.5]" />
    </button>
  </PopoverTrigger>
))
ComboboxMultiTrigger.displayName = 'ComboboxMultiTrigger'

const ComboboxContent = React.forwardRef<
  React.ElementRef<typeof PopoverContent>,
  React.ComponentPropsWithoutRef<typeof PopoverContent>
>(({ className, children, align = 'start', ...props }, ref) => (
  <PopoverContent ref={ref} align={align} className={cn('p-0', className)} {...props}>
    <Command>{children}</Command>
  </PopoverContent>
))
ComboboxContent.displayName = 'ComboboxContent'

export {
  Combobox,
  ComboboxTrigger,
  ComboboxMultiTrigger,
  ComboboxContent,
  CommandInput as ComboboxInput,
  CommandList as ComboboxList,
  CommandEmpty as ComboboxEmpty,
  CommandGroup as ComboboxGroup,
  CommandItem as ComboboxItem,
  CommandSeparator as ComboboxSeparator,
}
```

---

### Command <a name="command"></a>

Fast, composable, command menu for React with neubrutalism styling

- **Registry Name**: `@boldkit/command`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/command.json
```

#### Dependencies

- **NPM Packages**:
  - `cmdk`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/dialog`

#### How to Use

##### File: `registry/default/example/command-demo.tsx`

```tsx
import {
  Command,
  CommandEmpty,
  CommandGroup,
  CommandInput,
  CommandItem,
  CommandList,
} from '@/components/ui/command'

export default function Example() {
  return (
    <Command>
      <CommandInput placeholder="Type a command or search..." />
      <CommandList>
        <CommandEmpty>No results found.</CommandEmpty>
        <CommandGroup heading="Suggestions">
          <CommandItem>Calendar</CommandItem>
          <CommandItem>Search</CommandItem>
          <CommandItem>Settings</CommandItem>
        </CommandGroup>
      </CommandList>
    </Command>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/command.tsx`

```tsx
import * as React from 'react'
import * as DialogPrimitive from '@radix-ui/react-dialog'
import { type DialogProps } from '@radix-ui/react-dialog'
import { Command as CommandPrimitive } from 'cmdk'
import { Search } from 'lucide-react'
import { cn } from '@/lib/utils'
import { Dialog, DialogPortal, DialogOverlay } from '@/components/ui/dialog'

const Command = React.forwardRef<
  React.ElementRef<typeof CommandPrimitive>,
  React.ComponentPropsWithoutRef<typeof CommandPrimitive>
>(({ className, ...props }, ref) => (
  <CommandPrimitive
    ref={ref}
    className={cn(
      'flex h-full w-full flex-col overflow-hidden bg-popover text-popover-foreground',
      className
    )}
    {...props}
  />
))
Command.displayName = CommandPrimitive.displayName

const CommandDialog = ({ children, ...props }: DialogProps) => {
  return (
    <Dialog {...props}>
      <DialogPortal>
        <DialogOverlay />
        <DialogPrimitive.Content
          className="fixed left-[50%] top-[20%] z-50 w-full max-w-lg translate-x-[-50%] border-3 border-foreground bg-background shadow-[8px_8px_0px_hsl(var(--shadow-color))] duration-200 data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95"
        >
          <Command className="[&_[cmdk-group-heading]]:px-3 [&_[cmdk-group-heading]]:py-2 [&_[cmdk-group-heading]]:text-xs [&_[cmdk-group-heading]]:font-bold [&_[cmdk-group-heading]]:uppercase [&_[cmdk-group-heading]]:tracking-wide [&_[cmdk-group-heading]]:text-muted-foreground [&_[cmdk-group]:not([hidden])_~[cmdk-group]]:pt-0 [&_[cmdk-input-wrapper]_svg]:h-5 [&_[cmdk-input-wrapper]_svg]:w-5 [&_[cmdk-input]]:h-12">
            {children}
          </Command>
        </DialogPrimitive.Content>
      </DialogPortal>
    </Dialog>
  )
}

const CommandInput = React.forwardRef<
  React.ElementRef<typeof CommandPrimitive.Input>,
  React.ComponentPropsWithoutRef<typeof CommandPrimitive.Input>
>(({ className, ...props }, ref) => (
  <div className="flex items-center border-b-3 border-foreground px-4 bg-background" cmdk-input-wrapper="">
    <Search className="mr-3 h-5 w-5 shrink-0 text-muted-foreground stroke-[2.5]" />
    <CommandPrimitive.Input
      ref={ref}
      className={cn(
        'flex h-12 w-full bg-transparent py-3 text-base placeholder:text-muted-foreground disabled:cursor-not-allowed disabled:opacity-50',
        className
      )}
      style={{ outline: 'none', boxShadow: 'none' }}
      {...props}
    />
  </div>
))

CommandInput.displayName = CommandPrimitive.Input.displayName

const CommandList = React.forwardRef<
  React.ElementRef<typeof CommandPrimitive.List>,
  React.ComponentPropsWithoutRef<typeof CommandPrimitive.List>
>(({ className, ...props }, ref) => (
  <CommandPrimitive.List
    ref={ref}
    className={cn('max-h-[400px] overflow-y-auto overflow-x-hidden p-2', className)}
    {...props}
  />
))

CommandList.displayName = CommandPrimitive.List.displayName

const CommandEmpty = React.forwardRef<
  React.ElementRef<typeof CommandPrimitive.Empty>,
  React.ComponentPropsWithoutRef<typeof CommandPrimitive.Empty>
>((props, ref) => (
  <CommandPrimitive.Empty
    ref={ref}
    className="py-6 text-center text-sm"
    {...props}
  />
))

CommandEmpty.displayName = CommandPrimitive.Empty.displayName

const CommandGroup = React.forwardRef<
  React.ElementRef<typeof CommandPrimitive.Group>,
  React.ComponentPropsWithoutRef<typeof CommandPrimitive.Group>
>(({ className, ...props }, ref) => (
  <CommandPrimitive.Group
    ref={ref}
    className={cn(
      'overflow-hidden p-1 text-foreground [&_[cmdk-group-heading]]:px-2 [&_[cmdk-group-heading]]:py-1.5 [&_[cmdk-group-heading]]:text-xs [&_[cmdk-group-heading]]:font-bold [&_[cmdk-group-heading]]:uppercase [&_[cmdk-group-heading]]:tracking-wide [&_[cmdk-group-heading]]:text-muted-foreground',
      className
    )}
    {...props}
  />
))

CommandGroup.displayName = CommandPrimitive.Group.displayName

const CommandSeparator = React.forwardRef<
  React.ElementRef<typeof CommandPrimitive.Separator>,
  React.ComponentPropsWithoutRef<typeof CommandPrimitive.Separator>
>(({ className, ...props }, ref) => (
  <CommandPrimitive.Separator
    ref={ref}
    className={cn('-mx-1 h-px bg-foreground', className)}
    {...props}
  />
))
CommandSeparator.displayName = CommandPrimitive.Separator.displayName

const CommandItem = React.forwardRef<
  React.ElementRef<typeof CommandPrimitive.Item>,
  React.ComponentPropsWithoutRef<typeof CommandPrimitive.Item>
>(({ className, ...props }, ref) => (
  <CommandPrimitive.Item
    ref={ref}
    className={cn(
      'relative flex cursor-pointer gap-2 select-none items-center px-3 py-2.5 text-sm font-medium outline-none transition-colors data-[disabled=true]:pointer-events-none data-[selected=true]:bg-muted data-[selected=true]:border-l-3 data-[selected=true]:border-foreground data-[disabled=true]:opacity-50 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0 hover:bg-muted/50',
      className
    )}
    {...props}
  />
))

CommandItem.displayName = CommandPrimitive.Item.displayName

const CommandShortcut = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLSpanElement>) => {
  return (
    <span
      className={cn(
        'ml-auto text-xs tracking-widest text-muted-foreground',
        className
      )}
      {...props}
    />
  )
}
CommandShortcut.displayName = 'CommandShortcut'

export {
  Command,
  CommandDialog,
  CommandInput,
  CommandList,
  CommandEmpty,
  CommandGroup,
  CommandItem,
  CommandShortcut,
  CommandSeparator,
}
```

---

### Data Table <a name="data-table"></a>

Powerful data table with sorting, filtering, pagination, and row selection

- **Registry Name**: `@boldkit/data-table`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/data-table.json
```

#### Dependencies

- **NPM Packages**:
  - `@tanstack/react-table`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`
  - `@boldkit/checkbox`
  - `@boldkit/dropdown-menu`
  - `@boldkit/input`
  - `@boldkit/select`
  - `@boldkit/table`

#### How to Use

##### File: `registry/default/example/data-table-demo.tsx`

```tsx
import { DataTable, DataTableColumnHeader } from '@/components/ui/data-table'

const columns = [
  {
    accessorKey: 'name',
    header: ({ column }) => <DataTableColumnHeader column={column} title="Name" />,
  },
  // ... more columns
]

export default function Example() {
  return <DataTable columns={columns} data={data} />
}
```

#### Component Source Code

##### File: `registry/default/ui/data-table.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import {
  flexRender,
  getCoreRowModel,
  getFilteredRowModel,
  getPaginationRowModel,
  getSortedRowModel,
  useReactTable,
  type ColumnDef,
  type ColumnFiltersState,
  type SortingState,
  type VisibilityState,
  type Table as TanstackTable,
} from '@tanstack/react-table'
import { ArrowDown, ArrowUp, ArrowUpDown, ChevronDown, Search, Settings2 } from 'lucide-react'
import { cn } from '@/lib/utils'
import { Button } from '@/components/ui/button'
import { Checkbox } from '@/components/ui/checkbox'
import { Input } from '@/components/ui/input'
import {
  DropdownMenu,
  DropdownMenuCheckboxItem,
  DropdownMenuContent,
  DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu'
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table'

// DataTable Context
interface DataTableContextValue<TData> {
  table: TanstackTable<TData>
}

const DataTableContext = React.createContext<DataTableContextValue<unknown> | null>(null)

function useDataTable<TData>() {
  const context = React.useContext(DataTableContext)
  if (!context) {
    throw new Error('DataTable components must be used within a <DataTable />')
  }
  return context as DataTableContextValue<TData>
}

// Column Header with sorting
interface DataTableColumnHeaderProps<TData, TValue>
  extends React.HTMLAttributes<HTMLDivElement> {
  column: import('@tanstack/react-table').Column<TData, TValue>
  title: string
}

function DataTableColumnHeader<TData, TValue>({
  column,
  title,
  className,
}: DataTableColumnHeaderProps<TData, TValue>) {
  if (!column.getCanSort()) {
    return <div className={cn(className)}>{title}</div>
  }

  return (
    <Button
      variant="ghost"
      size="sm"
      className={cn('-ml-3 h-8 data-[state=open]:bg-accent', className)}
      onClick={() => column.toggleSorting(column.getIsSorted() === 'asc')}
    >
      <span>{title}</span>
      {column.getIsSorted() === 'desc' ? (
        <ArrowDown className="ml-2 h-4 w-4" />
      ) : column.getIsSorted() === 'asc' ? (
        <ArrowUp className="ml-2 h-4 w-4" />
      ) : (
        <ArrowUpDown className="ml-2 h-4 w-4" />
      )}
    </Button>
  )
}

// Toolbar
interface DataTableToolbarProps<TData> {
  table: TanstackTable<TData>
  filterPlaceholder?: string
  filterColumn?: string
  showColumnVisibility?: boolean
}

function DataTableToolbar<TData>({
  table,
  filterPlaceholder = 'Filter...',
  filterColumn,
  showColumnVisibility = true,
}: DataTableToolbarProps<TData>) {
  const column = filterColumn ? table.getColumn(filterColumn) : null

  return (
    <div className="flex items-center justify-between py-4">
      <div className="flex flex-1 items-center space-x-2">
        {column && (
          <div className="relative">
            <Search className="absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2 text-muted-foreground" />
            <Input
              placeholder={filterPlaceholder}
              value={String(column.getFilterValue() ?? '')}
              onChange={(event) => column.setFilterValue(event.target.value)}
              className="h-9 w-[150px] pl-9 lg:w-[250px]"
            />
          </div>
        )}
      </div>
      {showColumnVisibility && (
        <DropdownMenu>
          <DropdownMenuTrigger asChild>
            <Button variant="outline" size="sm" className="ml-auto h-9">
              <Settings2 className="mr-2 h-4 w-4" />
              Columns
              <ChevronDown className="ml-2 h-4 w-4" />
            </Button>
          </DropdownMenuTrigger>
          <DropdownMenuContent align="end">
            {table
              .getAllColumns()
              .filter((column) => column.getCanHide())
              .map((column) => {
                return (
                  <DropdownMenuCheckboxItem
                    key={column.id}
                    className="capitalize"
                    checked={column.getIsVisible()}
                    onCheckedChange={(value) => column.toggleVisibility(!!value)}
                  >
                    {column.id}
                  </DropdownMenuCheckboxItem>
                )
              })}
          </DropdownMenuContent>
        </DropdownMenu>
      )}
    </div>
  )
}

// Pagination
interface DataTablePaginationProps<TData> {
  table: TanstackTable<TData>
  pageSizeOptions?: number[]
}

function DataTablePagination<TData>({
  table,
  pageSizeOptions = [10, 20, 30, 50],
}: DataTablePaginationProps<TData>) {
  return (
    <div className="flex items-center justify-between py-4">
      <div className="flex-1 text-sm text-muted-foreground">
        {table.getFilteredSelectedRowModel().rows.length > 0 && (
          <>
            {table.getFilteredSelectedRowModel().rows.length} of{' '}
            {table.getFilteredRowModel().rows.length} row(s) selected.
          </>
        )}
      </div>
      <div className="flex items-center space-x-6 lg:space-x-8">
        <div className="flex items-center space-x-2">
          <p className="text-sm font-medium">Rows per page</p>
          <Select
            value={`${table.getState().pagination.pageSize}`}
            onValueChange={(value) => {
              table.setPageSize(Number(value))
            }}
          >
            <SelectTrigger className="h-9 w-[85px]">
              <SelectValue placeholder={table.getState().pagination.pageSize} />
            </SelectTrigger>
            <SelectContent side="top">
              {pageSizeOptions.map((pageSize) => (
                <SelectItem key={pageSize} value={`${pageSize}`}>
                  {pageSize}
                </SelectItem>
              ))}
            </SelectContent>
          </Select>
        </div>
        <div className="flex w-[100px] items-center justify-center text-sm font-medium">
          Page {table.getState().pagination.pageIndex + 1} of{' '}
          {table.getPageCount()}
        </div>
        <div className="flex items-center space-x-2">
          <Button
            variant="outline"
            size="sm"
            onClick={() => table.previousPage()}
            disabled={!table.getCanPreviousPage()}
          >
            Previous
          </Button>
          <Button
            variant="outline"
            size="sm"
            onClick={() => table.nextPage()}
            disabled={!table.getCanNextPage()}
          >
            Next
          </Button>
        </div>
      </div>
    </div>
  )
}

// Main DataTable component
export interface DataTableProps<TData, TValue> {
  columns: ColumnDef<TData, TValue>[]
  data: TData[]

  // Features
  enableSorting?: boolean
  enableFiltering?: boolean
  enableColumnVisibility?: boolean
  enableRowSelection?: boolean
  enablePagination?: boolean

  // Pagination
  pageSize?: number
  pageSizeOptions?: number[]

  // Search
  filterColumn?: string
  filterPlaceholder?: string

  // Empty/Loading
  emptyMessage?: string
  isLoading?: boolean

  // Callbacks
  onRowSelectionChange?: (selectedRows: TData[]) => void
}

function DataTable<TData, TValue>({
  columns,
  data,
  enableSorting = true,
  enableFiltering = true,
  enableColumnVisibility = true,
  enableRowSelection = false,
  enablePagination = true,
  pageSize = 10,
  pageSizeOptions = [10, 20, 30, 50],
  filterColumn,
  filterPlaceholder = 'Filter...',
  emptyMessage = 'No results.',
  isLoading = false,
  onRowSelectionChange,
}: DataTableProps<TData, TValue>) {
  const [sorting, setSorting] = React.useState<SortingState>([])
  const [columnFilters, setColumnFilters] = React.useState<ColumnFiltersState>([])
  const [columnVisibility, setColumnVisibility] = React.useState<VisibilityState>({})
  const [rowSelection, setRowSelection] = React.useState({})

  // Add selection column if row selection is enabled
  const tableColumns = React.useMemo(() => {
    if (!enableRowSelection) return columns

    const selectionColumn: ColumnDef<TData, unknown> = {
      id: 'select',
      header: ({ table }) => (
        <Checkbox
          checked={
            table.getIsAllPageRowsSelected() ||
            (table.getIsSomePageRowsSelected() && 'indeterminate')
          }
          onCheckedChange={(value) => table.toggleAllPageRowsSelected(!!value)}
          aria-label="Select all"
        />
      ),
      cell: ({ row }) => (
        <Checkbox
          checked={row.getIsSelected()}
          onCheckedChange={(value) => row.toggleSelected(!!value)}
          aria-label="Select row"
        />
      ),
      enableSorting: false,
      enableHiding: false,
    }

    return [selectionColumn, ...columns]
  }, [columns, enableRowSelection])

  const table = useReactTable({
    data,
    columns: tableColumns,
    onSortingChange: setSorting,
    onColumnFiltersChange: setColumnFilters,
    getCoreRowModel: getCoreRowModel(),
    ...(enablePagination && { getPaginationRowModel: getPaginationRowModel() }),
    ...(enableSorting && { getSortedRowModel: getSortedRowModel() }),
    ...(enableFiltering && { getFilteredRowModel: getFilteredRowModel() }),
    onColumnVisibilityChange: setColumnVisibility,
    onRowSelectionChange: setRowSelection,
    state: {
      sorting,
      columnFilters,
      columnVisibility,
      rowSelection,
    },
    initialState: {
      pagination: {
        pageSize,
      },
    },
  })

  // Notify parent of selection changes
  React.useEffect(() => {
    if (onRowSelectionChange) {
      const selectedRows = table.getFilteredSelectedRowModel().rows.map((row) => row.original)
      onRowSelectionChange(selectedRows)
    }
  }, [rowSelection, table, onRowSelectionChange])

  // Memoize context value to prevent unnecessary re-renders
  const contextValue = React.useMemo(
    () => ({ table: table as TanstackTable<unknown> }),
    [table]
  )

  return (
    <DataTableContext.Provider value={contextValue}>
      <div className="space-y-4">
        {/* Toolbar */}
        {(enableFiltering || enableColumnVisibility) && (
          <DataTableToolbar
            table={table}
            filterPlaceholder={filterPlaceholder}
            filterColumn={filterColumn}
            showColumnVisibility={enableColumnVisibility}
          />
        )}

        {/* Table */}
        <Table>
          <TableHeader>
            {table.getHeaderGroups().map((headerGroup) => (
              <TableRow key={headerGroup.id}>
                {headerGroup.headers.map((header) => {
                  return (
                    <TableHead key={header.id}>
                      {header.isPlaceholder
                        ? null
                        : flexRender(
                            header.column.columnDef.header,
                            header.getContext()
                          )}
                    </TableHead>
                  )
                })}
              </TableRow>
            ))}
          </TableHeader>
          <TableBody>
            {isLoading ? (
              <TableRow>
                <TableCell
                  colSpan={tableColumns.length}
                  className="h-24 text-center"
                >
                  <div className="flex items-center justify-center">
                    <div className="h-6 w-6 animate-spin border-3 border-foreground border-t-transparent" />
                  </div>
                </TableCell>
              </TableRow>
            ) : table.getRowModel().rows?.length ? (
              table.getRowModel().rows.map((row) => (
                <TableRow
                  key={row.id}
                  data-state={row.getIsSelected() && 'selected'}
                >
                  {row.getVisibleCells().map((cell) => (
                    <TableCell key={cell.id}>
                      {flexRender(
                        cell.column.columnDef.cell,
                        cell.getContext()
                      )}
                    </TableCell>
                  ))}
                </TableRow>
              ))
            ) : (
              <TableRow>
                <TableCell
                  colSpan={tableColumns.length}
                  className="h-24 text-center"
                >
                  {emptyMessage}
                </TableCell>
              </TableRow>
            )}
          </TableBody>
        </Table>

        {/* Pagination */}
        {enablePagination && (
          <DataTablePagination table={table} pageSizeOptions={pageSizeOptions} />
        )}
      </div>
    </DataTableContext.Provider>
  )
}

export {
  DataTable,
  DataTableColumnHeader,
  DataTableToolbar,
  DataTablePagination,
  useDataTable,
}
```

---

### Date Range Picker <a name="date-range-picker"></a>

Date range picker with presets, dual calendars, and customizable options

- **Registry Name**: `@boldkit/date-range-picker`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/date-range-picker.json
```

#### Dependencies

- **NPM Packages**:
  - `date-fns`
  - `react-day-picker`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`
  - `@boldkit/calendar`
  - `@boldkit/popover`

#### How to Use

##### File: `registry/default/example/date-range-picker-demo.tsx`

```tsx
import { DateRangePicker } from '@/components/ui/date-range-picker'

export default function Example() {
  return <DateRangePicker />
}
```

#### Component Source Code

##### File: `registry/default/ui/date-range-picker.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { format, subDays, startOfMonth, endOfMonth, subMonths, isSameDay } from 'date-fns'
import { CalendarIcon } from 'lucide-react'
import type { DateRange } from 'react-day-picker'
import { cn } from '@/lib/utils'
import { Button } from '@/components/ui/button'
import { Calendar } from '@/components/ui/calendar'
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from '@/components/ui/popover'

export interface DateRangePickerPreset {
  label: string
  value: DateRange
}

export interface DateRangePickerProps {
  value?: DateRange
  defaultValue?: DateRange
  onChange?: (range: DateRange | undefined) => void
  numberOfMonths?: 1 | 2
  presets?: DateRangePickerPreset[]
  showPresets?: boolean
  minDate?: Date
  maxDate?: Date
  disabled?: boolean
  placeholder?: string
  align?: 'start' | 'center' | 'end'
  className?: string
}

const getDefaultPresets = (): DateRangePickerPreset[] => {
  const today = new Date()
  const lastMonth = subMonths(today, 1)

  return [
    {
      label: 'Today',
      value: { from: today, to: today },
    },
    {
      label: 'Last 7 days',
      value: { from: subDays(today, 6), to: today },
    },
    {
      label: 'Last 30 days',
      value: { from: subDays(today, 29), to: today },
    },
    {
      label: 'This month',
      value: { from: startOfMonth(today), to: today },
    },
    {
      label: 'Last month',
      value: { from: startOfMonth(lastMonth), to: endOfMonth(lastMonth) },
    },
  ]
}

const DateRangePicker = React.forwardRef<HTMLButtonElement, DateRangePickerProps>(
  (
    {
      value: controlledValue,
      defaultValue,
      onChange,
      numberOfMonths = 2,
      presets,
      showPresets = true,
      minDate,
      maxDate,
      disabled = false,
      placeholder = 'Pick a date range',
      align = 'start',
      className,
    },
    ref
  ) => {
    const [open, setOpen] = React.useState(false)
    const [uncontrolledValue, setUncontrolledValue] = React.useState<DateRange | undefined>(defaultValue)

    const isControlled = controlledValue !== undefined
    const selectedRange = isControlled ? controlledValue : uncontrolledValue

    const resolvedPresets = presets ?? getDefaultPresets()

    const handleSelect = (range: DateRange | undefined) => {
      if (!isControlled) {
        setUncontrolledValue(range)
      }
      onChange?.(range)
    }

    const handlePresetClick = (preset: DateRangePickerPreset) => {
      handleSelect(preset.value)
    }

    const isPresetSelected = (preset: DateRangePickerPreset) => {
      if (!selectedRange?.from || !selectedRange?.to) return false
      if (!preset.value.from || !preset.value.to) return false

      return (
        isSameDay(selectedRange.from, preset.value.from) &&
        isSameDay(selectedRange.to, preset.value.to)
      )
    }

    const formatDateRange = (range: DateRange) => {
      if (!range.from) return placeholder
      if (!range.to) return format(range.from, 'LLL dd, y')
      return `${format(range.from, 'LLL dd, y')} - ${format(range.to, 'LLL dd, y')}`
    }

    // Use single month on mobile
    const [isMobile, setIsMobile] = React.useState(false)

    React.useEffect(() => {
      const checkMobile = () => {
        setIsMobile(window.innerWidth < 640)
      }
      checkMobile()
      window.addEventListener('resize', checkMobile)
      return () => window.removeEventListener('resize', checkMobile)
    }, [])

    const effectiveNumberOfMonths = isMobile ? 1 : numberOfMonths

    return (
      <Popover open={open} onOpenChange={setOpen}>
        <PopoverTrigger asChild>
          <Button
            ref={ref}
            variant="outline"
            disabled={disabled}
            className={cn(
              'w-full justify-start text-left font-normal',
              !selectedRange && 'text-muted-foreground',
              className
            )}
          >
            <CalendarIcon className="mr-2 h-4 w-4 shrink-0" />
            <span className="truncate">
              {selectedRange ? formatDateRange(selectedRange) : placeholder}
            </span>
          </Button>
        </PopoverTrigger>
        <PopoverContent
          className={cn(
            'w-auto p-0 overflow-hidden',
            'shadow-[8px_8px_0px_hsl(var(--shadow-color))]',
            'animate-in fade-in-0 zoom-in-95 duration-200',
            'max-w-[calc(100vw-2rem)]',
            'max-h-[calc(100vh-4rem)] overflow-auto'
          )}
          align={align}
          sideOffset={4}
        >
          <div className={cn(
            'flex',
            // Stack vertically on mobile
            isMobile && showPresets ? 'flex-col' : 'flex-row'
          )}>
            {/* Presets sidebar/header */}
            {showPresets && resolvedPresets.length > 0 && (
              <div className={cn(
                'p-3 bg-muted/30',
                isMobile
                  ? 'border-b-3 border-foreground'
                  : 'min-w-[130px] border-r-3 border-foreground'
              )}>
                <p className="mb-2 text-xs font-bold uppercase tracking-wide text-muted-foreground">
                  Presets
                </p>
                <div className={cn(
                  isMobile
                    ? 'flex flex-wrap gap-1'
                    : 'space-y-1'
                )}>
                  {resolvedPresets.map((preset) => (
                    <button
                      key={preset.label}
                      type="button"
                      onClick={() => handlePresetClick(preset)}
                      className={cn(
                        'text-left text-sm transition-all duration-150',
                        'hover:bg-muted',
                        isMobile
                          ? 'px-2 py-1 border-2 border-foreground text-xs'
                          : 'w-full px-3 py-2',
                        isPresetSelected(preset) && 'bg-accent font-medium shadow-[2px_2px_0px_hsl(var(--shadow-color))]'
                      )}
                    >
                      {preset.label}
                    </button>
                  ))}
                </div>
              </div>
            )}

            {/* Calendar(s) */}
            <div className="p-3">
              <Calendar
                mode="range"
                defaultMonth={selectedRange?.from}
                selected={selectedRange}
                onSelect={handleSelect}
                numberOfMonths={effectiveNumberOfMonths}
                disabled={(date) => {
                  if (minDate && date < minDate) return true
                  if (maxDate && date > maxDate) return true
                  return false
                }}
                className="border-0 shadow-none"
              />
            </div>
          </div>
        </PopoverContent>
      </Popover>
    )
  }
)
DateRangePicker.displayName = 'DateRangePicker'

export { DateRangePicker, getDefaultPresets }
export type { DateRange }
```

---

### Dialog <a name="dialog"></a>

A window overlaid on the primary window with neubrutalism borders and shadows

- **Registry Name**: `@boldkit/dialog`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/dialog.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-dialog`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`

#### How to Use

##### File: `registry/default/example/dialog-demo.tsx`

```tsx
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from '@/components/ui/dialog'
import { Button } from '@/components/ui/button'

export default function Example() {
  return (
    <Dialog>
      <DialogTrigger asChild>
        <Button>Open Dialog</Button>
      </DialogTrigger>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>Dialog Title</DialogTitle>
          <DialogDescription>
            This is a dialog description.
          </DialogDescription>
        </DialogHeader>
      </DialogContent>
    </Dialog>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/dialog.tsx`

```tsx
import * as React from 'react'
import * as DialogPrimitive from '@radix-ui/react-dialog'
import { X } from 'lucide-react'
import { cn } from '@/lib/utils'

const Dialog = DialogPrimitive.Root

const DialogTrigger = DialogPrimitive.Trigger

const DialogPortal = DialogPrimitive.Portal

const DialogClose = DialogPrimitive.Close

const DialogOverlay = React.forwardRef<
  React.ElementRef<typeof DialogPrimitive.Overlay>,
  React.ComponentPropsWithoutRef<typeof DialogPrimitive.Overlay>
>(({ className, ...props }, ref) => (
  <DialogPrimitive.Overlay
    ref={ref}
    className={cn(
      'fixed inset-0 z-50 bg-black/70 data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0',
      className
    )}
    {...props}
  />
))
DialogOverlay.displayName = DialogPrimitive.Overlay.displayName

const DialogContent = React.forwardRef<
  React.ElementRef<typeof DialogPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof DialogPrimitive.Content>
>(({ className, children, ...props }, ref) => (
  <DialogPortal>
    <DialogOverlay />
    <DialogPrimitive.Content
      ref={ref}
      className={cn(
        'fixed left-[50%] top-[50%] z-50 grid w-full max-w-lg translate-x-[-50%] translate-y-[-50%] gap-4 border-3 border-foreground bg-background p-6 shadow-[8px_8px_0px_hsl(var(--shadow-color))] duration-200 data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[state=closed]:slide-out-to-left-1/2 data-[state=closed]:slide-out-to-top-[48%] data-[state=open]:slide-in-from-left-1/2 data-[state=open]:slide-in-from-top-[48%] sm:max-w-[425px]',
        className
      )}
      {...props}
    >
      {children}
      <DialogPrimitive.Close className="absolute right-4 top-4 border-2 border-foreground bg-background p-1 shadow-[4px_4px_0px_hsl(var(--shadow-color))] transition-all duration-200 hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none active:translate-x-[4px] active:translate-y-[4px] active:shadow-none focus:outline-none focus:ring-2 focus:ring-ring disabled:pointer-events-none">
        <X className="h-4 w-4 stroke-[3]" />
        <span className="sr-only">Close</span>
      </DialogPrimitive.Close>
    </DialogPrimitive.Content>
  </DialogPortal>
))
DialogContent.displayName = DialogPrimitive.Content.displayName

const DialogHeader = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn(
      'flex flex-col space-y-1.5 text-center sm:text-left',
      className
    )}
    {...props}
  />
)
DialogHeader.displayName = 'DialogHeader'

const DialogFooter = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn(
      'flex flex-col-reverse sm:flex-row sm:justify-end sm:space-x-2',
      className
    )}
    {...props}
  />
)
DialogFooter.displayName = 'DialogFooter'

const DialogTitle = React.forwardRef<
  React.ElementRef<typeof DialogPrimitive.Title>,
  React.ComponentPropsWithoutRef<typeof DialogPrimitive.Title>
>(({ className, ...props }, ref) => (
  <DialogPrimitive.Title
    ref={ref}
    className={cn(
      'text-lg font-bold uppercase tracking-wide leading-none',
      className
    )}
    {...props}
  />
))
DialogTitle.displayName = DialogPrimitive.Title.displayName

const DialogDescription = React.forwardRef<
  React.ElementRef<typeof DialogPrimitive.Description>,
  React.ComponentPropsWithoutRef<typeof DialogPrimitive.Description>
>(({ className, ...props }, ref) => (
  <DialogPrimitive.Description
    ref={ref}
    className={cn('text-sm text-muted-foreground', className)}
    {...props}
  />
))
DialogDescription.displayName = DialogPrimitive.Description.displayName

export {
  Dialog,
  DialogPortal,
  DialogOverlay,
  DialogClose,
  DialogTrigger,
  DialogContent,
  DialogHeader,
  DialogFooter,
  DialogTitle,
  DialogDescription,
}
```

---

### Donut Chart <a name="donut-chart"></a>

Pie chart with center hole for KPI display, supports center content slot

- **Registry Name**: `@boldkit/donut-chart`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/donut-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/chart`

#### How to Use

##### File: `registry/default/example/donut-chart-demo.tsx`

```tsx
import { DonutChart, DonutChartCenter } from '@/components/ui/donut-chart'

const data = [
  { name: 'Chrome', value: 275, fill: 'hsl(var(--primary))' },
  { name: 'Safari', value: 200, fill: 'hsl(var(--secondary))' },
  { name: 'Firefox', value: 187, fill: 'hsl(var(--accent))' },
]

const config = {
  chrome: { label: 'Chrome', color: 'hsl(var(--primary))' },
  safari: { label: 'Safari', color: 'hsl(var(--secondary))' },
  firefox: { label: 'Firefox', color: 'hsl(var(--accent))' },
}

export default function Example() {
  return (
    <DonutChart
      data={data}
      config={config}
      centerContent={<DonutChartCenter value="662" label="Total" />}
    />
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/donut-chart.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'
import { Pie, PieChart, Cell, Label } from 'recharts'
import { ChartContainer } from './chart'
import { ChartEmpty } from './chart'
import { ChartTooltip, ChartTooltipContent } from './chart'
import type { ChartConfig } from './chart'

export interface DonutChartData {
  name: string
  value: number
  fill?: string
}

export interface DonutChartProps extends React.HTMLAttributes<HTMLDivElement> {
  data: DonutChartData[]
  config: ChartConfig
  innerRadius?: string | number
  outerRadius?: string | number
  centerContent?: React.ReactNode
  showLabels?: 'none' | 'inside' | 'outside'
  variant?: 'default' | 'separated'
  showTooltip?: boolean
  animated?: boolean
  emptyState?: React.ReactNode
}

const DonutChart = React.forwardRef<HTMLDivElement, DonutChartProps>(
  (
    {
      data,
      config,
      innerRadius = '60%',
      outerRadius = '80%',
      centerContent,
      showLabels = 'none',
      variant = 'default',
      showTooltip = true,
      animated = true,
      emptyState,
      className,
      ...props
    },
    ref
  ) => {
    if (!data || data.length === 0) {
      return <ChartEmpty ref={ref} message={emptyState} className={className} {...props} />
    }

    const paddingAngle = variant === 'separated' ? 4 : 0

    // Calculate total for center content (available for custom center content)
    const _total = React.useMemo(
      () => data.reduce((sum, item) => sum + item.value, 0),
      [data]
    )
    void _total // Used by consumer via centerContent prop

    return (
      <ChartContainer
        ref={ref}
        config={config}
        className={cn('aspect-square max-h-[300px]', className)}
        {...props}
      >
        <PieChart>
          {showTooltip && (
            <ChartTooltip
              cursor={false}
              content={<ChartTooltipContent hideLabel />}
            />
          )}
          <Pie
            data={data}
            dataKey="value"
            nameKey="name"
            cx="50%"
            cy="50%"
            innerRadius={innerRadius}
            outerRadius={outerRadius}
            paddingAngle={paddingAngle}
            strokeWidth={3}
            stroke="hsl(var(--foreground))"
            isAnimationActive={animated}
            animationDuration={400}
            label={
              showLabels === 'outside'
                ? ({ name, percent }: { name?: string; percent?: number }) => `${name ?? ''}: ${((percent ?? 0) * 100).toFixed(0)}%`
                : showLabels === 'inside'
                ? ({ percent }: { percent?: number }) => `${((percent ?? 0) * 100).toFixed(0)}%`
                : false
            }
            labelLine={showLabels === 'outside'}
          >
            {data.map((entry, index) => (
              <Cell
                key={`cell-${index}`}
                fill={entry.fill || `hsl(var(--chart-${(index % 5) + 1}))`}
              />
            ))}
            {centerContent && (
              <Label
                content={({ viewBox }) => {
                  if (viewBox && 'cx' in viewBox && 'cy' in viewBox) {
                    const pieView = viewBox as { cx: number; cy: number; innerRadius?: number; outerRadius?: number }
                    const ir = pieView.innerRadius ?? (typeof innerRadius === 'number' ? innerRadius : 60)
                    const fwHalf = Math.max(40, ir * 0.85)
                    const fhHalf = Math.max(25, ir * 0.55)
                    return (
                      <foreignObject
                        x={(pieView.cx || 0) - fwHalf}
                        y={(pieView.cy || 0) - fhHalf}
                        width={fwHalf * 2}
                        height={fhHalf * 2}
                      >
                        <div className="flex h-full w-full items-center justify-center">
                          {centerContent}
                        </div>
                      </foreignObject>
                    )
                  }
                  return null
                }}
              />
            )}
          </Pie>
        </PieChart>
      </ChartContainer>
    )
  }
)
DonutChart.displayName = 'DonutChart'

// Helper component for common center content pattern
export interface DonutChartCenterProps {
  value: string | number
  label?: string
  className?: string
}

const DonutChartCenter = React.forwardRef<HTMLDivElement, DonutChartCenterProps>(
  ({ value, label, className }, ref) => {
    return (
      <div ref={ref} className={cn('text-center', className)}>
        <div className="text-2xl font-black">{value}</div>
        {label && (
          <div className="text-xs font-bold uppercase tracking-wide text-muted-foreground">
            {label}
          </div>
        )}
      </div>
    )
  }
)
DonutChartCenter.displayName = 'DonutChartCenter'

export { DonutChart, DonutChartCenter }
```

---

### Drawer <a name="drawer"></a>

A drawer component that slides in from the edge of the screen

- **Registry Name**: `@boldkit/drawer`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/drawer.json
```

#### Dependencies

- **NPM Packages**:
  - `vaul`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/drawer-demo.tsx`

```tsx
import {
  Drawer,
  DrawerContent,
  DrawerDescription,
  DrawerHeader,
  DrawerTitle,
  DrawerTrigger,
} from '@/components/ui/drawer'

export default function Example() {
  return (
    <Drawer>
      <DrawerTrigger asChild>
        <Button>Open Drawer</Button>
      </DrawerTrigger>
      <DrawerContent>
        <DrawerHeader>
          <DrawerTitle>Drawer Title</DrawerTitle>
          <DrawerDescription>Drawer description.</DrawerDescription>
        </DrawerHeader>
      </DrawerContent>
    </Drawer>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/drawer.tsx`

```tsx
import * as React from 'react'
import { Drawer as DrawerPrimitive } from 'vaul'
import { cn } from '@/lib/utils'

const Drawer = ({
  shouldScaleBackground = true,
  ...props
}: React.ComponentProps<typeof DrawerPrimitive.Root>) => (
  <DrawerPrimitive.Root
    shouldScaleBackground={shouldScaleBackground}
    {...props}
  />
)
Drawer.displayName = 'Drawer'

const DrawerTrigger = DrawerPrimitive.Trigger

const DrawerPortal = DrawerPrimitive.Portal

const DrawerClose = DrawerPrimitive.Close

const DrawerOverlay = React.forwardRef<
  React.ElementRef<typeof DrawerPrimitive.Overlay>,
  React.ComponentPropsWithoutRef<typeof DrawerPrimitive.Overlay>
>(({ className, ...props }, ref) => (
  <DrawerPrimitive.Overlay
    ref={ref}
    className={cn('fixed inset-0 z-50 bg-black/70', className)}
    {...props}
  />
))
DrawerOverlay.displayName = DrawerPrimitive.Overlay.displayName

const DrawerContent = React.forwardRef<
  React.ElementRef<typeof DrawerPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof DrawerPrimitive.Content>
>(({ className, children, ...props }, ref) => (
  <DrawerPortal>
    <DrawerOverlay />
    <DrawerPrimitive.Content
      ref={ref}
      className={cn(
        'fixed inset-x-0 bottom-0 z-50 mt-24 flex h-auto flex-col border-3 border-b-0 border-foreground bg-background shadow-[0px_-8px_0px_hsl(var(--shadow-color))]',
        className
      )}
      {...props}
    >
      <div className="mx-auto mt-4 h-2 w-[100px] bg-foreground" />
      {children}
    </DrawerPrimitive.Content>
  </DrawerPortal>
))
DrawerContent.displayName = 'DrawerContent'

const DrawerHeader = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn('grid gap-1.5 p-4 text-center sm:text-left', className)}
    {...props}
  />
)
DrawerHeader.displayName = 'DrawerHeader'

const DrawerFooter = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn('mt-auto flex flex-col gap-2 p-4', className)}
    {...props}
  />
)
DrawerFooter.displayName = 'DrawerFooter'

const DrawerTitle = React.forwardRef<
  React.ElementRef<typeof DrawerPrimitive.Title>,
  React.ComponentPropsWithoutRef<typeof DrawerPrimitive.Title>
>(({ className, ...props }, ref) => (
  <DrawerPrimitive.Title
    ref={ref}
    className={cn('text-lg font-bold uppercase tracking-wide leading-none', className)}
    {...props}
  />
))
DrawerTitle.displayName = DrawerPrimitive.Title.displayName

const DrawerDescription = React.forwardRef<
  React.ElementRef<typeof DrawerPrimitive.Description>,
  React.ComponentPropsWithoutRef<typeof DrawerPrimitive.Description>
>(({ className, ...props }, ref) => (
  <DrawerPrimitive.Description
    ref={ref}
    className={cn('text-sm text-muted-foreground', className)}
    {...props}
  />
))
DrawerDescription.displayName = DrawerPrimitive.Description.displayName

export {
  Drawer,
  DrawerPortal,
  DrawerOverlay,
  DrawerTrigger,
  DrawerClose,
  DrawerContent,
  DrawerHeader,
  DrawerFooter,
  DrawerTitle,
  DrawerDescription,
}
```

---

### Dropdown Menu <a name="dropdown-menu"></a>

Displays a menu of actions/options with neubrutalism styling

- **Registry Name**: `@boldkit/dropdown-menu`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/dropdown-menu.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-dropdown-menu`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`

#### How to Use

##### File: `registry/default/example/dropdown-menu-demo.tsx`

```tsx
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu'
import { Button } from '@/components/ui/button'

export default function Example() {
  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button>Open Menu</Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent>
        <DropdownMenuLabel>My Account</DropdownMenuLabel>
        <DropdownMenuSeparator />
        <DropdownMenuItem>Profile</DropdownMenuItem>
        <DropdownMenuItem>Settings</DropdownMenuItem>
        <DropdownMenuItem>Logout</DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/dropdown-menu.tsx`

```tsx
import * as React from 'react'
import * as DropdownMenuPrimitive from '@radix-ui/react-dropdown-menu'
import { Check, ChevronRight, Circle } from 'lucide-react'
import { cn } from '@/lib/utils'

const DropdownMenu = DropdownMenuPrimitive.Root
const DropdownMenuTrigger = DropdownMenuPrimitive.Trigger
const DropdownMenuGroup = DropdownMenuPrimitive.Group
const DropdownMenuPortal = DropdownMenuPrimitive.Portal
const DropdownMenuSub = DropdownMenuPrimitive.Sub
const DropdownMenuRadioGroup = DropdownMenuPrimitive.RadioGroup

const DropdownMenuSubTrigger = React.forwardRef<
  React.ElementRef<typeof DropdownMenuPrimitive.SubTrigger>,
  React.ComponentPropsWithoutRef<typeof DropdownMenuPrimitive.SubTrigger> & {
    inset?: boolean
  }
>(({ className, inset, children, ...props }, ref) => (
  <DropdownMenuPrimitive.SubTrigger
    ref={ref}
    className={cn(
      'flex cursor-default select-none items-center px-2 py-1.5 text-sm outline-none focus:bg-accent data-[state=open]:bg-accent',
      inset && 'pl-8',
      className
    )}
    {...props}
  >
    {children}
    <ChevronRight className="ml-auto h-4 w-4 stroke-[3]" />
  </DropdownMenuPrimitive.SubTrigger>
))
DropdownMenuSubTrigger.displayName = DropdownMenuPrimitive.SubTrigger.displayName

const DropdownMenuSubContent = React.forwardRef<
  React.ElementRef<typeof DropdownMenuPrimitive.SubContent>,
  React.ComponentPropsWithoutRef<typeof DropdownMenuPrimitive.SubContent>
>(({ className, ...props }, ref) => (
  <DropdownMenuPrimitive.SubContent
    ref={ref}
    className={cn(
      'z-50 min-w-[8rem] overflow-hidden border-3 border-foreground bg-popover p-1 text-popover-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
      className
    )}
    {...props}
  />
))
DropdownMenuSubContent.displayName = DropdownMenuPrimitive.SubContent.displayName

const DropdownMenuContent = React.forwardRef<
  React.ElementRef<typeof DropdownMenuPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof DropdownMenuPrimitive.Content>
>(({ className, sideOffset = 4, ...props }, ref) => (
  <DropdownMenuPrimitive.Portal>
    <DropdownMenuPrimitive.Content
      ref={ref}
      sideOffset={sideOffset}
      className={cn(
        'z-50 min-w-[8rem] overflow-hidden border-3 border-foreground bg-popover p-1 text-popover-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        className
      )}
      {...props}
    />
  </DropdownMenuPrimitive.Portal>
))
DropdownMenuContent.displayName = DropdownMenuPrimitive.Content.displayName

const DropdownMenuItem = React.forwardRef<
  React.ElementRef<typeof DropdownMenuPrimitive.Item>,
  React.ComponentPropsWithoutRef<typeof DropdownMenuPrimitive.Item> & {
    inset?: boolean
  }
>(({ className, inset, ...props }, ref) => (
  <DropdownMenuPrimitive.Item
    ref={ref}
    className={cn(
      'relative flex cursor-default select-none items-center px-2 py-1.5 text-sm outline-none transition-all duration-150 focus:bg-accent focus:text-accent-foreground focus:translate-x-1 data-[disabled]:pointer-events-none data-[disabled]:opacity-50',
      inset && 'pl-8',
      className
    )}
    {...props}
  />
))
DropdownMenuItem.displayName = DropdownMenuPrimitive.Item.displayName

const DropdownMenuCheckboxItem = React.forwardRef<
  React.ElementRef<typeof DropdownMenuPrimitive.CheckboxItem>,
  React.ComponentPropsWithoutRef<typeof DropdownMenuPrimitive.CheckboxItem>
>(({ className, children, checked, ...props }, ref) => (
  <DropdownMenuPrimitive.CheckboxItem
    ref={ref}
    className={cn(
      'relative flex cursor-default select-none items-center py-1.5 pl-8 pr-2 text-sm outline-none transition-colors focus:bg-accent focus:text-accent-foreground data-[disabled]:pointer-events-none data-[disabled]:opacity-50',
      className
    )}
    checked={checked}
    {...props}
  >
    <span className="absolute left-2 flex h-3.5 w-3.5 items-center justify-center">
      <DropdownMenuPrimitive.ItemIndicator>
        <Check className="h-4 w-4 stroke-[3]" />
      </DropdownMenuPrimitive.ItemIndicator>
    </span>
    {children}
  </DropdownMenuPrimitive.CheckboxItem>
))
DropdownMenuCheckboxItem.displayName = DropdownMenuPrimitive.CheckboxItem.displayName

const DropdownMenuRadioItem = React.forwardRef<
  React.ElementRef<typeof DropdownMenuPrimitive.RadioItem>,
  React.ComponentPropsWithoutRef<typeof DropdownMenuPrimitive.RadioItem>
>(({ className, children, ...props }, ref) => (
  <DropdownMenuPrimitive.RadioItem
    ref={ref}
    className={cn(
      'relative flex cursor-default select-none items-center py-1.5 pl-8 pr-2 text-sm outline-none transition-colors focus:bg-accent focus:text-accent-foreground data-[disabled]:pointer-events-none data-[disabled]:opacity-50',
      className
    )}
    {...props}
  >
    <span className="absolute left-2 flex h-3.5 w-3.5 items-center justify-center">
      <DropdownMenuPrimitive.ItemIndicator>
        <Circle className="h-2 w-2 fill-current" />
      </DropdownMenuPrimitive.ItemIndicator>
    </span>
    {children}
  </DropdownMenuPrimitive.RadioItem>
))
DropdownMenuRadioItem.displayName = DropdownMenuPrimitive.RadioItem.displayName

const DropdownMenuLabel = React.forwardRef<
  React.ElementRef<typeof DropdownMenuPrimitive.Label>,
  React.ComponentPropsWithoutRef<typeof DropdownMenuPrimitive.Label> & {
    inset?: boolean
  }
>(({ className, inset, ...props }, ref) => (
  <DropdownMenuPrimitive.Label
    ref={ref}
    className={cn(
      'px-2 py-1.5 text-sm font-bold uppercase tracking-wide',
      inset && 'pl-8',
      className
    )}
    {...props}
  />
))
DropdownMenuLabel.displayName = DropdownMenuPrimitive.Label.displayName

const DropdownMenuSeparator = React.forwardRef<
  React.ElementRef<typeof DropdownMenuPrimitive.Separator>,
  React.ComponentPropsWithoutRef<typeof DropdownMenuPrimitive.Separator>
>(({ className, ...props }, ref) => (
  <DropdownMenuPrimitive.Separator
    ref={ref}
    className={cn('-mx-1 my-1 h-px bg-foreground', className)}
    {...props}
  />
))
DropdownMenuSeparator.displayName = DropdownMenuPrimitive.Separator.displayName

const DropdownMenuShortcut = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLSpanElement>) => {
  return (
    <span
      className={cn('ml-auto text-xs tracking-widest opacity-60', className)}
      {...props}
    />
  )
}
DropdownMenuShortcut.displayName = 'DropdownMenuShortcut'

export {
  DropdownMenu,
  DropdownMenuTrigger,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuCheckboxItem,
  DropdownMenuRadioItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuShortcut,
  DropdownMenuGroup,
  DropdownMenuPortal,
  DropdownMenuSub,
  DropdownMenuSubContent,
  DropdownMenuSubTrigger,
  DropdownMenuRadioGroup,
}
```

---

### Dropzone <a name="dropzone"></a>

Drag-and-drop file upload with validation, progress tracking, and file list

- **Registry Name**: `@boldkit/dropzone`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/dropzone.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/progress`
  - `@boldkit/spinner`

#### How to Use

##### File: `registry/default/example/dropzone-demo.tsx`

```tsx
import { Dropzone, FileList } from '@/components/ui/dropzone'

export default function Example() {
  const [files, setFiles] = useState([])

  return (
    <>
      <Dropzone
        onFilesAccepted={(accepted) => setFiles(prev => [...prev, ...accepted])}
        accept={{ 'image/*': ['.png', '.jpg', '.gif'] }}
        maxSize={5 * 1024 * 1024}
      />
      <FileList
        files={files.map(f => ({ file: f }))}
        onRemove={(file) => setFiles(prev => prev.filter(f => f !== file))}
      />
    </>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/dropzone.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'
import { Progress } from '@/components/ui/progress'
import { Spinner } from '@/components/ui/spinner'
import { Upload, X, File, Image, FileText, FileCode, FileAudio, FileVideo } from 'lucide-react'

// Types
export interface FileRejection {
  file: File
  errors: Array<{ code: string; message: string }>
}

export interface DropzoneState {
  isDragging: boolean
  isDisabled: boolean
  acceptedFiles: File[]
  rejectedFiles: FileRejection[]
  reset: () => void
}

// Variants
const dropzoneVariants = cva(
  'relative flex flex-col items-center justify-center border-3 border-dashed border-foreground transition-all duration-200 cursor-pointer',
  {
    variants: {
      state: {
        idle: 'bg-background hover:bg-muted/30 shadow-[4px_4px_0px_hsl(var(--shadow-color))] hover:shadow-[6px_6px_0px_hsl(var(--shadow-color))] hover:translate-x-[-2px] hover:translate-y-[-2px]',
        dragging: 'border-solid border-primary bg-primary/10 scale-[1.02] shadow-[8px_8px_0px_hsl(var(--primary))]',
        disabled: 'opacity-50 cursor-not-allowed shadow-none',
      },
      variant: {
        default: 'p-8',
        compact: 'p-6',
        minimal: 'p-3 border-2',
      },
    },
    defaultVariants: {
      state: 'idle',
      variant: 'default',
    },
  }
)

// Dropzone Props
export interface DropzoneProps
  extends Omit<React.HTMLAttributes<HTMLDivElement>, 'children'>,
    VariantProps<typeof dropzoneVariants> {
  onFilesAccepted: (files: File[]) => void
  onFilesRejected?: (files: FileRejection[]) => void
  accept?: Record<string, string[]>
  maxSize?: number
  maxFiles?: number
  disabled?: boolean
  children?: React.ReactNode | ((state: DropzoneState) => React.ReactNode)
}

const Dropzone = React.forwardRef<HTMLDivElement, DropzoneProps>(
  (
    {
      onFilesAccepted,
      onFilesRejected,
      accept,
      maxSize = 10 * 1024 * 1024, // 10MB default
      maxFiles = 10,
      disabled = false,
      variant,
      className,
      children,
      ...props
    },
    ref
  ) => {
    const [isDragging, setIsDragging] = React.useState(false)
    const [isFocused, setIsFocused] = React.useState(false)
    const [acceptedFiles, setAcceptedFiles] = React.useState<File[]>([])
    const [rejectedFiles, setRejectedFiles] = React.useState<FileRejection[]>([])
    const inputRef = React.useRef<HTMLInputElement>(null)

    const reset = React.useCallback(() => {
      setAcceptedFiles([])
      setRejectedFiles([])
      if (inputRef.current) inputRef.current.value = ''
    }, [])

    const state: DropzoneState = {
      isDragging,
      isDisabled: disabled,
      acceptedFiles,
      rejectedFiles,
      reset,
    }

    const stateVariant = disabled ? 'disabled' : isDragging ? 'dragging' : 'idle'

    // Validate file
    const validateFile = (file: File): FileRejection | null => {
      const errors: Array<{ code: string; message: string }> = []

      // Check file size
      if (file.size > maxSize) {
        errors.push({
          code: 'file-too-large',
          message: `File is larger than ${formatBytes(maxSize)}`,
        })
      }

      // Check file type
      if (accept) {
        const acceptedTypes = Object.entries(accept).flatMap(([mimeType, extensions]) => {
          return [mimeType, ...extensions]
        })

        const fileType = file.type
        const fileExtension = `.${file.name.split('.').pop()?.toLowerCase()}`

        const isAccepted = acceptedTypes.some((type) => {
          if (type.startsWith('.')) {
            return fileExtension === type.toLowerCase()
          }
          if (type.endsWith('/*')) {
            return fileType.startsWith(type.replace('/*', '/'))
          }
          return fileType === type
        })

        if (!isAccepted) {
          errors.push({
            code: 'file-invalid-type',
            message: 'File type not accepted',
          })
        }
      }

      return errors.length > 0 ? { file, errors } : null
    }

    // Process files
    const processFiles = (fileList: FileList | null) => {
      if (!fileList || disabled) return

      const allFiles = Array.from(fileList)
      const accepted: File[] = []
      const rejected: FileRejection[] = []

      allFiles.forEach((file) => {
        const rejection = validateFile(file)
        if (rejection) {
          rejected.push(rejection)
        } else if (accepted.length >= maxFiles) {
          rejected.push({
            file,
            errors: [{ code: 'too-many-files', message: `Too many files. Maximum is ${maxFiles}.` }],
          })
        } else {
          accepted.push(file)
        }
      })

      setAcceptedFiles(accepted)
      setRejectedFiles(rejected)

      if (accepted.length > 0) {
        onFilesAccepted(accepted)
      }
      if (rejected.length > 0) {
        onFilesRejected?.(rejected)
      }
    }

    // Event handlers
    const handleDragEnter = (e: React.DragEvent) => {
      e.preventDefault()
      e.stopPropagation()
      if (!disabled) {
        setIsDragging(true)
      }
    }

    const handleDragLeave = (e: React.DragEvent) => {
      e.preventDefault()
      e.stopPropagation()
      setIsDragging(false)
    }

    const handleDragOver = (e: React.DragEvent) => {
      e.preventDefault()
      e.stopPropagation()
    }

    const handleDrop = (e: React.DragEvent) => {
      e.preventDefault()
      e.stopPropagation()
      setIsDragging(false)
      processFiles(e.dataTransfer.files)
    }

    const handleClick = () => {
      if (!disabled) {
        inputRef.current?.click()
      }
    }

    const handleKeyDown = (e: React.KeyboardEvent<HTMLDivElement>) => {
      if (!disabled && (e.key === 'Enter' || e.key === ' ')) {
        e.preventDefault()
        inputRef.current?.click()
      }
    }

    const handleFocus = () => setIsFocused(true)
    const handleBlur = () => setIsFocused(false)

    const handleInputChange = (e: React.ChangeEvent<HTMLInputElement>) => {
      processFiles(e.target.files)
      e.target.value = ''
    }

    // Build accept string for input
    const acceptString = accept
      ? Object.entries(accept)
          .flatMap(([mimeType, extensions]) => [mimeType, ...extensions])
          .join(',')
      : undefined

    return (
      <div
        ref={ref}
        className={cn(
          dropzoneVariants({ state: stateVariant, variant }),
          isFocused && !disabled && 'outline outline-2 outline-offset-2 outline-primary',
          className
        )}
        onDragEnter={handleDragEnter}
        onDragLeave={handleDragLeave}
        onDragOver={handleDragOver}
        onDrop={handleDrop}
        onClick={handleClick}
        onKeyDown={handleKeyDown}
        onFocus={handleFocus}
        onBlur={handleBlur}
        role="button"
        tabIndex={disabled ? -1 : 0}
        aria-disabled={disabled}
        aria-label="File upload area"
        {...props}
      >
        <input
          ref={inputRef}
          type="file"
          accept={acceptString}
          multiple={maxFiles > 1}
          disabled={disabled}
          onChange={handleInputChange}
          className="hidden"
        />

        {typeof children === 'function' ? (
          children(state)
        ) : children ? (
          children
        ) : (
          <DefaultDropzoneContent isDragging={isDragging} variant={variant} />
        )}
      </div>
    )
  }
)
Dropzone.displayName = 'Dropzone'

// Default content
function DefaultDropzoneContent({
  isDragging,
  variant,
}: {
  isDragging: boolean
  variant: DropzoneProps['variant']
}) {
  return (
    <div className="flex flex-col items-center gap-3 text-center">
      <div
        className={cn(
          'flex items-center justify-center w-16 h-16 border-3 border-foreground bg-muted transition-all duration-200',
          isDragging && 'bg-primary border-primary shadow-[4px_4px_0px_hsl(var(--foreground))] -translate-x-1 -translate-y-1'
        )}
      >
        <Upload
          className={cn(
            'h-8 w-8 transition-all duration-200',
            isDragging ? 'text-primary-foreground animate-bounce' : 'text-foreground'
          )}
        />
      </div>
      {variant !== 'minimal' && (
        <>
          <p className="font-black uppercase tracking-wide text-lg">
            {isDragging ? 'Drop files here' : 'Drag & drop files'}
          </p>
          <p className="text-sm text-muted-foreground font-bold">
            or click to browse
          </p>
        </>
      )}
    </div>
  )
}

// File List Component
export interface FileListProps extends React.HTMLAttributes<HTMLDivElement> {
  files: Array<{
    file: File
    progress?: number
    error?: string
    uploading?: boolean
  }>
  onRemove?: (file: File) => void
}

const FileList = React.forwardRef<HTMLDivElement, FileListProps>(
  ({ files, onRemove, className, ...props }, ref) => {
    if (files.length === 0) return null

    return (
      <div
        ref={ref}
        className={cn('space-y-2 mt-4', className)}
        {...props}
      >
        {files.map((item, index) => (
          <FileListItem
            key={`${item.file.name}-${index}`}
            file={item.file}
            progress={item.progress}
            error={item.error}
            uploading={item.uploading}
            onRemove={onRemove ? () => onRemove(item.file) : undefined}
          />
        ))}
      </div>
    )
  }
)
FileList.displayName = 'FileList'

// File List Item
interface FileListItemProps {
  file: File
  progress?: number
  error?: string
  uploading?: boolean
  onRemove?: () => void
}

function FileListItem({ file, progress, error, uploading, onRemove }: FileListItemProps) {
  const Icon = getFileIcon(file.type)

  return (
    <div
      className={cn(
        'flex items-center gap-3 p-3 border-3 border-foreground bg-background shadow-[3px_3px_0px_hsl(var(--shadow-color))]',
        error && 'border-destructive bg-destructive/10 shadow-[3px_3px_0px_hsl(var(--destructive))]'
      )}
    >
      <div className="flex items-center justify-center w-10 h-10 bg-muted border-3 border-foreground">
        <Icon className="h-5 w-5" />
      </div>

      <div className="flex-1 min-w-0">
        <p className="font-bold text-sm truncate">{file.name}</p>
        <p className="text-xs text-muted-foreground">{formatBytes(file.size)}</p>
        {error && <p className="text-xs text-destructive font-bold">{error}</p>}
        {uploading && progress !== undefined && (
          <Progress value={progress} className="h-2 mt-1" />
        )}
      </div>

      {uploading ? (
        <Spinner size="sm" />
      ) : onRemove ? (
        <button
          type="button"
          onClick={(e) => {
            e.stopPropagation()
            onRemove()
          }}
          className="flex items-center justify-center w-8 h-8 border-3 border-foreground bg-background hover:bg-destructive hover:text-destructive-foreground hover:shadow-[2px_2px_0px_hsl(var(--foreground))] hover:-translate-x-0.5 hover:-translate-y-0.5 transition-all"
        >
          <X className="h-4 w-4" />
        </button>
      ) : null}
    </div>
  )
}

// Helpers
function formatBytes(bytes: number): string {
  if (bytes === 0) return '0 Bytes'
  const k = 1024
  const sizes = ['Bytes', 'KB', 'MB', 'GB']
  const i = Math.floor(Math.log(bytes) / Math.log(k))
  return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i]
}

function getFileIcon(mimeType: string) {
  if (mimeType.startsWith('image/')) return Image
  if (mimeType.startsWith('video/')) return FileVideo
  if (mimeType.startsWith('audio/')) return FileAudio
  if (mimeType.includes('pdf') || mimeType.includes('document')) return FileText
  if (mimeType.includes('code') || mimeType.includes('javascript') || mimeType.includes('json'))
    return FileCode
  return File
}

export { Dropzone, FileList, dropzoneVariants }
```

---

### Empty State <a name="empty-state"></a>

Empty state component with icon, title, description, and optional actions

- **Registry Name**: `@boldkit/empty-state`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/empty-state.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`

#### How to Use

##### File: `registry/default/example/empty-state-demo.tsx`

```tsx
import {
  EmptyState,
  EmptyStateIcon,
  EmptyStateTitle,
  EmptyStateDescription,
  EmptyStateActions,
} from '@/components/ui/empty-state'
import { Inbox, Plus } from 'lucide-react'
import { Button } from '@/components/ui/button'

export function MyEmptyState() {
  return (
    <EmptyState>
      <EmptyStateIcon>
        <Inbox className="h-10 w-10" />
      </EmptyStateIcon>
      <EmptyStateTitle>No messages yet</EmptyStateTitle>
      <EmptyStateDescription>
        Your inbox is empty. Send a message to get started.
      </EmptyStateDescription>
      <EmptyStateActions>
        <Button>
          <Plus className="mr-2 h-4 w-4" />
          Compose
        </Button>
      </EmptyStateActions>
    </EmptyState>
  )
}
export default MyEmptyState
```

#### Component Source Code

##### File: `registry/default/ui/empty-state.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'
import {
  FileQuestion,
  Search,
  Inbox,
  FolderOpen,
  Users,
  ShoppingCart,
  Bell,
  Image,
  AlertTriangle,
  WifiOff,
  ShieldX,
  Clock,
  Wrench,
  Upload,
} from 'lucide-react'

// ============================================================================
// Empty State Root
// ============================================================================

const emptyStateVariants = cva(
  'flex items-center justify-center',
  {
    variants: {
      variant: {
        default: '',
        filled: 'bg-muted/30 border-3 border-dashed border-foreground p-8',
        card: 'bg-card border-3 border-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))] p-8',
      },
      size: {
        compact: 'gap-2 p-2',
        sm: 'gap-3 p-4',
        md: 'gap-4 p-6',
        lg: 'gap-6 p-8',
      },
      layout: {
        vertical: 'flex-col text-center',
        horizontal: 'flex-row text-left',
      },
      animation: {
        none: '',
        fadeIn: 'animate-[fadeIn_0.3s_ease-out]',
        bounce: 'animate-[bounceIn_0.5s_ease-out]',
        scale: 'animate-[scaleIn_0.3s_ease-out]',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'md',
      layout: 'vertical',
      animation: 'none',
    },
  }
)

export interface EmptyStateProps
  extends React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof emptyStateVariants> {}

const EmptyState = React.forwardRef<HTMLDivElement, EmptyStateProps>(
  ({ className, variant, size, layout, animation, children, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(emptyStateVariants({ variant, size, layout, animation }), className)}
        {...props}
      >
        {children}
      </div>
    )
  }
)
EmptyState.displayName = 'EmptyState'

// ============================================================================
// Empty State Icon
// ============================================================================

const iconContainerVariants = cva(
  'flex items-center justify-center border-3 border-foreground shadow-[3px_3px_0px_hsl(var(--shadow-color))]',
  {
    variants: {
      size: {
        xs: 'w-10 h-10',
        sm: 'w-12 h-12',
        md: 'w-16 h-16',
        lg: 'w-20 h-20',
        xl: 'w-24 h-24',
      },
      iconColor: {
        default: 'bg-muted text-foreground',
        primary: 'bg-primary text-primary-foreground',
        secondary: 'bg-secondary text-secondary-foreground',
        accent: 'bg-accent text-accent-foreground',
        muted: 'bg-muted text-muted-foreground',
        destructive: 'bg-destructive text-destructive-foreground',
        warning: 'bg-warning text-warning-foreground',
        success: 'bg-success text-success-foreground',
      },
    },
    defaultVariants: {
      size: 'md',
      iconColor: 'default',
    },
  }
)

const iconSizeMap = {
  xs: 'h-5 w-5',
  sm: 'h-6 w-6',
  md: 'h-8 w-8',
  lg: 'h-10 w-10',
  xl: 'h-12 w-12',
}

export interface EmptyStateIconProps
  extends React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof iconContainerVariants> {
  /** Custom icon size independent of container */
  iconSize?: 'xs' | 'sm' | 'md' | 'lg' | 'xl'
}

const EmptyStateIcon = React.forwardRef<HTMLDivElement, EmptyStateIconProps>(
  ({ className, size, iconColor, iconSize, children, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(iconContainerVariants({ size, iconColor }), className)}
        {...props}
      >
        {React.isValidElement(children)
          ? React.cloneElement(children as React.ReactElement<{ className?: string }>, {
              className: cn(
                iconSizeMap[iconSize ?? size ?? 'md'],
                (children as React.ReactElement<{ className?: string }>).props.className
              ),
            })
          : children}
      </div>
    )
  }
)
EmptyStateIcon.displayName = 'EmptyStateIcon'

// ============================================================================
// Empty State Illustration (for custom SVGs/images)
// ============================================================================

export interface EmptyStateIllustrationProps extends React.HTMLAttributes<HTMLDivElement> {
  /** Max width for the illustration */
  maxWidth?: string | number
}

const EmptyStateIllustration = React.forwardRef<HTMLDivElement, EmptyStateIllustrationProps>(
  ({ className, maxWidth = '200px', children, style, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn('flex items-center justify-center', className)}
        style={{ maxWidth, ...style }}
        {...props}
      >
        {children}
      </div>
    )
  }
)
EmptyStateIllustration.displayName = 'EmptyStateIllustration'

// ============================================================================
// Empty State Title
// ============================================================================

export type EmptyStateTitleProps = React.HTMLAttributes<HTMLHeadingElement>

const EmptyStateTitle = React.forwardRef<HTMLHeadingElement, EmptyStateTitleProps>(
  ({ className, children, ...props }, ref) => {
    return (
      <h3
        ref={ref}
        className={cn('font-black text-lg uppercase tracking-wide', className)}
        {...props}
      >
        {children}
      </h3>
    )
  }
)
EmptyStateTitle.displayName = 'EmptyStateTitle'

// ============================================================================
// Empty State Description
// ============================================================================

export type EmptyStateDescriptionProps = React.HTMLAttributes<HTMLParagraphElement>

const EmptyStateDescription = React.forwardRef<HTMLParagraphElement, EmptyStateDescriptionProps>(
  ({ className, children, ...props }, ref) => {
    return (
      <p
        ref={ref}
        className={cn('text-muted-foreground font-medium max-w-sm', className)}
        {...props}
      >
        {children}
      </p>
    )
  }
)
EmptyStateDescription.displayName = 'EmptyStateDescription'

// ============================================================================
// Empty State Actions
// ============================================================================

export type EmptyStateActionsProps = React.HTMLAttributes<HTMLDivElement>

const EmptyStateActions = React.forwardRef<HTMLDivElement, EmptyStateActionsProps>(
  ({ className, children, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn('flex flex-wrap items-center justify-center gap-2 mt-2', className)}
        {...props}
      >
        {children}
      </div>
    )
  }
)
EmptyStateActions.displayName = 'EmptyStateActions'

// ============================================================================
// Presets
// ============================================================================

export type EmptyStatePresetType =
  | 'no-results'
  | 'no-data'
  | 'empty-inbox'
  | 'empty-folder'
  | 'no-users'
  | 'empty-cart'
  | 'no-notifications'
  | 'no-images'
  // New presets
  | 'error'
  | 'offline'
  | 'permission-denied'
  | 'coming-soon'
  | 'maintenance'
  | 'upload'

interface PresetConfig {
  icon: React.ReactNode
  title: string
  description: string
  iconColor?: VariantProps<typeof iconContainerVariants>['iconColor']
}

const presetIconSizeMap: Record<string, 'xs' | 'sm' | 'md' | 'lg' | 'xl'> = {
  compact: 'sm',
  sm: 'sm',
  md: 'md',
  lg: 'lg',
  xl: 'xl',
  xs: 'xs',
}

const presetConfig: Record<EmptyStatePresetType, PresetConfig> = {
  'no-results': {
    icon: <Search />,
    title: 'No results found',
    description: 'Try adjusting your search or filter to find what you\'re looking for.',
  },
  'no-data': {
    icon: <FileQuestion />,
    title: 'No data available',
    description: 'There\'s nothing to display here yet. Data will appear once available.',
  },
  'empty-inbox': {
    icon: <Inbox />,
    title: 'Inbox is empty',
    description: 'You\'re all caught up! New messages will appear here.',
  },
  'empty-folder': {
    icon: <FolderOpen />,
    title: 'Folder is empty',
    description: 'This folder doesn\'t contain any files yet.',
  },
  'no-users': {
    icon: <Users />,
    title: 'No users found',
    description: 'There are no users matching your criteria.',
  },
  'empty-cart': {
    icon: <ShoppingCart />,
    title: 'Your cart is empty',
    description: 'Looks like you haven\'t added anything to your cart yet.',
  },
  'no-notifications': {
    icon: <Bell />,
    title: 'No notifications',
    description: 'You\'re all caught up! Check back later for updates.',
  },
  'no-images': {
    icon: <Image />,
    title: 'No images',
    description: 'There are no images to display. Upload some to get started.',
  },
  // New presets
  'error': {
    icon: <AlertTriangle />,
    title: 'Something went wrong',
    description: 'An error occurred. Please try again or contact support if the problem persists.',
    iconColor: 'destructive',
  },
  'offline': {
    icon: <WifiOff />,
    title: 'You\'re offline',
    description: 'Please check your internet connection and try again.',
    iconColor: 'warning',
  },
  'permission-denied': {
    icon: <ShieldX />,
    title: 'Access denied',
    description: 'You don\'t have permission to view this content.',
    iconColor: 'destructive',
  },
  'coming-soon': {
    icon: <Clock />,
    title: 'Coming soon',
    description: 'This feature is under development. Stay tuned for updates!',
    iconColor: 'accent',
  },
  'maintenance': {
    icon: <Wrench />,
    title: 'Under maintenance',
    description: 'We\'re performing scheduled maintenance. Please check back soon.',
    iconColor: 'warning',
  },
  'upload': {
    icon: <Upload />,
    title: 'Upload files',
    description: 'Drag and drop files here, or click to browse.',
    iconColor: 'primary',
  },
}

export interface EmptyStatePresetProps
  extends Omit<EmptyStateProps, 'children'>,
    Omit<VariantProps<typeof iconContainerVariants>, 'size'> {
  preset: EmptyStatePresetType
  action?: React.ReactNode
  customTitle?: string
  customDescription?: string
  /** Custom icon to override the preset icon */
  customIcon?: React.ReactNode
  /** Custom illustration (overrides icon completely) */
  illustration?: React.ReactNode
  /** Icon container size */
  iconSize?: VariantProps<typeof iconContainerVariants>['size']
}

const EmptyStatePreset = React.forwardRef<HTMLDivElement, EmptyStatePresetProps>(
  (
    {
      preset,
      action,
      customTitle,
      customDescription,
      customIcon,
      illustration,
      iconColor,
      iconSize,
      size,
      layout,
      variant,
      animation,
      className,
      ...props
    },
    ref
  ) => {
    const config = presetConfig[preset]
    const finalIconColor = iconColor ?? config.iconColor ?? 'default'

    return (
      <EmptyState
        ref={ref}
        variant={variant}
        size={size}
        layout={layout}
        animation={animation}
        className={className}
        {...props}
      >
        {illustration ? (
          <EmptyStateIllustration>{illustration}</EmptyStateIllustration>
        ) : (
          <EmptyStateIcon iconColor={finalIconColor} size={iconSize ?? presetIconSizeMap[size ?? 'md'] ?? 'md'}>
            {customIcon ?? config.icon}
          </EmptyStateIcon>
        )}
        <div className={cn(layout === 'horizontal' && 'flex flex-col')}>
          <EmptyStateTitle>{customTitle ?? config.title}</EmptyStateTitle>
          <EmptyStateDescription>{customDescription ?? config.description}</EmptyStateDescription>
          {action && <EmptyStateActions>{action}</EmptyStateActions>}
        </div>
      </EmptyState>
    )
  }
)
EmptyStatePreset.displayName = 'EmptyStatePreset'

// ============================================================================
// Exports
// ============================================================================

export {
  EmptyState,
  EmptyStateIcon,
  EmptyStateIllustration,
  EmptyStateTitle,
  EmptyStateDescription,
  EmptyStateActions,
  EmptyStatePreset,
  emptyStateVariants,
  iconContainerVariants,
}
```

---

### Funnel Chart <a name="funnel-chart"></a>

Funnel chart for conversion flows and pipeline stages with neubrutalism styling

- **Registry Name**: `@boldkit/funnel-chart`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/funnel-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/chart`

#### How to Use

##### File: `registry/default/example/funnel-chart-demo.tsx`

```tsx
import { FunnelChart } from "@/components/ui/funnel-chart"

export default function FunnelChartDemo() {
  return (
    <FunnelChart
      data={[
        { name: "Visitors", value: 5000 },
        { name: "Leads", value: 3000 },
        { name: "Prospects", value: 1500 },
        { name: "Customers", value: 800 },
      ]}
    />
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/funnel-chart.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'
import { FunnelChart as RechartsFC, Funnel, LabelList, Tooltip, ResponsiveContainer } from 'recharts'
import { ChartEmpty } from './chart'

export interface FunnelChartData {
  name: string
  value: number
  fill?: string
}

export interface FunnelChartProps extends React.HTMLAttributes<HTMLDivElement> {
  data: FunnelChartData[]
  showLabels?: boolean
  showTooltip?: boolean
  animated?: boolean
  height?: number
  /** Accessible label for screen readers (default: "Funnel chart") */
  ariaLabel?: string
  emptyState?: React.ReactNode
}

const NEUBRUTALISM_COLORS = [
  'hsl(var(--primary))',
  'hsl(var(--secondary))',
  'hsl(var(--accent))',
  'hsl(var(--success))',
  'hsl(var(--info))',
  'hsl(var(--warning))',
]

const FunnelChart = React.forwardRef<HTMLDivElement, FunnelChartProps>(
  (
    {
      data,
      showLabels = true,
      showTooltip = true,
      animated = true,
      height = 300,
      ariaLabel = 'Funnel chart',
      emptyState,
      className,
      ...props
    },
    ref
  ) => {
    if (!data || data.length === 0) {
      return <ChartEmpty ref={ref} message={emptyState} className={className} {...props} />
    }

    const coloredData = data.map((d, i) => ({
      ...d,
      fill: d.fill || NEUBRUTALISM_COLORS[i % NEUBRUTALISM_COLORS.length],
    }))

    return (
      <div
        ref={ref}
        role="img"
        aria-label={ariaLabel}
        className={cn('w-full', className)}
        style={{ height }}
        {...props}
      >
        <ResponsiveContainer width="100%" height="100%">
          <RechartsFC>
            <Funnel
              dataKey="value"
              data={coloredData}
              isAnimationActive={animated}
              animationDuration={400}
              stroke="hsl(var(--foreground))"
              strokeWidth={3}
            >
              {showLabels && (
                <LabelList
                  position="right"
                  fill="hsl(var(--foreground))"
                  stroke="none"
                  dataKey="name"
                  style={{ fontFamily: "'DM Mono', monospace", fontSize: 11, fontWeight: 700 }}
                />
              )}
            </Funnel>
            {showTooltip && (
              <Tooltip
                contentStyle={{
                  border: '3px solid hsl(var(--foreground))',
                  borderRadius: 0,
                  boxShadow: '4px 4px 0px hsl(var(--foreground))',
                  background: 'hsl(var(--background))',
                  fontFamily: "'DM Mono', monospace",
                  fontSize: 12,
                }}
                formatter={(value: number | undefined, name: string | undefined) => [`${(value ?? 0).toLocaleString()}`, name ?? '']}
              />
            )}
          </RechartsFC>
        </ResponsiveContainer>
      </div>
    )
  }
)
FunnelChart.displayName = 'FunnelChart'

export { FunnelChart }
```

---

### Gauge Chart <a name="gauge-chart"></a>

Speedometer-style gauge with color zones and animated needle - custom SVG implementation

- **Registry Name**: `@boldkit/gauge-chart`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/gauge-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/gauge-chart-demo.tsx`

```tsx
import { GaugeChart } from "@/components/ui/gauge-chart"

export default function GaugeChartDemo() {
  return <GaugeChart value={72} label="Performance" />
}
```

#### Component Source Code

##### File: `registry/default/ui/gauge-chart.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'

const gaugeChartVariants = cva(
  'relative flex items-center justify-center',
  {
    variants: {
      size: {
        sm: '',
        md: '',
        lg: '',
      },
      variant: {
        semicircle: '',
        full: '',
        meter: '',
      },
    },
    defaultVariants: {
      size: 'md',
      variant: 'semicircle',
    },
  }
)

export interface GaugeChartZone {
  from: number
  to: number
  color: string
  label?: string
}

export interface GaugeChartProps
  extends Omit<React.HTMLAttributes<HTMLDivElement>, 'color'>,
    VariantProps<typeof gaugeChartVariants> {
  value: number
  min?: number
  max?: number
  zones?: GaugeChartZone[]
  label?: string
  valueFormatter?: (value: number) => string
  showTicks?: boolean
  animated?: boolean
}

const DEFAULT_ZONES: GaugeChartZone[] = [
  { from: 0, to: 33, color: 'hsl(var(--destructive))', label: 'Low' },
  { from: 33, to: 66, color: 'hsl(var(--warning))', label: 'Medium' },
  { from: 66, to: 100, color: 'hsl(var(--success))', label: 'High' },
]

/**
 * Variant arc configs:
 *   semicircle — 180° sweep, arc from left (-90°) to right (+90°), open at bottom
 *   full       — 360° sweep (330° drawn with a gap at the bottom to show min/max)
 *   meter      — same 180° sweep as semicircle but with denser tick marks (every 10%)
 */
const VARIANT_ARC_CONFIG = {
  semicircle: { arcStartDeg: -90, sweepDeg: 180 },
  full:       { arcStartDeg: -90, sweepDeg: 360 }, // full 360° sweep
  meter:      { arcStartDeg: -90, sweepDeg: 180 },
} as const

type Variant = 'semicircle' | 'full' | 'meter'

const GaugeChart = React.forwardRef<HTMLDivElement, GaugeChartProps>(
  (
    {
      value,
      min = 0,
      max = 100,
      zones = DEFAULT_ZONES,
      label,
      valueFormatter = (v) => `${v}`,
      showTicks = true,
      animated = true,
      size,
      variant,
      className,
      ...props
    },
    ref
  ) => {
    const resolvedVariant: Variant = (variant as Variant) || 'semicircle'
    const arcConfig = VARIANT_ARC_CONFIG[resolvedVariant]

    const normalizedValue = Math.max(min, Math.min(max, value))
    const percentage = max === min ? 0 : ((normalizedValue - min) / (max - min)) * 100

    // SVG dimensions — full variant needs a taller canvas to show the bottom arc
    const sizeConfig = {
      sm: {
        width: 140,
        height: resolvedVariant === 'full' ? 140 : 90,
        radius: 45,
        strokeWidth: 10,
        fontSize: 14,
        labelSize: 9,
      },
      md: {
        width: 180,
        height: resolvedVariant === 'full' ? 180 : 115,
        radius: 58,
        strokeWidth: 12,
        fontSize: 18,
        labelSize: 11,
      },
      lg: {
        width: 240,
        height: resolvedVariant === 'full' ? 240 : 150,
        radius: 76,
        strokeWidth: 14,
        fontSize: 22,
        labelSize: 13,
      },
    }

    const currentSize = size || 'md'
    const config = sizeConfig[currentSize]

    // For full variant, center is the geometric center of the SVG
    // For semicircle/meter, center is pushed up so the arc+needle fits in the half-height canvas
    const centerX = config.width / 2
    const centerY =
      resolvedVariant === 'full'
        ? config.height / 2
        : config.radius + config.strokeWidth + 5

    const isMeter = resolvedVariant === 'meter'

    const needleLength = config.radius - 8

    // Convert a percentage (0–100) along the arc to an SVG angle in radians
    const percentToAngleRad = (pct: number) => {
      const deg = arcConfig.arcStartDeg + (pct * arcConfig.sweepDeg) / 100
      return deg * (Math.PI / 180)
    }

    // Create an SVG arc path segment between two percentages on the gauge track
    const createArcPath = (startPercent: number, endPercent: number, radius: number) => {
      const startAngle = percentToAngleRad(startPercent)
      const endAngle = percentToAngleRad(endPercent)

      const startX = centerX + radius * Math.cos(startAngle)
      const startY = centerY + radius * Math.sin(startAngle)

      // A full 360° arc is degenerate in SVG (start === end point); split into two half-arcs
      if (resolvedVariant === 'full' && Math.abs(endPercent - startPercent) >= 100) {
        const midAngle = startAngle + Math.PI
        const midX = centerX + radius * Math.cos(midAngle)
        const midY = centerY + radius * Math.sin(midAngle)
        return `M ${startX} ${startY} A ${radius} ${radius} 0 1 1 ${midX} ${midY} A ${radius} ${radius} 0 1 1 ${startX} ${startY}`
      }

      const endX = centerX + radius * Math.cos(endAngle)
      const endY = centerY + radius * Math.sin(endAngle)

      const sweepDelta = endPercent - startPercent
      const sweepAngle = (sweepDelta / 100) * arcConfig.sweepDeg
      const largeArcFlag = sweepAngle > 180 ? 1 : 0

      return `M ${startX} ${startY} A ${radius} ${radius} 0 ${largeArcFlag} 1 ${endX} ${endY}`
    }

    // Needle angle: percentage along the sweep, converted to an absolute SVG rotation
    // The needle points upward (the line is drawn rightward then rotated)
    const needleAngle = arcConfig.arcStartDeg + (percentage * arcConfig.sweepDeg) / 100

    const currentZoneColor =
      zones.find((z) => percentage >= z.from && percentage <= z.to)?.color ||
      'hsl(var(--primary))'

    // Tick marks: semicircle/full use 5 ticks at 0/25/50/75/100 %
    // meter variant gets denser ticks at every 10%
    const tickPercentages =
      resolvedVariant === 'meter'
        ? [0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
        : [0, 25, 50, 75, 100]

    return (
      <div
        ref={ref}
        className={cn(gaugeChartVariants({ size, variant }), className)}
        style={{ maxWidth: config.width }}
        {...props}
      >
        <svg
          width="100%"
          height="auto"
          viewBox={`0 0 ${config.width} ${config.height}`}
        >
          {/* Background track */}
          <path
            d={createArcPath(0, 100, config.radius)}
            fill="none"
            stroke="hsl(var(--muted))"
            strokeWidth={config.strokeWidth}
            strokeLinecap="round"
          />

          {/* Zone arcs */}
          {zones.map((zone) => (
            <path
              key={`${zone.from}-${zone.to}-${zone.color}`}
              d={createArcPath(zone.from, zone.to, config.radius)}
              fill="none"
              stroke={zone.color}
              strokeWidth={config.strokeWidth}
              strokeLinecap="butt"
              className="transition-all duration-300"
            />
          ))}

          {/* Outer border */}
          <path
            d={createArcPath(0, 100, config.radius + config.strokeWidth / 2 + 2)}
            fill="none"
            stroke="hsl(var(--foreground))"
            strokeWidth="2"
            strokeLinecap="round"
          />

          {/* Inner border */}
          <path
            d={createArcPath(0, 100, config.radius - config.strokeWidth / 2 - 2)}
            fill="none"
            stroke="hsl(var(--foreground))"
            strokeWidth="2"
            strokeLinecap="round"
          />

          {/* Tick marks */}
          {showTicks &&
            tickPercentages.map((tick) => {
              const angle = percentToAngleRad(tick)
              const innerR = config.radius - config.strokeWidth / 2 - 6
              const outerR = config.radius + config.strokeWidth / 2 + 6
              const x1 = centerX + innerR * Math.cos(angle)
              const y1 = centerY + innerR * Math.sin(angle)
              const x2 = centerX + outerR * Math.cos(angle)
              const y2 = centerY + outerR * Math.sin(angle)

              return (
                <line
                  key={tick}
                  x1={x1}
                  y1={y1}
                  x2={x2}
                  y2={y2}
                  stroke="hsl(var(--foreground))"
                  strokeWidth="2"
                />
              )
            })}

          {/* Meter variant: filled progress arc using stroke-dasharray animation */}
          {isMeter && (
            <path
              d={createArcPath(0, 100, config.radius)}
              fill="none"
              stroke={currentZoneColor}
              strokeWidth={config.strokeWidth + 4}
              strokeLinecap="round"
              pathLength={100}
              strokeDasharray={`${percentage} 100`}
              style={{ transition: animated ? 'stroke-dasharray 0.5s ease-out' : 'none' }}
            />
          )}

          {/* Needle (hidden for meter variant) */}
          {!isMeter && (
            <g
              style={{
                transform: `rotate(${needleAngle}deg)`,
                transformOrigin: `${centerX}px ${centerY}px`,
                transition: animated ? 'transform 0.5s ease-out' : 'none',
                filter: 'drop-shadow(0 2px 2px rgba(0,0,0,0.3))',
              }}
            >
              {/* Needle body */}
              <line
                x1={centerX}
                y1={centerY}
                x2={centerX + needleLength}
                y2={centerY}
                stroke="hsl(var(--foreground))"
                strokeWidth="3"
                strokeLinecap="round"
              />
              {/* Needle tip */}
              <circle
                cx={centerX + needleLength}
                cy={centerY}
                r="3"
                fill="hsl(var(--primary))"
                stroke="hsl(var(--foreground))"
                strokeWidth="1.5"
              />
            </g>
          )}

          {/* Center pivot (hidden for meter variant) */}
          {!isMeter && (
            <>
              <circle
                cx={centerX}
                cy={centerY}
                r="6"
                fill="hsl(var(--foreground))"
              />
              <circle
                cx={centerX}
                cy={centerY}
                r="3"
                fill="hsl(var(--background))"
              />
            </>
          )}

          {/* Value display */}
          <text
            x={centerX}
            y={centerY + 20}
            textAnchor="middle"
            fill="hsl(var(--foreground))"
            fontWeight="900"
            fontSize={config.fontSize}
            fontFamily="ui-monospace, monospace"
          >
            {valueFormatter(normalizedValue)}
          </text>

          {/* Label */}
          {label && (
            <text
              x={centerX}
              y={centerY + 20 + config.fontSize}
              textAnchor="middle"
              fill="hsl(var(--muted-foreground))"
              fontWeight="700"
              fontSize={config.labelSize}
              style={{ textTransform: 'uppercase', letterSpacing: '0.05em' }}
            >
              {label}
            </text>
          )}

          {/* Min/Max labels — only for semicircle and meter (full variant has no clear endpoints) */}
          {resolvedVariant !== 'full' && (
            <>
              <text
                x={centerX - config.radius - 8}
                y={centerY + 4}
                textAnchor="end"
                fill="hsl(var(--muted-foreground))"
                fontWeight="600"
                fontSize={config.labelSize}
              >
                {min}
              </text>
              <text
                x={centerX + config.radius + 8}
                y={centerY + 4}
                textAnchor="start"
                fill="hsl(var(--muted-foreground))"
                fontWeight="600"
                fontSize={config.labelSize}
              >
                {max}
              </text>
            </>
          )}
        </svg>
      </div>
    )
  }
)
GaugeChart.displayName = 'GaugeChart'

export { GaugeChart, gaugeChartVariants }
```

---

### Heatmap Chart <a name="heatmap-chart"></a>

Heatmap grid for correlation and intensity data - custom CSS Grid implementation

- **Registry Name**: `@boldkit/heatmap-chart`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/heatmap-chart.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/chart`

#### How to Use

##### File: `registry/default/example/heatmap-chart-demo.tsx`

```tsx
import { HeatmapChart } from "@/components/ui/heatmap-chart"

export default function HeatmapChartDemo() {
  return (
    <HeatmapChart
      data={[
        { x: "Mon", y: "9am", value: 5 },
        { x: "Mon", y: "12pm", value: 8 },
        { x: "Tue", y: "9am", value: 3 },
        { x: "Tue", y: "12pm", value: 9 },
        { x: "Wed", y: "9am", value: 7 },
        { x: "Wed", y: "12pm", value: 4 },
      ]}
    />
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/heatmap-chart.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'
import { ChartEmpty } from './chart'

export interface HeatmapCellData {
  row: string
  col: string
  value: number
}

export interface HeatmapChartProps extends React.HTMLAttributes<HTMLDivElement> {
  data: HeatmapCellData[]
  rows: string[]
  cols: string[]
  /** CSS color at 0 intensity (default: transparent primary) */
  colorLow?: string
  /** CSS color at max intensity (default: primary) */
  colorHigh?: string
  showLabels?: boolean
  showTooltip?: boolean
  cellSize?: number
  /** Accessible label for screen readers (default: "Heatmap chart") */
  ariaLabel?: string
  /** Fires when a cell is clicked or activated via keyboard (Enter/Space). */
  onCellClick?: (cell: HeatmapCellData) => void
  emptyState?: React.ReactNode
}

function interpolateOpacity(value: number, min: number, max: number): number {
  if (max === min) return 0.5
  return (value - min) / (max - min)
}

/**
 * Compute cell background color from colorLow → colorHigh using CSS color-mix(),
 * which works with any valid CSS color string including hsl(var(--*)) tokens.
 * Falls back to primary-opacity when custom colors are not provided.
 */
function getCellColor(
  intensity: number,
  colorLow: string | undefined,
  colorHigh: string | undefined
): string {
  if (colorLow && colorHigh) {
    // color-mix interpolates in srgb space: at t=0 we want colorLow, at t=1 we want colorHigh.
    // color-mix(in srgb, colorHigh <t*100>%, colorLow) gives us colorHigh at t=1 and colorLow at t=0.
    const pct = Math.round(Math.max(0, Math.min(1, intensity)) * 100)
    return `color-mix(in srgb, ${colorHigh} ${pct}%, ${colorLow})`
  }
  // Default: vary opacity of primary from 0.08 (low) to 1 (high)
  return `hsl(var(--primary) / ${Math.max(0.08, intensity)})`
}

const HeatmapChart = React.forwardRef<HTMLDivElement, HeatmapChartProps>(
  (
    {
      data,
      rows,
      cols,
      colorLow,
      colorHigh,
      showLabels = true,
      showTooltip = true,
      cellSize = 40,
      ariaLabel = 'Heatmap chart',
      onCellClick,
      emptyState,
      className,
      ...props
    },
    ref
  ) => {
    const [tooltip, setTooltip] = React.useState<{ x: number; y: number; row: string; col: string; value: number } | null>(null)
    const isInteractive = !!onCellClick
    const cellTabIndex = showTooltip || isInteractive ? 0 : undefined

    const valueMap = React.useMemo(() => {
      const map = new Map<string, number>()
      data.forEach(d => map.set(`${d.row}__${d.col}`, d.value))
      return map
    }, [data])

    const { min, max } = React.useMemo(() => {
      if (data.length === 0) return { min: 0, max: 1 }
      const vals = data.map(d => d.value)
      return { min: Math.min(...vals), max: Math.max(...vals) }
    }, [data])

    const labelWidth = showLabels ? 72 : 8
    const headerHeight = showLabels ? 32 : 8

    if (!data || data.length === 0) {
      return <ChartEmpty ref={ref} message={emptyState} className={className} {...props} />
    }

    return (
      <div
        ref={ref}
        role="img"
        aria-label={ariaLabel}
        className={cn('relative w-full overflow-x-auto', className)}
        {...props}
      >
        <div
          style={{
            display: 'grid',
            gridTemplateColumns: `${labelWidth}px repeat(${cols.length}, ${cellSize}px)`,
            gridTemplateRows: `${headerHeight}px repeat(${rows.length}, ${cellSize}px)`,
            width: 'fit-content',
          }}
        >
          {/* Top-left empty corner */}
          <div />

          {/* Column headers */}
          {cols.map(col =>
            showLabels ? (
              <div
                key={col}
                className="flex items-end justify-center pb-1"
                style={{ fontSize: 10, fontFamily: "'DM Mono', monospace", fontWeight: 700 }}
              >
                <span style={{ transform: 'rotate(-45deg)', transformOrigin: 'bottom center', whiteSpace: 'nowrap' }}>
                  {col}
                </span>
              </div>
            ) : (
              <div key={col} />
            )
          )}

          {/* Rows */}
          {rows.map(row => (
            <React.Fragment key={row}>
              {/* Row label */}
              {showLabels ? (
                <div
                  className="flex items-center pr-2 text-right"
                  style={{ fontSize: 10, fontFamily: "'DM Mono', monospace", fontWeight: 700, justifyContent: 'flex-end' }}
                >
                  {row}
                </div>
              ) : (
                <div />
              )}

              {/* Cells */}
              {cols.map(col => {
                const value = valueMap.get(`${row}__${col}`) ?? 0
                const intensity = interpolateOpacity(value, min, max)

                return (
                  <div
                    key={col}
                    role={isInteractive ? 'button' : undefined}
                    aria-label={isInteractive ? `${row}, ${col}: ${value}` : undefined}
                    tabIndex={cellTabIndex}
                    className={cn(
                      'border border-foreground/30 transition-all duration-100 hover:border-foreground hover:border-2 hover:z-10 focus:border-foreground focus:border-2 focus:z-10 focus:outline-none',
                      isInteractive ? 'cursor-pointer' : 'cursor-default'
                    )}
                    style={{
                      backgroundColor: getCellColor(intensity, colorLow, colorHigh),
                    }}
                    onMouseEnter={(e) => {
                      if (showTooltip) {
                        const rect = e.currentTarget.getBoundingClientRect()
                        setTooltip({ x: rect.left + rect.width / 2, y: rect.top, row, col, value })
                      }
                    }}
                    onMouseLeave={() => setTooltip(null)}
                    onFocus={(e) => {
                      if (showTooltip) {
                        const rect = e.currentTarget.getBoundingClientRect()
                        setTooltip({ x: rect.left + rect.width / 2, y: rect.top, row, col, value })
                      }
                    }}
                    onBlur={() => setTooltip(null)}
                    onClick={onCellClick ? () => onCellClick({ row, col, value }) : undefined}
                    onKeyDown={
                      onCellClick
                        ? (e) => {
                            if (e.key === 'Enter' || e.key === ' ') {
                              e.preventDefault()
                              onCellClick({ row, col, value })
                            }
                          }
                        : undefined
                    }
                  />
                )
              })}
            </React.Fragment>
          ))}
        </div>

        {/* Tooltip */}
        {showTooltip && tooltip && (
          <div
            className="fixed z-50 pointer-events-none border-3 border-foreground bg-background px-3 py-2 text-xs font-mono shadow-[4px_4px_0px_hsl(var(--foreground))]"
            style={{
              left: Math.min(Math.max(tooltip.x, 100), window.innerWidth - 100),
              top: tooltip.y - 64 < 0 ? tooltip.y + 10 : tooltip.y - 64,
              transform: 'translateX(-50%)',
              maxWidth: 200,
            }}
          >
            <p className="font-black">{tooltip.row} × {tooltip.col}</p>
            <p className="text-muted-foreground">{tooltip.value}</p>
          </div>
        )}
      </div>
    )
  }
)
HeatmapChart.displayName = 'HeatmapChart'

export { HeatmapChart }
```

---

### Hover Card <a name="hover-card"></a>

A card that appears on hover with additional information

- **Registry Name**: `@boldkit/hover-card`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/hover-card.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-hover-card`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/hover-card-demo.tsx`

```tsx
import {
  HoverCard,
  HoverCardContent,
  HoverCardTrigger,
} from '@/components/ui/hover-card'

export default function Example() {
  return (
    <HoverCard>
      <HoverCardTrigger asChild>
        <Button variant="link">@boldkit</Button>
      </HoverCardTrigger>
      <HoverCardContent>
        <p>BoldKit - Neubrutalism UI Components</p>
      </HoverCardContent>
    </HoverCard>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/hover-card.tsx`

```tsx
import * as React from 'react'
import * as HoverCardPrimitive from '@radix-ui/react-hover-card'
import { cn } from '@/lib/utils'

const HoverCard = HoverCardPrimitive.Root

const HoverCardTrigger = HoverCardPrimitive.Trigger

const HoverCardContent = React.forwardRef<
  React.ElementRef<typeof HoverCardPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof HoverCardPrimitive.Content>
>(({ className, align = 'center', sideOffset = 4, ...props }, ref) => (
  <HoverCardPrimitive.Content
    ref={ref}
    align={align}
    sideOffset={sideOffset}
    className={cn(
      'z-50 w-64 border-3 border-foreground bg-popover p-4 text-popover-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))] outline-none data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2',
      className
    )}
    {...props}
  />
))
HoverCardContent.displayName = HoverCardPrimitive.Content.displayName

export { HoverCard, HoverCardTrigger, HoverCardContent }
```

---

### Input <a name="input"></a>

Displays a form input field with neubrutalism styling

- **Registry Name**: `@boldkit/input`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/input.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/input-demo.tsx`

```tsx
import { Input } from '@/components/ui/input'

export default function Example() {
  return <Input type="email" placeholder="Email" />
}
```

#### Component Source Code

##### File: `registry/default/ui/input.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'

const Input = React.forwardRef<HTMLInputElement, React.ComponentProps<'input'>>(
  ({ className, type, ...props }, ref) => {
    return (
      <input
        type={type}
        className={cn(
          'flex h-11 w-full border-3 border-input bg-background px-4 py-2 text-base shadow-[4px_4px_0px_hsl(var(--shadow-color))] transition-all duration-200 file:border-0 file:bg-transparent file:text-sm file:font-medium file:text-foreground placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-0 focus-visible:translate-x-[4px] focus-visible:translate-y-[4px] focus-visible:shadow-none disabled:cursor-not-allowed disabled:opacity-50 md:text-sm',
          className
        )}
        ref={ref}
        {...props}
      />
    )
  }
)
Input.displayName = 'Input'

export { Input }
```

---

### Input Otp <a name="input-otp"></a>

One-time password input with neubrutalism styling

- **Registry Name**: `@boldkit/input-otp`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/input-otp.json
```

#### Dependencies

- **NPM Packages**:
  - `input-otp`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/input-otp-demo.tsx`

```tsx
import {
  InputOTP,
  InputOTPGroup,
  InputOTPSlot,
} from '@/components/ui/input-otp'

export default function Example() {
  return (
    <InputOTP maxLength={6}>
      <InputOTPGroup>
        <InputOTPSlot index={0} />
        <InputOTPSlot index={1} />
        <InputOTPSlot index={2} />
        <InputOTPSlot index={3} />
        <InputOTPSlot index={4} />
        <InputOTPSlot index={5} />
      </InputOTPGroup>
    </InputOTP>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/input-otp.tsx`

```tsx
import * as React from 'react'
import { OTPInput, OTPInputContext } from 'input-otp'
import { Dot } from 'lucide-react'
import { cn } from '@/lib/utils'

const InputOTP = React.forwardRef<
  React.ElementRef<typeof OTPInput>,
  React.ComponentPropsWithoutRef<typeof OTPInput>
>(({ className, containerClassName, ...props }, ref) => (
  <OTPInput
    ref={ref}
    containerClassName={cn(
      'flex items-center gap-2 has-[:disabled]:opacity-50',
      containerClassName
    )}
    className={cn('disabled:cursor-not-allowed', className)}
    {...props}
  />
))
InputOTP.displayName = 'InputOTP'

const InputOTPGroup = React.forwardRef<
  React.ElementRef<'div'>,
  React.ComponentPropsWithoutRef<'div'>
>(({ className, ...props }, ref) => (
  <div ref={ref} className={cn('flex items-center', className)} {...props} />
))
InputOTPGroup.displayName = 'InputOTPGroup'

const InputOTPSlot = React.forwardRef<
  React.ElementRef<'div'>,
  React.ComponentPropsWithoutRef<'div'> & { index: number }
>(({ index, className, ...props }, ref) => {
  const inputOTPContext = React.useContext(OTPInputContext)
  const slot = inputOTPContext.slots?.[index] ?? { char: null, hasFakeCaret: false, isActive: false }
  const { char, hasFakeCaret, isActive } = slot

  return (
    <div
      ref={ref}
      className={cn(
        'relative flex h-12 w-12 items-center justify-center border-3 border-foreground bg-background text-sm font-bold uppercase shadow-[4px_4px_0px_hsl(var(--shadow-color))] transition-all duration-200',
        isActive && 'z-10 ring-2 ring-ring ring-offset-background translate-x-[4px] translate-y-[4px] shadow-none',
        className
      )}
      {...props}
    >
      {char}
      {hasFakeCaret && (
        <div className="pointer-events-none absolute inset-0 flex items-center justify-center">
          <div className="h-6 w-px animate-caret-blink bg-foreground duration-1000" />
        </div>
      )}
    </div>
  )
})
InputOTPSlot.displayName = 'InputOTPSlot'

const InputOTPSeparator = React.forwardRef<
  React.ElementRef<'div'>,
  React.ComponentPropsWithoutRef<'div'>
>(({ ...props }, ref) => (
  <div ref={ref} role="separator" {...props}>
    <Dot className="h-4 w-4" />
  </div>
))
InputOTPSeparator.displayName = 'InputOTPSeparator'

export { InputOTP, InputOTPGroup, InputOTPSlot, InputOTPSeparator }
```

---

### Kbd <a name="kbd"></a>

Keyboard key indicator for displaying shortcuts and hotkeys with neubrutalism styling

- **Registry Name**: `@boldkit/kbd`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/kbd.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/kbd-demo.tsx`

```tsx
import { Kbd, KbdCombo } from '@/components/ui/kbd'

export default function Example() {
  return (
    <>
      <Kbd>K</Kbd>
      <KbdCombo keys={['Ctrl', 'K']} />
    </>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/kbd.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'

const kbdVariants = cva(
  'inline-flex items-center justify-center font-mono font-bold uppercase border-3 border-foreground',
  {
    variants: {
      variant: {
        default: 'bg-muted shadow-[2px_2px_0px_hsl(var(--shadow-color))]',
        outline: 'bg-background',
        ghost: 'bg-transparent border-transparent text-muted-foreground',
      },
      size: {
        sm: 'min-w-5 h-5 px-1 text-[10px]',
        md: 'min-w-6 h-6 px-1.5 text-xs',
        lg: 'min-w-8 h-8 px-2 text-sm',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'md',
    },
  }
)

export interface KbdProps
  extends React.HTMLAttributes<HTMLElement>,
    VariantProps<typeof kbdVariants> {}

const Kbd = React.forwardRef<HTMLElement, KbdProps>(
  ({ className, variant, size, ...props }, ref) => {
    return (
      <kbd
        ref={ref}
        className={cn(kbdVariants({ variant, size, className }))}
        {...props}
      />
    )
  }
)
Kbd.displayName = 'Kbd'

export interface KbdComboProps extends Omit<KbdProps, 'children'> {
  keys: string[]
  separator?: React.ReactNode
}

const KbdCombo = React.forwardRef<HTMLDivElement, KbdComboProps>(
  ({ keys, separator = '+', variant, size, className, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn('inline-flex items-center gap-1', className)}
        {...props}
      >
        {keys.map((key, index) => (
          <React.Fragment key={`kbd-${index}-${key}`}>
            {index > 0 && (
              <span className="text-muted-foreground text-xs font-bold">{separator}</span>
            )}
            <Kbd variant={variant} size={size}>
              {key}
            </Kbd>
          </React.Fragment>
        ))}
      </div>
    )
  }
)
KbdCombo.displayName = 'KbdCombo'

export { Kbd, KbdCombo, kbdVariants }
```

---

### Label <a name="label"></a>

Renders an accessible label for form controls

- **Registry Name**: `@boldkit/label`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/label.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-label`
  - `class-variance-authority`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/input`

#### How to Use

##### File: `registry/default/example/label-demo.tsx`

```tsx
import { Label } from '@/components/ui/label'
import { Input } from '@/components/ui/input'

export default function Example() {
  return (
    <div className="grid w-full max-w-sm items-center gap-1.5">
      <Label htmlFor="email">Email</Label>
      <Input type="email" id="email" placeholder="Email" />
    </div>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/label.tsx`

```tsx
import * as React from 'react'
import * as LabelPrimitive from '@radix-ui/react-label'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'

const labelVariants = cva(
  'text-sm font-bold uppercase tracking-wide leading-none peer-disabled:cursor-not-allowed peer-disabled:opacity-70'
)

const Label = React.forwardRef<
  React.ElementRef<typeof LabelPrimitive.Root>,
  React.ComponentPropsWithoutRef<typeof LabelPrimitive.Root> &
    VariantProps<typeof labelVariants>
>(({ className, ...props }, ref) => (
  <LabelPrimitive.Root
    ref={ref}
    className={cn(labelVariants(), className)}
    {...props}
  />
))
Label.displayName = LabelPrimitive.Root.displayName

export { Label }
```

---

### Layered Card <a name="layered-card"></a>

Card component with stacked layers effect showing offset depth - neubrutalist depth illusion

- **Registry Name**: `@boldkit/layered-card`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/layered-card.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/layered-card-demo.tsx`

```tsx
import {
  LayeredCard,
  LayeredCardHeader,
  LayeredCardTitle,
  LayeredCardDescription,
  LayeredCardContent,
  LayeredCardFooter,
} from '@/components/ui/layered-card'

export default function Example() {
  return (
    <LayeredCard className="w-[350px]">
      <LayeredCardHeader>
        <LayeredCardTitle>Layered Card</LayeredCardTitle>
        <LayeredCardDescription>With stacked depth effect</LayeredCardDescription>
      </LayeredCardHeader>
      <LayeredCardContent>
        <p>This card has visible layers behind it for a 3D stacked paper effect.</p>
      </LayeredCardContent>
    </LayeredCard>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/layered-card.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'

const layeredCardVariants = cva('relative', {
  variants: {
    layers: {
      single: '',
      double: '',
      triple: '',
    },
    offset: {
      sm: '',
      default: '',
      lg: '',
    },
    layerColor: {
      default: '',
      primary: '',
      secondary: '',
      accent: '',
      muted: '',
    },
  },
  defaultVariants: {
    layers: 'double',
    offset: 'default',
    layerColor: 'default',
  },
})

export interface LayeredCardProps
  extends React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof layeredCardVariants> {
  /** Make the card interactive with hover effects */
  interactive?: boolean
}

const LayeredCard = React.forwardRef<HTMLDivElement, LayeredCardProps>(
  ({ className, layers, offset, layerColor, interactive = false, children, ...props }, ref) => {
    // Calculate offsets based on size
    const offsetSizes = {
      sm: 6,
      default: 8,
      lg: 12,
    }
    const offsetPx = offsetSizes[offset || 'default']

    // Get layer count
    const layerCount = layers === 'single' ? 1 : layers === 'triple' ? 3 : 2

    // Get background color class for layers
    const colorClasses = {
      default: 'bg-muted',
      primary: 'bg-primary',
      secondary: 'bg-secondary',
      accent: 'bg-accent',
      muted: 'bg-muted',
    }
    const layerBg = colorClasses[layerColor || 'default']

    return (
      <div
        ref={ref}
        className={cn(layeredCardVariants({ layers, offset, layerColor }), className)}
        {...props}
      >
        {/* Layer 3 (furthest back) */}
        {layerCount >= 3 && (
          <div
            className={cn(
              'absolute inset-0 border-3 border-foreground',
              layerBg,
              'opacity-50'
            )}
            style={{
              transform: `translate(${offsetPx * 3}px, ${offsetPx * 3}px)`,
            }}
          />
        )}

        {/* Layer 2 */}
        {layerCount >= 2 && (
          <div
            className={cn(
              'absolute inset-0 border-3 border-foreground',
              layerBg,
              'opacity-70'
            )}
            style={{
              transform: `translate(${offsetPx * 2}px, ${offsetPx * 2}px)`,
            }}
          />
        )}

        {/* Layer 1 */}
        {layerCount >= 1 && (
          <div
            className={cn('absolute inset-0 border-3 border-foreground', layerBg)}
            style={{
              transform: `translate(${offsetPx}px, ${offsetPx}px)`,
            }}
          />
        )}

        {/* Main card (top layer) */}
        <div
          className={cn(
            'relative border-3 border-foreground bg-card text-card-foreground',
            interactive &&
              'cursor-pointer transition-transform duration-200 hover:translate-x-[-4px] hover:translate-y-[-4px]'
          )}
        >
          {children}
        </div>
      </div>
    )
  }
)
LayeredCard.displayName = 'LayeredCard'

const LayeredCardHeader = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn(
      'flex flex-col space-y-1.5 border-b-3 border-foreground bg-muted p-4',
      className
    )}
    {...props}
  />
))
LayeredCardHeader.displayName = 'LayeredCardHeader'

const LayeredCardTitle = React.forwardRef<
  HTMLHeadingElement,
  React.HTMLAttributes<HTMLHeadingElement>
>(({ className, ...props }, ref) => (
  <h3
    ref={ref}
    className={cn('text-xl font-bold uppercase tracking-wide', className)}
    {...props}
  />
))
LayeredCardTitle.displayName = 'LayeredCardTitle'

const LayeredCardDescription = React.forwardRef<
  HTMLParagraphElement,
  React.HTMLAttributes<HTMLParagraphElement>
>(({ className, ...props }, ref) => (
  <p ref={ref} className={cn('text-sm text-muted-foreground', className)} {...props} />
))
LayeredCardDescription.displayName = 'LayeredCardDescription'

const LayeredCardContent = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div ref={ref} className={cn('p-4', className)} {...props} />
))
LayeredCardContent.displayName = 'LayeredCardContent'

const LayeredCardFooter = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn(
      'flex items-center border-t-3 border-foreground bg-muted p-4',
      className
    )}
    {...props}
  />
))
LayeredCardFooter.displayName = 'LayeredCardFooter'

export {
  LayeredCard,
  LayeredCardHeader,
  LayeredCardTitle,
  LayeredCardDescription,
  LayeredCardContent,
  LayeredCardFooter,
  layeredCardVariants,
}
```

---

### Marquee <a name="marquee"></a>

Auto-scrolling text ticker with neubrutalism styling - a common brutalist design element

- **Registry Name**: `@boldkit/marquee`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/marquee.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/marquee-demo.tsx`

```tsx
import { Marquee, MarqueeItem, MarqueeSeparator } from '@/components/ui/marquee'

export default function Example() {
  return (
    <Marquee>
      <MarqueeItem>Welcome to BoldKit</MarqueeItem>
      <MarqueeSeparator />
      <MarqueeItem>Neubrutalism UI</MarqueeItem>
      <MarqueeSeparator />
      <MarqueeItem>Bold & Beautiful</MarqueeItem>
      <MarqueeSeparator />
    </Marquee>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/marquee.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'

interface MarqueeProps extends React.HTMLAttributes<HTMLDivElement> {
  /** Content to display in the marquee */
  children: React.ReactNode
  /** Direction of the marquee animation */
  direction?: 'left' | 'right'
  /** Speed of the animation: 'slow' | 'normal' | 'fast' */
  speed?: 'slow' | 'normal' | 'fast'
  /** Pause animation on hover */
  pauseOnHover?: boolean
  /** Show neubrutalism border styling */
  bordered?: boolean
  /** Number of times to repeat the content (for seamless loop) */
  repeat?: number
}

const speedClasses = {
  slow: 'animate-marquee-slow',
  normal: 'animate-marquee',
  fast: 'animate-marquee-fast',
}

const reverseSpeedClasses = {
  slow: 'animate-marquee-slow-reverse',
  normal: 'animate-marquee-reverse',
  fast: 'animate-marquee-fast-reverse',
}

const Marquee = React.forwardRef<HTMLDivElement, MarqueeProps>(
  (
    {
      className,
      children,
      direction = 'left',
      speed = 'normal',
      pauseOnHover = true,
      bordered = true,
      repeat = 4,
      ...props
    },
    ref
  ) => {
    const animationClass = direction === 'right' ? reverseSpeedClasses[speed] : speedClasses[speed]

    return (
      <div
        ref={ref}
        className={cn(
          'flex overflow-hidden',
          bordered && 'border-3 border-foreground bg-background',
          className
        )}
        {...props}
      >
        <div
          className={cn(
            'marquee-content flex shrink-0 items-center gap-8 py-3',
            animationClass,
            pauseOnHover && 'hover:[animation-play-state:paused]'
          )}
        >
          {Array.from({ length: repeat }).map((_, i) => (
            <React.Fragment key={i}>{children}</React.Fragment>
          ))}
        </div>
        <div
          className={cn(
            'marquee-content flex shrink-0 items-center gap-8 py-3',
            animationClass,
            pauseOnHover && 'hover:[animation-play-state:paused]'
          )}
          aria-hidden="true"
        >
          {Array.from({ length: repeat }).map((_, i) => (
            <React.Fragment key={i}>{children}</React.Fragment>
          ))}
        </div>
      </div>
    )
  }
)
Marquee.displayName = 'Marquee'

interface MarqueeItemProps extends React.HTMLAttributes<HTMLSpanElement> {
  children: React.ReactNode
}

const MarqueeItem = React.forwardRef<HTMLSpanElement, MarqueeItemProps>(
  ({ className, children, ...props }, ref) => {
    return (
      <span
        ref={ref}
        className={cn(
          'inline-flex items-center gap-2 whitespace-nowrap px-4 text-lg font-bold uppercase tracking-wide',
          className
        )}
        {...props}
      >
        {children}
      </span>
    )
  }
)
MarqueeItem.displayName = 'MarqueeItem'

interface MarqueeSeparatorProps extends React.HTMLAttributes<HTMLSpanElement> {
  /** Separator character or element */
  children?: React.ReactNode
}

const MarqueeSeparator = React.forwardRef<HTMLSpanElement, MarqueeSeparatorProps>(
  ({ className, children = '/', ...props }, ref) => {
    return (
      <span
        ref={ref}
        className={cn('text-2xl font-black text-muted-foreground', className)}
        {...props}
      >
        {children}
      </span>
    )
  }
)
MarqueeSeparator.displayName = 'MarqueeSeparator'

export { Marquee, MarqueeItem, MarqueeSeparator }
```

---

### Math Curve Background <a name="math-curve-background"></a>

Ambient animated mathematical curve background layer with neubrutalism aesthetics — wraps content with a slow-moving curve animation

- **Registry Name**: `@boldkit/math-curve-background`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/math-curve-background.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-slot`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/math-curves`
  - `@boldkit/error-boundary`

#### How to Use

##### File: `registry/default/example/math-curve-background-demo.tsx`

```tsx
import { MathCurveBackground } from '@/components/ui/math-curve-background'

export default function Example() {
  return (
    <MathCurveBackground curve="rose" opacity={0.2} className="p-8">
      <p>Your content here</p>
    </MathCurveBackground>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/math-curve-background.tsx`

```tsx
import * as React from 'react'
import { Slot } from '@radix-ui/react-slot'
import { cn } from '@/lib/utils'
import { ErrorBoundary } from '@/components/ErrorBoundary'
import {
  buildPath,
  getPoint,
  getAngle,
  getDetailScale,
  getCurvePulseDuration,
  type BackgroundCurveKey,
} from '@/lib/math-curves'

const SPEED_DURATION: Record<string, number> = {
  slow: 9000,
  normal: 5500,
  fast: 3000,
}

export interface MathCurveBackgroundProps extends React.HTMLAttributes<HTMLDivElement> {
  curve?: BackgroundCurveKey
  speed?: 'slow' | 'normal' | 'fast'
  opacity?: number
  trackColor?: string
  headColor?: string
  strokeWidth?: number
  asChild?: boolean
  children?: React.ReactNode
}

const MathCurveBackground = React.forwardRef<HTMLDivElement, MathCurveBackgroundProps>(
  (
    {
      className,
      curve = 'rose',
      speed = 'slow',
      opacity = 0.15,
      trackColor,
      headColor,
      strokeWidth = 2,
      asChild = false,
      children,
      ...props
    },
    ref
  ) => {
    const pathRef = React.useRef<SVGPathElement>(null)
    const rectRef = React.useRef<SVGRectElement>(null)
    const rafRef = React.useRef<number>(0)
    const startTimeRef = React.useRef<number>(performance.now())

    const durationMs = SPEED_DURATION[speed] ?? SPEED_DURATION.slow
    const HEAD_SIZE = 8

    const initialTrackPath = React.useMemo(() => buildPath(curve, 1.0), [curve])

    React.useEffect(() => {
      startTimeRef.current = performance.now()

      const tick = () => {
        const now = performance.now()
        const elapsed = (now - startTimeRef.current) % durationMs
        const progress = elapsed / durationMs
        const detailScale = getDetailScale(now, getCurvePulseDuration(curve))

        const { x, y } = getPoint(curve, progress, detailScale)
        const angle = getAngle(curve, progress, detailScale)

        if (pathRef.current) {
          pathRef.current.setAttribute('d', buildPath(curve, detailScale))
        }

        if (rectRef.current) {
          const cx = x
          const cy = y
          rectRef.current.setAttribute('x', String(cx - HEAD_SIZE / 2))
          rectRef.current.setAttribute('y', String(cy - HEAD_SIZE / 2))
          rectRef.current.setAttribute('transform', `rotate(${angle} ${cx} ${cy})`)
        }

        rafRef.current = requestAnimationFrame(tick)
      }

      rafRef.current = requestAnimationFrame(tick)

      return () => {
        cancelAnimationFrame(rafRef.current)
      }
    }, [curve, speed, durationMs, strokeWidth])

    const resolvedTrackStroke = trackColor ?? 'currentColor'
    const resolvedHeadFill = headColor ?? 'hsl(var(--primary))'

    const Container = asChild ? Slot : 'div'

    return (
      <ErrorBoundary>
        <Container
          ref={ref}
          className={cn('relative', className)}
          {...props}
        >
          {/* Animated SVG background layer */}
          <svg
            viewBox="0 0 100 100"
            xmlns="http://www.w3.org/2000/svg"
            preserveAspectRatio="xMidYMid slice"
            aria-hidden="true"
            opacity={opacity}
            style={{
              position: 'absolute',
              inset: 0,
              width: '100%',
              height: '100%',
              zIndex: 0,
              overflow: 'hidden',
              pointerEvents: 'none',
            }}
          >
            <path
              ref={pathRef}
              d={initialTrackPath}
              fill="none"
              stroke={resolvedTrackStroke}
              strokeWidth={strokeWidth}
              strokeLinecap="square"
              strokeLinejoin="miter"
              className="transition-[stroke-opacity] duration-200"
            />
            <rect
              ref={rectRef}
              width={HEAD_SIZE}
              height={HEAD_SIZE}
              x={50 - HEAD_SIZE / 2}
              y={50 - HEAD_SIZE / 2}
              fill={resolvedHeadFill}
              stroke="currentColor"
              strokeWidth={1.5}
            />
          </svg>
          {/* Children sit above the SVG */}
          <div style={{ position: 'relative', zIndex: 1 }}>{children}</div>
        </Container>
      </ErrorBoundary>
    )
  }
)
MathCurveBackground.displayName = 'MathCurveBackground'

export { MathCurveBackground }
```

---

### Math Curve Loader <a name="math-curve-loader"></a>

Animated mathematical curve loader with neubrutalism aesthetics — 8 curve variants including rose, lissajous, butterfly, and more

- **Registry Name**: `@boldkit/math-curve-loader`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/math-curve-loader.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/math-curves`
  - `@boldkit/error-boundary`

#### How to Use

##### File: `registry/default/example/math-curve-loader-demo.tsx`

```tsx
import { MathCurveLoader } from '@/components/ui/math-curve-loader'

export default function Example() {
  return <MathCurveLoader curve="rose" />
}
```

#### Component Source Code

##### File: `registry/default/ui/math-curve-loader.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'
import { ErrorBoundary } from '@/components/ErrorBoundary'
import {
  buildPath,
  getPoint,
  getAngle,
  getDetailScale,
  getCurvePulseDuration,
  type LoaderCurveKey,
} from '@/lib/math-curves'

const mathCurveLoaderVariants = cva('', {
  variants: {
    size: {
      xs: 'w-6 h-6',
      sm: 'w-8 h-8',
      md: 'w-12 h-12',
      lg: 'w-16 h-16',
      xl: 'w-24 h-24',
    },
  },
  defaultVariants: {
    size: 'md',
  },
})

const SPEED_DURATION: Record<string, number> = {
  slow: 9000,
  normal: 5500,
  fast: 3000,
}

export interface MathCurveLoaderProps
  extends React.SVGAttributes<SVGSVGElement>,
    VariantProps<typeof mathCurveLoaderVariants> {
  curve?: LoaderCurveKey
  speed?: 'slow' | 'normal' | 'fast'
  trackColor?: string
  headColor?: string
  strokeWidth?: number
  headSize?: number
}

const MathCurveLoader = React.forwardRef<SVGSVGElement, MathCurveLoaderProps>(
  (
    {
      className,
      size,
      curve = 'rose',
      speed = 'normal',
      trackColor,
      headColor,
      strokeWidth = 4,
      headSize = 8,
      'aria-label': ariaLabel = 'Loading',
      ...props
    },
    ref
  ) => {
    const pathRef = React.useRef<SVGPathElement>(null)
    const rectRef = React.useRef<SVGRectElement>(null)
    const rafRef = React.useRef<number>(0)
    const startTimeRef = React.useRef<number>(performance.now())

    const durationMs = SPEED_DURATION[speed] ?? SPEED_DURATION.normal

    // Rebuild static track path when curve changes
    const trackPath = React.useMemo(() => buildPath(curve, 1.0), [curve])

    React.useEffect(() => {
      startTimeRef.current = performance.now()

      const tick = () => {
        const now = performance.now()
        const elapsed = (now - startTimeRef.current) % durationMs
        const progress = elapsed / durationMs
        const detailScale = getDetailScale(now, getCurvePulseDuration(curve))

        const { x, y } = getPoint(curve, progress, detailScale)
        const angle = getAngle(curve, progress, detailScale)

        // Update track path with breathing detail scale
        if (pathRef.current) {
          pathRef.current.setAttribute('d', buildPath(curve, detailScale))
        }

        // Update head rect position and rotation
        if (rectRef.current) {
          const cx = x
          const cy = y
          rectRef.current.setAttribute('x', String(cx - headSize / 2))
          rectRef.current.setAttribute('y', String(cy - headSize / 2))
          rectRef.current.setAttribute('transform', `rotate(${angle} ${cx} ${cy})`)
        }

        rafRef.current = requestAnimationFrame(tick)
      }

      rafRef.current = requestAnimationFrame(tick)

      return () => {
        cancelAnimationFrame(rafRef.current)
      }
    }, [curve, speed, durationMs, headSize])

    const resolvedTrackStroke = trackColor ?? 'currentColor'
    const resolvedHeadFill = headColor ?? 'hsl(var(--primary))'

    return (
      <ErrorBoundary>
        <svg
          ref={ref}
          viewBox="0 0 100 100"
          xmlns="http://www.w3.org/2000/svg"
          role="status"
          aria-label={ariaLabel}
          className={cn(
            mathCurveLoaderVariants({ size }),
            'group',
            className
          )}
          {...props}
        >
          {/* Track layer */}
          <path
            ref={pathRef}
            d={trackPath}
            fill="none"
            stroke={resolvedTrackStroke}
            strokeWidth={strokeWidth}
            strokeOpacity={0.2}
            strokeLinecap="square"
            strokeLinejoin="miter"
            className="group-hover:[stroke-opacity:0.4] transition-[stroke-opacity] duration-200"
          />
          {/* Head square */}
          <rect
            ref={rectRef}
            width={headSize}
            height={headSize}
            x={50 - headSize / 2}
            y={50 - headSize / 2}
            fill={resolvedHeadFill}
            stroke="currentColor"
            strokeWidth={1.5}
          />
        </svg>
      </ErrorBoundary>
    )
  }
)
MathCurveLoader.displayName = 'MathCurveLoader'

export { MathCurveLoader, mathCurveLoaderVariants }
```

---

### Math Curve Progress <a name="math-curve-progress"></a>

Progress indicator that traces a mathematical curve — value-driven head moves along the curve path from 0 to 100%

- **Registry Name**: `@boldkit/math-curve-progress`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/math-curve-progress.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/math-curves`
  - `@boldkit/error-boundary`

#### How to Use

##### File: `registry/default/example/math-curve-progress-demo.tsx`

```tsx
import { MathCurveProgress } from '@/components/ui/math-curve-progress'

export default function Example() {
  return <MathCurveProgress value={65} curve="spiral" />
}
```

#### Component Source Code

##### File: `registry/default/ui/math-curve-progress.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'
import { ErrorBoundary } from '@/components/ErrorBoundary'
import {
  buildPath,
  getPoint,
  getAngle,
  type ProgressCurveKey,
} from '@/lib/math-curves'

const mathCurveProgressVariants = cva('', {
  variants: {
    size: {
      sm: 'w-12 h-12',
      md: 'w-16 h-16',
      lg: 'w-24 h-24',
    },
  },
  defaultVariants: {
    size: 'md',
  },
})

export interface MathCurveProgressProps
  extends React.SVGAttributes<SVGSVGElement>,
    VariantProps<typeof mathCurveProgressVariants> {
  value: number
  curve?: ProgressCurveKey
  showValue?: boolean
  trackColor?: string
  fillColor?: string
  strokeWidth?: number
}

const MathCurveProgress = React.forwardRef<SVGSVGElement, MathCurveProgressProps>(
  (
    {
      className,
      size,
      value,
      curve = 'spiral',
      showValue = false,
      trackColor,
      fillColor,
      strokeWidth = 4,
      ...props
    },
    ref
  ) => {
    const clampedValue = Math.min(100, Math.max(0, value))
    const progress = clampedValue / 100

    const trackPath = React.useMemo(() => buildPath(curve, 1.0), [curve])
    const headPoint = React.useMemo(() => getPoint(curve, progress), [curve, progress])
    const headAngle = React.useMemo(() => getAngle(curve, progress), [curve, progress])
    const HEAD_SIZE = 8

    const resolvedTrackStroke = trackColor ?? 'currentColor'
    const resolvedFillColor = fillColor ?? 'hsl(var(--primary))'

    return (
      <ErrorBoundary>
        <svg
          ref={ref}
          viewBox="0 0 100 100"
          xmlns="http://www.w3.org/2000/svg"
          role="progressbar"
          aria-valuenow={clampedValue}
          aria-valuemin={0}
          aria-valuemax={100}
          aria-label={`${Math.round(clampedValue)}% progress`}
          className={cn(mathCurveProgressVariants({ size }), className)}
          {...props}
        >
          {/* Track layer */}
          <path
            d={trackPath}
            fill="none"
            stroke={resolvedTrackStroke}
            strokeWidth={strokeWidth}
            strokeOpacity={0.2}
            strokeLinecap="square"
            strokeLinejoin="miter"
          />
          {/* Head square — CSS transition for smooth value-driven movement */}
          <rect
            width={HEAD_SIZE}
            height={HEAD_SIZE}
            x={headPoint.x - HEAD_SIZE / 2}
            y={headPoint.y - HEAD_SIZE / 2}
            fill={resolvedFillColor}
            stroke="currentColor"
            strokeWidth={1.5}
            style={{
              transformBox: 'fill-box',
              transformOrigin: 'center',
              transform: `rotate(${headAngle}deg)`,
              transition: 'transform 300ms ease',
            }}
          />
          {/* Optional numeric label */}
          {showValue && (
            <text
              x={50}
              y={50}
              textAnchor="middle"
              dominantBaseline="central"
              fontSize={12}
              fontWeight={700}
              fill="currentColor"
              letterSpacing="0.05em"
              style={{ userSelect: 'none', fontFamily: 'inherit' }}
            >
              {Math.round(clampedValue)}%
            </text>
          )}
        </svg>
      </ErrorBoundary>
    )
  }
)
MathCurveProgress.displayName = 'MathCurveProgress'

export { MathCurveProgress, mathCurveProgressVariants }
```

---

### Pagination <a name="pagination"></a>

Pagination controls for navigating through pages

- **Registry Name**: `@boldkit/pagination`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/pagination.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`

#### How to Use

##### File: `registry/default/example/pagination-demo.tsx`

```tsx
import {
  Pagination,
  PaginationContent,
  PaginationItem,
  PaginationLink,
  PaginationNext,
  PaginationPrevious,
} from '@/components/ui/pagination'

export default function Example() {
  return (
    <Pagination>
      <PaginationContent>
        <PaginationItem>
          <PaginationPrevious href="#" />
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">1</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#" isActive>2</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">3</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationNext href="#" />
        </PaginationItem>
      </PaginationContent>
    </Pagination>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/pagination.tsx`

```tsx
import * as React from 'react'
import { ChevronLeft, ChevronRight, MoreHorizontal } from 'lucide-react'
import { cn } from '@/lib/utils'
import { type ButtonProps, buttonVariants } from '@/components/ui/button'

const Pagination = ({ className, ...props }: React.ComponentProps<'nav'>) => (
  <nav
    role="navigation"
    aria-label="pagination"
    className={cn('mx-auto flex w-full justify-center', className)}
    {...props}
  />
)
Pagination.displayName = 'Pagination'

const PaginationContent = React.forwardRef<
  HTMLUListElement,
  React.ComponentProps<'ul'>
>(({ className, ...props }, ref) => (
  <ul
    ref={ref}
    className={cn('flex flex-row items-center gap-1', className)}
    {...props}
  />
))
PaginationContent.displayName = 'PaginationContent'

const PaginationItem = React.forwardRef<
  HTMLLIElement,
  React.ComponentProps<'li'>
>(({ className, ...props }, ref) => (
  <li ref={ref} className={cn('', className)} {...props} />
))
PaginationItem.displayName = 'PaginationItem'

type PaginationLinkProps = {
  isActive?: boolean
} & Pick<ButtonProps, 'size'> &
  React.ComponentProps<'a'>

const PaginationLink = ({
  className,
  isActive,
  size = 'icon',
  ...props
}: PaginationLinkProps) => (
  <a
    aria-current={isActive ? 'page' : undefined}
    className={cn(
      buttonVariants({
        variant: isActive ? 'default' : 'outline',
        size,
      }),
      className
    )}
    {...props}
  />
)
PaginationLink.displayName = 'PaginationLink'

const PaginationPrevious = ({
  className,
  ...props
}: React.ComponentProps<typeof PaginationLink>) => (
  <PaginationLink
    aria-label="Go to previous page"
    size="default"
    className={cn('gap-1 pl-2.5', className)}
    {...props}
  >
    <ChevronLeft className="h-4 w-4 stroke-[3]" />
    <span>Previous</span>
  </PaginationLink>
)
PaginationPrevious.displayName = 'PaginationPrevious'

const PaginationNext = ({
  className,
  ...props
}: React.ComponentProps<typeof PaginationLink>) => (
  <PaginationLink
    aria-label="Go to next page"
    size="default"
    className={cn('gap-1 pr-2.5', className)}
    {...props}
  >
    <span>Next</span>
    <ChevronRight className="h-4 w-4 stroke-[3]" />
  </PaginationLink>
)
PaginationNext.displayName = 'PaginationNext'

const PaginationEllipsis = ({
  className,
  ...props
}: React.ComponentProps<'span'>) => (
  <>
    <span
      aria-hidden
      className={cn('flex h-9 w-9 items-center justify-center', className)}
      {...props}
    >
      <MoreHorizontal className="h-4 w-4 stroke-[3]" />
    </span>
    <span className="sr-only">More pages</span>
  </>
)
PaginationEllipsis.displayName = 'PaginationEllipsis'

export {
  Pagination,
  PaginationContent,
  PaginationLink,
  PaginationItem,
  PaginationPrevious,
  PaginationNext,
  PaginationEllipsis,
}
```

---

### Popover <a name="popover"></a>

Displays rich content in a portal with neubrutalism styling

- **Registry Name**: `@boldkit/popover`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/popover.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-popover`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`

#### How to Use

##### File: `registry/default/example/popover-demo.tsx`

```tsx
import { Popover, PopoverContent, PopoverTrigger } from '@/components/ui/popover'
import { Button } from '@/components/ui/button'

export default function Example() {
  return (
    <Popover>
      <PopoverTrigger asChild>
        <Button>Open Popover</Button>
      </PopoverTrigger>
      <PopoverContent>
        Place content for the popover here.
      </PopoverContent>
    </Popover>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/popover.tsx`

```tsx
import * as React from 'react'
import * as PopoverPrimitive from '@radix-ui/react-popover'
import { cn } from '@/lib/utils'

const Popover = PopoverPrimitive.Root

const PopoverTrigger = PopoverPrimitive.Trigger

const PopoverAnchor = PopoverPrimitive.Anchor

const PopoverContent = React.forwardRef<
  React.ElementRef<typeof PopoverPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof PopoverPrimitive.Content>
>(({ className, align = 'center', sideOffset = 4, ...props }, ref) => (
  <PopoverPrimitive.Portal>
    <PopoverPrimitive.Content
      ref={ref}
      align={align}
      sideOffset={sideOffset}
      className={cn(
        'z-50 w-72 border-3 border-foreground bg-popover p-4 text-popover-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))] outline-none',
        className
      )}
      {...props}
    />
  </PopoverPrimitive.Portal>
))
PopoverContent.displayName = PopoverPrimitive.Content.displayName

export { Popover, PopoverTrigger, PopoverContent, PopoverAnchor }
```

---

### Progress <a name="progress"></a>

Displays an indicator showing the completion progress of a task

- **Registry Name**: `@boldkit/progress`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/progress.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-progress`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/progress-demo.tsx`

```tsx
import { Progress } from '@/components/ui/progress'

export default function Example() {
  return <Progress value={60} />
}
```

#### Component Source Code

##### File: `registry/default/ui/progress.tsx`

```tsx
import * as React from 'react'
import * as ProgressPrimitive from '@radix-ui/react-progress'
import { cn } from '@/lib/utils'

const Progress = React.forwardRef<
  React.ElementRef<typeof ProgressPrimitive.Root>,
  React.ComponentPropsWithoutRef<typeof ProgressPrimitive.Root>
>(({ className, value, ...props }, ref) => {
  const clampedValue = Math.max(0, Math.min(100, value ?? 0))
  return (
  <ProgressPrimitive.Root
    ref={ref}
    className={cn(
      'relative h-5 w-full overflow-hidden border-3 border-foreground bg-muted shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
      className
    )}
    {...props}
  >
    <ProgressPrimitive.Indicator
      className="h-full w-full flex-1 bg-primary transition-all duration-500 ease-out"
      style={{ transform: `translateX(-${100 - clampedValue}%)` }}
    />
  </ProgressPrimitive.Root>
  )
})
Progress.displayName = ProgressPrimitive.Root.displayName

export { Progress }
```

---

### Radar Chart <a name="radar-chart"></a>

Multi-variable comparison chart for skills, metrics, and feature comparisons

- **Registry Name**: `@boldkit/radar-chart`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/radar-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/chart`

#### How to Use

##### File: `registry/default/example/radar-chart-demo.tsx`

```tsx
import { RadarChart } from '@/components/ui/radar-chart'

const data = [
  { subject: 'Speed', player1: 80, player2: 65 },
  { subject: 'Strength', player1: 70, player2: 85 },
  { subject: 'Accuracy', player1: 90, player2: 75 },
  { subject: 'Stamina', player1: 85, player2: 70 },
  { subject: 'Agility', player1: 75, player2: 90 },
]

const config = {
  player1: { label: 'Player 1', color: 'hsl(var(--primary))' },
  player2: { label: 'Player 2', color: 'hsl(var(--secondary))' },
}

export default function Example() {
  return (
    <RadarChart
      data={data}
      dataKeys={['player1', 'player2']}
      config={config}
    />
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/radar-chart.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'
import {
  Radar,
  RadarChart as RechartsRadarChart,
  PolarGrid,
  PolarAngleAxis,
  PolarRadiusAxis,
} from 'recharts'
import { ChartContainer } from './chart'
import { ChartEmpty } from './chart'
import { ChartTooltip, ChartTooltipContent } from './chart'
import { ChartLegend, ChartLegendContent } from './chart'
import type { ChartConfig } from './chart'

export interface RadarChartData {
  subject: string
  [key: string]: number | string
}

export interface RadarChartProps extends React.HTMLAttributes<HTMLDivElement> {
  data: RadarChartData[]
  dataKeys: string[]
  config: ChartConfig
  variant?: 'default' | 'filled' | 'outlined'
  showLegend?: boolean
  showGrid?: boolean
  showTooltip?: boolean
  fillOpacity?: number
  animated?: boolean
  emptyState?: React.ReactNode
}

const RadarChartComponent = React.forwardRef<HTMLDivElement, RadarChartProps>(
  (
    {
      data,
      dataKeys,
      config,
      variant = 'default',
      showLegend = true,
      showGrid = true,
      showTooltip = true,
      fillOpacity = 0.6,
      animated = true,
      emptyState,
      className,
      ...props
    },
    ref
  ) => {
    if (!data || data.length === 0 || !dataKeys || dataKeys.length === 0) {
      return <ChartEmpty ref={ref} message={emptyState} className={className} {...props} />
    }

    const getRadarProps = (index: number, key: string) => {
      const baseColor = config[key]?.color || `hsl(var(--chart-${(index % 5) + 1}))`

      switch (variant) {
        case 'outlined':
          return {
            fill: 'transparent',
            fillOpacity: 0,
            stroke: baseColor,
            strokeWidth: 3,
          }
        case 'filled':
          return {
            fill: baseColor,
            fillOpacity: fillOpacity,
            stroke: 'hsl(var(--foreground))',
            strokeWidth: 3,
          }
        default:
          return {
            fill: baseColor,
            fillOpacity: fillOpacity,
            stroke: 'hsl(var(--foreground))',
            strokeWidth: 3,
          }
      }
    }

    return (
      <ChartContainer
        ref={ref}
        config={config}
        className={cn('aspect-square max-h-[300px]', className)}
        {...props}
      >
        <RechartsRadarChart cx="50%" cy="50%" outerRadius="80%" data={data}>
          {showGrid && (
            <PolarGrid
              stroke="hsl(var(--foreground))"
              strokeWidth={2}
              strokeDasharray="none"
            />
          )}
          <PolarAngleAxis
            dataKey="subject"
            tick={{
              fill: 'hsl(var(--foreground))',
              fontWeight: 700,
              fontSize: 12,
            }}
            stroke="hsl(var(--foreground))"
            strokeWidth={2}
          />
          <PolarRadiusAxis
            angle={90}
            domain={[0, 'dataMax']}
            tick={{
              fill: 'hsl(var(--muted-foreground))',
              fontSize: 10,
            }}
            axisLine={false}
          />
          {showTooltip && (
            <ChartTooltip
              cursor={false}
              content={<ChartTooltipContent />}
            />
          )}
          {dataKeys.map((key, index) => (
            <Radar
              key={key}
              name={typeof config[key]?.label === 'string' ? config[key].label : key}
              dataKey={key}
              {...getRadarProps(index, key)}
              isAnimationActive={animated}
              animationDuration={400}
              dot={{
                r: 4,
                fill: config[key]?.color || `hsl(var(--chart-${(index % 5) + 1}))`,
                stroke: 'hsl(var(--foreground))',
                strokeWidth: 2,
              }}
            />
          ))}
          {showLegend && (
            <ChartLegend content={<ChartLegendContent />} />
          )}
        </RechartsRadarChart>
      </ChartContainer>
    )
  }
)
RadarChartComponent.displayName = 'RadarChart'

export { RadarChartComponent as RadarChart }
```

---

### Radial Bar Chart <a name="radial-bar-chart"></a>

Circular progress bars with nested rings for multiple metrics

- **Registry Name**: `@boldkit/radial-bar-chart`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/radial-bar-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/chart`

#### How to Use

##### File: `registry/default/example/radial-bar-chart-demo.tsx`

```tsx
import { RadialBarChart } from '@/components/ui/radial-bar-chart'

const data = [
  { name: 'Progress', value: 75, fill: 'hsl(var(--primary))' },
  { name: 'Goals', value: 60, fill: 'hsl(var(--secondary))' },
  { name: 'Tasks', value: 45, fill: 'hsl(var(--accent))' },
]

const config = {
  progress: { label: 'Progress', color: 'hsl(var(--primary))' },
  goals: { label: 'Goals', color: 'hsl(var(--secondary))' },
  tasks: { label: 'Tasks', color: 'hsl(var(--accent))' },
}

export default function Example() {
  return <RadialBarChart data={data} config={config} />
}
```

#### Component Source Code

##### File: `registry/default/ui/radial-bar-chart.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'
import {
  RadialBarChart as RechartsRadialBarChart,
  RadialBar,
  PolarAngleAxis,
} from 'recharts'
import { ChartContainer } from './chart'
import { ChartEmpty } from './chart'
import { ChartTooltip, ChartTooltipContent } from './chart'
import { ChartLegend, ChartLegendContent } from './chart'
import type { ChartConfig } from './chart'

export interface RadialBarChartData {
  name: string
  value: number
  fill?: string
}

export interface RadialBarChartProps extends React.HTMLAttributes<HTMLDivElement> {
  data: RadialBarChartData[]
  config: ChartConfig
  variant?: 'default' | 'stacked' | 'nested'
  innerRadius?: string | number
  outerRadius?: string | number
  showLabel?: boolean
  showBackground?: boolean
  showLegend?: boolean
  showTooltip?: boolean
  startAngle?: number
  endAngle?: number
  animated?: boolean
  maxValue?: number
  emptyState?: React.ReactNode
}

const RadialBarChartComponent = React.forwardRef<HTMLDivElement, RadialBarChartProps>(
  (
    {
      data,
      config,
      variant = 'default',
      innerRadius = '30%',
      outerRadius = '100%',
      showLabel = true,
      showBackground = true,
      showLegend = false,
      showTooltip = true,
      startAngle = 90,
      endAngle = -270,
      animated = true,
      maxValue,
      emptyState,
      className,
      ...props
    },
    ref
  ) => {
    if (!data || data.length === 0) {
      return <ChartEmpty ref={ref} message={emptyState} className={className} {...props} />
    }

    // Calculate max value for the scale
    const calculatedMax = maxValue || (data.length > 0 ? Math.max(...data.map((d) => d.value)) : 1)

    // Assign a chart color to each item that does not supply its own fill
    const chartData = React.useMemo(() => {
      return data.map((item, index) => ({
        ...item,
        fill: item.fill || `hsl(var(--chart-${(index % 5) + 1}))`,
      }))
    }, [data])

    return (
      <ChartContainer
        ref={ref}
        config={config}
        className={cn('aspect-square max-h-[300px]', className)}
        {...props}
      >
        <RechartsRadialBarChart
          cx="50%"
          cy="50%"
          innerRadius={innerRadius}
          outerRadius={outerRadius}
          data={chartData}
          startAngle={startAngle}
          endAngle={endAngle}
          barSize={variant === 'nested' ? 15 : 20}
        >
          <PolarAngleAxis
            type="number"
            domain={[0, calculatedMax]}
            angleAxisId={0}
            tick={false}
            axisLine={false}
          />
          {showTooltip && (
            <ChartTooltip
              cursor={false}
              content={<ChartTooltipContent hideLabel nameKey="name" />}
            />
          )}
          <RadialBar
            dataKey="value"
            background={showBackground ? { fill: 'hsl(var(--muted))' } : undefined}
            cornerRadius={0}
            isAnimationActive={animated}
            animationDuration={400}
            stackId={variant === 'stacked' ? 'stack' : undefined}
            label={
              showLabel
                ? {
                    position: 'insideStart',
                    fill: 'hsl(var(--foreground))',
                    fontWeight: 700,
                    fontSize: 12,
                  }
                : false
            }
          />
          {showLegend && (
            <ChartLegend
              content={<ChartLegendContent nameKey="name" />}
              verticalAlign="bottom"
            />
          )}
        </RechartsRadialBarChart>
      </ChartContainer>
    )
  }
)
RadialBarChartComponent.displayName = 'RadialBarChart'

export { RadialBarChartComponent as RadialBarChart }
```

---

### Radio Group <a name="radio-group"></a>

A set of checkable buttons where only one can be checked at a time

- **Registry Name**: `@boldkit/radio-group`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/radio-group.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-radio-group`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/label`

#### How to Use

##### File: `registry/default/example/radio-group-demo.tsx`

```tsx
import { RadioGroup, RadioGroupItem } from '@/components/ui/radio-group'
import { Label } from '@/components/ui/label'

export default function Example() {
  return (
    <RadioGroup defaultValue="option-one">
      <div className="flex items-center space-x-2">
        <RadioGroupItem value="option-one" id="option-one" />
        <Label htmlFor="option-one">Option One</Label>
      </div>
      <div className="flex items-center space-x-2">
        <RadioGroupItem value="option-two" id="option-two" />
        <Label htmlFor="option-two">Option Two</Label>
      </div>
    </RadioGroup>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/radio-group.tsx`

```tsx
import * as React from 'react'
import * as RadioGroupPrimitive from '@radix-ui/react-radio-group'
import { Circle } from 'lucide-react'
import { cn } from '@/lib/utils'

const RadioGroup = React.forwardRef<
  React.ElementRef<typeof RadioGroupPrimitive.Root>,
  React.ComponentPropsWithoutRef<typeof RadioGroupPrimitive.Root>
>(({ className, ...props }, ref) => {
  return (
    <RadioGroupPrimitive.Root
      className={cn('grid gap-2', className)}
      {...props}
      ref={ref}
    />
  )
})
RadioGroup.displayName = RadioGroupPrimitive.Root.displayName

const RadioGroupItem = React.forwardRef<
  React.ElementRef<typeof RadioGroupPrimitive.Item>,
  React.ComponentPropsWithoutRef<typeof RadioGroupPrimitive.Item>
>(({ className, ...props }, ref) => {
  return (
    <RadioGroupPrimitive.Item
      ref={ref}
      className={cn(
        'aspect-square h-5 w-5 rounded-full border-3 border-foreground bg-background shadow-[4px_4px_0px_hsl(var(--shadow-color))] transition-all duration-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none active:translate-x-[4px] active:translate-y-[4px] active:shadow-none data-[state=checked]:bg-primary',
        className
      )}
      {...props}
    >
      <RadioGroupPrimitive.Indicator className="flex items-center justify-center">
        <Circle className="h-2.5 w-2.5 fill-primary stroke-none" />
      </RadioGroupPrimitive.Indicator>
    </RadioGroupPrimitive.Item>
  )
})
RadioGroupItem.displayName = RadioGroupPrimitive.Item.displayName

export { RadioGroup, RadioGroupItem }
```

---

### Rating <a name="rating"></a>

Star rating component with half-values, multiple icons, and keyboard navigation

- **Registry Name**: `@boldkit/rating`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/rating.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/rating-demo.tsx`

```tsx
import { Rating } from '@/components/ui/rating'

export default function Example() {
  return <Rating defaultValue={3} />
}
```

#### Component Source Code

##### File: `registry/default/ui/rating.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'

// ── Icons ────────────────────────────────────────────────────────────────────

function StarIcon({ filled, half }: { filled: boolean; half?: boolean }) {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 24 24"
      aria-hidden="true"
    >
      {half ? (
        <>
          <defs>
            <linearGradient id="half-star">
              <stop offset="50%" stopColor="currentColor" />
              <stop offset="50%" stopColor="transparent" />
            </linearGradient>
          </defs>
          <path
            d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"
            fill="url(#half-star)"
            stroke="currentColor"
            strokeWidth="2"
            strokeLinecap="round"
            strokeLinejoin="round"
          />
        </>
      ) : (
        <path
          d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"
          fill={filled ? 'currentColor' : 'none'}
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
        />
      )}
    </svg>
  )
}

function HeartIcon({ filled }: { filled: boolean }) {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 24 24"
      aria-hidden="true"
    >
      <path
        d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"
        fill={filled ? 'currentColor' : 'none'}
        stroke="currentColor"
        strokeWidth="2"
        strokeLinecap="round"
        strokeLinejoin="round"
      />
    </svg>
  )
}

function CircleIcon({ filled }: { filled: boolean }) {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 24 24"
      aria-hidden="true"
    >
      <circle
        cx="12"
        cy="12"
        r="10"
        fill={filled ? 'currentColor' : 'none'}
        stroke="currentColor"
        strokeWidth="2"
      />
    </svg>
  )
}

// ── Types ────────────────────────────────────────────────────────────────────

type IconType = 'star' | 'heart' | 'circle'
type SizeVariant = 'sm' | 'md' | 'lg' | 'xl'

interface RatingProps {
  /** Controlled value */
  value?: number
  /** Uncontrolled default value */
  defaultValue?: number
  /** Maximum number of icons (default 5) */
  max?: number
  /** Precision: 1 for whole, 0.5 for half stars */
  precision?: number
  /** Icon type */
  icon?: IconType
  /** Size variant */
  size?: SizeVariant
  /** Disable interactions */
  readOnly?: boolean
  /** Disable completely */
  disabled?: boolean
  /** Called when value changes */
  onChange?: (value: number) => void
  /** Called when hover value changes (null = mouse left) */
  onHoverChange?: (value: number | null) => void
  className?: string
}

const sizeClasses: Record<SizeVariant, string> = {
  sm: '[&_svg]:h-4 [&_svg]:w-4',
  md: '[&_svg]:h-5 [&_svg]:w-5',
  lg: '[&_svg]:h-6 [&_svg]:w-6',
  xl: '[&_svg]:h-8 [&_svg]:w-8',
}

const iconLabel: Record<IconType, string> = {
  star: 'stars',
  heart: 'hearts',
  circle: 'circles',
}

// ── Component ────────────────────────────────────────────────────────────────

export function Rating({
  value: controlledValue,
  defaultValue = 0,
  max = 5,
  precision = 1,
  icon = 'star',
  size = 'md',
  readOnly = false,
  disabled = false,
  onChange,
  onHoverChange,
  className,
}: RatingProps) {
  const isControlled = controlledValue !== undefined
  const [internalValue, setInternalValue] = React.useState(defaultValue)

  const currentValue = isControlled ? controlledValue! : internalValue

  const setValue = (next: number) => {
    if (!isControlled) setInternalValue(next)
    onChange?.(next)
  }

  const interactive = !readOnly && !disabled

  const handleKeyDown = (e: React.KeyboardEvent) => {
    if (!interactive) return
    const step = precision
    let next = currentValue
    if (e.key === 'ArrowRight' || e.key === 'ArrowUp') {
      next = Math.min(max, currentValue + step)
      e.preventDefault()
    } else if (e.key === 'ArrowLeft' || e.key === 'ArrowDown') {
      next = Math.max(0, currentValue - step)
      e.preventDefault()
    } else if (e.key === 'Home') {
      next = 0
      e.preventDefault()
    } else if (e.key === 'End') {
      next = max
      e.preventDefault()
    } else {
      return
    }
    setValue(next)
  }

  const handleMouseLeave = () => {
    onHoverChange?.(null)
  }

  const handleStarMouseMove = (_e: React.MouseEvent<HTMLButtonElement>, starIndex: number) => {
    if (!interactive) return
    onHoverChange?.(starIndex)
  }

  const handleStarClick = (starIndex: number) => {
    if (!interactive) return
    setValue(starIndex)
  }

  const valueText =
    icon === 'heart'
      ? `${currentValue} out of ${max} hearts`
      : icon === 'circle'
        ? `${currentValue} out of ${max} circles`
        : `${currentValue} out of ${max} stars`

  function renderIcon(index: number) {
    const filled = index <= currentValue
    const half = !filled && index - 0.5 <= currentValue && precision === 0.5
    if (icon === 'heart') return <HeartIcon filled={filled} />
    if (icon === 'circle') return <CircleIcon filled={filled} />
    return <StarIcon filled={filled} half={half} />
  }

  return (
    <div
      role="slider"
      aria-label="Rating"
      aria-valuemin={0}
      aria-valuemax={max}
      aria-valuenow={currentValue}
      aria-valuetext={valueText}
      tabIndex={disabled || readOnly ? -1 : 0}
      onKeyDown={handleKeyDown}
      onMouseLeave={handleMouseLeave}
      className={cn(
        'flex items-center gap-0.5 outline-none',
        sizeClasses[size],
        disabled && 'opacity-50 pointer-events-none',
        className
      )}
    >
      {Array.from({ length: max }, (_, i) => {
        const starIndex = i + 1
        const isSelected = starIndex === Math.ceil(currentValue)
        const isFocusable = interactive && (currentValue === 0 ? starIndex === 1 : isSelected)
        return (
          <button
            key={starIndex}
            type="button"
            tabIndex={isFocusable ? 0 : -1}
            disabled={disabled}
            aria-label={`${starIndex} ${iconLabel[icon]}`}
            onClick={() => handleStarClick(starIndex)}
            onMouseMove={(e) => handleStarMouseMove(e, starIndex)}
            className={cn(
              'flex items-center justify-center transition-transform focus:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2',
              interactive && 'hover:scale-110 cursor-pointer',
              !interactive && 'cursor-default'
            )}
          >
            {renderIcon(starIndex)}
          </button>
        )
      })}
    </div>
  )
}
```

---

### Sankey Chart <a name="sankey-chart"></a>

Sankey flow diagram for showing proportional flows between nodes - custom SVG implementation

- **Registry Name**: `@boldkit/sankey-chart`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/sankey-chart.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/chart`

#### How to Use

##### File: `registry/default/example/sankey-chart-demo.tsx`

```tsx
import { SankeyChart } from "@/components/ui/sankey-chart"

export default function SankeyChartDemo() {
  return (
    <SankeyChart
      nodes={[
        { name: "Source A" },
        { name: "Source B" },
        { name: "Target X" },
        { name: "Target Y" },
      ]}
      links={[
        { source: 0, target: 2, value: 5 },
        { source: 0, target: 3, value: 3 },
        { source: 1, target: 2, value: 2 },
        { source: 1, target: 3, value: 7 },
      ]}
    />
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/sankey-chart.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'
import { ChartEmpty } from './chart'

export interface SankeyNode {
  id: string
  label: string
  color?: string
}

export interface SankeyLink {
  source: string
  target: string
  value: number
}

export interface SankeyChartProps extends React.HTMLAttributes<HTMLDivElement> {
  nodes: SankeyNode[]
  links: SankeyLink[]
  height?: number
  showTooltip?: boolean
  showLabels?: boolean
  /** Accessible label for screen readers (default: "Sankey chart") */
  ariaLabel?: string
  emptyState?: React.ReactNode
}

const NODE_COLORS = [
  'hsl(var(--primary))',
  'hsl(var(--secondary))',
  'hsl(var(--accent))',
  'hsl(var(--success))',
  'hsl(var(--info))',
  'hsl(var(--warning))',
]

interface ComputedNode {
  id: string
  label: string
  color: string
  x: number
  y: number
  width: number
  height: number
  value: number
}

interface ComputedLink {
  source: ComputedNode
  target: ComputedNode
  value: number
  sourceY: number
  targetY: number
  thickness: number
  path: string
  color: string
}

function computeLayout(
  nodes: SankeyNode[],
  links: SankeyLink[],
  width: number,
  height: number
): { computedNodes: ComputedNode[]; computedLinks: ComputedLink[] } {
  const padding = { top: 16, right: 120, bottom: 16, left: 120 }
  const nodeWidth = 16
  const nodePadding = 10

  // Build adjacency
  const nodeMap = new Map<string, SankeyNode & { index: number }>()
  nodes.forEach((n, i) => nodeMap.set(n.id, { ...n, index: i }))

  // Determine columns (depth) via BFS
  const depth = new Map<string, number>()
  const inLinks = new Map<string, SankeyLink[]>()
  const outLinks = new Map<string, SankeyLink[]>()
  nodes.forEach(n => { inLinks.set(n.id, []); outLinks.set(n.id, []) })
  links.forEach(l => {
    outLinks.get(l.source)?.push(l)
    inLinks.get(l.target)?.push(l)
  })

  const sources = nodes.filter(n => (inLinks.get(n.id) || []).length === 0)
  const queue = sources.map(n => n.id)
  queue.forEach(id => depth.set(id, 0))
  while (queue.length > 0) {
    const id = queue.shift()!
    const d = depth.get(id) ?? 0
    for (const link of outLinks.get(id) || []) {
      if (!depth.has(link.target)) {
        depth.set(link.target, d + 1)
        queue.push(link.target)
      }
    }
  }

  // Warn about nodes dropped due to cycles (nodes not reachable from any source)
  if (process.env.NODE_ENV === 'development') {
    const droppedNodes = nodes.filter(n => !depth.has(n.id))
    if (droppedNodes.length > 0) {
      console.warn(
        `[SankeyChart] ${droppedNodes.length} node(s) were dropped because they are part of a cycle or unreachable from source nodes: ${droppedNodes.map(n => n.id).join(', ')}`
      )
    }
  }

  const maxDepth = Math.max(...Array.from(depth.values()))
  const colWidth = maxDepth === 0 ? 0 : (width - padding.left - padding.right - nodeWidth) / maxDepth

  // Group nodes by column
  const cols = new Map<number, string[]>()
  depth.forEach((d, id) => {
    if (!cols.has(d)) cols.set(d, [])
    const col = cols.get(d)
    if (col) col.push(id)
  })

  // Node values = sum of in-links (or out-links for sources)
  const nodeValue = new Map<string, number>()
  nodes.forEach(n => {
    const inVal = (inLinks.get(n.id) || []).reduce((s, l) => s + l.value, 0)
    const outVal = (outLinks.get(n.id) || []).reduce((s, l) => s + l.value, 0)
    nodeValue.set(n.id, Math.max(inVal, outVal, 1))
  })

  const innerHeight = height - padding.top - padding.bottom

  // Compute node heights and y positions per column
  const computedNodes = new Map<string, ComputedNode>()
  cols.forEach((ids, col) => {
    const colTotal = ids.reduce((s, id) => s + (nodeValue.get(id) || 1), 0)
    const totalNodeHeight = innerHeight - nodePadding * (ids.length - 1)
    let yOffset = padding.top

    ids.forEach((id) => {
      const val = nodeValue.get(id) || 1
      const h = Math.max(8, (val / colTotal) * totalNodeHeight)
      const node = nodeMap.get(id)!
      computedNodes.set(id, {
        id,
        label: node.label,
        color: node.color || NODE_COLORS[node.index % NODE_COLORS.length],
        x: padding.left + col * colWidth,
        y: yOffset,
        width: nodeWidth,
        height: h,
        value: val,
      })
      yOffset += h + nodePadding
    })
  })

  // Track current link offsets per node side
  const sourceOffsets = new Map<string, number>()
  const targetOffsets = new Map<string, number>()
  computedNodes.forEach((n, id) => {
    sourceOffsets.set(id, n.y)
    targetOffsets.set(id, n.y)
  })

  const computedLinks: ComputedLink[] = links.map(link => {
    const src = computedNodes.get(link.source)
    const tgt = computedNodes.get(link.target)
    if (!src || !tgt) return null

    const srcTotal = (outLinks.get(link.source) || []).reduce((s, l) => s + l.value, 0)
    const tgtTotal = (inLinks.get(link.target) || []).reduce((s, l) => s + l.value, 0)
    const thickness = Math.max(2, (link.value / Math.max(srcTotal, tgtTotal)) * src.height)

    const sY = sourceOffsets.get(link.source) ?? 0
    const tY = targetOffsets.get(link.target) ?? 0
    sourceOffsets.set(link.source, sY + thickness)
    targetOffsets.set(link.target, tY + thickness)

    const x1 = src.x + src.width
    const x2 = tgt.x
    const mx = (x1 + x2) / 2

    const path = `M${x1},${sY} C${mx},${sY} ${mx},${tY} ${x2},${tY} L${x2},${tY + thickness} C${mx},${tY + thickness} ${mx},${sY + thickness} ${x1},${sY + thickness} Z`

    return {
      source: src,
      target: tgt,
      value: link.value,
      sourceY: sY,
      targetY: tY,
      thickness,
      path,
      color: src.color,
    }
  }).filter(Boolean) as ComputedLink[]

  return { computedNodes: Array.from(computedNodes.values()), computedLinks }
}

const SankeyChart = React.forwardRef<HTMLDivElement, SankeyChartProps>(
  (
    {
      nodes,
      links,
      height = 320,
      showTooltip = true,
      showLabels = true,
      ariaLabel = 'Sankey chart',
      emptyState,
      className,
      ...props
    },
    ref
  ) => {
    const isEmpty = !nodes || nodes.length === 0 || !links || links.length === 0
    const containerRef = React.useRef<HTMLDivElement>(null)
    const [width, setWidth] = React.useState(600)
    const [tooltip, setTooltip] = React.useState<{ x: number; y: number; label: string; value: number } | null>(null)

    // useLayoutEffect fires before paint, preventing the initial layout flash
    // that would occur with useEffect + the 600px placeholder width.
    // SankeyChart is client-only (ResizeObserver), so useLayoutEffect is safe here.
    const mountedRef = React.useRef(true)
    React.useLayoutEffect(() => {
      mountedRef.current = true
      if (!containerRef.current) return
      const ro = new ResizeObserver(entries => {
        if (mountedRef.current) {
          setWidth(entries[0].contentRect.width)
        }
      })
      ro.observe(containerRef.current)
      setWidth(containerRef.current.offsetWidth)
      return () => {
        mountedRef.current = false
        ro.disconnect()
      }
    }, [])

    const { computedNodes, computedLinks } = React.useMemo(
      () => computeLayout(nodes, links, width, height),
      [nodes, links, width, height]
    )

    if (isEmpty) {
      return <ChartEmpty ref={ref} message={emptyState} className={className} {...props} />
    }

    return (
      <div
        ref={ref}
        role="img"
        aria-label={ariaLabel}
        className={cn('relative w-full', className)}
        {...props}
      >
        <div ref={containerRef} style={{ height }}>
          <svg width="100%" height={height}>
            {/* Links */}
            {computedLinks.map((link, i) => (
              <path
                key={i}
                d={link.path}
                fill={link.color}
                fillOpacity={0.45}
                stroke="hsl(var(--foreground))"
                strokeWidth={1}
                onMouseEnter={(e) => {
                  if (showTooltip) {
                    setTooltip({
                      x: e.clientX,
                      y: e.clientY,
                      label: `${link.source.label} → ${link.target.label}`,
                      value: link.value,
                    })
                  }
                }}
                onMouseLeave={() => setTooltip(null)}
                style={{ cursor: 'default', transition: 'fill-opacity 100ms' }}
                className="hover:fill-opacity-70"
              />
            ))}

            {/* Nodes */}
            {computedNodes.map(node => (
              <g key={node.id}>
                <rect
                  x={node.x}
                  y={node.y}
                  width={node.width}
                  height={node.height}
                  fill={node.color}
                  stroke="hsl(var(--foreground))"
                  strokeWidth={3}
                />
                {showLabels && (
                  <text
                    x={node.x > width / 2 ? node.x - 6 : node.x + node.width + 6}
                    y={node.y + node.height / 2}
                    dominantBaseline="middle"
                    textAnchor={node.x > width / 2 ? 'end' : 'start'}
                    fill="hsl(var(--foreground))"
                    style={{ fontSize: 11, fontFamily: "'DM Mono', monospace", fontWeight: 700 }}
                  >
                    {node.label}
                  </text>
                )}
              </g>
            ))}
          </svg>
        </div>

        {/* Tooltip */}
        {showTooltip && tooltip && (
          <div
            className="fixed z-50 pointer-events-none border-3 border-foreground bg-background px-3 py-2 text-xs font-mono shadow-[4px_4px_0px_hsl(var(--foreground))]"
            style={{ left: tooltip.x + 12, top: tooltip.y - 32 }}
          >
            <p className="font-black">{tooltip.label}</p>
            <p className="text-muted-foreground">{tooltip.value.toLocaleString()}</p>
          </div>
        )}
      </div>
    )
  }
)
SankeyChart.displayName = 'SankeyChart'

export { SankeyChart }
```

---

### Scroll Area <a name="scroll-area"></a>

Augments native scroll functionality for custom styling

- **Registry Name**: `@boldkit/scroll-area`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/scroll-area.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-scroll-area`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/scroll-area-demo.tsx`

```tsx
import { ScrollArea } from '@/components/ui/scroll-area'

export default function Example() {
  return (
    <ScrollArea className="h-[200px] w-[350px] border-3 border-foreground p-4">
      <div>
        {/* Long content here */}
      </div>
    </ScrollArea>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/scroll-area.tsx`

```tsx
import * as React from 'react'
import * as ScrollAreaPrimitive from '@radix-ui/react-scroll-area'
import { cn } from '@/lib/utils'

const ScrollArea = React.forwardRef<
  React.ElementRef<typeof ScrollAreaPrimitive.Root>,
  React.ComponentPropsWithoutRef<typeof ScrollAreaPrimitive.Root>
>(({ className, children, ...props }, ref) => (
  <ScrollAreaPrimitive.Root
    ref={ref}
    className={cn('relative overflow-hidden', className)}
    {...props}
  >
    <ScrollAreaPrimitive.Viewport className="h-full w-full rounded-[inherit]">
      {children}
    </ScrollAreaPrimitive.Viewport>
    <ScrollBar />
    <ScrollAreaPrimitive.Corner />
  </ScrollAreaPrimitive.Root>
))
ScrollArea.displayName = ScrollAreaPrimitive.Root.displayName

const ScrollBar = React.forwardRef<
  React.ElementRef<typeof ScrollAreaPrimitive.ScrollAreaScrollbar>,
  React.ComponentPropsWithoutRef<typeof ScrollAreaPrimitive.ScrollAreaScrollbar>
>(({ className, orientation = 'vertical', ...props }, ref) => (
  <ScrollAreaPrimitive.ScrollAreaScrollbar
    ref={ref}
    orientation={orientation}
    className={cn(
      'flex touch-none select-none transition-colors',
      orientation === 'vertical' &&
        'h-full w-3 border-l-3 border-l-transparent p-[1px]',
      orientation === 'horizontal' &&
        'h-3 flex-col border-t-3 border-t-transparent p-[1px]',
      className
    )}
    {...props}
  >
    <ScrollAreaPrimitive.ScrollAreaThumb className="relative flex-1 bg-foreground" />
  </ScrollAreaPrimitive.ScrollAreaScrollbar>
))
ScrollBar.displayName = ScrollAreaPrimitive.ScrollAreaScrollbar.displayName

export { ScrollArea, ScrollBar }
```

---

### Select <a name="select"></a>

Displays a list of options with neubrutalism styling

- **Registry Name**: `@boldkit/select`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/select.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-select`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/select-demo.tsx`

```tsx
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'

export default function Example() {
  return (
    <Select>
      <SelectTrigger className="w-[180px]">
        <SelectValue placeholder="Select a fruit" />
      </SelectTrigger>
      <SelectContent>
        <SelectItem value="apple">Apple</SelectItem>
        <SelectItem value="banana">Banana</SelectItem>
        <SelectItem value="orange">Orange</SelectItem>
      </SelectContent>
    </Select>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/select.tsx`

```tsx
import * as React from 'react'
import * as SelectPrimitive from '@radix-ui/react-select'
import { Check, ChevronDown, ChevronUp } from 'lucide-react'
import { cn } from '@/lib/utils'

const Select = SelectPrimitive.Root

const SelectGroup = SelectPrimitive.Group

const SelectValue = SelectPrimitive.Value

const SelectTrigger = React.forwardRef<
  React.ElementRef<typeof SelectPrimitive.Trigger>,
  React.ComponentPropsWithoutRef<typeof SelectPrimitive.Trigger>
>(({ className, children, ...props }, ref) => (
  <SelectPrimitive.Trigger
    ref={ref}
    className={cn(
      'flex h-11 w-full items-center justify-between border-3 border-input bg-background px-4 py-2 text-sm font-medium shadow-[4px_4px_0px_hsl(var(--shadow-color))] placeholder:text-muted-foreground focus:outline-none focus:translate-x-[4px] focus:translate-y-[4px] focus:shadow-none disabled:cursor-not-allowed disabled:opacity-50 [&>span]:line-clamp-1 transition-all duration-200',
      className
    )}
    {...props}
  >
    {children}
    <SelectPrimitive.Icon asChild>
      <ChevronDown className="h-5 w-5 stroke-[3]" />
    </SelectPrimitive.Icon>
  </SelectPrimitive.Trigger>
))
SelectTrigger.displayName = SelectPrimitive.Trigger.displayName

const SelectScrollUpButton = React.forwardRef<
  React.ElementRef<typeof SelectPrimitive.ScrollUpButton>,
  React.ComponentPropsWithoutRef<typeof SelectPrimitive.ScrollUpButton>
>(({ className, ...props }, ref) => (
  <SelectPrimitive.ScrollUpButton
    ref={ref}
    className={cn(
      'flex cursor-default items-center justify-center py-1',
      className
    )}
    {...props}
  >
    <ChevronUp className="h-4 w-4 stroke-[3]" />
  </SelectPrimitive.ScrollUpButton>
))
SelectScrollUpButton.displayName = SelectPrimitive.ScrollUpButton.displayName

const SelectScrollDownButton = React.forwardRef<
  React.ElementRef<typeof SelectPrimitive.ScrollDownButton>,
  React.ComponentPropsWithoutRef<typeof SelectPrimitive.ScrollDownButton>
>(({ className, ...props }, ref) => (
  <SelectPrimitive.ScrollDownButton
    ref={ref}
    className={cn(
      'flex cursor-default items-center justify-center py-1',
      className
    )}
    {...props}
  >
    <ChevronDown className="h-4 w-4 stroke-[3]" />
  </SelectPrimitive.ScrollDownButton>
))
SelectScrollDownButton.displayName = SelectPrimitive.ScrollDownButton.displayName

const SelectContent = React.forwardRef<
  React.ElementRef<typeof SelectPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof SelectPrimitive.Content>
>(({ className, children, position = 'popper', ...props }, ref) => (
  <SelectPrimitive.Portal>
    <SelectPrimitive.Content
      ref={ref}
      className={cn(
        'relative z-50 max-h-96 min-w-[8rem] overflow-hidden border-3 border-foreground bg-popover text-popover-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        position === 'popper' &&
          'data-[side=bottom]:translate-y-1 data-[side=left]:-translate-x-1 data-[side=right]:translate-x-1 data-[side=top]:-translate-y-1',
        className
      )}
      position={position}
      {...props}
    >
      <SelectScrollUpButton />
      <SelectPrimitive.Viewport
        className={cn(
          'p-1',
          position === 'popper' &&
            'h-[var(--radix-select-trigger-height)] w-full min-w-[var(--radix-select-trigger-width)]'
        )}
      >
        {children}
      </SelectPrimitive.Viewport>
      <SelectScrollDownButton />
    </SelectPrimitive.Content>
  </SelectPrimitive.Portal>
))
SelectContent.displayName = SelectPrimitive.Content.displayName

const SelectLabel = React.forwardRef<
  React.ElementRef<typeof SelectPrimitive.Label>,
  React.ComponentPropsWithoutRef<typeof SelectPrimitive.Label>
>(({ className, ...props }, ref) => (
  <SelectPrimitive.Label
    ref={ref}
    className={cn('py-1.5 pl-8 pr-2 text-sm font-bold uppercase tracking-wide', className)}
    {...props}
  />
))
SelectLabel.displayName = SelectPrimitive.Label.displayName

const SelectItem = React.forwardRef<
  React.ElementRef<typeof SelectPrimitive.Item>,
  React.ComponentPropsWithoutRef<typeof SelectPrimitive.Item>
>(({ className, children, ...props }, ref) => (
  <SelectPrimitive.Item
    ref={ref}
    className={cn(
      'relative flex w-full cursor-default select-none items-center py-2 pl-8 pr-2 text-sm outline-none transition-all duration-150 focus:bg-accent focus:text-accent-foreground focus:translate-x-1 data-[disabled]:pointer-events-none data-[disabled]:opacity-50',
      className
    )}
    {...props}
  >
    <span className="absolute left-2 flex h-3.5 w-3.5 items-center justify-center">
      <SelectPrimitive.ItemIndicator>
        <Check className="h-4 w-4 stroke-[3]" />
      </SelectPrimitive.ItemIndicator>
    </span>
    <SelectPrimitive.ItemText>{children}</SelectPrimitive.ItemText>
  </SelectPrimitive.Item>
))
SelectItem.displayName = SelectPrimitive.Item.displayName

const SelectSeparator = React.forwardRef<
  React.ElementRef<typeof SelectPrimitive.Separator>,
  React.ComponentPropsWithoutRef<typeof SelectPrimitive.Separator>
>(({ className, ...props }, ref) => (
  <SelectPrimitive.Separator
    ref={ref}
    className={cn('-mx-1 my-1 h-px bg-foreground', className)}
    {...props}
  />
))
SelectSeparator.displayName = SelectPrimitive.Separator.displayName

export {
  Select,
  SelectGroup,
  SelectValue,
  SelectTrigger,
  SelectContent,
  SelectLabel,
  SelectItem,
  SelectSeparator,
  SelectScrollUpButton,
  SelectScrollDownButton,
}
```

---

### Separator <a name="separator"></a>

Visually separates content with a thick border

- **Registry Name**: `@boldkit/separator`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/separator.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-separator`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/separator-demo.tsx`

```tsx
import { Separator } from '@/components/ui/separator'

export default function Example() {
  return (
    <div>
      <div className="space-y-1">
        <h4 className="text-sm font-bold">Radix Primitives</h4>
        <p className="text-sm text-muted-foreground">
          An open-source UI component library.
        </p>
      </div>
      <Separator className="my-4" />
      <div className="text-sm">More content here</div>
    </div>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/separator.tsx`

```tsx
import * as React from 'react'
import * as SeparatorPrimitive from '@radix-ui/react-separator'
import { cn } from '@/lib/utils'

const Separator = React.forwardRef<
  React.ElementRef<typeof SeparatorPrimitive.Root>,
  React.ComponentPropsWithoutRef<typeof SeparatorPrimitive.Root>
>(
  (
    { className, orientation = 'horizontal', decorative = true, ...props },
    ref
  ) => (
    <SeparatorPrimitive.Root
      ref={ref}
      decorative={decorative}
      orientation={orientation}
      className={cn(
        'shrink-0 bg-foreground',
        orientation === 'horizontal' ? 'h-[3px] w-full' : 'h-full w-[3px]',
        className
      )}
      {...props}
    />
  )
)
Separator.displayName = SeparatorPrimitive.Root.displayName

export { Separator }
```

---

### Shapes <a name="shapes"></a>

55 neubrutalist SVG shapes organized by category for badges, decorations, and visual accents

- **Registry Name**: `@boldkit/shapes`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/shapes.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/shapes-demo.tsx`

```tsx
import { Star5Shape, HeartShape, LightningShape } from "@/components/ui/shapes"

export default function ShapesDemo() {
  return (
    <div className="flex gap-4 items-center">
      <Star5Shape className="w-16 h-16 text-primary" />
      <HeartShape className="w-16 h-16 text-destructive" />
      <LightningShape className="w-16 h-16 text-accent" />
    </div>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/shapes.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cn } from '@/lib/utils'

// ============================================================================
// Types & Factory
// ============================================================================

type ShapeAnimation = 'none' | 'spin' | 'pulse' | 'float' | 'wiggle' | 'bounce' | 'glitch'
type ShapeSpeed = 'slow' | 'normal' | 'fast'

interface ShapeProps extends React.SVGProps<SVGSVGElement> {
  size?: number
  strokeWidth?: number
  filled?: boolean
  color?: string
  animation?: ShapeAnimation
  speed?: ShapeSpeed
}

function getAnimClass(animation?: ShapeAnimation, speed?: ShapeSpeed): string {
  if (!animation || animation === 'none') return ''
  const s = speed && speed !== 'normal' ? `-${speed}` : ''
  return `shape-animate-${animation}${s}`
}

// ============================================================================
// GEOMETRIC SHAPES - Basic polygons and mathematical forms
// ============================================================================

export const TriangleShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-accent', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 5 L95 90 L5 90 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
TriangleShape.displayName = 'TriangleShape'

export const DiamondBadge = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-primary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 5 L95 50 L50 95 L5 50 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
DiamondBadge.displayName = 'DiamondBadge'

export const PentagonShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-primary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 5 L95 38 L77 90 L23 90 L5 38 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
PentagonShape.displayName = 'PentagonShape'

export const HexagonShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-secondary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M25 10 L75 10 L95 50 L75 90 L25 90 L5 50 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
HexagonShape.displayName = 'HexagonShape'

export const OctagonShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-accent', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M30 5 L70 5 L95 30 L95 70 L70 95 L30 95 L5 70 L5 30 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
OctagonShape.displayName = 'OctagonShape'

export const CrossShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-accent', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M35 5 L65 5 L65 35 L95 35 L95 65 L65 65 L65 95 L35 95 L35 65 L5 65 L5 35 L35 35 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
CrossShape.displayName = 'CrossShape'

export const TrapezoidShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.6}
      viewBox="0 0 100 60"
      className={cn('text-secondary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M20 5 L80 5 L95 55 L5 55 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
TrapezoidShape.displayName = 'TrapezoidShape'

export const ParallelogramShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.5}
      viewBox="0 0 100 50"
      className={cn('text-info', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M20 5 L95 5 L80 45 L5 45 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
ParallelogramShape.displayName = 'ParallelogramShape'

// ============================================================================
// STAR SHAPES - Stars, bursts, and explosion effects
// ============================================================================

export const Star4Shape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-info', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 0 L58 42 L100 50 L58 58 L50 100 L42 58 L0 50 L42 42 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
Star4Shape.displayName = 'Star4Shape'

export const Star5Shape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-accent', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 5 L61 38 L95 38 L68 59 L79 93 L50 72 L21 93 L32 59 L5 38 L39 38 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
Star5Shape.displayName = 'Star5Shape'

export const BurstShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-primary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 0 L58 35 L95 20 L68 45 L100 50 L68 55 L95 80 L58 65 L50 100 L42 65 L5 80 L32 55 L0 50 L32 45 L5 20 L42 35 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
BurstShape.displayName = 'BurstShape'

export const ExplosionShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-warning', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 5 L55 30 L80 15 L65 35 L95 35 L70 50 L95 65 L65 65 L80 85 L55 70 L50 95 L45 70 L20 85 L35 65 L5 65 L30 50 L5 35 L35 35 L20 15 L45 30 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
ExplosionShape.displayName = 'ExplosionShape'

export const SplatShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-destructive', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 5 Q55 20 70 15 Q65 30 85 25 Q75 40 95 50 Q75 55 85 75 Q65 65 70 85 Q55 75 50 95 Q45 75 30 85 Q35 65 15 75 Q25 55 5 50 Q25 40 15 25 Q35 30 30 15 Q45 20 50 5 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
SplatShape.displayName = 'SplatShape'

export const LightningShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size * 0.6}
      height={size}
      viewBox="0 0 60 100"
      className={cn('text-warning', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M35 0 L5 55 L25 55 L15 100 L55 40 L35 40 L50 0 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
LightningShape.displayName = 'LightningShape'

// ============================================================================
// ORGANIC SHAPES - Blobs, waves, and natural forms
// ============================================================================

export const BlobShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-secondary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 5 Q80 10 90 35 Q95 60 80 80 Q60 95 40 90 Q15 85 10 60 Q5 35 25 15 Q40 5 50 5 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
BlobShape.displayName = 'BlobShape'

export const WaveShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.5}
      viewBox="0 0 100 50"
      className={cn('text-secondary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M0 25 Q12.5 0 25 25 Q37.5 50 50 25 Q62.5 0 75 25 Q87.5 50 100 25 L100 50 L0 50 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
WaveShape.displayName = 'WaveShape'

export const CloudShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.7}
      viewBox="0 0 100 70"
      className={cn('text-info', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M25 60 Q5 60 5 45 Q5 30 20 30 Q20 15 40 15 Q55 10 65 20 Q75 10 90 25 Q100 35 90 50 Q95 65 75 65 L25 65 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
CloudShape.displayName = 'CloudShape'

export const HeartShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-destructive', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 90 L15 55 Q0 40 0 25 Q0 5 20 5 Q35 5 50 20 Q65 5 80 5 Q100 5 100 25 Q100 40 85 55 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
HeartShape.displayName = 'HeartShape'

// ============================================================================
// UI & BADGE SHAPES - Tags, badges, ribbons, and UI elements
// ============================================================================

export const ArrowBadge = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.6}
      viewBox="0 0 100 60"
      className={cn('text-accent', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M0 10 L70 10 L70 0 L100 30 L70 60 L70 50 L0 50 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
ArrowBadge.displayName = 'ArrowBadge'

export const ZigzagBanner = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.5}
      viewBox="0 0 100 50"
      className={cn('text-warning', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M0 0 L100 0 L100 35 L90 50 L80 35 L70 50 L60 35 L50 50 L40 35 L30 50 L20 35 L10 50 L0 35 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
ZigzagBanner.displayName = 'ZigzagBanner'

export const RibbonShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.4}
      viewBox="0 0 100 40"
      className={cn('text-primary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M0 10 L10 0 L10 10 L90 10 L90 0 L100 10 L100 30 L90 40 L90 30 L10 30 L10 40 L0 30 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
RibbonShape.displayName = 'RibbonShape'

export const ShieldShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-success', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 5 L90 20 L90 50 Q90 80 50 95 Q10 80 10 50 L10 20 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
ShieldShape.displayName = 'ShieldShape'

export const TagShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.6}
      viewBox="0 0 100 60"
      className={cn('text-warning', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M15 5 L85 5 L100 30 L85 55 L15 55 L0 30 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
      <circle cx="20" cy="30" r="6" fill="hsl(var(--foreground))" />
    </svg>
  )
)
TagShape.displayName = 'TagShape'

export const PriceTagShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.6}
      viewBox="0 0 100 60"
      className={cn('text-accent', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M10 5 L75 5 L95 30 L75 55 L10 55 L10 5 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
      <circle cx="22" cy="30" r="8" fill="none" stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} />
      <line x1="10" y1="30" x2="14" y2="30" stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} />
    </svg>
  )
)
PriceTagShape.displayName = 'PriceTagShape'

export const TicketShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.5}
      viewBox="0 0 100 50"
      className={cn('text-success', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M5 0 L95 0 Q100 0 100 5 L100 20 Q90 20 90 25 Q90 30 100 30 L100 45 Q100 50 95 50 L5 50 Q0 50 0 45 L0 30 Q10 30 10 25 Q10 20 0 20 L0 5 Q0 0 5 0 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
TicketShape.displayName = 'TicketShape'

export const CouponShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.5}
      viewBox="0 0 100 50"
      className={cn('text-success', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M5 5 L95 5 L95 15 Q90 15 90 20 Q90 25 95 25 L95 35 Q90 35 90 40 Q90 45 95 45 L5 45 L5 35 Q10 35 10 30 Q10 25 5 25 L5 15 Q10 15 10 10 Q10 5 5 5 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
      <line x1="35" y1="10" x2="35" y2="40" stroke="hsl(var(--foreground))" strokeWidth={strokeWidth - 1} strokeDasharray="4,4" />
    </svg>
  )
)
CouponShape.displayName = 'CouponShape'

export const BookmarkShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size * 0.6}
      height={size}
      viewBox="0 0 60 100"
      className={cn('text-destructive', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M5 5 L55 5 L55 95 L30 75 L5 95 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
BookmarkShape.displayName = 'BookmarkShape'

export const FlagShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-warning', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path d="M10 5 L10 95" fill="none" stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} />
      <path
        d="M10 5 L90 5 L75 30 L90 55 L10 55 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
FlagShape.displayName = 'FlagShape'

export const PillShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.4}
      viewBox="0 0 100 40"
      className={cn('text-primary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M20 5 L80 5 Q95 5 95 20 Q95 35 80 35 L20 35 Q5 35 5 20 Q5 5 20 5 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
PillShape.displayName = 'PillShape'

// ============================================================================
// COMMUNICATION SHAPES - Speech bubbles, cursors, and interactive elements
// ============================================================================

export const SpeechBubble = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-card', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M10 10 L90 10 L90 65 L40 65 L20 90 L25 65 L10 65 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
SpeechBubble.displayName = 'SpeechBubble'

export const CursorShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size * 0.7}
      height={size}
      viewBox="0 0 70 100"
      className={cn('text-success', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M5 5 L5 85 L25 65 L45 95 L55 90 L35 60 L65 60 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
CursorShape.displayName = 'CursorShape'

export const EyeShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.6}
      viewBox="0 0 100 60"
      className={cn('text-secondary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M5 30 Q25 5 50 5 Q75 5 95 30 Q75 55 50 55 Q25 55 5 30 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
      <circle cx="50" cy="30" r="12" fill="hsl(var(--foreground))" />
    </svg>
  )
)
EyeShape.displayName = 'EyeShape'

// ============================================================================
// NATURE & CELESTIAL SHAPES - Sun, moon, planet, rainbow, and natural forms
// ============================================================================

export const SunShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-warning', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <circle
        cx="50"
        cy="50"
        r="22"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
      <path
        d="M50 5 L54 22 L46 22 Z M50 95 L54 78 L46 78 Z M5 50 L22 46 L22 54 Z M95 50 L78 46 L78 54 Z M18 18 L32 28 L28 32 Z M82 18 L68 28 L72 32 Z M18 82 L28 68 L32 72 Z M82 82 L72 68 L68 72 Z"
        fill="hsl(var(--foreground))"
        stroke="hsl(var(--foreground))"
        strokeWidth={Math.max(1, strokeWidth - 2)}
        strokeLinejoin="round"
      />
    </svg>
  )
)
SunShape.displayName = 'SunShape'

export const Star6Shape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-accent', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      {/* 6-pointed star (hexagram) with 12 points alternating */}
      <path
        d="M50 2 L56 30 L85 15 L65 40 L98 50 L65 60 L85 85 L56 70 L50 98 L44 70 L15 85 L35 60 L2 50 L35 40 L15 15 L44 30 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
Star6Shape.displayName = 'Star6Shape'

export const CrescentShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-warning', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M70 10 Q90 30 90 55 Q90 80 65 92 Q80 75 80 55 Q80 30 60 15 Q55 12 70 10 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
        strokeLinejoin="round"
      />
    </svg>
  )
)
CrescentShape.displayName = 'CrescentShape'

export const RainbowShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.6}
      viewBox="0 0 100 60"
      className={cn('text-primary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M5 55 Q5 10 50 10 Q95 10 95 55"
        fill="none"
        stroke={filled ? (color || "hsl(var(--destructive))") : 'none'}
        strokeWidth={10}
        strokeLinecap="round"
      />
      <path
        d="M15 55 Q15 22 50 22 Q85 22 85 55"
        fill="none"
        stroke={filled ? (color || "hsl(var(--warning))") : 'none'}
        strokeWidth={10}
        strokeLinecap="round"
      />
      <path
        d="M25 55 Q25 34 50 34 Q75 34 75 55"
        fill="none"
        stroke={filled ? (color || "hsl(var(--info))") : 'none'}
        strokeWidth={10}
        strokeLinecap="round"
      />
      <path
        d="M5 55 Q5 10 50 10 Q95 10 95 55"
        fill="none"
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
      />
      <path
        d="M15 55 Q15 22 50 22 Q85 22 85 55"
        fill="none"
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
      />
      <path
        d="M25 55 Q25 34 50 34 Q75 34 75 55"
        fill="none"
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
      />
    </svg>
  )
)
RainbowShape.displayName = 'RainbowShape'

export const PlanetShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-secondary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <circle
        cx="50"
        cy="50"
        r="28"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
      <ellipse
        cx="50"
        cy="50"
        rx="45"
        ry="14"
        fill="none"
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
        transform="rotate(-25 50 50)"
      />
    </svg>
  )
)
PlanetShape.displayName = 'PlanetShape'

export const UmbrellaShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-info', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 10 Q15 10 5 45 L25 45 Q25 30 35 30 Q40 30 40 45 L60 45 Q60 30 65 30 Q75 30 75 45 L95 45 Q85 10 50 10 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
      <path
        d="M50 10 L50 80 Q50 92 40 92 Q32 92 32 85"
        fill="none"
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
      />
    </svg>
  )
)
UmbrellaShape.displayName = 'UmbrellaShape'

export const AppleShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-destructive', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 25 Q30 20 15 35 Q0 55 15 75 Q25 92 40 95 Q50 97 50 90 Q50 97 60 95 Q75 92 85 75 Q100 55 85 35 Q70 20 50 25 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
      <path
        d="M50 25 Q55 10 60 5"
        fill="none"
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
      />
      <path
        d="M58 12 Q68 8 72 15 Q70 20 62 18"
        fill="hsl(var(--foreground))"
        stroke="hsl(var(--foreground))"
        strokeWidth={Math.max(1, strokeWidth - 2)}
      />
    </svg>
  )
)
AppleShape.displayName = 'AppleShape'

// ============================================================================
// DECORATIVE SHAPES - Scribbles, tears, and artistic effects
// ============================================================================

export const ScribbleCircle = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-info', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <circle
        cx="50"
        cy="50"
        r="42"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
      <path
        d="M20 50 Q30 30 50 25 Q70 20 80 40 Q85 60 70 75 Q50 90 30 75 Q15 60 20 50"
        fill="none"
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth - 1}
        strokeDasharray="5,5"
      />
    </svg>
  )
)
ScribbleCircle.displayName = 'ScribbleCircle'

export const ScribbleUnderline = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.2}
      viewBox="0 0 100 20"
      className={cn('text-primary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M2 12 Q10 8 20 14 Q30 18 40 10 Q50 6 60 14 Q70 18 80 10 Q90 6 98 12"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
      />
      <path
        d="M5 16 Q15 10 25 16 Q35 20 45 12 Q55 8 65 16 Q75 20 85 12 Q95 8 98 14"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth - 1}
        strokeLinecap="round"
        opacity="0.5"
      />
    </svg>
  )
)
ScribbleUnderline.displayName = 'ScribbleUnderline'

export const PaperTearShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.3}
      viewBox="0 0 100 30"
      className={cn('text-card', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M0 0 L100 0 L100 15 L95 20 L90 12 L85 22 L80 10 L75 25 L70 8 L65 20 L60 12 L55 28 L50 10 L45 22 L40 15 L35 25 L30 10 L25 20 L20 8 L15 18 L10 12 L5 22 L0 15 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
PaperTearShape.displayName = 'PaperTearShape'

// ============================================================================
// NEW SHAPES - Seals, Gears, and Wavy Forms
// ============================================================================

export const SealShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-success', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M50 5 Q60 5 65 12 Q72 8 78 15 Q85 12 88 22 Q95 22 95 32 Q100 40 95 50 Q100 60 95 68 Q95 78 88 78 Q85 88 78 85 Q72 92 65 88 Q60 95 50 95 Q40 95 35 88 Q28 92 22 85 Q15 88 12 78 Q5 78 5 68 Q0 60 5 50 Q0 40 5 32 Q5 22 12 22 Q15 12 22 15 Q28 8 35 12 Q40 5 50 5 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
SealShape.displayName = 'SealShape'

export const GearShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size}
      viewBox="0 0 100 100"
      className={cn('text-muted-foreground', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <g transform="translate(0, 10)">
        <path
          d="M42 8 L58 8 L60 18 L68 14 L76 22 L72 30 L82 34 L82 48 L72 50 L76 58 L68 66 L60 62 L58 72 L42 72 L40 62 L32 66 L24 58 L28 50 L18 48 L18 34 L28 30 L24 22 L32 14 L40 18 Z"
          fill={filled ? (color || 'currentColor') : 'none'}
          stroke="hsl(var(--foreground))"
          strokeWidth={strokeWidth}
        />
        <circle
          cx="50"
          cy="40"
          r="12"
          fill="hsl(var(--background))"
          stroke="hsl(var(--foreground))"
          strokeWidth={strokeWidth}
        />
      </g>
    </svg>
  )
)
GearShape.displayName = 'GearShape'

export const WavyRectangleShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg
      ref={ref}
      width={size}
      height={size * 0.7}
      viewBox="0 0 100 70"
      className={cn('text-accent', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}
    >
      <path
        d="M10 15 Q15 10 20 15 Q25 20 30 15 Q35 10 40 15 Q45 20 50 15 Q55 10 60 15 Q65 20 70 15 Q75 10 80 15 Q85 20 90 15 L90 20 Q95 25 90 30 Q85 35 90 40 Q95 45 90 50 Q85 55 90 55 L90 60 Q85 65 80 60 Q75 55 70 60 Q65 65 60 60 Q55 55 50 60 Q45 65 40 60 Q35 55 30 60 Q25 65 20 60 Q15 55 10 60 L10 55 Q5 50 10 45 Q15 40 10 35 Q5 30 10 25 Q15 20 10 20 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))"
        strokeWidth={strokeWidth}
      />
    </svg>
  )
)
WavyRectangleShape.displayName = 'WavyRectangleShape'

// ============================================================================
// GEOMETRIC ADDITIONS — Regular polygons and ellipse
// ============================================================================

export const RhombusShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg ref={ref} width={size} height={size} viewBox="0 0 100 100"
      className={cn('text-info', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}>
      <path d="M50 5 L92 50 L50 95 L8 50 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} />
    </svg>
  )
)
RhombusShape.displayName = 'RhombusShape'

export const EllipseShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg ref={ref} width={size} height={size * 0.7} viewBox="0 0 100 70"
      className={cn('text-primary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}>
      <ellipse cx="50" cy="35" rx="46" ry="28"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} />
      <line x1="4" y1="35" x2="96" y2="35" stroke="hsl(var(--foreground))" strokeWidth={Math.max(1, strokeWidth - 1)} strokeDasharray="4,4" />
      <line x1="50" y1="7" x2="50" y2="63" stroke="hsl(var(--foreground))" strokeWidth={Math.max(1, strokeWidth - 1)} strokeDasharray="4,4" />
    </svg>
  )
)
EllipseShape.displayName = 'EllipseShape'

export const HeptagonShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg ref={ref} width={size} height={size} viewBox="0 0 100 100"
      className={cn('text-secondary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}>
      {/* 7 vertices: k=0..6, angle = k*2π/7 - π/2 */}
      <path d="M50 5 L85 22 L94 60 L70 91 L30 91 L6 60 L15 22 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} />
    </svg>
  )
)
HeptagonShape.displayName = 'HeptagonShape'

export const DecagonShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg ref={ref} width={size} height={size} viewBox="0 0 100 100"
      className={cn('text-accent', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}>
      {/* 10 vertices: k=0..9, angle = k*π/5 - π/2 */}
      <path d="M50 5 L76 14 L93 36 L93 64 L76 86 L50 95 L24 86 L7 64 L7 36 L24 14 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} />
    </svg>
  )
)
DecagonShape.displayName = 'DecagonShape'

// ============================================================================
// MATHEMATICAL SHAPES — Fractals, impossible figures, topological forms
// ============================================================================

/** Koch Snowflake iteration-2 path helper (runs once at module level, pure math) */
function buildKochPath(): string {
  let pts = [{ x: 50, y: 8 }, { x: 91, y: 73 }, { x: 9, y: 73 }]
  const cos60 = 0.5, sin60 = Math.sqrt(3) / 2
  for (let iter = 0; iter < 2; iter++) {
    const next: { x: number; y: number }[] = []
    for (let j = 0; j < pts.length; j++) {
      const a = pts[j], b = pts[(j + 1) % pts.length]
      const dx = (b.x - a.x) / 3, dy = (b.y - a.y) / 3
      const p1 = { x: a.x + dx, y: a.y + dy }
      const p2 = { x: a.x + 2 * dx, y: a.y + 2 * dy }
      const peak = { x: p1.x + dx * cos60 + dy * sin60, y: p1.y - dx * sin60 + dy * cos60 }
      next.push(a, p1, peak, p2)
    }
    pts = next
  }
  return pts.map((p, i) => `${i === 0 ? 'M' : 'L'}${p.x.toFixed(1)} ${p.y.toFixed(1)}`).join(' ') + ' Z'
}
const KOCH_PATH = buildKochPath()

export const KochSnowflakeShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg ref={ref} width={size} height={size} viewBox="0 0 100 100"
      className={cn('text-info', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}>
      <path d={KOCH_PATH}
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} strokeLinejoin="round" />
    </svg>
  )
)
KochSnowflakeShape.displayName = 'KochSnowflakeShape'

export const PenroseTriangleShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg ref={ref} width={size} height={size} viewBox="0 0 100 100"
      className={cn('text-warning', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}>
      {/* Top beam */}
      <path d="M50 8 L62 8 L38 50 L26 50 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} strokeLinejoin="round" />
      {/* Bottom-right beam */}
      <path d="M62 8 L88 52 L76 52 L50 8 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} strokeLinejoin="round" />
      {/* Bottom beam */}
      <path d="M26 50 L38 50 L76 52 L88 52 L82 62 L20 62 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} strokeLinejoin="round" />
    </svg>
  )
)
PenroseTriangleShape.displayName = 'PenroseTriangleShape'

/** Trefoil knot projection: x = sin(t)+2sin(2t), y = cos(t)-2cos(2t) */
function buildTrefoilPath(): string {
  const n = 120, scale = 14
  let d = ''
  for (let i = 0; i <= n; i++) {
    const t = (i / n) * 2 * Math.PI
    const x = 50 + scale * (Math.sin(t) + 2 * Math.sin(2 * t))
    const y = 50 + scale * (Math.cos(t) - 2 * Math.cos(2 * t))
    d += `${i === 0 ? 'M' : 'L'}${x.toFixed(2)} ${y.toFixed(2)} `
  }
  return d + 'Z'
}
const TREFOIL_PATH = buildTrefoilPath()

export const TrefoilShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg ref={ref} width={size} height={size} viewBox="0 0 100 100"
      className={cn('text-primary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}>
      <path d={TREFOIL_PATH}
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} strokeLinejoin="round" />
    </svg>
  )
)
TrefoilShape.displayName = 'TrefoilShape'

/** Fibonacci spiral: 8 quarter-circle arcs with φ-scaled radii */
function buildFibonacciPath(): string {
  const scale = 1.9
  const arcs: { cx: number; cy: number; r: number; startAngle: number }[] = [
    { cx: 51, cy: 51, r: 1, startAngle: Math.PI },
    { cx: 51, cy: 50, r: 1, startAngle: Math.PI / 2 },
    { cx: 50, cy: 50, r: 2, startAngle: 0 },
    { cx: 50, cy: 52, r: 3, startAngle: -Math.PI / 2 },
    { cx: 53, cy: 52, r: 5, startAngle: Math.PI },
    { cx: 53, cy: 47, r: 8, startAngle: Math.PI / 2 },
    { cx: 45, cy: 47, r: 13, startAngle: 0 },
    { cx: 45, cy: 60, r: 21, startAngle: -Math.PI / 2 },
  ]
  let d = ''
  arcs.forEach(({ cx, cy, r, startAngle }, i) => {
    const endAngle = startAngle - Math.PI / 2
    const sx = cx + r * scale * Math.cos(startAngle)
    const sy = cy + r * scale * Math.sin(startAngle)
    const ex = cx + r * scale * Math.cos(endAngle)
    const ey = cy + r * scale * Math.sin(endAngle)
    const R = r * scale
    if (i === 0) d += `M${sx.toFixed(1)} ${sy.toFixed(1)} `
    d += `A${R.toFixed(1)} ${R.toFixed(1)} 0 0 0 ${ex.toFixed(1)} ${ey.toFixed(1)} `
  })
  return d
}
const FIBONACCI_PATH = buildFibonacciPath()

export const FibonacciSpiralShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = false, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg ref={ref} width={size} height={size} viewBox="0 0 100 100"
      className={cn('text-accent', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}>
      <path d={FIBONACCI_PATH}
        fill="none"
        stroke={color || 'currentColor'}
        strokeWidth={strokeWidth} strokeLinecap="round" />
    </svg>
  )
)
FibonacciSpiralShape.displayName = 'FibonacciSpiralShape'

export const MobiusStripShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg ref={ref} width={size} height={size * 0.6} viewBox="0 0 100 60"
      className={cn('text-secondary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}>
      <path d="M5 15 Q15 5 30 10 Q50 18 70 8 Q85 3 95 15 Q100 25 95 35 Q85 47 70 42 Q50 35 30 45 Q15 52 5 42 Q0 35 5 25 Q5 20 5 15 Z"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} strokeLinejoin="round" />
      <path d="M38 28 Q50 22 62 28 Q50 34 38 28 Z"
        fill="hsl(var(--background))" stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} />
    </svg>
  )
)
MobiusStripShape.displayName = 'MobiusStripShape'

export const TorusShape = React.forwardRef<SVGSVGElement, ShapeProps>(
  ({ size = 100, strokeWidth = 3, filled = true, color, animation = 'none', speed = 'normal', className, ...props }, ref) => (
    <svg ref={ref} width={size} height={size} viewBox="0 0 100 100"
      className={cn('text-primary', getAnimClass(animation, speed), className)}
      aria-hidden="true"
      {...props}>
      <ellipse cx="50" cy="50" rx="45" ry="20"
        fill={filled ? (color || 'currentColor') : 'none'}
        stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} />
      <ellipse cx="50" cy="50" rx="20" ry="9"
        fill="hsl(var(--background))" stroke="hsl(var(--foreground))" strokeWidth={strokeWidth} />
      <ellipse cx="50" cy="55" rx="45" ry="20"
        fill="none" stroke="hsl(var(--foreground))" strokeWidth={Math.max(1, strokeWidth - 1)} strokeDasharray="4,4" />
    </svg>
  )
)
TorusShape.displayName = 'TorusShape'

// ============================================================================
// SHAPE CATEGORIES - Grouped exports for easy access
// ============================================================================

export const GeometricShapes = {
  TriangleShape,
  DiamondBadge,
  PentagonShape,
  HexagonShape,
  OctagonShape,
  CrossShape,
  TrapezoidShape,
  ParallelogramShape,
  RhombusShape,
  EllipseShape,
  HeptagonShape,
  DecagonShape,
}

export const MathematicalShapes = {
  KochSnowflakeShape,
  PenroseTriangleShape,
  TrefoilShape,
  FibonacciSpiralShape,
  MobiusStripShape,
  TorusShape,
}

export const StarShapes = {
  Star4Shape,
  Star5Shape,
  Star6Shape,
  BurstShape,
  ExplosionShape,
  SplatShape,
  LightningShape,
}

export const OrganicShapes = {
  BlobShape,
  WaveShape,
  CloudShape,
  HeartShape,
  AppleShape,
}

export const CelestialShapes = {
  SunShape,
  CrescentShape,
  RainbowShape,
  PlanetShape,
  UmbrellaShape,
}

export const BadgeShapes = {
  ArrowBadge,
  ZigzagBanner,
  RibbonShape,
  ShieldShape,
  TagShape,
  PriceTagShape,
  TicketShape,
  CouponShape,
  BookmarkShape,
  FlagShape,
  PillShape,
  SealShape,
  WavyRectangleShape,
}

export const MechanicalShapes = {
  GearShape,
}

export const CommunicationShapes = {
  SpeechBubble,
  CursorShape,
  EyeShape,
}

export const DecorativeShapes = {
  ScribbleCircle,
  ScribbleUnderline,
  PaperTearShape,
}

export const TicketShapes = {
  WavyRectangleShape,
}

// ============================================================================
// ALL SHAPES - Combined export for backwards compatibility
// ============================================================================

export const shapes = {
  // Geometric
  TriangleShape,
  DiamondBadge,
  PentagonShape,
  HexagonShape,
  OctagonShape,
  CrossShape,
  TrapezoidShape,
  ParallelogramShape,
  // Stars & Bursts
  Star4Shape,
  Star5Shape,
  Star6Shape,
  BurstShape,
  ExplosionShape,
  SplatShape,
  LightningShape,
  // Organic
  BlobShape,
  WaveShape,
  CloudShape,
  HeartShape,
  AppleShape,
  // Celestial & Nature
  SunShape,
  CrescentShape,
  RainbowShape,
  PlanetShape,
  UmbrellaShape,
  // Badges & UI
  ArrowBadge,
  ZigzagBanner,
  RibbonShape,
  ShieldShape,
  TagShape,
  PriceTagShape,
  TicketShape,
  CouponShape,
  BookmarkShape,
  FlagShape,
  PillShape,
  SealShape,
  WavyRectangleShape,
  // Communication
  SpeechBubble,
  CursorShape,
  EyeShape,
  // Decorative
  ScribbleCircle,
  ScribbleUnderline,
  PaperTearShape,
  // Mechanical
  GearShape,
  // Geometric additions
  RhombusShape,
  EllipseShape,
  HeptagonShape,
  DecagonShape,
  // Mathematical
  KochSnowflakeShape,
  PenroseTriangleShape,
  TrefoilShape,
  FibonacciSpiralShape,
  MobiusStripShape,
  TorusShape,
}
```

---

### Sheet <a name="sheet"></a>

Extends Dialog to display content that complements the main page

- **Registry Name**: `@boldkit/sheet`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/sheet.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-dialog`
  - `class-variance-authority`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/sheet-demo.tsx`

```tsx
import {
  Sheet,
  SheetContent,
  SheetDescription,
  SheetHeader,
  SheetTitle,
  SheetTrigger,
} from '@/components/ui/sheet'

export default function Example() {
  return (
    <Sheet>
      <SheetTrigger>Open</SheetTrigger>
      <SheetContent>
        <SheetHeader>
          <SheetTitle>Sheet Title</SheetTitle>
          <SheetDescription>
            This is a sheet description.
          </SheetDescription>
        </SheetHeader>
      </SheetContent>
    </Sheet>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/sheet.tsx`

```tsx
import * as React from 'react'
import * as SheetPrimitive from '@radix-ui/react-dialog'
import { cva, type VariantProps } from 'class-variance-authority'
import { X } from 'lucide-react'
import { cn } from '@/lib/utils'

const Sheet = SheetPrimitive.Root

const SheetTrigger = SheetPrimitive.Trigger

const SheetClose = SheetPrimitive.Close

const SheetPortal = SheetPrimitive.Portal

const SheetOverlay = React.forwardRef<
  React.ElementRef<typeof SheetPrimitive.Overlay>,
  React.ComponentPropsWithoutRef<typeof SheetPrimitive.Overlay>
>(({ className, ...props }, ref) => (
  <SheetPrimitive.Overlay
    className={cn(
      'fixed inset-0 z-50 bg-black/70 data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0',
      className
    )}
    {...props}
    ref={ref}
  />
))
SheetOverlay.displayName = SheetPrimitive.Overlay.displayName

const sheetVariants = cva(
  'fixed z-50 gap-4 bg-background border-3 border-foreground p-6 shadow-[8px_8px_0px_hsl(var(--shadow-color))] transition ease-in-out data-[state=closed]:duration-300 data-[state=open]:duration-500 data-[state=open]:animate-in data-[state=closed]:animate-out',
  {
    variants: {
      side: {
        top: 'inset-x-0 top-0 border-t-0 data-[state=closed]:slide-out-to-top data-[state=open]:slide-in-from-top',
        bottom:
          'inset-x-0 bottom-0 border-b-0 data-[state=closed]:slide-out-to-bottom data-[state=open]:slide-in-from-bottom',
        left: 'inset-y-0 left-0 h-full w-3/4 border-l-0 data-[state=closed]:slide-out-to-left data-[state=open]:slide-in-from-left sm:max-w-sm',
        right:
          'inset-y-0 right-0 h-full w-3/4 border-r-0 data-[state=closed]:slide-out-to-right data-[state=open]:slide-in-from-right sm:max-w-sm',
      },
    },
    defaultVariants: {
      side: 'right',
    },
  }
)

interface SheetContentProps
  extends React.ComponentPropsWithoutRef<typeof SheetPrimitive.Content>,
    VariantProps<typeof sheetVariants> {}

const SheetContent = React.forwardRef<
  React.ElementRef<typeof SheetPrimitive.Content>,
  SheetContentProps
>(({ side = 'right', className, children, ...props }, ref) => (
  <SheetPortal>
    <SheetOverlay />
    <SheetPrimitive.Content
      ref={ref}
      className={cn(sheetVariants({ side }), className)}
      {...props}
    >
      <SheetPrimitive.Close className="absolute right-4 top-4 border-2 border-foreground bg-background p-1 shadow-[4px_4px_0px_hsl(var(--shadow-color))] transition-all duration-200 hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none active:translate-x-[4px] active:translate-y-[4px] active:shadow-none focus:outline-none focus:ring-2 focus:ring-ring disabled:pointer-events-none">
        <X className="h-4 w-4 stroke-[3]" />
        <span className="sr-only">Close</span>
      </SheetPrimitive.Close>
      {children}
    </SheetPrimitive.Content>
  </SheetPortal>
))
SheetContent.displayName = SheetPrimitive.Content.displayName

const SheetHeader = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn(
      'flex flex-col space-y-2 text-center sm:text-left',
      className
    )}
    {...props}
  />
)
SheetHeader.displayName = 'SheetHeader'

const SheetFooter = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn(
      'flex flex-col-reverse sm:flex-row sm:justify-end sm:space-x-2',
      className
    )}
    {...props}
  />
)
SheetFooter.displayName = 'SheetFooter'

const SheetTitle = React.forwardRef<
  React.ElementRef<typeof SheetPrimitive.Title>,
  React.ComponentPropsWithoutRef<typeof SheetPrimitive.Title>
>(({ className, ...props }, ref) => (
  <SheetPrimitive.Title
    ref={ref}
    className={cn('text-lg font-bold uppercase tracking-wide', className)}
    {...props}
  />
))
SheetTitle.displayName = SheetPrimitive.Title.displayName

const SheetDescription = React.forwardRef<
  React.ElementRef<typeof SheetPrimitive.Description>,
  React.ComponentPropsWithoutRef<typeof SheetPrimitive.Description>
>(({ className, ...props }, ref) => (
  <SheetPrimitive.Description
    ref={ref}
    className={cn('text-sm text-muted-foreground', className)}
    {...props}
  />
))
SheetDescription.displayName = SheetPrimitive.Description.displayName

export {
  Sheet,
  SheetPortal,
  SheetOverlay,
  SheetTrigger,
  SheetClose,
  SheetContent,
  SheetHeader,
  SheetFooter,
  SheetTitle,
  SheetDescription,
}
```

---

### Sidebar <a name="sidebar"></a>

Collapsible sidebar with mobile drawer, tooltips, and keyboard shortcut

- **Registry Name**: `@boldkit/sidebar`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/sidebar.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`
  - `@boldkit/sheet`
  - `@boldkit/tooltip`

#### How to Use

##### File: `registry/default/example/sidebar-demo.tsx`

```tsx
import {
  Sidebar, SidebarProvider, SidebarHeader, SidebarContent,
  SidebarItem, SidebarToggle, SidebarInset
} from '@/components/ui/sidebar'

export default function Example() {
  return (
    <SidebarProvider>
      <Sidebar>
        <SidebarHeader>Logo</SidebarHeader>
        <SidebarContent>
          <SidebarItem icon={<Home />}>Home</SidebarItem>
          <SidebarItem icon={<Settings />}>Settings</SidebarItem>
        </SidebarContent>
      </Sidebar>
      <SidebarInset>
        <SidebarToggle />
        <main>Content</main>
      </SidebarInset>
    </SidebarProvider>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/sidebar.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { PanelLeft } from 'lucide-react'
import { cn } from '@/lib/utils'
import { Button } from '@/components/ui/button'
import { Sheet, SheetContent } from '@/components/ui/sheet'
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/ui/tooltip'

const SIDEBAR_COOKIE_NAME = 'sidebar:state'
const SIDEBAR_COOKIE_MAX_AGE = 60 * 60 * 24 * 7 // 7 days
const SIDEBAR_WIDTH = '16rem'
const SIDEBAR_WIDTH_COLLAPSED = '4rem'
const SIDEBAR_WIDTH_MOBILE = '18rem'
const SIDEBAR_KEYBOARD_SHORTCUT = 'b'

// Context
interface SidebarContextValue {
  state: 'expanded' | 'collapsed'
  open: boolean
  setOpen: (open: boolean) => void
  openMobile: boolean
  setOpenMobile: (open: boolean) => void
  isMobile: boolean
  toggleSidebar: () => void
}

const SidebarContext = React.createContext<SidebarContextValue | null>(null)

function useSidebar() {
  const context = React.useContext(SidebarContext)
  if (!context) {
    throw new Error('useSidebar must be used within a <SidebarProvider />')
  }
  return context
}

// Provider
interface SidebarProviderProps extends React.HTMLAttributes<HTMLDivElement> {
  defaultOpen?: boolean
  open?: boolean
  onOpenChange?: (open: boolean) => void
}

const SidebarProvider = React.forwardRef<HTMLDivElement, SidebarProviderProps>(
  (
    {
      defaultOpen = true,
      open: controlledOpen,
      onOpenChange,
      className,
      style,
      children,
      ...props
    },
    ref
  ) => {
    const [uncontrolledOpen, setUncontrolledOpen] = React.useState(defaultOpen)
    const [openMobile, setOpenMobile] = React.useState(false)
    const [isMobile, setIsMobile] = React.useState(false)

    const isControlled = controlledOpen !== undefined
    const open = isControlled ? controlledOpen : uncontrolledOpen

    const setOpen = React.useCallback(
      (value: boolean) => {
        if (!isControlled) {
          setUncontrolledOpen(value)
        }
        onOpenChange?.(value)

        // Save to cookie
        try {
          document.cookie = `${SIDEBAR_COOKIE_NAME}=${value}; path=/; max-age=${SIDEBAR_COOKIE_MAX_AGE}`
        } catch {
          // Silent fail — cookie storage unavailable (private browsing, etc.)
        }
      },
      [isControlled, onOpenChange]
    )

    const toggleSidebar = React.useCallback(() => {
      if (isMobile) {
        setOpenMobile((prev) => !prev)
      } else {
        setOpen(!open)
      }
    }, [isMobile, open, setOpen])

    // Check if mobile
    React.useEffect(() => {
      const checkMobile = () => {
        setIsMobile(window.innerWidth < 768)
      }
      checkMobile()
      window.addEventListener('resize', checkMobile)
      return () => window.removeEventListener('resize', checkMobile)
    }, [])

    // Keyboard shortcut
    React.useEffect(() => {
      const handleKeyDown = (e: KeyboardEvent) => {
        if (
          e.key === SIDEBAR_KEYBOARD_SHORTCUT &&
          (e.metaKey || e.ctrlKey) &&
          !e.shiftKey &&
          !e.altKey
        ) {
          e.preventDefault()
          toggleSidebar()
        }
      }

      window.addEventListener('keydown', handleKeyDown)
      return () => window.removeEventListener('keydown', handleKeyDown)
    }, [toggleSidebar])

    const state = open ? 'expanded' : 'collapsed'

    const contextValue = React.useMemo<SidebarContextValue>(
      () => ({
        state,
        open,
        setOpen,
        openMobile,
        setOpenMobile,
        isMobile,
        toggleSidebar,
      }),
      [state, open, setOpen, openMobile, setOpenMobile, isMobile, toggleSidebar]
    )

    return (
      <SidebarContext.Provider value={contextValue}>
        <div
          ref={ref}
          style={
            {
              '--sidebar-width': SIDEBAR_WIDTH,
              '--sidebar-width-collapsed': SIDEBAR_WIDTH_COLLAPSED,
              ...style,
            } as React.CSSProperties & Record<string, string>
          }
          className={cn(
            'group/sidebar-wrapper flex min-h-screen w-full',
            className
          )}
          {...props}
        >
          {children}
        </div>
      </SidebarContext.Provider>
    )
  }
)
SidebarProvider.displayName = 'SidebarProvider'

// Main Sidebar
const sidebarVariants = cva(
  'relative flex h-full flex-col border-r-3 border-foreground bg-background transition-all duration-300 ease-out',
  {
    variants: {
      collapsible: {
        none: 'w-[var(--sidebar-width)]',
        icon: 'w-[var(--sidebar-width)] group-data-[state=collapsed]/sidebar:w-[var(--sidebar-width-collapsed)]',
        hidden: 'w-[var(--sidebar-width)] group-data-[state=collapsed]/sidebar:w-0 group-data-[state=collapsed]/sidebar:border-0 group-data-[state=collapsed]/sidebar:overflow-hidden',
      },
      side: {
        left: '',
        right: 'border-r-0 border-l-3',
      },
    },
    defaultVariants: {
      collapsible: 'icon',
      side: 'left',
    },
  }
)

interface SidebarProps
  extends React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof sidebarVariants> {}

const Sidebar = React.forwardRef<HTMLDivElement, SidebarProps>(
  ({ side = 'left', collapsible = 'icon', className, children, ...props }, ref) => {
    const { isMobile, state, openMobile, setOpenMobile } = useSidebar()

    if (isMobile) {
      return (
        <Sheet open={openMobile} onOpenChange={setOpenMobile}>
          <SheetContent
            side={side === 'left' ? 'left' : 'right'}
            className="w-[var(--sidebar-width-mobile)] p-0"
            style={{ '--sidebar-width-mobile': SIDEBAR_WIDTH_MOBILE } as React.CSSProperties & Record<string, string>}
          >
            <div className="flex h-full flex-col">{children}</div>
          </SheetContent>
        </Sheet>
      )
    }

    return (
      <div
        ref={ref}
        data-state={state}
        data-collapsible={collapsible}
        className="group/sidebar hidden md:block"
      >
        <div
          className={cn(sidebarVariants({ collapsible, side }), className)}
          {...props}
        >
          {children}
        </div>
      </div>
    )
  }
)
Sidebar.displayName = 'Sidebar'

// Sidebar Toggle
const SidebarToggle = React.forwardRef<
  HTMLButtonElement,
  React.ComponentProps<typeof Button>
>(({ className, ...props }, ref) => {
  const { toggleSidebar, state } = useSidebar()

  return (
    <Button
      ref={ref}
      variant="outline"
      size="icon"
      className={cn('h-8 w-8 transition-transform duration-300', className)}
      onClick={toggleSidebar}
      {...props}
    >
      <PanelLeft
        className={cn(
          'h-4 w-4 transition-transform duration-300',
          state === 'collapsed' && 'rotate-180'
        )}
      />
      <span className="sr-only">Toggle Sidebar</span>
    </Button>
  )
})
SidebarToggle.displayName = 'SidebarToggle'

// Sidebar Header
const SidebarHeader = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn(
      'flex items-center border-b-3 border-foreground p-4',
      'group-data-[state=collapsed]/sidebar:justify-center',
      className
    )}
    {...props}
  />
))
SidebarHeader.displayName = 'SidebarHeader'

// Sidebar Content
const SidebarContent = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn('flex-1 overflow-auto p-4', className)}
    {...props}
  />
))
SidebarContent.displayName = 'SidebarContent'

// Sidebar Footer
const SidebarFooter = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn(
      'flex items-center border-t-3 border-foreground p-4',
      'group-data-[state=collapsed]/sidebar:justify-center',
      className
    )}
    {...props}
  />
))
SidebarFooter.displayName = 'SidebarFooter'

// Sidebar Group
const SidebarGroup = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn('space-y-2', className)}
    {...props}
  />
))
SidebarGroup.displayName = 'SidebarGroup'

// Sidebar Group Label
const SidebarGroupLabel = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => {
  const { state } = useSidebar()

  return (
    <div
      ref={ref}
      className={cn(
        'px-2 py-1 text-xs font-bold uppercase tracking-wide text-muted-foreground',
        'transition-opacity duration-200',
        state === 'collapsed' && 'opacity-0 hidden',
        className
      )}
      {...props}
    />
  )
})
SidebarGroupLabel.displayName = 'SidebarGroupLabel'

// Sidebar Item
const sidebarItemVariants = cva(
  'flex w-full items-center gap-3 px-3 py-2 text-sm transition-all duration-150',
  {
    variants: {
      variant: {
        default: 'hover:bg-muted',
        active: 'bg-accent shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
      },
    },
    defaultVariants: {
      variant: 'default',
    },
  }
)

interface SidebarItemProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof sidebarItemVariants> {
  icon?: React.ReactNode
  tooltip?: string
}

const SidebarItem = React.forwardRef<HTMLButtonElement, SidebarItemProps>(
  ({ variant, icon, tooltip, className, children, ...props }, ref) => {
    const { state } = useSidebar()
    const isCollapsed = state === 'collapsed'

    const button = (
      <button
        ref={ref}
        className={cn(
          sidebarItemVariants({ variant }),
          isCollapsed && 'justify-center px-2',
          className
        )}
        {...props}
      >
        {icon && <span className="shrink-0">{icon}</span>}
        {!isCollapsed && <span className="truncate">{children}</span>}
      </button>
    )

    if (isCollapsed && tooltip) {
      return (
        <Tooltip>
          <TooltipTrigger asChild>{button}</TooltipTrigger>
          <TooltipContent side="right">{tooltip}</TooltipContent>
        </Tooltip>
      )
    }

    return button
  }
)
SidebarItem.displayName = 'SidebarItem'

// Sidebar Separator
const SidebarSeparator = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn('mx-2 h-[3px] bg-foreground', className)}
    {...props}
  />
))
SidebarSeparator.displayName = 'SidebarSeparator'

// Sidebar Inset (main content wrapper)
const SidebarInset = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn('flex-1 overflow-auto', className)}
    {...props}
  />
))
SidebarInset.displayName = 'SidebarInset'

export {
  Sidebar,
  SidebarProvider,
  SidebarHeader,
  SidebarContent,
  SidebarFooter,
  SidebarGroup,
  SidebarGroupLabel,
  SidebarItem,
  SidebarSeparator,
  SidebarToggle,
  SidebarInset,
  useSidebar,
  sidebarVariants,
  sidebarItemVariants,
}
```

---

### Skeleton <a name="skeleton"></a>

Used to show a placeholder while content is loading

- **Registry Name**: `@boldkit/skeleton`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/skeleton.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/skeleton-demo.tsx`

```tsx
import { Skeleton } from '@/components/ui/skeleton'

export default function Example() {
  return (
    <div className="flex items-center space-x-4">
      <Skeleton className="h-12 w-12 rounded-full" />
      <div className="space-y-2">
        <Skeleton className="h-4 w-[250px]" />
        <Skeleton className="h-4 w-[200px]" />
      </div>
    </div>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/skeleton.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'

function Skeleton({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) {
  return (
    <div
      className={cn('animate-pulse bg-muted border-2 border-foreground/20', className)}
      {...props}
    />
  )
}

export { Skeleton }
```

---

### Slider <a name="slider"></a>

A jelly slider with spring physics and bouncy animations

- **Registry Name**: `@boldkit/slider`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/slider.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/slider-demo.tsx`

```tsx
import { Slider } from '@/components/ui/slider'

export default function Example() {
  return (
    <Slider defaultValue={[50]} max={100} step={1} />
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/slider.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'

export interface SliderProps extends Omit<React.HTMLAttributes<HTMLDivElement>, 'onChange' | 'defaultValue'> {
  value?: number[]
  defaultValue?: number[]
  min?: number
  max?: number
  step?: number
  onValueChange?: (value: number[]) => void
  onValueCommit?: (value: number[]) => void
  disabled?: boolean
  orientation?: 'horizontal' | 'vertical'
  // Jelly physics settings
  stiffness?: number
  damping?: number
  mass?: number
}

interface SpringState {
  position: number
  velocity: number
}

const Slider = React.forwardRef<HTMLDivElement, SliderProps>(
  (
    {
      value: controlledValue,
      defaultValue = [0],
      min = 0,
      max = 100,
      step = 1,
      onValueChange,
      onValueCommit,
      disabled = false,
      orientation = 'horizontal',
      stiffness = 400,
      damping = 28,
      mass = 1,
      className,
      ...props
    },
    ref
  ) => {
    const trackRef = React.useRef<HTMLDivElement>(null)
    const animationRef = React.useRef<number | null>(null)
    const lastTimeRef = React.useRef<number>(0)
    const isDraggingRef = React.useRef<boolean>(false)
    const keyboardDragTimeoutRef = React.useRef<ReturnType<typeof setTimeout> | null>(null)
    const currentValueRef = React.useRef<number[]>(defaultValue)

    // Track active drag handlers for cleanup on unmount
    const dragHandlersRef = React.useRef<{
      onMove: ((e: PointerEvent) => void) | null
      onUp: (() => void) | null
    }>({ onMove: null, onUp: null })

    const isControlled = controlledValue !== undefined
    const wasControlled = React.useRef(isControlled)
    React.useEffect(() => {
      if (wasControlled.current !== isControlled) {
        if (process.env.NODE_ENV === 'development') {
          console.warn('[Slider] Component is changing from ' + (wasControlled.current ? 'controlled' : 'uncontrolled') + ' to ' + (isControlled ? 'controlled' : 'uncontrolled') + '. This is not supported.')
        }
        wasControlled.current = isControlled
      }
    }, [isControlled])
    const [uncontrolledValue, setUncontrolledValue] = React.useState(defaultValue)
    const actualValue = isControlled ? controlledValue : uncontrolledValue

    const isRange = actualValue.length > 1

    // Spring physics state for each thumb
    const [springs, setSprings] = React.useState<SpringState[]>(() =>
      actualValue.map((v) => ({
        position: ((v - min) / (max - min)) * 100,
        velocity: 0,
      }))
    )

    const [targets, setTargets] = React.useState<number[]>(() =>
      actualValue.map((v) => ((v - min) / (max - min)) * 100)
    )
    const targetsRef = React.useRef<number[]>(targets)

    const [activeThumb, setActiveThumb] = React.useState<number | null>(null)
    const [squishes, setSquishes] = React.useState<{ scaleX: number; scaleY: number }[]>(() =>
      actualValue.map(() => ({ scaleX: 1, scaleY: 1 }))
    )
    const [hoveringThumb, setHoveringThumb] = React.useState<number | null>(null)

    // Keep currentValueRef in sync for stale-closure-free commit
    React.useEffect(() => {
      currentValueRef.current = actualValue
    }, [actualValue])

    // Update targets when value changes externally
    React.useEffect(() => {
      const newTargets = actualValue.map((v) => ((v - min) / (max - min)) * 100)
      setTargets(newTargets)
      targetsRef.current = newTargets

      // Ensure springs array matches value array length
      if (springs.length !== actualValue.length) {
        setSprings(actualValue.map((v) => ({
          position: ((v - min) / (max - min)) * 100,
          velocity: 0,
        })))
        setSquishes(actualValue.map(() => ({ scaleX: 1, scaleY: 1 })))
      }
    }, [actualValue, min, max, springs.length])

    // Spring physics simulation — only runs while dragging
    const startSpringLoop = React.useCallback(() => {
      if (animationRef.current) return // already running

      lastTimeRef.current = 0

      const simulate = (timestamp: number) => {
        if (!isDraggingRef.current) {
          animationRef.current = null
          return
        }

        if (!lastTimeRef.current) {
          lastTimeRef.current = timestamp
        }

        const deltaTime = Math.min((timestamp - lastTimeRef.current) / 1000, 0.064)
        lastTimeRef.current = timestamp

        setSprings((prev) => {
          const newSprings: SpringState[] = []
          const newSquishes: { scaleX: number; scaleY: number }[] = []

          const len = Math.min(prev.length, targetsRef.current.length)
          for (let i = 0; i < len; i++) {
            const displacement = targetsRef.current[i] - prev[i].position
            const springForce = stiffness * displacement
            const dampingForce = damping * prev[i].velocity
            const acceleration = (springForce - dampingForce) / mass

            const newVelocity = prev[i].velocity + acceleration * deltaTime
            const newPosition = prev[i].position + newVelocity * deltaTime

            // Calculate squish based on velocity
            const velocityMagnitude = Math.abs(newVelocity)
            const squishAmount = Math.min(velocityMagnitude / 500, 0.25)
            const direction = newVelocity > 0 ? 1 : -1

            newSquishes.push({
              scaleX: 1 + squishAmount * direction * 0.4,
              scaleY: 1 - squishAmount * 0.25,
            })

            // Stop animation when settled
            if (Math.abs(displacement) < 0.01 && Math.abs(newVelocity) < 0.01) {
              newSprings.push({ position: targetsRef.current[i], velocity: 0 })
              newSquishes[i] = { scaleX: 1, scaleY: 1 }
            } else {
              newSprings.push({ position: newPosition, velocity: newVelocity })
            }
          }

          setSquishes(newSquishes)
          return newSprings
        })

        animationRef.current = requestAnimationFrame(simulate)
      }

      animationRef.current = requestAnimationFrame(simulate)
    }, [stiffness, damping, mass])

    // Cleanup rAF on unmount
    React.useEffect(() => {
      return () => {
        if (animationRef.current) {
          cancelAnimationFrame(animationRef.current)
        }
        if (keyboardDragTimeoutRef.current) {
          clearTimeout(keyboardDragTimeoutRef.current)
        }
      }
    }, [])

    // Cleanup drag handlers on unmount
    React.useEffect(() => {
      return () => {
        const { onMove, onUp } = dragHandlersRef.current
        if (onMove) {
          document.removeEventListener('pointermove', onMove)
        }
        if (onUp) {
          document.removeEventListener('pointerup', onUp)
        }
      }
    }, [])

    const getValueFromPosition = (clientX: number, clientY: number) => {
      if (!trackRef.current) return 0

      const rect = trackRef.current.getBoundingClientRect()
      let percent: number

      if (orientation === 'vertical') {
        percent = 1 - Math.max(0, Math.min(1, (clientY - rect.top) / rect.height))
      } else {
        percent = Math.max(0, Math.min(1, (clientX - rect.left) / rect.width))
      }

      const rawValue = min + percent * (max - min)
      const steppedValue = Math.round(rawValue / step) * step
      return Math.max(min, Math.min(max, steppedValue))
    }

    const getPercentFromPosition = (clientX: number, clientY: number) => {
      if (!trackRef.current) return 0

      const rect = trackRef.current.getBoundingClientRect()

      if (orientation === 'vertical') {
        return (1 - Math.max(0, Math.min(1, (clientY - rect.top) / rect.height))) * 100
      }
      return Math.max(0, Math.min(1, (clientX - rect.left) / rect.width)) * 100
    }

    const findNearestThumb = (percent: number): number => {
      if (!isRange) return 0

      let nearestIndex = 0
      let nearestDistance = Infinity

      for (let i = 0; i < springs.length; i++) {
        const distance = Math.abs(springs[i].position - percent)
        if (distance < nearestDistance) {
          nearestDistance = distance
          nearestIndex = i
        }
      }

      return nearestIndex
    }

    const updateValue = (index: number, newValue: number) => {
      const newValues = [...actualValue]

      if (isRange) {
        // Ensure values don't cross over
        if (index === 0) {
          newValues[0] = Math.min(newValue, actualValue[1] - step)
        } else {
          newValues[1] = Math.max(newValue, actualValue[0] + step)
        }
      } else {
        newValues[index] = newValue
      }

      // Clamp to min/max
      newValues[index] = Math.max(min, Math.min(max, newValues[index]))

      if (!isControlled) {
        setUncontrolledValue(newValues)
      }
      onValueChange?.(newValues)
    }

    const handleTrackPointerDown = (e: React.PointerEvent) => {
      if (disabled) return

      e.preventDefault()
      const percent = getPercentFromPosition(e.clientX, e.clientY)
      const nearestThumbIndex = findNearestThumb(percent)
      const newValue = getValueFromPosition(e.clientX, e.clientY)

      setActiveThumb(nearestThumbIndex)
      isDraggingRef.current = true
      startSpringLoop()
      updateValue(nearestThumbIndex, newValue)

      // Add velocity boost for jelly effect
      setSprings((prev) => {
        const newSprings = [...prev]
        newSprings[nearestThumbIndex] = {
          ...newSprings[nearestThumbIndex],
          velocity: (e.movementX || 0) * 10,
        }
        return newSprings
      })

      const handlePointerMove = (e: PointerEvent) => {
        const newValue = getValueFromPosition(e.clientX, e.clientY)
        updateValue(nearestThumbIndex, newValue)

        // Add velocity for jelly effect
        setSprings((prev) => {
          const newSprings = [...prev]
          newSprings[nearestThumbIndex] = {
            ...newSprings[nearestThumbIndex],
            velocity: newSprings[nearestThumbIndex].velocity + (e.movementX || 0) * 3,
          }
          return newSprings
        })
      }

      const handlePointerUp = () => {
        setActiveThumb(null)
        isDraggingRef.current = false
        onValueCommit?.(currentValueRef.current)
        document.removeEventListener('pointermove', handlePointerMove)
        document.removeEventListener('pointerup', handlePointerUp)
        dragHandlersRef.current = { onMove: null, onUp: null }
      }

      // Store handlers for cleanup
      dragHandlersRef.current = { onMove: handlePointerMove, onUp: handlePointerUp }

      document.addEventListener('pointermove', handlePointerMove)
      document.addEventListener('pointerup', handlePointerUp)
    }

    const handleThumbPointerDown = (index: number) => (e: React.PointerEvent) => {
      if (disabled) return

      e.preventDefault()
      e.stopPropagation()
      setActiveThumb(index)
      isDraggingRef.current = true
      startSpringLoop()

      const handlePointerMove = (e: PointerEvent) => {
        const newValue = getValueFromPosition(e.clientX, e.clientY)
        updateValue(index, newValue)

        // Add velocity for jelly effect
        setSprings((prev) => {
          const newSprings = [...prev]
          newSprings[index] = {
            ...newSprings[index],
            velocity: newSprings[index].velocity + (e.movementX || 0) * 3,
          }
          return newSprings
        })
      }

      const handlePointerUp = () => {
        setActiveThumb(null)
        isDraggingRef.current = false
        onValueCommit?.(currentValueRef.current)
        document.removeEventListener('pointermove', handlePointerMove)
        document.removeEventListener('pointerup', handlePointerUp)
        dragHandlersRef.current = { onMove: null, onUp: null }
      }

      // Store handlers for cleanup
      dragHandlersRef.current = { onMove: handlePointerMove, onUp: handlePointerUp }

      document.addEventListener('pointermove', handlePointerMove)
      document.addEventListener('pointerup', handlePointerUp)
    }

    const handleKeyDown = (index: number) => (e: React.KeyboardEvent) => {
      if (disabled) return

      let newValue = actualValue[index]
      const largeStep = (max - min) / 10

      switch (e.key) {
        case 'ArrowRight':
        case 'ArrowUp':
          newValue = Math.min(max, actualValue[index] + step)
          break
        case 'ArrowLeft':
        case 'ArrowDown':
          newValue = Math.max(min, actualValue[index] - step)
          break
        case 'PageUp':
          newValue = Math.min(max, actualValue[index] + largeStep)
          break
        case 'PageDown':
          newValue = Math.max(min, actualValue[index] - largeStep)
          break
        case 'Home':
          newValue = min
          break
        case 'End':
          newValue = max
          break
        default:
          return
      }

      e.preventDefault()
      updateValue(index, newValue)

      // Add velocity for keyboard navigation jelly effect
      setSprings((prev) => {
        const newSprings = [...prev]
        newSprings[index] = {
          ...newSprings[index],
          velocity: (newValue > actualValue[index] ? 1 : -1) * 80,
        }
        return newSprings
      })
      isDraggingRef.current = true
      startSpringLoop()
      if (keyboardDragTimeoutRef.current) clearTimeout(keyboardDragTimeoutRef.current)
      keyboardDragTimeoutRef.current = setTimeout(() => { isDraggingRef.current = false }, 300)
    }

    // Calculate range fill position
    const getRangeFillStyle = () => {
      if (isRange) {
        const left = Math.min(springs[0]?.position ?? 0, springs[1]?.position ?? 0)
        const right = Math.max(springs[0]?.position ?? 0, springs[1]?.position ?? 0)
        return {
          left: `${left}%`,
          width: `${right - left}%`,
        }
      }
      return {
        left: '0%',
        width: `${springs[0]?.position ?? 0}%`,
      }
    }

    return (
      <div
        ref={ref}
        className={cn(
          'relative flex touch-none select-none items-center',
          orientation === 'vertical' ? 'h-full w-5 flex-col' : 'w-full py-2',
          disabled && 'opacity-50 pointer-events-none',
          className
        )}
        {...props}
      >
        {/* Track */}
        <div
          ref={trackRef}
          className={cn(
            'relative cursor-pointer overflow-hidden',
            'border-3 border-foreground bg-muted',
            'shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
            'transition-shadow duration-200',
            orientation === 'vertical' ? 'h-full w-4' : 'h-4 w-full',
            activeThumb !== null && 'shadow-[2px_2px_0px_hsl(var(--shadow-color))]'
          )}
          onPointerDown={handleTrackPointerDown}
        >
          {/* Range fill */}
          <div
            className={cn(
              'absolute bg-primary',
              orientation === 'vertical' ? 'w-full' : 'h-full'
            )}
            style={
              orientation === 'vertical'
                ? {
                    bottom: getRangeFillStyle().left,
                    height: getRangeFillStyle().width,
                  }
                : getRangeFillStyle()
            }
          />

          {/* Decorative stripes */}
          <div
            className="absolute inset-0 opacity-10 pointer-events-none"
            style={{
              backgroundImage: `repeating-linear-gradient(
                ${orientation === 'vertical' ? '0deg' : '45deg'},
                transparent,
                transparent 3px,
                currentColor 3px,
                currentColor 4px
              )`,
            }}
          />
        </div>

        {/* Thumbs */}
        {springs.map((spring, index) => (
          <div
            key={`thumb-${index}`}
            role="slider"
            tabIndex={disabled ? -1 : 0}
            aria-valuemin={min}
            aria-valuemax={max}
            aria-valuenow={actualValue[index]}
            aria-valuetext={`${actualValue[index]} of ${max}`}
            aria-disabled={disabled}
            aria-orientation={orientation}
            onKeyDown={handleKeyDown(index)}
            onPointerDown={handleThumbPointerDown(index)}
            onMouseEnter={() => setHoveringThumb(index)}
            onMouseLeave={() => setHoveringThumb(null)}
            className={cn(
              'absolute h-7 w-7 cursor-grab',
              'border-3 border-foreground bg-background',
              'shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
              'focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2',
              'transition-shadow duration-150',
              activeThumb === index && 'cursor-grabbing shadow-[2px_2px_0px_hsl(var(--shadow-color))] z-10',
              hoveringThumb === index && activeThumb !== index && 'shadow-[5px_5px_0px_hsl(var(--shadow-color))]'
            )}
            style={
              orientation === 'vertical'
                ? {
                    bottom: `calc(${spring.position}% - 14px)`,
                    left: '50%',
                    transform: `
                      translateX(-50%)
                      scaleX(${squishes[index]?.scaleX ?? 1})
                      scaleY(${squishes[index]?.scaleY ?? 1})
                      rotate(${((squishes[index]?.scaleX ?? 1) - 1) * 8}deg)
                    `,
                  }
                : {
                    left: `calc(${spring.position}% - 14px)`,
                    top: '50%',
                    transform: `
                      translateY(-50%)
                      scaleX(${squishes[index]?.scaleX ?? 1})
                      scaleY(${squishes[index]?.scaleY ?? 1})
                      rotate(${((squishes[index]?.scaleX ?? 1) - 1) * 8}deg)
                    `,
                  }
            }
          >
            {/* Inner border detail */}
            <div className="absolute inset-1 border-2 border-foreground/20" />
            {/* Grip lines */}
            <div className="absolute inset-0 flex items-center justify-center">
              <div className={cn(
                'flex gap-0.5',
                orientation === 'vertical' ? 'flex-row' : 'flex-col'
              )}>
                <div className={cn(
                  'bg-foreground/30',
                  orientation === 'vertical' ? 'w-0.5 h-3' : 'w-3 h-0.5'
                )} />
                <div className={cn(
                  'bg-foreground/30',
                  orientation === 'vertical' ? 'w-0.5 h-3' : 'w-3 h-0.5'
                )} />
                <div className={cn(
                  'bg-foreground/30',
                  orientation === 'vertical' ? 'w-0.5 h-3' : 'w-3 h-0.5'
                )} />
              </div>
            </div>
          </div>
        ))}
      </div>
    )
  }
)
Slider.displayName = 'Slider'

export { Slider }
```

---

### Sonner <a name="sonner"></a>

An opinionated toast component with neubrutalism styling

- **Registry Name**: `@boldkit/sonner`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/sonner.json
```

#### Dependencies

- **NPM Packages**:
  - `sonner`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/use-theme`
  - `@boldkit/button`

#### How to Use

##### File: `registry/default/example/sonner-demo.tsx`

```tsx
import { toast } from 'sonner'
import { Button } from '@/components/ui/button'

export default function Example() {
  return (
    <Button
      onClick={() => toast('Event has been created')}
    >
      Show Toast
    </Button>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/sonner.tsx`

```tsx
import { useTheme } from '@/hooks/use-theme'
import { Toaster as Sonner } from 'sonner'

type ToasterProps = React.ComponentProps<typeof Sonner>

const Toaster = ({ ...props }: ToasterProps) => {
  const { resolvedTheme } = useTheme()

  return (
    <Sonner
      theme={resolvedTheme as ToasterProps['theme']}
      className="toaster group"
      toastOptions={{
        classNames: {
          toast:
            'group toast group-[.toaster]:bg-background group-[.toaster]:text-foreground group-[.toaster]:border-3 group-[.toaster]:border-foreground group-[.toaster]:shadow-[4px_4px_0px_hsl(var(--shadow-color))] group-[.toaster]:rounded-none group-[.toaster]:font-bold',
          title: 'group-[.toast]:font-black group-[.toast]:uppercase group-[.toast]:tracking-wide',
          description: 'group-[.toast]:text-muted-foreground group-[.toast]:font-medium',
          actionButton:
            'group-[.toast]:bg-primary group-[.toast]:text-primary-foreground group-[.toast]:border-2 group-[.toast]:border-foreground group-[.toast]:font-bold group-[.toast]:uppercase group-[.toast]:rounded-none group-[.toast]:shadow-[2px_2px_0px_hsl(var(--foreground))] group-[.toast]:hover:translate-x-[2px] group-[.toast]:hover:translate-y-[2px] group-[.toast]:hover:shadow-none group-[.toast]:transition-all',
          cancelButton:
            'group-[.toast]:bg-muted group-[.toast]:text-muted-foreground group-[.toast]:border-2 group-[.toast]:border-foreground group-[.toast]:font-bold group-[.toast]:uppercase group-[.toast]:rounded-none',
          success:
            'group-[.toaster]:bg-success group-[.toaster]:text-success-foreground group-[.toaster]:border-foreground',
          error:
            'group-[.toaster]:bg-destructive group-[.toaster]:text-destructive-foreground group-[.toaster]:border-foreground',
          warning:
            'group-[.toaster]:bg-warning group-[.toaster]:text-warning-foreground group-[.toaster]:border-foreground',
          info:
            'group-[.toaster]:bg-info group-[.toaster]:text-info-foreground group-[.toaster]:border-foreground',
          closeButton:
            'group-[.toast]:border-2 group-[.toast]:border-foreground group-[.toast]:bg-background group-[.toast]:text-foreground group-[.toast]:hover:bg-muted group-[.toast]:rounded-none',
        },
      }}
      {...props}
    />
  )
}

export { Toaster }
```

---

### Sparkline <a name="sparkline"></a>

Tiny inline trend charts for tables and stat cards - line, area, and bar variants

- **Registry Name**: `@boldkit/sparkline`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/sparkline.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/sparkline-demo.tsx`

```tsx
import { Sparkline } from '@/components/ui/sparkline'

export default function Example() {
  const data = [10, 15, 8, 20, 14, 25, 18, 30]

  return <Sparkline data={data} />
}
```

#### Component Source Code

##### File: `registry/default/ui/sparkline.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'
import {
  Area,
  AreaChart,
  Bar,
  BarChart,
  Line,
  LineChart,
  ResponsiveContainer,
} from 'recharts'

export interface SparklineProps extends React.HTMLAttributes<HTMLDivElement> {
  data: number[]
  type?: 'line' | 'area' | 'bar'
  color?: string
  height?: number
  width?: number | string
  showEndDot?: boolean
  strokeWidth?: number
  trend?: 'up' | 'down' | 'neutral'
  animated?: boolean
}

const Sparkline = React.forwardRef<HTMLDivElement, SparklineProps>(
  (
    {
      data,
      type = 'line',
      color,
      height = 32,
      width = '100%',
      showEndDot = false,
      strokeWidth = 2,
      trend,
      animated = true,
      className,
      ...props
    },
    ref
  ) => {
    // Unique ID per instance prevents gradient collision when multiple sparklines render on the same page
    const uid = React.useId().replace(/:/g, '')

    if (!data || data.length === 0) {
      return (
        <div
          ref={ref}
          aria-label="No data"
          className={cn('inline-block border-b-2 border-dashed border-foreground/30', className)}
          style={{ width, height }}
          {...props}
        />
      )
    }

    // Convert data array to format recharts expects
    const chartData = data.map((value, index) => ({ value, index }))

    // Determine color based on trend or explicit color
    const resolvedColor = React.useMemo(() => {
      if (color) return color
      if (trend === 'up') return 'hsl(var(--success))'
      if (trend === 'down') return 'hsl(var(--destructive))'
      return 'hsl(var(--primary))'
    }, [color, trend])

    const strokeColor = 'hsl(var(--foreground))'
    const lastIndex = data.length - 1

    /**
     * Custom dot renderer that only draws a circle on the final data point.
     * Used directly on the primary series — no duplicate series needed.
     */
    // eslint-disable-next-line @typescript-eslint/no-explicit-any
    const endDotRenderer = (dotProps: any) => {
      const { cx, cy, index } = dotProps
      if (!showEndDot || index !== lastIndex) return null
      return (
        <circle
          key="end-dot"
          cx={cx}
          cy={cy}
          r={4}
          fill={resolvedColor}
          stroke={strokeColor}
          strokeWidth={2}
        />
      )
    }

    if (type === 'bar') {
      return (
        <div
          ref={ref}
          className={cn('inline-block', className)}
          style={{ width, height }}
          {...props}
        >
          <ResponsiveContainer width="100%" height="100%">
            <BarChart data={chartData} margin={{ top: 0, right: 0, bottom: 0, left: 0 }}>
              <Bar
                dataKey="value"
                fill={resolvedColor}
                stroke={strokeColor}
                strokeWidth={1}
                isAnimationActive={animated}
                animationDuration={300}
              />
            </BarChart>
          </ResponsiveContainer>
        </div>
      )
    }

    if (type === 'area') {
      return (
        <div
          ref={ref}
          className={cn('inline-block', className)}
          style={{ width, height }}
          {...props}
        >
          <ResponsiveContainer width="100%" height="100%">
            <AreaChart data={chartData} margin={{ top: 0, right: 0, bottom: 0, left: 0 }}>
              <defs>
                <linearGradient id={`sparkline-gradient-${uid}`} x1="0" y1="0" x2="0" y2="1">
                  <stop offset="0%" stopColor={resolvedColor} stopOpacity={0.6} />
                  <stop offset="100%" stopColor={resolvedColor} stopOpacity={0.1} />
                </linearGradient>
              </defs>
              <Area
                type="monotone"
                dataKey="value"
                stroke={resolvedColor}
                strokeWidth={strokeWidth}
                fill={`url(#sparkline-gradient-${uid})`}
                isAnimationActive={animated}
                animationDuration={300}
                dot={endDotRenderer}
                activeDot={false}
              />
            </AreaChart>
          </ResponsiveContainer>
        </div>
      )
    }

    // Default: line
    return (
      <div
        ref={ref}
        className={cn('inline-block', className)}
        style={{ width, height }}
        {...props}
      >
        <ResponsiveContainer width="100%" height="100%">
          <LineChart data={chartData} margin={{ top: 2, right: 2, bottom: 2, left: 2 }}>
            <Line
              type="monotone"
              dataKey="value"
              stroke={resolvedColor}
              strokeWidth={strokeWidth}
              dot={endDotRenderer}
              activeDot={false}
              isAnimationActive={animated}
              animationDuration={300}
            />
          </LineChart>
        </ResponsiveContainer>
      </div>
    )
  }
)
Sparkline.displayName = 'Sparkline'

export { Sparkline }
```

---

### Spinner <a name="spinner"></a>

Loading spinner with multiple animation variants - dots, bars, blocks, and brutal shadow spin

- **Registry Name**: `@boldkit/spinner`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/spinner.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/spinner-demo.tsx`

```tsx
import { Spinner } from '@/components/ui/spinner'

export default function Example() {
  return <Spinner />
}
```

#### Component Source Code

##### File: `registry/default/ui/spinner.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'

const spinnerVariants = cva(
  'inline-flex items-center justify-center',
  {
    variants: {
      size: {
        xs: 'h-3 w-3',
        sm: 'h-4 w-4',
        md: 'h-6 w-6',
        lg: 'h-8 w-8',
        xl: 'h-12 w-12',
      },
      variant: {
        default: '',
        dots: '',
        bars: '',
        blocks: '',
        brutal: '',
      },
    },
    defaultVariants: {
      size: 'md',
      variant: 'default',
    },
  }
)

export interface SpinnerProps
  extends React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof spinnerVariants> {}

const Spinner = React.forwardRef<HTMLDivElement, SpinnerProps>(
  ({ className, size, variant, ...props }, ref) => {
    const sizeClasses = {
      xs: { element: 'h-3 w-3', dot: 'h-1 w-1', bar: 'w-0.5 h-2', block: 'h-1 w-1', ring: 'h-3 w-3' },
      sm: { element: 'h-4 w-4', dot: 'h-1 w-1', bar: 'w-0.5 h-2.5', block: 'h-1.5 w-1.5', ring: 'h-4 w-4' },
      md: { element: 'h-6 w-6', dot: 'h-1.5 w-1.5', bar: 'w-1 h-4', block: 'h-2 w-2', ring: 'h-6 w-6' },
      lg: { element: 'h-8 w-8', dot: 'h-2 w-2', bar: 'w-1 h-5', block: 'h-2.5 w-2.5', ring: 'h-8 w-8' },
      xl: { element: 'h-12 w-12', dot: 'h-3 w-3', bar: 'w-1.5 h-8', block: 'h-4 w-4', ring: 'h-12 w-12' },
    }

    const currentSize = size ?? 'md'
    const sizes = sizeClasses[currentSize]

    if (variant === 'dots') {
      return (
        <div
          ref={ref}
          role="status"
          aria-label="Loading"
          className={cn(spinnerVariants({ size, variant }), 'gap-1', className)}
          {...props}
        >
          <div className={cn(sizes.dot, 'bg-foreground animate-[brutal-dots_1.4s_ease-in-out_infinite]')} style={{ animationDelay: '0ms' }} />
          <div className={cn(sizes.dot, 'bg-foreground animate-[brutal-dots_1.4s_ease-in-out_infinite]')} style={{ animationDelay: '200ms' }} />
          <div className={cn(sizes.dot, 'bg-foreground animate-[brutal-dots_1.4s_ease-in-out_infinite]')} style={{ animationDelay: '400ms' }} />
        </div>
      )
    }

    if (variant === 'bars') {
      return (
        <div
          ref={ref}
          role="status"
          aria-label="Loading"
          className={cn(spinnerVariants({ size, variant }), 'gap-0.5', className)}
          {...props}
        >
          <div className={cn(sizes.bar, 'bg-foreground animate-[brutal-bars_1.2s_ease-in-out_infinite]')} style={{ animationDelay: '0ms' }} />
          <div className={cn(sizes.bar, 'bg-foreground animate-[brutal-bars_1.2s_ease-in-out_infinite]')} style={{ animationDelay: '150ms' }} />
          <div className={cn(sizes.bar, 'bg-foreground animate-[brutal-bars_1.2s_ease-in-out_infinite]')} style={{ animationDelay: '300ms' }} />
        </div>
      )
    }

    if (variant === 'blocks') {
      return (
        <div
          ref={ref}
          role="status"
          aria-label="Loading"
          className={cn(spinnerVariants({ size, variant }), 'relative', className)}
          {...props}
        >
          <div className="relative animate-[brutal-blocks_1.2s_linear_infinite]">
            <div className={cn(sizes.block, 'absolute bg-foreground')} style={{ top: 0, left: '50%', transform: 'translateX(-50%)' }} />
            <div className={cn(sizes.block, 'absolute bg-foreground')} style={{ top: '50%', right: 0, transform: 'translateY(-50%)' }} />
            <div className={cn(sizes.block, 'absolute bg-foreground')} style={{ bottom: 0, left: '50%', transform: 'translateX(-50%)' }} />
            <div className={cn(sizes.block, 'absolute bg-foreground')} style={{ top: '50%', left: 0, transform: 'translateY(-50%)' }} />
          </div>
        </div>
      )
    }

    if (variant === 'brutal') {
      return (
        <div
          ref={ref}
          role="status"
          aria-label="Loading"
          className={cn(spinnerVariants({ size, variant }), className)}
          {...props}
        >
          <div className={cn(sizes.ring, 'relative')}>
            <div className="absolute inset-0 bg-foreground border-3 border-foreground" />
            <div className="absolute inset-0 bg-foreground animate-[brutal-shadow-spin_1s_linear_infinite]" style={{ boxShadow: '3px 3px 0px hsl(var(--primary))' }} />
          </div>
        </div>
      )
    }

    // Default ring spinner
    return (
      <div
        ref={ref}
        role="status"
        aria-label="Loading"
        className={cn(spinnerVariants({ size, variant }), className)}
        {...props}
      >
        <svg
          className={cn(sizes.ring, 'animate-spin')}
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
        >
          <circle
            className="opacity-25"
            cx="12"
            cy="12"
            r="10"
            stroke="currentColor"
            strokeWidth="4"
          />
          <path
            className="opacity-75"
            fill="currentColor"
            d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
          />
        </svg>
      </div>
    )
  }
)
Spinner.displayName = 'Spinner'

export { Spinner, spinnerVariants }
```

---

### Stat Card <a name="stat-card"></a>

Statistics card with trend indicators, icons, and optional progress bars

- **Registry Name**: `@boldkit/stat-card`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/stat-card.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/card`
  - `@boldkit/progress`

#### How to Use

##### File: `registry/default/example/stat-card-demo.tsx`

```tsx
import { StatCard } from '@/components/ui/stat-card'
import { DollarSign } from 'lucide-react'

export default function Example() {
  return (
    <StatCard
      title="Total Revenue"
      value="$45,231"
      change="+20.1%"
      trend="up"
      icon={<DollarSign className="h-6 w-6" />}
      color="success"
    />
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/stat-card.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'
import { Card, CardContent } from '@/components/ui/card'
import { Progress } from '@/components/ui/progress'
import { TrendingUp, TrendingDown, Minus } from 'lucide-react'

const statCardVariants = cva(
  'relative overflow-hidden',
  {
    variants: {
      variant: {
        default: '',
        compact: '[&_.stat-content]:p-4',
        large: 'md:col-span-2',
      },
      colorScheme: {
        primary: '[&_.stat-icon]:bg-primary [&_.stat-bg]:bg-primary',
        secondary: '[&_.stat-icon]:bg-secondary [&_.stat-bg]:bg-secondary',
        accent: '[&_.stat-icon]:bg-accent [&_.stat-bg]:bg-accent',
        success: '[&_.stat-icon]:bg-success [&_.stat-bg]:bg-success',
        warning: '[&_.stat-icon]:bg-warning [&_.stat-bg]:bg-warning',
        info: '[&_.stat-icon]:bg-info [&_.stat-bg]:bg-info',
        destructive: '[&_.stat-icon]:bg-destructive [&_.stat-bg]:bg-destructive',
      },
    },
    defaultVariants: {
      variant: 'default',
      colorScheme: 'primary',
    },
  }
)

export interface StatCardProps
  extends Omit<React.HTMLAttributes<HTMLDivElement>, 'title' | 'color'>,
    VariantProps<typeof statCardVariants> {
  title: string
  value: string | number
  change?: string
  trend?: 'up' | 'down' | 'neutral'
  icon?: React.ReactNode
  progress?: { value: number; label?: string }
  comparison?: string
  /** @deprecated Use colorScheme instead */
  color?: 'primary' | 'secondary' | 'accent' | 'success' | 'warning' | 'info' | 'destructive'
}

const StatCard = React.forwardRef<HTMLDivElement, StatCardProps>(
  (
    {
      className,
      variant,
      colorScheme,
      color,
      title,
      value,
      change,
      trend = 'neutral',
      icon,
      progress,
      comparison = 'vs last month',
      ...props
    },
    ref
  ) => {
    // Support legacy color prop
    const resolvedColorScheme = colorScheme ?? color ?? 'primary'

    const TrendIcon = trend === 'up' ? TrendingUp : trend === 'down' ? TrendingDown : Minus
    const trendColor = trend === 'up' ? 'text-success' : trend === 'down' ? 'text-destructive' : 'text-muted-foreground'

    return (
      <Card
        ref={ref}
        className={cn(statCardVariants({ variant, colorScheme: resolvedColorScheme }), className)}
        {...props}
      >
        {/* Decorative background element */}
        <div className="stat-bg absolute top-0 right-0 w-24 h-24 opacity-20 -translate-y-8 translate-x-8 rotate-12" />

        <CardContent className="stat-content p-6">
          <div className="flex items-start justify-between">
            <div>
              <p className="text-sm font-bold uppercase tracking-wide text-muted-foreground mb-1">
                {title}
              </p>
              <p className="text-3xl font-black">{value}</p>
              {change && (
                <div className={cn('flex items-center gap-1 mt-2', trendColor)}>
                  <TrendIcon className="h-4 w-4" />
                  <span className="text-sm font-bold">{change}</span>
                  <span className="text-sm text-muted-foreground">{comparison}</span>
                </div>
              )}
            </div>
            {icon && (
              <div className="stat-icon w-12 h-12 border-3 border-foreground flex items-center justify-center shadow-[3px_3px_0px_hsl(var(--shadow-color))] text-foreground">
                {icon}
              </div>
            )}
          </div>

          {/* Progress section */}
          {progress && (
            <div className="mt-4 pt-4 border-t-2 border-foreground/10">
              <div className="flex items-center justify-between text-sm mb-2">
                <span className="text-muted-foreground">
                  {progress.label || 'Progress'}
                </span>
                <span className="font-bold">{progress.value}%</span>
              </div>
              <Progress value={progress.value} className="h-3" />
            </div>
          )}
        </CardContent>
      </Card>
    )
  }
)
StatCard.displayName = 'StatCard'

export { StatCard, statCardVariants }
```

---

### Stepper <a name="stepper"></a>

Multi-step form wizard with progress indicators, compound components pattern

- **Registry Name**: `@boldkit/stepper`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/stepper.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/stepper-demo.tsx`

```tsx
import {
  Stepper,
  StepperList,
  StepperItem,
  StepperTrigger,
  StepperSeparator,
  StepperContent,
} from '@/components/ui/stepper'

export default function Example() {
  return (
    <Stepper>
      <StepperList>
        <StepperItem index={0}>
          <StepperTrigger />
        </StepperItem>
        <StepperSeparator />
        <StepperItem index={1}>
          <StepperTrigger />
        </StepperItem>
        <StepperSeparator />
        <StepperItem index={2}>
          <StepperTrigger />
        </StepperItem>
      </StepperList>
      <StepperContent index={0}>Step 1 content</StepperContent>
      <StepperContent index={1}>Step 2 content</StepperContent>
      <StepperContent index={2}>Step 3 content</StepperContent>
    </Stepper>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/stepper.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'
import { Check } from 'lucide-react'

// Context
interface StepperContextValue {
  activeStep: number
  setActiveStep: (step: number) => void
  totalSteps: number
  orientation: 'horizontal' | 'vertical'
}

const StepperContext = React.createContext<StepperContextValue | null>(null)

function useStepperContext() {
  const context = React.useContext(StepperContext)
  if (!context) {
    throw new Error('Stepper components must be used within a <Stepper />')
  }
  return context
}

// Step variants
const stepVariants = cva(
  'flex items-center justify-center border-3 border-foreground font-bold transition-all duration-200',
  {
    variants: {
      state: {
        completed: 'bg-success text-success-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        active: 'bg-primary text-primary-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))] scale-110',
        upcoming: 'bg-muted text-muted-foreground',
      },
      size: {
        sm: 'h-8 w-8 text-sm',
        md: 'h-10 w-10',
        lg: 'h-12 w-12 text-lg',
      },
    },
    defaultVariants: {
      state: 'upcoming',
      size: 'md',
    },
  }
)

// Stepper Root
export interface StepperProps extends React.HTMLAttributes<HTMLDivElement> {
  activeStep?: number
  onStepChange?: (step: number) => void
  orientation?: 'horizontal' | 'vertical'
}

const Stepper = React.forwardRef<HTMLDivElement, StepperProps>(
  (
    {
      activeStep: controlledActiveStep,
      onStepChange,
      orientation = 'horizontal',
      className,
      children,
      ...props
    },
    ref
  ) => {
    const [uncontrolledActiveStep, setUncontrolledActiveStep] = React.useState(0)

    const isControlled = controlledActiveStep !== undefined
    const activeStep = isControlled ? controlledActiveStep : uncontrolledActiveStep

    const setActiveStep = React.useCallback(
      (step: number) => {
        if (!isControlled) {
          setUncontrolledActiveStep(step)
        }
        onStepChange?.(step)
      },
      [isControlled, onStepChange]
    )

    // Count total steps from children
    const totalSteps = React.Children.toArray(children).filter((child) => {
      if (!React.isValidElement(child)) return false
      return (child.type as typeof StepperItem & { _isBoldKitStepperItem?: boolean })._isBoldKitStepperItem === true
    }).length

    return (
      <StepperContext.Provider value={{ activeStep, setActiveStep, totalSteps, orientation }}>
        <div
          ref={ref}
          className={cn(
            'flex',
            orientation === 'horizontal' ? 'flex-row' : 'flex-col',
            className
          )}
          {...props}
        >
          {children}
        </div>
      </StepperContext.Provider>
    )
  }
)
Stepper.displayName = 'Stepper'

// Stepper List (container for triggers)
export type StepperListProps = React.HTMLAttributes<HTMLDivElement>

const StepperList = React.forwardRef<HTMLDivElement, StepperListProps>(
  ({ className, children, ...props }, ref) => {
    const { orientation } = useStepperContext()

    return (
      <div
        ref={ref}
        role="tablist"
        className={cn(
          'flex items-center',
          orientation === 'horizontal' ? 'flex-row' : 'flex-col items-start',
          className
        )}
        {...props}
      >
        {children}
      </div>
    )
  }
)
StepperList.displayName = 'StepperList'

// Stepper Item (wrapper for a single step)
interface StepperItemContextValue {
  index: number
  triggerId: string
}

const StepperItemContext = React.createContext<StepperItemContextValue | null>(null)

function useStepperItemContext() {
  const context = React.useContext(StepperItemContext)
  if (!context) {
    throw new Error('StepperItem components must be used within a <StepperItem />')
  }
  return context
}

export interface StepperItemProps extends React.HTMLAttributes<HTMLDivElement> {
  index: number
}

const StepperItem = React.forwardRef<HTMLDivElement, StepperItemProps>(
  ({ index, className, children, ...props }, ref) => {
    const { orientation } = useStepperContext()

    const triggerId = `stepper-trigger-${index}`

    return (
      <StepperItemContext.Provider value={{ index, triggerId }}>
        <div
          ref={ref}
          className={cn(
            'flex items-center',
            orientation === 'horizontal' ? 'flex-row' : 'flex-col',
            className
          )}
          {...props}
        >
          {children}
        </div>
      </StepperItemContext.Provider>
    )
  }
)
StepperItem.displayName = 'StepperItem'
;(StepperItem as typeof StepperItem & { _isBoldKitStepperItem: boolean })._isBoldKitStepperItem = true

// Stepper Trigger (the clickable step indicator)
export interface StepperTriggerProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof stepVariants> {
  showStepNumber?: boolean
}

const StepperTrigger = React.forwardRef<HTMLButtonElement, StepperTriggerProps>(
  ({ size, showStepNumber = true, className, children, ...props }, ref) => {
    const { activeStep, setActiveStep } = useStepperContext()
    const { index, triggerId } = useStepperItemContext()

    const state: 'completed' | 'active' | 'upcoming' =
      index < activeStep ? 'completed' : index === activeStep ? 'active' : 'upcoming'

    return (
      <button
        ref={ref}
        id={triggerId}
        type="button"
        role="tab"
        aria-selected={state === 'active'}
        onClick={() => setActiveStep(index)}
        className={cn(stepVariants({ state, size }), className)}
        {...props}
      >
        {state === 'completed' ? (
          <Check className="h-5 w-5" />
        ) : showStepNumber ? (
          index + 1
        ) : (
          children
        )}
      </button>
    )
  }
)
StepperTrigger.displayName = 'StepperTrigger'

// Stepper Separator (line between steps)
export type StepperSeparatorProps = React.HTMLAttributes<HTMLDivElement>

const StepperSeparator = React.forwardRef<HTMLDivElement, StepperSeparatorProps>(
  ({ className, ...props }, ref) => {
    const { activeStep, orientation } = useStepperContext()
    const { index } = useStepperItemContext()

    const isCompleted = index < activeStep

    return (
      <div
        ref={ref}
        className={cn(
          'transition-all duration-200',
          orientation === 'horizontal'
            ? 'h-[3px] flex-1 min-w-8 mx-2'
            : 'w-[3px] min-h-8 my-2 ml-5',
          isCompleted
            ? 'bg-foreground'
            : 'bg-foreground/30 border-foreground/30',
          !isCompleted && 'border-dashed border-2 bg-transparent',
          className
        )}
        {...props}
      />
    )
  }
)
StepperSeparator.displayName = 'StepperSeparator'

// Stepper Content (panel for each step)
export interface StepperContentProps extends React.HTMLAttributes<HTMLDivElement> {
  index: number
}

const StepperContent = React.forwardRef<HTMLDivElement, StepperContentProps>(
  ({ index, className, children, ...props }, ref) => {
    const { activeStep } = useStepperContext()

    if (index !== activeStep) {
      return null
    }

    return (
      <div
        ref={ref}
        role="tabpanel"
        aria-labelledby={`stepper-trigger-${index}`}
        className={cn(
          'mt-4 animate-[slide-in-from-bottom_200ms_ease-out]',
          className
        )}
        {...props}
      >
        {children}
      </div>
    )
  }
)
StepperContent.displayName = 'StepperContent'

// Stepper Actions (prev/next buttons helper)
export interface StepperActionsProps extends React.HTMLAttributes<HTMLDivElement> {
  onComplete?: () => void
  prevLabel?: string
  nextLabel?: string
  completeLabel?: string
}

const StepperActions = React.forwardRef<HTMLDivElement, StepperActionsProps>(
  (
    {
      onComplete,
      prevLabel = 'Previous',
      nextLabel = 'Next',
      completeLabel = 'Complete',
      className,
      children,
      ...props
    },
    ref
  ) => {
    const { activeStep, setActiveStep, totalSteps } = useStepperContext()

    const isFirst = activeStep === 0
    const isLast = activeStep === totalSteps - 1

    const handlePrev = () => {
      if (!isFirst) {
        setActiveStep(activeStep - 1)
      }
    }

    const handleNext = () => {
      if (isLast) {
        onComplete?.()
      } else {
        setActiveStep(activeStep + 1)
      }
    }

    return (
      <div
        ref={ref}
        className={cn('flex items-center gap-2 mt-4', className)}
        {...props}
      >
        {children || (
          <>
            <button
              type="button"
              onClick={handlePrev}
              disabled={isFirst}
              className={cn(
                'px-4 py-2 border-3 border-foreground font-bold uppercase text-sm',
                'bg-muted shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
                'hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none',
                'disabled:opacity-50 disabled:pointer-events-none',
                'transition-all duration-200'
              )}
            >
              {prevLabel}
            </button>
            <button
              type="button"
              onClick={handleNext}
              className={cn(
                'px-4 py-2 border-3 border-foreground font-bold uppercase text-sm',
                'bg-primary text-primary-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
                'hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none',
                'transition-all duration-200'
              )}
            >
              {isLast ? completeLabel : nextLabel}
            </button>
          </>
        )}
      </div>
    )
  }
)
StepperActions.displayName = 'StepperActions'

export {
  Stepper,
  StepperList,
  StepperItem,
  StepperTrigger,
  StepperSeparator,
  StepperContent,
  StepperActions,
  stepVariants,
  useStepperContext,
}
```

---

### Sticker <a name="sticker"></a>

Rotated labels and stamps with tape effect and double borders - neubrutalist decorative elements

- **Registry Name**: `@boldkit/sticker`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/sticker.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/sticker-demo.tsx`

```tsx
import { Sticker, Stamp } from '@/components/ui/sticker'

export default function Example() {
  return (
    <div className="flex gap-8 items-center">
      <Sticker>New</Sticker>
      <Stamp>Approved</Stamp>
    </div>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/sticker.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'

const stickerVariants = cva(
  'relative inline-flex items-center justify-center border-3 border-foreground font-bold uppercase tracking-wide transition-transform',
  {
    variants: {
      variant: {
        default: 'bg-accent text-accent-foreground',
        primary: 'bg-primary text-primary-foreground',
        secondary: 'bg-secondary text-secondary-foreground',
        destructive: 'bg-destructive text-destructive-foreground',
        outline: 'bg-background text-foreground',
        neon: 'bg-[#ff2d78] text-white',
      },
      size: {
        sm: 'px-2 py-1 text-xs',
        default: 'px-3 py-1.5 text-sm',
        lg: 'px-4 py-2 text-base',
        xl: 'px-6 py-3 text-lg',
      },
      rotation: {
        none: 'rotate-0',
        slight: '-rotate-2',
        medium: '-rotate-6',
        heavy: '-rotate-12',
        'slight-right': 'rotate-2',
        'medium-right': 'rotate-6',
        'heavy-right': 'rotate-12',
      },
      shadow: {
        none: '',
        default: 'shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        colored:
          'shadow-[4px_4px_0px_hsl(var(--primary))]',
        double:
          'shadow-[3px_3px_0px_hsl(var(--primary)),6px_6px_0px_hsl(var(--shadow-color))]',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
      rotation: 'slight',
      shadow: 'default',
    },
  }
)

export interface StickerProps
  extends React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof stickerVariants> {
  /** Show dashed outline effect */
  dashed?: boolean
  /** Show tape decoration on top */
  tape?: boolean
  /** Interactive hover effect */
  interactive?: boolean
}

const Sticker = React.forwardRef<HTMLDivElement, StickerProps>(
  (
    {
      className,
      variant,
      size,
      rotation,
      shadow,
      dashed = false,
      tape = false,
      interactive = false,
      children,
      ...props
    },
    ref
  ) => {
    return (
      <div
        ref={ref}
        className={cn(
          stickerVariants({ variant, size, rotation, shadow }),
          dashed && 'before:absolute before:inset-[-6px] before:border-2 before:border-dashed before:border-foreground/50',
          tape && 'after:absolute after:left-1/2 after:top-[-8px] after:-translate-x-1/2 after:rotate-[-2deg] after:w-[50px] after:h-[16px] after:bg-accent/80 after:border-2 after:border-foreground',
          interactive && 'cursor-pointer hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none active:translate-x-[4px] active:translate-y-[4px] active:shadow-none',
          className
        )}
        {...props}
      >
        {children}
      </div>
    )
  }
)
Sticker.displayName = 'Sticker'

const stampVariants = cva(
  'relative inline-flex items-center justify-center rounded-full border-4 border-foreground font-black uppercase tracking-widest',
  {
    variants: {
      variant: {
        default: 'bg-primary text-primary-foreground',
        secondary: 'bg-secondary text-secondary-foreground',
        accent: 'bg-accent text-accent-foreground',
        destructive: 'bg-destructive text-destructive-foreground',
        outline: 'bg-background text-foreground',
      },
      size: {
        sm: 'h-16 w-16 text-[10px]',
        default: 'h-24 w-24 text-xs',
        lg: 'h-32 w-32 text-sm',
        xl: 'h-40 w-40 text-base',
      },
      rotation: {
        none: 'rotate-0',
        slight: '-rotate-12',
        medium: '-rotate-[25deg]',
        heavy: '-rotate-45',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
      rotation: 'slight',
    },
  }
)

export interface StampProps
  extends React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof stampVariants> {
  /** Show double ring effect */
  doubleRing?: boolean
}

const Stamp = React.forwardRef<HTMLDivElement, StampProps>(
  ({ className, variant, size, rotation, doubleRing = false, children, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(
          stampVariants({ variant, size, rotation }),
          'shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
          doubleRing && 'ring-2 ring-foreground ring-offset-2 ring-offset-background',
          className
        )}
        {...props}
      >
        <span className="text-center leading-tight">{children}</span>
      </div>
    )
  }
)
Stamp.displayName = 'Stamp'

const stickyNoteVariants = cva(
  'relative border-3 border-foreground font-medium shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
  {
    variants: {
      variant: {
        yellow: 'bg-accent text-accent-foreground',
        pink: 'bg-primary text-primary-foreground',
        blue: 'bg-info text-info-foreground',
        green: 'bg-success text-success-foreground',
        purple: 'bg-secondary text-secondary-foreground',
      },
      size: {
        sm: 'p-3 text-sm min-w-[120px]',
        default: 'p-4 text-base min-w-[160px]',
        lg: 'p-6 text-lg min-w-[200px]',
      },
      rotation: {
        none: 'rotate-0',
        left: '-rotate-2',
        right: 'rotate-2',
        'tilt-left': '-rotate-6',
        'tilt-right': 'rotate-6',
      },
    },
    defaultVariants: {
      variant: 'yellow',
      size: 'default',
      rotation: 'left',
    },
  }
)

export interface StickyNoteProps
  extends React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof stickyNoteVariants> {
  /** Show pin decoration on top */
  pin?: boolean
  /** Show folded corner effect */
  folded?: boolean
}

const StickyNote = React.forwardRef<HTMLDivElement, StickyNoteProps>(
  ({ className, variant, size, rotation, pin = false, folded = true, children, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(
          stickyNoteVariants({ variant, size, rotation }),
          folded && 'before:absolute before:bottom-0 before:right-0 before:w-0 before:h-0 before:border-l-[20px] before:border-l-transparent before:border-b-[20px] before:border-b-foreground/20',
          className
        )}
        {...props}
      >
        {pin && (
          <div className="absolute -top-3 left-1/2 -translate-x-1/2 w-4 h-4 rounded-full bg-destructive border-2 border-foreground shadow-[2px_2px_0px_hsl(var(--shadow-color))]" />
        )}
        {children}
      </div>
    )
  }
)
StickyNote.displayName = 'StickyNote'

export { Sticker, stickerVariants, Stamp, stampVariants, StickyNote, stickyNoteVariants }
```

---

### Switch <a name="switch"></a>

A control that allows the user to toggle between on and off

- **Registry Name**: `@boldkit/switch`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/switch.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-switch`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/label`

#### How to Use

##### File: `registry/default/example/switch-demo.tsx`

```tsx
import { Switch } from '@/components/ui/switch'
import { Label } from '@/components/ui/label'

export default function Example() {
  return (
    <div className="flex items-center space-x-2">
      <Switch id="airplane-mode" />
      <Label htmlFor="airplane-mode">Airplane Mode</Label>
    </div>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/switch.tsx`

```tsx
import * as React from 'react'
import * as SwitchPrimitives from '@radix-ui/react-switch'
import { cn } from '@/lib/utils'

const Switch = React.forwardRef<
  React.ElementRef<typeof SwitchPrimitives.Root>,
  React.ComponentPropsWithoutRef<typeof SwitchPrimitives.Root>
>(({ className, ...props }, ref) => (
  <SwitchPrimitives.Root
    className={cn(
      'peer inline-flex h-7 w-12 shrink-0 cursor-pointer items-center border-3 border-foreground bg-muted shadow-[4px_4px_0px_hsl(var(--shadow-color))] transition-all duration-200 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background disabled:cursor-not-allowed disabled:opacity-50 data-[state=checked]:bg-primary hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none active:translate-x-[4px] active:translate-y-[4px] active:shadow-none',
      className
    )}
    {...props}
    ref={ref}
  >
    <SwitchPrimitives.Thumb
      className={cn(
        'pointer-events-none block h-5 w-5 border-2 border-foreground bg-background shadow-sm transition-transform duration-200 data-[state=checked]:translate-x-5 data-[state=unchecked]:translate-x-0.5'
      )}
    />
  </SwitchPrimitives.Root>
))
Switch.displayName = SwitchPrimitives.Root.displayName

export { Switch }
```

---

### Table <a name="table"></a>

A responsive table component with neubrutalism styling

- **Registry Name**: `@boldkit/table`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/table.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/table-demo.tsx`

```tsx
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table'

export default function Example() {
  return (
    <Table>
      <TableHeader>
        <TableRow>
          <TableHead>Name</TableHead>
          <TableHead>Status</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        <TableRow>
          <TableCell>John Doe</TableCell>
          <TableCell>Active</TableCell>
        </TableRow>
      </TableBody>
    </Table>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/table.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'

const Table = React.forwardRef<
  HTMLTableElement,
  React.HTMLAttributes<HTMLTableElement>
>(({ className, ...props }, ref) => (
  <div className="relative w-full overflow-auto">
    <table
      ref={ref}
      className={cn('w-full caption-bottom text-sm border-3 border-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))]', className)}
      {...props}
    />
  </div>
))
Table.displayName = 'Table'

const TableHeader = React.forwardRef<
  HTMLTableSectionElement,
  React.HTMLAttributes<HTMLTableSectionElement>
>(({ className, ...props }, ref) => (
  <thead ref={ref} className={cn('bg-muted [&_tr]:border-b-3', className)} {...props} />
))
TableHeader.displayName = 'TableHeader'

const TableBody = React.forwardRef<
  HTMLTableSectionElement,
  React.HTMLAttributes<HTMLTableSectionElement>
>(({ className, ...props }, ref) => (
  <tbody
    ref={ref}
    className={cn('[&_tr:last-child]:border-0', className)}
    {...props}
  />
))
TableBody.displayName = 'TableBody'

const TableFooter = React.forwardRef<
  HTMLTableSectionElement,
  React.HTMLAttributes<HTMLTableSectionElement>
>(({ className, ...props }, ref) => (
  <tfoot
    ref={ref}
    className={cn(
      'border-t-3 bg-muted/50 font-bold [&>tr]:last:border-b-0',
      className
    )}
    {...props}
  />
))
TableFooter.displayName = 'TableFooter'

const TableRow = React.forwardRef<
  HTMLTableRowElement,
  React.HTMLAttributes<HTMLTableRowElement>
>(({ className, ...props }, ref) => (
  <tr
    ref={ref}
    className={cn(
      'border-b-3 border-foreground transition-colors hover:bg-muted/50 data-[state=selected]:bg-accent',
      className
    )}
    {...props}
  />
))
TableRow.displayName = 'TableRow'

const TableHead = React.forwardRef<
  HTMLTableCellElement,
  React.ThHTMLAttributes<HTMLTableCellElement>
>(({ className, ...props }, ref) => (
  <th
    ref={ref}
    className={cn(
      'h-12 px-4 text-left align-middle font-bold uppercase tracking-wide text-foreground [&:has([role=checkbox])]:pr-0',
      className
    )}
    {...props}
  />
))
TableHead.displayName = 'TableHead'

const TableCell = React.forwardRef<
  HTMLTableCellElement,
  React.TdHTMLAttributes<HTMLTableCellElement>
>(({ className, ...props }, ref) => (
  <td
    ref={ref}
    className={cn('p-4 align-middle [&:has([role=checkbox])]:pr-0', className)}
    {...props}
  />
))
TableCell.displayName = 'TableCell'

const TableCaption = React.forwardRef<
  HTMLTableCaptionElement,
  React.HTMLAttributes<HTMLTableCaptionElement>
>(({ className, ...props }, ref) => (
  <caption
    ref={ref}
    className={cn('mt-4 text-sm text-muted-foreground', className)}
    {...props}
  />
))
TableCaption.displayName = 'TableCaption'

export {
  Table,
  TableHeader,
  TableBody,
  TableFooter,
  TableHead,
  TableRow,
  TableCell,
  TableCaption,
}
```

---

### Tabs <a name="tabs"></a>

A tabbed interface component with neubrutalism styling

- **Registry Name**: `@boldkit/tabs`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/tabs.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-tabs`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/tabs-demo.tsx`

```tsx
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs'

export default function Example() {
  return (
    <Tabs defaultValue="account">
      <TabsList>
        <TabsTrigger value="account">Account</TabsTrigger>
        <TabsTrigger value="password">Password</TabsTrigger>
      </TabsList>
      <TabsContent value="account">
        Make changes to your account here.
      </TabsContent>
      <TabsContent value="password">
        Change your password here.
      </TabsContent>
    </Tabs>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/tabs.tsx`

```tsx
import * as React from 'react'
import * as TabsPrimitive from '@radix-ui/react-tabs'
import { cn } from '@/lib/utils'

function Tabs({
  className,
  ...props
}: React.ComponentProps<typeof TabsPrimitive.Root>) {
  return (
    <TabsPrimitive.Root
      className={cn('w-full', className)}
      {...props}
    />
  )
}

function TabsList({
  className,
  ...props
}: React.ComponentProps<typeof TabsPrimitive.List>) {
  return (
    <TabsPrimitive.List
      className={cn(
        'inline-flex h-12 items-center justify-center border-3 border-foreground bg-background p-1 text-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        className
      )}
      {...props}
    />
  )
}

function TabsTrigger({
  className,
  ...props
}: React.ComponentProps<typeof TabsPrimitive.Trigger>) {
  return (
    <TabsPrimitive.Trigger
      className={cn(
        'inline-flex items-center justify-center whitespace-nowrap border-2 border-transparent px-4 py-1.5 gap-1.5 text-sm font-bold uppercase tracking-wide transition-all duration-200 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50 data-[state=active]:bg-primary data-[state=active]:text-primary-foreground data-[state=active]:border-foreground',
        className
      )}
      {...props}
    />
  )
}

function TabsContent({
  className,
  ...props
}: React.ComponentProps<typeof TabsPrimitive.Content>) {
  return (
    <TabsPrimitive.Content
      className={cn(
        'mt-4 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2',
        className
      )}
      {...props}
    />
  )
}

export { Tabs, TabsList, TabsTrigger, TabsContent }
```

---

### Tag Input <a name="tag-input"></a>

Multi-tag input with suggestions, validation, and keyboard support

- **Registry Name**: `@boldkit/tag-input`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/tag-input.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/tag-input-demo.tsx`

```tsx
import { TagInput } from '@/components/ui/tag-input'

export default function Example() {
  return <TagInput placeholder="Add tags..." />
}
```

#### Component Source Code

##### File: `registry/default/ui/tag-input.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'
import { X } from 'lucide-react'

export interface TagInputHandle {
  focus: () => void
  blur: () => void
  get value(): string
  set value(v: string)
}

export interface TagInputProps
  extends Omit<React.InputHTMLAttributes<HTMLInputElement>, 'value' | 'defaultValue' | 'onChange'> {
  value?: string[]
  defaultValue?: string[]
  onChange?: (tags: string[]) => void
  suggestions?: string[]
  maxTags?: number
  allowDuplicates?: boolean
  delimiter?: string | RegExp
  validateTag?: (tag: string) => boolean | string
}

const TagInput = React.forwardRef<TagInputHandle, TagInputProps>(
  (
    {
      value: controlledValue,
      defaultValue = [],
      onChange,
      suggestions = [],
      maxTags,
      allowDuplicates = false,
      delimiter = ',',
      validateTag,
      placeholder = 'Add tag...',
      disabled,
      className,
      ...props
    },
    ref
  ) => {
    const [uncontrolledTags, setUncontrolledTags] = React.useState<string[]>(defaultValue)
    const [inputValue, setInputValue] = React.useState('')
    const [showSuggestions, setShowSuggestions] = React.useState(false)
    const [error, setError] = React.useState<string | null>(null)
    const [selectedSuggestionIndex, setSelectedSuggestionIndex] = React.useState(-1)

    const inputRef = React.useRef<HTMLInputElement>(null)
    const containerRef = React.useRef<HTMLDivElement>(null)

    React.useImperativeHandle(ref, () => ({
      focus: () => inputRef.current?.focus(),
      blur: () => inputRef.current?.blur(),
      get value() { return inputRef.current?.value ?? '' },
      set value(v: string) { if (inputRef.current) inputRef.current.value = v },
    }))

    const isControlled = controlledValue !== undefined
    const tags = isControlled ? controlledValue : uncontrolledTags

    const filteredSuggestions = React.useMemo(() => {
      if (!inputValue.trim() || suggestions.length === 0) return []
      const lowerInput = inputValue.toLowerCase()
      return suggestions.filter(
        (suggestion) =>
          suggestion.toLowerCase().includes(lowerInput) &&
          (allowDuplicates || !tags.includes(suggestion))
      )
    }, [inputValue, suggestions, tags, allowDuplicates])

    const updateTags = (newTags: string[]) => {
      if (!isControlled) {
        setUncontrolledTags(newTags)
      }
      onChange?.(newTags)
    }

    const addTag = (tagValue: string) => {
      const trimmedTag = tagValue.trim()
      if (!trimmedTag) return false

      // Check max tags
      if (maxTags && tags.length >= maxTags) {
        setError(`Maximum ${maxTags} tags allowed`)
        return false
      }

      // Check duplicates
      if (!allowDuplicates && tags.includes(trimmedTag)) {
        setError('Tag already exists')
        return false
      }

      // Validate tag
      if (validateTag) {
        const validationResult = validateTag(trimmedTag)
        if (validationResult !== true) {
          setError(typeof validationResult === 'string' ? validationResult : 'Invalid tag')
          return false
        }
      }

      setError(null)
      updateTags([...tags, trimmedTag])
      return true
    }

    const removeTag = (index: number) => {
      if (disabled) return
      const newTags = tags.filter((_, i) => i !== index)
      updateTags(newTags)
      setError(null)
    }

    const handleInputChange = (e: React.ChangeEvent<HTMLInputElement>) => {
      const value = e.target.value
      setInputValue(value)
      setShowSuggestions(true)
      setSelectedSuggestionIndex(-1)
      setError(null)

      // Check for delimiter
      if (delimiter) {
        const parts = value.split(delimiter)

        if (parts.length > 1) {
          const newTags = parts.slice(0, -1).filter((part) => part.trim())
          newTags.forEach((tag) => addTag(tag))
          setInputValue(parts[parts.length - 1])
        }
      }
    }

    const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>) => {
      switch (e.key) {
        case 'Enter':
          e.preventDefault()
          if (selectedSuggestionIndex >= 0 && filteredSuggestions[selectedSuggestionIndex]) {
            if (addTag(filteredSuggestions[selectedSuggestionIndex])) {
              setInputValue('')
              setShowSuggestions(false)
              setSelectedSuggestionIndex(-1)
            }
          } else if (inputValue.trim()) {
            if (addTag(inputValue)) {
              setInputValue('')
            }
          }
          break
        case 'Backspace':
          if (!inputValue && tags.length > 0) {
            removeTag(tags.length - 1)
          }
          break
        case 'ArrowDown':
          if (filteredSuggestions.length > 0) {
            e.preventDefault()
            setSelectedSuggestionIndex((prev) =>
              prev < filteredSuggestions.length - 1 ? prev + 1 : 0
            )
          }
          break
        case 'ArrowUp':
          if (filteredSuggestions.length > 0) {
            e.preventDefault()
            setSelectedSuggestionIndex((prev) =>
              prev > 0 ? prev - 1 : filteredSuggestions.length - 1
            )
          }
          break
        case 'Escape':
          setShowSuggestions(false)
          setSelectedSuggestionIndex(-1)
          break
      }
    }

    const handleSuggestionClick = (suggestion: string) => {
      if (addTag(suggestion)) {
        setInputValue('')
        setShowSuggestions(false)
        inputRef.current?.focus()
      }
    }

    const handleContainerClick = () => {
      inputRef.current?.focus()
    }

    // Close suggestions on click outside
    React.useEffect(() => {
      const handleClickOutside = (e: MouseEvent) => {
        if (containerRef.current && !containerRef.current.contains(e.target as Node)) {
          setShowSuggestions(false)
        }
      }
      document.addEventListener('mousedown', handleClickOutside)
      return () => document.removeEventListener('mousedown', handleClickOutside)
    }, [])

    return (
      <div ref={containerRef} className="relative">
        <div
          onClick={handleContainerClick}
          className={cn(
            'flex flex-wrap items-center gap-2 min-h-11 w-full border-3 border-input bg-background px-3 py-2',
            'shadow-[4px_4px_0px_hsl(var(--shadow-color))] transition-all duration-200',
            'focus-within:translate-x-[4px] focus-within:translate-y-[4px] focus-within:shadow-none',
            disabled && 'opacity-50 cursor-not-allowed',
            error && 'border-destructive',
            className
          )}
        >
          {/* Tags */}
          {tags.map((tag, index) => (
            <span
              key={`tag-${index}`}
              className={cn(
                'inline-flex items-center gap-1 px-2 py-0.5 text-xs font-bold uppercase tracking-wide',
                'border-2 border-foreground bg-primary text-primary-foreground',
                'shadow-[2px_2px_0px_hsl(var(--shadow-color))]'
              )}
            >
              {tag}
              {!disabled && (
                <button
                  type="button"
                  onClick={(e) => {
                    e.stopPropagation()
                    removeTag(index)
                  }}
                  className="hover:bg-primary-foreground/20 rounded-sm p-0.5 transition-colors"
                  aria-label={`Remove ${tag}`}
                >
                  <X className="h-3 w-3" />
                </button>
              )}
            </span>
          ))}

          {/* Input */}
          <input
            ref={inputRef}
            type="text"
            value={inputValue}
            onChange={handleInputChange}
            onKeyDown={handleKeyDown}
            onFocus={() => setShowSuggestions(true)}
            placeholder={tags.length === 0 ? placeholder : ''}
            disabled={disabled}
            className={cn(
              'flex-1 min-w-[120px] bg-transparent outline-none text-sm',
              'placeholder:text-muted-foreground disabled:cursor-not-allowed'
            )}
            {...props}
          />
        </div>

        {/* Error message */}
        {error && (
          <p className="mt-1 text-xs font-medium text-destructive">{error}</p>
        )}

        {/* Suggestions dropdown */}
        {showSuggestions && filteredSuggestions.length > 0 && (
          <div
            className={cn(
              'absolute z-50 mt-1 w-full',
              'border-3 border-foreground bg-popover',
              'shadow-[4px_4px_0px_hsl(var(--shadow-color))]'
            )}
          >
            {filteredSuggestions.map((suggestion, index) => (
              <button
                key={suggestion}
                type="button"
                onClick={() => handleSuggestionClick(suggestion)}
                className={cn(
                  'w-full px-3 py-2 text-left text-sm transition-colors',
                  'hover:bg-muted',
                  index === selectedSuggestionIndex && 'bg-accent'
                )}
              >
                {suggestion}
              </button>
            ))}
          </div>
        )}
      </div>
    )
  }
)
TagInput.displayName = 'TagInput'

export { TagInput }
```

---

### Textarea <a name="textarea"></a>

Displays a form textarea with neubrutalism styling

- **Registry Name**: `@boldkit/textarea`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/textarea.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/textarea-demo.tsx`

```tsx
import { Textarea } from '@/components/ui/textarea'

export default function Example() {
  return <Textarea placeholder="Type your message here." />
}
```

#### Component Source Code

##### File: `registry/default/ui/textarea.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'

const Textarea = React.forwardRef<
  HTMLTextAreaElement,
  React.ComponentProps<'textarea'>
>(({ className, ...props }, ref) => {
  return (
    <textarea
      className={cn(
        'flex min-h-[100px] w-full border-3 border-input bg-background px-4 py-3 text-base shadow-[4px_4px_0px_hsl(var(--shadow-color))] transition-all duration-200 placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-0 focus-visible:translate-x-[4px] focus-visible:translate-y-[4px] focus-visible:shadow-none disabled:cursor-not-allowed disabled:opacity-50 md:text-sm',
        className
      )}
      ref={ref}
      {...props}
    />
  )
})
Textarea.displayName = 'Textarea'

export { Textarea }
```

---

### Time Picker <a name="time-picker"></a>

Popover-based time picker with 12h/24h format and scrollable columns

- **Registry Name**: `@boldkit/time-picker`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/time-picker.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`
  - `@boldkit/popover`
  - `@boldkit/scroll-area`

#### How to Use

##### File: `registry/default/example/time-picker-demo.tsx`

```tsx
import { TimePicker } from '@/components/ui/time-picker'

export default function Example() {
  return <TimePicker />
}
```

#### Component Source Code

##### File: `registry/default/ui/time-picker.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'
import { Clock } from 'lucide-react'
import { Button } from '@/components/ui/button'
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from '@/components/ui/popover'
import { ScrollArea } from '@/components/ui/scroll-area'

export interface TimePickerProps {
  value?: Date
  defaultValue?: Date
  onChange?: (date: Date | undefined) => void
  format?: '12h' | '24h'
  minuteStep?: 1 | 5 | 10 | 15 | 30
  showSeconds?: boolean
  minTime?: Date
  maxTime?: Date
  disabled?: boolean
  placeholder?: string
  className?: string
}

const TimePicker = React.forwardRef<HTMLButtonElement, TimePickerProps>(
  (
    {
      value: controlledValue,
      defaultValue,
      onChange,
      format = '12h',
      minuteStep = 1,
      showSeconds = false,
      minTime,
      maxTime,
      disabled = false,
      placeholder = 'Select time',
      className,
    },
    ref
  ) => {
    const [open, setOpen] = React.useState(false)
    const [uncontrolledValue, setUncontrolledValue] = React.useState<Date | undefined>(defaultValue)

    const isControlled = controlledValue !== undefined
    const selectedTime = isControlled ? controlledValue : uncontrolledValue

    const hours = format === '12h' ? 12 : 24
    const hoursArray = Array.from({ length: hours }, (_, i) => (format === '12h' ? i + 1 : i))
    const minutesArray = Array.from({ length: 60 / minuteStep }, (_, i) => i * minuteStep)
    const secondsArray = Array.from({ length: 60 }, (_, i) => i)

    const getHour = (date: Date) => {
      const h = date.getHours()
      if (format === '12h') {
        return h === 0 ? 12 : h > 12 ? h - 12 : h
      }
      return h
    }

    const getPeriod = (date: Date) => {
      return date.getHours() >= 12 ? 'PM' : 'AM'
    }

    const selectedHour = selectedTime ? getHour(selectedTime) : null
    const selectedMinute = selectedTime ? selectedTime.getMinutes() : null
    const selectedSecond = selectedTime ? selectedTime.getSeconds() : null
    const selectedPeriod = selectedTime ? getPeriod(selectedTime) : 'AM'

    const updateTime = (
      hour?: number,
      minute?: number,
      second?: number,
      period?: 'AM' | 'PM'
    ) => {
      const newDate = new Date(selectedTime || new Date())

      if (hour !== undefined) {
        let h = hour
        if (format === '12h') {
          const currentPeriod = period ?? selectedPeriod
          if (currentPeriod === 'PM' && hour !== 12) h = hour + 12
          else if (currentPeriod === 'AM' && hour === 12) h = 0
        }
        newDate.setHours(h)
      }

      if (minute !== undefined) {
        newDate.setMinutes(minute)
      }

      if (second !== undefined) {
        newDate.setSeconds(second)
      }

      if (period !== undefined && hour === undefined && selectedTime) {
        let h = selectedTime.getHours()
        if (period === 'PM' && h < 12) h += 12
        else if (period === 'AM' && h >= 12) h -= 12
        newDate.setHours(h)
      }

      // Check min/max time
      if (minTime) {
        const minMinutes = minTime.getHours() * 60 + minTime.getMinutes()
        const newMinutes = newDate.getHours() * 60 + newDate.getMinutes()
        if (newMinutes < minMinutes) return
      }

      if (maxTime) {
        const maxMinutes = maxTime.getHours() * 60 + maxTime.getMinutes()
        const newMinutes = newDate.getHours() * 60 + newDate.getMinutes()
        if (newMinutes > maxMinutes) return
      }

      if (!isControlled) {
        setUncontrolledValue(newDate)
      }
      onChange?.(newDate)
    }

    const formatDisplayTime = (date: Date) => {
      const h = getHour(date)
      const m = date.getMinutes().toString().padStart(2, '0')
      const s = date.getSeconds().toString().padStart(2, '0')
      const period = format === '12h' ? ` ${getPeriod(date)}` : ''
      const hourStr = format === '12h' ? h.toString() : h.toString().padStart(2, '0')

      if (showSeconds) {
        return `${hourStr}:${m}:${s}${period}`
      }
      return `${hourStr}:${m}${period}`
    }

    const isTimeDisabled = (hour: number, minute: number) => {
      let h = hour
      if (format === '12h') {
        if (selectedPeriod === 'PM' && hour !== 12) h = hour + 12
        else if (selectedPeriod === 'AM' && hour === 12) h = 0
      }

      const timeMinutes = h * 60 + minute

      if (minTime) {
        const minMinutes = minTime.getHours() * 60 + minTime.getMinutes()
        if (timeMinutes < minMinutes) return true
      }

      if (maxTime) {
        const maxMinutes = maxTime.getHours() * 60 + maxTime.getMinutes()
        if (timeMinutes > maxMinutes) return true
      }

      return false
    }

    // Calculate number of columns for responsive width
    const columnCount = 2 + (showSeconds ? 1 : 0) + (format === '12h' ? 1 : 0)

    return (
      <Popover open={open} onOpenChange={setOpen}>
        <PopoverTrigger asChild>
          <Button
            ref={ref}
            variant="outline"
            disabled={disabled}
            className={cn(
              'w-[180px] justify-start text-left font-normal',
              !selectedTime && 'text-muted-foreground',
              className
            )}
          >
            <Clock className="mr-2 h-4 w-4 shrink-0" />
            <span className="truncate">
              {selectedTime ? formatDisplayTime(selectedTime) : placeholder}
            </span>
          </Button>
        </PopoverTrigger>
        <PopoverContent
          className={cn(
            'w-auto p-0 overflow-hidden',
            'animate-in fade-in-0 zoom-in-95 duration-200'
          )}
          align="start"
          sideOffset={4}
        >
          <div
            className={cn(
              'flex',
              // Responsive: stack on very small screens
              'max-w-[calc(100vw-2rem)]'
            )}
            style={{
              // Dynamic width based on column count
              minWidth: `${columnCount * 60}px`,
            }}
          >
            {/* Hours column */}
            <div className="flex-1 min-w-[60px] border-r-3 border-foreground">
              <div className="px-2 py-2 text-center text-xs font-bold uppercase tracking-wide text-muted-foreground border-b-3 border-foreground bg-muted/30">
                Hour
              </div>
              <ScrollArea className="h-[200px]">
                <div className="p-1">
                  {hoursArray.map((hour) => (
                    <button
                      key={hour}
                      type="button"
                      onClick={() => updateTime(hour)}
                      className={cn(
                        'w-full px-2 py-1.5 text-center text-sm',
                        'transition-all duration-150 ease-out',
                        'hover:bg-muted hover:scale-105',
                        'focus:outline-none focus:bg-muted',
                        selectedHour === hour && 'bg-primary text-primary-foreground shadow-[2px_2px_0px_hsl(var(--shadow-color))] scale-105',
                        isTimeDisabled(hour, selectedMinute ?? 0) && 'opacity-50 cursor-not-allowed hover:scale-100'
                      )}
                    >
                      {format === '12h' ? hour : hour.toString().padStart(2, '0')}
                    </button>
                  ))}
                </div>
              </ScrollArea>
            </div>

            {/* Minutes column */}
            <div className={cn(
              'flex-1 min-w-[60px]',
              (showSeconds || format === '12h') && 'border-r-3 border-foreground'
            )}>
              <div className="px-2 py-2 text-center text-xs font-bold uppercase tracking-wide text-muted-foreground border-b-3 border-foreground bg-muted/30">
                Min
              </div>
              <ScrollArea className="h-[200px]">
                <div className="p-1">
                  {minutesArray.map((minute) => (
                    <button
                      key={minute}
                      type="button"
                      onClick={() => updateTime(undefined, minute)}
                      className={cn(
                        'w-full px-2 py-1.5 text-center text-sm',
                        'transition-all duration-150 ease-out',
                        'hover:bg-muted hover:scale-105',
                        'focus:outline-none focus:bg-muted',
                        selectedMinute === minute && 'bg-primary text-primary-foreground shadow-[2px_2px_0px_hsl(var(--shadow-color))] scale-105'
                      )}
                    >
                      {minute.toString().padStart(2, '0')}
                    </button>
                  ))}
                </div>
              </ScrollArea>
            </div>

            {/* Seconds column */}
            {showSeconds && (
              <div className={cn(
                'flex-1 min-w-[60px]',
                format === '12h' && 'border-r-3 border-foreground'
              )}>
                <div className="px-2 py-2 text-center text-xs font-bold uppercase tracking-wide text-muted-foreground border-b-3 border-foreground bg-muted/30">
                  Sec
                </div>
                <ScrollArea className="h-[200px]">
                  <div className="p-1">
                    {secondsArray.map((second) => (
                      <button
                        key={second}
                        type="button"
                        onClick={() => updateTime(undefined, undefined, second)}
                        className={cn(
                          'w-full px-2 py-1.5 text-center text-sm',
                          'transition-all duration-150 ease-out',
                          'hover:bg-muted hover:scale-105',
                          'focus:outline-none focus:bg-muted',
                          selectedSecond === second && 'bg-primary text-primary-foreground shadow-[2px_2px_0px_hsl(var(--shadow-color))] scale-105'
                        )}
                      >
                        {second.toString().padStart(2, '0')}
                      </button>
                    ))}
                  </div>
                </ScrollArea>
              </div>
            )}

            {/* AM/PM column */}
            {format === '12h' && (
              <div className="flex-1 min-w-[50px]">
                <div className="px-2 py-2 text-center text-xs font-bold uppercase tracking-wide text-muted-foreground border-b-3 border-foreground bg-muted/30">
                  <span className="hidden sm:inline">Period</span>
                  <span className="sm:hidden">AP</span>
                </div>
                <div className="p-1 space-y-1">
                  {(['AM', 'PM'] as const).map((period) => (
                    <button
                      key={period}
                      type="button"
                      onClick={() => updateTime(undefined, undefined, undefined, period)}
                      className={cn(
                        'w-full px-2 py-3 text-center text-sm font-bold',
                        'transition-all duration-150 ease-out',
                        'hover:bg-muted hover:scale-105',
                        'focus:outline-none focus:bg-muted',
                        selectedPeriod === period && 'bg-primary text-primary-foreground shadow-[2px_2px_0px_hsl(var(--shadow-color))] scale-105'
                      )}
                    >
                      {period}
                    </button>
                  ))}
                </div>
              </div>
            )}
          </div>
        </PopoverContent>
      </Popover>
    )
  }
)
TimePicker.displayName = 'TimePicker'

export { TimePicker }
```

---

### Timeline <a name="timeline"></a>

Composable timeline for activity feeds, order tracking, and version history

- **Registry Name**: `@boldkit/timeline`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/timeline.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/timeline-demo.tsx`

```tsx
import {
  Timeline, TimelineItem, TimelineDot, TimelineConnector,
  TimelineContent, TimelineTitle, TimelineDescription
} from '@/components/ui/timeline'

export default function Example() {
  return (
    <Timeline>
      <TimelineItem status="completed">
        <TimelineDot status="completed" />
        <TimelineConnector status="completed" />
        <TimelineContent>
          <TimelineTitle>Order Placed</TimelineTitle>
          <TimelineDescription>Your order has been placed</TimelineDescription>
        </TimelineContent>
      </TimelineItem>
      {/* More items... */}
    </Timeline>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/timeline.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'

// Timeline Context
interface TimelineContextValue {
  orientation: 'vertical' | 'horizontal'
}

const TimelineContext = React.createContext<TimelineContextValue>({
  orientation: 'vertical',
})

function useTimeline() {
  return React.useContext(TimelineContext)
}

// Timeline Root
export interface TimelineProps extends React.HTMLAttributes<HTMLDivElement> {
  orientation?: 'vertical' | 'horizontal'
}

const Timeline = React.forwardRef<HTMLDivElement, TimelineProps>(
  ({ orientation = 'vertical', className, children, ...props }, ref) => {
    return (
      <TimelineContext.Provider value={{ orientation }}>
        <div
          ref={ref}
          className={cn(
            'relative',
            orientation === 'vertical' ? 'flex flex-col' : 'flex flex-row',
            className
          )}
          {...props}
        >
          {children}
        </div>
      </TimelineContext.Provider>
    )
  }
)
Timeline.displayName = 'Timeline'

// Timeline Item
const timelineItemVariants = cva('relative flex')

export interface TimelineItemProps extends React.HTMLAttributes<HTMLDivElement> {
  status?: 'completed' | 'current' | 'upcoming'
}

const TimelineItem = React.forwardRef<HTMLDivElement, TimelineItemProps>(
  ({ status, className, children, ...props }, ref) => {
    const { orientation } = useTimeline()

    return (
      <div
        ref={ref}
        data-status={status}
        className={cn(
          timelineItemVariants(),
          orientation === 'vertical' ? 'flex-row gap-4' : 'flex-col gap-4 items-center',
          className
        )}
        {...props}
      >
        {children}
      </div>
    )
  }
)
TimelineItem.displayName = 'TimelineItem'

// Timeline Dot
const timelineDotVariants = cva(
  'relative z-10 flex items-center justify-center border-3 border-foreground transition-all duration-200',
  {
    variants: {
      status: {
        completed:
          'bg-success shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        current:
          'bg-primary shadow-[4px_4px_0px_hsl(var(--shadow-color))] scale-110',
        upcoming: 'bg-muted',
      },
      size: {
        sm: 'h-6 w-6',
        md: 'h-8 w-8',
        lg: 'h-10 w-10',
      },
    },
    defaultVariants: {
      status: 'upcoming',
      size: 'md',
    },
  }
)

export interface TimelineDotProps
  extends React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof timelineDotVariants> {}

const TimelineDot = React.forwardRef<HTMLDivElement, TimelineDotProps>(
  ({ status, size, className, children, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(timelineDotVariants({ status, size }), className)}
        {...props}
      >
        {children}
      </div>
    )
  }
)
TimelineDot.displayName = 'TimelineDot'

// Timeline Connector
const timelineConnectorVariants = cva('transition-all duration-200', {
  variants: {
    status: {
      completed: 'bg-foreground',
      current: 'bg-foreground',
      upcoming: 'border-dashed border-2 border-foreground/50 bg-transparent',
    },
    orientation: {
      vertical: 'w-[3px] min-h-8 ml-[14px]',
      horizontal: 'h-[3px] min-w-8 mt-[14px]',
    },
  },
  defaultVariants: {
    status: 'upcoming',
    orientation: 'vertical',
  },
})

export interface TimelineConnectorProps
  extends React.HTMLAttributes<HTMLDivElement>,
    Omit<VariantProps<typeof timelineConnectorVariants>, 'orientation'> {}

const TimelineConnector = React.forwardRef<HTMLDivElement, TimelineConnectorProps>(
  ({ status, className, ...props }, ref) => {
    const { orientation } = useTimeline()

    return (
      <div
        ref={ref}
        className={cn(
          timelineConnectorVariants({ status, orientation }),
          className
        )}
        {...props}
      />
    )
  }
)
TimelineConnector.displayName = 'TimelineConnector'

// Timeline Content
const TimelineContent = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => {
  const { orientation } = useTimeline()

  return (
    <div
      ref={ref}
      className={cn(
        'flex-1',
        orientation === 'vertical' ? 'pb-8' : 'pr-8',
        className
      )}
      {...props}
    />
  )
})
TimelineContent.displayName = 'TimelineContent'

// Timeline Header
const TimelineHeader = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => {
  return (
    <div
      ref={ref}
      className={cn('flex items-center gap-2', className)}
      {...props}
    />
  )
})
TimelineHeader.displayName = 'TimelineHeader'

// Timeline Title
const TimelineTitle = React.forwardRef<
  HTMLHeadingElement,
  React.HTMLAttributes<HTMLHeadingElement>
>(({ className, ...props }, ref) => {
  return (
    <h3
      ref={ref}
      className={cn(
        'text-base font-bold uppercase tracking-wide',
        className
      )}
      {...props}
    />
  )
})
TimelineTitle.displayName = 'TimelineTitle'

// Timeline Description
const TimelineDescription = React.forwardRef<
  HTMLParagraphElement,
  React.HTMLAttributes<HTMLParagraphElement>
>(({ className, ...props }, ref) => {
  return (
    <p
      ref={ref}
      className={cn('text-sm text-muted-foreground mt-1', className)}
      {...props}
    />
  )
})
TimelineDescription.displayName = 'TimelineDescription'

// Timeline Time
const TimelineTime = React.forwardRef<
  HTMLTimeElement,
  React.TimeHTMLAttributes<HTMLTimeElement>
>(({ className, ...props }, ref) => {
  return (
    <time
      ref={ref}
      className={cn(
        'text-xs font-medium text-muted-foreground',
        className
      )}
      {...props}
    />
  )
})
TimelineTime.displayName = 'TimelineTime'

// Timeline Card - convenience wrapper
const TimelineCard = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => {
  return (
    <div
      ref={ref}
      className={cn(
        'border-3 border-foreground bg-card p-4',
        'shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        className
      )}
      {...props}
    />
  )
})
TimelineCard.displayName = 'TimelineCard'

export {
  Timeline,
  TimelineItem,
  TimelineDot,
  TimelineConnector,
  TimelineContent,
  TimelineHeader,
  TimelineTitle,
  TimelineDescription,
  TimelineTime,
  TimelineCard,
  timelineItemVariants,
  timelineDotVariants,
  timelineConnectorVariants,
}
```

---

### Toggle <a name="toggle"></a>

A two-state button that can be either on or off

- **Registry Name**: `@boldkit/toggle`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/toggle.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-toggle`
  - `class-variance-authority`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/toggle-demo.tsx`

```tsx
import { Toggle } from '@/components/ui/toggle'
import { Bold } from 'lucide-react'

export default function Example() {
  return (
    <Toggle aria-label="Toggle bold">
      <Bold className="h-4 w-4" />
    </Toggle>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/toggle.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import * as TogglePrimitive from '@radix-ui/react-toggle'
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'

const toggleVariants = cva(
  'inline-flex items-center justify-center text-sm font-bold uppercase tracking-wide transition-all duration-200 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50 border-3 border-foreground shadow-[4px_4px_0px_hsl(var(--shadow-color))] [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0 gap-2 hover:translate-x-[4px] hover:translate-y-[4px] hover:shadow-none active:translate-x-[4px] active:translate-y-[4px] active:shadow-none data-[state=on]:bg-primary data-[state=on]:text-primary-foreground data-[state=on]:translate-x-[4px] data-[state=on]:translate-y-[4px] data-[state=on]:shadow-none',
  {
    variants: {
      variant: {
        default: 'bg-transparent hover:bg-muted',
        outline: 'bg-background hover:bg-muted',
      },
      size: {
        default: 'h-10 px-3 min-w-10',
        sm: 'h-9 px-2.5 min-w-9',
        lg: 'h-11 px-5 min-w-11',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  }
)

const Toggle = React.forwardRef<
  React.ElementRef<typeof TogglePrimitive.Root>,
  React.ComponentPropsWithoutRef<typeof TogglePrimitive.Root> &
    VariantProps<typeof toggleVariants>
>(({ className, variant, size, ...props }, ref) => (
  <TogglePrimitive.Root
    ref={ref}
    className={cn(toggleVariants({ variant, size, className }))}
    {...props}
  />
))

Toggle.displayName = TogglePrimitive.Root.displayName

export { Toggle, toggleVariants }
```

---

### Toggle Group <a name="toggle-group"></a>

A group of toggle buttons where only one can be pressed at a time

- **Registry Name**: `@boldkit/toggle-group`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/toggle-group.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-toggle-group`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/toggle`

#### How to Use

##### File: `registry/default/example/toggle-group-demo.tsx`

```tsx
import { ToggleGroup, ToggleGroupItem } from '@/components/ui/toggle-group'
import { AlignLeft, AlignCenter, AlignRight } from 'lucide-react'

export default function Example() {
  return (
    <ToggleGroup type="single">
      <ToggleGroupItem value="left" aria-label="Align left">
        <AlignLeft className="h-4 w-4" />
      </ToggleGroupItem>
      <ToggleGroupItem value="center" aria-label="Align center">
        <AlignCenter className="h-4 w-4" />
      </ToggleGroupItem>
      <ToggleGroupItem value="right" aria-label="Align right">
        <AlignRight className="h-4 w-4" />
      </ToggleGroupItem>
    </ToggleGroup>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/toggle-group.tsx`

```tsx
import * as React from 'react'
import * as ToggleGroupPrimitive from '@radix-ui/react-toggle-group'
import { type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'
import { toggleVariants } from '@/components/ui/toggle'

const ToggleGroupContext = React.createContext<
  VariantProps<typeof toggleVariants>
>({
  size: 'default',
  variant: 'default',
})

const ToggleGroup = React.forwardRef<
  React.ElementRef<typeof ToggleGroupPrimitive.Root>,
  React.ComponentPropsWithoutRef<typeof ToggleGroupPrimitive.Root> &
    VariantProps<typeof toggleVariants>
>(({ className, variant, size, children, ...props }, ref) => (
  <ToggleGroupPrimitive.Root
    ref={ref}
    className={cn('flex items-center justify-center gap-1', className)}
    {...props}
  >
    <ToggleGroupContext.Provider value={{ variant, size }}>
      {children}
    </ToggleGroupContext.Provider>
  </ToggleGroupPrimitive.Root>
))

ToggleGroup.displayName = ToggleGroupPrimitive.Root.displayName

const ToggleGroupItem = React.forwardRef<
  React.ElementRef<typeof ToggleGroupPrimitive.Item>,
  React.ComponentPropsWithoutRef<typeof ToggleGroupPrimitive.Item> &
    VariantProps<typeof toggleVariants>
>(({ className, children, variant, size, ...props }, ref) => {
  const context = React.useContext(ToggleGroupContext)

  return (
    <ToggleGroupPrimitive.Item
      ref={ref}
      className={cn(
        toggleVariants({
          variant: context.variant || variant,
          size: context.size || size,
        }),
        className
      )}
      {...props}
    >
      {children}
    </ToggleGroupPrimitive.Item>
  )
})

ToggleGroupItem.displayName = ToggleGroupPrimitive.Item.displayName

export { ToggleGroup, ToggleGroupItem }
```

---

### Tooltip <a name="tooltip"></a>

A popup that displays information related to an element

- **Registry Name**: `@boldkit/tooltip`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/tooltip.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-tooltip`
- **Local Registry Dependencies**:
  - `@boldkit/utils`

#### How to Use

##### File: `registry/default/example/tooltip-demo.tsx`

```tsx
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/ui/tooltip'

export default function Example() {
  return (
    <Tooltip>
      <TooltipTrigger>Hover me</TooltipTrigger>
      <TooltipContent>
        <p>Tooltip content</p>
      </TooltipContent>
    </Tooltip>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/tooltip.tsx`

```tsx
import * as React from 'react'
import * as TooltipPrimitive from '@radix-ui/react-tooltip'
import { cn } from '@/lib/utils'

const TooltipProvider = TooltipPrimitive.Provider

const Tooltip = TooltipPrimitive.Root

const TooltipTrigger = TooltipPrimitive.Trigger

const TooltipContent = React.forwardRef<
  React.ElementRef<typeof TooltipPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof TooltipPrimitive.Content>
>(({ className, sideOffset = 6, ...props }, ref) => (
  <TooltipPrimitive.Portal>
    <TooltipPrimitive.Content
      ref={ref}
      sideOffset={sideOffset}
      className={cn(
        'z-50 overflow-hidden border-2 border-foreground bg-foreground px-3 py-1.5 text-xs font-medium text-background shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
        className
      )}
      {...props}
    />
  </TooltipPrimitive.Portal>
))
TooltipContent.displayName = TooltipPrimitive.Content.displayName

export { Tooltip, TooltipTrigger, TooltipContent, TooltipProvider }
```

---

### Tour <a name="tour"></a>

Step-by-step product tour with spotlight highlighting and progress indicators

- **Registry Name**: `@boldkit/tour`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/tour.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/button`

#### How to Use

##### File: `registry/default/example/tour-demo.tsx`

```tsx
import { Tour, type TourStep } from '@/components/ui/tour'

const steps: TourStep[] = [
  {
    target: '#step-1',
    title: 'Welcome',
    description: 'This is the first step of the tour.',
  },
  {
    target: '#step-2',
    title: 'Features',
    description: 'Here are some features.',
  },
]

export default function Example() {
  const [open, setOpen] = useState(false)

  return (
    <>
      <Button onClick={() => setOpen(true)}>Start Tour</Button>
      <Tour steps={steps} open={open} onOpenChange={setOpen} />
    </>
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/tour.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { createPortal } from 'react-dom'
import { X } from 'lucide-react'
import { cn } from '@/lib/utils'
import { Button } from '@/components/ui/button'

export interface TourStep {
  target: string | HTMLElement | React.RefObject<HTMLElement>
  title: string
  description: string
  placement?: 'top' | 'right' | 'bottom' | 'left' | 'center'
  spotlightPadding?: number
  content?: React.ReactNode
}

export interface TourProps {
  steps: TourStep[]
  open?: boolean
  onOpenChange?: (open: boolean) => void
  onComplete?: () => void
  onSkip?: () => void
  showSkipButton?: boolean
  showProgress?: boolean
}

interface TourContextValue {
  currentStep: number
  totalSteps: number
  nextStep: () => void
  prevStep: () => void
  goToStep: (index: number) => void
  close: () => void
  skip: () => void
}

const TourContext = React.createContext<TourContextValue | null>(null)

function useTour() {
  const context = React.useContext(TourContext)
  if (!context) {
    throw new Error('useTour must be used within a <Tour />')
  }
  return context
}

// Get element from target
function getTargetElement(
  target: string | HTMLElement | React.RefObject<HTMLElement>
): HTMLElement | null {
  if (typeof target === 'string') {
    return document.querySelector(target)
  }
  if (target instanceof HTMLElement) {
    return target
  }
  return target.current
}

// Calculate popover position
function calculatePosition(
  targetRect: DOMRect,
  popoverRect: DOMRect,
  placement: TourStep['placement'] = 'bottom',
  padding: number = 8
): { top: number; left: number; actualPlacement: TourStep['placement'] } {
  const viewport = {
    width: window.innerWidth,
    height: window.innerHeight,
  }

  let top = 0
  let left = 0
  let actualPlacement = placement

  if (placement === 'center') {
    return {
      top: viewport.height / 2 - popoverRect.height / 2,
      left: viewport.width / 2 - popoverRect.width / 2,
      actualPlacement: 'center',
    }
  }

  // Calculate base positions
  switch (placement) {
    case 'top':
      top = targetRect.top - popoverRect.height - padding
      left = targetRect.left + targetRect.width / 2 - popoverRect.width / 2
      break
    case 'bottom':
      top = targetRect.bottom + padding
      left = targetRect.left + targetRect.width / 2 - popoverRect.width / 2
      break
    case 'left':
      top = targetRect.top + targetRect.height / 2 - popoverRect.height / 2
      left = targetRect.left - popoverRect.width - padding
      break
    case 'right':
      top = targetRect.top + targetRect.height / 2 - popoverRect.height / 2
      left = targetRect.right + padding
      break
  }

  // Check if popover fits, flip if necessary
  if (placement === 'top' && top < 0) {
    top = targetRect.bottom + padding
    actualPlacement = 'bottom'
  } else if (placement === 'bottom' && top + popoverRect.height > viewport.height) {
    top = targetRect.top - popoverRect.height - padding
    actualPlacement = 'top'
  } else if (placement === 'left' && left < 0) {
    left = targetRect.right + padding
    actualPlacement = 'right'
  } else if (placement === 'right' && left + popoverRect.width > viewport.width) {
    left = targetRect.left - popoverRect.width - padding
    actualPlacement = 'left'
  }

  // Keep within viewport bounds
  left = Math.max(padding, Math.min(left, viewport.width - popoverRect.width - padding))
  top = Math.max(padding, Math.min(top, viewport.height - popoverRect.height - padding))

  return { top, left, actualPlacement }
}

// Tour Overlay
interface TourOverlayProps {
  targetRect: DOMRect | null
  spotlightPadding: number
}

function TourOverlay({ targetRect, spotlightPadding }: TourOverlayProps) {
  if (!targetRect) {
    // Center placement - full overlay
    return (
      <div className="fixed inset-0 z-[9998] bg-black/70" />
    )
  }

  const spotlightRect = {
    top: targetRect.top - spotlightPadding,
    left: targetRect.left - spotlightPadding,
    width: targetRect.width + spotlightPadding * 2,
    height: targetRect.height + spotlightPadding * 2,
  }

  return (
    <div
      className="fixed inset-0 z-[9998]"
      style={{
        background: `
          linear-gradient(to right, rgba(0,0,0,0.7) ${spotlightRect.left}px, transparent ${spotlightRect.left}px, transparent ${spotlightRect.left + spotlightRect.width}px, rgba(0,0,0,0.7) ${spotlightRect.left + spotlightRect.width}px),
          linear-gradient(to bottom, rgba(0,0,0,0.7) ${spotlightRect.top}px, transparent ${spotlightRect.top}px, transparent ${spotlightRect.top + spotlightRect.height}px, rgba(0,0,0,0.7) ${spotlightRect.top + spotlightRect.height}px)
        `,
        backgroundBlendMode: 'multiply',
      }}
    />
  )
}

// Tour Popover
interface TourPopoverProps {
  step: TourStep
  targetRect: DOMRect | null
  showSkipButton: boolean
  showProgress: boolean
}

function TourPopover({
  step,
  targetRect,
  showSkipButton,
  showProgress,
}: TourPopoverProps) {
  const { currentStep, totalSteps, nextStep, prevStep, close, skip } = useTour()
  const popoverRef = React.useRef<HTMLDivElement>(null)
  const [position, setPosition] = React.useState({ top: 0, left: 0 })

  const isFirst = currentStep === 0
  const isLast = currentStep === totalSteps - 1

  // Calculate position
  React.useEffect(() => {
    if (!popoverRef.current) return

    const popoverRect = popoverRef.current.getBoundingClientRect()

    if (step.placement === 'center' || !targetRect) {
      setPosition({
        top: window.innerHeight / 2 - popoverRect.height / 2,
        left: window.innerWidth / 2 - popoverRect.width / 2,
      })
    } else {
      const pos = calculatePosition(
        targetRect,
        popoverRect,
        step.placement,
        16
      )
      setPosition({ top: pos.top, left: pos.left })
    }
  }, [step, targetRect])

  return (
    <div
      ref={popoverRef}
      className={cn(
        'fixed z-[9999] w-80 border-3 border-foreground bg-popover p-4',
        'shadow-[8px_8px_0px_hsl(var(--shadow-color))]',
        'animate-in fade-in-0 zoom-in-95 duration-200'
      )}
      style={{ top: position.top, left: position.left }}
    >
      {/* Close button */}
      <button
        type="button"
        onClick={close}
        className={cn(
          'absolute -right-3 -top-3 border-2 border-foreground bg-background p-1',
          'shadow-[2px_2px_0px_hsl(var(--shadow-color))]',
          'hover:translate-x-[2px] hover:translate-y-[2px] hover:shadow-none',
          'transition-all duration-150'
        )}
      >
        <X className="h-3 w-3 stroke-[3]" />
        <span className="sr-only">Close</span>
      </button>

      {/* Content */}
      <div className="space-y-3">
        <h3 className="text-base font-bold uppercase tracking-wide">
          {step.title}
        </h3>
        <p className="text-sm text-muted-foreground">{step.description}</p>
        {step.content}
      </div>

      {/* Progress dots */}
      {showProgress && totalSteps > 1 && (
        <div className="mt-4 flex items-center justify-center gap-1.5">
          {Array.from({ length: totalSteps }, (_, i) => (
            <div
              key={i}
              className={cn(
                'h-2 w-2 border-2 border-foreground transition-all duration-150',
                i === currentStep ? 'bg-primary scale-110' : 'bg-muted'
              )}
            />
          ))}
        </div>
      )}

      {/* Navigation */}
      <div className="mt-4 flex items-center justify-between gap-2">
        <div>
          {showSkipButton && !isLast && (
            <Button
              variant="ghost"
              size="sm"
              onClick={skip}
              className="text-muted-foreground"
            >
              Skip Tour
            </Button>
          )}
        </div>
        <div className="flex items-center gap-2">
          {!isFirst && (
            <Button variant="outline" size="sm" onClick={prevStep}>
              Previous
            </Button>
          )}
          <Button size="sm" onClick={nextStep}>
            {isLast ? 'Finish' : 'Next'}
          </Button>
        </div>
      </div>
    </div>
  )
}

// Main Tour Component
const Tour = React.forwardRef<HTMLDivElement, TourProps>(
  (
    {
      steps,
      open: controlledOpen,
      onOpenChange,
      onComplete,
      onSkip,
      showSkipButton = true,
      showProgress = true,
    },
    ref
  ) => {
    const [uncontrolledOpen, setUncontrolledOpen] = React.useState(false)
    const [currentStep, setCurrentStep] = React.useState(0)
    const [targetRect, setTargetRect] = React.useState<DOMRect | null>(null)

    const isControlled = controlledOpen !== undefined
    const open = isControlled ? controlledOpen : uncontrolledOpen

    const setOpen = React.useCallback(
      (value: boolean) => {
        if (!isControlled) {
          setUncontrolledOpen(value)
        }
        onOpenChange?.(value)
      },
      [isControlled, onOpenChange]
    )

    const currentStepData = steps[currentStep]

    // Update target rect when step changes
    React.useEffect(() => {
      if (!open || !currentStepData) return

      const updateRect = () => {
        const element = getTargetElement(currentStepData.target)
        if (element) {
          const rect = element.getBoundingClientRect()
          setTargetRect(rect)

          // Scroll element into view
          element.scrollIntoView({
            behavior: 'smooth',
            block: 'center',
          })
        } else {
          // Target not found — fall back to center placement
          if (process.env.NODE_ENV === 'development') {
            console.warn(`[Tour] Step target "${currentStepData.target}" not found in DOM`)
          }
          setTargetRect(null)
        }
      }

      updateRect()

      // Update on resize/scroll
      window.addEventListener('resize', updateRect)
      window.addEventListener('scroll', updateRect, true)

      return () => {
        window.removeEventListener('resize', updateRect)
        window.removeEventListener('scroll', updateRect, true)
      }
    }, [open, currentStep, currentStepData])

    // Reset step when closed
    React.useEffect(() => {
      if (!open) {
        setCurrentStep(0)
      }
    }, [open])

    const nextStep = React.useCallback(() => {
      if (currentStep < steps.length - 1) {
        setCurrentStep((prev) => prev + 1)
      } else {
        setOpen(false)
        onComplete?.()
      }
    }, [currentStep, steps.length, setOpen, onComplete])

    const prevStep = React.useCallback(() => {
      if (currentStep > 0) {
        setCurrentStep((prev) => prev - 1)
      }
    }, [currentStep])

    const goToStep = React.useCallback((index: number) => {
      if (index >= 0 && index < steps.length) {
        setCurrentStep(index)
      }
    }, [steps.length])

    const close = React.useCallback(() => {
      setOpen(false)
    }, [setOpen])

    const skip = React.useCallback(() => {
      setOpen(false)
      onSkip?.()
    }, [setOpen, onSkip])

    const contextValue = React.useMemo<TourContextValue>(
      () => ({
        currentStep,
        totalSteps: steps.length,
        nextStep,
        prevStep,
        goToStep,
        close,
        skip,
      }),
      [currentStep, steps.length, nextStep, prevStep, goToStep, close, skip]
    )

    return (
      <>
        {/* Stable ref anchor — always mounted so ref is never null */}
        <div ref={ref} style={{ display: 'none' }} />
        {open && currentStepData && createPortal(
          <TourContext.Provider value={contextValue}>
            <TourOverlay
              targetRect={targetRect}
              spotlightPadding={currentStepData.spotlightPadding ?? 8}
            />
            <TourPopover
              step={currentStepData}
              targetRect={targetRect}
              showSkipButton={showSkipButton}
              showProgress={showProgress}
            />
          </TourContext.Provider>,
          document.body
        )}
      </>
    )
  }
)
Tour.displayName = 'Tour'

export { Tour, useTour }
```

---

### Tree View <a name="tree-view"></a>

Hierarchical tree view with expand/collapse, selection, and checkboxes

- **Registry Name**: `@boldkit/tree-view`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/tree-view.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-collapsible`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/checkbox`
  - `@boldkit/collapsible`

#### How to Use

##### File: `registry/default/example/tree-view-demo.tsx`

```tsx
import { TreeView } from '@/components/ui/tree-view'

const data = [
  {
    id: 'root',
    label: 'Documents',
    children: [
      { id: 'doc1', label: 'File 1.txt' },
      { id: 'doc2', label: 'File 2.txt' },
    ],
  },
]

export default function Example() {
  return <TreeView data={data} />
}
```

#### Component Source Code

##### File: `registry/default/ui/tree-view.tsx`

```tsx
/* eslint-disable react-refresh/only-export-components */
import * as React from 'react'
import { ChevronRight, Folder, File } from 'lucide-react'
import { cn } from '@/lib/utils'
import { Checkbox } from '@/components/ui/checkbox'
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from '@/components/ui/collapsible'

export interface TreeNode {
  id: string
  label: string
  icon?: React.ReactNode
  children?: TreeNode[]
  disabled?: boolean
}

export interface TreeViewProps extends React.HTMLAttributes<HTMLDivElement> {
  data: TreeNode[]
  expandedIds?: string[]
  onExpandedChange?: (ids: string[]) => void
  selectedIds?: string[]
  onSelectedChange?: (ids: string[]) => void
  selectionMode?: 'none' | 'single' | 'multiple'
  showCheckboxes?: boolean
  showIcons?: boolean
  defaultExpandedIds?: string[]
  defaultSelectedIds?: string[]
}

// Context
interface TreeViewContextValue {
  expandedIds: Set<string>
  toggleExpanded: (id: string) => void
  selectedIds: Set<string>
  toggleSelected: (id: string) => void
  selectionMode: 'none' | 'single' | 'multiple'
  showCheckboxes: boolean
  showIcons: boolean
}

const TreeViewContext = React.createContext<TreeViewContextValue | null>(null)

function useTreeView() {
  const context = React.useContext(TreeViewContext)
  if (!context) {
    throw new Error('TreeView components must be used within a <TreeView />')
  }
  return context
}

const TreeView = React.forwardRef<HTMLDivElement, TreeViewProps>(
  (
    {
      data,
      expandedIds: controlledExpandedIds,
      onExpandedChange,
      selectedIds: controlledSelectedIds,
      onSelectedChange,
      selectionMode = 'none',
      showCheckboxes = false,
      showIcons = true,
      defaultExpandedIds = [],
      defaultSelectedIds = [],
      className,
      ...props
    },
    ref
  ) => {
    const [uncontrolledExpandedIds, setUncontrolledExpandedIds] = React.useState<Set<string>>(
      new Set(defaultExpandedIds)
    )
    const [uncontrolledSelectedIds, setUncontrolledSelectedIds] = React.useState<Set<string>>(
      new Set(defaultSelectedIds)
    )

    const isExpandedControlled = controlledExpandedIds !== undefined
    const isSelectedControlled = controlledSelectedIds !== undefined

    // Memoize Set objects to prevent useCallback dependencies from changing on every render
    const expandedIds = React.useMemo(
      () => (isExpandedControlled ? new Set(controlledExpandedIds) : uncontrolledExpandedIds),
      [isExpandedControlled, controlledExpandedIds, uncontrolledExpandedIds]
    )

    const selectedIds = React.useMemo(
      () => (isSelectedControlled ? new Set(controlledSelectedIds) : uncontrolledSelectedIds),
      [isSelectedControlled, controlledSelectedIds, uncontrolledSelectedIds]
    )

    const toggleExpanded = React.useCallback(
      (id: string) => {
        const newExpandedIds = new Set(expandedIds)
        if (newExpandedIds.has(id)) {
          newExpandedIds.delete(id)
        } else {
          newExpandedIds.add(id)
        }

        if (!isExpandedControlled) {
          setUncontrolledExpandedIds(newExpandedIds)
        }
        onExpandedChange?.(Array.from(newExpandedIds))
      },
      [expandedIds, isExpandedControlled, onExpandedChange]
    )

    const toggleSelected = React.useCallback(
      (id: string) => {
        if (selectionMode === 'none') return

        let newSelectedIds: Set<string>

        if (selectionMode === 'single') {
          newSelectedIds = selectedIds.has(id) ? new Set() : new Set([id])
        } else {
          newSelectedIds = new Set(selectedIds)
          if (newSelectedIds.has(id)) {
            newSelectedIds.delete(id)
          } else {
            newSelectedIds.add(id)
          }
        }

        if (!isSelectedControlled) {
          setUncontrolledSelectedIds(newSelectedIds)
        }
        onSelectedChange?.(Array.from(newSelectedIds))
      },
      [selectedIds, selectionMode, isSelectedControlled, onSelectedChange]
    )

    return (
      <TreeViewContext.Provider
        value={{
          expandedIds,
          toggleExpanded,
          selectedIds,
          toggleSelected,
          selectionMode,
          showCheckboxes,
          showIcons,
        }}
      >
        <div
          ref={ref}
          role="tree"
          className={cn(
            'border-3 border-foreground bg-background p-2',
            'shadow-[4px_4px_0px_hsl(var(--shadow-color))]',
            className
          )}
          {...props}
        >
          {data.map((node) => (
            <TreeNodeItem key={node.id} node={node} level={0} />
          ))}
        </div>
      </TreeViewContext.Provider>
    )
  }
)
TreeView.displayName = 'TreeView'

// Tree Node Component
interface TreeNodeProps {
  node: TreeNode
  level: number
}

function TreeNodeItem({ node, level }: TreeNodeProps) {
  const {
    expandedIds,
    toggleExpanded,
    selectedIds,
    toggleSelected,
    selectionMode,
    showCheckboxes,
    showIcons,
  } = useTreeView()

  const hasChildren = node.children && node.children.length > 0
  const isExpanded = expandedIds.has(node.id)
  const isSelected = selectedIds.has(node.id)

  const handleKeyDown = (e: React.KeyboardEvent) => {
    switch (e.key) {
      case 'Enter':
      case ' ':
        e.preventDefault()
        if (selectionMode !== 'none') {
          toggleSelected(node.id)
        } else if (hasChildren) {
          toggleExpanded(node.id)
        }
        break
      case 'ArrowRight':
        if (hasChildren && !isExpanded) {
          e.preventDefault()
          toggleExpanded(node.id)
        }
        break
      case 'ArrowLeft':
        if (hasChildren && isExpanded) {
          e.preventDefault()
          toggleExpanded(node.id)
        }
        break
    }
  }

  const content = (
    <div
      role="treeitem"
      aria-selected={isSelected}
      aria-expanded={hasChildren ? isExpanded : undefined}
      aria-disabled={node.disabled}
      tabIndex={node.disabled ? -1 : 0}
      onKeyDown={handleKeyDown}
      onClick={() => {
        if (node.disabled) return
        if (hasChildren) {
          toggleExpanded(node.id)
        }
        if (selectionMode !== 'none') {
          toggleSelected(node.id)
        }
      }}
      className={cn(
        'flex items-center gap-2 px-2 py-1.5 cursor-pointer transition-colors',
        'hover:bg-muted focus:outline-none focus:bg-muted',
        isSelected && 'bg-accent',
        node.disabled && 'opacity-50 cursor-not-allowed'
      )}
      style={{ paddingLeft: `${level * 16 + 8}px` }}
    >
      {/* Expand/collapse button */}
      {hasChildren ? (
        <button
          type="button"
          onClick={(e) => {
            e.stopPropagation()
            if (!node.disabled) {
              toggleExpanded(node.id)
            }
          }}
          className="p-0.5 hover:bg-muted-foreground/20 transition-colors"
          aria-label={isExpanded ? 'Collapse' : 'Expand'}
        >
          <ChevronRight
            className={cn(
              'h-4 w-4 stroke-[3] transition-transform duration-200',
              isExpanded && 'rotate-90'
            )}
          />
        </button>
      ) : (
        <span className="w-5" />
      )}

      {/* Checkbox */}
      {showCheckboxes && selectionMode !== 'none' && (
        <Checkbox
          checked={isSelected}
          onCheckedChange={() => toggleSelected(node.id)}
          onClick={(e) => e.stopPropagation()}
          disabled={node.disabled}
          className="h-5 w-5 border-2 border-foreground data-[state=checked]:bg-primary data-[state=checked]:shadow-[2px_2px_0px_hsl(var(--shadow-color))]"
        />
      )}

      {/* Icon */}
      {showIcons && (
        <span className="shrink-0">
          {node.icon || (hasChildren ? (
            <Folder className="h-4 w-4" />
          ) : (
            <File className="h-4 w-4" />
          ))}
        </span>
      )}

      {/* Label */}
      <span className="text-sm truncate">{node.label}</span>
    </div>
  )

  if (!hasChildren) {
    return content
  }

  return (
    <Collapsible open={isExpanded}>
      <CollapsibleTrigger asChild className="w-full">
        {content}
      </CollapsibleTrigger>
      <CollapsibleContent>
        <div role="group">
          {(node.children ?? []).map((child) => (
            <TreeNodeItem key={child.id} node={child} level={level + 1} />
          ))}
        </div>
      </CollapsibleContent>
    </Collapsible>
  )
}

export { TreeView, useTreeView }
```

---

### Treemap Chart <a name="treemap-chart"></a>

Treemap for hierarchical data visualization with proportional rectangles

- **Registry Name**: `@boldkit/treemap-chart`
- **Type**: `registry:ui`

#### Installation

```bash
npx shadcn@latest add https://boldkit.dev/r/treemap-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `@boldkit/utils`
  - `@boldkit/chart`

#### How to Use

##### File: `registry/default/example/treemap-chart-demo.tsx`

```tsx
import { TreemapChart } from "@/components/ui/treemap-chart"

export default function TreemapChartDemo() {
  return (
    <TreemapChart
      data={[
        { name: "React", value: 400 },
        { name: "Vue", value: 300 },
        { name: "Angular", value: 200 },
        { name: "Svelte", value: 100 },
      ]}
    />
  )
}
```

#### Component Source Code

##### File: `registry/default/ui/treemap-chart.tsx`

```tsx
import * as React from 'react'
import { cn } from '@/lib/utils'
import { Treemap as RechartsTreemap, ResponsiveContainer, Tooltip } from 'recharts'
import { ChartEmpty } from './chart'

export interface TreemapChartData {
  name: string
  value?: number
  children?: TreemapChartData[]
  fill?: string
  [key: string]: unknown
}

export interface TreemapChartProps extends React.HTMLAttributes<HTMLDivElement> {
  data: TreemapChartData[]
  showTooltip?: boolean
  animated?: boolean
  height?: number
  /** Accessible label for screen readers (default: "Treemap chart") */
  ariaLabel?: string
  emptyState?: React.ReactNode
}

const NEUBRUTALISM_COLORS = [
  'hsl(var(--primary))',
  'hsl(var(--secondary))',
  'hsl(var(--accent))',
  'hsl(var(--success))',
  'hsl(var(--info))',
  'hsl(var(--warning))',
]

interface CustomContentProps {
  x?: number
  y?: number
  width?: number
  height?: number
  name?: string
  value?: number
  depth?: number
  index?: number
}

function CustomTreemapContent(props: CustomContentProps) {
  const { x = 0, y = 0, width = 0, height = 0, name, value, depth = 0, index = 0 } = props
  const colorIndex = (depth * 7 + index) % NEUBRUTALISM_COLORS.length
  const fill = NEUBRUTALISM_COLORS[colorIndex]
  const isSmall = width < 60 || height < 40

  if (depth === 0) return null

  return (
    <g>
      <rect
        x={x + 2}
        y={y + 2}
        width={width - 4}
        height={height - 4}
        style={{ fill, stroke: 'hsl(var(--foreground))', strokeWidth: 3 }}
      />
      {!isSmall && (
        <>
          <text
            x={x + width / 2}
            y={y + height / 2 - (value !== undefined ? 8 : 0)}
            textAnchor="middle"
            dominantBaseline="central"
            fill="hsl(var(--foreground))"
            style={{ fontFamily: 'inherit', fontSize: Math.min(14, width / 6), fontWeight: 900 }}
          >
            {name}
          </text>
          {value !== undefined && (
            <text
              x={x + width / 2}
              y={y + height / 2 + 10}
              textAnchor="middle"
              dominantBaseline="central"
              fill="hsl(var(--foreground))"
              style={{ fontFamily: "'DM Mono', monospace", fontSize: Math.min(11, width / 8), fontWeight: 700, opacity: 0.75 }}
            >
              {value.toLocaleString()}
            </text>
          )}
        </>
      )}
    </g>
  )
}

const TreemapChart = React.forwardRef<HTMLDivElement, TreemapChartProps>(
  (
    {
      data,
      showTooltip = true,
      animated = true,
      height = 320,
      ariaLabel = 'Treemap chart',
      emptyState,
      className,
      ...props
    },
    ref
  ) => {
    if (!data || data.length === 0) {
      return <ChartEmpty ref={ref} message={emptyState} className={className} {...props} />
    }

    return (
      <div
        ref={ref}
        role="img"
        aria-label={ariaLabel}
        className={cn('w-full', className)}
        style={{ height }}
        {...props}
      >
        <ResponsiveContainer width="100%" height="100%">
          <RechartsTreemap
            data={data}
            dataKey="value"
            aspectRatio={4 / 3}
            isAnimationActive={animated}
            animationDuration={400}
            content={<CustomTreemapContent />}
          >
            {showTooltip && (
              <Tooltip
                contentStyle={{
                  border: '3px solid hsl(var(--foreground))',
                  borderRadius: 0,
                  boxShadow: '4px 4px 0px hsl(var(--foreground))',
                  background: 'hsl(var(--background))',
                  fontFamily: "'DM Mono', monospace",
                  fontSize: 12,
                }}
                formatter={(value, name) => [
                  typeof value === 'number' ? value.toLocaleString() : String(value ?? ''),
                  String(name ?? ''),
                ]}
              />
            )}
          </RechartsTreemap>
        </ResponsiveContainer>
      </div>
    )
  }
)
TreemapChart.displayName = 'TreemapChart'

export { TreemapChart }
```

---
