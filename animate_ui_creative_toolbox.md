# Animate UI Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **Animate UI** (animate-ui.com) — a library of clean, copy-paste React components designed with Tailwind CSS, TypeScript, Framer Motion (Motion), and Radix Primitives.

## Table of Contents

- [Animate](#animate)
  - [Avatar Group](#avatar-group)
  - [Code Tabs](#code-tabs)
  - [Code](#code)
  - [Cursor](#cursor)
  - [GitHub Stars Wheel](#github-stars-wheel)
  - [Tabs](#tabs)
  - [Tooltip](#tooltip)
- [Backgrounds](#backgrounds)
  - [Bubble Background](#bubble)
  - [Fireworks Background](#fireworks)
  - [Gradient Background](#gradient)
  - [Gravity Stars Background](#gravity-stars)
  - [Hexagon Background](#hexagon)
  - [Hole Background](#hole)
  - [Stars Background](#stars)
- [Base](#base)
  - [Accordion](#accordion)
  - [Alert Dialog](#alert-dialog)
  - [Checkbox](#checkbox)
  - [Dialog](#dialog)
  - [Files](#files)
  - [Menu](#menu)
  - [Popover](#popover)
  - [Preview Card](#preview-card)
  - [Preview Link Card](#preview-link-card)
  - [Progress](#progress)
  - [Radio](#radio)
  - [Switch](#switch)
  - [Tabs](#tabs)
  - [Toggle Group](#toggle-group)
  - [Toggle](#toggle)
  - [Tooltip](#tooltip)
- [Buttons](#buttons)
  - [Button](#button)
  - [Copy Button](#copy)
  - [Flip Button](#flip)
  - [GitHub Stars Button](#github-stars)
  - [Icon Button](#icon)
  - [Liquid Button](#liquid)
  - [Ripple Button](#ripple)
  - [Theme Toggler Button](#theme-toggler)
- [Community](#community)
  - [Flip Card](#flip-card)
  - [Management Bar](#management-bar)
  - [Motion Carousel](#motion-carousel)
  - [Notification List](#notification-list)
  - [Pin List](#pin-list)
  - [Playful Todolist](#playful-todolist)
  - [Radial Intro](#radial-intro)
  - [Radial Menu](#radial-menu)
  - [Radial Nav](#radial-nav)
  - [Share Button](#share-button)
  - [User Presence Avatar](#user-presence-avatar)
- [Headless](#headless)
  - [Accordion](#accordion)
  - [Checkbox](#checkbox)
  - [Dialog](#dialog)
  - [Popover](#popover)
  - [Switch](#switch)
  - [Tabs](#tabs)
- [Radix](#radix)
  - [Accordion](#accordion)
  - [Alert Dialog](#alert-dialog)
  - [Checkbox](#checkbox)
  - [Dialog](#dialog)
  - [Dropdown Menu](#dropdown-menu)
  - [Files](#files)
  - [Hover Card](#hover-card)
  - [Popover](#popover)
  - [Preview Link Card](#preview-link-card)
  - [Progress](#progress)
  - [Radio Group](#radio-group)
  - [Sheet](#sheet)
  - [Sidebar](#sidebar)
  - [Switch](#switch)
  - [Tabs](#tabs)
  - [Toggle Group](#toggle-group)
  - [Toggle](#toggle)
  - [Tooltip](#tooltip)

---

## Animate <a name="animate"></a>

### Avatar Group <a name="avatar-group"></a>

An animated avatar group that displays overlapping user images and smoothly shifts each avatar forward on hover to highlight it.

- **Category**: Animate
- **Registry Key**: `components-animate-avatar-group`

#### Installation

```bash
npx shadcn@latest add components-animate-avatar-group
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
<AvatarGroup>
  {AVATARS.map((avatar, index) => (
    <Avatar key={index}>
      <AvatarImage src={avatar.src} />
      <AvatarFallback>{avatar.fallback}</AvatarFallback>
      <AvatarGroupTooltip>{avatar.tooltip}</AvatarGroupTooltip>
    </Avatar>
  ))}
</AvatarGroup>
```

#### Demo Code

##### File: `registry/demo/components/animate/avatar-group/index.tsx`

```tsx
'use client';

import {
  AvatarGroup,
  AvatarGroupTooltip,
} from '@/components/animate-ui/components/animate/avatar-group';
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from '@/components/ui/avatar';

const AVATARS = [
  {
    src: 'https://pbs.twimg.com/profile_images/1948770261848756224/oPwqXMD6_400x400.jpg',
    fallback: 'SK',
    tooltip: 'Skyleen',
  },
  {
    src: 'https://pbs.twimg.com/profile_images/1593304942210478080/TUYae5z7_400x400.jpg',
    fallback: 'CN',
    tooltip: 'Shadcn',
  },
  {
    src: 'https://pbs.twimg.com/profile_images/1677042510839857154/Kq4tpySA_400x400.jpg',
    fallback: 'AW',
    tooltip: 'Adam Wathan',
  },
  {
    src: 'https://pbs.twimg.com/profile_images/1783856060249595904/8TfcCN0r_400x400.jpg',
    fallback: 'GR',
    tooltip: 'Guillermo Rauch',
  },
  {
    src: 'https://pbs.twimg.com/profile_images/1534700564810018816/anAuSfkp_400x400.jpg',
    fallback: 'JH',
    tooltip: 'Jhey',
  },
  {
    src: 'https://pbs.twimg.com/profile_images/1927474594102784000/Al0g-I6o_400x400.jpg',
    fallback: 'DH',
    tooltip: 'David Haz',
  },
];

export const AvatarGroupDemo = () => {
  return (
    <AvatarGroup>
      {AVATARS.map((avatar, index) => (
        <Avatar key={index} className="size-12 border-3 border-background">
          <AvatarImage src={avatar.src} />
          <AvatarFallback>{avatar.fallback}</AvatarFallback>
          <AvatarGroupTooltip>{avatar.tooltip}</AvatarGroupTooltip>
        </Avatar>
      ))}
    </AvatarGroup>
  );
};
```

#### Component Source Code

##### File: `registry/components/animate/avatar-group/index.tsx`

```tsx
import * as React from 'react';
import * as motion from 'motion/react-client';

import {
  AvatarGroup as AvatarGroupPrimitive,
  AvatarGroupTooltip as AvatarGroupTooltipPrimitive,
  AvatarGroupTooltipArrow as AvatarGroupTooltipArrowPrimitive,
  type AvatarGroupProps as AvatarGroupPropsPrimitive,
  type AvatarGroupTooltipProps as AvatarGroupTooltipPropsPrimitive,
} from '@/components/animate-ui/primitives/animate/avatar-group';
import { cn } from '@/lib/utils';

type AvatarGroupProps = AvatarGroupPropsPrimitive;

function AvatarGroup({
  className,
  invertOverlap = true,
  ...props
}: AvatarGroupProps) {
  return (
    <AvatarGroupPrimitive
      className={cn('h-12 -space-x-3', className)}
      invertOverlap={invertOverlap}
      {...props}
    />
  );
}

type AvatarGroupTooltipProps = Omit<
  AvatarGroupTooltipPropsPrimitive,
  'asChild'
> & {
  children: React.ReactNode;
  layout?: boolean | 'position' | 'size' | 'preserve-aspect';
};

function AvatarGroupTooltip({
  className,
  children,
  layout = 'preserve-aspect',
  ...props
}: AvatarGroupTooltipProps) {
  return (
    <AvatarGroupTooltipPrimitive
      className={cn(
        'bg-primary text-primary-foreground z-50 w-fit rounded-md px-3 py-1.5 text-xs text-balance',
        className,
      )}
      {...props}
    >
      <motion.div layout={layout} className="overflow-hidden">
        {children}
      </motion.div>
      <AvatarGroupTooltipArrowPrimitive
        className="fill-primary size-3 data-[side='bottom']:translate-y-[1px] data-[side='right']:translate-x-[1px] data-[side='left']:translate-x-[-1px] data-[side='top']:translate-y-[-1px]"
        tipRadius={2}
      />
    </AvatarGroupTooltipPrimitive>
  );
}

export {
  AvatarGroup,
  AvatarGroupTooltip,
  type AvatarGroupProps,
  type AvatarGroupTooltipProps,
};
```

---

### Code Tabs <a name="code-tabs"></a>

A tabs component that displays code for different languages.

- **Category**: Animate
- **Registry Key**: `components-animate-code-tabs`

#### Installation

```bash
npx shadcn@latest add components-animate-code-tabs
```

#### Dependencies

- **NPM Packages**:
  - `shiki`

#### How to Use

```tsx
<CodeTabs codes={codes} />
```

#### Demo Code

##### File: `registry/demo/components/animate/code-tabs/index.tsx`

```tsx
import { CodeTabs } from '@/components/animate-ui/components/animate/code-tabs';

const CODES = {
  Cursor: `// Copy and paste the code into .cursor/mcp.json
{
  "mcpServers": {
    "shadcn": {
      "command": "npx",
      "args": ["-y", "shadcn@canary", "registry:mcp"],
      "env": {
        "REGISTRY_URL": "@animate-ui/registry.json"
      }
    }
  }
}`,
  Windsurf: `// Copy and paste the code into .codeium/windsurf/mcp_config.json
{
  "mcpServers": {
    "shadcn": {
      "command": "npx",
      "args": ["-y", "shadcn@canary", "registry:mcp"],
      "env": {
        "REGISTRY_URL": "@animate-ui/registry.json"
      }
    }
  }
}`,
};

export const CodeTabsDemo = () => {
  return <CodeTabs lang="json" codes={CODES} />;
};
```

#### Component Source Code

##### File: `registry/components/animate/code-tabs/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { useTheme } from 'next-themes';

import { cn } from '@/lib/utils';
import {
  Tabs,
  TabsContent,
  TabsList,
  TabsTrigger,
  TabsContents,
  TabsHighlight,
  TabsHighlightItem,
  type TabsProps,
} from '@/components/animate-ui/primitives/animate/tabs';
import { CopyButton } from '@/components/animate-ui/components/buttons/copy';

type CodeTabsProps = {
  codes: Record<string, string>;
  lang?: string;
  themes?: { light: string; dark: string };
  copyButton?: boolean;
  onCopiedChange?: (copied: boolean, content?: string) => void;
} & Omit<TabsProps, 'children'>;

function CodeTabs({
  codes,
  lang = 'bash',
  themes = {
    light: 'github-light',
    dark: 'github-dark',
  },
  className,
  defaultValue,
  value,
  onValueChange,
  copyButton = true,
  onCopiedChange,
  ...props
}: CodeTabsProps) {
  const { resolvedTheme } = useTheme();

  const [highlightedCodes, setHighlightedCodes] = React.useState<Record<
    string,
    string
  > | null>(null);
  const [selectedCode, setSelectedCode] = React.useState<string>(
    value ?? defaultValue ?? Object.keys(codes)[0] ?? '',
  );

  React.useEffect(() => {
    async function loadHighlightedCode() {
      try {
        const { codeToHtml } = await import('shiki');
        const newHighlightedCodes: Record<string, string> = {};

        for (const [command, val] of Object.entries(codes)) {
          const highlighted = await codeToHtml(val, {
            lang,
            themes: {
              light: themes.light,
              dark: themes.dark,
            },
            defaultColor: resolvedTheme === 'dark' ? 'dark' : 'light',
          });

          newHighlightedCodes[command] = highlighted;
        }

        setHighlightedCodes(newHighlightedCodes);
      } catch (error) {
        console.error('Error highlighting codes', error);
        setHighlightedCodes(codes);
      }
    }
    loadHighlightedCode();
  }, [resolvedTheme, lang, themes.light, themes.dark, codes]);

  return (
    <Tabs
      data-slot="install-tabs"
      className={cn(
        'w-full gap-0 bg-muted/50 rounded-xl border overflow-hidden',
        className,
      )}
      {...props}
      value={selectedCode}
      onValueChange={(val) => {
        setSelectedCode(val);
        onValueChange?.(val);
      }}
    >
      <TabsHighlight className="absolute z-0 inset-0 rounded-none shadow-none bg-transparent after:content-[''] after:absolute after:inset-x-0 after:h-0.5 after:bottom-0 dark:after:bg-white after:bg-black after:rounded-t-full">
        <TabsList
          data-slot="install-tabs-list"
          className="w-full relative flex items-center justify-between rounded-none h-10 bg-muted border-b border-border/75 dark:border-border/50 text-current py-0 px-4"
        >
          <div className="flex gap-x-3 h-full">
            {highlightedCodes &&
              Object.keys(highlightedCodes).map((code) => (
                <TabsHighlightItem
                  key={code}
                  value={code}
                  className="flex items-center justify-center"
                >
                  <TabsTrigger
                    key={code}
                    value={code}
                    className="text-muted-foreground h-full text-sm font-medium data-[state=active]:text-current px-0"
                  >
                    {code}
                  </TabsTrigger>
                </TabsHighlightItem>
              ))}
          </div>

          {copyButton && highlightedCodes && (
            <CopyButton
              content={codes[selectedCode]}
              size="xs"
              variant="ghost"
              className="-me-2.5 bg-transparent hover:bg-black/5 dark:hover:bg-white/10"
              onCopiedChange={onCopiedChange}
            />
          )}
        </TabsList>
      </TabsHighlight>

      <TabsContents data-slot="install-tabs-contents">
        {highlightedCodes &&
          Object.entries(highlightedCodes).map(([code, val]) => (
            <TabsContent
              data-slot="install-tabs-content"
              key={code}
              className="w-full"
              value={code}
            >
              <div
                className="w-full text-sm overflow-auto flex items-center p-4 [&>pre,_&_code]:!bg-transparent [&_code_.line]:!px-0 [&>pre,_&_code]:[background:transparent_!important] [&>pre,_&_code]:border-none [&_code]:!text-[13px]"
                dangerouslySetInnerHTML={{ __html: val }}
              />
            </TabsContent>
          ))}
      </TabsContents>
    </Tabs>
  );
}

export { CodeTabs, type CodeTabsProps };
```

---

### Code <a name="code"></a>

A code component that animates the code as it is written.

- **Category**: Animate
- **Registry Key**: `components-animate-code`

#### Installation

```bash
npx shadcn@latest add components-animate-code
```

#### Dependencies

- **NPM Packages**:
  - `next-themes`

#### How to Use

```tsx
<Code>
  <CodeHeader />
  <CodeBlock />
</Code>
```

#### Demo Code

##### File: `registry/demo/components/animate/code/index.tsx`

```tsx
'use client';

import {
  Code,
  CodeBlock,
  CodeHeader,
} from '@/components/animate-ui/components/animate/code';
import ReactIcon from '@/components/icons/react-icon';
import { File } from 'lucide-react';

interface CodeDemoProps {
  duration: number;
  delay: number;
  writing: boolean;
  cursor: boolean;
}

export const CodeDemo = ({
  duration,
  delay,
  writing,
  cursor,
}: CodeDemoProps) => {
  return (
    <Code
      key={`${duration}-${delay}-${writing}-${cursor}`}
      className="w-[420px] h-[372px]"
      code={`'use client';
 
import * as React from 'react';
  
type MyComponentProps = {
  myProps: string;
} & React.ComponentProps<'div'>;
  
function MyComponent(props: MyComponentProps) {
  return (
    <div {...props}>
      <p>My Component</p>
    </div>
  );
}

export { MyComponent, type MyComponentProps };`}
    >
      <CodeHeader icon={ReactIcon} copyButton>
        my-component.tsx
      </CodeHeader>

      <CodeBlock
        cursor={cursor}
        lang="tsx"
        writing={writing}
        duration={duration}
        delay={delay}
      />
    </Code>
  );
};
```

#### Component Source Code

##### File: `registry/components/animate/code/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { useTheme } from 'next-themes';

import {
  CodeBlock as CodeBlockPrimitive,
  type CodeBlockProps as CodeBlockPropsPrimitive,
} from '@/components/animate-ui/primitives/animate/code-block';
import { cn } from '@/lib/utils';
import { CopyButton } from '@/components/animate-ui/components/buttons/copy';
import { getStrictContext } from '@/lib/get-strict-context';

type CodeContextType = {
  code: string;
};

const [CodeProvider, useCode] =
  getStrictContext<CodeContextType>('CodeContext');

type CodeProps = React.ComponentProps<'div'> & {
  code: string;
};

function Code({ className, code, ...props }: CodeProps) {
  return (
    <CodeProvider value={{ code }}>
      <div
        className={cn(
          'relative flex flex-col overflow-hidden border bg-accent/50 rounded-lg',
          className,
        )}
        {...props}
      />
    </CodeProvider>
  );
}

type CodeHeaderProps = React.ComponentProps<'div'> & {
  icon?: React.ElementType;
  copyButton?: boolean;
};

function CodeHeader({
  className,
  children,
  icon: Icon,
  copyButton = false,
  ...props
}: CodeHeaderProps) {
  const { code } = useCode();

  return (
    <div
      className={cn(
        'bg-accent shrink-0 gap-x-2 border-b border-border/75 dark:border-border/50 text-sm flex text-muted-foreground items-center px-4 w-full h-10',
        className,
      )}
      {...props}
    >
      {Icon && <Icon className="size-4" />}
      {children}
      {copyButton && (
        <CopyButton
          content={code}
          size="xs"
          variant="ghost"
          className="ml-auto w-auto h-auto p-2 -mr-2"
        />
      )}
    </div>
  );
}

type CodeBlockProps = Omit<CodeBlockPropsPrimitive, 'code'> & {
  cursor?: boolean;
};

function CodeBlock({ cursor, className, ...props }: CodeBlockProps) {
  const { resolvedTheme } = useTheme();
  const { code } = useCode();
  const scrollRef = React.useRef<HTMLDivElement>(null);

  return (
    <CodeBlockPrimitive
      ref={scrollRef}
      theme={resolvedTheme === 'dark' ? 'dark' : 'light'}
      scrollContainerRef={scrollRef}
      className={cn(
        'relative text-sm p-4 overflow-auto',
        '[&>pre,_&_code]:!bg-transparent [&>pre,_&_code]:[background:transparent_!important] [&>pre,_&_code]:border-none [&_code]:!text-[13px] [&_code_.line]:!px-0',
        cursor &&
          "data-[done=false]:[&_.line:last-of-type::after]:content-['|'] data-[done=false]:[&_.line:last-of-type::after]:inline-block data-[done=false]:[&_.line:last-of-type::after]:w-[1ch] data-[done=false]:[&_.line:last-of-type::after]:-translate-px",
        className,
      )}
      code={code}
      {...props}
    />
  );
}

export {
  Code,
  CodeHeader,
  CodeBlock,
  type CodeProps,
  type CodeHeaderProps,
  type CodeBlockProps,
};
```

---

### Cursor <a name="cursor"></a>

An animated cursor component that allows you to customize both the cursor and cursor follow elements with smooth animations.

- **Category**: Animate
- **Registry Key**: `components-animate-cursor`

#### Installation

```bash
npx shadcn@latest add components-animate-cursor
```

#### How to Use

```tsx
<CursorProvider>
  <Cursor />
  <CursorFollow>Designer</CursorFollow>
</CursorProvider>
```

#### Demo Code

##### File: `registry/demo/components/animate/cursor/index.tsx`

```tsx
import {
  Cursor,
  CursorFollow,
  CursorProvider,
  type CursorFollowProps,
} from '@/components/animate-ui/components/animate/cursor';

interface CursorDemoProps {
  global?: boolean;
  enableCursor?: boolean;
  enableCursorFollow?: boolean;
  side?: CursorFollowProps['side'];
  sideOffset?: number;
  align?: CursorFollowProps['align'];
  alignOffset?: number;
}

export const CursorDemo = ({
  global = false,
  enableCursor = true,
  enableCursorFollow = true,
  side = 'bottom',
  sideOffset = 15,
  align = 'end',
  alignOffset = 5,
}: CursorDemoProps) => {
  return (
    <div
      key={String(global)}
      className="max-w-[400px] h-[400px] w-full bg-accent rounded-lg flex items-center justify-center"
    >
      <p className="font-medium italic text-muted-foreground">
        Move your mouse over the div
      </p>
      <CursorProvider global={global}>
        {enableCursor && <Cursor />}
        {enableCursorFollow && (
          <CursorFollow
            side={side}
            sideOffset={sideOffset}
            align={align}
            alignOffset={alignOffset}
          >
            Designer
          </CursorFollow>
        )}
      </CursorProvider>
    </div>
  );
};
```

#### Component Source Code

##### File: `registry/components/animate/cursor/index.tsx`

```tsx
import * as React from 'react';

import {
  CursorProvider as CursorProviderPrimitive,
  Cursor as CursorPrimitive,
  CursorFollow as CursorFollowPrimitive,
  CursorContainer as CursorContainerPrimitive,
  type CursorProviderProps as CursorProviderPropsPrimitive,
  type CursorContainerProps as CursorContainerPropsPrimitive,
  type CursorProps as CursorPropsPrimitive,
  type CursorFollowProps as CursorFollowPropsPrimitive,
} from '@/components/animate-ui/primitives/animate/cursor';
import { cn } from '@/lib/utils';

type CursorProviderProps = Omit<CursorProviderPropsPrimitive, 'children'> &
  CursorContainerPropsPrimitive;

function CursorProvider({ global, ...props }: CursorProviderProps) {
  return (
    <CursorProviderPrimitive global={global}>
      <CursorContainerPrimitive {...props} />
    </CursorProviderPrimitive>
  );
}

type CursorProps = Omit<CursorPropsPrimitive, 'children' | 'asChild'>;

function Cursor({ className, ...props }: CursorProps) {
  return (
    <CursorPrimitive asChild {...props}>
      <svg
        className={cn('size-6 text-foreground', className)}
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 40 40"
      >
        <path
          fill="currentColor"
          d="M1.8 4.4 7 36.2c.3 1.8 2.6 2.3 3.6.8l3.9-5.7c1.7-2.5 4.5-4.1 7.5-4.3l6.9-.5c1.8-.1 2.5-2.4 1.1-3.5L5 2.5c-1.4-1.1-3.5 0-3.3 1.9Z"
        />
      </svg>
    </CursorPrimitive>
  );
}

type CursorFollowProps = Omit<CursorFollowPropsPrimitive, 'asChild'>;

function CursorFollow({
  className,
  children,
  sideOffset = 15,
  alignOffset = 5,
  ...props
}: CursorFollowProps) {
  return (
    <CursorFollowPrimitive
      sideOffset={sideOffset}
      alignOffset={alignOffset}
      asChild
      {...props}
    >
      <div
        className={cn(
          'bg-foreground rounded-md text-background px-2 py-1 text-sm',
          className,
        )}
      >
        {children}
      </div>
    </CursorFollowPrimitive>
  );
}

export {
  CursorProvider,
  Cursor,
  CursorFollow,
  type CursorProviderProps,
  type CursorProps,
  type CursorFollowProps,
};
```

---

### GitHub Stars Wheel <a name="github-stars-wheel"></a>

A scrolling wheel that displays GitHub stars count.

- **Category**: Animate
- **Registry Key**: `components-animate-github-stars-wheel`

#### Installation

```bash
npx shadcn@latest add components-animate-github-stars-wheel
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
<GitHubStarsWheel />
```

#### Demo Code

##### File: `registry/demo/components/animate/github-stars-wheel/index.tsx`

```tsx
import { GitHubStarsWheel } from '@/components/animate-ui/components/animate/github-stars-wheel';

interface GitHubStarsWheelDemoProps {
  delay: number;
  direction: 'btt' | 'ttb';
}

export const GitHubStarsWheelDemo = ({
  delay,
  direction,
}: GitHubStarsWheelDemoProps) => {
  return (
    <div className="size-full flex items-center justify-center">
      <GitHubStarsWheel
        username="imskyleen"
        repo="animate-ui"
        delay={delay}
        direction={direction}
      />
    </div>
  );
};
```

#### Component Source Code

##### File: `registry/components/animate/github-stars-wheel/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { StarIcon } from 'lucide-react';

import {
  ScrollingNumber as ScrollingNumberPrimitive,
  ScrollingNumberContainer as ScrollingNumberContainerPrimitive,
  ScrollingNumberItems as ScrollingNumberItemsPrimitive,
  ScrollingNumberHighlight as ScrollingNumberHighlightPrimitive,
  type ScrollingNumberContainerProps as ScrollingNumberContainerPrimitiveProps,
} from '@/components/animate-ui/primitives/texts/scrolling-number';
import { cn } from '@/lib/utils';
import {
  Particles,
  ParticlesEffect,
} from '@/components/animate-ui/primitives/effects/particles';

function percentageBetween(value: number, min: number, max: number): number {
  return ((value - min) / (max - min)) * 100;
}

type GitHubStarsWheelProps = {
  username?: string;
  repo?: string;
  direction?: 'btt' | 'ttb';
  delay?: number;
  value?: number;
  step?: number;
} & Omit<
  ScrollingNumberContainerPrimitiveProps,
  'direction' | 'number' | 'step'
>;

function GitHubStarsWheel({
  username,
  repo,
  direction = 'btt',
  itemsSize = 35,
  sideItemsCount = 2,
  delay = 0,
  step = 100,
  value,
  className,
  ...props
}: GitHubStarsWheelProps) {
  const [stars, setStars] = React.useState(value ?? 0);
  const [currentStars, setCurrentStars] = React.useState(0);
  const [isLoading, setIsLoading] = React.useState(true);
  const roundedStars = React.useMemo(
    () => Math.round(stars / step) * step,
    [stars, step],
  );
  const isCompleted = React.useMemo(
    () => currentStars === roundedStars,
    [currentStars, roundedStars],
  );
  const fillPercentage = React.useMemo(
    () => percentageBetween(currentStars, 0, roundedStars),
    [currentStars, roundedStars],
  );

  React.useEffect(() => {
    if (value !== undefined && username && repo) return;

    const timeout = setTimeout(() => {
      fetch(`https://api.github.com/repos/${username}/${repo}`)
        .then((response) => response.json())
        .then((data) => {
          if (data && typeof data.stargazers_count === 'number') {
            setStars(data.stargazers_count);
          }
        })
        .catch(console.error)
        .finally(() => setIsLoading(false));
    }, delay);

    return () => clearTimeout(timeout);
  }, [username, repo, value, delay]);

  return (
    !isLoading && (
      <ScrollingNumberContainerPrimitive
        key={direction}
        className={cn('w-28', className)}
        direction={direction}
        number={roundedStars}
        step={step}
        itemsSize={itemsSize}
        onNumberChange={setCurrentStars}
        {...props}
      >
        <div
          className="absolute top-0 left-0 w-full bg-gradient-to-t from-transparent to-background z-10"
          style={{
            height: `${itemsSize * sideItemsCount}px`,
          }}
        />
        <div
          className="absolute bottom-0 left-0 w-full bg-gradient-to-b from-transparent to-background z-10"
          style={{
            height: `${itemsSize * sideItemsCount}px`,
          }}
        />
        <ScrollingNumberPrimitive delay={delay}>
          <ScrollingNumberItemsPrimitive className="flex items-center justify-start pl-8" />
        </ScrollingNumberPrimitive>
        <ScrollingNumberHighlightPrimitive className="bg-accent/40 border rounded-md size-full flex items-center pl-2">
          <Particles animate={isCompleted}>
            <StarIcon
              aria-hidden="true"
              className="fill-neutral-300 stroke-neutral-300 dark:fill-neutral-700 dark:stroke-neutral-700 size-4"
            />
            <StarIcon
              aria-hidden="true"
              className="absolute top-0 left-0 size-4 fill-yellow-500 stroke-yellow-500"
              style={{
                clipPath: `inset(${100 - (isCompleted ? fillPercentage : fillPercentage - 10)}% 0 0 0)`,
              }}
            />
            <ParticlesEffect
              delay={0.5}
              className="size-1 rounded-full bg-yellow-500"
            />
          </Particles>
        </ScrollingNumberHighlightPrimitive>
      </ScrollingNumberContainerPrimitive>
    )
  );
}

export { GitHubStarsWheel, type GitHubStarsWheelProps };
```

---

### Tabs <a name="tabs"></a>

A set of layered sections of content—known as tab panels—that are displayed one at a time.

- **Category**: Animate
- **Registry Key**: `components-animate-tabs`

#### Installation

```bash
npx shadcn@latest add components-animate-tabs
```

#### How to Use

```tsx
<Tabs>
  <TabsList>
    <TabsTrigger value="account">Account</TabsTrigger>
    <TabsTrigger value="password">Password</TabsTrigger>
  </TabsList>
  <TabContents>
    <TabsContent value="account">
      Make changes to your account here.
    </TabsContent>
    <TabsContent value="password">Change your password here.</TabsContent>
  </TabContents>
</Tabs>
```

#### Demo Code

##### File: `registry/demo/components/animate/tabs/index.tsx`

```tsx
import {
  Tabs,
  TabsContent,
  TabsContents,
  TabsList,
  TabsTrigger,
} from '@/components/animate-ui/components/animate/tabs';
import { Button } from '@/components/ui/button';
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';

export function AnimateTabsDemo() {
  return (
    <div className="flex w-full max-w-sm flex-col gap-6">
      <Tabs defaultValue="account">
        <TabsList>
          <TabsTrigger value="account">Account</TabsTrigger>
          <TabsTrigger value="password">Password</TabsTrigger>
        </TabsList>
        <Card className="shadow-none py-0">
          <TabsContents className="py-6">
            <TabsContent value="account" className="flex flex-col gap-6">
              <CardHeader>
                <CardTitle>Account</CardTitle>
                <CardDescription>
                  Make changes to your account here. Click save when you&apos;re
                  done.
                </CardDescription>
              </CardHeader>
              <CardContent className="grid gap-6">
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-name">Name</Label>
                  <Input id="tabs-demo-name" defaultValue="Pedro Duarte" />
                </div>
              </CardContent>
              <CardFooter>
                <Button>Save changes</Button>
              </CardFooter>
            </TabsContent>
            <TabsContent value="password" className="flex flex-col gap-6">
              <CardHeader>
                <CardTitle>Password</CardTitle>
                <CardDescription>
                  Change your password here. After saving, you&apos;ll be logged
                  out.
                </CardDescription>
              </CardHeader>
              <CardContent className="grid gap-6">
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-current">Current password</Label>
                  <Input id="tabs-demo-current" type="password" />
                </div>
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-new">New password</Label>
                  <Input id="tabs-demo-new" type="password" />
                </div>
              </CardContent>
              <CardFooter>
                <Button>Save password</Button>
              </CardFooter>
            </TabsContent>
          </TabsContents>
        </Card>
      </Tabs>
    </div>
  );
}
```

#### Component Source Code

##### File: `registry/components/animate/tabs/index.tsx`

```tsx
import * as React from 'react';

import {
  Tabs as TabsPrimitive,
  TabsList as TabsListPrimitive,
  TabsTrigger as TabsTriggerPrimitive,
  TabsContent as TabsContentPrimitive,
  TabsContents as TabsContentsPrimitive,
  TabsHighlight as TabsHighlightPrimitive,
  TabsHighlightItem as TabsHighlightItemPrimitive,
  type TabsProps as TabsPrimitiveProps,
  type TabsListProps as TabsListPrimitiveProps,
  type TabsTriggerProps as TabsTriggerPrimitiveProps,
  type TabsContentProps as TabsContentPrimitiveProps,
  type TabsContentsProps as TabsContentsPrimitiveProps,
} from '@/components/animate-ui/primitives/animate/tabs';
import { cn } from '@/lib/utils';

type TabsProps = TabsPrimitiveProps;

function Tabs({ className, ...props }: TabsProps) {
  return (
    <TabsPrimitive
      className={cn('flex flex-col gap-2', className)}
      {...props}
    />
  );
}

type TabsListProps = TabsListPrimitiveProps;

function TabsList({ className, ...props }: TabsListProps) {
  return (
    <TabsHighlightPrimitive className="absolute z-0 inset-0 border border-transparent rounded-md bg-background dark:border-input dark:bg-input/30 shadow-sm">
      <TabsListPrimitive
        className={cn(
          'bg-muted text-muted-foreground inline-flex h-9 w-fit items-center justify-center rounded-lg p-[3px]',
          className,
        )}
        {...props}
      />
    </TabsHighlightPrimitive>
  );
}

type TabsTriggerProps = TabsTriggerPrimitiveProps;

function TabsTrigger({ className, ...props }: TabsTriggerProps) {
  return (
    <TabsHighlightItemPrimitive value={props.value} className="flex-1">
      <TabsTriggerPrimitive
        className={cn(
          "data-[state=active]:text-foreground focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:outline-ring text-muted-foreground inline-flex h-[calc(100%-1px)] flex-1 items-center justify-center gap-1.5 rounded-md w-full px-2 py-1 text-sm font-medium whitespace-nowrap transition-colors duration-500 ease-in-out focus-visible:ring-[3px] focus-visible:outline-1 disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
          className,
        )}
        {...props}
      />
    </TabsHighlightItemPrimitive>
  );
}

type TabsContentsProps = TabsContentsPrimitiveProps;

function TabsContents(props: TabsContentsProps) {
  return <TabsContentsPrimitive {...props} />;
}

type TabsContentProps = TabsContentPrimitiveProps;

function TabsContent({ className, ...props }: TabsContentProps) {
  return (
    <TabsContentPrimitive
      className={cn('outline-none', className)}
      {...props}
    />
  );
}

export {
  Tabs,
  TabsList,
  TabsTrigger,
  TabsContents,
  TabsContent,
  type TabsProps,
  type TabsListProps,
  type TabsTriggerProps,
  type TabsContentsProps,
  type TabsContentProps,
};
```

---

### Tooltip <a name="tooltip"></a>

A tooltip is a small box that appears when hovering over an element.

- **Category**: Animate
- **Registry Key**: `components-animate-tooltip`

#### Installation

```bash
npx shadcn@latest add components-animate-tooltip
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
<TooltipProvider>
  <Tooltip>
    <TooltipTrigger>Hover</TooltipTrigger>
    <TooltipContent>
      <p>Tooltip content</p>
    </TooltipContent>
  </Tooltip>

  <Tooltip>
    <TooltipTrigger>And me</TooltipTrigger>
    <TooltipContent>
      <p>Tooltip content</p>
    </TooltipContent>
  </Tooltip>
</TooltipProvider>
```

#### Demo Code

##### File: `registry/demo/components/animate/tooltip/index.tsx`

```tsx
'use client';

import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from '@/components/animate-ui/components/animate/tooltip';
import { Button } from '@/components/ui/button';

interface TooltipDemoProps {
  openDelay?: number;
  closeDelay?: number;
  side?: 'top' | 'bottom' | 'left' | 'right';
  sideOffset?: number;
  align?: 'start' | 'center' | 'end';
  alignOffset?: number;
}

export const AnimateTooltipDemo = ({
  openDelay,
  closeDelay,
  side,
  sideOffset,
  align,
  alignOffset,
}: TooltipDemoProps) => {
  return (
    <TooltipProvider
      key={`${side}-${align}-${sideOffset}-${alignOffset}-${openDelay}-${closeDelay}`}
      openDelay={openDelay}
      closeDelay={closeDelay}
    >
      <div className="flex flex-col gap-5 justify-center items-center">
        <div className="flex flex-row gap-2 border p-2">
          <Tooltip
            side={side}
            sideOffset={sideOffset}
            align={align}
            alignOffset={alignOffset}
          >
            <TooltipTrigger>
              <Button variant="accent">Docs</Button>
            </TooltipTrigger>

            <TooltipContent>
              <p>Documentation</p>
            </TooltipContent>
          </Tooltip>

          <Tooltip
            side={side}
            sideOffset={sideOffset}
            align={align}
            alignOffset={alignOffset}
          >
            <TooltipTrigger>
              <Button variant="accent">Lorem</Button>
            </TooltipTrigger>

            <TooltipContent>
              <p>Lorem ipsum dolor sit amet</p>
              <p>consectetur adipisicing elit</p>
            </TooltipContent>
          </Tooltip>

          <Tooltip
            side={side}
            sideOffset={sideOffset}
            align={align}
            alignOffset={alignOffset}
          >
            <TooltipTrigger>
              <Button variant="accent">Guide</Button>
            </TooltipTrigger>

            <TooltipContent>
              <p>User Guide</p>
            </TooltipContent>
          </Tooltip>
        </div>
        <div className="flex flex-row gap-5">
          <Tooltip
            side={side}
            sideOffset={sideOffset}
            align={align}
            alignOffset={alignOffset}
          >
            <TooltipTrigger>
              <Button variant="accent">Repo</Button>
            </TooltipTrigger>

            <TooltipContent>
              <p>GitHub</p>
            </TooltipContent>
          </Tooltip>
        </div>
      </div>
    </TooltipProvider>
  );
};
```

#### Component Source Code

##### File: `registry/components/animate/tooltip/index.tsx`

```tsx
import * as React from 'react';
import * as motion from 'motion/react-client';

import {
  TooltipProvider as TooltipProviderPrimitive,
  Tooltip as TooltipPrimitive,
  TooltipTrigger as TooltipTriggerPrimitive,
  TooltipContent as TooltipContentPrimitive,
  TooltipArrow as TooltipArrowPrimitive,
  type TooltipProviderProps as TooltipProviderPrimitiveProps,
  type TooltipProps as TooltipPrimitiveProps,
  type TooltipTriggerProps as TooltipTriggerPrimitiveProps,
  type TooltipContentProps as TooltipContentPrimitiveProps,
} from '@/components/animate-ui/primitives/animate/tooltip';
import { cn } from '@/lib/utils';

type TooltipProviderProps = TooltipProviderPrimitiveProps;

function TooltipProvider({ openDelay = 0, ...props }: TooltipProviderProps) {
  return <TooltipProviderPrimitive openDelay={openDelay} {...props} />;
}

type TooltipProps = TooltipPrimitiveProps;

function Tooltip({ sideOffset = 10, ...props }: TooltipProps) {
  return <TooltipPrimitive sideOffset={sideOffset} {...props} />;
}

type TooltipTriggerProps = TooltipTriggerPrimitiveProps;

function TooltipTrigger({ ...props }: TooltipTriggerProps) {
  return <TooltipTriggerPrimitive {...props} />;
}

type TooltipContentProps = Omit<TooltipContentPrimitiveProps, 'asChild'> & {
  children: React.ReactNode;
  layout?: boolean | 'position' | 'size' | 'preserve-aspect';
};

function TooltipContent({
  className,
  children,
  layout = 'preserve-aspect',
  ...props
}: TooltipContentProps) {
  return (
    <TooltipContentPrimitive
      className={cn(
        'z-50 w-fit bg-primary text-primary-foreground rounded-md',
        className,
      )}
      {...props}
    >
      <motion.div className="overflow-hidden px-3 py-1.5 text-xs text-balance">
        <motion.div layout={layout}>{children}</motion.div>
      </motion.div>
      <TooltipArrowPrimitive
        className="fill-primary size-3 data-[side='bottom']:translate-y-[1px] data-[side='right']:translate-x-[1px] data-[side='left']:translate-x-[-1px] data-[side='top']:translate-y-[-1px]"
        tipRadius={2}
      />
    </TooltipContentPrimitive>
  );
}

export {
  TooltipProvider,
  Tooltip,
  TooltipTrigger,
  TooltipContent,
  type TooltipProviderProps,
  type TooltipProps,
  type TooltipTriggerProps,
  type TooltipContentProps,
};
```

---

## Backgrounds <a name="backgrounds"></a>

### Bubble Background <a name="bubble"></a>

An interactive background featuring smoothly animated gradient bubbles, creating a playful, dynamic, and visually engaging backdrop.

- **Category**: Backgrounds
- **Registry Key**: `components-backgrounds-bubble`

#### Installation

```bash
npx shadcn@latest add components-backgrounds-bubble
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
<BubbleBackground />
```

#### Demo Code

##### File: `registry/demo/components/backgrounds/bubble/index.tsx`

```tsx
import { BubbleBackground } from '@/components/animate-ui/components/backgrounds/bubble';

type BubbleBackgroundDemoProps = {
  interactive: boolean;
};

export const BubbleBackgroundDemo = ({
  interactive,
}: BubbleBackgroundDemoProps) => {
  return (
    <BubbleBackground
      interactive={interactive}
      className="absolute inset-0 flex items-center justify-center rounded-xl"
    />
  );
};
```

#### Component Source Code

##### File: `registry/components/backgrounds/bubble/index.tsx`

```tsx
'use client';

import * as React from 'react';
import {
  motion,
  useMotionValue,
  useSpring,
  type SpringOptions,
} from 'motion/react';

import { cn } from '@/lib/utils';

type BubbleColors = {
  first: string;
  second: string;
  third: string;
  fourth: string;
  fifth: string;
  sixth: string;
};

type BubbleBackgroundProps = React.ComponentProps<'div'> & {
  interactive?: boolean;
  transition?: SpringOptions;
  colors?: BubbleColors;
};

function BubbleBackground({
  ref,
  className,
  children,
  interactive = false,
  transition = { stiffness: 100, damping: 20 },
  colors = {
    first: '18,113,255',
    second: '221,74,255',
    third: '0,220,255',
    fourth: '200,50,50',
    fifth: '180,180,50',
    sixth: '140,100,255',
  },
  ...props
}: BubbleBackgroundProps) {
  const containerRef = React.useRef<HTMLDivElement>(null);
  React.useImperativeHandle(ref, () => containerRef.current as HTMLDivElement);

  const mouseX = useMotionValue(0);
  const mouseY = useMotionValue(0);
  const springX = useSpring(mouseX, transition);
  const springY = useSpring(mouseY, transition);

  const rectRef = React.useRef<DOMRect | null>(null);
  const rafIdRef = React.useRef<number | null>(null);

  React.useLayoutEffect(() => {
    const updateRect = () => {
      if (containerRef.current) {
        rectRef.current = containerRef.current.getBoundingClientRect();
      }
    };

    updateRect();

    const el = containerRef.current;
    const ro = new ResizeObserver(updateRect);
    if (el) ro.observe(el);

    window.addEventListener('resize', updateRect);
    window.addEventListener('scroll', updateRect, { passive: true });

    return () => {
      ro.disconnect();
      window.removeEventListener('resize', updateRect);
      window.removeEventListener('scroll', updateRect);
    };
  }, []);

  React.useEffect(() => {
    if (!interactive) return;

    const el = containerRef.current;
    if (!el) return;

    const handleMouseMove = (e: MouseEvent) => {
      const rect = rectRef.current;
      if (!rect) return;
      const centerX = rect.left + rect.width / 2;
      const centerY = rect.top + rect.height / 2;

      if (rafIdRef.current != null) cancelAnimationFrame(rafIdRef.current);
      rafIdRef.current = requestAnimationFrame(() => {
        mouseX.set(e.clientX - centerX);
        mouseY.set(e.clientY - centerY);
      });
    };

    el.addEventListener('mousemove', handleMouseMove as EventListener, {
      passive: true,
    });
    return () => {
      el.removeEventListener('mousemove', handleMouseMove as EventListener);
      if (rafIdRef.current != null) cancelAnimationFrame(rafIdRef.current);
    };
  }, [interactive, mouseX, mouseY]);

  return (
    <div
      ref={containerRef}
      data-slot="bubble-background"
      className={cn(
        'relative size-full overflow-hidden bg-gradient-to-br from-violet-900 to-blue-900',
        className,
      )}
      {...props}
    >
      <style>
        {`
            :root {
              --first-color: ${colors.first};
              --second-color: ${colors.second};
              --third-color: ${colors.third};
              --fourth-color: ${colors.fourth};
              --fifth-color: ${colors.fifth};
              --sixth-color: ${colors.sixth};
            }
          `}
      </style>

      <svg
        xmlns="http://www.w3.org/2000/svg"
        className="absolute top-0 left-0 w-0 h-0"
      >
        <defs>
          <filter id="goo">
            <feGaussianBlur
              in="SourceGraphic"
              stdDeviation="16"
              result="blur"
            />
            <feColorMatrix
              in="blur"
              mode="matrix"
              values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -8"
              result="goo"
            />
            <feBlend in="SourceGraphic" in2="goo" />
          </filter>
        </defs>
      </svg>

      <div
        className="absolute inset-0"
        style={{ filter: 'url(#goo) blur(40px)' }}
      >
        <motion.div
          className="absolute rounded-full size-[80%] top-[10%] left-[10%] mix-blend-hard-light bg-[radial-gradient(circle_at_center,rgba(var(--first-color),0.8)_0%,rgba(var(--first-color),0)_50%)]"
          animate={{ y: [-50, 50, -50] }}
          transition={{ duration: 30, ease: 'easeInOut', repeat: Infinity }}
          style={{ transform: 'translateZ(0)', willChange: 'transform' }}
        />

        <motion.div
          className="absolute inset-0 flex justify-center items-center origin-[calc(50%-400px)]"
          animate={{ rotate: 360 }}
          transition={{
            duration: 20,
            ease: 'linear',
            repeat: Infinity,
            repeatType: 'loop',
          }}
          style={{ transform: 'translateZ(0)', willChange: 'transform' }}
        >
          <div className="rounded-full size-[80%] top-[10%] left-[10%] mix-blend-hard-light bg-[radial-gradient(circle_at_center,rgba(var(--second-color),0.8)_0%,rgba(var(--second-color),0)_50%)]" />
        </motion.div>

        <motion.div
          className="absolute inset-0 flex justify-center items-center origin-[calc(50%+400px)]"
          animate={{ rotate: 360 }}
          transition={{ duration: 40, ease: 'linear', repeat: Infinity }}
          style={{ transform: 'translateZ(0)', willChange: 'transform' }}
        >
          <div className="absolute rounded-full size-[80%] bg-[radial-gradient(circle_at_center,rgba(var(--third-color),0.8)_0%,rgba(var(--third-color),0)_50%)] mix-blend-hard-light top-[calc(50%+200px)] left-[calc(50%-500px)]" />
        </motion.div>

        <motion.div
          className="absolute rounded-full size-[80%] top-[10%] left-[10%] mix-blend-hard-light bg-[radial-gradient(circle_at_center,rgba(var(--fourth-color),0.8)_0%,rgba(var(--fourth-color),0)_50%)] opacity-70"
          animate={{ x: [-50, 50, -50] }}
          transition={{ duration: 40, ease: 'easeInOut', repeat: Infinity }}
          style={{ transform: 'translateZ(0)', willChange: 'transform' }}
        />

        <motion.div
          className="absolute inset-0 flex justify-center items-center origin-[calc(50%_-_800px)_calc(50%_+_200px)]"
          animate={{ rotate: 360 }}
          transition={{ duration: 20, ease: 'linear', repeat: Infinity }}
          style={{ transform: 'translateZ(0)', willChange: 'transform' }}
        >
          <div className="absolute rounded-full size-[160%] mix-blend-hard-light bg-[radial-gradient(circle_at_center,rgba(var(--fifth-color),0.8)_0%,rgba(var(--fifth-color),0)_50%)] top-[calc(50%-80%)] left-[calc(50%-80%)]" />
        </motion.div>

        {interactive && (
          <motion.div
            className="absolute rounded-full size-full mix-blend-hard-light bg-[radial-gradient(circle_at_center,rgba(var(--sixth-color),0.8)_0%,rgba(var(--sixth-color),0)_50%)] opacity-70"
            style={{
              x: springX,
              y: springY,
              transform: 'translateZ(0)',
              willChange: 'transform',
            }}
          />
        )}
      </div>

      {children}
    </div>
  );
}

export { BubbleBackground, type BubbleBackgroundProps };
```

---

### Fireworks Background <a name="fireworks"></a>

A background component that displays a fireworks animation.

- **Category**: Backgrounds
- **Registry Key**: `components-backgrounds-fireworks`

#### Installation

```bash
npx shadcn@latest add components-backgrounds-fireworks
```

#### How to Use

```tsx
<FireworksBackground />
```

#### Demo Code

##### File: `registry/demo/components/backgrounds/fireworks/index.tsx`

```tsx
'use client';

import { useTheme } from 'next-themes';
import { FireworksBackground } from '@/components/animate-ui/components/backgrounds/fireworks';

type FireworksBackgroundDemoProps = {
  population: number;
};

export default function FireworksBackgroundDemo({
  population,
}: FireworksBackgroundDemoProps) {
  const { resolvedTheme: theme } = useTheme();

  return (
    <FireworksBackground
      className="absolute inset-0 flex items-center justify-center rounded-xl"
      color={theme === 'dark' ? 'white' : 'black'}
      population={population}
    />
  );
}
```

#### Component Source Code

##### File: `registry/components/backgrounds/fireworks/index.tsx`

```tsx
'use client';

import * as React from 'react';

import { cn } from '@/lib/utils';

const rand = (min: number, max: number): number =>
  Math.random() * (max - min) + min;

const randInt = (min: number, max: number): number =>
  Math.floor(Math.random() * (max - min) + min);

const randColor = (): string => `hsl(${randInt(0, 360)}, 100%, 50%)`;

type ParticleType = {
  x: number;
  y: number;
  color: string;
  speed: number;
  direction: number;
  vx: number;
  vy: number;
  gravity: number;
  friction: number;
  alpha: number;
  decay: number;
  size: number;
  update: () => void;
  draw: (ctx: CanvasRenderingContext2D) => void;
  isAlive: () => boolean;
};

function createParticle(
  x: number,
  y: number,
  color: string,
  speed: number,
  direction: number,
  gravity: number,
  friction: number,
  size: number,
): ParticleType {
  const vx = Math.cos(direction) * speed;
  const vy = Math.sin(direction) * speed;
  const alpha = 1;
  const decay = rand(0.005, 0.02);

  return {
    x,
    y,
    color,
    speed,
    direction,
    vx,
    vy,
    gravity,
    friction,
    alpha,
    decay,
    size,
    update() {
      this.vx *= this.friction;
      this.vy *= this.friction;
      this.vy += this.gravity;
      this.x += this.vx;
      this.y += this.vy;
      this.alpha -= this.decay;
    },
    draw(ctx: CanvasRenderingContext2D) {
      ctx.save();
      ctx.globalAlpha = this.alpha;
      ctx.beginPath();
      ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
      ctx.fillStyle = this.color;
      ctx.fill();
      ctx.restore();
    },
    isAlive() {
      return this.alpha > 0;
    },
  };
}

type FireworkType = {
  x: number;
  y: number;
  targetY: number;
  color: string;
  speed: number;
  size: number;
  angle: number;
  vx: number;
  vy: number;
  trail: { x: number; y: number }[];
  trailLength: number;
  exploded: boolean;
  update: () => boolean;
  explode: () => void;
  draw: (ctx: CanvasRenderingContext2D) => void;
};

function createFirework(
  x: number,
  y: number,
  targetY: number,
  color: string,
  speed: number,
  size: number,
  particleSpeed: { min: number; max: number } | number,
  particleSize: { min: number; max: number } | number,
  onExplode: (particles: ParticleType[]) => void,
): FireworkType {
  const angle = -Math.PI / 2 + rand(-0.3, 0.3);
  const vx = Math.cos(angle) * speed;
  const vy = Math.sin(angle) * speed;
  const trail: { x: number; y: number }[] = [];
  const trailLength = randInt(10, 25);

  return {
    x,
    y,
    targetY,
    color,
    speed,
    size,
    angle,
    vx,
    vy,
    trail,
    trailLength,
    exploded: false,
    update() {
      this.trail.push({ x: this.x, y: this.y });
      if (this.trail.length > this.trailLength) {
        this.trail.shift();
      }
      this.x += this.vx;
      this.y += this.vy;
      this.vy += 0.02;
      if (this.vy >= 0 || this.y <= this.targetY) {
        this.explode();
        return false;
      }
      return true;
    },
    explode() {
      const numParticles = randInt(50, 150);
      const particles: ParticleType[] = [];
      for (let i = 0; i < numParticles; i++) {
        const particleAngle = rand(0, Math.PI * 2);
        const localParticleSpeed = getValueByRange(particleSpeed);
        const localParticleSize = getValueByRange(particleSize);
        particles.push(
          createParticle(
            this.x,
            this.y,
            this.color,
            localParticleSpeed,
            particleAngle,
            0.05,
            0.98,
            localParticleSize,
          ),
        );
      }
      onExplode(particles);
    },
    draw(ctx: CanvasRenderingContext2D) {
      ctx.save();
      ctx.beginPath();
      if (this.trail.length > 1) {
        ctx.moveTo(this.trail[0]?.x ?? this.x, this.trail[0]?.y ?? this.y);
        for (const point of this.trail) {
          ctx.lineTo(point.x, point.y);
        }
      } else {
        ctx.moveTo(this.x, this.y);
        ctx.lineTo(this.x, this.y);
      }
      ctx.strokeStyle = this.color;
      ctx.lineWidth = this.size;
      ctx.lineCap = 'round';
      ctx.stroke();
      ctx.restore();
    },
  };
}

function getValueByRange(range: { min: number; max: number } | number): number {
  if (typeof range === 'number') {
    return range;
  }
  return rand(range.min, range.max);
}

function getColor(color: string | string[] | undefined): string {
  if (Array.isArray(color)) {
    return color[randInt(0, color.length)] ?? randColor();
  }
  return color ?? randColor();
}

type FireworksBackgroundProps = Omit<React.ComponentProps<'div'>, 'color'> & {
  canvasProps?: React.ComponentProps<'canvas'>;
  population?: number;
  color?: string | string[];
  fireworkSpeed?: { min: number; max: number } | number;
  fireworkSize?: { min: number; max: number } | number;
  particleSpeed?: { min: number; max: number } | number;
  particleSize?: { min: number; max: number } | number;
};

function FireworksBackground({
  ref,
  className,
  canvasProps,
  population = 1,
  color,
  fireworkSpeed = { min: 4, max: 8 },
  fireworkSize = { min: 2, max: 5 },
  particleSpeed = { min: 2, max: 7 },
  particleSize = { min: 1, max: 5 },
  ...props
}: FireworksBackgroundProps) {
  const canvasRef = React.useRef<HTMLCanvasElement>(null);
  const containerRef = React.useRef<HTMLDivElement>(null);
  React.useImperativeHandle(ref, () => containerRef.current as HTMLDivElement);

  React.useEffect(() => {
    const canvas = canvasRef.current;
    const container = containerRef.current;
    if (!canvas || !container) return;
    const ctx = canvas.getContext('2d');
    if (!ctx) return;

    let maxX = window.innerWidth;
    let ratio = container.offsetHeight / container.offsetWidth;
    let maxY = maxX * ratio;
    canvas.width = maxX;
    canvas.height = maxY;

    const setCanvasSize = () => {
      maxX = window.innerWidth;
      ratio = container.offsetHeight / container.offsetWidth;
      maxY = maxX * ratio;
      canvas.width = maxX;
      canvas.height = maxY;
    };
    window.addEventListener('resize', setCanvasSize);

    const explosions: ParticleType[] = [];
    const fireworks: FireworkType[] = [];

    const handleExplosion = (particles: ParticleType[]) => {
      explosions.push(...particles);
    };

    const launchFirework = () => {
      const x = rand(maxX * 0.1, maxX * 0.9);
      const y = maxY;
      const targetY = rand(maxY * 0.1, maxY * 0.4);
      const fireworkColor = getColor(color);
      const speed = getValueByRange(fireworkSpeed);
      const size = getValueByRange(fireworkSize);
      fireworks.push(
        createFirework(
          x,
          y,
          targetY,
          fireworkColor,
          speed,
          size,
          particleSpeed,
          particleSize,
          handleExplosion,
        ),
      );
      const timeout = rand(300, 800) / population;
      setTimeout(launchFirework, timeout);
    };

    launchFirework();

    let animationFrameId: number;
    const animate = () => {
      ctx.clearRect(0, 0, maxX, maxY);

      for (let i = fireworks.length - 1; i >= 0; i--) {
        const firework = fireworks[i];
        if (!firework?.update()) {
          fireworks.splice(i, 1);
        } else {
          firework.draw(ctx);
        }
      }

      for (let i = explosions.length - 1; i >= 0; i--) {
        const particle = explosions[i];
        particle?.update();
        if (particle?.isAlive()) {
          particle.draw(ctx);
        } else {
          explosions.splice(i, 1);
        }
      }

      animationFrameId = requestAnimationFrame(animate);
    };

    animate();

    const handleClick = (event: MouseEvent) => {
      const x = event.clientX;
      const y = maxY;
      const targetY = event.clientY;
      const fireworkColor = getColor(color);
      const speed = getValueByRange(fireworkSpeed);
      const size = getValueByRange(fireworkSize);
      fireworks.push(
        createFirework(
          x,
          y,
          targetY,
          fireworkColor,
          speed,
          size,
          particleSpeed,
          particleSize,
          handleExplosion,
        ),
      );
    };

    container.addEventListener('click', handleClick);

    return () => {
      window.removeEventListener('resize', setCanvasSize);
      container.removeEventListener('click', handleClick);
      cancelAnimationFrame(animationFrameId);
    };
  }, [
    population,
    color,
    fireworkSpeed,
    fireworkSize,
    particleSpeed,
    particleSize,
  ]);

  return (
    <div
      ref={containerRef}
      data-slot="fireworks-background"
      className={cn('relative size-full overflow-hidden', className)}
      {...props}
    >
      <canvas
        {...canvasProps}
        ref={canvasRef}
        className={cn('absolute inset-0 size-full', canvasProps?.className)}
      />
    </div>
  );
}

export { FireworksBackground, type FireworksBackgroundProps };
```

---

### Gradient Background <a name="gradient"></a>

A background component featuring a subtle yet engaging animated gradient effect, smoothly transitioning colors to enhance visual depth.

- **Category**: Backgrounds
- **Registry Key**: `components-backgrounds-gradient`

#### Installation

```bash
npx shadcn@latest add components-backgrounds-gradient
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
<GradientBackground />
```

#### Demo Code

##### File: `registry/demo/components/backgrounds/gradient/index.tsx`

```tsx
import { GradientBackground } from '@/components/animate-ui/components/backgrounds/gradient';

export const GradientBackgroundDemo = () => {
  return (
    <GradientBackground className="absolute inset-0 flex items-center justify-center rounded-xl" />
  );
};
```

#### Component Source Code

##### File: `registry/components/backgrounds/gradient/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { motion, type HTMLMotionProps } from 'motion/react';

import { cn } from '@/lib/utils';

type GradientBackgroundProps = HTMLMotionProps<'div'>;

function GradientBackground({
  className,
  transition = { duration: 15, ease: 'easeInOut', repeat: Infinity },
  ...props
}: GradientBackgroundProps) {
  return (
    <motion.div
      data-slot="gradient-background"
      className={cn(
        'size-full bg-gradient-to-br from-blue-500 via-purple-500 to-pink-500 bg-[length:400%_400%]',
        className,
      )}
      animate={{ backgroundPosition: ['0% 50%', '100% 50%', '0% 50%'] }}
      transition={transition}
      {...props}
    />
  );
}

export { GradientBackground, type GradientBackgroundProps };
```

---

### Gravity Stars Background <a name="gravity-stars"></a>

A background component featuring an interactive gravity stars effect.

- **Category**: Backgrounds
- **Registry Key**: `components-backgrounds-gravity-stars`

#### Installation

```bash
npx shadcn@latest add components-backgrounds-gravity-stars
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
<GravityStarsBackground />
```

#### Demo Code

##### File: `registry/demo/components/backgrounds/gravity-stars/index.tsx`

```tsx
import { GravityStarsBackground } from '@/components/animate-ui/components/backgrounds/gravity-stars';

export const GravityStarsBackgroundDemo = () => {
  return (
    <GravityStarsBackground className="absolute inset-0 flex items-center justify-center rounded-xl" />
  );
};
```

#### Component Source Code

##### File: `registry/components/backgrounds/gravity-stars/index.tsx`

```tsx
'use client';

import * as React from 'react';

import { cn } from '@/lib/utils';

type MouseGravity = 'attract' | 'repel';
type GlowAnimation = 'instant' | 'ease' | 'spring';
type StarsInteractionType = 'bounce' | 'merge';

type GravityStarsProps = {
  starsCount?: number;
  starsSize?: number;
  starsOpacity?: number;
  glowIntensity?: number;
  glowAnimation?: GlowAnimation;
  movementSpeed?: number;
  mouseInfluence?: number;
  mouseGravity?: MouseGravity;
  gravityStrength?: number;
  starsInteraction?: boolean;
  starsInteractionType?: StarsInteractionType;
} & React.ComponentProps<'div'>;

type Particle = {
  x: number;
  y: number;
  vx: number;
  vy: number;
  size: number;
  opacity: number;
  baseOpacity: number;
  mass: number;
  glowMultiplier?: number;
  glowVelocity?: number;
};

function GravityStarsBackground({
  starsCount = 75,
  starsSize = 2,
  starsOpacity = 0.75,
  glowIntensity = 15,
  glowAnimation = 'ease',
  movementSpeed = 0.3,
  mouseInfluence = 100,
  mouseGravity = 'attract',
  gravityStrength = 75,
  starsInteraction = false,
  starsInteractionType = 'bounce',
  className,
  ...props
}: GravityStarsProps) {
  const containerRef = React.useRef<HTMLDivElement | null>(null);
  const canvasRef = React.useRef<HTMLCanvasElement | null>(null);
  const animRef = React.useRef<number | null>(null);
  const starsRef = React.useRef<Particle[]>([]);
  const mouseRef = React.useRef<{ x: number; y: number }>({ x: 0, y: 0 });
  const [dpr, setDpr] = React.useState(1);
  const [canvasSize, setCanvasSize] = React.useState({
    width: 800,
    height: 600,
  });

  const readColor = React.useCallback(() => {
    const el = containerRef.current;
    if (!el) return '#ffffff';
    const cs = getComputedStyle(el);
    return cs.color || '#ffffff';
  }, []);

  const initStars = React.useCallback(
    (w: number, h: number) => {
      starsRef.current = Array.from({ length: starsCount }).map(() => {
        const angle = Math.random() * Math.PI * 2;
        const speed = movementSpeed * (0.5 + Math.random() * 0.5);
        return {
          x: Math.random() * w,
          y: Math.random() * h,
          vx: Math.cos(angle) * speed,
          vy: Math.sin(angle) * speed,
          size: Math.random() * starsSize + 1,
          opacity: starsOpacity,
          baseOpacity: starsOpacity,
          mass: Math.random() * 0.5 + 0.5,
          glowMultiplier: 1,
          glowVelocity: 0,
        };
      });
    },
    [starsCount, movementSpeed, starsOpacity, starsSize],
  );

  const redistributeStars = React.useCallback((w: number, h: number) => {
    starsRef.current.forEach((p) => {
      p.x = Math.random() * w;
      p.y = Math.random() * h;
    });
  }, []);

  const resizeCanvas = React.useCallback(() => {
    const canvas = canvasRef.current;
    const container = containerRef.current;
    if (!canvas || !container) return;
    const rect = container.getBoundingClientRect();
    const nextDpr = Math.max(1, Math.min(window.devicePixelRatio || 1, 2));
    setDpr(nextDpr);
    canvas.width = Math.max(1, Math.floor(rect.width * nextDpr));
    canvas.height = Math.max(1, Math.floor(rect.height * nextDpr));
    canvas.style.width = `${rect.width}px`;
    canvas.style.height = `${rect.height}px`;
    setCanvasSize({ width: rect.width, height: rect.height });
    if (starsRef.current.length === 0) {
      initStars(rect.width, rect.height);
    } else {
      redistributeStars(rect.width, rect.height);
    }
  }, [initStars, redistributeStars]);

  const handlePointerMove = React.useCallback(
    (e: React.MouseEvent | React.TouchEvent) => {
      const canvas = canvasRef.current;
      if (!canvas) return;
      const rect = canvas.getBoundingClientRect();
      let clientX = 0;
      let clientY = 0;
      if ('touches' in e) {
        const t = e.touches[0];
        if (!t) return;
        clientX = t.clientX;
        clientY = t.clientY;
      } else {
        clientX = e.clientX;
        clientY = e.clientY;
      }
      mouseRef.current = { x: clientX - rect.left, y: clientY - rect.top };
    },
    [],
  );

  const updateStars = React.useCallback(() => {
    const w = canvasSize.width;
    const h = canvasSize.height;
    const mouse = mouseRef.current;

    for (let i = 0; i < starsRef.current.length; i++) {
      const p = starsRef.current[i];

      const dx = mouse.x - p.x;
      const dy = mouse.y - p.y;
      const dist = Math.hypot(dx, dy);

      if (dist < mouseInfluence && dist > 0) {
        const force = (mouseInfluence - dist) / mouseInfluence;
        const nx = dx / dist;
        const ny = dy / dist;
        const g = force * (gravityStrength * 0.001);

        if (mouseGravity === 'attract') {
          p.vx += nx * g;
          p.vy += ny * g;
        } else if (mouseGravity === 'repel') {
          p.vx -= nx * g;
          p.vy -= ny * g;
        }

        p.opacity = Math.min(1, p.baseOpacity + force * 0.4);

        const targetGlow = 1 + force * 2;
        const currentGlow = p.glowMultiplier || 1;

        if (glowAnimation === 'instant') {
          p.glowMultiplier = targetGlow;
        } else if (glowAnimation === 'ease') {
          const ease = 0.15;
          p.glowMultiplier = currentGlow + (targetGlow - currentGlow) * ease;
        } else {
          const spring = (targetGlow - currentGlow) * 0.2;
          const damping = 0.85;
          p.glowVelocity = (p.glowVelocity || 0) * damping + spring;
          p.glowMultiplier = currentGlow + (p.glowVelocity || 0);
        }
      } else {
        p.opacity = Math.max(p.baseOpacity * 0.3, p.opacity - 0.02);
        const targetGlow = 1;
        const currentGlow = p.glowMultiplier || 1;
        if (glowAnimation === 'instant') {
          p.glowMultiplier = targetGlow;
        } else if (glowAnimation === 'ease') {
          const ease = 0.08;
          p.glowMultiplier = Math.max(
            1,
            currentGlow + (targetGlow - currentGlow) * ease,
          );
        } else {
          const spring = (targetGlow - currentGlow) * 0.15;
          const damping = 0.9;
          p.glowVelocity = (p.glowVelocity || 0) * damping + spring;
          p.glowMultiplier = Math.max(1, currentGlow + (p.glowVelocity || 0));
        }
      }

      if (starsInteraction) {
        for (let j = i + 1; j < starsRef.current.length; j++) {
          const o = starsRef.current[j];
          const dx2 = o.x - p.x;
          const dy2 = o.y - p.y;
          const d = Math.hypot(dx2, dy2);
          const minD = p.size + o.size + 5;
          if (d < minD && d > 0) {
            if (starsInteractionType === 'bounce') {
              const nx = dx2 / d;
              const ny = dy2 / d;
              const rvx = p.vx - o.vx;
              const rvy = p.vy - o.vy;
              const speed = rvx * nx + rvy * ny;
              if (speed < 0) continue;
              const impulse = (2 * speed) / (p.mass + o.mass);
              p.vx -= impulse * o.mass * nx;
              p.vy -= impulse * o.mass * ny;
              o.vx += impulse * p.mass * nx;
              o.vy += impulse * p.mass * ny;
              const overlap = minD - d;
              const sx = nx * overlap * 0.5;
              const sy = ny * overlap * 0.5;
              p.x -= sx;
              p.y -= sy;
              o.x += sx;
              o.y += sy;
            } else {
              const mergeForce = (minD - d) / minD;
              p.glowMultiplier = (p.glowMultiplier || 1) + mergeForce * 0.5;
              o.glowMultiplier = (o.glowMultiplier || 1) + mergeForce * 0.5;
              const af = mergeForce * 0.01;
              p.vx += dx2 * af;
              p.vy += dy2 * af;
              o.vx -= dx2 * af;
              o.vy -= dy2 * af;
            }
          }
        }
      }

      p.x += p.vx;
      p.y += p.vy;

      p.vx += (Math.random() - 0.5) * 0.001;
      p.vy += (Math.random() - 0.5) * 0.001;

      p.vx *= 0.999;
      p.vy *= 0.999;

      if (p.x < 0) p.x = w;
      if (p.x > w) p.x = 0;
      if (p.y < 0) p.y = h;
      if (p.y > h) p.y = 0;
    }
  }, [
    canvasSize.width,
    canvasSize.height,
    mouseInfluence,
    mouseGravity,
    gravityStrength,
    glowAnimation,
    starsInteraction,
    starsInteractionType,
  ]);

  const drawStars = React.useCallback(
    (ctx: CanvasRenderingContext2D) => {
      ctx.clearRect(0, 0, ctx.canvas.width, ctx.canvas.height);
      const color = readColor();
      for (const p of starsRef.current) {
        ctx.save();
        ctx.shadowColor = color;
        ctx.shadowBlur = glowIntensity * (p.glowMultiplier || 1) * 2;
        ctx.globalAlpha = p.opacity;
        ctx.fillStyle = color;
        ctx.beginPath();
        ctx.arc(p.x * dpr, p.y * dpr, p.size * dpr, 0, Math.PI * 2);
        ctx.fill();
        ctx.restore();
      }
    },
    [dpr, glowIntensity, readColor],
  );

  const animate = React.useCallback(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const ctx = canvas.getContext('2d');
    if (!ctx) return;
    updateStars();
    drawStars(ctx);
    animRef.current = requestAnimationFrame(animate);
  }, [updateStars, drawStars]);

  React.useEffect(() => {
    resizeCanvas();
    const container = containerRef.current;
    const ro =
      typeof ResizeObserver !== 'undefined'
        ? new ResizeObserver(resizeCanvas)
        : null;
    if (container && ro) ro.observe(container);
    const onResize = () => resizeCanvas();
    window.addEventListener('resize', onResize);
    return () => {
      window.removeEventListener('resize', onResize);
      if (ro && container) ro.disconnect();
    };
  }, [resizeCanvas]);

  React.useEffect(() => {
    if (starsRef.current.length === 0) {
      initStars(canvasSize.width, canvasSize.height);
    } else {
      starsRef.current.forEach((p) => {
        p.baseOpacity = starsOpacity;
        p.opacity = starsOpacity;
        const spd = Math.hypot(p.vx, p.vy);
        if (spd > 0) {
          const ratio = movementSpeed / spd;
          p.vx *= ratio;
          p.vy *= ratio;
        }
      });
    }
  }, [
    starsCount,
    starsOpacity,
    movementSpeed,
    canvasSize.width,
    canvasSize.height,
    initStars,
  ]);

  React.useEffect(() => {
    if (animRef.current) cancelAnimationFrame(animRef.current);
    animRef.current = requestAnimationFrame(animate);
    return () => {
      if (animRef.current) cancelAnimationFrame(animRef.current);
      animRef.current = null;
    };
  }, [animate]);

  return (
    <div
      ref={containerRef}
      data-slot="gravity-stars-background"
      className={cn('relative size-full overflow-hidden', className)}
      onMouseMove={(e) => handlePointerMove(e)}
      onTouchMove={(e) => handlePointerMove(e)}
      {...props}
    >
      <canvas ref={canvasRef} className="block w-full h-full" />
    </div>
  );
}

export { GravityStarsBackground, type GravityStarsProps };
```

---

### Hexagon Background <a name="hexagon"></a>

A background component featuring an interactive hexagon grid.

- **Category**: Backgrounds
- **Registry Key**: `components-backgrounds-hexagon`

#### Installation

```bash
npx shadcn@latest add components-backgrounds-hexagon
```

#### How to Use

```tsx
<HexagonBackground />
```

#### Demo Code

##### File: `registry/demo/components/backgrounds/hexagon/index.tsx`

```tsx
import { HexagonBackground } from '@/components/animate-ui/components/backgrounds/hexagon';

export const HexagonBackgroundDemo = () => {
  return (
    <HexagonBackground className="absolute inset-0 flex items-center justify-center rounded-xl" />
  );
};
```

#### Component Source Code

##### File: `registry/components/backgrounds/hexagon/index.tsx`

```tsx
'use client';

import * as React from 'react';

import { cn } from '@/lib/utils';

type HexagonBackgroundProps = React.ComponentProps<'div'> & {
  hexagonProps?: React.ComponentProps<'div'>;
  hexagonSize?: number; // value greater than 50
  hexagonMargin?: number;
};

function HexagonBackground({
  className,
  children,
  hexagonProps,
  hexagonSize = 75,
  hexagonMargin = 3,
  ...props
}: HexagonBackgroundProps) {
  const hexagonWidth = hexagonSize;
  const hexagonHeight = hexagonSize * 1.1;
  const rowSpacing = hexagonSize * 0.8;
  const baseMarginTop = -36 - 0.275 * (hexagonSize - 100);
  const computedMarginTop = baseMarginTop + hexagonMargin;
  const oddRowMarginLeft = -(hexagonSize / 2);
  const evenRowMarginLeft = hexagonMargin / 2;

  const [gridDimensions, setGridDimensions] = React.useState({
    rows: 0,
    columns: 0,
  });

  const updateGridDimensions = React.useCallback(() => {
    const rows = Math.ceil(window.innerHeight / rowSpacing);
    const columns = Math.ceil(window.innerWidth / hexagonWidth) + 1;
    setGridDimensions({ rows, columns });
  }, [rowSpacing, hexagonWidth]);

  React.useEffect(() => {
    updateGridDimensions();
    window.addEventListener('resize', updateGridDimensions);
    return () => window.removeEventListener('resize', updateGridDimensions);
  }, [updateGridDimensions]);

  return (
    <div
      data-slot="hexagon-background"
      className={cn(
        'relative size-full overflow-hidden dark:bg-neutral-900 bg-neutral-100',
        className,
      )}
      {...props}
    >
      <style>{`:root { --hexagon-margin: ${hexagonMargin}px; }`}</style>
      <div className="absolute top-0 -left-0 size-full overflow-hidden">
        {Array.from({ length: gridDimensions.rows }).map((_, rowIndex) => (
          <div
            key={`row-${rowIndex}`}
            style={{
              marginTop: computedMarginTop,
              marginLeft:
                ((rowIndex + 1) % 2 === 0
                  ? evenRowMarginLeft
                  : oddRowMarginLeft) - 10,
            }}
            className="inline-flex"
          >
            {Array.from({ length: gridDimensions.columns }).map(
              (_, colIndex) => (
                <div
                  key={`hexagon-${rowIndex}-${colIndex}`}
                  {...hexagonProps}
                  style={{
                    width: hexagonWidth,
                    height: hexagonHeight,
                    marginLeft: hexagonMargin,
                    ...hexagonProps?.style,
                  }}
                  className={cn(
                    'relative',
                    '[clip-path:polygon(50%_0%,_100%_25%,_100%_75%,_50%_100%,_0%_75%,_0%_25%)]',
                    "before:content-[''] before:absolute before:top-0 before:left-0 before:w-full before:h-full dark:before:bg-neutral-950 before:bg-white before:opacity-100 before:transition-all before:duration-1000",
                    "after:content-[''] after:absolute after:inset-[var(--hexagon-margin)] dark:after:bg-neutral-950 after:bg-white",
                    'after:[clip-path:polygon(50%_0%,_100%_25%,_100%_75%,_50%_100%,_0%_75%,_0%_25%)]',
                    'hover:before:bg-neutral-200 dark:hover:before:bg-neutral-800 hover:before:opacity-100 hover:before:duration-0 dark:hover:after:bg-neutral-900 hover:after:bg-neutral-100 hover:after:opacity-100 hover:after:duration-0',
                    hexagonProps?.className,
                  )}
                />
              ),
            )}
          </div>
        ))}
      </div>
      {children}
    </div>
  );
}

export { HexagonBackground, type HexagonBackgroundProps };
```

---

### Hole Background <a name="hole"></a>

A background component featuring an animated hole grid.

- **Category**: Backgrounds
- **Registry Key**: `components-backgrounds-hole`

#### Installation

```bash
npx shadcn@latest add components-backgrounds-hole
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
<HoleBackground />
```

#### Demo Code

##### File: `registry/demo/components/backgrounds/hole/index.tsx`

```tsx
import { HoleBackground } from '@/components/animate-ui/components/backgrounds/hole';

export const HoleBackgroundDemo = () => {
  return (
    <HoleBackground className="absolute inset-0 flex items-center justify-center rounded-xl" />
  );
};
```

#### Component Source Code

##### File: `registry/components/backgrounds/hole/index.tsx`

```tsx
/* eslint-disable @typescript-eslint/no-explicit-any */

'use client';

import * as React from 'react';
import { motion } from 'motion/react';

import { cn } from '@/lib/utils';

type HoleBackgroundProps = React.ComponentProps<'div'> & {
  strokeColor?: string;
  numberOfLines?: number;
  numberOfDiscs?: number;
  particleRGBColor?: [number, number, number];
};

function HoleBackground({
  strokeColor = '#737373',
  numberOfLines = 50,
  numberOfDiscs = 50,
  particleRGBColor = [255, 255, 255],
  className,
  children,
  ...props
}: HoleBackgroundProps) {
  const canvasRef = React.useRef<HTMLCanvasElement>(null);
  const animationFrameIdRef = React.useRef<number>(0);
  const stateRef = React.useRef<any>({
    discs: [] [],
    lines: [] [],
    particles: [] [],
    clip: {},
    startDisc: {},
    endDisc: {},
    rect: { width: 0, height: 0 },
    render: { width: 0, height: 0, dpi: 1 },
    particleArea: {},
    linesCanvas: null,
  });

  const linear = (p: number) => p;
  const easeInExpo = (p: number) => (p === 0 ? 0 : Math.pow(2, 10 * (p - 1)));

  const tweenValue = React.useCallback(
    (start: number, end: number, p: number, ease: 'inExpo' | null = null) => {
      const delta = end - start;
      const easeFn = ease === 'inExpo' ? easeInExpo : linear;
      return start + delta * easeFn(p);
    },
    [],
  );

  const tweenDisc = React.useCallback(
    (disc: any) => {
      const { startDisc, endDisc } = stateRef.current;
      disc.x = tweenValue(startDisc.x, endDisc.x, disc.p);
      disc.y = tweenValue(startDisc.y, endDisc.y, disc.p, 'inExpo');
      disc.w = tweenValue(startDisc.w, endDisc.w, disc.p);
      disc.h = tweenValue(startDisc.h, endDisc.h, disc.p);
    },
    [tweenValue],
  );

  const setSize = React.useCallback(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const rect = canvas.getBoundingClientRect();
    stateRef.current.rect = { width: rect.width, height: rect.height };
    stateRef.current.render = {
      width: rect.width,
      height: rect.height,
      dpi: window.devicePixelRatio || 1,
    };
    canvas.width = stateRef.current.render.width * stateRef.current.render.dpi;
    canvas.height =
      stateRef.current.render.height * stateRef.current.render.dpi;
  }, []);

  const setDiscs = React.useCallback(() => {
    const { width, height } = stateRef.current.rect;
    stateRef.current.discs = [];
    stateRef.current.startDisc = {
      x: width * 0.5,
      y: height * 0.45,
      w: width * 0.75,
      h: height * 0.7,
    };
    stateRef.current.endDisc = {
      x: width * 0.5,
      y: height * 0.95,
      w: 0,
      h: 0,
    };
    let prevBottom = height;
    stateRef.current.clip = {};
    for (let i = 0; i < numberOfDiscs; i++) {
      const p = i / numberOfDiscs;
      const disc = { p, x: 0, y: 0, w: 0, h: 0 };
      tweenDisc(disc);
      const bottom = disc.y + disc.h;
      if (bottom <= prevBottom) {
        stateRef.current.clip = { disc: { ...disc }, i };
      }
      prevBottom = bottom;
      stateRef.current.discs.push(disc);
    }
    const clipPath = new Path2D();
    const disc = stateRef.current.clip.disc;
    clipPath.ellipse(disc.x, disc.y, disc.w, disc.h, 0, 0, Math.PI * 2);
    clipPath.rect(disc.x - disc.w, 0, disc.w * 2, disc.y);
    stateRef.current.clip.path = clipPath;
  }, [numberOfDiscs, tweenDisc]);

  const setLines = React.useCallback(() => {
    const { width, height } = stateRef.current.rect;
    stateRef.current.lines = [];
    const linesAngle = (Math.PI * 2) / numberOfLines;
    for (let i = 0; i < numberOfLines; i++) {
      stateRef.current.lines.push([]);
    }
    stateRef.current.discs.forEach((disc: any) => {
      for (let i = 0; i < numberOfLines; i++) {
        const angle = i * linesAngle;
        const p = {
          x: disc.x + Math.cos(angle) * disc.w,
          y: disc.y + Math.sin(angle) * disc.h,
        };
        stateRef.current.lines[i].push(p);
      }
    });
    const offCanvas = document.createElement('canvas');
    offCanvas.width = width;
    offCanvas.height = height;
    const ctx = offCanvas.getContext('2d');
    if (!ctx) return;
    stateRef.current.lines.forEach((line: any) => {
      ctx.save();
      let lineIsIn = false;
      line.forEach((p1: any, j: number) => {
        if (j === 0) return;
        const p0 = line[j - 1];
        if (
          !lineIsIn &&
          (ctx.isPointInPath(stateRef.current.clip.path, p1.x, p1.y) ||
            ctx.isPointInStroke(stateRef.current.clip.path, p1.x, p1.y))
        ) {
          lineIsIn = true;
        } else if (lineIsIn) {
          ctx.clip(stateRef.current.clip.path);
        }
        ctx.beginPath();
        ctx.moveTo(p0.x, p0.y);
        ctx.lineTo(p1.x, p1.y);
        ctx.strokeStyle = strokeColor;
        ctx.lineWidth = 2;
        ctx.stroke();
        ctx.closePath();
      });
      ctx.restore();
    });
    stateRef.current.linesCanvas = offCanvas;
  }, [numberOfLines, strokeColor]);

  const initParticle = React.useCallback(
    (start: boolean = false) => {
      const sx =
        stateRef.current.particleArea.sx +
        stateRef.current.particleArea.sw * Math.random();
      const ex =
        stateRef.current.particleArea.ex +
        stateRef.current.particleArea.ew * Math.random();
      const dx = ex - sx;
      const y = start
        ? stateRef.current.particleArea.h * Math.random()
        : stateRef.current.particleArea.h;
      const r = 0.5 + Math.random() * 4;
      const vy = 0.5 + Math.random();
      return {
        x: sx,
        sx,
        dx,
        y,
        vy,
        p: 0,
        r,
        c: `rgba(${particleRGBColor[0]}, ${particleRGBColor[1]}, ${particleRGBColor[2]}, ${Math.random()})`,
      };
    },
    [particleRGBColor],
  );

  const setParticles = React.useCallback(() => {
    const { width, height } = stateRef.current.rect;
    stateRef.current.particles = [];
    const disc = stateRef.current.clip.disc;
    stateRef.current.particleArea = {
      sw: disc.w * 0.5,
      ew: disc.w * 2,
      h: height * 0.85,
    };
    stateRef.current.particleArea.sx =
      (width - stateRef.current.particleArea.sw) / 2;
    stateRef.current.particleArea.ex =
      (width - stateRef.current.particleArea.ew) / 2;
    const totalParticles = 100;
    for (let i = 0; i < totalParticles; i++) {
      stateRef.current.particles.push(initParticle(true));
    }
  }, [initParticle]);

  const drawDiscs = React.useCallback(
    (ctx: CanvasRenderingContext2D) => {
      ctx.strokeStyle = strokeColor;
      ctx.lineWidth = 2;
      const outerDisc = stateRef.current.startDisc;
      ctx.beginPath();
      ctx.ellipse(
        outerDisc.x,
        outerDisc.y,
        outerDisc.w,
        outerDisc.h,
        0,
        0,
        Math.PI * 2,
      );
      ctx.stroke();
      ctx.closePath();
      stateRef.current.discs.forEach((disc: any, i: number) => {
        if (i % 5 !== 0) return;
        if (disc.w < stateRef.current.clip.disc.w - 5) {
          ctx.save();
          ctx.clip(stateRef.current.clip.path);
        }
        ctx.beginPath();
        ctx.ellipse(disc.x, disc.y, disc.w, disc.h, 0, 0, Math.PI * 2);
        ctx.stroke();
        ctx.closePath();
        if (disc.w < stateRef.current.clip.disc.w - 5) {
          ctx.restore();
        }
      });
    },
    [strokeColor],
  );

  const drawLines = React.useCallback((ctx: CanvasRenderingContext2D) => {
    if (stateRef.current.linesCanvas) {
      ctx.drawImage(stateRef.current.linesCanvas, 0, 0);
    }
  }, []);

  const drawParticles = React.useCallback((ctx: CanvasRenderingContext2D) => {
    ctx.save();
    ctx.clip(stateRef.current.clip.path);
    stateRef.current.particles.forEach((particle: any) => {
      ctx.fillStyle = particle.c;
      ctx.beginPath();
      ctx.rect(particle.x, particle.y, particle.r, particle.r);
      ctx.closePath();
      ctx.fill();
    });
    ctx.restore();
  }, []);

  const moveDiscs = React.useCallback(() => {
    stateRef.current.discs.forEach((disc: any) => {
      disc.p = (disc.p + 0.001) % 1;
      tweenDisc(disc);
    });
  }, [tweenDisc]);

  const moveParticles = React.useCallback(() => {
    stateRef.current.particles.forEach((particle: any, idx: number) => {
      particle.p = 1 - particle.y / stateRef.current.particleArea.h;
      particle.x = particle.sx + particle.dx * particle.p;
      particle.y -= particle.vy;
      if (particle.y < 0) {
        stateRef.current.particles[idx] = initParticle();
      }
    });
  }, [initParticle]);

  const tick = React.useCallback(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const ctx = canvas.getContext('2d');
    if (!ctx) return;
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.save();
    ctx.scale(stateRef.current.render.dpi, stateRef.current.render.dpi);
    moveDiscs();
    moveParticles();
    drawDiscs(ctx);
    drawLines(ctx);
    drawParticles(ctx);
    ctx.restore();
    animationFrameIdRef.current = requestAnimationFrame(tick);
  }, [moveDiscs, moveParticles, drawDiscs, drawLines, drawParticles]);

  const init = React.useCallback(() => {
    setSize();
    setDiscs();
    setLines();
    setParticles();
  }, [setSize, setDiscs, setLines, setParticles]);

  React.useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    init();
    tick();
    const handleResize = () => {
      setSize();
      setDiscs();
      setLines();
      setParticles();
    };
    window.addEventListener('resize', handleResize);
    return () => {
      window.removeEventListener('resize', handleResize);
      cancelAnimationFrame(animationFrameIdRef.current);
    };
  }, [init, tick, setSize, setDiscs, setLines, setParticles]);

  return (
    <div
      data-slot="hole-background"
      className={cn(
        'relative size-full overflow-hidden',
        'before:content-[""] before:absolute before:top-1/2 before:left-1/2 before:block before:size-[140%] dark:before:[background:radial-gradient(ellipse_at_50%_55%,transparent_10%,black_50%)] before:[background:radial-gradient(ellipse_at_50%_55%,transparent_10%,white_50%)] before:[transform:translate3d(-50%,-50%,0)]',
        'after:content-[""] after:absolute after:z-[5] after:top-1/2 after:left-1/2 after:block after:size-full after:[background:radial-gradient(ellipse_at_50%_75%,#a900ff_20%,transparent_75%)] after:[transform:translate3d(-50%,-50%,0)] after:mix-blend-overlay',
        className,
      )}
      {...props}
    >
      {children}
      <canvas
        ref={canvasRef}
        className="absolute inset-0 block size-full dark:opacity-20 opacity-10"
      />
      <motion.div
        className={cn(
          'absolute top-[-71.5%] left-1/2 z-[3] w-[30%] h-[140%] rounded-b-full blur-3xl opacity-75 dark:mix-blend-plus-lighter mix-blend-plus-darker [transform:translate3d(-50%,0,0)] [background-position:0%_100%] [background-size:100%_200%]',
          'dark:[background:linear-gradient(20deg,#00f8f1,#ffbd1e20_16.5%,#fe848f_33%,#fe848f20_49.5%,#00f8f1_66%,#00f8f160_85.5%,#ffbd1e_100%)_0_100%_/_100%_200%] [background:linear-gradient(20deg,#00f8f1,#ffbd1e40_16.5%,#fe848f_33%,#fe848f40_49.5%,#00f8f1_66%,#00f8f180_85.5%,#ffbd1e_100%)_0_100%_/_100%_200%]',
        )}
        animate={{ backgroundPosition: '0% 300%' }}
        transition={{ duration: 5, ease: 'linear', repeat: Infinity }}
      />
      <div className="absolute top-0 left-0 z-[7] size-full dark:[background:repeating-linear-gradient(transparent,transparent_1px,white_1px,white_2px)] mix-blend-overlay opacity-50" />
    </div>
  );
}

export { HoleBackground, type HoleBackgroundProps };
```

---

### Stars Background <a name="stars"></a>

An interactive background featuring animated dots of varying sizes and speeds, simulating a dynamic and immersive starry space effect.

- **Category**: Backgrounds
- **Registry Key**: `components-backgrounds-stars`

#### Installation

```bash
npx shadcn@latest add components-backgrounds-stars
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
<StarsBackground />
```

#### Demo Code

##### File: `registry/demo/components/backgrounds/stars/index.tsx`

```tsx
import { StarsBackground } from '@/components/animate-ui/components/backgrounds/stars';
import { cn } from '@/lib/utils';
import { useTheme } from 'next-themes';

export const StarsBackgroundDemo = () => {
  const { resolvedTheme } = useTheme();

  return (
    <StarsBackground
      starColor={resolvedTheme === 'dark' ? '#FFF' : '#000'}
      className={cn(
        'absolute inset-0 flex items-center justify-center rounded-xl',
        'dark:bg-[radial-gradient(ellipse_at_bottom,_#262626_0%,_#000_100%)] bg-[radial-gradient(ellipse_at_bottom,_#f5f5f5_0%,_#fff_100%)]',
      )}
    />
  );
};
```

#### Component Source Code

##### File: `registry/components/backgrounds/stars/index.tsx`

```tsx
'use client';

import * as React from 'react';
import {
  type HTMLMotionProps,
  motion,
  useMotionValue,
  useSpring,
  type SpringOptions,
  type Transition,
} from 'motion/react';

import { cn } from '@/lib/utils';

type StarLayerProps = HTMLMotionProps<'div'> & {
  count: number;
  size: number;
  transition: Transition;
  starColor: string;
};

function generateStars(count: number, starColor: string) {
  const shadows: string[] = [];
  for (let i = 0; i < count; i++) {
    const x = Math.floor(Math.random() * 4000) - 2000;
    const y = Math.floor(Math.random() * 4000) - 2000;
    shadows.push(`${x}px ${y}px ${starColor}`);
  }
  return shadows.join(', ');
}

function StarLayer({
  count = 1000,
  size = 1,
  transition = { repeat: Infinity, duration: 50, ease: 'linear' },
  starColor = '#fff',
  className,
  ...props
}: StarLayerProps) {
  const [boxShadow, setBoxShadow] = React.useState<string>('');

  React.useEffect(() => {
    setBoxShadow(generateStars(count, starColor));
  }, [count, starColor]);

  return (
    <motion.div
      data-slot="star-layer"
      animate={{ y: [0, -2000] }}
      transition={transition}
      className={cn('absolute top-0 left-0 w-full h-[2000px]', className)}
      {...props}
    >
      <div
        className="absolute bg-transparent rounded-full"
        style={{
          width: `${size}px`,
          height: `${size}px`,
          boxShadow: boxShadow,
        }}
      />
      <div
        className="absolute bg-transparent rounded-full top-[2000px]"
        style={{
          width: `${size}px`,
          height: `${size}px`,
          boxShadow: boxShadow,
        }}
      />
    </motion.div>
  );
}

type StarsBackgroundProps = React.ComponentProps<'div'> & {
  factor?: number;
  speed?: number;
  transition?: SpringOptions;
  starColor?: string;
  pointerEvents?: boolean;
};

function StarsBackground({
  children,
  className,
  factor = 0.05,
  speed = 50,
  transition = { stiffness: 50, damping: 20 },
  starColor = '#fff',
  pointerEvents = true,
  ...props
}: StarsBackgroundProps) {
  const offsetX = useMotionValue(1);
  const offsetY = useMotionValue(1);

  const springX = useSpring(offsetX, transition);
  const springY = useSpring(offsetY, transition);

  const handleMouseMove = React.useCallback(
    (e: React.MouseEvent<HTMLDivElement, MouseEvent>) => {
      const centerX = window.innerWidth / 2;
      const centerY = window.innerHeight / 2;
      const newOffsetX = -(e.clientX - centerX) * factor;
      const newOffsetY = -(e.clientY - centerY) * factor;
      offsetX.set(newOffsetX);
      offsetY.set(newOffsetY);
    },
    [offsetX, offsetY, factor],
  );

  return (
    <div
      data-slot="stars-background"
      className={cn(
        'relative size-full overflow-hidden bg-[radial-gradient(ellipse_at_bottom,_#262626_0%,_#000_100%)]',
        className,
      )}
      onMouseMove={handleMouseMove}
      {...props}
    >
      <motion.div
        style={{ x: springX, y: springY }}
        className={cn({ 'pointer-events-none': !pointerEvents })}
      >
        <StarLayer
          count={1000}
          size={1}
          transition={{ repeat: Infinity, duration: speed, ease: 'linear' }}
          starColor={starColor}
        />
        <StarLayer
          count={400}
          size={2}
          transition={{
            repeat: Infinity,
            duration: speed * 2,
            ease: 'linear',
          }}
          starColor={starColor}
        />
        <StarLayer
          count={200}
          size={3}
          transition={{
            repeat: Infinity,
            duration: speed * 3,
            ease: 'linear',
          }}
          starColor={starColor}
        />
      </motion.div>
      {children}
    </div>
  );
}

export {
  StarLayer,
  StarsBackground,
  type StarLayerProps,
  type StarsBackgroundProps,
};
```

---

## Base <a name="base"></a>

### Accordion <a name="accordion"></a>

A set of collapsible panels with headings.

- **Category**: Base
- **Registry Key**: `components-base-accordion`

#### Installation

```bash
npx shadcn@latest add components-base-accordion
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
<Accordion>
  <AccordionItem>
    <AccordionTrigger>Accordion Item 1</AccordionTrigger>
    <AccordionPanel>
      <div>Accordion Content 1</div>
    </AccordionPanel>
  </AccordionItem>
  <AccordionItem>
    <AccordionTrigger>Accordion Item 2</AccordionTrigger>
    <AccordionPanel>
      <div>Accordion Content 2</div>
    </AccordionPanel>
  </AccordionItem>
</Accordion>
```

#### Demo Code

##### File: `registry/demo/components/base/accordion/index.tsx`

```tsx
import {
  Accordion,
  AccordionItem,
  AccordionTrigger,
  AccordionPanel,
} from '@/components/animate-ui/components/base/accordion';

const ITEMS = [
  {
    title: 'What is Animate UI?',
    content:
      'Animate UI is an open-source distribution of React components built with TypeScript, Tailwind CSS, and Motion.',
  },
  {
    title: 'How is it different from other libraries?',
    content:
      'Instead of installing via NPM, you copy and paste the components directly. This gives you full control to modify or customize them as needed.',
  },
  {
    title: 'Is Animate UI free to use?',
    content:
      'Absolutely! Animate UI is fully open-source. You can use, modify, and adapt it to fit your needs.',
  },
];

type BaseAccordionDemoProps = {
  multiple?: boolean;
  keepRendered?: boolean;
  showArrow?: boolean;
};

export const BaseAccordionDemo = ({
  multiple = false,
  keepRendered = false,
  showArrow = true,
}: BaseAccordionDemoProps) => {
  return (
    <Accordion multiple={multiple} className="max-w-[400px] w-full">
      {ITEMS.map((item, index) => (
        <AccordionItem key={index} value={`item-${index + 1}`}>
          <AccordionTrigger showArrow={showArrow}>
            {item.title}
          </AccordionTrigger>
          <AccordionPanel keepRendered={keepRendered}>
            {item.content}
          </AccordionPanel>
        </AccordionItem>
      ))}
    </Accordion>
  );
};
```

#### Component Source Code

##### File: `registry/components/base/accordion/index.tsx`

```tsx
import * as React from 'react';
import { ChevronDownIcon } from 'lucide-react';

import {
  Accordion as AccordionPrimitive,
  AccordionItem as AccordionItemPrimitive,
  AccordionHeader as AccordionHeaderPrimitive,
  AccordionTrigger as AccordionTriggerPrimitive,
  AccordionPanel as AccordionPanelPrimitive,
  type AccordionProps as AccordionPrimitiveProps,
  type AccordionItemProps as AccordionItemPrimitiveProps,
  type AccordionTriggerProps as AccordionTriggerPrimitiveProps,
  type AccordionPanelProps as AccordionPanelPrimitiveProps,
} from '@/components/animate-ui/primitives/base/accordion';
import { cn } from '@/lib/utils';

type AccordionProps = AccordionPrimitiveProps;

function Accordion(props: AccordionProps) {
  return <AccordionPrimitive {...props} />;
}

type AccordionItemProps = AccordionItemPrimitiveProps;

function AccordionItem({ className, ...props }: AccordionItemProps) {
  return (
    <AccordionItemPrimitive
      className={cn('border-b last:border-b-0', className)}
      {...props}
    />
  );
}

type AccordionTriggerProps = AccordionTriggerPrimitiveProps & {
  showArrow?: boolean;
};

function AccordionTrigger({
  className,
  children,
  showArrow = true,
  ...props
}: AccordionTriggerProps) {
  return (
    <AccordionHeaderPrimitive className="flex">
      <AccordionTriggerPrimitive
        className={cn(
          'focus-visible:border-ring focus-visible:ring-ring/50 flex flex-1 items-start justify-between gap-4 rounded-md py-4 text-left text-sm font-medium transition-all outline-none hover:underline focus-visible:ring-[3px] disabled:pointer-events-none disabled:opacity-50 [&[data-panel-open]>svg]:rotate-180',
          className,
        )}
        {...props}
      >
        {children}
        {showArrow && (
          <ChevronDownIcon className="text-muted-foreground pointer-events-none size-4 shrink-0 translate-y-0.5 transition-transform duration-200" />
        )}
      </AccordionTriggerPrimitive>
    </AccordionHeaderPrimitive>
  );
}

type AccordionPanelProps = AccordionPanelPrimitiveProps & {
  children: React.ReactNode;
};

function AccordionPanel({
  className,
  children,
  ...props
}: AccordionPanelProps) {
  return (
    <AccordionPanelPrimitive {...props}>
      <div className={cn('text-sm pt-0 pb-4', className)}>{children}</div>
    </AccordionPanelPrimitive>
  );
}

export {
  Accordion,
  AccordionItem,
  AccordionTrigger,
  AccordionPanel,
  type AccordionProps,
  type AccordionItemProps,
  type AccordionTriggerProps,
  type AccordionPanelProps,
};
```

---

### Alert Dialog <a name="alert-dialog"></a>

A dialog that requires user response to proceed.

- **Category**: Base
- **Registry Key**: `components-base-alert-dialog`

#### Installation

```bash
npx shadcn@latest add components-base-alert-dialog
```

#### How to Use

```tsx
<AlertDialog>
  <AlertDialogTrigger>Open Dialog</AlertDialogTrigger>
  <AlertDialogPopup>
    <AlertDialogHeader>
      <AlertDialogTitle>Alert Dialog Title</AlertDialogTitle>
      <AlertDialogDescription>Alert Dialog Description</AlertDialogDescription>
    </AlertDialogHeader>
    <AlertDialogFooter>
      <AlertDialogCancel>Cancel</AlertDialogCancel>
      <AlertDialogAction>Accept</AlertDialogAction>
    </AlertDialogFooter>
  </AlertDialogPopup>
</AlertDialog>
```

#### Demo Code

##### File: `registry/demo/components/base/alert-dialog/index.tsx`

```tsx
import * as React from 'react';

import {
  AlertDialog,
  AlertDialogTrigger,
  AlertDialogPopup,
  AlertDialogHeader,
  AlertDialogTitle,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogCancel,
  AlertDialogAction,
  type AlertDialogPopupProps,
} from '@/components/animate-ui/components/base/alert-dialog';
import { Button } from '@/components/ui/button';

interface BaseAlertDialogDemoProps {
  from: AlertDialogPopupProps['from'];
}

export const BaseAlertDialogDemo = ({ from }: BaseAlertDialogDemoProps) => {
  return (
    <AlertDialog>
      <AlertDialogTrigger
        render={<Button variant="outline">Open Dialog</Button>}
      />
      <AlertDialogPopup from={from} className="sm:max-w-[425px]">
        <AlertDialogHeader>
          <AlertDialogTitle>Are you absolutely sure?</AlertDialogTitle>
          <AlertDialogDescription>
            This action cannot be undone. This will permanently delete your
            account and remove your data from our servers.
          </AlertDialogDescription>
        </AlertDialogHeader>
        <AlertDialogFooter>
          <AlertDialogCancel>Cancel</AlertDialogCancel>
          <AlertDialogAction>Continue</AlertDialogAction>
        </AlertDialogFooter>
      </AlertDialogPopup>
    </AlertDialog>
  );
};
```

#### Component Source Code

##### File: `registry/components/base/alert-dialog/index.tsx`

```tsx
import * as React from 'react';

import {
  AlertDialog as AlertDialogPrimitive,
  AlertDialogPopup as AlertDialogPopupPrimitive,
  AlertDialogDescription as AlertDialogDescriptionPrimitive,
  AlertDialogFooter as AlertDialogFooterPrimitive,
  AlertDialogHeader as AlertDialogHeaderPrimitive,
  AlertDialogTitle as AlertDialogTitlePrimitive,
  AlertDialogTrigger as AlertDialogTriggerPrimitive,
  AlertDialogPortal as AlertDialogPortalPrimitive,
  AlertDialogBackdrop as AlertDialogBackdropPrimitive,
  AlertDialogClose as AlertDialogClosePrimitive,
  type AlertDialogProps as AlertDialogPrimitiveProps,
  type AlertDialogPopupProps as AlertDialogPopupPrimitiveProps,
  type AlertDialogDescriptionProps as AlertDialogDescriptionPrimitiveProps,
  type AlertDialogFooterProps as AlertDialogFooterPrimitiveProps,
  type AlertDialogHeaderProps as AlertDialogHeaderPrimitiveProps,
  type AlertDialogTitleProps as AlertDialogTitlePrimitiveProps,
  type AlertDialogTriggerProps as AlertDialogTriggerPrimitiveProps,
  type AlertDialogBackdropProps as AlertDialogBackdropPrimitiveProps,
  type AlertDialogCloseProps as AlertDialogClosePrimitiveProps,
} from '@/components/animate-ui/primitives/base/alert-dialog';
import { buttonVariants } from '@/components/animate-ui/components/buttons/button';
import { cn } from '@/lib/utils';

type AlertDialogProps = AlertDialogPrimitiveProps;

function AlertDialog(props: AlertDialogProps) {
  return <AlertDialogPrimitive {...props} />;
}

type AlertDialogTriggerProps = AlertDialogTriggerPrimitiveProps;

function AlertDialogTrigger(props: AlertDialogTriggerProps) {
  return <AlertDialogTriggerPrimitive {...props} />;
}

type AlertDialogBackdropProps = AlertDialogBackdropPrimitiveProps;

function AlertDialogBackdrop({
  className,
  ...props
}: AlertDialogBackdropProps) {
  return (
    <AlertDialogBackdropPrimitive
      className={cn('fixed inset-0 z-50 bg-black/50', className)}
      {...props}
    />
  );
}

type AlertDialogPopupProps = AlertDialogPopupPrimitiveProps;

function AlertDialogPopup({ className, ...props }: AlertDialogPopupProps) {
  return (
    <AlertDialogPortalPrimitive>
      <AlertDialogBackdrop />
      <AlertDialogPopupPrimitive
        className={cn(
          'bg-background fixed top-[50%] left-[50%] z-50 grid w-full max-w-[calc(100%-2rem)] translate-x-[-50%] translate-y-[-50%] gap-4 rounded-lg border p-6 shadow-lg sm:max-w-lg',
          className,
        )}
        {...props}
      />
    </AlertDialogPortalPrimitive>
  );
}

type AlertDialogHeaderProps = AlertDialogHeaderPrimitiveProps;

function AlertDialogHeader({ className, ...props }: AlertDialogHeaderProps) {
  return (
    <AlertDialogHeaderPrimitive
      className={cn('flex flex-col gap-2 text-center sm:text-left', className)}
      {...props}
    />
  );
}

type AlertDialogFooterProps = AlertDialogFooterPrimitiveProps;

function AlertDialogFooter({ className, ...props }: AlertDialogFooterProps) {
  return (
    <AlertDialogFooterPrimitive
      className={cn(
        'flex flex-col-reverse gap-2 sm:flex-row sm:justify-end',
        className,
      )}
      {...props}
    />
  );
}

type AlertDialogTitleProps = AlertDialogTitlePrimitiveProps;

function AlertDialogTitle({ className, ...props }: AlertDialogTitleProps) {
  return (
    <AlertDialogTitlePrimitive
      className={cn('text-lg font-semibold', className)}
      {...props}
    />
  );
}

type AlertDialogDescriptionProps = AlertDialogDescriptionPrimitiveProps;

function AlertDialogDescription({
  className,
  ...props
}: AlertDialogDescriptionProps) {
  return (
    <AlertDialogDescriptionPrimitive
      className={cn('text-muted-foreground text-sm', className)}
      {...props}
    />
  );
}

type AlertDialogActionProps = AlertDialogClosePrimitiveProps;

function AlertDialogAction({ className, ...props }: AlertDialogActionProps) {
  return (
    <AlertDialogClosePrimitive
      className={cn(buttonVariants(), className)}
      {...props}
    />
  );
}

type AlertDialogCancelProps = AlertDialogClosePrimitiveProps;

function AlertDialogCancel({ className, ...props }: AlertDialogCancelProps) {
  return (
    <AlertDialogClosePrimitive
      className={cn(buttonVariants({ variant: 'outline' }), className)}
      {...props}
    />
  );
}

export {
  AlertDialog,
  AlertDialogTrigger,
  AlertDialogPopup,
  AlertDialogHeader,
  AlertDialogFooter,
  AlertDialogTitle,
  AlertDialogDescription,
  AlertDialogAction,
  AlertDialogCancel,
  type AlertDialogProps,
  type AlertDialogTriggerProps,
  type AlertDialogPopupProps,
  type AlertDialogHeaderProps,
  type AlertDialogFooterProps,
  type AlertDialogTitleProps,
  type AlertDialogDescriptionProps,
  type AlertDialogActionProps,
  type AlertDialogCancelProps,
};
```

---

### Checkbox <a name="checkbox"></a>

An easily stylable checkbox component.

- **Category**: Base
- **Registry Key**: `components-base-checkbox`

#### Installation

```bash
npx shadcn@latest add components-base-checkbox
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`

#### How to Use

```tsx
<Checkbox />
```

#### Demo Code

##### File: `registry/demo/components/base/checkbox/index.tsx`

```tsx
import { Label } from '@/components/ui/label';
import {
  Checkbox,
  type CheckboxProps,
} from '@/components/animate-ui/components/base/checkbox';

interface BaseCheckboxDemoProps {
  indeterminate: boolean;
  variant: CheckboxProps['variant'];
  size: CheckboxProps['size'];
}

export const BaseCheckboxDemo = ({
  indeterminate,
  variant,
  size,
}: BaseCheckboxDemoProps) => {
  return (
    <Label className="flex items-center gap-x-3">
      <Checkbox indeterminate={indeterminate} variant={variant} size={size} />
      Accept terms and conditions
    </Label>
  );
};
```

#### Component Source Code

##### File: `registry/components/base/checkbox/index.tsx`

```tsx
import * as React from 'react';

import {
  Checkbox as CheckboxPrimitive,
  CheckboxIndicator as CheckboxIndicatorPrimitive,
  type CheckboxProps as CheckboxPrimitiveProps,
} from '@/components/animate-ui/primitives/base/checkbox';
import { cn } from '@/lib/utils';
import { cva, type VariantProps } from 'class-variance-authority';

const checkboxVariants = cva(
  'peer shrink-0 flex items-center justify-center outline-none focus-visible:ring-[3px] focus-visible:ring-ring/50 aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 disabled:cursor-not-allowed disabled:opacity-50 transition-colors duration-500 focus-visible:ring-offset-2 [&[data-checked],&[data-indeterminate]]:bg-primary [&[data-checked],&[data-indeterminate]]:text-primary-foreground',
  {
    variants: {
      variant: {
        default: 'bg-background border',
        accent: 'bg-input',
      },
      size: {
        default: 'size-5 rounded-sm',
        sm: 'size-4.5 rounded-[5px]',
        lg: 'size-6 rounded-[7px]',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  },
);

const checkboxIndicatorVariants = cva('', {
  variants: {
    size: {
      default: 'size-3.5',
      sm: 'size-3',
      lg: 'size-4',
    },
  },
  defaultVariants: {
    size: 'default',
  },
});

type CheckboxProps = CheckboxPrimitiveProps &
  VariantProps<typeof checkboxVariants> & {
    children?: React.ReactNode;
  };

function Checkbox({
  className,
  children,
  variant,
  size,
  ...props
}: CheckboxProps) {
  return (
    <CheckboxPrimitive
      className={cn(checkboxVariants({ variant, size, className }))}
      {...props}
    >
      {children}
      <CheckboxIndicatorPrimitive
        className={cn(checkboxIndicatorVariants({ size }))}
      />
    </CheckboxPrimitive>
  );
}

export { Checkbox, type CheckboxProps };
```

---

### Dialog <a name="dialog"></a>

A popup that opens on top of the entire page.

- **Category**: Base
- **Registry Key**: `components-base-dialog`

#### Installation

```bash
npx shadcn@latest add components-base-dialog
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
<Dialog>
  <DialogTrigger>Open Dialog</DialogTrigger>
  <DialogPopup>
    <DialogHeader>
      <DialogTitle>Dialog Title</DialogTitle>
      <DialogDescription>Dialog Description</DialogDescription>
    </DialogHeader>
    <p>Dialog Content</p>
    <DialogFooter>
      <button>Accept</button>
    </DialogFooter>
  </DialogPopup>
</Dialog>
```

#### Demo Code

##### File: `registry/demo/components/base/dialog/index.tsx`

```tsx
import * as React from 'react';

import { Button } from '@/components/ui/button';
import {
  Dialog,
  DialogTrigger,
  DialogPopup,
  DialogHeader,
  DialogTitle,
  DialogDescription,
  DialogClose,
  DialogFooter,
  type DialogPopupProps,
} from '@/components/animate-ui/components/base/dialog';
import { Label } from '@/components/ui/label';
import { Input } from '@/components/ui/input';

interface BaseDialogDemoProps {
  from: DialogPopupProps['from'];
  showCloseButton: boolean;
}

export const BaseDialogDemo = ({
  from,
  showCloseButton,
}: BaseDialogDemoProps) => {
  return (
    <Dialog>
      <form>
        <DialogTrigger
          render={<Button variant="outline">Open Dialog</Button>}
        />

        <DialogPopup
          from={from}
          showCloseButton={showCloseButton}
          className="sm:max-w-[425px]"
        >
          <DialogHeader>
            <DialogTitle>Edit profile</DialogTitle>
            <DialogDescription>
              Make changes to your profile here. Click save when you&apos;re
              done.
            </DialogDescription>
          </DialogHeader>
          <div className="grid gap-4">
            <div className="grid gap-3">
              <Label htmlFor="name-1">Name</Label>
              <Input id="name-1" name="name" defaultValue="Pedro Duarte" />
            </div>
            <div className="grid gap-3">
              <Label htmlFor="username-1">Username</Label>
              <Input id="username-1" name="username" defaultValue="@peduarte" />
            </div>
          </div>
          <DialogFooter>
            <DialogClose render={<Button variant="outline">Cancel</Button>} />
            <Button type="submit">Save changes</Button>
          </DialogFooter>
        </DialogPopup>
      </form>
    </Dialog>
  );
};
```

#### Component Source Code

##### File: `registry/components/base/dialog/index.tsx`

```tsx
import * as React from 'react';
import { XIcon } from 'lucide-react';

import {
  Dialog as DialogPrimitive,
  DialogPopup as DialogPopupPrimitive,
  DialogDescription as DialogDescriptionPrimitive,
  DialogFooter as DialogFooterPrimitive,
  DialogHeader as DialogHeaderPrimitive,
  DialogTitle as DialogTitlePrimitive,
  DialogTrigger as DialogTriggerPrimitive,
  DialogPortal as DialogPortalPrimitive,
  DialogBackdrop as DialogBackdropPrimitive,
  DialogClose as DialogClosePrimitive,
  type DialogProps as DialogPrimitiveProps,
  type DialogPopupProps as DialogPopupPrimitiveProps,
  type DialogDescriptionProps as DialogDescriptionPrimitiveProps,
  type DialogFooterProps as DialogFooterPrimitiveProps,
  type DialogHeaderProps as DialogHeaderPrimitiveProps,
  type DialogTitleProps as DialogTitlePrimitiveProps,
  type DialogTriggerProps as DialogTriggerPrimitiveProps,
  type DialogBackdropProps as DialogBackdropPrimitiveProps,
  type DialogCloseProps as DialogClosePrimitiveProps,
} from '@/components/animate-ui/primitives/base/dialog';
import { cn } from '@/lib/utils';

type DialogProps = DialogPrimitiveProps;

function Dialog(props: DialogProps) {
  return <DialogPrimitive {...props} />;
}

type DialogTriggerProps = DialogTriggerPrimitiveProps;

function DialogTrigger(props: DialogTriggerProps) {
  return <DialogTriggerPrimitive {...props} />;
}

type DialogCloseProps = DialogClosePrimitiveProps;

function DialogClose(props: DialogCloseProps) {
  return <DialogClosePrimitive {...props} />;
}

type DialogBackdropProps = DialogBackdropPrimitiveProps;

function DialogBackdrop({ className, ...props }: DialogBackdropProps) {
  return (
    <DialogBackdropPrimitive
      className={cn('fixed inset-0 z-50 bg-black/50', className)}
      {...props}
    />
  );
}

type DialogPopupProps = DialogPopupPrimitiveProps & {
  showCloseButton?: boolean;
};

function DialogPopup({
  className,
  children,
  showCloseButton = true,
  ...props
}: DialogPopupProps) {
  return (
    <DialogPortalPrimitive>
      <DialogBackdrop />
      <DialogPopupPrimitive
        className={cn(
          'bg-background fixed top-[50%] left-[50%] z-50 grid w-full max-w-[calc(100%-2rem)] translate-x-[-50%] translate-y-[-50%] gap-4 rounded-lg border p-6 shadow-lg sm:max-w-lg',
          className,
        )}
        {...props}
      >
        {children}
        {showCloseButton && (
          <DialogClosePrimitive className="ring-offset-background focus:ring-ring data-[open]:bg-accent data-[open]:text-muted-foreground absolute top-4 right-4 rounded-xs opacity-70 transition-opacity hover:opacity-100 focus:ring-2 focus:ring-offset-2 focus:outline-hidden disabled:pointer-events-none [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4">
            <XIcon />
            <span className="sr-only">Close</span>
          </DialogClosePrimitive>
        )}
      </DialogPopupPrimitive>
    </DialogPortalPrimitive>
  );
}

type DialogHeaderProps = DialogHeaderPrimitiveProps;

function DialogHeader({ className, ...props }: DialogHeaderProps) {
  return (
    <DialogHeaderPrimitive
      className={cn('flex flex-col gap-2 text-center sm:text-left', className)}
      {...props}
    />
  );
}

type DialogFooterProps = DialogFooterPrimitiveProps;

function DialogFooter({ className, ...props }: DialogFooterProps) {
  return (
    <DialogFooterPrimitive
      className={cn(
        'flex flex-col-reverse gap-2 sm:flex-row sm:justify-end',
        className,
      )}
      {...props}
    />
  );
}

type DialogTitleProps = DialogTitlePrimitiveProps;

function DialogTitle({ className, ...props }: DialogTitleProps) {
  return (
    <DialogTitlePrimitive
      className={cn('text-lg leading-none font-semibold', className)}
      {...props}
    />
  );
}

type DialogDescriptionProps = DialogDescriptionPrimitiveProps;

function DialogDescription({ className, ...props }: DialogDescriptionProps) {
  return (
    <DialogDescriptionPrimitive
      className={cn('text-muted-foreground text-sm', className)}
      {...props}
    />
  );
}

export {
  Dialog,
  DialogTrigger,
  DialogClose,
  DialogPopup,
  DialogHeader,
  DialogFooter,
  DialogTitle,
  DialogDescription,
  type DialogProps,
  type DialogTriggerProps,
  type DialogCloseProps,
  type DialogPopupProps,
  type DialogHeaderProps,
  type DialogFooterProps,
  type DialogTitleProps,
  type DialogDescriptionProps,
};
```

---

### Files <a name="files"></a>

A component that allows you to display a list of files and folders.

- **Category**: Base
- **Registry Key**: `components-base-files`

#### Installation

```bash
npx shadcn@latest add components-base-files
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
<Files>
  <FolderItem value="app">
    <FolderTrigger>app</FolderTrigger>

    <FolderPanel>
      <SubFiles>
        <FolderItem value="(home)">
          <FolderTrigger>(home)</FolderTrigger>

          <FolderPanel>
            <FileItem>page.tsx</FileItem>
            <FileItem>layout.tsx</FileItem>
          </FolderPanel>
        </FolderItem>

        <FileItem>layout.tsx</FileItem>
        <FileItem>page.tsx</FileItem>
        <FileItem>global.css</FileItem>
      </SubFiles>
    </FolderPanel>
  </FolderItem>

  <FolderItem value="components">
    <FolderTrigger>components</FolderTrigger>

    <FolderPanel>
      <SubFiles>
        <FileItem>button.tsx</FileItem>
        <FileItem>tabs.tsx</FileItem>
        <FileItem>dialog.tsx</FileItem>

        <FolderItem value="empty">
          <FolderTrigger>empty</FolderTrigger>
        </FolderItem>
      </SubFiles>
    </FolderPanel>
  </FolderItem>

  <FileItem>package.json</FileItem>
</Files>
```

#### Demo Code

##### File: `registry/demo/components/base/files/index.tsx`

```tsx
'use client';

import React from 'react';
import {
  FileItem,
  FolderItem,
  FolderTrigger,
  FolderPanel,
  Files,
  SubFiles,
} from '@/components/animate-ui/components/base/files';
import { FileJsonIcon } from 'lucide-react';

export const BaseFilesDemo = () => {
  return (
    <div className="relative max-w-[500px] max-h-[350px] size-full rounded-2xl border bg-background overflow-auto">
      <Files className="w-full" defaultOpen={['app']}>
        <FolderItem value="app">
          <FolderTrigger
            gitStatus="modified"
            className="w-full flex items-center justify-between"
          >
            app
          </FolderTrigger>

          <FolderPanel>
            <SubFiles defaultOpen={['(home)']}>
              <FolderItem value="(home)">
                <FolderTrigger gitStatus="untracked">(home)</FolderTrigger>

                <FolderPanel>
                  <FileItem gitStatus="untracked">page.tsx</FileItem>
                  <FileItem gitStatus="untracked">layout.tsx</FileItem>
                </FolderPanel>
              </FolderItem>

              <FileItem>layout.tsx</FileItem>
              <FileItem gitStatus="modified">page.tsx</FileItem>
              <FileItem>global.css</FileItem>
            </SubFiles>
          </FolderPanel>
        </FolderItem>

        <FolderItem value="components">
          <FolderTrigger>components</FolderTrigger>

          <FolderPanel>
            <SubFiles>
              <FileItem>button.tsx</FileItem>
              <FileItem>tabs.tsx</FileItem>
              <FileItem>dialog.tsx</FileItem>

              <FolderItem value="empty">
                <FolderTrigger>empty</FolderTrigger>
              </FolderItem>
            </SubFiles>
          </FolderPanel>
        </FolderItem>

        <FileItem icon={FileJsonIcon}>package.json</FileItem>
      </Files>
    </div>
  );
};
```

#### Component Source Code

##### File: `registry/components/base/files/index.tsx`

```tsx
import * as React from 'react';
import { FolderIcon, FolderOpenIcon, FileIcon } from 'lucide-react';

import {
  Files as FilesPrimitive,
  FilesHighlight as FilesHighlightPrimitive,
  FolderItem as FolderItemPrimitive,
  FolderHeader as FolderHeaderPrimitive,
  FolderTrigger as FolderTriggerPrimitive,
  FolderHighlight as FolderHighlightPrimitive,
  Folder as FolderPrimitive,
  FolderIcon as FolderIconPrimitive,
  FileLabel as FileLabelPrimitive,
  FolderPanel as FolderPanelPrimitive,
  FileHighlight as FileHighlightPrimitive,
  File as FilePrimitive,
  FileIcon as FileIconPrimitive,
  type FilesProps as FilesPrimitiveProps,
  type FolderItemProps as FolderItemPrimitiveProps,
  type FolderPanelProps as FolderPanelPrimitiveProps,
  type FileProps as FilePrimitiveProps,
  type FileLabelProps as FileLabelPrimitiveProps,
} from '@/components/animate-ui/primitives/base/files';
import { cn } from '@/lib/utils';

type GitStatus = 'untracked' | 'modified' | 'deleted';

type FilesProps = FilesPrimitiveProps;

function Files({ className, children, ...props }: FilesProps) {
  return (
    <FilesPrimitive className={cn('p-2 w-full', className)} {...props}>
      <FilesHighlightPrimitive className="bg-accent rounded-lg pointer-events-none">
        {children}
      </FilesHighlightPrimitive>
    </FilesPrimitive>
  );
}

type SubFilesProps = FilesProps;

function SubFiles(props: SubFilesProps) {
  return <FilesPrimitive {...props} />;
}

type FolderItemProps = FolderItemPrimitiveProps;

function FolderItem(props: FolderItemProps) {
  return <FolderItemPrimitive {...props} />;
}

type FolderTriggerProps = FileLabelPrimitiveProps & {
  gitStatus?: GitStatus;
};

function FolderTrigger({
  children,
  className,
  gitStatus,
  ...props
}: FolderTriggerProps) {
  return (
    <FolderHeaderPrimitive>
      <FolderTriggerPrimitive className="w-full text-start">
        <FolderHighlightPrimitive>
          <FolderPrimitive className="flex items-center justify-between gap-2 p-2 pointer-events-none">
            <div
              className={cn(
                'flex items-center gap-2',
                gitStatus === 'untracked' && 'text-green-400',
                gitStatus === 'modified' && 'text-amber-400',
                gitStatus === 'deleted' && 'text-red-400',
              )}
            >
              <FolderIconPrimitive
                closeIcon={<FolderIcon className="size-4.5" />}
                openIcon={<FolderOpenIcon className="size-4.5" />}
              />
              <FileLabelPrimitive
                className={cn('text-sm', className)}
                {...props}
              >
                {children}
              </FileLabelPrimitive>
            </div>

            {gitStatus && (
              <span
                className={cn(
                  'rounded-full size-2',
                  gitStatus === 'untracked' && 'bg-green-400',
                  gitStatus === 'modified' && 'bg-amber-400',
                  gitStatus === 'deleted' && 'bg-red-400',
                )}
              />
            )}
          </FolderPrimitive>
        </FolderHighlightPrimitive>
      </FolderTriggerPrimitive>
    </FolderHeaderPrimitive>
  );
}

type FolderPanelProps = FolderPanelPrimitiveProps;

function FolderPanel(props: FolderPanelProps) {
  return (
    <div className="relative ml-6 before:absolute before:-left-2 before:inset-y-0 before:w-px before:h-full before:bg-border">
      <FolderPanelPrimitive {...props} />
    </div>
  );
}

type FileItemProps = FilePrimitiveProps & {
  icon?: React.ElementType;
  gitStatus?: GitStatus;
};

function FileItem({
  icon: Icon = FileIcon,
  className,
  children,
  gitStatus,
  ...props
}: FileItemProps) {
  return (
    <FileHighlightPrimitive>
      <FilePrimitive
        className={cn(
          'flex items-center justify-between gap-2 p-2 pointer-events-none',
          gitStatus === 'untracked' && 'text-green-400',
          gitStatus === 'modified' && 'text-amber-400',
          gitStatus === 'deleted' && 'text-red-400',
        )}
      >
        <div className="flex items-center gap-2">
          <FileIconPrimitive>
            <Icon className="size-4.5" />
          </FileIconPrimitive>
          <FileLabelPrimitive className={cn('text-sm', className)} {...props}>
            {children}
          </FileLabelPrimitive>
        </div>

        {gitStatus && (
          <span className="text-sm font-medium">
            {gitStatus === 'untracked' && 'U'}
            {gitStatus === 'modified' && 'M'}
            {gitStatus === 'deleted' && 'D'}
          </span>
        )}
      </FilePrimitive>
    </FileHighlightPrimitive>
  );
}

export {
  Files,
  FolderItem,
  FolderTrigger,
  FolderPanel,
  FileItem,
  SubFiles,
  type FilesProps,
  type FolderItemProps,
  type FolderTriggerProps,
  type FolderPanelProps,
  type FileItemProps,
  type SubFilesProps,
};
```

---

### Menu <a name="menu"></a>

A list of actions in a dropdown, enhanced with keyboard navigation.

- **Category**: Base
- **Registry Key**: `components-base-menu`

#### Installation

```bash
npx shadcn@latest add components-base-menu
```

#### How to Use

```tsx
<Menu>
  <MenuTrigger>Open</MenuTrigger>

  <MenuPanel>
    <MenuGroup>
      <MenuItem>
        <span>Profile</span>
        <MenuShortcut>⇧⌘P</MenuShortcut>
      </MenuItem>
      <MenuItem>
        <span>Billing</span>
        <MenuShortcut>⌘B</MenuShortcut>
      </MenuItem>
      <MenuItem>
        <span>Settings</span>
        <MenuShortcut>⌘S</MenuShortcut>
      </MenuItem>
      <MenuItem>
        <span>Keyboard shortcuts</span>
        <MenuShortcut>⌘K</MenuShortcut>
      </MenuItem>
    </MenuGroup>

    <MenuSeparator />

    <MenuGroup>
      <MenuItem>
        <span>Team</span>
      </MenuItem>

      <MenuSubmenu>
        <MenuSubmenuTrigger>
          <span>Invite users</span>
          <ChevronRight />
        </MenuSubmenuTrigger>

        <MenuSubmenuPanel>
          <MenuItem>
            <span>Email</span>
          </MenuItem>
          <MenuItem>
            <span>Message</span>
          </MenuItem>

          <MenuSeparator />

          <MenuItem>
            <span>More...</span>
          </MenuItem>
        </MenuSubmenuPanel>
      </MenuSubmenu>

      <MenuItem>
        <span>New Team</span>
        <MenuShortcut>⌘+T</MenuShortcut>
      </MenuItem>
    </MenuGroup>

    <MenuSeparator />

    <MenuItem>
      <span>Log out</span>
      <MenuShortcut>⇧⌘Q</MenuShortcut>
    </MenuItem>
  </MenuPanel>
</Menu>
```

#### Demo Code

##### File: `registry/demo/components/base/menu/index.tsx`

```tsx
import { Button } from '@/components/ui/button';
import {
  Menu,
  MenuTrigger,
  MenuPanel,
  MenuGroup,
  MenuGroupLabel,
  MenuItem,
  MenuSeparator,
  MenuShortcut,
  MenuSubmenu,
  MenuSubmenuTrigger,
  MenuSubmenuPanel,
} from '@/components/animate-ui/components/base/menu';

interface BaseMenuDemoProps {
  side?: 'top' | 'bottom' | 'left' | 'right' | 'inline-start' | 'inline-end';
  sideOffset?: number;
  align?: 'start' | 'center' | 'end';
  alignOffset?: number;
}

export function BaseMenuDemo({
  side,
  sideOffset,
  align,
  alignOffset,
}: BaseMenuDemoProps) {
  return (
    <Menu>
      <MenuTrigger render={<Button variant="outline">Open</Button>} />
      <MenuPanel
        className="w-56"
        align={align}
        alignOffset={alignOffset}
        side={side}
        sideOffset={sideOffset}
      >
        <MenuGroup>
          <MenuGroupLabel>My Account</MenuGroupLabel>
          <MenuItem>
            Profile
            <MenuShortcut>⇧⌘P</MenuShortcut>
          </MenuItem>
          <MenuItem>
            Billing
            <MenuShortcut>⌘B</MenuShortcut>
          </MenuItem>
          <MenuItem>
            Settings
            <MenuShortcut>⌘S</MenuShortcut>
          </MenuItem>
          <MenuItem>
            Keyboard shortcuts
            <MenuShortcut>⌘K</MenuShortcut>
          </MenuItem>
        </MenuGroup>
        <MenuSeparator />
        <MenuGroup>
          <MenuItem>Team</MenuItem>
          <MenuSubmenu>
            <MenuSubmenuTrigger>Invite users</MenuSubmenuTrigger>
            <MenuSubmenuPanel>
              <MenuItem>Email</MenuItem>
              <MenuItem>Message</MenuItem>
              <MenuSeparator />
              <MenuItem>More...</MenuItem>
            </MenuSubmenuPanel>
          </MenuSubmenu>
          <MenuItem>
            New Team
            <MenuShortcut>⌘+T</MenuShortcut>
          </MenuItem>
        </MenuGroup>
        <MenuSeparator />
        <MenuItem>GitHub</MenuItem>
        <MenuItem>Support</MenuItem>
        <MenuItem disabled>API</MenuItem>
        <MenuSeparator />
        <MenuItem variant="destructive">
          Log out
          <MenuShortcut>⇧⌘Q</MenuShortcut>
        </MenuItem>
      </MenuPanel>
    </Menu>
  );
}
```

#### Component Source Code

##### File: `registry/components/base/menu/index.tsx`

```tsx
import * as React from 'react';

import {
  Menu as MenuPrimitive,
  MenuTrigger as MenuTriggerPrimitive,
  MenuPortal as MenuPortalPrimitive,
  MenuPopup as MenuPopupPrimitive,
  MenuPositioner as MenuPositionerPrimitive,
  MenuGroup as MenuGroupPrimitive,
  MenuGroupLabel as MenuGroupLabelPrimitive,
  MenuArrow as MenuArrowPrimitive,
  MenuItem as MenuItemPrimitive,
  MenuCheckboxItem as MenuCheckboxItemPrimitive,
  MenuCheckboxItemIndicator as MenuCheckboxItemIndicatorPrimitive,
  MenuRadioGroup as MenuRadioGroupPrimitive,
  MenuRadioItem as MenuRadioItemPrimitive,
  MenuRadioItemIndicator as MenuRadioItemIndicatorPrimitive,
  MenuHighlightItem as MenuHighlightItemPrimitive,
  MenuHighlight as MenuHighlightPrimitive,
  MenuSeparator as MenuSeparatorPrimitive,
  MenuShortcut as MenuShortcutPrimitive,
  MenuSubmenu as MenuSubmenuPrimitive,
  MenuSubmenuTrigger as MenuSubmenuTriggerPrimitive,
  type MenuProps as MenuPrimitiveProps,
  type MenuTriggerProps as MenuTriggerPrimitiveProps,
  type MenuPortalProps as MenuPortalPrimitiveProps,
  type MenuPopupProps as MenuPopupPrimitiveProps,
  type MenuPositionerProps as MenuPositionerPrimitiveProps,
  type MenuGroupProps as MenuGroupPrimitiveProps,
  type MenuGroupLabelProps as MenuGroupLabelPrimitiveProps,
  type MenuArrowProps as MenuArrowPrimitiveProps,
  type MenuItemProps as MenuItemPrimitiveProps,
  type MenuCheckboxItemProps as MenuCheckboxItemPrimitiveProps,
  type MenuRadioGroupProps as MenuRadioGroupPrimitiveProps,
  type MenuRadioItemProps as MenuRadioItemPrimitiveProps,
  type MenuSeparatorProps as MenuSeparatorPrimitiveProps,
  type MenuShortcutProps as MenuShortcutPrimitiveProps,
  type MenuSubmenuProps as MenuSubmenuPrimitiveProps,
  type MenuSubmenuTriggerProps as MenuSubmenuTriggerPrimitiveProps,
} from '@/components/animate-ui/primitives/base/menu';
import { cn } from '@/lib/utils';
import { CheckIcon, ChevronRightIcon, CircleIcon } from 'lucide-react';

type MenuProps = MenuPrimitiveProps;

function Menu(props: MenuProps) {
  return <MenuPrimitive {...props} />;
}

type MenuTriggerProps = MenuTriggerPrimitiveProps;

function MenuTrigger(props: MenuTriggerProps) {
  return <MenuTriggerPrimitive {...props} />;
}

type MenuPortalProps = MenuPortalPrimitiveProps;

function MenuPortal(props: MenuPortalProps) {
  return <MenuPortalPrimitive {...props} />;
}

type MenuPanelProps = MenuPopupPrimitiveProps & MenuPositionerPrimitiveProps;

function MenuPanel({
  className,
  finalFocus,
  id,
  children,
  sideOffset = 4,
  transition = { duration: 0.2 },
  ...props
}: MenuPanelProps) {
  return (
    <MenuPortal>
      <MenuPositionerPrimitive
        className="z-50"
        sideOffset={sideOffset}
        {...props}
      >
        <MenuPopupPrimitive
          finalFocus={finalFocus}
          transition={transition}
          id={id}
          className={cn(
            'bg-popover text-popover-foreground max-h-(--available-height) min-w-[8rem] origin-(--transform-origin) overflow-x-hidden overflow-y-auto rounded-md border p-1 shadow-md outline-none',
            className,
          )}
        >
          <MenuHighlightPrimitive className="absolute inset-0 bg-accent z-0 rounded-sm">
            {children}
          </MenuHighlightPrimitive>
        </MenuPopupPrimitive>
      </MenuPositionerPrimitive>
    </MenuPortal>
  );
}

type MenuGroupProps = MenuGroupPrimitiveProps;

function MenuGroup(props: MenuGroupProps) {
  return <MenuGroupPrimitive {...props} />;
}

type MenuGroupLabelProps = MenuGroupLabelPrimitiveProps & {
  inset?: boolean;
};

function MenuGroupLabel({ className, inset, ...props }: MenuGroupLabelProps) {
  return (
    <MenuGroupLabelPrimitive
      data-inset={inset}
      className={cn(
        'px-2 py-1.5 text-sm font-medium data-[inset]:pl-8',
        className,
      )}
      {...props}
    />
  );
}

type MenuItemProps = MenuItemPrimitiveProps & {
  inset?: boolean;
  variant?: 'default' | 'destructive';
};

function MenuItem({
  className,
  inset,
  variant = 'default',
  disabled,
  ...props
}: MenuItemProps) {
  return (
    <MenuHighlightItemPrimitive
      activeClassName={
        variant === 'destructive'
          ? 'bg-destructive/10 dark:bg-destructive/20'
          : ''
      }
      disabled={disabled}
    >
      <MenuItemPrimitive
        disabled={disabled}
        data-inset={inset}
        data-variant={variant}
        className={cn(
          "focus:text-accent-foreground data-[variant=destructive]:text-destructive data-[variant=destructive]:focus:text-destructive data-[variant=destructive]:*:[svg]:!text-destructive [&_svg:not([class*='text-'])]:text-muted-foreground relative flex cursor-default items-center gap-2 rounded-sm px-2 py-1.5 text-sm outline-hidden select-none data-[disabled=true]:pointer-events-none data-[disabled=true]:opacity-50 data-[inset]:pl-8 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
          className,
        )}
        {...props}
      />
    </MenuHighlightItemPrimitive>
  );
}

type MenuCheckboxItemProps = MenuCheckboxItemPrimitiveProps;

function MenuCheckboxItem({
  className,
  children,
  checked,
  disabled,
  ...props
}: MenuCheckboxItemProps) {
  return (
    <MenuHighlightItemPrimitive disabled={disabled}>
      <MenuCheckboxItemPrimitive
        disabled={disabled}
        className={cn(
          "focus:text-accent-foreground relative flex cursor-default items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm outline-hidden select-none data-[disabled=true]:pointer-events-none data-[disabled=true]:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
          className,
        )}
        checked={checked}
        {...props}
      >
        <span className="pointer-events-none absolute left-2 flex size-3.5 items-center justify-center">
          <MenuCheckboxItemIndicatorPrimitive
            initial={{ opacity: 0, scale: 0.5 }}
            animate={{ opacity: 1, scale: 1 }}
          >
            <CheckIcon className="size-4" />
          </MenuCheckboxItemIndicatorPrimitive>
        </span>
        {children}
      </MenuCheckboxItemPrimitive>
    </MenuHighlightItemPrimitive>
  );
}

type MenuRadioGroupProps = MenuRadioGroupPrimitiveProps;

function MenuRadioGroup(props: MenuRadioGroupProps) {
  return <MenuRadioGroupPrimitive {...props} />;
}

type MenuRadioItemProps = MenuRadioItemPrimitiveProps;

function MenuRadioItem({
  className,
  children,
  disabled,
  ...props
}: MenuRadioItemProps) {
  return (
    <MenuHighlightItemPrimitive disabled={disabled}>
      <MenuRadioItemPrimitive
        disabled={disabled}
        className={cn(
          "focus:text-accent-foreground relative flex cursor-default items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm outline-hidden select-none data-[disabled=true]:pointer-events-none data-[disabled=true]:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
          className,
        )}
        {...props}
      >
        <span className="pointer-events-none absolute left-2 flex size-3.5 items-center justify-center">
          <MenuRadioItemIndicatorPrimitive layoutId="dropdown-menu-item-indicator-radio">
            <CircleIcon className="size-2 fill-current" />
          </MenuRadioItemIndicatorPrimitive>
        </span>
        {children}
      </MenuRadioItemPrimitive>
    </MenuHighlightItemPrimitive>
  );
}

type MenuSeparatorProps = MenuSeparatorPrimitiveProps;

function MenuSeparator({ className, ...props }: MenuSeparatorProps) {
  return (
    <MenuSeparatorPrimitive
      className={cn('bg-border -mx-1 my-1 h-px', className)}
      {...props}
    />
  );
}

type MenuShortcutProps = MenuShortcutPrimitiveProps;

function MenuShortcut({ className, ...props }: MenuShortcutProps) {
  return (
    <MenuShortcutPrimitive
      className={cn(
        'text-muted-foreground ml-auto text-xs tracking-widest',
        className,
      )}
      {...props}
    />
  );
}

type MenuArrowProps = MenuArrowPrimitiveProps;

function MenuArrow(props: MenuArrowProps) {
  return <MenuArrowPrimitive {...props} />;
}

type MenuSubmenuProps = MenuSubmenuPrimitiveProps;

function MenuSubmenu(props: MenuSubmenuProps) {
  return <MenuSubmenuPrimitive {...props} />;
}

type MenuSubmenuTriggerProps = MenuSubmenuTriggerPrimitiveProps & {
  inset?: boolean;
  children?: React.ReactNode;
};

function MenuSubmenuTrigger({
  disabled,
  className,
  inset,
  children,
  ...props
}: MenuSubmenuTriggerProps) {
  return (
    <MenuHighlightItemPrimitive disabled={disabled}>
      <MenuSubmenuTriggerPrimitive
        disabled={disabled}
        data-inset={inset}
        className={cn(
          'focus:text-accent-foreground data-[state=open]:text-accent-foreground flex cursor-default items-center rounded-sm px-2 py-1.5 text-sm outline-hidden select-none data-[inset]:pl-8',
          'aria-[expanded=true]:[&_[data-slot=chevron]]:rotate-90 [&_[data-slot=chevron]]:transition-transform [&_[data-slot=chevron]]:duration-300 [&_[data-slot=chevron]]:ease-in-out',
          className,
        )}
        {...props}
      >
        {children}
        <ChevronRightIcon data-slot="chevron" className="ml-auto size-4" />
      </MenuSubmenuTriggerPrimitive>
    </MenuHighlightItemPrimitive>
  );
}

type MenuSubmenuPanelProps = MenuPopupPrimitiveProps &
  MenuPositionerPrimitiveProps;

function MenuSubmenuPanel({
  className,
  finalFocus,
  id,
  children,
  sideOffset = 4,
  transition = { duration: 0.2 },
  ...props
}: MenuSubmenuPanelProps) {
  return (
    <MenuPortal>
      <MenuPositionerPrimitive
        className="z-50"
        sideOffset={sideOffset}
        {...props}
      >
        <MenuPopupPrimitive
          finalFocus={finalFocus}
          transition={transition}
          id={id}
          className={cn(
            'bg-popover text-popover-foreground max-h-(--available-height) min-w-[8rem] origin-(--transform-origin) overflow-x-hidden overflow-y-auto rounded-md border p-1 shadow-md',
            className,
          )}
        >
          {children}
        </MenuPopupPrimitive>
      </MenuPositionerPrimitive>
    </MenuPortal>
  );
}

export {
  Menu,
  MenuTrigger,
  MenuPortal,
  MenuPanel,
  MenuGroup,
  MenuGroupLabel,
  MenuItem,
  MenuCheckboxItem,
  MenuRadioGroup,
  MenuRadioItem,
  MenuSeparator,
  MenuShortcut,
  MenuArrow,
  MenuSubmenu,
  MenuSubmenuTrigger,
  MenuSubmenuPanel,
  type MenuProps,
  type MenuTriggerProps,
  type MenuPortalProps,
  type MenuPanelProps,
  type MenuGroupProps,
  type MenuGroupLabelProps,
  type MenuItemProps,
  type MenuCheckboxItemProps,
  type MenuRadioGroupProps,
  type MenuRadioItemProps,
  type MenuSeparatorProps,
  type MenuShortcutProps,
  type MenuArrowProps,
  type MenuSubmenuProps,
  type MenuSubmenuTriggerProps,
  type MenuSubmenuPanelProps,
};
```

---

### Popover <a name="popover"></a>

An accessible popup anchored to a button.

- **Category**: Base
- **Registry Key**: `components-base-popover`

#### Installation

```bash
npx shadcn@latest add components-base-popover
```

#### How to Use

```tsx
<Popover>
  <PopoverTrigger>Open popover</PopoverTrigger>
  <PopoverPanel>
    <div>Popover panel</div>
  </PopoverPanel>
</Popover>
```

#### Demo Code

##### File: `registry/demo/components/base/popover/index.tsx`

```tsx
import {
  Popover,
  PopoverTrigger,
  PopoverPanel,
} from '@/components/animate-ui/components/base/popover';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';

interface BasePopoverDemoProps {
  side?: 'top' | 'bottom' | 'left' | 'right';
  sideOffset?: number;
  align?: 'start' | 'center' | 'end';
  alignOffset?: number;
}

export const BasePopoverDemo = ({
  side,
  sideOffset,
  align,
  alignOffset,
}: BasePopoverDemoProps) => {
  return (
    <Popover>
      <PopoverTrigger
        render={<Button variant="outline">Open popover</Button>}
      />
      <PopoverPanel
        side={side}
        sideOffset={sideOffset}
        align={align}
        alignOffset={alignOffset}
        className="w-80"
      >
        <div className="grid gap-4">
          <div className="space-y-2">
            <h4 className="leading-none font-medium">Dimensions</h4>
            <p className="text-muted-foreground text-sm">
              Set the dimensions for the layer.
            </p>
          </div>
          <div className="grid gap-2">
            <div className="grid grid-cols-3 items-center gap-4">
              <Label htmlFor="width">Width</Label>
              <Input
                id="width"
                defaultValue="100%"
                className="col-span-2 h-8"
              />
            </div>
            <div className="grid grid-cols-3 items-center gap-4">
              <Label htmlFor="maxWidth">Max. width</Label>
              <Input
                id="maxWidth"
                defaultValue="300px"
                className="col-span-2 h-8"
              />
            </div>
            <div className="grid grid-cols-3 items-center gap-4">
              <Label htmlFor="height">Height</Label>
              <Input
                id="height"
                defaultValue="25px"
                className="col-span-2 h-8"
              />
            </div>
            <div className="grid grid-cols-3 items-center gap-4">
              <Label htmlFor="maxHeight">Max. height</Label>
              <Input
                id="maxHeight"
                defaultValue="none"
                className="col-span-2 h-8"
              />
            </div>
          </div>
        </div>
      </PopoverPanel>
    </Popover>
  );
};
```

#### Component Source Code

##### File: `registry/components/base/popover/index.tsx`

```tsx
import * as React from 'react';

import {
  Popover as PopoverPrimitive,
  PopoverTrigger as PopoverTriggerPrimitive,
  PopoverPositioner as PopoverPositionerPrimitive,
  PopoverPopup as PopoverPopupPrimitive,
  PopoverPortal as PopoverPortalPrimitive,
  PopoverClose as PopoverClosePrimitive,
  PopoverBackdrop as PopoverBackdropPrimitive,
  PopoverTitle as PopoverTitlePrimitive,
  PopoverDescription as PopoverDescriptionPrimitive,
  type PopoverProps as PopoverPrimitiveProps,
  type PopoverTriggerProps as PopoverTriggerPrimitiveProps,
  type PopoverPositionerProps as PopoverPositionerPrimitiveProps,
  type PopoverPopupProps as PopoverPopupPrimitiveProps,
  type PopoverCloseProps as PopoverClosePrimitiveProps,
  type PopoverBackdropProps as PopoverBackdropPrimitiveProps,
  type PopoverTitleProps as PopoverTitlePrimitiveProps,
  type PopoverDescriptionProps as PopoverDescriptionPrimitiveProps,
} from '@/components/animate-ui/primitives/base/popover';
import { cn } from '@/lib/utils';

type PopoverProps = PopoverPrimitiveProps;

function Popover(props: PopoverProps) {
  return <PopoverPrimitive {...props} />;
}

type PopoverTriggerProps = PopoverTriggerPrimitiveProps;

function PopoverTrigger(props: PopoverTriggerProps) {
  return <PopoverTriggerPrimitive {...props} />;
}

type PopoverPanelProps = PopoverPositionerPrimitiveProps &
  PopoverPopupPrimitiveProps;

function PopoverPanel({
  className,
  align = 'center',
  sideOffset = 4,
  initialFocus,
  finalFocus,
  style,
  children,
  ...props
}: PopoverPanelProps) {
  return (
    <PopoverPortalPrimitive>
      <PopoverPositionerPrimitive
        align={align}
        sideOffset={sideOffset}
        className="z-50"
        {...props}
      >
        <PopoverPopupPrimitive
          initialFocus={initialFocus}
          finalFocus={finalFocus}
          className={cn(
            'bg-popover text-popover-foreground w-72 rounded-md border p-4 shadow-md outline-hidden origin-(--transform-origin)',
            className,
          )}
          style={style}
        >
          {children}
        </PopoverPopupPrimitive>
      </PopoverPositionerPrimitive>
    </PopoverPortalPrimitive>
  );
}

type PopoverCloseProps = PopoverClosePrimitiveProps;

function PopoverClose(props: PopoverCloseProps) {
  return <PopoverClosePrimitive {...props} />;
}

type PopoverBackdropProps = PopoverBackdropPrimitiveProps;

function PopoverBackdrop(props: PopoverBackdropProps) {
  return <PopoverBackdropPrimitive {...props} />;
}

type PopoverTitleProps = PopoverTitlePrimitiveProps;

function PopoverTitle(props: PopoverTitleProps) {
  return <PopoverTitlePrimitive {...props} />;
}

type PopoverDescriptionProps = PopoverDescriptionPrimitiveProps;

function PopoverDescription(props: PopoverDescriptionProps) {
  return <PopoverDescriptionPrimitive {...props} />;
}

export {
  Popover,
  PopoverTrigger,
  PopoverPanel,
  PopoverClose,
  PopoverBackdrop,
  PopoverTitle,
  PopoverDescription,
  type PopoverProps,
  type PopoverTriggerProps,
  type PopoverPanelProps,
  type PopoverCloseProps,
  type PopoverBackdropProps,
  type PopoverTitleProps,
  type PopoverDescriptionProps,
};
```

---

### Preview Card <a name="preview-card"></a>

A popup that appears when a link is hovered, showing a preview for sighted users.

- **Category**: Base
- **Registry Key**: `components-base-preview-card`

#### Installation

```bash
npx shadcn@latest add components-base-preview-card
```

#### How to Use

```tsx
<PreviewCard>
  <PreviewCardTrigger>Hover me</PreviewCardTrigger>
  <PreviewCardPanel>
    <p>Preview card content</p>
  </PreviewCardPanel>
</PreviewCard>
```

#### Demo Code

##### File: `registry/demo/components/base/preview-card/index.tsx`

```tsx
import {
  PreviewCard,
  PreviewCardTrigger,
  PreviewCardPanel,
} from '@/components/animate-ui/components/base/preview-card';

interface BasePreviewCardDemoProps {
  side?: 'top' | 'bottom' | 'left' | 'right';
  sideOffset?: number;
  align?: 'start' | 'center' | 'end';
  alignOffset?: number;
  followCursor?: boolean | 'x' | 'y';
}

export const BasePreviewCardDemo = ({
  side,
  sideOffset,
  align,
  alignOffset,
  followCursor,
}: BasePreviewCardDemoProps) => {
  return (
    <PreviewCard followCursor={followCursor}>
      <PreviewCardTrigger
        render={
          <a
            className="size-12 border rounded-full overflow-hidden"
            href="https://twitter.com/animate_ui"
            target="_blank"
            rel="noreferrer noopener"
          >
            <img
              src="https://pbs.twimg.com/profile_images/1950218390741618688/72447Y7e_400x400.jpg"
              alt="Animate UI"
            />
          </a>
        }
      />

      <PreviewCardPanel
        side={side}
        sideOffset={sideOffset}
        align={align}
        alignOffset={alignOffset}
        className="w-80"
      >
        <div className="flex flex-col gap-4">
          <img
            className="size-16 rounded-full overflow-hidden border"
            src="https://pbs.twimg.com/profile_images/1950218390741618688/72447Y7e_400x400.jpg"
            alt="Animate UI"
          />
          <div className="flex flex-col gap-4">
            <div>
              <div className="font-bold">Animate UI</div>
              <div className="text-sm text-muted-foreground">@animate_ui</div>
            </div>
            <div className="text-sm text-muted-foreground">
              A fully animated, open-source component distribution built with
              React, TypeScript, Tailwind CSS, and Motion.
            </div>
            <div className="flex gap-4">
              <div className="flex gap-1 text-sm items-center">
                <div className="font-bold">0</div>{' '}
                <div className="text-muted-foreground">Following</div>
              </div>
              <div className="flex gap-1 text-sm items-center">
                <div className="font-bold">2,900</div>{' '}
                <div className="text-muted-foreground">Followers</div>
              </div>
            </div>
          </div>
        </div>
      </PreviewCardPanel>
    </PreviewCard>
  );
};
```

#### Component Source Code

##### File: `registry/components/base/preview-card/index.tsx`

```tsx
import * as React from 'react';

import {
  PreviewCard as PreviewCardPrimitive,
  PreviewCardTrigger as PreviewCardTriggerPrimitive,
  PreviewCardPortal as PreviewCardPortalPrimitive,
  PreviewCardPositioner as PreviewCardPositionerPrimitive,
  PreviewCardPopup as PreviewCardPopupPrimitive,
  PreviewCardBackdrop as PreviewCardBackdropPrimitive,
  type PreviewCardProps as PreviewCardPrimitiveProps,
  type PreviewCardTriggerProps as PreviewCardTriggerPrimitiveProps,
  type PreviewCardPositionerProps as PreviewCardPositionerPrimitiveProps,
  type PreviewCardPopupProps as PreviewCardPopupPrimitiveProps,
  type PreviewCardBackdropProps as PreviewCardBackdropPrimitiveProps,
} from '@/components/animate-ui/primitives/base/preview-card';
import { cn } from '@/lib/utils';

type PreviewCardProps = PreviewCardPrimitiveProps;

function PreviewCard(props: PreviewCardProps) {
  return <PreviewCardPrimitive {...props} />;
}

type PreviewCardTriggerProps = PreviewCardTriggerPrimitiveProps;

function PreviewCardTrigger(props: PreviewCardTriggerProps) {
  return <PreviewCardTriggerPrimitive {...props} />;
}

type PreviewCardPanelProps = PreviewCardPositionerPrimitiveProps &
  PreviewCardPopupPrimitiveProps;

function PreviewCardPanel({
  className,
  align = 'center',
  sideOffset = 4,
  style,
  children,
  ...props
}: PreviewCardPanelProps) {
  return (
    <PreviewCardPortalPrimitive>
      <PreviewCardPositionerPrimitive
        align={align}
        sideOffset={sideOffset}
        className="z-50"
        {...props}
      >
        <PreviewCardPopupPrimitive
          className={cn(
            'bg-popover text-popover-foreground w-64 origin-(--transform-origin) rounded-md border p-4 shadow-md outline-hidden',
            className,
          )}
          style={style}
        >
          {children}
        </PreviewCardPopupPrimitive>
      </PreviewCardPositionerPrimitive>
    </PreviewCardPortalPrimitive>
  );
}

type PreviewCardBackdropProps = PreviewCardBackdropPrimitiveProps;

function PreviewCardBackdrop(props: PreviewCardBackdropProps) {
  return <PreviewCardBackdropPrimitive {...props} />;
}

export {
  PreviewCard,
  PreviewCardTrigger,
  PreviewCardPanel,
  PreviewCardBackdrop,
  type PreviewCardProps,
  type PreviewCardTriggerProps,
  type PreviewCardPanelProps,
  type PreviewCardBackdropProps,
};
```

---

### Preview Link Card <a name="preview-link-card"></a>

Displays a preview image of a link when hovered.

- **Category**: Base
- **Registry Key**: `components-base-preview-link-card`

#### Installation

```bash
npx shadcn@latest add components-base-preview-link-card
```

#### How to Use

```tsx
<PreviewLinkCard href="https://animate-ui.com/docs">
  <PreviewLinkCardTrigger>Animate UI Docs</PreviewLinkCardTrigger>
  <PreviewLinkCardPanel>
    <PreviewLinkCardImage alt="Preview link card content" />
  </PreviewLinkCardPanel>
</PreviewLinkCard>
```

#### Demo Code

##### File: `registry/demo/components/base/preview-link-card/index.tsx`

```tsx
import {
  PreviewLinkCard,
  PreviewLinkCardTrigger,
  PreviewLinkCardPanel,
  PreviewLinkCardImage,
} from '@/components/animate-ui/components/base/preview-link-card';

interface BasePreviewLinkCardDemoProps {
  side?: 'top' | 'bottom' | 'left' | 'right';
  sideOffset?: number;
  align?: 'start' | 'center' | 'end';
  alignOffset?: number;
  followCursor?: boolean | 'x' | 'y';
  href: string;
}

export const BasePreviewLinkCardDemo = ({
  side,
  sideOffset,
  align,
  alignOffset,
  followCursor,
  href,
}: BasePreviewLinkCardDemoProps) => {
  return (
    <p className="text-muted-foreground">
      Read the{' '}
      <PreviewLinkCard href={href} followCursor={followCursor}>
        <PreviewLinkCardTrigger
          target="_blank"
          className="underline text-foreground"
        >
          Animate UI Docs
        </PreviewLinkCardTrigger>
        <PreviewLinkCardPanel
          side={side}
          sideOffset={sideOffset}
          align={align}
          alignOffset={alignOffset}
          target="_blank"
        >
          <PreviewLinkCardImage alt="Animate UI Docs" />
        </PreviewLinkCardPanel>
      </PreviewLinkCard>{' '}
      — hover to preview, click to dive in.
    </p>
  );
};
```

#### Component Source Code

##### File: `registry/components/base/preview-link-card/index.tsx`

```tsx
import * as React from 'react';

import {
  PreviewLinkCard as PreviewLinkCardPrimitive,
  PreviewLinkCardTrigger as PreviewLinkCardTriggerPrimitive,
  PreviewLinkCardPortal as PreviewLinkCardPortalPrimitive,
  PreviewLinkCardPositioner as PreviewLinkCardPositionerPrimitive,
  PreviewLinkCardPopup as PreviewLinkCardPopupPrimitive,
  PreviewLinkCardImage as PreviewLinkCardImagePrimitive,
  PreviewLinkCardBackdrop as PreviewLinkCardBackdropPrimitive,
  type PreviewLinkCardProps as PreviewLinkCardPrimitiveProps,
  type PreviewLinkCardTriggerProps as PreviewLinkCardTriggerPrimitiveProps,
  type PreviewLinkCardPositionerProps as PreviewLinkCardPositionerPrimitiveProps,
  type PreviewLinkCardPopupProps as PreviewLinkCardPopupPrimitiveProps,
  type PreviewLinkCardImageProps as PreviewLinkCardImagePrimitiveProps,
  type PreviewLinkCardBackdropProps as PreviewLinkCardBackdropPrimitiveProps,
} from '@/components/animate-ui/primitives/base/preview-link-card';
import { cn } from '@/lib/utils';

type PreviewLinkCardProps = PreviewLinkCardPrimitiveProps;

function PreviewLinkCard(props: PreviewLinkCardProps) {
  return <PreviewLinkCardPrimitive {...props} />;
}

type PreviewLinkCardTriggerProps = PreviewLinkCardTriggerPrimitiveProps;

function PreviewLinkCardTrigger(props: PreviewLinkCardTriggerProps) {
  return <PreviewLinkCardTriggerPrimitive {...props} />;
}

type PreviewLinkCardPanelProps = PreviewLinkCardPositionerPrimitiveProps &
  PreviewLinkCardPopupPrimitiveProps;

function PreviewLinkCardPanel({
  className,
  align = 'center',
  sideOffset = 4,
  style,
  children,
  ...props
}: PreviewLinkCardPanelProps) {
  return (
    <PreviewLinkCardPortalPrimitive>
      <PreviewLinkCardPositionerPrimitive
        align={align}
        sideOffset={sideOffset}
        className="z-50"
        {...props}
      >
        <PreviewLinkCardPopupPrimitive
          className={cn(
            'border origin-(--transform-origin) rounded-md shadow-md outline-hidden overflow-hidden',
            className,
          )}
          style={style}
        >
          {children}
        </PreviewLinkCardPopupPrimitive>
      </PreviewLinkCardPositionerPrimitive>
    </PreviewLinkCardPortalPrimitive>
  );
}

type PreviewLinkCardBackdropProps = PreviewLinkCardBackdropPrimitiveProps;

function PreviewLinkCardBackdrop(props: PreviewLinkCardBackdropProps) {
  return <PreviewLinkCardBackdropPrimitive {...props} />;
}

type PreviewLinkCardImageProps = PreviewLinkCardImagePrimitiveProps;

function PreviewLinkCardImage(props: PreviewLinkCardImageProps) {
  return <PreviewLinkCardImagePrimitive {...props} />;
}

export {
  PreviewLinkCard,
  PreviewLinkCardTrigger,
  PreviewLinkCardPanel,
  PreviewLinkCardBackdrop,
  PreviewLinkCardImage,
  type PreviewLinkCardProps,
  type PreviewLinkCardTriggerProps,
  type PreviewLinkCardPanelProps,
  type PreviewLinkCardBackdropProps,
  type PreviewLinkCardImageProps,
};
```

---

### Progress <a name="progress"></a>

Displays the status of a task that takes a long time.

- **Category**: Base
- **Registry Key**: `components-base-progress`

#### Installation

```bash
npx shadcn@latest add components-base-progress
```

#### How to Use

```tsx
<Progress>
  <ProgressLabel>Export data</ProgressLabel>
  <ProgressValue />
  <ProgressTrack />
</Progress>
```

#### Demo Code

##### File: `registry/demo/components/base/progress/index.tsx`

```tsx
'use client';

import * as React from 'react';
import {
  Progress,
  ProgressLabel,
  ProgressTrack,
  ProgressValue,
} from '@/components/animate-ui/components/base/progress';

export const BaseProgressDemo = () => {
  const [progress, setProgress] = React.useState(0);

  React.useEffect(() => {
    const timer = setInterval(() => {
      setProgress((prev) => {
        if (prev >= 100) return 100;
        return prev + 25;
      });
    }, 2000);
    return () => clearInterval(timer);
  }, []);

  React.useEffect(() => {
    if (progress >= 100) setTimeout(() => setProgress(0), 4000);
  }, [progress]);

  return (
    <Progress value={progress} className="w-[300px] space-y-2">
      <div className="flex items-center justify-between gap-1">
        <ProgressLabel>Export data</ProgressLabel>
        <span className="text-sm">
          <ProgressValue /> %
        </span>
      </div>
      <ProgressTrack />
    </Progress>
  );
};
```

#### Component Source Code

##### File: `registry/components/base/progress/index.tsx`

```tsx
import * as React from 'react';

import {
  Progress as ProgressPrimitive,
  ProgressTrack as ProgressTrackPrimitive,
  ProgressIndicator as ProgressIndicatorPrimitive,
  ProgressLabel as ProgressLabelPrimitive,
  ProgressValue as ProgressValuePrimitive,
  type ProgressProps as ProgressPrimitiveProps,
  type ProgressTrackProps as ProgressTrackPrimitiveProps,
  type ProgressLabelProps as ProgressLabelPrimitiveProps,
  type ProgressValueProps as ProgressValuePrimitiveProps,
} from '@/components/animate-ui/primitives/base/progress';
import { cn } from '@/lib/utils';

type ProgressProps = ProgressPrimitiveProps;

function Progress(props: ProgressProps) {
  return <ProgressPrimitive {...props} />;
}

type ProgressTrackProps = ProgressTrackPrimitiveProps;

function ProgressTrack({ className, ...props }: ProgressTrackProps) {
  return (
    <ProgressTrackPrimitive
      className={cn(
        'bg-primary/20 relative h-2 w-full overflow-hidden rounded-full',
        className,
      )}
      {...props}
    >
      <ProgressIndicatorPrimitive className="bg-primary rounded-full h-full w-full flex-1" />
    </ProgressTrackPrimitive>
  );
}

type ProgressLabelProps = ProgressLabelPrimitiveProps;

function ProgressLabel(props: ProgressLabelProps) {
  return <ProgressLabelPrimitive className="text-sm font-medium" {...props} />;
}

type ProgressValueProps = ProgressValuePrimitiveProps;

function ProgressValue(props: ProgressValueProps) {
  return <ProgressValuePrimitive className="text-sm" {...props} />;
}

export {
  Progress,
  ProgressTrack,
  ProgressLabel,
  ProgressValue,
  type ProgressProps,
  type ProgressTrackProps,
  type ProgressLabelProps,
  type ProgressValueProps,
};
```

---

### Radio <a name="radio"></a>

An easily stylable radio button component.

- **Category**: Base
- **Registry Key**: `components-base-radio`

#### Installation

```bash
npx shadcn@latest add components-base-radio
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
<RadioGroup>
  <Radio value="1" />
  <Radio value="2" />
  <Radio value="3" />
</RadioGroup>
```

#### Demo Code

##### File: `registry/demo/components/base/radio/index.tsx`

```tsx
import * as React from 'react';

import { RadioGroup, Radio } from '@/components/animate-ui/components/base/radio';
import { Label } from '@/components/ui/label';

export const BaseRadioDemo = () => {
  return (
    <RadioGroup defaultValue="default">
      <Label className="flex items-center gap-x-3">
        <Radio value="default" />
        Default
      </Label>
      <Label className="flex items-center gap-x-3">
        <Radio value="comfortable" />
        Comfortable
      </Label>
      <Label className="flex items-center gap-x-3">
        <Radio value="compact" />
        Compact
      </Label>
    </RadioGroup>
  );
};
```

#### Component Source Code

##### File: `registry/components/base/radio/index.tsx`

```tsx
import * as React from 'react';
import { CircleIcon } from 'lucide-react';

import {
  RadioGroup as RadioGroupPrimitive,
  Radio as RadioPrimitive,
  RadioIndicator as RadioIndicatorPrimitive,
  type RadioGroupProps as RadioGroupPrimitiveProps,
  type RadioProps as RadioPrimitiveProps,
} from '@/components/animate-ui/primitives/base/radio';
import { cn } from '@/lib/utils';

type RadioGroupProps = RadioGroupPrimitiveProps;

function RadioGroup({ className, ...props }: RadioGroupProps) {
  return (
    <RadioGroupPrimitive className={cn('grid gap-3', className)} {...props} />
  );
}

type RadioProps = RadioPrimitiveProps;

function Radio({ className, ...props }: RadioProps) {
  return (
    <RadioPrimitive
      className={cn(
        'border-input text-primary focus-visible:border-ring focus-visible:ring-ring/50 aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive dark:bg-input/30 aspect-square size-4 shrink-0 rounded-full border shadow-xs transition-[color,box-shadow] outline-none focus-visible:ring-[3px] disabled:cursor-not-allowed disabled:opacity-50',
        className,
      )}
      {...props}
    >
      <RadioIndicatorPrimitive className="relative flex items-center justify-center">
        <CircleIcon className="fill-primary absolute top-1/2 left-1/2 size-2 -translate-x-1/2 -translate-y-1/2" />
      </RadioIndicatorPrimitive>
    </RadioPrimitive>
  );
}

export { RadioGroup, Radio, type RadioGroupProps, type RadioProps };
```

---

### Switch <a name="switch"></a>

A control that indicates whether a setting is on or off.

- **Category**: Base
- **Registry Key**: `components-base-switch`

#### Installation

```bash
npx shadcn@latest add components-base-switch
```

#### How to Use

```tsx
<Switch />
```

#### Demo Code

##### File: `registry/demo/components/base/switch/index.tsx`

```tsx
import { Label } from '@/components/ui/label';
import { Switch } from '@/components/animate-ui/components/base/switch';

export function BaseSwitchDemo() {
  return (
    <Label className="flex items-center gap-x-3">
      <Switch />
      Airplane Mode
    </Label>
  );
}
```

#### Component Source Code

##### File: `registry/components/base/switch/index.tsx`

```tsx
import * as React from 'react';

import {
  Switch as SwitchPrimitive,
  SwitchThumb as SwitchThumbPrimitive,
  SwitchIcon as SwitchIconPrimitive,
  type SwitchProps as SwitchPrimitiveProps,
} from '@/components/animate-ui/primitives/base/switch';
import { cn } from '@/lib/utils';

type SwitchProps = SwitchPrimitiveProps & {
  pressedWidth?: number;
  startIcon?: React.ReactElement;
  endIcon?: React.ReactElement;
  thumbIcon?: React.ReactElement;
};

function Switch({
  className,
  pressedWidth = 19,
  startIcon,
  endIcon,
  thumbIcon,
  ...props
}: SwitchProps) {
  return (
    <SwitchPrimitive
      className={cn(
        'relative peer focus-visible:border-ring focus-visible:ring-ring/50 flex h-5 w-8 px-px shrink-0 items-center justify-start rounded-full border border-transparent shadow-xs outline-none focus-visible:ring-[3px] disabled:cursor-not-allowed disabled:opacity-50',
        'data-[checked]:bg-primary data-[unchecked]:bg-input dark:data-[unchecked]:bg-input/80 data-[checked]:justify-end',
        className,
      )}
      {...props}
    >
      <SwitchThumbPrimitive
        className={cn(
          'relative z-10 bg-background dark:data-[unchecked]:bg-foreground dark:data-[checked]:bg-primary-foreground pointer-events-none block size-4 rounded-full ring-0',
        )}
        pressedAnimation={{ width: pressedWidth }}
      >
        {thumbIcon && (
          <SwitchIconPrimitive
            position="thumb"
            className="absolute [&_svg]:size-[9px] left-1/2 top-1/2 -translate-1/2 dark:text-neutral-500 text-neutral-400"
          >
            {thumbIcon}
          </SwitchIconPrimitive>
        )}
      </SwitchThumbPrimitive>

      {startIcon && (
        <SwitchIconPrimitive
          position="left"
          className="absolute [&_svg]:size-[9px] left-0.5 top-1/2 -translate-y-1/2 dark:text-neutral-500 text-neutral-400"
        >
          {startIcon}
        </SwitchIconPrimitive>
      )}
      {endIcon && (
        <SwitchIconPrimitive
          position="right"
          className="absolute [&_svg]:size-[9px] right-0.5 top-1/2 -translate-y-1/2 dark:text-neutral-400 text-neutral-500"
        >
          {endIcon}
        </SwitchIconPrimitive>
      )}
    </SwitchPrimitive>
  );
}

export { Switch, type SwitchProps };
```

---

### Tabs <a name="tabs"></a>

A component for toggling between related panels on the same page.

- **Category**: Base
- **Registry Key**: `components-base-tabs`

#### Installation

```bash
npx shadcn@latest add components-base-tabs
```

#### How to Use

```tsx
<Tabs>
  <TabsList>
    <TabsTab value="account">Account</TabsTab>
    <TabsTab value="password">Password</TabsTab>
  </TabsList>
  <TabsPanels>
    <TabsPanel value="account">Make changes to your account here.</TabsPanel>
    <TabsPanel value="password">Change your password here.</TabsPanel>
  </TabsPanels>
</Tabs>
```

#### Demo Code

##### File: `registry/demo/components/base/tabs/index.tsx`

```tsx
import {
  Tabs,
  TabsPanel,
  TabsPanels,
  TabsList,
  TabsTab,
} from '@/components/animate-ui/components/base/tabs';
import { Button } from '@/components/ui/button';
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';

export function BaseTabsDemo() {
  return (
    <div className="flex w-full max-w-sm flex-col gap-6">
      <Tabs defaultValue="account">
        <TabsList>
          <TabsTab value="account">Account</TabsTab>
          <TabsTab value="password">Password</TabsTab>
        </TabsList>
        <Card className="shadow-none py-0">
          <TabsPanels className="py-6">
            <TabsPanel value="account" className="flex flex-col gap-6">
              <CardHeader>
                <CardTitle>Account</CardTitle>
                <CardDescription>
                  Make changes to your account here. Click save when you&apos;re
                  done.
                </CardDescription>
              </CardHeader>
              <CardContent className="grid gap-6">
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-name">Name</Label>
                  <Input id="tabs-demo-name" defaultValue="Pedro Duarte" />
                </div>
              </CardContent>
              <CardFooter>
                <Button>Save changes</Button>
              </CardFooter>
            </TabsPanel>
            <TabsPanel value="password" className="flex flex-col gap-6">
              <CardHeader>
                <CardTitle>Password</CardTitle>
                <CardDescription>
                  Change your password here. After saving, you&apos;ll be logged
                  out.
                </CardDescription>
              </CardHeader>
              <CardContent className="grid gap-6">
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-current">Current password</Label>
                  <Input id="tabs-demo-current" type="password" />
                </div>
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-new">New password</Label>
                  <Input id="tabs-demo-new" type="password" />
                </div>
              </CardContent>
              <CardFooter>
                <Button>Save password</Button>
              </CardFooter>
            </TabsPanel>
          </TabsPanels>
        </Card>
      </Tabs>
    </div>
  );
}
```

#### Component Source Code

##### File: `registry/components/base/tabs/index.tsx`

```tsx
import * as React from 'react';

import {
  Tabs as TabsPrimitive,
  TabsList as TabsListPrimitive,
  TabsTab as TabsTabPrimitive,
  TabsPanel as TabsPanelPrimitive,
  TabsPanels as TabsPanelsPrimitive,
  TabsHighlight as TabsHighlightPrimitive,
  TabsHighlightItem as TabsHighlightItemPrimitive,
  type TabsProps as TabsPrimitiveProps,
  type TabsListProps as TabsListPrimitiveProps,
  type TabsTabProps as TabsTabPrimitiveProps,
  type TabsPanelProps as TabsPanelPrimitiveProps,
  type TabsPanelsProps as TabsPanelsPrimitiveProps,
} from '@/components/animate-ui/primitives/base/tabs';
import { cn } from '@/lib/utils';

type TabsProps = TabsPrimitiveProps;

function Tabs({ className, ...props }: TabsProps) {
  return (
    <TabsPrimitive
      className={cn('flex flex-col gap-2', className)}
      {...props}
    />
  );
}

type TabsListProps = TabsListPrimitiveProps;

function TabsList({ className, ...props }: TabsListProps) {
  return (
    <TabsHighlightPrimitive className="absolute z-0 inset-0 border border-transparent rounded-md bg-background dark:border-input dark:bg-input/30 shadow-sm">
      <TabsListPrimitive
        className={cn(
          'bg-muted text-muted-foreground inline-flex h-9 w-fit items-center justify-center rounded-lg p-[3px]',
          className,
        )}
        {...props}
      />
    </TabsHighlightPrimitive>
  );
}

type TabsTabProps = TabsTabPrimitiveProps;

function TabsTab({ className, ...props }: TabsTabProps) {
  return (
    <TabsHighlightItemPrimitive value={props.value} className="flex-1">
      <TabsTabPrimitive
        className={cn(
          "data-[selected]:text-foreground focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:outline-ring text-muted-foreground inline-flex h-[calc(100%-1px)] flex-1 items-center justify-center gap-1.5 rounded-md w-full px-2 py-1 text-sm font-medium whitespace-nowrap transition-colors duration-500 ease-in-out focus-visible:ring-[3px] focus-visible:outline-1 disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
          className,
        )}
        {...props}
      />
    </TabsHighlightItemPrimitive>
  );
}

type TabsPanelsProps = TabsPanelsPrimitiveProps;

function TabsPanels(props: TabsPanelsProps) {
  return <TabsPanelsPrimitive {...props} />;
}

type TabsPanelProps = TabsPanelPrimitiveProps;

function TabsPanel({ className, ...props }: TabsPanelProps) {
  return (
    <TabsPanelPrimitive
      className={cn('flex-1 outline-none', className)}
      {...props}
    />
  );
}

export {
  Tabs,
  TabsList,
  TabsTab,
  TabsPanels,
  TabsPanel,
  type TabsProps,
  type TabsListProps,
  type TabsTabProps,
  type TabsPanelsProps,
  type TabsPanelProps,
};
```

---

### Toggle Group <a name="toggle-group"></a>

Provides a shared state to a series of toggle buttons.

- **Category**: Base
- **Registry Key**: `components-base-toggle-group`

#### Installation

```bash
npx shadcn@latest add components-base-toggle-group
```

#### How to Use

```tsx
<ToggleGroup defaultValue={['bold']}>
  <Toggle value="bold" />
  <Toggle value="italic" />
  <Toggle value="underline" />
</ToggleGroup>
```

#### Demo Code

##### File: `registry/demo/components/base/toggle-group/index.tsx`

```tsx
import {
  Toggle,
  ToggleGroup,
  type ToggleGroupProps,
} from '@/components/animate-ui/components/base/toggle-group';
import { Bold, Italic, Underline } from 'lucide-react';

interface BaseToggleGroupDemoProps {
  multiple: boolean;
  variant: ToggleGroupProps['variant'];
  size: ToggleGroupProps['size'];
}

export function BaseToggleGroupDemo({
  multiple,
  variant,
  size,
}: BaseToggleGroupDemoProps) {
  return (
    <ToggleGroup multiple={multiple} variant={variant} size={size}>
      <Toggle value="bold" aria-label="Toggle bold">
        <Bold />
      </Toggle>
      <Toggle value="italic" aria-label="Toggle italic">
        <Italic />
      </Toggle>
      <Toggle value="strikethrough" aria-label="Toggle strikethrough">
        <Underline />
      </Toggle>
    </ToggleGroup>
  );
}
```

#### Component Source Code

##### File: `registry/components/base/toggle-group/index.tsx`

```tsx
import * as React from 'react';
import { type VariantProps } from 'class-variance-authority';

import {
  ToggleGroup as ToggleGroupPrimitive,
  Toggle as TogglePrimitive,
  ToggleGroupHighlight as ToggleGroupHighlightPrimitive,
  ToggleHighlight as ToggleHighlightPrimitive,
  useToggleGroup as useToggleGroupPrimitive,
  type ToggleGroupProps as ToggleGroupPrimitiveProps,
  type ToggleProps as TogglePrimitiveProps,
} from '@/components/animate-ui/primitives/base/toggle-group';
import { toggleVariants } from '@/components/animate-ui/components/base/toggle';
import { cn } from '@/lib/utils';
import { getStrictContext } from '@/lib/get-strict-context';

const [ToggleGroupProvider, useToggleGroup] =
  getStrictContext<VariantProps<typeof toggleVariants>>('ToggleGroupContext');

type ToggleGroupProps = ToggleGroupPrimitiveProps &
  VariantProps<typeof toggleVariants>;

function ToggleGroup({
  className,
  variant,
  size,
  children,
  multiple,
  ...props
}: ToggleGroupProps) {
  return (
    <ToggleGroupPrimitive
      data-variant={variant}
      data-size={size}
      className={cn(
        'group/toggle-group flex gap-0.5 w-fit items-center rounded-lg data-[variant=outline]:shadow-xs data-[variant=outline]:border data-[variant=outline]:p-0.5',
        className,
      )}
      multiple={multiple}
      {...props}
    >
      <ToggleGroupProvider value={{ variant, size }}>
        {!multiple ? (
          <ToggleGroupHighlightPrimitive className="bg-accent rounded-md">
            {children}
          </ToggleGroupHighlightPrimitive>
        ) : (
          children
        )}
      </ToggleGroupProvider>
    </ToggleGroupPrimitive>
  );
}

type ToggleProps = TogglePrimitiveProps & VariantProps<typeof toggleVariants>;

function Toggle({ className, children, variant, size, ...props }: ToggleProps) {
  const { variant: contextVariant, size: contextSize } = useToggleGroup();
  const { multiple } = useToggleGroupPrimitive();

  return (
    <ToggleHighlightPrimitive
      value={props.value?.toString()}
      className={cn(multiple && 'bg-accent rounded-md')}
    >
      <TogglePrimitive
        data-variant={contextVariant || variant}
        data-size={contextSize || size}
        className={cn(
          toggleVariants({
            variant: contextVariant || variant,
            size: contextSize || size,
          }),
          'min-w-0 border-0 flex-1 shrink-0 shadow-none rounded-md focus:z-10 focus-visible:z-10',
          className,
        )}
        {...props}
      >
        {children}
      </TogglePrimitive>
    </ToggleHighlightPrimitive>
  );
}

export { ToggleGroup, Toggle, type ToggleGroupProps, type ToggleProps };
```

---

### Toggle <a name="toggle"></a>

A two-state button that can be on or off.

- **Category**: Base
- **Registry Key**: `components-base-toggle`

#### Installation

```bash
npx shadcn@latest add components-base-toggle
```

#### How to Use

```tsx
<Toggle />
```

#### Demo Code

##### File: `registry/demo/components/base/toggle/index.tsx`

```tsx
import { Toggle, type ToggleProps } from '@/components/animate-ui/components/base/toggle';
import { Bold } from 'lucide-react';

interface BaseToggleDemoProps {
  variant: ToggleProps['variant'];
  size: ToggleProps['size'];
}

export function BaseToggleDemo({ variant, size }: BaseToggleDemoProps) {
  return (
    <Toggle aria-label="Toggle italic" variant={variant} size={size}>
      <Bold />
    </Toggle>
  );
}
```

#### Component Source Code

##### File: `registry/components/base/toggle/index.tsx`

```tsx
import * as React from 'react';
import { cva, type VariantProps } from 'class-variance-authority';

import {
  Toggle as TogglePrimitive,
  ToggleItem as ToggleItemPrimitive,
  ToggleHighlight as ToggleHighlightPrimitive,
  type ToggleProps as TogglePrimitiveProps,
  type ToggleItemProps as ToggleItemPrimitiveProps,
} from '@/components/animate-ui/primitives/base/toggle';
import { cn } from '@/lib/utils';

const toggleVariants = cva(
  "inline-flex items-center justify-center gap-2 rounded-md text-sm font-medium hover:bg-muted/40 hover:text-muted-foreground disabled:pointer-events-none disabled:opacity-50 data-[state=on]:text-accent-foreground [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4 [&_svg]:shrink-0 focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px] outline-none transition-[color,background-color,box-shadow] duration-200 ease-in-out aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive whitespace-nowrap",
  {
    variants: {
      variant: {
        default: 'bg-transparent',
        outline:
          'border border-input bg-transparent shadow-xs hover:bg-accent/40 hover:text-accent-foreground',
      },
      size: {
        default: 'h-9 px-2 min-w-9',
        sm: 'h-8 px-1.5 min-w-8',
        lg: 'h-10 px-2.5 min-w-10',
        icon: 'size-9',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  },
);

type ToggleProps = TogglePrimitiveProps &
  ToggleItemPrimitiveProps &
  VariantProps<typeof toggleVariants>;

function Toggle({
  className,
  variant,
  size,
  pressed,
  defaultPressed,
  onPressedChange,
  disabled,
  ...props
}: ToggleProps) {
  return (
    <TogglePrimitive
      pressed={pressed}
      defaultPressed={defaultPressed}
      onPressedChange={onPressedChange}
      disabled={disabled}
      className="relative"
    >
      <ToggleHighlightPrimitive className="bg-accent rounded-md" />
      <ToggleItemPrimitive
        className={cn(toggleVariants({ variant, size, className }))}
        {...props}
      />
    </TogglePrimitive>
  );
}

export { Toggle, toggleVariants, type ToggleProps };
```

---

### Tooltip <a name="tooltip"></a>

A popup that appears when an element is hovered or focused, showing a hint for sighted users.

- **Category**: Base
- **Registry Key**: `components-base-tooltip`

#### Installation

```bash
npx shadcn@latest add components-base-tooltip
```

#### How to Use

```tsx
<Tooltip>
  <TooltipTrigger>Open tooltip</TooltipTrigger>
  <TooltipPanel>
    <p>Tooltip content</p>
  </TooltipPanel>
</Tooltip>
```

#### Demo Code

##### File: `registry/demo/components/base/tooltip/index.tsx`

```tsx
import {
  Tooltip,
  TooltipTrigger,
  TooltipPanel,
  type TooltipPanelProps,
} from '@/components/animate-ui/components/base/tooltip';
import { Button } from '@/components/ui/button';

interface BaseTooltipDemoProps {
  side: TooltipPanelProps['side'];
  sideOffset: TooltipPanelProps['sideOffset'];
  align: TooltipPanelProps['align'];
  alignOffset: TooltipPanelProps['alignOffset'];
  followCursor?: boolean | 'x' | 'y';
}

export function BaseTooltipDemo({
  side,
  sideOffset,
  align,
  alignOffset,
  followCursor,
}: BaseTooltipDemoProps) {
  return (
    <Tooltip followCursor={followCursor}>
      <TooltipTrigger render={<Button variant="outline">Hover</Button>} />
      <TooltipPanel
        side={side}
        sideOffset={sideOffset}
        align={align}
        alignOffset={alignOffset}
      >
        <p>Add to library</p>
      </TooltipPanel>
    </Tooltip>
  );
}
```

#### Component Source Code

##### File: `registry/components/base/tooltip/index.tsx`

```tsx
import * as React from 'react';

import {
  TooltipProvider as TooltipProviderPrimitive,
  Tooltip as TooltipPrimitive,
  TooltipTrigger as TooltipTriggerPrimitive,
  TooltipPositioner as TooltipPositionerPrimitive,
  TooltipPopup as TooltipPopupPrimitive,
  TooltipArrow as TooltipArrowPrimitive,
  TooltipPortal as TooltipPortalPrimitive,
  type TooltipProviderProps as TooltipProviderPrimitiveProps,
  type TooltipProps as TooltipPrimitiveProps,
  type TooltipTriggerProps as TooltipTriggerPrimitiveProps,
  type TooltipPositionerProps as TooltipPositionerPrimitiveProps,
  type TooltipPopupProps as TooltipPopupPrimitiveProps,
} from '@/components/animate-ui/primitives/base/tooltip';
import { cn } from '@/lib/utils';

type TooltipProviderProps = TooltipProviderPrimitiveProps;

function TooltipProvider({ delay = 0, ...props }: TooltipProviderProps) {
  return <TooltipProviderPrimitive delay={delay} {...props} />;
}

type TooltipProps = TooltipPrimitiveProps & {
  delay?: TooltipPrimitiveProps['delay'];
};

function Tooltip({ delay = 0, ...props }: TooltipProps) {
  return (
    <TooltipProvider delay={delay}>
      <TooltipPrimitive {...props} />
    </TooltipProvider>
  );
}

type TooltipTriggerProps = TooltipTriggerPrimitiveProps;

function TooltipTrigger({ ...props }: TooltipTriggerProps) {
  return <TooltipTriggerPrimitive {...props} />;
}

type TooltipPanelProps = TooltipPositionerPrimitiveProps &
  TooltipPopupPrimitiveProps;

function TooltipPanel({
  className,
  sideOffset = 4,
  children,
  style,
  ...props
}: TooltipPanelProps) {
  return (
    <TooltipPortalPrimitive>
      <TooltipPositionerPrimitive
        sideOffset={sideOffset}
        className="z-50"
        {...props}
      >
        <TooltipPopupPrimitive
          className={cn(
            'bg-primary text-primary-foreground w-fit origin-(--transform-origin) rounded-md px-3 py-1.5 text-xs text-balance',
            className,
          )}
          style={style}
        >
          {children}
          <TooltipArrowPrimitive className="bg-primary fill-primary z-50 size-2.5 data-[side='bottom']:-top-[4px] data-[side='right']:-left-[4px] data-[side='left']:-right-[4px] data-[side='inline-start']:-right-[4px] data-[side='inline-end']:-left-[4px] rotate-45 rounded-[2px]" />
        </TooltipPopupPrimitive>
      </TooltipPositionerPrimitive>
    </TooltipPortalPrimitive>
  );
}

export {
  Tooltip,
  TooltipTrigger,
  TooltipPanel,
  type TooltipProps,
  type TooltipTriggerProps,
  type TooltipPanelProps,
};
```

---

## Buttons <a name="buttons"></a>

### Button <a name="button"></a>

An animated button component with a variety of styles.

- **Category**: Buttons
- **Registry Key**: `components-buttons-button`

#### Installation

```bash
npx shadcn@latest add components-buttons-button
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`

#### How to Use

```tsx
<Button>Click me</Button>
```

#### Demo Code

##### File: `registry/demo/components/buttons/button/index.tsx`

```tsx
import { PlusIcon } from 'lucide-react';
import { Button, type ButtonProps } from '@/components/animate-ui/components/buttons/button';

interface ButtonDemoProps {
  variant: ButtonProps['variant'];
  size: ButtonProps['size'];
}

export default function ButtonDemo({ variant, size }: ButtonDemoProps) {
  return (
    <Button variant={variant} size={size}>
      {size === 'icon' ? <PlusIcon /> : 'Click me'}
    </Button>
  );
}
```

#### Component Source Code

##### File: `registry/components/buttons/button/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { cva, type VariantProps } from 'class-variance-authority';

import {
  Button as ButtonPrimitive,
  type ButtonProps as ButtonPrimitiveProps,
} from '@/components/animate-ui/primitives/buttons/button';
import { cn } from '@/lib/utils';

const buttonVariants = cva(
  "inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-md text-sm font-medium transition-[box-shadow,_color,_background-color,_border-color,_outline-color,_text-decoration-color,_fill,_stroke] disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4 shrink-0 [&_svg]:shrink-0 outline-none focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px] aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive",
  {
    variants: {
      variant: {
        default:
          'bg-primary text-primary-foreground shadow-xs hover:bg-primary/90',
        accent: 'bg-accent text-accent-foreground shadow-xs hover:bg-accent/90',
        destructive:
          'bg-destructive text-white shadow-xs hover:bg-destructive/90 focus-visible:ring-destructive/20 dark:focus-visible:ring-destructive/40 dark:bg-destructive/60',
        outline:
          'border bg-background shadow-xs hover:bg-accent hover:text-accent-foreground dark:bg-input/30 dark:border-input dark:hover:bg-input/50',
        secondary:
          'bg-secondary text-secondary-foreground shadow-xs hover:bg-secondary/80',
        ghost:
          'hover:bg-accent hover:text-accent-foreground dark:hover:bg-accent/50',
        link: 'text-primary underline-offset-4 hover:underline',
      },
      size: {
        default: 'h-9 px-4 py-2 has-[>svg]:px-3',
        sm: 'h-8 rounded-md gap-1.5 px-3 has-[>svg]:px-2.5',
        lg: 'h-10 rounded-md px-6 has-[>svg]:px-4',
        icon: 'size-9',
        'icon-sm': 'size-8 rounded-md',
        'icon-lg': 'size-10 rounded-md',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  },
);

type ButtonProps = ButtonPrimitiveProps & VariantProps<typeof buttonVariants>;

function Button({ className, variant, size, ...props }: ButtonProps) {
  return (
    <ButtonPrimitive
      className={cn(buttonVariants({ variant, size, className }))}
      {...props}
    />
  );
}

export { Button, buttonVariants, type ButtonProps };
```

---

### Copy Button <a name="copy"></a>

A copy button component with a variety of styles and animations.

- **Category**: Buttons
- **Registry Key**: `components-buttons-copy`

#### Installation

```bash
npx shadcn@latest add components-buttons-copy
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `lucide-react`
  - `class-variance-authority`

#### How to Use

```tsx
<CopyButton content="Hello world!" />
```

#### Demo Code

##### File: `registry/demo/components/buttons/copy/index.tsx`

```tsx
import {
  CopyButton,
  type CopyButtonProps,
} from '@/components/animate-ui/components/buttons/copy';

interface CopyButtonDemoProps {
  variant: CopyButtonProps['variant'];
  size: CopyButtonProps['size'];
}

export default function CopyButtonDemo({ variant, size }: CopyButtonDemoProps) {
  return <CopyButton variant={variant} size={size} content="Hello world!" />;
}
```

#### Component Source Code

##### File: `registry/components/buttons/copy/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { cva, type VariantProps } from 'class-variance-authority';
import { AnimatePresence, motion } from 'motion/react';
import { CheckIcon, CopyIcon } from 'lucide-react';

import {
  Button as ButtonPrimitive,
  type ButtonProps as ButtonPrimitiveProps,
} from '@/components/animate-ui/primitives/buttons/button';
import { cn } from '@/lib/utils';
import { useControlledState } from '@/hooks/use-controlled-state';

const buttonVariants = cva(
  "flex items-center justify-center rounded-md transition-[box-shadow,_color,_background-color,_border-color,_outline-color,_text-decoration-color,_fill,_stroke] disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4 shrink-0 [&_svg]:shrink-0 outline-none focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px] aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive",
  {
    variants: {
      variant: {
        default:
          'bg-primary text-primary-foreground shadow-xs hover:bg-primary/90',
        accent: 'bg-accent text-accent-foreground shadow-xs hover:bg-accent/90',
        destructive:
          'bg-destructive text-white shadow-xs hover:bg-destructive/90 focus-visible:ring-destructive/20 dark:focus-visible:ring-destructive/40 dark:bg-destructive/60',
        outline:
          'border bg-background shadow-xs hover:bg-accent hover:text-accent-foreground dark:bg-input/30 dark:border-input dark:hover:bg-input/50',
        secondary:
          'bg-secondary text-secondary-foreground shadow-xs hover:bg-secondary/80',
        ghost:
          'hover:bg-accent hover:text-accent-foreground dark:hover:bg-accent/50',
        link: 'text-primary underline-offset-4 hover:underline',
      },
      size: {
        default: 'size-9',
        xs: "size-7 [&_svg:not([class*='size-'])]:size-3.5 rounded-md",
        sm: 'size-8 rounded-md',
        lg: 'size-10 rounded-md',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  },
);

type CopyButtonProps = Omit<ButtonPrimitiveProps, 'children'> &
  VariantProps<typeof buttonVariants> & {
    content: string;
    copied?: boolean;
    onCopiedChange?: (copied: boolean, content?: string) => void;
    delay?: number;
  };

function CopyButton({
  className,
  content,
  copied,
  onCopiedChange,
  onClick,
  variant,
  size,
  delay = 3000,
  ...props
}: CopyButtonProps) {
  const [isCopied, setIsCopied] = useControlledState({
    value: copied,
    onChange: onCopiedChange,
  });

  const handleCopy = React.useCallback(
    (e: React.MouseEvent<HTMLButtonElement>) => {
      onClick?.(e);
      if (copied) return;
      if (content) {
        navigator.clipboard
          .writeText(content)
          .then(() => {
            setIsCopied(true);
            onCopiedChange?.(true, content);
            setTimeout(() => {
              setIsCopied(false);
              onCopiedChange?.(false);
            }, delay);
          })
          .catch((error) => {
            console.error('Error copying command', error);
          });
      }
    },
    [onClick, copied, content, setIsCopied, onCopiedChange, delay],
  );

  const Icon = isCopied ? CheckIcon : CopyIcon;

  return (
    <ButtonPrimitive
      data-slot="copy-button"
      className={cn(buttonVariants({ variant, size, className }))}
      onClick={handleCopy}
      {...props}
    >
      <AnimatePresence mode="popLayout">
        <motion.span
          key={isCopied ? 'check' : 'copy'}
          data-slot="copy-button-icon"
          initial={{ scale: 0, opacity: 0.4, filter: 'blur(4px)' }}
          animate={{ scale: 1, opacity: 1, filter: 'blur(0px)' }}
          exit={{ scale: 0, opacity: 0.4, filter: 'blur(4px)' }}
          transition={{ duration: 0.25 }}
        >
          <Icon />
        </motion.span>
      </AnimatePresence>
    </ButtonPrimitive>
  );
}

export { CopyButton, buttonVariants, type CopyButtonProps };
```

---

### Flip Button <a name="flip"></a>

An animated flip button component with a variety of styles.

- **Category**: Buttons
- **Registry Key**: `components-buttons-flip`

#### Installation

```bash
npx shadcn@latest add components-buttons-flip
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`

#### How to Use

```tsx
<FlipButton>
  <FlipButtonFront>Front</FlipButtonFront>
  <FlipButtonBack>Back</FlipButtonBack>
</FlipButton>
```

#### Demo Code

##### File: `registry/demo/components/buttons/flip/index.tsx`

```tsx
import { PlusIcon } from 'lucide-react';
import {
  FlipButton,
  FlipButtonBack,
  FlipButtonFront,
  type FlipButtonProps,
} from '@/components/animate-ui/components/buttons/flip';

interface ButtonDemoProps {
  frontVariant: FlipButtonProps['variant'];
  frontSize: FlipButtonProps['size'];
  backVariant: FlipButtonProps['variant'];
  backSize: FlipButtonProps['size'];
}

export default function ButtonDemo({
  frontVariant,
  frontSize,
  backVariant,
  backSize,
}: ButtonDemoProps) {
  return (
    <FlipButton>
      <FlipButtonFront variant={frontVariant} size={frontSize}>
        {frontSize === 'icon' ? <PlusIcon /> : 'Front Button'}
      </FlipButtonFront>
      <FlipButtonBack variant={backVariant} size={backSize}>
        {backSize === 'icon' ? <PlusIcon /> : 'Back Button'}
      </FlipButtonBack>
    </FlipButton>
  );
}
```

#### Component Source Code

##### File: `registry/components/buttons/flip/index.tsx`

```tsx
import * as React from 'react';
import { type VariantProps } from 'class-variance-authority';

import {
  FlipButton as FlipButtonPrimitive,
  FlipButtonFront as FlipButtonFrontPrimitive,
  FlipButtonBack as FlipButtonBackPrimitive,
  type FlipButtonProps as FlipButtonPrimitiveProps,
  type FlipButtonFrontProps as FlipButtonFrontPrimitiveProps,
  type FlipButtonBackProps as FlipButtonBackPrimitiveProps,
} from '@/components/animate-ui/primitives/buttons/flip';
import { getStrictContext } from '@/lib/get-strict-context';
import { buttonVariants } from '@/components/animate-ui/components/buttons/button';
import { cn } from '@/lib/utils';

type FlipButtonContextType = VariantProps<typeof buttonVariants>;

const [FlipButtonProvider, useFlipButton] =
  getStrictContext<FlipButtonContextType>('FlipButtonContext');

type FlipButtonProps = FlipButtonPrimitiveProps &
  VariantProps<typeof buttonVariants>;

function FlipButton({ variant, size, ...props }: FlipButtonProps) {
  return (
    <FlipButtonProvider value={{ variant, size }}>
      <FlipButtonPrimitive {...props} />
    </FlipButtonProvider>
  );
}

type FlipButtonFrontProps = FlipButtonFrontPrimitiveProps &
  VariantProps<typeof buttonVariants>;

function FlipButtonFront({
  variant,
  size,
  className,
  ...props
}: FlipButtonFrontProps) {
  const { variant: buttonVariant, size: buttonSize } = useFlipButton();
  return (
    <FlipButtonFrontPrimitive
      className={cn(
        buttonVariants({
          variant: variant ?? buttonVariant,
          size: size ?? buttonSize,
          className,
        }),
      )}
      {...props}
    />
  );
}

type FlipButtonBackProps = FlipButtonBackPrimitiveProps &
  VariantProps<typeof buttonVariants>;

function FlipButtonBack({
  variant,
  size,
  className,
  ...props
}: FlipButtonBackProps) {
  const { variant: buttonVariant, size: buttonSize } = useFlipButton();
  return (
    <FlipButtonBackPrimitive
      className={cn(
        buttonVariants({
          variant: variant ?? buttonVariant,
          size: size ?? buttonSize,
          className,
        }),
      )}
      {...props}
    />
  );
}

export {
  FlipButton,
  FlipButtonFront,
  FlipButtonBack,
  type FlipButtonProps,
  type FlipButtonFrontProps,
  type FlipButtonBackProps,
};
```

---

### GitHub Stars Button <a name="github-stars"></a>

A clickable button that links to a GitHub repository and displays the number of stars.

- **Category**: Buttons
- **Registry Key**: `components-buttons-github-stars`

#### Installation

```bash
npx shadcn@latest add components-buttons-github-stars
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
<GitHubStarsButton username="imskyleen" repo="animate-ui" />
```

#### Demo Code

##### File: `registry/demo/components/buttons/github-stars/index.tsx`

```tsx
import {
  GitHubStarsButton,
  type GitHubStarsButtonProps,
} from '@/components/animate-ui/components/buttons/github-stars';

interface GitHubStarsButtonDemoProps {
  variant: GitHubStarsButtonProps['variant'];
  size: GitHubStarsButtonProps['size'];
}

export default function GitHubStarsButtonDemo({
  variant,
  size,
}: GitHubStarsButtonDemoProps) {
  return (
    <GitHubStarsButton
      variant={variant}
      size={size}
      username="imskyleen"
      repo="animate-ui"
    />
  );
}
```

#### Component Source Code

##### File: `registry/components/buttons/github-stars/index.tsx`

```tsx
import * as React from 'react';
import { cva, type VariantProps } from 'class-variance-authority';
import { StarIcon } from 'lucide-react';

import {
  Button as ButtonPrimitive,
  type ButtonProps as ButtonPrimitiveProps,
} from '@/components/animate-ui/primitives/buttons/button';
import {
  GithubStars,
  GithubStarsIcon,
  GithubStarsLogo,
  GithubStarsNumber,
  GithubStarsParticles,
  type GithubStarsProps,
} from '@/components/animate-ui/primitives/animate/github-stars';
import { cn } from '@/lib/utils';

const buttonVariants = cva(
  "inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-md text-sm font-medium transition-[box-shadow,_color,_background-color,_border-color,_outline-color,_text-decoration-color,_fill,_stroke] disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4 shrink-0 [&_svg]:shrink-0 outline-none focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px] aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive",
  {
    variants: {
      variant: {
        default:
          'bg-primary text-primary-foreground shadow-xs hover:bg-primary/90',
        accent: 'bg-accent text-accent-foreground shadow-xs hover:bg-accent/90',
        outline:
          'border bg-background shadow-xs hover:bg-accent hover:text-accent-foreground dark:bg-input/30 dark:border-input dark:hover:bg-input/50',
        ghost:
          'hover:bg-accent hover:text-accent-foreground dark:hover:bg-accent/50',
      },
      size: {
        default: 'h-9 px-4 py-2 has-[>svg]:px-3',
        sm: 'h-8 rounded-md gap-1.5 px-3 has-[>svg]:px-2.5',
        lg: 'h-10 rounded-md px-6 has-[>svg]:px-4',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  },
);

const buttonStarVariants = cva('', {
  variants: {
    variant: {
      default:
        'fill-neutral-700 stroke-neutral-700 dark:fill-neutral-300 dark:stroke-neutral-300',
      accent:
        'fill-neutral-300 stroke-neutral-300 dark:fill-neutral-700 dark:stroke-neutral-700',
      outline:
        'fill-neutral-300 stroke-neutral-300 dark:fill-neutral-700 dark:stroke-neutral-700',
      ghost:
        'fill-neutral-300 stroke-neutral-300 dark:fill-neutral-700 dark:stroke-neutral-700',
    },
  },
  defaultVariants: {
    variant: 'default',
  },
});

type GitHubStarsButtonProps = Omit<
  ButtonPrimitiveProps & GithubStarsProps,
  'asChild' | 'children'
> &
  VariantProps<typeof buttonVariants>;

function GitHubStarsButton({
  className,
  username,
  repo,
  value,
  delay,
  inView,
  inViewMargin,
  inViewOnce,
  variant,
  size,
  ...props
}: GitHubStarsButtonProps) {
  return (
    <GithubStars
      asChild
      username={username}
      repo={repo}
      value={value}
      delay={delay}
      inView={inView}
      inViewMargin={inViewMargin}
      inViewOnce={inViewOnce}
    >
      <ButtonPrimitive
        className={cn(buttonVariants({ variant, size, className }))}
        {...props}
      >
        <GithubStarsLogo />
        <GithubStarsNumber />
        <GithubStarsParticles className="text-yellow-500">
          <GithubStarsIcon
            icon={StarIcon}
            data-variant={variant}
            className={cn(buttonStarVariants({ variant }))}
            activeClassName="text-yellow-500"
            size={18}
          />
        </GithubStarsParticles>
      </ButtonPrimitive>
    </GithubStars>
  );
}

export { GitHubStarsButton, type GitHubStarsButtonProps };
```

---

### Icon Button <a name="icon"></a>

An icon button that displays particles when clicked.

- **Category**: Buttons
- **Registry Key**: `components-buttons-icon`

#### Installation

```bash
npx shadcn@latest add components-buttons-icon
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`

#### How to Use

```tsx
<IconButton>
  <Icon />
</IconButton>
```

#### Demo Code

##### File: `registry/demo/components/buttons/icon/index.tsx`

```tsx
import {
  IconButton,
  type IconButtonProps,
} from '@/components/animate-ui/components/buttons/icon';
import { StarIcon } from 'lucide-react';

interface IconButtonDemoProps {
  variant: IconButtonProps['variant'];
  size: IconButtonProps['size'];
}

export default function IconButtonDemo({ variant, size }: IconButtonDemoProps) {
  return (
    <IconButton variant={variant} size={size}>
      <StarIcon />
    </IconButton>
  );
}
```

#### Component Source Code

##### File: `registry/components/buttons/icon/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { cva, type VariantProps } from 'class-variance-authority';

import {
  Button as ButtonPrimitive,
  type ButtonProps as ButtonPrimitiveProps,
} from '@/components/animate-ui/primitives/buttons/button';
import { cn } from '@/lib/utils';
import {
  Particles,
  ParticlesEffect,
} from '@/components/animate-ui/primitives/effects/particles';

const buttonVariants = cva(
  "flex items-center justify-center rounded-md transition-[box-shadow,_color,_background-color,_border-color,_outline-color,_text-decoration-color,_fill,_stroke] disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4 shrink-0 [&_svg]:shrink-0 outline-none focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px] aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive",
  {
    variants: {
      variant: {
        default:
          'bg-primary text-primary-foreground shadow-xs hover:bg-primary/90',
        accent: 'bg-accent text-accent-foreground shadow-xs hover:bg-accent/90',
        destructive:
          'bg-destructive text-white shadow-xs hover:bg-destructive/90 focus-visible:ring-destructive/20 dark:focus-visible:ring-destructive/40 dark:bg-destructive/60',
        outline:
          'border bg-background shadow-xs hover:bg-accent hover:text-accent-foreground dark:bg-input/30 dark:border-input dark:hover:bg-input/50',
        secondary:
          'bg-secondary text-secondary-foreground shadow-xs hover:bg-secondary/80',
        ghost:
          'hover:bg-accent hover:text-accent-foreground dark:hover:bg-accent/50',
        link: 'text-primary underline-offset-4 hover:underline',
      },
      size: {
        default: 'size-9',
        xs: "size-7 [&_svg:not([class*='size-'])]:size-3.5 rounded-md",
        sm: 'size-8 rounded-md',
        lg: 'size-10 rounded-md',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  },
);

type IconButtonProps = Omit<ButtonPrimitiveProps, 'asChild'> &
  VariantProps<typeof buttonVariants> & {
    children?: React.ReactNode;
  };

function IconButton({
  className,
  onClick,
  variant,
  size,
  children,
  ...props
}: IconButtonProps) {
  const [isActive, setIsActive] = React.useState(false);
  const [key, setKey] = React.useState(0);

  return (
    <Particles asChild animate={isActive} key={key}>
      <ButtonPrimitive
        data-slot="icon-button"
        className={cn(buttonVariants({ variant, size, className }))}
        onClick={(e) => {
          setKey((prev) => prev + 1);
          setIsActive(true);
          onClick?.(e);
        }}
        {...props}
      >
        {children}
        <ParticlesEffect
          data-variant={variant}
          className="bg-neutral-500 size-1 rounded-full"
        />
      </ButtonPrimitive>
    </Particles>
  );
}

export { IconButton, buttonVariants, type IconButtonProps };
```

---

### Liquid Button <a name="liquid"></a>

A button that fills on hover.

- **Category**: Buttons
- **Registry Key**: `components-buttons-liquid`

#### Installation

```bash
npx shadcn@latest add components-buttons-liquid
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`

#### How to Use

```tsx
<LiquidButton>Liquid Button</LiquidButton>
```

#### Demo Code

##### File: `registry/demo/components/buttons/liquid/index.tsx`

```tsx
import { PlusIcon } from 'lucide-react';
import {
  LiquidButton,
  type LiquidButtonProps,
} from '@/components/animate-ui/components/buttons/liquid';

interface LiquidButtonDemoProps {
  variant: LiquidButtonProps['variant'];
  size: LiquidButtonProps['size'];
}

export default function LiquidButtonDemo({
  variant,
  size,
}: LiquidButtonDemoProps) {
  return (
    <LiquidButton variant={variant} size={size}>
      {size === 'icon' ? <PlusIcon /> : 'Hover me'}
    </LiquidButton>
  );
}
```

#### Component Source Code

##### File: `registry/components/buttons/liquid/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { cva, type VariantProps } from 'class-variance-authority';

import {
  LiquidButton as LiquidButtonPrimitive,
  type LiquidButtonProps as LiquidButtonPrimitiveProps,
} from '@/components/animate-ui/primitives/buttons/liquid';
import { cn } from '@/lib/utils';

const buttonVariants = cva(
  "inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-md text-sm font-medium transition-[box-shadow,_color,_background-color,_border-color,_outline-color,_text-decoration-color,_fill,_stroke] disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4 shrink-0 [&_svg]:shrink-0 outline-none focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px] aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive",
  {
    variants: {
      variant: {
        default:
          '[--liquid-button-background-color:var(--accent)] [--liquid-button-color:var(--primary)] text-primary hover:text-primary-foreground shadow-xs',
        destructive:
          '[--liquid-button-background-color:var(--accent)] [--liquid-button-color:var(--destructive)] text-white shadow-xs focus-visible:ring-destructive/20 dark:focus-visible:ring-destructive/40',
        secondary:
          '[--liquid-button-background-color:var(--accent)] [--liquid-button-color:var(--secondary)] text-secondary hover:text-secondary-foreground shadow-xs',
        ghost:
          '[--liquid-button-background-color:var(--transparent)] [--liquid-button-color:var(--primary)] text-primary hover:text-primary-foreground shadow-xs',
      },
      size: {
        default: 'h-9 px-4 py-2 has-[>svg]:px-3',
        sm: 'h-8 rounded-md gap-1.5 px-3 has-[>svg]:px-2.5',
        lg: 'h-10 rounded-md px-6 has-[>svg]:px-4',
        icon: 'size-9',
        'icon-sm': 'size-8 rounded-md',
        'icon-lg': 'size-10 rounded-md',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  },
);

type LiquidButtonProps = LiquidButtonPrimitiveProps &
  VariantProps<typeof buttonVariants>;

function LiquidButton({
  className,
  variant,
  size,
  ...props
}: LiquidButtonProps) {
  return (
    <LiquidButtonPrimitive
      className={cn(buttonVariants({ variant, size, className }))}
      {...props}
    />
  );
}

export { LiquidButton, buttonVariants, type LiquidButtonProps };
```

---

### Ripple Button <a name="ripple"></a>

A button that animates on tap with a ripple effect.

- **Category**: Buttons
- **Registry Key**: `components-buttons-ripple`

#### Installation

```bash
npx shadcn@latest add components-buttons-ripple
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`

#### How to Use

```tsx
<RippleButton>
  Ripple Button
  <RippleButtonRipples />
</RippleButton>
```

#### Demo Code

##### File: `registry/demo/components/buttons/ripple/index.tsx`

```tsx
import { PlusIcon } from 'lucide-react';
import {
  RippleButton,
  RippleButtonRipples,
  type RippleButtonProps,
} from '@/components/animate-ui/components/buttons/ripple';

interface RippleButtonDemoProps {
  variant: RippleButtonProps['variant'];
  size: RippleButtonProps['size'];
}

export default function RippleButtonDemo({
  variant,
  size,
}: RippleButtonDemoProps) {
  return (
    <RippleButton variant={variant} size={size}>
      {size === 'icon' ? <PlusIcon /> : 'Click me'}
      <RippleButtonRipples />
    </RippleButton>
  );
}
```

#### Component Source Code

##### File: `registry/components/buttons/ripple/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { type VariantProps } from 'class-variance-authority';

import { buttonVariants } from '@/components/animate-ui/components/buttons/button';
import {
  RippleButton as RippleButtonPrimitive,
  RippleButtonRipples as RippleButtonRipplesPrimitive,
  type RippleButtonProps as RippleButtonPrimitiveProps,
  type RippleButtonRipplesProps as RippleButtonRipplesPrimitiveProps,
} from '@/components/animate-ui/primitives/buttons/ripple';
import { cn } from '@/lib/utils';

const rippleButtonVariants = {
  default: '[--ripple-button-ripple-color:var(--primary-foreground)]',
  accent: '[--ripple-button-ripple-color:var(--accent-foreground)]',
  destructive: '[--ripple-button-ripple-color:var(--destructive-foreground)]',
  outline: '[--ripple-button-ripple-color:var(--foreground)]',
  secondary: '[--ripple-button-ripple-color:var(--secondary-foreground)]',
  ghost: '[--ripple-button-ripple-color:var(--foreground)]',
  link: '[--ripple-button-ripple-color:var(--primary-foreground)]',
};

type RippleButtonProps = RippleButtonPrimitiveProps &
  VariantProps<typeof buttonVariants>;

function RippleButton({
  className,
  variant,
  size,
  ...props
}: RippleButtonProps) {
  return (
    <RippleButtonPrimitive
      className={cn(
        buttonVariants({ variant, size, className }),
        rippleButtonVariants[variant as keyof typeof rippleButtonVariants],
      )}
      {...props}
    />
  );
}

type RippleButtonRipplesProps = RippleButtonRipplesPrimitiveProps;

function RippleButtonRipples(props: RippleButtonRipplesProps) {
  return <RippleButtonRipplesPrimitive {...props} />;
}

export {
  RippleButton,
  RippleButtonRipples,
  type RippleButtonProps,
  type RippleButtonRipplesProps,
};
```

---

### Theme Toggler Button <a name="theme-toggler"></a>

A button that toggles the theme gradually.

- **Category**: Buttons
- **Registry Key**: `components-buttons-theme-toggler`

#### Installation

```bash
npx shadcn@latest add components-buttons-theme-toggler
```

#### Dependencies

- **NPM Packages**:
  - `next-themes`
  - `class-variance-authority`
  - `lucide-react`

#### How to Use

```tsx
<ThemeTogglerButton />
```

#### Demo Code

##### File: `registry/demo/components/buttons/theme-toggler/index.tsx`

```tsx
import {
  ThemeTogglerButton,
  type ThemeTogglerButtonProps,
} from '@/components/animate-ui/components/buttons/theme-toggler';

interface ThemeTogglerButtonDemoProps {
  variant: ThemeTogglerButtonProps['variant'];
  size: ThemeTogglerButtonProps['size'];
  direction: ThemeTogglerButtonProps['direction'];
  system: boolean;
}

export default function ThemeTogglerButtonDemo({
  variant,
  size,
  direction,
  system,
}: ThemeTogglerButtonDemoProps) {
  return (
    <ThemeTogglerButton
      variant={variant}
      size={size}
      direction={direction}
      modes={system ? ['light', 'dark', 'system'] : ['light', 'dark']}
    />
  );
}
```

#### Component Source Code

##### File: `registry/components/buttons/theme-toggler/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { useTheme } from 'next-themes';
import { Monitor, Moon, Sun } from 'lucide-react';
import { VariantProps } from 'class-variance-authority';

import {
  ThemeToggler as ThemeTogglerPrimitive,
  type ThemeTogglerProps as ThemeTogglerPrimitiveProps,
  type ThemeSelection,
  type Resolved,
} from '@/components/animate-ui/primitives/effects/theme-toggler';
import { buttonVariants } from '@/components/animate-ui/components/buttons/icon';
import { cn } from '@/lib/utils';

const getIcon = (
  effective: ThemeSelection,
  resolved: Resolved,
  modes: ThemeSelection[],
) => {
  const theme = modes.includes('system') ? effective : resolved;
  return theme === 'system' ? (
    <Monitor />
  ) : theme === 'dark' ? (
    <Moon />
  ) : (
    <Sun />
  );
};

const getNextTheme = (
  effective: ThemeSelection,
  modes: ThemeSelection[],
): ThemeSelection => {
  const i = modes.indexOf(effective);
  if (i === -1) return modes[0];
  return modes[(i + 1) % modes.length];
};

type ThemeTogglerButtonProps = React.ComponentProps<'button'> &
  VariantProps<typeof buttonVariants> & {
    modes?: ThemeSelection[];
    onImmediateChange?: ThemeTogglerPrimitiveProps['onImmediateChange'];
    direction?: ThemeTogglerPrimitiveProps['direction'];
  };

function ThemeTogglerButton({
  variant = 'default',
  size = 'default',
  modes = ['light', 'dark', 'system'],
  direction = 'ltr',
  onImmediateChange,
  onClick,
  className,
  ...props
}: ThemeTogglerButtonProps) {
  const { theme, resolvedTheme, setTheme } = useTheme();

  return (
    <ThemeTogglerPrimitive
      theme={theme as ThemeSelection}
      resolvedTheme={resolvedTheme as Resolved}
      setTheme={setTheme}
      direction={direction}
      onImmediateChange={onImmediateChange}
    >
      {({ effective, resolved, toggleTheme }) => (
        <button
          data-slot="theme-toggler-button"
          className={cn(buttonVariants({ variant, size, className }))}
          onClick={(e) => {
            onClick?.(e);
            toggleTheme(getNextTheme(effective, modes));
          }}
          {...props}
        >
          {getIcon(effective, resolved, modes)}
        </button>
      )}
    </ThemeTogglerPrimitive>
  );
}

export { ThemeTogglerButton, type ThemeTogglerButtonProps };
```

---

## Community <a name="community"></a>

### Flip Card <a name="flip-card"></a>

A 3D animated card component that flips to reveal content on the back.

- **Category**: Community
- **Registry Key**: `demo-components-community-flip-card`

#### Installation

```bash
npx shadcn@latest add demo-components-community-flip-card
```

#### How to Use

```tsx
<FlipCard data={data} />
```

#### Demo Code

##### File: `registry/demo/components/community/flip-card/index.tsx`

```tsx
'use client';

import { FlipCard } from '@/components/animate-ui/components/community/flip-card';

const data = {
  name: 'Animate UI',
  username: 'animate_ui',
  image:
    'https://pbs.twimg.com/profile_images/1950218390741618688/72447Y7e_400x400.jpg',
  bio: 'A fully animated, open-source component distribution built with React, TypeScript, Tailwind CSS, and Motion.',
  stats: { following: 200, followers: 2900, posts: 120 },
  socialLinks: {
    linkedin: 'https://linkedin.com',
    github: 'https://github.com',
    twitter: 'https://twitter.com',
  },
};

export const FlipCardDemo = () => {
  return <FlipCard data={data} />;
};
```

#### Component Source Code

##### File: `registry/demo/components/community/flip-card/index.tsx`

```tsx
'use client';

import { FlipCard } from '@/components/animate-ui/components/community/flip-card';

const data = {
  name: 'Animate UI',
  username: 'animate_ui',
  image:
    'https://pbs.twimg.com/profile_images/1950218390741618688/72447Y7e_400x400.jpg',
  bio: 'A fully animated, open-source component distribution built with React, TypeScript, Tailwind CSS, and Motion.',
  stats: { following: 200, followers: 2900, posts: 120 },
  socialLinks: {
    linkedin: 'https://linkedin.com',
    github: 'https://github.com',
    twitter: 'https://twitter.com',
  },
};

export const FlipCardDemo = () => {
  return <FlipCard data={data} />;
};
```

---

### Management Bar <a name="management-bar"></a>

A management bar that combines pagination, action buttons, and a call-to-action for streamlined item handling.

- **Category**: Community
- **Registry Key**: `components-community-management-bar`

#### Installation

```bash
npx shadcn@latest add components-community-management-bar
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### How to Use

```tsx
<ManagementBar />
```

#### Demo Code

##### File: `registry/demo/components/community/management-bar/index.tsx`

```tsx
'use client';

import * as React from 'react';

import { ManagementBar } from '@/components/animate-ui/components/community/management-bar';

export const ManagementBarDemo = () => <ManagementBar />;
```

#### Component Source Code

##### File: `registry/components/community/management-bar/index.tsx`

```tsx
'use client';

import * as React from 'react';
import {
  ChevronLeft,
  ChevronRight,
  Ban,
  X,
  Command,
  IdCard,
} from 'lucide-react';
import { SlidingNumber } from '@/components/animate-ui/primitives/texts/sliding-number';
import { motion, type Variants, type Transition } from 'motion/react';

const TOTAL_PAGES = 10;

const BUTTON_MOTION_CONFIG = {
  initial: 'rest',
  whileHover: 'hover',
  whileTap: 'tap',
  variants: {
    rest: { maxWidth: '40px' },
    hover: {
      maxWidth: '140px',
      transition: { type: 'spring', stiffness: 200, damping: 35, delay: 0.15 },
    },
    tap: { scale: 0.95 },
  },
  transition: { type: 'spring', stiffness: 250, damping: 25 },
} as const;

const LABEL_VARIANTS: Variants = {
  rest: { opacity: 0, x: 4 },
  hover: { opacity: 1, x: 0, visibility: 'visible' },
  tap: { opacity: 1, x: 0, visibility: 'visible' },
};

const LABEL_TRANSITION: Transition = {
  type: 'spring',
  stiffness: 200,
  damping: 25,
};

function ManagementBar() {
  const [currentPage, setCurrentPage] = React.useState(1);

  const handlePrevPage = React.useCallback(() => {
    if (currentPage > 1) setCurrentPage(currentPage - 1);
  }, [currentPage]);

  const handleNextPage = React.useCallback(() => {
    if (currentPage < TOTAL_PAGES) setCurrentPage(currentPage + 1);
  }, [currentPage]);

  return (
    <div className="@container/wrapper w-full flex justify-center">
      <div className="flex w-fit flex-col @xl/wrapper:flex-row items-center gap-y-2 rounded-2xl border border-border bg-background p-2 shadow-lg">
        <div className="mx-auto flex flex-col @lg/wrapper:flex-row shrink-0 items-center">
          <div className="flex h-10">
            <button
              disabled={currentPage === 1}
              className="p-1 text-muted-foreground transition-colors hover:text-foreground disabled:text-muted-foreground/30 disabled:hover:text-muted-foreground/30"
              onClick={handlePrevPage}
            >
              <ChevronLeft size={20} />
            </button>
            <div className="mx-2 flex items-center space-x-1 text-sm tabular-nums">
              <SlidingNumber
                className="text-foreground"
                padStart
                number={currentPage}
              />
              <span className="text-muted-foreground">/ {TOTAL_PAGES}</span>
            </div>
            <button
              disabled={currentPage === TOTAL_PAGES}
              className="p-1 text-muted-foreground transition-colors hover:text-foreground disabled:text-muted-foreground/30 disabled:hover:text-muted-foreground/30"
              onClick={handleNextPage}
            >
              <ChevronRight size={20} />
            </button>
          </div>
          <div className="mx-3 h-6 w-px bg-border rounded-full hidden @lg/wrapper:block" />

          <motion.div
            layout
            layoutRoot
            className="mx-auto flex flex-wrap space-x-2 sm:flex-nowrap"
          >
            <motion.button
              {...BUTTON_MOTION_CONFIG}
              className="flex h-10 items-center space-x-2 overflow-hidden whitespace-nowrap rounded-lg bg-neutral-200/60 dark:bg-neutral-600/80 px-2.5 py-2 text-neutral-600 dark:text-neutral-200"
              aria-label="Blacklist"
            >
              <Ban size={20} className="shrink-0" />
              <motion.span
                variants={LABEL_VARIANTS}
                transition={LABEL_TRANSITION}
                className="invisible text-sm"
              >
                Blacklist
              </motion.span>
            </motion.button>

            <motion.button
              {...BUTTON_MOTION_CONFIG}
              className="flex h-10 items-center space-x-2 overflow-hidden whitespace-nowrap rounded-lg bg-red-200/60 dark:bg-red-800/80 px-2.5 py-2 text-red-600 dark:text-red-300"
              aria-label="Reject"
            >
              <X size={20} className="shrink-0" />
              <motion.span
                variants={LABEL_VARIANTS}
                transition={LABEL_TRANSITION}
                className="invisible text-sm"
              >
                Reject
              </motion.span>
            </motion.button>

            <motion.button
              {...BUTTON_MOTION_CONFIG}
              className="flex h-10 items-center space-x-2 overflow-hidden whitespace-nowrap rounded-lg bg-green-200/60 dark:bg-green-800/80 px-2.5 py-2 text-green-600 dark:text-green-300"
              aria-label="Hire"
            >
              <IdCard size={20} className="shrink-0" />
              <motion.span
                variants={LABEL_VARIANTS}
                transition={LABEL_TRANSITION}
                className="invisible text-sm"
              >
                Hire
              </motion.span>
            </motion.button>
          </motion.div>
        </div>

        <div className="mx-3 hidden h-6 w-px bg-border @xl/wrapper:block rounded-full" />

        <motion.button
          whileTap={{ scale: 0.975 }}
          className="flex h-10 text-sm cursor-pointer items-center justify-center rounded-lg bg-teal-500 dark:bg-teal-600/80 px-3 py-2 text-white transition-colors duration-300 dark:hover:bg-teal-800 hover:bg-teal-600 w-full @xl/wrapper:w-auto"
        >
          <span className="mr-1 text-neutral-200">Move to:</span>
          <span>Interview I</span>
          <div className="mx-3 h-5 w-px bg-white/40 rounded-full" />
          <div className="flex items-center gap-1 rounded-md bg-white/20 px-1.5 py-0.5 -mr-1">
            <Command size={14} />E
          </div>
        </motion.button>
      </div>
    </div>
  );
}

export { ManagementBar };
```

---

### Motion Carousel <a name="motion-carousel"></a>

A carousel built on top of Embla Carousel with smooth Motion-powered animations. Each slide scales dynamically based on its active state, and the pagination uses animated pill-style dot buttons for an interactive, fluid experience.

- **Category**: Community
- **Registry Key**: `components-community-motion-carousel`

#### Installation

```bash
npx shadcn@latest add components-community-motion-carousel
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `lucide-react`
  - `embla-carousel`
  - `embla-carousel-react`

#### How to Use

```tsx
<MotionCarousel slides={SLIDES} options={OPTIONS} />
```

#### Demo Code

##### File: `registry/demo/components/community/motion-carousel/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { MotionCarousel } from '@/components/animate-ui/components/community/motion-carousel';
import { EmblaOptionsType } from 'embla-carousel';

export const MotionCarouselDemo = () => {
  const OPTIONS: EmblaOptionsType = { loop: true };
  const SLIDE_COUNT = 6;
  const SLIDES = Array.from(Array(SLIDE_COUNT).keys());

  return <MotionCarousel slides={SLIDES} options={OPTIONS} />;
};
```

#### Component Source Code

##### File: `registry/components/community/motion-carousel/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { motion, type Transition } from 'motion/react';
import { EmblaOptionsType, EmblaCarouselType } from 'embla-carousel';
import useEmblaCarousel from 'embla-carousel-react';
import { Button } from '@/components/animate-ui/components/buttons/button';
import { ChevronRight, ChevronLeft } from 'lucide-react';

type PropType = {
  slides: number[];
  options?: EmblaOptionsType;
};

type EmblaControls = {
  selectedIndex: number;
  scrollSnaps: number[];
  prevDisabled: boolean;
  nextDisabled: boolean;
  onDotClick: (index: number) => void;
  onPrev: () => void;
  onNext: () => void;
};

type DotButtonProps = {
  selected?: boolean;
  label: string;
  onClick: () => void;
};

const transition: Transition = {
  type: 'spring',
  stiffness: 240,
  damping: 24,
  mass: 1,
};

const useEmblaControls = (
  emblaApi: EmblaCarouselType | undefined,
): EmblaControls => {
  const [selectedIndex, setSelectedIndex] = React.useState(0);
  const [scrollSnaps, setScrollSnaps] = React.useState<number[]>([]);
  const [prevDisabled, setPrevDisabled] = React.useState(true);
  const [nextDisabled, setNextDisabled] = React.useState(true);

  const onDotClick = React.useCallback(
    (index: number) => emblaApi?.scrollTo(index),
    [emblaApi],
  );

  const onPrev = React.useCallback(() => emblaApi?.scrollPrev(), [emblaApi]);
  const onNext = React.useCallback(() => emblaApi?.scrollNext(), [emblaApi]);

  const updateSelectionState = (api: EmblaCarouselType) => {
    setSelectedIndex(api.selectedScrollSnap());
    setPrevDisabled(!api.canScrollPrev());
    setNextDisabled(!api.canScrollNext());
  };

  const onInit = React.useCallback((api: EmblaCarouselType) => {
    setScrollSnaps(api.scrollSnapList());
    updateSelectionState(api);
  }, []);

  const onSelect = React.useCallback((api: EmblaCarouselType) => {
    updateSelectionState(api);
  }, []);

  React.useEffect(() => {
    if (!emblaApi) return;

    onInit(emblaApi);
    emblaApi.on('reInit', onInit).on('select', onSelect);

    return () => {
      emblaApi.off('reInit', onInit).off('select', onSelect);
    };
  }, [emblaApi, onInit, onSelect]);

  return {
    selectedIndex,
    scrollSnaps,
    prevDisabled,
    nextDisabled,
    onDotClick,
    onPrev,
    onNext,
  };
};

function MotionCarousel(props: PropType) {
  const { slides, options } = props;
  const [emblaRef, emblaApi] = useEmblaCarousel(options);
  const {
    selectedIndex,
    scrollSnaps,
    prevDisabled,
    nextDisabled,
    onDotClick,
    onPrev,
    onNext,
  } = useEmblaControls(emblaApi);

  return (
    <div className="w-full space-y-4 [--slide-height:9rem] sm:[--slide-height:13rem] md:[--slide-height:18rem] [--slide-spacing:1.5rem] [--slide-size:55%]">
      <div className="overflow-hidden" ref={emblaRef}>
        <div className="flex touch-pan-y touch-pinch-zoom">
          {slides.map((index) => {
            const isActive = index === selectedIndex;

            return (
              <motion.div
                key={index}
                className="h-[var(--slide-height)] mr-[var(--slide-spacing)] basis-[var(--slide-size)] flex-none flex min-w-0"
              >
                <motion.div
                  className="size-full flex items-center justify-center text-3xl md:text-5xl font-semibold select-none border-4 rounded-xl"
                  initial={false}
                  animate={{
                    scale: isActive ? 1 : 0.9,
                  }}
                  transition={transition}
                >
                  {index + 1}
                </motion.div>
              </motion.div>
            );
          })}
        </div>
      </div>

      <div className="flex justify-between">
        <Button size="icon" onClick={onPrev} disabled={prevDisabled}>
          <ChevronLeft className="size-5" />
        </Button>

        <div className="flex flex-wrap justify-end items-center gap-2">
          {scrollSnaps.map((_, index) => (
            <DotButton
              key={index}
              label={`Slide ${index + 1}`}
              selected={index === selectedIndex}
              onClick={() => onDotClick(index)}
            />
          ))}
        </div>

        <Button size="icon" onClick={onNext} disabled={nextDisabled}>
          <ChevronRight className="size-5" />
        </Button>
      </div>
    </div>
  );
}

function DotButton({ selected = false, label, onClick }: DotButtonProps) {
  return (
    <motion.button
      type="button"
      onClick={onClick}
      layout
      initial={false}
      className="flex cursor-pointer select-none items-center justify-center rounded-full border-none bg-primary text-primary-foreground text-sm"
      animate={{
        width: selected ? 68 : 12,
        height: selected ? 28 : 12,
      }}
      transition={transition}
    >
      <motion.span
        layout
        initial={false}
        className="block whitespace-nowrap px-3 py-1"
        animate={{
          opacity: selected ? 1 : 0,
          scale: selected ? 1 : 0,
          filter: selected ? 'blur(0)' : 'blur(4px)',
        }}
        transition={transition}
      >
        {label}
      </motion.span>
    </motion.button>
  );
}

export { MotionCarousel };
```

---

### Notification List <a name="notification-list"></a>

A fun notification list with animated stacking and cards that expand as you interact.

- **Category**: Community
- **Registry Key**: `components-community-notification-list`

#### Installation

```bash
npx shadcn@latest add components-community-notification-list
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### How to Use

```tsx
<NotificationList />
```

#### Demo Code

##### File: `registry/demo/components/community/notification-list/index.tsx`

```tsx
'use client';

import * as React from 'react';

import { NotificationList } from '@/components/animate-ui/components/community/notification-list';

export const NotificationListDemo = () => <NotificationList />;
```

#### Component Source Code

##### File: `registry/components/community/notification-list/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { RotateCcw, ArrowUpRight } from 'lucide-react';
import { motion, type Transition } from 'motion/react';

const notifications = [
  {
    id: 1,
    title: 'NPM Install Complete',
    subtitle: '1,227 packages added!',
    time: 'just now',
    count: 2,
  },
  {
    id: 2,
    title: 'Build Succeeded',
    subtitle: 'Build finished in 12.34s',
    time: '1m 11s',
  },
  {
    id: 3,
    title: 'Lint Passed',
    subtitle: 'No problems found',
    time: '5m',
  },
];

const transition: Transition = {
  type: 'spring',
  stiffness: 300,
  damping: 26,
};

const getCardVariants = (i: number) => ({
  collapsed: {
    marginTop: i === 0 ? 0 : -44,
    scaleX: 1 - i * 0.05,
  },
  expanded: {
    marginTop: i === 0 ? 0 : 4,
    scaleX: 1,
  },
});

const textSwitchTransition: Transition = {
  duration: 0.22,
  ease: 'easeInOut',
};

const notificationTextVariants = {
  collapsed: { opacity: 1, y: 0, pointerEvents: 'auto' },
  expanded: { opacity: 0, y: -16, pointerEvents: 'none' },
};

const viewAllTextVariants = {
  collapsed: { opacity: 0, y: 16, pointerEvents: 'none' },
  expanded: { opacity: 1, y: 0, pointerEvents: 'auto' },
};

function NotificationList() {
  return (
    <motion.div
      className="bg-neutral-200 dark:bg-neutral-900 p-3 rounded-3xl w-xs space-y-3 shadow-md"
      initial="collapsed"
      whileHover="expanded"
    >
      <div>
        {notifications.map((notification, i) => (
          <motion.div
            key={notification.id}
            className="bg-neutral-100 dark:bg-neutral-800 rounded-xl px-4 py-2 shadow-sm hover:shadow-lg transition-shadow duration-200 relative"
            variants={getCardVariants(i)}
            transition={transition}
            style={{
              zIndex: notifications.length - i,
            }}
          >
            <div className="flex justify-between items-center">
              <h1 className="text-sm font-medium">{notification.title}</h1>
              {notification.count && (
                <div className="flex items-center text-xs gap-0.5 font-medium text-neutral-500 dark:text-neutral-300">
                  <RotateCcw className="size-3" />
                  <span>{notification.count}</span>
                </div>
              )}
            </div>
            <div className="text-xs text-neutral-500 font-medium">
              <span>{notification.time}</span>
              &nbsp;•&nbsp;
              <span>{notification.subtitle}</span>
            </div>
          </motion.div>
        ))}
      </div>

      <div className="flex items-center gap-2">
        <div className="size-5 rounded-full bg-neutral-400 text-white text-xs flex items-center justify-center font-medium">
          {notifications.length}
        </div>
        <span className="grid">
          <motion.span
            className="text-sm font-medium text-neutral-600 dark:text-neutral-300 row-start-1 col-start-1"
            variants={notificationTextVariants}
            transition={textSwitchTransition}
          >
            Notifications
          </motion.span>
          <motion.span
            className="text-sm font-medium text-neutral-600 dark:text-neutral-300 flex items-center gap-1 cursor-pointer select-none row-start-1 col-start-1"
            variants={viewAllTextVariants}
            transition={textSwitchTransition}
          >
            View all <ArrowUpRight className="size-4" />
          </motion.span>
        </span>
      </div>
    </motion.div>
  );
}

export { NotificationList };
```

---

### Pin List <a name="pin-list"></a>

A playful list for pinning and unpinning items, with smooth animated transitions as items move between groups.

- **Category**: Community
- **Registry Key**: `components-community-pin-list`

#### Installation

```bash
npx shadcn@latest add components-community-pin-list
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### How to Use

```tsx
<PinList items={ITEMS} />
```

#### Demo Code

##### File: `registry/demo/components/community/pin-list/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { GitCommit, AlertTriangle, Box, KeyRound, Regex } from 'lucide-react';

import { PinList } from '@/components/animate-ui/components/community/pin-list';

const ITEMS = [
  {
    id: 1,
    name: 'Commit Zone',
    info: 'Code updates · Closes 9:00 PM',
    icon: GitCommit,
    pinned: true,
  },
  {
    id: 2,
    name: '404 Room',
    info: 'Fixing errors · Open 24 hours',
    icon: AlertTriangle,
    pinned: true,
  },
  {
    id: 3,
    name: 'NPM Stop',
    info: 'Install stuff · Closes 8:00 PM',
    icon: Box,
    pinned: false,
  },
  {
    id: 4,
    name: 'Token Lock',
    info: 'Login stuff · Open 24 hours',
    icon: KeyRound,
    pinned: false,
  },
  {
    id: 5,
    name: 'Regex Zone',
    info: 'Find words · Closes 9:00 PM',
    icon: Regex,
    pinned: false,
  },
];

export const PinListDemo = () => <PinList items={ITEMS} />;
```

#### Component Source Code

##### File: `registry/components/community/pin-list/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { Pin } from 'lucide-react';
import {
  motion,
  LayoutGroup,
  AnimatePresence,
  type HTMLMotionProps,
  type Transition,
} from 'motion/react';
import { cn } from '@/lib/utils';

type PinListItem = {
  id: number;
  name: string;
  info: string;
  icon: React.ElementType;
  pinned: boolean;
};

type PinListProps = {
  items: PinListItem[];
  labels?: {
    pinned?: string;
    unpinned?: string;
  };
  transition?: Transition;
  labelMotionProps?: HTMLMotionProps<'p'>;
  className?: string;
  labelClassName?: string;
  pinnedSectionClassName?: string;
  unpinnedSectionClassName?: string;
  zIndexResetDelay?: number;
} & HTMLMotionProps<'div'>;

function PinList({
  items,
  labels = { pinned: 'Pinned Items', unpinned: 'All Items' },
  transition = { stiffness: 320, damping: 20, mass: 0.8, type: 'spring' },
  labelMotionProps = {
    initial: { opacity: 0 },
    animate: { opacity: 1 },
    exit: { opacity: 0 },
    transition: { duration: 0.22, ease: 'easeInOut' },
  },
  className,
  labelClassName,
  pinnedSectionClassName,
  unpinnedSectionClassName,
  zIndexResetDelay = 500,
  ...props
}: PinListProps) {
  const [listItems, setListItems] = React.useState(items);
  const [togglingGroup, setTogglingGroup] = React.useState<
    'pinned' | 'unpinned' | null
  >(null);

  const pinned = listItems.filter((u) => u.pinned);
  const unpinned = listItems.filter((u) => !u.pinned);

  const toggleStatus = (id: number) => {
    const item = listItems.find((u) => u.id === id);
    if (!item) return;

    setTogglingGroup(item.pinned ? 'pinned' : 'unpinned');
    setListItems((prev) => {
      const idx = prev.findIndex((u) => u.id === id);
      if (idx === -1) return prev;
      const updated = [...prev];
      const [item] = updated.splice(idx, 1);
      if (!item) return prev;
      const toggled = { ...item, pinned: !item.pinned };
      if (toggled.pinned) updated.push(toggled);
      else updated.unshift(toggled);
      return updated;
    });
    // Reset group z-index after the animation duration (keep in sync with animation timing)
    setTimeout(() => setTogglingGroup(null), zIndexResetDelay);
  };

  return (
    <motion.div className={cn('space-y-10', className)} {...props}>
      <LayoutGroup>
        <div>
          {pinned.length > 0 && (
            <div
              className={cn(
                'space-y-3 relative',
                togglingGroup === 'pinned' ? 'z-5' : 'z-10',
                pinnedSectionClassName,
              )}
            >
              {pinned.map((item) => (
                <motion.div
                  key={item.id}
                  layoutId={`item-${item.id}`}
                  onClick={() => toggleStatus(item.id)}
                  transition={transition}
                  className="flex items-center justify-between gap-5 rounded-2xl bg-neutral-200 dark:bg-neutral-800 p-2"
                >
                  <div className="flex items-center gap-2">
                    <div className="rounded-lg bg-background p-2">
                      <item.icon className="size-5 text-neutral-500 dark:text-neutral-400" />
                    </div>
                    <div>
                      <div className="text-sm font-semibold">{item.name}</div>
                      <div className="text-xs text-neutral-500 dark:text-neutral-400 font-medium">
                        {item.info}
                      </div>
                    </div>
                  </div>
                  <div className="flex items-center justify-center size-8 rounded-full bg-neutral-400 dark:bg-neutral-600">
                    <Pin className="size-4 text-white fill-white" />
                  </div>
                </motion.div>
              ))}
            </div>
          )}
        </div>

        <div>
          <AnimatePresence>
            {unpinned.length > 0 && (
              <motion.p
                layout
                key="all-label"
                className={cn(
                  'font-medium px-3 text-neutral-500 dark:text-neutral-300 text-sm mb-2',
                  labelClassName,
                )}
                {...labelMotionProps}
              >
                {labels.unpinned}
              </motion.p>
            )}
          </AnimatePresence>
          {unpinned.length > 0 && (
            <div
              className={cn(
                'space-y-3 relative',
                togglingGroup === 'unpinned' ? 'z-5' : 'z-10',
                unpinnedSectionClassName,
              )}
            >
              {unpinned.map((item) => (
                <motion.div
                  key={item.id}
                  layoutId={`item-${item.id}`}
                  onClick={() => toggleStatus(item.id)}
                  transition={transition}
                  className="flex items-center justify-between gap-5 rounded-2xl bg-neutral-200 dark:bg-neutral-800 p-2 group"
                >
                  <div className="flex items-center gap-2">
                    <div className="rounded-lg bg-background p-2">
                      <item.icon className="size-5 text-neutral-500 dark:text-neutral-400" />
                    </div>
                    <div>
                      <div className="text-sm font-semibold">{item.name}</div>
                      <div className="text-xs text-neutral-500 dark:text-neutral-400 font-medium">
                        {item.info}
                      </div>
                    </div>
                  </div>
                  <div className="flex items-center justify-center size-8 rounded-full bg-neutral-400 dark:bg-neutral-600 opacity-0 group-hover:opacity-100 transition-opacity duration-250">
                    <Pin className="size-4 text-white" />
                  </div>
                </motion.div>
              ))}
            </div>
          )}
        </div>
      </LayoutGroup>
    </motion.div>
  );
}

export { PinList, type PinListProps, type PinListItem };
```

---

### Playful Todolist <a name="playful-todolist"></a>

A playful todolist component with animated wavy strikethroughs for each completed task.

- **Category**: Community
- **Registry Key**: `components-community-playful-todolist`

#### Installation

```bash
npx shadcn@latest add components-community-playful-todolist
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
<PlayfulTodolist />
```

#### Demo Code

##### File: `registry/demo/components/community/playful-todolist/index.tsx`

```tsx
'use client';

import * as React from 'react';

import { PlayfulTodolist } from '@/components/animate-ui/components/community/playful-todolist';

export const PlayfulTodolistDemo = () => <PlayfulTodolist />;
```

#### Component Source Code

##### File: `registry/components/community/playful-todolist/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { motion, type Transition } from 'motion/react';
import { Label } from '@/components/ui/label';
import { Checkbox } from '@/components/animate-ui/components/radix/checkbox';

const checkboxItems = [
  {
    id: 1,
    label: 'Code in Assembly 💾',
    defaultChecked: false,
  },
  {
    id: 2,
    label: 'Present a bug as a feature 🪲',
    defaultChecked: false,
  },
  {
    id: 3,
    label: 'Push to prod on a Friday 🚀',
    defaultChecked: false,
  },
];

const getPathAnimate = (isChecked: boolean) => ({
  pathLength: isChecked ? 1 : 0,
  opacity: isChecked ? 1 : 0,
});

const getPathTransition = (isChecked: boolean): Transition => ({
  pathLength: { duration: 1, ease: 'easeInOut' },
  opacity: {
    duration: 0.01,
    delay: isChecked ? 0 : 1,
  },
});

function PlayfulTodolist() {
  const [checked, setChecked] = React.useState(
    checkboxItems.map((i) => !!i.defaultChecked),
  );

  return (
    <div className="bg-neutral-100 dark:bg-neutral-900 rounded-2xl p-6 space-y-6">
      {checkboxItems.map((item, idx) => (
        <div key={item.id} className="space-y-6">
          <div className="flex items-center space-x-2">
            <Checkbox
              variant="accent"
              checked={checked[idx]}
              onCheckedChange={(val) => {
                const updated = [...checked];
                updated[idx] = val === true;
                setChecked(updated);
              }}
              id={`checkbox-${item.id}`}
            />
            <div className="relative inline-block">
              <Label htmlFor={`checkbox-${item.id}`}>{item.label}</Label>
              <motion.svg
                width="340"
                height="32"
                viewBox="0 0 340 32"
                className="absolute left-0 top-1/2 -translate-y-1/2 pointer-events-none z-20 w-full h-10"
              >
                <motion.path
                  d="M 10 16.91 s 79.8 -11.36 98.1 -11.34 c 22.2 0.02 -47.82 14.25 -33.39 22.02 c 12.61 6.77 124.18 -27.98 133.31 -17.28 c 7.52 8.38 -26.8 20.02 4.61 22.05 c 24.55 1.93 113.37 -20.36 113.37 -20.36"
                  vectorEffect="non-scaling-stroke"
                  strokeWidth={2}
                  strokeLinecap="round"
                  strokeMiterlimit={10}
                  fill="none"
                  initial={false}
                  animate={getPathAnimate(!!checked[idx])}
                  transition={getPathTransition(!!checked[idx])}
                  className="stroke-neutral-900 dark:stroke-neutral-100"
                />
              </motion.svg>
            </div>
          </div>
          {idx !== checkboxItems.length - 1 && (
            <div className="border-t border-neutral-300 dark:border-neutral-700" />
          )}
        </div>
      ))}
    </div>
  );
}

export { PlayfulTodolist };
```

---

### Radial Intro <a name="radial-intro"></a>

A circular intro animation component that arranges elements in a radial layout, smoothly transitioning them into orbit with looping motion.

- **Category**: Community
- **Registry Key**: `components-community-radial-intro`

#### Installation

```bash
npx shadcn@latest add components-community-radial-intro
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
<RadialIntro orbitItems={ITEMS}
```

#### Demo Code

##### File: `registry/demo/components/community/radial-intro/index.tsx`

```tsx
'use client';

import * as React from 'react';

import { RadialIntro } from '@/components/animate-ui/components/community/radial-intro';

const ITEMS = [
  {
    id: 1,
    name: 'Framer University',
    src: 'https://pbs.twimg.com/profile_images/1602734731728142336/9Bppcs67_400x400.jpg',
  },
  {
    id: 2,
    name: 'arhamkhnz',
    src: 'https://pbs.twimg.com/profile_images/1897311929028255744/otxpL-ke_400x400.jpg',
  },
  {
    id: 3,
    name: 'Skyleen',
    src: 'https://pbs.twimg.com/profile_images/1948770261848756224/oPwqXMD6_400x400.jpg',
  },
  {
    id: 4,
    name: 'Shadcn',
    src: 'https://pbs.twimg.com/profile_images/1593304942210478080/TUYae5z7_400x400.jpg',
  },
  {
    id: 5,
    name: 'Adam Wathan',
    src: 'https://pbs.twimg.com/profile_images/1677042510839857154/Kq4tpySA_400x400.jpg',
  },
  {
    id: 6,
    name: 'Guillermo Rauch',
    src: 'https://pbs.twimg.com/profile_images/1783856060249595904/8TfcCN0r_400x400.jpg',
  },
  {
    id: 7,
    name: 'Jhey',
    src: 'https://pbs.twimg.com/profile_images/1534700564810018816/anAuSfkp_400x400.jpg',
  },
  {
    id: 8,
    name: 'David Haz',
    src: 'https://pbs.twimg.com/profile_images/1927474594102784000/Al0g-I6o_400x400.jpg',
  },
  {
    id: 9,
    name: 'Matt Perry',
    src: 'https://pbs.twimg.com/profile_images/1690345911149375488/wfD0Ai9j_400x400.jpg',
  },
];

export const RadialIntroDemo = () => <RadialIntro orbitItems={ITEMS} />;
```

#### Component Source Code

##### File: `registry/components/community/radial-intro/index.tsx`

```tsx
'use client';

import * as React from 'react';
import {
  LayoutGroup,
  motion,
  useAnimate,
  delay,
  type Transition,
  type AnimationSequence,
} from 'motion/react';

interface ComponentProps {
  orbitItems: OrbitItem[];
  stageSize?: number;
  imageSize?: number;
}

type OrbitItem = {
  id: number;
  name: string;
  src: string;
};

const transition: Transition = {
  delay: 0,
  stiffness: 300,
  damping: 35,
  type: 'spring',
  restSpeed: 0.01,
  restDelta: 0.01,
};

const spinConfig = {
  duration: 30,
  ease: 'linear' as const,
  repeat: Infinity,
};

const qsa = (root: Element, sel: string) =>
  Array.from(root.querySelectorAll(sel));

const angleOf = (el: Element) => Number((el as HTMLElement).dataset.angle || 0);

const armOfImg = (img: Element) =>
  (img as HTMLElement).closest('[data-arm]') as HTMLElement | null;

function RadialIntro({
  orbitItems,
  stageSize = 320,
  imageSize = 60,
}: ComponentProps) {
  const step = 360 / orbitItems.length;
  const [scope, animate] = useAnimate();

  React.useEffect(() => {
    const root = scope.current;
    if (!root) return;

    // get arm and image elements
    const arms = qsa(root, '[data-arm]');
    const imgs = qsa(root, '[data-arm-image]');
    const stops: Array<() => void> = [];

    // image lift-in
    delay(() => animate(imgs, { top: 0 }, transition), 250);

    // build sequence for orbit placement
    const orbitPlacementSequence: AnimationSequence = [
      ...arms.map((el): [Element, Record<string, any>, any] => [
        el,
        { rotate: angleOf(el) },
        { ...transition, at: 0 },
      ]),
      ...imgs.map((img): [Element, Record<string, any>, any] => [
        img,
        { rotate: -angleOf(armOfImg(img)!), opacity: 1 },
        { ...transition, at: 0 },
      ]),
    ];

    // play placement sequence
    delay(() => animate(orbitPlacementSequence), 700);

    // start continuous spin for arms and images
    delay(() => {
      // arms spin clockwise
      arms.forEach((el) => {
        const angle = angleOf(el);
        const ctrl = animate(el, { rotate: [angle, angle + 360] }, spinConfig);
        stops.push(() => ctrl.cancel());
      });

      // images counter-spin to stay upright
      imgs.forEach((img) => {
        const arm = armOfImg(img);
        const angle = arm ? angleOf(arm) : 0;
        const ctrl = animate(
          img,
          { rotate: [-angle, -angle - 360] },
          spinConfig,
        );
        stops.push(() => ctrl.cancel());
      });
    }, 1300);

    return () => stops.forEach((stop) => stop());
  }, []);

  return (
    <LayoutGroup>
      <motion.div
        ref={scope}
        className="relative overflow-visible"
        style={{ width: stageSize, height: stageSize }}
        initial={false}
      >
        {orbitItems.map((item, i) => (
          <motion.div
            key={item.id}
            data-arm
            className="will-change-transform absolute inset-0"
            style={{ zIndex: orbitItems.length - i }}
            data-angle={i * step}
            layoutId={`arm-${item.id}`}
          >
            <motion.img
              data-arm-image
              className="rounded-full object-fill absolute left-1/2 top-1/2 aspect-square translate -translate-x-1/2"
              style={{
                width: imageSize,
                height: imageSize,
                opacity: i === 0 ? 1 : 0,
              }}
              src={item.src}
              alt={item.name}
              draggable={false}
              layoutId={`arm-img-${item.id}`}
            />
          </motion.div>
        ))}
      </motion.div>
    </LayoutGroup>
  );
}

export { RadialIntro };
```

---

### Radial Menu <a name="radial-menu"></a>

A circular context menu built with Base UI, displaying actions in a clean radial layout with full keyboard support and smooth interaction.

- **Category**: Community
- **Registry Key**: `components-community-radial-menu`

#### Installation

```bash
npx shadcn@latest add components-community-radial-menu
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `lucide-react`
  - `@base-ui-components/react`

#### How to Use

```tsx
<RadialMenu menuItems={MENU_ITEMS} />
```

#### Demo Code

##### File: `registry/demo/components/community/radial-menu/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { RadialMenu } from '@/components/animate-ui/components/community/radial-menu';
import {
  Copy,
  Scissors,
  ClipboardPaste,
  Trash2,
  Star,
  Pin,
} from 'lucide-react';

const MENU_ITEMS = [
  { id: 1, label: 'Copy', icon: Copy },
  { id: 2, label: 'Cut', icon: Scissors },
  { id: 3, label: 'Paste', icon: ClipboardPaste },
  { id: 4, label: 'Favorite', icon: Star },
  { id: 5, label: 'Pin', icon: Pin },
  { id: 6, label: 'Delete', icon: Trash2 },
];

export const RadialMenuDemo = () => (
  <RadialMenu
    menuItems={MENU_ITEMS}
    onSelect={(item) => {
      console.log(item);
      // run your action here
    }}
  >
    <div className="size-80 flex justify-center items-center border-2 border-dashed rounded-lg">
      Right click to open the radial menu
    </div>
  </RadialMenu>
);
```

#### Component Source Code

##### File: `registry/components/community/radial-menu/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { LucideIcon } from 'lucide-react';
import { motion, AnimatePresence, type Transition } from 'motion/react';
import { ContextMenu } from '@base-ui-components/react/context-menu';
import { cn } from '@/lib/utils';

type RadialMenuProps = {
  children?: React.ReactNode;
  menuItems: MenuItem[];
  size?: number;
  iconSize?: number;
  bandWidth?: number;
  innerGap?: number;
  outerGap?: number;
  outerRingWidth?: number;
  onSelect?: (item: MenuItem) => void;
};

type MenuItem = {
  id: number;
  label: string;
  icon: LucideIcon;
};

type Point = { x: number; y: number };

const menuTransition: Transition = {
  type: 'spring',
  stiffness: 420,
  damping: 32,
  mass: 1,
};

const wedgeTransition: Transition = {
  duration: 0.05,
  ease: 'easeOut',
};

const FULL_CIRCLE = 360;
const START_ANGLE = -90;

function degToRad(deg: number) {
  return (deg * Math.PI) / 180;
}

function polarToCartesian(radius: number, angleDeg: number): Point {
  const rad = degToRad(angleDeg);
  return {
    x: Math.cos(rad) * radius,
    y: Math.sin(rad) * radius,
  };
}

function slicePath(
  index: number,
  total: number,
  wedgeRadius: number,
  innerRadius: number,
) {
  if (total <= 0) return '';

  // single item → full donut ring
  if (total === 1) {
    return `
      M ${wedgeRadius} 0
      A ${wedgeRadius} ${wedgeRadius} 0 1 1 ${-wedgeRadius} 0
      A ${wedgeRadius} ${wedgeRadius} 0 1 1 ${wedgeRadius} 0
      M ${innerRadius} 0
      A ${innerRadius} ${innerRadius} 0 1 0 ${-innerRadius} 0
      A ${innerRadius} ${innerRadius} 0 1 0 ${innerRadius} 0
    `;
  }

  const anglePerSlice = FULL_CIRCLE / total;
  const midDeg = START_ANGLE + anglePerSlice * index;
  const halfSlice = anglePerSlice / 2;

  const startDeg = midDeg - halfSlice;
  const endDeg = midDeg + halfSlice;

  const outerStart = polarToCartesian(wedgeRadius, startDeg);
  const outerEnd = polarToCartesian(wedgeRadius, endDeg);
  const innerStart = polarToCartesian(innerRadius, startDeg);
  const innerEnd = polarToCartesian(innerRadius, endDeg);

  const largeArcFlag = anglePerSlice > 180 ? 1 : 0;

  return `
    M ${outerStart.x} ${outerStart.y}
    A ${wedgeRadius} ${wedgeRadius} 0 ${largeArcFlag} 1 ${outerEnd.x} ${outerEnd.y}
    L ${innerEnd.x} ${innerEnd.y}
    A ${innerRadius} ${innerRadius} 0 ${largeArcFlag} 0 ${innerStart.x} ${innerStart.y}
    Z
  `;
}

function RadialMenu({
  children,
  menuItems,
  size = 240,
  iconSize = 18,
  bandWidth = 50,
  innerGap = 8,
  outerGap = 8,
  outerRingWidth = 12,
  onSelect,
}: RadialMenuProps) {
  const radius = size / 2;

  const outerRingOuterRadius = radius;
  const outerRingInnerRadius = outerRingOuterRadius - outerRingWidth;

  const wedgeOuterRadius = outerRingInnerRadius - outerGap;
  const wedgeInnerRadius = wedgeOuterRadius - bandWidth;

  const iconRingRadius = (wedgeOuterRadius + wedgeInnerRadius) / 2;

  const centerRadius = Math.max(wedgeInnerRadius - innerGap, 0);

  const slice = 360 / menuItems.length;

  const itemRefs = React.useRef<(HTMLElement | null)[]>([]);
  const [activeIndex, setActiveIndex] = React.useState<number | null>(null);
  const [open, setOpen] = React.useState<boolean>(false);

  const resetActive = () => setActiveIndex(null);

  const handleOpenChange = (isOpen: boolean) => {
    setOpen(isOpen);
    if (!isOpen) resetActive();
  };

  return (
    <ContextMenu.Root open={open} onOpenChange={handleOpenChange}>
      <ContextMenu.Trigger
        render={(triggerProps) => {
          return (
            <div
              {...triggerProps}
              className={cn('select-none outline-none', triggerProps.className)}
            >
              {children ? (
                children
              ) : (
                <div className="size-80 flex justify-center items-center border-2 border-dashed rounded-lg">
                  Right-click here.
                </div>
              )}
            </div>
          );
        }}
      />

      <AnimatePresence>
        {open && (
          <ContextMenu.Portal keepMounted>
            <ContextMenu.Positioner
              align="center"
              sideOffset={({ positioner }) => -positioner.height / 2}
              className="outline-none"
            >
              <ContextMenu.Popup
                style={{ width: size, height: size }}
                className="relative rounded-full overflow-hidden shadow-xl outline-none"
                render={
                  <motion.div
                    className="absolute inset-0"
                    initial={{ opacity: 0, scale: 0.5 }}
                    animate={{ opacity: 1, scale: 1 }}
                    exit={{ opacity: 0, scale: 0.5 }}
                    transition={menuTransition}
                  />
                }
              >
                <svg
                  className="absolute inset-0 size-full"
                  viewBox={`${-radius} ${-radius} ${radius * 2} ${radius * 2}`}
                >
                  {menuItems.map((item, index) => {
                    const Icon = item.icon;
                    const midDeg = START_ANGLE + slice * index;
                    const { x: iconX, y: iconY } = polarToCartesian(
                      iconRingRadius,
                      midDeg,
                    );
                    const ICON_BOX = iconSize * 2;
                    const isActive = activeIndex === index;

                    return (
                      <g
                        key={item.id}
                        className="cursor-pointer"
                        onClick={() => itemRefs.current[index]?.click()}
                        onMouseEnter={() => {
                          setActiveIndex(index);
                          itemRefs.current[index]?.focus();
                        }}
                      >
                        <motion.path
                          d={slicePath(
                            index,
                            menuItems.length,
                            outerRingOuterRadius,
                            outerRingInnerRadius,
                          )}
                          className={cn({
                            'fill-neutral-200 dark:fill-neutral-700': isActive,
                            'fill-neutral-100 dark:fill-neutral-800': !isActive,
                          })}
                          initial={false}
                          transition={wedgeTransition}
                        />
                        <motion.path
                          d={slicePath(
                            index,
                            menuItems.length,
                            wedgeOuterRadius,
                            wedgeInnerRadius,
                          )}
                          className={cn(
                            'stroke-neutral-300 dark:stroke-neutral-600 stroke-1',
                            {
                              'fill-neutral-200 dark:fill-neutral-700':
                                isActive,
                              'fill-neutral-100 dark:fill-neutral-800':
                                !isActive,
                            },
                          )}
                          initial={false}
                          transition={wedgeTransition}
                        />

                        <foreignObject
                          x={iconX - ICON_BOX / 2}
                          y={iconY - ICON_BOX / 2}
                          width={ICON_BOX}
                          height={ICON_BOX}
                        >
                          <ContextMenu.Item
                            ref={(el) => {
                              itemRefs.current[index] =
                                el as HTMLElement | null;
                            }}
                            onFocus={() => setActiveIndex(index)}
                            onClick={() => {
                              onSelect?.(item);
                            }}
                            aria-label={item.label}
                            className={cn(
                              'size-full flex items-center justify-center rounded-full outline-none text-neutral-600 dark:text-neutral-400',
                              {
                                'text-neutral-900 dark:text-neutral-50':
                                  isActive,
                              },
                            )}
                          >
                            <Icon
                              style={{ height: iconSize, width: iconSize }}
                            />
                          </ContextMenu.Item>
                        </foreignObject>
                      </g>
                    );
                  })}

                  <circle
                    cx={0}
                    cy={0}
                    r={centerRadius}
                    className="fill-neutral-100 dark:fill-neutral-950 stroke-1 opacity-50 stroke-neutral-400 dark:stroke-neutral-600"
                  />
                  <circle
                    cx={0}
                    cy={0}
                    r={3}
                    className="fill-none stroke-neutral-400 dark:stroke-neutral-600"
                  />
                </svg>
              </ContextMenu.Popup>
            </ContextMenu.Positioner>
          </ContextMenu.Portal>
        )}
      </AnimatePresence>
    </ContextMenu.Root>
  );
}

export { RadialMenu };
```

---

### Radial Nav <a name="radial-nav"></a>

A circular navigation menu with animated pointer and expanding buttons for smooth interactive transitions.

- **Category**: Community
- **Registry Key**: `components-community-radial-nav`

#### Installation

```bash
npx shadcn@latest add components-community-radial-nav
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### How to Use

```tsx
<RadialNav items={ITEMS} />
```

#### Demo Code

##### File: `registry/demo/components/community/radial-nav/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { RadialNav } from '@/components/animate-ui/components/community/radial-nav';
import { Bookmark, LayoutGrid, User } from 'lucide-react';

const ITEMS = [
  { id: 1, icon: LayoutGrid, label: 'Projects', angle: 0 },
  { id: 2, icon: Bookmark, label: 'Bookmarks', angle: -115 },
  { id: 3, icon: User, label: 'About', angle: 115 },
];

export const RadialNavDemo = () => (
  <RadialNav items={ITEMS} defaultActiveId={1} />
);
```

#### Component Source Code

##### File: `registry/components/community/radial-nav/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { MousePointer2, type LucideIcon } from 'lucide-react';
import { motion, type Variants, type Transition } from 'motion/react';

type RadialNavProps = {
  size?: number;
  items: RadialNavItem[];
  menuButtonConfig?: MenuButtonConfig;
  defaultActiveId?: number;
  onActiveChange?: (id: number) => void;
};

type RadialNavItem = {
  id: number;
  icon: LucideIcon;
  label: string;
  angle: number;
};

type MenuButtonConfig = {
  iconSize?: number; // px
  buttonSize?: number; // px, button diameter when collapsed
  buttonPadding?: number; // px
};

const defaultMenuButtonConfig: Required<MenuButtonConfig> = {
  iconSize: 20,
  buttonSize: 40,
  buttonPadding: 8,
};

const POINTER_BASE_DEG = 45;

const POINTER_ROT_SPRING = {
  type: 'spring',
  stiffness: 220,
  damping: 26,
} as const;

const BUTTON_MOTION_CONFIG = {
  initial: 'rest',
  variants: {
    rest: { maxWidth: '40px' },
    hover: {
      maxWidth: '140px',
      transition: { type: 'spring', stiffness: 200, damping: 35, delay: 0.05 },
    },
    tap: { scale: 0.95 },
  },
  transition: { type: 'spring', stiffness: 200, damping: 25 },
} as const;

const LABEL_VARIANTS: Variants = {
  rest: { opacity: 0, x: 4 },
  hover: {
    opacity: 1,
    x: 0,
    visibility: 'visible',
    width: 'auto',
  },
  tap: { opacity: 1, x: 0, visibility: 'visible', width: 'auto' },
};

const LABEL_TRANSITION: Transition = {
  type: 'spring',
  stiffness: 200,
  damping: 25,
};

function getPolarCoordinates(angleDeg: number, r: number) {
  const rad = ((angleDeg - 90) * Math.PI) / 180;
  return { x: r * Math.cos(rad), y: r * Math.sin(rad) };
}

function calculateIconOffset({
  buttonSize,
  iconSize,
  buttonPadding,
  bias = 0,
}: {
  buttonSize: number;
  iconSize: number;
  buttonPadding: number;
  bias?: number;
}) {
  const centerOffset = (buttonSize - iconSize) / 2;
  return centerOffset - buttonPadding + bias;
}

// eslint-disable-next-line @typescript-eslint/no-explicit-any
function withDefaults<T extends Record<string, any>>(
  defaults: T,
  overrides?: Partial<T>,
): T {
  return { ...defaults, ...overrides };
}

function normalizeDeg(a: number) {
  return ((a % 360) + 360) % 360;
}

function toNearestTurn(prev: number | undefined, target: number) {
  const b = normalizeDeg(target);
  if (prev === undefined) return b;
  const k = Math.round((prev - b) / 360);
  return b + 360 * k;
}

function useShortestRotation(target: number) {
  const prevRef = React.useRef<number | undefined>(undefined);
  return React.useMemo(() => {
    const next = toNearestTurn(prevRef.current, target);
    prevRef.current = next;
    return next;
  }, [target]);
}

function MenuButton({
  item,
  isActive,
  onActivate,
  menuButtonConfig,
}: {
  item: RadialNavItem;
  isActive?: boolean;
  onActivate?: () => void;
  menuButtonConfig: Required<MenuButtonConfig>;
}) {
  const { icon: Icon, label } = item;
  const { iconSize, buttonSize, buttonPadding } = menuButtonConfig;

  const translateX = calculateIconOffset({
    ...menuButtonConfig,
    bias: -1,
  });

  return (
    <motion.button
      {...BUTTON_MOTION_CONFIG}
      initial={false}
      animate={isActive ? 'hover' : 'rest'}
      className="relative flex space-x-1 items-center overflow-hidden whitespace-nowrap rounded-full border border-neutral-800 dark:border-neutral-200 bg-background text-foreground font-medium"
      style={{
        height: buttonSize,
        minWidth: buttonSize,
        padding: buttonPadding,
      }}
      onClick={onActivate}
      type="button"
      role="menuitem"
      aria-pressed={!!isActive}
      aria-label={label}
    >
      <Icon
        className="shrink-0"
        style={{
          height: iconSize,
          width: iconSize,
          transform: `translateX(${translateX}px)`,
        }}
      />
      <motion.span
        variants={LABEL_VARIANTS}
        transition={LABEL_TRANSITION}
        className="invisible text-sm w-0"
      >
        {label}
      </motion.span>
    </motion.button>
  );
}

// orbitRadius determines how far from the center each item should be placed.
// It positions the CENTER of each small circle exactly on the parent circle's stroke.
// Formula: parentRadius (size/2) minus half of the child diameter (~0.5 accounts for border).
function RadialNav({
  size = 180,
  items,
  menuButtonConfig,
  defaultActiveId,
  onActiveChange,
}: RadialNavProps) {
  const orbitRadius = size / 2 - 0.5;
  const [activeId, setActiveId] = React.useState<number | null>(
    defaultActiveId ?? null,
  );

  const handleActivate = React.useCallback(
    (id: number) => {
      setActiveId(id);
      onActiveChange?.(id);
    },
    [onActiveChange],
  );

  const baseAngle =
    (items.find((it) => it.id === activeId)?.angle ?? 0) + POINTER_BASE_DEG;
  const rotateAngle = useShortestRotation(baseAngle);

  const resolvedMenuButtonConfig = withDefaults(
    defaultMenuButtonConfig,
    menuButtonConfig,
  );

  return (
    <div
      className="relative flex items-center justify-center rounded-full border border-neutral-800 dark:border-neutral-200"
      style={{ width: size, height: size }}
      role="menu"
      aria-label="Radial navigation"
    >
      <motion.div
        initial={false}
        className="absolute left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2"
        animate={{ rotate: rotateAngle }}
        transition={POINTER_ROT_SPRING}
        style={{ originX: 0.5, originY: 0.5 }}
        aria-hidden="true"
      >
        <MousePointer2 className="size-5 text-foreground" />
      </motion.div>
      {items.map((item) => {
        const { id, angle } = item;
        const { x, y } = getPolarCoordinates(angle, orbitRadius);
        return (
          <div
            key={id}
            className="group absolute"
            style={{
              left: `calc(50% + ${x}px)`,
              top: `calc(50% + ${y}px)`,
              transform: 'translate(-50%, -50%)',
            }}
          >
            <MenuButton
              item={item}
              isActive={activeId === id}
              onActivate={() => handleActivate(id)}
              menuButtonConfig={resolvedMenuButtonConfig}
            />
          </div>
        );
      })}
    </div>
  );
}

export {
  RadialNav,
  type RadialNavItem,
  type MenuButtonConfig,
  type RadialNavProps,
};
```

---

### Share Button <a name="share-button"></a>

A share button component with animated icons.

- **Category**: Community
- **Registry Key**: `components-community-share-button`

#### Installation

```bash
npx shadcn@latest add components-community-share-button
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `lucide-react`
  - `class-variance-authority`

#### How to Use

```tsx
<ShareButton />
```

#### Demo Code

##### File: `registry/demo/components/community/share-button/index.tsx`

```tsx
'use client';

import {
  ShareButton,
  type ShareButtonProps,
} from '@/components/animate-ui/components/community/share-button';

type ShareButtonDemoProps = {
  size?: ShareButtonProps['size'];
  icon?: ShareButtonProps['icon'];
};

export const ShareButtonDemo = ({ size, icon }: ShareButtonDemoProps) => {
  return (
    <ShareButton size={size} icon={icon}>
      Share
    </ShareButton>
  );
};
```

#### Component Source Code

##### File: `registry/components/community/share-button/index.tsx`

```tsx
'use client';
import * as React from 'react';
import { Share2, Github, X, Facebook } from 'lucide-react';
import { cva, type VariantProps } from 'class-variance-authority';
import { HTMLMotionProps, motion, AnimatePresence } from 'motion/react';

import { cn } from '@/lib/utils';

const buttonVariants = cva(
  "relative overflow-hidden cursor-pointer inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-lg text-sm font-medium transition-all disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4 shrink-0 [&_svg]:shrink-0 outline-none focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px] aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive",
  {
    variants: {
      size: {
        default: 'min-w-28 h-10 px-4 py-2',
        sm: 'min-w-24 h-9 rounded-md gap-1.5 px-3',
        md: 'min-w-28 h-10 px-4 py-2',
        lg: 'min-w-32 h-11 px-8',
      },
      icon: {
        suffix: 'pl-4',
        prefix: 'pr-4',
      },
    },
    defaultVariants: {
      size: 'default',
    },
  },
);

const iconSizeMap = {
  sm: 16,
  md: 20,
  lg: 28,
  default: 16,
};

type ShareButtonProps = HTMLMotionProps<'button'> & {
  children: React.ReactNode;
  className?: string;
  onIconClick?: (platform: 'github' | 'x' | 'facebook') => void;
} & VariantProps<typeof buttonVariants>;

function ShareButton({
  children,
  className,
  size,
  icon,
  onIconClick,
  ...props
}: ShareButtonProps) {
  const [hovered, setHovered] = React.useState(false);
  return (
    <motion.button
      className={cn(
        'bg-primary text-primary-foreground hover:bg-primary/90',
        buttonVariants({ size, className, icon }),
      )}
      onMouseEnter={() => setHovered(true)}
      onMouseLeave={() => setHovered(false)}
      {...props}
    >
      <AnimatePresence initial={false} mode="wait">
        {!hovered ? (
          <motion.div
            key="content"
            initial={{ opacity: 1, y: 0 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -24 }}
            transition={{ duration: 0.3 }}
            className=" absolute left-0 right-0 top-0 bottom-0 flex items-center justify-center gap-2"
          >
            {icon === 'prefix' && (
              <Share2
                className="size-4"
                size={iconSizeMap[size as keyof typeof iconSizeMap]}
              />
            )}
            {children}
            {icon === 'suffix' && (
              <Share2
                className="size-4"
                size={iconSizeMap[size as keyof typeof iconSizeMap]}
              />
            )}
          </motion.div>
        ) : (
          <motion.div
            key="icons"
            initial={{ opacity: 0, y: 24 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: 24 }}
            transition={{ duration: 0.3 }}
            className=" absolute left-0 right-0 top-0 bottom-0 flex items-center justify-center gap-2"
          >
            <ShareIconGroup size={size} onIconClick={onIconClick} />
          </motion.div>
        )}
      </AnimatePresence>
    </motion.button>
  );
}

const shareIconGroupVariants = cva('flex items-center justify-center gap-3', {
  variants: {
    size: {
      default: 'text-[16px]',
      sm: 'text-[16px]',
      md: 'text-[20px]',
      lg: 'text-[28px]',
    },
  },
  defaultVariants: {
    size: 'default',
  },
});

type ShareIconGroupProps = HTMLMotionProps<'div'> & {
  className?: string;
  onIconClick?: (
    platform: 'github' | 'x' | 'facebook',
    event: React.MouseEvent<HTMLDivElement>,
  ) => void;
} & VariantProps<typeof shareIconGroupVariants>;

function ShareIconGroup({
  size = 'default',
  className,
  onIconClick,
}: ShareIconGroupProps) {
  const iconSize = iconSizeMap[size as keyof typeof iconSizeMap];

  const handleIconClick = React.useCallback(
    (
      platform: 'github' | 'x' | 'facebook',
      event: React.MouseEvent<HTMLDivElement>,
    ) => {
      onIconClick?.(platform, event);
    },
    [onIconClick],
  );

  return (
    <motion.div
      className={cn(shareIconGroupVariants({ size }), 'group', className)}
    >
      <motion.div
        initial={{ opacity: 0, y: 24 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ delay: 0, duration: 0.5, type: 'spring', bounce: 0.4 }}
        whileHover={{
          y: -8,
          transition: { duration: 0.2, ease: 'easeOut' },
        }}
        className="group-hover:opacity-100 cursor-pointer py-3 rounded-lg box-border"
        onClick={(event) => handleIconClick('github', event)}
      >
        <Github size={iconSize} />
      </motion.div>
      <motion.div
        initial={{ opacity: 0, y: 24 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ delay: 0.1, duration: 0.5, type: 'spring', bounce: 0.4 }}
        whileHover={{
          y: -8,
          transition: { duration: 0.2, ease: 'easeOut' },
        }}
        className="group-hover:opacity-100 cursor-pointer py-3 rounded-lg box-border"
        onClick={(event) => handleIconClick('x', event)}
      >
        <X size={iconSize} />
      </motion.div>
      <motion.div
        initial={{ opacity: 0, y: 24 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ delay: 0.2, duration: 0.5, type: 'spring', bounce: 0.4 }}
        whileHover={{
          y: -8,
          transition: { duration: 0.2, ease: 'easeOut' },
        }}
        className="group-hover:opacity-100 cursor-pointer py-3 rounded-lg box-border"
        onClick={(event) => handleIconClick('facebook', event)}
      >
        <Facebook size={iconSize} />
      </motion.div>
    </motion.div>
  );
}

export { ShareButton, type ShareButtonProps };
```

---

### User Presence Avatar <a name="user-presence-avatar"></a>

A compact avatar group for showing online and offline users, with smooth animated transitions when presence changes.

- **Category**: Community
- **Registry Key**: `components-community-user-presence-avatar`

#### Installation

```bash
npx shadcn@latest add components-community-user-presence-avatar
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
<UserPresenceAvatar />
```

#### Demo Code

##### File: `registry/demo/components/community/user-presence-avatar/index.tsx`

```tsx
'use client';

import * as React from 'react';

import { UserPresenceAvatar } from '@/components/animate-ui/components/community/user-presence-avatar';

export const UserPresenceAvatarDemo = () => <UserPresenceAvatar />;
```

#### Component Source Code

##### File: `registry/components/community/user-presence-avatar/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { motion, LayoutGroup } from 'motion/react';

import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from '@/components/ui/avatar';
import {
  TooltipProvider,
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/animate-ui/components/animate/tooltip';
import { cn } from '@/lib/utils';

const USERS = [
  {
    id: 1,
    src: 'https://pbs.twimg.com/profile_images/1897311929028255744/otxpL-ke_400x400.jpg',
    fallback: 'AK',
    tooltip: 'Arhamkhnz',
    online: true,
  },
  {
    id: 2,
    src: 'https://pbs.twimg.com/profile_images/1948770261848756224/oPwqXMD6_400x400.jpg',
    fallback: 'SK',
    tooltip: 'Skyleen',
    online: true,
  },
  {
    id: 3,
    src: 'https://pbs.twimg.com/profile_images/1593304942210478080/TUYae5z7_400x400.jpg',
    fallback: 'CN',
    tooltip: 'Shadcn',
    online: true,
  },
  {
    id: 4,
    src: 'https://pbs.twimg.com/profile_images/1677042510839857154/Kq4tpySA_400x400.jpg',
    fallback: 'AW',
    tooltip: 'Adam Wathan',
    online: false,
  },
  {
    id: 5,
    src: 'https://pbs.twimg.com/profile_images/1783856060249595904/8TfcCN0r_400x400.jpg',
    fallback: 'GR',
    tooltip: 'Guillermo Rauch',
    online: false,
  },
  {
    id: 6,
    src: 'https://pbs.twimg.com/profile_images/1534700564810018816/anAuSfkp_400x400.jpg',
    fallback: 'JH',
    tooltip: 'Jhey',
    online: false,
  },
];

const AVATAR_MOTION_TRANSITION = {
  type: 'spring',
  stiffness: 200,
  damping: 25,
} as const;

const GROUP_CONTAINER_TRANSITION = {
  type: 'spring',
  stiffness: 150,
  damping: 20,
} as const;

function UserPresenceAvatar() {
  const [users, setUsers] = React.useState(USERS);
  const [togglingGroup, setTogglingGroup] = React.useState<
    'online' | 'offline' | null
  >(null);

  const online = users.filter((u) => u.online);
  const offline = users.filter((u) => !u.online);

  const toggleStatus = (id: number) => {
    const user = users.find((u) => u.id === id);
    if (!user) return;

    setTogglingGroup(user.online ? 'online' : 'offline');
    setUsers((prev) => {
      const idx = prev.findIndex((u) => u.id === id);
      if (idx === -1) return prev;
      const updated = [...prev];
      const target = updated[idx];
      if (!target) return prev;
      updated.splice(idx, 1);
      updated.push({ ...target, online: !target.online });
      return updated;
    });
    // Reset group z-index after the animation duration (keep in sync with animation timing)
    setTimeout(() => setTogglingGroup(null), 500);
  };

  return (
    <div className="flex items-center gap-5">
      <LayoutGroup>
        <TooltipProvider>
          {online.length > 0 && (
            <motion.div
              layout
              className={cn(
                'bg-neutral-300 dark:bg-neutral-700 p-0.5 rounded-full',
                togglingGroup === 'online' ? 'z-5' : 'z-10',
              )}
              transition={GROUP_CONTAINER_TRANSITION}
            >
              <div
                key={online.map((u) => u.id).join('_') + '-online'}
                className="flex items-center h-12 -space-x-3"
              >
                {online.map((user) => (
                  <Tooltip key={user.id}>
                    <TooltipTrigger asChild>
                      <motion.div
                        layoutId={`avatar-${user.id}`}
                        className="cursor-pointer"
                        onClick={() => toggleStatus(user.id)}
                        animate={{
                          filter: 'grayscale(0)',
                          scale: 1,
                        }}
                        transition={AVATAR_MOTION_TRANSITION}
                        title="Click to go offline"
                        initial={false}
                      >
                        <Avatar className="size-12 border-3 border-neutral-300 dark:border-neutral-700">
                          <AvatarImage src={user.src} />
                          <AvatarFallback>{user.fallback}</AvatarFallback>
                          <TooltipContent>
                            <p>{user.tooltip}</p>
                          </TooltipContent>
                        </Avatar>
                      </motion.div>
                    </TooltipTrigger>
                  </Tooltip>
                ))}
              </div>
            </motion.div>
          )}

          {offline.length > 0 && (
            <motion.div
              layout
              className={cn(
                'bg-neutral-300 dark:bg-neutral-700 p-0.5 rounded-full',
                togglingGroup === 'offline' ? 'z-5' : 'z-10',
              )}
              transition={GROUP_CONTAINER_TRANSITION}
            >
              <div
                key={offline.map((u) => u.id).join('_') + '-offline'}
                className="flex items-center h-12 -space-x-3"
              >
                {offline.map((user) => (
                  <Tooltip key={user.id}>
                    <TooltipTrigger asChild>
                      <motion.div
                        layoutId={`avatar-${user.id}`}
                        className="cursor-pointer"
                        onClick={() => toggleStatus(user.id)}
                        animate={{
                          filter: 'grayscale(1)',
                          scale: 1,
                        }}
                        transition={AVATAR_MOTION_TRANSITION}
                        title="Click to go online"
                        initial={false}
                      >
                        <Avatar className="size-12 border-3 border-neutral-300 dark:border-neutral-700">
                          <AvatarImage src={user.src} />
                          <AvatarFallback>{user.fallback}</AvatarFallback>
                          <TooltipContent>
                            <p>{user.tooltip}</p>
                          </TooltipContent>
                        </Avatar>
                      </motion.div>
                    </TooltipTrigger>
                  </Tooltip>
                ))}
              </div>
            </motion.div>
          )}
        </TooltipProvider>
      </LayoutGroup>
    </div>
  );
}

export { UserPresenceAvatar };
```

---

## Headless <a name="headless"></a>

### Accordion <a name="accordion"></a>

A vertically stacked set of interactive headings that each reveal an associated section of content.

- **Category**: Headless
- **Registry Key**: `components-headless-accordion`

#### Installation

```bash
npx shadcn@latest add components-headless-accordion
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
<Accordion>
  <AccordionItem>
    <AccordionButton>Accordion Item 1</AccordionButton>
    <AccordionPanel>
      <div>Accordion Content 1</div>
    </AccordionPanel>
  </AccordionItem>
  <AccordionItem>
    <AccordionButton>Accordion Item 2</AccordionButton>
    <AccordionPanel>
      <div>Accordion Content 2</div>
    </AccordionPanel>
  </AccordionItem>
</Accordion>
```

#### Demo Code

##### File: `registry/demo/components/headless/accordion/index.tsx`

```tsx
import {
  Accordion,
  AccordionItem,
  AccordionButton,
  AccordionPanel,
} from '@/components/animate-ui/components/headless/accordion';

const ITEMS = [
  {
    title: 'What is Animate UI?',
    content:
      'Animate UI is an open-source distribution of React components built with TypeScript, Tailwind CSS, and Motion.',
  },
  {
    title: 'How is it different from other libraries?',
    content:
      'Instead of installing via NPM, you copy and paste the components directly. This gives you full control to modify or customize them as needed.',
  },
  {
    title: 'Is Animate UI free to use?',
    content:
      'Absolutely! Animate UI is fully open-source. You can use, modify, and adapt it to fit your needs.',
  },
];

type HeadlessAccordionDemoProps = {
  keepRendered?: boolean;
  showArrow?: boolean;
};

export const HeadlessAccordionDemo = ({
  keepRendered = false,
  showArrow = true,
}: HeadlessAccordionDemoProps) => {
  return (
    <Accordion className="max-w-[400px] w-full">
      {ITEMS.map((item, index) => (
        <AccordionItem key={index}>
          <AccordionButton showArrow={showArrow}>{item.title}</AccordionButton>
          <AccordionPanel keepRendered={keepRendered}>
            {item.content}
          </AccordionPanel>
        </AccordionItem>
      ))}
    </Accordion>
  );
};
```

#### Component Source Code

##### File: `registry/components/headless/accordion/index.tsx`

```tsx
import * as React from 'react';
import { motion } from 'motion/react';
import { ChevronDownIcon } from 'lucide-react';

import {
  Disclosure as DisclosurePrimitive,
  DisclosureButton as DisclosureButtonPrimitive,
  DisclosurePanel as DisclosurePanelPrimitive,
  type DisclosureProps as DisclosurePrimitiveProps,
  type DisclosureButtonProps as DisclosureButtonPrimitiveProps,
  type DisclosurePanelProps as DisclosurePanelPrimitiveProps,
} from '@/components/animate-ui/primitives/headless/disclosure';
import { cn } from '@/lib/utils';

type AccordionProps<TTag extends React.ElementType = 'div'> =
  React.ComponentProps<TTag> & {
    children: React.ReactNode;
    as?: TTag;
  };

function Accordion<TTag extends React.ElementType = 'div'>({
  as: Component = 'div',
  ...props
}: AccordionProps<TTag>) {
  return <Component data-slot="accordion" {...props} />;
}

type AccordionItemProps<TTag extends React.ElementType = 'div'> =
  DisclosurePrimitiveProps<TTag>;

function AccordionItem<TTag extends React.ElementType = 'div'>({
  className,
  children,
  ...props
}: AccordionItemProps<TTag>) {
  return (
    <DisclosurePrimitive {...props}>
      {(bag) => (
        <div className={cn('border-b last:border-b-0', className)}>
          {typeof children === 'function' ? children(bag) : children}
        </div>
      )}
    </DisclosurePrimitive>
  );
}

type AccordionButtonProps = DisclosureButtonPrimitiveProps & {
  showArrow?: boolean;
};

function AccordionButton({
  className,
  children,
  showArrow = true,
  ...props
}: AccordionButtonProps) {
  return (
    <DisclosureButtonPrimitive
      className={cn(
        'focus-visible:border-ring focus-visible:ring-ring/50 flex flex-1 items-start justify-between gap-4 w-full rounded-md py-4 text-left text-sm font-medium transition-all outline-none hover:underline focus-visible:ring-[3px] disabled:pointer-events-none disabled:opacity-50 [&[data-open]>svg]:rotate-180',
        className,
      )}
      {...props}
    >
      {(bag) => (
        <>
          {typeof children === 'function' ? children(bag) : children}
          {showArrow && (
            <ChevronDownIcon className="text-muted-foreground pointer-events-none size-4 shrink-0 translate-y-0.5 transition-transform duration-200" />
          )}
        </>
      )}
    </DisclosureButtonPrimitive>
  );
}

type AccordionPanelProps<TTag extends React.ElementType = typeof motion.div> =
  DisclosurePanelPrimitiveProps<TTag>;

function AccordionPanel<TTag extends React.ElementType = typeof motion.div>({
  className,
  children,
  ...props
}: AccordionPanelProps<TTag>) {
  return (
    // eslint-disable-next-line @typescript-eslint/no-explicit-any
    <DisclosurePanelPrimitive<any> {...props}>
      {(bag) => (
        <div className={cn('text-sm pt-0 pb-4', className)}>
          {typeof children === 'function' ? children(bag) : children}
        </div>
      )}
    </DisclosurePanelPrimitive>
  );
}

export {
  Accordion,
  AccordionItem,
  AccordionButton,
  AccordionPanel,
  type AccordionProps,
  type AccordionItemProps,
  type AccordionButtonProps,
  type AccordionPanelProps,
};
```

---

### Checkbox <a name="checkbox"></a>

Checkboxes provide the same functionality as native HTML checkboxes, without any of the styling, giving you a clean slate to design them however you'd like.

- **Category**: Headless
- **Registry Key**: `components-headless-checkbox`

#### Installation

```bash
npx shadcn@latest add components-headless-checkbox
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
  - `motion`

#### How to Use

```tsx
<Checkbox />
```

#### Demo Code

##### File: `registry/demo/components/headless/checkbox/index.tsx`

```tsx
import { Label } from '@/components/ui/label';
import {
  Checkbox,
  type CheckboxProps,
} from '@/components/animate-ui/components/headless/checkbox';

interface HeadlessCheckboxDemoProps {
  indeterminate: boolean;
  variant: CheckboxProps['variant'];
  size: CheckboxProps['size'];
}

export const HeadlessCheckboxDemo = ({
  indeterminate,
  variant,
  size,
}: HeadlessCheckboxDemoProps) => {
  return (
    <Label className="flex items-center gap-x-3">
      <Checkbox indeterminate={indeterminate} variant={variant} size={size} />
      Accept terms and conditions
    </Label>
  );
};
```

#### Component Source Code

##### File: `registry/components/headless/checkbox/index.tsx`

```tsx
import * as React from 'react';
import { motion } from 'motion/react';

import {
  Checkbox as CheckboxPrimitive,
  CheckboxIndicator as CheckboxIndicatorPrimitive,
  type CheckboxProps as CheckboxPrimitiveProps,
} from '@/components/animate-ui/primitives/headless/checkbox';
import { cn } from '@/lib/utils';
import { cva, type VariantProps } from 'class-variance-authority';

const checkboxVariants = cva(
  'peer shrink-0 flex items-center justify-center outline-none focus-visible:ring-[3px] focus-visible:ring-ring/50 aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 disabled:cursor-not-allowed disabled:opacity-50 transition-colors duration-500 focus-visible:ring-offset-2 [&[data-checked],&[data-indeterminate]]:bg-primary [&[data-checked],&[data-indeterminate]]:text-primary-foreground',
  {
    variants: {
      variant: {
        default: 'bg-background border',
        accent: 'bg-input',
      },
      size: {
        default: 'size-5 rounded-sm',
        sm: 'size-4.5 rounded-[5px]',
        lg: 'size-6 rounded-[7px]',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  },
);

const checkboxIndicatorVariants = cva('', {
  variants: {
    size: {
      default: 'size-3.5',
      sm: 'size-3',
      lg: 'size-4',
    },
  },
  defaultVariants: {
    size: 'default',
  },
});

type CheckboxProps<TTag extends React.ElementType = typeof motion.button> =
  CheckboxPrimitiveProps<TTag> & VariantProps<typeof checkboxVariants>;

function Checkbox<TTag extends React.ElementType = typeof motion.button>({
  className,
  children,
  variant,
  size,
  ...props
}: CheckboxProps<TTag>) {
  return (
    <CheckboxPrimitive
      className={cn(checkboxVariants({ variant, size, className }))}
      {...props}
    >
      {(bag) => (
        <>
          {typeof children === 'function' ? children(bag) : children}
          <CheckboxIndicatorPrimitive
            className={cn(checkboxIndicatorVariants({ size }))}
          />
        </>
      )}
    </CheckboxPrimitive>
  );
}

export { Checkbox, type CheckboxProps };
```

---

### Dialog <a name="dialog"></a>

A fully-managed, renderless dialog component jam-packed with accessibility and keyboard features, perfect for building completely custom dialogs and alerts.

- **Category**: Headless
- **Registry Key**: `components-headless-dialog`

#### Installation

```bash
npx shadcn@latest add components-headless-dialog
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`
  - `motion`

#### How to Use

```tsx
<Dialog>
  <DialogPanel>
    <DialogHeader>
      <DialogTitle>Dialog Title</DialogTitle>
      <DialogDescription>Dialog Description</DialogDescription>
    </DialogHeader>
    <DialogFooter>
      <button>Accept</button>
    </DialogFooter>
    <DialogClose>Close</DialogClose>
  </DialogPanel>
</Dialog>
```

#### Demo Code

##### File: `registry/demo/components/headless/dialog/index.tsx`

```tsx
import * as React from 'react';

import { Button } from '@/components/ui/button';
import {
  Dialog,
  DialogPanel,
  DialogHeader,
  DialogTitle,
  DialogDescription,
  DialogFooter,
  type DialogPanelProps,
} from '@/components/animate-ui/components/headless/dialog';
import { Label } from '@/components/ui/label';
import { Input } from '@/components/ui/input';

interface HeadlessDialogDemoProps {
  from: DialogPanelProps['from'];
  showCloseButton: boolean;
}

export const HeadlessDialogDemo = ({
  from,
  showCloseButton,
}: HeadlessDialogDemoProps) => {
  const [isOpen, setIsOpen] = React.useState(false);

  return (
    <div>
      <Button variant="outline" onClick={() => setIsOpen(true)}>
        Open Dialog
      </Button>

      <Dialog open={isOpen} onClose={() => setIsOpen(false)}>
        <DialogPanel
          from={from}
          showCloseButton={showCloseButton}
          className="sm:max-w-[425px]"
        >
          <form className="flex flex-col gap-4">
            <DialogHeader>
              <DialogTitle>Edit profile</DialogTitle>
              <DialogDescription>
                Make changes to your profile here. Click save when you&apos;re
                done.
              </DialogDescription>
            </DialogHeader>
            <div className="grid gap-4">
              <div className="grid gap-3">
                <Label htmlFor="name-1">Name</Label>
                <Input id="name-1" name="name" defaultValue="Pedro Duarte" />
              </div>
              <div className="grid gap-3">
                <Label htmlFor="username-1">Username</Label>
                <Input
                  id="username-1"
                  name="username"
                  defaultValue="@peduarte"
                />
              </div>
            </div>
            <DialogFooter>
              <Button variant="outline" onClick={() => setIsOpen(false)}>
                Cancel
              </Button>
              <Button type="submit">Save changes</Button>
            </DialogFooter>
          </form>
        </DialogPanel>
      </Dialog>
    </div>
  );
};
```

#### Component Source Code

##### File: `registry/components/headless/dialog/index.tsx`

```tsx
import * as React from 'react';
import { motion } from 'motion/react';

import {
  Dialog as DialogPrimitive,
  DialogPanel as DialogPanelPrimitive,
  DialogDescription as DialogDescriptionPrimitive,
  DialogFooter as DialogFooterPrimitive,
  DialogHeader as DialogHeaderPrimitive,
  DialogTitle as DialogTitlePrimitive,
  DialogBackdrop as DialogBackdropPrimitive,
  DialogClose as DialogClosePrimitive,
  type DialogProps as DialogPrimitiveProps,
  type DialogPanelProps as DialogPanelPrimitiveProps,
  type DialogDescriptionProps as DialogDescriptionPrimitiveProps,
  type DialogFooterProps as DialogFooterPrimitiveProps,
  type DialogHeaderProps as DialogHeaderPrimitiveProps,
  type DialogTitleProps as DialogTitlePrimitiveProps,
  type DialogBackdropProps as DialogBackdropPrimitiveProps,
  type DialogCloseProps as DialogClosePrimitiveProps,
} from '@/components/animate-ui/primitives/headless/dialog';
import { cn } from '@/lib/utils';
import { XIcon } from 'lucide-react';

type DialogProps<TTag extends React.ElementType = 'div'> =
  DialogPrimitiveProps<TTag>;

function Dialog<TTag extends React.ElementType = 'div'>(
  props: DialogProps<TTag>,
) {
  return <DialogPrimitive {...props} />;
}

type DialogCloseProps<TTag extends React.ElementType = 'button'> =
  DialogClosePrimitiveProps<TTag>;

function DialogClose<TTag extends React.ElementType = 'button'>(
  props: DialogCloseProps<TTag>,
) {
  return <DialogClosePrimitive {...props} />;
}

type DialogBackdropProps<TTag extends React.ElementType = typeof motion.div> =
  DialogBackdropPrimitiveProps<TTag>;

function DialogBackdrop<TTag extends React.ElementType = typeof motion.div>({
  className,
  ...props
}: DialogBackdropProps<TTag>) {
  return (
    <DialogBackdropPrimitive
      className={cn('fixed inset-0 z-50 bg-black/50', className)}
      {...props}
    />
  );
}

type DialogPanelProps<TTag extends React.ElementType = typeof motion.div> =
  DialogPanelPrimitiveProps<TTag> & {
    showCloseButton?: boolean;
  };

function DialogPanel<TTag extends React.ElementType = typeof motion.div>({
  className,
  children,
  showCloseButton = true,
  ...props
}: DialogPanelProps<TTag>) {
  return (
    <>
      <DialogBackdrop />
      <DialogPanelPrimitive
        className={cn(
          'bg-background fixed top-[50%] left-[50%] z-50 grid w-full max-w-[calc(100%-2rem)] translate-x-[-50%] translate-y-[-50%] gap-4 rounded-lg border p-6 shadow-lg sm:max-w-lg',
          className,
        )}
        {...props}
      >
        {(bag) => (
          <>
            {typeof children === 'function' ? children(bag) : children}
            {showCloseButton && (
              <DialogClosePrimitive className="ring-offset-background focus:ring-ring data-[state=open]:bg-accent data-[state=open]:text-muted-foreground absolute top-4 right-4 rounded-xs opacity-70 transition-opacity hover:opacity-100 focus:ring-2 focus:ring-offset-2 focus:outline-hidden disabled:pointer-events-none [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4">
                <XIcon />
                <span className="sr-only">Close</span>
              </DialogClosePrimitive>
            )}
          </>
        )}
      </DialogPanelPrimitive>
    </>
  );
}

type DialogHeaderProps<TTag extends React.ElementType = 'div'> =
  DialogHeaderPrimitiveProps<TTag>;

function DialogHeader<TTag extends React.ElementType = 'div'>(
  props: DialogHeaderProps<TTag>,
) {
  const { as = 'div', className, ...rest } = props;

  return (
    <DialogHeaderPrimitive
      as={as}
      className={cn('flex flex-col gap-2 text-center sm:text-left', className)}
      {...rest}
    />
  );
}

type DialogFooterProps<TTag extends React.ElementType = 'div'> =
  DialogFooterPrimitiveProps<TTag>;

function DialogFooter<TTag extends React.ElementType = 'div'>({
  className,
  ...props
}: DialogFooterProps<TTag>) {
  return (
    <DialogFooterPrimitive
      className={cn(
        'flex flex-col-reverse gap-2 sm:flex-row sm:justify-end',
        className,
      )}
      {...props}
    />
  );
}

type DialogTitleProps<TTag extends React.ElementType = 'h2'> =
  DialogTitlePrimitiveProps<TTag>;

function DialogTitle<TTag extends React.ElementType = 'h2'>({
  className,
  ...props
}: DialogTitleProps<TTag>) {
  return (
    <DialogTitlePrimitive
      className={cn('text-lg leading-none font-semibold', className)}
      {...props}
    />
  );
}

type DialogDescriptionProps<TTag extends React.ElementType = 'div'> =
  DialogDescriptionPrimitiveProps<TTag>;

function DialogDescription<TTag extends React.ElementType = 'div'>({
  className,
  ...props
}: DialogDescriptionProps<TTag>) {
  return (
    <DialogDescriptionPrimitive
      className={cn('text-muted-foreground text-sm', className)}
      {...props}
    />
  );
}

export {
  Dialog,
  DialogClose,
  DialogPanel,
  DialogHeader,
  DialogFooter,
  DialogTitle,
  DialogDescription,
  type DialogProps,
  type DialogCloseProps,
  type DialogPanelProps,
  type DialogHeaderProps,
  type DialogFooterProps,
  type DialogTitleProps,
  type DialogDescriptionProps,
};
```

---

### Popover <a name="popover"></a>

Popovers are perfect for floating panels with arbitrary content like navigation menus, mobile menus and flyout menus.

- **Category**: Headless
- **Registry Key**: `components-headless-popover`

#### Installation

```bash
npx shadcn@latest add components-headless-popover
```

#### How to Use

```tsx
<Popover>
  <PopoverTrigger>Open Popover</PopoverTrigger>
  <PopoverContent>
    <div>Popover Content</div>
  </PopoverContent>
</Popover>
```

#### Demo Code

##### File: `registry/demo/components/headless/popover/index.tsx`

```tsx
import {
  Popover,
  PopoverButton,
  PopoverPanel,
} from '@/components/animate-ui/components/headless/popover';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';

interface HeadlessPopoverDemoProps {
  anchor?:
    | 'top'
    | 'bottom'
    | 'left'
    | 'right'
    | 'top start'
    | 'top end'
    | 'bottom start'
    | 'bottom end'
    | 'left start'
    | 'left end'
    | 'right start'
    | 'right end';
  gap?: number;
}

export function HeadlessPopoverDemo({
  anchor = 'bottom',
  gap = 4,
}: HeadlessPopoverDemoProps) {
  return (
    <Popover>
      <PopoverButton as={Button} variant="outline">
        Open popover
      </PopoverButton>

      <PopoverPanel anchor={{ to: anchor, gap }} className="w-80">
        <div className="grid gap-4">
          <div className="space-y-2">
            <h4 className="leading-none font-medium">Dimensions</h4>
            <p className="text-muted-foreground text-sm">
              Set the dimensions for the layer.
            </p>
          </div>
          <div className="grid gap-2">
            <div className="grid grid-cols-3 items-center gap-4">
              <Label htmlFor="width">Width</Label>
              <Input
                id="width"
                defaultValue="100%"
                className="col-span-2 h-8"
              />
            </div>
            <div className="grid grid-cols-3 items-center gap-4">
              <Label htmlFor="maxWidth">Max. width</Label>
              <Input
                id="maxWidth"
                defaultValue="300px"
                className="col-span-2 h-8"
              />
            </div>
            <div className="grid grid-cols-3 items-center gap-4">
              <Label htmlFor="height">Height</Label>
              <Input
                id="height"
                defaultValue="25px"
                className="col-span-2 h-8"
              />
            </div>
            <div className="grid grid-cols-3 items-center gap-4">
              <Label htmlFor="maxHeight">Max. height</Label>
              <Input
                id="maxHeight"
                defaultValue="none"
                className="col-span-2 h-8"
              />
            </div>
          </div>
        </div>
      </PopoverPanel>
    </Popover>
  );
}
```

#### Component Source Code

##### File: `registry/components/headless/popover/index.tsx`

```tsx
import * as React from 'react';

import {
  Popover as PopoverPrimitive,
  PopoverButton as PopoverButtonPrimitive,
  PopoverPanel as PopoverPanelPrimitive,
  PopoverBackdrop as PopoverBackdropPrimitive,
  PopoverGroup as PopoverGroupPrimitive,
  type PopoverProps as PopoverPrimitiveProps,
  type PopoverButtonProps as PopoverButtonPrimitiveProps,
  type PopoverPanelProps as PopoverPanelPrimitiveProps,
  type PopoverBackdropProps as PopoverBackdropPrimitiveProps,
  type PopoverGroupProps as PopoverGroupPrimitiveProps,
} from '@/components/animate-ui/primitives/headless/popover';
import { cn } from '@/lib/utils';

type PopoverProps<TTag extends React.ElementType = 'div'> =
  PopoverPrimitiveProps<TTag>;

function Popover<TTag extends React.ElementType = 'div'>(
  props: PopoverProps<TTag>,
) {
  return <PopoverPrimitive {...props} />;
}

type PopoverButtonProps<TTag extends React.ElementType = 'button'> =
  PopoverButtonPrimitiveProps<TTag>;

function PopoverButton<TTag extends React.ElementType = 'button'>(
  props: PopoverButtonProps<TTag>,
) {
  return <PopoverButtonPrimitive {...props} />;
}

type PopoverPanelProps<TTag extends React.ElementType = 'div'> =
  PopoverPanelPrimitiveProps<TTag>;

function PopoverPanel<TTag extends React.ElementType = 'div'>({
  className,
  anchor = { to: 'bottom', gap: 4 },
  ...props
}: PopoverPanelProps<TTag>) {
  return (
    <PopoverPanelPrimitive
      anchor={anchor}
      className={cn(
        'bg-popover text-popover-foreground z-50 w-72 rounded-md border p-4 shadow-md outline-hidden',
        'data-[anchor=top_center]:origin-bottom data-[anchor=top_start]:origin-bottom-left data-[anchor=top_end]:origin-bottom-right',
        'data-[anchor=bottom_center]:origin-top data-[anchor=bottom_start]:origin-top-left data-[anchor=bottom_end]:origin-top-right',
        'data-[anchor=left_center]:origin-right data-[anchor=left_start]:origin-top-right data-[anchor=left_end]:origin-bottom-right',
        'data-[anchor=right_center]:origin-left data-[anchor=right_start]:origin-top-left data-[anchor=right_end]:origin-bottom-left',
        className,
      )}
      {...props}
    />
  );
}

type PopoverBackdropProps<TTag extends React.ElementType = 'div'> =
  PopoverBackdropPrimitiveProps<TTag>;

function PopoverBackdrop<TTag extends React.ElementType = 'div'>(
  props: PopoverBackdropProps<TTag>,
) {
  return <PopoverBackdropPrimitive {...props} />;
}

type PopoverGroupProps<TTag extends React.ElementType = 'div'> =
  PopoverGroupPrimitiveProps<TTag>;

function PopoverGroup<TTag extends React.ElementType = 'div'>(
  props: PopoverGroupProps<TTag>,
) {
  return <PopoverGroupPrimitive {...props} />;
}

export {
  Popover,
  PopoverButton,
  PopoverPanel,
  PopoverBackdrop,
  PopoverGroup,
  type PopoverProps,
  type PopoverButtonProps,
  type PopoverPanelProps,
  type PopoverBackdropProps,
  type PopoverGroupProps,
};
```

---

### Switch <a name="switch"></a>

Switches are a pleasant interface for toggling a value between two states, and offer the same semantics and keyboard navigation as native checkbox elements.

- **Category**: Headless
- **Registry Key**: `components-headless-switch`

#### Installation

```bash
npx shadcn@latest add components-headless-switch
```

#### How to Use

```tsx
<Switch />
```

#### Demo Code

##### File: `registry/demo/components/headless/switch/index.tsx`

```tsx
import { Label } from '@/components/ui/label';
import { Switch } from '@/components/animate-ui/components/headless/switch';

export function HeadlessSwitchDemo() {
  return (
    <Label className="flex items-center gap-x-3">
      <Switch />
      Airplane Mode
    </Label>
  );
}
```

#### Component Source Code

##### File: `registry/components/headless/switch/index.tsx`

```tsx
import * as React from 'react';

import {
  Switch as SwitchPrimitive,
  SwitchThumb as SwitchThumbPrimitive,
  SwitchIcon as SwitchIconPrimitive,
  type SwitchProps as SwitchPrimitiveProps,
} from '@/components/animate-ui/primitives/headless/switch';
import { cn } from '@/lib/utils';

type SwitchProps = SwitchPrimitiveProps & {
  pressedWidth?: number;
  startIcon?: React.ReactElement;
  endIcon?: React.ReactElement;
  thumbIcon?: React.ReactElement;
};

function Switch({
  className,
  pressedWidth = 19,
  startIcon,
  endIcon,
  thumbIcon,
  ...props
}: SwitchProps) {
  return (
    <SwitchPrimitive
      className={cn(
        'relative peer focus-visible:border-ring focus-visible:ring-ring/50 flex h-5 w-8 px-px shrink-0 items-center justify-start rounded-full border border-transparent shadow-xs outline-none focus-visible:ring-[3px] disabled:cursor-not-allowed disabled:opacity-50',
        'data-[checked]:bg-primary bg-input dark:bg-input/80 data-[checked]:justify-end',
        className,
      )}
      {...props}
    >
      <SwitchThumbPrimitive
        className={cn(
          'relative z-10 bg-background dark:bg-foreground dark:data-[checked="true"]:bg-primary-foreground pointer-events-none block size-4 rounded-full ring-0',
        )}
        pressedAnimation={{ width: pressedWidth }}
      >
        {thumbIcon && (
          <SwitchIconPrimitive
            position="thumb"
            className="absolute [&_svg]:size-[9px] left-1/2 top-1/2 -translate-1/2 dark:text-neutral-500 text-neutral-400"
          >
            {thumbIcon}
          </SwitchIconPrimitive>
        )}
      </SwitchThumbPrimitive>

      {startIcon && (
        <SwitchIconPrimitive
          position="left"
          className="absolute [&_svg]:size-[9px] left-0.5 top-1/2 -translate-y-1/2 dark:text-neutral-500 text-neutral-400"
        >
          {startIcon}
        </SwitchIconPrimitive>
      )}
      {endIcon && (
        <SwitchIconPrimitive
          position="right"
          className="absolute [&_svg]:size-[9px] right-0.5 top-1/2 -translate-y-1/2 dark:text-neutral-400 text-neutral-500"
        >
          {endIcon}
        </SwitchIconPrimitive>
      )}
    </SwitchPrimitive>
  );
}

export { Switch, type SwitchProps };
```

---

### Tabs <a name="tabs"></a>

Easily create accessible, fully customizable tab interfaces, with robust focus management and keyboard navigation support.

- **Category**: Headless
- **Registry Key**: `components-headless-tabs`

#### Installation

```bash
npx shadcn@latest add components-headless-tabs
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
<TabGroup>
  <TabList>
    <Tab index={0}>Account</Tab>
    <Tab index={1}>Password</Tab>
  </TabList>
  <TabPanels>
    <TabPanel>Make changes to your account here.</TabPanel>
    <TabPanel>Change your password here.</TabPanel>
  </TabPanels>
</TabGroup>
```

#### Demo Code

##### File: `registry/demo/components/headless/tabs/index.tsx`

```tsx
import {
  TabGroup,
  TabPanel,
  TabPanels,
  TabList,
  Tab,
} from '@/components/animate-ui/components/headless/tabs';
import { Button } from '@/components/ui/button';
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';

export function RadixTabsDemo() {
  return (
    <div className="flex w-full max-w-sm flex-col gap-6">
      <TabGroup defaultValue="account">
        <TabList>
          <Tab index={0} value="account">
            Account
          </Tab>
          <Tab index={1} value="password">
            Password
          </Tab>
        </TabList>
        <Card className="shadow-none py-0">
          <TabPanels className="py-6">
            <TabPanel className="flex flex-col gap-6">
              <CardHeader>
                <CardTitle>Account</CardTitle>
                <CardDescription>
                  Make changes to your account here. Click save when you&apos;re
                  done.
                </CardDescription>
              </CardHeader>
              <CardContent className="grid gap-6">
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-name">Name</Label>
                  <Input id="tabs-demo-name" defaultValue="Pedro Duarte" />
                </div>
              </CardContent>
              <CardFooter>
                <Button>Save changes</Button>
              </CardFooter>
            </TabPanel>
            <TabPanel className="flex flex-col gap-6">
              <CardHeader>
                <CardTitle>Password</CardTitle>
                <CardDescription>
                  Change your password here. After saving, you&apos;ll be logged
                  out.
                </CardDescription>
              </CardHeader>
              <CardContent className="grid gap-6">
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-current">Current password</Label>
                  <Input id="tabs-demo-current" type="password" />
                </div>
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-new">New password</Label>
                  <Input id="tabs-demo-new" type="password" />
                </div>
              </CardContent>
              <CardFooter>
                <Button>Save password</Button>
              </CardFooter>
            </TabPanel>
          </TabPanels>
        </Card>
      </TabGroup>
    </div>
  );
}
```

#### Component Source Code

##### File: `registry/components/headless/tabs/index.tsx`

```tsx
import * as React from 'react';
import { motion } from 'motion/react';

import {
  TabGroup as TabGroupPrimitive,
  TabList as TabListPrimitive,
  Tab as TabPrimitive,
  TabPanel as TabPanelPrimitive,
  TabPanels as TabPanelsPrimitive,
  TabHighlight as TabHighlightPrimitive,
  TabHighlightItem as TabHighlightItemPrimitive,
  type TabGroupProps as TabGroupPrimitiveProps,
  type TabListProps as TabListPrimitiveProps,
  type TabProps as TabPrimitiveProps,
  type TabPanelProps as TabPanelPrimitiveProps,
  type TabPanelsProps as TabPanelsPrimitiveProps,
} from '@/components/animate-ui/primitives/headless/tabs';
import { cn } from '@/lib/utils';

type TabGroupProps<TTag extends React.ElementType = 'div'> =
  TabGroupPrimitiveProps<TTag>;

function TabGroup<TTag extends React.ElementType = 'div'>({
  className,
  ...props
}: TabGroupProps<TTag>) {
  return (
    <TabGroupPrimitive
      className={cn('flex flex-col gap-2', className)}
      {...props}
    />
  );
}

type TabListProps<TTag extends React.ElementType = 'div'> =
  TabListPrimitiveProps<TTag>;

function TabList<TTag extends React.ElementType = 'div'>({
  className,
  ...props
}: TabListProps<TTag>) {
  return (
    <TabHighlightPrimitive className="absolute z-0 inset-0 border border-transparent rounded-md bg-background dark:border-input dark:bg-input/30 shadow-sm">
      <TabListPrimitive
        className={cn(
          'bg-muted text-muted-foreground inline-flex h-9 w-fit items-center justify-center rounded-lg p-[3px]',
          className,
        )}
        {...props}
      />
    </TabHighlightPrimitive>
  );
}

type TabProps<TTag extends React.ElementType = 'button'> =
  TabPrimitiveProps<TTag>;

function Tab<TTag extends React.ElementType = 'button'>({
  className,
  ...props
}: TabProps<TTag>) {
  return (
    <TabHighlightItemPrimitive index={props.index} className="flex-1">
      <TabPrimitive
        className={cn(
          "data-[active='true']:text-foreground focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:outline-ring text-muted-foreground inline-flex h-[calc(100%-1px)] flex-1 items-center justify-center gap-1.5 rounded-md w-full px-2 py-1 text-sm font-medium whitespace-nowrap transition-colors duration-500 ease-in-out focus-visible:ring-[3px] focus-visible:outline-1 disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
          className,
        )}
        {...props}
      />
    </TabHighlightItemPrimitive>
  );
}

type TabPanelsProps<TTag extends React.ElementType = typeof motion.div> =
  TabPanelsPrimitiveProps<TTag>;

function TabPanels<TTag extends React.ElementType = typeof motion.div>(
  props: TabPanelsProps<TTag>,
) {
  return <TabPanelsPrimitive {...props} />;
}

type TabPanelProps<TTag extends React.ElementType = typeof motion.div> =
  TabPanelPrimitiveProps<TTag>;

function TabPanel<TTag extends React.ElementType = typeof motion.div>({
  className,
  ...props
}: TabPanelProps<TTag>) {
  return (
    <TabPanelPrimitive
      className={cn('flex-1 outline-none', className)}
      {...props}
    />
  );
}

export {
  TabGroup,
  TabList,
  Tab,
  TabPanels,
  TabPanel,
  type TabGroupProps,
  type TabListProps,
  type TabProps,
  type TabPanelsProps,
  type TabPanelProps,
};
```

---

## Radix <a name="radix"></a>

### Accordion <a name="accordion"></a>

A vertically stacked set of interactive headings that each reveal an associated section of content.

- **Category**: Radix
- **Registry Key**: `components-radix-accordion`

#### Installation

```bash
npx shadcn@latest add components-radix-accordion
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
<Accordion>
  <AccordionItem>
    <AccordionTrigger>Accordion Item 1</AccordionTrigger>
    <AccordionContent>
      <div>Accordion Content 1</div>
    </AccordionContent>
  </AccordionItem>
  <AccordionItem>
    <AccordionTrigger>Accordion Item 2</AccordionTrigger>
    <AccordionContent>
      <div>Accordion Content 2</div>
    </AccordionContent>
  </AccordionItem>
</Accordion>
```

#### Demo Code

##### File: `registry/demo/components/radix/accordion/index.tsx`

```tsx
import {
  Accordion,
  AccordionItem,
  AccordionTrigger,
  AccordionContent,
} from '@/components/animate-ui/components/radix/accordion';

const ITEMS = [
  {
    title: 'What is Animate UI?',
    content:
      'Animate UI is an open-source distribution of React components built with TypeScript, Tailwind CSS, and Motion.',
  },
  {
    title: 'How is it different from other libraries?',
    content:
      'Instead of installing via NPM, you copy and paste the components directly. This gives you full control to modify or customize them as needed.',
  },
  {
    title: 'Is Animate UI free to use?',
    content:
      'Absolutely! Animate UI is fully open-source. You can use, modify, and adapt it to fit your needs.',
  },
];

type RadixAccordionDemoProps = {
  multiple?: boolean;
  collapsible?: boolean;
  keepRendered?: boolean;
  showArrow?: boolean;
};

export const RadixAccordionDemo = ({
  multiple = false,
  collapsible = true,
  keepRendered = false,
  showArrow = true,
}: RadixAccordionDemoProps) => {
  return (
    <Accordion
      type={multiple ? 'multiple' : 'single'}
      collapsible={collapsible}
      className="max-w-[400px] w-full"
    >
      {ITEMS.map((item, index) => (
        <AccordionItem key={index} value={`item-${index + 1}`}>
          <AccordionTrigger showArrow={showArrow}>
            {item.title}
          </AccordionTrigger>
          <AccordionContent keepRendered={keepRendered}>
            {item.content}
          </AccordionContent>
        </AccordionItem>
      ))}
    </Accordion>
  );
};
```

#### Component Source Code

##### File: `registry/components/radix/accordion/index.tsx`

```tsx
import * as React from 'react';
import { ChevronDownIcon } from 'lucide-react';

import {
  Accordion as AccordionPrimitive,
  AccordionItem as AccordionItemPrimitive,
  AccordionHeader as AccordionHeaderPrimitive,
  AccordionTrigger as AccordionTriggerPrimitive,
  AccordionContent as AccordionContentPrimitive,
  type AccordionProps as AccordionPrimitiveProps,
  type AccordionItemProps as AccordionItemPrimitiveProps,
  type AccordionTriggerProps as AccordionTriggerPrimitiveProps,
  type AccordionContentProps as AccordionContentPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/accordion';
import { cn } from '@/lib/utils';

type AccordionProps = AccordionPrimitiveProps;

function Accordion(props: AccordionProps) {
  return <AccordionPrimitive {...props} />;
}

type AccordionItemProps = AccordionItemPrimitiveProps;

function AccordionItem({ className, ...props }: AccordionItemProps) {
  return (
    <AccordionItemPrimitive
      className={cn('border-b last:border-b-0', className)}
      {...props}
    />
  );
}

type AccordionTriggerProps = AccordionTriggerPrimitiveProps & {
  showArrow?: boolean;
};

function AccordionTrigger({
  className,
  children,
  showArrow = true,
  ...props
}: AccordionTriggerProps) {
  return (
    <AccordionHeaderPrimitive className="flex">
      <AccordionTriggerPrimitive
        className={cn(
          'focus-visible:border-ring focus-visible:ring-ring/50 flex flex-1 items-start justify-between gap-4 rounded-md py-4 text-left text-sm font-medium transition-all outline-none hover:underline focus-visible:ring-[3px] disabled:pointer-events-none disabled:opacity-50 [&[data-state=open]>svg]:rotate-180',
          className,
        )}
        {...props}
      >
        {children}
        {showArrow && (
          <ChevronDownIcon className="text-muted-foreground pointer-events-none size-4 shrink-0 translate-y-0.5 transition-transform duration-200" />
        )}
      </AccordionTriggerPrimitive>
    </AccordionHeaderPrimitive>
  );
}

type AccordionContentProps = AccordionContentPrimitiveProps;

function AccordionContent({
  className,
  children,
  ...props
}: AccordionContentProps) {
  return (
    <AccordionContentPrimitive {...props}>
      <div className={cn('text-sm pt-0 pb-4', className)}>{children}</div>
    </AccordionContentPrimitive>
  );
}

export {
  Accordion,
  AccordionItem,
  AccordionTrigger,
  AccordionContent,
  type AccordionProps,
  type AccordionItemProps,
  type AccordionTriggerProps,
  type AccordionContentProps,
};
```

---

### Alert Dialog <a name="alert-dialog"></a>

A modal dialog that interrupts the user with important content and expects a response.

- **Category**: Radix
- **Registry Key**: `components-radix-alert-dialog`

#### Installation

```bash
npx shadcn@latest add components-radix-alert-dialog
```

#### How to Use

```tsx
<AlertDialog>
  <AlertDialogTrigger>Open Dialog</AlertDialogTrigger>
  <AlertDialogContent>
    <AlertDialogHeader>
      <AlertDialogTitle>Alert Dialog Title</AlertDialogTitle>
      <AlertDialogDescription>Alert Dialog Description</AlertDialogDescription>
    </AlertDialogHeader>
    <AlertDialogFooter>
      <AlertDialogCancel>Cancel</AlertDialogCancel>
      <AlertDialogAction>Accept</AlertDialogAction>
    </AlertDialogFooter>
  </AlertDialogContent>
</AlertDialog>
```

#### Demo Code

##### File: `registry/demo/components/radix/alert-dialog/index.tsx`

```tsx
import * as React from 'react';

import {
  AlertDialog,
  AlertDialogTrigger,
  AlertDialogContent,
  AlertDialogHeader,
  AlertDialogTitle,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogCancel,
  AlertDialogAction,
  type AlertDialogContentProps,
} from '@/components/animate-ui/components/radix/alert-dialog';
import { Button } from '@/components/ui/button';

interface RadixAlertDialogDemoProps {
  from: AlertDialogContentProps['from'];
}

export const RadixAlertDialogDemo = ({ from }: RadixAlertDialogDemoProps) => {
  return (
    <AlertDialog>
      <AlertDialogTrigger asChild>
        <Button variant="outline">Open Dialog</Button>
      </AlertDialogTrigger>
      <AlertDialogContent from={from} className="sm:max-w-[425px]">
        <AlertDialogHeader>
          <AlertDialogTitle>Are you absolutely sure?</AlertDialogTitle>
          <AlertDialogDescription>
            This action cannot be undone. This will permanently delete your
            account and remove your data from our servers.
          </AlertDialogDescription>
        </AlertDialogHeader>
        <AlertDialogFooter>
          <AlertDialogCancel>Cancel</AlertDialogCancel>
          <AlertDialogAction>Continue</AlertDialogAction>
        </AlertDialogFooter>
      </AlertDialogContent>
    </AlertDialog>
  );
};
```

#### Component Source Code

##### File: `registry/components/radix/alert-dialog/index.tsx`

```tsx
import * as React from 'react';

import {
  AlertDialog as AlertDialogPrimitive,
  AlertDialogContent as AlertDialogContentPrimitive,
  AlertDialogDescription as AlertDialogDescriptionPrimitive,
  AlertDialogFooter as AlertDialogFooterPrimitive,
  AlertDialogHeader as AlertDialogHeaderPrimitive,
  AlertDialogTitle as AlertDialogTitlePrimitive,
  AlertDialogTrigger as AlertDialogTriggerPrimitive,
  AlertDialogPortal as AlertDialogPortalPrimitive,
  AlertDialogOverlay as AlertDialogOverlayPrimitive,
  AlertDialogAction as AlertDialogActionPrimitive,
  AlertDialogCancel as AlertDialogCancelPrimitive,
  type AlertDialogProps as AlertDialogPrimitiveProps,
  type AlertDialogContentProps as AlertDialogContentPrimitiveProps,
  type AlertDialogDescriptionProps as AlertDialogDescriptionPrimitiveProps,
  type AlertDialogFooterProps as AlertDialogFooterPrimitiveProps,
  type AlertDialogHeaderProps as AlertDialogHeaderPrimitiveProps,
  type AlertDialogTitleProps as AlertDialogTitlePrimitiveProps,
  type AlertDialogTriggerProps as AlertDialogTriggerPrimitiveProps,
  type AlertDialogOverlayProps as AlertDialogOverlayPrimitiveProps,
  type AlertDialogActionProps as AlertDialogActionPrimitiveProps,
  type AlertDialogCancelProps as AlertDialogCancelPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/alert-dialog';
import { buttonVariants } from '@/components/animate-ui/components/buttons/button';
import { cn } from '@/lib/utils';

type AlertDialogProps = AlertDialogPrimitiveProps;

function AlertDialog(props: AlertDialogProps) {
  return <AlertDialogPrimitive {...props} />;
}

type AlertDialogTriggerProps = AlertDialogTriggerPrimitiveProps;

function AlertDialogTrigger(props: AlertDialogTriggerProps) {
  return <AlertDialogTriggerPrimitive {...props} />;
}

type AlertDialogOverlayProps = AlertDialogOverlayPrimitiveProps;

function AlertDialogOverlay({ className, ...props }: AlertDialogOverlayProps) {
  return (
    <AlertDialogOverlayPrimitive
      className={cn('fixed inset-0 z-50 bg-black/50', className)}
      {...props}
    />
  );
}

type AlertDialogContentProps = AlertDialogContentPrimitiveProps;

function AlertDialogContent({ className, ...props }: AlertDialogContentProps) {
  return (
    <AlertDialogPortalPrimitive>
      <AlertDialogOverlay />
      <AlertDialogContentPrimitive
        className={cn(
          'bg-background fixed top-[50%] left-[50%] z-50 grid w-full max-w-[calc(100%-2rem)] translate-x-[-50%] translate-y-[-50%] gap-4 rounded-lg border p-6 shadow-lg sm:max-w-lg',
          className,
        )}
        {...props}
      />
    </AlertDialogPortalPrimitive>
  );
}

type AlertDialogHeaderProps = AlertDialogHeaderPrimitiveProps;

function AlertDialogHeader({ className, ...props }: AlertDialogHeaderProps) {
  return (
    <AlertDialogHeaderPrimitive
      className={cn('flex flex-col gap-2 text-center sm:text-left', className)}
      {...props}
    />
  );
}

type AlertDialogFooterProps = AlertDialogFooterPrimitiveProps;

function AlertDialogFooter({ className, ...props }: AlertDialogFooterProps) {
  return (
    <AlertDialogFooterPrimitive
      className={cn(
        'flex flex-col-reverse gap-2 sm:flex-row sm:justify-end',
        className,
      )}
      {...props}
    />
  );
}

type AlertDialogTitleProps = AlertDialogTitlePrimitiveProps;

function AlertDialogTitle({ className, ...props }: AlertDialogTitleProps) {
  return (
    <AlertDialogTitlePrimitive
      className={cn('text-lg font-semibold', className)}
      {...props}
    />
  );
}

type AlertDialogDescriptionProps = AlertDialogDescriptionPrimitiveProps;

function AlertDialogDescription({
  className,
  ...props
}: AlertDialogDescriptionProps) {
  return (
    <AlertDialogDescriptionPrimitive
      className={cn('text-muted-foreground text-sm', className)}
      {...props}
    />
  );
}

type AlertDialogActionProps = AlertDialogActionPrimitiveProps;

function AlertDialogAction({
  className,
  ...props
}: AlertDialogActionPrimitiveProps) {
  return (
    <AlertDialogActionPrimitive
      className={cn(buttonVariants(), className)}
      {...props}
    />
  );
}

type AlertDialogCancelProps = AlertDialogCancelPrimitiveProps;

function AlertDialogCancel({
  className,
  ...props
}: AlertDialogCancelPrimitiveProps) {
  return (
    <AlertDialogCancelPrimitive
      className={cn(buttonVariants({ variant: 'outline' }), className)}
      {...props}
    />
  );
}

export {
  AlertDialog,
  AlertDialogTrigger,
  AlertDialogContent,
  AlertDialogHeader,
  AlertDialogFooter,
  AlertDialogTitle,
  AlertDialogDescription,
  AlertDialogAction,
  AlertDialogCancel,
  type AlertDialogProps,
  type AlertDialogTriggerProps,
  type AlertDialogContentProps,
  type AlertDialogHeaderProps,
  type AlertDialogFooterProps,
  type AlertDialogTitleProps,
  type AlertDialogDescriptionProps,
  type AlertDialogActionProps,
  type AlertDialogCancelProps,
};
```

---

### Checkbox <a name="checkbox"></a>

A control that allows the user to toggle between checked and not checked.

- **Category**: Radix
- **Registry Key**: `components-radix-checkbox`

#### Installation

```bash
npx shadcn@latest add components-radix-checkbox
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`

#### How to Use

```tsx
<Checkbox />
```

#### Demo Code

##### File: `registry/demo/components/radix/checkbox/index.tsx`

```tsx
import { useEffect, useState } from 'react';

import { Label } from '@/components/ui/label';
import {
  Checkbox,
  type CheckboxProps,
} from '@/components/animate-ui/components/radix/checkbox';

interface RadixCheckboxDemoProps {
  checked: boolean | 'indeterminate';
  variant: CheckboxProps['variant'];
  size: CheckboxProps['size'];
}

export const RadixCheckboxDemo = ({
  checked,
  variant,
  size,
}: RadixCheckboxDemoProps) => {
  const [isChecked, setIsChecked] = useState(checked ?? false);

  useEffect(() => {
    setIsChecked(checked);
  }, [checked]);

  return (
    <Label className="flex items-center gap-x-3">
      <Checkbox
        checked={isChecked}
        onCheckedChange={setIsChecked}
        variant={variant}
        size={size}
      />
      Accept terms and conditions
    </Label>
  );
};
```

#### Component Source Code

##### File: `registry/components/radix/checkbox/index.tsx`

```tsx
import * as React from 'react';

import {
  Checkbox as CheckboxPrimitive,
  CheckboxIndicator as CheckboxIndicatorPrimitive,
  type CheckboxProps as CheckboxPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/checkbox';
import { cn } from '@/lib/utils';
import { cva, type VariantProps } from 'class-variance-authority';

const checkboxVariants = cva(
  'peer shrink-0 flex items-center justify-center outline-none focus-visible:ring-[3px] focus-visible:ring-ring/50 aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 disabled:cursor-not-allowed disabled:opacity-50 transition-colors duration-500 focus-visible:ring-offset-2 [&[data-state=checked],&[data-state=indeterminate]]:bg-primary [&[data-state=checked],&[data-state=indeterminate]]:text-primary-foreground',
  {
    variants: {
      variant: {
        default: 'bg-background border',
        accent: 'bg-input',
      },
      size: {
        default: 'size-5 rounded-sm',
        sm: 'size-4.5 rounded-[5px]',
        lg: 'size-6 rounded-[7px]',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  },
);

const checkboxIndicatorVariants = cva('', {
  variants: {
    size: {
      default: 'size-3.5',
      sm: 'size-3',
      lg: 'size-4',
    },
  },
  defaultVariants: {
    size: 'default',
  },
});

type CheckboxProps = CheckboxPrimitiveProps &
  VariantProps<typeof checkboxVariants>;

function Checkbox({
  className,
  children,
  variant,
  size,
  ...props
}: CheckboxProps) {
  return (
    <CheckboxPrimitive
      className={cn(checkboxVariants({ variant, size, className }))}
      {...props}
    >
      {children}
      <CheckboxIndicatorPrimitive
        className={cn(checkboxIndicatorVariants({ size }))}
      />
    </CheckboxPrimitive>
  );
}

export { Checkbox, type CheckboxProps };
```

---

### Dialog <a name="dialog"></a>

A window overlaid on either the primary window or another dialog window, rendering the content underneath inert.

- **Category**: Radix
- **Registry Key**: `components-radix-dialog`

#### Installation

```bash
npx shadcn@latest add components-radix-dialog
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
<Dialog>
  <DialogTrigger>Open Dialog</DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Dialog Title</DialogTitle>
      <DialogDescription>Dialog Description</DialogDescription>
    </DialogHeader>
    <p>Dialog Content</p>
    <DialogFooter>
      <button>Accept</button>
    </DialogFooter>
  </DialogContent>
</Dialog>
```

#### Demo Code

##### File: `registry/demo/components/radix/dialog/index.tsx`

```tsx
import * as React from 'react';

import { Button } from '@/components/ui/button';
import {
  Dialog,
  DialogTrigger,
  DialogContent,
  DialogHeader,
  DialogTitle,
  DialogDescription,
  DialogClose,
  DialogFooter,
  type DialogContentProps,
} from '@/components/animate-ui/components/radix/dialog';
import { Label } from '@/components/ui/label';
import { Input } from '@/components/ui/input';

interface RadixDialogDemoProps {
  from: DialogContentProps['from'];
  showCloseButton: boolean;
}

export const RadixDialogDemo = ({
  from,
  showCloseButton,
}: RadixDialogDemoProps) => {
  return (
    <Dialog>
      <form>
        <DialogTrigger asChild>
          <Button variant="outline">Open Dialog</Button>
        </DialogTrigger>
        <DialogContent
          from={from}
          showCloseButton={showCloseButton}
          className="sm:max-w-[425px]"
        >
          <DialogHeader>
            <DialogTitle>Edit profile</DialogTitle>
            <DialogDescription>
              Make changes to your profile here. Click save when you&apos;re
              done.
            </DialogDescription>
          </DialogHeader>
          <div className="grid gap-4">
            <div className="grid gap-3">
              <Label htmlFor="name-1">Name</Label>
              <Input id="name-1" name="name" defaultValue="Pedro Duarte" />
            </div>
            <div className="grid gap-3">
              <Label htmlFor="username-1">Username</Label>
              <Input id="username-1" name="username" defaultValue="@peduarte" />
            </div>
          </div>
          <DialogFooter>
            <DialogClose asChild>
              <Button variant="outline">Cancel</Button>
            </DialogClose>
            <Button type="submit">Save changes</Button>
          </DialogFooter>
        </DialogContent>
      </form>
    </Dialog>
  );
};
```

#### Component Source Code

##### File: `registry/components/radix/dialog/index.tsx`

```tsx
import * as React from 'react';
import { XIcon } from 'lucide-react';

import {
  Dialog as DialogPrimitive,
  DialogContent as DialogContentPrimitive,
  DialogDescription as DialogDescriptionPrimitive,
  DialogFooter as DialogFooterPrimitive,
  DialogHeader as DialogHeaderPrimitive,
  DialogTitle as DialogTitlePrimitive,
  DialogTrigger as DialogTriggerPrimitive,
  DialogPortal as DialogPortalPrimitive,
  DialogOverlay as DialogOverlayPrimitive,
  DialogClose as DialogClosePrimitive,
  type DialogProps as DialogPrimitiveProps,
  type DialogContentProps as DialogContentPrimitiveProps,
  type DialogDescriptionProps as DialogDescriptionPrimitiveProps,
  type DialogFooterProps as DialogFooterPrimitiveProps,
  type DialogHeaderProps as DialogHeaderPrimitiveProps,
  type DialogTitleProps as DialogTitlePrimitiveProps,
  type DialogTriggerProps as DialogTriggerPrimitiveProps,
  type DialogOverlayProps as DialogOverlayPrimitiveProps,
  type DialogCloseProps as DialogClosePrimitiveProps,
} from '@/components/animate-ui/primitives/radix/dialog';
import { cn } from '@/lib/utils';

type DialogProps = DialogPrimitiveProps;

function Dialog(props: DialogProps) {
  return <DialogPrimitive {...props} />;
}

type DialogTriggerProps = DialogTriggerPrimitiveProps;

function DialogTrigger(props: DialogTriggerProps) {
  return <DialogTriggerPrimitive {...props} />;
}

type DialogCloseProps = DialogClosePrimitiveProps;

function DialogClose(props: DialogCloseProps) {
  return <DialogClosePrimitive {...props} />;
}

type DialogOverlayProps = DialogOverlayPrimitiveProps;

function DialogOverlay({ className, ...props }: DialogOverlayProps) {
  return (
    <DialogOverlayPrimitive
      className={cn('fixed inset-0 z-50 bg-black/50', className)}
      {...props}
    />
  );
}

type DialogContentProps = DialogContentPrimitiveProps & {
  showCloseButton?: boolean;
};

function DialogContent({
  className,
  children,
  showCloseButton = true,
  ...props
}: DialogContentProps) {
  return (
    <DialogPortalPrimitive>
      <DialogOverlay />
      <DialogContentPrimitive
        className={cn(
          'bg-background fixed top-[50%] left-[50%] z-50 grid w-full max-w-[calc(100%-2rem)] translate-x-[-50%] translate-y-[-50%] gap-4 rounded-lg border p-6 shadow-lg sm:max-w-lg',
          className,
        )}
        {...props}
      >
        {children}
        {showCloseButton && (
          <DialogClosePrimitive className="ring-offset-background focus:ring-ring data-[state=open]:bg-accent data-[state=open]:text-muted-foreground absolute top-4 right-4 rounded-xs opacity-70 transition-opacity hover:opacity-100 focus:ring-2 focus:ring-offset-2 focus:outline-hidden disabled:pointer-events-none [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4">
            <XIcon />
            <span className="sr-only">Close</span>
          </DialogClosePrimitive>
        )}
      </DialogContentPrimitive>
    </DialogPortalPrimitive>
  );
}

type DialogHeaderProps = DialogHeaderPrimitiveProps;

function DialogHeader({ className, ...props }: DialogHeaderProps) {
  return (
    <DialogHeaderPrimitive
      className={cn('flex flex-col gap-2 text-center sm:text-left', className)}
      {...props}
    />
  );
}

type DialogFooterProps = DialogFooterPrimitiveProps;

function DialogFooter({ className, ...props }: DialogFooterProps) {
  return (
    <DialogFooterPrimitive
      className={cn(
        'flex flex-col-reverse gap-2 sm:flex-row sm:justify-end',
        className,
      )}
      {...props}
    />
  );
}

type DialogTitleProps = DialogTitlePrimitiveProps;

function DialogTitle({ className, ...props }: DialogTitleProps) {
  return (
    <DialogTitlePrimitive
      className={cn('text-lg leading-none font-semibold', className)}
      {...props}
    />
  );
}

type DialogDescriptionProps = DialogDescriptionPrimitiveProps;

function DialogDescription({ className, ...props }: DialogDescriptionProps) {
  return (
    <DialogDescriptionPrimitive
      className={cn('text-muted-foreground text-sm', className)}
      {...props}
    />
  );
}

export {
  Dialog,
  DialogTrigger,
  DialogClose,
  DialogContent,
  DialogHeader,
  DialogFooter,
  DialogTitle,
  DialogDescription,
  type DialogProps,
  type DialogTriggerProps,
  type DialogCloseProps,
  type DialogContentProps,
  type DialogHeaderProps,
  type DialogFooterProps,
  type DialogTitleProps,
  type DialogDescriptionProps,
};
```

---

### Dropdown Menu <a name="dropdown-menu"></a>

Displays a menu to the user — such as a set of actions or functions — triggered by a button.

- **Category**: Radix
- **Registry Key**: `components-radix-dropdown-menu`

#### Installation

```bash
npx shadcn@latest add components-radix-dropdown-menu
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
<DropdownMenu>
  <DropdownMenuTrigger>Open</DropdownMenuTrigger>

  <DropdownMenuContent>
    <DropdownMenuLabel>My Account</DropdownMenuLabel>

    <DropdownMenuSeparator />

    <DropdownMenuGroup>
      <DropdownMenuItem>
        <span>Profile</span>
        <DropdownMenuShortcut>⇧⌘P</DropdownMenuShortcut>
      </DropdownMenuItem>
      <DropdownMenuItem>
        <span>Billing</span>
        <DropdownMenuShortcut>⌘B</DropdownMenuShortcut>
      </DropdownMenuItem>
      <DropdownMenuItem>
        <span>Settings</span>
        <DropdownMenuShortcut>⌘S</DropdownMenuShortcut>
      </DropdownMenuItem>
      <DropdownMenuItem>
        <span>Keyboard shortcuts</span>
        <DropdownMenuShortcut>⌘K</DropdownMenuShortcut>
      </DropdownMenuItem>
    </DropdownMenuGroup>

    <DropdownMenuSeparator />

    <DropdownMenuGroup>
      <DropdownMenuItem>
        <span>Team</span>
      </DropdownMenuItem>

      <DropdownMenuSub>
        <DropdownMenuSubTrigger>
          <span>Invite users</span>
          <ChevronRight />
        </DropdownMenuSubTrigger>

        <DropdownMenuSubContent>
          <DropdownMenuItem>
            <span>Email</span>
          </DropdownMenuItem>
          <DropdownMenuItem>
            <span>Message</span>
          </DropdownMenuItem>

          <DropdownMenuSeparator />

          <DropdownMenuItem>
            <span>More...</span>
          </DropdownMenuItem>
        </DropdownMenuSubContent>
      </DropdownMenuSub>

      <DropdownMenuItem>
        <span>New Team</span>
        <DropdownMenuShortcut>⌘+T</DropdownMenuShortcut>
      </DropdownMenuItem>
    </DropdownMenuGroup>

    <DropdownMenuSeparator />

    <DropdownMenuItem>
      <span>Log out</span>
      <DropdownMenuShortcut>⇧⌘Q</DropdownMenuShortcut>
    </DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

#### Demo Code

##### File: `registry/demo/components/radix/dropdown-menu/index.tsx`

```tsx
import { Button } from '@/components/ui/button';
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuShortcut,
  DropdownMenuSub,
  DropdownMenuSubContent,
  DropdownMenuSubTrigger,
  DropdownMenuTrigger,
} from '@/components/animate-ui/components/radix/dropdown-menu';

interface RadixDropdownMenuDemoProps {
  side?: 'top' | 'bottom' | 'left' | 'right';
  sideOffset?: number;
  align?: 'start' | 'center' | 'end';
  alignOffset?: number;
}

export function RadixDropdownMenuDemo({
  side,
  sideOffset,
  align,
  alignOffset,
}: RadixDropdownMenuDemoProps) {
  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button variant="outline">Open</Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent
        className="w-56"
        align={align}
        alignOffset={alignOffset}
        side={side}
        sideOffset={sideOffset}
      >
        <DropdownMenuLabel>My Account</DropdownMenuLabel>
        <DropdownMenuGroup>
          <DropdownMenuItem>
            Profile
            <DropdownMenuShortcut>⇧⌘P</DropdownMenuShortcut>
          </DropdownMenuItem>
          <DropdownMenuItem>
            Billing
            <DropdownMenuShortcut>⌘B</DropdownMenuShortcut>
          </DropdownMenuItem>
          <DropdownMenuItem>
            Settings
            <DropdownMenuShortcut>⌘S</DropdownMenuShortcut>
          </DropdownMenuItem>
          <DropdownMenuItem>
            Keyboard shortcuts
            <DropdownMenuShortcut>⌘K</DropdownMenuShortcut>
          </DropdownMenuItem>
        </DropdownMenuGroup>
        <DropdownMenuSeparator />
        <DropdownMenuGroup>
          <DropdownMenuItem>Team</DropdownMenuItem>
          <DropdownMenuSub>
            <DropdownMenuSubTrigger>Invite users</DropdownMenuSubTrigger>
            <DropdownMenuSubContent>
              <DropdownMenuItem>Email</DropdownMenuItem>
              <DropdownMenuItem>Message</DropdownMenuItem>
              <DropdownMenuSeparator />
              <DropdownMenuItem>More...</DropdownMenuItem>
            </DropdownMenuSubContent>
          </DropdownMenuSub>
          <DropdownMenuItem>
            New Team
            <DropdownMenuShortcut>⌘+T</DropdownMenuShortcut>
          </DropdownMenuItem>
        </DropdownMenuGroup>
        <DropdownMenuSeparator />
        <DropdownMenuItem>GitHub</DropdownMenuItem>
        <DropdownMenuItem>Support</DropdownMenuItem>
        <DropdownMenuItem disabled>API</DropdownMenuItem>
        <DropdownMenuSeparator />
        <DropdownMenuItem variant="destructive">
          Log out
          <DropdownMenuShortcut>⇧⌘Q</DropdownMenuShortcut>
        </DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  );
}
```

#### Component Source Code

##### File: `registry/components/radix/dropdown-menu/index.tsx`

```tsx
import * as React from 'react';

import {
  DropdownMenu as DropdownMenuPrimitive,
  DropdownMenuContent as DropdownMenuContentPrimitive,
  DropdownMenuGroup as DropdownMenuGroupPrimitive,
  DropdownMenuHighlightItem as DropdownMenuHighlightItemPrimitive,
  DropdownMenuHighlight as DropdownMenuHighlightPrimitive,
  DropdownMenuItem as DropdownMenuItemPrimitive,
  DropdownMenuItemIndicator as DropdownMenuItemIndicatorPrimitive,
  DropdownMenuCheckboxItem as DropdownMenuCheckboxItemPrimitive,
  DropdownMenuRadioGroup as DropdownMenuRadioGroupPrimitive,
  DropdownMenuRadioItem as DropdownMenuRadioItemPrimitive,
  DropdownMenuLabel as DropdownMenuLabelPrimitive,
  DropdownMenuSeparator as DropdownMenuSeparatorPrimitive,
  DropdownMenuShortcut as DropdownMenuShortcutPrimitive,
  DropdownMenuSub as DropdownMenuSubPrimitive,
  DropdownMenuSubContent as DropdownMenuSubContentPrimitive,
  DropdownMenuSubTrigger as DropdownMenuSubTriggerPrimitive,
  DropdownMenuTrigger as DropdownMenuTriggerPrimitive,
  type DropdownMenuProps as DropdownMenuPrimitiveProps,
  type DropdownMenuContentProps as DropdownMenuContentPrimitiveProps,
  type DropdownMenuGroupProps as DropdownMenuGroupPrimitiveProps,
  type DropdownMenuItemProps as DropdownMenuItemPrimitiveProps,
  type DropdownMenuCheckboxItemProps as DropdownMenuCheckboxItemPrimitiveProps,
  type DropdownMenuRadioGroupProps as DropdownMenuRadioGroupPrimitiveProps,
  type DropdownMenuRadioItemProps as DropdownMenuRadioItemPrimitiveProps,
  type DropdownMenuLabelProps as DropdownMenuLabelPrimitiveProps,
  type DropdownMenuSeparatorProps as DropdownMenuSeparatorPrimitiveProps,
  type DropdownMenuShortcutProps as DropdownMenuShortcutPrimitiveProps,
  type DropdownMenuSubProps as DropdownMenuSubPrimitiveProps,
  type DropdownMenuSubContentProps as DropdownMenuSubContentPrimitiveProps,
  type DropdownMenuSubTriggerProps as DropdownMenuSubTriggerPrimitiveProps,
  type DropdownMenuTriggerProps as DropdownMenuTriggerPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/dropdown-menu';
import { cn } from '@/lib/utils';
import { CheckIcon, ChevronRightIcon, CircleIcon } from 'lucide-react';

type DropdownMenuProps = DropdownMenuPrimitiveProps;

function DropdownMenu(props: DropdownMenuProps) {
  return <DropdownMenuPrimitive {...props} />;
}

type DropdownMenuTriggerProps = DropdownMenuTriggerPrimitiveProps;

function DropdownMenuTrigger(props: DropdownMenuTriggerProps) {
  return <DropdownMenuTriggerPrimitive {...props} />;
}

type DropdownMenuContentProps = DropdownMenuContentPrimitiveProps;

function DropdownMenuContent({
  sideOffset = 4,
  className,
  children,
  ...props
}: DropdownMenuContentProps) {
  return (
    <DropdownMenuContentPrimitive
      sideOffset={sideOffset}
      className={cn(
        'bg-popover text-popover-foreground z-50 max-h-(--radix-dropdown-menu-content-available-height) min-w-[8rem] origin-(--radix-dropdown-menu-content-transform-origin) overflow-x-hidden overflow-y-auto rounded-md border p-1 shadow-md outline-none',
        className,
      )}
      {...props}
    >
      <DropdownMenuHighlightPrimitive className="absolute inset-0 bg-accent z-0 rounded-sm">
        {children}
      </DropdownMenuHighlightPrimitive>
    </DropdownMenuContentPrimitive>
  );
}

type DropdownMenuGroupProps = DropdownMenuGroupPrimitiveProps;

function DropdownMenuGroup({ ...props }: DropdownMenuGroupProps) {
  return <DropdownMenuGroupPrimitive {...props} />;
}

type DropdownMenuItemProps = DropdownMenuItemPrimitiveProps & {
  inset?: boolean;
  variant?: 'default' | 'destructive';
};

function DropdownMenuItem({
  className,
  inset,
  variant = 'default',
  disabled,
  ...props
}: DropdownMenuItemProps) {
  return (
    <DropdownMenuHighlightItemPrimitive
      activeClassName={
        variant === 'destructive'
          ? 'bg-destructive/10 dark:bg-destructive/20'
          : ''
      }
      disabled={disabled}
    >
      <DropdownMenuItemPrimitive
        disabled={disabled}
        data-inset={inset}
        data-variant={variant}
        className={cn(
          "focus:text-accent-foreground data-[variant=destructive]:text-destructive data-[variant=destructive]:focus:text-destructive data-[variant=destructive]:*:[svg]:!text-destructive [&_svg:not([class*='text-'])]:text-muted-foreground relative flex cursor-default items-center gap-2 rounded-sm px-2 py-1.5 text-sm outline-hidden select-none data-[disabled=true]:pointer-events-none data-[disabled=true]:opacity-50 data-[inset]:pl-8 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
          className,
        )}
        {...props}
      />
    </DropdownMenuHighlightItemPrimitive>
  );
}

type DropdownMenuCheckboxItemProps = DropdownMenuCheckboxItemPrimitiveProps;

function DropdownMenuCheckboxItem({
  className,
  children,
  checked,
  disabled,
  ...props
}: DropdownMenuCheckboxItemProps) {
  return (
    <DropdownMenuHighlightItemPrimitive disabled={disabled}>
      <DropdownMenuCheckboxItemPrimitive
        disabled={disabled}
        className={cn(
          "focus:text-accent-foreground relative flex cursor-default items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm outline-hidden select-none data-[disabled=true]:pointer-events-none data-[disabled=true]:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
          className,
        )}
        checked={checked}
        {...props}
      >
        <span className="pointer-events-none absolute left-2 flex size-3.5 items-center justify-center">
          <DropdownMenuItemIndicatorPrimitive
            initial={{ opacity: 0, scale: 0.5 }}
            animate={{ opacity: 1, scale: 1 }}
          >
            <CheckIcon className="size-4" />
          </DropdownMenuItemIndicatorPrimitive>
        </span>
        {children}
      </DropdownMenuCheckboxItemPrimitive>
    </DropdownMenuHighlightItemPrimitive>
  );
}

type DropdownMenuRadioGroupProps = DropdownMenuRadioGroupPrimitiveProps;

function DropdownMenuRadioGroup(props: DropdownMenuRadioGroupProps) {
  return <DropdownMenuRadioGroupPrimitive {...props} />;
}

type DropdownMenuRadioItemProps = DropdownMenuRadioItemPrimitiveProps;

function DropdownMenuRadioItem({
  className,
  children,
  disabled,
  ...props
}: DropdownMenuRadioItemProps) {
  return (
    <DropdownMenuHighlightItemPrimitive disabled={disabled}>
      <DropdownMenuRadioItemPrimitive
        disabled={disabled}
        className={cn(
          "focus:text-accent-foreground relative flex cursor-default items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm outline-hidden select-none data-[disabled=true]:pointer-events-none data-[disabled=true]:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
          className,
        )}
        {...props}
      >
        <span className="pointer-events-none absolute left-2 flex size-3.5 items-center justify-center">
          <DropdownMenuItemIndicatorPrimitive layoutId="dropdown-menu-item-indicator-radio">
            <CircleIcon className="size-2 fill-current" />
          </DropdownMenuItemIndicatorPrimitive>
        </span>
        {children}
      </DropdownMenuRadioItemPrimitive>
    </DropdownMenuHighlightItemPrimitive>
  );
}

type DropdownMenuLabelProps = DropdownMenuLabelPrimitiveProps & {
  inset?: boolean;
};

function DropdownMenuLabel({
  className,
  inset,
  ...props
}: DropdownMenuLabelProps) {
  return (
    <DropdownMenuLabelPrimitive
      data-inset={inset}
      className={cn(
        'px-2 py-1.5 text-sm font-medium data-[inset]:pl-8',
        className,
      )}
      {...props}
    />
  );
}

type DropdownMenuSeparatorProps = DropdownMenuSeparatorPrimitiveProps;

function DropdownMenuSeparator({
  className,
  ...props
}: DropdownMenuSeparatorProps) {
  return (
    <DropdownMenuSeparatorPrimitive
      className={cn('bg-border -mx-1 my-1 h-px', className)}
      {...props}
    />
  );
}

type DropdownMenuShortcutProps = DropdownMenuShortcutPrimitiveProps;

function DropdownMenuShortcut({
  className,
  ...props
}: DropdownMenuShortcutProps) {
  return (
    <DropdownMenuShortcutPrimitive
      className={cn(
        'text-muted-foreground ml-auto text-xs tracking-widest',
        className,
      )}
      {...props}
    />
  );
}

type DropdownMenuSubProps = DropdownMenuSubPrimitiveProps;

function DropdownMenuSub(props: DropdownMenuSubProps) {
  return <DropdownMenuSubPrimitive {...props} />;
}

type DropdownMenuSubTriggerProps = DropdownMenuSubTriggerPrimitiveProps & {
  inset?: boolean;
};

function DropdownMenuSubTrigger({
  disabled,
  className,
  inset,
  children,
  ...props
}: DropdownMenuSubTriggerProps) {
  return (
    <DropdownMenuHighlightItemPrimitive disabled={disabled}>
      <DropdownMenuSubTriggerPrimitive
        disabled={disabled}
        data-inset={inset}
        className={cn(
          'focus:text-accent-foreground data-[state=open]:text-accent-foreground flex cursor-default items-center rounded-sm px-2 py-1.5 text-sm outline-hidden select-none data-[inset]:pl-8',
          'data-[state=open]:[&_[data-slot=chevron]]:rotate-90 [&_[data-slot=chevron]]:transition-transform [&_[data-slot=chevron]]:duration-300 [&_[data-slot=chevron]]:ease-in-out',
          className,
        )}
        {...props}
      >
        {children}
        <ChevronRightIcon data-slot="chevron" className="ml-auto size-4" />
      </DropdownMenuSubTriggerPrimitive>
    </DropdownMenuHighlightItemPrimitive>
  );
}

type DropdownMenuSubContentProps = DropdownMenuSubContentPrimitiveProps;

function DropdownMenuSubContent({
  className,
  ...props
}: DropdownMenuSubContentProps) {
  return (
    <DropdownMenuSubContentPrimitive
      className={cn(
        'bg-popover text-popover-foreground z-50 min-w-[8rem] origin-(--radix-dropdown-menu-content-transform-origin) overflow-hidden rounded-md border p-1 shadow-lg outline-none',
        className,
      )}
      {...props}
    />
  );
}

export {
  DropdownMenu,
  DropdownMenuTrigger,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuItem,
  DropdownMenuCheckboxItem,
  DropdownMenuRadioGroup,
  DropdownMenuRadioItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuShortcut,
  DropdownMenuSub,
  DropdownMenuSubTrigger,
  DropdownMenuSubContent,
  type DropdownMenuProps,
  type DropdownMenuTriggerProps,
  type DropdownMenuContentProps,
  type DropdownMenuGroupProps,
  type DropdownMenuItemProps,
  type DropdownMenuCheckboxItemProps,
  type DropdownMenuRadioGroupProps,
  type DropdownMenuRadioItemProps,
  type DropdownMenuLabelProps,
  type DropdownMenuSeparatorProps,
  type DropdownMenuShortcutProps,
  type DropdownMenuSubProps,
  type DropdownMenuSubTriggerProps,
  type DropdownMenuSubContentProps,
};
```

---

### Files <a name="files"></a>

A component that allows you to display a list of files and folders.

- **Category**: Radix
- **Registry Key**: `components-radix-files`

#### Installation

```bash
npx shadcn@latest add components-radix-files
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
<Files>
  <FolderItem value="app">
    <FolderTrigger>app</FolderTrigger>

    <FolderContent>
      <SubFiles>
        <FolderItem value="(home)">
          <FolderTrigger>(home)</FolderTrigger>

          <FolderContent>
            <FileItem>page.tsx</FileItem>
            <FileItem>layout.tsx</FileItem>
          </FolderContent>
        </FolderItem>

        <FileItem>layout.tsx</FileItem>
        <FileItem>page.tsx</FileItem>
        <FileItem>global.css</FileItem>
      </SubFiles>
    </FolderContent>
  </FolderItem>

  <FolderItem value="components">
    <FolderTrigger>components</FolderTrigger>

    <FolderContent>
      <SubFiles>
        <FileItem>button.tsx</FileItem>
        <FileItem>tabs.tsx</FileItem>
        <FileItem>dialog.tsx</FileItem>

        <FolderItem value="empty">
          <FolderTrigger>empty</FolderTrigger>
        </FolderItem>
      </SubFiles>
    </FolderContent>
  </FolderItem>

  <FileItem>package.json</FileItem>
</Files>
```

#### Demo Code

##### File: `registry/demo/components/radix/files/index.tsx`

```tsx
'use client';

import React from 'react';
import {
  FileItem,
  FolderItem,
  FolderTrigger,
  FolderContent,
  Files,
  SubFiles,
} from '@/components/animate-ui/components/radix/files';
import { FileJsonIcon } from 'lucide-react';

export const RadixFilesDemo = () => {
  return (
    <div className="relative max-w-[500px] max-h-[350px] size-full rounded-2xl border bg-background overflow-auto">
      <Files className="w-full" defaultOpen={['app']}>
        <FolderItem value="app">
          <FolderTrigger
            gitStatus="modified"
            className="w-full flex items-center justify-between"
          >
            app
          </FolderTrigger>

          <FolderContent>
            <SubFiles defaultOpen={['(home)']}>
              <FolderItem value="(home)">
                <FolderTrigger gitStatus="untracked">(home)</FolderTrigger>

                <FolderContent>
                  <FileItem gitStatus="untracked">page.tsx</FileItem>
                  <FileItem gitStatus="untracked">layout.tsx</FileItem>
                </FolderContent>
              </FolderItem>

              <FileItem>layout.tsx</FileItem>
              <FileItem gitStatus="modified">page.tsx</FileItem>
              <FileItem>global.css</FileItem>
            </SubFiles>
          </FolderContent>
        </FolderItem>

        <FolderItem value="components">
          <FolderTrigger>components</FolderTrigger>

          <FolderContent>
            <SubFiles>
              <FileItem>button.tsx</FileItem>
              <FileItem>tabs.tsx</FileItem>
              <FileItem>dialog.tsx</FileItem>

              <FolderItem value="empty">
                <FolderTrigger>empty</FolderTrigger>
              </FolderItem>
            </SubFiles>
          </FolderContent>
        </FolderItem>

        <FileItem icon={FileJsonIcon}>package.json</FileItem>
      </Files>
    </div>
  );
};
```

#### Component Source Code

##### File: `registry/components/radix/files/index.tsx`

```tsx
import * as React from 'react';
import { FolderIcon, FolderOpenIcon, FileIcon } from 'lucide-react';

import {
  Files as FilesPrimitive,
  FilesHighlight as FilesHighlightPrimitive,
  FolderItem as FolderItemPrimitive,
  FolderHeader as FolderHeaderPrimitive,
  FolderTrigger as FolderTriggerPrimitive,
  FolderHighlight as FolderHighlightPrimitive,
  Folder as FolderPrimitive,
  FolderIcon as FolderIconPrimitive,
  FileLabel as FileLabelPrimitive,
  FolderContent as FolderContentPrimitive,
  FileHighlight as FileHighlightPrimitive,
  File as FilePrimitive,
  FileIcon as FileIconPrimitive,
  type FilesProps as FilesPrimitiveProps,
  type FolderItemProps as FolderItemPrimitiveProps,
  type FolderContentProps as FolderContentPrimitiveProps,
  type FileProps as FilePrimitiveProps,
  type FileLabelProps as FileLabelPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/files';
import { cn } from '@/lib/utils';

type GitStatus = 'untracked' | 'modified' | 'deleted';

type FilesProps = FilesPrimitiveProps;

function Files({ className, children, ...props }: FilesProps) {
  return (
    <FilesPrimitive className={cn('p-2 w-full', className)} {...props}>
      <FilesHighlightPrimitive className="bg-accent rounded-lg pointer-events-none">
        {children}
      </FilesHighlightPrimitive>
    </FilesPrimitive>
  );
}

type SubFilesProps = FilesProps;

function SubFiles(props: SubFilesProps) {
  return <FilesPrimitive {...props} />;
}

type FolderItemProps = FolderItemPrimitiveProps;

function FolderItem(props: FolderItemProps) {
  return <FolderItemPrimitive {...props} />;
}

type FolderTriggerProps = FileLabelPrimitiveProps & {
  gitStatus?: GitStatus;
};

function FolderTrigger({
  children,
  className,
  gitStatus,
  ...props
}: FolderTriggerProps) {
  return (
    <FolderHeaderPrimitive>
      <FolderTriggerPrimitive className="w-full text-start">
        <FolderHighlightPrimitive>
          <FolderPrimitive className="flex items-center justify-between gap-2 p-2 pointer-events-none">
            <div
              className={cn(
                'flex items-center gap-2',
                gitStatus === 'untracked' && 'text-green-400',
                gitStatus === 'modified' && 'text-amber-400',
                gitStatus === 'deleted' && 'text-red-400',
              )}
            >
              <FolderIconPrimitive
                closeIcon={<FolderIcon className="size-4.5" />}
                openIcon={<FolderOpenIcon className="size-4.5" />}
              />
              <FileLabelPrimitive
                className={cn('text-sm', className)}
                {...props}
              >
                {children}
              </FileLabelPrimitive>
            </div>

            {gitStatus && (
              <span
                className={cn(
                  'rounded-full size-2',
                  gitStatus === 'untracked' && 'bg-green-400',
                  gitStatus === 'modified' && 'bg-amber-400',
                  gitStatus === 'deleted' && 'bg-red-400',
                )}
              />
            )}
          </FolderPrimitive>
        </FolderHighlightPrimitive>
      </FolderTriggerPrimitive>
    </FolderHeaderPrimitive>
  );
}

type FolderContentProps = FolderContentPrimitiveProps;

function FolderContent(props: FolderContentProps) {
  return (
    <div className="relative ml-6 before:absolute before:-left-2 before:inset-y-0 before:w-px before:h-full before:bg-border">
      <FolderContentPrimitive {...props} />
    </div>
  );
}

type FileItemProps = FilePrimitiveProps & {
  icon?: React.ElementType;
  gitStatus?: GitStatus;
};

function FileItem({
  icon: Icon = FileIcon,
  className,
  children,
  gitStatus,
  ...props
}: FileItemProps) {
  return (
    <FileHighlightPrimitive>
      <FilePrimitive
        className={cn(
          'flex items-center justify-between gap-2 p-2 pointer-events-none',
          gitStatus === 'untracked' && 'text-green-400',
          gitStatus === 'modified' && 'text-amber-400',
          gitStatus === 'deleted' && 'text-red-400',
        )}
      >
        <div className="flex items-center gap-2">
          <FileIconPrimitive>
            <Icon className="size-4.5" />
          </FileIconPrimitive>
          <FileLabelPrimitive className={cn('text-sm', className)} {...props}>
            {children}
          </FileLabelPrimitive>
        </div>

        {gitStatus && (
          <span className="text-sm font-medium">
            {gitStatus === 'untracked' && 'U'}
            {gitStatus === 'modified' && 'M'}
            {gitStatus === 'deleted' && 'D'}
          </span>
        )}
      </FilePrimitive>
    </FileHighlightPrimitive>
  );
}

export {
  Files,
  FolderItem,
  FolderTrigger,
  FolderContent,
  FileItem,
  SubFiles,
  type FilesProps,
  type FolderItemProps,
  type FolderTriggerProps,
  type FolderContentProps,
  type FileItemProps,
  type SubFilesProps,
};
```

---

### Hover Card <a name="hover-card"></a>

For sighted users to preview content available behind a link.

- **Category**: Radix
- **Registry Key**: `components-radix-hover-card`

#### Installation

```bash
npx shadcn@latest add components-radix-hover-card
```

#### How to Use

```tsx
<HoverCard>
  <HoverCardTrigger>Hover me</HoverCardTrigger>
  <HoverCardContent>
    <p>Hover card content</p>
  </HoverCardContent>
</HoverCard>
```

#### Demo Code

##### File: `registry/demo/components/radix/hover-card/index.tsx`

```tsx
import {
  HoverCard,
  HoverCardTrigger,
  HoverCardContent,
} from '@/components/animate-ui/components/radix/hover-card';

interface RadixHoverCardDemoProps {
  side?: 'top' | 'bottom' | 'left' | 'right';
  sideOffset?: number;
  align?: 'start' | 'center' | 'end';
  alignOffset?: number;
  followCursor?: boolean | 'x' | 'y';
}

export const RadixHoverCardDemo = ({
  side,
  sideOffset,
  align,
  alignOffset,
  followCursor,
}: RadixHoverCardDemoProps) => {
  return (
    <HoverCard followCursor={followCursor}>
      <HoverCardTrigger asChild>
        <a
          className="size-12 border rounded-full overflow-hidden"
          href="https://twitter.com/animate_ui"
          target="_blank"
          rel="noreferrer noopener"
        >
          <img
            src="https://pbs.twimg.com/profile_images/1950218390741618688/72447Y7e_400x400.jpg"
            alt="Animate UI"
          />
        </a>
      </HoverCardTrigger>

      <HoverCardContent
        side={side}
        sideOffset={sideOffset}
        align={align}
        alignOffset={alignOffset}
        className="w-80"
      >
        <div className="flex flex-col gap-4">
          <img
            className="size-16 rounded-full overflow-hidden border"
            src="https://pbs.twimg.com/profile_images/1950218390741618688/72447Y7e_400x400.jpg"
            alt="Animate UI"
          />
          <div className="flex flex-col gap-4">
            <div>
              <div className="font-bold">Animate UI</div>
              <div className="text-sm text-muted-foreground">@animate_ui</div>
            </div>
            <div className="text-sm text-muted-foreground">
              A fully animated, open-source component distribution built with
              React, TypeScript, Tailwind CSS, and Motion.
            </div>
            <div className="flex gap-4">
              <div className="flex gap-1 text-sm items-center">
                <div className="font-bold">0</div>{' '}
                <div className="text-muted-foreground">Following</div>
              </div>
              <div className="flex gap-1 text-sm items-center">
                <div className="font-bold">2,900</div>{' '}
                <div className="text-muted-foreground">Followers</div>
              </div>
            </div>
          </div>
        </div>
      </HoverCardContent>
    </HoverCard>
  );
};
```

#### Component Source Code

##### File: `registry/components/radix/hover-card/index.tsx`

```tsx
import * as React from 'react';

import {
  HoverCard as HoverCardPrimitive,
  HoverCardTrigger as HoverCardTriggerPrimitive,
  HoverCardPortal as HoverCardPortalPrimitive,
  HoverCardContent as HoverCardContentPrimitive,
  type HoverCardProps as HoverCardPrimitiveProps,
  type HoverCardTriggerProps as HoverCardTriggerPrimitiveProps,
  type HoverCardContentProps as HoverCardContentPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/hover-card';
import { cn } from '@/lib/utils';

type HoverCardProps = HoverCardPrimitiveProps;

function HoverCard(props: HoverCardProps) {
  return <HoverCardPrimitive {...props} />;
}

type HoverCardTriggerProps = HoverCardTriggerPrimitiveProps;

function HoverCardTrigger(props: HoverCardTriggerProps) {
  return <HoverCardTriggerPrimitive {...props} />;
}

type HoverCardContentProps = HoverCardContentPrimitiveProps;

function HoverCardContent({
  className,
  align = 'center',
  sideOffset = 4,
  ...props
}: HoverCardContentProps) {
  return (
    <HoverCardPortalPrimitive>
      <HoverCardContentPrimitive
        align={align}
        sideOffset={sideOffset}
        className={cn(
          'bg-popover text-popover-foreground z-50 w-64 origin-(--radix-hover-card-content-transform-origin) rounded-md border p-4 shadow-md outline-hidden',
          className,
        )}
        {...props}
      />
    </HoverCardPortalPrimitive>
  );
}

export {
  HoverCard,
  HoverCardTrigger,
  HoverCardContent,
  type HoverCardProps,
  type HoverCardTriggerProps,
  type HoverCardContentProps,
};
```

---

### Popover <a name="popover"></a>

Displays rich content in a portal, triggered by a button.

- **Category**: Radix
- **Registry Key**: `components-radix-popover`

#### Installation

```bash
npx shadcn@latest add components-radix-popover
```

#### How to Use

```tsx
<Popover>
  <PopoverTrigger>Open popover</PopoverTrigger>
  <PopoverContent>
    <div>Popover content</div>
    <PopoverClose>Close popover</PopoverClose>
  </PopoverContent>
</Popover>
```

#### Demo Code

##### File: `registry/demo/components/radix/popover/index.tsx`

```tsx
import {
  Popover,
  PopoverTrigger,
  PopoverContent,
} from '@/components/animate-ui/components/radix/popover';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';

interface RadixPopoverDemoProps {
  side?: 'top' | 'bottom' | 'left' | 'right';
  sideOffset?: number;
  align?: 'start' | 'center' | 'end';
  alignOffset?: number;
}

export const RadixPopoverDemo = ({
  side,
  sideOffset,
  align,
  alignOffset,
}: RadixPopoverDemoProps) => {
  return (
    <Popover>
      <PopoverTrigger asChild>
        <Button variant="outline">Open popover</Button>
      </PopoverTrigger>
      <PopoverContent
        side={side}
        sideOffset={sideOffset}
        align={align}
        alignOffset={alignOffset}
        className="w-80"
      >
        <div className="grid gap-4">
          <div className="space-y-2">
            <h4 className="leading-none font-medium">Dimensions</h4>
            <p className="text-muted-foreground text-sm">
              Set the dimensions for the layer.
            </p>
          </div>
          <div className="grid gap-2">
            <div className="grid grid-cols-3 items-center gap-4">
              <Label htmlFor="width">Width</Label>
              <Input
                id="width"
                defaultValue="100%"
                className="col-span-2 h-8"
              />
            </div>
            <div className="grid grid-cols-3 items-center gap-4">
              <Label htmlFor="maxWidth">Max. width</Label>
              <Input
                id="maxWidth"
                defaultValue="300px"
                className="col-span-2 h-8"
              />
            </div>
            <div className="grid grid-cols-3 items-center gap-4">
              <Label htmlFor="height">Height</Label>
              <Input
                id="height"
                defaultValue="25px"
                className="col-span-2 h-8"
              />
            </div>
            <div className="grid grid-cols-3 items-center gap-4">
              <Label htmlFor="maxHeight">Max. height</Label>
              <Input
                id="maxHeight"
                defaultValue="none"
                className="col-span-2 h-8"
              />
            </div>
          </div>
        </div>
      </PopoverContent>
    </Popover>
  );
};
```

#### Component Source Code

##### File: `registry/components/radix/popover/index.tsx`

```tsx
import * as React from 'react';

import {
  Popover as PopoverPrimitive,
  PopoverTrigger as PopoverTriggerPrimitive,
  PopoverContent as PopoverContentPrimitive,
  PopoverPortal as PopoverPortalPrimitive,
  PopoverClose as PopoverClosePrimitive,
  type PopoverProps as PopoverPrimitiveProps,
  type PopoverTriggerProps as PopoverTriggerPrimitiveProps,
  type PopoverContentProps as PopoverContentPrimitiveProps,
  type PopoverCloseProps as PopoverClosePrimitiveProps,
} from '@/components/animate-ui/primitives/radix/popover';
import { cn } from '@/lib/utils';

type PopoverProps = PopoverPrimitiveProps;

function Popover(props: PopoverProps) {
  return <PopoverPrimitive {...props} />;
}

type PopoverTriggerProps = PopoverTriggerPrimitiveProps;

function PopoverTrigger(props: PopoverTriggerProps) {
  return <PopoverTriggerPrimitive {...props} />;
}

type PopoverContentProps = PopoverContentPrimitiveProps;

function PopoverContent({
  className,
  align = 'center',
  sideOffset = 4,
  ...props
}: PopoverContentProps) {
  return (
    <PopoverPortalPrimitive>
      <PopoverContentPrimitive
        align={align}
        sideOffset={sideOffset}
        className={cn(
          'bg-popover text-popover-foreground z-50 w-72 origin-(--radix-popover-content-transform-origin) rounded-md border p-4 shadow-md outline-hidden',
          className,
        )}
        {...props}
      />
    </PopoverPortalPrimitive>
  );
}

type PopoverCloseProps = PopoverClosePrimitiveProps;

function PopoverClose(props: PopoverCloseProps) {
  return <PopoverClosePrimitive {...props} />;
}

export {
  Popover,
  PopoverTrigger,
  PopoverContent,
  PopoverClose,
  type PopoverProps,
  type PopoverTriggerProps,
  type PopoverContentProps,
  type PopoverCloseProps,
};
```

---

### Preview Link Card <a name="preview-link-card"></a>

Displays a preview image of a link when hovered.

- **Category**: Radix
- **Registry Key**: `components-radix-preview-link-card`

#### Installation

```bash
npx shadcn@latest add components-radix-preview-link-card
```

#### How to Use

```tsx
<PreviewLinkCard href="https://animate-ui.com/docs">
  <PreviewLinkCardTrigger>Animate UI Docs</PreviewLinkCardTrigger>
  <PreviewLinkCardContent>
    <PreviewLinkCardImage alt="Preview link card content" />
  </PreviewLinkCardContent>
</PreviewLinkCard>
```

#### Demo Code

##### File: `registry/demo/components/radix/preview-link-card/index.tsx`

```tsx
import {
  PreviewLinkCard,
  PreviewLinkCardTrigger,
  PreviewLinkCardContent,
  PreviewLinkCardImage,
} from '@/components/animate-ui/components/radix/preview-link-card';

interface RadixPreviewLinkCardDemoProps {
  side?: 'top' | 'bottom' | 'left' | 'right';
  sideOffset?: number;
  align?: 'start' | 'center' | 'end';
  alignOffset?: number;
  followCursor?: boolean | 'x' | 'y';
  href: string;
}

export const RadixPreviewLinkCardDemo = ({
  side,
  sideOffset,
  align,
  alignOffset,
  followCursor,
  href,
}: RadixPreviewLinkCardDemoProps) => {
  return (
    <p className="text-muted-foreground">
      Read the{' '}
      <PreviewLinkCard href={href} followCursor={followCursor}>
        <PreviewLinkCardTrigger
          target="_blank"
          className="underline text-foreground"
        >
          Animate UI Docs
        </PreviewLinkCardTrigger>

        <PreviewLinkCardContent
          side={side}
          sideOffset={sideOffset}
          align={align}
          alignOffset={alignOffset}
          target="_blank"
        >
          <PreviewLinkCardImage alt="Animate UI Docs" />
        </PreviewLinkCardContent>
      </PreviewLinkCard>{' '}
      — hover to preview, click to dive in.
    </p>
  );
};
```

#### Component Source Code

##### File: `registry/components/radix/preview-link-card/index.tsx`

```tsx
import * as React from 'react';

import {
  PreviewLinkCard as PreviewLinkCardPrimitive,
  PreviewLinkCardTrigger as PreviewLinkCardTriggerPrimitive,
  PreviewLinkCardPortal as PreviewLinkCardPortalPrimitive,
  PreviewLinkCardContent as PreviewLinkCardContentPrimitive,
  PreviewLinkCardImage as PreviewLinkCardImagePrimitive,
  type PreviewLinkCardProps as PreviewLinkCardPrimitiveProps,
  type PreviewLinkCardTriggerProps as PreviewLinkCardTriggerPrimitiveProps,
  type PreviewLinkCardContentProps as PreviewLinkCardContentPrimitiveProps,
  type PreviewLinkCardImageProps as PreviewLinkCardImagePrimitiveProps,
} from '@/components/animate-ui/primitives/radix/preview-link-card';
import { cn } from '@/lib/utils';

type PreviewLinkCardProps = PreviewLinkCardPrimitiveProps;

function PreviewLinkCard(props: PreviewLinkCardProps) {
  return <PreviewLinkCardPrimitive {...props} />;
}

type PreviewLinkCardTriggerProps = PreviewLinkCardTriggerPrimitiveProps;

function PreviewLinkCardTrigger(props: PreviewLinkCardTriggerProps) {
  return <PreviewLinkCardTriggerPrimitive {...props} />;
}

type PreviewLinkCardContentProps = PreviewLinkCardContentPrimitiveProps;

function PreviewLinkCardContent({
  className,
  align = 'center',
  sideOffset = 4,
  ...props
}: PreviewLinkCardContentProps) {
  return (
    <PreviewLinkCardPortalPrimitive>
      <PreviewLinkCardContentPrimitive
        align={align}
        sideOffset={sideOffset}
        className={cn(
          'z-50 origin-(--radix-hover-card-content-transform-origin) rounded-md border shadow-md outline-hidden overflow-hidden',
          className,
        )}
        {...props}
      />
    </PreviewLinkCardPortalPrimitive>
  );
}

type PreviewLinkCardImageProps = PreviewLinkCardImagePrimitiveProps;

function PreviewLinkCardImage(props: PreviewLinkCardImageProps) {
  return <PreviewLinkCardImagePrimitive {...props} />;
}

export {
  PreviewLinkCard,
  PreviewLinkCardTrigger,
  PreviewLinkCardContent,
  PreviewLinkCardImage,
  type PreviewLinkCardProps,
  type PreviewLinkCardTriggerProps,
  type PreviewLinkCardContentProps,
  type PreviewLinkCardImageProps,
};
```

---

### Progress <a name="progress"></a>

Displays an indicator showing the completion progress of a task, typically displayed as a progress bar.

- **Category**: Radix
- **Registry Key**: `components-radix-progress`

#### Installation

```bash
npx shadcn@latest add components-radix-progress
```

#### How to Use

```tsx
<Progress />
```

#### Demo Code

##### File: `registry/demo/components/radix/progress/index.tsx`

```tsx
import * as React from 'react';
import { Progress } from '@/components/animate-ui/components/radix/progress';

export const RadixProgressDemo = () => {
  const [progress, setProgress] = React.useState(0);

  React.useEffect(() => {
    const timer = setInterval(() => {
      setProgress((prev) => {
        if (prev >= 100) return 100;
        return prev + 25;
      });
    }, 2000);
    return () => clearInterval(timer);
  }, []);

  React.useEffect(() => {
    if (progress >= 100) setTimeout(() => setProgress(0), 4000);
  }, [progress]);

  return <Progress value={progress} className="w-[300px]" />;
};
```

#### Component Source Code

##### File: `registry/components/radix/progress/index.tsx`

```tsx
import * as React from 'react';

import {
  Progress as ProgressPrimitive,
  ProgressIndicator as ProgressIndicatorPrimitive,
  type ProgressProps as ProgressPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/progress';
import { cn } from '@/lib/utils';

type ProgressProps = ProgressPrimitiveProps;

function Progress({ className, ...props }: ProgressProps) {
  return (
    <ProgressPrimitive
      className={cn(
        'bg-primary/20 relative h-2 w-full overflow-hidden rounded-full',
        className,
      )}
      {...props}
    >
      <ProgressIndicatorPrimitive className="bg-primary rounded-full h-full w-full flex-1" />
    </ProgressPrimitive>
  );
}

export { Progress, type ProgressProps };
```

---

### Radio Group <a name="radio-group"></a>

A set of checkable buttons—known as radio buttons—where no more than one of the buttons can be checked at a time.

- **Category**: Radix
- **Registry Key**: `components-radix-radio-group`

#### Installation

```bash
npx shadcn@latest add components-radix-radio-group
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
<RadioGroup>
  <RadioGroupItem value="1" />
  <RadioGroupItem value="2" />
  <RadioGroupItem value="3" />
</RadioGroup>
```

#### Demo Code

##### File: `registry/demo/components/radix/radio-group/index.tsx`

```tsx
import * as React from 'react';

import {
  RadioGroup,
  RadioGroupItem,
} from '@/components/animate-ui/components/radix/radio-group';
import { Label } from '@/components/ui/label';

export const RadixRadioGroupDemo = () => {
  return (
    <RadioGroup defaultValue="default">
      <Label className="flex items-center gap-x-3">
        <RadioGroupItem value="default" />
        Default
      </Label>
      <Label className="flex items-center gap-x-3">
        <RadioGroupItem value="comfortable" />
        Comfortable
      </Label>
      <Label className="flex items-center gap-x-3">
        <RadioGroupItem value="compact" />
        Compact
      </Label>
    </RadioGroup>
  );
};
```

#### Component Source Code

##### File: `registry/components/radix/radio-group/index.tsx`

```tsx
import * as React from 'react';
import { CircleIcon } from 'lucide-react';

import {
  RadioGroup as RadioGroupPrimitive,
  RadioGroupItem as RadioGroupItemPrimitive,
  RadioGroupIndicator as RadioGroupIndicatorPrimitive,
  type RadioGroupProps as RadioGroupPrimitiveProps,
  type RadioGroupItemProps as RadioGroupItemPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/radio-group';
import { cn } from '@/lib/utils';

type RadioGroupProps = RadioGroupPrimitiveProps;

function RadioGroup({ className, ...props }: RadioGroupProps) {
  return (
    <RadioGroupPrimitive className={cn('grid gap-3', className)} {...props} />
  );
}

type RadioGroupItemProps = RadioGroupItemPrimitiveProps;

function RadioGroupItem({ className, ...props }: RadioGroupItemProps) {
  return (
    <RadioGroupItemPrimitive
      className={cn(
        'border-input text-primary focus-visible:border-ring focus-visible:ring-ring/50 aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive dark:bg-input/30 aspect-square size-4 shrink-0 rounded-full border shadow-xs transition-[color,box-shadow] outline-none focus-visible:ring-[3px] disabled:cursor-not-allowed disabled:opacity-50',
        className,
      )}
      {...props}
    >
      <RadioGroupIndicatorPrimitive className="relative flex items-center justify-center">
        <CircleIcon className="fill-primary absolute top-1/2 left-1/2 size-2 -translate-x-1/2 -translate-y-1/2" />
      </RadioGroupIndicatorPrimitive>
    </RadioGroupItemPrimitive>
  );
}

export {
  RadioGroup,
  RadioGroupItem,
  type RadioGroupProps,
  type RadioGroupItemProps,
};
```

---

### Sheet <a name="sheet"></a>

Extends the Dialog component to display content that complements the main content of the screen.

- **Category**: Radix
- **Registry Key**: `components-radix-sheet`

#### Installation

```bash
npx shadcn@latest add components-radix-sheet
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
<Sheet>
  <SheetTrigger>Open Sheet</SheetTrigger>
  <SheetContent>
    <SheetHeader>
      <SheetTitle>Sheet Title</SheetTitle>
      <SheetDescription>Sheet Description</SheetDescription>
    </SheetHeader>
    <p>Sheet Content</p>
    <SheetFooter>
      <button>Accept</button>
    </SheetFooter>
  </SheetContent>
</Sheet>
```

#### Demo Code

##### File: `registry/demo/components/radix/sheet/index.tsx`

```tsx
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import {
  Sheet,
  SheetClose,
  SheetContent,
  SheetDescription,
  SheetFooter,
  SheetHeader,
  SheetTitle,
  SheetTrigger,
} from '@/components/animate-ui/components/radix/sheet';

export function RadixSheetDemo() {
  return (
    <Sheet>
      <SheetTrigger asChild>
        <Button variant="outline">Open</Button>
      </SheetTrigger>
      <SheetContent>
        <SheetHeader>
          <SheetTitle>Edit profile</SheetTitle>
          <SheetDescription>
            Make changes to your profile here. Click save when you&apos;re done.
          </SheetDescription>
        </SheetHeader>
        <div className="grid flex-1 auto-rows-min gap-6 px-4">
          <div className="grid gap-3">
            <Label htmlFor="sheet-demo-name">Name</Label>
            <Input id="sheet-demo-name" defaultValue="Pedro Duarte" />
          </div>
          <div className="grid gap-3">
            <Label htmlFor="sheet-demo-username">Username</Label>
            <Input id="sheet-demo-username" defaultValue="@peduarte" />
          </div>
        </div>
        <SheetFooter>
          <Button type="submit">Save changes</Button>
          <SheetClose asChild>
            <Button variant="outline">Close</Button>
          </SheetClose>
        </SheetFooter>
      </SheetContent>
    </Sheet>
  );
}
```

#### Component Source Code

##### File: `registry/components/radix/sheet/index.tsx`

```tsx
import * as React from 'react';

import {
  Sheet as SheetPrimitive,
  SheetTrigger as SheetTriggerPrimitive,
  SheetOverlay as SheetOverlayPrimitive,
  SheetClose as SheetClosePrimitive,
  SheetPortal as SheetPortalPrimitive,
  SheetContent as SheetContentPrimitive,
  SheetHeader as SheetHeaderPrimitive,
  SheetFooter as SheetFooterPrimitive,
  SheetTitle as SheetTitlePrimitive,
  SheetDescription as SheetDescriptionPrimitive,
  type SheetProps as SheetPrimitiveProps,
  type SheetTriggerProps as SheetTriggerPrimitiveProps,
  type SheetOverlayProps as SheetOverlayPrimitiveProps,
  type SheetCloseProps as SheetClosePrimitiveProps,
  type SheetContentProps as SheetContentPrimitiveProps,
  type SheetHeaderProps as SheetHeaderPrimitiveProps,
  type SheetFooterProps as SheetFooterPrimitiveProps,
  type SheetTitleProps as SheetTitlePrimitiveProps,
  type SheetDescriptionProps as SheetDescriptionPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/sheet';
import { cn } from '@/lib/utils';
import { XIcon } from 'lucide-react';

type SheetProps = SheetPrimitiveProps;

function Sheet(props: SheetProps) {
  return <SheetPrimitive {...props} />;
}

type SheetTriggerProps = SheetTriggerPrimitiveProps;

function SheetTrigger(props: SheetTriggerProps) {
  return <SheetTriggerPrimitive {...props} />;
}

type SheetOverlayProps = SheetOverlayPrimitiveProps;

function SheetOverlay({ className, ...props }: SheetOverlayProps) {
  return (
    <SheetOverlayPrimitive
      className={cn('fixed inset-0 z-50 bg-black/50', className)}
      {...props}
    />
  );
}

type SheetCloseProps = SheetClosePrimitiveProps;

function SheetClose(props: SheetCloseProps) {
  return <SheetClosePrimitive {...props} />;
}

type SheetContentProps = SheetContentPrimitiveProps & {
  showCloseButton?: boolean;
};

function SheetContent({
  className,
  children,
  side = 'right',
  showCloseButton = true,
  ...props
}: SheetContentProps) {
  return (
    <SheetPortalPrimitive>
      <SheetOverlay />
      <SheetContentPrimitive
        className={cn(
          'bg-background fixed z-50 flex flex-col gap-4 shadow-lg',
          side === 'right' && 'h-full w-[350px] border-l',
          side === 'left' && 'h-full w-[350px] border-r',
          side === 'top' && 'w-full h-[350px] border-b',
          side === 'bottom' && 'w-full h-[350px] border-t',
          className,
        )}
        side={side}
        {...props}
      >
        {children}
        {showCloseButton && (
          <SheetClose className="ring-offset-background focus:ring-ring data-[state=open]:bg-secondary absolute top-4 right-4 rounded-xs opacity-70 transition-opacity hover:opacity-100 focus:ring-2 focus:ring-offset-2 focus:outline-hidden disabled:pointer-events-none">
            <XIcon className="size-4" />
            <span className="sr-only">Close</span>
          </SheetClose>
        )}
      </SheetContentPrimitive>
    </SheetPortalPrimitive>
  );
}

type SheetHeaderProps = SheetHeaderPrimitiveProps;

function SheetHeader({ className, ...props }: SheetHeaderProps) {
  return (
    <SheetHeaderPrimitive
      className={cn('flex flex-col gap-1.5 p-4', className)}
      {...props}
    />
  );
}

type SheetFooterProps = SheetFooterPrimitiveProps;

function SheetFooter({ className, ...props }: SheetFooterProps) {
  return (
    <SheetFooterPrimitive
      className={cn('mt-auto flex flex-col gap-2 p-4', className)}
      {...props}
    />
  );
}

type SheetTitleProps = SheetTitlePrimitiveProps;

function SheetTitle({ className, ...props }: SheetTitleProps) {
  return (
    <SheetTitlePrimitive
      className={cn('text-foreground font-semibold', className)}
      {...props}
    />
  );
}

type SheetDescriptionProps = SheetDescriptionPrimitiveProps;

function SheetDescription({ className, ...props }: SheetDescriptionProps) {
  return (
    <SheetDescriptionPrimitive
      className={cn('text-muted-foreground text-sm', className)}
      {...props}
    />
  );
}

export {
  Sheet,
  SheetTrigger,
  SheetClose,
  SheetContent,
  SheetHeader,
  SheetFooter,
  SheetTitle,
  SheetDescription,
  type SheetProps,
  type SheetTriggerProps,
  type SheetCloseProps,
  type SheetContentProps,
  type SheetHeaderProps,
  type SheetFooterProps,
  type SheetTitleProps,
  type SheetDescriptionProps,
};
```

---

### Sidebar <a name="sidebar"></a>

A composable, themeable and customizable sidebar component. Created by Shadcn and animated by Animate UI.

- **Category**: Radix
- **Registry Key**: `components-radix-sidebar`

#### Installation

```bash
npx shadcn@latest add components-radix-sidebar
```

#### Dependencies

- **NPM Packages**:
  - `radix-ui`
  - `class-variance-authority`
  - `motion`
  - `lucide-react`

#### How to Use

```tsx
<SidebarProvider>
  <Sidebar>
    <SidebarHeader>
      <SidebarMenu>
        <SidebarMenuItem>Item 1</SidebarMenuItem>
        <SidebarMenuItem>Item 2</SidebarMenuItem>
        <SidebarMenuItem>Item 3</SidebarMenuItem>
      </SidebarMenu>
    </SidebarHeader>
    <SidebarContent>
      <SidebarGroup>
        <SidebarGroupLabel>Label 1</SidebarGroupLabel>
        <SidebarMenu>
          <SidebarMenuItem>Item 1</SidebarMenuItem>
          <SidebarMenuItem>Item 2</SidebarMenuItem>
          <SidebarMenuItem>Item 3</SidebarMenuItem>
        </SidebarMenu>
      </SidebarGroup>
      <SidebarGroup>
        <SidebarGroupLabel>Label 2</SidebarGroupLabel>
        <SidebarMenu>
          <SidebarMenuItem>Item 1</SidebarMenuItem>
          <SidebarMenuItem>Item 2</SidebarMenuItem>
          <SidebarMenuItem>Item 3</SidebarMenuItem>
        </SidebarMenu>
      </SidebarGroup>
    </SidebarContent>
    <SidebarFooter>
      <SidebarMenu>
        <SidebarMenuItem>Item 1</SidebarMenuItem>
        <SidebarMenuItem>Item 2</SidebarMenuItem>
        <SidebarMenuItem>Item 3</SidebarMenuItem>
      </SidebarMenu>
    </SidebarFooter>
    <SidebarRail />
  </Sidebar>
  <SidebarInset>
    <SidebarTrigger />
    {...}
  </SidebarInset>
</SidebarProvider>
```

#### Demo Code

##### File: `registry/demo/components/radix/sidebar/index.tsx`

```tsx
'use client';

import * as React from 'react';

import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from '@/components/ui/breadcrumb';
import { Separator } from '@/components/ui/separator';
import {
  SidebarProvider,
  SidebarInset,
  SidebarTrigger,
  Sidebar,
  SidebarHeader,
  SidebarContent,
  SidebarFooter,
  SidebarRail,
  SidebarGroup,
  SidebarGroupLabel,
  SidebarMenu,
  SidebarMenuItem,
  SidebarMenuButton,
  SidebarMenuSub,
  SidebarMenuSubItem,
  SidebarMenuSubButton,
  SidebarMenuAction,
} from '@/components/animate-ui/components/radix/sidebar';
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from '@/components/animate-ui/primitives/radix/collapsible';
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuShortcut,
  DropdownMenuTrigger,
} from '@/components/animate-ui/components/radix/dropdown-menu';
import {
  AudioWaveform,
  BadgeCheck,
  Bell,
  BookOpen,
  Bot,
  ChevronRight,
  ChevronsUpDown,
  Command,
  CreditCard,
  Folder,
  Forward,
  Frame,
  GalleryVerticalEnd,
  LogOut,
  Map,
  MoreHorizontal,
  PieChart,
  Plus,
  Settings2,
  Sparkles,
  SquareTerminal,
  Trash2,
} from 'lucide-react';
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from '@/components/ui/avatar';
import { useIsMobile } from '@/hooks/use-mobile';

const DATA = {
  user: {
    name: 'Skyleen',
    email: 'skyleen@example.com',
    avatar:
      'https://pbs.twimg.com/profile_images/1909615404789506048/MTqvRsjo_400x400.jpg',
  },
  teams: [
    {
      name: 'Acme Inc',
      logo: GalleryVerticalEnd,
      plan: 'Enterprise',
    },
    {
      name: 'Acme Corp.',
      logo: AudioWaveform,
      plan: 'Startup',
    },
    {
      name: 'Evil Corp.',
      logo: Command,
      plan: 'Free',
    },
  ],
  navMain: [
    {
      title: 'Playground',
      url: '#',
      icon: SquareTerminal,
      isActive: true,
      items: [
        {
          title: 'History',
          url: '#',
        },
        {
          title: 'Starred',
          url: '#',
        },
        {
          title: 'Settings',
          url: '#',
        },
      ],
    },
    {
      title: 'Models',
      url: '#',
      icon: Bot,
      items: [
        {
          title: 'Genesis',
          url: '#',
        },
        {
          title: 'Explorer',
          url: '#',
        },
        {
          title: 'Quantum',
          url: '#',
        },
      ],
    },
    {
      title: 'Documentation',
      url: '#',
      icon: BookOpen,
      items: [
        {
          title: 'Introduction',
          url: '#',
        },
        {
          title: 'Get Started',
          url: '#',
        },
        {
          title: 'Tutorials',
          url: '#',
        },
        {
          title: 'Changelog',
          url: '#',
        },
      ],
    },
    {
      title: 'Settings',
      url: '#',
      icon: Settings2,
      items: [
        {
          title: 'General',
          url: '#',
        },
        {
          title: 'Team',
          url: '#',
        },
        {
          title: 'Billing',
          url: '#',
        },
        {
          title: 'Limits',
          url: '#',
        },
      ],
    },
  ],
  projects: [
    {
      name: 'Design Engineering',
      url: '#',
      icon: Frame,
    },
    {
      name: 'Sales & Marketing',
      url: '#',
      icon: PieChart,
    },
    {
      name: 'Travel',
      url: '#',
      icon: Map,
    },
  ],
};

export const RadixSidebarDemo = () => {
  const isMobile = useIsMobile();
  const [activeTeam, setActiveTeam] = React.useState(DATA.teams[0]);

  if (!activeTeam) return null;

  return (
    <SidebarProvider>
      <Sidebar collapsible="icon">
        <SidebarHeader>
          {/* Team Switcher */}
          <SidebarMenu>
            <SidebarMenuItem>
              <DropdownMenu>
                <DropdownMenuTrigger asChild>
                  <SidebarMenuButton
                    size="lg"
                    className="data-[state=open]:bg-sidebar-accent data-[state=open]:text-sidebar-accent-foreground"
                  >
                    <div className="flex aspect-square size-8 items-center justify-center rounded-lg bg-sidebar-primary text-sidebar-primary-foreground">
                      <activeTeam.logo className="size-4" />
                    </div>
                    <div className="grid flex-1 text-left text-sm leading-tight">
                      <span className="truncate font-semibold">
                        {activeTeam.name}
                      </span>
                      <span className="truncate text-xs">
                        {activeTeam.plan}
                      </span>
                    </div>
                    <ChevronsUpDown className="ml-auto" />
                  </SidebarMenuButton>
                </DropdownMenuTrigger>
                <DropdownMenuContent
                  className="w-[--radix-dropdown-menu-trigger-width] min-w-56 rounded-lg"
                  align="start"
                  side={isMobile ? 'bottom' : 'right'}
                  sideOffset={4}
                >
                  <DropdownMenuLabel className="text-xs text-muted-foreground">
                    Teams
                  </DropdownMenuLabel>
                  {DATA.teams.map((team, index) => (
                    <DropdownMenuItem
                      key={team.name}
                      onClick={() => setActiveTeam(team)}
                      className="gap-2 p-2"
                    >
                      <div className="flex size-6 items-center justify-center rounded-sm border">
                        <team.logo className="size-4 shrink-0" />
                      </div>
                      {team.name}
                      <DropdownMenuShortcut>⌘{index + 1}</DropdownMenuShortcut>
                    </DropdownMenuItem>
                  ))}
                  <DropdownMenuSeparator />
                  <DropdownMenuItem className="gap-2 p-2">
                    <div className="flex size-6 items-center justify-center rounded-md border bg-background">
                      <Plus className="size-4" />
                    </div>
                    <div className="font-medium text-muted-foreground">
                      Add team
                    </div>
                  </DropdownMenuItem>
                </DropdownMenuContent>
              </DropdownMenu>
            </SidebarMenuItem>
          </SidebarMenu>
          {/* Team Switcher */}
        </SidebarHeader>

        <SidebarContent>
          {/* Nav Main */}
          <SidebarGroup>
            <SidebarGroupLabel>Platform</SidebarGroupLabel>
            <SidebarMenu>
              {DATA.navMain.map((item) => (
                <Collapsible
                  key={item.title}
                  asChild
                  defaultOpen={item.isActive}
                  className="group/collapsible"
                >
                  <SidebarMenuItem>
                    <CollapsibleTrigger asChild>
                      <SidebarMenuButton tooltip={item.title}>
                        {item.icon && <item.icon />}
                        <span>{item.title}</span>
                        <ChevronRight className="ml-auto transition-transform duration-300 group-data-[state=open]/collapsible:rotate-90" />
                      </SidebarMenuButton>
                    </CollapsibleTrigger>
                    <CollapsibleContent>
                      <SidebarMenuSub>
                        {item.items?.map((subItem) => (
                          <SidebarMenuSubItem key={subItem.title}>
                            <SidebarMenuSubButton asChild>
                              <a href={subItem.url}>
                                <span>{subItem.title}</span>
                              </a>
                            </SidebarMenuSubButton>
                          </SidebarMenuSubItem>
                        ))}
                      </SidebarMenuSub>
                    </CollapsibleContent>
                  </SidebarMenuItem>
                </Collapsible>
              ))}
            </SidebarMenu>
          </SidebarGroup>
          {/* Nav Main */}

          {/* Nav Project */}
          <SidebarGroup className="group-data-[collapsible=icon]:hidden">
            <SidebarGroupLabel>Projects</SidebarGroupLabel>
            <SidebarMenu>
              {DATA.projects.map((item) => (
                <SidebarMenuItem key={item.name}>
                  <SidebarMenuButton asChild>
                    <a href={item.url}>
                      <item.icon />
                      <span>{item.name}</span>
                    </a>
                  </SidebarMenuButton>
                  <DropdownMenu>
                    <DropdownMenuTrigger asChild>
                      <SidebarMenuAction showOnHover>
                        <MoreHorizontal />
                        <span className="sr-only">More</span>
                      </SidebarMenuAction>
                    </DropdownMenuTrigger>
                    <DropdownMenuContent
                      className="w-48 rounded-lg"
                      side={isMobile ? 'bottom' : 'right'}
                      align={isMobile ? 'end' : 'start'}
                    >
                      <DropdownMenuItem>
                        <Folder className="text-muted-foreground" />
                        <span>View Project</span>
                      </DropdownMenuItem>
                      <DropdownMenuItem>
                        <Forward className="text-muted-foreground" />
                        <span>Share Project</span>
                      </DropdownMenuItem>
                      <DropdownMenuSeparator />
                      <DropdownMenuItem>
                        <Trash2 className="text-muted-foreground" />
                        <span>Delete Project</span>
                      </DropdownMenuItem>
                    </DropdownMenuContent>
                  </DropdownMenu>
                </SidebarMenuItem>
              ))}
              <SidebarMenuItem>
                <SidebarMenuButton className="text-sidebar-foreground/70">
                  <MoreHorizontal className="text-sidebar-foreground/70" />
                  <span>More</span>
                </SidebarMenuButton>
              </SidebarMenuItem>
            </SidebarMenu>
          </SidebarGroup>
          {/* Nav Project */}
        </SidebarContent>
        <SidebarFooter>
          {/* Nav User */}
          <SidebarMenu>
            <SidebarMenuItem>
              <DropdownMenu>
                <DropdownMenuTrigger asChild>
                  <SidebarMenuButton
                    size="lg"
                    className="data-[state=open]:bg-sidebar-accent data-[state=open]:text-sidebar-accent-foreground"
                  >
                    <Avatar className="h-8 w-8 rounded-lg">
                      <AvatarImage
                        src={DATA.user.avatar}
                        alt={DATA.user.name}
                      />
                      <AvatarFallback className="rounded-lg">CN</AvatarFallback>
                    </Avatar>
                    <div className="grid flex-1 text-left text-sm leading-tight">
                      <span className="truncate font-semibold">
                        {DATA.user.name}
                      </span>
                      <span className="truncate text-xs">
                        {DATA.user.email}
                      </span>
                    </div>
                    <ChevronsUpDown className="ml-auto size-4" />
                  </SidebarMenuButton>
                </DropdownMenuTrigger>
                <DropdownMenuContent
                  className="w-[--radix-dropdown-menu-trigger-width] min-w-56 rounded-lg"
                  side={isMobile ? 'bottom' : 'right'}
                  align="end"
                  sideOffset={4}
                >
                  <DropdownMenuLabel className="p-0 font-normal">
                    <div className="flex items-center gap-2 px-1 py-1.5 text-left text-sm">
                      <Avatar className="h-8 w-8 rounded-lg">
                        <AvatarImage
                          src={DATA.user.avatar}
                          alt={DATA.user.name}
                        />
                        <AvatarFallback className="rounded-lg">
                          CN
                        </AvatarFallback>
                      </Avatar>
                      <div className="grid flex-1 text-left text-sm leading-tight">
                        <span className="truncate font-semibold">
                          {DATA.user.name}
                        </span>
                        <span className="truncate text-xs">
                          {DATA.user.email}
                        </span>
                      </div>
                    </div>
                  </DropdownMenuLabel>
                  <DropdownMenuSeparator />
                  <DropdownMenuGroup>
                    <DropdownMenuItem>
                      <Sparkles />
                      Upgrade to Pro
                    </DropdownMenuItem>
                  </DropdownMenuGroup>
                  <DropdownMenuSeparator />
                  <DropdownMenuGroup>
                    <DropdownMenuItem>
                      <BadgeCheck />
                      Account
                    </DropdownMenuItem>
                    <DropdownMenuItem>
                      <CreditCard />
                      Billing
                    </DropdownMenuItem>
                    <DropdownMenuItem>
                      <Bell />
                      Notifications
                    </DropdownMenuItem>
                  </DropdownMenuGroup>
                  <DropdownMenuSeparator />
                  <DropdownMenuItem>
                    <LogOut />
                    Log out
                  </DropdownMenuItem>
                </DropdownMenuContent>
              </DropdownMenu>
            </SidebarMenuItem>
          </SidebarMenu>
          {/* Nav User */}
        </SidebarFooter>
        <SidebarRail />
      </Sidebar>

      <SidebarInset>
        <header className="flex h-16 shrink-0 items-center gap-2 transition-[width,height] ease-linear group-has-[[data-collapsible=icon]]/sidebar-wrapper:h-12">
          <div className="flex items-center gap-2 px-4">
            <SidebarTrigger className="-ml-1" />
            <Separator orientation="vertical" className="mr-2 h-4" />
            <Breadcrumb>
              <BreadcrumbList>
                <BreadcrumbItem className="hidden md:block">
                  <BreadcrumbLink href="#">
                    Building Your Application
                  </BreadcrumbLink>
                </BreadcrumbItem>
                <BreadcrumbSeparator className="hidden md:block" />
                <BreadcrumbItem>
                  <BreadcrumbPage>Data Fetching</BreadcrumbPage>
                </BreadcrumbItem>
              </BreadcrumbList>
            </Breadcrumb>
          </div>
        </header>
        <div className="flex flex-1 flex-col gap-4 p-4 pt-0">
          <div className="grid auto-rows-min gap-4 md:grid-cols-3">
            <div className="aspect-video rounded-xl bg-muted/50" />
            <div className="aspect-video rounded-xl bg-muted/50" />
            <div className="aspect-video rounded-xl bg-muted/50" />
          </div>
          <div className="min-h-[100vh] flex-1 rounded-xl bg-muted/50 md:min-h-min" />
        </div>
      </SidebarInset>
    </SidebarProvider>
  );
};
```

#### Component Source Code

##### File: `registry/components/radix/sidebar/index.tsx`

```tsx
'use client';

import * as React from 'react';
import { Slot } from 'radix-ui';
import { cva, VariantProps } from 'class-variance-authority';
import { PanelLeftIcon } from 'lucide-react';
import { type Transition } from 'motion/react';

import { useIsMobile } from '@/hooks/use-mobile';
import { cn } from '@/lib/utils';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Separator } from '@/components/ui/separator';
import { Skeleton } from '@/components/ui/skeleton';
import {
  Sheet,
  SheetContent,
  SheetDescription,
  SheetHeader,
  SheetTitle,
} from '@/components/animate-ui/components/radix/sheet';
import {
  TooltipProvider,
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/animate-ui/components/animate/tooltip';
import {
  Highlight,
  HighlightItem,
} from '@/components/animate-ui/primitives/effects/highlight';
import { getStrictContext } from '@/lib/get-strict-context';

const SIDEBAR_COOKIE_NAME = 'sidebar_state';
const SIDEBAR_COOKIE_MAX_AGE = 60 * 60 * 24 * 7;
const SIDEBAR_WIDTH = '16rem';
const SIDEBAR_WIDTH_MOBILE = '18rem';
const SIDEBAR_WIDTH_ICON = '3rem';
const SIDEBAR_KEYBOARD_SHORTCUT = 'b';

type SidebarContextProps = {
  state: 'expanded' | 'collapsed';
  open: boolean;
  setOpen: (open: boolean) => void;
  openMobile: boolean;
  setOpenMobile: (open: boolean) => void;
  isMobile: boolean;
  toggleSidebar: () => void;
};

const [LocalSidebarProvider, useSidebar] =
  getStrictContext<SidebarContextProps>('SidebarContext');

type SidebarProviderProps = React.ComponentProps<'div'> & {
  defaultOpen?: boolean;
  open?: boolean;
  onOpenChange?: (open: boolean) => void;
};

function SidebarProvider({
  defaultOpen = true,
  open: openProp,
  onOpenChange: setOpenProp,
  className,
  style,
  children,
  ...props
}: SidebarProviderProps) {
  const isMobile = useIsMobile();
  const [openMobile, setOpenMobile] = React.useState(false);

  // This is the internal state of the sidebar.
  // We use openProp and setOpenProp for control from outside the component.
  const [_open, _setOpen] = React.useState(defaultOpen);
  const open = openProp ?? _open;
  const setOpen = React.useCallback(
    (value: boolean | ((value: boolean) => boolean)) => {
      const openState = typeof value === 'function' ? value(open) : value;
      if (setOpenProp) {
        setOpenProp(openState);
      } else {
        _setOpen(openState);
      }

      // This sets the cookie to keep the sidebar state.
      document.cookie = `${SIDEBAR_COOKIE_NAME}=${openState}; path=/; max-age=${SIDEBAR_COOKIE_MAX_AGE}`;
    },
    [setOpenProp, open],
  );

  // Helper to toggle the sidebar.
  const toggleSidebar = React.useCallback(() => {
    return isMobile ? setOpenMobile((open) => !open) : setOpen((open) => !open);
  }, [isMobile, setOpen, setOpenMobile]);

  // Adds a keyboard shortcut to toggle the sidebar.
  React.useEffect(() => {
    const handleKeyDown = (event: KeyboardEvent) => {
      if (
        event.key === SIDEBAR_KEYBOARD_SHORTCUT &&
        (event.metaKey || event.ctrlKey)
      ) {
        event.preventDefault();
        toggleSidebar();
      }
    };

    window.addEventListener('keydown', handleKeyDown);
    return () => window.removeEventListener('keydown', handleKeyDown);
  }, [toggleSidebar]);

  // We add a state so that we can do data-state="expanded" or "collapsed".
  // This makes it easier to style the sidebar with Tailwind classes.
  const state = open ? 'expanded' : 'collapsed';

  const contextValue = React.useMemo<SidebarContextProps>(
    () => ({
      state,
      open,
      setOpen,
      isMobile,
      openMobile,
      setOpenMobile,
      toggleSidebar,
    }),
    [state, open, setOpen, isMobile, openMobile, setOpenMobile, toggleSidebar],
  );

  return (
    <LocalSidebarProvider value={contextValue}>
      <TooltipProvider openDelay={0}>
        <div
          data-slot="sidebar-wrapper"
          style={
            {
              '--sidebar-width': SIDEBAR_WIDTH,
              '--sidebar-width-icon': SIDEBAR_WIDTH_ICON,
              ...style,
            } as React.CSSProperties
          }
          className={cn(
            'group/sidebar-wrapper has-data-[variant=inset]:bg-sidebar flex min-h-svh w-full',
            className,
          )}
          {...props}
        >
          {children}
        </div>
      </TooltipProvider>
    </LocalSidebarProvider>
  );
}

type SidebarProps = React.ComponentProps<'div'> & {
  side?: 'left' | 'right';
  variant?: 'sidebar' | 'floating' | 'inset';
  collapsible?: 'offcanvas' | 'icon' | 'none';
  containerClassName?: string;
  animateOnHover?: boolean;
  transition?: Transition;
};

function Sidebar({
  side = 'left',
  variant = 'sidebar',
  collapsible = 'offcanvas',
  className,
  children,
  animateOnHover = true,
  containerClassName,
  transition = { type: 'spring', stiffness: 350, damping: 35 },
  ...props
}: SidebarProps) {
  const { isMobile, state, openMobile, setOpenMobile } = useSidebar();

  if (collapsible === 'none') {
    return (
      <Highlight
        enabled={animateOnHover}
        hover
        controlledItems
        mode="parent"
        containerClassName={containerClassName}
        transition={transition}
      >
        <div
          data-slot="sidebar"
          className={cn(
            'bg-sidebar text-sidebar-foreground flex h-full w-(--sidebar-width) flex-col',
            className,
          )}
          {...props}
        >
          {children}
        </div>
      </Highlight>
    );
  }

  if (isMobile) {
    return (
      <Sheet open={openMobile} onOpenChange={setOpenMobile} {...props}>
        <SheetContent
          data-sidebar="sidebar"
          data-slot="sidebar"
          data-mobile="true"
          className="bg-sidebar text-sidebar-foreground w-(--sidebar-width) p-0 [&>button]:hidden"
          style={
            {
              '--sidebar-width': SIDEBAR_WIDTH_MOBILE,
            } as React.CSSProperties
          }
          side={side}
        >
          <SheetHeader className="sr-only">
            <SheetTitle>Sidebar</SheetTitle>
            <SheetDescription>Displays the mobile sidebar.</SheetDescription>
          </SheetHeader>
          <Highlight
            enabled={animateOnHover}
            hover
            controlledItems
            mode="parent"
            containerClassName={cn('h-full', containerClassName)}
            transition={transition}
          >
            <div className="flex h-full w-full flex-col">{children}</div>
          </Highlight>
        </SheetContent>
      </Sheet>
    );
  }

  return (
    <div
      className="group peer text-sidebar-foreground hidden md:block"
      data-state={state}
      data-collapsible={state === 'collapsed' ? collapsible : ''}
      data-variant={variant}
      data-side={side}
      data-slot="sidebar"
    >
      {/* This is what handles the sidebar gap on desktop */}
      <div
        data-slot="sidebar-gap"
        className={cn(
          'relative w-(--sidebar-width) bg-transparent transition-[width] duration-400 ease-[cubic-bezier(0.7,-0.15,0.25,1.15)]',
          'group-data-[collapsible=offcanvas]:w-0',
          'group-data-[side=right]:rotate-180',
          variant === 'floating' || variant === 'inset'
            ? 'group-data-[collapsible=icon]:w-[calc(var(--sidebar-width-icon)+(--spacing(4)))]'
            : 'group-data-[collapsible=icon]:w-(--sidebar-width-icon)',
        )}
      />
      <div
        data-slot="sidebar-container"
        className={cn(
          'fixed inset-y-0 z-10 hidden h-svh w-(--sidebar-width) transition-[left,right,width] duration-400 ease-[cubic-bezier(0.75,0,0.25,1)] md:flex',
          side === 'left'
            ? 'left-0 group-data-[collapsible=offcanvas]:left-[calc(var(--sidebar-width)*-1)]'
            : 'right-0 group-data-[collapsible=offcanvas]:right-[calc(var(--sidebar-width)*-1)]',
          // Adjust the padding for floating and inset variants.
          variant === 'floating' || variant === 'inset'
            ? 'p-2 group-data-[collapsible=icon]:w-[calc(var(--sidebar-width-icon)+(--spacing(4))+2px)]'
            : 'group-data-[collapsible=icon]:w-(--sidebar-width-icon) group-data-[side=left]:border-r group-data-[side=right]:border-l',
          className,
        )}
        {...props}
      >
        <Highlight
          containerClassName={cn('size-full', containerClassName)}
          enabled={animateOnHover}
          hover
          controlledItems
          mode="parent"
          forceUpdateBounds
          transition={transition}
        >
          <div
            data-sidebar="sidebar"
            data-slot="sidebar-inner"
            className="bg-sidebar group-data-[variant=floating]:border-sidebar-border flex h-full w-full flex-col group-data-[variant=floating]:rounded-lg group-data-[variant=floating]:border group-data-[variant=floating]:shadow-sm"
          >
            {children}
          </div>
        </Highlight>
      </div>
    </div>
  );
}

type SidebarTriggerProps = React.ComponentProps<typeof Button>;

function SidebarTrigger({ className, onClick, ...props }: SidebarTriggerProps) {
  const { toggleSidebar } = useSidebar();

  return (
    <Button
      data-sidebar="trigger"
      data-slot="sidebar-trigger"
      variant="ghost"
      size="icon"
      className={cn('size-7', className)}
      onClick={(event) => {
        onClick?.(event);
        toggleSidebar();
      }}
      {...props}
    >
      <PanelLeftIcon />
      <span className="sr-only">Toggle Sidebar</span>
    </Button>
  );
}

type SidebarRailProps = React.ComponentProps<'button'>;

function SidebarRail({ className, ...props }: SidebarRailProps) {
  const { toggleSidebar } = useSidebar();

  return (
    <button
      data-sidebar="rail"
      data-slot="sidebar-rail"
      aria-label="Toggle Sidebar"
      tabIndex={-1}
      onClick={toggleSidebar}
      title="Toggle Sidebar"
      className={cn(
        'hover:after:bg-sidebar-border absolute inset-y-0 z-20 hidden w-4 -translate-x-1/2 transition-all ease-linear group-data-[side=left]:-right-4 group-data-[side=right]:left-0 after:absolute after:inset-y-0 after:left-1/2 after:w-[2px] sm:flex',
        'in-data-[side=left]:cursor-w-resize in-data-[side=right]:cursor-e-resize',
        '[[data-side=left][data-state=collapsed]_&]:cursor-e-resize [[data-side=right][data-state=collapsed]_&]:cursor-w-resize',
        'hover:group-data-[collapsible=offcanvas]:bg-sidebar group-data-[collapsible=offcanvas]:translate-x-0 group-data-[collapsible=offcanvas]:after:left-full',
        '[[data-side=left][data-collapsible=offcanvas]_&]:-right-2',
        '[[data-side=right][data-collapsible=offcanvas]_&]:-left-2',
        className,
      )}
      {...props}
    />
  );
}

type SidebarInsetProps = React.ComponentProps<'main'>;

function SidebarInset({ className, ...props }: SidebarInsetProps) {
  return (
    <main
      data-slot="sidebar-inset"
      className={cn(
        'bg-background relative flex w-full flex-1 flex-col',
        'md:peer-data-[variant=inset]:m-2 md:peer-data-[variant=inset]:ml-0 md:peer-data-[variant=inset]:rounded-xl md:peer-data-[variant=inset]:shadow-sm md:peer-data-[variant=inset]:peer-data-[state=collapsed]:ml-2',
        className,
      )}
      {...props}
    />
  );
}

type SidebarInputProps = React.ComponentProps<typeof Input>;

function SidebarInput({ className, ...props }: SidebarInputProps) {
  return (
    <Input
      data-slot="sidebar-input"
      data-sidebar="input"
      className={cn('bg-background h-8 w-full shadow-none', className)}
      {...props}
    />
  );
}

type SidebarHeaderProps = React.ComponentProps<'div'>;

function SidebarHeader({ className, ...props }: SidebarHeaderProps) {
  return (
    <div
      data-slot="sidebar-header"
      data-sidebar="header"
      className={cn('flex flex-col gap-2 p-2', className)}
      {...props}
    />
  );
}

type SidebarFooterProps = React.ComponentProps<'div'>;

function SidebarFooter({ className, ...props }: SidebarFooterProps) {
  return (
    <div
      data-slot="sidebar-footer"
      data-sidebar="footer"
      className={cn('flex flex-col gap-2 p-2', className)}
      {...props}
    />
  );
}

type SidebarSeparatorProps = React.ComponentProps<typeof Separator>;

function SidebarSeparator({ className, ...props }: SidebarSeparatorProps) {
  return (
    <Separator
      data-slot="sidebar-separator"
      data-sidebar="separator"
      className={cn('bg-sidebar-border mx-2 w-auto', className)}
      {...props}
    />
  );
}

type SidebarContentProps = React.ComponentProps<'div'>;

function SidebarContent({ className, ...props }: SidebarContentProps) {
  return (
    <div
      data-slot="sidebar-content"
      data-sidebar="content"
      className={cn(
        'flex min-h-0 flex-1 flex-col gap-2 overflow-auto group-data-[collapsible=icon]:overflow-hidden',
        className,
      )}
      {...props}
    />
  );
}

type SidebarGroupProps = React.ComponentProps<'div'>;

function SidebarGroup({ className, ...props }: SidebarGroupProps) {
  return (
    <div
      data-slot="sidebar-group"
      data-sidebar="group"
      className={cn('relative flex w-full min-w-0 flex-col p-2', className)}
      {...props}
    />
  );
}

type SidebarGroupLabelProps = React.ComponentProps<'div'> & {
  asChild?: boolean;
};

function SidebarGroupLabel({
  className,
  asChild = false,
  ...props
}: SidebarGroupLabelProps) {
  const Comp = asChild ? Slot.Root : 'div';

  return (
    <Comp
      data-slot="sidebar-group-label"
      data-sidebar="group-label"
      className={cn(
        'text-sidebar-foreground/70 ring-sidebar-ring flex h-8 shrink-0 items-center rounded-md px-2 text-xs font-medium outline-hidden transition-[margin,opacity] duration-300 ease-linear focus-visible:ring-2 [&>svg]:size-4 [&>svg]:shrink-0',
        'group-data-[collapsible=icon]:-mt-8 group-data-[collapsible=icon]:opacity-0',
        className,
      )}
      {...props}
    />
  );
}

type SidebarGroupActionProps = React.ComponentProps<'button'> & {
  asChild?: boolean;
};

function SidebarGroupAction({
  className,
  asChild = false,
  ...props
}: SidebarGroupActionProps) {
  const Comp = asChild ? Slot.Root : 'button';

  return (
    <Comp
      data-slot="sidebar-group-action"
      data-sidebar="group-action"
      className={cn(
        'text-sidebar-foreground ring-sidebar-ring hover:bg-sidebar-accent hover:text-sidebar-accent-foreground absolute top-3.5 right-3 flex aspect-square w-5 items-center justify-center rounded-md p-0 outline-hidden transition-transform focus-visible:ring-2 [&>svg]:size-4 [&>svg]:shrink-0',
        // Increases the hit area of the button on mobile.
        'after:absolute after:-inset-2 md:after:hidden',
        'group-data-[collapsible=icon]:hidden',
        className,
      )}
      {...props}
    />
  );
}

type SidebarGroupContentProps = React.ComponentProps<'div'>;

function SidebarGroupContent({
  className,
  ...props
}: SidebarGroupContentProps) {
  return (
    <div
      data-slot="sidebar-group-content"
      data-sidebar="group-content"
      className={cn('w-full text-sm', className)}
      {...props}
    />
  );
}

type SidebarMenuProps = React.ComponentProps<'ul'>;

function SidebarMenu({ className, ...props }: SidebarMenuProps) {
  return (
    <ul
      data-slot="sidebar-menu"
      data-sidebar="menu"
      className={cn('flex w-full min-w-0 flex-col gap-1', className)}
      {...props}
    />
  );
}

type SidebarMenuItemProps = React.ComponentProps<'li'>;

function SidebarMenuItem({ className, ...props }: SidebarMenuItemProps) {
  return (
    <li
      data-slot="sidebar-menu-item"
      data-sidebar="menu-item"
      className={cn('group/menu-item relative', className)}
      {...props}
    />
  );
}

const sidebarMenuButtonActiveVariants = cva(
  'bg-sidebar-accent text-sidebar-accent-foreground rounded-md',
  {
    variants: {
      variant: {
        default: 'bg-sidebar-accent text-sidebar-accent-foreground',
        outline:
          'bg-sidebar-accent text-sidebar-accent-foreground shadow-[0_0_0_1px_hsl(var(--sidebar-accent))]',
      },
    },
    defaultVariants: {
      variant: 'default',
    },
  },
);

const sidebarMenuButtonVariants = cva(
  'peer/menu-button flex w-full items-center gap-2 overflow-hidden rounded-md p-2 text-left text-sm outline-hidden ring-sidebar-ring transition-[width,height,padding] [&:not([data-highlight])]:hover:bg-sidebar-accent [&:not([data-highlight])]:hover:text-sidebar-accent-foreground focus-visible:ring-2 active:bg-sidebar-accent active:text-sidebar-accent-foreground disabled:pointer-events-none disabled:opacity-50 group-has-data-[sidebar=menu-action]/menu-item:pr-8 aria-disabled:pointer-events-none aria-disabled:opacity-50 data-[active=true]:bg-sidebar-accent data-[active=true]:font-medium data-[active=true]:text-sidebar-accent-foreground [&:not([data-highlight])]:data-[state=open]:hover:bg-sidebar-accent [&:not([data-highlight])]:data-[state=open]:hover:text-sidebar-accent-foreground group-data-[collapsible=icon]:size-8! group-data-[collapsible=icon]:p-2! [&>span:last-child]:truncate [&>svg]:size-4 [&>svg]:shrink-0',
  {
    variants: {
      variant: {
        default:
          '[&:not([data-highlight])]:hover:bg-sidebar-accent [&:not([data-highlight])]:hover:text-sidebar-accent-foreground',
        outline:
          'bg-background shadow-[0_0_0_1px_hsl(var(--sidebar-border))] [&:not([data-highlight])]:hover:bg-sidebar-accent [&:not([data-highlight])]:hover:text-sidebar-accent-foreground [&:not([data-highlight])]:hover:shadow-[0_0_0_1px_hsl(var(--sidebar-accent))]',
      },
      size: {
        default: 'h-8 text-sm',
        sm: 'h-7 text-xs',
        lg: 'h-12 text-sm group-data-[collapsible=icon]:p-0!',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  },
);

type SidebarMenuButtonProps = React.ComponentProps<'button'> & {
  asChild?: boolean;
  isActive?: boolean;
  tooltip?: string | React.ComponentProps<typeof TooltipContent>;
} & VariantProps<typeof sidebarMenuButtonVariants>;

function SidebarMenuButton({
  asChild = false,
  isActive = false,
  variant = 'default',
  size = 'default',
  tooltip,
  className,
  ...props
}: SidebarMenuButtonProps) {
  const Comp = asChild ? Slot.Root : 'button';
  const { isMobile, state } = useSidebar();

  const button = (
    <HighlightItem
      activeClassName={sidebarMenuButtonActiveVariants({ variant })}
    >
      <Comp
        data-slot="sidebar-menu-button"
        data-sidebar="menu-button"
        data-size={size}
        data-active={isActive}
        className={cn(sidebarMenuButtonVariants({ variant, size }), className)}
        {...props}
      />
    </HighlightItem>
  );

  if (!tooltip) {
    return button;
  }

  if (typeof tooltip === 'string') {
    tooltip = {
      children: tooltip,
    };
  }

  return (
    <Tooltip side="right" align="center">
      <TooltipTrigger asChild>{button}</TooltipTrigger>
      <TooltipContent hidden={state !== 'collapsed' || isMobile} {...tooltip} />
    </Tooltip>
  );
}

type SidebarMenuActionProps = React.ComponentProps<'button'> & {
  asChild?: boolean;
  showOnHover?: boolean;
};

function SidebarMenuAction({
  className,
  asChild = false,
  showOnHover = false,
  ...props
}: SidebarMenuActionProps) {
  const Comp = asChild ? Slot.Root : 'button';

  return (
    <Comp
      data-slot="sidebar-menu-action"
      data-sidebar="menu-action"
      className={cn(
        // Increases the hit area of the button on mobile.
        'z-[1] text-sidebar-foreground ring-sidebar-ring hover:bg-sidebar-accent hover:text-sidebar-accent-foreground peer-hover/menu-button:text-sidebar-accent-foreground absolute top-1.5 right-1 flex aspect-square w-5 items-center justify-center rounded-md p-0 outline-hidden transition-transform focus-visible:ring-2 [&>svg]:size-4 [&>svg]:shrink-0',
        'after:absolute after:-inset-2 md:after:hidden',
        'peer-data-[size=sm]/menu-button:top-1',
        'peer-data-[size=default]/menu-button:top-1.5',
        'peer-data-[size=lg]/menu-button:top-2.5',
        'group-data-[collapsible=icon]:hidden',
        showOnHover &&
          'peer-data-[active=true]/menu-button:text-sidebar-accent-foreground group-focus-within/menu-item:opacity-100 group-hover/menu-item:opacity-100 data-[state=open]:opacity-100 md:opacity-0',
        className,
      )}
      {...props}
    />
  );
}

type SidebarMenuBadgeProps = React.ComponentProps<'div'>;

function SidebarMenuBadge({ className, ...props }: SidebarMenuBadgeProps) {
  return (
    <div
      data-slot="sidebar-menu-badge"
      data-sidebar="menu-badge"
      className={cn(
        'text-sidebar-foreground pointer-events-none absolute right-1 flex h-5 min-w-5 items-center justify-center rounded-md px-1 text-xs font-medium tabular-nums select-none',
        'peer-hover/menu-button:text-sidebar-accent-foreground peer-data-[active=true]/menu-button:text-sidebar-accent-foreground',
        'peer-data-[size=sm]/menu-button:top-1',
        'peer-data-[size=default]/menu-button:top-1.5',
        'peer-data-[size=lg]/menu-button:top-2.5',
        'group-data-[collapsible=icon]:hidden',
        className,
      )}
      {...props}
    />
  );
}

type SidebarMenuSkeletonProps = React.ComponentProps<'div'> & {
  showIcon?: boolean;
};

function SidebarMenuSkeleton({
  className,
  showIcon = false,
  ...props
}: SidebarMenuSkeletonProps) {
  // Random width between 50 to 90%.
  const width = React.useMemo(() => {
    return `${Math.floor(Math.random() * 40) + 50}%`;
  }, []);

  return (
    <div
      data-slot="sidebar-menu-skeleton"
      data-sidebar="menu-skeleton"
      className={cn('flex h-8 items-center gap-2 rounded-md px-2', className)}
      {...props}
    >
      {showIcon && (
        <Skeleton
          className="size-4 rounded-md"
          data-sidebar="menu-skeleton-icon"
        />
      )}
      <Skeleton
        className="h-4 max-w-(--skeleton-width) flex-1"
        data-sidebar="menu-skeleton-text"
        style={
          {
            '--skeleton-width': width,
          } as React.CSSProperties
        }
      />
    </div>
  );
}

type SidebarMenuSubProps = React.ComponentProps<'ul'>;

function SidebarMenuSub({ className, ...props }: SidebarMenuSubProps) {
  return (
    <ul
      data-slot="sidebar-menu-sub"
      data-sidebar="menu-sub"
      className={cn(
        'border-sidebar-border mx-3.5 flex min-w-0 translate-x-px flex-col gap-1 border-l px-2.5 py-0.5',
        'group-data-[collapsible=icon]:hidden',
        className,
      )}
      {...props}
    />
  );
}

type SidebarMenuSubItemProps = React.ComponentProps<'li'>;

function SidebarMenuSubItem({ className, ...props }: SidebarMenuSubItemProps) {
  return (
    <li
      data-slot="sidebar-menu-sub-item"
      data-sidebar="menu-sub-item"
      className={cn('group/menu-sub-item relative', className)}
      {...props}
    />
  );
}

type SidebarMenuSubButtonProps = React.ComponentProps<'a'> & {
  asChild?: boolean;
  size?: 'sm' | 'md';
  isActive?: boolean;
};

function SidebarMenuSubButton({
  asChild = false,
  size = 'md',
  isActive = false,
  className,
  ...props
}: SidebarMenuSubButtonProps) {
  const Comp = asChild ? Slot.Root : 'a';

  return (
    <HighlightItem activeClassName="bg-sidebar-accent text-sidebar-accent-foreground rounded-md">
      <Comp
        data-slot="sidebar-menu-sub-button"
        data-sidebar="menu-sub-button"
        data-size={size}
        data-active={isActive}
        className={cn(
          'text-sidebar-foreground ring-sidebar-ring [&:not([data-highlight])]:hover:bg-sidebar-accent [&:not([data-highlight])]:hover:text-sidebar-accent-foreground active:bg-sidebar-accent active:text-sidebar-accent-foreground [&>svg]:text-sidebar-accent-foreground flex h-7 min-w-0 -translate-x-px items-center gap-2 overflow-hidden rounded-md px-2 outline-hidden focus-visible:ring-2 disabled:pointer-events-none disabled:opacity-50 aria-disabled:pointer-events-none aria-disabled:opacity-50 [&>span:last-child]:truncate [&>svg]:size-4 [&>svg]:shrink-0',
          'data-[active=true]:bg-sidebar-accent data-[active=true]:text-sidebar-accent-foreground',
          size === 'sm' && 'text-xs',
          size === 'md' && 'text-sm',
          'group-data-[collapsible=icon]:hidden',
          className,
        )}
        {...props}
      />
    </HighlightItem>
  );
}

export {
  Sidebar,
  SidebarContent,
  SidebarFooter,
  SidebarGroup,
  SidebarGroupAction,
  SidebarGroupContent,
  SidebarGroupLabel,
  SidebarHeader,
  SidebarInput,
  SidebarInset,
  SidebarMenu,
  SidebarMenuAction,
  SidebarMenuBadge,
  SidebarMenuButton,
  SidebarMenuItem,
  SidebarMenuSkeleton,
  SidebarMenuSub,
  SidebarMenuSubButton,
  SidebarMenuSubItem,
  SidebarProvider,
  SidebarRail,
  SidebarSeparator,
  SidebarTrigger,
  useSidebar,
};
```

---

### Switch <a name="switch"></a>

A control that allows the user to toggle between checked and not checked.

- **Category**: Radix
- **Registry Key**: `components-radix-switch`

#### Installation

```bash
npx shadcn@latest add components-radix-switch
```

#### How to Use

```tsx
<Switch />
```

#### Demo Code

##### File: `registry/demo/components/radix/switch/index.tsx`

```tsx
import { Label } from '@/components/ui/label';
import { Switch } from '@/components/animate-ui/components/radix/switch';

export function RadixSwitchDemo() {
  return (
    <Label className="flex items-center gap-x-3">
      <Switch />
      Airplane Mode
    </Label>
  );
}
```

#### Component Source Code

##### File: `registry/components/radix/switch/index.tsx`

```tsx
import * as React from 'react';

import {
  Switch as SwitchPrimitive,
  SwitchThumb as SwitchThumbPrimitive,
  SwitchIcon as SwitchIconPrimitive,
  type SwitchProps as SwitchPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/switch';
import { cn } from '@/lib/utils';

type SwitchProps = SwitchPrimitiveProps & {
  pressedWidth?: number;
  startIcon?: React.ReactElement;
  endIcon?: React.ReactElement;
  thumbIcon?: React.ReactElement;
};

function Switch({
  className,
  pressedWidth = 19,
  startIcon,
  endIcon,
  thumbIcon,
  ...props
}: SwitchProps) {
  return (
    <SwitchPrimitive
      className={cn(
        'relative peer focus-visible:border-ring focus-visible:ring-ring/50 flex h-5 w-8 px-px shrink-0 items-center justify-start rounded-full border border-transparent shadow-xs outline-none focus-visible:ring-[3px] disabled:cursor-not-allowed disabled:opacity-50',
        'data-[state=checked]:bg-primary data-[state=unchecked]:bg-input dark:data-[state=unchecked]:bg-input/80 data-[state=checked]:justify-end',
        className,
      )}
      {...props}
    >
      <SwitchThumbPrimitive
        className={cn(
          'relative z-10 bg-background dark:data-[state=unchecked]:bg-foreground dark:data-[state=checked]:bg-primary-foreground pointer-events-none block size-4 rounded-full ring-0',
        )}
        pressedAnimation={{ width: pressedWidth }}
      >
        {thumbIcon && (
          <SwitchIconPrimitive
            position="thumb"
            className="absolute [&_svg]:size-[9px] left-1/2 top-1/2 -translate-1/2 dark:text-neutral-500 text-neutral-400"
          >
            {thumbIcon}
          </SwitchIconPrimitive>
        )}
      </SwitchThumbPrimitive>

      {startIcon && (
        <SwitchIconPrimitive
          position="left"
          className="absolute [&_svg]:size-[9px] left-0.5 top-1/2 -translate-y-1/2 dark:text-neutral-500 text-neutral-400"
        >
          {startIcon}
        </SwitchIconPrimitive>
      )}
      {endIcon && (
        <SwitchIconPrimitive
          position="right"
          className="absolute [&_svg]:size-[9px] right-0.5 top-1/2 -translate-y-1/2 dark:text-neutral-400 text-neutral-500"
        >
          {endIcon}
        </SwitchIconPrimitive>
      )}
    </SwitchPrimitive>
  );
}

export { Switch, type SwitchProps };
```

---

### Tabs <a name="tabs"></a>

A set of layered sections of content—known as tab panels—that are displayed one at a time.

- **Category**: Radix
- **Registry Key**: `components-radix-tabs`

#### Installation

```bash
npx shadcn@latest add components-radix-tabs
```

#### How to Use

```tsx
<Tabs>
  <TabsList>
    <TabsTrigger value="account">Account</TabsTrigger>
    <TabsTrigger value="password">Password</TabsTrigger>
  </TabsList>
  <TabsContents>
    <TabsContent value="account">
      Make changes to your account here.
    </TabsContent>
    <TabsContent value="password">Change your password here.</TabsContent>
  </TabsContents>
</Tabs>
```

#### Demo Code

##### File: `registry/demo/components/radix/tabs/index.tsx`

```tsx
import {
  Tabs,
  TabsContent,
  TabsContents,
  TabsList,
  TabsTrigger,
} from '@/components/animate-ui/components/radix/tabs';
import { Button } from '@/components/ui/button';
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';

export function RadixTabsDemo() {
  return (
    <div className="flex w-full max-w-sm flex-col gap-6">
      <Tabs defaultValue="account">
        <TabsList>
          <TabsTrigger value="account">Account</TabsTrigger>
          <TabsTrigger value="password">Password</TabsTrigger>
        </TabsList>
        <Card className="shadow-none py-0">
          <TabsContents className="py-6">
            <TabsContent value="account" className="flex flex-col gap-6">
              <CardHeader>
                <CardTitle>Account</CardTitle>
                <CardDescription>
                  Make changes to your account here. Click save when you&apos;re
                  done.
                </CardDescription>
              </CardHeader>
              <CardContent className="grid gap-6">
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-name">Name</Label>
                  <Input id="tabs-demo-name" defaultValue="Pedro Duarte" />
                </div>
              </CardContent>
              <CardFooter>
                <Button>Save changes</Button>
              </CardFooter>
            </TabsContent>
            <TabsContent value="password" className="flex flex-col gap-6">
              <CardHeader>
                <CardTitle>Password</CardTitle>
                <CardDescription>
                  Change your password here. After saving, you&apos;ll be logged
                  out.
                </CardDescription>
              </CardHeader>
              <CardContent className="grid gap-6">
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-current">Current password</Label>
                  <Input id="tabs-demo-current" type="password" />
                </div>
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-new">New password</Label>
                  <Input id="tabs-demo-new" type="password" />
                </div>
              </CardContent>
              <CardFooter>
                <Button>Save password</Button>
              </CardFooter>
            </TabsContent>
          </TabsContents>
        </Card>
      </Tabs>
    </div>
  );
}
```

#### Component Source Code

##### File: `registry/components/radix/tabs/index.tsx`

```tsx
import * as React from 'react';

import {
  Tabs as TabsPrimitive,
  TabsList as TabsListPrimitive,
  TabsTrigger as TabsTriggerPrimitive,
  TabsContent as TabsContentPrimitive,
  TabsContents as TabsContentsPrimitive,
  TabsHighlight as TabsHighlightPrimitive,
  TabsHighlightItem as TabsHighlightItemPrimitive,
  type TabsProps as TabsPrimitiveProps,
  type TabsListProps as TabsListPrimitiveProps,
  type TabsTriggerProps as TabsTriggerPrimitiveProps,
  type TabsContentProps as TabsContentPrimitiveProps,
  type TabsContentsProps as TabsContentsPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/tabs';
import { cn } from '@/lib/utils';

type TabsProps = TabsPrimitiveProps;

function Tabs({ className, ...props }: TabsProps) {
  return (
    <TabsPrimitive
      className={cn('flex flex-col gap-2', className)}
      {...props}
    />
  );
}

type TabsListProps = TabsListPrimitiveProps;

function TabsList({ className, ...props }: TabsListProps) {
  return (
    <TabsHighlightPrimitive className="absolute z-0 inset-0 border border-transparent rounded-md bg-background dark:border-input dark:bg-input/30 shadow-sm">
      <TabsListPrimitive
        className={cn(
          'bg-muted text-muted-foreground inline-flex h-9 w-fit items-center justify-center rounded-lg p-[3px]',
          className,
        )}
        {...props}
      />
    </TabsHighlightPrimitive>
  );
}

type TabsTriggerProps = TabsTriggerPrimitiveProps;

function TabsTrigger({ className, ...props }: TabsTriggerProps) {
  return (
    <TabsHighlightItemPrimitive value={props.value} className="flex-1">
      <TabsTriggerPrimitive
        className={cn(
          "data-[state=active]:text-foreground focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:outline-ring text-muted-foreground inline-flex h-[calc(100%-1px)] flex-1 items-center justify-center gap-1.5 rounded-md w-full px-2 py-1 text-sm font-medium whitespace-nowrap transition-colors duration-500 ease-in-out focus-visible:ring-[3px] focus-visible:outline-1 disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
          className,
        )}
        {...props}
      />
    </TabsHighlightItemPrimitive>
  );
}

type TabsContentsProps = TabsContentsPrimitiveProps;

function TabsContents(props: TabsContentsProps) {
  return <TabsContentsPrimitive {...props} />;
}

type TabsContentProps = TabsContentPrimitiveProps;

function TabsContent({ className, ...props }: TabsContentProps) {
  return (
    <TabsContentPrimitive
      className={cn('flex-1 outline-none', className)}
      {...props}
    />
  );
}

export {
  Tabs,
  TabsList,
  TabsTrigger,
  TabsContents,
  TabsContent,
  type TabsProps,
  type TabsListProps,
  type TabsTriggerProps,
  type TabsContentsProps,
  type TabsContentProps,
};
```

---

### Toggle Group <a name="toggle-group"></a>

A set of two-state buttons that can be toggled on or off.

- **Category**: Radix
- **Registry Key**: `components-radix-toggle-group`

#### Installation

```bash
npx shadcn@latest add components-radix-toggle-group
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`

#### How to Use

```tsx
<ToggleGroup type="single" defaultValue="bold">
  <ToggleGroupItem value="bold" />
  <ToggleGroupItem value="italic" />
  <ToggleGroupItem value="underline" />
</ToggleGroup>
```

#### Demo Code

##### File: `registry/demo/components/radix/toggle-group/index.tsx`

```tsx
import {
  ToggleGroup,
  ToggleGroupItem,
  type ToggleGroupProps,
} from '@/components/animate-ui/components/radix/toggle-group';
import { Bold, Italic, Underline } from 'lucide-react';

interface RadixToggleGroupDemoProps {
  type: 'single' | 'multiple';
  variant: ToggleGroupProps['variant'];
  size: ToggleGroupProps['size'];
}

export function RadixToggleGroupDemo({
  type,
  variant,
  size,
}: RadixToggleGroupDemoProps) {
  return (
    <ToggleGroup type={type} variant={variant} size={size}>
      <ToggleGroupItem value="bold" aria-label="Toggle bold">
        <Bold />
      </ToggleGroupItem>
      <ToggleGroupItem value="italic" aria-label="Toggle italic">
        <Italic />
      </ToggleGroupItem>
      <ToggleGroupItem value="strikethrough" aria-label="Toggle strikethrough">
        <Underline />
      </ToggleGroupItem>
    </ToggleGroup>
  );
}
```

#### Component Source Code

##### File: `registry/components/radix/toggle-group/index.tsx`

```tsx
import * as React from 'react';
import { type VariantProps } from 'class-variance-authority';

import {
  ToggleGroup as ToggleGroupPrimitive,
  ToggleGroupItem as ToggleGroupItemPrimitive,
  ToggleGroupHighlight as ToggleGroupHighlightPrimitive,
  ToggleGroupHighlightItem as ToggleGroupHighlightItemPrimitive,
  useToggleGroup as useToggleGroupPrimitive,
  type ToggleGroupProps as ToggleGroupPrimitiveProps,
  type ToggleGroupItemProps as ToggleGroupItemPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/toggle-group';
import { toggleVariants } from '@/components/animate-ui/components/radix/toggle';
import { cn } from '@/lib/utils';
import { getStrictContext } from '@/lib/get-strict-context';

const [ToggleGroupProvider, useToggleGroup] =
  getStrictContext<VariantProps<typeof toggleVariants>>('ToggleGroupContext');

type ToggleGroupProps = ToggleGroupPrimitiveProps &
  VariantProps<typeof toggleVariants>;

function ToggleGroup({
  className,
  variant,
  size,
  children,
  ...props
}: ToggleGroupProps) {
  return (
    <ToggleGroupPrimitive
      data-variant={variant}
      data-size={size}
      className={cn(
        'group/toggle-group flex gap-0.5 w-fit items-center rounded-lg data-[variant=outline]:shadow-xs data-[variant=outline]:border data-[variant=outline]:p-0.5',
        className,
      )}
      {...props}
    >
      <ToggleGroupProvider value={{ variant, size }}>
        {props.type === 'single' ? (
          <ToggleGroupHighlightPrimitive className="bg-accent rounded-md">
            {children}
          </ToggleGroupHighlightPrimitive>
        ) : (
          children
        )}
      </ToggleGroupProvider>
    </ToggleGroupPrimitive>
  );
}

type ToggleGroupItemProps = ToggleGroupItemPrimitiveProps &
  VariantProps<typeof toggleVariants>;

function ToggleGroupItem({
  className,
  children,
  variant,
  size,
  ...props
}: ToggleGroupItemProps) {
  const { variant: contextVariant, size: contextSize } = useToggleGroup();
  const { type } = useToggleGroupPrimitive();

  return (
    <ToggleGroupHighlightItemPrimitive
      value={props.value}
      className={cn(type === 'multiple' && 'bg-accent rounded-md')}
    >
      <ToggleGroupItemPrimitive
        data-variant={contextVariant || variant}
        data-size={contextSize || size}
        className={cn(
          toggleVariants({
            variant: contextVariant || variant,
            size: contextSize || size,
          }),
          'min-w-0 border-0 flex-1 shrink-0 shadow-none rounded-md focus:z-10 focus-visible:z-10',
          className,
        )}
        {...props}
      >
        {children}
      </ToggleGroupItemPrimitive>
    </ToggleGroupHighlightItemPrimitive>
  );
}

export {
  ToggleGroup,
  ToggleGroupItem,
  type ToggleGroupProps,
  type ToggleGroupItemProps,
};
```

---

### Toggle <a name="toggle"></a>

A two-state button that can be either on or off.

- **Category**: Radix
- **Registry Key**: `components-radix-toggle`

#### Installation

```bash
npx shadcn@latest add components-radix-toggle
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`

#### How to Use

```tsx
<Toggle />
```

#### Demo Code

##### File: `registry/demo/components/radix/toggle/index.tsx`

```tsx
import { Toggle, type ToggleProps } from '@/components/animate-ui/components/radix/toggle';
import { Bold } from 'lucide-react';

interface RadixToggleDemoProps {
  variant: ToggleProps['variant'];
  size: ToggleProps['size'];
}

export function RadixToggleDemo({ variant, size }: RadixToggleDemoProps) {
  return (
    <Toggle aria-label="Toggle italic" variant={variant} size={size}>
      <Bold />
    </Toggle>
  );
}
```

#### Component Source Code

##### File: `registry/components/radix/toggle/index.tsx`

```tsx
import * as React from 'react';
import { cva, type VariantProps } from 'class-variance-authority';

import {
  Toggle as TogglePrimitive,
  ToggleItem as ToggleItemPrimitive,
  ToggleHighlight as ToggleHighlightPrimitive,
  type ToggleProps as TogglePrimitiveProps,
  type ToggleItemProps as ToggleItemPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/toggle';
import { cn } from '@/lib/utils';

const toggleVariants = cva(
  "inline-flex items-center justify-center gap-2 rounded-md text-sm font-medium hover:bg-muted/40 hover:text-muted-foreground disabled:pointer-events-none disabled:opacity-50 data-[state=on]:text-accent-foreground [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4 [&_svg]:shrink-0 focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px] outline-none transition-[color,background-color,box-shadow] duration-200 ease-in-out aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive whitespace-nowrap",
  {
    variants: {
      variant: {
        default: 'bg-transparent',
        outline:
          'border border-input bg-transparent shadow-xs hover:bg-accent/40 hover:text-accent-foreground',
      },
      size: {
        default: 'h-9 px-2 min-w-9',
        sm: 'h-8 px-1.5 min-w-8',
        lg: 'h-10 px-2.5 min-w-10',
        icon: 'size-9',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  },
);

type ToggleProps = TogglePrimitiveProps &
  ToggleItemPrimitiveProps &
  VariantProps<typeof toggleVariants>;

function Toggle({
  className,
  variant,
  size,
  pressed,
  defaultPressed,
  onPressedChange,
  disabled,
  ...props
}: ToggleProps) {
  return (
    <TogglePrimitive
      pressed={pressed}
      defaultPressed={defaultPressed}
      onPressedChange={onPressedChange}
      disabled={disabled}
      className="relative"
    >
      <ToggleHighlightPrimitive className="bg-accent rounded-md" />
      <ToggleItemPrimitive
        className={cn(toggleVariants({ variant, size, className }))}
        {...props}
      />
    </TogglePrimitive>
  );
}

export { Toggle, toggleVariants, type ToggleProps };
```

---

### Tooltip <a name="tooltip"></a>

A popup that displays information related to an element when the element receives keyboard focus or the mouse hovers over it.

- **Category**: Radix
- **Registry Key**: `components-radix-tooltip`

#### Installation

```bash
npx shadcn@latest add components-radix-tooltip
```

#### How to Use

```tsx
<Tooltip>
  <TooltipTrigger>Open tooltip</TooltipTrigger>
  <TooltipContent>
    <p>Tooltip content</p>
  </TooltipContent>
</Tooltip>
```

#### Demo Code

##### File: `registry/demo/components/radix/tooltip/index.tsx`

```tsx
import {
  Tooltip,
  TooltipTrigger,
  TooltipContent,
  type TooltipContentProps,
} from '@/components/animate-ui/components/radix/tooltip';
import { Button } from '@/components/ui/button';

interface RadixTooltipDemoProps {
  side: TooltipContentProps['side'];
  sideOffset: TooltipContentProps['sideOffset'];
  align: TooltipContentProps['align'];
  alignOffset: TooltipContentProps['alignOffset'];
  followCursor?: boolean | 'x' | 'y';
}

export function RadixTooltipDemo({
  side,
  sideOffset,
  align,
  alignOffset,
  followCursor,
}: RadixTooltipDemoProps) {
  return (
    <Tooltip followCursor={followCursor}>
      <TooltipTrigger asChild>
        <Button variant="outline">Hover</Button>
      </TooltipTrigger>
      <TooltipContent
        side={side}
        sideOffset={sideOffset}
        align={align}
        alignOffset={alignOffset}
      >
        <p>Add to library</p>
      </TooltipContent>
    </Tooltip>
  );
}
```

#### Component Source Code

##### File: `registry/components/radix/tooltip/index.tsx`

```tsx
import * as React from 'react';

import {
  TooltipProvider as TooltipProviderPrimitive,
  Tooltip as TooltipPrimitive,
  TooltipTrigger as TooltipTriggerPrimitive,
  TooltipContent as TooltipContentPrimitive,
  TooltipArrow as TooltipArrowPrimitive,
  TooltipPortal as TooltipPortalPrimitive,
  type TooltipProviderProps as TooltipProviderPrimitiveProps,
  type TooltipProps as TooltipPrimitiveProps,
  type TooltipTriggerProps as TooltipTriggerPrimitiveProps,
  type TooltipContentProps as TooltipContentPrimitiveProps,
} from '@/components/animate-ui/primitives/radix/tooltip';
import { cn } from '@/lib/utils';

type TooltipProviderProps = TooltipProviderPrimitiveProps;

function TooltipProvider({
  delayDuration = 0,
  ...props
}: TooltipProviderProps) {
  return <TooltipProviderPrimitive delayDuration={delayDuration} {...props} />;
}

type TooltipProps = TooltipPrimitiveProps & {
  delayDuration?: TooltipPrimitiveProps['delayDuration'];
};

function Tooltip({ delayDuration = 0, ...props }: TooltipProps) {
  return (
    <TooltipProvider delayDuration={delayDuration}>
      <TooltipPrimitive {...props} />
    </TooltipProvider>
  );
}

type TooltipTriggerProps = TooltipTriggerPrimitiveProps;

function TooltipTrigger({ ...props }: TooltipTriggerProps) {
  return <TooltipTriggerPrimitive {...props} />;
}

type TooltipContentProps = TooltipContentPrimitiveProps;

function TooltipContent({
  className,
  sideOffset,
  children,
  ...props
}: TooltipContentProps) {
  return (
    <TooltipPortalPrimitive>
      <TooltipContentPrimitive
        sideOffset={sideOffset}
        className={cn(
          'bg-primary text-primary-foreground z-50 w-fit origin-(--radix-tooltip-content-transform-origin) rounded-md px-3 py-1.5 text-xs text-balance',
          className,
        )}
        {...props}
      >
        {children}
        <TooltipArrowPrimitive className="bg-primary fill-primary z-50 size-2.5 translate-y-[calc(-50%_-_2px)] rotate-45 rounded-[2px]" />
      </TooltipContentPrimitive>
    </TooltipPortalPrimitive>
  );
}

export {
  Tooltip,
  TooltipTrigger,
  TooltipContent,
  type TooltipProps,
  type TooltipTriggerProps,
  type TooltipContentProps,
};
```

---
