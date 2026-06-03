# Chanhdai UI Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **Chanhdai UI** (chanhdai.com) — a set of gorgeous creative web animations, customized theme switchers with circle blur/polygon effects, circular texts, elastic sliders, and mobile haptic utilities built with Framer Motion, Tailwind CSS, and TypeScript.

## Table of Contents

- [Apple Hello Effect](#apple-hello-effect)
- [Awesome Terminal — iTerm2 + Zsh + Oh My Zsh + Amazon Q](#awesome-terminal)
- [Brand Assets Menu](#brand-assets-menu)
- [Code Block Command](#code-block-command)
- [Consent Manager](#consent-manager)
- [Copy Button](#copy-button)
- [Dot Grid Spotlight](#dot-grid-spotlight)
- [Elastic Slider](#elastic-slider)
- [Fluid Gradient Text](#fluid-gradient-text)
- [Followed by @shadcn on X](#followed-by-shadcn-on-x)
- [GitHub Contributions](#github-contributions)
- [GitHub Stars](#github-stars)
- [Glow Card Grid](#glow-card-grid)
- [Grateful for the Feedback on My Portfolio Website](#grateful-for-the-feedback-on-my-portfolio-website)
- [Haptic Feedback](#haptic)
- [Icon Swap](#icon-swap)
- [Manu Arora reviewed My Portfolio Website](#manu-arora-reviewed-my-portfolio-website)
- [Middle Truncation](#middle-truncation)
- [Mobius Loop Icon](#mobius-loop-icon)
- [OrcDev and Francesco Ciulla Reviewed My Open Source Portfolio](#orcdev-and-francesco-ciulla-reviewed-my-open-source-portfolio)
- [OrcDev discovered the WILDEST shadcn component libraries ever](#orcdev-discovered-the-wildest-shadcn-component-libraries-ever)
- [React Wheel Picker joins Vercel Open Source Program](#react-wheel-picker-joins-vercel-open-source-program)
- [React Wheel Picker](#react-wheel-picker)
- [Scroll Fade Effect](#scroll-fade-effect)
- [Shimmering Text](#shimmering-text)
- [Slide to Unlock](#slide-to-unlock)
- [Spinning Circular Text](#spinning-circular-text)
- [Testimonial Spotlight](#testimonial-spotlight)
- [Testimonial](#testimonial)
- [Testimonials Marquee](#testimonials-marquee)
- [Text Flip](#text-flip)
- [Theme Switcher](#theme-switcher)
- [Theme Toggle Effect](#theme-toggle-effect)
- [Tips for Creating Beautiful Image Borders](#tips-for-creating-beautiful-image-borders)
- [TOC Minimap](#toc-minimap)
- [Twemoji](#twemoji)
- [Two of My Projects Featured on OrcDev’s YouTube Channel](#two-of-my-projects-featured-on-orcdev-s-youtube-channel)
- [Installing Uptime Kuma with Docker and setting up NGINX with SSL](#uptime-kuma)
- [Welcome to chanhdai.com](#welcome)
- [Work Experience](#work-experience-component)

---

### Apple Hello Effect <a name="apple-hello-effect"></a>

SVG writing animation inspired by Apple’s “Hello” screen.

- **Registry Key**: `apple-hello-effect`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/apple-hello-effect.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

##### Example 1

```tsx
import { AppleHelloEffectEnglish } from "@/components/apple-hello-effect-english"
import { AppleHelloEffectHindi } from "@/components/apple-hello-effect-hindi"
import { AppleHelloEffectSpanish } from "@/components/apple-hello-effect-spanish"
import { AppleHelloEffectVietnamese } from "@/components/apple-hello-effect-vietnamese"
```

##### Example 2

```tsx
<AppleHelloEffectEnglish />
<AppleHelloEffectHindi />
<AppleHelloEffectSpanish />
<AppleHelloEffectVietnamese />
```

#### Component Source Code

##### File: `src/registry/components/apple-hello-effect/apple-hello-effect-english.tsx`

```tsx
"use client"

import type { ComponentProps } from "react"
import type { TargetAndTransition } from "motion/react"
import { motion } from "motion/react"

import { cn } from "@/lib/utils"

const initialProps: TargetAndTransition = {
  pathLength: 0,
  opacity: 0,
}

const animateProps: TargetAndTransition = {
  pathLength: 1,
  opacity: 1,
}

export type AppleHelloEffectEnglishProps = Omit<
  ComponentProps<typeof motion.svg>,
  "durationScale" | "onAnimationComplete"
> & {
  /**
   * Scales the duration and delay of the handwriting animation.
   * Values below 1 speed up, values above 1 slow down.
   * @defaultValue 1
   */
  durationScale?: number
  /** Called when the full handwriting animation completes. */
  onAnimationComplete?: () => void
}

export function AppleHelloEffectEnglish({
  className,
  durationScale = 1,
  onAnimationComplete,
  ...props
}: AppleHelloEffectEnglishProps) {
  const calc = (x: number) => x * durationScale

  return (
    <motion.svg
      className={cn("h-20", className)}
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 638 200"
      fill="none"
      stroke="currentColor"
      strokeWidth="14.8883"
      strokeLinecap="round"
      initial={{ opacity: 1 }}
      exit={{ opacity: 0 }}
      transition={{ duration: 0.5 }}
      {...props}
    >
      <title>hello</title>

      {/* h1 */}
      <motion.path
        d="M8.69214 166.553C36.2393 151.239 61.3409 131.548 89.8191 98.0295C109.203 75.1488 119.625 49.0228 120.122 31.0026C120.37 17.6036 113.836 7.43883 101.759 7.43883C88.3598 7.43883 79.9231 17.6036 74.7122 40.9363C69.005 66.5793 64.7866 96.0036 54.1166 190.356"
        initial={initialProps}
        animate={animateProps}
        transition={{
          duration: calc(0.8),
          ease: "easeInOut",
          opacity: { duration: 0.4 },
        }}
      />

      {/* h2, ello */}
      <motion.path
        d="M55.1624 181.135C60.6251 133.114 81.4118 98.0479 107.963 98.0479C123.844 98.0479 133.937 110.703 131.071 128.817C129.457 139.487 127.587 150.405 125.408 163.06C122.869 178.941 130.128 191.348 152.122 191.348C184.197 191.348 219.189 173.523 237.097 145.915C243.198 136.509 245.68 128.073 245.928 119.884C246.176 104.996 237.739 93.8296 222.851 93.8296C203.992 93.8296 189.6 115.17 189.6 142.465C189.6 171.745 205.481 192.341 239.208 192.341C285.066 192.341 335.86 137.292 359.199 75.8585C365.788 58.513 368.26 42.4065 368.26 31.1512C368.26 17.8057 364.042 7.55823 352.131 7.55823C340.469 7.55823 332.777 16.6141 325.829 30.9129C317.688 47.4967 311.667 71.4162 309.203 98.4549C303 166.301 316.896 191.348 349.936 191.348C390 191.348 434.542 135.534 457.286 75.6686C463.803 58.513 466.275 42.4065 466.275 31.1512C466.275 17.8057 462.057 7.55823 450.146 7.55823C438.484 7.55823 430.792 16.6141 423.844 30.9129C415.703 47.4967 409.682 71.4162 407.218 98.4549C401.015 166.301 414.911 191.348 444.416 191.348C473.874 191.348 489.877 165.67 499.471 138.402C508.955 111.447 520.618 94.8221 544.935 94.8221C565.035 94.8221 580.916 109.71 580.916 137.75C580.916 168.768 560.792 192.093 535.362 192.341C512.984 192.589 498.285 174.475 499.774 147.179C501.511 116.907 519.873 94.8221 543.943 94.8221C557.839 94.8221 569.51 100.999 578.682 107.725C603.549 125.866 622.709 114.656 630.047 96.7186"
        initial={initialProps}
        animate={animateProps}
        transition={{
          duration: calc(2.8),
          ease: "easeInOut",
          delay: calc(0.7),
          opacity: { duration: 0.7, delay: calc(0.7) },
        }}
        onAnimationComplete={onAnimationComplete}
      />
    </motion.svg>
  )
}
```

---

### Awesome Terminal — iTerm2 + Zsh + Oh My Zsh + Amazon Q <a name="awesome-terminal"></a>

Optimize your terminal with iTerm2, Zsh, Oh My Zsh, and Amazon Q - a guide to installation, theme customization, plugins, and configuration for an enhanced command-line experience.

- **Registry Key**: `awesome-terminal`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/awesome-terminal.json
```

---

### Brand Assets Menu <a name="brand-assets-menu"></a>

Context menu for copying brand SVGs and opening asset links.

- **Registry Key**: `brand-assets-menu`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/brand-assets-menu.json
```

#### Dependencies

- **NPM Packages**:
  - `@rexa-developer/tiks`
- **Local Registry Dependencies**:
  - `context-menu`
  - `sonner`

#### How to Use

##### Example 1

```tsx
import { BrandAssetsMenu } from "@/components/brand-assets-menu"
```

##### Example 2

```tsx
<BrandAssetsMenu
  logomark={<ChanhDaiMark />}
  logomarkSVG="<svg>...</svg>"
  logotypeSVG="<svg>...</svg>"
  brandGuidelinesURL="https://chanhdai.com/blog/chanhdai-brand"
  brandAssetsURL="https://assets.chanhdai.com/chanhdai-brand.zip"
>
  <Link />
</BrandAssetsMenu>
```

#### Component Source Code

##### File: `src/registry/components/brand-assets-menu/brand-assets-menu.tsx`

```tsx
"use client"

import { useTiks } from "@rexa-developer/tiks/react"
import { toast } from "sonner"

import {
  ContextMenu,
  ContextMenuContent,
  ContextMenuItem,
  ContextMenuSeparator,
  ContextMenuTrigger,
} from "@/components/ui/context-menu"
import { IconPlaceholder } from "@/registry/icons/icon-placeholder"

export type BrandAssetsMenuProps = {
  logomark: React.ReactElement
  logomarkSVG: string
  logotypeSVG: string
  brandGuidelinesURL: string
  brandAssetsURL: string
  children: React.ReactElement
}

export function BrandAssetsMenu({
  logomark,
  logomarkSVG,
  logotypeSVG,
  brandGuidelinesURL,
  brandAssetsURL,
  children,
}: BrandAssetsMenuProps) {
  const { success } = useTiks()

  return (
    <ContextMenu>
      <ContextMenuTrigger asChild>{children}</ContextMenuTrigger>

      <ContextMenuContent className="w-fit">
        <ContextMenuItem
          onClick={() => {
            copyText(logomarkSVG)
            toast.success("Logomark as SVG copied")
            success()
          }}
        >
          {logomark}
          Copy Logomark as SVG
        </ContextMenuItem>

        <ContextMenuItem
          onClick={() => {
            copyText(logotypeSVG)
            toast.success("Logotype as SVG copied")
            success()
          }}
        >
          <IconPlaceholder
            lucide="TypeIcon"
            tabler="IconLetterT"
            hugeicons="TextIcon"
            phosphor="TextTIcon"
            remixicon="RiText"
          />
          Copy Logotype as SVG
        </ContextMenuItem>

        <ContextMenuSeparator />

        <ContextMenuItem asChild>
          <a
            href={brandGuidelinesURL}
            target="_blank"
            rel="noopener noreferrer"
          >
            <IconPlaceholder
              lucide="SquareDashedIcon"
              tabler="IconShape"
              hugeicons="DashedLine02Icon"
              phosphor="BoundingBoxIcon"
              remixicon="RiShapeLine"
            />
            Brand Guidelines
          </a>
        </ContextMenuItem>

        <ContextMenuItem asChild>
          <a
            href={brandAssetsURL}
            target="_blank"
            rel="noopener noreferrer"
            download
          >
            <IconPlaceholder
              lucide="DownloadIcon"
              tabler="IconDownload"
              hugeicons="Download01Icon"
              phosphor="DownloadSimpleIcon"
              remixicon="RiDownloadLine"
            />
            Download Brand Assets
          </a>
        </ContextMenuItem>
      </ContextMenuContent>
    </ContextMenu>
  )
}

const copyText = async (text: string) => {
  try {
    await navigator.clipboard.writeText(text)
    return true
  } catch {
    return false
  }
}
```

---

### Code Block Command <a name="code-block-command"></a>

Display install commands with package manager switcher and copy button.

- **Registry Key**: `code-block-command`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/code-block-command.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`
  - `motion`
  - `jotai`
- **Local Registry Dependencies**:
  - `https://chanhdai.com/r/icon-swap.json`
  - `https://chanhdai.com/r/copy-button.json`

#### How to Use

##### Example 1

```tsx
import {
  CodeBlockCommand,
  convertNpmCommand,
} from "@/components/code-block-command"
```

##### Example 2

```tsx
<CodeBlockCommand
  prompt="Add the button component to my project"
  pnpm="pnpm dlx shadcn add button"
  yarn="yarn dlx shadcn add button"
  npm="npx shadcn add button"
  bun="bunx --bun shadcn add button"
/>
```

##### Example 3

```tsx
<CodeBlockCommand
  prompt="Add the button component to my project"
  {...convertNpmCommand("npx shadcn add button")}
/>
```

#### Component Source Code

##### File: `src/registry/components/code-block-command/code-block-command.tsx`

```tsx
"use client"

import { useMemo } from "react"
import { ScrollArea } from "@base-ui/react/scroll-area"
import { useAtom } from "jotai"
import { atomWithStorage } from "jotai/utils"

import { cn } from "@/lib/utils"
import {
  Tabs,
  TabsContent,
  TabsIndicator,
  TabsList,
  TabsTrigger,
} from "@/components/base/ui/tabs"
import { CopyButton } from "@/registry/components/copy-button"
import { IconSwap, IconSwapItem } from "@/registry/components/icon-swap"
import { IconPlaceholder } from "@/registry/icons/icon-placeholder"

export type PackageManager = "prompt" | "pnpm" | "yarn" | "npm" | "bun"

const packageManagerAtom = atomWithStorage<PackageManager>(
  "packageManager",
  "pnpm"
)

export function usePackageManager() {
  return useAtom(packageManagerAtom)
}

/**
 * Props for the CodeBlockCommand component.
 */
export type CodeBlockCommandProps = {
  /**
   * Natural language instruction for AI agents to install a package or component.
   */
  prompt?: string
  /**
   * Command to execute with pnpm package manager.
   */
  pnpm?: string

  /**
   * Command to execute with yarn package manager.
   */
  yarn?: string

  /**
   * Command to execute with npm package manager.
   */
  npm?: string

  /**
   * Command to execute with bun package manager.
   */
  bun?: string

  /**
   * Callback invoked when a command is successfully copied to clipboard.
   *
   * Receives an object containing the selected package manager and the copied command text.
   * Useful for tracking user interactions or showing custom success notifications.
   *
   * @param data - Object containing copy operation details
   * @param data.packageManager - The package manager that was selected when copying
   * @param data.command - The actual command text that was copied
   *
   * @example
   * ```tsx
   * <CodeBlockCommand
   *   pnpm="pnpm add react"
   *   onCopySuccess={({ packageManager, command }) => {
   *     console.log(`Copied ${command} for ${packageManager}`)
   *   }}
   * />
   * ```
   */
  onCopySuccess?: (data: {
    packageManager: PackageManager
    command: string
  }) => void

  /**
   * Callback invoked when copying to clipboard fails.
   *
   * Receives the error object for debugging or showing custom error messages.
   *
   * @param error - The error that occurred during the copy operation
   *
   * @example
   * ```tsx
   * <CodeBlockCommand
   *   pnpm="pnpm add react"
   *   onCopyError={(error) => {
   *     console.error('Copy failed:', error)
   *   }}
   * />
   * ```
   */
  onCopyError?: (error: Error) => void
}

export function CodeBlockCommand({
  prompt,
  pnpm,
  yarn,
  npm,
  bun,
  onCopySuccess,
  onCopyError,
}: CodeBlockCommandProps) {
  const [packageManager, setPackageManager] = usePackageManager()

  const tabs = useMemo(() => {
    return {
      prompt,
      pnpm,
      yarn,
      npm,
      bun,
    }
  }, [prompt, pnpm, yarn, npm, bun])

  const tabsFiltered = useMemo(
    () => Object.entries(tabs).filter(([, value]) => !!value),
    [tabs]
  )

  return (
    <div className="relative overflow-hidden rounded-xl bg-code">
      <Tabs
        className="gap-0"
        value={packageManager}
        onValueChange={(value) => {
          setPackageManager(value as PackageManager)
        }}
      >
        <ScrollArea.Root className="w-full pr-10 shadow-[inset_0_-1px_0_0] shadow-border">
          <TabsList
            className={cn(
              "h-10 max-w-full justify-start rounded-none bg-transparent p-0 pl-4 inset-ring-0 dark:bg-transparent [&_svg]:size-4 [&_svg]:shrink-0 [&_svg]:text-muted-foreground",
              "[--scroll-area-overflow-x-end:inherit] [--scroll-area-overflow-x-start:inherit]",
              "mask-linear-[to_right,transparent_0,black_min(2.5rem,var(--scroll-area-overflow-x-start)),black_calc(100%-min(2.5rem,var(--scroll-area-overflow-x-end,2.5rem))),transparent_100%]"
            )}
            render={<ScrollArea.Viewport />}
          >
            <IconSwap>
              <IconSwapItem className="mr-2" key={packageManager}>
                {getIconForPackageManager(packageManager)}
              </IconSwapItem>
            </IconSwap>

            {tabsFiltered.map(([key]) => {
              return (
                <TabsTrigger
                  key={key}
                  className="h-7 rounded-lg p-0 px-2 font-mono"
                  value={key}
                >
                  {key}
                </TabsTrigger>
              )
            })}

            <TabsIndicator className="h-0.5 translate-y-0 rounded-none bg-foreground ring-0 dark:bg-foreground" />
          </TabsList>
        </ScrollArea.Root>

        {tabsFiltered.map(([key, value]) => {
          return (
            <TabsContent key={key} value={key}>
              <pre
                data-pm={key}
                className="group/tabs-content-pre overscroll-x-contain p-4 leading-6 not-data-[pm=prompt]:overflow-x-auto"
              >
                <code
                  data-slot="code-block"
                  data-language="bash"
                  className="font-mono text-sm/none text-muted-foreground group-data-[pm=prompt]/tabs-content-pre:whitespace-normal"
                >
                  <span className="select-none group-data-[pm=prompt]/tabs-content-pre:hidden">
                    ${" "}
                  </span>
                  {value}
                </code>
              </pre>
            </TabsContent>
          )
        })}
      </Tabs>

      <CopyButton
        className="absolute top-2 right-2 z-10 size-6 rounded-md border-none [&_svg:not([class*='size-'])]:size-3.5"
        variant="ghost"
        size="icon-sm"
        text={tabs[packageManager] || ""}
        onCopySuccess={(copiedCommand) => {
          onCopySuccess?.({
            packageManager,
            command: copiedCommand,
          })
        }}
        onCopyError={onCopyError}
      />
    </div>
  )
}

function getIconForPackageManager(manager: PackageManager) {
  switch (manager) {
    case "prompt":
      return (
        <IconPlaceholder
          lucide="TextAlignStartIcon"
          tabler="IconAlignLeft"
          hugeicons="TextAlignLeftIcon"
          phosphor="TextAlignLeftIcon"
          remixicon="RiAlignLeft"
        />
      )
    case "pnpm":
      return (
        <svg viewBox="0 0 24 24">
          <path
            d="M0 0v7.5h7.5V0zm8.25 0v7.5h7.498V0zm8.25 0v7.5H24V0zM8.25 8.25v7.5h7.498v-7.5zm8.25 0v7.5H24v-7.5zM0 16.5V24h7.5v-7.5zm8.25 0V24h7.498v-7.5zm8.25 0V24H24v-7.5z"
            fill="currentColor"
          />
        </svg>
      )
    case "yarn":
      return (
        <svg viewBox="0 0 24 24">
          <path
            d="M12 0C5.375 0 0 5.375 0 12s5.375 12 12 12 12-5.375 12-12S18.625 0 12 0zm.768 4.105c.183 0 .363.053.525.157.125.083.287.185.755 1.154.31-.088.468-.042.551-.019.204.056.366.19.463.375.477.917.542 2.553.334 3.605-.241 1.232-.755 2.029-1.131 2.576.324.329.778.899 1.117 1.825.278.774.31 1.478.273 2.015a5.51 5.51 0 0 0 .602-.329c.593-.366 1.487-.917 2.553-.931.714-.009 1.269.445 1.353 1.103a1.23 1.23 0 0 1-.945 1.362c-.649.158-.95.278-1.821.843-1.232.797-2.539 1.242-3.012 1.39a1.686 1.686 0 0 1-.704.343c-.737.181-3.266.315-3.466.315h-.046c-.783 0-1.214-.241-1.45-.491-.658.329-1.51.19-2.122-.134a1.078 1.078 0 0 1-.58-1.153 1.243 1.243 0 0 1-.153-.195c-.162-.25-.528-.936-.454-1.946.056-.723.556-1.367.88-1.71a5.522 5.522 0 0 1 .408-2.256c.306-.727.885-1.348 1.32-1.737-.32-.537-.644-1.367-.329-2.21.227-.602.412-.936.82-1.08h-.005c.199-.074.389-.153.486-.259a3.418 3.418 0 0 1 2.298-1.103c.037-.093.079-.185.125-.283.31-.658.639-1.029 1.024-1.168a.94.94 0 0 1 .328-.06zm.006.7c-.507.016-1.001 1.519-1.001 1.519s-1.27-.204-2.266.871c-.199.218-.468.334-.746.44-.079.028-.176.023-.417.672-.371.991.625 2.094.625 2.094s-1.186.839-1.626 1.881c-.486 1.144-.338 2.261-.338 2.261s-.843.732-.899 1.487c-.051.663.139 1.2.343 1.515.227.343.51.176.51.176s-.561.653-.037.931c.477.25 1.283.394 1.71-.037.31-.31.371-1.001.486-1.283.028-.065.12.111.209.199.097.093.264.195.264.195s-.755.324-.445 1.066c.102.246.468.403 1.066.398.222-.005 2.664-.139 3.313-.296.375-.088.505-.283.505-.283s1.566-.431 2.998-1.357c.917-.598 1.293-.76 2.034-.936.612-.148.57-1.098-.241-1.084-.839.009-1.575.44-2.196.825-1.163.718-1.742.672-1.742.672l-.018-.032c-.079-.13.371-1.293-.134-2.678-.547-1.515-1.413-1.881-1.344-1.997.297-.5 1.038-1.297 1.334-2.78.176-.899.13-2.377-.269-3.151-.074-.144-.732.241-.732.241s-.616-1.371-.788-1.483a.271.271 0 0 0-.157-.046z"
            fill="currentColor"
          />
        </svg>
      )
    case "npm":
      return (
        <svg viewBox="0 0 24 24">
          <path
            d="M1.763 0C.786 0 0 .786 0 1.763v20.474C0 23.214.786 24 1.763 24h20.474c.977 0 1.763-.786 1.763-1.763V1.763C24 .786 23.214 0 22.237 0zM5.13 5.323l13.837.019-.009 13.836h-3.464l.01-10.382h-3.456L12.04 19.17H5.113z"
            fill="currentColor"
          />
        </svg>
      )
    case "bun":
      return (
        <svg viewBox="0 0 24 24">
          <path
            d="M12 22.596c6.628 0 12-4.338 12-9.688 0-3.318-2.057-6.248-5.219-7.986-1.286-.715-2.297-1.357-3.139-1.89C14.058 2.025 13.08 1.404 12 1.404c-1.097 0-2.334.785-3.966 1.821a49.92 49.92 0 0 1-2.816 1.697C2.057 6.66 0 9.59 0 12.908c0 5.35 5.372 9.687 12 9.687v.001ZM10.599 4.715c.334-.759.503-1.58.498-2.409 0-.145.202-.187.23-.029.658 2.783-.902 4.162-2.057 4.624-.124.048-.199-.121-.103-.209a5.763 5.763 0 0 0 1.432-1.977Zm2.058-.102a5.82 5.82 0 0 0-.782-2.306v-.016c-.069-.123.086-.263.185-.172 1.962 2.111 1.307 4.067.556 5.051-.082.103-.23-.003-.189-.126a5.85 5.85 0 0 0 .23-2.431Zm1.776-.561a5.727 5.727 0 0 0-1.612-1.806v-.014c-.112-.085-.024-.274.114-.218 2.595 1.087 2.774 3.18 2.459 4.407a.116.116 0 0 1-.049.071.11.11 0 0 1-.153-.026.122.122 0 0 1-.022-.083 5.891 5.891 0 0 0-.737-2.331Zm-5.087.561c-.617.546-1.282.76-2.063 1-.117 0-.195-.078-.156-.181 1.752-.909 2.376-1.649 2.999-2.778 0 0 .155-.118.188.085 0 .304-.349 1.329-.968 1.874Zm4.945 11.237a2.957 2.957 0 0 1-.937 1.553c-.346.346-.8.565-1.286.62a2.178 2.178 0 0 1-1.327-.62 2.955 2.955 0 0 1-.925-1.553.244.244 0 0 1 .064-.198.234.234 0 0 1 .193-.069h3.965a.226.226 0 0 1 .19.07c.05.053.073.125.063.197Zm-5.458-2.176a1.862 1.862 0 0 1-2.384-.245 1.98 1.98 0 0 1-.233-2.447c.207-.319.503-.566.848-.713a1.84 1.84 0 0 1 1.092-.11c.366.075.703.261.967.531a1.98 1.98 0 0 1 .408 2.114 1.931 1.931 0 0 1-.698.869v.001Zm8.495.005a1.86 1.86 0 0 1-2.381-.253 1.964 1.964 0 0 1-.547-1.366c0-.384.11-.76.32-1.079.207-.319.503-.567.849-.713a1.844 1.844 0 0 1 1.093-.108c.367.076.704.262.968.534a1.98 1.98 0 0 1 .4 2.117 1.932 1.932 0 0 1-.702.868Z"
            fill="currentColor"
          />
        </svg>
      )
    default:
      return (
        <IconPlaceholder
          lucide="TerminalIcon"
          tabler="IconTerminal"
          hugeicons="TerminalIcon"
          phosphor="TerminalIcon"
          remixicon="RiTerminalLine"
        />
      )
  }
}

/**
 * Result of converting an npm command to all package managers.
 */
export type ConvertNpmCommandResult = {
  /**
   * Command for pnpm package manager.
   */
  pnpm: string

  /**
   * Command for yarn package manager.
   */
  yarn: string

  /**
   * Command for npm package manager.
   */
  npm: string

  /**
   * Command for bun package manager.
   */
  bun: string
}

/**
 * Converts a standard npm command into equivalent commands for pnpm, yarn, npm,
 * and bun. The result can be spread directly into `CodeBlockCommand` props.
 *
 * Supported command patterns:
 * - `npm install <pkg>` -> add commands for each manager
 * - `npx create-<name>` -> create commands for each manager
 * - `npm create <name>` -> create commands for each manager
 * - `npx <command>` -> execute commands for each manager
 * - `npm run <script>` -> run commands for each manager
 *
 * Unrecognized commands are returned as-is for all package managers.
 *
 * @param npmCommand - A standard npm/npx command string.
 * @returns An object with `pnpm`, `yarn`, `npm`, and `bun` command strings.
 *
 * @example
 * ```tsx
 * import { CodeBlockCommand, convertNpmCommand } from "@/components/ncdai/code-block-command"
 *
 * <CodeBlockCommand {...convertNpmCommand("npx shadcn add button")} />
 * ```
 */
export function convertNpmCommand(npmCommand: string): ConvertNpmCommandResult {
  // npm install
  if (npmCommand.startsWith("npm install")) {
    return {
      pnpm: npmCommand.replaceAll("npm install", "pnpm add"),
      yarn: npmCommand.replaceAll("npm install", "yarn add"),
      npm: npmCommand,
      bun: npmCommand.replaceAll("npm install", "bun add"),
    }
  }

  // npx create- (must be checked before generic npx)
  if (npmCommand.startsWith("npx create-")) {
    return {
      pnpm: npmCommand.replace("npx create-", "pnpm create "),
      yarn: npmCommand.replace("npx create-", "yarn create "),
      npm: npmCommand,
      bun: npmCommand.replace("npx", "bunx --bun"),
    }
  }

  // npm create
  if (npmCommand.startsWith("npm create")) {
    return {
      pnpm: npmCommand.replace("npm create", "pnpm create"),
      yarn: npmCommand.replace("npm create", "yarn create"),
      npm: npmCommand,
      bun: npmCommand.replace("npm create", "bun create"),
    }
  }

  // npx (general)
  if (npmCommand.startsWith("npx")) {
    return {
      pnpm: npmCommand.replace("npx", "pnpm dlx"),
      yarn: npmCommand.replace("npx", "yarn dlx"),
      npm: npmCommand,
      bun: npmCommand.replace("npx", "bunx --bun"),
    }
  }

  // npm run
  if (npmCommand.startsWith("npm run")) {
    return {
      pnpm: npmCommand.replace("npm run", "pnpm"),
      yarn: npmCommand.replace("npm run", "yarn"),
      npm: npmCommand,
      bun: npmCommand.replace("npm run", "bun"),
    }
  }

  return {
    pnpm: npmCommand,
    yarn: npmCommand,
    npm: npmCommand,
    bun: npmCommand,
  }
}
```

##### File: `src/components/base/ui/tabs.tsx`

```tsx
"use client"

import { Tabs as TabsPrimitive } from "@base-ui/react/tabs"

import { cn } from "@/lib/utils"

function Tabs({ className, ...props }: TabsPrimitive.Root.Props) {
  return (
    <TabsPrimitive.Root
      data-slot="tabs"
      className={cn("flex flex-col gap-2", className)}
      {...props}
    />
  )
}

function TabsList({ className, ...props }: TabsPrimitive.List.Props) {
  return (
    <TabsPrimitive.List
      data-slot="tabs-list"
      className={cn(
        "relative z-0 flex h-8 w-fit items-center justify-center rounded-lg p-0.5",
        "bg-zinc-50 text-muted-foreground dark:bg-zinc-900",
        "inset-ring-1 inset-ring-border/64",
        className
      )}
      {...props}
    />
  )
}

function TabsIndicator({ className, ...props }: TabsPrimitive.Indicator.Props) {
  return (
    <TabsPrimitive.Indicator
      data-slot="tabs-indicator"
      className={cn(
        "absolute bottom-0 left-0 -z-1 h-(--active-tab-height) w-(--active-tab-width) translate-x-(--active-tab-left) -translate-y-(--active-tab-bottom) rounded-md bg-white transition-[width,translate] duration-200 ease-in-out dark:bg-muted",
        "inset-ring-1 inset-ring-foreground/10 dark:inset-ring-foreground/6",
        className
      )}
      {...props}
    />
  )
}

function TabsTrigger({ className, ...props }: TabsPrimitive.Tab.Props) {
  return (
    <TabsPrimitive.Tab
      data-slot="tabs-trigger"
      className={cn(
        "flex flex-1 shrink-0 items-center justify-center gap-2 rounded-md px-4 py-1 font-sans text-sm font-medium whitespace-nowrap transition-[color,background-color] outline-none hover:text-foreground focus-visible:inset-ring-1 focus-visible:inset-ring-ring disabled:pointer-events-none disabled:opacity-50 data-active:text-foreground [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    />
  )
}

function TabsContent({ className, ...props }: TabsPrimitive.Panel.Props) {
  return (
    <TabsPrimitive.Panel
      data-slot="tabs-content"
      className={cn("flex-1 outline-none", className)}
      {...props}
    />
  )
}

export { Tabs, TabsContent, TabsIndicator, TabsList, TabsTrigger }
```

---

### Consent Manager <a name="consent-manager"></a>

Cookie and tracking consent banner for Next.js, built on c15t.

- **Registry Key**: `consent-manager`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/consent-manager.json
```

#### Dependencies

- **NPM Packages**:
  - `@c15t/nextjs`
- **Local Registry Dependencies**:
  - `button`

#### Component Source Code

##### File: `src/registry/components/consent-manager/consent-manager.tsx`

```tsx
import {
  ConsentManagerDialog,
  ConsentManagerProvider,
  CookieBanner,
} from "@c15t/nextjs"

import { cn } from "@/lib/utils"
import { buttonVariants } from "@/components/ui/button"

export function ConsentManager({ children }: { children: React.ReactNode }) {
  return (
    <ConsentManagerProvider
      options={{
        mode: "offline",
        consentCategories: ["necessary", "measurement"],
        // ignoreGeoLocation: process.env.NODE_ENV === "development", // Useful for development to always view the banner.
      }}
    >
      <CookieBanner
        theme={{
          "banner.card": {
            noStyle: true,
            className: cn(
              "relative w-full max-w-(--banner-max-width) divide-y overflow-hidden rounded-2xl",
              "bg-popover text-popover-foreground shadow-lg ring-1 ring-foreground/10 dark:ring-foreground/20"
            ),
          },
          "banner.header.title": {
            noStyle: true,
            className: "text-base leading-none font-medium text-foreground",
          },
          "banner.header.description": {
            noStyle: true,
            className: "text-sm text-muted-foreground",
          },
          "banner.footer": {
            style: {
              "--banner-footer-background-color": "transparent",
              "--banner-footer-background-color-dark": "transparent",
            },
          },
          "banner.footer.reject-button": {
            noStyle: true,
            className: buttonVariants({ variant: "secondary" }),
          },
          "banner.footer.accept-button": {
            noStyle: true,
            className: buttonVariants({ variant: "secondary" }),
          },
          "banner.footer.customize-button": {
            noStyle: true,
            className: buttonVariants({ variant: "default" }),
          },
        }}
      />

      <ConsentManagerDialog
        theme={{
          "dialog.root": {
            style: {
              "--dialog-height": "100%",
              "--dialog-font-family": "var(--font-sans)",

              "--dialog-background-color": "var(--background)",
              "--dialog-background-color-dark": "var(--background)",

              "--dialog-border-width": "0",
              "--dialog-border-color": "var(--border)",
              "--dialog-border-color-dark": "var(--border)",

              "--dialog-card-radius": "1rem",
              "--dialog-card-shadow":
                "0 0 0 1px var(--dialog-card-shadow-color)",
            },
            className:
              "[--dialog-card-shadow-color:var(--foreground)]/10 dark:[--dialog-card-shadow-color:var(--border)]",
          },
          "dialog.overlay": {
            style: {
              "--dialog-overlay-background-color": "lab(0% 0 0 / 0.2)",
              "--dialog-overlay-background-color-dark": "lab(0% 0 0 / 0.4)",
            },
          },
          "dialog.title": {
            noStyle: true,
            className:
              "text-base leading-none font-medium tracking-tight text-foreground",
          },
          "dialog.description": {
            noStyle: true,
            className: "text-sm text-muted-foreground",
          },
          "dialog.header": {
            style: {
              "--dialog-header-gap": "0.5rem",
              "--dialog-card-gap": "0",
            },
          },
          "dialog.footer": {
            style: {
              "--dialog-border-width": "1px",

              "--dialog-stroke-color": "var(--border)",
              "--dialog-stroke-color-dark": "var(--border)",

              "--dialog-branding-icon-height": "1.125rem",
              "--dialog-branding-font-size": "0.875rem",

              "--dialog-branding-focus-color":
                "color-mix(in oklab, var(--ring) 50%, transparent)",
              "--dialog-branding-focus-color-dark":
                "color-mix(in oklab, var(--ring) 50%, transparent)",

              "--dialog-foreground-color": "var(--muted-foreground)",
              "--dialog-foreground-color-dark": "var(--muted-foreground)",
            },
          },
          "widget.accordion.item": {
            noStyle: true,
            className: "rounded-lg border",
          },
          "widget.accordion.trigger": {
            noStyle: true,
            className: "flex items-center gap-3 pr-3",
          },
          "widget.accordion.trigger-inner": {
            noStyle: true,
            className: cn(
              "flex flex-1 items-center rounded-lg px-4 py-3 text-sm font-medium text-foreground **:[svg]:hidden",
              "outline-none focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50"
            ),
          },
          "widget.accordion.content": {
            noStyle: true,
            className:
              "overflow-hidden px-4 data-[state=closed]:animate-accordion-up data-[state=open]:animate-accordion-down",
          },
          "widget.accordion.content-inner": {
            noStyle: true,
            className: "pb-4 text-sm text-muted-foreground",
          },
          "widget.switch": {
            noStyle: true,
            className: cn(
              "inline-flex h-4.5 w-8 shrink-0 items-center rounded-full transition-all",
              "border border-transparent",
              "outline-none focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50",
              "disabled:cursor-not-allowed disabled:opacity-50",
              "data-[state=checked]:bg-primary data-[state=unchecked]:bg-input dark:data-[state=unchecked]:bg-input/80"
            ),
          },
          "widget.switch.track": {
            noStyle: true,
          },
          "widget.switch.thumb": {
            noStyle: true,
            className: cn(
              "pointer-events-none block size-4 rounded-full bg-background ring-0 transition-[translate,background-color]",
              "data-[state=checked]:translate-x-[calc(100%-2px)] data-[state=unchecked]:translate-x-0 dark:data-[state=checked]:bg-primary-foreground dark:data-[state=unchecked]:bg-foreground"
            ),
          },
          "widget.footer.sub-group": {
            noStyle: true,
            className: "grid grid-cols-2 gap-4 sm:flex",
          },
          "widget.footer.reject-button": {
            noStyle: true,
            className: buttonVariants({ variant: "secondary" }),
          },
          "widget.footer.accept-button": {
            noStyle: true,
            className: buttonVariants({ variant: "secondary" }),
          },
          "widget.footer.save-button": {
            noStyle: true,
            className: buttonVariants({ variant: "default" }),
          },
        }}
      />

      {children}
    </ConsentManagerProvider>
  )
}
```

---

### Copy Button <a name="copy-button"></a>

Copy text to clipboard with visual, haptic, and audio feedback.

- **Registry Key**: `copy-button`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/copy-button.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `@rexa-developer/tiks`
  - `web-haptics`
- **Local Registry Dependencies**:
  - `button`
  - `https://chanhdai.com/r/icon-swap.json`

#### How to Use

##### Example 1

```tsx
import { CopyButton } from "@/components/copy-button"
```

##### Example 2

```tsx
<CopyButton text="Some text to copy" />
```

#### Component Source Code

##### File: `src/registry/components/copy-button/copy-button.tsx`

```tsx
"use client"

import type { ComponentProps } from "react"
import { motion } from "motion/react"

import type { CopyState } from "@/hooks/use-copy-to-clipboard"
import { useCopyToClipboard } from "@/hooks/use-copy-to-clipboard"
import { Button } from "@/components/ui/button"
import { IconSwap, IconSwapItem } from "@/registry/components/icon-swap"
import { IconPlaceholder } from "@/registry/icons/icon-placeholder"

export type CopyStateIconProps = {
  state: CopyState
  /** Custom icon for idle state. */
  idleIcon?: React.ReactNode
  /** Custom icon for done state. */
  doneIcon?: React.ReactNode
  /** Custom icon for error state. */
  errorIcon?: React.ReactNode
}

export function CopyStateIcon({
  state,
  idleIcon,
  doneIcon,
  errorIcon,
}: CopyStateIconProps) {
  return (
    <IconSwap>
      <IconSwapItem key={state} as={motion.span}>
        {state === "idle" &&
          (idleIcon ?? (
            <IconPlaceholder
              data-slot="idle-icon"
              lucide="CopyIcon"
              tabler="IconCopy"
              hugeicons="Copy01Icon"
              phosphor="CopyIcon"
              remixicon="RiFileCopyLine"
            />
          ))}

        {state === "done" &&
          (doneIcon ?? (
            <IconPlaceholder
              data-slot="done-icon"
              lucide="CheckIcon"
              tabler="IconCheck"
              hugeicons="Tick02Icon"
              phosphor="CheckIcon"
              remixicon="RiCheckLine"
            />
          ))}

        {state === "error" &&
          (errorIcon ?? (
            <IconPlaceholder
              data-slot="error-icon"
              lucide="CircleXIcon"
              tabler="IconX"
              hugeicons="CancelCircleIcon"
              phosphor="XCircleIcon"
              remixicon="RiCloseCircleLine"
            />
          ))}
      </IconSwapItem>
    </IconSwap>
  )
}

export type CopyButtonProps = ComponentProps<typeof Button> & {
  /** The text to copy, or a function that returns the text. */
  text: string | (() => string)
  /** Called with the copied text on successful copy. */
  onCopySuccess?: (text: string) => void
  /** Called with the error if the copy operation fails. */
  onCopyError?: (error: Error) => void
} & Omit<CopyStateIconProps, "state">

export function CopyButton({
  size = "icon",
  children,
  text,
  idleIcon,
  doneIcon,
  errorIcon,
  onClick,
  onCopySuccess,
  onCopyError,
  ...props
}: CopyButtonProps) {
  const { state, copy } = useCopyToClipboard({
    onCopySuccess,
    onCopyError,
  })

  return (
    <Button
      size={size}
      onClick={(e) => {
        copy(text)
        onClick?.(e)
      }}
      aria-label="Copy"
      {...props}
    >
      <CopyStateIcon
        state={state}
        idleIcon={idleIcon}
        doneIcon={doneIcon}
        errorIcon={errorIcon}
      />
      {children}
    </Button>
  )
}
```

##### File: `src/hooks/use-copy-to-clipboard.ts`

```tsx
"use client"

import { useCallback, useRef, useState } from "react"
import { useTiks } from "@rexa-developer/tiks/react"
import { useWebHaptics } from "web-haptics/react"

export type CopyState = "idle" | "done" | "error"

export type UseCopyToClipboardOptions = {
  onCopySuccess?: (text: string) => void
  onCopyError?: (error: Error) => void
  resetDelay?: number
}

export function useCopyToClipboard({
  onCopySuccess,
  onCopyError,
  resetDelay = 1500,
}: UseCopyToClipboardOptions = {}) {
  const [state, setState] = useState<CopyState>("idle")
  const resetTimeoutRef = useRef<ReturnType<typeof setTimeout> | null>(null)

  const { trigger: haptic } = useWebHaptics()
  const { success: tiksSuccess, error: tiksError } = useTiks()

  const copy = useCallback(
    async (text: string | (() => string)) => {
      // Clear any pending reset
      if (resetTimeoutRef.current) {
        clearTimeout(resetTimeoutRef.current)
      }

      try {
        const finalText = typeof text === "function" ? text() : text
        await navigator.clipboard.writeText(finalText)

        setState("done")

        haptic("success")
        tiksSuccess()

        onCopySuccess?.(finalText)
      } catch (error) {
        setState("error")

        haptic("error")
        tiksError()

        onCopyError?.(error instanceof Error ? error : new Error("Copy failed"))
      } finally {
        // Schedule reset to idle
        resetTimeoutRef.current = setTimeout(() => {
          setState("idle")
        }, resetDelay)
      }
    },
    [onCopySuccess, onCopyError, haptic, tiksSuccess, tiksError, resetDelay]
  )

  return { state, copy } as const
}
```

---

### Dot Grid Spotlight <a name="dot-grid-spotlight"></a>

Interactive dot grid with a cursor-tracking spotlight effect.

- **Registry Key**: `dot-grid-spotlight`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/dot-grid-spotlight.json
```

#### How to Use

##### Example 1

```tsx
import { DotGridSpotlight } from "@/components/dot-grid-spotlight"
```

##### Example 2

```tsx
<div className="relative aspect-square w-xs">
  <DotGridSpotlight />
</div>
```

#### Component Source Code

##### File: `src/registry/components/dot-grid-spotlight/dot-grid-spotlight.tsx`

```tsx
"use client"

import React, { useEffect, useRef } from "react"

import { cn } from "@/lib/utils"

export type DotGridSpotlightProps = {
  /**
   * The base color of the default/inactive dots.
   * @default "rgba(255, 255, 255, 0.05)"
   */
  dotColor?: string

  /**
   * The color of the active dots when illuminated by the cursor's spotlight.
   * @default "rgba(255, 255, 255, 0.1)"
   */
  activeDotColor?: string

  /**
   * The distance (in pixels) between each dot in the grid.
   * @default 10
   */
  spacing?: number

  /**
   * The default radius of the dots when they are outside the interaction area.
   * @default 1
   */
  baseRadius?: number

  /**
   * The maximum radius of a dot when it is at the exact center of the cursor.
   * @default 2
   */
  activeRadius?: number

  /**
   * The radius (in pixels) of the interaction area (spotlight) around the cursor.
   * @default 128
   */
  interactionRadius?: number

  /**
   * The maximum opacity (alpha) at the exact center of the spotlight.
   * Accepts a value between `0` and `1` (e.g., `1` for full opacity).
   * @default 1.0
   */
  activeMaxAlpha?: number

  /**
   * The minimum opacity (alpha) at the outer edge of the spotlight.
   * Accepts a value between `0` and `1` (e.g., a low value for a soft, subtle fade).
   * @default 0.5
   */
  activeMinAlpha?: number

  /**
   * Optional CSS class name to apply to the canvas or its wrapper.
   */
  className?: string
}

export function DotGridSpotlight({
  dotColor = "rgba(255, 255, 255, 0.05)",
  activeDotColor = "rgba(255, 255, 255, 0.1)",
  spacing = 10,
  baseRadius = 1,
  activeRadius = 2,
  interactionRadius = 128,
  activeMaxAlpha = 1.0,
  activeMinAlpha = 0.5,
  className,
}: DotGridSpotlightProps) {
  const canvasRef = useRef<HTMLCanvasElement>(null)
  const mouse = useRef({ x: -1000, y: -1000, isActive: false })

  useEffect(() => {
    const canvas = canvasRef.current
    if (!canvas) return

    const ctx = canvas.getContext("2d")
    if (!ctx) return

    let width = 0
    let height = 0
    let renderFrameId: number | null = null

    const draw = () => {
      ctx.clearRect(0, 0, width, height)

      const offsetX = (width % spacing) / 2
      const offsetY = (height % spacing) / 2

      for (let x = offsetX; x <= width; x += spacing) {
        for (let y = offsetY; y <= height; y += spacing) {
          const dx = x - mouse.current.x
          const dy = y - mouse.current.y
          const distance = Math.sqrt(dx * dx + dy * dy)

          let currentRadius = baseRadius
          let currentColor = dotColor
          let currentAlpha = 1.0

          if (mouse.current.isActive && distance < interactionRadius) {
            const factor = 1 - distance / interactionRadius
            currentRadius = baseRadius + (activeRadius - baseRadius) * factor
            currentColor = activeDotColor
            currentAlpha =
              activeMinAlpha + (activeMaxAlpha - activeMinAlpha) * factor
          }

          ctx.globalAlpha = currentAlpha
          ctx.beginPath()
          ctx.arc(x, y, currentRadius, 0, Math.PI * 2)
          ctx.fillStyle = currentColor
          ctx.fill()
        }
      }
      ctx.globalAlpha = 1.0
    }

    const resizeCanvas = () => {
      const parent = canvas.parentElement
      if (!parent) return

      const dpr = window.devicePixelRatio || 1
      width = parent.clientWidth
      height = parent.clientHeight

      if (width === 0 || height === 0) return

      canvas.width = width * dpr
      canvas.height = height * dpr
      canvas.style.width = `${width}px`
      canvas.style.height = `${height}px`
      ctx.scale(dpr, dpr)

      draw()

      requestAnimationFrame(() => {
        canvas.dataset.ready = "true"
      })
    }

    const handleMouseMove = (e: MouseEvent) => {
      const rect = canvas.getBoundingClientRect()
      mouse.current = {
        x: e.clientX - rect.left,
        y: e.clientY - rect.top,
        isActive: true,
      }

      if (renderFrameId === null) {
        renderFrameId = requestAnimationFrame(() => {
          draw()
          renderFrameId = null
        })
      }
    }

    const handleMouseLeave = () => {
      mouse.current.isActive = false
      if (renderFrameId === null) {
        renderFrameId = requestAnimationFrame(() => {
          draw()
          renderFrameId = null
        })
      }
    }

    canvas.addEventListener("mousemove", handleMouseMove)
    canvas.addEventListener("mouseleave", handleMouseLeave)

    const resizeObserver = new ResizeObserver(() => resizeCanvas())
    if (canvas.parentElement) resizeObserver.observe(canvas.parentElement)

    resizeCanvas()

    return () => {
      canvas.removeEventListener("mousemove", handleMouseMove)
      canvas.removeEventListener("mouseleave", handleMouseLeave)
      resizeObserver.disconnect()
      if (renderFrameId !== null) cancelAnimationFrame(renderFrameId)
    }
  }, [
    spacing,
    baseRadius,
    activeRadius,
    interactionRadius,
    dotColor,
    activeDotColor,
    activeMaxAlpha,
    activeMinAlpha,
  ])

  const handleMouseMove = (e: React.MouseEvent<HTMLCanvasElement>) => {
    const rect = canvasRef.current?.getBoundingClientRect()
    if (rect) {
      mouse.current = {
        x: e.clientX - rect.left,
        y: e.clientY - rect.top,
        isActive: true,
      }
    }
  }

  const handleMouseLeave = () => {
    mouse.current.isActive = false
  }

  return (
    <canvas
      ref={canvasRef}
      data-ready="false"
      className={cn(
        "pointer-events-auto absolute inset-0 block opacity-0 transition-opacity! duration-500 data-[ready=true]:opacity-100",
        className
      )}
      onMouseMove={handleMouseMove}
      onMouseLeave={handleMouseLeave}
    />
  )
}
```

---

### Elastic Slider <a name="elastic-slider"></a>

Slider with elastic rubber-band drag and magnetic snap feedback.

- **Registry Key**: `elastic-slider`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/elastic-slider.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
- **Local Registry Dependencies**:
  - `https://chanhdai.com/r/use-controllable-state.json`

#### How to Use

##### Example 1

```tsx
import { ElasticSlider } from "@/components/elastic-slider"
```

##### Example 2

```tsx
const [opacity, setOpacity] = useState(0.5)

<ElasticSlider
  label="Opacity"
  min={0}
  max={1}
  value={opacity}
  onValueChange={setOpacity}
/>
```

##### Example 3

```tsx
<ElasticSlider label="Opacity" min={0} max={1} defaultValue={0.5} />
```

#### Component Source Code

##### File: `src/registry/components/elastic-slider/elastic-slider.tsx`

```tsx
"use client"

import {
  useCallback,
  useEffect,
  useLayoutEffect,
  useRef,
  useState,
} from "react"
import {
  animate,
  motion,
  useMotionValue,
  useReducedMotion,
  useTransform,
} from "motion/react"

import { cn } from "@/lib/utils"
import { useControllableState } from "@/registry/hooks/use-controllable-state"

// Drag detection & rubber band
const CLICK_THRESHOLD = 3
const DEAD_ZONE = 32
const MAX_CURSOR_RANGE = 200
const MAX_STRETCH = 8

// Layout offsets used by the "handle dodges label/value" calculation.
const HANDLE_BUFFER = 8
const LABEL_OFFSET = 12 + 4
const VALUE_OFFSET = 12 - 8

function clamp(v: number, lo: number, hi: number) {
  return Math.max(lo, Math.min(hi, v))
}

function decimalsForStep(step: number): number {
  const s = step.toString()
  const dot = s.indexOf(".")
  return dot === -1 ? 0 : s.length - dot - 1
}

function roundValue(val: number, step: number): number {
  const raw = Math.round(val / step) * step
  return parseFloat(raw.toFixed(decimalsForStep(step)))
}

// Magnetic snap to the nearest decile when within 3.125% of it.
function snapToDecile(rawValue: number, min: number, max: number): number {
  const normalized = (rawValue - min) / (max - min)
  const nearest = Math.round(normalized * 10) / 10
  if (Math.abs(normalized - nearest) <= 0.03125) {
    return min + nearest * (max - min)
  }
  return rawValue
}

export type ElasticSliderProps = {
  /** Label shown inside the track. */
  label: string

  /** Controlled value. Use together with `onValueChange` */
  value?: number
  /** Initial value for uncontrolled mode. Falls back to `min` */
  defaultValue?: number
  /** Called with the new value on drag, click, or key press. */
  onValueChange?: (value: number) => void

  /**
   * Minimum value.
   * @defaultValue 0 */
  min?: number
  /**
   * Maximum value.
   * @defaultValue 1 */
  max?: number
  /**
   * Smallest increment.
   * @defaultValue 0.01 */
  step?: number
  /** Format the displayed value. Defaults to `value.toFixed(...)` based on `step` */
  formatValue?: (value: number) => string

  className?: string
  /** Accessible name. Falls back to `label` */
  "aria-label"?: string
}

export function ElasticSlider({
  label,

  value: valueProp,
  defaultValue,
  onValueChange,

  min = 0,
  max = 1,
  step = 0.01,
  formatValue,

  className,
  "aria-label": ariaLabel,
}: ElasticSliderProps) {
  const [value = min, setValue] = useControllableState({
    prop: valueProp,
    defaultProp: defaultValue ?? min,
    onChange: onValueChange,
  })

  const shouldReduceMotion = useReducedMotion()

  const wrapperRef = useRef<HTMLDivElement>(null)
  const trackRef = useRef<HTMLDivElement>(null)
  const labelRef = useRef<HTMLSpanElement>(null)
  const valueRef = useRef<HTMLSpanElement>(null)

  const [isInteracting, setIsInteracting] = useState(false)
  const [isDragging, setIsDragging] = useState(false)
  const [isHovered, setIsHovered] = useState(false)
  /** Ring only for Tab focus or keyboard value nudges, not pointer press/drag. */
  const [keyboardFocusRing, setKeyboardFocusRing] = useState(false)

  // Pointer session state — mutable, does not trigger re-renders.
  const pointerDownPos = useRef<{ x: number; y: number } | null>(null)
  const pendingPointerFocusRef = useRef(false)
  const isClickRef = useRef(true)
  const animRef = useRef<ReturnType<typeof animate> | null>(null)
  const wrapperRectRef = useRef<DOMRect | null>(null)
  const scaleRef = useRef(1)

  const percentage = ((value - min) / (max - min)) * 100
  const isActive = isInteracting || isHovered
  const displayValue = formatValue
    ? formatValue(value)
    : value.toFixed(decimalsForStep(step))

  // Fill + handle driven by a single motion value for imperative updates.
  const fillPercent = useMotionValue(percentage)
  const fillWidth = useTransform(fillPercent, (pct) => `${pct}%`)
  const handleLeft = useTransform(
    fillPercent,
    (pct) => `max(4px, calc(${pct}% - 8px))`
  )

  // Rubber band: widens the track and pulls it left when dragged past bounds.
  const rubberStretch = useMotionValue(0)
  const rubberWidth = useTransform(
    rubberStretch,
    (s) => `calc(100% + ${Math.abs(s)}px)`
  )
  const rubberX = useTransform(rubberStretch, (s) => (s < 0 ? s : 0))

  // Sync from props when not interacting and no spring is in flight.
  useEffect(() => {
    if (!isInteracting && !animRef.current) {
      fillPercent.jump(percentage)
    }
  }, [percentage, isInteracting, fillPercent])

  const positionToValue = useCallback(
    (clientX: number) => {
      const rect = wrapperRectRef.current
      if (!rect) return min

      const sceneX = (clientX - rect.left) / scaleRef.current
      const nativeWidth = wrapperRef.current?.offsetWidth ?? rect.width
      const percent = clamp(sceneX / nativeWidth, 0, 1)

      return clamp(min + percent * (max - min), min, max)
    },
    [min, max]
  )

  const percentFromValue = useCallback(
    (v: number) => ((v - min) / (max - min)) * 100,
    [min, max]
  )

  // Animate fill to a target percent, or jump instantly when the user prefers
  // reduced motion. Position still updates — only the spring is skipped.
  const animateFillTo = useCallback(
    (targetPercent: number) => {
      animRef.current?.stop()

      if (shouldReduceMotion) {
        fillPercent.jump(targetPercent)
        animRef.current = null
        return
      }

      animRef.current = animate(fillPercent, targetPercent, {
        type: "spring",
        stiffness: 300,
        damping: 25,
        mass: 0.8,
        onComplete: () => {
          animRef.current = null
        },
      })
    },
    [fillPercent, shouldReduceMotion]
  )

  const computeRubberStretch = useCallback((clientX: number, sign: number) => {
    const rect = wrapperRectRef.current
    if (!rect) return 0

    const distancePast = sign < 0 ? rect.left - clientX : clientX - rect.right
    const overflow = Math.max(0, distancePast - DEAD_ZONE)

    return (
      sign * MAX_STRETCH * Math.sqrt(Math.min(overflow / MAX_CURSOR_RANGE, 1))
    )
  }, [])

  const handlePointerDown = useCallback((e: React.PointerEvent) => {
    e.preventDefault()
    ;(e.target as HTMLElement).setPointerCapture(e.pointerId)

    pointerDownPos.current = { x: e.clientX, y: e.clientY }

    isClickRef.current = true

    setIsInteracting(true)

    pendingPointerFocusRef.current = true
    setKeyboardFocusRing(false)

    // Pointer interactions should move focus to the slider so subsequent
    // keyboard input is received and focus styles match the active state.
    trackRef.current?.focus({ preventScroll: true })
    requestAnimationFrame(() => {
      pendingPointerFocusRef.current = false
    })

    // Snapshot the wrapper rect so later math is immune to layout shifts.
    const wrapper = wrapperRef.current
    if (wrapper) {
      const rect = wrapper.getBoundingClientRect()
      wrapperRectRef.current = rect
      scaleRef.current = rect.width / wrapper.offsetWidth
    }
  }, [])

  const handlePointerMove = useCallback(
    (e: React.PointerEvent) => {
      if (!isInteracting || !pointerDownPos.current) return

      const dx = e.clientX - pointerDownPos.current.x
      const dy = e.clientY - pointerDownPos.current.y

      if (isClickRef.current && Math.hypot(dx, dy) > CLICK_THRESHOLD) {
        isClickRef.current = false
        setIsDragging(true)
      }

      if (isClickRef.current) return

      const rect = wrapperRectRef.current
      if (rect && !shouldReduceMotion) {
        if (e.clientX < rect.left) {
          rubberStretch.jump(computeRubberStretch(e.clientX, -1))
        } else if (e.clientX > rect.right) {
          rubberStretch.jump(computeRubberStretch(e.clientX, 1))
        } else {
          rubberStretch.jump(0)
        }
      }

      const newValue = positionToValue(e.clientX)
      animRef.current?.stop()
      animRef.current = null
      fillPercent.jump(percentFromValue(newValue))
      setValue(roundValue(newValue, step))
    },
    [
      isInteracting,
      positionToValue,
      percentFromValue,
      setValue,
      step,
      fillPercent,
      rubberStretch,
      computeRubberStretch,
      shouldReduceMotion,
    ]
  )

  const handlePointerUp = useCallback(
    (e: React.PointerEvent) => {
      if (!isInteracting) return

      if (isClickRef.current) {
        // Coarse sliders (≤10 positions) snap to the nearest step;
        // continuous sliders keep the decile-magnetic behavior.
        const rawValue = positionToValue(e.clientX)
        const discreteSteps = (max - min) / step
        const snapped =
          discreteSteps <= 10
            ? clamp(min + Math.round((rawValue - min) / step) * step, min, max)
            : snapToDecile(rawValue, min, max)

        animateFillTo(percentFromValue(snapped))
        setValue(roundValue(snapped, step))
      }

      if (!shouldReduceMotion && rubberStretch.get() !== 0) {
        animate(rubberStretch, 0, {
          type: "spring",
          visualDuration: 0.35,
          bounce: 0.15,
        })
      }

      setIsInteracting(false)
      setIsDragging(false)
      pointerDownPos.current = null
    },
    [
      isInteracting,
      positionToValue,
      percentFromValue,
      setValue,
      min,
      max,
      step,
      animateFillTo,
      rubberStretch,
      shouldReduceMotion,
    ]
  )

  const handleKeyDown = useCallback(
    (e: React.KeyboardEvent) => {
      // Shift + Arrow is a Figma-style fast nudge: jumps by 10x the step,
      // independent of the WAI-ARIA Page step (which scales with range).
      const arrowStep = e.shiftKey ? step * 10 : step

      let next: number | null = null

      switch (e.key) {
        case "ArrowRight":
        case "ArrowUp":
          next = value + arrowStep
          break

        case "ArrowLeft":
        case "ArrowDown":
          next = value - arrowStep
          break

        case "Home":
          next = min
          break

        case "End":
          next = max
          break

        default:
          return
      }

      e.preventDefault()

      setKeyboardFocusRing(true)

      const snapped = roundValue(clamp(next, min, max), step)
      animateFillTo(percentFromValue(snapped))
      setValue(snapped)
    },
    [value, min, max, step, animateFillTo, percentFromValue, setValue]
  )

  const handleTrackFocus = useCallback(() => {
    if (!pendingPointerFocusRef.current) {
      setKeyboardFocusRing(true)
    }
  }, [])

  const handleTrackBlur = useCallback(() => {
    setKeyboardFocusRing(false)
  }, [])

  // Measure label + value to derive "dodge" thresholds so the handle fades
  // when it would overlap either text.
  const [dodge, setDodge] = useState({ left: 38, right: 72 })

  useLayoutEffect(() => {
    const wrapper = wrapperRef.current
    if (!wrapper) return

    const measure = () => {
      const trackWidth = wrapper.offsetWidth
      if (trackWidth <= 0) return

      const labelEl = labelRef.current
      const valueEl = valueRef.current

      const left = labelEl
        ? ((LABEL_OFFSET + labelEl.offsetWidth + HANDLE_BUFFER) / trackWidth) *
          100
        : 38

      const right = valueEl
        ? ((trackWidth - VALUE_OFFSET - valueEl.offsetWidth - HANDLE_BUFFER) /
            trackWidth) *
          100
        : 72

      setDodge((prev) => {
        return prev.left === left && prev.right === right
          ? prev
          : { left, right }
      })
    }

    measure()

    const observer = new ResizeObserver(measure)
    observer.observe(wrapper)

    if (labelRef.current) observer.observe(labelRef.current)
    if (valueRef.current) observer.observe(valueRef.current)

    return () => observer.disconnect()
  }, [label, displayValue])

  const valueDodge = percentage < dodge.left || percentage > dodge.right
  const handleOpacity = !isActive
    ? 0
    : valueDodge
      ? 0.1
      : isDragging
        ? 0.8
        : 0.5

  const discreteSteps = (max - min) / step
  const hashMarkCount = discreteSteps <= 10 ? discreteSteps - 1 : 9

  const hashMarkPct = (i: number) => {
    return discreteSteps <= 10
      ? (((i + 1) * step) / (max - min)) * 100
      : (i + 1) * 10
  }

  return (
    <div
      ref={wrapperRef}
      data-slot="elastic-slider"
      className={cn(
        "[--elastic-slider-height:--spacing(9)] [--elastic-slider-radius:var(--radius-lg)]",
        "[--elastic-slider-bg:var(--muted)]",
        "[--elastic-slider-fill:var(--muted-foreground)]/10",
        "[--elastic-slider-fill-active:var(--muted-foreground)]/20",
        "[--elastic-slider-hash:var(--muted-foreground)]/30",
        "[--elastic-slider-handle:var(--foreground)]",
        "[--elastic-slider-label:var(--muted-foreground)]",
        "[--elastic-slider-focus:var(--foreground)]",
        "relative h-(--elastic-slider-height)",
        className
      )}
    >
      <motion.div
        ref={trackRef}
        role="slider"
        tabIndex={0}
        data-slot="elastic-slider-track"
        data-active={isActive}
        data-focus-visible={keyboardFocusRing}
        aria-label={ariaLabel ?? label}
        aria-orientation="horizontal"
        aria-valuemin={min}
        aria-valuemax={max}
        aria-valuenow={value}
        aria-valuetext={displayValue}
        className={cn(
          "group/elastic-slider absolute inset-0 cursor-pointer touch-none overflow-hidden rounded-(--elastic-slider-radius) bg-(--elastic-slider-bg) outline-none select-none",
          "data-[focus-visible=true]:ring-2 data-[focus-visible=true]:ring-ring/50 data-[focus-visible=true]:ring-offset-1 data-[focus-visible=true]:ring-offset-background"
        )}
        style={{ width: rubberWidth, x: rubberX }}
        onPointerDown={handlePointerDown}
        onPointerMove={handlePointerMove}
        onPointerUp={handlePointerUp}
        onFocus={handleTrackFocus}
        onBlur={handleTrackBlur}
        onKeyDown={handleKeyDown}
        onMouseEnter={() => setIsHovered(true)}
        onMouseLeave={() => setIsHovered(false)}
      >
        <div
          data-slot="elastic-slider-hash-marks"
          aria-hidden="true"
          className="pointer-events-none absolute inset-0"
        >
          {Array.from({ length: hashMarkCount }, (_, i) => (
            <div
              key={i}
              className={cn(
                "absolute top-1/2 h-2 w-px -translate-x-1/2 -translate-y-1/2 rounded-full transition-colors duration-200",
                "bg-transparent group-data-[active=true]/elastic-slider:bg-(--elastic-slider-hash)"
              )}
              style={{ left: `${hashMarkPct(i)}%` }}
            />
          ))}
        </div>

        <motion.div
          data-slot="elastic-slider-fill"
          aria-hidden="true"
          className={cn(
            "pointer-events-none absolute inset-y-0 left-0 transition-colors",
            "bg-(--elastic-slider-fill) group-data-[active=true]/elastic-slider:bg-(--elastic-slider-fill-active)"
          )}
          style={{ width: fillWidth }}
        />

        <motion.div
          data-slot="elastic-slider-handle"
          aria-hidden="true"
          className="pointer-events-none absolute top-1/2 h-5 w-1 rounded-full bg-(--elastic-slider-handle)"
          style={{ left: handleLeft, y: "-50%" }}
          animate={{
            opacity: handleOpacity,
            scaleX: isActive ? 1 : 0.25,
            scaleY: isActive && valueDodge ? 0.75 : 1,
          }}
          transition={
            shouldReduceMotion
              ? { duration: 0 }
              : {
                  scaleX: {
                    type: "spring",
                    visualDuration: 0.25,
                    bounce: 0.15,
                  },
                  scaleY: { type: "spring", visualDuration: 0.2, bounce: 0.1 },
                  opacity: { duration: 0.15 },
                }
          }
        />

        <span
          ref={labelRef}
          data-slot="elastic-slider-label"
          aria-hidden="true"
          className="pointer-events-none absolute top-1/2 left-3 inline-flex -translate-y-1/2 items-center text-sm/none font-medium text-(--elastic-slider-label) transition-colors"
        >
          {label}
        </span>

        <span
          ref={valueRef}
          data-slot="elastic-slider-value"
          aria-hidden="true"
          className={cn(
            "pointer-events-none absolute top-1/2 right-3 -translate-y-1/2 font-mono text-sm/none font-medium transition-colors",
            "text-(--elastic-slider-label) group-data-[active=true]/elastic-slider:text-(--elastic-slider-focus)"
          )}
        >
          {displayValue}
        </span>
      </motion.div>
    </div>
  )
}
```

---

### Fluid Gradient Text <a name="fluid-gradient-text"></a>

Render text with a fluid gradient that shifts with pointer movement.

- **Registry Key**: `fluid-gradient-text`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/fluid-gradient-text.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

##### Example 1

```tsx
import { FluidGradientText } from "@/components/fluid-gradient-text"
```

##### Example 2

```tsx
<FluidGradientText text="shadcn" />
```

#### Component Source Code

##### File: `src/registry/components/fluid-gradient-text/fluid-gradient-text.tsx`

```tsx
"use client"

import { motion, useMotionValue, useSpring } from "motion/react"

export type FluidGradientTextProps = {
  /** Text content rendered inside the SVG. */
  text: string
  /**
   * SVG viewBox width used to scale the gradient and text layout.
   * @default 1200
   * */
  svgViewBoxWidth?: number
  /**
   * SVG viewBox height used as the base text size.
   * @default 300
   * */
  svgViewBoxHeight?: number
}

export function FluidGradientText({
  text,
  svgViewBoxWidth = 1200,
  svgViewBoxHeight = 300,
}: FluidGradientTextProps) {
  const gradientX1Raw = useMotionValue(svgViewBoxWidth / 2)
  const gradientX1 = useSpring(gradientX1Raw, {
    stiffness: 200,
    damping: 30,
    mass: 0.5,
  })

  const handleMouseMove = (event: React.MouseEvent<HTMLDivElement>) => {
    const container = event.currentTarget
    const containerRect = container.getBoundingClientRect()
    const mouseX = event.clientX - containerRect.left
    const containerWidth = containerRect.width

    const normalizedX = (mouseX / containerWidth) * svgViewBoxWidth
    const clampedX = Math.max(0, Math.min(svgViewBoxWidth, normalizedX))

    gradientX1Raw.set(clampedX)
  }

  const handleMouseLeave = () => {
    gradientX1Raw.set(svgViewBoxWidth / 2)
  }

  return (
    <div
      className="relative size-full overflow-hidden after:absolute after:bottom-0 after:h-px after:w-full after:bg-current/15"
      onMouseMove={handleMouseMove}
      onMouseLeave={handleMouseLeave}
    >
      <svg
        className="size-full translate-y-[37.5%] select-none"
        viewBox={`0 0 ${svgViewBoxWidth} ${svgViewBoxHeight}`}
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <text
          x="50%"
          y="50%"
          textAnchor="middle"
          dominantBaseline="central"
          stroke="currentColor"
          strokeOpacity="0.1"
          strokeWidth="2"
          fill="url(#fluid_gradient_text_linear)"
          style={{
            fontFamily: "Helvetica",
            fontSize: svgViewBoxHeight,
            fontWeight: "bold",
          }}
        >
          {text}
        </text>
        <defs>
          <motion.linearGradient
            id="fluid_gradient_text_linear"
            x1={gradientX1}
            y1="0"
            x2={svgViewBoxWidth / 2}
            y2={svgViewBoxHeight}
            gradientUnits="userSpaceOnUse"
          >
            <stop offset="0.625" stopColor="currentColor" stopOpacity="0" />
            <stop offset="1" stopColor="currentColor" />
          </motion.linearGradient>
        </defs>
      </svg>
    </div>
  )
}
```

---

### Followed by @shadcn on X <a name="followed-by-shadcn-on-x"></a>

Still can’t believe it — such an honor.

- **Registry Key**: `followed-by-shadcn-on-x`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/followed-by-shadcn-on-x.json
```

---

### GitHub Contributions <a name="github-contributions"></a>

Visualize year-long GitHub contribution activity with daily counts, tooltips, and a profile link.

- **Registry Key**: `github-contributions`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/github-contributions.json
```

#### Dependencies

- **NPM Packages**:
  - `date-fns`
- **Local Registry Dependencies**:
  - `tooltip`
  - `spinner`
  - `https://chanhdai.com/r/contribution-graph.json`

#### Component Source Code

##### File: `src/registry/components/github-contributions/github-contributions.tsx`

```tsx
"use client"

import { use } from "react"
import { format } from "date-fns"

import { cn } from "@/lib/utils"
import { Spinner } from "@/components/ui/spinner"
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from "@/components/ui/tooltip"
import type { Activity } from "@/registry/components/contribution-graph"
import {
  ContributionGraph,
  ContributionGraphBlock,
  ContributionGraphCalendar,
  ContributionGraphFooter,
  ContributionGraphLegend,
  ContributionGraphTotalCount,
} from "@/registry/components/contribution-graph"

export function GitHubContributions({
  contributions,
  githubProfileUrl,
  className,
}: {
  contributions: Promise<Activity[]>
  githubProfileUrl: string
  className?: string
}) {
  const data = use(contributions)

  return (
    <ContributionGraph
      className={cn("mx-auto py-2", className)}
      data={data}
      blockSize={11}
      blockMargin={3}
      blockRadius={2}
    >
      <ContributionGraphCalendar
        className="no-scrollbar px-2"
        title="GitHub Contributions"
      >
        {({ activity, dayIndex, weekIndex }) => (
          <Tooltip>
            <TooltipTrigger asChild>
              <g>
                <ContributionGraphBlock
                  activity={activity}
                  dayIndex={dayIndex}
                  weekIndex={weekIndex}
                />
              </g>
            </TooltipTrigger>
            <TooltipContent className="font-sans">
              <p>
                {activity.count} contribution{activity.count > 1 ? "s" : null}{" "}
                on {format(new Date(activity.date), "dd.MM.yyyy")}
              </p>
            </TooltipContent>
          </Tooltip>
        )}
      </ContributionGraphCalendar>

      <ContributionGraphFooter className="px-2">
        <ContributionGraphTotalCount>
          {({ totalCount, year }) => (
            <div className="text-muted-foreground">
              {totalCount.toLocaleString("en")} contributions in {year} on{" "}
              <a
                className="text-foreground link-underline"
                href={githubProfileUrl}
                target="_blank"
                rel="noopener"
              >
                GitHub
              </a>
              .
            </div>
          )}
        </ContributionGraphTotalCount>

        <ContributionGraphLegend />
      </ContributionGraphFooter>
    </ContributionGraph>
  )
}

export function GitHubContributionsFallback() {
  return (
    <div className="flex h-40.5 w-full items-center justify-center">
      <Spinner className="text-muted-foreground" />
    </div>
  )
}
```

##### File: `src/registry/components/github-contributions/lib/get-cached-contributions.ts`

```tsx
import { unstable_cache } from "next/cache"

import type { Activity } from "@/registry/components/contribution-graph"

type GitHubContributionsResponse = {
  contributions: Activity[]
}

export const getCachedContributions = unstable_cache(
  async (username: string) => {
    const res = await fetch(
      `${process.env.GITHUB_CONTRIBUTIONS_API_URL || `https://github-contributions-api.jogruber.de`}/v4/${username}?y=last`
    )
    const data = (await res.json()) as GitHubContributionsResponse
    return data.contributions
  },
  ["github-contributions"],
  { revalidate: 86400 } // Cache for 1 day (86400 seconds)
)
```

---

### GitHub Stars <a name="github-stars"></a>

Display GitHub repo star count with formatted numbers and full-count tooltip.

- **Registry Key**: `github-stars`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/github-stars.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `button`
  - `tooltip`

#### How to Use

##### Example 1

```tsx
import { GithubStars } from "@/components/github-stars"
```

##### Example 2

```tsx
<GithubStars repo="ncdai/chanhdai.com" stargazersCount={1800} />
```

#### Component Source Code

##### File: `src/registry/components/github-stars/github-stars.tsx`

```tsx
import { Button } from "@/components/ui/button"
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from "@/components/ui/tooltip"

export type GitHubStarsProps = {
  /** GitHub repository in `owner/repo` format. */
  repo: string
  /** Number of stars to display. */
  stargazersCount: number
  /**
   * Optional locales for number formatting.
   * See [MDN - Intl - locales argument](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Intl#locales_argument).
   * @defaultValue "en-US"
   */
  locales?: Intl.LocalesArgument
}

export function GitHubStars({
  repo,
  stargazersCount,
  locales = "en-US",
}: GitHubStarsProps) {
  return (
    <Tooltip>
      <TooltipTrigger asChild>
        <Button className="gap-1.5 pr-1.5 pl-2" variant="ghost" asChild>
          <a href={`https://github.com/${repo}`} target="_blank" rel="noopener">
            <svg className="-translate-y-px" viewBox="0 0 24 24">
              <path
                d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"
                fill="currentColor"
              />
            </svg>
            <span className="text-[0.8125rem] text-muted-foreground tabular-nums">
              {new Intl.NumberFormat(locales, {
                notation: "compact",
                compactDisplay: "short",
              })
                .format(stargazersCount)
                .toLowerCase()}
            </span>
          </a>
        </Button>
      </TooltipTrigger>

      <TooltipContent className="tabular-nums">
        {new Intl.NumberFormat(locales).format(stargazersCount)} stars
      </TooltipContent>
    </Tooltip>
  )
}
```

---

### Glow Card Grid <a name="glow-card-grid"></a>

Display cards with glowing border and background effects.

- **Registry Key**: `glow-card-grid`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/glow-card-grid.json
```

#### How to Use

##### Example 1

```tsx
import { GlowCard, GlowCardGrid } from "@/components/glow-card-grid"
```

##### Example 2

```tsx
<GlowCardGrid>
  <GlowCard />
</GlowCardGrid>
```

##### Example 3

```tsx
import { useDialKit } from "dialkit"

import { GlowCard, GlowCardGrid } from "@/components/glow-card-grid"
```

##### Example 4

```tsx
const params = useDialKit("GlowCard", {
  cardRadius: [16, 0, 32, 1],
  icon: {
    blur: [25, 0, 100, 1], // [default, min, max, step]
    saturate: [5.0, 0, 10, 0.1],
    brightness: [1.3, 0, 4, 0.1],
    scale: [4, 1, 6, 0.1],
    opacity: [0.3, 0, 1, 0.01],
  },
  border: {
    width: [3, 1, 6, 1],
    blur: [10, 0, 100, 1],
    saturate: [4.2, 0, 10, 0.1],
    brightness: [2.5, 0, 4, 0.1],
    contrast: [2.5, 0, 3, 0.1],
  },
})

<GlowCardGrid
  // Card parameters
  cardRadius={params.cardRadius}
  // Icon parameters
  iconBlur={params.icon.blur}
  iconSaturate={params.icon.saturate}
  iconBrightness={params.icon.brightness}
  iconScale={params.icon.scale}
  iconOpacity={params.icon.opacity}
  // Border parameters
  borderWidth={params.border.width}
  borderBlur={params.border.blur}
  borderSaturate={params.border.saturate}
  borderBrightness={params.border.brightness}
  borderContrast={params.border.contrast}
>
  <GlowCard />
  <GlowCard />
  <GlowCard />
</GlowCardGrid>
```

#### Component Source Code

##### File: `src/registry/components/glow-card-grid/glow-card-grid.tsx`

```tsx
"use client"

import { useEffect, useRef } from "react"

import { cn } from "@/lib/utils"

export type GlowCardGridProps = React.ComponentPropsWithoutRef<"div"> & {
  // Card parameters
  cardRadius?: number

  // Icon parameters
  iconBlur?: number
  iconSaturate?: number
  iconBrightness?: number
  iconScale?: number
  iconOpacity?: number

  // Border parameters
  borderWidth?: number
  borderBlur?: number
  borderSaturate?: number
  borderBrightness?: number
  borderContrast?: number

  children: React.ReactNode
}

export function GlowCardGrid({
  cardRadius = 16,

  iconBlur = 25,
  iconSaturate = 5.0,
  iconBrightness = 1.3,
  iconScale = 4,
  iconOpacity = 0.3,

  borderWidth = 3,
  borderBlur = 10,
  borderSaturate = 4.2,
  borderBrightness = 2.5,
  borderContrast = 2.5,

  className,
  style,
  ...props
}: GlowCardGridProps) {
  const gridRef = useRef<HTMLDivElement>(null)

  useEffect(() => {
    const handlePointerMove = (event: PointerEvent) => {
      if (!gridRef.current) return

      const cards = gridRef.current.querySelectorAll<HTMLElement>(
        "[data-slot='glow-card']"
      )

      cards.forEach((card) => {
        const rect = card.getBoundingClientRect()

        const centerX = rect.left + rect.width / 2
        const centerY = rect.top + rect.height / 2

        const x = (event.clientX - centerX) / (rect.width / 2)
        const y = (event.clientY - centerY) / (rect.height / 2)

        card.style.setProperty("--pointer-x", x.toFixed(3))
        card.style.setProperty("--pointer-y", y.toFixed(3))
      })
    }

    document.addEventListener("pointermove", handlePointerMove)

    return () => document.removeEventListener("pointermove", handlePointerMove)
  }, [])

  return (
    <div
      ref={gridRef}
      className={cn(
        "grid w-full gap-4 sm:grid-cols-2 md:grid-cols-3",
        className
      )}
      style={
        {
          "--card-radius": `${cardRadius}px`,
          "--card-icon-blur": `${iconBlur}px`,
          "--card-icon-saturate": iconSaturate,
          "--card-icon-brightness": iconBrightness,
          "--card-icon-scale": iconScale,
          "--card-icon-opacity": iconOpacity,
          "--card-border-width": `${borderWidth}px`,
          "--card-border-blur": `${borderBlur}px`,
          "--card-border-saturate": borderSaturate,
          "--card-border-brightness": borderBrightness,
          "--card-border-contrast": borderContrast,
          ...style,
        } as React.CSSProperties
      }
      {...props}
    />
  )
}

export type GlowCardProps = {
  name: string
  handle: string
  avatar: string
  className?: string
}

export function GlowCard({ name, handle, avatar, className }: GlowCardProps) {
  return (
    <div
      data-slot="glow-card"
      className={cn(
        "@container-size relative h-52 w-full overflow-hidden rounded-(--card-radius) ring-1 ring-border transition-[translate,scale] select-none active:scale-[0.98]",
        className
      )}
    >
      <div className="flex size-full overflow-hidden rounded-(--card-radius) [clip-path:inset(0_round_var(--card-radius))]">
        <div
          className={cn(
            "pointer-events-none absolute inset-0 flex items-center justify-center",
            "translate-x-[calc(var(--pointer-x,-10)*50cqi)] translate-y-[calc(var(--pointer-y,-10)*50cqh)] translate-z-0 scale-(--card-icon-scale)",
            "blur-(--card-icon-blur) brightness-(--card-icon-brightness) saturate-(--card-icon-saturate)",
            "opacity-(--card-icon-opacity) will-change-[transform,filter]"
          )}
        >
          <img className="size-20" src={avatar} alt={name} />
        </div>

        <div className="z-1 flex flex-1 flex-col items-center justify-center gap-4">
          <img className="size-20 rounded-full" src={avatar} alt={name} />

          <div className="flex flex-col items-center gap-1">
            <h2 className="text-base leading-none font-semibold text-foreground">
              {name}
            </h2>
            <p className="text-sm leading-none text-foreground/50">{handle}</p>
          </div>
        </div>
      </div>

      <div
        className={cn(
          "pointer-events-none absolute inset-0 translate-z-0 rounded-(--card-radius)",
          "border-(length:--card-border-width) border-solid border-transparent",
          "backdrop-blur-(--card-border-blur) backdrop-brightness-(--card-border-brightness) backdrop-contrast-(--card-border-contrast) backdrop-saturate-(--card-border-saturate)",
          "[clip-path:inset(0_round_var(--card-radius))]"
        )}
        style={
          {
            maskImage:
              "linear-gradient(#fff 0 100%), linear-gradient(#fff 0 100%)",
            maskOrigin: "border-box, padding-box",
            maskClip: "border-box, padding-box",
            maskComposite: "exclude",
            WebkitMaskComposite: "xor",
          } as React.CSSProperties
        }
      />
    </div>
  )
}
```

---

### Grateful for the Feedback on My Portfolio Website <a name="grateful-for-the-feedback-on-my-portfolio-website"></a>

Some kind words from X. Thanks to those behind the tools we rely on.

- **Registry Key**: `grateful-for-the-feedback-on-my-portfolio-website`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/grateful-for-the-feedback-on-my-portfolio-website.json
```

---

### Haptic Feedback <a name="haptic"></a>

Trigger haptic feedback on mobile devices.

- **Registry Key**: `haptic`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/haptic.json
```

#### How to Use

##### Example 1

```tsx
import { haptic } from "@/lib/haptic"
```

##### Example 2

```tsx
<Button onClick={() => haptic()}>Haptic</Button>
```

#### Component Source Code

##### File: `src/registry/lib/haptic/haptic.ts`

```tsx
const isTouchDevice =
  typeof window !== "undefined"
    ? window.matchMedia("(pointer: coarse)").matches
    : false

/**
 * Trigger haptic feedback on mobile devices.
 * Uses Vibration API on Android/modern browsers, and iOS checkbox trick on iOS.
 *
 * @param pattern - Vibration duration (ms) or pattern.
 * Custom patterns only work on Android devices. iOS uses fixed feedback.
 * See [Vibration API](https://developer.mozilla.org/docs/Web/API/Vibration_API)
 *
 * @example
 * import { haptic } from "@/lib/haptic"
 *
 * <Button onClick={() => haptic()}>Haptic</Button>
 */
export function haptic(pattern: number | number[] = 50) {
  try {
    if (!isTouchDevice) return

    if ("vibrate" in navigator) {
      navigator.vibrate(pattern)
      return
    }

    // iOS haptic trick via checkbox switch element
    const label = document.createElement("label")
    label.ariaHidden = "true"
    label.style.display = "none"

    const input = document.createElement("input")
    input.type = "checkbox"
    input.setAttribute("switch", "")
    label.appendChild(input)

    try {
      document.head.appendChild(label)
      label.click()
    } finally {
      document.head.removeChild(label)
    }
  } catch {}
}
```

---

### Icon Swap <a name="icon-swap"></a>

Animate icon swaps with scale, blur, and fade transitions.

- **Registry Key**: `icon-swap`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/icon-swap.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

##### Example 1

```tsx
import { IconSwap, IconSwapItem } from "@/components/icon-swap"
```

##### Example 2

```tsx
<IconSwap>
  <IconSwapItem key={isCopied ? "check" : "copy"}>
    {isCopied ? <CheckIcon /> : <CopyIcon />}
  </IconSwapItem>
</IconSwap>
```

#### Component Source Code

##### File: `src/registry/components/icon-swap/icon-swap.tsx`

```tsx
"use client"

import type { AnimatePresenceProps, HTMLMotionProps } from "motion/react"
import { AnimatePresence, motion } from "motion/react"

export function IconSwap(props: React.PropsWithChildren<AnimatePresenceProps>) {
  return <AnimatePresence mode="popLayout" initial={false} {...props} />
}

type MotionElement = typeof motion.div | typeof motion.span

export function IconSwapItem({
  as: Component = motion.div,
  ...props
}: HTMLMotionProps<"div"> & {
  as?: MotionElement
}) {
  return (
    <Component
      initial={{ opacity: 0, scale: 0.25, filter: "blur(4px)" }}
      animate={{ opacity: 1, scale: 1, filter: "blur(0px)" }}
      exit={{ opacity: 0, scale: 0.25, filter: "blur(4px)" }}
      transition={{
        type: "spring",
        duration: 0.3,
        bounce: 0,
      }}
      {...props}
    />
  )
}
```

---

### Manu Arora reviewed My Portfolio Website <a name="manu-arora-reviewed-my-portfolio-website"></a>

A heartfelt moment when the creator of Aceternity UI — Manu Arora, shared his thoughts on my portfolio website.

- **Registry Key**: `manu-arora-reviewed-my-portfolio-website`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/manu-arora-reviewed-my-portfolio-website.json
```

---

### Middle Truncation <a name="middle-truncation"></a>

Truncate text in the middle while preserving start and end.

- **Registry Key**: `middle-truncation`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/middle-truncation.json
```

#### How to Use

##### Example 1

```tsx
import { MiddleTruncation } from "@/components/middle-truncation"
```

##### Example 2

```tsx
<MiddleTruncation>
  /Users/ncdai/Code/chanhdai.com/src/components/ui/button.tsx
</MiddleTruncation>
```

##### Example 3

```tsx
<MiddleTruncation end={4}>
  FY26_Q1_Consolidated_Financial_Statements.pdf
</MiddleTruncation>
```

##### Example 4

```tsx
<MiddleTruncation minEnd={12}>
  /Users/ncdai/Code/chanhdai.com/node_modules/shadcn/package.json
</MiddleTruncation>
```

##### Example 5

```tsx
<MiddleTruncation>
  /Users/ncdai/Code/chanhdai.com/src/components/ui/button.tsx
</MiddleTruncation>
```

#### Component Source Code

##### File: `src/registry/components/middle-truncation/middle-truncation.tsx`

```tsx
"use client"

import React, { useLayoutEffect, useRef, useState } from "react"

import { cn } from "@/lib/utils"

let cachedCanvas: HTMLCanvasElement | null = null
let cachedCtx: CanvasRenderingContext2D | null = null

/**
 * Returns a singleton canvas 2D context for text measurement.
 * Creates the canvas on first call and reuses it for all subsequent calls.
 *
 * @throws {Error} If canvas 2D context creation fails.
 */
function getCanvas(): CanvasRenderingContext2D {
  if (!cachedCtx) {
    cachedCanvas = document.createElement("canvas")
    const ctx = cachedCanvas.getContext("2d")
    if (!ctx) {
      throw new Error("Failed to get 2d context from canvas")
    }
    cachedCtx = ctx
  }
  return cachedCtx
}

function measureText(text: string, font: string) {
  const ctx = getCanvas()
  ctx.font = font
  return ctx.measureText(text).width
}

function getComputedFont(el: HTMLElement) {
  const cs = window.getComputedStyle(el)
  return `${cs.fontStyle} ${cs.fontWeight} ${cs.fontSize} ${cs.fontFamily}`
}

/**
 * Creates a debounced version of a function that syncs execution with the browser's paint cycle.
 *
 * Combines debouncing (waits for inactivity) with requestAnimationFrame (syncs with browser rendering)
 * to ensure smooth UI updates without jank.
 *
 * @template Args - The argument types of the function.
 * @template Return - The return type of the function (ignored in debounced version).
 * @param fn - The function to debounce.
 * @param delay - Milliseconds to wait before executing after the last call.
 * @returns A debounced version that executes on the next animation frame after the delay.
 *
 * @example
 * const debouncedScroll = debounceWithRAF(handleScroll, 150)
 * window.addEventListener('scroll', debouncedScroll)
 */
function debounceWithRAF<Args extends unknown[], Return = void>(
  fn: (...args: Args) => Return,
  delay: number
): (...args: Args) => void {
  let timeoutId: ReturnType<typeof setTimeout> | undefined
  let rafId: number | undefined

  return (...args: Args): void => {
    if (timeoutId !== undefined) {
      clearTimeout(timeoutId)
    }
    if (rafId !== undefined) {
      cancelAnimationFrame(rafId)
    }

    timeoutId = setTimeout(() => {
      rafId = requestAnimationFrame(() => {
        fn(...args)
      })
    }, delay)
  }
}

/**
 * Truncates text in the middle, preserving the start and end portions.
 *
 * Uses binary search to find the optimal truncation point based on pixel width,
 * ensuring the result fits within the container. The truncated text will be in
 * the format: "start{ellipsis}end".
 *
 * @param text - The text to truncate.
 * @param end - Fixed number of characters to preserve at the end. Mutually exclusive with minEnd.
 * @param minEnd - Minimum characters at the end when splitting evenly. Mutually exclusive with end.
 * @param containerW - Available width in pixels.
 * @param font - CSS font string for accurate measurement.
 * @param ellipsis - The string to use as separator in the middle.
 * @returns The original text if it fits, otherwise truncated text with ellipsis in the middle.
 *
 * @example
 * // Fixed end: always preserve exactly 4 chars at the end
 * computeTruncated("very-long-filename.txt", 4, undefined, 100, "16px Arial", "...")
 * // Returns: "very-long-file...txt"
 *
 * @example
 * // MinEnd: split evenly, but ensure at least 4 chars at the end
 * computeTruncated("document.pdf", undefined, 4, 100, "16px Arial", "...")
 * // Returns: "doc....pdf" (prioritizes minEnd when width is small)
 *
 * @example
 * // No constraints: split evenly in the middle
 * computeTruncated("abcdefghijklmnop", undefined, undefined, 100, "16px Arial", "...")
 * // Returns: "abcd...mnop"
 */
function computeTruncated(
  text: string,
  end: number | undefined,
  minEnd: number | undefined,
  containerW: number,
  font: string,
  ellipsis: string
): string {
  const fullW = measureText(text, font)
  if (fullW <= containerW) return text

  // Strategy 1: Fixed end (always preserve exactly X chars at the end)
  if (end !== undefined) {
    const endStr = text.slice(-end)
    const endW = measureText(ellipsis + endStr, font)
    const available = containerW - endW

    let lo = 0
    let hi = text.length - end
    while (lo < hi) {
      const mid = Math.ceil((lo + hi) / 2)
      if (measureText(text.slice(0, mid), font) <= available) lo = mid
      else hi = mid - 1
    }

    return text.slice(0, lo) + ellipsis + endStr
  }

  // Strategy 2: Split evenly (with optional minEnd constraint)
  const ellipsisW = measureText(ellipsis, font)
  const availableForText = containerW - ellipsisW

  let lo = 0
  let hi = text.length
  while (lo < hi) {
    const mid = Math.ceil((lo + hi) / 2)

    let startLen: number
    let endLen: number

    if (minEnd !== undefined) {
      endLen = Math.max(Math.ceil(mid / 2), minEnd)
      startLen = Math.max(0, mid - endLen)
    } else {
      startLen = Math.floor(mid / 2)
      endLen = Math.ceil(mid / 2)
    }

    const startStr = text.slice(0, startLen)
    const endStr = text.slice(-endLen)
    const combinedW = measureText(startStr + endStr, font)

    if (combinedW <= availableForText) lo = mid
    else hi = mid - 1
  }

  let startLen: number
  let endLen: number

  if (minEnd !== undefined) {
    endLen = Math.max(Math.ceil(lo / 2), minEnd)
    startLen = Math.max(0, lo - endLen)
  } else {
    startLen = Math.floor(lo / 2)
    endLen = Math.ceil(lo / 2)
  }

  return text.slice(0, startLen) + ellipsis + text.slice(-endLen)
}

type BaseProps = React.ComponentPropsWithoutRef<"span"> & {
  /** The text content to truncate. */
  children: string
  /** Custom ellipsis string to show in the middle. @default "..." */
  ellipsis?: string
}

export type MiddleTruncationProps = BaseProps &
  (
    | {
        /** Fixed number of characters to always preserve at the end. Cannot be used with minEnd. */
        end: number
        minEnd?: never
      }
    | {
        /** When splitting evenly, ensure at least this many characters at the end. Cannot be used with end. */
        minEnd: number
        end?: never
      }
    | {
        /** When neither end nor minEnd is provided, splits text evenly in the middle. */
        end?: never
        minEnd?: never
      }
  )

export function MiddleTruncation({
  className,
  children,
  end,
  minEnd,
  ellipsis = "...",
  ...props
}: MiddleTruncationProps) {
  const containerRef = useRef<HTMLSpanElement>(null)
  const [displayed, setDisplayed] = useState<string>(children)

  useLayoutEffect(() => {
    const el = containerRef.current
    if (!el) return

    const recalculate = (width: number) => {
      const font = getComputedFont(el)
      setDisplayed(
        computeTruncated(children, end, minEnd, width, font, ellipsis)
      )
    }

    const debouncedRecalculate = debounceWithRAF(recalculate, 150)

    const ro = new ResizeObserver(([entry]) => {
      debouncedRecalculate(entry.contentRect.width)
    })

    recalculate(el.offsetWidth)
    ro.observe(el)

    return () => ro.disconnect()
  }, [children, end, minEnd, ellipsis])

  return (
    <span
      ref={containerRef}
      className={cn(
        "block overflow-hidden text-ellipsis whitespace-nowrap",
        className
      )}
      title={children}
      {...props}
    >
      {displayed}
    </span>
  )
}
```

---

### Mobius Loop Icon <a name="mobius-loop-icon"></a>

Animated Mobius loop icon that morphs between circles and infinity shape.

- **Registry Key**: `mobius-loop-icon`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/mobius-loop-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

##### Example 1

```tsx
import { MobiusLoopIcon } from "@/components/mobius-loop-icon"
```

##### Example 2

```tsx
<MobiusLoopIcon />
```

#### Component Source Code

##### File: `src/registry/components/mobius-loop-icon/mobius-loop-icon.tsx`

```tsx
"use client"

import type { SVGMotionProps } from "motion/react"
import { motion } from "motion/react"

const circle1 =
  "M12 4C16.42 4 20 7.58 20 12C20 16.42 16.42 20 12 20C7.58 20 4 16.42 4 12C4 7.58 7.58 4 12 4Z"

const infinity =
  "M 6 16 C 11 16 13 8 18 8 C 23.333 8 23.333 16 18 16 C 13 16 11 8 6 8 C 0.667 8 0.667 16 6 16 Z"

const circle2 =
  "M12 20C16.42 20 20 16.42 20 12C20 7.58 16.42 4 12 4C7.58 4 4 7.58 4 12C4 16.42 7.58 20 12 20Z"

export function MobiusLoopIcon(props: SVGMotionProps<SVGSVGElement>) {
  return (
    <motion.svg
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      strokeWidth="2"
      strokeLinecap="round"
      strokeLinejoin="round"
      aria-hidden
      {...props}
    >
      <motion.path
        animate={{
          d: [circle1, infinity, circle2],
        }}
        transition={{
          d: {
            duration: 3,
            ease: "easeInOut",
            repeat: Infinity,
          },
        }}
      />
    </motion.svg>
  )
}
```

---

### OrcDev and Francesco Ciulla Reviewed My Open Source Portfolio <a name="orcdev-and-francesco-ciulla-reviewed-my-open-source-portfolio"></a>

My perfect last day of the year – having my portfolio reviewed by @orcdev and @FrancescoCiull4.

- **Registry Key**: `orcdev-and-francesco-ciulla-reviewed-my-open-source-portfolio`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/orcdev-and-francesco-ciulla-reviewed-my-open-source-portfolio.json
```

---

### OrcDev discovered the WILDEST shadcn component libraries ever <a name="orcdev-discovered-the-wildest-shadcn-component-libraries-ever"></a>

Recently, OrcDev discovered some of the wildest component libraries built on shadcn, and a few of them genuinely stand out. Each library targets a completely different use case and aesthetic, showing just how flexible and future-ready this ecosystem is becoming.

- **Registry Key**: `orcdev-discovered-the-wildest-shadcn-component-libraries-ever`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/orcdev-discovered-the-wildest-shadcn-component-libraries-ever.json
```

---

### React Wheel Picker joins Vercel Open Source Program <a name="react-wheel-picker-joins-vercel-open-source-program"></a>

Excited to share that my open source project is now officially supported by Vercel.

- **Registry Key**: `react-wheel-picker-joins-vercel-open-source-program`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/react-wheel-picker-joins-vercel-open-source-program.json
```

---

### React Wheel Picker <a name="react-wheel-picker"></a>

iOS-like wheel picker for React with smooth inertia scrolling and infinite loop support.

- **Registry Key**: `react-wheel-picker`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/react-wheel-picker.json
```

#### How to Use

##### Example 1

```tsx
import {
  WheelPicker,
  WheelPickerWrapper,
  type WheelPickerOption,
} from "@/components/wheel-picker"
```

##### Example 2

```tsx
const options: WheelPickerOption[] = [
  {
    label: "React",
    value: "react",
  },
  {
    label: "Vue",
    value: "vue",
  },
  {
    label: "Angular",
    value: "angular",
    disabled: true,
  },
  {
    label: "Svelte",
    value: "svelte",
  },
]

export function WheelPickerDemo() {
  const [value, setValue] = useState("react")

  return (
    <WheelPickerWrapper>
      <WheelPicker options={options} value={value} onValueChange={setValue} />
    </WheelPickerWrapper>
  )
}
```

---

### Scroll Fade Effect <a name="scroll-fade-effect"></a>

Fade content edges as you scroll, for both vertical and horizontal layouts.

- **Registry Key**: `scroll-fade-effect`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/scroll-fade-effect.json
```

#### How to Use

##### Example 1

```tsx
import { ScrollFadeEffect } from "@/components/scroll-fade-effect"
```

##### Example 2

```tsx
<div className="rounded-lg border">
  <ScrollFadeEffect className="h-72 w-48">
    Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
    tempor incididunt ut labore et dolore magna aliqua. Turpis egestas pretium
    aenean pharetra. Orci eu lobortis elementum nibh tellus molestie. Vulputate
    dignissim suspendisse in est. Vel pharetra vel turpis nunc. Malesuada nunc
    vel risus commodo. Nisi vitae suscipit tellus mauris. Posuere morbi leo urna
    molestie at elementum eu. Urna duis convallis convallis tellus. Urna
    molestie at elementum eu. Nunc sed blandit libero volutpat.
  </ScrollFadeEffect>
</div>
```

#### Component Source Code

##### File: `src/registry/components/scroll-fade-effect/scroll-fade-effect.tsx`

```tsx
import type { ComponentProps } from "react"

import { cn } from "@/lib/utils"

export type ScrollFadeEffectProps = ComponentProps<"div"> & {
  /**
   * Scroll direction to apply the fade effect.
   * @defaultValue "vertical"
   * */
  orientation?: "horizontal" | "vertical"
}

export function ScrollFadeEffect({
  className,
  orientation = "vertical",
  ...props
}: ScrollFadeEffectProps) {
  return (
    <div
      data-orientation={orientation}
      className={cn(
        "data-[orientation=horizontal]:overflow-x-auto data-vertical:overflow-y-auto",
        "data-[orientation=horizontal]:scroll-fade-effect-x data-vertical:scroll-fade-effect-y",
        className
      )}
      {...props}
    />
  )
}
```

---

### Shimmering Text <a name="shimmering-text"></a>

Smooth, light-sweeping shimmer animation for text.

- **Registry Key**: `shimmering-text`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/shimmering-text.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

##### Example 1

```tsx
import { ShimmeringText } from "@/components/shimmering-text"
```

##### Example 2

```tsx
<ShimmeringText text="slide to unlock" />
```

#### Component Source Code

##### File: `src/registry/components/shimmering-text/shimmering-text.tsx`

```tsx
"use client"

import * as React from "react"
import type { Variants } from "motion/react"
import { motion } from "motion/react"

import { cn } from "@/lib/utils"

export type ShimmeringTextProps = Omit<
  React.ComponentProps<typeof motion.span>,
  "children"
> & {
  /** The text to render with the shimmering effect. */
  text: string
  /**
   * Duration in seconds for one shimmer cycle.
   * @defaultValue 1 */
  duration?: number
  /**
   * Whether the shimmer animation is paused.
   * @defaultValue false */
  isStopped?: boolean
}

export function ShimmeringText({
  text,
  duration = 1,
  isStopped = false,
  className,
  ...props
}: ShimmeringTextProps) {
  const createCharVariants = React.useCallback(
    (charIndex: number): Variants => ({
      running: {
        color: ["var(--color)", "var(--shimmering-color)", "var(--color)"],
        transition: {
          duration,
          repeat: Infinity,
          repeatType: "loop" as const,
          repeatDelay: text.length * 0.05,
          delay: (charIndex * duration) / text.length,
          ease: "easeInOut",
        },
      },
      stopped: {
        color: "var(--color)",
        transition: {
          duration: duration * 0.5,
          ease: "easeOut",
        },
      },
    }),
    [duration, text.length]
  )

  return (
    <motion.span
      className={cn(
        "inline-block select-none",
        "[--color:var(--muted-foreground)] [--shimmering-color:var(--foreground)]",
        className
      )}
      {...props}
    >
      {text?.split("")?.map((char, i) => (
        <motion.span
          key={i}
          className="inline-block whitespace-pre"
          initial="stopped"
          animate={isStopped ? "stopped" : "running"}
          variants={createCharVariants(i)}
          aria-hidden
        >
          {char}
        </motion.span>
      ))}
      <span className="sr-only">{text}</span>
    </motion.span>
  )
}
```

---

### Slide to Unlock <a name="slide-to-unlock"></a>

Interactive slider inspired by the classic iPhone “slide to unlock” gesture.

- **Registry Key**: `slide-to-unlock`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/slide-to-unlock.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
- **Local Registry Dependencies**:
  - `https://chanhdai.com/r/shimmering-text.json`

#### How to Use

##### Example 1

```tsx
import { ShimmeringText } from "@/components/shimmering-text"
import {
  SlideToUnlock,
  SlideToUnlockHandle,
  SlideToUnlockText,
  SlideToUnlockTrack,
} from "@/components/slide-to-unlock"
```

##### Example 2

```tsx
<SlideToUnlock>
  <SlideToUnlockTrack>
    <SlideToUnlockText>
      <ShimmeringText />
    </SlideToUnlockText>
    <SlideToUnlockHandle />
  </SlideToUnlockTrack>
</SlideToUnlock>
```

#### Component Source Code

##### File: `src/registry/components/slide-to-unlock/slide-to-unlock.tsx`

```tsx
"use client"

import type { ComponentProps, ComponentPropsWithoutRef, JSX } from "react"
import { createContext, useCallback, useContext, useRef, useState } from "react"
import {
  animate,
  motion,
  useMotionValue,
  useTransform,
  type MotionValue,
} from "motion/react"

import { cn } from "@/lib/utils"

type SlideToUnlockContextValue = {
  x: MotionValue<number>
  trackRef: React.RefObject<HTMLDivElement | null>
  isDragging: boolean
  handleWidth: number
  textOpacity: MotionValue<number>
  onDragStart: () => void
  onDragEnd: () => void
}

const SlideToUnlockContext = createContext<SlideToUnlockContextValue | null>(
  null
)

function useSlideToUnlock() {
  const context = useContext(SlideToUnlockContext)
  if (!context) {
    throw new Error(
      `SlideToUnlock components must be used within SlideToUnlock`
    )
  }
  return context
}

export type SlideToUnlockRootProps = ComponentProps<"div"> & {
  /**
   * Width of the drag handle in pixels.
   * @defaultValue 56
   * */
  handleWidth?: number
  /** Called when the handle is dragged fully to the end. */
  onUnlock?: () => void
}

export function SlideToUnlock({
  className,
  handleWidth = 56,
  children,
  onUnlock,
  ...props
}: SlideToUnlockRootProps) {
  const trackRef = useRef<HTMLDivElement>(null)
  const [isDragging, setIsDragging] = useState(false)
  const x = useMotionValue(0)

  const fadeDistance = handleWidth
  const textOpacity = useTransform(x, [0, fadeDistance], [1, 0])

  const handleDragStart = useCallback(() => {
    setIsDragging(true)
  }, [])

  const handleDragEnd = useCallback(() => {
    setIsDragging(false)

    const trackWidth = trackRef.current?.offsetWidth || 0
    const maxX = trackWidth - handleWidth

    if (x.get() >= maxX) {
      onUnlock?.()
    } else {
      animate(x, 0, { type: "spring", bounce: 0, duration: 0.25 })
    }
  }, [x, onUnlock, handleWidth])

  return (
    <SlideToUnlockContext.Provider
      value={{
        x,
        trackRef,
        isDragging,
        handleWidth,
        textOpacity,
        onDragStart: handleDragStart,
        onDragEnd: handleDragEnd,
      }}
    >
      <div
        data-slot="slide-to-unlock"
        className={cn(
          "w-54 rounded-xl bg-muted p-1 shadow-inner inset-ring-1 inset-ring-foreground/10",
          className
        )}
        {...props}
      >
        {children}
      </div>
    </SlideToUnlockContext.Provider>
  )
}

export type SlideToUnlockTrackProps = ComponentProps<"div">

export function SlideToUnlockTrack({
  className,
  children,
  ...props
}: SlideToUnlockTrackProps) {
  const { trackRef } = useSlideToUnlock()

  return (
    <div
      ref={trackRef}
      data-slot="track"
      className={cn(
        "relative flex h-10 items-center justify-center",
        className
      )}
      {...props}
    >
      {children}
    </div>
  )
}

export type SlideToUnlockTextProps = Omit<
  ComponentPropsWithoutRef<typeof motion.div>,
  "children"
> & {
  /**
   * Accepts a render function as `children` to react to the dragging state.
   *
   * @example
   * ```tsx
   * <SlideToUnlockText>
   *   {({ isDragging }) => <span>{isDragging ? "Release..." : "Slide to unlock"}</span>}
   * </SlideToUnlockText>
   * ```
   */
  children: JSX.Element | ((props: { isDragging: boolean }) => JSX.Element)
}

export function SlideToUnlockText({
  className,
  children,
  style,
  ...props
}: SlideToUnlockTextProps) {
  const { handleWidth, textOpacity, isDragging } = useSlideToUnlock()

  return (
    <motion.div
      data-slot="text"
      data-dragging={isDragging}
      className={cn("pl-1 text-lg font-medium", className)}
      style={{ marginLeft: handleWidth, opacity: textOpacity, ...style }}
      {...props}
    >
      {typeof children === "function" ? children({ isDragging }) : children}
    </motion.div>
  )
}

export type SlideToUnlockHandleProps = ComponentPropsWithoutRef<
  typeof motion.div
>

export function SlideToUnlockHandle({
  className,
  children,
  style,
  ...props
}: SlideToUnlockHandleProps) {
  const {
    x,
    trackRef,
    onDragStart,
    onDragEnd,
    handleWidth: width,
  } = useSlideToUnlock()

  return (
    <motion.div
      data-slot="handle"
      className={cn(
        "absolute top-0 left-0 flex h-10 cursor-grab items-center justify-center rounded-lg bg-white text-zinc-400 shadow-sm active:cursor-grabbing",
        "[&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-6",
        className
      )}
      style={{ width, x, ...style }}
      drag="x"
      dragConstraints={trackRef}
      dragElastic={0}
      dragMomentum={false}
      onDragStart={onDragStart}
      onDragEnd={onDragEnd}
      {...props}
    >
      {children ?? (
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" aria-hidden>
          <path
            d="M24 12 12.75 3v4.696H0v8.608h12.75V21z"
            fill="currentColor"
          />
        </svg>
      )}
    </motion.div>
  )
}
```

---

### Spinning Circular Text <a name="spinning-circular-text"></a>

Text arranged in a circle with a continuous spinning animation.

- **Registry Key**: `spinning-circular-text`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/spinning-circular-text.json
```

#### How to Use

##### Example 1

```tsx
import { SpinningCircularText } from "@/components/spinning-circular-text"
```

##### Example 2

```tsx
<SpinningCircularText text="Built with care by ncdai • " />
```

#### Component Source Code

##### File: `src/registry/components/spinning-circular-text/spinning-circular-text.tsx`

```tsx
import { cn } from "@/lib/utils"

export type SpinningCircularTextProps = Omit<
  React.ComponentProps<"div">,
  "children"
> & {
  text: string

  /**
   * @defaultValue 1
   * */
  charSpacing?: number

  /**
   * @defaultValue 1rem
   * */
  fontSize?: string
}

export function SpinningCircularText({
  text,
  charSpacing = 1,
  fontSize = "1rem",
  className,
  style,
  ...props
}: SpinningCircularTextProps) {
  return (
    <div
      className={cn(
        "grid size-(--sc-container-size) place-items-center font-mono font-medium uppercase select-none",
        className
      )}
      style={
        {
          "--sc-size": fontSize,
          "--sc-char-count": text.length,
          "--sc-char-spacing": charSpacing,
          "--sc-inner-angle": "calc((360 / var(--sc-char-count)) * 1deg)",
          "--sc-radius-factor":
            "calc(var(--sc-char-spacing) / sin(var(--sc-inner-angle)))",
          "--sc-radius": "calc(var(--sc-radius-factor) * -1ch)",
          "--sc-container-size":
            "calc(var(--sc-radius-factor) * var(--sc-size) * 2)",
          ...style,
        } as React.CSSProperties
      }
      {...props}
    >
      <div
        className={cn(
          "relative animate-spin-ccw text-(size:--sc-size) leading-none",
          "*:absolute *:top-1/2 *:left-1/2 *:inline-block",
          "*:[--sc-char-rotate:calc(var(--sc-inner-angle)*var(--sc-char-index))]",
          "*:transform-[translate(-50%,-50%)_rotate(var(--sc-char-rotate))_translateY(var(--sc-radius))]"
        )}
        aria-hidden
      >
        {text.split("").map((char, index) => (
          <span
            key={index}
            style={{ "--sc-char-index": index } as React.CSSProperties}
          >
            {char}
          </span>
        ))}
      </div>
      <span className="sr-only">{text}</span>
    </div>
  )
}
```

---

### Testimonial Spotlight <a name="testimonial-spotlight"></a>

Testimonial card with spotlight effect on hover.

- **Registry Key**: `testimonial-spotlight`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/testimonial-spotlight.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `https://chanhdai.com/r/testimonial.json`

#### How to Use

##### Example 1

```tsx
import {
  Testimonial,
  TestimonialAuthor,
  TestimonialAuthorInfo,
  TestimonialAuthorName,
  TestimonialAuthorTagline,
  TestimonialAvatar,
  TestimonialAvatarImg,
  TestimonialAvatarRing,
  TestimonialQuote,
  TestimonialVerifiedBadge,
} from "@/components/testimonial"
import { TestimonialSpotlight } from "@/components/testimonial-spotlight"
```

##### Example 2

```tsx
<TestimonialSpotlight>
  <Testimonial>
    <TestimonialQuote />

    <TestimonialAuthor>
      <TestimonialAvatar>
        <TestimonialAvatarImg />
        <TestimonialAvatarRing />
      </TestimonialAvatar>

      <TestimonialAuthorName>
        <TestimonialVerifiedBadge />
      </TestimonialAuthorName>

      <TestimonialAuthorTagline />
    </TestimonialAuthor>
  </Testimonial>
</TestimonialSpotlight>
```

##### Example 3

```tsx
<TestimonialSpotlight
  className={cn(
    "[--spotlight-color:rgba(219,39,119,0.15)] dark:[--spotlight-color:rgba(255,255,255,0.2)]",
    "[--spotlight-opacity:0.8] [--spotlight-size:50%]"
  )}
>
  ...
</TestimonialSpotlight>
```

##### Example 4

```tsx
<TestimonialSpotlight
  style={
    {
      "--spotlight-color": "rgba(219,39,119,0.15)",
      "--spotlight-size": "50%",
      "--spotlight-opacity": "0.8",
    } as React.CSSProperties
  }
>
  ...
</TestimonialSpotlight>
```

#### Component Source Code

##### File: `src/registry/components/testimonial-spotlight/testimonial-spotlight.tsx`

```tsx
"use client"

import { useRef } from "react"

import { cn } from "@/lib/utils"

export type TestimonialSpotlightProps = Omit<
  React.ComponentPropsWithoutRef<"div">,
  "children" | "onMouseMove"
> & {
  children: React.ReactNode
}

export function TestimonialSpotlight({
  children,
  className,
  ...props
}: TestimonialSpotlightProps) {
  const itemRef = useRef<HTMLDivElement>(null)

  const handleMouseMove: React.MouseEventHandler<HTMLDivElement> = (e) => {
    if (!itemRef.current) return

    const rect = itemRef.current.getBoundingClientRect()

    itemRef.current.style.setProperty(
      "--spotlight-x",
      `${e.clientX - rect.left}px`
    )
    itemRef.current.style.setProperty(
      "--spotlight-y",
      `${e.clientY - rect.top}px`
    )
  }

  return (
    <div
      ref={itemRef}
      data-slot="testimonial-spotlight"
      className={cn(
        "group/testimonial-spotlight relative overflow-hidden rounded-xl bg-card/50 inset-ring-1 inset-ring-foreground/10",
        className
      )}
      onMouseMove={handleMouseMove}
      {...props}
    >
      <div
        className="pointer-events-none absolute inset-0 opacity-0 transition-opacity duration-500 ease-in-out group-hover/testimonial-spotlight:opacity-(--spotlight-opacity,0.5)"
        style={{
          background: `radial-gradient(circle at var(--spotlight-x) var(--spotlight-y), var(--spotlight-color,rgba(255,255,255,0.2)), transparent var(--spotlight-size,60%))`,
        }}
      />
      {children}
    </div>
  )
}
```

---

### Testimonial <a name="testimonial"></a>

Display user feedback with author info, avatar, and verified badge.

- **Registry Key**: `testimonial`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/testimonial.json
```

#### How to Use

##### Example 1

```tsx
import {
  Testimonial,
  TestimonialAuthor,
  TestimonialAuthorInfo,
  TestimonialAuthorName,
  TestimonialAuthorTagline,
  TestimonialAvatar,
  TestimonialAvatarImg,
  TestimonialAvatarRing,
  TestimonialQuote,
  TestimonialVerifiedBadge,
} from "@/components/testimonial"
```

##### Example 2

```tsx
<Testimonial>
  <TestimonialQuote />

  <TestimonialAuthor>
    <TestimonialAvatar>
      <TestimonialAvatarImg />
      <TestimonialAvatarRing />
    </TestimonialAvatar>

    <TestimonialAuthorName>
      <TestimonialVerifiedBadge />
    </TestimonialAuthorName>

    <TestimonialAuthorTagline />
  </TestimonialAuthor>
</Testimonial>
```

#### Component Source Code

##### File: `src/registry/components/testimonial/testimonial.tsx`

```tsx
import type { ComponentProps } from "react"

import { cn } from "@/lib/utils"

export function Testimonial({ className, ...props }: ComponentProps<"figure">) {
  return (
    <figure
      data-slot="testimonial"
      className={cn("flex h-full flex-col", className)}
      {...props}
    />
  )
}

export function TestimonialQuote({
  className,
  ...props
}: ComponentProps<"blockquote">) {
  return (
    <blockquote
      data-slot="testimonial-quote"
      className={cn(
        "grow px-4 py-3 text-base text-pretty text-foreground",
        className
      )}
      {...props}
    />
  )
}

export function TestimonialAuthor({
  className,
  ...props
}: ComponentProps<"figcaption">) {
  return (
    <figcaption
      data-slot="testimonial-author"
      className={cn(
        "grid grid-cols-[auto_1fr] grid-rows-2 items-center gap-x-3.5 px-4 pt-1 pb-3",
        className
      )}
      {...props}
    />
  )
}

export function TestimonialAvatar({
  className,
  ...props
}: ComponentProps<"div">) {
  return (
    <div
      data-slot="testimonial-avatar"
      className={cn("relative row-span-2 size-8 shrink-0", className)}
      {...props}
    />
  )
}

export function TestimonialAvatarImg({
  className,
  src,
  alt,
  ...props
}: ComponentProps<"img">) {
  return (
    <img
      data-slot="testimonial-avatar-img"
      className={cn("size-8 rounded-full select-none", className)}
      src={src}
      alt={alt}
      {...props}
    />
  )
}

export function TestimonialAvatarRing({
  className,
  ...props
}: ComponentProps<"div">) {
  return (
    <div
      data-slot="testimonial-avatar-ring"
      className={cn(
        "pointer-events-none absolute inset-0 rounded-full inset-ring-1 inset-ring-black/10 dark:inset-ring-white/15",
        className
      )}
      {...props}
    />
  )
}

export function TestimonialAuthorName({
  className,
  ...props
}: ComponentProps<"div">) {
  return (
    <div
      data-slot="testimonial-author-name"
      className={cn(
        "flex items-center gap-1.5 text-sm leading-4.5 font-semibold text-foreground",
        className
      )}
      {...props}
    />
  )
}

export function TestimonialAuthorTagline({
  className,
  ...props
}: ComponentProps<"div">) {
  return (
    <div
      data-slot="testimonial-author-tagline"
      className={cn(
        "text-xs leading-4 text-balance text-muted-foreground",
        className
      )}
      {...props}
    />
  )
}

export function TestimonialVerifiedBadge({
  className,
  ...props
}: ComponentProps<"span">) {
  return (
    <span
      data-slot="testimonial-verified-badge"
      className={cn(
        "flex [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-3.5",
        className
      )}
      aria-hidden
      {...props}
    />
  )
}
```

---

### Testimonials Marquee <a name="testimonials-marquee"></a>

Scrolling marquee to showcase user testimonials.

- **Registry Key**: `testimonials-marquee`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/testimonials-marquee.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `@kibo-ui/marquee`
  - `https://chanhdai.com/r/testimonial.json`

#### How to Use

##### Example 1

```tsx
import {
  Marquee,
  MarqueeContent,
  MarqueeFade,
  MarqueeItem,
} from "@/components/kibo-ui/marquee"
import {
  Testimonial,
  TestimonialAuthor,
  TestimonialAuthorInfo,
  TestimonialAuthorName,
  TestimonialAuthorTagline,
  TestimonialAvatar,
  TestimonialAvatarImg,
  TestimonialAvatarRing,
  TestimonialQuote,
  TestimonialVerifiedBadge,
} from "@/components/testimonial"
```

##### Example 2

```tsx
<Marquee>
  <MarqueeFade />

  <MarqueeContent>
    <MarqueeItem>
      <Testimonial>
        <TestimonialQuote />

        <TestimonialAuthor>
          <TestimonialAvatar>
            <TestimonialAvatarImg />
            <TestimonialAvatarRing />
          </TestimonialAvatar>

          <TestimonialAuthorName>
            <TestimonialVerifiedBadge />
          </TestimonialAuthorName>

          <TestimonialAuthorTagline />
        </TestimonialAuthor>
      </Testimonial>
    </MarqueeItem>
  </MarqueeContent>
</Marquee>
```

---

### Text Flip <a name="text-flip"></a>

Animated text that cycles through items with a smooth flip transition.

- **Registry Key**: `text-flip`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/text-flip.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

##### Example 1

```tsx
import { TextFlip } from "@/components/text-flip"
```

##### Example 2

```tsx
<TextFlip>
  <span>Developer</span>
  <span>Designer</span>
  <span>Creator</span>
</TextFlip>
```

#### Component Source Code

##### File: `src/registry/components/text-flip/text-flip.tsx`

```tsx
"use client"

import { Children, useEffect, useState } from "react"
import type { Transition, Variants } from "motion/react"
import { AnimatePresence, motion } from "motion/react"

import { cn } from "@/lib/utils"

const defaultVariants: Variants = {
  initial: { y: -8, opacity: 0 },
  animate: { y: 0, opacity: 1 },
  exit: { y: 8, opacity: 0 },
}

type MotionElement = typeof motion.p | typeof motion.span | typeof motion.code

export type TextFlipProps = {
  /**
   * Motion element to render.
   * @defaultValue motion.p
   * */
  as?: MotionElement
  className?: string
  /** Array of children to cycle through. */
  children: React.ReactNode[]

  /**
   * Time in seconds between each flip.
   * @defaultValue 2
   * */
  interval?: number
  /**
   * Motion transition configuration.
   * @defaultValue { duration: 0.3 }
   * */
  transition?: Transition
  /** Motion variants for enter/exit animations. */
  variants?: Variants

  /** Controls whether the flip animation runs. */
  play?: boolean

  /** Called with the new index after each flip. */
  onIndexChange?: (index: number) => void
}

export function TextFlip({
  as: Component = motion.p,
  className,
  children,

  interval = 2,
  transition = { duration: 0.3 },
  variants = defaultVariants,
  play = true,

  onIndexChange,
}: TextFlipProps) {
  const [currentIndex, setCurrentIndex] = useState(0)

  const items = Children.toArray(children)

  useEffect(() => {
    if (!play) return

    const timer = setInterval(() => {
      setCurrentIndex((prev) => {
        const next = (prev + 1) % items.length
        onIndexChange?.(next)
        return next
      })
    }, interval * 1000)

    return () => clearInterval(timer)
  }, [play, interval, items.length, onIndexChange])

  return (
    <AnimatePresence mode="wait" initial={false}>
      <Component
        key={currentIndex}
        className={cn("inline-block", className)}
        initial="initial"
        animate="animate"
        exit="exit"
        transition={transition}
        variants={variants}
      >
        {items[currentIndex]}
      </Component>
    </AnimatePresence>
  )
}
```

---

### Theme Switcher <a name="theme-switcher"></a>

Toggle between system, light, and dark themes in Next.js apps.

- **Registry Key**: `theme-switcher`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/theme-switcher.json
```

#### Dependencies

- **NPM Packages**:
  - `next-themes`
  - `motion`

#### How to Use

##### Example 1

```tsx
import { ThemeSwitcher } from "@/components/theme-switcher"
```

##### Example 2

```tsx
<ThemeSwitcher />
```

#### Component Source Code

##### File: `src/registry/components/theme-switcher/theme-switcher.tsx`

```tsx
"use client"

import type { JSX } from "react"
import { useSyncExternalStore } from "react"
import { motion } from "motion/react"
import { useTheme } from "next-themes"

import { IconPlaceholder } from "@/registry/icons/icon-placeholder"

function ThemeOption({
  icon,
  value,
  isActive,
  onClick,
}: {
  icon: JSX.Element
  value: string
  isActive?: boolean
  onClick: (value: string) => void
}) {
  return (
    <button
      data-active={isActive}
      className="relative flex size-8 items-center justify-center rounded-full text-muted-foreground transition-[color] hover:text-foreground data-[active=true]:text-foreground [&_svg]:size-4"
      role="radio"
      aria-checked={isActive}
      aria-label={`Switch to ${value} theme`}
      onClick={() => onClick(value)}
    >
      {icon}

      {isActive && (
        <motion.span
          layoutId="theme-option"
          transition={{ type: "spring", bounce: 0.3, duration: 0.6 }}
          className="absolute inset-0 rounded-full border"
        />
      )}
    </button>
  )
}

const THEME_OPTIONS = [
  {
    icon: (
      <IconPlaceholder
        lucide="MonitorIcon"
        tabler="IconDeviceDesktop"
        hugeicons="ComputerIcon"
        phosphor="DesktopIcon"
        remixicon="RiComputerLine"
      />
    ),
    value: "system",
  },
  {
    icon: (
      <IconPlaceholder
        lucide="SunIcon"
        tabler="IconSun"
        hugeicons="Sun03Icon"
        phosphor="SunIcon"
        remixicon="RiSunLine"
      />
    ),
    value: "light",
  },
  {
    icon: (
      <IconPlaceholder
        lucide="MoonIcon"
        tabler="IconMoon"
        hugeicons="Moon02Icon"
        phosphor="MoonIcon"
        remixicon="RiMoonLine"
      />
    ),
    value: "dark",
  },
]

function ThemeSwitcher() {
  const { theme, setTheme } = useTheme()

  const isMounted = useSyncExternalStore(
    () => () => {},
    () => true,
    () => false
  )

  if (!isMounted) {
    return <div className="flex h-8 w-24" />
  }

  return (
    <motion.div
      key={String(isMounted)}
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      transition={{ duration: 0.3 }}
      className="inline-flex items-center overflow-clip rounded-full bg-background inset-ring-1 inset-ring-border"
      role="radiogroup"
    >
      {THEME_OPTIONS.map((option) => (
        <ThemeOption
          key={option.value}
          icon={option.icon}
          value={option.value}
          isActive={theme === option.value}
          onClick={setTheme}
        />
      ))}
    </motion.div>
  )
}

export { ThemeSwitcher }
```

---

### Theme Toggle Effect <a name="theme-toggle-effect"></a>

Animated transitions when switching between light and dark themes.

- **Registry Key**: `theme-toggle-effect`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/theme-toggle-effect.json
```

#### How to Use

```tsx
const { setTheme } = useTheme()

function toggleTheme(theme: string) {
  if (!document.startViewTransition) {
    setTheme(theme)
    return
  }

  document.startViewTransition(() => setTheme(theme))
}
```

---

### Tips for Creating Beautiful Image Borders <a name="tips-for-creating-beautiful-image-borders"></a>

Borders help images stand out, but fixed colors often clash with the content. Here’s a simple tip to make borders look more natural and balanced.

- **Registry Key**: `tips-for-creating-beautiful-image-borders`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/tips-for-creating-beautiful-image-borders.json
```

#### How to Use

##### Example 1

```tsx
<img
  src="https://images.unsplash.com/photo-1503023345310-bd7c1de61c7d"
  className="rounded-xl border border-black/10 dark:border-white/10"
/>
```

##### Example 2

```tsx
<div className="relative">
  <img
    src="https://images.unsplash.com/photo-1503023345310-bd7c1de61c7d"
    className="rounded-xl"
  />
  <div className="pointer-events-none absolute inset-0 rounded-xl inset-ring-1 inset-ring-black/10 dark:inset-ring-white/10" />
</div>
```

---

### TOC Minimap <a name="toc-minimap"></a>

Navigate page sections with a compact, hoverable TOC minimap.

- **Registry Key**: `toc-minimap`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/toc-minimap.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `hover-card`
  - `@soundcn/u-mini-map-open`

#### How to Use

##### Example 1

```tsx
import { TOCMinimap } from "@/components/toc-minimap"
```

##### Example 2

```tsx
<TOCMinimap items={toc} />
```

#### Component Source Code

##### File: `src/registry/components/toc-minimap/toc-minimap.tsx`

```tsx
"use client"

import { useEffect, useMemo, useState } from "react"

import { uMiniMapOpenSound } from "@/lib/soundcn/u-mini-map-open"
import { cn } from "@/lib/utils"
import { useSound } from "@/hooks/soundcn/use-sound"
import {
  HoverCard,
  HoverCardContent,
  HoverCardTrigger,
} from "@/components/ui/hover-card"

export type TOCItemType = {
  title: React.ReactNode
  url: string
  depth: number
}

export type TOCMinimapProps = {
  /** @fumadocsHref #tocitemtype */
  items: TOCItemType[]
  className?: string
}

export function TOCMinimap({ items, className }: TOCMinimapProps) {
  const itemIds = useMemo(
    () => items.map((item) => item.url.replace("#", "")),
    [items]
  )

  const activeHeading = useActiveHeading(itemIds)

  const [play] = useSound(uMiniMapOpenSound, { volume: 0.3 })

  if (!items.length) {
    return null
  }

  return (
    <div className={cn("ml-auto w-18", className)}>
      <HoverCard
        openDelay={0}
        closeDelay={0}
        onOpenChange={(open) => {
          if (open) play()
        }}
      >
        <HoverCardTrigger asChild>
          <div className="flex max-h-[50dvh] flex-col gap-3 overflow-hidden py-3 pl-6 opacity-100 transition-opacity duration-200 data-popup-open:opacity-0">
            {items.map((item) => (
              <div
                key={item.url}
                data-depth={item.depth}
                data-active={item.url === `#${activeHeading}`}
                className={cn(
                  "h-0.5 w-6 shrink-0 rounded-xs bg-ring/50 transition-[background-color] duration-200",
                  "data-[depth=3]:ml-2 data-[depth=3]:w-4",
                  "data-[depth=4]:ml-4 data-[depth=4]:w-2",
                  "data-active:bg-foreground"
                )}
              />
            ))}
          </div>
        </HoverCardTrigger>

        <HoverCardContent
          className="w-56 overflow-hidden p-0 duration-200 data-[side=left]:slide-in-from-right-3 data-[side=left]:slide-out-to-right-3 data-open:zoom-in-100 data-closed:zoom-out-100"
          align="start"
          alignOffset={0}
          side="left"
          sideOffset={-60}
        >
          <div className="flex max-h-[50dvh] overflow-y-auto overscroll-contain">
            <ul className="flex size-full flex-col px-6 py-4 text-sm">
              {items.map((item) => (
                <li key={item.url} className="flex py-1">
                  <a
                    href={item.url}
                    data-depth={item.depth}
                    data-active={item.url === `#${activeHeading}`}
                    className={cn(
                      "line-clamp-2 w-full transition-[color] duration-200",
                      "text-muted-foreground hover:text-foreground data-active:text-foreground",
                      "data-[depth=3]:pl-4 data-[depth=4]:pl-8"
                    )}
                    onClick={handleItemClick}
                  >
                    {item.title}
                  </a>
                </li>
              ))}
            </ul>
          </div>
        </HoverCardContent>
      </HoverCard>
    </div>
  )
}

export function useActiveHeading(itemIds: string[]) {
  const [activeId, setActiveId] = useState<string | null>(null)

  useEffect(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        for (const entry of entries) {
          if (entry.isIntersecting) {
            setActiveId(entry.target.id)
          }
        }
      },
      { rootMargin: "0% 0% -80% 0%", threshold: 0.98 }
    )

    for (const id of itemIds ?? []) {
      const element = document.getElementById(id)
      if (element) {
        observer.observe(element)
      }
    }

    return () => {
      for (const id of itemIds ?? []) {
        const element = document.getElementById(id)
        if (element) {
          observer.unobserve(element)
        }
      }
    }
  }, [itemIds])

  return activeId
}

function handleItemClick(e: React.MouseEvent<HTMLAnchorElement>) {
  e.preventDefault()
  const url = e.currentTarget.getAttribute("href") ?? ""
  scrollToHeading(url)
}

function scrollToHeading(url: string) {
  history.pushState(null, "", url)
  document.getElementById(url.replace("#", ""))?.scrollIntoView({
    behavior: "smooth",
  })
}
```

---

### Twemoji <a name="twemoji"></a>

Render Unicode emoji as Twemoji SVG images inline with text.

- **Registry Key**: `twemoji`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/twemoji.json
```

#### How to Use

##### Example 1

```tsx
import { Twemoji } from "@/components/twemoji"
```

##### Example 2

```tsx
<Twemoji>Hello from Viet Nam 🇻🇳</Twemoji>
```

##### Example 3

```tsx
<Twemoji
  source={(codePoint) =>
    `https://cdn.jsdelivr.net/gh/twitter/twemoji@latest/assets/svg/${codePoint}.svg`
  }
>
  Hello from Viet Nam 🇻🇳
</Twemoji>
```

#### Component Source Code

##### File: `src/registry/components/twemoji/twemoji.tsx`

```tsx
import type { ReactNode } from "react"

import { cn } from "@/lib/utils"
import emojiRegex from "@/registry/components/twemoji/lib/twemoji-regex"

const VARIATION_SELECTOR_16 = /\uFE0F/g
const ZERO_WIDTH_JOINER = String.fromCharCode(0x200d)

export const TWEMOJI_CDN_URL = "https://abs-0.twimg.com/emoji/v2/svg"

export type TwemojiProps = {
  children: string
  className?: string
  /**
   * Resolve an emoji code point to an image URL.
   *
   * @param codePoint - The Unicode code point of the emoji (e.g. `1f44d`)
   * @returns The URL string pointing to the emoji image asset
   *
   * @example
   * ```tsx
   * source={(codePoint) => `https://cdn.example.com/emoji/${codePoint}.svg`}
   * ```
   * */
  source?: (codePoint: string) => string
}

export function Twemoji({
  children,
  className,
  source = defaultSource,
}: TwemojiProps) {
  const parts: ReactNode[] = []
  let lastIndex = 0
  const globalRegex = new RegExp(emojiRegex.source, "g")
  let match

  while ((match = globalRegex.exec(children)) !== null) {
    if (match.index > lastIndex) {
      parts.push(children.substring(lastIndex, match.index))
    }

    const rawText = match[0]
    const codePoint = getEmojiCodePoint(rawText)

    if (codePoint) {
      parts.push(
        <img
          key={match.index}
          className={cn("twemoji", className)}
          draggable={false}
          alt={rawText}
          src={source(codePoint)}
        />
      )
    } else {
      parts.push(rawText)
    }

    lastIndex = globalRegex.lastIndex
  }

  if (lastIndex < children.length) {
    parts.push(children.substring(lastIndex))
  }

  return <>{parts}</>
}

function getEmojiCodePoint(rawText: string) {
  return toCodePoint(
    rawText.indexOf(ZERO_WIDTH_JOINER) < 0
      ? rawText.replace(VARIATION_SELECTOR_16, "")
      : rawText
  )
}

function toCodePoint(unicodeSurrogates: string, sep = "-") {
  const result: string[] = []
  let code = 0
  let previous = 0
  let i = 0

  while (i < unicodeSurrogates.length) {
    code = unicodeSurrogates.charCodeAt(i++)
    if (previous) {
      result.push(
        (0x10000 + ((previous - 0xd800) << 10) + (code - 0xdc00)).toString(16)
      )
      previous = 0
    } else if (0xd800 <= code && code <= 0xdbff) {
      previous = code
    } else {
      result.push(code.toString(16))
    }
  }

  return result.join(sep)
}

function defaultSource(codePoint: string) {
  return `${TWEMOJI_CDN_URL}/${codePoint}.svg`
}
```

##### File: `src/registry/components/twemoji/lib/twemoji-regex.ts`

```tsx
// Copyright Twitter Inc. Licensed under MIT
// https://github.com/twitter/twemoji-parser/blob/master/LICENSE.md

// This file is generated by source/emoji/scripts/generate.sh

// RegExp based on emoji's official Unicode standards
// http://www.unicode.org/Public/UNIDATA/EmojiSources.txt
const regex =
  /(?:\ud83d\udc68\ud83c\udffb\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc68\ud83c\udffc\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc68\ud83c\udffd\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc68\ud83c\udffe\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc68\ud83c\udfff\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffb\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffb\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc69\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffc\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffc\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc69\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffd\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffd\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc69\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffe\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffe\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc69\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udfff\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udfff\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc69\ud83c[\udffb-\udfff]|\ud83e\uddd1\ud83c\udffb\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83e\uddd1\ud83c[\udffc-\udfff]|\ud83e\uddd1\ud83c\udffc\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83e\uddd1\ud83c[\udffb\udffd-\udfff]|\ud83e\uddd1\ud83c\udffd\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83e\uddd1\ud83c[\udffb\udffc\udffe\udfff]|\ud83e\uddd1\ud83c\udffe\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83e\uddd1\ud83c[\udffb-\udffd\udfff]|\ud83e\uddd1\ud83c\udfff\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83e\uddd1\ud83c[\udffb-\udffe]|\ud83d\udc68\ud83c\udffb\u200d\u2764\ufe0f\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc68\ud83c\udffb\u200d\ud83e\udd1d\u200d\ud83d\udc68\ud83c[\udffc-\udfff]|\ud83d\udc68\ud83c\udffc\u200d\u2764\ufe0f\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc68\ud83c\udffc\u200d\ud83e\udd1d\u200d\ud83d\udc68\ud83c[\udffb\udffd-\udfff]|\ud83d\udc68\ud83c\udffd\u200d\u2764\ufe0f\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc68\ud83c\udffd\u200d\ud83e\udd1d\u200d\ud83d\udc68\ud83c[\udffb\udffc\udffe\udfff]|\ud83d\udc68\ud83c\udffe\u200d\u2764\ufe0f\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc68\ud83c\udffe\u200d\ud83e\udd1d\u200d\ud83d\udc68\ud83c[\udffb-\udffd\udfff]|\ud83d\udc68\ud83c\udfff\u200d\u2764\ufe0f\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc68\ud83c\udfff\u200d\ud83e\udd1d\u200d\ud83d\udc68\ud83c[\udffb-\udffe]|\ud83d\udc69\ud83c\udffb\u200d\u2764\ufe0f\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffb\u200d\u2764\ufe0f\u200d\ud83d\udc69\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffb\u200d\ud83e\udd1d\u200d\ud83d\udc68\ud83c[\udffc-\udfff]|\ud83d\udc69\ud83c\udffb\u200d\ud83e\udd1d\u200d\ud83d\udc69\ud83c[\udffc-\udfff]|\ud83d\udc69\ud83c\udffc\u200d\u2764\ufe0f\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffc\u200d\u2764\ufe0f\u200d\ud83d\udc69\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffc\u200d\ud83e\udd1d\u200d\ud83d\udc68\ud83c[\udffb\udffd-\udfff]|\ud83d\udc69\ud83c\udffc\u200d\ud83e\udd1d\u200d\ud83d\udc69\ud83c[\udffb\udffd-\udfff]|\ud83d\udc69\ud83c\udffd\u200d\u2764\ufe0f\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffd\u200d\u2764\ufe0f\u200d\ud83d\udc69\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffd\u200d\ud83e\udd1d\u200d\ud83d\udc68\ud83c[\udffb\udffc\udffe\udfff]|\ud83d\udc69\ud83c\udffd\u200d\ud83e\udd1d\u200d\ud83d\udc69\ud83c[\udffb\udffc\udffe\udfff]|\ud83d\udc69\ud83c\udffe\u200d\u2764\ufe0f\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffe\u200d\u2764\ufe0f\u200d\ud83d\udc69\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udffe\u200d\ud83e\udd1d\u200d\ud83d\udc68\ud83c[\udffb-\udffd\udfff]|\ud83d\udc69\ud83c\udffe\u200d\ud83e\udd1d\u200d\ud83d\udc69\ud83c[\udffb-\udffd\udfff]|\ud83d\udc69\ud83c\udfff\u200d\u2764\ufe0f\u200d\ud83d\udc68\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udfff\u200d\u2764\ufe0f\u200d\ud83d\udc69\ud83c[\udffb-\udfff]|\ud83d\udc69\ud83c\udfff\u200d\ud83e\udd1d\u200d\ud83d\udc68\ud83c[\udffb-\udffe]|\ud83d\udc69\ud83c\udfff\u200d\ud83e\udd1d\u200d\ud83d\udc69\ud83c[\udffb-\udffe]|\ud83e\uddd1\ud83c\udffb\u200d\u2764\ufe0f\u200d\ud83e\uddd1\ud83c[\udffc-\udfff]|\ud83e\uddd1\ud83c\udffb\u200d\ud83e\udd1d\u200d\ud83e\uddd1\ud83c[\udffb-\udfff]|\ud83e\uddd1\ud83c\udffc\u200d\u2764\ufe0f\u200d\ud83e\uddd1\ud83c[\udffb\udffd-\udfff]|\ud83e\uddd1\ud83c\udffc\u200d\ud83e\udd1d\u200d\ud83e\uddd1\ud83c[\udffb-\udfff]|\ud83e\uddd1\ud83c\udffd\u200d\u2764\ufe0f\u200d\ud83e\uddd1\ud83c[\udffb\udffc\udffe\udfff]|\ud83e\uddd1\ud83c\udffd\u200d\ud83e\udd1d\u200d\ud83e\uddd1\ud83c[\udffb-\udfff]|\ud83e\uddd1\ud83c\udffe\u200d\u2764\ufe0f\u200d\ud83e\uddd1\ud83c[\udffb-\udffd\udfff]|\ud83e\uddd1\ud83c\udffe\u200d\ud83e\udd1d\u200d\ud83e\uddd1\ud83c[\udffb-\udfff]|\ud83e\uddd1\ud83c\udfff\u200d\u2764\ufe0f\u200d\ud83e\uddd1\ud83c[\udffb-\udffe]|\ud83e\uddd1\ud83c\udfff\u200d\ud83e\udd1d\u200d\ud83e\uddd1\ud83c[\udffb-\udfff]|\ud83d\udc68\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d\udc68|\ud83d\udc69\u200d\u2764\ufe0f\u200d\ud83d\udc8b\u200d\ud83d[\udc68\udc69]|\ud83e\udef1\ud83c\udffb\u200d\ud83e\udef2\ud83c[\udffc-\udfff]|\ud83e\udef1\ud83c\udffc\u200d\ud83e\udef2\ud83c[\udffb\udffd-\udfff]|\ud83e\udef1\ud83c\udffd\u200d\ud83e\udef2\ud83c[\udffb\udffc\udffe\udfff]|\ud83e\udef1\ud83c\udffe\u200d\ud83e\udef2\ud83c[\udffb-\udffd\udfff]|\ud83e\udef1\ud83c\udfff\u200d\ud83e\udef2\ud83c[\udffb-\udffe]|\ud83d\udc68\u200d\u2764\ufe0f\u200d\ud83d\udc68|\ud83d\udc69\u200d\u2764\ufe0f\u200d\ud83d[\udc68\udc69]|\ud83e\uddd1\u200d\ud83e\udd1d\u200d\ud83e\uddd1|\ud83d\udc6b\ud83c[\udffb-\udfff]|\ud83d\udc6c\ud83c[\udffb-\udfff]|\ud83d\udc6d\ud83c[\udffb-\udfff]|\ud83d\udc8f\ud83c[\udffb-\udfff]|\ud83d\udc91\ud83c[\udffb-\udfff]|\ud83e\udd1d\ud83c[\udffb-\udfff]|\ud83d[\udc6b-\udc6d\udc8f\udc91]|\ud83e\udd1d)|(?:\ud83d[\udc68\udc69]|\ud83e\uddd1)(?:\ud83c[\udffb-\udfff])?\u200d(?:\u2695\ufe0f|\u2696\ufe0f|\u2708\ufe0f|\ud83c[\udf3e\udf73\udf7c\udf84\udf93\udfa4\udfa8\udfeb\udfed]|\ud83d[\udcbb\udcbc\udd27\udd2c\ude80\ude92]|\ud83e[\uddaf-\uddb3\uddbc\uddbd])|(?:\ud83c[\udfcb\udfcc]|\ud83d[\udd74\udd75]|\u26f9)((?:\ud83c[\udffb-\udfff]|\ufe0f)\u200d[\u2640\u2642]\ufe0f)|(?:\ud83c[\udfc3\udfc4\udfca]|\ud83d[\udc6e\udc70\udc71\udc73\udc77\udc81\udc82\udc86\udc87\ude45-\ude47\ude4b\ude4d\ude4e\udea3\udeb4-\udeb6]|\ud83e[\udd26\udd35\udd37-\udd39\udd3d\udd3e\uddb8\uddb9\uddcd-\uddcf\uddd4\uddd6-\udddd])(?:\ud83c[\udffb-\udfff])?\u200d[\u2640\u2642]\ufe0f|(?:\ud83d\udc68\u200d\ud83d\udc68\u200d\ud83d\udc66\u200d\ud83d\udc66|\ud83d\udc68\u200d\ud83d\udc68\u200d\ud83d\udc67\u200d\ud83d[\udc66\udc67]|\ud83d\udc68\u200d\ud83d\udc69\u200d\ud83d\udc66\u200d\ud83d\udc66|\ud83d\udc68\u200d\ud83d\udc69\u200d\ud83d\udc67\u200d\ud83d[\udc66\udc67]|\ud83d\udc69\u200d\ud83d\udc69\u200d\ud83d\udc66\u200d\ud83d\udc66|\ud83d\udc69\u200d\ud83d\udc69\u200d\ud83d\udc67\u200d\ud83d[\udc66\udc67]|\ud83d\udc68\u200d\ud83d\udc66\u200d\ud83d\udc66|\ud83d\udc68\u200d\ud83d\udc67\u200d\ud83d[\udc66\udc67]|\ud83d\udc68\u200d\ud83d\udc68\u200d\ud83d[\udc66\udc67]|\ud83d\udc68\u200d\ud83d\udc69\u200d\ud83d[\udc66\udc67]|\ud83d\udc69\u200d\ud83d\udc66\u200d\ud83d\udc66|\ud83d\udc69\u200d\ud83d\udc67\u200d\ud83d[\udc66\udc67]|\ud83d\udc69\u200d\ud83d\udc69\u200d\ud83d[\udc66\udc67]|\ud83c\udff3\ufe0f\u200d\u26a7\ufe0f|\ud83c\udff3\ufe0f\u200d\ud83c\udf08|\ud83d\ude36\u200d\ud83c\udf2b\ufe0f|\u2764\ufe0f\u200d\ud83d\udd25|\u2764\ufe0f\u200d\ud83e\ude79|\ud83c\udff4\u200d\u2620\ufe0f|\ud83d\udc15\u200d\ud83e\uddba|\ud83d\udc3b\u200d\u2744\ufe0f|\ud83d\udc41\u200d\ud83d\udde8|\ud83d\udc68\u200d\ud83d[\udc66\udc67]|\ud83d\udc69\u200d\ud83d[\udc66\udc67]|\ud83d\udc6f\u200d\u2640\ufe0f|\ud83d\udc6f\u200d\u2642\ufe0f|\ud83d\ude2e\u200d\ud83d\udca8|\ud83d\ude35\u200d\ud83d\udcab|\ud83e\udd3c\u200d\u2640\ufe0f|\ud83e\udd3c\u200d\u2642\ufe0f|\ud83e\uddde\u200d\u2640\ufe0f|\ud83e\uddde\u200d\u2642\ufe0f|\ud83e\udddf\u200d\u2640\ufe0f|\ud83e\udddf\u200d\u2642\ufe0f|\ud83d\udc08\u200d\u2b1b)|[#*0-9]\ufe0f?\u20e3|(?:[©®\u2122\u265f]\ufe0f)|(?:\ud83c[\udc04\udd70\udd71\udd7e\udd7f\ude02\ude1a\ude2f\ude37\udf21\udf24-\udf2c\udf36\udf7d\udf96\udf97\udf99-\udf9b\udf9e\udf9f\udfcd\udfce\udfd4-\udfdf\udff3\udff5\udff7]|\ud83d[\udc3f\udc41\udcfd\udd49\udd4a\udd6f\udd70\udd73\udd76-\udd79\udd87\udd8a-\udd8d\udda5\udda8\uddb1\uddb2\uddbc\uddc2-\uddc4\uddd1-\uddd3\udddc-\uddde\udde1\udde3\udde8\uddef\uddf3\uddfa\udecb\udecd-\udecf\udee0-\udee5\udee9\udef0\udef3]|[\u203c\u2049\u2139\u2194-\u2199\u21a9\u21aa\u231a\u231b\u2328\u23cf\u23ed-\u23ef\u23f1\u23f2\u23f8-\u23fa\u24c2\u25aa\u25ab\u25b6\u25c0\u25fb-\u25fe\u2600-\u2604\u260e\u2611\u2614\u2615\u2618\u2620\u2622\u2623\u2626\u262a\u262e\u262f\u2638-\u263a\u2640\u2642\u2648-\u2653\u2660\u2663\u2665\u2666\u2668\u267b\u267f\u2692-\u2697\u2699\u269b\u269c\u26a0\u26a1\u26a7\u26aa\u26ab\u26b0\u26b1\u26bd\u26be\u26c4\u26c5\u26c8\u26cf\u26d1\u26d3\u26d4\u26e9\u26ea\u26f0-\u26f5\u26f8\u26fa\u26fd\u2702\u2708\u2709\u270f\u2712\u2714\u2716\u271d\u2721\u2733\u2734\u2744\u2747\u2757\u2763\u2764\u27a1\u2934\u2935\u2b05-\u2b07\u2b1b\u2b1c\u2b50\u2b55\u3030\u303d\u3297\u3299])(?:\ufe0f|(?!\ufe0e))|(?:(?:\ud83c[\udfcb\udfcc]|\ud83d[\udd74\udd75\udd90]|[\u261d\u26f7\u26f9\u270c\u270d])(?:\ufe0f|(?!\ufe0e))|(?:\ud83c[\udf85\udfc2-\udfc4\udfc7\udfca]|\ud83d[\udc42\udc43\udc46-\udc50\udc66-\udc69\udc6e\udc70-\udc78\udc7c\udc81-\udc83\udc85-\udc87\udcaa\udd7a\udd95\udd96\ude45-\ude47\ude4b-\ude4f\udea3\udeb4-\udeb6\udec0\udecc]|\ud83e[\udd0c\udd0f\udd18-\udd1c\udd1e\udd1f\udd26\udd30-\udd39\udd3d\udd3e\udd77\uddb5\uddb6\uddb8\uddb9\uddbb\uddcd-\uddcf\uddd1-\udddd\udec3-\udec5\udef0-\udef6]|[\u270a\u270b]))(?:\ud83c[\udffb-\udfff])?|(?:\ud83c\udff4\udb40\udc67\udb40\udc62\udb40\udc65\udb40\udc6e\udb40\udc67\udb40\udc7f|\ud83c\udff4\udb40\udc67\udb40\udc62\udb40\udc73\udb40\udc63\udb40\udc74\udb40\udc7f|\ud83c\udff4\udb40\udc67\udb40\udc62\udb40\udc77\udb40\udc6c\udb40\udc73\udb40\udc7f|\ud83c\udde6\ud83c[\udde8-\uddec\uddee\uddf1\uddf2\uddf4\uddf6-\uddfa\uddfc\uddfd\uddff]|\ud83c\udde7\ud83c[\udde6\udde7\udde9-\uddef\uddf1-\uddf4\uddf6-\uddf9\uddfb\uddfc\uddfe\uddff]|\ud83c\udde8\ud83c[\udde6\udde8\udde9\uddeb-\uddee\uddf0-\uddf5\uddf7\uddfa-\uddff]|\ud83c\udde9\ud83c[\uddea\uddec\uddef\uddf0\uddf2\uddf4\uddff]|\ud83c\uddea\ud83c[\udde6\udde8\uddea\uddec\udded\uddf7-\uddfa]|\ud83c\uddeb\ud83c[\uddee-\uddf0\uddf2\uddf4\uddf7]|\ud83c\uddec\ud83c[\udde6\udde7\udde9-\uddee\uddf1-\uddf3\uddf5-\uddfa\uddfc\uddfe]|\ud83c\udded\ud83c[\uddf0\uddf2\uddf3\uddf7\uddf9\uddfa]|\ud83c\uddee\ud83c[\udde8-\uddea\uddf1-\uddf4\uddf6-\uddf9]|\ud83c\uddef\ud83c[\uddea\uddf2\uddf4\uddf5]|\ud83c\uddf0\ud83c[\uddea\uddec-\uddee\uddf2\uddf3\uddf5\uddf7\uddfc\uddfe\uddff]|\ud83c\uddf1\ud83c[\udde6-\udde8\uddee\uddf0\uddf7-\uddfb\uddfe]|\ud83c\uddf2\ud83c[\udde6\udde8-\udded\uddf0-\uddff]|\ud83c\uddf3\ud83c[\udde6\udde8\uddea-\uddec\uddee\uddf1\uddf4\uddf5\uddf7\uddfa\uddff]|\ud83c\uddf4\ud83c\uddf2|\ud83c\uddf5\ud83c[\udde6\uddea-\udded\uddf0-\uddf3\uddf7-\uddf9\uddfc\uddfe]|\ud83c\uddf6\ud83c\udde6|\ud83c\uddf7\ud83c[\uddea\uddf4\uddf8\uddfa\uddfc]|\ud83c\uddf8\ud83c[\udde6-\uddea\uddec-\uddf4\uddf7-\uddf9\uddfb\uddfd-\uddff]|\ud83c\uddf9\ud83c[\udde6\udde8\udde9\uddeb-\udded\uddef-\uddf4\uddf7\uddf9\uddfb\uddfc\uddff]|\ud83c\uddfa\ud83c[\udde6\uddec\uddf2\uddf3\uddf8\uddfe\uddff]|\ud83c\uddfb\ud83c[\udde6\udde8\uddea\uddec\uddee\uddf3\uddfa]|\ud83c\uddfc\ud83c[\uddeb\uddf8]|\ud83c\uddfd\ud83c\uddf0|\ud83c\uddfe\ud83c[\uddea\uddf9]|\ud83c\uddff\ud83c[\udde6\uddf2\uddfc]|\ud83c[\udccf\udd8e\udd91-\udd9a\udde6-\uddff\ude01\ude32-\ude36\ude38-\ude3a\ude50\ude51\udf00-\udf20\udf2d-\udf35\udf37-\udf7c\udf7e-\udf84\udf86-\udf93\udfa0-\udfc1\udfc5\udfc6\udfc8\udfc9\udfcf-\udfd3\udfe0-\udff0\udff4\udff8-\udfff]|\ud83d[\udc00-\udc3e\udc40\udc44\udc45\udc51-\udc65\udc6a\udc6f\udc79-\udc7b\udc7d-\udc80\udc84\udc88-\udc8e\udc90\udc92-\udca9\udcab-\udcfc\udcff-\udd3d\udd4b-\udd4e\udd50-\udd67\udda4\uddfb-\ude44\ude48-\ude4a\ude80-\udea2\udea4-\udeb3\udeb7-\udebf\udec1-\udec5\uded0-\uded2\uded5-\uded7\udedd-\udedf\udeeb\udeec\udef4-\udefc\udfe0-\udfeb\udff0]|\ud83e[\udd0d\udd0e\udd10-\udd17\udd20-\udd25\udd27-\udd2f\udd3a\udd3c\udd3f-\udd45\udd47-\udd76\udd78-\uddb4\uddb7\uddba\uddbc-\uddcc\uddd0\uddde-\uddff\ude70-\ude74\ude78-\ude7c\ude80-\ude86\ude90-\udeac\udeb0-\udeba\udec0-\udec2\uded0-\uded9\udee0-\udee7]|[\u23e9-\u23ec\u23f0\u23f3\u267e\u26ce\u2705\u2728\u274c\u274e\u2753-\u2755\u2795-\u2797\u27b0\u27bf\ue50a])|\ufe0f/g

export default regex
```

---

### Two of My Projects Featured on OrcDev’s YouTube Channel <a name="two-of-my-projects-featured-on-orcdev-s-youtube-channel"></a>

Both My Portfolio Website and React Wheel Picker were highlighted by OrcDev in his videos about shadcn tools.

- **Registry Key**: `two-of-my-projects-featured-on-orcdev-s-youtube-channel`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/two-of-my-projects-featured-on-orcdev-s-youtube-channel.json
```

---

### Installing Uptime Kuma with Docker and setting up NGINX with SSL <a name="uptime-kuma"></a>

Uptime Kuma is an easy-to-use self-hosted monitoring tool.

- **Registry Key**: `uptime-kuma`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/uptime-kuma.json
```

---

### Welcome to chanhdai.com <a name="welcome"></a>

A pixel-perfect dev portfolio and shadcn registry showcasing my work as a Design Engineer.

- **Registry Key**: `welcome`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/welcome.json
```

---

### Work Experience <a name="work-experience-component"></a>

Display work experiences with role details, company logos, and durations.

- **Registry Key**: `work-experience-component`

#### Installation

```bash
npx shadcn@latest add https://chanhdai.com/r/work-experience-component.json
```

#### How to Use

##### Example 1

```tsx
import { WorkExperience } from "@/components/work-experience"
import type { ExperienceItemType } from "@/components/work-experience"
```

##### Example 2

```tsx
const experiences: ExperienceItemType[] = []

<WorkExperience experiences={experiences} />
```

---
