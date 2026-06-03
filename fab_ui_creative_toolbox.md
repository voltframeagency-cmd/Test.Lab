# Fab UI Creative Developer Reference Guide

A complete, structured reference guide for **Fab UI** (fab-ui.com) — a set of high-end interactive React components wrapping paper-design WebGL shaders, animated using Framer Motion, class-variance-authority sizing, and Tailwind CSS.

## 💎 Special Highlight: The Liquid Metal & Mesh Gradient Suite
Fab UI utilizes highly optimized custom WebGL shaders loaded via `@paper-design/shaders-react` to deliver sensory-rich interfaces. Below is the custom design implementation detail for its signature pieces:

### 1. Liquid Metal Button
- **Concept**: A declarative React button with an organic, fluid mercury backdrop.
- **Implementation**: Wraps `@paper-design/shaders-react`'s `<LiquidMetal />` component. It uses `class-variance-authority` (`cva`) to handle sizes (`sm`, `md`, `lg`) and manages the padding with a formula: `inset-[calc(var(--spacing)*0.68)]` to overlay the black center plate on top of the liquid canvas, exposing only the borders as the animated liquid metal rim. The label text floats on top with a relative index.
### 2. Liquid Metal Avatar
- **Concept**: Renders user profiles with an animated, morphing metallic outer ring.
- **Implementation**: Displays the profile image in the center, wrapped in a parent frame that exposes the `<LiquidMetal />` shader component around its borders as a fluid ring. Uses a custom image shadow and border masking.
### 3. Liquid Metal Card
- **Concept**: Renders container components with high-quality liquid metal backgrounds.
- **Implementation**: Employs `<LiquidMetal />` spanning 100% width and height as a backplate, layered beneath a glassmorphic overlay (`backdrop-blur`) with subtle border shadows, giving a sense of fluid metal floating behind glass.
### 4. Mesh Gradient Card
- **Concept**: A container card displaying a colorful, dynamic animated mesh gradient backdrop.
- **Implementation**: Renders via `<MeshGradient />` using 4 color stops. It contains a state machine `useGradientCycler` cycling through **7 discrete predefined steps** of distortion, swirl, rotation, scale, speed, grain mixer, grain overlay, and offset. It leverages Framer Motion's `animate(from, to, {...})` function to smoothly transition between these steps at a set interval (default 4000ms), giving an organic, continuous shifting mesh aesthetic.

#### Mesh Gradient Card Source Code (`registry/default/ui/mesh-gradient-card.tsx`)
```tsx
'use client';

import React, {
  createContext,
  useCallback,
  useContext,
  useEffect,
  useRef,
  useState,
} from 'react';

import {
  MeshGradient,
  type MeshGradientParams,
  type MeshGradientProps,
} from '@paper-design/shaders-react';
import { animate, AnimatePresence, motion } from 'motion/react';

import { cn } from '@/lib/utils';

type GradientStep = Required<
  Pick<
    MeshGradientParams,
    | 'distortion'
    | 'swirl'
    | 'grainMixer'
    | 'grainOverlay'
    | 'speed'
    | 'scale'
    | 'rotation'
    | 'offsetX'
    | 'offsetY'
  >
>;

const DEFAULT_STEPS: GradientStep[] = [
  {
    distortion: 0.3,
    swirl: 0.1,
    grainMixer: 0.16,
    grainOverlay: 0.15,
    speed: 0.45,
    scale: 1.0,
    rotation: 0,
    offsetX: 0,
    offsetY: 0,
  },
  {
    distortion: 0.5,
    swirl: 0.3,
    grainMixer: 0.1,
    grainOverlay: 0.1,
    speed: 0.55,
    scale: 1.2,
    rotation: 15,
    offsetX: 0.1,
    offsetY: -0.05,
  },
  {
    distortion: 0.4,
    swirl: 0.8,
    grainMixer: 0.2,
    grainOverlay: 0.1,
    speed: 0.38,
    scale: 0.9,
    rotation: -10,
    offsetX: -0.15,
    offsetY: 0.1,
  },
  {
    distortion: 0.9,
    swirl: 0.5,
    grainMixer: 0.12,
    grainOverlay: 0.2,
    speed: 0.65,
    scale: 1.4,
    rotation: 25,
    offsetX: 0.05,
    offsetY: -0.1,
  },
  {
    distortion: 0.25,
    swirl: 0.2,
    grainMixer: 0.6,
    grainOverlay: 0.5,
    speed: 0.5,
    scale: 1.1,
    rotation: -5,
    offsetX: -0.05,
    offsetY: 0.05,
  },
  {
    distortion: 0.6,
    swirl: 0.9,
    grainMixer: 0.08,
    grainOverlay: 0.05,
    speed: 0.3,
    scale: 0.8,
    rotation: 35,
    offsetX: 0.12,
    offsetY: 0.08,
  },
  {
    distortion: 0.15,
    swirl: 0.05,
    grainMixer: 0.5,
    grainOverlay: 0.4,
    speed: 0.6,
    scale: 1.3,
    rotation: -20,
    offsetX: -0.1,
    offsetY: -0.08,
  },
];

const MeshGradientCardContext = createContext<{ messageIndex: number }>({
  messageIndex: 0,
});

type MeshGradientCardProps = Partial<
  Omit<MeshGradientProps, 'width' | 'height'>
> & {
  steps?: GradientStep[];
  interval?: number;
  children?: React.ReactNode;
};

function MeshGradientCardRoot({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='mesh-gradient-card'
      className={cn('relative overflow-hidden rounded-4xl', className)}
      {...props}
    />
  );
}

function MeshGradientCard({
  className,
  children,
  colors = ['#fafafa', '#18181b', '#fafafa', '#18181b'],
  steps = DEFAULT_STEPS,
  interval = 4000,
  ...shaderProps
}: MeshGradientCardProps) {
  const { config, messageIndex } = useGradientCycler(steps, interval);

  return (
    <MeshGradientCardContext.Provider value={{ messageIndex }}>
      <MeshGradientCardRoot className={className}>
        <MeshGradient
          width='100%'
          height='100%'
          colors={colors}
          {...config}
          {...shaderProps}
        />
        {children}
      </MeshGradientCardRoot>
    </MeshGradientCardContext.Provider>
  );
}

function MeshGradientCardMessages({
  children,
  className,
}: {
  children?: React.ReactNode;
  className?: string;
}) {
  const { messageIndex } = useContext(MeshGradientCardContext);
  const items = React.Children.toArray(children);
  const active = items[messageIndex % items.length];

  return (
    <span data-slot='mesh-gradient-card-messages' className={className}>
      <AnimatePresence mode='wait'>
        {React.isValidElement(active)
          ? React.cloneElement(active, {
              key: messageIndex,
            } as React.Attributes)
          : active}
      </AnimatePresence>
    </span>
  );
}

function MeshGradientCardMessage({
  children,
  className,
}: {
  children?: React.ReactNode;
  className?: string;
}) {
  return (
    <motion.span
      data-slot='mesh-gradient-card-message'
      initial={{ opacity: 0, filter: 'blur(8px)' }}
      animate={{ opacity: 1, filter: 'blur(0px)' }}
      exit={{ opacity: 0, filter: 'blur(8px)' }}
      transition={{ duration: 0.45, ease: 'easeInOut' }}
      style={{
        textShadow: '0 1px 4px rgba(0,0,0,0.3), 0 4px 12px rgba(0,0,0,0.2)',
      }}
      className={cn(
        'absolute right-5 bottom-4 pb-1 text-lg font-medium text-white select-none',
        className
      )}
    >
      {children}
    </motion.span>
  );
}

function useGradientCycler(
  steps: GradientStep[] = DEFAULT_STEPS,
  interval: number = 4000
) {
  const [stepIndex, setStepIndex] = useState(0);
  const [config, setConfig] = useState<GradientStep>(steps[0]);
  const animationRef = useRef<ReturnType<typeof animate> | null>(null);
  const [messageIndex, setMessageIndex] = useState(0);

  const goToStep = useCallback(
    (n: number) => {
      if (n === stepIndex) return;
      animationRef.current?.stop();

      const from = { ...config };
      const to = steps[n];

      animationRef.current = animate(from, to, {
        duration: 0.6,
        ease: 'easeOut',
        onUpdate: () => setConfig({ ...from }),
      });

      setStepIndex(n);
      setMessageIndex((i) => i + 1);
    },
    [stepIndex, config, steps]
  );

  useEffect(() => {
    const id = setTimeout(() => {
      goToStep((stepIndex + 1) % steps.length);
    }, interval);
    return () => clearTimeout(id);
  }, [stepIndex, goToStep, steps.length, interval]);

  return { config, stepIndex, messageIndex, goToStep };
}

export {
  MeshGradientCard,
  MeshGradientCardMessage,
  MeshGradientCardMessages,
  MeshGradientCardRoot,
  useGradientCycler,
};

export type { GradientStep, MeshGradientCardProps };
```

