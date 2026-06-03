# Launch UI Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **Launch UI** (launchuicomponents.com) — a set of high-aesthetic landing page sections, animations (glows, beams), grid layout guides, mockups, and client logo arrays built with Next.js 15+, Tailwind CSS v4, Framer Motion, and TypeScript.

## Table of Contents

- [UI Elements](#ui-elements)
  - [Accordion](#accordion)
  - [Badge](#badge)
  - [Beam](#beam)
  - [Button](#button)
  - [Card](#card)
  - [Dropdown Menu](#dropdown-menu)
  - [Footer](#footer)
  - [Glow](#glow)
  - [Item](#item)
  - [Layout Lines](#layout-lines)
  - [Link Button](#link-button)
  - [Logo](#logo)
  - [Mockup](#mockup)
  - [Mode Toggle](#mode-toggle)
  - [Navbar](#navbar)
  - [Navigation Menu](#navigation-menu)
  - [Navigation](#navigation)
  - [Pricing Column](#pricing-column)
  - [Screenshot](#screenshot)
  - [Section](#section)
  - [Sheet](#sheet)
- [Landing Page Sections](#landing-page-sections)
  - [Cta Section](#section-cta)
  - [Faq Section](#section-faq)
  - [Footer Section](#section-footer)
  - [Hero Section](#section-hero)
  - [Items Section](#section-items)
  - [Logos Section](#section-logos)
  - [Navbar Section](#section-navbar)
  - [Pricing Section](#section-pricing)
  - [Stats Section](#section-stats)
- [Assembled Page Example](#assembled-page-example)

---

## UI Elements <a name="ui-elements"></a>

### Accordion <a name="accordion"></a>

Shadcn-based Accordion component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\accordion.tsx`

#### Component Source Code

```tsx
"use client";

import * as AccordionPrimitive from "@radix-ui/react-accordion";
import { ChevronDownIcon } from "lucide-react";
import * as React from "react";

import { cn } from "@/lib/utils";

function Accordion({
  ...props
}: React.ComponentProps<typeof AccordionPrimitive.Root>) {
  return <AccordionPrimitive.Root data-slot="accordion" {...props} />;
}

function AccordionItem({
  className,
  ...props
}: React.ComponentProps<typeof AccordionPrimitive.Item>) {
  return (
    <AccordionPrimitive.Item
      data-slot="accordion-item"
      className={cn("border-border dark:border-border/15 border-b", className)}
      {...props}
    />
  );
}

function AccordionTrigger({
  className,
  children,
  ...props
}: React.ComponentProps<typeof AccordionPrimitive.Trigger>) {
  return (
    <AccordionPrimitive.Header className="flex">
      <AccordionPrimitive.Trigger
        data-slot="accordion-trigger"
        className={cn(
          "focus-visible:border-ring focus-visible:ring-ring/50 text-md flex flex-1 items-center justify-between py-4 text-left font-medium transition-all outline-none hover:underline focus-visible:ring-[3px] disabled:pointer-events-none disabled:opacity-50 [&[data-state=open]>svg]:rotate-180",
          className,
        )}
        {...props}
      >
        {children}
        <ChevronDownIcon className="text-muted-foreground pointer-events-none size-4 shrink-0 transition-transform duration-200" />
      </AccordionPrimitive.Trigger>
    </AccordionPrimitive.Header>
  );
}

function AccordionContent({
  className,
  children,
  ...props
}: React.ComponentProps<typeof AccordionPrimitive.Content>) {
  return (
    <AccordionPrimitive.Content
      data-slot="accordion-content"
      className="data-[state=closed]:animate-accordion-up data-[state=open]:animate-accordion-down overflow-hidden text-sm transition-all"
      {...props}
    >
      <div className={cn("pt-0 pb-4", className)}>{children}</div>
    </AccordionPrimitive.Content>
  );
}

export { Accordion, AccordionContent, AccordionItem, AccordionTrigger };

```

---

### Badge <a name="badge"></a>

Shadcn-based Badge component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\badge.tsx`

#### Component Source Code

```tsx
import { Slot } from "@radix-ui/react-slot";
import { cva, type VariantProps } from "class-variance-authority";
import * as React from "react";

import { cn } from "@/lib/utils";

const badgeVariants = cva(
  "inline-flex items-center rounded-full border border-border/100 dark:border-border/20 text-xs font-semibold transition-colors focus:outline-hidden focus:ring-2 focus:ring-ring focus:ring-offset-2 gap-2",
  {
    variants: {
      variant: {
        default:
          "border-transparent bg-primary text-primary-foreground dark:shadow-sm dark:border-transparent",
        brand:
          "border-transparent bg-brand text-primary-foreground dark:shadow-sm dark:border-transparent",
        "brand-secondary":
          "border-transparent bg-brand-foreground/20 text-brand dark:border-transparent",
        secondary:
          "border-transparent bg-secondary text-secondary-foreground dark:shadow-sm dark:border-transparent",
        destructive:
          "border-transparent bg-destructive/30 text-destructive-foreground dark:shadow-sm dark:border-transparent",
        outline: "text-foreground",
      },
      size: {
        default: "px-2.5 py-1",
        sm: "px-1",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  },
);

function Badge({
  className,
  variant,
  size,
  asChild = false,
  ...props
}: React.ComponentProps<"span"> &
  VariantProps<typeof badgeVariants> & { asChild?: boolean }) {
  const Comp = asChild ? Slot : "span";

  return (
    <Comp
      data-slot="badge"
      className={cn(badgeVariants({ variant, size }), className)}
      {...props}
    />
  );
}

export { Badge, badgeVariants };

```

---

### Beam <a name="beam"></a>

An animated linear light beam that pulses along a path, perfect for connection graphics.

- **Category**: UI Elements
- **Local Path**: `components\ui\beam.tsx`

#### Component Source Code

```tsx
import { cva, type VariantProps } from "class-variance-authority";
import * as React from "react";

import { cn } from "@/lib/utils";

const beamVariants = cva(
  "relative after:content-[''] after:absolute after:inset-0 after:rounded-full after:scale-200",
  {
    variants: {
      tone: {
        default:
          "after:bg-radial after:from-foreground/30 after:from-10% after:to-foreground/0 after:to-60%",
        brand:
          "after:bg-radial after:from-brand/10 dark:after:from-brand/30 after:from-10% after:to-brand/0 after:to-60%",
        brandLight:
          "after:bg-radial dark:after:from-brand-foreground/30 after:from-brand-foreground/10 after:from-10% after:to-brand-foreground/0 after:to-60%",
      },
    },
    defaultVariants: {
      tone: "default",
    },
  },
);

export interface BeamProps
  extends
    React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof beamVariants> {}

function Beam({ className, tone, ...props }: BeamProps) {
  return (
    <div
      data-slot="beam"
      className={cn(beamVariants({ tone, className }))}
      {...props}
    />
  );
}

export { Beam, beamVariants };

```

---

### Button <a name="button"></a>

Shadcn-based Button component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\button.tsx`

#### Component Source Code

```tsx
import { Slot } from "@radix-ui/react-slot";
import { cva, type VariantProps } from "class-variance-authority";
import * as React from "react";

import { cn } from "@/lib/utils";

const buttonVariants = cva(
  "inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-md text-sm font-medium transition-colors focus-visible:outline-hidden focus-visible:ring-1 focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0",
  {
    variants: {
      variant: {
        default:
          "text-primary-foreground shadow-sm dark:hover:from-primary/80 hover:from-primary/70 dark:hover:to-primary/70 hover:to-primary/90 bg-linear-to-b from-primary/60 to-primary/100 dark:from-primary/100 dark:to-primary/70 border-t-primary",
        destructive:
          "bg-destructive/30 text-destructive-foreground shadow-xs hover:bg-destructive/90",
        outline:
          "border border-input bg-background shadow-xs hover:bg-accent hover:text-accent-foreground",
        glow: "glass-4 hover:glass-5 shadow-md",
        secondary:
          "bg-secondary text-secondary-foreground shadow-xs hover:bg-secondary/80",
        ghost: "hover:bg-accent hover:text-accent-foreground",
        link: "text-foreground underline-offset-4 hover:underline",
      },
      size: {
        default: "h-9 px-4 py-2",
        xs: "h-7 rounded-md px-2",
        sm: "h-8 rounded-md px-3 text-xs",
        lg: "h-10 rounded-md px-5",
        icon: "size-9",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  },
);

function Button({
  className,
  variant,
  size,
  asChild = false,
  ...props
}: React.ComponentProps<"button"> &
  VariantProps<typeof buttonVariants> & {
    asChild?: boolean;
  }) {
  const Comp = asChild ? Slot : "button";

  return (
    <Comp
      data-slot="button"
      className={cn(buttonVariants({ variant, size, className }))}
      {...props}
    />
  );
}

export { Button, buttonVariants };

```

---

### Card <a name="card"></a>

Shadcn-based Card component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\card.tsx`

#### Component Source Code

```tsx
import * as React from "react";

import { cn } from "@/lib/utils";

function Card({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card"
      className={cn(
        "bg-card text-card-foreground rounded-xl border shadow-sm",
        className,
      )}
      {...props}
    />
  );
}

function CardHeader({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-header"
      className={cn("flex flex-col space-y-1.5 p-6", className)}
      {...props}
    />
  );
}

function CardTitle({ className, ...props }: React.ComponentProps<"h3">) {
  return (
    <h3
      data-slot="card-title"
      className={cn("leading-none font-semibold tracking-tight", className)}
      {...props}
    />
  );
}

function CardDescription({ className, ...props }: React.ComponentProps<"p">) {
  return (
    <p
      data-slot="card-description"
      className={cn("text-muted-foreground text-sm", className)}
      {...props}
    />
  );
}

function CardContent({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-content"
      className={cn("p-6 pt-0", className)}
      {...props}
    />
  );
}

function CardFooter({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-footer"
      className={cn("flex items-center p-6 pt-0", className)}
      {...props}
    />
  );
}

export {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
};

```

---

### Dropdown Menu <a name="dropdown-menu"></a>

Shadcn-based Dropdown Menu component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\dropdown-menu.tsx`

#### Component Source Code

```tsx
"use client";

import * as DropdownMenuPrimitive from "@radix-ui/react-dropdown-menu";
import { CheckIcon, ChevronRightIcon, CircleIcon } from "lucide-react";
import * as React from "react";

import { cn } from "@/lib/utils";

function DropdownMenu({
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Root>) {
  return <DropdownMenuPrimitive.Root data-slot="dropdown-menu" {...props} />;
}

function DropdownMenuPortal({
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Portal>) {
  return (
    <DropdownMenuPrimitive.Portal data-slot="dropdown-menu-portal" {...props} />
  );
}

function DropdownMenuTrigger({
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Trigger>) {
  return (
    <DropdownMenuPrimitive.Trigger
      data-slot="dropdown-menu-trigger"
      {...props}
    />
  );
}

function DropdownMenuGroup({
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Group>) {
  return (
    <DropdownMenuPrimitive.Group data-slot="dropdown-menu-group" {...props} />
  );
}

function DropdownMenuRadioGroup({
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.RadioGroup>) {
  return (
    <DropdownMenuPrimitive.RadioGroup
      data-slot="dropdown-menu-radio-group"
      {...props}
    />
  );
}

function DropdownMenuSub({
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Sub>) {
  return <DropdownMenuPrimitive.Sub data-slot="dropdown-menu-sub" {...props} />;
}

function DropdownMenuSubTrigger({
  className,
  inset,
  children,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.SubTrigger> & {
  inset?: boolean;
}) {
  return (
    <DropdownMenuPrimitive.SubTrigger
      data-slot="dropdown-menu-sub-trigger"
      data-inset={inset}
      className={cn(
        "focus:bg-accent focus:text-accent-foreground data-[state=open]:bg-accent data-[state=open]:text-accent-foreground [&_svg:not([class*='text-'])]:text-muted-foreground flex cursor-default items-center gap-2 rounded-sm px-2 py-1.5 text-sm outline-hidden select-none data-[inset]:pl-8 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className,
      )}
      {...props}
    >
      {children}
      <ChevronRightIcon className="ml-auto size-4" />
    </DropdownMenuPrimitive.SubTrigger>
  );
}

function DropdownMenuSubContent({
  className,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.SubContent>) {
  return (
    <DropdownMenuPrimitive.SubContent
      data-slot="dropdown-menu-sub-content"
      className={cn(
        "border-border dark:border-border/15 bg-popover text-popover-foreground data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 z-50 min-w-[8rem] origin-(--radix-dropdown-menu-content-transform-origin) overflow-hidden rounded-md border p-1 shadow-lg",
        className,
      )}
      {...props}
    />
  );
}

function DropdownMenuContent({
  className,
  sideOffset = 4,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Content>) {
  return (
    <DropdownMenuPrimitive.Portal>
      <DropdownMenuPrimitive.Content
        data-slot="dropdown-menu-content"
        sideOffset={sideOffset}
        className={cn(
          "border-border dark:border-border/15 bg-popover text-popover-foreground data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 z-50 max-h-(--radix-dropdown-menu-content-available-height) min-w-[8rem] origin-(--radix-dropdown-menu-content-transform-origin) overflow-x-hidden overflow-y-auto rounded-md border p-1 shadow-md",
          className,
        )}
        {...props}
      />
    </DropdownMenuPrimitive.Portal>
  );
}

function DropdownMenuItem({
  className,
  inset,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Item> & {
  inset?: boolean;
}) {
  return (
    <DropdownMenuPrimitive.Item
      data-slot="dropdown-menu-item"
      data-inset={inset}
      className={cn(
        "focus:bg-accent focus:text-accent-foreground [&_svg:not([class*='text-'])]:text-muted-foreground relative flex cursor-default items-center gap-2 rounded-sm px-2 py-1.5 text-sm outline-hidden select-none data-[disabled]:pointer-events-none data-[disabled]:opacity-50 data-[inset]:pl-8 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className,
      )}
      {...props}
    />
  );
}

function DropdownMenuCheckboxItem({
  className,
  children,
  checked,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.CheckboxItem>) {
  return (
    <DropdownMenuPrimitive.CheckboxItem
      data-slot="dropdown-menu-checkbox-item"
      className={cn(
        "focus:bg-accent focus:text-accent-foreground relative flex cursor-default items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm outline-hidden select-none data-[disabled]:pointer-events-none data-[disabled]:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className,
      )}
      checked={checked}
      {...props}
    >
      <span className="pointer-events-none absolute left-2 flex size-3.5 items-center justify-center">
        <DropdownMenuPrimitive.ItemIndicator>
          <CheckIcon className="size-4" />
        </DropdownMenuPrimitive.ItemIndicator>
      </span>
      {children}
    </DropdownMenuPrimitive.CheckboxItem>
  );
}

function DropdownMenuRadioItem({
  className,
  children,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.RadioItem>) {
  return (
    <DropdownMenuPrimitive.RadioItem
      data-slot="dropdown-menu-radio-item"
      className={cn(
        "focus:bg-accent focus:text-accent-foreground relative flex cursor-default items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm outline-hidden select-none data-[disabled]:pointer-events-none data-[disabled]:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className,
      )}
      {...props}
    >
      <span className="pointer-events-none absolute left-2 flex size-3.5 items-center justify-center">
        <DropdownMenuPrimitive.ItemIndicator>
          <CircleIcon className="size-2 fill-current" />
        </DropdownMenuPrimitive.ItemIndicator>
      </span>
      {children}
    </DropdownMenuPrimitive.RadioItem>
  );
}

function DropdownMenuLabel({
  className,
  inset,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Label> & {
  inset?: boolean;
}) {
  return (
    <DropdownMenuPrimitive.Label
      data-slot="dropdown-menu-label"
      data-inset={inset}
      className={cn(
        "px-2 py-1.5 text-sm font-medium data-[inset]:pl-8",
        className,
      )}
      {...props}
    />
  );
}

function DropdownMenuSeparator({
  className,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Separator>) {
  return (
    <DropdownMenuPrimitive.Separator
      data-slot="dropdown-menu-separator"
      className={cn("bg-border -mx-1 my-1 h-px", className)}
      {...props}
    />
  );
}

function DropdownMenuShortcut({
  className,
  ...props
}: React.ComponentProps<"span">) {
  return (
    <span
      data-slot="dropdown-menu-shortcut"
      className={cn(
        "text-muted-foreground ml-auto text-xs tracking-widest",
        className,
      )}
      {...props}
    />
  );
}

export {
  DropdownMenu,
  DropdownMenuCheckboxItem,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuPortal,
  DropdownMenuRadioGroup,
  DropdownMenuRadioItem,
  DropdownMenuSeparator,
  DropdownMenuShortcut,
  DropdownMenuSub,
  DropdownMenuSubContent,
  DropdownMenuSubTrigger,
  DropdownMenuTrigger,
};

```

---

### Footer <a name="footer"></a>

Shadcn-based Footer component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\footer.tsx`

#### Component Source Code

```tsx
import * as React from "react";

import { cn } from "@/lib/utils";

function Footer({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="footer"
      className={cn("bg-background text-foreground pt-12 pb-4", className)}
      {...props}
    />
  );
}

function FooterContent({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="footer-content"
      className={cn(
        "grid grid-cols-2 gap-8 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5",
        className,
      )}
      {...props}
    />
  );
}

function FooterColumn({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="footer-column"
      className={cn("flex flex-col gap-4", className)}
      {...props}
    />
  );
}

function FooterBottom({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="footer-bottom"
      className={cn(
        "border-border dark:border-border/15 text-muted-foreground mt-8 flex flex-col items-center justify-between gap-4 border-t pt-4 text-xs sm:flex-row",
        className,
      )}
      {...props}
    />
  );
}

export { Footer, FooterBottom, FooterColumn, FooterContent };

```

---

### Glow <a name="glow"></a>

A dynamic radial lighting glow effect with size and color customization.

- **Category**: UI Elements
- **Local Path**: `components\ui\glow.tsx`

#### Component Source Code

```tsx
import { cva, VariantProps } from "class-variance-authority";
import React from "react";

import { cn } from "@/lib/utils";

const glowVariants = cva("absolute w-full", {
  variants: {
    variant: {
      top: "top-0",
      above: "-top-[128px]",
      bottom: "bottom-0",
      below: "-bottom-[128px]",
      center: "top-[50%]",
    },
  },
  defaultVariants: {
    variant: "top",
  },
});

function Glow({
  className,
  variant,
  ...props
}: React.ComponentProps<"div"> & VariantProps<typeof glowVariants>) {
  return (
    <div
      data-slot="glow"
      className={cn(glowVariants({ variant }), className)}
      {...props}
    >
      <div
        className={cn(
          "from-brand-foreground/50 to-brand-foreground/0 absolute left-1/2 h-[256px] w-[60%] -translate-x-1/2 scale-[2.5] rounded-[50%] bg-radial from-10% to-60% opacity-20 sm:h-[512px] dark:opacity-100",
          variant === "center" && "-translate-y-1/2",
        )}
      />
      <div
        className={cn(
          "from-brand/30 to-brand-foreground/0 absolute left-1/2 h-[128px] w-[40%] -translate-x-1/2 scale-200 rounded-[50%] bg-radial from-10% to-60% opacity-20 sm:h-[256px] dark:opacity-100",
          variant === "center" && "-translate-y-1/2",
        )}
      />
    </div>
  );
}

export default Glow;

```

---

### Item <a name="item"></a>

Shadcn-based Item component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\item.tsx`

#### Component Source Code

```tsx
import * as React from "react";

import { cn } from "@/lib/utils";

function Item({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="item"
      className={cn("text-foreground flex flex-col gap-4 p-4", className)}
      {...props}
    />
  );
}

function ItemTitle({ className, ...props }: React.ComponentProps<"h3">) {
  return (
    <h3
      data-slot="item-title"
      className={cn(
        "text-sm leading-none font-semibold tracking-tight sm:text-base",
        className,
      )}
      {...props}
    />
  );
}

function ItemDescription({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="item-description"
      className={cn(
        "text-muted-foreground flex max-w-[240px] flex-col gap-2 text-sm text-balance",
        className,
      )}
      {...props}
    />
  );
}

function ItemIcon({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="item-icon"
      className={cn("flex items-center self-start", className)}
      {...props}
    />
  );
}

export { Item, ItemDescription, ItemIcon, ItemTitle };

```

---

### Layout Lines <a name="layout-lines"></a>

A grid overlay with thin accent vertical lines for modern tech-styled layouts.

- **Category**: UI Elements
- **Local Path**: `components\ui\layout-lines.tsx`

#### Component Source Code

```tsx
import * as React from "react";

import { cn } from "@/lib/utils";

function LayoutLines({ className, ...props }: React.ComponentProps<"section">) {
  return (
    <section
      className={cn("pointer-events-none fixed inset-0 top-0", className)}
      {...props}
    >
      <div className="max-w-container line-y line-dashed mx-auto flex h-full flex-col"></div>
    </section>
  );
}

export { LayoutLines };

```

---

### Link Button <a name="link-button"></a>

A button component that handles navigation linking and active states.

- **Category**: UI Elements
- **Local Path**: `components\ui\link-button.tsx`

#### Component Source Code

```tsx
import { type VariantProps } from "class-variance-authority";
import { type ComponentProps, type ReactNode } from "react";

import { Button, buttonVariants } from "./button";

export interface LinkButtonProps {
  href: string;
  children: ReactNode;
  variant?: VariantProps<typeof buttonVariants>["variant"];
  icon?: ReactNode;
  iconRight?: ReactNode;
  size?: ComponentProps<typeof Button>["size"];
}

export function LinkButton({
  href,
  children,
  variant = "default",
  icon,
  iconRight,
  size = "lg",
}: LinkButtonProps) {
  return (
    <Button variant={variant} size={size} asChild>
      <a href={href}>
        {icon}
        {children}
        {iconRight}
      </a>
    </Button>
  );
}

```

---

### Logo <a name="logo"></a>

Shadcn-based Logo component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\logo.tsx`

#### Component Source Code

```tsx
import React from "react";

import { cn } from "@/lib/utils";

import { Badge } from "./badge";

export interface LogoProps extends React.HTMLAttributes<HTMLDivElement> {
  image: React.ComponentType<React.SVGProps<SVGSVGElement>>;
  name: string;
  version?: string;
  width?: number;
  height?: number;
  showName?: boolean;
  badge?: string;
}

export default function Logo({
  className,
  image: SvgImage,
  name,
  version,
  width = 24,
  height = 24,
  showName = true,
  badge,
  ...props
}: LogoProps) {
  return (
    <div
      data-slot="logo"
      className={cn("flex items-center gap-2 text-sm font-medium", className)}
      {...props}
    >
      <SvgImage
        width={width}
        height={height}
        aria-hidden="true"
        className="max-h-full max-w-full opacity-70"
      />
      <span className={cn(!showName && "sr-only")}>{name}</span>
      {version && <span className="text-muted-foreground">{version}</span>}
      {badge && (
        <Badge variant="brand" size="sm">
          {badge}
        </Badge>
      )}
    </div>
  );
}

```

---

### Mockup <a name="mockup"></a>

A device browser wrapper for displaying visual screenshots and dashboards.

- **Category**: UI Elements
- **Local Path**: `components\ui\mockup.tsx`

#### Component Source Code

```tsx
import { cva, type VariantProps } from "class-variance-authority";
import React from "react";

import { cn } from "@/lib/utils";

const mockupVariants = cva(
  "flex relative z-10 overflow-hidden shadow-2xl border border-border/70 dark:border-border/5 dark:border-t-border/15",
  {
    variants: {
      type: {
        mobile: "rounded-[48px] max-w-[350px]",
        responsive: "rounded-md",
      },
    },
    defaultVariants: {
      type: "responsive",
    },
  },
);

export interface MockupProps
  extends
    React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof mockupVariants> {}

function Mockup({ className, type, ...props }: MockupProps) {
  return (
    <div
      data-slot="mockup"
      className={cn(mockupVariants({ type, className }))}
      {...props}
    />
  );
}

const frameVariants = cva(
  "bg-border/50 flex relative z-10 overflow-hidden rounded-2xl dark:bg-border/10",
  {
    variants: {
      size: {
        small: "p-2",
        large: "p-4",
      },
    },
    defaultVariants: {
      size: "small",
    },
  },
);

export interface MockupFrameProps
  extends
    React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof frameVariants> {}

function MockupFrame({ className, size, ...props }: MockupFrameProps) {
  return (
    <div
      data-slot="mockup-frame"
      className={cn(frameVariants({ size, className }))}
      {...props}
    />
  );
}

export { Mockup, MockupFrame };

```

---

### Mode Toggle <a name="mode-toggle"></a>

Shadcn-based Mode Toggle component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\mode-toggle.tsx`

#### Component Source Code

```tsx
"use client";

import { ChevronsUpDownIcon } from "lucide-react";
import { useTheme } from "next-themes";

import { Button } from "./button";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "./dropdown-menu";

export function ModeToggle() {
  const { theme, setTheme } = useTheme();
  const selectedTheme = theme ?? "dark";

  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button variant="ghost" className="gap-1 px-2 py-0 text-xs">
          <span className="capitalize" suppressHydrationWarning>
            {selectedTheme}
          </span>
          <span className="inline"> theme</span>
          <ChevronsUpDownIcon className="size-3" />
        </Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent align="end">
        <DropdownMenuItem onClick={() => setTheme("light")}>
          Light
        </DropdownMenuItem>
        <DropdownMenuItem onClick={() => setTheme("dark")}>
          Dark
        </DropdownMenuItem>
        <DropdownMenuItem onClick={() => setTheme("system")}>
          System
        </DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  );
}

```

---

### Navbar <a name="navbar"></a>

Shadcn-based Navbar component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\navbar.tsx`

#### Component Source Code

```tsx
import * as React from "react";

import { cn } from "@/lib/utils";

function Navbar({ className, ...props }: React.ComponentProps<"nav">) {
  return (
    <nav
      data-slot="navbar"
      className={cn("flex items-center justify-between py-4", className)}
      {...props}
    />
  );
}

function NavbarLeft({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="navbar-left"
      className={cn("flex items-center justify-start gap-4", className)}
      {...props}
    />
  );
}

function NavbarRight({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="navbar-right"
      className={cn("flex items-center justify-end gap-4", className)}
      {...props}
    />
  );
}

function NavbarCenter({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="navbar-center"
      className={cn("flex items-center justify-center gap-4", className)}
      {...props}
    />
  );
}

export { Navbar, NavbarCenter, NavbarLeft, NavbarRight };

```

---

### Navigation Menu <a name="navigation-menu"></a>

Shadcn-based Navigation Menu component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\navigation-menu.tsx`

#### Component Source Code

```tsx
import * as NavigationMenuPrimitive from "@radix-ui/react-navigation-menu";
import { cva } from "class-variance-authority";
import { ChevronDownIcon } from "lucide-react";
import * as React from "react";

import { cn } from "@/lib/utils";

function NavigationMenu({
  className,
  children,
  viewport = true,
  ...props
}: React.ComponentProps<typeof NavigationMenuPrimitive.Root> & {
  viewport?: boolean;
}) {
  return (
    <NavigationMenuPrimitive.Root
      data-slot="navigation-menu"
      data-viewport={viewport}
      className={cn(
        "group/navigation-menu relative z-10 flex max-w-max flex-1 items-center justify-center",
        className,
      )}
      {...props}
    >
      {children}
      {viewport && <NavigationMenuViewport />}
    </NavigationMenuPrimitive.Root>
  );
}

function NavigationMenuList({
  className,
  ...props
}: React.ComponentProps<typeof NavigationMenuPrimitive.List>) {
  return (
    <NavigationMenuPrimitive.List
      data-slot="navigation-menu-list"
      className={cn(
        "group flex flex-1 list-none items-center justify-center gap-1",
        className,
      )}
      {...props}
    />
  );
}

function NavigationMenuItem({
  className,
  ...props
}: React.ComponentProps<typeof NavigationMenuPrimitive.Item>) {
  return (
    <NavigationMenuPrimitive.Item
      data-slot="navigation-menu-item"
      className={cn("relative", className)}
      {...props}
    />
  );
}

const navigationMenuTriggerStyle = cva(
  "group inline-flex h-9 w-max items-center justify-center rounded-md px-4 py-2 text-sm font-medium hover:bg-foreground/5 hover:text-accent-foreground focus:bg-foreground/10 focus:text-accent-foreground disabled:pointer-events-none disabled:opacity-50 data-[state=open]:hover:bg-foreground/10 data-[state=open]:text-accent-foreground data-[state=open]:focus:bg-foreground/10 data-[state=open]:bg-foreground/50 focus-visible:ring-ring/50 outline-none transition-[color,box-shadow] focus-visible:ring-[3px] focus-visible:outline-1",
);

function NavigationMenuTrigger({
  className,
  children,
  ...props
}: React.ComponentProps<typeof NavigationMenuPrimitive.Trigger>) {
  return (
    <NavigationMenuPrimitive.Trigger
      data-slot="navigation-menu-trigger"
      className={cn(navigationMenuTriggerStyle(), "group", className)}
      {...props}
    >
      {children}{" "}
      <ChevronDownIcon
        className="relative top-[1px] ml-1 size-3 transition duration-300 group-data-[state=open]:rotate-180"
        aria-hidden="true"
      />
    </NavigationMenuPrimitive.Trigger>
  );
}

function NavigationMenuContent({
  className,
  ...props
}: React.ComponentProps<typeof NavigationMenuPrimitive.Content>) {
  return (
    <NavigationMenuPrimitive.Content
      data-slot="navigation-menu-content"
      className={cn(
        "data-[motion^=from-]:animate-in data-[motion^=to-]:animate-out data-[motion^=from-]:fade-in data-[motion^=to-]:fade-out data-[motion=from-end]:slide-in-from-right-52 data-[motion=from-start]:slide-in-from-left-52 data-[motion=to-end]:slide-out-to-right-52 data-[motion=to-start]:slide-out-to-left-52 top-0 left-0 w-full p-2 pr-2.5 md:absolute md:w-auto",
        "group-data-[viewport=false]/navigation-menu:bg-popover group-data-[viewport=false]/navigation-menu:text-popover-foreground group-data-[viewport=false]/navigation-menu:data-[state=open]:animate-in group-data-[viewport=false]/navigation-menu:data-[state=closed]:animate-out group-data-[viewport=false]/navigation-menu:data-[state=closed]:zoom-out-95 group-data-[viewport=false]/navigation-menu:data-[state=open]:zoom-in-95 group-data-[viewport=false]/navigation-menu:data-[state=open]:fade-in-0 group-data-[viewport=false]/navigation-menu:data-[state=closed]:fade-out-0 group-data-[viewport=false]/navigation-menu:top-full group-data-[viewport=false]/navigation-menu:mt-1.5 group-data-[viewport=false]/navigation-menu:overflow-hidden group-data-[viewport=false]/navigation-menu:rounded-md group-data-[viewport=false]/navigation-menu:border group-data-[viewport=false]/navigation-menu:shadow group-data-[viewport=false]/navigation-menu:duration-200 **:data-[slot=navigation-menu-link]:focus:ring-0 **:data-[slot=navigation-menu-link]:focus:outline-none",
        className,
      )}
      {...props}
    />
  );
}

function NavigationMenuLink({
  className,
  ...props
}: React.ComponentProps<typeof NavigationMenuPrimitive.Link>) {
  return (
    <NavigationMenuPrimitive.Link
      data-slot="navigation-menu-link"
      className={cn(
        "data-[active=true]:focus:bg-foreground/10 data-[active=true]:hover:bg-foreground/10 data-[active=true]:bg-foreground/10 data-[active=true]:text-accent-foreground hover:bg-foreground/10 hover:text-accent-foreground focus:bg-foreground/10 focus:text-accent-foreground focus-visible:ring-ring/50 [&_svg:not([class*='text-'])]:text-muted-foreground flex flex-col gap-1 rounded-sm p-2 text-sm transition-all outline-none focus-visible:ring-[3px] focus-visible:outline-1 [&_svg:not([class*='size-'])]:size-4",
        className,
      )}
      {...props}
    />
  );
}

function NavigationMenuViewport({
  className,
  ...props
}: React.ComponentProps<typeof NavigationMenuPrimitive.Viewport>) {
  return (
    <div
      className={cn(
        "absolute top-full left-0 isolate z-50 flex justify-center",
      )}
    >
      <NavigationMenuPrimitive.Viewport
        data-slot="navigation-menu-viewport"
        className={cn(
          "origin-top-center bg-popover text-popover-foreground data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-90 border-border dark:border-border/15 relative mt-1.5 h-[var(--radix-navigation-menu-viewport-height)] w-full overflow-hidden rounded-md border shadow-sm md:w-[var(--radix-navigation-menu-viewport-width)]",
          className,
        )}
        {...props}
      />
    </div>
  );
}

function NavigationMenuIndicator({
  className,
  ...props
}: React.ComponentProps<typeof NavigationMenuPrimitive.Indicator>) {
  return (
    <NavigationMenuPrimitive.Indicator
      data-slot="navigation-menu-indicator"
      className={cn(
        "data-[state=visible]:animate-in data-[state=hidden]:animate-out data-[state=hidden]:fade-out data-[state=visible]:fade-in top-full z-[1] flex h-1.5 items-end justify-center overflow-hidden",
        className,
      )}
      {...props}
    >
      <div className="bg-border relative top-[60%] h-2 w-2 rotate-45 rounded-tl-sm shadow-md" />
    </NavigationMenuPrimitive.Indicator>
  );
}

export {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuIndicator,
  NavigationMenuItem,
  NavigationMenuLink,
  NavigationMenuList,
  NavigationMenuTrigger,
  navigationMenuTriggerStyle,
  NavigationMenuViewport,
};

```

---

### Navigation <a name="navigation"></a>

Shadcn-based Navigation component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\navigation.tsx`

#### Component Source Code

```tsx
"use client";

import Link from "next/link";
import * as React from "react";
import { ReactNode } from "react";

import { siteConfig } from "@/config/site";
import { cn } from "@/lib/utils";

import LaunchUI from "../logos/launch-ui";
import {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuItem,
  NavigationMenuLink,
  NavigationMenuList,
  NavigationMenuTrigger,
  navigationMenuTriggerStyle,
} from "./navigation-menu";

interface ComponentItem {
  title: string;
  href: string;
  description: string;
}

interface MenuItem {
  title: string;
  href?: string;
  isLink?: boolean;
  content?: ReactNode;
}

interface NavigationProps {
  menuItems?: MenuItem[];
  components?: ComponentItem[];
  logo?: ReactNode;
  logoTitle?: string;
  logoDescription?: string;
  logoHref?: string;
  introItems?: {
    title: string;
    href: string;
    description: string;
  }[];
}

export default function Navigation({
  menuItems = [
    {
      title: "Getting started",
      content: "default",
    },
    {
      title: "Components",
      content: "components",
    },
    {
      title: "Documentation",
      isLink: true,
      href: siteConfig.getStartedUrl,
    },
  ],
  components = [
    {
      title: "Alert Dialog",
      href: `${siteConfig.url}/docs/primitives/alert-dialog`,
      description:
        "A modal dialog that interrupts the user with important content and expects a response.",
    },
    {
      title: "Hover Card",
      href: `${siteConfig.url}/docs/primitives/hover-card`,
      description:
        "For sighted users to preview content available behind a link.",
    },
    {
      title: "Progress",
      href: `${siteConfig.url}/docs/primitives/progress`,
      description:
        "Displays an indicator showing the completion progress of a task, typically displayed as a progress bar.",
    },
    {
      title: "Scroll Area",
      href: `${siteConfig.url}/docs/primitives/scroll-area`,
      description: "A scrollable container with custom scrollbars.",
    },
    {
      title: "Tabs",
      href: `${siteConfig.url}/docs/primitives/tabs`,
      description:
        "A set of layered sections of content, known as tab panels, that are displayed one at a time.",
    },
    {
      title: "Tooltip",
      href: `${siteConfig.url}/docs/primitives/tooltip`,
      description:
        "A popup that displays information related to an element when the element receives keyboard focus or the mouse hovers over it.",
    },
  ],
  logo = <LaunchUI />,
  logoTitle = "Launch UI",
  logoDescription = "Landing page template built with React, Shadcn/ui and Tailwind that you can copy/paste into your project.",
  logoHref = siteConfig.getStartedUrl,
  introItems = [
    {
      title: "Introduction",
      href: siteConfig.getStartedUrl,
      description: "Reusable components built using Radix UI and Tailwind CSS.",
    },
    {
      title: "Installation",
      href: siteConfig.getStartedUrl,
      description: "How to install dependencies and structure your app.",
    },
    {
      title: "Typography",
      href: siteConfig.getStartedUrl,
      description: "Styles for headings, paragraphs, and lists.",
    },
  ],
}: NavigationProps) {
  return (
    <NavigationMenu className="hidden md:flex">
      <NavigationMenuList>
        {menuItems.map((item) => (
          <NavigationMenuItem key={item.title}>
            {item.isLink ? (
              <NavigationMenuLink
                className={navigationMenuTriggerStyle()}
                asChild
              >
                <Link href={item.href || ""}>{item.title}</Link>
              </NavigationMenuLink>
            ) : (
              <>
                <NavigationMenuTrigger>{item.title}</NavigationMenuTrigger>
                <NavigationMenuContent>
                  {item.content === "default" ? (
                    <ul className="grid gap-3 p-4 md:w-[400px] lg:w-[500px] lg:grid-cols-[.75fr_1fr]">
                      <li className="row-span-3">
                        <NavigationMenuLink asChild>
                          <a
                            className="from-muted/30 to-muted/10 flex h-full w-full flex-col justify-end rounded-md bg-linear-to-b p-6 no-underline outline-hidden select-none focus:shadow-md"
                            href={logoHref}
                          >
                            {logo}
                            <div className="mt-4 mb-2 text-lg font-medium">
                              {logoTitle}
                            </div>
                            <p className="text-muted-foreground text-sm leading-tight">
                              {logoDescription}
                            </p>
                          </a>
                        </NavigationMenuLink>
                      </li>
                      {introItems.map((intro) => (
                        <ListItem
                          key={intro.title}
                          href={intro.href}
                          title={intro.title}
                        >
                          {intro.description}
                        </ListItem>
                      ))}
                    </ul>
                  ) : item.content === "components" ? (
                    <ul className="grid w-[400px] gap-3 p-4 md:w-[500px] md:grid-cols-2 lg:w-[600px]">
                      {components.map((component) => (
                        <ListItem
                          key={component.title}
                          title={component.title}
                          href={component.href}
                        >
                          {component.description}
                        </ListItem>
                      ))}
                    </ul>
                  ) : (
                    item.content
                  )}
                </NavigationMenuContent>
              </>
            )}
          </NavigationMenuItem>
        ))}
      </NavigationMenuList>
    </NavigationMenu>
  );
}

function ListItem({
  className,
  title,
  children,
  ...props
}: React.ComponentProps<"a"> & { title: string }) {
  return (
    <li>
      <NavigationMenuLink asChild>
        <a
          data-slot="list-item"
          className={cn(
            "hover:bg-accent hover:text-accent-foreground focus:bg-accent focus:text-accent-foreground block space-y-1 rounded-md p-3 leading-none no-underline outline-hidden transition-colors select-none",
            className,
          )}
          {...props}
        >
          <div className="text-sm leading-none font-medium">{title}</div>
          <p className="text-muted-foreground line-clamp-2 text-sm leading-snug">
            {children}
          </p>
        </a>
      </NavigationMenuLink>
    </li>
  );
}

```

---

### Pricing Column <a name="pricing-column"></a>

Shadcn-based Pricing Column component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\pricing-column.tsx`

#### Component Source Code

```tsx
import { cva, type VariantProps } from "class-variance-authority";
import { CircleCheckBig } from "lucide-react";
import Link from "next/link";
import { ReactNode } from "react";

import { cn } from "@/lib/utils";

import { Button } from "./button";

const pricingColumnVariants = cva(
  "max-w-container relative flex flex-col gap-6 overflow-hidden rounded-2xl p-8 shadow-xl",
  {
    variants: {
      variant: {
        default: "glass-1 to-transparent dark:glass-3",
        glow: "glass-2 to-transparent dark:glass-3 after:content-[''] after:absolute after:-top-[128px] after:left-1/2 after:h-[128px] after:w-[100%] after:max-w-[960px] after:-translate-x-1/2 after:rounded-[50%] dark:after:bg-foreground/30 after:blur-[72px]",
        "glow-brand":
          "glass-3 from-card/100 to-card/100 dark:glass-4 after:content-[''] after:absolute after:-top-[128px] after:left-1/2 after:h-[128px] after:w-[100%] after:max-w-[960px] after:-translate-x-1/2 after:rounded-[50%] after:bg-brand-foreground/70 after:blur-[72px]",
      },
    },
    defaultVariants: {
      variant: "default",
    },
  },
);

export interface PricingColumnProps
  extends
    React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof pricingColumnVariants> {
  name: string;
  icon?: ReactNode;
  description: string;
  price: number;
  originalPrice?: number;
  promotionText?: ReactNode;
  priceNote: string;
  cta: {
    variant: "glow" | "default";
    label: string;
    href: string;
  };
  features: string[];
}

export function PricingColumn({
  name,
  icon,
  description,
  price,
  originalPrice,
  promotionText,
  priceNote,
  cta,
  features,
  variant,
  className,
  ...props
}: PricingColumnProps) {
  return (
    <div
      className={cn(pricingColumnVariants({ variant, className }))}
      {...props}
    >
      <hr
        className={cn(
          "via-foreground/60 absolute top-0 left-[10%] h-[1px] w-[80%] border-0 bg-linear-to-r from-transparent to-transparent",
          variant === "glow-brand" && "via-brand",
        )}
      />
      <div className="flex flex-col gap-7">
        <header className="flex flex-col gap-2">
          <h2 className="flex items-center gap-2 font-bold">
            {icon && (
              <div className="text-muted-foreground flex items-center gap-2">
                {icon}
              </div>
            )}
            {name}
          </h2>
          <p className="text-muted-foreground max-w-[220px] text-sm">
            {description}
          </p>
        </header>
        <section className="flex flex-col gap-3">
          {originalPrice !== undefined && (
            <div className="flex h-6 items-baseline gap-1">
              <span className="text-muted-foreground text-lg font-medium line-through">
                {originalPrice > 0 && price !== originalPrice
                  ? `$${originalPrice}`
                  : ""}
              </span>
            </div>
          )}
          <div className="flex items-center gap-3 lg:flex-col lg:items-start xl:flex-row xl:items-center">
            <div className="flex flex-col gap-1">
              <div className="flex items-baseline gap-1">
                <span className="text-muted-foreground text-2xl font-bold">
                  $
                </span>
                <span className="text-6xl font-bold">{price}</span>
              </div>
            </div>
            <div className="flex min-h-[40px] flex-col">
              {price > 0 && (
                <>
                  <span className="text-sm">one-time payment</span>
                  <span className="text-muted-foreground text-sm">
                    plus local taxes
                  </span>
                </>
              )}
            </div>
          </div>
          {promotionText && (
            <div className="text-brand-foreground h-6 text-sm font-medium">
              {promotionText}
            </div>
          )}
        </section>
        <Button variant={cta.variant} size="lg" asChild>
          <Link href={cta.href}>{cta.label}</Link>
        </Button>
        <p className="text-muted-foreground min-h-[40px] max-w-[220px] text-sm">
          {priceNote}
        </p>
        <hr className="border-input" />
      </div>
      <div>
        <ul className="flex flex-col gap-2">
          {features.map((feature, index) => (
            <li
              key={`${feature}-${index}`}
              className="flex items-center gap-2 text-sm"
            >
              <CircleCheckBig className="text-muted-foreground size-4 shrink-0" />
              {feature}
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
}

export { pricingColumnVariants };

```

---

### Screenshot <a name="screenshot"></a>

A visual screenshot card wrapper with zoom and shadow effects.

- **Category**: UI Elements
- **Local Path**: `components\ui\screenshot.tsx`

#### Component Source Code

```tsx
import Image from "next/image";

import { cn } from "@/lib/utils";

interface ScreenshotProps {
  srcLight: string;
  srcDark?: string;
  alt: string;
  width: number;
  height: number;
  className?: string;
}

export default function Screenshot({
  srcLight,
  srcDark,
  alt,
  width,
  height,
  className,
}: ScreenshotProps) {
  if (!srcDark) {
    return (
      <Image
        src={srcLight}
        alt={alt}
        width={width}
        height={height}
        className={className}
        unoptimized={srcLight.endsWith(".svg")}
      />
    );
  }

  return (
    <>
      <Image
        src={srcLight}
        alt={alt}
        width={width}
        height={height}
        className={cn(className, "block dark:hidden")}
        unoptimized={srcLight.endsWith(".svg")}
      />
      <Image
        src={srcDark}
        alt={alt}
        width={width}
        height={height}
        className={cn(className, "hidden dark:block")}
        unoptimized={srcDark.endsWith(".svg")}
      />
    </>
  );
}

```

---

### Section <a name="section"></a>

Shadcn-based Section component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\section.tsx`

#### Component Source Code

```tsx
import * as React from "react";

import { cn } from "@/lib/utils";

function Section({ className, ...props }: React.ComponentProps<"section">) {
  return (
    <section
      data-slot="section"
      className={cn("line-b px-4 py-12 sm:py-24 md:py-32", className)}
      {...props}
    />
  );
}

export { Section };

```

---

### Sheet <a name="sheet"></a>

Shadcn-based Sheet component styled for neubrutalist/modern startup templates.

- **Category**: UI Elements
- **Local Path**: `components\ui\sheet.tsx`

#### Component Source Code

```tsx
"use client";

import * as SheetPrimitive from "@radix-ui/react-dialog";
import { XIcon } from "lucide-react";
import * as React from "react";

import { cn } from "@/lib/utils";

function sheetContentHasAccessibleTitle(node: React.ReactNode): boolean {
  return React.Children.toArray(node).some((child) => {
    if (!React.isValidElement(child)) return false;
    if (child.type === SheetPrimitive.Title || child.type === SheetTitle) {
      return true;
    }
    const nested = (child.props as { children?: React.ReactNode }).children;
    if (nested != null) {
      return sheetContentHasAccessibleTitle(nested);
    }
    return false;
  });
}

function Sheet({ ...props }: React.ComponentProps<typeof SheetPrimitive.Root>) {
  return <SheetPrimitive.Root data-slot="sheet" {...props} />;
}

function SheetTrigger({
  ...props
}: React.ComponentProps<typeof SheetPrimitive.Trigger>) {
  return <SheetPrimitive.Trigger data-slot="sheet-trigger" {...props} />;
}

function SheetClose({
  ...props
}: React.ComponentProps<typeof SheetPrimitive.Close>) {
  return <SheetPrimitive.Close data-slot="sheet-close" {...props} />;
}

function SheetPortal({
  ...props
}: React.ComponentProps<typeof SheetPrimitive.Portal>) {
  return <SheetPrimitive.Portal data-slot="sheet-portal" {...props} />;
}

function SheetOverlay({
  className,
  ...props
}: React.ComponentProps<typeof SheetPrimitive.Overlay>) {
  return (
    <SheetPrimitive.Overlay
      data-slot="sheet-overlay"
      className={cn(
        "data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 fixed inset-0 z-50 bg-black/80",
        className,
      )}
      {...props}
    />
  );
}

function SheetContent({
  className,
  children,
  side = "right",
  ...props
}: React.ComponentProps<typeof SheetPrimitive.Content> & {
  side?: "top" | "right" | "bottom" | "left";
}) {
  return (
    <SheetPortal>
      <SheetOverlay />
      <SheetPrimitive.Content
        data-slot="sheet-content"
        className={cn(
          "border-border dark:border-border/15 bg-background data-[state=open]:animate-in data-[state=closed]:animate-out fixed z-50 gap-4 p-6 shadow-lg transition ease-in-out data-[state=closed]:duration-300 data-[state=open]:duration-500",
          side === "right" &&
            "data-[state=closed]:slide-out-to-right data-[state=open]:slide-in-from-right inset-y-0 right-0 h-full w-3/4 border-l sm:max-w-sm",
          side === "left" &&
            "data-[state=closed]:slide-out-to-left data-[state=open]:slide-in-from-left inset-y-0 left-0 h-full w-3/4 border-r sm:max-w-sm",
          side === "top" &&
            "data-[state=closed]:slide-out-to-top data-[state=open]:slide-in-from-top inset-x-0 top-0 border-b",
          side === "bottom" &&
            "data-[state=closed]:slide-out-to-bottom data-[state=open]:slide-in-from-bottom inset-x-0 bottom-0 border-t",
          className,
        )}
        {...props}
      >
        {!sheetContentHasAccessibleTitle(children) && (
          <SheetPrimitive.Title className="sr-only">Menu</SheetPrimitive.Title>
        )}
        {children}
        <SheetPrimitive.Close className="ring-offset-background focus:ring-ring data-[state=open]:bg-secondary absolute top-6 right-6 z-[100] rounded-sm opacity-70 transition-opacity hover:opacity-100 focus:ring-2 focus:ring-offset-2 focus:outline-hidden disabled:pointer-events-none">
          <XIcon className="size-5" />
          <span className="sr-only">Close</span>
        </SheetPrimitive.Close>
      </SheetPrimitive.Content>
    </SheetPortal>
  );
}

function SheetHeader({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="sheet-header"
      className={cn(
        "flex flex-col space-y-2 text-center sm:text-left",
        className,
      )}
      {...props}
    />
  );
}

function SheetFooter({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="sheet-footer"
      className={cn(
        "flex flex-col-reverse sm:flex-row sm:justify-end sm:space-x-2",
        className,
      )}
      {...props}
    />
  );
}

function SheetTitle({
  className,
  ...props
}: React.ComponentProps<typeof SheetPrimitive.Title>) {
  return (
    <SheetPrimitive.Title
      data-slot="sheet-title"
      className={cn("text-foreground text-lg font-semibold", className)}
      {...props}
    />
  );
}

function SheetDescription({
  className,
  ...props
}: React.ComponentProps<typeof SheetPrimitive.Description>) {
  return (
    <SheetPrimitive.Description
      data-slot="sheet-description"
      className={cn("text-muted-foreground text-sm", className)}
      {...props}
    />
  );
}

export {
  Sheet,
  SheetClose,
  SheetContent,
  SheetDescription,
  SheetFooter,
  SheetHeader,
  SheetTitle,
  SheetTrigger,
};

```

---

## Landing Page Sections <a name="landing-page-sections"></a>

### Cta Section <a name="section-cta"></a>

Call to action block with clean grid layout, card wrappers, and email signup input.

- **Category**: Landing Page Sections
- **Local Path**: `components\sections\cta\default.tsx`

#### Component Source Code

```tsx
import { siteConfig } from "@/config/site";
import { cn } from "@/lib/utils";

import Glow from "../../ui/glow";
import { LinkButton, type LinkButtonProps } from "../../ui/link-button";
import { Section } from "../../ui/section";

interface CTAButtonProps extends Omit<LinkButtonProps, "children"> {
  text: string;
}

interface CTAProps {
  title?: string;
  buttons?: CTAButtonProps[] | false;
  className?: string;
}

const DEFAULT_CTA_BUTTONS: CTAButtonProps[] = [
  {
    href: siteConfig.getStartedUrl,
    text: "Get Started",
    variant: "default",
  },
];

export default function CTA({
  title = "Start building",
  buttons = DEFAULT_CTA_BUTTONS,
  className,
}: CTAProps) {
  return (
    <Section className={cn("group relative overflow-hidden", className)}>
      <div className="max-w-container relative z-10 mx-auto flex flex-col items-center gap-6 text-center sm:gap-8">
        <h2 className="max-w-[640px] text-3xl leading-tight font-semibold sm:text-5xl sm:leading-tight">
          {title}
        </h2>
        {buttons !== false && buttons.length > 0 && (
          <div className="flex justify-center gap-4">
            {buttons.map((button) => (
              <LinkButton
                key={`${button.href}-${button.text}`}
                variant={button.variant || "default"}
                size="lg"
                href={button.href}
                icon={button.icon}
                iconRight={button.iconRight}
              >
                {button.text}
              </LinkButton>
            ))}
          </div>
        )}
      </div>
      <div className="absolute top-0 left-0 h-full w-full translate-y-[1rem] opacity-80 transition-all duration-500 ease-in-out group-hover:translate-y-[-2rem] group-hover:opacity-100">
        <Glow variant="bottom" />
      </div>
    </Section>
  );
}

```

---

### Faq Section <a name="section-faq"></a>

Frequently Asked Questions block featuring Radix Accordion with neubrutalistic border details.

- **Category**: Landing Page Sections
- **Local Path**: `components\sections\faq\default.tsx`

#### Component Source Code

```tsx
import Link from "next/link";
import { ReactNode } from "react";

import { siteConfig } from "@/config/site";

import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "../../ui/accordion";
import { Section } from "../../ui/section";

interface FAQItemProps {
  question: string;
  answer: ReactNode;
  value?: string;
}

interface FAQProps {
  title?: string;
  items?: FAQItemProps[] | false;
  className?: string;
}

export default function FAQ({
  title = "Questions and Answers",
  items = [
    {
      question:
        "Why is building a great landing page critical for your business?",
      answer: (
        <>
          <p className="text-muted-foreground mb-4 max-w-[640px] text-balance">
            In today&apos;s AI-driven world, standing out is harder than ever.
            While anyone can build a product, a professional landing page makes
            the difference between success and failure.
          </p>
          <p className="text-muted-foreground mb-4 max-w-[640px] text-balance">
            Launch UI helps you ship faster without compromising on quality.
          </p>
        </>
      ),
    },
    {
      question: "Why use Launch UI instead of a no-code tool?",
      answer: (
        <>
          <p className="text-muted-foreground mb-4 max-w-[600px]">
            No-code tools lock you into their ecosystem with recurring fees and
            limited control. They often come with performance issues and make it
            difficult to integrate with your product.
          </p>
          <p className="text-muted-foreground mb-4 max-w-[600px]">
            You can&apos;t even change your hosting provider and basic things
            like web analytics come as extra costs and paid add-ons.
          </p>
          <p className="text-muted-foreground mb-4 max-w-[600px]">
            What might seem like a convenient solution today could paint you
            into a corner tomorrow, limiting your ability to scale and adapt.
            Launch UI gives you full control of your code while maintaining
            professional quality.
          </p>
        </>
      ),
    },
    {
      question:
        "How is Launch UI different from other component libraries and templates?",
      answer: (
        <>
          <p className="text-muted-foreground mb-4 max-w-[580px]">
            Launch UI stands out with premium design quality and delightful
            touches of custom animations and illustrations.
          </p>
          <p className="text-muted-foreground mb-4 max-w-[580px]">
            All components are carefully crafted to help position your product
            as a professional tool, avoiding the generic template look.
          </p>
          <p className="text-muted-foreground mb-4 max-w-[640px] text-balance">
            Unlike many libraries that rely on outdated CSS practices and old
            dependencies, Launch UI is built with modern technologies and best
            practices in mind.
          </p>
        </>
      ),
    },
    {
      question: 'What exactly does it mean that "The code is yours"?',
      answer: (
        <>
          <p className="text-muted-foreground mb-4 max-w-[580px]">
            The basic version of Launch UI is open-source and free forever,
            under a do-whatever-you-want license.
          </p>
          <p className="text-muted-foreground mb-4 max-w-[580px]">
            The pro version that contains more components and options is a
            one-time purchase that gives you lifetime access to all current and
            future content. Use it for unlimited personal and commercial
            projects - no recurring fees or restrictions.
          </p>
          <p className="text-muted-foreground mb-4 max-w-[580px]">
            For complete details about licensing and usage rights, check out{" "}
            <Link
              href={`${siteConfig.url}/pricing`}
              className="text-foreground underline"
            >
              the pricing page
            </Link>
            .
          </p>
        </>
      ),
    },
    {
      question: "Are Figma files included?",
      answer: (
        <p className="text-muted-foreground mb-4 max-w-[580px]">
          Yes! The complete Launch UI template is available for free on the{" "}
          <Link
            href="https://www.figma.com/community/file/1420131743903900629/launch-ui-landing-page-components-ui-kit"
            className="text-foreground underline"
          >
            Figma community
          </Link>
          .
        </p>
      ),
    },
    {
      question: "Can I get a discount?",
      answer: (
        <>
          <p className="text-muted-foreground mb-4 max-w-[580px]">
            Actually, yes! I&apos;m always actively looking for beta testers of
            new features. If you are interested in exchanging feedback for a
            discount, please contact me via{" "}
            <a
              href={siteConfig.links.email}
              className="underline underline-offset-2"
            >
              email
            </a>
            .
          </p>
        </>
      ),
    },
  ],
  className,
}: FAQProps) {
  return (
    <Section className={className}>
      <div className="max-w-container mx-auto flex flex-col items-center gap-8">
        <h2 className="text-center text-3xl font-semibold sm:text-5xl">
          {title}
        </h2>
        {items !== false && items.length > 0 && (
          <Accordion type="single" collapsible className="w-full max-w-[800px]">
            {items.map((item, index) => (
              <AccordionItem
                key={item.value ?? item.question}
                value={item.value || `item-${index + 1}`}
              >
                <AccordionTrigger>{item.question}</AccordionTrigger>
                <AccordionContent>{item.answer}</AccordionContent>
              </AccordionItem>
            ))}
          </Accordion>
        )}
      </div>
    </Section>
  );
}

```

---

### Footer Section <a name="section-footer"></a>

Pre-styled landing page block representing footer.

- **Category**: Landing Page Sections
- **Local Path**: `components\sections\footer\default.tsx`

#### Component Source Code

```tsx
import { ReactNode } from "react";

import { siteConfig } from "@/config/site";
import { cn } from "@/lib/utils";

import LaunchUI from "../../logos/launch-ui";
import {
  Footer,
  FooterBottom,
  FooterColumn,
  FooterContent,
} from "../../ui/footer";
import { ModeToggle } from "../../ui/mode-toggle";

interface FooterLink {
  text: string;
  href: string;
}

interface FooterColumnProps {
  title: string;
  links: FooterLink[];
}

interface FooterProps {
  logo?: ReactNode;
  name?: string;
  columns?: FooterColumnProps[];
  copyright?: string;
  policies?: FooterLink[];
  showModeToggle?: boolean;
  className?: string;
}

export default function FooterSection({
  logo = <LaunchUI />,
  name = "Launch UI",
  columns = [
    {
      title: "Product",
      links: [
        { text: "Changelog", href: siteConfig.url },
        { text: "Documentation", href: siteConfig.url },
      ],
    },
    {
      title: "Company",
      links: [
        { text: "About", href: siteConfig.url },
        { text: "Careers", href: siteConfig.url },
        { text: "Blog", href: siteConfig.url },
      ],
    },
    {
      title: "Contact",
      links: [
        { text: "Discord", href: siteConfig.url },
        { text: "Twitter", href: siteConfig.url },
        { text: "GitHub", href: siteConfig.links.github },
      ],
    },
  ],
  copyright = "© 2026 Mikołaj Dobrucki. All rights reserved",
  policies = [
    { text: "Privacy Policy", href: siteConfig.url },
    { text: "Terms of Service", href: siteConfig.url },
  ],
  showModeToggle = true,
  className,
}: FooterProps) {
  return (
    <footer className={cn("bg-background w-full px-4", className)}>
      <div className="max-w-container mx-auto">
        <Footer>
          <FooterContent>
            <FooterColumn className="col-span-2 sm:col-span-3 md:col-span-1">
              <div className="flex items-center gap-2">
                {logo}
                <h3 className="text-xl font-bold">{name}</h3>
              </div>
            </FooterColumn>
            {columns.map((column) => (
              <FooterColumn key={column.title}>
                <h3 className="text-md pt-1 font-semibold">{column.title}</h3>
                {column.links.map((link) => (
                  <a
                    key={`${link.href}-${link.text}`}
                    href={link.href}
                    className="text-muted-foreground text-sm"
                  >
                    {link.text}
                  </a>
                ))}
              </FooterColumn>
            ))}
          </FooterContent>
          <FooterBottom>
            <div>{copyright}</div>
            <div className="flex items-center gap-4">
              {policies.map((policy) => (
                <a key={`${policy.href}-${policy.text}`} href={policy.href}>
                  {policy.text}
                </a>
              ))}
              {showModeToggle && <ModeToggle />}
            </div>
          </FooterBottom>
        </Footer>
      </div>
    </footer>
  );
}

```

---

### Hero Section <a name="section-hero"></a>

Hero block with headline, subtext, dual action buttons, and nested product screenshot/mockup.

- **Category**: Landing Page Sections
- **Local Path**: `components\sections\hero\default.tsx`

#### Component Source Code

```tsx
import { ArrowRightIcon } from "lucide-react";
import { ReactNode } from "react";

import { siteConfig } from "@/config/site";
import { cn } from "@/lib/utils";

import Github from "../../logos/github";
import { Badge } from "../../ui/badge";
import Glow from "../../ui/glow";
import { LinkButton, type LinkButtonProps } from "../../ui/link-button";
import { Mockup, MockupFrame } from "../../ui/mockup";
import Screenshot from "../../ui/screenshot";
import { Section } from "../../ui/section";

interface HeroButtonProps extends Omit<LinkButtonProps, "children"> {
  text: string;
}

interface HeroProps {
  title?: string;
  description?: string;
  mockup?: ReactNode | false;
  badge?: ReactNode | false;
  buttons?: HeroButtonProps[] | false;
  className?: string;
}

const DEFAULT_HERO_BUTTONS: HeroButtonProps[] = [
  {
    href: siteConfig.getStartedUrl,
    text: "Get Started",
    variant: "default",
  },
  {
    href: siteConfig.links.github,
    text: "GitHub",
    variant: "glow",
    icon: <Github className="mr-2 size-4" />,
  },
];

const DEFAULT_HERO_BADGE = (
  <Badge variant="outline" className="animate-appear">
    <span className="text-muted-foreground">
      New version of Launch UI is out!
    </span>
    <a href={siteConfig.getStartedUrl} className="flex items-center gap-1">
      Get started
      <ArrowRightIcon className="size-3" />
    </a>
  </Badge>
);

const DEFAULT_HERO_MOCKUP = (
  <Screenshot
    srcLight="/dashboard-light.png"
    srcDark="/dashboard-dark.png"
    alt="Launch UI app screenshot"
    width={1248}
    height={765}
    className="w-full"
  />
);

export default function Hero({
  title = "Give your big idea the design it deserves",
  description = "Professionally designed blocks and templates built with React, Shadcn/ui and Tailwind that will help your product stand out.",
  mockup = DEFAULT_HERO_MOCKUP,
  badge = DEFAULT_HERO_BADGE,
  buttons = DEFAULT_HERO_BUTTONS,
  className,
}: HeroProps) {
  return (
    <Section
      className={cn(
        "fade-bottom overflow-hidden pb-0 sm:pb-0 md:pb-0",
        className,
      )}
    >
      <div className="max-w-container mx-auto flex flex-col gap-12 pt-16 sm:gap-24">
        <div className="flex flex-col items-center gap-6 text-center sm:gap-12">
          {badge !== false && badge}
          <h1 className="animate-appear from-foreground to-foreground dark:to-muted-foreground relative z-10 inline-block bg-linear-to-r bg-clip-text text-4xl leading-tight font-semibold text-balance text-transparent drop-shadow-2xl sm:text-6xl sm:leading-tight md:text-8xl md:leading-tight">
            {title}
          </h1>
          <p className="text-md animate-appear text-muted-foreground relative z-10 max-w-[740px] font-medium text-balance opacity-0 delay-100 sm:text-xl">
            {description}
          </p>
          {buttons !== false && buttons.length > 0 && (
            <div className="animate-appear relative z-10 flex justify-center gap-4 opacity-0 delay-300">
              {buttons.map((button) => (
                <LinkButton
                  key={`${button.href}-${button.text}`}
                  variant={button.variant || "default"}
                  size="lg"
                  href={button.href}
                  icon={button.icon}
                  iconRight={button.iconRight}
                >
                  {button.text}
                </LinkButton>
              ))}
            </div>
          )}
          {mockup !== false && (
            <div className="relative w-full pt-12">
              <MockupFrame
                className="animate-appear opacity-0 delay-700"
                size="small"
              >
                <Mockup
                  type="responsive"
                  className="bg-background/90 w-full rounded-xl border-0"
                >
                  {mockup}
                </Mockup>
              </MockupFrame>
              <Glow
                variant="top"
                className="animate-appear-zoom opacity-0 delay-1000"
              />
            </div>
          )}
        </div>
      </div>
    </Section>
  );
}

```

---

### Items Section <a name="section-items"></a>

Feature items grid showcasing core product benefits with custom visual illustrations/badges.

- **Category**: Landing Page Sections
- **Local Path**: `components\sections\items\default.tsx`

#### Component Source Code

```tsx
import {
  BlocksIcon,
  EclipseIcon,
  FastForwardIcon,
  LanguagesIcon,
  MonitorSmartphoneIcon,
  RocketIcon,
  ScanFaceIcon,
  SquarePenIcon,
} from "lucide-react";
import { ReactNode } from "react";

import { Item, ItemDescription, ItemIcon, ItemTitle } from "../../ui/item";
import { Section } from "../../ui/section";

interface ItemProps {
  title: string;
  description: string;
  icon: ReactNode;
}

interface ItemsProps {
  title?: string;
  items?: ItemProps[] | false;
  className?: string;
}

const DEFAULT_ITEMS: ItemProps[] = [
  {
    title: "Accessibility first",
    description: "Fully WCAG 2.0 compliant, made with best a11y practices",
    icon: <ScanFaceIcon className="size-5 stroke-1" />,
  },
  {
    title: "Responsive design",
    description: "Looks and works great on any device and screen size",
    icon: <MonitorSmartphoneIcon className="size-5 stroke-1" />,
  },
  {
    title: "Light and dark mode",
    description: "Seamless switching between color schemes, 6 themes included",
    icon: <EclipseIcon className="size-5 stroke-1" />,
  },
  {
    title: "Easy to customize",
    description: "Flexible options to match your product or brand",
    icon: <BlocksIcon className="size-5 stroke-1" />,
  },
  {
    title: "Top-level performance",
    description: "Made for lightning-fast load times and smooth interactions",
    icon: <FastForwardIcon className="size-5 stroke-1" />,
  },
  {
    title: "Production ready",
    description: "Thoroughly tested and launch-prepared",
    icon: <RocketIcon className="size-5 stroke-1" />,
  },
  {
    title: "Made for localization",
    description: "Easy to implement support for multiple languages and regions",
    icon: <LanguagesIcon className="size-5 stroke-1" />,
  },
  {
    title: "CMS friendly",
    description: "Built to work with any headless content management system",
    icon: <SquarePenIcon className="size-5 stroke-1" />,
  },
];

export default function Items({
  title = "Everything you need. Nothing you don't.",
  items = DEFAULT_ITEMS,
  className,
}: ItemsProps) {
  return (
    <Section className={className}>
      <div className="max-w-container mx-auto flex flex-col items-center gap-6 sm:gap-20">
        <h2 className="max-w-[560px] text-center text-3xl leading-tight font-semibold sm:text-5xl sm:leading-tight">
          {title}
        </h2>
        {items !== false && items.length > 0 && (
          <div className="grid auto-rows-fr grid-cols-2 gap-0 sm:grid-cols-3 sm:gap-4 lg:grid-cols-4">
            {items.map((item) => (
              <Item key={item.title}>
                <ItemTitle className="flex items-center gap-2">
                  <ItemIcon>{item.icon}</ItemIcon>
                  {item.title}
                </ItemTitle>
                <ItemDescription>{item.description}</ItemDescription>
              </Item>
            ))}
          </div>
        )}
      </div>
    </Section>
  );
}

```

---

### Logos Section <a name="section-logos"></a>

Social logos banner for client/partner logos.

- **Category**: Landing Page Sections
- **Local Path**: `components\sections\logos\default.tsx`

#### Component Source Code

```tsx
import { ReactNode } from "react";

import { siteConfig } from "@/config/site";

import Figma from "../../logos/figma";
import React from "../../logos/react";
import ShadcnUi from "../../logos/shadcn-ui";
import Tailwind from "../../logos/tailwind";
import TypeScript from "../../logos/typescript";
import { Badge } from "../../ui/badge";
import Logo from "../../ui/logo";
import { Section } from "../../ui/section";

interface LogosProps {
  title?: string;
  badge?: ReactNode | false;
  logos?: ReactNode[] | false;
  className?: string;
}

export default function Logos({
  title = "Built with industry-standard tools and best practices",
  badge = (
    <Badge variant="outline" className="border-brand/30 text-brand">
      Last updated: {siteConfig.stats.updated}
    </Badge>
  ),
  logos = [
    <Logo key="figma" image={Figma} name="Figma" />,
    <Logo key="react" image={React} name="React" version="19.2.4" />,
    <Logo
      key="typescript"
      image={TypeScript}
      name="TypeScript"
      version="5.9.3"
    />,
    <Logo key="shadcn" image={ShadcnUi} name="Shadcn/ui" version="4.0.5" />,
    <Logo key="tailwind" image={Tailwind} name="Tailwind" version="4.2.1" />,
  ],
  className,
}: LogosProps) {
  return (
    <Section className={className}>
      <div className="max-w-container mx-auto flex flex-col items-center gap-8 text-center">
        <div className="flex flex-col items-center gap-6">
          {badge !== false && badge}
          <h2 className="text-md font-semibold sm:text-2xl">{title}</h2>
        </div>
        {logos !== false && logos.length > 0 && (
          <div className="flex flex-wrap items-center justify-center gap-8">
            {logos}
          </div>
        )}
      </div>
    </Section>
  );
}

```

---

### Navbar Section <a name="section-navbar"></a>

Responsive navigation header with slideout mobile menu sheet.

- **Category**: Landing Page Sections
- **Local Path**: `components\sections\navbar\default.tsx`

#### Component Source Code

```tsx
"use client";

import { type VariantProps } from "class-variance-authority";
import { Menu } from "lucide-react";
import { ReactNode } from "react";

import { siteConfig } from "@/config/site";
import { cn } from "@/lib/utils";

import LaunchUI from "../../logos/launch-ui";
import { Button, buttonVariants } from "../../ui/button";
import {
  Navbar as NavbarComponent,
  NavbarLeft,
  NavbarRight,
} from "../../ui/navbar";
import Navigation from "../../ui/navigation";
import { Sheet, SheetContent, SheetTitle, SheetTrigger } from "../../ui/sheet";

interface NavbarLink {
  text: string;
  href: string;
}

interface NavbarActionProps {
  text: string;
  href: string;
  variant?: VariantProps<typeof buttonVariants>["variant"];
  icon?: ReactNode;
  iconRight?: ReactNode;
  isButton?: boolean;
}

interface NavbarProps {
  logo?: ReactNode;
  name?: string;
  homeUrl?: string;
  mobileLinks?: NavbarLink[];
  actions?: NavbarActionProps[];
  showNavigation?: boolean;
  customNavigation?: ReactNode;
  className?: string;
}

export default function Navbar({
  logo = <LaunchUI />,
  name = "Launch UI",
  homeUrl = siteConfig.url,
  mobileLinks = [
    { text: "Getting Started", href: siteConfig.url },
    { text: "Components", href: siteConfig.url },
    { text: "Documentation", href: siteConfig.url },
  ],
  actions = [
    { text: "Sign in", href: siteConfig.url, isButton: false },
    {
      text: "Get Started",
      href: siteConfig.url,
      isButton: true,
      variant: "default",
    },
  ],
  showNavigation = true,
  customNavigation,
  className,
}: NavbarProps) {
  return (
    <header className={cn("sticky top-0 z-50 -mb-4 px-4 pb-4", className)}>
      <div className="fade-bottom bg-background/15 absolute left-0 h-24 w-full backdrop-blur-lg"></div>
      <div className="max-w-container relative mx-auto">
        <NavbarComponent>
          <NavbarLeft>
            <a
              href={homeUrl}
              className="flex items-center gap-2 text-xl font-bold"
            >
              {logo}
              {name}
            </a>
            {showNavigation && (customNavigation || <Navigation />)}
          </NavbarLeft>
          <NavbarRight>
            {actions.map((action) =>
              action.isButton ? (
                <Button
                  key={`${action.href}-${action.text}`}
                  variant={action.variant || "default"}
                  asChild
                >
                  <a href={action.href}>
                    {action.icon}
                    {action.text}
                    {action.iconRight}
                  </a>
                </Button>
              ) : (
                <a
                  key={`${action.href}-${action.text}`}
                  href={action.href}
                  className="hidden text-sm md:block"
                >
                  {action.text}
                </a>
              ),
            )}
            <Sheet>
              <SheetTrigger asChild>
                <Button
                  variant="ghost"
                  size="icon"
                  className="shrink-0 md:hidden"
                >
                  <Menu className="size-5" />
                  <span className="sr-only">Toggle navigation menu</span>
                </Button>
              </SheetTrigger>
              <SheetContent side="right">
                <SheetTitle className="sr-only">Navigation menu</SheetTitle>
                <nav className="grid gap-6 text-lg font-medium">
                  <a
                    href={homeUrl}
                    className="flex items-center gap-2 text-xl font-bold"
                  >
                    <span>{name}</span>
                  </a>
                  {mobileLinks.map((link) => (
                    <a
                      key={`${link.href}-${link.text}`}
                      href={link.href}
                      className="text-muted-foreground hover:text-foreground"
                    >
                      {link.text}
                    </a>
                  ))}
                </nav>
              </SheetContent>
            </Sheet>
          </NavbarRight>
        </NavbarComponent>
      </div>
    </header>
  );
}

```

---

### Pricing Section <a name="section-pricing"></a>

Interactive pricing comparison table showcasing tiered plans and feature checklists.

- **Category**: Landing Page Sections
- **Local Path**: `components\sections\pricing\default.tsx`

#### Component Source Code

```tsx
import { User, Users } from "lucide-react";

import { siteConfig } from "@/config/site";
import { cn } from "@/lib/utils";

import { PricingColumn, PricingColumnProps } from "../../ui/pricing-column";
import { Section } from "../../ui/section";

interface PricingProps {
  title?: string | false;
  description?: string | false;
  plans?: PricingColumnProps[] | false;
  className?: string;
}

const DEFAULT_PRICING_PLANS: PricingColumnProps[] = [
  {
    name: "Free",
    description: "For everyone starting out on a website for their big idea",
    price: 0,
    priceNote: "Free and open-source forever. Get started now.",
    cta: {
      variant: "glow",
      label: "Get started for free",
      href: siteConfig.getStartedUrl,
    },
    features: [
      "1 website template",
      "9 blocks and sections",
      "4 custom animations",
    ],
    variant: "default",
    className: "hidden lg:flex",
  },
  {
    name: "Pro",
    icon: <User className="size-4" />,
    description: "For early-stage founders, solopreneurs and indie devs",
    price: 99,
    priceNote: "Lifetime access. Free updates. No recurring fees.",
    cta: {
      variant: "default",
      label: "Get all-access",
      href: siteConfig.pricing.pro,
    },
    features: [
      `${siteConfig.stats.templates} templates`,
      `${siteConfig.stats.sections} blocks and sections`,
      `${siteConfig.stats.illustrations} illustrations`,
      `${siteConfig.stats.animations} custom animations`,
    ],
    variant: "glow-brand",
  },
  {
    name: "Pro Team",
    icon: <Users className="size-4" />,
    description: "For teams and agencies working on cool products together",
    price: 499,
    priceNote: "Lifetime access. Free updates. No recurring fees.",
    cta: {
      variant: "default",
      label: "Get all-access for your team",
      href: siteConfig.pricing.team,
    },
    features: [
      "All the templates, components and sections available for your entire team",
    ],
    variant: "glow",
  },
];

export default function Pricing({
  title = "Build your dream landing page, today.",
  description = "Get lifetime access to all the components. No recurring fees. Just simple, transparent pricing.",
  plans = DEFAULT_PRICING_PLANS,
  className = "",
}: PricingProps) {
  return (
    <Section className={cn(className)}>
      <div className="mx-auto flex max-w-6xl flex-col items-center gap-12">
        {(title || description) && (
          <div className="flex flex-col items-center gap-4 px-4 text-center sm:gap-8">
            {title && (
              <h2 className="text-3xl leading-tight font-semibold sm:text-5xl sm:leading-tight">
                {title}
              </h2>
            )}
            {description && (
              <p className="text-md text-muted-foreground max-w-[600px] font-medium sm:text-xl">
                {description}
              </p>
            )}
          </div>
        )}
        {plans !== false && plans.length > 0 && (
          <div className="max-w-container mx-auto grid grid-cols-1 gap-8 sm:grid-cols-2 lg:grid-cols-3">
            {plans.map((plan) => (
              <PricingColumn
                key={plan.name}
                name={plan.name}
                icon={plan.icon}
                description={plan.description}
                price={plan.price}
                originalPrice={plan.originalPrice}
                promotionText={plan.promotionText}
                priceNote={plan.priceNote}
                cta={plan.cta}
                features={plan.features}
                variant={plan.variant}
                className={plan.className}
              />
            ))}
          </div>
        )}
      </div>
    </Section>
  );
}

```

---

### Stats Section <a name="section-stats"></a>

Company stats and numbers grid with counts, labels, and borders.

- **Category**: Landing Page Sections
- **Local Path**: `components\sections\stats\default.tsx`

#### Component Source Code

```tsx
import { siteConfig } from "@/config/site";

import { Section } from "../../ui/section";

interface StatItemProps {
  label?: string;
  value: string | number;
  suffix?: string;
  description?: string;
}

interface StatsProps {
  items?: StatItemProps[] | false;
  className?: string;
}

function formatToThousands(value: number) {
  return Math.round(value / 100) / 10;
}

const DEFAULT_STATS: StatItemProps[] = [
  {
    label: "used by",
    value: formatToThousands(siteConfig.stats.figma),
    suffix: "k",
    description: "designers on Figma Community",
  },
  {
    label: "over",
    value: siteConfig.stats.github,
    description: "clones and forks of the template on GitHub",
  },
  {
    label: "already",
    value: formatToThousands(siteConfig.stats.cli),
    suffix: "k",
    description: "installations with shadcn/ui CLI",
  },
  {
    label: "includes",
    value: siteConfig.stats.sections,
    description: "blocks and sections",
  },
];

export default function Stats({
  items = DEFAULT_STATS,
  className,
}: StatsProps) {
  return (
    <Section className={className}>
      <div className="container mx-auto max-w-[960px]">
        {items !== false && items.length > 0 && (
          <div className="grid grid-cols-2 gap-12 sm:grid-cols-4">
            {items.map((item) => (
              <div
                key={`${item.label}-${item.description}`}
                className="flex flex-col items-start gap-3 text-left"
              >
                {item.label && (
                  <div className="text-muted-foreground text-sm font-semibold">
                    {item.label}
                  </div>
                )}
                <div className="flex items-baseline gap-2">
                  <div className="from-foreground to-foreground dark:to-brand bg-linear-to-r bg-clip-text text-4xl font-medium text-transparent drop-shadow-[2px_1px_24px_var(--brand-foreground)] transition-all duration-300 sm:text-5xl md:text-6xl">
                    {item.value}
                  </div>
                  {item.suffix && (
                    <div className="text-brand text-2xl font-semibold">
                      {item.suffix}
                    </div>
                  )}
                </div>
                {item.description && (
                  <div className="text-muted-foreground text-sm font-semibold text-pretty">
                    {item.description}
                  </div>
                )}
              </div>
            ))}
          </div>
        )}
      </div>
    </Section>
  );
}

```

---

## Assembled Page Example <a name="assembled-page-example"></a>

The following example shows how to import and assemble the landing page sections inside a Next.js App Router `page.tsx` file:

```tsx
import CTA from "../components/sections/cta/default";
import FAQ from "../components/sections/faq/default";
import Footer from "../components/sections/footer/default";
import Hero from "../components/sections/hero/default";
import Items from "../components/sections/items/default";
import Logos from "../components/sections/logos/default";
import Navbar from "../components/sections/navbar/default";
import Pricing from "../components/sections/pricing/default";
import Stats from "../components/sections/stats/default";
import { LayoutLines } from "../components/ui/layout-lines";

export default function Home() {
  return (
    <main className="bg-background text-foreground min-h-screen w-full">
      <LayoutLines />
      <Navbar />
      <Hero />
      <Logos />
      <Items />
      <Stats />
      <Pricing />
      <FAQ />
      <CTA />
      <Footer />
    </main>
  );
}

```
