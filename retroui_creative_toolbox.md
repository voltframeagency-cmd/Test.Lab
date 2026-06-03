# RetroUI Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **RetroUI** (retroui.dev) — a library of neubrutalist UI components, cards, loaders, tables, and charts built with Tailwind CSS, Framer Motion, Radix, and TypeScript.

## Table of Contents

- [UI Components](#ui-components)
  - [Accordion](#accordion)
  - [Alert](#alert)
  - [Avatar](#avatar)
  - [Badge](#badge)
  - [Breadcrumb](#breadcrumb)
  - [Button](#button)
  - [Calendar](#calendar)
  - [Card](#card)
  - [Carousel](#carousel)
  - [Checkbox](#checkbox)
  - [Command](#command)
  - [Context Menu](#context-menu)
  - [Dialog](#dialog)
  - [Drawer](#drawer)
  - [Empty](#empty)
  - [Input](#input)
  - [Label](#label)
  - [Loader](#loader)
  - [Menu](#menu)
  - [Popover](#popover)
  - [Progress](#progress)
  - [Radio](#radio)
  - [Select](#select)
  - [Slider](#slider)
  - [Sonner](#sonner)
  - [Switch](#switch)
  - [Tabs](#tab)
  - [Table](#table)
  - [Text](#text)
  - [Textarea](#textarea)
  - [Table of Contents](#toc)
  - [Toggle Group](#toggle-group)
  - [Toggle](#toggle)
  - [Tooltip](#tooltip)
- [Charts](#charts)
  - [Area Chart](#area-chart)
  - [Bar Chart](#bar-chart)
  - [Line Chart](#line-chart)
  - [Pie Chart](#pie-chart)

---

## UI Components <a name="ui-components"></a>

### Accordion <a name="accordion"></a>

This collapsible component let's your users read only the content they care about. 😌

- **Category**: UI Components
- **Registry Key**: `accordion`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/accordion.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Accordion.tsx`

```tsx
"use client";

import { Accordion as BaseAccordion } from "@base-ui/react/accordion";
import { ChevronDown } from "lucide-react";

import { cn } from "@/lib/utils";

const Accordion = BaseAccordion.Root;

const AccordionItem = ({ className, ref, ...props }: BaseAccordion.Item.Props) => (
  <BaseAccordion.Item
    ref={ref}
    className={cn(
      "border-2 bg-background rounded text-foreground shadow-md hover:shadow-sm data-[open]:shadow-sm transition-all overflow-hidden",
      className,
    )}
    {...props}
  />
);

const AccordionHeader = ({ className, children, ref, ...props }: BaseAccordion.Trigger.Props) => (
  <BaseAccordion.Header className="flex">
    <BaseAccordion.Trigger
      ref={ref}
      className={cn(
        "flex flex-1 items-start justify-between px-4 py-2 font-head cursor-pointer focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-primary [&[data-open]>svg]:rotate-180",
        className,
      )}
      {...props}
    >
      {children}
      <ChevronDown className="h-4 w-4 shrink-0 transition-transform duration-200" />
    </BaseAccordion.Trigger>
  </BaseAccordion.Header>
);

const AccordionContent = ({ className, children, ref, ...props }:   BaseAccordion.Panel.Props) => (
  <BaseAccordion.Panel
    ref={ref}
    className="overflow-hidden font-body bg-white text-gray-700 data-[open]:animate-accordion-down data-[closed]:animate-accordion-up"
    {...props}
  >
    <div className={cn("px-4 pt-2 pb-4", className)}>{children}</div>
  </BaseAccordion.Panel>
);

const AccordionComponent = Object.assign(Accordion, {
  Item: AccordionItem,
  Header: AccordionHeader,
  Content: AccordionContent,
});

export { AccordionComponent as Accordion };
```

---

### Alert <a name="alert"></a>

Notify your users about important events and updates. 📣

- **Category**: UI Components
- **Registry Key**: `alert`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/alert.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Alert.tsx`

```tsx
import { HtmlHTMLAttributes } from "react";
import { cva, type VariantProps } from "class-variance-authority";

import { cn } from "@/lib/utils";
import { Text } from "@/components/retroui/Text";

const alertVariants = cva("relative w-full rounded border-2 p-4", {
  variants: {
    variant: {
      default: "bg-background text-foreground [&_svg]:shrink-0",
      solid: "bg-black text-white",
    },
    status: {
      error: "bg-red-300 text-red-800 border-red-800",
      success: "bg-green-300 text-green-800 border-green-800",
      warning: "bg-yellow-300 text-yellow-800 border-yellow-800",
      info: "bg-blue-300 text-blue-800 border-blue-800",
    },
  },
  defaultVariants: {
    variant: "default",
  },
});

interface IAlertProps
  extends HtmlHTMLAttributes<HTMLDivElement>,
    VariantProps<typeof alertVariants> {}

const Alert = ({ className, variant, status, ...props }: IAlertProps) => (
  <div
    role="alert"
    className={cn(alertVariants({ variant, status }), className)}
    {...props}
  />
);
Alert.displayName = "Alert";

interface IAlertTitleProps extends HtmlHTMLAttributes<HTMLHeadingElement> {}
const AlertTitle = ({ className, ...props }: IAlertTitleProps) => (
  <Text as="h5" className={cn(className)} {...props} />
);
AlertTitle.displayName = "AlertTitle";

interface IAlertDescriptionProps
  extends HtmlHTMLAttributes<HTMLParagraphElement> {}
const AlertDescription = ({ className, ...props }: IAlertDescriptionProps) => (
  <div className={cn("text-muted-foreground", className)} {...props} />
);

AlertDescription.displayName = "AlertDescription";

const AlertComponent = Object.assign(Alert, {
  Title: AlertTitle,
  Description: AlertDescription,
});

export { AlertComponent as Alert };
```

---

### Avatar <a name="avatar"></a>

Default rounded avatar that can show your users profile picture. ✨

- **Category**: UI Components
- **Registry Key**: `avatar`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/avatar.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Avatar.tsx`

```tsx
import * as React from "react";
import { Avatar as BaseAvatar } from "@base-ui/react/avatar";

import { cn } from "@/lib/utils";

const Avatar = ({ className, ref, ...props }: React.ComponentPropsWithRef<typeof BaseAvatar.Root>) => (
  <BaseAvatar.Root
    ref={ref}
    className={cn(
      "relative flex h-14 w-14 border-2 rounded-full overflow-hidden",
      className,
    )}
    {...props}
  />
);

const AvatarImage = ({ className, ref, ...props }: React.ComponentPropsWithRef<typeof BaseAvatar.Image>) => (
  <BaseAvatar.Image
    ref={ref}
    className={cn("aspect-square h-full w-full", className)}
    {...props}
  />
);

const AvatarFallback = ({ className, ref, ...props }: React.ComponentPropsWithRef<typeof BaseAvatar.Fallback>) => (
  <BaseAvatar.Fallback
    ref={ref}
    className={cn(
      "flex h-full w-full items-center justify-center rounded-full bg-primary text-primary-foreground",
      className,
    )}
    {...props}
  />
);

const AvatarComponent = Object.assign(Avatar, {
  Image: AvatarImage,
  Fallback: AvatarFallback,
});

export { AvatarComponent as Avatar };
```

---

### Badge <a name="badge"></a>

The component that looks like a button but isn't clickable!

- **Category**: UI Components
- **Registry Key**: `badge`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/badge.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Badge.tsx`

```tsx
import { cn } from "@/lib/utils";
import { cva, VariantProps } from "class-variance-authority";
import { HTMLAttributes } from "react";

const badgeVariants = cva("font-semibold rounded inline-flex items-center", {
  variants: {
    variant: {
      default: "bg-muted text-muted-foreground",
      outline: "outline-2 outline-foreground text-foreground",
      solid: "bg-foreground text-background",
      surface: "outline-2 bg-primary text-primary-foreground",
    },
    size: {
      sm: "px-2 py-1 text-xs",
      md: "px-2.5 py-1.5 text-sm",
      lg: "px-3 py-2 text-base",
    },
  },
  defaultVariants: {
    variant: "default",
    size: "md",
  },
});

interface ButtonProps
  extends HTMLAttributes<HTMLSpanElement>,
    VariantProps<typeof badgeVariants> {}

export function Badge({
  children,
  size = "md",
  variant = "default",
  className = "",
  ...props
}: ButtonProps) {
  return (
    <span
      className={cn(badgeVariants({ variant, size }), className)}
      {...props}
    >
      {children}
    </span>
  );
}
```

---

### Breadcrumb <a name="breadcrumb"></a>

A navigation component that shows users where they are within a hierarchy.

- **Category**: UI Components
- **Registry Key**: `breadcrumb`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/breadcrumb.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Breadcrumb.tsx`

```tsx
import * as React from "react"
import { ChevronRight, MoreHorizontal } from "lucide-react"
import { cn } from "@/lib/utils"

const BreadcrumbRoot = React.forwardRef<
  HTMLElement,
  React.ComponentPropsWithoutRef<"nav">
>(({ className, ...props }, ref) => (
  <nav
    ref={ref}
    aria-label="breadcrumb"
    className={cn("w-full text-sm", className)}
    {...props}
  />
))
BreadcrumbRoot.displayName = "Breadcrumb"

const BreadcrumbList = React.forwardRef<
  HTMLOListElement,
  React.ComponentPropsWithoutRef<"ol">
>(({ className, ...props }, ref) => (
  <ol
    ref={ref}
    className={cn(
      "flex flex-wrap items-center gap-1.5 sm:gap-2.5 text-muted-foreground",
      className
    )}
    {...props}
  />
))
BreadcrumbList.displayName = "BreadcrumbList"

const BreadcrumbItem = React.forwardRef<
  HTMLLIElement,
  React.ComponentPropsWithoutRef<"li">
>(({ className, ...props }, ref) => (
  <li ref={ref} className={cn("inline-flex items-center", className)} {...props} />
))
BreadcrumbItem.displayName = "BreadcrumbItem"

const BreadcrumbLink = React.forwardRef<
  HTMLAnchorElement,
  React.ComponentPropsWithoutRef<"a"> & { asChild?: boolean }
>(({ asChild, className, ...props }, ref) => {
  const Comp = asChild ? "span" : "a"
  return (
    <Comp
      ref={ref}
      className={cn(
        "font-medium transition-colors hover:text-foreground focus:outline-none focus:ring-2 focus:ring-ring rounded-sm",
        className
      )}
      {...props}
    />
  )
})
BreadcrumbLink.displayName = "BreadcrumbLink"

const BreadcrumbPage = React.forwardRef<
  HTMLSpanElement,
  React.ComponentPropsWithoutRef<"span">
>(({ className, ...props }, ref) => (
  <span
    ref={ref}
    aria-current="page"
    className={cn("text-foreground font-semibold", className)}
    {...props}
  />
))
BreadcrumbPage.displayName = "BreadcrumbPage"

const BreadcrumbSeparator = ({
  children,
  className,
  ...props
}: React.ComponentProps<"li">) => (
  <li
    role="presentation"
    aria-hidden="true"
    className={cn("text-muted-foreground [&>svg]:h-4 [&>svg]:w-4", className)}
    {...props}
  >
    {children ?? <ChevronRight />}
  </li>
)
BreadcrumbSeparator.displayName = "BreadcrumbSeparator"

const BreadcrumbEllipsis = ({
  className,
  ...props
}: React.ComponentProps<"span">) => (
  <span
    role="presentation"
    className={cn("flex h-9 w-9 items-center justify-center", className)}
    {...props}
  >
    <MoreHorizontal className="h-4 w-4" />
    <span className="sr-only">More</span>
  </span>
)
BreadcrumbEllipsis.displayName = "BreadcrumbEllipsis"

const Breadcrumb = Object.assign(BreadcrumbRoot, {
  List: BreadcrumbList,
  Item: BreadcrumbItem,
  Link: BreadcrumbLink,
  Page: BreadcrumbPage,
  Separator: BreadcrumbSeparator,
  Ellipsis: BreadcrumbEllipsis,
})

export { Breadcrumb }
```

---

### Button <a name="button"></a>

This bold button makes sure your users click on it and perform the actions you want! 🚀

- **Category**: UI Components
- **Registry Key**: `button`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/button.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Button.tsx`

```tsx
import { cn } from "@/lib/utils";
import { cva, VariantProps } from "class-variance-authority";
import React, { ButtonHTMLAttributes } from "react";
import { Button as BaseButton } from "@base-ui/react/button";

export const buttonVariants = cva(
  "font-head transition-all rounded cursor-pointer duration-200 font-medium flex justify-center items-center disabled:opacity-60 disabled:cursor-not-allowed focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-primary",
  {
    variants: {
      variant: {
        default:
          "shadow-md hover:shadow active:shadow-none bg-primary text-primary-foreground border-2 border-black transition hover:translate-y-1 active:translate-y-2 active:translate-x-1 hover:bg-primary-hover",
        secondary:
          "shadow-md hover:shadow active:shadow-none bg-secondary shadow-primary text-secondary-foreground border-2 border-black transition hover:translate-y-1 active:translate-y-2 active:translate-x-1 hover:bg-secondary-hover",
        outline:
          "shadow-md hover:shadow active:shadow-none bg-transparent border-2 transition hover:translate-y-1 active:translate-y-2 active:translate-x-1",
        link: "bg-transparent hover:underline",
        ghost: "bg-transparent hover:bg-accent"
      },
      size: {
        sm: "px-3 py-1 text-sm shadow hover:shadow-sm",
        md: "px-4 py-1.5 text-base",
        lg: "px-6 lg:px-8 py-2 lg:py-3 text-md lg:text-lg",
        icon: "p-2",
      },
    },
    defaultVariants: {
      size: "md",
      variant: "default",
    },
  },
);

export interface IButtonProps
  extends ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  render?: React.ReactElement | ((props: Record<string, any>) => React.ReactElement);
}

export const Button = ({
  children,
  size = "md",
  className = "",
  variant = "default",
  render,
  ref,
  ...props
}: IButtonProps & { ref?: React.Ref<HTMLButtonElement> }) => {
  return (
    <BaseButton
      ref={ref}
      className={cn(buttonVariants({ variant, size }), className)}
      render={render}
      {...props}
    >
      {children}
    </BaseButton>
  );
};
```

---

### Calendar <a name="calendar"></a>

Let your users select a date to cancel subscription.

- **Category**: UI Components
- **Registry Key**: `calendar`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/calendar.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`
  - `react-day-picker`

#### Component Source Code

##### File: `components/retroui/Calendar.tsx`

```tsx
"use client"

import * as React from "react"
import {
  ChevronDownIcon,
  ChevronLeftIcon,
  ChevronRightIcon,
} from "lucide-react"
import { DayButton, DayPicker, getDefaultClassNames } from "react-day-picker"

import { cn } from "@/lib/utils"
import { Button, buttonVariants } from "@/components/retroui/Button"

function Calendar({
  className,
  classNames,
  showOutsideDays = true,
  captionLayout = "label",
  buttonVariant = "ghost",
  formatters,
  components,
  ...props
}: React.ComponentProps<typeof DayPicker> & {
  buttonVariant?: React.ComponentProps<typeof Button>["variant"]
}) {
  const defaultClassNames = getDefaultClassNames()

  return (
    <DayPicker
      showOutsideDays={showOutsideDays}
      className={cn(
        "bg-background w-full outline-2 shadow-md group/calendar p-3 [--cell-size:--spacing(8)] [[data-slot=card-content]_&]:bg-transparent [[data-slot=popover-content]_&]:bg-transparent",
        String.raw`rtl:**:[.rdp-button\_next>svg]:rotate-180`,
        String.raw`rtl:**:[.rdp-button\_previous>svg]:rotate-180`,
        className
      )}
      captionLayout={captionLayout}
      formatters={{
        formatMonthDropdown: (date) =>
          date.toLocaleString("default", { month: "short" }),
        ...formatters,
      }}
      classNames={{
        root: cn("w-fit", defaultClassNames.root),
        months: cn(
          "flex gap-4 flex-col md:flex-row relative",
          defaultClassNames.months
        ),
        month: cn("flex flex-col w-full gap-4 font-head", defaultClassNames.month),
        nav: cn(
          "flex items-center gap-1 w-full absolute top-0 inset-x-0 justify-between",
          defaultClassNames.nav
        ),
        button_previous: cn(
          buttonVariants({ variant: buttonVariant }),
          "size-8 p-2 border-2 rounded select-none",
          defaultClassNames.button_previous
        ),
        button_next: cn(
          buttonVariants({ variant: buttonVariant }),
          "size-8 p-2 border-2 rounded select-none",
          defaultClassNames.button_next
        ),
        month_caption: cn(
          "flex items-center justify-center h-(--cell-size) w-full px-(--cell-size)",
          defaultClassNames.month_caption
        ),
        dropdowns: cn(
          "w-full flex items-center text-sm font-medium justify-center h-(--cell-size) gap-1.5",
          defaultClassNames.dropdowns
        ),
        dropdown_root: cn(
          "relative has-focus:outline-ring outline outline-input has-focus:ring-ring/50 has-focus:ring-[3px] rounded",
          defaultClassNames.dropdown_root
        ),
        dropdown: cn(
          "absolute bg-popover inset-0 opacity-0",
          defaultClassNames.dropdown
        ),
        caption_label: cn(
          "select-none font-medium",
          captionLayout === "label"
            ? "text-base"
            : "rounded-none pl-2 pr-1 flex items-center gap-1 text-sm h-8 [&>svg]:text-muted-foreground [&>svg]:size-3.5",
          defaultClassNames.caption_label
        ),
        table: "w-full outline-collapse",
        weekdays: cn("flex", defaultClassNames.weekdays),
        weekday: cn(
          "flex-1 font-normal text-sm select-none",
          defaultClassNames.weekday
        ),
        week: cn("flex w-full mt-2", defaultClassNames.week),
        week_number_header: cn(
          "select-none w-(--cell-size)",
          defaultClassNames.week_number_header
        ),
        week_number: cn(
          "text-[0.8rem] select-none text-muted-foreground",
          defaultClassNames.week_number
        ),
        day: cn(
          "relative w-full h-full p-0 text-center [&:last-child[data-selected=true]_button]:rounded-r group/day aspect-square select-none",
          props.showWeekNumber
            ? "[&:nth-child(2)[data-selected=true]_button]:rounded-l"
            : "[&:first-child[data-selected=true]_button]:rounded-l",
          defaultClassNames.day
        ),
        today: cn(
          "bg-accent text-accent-foreground rounded data-[selected=true]:rounded-none",
          defaultClassNames.today
        ),
        outside: cn(
          "text-muted-foreground aria-selected:text-muted-foreground opacity-80",
          defaultClassNames.outside
        ),
        disabled: cn(
          "text-muted-foreground opacity-50",
          defaultClassNames.disabled
        ),
        hidden: cn("invisible", defaultClassNames.hidden),
        ...classNames,
      }}
      components={{
        Root: ({ className, rootRef, ...props }) => {
          return (
            <div
              data-slot="calendar"
              ref={rootRef}
              className={cn(className)}
              {...props}
            />
          )
        },
        Chevron: ({ className, orientation, ...props }) => {
          if (orientation === "left") {
            return (
              <ChevronLeftIcon className={cn("size-4", className)} {...props} />
            )
          }

          if (orientation === "right") {
            return (
              <ChevronRightIcon
                className={cn("size-4", className)}
                {...props}
              />
            )
          }

          return (
            <ChevronDownIcon className={cn("size-4", className)} {...props} />
          )
        },
        DayButton: CalendarDayButton,
        WeekNumber: ({ children, ...props }) => {
          return (
            <td {...props}>
              <div className="flex size-(--cell-size) items-center justify-center text-center">
                {children}
              </div>
            </td>
          )
        },
        ...components,
      }}
      {...props}
    />
  )
}

function CalendarDayButton({
  className,
  day,
  modifiers,
  ...props
}: React.ComponentProps<typeof DayButton>) {
  const defaultClassNames = getDefaultClassNames()

  const ref = React.useRef<HTMLButtonElement>(null)
  React.useEffect(() => {
    if (modifiers.focused) ref.current?.focus()
  }, [modifiers.focused])

  return (
    <Button
      ref={ref}
      variant="ghost"
      size="icon"
      data-day={day.date.toLocaleDateString()}
      data-selected-single={
        modifiers.selected &&
        !modifiers.range_start &&
        !modifiers.range_end &&
        !modifiers.range_middle
      }
      data-range-start={modifiers.range_start}
      data-range-end={modifiers.range_end}
      data-range-middle={modifiers.range_middle}
      className={cn(
        "font-sans flex justify-center items-center data-[selected-single=true]:shadow-md data-[selected-single=true]:outline-2 outline-border data-[selected-single=true]:bg-primary data-[selected-single=true]:text-primary-foreground data-[range-middle=true]:bg-secondary data-[range-middle=true]:hover:text-secondary-foreground data-[range-middle=true]:text-secondary-foreground data-[range-start=true]:bg-primary data-[range-start=true]:text-primary-foreground data-[range-end=true]:bg-primary data-[range-end=true]:text-primary-foreground group-data-[focused=true]/day:border-ring-1 group-data-[focused=true]/day:ring-ring/50 dark:hover:text-accent-foreground flex aspect-square size-auto w-full min-w-(--cell-size) flex-col gap-1 leading-none font-normal group-data-[focused=true]/day:relative group-data-[focused=true]/day:z-10 group-data-[focused=true]/day:ring-[2px] data-[range-end=true]:rounded-none data-[range-end=true]:rounded-none data-[range-middle=true]:rounded-none data-[range-start=true]:rounded-none [&>span]:text-xs",
        defaultClassNames.day,
        className
      )}
      {...props}
    />
  )
}

export { Calendar, CalendarDayButton }
```

---

### Card <a name="card"></a>

A customizable card component to visualize your content. 📝

- **Category**: UI Components
- **Registry Key**: `card`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/card.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Card.tsx`

```tsx
import { cn } from "@/lib/utils";
import { HTMLAttributes } from "react";
import { Text } from "@/components/retroui/Text";

interface ICardProps extends HTMLAttributes<HTMLDivElement> {
  className?: string;
}

const Card = ({ className, ...props }: ICardProps) => {
  return (
    <div
      className={cn(
        "inline-block border-2 rounded shadow-md transition-all hover:shadow-none bg-card",
        className,
      )}
      {...props}
    />
  );
};

const CardHeader = ({ className, ...props }: ICardProps) => {
  return (
    <div
      className={cn("flex flex-col justify-start p-4", className)}
      {...props}
    />
  );
};

const CardTitle = ({ className, ...props }: ICardProps) => {
  return <Text as="h3" className={cn("mb-2", className)} {...props} />;
};

const CardDescription = ({ className, ...props }: ICardProps) => (
  <p className={cn("text-muted-foreground", className)} {...props} />
);

const CardContent = ({ className, ...props }: ICardProps) => {
  return <div className={cn("p-4", className)} {...props} />;
};

const CardComponent = Object.assign(Card, {
  Header: CardHeader,
  Title: CardTitle,
  Description: CardDescription,
  Content: CardContent,
});

export { CardComponent as Card };
```

---

### Carousel <a name="carousel"></a>

Let your users select a date to cancel subscription.

- **Category**: UI Components
- **Registry Key**: `carousel`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/carousel.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Carousel.tsx`

```tsx
"use client"

import * as React from "react"
import useEmblaCarousel, {
    type UseEmblaCarouselType,
} from "embla-carousel-react"
import { ArrowLeft, ArrowRight } from "lucide-react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/retroui/Button"

type CarouselApi = UseEmblaCarouselType[1]
type UseCarouselParameters = Parameters<typeof useEmblaCarousel>
type CarouselOptions = UseCarouselParameters[0]
type CarouselPlugin = UseCarouselParameters[1]

type CarouselProps = {
    opts?: CarouselOptions
    plugins?: CarouselPlugin
    orientation?: "horizontal" | "vertical"
    setApi?: (api: CarouselApi) => void
}

type CarouselContextProps = {
    carouselRef: ReturnType<typeof useEmblaCarousel>[0]
    api: ReturnType<typeof useEmblaCarousel>[1]
    scrollPrev: () => void
    scrollNext: () => void
    canScrollPrev: boolean
    canScrollNext: boolean
} & CarouselProps

const CarouselContext = React.createContext<CarouselContextProps | null>(null)

function useCarousel() {
    const context = React.useContext(CarouselContext)

    if (!context) {
        throw new Error("useCarousel must be used within a <Carousel />")
    }

    return context
}

function Carousel({
    orientation = "horizontal",
    opts,
    setApi,
    plugins,
    className,
    children,
    ...props
}: React.ComponentProps<"div"> & CarouselProps) {
    const [carouselRef, api] = useEmblaCarousel(
        {
            ...opts,
            axis: orientation === "horizontal" ? "x" : "y",
        },
        plugins
    )
    const [canScrollPrev, setCanScrollPrev] = React.useState(false)
    const [canScrollNext, setCanScrollNext] = React.useState(false)

    const onSelect = React.useCallback((api: CarouselApi) => {
        if (!api) return
        setCanScrollPrev(api.canScrollPrev())
        setCanScrollNext(api.canScrollNext())
    }, [])

    const scrollPrev = React.useCallback(() => {
        api?.scrollPrev()
    }, [api])

    const scrollNext = React.useCallback(() => {
        api?.scrollNext()
    }, [api])

    const handleKeyDown = React.useCallback(
        (event: React.KeyboardEvent<HTMLDivElement>) => {
            if (event.key === "ArrowLeft") {
                event.preventDefault()
                scrollPrev()
            } else if (event.key === "ArrowRight") {
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
        onSelect(api)
        api.on("reInit", onSelect)
        api.on("select", onSelect)

        return () => {
            api?.off("select", onSelect)
        }
    }, [api, onSelect])

    return (
        <CarouselContext.Provider
            value={{
                carouselRef,
                api: api,
                opts,
                orientation:
                    orientation || (opts?.axis === "y" ? "vertical" : "horizontal"),
                scrollPrev,
                scrollNext,
                canScrollPrev,
                canScrollNext,
            }}
        >
            <div
                onKeyDownCapture={handleKeyDown}
                className={cn("relative", className)}
                role="region"
                aria-roledescription="carousel"
                data-slot="carousel"
                {...props}
            >
                {children}
            </div>
        </CarouselContext.Provider>
    )
}

function CarouselContent({ className, ...props }: React.ComponentProps<"div">) {
    const { carouselRef, orientation } = useCarousel()

    return (
        <div
            ref={carouselRef}
            className="overflow-hidden"
            data-slot="carousel-content"
        >
            <div
                className={cn(
                    "flex",
                    orientation === "horizontal" ? "-ml-4" : "-mt-4 flex-col",
                    className
                )}
                {...props}
            />
        </div>
    )
}

function CarouselItem({ className, ...props }: React.ComponentProps<"div">) {
    const { orientation } = useCarousel()

    return (
        <div
            role="group"
            aria-roledescription="slide"
            data-slot="carousel-item"
            className={cn(
                "min-w-0 shrink-0 grow-0 basis-full",
                orientation === "horizontal" ? "pl-4" : "pt-4",
                className
            )}
            {...props}
        />
    )
}

function CarouselPrevious({
    className,
    variant = "outline",
    size = "icon",
    ...props
}: React.ComponentProps<typeof Button>) {
    const { orientation, scrollPrev, canScrollPrev } = useCarousel()

    return (
        <Button
            data-slot="carousel-previous"
            variant={variant}
            size={size}
            className={cn(
                "absolute size-8 rounded",
                orientation === "horizontal"
                    ? "top-1/2 -left-12 -translate-y-1/2 hover:-translate-y-[calc(50%-2px)] active:-translate-y-[calc(50%-4px)]"
                    : "-top-12 left-1/2 -translate-x-1/2 rotate-90 hover:-translate-x-[calc(50%-2px)] active:-translate-x-[calc(50%-4px)]",
                className
            )}
            disabled={!canScrollPrev}
            onClick={scrollPrev}
            {...props}
        >
            <ArrowLeft />
            <span className="sr-only">Previous slide</span>
        </Button>
    )
}

function CarouselNext({
    className,
    variant = "outline",
    size = "icon",
    ...props
}: React.ComponentProps<typeof Button>) {
    const { orientation, scrollNext, canScrollNext } = useCarousel()

    return (
        <Button
            data-slot="carousel-next"
            variant={variant}
            size={size}
            className={cn(
                "absolute size-8 rounded",
                orientation === "horizontal"
                    ? "top-1/2 -right-12 -translate-y-1/2 hover:-translate-y-[calc(50%-2px)] active:-translate-y-[calc(50%-4px)]"
                    : "-bottom-12 left-1/2 -translate-x-1/2 rotate-90 hover:-translate-x-[calc(50%-2px)] active:-translate-x-[calc(50%-4px)]",
                className
            )}
            disabled={!canScrollNext}
            onClick={scrollNext}
            {...props}
        >
            <ArrowRight />
            <span className="sr-only">Next slide</span>
        </Button>
    )
}

const CarouselObject = Object.assign(Carousel, {
    Content: CarouselContent,
    Item: CarouselItem,
    Previous: CarouselPrevious,
    Next: CarouselNext,
})

export { CarouselObject as Carousel }
```

---

### Checkbox <a name="checkbox"></a>

Let users confirm or reject an option.

- **Category**: UI Components
- **Registry Key**: `checkbox`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/checkbox.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Checkbox.tsx`

```tsx
import { cn } from "@/lib/utils";
import { Checkbox as BaseCheckbox } from "@base-ui/react/checkbox";
import { cva, VariantProps } from "class-variance-authority";
import { Check } from "lucide-react";

const checkboxVariants = cva("border-2 rounded", {
  variants: {
    variant: {
      default: "data-[checked]:bg-primary data-[checked]:text-primary-foreground ",
      outline: "",
      solid:
        "data-[checked]:bg-foreground data-[checked]:text-background",
    },
    size: {
      sm: "h-4 w-4",
      md: "h-5 w-5",
      lg: "h-6 w-6",
    },
  },
  defaultVariants: {
    variant: "default",
    size: "md",
  },
});

interface CheckboxProps
  extends React.ComponentProps<typeof BaseCheckbox.Root>,
    VariantProps<typeof checkboxVariants> {}

export const Checkbox = ({
  className,
  size,
  variant,
  ...props
}: CheckboxProps) => (
  <BaseCheckbox.Root
    className={cn(
      checkboxVariants({
        size,
        variant,
      }),
      className,
    )}
    {...props}
  >
    <BaseCheckbox.Indicator className="h-full w-full">
      <Check className="h-full w-full" />
    </BaseCheckbox.Indicator>
  </BaseCheckbox.Root>
);
```

---

### Command <a name="command"></a>

A command palette component for quick navigation and actions

- **Category**: UI Components
- **Registry Key**: `command`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/command.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Command.tsx`

```tsx
"use client";

import React from "react";
import { cn } from "@/lib/utils";
import { Dialog } from "@/components/retroui/Dialog";
import { Command as CommandPrimitive } from "cmdk";
import { Check, LucideIcon, Search } from "lucide-react";

function Command({
  className,
  ...props
}: React.ComponentProps<typeof CommandPrimitive>) {
  return (
    <CommandPrimitive
      className={cn(
        "flex h-full w-full flex-col overflow-hidden rounded bg-background text-foreground border-border shadow-md",
        className,
      )}
      {...props}
    />
  );
}

type CommandDialogProps = Omit<React.ComponentProps<typeof Dialog>, 'children'> & {
  className?: string;
  children?: React.ReactNode;
};

const CommandDialog = ({
  children,
  className,
  ...props
}: CommandDialogProps) => {
  return (
    <Dialog {...props}>
      <Dialog.Content
        className={cn(
          "overflow-hidden p-0 shadow-lg rounded w-full max-w-md",
          className,
        )}
      >
        <Command className="[&_[cmdk-group-heading]]:px-2 [&_[cmdk-group-heading]]:font-medium [&_[cmdk-group-heading]]:text-muted-foreground [&_[cmdk-group]:not([hidden])_~[cmdk-group]]:pt-0 [&_[cmdk-group]]:px-2 [&_[cmdk-input-wrapper]_svg]:h-5 [&_[cmdk-input-wrapper]_svg]:w-5 [&_[cmdk-input]]:h-12 [&_[cmdk-item]]:px-2 [&_[cmdk-item]]:py-3 [&_[cmdk-item]_svg]:h-5 [&_[cmdk-item]_svg]:w-5">
          {children}
        </Command>
      </Dialog.Content>
    </Dialog>
  );
};

function CommandInput({
  className,
  ...props
}: React.ComponentProps<typeof CommandPrimitive.Input>) {
  return (
    <div
      className="flex items-center border-border border-b px-3"
      cmdk-input-wrapper=""
      data-slot="command-input"
    >
      <Search className="me-2 h-4 w-4 shrink-0 opacity-50 text-foreground" />
      <CommandPrimitive.Input
        className={cn(
          "flex h-11 w-full rounded bg-transparent py-3 text-sm focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-primary text-foreground placeholder:text-muted-foreground disabled:cursor-not-allowed disabled:opacity-50 font-body",
          className,
        )}
        {...props}
      />
    </div>
  );
}

function CommandList({
  className,
  ...props
}: React.ComponentProps<typeof CommandPrimitive.List>) {
  return (
    <CommandPrimitive.List
      data-slot="command-list"
      className={cn(
        "max-h-[400px] overflow-auto overscroll-contain transition-[height]  h-[calc(min(300px,var(--cmdk-list-height)))] bg-background",
        className,
      )}
      {...props}
    />
  );
}

function CommandEmpty({
  ...props
}: React.ComponentProps<typeof CommandPrimitive.Empty>) {
  return (
    <CommandPrimitive.Empty
      data-slot="command-empty"
      className="py-6 text-center text-sm text-muted-foreground font-body"
      {...props}
    />
  );
}

function CommandGroup({
  className,
  ...props
}: React.ComponentProps<typeof CommandPrimitive.Group>) {
  return (
    <CommandPrimitive.Group
      data-slot="command-group"
      className={cn(
        "overflow-hidden p-1.5 text-foreground [&_[cmdk-group-heading]]:px-2 [&_[cmdk-group-heading]]:py-1.5 [&_[cmdk-group-heading]]:text-xs [&_[cmdk-group-heading]]:font-medium [&_[cmdk-group-heading]]:text-muted-foreground font-body",
        className,
      )}
      {...props}
    />
  );
}

function CommandSeparator({
  className,
  ...props
}: React.ComponentProps<typeof CommandPrimitive.Separator>) {
  return (
    <CommandPrimitive.Separator
      data-slot="command-separator"
      className={cn("h-px bg-border", className)}
      {...props}
    />
  );
}

function CommandItem({
  className,
  ...props
}: React.ComponentProps<typeof CommandPrimitive.Item>) {
  return (
    <CommandPrimitive.Item
      data-slot="command-item"
      className={cn(
        "relative flex text-foreground cursor-pointer gap-2 select-none items-center rounded px-2 py-1.5 text-sm focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-primary data-[disabled=true]:pointer-events-none data-[selected=true]:bg-primary data-[selected=true]:text-primary-foreground data-[disabled=true]:opacity-50 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0 hover:bg-accent hover:text-accent-foreground transition-all font-body",
        className,
      )}
      {...props}
    />
  );
}

const CommandShortcut = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLSpanElement>) => {
  return (
    <span
      data-slot="command-shortcut"
      className={cn(
        "ms-auto text-xs tracking-widest text-muted-foreground font-body",
        className,
      )}
      {...props}
    />
  );
};

interface CommandCheckProps {
  icon?: LucideIcon;
  className?: string;
}

function CommandCheck({
  icon: Icon = Check,
  className,
  ...props
}: CommandCheckProps) {
  return (
    <Icon
      data-slot="command-check"
      data-check="true"
      className={cn("size-4 ms-auto text-primary", className)}
      {...props}
    />
  );
}

const CommandComponent = Object.assign(Command, {
  Check: CommandCheck,
  Dialog: CommandDialog,
  Empty: CommandEmpty,
  Group: CommandGroup,
  Input: CommandInput,
  Item: CommandItem,
  List: CommandList,
  Separator: CommandSeparator,
  Shortcut: CommandShortcut,
});

export { CommandComponent as Command };
```

---

### Context Menu <a name="context-menu"></a>

Displays a menu to the user — such as a set of actions or functions — triggered by a button.

- **Category**: UI Components
- **Registry Key**: `context-menu`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/context-menu.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/ContextMenu.tsx`

```tsx
"use client";

import * as React from "react";
import { ContextMenu as BaseContextMenu } from "@base-ui/react/context-menu";
import { CheckIcon, ChevronRightIcon, CircleIcon } from "lucide-react";

import { cn } from "@/lib/utils";

function ContextMenu({
  ...props
}: React.ComponentProps<typeof BaseContextMenu.Root>) {
  return <BaseContextMenu.Root data-slot="context-menu" {...props} />;
}

function ContextMenuTrigger({
  ...props
}: React.ComponentProps<typeof BaseContextMenu.Trigger>) {
  return (
    <BaseContextMenu.Trigger data-slot="context-menu-trigger" {...props} />
  );
}

function ContextMenuGroup({
  ...props
}: React.ComponentProps<typeof BaseContextMenu.Group>) {
  return (
    <BaseContextMenu.Group data-slot="context-menu-group" {...props} />
  );
}

function ContextMenuPortal({
  ...props
}: React.ComponentProps<typeof BaseContextMenu.Portal>) {
  return (
    <BaseContextMenu.Portal data-slot="context-menu-portal" {...props} />
  );
}

function ContextMenuSub({
  ...props
}: React.ComponentProps<typeof BaseContextMenu.SubmenuTrigger>) {
  return <BaseContextMenu.SubmenuTrigger data-slot="context-menu-sub" {...props} />;
}

function ContextMenuRadioGroup({
  ...props
}: React.ComponentProps<typeof BaseContextMenu.RadioGroup>) {
  return (
    <BaseContextMenu.RadioGroup
      data-slot="context-menu-radio-group"
      {...props}
    />
  );
}

function ContextMenuSubTrigger({
  className,
  inset,
  children,
  ...props
}: React.ComponentProps<typeof BaseContextMenu.SubmenuTrigger> & {
  inset?: boolean;
}) {
  return (
    <BaseContextMenu.SubmenuTrigger
      data-slot="context-menu-sub-trigger"
      data-inset={inset}
      className={cn(
        "focus:bg-primary focus:text-primary-foreground data-[open]:bg-primary data-[open]:text-primary-foreground flex cursor-default items-center rounded-sm px-2 py-1.5 text-sm focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-primary select-none data-[inset]:pl-8 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 transition-colors",
        className,
      )}
      {...props}
    >
      {children}
      <ChevronRightIcon className="ml-auto" />
    </BaseContextMenu.SubmenuTrigger>
  );
}

function ContextMenuSubContent({
  className,
  ...props
}: React.ComponentProps<typeof BaseContextMenu.Popup>) {
  return (
    <BaseContextMenu.Popup
      data-slot="context-menu-sub-content"
      className={cn(
        "bg-background text-foreground border-2 shadow-md data-[open]:animate-in data-[closed]:animate-out data-[closed]:fade-out-0 data-[open]:fade-in-0 data-[closed]:zoom-out-95 data-[open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 z-50 min-w-[8rem] overflow-hidden rounded-sm p-1",
        className,
      )}
      {...props}
    />
  );
}

function ContextMenuContent({
  className,
  ...props
}: React.ComponentProps<typeof BaseContextMenu.Popup>) {
  return (
    <BaseContextMenu.Portal>
      <BaseContextMenu.Popup
        data-slot="context-menu-content"
        className={cn(
          "bg-background text-foreground border-2 shadow-md data-[open]:animate-in data-[closed]:animate-out data-[closed]:fade-out-0 data-[open]:fade-in-0 data-[closed]:zoom-out-95 data-[open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 z-50 min-w-[8rem] overflow-x-hidden overflow-y-auto rounded-sm p-1",
          className,
        )}
        {...props}
      />
    </BaseContextMenu.Portal>
  );
}

function ContextMenuItem({
  className,
  inset,
  variant = "default",
  ...props
}: React.ComponentProps<typeof BaseContextMenu.Item> & {
  inset?: boolean;
  variant?: "default" | "destructive";
}) {
  return (
    <BaseContextMenu.Item
      data-slot="context-menu-item"
      data-inset={inset}
      data-variant={variant}
      className={cn(
        "focus:bg-primary focus:text-primary-foreground data-[variant=destructive]:text-destructive data-[variant=destructive]:focus:bg-destructive/10 dark:data-[variant=destructive]:focus:bg-destructive/20 data-[variant=destructive]:focus:text-destructive data-[variant=destructive]:*:[svg]:!text-destructive [&_svg:not([class*='text-'])]:text-muted-foreground relative flex cursor-default items-center gap-2 rounded-sm px-2 py-1.5 text-sm focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-primary select-none data-[disabled]:pointer-events-none data-[disabled]:opacity-50 data-[inset]:pl-8 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 transition-colors",
        className,
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
}: React.ComponentProps<typeof BaseContextMenu.CheckboxItem>) {
  return (
    <BaseContextMenu.CheckboxItem
      data-slot="context-menu-checkbox-item"
      className={cn(
        "focus:bg-primary focus:text-primary-foreground relative flex cursor-default items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-primary select-none data-[disabled]:pointer-events-none data-[disabled]:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 transition-colors",
        className,
      )}
      checked={checked}
      {...props}
    >
      <span className="pointer-events-none absolute left-2 flex size-3.5 items-center justify-center">
        <BaseContextMenu.CheckboxItemIndicator>
          <CheckIcon className="size-4" />
        </BaseContextMenu.CheckboxItemIndicator>
      </span>
      {children}
    </BaseContextMenu.CheckboxItem>
  );
}

function ContextMenuRadioItem({
  className,
  children,
  ...props
}: React.ComponentProps<typeof BaseContextMenu.RadioItem>) {
  return (
    <BaseContextMenu.RadioItem
      data-slot="context-menu-radio-item"
      className={cn(
        "focus:bg-primary focus:text-primary-foreground relative flex cursor-default items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-primary select-none data-[disabled]:pointer-events-none data-[disabled]:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 transition-colors",
        className,
      )}
      {...props}
    >
      <span className="pointer-events-none absolute left-2 flex size-3.5 items-center justify-center">
        <BaseContextMenu.RadioItemIndicator>
          <CircleIcon className="size-2 fill-current" />
        </BaseContextMenu.RadioItemIndicator>
      </span>
      {children}
    </BaseContextMenu.RadioItem>
  );
}

function ContextMenuLabel({
  className,
  inset,
  ...props
}: React.ComponentProps<"div"> & {
  inset?: boolean;
}) {
  return (
    <div
      data-slot="context-menu-label"
      data-inset={inset}
      className={cn(
        "text-foreground px-2 py-1.5 text-sm font-medium data-[inset]:pl-8",
        className,
      )}
      {...props}
    />
  );
}

function ContextMenuSeparator({
  className,
  ...props
}: React.ComponentProps<typeof BaseContextMenu.Separator>) {
  return (
    <BaseContextMenu.Separator
      data-slot="context-menu-separator"
      className={cn("bg-border -mx-1 my-1 h-px", className)}
      {...props}
    />
  );
}

function ContextMenuShortcut({
  className,
  ...props
}: React.ComponentProps<"span">) {
  return (
    <span
      data-slot="context-menu-shortcut"
      className={cn(
        "text-muted-foreground ml-auto text-xs tracking-widest",
        className,
      )}
      {...props}
    />
  );
}

const ContextMenuComponent = Object.assign(ContextMenu, {
  Trigger: ContextMenuTrigger,
  Content: ContextMenuContent,
  Item: ContextMenuItem,
  CheckboxItem: ContextMenuCheckboxItem,
  RadioItem: ContextMenuRadioItem,
  Label: ContextMenuLabel,
  Separator: ContextMenuSeparator,
  Shortcut: ContextMenuShortcut,
  Group: ContextMenuGroup,
  Portal: ContextMenuPortal,
  Sub: ContextMenuSub,
  SubContent: ContextMenuSubContent,
  SubTrigger: ContextMenuSubTrigger,
  RadioGroup: ContextMenuRadioGroup,
});

export { ContextMenuComponent as ContextMenu };
```

---

### Dialog <a name="dialog"></a>

An impactful dialog that ensures your important messages and actions get the attention they deserve! 💬✨

- **Category**: UI Components
- **Registry Key**: `dialog`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/dialog.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Dialog.tsx`

```tsx
"use client";

import { Dialog as BaseDialog } from "@base-ui/react/dialog";
import { cn } from "@/lib/utils";
import { cva, VariantProps } from "class-variance-authority";
import React, { HTMLAttributes, ReactNode } from "react";
import { X } from "lucide-react";

const Dialog = BaseDialog.Root;
const DialogTrigger = BaseDialog.Trigger;

const overlayVariants = cva(
  ` fixed bg-black/80 font-head
    data-[open]:fade-in-0
    data-[open]:animate-in
    data-[closed]:animate-out
    data-[closed]:fade-out-0
  `,
  {
    variants: {
      variant: {
        default: "inset-0 z-50 bg-black/85",
        none: "fixed bg-transparent",
      },
    },
    defaultVariants: {
      variant: "default",
    },
  },
);

interface IDialogBackgroupProps
  extends HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof overlayVariants> {}

const DialogBackdrop = (inputProps: IDialogBackgroupProps & { ref?: React.Ref<HTMLDivElement> }) => {
  const { variant = "default", className, ref, ...props } = inputProps;

  return (
    <BaseDialog.Backdrop
      className={cn(overlayVariants({ variant }), className)}
      ref={ref}
      {...props}
    />
  );
};

const dialogVariants = cva(
  `fixed left-[50%] top-[50%] z-50 grid rounded overflow-hidden w-full max-w-lg translate-x-[-50%] translate-y-[-50%] gap-4 border-2 bg-background shadow-lg duration-200
  data-[open]:animate-in
  data-[open]:fade-in-0
  data-[open]:zoom-in-95
  data-[closed]:animate-out
  data-[closed]:fade-out-0
  data-[closed]:zoom-out-95`,
  {
    variants: {
      size: {
        auto: "max-w-fit",
        sm: "lg:max-w-[30%]",
        md: "lg:max-w-[40%]",
        lg: "lg:max-w-[50%]",
        xl: "lg:max-w-[60%]",
        "2xl": "lg:max-w-[70%]",
        "3xl": "lg:max-w-[80%]",
        "4xl": "lg:max-w-[90%]",
        screen: "max-w-[100%]",
      },
    },
    defaultVariants: {
      size: "auto",
    },
  },
);

interface IDialogContentProps
  extends HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof dialogVariants> {
  overlay?: IDialogBackgroupProps;
}

const DialogContent = (inputProps: IDialogContentProps & { ref?: React.Ref<HTMLDivElement> }) => {
  const {
    children,
    size = "auto",
    className,
    overlay,
    ref,
    ...props
  } = inputProps;

  return (
    <BaseDialog.Portal>
      <DialogBackdrop {...overlay} />
      <BaseDialog.Popup
        className={cn(dialogVariants({ size }), className)}
        ref={ref}
        {...props}
      >
        <BaseDialog.Title className="sr-only" />
        <div className="flex flex-col relative">{children}</div>
      </BaseDialog.Popup>
    </BaseDialog.Portal>
  );
};

interface IDialogDescriptionProps extends HTMLAttributes<HTMLDivElement> {}
const DialogDescription = ({
  children,
  className,
  ...props
}: IDialogDescriptionProps) => {
  return (
    <BaseDialog.Description className={cn(className)} {...props}>
      {children}
    </BaseDialog.Description>
  );
};

const dialogFooterVariants = cva(
  "flex items-center justify-end border-t-2 min-h-12 gap-4 px-4 py-2",
  {
    variants: {
      variant: {
        default: "bg-background text-foreground",
      },
      position: {
        fixed: "sticky bottom-0",
        static: "static",
      },
    },
    defaultVariants: {
      position: "fixed",
    },
  },
);

export interface IDialogFooterProps
  extends HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof dialogFooterVariants> {}

const DialogFooter = ({
  children,
  className,
  position,
  variant,
  ...props
}: IDialogFooterProps) => {
  return (
    <div
      className={cn(dialogFooterVariants({ position, variant }), className)}
      {...props}
    >
      {children}
    </div>
  );
};

const dialogHeaderVariants = cva(
  "flex items-center justify-between border-b-2 px-4 min-h-12",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground",
      },
      position: {
        fixed: "sticky top-0",
        static: "static",
      },
    },
    defaultVariants: {
      variant: "default",
      position: "static",
    },
  },
);

const DialogHeaderDefaultLayout = ({ children }: { children: ReactNode }) => {
  return (
    <>
      {children}
      <BaseDialog.Close title="Close pop-up" className="cursor-pointer">
        <X />
      </BaseDialog.Close>
    </>
  );
};

interface IDialogHeaderProps
  extends HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof dialogHeaderVariants> {
  asChild?: boolean;
}

const DialogHeader = ({
  children,
  className,
  position,
  variant,
  asChild,
  ...props
}: IDialogHeaderProps) => {
  return (
    <div
      className={cn(dialogHeaderVariants({ position, variant }), className)}
      {...props}
    >
      {asChild ? (
        children
      ) : (
        <DialogHeaderDefaultLayout>{children}</DialogHeaderDefaultLayout>
      )}
    </div>
  );
};

const DialogComponent = Object.assign(Dialog, {
  Trigger: DialogTrigger,
  Header: DialogHeader,
  Content: DialogContent,
  Description: DialogDescription,
  Footer: DialogFooter,
  Close: BaseDialog.Close,
});

export { DialogComponent as Dialog };
```

---

### Drawer <a name="drawer"></a>

A component that can slide in from any side of the screen.

- **Category**: UI Components
- **Registry Key**: `drawer`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/drawer.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Drawer.tsx`

```tsx
"use client"

import * as React from "react"
import { Drawer as BaseDrawer } from "@base-ui/react/drawer"

import { cn } from "@/lib/utils"

function Drawer({
  ...props
}: React.ComponentProps<typeof BaseDrawer.Root>) {
  return <BaseDrawer.Root data-slot="drawer" {...props} />
}

function DrawerTrigger({
  ...props
}: React.ComponentProps<typeof BaseDrawer.Trigger>) {
  return <BaseDrawer.Trigger data-slot="drawer-trigger" {...props} />
}

function DrawerPortal({
  ...props
}: React.ComponentProps<typeof BaseDrawer.Portal>) {
  return <BaseDrawer.Portal data-slot="drawer-portal" {...props} />
}

function DrawerClose({
  ...props
}: React.ComponentProps<typeof BaseDrawer.Close>) {
  return <BaseDrawer.Close data-slot="drawer-close" {...props} />
}

function DrawerBackdrop({
  className,
  ...props
}: React.ComponentProps<typeof BaseDrawer.Backdrop>) {
  return (
    <BaseDrawer.Backdrop
      data-slot="drawer-backdrop"
      className={cn(
        "data-[open]:animate-in data-[closed]:animate-out data-[closed]:fade-out-0 data-[open]:fade-in-0 fixed inset-0 z-50 bg-black/50",
        className
      )}
      {...props}
    />
  )
}

function DrawerContent({
  className,
  children,
  ...props
}: React.ComponentProps<typeof BaseDrawer.Popup>) {
  return (
    <DrawerPortal data-slot="drawer-portal">
      <DrawerBackdrop />
      <BaseDrawer.Popup
        data-slot="drawer-content"
        className={cn(
          "group/drawer-content bg-background fixed z-50 flex h-auto flex-col",
          "data-[side=top]:inset-x-0 data-[side=top]:top-0 data-[side=top]:mb-24 data-[side=top]:max-h-[80vh] data-[side=top]:rounded-b data-[side=top]:border-b-2",
          "data-[side=bottom]:inset-x-0 data-[side=bottom]:bottom-0 data-[side=bottom]:mt-24 data-[side=bottom]:max-h-[80vh] data-[side=bottom]:rounded-t data-[side=bottom]:border-t-2",
          "data-[side=right]:inset-y-0 data-[side=right]:right-0 data-[side=right]:w-3/4 data-[side=right]:border-l-2 data-[side=right]:sm:max-w-sm",
          "data-[side=left]:inset-y-0 data-[side=left]:left-0 data-[side=left]:w-3/4 data-[side=left]:border-r-2 data-[side=left]:sm:max-w-sm",
          className
        )}
        {...props}
      >
        <div className="bg-foreground mx-auto mt-4 hidden h-1 w-[60px] shrink-0 rounded-full group-data-[side=bottom]/drawer-content:block" />
        {children}
      </BaseDrawer.Popup>
    </DrawerPortal>
  )
}

function DrawerHeader({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="drawer-header"
      className={cn(
        "flex flex-col gap-0.5 p-4 group-data-[side=bottom]/drawer-content:text-center group-data-[side=top]/drawer-content:text-center md:gap-1.5 md:text-left",
        className
      )}
      {...props}
    />
  )
}

function DrawerFooter({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="drawer-footer"
      className={cn("mt-auto flex flex-col gap-2 p-4", className)}
      {...props}
    />
  )
}

function DrawerTitle({
  className,
  ...props
}: React.ComponentProps<typeof BaseDrawer.Title>) {
  return (
    <BaseDrawer.Title
      data-slot="drawer-title"
      className={cn("text-foreground text-xl font-head font-medium", className)}
      {...props}
    />
  )
}

function DrawerDescription({
  className,
  ...props
}: React.ComponentProps<typeof BaseDrawer.Description>) {
  return (
    <BaseDrawer.Description
      data-slot="drawer-description"
      className={cn("text-muted-foreground text-sm", className)}
      {...props}
    />
  )
}

const DrawerComponent = Object.assign(Drawer, {
    Trigger: DrawerTrigger,
    Portal: DrawerPortal,
    Backdrop: DrawerBackdrop,
    Close: DrawerClose,
    Content: DrawerContent,
    Header: DrawerHeader,
    Footer: DrawerFooter,
    Title: DrawerTitle,
    Description: DrawerDescription,
});

export { DrawerComponent as Drawer };
```

---

### Empty <a name="empty"></a>

The component that shows when there is no data to show!

- **Category**: UI Components
- **Registry Key**: `empty`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/empty.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Empty.tsx`

```tsx
import { Text } from "@/components/retroui/Text";
import { cn } from "@/lib/utils";
import { Ghost } from "lucide-react";
import { HTMLAttributes } from "react";

interface IEmptyProps extends HTMLAttributes<HTMLDivElement> {
  className?: string;
}

const Empty = ({ className, ...props }: IEmptyProps) => {
  return (
    <div
      className={cn(
        "flex flex-col items-center justify-center p-4 md:p-8 border-2 rounded shadow-md transition-all hover:shadow-none bg-card text-center",
        className,
      )}
      {...props}
    />
  );
};
Empty.displayName = "Empty";

const EmptyContent = ({ className, ...props }: IEmptyProps) => {
  return (
    <div
      className={cn("flex flex-col items-center gap-3", className)}
      {...props}
    />
  );
};
EmptyContent.displayName = "Empty.Content";

const EmptyIcon = ({ children, className, ...props }: IEmptyProps) => {
  return (
    <div className={cn(className)} {...props}>
      {children || <Ghost className="w-full h-full" />}
    </div>
  );
};
EmptyIcon.displayName = "Empty.Icon";

const EmptyTitle = ({ className, ...props }: IEmptyProps) => {
  return (
    <Text
      as="h3"
      className={cn("text-lg md:text-2xl font-bold", className)}
      {...props}
    />
  );
};
EmptyTitle.displayName = "Empty.Title";

const EmptySeparator = ({ className, ...props }: IEmptyProps) => {
  return <div role="separator" className={cn("w-full h-1 bg-primary", className)} {...props} />;
};
EmptySeparator.displayName = "Empty.Separator";

const EmptyDescription = ({
  className,
  ...props
}: HTMLAttributes<HTMLParagraphElement>) => (
  <p
    className={cn("text-muted-foreground max-w-[320px]", className)}
    {...props}
  />
);
EmptyDescription.displayName = "Empty.Description";

const EmptyComponent = Object.assign(Empty, {
  Content: EmptyContent,
  Icon: EmptyIcon,
  Title: EmptyTitle,
  Separator: EmptySeparator,
  Description: EmptyDescription,
});

export { EmptyComponent as Empty };
```

---

### Input <a name="input"></a>

This pretty input makes your users want to type stuff! ⌨️

- **Category**: UI Components
- **Registry Key**: `input`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/input.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Input.tsx`

```tsx
import React, { InputHTMLAttributes } from "react";

interface InputProps extends InputHTMLAttributes<HTMLInputElement> {
  className?: string;
}

export const Input: React.FC<InputProps> = ({
  type = "text",
  placeholder = "Enter text",
  className = "",
  ...props
}) => {
  return (
    <input
      type={type}
      placeholder={placeholder}
      className={`px-4 py-2 w-full rounded border-2 shadow-md transition focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-primary focus:shadow-xs ${
        props["aria-invalid"]
          ? "border-destructive text-destructive shadow-xs shadow-destructive"
          : ""
      } ${className}`}
      {...props}
    />
  );
};
```

---

### Label <a name="label"></a>

A universal component for labeling various inputs.

- **Category**: UI Components
- **Registry Key**: `label`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/label.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### How to Use

```tsx
import { Label } from "@/components/retroui/Label";
```

#### Component Source Code

##### File: `components/retroui/Label.tsx`

```tsx
import * as React from "react";
import { Field } from "@base-ui/react/field";
import { cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

const labelVariants = cva(
  "leading-none peer-disabled:cursor-not-allowed peer-disabled:opacity-70",
);

export const Label = ({
  className,
  ...props
}: React.ComponentProps<typeof Field.Label>) => (
  <Field.Label className={cn(labelVariants(), className)} {...props} />
);
```

---

### Loader <a name="loader"></a>

A customizable loading indicator with bouncing squares. 🔄

- **Category**: UI Components
- **Registry Key**: `loader`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/loader.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### How to Use

```tsx
import { Loader } from "@/components/ui/loader"

// Default
<Loader />

// With variants
<Loader variant="secondary" size="lg" />

// Custom configuration
<Loader
  count={4}           // Number of squares
  duration={1.2}      // Animation duration in seconds
  delayStep={120}     // Delay between squares in milliseconds
/>

// Fully customized
<Loader
  variant="secondary"
  size="lg"
  count={4}
  duration={1.2}
  delayStep={120}
  className="my-4"
/>
```

#### Component Source Code

##### File: `components/retroui/Loader.tsx`

```tsx
import * as React from "react";
import { cva, type VariantProps } from "class-variance-authority";
import { cn } from "@/lib/utils";

const loaderVariants = cva("flex gap-1", {
  variants: {
    variant: {
      default: "[&>div]:bg-primary [&>div]:border-border",
      secondary:
        "[&>div]:bg-secondary [&>div]:border-border",
      outline: "[&>div]:bg-transparent [&>div]:border-border",
    },
    size: {
      sm: "[&>div]:w-2 [&>div]:h-2",
      md: "[&>div]:w-3 [&>div]:h-3",
      lg: "[&>div]:w-4 [&>div]:h-4",
    },
  },
  defaultVariants: {
    variant: "default",
    size: "md",
  },
});

interface LoaderProps
  extends Omit<React.HTMLAttributes<HTMLDivElement>, "color">,
    VariantProps<typeof loaderVariants> {
  asChild?: boolean;
  count?: number; // number of bouncing dots
  duration?: number; // animation duration in seconds
  delayStep?: number; // delay in ms
}

const Loader = React.forwardRef<HTMLDivElement, LoaderProps>(
  (
    {
      className,
      variant,
      size,
      count = 3,
      duration = 0.5,
      delayStep = 100,
      ...props
    },
    ref,
  ) => {
    return (
      <div
        className={cn(loaderVariants({ variant, size }), className)}
        ref={ref}
        role="status"
        aria-label="Loading..."
        {...props}
      >
        {Array.from({ length: count }).map((_, i) => (
          <div
            key={i}
            className="border-2 animate-bounce"
            style={{
              animationDuration: `${duration}s`,
              animationIterationCount: "infinite",
              animationDelay: `${i * delayStep}ms`,
            }}
          />
        ))}
      </div>
    );
  },
);

Loader.displayName = "Loader";
export { Loader };
```

---

### Menu <a name="menu"></a>

Show your users a list of actions they can take. 📋

- **Category**: UI Components
- **Registry Key**: `menu`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/menu.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Menu.tsx`

```tsx
"use client";

import { cn } from "@/lib/utils";
import { Menu as BaseMenu } from "@base-ui/react/menu";
import React, { ComponentPropsWithoutRef } from "react";

const Menu = BaseMenu.Root;
const Trigger = BaseMenu.Trigger;

interface IMenuContent
  extends ComponentPropsWithoutRef<typeof BaseMenu.Popup> {}

const Content = ({ className, ...props }: IMenuContent) => (
  <BaseMenu.Portal>
    <BaseMenu.Popup
      className={cn(
        "bg-white border-2 shadow-md absolute top-2 min-w-20",
        className,
      )}
      {...props}
    />
  </BaseMenu.Portal>
);

const MenuItem = React.forwardRef<
  HTMLDivElement,
  ComponentPropsWithoutRef<typeof BaseMenu.Item>
>(({ className, ...props }, ref) => (
  <BaseMenu.Item
    ref={ref}
    className={cn(
      "relative text-black flex cursor-default select-none items-center rounded-xs px-2 py-1.5 text-sm outline-hidden transition-colors hover:bg-primary focus:bg-primary data-[disabled]:pointer-events-none data-[disabled]:opacity-50",
      className,
    )}
    {...props}
  />
));
MenuItem.displayName = "MenuItem";

const MenuComponent = Object.assign(Menu, {
  Trigger,
  Content,
  Item: MenuItem,
});

export { MenuComponent as Menu };
```

---

### Popover <a name="popover"></a>

A handy small component for your little input needs...😉

- **Category**: UI Components
- **Registry Key**: `popover`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/popover.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Popover.tsx`

```tsx
"use client";

import * as React from "react";
import { Popover as PopoverPrimitive } from "@base-ui/react/popover";

import { cn } from "@/lib/utils";

function Popover({ ...props }: PopoverPrimitive.Root.Props) {
  return <PopoverPrimitive.Root data-slot="popover" {...props} />;
}

function PopoverTrigger({ ...props }: PopoverPrimitive.Trigger.Props) {
  return <PopoverPrimitive.Trigger data-slot="popover-trigger" {...props} />;
}

function PopoverContent({
  className,
  align = "center",
  alignOffset = 0,
  side = "bottom",
  sideOffset = 4,
  ...props
}: PopoverPrimitive.Popup.Props &
  Pick<
    PopoverPrimitive.Positioner.Props,
    "align" | "alignOffset" | "side" | "sideOffset"
  >) {
  return (
    <PopoverPrimitive.Portal>
      <PopoverPrimitive.Positioner
        align={align}
        alignOffset={alignOffset}
        side={side}
        sideOffset={sideOffset}
        className="isolate z-50"
      >
        <PopoverPrimitive.Popup
          data-slot="popover-content"
          className={cn(
            "z-50 w-72 origin-(--transform-origin) border-2 border-border bg-background p-4 text-popover-foreground shadow-md focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-primary duration-100 data-[side=bottom]:slide-in-from-top-2 data-[side=inline-end]:slide-in-from-left-2 data-[side=inline-start]:slide-in-from-right-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 data-closed:animate-out data-closed:fade-out-0 data-closed:zoom-out-95",
            className
          )}
          {...props}
        />
      </PopoverPrimitive.Positioner>
    </PopoverPrimitive.Portal>
  );
}

function PopoverHeader({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="popover-header"
      className={cn("flex flex-col gap-1 text-sm", className)}
      {...props}
    />
  );
}

function PopoverTitle({ className, ...props }: PopoverPrimitive.Title.Props) {
  return (
    <PopoverPrimitive.Title
      data-slot="popover-title"
      className={cn("text-base font-medium", className)}
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
      data-slot="popover-description"
      className={cn("text-muted-foreground", className)}
      {...props}
    />
  );
}

const PopoverObject = Object.assign(Popover, {
  Trigger: PopoverTrigger,
  Content: PopoverContent,
  Header: PopoverHeader,
  Title: PopoverTitle,
  Description: PopoverDescription,
});

export { PopoverObject as Popover };
```

---

### Progress <a name="progress"></a>

Let's you users know how long to wait.

- **Category**: UI Components
- **Registry Key**: `progress`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/progress.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Progress.tsx`

```tsx
"use client";

import * as React from "react";
import { Progress as BaseProgress } from "@base-ui/react/progress";

import { cn } from "@/lib/utils";

const Progress = ({ className, value, ref, ...props }: React.ComponentPropsWithRef<typeof BaseProgress.Root>) => (
  <BaseProgress.Root
    ref={ref}
    className={cn(
      "relative h-4 w-full overflow-hidden bg-background border-2",
      className,
    )}
    value={value}
    {...props}
  >
    <BaseProgress.Indicator
      className="h-full w-full flex-1 bg-primary transition-all"
      style={{ transform: `translateX(-${100 - (value || 0)}%)` }}
    />
  </BaseProgress.Root>
);

export { Progress };
```

---

### Radio <a name="radio"></a>

Sometimes you need to pick multiple options! That's where the Radio component comes into play.

- **Category**: UI Components
- **Registry Key**: `radio`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/radio.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Radio.tsx`

```tsx
"use client";

import { cn } from "@/lib/utils";
import { Radio as RadioPrimitive } from "@base-ui/react/radio";
import { RadioGroup as RadioGroupPrimitive } from "@base-ui/react/radio-group";
import { cva, VariantProps } from "class-variance-authority";

const radioVariants = cva("border-border border-2", {
  variants: {
    variant: {
      default: "",
      outline: "",
      solid: "",
    },
    size: {
      sm: "h-4 w-4",
      md: "h-5 w-5",
      lg: "h-6 w-6",
    },
  },
  defaultVariants: {
    variant: "default",
    size: "md",
  },
});

const radioIndicatorVariants = cva("flex", {
  variants: {
    variant: {
      default: "bg-primary border-2 border-border",
      outline: "border-2 border-border",
      solid: "bg-border",
    },
    size: {
      sm: "h-2 w-2",
      md: "h-2.5 w-2.5",
      lg: "h-3.5 w-3.5",
    },
  },
  defaultVariants: {
    variant: "default",
    size: "md",
  },
});

function RadioGroupRoot({ className, ...props }: RadioGroupPrimitive.Props) {
  return (
    <RadioGroupPrimitive
      data-slot="radio-group"
      className={cn("grid gap-2", className)}
      {...props}
    />
  );
}

interface RadioItemProps
  extends RadioPrimitive.Root.Props,
    VariantProps<typeof radioVariants> {}

function RadioItem({
  className,
  size,
  variant,
  ...props
}: RadioItemProps) {
  return (
    <RadioPrimitive.Root
      data-slot="radio-group-item"
      className={cn(
        radioVariants({
          size,
          variant,
        }),
        className
      )}
      {...props}
    >
      <RadioPrimitive.Indicator
        data-slot="radio-group-indicator"
        className="flex items-center justify-center"
      >
        <span className={radioIndicatorVariants({ size, variant })} />
      </RadioPrimitive.Indicator>
    </RadioPrimitive.Root>
  );
}

const RadioComponent = Object.assign(RadioGroupRoot, {
  Item: RadioItem,
});

export { RadioComponent as RadioGroup };
```

---

### Select <a name="select"></a>

Let your users pick what they want.

- **Category**: UI Components
- **Registry Key**: `select`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/select.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Select.tsx`

```tsx
"use client";

import * as React from "react";
import { Select as SelectPrimitive } from "@base-ui/react/select";
import { Check, ChevronDown, ChevronUp } from "lucide-react";

import { cn } from "@/lib/utils";

const Select = SelectPrimitive.Root;

function SelectGroup({ className, ...props }: SelectPrimitive.Group.Props) {
  return (
    <SelectPrimitive.Group
      data-slot="select-group"
      className={cn("scroll-my-1 p-1", className)}
      {...props}
    />
  );
}

function SelectValue({ className, ...props }: SelectPrimitive.Value.Props) {
  return (
    <SelectPrimitive.Value
      data-slot="select-value"
      className={cn("flex flex-1 text-left", className)}
      {...props}
    />
  );
}

function SelectTrigger({
  className,
  children,
  ...props
}: SelectPrimitive.Trigger.Props) {
  return (
    <SelectPrimitive.Trigger
      data-slot="select-trigger"
      className={cn(
        "flex h-10 rounded min-w-40 items-center shadow-md focus:shadow-xs justify-between border-2 border-input border-border bg-input px-4 py-2 placeholder:text-muted-foreground outline-none focus:outline-none focus-visible:outline-none disabled:cursor-not-allowed disabled:opacity-50 whitespace-nowrap data-placeholder:text-muted-foreground [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    >
      {children}
      <SelectPrimitive.Icon
        render={
          <ChevronDown className="ml-2 h-4 w-4 text-muted-foreground" />
        }
      />
    </SelectPrimitive.Trigger>
  );
}

function SelectContent({
  className,
  children,
  side = "bottom",
  sideOffset = 4,
  align = "center",
  alignOffset = 0,
  alignItemWithTrigger = true,
  ...props
}: SelectPrimitive.Popup.Props &
  Pick<
    SelectPrimitive.Positioner.Props,
    "align" | "alignOffset" | "side" | "sideOffset" | "alignItemWithTrigger"
  >) {
  return (
    <SelectPrimitive.Portal>
      <SelectPrimitive.Positioner
        side={side}
        sideOffset={sideOffset}
        align={align}
        alignOffset={alignOffset}
        alignItemWithTrigger={alignItemWithTrigger}
        className="isolate z-50"
      >
        <SelectPrimitive.Popup
          data-slot="select-content"
          className={cn(
            "relative z-50 max-h-(--available-height) w-(--anchor-width) min-w-[8rem] origin-(--transform-origin) overflow-x-hidden overflow-y-auto border-2 border-border bg-background text-foreground shadow-md duration-100 data-[side=bottom]:slide-in-from-top-2 data-[side=inline-end]:slide-in-from-left-2 data-[side=inline-start]:slide-in-from-right-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 data-closed:animate-out data-closed:fade-out-0 data-closed:zoom-out-95",
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
      data-slot="select-label"
      className={cn("py-1.5 px-2 text-sm font-semibold", className)}
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
      data-slot="select-item"
      className={cn(
        "relative flex w-full cursor-default select-none items-center py-1.5 px-2 outline-none data-[highlighted]:bg-primary data-[highlighted]:text-primary-foreground focus:bg-primary focus:text-primary-foreground data-disabled:pointer-events-none data-disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    >
      <SelectPrimitive.ItemText className="flex flex-1 shrink-0">
        {children}
      </SelectPrimitive.ItemText>
      <SelectPrimitive.ItemIndicator
        render={
          <span className="absolute right-2 flex h-3.5 w-3.5 items-center justify-center" />
        }
      >
        <Check className="h-4 w-4 text-foreground" />
      </SelectPrimitive.ItemIndicator>
    </SelectPrimitive.Item>
  );
}

function SelectSeparator({
  className,
  ...props
}: SelectPrimitive.Separator.Props) {
  return (
    <SelectPrimitive.Separator
      data-slot="select-separator"
      className={cn("-mx-1 my-1 h-px bg-border", className)}
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
      data-slot="select-scroll-up-button"
      className={cn(
        "flex cursor-default items-center justify-center py-1 text-muted-foreground bg-background [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    >
      <ChevronUp />
    </SelectPrimitive.ScrollUpArrow>
  );
}

function SelectScrollDownButton({
  className,
  ...props
}: React.ComponentProps<typeof SelectPrimitive.ScrollDownArrow>) {
  return (
    <SelectPrimitive.ScrollDownArrow
      data-slot="select-scroll-down-button"
      className={cn(
        "flex cursor-default items-center justify-center py-1 text-muted-foreground bg-background [&_svg:not([class*='size-'])]:size-4",
        className
      )}
      {...props}
    >
      <ChevronDown />
    </SelectPrimitive.ScrollDownArrow>
  );
}

const SelectIcon = SelectPrimitive.Icon;

const SelectObj = Object.assign(Select, {
  Trigger: SelectTrigger,
  Value: SelectValue,
  Icon: SelectIcon,
  Content: SelectContent,
  Group: SelectGroup,
  Item: SelectItem,
  Label: SelectLabel,
  Separator: SelectSeparator,
  ScrollUpButton: SelectScrollUpButton,
  ScrollDownButton: SelectScrollDownButton,
});

export { SelectObj as Select };
```

---

### Slider <a name="slider"></a>

A customizable slider component with two variants.

- **Category**: UI Components
- **Registry Key**: `slider`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/slider.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Slider.tsx`

```tsx
"use client";

import * as React from "react";
import { Slider as BaseSlider } from "@base-ui/react/slider";

import { cn } from "@/lib/utils";

const Slider = ({ className, ref, ...props }: React.ComponentPropsWithRef<typeof BaseSlider.Root>) => (
  <BaseSlider.Root
    ref={ref}
    className={cn(
      "relative flex w-full touch-none select-none items-center",
      className,
    )}
    {...props}
  >
    <BaseSlider.Track className="relative h-3 w-full grow overflow-hidden bg-background border-2">
      <BaseSlider.Indicator className="absolute h-full bg-primary" />
    </BaseSlider.Track>
    <BaseSlider.Thumb className="block h-4.5 w-4.5 border-2 bg-background shadow-sm transition-colors focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-50" />
  </BaseSlider.Root>
);

export { Slider };
```

---

### Sonner <a name="sonner"></a>

A beautiful toast component that can grab audience attention from any place.

- **Category**: UI Components
- **Registry Key**: `sonner`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/sonner.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Sonner.tsx`

```tsx
"use client";

import { Toaster as Sonner } from "sonner";

type ToasterProps = React.ComponentProps<typeof Sonner>;

const Toaster = ({ ...props }: ToasterProps) => {
  return (
    <Sonner
      toastOptions={{
        classNames: {
          toast:
            "h-auto w-full p-4 bg-background border group toast group-[.toaster]:bg-background group-[.toaster]:text-foreground group-[.toaster]:border-border flex items-center relative",
          description:
            "group-[.toast]:text-muted-foreground ml-2 text-sm font-sans",
          actionButton:
            "group-[.toast]:bg-primary group-[.toast]:text-primary-foreground py-1 px-2 bg-background border-border shadow hover:shadow-xs hover:translate-[2px] duration-200 transition-all focus:shadow-none border-2 ml-auto h-fit min-w-fit",
          cancelButton:
            "group-[.toast]:bg-muted group-[.toast]:text-foreground py-1 px-2 text-sm bg-background border-border shadow hover:shadow-xs hover:translate-[2px] duration-200 transition-all focus:shadow-none border-2 ml-auto h-fit min-w-fit",
          title: "ml-2 font-sans",
          closeButton:
            "absolute bg-background -top-1 -left-1 rounded-full p-0.5",
        },
        unstyled: true,
      }}
      {...props}
    />
  );
};

export { Toaster };
```

---

### Switch <a name="switch"></a>

Let users to turn on or off your marketing emails or notifications.

- **Category**: UI Components
- **Registry Key**: `switch`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/switch.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Switch.tsx`

```tsx
"use client";

import * as React from "react";
import { Switch as BaseSwitch } from "@base-ui/react/switch";

import { cn } from "@/lib/utils";

const Switch = ({ className, ...props }: React.ComponentProps<typeof BaseSwitch.Root>) => (
  <BaseSwitch.Root
    className={cn(
      "peer inline-flex h-6 w-11 shrink-0 cursor-pointer border-2 border-foreground items-center disabled:cursor-not-allowed disabled:opacity-50 data-[checked]:bg-primary",
      className,
    )}
    {...props}
  >
    <BaseSwitch.Thumb
      className={cn(
        "pointer-events-none block h-4 w-4 bg-primary border-2 mx-0.5 border-foreground ring-0 transition-transform data-[checked]:translate-x-5 data-[unchecked]:translate-x-0 data-[checked]:bg-background",
      )}
    />
  </BaseSwitch.Root>
);

export { Switch };
```

---

### Tabs <a name="tab"></a>

Switch between different views using tabs.

- **Category**: UI Components
- **Registry Key**: `tab`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/tab.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Tab.tsx`

```tsx
import { cn } from "@/lib/utils";
import { Tabs as BaseTabs } from "@base-ui/react/tabs";

const Tabs = BaseTabs.Root;
const TabsPanels = ({ children, className, ...props }: React.ComponentProps<"div">) => (
  <div className={className} {...props}>
    {children}
  </div>
);

interface ITabsTriggerList extends React.ComponentProps<typeof BaseTabs.List> {
  className?: string;
}
const TabsTriggerList = ({
  children,
  className,
  ...props
}: ITabsTriggerList) => {
  return (
    <BaseTabs.List className={cn("flex flex-row space-x-2 w-full", className)} {...props}>
      {children}
    </BaseTabs.List>
  );
};

interface ITabsTrigger extends React.ComponentProps<typeof BaseTabs.Tab> {
  className?: string;
}
const TabsTrigger = ({ children, className, ...props }: ITabsTrigger) => {
  return (
    <BaseTabs.Tab
      className={cn(
        "px-4 flex items-center py-1 border-2 border-transparent data-[active]:border-border data-[active]:bg-primary data-[active]:text-primary-foreground data-[active]:font-semibold focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-primary",
        className,
      )}
      {...props}
    >
      {children}
    </BaseTabs.Tab>
  );
};

interface ITabsContent extends React.ComponentProps<typeof BaseTabs.Panel> {
  className?: string;
}
const TabsContent = ({ children, className, ...props }: ITabsContent) => {
  return (
    <BaseTabs.Panel className={cn("mt-2 w-full", className)} {...props}>
      {children}
    </BaseTabs.Panel>
  );
};

const TabsObj = Object.assign(BaseTabs.Root, {
  List: TabsTriggerList,
  Trigger: TabsTrigger,
  Content: TabsContent,
});

export { TabsObj as Tabs };
```

---

### Table <a name="table"></a>

Display your data in a structured tabular format.

- **Category**: UI Components
- **Registry Key**: `table`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/table.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Table.tsx`

```tsx
import * as React from "react"

import { cn } from "@/lib/utils"

const Table = React.forwardRef<
    HTMLTableElement,
    React.HTMLAttributes<HTMLTableElement>
>(({ className, ...props }, ref) => (
    <div className="relative h-full w-full overflow-auto">
        <table
            ref={ref}
            className={cn("w-full caption-bottom text-sm border-2 shadow-lg", className)}
            {...props}
        />
    </div>
))
Table.displayName = "Table"

const TableHeader = React.forwardRef<
    HTMLTableSectionElement,
    React.HTMLAttributes<HTMLTableSectionElement>
>(({ className, ...props }, ref) => (
    <thead ref={ref} className={cn("[&_tr]:border-b bg-primary text-primary-foreground font-head", className)} {...props} />
))
TableHeader.displayName = "TableHeader"

const TableBody = React.forwardRef<
    HTMLTableSectionElement,
    React.HTMLAttributes<HTMLTableSectionElement>
>(({ className, ...props }, ref) => (
    <tbody
        ref={ref}
        className={cn("[&_tr:last-child]:border-0", className)}
        {...props}
    />
))
TableBody.displayName = "TableBody"

const TableFooter = React.forwardRef<
    HTMLTableSectionElement,
    React.HTMLAttributes<HTMLTableSectionElement>
>(({ className, ...props }, ref) => (
    <tfoot
        ref={ref}
        className={cn(
            "border-t bg-accent text-accent-foreground font-medium [&>tr]:last:border-b-0",
            className
        )}
        {...props}
    />
))
TableFooter.displayName = "TableFooter"

const TableRow = React.forwardRef<
    HTMLTableRowElement,
    React.HTMLAttributes<HTMLTableRowElement>
>(({ className, ...props }, ref) => (
    <tr
        ref={ref}
        className={cn(
            "border-b transition-colors hover:bg-primary/50 hover:text-primary-foreground data-[state=selected]:bg-muted",
            className
        )}
        {...props}
    />
))
TableRow.displayName = "TableRow"

const TableHead = React.forwardRef<
    HTMLTableCellElement,
    React.ThHTMLAttributes<HTMLTableCellElement>
>(({ className, ...props }, ref) => (
    <th
        ref={ref}
        className={cn(
            "h-10 md:h-12 px-4 text-left align-middle font-medium text-primary-foreground [&:has([role=checkbox])]:pr-0",
            className
        )}
        {...props}
    />
))
TableHead.displayName = "TableHead"

const TableCell = React.forwardRef<
    HTMLTableCellElement,
    React.TdHTMLAttributes<HTMLTableCellElement>
>(({ className, ...props }, ref) => (
    <td
        ref={ref}
        className={cn("p-2 md:p-3 align-middle [&:has([role=checkbox])]:pr-0", className)}
        {...props}
    />
))
TableCell.displayName = "TableCell"

const TableCaption = React.forwardRef<
    HTMLTableCaptionElement,
    React.HTMLAttributes<HTMLTableCaptionElement>
>(({ className, ...props }, ref) => (
    <caption
        ref={ref}
        className={cn("my-2 text-sm text-muted-foreground", className)}
        {...props}
    />
))
TableCaption.displayName = "TableCaption"

const TableObj = Object.assign(Table, {
    Header: TableHeader,
    Body: TableBody,
    Footer: TableFooter,
    Row: TableRow,
    Head: TableHead,
    Cell: TableCell,
    Caption: TableCaption,
})

export {
    TableObj as Table,
}
```

---

### Text <a name="text"></a>

Show your texts in different styles. 💅

- **Category**: UI Components
- **Registry Key**: `text`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/text.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Text.tsx`

```tsx
import { ElementType, HTMLAttributes } from "react";
import { VariantProps, cva } from "class-variance-authority";
import { cn } from "@/lib/utils";

const textVariants = cva("font-head", {
  variants: {
    as: {
      p: "font-sans text-base",
      li: "font-sans text-base",
      a: "font-sans text-base hover:underline underline-offset-2 decoration-primary",
      h1: "text-4xl lg:text-5xl font-bold",
      h2: "text-3xl lg:text-4xl font-semibold",
      h3: "text-2xl font-medium",
      h4: "text-xl font-normal",
      h5: "text-lg font-normal",
      h6: "text-base font-normal",
    },
  },
  defaultVariants: {
    as: "p",
  },
});

interface TextProps
  extends Omit<HTMLAttributes<HTMLElement>, "className">,
    VariantProps<typeof textVariants> {
  className?: string;
}

export const Text = (props: TextProps) => {
  const { className, as, ...otherProps } = props;
  const Tag: ElementType = as || "p";

  return (
    <Tag className={cn(textVariants({ as }), className)} {...otherProps} />
  );
};
```

---

### Textarea <a name="textarea"></a>

This pretty input makes your users want to type lots of stuff! ⌨️ ⌨️

- **Category**: UI Components
- **Registry Key**: `textarea`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/textarea.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Textarea.tsx`

```tsx
import { cn } from "@/lib/utils";

export function Textarea({
  type = "text",
  placeholder = "Enter text...",
  className = "",
  ...props
}) {
  return (
    <textarea
      placeholder={placeholder}
      rows={4}
      className={cn(
        "px-4 py-2 w-full border-2 rounded shadow-md transition focus:outline-hidden focus:shadow-xs placeholder:text-muted-foreground",
        className
      )}
      {...props}
    />
  );
}
```

---

### Table of Contents <a name="toc"></a>

Generate a table of contents for your markdown content.

- **Category**: UI Components
- **Registry Key**: `toc`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/toc.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/TableOfContents.tsx`

```tsx
"use client";

import { cn } from '@/lib/utils';
import React, { useEffect, useState } from 'react';

interface TOCItem {
    title: string;
    url: string;
    level: number;
    items?: TOCItem[];
}

interface ManualTOCItem {
    id: string;
    title: string;
    depth: number;
}

interface TableOfContentsProps {
    depth?: number;
    className?: string;
    children?: React.ReactNode;
    data?: ManualTOCItem[];
}

function generateTOCFromDOM(depth: number = 6): TOCItem[] {
    const headings: NodeListOf<HTMLHeadingElement> = document.querySelectorAll(
        Array.from({ length: depth }, (_, i) => `h${i + 1}`).join(', ')
    );

    const items: TOCItem[] = [];
    const stack: TOCItem[] = [];

    headings.forEach((heading) => {
        const level = parseInt(heading.tagName.charAt(1));
        const id = heading.id || heading.textContent?.toLowerCase().replace(/\s+/g, '-').replace(/[^\w-]/g, '') || '';

        if (!heading.id && id) {
            heading.id = id;
        }

        const item: TOCItem = {
            title: heading.textContent || '',
            url: `#${id}`,
            level,
        };

        while (stack.length > 0 && stack[stack.length - 1].level >= level) {
            stack.pop();
        }

        if (stack.length === 0) {
            items.push(item);
        } else {
            const parent = stack[stack.length - 1];
            if (!parent.items) parent.items = [];
            parent.items.push(item);
        }

        stack.push(item);
    });

    return items;
}

function convertManualDataToTOC(data: ManualTOCItem[]): TOCItem[] {
    const items: TOCItem[] = [];
    const stack: TOCItem[] = [];

    data.forEach((item) => {
        const tocItem: TOCItem = {
            title: item.title,
            url: `#${item.id}`,
            level: item.depth,
        };

        while (stack.length > 0 && stack[stack.length - 1].level >= item.depth) {
            stack.pop();
        }

        if (stack.length === 0) {
            items.push(tocItem);
        } else {
            const parent = stack[stack.length - 1];
            if (!parent.items) parent.items = [];
            parent.items.push(tocItem);
        }

        stack.push(tocItem);
    });

    return items;
}

function renderTOCItems(items: TOCItem[], level = 0, activeId: string | null) {
    if (!items || items.length === 0) return null;

    return (
        <ul className={`space-y-1 ${level > 0 ? 'ml-4 mt-1' : ''}`}>
            {items.map((item, index) => {
                const isActive = activeId === item.url.substring(1);
                return (
                    <li key={index}>
                        <a
                            href={item.url}
                            className={`text-sm max-w-full truncate transition-colors block py-1 border-l-2 pl-2 ${
                                isActive
                                    ? 'text-accent-foreground border-border bg-accent'
                                    : 'border-transparent hover:border-border hover:text-foreground'
                            }`}
                        >
                            {item.title}
                        </a>
                        {item.items && renderTOCItems(item.items, level + 1, activeId)}
                    </li>
                );
            })}
        </ul>
    );
}

export function TableOfContents({
    depth = 2,
    className = "",
    children,
    data,
}: TableOfContentsProps) {
    const [tocItems, setTocItems] = useState<TOCItem[]>([]);
    const [activeId, setActiveId] = useState<string | null>(null);

    useEffect(() => {
        if (data) {
            const items = convertManualDataToTOC(data);
            setTocItems(items);
            return;
        }

        const items = generateTOCFromDOM(depth);
        setTocItems(items);

        const observer = new MutationObserver(() => {
            const updatedItems = generateTOCFromDOM(depth);
            setTocItems(updatedItems);
        });

        observer.observe(document.body, {
            childList: true,
            subtree: true,
            attributes: true,
            attributeFilter: ['id']
        });

        return () => observer.disconnect();
    }, [depth, data]);

    useEffect(() => {
        const observer = new IntersectionObserver(
            (entries) => {
                entries.forEach((entry) => {
                    if (entry.isIntersecting) {
                        setActiveId(entry.target.id);
                    }
                });
            },
            { rootMargin: '-20% 0% -35% 0%' }
        );

        const headings = document.querySelectorAll('h1, h2, h3, h4, h5, h6');
        headings.forEach((heading) => observer.observe(heading));

        return () => observer.disconnect();
    }, []);

    if (tocItems.length === 0) {
        return null;
    }

    return (
        <div className={cn("border-2 shadow-md p-4 rounded w-52 h-60 overflow-y-auto sidebar-scroll", className)}>
            {children}
            {renderTOCItems(tocItems, 0, activeId)}
        </div>
    );
}
```

---

### Toggle Group <a name="toggle-group"></a>

Group of toggling buttons...🤙

- **Category**: UI Components
- **Registry Key**: `toggle-group`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/toggle-group.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### How to Use

##### Example 1

```tsx
import {
  ToggleGroup,
  ToggleGroupItem,
} from "@/components/retroui/toggle-group";
```

##### Example 2

```tsx
<ToggleGroup type="single">
  <ToggleGroupItem value="a">A</ToggleGroupItem>
  <ToggleGroupItem value="b">B</ToggleGroupItem>
  <ToggleGroupItem value="c">C</ToggleGroupItem>
</ToggleGroup>
```

#### Component Source Code

##### File: `components/retroui/ToggleGroup.tsx`

```tsx
"use client";

import * as React from "react";
import { Toggle as TogglePrimitive } from "@base-ui/react/toggle";
import { ToggleGroup as ToggleGroupPrimitive } from "@base-ui/react/toggle-group";
import { type VariantProps } from "class-variance-authority";

import { cn } from "@/lib/utils";
import { toggleVariants } from "./Toggle";

const ToggleGroupContext = React.createContext<
  VariantProps<typeof toggleVariants> & {
    spacing?: number;
    orientation?: "horizontal" | "vertical";
  }
>({
  size: "default",
  variant: "default",
  spacing: 1,
  orientation: "horizontal",
});

function ToggleGroup({
  className,
  variant,
  size,
  spacing = 1,
  orientation = "horizontal",
  children,
  ...props
}: ToggleGroupPrimitive.Props &
  VariantProps<typeof toggleVariants> & {
    spacing?: number;
    orientation?: "horizontal" | "vertical";
  }) {
  return (
    <ToggleGroupPrimitive
      data-slot="toggle-group"
      data-variant={variant}
      data-size={size}
      data-spacing={spacing}
      data-orientation={orientation}
      className={cn(
        "flex w-fit items-center gap-1 data-vertical:flex-col data-vertical:items-stretch",
        className
      )}
      {...props}
    >
      <ToggleGroupContext.Provider
        value={{ variant, size, spacing, orientation }}
      >
        {children}
      </ToggleGroupContext.Provider>
    </ToggleGroupPrimitive>
  );
}

function ToggleGroupItem({
  className,
  children,
  variant = "default",
  size = "default",
  ...props
}: TogglePrimitive.Props & VariantProps<typeof toggleVariants>) {
  const context = React.useContext(ToggleGroupContext);

  return (
    <TogglePrimitive
      data-slot="toggle-group-item"
      data-variant={context.variant || variant}
      data-size={context.size || size}
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
    </TogglePrimitive>
  );
}

export { ToggleGroup, ToggleGroupItem };
```

---

### Toggle <a name="toggle"></a>

This crazy toggling button keeps people toggling...😃

- **Category**: UI Components
- **Registry Key**: `toggle`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/toggle.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Toggle.tsx`

```tsx
"use client";

import { Toggle as TogglePrimitive } from "@base-ui/react/toggle";
import { cva, type VariantProps } from "class-variance-authority";

import { cn } from "@/lib/utils";

const toggleVariants = cva(
  "inline-flex items-center justify-center text-sm font-medium ring-offset-background transition-colors hover:bg-muted hover:text-muted-foreground focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0 gap-2",
  {
    variants: {
      variant: {
        default:
          "bg-transparent hover:bg-muted/70 hover:text-muted-foreground aria-pressed:bg-muted",
        outlined:
          "border-2 border-input bg-transparent hover:bg-accent hover:text-accent-foreground/80 aria-pressed:bg-accent aria-pressed:text-accent-foreground",
        solid: "border-2 border-input bg-transparent hover:bg-secondary hover:text-secondary-foreground hover:border-secondary aria-pressed:bg-secondary aria-pressed:text-secondary-foreground aria-pressed:border-secondary",
        "outline-muted":
          "border-2 border-input bg-transparent hover:bg-muted/70 hover:text-muted-foreground aria-pressed:bg-muted",
      },
      size: {
        default: "h-10 px-3 min-w-10",
        sm: "h-9 px-2.5 min-w-9",
        lg: "h-11 px-5 min-w-11",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  }
);

function Toggle({
  className,
  variant = "default",
  size = "default",
  ...props
}: TogglePrimitive.Props & VariantProps<typeof toggleVariants>) {
  return (
    <TogglePrimitive
      data-slot="toggle"
      className={cn(toggleVariants({ variant, size, className }))}
      {...props}
    />
  );
}

export { Toggle, toggleVariants };
```

---

### Tooltip <a name="tooltip"></a>

A cool way to give your users a hint of what a component does...😉

- **Category**: UI Components
- **Registry Key**: `tooltip`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/tooltip.json
```

#### Dependencies

- **NPM Packages**:
  - `@base-ui/react`

#### Component Source Code

##### File: `components/retroui/Tooltip.tsx`

```tsx
"use client";

import * as React from "react";
import { Tooltip as BaseTooltip } from "@base-ui/react/tooltip";

import { cn } from "@/lib/utils";
import { cva, VariantProps } from "class-variance-authority";

const tooltipContentVariants = cva(
  "z-50 overflow-hidden border-2 border-border bg-background px-3 py-1.5 text-xs text-primary-foreground animate-in fade-in-0 zoom-in-95 data-[closed]:animate-out data-[closed]:fade-out-0 data-[closed]:zoom-out-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2",
  {
    variants: {
      variant: {
        default: "bg-background text-foreground",
        primary: "bg-primary text-primary-foreground",
        solid: "bg-black text-white",
      },
    },
    defaultVariants: {
      variant: "default",
    },
  },
);

const TooltipProvider = BaseTooltip.Provider;

const Tooltip = BaseTooltip.Root;

const TooltipTrigger = BaseTooltip.Trigger;

const TooltipContent = ({ className, variant, ref, ...props }: React.ComponentPropsWithRef<typeof BaseTooltip.Popup> & VariantProps<typeof tooltipContentVariants>) => (
  <BaseTooltip.Portal>
    <BaseTooltip.Popup
      ref={ref}
      className={cn(
        tooltipContentVariants({
          variant,
          className,
        }),
      )}
      {...props}
    />
  </BaseTooltip.Portal>
);

const TooltipObject = Object.assign(Tooltip, {
  Trigger: TooltipTrigger,
  Content: TooltipContent,
  Provider: TooltipProvider,
});

export { TooltipObject as Tooltip };
```

---

## Charts <a name="charts"></a>

### Area Chart <a name="area-chart"></a>

A customizable area chart component to visualize your data with gradient fills and smooth curves. 📈

- **Category**: Charts
- **Registry Key**: `area-chart`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/area-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`

#### Component Source Code

##### File: `components/retroui/charts/AreaChart.tsx`

```tsx
"use client"

import { cn } from "@/lib/utils"
import React from "react"
import {
  Area,
  AreaChart as RechartsAreaChart,
  CartesianGrid,
  ResponsiveContainer,
  Tooltip,
  XAxis,
  YAxis,
} from "recharts"

interface AreaChartProps extends React.HTMLAttributes<HTMLDivElement> {
  data: Record<string, any>[]
  index: string
  categories: string[]
  strokeColors?: string[]
  fillColors?: string[]
  tooltipBgColor?: string
  tooltipBorderColor?: string
  gridColor?: string
  valueFormatter?: (value: number) => string
  showGrid?: boolean
  showTooltip?: boolean
  fill?: "gradient" | "solid"
  className?: string
}

const AreaChart = React.forwardRef<HTMLDivElement, AreaChartProps>(
  (
    {
      data = [],
      index,
      categories = [],
      strokeColors = ["var(--foreground)"],
      fillColors = ["var(--primary)"],
      tooltipBgColor = "var(--background)",
      tooltipBorderColor = "var(--border)",
      gridColor = "var(--muted)",
      valueFormatter = (value: number) => value.toString(),
      showGrid = true,
      showTooltip = true,
      fill = "gradient",
      className,
      ...props
    },
    ref
  ) => {
    const chartId = React.useId()

    return (
      <div ref={ref} className={cn("h-80 w-full", className)} {...props}>
        <ResponsiveContainer width="100%" height="100%">
          <RechartsAreaChart data={data} margin={{ top: 10, right: 30, left: 0, bottom: 0 }}>
            <defs>
              {categories.map((category, index) => {
                const fillColor = fillColors[index] || fillColors[0]
                const gradientId = `gradient-${chartId}-${category}`
                return (
                  <linearGradient key={category} id={gradientId} x1="0" y1="0" x2="0" y2="1">
                    {fill === "gradient" ? (
                      <>
                        <stop offset="5%" stopColor={fillColor} stopOpacity={0.8} />
                        <stop offset="95%" stopColor={fillColor} stopOpacity={0} />
                      </>
                    ) : (
                      <stop stopColor={fillColor} stopOpacity={0.6} />
                    )}
                  </linearGradient>
                )
              })}
            </defs>
            
            {showGrid && (
              <CartesianGrid strokeDasharray="3 3" stroke={gridColor} />
            )}
            
            <XAxis 
              dataKey={index}
              axisLine={false}
              tickLine={false}
              className="text-xs fill-muted-foreground"
            />
            
            <YAxis
              axisLine={false}
              tickLine={false}
              className="text-xs fill-muted-foreground"
              tickFormatter={valueFormatter}
            />
            
            {showTooltip && (
              <Tooltip
                content={({ active, payload, label }) => {
                  if (!active || !payload?.length) return null
                  
                  return (
                    <div 
                      className="border p-2 shadow"
                      style={{ 
                        backgroundColor: tooltipBgColor,
                        borderColor: tooltipBorderColor 
                      }}
                    >
                      <div className="grid grid-cols-2 gap-2">
                        <div className="flex flex-col">
                          <span className="text-[0.70rem] uppercase text-muted-foreground">
                            {index}
                          </span>
                          <span className="font-bold text-muted-foreground">
                            {label}
                          </span>
                        </div>
                        {payload.map((entry, index) => (
                          <div key={index} className="flex flex-col">
                            <span className="text-[0.70rem] uppercase text-muted-foreground">
                              {entry.dataKey}
                            </span>
                            <span className="font-bold" style={{ color: strokeColors[0] }}>
                              {valueFormatter(entry.value as number)}
                            </span>
                          </div>
                        ))}
                      </div>
                    </div>
                  )
                }}
              />
            )}
            
            {categories.map((category, index) => {
              const strokeColor = strokeColors[index] || strokeColors[0]
              const gradientId = `gradient-${chartId}-${category}`
              
              return (
                <Area
                  key={category}
                  dataKey={category}
                  stroke={strokeColor}
                  fill={`url(#${gradientId})`}
                  strokeWidth={2}
                />
              )
            })}
          </RechartsAreaChart>
        </ResponsiveContainer>
      </div>
    )
  }
)

AreaChart.displayName = "AreaChart"

export { AreaChart, type AreaChartProps }
```

---

### Bar Chart <a name="bar-chart"></a>

A customizable bar chart component to visualize your data with support for multiple categories, custom colors, and horizontal alignment. 📊

- **Category**: Charts
- **Registry Key**: `bar-chart`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/bar-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`

#### Component Source Code

##### File: `components/retroui/charts/BarChart.tsx`

```tsx
"use client"

import { cn } from "@/lib/utils"
import React from "react"
import {
  Bar,
  BarChart as RechartsBarChart,
  CartesianGrid,
  ResponsiveContainer,
  Tooltip,
  XAxis,
  YAxis,
} from "recharts"

interface BarChartProps extends React.HTMLAttributes<HTMLDivElement> {
  data: Record<string, any>[]
  index: string
  categories: string[]
  strokeColors?: string[]
  fillColors?: string[]
  tooltipBgColor?: string
  tooltipBorderColor?: string
  gridColor?: string
  valueFormatter?: (value: number) => string
  showGrid?: boolean
  showTooltip?: boolean
  stacked?: boolean
  alignment?: "vertical" | "horizontal"
  className?: string
}

const BarChart = React.forwardRef<HTMLDivElement, BarChartProps>(
  (
    {
      data = [],
      index,
      categories = [],
      strokeColors = ["var(--foreground)"],
      fillColors = ["var(--primary)", "var(--secondary)"],
      tooltipBgColor = "var(--background)",
      tooltipBorderColor = "var(--border)",
      gridColor = "var(--muted)",
      valueFormatter = (value: number) => value.toString(),
      showGrid = true,
      showTooltip = true,
      stacked = false,
      alignment = "vertical",
      className,
      ...props
    },
    ref
  ) => {
    return (
      <div ref={ref} className={cn("h-80 w-full", className)} {...props}>
        <ResponsiveContainer width="100%" height="100%">
          <RechartsBarChart
            data={data}
            layout={alignment === "horizontal" ? "vertical" : undefined}
            margin={{ top: 10, right: 30, left: 0, bottom: 0 }}
          >
            {showGrid && (
              <CartesianGrid strokeDasharray="3 3" stroke={gridColor} />
            )}

            {alignment === "horizontal" ? (
              <>
                <XAxis
                  type="number"
                  axisLine={false}
                  tickLine={false}
                  className="text-xs fill-muted-foreground"
                  tickFormatter={valueFormatter}
                />

                <YAxis
                  type="category"
                  dataKey={index}
                  axisLine={false}
                  tickLine={false}
                  className="text-xs fill-muted-foreground"
                  width={80}
                />
              </>
            ) : (
              <>
                <XAxis
                  dataKey={index}
                  axisLine={false}
                  tickLine={false}
                  className="text-xs fill-muted-foreground"
                />

                <YAxis
                  axisLine={false}
                  tickLine={false}
                  className="text-xs fill-muted-foreground"
                  tickFormatter={valueFormatter}
                />
              </>
            )}

            {showTooltip && (
              <Tooltip
                content={({ active, payload, label }) => {
                  if (!active || !payload?.length) return null

                  return (
                    <div
                      className="border p-2 shadow"
                      style={{
                        backgroundColor: tooltipBgColor,
                        borderColor: tooltipBorderColor
                      }}
                    >
                      <div className="grid grid-cols-2 gap-2">
                        <div className="flex flex-col">
                          <span className="text-[0.70rem] uppercase text-muted-foreground">
                            {index}
                          </span>
                          <span className="font-bold text-muted-foreground">
                            {label}
                          </span>
                        </div>
                        {payload.map((entry, index) => (
                          <div key={index} className="flex flex-col">
                            <span className="text-[0.70rem] uppercase text-muted-foreground">
                              {entry.dataKey}
                            </span>
                            <span className="font-bold" style={{ color: strokeColors[0] }}>
                              {valueFormatter(entry.value as number)}
                            </span>
                          </div>
                        ))}
                      </div>
                    </div>
                  )
                }}
              />
            )}

            {categories.map((category, index) => {
              const fillColor = fillColors[index] || fillColors[0]
              const strokeColor = strokeColors[index] || strokeColors[0]

              return (
                <Bar
                  key={category}
                  dataKey={category}
                  fill={fillColor}
                  stroke={strokeColor}
                  strokeWidth={1}
                  stackId={stacked ? "strokeId" : undefined} />
              )
            })}
          </RechartsBarChart>
        </ResponsiveContainer>
      </div>
    )
  }
)

BarChart.displayName = "BarChart"

export { BarChart, type BarChartProps }
```

---

### Line Chart <a name="line-chart"></a>

A customizable line chart component to visualize trends and data over time with smooth curves and data points. 📉

- **Category**: Charts
- **Registry Key**: `line-chart`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/line-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`

#### Component Source Code

##### File: `components/retroui/charts/LineChart.tsx`

```tsx
"use client"

import { cn } from "@/lib/utils"
import React from "react"
import {
  CartesianGrid,
  Line,
  LineChart as RechartsLineChart,
  ResponsiveContainer,
  Tooltip,
  XAxis,
  YAxis,
} from "recharts"

interface LineChartProps extends React.HTMLAttributes<HTMLDivElement> {
  data: Record<string, any>[]
  index: string
  categories: string[]
  strokeColors?: string[]
  tooltipBgColor?: string
  tooltipBorderColor?: string
  gridColor?: string
  valueFormatter?: (value: number) => string
  showGrid?: boolean
  showTooltip?: boolean
  strokeWidth?: number
  dotSize?: number
  className?: string
}

const LineChart = React.forwardRef<HTMLDivElement, LineChartProps>(
  (
    {
      data = [],
      index,
      categories = [],
      strokeColors = ["var(--foreground)"],
      tooltipBgColor = "var(--background)",
      tooltipBorderColor = "var(--border)",
      gridColor = "var(--muted)",
      valueFormatter = (value: number) => value.toString(),
      showGrid = true,
      showTooltip = true,
      strokeWidth = 2,
      dotSize = 4,
      className,
      ...props
    },
    ref
  ) => {
    return (
      <div ref={ref} className={cn("h-80 w-full", className)} {...props}>
        <ResponsiveContainer width="100%" height="100%">
          <RechartsLineChart data={data} margin={{ top: 0, right: 30, left: 0, bottom: 0 }}>
            {showGrid && (
              <CartesianGrid strokeDasharray="3 3" stroke={gridColor} />
            )}
            
            <XAxis 
              dataKey={index}
              axisLine={false}
              tickLine={false}
              className="text-xs fill-muted-foreground"
            />
            
            <YAxis
              axisLine={false}
              tickLine={false}
              className="text-xs fill-muted-foreground"
              tickFormatter={valueFormatter}
            />
            
            {showTooltip && (
              <Tooltip
                content={({ active, payload, label }) => {
                  if (!active || !payload?.length) return null
                  
                  return (
                    <div 
                      className="border p-2 shadow"
                      style={{ 
                        backgroundColor: tooltipBgColor,
                        borderColor: tooltipBorderColor 
                      }}
                    >
                      <div className="grid grid-cols-2 gap-2">
                        <div className="flex flex-col">
                          <span className="text-[0.70rem] uppercase text-muted-foreground">
                            {index}
                          </span>
                          <span className="font-bold text-muted-foreground">
                            {label}
                          </span>
                        </div>
                        {payload.map((entry, index) => (
                          <div key={index} className="flex flex-col">
                            <span className="text-[0.70rem] uppercase text-muted-foreground">
                              {entry.dataKey}
                            </span>
                            <span className="font-bold" style={{ color: entry.color }}>
                              {valueFormatter(entry.value as number)}
                            </span>
                          </div>
                        ))}
                      </div>
                    </div>
                  )
                }}
              />
            )}
            
            {categories.map((category, index) => {
              const strokeColor = strokeColors[index] || strokeColors[0]
              
              return (
                <Line
                  key={category}
                  dataKey={category}
                  stroke={strokeColor}
                  strokeWidth={strokeWidth}
                  dot={{ r: dotSize, fill: strokeColor }}
                  activeDot={{ r: dotSize + 2, fill: strokeColor }}
                />
              )
            })}
          </RechartsLineChart>
        </ResponsiveContainer>
      </div>
    )
  }
)

LineChart.displayName = "LineChart"

export { LineChart, type LineChartProps }
```

---

### Pie Chart <a name="pie-chart"></a>

A customizable pie chart component to visualize proportional data with support for donut charts. 🍰

- **Category**: Charts
- **Registry Key**: `pie-chart`

#### Installation

```bash
npx shadcn@latest add https://retroui.dev/r/pie-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`

#### Component Source Code

##### File: `components/retroui/charts/PieChart.tsx`

```tsx
"use client"

import { cn } from "@/lib/utils"
import React from "react"
import {
  Cell,
  Pie,
  PieChart as RechartsPieChart,
  ResponsiveContainer,
  Tooltip,
} from "recharts"

interface PieChartProps extends React.HTMLAttributes<HTMLDivElement> {
  data: Record<string, any>[]
  dataKey: string
  nameKey: string
  colors?: string[]
  tooltipBgColor?: string
  tooltipBorderColor?: string
  valueFormatter?: (value: number) => string
  showTooltip?: boolean
  innerRadius?: number
  outerRadius?: number
  className?: string
}

const PieChart = React.forwardRef<HTMLDivElement, PieChartProps>(
  (
    {
      data = [],
      dataKey,
      nameKey,
      colors = ["var(--chart-1)", "var(--chart-2)", "var(--chart-3)", "var(--chart-4)", "var(--chart-5)"],
      tooltipBgColor = "var(--background)",
      tooltipBorderColor = "var(--border)",
      valueFormatter = (value: number) => value.toString(),
      showTooltip = true,
      innerRadius = 0,
      outerRadius = 100,
      className,
      ...props
    },
    ref
  ) => {
    return (
      <div ref={ref} className={cn("h-80 w-full", className)} {...props}>
        <ResponsiveContainer width="100%" height="100%">
          <RechartsPieChart>
            <Pie
              data={data}
              dataKey={dataKey}
              nameKey={nameKey}
              cx="50%"
              cy="50%"
              innerRadius={innerRadius}
              outerRadius={outerRadius}
              isAnimationActive={false}
              className="w-full h-full"
            >
              {data.map((entry, index) => (
                <Cell 
                  key={`cell-${index}`} 
                  fill={colors[index % colors.length]} 
                />
              ))}
            </Pie>
            
            {showTooltip && (
              <Tooltip
                content={({ active, payload }) => {
                  if (!active || !payload?.length) return null
                  
                  const data = payload[0]
                  
                  return (
                    <div 
                      className="border p-2 shadow"
                      style={{ 
                        backgroundColor: tooltipBgColor,
                        borderColor: tooltipBorderColor 
                      }}
                    >
                      <div className="flex flex-col gap-1">
                        <span className="text-[0.70rem] uppercase text-muted-foreground">
                          {data.name}
                        </span>
                        <span className="font-bold text-foreground">
                          {valueFormatter(data.value as number)}
                        </span>
                      </div>
                    </div>
                  )
                }}
              />
            )}
          </RechartsPieChart>
        </ResponsiveContainer>
      </div>
    )
  }
)

PieChart.displayName = "PieChart"

export { PieChart, type PieChartProps }
```

---