## Table of Contents
- [Accordion](#accordion)
- [Alert Dialog](#alert-dialog)
- [Autocomplete](#autocomplete)
- [Avatar](#avatar)
- [Badge](#badge)
- [Button](#button)
- [Card](#card)
- [Checkbox](#checkbox)
- [Checkbox Group](#checkbox-group)
- [Collapsible](#collapsible)
- [Combobox](#combobox)
- [Context Menu](#context-menu)
- [Dialog](#dialog)
- [Drawer](#drawer)
- [Dropdown Menu](#dropdown-menu)
- [Field](#field)
- [Fieldset](#fieldset)
- [Form](#form)
- [Hover Card](#hover-card)
- [Input](#input)
- [Input Group](#input-group)
- [Label](#label)
- [Liquid Metal Avatar](#liquid-metal-avatar)
- [Liquid Metal Button](#liquid-metal-button)
- [Liquid Metal Card](#liquid-metal-card)
- [Mesh Gradient Card](#mesh-gradient-card)
- [Meter](#meter)
- [Navigation Menu](#navigation-menu)
- [Number Field](#number-field)
- [Popover](#popover)
- [Progress](#progress)
- [Radio Group](#radio-group)
- [Scroll Area](#scroll-area)
- [Select](#select)
- [Separator](#separator)
- [Slider](#slider)
- [Switch](#switch)
- [Tabs](#tabs)
- [Textarea](#textarea)
- [Toggle](#toggle)
- [Toggle Group](#toggle-group)
- [Toolbar](#toolbar)
- [Tooltip](#tooltip)
- [Utils](#utils)

---

## Accordion <a name="accordion"></a>

A set of collapsible panels with headings.

- **Registry Key**: `accordion`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/accordion.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `lucide-react`

#### How to Use
##### Example: `default`
```tsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from '@/registry/default/ui/accordion';

type AccordionItem = {
  id: string;
  title: string;
  content: string;
};

export const accordionItems: AccordionItem[] = [
  {
    id: 'item-1',
    title: 'What is your return policy?',
    content:
      "We accept returns within 30 days of purchase, provided the item is unused and in its original condition. To start a return, you'll need your order number and the email address used at checkout. Once your return is approved, we'll send you instructions along with a prepaid return label (where applicable). Refunds are typically processed within 5-7 business days after the item is received.",
  },
  {
    id: 'item-2',
    title: 'How long does shipping take?',
    content:
      "Shipping times depend on your location and the shipping method selected at checkout. Standard shipping usually takes 3-5 business days, while express shipping can arrive in 1-2 business days. You'll receive a tracking link as soon as your order ships, so you can monitor delivery progress. Please note that delays may occur during peak periods or due to customs processing for international orders.",
  },
  {
    id: 'item-3',
    title: 'Do you offer customer support?',
    content:
      'Yes, our customer support team is here to help. You can reach us Monday through Friday from 9am to 5pm via email or live chat. We aim to respond to all inquiries within 24 hours, and often much sooner. For faster service, include your order number and a brief description of the issue so we can resolve it as quickly as possible.',
  },
];

export function AccordionDemo() {
  return (
    <Accordion className='w-full'>
      {accordionItems.map((accordionItem) => {
        return (
          <AccordionItem key={accordionItem.id} value={accordionItem.id}>
            <AccordionTrigger>{accordionItem.title}</AccordionTrigger>
            <AccordionContent>{accordionItem.content}</AccordionContent>
          </AccordionItem>
        );
      })}
    </Accordion>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/accordion.tsx`
```tsx
'use client';

import { Accordion as AccordionPrimitive } from '@base-ui/react/accordion';
import { PlusIcon } from 'lucide-react';

import { cn } from '@/lib/utils';

function Accordion({ className, ...props }: AccordionPrimitive.Root.Props) {
  return (
    <AccordionPrimitive.Root
      data-slot='accordion'
      className={cn('flex w-full flex-col', className)}
      {...props}
    />
  );
}

function AccordionItem({ className, ...props }: AccordionPrimitive.Item.Props) {
  return (
    <AccordionPrimitive.Item
      data-slot='accordion-item'
      className={cn('not-last:border-b', className)}
      {...props}
    />
  );
}

function AccordionTrigger({
  className,
  children,
  ...props
}: AccordionPrimitive.Trigger.Props) {
  return (
    <AccordionPrimitive.Header className='mt-0 mb-0 flex py-4'>
      <AccordionPrimitive.Trigger
        className={cn(
          'group/accordion-trigger relative flex flex-1 cursor-pointer items-start justify-between gap-4 rounded-md border border-transparent text-left text-sm font-medium transition-all outline-none hover:underline focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50 focus-visible:after:border-ring disabled:pointer-events-none disabled:opacity-64 **:data-[slot=accordion-trigger-icon]:ml-auto **:data-[slot=accordion-trigger-icon]:size-4 **:data-[slot=accordion-trigger-icon]:text-muted-foreground [&[data-panel-open]>svg]:rotate-45',
          className
        )}
        data-slot='accordion-trigger'
        {...props}
      >
        {children}
        <PlusIcon className='pointer-events-none size-4 shrink-0 translate-y-0.5 opacity-72 transition-transform duration-150 ease-out' />
      </AccordionPrimitive.Trigger>
    </AccordionPrimitive.Header>
  );
}

function AccordionContent({
  className,
  children,
  ...props
}: AccordionPrimitive.Panel.Props) {
  return (
    <AccordionPrimitive.Panel
      className='h-(--accordion-panel-height) overflow-hidden text-sm text-muted-foreground transition-[height] duration-150 ease-out data-ending-style:h-0 data-starting-style:h-0'
      data-slot='accordion-panel'
      {...props}
    >
      <div className={cn('pt-0 pb-4', className)}>{children}</div>
    </AccordionPrimitive.Panel>
  );
}

export { Accordion, AccordionItem, AccordionTrigger, AccordionContent };
```
---

## Alert Dialog <a name="alert-dialog"></a>

A dialog that requires a user response to proceed.

- **Registry Key**: `alert-dialog`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/alert-dialog.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

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
} from '@/registry/default/ui/alert-dialog';
import { Button } from '@/registry/default/ui/button';

export function AlertDialogDemo() {
  return (
    <AlertDialog>
      <AlertDialogTrigger
        render={<Button variant='outline'>Show Dialog</Button>}
      />
      <AlertDialogContent>
        <AlertDialogHeader>
          <AlertDialogTitle>Are you sure?</AlertDialogTitle>
          <AlertDialogDescription>
            This action cannot be undone. This will permanently delete your data
            from our servers.
          </AlertDialogDescription>
        </AlertDialogHeader>
        <AlertDialogFooter>
          <AlertDialogCancel>Cancel</AlertDialogCancel>
          <AlertDialogAction>Continue</AlertDialogAction>
        </AlertDialogFooter>
      </AlertDialogContent>
    </AlertDialog>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/alert-dialog.tsx`
```tsx
'use client';

import * as React from 'react';

import { Button } from '@/registry/default/ui/button';
import { AlertDialog as AlertDialogPrimitive } from '@base-ui/react/alert-dialog';

import { cn } from '@/lib/utils';

function AlertDialog({ ...props }: AlertDialogPrimitive.Root.Props) {
  return <AlertDialogPrimitive.Root data-slot='alert-dialog' {...props} />;
}

function AlertDialogTrigger({ ...props }: AlertDialogPrimitive.Trigger.Props) {
  return (
    <AlertDialogPrimitive.Trigger data-slot='alert-dialog-trigger' {...props} />
  );
}

function AlertDialogPortal({ ...props }: AlertDialogPrimitive.Portal.Props) {
  return (
    <AlertDialogPrimitive.Portal data-slot='alert-dialog-portal' {...props} />
  );
}

function AlertDialogOverlay({
  className,
  ...props
}: AlertDialogPrimitive.Backdrop.Props) {
  return (
    <AlertDialogPrimitive.Backdrop
      data-slot='alert-dialog-overlay'
      className={cn(
        'fixed inset-0 isolate z-50 bg-black/10 duration-100 data-closed:animate-out data-closed:fade-out-0 data-open:animate-in data-open:fade-in-0 supports-backdrop-filter:backdrop-blur-xs',
        className
      )}
      {...props}
    />
  );
}

function AlertDialogContent({
  className,
  size = 'default',
  ...props
}: AlertDialogPrimitive.Popup.Props & {
  size?: 'default' | 'sm';
}) {
  return (
    <AlertDialogPortal>
      <AlertDialogOverlay />
      <AlertDialogPrimitive.Popup
        data-slot='alert-dialog-content'
        data-size={size}
        className={cn(
          'group/alert-dialog-content fixed top-1/2 left-1/2 z-50 grid w-full -translate-x-1/2 -translate-y-1/2 gap-4 rounded-4xl bg-background p-4 ring-1 ring-foreground/10 duration-100 outline-none data-closed:animate-out data-closed:fade-out-0 data-closed:zoom-out-95 data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 data-[size=default]:max-w-xs data-[size=sm]:max-w-xs data-[size=default]:sm:max-w-sm',
          className
        )}
        {...props}
      />
    </AlertDialogPortal>
  );
}

function AlertDialogHeader({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='alert-dialog-header'
      className={cn(
        'grid grid-rows-[auto_1fr] place-items-center gap-1.5 text-center has-data-[slot=alert-dialog-media]:grid-rows-[auto_auto_1fr] has-data-[slot=alert-dialog-media]:gap-x-4 sm:group-data-[size=default]/alert-dialog-content:place-items-start sm:group-data-[size=default]/alert-dialog-content:text-left sm:group-data-[size=default]/alert-dialog-content:has-data-[slot=alert-dialog-media]:grid-rows-[auto_1fr]',
        className
      )}
      {...props}
    />
  );
}

function AlertDialogFooter({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='alert-dialog-footer'
      className={cn(
        '-mx-4 -mb-4 flex flex-col-reverse gap-2 rounded-b-4xl border-t bg-background p-4 group-data-[size=sm]/alert-dialog-content:grid group-data-[size=sm]/alert-dialog-content:grid-cols-2 sm:flex-row sm:justify-end',
        className
      )}
      {...props}
    />
  );
}

function AlertDialogMedia({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='alert-dialog-media'
      className={cn(
        "mb-2 inline-flex size-10 items-center justify-center rounded-md bg-muted sm:group-data-[size=default]/alert-dialog-content:row-span-2 *:[svg:not([class*='size-'])]:size-6",
        className
      )}
      {...props}
    />
  );
}

function AlertDialogTitle({
  className,
  ...props
}: React.ComponentProps<typeof AlertDialogPrimitive.Title>) {
  return (
    <AlertDialogPrimitive.Title
      data-slot='alert-dialog-title'
      className={cn(
        'text-base font-medium sm:group-data-[size=default]/alert-dialog-content:group-has-data-[slot=alert-dialog-media]/alert-dialog-content:col-start-2',
        className
      )}
      {...props}
    />
  );
}

function AlertDialogDescription({
  className,
  ...props
}: React.ComponentProps<typeof AlertDialogPrimitive.Description>) {
  return (
    <AlertDialogPrimitive.Description
      data-slot='alert-dialog-description'
      className={cn(
        'text-sm text-balance text-muted-foreground md:text-pretty *:[a]:underline *:[a]:underline-offset-3 *:[a]:hover:text-foreground',
        className
      )}
      {...props}
    />
  );
}

function AlertDialogAction({
  className,
  ...props
}: React.ComponentProps<typeof Button>) {
  return (
    <Button
      data-slot='alert-dialog-action'
      className={cn(className)}
      {...props}
    />
  );
}

function AlertDialogCancel({
  className,
  variant = 'outline',
  size = 'default',
  ...props
}: AlertDialogPrimitive.Close.Props &
  Pick<React.ComponentProps<typeof Button>, 'variant' | 'size'>) {
  return (
    <AlertDialogPrimitive.Close
      data-slot='alert-dialog-cancel'
      className={cn(className)}
      render={<Button variant={variant} size={size} />}
      {...props}
    />
  );
}

export {
  AlertDialog,
  AlertDialogAction,
  AlertDialogCancel,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogHeader,
  AlertDialogMedia,
  AlertDialogOverlay,
  AlertDialogPortal,
  AlertDialogTitle,
  AlertDialogTrigger,
};
```
---

## Autocomplete <a name="autocomplete"></a>

An input that suggests options as you type.

- **Registry Key**: `autocomplete`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/autocomplete.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `lucide-react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import {
  Autocomplete,
  AutocompleteEmpty,
  AutocompleteInput,
  AutocompleteItem,
  AutocompleteList,
  AutocompletePopup,
} from '@/registry/default/ui/autocomplete';

const items = [
  { label: 'React', value: 'react' },
  { label: 'Next.js', value: 'nextjs' },
  { label: 'Vue.js', value: 'vue' },
  { label: 'Angular', value: 'angular' },
  { label: 'Svelte', value: 'svelte' },
  { label: 'TypeScript', value: 'typescript' },
  { label: 'Express', value: 'express' },
  { label: 'Tailwind CSS', value: 'tailwindcss' },
];

export function AutocompleteDemo() {
  return (
    <div className='w-full max-w-xs'>
      <Autocomplete items={items}>
        <AutocompleteInput
          aria-label='Search frameworks'
          placeholder='Search frameworks...'
        />
        <AutocompletePopup>
          <AutocompleteEmpty>No items found.</AutocompleteEmpty>
          <AutocompleteList>
            {(item) => (
              <AutocompleteItem key={item.value} value={item}>
                {item.label}
              </AutocompleteItem>
            )}
          </AutocompleteList>
        </AutocompletePopup>
      </Autocomplete>
    </div>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/autocomplete.tsx`
```tsx
'use client';

import { Button } from '@/registry/default/ui/button';
import { Input } from '@/registry/default/ui/input';
import { Autocomplete as AutocompletePrimitive } from '@base-ui/react/autocomplete';
import { XIcon } from 'lucide-react';

import { cn } from '@/lib/utils';

const Autocomplete = AutocompletePrimitive.Root;

function AutocompleteInput(props: AutocompletePrimitive.Input.Props) {
  return (
    <AutocompletePrimitive.Input
      data-slot='autocomplete-input'
      render={<Input />}
      {...props}
    />
  );
}

function AutocompletePopup({
  className,
  sideOffset = 4,
  children,
  ...props
}: AutocompletePrimitive.Popup.Props & {
  sideOffset?: number;
}) {
  return (
    <AutocompletePositioner sideOffset={sideOffset}>
      <AutocompletePrimitive.Popup
        data-slot='autocomplete-popup'
        className={cn(
          'max-h-[min(var(--available-height),23rem)] w-(--anchor-width) max-w-(--available-width) scroll-pt-2 scroll-pb-2 overflow-y-auto overscroll-contain rounded-2xl bg-popover text-popover-foreground shadow-lg outline-1 outline-muted dark:shadow-none dark:-outline-offset-1 dark:outline-muted',
          className
        )}
        {...props}
      >
        {children}
      </AutocompletePrimitive.Popup>
    </AutocompletePositioner>
  );
}

function AutocompletePositioner({
  className,
  ...props
}: AutocompletePrimitive.Positioner.Props) {
  return (
    <AutocompletePrimitive.Portal>
      <AutocompletePrimitive.Positioner
        data-slot='autocomplete-positioner'
        className={cn('outline-none', className)}
        {...props}
      />
    </AutocompletePrimitive.Portal>
  );
}

function AutocompleteList({
  className,
  ...props
}: AutocompletePrimitive.List.Props) {
  return (
    <AutocompletePrimitive.List
      data-slot='autocomplete-list'
      className={cn('p-1 data-empty:p-0', className)}
      {...props}
    />
  );
}

function AutocompleteEmpty({
  className,
  ...props
}: AutocompletePrimitive.Empty.Props) {
  return (
    <AutocompletePrimitive.Empty
      data-slot='autocomplete-empty'
      className={cn(
        'px-4 py-2 text-[0.925rem] leading-4 text-gray-600 empty:m-0 empty:p-0',
        className
      )}
      {...props}
    />
  );
}

function AutocompleteItem({
  className,
  ...props
}: AutocompletePrimitive.Item.Props) {
  return (
    <AutocompletePrimitive.Item
      data-slot='autocomplete-item'
      className={cn(
        "relative flex w-full cursor-default items-center gap-2.5 rounded-xl py-2 pr-8 pl-3 text-sm outline-hidden select-none data-disabled:pointer-events-none data-disabled:opacity-50 data-highlighted:bg-accent data-highlighted:text-accent-foreground not-data-[variant=destructive]:data-highlighted:**:text-accent-foreground [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    />
  );
}

function AutocompleteGroup({
  className,
  ...props
}: AutocompletePrimitive.Group.Props) {
  return (
    <AutocompletePrimitive.Group
      data-slot='autocomplete-group'
      className={cn('block pb-2', className)}
      {...props}
    />
  );
}

function AutocompleteGroupLabel({
  className,
  ...props
}: AutocompletePrimitive.GroupLabel.Props) {
  return (
    <AutocompletePrimitive.GroupLabel
      data-slot='autocomplete-group-label'
      className={cn(
        'sticky top-0 z-1 mt-0 mr-2 mb-0 ml-0 w-[calc(100%-0.5rem)] bg-[canvas] px-4 pt-2 pb-1 text-xs font-semibold tracking-wider uppercase',
        className
      )}
      {...props}
    />
  );
}

function AutocompleteCollection({
  ...props
}: AutocompletePrimitive.Collection.Props) {
  return (
    <AutocompletePrimitive.Collection
      data-slot='autocomplete-collection'
      {...props}
    />
  );
}

function AutocompleteStatus({
  className,
  ...props
}: AutocompletePrimitive.Status.Props) {
  return (
    <AutocompletePrimitive.Status
      data-slot='autocomplete-status'
      className={cn(
        'mt-1 px-4 py-2 text-sm leading-5 text-muted-foreground empty:m-0 empty:p-0',
        className
      )}
      {...props}
    />
  );
}

function AutocompleteClear({
  className,
  children,
  ...props
}: AutocompletePrimitive.Clear.Props) {
  return (
    <AutocompletePrimitive.Clear
      data-slot='autocomplete-clear'
      className={cn(className)}
      {...props}
    >
      {children ?? <XIcon className='h-4 w-4 text-muted-foreground' />}
    </AutocompletePrimitive.Clear>
  );
}

function AutocompleteRow({
  className,
  ...props
}: AutocompletePrimitive.Row.Props) {
  return (
    <AutocompletePrimitive.Row
      data-slot='autocomplete-row'
      className={cn(className)}
      {...props}
    />
  );
}

function AutocompleteTrigger({
  className,
  ...props
}: AutocompletePrimitive.Trigger.Props) {
  return (
    <AutocompletePrimitive.Trigger
      data-slot='autocomplete-trigger'
      className={cn(className)}
      render={<Button variant='outline' />}
      {...props}
    />
  );
}

export {
  Autocomplete,
  AutocompleteClear,
  AutocompleteCollection,
  AutocompleteEmpty,
  AutocompleteGroup,
  AutocompleteGroupLabel,
  AutocompleteInput,
  AutocompleteItem,
  AutocompleteList,
  AutocompletePopup,
  AutocompletePositioner,
  AutocompleteRow,
  AutocompleteStatus,
  AutocompleteTrigger,
};
```
---

## Avatar <a name="avatar"></a>

An easily stylable avatar component.

- **Registry Key**: `avatar`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/avatar.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `badge-icon`
```tsx
import {
  Avatar,
  AvatarBadge,
  AvatarFallback,
  AvatarImage,
} from '@/registry/default/ui/avatar';
import { PlusIcon } from 'lucide-react';

export function AvatarBadgeIcon() {
  return (
    <Avatar>
      <AvatarImage
        src='https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?w=128&h=128&dpr=2&q=80'
        alt='Avatar'
      />
      <AvatarFallback>MK</AvatarFallback>
      <AvatarBadge>
        <PlusIcon />
      </AvatarBadge>
    </Avatar>
  );
}
```

##### Example: `default`
```tsx
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from '@/registry/default/ui/avatar';

export function AvatarDemo() {
  return (
    <Avatar>
      <AvatarImage
        alt='Avatar image'
        src='https://images.unsplash.com/photo-1543610892-0b1f7e6d8ac1?w=128&h=128&dpr=2&q=80'
      />
      <AvatarFallback>AI</AvatarFallback>
    </Avatar>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/avatar.tsx`
```tsx
import { Avatar as AvatarPrimitive } from '@base-ui/react/avatar';

import { cn } from '@/lib/utils';

function Avatar({ className, ...props }: AvatarPrimitive.Root.Props) {
  return (
    <AvatarPrimitive.Root
      data-slot='avatar'
      className={cn(
        'group/avatar relative inline-flex size-12 items-center justify-center rounded-full bg-muted align-middle text-sm font-medium text-foreground select-none after:absolute after:top-0 after:right-0 after:bottom-0 after:left-0 after:rounded-full after:border after:border-border after:mix-blend-darken dark:after:mix-blend-lighten',
        className
      )}
      {...props}
    />
  );
}

function AvatarImage({ className, ...props }: AvatarPrimitive.Image.Props) {
  return (
    <AvatarPrimitive.Image
      data-slot='avatar-image'
      className={cn(
        'pointer-events-none aspect-square size-full rounded-full object-cover select-none',
        className
      )}
      {...props}
    />
  );
}

function AvatarFallback({
  className,
  ...props
}: AvatarPrimitive.Fallback.Props) {
  return (
    <AvatarPrimitive.Fallback
      data-slot='avatar-fallback'
      className={cn(
        'flex size-full items-center justify-center rounded-full bg-muted text-base text-muted-foreground',
        className
      )}
      {...props}
    />
  );
}

function AvatarBadge({ className, ...props }: React.ComponentProps<'span'>) {
  return (
    <span
      data-slot='avatar-badge'
      className={cn(
        'absolute right-0 bottom-0 z-10 inline-flex size-4 items-center justify-center rounded-full bg-primary text-primary-foreground ring-2 ring-background select-none [&>svg]:size-3',
        className
      )}
      {...props}
    />
  );
}

function AvatarGroup({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='avatar-group'
      className={cn(
        'group/avatar-group flex -space-x-2 *:data-[slot=avatar]:ring-2 *:data-[slot=avatar]:ring-background',
        className
      )}
      {...props}
    />
  );
}

function AvatarGroupCount({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='avatar-group-count'
      className={cn(
        'relative flex size-12 shrink-0 items-center justify-center rounded-full bg-muted text-sm text-muted-foreground ring-2 ring-background [&>svg]:size-3',
        className
      )}
      {...props}
    />
  );
}

export {
  Avatar,
  AvatarImage,
  AvatarFallback,
  AvatarBadge,
  AvatarGroup,
  AvatarGroupCount,
};
```
---

## Badge <a name="badge"></a>

Displays a badge or a component that looks like a badge.

- **Registry Key**: `badge`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/badge.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `class-variance-authority`

#### How to Use
##### Example: `default`
```tsx
import { Badge } from '@/registry/default/ui/badge';
import { BadgeCheckIcon } from 'lucide-react';

export function BadgeDemo() {
  return (
    <div className='flex flex-col items-center gap-2'>
      <div className='flex w-full flex-wrap gap-2'>
        <Badge>Badge</Badge>
        <Badge variant='secondary'>Secondary</Badge>
        <Badge variant='destructive'>Destructive</Badge>
        <Badge variant='outline'>Outline</Badge>
      </div>
      <div className='flex w-full flex-wrap gap-2'>
        <Badge
          variant='secondary'
          className='bg-blue-500 text-white dark:bg-blue-600'
        >
          <BadgeCheckIcon />
          Verified
        </Badge>
        <Badge className='h-5 min-w-5 rounded-full px-1 font-mono tabular-nums'>
          8
        </Badge>
        <Badge
          className='h-5 min-w-5 rounded-full px-1 font-mono tabular-nums'
          variant='destructive'
        >
          99
        </Badge>
        <Badge
          className='h-5 min-w-5 rounded-full px-1 font-mono tabular-nums'
          variant='outline'
        >
          20+
        </Badge>
      </div>
    </div>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/badge.tsx`
```tsx
import { mergeProps } from '@base-ui/react/merge-props';
import { useRender } from '@base-ui/react/use-render';
import { cva, type VariantProps } from 'class-variance-authority';

import { cn } from '@/lib/utils';

const badgeVariants = cva(
  'h-5 gap-1 rounded-4xl border border-transparent px-2 py-0.5 text-xs font-medium transition-all has-data-[icon=inline-end]:pr-1.5 has-data-[icon=inline-start]:pl-1.5 [&>svg]:size-3! inline-flex items-center justify-center w-fit whitespace-nowrap shrink-0 [&>svg]:pointer-events-none focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px] aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive transition-colors overflow-hidden group/badge',
  {
    variants: {
      variant: {
        default: 'bg-primary text-primary-foreground [a]:hover:bg-primary/80',
        secondary:
          'bg-secondary text-secondary-foreground [a]:hover:bg-secondary/80',
        destructive:
          'bg-destructive/20 [a]:hover:bg-destructive/20 focus-visible:ring-destructive/20 dark:focus-visible:ring-destructive/40 text-destructive dark:bg-destructive/20',
        outline:
          'border-border text-foreground [a]:hover:bg-muted [a]:hover:text-muted-foreground bg-background dark:bg-input/30 dark:border-input',
        ghost:
          'hover:bg-muted hover:text-muted-foreground dark:hover:bg-muted/50',
        link: 'text-primary underline-offset-4 hover:underline',
      },
    },
    defaultVariants: {
      variant: 'default',
    },
  }
);

function Badge({
  className,
  variant = 'default',
  render,
  ...props
}: useRender.ComponentProps<'span'> & VariantProps<typeof badgeVariants>) {
  return useRender({
    defaultTagName: 'span',
    props: mergeProps<'span'>(
      {
        className: cn(badgeVariants({ className, variant })),
      },
      props
    ),
    render,
    state: {
      slot: 'badge',
      variant,
    },
  });
}

export { Badge, badgeVariants };
```
---

## Button <a name="button"></a>

A button component that can be rendered as another tag or focusable when disabled.

- **Registry Key**: `button`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/button.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `class-variance-authority`

#### How to Use
##### Example: `default`
```tsx
import { Button } from '@/registry/default/ui/button';

export function ButtonDefault() {
  return <Button>Button</Button>;
}
```

##### Example: `ghost`
```tsx
import { Button } from '@/registry/default/ui/button';

export function ButtonGhost() {
  return <Button variant='ghost'>Ghost</Button>;
}
```

#### Component Source Code
##### File: `registry/default/ui/button.tsx`
```tsx
'use client';

import { Button as ButtonPrimitive } from '@base-ui/react/button';
import { cva, type VariantProps } from 'class-variance-authority';

import { cn } from '@/lib/utils';

const buttonVariants = cva(
  "focus-visible:border-ring focus-visible:ring-ring/50 aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive dark:aria-invalid:border-destructive/50 rounded-4xl border border-transparent bg-clip-padding text-sm font-medium focus-visible:ring-[3px] aria-invalid:ring-[3px] [&_svg:not([class*='size-'])]:size-4 inline-flex items-center justify-center whitespace-nowrap transition-all disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none shrink-0 [&_svg]:shrink-0 outline-none group/button select-none",
  {
    variants: {
      variant: {
        default: 'bg-primary text-primary-foreground hover:bg-primary/80',
        outline:
          'border-border bg-background hover:bg-accent hover:text-accent-foreground aria-expanded:bg-muted aria-expanded:text-foreground dark:bg-input/30 dark:border-input dark:hover:bg-input/50',
        secondary:
          'bg-secondary text-secondary-foreground hover:bg-secondary/80 aria-expanded:bg-secondary aria-expanded:text-secondary-foreground',
        ghost:
          'hover:bg-muted hover:text-foreground dark:hover:bg-muted/50 aria-expanded:bg-muted aria-expanded:text-foreground',
        destructive:
          'bg-destructive/10 hover:bg-destructive/20 focus-visible:ring-destructive/20 dark:focus-visible:ring-destructive/40 dark:bg-destructive/20 text-destructive focus-visible:border-destructive/40 dark:hover:bg-destructive/30',
        link: 'text-primary underline-offset-4 hover:underline',
      },
      size: {
        default:
          'h-9 gap-1.5 px-3 has-data-[icon=inline-end]:pr-2.5 has-data-[icon=inline-start]:pl-2.5',
        xs: "h-6 gap-1 px-2.5 text-xs has-data-[icon=inline-end]:pr-2 has-data-[icon=inline-start]:pl-2 [&_svg:not([class*='size-'])]:size-3",
        sm: 'h-8 gap-1 px-3 has-data-[icon=inline-end]:pr-2 has-data-[icon=inline-start]:pl-2',
        lg: 'h-10 gap-1.5 px-4 has-data-[icon=inline-end]:pr-3 has-data-[icon=inline-start]:pl-3',
        icon: 'size-9',
        'icon-xs': "size-6 [&_svg:not([class*='size-'])]:size-3",
        'icon-sm': 'size-8',
        'icon-lg': 'size-10',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  }
);

function Button({
  className,
  variant = 'default',
  size = 'default',
  ...props
}: ButtonPrimitive.Props & VariantProps<typeof buttonVariants>) {
  return (
    <ButtonPrimitive
      data-slot='button'
      className={cn(buttonVariants({ variant, size, className }))}
      {...props}
    />
  );
}

export { Button, buttonVariants };
```
---

## Card <a name="card"></a>

Displays a card with header, content, and footer.

- **Registry Key**: `card`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/card.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import {
  AlertDialog,
  AlertDialogAction,
  AlertDialogCancel,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogHeader,
  AlertDialogMedia,
  AlertDialogTitle,
  AlertDialogTrigger,
} from '@/registry/default/ui/alert-dialog';
import { Badge } from '@/registry/default/ui/badge';
import { Button } from '@/registry/default/ui/button';
import {
  Card,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/registry/default/ui/card';
import { BluetoothIcon, PlusIcon } from 'lucide-react';

export function CardDemo() {
  return (
    <Card className='relative w-full max-w-sm overflow-hidden pt-0'>
      <div className='absolute inset-0 z-30 aspect-video bg-primary opacity-50 mix-blend-color' />
      <img
        src='https://images.unsplash.com/photo-1604076850742-4c7221f3101b?q=80&w=1887&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
        alt='Photo by mymind on Unsplash'
        title='Photo by mymind on Unsplash'
        className='relative z-20 my-0! aspect-video w-full object-cover brightness-60 grayscale'
      />
      <CardHeader>
        <CardTitle>Observability Plus is replacing Monitoring</CardTitle>
        <CardDescription>
          Switch to the improved way to explore your data, with natural
          language. Monitoring will no longer be available on the Pro plan in
          November, 2025
        </CardDescription>
      </CardHeader>
      <CardFooter>
        <AlertDialog>
          <AlertDialogTrigger render={<Button />}>
            <PlusIcon data-icon='inline-start' />
            Show Dialog
          </AlertDialogTrigger>
          <AlertDialogContent size='sm'>
            <AlertDialogHeader>
              <AlertDialogMedia>
                <BluetoothIcon />
              </AlertDialogMedia>
              <AlertDialogTitle>Allow accessory to connect?</AlertDialogTitle>
              <AlertDialogDescription>
                Do you want to allow the USB accessory to connect to this
                device?
              </AlertDialogDescription>
            </AlertDialogHeader>
            <AlertDialogFooter>
              <AlertDialogCancel>Don&apos;t allow</AlertDialogCancel>
              <AlertDialogAction>Allow</AlertDialogAction>
            </AlertDialogFooter>
          </AlertDialogContent>
        </AlertDialog>
        <Badge variant='secondary' className='ml-auto'>
          Warning
        </Badge>
      </CardFooter>
    </Card>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/card.tsx`
```tsx
import * as React from 'react';

import { cn } from '@/lib/utils';

function Card({
  className,
  size = 'default',
  ...props
}: React.ComponentProps<'div'> & { size?: 'default' | 'sm' }) {
  return (
    <div
      data-slot='card'
      data-size={size}
      className={cn(
        'group/card flex flex-col gap-6 overflow-hidden rounded-2xl bg-card py-6 text-sm text-card-foreground ring-1 ring-foreground/10 has-[>img:first-child]:pt-0 data-[size=sm]:gap-4 data-[size=sm]:py-4 *:[img:first-child]:rounded-t-xl *:[img:last-child]:rounded-b-xl',
        className
      )}
      {...props}
    />
  );
}

function CardHeader({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='card-header'
      className={cn(
        'group/card-header @container/card-header grid auto-rows-min items-start gap-2 rounded-t-xl px-6 group-data-[size=sm]/card:px-4 has-data-[slot=card-action]:grid-cols-[1fr_auto] has-data-[slot=card-description]:grid-rows-[auto_auto] [.border-b]:pb-6 group-data-[size=sm]/card:[.border-b]:pb-4',
        className
      )}
      {...props}
    />
  );
}

function CardTitle({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='card-title'
      className={cn('text-base font-medium', className)}
      {...props}
    />
  );
}

function CardDescription({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='card-description'
      className={cn('text-sm text-muted-foreground', className)}
      {...props}
    />
  );
}

function CardAction({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='card-action'
      className={cn(
        'col-start-2 row-span-2 row-start-1 self-start justify-self-end',
        className
      )}
      {...props}
    />
  );
}

function CardContent({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='card-content'
      className={cn('px-6 group-data-[size=sm]/card:px-4', className)}
      {...props}
    />
  );
}

function CardFooter({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='card-footer'
      className={cn(
        'flex items-center rounded-b-xl px-6 group-data-[size=sm]/card:px-4 [.border-t]:pt-6 group-data-[size=sm]/card:[.border-t]:pt-4',
        className
      )}
      {...props}
    />
  );
}

export {
  Card,
  CardHeader,
  CardFooter,
  CardTitle,
  CardAction,
  CardDescription,
  CardContent,
};
```
---

## Checkbox <a name="checkbox"></a>

A control that allows the user to toggle between checked and not checked.

- **Registry Key**: `checkbox`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/checkbox.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `lucide-react`

#### How to Use
##### Example: `checked`
```tsx
import { Checkbox } from '@/registry/default/ui/checkbox';

export function CheckboxChecked() {
  return <Checkbox defaultChecked />;
}
```

##### Example: `default`
```tsx
import { Checkbox } from '@/registry/default/ui/checkbox';

export function CheckboxDefault() {
  return <Checkbox />;
}
```

#### Component Source Code
##### File: `registry/default/ui/checkbox.tsx`
```tsx
'use client';

import { Checkbox as CheckboxPrimitive } from '@base-ui/react/checkbox';
import { CheckIcon } from 'lucide-react';

import { cn } from '@/lib/utils';

function Checkbox({ className, ...props }: CheckboxPrimitive.Root.Props) {
  return (
    <CheckboxPrimitive.Root
      data-slot='checkbox'
      className={cn(
        'peer relative flex size-4 shrink-0 items-center justify-center rounded-lg border border-input transition-colors outline-none group-has-disabled/field:opacity-50 after:absolute after:-inset-x-3 after:-inset-y-2 focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50 disabled:cursor-not-allowed disabled:opacity-50 aria-invalid:border-destructive aria-invalid:ring-[3px] aria-invalid:ring-destructive/20 aria-invalid:aria-checked:border-primary data-checked:border-primary data-checked:bg-primary data-checked:text-primary-foreground dark:bg-input/30 dark:aria-invalid:border-destructive/50 dark:aria-invalid:ring-destructive/40 dark:data-checked:bg-primary',
        className
      )}
      {...props}
    >
      <CheckboxPrimitive.Indicator
        data-slot='checkbox-indicator'
        className='grid place-content-center text-current transition-none [&>svg]:size-3.5'
      >
        <CheckIcon />
      </CheckboxPrimitive.Indicator>
    </CheckboxPrimitive.Root>
  );
}

export { Checkbox };
```
---

## Checkbox Group <a name="checkbox-group"></a>

A group of checkboxes that can be controlled together.

- **Registry Key**: `checkbox-group`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/checkbox-group.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
import { Checkbox } from '@/registry/default/ui/checkbox';
import { CheckboxGroup } from '@/registry/default/ui/checkbox-group';
import { Label } from '@/registry/default/ui/label';

export function CheckboxGroupDefault() {
  return (
    <CheckboxGroup defaultValue={['updates']}>
      <Label>
        <Checkbox name='notifications' value='updates' />
        Product updates
      </Label>
      <Label>
        <Checkbox name='notifications' value='news' />
        Company news
      </Label>
      <Label>
        <Checkbox name='notifications' value='offers' />
        Special offers
      </Label>
    </CheckboxGroup>
  );
}
```

##### Example: `with-field`
```tsx
import { Checkbox } from '@/registry/default/ui/checkbox';
import { CheckboxGroup } from '@/registry/default/ui/checkbox-group';
import {
  Field,
  FieldDescription,
  FieldLabel,
} from '@/registry/default/ui/field';
import { Label } from '@/registry/default/ui/label';

export function CheckboxGroupWithField() {
  return (
    <div>
      <Field>
        <FieldLabel>Email Preferences</FieldLabel>
        <FieldDescription>
          Choose which emails you&apos;d like to receive.
        </FieldDescription>
        <CheckboxGroup className='mt-2'>
          <Label>
            <Checkbox name='emails' value='marketing' />
            Marketing emails
          </Label>
          <Label>
            <Checkbox name='emails' value='social' />
            Social notifications
          </Label>
          <Label>
            <Checkbox name='emails' value='security' />
            Security alerts
          </Label>
        </CheckboxGroup>
      </Field>
    </div>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/checkbox-group.tsx`
```tsx
import { CheckboxGroup as CheckboxGroupPrimitive } from '@base-ui/react/checkbox-group';

import { cn } from '@/lib/utils';

function CheckboxGroup({ className, ...props }: CheckboxGroupPrimitive.Props) {
  return (
    <CheckboxGroupPrimitive
      className={cn('flex flex-col items-start gap-1', className)}
      {...props}
    />
  );
}

export { CheckboxGroup };
```
---

## Collapsible <a name="collapsible"></a>

An interactive component that expands and collapses content.

- **Registry Key**: `collapsible`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/collapsible.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from '@/registry/default/ui/collapsible';
import { ChevronDownIcon } from 'lucide-react';

export function CollapsibleDefault() {
  return (
    <Collapsible className='w-64'>
      <CollapsibleTrigger className='flex items-center gap-2'>
        Show more
        <ChevronDownIcon className='size-4 transition-transform duration-200 in-data-panel-open:rotate-180' />
      </CollapsibleTrigger>
      <CollapsibleContent>
        <p className='text-sm text-muted-foreground'>
          The chevron icon rotates when the panel is open.
        </p>
      </CollapsibleContent>
    </Collapsible>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/collapsible.tsx`
```tsx
'use client';

import { Collapsible as CollapsiblePrimitive } from '@base-ui/react/collapsible';

function Collapsible({ ...props }: CollapsiblePrimitive.Root.Props) {
  return <CollapsiblePrimitive.Root data-slot='collapsible' {...props} />;
}

function CollapsibleTrigger({ ...props }: CollapsiblePrimitive.Trigger.Props) {
  return (
    <CollapsiblePrimitive.Trigger data-slot='collapsible-trigger' {...props} />
  );
}

function CollapsibleContent({ ...props }: CollapsiblePrimitive.Panel.Props) {
  return (
    <CollapsiblePrimitive.Panel data-slot='collapsible-content' {...props} />
  );
}

export { Collapsible, CollapsibleTrigger, CollapsibleContent };
```
---

## Combobox <a name="combobox"></a>

A searchable dropdown that allows users to filter and select from a list of options.

- **Registry Key**: `combobox`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/combobox.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `lucide-react`

#### How to Use
##### Example: `auto-highlight`
```tsx
'use client';

import {
  Combobox,
  ComboboxContent,
  ComboboxEmpty,
  ComboboxInput,
  ComboboxItem,
  ComboboxList,
} from '@/registry/default/ui/combobox';

const frameworks = ['React', 'Vue', 'Angular', 'Svelte', 'Next.js', 'Nuxt'];

export function ComboboxAutoHighlight() {
  return (
    <Combobox items={frameworks} autoHighlight>
      <ComboboxInput placeholder='Select a framework...' />
      <ComboboxContent>
        <ComboboxEmpty>No frameworks found.</ComboboxEmpty>
        <ComboboxList>
          {(item) => (
            <ComboboxItem key={item} value={item}>
              {item}
            </ComboboxItem>
          )}
        </ComboboxList>
      </ComboboxContent>
    </Combobox>
  );
}
```

##### Example: `default`
```tsx
'use client';

import {
  Combobox,
  ComboboxContent,
  ComboboxEmpty,
  ComboboxInput,
  ComboboxItem,
  ComboboxList,
} from '@/registry/default/ui/combobox';

const frameworks = ['React', 'Vue', 'Angular', 'Svelte', 'Next.js', 'Nuxt'];

export function ComboboxDefault() {
  return (
    <Combobox items={frameworks}>
      <ComboboxInput placeholder='Select a framework...' />
      <ComboboxContent>
        <ComboboxEmpty>No frameworks found.</ComboboxEmpty>
        <ComboboxList>
          {(item) => (
            <ComboboxItem key={item} value={item}>
              {item}
            </ComboboxItem>
          )}
        </ComboboxList>
      </ComboboxContent>
    </Combobox>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/combobox.tsx`
```tsx
'use client';

import * as React from 'react';

import { Button } from '@/registry/default/ui/button';
import {
  InputGroup,
  InputGroupAddon,
  InputGroupButton,
  InputGroupInput,
} from '@/registry/default/ui/input-group';
import { Combobox as ComboboxPrimitive } from '@base-ui/react/combobox';
import { CheckIcon, ChevronDownIcon, XIcon } from 'lucide-react';

import { cn } from '@/lib/utils';

const Combobox = ComboboxPrimitive.Root;

function ComboboxValue({ ...props }: ComboboxPrimitive.Value.Props) {
  return <ComboboxPrimitive.Value data-slot='combobox-value' {...props} />;
}

function ComboboxTrigger({
  className,
  children,
  ...props
}: ComboboxPrimitive.Trigger.Props) {
  return (
    <ComboboxPrimitive.Trigger
      data-slot='combobox-trigger'
      className={cn("[&_svg:not([class*='size-'])]:size-4", className)}
      {...props}
    >
      {children}
      <ChevronDownIcon className='pointer-events-none size-4 text-muted-foreground' />
    </ComboboxPrimitive.Trigger>
  );
}

function ComboboxClear({ className, ...props }: ComboboxPrimitive.Clear.Props) {
  return (
    <ComboboxPrimitive.Clear
      data-slot='combobox-clear'
      className={cn(className)}
      {...props}
      render={
        <InputGroupButton variant='ghost' size='icon-xs'>
          <XIcon className='pointer-events-none' />
        </InputGroupButton>
      }
    />
  );
}

function ComboboxInput({
  className,
  children,
  disabled = false,
  showTrigger = true,
  showClear = false,
  ...props
}: ComboboxPrimitive.Input.Props & {
  showTrigger?: boolean;
  showClear?: boolean;
}) {
  return (
    <InputGroup className={cn('w-auto', className)}>
      <ComboboxPrimitive.Input
        render={<InputGroupInput disabled={disabled} />}
        {...props}
      />
      <InputGroupAddon align='inline-end'>
        {showTrigger && (
          <InputGroupButton
            size='icon-xs'
            variant='ghost'
            render={<ComboboxTrigger />}
            data-slot='input-group-button'
            className='group-has-data-[slot=combobox-clear]/input-group:hidden data-pressed:bg-transparent'
            disabled={disabled}
          />
        )}
        {showClear && <ComboboxClear disabled={disabled} />}
      </InputGroupAddon>
      {children}
    </InputGroup>
  );
}

function ComboboxContent({
  className,
  side = 'bottom',
  sideOffset = 6,
  align = 'start',
  alignOffset = 0,
  anchor,
  ...props
}: ComboboxPrimitive.Popup.Props &
  Pick<
    ComboboxPrimitive.Positioner.Props,
    'side' | 'align' | 'sideOffset' | 'alignOffset' | 'anchor'
  >) {
  return (
    <ComboboxPrimitive.Portal>
      <ComboboxPrimitive.Positioner
        side={side}
        sideOffset={sideOffset}
        align={align}
        alignOffset={alignOffset}
        anchor={anchor}
        className='isolate z-50'
      >
        <ComboboxPrimitive.Popup
          data-slot='combobox-content'
          data-chips={!!anchor}
          className={cn(
            'group/combobox-content relative max-h-(--available-height) w-(--anchor-width) max-w-(--available-width) min-w-[calc(var(--anchor-width)+--spacing(7))] origin-(--transform-origin) overflow-hidden rounded-lg bg-popover text-popover-foreground shadow-md ring-1 ring-foreground/10 duration-100 data-closed:animate-out data-closed:fade-out-0 data-closed:zoom-out-95 data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 data-[chips=true]:min-w-(--anchor-width) data-[side=bottom]:slide-in-from-top-2 data-[side=inline-end]:slide-in-from-left-2 data-[side=inline-start]:slide-in-from-right-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 *:data-[slot=input-group]:m-1 *:data-[slot=input-group]:mb-0 *:data-[slot=input-group]:h-8 *:data-[slot=input-group]:border-input/30 *:data-[slot=input-group]:bg-input/30 *:data-[slot=input-group]:shadow-none',
            className
          )}
          {...props}
        />
      </ComboboxPrimitive.Positioner>
    </ComboboxPrimitive.Portal>
  );
}

function ComboboxList({ className, ...props }: ComboboxPrimitive.List.Props) {
  return (
    <ComboboxPrimitive.List
      data-slot='combobox-list'
      className={cn(
        'no-scrollbar max-h-[min(calc(--spacing(72)---spacing(9)),calc(var(--available-height)---spacing(9)))] scroll-py-1 overflow-y-auto overscroll-contain p-1 data-empty:p-0',
        className
      )}
      {...props}
    />
  );
}

function ComboboxItem({
  className,
  children,
  ...props
}: ComboboxPrimitive.Item.Props) {
  return (
    <ComboboxPrimitive.Item
      data-slot='combobox-item'
      className={cn(
        "relative flex w-full cursor-default items-center gap-2 rounded-md py-1 pr-8 pl-1.5 text-sm outline-hidden select-none data-disabled:pointer-events-none data-disabled:opacity-50 data-highlighted:bg-accent data-highlighted:text-accent-foreground not-data-[variant=destructive]:data-highlighted:**:text-accent-foreground [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    >
      {children}
      <ComboboxPrimitive.ItemIndicator
        render={
          <span className='pointer-events-none absolute right-2 flex size-4 items-center justify-center'>
            <CheckIcon className='pointer-events-none' />
          </span>
        }
      />
    </ComboboxPrimitive.Item>
  );
}

function ComboboxGroup({ className, ...props }: ComboboxPrimitive.Group.Props) {
  return (
    <ComboboxPrimitive.Group
      data-slot='combobox-group'
      className={cn(className)}
      {...props}
    />
  );
}

function ComboboxLabel({
  className,
  ...props
}: ComboboxPrimitive.GroupLabel.Props) {
  return (
    <ComboboxPrimitive.GroupLabel
      data-slot='combobox-label'
      className={cn('px-2 py-1.5 text-xs text-muted-foreground', className)}
      {...props}
    />
  );
}

function ComboboxCollection({ ...props }: ComboboxPrimitive.Collection.Props) {
  return (
    <ComboboxPrimitive.Collection data-slot='combobox-collection' {...props} />
  );
}

function ComboboxEmpty({ className, ...props }: ComboboxPrimitive.Empty.Props) {
  return (
    <ComboboxPrimitive.Empty
      data-slot='combobox-empty'
      className={cn(
        'hidden w-full justify-center py-2 text-center text-sm text-muted-foreground group-data-empty/combobox-content:flex',
        className
      )}
      {...props}
    />
  );
}

function ComboboxSeparator({
  className,
  ...props
}: ComboboxPrimitive.Separator.Props) {
  return (
    <ComboboxPrimitive.Separator
      data-slot='combobox-separator'
      className={cn('-mx-1 my-1 h-px bg-border', className)}
      {...props}
    />
  );
}

function ComboboxChips({
  className,
  ...props
}: React.ComponentPropsWithRef<typeof ComboboxPrimitive.Chips> &
  ComboboxPrimitive.Chips.Props) {
  return (
    <ComboboxPrimitive.Chips
      data-slot='combobox-chips'
      className={cn(
        'flex min-h-8 flex-wrap items-center gap-1 rounded-xl border border-input bg-transparent bg-clip-padding px-2.5 py-1 text-sm transition-colors focus-within:border-ring focus-within:ring-[3px] focus-within:ring-ring/50 has-aria-invalid:border-destructive has-aria-invalid:ring-[3px] has-aria-invalid:ring-destructive/20 has-data-[slot=combobox-chip]:px-1 dark:bg-input/30 dark:has-aria-invalid:border-destructive/50 dark:has-aria-invalid:ring-destructive/40',
        className
      )}
      {...props}
    />
  );
}

function ComboboxChip({
  className,
  children,
  showRemove = true,
  ...props
}: ComboboxPrimitive.Chip.Props & {
  showRemove?: boolean;
}) {
  return (
    <ComboboxPrimitive.Chip
      data-slot='combobox-chip'
      className={cn(
        'flex h-[calc(--spacing(5.25))] w-fit items-center justify-center gap-1 rounded-md bg-muted px-1.5 text-xs font-medium whitespace-nowrap text-foreground has-disabled:pointer-events-none has-disabled:cursor-not-allowed has-disabled:opacity-50 has-data-[slot=combobox-chip-remove]:pr-0',
        className
      )}
      {...props}
    >
      {children}
      {showRemove && (
        <ComboboxPrimitive.ChipRemove
          className='-ml-1 opacity-50 hover:opacity-100'
          data-slot='combobox-chip-remove'
          render={
            <Button variant='ghost' size='icon-xs'>
              <XIcon className='pointer-events-none' />
            </Button>
          }
        />
      )}
    </ComboboxPrimitive.Chip>
  );
}

function ComboboxChipsInput({
  className,
  ...props
}: ComboboxPrimitive.Input.Props) {
  return (
    <ComboboxPrimitive.Input
      data-slot='combobox-chip-input'
      className={cn('min-w-16 flex-1 outline-none', className)}
      {...props}
    />
  );
}

function useComboboxAnchor() {
  return React.useRef<HTMLDivElement | null>(null);
}

export {
  Combobox,
  ComboboxInput,
  ComboboxContent,
  ComboboxList,
  ComboboxItem,
  ComboboxGroup,
  ComboboxLabel,
  ComboboxCollection,
  ComboboxEmpty,
  ComboboxSeparator,
  ComboboxChips,
  ComboboxChip,
  ComboboxChipsInput,
  ComboboxTrigger,
  ComboboxValue,
  useComboboxAnchor,
};
```
---

## Context Menu <a name="context-menu"></a>

A menu that appears on right-click, providing contextual actions for the target element.

- **Registry Key**: `context-menu`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/context-menu.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `lucide-react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import {
  ContextMenu,
  ContextMenuItem,
  ContextMenuPopup,
  ContextMenuSeparator,
  ContextMenuTrigger,
} from '@/registry/default/ui/context-menu';

export function ContextMenuDefault() {
  return (
    <ContextMenu>
      <ContextMenuTrigger className='flex h-32 w-64 items-center justify-center rounded-md border border-dashed'>
        Right click here
      </ContextMenuTrigger>
      <ContextMenuPopup>
        <ContextMenuItem>Back</ContextMenuItem>
        <ContextMenuItem>Forward</ContextMenuItem>
        <ContextMenuItem>Reload</ContextMenuItem>
        <ContextMenuSeparator />
        <ContextMenuItem>Save As...</ContextMenuItem>
        <ContextMenuItem>Print...</ContextMenuItem>
      </ContextMenuPopup>
    </ContextMenu>
  );
}
```

##### Example: `with-submenu`
```tsx
'use client';

import {
  ContextMenu,
  ContextMenuItem,
  ContextMenuPopup,
  ContextMenuSeparator,
  ContextMenuSub,
  ContextMenuSubPopup,
  ContextMenuSubTrigger,
  ContextMenuTrigger,
} from '@/registry/default/ui/context-menu';

export function ContextMenuWithSubmenu() {
  return (
    <ContextMenu>
      <ContextMenuTrigger className='flex h-32 w-64 items-center justify-center rounded-md border border-dashed'>
        Right click here
      </ContextMenuTrigger>
      <ContextMenuPopup>
        <ContextMenuItem>New Tab</ContextMenuItem>
        <ContextMenuItem>New Window</ContextMenuItem>
        <ContextMenuSeparator />
        <ContextMenuSub>
          <ContextMenuSubTrigger>More Tools</ContextMenuSubTrigger>
          <ContextMenuSubPopup>
            <ContextMenuItem>Save Page As...</ContextMenuItem>
            <ContextMenuItem>Create Shortcut...</ContextMenuItem>
            <ContextMenuItem>Developer Tools</ContextMenuItem>
          </ContextMenuSubPopup>
        </ContextMenuSub>
        <ContextMenuSeparator />
        <ContextMenuItem>Settings</ContextMenuItem>
      </ContextMenuPopup>
    </ContextMenu>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/context-menu.tsx`
```tsx
'use client';

import { ContextMenu as ContextMenuPrimitive } from '@base-ui/react/context-menu';
import { CheckIcon, ChevronRightIcon, CircleIcon } from 'lucide-react';

import { cn } from '@/lib/utils';

const ContextMenu = ContextMenuPrimitive.Root;

function ContextMenuTrigger({
  className,
  ...props
}: ContextMenuPrimitive.Trigger.Props) {
  return (
    <ContextMenuPrimitive.Trigger
      data-slot='context-menu-trigger'
      className={cn(
        "flex cursor-default items-center rounded-sm px-2 py-1.5 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground data-inset:pl-8 data-[state=open]:bg-accent data-[state=open]:text-accent-foreground [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    />
  );
}

function ContextMenuPopup({
  align = 'start',
  sideOffset = 4,
  alignOffset = 0,
  className,
  ...props
}: ContextMenuPrimitive.Popup.Props & {
  align?: ContextMenuPrimitive.Positioner.Props['align'];
  sideOffset?: ContextMenuPrimitive.Positioner.Props['sideOffset'];
  alignOffset?: ContextMenuPrimitive.Positioner.Props['alignOffset'];
}) {
  return (
    <ContextMenuPrimitive.Portal>
      <ContextMenuPrimitive.Positioner
        data-slot='context-menu-positioner'
        className='outline-none'
        alignOffset={alignOffset}
        align={align}
        sideOffset={sideOffset}
      >
        <ContextMenuPrimitive.Popup
          data-slot='context-menu-popup'
          className={cn(
            'origin-(--transform-origin) rounded-md bg-popover py-1 text-popover-foreground shadow-sm transition-[transform,scale,opacity] duration-100 outline-none data-ending-style:scale-90 data-ending-style:opacity-0 data-starting-style:scale-90 data-starting-style:opacity-0 dark:shadow-none',
            className
          )}
          {...props}
        />
      </ContextMenuPrimitive.Positioner>
    </ContextMenuPrimitive.Portal>
  );
}

function ContextMenuGroup({ ...props }: ContextMenuPrimitive.Group.Props) {
  return (
    <ContextMenuPrimitive.Group data-slot='context-menu-group' {...props} />
  );
}

function ContextMenuItem({
  className,
  ...props
}: ContextMenuPrimitive.Item.Props) {
  return (
    <ContextMenuPrimitive.Item
      data-slot='context-menu-item'
      className={cn(
        'flex cursor-pointer px-4 py-2 text-sm leading-4 outline-none select-none data-highlighted:relative data-highlighted:z-0 data-highlighted:before:absolute data-highlighted:before:inset-x-1 data-highlighted:before:inset-y-0 data-highlighted:before:z-[-1] data-highlighted:before:rounded-sm data-highlighted:before:bg-accent',
        className
      )}
      {...props}
    />
  );
}

function ContextMenuCheckboxItem({
  className,
  children,
  checked,
  ...props
}: ContextMenuPrimitive.CheckboxItem.Props) {
  return (
    <ContextMenuPrimitive.CheckboxItem
      data-slot='context-menu-checkbox-item'
      className={cn(
        'grid cursor-pointer grid-cols-[0.75rem_1fr] items-center gap-2 py-2 pr-8 pl-2.5 text-sm leading-4 outline-none select-none data-highlighted:relative data-highlighted:z-0 data-highlighted:before:absolute data-highlighted:before:inset-x-1 data-highlighted:before:inset-y-0 data-highlighted:before:z-[-1] data-highlighted:before:rounded-sm data-highlighted:before:bg-accent',
        className
      )}
      checked={checked}
      {...props}
    >
      <ContextMenuPrimitive.CheckboxItemIndicator className="col-start-1 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4">
        <CheckIcon />
      </ContextMenuPrimitive.CheckboxItemIndicator>
      <span className='col-start-2'>{children}</span>
    </ContextMenuPrimitive.CheckboxItem>
  );
}

function ContextMenuRadioGroup({
  ...props
}: ContextMenuPrimitive.RadioGroup.Props) {
  return (
    <ContextMenuPrimitive.RadioGroup
      data-slot='context-menu-radio-group'
      {...props}
    />
  );
}

function ContextMenuRadioItem({
  className,
  children,
  ...props
}: ContextMenuPrimitive.RadioItem.Props) {
  return (
    <ContextMenuPrimitive.RadioItem
      data-slot='context-menu-radio-item'
      className={cn(
        'grid cursor-pointer grid-cols-[0.75rem_1fr] items-center gap-2 py-2 pr-8 pl-2.5 text-sm leading-4 outline-none select-none data-highlighted:relative data-highlighted:z-0 data-highlighted:before:absolute data-highlighted:before:inset-x-1 data-highlighted:before:inset-y-0 data-highlighted:before:z-[-1] data-highlighted:before:rounded-sm data-highlighted:before:bg-accent',
        className
      )}
      {...props}
    >
      <ContextMenuPrimitive.RadioItemIndicator className="col-start-1 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-2.5">
        <CircleIcon className='fill-current' />
      </ContextMenuPrimitive.RadioItemIndicator>
      <span className='col-start-2'>{children}</span>
    </ContextMenuPrimitive.RadioItem>
  );
}

function ContextMenuGroupLabel({
  className,
  ...props
}: ContextMenuPrimitive.GroupLabel.Props) {
  return (
    <ContextMenuPrimitive.GroupLabel
      data-slot='context-menu-label'
      className={cn(
        'px-2 py-2 pr-8 pl-7.5 text-sm leading-4 font-medium text-muted-foreground select-none',
        className
      )}
      {...props}
    />
  );
}

function ContextMenuSeparator({
  className,
  ...props
}: ContextMenuPrimitive.Separator.Props) {
  return (
    <ContextMenuPrimitive.Separator
      data-slot='context-menu-separator'
      className={cn('mx-2 my-1 h-px bg-border', className)}
      {...props}
    />
  );
}

function ContextMenuShortcut({
  className,
  ...props
}: React.ComponentProps<'span'>) {
  return (
    <span
      data-slot='context-menu-shortcut'
      className={cn(
        'ms-auto text-xs tracking-widest text-muted-foreground/60',
        className
      )}
      {...props}
    />
  );
}

function ContextMenuSub({ ...props }: ContextMenuPrimitive.SubmenuRoot.Props) {
  return (
    <ContextMenuPrimitive.SubmenuRoot data-slot='context-menu-sub' {...props} />
  );
}

function ContextMenuSubTrigger({
  className,
  children,
  ...props
}: ContextMenuPrimitive.SubmenuTrigger.Props) {
  return (
    <ContextMenuPrimitive.SubmenuTrigger
      data-slot='context-menu-sub-trigger'
      className={cn(
        "flex cursor-default py-2 pr-2 pl-4 text-sm leading-4 outline-none select-none data-highlighted:relative data-highlighted:z-0 data-highlighted:before:absolute data-highlighted:before:inset-x-1 data-highlighted:before:inset-y-0 data-highlighted:before:z-[-1] data-highlighted:before:rounded-sm data-highlighted:before:bg-accent [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    >
      {children}
      <ChevronRightIcon className='ms-auto' />
    </ContextMenuPrimitive.SubmenuTrigger>
  );
}

function ContextMenuSubPopup({
  align = 'start',
  sideOffset = 1,
  alignOffset = -4,
  className,
  ...props
}: ContextMenuPrimitive.Popup.Props & {
  align?: ContextMenuPrimitive.Positioner.Props['align'];
  sideOffset?: ContextMenuPrimitive.Positioner.Props['sideOffset'];
  alignOffset?: ContextMenuPrimitive.Positioner.Props['alignOffset'];
}) {
  return (
    <ContextMenuPopup
      className={className}
      data-slot='context-menu-sub-content'
      align={align}
      sideOffset={sideOffset}
      alignOffset={alignOffset}
      {...props}
    />
  );
}

export {
  ContextMenu,
  ContextMenuTrigger,
  ContextMenuPopup,
  ContextMenuGroup,
  ContextMenuItem,
  ContextMenuCheckboxItem,
  ContextMenuRadioGroup,
  ContextMenuRadioItem,
  ContextMenuGroupLabel,
  ContextMenuSeparator,
  ContextMenuShortcut,
  ContextMenuSub,
  ContextMenuSubTrigger,
  ContextMenuSubPopup,
};
```
---

## Dialog <a name="dialog"></a>

A modal window that appears on top of the main content, requiring user interaction.

- **Registry Key**: `dialog`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/dialog.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `lucide-react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Button } from '@/registry/default/ui/button';
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from '@/registry/default/ui/dialog';

export function DialogDefault() {
  return (
    <Dialog>
      <DialogTrigger render={<Button variant='outline' />}>
        Open Dialog
      </DialogTrigger>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>Edit Profile</DialogTitle>
          <DialogDescription>
            Make changes to your profile here. Click save when you&apos;re done.
          </DialogDescription>
        </DialogHeader>
        <div className='py-4'>
          <p className='text-sm text-muted-foreground'>
            Your profile settings will be updated.
          </p>
        </div>
        <DialogFooter>
          <Button type='submit'>Save changes</Button>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}
```

##### Example: `no-close-button`
```tsx
'use client';

import { Button } from '@/registry/default/ui/button';
import {
  Dialog,
  DialogClose,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from '@/registry/default/ui/dialog';

export function DialogNoCloseButton() {
  return (
    <Dialog>
      <DialogTrigger render={<Button variant='outline' />}>
        Open Dialog
      </DialogTrigger>
      <DialogContent showCloseButton={false}>
        <DialogHeader>
          <DialogTitle>Confirm Action</DialogTitle>
          <DialogDescription>
            Are you sure you want to proceed with this action?
          </DialogDescription>
        </DialogHeader>
        <DialogFooter className='mt-4'>
          <DialogClose render={<Button variant='outline' />}>
            Cancel
          </DialogClose>
          <Button>Confirm</Button>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/dialog.tsx`
```tsx
'use client';

import * as React from 'react';

import { Button } from '@/registry/default/ui/button';
import { Dialog as DialogPrimitive } from '@base-ui/react/dialog';
import { XIcon } from 'lucide-react';

import { cn } from '@/lib/utils';

function Dialog({ ...props }: DialogPrimitive.Root.Props) {
  return <DialogPrimitive.Root data-slot='dialog' {...props} />;
}

function DialogTrigger({ ...props }: DialogPrimitive.Trigger.Props) {
  return <DialogPrimitive.Trigger data-slot='dialog-trigger' {...props} />;
}

function DialogPortal({ ...props }: DialogPrimitive.Portal.Props) {
  return <DialogPrimitive.Portal data-slot='dialog-portal' {...props} />;
}

function DialogClose({ ...props }: DialogPrimitive.Close.Props) {
  return <DialogPrimitive.Close data-slot='dialog-close' {...props} />;
}

function DialogOverlay({
  className,
  ...props
}: DialogPrimitive.Backdrop.Props) {
  return (
    <DialogPrimitive.Backdrop
      data-slot='dialog-overlay'
      className={cn(
        'fixed inset-0 isolate z-50 bg-black/10 duration-100 data-closed:animate-out data-closed:fade-out-0 data-open:animate-in data-open:fade-in-0 supports-backdrop-filter:backdrop-blur-xs',
        className
      )}
      {...props}
    />
  );
}

function DialogContent({
  className,
  children,
  showCloseButton = true,
  ...props
}: DialogPrimitive.Popup.Props & {
  showCloseButton?: boolean;
}) {
  return (
    <DialogPortal>
      <DialogOverlay />
      <DialogPrimitive.Popup
        data-slot='dialog-content'
        className={cn(
          'fixed top-1/2 left-1/2 z-50 grid w-full max-w-[calc(100%-2rem)] -translate-x-1/2 -translate-y-1/2 gap-4 rounded-xl bg-background p-4 text-sm ring-1 ring-foreground/10 duration-100 outline-none data-closed:animate-out data-closed:fade-out-0 data-closed:zoom-out-95 data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 sm:max-w-sm',
          className
        )}
        {...props}
      >
        {children}
        {showCloseButton && (
          <DialogPrimitive.Close
            data-slot='dialog-close'
            render={
              <Button
                variant='ghost'
                className='absolute top-2 right-2'
                size='icon-sm'
              >
                <XIcon />
                <span className='sr-only'>Close</span>
              </Button>
            }
          />
        )}
      </DialogPrimitive.Popup>
    </DialogPortal>
  );
}

function DialogHeader({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='dialog-header'
      className={cn('flex flex-col gap-2', className)}
      {...props}
    />
  );
}

function DialogFooter({
  className,
  showCloseButton = false,
  children,
  ...props
}: React.ComponentProps<'div'> & {
  showCloseButton?: boolean;
}) {
  return (
    <div
      data-slot='dialog-footer'
      className={cn(
        '-mx-4 -mb-4 flex flex-col-reverse gap-2 rounded-b-xl border-t bg-background p-4 sm:flex-row sm:justify-end',
        className
      )}
      {...props}
    >
      {children}
      {showCloseButton && (
        <DialogPrimitive.Close
          render={<Button variant='outline'>Close</Button>}
        />
      )}
    </div>
  );
}

function DialogTitle({ className, ...props }: DialogPrimitive.Title.Props) {
  return (
    <DialogPrimitive.Title
      data-slot='dialog-title'
      className={cn('text-base leading-none font-medium', className)}
      {...props}
    />
  );
}

function DialogDescription({
  className,
  ...props
}: DialogPrimitive.Description.Props) {
  return (
    <DialogPrimitive.Description
      data-slot='dialog-description'
      className={cn(
        'text-sm text-muted-foreground *:[a]:underline *:[a]:underline-offset-3 *:[a]:hover:text-foreground',
        className
      )}
      {...props}
    />
  );
}

export {
  Dialog,
  DialogClose,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogOverlay,
  DialogPortal,
  DialogTitle,
  DialogTrigger,
};
```
---

## Drawer <a name="drawer"></a>

A panel that slides in from the edge of the screen with swipe-to-dismiss gestures.

- **Registry Key**: `drawer`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/drawer.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `lucide-react`

#### How to Use
##### Example: `action-sheet`
```tsx
'use client';

import * as React from 'react';

import { Button } from '@/registry/default/ui/button';
import {
  Drawer,
  DrawerClose,
  DrawerContent,
  DrawerDescription,
  DrawerPopup,
  DrawerTitle,
  DrawerTrigger,
} from '@/registry/default/ui/drawer';

const ACTIONS = [
  'Unfollow',
  'Mute',
  'Add to Favourites',
  'Add to Close Friends',
  'Restrict',
];

export function DrawerActionSheet() {
  const [open, setOpen] = React.useState(false);

  return (
    <Drawer open={open} onOpenChange={setOpen}>
      <DrawerTrigger render={<Button variant='outline' />}>
        Open Action Sheet
      </DrawerTrigger>
      <DrawerPopup
        unstyled
        overlayClassName='bg-black/40'
        className='pointer-events-none relative flex w-full max-w-md transform-[translateY(var(--drawer-swipe-movement-y))] touch-auto flex-col gap-3 overflow-y-auto overscroll-contain bg-transparent px-4 pt-4 pb-[calc(1rem+env(safe-area-inset-bottom,0px))] text-foreground ring-0 transition-transform duration-300 ease-[cubic-bezier(0.32,0.72,0,1)] outline-none data-ending-style:transform-[translateY(calc(100%+1rem))] data-ending-style:duration-[calc(var(--drawer-swipe-strength)*300ms)] data-starting-style:transform-[translateY(calc(100%+1rem))] data-swiping:transition-none data-swiping:select-none'
      >
        <DrawerContent className='pointer-events-auto overflow-hidden rounded-2xl bg-background ring-1 ring-foreground/10'>
          <DrawerTitle className='sr-only'>Profile actions</DrawerTitle>
          <DrawerDescription className='sr-only'>
            Choose an action for this user.
          </DrawerDescription>

          <ul
            className='m-0 list-none divide-y divide-foreground/10 p-0'
            aria-label='Profile actions'
          >
            {ACTIONS.map((action, index) => (
              <li key={action}>
                {index === 0 && (
                  <DrawerClose className='sr-only'>
                    Close action sheet
                  </DrawerClose>
                )}
                <button
                  type='button'
                  className='block w-full border-0 bg-transparent px-5 py-4 text-center text-sm select-none hover:bg-muted focus-visible:bg-muted focus-visible:outline-none'
                  onClick={() => setOpen(false)}
                >
                  {action}
                </button>
              </li>
            ))}
          </ul>
        </DrawerContent>
        <div className='pointer-events-auto overflow-hidden rounded-2xl bg-background ring-1 ring-foreground/10'>
          <button
            type='button'
            className='block w-full border-0 bg-transparent px-5 py-4 text-center text-sm text-destructive select-none hover:bg-muted focus-visible:bg-muted focus-visible:outline-none'
            onClick={() => setOpen(false)}
          >
            Block User
          </button>
        </div>
      </DrawerPopup>
    </Drawer>
  );
}
```

##### Example: `default`
```tsx
'use client';

import { Button } from '@/registry/default/ui/button';
import {
  Drawer,
  DrawerClose,
  DrawerContent,
  DrawerDescription,
  DrawerFooter,
  DrawerHeader,
  DrawerPopup,
  DrawerTitle,
  DrawerTrigger,
} from '@/registry/default/ui/drawer';

export function DrawerDefault() {
  return (
    <Drawer>
      <DrawerTrigger render={<Button variant='outline' />}>
        Open Drawer
      </DrawerTrigger>
      <DrawerPopup>
        <DrawerContent>
          <DrawerHeader>
            <DrawerTitle>Notifications</DrawerTitle>
            <DrawerDescription>
              You are all caught up. Good job!
            </DrawerDescription>
          </DrawerHeader>
          <DrawerFooter>
            <DrawerClose render={<Button variant='outline' />}>
              Close
            </DrawerClose>
          </DrawerFooter>
        </DrawerContent>
      </DrawerPopup>
    </Drawer>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/drawer.tsx`
```tsx
'use client';

import * as React from 'react';

import { Drawer as DrawerPrimitive } from '@base-ui/react/drawer';

import { cn } from '@/lib/utils';

function Drawer({
  swipeDirection = 'down',
  ...props
}: DrawerPrimitive.Root.Props) {
  return (
    <DrawerPrimitive.Root
      data-slot='drawer'
      swipeDirection={swipeDirection}
      {...props}
    />
  );
}

function DrawerTrigger({ ...props }: DrawerPrimitive.Trigger.Props) {
  return <DrawerPrimitive.Trigger data-slot='drawer-trigger' {...props} />;
}

function DrawerClose({ ...props }: DrawerPrimitive.Close.Props) {
  return <DrawerPrimitive.Close data-slot='drawer-close' {...props} />;
}

function DrawerProvider({ ...props }: DrawerPrimitive.Provider.Props) {
  return <DrawerPrimitive.Provider data-slot='drawer-provider' {...props} />;
}

function DrawerPortal({
  className,
  ...props
}: Omit<DrawerPrimitive.Portal.Props, 'className'> & {
  className?: string;
}) {
  return (
    <DrawerPrimitive.Portal
      data-slot='drawer-portal'
      className={cn('z-50', className)}
      {...props}
    />
  );
}

function DrawerOverlay({
  className,
  ...props
}: Omit<DrawerPrimitive.Backdrop.Props, 'className'> & {
  className?: string;
}) {
  return (
    <DrawerPrimitive.Backdrop
      data-slot='drawer-overlay'
      className={cn(
        'fixed inset-0 min-h-dvh bg-black opacity-[calc(var(--backdrop-opacity)*(1-var(--drawer-swipe-progress)))] transition-opacity duration-300 ease-[cubic-bezier(0.32,0.72,0,1)] [--backdrop-opacity:0.2] data-ending-style:opacity-0 data-ending-style:duration-[calc(var(--drawer-swipe-strength)*300ms)] data-starting-style:opacity-0 data-swiping:duration-0 supports-backdrop-filter:backdrop-blur-3xl supports-[-webkit-touch-callout:none]:absolute dark:[--backdrop-opacity:0.7]',
        className
      )}
      {...props}
    />
  );
}

function DrawerViewport({
  className,
  ...props
}: Omit<DrawerPrimitive.Viewport.Props, 'className'> & {
  className?: string;
}) {
  return (
    <DrawerPrimitive.Viewport
      data-slot='drawer-viewport'
      className={cn(
        'fixed inset-0 flex',
        // Horizontal: stretch + viewport padding
        'has-data-[swipe-direction=left]:items-stretch has-data-[swipe-direction=left]:justify-start has-data-[swipe-direction=left]:p-(--viewport-padding)',
        'has-data-[swipe-direction=right]:items-stretch has-data-[swipe-direction=right]:justify-end has-data-[swipe-direction=right]:p-(--viewport-padding)',
        '[--viewport-padding:0px] supports-[-webkit-touch-callout:none]:[--viewport-padding:0.625rem]',
        // Vertical: center horizontally, align to edge
        'has-data-[swipe-direction=down]:items-end has-data-[swipe-direction=down]:justify-center',
        'has-data-[swipe-direction=up]:items-start has-data-[swipe-direction=up]:justify-center',
        className
      )}
      {...props}
    />
  );
}

function DrawerHandle({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='drawer-handle'
      className={cn(
        'mx-auto mb-5 h-1 w-12 shrink-0 rounded-full bg-muted transition-opacity duration-200 group-data-nested-drawer-open/popup:opacity-0 group-data-nested-drawer-swiping/popup:opacity-100',
        className
      )}
      {...props}
    />
  );
}

function DrawerContent({
  className,
  ...props
}: Omit<DrawerPrimitive.Content.Props, 'className'> & {
  className?: string;
}) {
  return (
    <DrawerPrimitive.Content
      data-slot='drawer-content'
      className={cn(
        'transition-opacity duration-300 ease-[cubic-bezier(0.45,1.005,0,1.005)] group-data-nested-drawer-open/popup:opacity-0 group-data-nested-drawer-swiping/popup:opacity-100',
        className
      )}
      {...props}
    />
  );
}

function DrawerPopup({
  className,
  children,
  container,
  showHandle,
  unstyled,
  overlayClassName,
  ...props
}: Omit<DrawerPrimitive.Popup.Props, 'className'> & {
  className?: string;
  container?: DrawerPrimitive.Portal.Props['container'];
  showHandle?: boolean;
  unstyled?: boolean;
  overlayClassName?: string;
}) {
  return (
    <DrawerPortal container={container}>
      <DrawerOverlay className={overlayClassName} />
      <DrawerViewport>
        <DrawerPrimitive.Popup
          data-slot='drawer-popup'
          className={
            unstyled
              ? className
              : cn(
                  'group/popup relative',
                  'touch-auto overflow-y-auto overscroll-contain bg-background text-foreground ring-1 ring-foreground/5 data-swiping:select-none dark:ring-border',
                  'data-ending-style:duration-[calc(var(--drawer-swipe-strength)*300ms)]',
                  // Nested drawer stacking variables (no-ops when not nested)
                  '[--bleed:3rem] [--height:max(0px,calc(var(--drawer-frontmost-height,var(--drawer-height))-var(--bleed)))] [--peek:1rem] [--scale-base:calc(max(0,1-(var(--nested-drawers)*var(--stack-step))))] [--scale:clamp(0,calc(var(--scale-base)+(var(--stack-step)*var(--stack-progress))),1)] [--shrink:calc(1-var(--scale))] [--stack-peek-offset:max(0px,calc((var(--nested-drawers)-var(--stack-progress))*var(--peek)))] [--stack-progress:clamp(0,var(--drawer-swipe-progress),1)] [--stack-step:0.05]',
                  // Nested drawer overlay (::after pseudo-element)
                  "after:pointer-events-none after:absolute after:inset-0 after:rounded-[inherit] after:bg-transparent after:transition-[background-color] after:duration-300 after:ease-[cubic-bezier(0.32,0.72,0,1)] after:content-['']",
                  // Nested drawer states
                  'data-nested-drawer-open:overflow-hidden data-nested-drawer-open:after:bg-black/5 data-nested-drawer-swiping:duration-0',
                  // Shared horizontal (left & right)
                  'data-[swipe-direction=left]:supports-[-webkit-touch-callout:none]:[--bleed:0px] data-[swipe-direction=right]:supports-[-webkit-touch-callout:none]:[--bleed:0px]',
                  'data-[swipe-direction=left]:h-full data-[swipe-direction=right]:h-full',
                  'data-[swipe-direction=left]:w-[calc(22rem+var(--bleed))] data-[swipe-direction=right]:w-[calc(22rem+var(--bleed))]',
                  'data-[swipe-direction=left]:max-w-[calc(100vw-3rem+var(--bleed))] data-[swipe-direction=right]:max-w-[calc(100vw-3rem+var(--bleed))]',
                  'data-[swipe-direction=left]:p-6 data-[swipe-direction=right]:p-6',
                  'data-[swipe-direction=left]:supports-[-webkit-touch-callout:none]:w-[20rem] data-[swipe-direction=right]:supports-[-webkit-touch-callout:none]:w-[20rem]',
                  'data-[swipe-direction=left]:supports-[-webkit-touch-callout:none]:max-w-[calc(100vw-20px)] data-[swipe-direction=right]:supports-[-webkit-touch-callout:none]:max-w-[calc(100vw-20px)]',
                  'data-[swipe-direction=left]:supports-[-webkit-touch-callout:none]:rounded-[10px] data-[swipe-direction=right]:supports-[-webkit-touch-callout:none]:rounded-[10px]',
                  // Right-only (with stacking transform + transition for box-shadow)
                  'data-[swipe-direction=right]:-mr-(--bleed) data-[swipe-direction=right]:origin-[calc(100%-var(--bleed))_50%] data-[swipe-direction=right]:transform-[translateX(calc(var(--drawer-snap-point-offset,0px)+var(--drawer-swipe-movement-x)-var(--stack-peek-offset)-(var(--shrink)*100%)))_scale(var(--scale))] data-[swipe-direction=right]:rounded-l-xl data-[swipe-direction=right]:pr-[calc(1.5rem+var(--bleed))] data-[swipe-direction=right]:shadow-[-2px_0_10px_rgb(0_0_0/0.1)] data-[swipe-direction=right]:[transition:transform_300ms_cubic-bezier(0.32,0.72,0,1),box-shadow_300ms_cubic-bezier(0.32,0.72,0,1)] data-[swipe-direction=right]:data-ending-style:shadow-[-2px_0_10px_rgb(0_0_0/0)] data-[swipe-direction=right]:data-swiping:duration-0 data-[swipe-direction=right]:supports-[-webkit-touch-callout:none]:mr-0 data-[swipe-direction=right]:supports-[-webkit-touch-callout:none]:pr-6',
                  // Right enter/exit
                  'data-[swipe-direction=right]:data-ending-style:transform-[translateX(calc(100%-var(--bleed)+var(--viewport-padding)))] data-[swipe-direction=right]:data-starting-style:transform-[translateX(calc(100%-var(--bleed)+var(--viewport-padding)))]',
                  // Left-only (with stacking transform + transition for box-shadow)
                  'data-[swipe-direction=left]:-ml-(--bleed) data-[swipe-direction=left]:origin-[var(--bleed)_50%] data-[swipe-direction=left]:transform-[translateX(calc(var(--drawer-snap-point-offset,0px)+var(--drawer-swipe-movement-x)+var(--stack-peek-offset)+(var(--shrink)*100%)))_scale(var(--scale))] data-[swipe-direction=left]:rounded-r-xl data-[swipe-direction=left]:pl-[calc(1.5rem+var(--bleed))] data-[swipe-direction=left]:shadow-[2px_0_10px_rgb(0_0_0/0.1)] data-[swipe-direction=left]:[transition:transform_300ms_cubic-bezier(0.32,0.72,0,1),box-shadow_300ms_cubic-bezier(0.32,0.72,0,1)] data-[swipe-direction=left]:data-ending-style:shadow-[2px_0_10px_rgb(0_0_0/0)] data-[swipe-direction=left]:data-swiping:duration-0 data-[swipe-direction=left]:supports-[-webkit-touch-callout:none]:ml-0 data-[swipe-direction=left]:supports-[-webkit-touch-callout:none]:pl-6',
                  // Left enter/exit
                  'data-[swipe-direction=left]:data-ending-style:transform-[translateX(calc(-100%+var(--bleed)-var(--viewport-padding)))] data-[swipe-direction=left]:data-starting-style:transform-[translateX(calc(-100%+var(--bleed)-var(--viewport-padding)))]',
                  // Shared vertical (up & down)
                  'data-[swipe-direction=down]:w-full data-[swipe-direction=up]:w-full',
                  'data-[swipe-direction=down]:max-h-[calc(80vh+var(--bleed))] data-[swipe-direction=up]:max-h-[calc(80vh+var(--bleed))]',
                  'data-[swipe-direction=down]:px-6 data-[swipe-direction=up]:px-6',
                  // Down-only (with stacking transform + transitions for height & box-shadow)
                  'data-[swipe-direction=down]:-mb-(--bleed) data-[swipe-direction=down]:h-(--drawer-height,auto) data-[swipe-direction=down]:origin-[50%_calc(100%-var(--bleed))] data-[swipe-direction=down]:transform-[translateY(calc(var(--drawer-snap-point-offset,0px)+var(--drawer-swipe-movement-y)-var(--stack-peek-offset)-(var(--shrink)*var(--height))))_scale(var(--scale))] data-[swipe-direction=down]:rounded-t-xl data-[swipe-direction=down]:pt-4 data-[swipe-direction=down]:pb-[calc(1.5rem+env(safe-area-inset-bottom,0px)+var(--bleed))] data-[swipe-direction=down]:shadow-[0_2px_10px_rgb(0_0_0/0.1)] data-[swipe-direction=down]:[transition:transform_300ms_cubic-bezier(0.32,0.72,0,1),height_300ms_cubic-bezier(0.32,0.72,0,1),box-shadow_300ms_cubic-bezier(0.32,0.72,0,1)] data-[swipe-direction=down]:data-ending-style:shadow-[0_2px_10px_rgb(0_0_0/0)] data-[swipe-direction=down]:data-nested-drawer-open:h-[calc(var(--height)+var(--bleed))] data-[swipe-direction=down]:data-swiping:duration-0',
                  // Down enter/exit
                  'data-[swipe-direction=down]:data-ending-style:transform-[translateY(calc(100%-var(--bleed)))] data-[swipe-direction=down]:data-starting-style:transform-[translateY(calc(100%-var(--bleed)))]',
                  // Up-only (with stacking transform + transitions for height & box-shadow)
                  'data-[swipe-direction=up]:-mt-(--bleed) data-[swipe-direction=up]:h-(--drawer-height,auto) data-[swipe-direction=up]:origin-[50%_var(--bleed)] data-[swipe-direction=up]:transform-[translateY(calc(var(--drawer-snap-point-offset,0px)+var(--drawer-swipe-movement-y)+var(--stack-peek-offset)+(var(--shrink)*var(--height))))_scale(var(--scale))] data-[swipe-direction=up]:rounded-b-xl data-[swipe-direction=up]:pt-[calc(1.5rem+env(safe-area-inset-top,0px)+var(--bleed))] data-[swipe-direction=up]:pb-6 data-[swipe-direction=up]:shadow-[0_-2px_10px_rgb(0_0_0/0.1)] data-[swipe-direction=up]:[transition:transform_300ms_cubic-bezier(0.32,0.72,0,1),height_300ms_cubic-bezier(0.32,0.72,0,1),box-shadow_300ms_cubic-bezier(0.32,0.72,0,1)] data-[swipe-direction=up]:data-ending-style:shadow-[0_-2px_10px_rgb(0_0_0/0)] data-[swipe-direction=up]:data-nested-drawer-open:h-[calc(var(--height)+var(--bleed))] data-[swipe-direction=up]:data-swiping:duration-0',
                  // Up enter/exit
                  'data-[swipe-direction=up]:data-ending-style:transform-[translateY(calc(-100%+var(--bleed)))] data-[swipe-direction=up]:data-starting-style:transform-[translateY(calc(-100%+var(--bleed)))]',
                  className
                )
          }
          {...props}
        >
          {!unstyled && showHandle !== false && (
            <DrawerHandle
              className={
                showHandle
                  ? undefined
                  : 'hidden group-data-[swipe-direction=down]/popup:block group-data-[swipe-direction=up]/popup:block'
              }
            />
          )}
          {children}
        </DrawerPrimitive.Popup>
      </DrawerViewport>
    </DrawerPortal>
  );
}

function DrawerHeader({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='drawer-header'
      className={cn('flex flex-col gap-2', className)}
      {...props}
    />
  );
}

function DrawerFooter({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='drawer-footer'
      className={cn(
        'mt-4 flex flex-col-reverse gap-2 sm:flex-row sm:justify-end',
        className
      )}
      {...props}
    />
  );
}

function DrawerTitle({
  className,
  ...props
}: Omit<DrawerPrimitive.Title.Props, 'className'> & {
  className?: string;
}) {
  return (
    <DrawerPrimitive.Title
      data-slot='drawer-title'
      className={cn(
        'text-base font-medium text-foreground group-data-[swipe-direction=down]/popup:text-center group-data-[swipe-direction=up]/popup:text-center',
        className
      )}
      {...props}
    />
  );
}

function DrawerDescription({
  className,
  ...props
}: Omit<DrawerPrimitive.Description.Props, 'className'> & {
  className?: string;
}) {
  return (
    <DrawerPrimitive.Description
      data-slot='drawer-description'
      className={cn(
        'mt-1.5 text-sm text-muted-foreground group-data-[swipe-direction=down]/popup:text-center group-data-[swipe-direction=up]/popup:text-center',
        className
      )}
      {...props}
    />
  );
}

function DrawerIndentBackground({
  className,
  ...props
}: DrawerPrimitive.IndentBackground.Props) {
  return (
    <DrawerPrimitive.IndentBackground
      data-slot='drawer-indent-background'
      className='absolute inset-0 isolate bg-foreground/70'
      {...props}
    />
  );
}

function DrawerIndent({
  className,
  ...props
}: Omit<DrawerPrimitive.Indent.Props, 'className'> & {
  className?: string;
}) {
  return (
    <DrawerPrimitive.Indent
      data-slot='drawer-indent'
      className={cn(
        'relative origin-[center_top] transform-[scale(1)_translateY(0)] border bg-background p-4 duration-[calc(400ms*var(--indent-transition)),calc(250ms*var(--indent-transition))] will-change-transform [--indent-progress:var(--drawer-swipe-progress)] [--indent-radius:calc(1rem*(1-var(--indent-progress)))] [--indent-transition:calc(1-clamp(0,calc(var(--drawer-swipe-progress)*100000),1))] [transition:transform_0.4s_cubic-bezier(0.32,0.72,0,1),border-radius_0.25s_cubic-bezier(0.32,0.72,0,1)] data-active:transform-[scale(calc(0.98+(0.02*var(--indent-progress))))_translateY(calc(0.5rem*(1-var(--indent-progress))))] data-active:rounded-tl-(--indent-radius) data-active:rounded-tr-(--indent-radius)',
        className
      )}
      {...props}
    />
  );
}

const createDrawerHandle = DrawerPrimitive.createHandle;

export {
  Drawer,
  DrawerClose,
  DrawerContent,
  DrawerDescription,
  DrawerFooter,
  DrawerHandle,
  DrawerHeader,
  DrawerIndent,
  DrawerIndentBackground,
  DrawerOverlay,
  DrawerPopup,
  DrawerPortal,
  DrawerProvider,
  DrawerTitle,
  DrawerTrigger,
  DrawerViewport,
  createDrawerHandle,
};
```
---

## Dropdown Menu <a name="dropdown-menu"></a>

A menu that appears when clicking a trigger element, providing a list of actions or options.

- **Registry Key**: `dropdown-menu`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/dropdown-menu.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `lucide-react`

#### How to Use
##### Example: `checkboxes`
```tsx
'use client';

import * as React from 'react';

import { Button } from '@/registry/default/ui/button';
import {
  DropdownMenu,
  DropdownMenuCheckboxItem,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuLabel,
  DropdownMenuTrigger,
} from '@/registry/default/ui/dropdown-menu';

export function DropdownMenuCheckboxes() {
  const [emailNotifs, setEmailNotifs] = React.useState(true);
  const [pushNotifs, setPushNotifs] = React.useState(false);
  const [smsNotifs, setSmsNotifs] = React.useState(false);

  return (
    <DropdownMenu>
      <DropdownMenuTrigger render={<Button variant='outline' />}>
        Notifications
      </DropdownMenuTrigger>
      <DropdownMenuContent className='w-44'>
        <DropdownMenuGroup>
          <DropdownMenuLabel>Notify me via</DropdownMenuLabel>
          <DropdownMenuCheckboxItem
            checked={emailNotifs}
            onCheckedChange={setEmailNotifs}
          >
            Email
          </DropdownMenuCheckboxItem>
          <DropdownMenuCheckboxItem
            checked={pushNotifs}
            onCheckedChange={setPushNotifs}
          >
            Push notifications
          </DropdownMenuCheckboxItem>
          <DropdownMenuCheckboxItem
            checked={smsNotifs}
            onCheckedChange={setSmsNotifs}
            disabled
          >
            SMS
          </DropdownMenuCheckboxItem>
        </DropdownMenuGroup>
      </DropdownMenuContent>
    </DropdownMenu>
  );
}
```

##### Example: `default`
```tsx
'use client';

import { Button } from '@/registry/default/ui/button';
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuSeparator,
  DropdownMenuTrigger,
} from '@/registry/default/ui/dropdown-menu';

export function DropdownMenuDefault() {
  return (
    <DropdownMenu>
      <DropdownMenuTrigger render={<Button variant='outline' />}>
        Open Menu
      </DropdownMenuTrigger>
      <DropdownMenuContent>
        <DropdownMenuItem>Profile</DropdownMenuItem>
        <DropdownMenuItem>Settings</DropdownMenuItem>
        <DropdownMenuItem>Billing</DropdownMenuItem>
        <DropdownMenuSeparator />
        <DropdownMenuItem>Log out</DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/dropdown-menu.tsx`
```tsx
'use client';

import * as React from 'react';

import { Menu as MenuPrimitive } from '@base-ui/react/menu';
import { CheckIcon, ChevronRightIcon } from 'lucide-react';

import { cn } from '@/lib/utils';

function DropdownMenu({ ...props }: MenuPrimitive.Root.Props) {
  return <MenuPrimitive.Root data-slot='dropdown-menu' {...props} />;
}

function DropdownMenuPortal({ ...props }: MenuPrimitive.Portal.Props) {
  return <MenuPrimitive.Portal data-slot='dropdown-menu-portal' {...props} />;
}

function DropdownMenuTrigger({ ...props }: MenuPrimitive.Trigger.Props) {
  return <MenuPrimitive.Trigger data-slot='dropdown-menu-trigger' {...props} />;
}

function DropdownMenuContent({
  align = 'start',
  alignOffset = 0,
  side = 'bottom',
  sideOffset = 4,
  className,
  ...props
}: MenuPrimitive.Popup.Props &
  Pick<
    MenuPrimitive.Positioner.Props,
    'align' | 'alignOffset' | 'side' | 'sideOffset'
  >) {
  return (
    <MenuPrimitive.Portal>
      <MenuPrimitive.Positioner
        className='isolate z-50 outline-none'
        align={align}
        alignOffset={alignOffset}
        side={side}
        sideOffset={sideOffset}
      >
        <MenuPrimitive.Popup
          data-slot='dropdown-menu-content'
          className={cn(
            'z-50 max-h-(--available-height) w-(--anchor-width) min-w-32 origin-(--transform-origin) overflow-x-hidden overflow-y-auto rounded-xl bg-popover p-1 text-popover-foreground shadow-md ring-1 ring-foreground/10 duration-100 outline-none data-closed:animate-out data-closed:overflow-hidden data-closed:fade-out-0 data-closed:zoom-out-95 data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=inline-end]:slide-in-from-left-2 data-[side=inline-start]:slide-in-from-right-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2',
            className
          )}
          {...props}
        />
      </MenuPrimitive.Positioner>
    </MenuPrimitive.Portal>
  );
}

function DropdownMenuGroup({ ...props }: MenuPrimitive.Group.Props) {
  return <MenuPrimitive.Group data-slot='dropdown-menu-group' {...props} />;
}

function DropdownMenuLabel({
  className,
  inset,
  ...props
}: MenuPrimitive.GroupLabel.Props & {
  inset?: boolean;
}) {
  return (
    <MenuPrimitive.GroupLabel
      data-slot='dropdown-menu-label'
      data-inset={inset}
      className={cn(
        'px-1.5 py-1 text-xs font-medium text-muted-foreground data-inset:pl-8',
        className
      )}
      {...props}
    />
  );
}

function DropdownMenuItem({
  className,
  inset,
  variant = 'default',
  ...props
}: MenuPrimitive.Item.Props & {
  inset?: boolean;
  variant?: 'default' | 'destructive';
}) {
  return (
    <MenuPrimitive.Item
      data-slot='dropdown-menu-item'
      data-inset={inset}
      data-variant={variant}
      className={cn(
        "group/dropdown-menu-item relative flex cursor-default items-center gap-1.5 rounded-lg px-1.5 py-1 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground not-data-[variant=destructive]:focus:**:text-accent-foreground data-disabled:pointer-events-none data-disabled:opacity-50 data-inset:pl-8 data-[variant=destructive]:text-destructive data-[variant=destructive]:focus:bg-destructive/10 data-[variant=destructive]:focus:text-destructive dark:data-[variant=destructive]:focus:bg-destructive/20 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 data-[variant=destructive]:*:[svg]:text-destructive",
        className
      )}
      {...props}
    />
  );
}

function DropdownMenuSub({ ...props }: MenuPrimitive.SubmenuRoot.Props) {
  return <MenuPrimitive.SubmenuRoot data-slot='dropdown-menu-sub' {...props} />;
}

function DropdownMenuSubTrigger({
  className,
  inset,
  children,
  ...props
}: MenuPrimitive.SubmenuTrigger.Props & {
  inset?: boolean;
}) {
  return (
    <MenuPrimitive.SubmenuTrigger
      data-slot='dropdown-menu-sub-trigger'
      data-inset={inset}
      className={cn(
        "flex cursor-default items-center gap-1.5 rounded-md px-1.5 py-1 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground not-data-[variant=destructive]:focus:**:text-accent-foreground data-inset:pl-8 data-open:bg-accent data-open:text-accent-foreground data-popup-open:bg-accent data-popup-open:text-accent-foreground [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    >
      {children}
      <ChevronRightIcon className='ml-auto' />
    </MenuPrimitive.SubmenuTrigger>
  );
}

function DropdownMenuSubContent({
  align = 'start',
  alignOffset = -3,
  side = 'right',
  sideOffset = 0,
  className,
  ...props
}: React.ComponentProps<typeof DropdownMenuContent>) {
  return (
    <DropdownMenuContent
      data-slot='dropdown-menu-sub-content'
      className={cn(
        'w-auto min-w-24 rounded-md bg-popover p-1 text-popover-foreground shadow-lg ring-1 ring-foreground/10 duration-100 data-closed:animate-out data-closed:fade-out-0 data-closed:zoom-out-95 data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2',
        className
      )}
      align={align}
      alignOffset={alignOffset}
      side={side}
      sideOffset={sideOffset}
      {...props}
    />
  );
}

function DropdownMenuCheckboxItem({
  className,
  children,
  checked,
  ...props
}: MenuPrimitive.CheckboxItem.Props) {
  return (
    <MenuPrimitive.CheckboxItem
      data-slot='dropdown-menu-checkbox-item'
      className={cn(
        "relative flex cursor-default items-center gap-1.5 rounded-md py-1 pr-8 pl-1.5 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground focus:**:text-accent-foreground data-disabled:pointer-events-none data-disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      checked={checked}
      {...props}
    >
      <span
        className='pointer-events-none absolute right-2 flex items-center justify-center'
        data-slot='dropdown-menu-checkbox-item-indicator'
      >
        <MenuPrimitive.CheckboxItemIndicator>
          <CheckIcon />
        </MenuPrimitive.CheckboxItemIndicator>
      </span>
      {children}
    </MenuPrimitive.CheckboxItem>
  );
}

function DropdownMenuRadioGroup({ ...props }: MenuPrimitive.RadioGroup.Props) {
  return (
    <MenuPrimitive.RadioGroup
      data-slot='dropdown-menu-radio-group'
      {...props}
    />
  );
}

function DropdownMenuRadioItem({
  className,
  children,
  ...props
}: MenuPrimitive.RadioItem.Props) {
  return (
    <MenuPrimitive.RadioItem
      data-slot='dropdown-menu-radio-item'
      className={cn(
        "relative flex cursor-default items-center gap-1.5 rounded-md py-1 pr-8 pl-1.5 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground focus:**:text-accent-foreground data-disabled:pointer-events-none data-disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    >
      <span
        className='pointer-events-none absolute right-2 flex items-center justify-center'
        data-slot='dropdown-menu-radio-item-indicator'
      >
        <MenuPrimitive.RadioItemIndicator>
          <CheckIcon />
        </MenuPrimitive.RadioItemIndicator>
      </span>
      {children}
    </MenuPrimitive.RadioItem>
  );
}

function DropdownMenuSeparator({
  className,
  ...props
}: MenuPrimitive.Separator.Props) {
  return (
    <MenuPrimitive.Separator
      data-slot='dropdown-menu-separator'
      className={cn('-mx-1 my-1 h-px bg-border', className)}
      {...props}
    />
  );
}

function DropdownMenuShortcut({
  className,
  ...props
}: React.ComponentProps<'span'>) {
  return (
    <span
      data-slot='dropdown-menu-shortcut'
      className={cn(
        'ml-auto text-xs tracking-widest text-muted-foreground group-focus/dropdown-menu-item:text-accent-foreground',
        className
      )}
      {...props}
    />
  );
}

export {
  DropdownMenu,
  DropdownMenuPortal,
  DropdownMenuTrigger,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuLabel,
  DropdownMenuItem,
  DropdownMenuCheckboxItem,
  DropdownMenuRadioGroup,
  DropdownMenuRadioItem,
  DropdownMenuSeparator,
  DropdownMenuShortcut,
  DropdownMenuSub,
  DropdownMenuSubTrigger,
  DropdownMenuSubContent,
};
```
---

## Field <a name="field"></a>

A form field wrapper that associates a label, description, and error message with an input.

- **Registry Key**: `field`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/field.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Field, FieldLabel } from '@/registry/default/ui/field';
import { Input } from '@/registry/default/ui/input';

export function FieldDefault() {
  return (
    <div>
      <Field>
        <FieldLabel>Email</FieldLabel>
        <Input type='email' placeholder='Enter your email' />
      </Field>
    </div>
  );
}
```

##### Example: `with-description`
```tsx
'use client';

import {
  Field,
  FieldDescription,
  FieldLabel,
} from '@/registry/default/ui/field';
import { Input } from '@/registry/default/ui/input';

export function FieldWithDescription() {
  return (
    <div>
      <Field>
        <FieldLabel>Username</FieldLabel>
        <Input placeholder='Enter your username' />
        <FieldDescription className='my-0!'>
          This will be your public display name.
        </FieldDescription>
      </Field>
    </div>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/field.tsx`
```tsx
'use client';

import { Field as FieldPrimitive } from '@base-ui/react/field';

import { cn } from '@/lib/utils';

function Field({ className, ...props }: FieldPrimitive.Root.Props) {
  return (
    <FieldPrimitive.Root
      data-slot='field'
      className={cn('flex w-full flex-col items-start gap-1', className)}
      {...props}
    />
  );
}

function FieldLabel({ className, ...props }: FieldPrimitive.Label.Props) {
  return (
    <FieldPrimitive.Label
      data-slot='field-label'
      className={cn('text-sm font-medium', className)}
      {...props}
    />
  );
}

function FieldDescription({
  className,
  ...props
}: FieldPrimitive.Description.Props) {
  return (
    <FieldPrimitive.Description
      data-slot='field-description'
      className={cn('text-sm text-muted-foreground', className)}
      {...props}
    />
  );
}

function FieldError({ className, ...props }: FieldPrimitive.Error.Props) {
  return (
    <FieldPrimitive.Error
      data-slot='field-error'
      className={cn('text-destructive-foreground text-sm', className)}
      {...props}
    />
  );
}

const FieldControl = FieldPrimitive.Control;
const FieldValidity = FieldPrimitive.Validity;

export {
  Field,
  FieldLabel,
  FieldDescription,
  FieldError,
  FieldControl,
  FieldValidity,
};
```
---

## Fieldset <a name="fieldset"></a>

A container for grouping related form fields with a legend.

- **Registry Key**: `fieldset`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/fieldset.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Field, FieldLabel } from '@/registry/default/ui/field';
import { Fieldset, FieldsetLegend } from '@/registry/default/ui/fieldset';
import { Input } from '@/registry/default/ui/input';

export function FieldsetDefault() {
  return (
    <Fieldset className='flex flex-col gap-4'>
      <FieldsetLegend>Shipping Address</FieldsetLegend>
      <Field>
        <FieldLabel>Street Address</FieldLabel>
        <Input placeholder='123 Main St' />
      </Field>
      <Field>
        <FieldLabel>City</FieldLabel>
        <Input placeholder='New York' />
      </Field>
    </Fieldset>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/fieldset.tsx`
```tsx
'use client';

import { Fieldset as FieldsetPrimitive } from '@base-ui/react/fieldset';

import { cn } from '@/lib/utils';

function Fieldset({ className, ...props }: FieldsetPrimitive.Root.Props) {
  return (
    <FieldsetPrimitive.Root
      data-slot='fieldset-legend'
      className={cn('font-medium', className)}
      {...props}
    />
  );
}

function FieldsetLegend({
  className,
  ...props
}: FieldsetPrimitive.Legend.Props) {
  return (
    <FieldsetPrimitive.Legend
      data-slot='fieldset-legend'
      className={cn('font-medium', className)}
      {...props}
    />
  );
}

export { Fieldset, FieldsetLegend };
```
---

## Form <a name="form"></a>

A form container that handles validation and submission.

- **Registry Key**: `form`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/form.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Button } from '@/registry/default/ui/button';
import {
  Field,
  FieldDescription,
  FieldLabel,
} from '@/registry/default/ui/field';
import { Form } from '@/registry/default/ui/form';
import { Input } from '@/registry/default/ui/input';

export function FormDefault() {
  return (
    <Form className='w-full max-w-sm'>
      <Field>
        <FieldLabel>Email</FieldLabel>
        <Input type='email' placeholder='you@example.com' />
        <FieldDescription className='my-0!'>
          We&apos;ll never share your email.
        </FieldDescription>
      </Field>
      <Field>
        <FieldLabel>Password</FieldLabel>
        <Input type='password' placeholder='Enter your password' />
      </Field>
      <Button type='submit' className='mt-2'>
        Sign In
      </Button>
    </Form>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/form.tsx`
```tsx
'use client';

import { Form as FormPrimitive } from '@base-ui/react/form';

import { cn } from '@/lib/utils';

function Form({ className, ...props }: FormPrimitive.Props) {
  return (
    <FormPrimitive
      className={cn('flex w-full flex-col gap-4', className)}
      data-slot='form'
      {...props}
    />
  );
}

export { Form };
```
---

## Hover Card <a name="hover-card"></a>

A card that appears on hover to preview content.

- **Registry Key**: `hover-card`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/hover-card.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import {
  HoverCard,
  HoverCardContent,
  HoverCardTrigger,
} from '@/registry/default/ui/hover-card';

export function HoverCardDefault() {
  return (
    <HoverCard>
      <HoverCardTrigger
        delay={10}
        closeDelay={100}
        className='text-primary underline underline-offset-4'
      >
        Hover over me
      </HoverCardTrigger>
      <HoverCardContent>
        <div className='flex flex-col gap-2'>
          <p className='font-semibold'>Hover Card</p>
          <p className='text-muted-foreground'>
            This is a hover card that appears on hover. It can contain any
            content you want to display.
          </p>
        </div>
      </HoverCardContent>
    </HoverCard>
  );
}
```

##### Example: `sides`
```tsx
'use client';

import { Button } from '@/registry/default/ui/button';
import {
  HoverCard,
  HoverCardContent,
  HoverCardTrigger,
} from '@/registry/default/ui/hover-card';

const HOVER_CARD_SIDES = ['left', 'top', 'bottom', 'right'] as const;

export function HoverCardSides() {
  return (
    <div className='flex flex-wrap justify-center gap-2'>
      {HOVER_CARD_SIDES.map((side) => (
        <HoverCard key={side}>
          <HoverCardTrigger
            delay={100}
            closeDelay={100}
            render={
              <Button variant='outline' className='capitalize'>
                {side}
              </Button>
            }
          />
          <HoverCardContent side={side}>
            <div className='flex flex-col gap-1'>
              <h4 className='font-medium'>Hover Card</h4>
              <p>This hover card appears on the {side} side of the trigger.</p>
            </div>
          </HoverCardContent>
        </HoverCard>
      ))}
    </div>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/hover-card.tsx`
```tsx
'use client';

import { PreviewCard as PreviewCardPrimitive } from '@base-ui/react/preview-card';

import { cn } from '@/lib/utils';

function HoverCard({ ...props }: PreviewCardPrimitive.Root.Props) {
  return <PreviewCardPrimitive.Root data-slot='hover-card' {...props} />;
}

function HoverCardTrigger({ ...props }: PreviewCardPrimitive.Trigger.Props) {
  return (
    <PreviewCardPrimitive.Trigger data-slot='hover-card-trigger' {...props} />
  );
}

function HoverCardContent({
  className,
  side = 'bottom',
  sideOffset = 4,
  align = 'center',
  alignOffset = 4,
  ...props
}: PreviewCardPrimitive.Popup.Props &
  Pick<
    PreviewCardPrimitive.Positioner.Props,
    'align' | 'alignOffset' | 'side' | 'sideOffset'
  >) {
  return (
    <PreviewCardPrimitive.Portal data-slot='hover-card-portal'>
      <PreviewCardPrimitive.Positioner
        align={align}
        alignOffset={alignOffset}
        side={side}
        sideOffset={sideOffset}
        className='isolate z-50'
      >
        <PreviewCardPrimitive.Popup
          data-slot='hover-card-content'
          className={cn(
            'z-50 w-64 origin-(--transform-origin) rounded-lg bg-popover p-2.5 text-sm text-popover-foreground shadow-md ring-1 ring-foreground/10 outline-hidden duration-100 data-closed:animate-out data-closed:fade-out-0 data-closed:zoom-out-95 data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=inline-end]:slide-in-from-left-2 data-[side=inline-start]:slide-in-from-right-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2',
            className
          )}
          {...props}
        />
      </PreviewCardPrimitive.Positioner>
    </PreviewCardPrimitive.Portal>
  );
}

export { HoverCard, HoverCardTrigger, HoverCardContent };
```
---

## Input <a name="input"></a>

A text input field for capturing user data.

- **Registry Key**: `input`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/input.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Input } from '@/registry/default/ui/input';

export function InputDefault() {
  return <Input type='text' placeholder='Enter text...' className='w-64' />;
}
```

##### Example: `disabled`
```tsx
'use client';

import { Input } from '@/registry/default/ui/input';

export function InputDisabled() {
  return (
    <Input type='text' placeholder='Disabled input' disabled className='w-64' />
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/input.tsx`
```tsx
import * as React from 'react';

import { Input as InputPrimitive } from '@base-ui/react/input';

import { cn } from '@/lib/utils';

function Input({ className, type, ...props }: React.ComponentProps<'input'>) {
  return (
    <InputPrimitive
      type={type}
      data-slot='input'
      className={cn(
        'h-8 w-full min-w-0 rounded-4xl border border-input bg-transparent px-2.5 py-1 text-base transition-colors outline-none file:inline-flex file:h-6 file:border-0 file:bg-transparent file:text-sm file:font-medium file:text-foreground placeholder:text-muted-foreground focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50 disabled:pointer-events-none disabled:cursor-not-allowed disabled:bg-input/50 disabled:opacity-50 aria-invalid:border-destructive aria-invalid:ring-[3px] aria-invalid:ring-destructive/20 md:text-sm dark:bg-input/30 dark:disabled:bg-input/80 dark:aria-invalid:border-destructive/50 dark:aria-invalid:ring-destructive/40',
        className
      )}
      {...props}
    />
  );
}

export { Input };
```
---

## Input Group <a name="input-group"></a>

An input with addons like icons, buttons, or text on either side.

- **Registry Key**: `input-group`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/input-group.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `class-variance-authority`

#### How to Use
##### Example: `default`
```tsx
'use client';

import {
  InputGroup,
  InputGroupAddon,
  InputGroupInput,
  InputGroupText,
  InputGroupTextarea,
} from '@/registry/default/ui/input-group';

export function InputGroupDefault() {
  return (
    <div className='grid w-full max-w-sm gap-6'>
      <InputGroup>
        <InputGroupAddon>
          <InputGroupText>https://</InputGroupText>
        </InputGroupAddon>
        <InputGroupInput placeholder='example.com' className='pl-0.5!' />
      </InputGroup>
      <InputGroup>
        <InputGroupAddon>
          <InputGroupText>@</InputGroupText>
        </InputGroupAddon>
        <InputGroupInput placeholder='username' />
      </InputGroup>
      <InputGroup>
        <InputGroupInput placeholder='18' />
        <InputGroupAddon align='inline-end'>
          <InputGroupText>years</InputGroupText>
        </InputGroupAddon>
      </InputGroup>
      <InputGroup>
        <InputGroupTextarea placeholder='Write a bio...' />
        <InputGroupAddon align='block-end'>
          <InputGroupText className='text-xs text-muted-foreground'>
            Max 200 characters
          </InputGroupText>
        </InputGroupAddon>
      </InputGroup>
    </div>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/input-group.tsx`
```tsx
'use client';

import * as React from 'react';

import { Button } from '@/registry/default/ui/button';
import { Input } from '@/registry/default/ui/input';
import { Textarea } from '@/registry/default/ui/textarea';
import { cva, type VariantProps } from 'class-variance-authority';

import { cn } from '@/lib/utils';

function InputGroup({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='input-group'
      role='group'
      className={cn(
        'group/input-group relative flex h-8 w-full min-w-0 items-center rounded-lg border border-input transition-colors outline-none in-data-[slot=combobox-content]:focus-within:border-inherit in-data-[slot=combobox-content]:focus-within:ring-0 has-disabled:bg-input/50 has-disabled:opacity-50 has-[[data-slot=input-group-control]:focus-visible]:border-ring has-[[data-slot=input-group-control]:focus-visible]:ring-[3px] has-[[data-slot=input-group-control]:focus-visible]:ring-ring/50 has-[[data-slot][aria-invalid=true]]:border-destructive has-[[data-slot][aria-invalid=true]]:ring-[3px] has-[[data-slot][aria-invalid=true]]:ring-destructive/20 has-[>[data-align=block-end]]:h-auto has-[>[data-align=block-end]]:flex-col has-[>[data-align=block-start]]:h-auto has-[>[data-align=block-start]]:flex-col has-[>textarea]:h-auto dark:bg-input/30 dark:has-disabled:bg-input/80 dark:has-[[data-slot][aria-invalid=true]]:ring-destructive/40 has-[>[data-align=block-end]]:[&>input]:pt-3 has-[>[data-align=block-start]]:[&>input]:pb-3 has-[>[data-align=inline-end]]:[&>input]:pr-1.5 has-[>[data-align=inline-start]]:[&>input]:pl-1.5',
        className
      )}
      {...props}
    />
  );
}

const inputGroupAddonVariants = cva(
  "text-muted-foreground h-auto gap-2 py-1.5 text-sm font-medium group-data-[disabled=true]/input-group:opacity-50 [&>kbd]:rounded-[calc(var(--radius)-5px)] [&>svg:not([class*='size-'])]:size-4 flex cursor-text items-center justify-center select-none",
  {
    variants: {
      align: {
        'inline-start':
          'pl-2 has-[>button]:ml-[-0.3rem] has-[>kbd]:ml-[-0.15rem] order-first',
        'inline-end':
          'pr-2 has-[>button]:mr-[-0.3rem] has-[>kbd]:mr-[-0.15rem] order-last',
        'block-start':
          'px-2.5 pt-2 group-has-[>input]/input-group:pt-2 [.border-b]:pb-2 order-first w-full justify-start',
        'block-end':
          'px-2.5 pb-2 group-has-[>input]/input-group:pb-2 [.border-t]:pt-2 order-last w-full justify-start',
      },
    },
    defaultVariants: {
      align: 'inline-start',
    },
  }
);

function InputGroupAddon({
  className,
  align = 'inline-start',
  ...props
}: React.ComponentProps<'div'> & VariantProps<typeof inputGroupAddonVariants>) {
  return (
    <div
      role='group'
      data-slot='input-group-addon'
      data-align={align}
      className={cn(inputGroupAddonVariants({ align }), className)}
      onClick={(e) => {
        if ((e.target as HTMLElement).closest('button')) {
          return;
        }
        e.currentTarget.parentElement?.querySelector('input')?.focus();
      }}
      {...props}
    />
  );
}

const inputGroupButtonVariants = cva(
  'gap-2 text-sm shadow-none flex items-center',
  {
    variants: {
      size: {
        xs: "h-6 gap-1 rounded-[calc(var(--radius)-3px)] px-1.5 [&>svg:not([class*='size-'])]:size-3.5",
        sm: '',
        'icon-xs':
          'size-6 rounded-[calc(var(--radius)-3px)] p-0 has-[>svg]:p-0',
        'icon-sm': 'size-8 p-0 has-[>svg]:p-0',
      },
    },
    defaultVariants: {
      size: 'xs',
    },
  }
);

function InputGroupButton({
  className,
  type = 'button',
  variant = 'ghost',
  size = 'xs',
  ...props
}: Omit<React.ComponentProps<typeof Button>, 'size' | 'type'> &
  VariantProps<typeof inputGroupButtonVariants> & {
    type?: 'button' | 'submit' | 'reset';
  }) {
  return (
    <Button
      type={type}
      data-size={size}
      variant={variant}
      className={cn(inputGroupButtonVariants({ size }), className)}
      {...props}
    />
  );
}

function InputGroupText({ className, ...props }: React.ComponentProps<'span'>) {
  return (
    <span
      className={cn(
        "flex items-center gap-2 text-sm text-muted-foreground [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    />
  );
}

function InputGroupInput({
  className,
  ...props
}: React.ComponentProps<'input'>) {
  return (
    <Input
      data-slot='input-group-control'
      className={cn(
        'flex-1 rounded-none border-0 bg-transparent shadow-none ring-0 focus-visible:ring-0 disabled:bg-transparent aria-invalid:ring-0 dark:bg-transparent dark:disabled:bg-transparent',
        className
      )}
      {...props}
    />
  );
}

function InputGroupTextarea({
  className,
  ...props
}: React.ComponentProps<'textarea'>) {
  return (
    <Textarea
      data-slot='input-group-control'
      className={cn(
        'flex-1 resize-none rounded-none border-0 bg-transparent py-2 shadow-none ring-0 focus-visible:ring-0 disabled:bg-transparent aria-invalid:ring-0 dark:bg-transparent dark:disabled:bg-transparent',
        className
      )}
      {...props}
    />
  );
}

export {
  InputGroup,
  InputGroupAddon,
  InputGroupButton,
  InputGroupText,
  InputGroupInput,
  InputGroupTextarea,
};
```
---

## Label <a name="label"></a>

A text label for form inputs that provides accessible labeling.

- **Registry Key**: `label`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/label.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Input } from '@/registry/default/ui/input';
import { Label } from '@/registry/default/ui/label';

export function LabelDefault() {
  return (
    <div className='flex w-64 flex-col gap-2'>
      <Label htmlFor='email'>Email</Label>
      <Input id='email' type='email' placeholder='you@example.com' />
    </div>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/label.tsx`
```tsx
'use client';

import * as React from 'react';

import { cn } from '@/lib/utils';

function Label({ className, ...props }: React.ComponentProps<'label'>) {
  return (
    <label
      data-slot='label'
      className={cn(
        'flex items-center gap-2 text-sm leading-none font-medium select-none group-data-[disabled=true]:pointer-events-none group-data-[disabled=true]:opacity-50 peer-disabled:cursor-not-allowed peer-disabled:opacity-50',
        className
      )}
      {...props}
    />
  );
}

export { Label };
```
---

## Liquid Metal Avatar <a name="liquid-metal-avatar"></a>

An avatar with an animated liquid metal shader effect.

- **Registry Key**: `liquid-metal-avatar`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/liquid-metal-avatar.json
```

#### Dependencies
- **NPM Packages**:
  - `@paper-design/shaders-react`

#### How to Use
##### Example: `custom`
```tsx
import {
  AvatarFallback,
  AvatarImage,
  LiquidMetalAvatar,
} from '@/registry/default/ui/liquid-metal-avatar';

export function LiquidMetalAvatarCustom() {
  return (
    <LiquidMetalAvatar
      size='lg'
      speed={0.8}
      repetition={8}
      softness={0.7}
      shiftRed={0.5}
      shiftBlue={0.1}
    >
      <AvatarImage
        src='https://images.unsplash.com/photo-1543610892-0b1f7e6d8ac1?w=128&h=128&dpr=2&q=80'
        alt='Avatar'
      />
      <AvatarFallback>AB</AvatarFallback>
    </LiquidMetalAvatar>
  );
}
```

##### Example: `default`
```tsx
import {
  AvatarFallback,
  AvatarImage,
  LiquidMetalAvatar,
} from '@/registry/default/ui/liquid-metal-avatar';

export function LiquidMetalAvatarDefault() {
  return (
    <LiquidMetalAvatar>
      <AvatarImage
        src='https://images.unsplash.com/photo-1543610892-0b1f7e6d8ac1?w=128&h=128&dpr=2&q=80'
        alt='Avatar'
      />
      <AvatarFallback>JD</AvatarFallback>
    </LiquidMetalAvatar>
  );
}
```

#### Props & API Reference
| Prop         | Type                   | Default | Description                               |
| ------------ | ---------------------- | ------- | ----------------------------------------- |
| `children`   | `ReactNode`            | -       | Avatar content (`AvatarImage`, `AvatarFallback`). |
| `size`       | `'sm' \| 'md' \| 'lg'` | `'md'`  | The size of the avatar.                   |
| `speed`      | `number`               | `0.4`   | Animation speed of the shader effect.     |
| `repetition` | `number`               | `6`     | Number of pattern repetitions.            |
| `softness`   | `number`               | `0.5`   | Softness of the metallic highlights.      |
| `shiftRed`   | `number`               | `0.3`   | Red color shift amount.                   |
| `shiftBlue`  | `number`               | `0.3`   | Blue color shift amount.                  |
| `distortion` | `number`               | `0`     | Distortion amount applied to the pattern. |
| `contour`    | `number`               | `0`     | Contour intensity.                        |
| `angle`      | `number`               | `45`    | Angle of the shader pattern in degrees.   |
| `scale`      | `number`               | `4`     | Scale of the shader pattern.              |
| `offsetX`    | `number`               | `0.1`   | Horizontal offset of the pattern.         |
| `offsetY`    | `number`               | `-0.1`  | Vertical offset of the pattern.           |

#### Component Source Code
##### File: `registry/default/ui/liquid-metal-avatar.tsx`
```tsx
'use client';

import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from '@/registry/default/ui/avatar';
import { LiquidMetal, LiquidMetalProps } from '@paper-design/shaders-react';
import { cva, type VariantProps } from 'class-variance-authority';

import { cn } from '@/lib/utils';

const avatarVariants = cva(
  'relative flex items-center justify-center rounded-full overflow-visible bg-transparent',
  {
    variants: {
      size: {
        sm: 'size-8',
        md: 'size-12',
        lg: 'size-16',
      },
    },
    defaultVariants: { size: 'md' },
  }
);

const innerVariants = cva(
  'absolute rounded-full bg-primary-foreground inset-shadow-lg',
  {
    variants: {
      size: {
        sm: 'inset-[1px]',
        md: 'inset-[2px]',
        lg: 'inset-[3px]',
      },
    },
    defaultVariants: { size: 'md' },
  }
);

type ShaderProps = Omit<LiquidMetalProps, 'className' | 'style' | 'shape'>;

type LiquidMetalAvatarProps = React.ComponentProps<'div'> &
  Partial<ShaderProps> &
  VariantProps<typeof avatarVariants>;

function LiquidMetalAvatar({
  size,
  className,
  children,
  speed = 0.6,
  repetition = 4,
  softness = 0.5,
  shiftRed = 0.3,
  shiftBlue = 0.3,
  distortion = 0,
  contour = 0,
  angle = 45,
  scale = 8,
  offsetX = 0.1,
  offsetY = -0.1,
  ...props
}: LiquidMetalAvatarProps) {
  return (
    <div
      data-slot='liquid-metal-avatar'
      className={cn(avatarVariants({ size }), className)}
      {...props}
    >
      <LiquidMetal
        className='absolute inset-0 rounded-full'
        shape='none'
        speed={speed}
        repetition={repetition}
        softness={softness}
        shiftRed={shiftRed}
        shiftBlue={shiftBlue}
        distortion={distortion}
        contour={contour}
        angle={angle}
        scale={scale}
        offsetX={offsetX}
        offsetY={offsetY}
      />
      <Avatar
        className={cn(innerVariants({ size }), 'size-auto bg-transparent')}
      >
        {children}
      </Avatar>
    </div>
  );
}

export { LiquidMetalAvatar, AvatarImage, AvatarFallback };
```
---

## Liquid Metal Button <a name="liquid-metal-button"></a>

A button with an animated liquid metal shader effect.

- **Registry Key**: `liquid-metal-button`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/liquid-metal-button.json
```

#### Dependencies
- **NPM Packages**:
  - `@paper-design/shaders-react`

#### How to Use
##### Example: `custom`
```tsx
import { LiquidMetalButton } from '@/registry/default/ui/liquid-metal-button';

export function LiquidMetalButtonCustom() {
  return (
    <LiquidMetalButton
      speed={1.2}
      repetition={6}
      softness={0.8}
      shiftRed={0.5}
      shiftBlue={0.1}
    >
      Custom Effect
    </LiquidMetalButton>
  );
}
```

##### Example: `default`
```tsx
import { LiquidMetalButton } from '@/registry/default/ui/liquid-metal-button';

export function LiquidMetalButtonDefault() {
  return <LiquidMetalButton>Click me</LiquidMetalButton>;
}
```

#### Props & API Reference
| Prop         | Type                   | Default | Description                               |
| ------------ | ---------------------- | ------- | ----------------------------------------- |
| `size`       | `'sm' \| 'md' \| 'lg'` | `'md'`  | The size of the button.                   |
| `speed`      | `number`               | `0.6`   | Animation speed of the shader effect.     |
| `repetition` | `number`               | `4`     | Number of pattern repetitions.            |
| `softness`   | `number`               | `0.5`   | Softness of the metallic highlights.      |
| `shiftRed`   | `number`               | `0.3`   | Red color shift amount.                   |
| `shiftBlue`  | `number`               | `0.3`   | Blue color shift amount.                  |
| `distortion` | `number`               | `0`     | Distortion amount applied to the pattern. |
| `contour`    | `number`               | `0`     | Contour intensity.                        |
| `angle`      | `number`               | `45`    | Angle of the shader pattern in degrees.   |
| `scale`      | `number`               | `8`     | Scale of the shader pattern.              |
| `offsetX`    | `number`               | `0.1`   | Horizontal offset of the pattern.         |
| `offsetY`    | `number`               | `-0.1`  | Vertical offset of the pattern.           |

#### Component Source Code
##### File: `registry/default/ui/liquid-metal-button.tsx`
```tsx
'use client';

import { Button } from '@/registry/default/ui/button';
import { LiquidMetal, LiquidMetalProps } from '@paper-design/shaders-react';
import { cva, type VariantProps } from 'class-variance-authority';

import { cn } from '@/lib/utils';

const buttonVariants = cva(
  'relative flex items-center justify-center bg-transparent border-none cursor-pointer outline-none rounded-full overflow-hidden font-medium whitespace-nowrap text-primary',
  {
    variants: {
      size: {
        sm: 'min-w-28 h-9 px-4 text-xs',
        md: 'min-w-35.5 h-11.5 px-5 text-sm',
        lg: 'min-w-44 h-14 px-6 text-base',
      },
    },
    defaultVariants: { size: 'md' },
  }
);

type ShaderProps = Omit<LiquidMetalProps, 'className' | 'style' | 'shape'>;

type LiquidMetalButtonProps = Omit<
  React.ComponentProps<typeof Button>,
  'variant' | 'size'
> &
  Partial<ShaderProps> &
  VariantProps<typeof buttonVariants>;

export function LiquidMetalButton({
  children,
  size,
  className,
  ref,
  speed = 0.6,
  repetition = 4,
  softness = 0.5,
  shiftRed = 0.3,
  shiftBlue = 0.3,
  distortion = 0,
  contour = 0,
  angle = 45,
  scale = 8,
  offsetX = 0.1,
  offsetY = -0.1,
  ...props
}: LiquidMetalButtonProps) {
  return (
    <Button
      ref={ref}
      data-slot='liquid-metal-button'
      className={cn(buttonVariants({ size }), className)}
      {...props}
    >
      <LiquidMetal
        className='absolute inset-0 rounded-full'
        shape='none'
        speed={speed}
        repetition={repetition}
        softness={softness}
        shiftRed={shiftRed}
        shiftBlue={shiftBlue}
        distortion={distortion}
        contour={contour}
        angle={angle}
        scale={scale}
        offsetX={offsetX}
        offsetY={offsetY}
      />
      <div className='absolute inset-[calc(var(--spacing)*0.68)] rounded-full bg-primary-foreground inset-shadow-lg' />
      <span className='relative'>{children}</span>
    </Button>
  );
}
```
---

## Liquid Metal Card <a name="liquid-metal-card"></a>

A card with an animated liquid metal shader effect.

- **Registry Key**: `liquid-metal-card`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/liquid-metal-card.json
```

#### Dependencies
- **NPM Packages**:
  - `@paper-design/shaders-react`

#### How to Use
##### Example: `custom`
```tsx
import { Badge } from '@/registry/default/ui/badge';
import { Button } from '@/registry/default/ui/button';
import {
  CardAction,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
  LiquidMetalCard,
} from '@/registry/default/ui/liquid-metal-card';

export function LiquidMetalCardCustom() {
  return (
    <LiquidMetalCard
      className='w-full max-w-sm'
      speed={1.2}
      repetition={6}
      softness={0.8}
      shiftRed={0.5}
      shiftBlue={0.1}
    >
      <CardHeader>
        <CardTitle>Notifications</CardTitle>
        <CardDescription>You have 3 unread messages.</CardDescription>
        <CardAction>
          <Badge variant='secondary'>New</Badge>
        </CardAction>
      </CardHeader>
      <CardContent>
        <p className='text-sm text-muted-foreground'>
          Customize the liquid metal effect with props like speed, repetition,
          softness, and color shifts.
        </p>
      </CardContent>
      <CardFooter>
        <Button className='w-full'>Mark all as read</Button>
      </CardFooter>
    </LiquidMetalCard>
  );
}
```

##### Example: `default`
```tsx
import { Button } from '@/registry/default/ui/button';
import {
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
  LiquidMetalCard,
} from '@/registry/default/ui/liquid-metal-card';

export function LiquidMetalCardDefault() {
  return (
    <LiquidMetalCard className='w-full max-w-sm'>
      <CardHeader>
        <CardTitle>Create project</CardTitle>
        <CardDescription>Deploy your new project in one-click.</CardDescription>
      </CardHeader>
      <CardContent>
        <p className='text-sm text-muted-foreground'>
          Your new project will be created with the default settings. You can
          customize it later in the project settings.
        </p>
      </CardContent>
      <CardFooter className='flex justify-between'>
        <Button variant='outline'>Cancel</Button>
        <Button>Deploy</Button>
      </CardFooter>
    </LiquidMetalCard>
  );
}
```

#### Props & API Reference
| Prop         | Type     | Default | Description                               |
| ------------ | -------- | ------- | ----------------------------------------- |
| `speed`      | `number` | `0.6`   | Animation speed of the shader effect.     |
| `repetition` | `number` | `4`     | Number of pattern repetitions.            |
| `softness`   | `number` | `0.5`   | Softness of the metallic highlights.      |
| `shiftRed`   | `number` | `0.3`   | Red color shift amount.                   |
| `shiftBlue`  | `number` | `0.3`   | Blue color shift amount.                  |
| `distortion` | `number` | `0`     | Distortion amount applied to the pattern. |
| `contour`    | `number` | `0`     | Contour intensity.                        |
| `angle`      | `number` | `45`    | Angle of the shader pattern in degrees.   |
| `scale`      | `number` | `8`     | Scale of the shader pattern.              |
| `offsetX`    | `number` | `0.1`   | Horizontal offset of the pattern.         |
| `offsetY`    | `number` | `-0.1`  | Vertical offset of the pattern.           |

#### Component Source Code
##### File: `registry/default/ui/liquid-metal-card.tsx`
```tsx
'use client';

import {
  Card,
  CardAction,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/registry/default/ui/card';
import { LiquidMetal, LiquidMetalProps } from '@paper-design/shaders-react';

import { cn } from '@/lib/utils';

type ShaderProps = Omit<LiquidMetalProps, 'className' | 'style' | 'shape'>;

type LiquidMetalCardProps = React.ComponentProps<typeof Card> &
  Partial<ShaderProps>;

function LiquidMetalCard({
  className,
  children,
  speed = 0.6,
  repetition = 4,
  softness = 0.5,
  shiftRed = 0.3,
  shiftBlue = 0.3,
  distortion = 0,
  contour = 0,
  angle = 45,
  scale = 8,
  offsetX = 0.1,
  offsetY = -0.1,
  ...props
}: LiquidMetalCardProps) {
  return (
    <Card
      data-slot='liquid-metal-card'
      className={cn(
        'relative overflow-hidden bg-transparent ring-0',
        className
      )}
      {...props}
    >
      <LiquidMetal
        className='absolute inset-0 rounded-2xl'
        shape='none'
        speed={speed}
        repetition={repetition}
        softness={softness}
        shiftRed={shiftRed}
        shiftBlue={shiftBlue}
        distortion={distortion}
        contour={contour}
        angle={angle}
        scale={scale}
        offsetX={offsetX}
        offsetY={offsetY}
      />
      <div className='absolute inset-0.75 rounded-[calc(1rem-1px)] bg-card inset-shadow-lg' />
      <div className='relative flex flex-col gap-6 group-data-[size=sm]/card:gap-4'>
        {children}
      </div>
    </Card>
  );
}

export {
  LiquidMetalCard,
  CardHeader,
  CardFooter,
  CardTitle,
  CardAction,
  CardDescription,
  CardContent,
};
```
---

## Mesh Gradient Card <a name="mesh-gradient-card"></a>

A card with an animated mesh gradient shader effect that cycles through configurable steps.

- **Registry Key**: `mesh-gradient-card`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/mesh-gradient-card.json
```

#### Dependencies
- **NPM Packages**:
  - `@paper-design/shaders-react`
  - `motion`

#### How to Use
##### Example: `custom-steps`
```tsx
import {
  MeshGradientCard,
  MeshGradientCardMessage,
  MeshGradientCardMessages,
  type GradientStep,
} from '@/registry/default/ui/mesh-gradient-card';

const steps: GradientStep[] = [
  {
    distortion: 0.8,
    swirl: 0.1,
    grainMixer: 0.2,
    grainOverlay: 0.15,
    speed: 0.7,
    scale: 1.3,
    rotation: 10,
    offsetX: 0.1,
    offsetY: -0.05,
  },
  {
    distortion: 0.1,
    swirl: 0.9,
    grainMixer: 0.05,
    grainOverlay: 0.08,
    speed: 0.4,
    scale: 0.85,
    rotation: -15,
    offsetX: -0.1,
    offsetY: 0.1,
  },
  {
    distortion: 0.45,
    swirl: 0.45,
    grainMixer: 0.12,
    grainOverlay: 0.12,
    speed: 0.55,
    scale: 1.1,
    rotation: 20,
    offsetX: 0.05,
    offsetY: 0.05,
  },
];

const messages = ['High distortion.', 'Deep swirl.', 'Balanced flow.'];

export function MeshGradientCardCustomSteps() {
  return (
    <MeshGradientCard
      steps={steps}
      interval={2000}
      className='aspect-video w-full max-w-md'
    >
      <MeshGradientCardMessages>
        {messages.map((msg) => (
          <MeshGradientCardMessage key={msg}>{msg}</MeshGradientCardMessage>
        ))}
      </MeshGradientCardMessages>
    </MeshGradientCard>
  );
}
```

##### Example: `custom`
```tsx
import {
  MeshGradientCard,
  MeshGradientCardMessage,
  MeshGradientCardMessages,
} from '@/registry/default/ui/mesh-gradient-card';

const messages = [
  'Forest calm.',
  'Emerald glow.',
  'Green pulse.',
  'Leaf drift.',
  'Deep canopy.',
  'Jade shimmer.',
  'Moss light.',
];

export function MeshGradientCardCustom() {
  return (
    <MeshGradientCard
      colors={['#ecfdf5', '#059669', '#d1fae5', '#047857']}
      className='aspect-video w-full max-w-md'
    >
      <MeshGradientCardMessages>
        {messages.map((msg) => (
          <MeshGradientCardMessage key={msg}>{msg}</MeshGradientCardMessage>
        ))}
      </MeshGradientCardMessages>
    </MeshGradientCard>
  );
}
```

#### Props & API Reference
| Prop           | Type             | Default                                        | Description                                      |
| -------------- | ---------------- | ---------------------------------------------- | ------------------------------------------------ |
| `colors`       | `string[]`       | `['#fafafa', '#18181b', '#fafafa', '#18181b']` | Array of colors for the mesh gradient.           |
| `steps`        | `GradientStep[]` | 7 built-in steps                               | Array of gradient step configurations to cycle.  |
| `interval`     | `number`         | `4000`                                         | Time in ms between gradient step transitions.    |
| `distortion`   | `number`         | per step                                       | Distortion amount applied to the gradient.       |
| `swirl`        | `number`         | per step                                       | Swirl intensity of the gradient.                 |
| `grainMixer`   | `number`         | per step                                       | Grain mixer intensity.                           |
| `grainOverlay` | `number`         | per step                                       | Grain overlay intensity.                         |
| `speed`        | `number`         | per step                                       | Animation speed of the shader effect.            |
| `scale`        | `number`         | per step                                       | Scale of the gradient pattern.                   |
| `rotation`     | `number`         | per step                                       | Rotation angle of the gradient in degrees.       |
| `offsetX`      | `number`         | per step                                       | Horizontal offset of the gradient.               |
| `offsetY`      | `number`         | per step                                       | Vertical offset of the gradient.                 |

#### Component Source Code
##### File: `registry/default/ui/mesh-gradient-card.tsx`
```tsx
'use client';

import React, {
  createContext,
  useCallback,
  useContext,
  useEffect,
  useRef,
  useState,
} from 'react';

import {
  MeshGradient,
  type MeshGradientParams,
  type MeshGradientProps,
} from '@paper-design/shaders-react';
import { animate, AnimatePresence, motion } from 'motion/react';

import { cn } from '@/lib/utils';

type GradientStep = Required<
  Pick<
    MeshGradientParams,
    | 'distortion'
    | 'swirl'
    | 'grainMixer'
    | 'grainOverlay'
    | 'speed'
    | 'scale'
    | 'rotation'
    | 'offsetX'
    | 'offsetY'
  >
>;

const DEFAULT_STEPS: GradientStep[] = [
  {
    distortion: 0.3,
    swirl: 0.1,
    grainMixer: 0.16,
    grainOverlay: 0.15,
    speed: 0.45,
    scale: 1.0,
    rotation: 0,
    offsetX: 0,
    offsetY: 0,
  },
  {
    distortion: 0.5,
    swirl: 0.3,
    grainMixer: 0.1,
    grainOverlay: 0.1,
    speed: 0.55,
    scale: 1.2,
    rotation: 15,
    offsetX: 0.1,
    offsetY: -0.05,
  },
  {
    distortion: 0.4,
    swirl: 0.8,
    grainMixer: 0.2,
    grainOverlay: 0.1,
    speed: 0.38,
    scale: 0.9,
    rotation: -10,
    offsetX: -0.15,
    offsetY: 0.1,
  },
  {
    distortion: 0.9,
    swirl: 0.5,
    grainMixer: 0.12,
    grainOverlay: 0.2,
    speed: 0.65,
    scale: 1.4,
    rotation: 25,
    offsetX: 0.05,
    offsetY: -0.1,
  },
  {
    distortion: 0.25,
    swirl: 0.2,
    grainMixer: 0.6,
    grainOverlay: 0.5,
    speed: 0.5,
    scale: 1.1,
    rotation: -5,
    offsetX: -0.05,
    offsetY: 0.05,
  },
  {
    distortion: 0.6,
    swirl: 0.9,
    grainMixer: 0.08,
    grainOverlay: 0.05,
    speed: 0.3,
    scale: 0.8,
    rotation: 35,
    offsetX: 0.12,
    offsetY: 0.08,
  },
  {
    distortion: 0.15,
    swirl: 0.05,
    grainMixer: 0.5,
    grainOverlay: 0.4,
    speed: 0.6,
    scale: 1.3,
    rotation: -20,
    offsetX: -0.1,
    offsetY: -0.08,
  },
];

const MeshGradientCardContext = createContext<{ messageIndex: number }>({
  messageIndex: 0,
});

type MeshGradientCardProps = Partial<
  Omit<MeshGradientProps, 'width' | 'height'>
> & {
  steps?: GradientStep[];
  interval?: number;
  children?: React.ReactNode;
};

function MeshGradientCardRoot({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='mesh-gradient-card'
      className={cn('relative overflow-hidden rounded-4xl', className)}
      {...props}
    />
  );
}

function MeshGradientCard({
  className,
  children,
  colors = ['#fafafa', '#18181b', '#fafafa', '#18181b'],
  steps = DEFAULT_STEPS,
  interval = 4000,
  ...shaderProps
}: MeshGradientCardProps) {
  const { config, messageIndex } = useGradientCycler(steps, interval);

  return (
    <MeshGradientCardContext.Provider value={{ messageIndex }}>
      <MeshGradientCardRoot className={className}>
        <MeshGradient
          width='100%'
          height='100%'
          colors={colors}
          {...config}
          {...shaderProps}
        />
        {children}
      </MeshGradientCardRoot>
    </MeshGradientCardContext.Provider>
  );
}

function MeshGradientCardMessages({
  children,
  className,
}: {
  children?: React.ReactNode;
  className?: string;
}) {
  const { messageIndex } = useContext(MeshGradientCardContext);
  const items = React.Children.toArray(children);
  const active = items[messageIndex % items.length];

  return (
    <span data-slot='mesh-gradient-card-messages' className={className}>
      <AnimatePresence mode='wait'>
        {React.isValidElement(active)
          ? React.cloneElement(active, {
              key: messageIndex,
            } as React.Attributes)
          : active}
      </AnimatePresence>
    </span>
  );
}

function MeshGradientCardMessage({
  children,
  className,
}: {
  children?: React.ReactNode;
  className?: string;
}) {
  return (
    <motion.span
      data-slot='mesh-gradient-card-message'
      initial={{ opacity: 0, filter: 'blur(8px)' }}
      animate={{ opacity: 1, filter: 'blur(0px)' }}
      exit={{ opacity: 0, filter: 'blur(8px)' }}
      transition={{ duration: 0.45, ease: 'easeInOut' }}
      style={{
        textShadow: '0 1px 4px rgba(0,0,0,0.3), 0 4px 12px rgba(0,0,0,0.2)',
      }}
      className={cn(
        'absolute right-5 bottom-4 pb-1 text-lg font-medium text-white select-none',
        className
      )}
    >
      {children}
    </motion.span>
  );
}

function useGradientCycler(
  steps: GradientStep[] = DEFAULT_STEPS,
  interval: number = 4000
) {
  const [stepIndex, setStepIndex] = useState(0);
  const [config, setConfig] = useState<GradientStep>(steps[0]);
  const animationRef = useRef<ReturnType<typeof animate> | null>(null);
  const [messageIndex, setMessageIndex] = useState(0);

  const goToStep = useCallback(
    (n: number) => {
      if (n === stepIndex) return;
      animationRef.current?.stop();

      const from = { ...config };
      const to = steps[n];

      animationRef.current = animate(from, to, {
        duration: 0.6,
        ease: 'easeOut',
        onUpdate: () => setConfig({ ...from }),
      });

      setStepIndex(n);
      setMessageIndex((i) => i + 1);
    },
    [stepIndex, config, steps]
  );

  useEffect(() => {
    const id = setTimeout(() => {
      goToStep((stepIndex + 1) % steps.length);
    }, interval);
    return () => clearTimeout(id);
  }, [stepIndex, goToStep, steps.length, interval]);

  return { config, stepIndex, messageIndex, goToStep };
}

export {
  MeshGradientCard,
  MeshGradientCardMessage,
  MeshGradientCardMessages,
  MeshGradientCardRoot,
  useGradientCycler,
};

export type { GradientStep, MeshGradientCardProps };
```
---

## Meter <a name="meter"></a>

A visual indicator that displays a value within a known range.

- **Registry Key**: `meter`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/meter.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Meter } from '@/registry/default/ui/meter';

export function MeterDefault() {
  return <Meter value={60} />;
}
```

##### Example: `with-label`
```tsx
'use client';

import {
  Meter,
  MeterIndicator,
  MeterLabel,
  MeterTrack,
  MeterValue,
} from '@/registry/default/ui/meter';

export function MeterWithLabel() {
  return (
    <Meter value={75}>
      <MeterLabel>Storage Used</MeterLabel>
      <MeterValue />
      <MeterTrack>
        <MeterIndicator />
      </MeterTrack>
    </Meter>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/meter.tsx`
```tsx
import { Meter as MeterPrimitive } from '@base-ui/react/meter';

import { cn } from '@/lib/utils';

function Meter({ className, children, ...props }: MeterPrimitive.Root.Props) {
  return (
    <MeterPrimitive.Root
      data-slot='meter'
      className={cn('box-border grid w-48 grid-cols-2 gap-y-2', className)}
      {...props}
    >
      {children ? (
        children
      ) : (
        <MeterTrack>
          <MeterIndicator />
        </MeterTrack>
      )}
    </MeterPrimitive.Root>
  );
}

function MeterLabel({ className, ...props }: MeterPrimitive.Label.Props) {
  return (
    <MeterPrimitive.Label
      data-slot='meter-label'
      className={cn('text-sm font-medium', className)}
      {...props}
    />
  );
}

function MeterTrack({ className, ...props }: MeterPrimitive.Track.Props) {
  return (
    <MeterPrimitive.Track
      data-slot='meter-track'
      className={cn(
        'col-span-2 block h-2 w-48 overflow-hidden bg-input',
        className
      )}
      {...props}
    />
  );
}

function MeterIndicator({
  className,
  ...props
}: MeterPrimitive.Indicator.Props) {
  return (
    <MeterPrimitive.Indicator
      data-slot='meter-indicator'
      className={cn('block bg-primary transition-all duration-500', className)}
      {...props}
    />
  );
}

function MeterValue({ className, ...props }: MeterPrimitive.Value.Props) {
  return (
    <MeterPrimitive.Value
      data-slot='meter-value'
      className={cn('col-start-2 m-0 text-right text-sm leading-5', className)}
      {...props}
    />
  );
}

export { Meter, MeterLabel, MeterTrack, MeterIndicator, MeterValue };
```
---

## Navigation Menu <a name="navigation-menu"></a>

A collection of links and menus for website navigation.

- **Registry Key**: `navigation-menu`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/navigation-menu.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `lucide-react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuIcon,
  NavigationMenuItem,
  NavigationMenuLink,
  NavigationMenuList,
  NavigationMenuPositioner,
  NavigationMenuTrigger,
} from '@/registry/default/ui/navigation-menu';

const overviewLinks = [
  {
    href: '#',
    title: 'Quick Start',
    description: 'Install and assemble your first component.',
  },
  {
    href: '#',
    title: 'Accessibility',
    description: 'Learn how we build accessible components.',
  },
  {
    href: '#',
    title: 'Releases',
    description: "See what's new in the latest versions.",
  },
  {
    href: '#',
    title: 'About',
    description: 'Learn more about the project and our mission.',
  },
] as const;

const handbookLinks = [
  {
    href: '#',
    title: 'Styling',
    description:
      'Components can be styled with plain CSS, Tailwind CSS, CSS-in-JS, or CSS Modules.',
  },
  {
    href: '#',
    title: 'Animation',
    description:
      'Components can be animated with CSS transitions, CSS animations, or JavaScript libraries.',
  },
  {
    href: '#',
    title: 'Composition',
    description:
      'Components can be replaced and composed with your own existing components.',
  },
] as const;

export function NavigationMenuDefault() {
  return (
    <NavigationMenu>
      <NavigationMenuList>
        <NavigationMenuItem>
          <NavigationMenuTrigger>
            Overview <NavigationMenuIcon />
          </NavigationMenuTrigger>

          <NavigationMenuContent className='w-max min-w-100'>
            <ul className='grid list-none grid-cols-[12rem_12rem] gap-0'>
              {overviewLinks.map((item) => (
                <li key={item.title}>
                  <NavigationMenuLink href={item.href}>
                    <h3 className='m-0 mb-1 text-base leading-5 font-medium'>
                      {item.title}
                    </h3>
                    <p className='m-0 text-sm leading-5 text-muted-foreground'>
                      {item.description}
                    </p>
                  </NavigationMenuLink>
                </li>
              ))}
            </ul>
          </NavigationMenuContent>
        </NavigationMenuItem>

        <NavigationMenuItem>
          <NavigationMenuTrigger>
            Handbook <NavigationMenuIcon />
          </NavigationMenuTrigger>

          <NavigationMenuContent className='w-max min-w-100'>
            <ul className='flex max-w-100 flex-col justify-center'>
              {handbookLinks.map((item) => (
                <li key={item.title}>
                  <NavigationMenuLink href={item.href}>
                    <h3 className='m-0 mb-1 text-base leading-5 font-medium'>
                      {item.title}
                    </h3>
                    <p className='m-0 text-sm leading-5 text-muted-foreground'>
                      {item.description}
                    </p>
                  </NavigationMenuLink>
                </li>
              ))}
            </ul>
          </NavigationMenuContent>
        </NavigationMenuItem>

        <NavigationMenuItem>
          <NavigationMenuLink
            className='flex h-10 items-center justify-center rounded-md px-3.5 text-base font-medium hover:bg-accent'
            href='#'
          >
            GitHub
          </NavigationMenuLink>
        </NavigationMenuItem>
      </NavigationMenuList>
      <NavigationMenuPositioner />
    </NavigationMenu>
  );
}
```

##### Example: `nested`
```tsx
'use client';

import {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuIcon,
  NavigationMenuItem,
  NavigationMenuLink,
  NavigationMenuList,
  NavigationMenuPositioner,
  NavigationMenuTrigger,
} from '@/registry/default/ui/navigation-menu';
import { ChevronRightIcon } from 'lucide-react';

const overviewLinks = [
  {
    href: '#',
    title: 'Quick Start',
    description: 'Install and assemble your first component.',
  },
  {
    href: '#',
    title: 'Accessibility',
    description: 'Learn how we build accessible components.',
  },
  {
    href: '#',
    title: 'Releases',
    description: "See what's new in the latest versions.",
  },
] as const;

const handbookLinks = [
  {
    href: '#',
    title: 'Styling',
    description:
      'Components can be styled with plain CSS, Tailwind CSS, CSS-in-JS, or CSS Modules.',
  },
  {
    href: '#',
    title: 'Animation',
    description:
      'Components can be animated with CSS transitions, CSS animations, or JavaScript libraries.',
  },
  {
    href: '#',
    title: 'Composition',
    description:
      'Components can be replaced and composed with your own existing components.',
  },
] as const;

export function NavigationMenuNested() {
  return (
    <NavigationMenu>
      <NavigationMenuList>
        <NavigationMenuItem>
          <NavigationMenuTrigger>
            Overview <NavigationMenuIcon />
          </NavigationMenuTrigger>

          <NavigationMenuContent className='w-max min-w-100'>
            <ul className='grid list-none grid-cols-[12rem_12rem] gap-0'>
              {overviewLinks.map((item) => (
                <li key={item.title}>
                  <NavigationMenuLink href={item.href}>
                    <h3 className='m-0 mb-1 text-base leading-5 font-medium'>
                      {item.title}
                    </h3>
                    <p className='m-0 text-sm leading-5 text-muted-foreground'>
                      {item.description}
                    </p>
                  </NavigationMenuLink>
                </li>
              ))}
              <li>
                <NavigationMenu
                  orientation='vertical'
                  className='min-w-0 items-stretch rounded-none bg-transparent p-0'
                >
                  <NavigationMenuItem render={<div />}>
                    <NavigationMenuTrigger className='relative block h-auto w-full bg-transparent p-3 text-left text-inherit'>
                      <span className='m-0 mb-1 text-base leading-5 font-medium'>
                        Handbook
                      </span>
                      <p className='m-0 text-sm leading-5 text-muted-foreground'>
                        How to use components effectively.
                      </p>
                      <NavigationMenuIcon className='absolute top-1/2 right-2.5 flex size-2.5 -translate-y-1/2 items-center justify-center'>
                        <ChevronRightIcon className='size-3' />
                      </NavigationMenuIcon>
                    </NavigationMenuTrigger>
                    <NavigationMenuContent>
                      <ul className='flex max-w-100 flex-col justify-center'>
                        {handbookLinks.map((item) => (
                          <li key={item.title}>
                            <NavigationMenuLink href={item.href}>
                              <h3 className='m-0 mb-1 text-base leading-5 font-medium'>
                                {item.title}
                              </h3>
                              <p className='m-0 text-sm leading-5 text-muted-foreground'>
                                {item.description}
                              </p>
                            </NavigationMenuLink>
                          </li>
                        ))}
                      </ul>
                    </NavigationMenuContent>
                  </NavigationMenuItem>

                  <NavigationMenuPositioner
                    side='right'
                    sideOffset={24}
                    alignOffset={-24}
                    align='end'
                  />
                </NavigationMenu>
              </li>
            </ul>
          </NavigationMenuContent>
        </NavigationMenuItem>

        <NavigationMenuItem>
          <NavigationMenuLink
            className='flex h-10 items-center justify-center rounded-md px-3.5 text-base font-medium hover:bg-accent'
            href='#'
          >
            GitHub
          </NavigationMenuLink>
        </NavigationMenuItem>
      </NavigationMenuList>
      <NavigationMenuPositioner />
    </NavigationMenu>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/navigation-menu.tsx`
```tsx
'use client';

import * as React from 'react';

import { NavigationMenu as NavigationMenuPrimitive } from '@base-ui/react/navigation-menu';
import { ChevronDownIcon } from 'lucide-react';

import { cn } from '@/lib/utils';

function NavigationMenu({
  className,
  ...props
}: React.ComponentPropsWithRef<typeof NavigationMenuPrimitive.Root>) {
  return (
    <NavigationMenuPrimitive.Root
      data-slot='navigation-menu'
      className={cn(
        'group/navigation-menu relative flex min-w-max items-center justify-center rounded-xl bg-popover p-1 text-popover-foreground',
        className
      )}
      {...props}
    />
  );
}

function NavigationMenuList({
  className,
  ...props
}: React.ComponentPropsWithRef<typeof NavigationMenuPrimitive.List>) {
  return (
    <NavigationMenuPrimitive.List
      data-slot='navigation-menu-list'
      className={cn('not-prose relative flex', className)}
      {...props}
    />
  );
}

function NavigationMenuItem({
  className,
  ...props
}: React.ComponentPropsWithRef<typeof NavigationMenuPrimitive.Item>) {
  return (
    <NavigationMenuPrimitive.Item
      data-slot='navigation-menu-item'
      className={cn('relative', className)}
      {...props}
    />
  );
}

function NavigationMenuTrigger({
  className,
  ...props
}: NavigationMenuPrimitive.Trigger.Props) {
  return (
    <NavigationMenuPrimitive.Trigger
      data-slot='navigation-menu-trigger'
      className={cn(
        'm-0 box-border flex h-10 items-center justify-center gap-1.5 rounded-lg bg-popover px-3.5 text-base leading-6 font-medium text-popover-foreground no-underline select-none hover:bg-accent focus-visible:relative focus-visible:z-10 focus-visible:ring-[3px] focus-visible:ring-foreground/10 focus-visible:outline-none active:bg-accent data-popup-open:bg-accent',
        className
      )}
      {...props}
    />
  );
}

function NavigationMenuIcon({
  className,
  children,
  ...props
}: NavigationMenuPrimitive.Icon.Props) {
  return (
    <NavigationMenuPrimitive.Icon
      data-slot='navigation-menu-icon'
      className={cn(
        'transition-transform duration-200 ease-in-out data-popup-open:rotate-180',
        className
      )}
      {...props}
    >
      {children ?? <ChevronDownIcon className='size-3' />}
    </NavigationMenuPrimitive.Icon>
  );
}

function NavigationMenuContent({
  className,
  ...props
}: NavigationMenuPrimitive.Content.Props) {
  return (
    <NavigationMenuPrimitive.Content
      data-slot='navigation-menu-content'
      className={cn(
        'h-full p-6 transition-[opacity,transform,translate] duration-(--duration) ease-(--easing) data-ending-style:opacity-0 data-starting-style:opacity-0 data-ending-style:data-[activation-direction=left]:translate-x-[50%] data-starting-style:data-[activation-direction=left]:translate-x-[-50%] data-ending-style:data-[activation-direction=right]:translate-x-[-50%] data-starting-style:data-[activation-direction=right]:translate-x-[50%]',
        className
      )}
      {...props}
    />
  );
}

function NavigationMenuLink({
  className,
  ...props
}: NavigationMenuPrimitive.Link.Props) {
  return (
    <NavigationMenuPrimitive.Link
      data-slot='navigation-menu-link'
      className={cn(
        'block rounded-lg p-3 text-inherit no-underline hover:bg-accent focus-visible:relative focus-visible:z-10 focus-visible:ring-[3px] focus-visible:ring-foreground/10 focus-visible:outline-none data-active:bg-accent',
        className
      )}
      {...props}
    />
  );
}

function NavigationMenuBackdrop({
  className,
  ...props
}: NavigationMenuPrimitive.Backdrop.Props) {
  return (
    <NavigationMenuPrimitive.Backdrop
      data-slot='navigation-menu-backdrop'
      className={cn(
        'fixed inset-0 transition-opacity data-ending-style:opacity-0 data-starting-style:opacity-0',
        className
      )}
      {...props}
    />
  );
}

function NavigationMenuPortal({
  ...props
}: NavigationMenuPrimitive.Portal.Props) {
  return (
    <NavigationMenuPrimitive.Portal
      data-slot='navigation-menu-portal'
      {...props}
    />
  );
}

function NavigationMenuPositioner({
  className,
  sideOffset = 12,
  style: styleProp,
  ...props
}: NavigationMenuPrimitive.Positioner.Props) {
  const defaultStyle = {
    ['--duration' as string]: '0.35s',
    ['--easing' as string]: 'cubic-bezier(0.22, 1, 0.36, 1)',
  };

  return (
    <NavigationMenuPrimitive.Portal>
      <NavigationMenuPrimitive.Positioner
        data-slot='navigation-menu-positioner'
        sideOffset={sideOffset}
        className={cn(
          "box-border h-(--positioner-height) w-(--positioner-width) max-w-(--available-width) transition-[top,left,right,bottom] duration-(--duration) ease-(--easing) before:absolute before:content-[''] data-instant:transition-none data-[side=bottom]:before:-top-2 data-[side=bottom]:before:right-0 data-[side=bottom]:before:left-0 data-[side=bottom]:before:h-2 data-[side=left]:before:top-0 data-[side=left]:before:-right-2 data-[side=left]:before:bottom-0 data-[side=left]:before:w-2 data-[side=right]:before:top-0 data-[side=right]:before:bottom-0 data-[side=right]:before:-left-2 data-[side=right]:before:w-2 data-[side=top]:before:right-0 data-[side=top]:before:-bottom-2 data-[side=top]:before:left-0 data-[side=top]:before:h-2",
          className
        )}
        style={
          typeof styleProp === 'function'
            ? (state: NavigationMenuPrimitive.Positioner.State) => ({
                ...defaultStyle,
                ...styleProp(state),
              })
            : { ...defaultStyle, ...styleProp }
        }
        {...props}
      >
        <NavigationMenuPrimitive.Popup className='relative h-(--popup-height) w-(--popup-width) origin-(--transform-origin) rounded-xl bg-popover text-popover-foreground shadow-lg ring-1 ring-foreground/10 transition-[opacity,transform,width,height,scale,translate] duration-(--duration) ease-(--easing) data-ending-style:scale-90 data-ending-style:opacity-0 data-ending-style:duration-150 data-starting-style:scale-90 data-starting-style:opacity-0 dark:shadow-none dark:-outline-offset-1'>
          <NavigationMenuArrow />
          <NavigationMenuPrimitive.Viewport className='relative size-full overflow-hidden' />
        </NavigationMenuPrimitive.Popup>
      </NavigationMenuPrimitive.Positioner>
    </NavigationMenuPrimitive.Portal>
  );
}

function NavigationMenuViewport({
  className,
  ...props
}: NavigationMenuPrimitive.Viewport.Props) {
  return (
    <NavigationMenuPrimitive.Viewport
      data-slot='navigation-menu-viewport'
      className={cn('relative h-full w-full overflow-hidden', className)}
      {...props}
    />
  );
}

function ArrowSvg(props: React.ComponentProps<'svg'>) {
  return (
    <svg width='20' height='10' viewBox='0 0 20 10' fill='none' {...props}>
      <path
        d='M9.66437 2.60207L4.80758 6.97318C4.07308 7.63423 3.11989 8 2.13172 8H0V10H20V8H18.5349C17.5468 8 16.5936 7.63423 15.8591 6.97318L11.0023 2.60207C10.622 2.2598 10.0447 2.25979 9.66437 2.60207Z'
        className='fill-popover'
      />
      <path
        d='M8.99542 1.85876C9.75604 1.17425 10.9106 1.17422 11.6713 1.85878L16.5281 6.22989C17.0789 6.72568 17.7938 7.00001 18.5349 7.00001L15.89 7L11.0023 2.60207C10.622 2.2598 10.0447 2.2598 9.66436 2.60207L4.77734 7L2.13171 7.00001C2.87284 7.00001 3.58774 6.72568 4.13861 6.22989L8.99542 1.85876Z'
        className='fill-foreground/10'
      />
    </svg>
  );
}

function NavigationMenuArrow({
  className,
  ...props
}: NavigationMenuPrimitive.Arrow.Props) {
  return (
    <NavigationMenuPrimitive.Arrow
      data-slot='navigation-menu-arrow'
      className={cn(
        'flex transition-[left] duration-(--duration) ease-(--easing) data-[side=bottom]:-top-2 data-[side=left]:-right-3.25 data-[side=left]:rotate-90 data-[side=right]:-left-3.25 data-[side=right]:-rotate-90 data-[side=top]:-bottom-2 data-[side=top]:rotate-180',
        className
      )}
      {...props}
    >
      <ArrowSvg />
    </NavigationMenuPrimitive.Arrow>
  );
}

export {
  NavigationMenu,
  NavigationMenuList,
  NavigationMenuItem,
  NavigationMenuTrigger,
  NavigationMenuIcon,
  NavigationMenuContent,
  NavigationMenuLink,
  NavigationMenuBackdrop,
  NavigationMenuPortal,
  NavigationMenuPositioner,
  NavigationMenuViewport,
};
```
---

## Number Field <a name="number-field"></a>

A numeric input with increment and decrement buttons.

- **Registry Key**: `number-field`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/number-field.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `lucide-react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import {
  NumberField,
  NumberFieldDecrement,
  NumberFieldGroup,
  NumberFieldIncrement,
  NumberFieldInput,
} from '@/registry/default/ui/number-field';

export function NumberFieldDefault() {
  return (
    <NumberField defaultValue={5}>
      <NumberFieldGroup>
        <NumberFieldDecrement />
        <NumberFieldInput />
        <NumberFieldIncrement />
      </NumberFieldGroup>
    </NumberField>
  );
}
```

##### Example: `with-scrub`
```tsx
'use client';

import {
  NumberField,
  NumberFieldDecrement,
  NumberFieldGroup,
  NumberFieldIncrement,
  NumberFieldInput,
  NumberFieldScrubArea,
} from '@/registry/default/ui/number-field';

export function NumberFieldWithScrub() {
  return (
    <NumberField defaultValue={50}>
      <NumberFieldScrubArea label='Quantity' />
      <NumberFieldGroup>
        <NumberFieldDecrement />
        <NumberFieldInput />
        <NumberFieldIncrement />
      </NumberFieldGroup>
    </NumberField>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/number-field.tsx`
```tsx
'use client';

import * as React from 'react';

import { Label } from '@/registry/default/ui/label';
import { NumberField as NumberFieldPrimitive } from '@base-ui/react/number-field';
import { MinusIcon, MoveHorizontal, PlusIcon } from 'lucide-react';

import { cn } from '@/lib/utils';

const NumberFieldContext = React.createContext<{
  fieldId: string;
} | null>(null);

function NumberField({
  id,
  className,
  ...props
}: NumberFieldPrimitive.Root.Props) {
  const numberFieldId = React.useId();
  const fieldId = id ?? numberFieldId;

  return (
    <NumberFieldContext.Provider value={{ fieldId }}>
      <NumberFieldPrimitive.Root
        id={fieldId}
        data-slot='number-field'
        className={cn('flex flex-col items-start gap-2', className)}
        {...props}
      />
    </NumberFieldContext.Provider>
  );
}

function NumberFieldScrubArea({
  className,
  label,
  ...props
}: NumberFieldPrimitive.ScrubArea.Props & { label: string }) {
  const context = React.useContext(NumberFieldContext);

  if (!context) {
    throw new Error(
      'NumberFieldScrubArea must be used within the NumberField.'
    );
  }

  return (
    <NumberFieldPrimitive.ScrubArea
      data-slot='number-field-scrub-area'
      className={cn('cursor-ew-resize"', className)}
      {...props}
    >
      <Label htmlFor={context.fieldId} className='cursor-ew-resize font-normal'>
        {label}
      </Label>
      <NumberFieldPrimitive.ScrubAreaCursor className='drop-shadow-[0_1px_1px_#0008] filter'>
        <MoveHorizontal />
      </NumberFieldPrimitive.ScrubAreaCursor>
    </NumberFieldPrimitive.ScrubArea>
  );
}

function NumberFieldGroup({
  className,
  ...props
}: NumberFieldPrimitive.Group.Props) {
  return (
    <NumberFieldPrimitive.Group
      data-slot='number-field-group'
      className={cn(
        'flex rounded-md ring-ring/40 ring-offset-1 focus-within:border-ring focus-within:ring-2 focus-within:has-aria-invalid:border-destructive/60 focus-within:has-aria-invalid:ring-destructive/40',
        className
      )}
      {...props}
    />
  );
}

function NumberFieldDecrement({
  className,
  ...props
}: NumberFieldPrimitive.Decrement.Props) {
  return (
    <NumberFieldPrimitive.Decrement
      className={cn(
        "flex size-9 items-center justify-center rounded-tl-md rounded-bl-md border border-border bg-clip-padding text-foreground select-none [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      data-slot='number-field-decrement'
      {...props}
    >
      <MinusIcon />
    </NumberFieldPrimitive.Decrement>
  );
}

function NumberFieldIncrement({
  className,
  ...props
}: NumberFieldPrimitive.Increment.Props) {
  return (
    <NumberFieldPrimitive.Increment
      className={cn(
        "flex size-9 items-center justify-center rounded-tr-md rounded-br-md border border-border bg-clip-padding text-foreground select-none [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      data-slot='number-field-increment'
      {...props}
    >
      <PlusIcon />
    </NumberFieldPrimitive.Increment>
  );
}

function NumberFieldInput({
  className,
  ...props
}: NumberFieldPrimitive.Input.Props) {
  return (
    <NumberFieldPrimitive.Input
      className={cn(
        'h-9 w-32 border-t border-b border-border text-center text-base text-foreground tabular-nums outline-none focus:z-1',
        className
      )}
      data-slot='number-field-input'
      {...props}
    />
  );
}

export {
  NumberField,
  NumberFieldScrubArea,
  NumberFieldDecrement,
  NumberFieldIncrement,
  NumberFieldGroup,
  NumberFieldInput,
};
```
---

## Popover <a name="popover"></a>

A floating panel that appears next to a trigger element.

- **Registry Key**: `popover`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/popover.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Button } from '@/registry/default/ui/button';
import {
  Popover,
  PopoverContent,
  PopoverDescription,
  PopoverTitle,
  PopoverTrigger,
} from '@/registry/default/ui/popover';

export function PopoverDefault() {
  return (
    <Popover>
      <PopoverTrigger render={<Button variant='outline' />}>
        Open Popover
      </PopoverTrigger>
      <PopoverContent className='w-72'>
        <PopoverTitle>Dimensions</PopoverTitle>
        <PopoverDescription className='mt-1'>
          Set the dimensions for the layer.
        </PopoverDescription>
      </PopoverContent>
    </Popover>
  );
}
```

##### Example: `with-form`
```tsx
'use client';

import { Button } from '@/registry/default/ui/button';
import { Field, FieldLabel } from '@/registry/default/ui/field';
import { Input } from '@/registry/default/ui/input';
import {
  Popover,
  PopoverContent,
  PopoverDescription,
  PopoverTitle,
  PopoverTrigger,
} from '@/registry/default/ui/popover';

export function PopoverWithForm() {
  return (
    <Popover>
      <PopoverTrigger render={<Button variant='outline' />}>
        Edit Profile
      </PopoverTrigger>
      <PopoverContent className='w-80'>
        <PopoverTitle>Edit Profile</PopoverTitle>
        <PopoverDescription className='mt-1'>
          Make changes to your profile here.
        </PopoverDescription>
        <div className='mt-4 flex flex-col gap-3'>
          <Field>
            <FieldLabel>Name</FieldLabel>
            <Input defaultValue='John Doe' />
          </Field>
          <Field>
            <FieldLabel>Email</FieldLabel>
            <Input type='email' defaultValue='john@example.com' />
          </Field>
          <Button className='mt-2'>Save changes</Button>
        </div>
      </PopoverContent>
    </Popover>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/popover.tsx`
```tsx
'use client';

import * as React from 'react';

import { Popover as PopoverPrimitive } from '@base-ui/react/popover';

import { cn } from '@/lib/utils';

function Popover({ ...props }: PopoverPrimitive.Root.Props) {
  return <PopoverPrimitive.Root data-slot='popover' {...props} />;
}

function PopoverTrigger({ ...props }: PopoverPrimitive.Trigger.Props) {
  return <PopoverPrimitive.Trigger data-slot='popover-trigger' {...props} />;
}

function PopoverContent({
  className,
  align = 'center',
  alignOffset = 0,
  side = 'bottom',
  sideOffset = 4,
  ...props
}: PopoverPrimitive.Popup.Props &
  Pick<
    PopoverPrimitive.Positioner.Props,
    'align' | 'alignOffset' | 'side' | 'sideOffset'
  >) {
  return (
    <PopoverPrimitive.Portal>
      <PopoverPrimitive.Positioner
        align={align}
        alignOffset={alignOffset}
        side={side}
        sideOffset={sideOffset}
        className='isolate z-50'
      >
        <PopoverPrimitive.Popup
          data-slot='popover-content'
          className={cn(
            'z-50 flex w-72 origin-(--transform-origin) flex-col gap-2.5 rounded-lg bg-popover p-2.5 text-sm text-popover-foreground shadow-md ring-1 ring-foreground/10 outline-hidden duration-100 data-closed:animate-out data-closed:fade-out-0 data-closed:zoom-out-95 data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=inline-end]:slide-in-from-left-2 data-[side=inline-start]:slide-in-from-right-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2',
            className
          )}
          {...props}
        />
      </PopoverPrimitive.Positioner>
    </PopoverPrimitive.Portal>
  );
}

function PopoverHeader({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot='popover-header'
      className={cn('flex flex-col gap-0.5 text-sm', className)}
      {...props}
    />
  );
}

function PopoverTitle({ className, ...props }: PopoverPrimitive.Title.Props) {
  return (
    <PopoverPrimitive.Title
      data-slot='popover-title'
      className={cn('font-medium', className)}
      {...props}
    />
  );
}

function PopoverDescription({
  className,
  ...props
}: PopoverPrimitive.Description.Props) {
  return (
    <PopoverPrimitive.Description
      data-slot='popover-description'
      className={cn('text-muted-foreground', className)}
      {...props}
    />
  );
}

export {
  Popover,
  PopoverContent,
  PopoverDescription,
  PopoverHeader,
  PopoverTitle,
  PopoverTrigger,
};
```
---

## Progress <a name="progress"></a>

A progress bar that shows completion status of a task.

- **Registry Key**: `progress`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/progress.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import * as React from 'react';

import { Progress } from '@/registry/default/ui/progress';

export function ProgressDefault() {
  const [value, setValue] = React.useState(0);

  React.useEffect(() => {
    const interval = setInterval(() => {
      setValue((current) =>
        Math.min(100, Math.round(current + Math.random() * 25))
      );
    }, 1000);
    return () => clearInterval(interval);
  }, []);

  return <Progress value={value} className='w-64' />;
}
```

##### Example: `with-label`
```tsx
'use client';

import {
  Progress,
  ProgressLabel,
  ProgressValue,
} from '@/registry/default/ui/progress';

export function ProgressWithLabel() {
  return (
    <Progress value={56} className='w-64'>
      <ProgressLabel>Upload progress</ProgressLabel>
      <ProgressValue />
    </Progress>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/progress.tsx`
```tsx
'use client';

import { Progress as ProgressPrimitive } from '@base-ui/react/progress';

import { cn } from '@/lib/utils';

function Progress({
  className,
  children,
  value,
  ...props
}: ProgressPrimitive.Root.Props) {
  return (
    <ProgressPrimitive.Root
      value={value}
      data-slot='progress'
      className={cn('flex flex-wrap gap-3', className)}
      {...props}
    >
      {children}
      <ProgressTrack>
        <ProgressIndicator />
      </ProgressTrack>
    </ProgressPrimitive.Root>
  );
}

function ProgressTrack({ className, ...props }: ProgressPrimitive.Track.Props) {
  return (
    <ProgressPrimitive.Track
      className={cn(
        'relative flex h-1 w-full items-center overflow-x-hidden rounded-full bg-muted',
        className
      )}
      data-slot='progress-track'
      {...props}
    />
  );
}

function ProgressIndicator({
  className,
  ...props
}: ProgressPrimitive.Indicator.Props) {
  return (
    <ProgressPrimitive.Indicator
      data-slot='progress-indicator'
      className={cn('h-full bg-primary transition-all', className)}
      {...props}
    />
  );
}

function ProgressLabel({ className, ...props }: ProgressPrimitive.Label.Props) {
  return (
    <ProgressPrimitive.Label
      className={cn('text-sm font-medium', className)}
      data-slot='progress-label'
      {...props}
    />
  );
}

function ProgressValue({ className, ...props }: ProgressPrimitive.Value.Props) {
  return (
    <ProgressPrimitive.Value
      className={cn(
        'ml-auto text-sm text-muted-foreground tabular-nums',
        className
      )}
      data-slot='progress-value'
      {...props}
    />
  );
}

export {
  Progress,
  ProgressTrack,
  ProgressIndicator,
  ProgressLabel,
  ProgressValue,
};
```
---

## Radio Group <a name="radio-group"></a>

A set of mutually exclusive options where only one can be selected.

- **Registry Key**: `radio-group`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/radio-group.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `lucide-react`

#### How to Use
```tsx
npx shadcn@latest add @fab-ui/radio-group
```
```tsx
npm install @base-ui/react
```
#### Component Source Code
##### File: `registry/default/ui/radio-group.tsx`
```tsx
'use client';

import { Radio as RadioPrimitive } from '@base-ui/react/radio';
import { RadioGroup as RadioGroupPrimitive } from '@base-ui/react/radio-group';
import { CircleIcon } from 'lucide-react';

import { cn } from '@/lib/utils';

function RadioGroup({ className, ...props }: RadioGroupPrimitive.Props) {
  return (
    <RadioGroupPrimitive
      data-slot='radio-group'
      className={cn('grid w-full gap-2', className)}
      {...props}
    />
  );
}

function RadioGroupItem({ className, ...props }: RadioPrimitive.Root.Props) {
  return (
    <RadioPrimitive.Root
      data-slot='radio-group-item'
      className={cn(
        'group/radio-group-item peer relative flex aspect-square size-4 shrink-0 rounded-full border border-input text-primary outline-none after:absolute after:-inset-x-3 after:-inset-y-2 focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50 disabled:cursor-not-allowed disabled:opacity-50 aria-invalid:border-destructive aria-invalid:ring-[3px] aria-invalid:ring-destructive/20 dark:bg-input/30 dark:aria-invalid:border-destructive/50 dark:aria-invalid:ring-destructive/40',
        className
      )}
      {...props}
    >
      <RadioPrimitive.Indicator
        data-slot='radio-group-indicator'
        className='flex size-4 items-center justify-center text-primary group-aria-invalid/radio-group-item:text-destructive'
      >
        <CircleIcon className='absolute top-1/2 left-1/2 size-2 -translate-x-1/2 -translate-y-1/2 fill-current' />
      </RadioPrimitive.Indicator>
    </RadioPrimitive.Root>
  );
}

export { RadioGroup, RadioGroupItem };
```
---

## Scroll Area <a name="scroll-area"></a>

A scrollable container with custom scrollbars.

- **Registry Key**: `scroll-area`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/scroll-area.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { ScrollArea } from '@/registry/default/ui/scroll-area';
import { Separator } from '@/registry/default/ui/separator';

const tags = Array.from({ length: 50 }).map(
  (_, i, a) => `v1.2.0-beta.${a.length - i}`
);

export function ScrollAreaDefault() {
  return (
    <ScrollArea className='h-72 w-48 rounded-md border'>
      <div className='p-4'>
        <h4 className='mb-4 text-sm leading-none font-medium'>Tags</h4>
        {tags.map((tag, index) => (
          <div key={tag}>
            <div className='text-sm'>{tag}</div>
            {index < tags.length - 1 && <Separator className='my-2' />}
          </div>
        ))}
      </div>
    </ScrollArea>
  );
}
```

##### Example: `horizontal`
```tsx
'use client';

import { ScrollArea } from '@/registry/default/ui/scroll-area';

const items = Array.from({ length: 20 }).map((_, i) => ({
  id: i + 1,
  name: `Item ${i + 1}`,
}));

export function ScrollAreaHorizontal() {
  return (
    <ScrollArea orientation='horizontal' className='w-96 rounded-md border'>
      <div className='flex gap-4 p-4'>
        {items.map((item) => (
          <div
            key={item.id}
            className='flex h-20 w-32 shrink-0 items-center justify-center rounded-md border bg-muted'
          >
            {item.name}
          </div>
        ))}
      </div>
    </ScrollArea>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/scroll-area.tsx`
```tsx
'use client';

import { ScrollArea as ScrollAreaPrimitive } from '@base-ui/react/scroll-area';

import { cn } from '@/lib/utils';

function ScrollArea({
  className,
  children,
  orientation,
  ...props
}: ScrollAreaPrimitive.Root.Props & {
  orientation?: 'horizontal' | 'vertical' | 'both';
}) {
  return (
    <ScrollAreaPrimitive.Root className='min-h-0' {...props}>
      <ScrollAreaPrimitive.Viewport
        data-slot='scroll-area-viewport'
        className={cn(
          'size-full overscroll-contain rounded-md transition-shadow outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-1 focus-visible:ring-offset-background',
          className
        )}
      >
        {children}
      </ScrollAreaPrimitive.Viewport>
      {orientation === 'both' ? (
        <>
          <ScrollBar orientation='vertical' />
          <ScrollBar orientation='horizontal' />
        </>
      ) : (
        <ScrollBar orientation={orientation} />
      )}
      <ScrollAreaPrimitive.Corner data-slot='scroll-area-corner' />
    </ScrollAreaPrimitive.Root>
  );
}

function ScrollBar({
  className,
  orientation = 'vertical',
  ...props
}: ScrollAreaPrimitive.Scrollbar.Props) {
  return (
    <ScrollAreaPrimitive.Scrollbar
      data-slot='scroll-area-scrollbar'
      orientation={orientation}
      className={cn(
        'm-0.5 flex opacity-0 transition-opacity delay-300 data-hovering:opacity-100 data-hovering:delay-0 data-hovering:duration-100 data-scrolling:opacity-100 data-scrolling:delay-0 data-scrolling:duration-100 data-[orientation=horizontal]:h-1.5 data-[orientation=horizontal]:flex-col data-[orientation=vertical]:w-1.5',
        className
      )}
      {...props}
    >
      <ScrollAreaPrimitive.Thumb
        data-slot='scroll-area-thumb'
        className='relative flex-1 rounded-full bg-foreground/20'
      />
    </ScrollAreaPrimitive.Scrollbar>
  );
}

export { ScrollArea, ScrollBar };
```
---

## Select <a name="select"></a>

A dropdown menu for selecting a value from a list of options.

- **Registry Key**: `select`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/select.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `lucide-react`

#### How to Use
##### Example: `default`
```tsx
import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectLabel,
  SelectTrigger,
  SelectValue,
} from '@/registry/default/ui/select';

const fonts = [
  { label: 'Select font', value: null },
  { label: 'Sans-serif', value: 'sans' },
  { label: 'Serif', value: 'serif' },
  { label: 'Monospace', value: 'mono' },
  { label: 'Cursive', value: 'cursive' },
];

export function SelectDemo() {
  return (
    <Select items={fonts}>
      <SelectTrigger className='w-full max-w-48'>
        <SelectValue />
      </SelectTrigger>
      <SelectContent>
        <SelectGroup>
          <SelectLabel>Fonts</SelectLabel>
          {fonts.map((font) => (
            <SelectItem key={font.value} value={font.value}>
              {font.label}
            </SelectItem>
          ))}
        </SelectGroup>
      </SelectContent>
    </Select>
  );
}
```

##### Example: `grouped`
```tsx
import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectLabel,
  SelectSeparator,
  SelectTrigger,
  SelectValue,
} from '@/registry/default/ui/select';

export function SelectGroups() {
  const fruits = [
    { label: 'Apple', value: 'apple' },
    { label: 'Banana', value: 'banana' },
    { label: 'Blueberry', value: 'blueberry' },
  ];
  const vegetables = [
    { label: 'Carrot', value: 'carrot' },
    { label: 'Broccoli', value: 'broccoli' },
    { label: 'Spinach', value: 'spinach' },
  ];
  const allItems = [
    { label: 'Select a fruit', value: null },
    ...fruits,
    ...vegetables,
  ];
  return (
    <Select items={allItems}>
      <SelectTrigger className='w-full max-w-48'>
        <SelectValue />
      </SelectTrigger>
      <SelectContent>
        <SelectGroup>
          <SelectLabel>Fruits</SelectLabel>
          {fruits.map((item) => (
            <SelectItem key={item.value} value={item.value}>
              {item.label}
            </SelectItem>
          ))}
        </SelectGroup>
        <SelectSeparator />
        <SelectGroup>
          <SelectLabel>Vegetables</SelectLabel>
          {vegetables.map((item) => (
            <SelectItem key={item.value} value={item.value}>
              {item.label}
            </SelectItem>
          ))}
        </SelectGroup>
      </SelectContent>
    </Select>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/select.tsx`
```tsx
'use client';

import * as React from 'react';

import { Select as SelectPrimitive } from '@base-ui/react/select';
import { CheckIcon, ChevronDownIcon, ChevronUpIcon } from 'lucide-react';

import { cn } from '@/lib/utils';

const Select = SelectPrimitive.Root;

function SelectGroup({ className, ...props }: SelectPrimitive.Group.Props) {
  return (
    <SelectPrimitive.Group
      data-slot='select-group'
      className={cn('scroll-my-1 p-1', className)}
      {...props}
    />
  );
}

function SelectValue({ className, ...props }: SelectPrimitive.Value.Props) {
  return (
    <SelectPrimitive.Value
      data-slot='select-value'
      className={cn('flex flex-1 text-left', className)}
      {...props}
    />
  );
}

function SelectTrigger({
  className,
  size = 'default',
  children,
  ...props
}: SelectPrimitive.Trigger.Props & {
  size?: 'sm' | 'default';
}) {
  return (
    <SelectPrimitive.Trigger
      data-slot='select-trigger'
      data-size={size}
      className={cn(
        "flex w-fit items-center justify-between gap-1.5 rounded-4xl border border-input bg-transparent py-2 pr-2 pl-2.5 text-sm whitespace-nowrap transition-colors outline-none select-none focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50 disabled:cursor-not-allowed disabled:opacity-50 aria-invalid:border-destructive aria-invalid:ring-[3px] aria-invalid:ring-destructive/20 data-placeholder:text-muted-foreground data-[size=default]:h-8 data-[size=sm]:h-7 data-[size=sm]:rounded-[min(var(--radius-md),10px)] *:data-[slot=select-value]:line-clamp-1 *:data-[slot=select-value]:flex *:data-[slot=select-value]:items-center *:data-[slot=select-value]:gap-1.5 dark:bg-input/30 dark:hover:bg-input/50 dark:aria-invalid:border-destructive/50 dark:aria-invalid:ring-destructive/40 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    >
      {children}
      <SelectPrimitive.Icon
        render={
          <ChevronDownIcon className='pointer-events-none size-4 text-muted-foreground' />
        }
      />
    </SelectPrimitive.Trigger>
  );
}

function SelectContent({
  className,
  children,
  side = 'bottom',
  sideOffset = 4,
  align = 'center',
  alignOffset = 0,
  alignItemWithTrigger = true,
  ...props
}: SelectPrimitive.Popup.Props &
  Pick<
    SelectPrimitive.Positioner.Props,
    'align' | 'alignOffset' | 'side' | 'sideOffset' | 'alignItemWithTrigger'
  >) {
  return (
    <SelectPrimitive.Portal>
      <SelectPrimitive.Positioner
        side={side}
        sideOffset={sideOffset}
        align={align}
        alignOffset={alignOffset}
        alignItemWithTrigger={alignItemWithTrigger}
        className='isolate z-50'
      >
        <SelectPrimitive.Popup
          data-slot='select-content'
          data-align-trigger={alignItemWithTrigger}
          className={cn(
            'relative isolate z-50 max-h-(--available-height) w-(--anchor-width) min-w-36 origin-(--transform-origin) overflow-x-hidden overflow-y-auto rounded-2xl bg-popover text-popover-foreground shadow-md ring-1 ring-foreground/10 duration-100 data-closed:animate-out data-closed:fade-out-0 data-closed:zoom-out-95 data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 data-[align-trigger=true]:animate-none data-[side=bottom]:slide-in-from-top-2 data-[side=inline-end]:slide-in-from-left-2 data-[side=inline-start]:slide-in-from-right-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2',
            className
          )}
          {...props}
        >
          <SelectScrollUpButton />
          <SelectPrimitive.List>{children}</SelectPrimitive.List>
          <SelectScrollDownButton />
        </SelectPrimitive.Popup>
      </SelectPrimitive.Positioner>
    </SelectPrimitive.Portal>
  );
}

function SelectLabel({
  className,
  ...props
}: SelectPrimitive.GroupLabel.Props) {
  return (
    <SelectPrimitive.GroupLabel
      data-slot='select-label'
      className={cn('px-1.5 py-1 text-xs text-muted-foreground', className)}
      {...props}
    />
  );
}

function SelectItem({
  className,
  children,
  ...props
}: SelectPrimitive.Item.Props) {
  return (
    <SelectPrimitive.Item
      data-slot='select-item'
      className={cn(
        "relative flex w-full cursor-default items-center gap-1.5 rounded-xl py-1 pr-8 pl-1.5 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground not-data-[variant=destructive]:focus:**:text-accent-foreground data-disabled:pointer-events-none data-disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 *:[span]:last:flex *:[span]:last:items-center *:[span]:last:gap-2",
        className
      )}
      {...props}
    >
      <SelectPrimitive.ItemText className='flex flex-1 shrink-0 gap-2 whitespace-nowrap'>
        {children}
      </SelectPrimitive.ItemText>
      <SelectPrimitive.ItemIndicator
        render={
          <span className='pointer-events-none absolute right-2 flex size-4 items-center justify-center'>
            <CheckIcon className='pointer-events-none' />
          </span>
        }
      />
    </SelectPrimitive.Item>
  );
}

function SelectSeparator({
  className,
  ...props
}: SelectPrimitive.Separator.Props) {
  return (
    <SelectPrimitive.Separator
      data-slot='select-separator'
      className={cn('pointer-events-none -mx-1 my-1 h-px bg-border', className)}
      {...props}
    />
  );
}

function SelectScrollUpButton({
  className,
  ...props
}: React.ComponentProps<typeof SelectPrimitive.ScrollUpArrow>) {
  return (
    <SelectPrimitive.ScrollUpArrow
      data-slot='select-scroll-up-button'
      className={cn(
        "top-0 z-10 flex w-full cursor-default items-center justify-center bg-popover py-1 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    >
      <ChevronUpIcon />
    </SelectPrimitive.ScrollUpArrow>
  );
}

function SelectScrollDownButton({
  className,
  ...props
}: React.ComponentProps<typeof SelectPrimitive.ScrollDownArrow>) {
  return (
    <SelectPrimitive.ScrollDownArrow
      data-slot='select-scroll-down-button'
      className={cn(
        "bottom-0 z-10 flex w-full cursor-default items-center justify-center bg-popover py-1 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    >
      <ChevronDownIcon />
    </SelectPrimitive.ScrollDownArrow>
  );
}

export {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectLabel,
  SelectScrollDownButton,
  SelectScrollUpButton,
  SelectSeparator,
  SelectTrigger,
  SelectValue,
};
```
---

## Separator <a name="separator"></a>

A visual divider between content sections.

- **Registry Key**: `separator`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/separator.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Separator } from '@/registry/default/ui/separator';

export function SeparatorDefault() {
  return (
    <div className='w-64'>
      <div className='space-y-1'>
        <h4 className='text-sm leading-none font-medium'>Fab UI</h4>
        <p className='text-sm text-muted-foreground'>
          An open-source UI component library.
        </p>
      </div>
      <Separator className='my-4' />
      <div className='flex h-5 items-center space-x-4 text-sm'>
        <div>Blog</div>
        <Separator orientation='vertical' />
        <div>Docs</div>
        <Separator orientation='vertical' />
        <div>Source</div>
      </div>
    </div>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/separator.tsx`
```tsx
'use client';

import { Separator as SeparatorPrimitive } from '@base-ui/react/separator';

import { cn } from '@/lib/utils';

function Separator({
  className,
  orientation = 'horizontal',
  ...props
}: SeparatorPrimitive.Props) {
  return (
    <SeparatorPrimitive
      data-slot='separator'
      orientation={orientation}
      className={cn(
        'shrink-0 bg-border data-[orientation=horizontal]:h-px data-[orientation=horizontal]:w-full data-[orientation=vertical]:w-px data-[orientation=vertical]:self-stretch',
        className
      )}
      {...props}
    />
  );
}

export { Separator };
```
---

## Slider <a name="slider"></a>

A control for selecting a value from a range by dragging a thumb.

- **Registry Key**: `slider`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/slider.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Slider } from '@/registry/default/ui/slider';

export function SliderDefault() {
  return <Slider defaultValue={50} className='w-full max-w-xs' />;
}
```

##### Example: `with-value`
```tsx
'use client';

import { Slider } from '@/registry/default/ui/slider';

export function SliderWithValue() {
  return (
    <Slider
      defaultValue={[75]}
      max={100}
      step={1}
      className='mx-auto w-full max-w-xs'
    />
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/slider.tsx`
```tsx
'use client';

import * as React from 'react';

import { Slider as SliderPrimitive } from '@base-ui/react/slider';

import { cn } from '@/lib/utils';

function Slider({
  className,
  defaultValue,
  value,
  min = 0,
  max = 100,
  ...props
}: SliderPrimitive.Root.Props) {
  const _values = React.useMemo(
    () =>
      Array.isArray(value)
        ? value
        : Array.isArray(defaultValue)
          ? defaultValue
          : [min, max],
    [value, defaultValue, min, max]
  );

  return (
    <SliderPrimitive.Root
      className={cn(
        'data-[orientation=horizontal]:w-full data-[orientation=vertical]:h-full',
        className
      )}
      data-slot='slider'
      defaultValue={defaultValue}
      value={value}
      min={min}
      max={max}
      thumbAlignment='edge'
      {...props}
    >
      <SliderPrimitive.Control className='ddata-[orientation=vertical]:flex-col relative flex w-full touch-none items-center select-none data-disabled:opacity-50 data-[orientation=vertical]:h-full data-[orientation=vertical]:min-h-40 data-[orientation=vertical]:w-auto'>
        <SliderPrimitive.Track
          data-slot='slider-track'
          className='relative grow overflow-hidden rounded-full bg-muted select-none data-[orientation=horizontal]:h-1 data-[orientation=horizontal]:w-full data-[orientation=vertical]:h-full data-[orientation=vertical]:w-1'
        >
          <SliderPrimitive.Indicator
            data-slot='slider-range'
            className='data-[orientation=horizontal]::h-full bg-primary select-none data-[orientation=vertical]:w-full'
          />
        </SliderPrimitive.Track>
        {Array.from({ length: _values.length }, (_, index) => (
          <SliderPrimitive.Thumb
            data-slot='slider-thumb'
            key={index}
            className='relative block size-3 shrink-0 rounded-full border border-ring bg-white ring-ring/50 transition-[color,box-shadow] select-none after:absolute after:-inset-2 hover:ring-[3px] focus-visible:ring-[3px] focus-visible:outline-hidden active:ring-[3px] disabled:pointer-events-none disabled:opacity-50'
          />
        ))}
      </SliderPrimitive.Control>
    </SliderPrimitive.Root>
  );
}

export { Slider };
```
---

## Switch <a name="switch"></a>

A toggle control for switching between two states.

- **Registry Key**: `switch`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/switch.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Switch } from '@/registry/default/ui/switch';

export function SwitchDefault() {
  return <Switch />;
}
```

##### Example: `with-label`
```tsx
'use client';

import { Label } from '@/registry/default/ui/label';
import { Switch } from '@/registry/default/ui/switch';

export function SwitchWithLabel() {
  return (
    <div className='flex items-center gap-2'>
      <Switch id='airplane-mode' />
      <Label htmlFor='airplane-mode'>Airplane Mode</Label>
    </div>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/switch.tsx`
```tsx
'use client';

import { Switch as SwitchPrimitive } from '@base-ui/react/switch';

import { cn } from '@/lib/utils';

function Switch({
  className,
  size = 'default',
  ...props
}: SwitchPrimitive.Root.Props & {
  size?: 'sm' | 'default';
}) {
  return (
    <SwitchPrimitive.Root
      data-slot='switch'
      data-size={size}
      className={cn(
        'peer group/switch relative inline-flex shrink-0 items-center rounded-full border border-transparent transition-all outline-none after:absolute after:-inset-x-3 after:-inset-y-2 focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50 aria-invalid:border-destructive aria-invalid:ring-[3px] aria-invalid:ring-destructive/20 data-checked:bg-primary data-disabled:cursor-not-allowed data-disabled:opacity-50 data-unchecked:bg-input data-[size=default]:h-[18.4px] data-[size=default]:w-8 data-[size=sm]:h-3.5 data-[size=sm]:w-6 dark:aria-invalid:border-destructive/50 dark:aria-invalid:ring-destructive/40 dark:data-unchecked:bg-input/80',
        className
      )}
      {...props}
    >
      <SwitchPrimitive.Thumb
        data-slot='switch-thumb'
        className='pointer-events-none block rounded-full bg-background ring-0 transition-transform group-data-[size=default]/switch:size-4 group-data-[size=sm]/switch:size-3 group-data-[size=default]/switch:data-checked:translate-x-[calc(100%-2px)] group-data-[size=sm]/switch:data-checked:translate-x-[calc(100%-2px)] group-data-[size=default]/switch:data-unchecked:translate-x-0 group-data-[size=sm]/switch:data-unchecked:translate-x-0 dark:data-checked:bg-primary-foreground dark:data-unchecked:bg-foreground'
      />
    </SwitchPrimitive.Root>
  );
}

export { Switch };
```
---

## Tabs <a name="tabs"></a>

A set of layered sections of content displayed one at a time.

- **Registry Key**: `tabs`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/tabs.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import {
  Tabs,
  TabsContent,
  TabsList,
  TabsTrigger,
} from '@/registry/default/ui/tabs';

export function TabsDefault() {
  return (
    <Tabs defaultValue='account'>
      <TabsList>
        <TabsTrigger value='account'>Account</TabsTrigger>
        <TabsTrigger value='password'>Password</TabsTrigger>
        <TabsTrigger value='settings'>Settings</TabsTrigger>
      </TabsList>
      <TabsContent value='account' className='p-4'>
        <p className='text-sm text-muted-foreground'>
          Make changes to your account.
        </p>
      </TabsContent>
      <TabsContent value='password' className='p-4'>
        <p className='text-sm text-muted-foreground'>
          Change your password here.
        </p>
      </TabsContent>
      <TabsContent value='settings' className='p-4'>
        <p className='text-sm text-muted-foreground'>
          Adjust your settings here.
        </p>
      </TabsContent>
    </Tabs>
  );
}
```

##### Example: `underline`
```tsx
'use client';

import {
  Tabs,
  TabsContent,
  TabsList,
  TabsTrigger,
} from '@/registry/default/ui/tabs';

export function TabsUnderline() {
  return (
    <Tabs defaultValue='overview'>
      <TabsList variant='underline'>
        <TabsTrigger value='overview'>Overview</TabsTrigger>
        <TabsTrigger value='analytics'>Analytics</TabsTrigger>
        <TabsTrigger value='reports'>Reports</TabsTrigger>
      </TabsList>
      <TabsContent value='overview' className='p-4'>
        <p className='text-sm text-muted-foreground'>
          View your dashboard overview.
        </p>
      </TabsContent>
      <TabsContent value='analytics' className='p-4'>
        <p className='text-sm text-muted-foreground'>
          Analyze your data and metrics.
        </p>
      </TabsContent>
      <TabsContent value='reports' className='p-4'>
        <p className='text-sm text-muted-foreground'>
          Generate and view reports.
        </p>
      </TabsContent>
    </Tabs>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/tabs.tsx`
```tsx
'use client';

import { Tabs as TabsPrimitive } from '@base-ui/react/tabs';

import { cn } from '@/lib/utils';

type TabsVariant = 'default' | 'underline';

function Tabs({ className, ...props }: TabsPrimitive.Root.Props) {
  return (
    <TabsPrimitive.Root
      data-slot='tabs'
      className={cn(
        'flex flex-col gap-2 data-[orientation=vertical]:flex-row',
        className
      )}
      {...props}
    />
  );
}

function TabsList({
  variant = 'default',
  className,
  children,
  ...props
}: TabsPrimitive.List.Props & {
  variant?: TabsVariant;
}) {
  return (
    <TabsPrimitive.List
      data-slot='tabs-list'
      className={cn(
        'relative z-0 flex w-fit items-center justify-center gap-x-0.5',
        'data-[orientation=vertical]:flex-col',
        variant === 'default'
          ? 'rounded-lg bg-muted p-0.5 text-foreground'
          : 'text-foreground/50 hover:text-foreground data-[orientation=horizontal]:py-1 data-[orientation=vertical]:px-1 *:data-[slot=tabs-trigger]:data-active:text-foreground',
        className
      )}
      {...props}
    >
      {children}
      <TabsPrimitive.Indicator
        data-slot='tab-indicator'
        className={cn(
          'absolute bottom-0 left-0 h-(--active-tab-height) w-(--active-tab-width) translate-x-(--active-tab-left) -translate-y-(--active-tab-bottom) transition-[width,translate] duration-200 ease-in-out',
          variant === 'underline'
            ? 'z-10 bg-primary data-[orientation=horizontal]:h-0.5 data-[orientation=horizontal]:translate-y-px data-[orientation=vertical]:w-0.5 data-[orientation=vertical]:-translate-x-px'
            : '-z-1 rounded-md bg-background/70 shadow-sm dark:bg-background/50'
        )}
      />
    </TabsPrimitive.List>
  );
}

function TabsTrigger({ className, ...props }: TabsPrimitive.Tab.Props) {
  return (
    <TabsPrimitive.Tab
      data-slot='tabs-trigger'
      className={cn(
        "flex flex-1 shrink-0 cursor-pointer items-center justify-center rounded-md border border-transparent text-sm font-medium whitespace-nowrap shadow-none transition-[color,background-color,box-shadow] outline-none focus-visible:ring-2 focus-visible:ring-ring data-disabled:pointer-events-none data-disabled:opacity-64 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        'gap-1.5 px-[calc(--spacing(2.5)-1px)] py-[calc(--spacing(1.5)-1px)]',
        'data-[orientation=vertical]:w-full data-[orientation=vertical]:justify-start',
        className
      )}
      {...props}
    />
  );
}

function TabsContent({ className, ...props }: TabsPrimitive.Panel.Props) {
  return (
    <TabsPrimitive.Panel
      data-slot='tabs-content'
      className={cn('flex-1 outline-none', className)}
      {...props}
    />
  );
}

export { Tabs, TabsList, TabsTrigger, TabsContent };
```
---

## Textarea <a name="textarea"></a>

A multi-line text input field.

- **Registry Key**: `textarea`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/textarea.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Textarea } from '@/registry/default/ui/textarea';

export function TextareaDefault() {
  return <Textarea placeholder='Type your message here.' className='w-64' />;
}
```

##### Example: `with-label`
```tsx
'use client';

import {
  Field,
  FieldDescription,
  FieldLabel,
} from '@/registry/default/ui/field';
import { Textarea } from '@/registry/default/ui/textarea';

export function TextareaWithLabel() {
  return (
    <Field className='w-64'>
      <FieldLabel>Bio</FieldLabel>
      <Textarea placeholder='Tell us about yourself' />
      <FieldDescription className='my-0!'>
        You can @mention other users and organizations.
      </FieldDescription>
    </Field>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/textarea.tsx`
```tsx
import * as React from 'react';

import { cn } from '@/lib/utils';

function Textarea({ className, ...props }: React.ComponentProps<'textarea'>) {
  return (
    <textarea
      data-slot='textarea'
      className={cn(
        'flex field-sizing-content min-h-16 w-full rounded-lg border border-input bg-transparent px-2.5 py-2 text-base transition-colors outline-none placeholder:text-muted-foreground focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50 disabled:cursor-not-allowed disabled:bg-input/50 disabled:opacity-50 aria-invalid:border-destructive aria-invalid:ring-[3px] aria-invalid:ring-destructive/20 md:text-sm dark:bg-input/30 dark:disabled:bg-input/80 dark:aria-invalid:border-destructive/50 dark:aria-invalid:ring-destructive/40',
        className
      )}
      {...props}
    />
  );
}

export { Textarea };
```
---

## Toggle <a name="toggle"></a>

A two-state button that can be either on or off.

- **Registry Key**: `toggle`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/toggle.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`
  - `class-variance-authority`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Toggle } from '@/registry/default/ui/toggle';
import { Bold } from 'lucide-react';

export function ToggleDefault() {
  return (
    <Toggle aria-label='Toggle bold'>
      <Bold />
    </Toggle>
  );
}
```

##### Example: `group-default`
```tsx
'use client';

import {
  ToggleGroup,
  ToggleGroupItem,
} from '@/registry/default/ui/toggle-group';
import { AlignCenter, AlignLeft, AlignRight } from 'lucide-react';

export function ToggleGroupDefault() {
  return (
    <ToggleGroup aria-label='Text alignment'>
      <ToggleGroupItem value='left' aria-label='Align left'>
        <AlignLeft />
      </ToggleGroupItem>
      <ToggleGroupItem value='center' aria-label='Align center'>
        <AlignCenter />
      </ToggleGroupItem>
      <ToggleGroupItem value='right' aria-label='Align right'>
        <AlignRight />
      </ToggleGroupItem>
    </ToggleGroup>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/toggle.tsx`
```tsx
'use client';

import { Toggle as TogglePrimitive } from '@base-ui/react/toggle';
import { cva, type VariantProps } from 'class-variance-authority';

import { cn } from '@/lib/utils';

const toggleVariants = cva(
  "hover:text-foreground aria-pressed:bg-muted focus-visible:border-ring focus-visible:ring-ring/50 aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive data-[state=on]:bg-muted gap-1 rounded-lg text-sm font-medium transition-all [&_svg:not([class*='size-'])]:size-4 group/toggle hover:bg-muted inline-flex items-center justify-center whitespace-nowrap outline-none focus-visible:ring-[3px] disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0",
  {
    variants: {
      variant: {
        default: 'bg-transparent',
        outline: 'border-input hover:bg-muted border bg-transparent',
      },
      size: {
        default: 'h-8 min-w-8 px-2',
        sm: 'h-7 min-w-7 rounded-[min(var(--radius-md),12px)] px-1.5 text-[0.8rem]',
        lg: 'h-9 min-w-9 px-2.5',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  }
);

function Toggle({
  className,
  variant = 'default',
  size = 'default',
  ...props
}: TogglePrimitive.Props & VariantProps<typeof toggleVariants>) {
  return (
    <TogglePrimitive
      data-slot='toggle'
      className={cn(toggleVariants({ variant, size, className }))}
      {...props}
    />
  );
}

export { Toggle, toggleVariants };
```
---

## Toggle Group <a name="toggle-group"></a>

A set of toggle buttons that allows single or multiple selections.

- **Registry Key**: `toggle-group`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/toggle-group.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import {
  ToggleGroup,
  ToggleGroupItem,
} from '@/registry/default/ui/toggle-group';
import { AlignCenter, AlignLeft, AlignRight } from 'lucide-react';

export function ToggleGroupDefault() {
  return (
    <ToggleGroup aria-label='Text alignment'>
      <ToggleGroupItem value='left' aria-label='Align left'>
        <AlignLeft />
      </ToggleGroupItem>
      <ToggleGroupItem value='center' aria-label='Align center'>
        <AlignCenter />
      </ToggleGroupItem>
      <ToggleGroupItem value='right' aria-label='Align right'>
        <AlignRight />
      </ToggleGroupItem>
    </ToggleGroup>
  );
}
```

##### Example: `multiple`
```tsx
'use client';

import {
  ToggleGroup,
  ToggleGroupItem,
} from '@/registry/default/ui/toggle-group';
import { Bold, Italic, Underline } from 'lucide-react';

export function ToggleGroupMultiple() {
  return (
    <ToggleGroup multiple aria-label='Text formatting'>
      <ToggleGroupItem value='bold' aria-label='Toggle bold'>
        <Bold />
      </ToggleGroupItem>
      <ToggleGroupItem value='italic' aria-label='Toggle italic'>
        <Italic />
      </ToggleGroupItem>
      <ToggleGroupItem value='underline' aria-label='Toggle underline'>
        <Underline />
      </ToggleGroupItem>
    </ToggleGroup>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/toggle-group.tsx`
```tsx
'use client';

import * as React from 'react';

import { Toggle, toggleVariants } from '@/registry/default/ui/toggle';
import { ToggleGroup as ToggleGroupPrimitive } from '@base-ui/react/toggle-group';
import { type VariantProps } from 'class-variance-authority';

import { cn } from '@/lib/utils';

const ToggleGroupContext = React.createContext<
  VariantProps<typeof toggleVariants>
>({
  size: 'default',
  variant: 'default',
});

function ToggleGroup({
  className,
  variant,
  size,
  children,
  ...props
}: ToggleGroupPrimitive.Props & VariantProps<typeof toggleVariants>) {
  return (
    <ToggleGroupPrimitive
      data-slot='toggle-group'
      data-variant={variant}
      data-size={size}
      className={cn(
        'flex gap-px rounded-md border border-border bg-[canvas] p-0.5',
        className
      )}
      {...props}
    >
      <ToggleGroupContext.Provider value={{ variant, size }}>
        {children}
      </ToggleGroupContext.Provider>
    </ToggleGroupPrimitive>
  );
}

function ToggleGroupItem({
  className,
  children,
  variant,
  size,
  ...props
}: React.ComponentProps<typeof Toggle> & VariantProps<typeof toggleVariants>) {
  const context = React.useContext(ToggleGroupContext);

  return (
    <Toggle
      data-slot='toggle-group-item'
      data-variant={context.variant || variant}
      data-size={context.size || size}
      className={cn(
        toggleVariants({
          variant: context.variant || variant,
          size: context.size || size,
        }),
        'min-w-0 flex-1 shrink-0 rounded-none shadow-none first:rounded-l-md last:rounded-r-md focus:z-10 focus-visible:z-10 data-[variant=outline]:border-l-0 data-[variant=outline]:first:border-l',
        className
      )}
      {...props}
    >
      {children}
    </Toggle>
  );
}

export { ToggleGroup, ToggleGroupItem };
```
---

## Toolbar <a name="toolbar"></a>

A container for grouping a set of buttons and controls.

- **Registry Key**: `toolbar`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/toolbar.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Button } from '@/registry/default/ui/button';
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/registry/default/ui/select';
import {
  ToggleGroup,
  ToggleGroupItem,
} from '@/registry/default/ui/toggle-group';
import {
  Toolbar,
  ToolbarButton,
  ToolbarGroup,
  ToolbarSeparator,
} from '@/registry/default/ui/toolbar';
import {
  Tooltip,
  TooltipPopup,
  TooltipProvider,
  TooltipTrigger,
} from '@/registry/default/ui/tooltip';
import {
  AlignCenter,
  AlignLeft,
  AlignRight,
  DollarSign,
  Percent,
} from 'lucide-react';

const fonts = [
  { label: 'Helvetica', value: 'helvetica' },
  { label: 'Arial', value: 'arial' },
];

export function ToolbarDefault() {
  return (
    <TooltipProvider>
      <Toolbar className='space-x-4'>
        <ToggleGroup className='border-none p-0'>
          <Tooltip>
            <TooltipTrigger
              render={
                <ToolbarButton
                  aria-label='Align left'
                  render={<ToggleGroupItem value='left' />}
                >
                  <AlignLeft />
                </ToolbarButton>
              }
            />
            <TooltipPopup sideOffset={8}>Align left</TooltipPopup>
          </Tooltip>
          <Tooltip>
            <TooltipTrigger
              render={
                <ToolbarButton
                  aria-label='Align center'
                  render={<ToggleGroupItem value='center' />}
                >
                  <AlignCenter />
                </ToolbarButton>
              }
            />
            <TooltipPopup sideOffset={8}>Align center</TooltipPopup>
          </Tooltip>
          <Tooltip>
            <TooltipTrigger
              render={
                <ToolbarButton
                  aria-label='Align right'
                  render={<ToggleGroupItem value='right' />}
                >
                  <AlignRight />
                </ToolbarButton>
              }
            />
            <TooltipPopup sideOffset={8}>Align right</TooltipPopup>
          </Tooltip>
        </ToggleGroup>
        <ToolbarSeparator />
        <ToolbarGroup>
          <Tooltip>
            <TooltipTrigger
              render={
                <ToolbarButton
                  aria-label='Format as currency'
                  render={<Button size='icon' variant='ghost' />}
                >
                  <DollarSign />
                </ToolbarButton>
              }
            />
            <TooltipPopup sideOffset={8}>Format as currency</TooltipPopup>
          </Tooltip>
          <Tooltip>
            <TooltipTrigger
              render={
                <ToolbarButton
                  aria-label='Format as percent'
                  render={<Button size='icon' variant='ghost' />}
                >
                  <Percent />
                </ToolbarButton>
              }
            />
            <TooltipPopup sideOffset={8}>Format as percent</TooltipPopup>
          </Tooltip>
        </ToolbarGroup>
        <ToolbarSeparator />
        <ToolbarGroup>
          <Select defaultValue='helvetica' items={fonts}>
            <Tooltip>
              <TooltipTrigger
                render={
                  <ToolbarButton
                    render={
                      <SelectTrigger className='w-36'>
                        <SelectValue />
                      </SelectTrigger>
                    }
                  />
                }
              />
              <TooltipPopup sideOffset={8}>Select font</TooltipPopup>
            </Tooltip>
            <SelectContent>
              {fonts.map(({ label, value }) => (
                <SelectItem key={value} value={value}>
                  {label}
                </SelectItem>
              ))}
            </SelectContent>
          </Select>
        </ToolbarGroup>
      </Toolbar>
    </TooltipProvider>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/toolbar.tsx`
```tsx
'use client';

import { Toolbar as ToolbarPrimitive } from '@base-ui/react/toolbar';

import { cn } from '@/lib/utils';

function Toolbar({ className, ...props }: ToolbarPrimitive.Root.Props) {
  return (
    <ToolbarPrimitive.Root
      data-slot='toolbar'
      className={cn(
        'relative flex gap-px rounded-md border bg-card bg-clip-padding p-1 text-card-foreground',
        className
      )}
      {...props}
    />
  );
}

function ToolbarButton({ className, ...props }: ToolbarPrimitive.Button.Props) {
  return (
    <ToolbarPrimitive.Button
      data-slot='toolbar-button'
      className={cn(className)}
      {...props}
    />
  );
}

function ToolbarLink({ className, ...props }: ToolbarPrimitive.Link.Props) {
  return (
    <ToolbarPrimitive.Link
      data-slot='toolbar-link'
      className={cn(className)}
      {...props}
    />
  );
}

function ToolbarInput({ className, ...props }: ToolbarPrimitive.Input.Props) {
  return (
    <ToolbarPrimitive.Input
      data-slot='toolbar-input'
      className={cn(className)}
      {...props}
    />
  );
}

function ToolbarGroup({ className, ...props }: ToolbarPrimitive.Group.Props) {
  return (
    <ToolbarPrimitive.Group
      data-slot='toolbar-group'
      className={cn('flex items-center gap-1', className)}
      {...props}
    />
  );
}

function ToolbarSeparator({
  className,
  ...props
}: ToolbarPrimitive.Separator.Props) {
  return (
    <ToolbarPrimitive.Separator
      data-slot='toolbar-separator'
      className={cn(
        "shrink-0 bg-border data-[orientation=horizontal]:my-0.5 data-[orientation=horizontal]:h-px data-[orientation=horizontal]:w-full data-[orientation=vertical]:my-1.5 data-[orientation=vertical]:w-px data-[orientation=vertical]:not-[[class^='h-']]:not-[[class*='_h-']]:self-stretch",
        className
      )}
      {...props}
    />
  );
}

export {
  Toolbar,
  ToolbarGroup,
  ToolbarSeparator,
  ToolbarButton,
  ToolbarLink,
  ToolbarInput,
};
```
---

## Tooltip <a name="tooltip"></a>

A popup that displays information related to an element when the element receives keyboard focus or the mouse hovers over it.

- **Registry Key**: `tooltip`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/tooltip.json
```

#### Dependencies
- **NPM Packages**:
  - `@base-ui/react`

#### How to Use
##### Example: `default`
```tsx
'use client';

import { Button } from '@/registry/default/ui/button';
import {
  Tooltip,
  TooltipPopup,
  TooltipProvider,
  TooltipTrigger,
} from '@/registry/default/ui/tooltip';

export function TooltipDefault() {
  return (
    <TooltipProvider>
      <Tooltip>
        <TooltipTrigger render={<Button variant='outline'>Hover me</Button>} />
        <TooltipPopup>Add to library</TooltipPopup>
      </Tooltip>
    </TooltipProvider>
  );
}
```

##### Example: `positions`
```tsx
'use client';

import { Button } from '@/registry/default/ui/button';
import {
  Tooltip,
  TooltipPopup,
  TooltipProvider,
  TooltipTrigger,
} from '@/registry/default/ui/tooltip';

export function TooltipPositions() {
  return (
    <TooltipProvider>
      <div className='flex gap-4'>
        <Tooltip>
          <TooltipTrigger render={<Button variant='outline'>Top</Button>} />
          <TooltipPopup side='top'>Tooltip on top</TooltipPopup>
        </Tooltip>
        <Tooltip>
          <TooltipTrigger render={<Button variant='outline'>Right</Button>} />
          <TooltipPopup side='right'>Tooltip on right</TooltipPopup>
        </Tooltip>
        <Tooltip>
          <TooltipTrigger render={<Button variant='outline'>Bottom</Button>} />
          <TooltipPopup side='bottom'>Tooltip on bottom</TooltipPopup>
        </Tooltip>
        <Tooltip>
          <TooltipTrigger render={<Button variant='outline'>Left</Button>} />
          <TooltipPopup side='left'>Tooltip on left</TooltipPopup>
        </Tooltip>
      </div>
    </TooltipProvider>
  );
}
```

#### Component Source Code
##### File: `registry/default/ui/tooltip.tsx`
```tsx
import { Tooltip as TooltipPrimitive } from '@base-ui/react/tooltip';

import { cn } from '@/lib/utils';

const TooltipProvider = TooltipPrimitive.Provider;

const Tooltip = TooltipPrimitive.Root;

function TooltipTrigger(props: TooltipPrimitive.Trigger.Props) {
  return <TooltipPrimitive.Trigger data-slot='tooltip-trigger' {...props} />;
}

function ArrowSvg(props: React.ComponentProps<'svg'>) {
  return (
    <svg width='20' height='10' viewBox='0 0 20 10' fill='none' {...props}>
      <path
        d='M9.66437 2.60207L4.80758 6.97318C4.07308 7.63423 3.11989 8 2.13172 8H0V10H20V8H18.5349C17.5468 8 16.5936 7.63423 15.8591 6.97318L11.0023 2.60207C10.622 2.2598 10.0447 2.25979 9.66437 2.60207Z'
        className='fill-foreground'
      />
      <path
        d='M8.99542 1.85876C9.75604 1.17425 10.9106 1.17422 11.6713 1.85878L16.5281 6.22989C17.0789 6.72568 17.7938 7.00001 18.5349 7.00001L15.89 7L11.0023 2.60207C10.622 2.2598 10.0447 2.2598 9.66436 2.60207L4.77734 7L2.13171 7.00001C2.87284 7.00001 3.58774 6.72568 4.13861 6.22989L8.99542 1.85876Z'
        className='fill-background/10 dark:fill-background/10'
      />
    </svg>
  );
}

function TooltipArrow({ className, ...props }: TooltipPrimitive.Arrow.Props) {
  return (
    <TooltipPrimitive.Arrow
      className={cn(
        'flex data-[side=bottom]:-top-2 data-[side=bottom]:rotate-0 data-[side=left]:-right-3.25 data-[side=left]:rotate-90 data-[side=right]:-left-3.25 data-[side=right]:-rotate-90 data-[side=top]:-bottom-2 data-[side=top]:rotate-180',
        className
      )}
      {...props}
    >
      <ArrowSvg />
    </TooltipPrimitive.Arrow>
  );
}

function TooltipPopup({
  className,
  align = 'center',
  sideOffset = 9,
  side = 'top',
  children,
  ...props
}: TooltipPrimitive.Popup.Props & {
  align?: TooltipPrimitive.Positioner.Props['align'];
  side?: TooltipPrimitive.Positioner.Props['side'];
  sideOffset?: TooltipPrimitive.Positioner.Props['sideOffset'];
}) {
  return (
    <TooltipPrimitive.Portal>
      <TooltipPrimitive.Positioner
        data-slot='tooltip-positioner'
        className='z-50'
        sideOffset={sideOffset}
        align={align}
        side={side}
      >
        <TooltipPrimitive.Popup
          data-slot='tooltip-content'
          className={cn(
            'flex origin-(--transform-origin) flex-col rounded-md bg-foreground px-2 py-1 text-sm text-background shadow-sm ring ring-foreground/10 transition-[transform,scale,opacity] data-ending-style:scale-90 data-ending-style:opacity-0 data-instant:transition-none data-starting-style:scale-90 data-starting-style:opacity-0 dark:shadow-none dark:ring-foreground/10',
            className
          )}
          {...props}
        >
          {children}
          <TooltipArrow />
        </TooltipPrimitive.Popup>
      </TooltipPrimitive.Positioner>
    </TooltipPrimitive.Portal>
  );
}

export { TooltipProvider, Tooltip, TooltipTrigger, TooltipPopup };
```
---

## Utils <a name="utils"></a>

- **Registry Key**: `utils`
#### Installation
```bash
npx shadcn@latest add https://www.fab-ui.com/r/utils.json
```

#### Dependencies
- **NPM Packages**:
  - `clsx`
  - `tailwind-merge`

#### Component Source Code
##### File: `registry/default/lib/utils.ts`
```ts
import { clsx, type ClassValue } from 'clsx';
import { twMerge } from 'tailwind-merge';

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```
---
