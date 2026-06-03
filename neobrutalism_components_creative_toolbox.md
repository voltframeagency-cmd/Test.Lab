# Neobrutalism Components Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **Neobrutalism Components** (neobrutalism.dev) — a library of neobrutalist UI elements, forms, navigation components, and cards built on shadcn/ui with Tailwind CSS, Framer Motion, and TypeScript.

## Table of Contents

- [Accordion](#accordion)
- [Alert Dialog](#alert-dialog)
- [Alert](#alert)
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
- [Context Menu](#context-menu)
- [Data Table](#data-table)
- [Date Picker](#date-picker)
- [Dialog](#dialog)
- [Drawer](#drawer)
- [Dropdown Menu](#dropdown-menu)
- [Form](#form)
- [Hover Card](#hover-card)
- [Image Card](#image-card)
- [Input OTP](#input-otp)
- [Input](#input)
- [Label](#label)
- [Marquee](#marquee)
- [Menubar](#menubar)
- [Navigation Menu](#navigation-menu)
- [Pagination](#pagination)
- [Popover](#popover)
- [Progress](#progress)
- [Radio Group](#radio-group)
- [Resizable](#resizable)
- [Scroll Area](#scroll-area)
- [Select](#select)
- [Sheet](#sheet)
- [Sidebar](#sidebar)
- [Skeleton](#skeleton)
- [Slider](#slider)
- [Sonner](#sonner)
- [Switch](#switch)
- [Table](#table)
- [Tabs](#tabs)
- [Textarea](#textarea)
- [Tooltip](#tooltip)

---

### Accordion <a name="accordion"></a>

A vertically stacked set of interactive headings that each reveal a section of content.

- **Registry Key**: `accordion`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/accordion.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-accordion`

#### How to Use

##### Example 1

```tsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion"
```

##### Example 2

```tsx
<Accordion type="single" collapsible className="w-full max-w-xl">
  <AccordionItem value="item-1">
    <AccordionTrigger>Is it accessible?</AccordionTrigger>
    <AccordionContent>
      Yes. It adheres to the WAI-ARIA design pattern.
    </AccordionContent>
  </AccordionItem>
</Accordion>
```

#### Component Source Code

##### File: `src/components/ui/accordion.tsx`

```tsx
"use client"



import * as AccordionPrimitive from "@radix-ui/react-accordion"

import { ChevronDown } from "lucide-react"



import * as React from "react"



import { cn } from "@/lib/utils"



function Accordion({

  ...props

}: React.ComponentProps<typeof AccordionPrimitive.Root>) {

  return <AccordionPrimitive.Root data-slot="accordion" {...props} />

}



function AccordionItem({

  className,

  ...props

}: React.ComponentProps<typeof AccordionPrimitive.Item>) {

  return (

    <AccordionPrimitive.Item

      data-slot="accordion-item"

      className={cn(

        "rounded-base overflow-hidden border-2 border-b border-border shadow-shadow",

        className,

      )}

      {...props}

    />

  )

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

          "flex flex-1 items-center justify-between text-left text-base text-main-foreground border-border focus-visible:ring-[3px] bg-main p-4 font-heading transition-all [&[data-state=open]>svg]:rotate-180 data-[state=open]:rounded-b-none data-[state=open]:border-b-2 disabled:pointer-events-none disabled:opacity-50",

          className,

        )}

        {...props}

      >

        {children}

        <ChevronDown className="pointer-events-none size-5 shrink-0 transition-transform duration-200" />

      </AccordionPrimitive.Trigger>

    </AccordionPrimitive.Header>

  )

}



function AccordionContent({

  className,

  children,

  ...props

}: React.ComponentProps<typeof AccordionPrimitive.Content>) {

  return (

    <AccordionPrimitive.Content

      data-slot="accordion-content"

      className="overflow-hidden rounded-b-base bg-secondary-background text-sm font-base transition-all data-[state=closed]:animate-accordion-up data-[state=open]:animate-accordion-down"

      {...props}

    >

      <div className={cn("p-4", className)}>{children}</div>

    </AccordionPrimitive.Content>

  )

}



AccordionContent.displayName = AccordionPrimitive.Content.displayName



export { Accordion, AccordionItem, AccordionTrigger, AccordionContent }
```

---

### Alert Dialog <a name="alert-dialog"></a>

A modal dialog that interrupts the user with important content and expects a response.

- **Registry Key**: `alert-dialog`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/alert-dialog.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-alert-dialog`
- **Local Registry Dependencies**:
  - `https://neobrutalism.dev/r/nbutton.json`

#### How to Use

```tsx
<AlertDialog>
  <AlertDialogTrigger asChild>
    <Button>Open</Button>
  </AlertDialogTrigger>
  <AlertDialogContent>
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
```

#### Component Source Code

##### File: `src/components/ui/alert-dialog.tsx`

```tsx
"use client"



import * as AlertDialogPrimitive from "@radix-ui/react-alert-dialog"



import * as React from "react"



import { buttonVariants } from "@/components/ui/button"



import { cn } from "@/lib/utils"



function AlertDialog({

  ...props

}: React.ComponentProps<typeof AlertDialogPrimitive.Root>) {

  return <AlertDialogPrimitive.Root data-slot="alert-dialog" {...props} />

}



function AlertDialogTrigger({

  ...props

}: React.ComponentProps<typeof AlertDialogPrimitive.Trigger>) {

  return (

    <AlertDialogPrimitive.Trigger data-slot="alert-dialog-trigger" {...props} />

  )

}



function AlertDialogPortal({

  ...props

}: React.ComponentProps<typeof AlertDialogPrimitive.Portal>) {

  return (

    <AlertDialogPrimitive.Portal data-slot="alert-dialog-portal" {...props} />

  )

}



function AlertDialogOverlay({

  className,

  ...props

}: React.ComponentProps<typeof AlertDialogPrimitive.Overlay>) {

  return (

    <AlertDialogPrimitive.Overlay

      data-slot="alert-dialog-overlay"

      className={cn(

        "fixed inset-0 z-50 bg-overlay data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0",

        className,

      )}

      {...props}

    />

  )

}



function AlertDialogContent({

  className,

  ...props

}: React.ComponentProps<typeof AlertDialogPrimitive.Content>) {

  return (

    <AlertDialogPortal>

      <AlertDialogOverlay />

      <AlertDialogPrimitive.Content

        data-slot="alert-dialog-content"

        className={cn(

          "bg-background data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 fixed top-[50%] left-[50%] z-50 grid w-full max-w-[calc(100%-2rem)] translate-x-[-50%] translate-y-[-50%] gap-4 rounded-base border-2 border-border p-6 shadow-shadow duration-200 sm:max-w-lg",

          className,

        )}

        {...props}

      />

    </AlertDialogPortal>

  )

}



function AlertDialogHeader({

  className,

  ...props

}: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="alert-dialog-header"

      className={cn("flex flex-col gap-2 text-center sm:text-left", className)}

      {...props}

    />

  )

}



function AlertDialogFooter({

  className,

  ...props

}: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="alert-dialog-footer"

      className={cn(

        "flex flex-col-reverse gap-3 sm:flex-row sm:justify-end",

        className,

      )}

      {...props}

    />

  )

}



function AlertDialogTitle({

  className,

  ...props

}: React.ComponentProps<typeof AlertDialogPrimitive.Title>) {

  return (

    <AlertDialogPrimitive.Title

      data-slot="alert-dialog-title"

      className={cn("text-lg font-heading", className)}

      {...props}

    />

  )

}



function AlertDialogDescription({

  className,

  ...props

}: React.ComponentProps<typeof AlertDialogPrimitive.Description>) {

  return (

    <AlertDialogPrimitive.Description

      data-slot="alert-dialog-description"

      className={cn("text-sm font-base text-foreground", className)}

      {...props}

    />

  )

}



function AlertDialogAction({

  className,

  ...props

}: React.ComponentProps<typeof AlertDialogPrimitive.Action>) {

  return (

    <AlertDialogPrimitive.Action

      className={cn(buttonVariants(), className)}

      {...props}

    />

  )

}



function AlertDialogCancel({

  className,

  ...props

}: React.ComponentProps<typeof AlertDialogPrimitive.Cancel>) {

  return (

    <AlertDialogPrimitive.Cancel

      className={cn(buttonVariants({ variant: "neutral" }), className)}

      {...props}

    />

  )

}



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

### Alert <a name="alert"></a>

Displays a callout for user attention.

- **Registry Key**: `alert`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/alert.json
```

#### How to Use

```tsx
<Alert>
  <CheckCircle2Icon />
  <AlertTitle>Success! Your changes have been saved</AlertTitle>
  <AlertDescription>
    This is an alert with icon, title and description.
  </AlertDescription>
</Alert>
```

#### Component Source Code

##### File: `src/components/ui/alert.tsx`

```tsx
import { cva, type VariantProps } from "class-variance-authority"



import * as React from "react"



import { cn } from "@/lib/utils"



const alertVariants = cva(

  "relative w-full rounded-base border-2 border-border px-4 py-3 text-sm grid has-[>svg]:grid-cols-[calc(var(--spacing)*4)_1fr] grid-cols-[0_1fr] has-[>svg]:gap-x-3 gap-y-0.5 items-start [&>svg]:size-4 [&>svg]:translate-y-0.5 [&>svg]:text-current shadow-shadow",

  {

    variants: {

      variant: {

        default: "bg-main text-main-foreground",

        destructive: "bg-black text-white",

      },

    },

    defaultVariants: {

      variant: "default",

    },

  },

)



function Alert({

  className,

  variant,

  ...props

}: React.ComponentProps<"div"> & VariantProps<typeof alertVariants>) {

  return (

    <div

      data-slot="alert"

      role="alert"

      className={cn(alertVariants({ variant }), className)}

      {...props}

    />

  )

}



function AlertTitle({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="alert-title"

      className={cn(

        "col-start-2 line-clamp-1 min-h-4 font-heading tracking-tight",

        className,

      )}

      {...props}

    />

  )

}



function AlertDescription({

  className,

  ...props

}: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="alert-description"

      className={cn(

        "col-start-2 grid justify-items-start gap-1 text-sm font-base [&_p]:leading-relaxed",

        className,

      )}

      {...props}

    />

  )

}



export { Alert, AlertTitle, AlertDescription }
```

---

### Avatar <a name="avatar"></a>

An image element with a fallback for representing the user.

- **Registry Key**: `avatar`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/avatar.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-avatar`

#### How to Use

```tsx
<Avatar>
  <AvatarImage src="https://github.com/shadcn.png" alt="@shadcn" />
  <AvatarFallback>CN</AvatarFallback>
</Avatar>
```

#### Component Source Code

##### File: `src/components/ui/avatar.tsx`

```tsx
"use client"



import * as AvatarPrimitive from "@radix-ui/react-avatar"



import * as React from "react"



import { cn } from "@/lib/utils"



function Avatar({

  className,

  ...props

}: React.ComponentProps<typeof AvatarPrimitive.Root>) {

  return (

    <AvatarPrimitive.Root

      data-slot="avatar"

      className={cn(

        "relative flex size-10 shrink-0 overflow-hidden rounded-full outline-2 outline-border",

        className,

      )}

      {...props}

    />

  )

}



function AvatarImage({

  className,

  ...props

}: React.ComponentProps<typeof AvatarPrimitive.Image>) {

  return (

    <AvatarPrimitive.Image

      data-slot="avatar-image"

      className={cn("aspect-square size-full", className)}

      {...props}

    />

  )

}



function AvatarFallback({

  className,

  ...props

}: React.ComponentProps<typeof AvatarPrimitive.Fallback>) {

  return (

    <AvatarPrimitive.Fallback

      data-slot="avatar-fallback"

      className={cn(

        "flex size-full items-center justify-center rounded-full bg-secondary-background text-foreground font-base",

        className,

      )}

      {...props}

    />

  )

}



export { Avatar, AvatarImage, AvatarFallback }
```

---

### Badge <a name="badge"></a>

Displays a badge or a component that looks like a badge.

- **Registry Key**: `badge`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/badge.json
```

#### How to Use

```tsx
<Badge>Badge</Badge>
```

#### Component Source Code

##### File: `src/components/ui/badge.tsx`

```tsx
import { Slot } from "@radix-ui/react-slot"

import { cva, type VariantProps } from "class-variance-authority"



import * as React from "react"



import { cn } from "@/lib/utils"



const badgeVariants = cva(

  "inline-flex items-center justify-center rounded-base border-2 border-border px-2.5 py-0.5 text-xs font-base w-fit whitespace-nowrap shrink-0 [&>svg]:size-3 gap-1 [&>svg]:pointer-events-none focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px] overflow-hidden",

  {

    variants: {

      variant: {

        default: "bg-main text-main-foreground",

        neutral: "bg-secondary-background text-foreground",

      },

    },

    defaultVariants: {

      variant: "default",

    },

  },

)



function Badge({

  className,

  variant,

  asChild = false,

  ...props

}: React.ComponentProps<"span"> &

  VariantProps<typeof badgeVariants> & {

    asChild?: boolean

  }) {

  const Comp = asChild ? Slot : "span"



  return (

    <Comp

      data-slot="badge"

      className={cn(badgeVariants({ variant }), className)}

      {...props}

    />

  )

}



export { Badge, badgeVariants }
```

---

### Breadcrumb <a name="breadcrumb"></a>

Displays the current page location within a hierarchical structure.

- **Registry Key**: `breadcrumb`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/breadcrumb.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-slot`

#### How to Use

```tsx
<Breadcrumb>
  <BreadcrumbList>
    <BreadcrumbItem>
      <BreadcrumbLink href="/">Home</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <DropdownMenu>
        <DropdownMenuTrigger className="flex items-center gap-1">
          <BreadcrumbEllipsis className="size-4" />
          <span className="sr-only">Toggle menu</span>
        </DropdownMenuTrigger>
        <DropdownMenuContent align="start">
          <DropdownMenuItem>Documentation</DropdownMenuItem>
          <DropdownMenuItem>Themes</DropdownMenuItem>
          <DropdownMenuItem>GitHub</DropdownMenuItem>
        </DropdownMenuContent>
      </DropdownMenu>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbLink href="/components">Components</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbPage>Breadcrumb</BreadcrumbPage>
    </BreadcrumbItem>
  </BreadcrumbList>
</Breadcrumb>
```

#### Component Source Code

##### File: `src/components/ui/breadcrumb.tsx`

```tsx
import { Slot } from "@radix-ui/react-slot"

import { ChevronRight, MoreHorizontal } from "lucide-react"



import * as React from "react"



import { cn } from "@/lib/utils"



function Breadcrumb({ ...props }: React.ComponentProps<"nav">) {

  return <nav data-slot="breadcrumb" aria-label="breadcrumb" {...props} />

}



function BreadcrumbList({ className, ...props }: React.ComponentProps<"ol">) {

  return (

    <ol

      data-slot="breadcrumb-list"

      className={cn(

        "flex flex-wrap items-center gap-1.5 text-sm font-base break-words text-foreground sm:gap-2.5",

        className,

      )}

      {...props}

    />

  )

}



function BreadcrumbItem({ className, ...props }: React.ComponentProps<"li">) {

  return (

    <li

      data-slot="breadcrumb-item"

      className={cn("inline-flex items-center gap-1.5", className)}

      {...props}

    />

  )

}



function BreadcrumbLink({

  asChild,

  className,

  ...props

}: React.ComponentProps<"a"> & {

  asChild?: boolean

}) {

  const Comp = asChild ? Slot : "a"



  return (

    <Comp data-slot="breadcrumb-link" className={cn(className)} {...props} />

  )

}



function BreadcrumbPage({ className, ...props }: React.ComponentProps<"span">) {

  return (

    <span

      data-slot="breadcrumb-page"

      role="link"

      aria-disabled="true"

      aria-current="page"

      className={cn(className)}

      {...props}

    />

  )

}



function BreadcrumbSeparator({

  children,

  className,

  ...props

}: React.ComponentProps<"li">) {

  return (

    <li

      data-slot="breadcrumb-separator"

      role="presentation"

      aria-hidden="true"

      className={cn("[&>svg]:size-3.5", className)}

      {...props}

    >

      {children ?? <ChevronRight />}

    </li>

  )

}



function BreadcrumbEllipsis({

  className,

  ...props

}: React.ComponentProps<"span">) {

  return (

    <span

      data-slot="breadcrumb-ellipsis"

      role="presentation"

      aria-hidden="true"

      className={cn("flex size-9 items-center justify-center", className)}

      {...props}

    >

      <MoreHorizontal className="size-4" />

      <span className="sr-only">More</span>

    </span>

  )

}



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

Displays a button or a component that looks like a button.

- **Registry Key**: `button`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/button.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-slot`

#### How to Use

```tsx
<Button>Default</Button>
```

#### Component Source Code

##### File: `src/components/ui/button.tsx`

```tsx
import { Slot } from "@radix-ui/react-slot"

import { cva, type VariantProps } from "class-variance-authority"



import * as React from "react"



import { cn } from "@/lib/utils"



const buttonVariants = cva(

  "inline-flex items-center justify-center whitespace-nowrap rounded-base text-sm font-base ring-offset-white transition-all gap-2 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0 focus-visible:outline-hidden focus-visible:ring-2 focus-visible:ring-black focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50",

  {

    variants: {

      variant: {

        default:

          "text-main-foreground bg-main border-2 border-border shadow-shadow hover:translate-x-boxShadowX hover:translate-y-boxShadowY hover:shadow-none",

        noShadow: "text-main-foreground bg-main border-2 border-border",

        neutral:

          "bg-secondary-background text-foreground border-2 border-border shadow-shadow hover:translate-x-boxShadowX hover:translate-y-boxShadowY hover:shadow-none",

        reverse:

          "text-main-foreground bg-main border-2 border-border hover:translate-x-reverseBoxShadowX hover:translate-y-reverseBoxShadowY hover:shadow-shadow",

      },

      size: {

        default: "h-10 px-4 py-2",

        sm: "h-9 px-3",

        lg: "h-11 px-8",

        icon: "size-10",

      },

    },

    defaultVariants: {

      variant: "default",

      size: "default",

    },

  },

)



function Button({

  className,

  variant,

  size,

  asChild = false,

  ...props

}: React.ComponentProps<"button"> &

  VariantProps<typeof buttonVariants> & {

    asChild?: boolean

  }) {

  const Comp = asChild ? Slot : "button"



  return (

    <Comp

      data-slot="button"

      className={cn(buttonVariants({ variant, size, className }))}

      {...props}

    />

  )

}



export { Button, buttonVariants }
```

---

### Calendar <a name="calendar"></a>

A date field component that allows users to enter and edit date.

- **Registry Key**: `calendar`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/calendar.json
```

#### Dependencies

- **NPM Packages**:
  - `react-day-picker@8.10.1`
  - `date-fns`
- **Local Registry Dependencies**:
  - `https://neobrutalism.dev/r/nbutton.json`

#### How to Use

##### Example 1

```tsx
const [date, setDate] = useState<Date | undefined>(new Date())
```

##### Example 2

```tsx
<Calendar mode="single" selected={date} onSelect={setDate} />
```

#### Component Source Code

##### File: `src/components/ui/calendar.tsx`

```tsx
"use client"



import { ChevronLeft, ChevronRight } from "lucide-react"

import { DayPicker } from "react-day-picker"



import * as React from "react"



import { buttonVariants } from "@/components/ui/button"



import { cn } from "@/lib/utils"



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

        "rounded-base! border-2 border-border bg-main p-3 font-heading shadow-shadow",

        className,

      )}

      classNames={{

        months: "flex flex-col sm:flex-row gap-2",

        month: "flex flex-col gap-4",

        caption:

          "flex justify-center pt-1 relative items-center w-full text-main-foreground",

        caption_label: "text-sm font-heading",

        nav: "gap-1 flex items-center",

        nav_button: cn(

          buttonVariants({ variant: "noShadow" }),

          "size-7 bg-transparent p-0",

        ),

        nav_button_previous: "absolute left-1",

        nav_button_next: "absolute right-1",

        table: "w-full border-collapse space-y-1",

        head_row: "flex",

        head_cell:

          "text-main-foreground rounded-base w-9 font-base text-[0.8rem]",

        row: "flex w-full mt-2",

        cell: cn(

          "relative p-0 text-center text-sm focus-within:relative focus-within:z-20 [&:has([aria-selected])]:bg-black/50 [&:has([aria-selected])]:text-white! [&:has([aria-selected].day-range-end)]:rounded-r-base",

          props.mode === "range"

            ? "[&:has(>.day-range-end)]:rounded-r-base [&:has(>.day-range-start)]:rounded-l-base [&:has([aria-selected])]:bg-black/50! first:[&:has([aria-selected])]:rounded-l-base last:[&:has([aria-selected])]:rounded-r-base"

            : "[&:has([aria-selected])]:rounded-base [&:has([aria-selected])]:bg-black/50",

        ),

        day: cn(

          buttonVariants({ variant: "noShadow" }),

          "size-9 p-0 font-base aria-selected:opacity-100",

        ),

        day_range_start:

          "day-range-start aria-selected:bg-black! aria-selected:text-white rounded-base",

        day_range_end:

          "day-range-end aria-selected:bg-black! aria-selected:text-white rounded-base",

        day_selected: "bg-black! text-white! rounded-base",

        day_today: "bg-secondary-background text-foreground!",

        day_outside:

          "day-outside text-main-foreground opacity-50 aria-selected:bg-none",

        day_disabled: "text-main-foreground opacity-50 rounded-base",

        day_range_middle: "aria-selected:bg-black/50! aria-selected:text-white",

        day_hidden: "invisible",

        ...classNames,

      }}

      components={{

        IconLeft: ({ className, ...props }) => (

          <ChevronLeft className={cn("size-4", className)} {...props} />

        ),

        IconRight: ({ className, ...props }) => (

          <ChevronRight className={cn("size-4", className)} {...props} />

        ),

      }}

      {...props}

    />

  )

}

Calendar.displayName = "Calendar"



export { Calendar }
```

---

### Card <a name="card"></a>

Displays a card with header, content, and footer.

- **Registry Key**: `card`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/card.json
```

#### How to Use

```tsx
<Card className="w-full max-w-sm">
  <CardHeader>
    <CardTitle>Login to your account</CardTitle>
    <CardDescription>
      Enter your email below to login to your account
    </CardDescription>
  </CardHeader>
  <CardContent>
    <form>
      <div className="flex flex-col gap-6">
        <div className="grid gap-2">
          <Label htmlFor="email">Email</Label>
          <Input
            id="email"
            type="email"
            placeholder="m@example.com"
            required
          />
        </div>
        <div className="grid gap-2">
          <div className="flex items-center">
            <Label htmlFor="password">Password</Label>
            <a
              href="#"
              className="ml-auto inline-block text-sm underline-offset-4 hover:underline"
            >
              Forgot your password?
            </a>
          </div>
          <Input id="password" type="password" required />
        </div>
      </div>
    </form>
  </CardContent>
  <CardFooter className="flex-col gap-2">
    <Button type="submit" className="w-full">
      Login
    </Button>
    <Button variant="neutral" className="w-full">
      Login with Google
    </Button>
    <div className="mt-4 text-center text-sm">
      Don&apos;t have an account?{" "}
      <a href="#" className="underline underline-offset-4">
        Sign up
      </a>
    </div>
  </CardFooter>
</Card>
```

#### Component Source Code

##### File: `src/components/ui/card.tsx`

```tsx
import * as React from "react"



import { cn } from "@/lib/utils"



function Card({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="card"

      className={cn(

        "rounded-base flex flex-col shadow-shadow border-2 gap-6 py-6 border-border bg-background text-foreground font-base",

        className,

      )}

      {...props}

    />

  )

}



function CardHeader({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="card-header"

      className={cn(

        "@container/card-header grid auto-rows-min grid-rows-[auto_auto] items-start gap-1.5 px-6 has-[data-slot=card-action]:grid-cols-[1fr_auto] [.border-b]:pb-6",

        className,

      )}

      {...props}

    />

  )

}



function CardTitle({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="card-title"

      className={cn("font-heading leading-none", className)}

      {...props}

    />

  )

}



function CardDescription({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="card-description"

      className={cn("text-sm font-base", className)}

      {...props}

    />

  )

}



function CardAction({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="card-action"

      className={cn(

        "col-start-2 row-span-2 row-start-1 self-start justify-self-end",

        className,

      )}

      {...props}

    />

  )

}



function CardContent({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="card-content"

      className={cn("px-6", className)}

      {...props}

    />

  )

}



function CardFooter({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="card-footer"

      className={cn("flex items-center px-6 [.border-t]:pt-6", className)}

      {...props}

    />

  )

}



export {

  Card,

  CardHeader,

  CardFooter,

  CardTitle,

  CardDescription,

  CardContent,

  CardAction,

}
```

---

### Carousel <a name="carousel"></a>

A carousel with motion and swipe built using Embla.

- **Registry Key**: `carousel`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/carousel.json
```

#### Dependencies

- **NPM Packages**:
  - `embla-carousel-react`
- **Local Registry Dependencies**:
  - `https://neobrutalism.dev/r/nbutton.json`

#### How to Use

```tsx
<div className="w-full flex-col items-center gap-4 flex">
  <Carousel className="w-full max-w-[200px]">
    <CarouselContent>
      {Array.from({ length: 5 }).map((_, index) => (
        <CarouselItem key={index}>
          <div className="p-[10px]">
            <Card className="shadow-none p-0 bg-main text-main-foreground">
              <CardContent className="flex aspect-square items-center justify-center p-4">
                <span className="text-3xl font-base">{index + 1}</span>
              </CardContent>
            </Card>
          </div>
        </CarouselItem>
      ))}
    </CarouselContent>
    <CarouselPrevious />
    <CarouselNext />
  </Carousel>
</div>
```

#### Component Source Code

##### File: `src/components/ui/carousel.tsx`

```tsx
"use client"



import useEmblaCarousel, {

  type UseEmblaCarouselType,

} from "embla-carousel-react"

import { ArrowLeft, ArrowRight } from "lucide-react"



import * as React from "react"



import { Button } from "@/components/ui/button"



import { cn } from "@/lib/utils"



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

  plugins?: CarouselPlugin

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

    plugins,

  )

  const [canScrollPrev, setCanScrollPrev] = React.useState(false)

  const [canScrollNext, setCanScrollNext] = React.useState(false)



  const onSelect = React.useCallback((api: CarouselApi) => {

    if (!api) {

      return

    }



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

    [scrollPrev, scrollNext],

  )



  React.useEffect(() => {

    if (!api || !setApi) {

      return

    }



    setApi(api)

  }, [api, setApi])



  React.useEffect(() => {

    if (!api) {

      return

    }



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

          className,

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

      data-slot="carousel-item"

      role="group"

      aria-roledescription="slide"

      className={cn(

        "min-w-0 shrink-0 grow-0 basis-full",

        orientation === "horizontal" ? "pl-4" : "pt-4",

        className,

      )}

      {...props}

    />

  )

}



function CarouselPrevious({

  className,

  variant = "noShadow",

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

        "absolute size-8 rounded-base",

        orientation === "horizontal"

          ? "top-1/2 -left-12 -translate-y-1/2"

          : "-top-12 left-1/2 -translate-x-1/2 rotate-90",

        className,

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

  variant = "noShadow",

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

        "absolute h-8 w-8 rounded-base",

        orientation === "horizontal"

          ? "-right-12 top-1/2 -translate-y-1/2"

          : "-bottom-12 left-1/2 -translate-x-1/2 rotate-90",

        className,

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



export {

  type CarouselApi,

  Carousel,

  CarouselContent,

  CarouselItem,

  CarouselPrevious,

  CarouselNext,

}
```

---

### Chart <a name="chart"></a>

Beautiful charts. Built using Recharts. Copy and paste into your apps.

- **Registry Key**: `chart`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `https://neobrutalism.dev/r/ncard.json`

#### Component Source Code

##### File: `src/components/ui/chart.tsx`

```tsx
"use client"



import * as RechartsPrimitive from "recharts"



import * as React from "react"



import { cn } from "@/lib/utils"



// Format: { THEME_NAME: CSS_SELECTOR }

const THEMES = { light: "", dark: ".dark" } as const



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

    throw new Error("useChart must be used within a <ChartContainer />")

  }



  return context

}



function ChartContainer({

  id,

  className,

  children,

  config,

  ...props

}: React.ComponentProps<"div"> & {

  config: ChartConfig

  children: React.ComponentProps<

    typeof RechartsPrimitive.ResponsiveContainer

  >["children"]

}) {

  const uniqueId = React.useId()

  const chartId = `chart-${id || uniqueId.replace(/:/g, "")}`



  return (

    <ChartContext.Provider value={{ config }}>

      <div

        data-slot="chart"

        data-chart={chartId}

        className={cn(

          "[&_.recharts-cartesian-axis-tick_text]:fill-foreground [&_.recharts-cartesian-grid_line[stroke='#ccc']]:stroke-[#80808080] [&_.recharts-curve.recharts-tooltip-cursor]:stroke-[#80808080] [&_.recharts-polar-grid_[stroke='#ccc']]:stroke-black [&_.recharts-polar-grid_[stroke='#ccc']]:dark:stroke-white [&_.recharts-reference-line_[stroke='#ccc']]:stroke-black [&_.recharts-reference-line_[stroke='#ccc']]:dark:stroke-white flex aspect-video justify-center text-xs [&_.recharts-dot[stroke='#fff']]:stroke-transparent [&_.recharts-sector]:outline-hidden [&_.recharts-sector[stroke='#fff']]:stroke-border [&_.recharts-surface]:outline-hidden",

          "[&_.recharts-layer_path]:[fill-opacity:1] [&_.recharts-layer_path]:[stroke-width:2] [&_.recharts-layer_path]:[stroke:var(--color-border)]",

          className,

        )}

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

    ([, config]) => config.theme || config.color,

  )



  if (!colorConfig.length) {

    return null

  }



  return (

    <style

      dangerouslySetInnerHTML={{

        __html: Object.entries(THEMES)

          .map(

            ([theme, prefix]) => `

${prefix} [data-chart=${id}] {

${colorConfig

  .map(([key, itemConfig]) => {

    const color =

      itemConfig.theme?.[theme as keyof typeof itemConfig.theme] ||

      itemConfig.color

    return color ? `  --color-${key}: ${color};` : null

  })

  .join("\n")}

}

`,

          )

          .join("\n"),

      }}

    />

  )

}



const ChartTooltip = RechartsPrimitive.Tooltip



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

  color,

  nameKey,

  labelKey,

}: React.ComponentProps<typeof RechartsPrimitive.Tooltip> &

  React.ComponentProps<"div"> & {

    hideLabel?: boolean

    hideIndicator?: boolean

    indicator?: "line" | "dot" | "dashed"

    nameKey?: string

    labelKey?: string

  }) {

  const { config } = useChart()



  const tooltipLabel = React.useMemo(() => {

    if (hideLabel || !payload?.length) {

      return null

    }



    const [item] = payload

    const key = `${labelKey || item?.dataKey || item?.name || "value"}`

    const itemConfig = getPayloadConfigFromPayload(config, item, key)

    const value =

      !labelKey && typeof label === "string"

        ? config[label as keyof typeof config]?.label || label

        : itemConfig?.label



    if (labelFormatter) {

      return (

        <div className={cn("font-heading", labelClassName)}>

          {labelFormatter(value, payload)}

        </div>

      )

    }



    if (!value) {

      return null

    }



    return <div className={cn("font-base", labelClassName)}>{value}</div>

  }, [

    label,

    labelFormatter,

    payload,

    hideLabel,

    labelClassName,

    config,

    labelKey,

  ])



  if (!active || !payload?.length) {

    return null

  }



  const nestLabel = payload.length === 1 && indicator !== "dot"



  return (

    <div

      className={cn(

        "border-border bg-background grid min-w-[8rem] items-start gap-1.5 rounded-lg border px-2.5 py-1.5 text-xs shadow-xl",

        className,

      )}

    >

      {!nestLabel ? tooltipLabel : null}

      <div className="grid gap-1.5">

        {payload.map((item, index) => {

          const key = `${nameKey || item.name || item.dataKey || "value"}`

          const itemConfig = getPayloadConfigFromPayload(config, item, key)

          const indicatorColor = color || item.payload.fill || item.color



          return (

            <div

              key={item.dataKey}

              className={cn(

                "[&>svg]:text-muted-foreground flex w-full flex-wrap items-stretch gap-2 [&>svg]:h-2.5 [&>svg]:w-2.5 ",

                indicator === "dot" && "items-center",

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

                        className={cn(

                          "shrink-0 rounded-[2px] bg-(--color-bg)",

                          {

                            "size-2.5 border border-border":

                              indicator === "dot",

                            "w-1": indicator === "line",

                            "w-0 border-[1.5px] border-dashed bg-transparent":

                              indicator === "dashed",

                            "my-0.5": nestLabel && indicator === "dashed",

                          },

                        )}

                        style={

                          {

                            "--color-bg": indicatorColor,

                            "--color-border": indicatorColor,

                          } as React.CSSProperties

                        }

                      />

                    )

                  )}

                  <div

                    className={cn(

                      "flex flex-1 justify-between leading-none",

                      nestLabel ? "items-end" : "items-center",

                    )}

                  >

                    <div className="grid gap-1.5">

                      {nestLabel ? tooltipLabel : null}

                      <span className="text-muted-foreground">

                        {itemConfig?.label || item.name}

                      </span>

                    </div>

                    {item.value && (

                      <span className="text-foreground font-mono font-medium tabular-nums">

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



function ChartLegendContent({

  className,

  hideIcon = false,

  payload,

  verticalAlign = "bottom",

  nameKey,

}: React.ComponentProps<"div"> &

  Pick<RechartsPrimitive.LegendProps, "payload" | "verticalAlign"> & {

    hideIcon?: boolean

    nameKey?: string

  }) {

  const { config } = useChart()



  if (!payload?.length) {

    return null

  }



  return (

    <div

      className={cn(

        "flex items-center justify-center gap-4",

        verticalAlign === "top" ? "pb-3" : "pt-3",

        className,

      )}

    >

      {payload.map((item) => {

        const key = `${nameKey || item.dataKey || "value"}`

        const itemConfig = getPayloadConfigFromPayload(config, item, key)



        return (

          <div

            key={item.value}

            className={cn(

              "[&>svg]:text-foreground flex items-center gap-1.5 [&>svg]:h-3 [&>svg]:w-3",

            )}

          >

            {itemConfig?.icon && !hideIcon ? (

              <itemConfig.icon />

            ) : (

              <div

                className="h-2 w-2 border border-border shrink-0 rounded-[2px]"

                style={{

                  backgroundColor: item.color,

                }}

              />

            )}

            {itemConfig?.label}

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

  key: string,

) {

  if (typeof payload !== "object" || payload === null) {

    return undefined

  }



  const payloadPayload =

    "payload" in payload &&

    typeof payload.payload === "object" &&

    payload.payload !== null

      ? payload.payload

      : undefined



  let configLabelKey: string = key



  if (

    key in payload &&

    typeof payload[key as keyof typeof payload] === "string"

  ) {

    configLabelKey = payload[key as keyof typeof payload] as string

  } else if (

    payloadPayload &&

    key in payloadPayload &&

    typeof payloadPayload[key as keyof typeof payloadPayload] === "string"

  ) {

    configLabelKey = payloadPayload[

      key as keyof typeof payloadPayload

    ] as string

  }



  return configLabelKey in config

    ? config[configLabelKey]

    : config[key as keyof typeof config]

}



export {

  ChartContainer,

  ChartTooltip,

  ChartTooltipContent,

  ChartLegend,

  ChartLegendContent,

  ChartStyle,

}
```

---

### Checkbox <a name="checkbox"></a>

A control that allows the user to toggle between checked and not checked.

- **Registry Key**: `checkbox`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/checkbox.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-checkbox`

#### How to Use

```tsx
<Checkbox />
```

#### Component Source Code

##### File: `src/components/ui/checkbox.tsx`

```tsx
"use client"



import * as CheckboxPrimitive from "@radix-ui/react-checkbox"

import { Check } from "lucide-react"



import * as React from "react"



import { cn } from "@/lib/utils"



function Checkbox({

  className,

  ...props

}: React.ComponentProps<typeof CheckboxPrimitive.Root>) {

  return (

    <CheckboxPrimitive.Root

      data-slot="checkbox"

      className={cn(

        "peer size-4 shrink-0 outline-2 outline-border ring-offset-white focus-visible:outline-hidden focus-visible:ring-2 focus-visible:ring-black focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 data-[state=checked]:bg-main data-[state=checked]:text-white",

        className,

      )}

      {...props}

    >

      <CheckboxPrimitive.Indicator

        data-slot="checkbox-indicator"

        className={cn("flex items-center justify-center text-current")}

      >

        <Check className="size-4 text-main-foreground" />

      </CheckboxPrimitive.Indicator>

    </CheckboxPrimitive.Root>

  )

}



export { Checkbox }
```

---

### Collapsible <a name="collapsible"></a>

An interactive component that expands/collapses a panel.

- **Registry Key**: `collapsible`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/collapsible.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-collapsible`

#### How to Use

##### Example 1

```tsx
const [isOpen, setIsOpen] = React.useState(false)
```

##### Example 2

```tsx
<Collapsible
  open={isOpen}
  onOpenChange={setIsOpen}
  className="w-[350px] space-y-2"
>
  <div className="rounded-base flex items-center justify-between space-x-4 border-2 border-border text-main-foreground bg-main px-4 py-2">
    <h4 className="text-sm font-heading">
      @peduarte starred 3 repositories
    </h4>
    <CollapsibleTrigger asChild>
      <Button
        variant="noShadow"
        size="sm"
        className="w-9 bg-secondary-background text-foreground p-0"
      >
        <ChevronsUpDown className="size-4" />
        <span className="sr-only">Toggle</span>
      </Button>
    </CollapsibleTrigger>
  </div>
  <div className="rounded-base border-2 border-border bg-main px-4 py-3 font-mono font-base text-main-foreground text-sm">
    @radix-ui/primitives
  </div>
  <CollapsibleContent className="space-y-2 text-main-foreground font-base">
    <div className="rounded-base border-2 border-border bg-main px-4 py-3 font-mono text-sm">
      @radix-ui/colors
    </div>
    <div className="rounded-base border-2 border-border bg-main px-4 py-3 font-mono text-sm">
      @stitches/react
    </div>
  </CollapsibleContent>
</Collapsible>
```

#### Component Source Code

##### File: `src/components/ui/collapsible.tsx`

```tsx
"use client"



import * as CollapsiblePrimitive from "@radix-ui/react-collapsible"



import * as React from "react"



function Collapsible({

  ...props

}: React.ComponentProps<typeof CollapsiblePrimitive.Root>) {

  return <CollapsiblePrimitive.Root data-slot="collapsible" {...props} />

}



function CollapsibleTrigger({

  ...props

}: React.ComponentProps<typeof CollapsiblePrimitive.CollapsibleTrigger>) {

  return (

    <CollapsiblePrimitive.CollapsibleTrigger

      data-slot="collapsible-trigger"

      {...props}

    />

  )

}



function CollapsibleContent({

  ...props

}: React.ComponentProps<typeof CollapsiblePrimitive.CollapsibleContent>) {

  return (

    <CollapsiblePrimitive.CollapsibleContent

      data-slot="collapsible-content"

      {...props}

    />

  )

}



export { Collapsible, CollapsibleTrigger, CollapsibleContent }
```

---

### Combobox <a name="combobox"></a>

Autocomplete input and command palette with a list of suggestions.

- **Registry Key**: `combobox`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/combobox.json
```

---

### Command <a name="command"></a>

Fast, composable, unstyled command menu for React.

- **Registry Key**: `command`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/command.json
```

#### Dependencies

- **NPM Packages**:
  - `cmdk`
- **Local Registry Dependencies**:
  - `https://neobrutalism.dev/r/ndialog.json`

#### How to Use

##### Example 1

```tsx
const [open, setOpen] = React.useState(false)

React.useEffect(() => {
  const down = (e: KeyboardEvent) => {
    if (e.key === "j" && (e.metaKey || e.ctrlKey)) {
      e.preventDefault()
      setOpen((open) => !open)
    }
  }

  document.addEventListener("keydown", down)
  return () => document.removeEventListener("keydown", down)
}, [])
```

##### Example 2

```tsx
<>
  <p className="text-foreground text-sm">
    Press{" "}
    <kbd className="bg-main text-main-foreground pointer-events-none inline-flex h-5 items-center gap-1 rounded-base border-2 px-1.5 font-mono text-[10px] font-heading select-none">
      <span className="text-xs">⌘</span>J
    </kbd>
  </p>
  <CommandDialog open={open} onOpenChange={setOpen}>
    <CommandInput placeholder="Type a command or search..." />
    <CommandList>
      <CommandEmpty>No results found.</CommandEmpty>
      <CommandGroup heading="Suggestions">
        <CommandItem>
          <Calendar />
          <span>Calendar</span>
        </CommandItem>
        <CommandItem>
          <Smile />
          <span>Search Emoji</span>
        </CommandItem>
        <CommandItem>
          <Calculator />
          <span>Calculator</span>
        </CommandItem>
      </CommandGroup>
      <CommandSeparator />
      <CommandGroup heading="Settings">
        <CommandItem>
          <User />
          <span>Profile</span>
          <CommandShortcut>⌘P</CommandShortcut>
        </CommandItem>
        <CommandItem>
          <CreditCard />
          <span>Billing</span>
          <CommandShortcut>⌘B</CommandShortcut>
        </CommandItem>
        <CommandItem>
          <Settings />
          <span>Settings</span>
          <CommandShortcut>⌘S</CommandShortcut>
        </CommandItem>
      </CommandGroup>
    </CommandList>
  </CommandDialog>
</>
```

#### Component Source Code

##### File: `src/components/ui/command.tsx`

```tsx
"use client"



import { Command as CommandPrimitive } from "cmdk"

import { Search } from "lucide-react"



import * as React from "react"



import {

  Dialog,

  DialogContent,

  DialogDescription,

  DialogHeader,

  DialogTitle,

} from "@/components/ui/dialog"



import { cn } from "@/lib/utils"



function Command({

  className,

  ...props

}: React.ComponentProps<typeof CommandPrimitive>) {

  return (

    <CommandPrimitive

      data-slot="command"

      className={cn(

        "flex h-full w-full flex-col overflow-hidden rounded-[0px] border-2 border-border bg-main font-base text-main-foreground",

        className,

      )}

      {...props}

    />

  )

}



function CommandDialog({

  title = "Command Palette",

  description = "Search for a command to run...",

  children,

  ...props

}: React.ComponentProps<typeof Dialog> & {

  title?: string

  description?: string

}) {

  return (

    <Dialog {...props}>

      <DialogHeader className="sr-only">

        <DialogTitle>{title}</DialogTitle>

        <DialogDescription>{description}</DialogDescription>

      </DialogHeader>

      <DialogContent className="overflow-hidden p-0 rounded-[0px]! shadow-shadow border-0">

        <Command className="**:data-[slot=command-input-wrapper]:h-12 [&_[cmdk-group-heading]]:px-2 [&_[cmdk-group-heading]]:font-heading [&_[cmdk-group-heading]]:mb-1 [&_[cmdk-group]]:px-2 [&_[cmdk-input-wrapper]_svg]:h-5 [&_[cmdk-input-wrapper]_svg]:w-5 [&_[cmdk-input]]:h-12 [&_[cmdk-item]]:px-2 [&_[cmdk-item]]:py-3 [&_[cmdk-item]_svg]:h-5 [&_[cmdk-item]_svg]:w-5">

          {children}

        </Command>

      </DialogContent>

    </Dialog>

  )

}



function CommandInput({

  className,

  ...props

}: React.ComponentProps<typeof CommandPrimitive.Input>) {

  return (

    <div

      data-slot="command-input-wrapper"

      className="flex h-9 gap-2 items-center border-b-2 border-border px-3"

    >

      <Search className="size-4 shrink-0" />

      <CommandPrimitive.Input

        data-slot="command-input"

        className={cn(

          "flex h-10 w-full rounded-base bg-transparent py-3 text-sm outline-hidden placeholder:text-main-foreground placeholder:opacity-50 disabled:cursor-not-allowed disabled:opacity-50",

          className,

        )}

        {...props}

      />

    </div>

  )

}



function CommandList({

  className,

  ...props

}: React.ComponentProps<typeof CommandPrimitive.List>) {

  return (

    <CommandPrimitive.List

      data-slot="command-list"

      className={cn(

        "max-h-[300px] scroll-py-1 overflow-x-hidden overflow-y-auto",

        className,

      )}

      {...props}

    />

  )

}



function CommandEmpty({

  className,

  ...props

}: React.ComponentProps<typeof CommandPrimitive.Empty>) {

  return (

    <CommandPrimitive.Empty

      data-slot="command-empty"

      className={cn("py-6 text-center text-sm", className)}

      {...props}

    />

  )

}



function CommandGroup({

  className,

  ...props

}: React.ComponentProps<typeof CommandPrimitive.Group>) {

  return (

    <CommandPrimitive.Group

      data-slot="command-group"

      className={cn(

        "text-main-foreground overflow-hidden p-2 [&_[cmdk-group-heading]]:px-2 [&_[cmdk-group-heading]]:py-1.5 [&_[cmdk-group-heading]]:text-base [&_[cmdk-group-heading]]:font-heading",

        className,

      )}

      {...props}

    />

  )

}



function CommandSeparator({

  className,

  ...props

}: React.ComponentProps<typeof CommandPrimitive.Separator>) {

  return (

    <CommandPrimitive.Separator

      data-slot="command-separator"

      className={cn("-mx-1 h-0.5 bg-border", className)}

      {...props}

    />

  )

}



function CommandItem({

  className,

  ...props

}: React.ComponentProps<typeof CommandPrimitive.Item>) {

  return (

    <CommandPrimitive.Item

      data-slot="command-item"

      className={cn(

        "relative flex cursor-default select-none items-center rounded-base px-2 py-1.5 gap-2 text-sm text-main-foreground outline-border outline-0 aria-selected:outline-2 data-[disabled=true]:pointer-events-none data-[disabled=true]:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",

        className,

      )}

      {...props}

    />

  )

}



function CommandShortcut({

  className,

  ...props

}: React.ComponentProps<"span">) {

  return (

    <span

      data-slot="command-shortcut"

      className={cn(

        "ml-auto text-xs tracking-widest text-main-foreground",

        className,

      )}

      {...props}

    />

  )

}



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

### Context Menu <a name="context-menu"></a>

Displays a menu to the user — such as a set of actions or functions.

- **Registry Key**: `context-menu`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/context-menu.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-context-menu`

#### How to Use

```tsx
<ContextMenu>
  <ContextMenuTrigger className="flex h-[150px] w-[300px] items-center justify-center rounded-md border border-text border-dashed text-sm font-base">
    Right click here
  </ContextMenuTrigger>
  <ContextMenuContent className="w-64">
    <ContextMenuItem inset>
      Back
      <ContextMenuShortcut>⌘[</ContextMenuShortcut>
    </ContextMenuItem>
    <ContextMenuItem inset disabled>
      Forward
      <ContextMenuShortcut>⌘]</ContextMenuShortcut>
    </ContextMenuItem>
    <ContextMenuItem inset>
      Reload
      <ContextMenuShortcut>⌘R</ContextMenuShortcut>
    </ContextMenuItem>
    <ContextMenuSub>
      <ContextMenuSubTrigger inset>More Tools</ContextMenuSubTrigger>
      <ContextMenuSubContent className="w-48">
        <ContextMenuItem>
          Save Page As...
          <ContextMenuShortcut>⇧⌘S</ContextMenuShortcut>
        </ContextMenuItem>
        <ContextMenuItem>Create Shortcut...</ContextMenuItem>
        <ContextMenuItem>Name Window...</ContextMenuItem>
        <ContextMenuSeparator />
        <ContextMenuItem>Developer Tools</ContextMenuItem>
      </ContextMenuSubContent>
    </ContextMenuSub>
    <ContextMenuSeparator />
    <ContextMenuCheckboxItem checked>
      Show Bookmarks Bar
      <ContextMenuShortcut>⌘⇧B</ContextMenuShortcut>
    </ContextMenuCheckboxItem>
    <ContextMenuCheckboxItem>Show Full URLs</ContextMenuCheckboxItem>
    <ContextMenuSeparator />
    <ContextMenuRadioGroup value="pedro">
      <ContextMenuLabel inset>People</ContextMenuLabel>
      <ContextMenuSeparator />
      <ContextMenuRadioItem value="pedro">
        Pedro Duarte
      </ContextMenuRadioItem>
      <ContextMenuRadioItem value="colm">Colm Tuite</ContextMenuRadioItem>
    </ContextMenuRadioGroup>
  </ContextMenuContent>
</ContextMenu>
```

#### Component Source Code

##### File: `src/components/ui/context-menu.tsx`

```tsx
"use client"



import * as ContextMenuPrimitive from "@radix-ui/react-context-menu"

import { Check, ChevronRight, Circle } from "lucide-react"



import * as React from "react"



import { cn } from "@/lib/utils"



function ContextMenu({

  ...props

}: React.ComponentProps<typeof ContextMenuPrimitive.Root>) {

  return <ContextMenuPrimitive.Root data-slot="context-menu" {...props} />

}



const ContextMenuTrigger = ({

  ...props

}: React.ComponentProps<typeof ContextMenuPrimitive.Trigger>) => {

  return (

    <ContextMenuPrimitive.Trigger data-slot="context-menu-trigger" {...props} />

  )

}



const ContextMenuGroup = ({

  ...props

}: React.ComponentProps<typeof ContextMenuPrimitive.Group>) => {

  return (

    <ContextMenuPrimitive.Group data-slot="context-menu-group" {...props} />

  )

}



const ContextMenuPortal = ({

  ...props

}: React.ComponentProps<typeof ContextMenuPrimitive.Portal>) => {

  return (

    <ContextMenuPrimitive.Portal data-slot="context-menu-portal" {...props} />

  )

}



const ContextMenuSub = ({

  ...props

}: React.ComponentProps<typeof ContextMenuPrimitive.Sub>) => {

  return <ContextMenuPrimitive.Sub data-slot="context-menu-sub" {...props} />

}



const ContextMenuRadioGroup = ({

  ...props

}: React.ComponentProps<typeof ContextMenuPrimitive.RadioGroup>) => {

  return (

    <ContextMenuPrimitive.RadioGroup

      data-slot="context-menu-radio-group"

      {...props}

    />

  )

}



function ContextMenuSubTrigger({

  className,

  inset,

  children,

  ...props

}: React.ComponentPropsWithoutRef<typeof ContextMenuPrimitive.SubTrigger> & {

  inset?: boolean

}) {

  return (

    <ContextMenuPrimitive.SubTrigger

      data-slot="context-menu-sub-trigger"

      data-inset={inset}

      className={cn(

        "flex cursor-default select-none items-center rounded-base border-2 border-transparent bg-main px-2 py-1.5 text-sm font-base text-main-foreground outline-hidden data-[inset]:pl-8 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 focus:border-border data-[state=open]:border-border",

        className,

      )}

      {...props}

    >

      {children}

      <ChevronRight className="ml-auto" />

    </ContextMenuPrimitive.SubTrigger>

  )

}



function ContextMenuSubContent({

  className,

  ...props

}: React.ComponentPropsWithoutRef<typeof ContextMenuPrimitive.SubContent>) {

  return (

    <ContextMenuPrimitive.SubContent

      data-slot="context-menu-sub-content"

      className={cn(

        "z-50 min-w-[8rem] overflow-hidden rounded-base border-2 border-border bg-main p-1 font-base text-main-foreground data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 origin-(--radix-context-menu-content-transform-origin)",

        className,

      )}

      {...props}

    />

  )

}



function ContextMenuContent({

  className,

  ...props

}: React.ComponentPropsWithoutRef<typeof ContextMenuPrimitive.Content>) {

  return (

    <ContextMenuPrimitive.Portal>

      <ContextMenuPrimitive.Content

        data-slot="context-menu-content"

        className={cn(

          "z-50 min-w-[8rem] overflow-hidden rounded-base border-2 border-border bg-main p-1 font-base text-main-foreground shadow-md animate-in fade-in-80 data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 origin-(--radix-context-menu-content-transform-origin)",

          className,

        )}

        {...props}

      />

    </ContextMenuPrimitive.Portal>

  )

}



function ContextMenuItem({

  className,

  inset,

  ...props

}: React.ComponentProps<typeof ContextMenuPrimitive.Item> & {

  inset?: boolean

}) {

  return (

    <ContextMenuPrimitive.Item

      data-slot="context-menu-item"

      data-inset={inset}

      className={cn(

        "relative flex cursor-default select-none items-center rounded-base border-2 border-transparent px-2 py-1.5 gap-2 text-sm outline-none focus:border-border data-[disabled]:pointer-events-none data-[disabled]:opacity-50 data-[inset]:pl-8 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",

        className,

      )}

      {...props}

    />

  )

}



function ContextMenuCheckboxItem({

  className,

  children,

  checked,

  ...props

}: React.ComponentPropsWithoutRef<typeof ContextMenuPrimitive.CheckboxItem>) {

  return (

    <ContextMenuPrimitive.CheckboxItem

      data-slot="context-menu-checkbox-item"

      className={cn(

        "relative flex cursor-default select-none items-center rounded-base border-2 border-transparent py-1.5 pl-8 pr-2 gap-2 text-sm font-base text-main-foreground outline-hidden focus:border-border data-disabled:pointer-events-none data-disabled:opacity-50",

        className,

      )}

      checked={checked}

      {...props}

    >

      <span className="absolute left-2 flex h-3.5 w-3.5 items-center justify-center">

        <ContextMenuPrimitive.ItemIndicator>

          <Check className="h-4 w-4" />

        </ContextMenuPrimitive.ItemIndicator>

      </span>

      {children}

    </ContextMenuPrimitive.CheckboxItem>

  )

}



function ContextMenuRadioItem({

  className,

  children,

  ...props

}: React.ComponentPropsWithoutRef<typeof ContextMenuPrimitive.RadioItem>) {

  return (

    <ContextMenuPrimitive.RadioItem

      data-slot="context-menu-radio-item"

      className={cn(

        "relative flex cursor-default select-none items-center rounded-base border-2 border-transparent py-1.5 pl-8 pr-2 gap-2 text-sm font-base text-main-foreground outline-hidden focus:border-border data-disabled:pointer-events-none data-disabled:opacity-50",

        className,

      )}

      {...props}

    >

      <span className="absolute left-2 flex h-3.5 w-3.5 items-center justify-center">

        <ContextMenuPrimitive.ItemIndicator>

          <Circle className="h-2 w-2 fill-current" />

        </ContextMenuPrimitive.ItemIndicator>

      </span>

      {children}

    </ContextMenuPrimitive.RadioItem>

  )

}



function ContextMenuLabel({

  className,

  inset,

  ...props

}: React.ComponentPropsWithoutRef<typeof ContextMenuPrimitive.Label> & {

  inset?: boolean

}) {

  return (

    <ContextMenuPrimitive.Label

      data-slot="context-menu-label"

      data-inset={inset}

      className={cn(

        "px-2 py-1.5 text-sm font-base border-2 border-transparent text-main-foreground data-[inset]:pl-8",

        className,

      )}

      {...props}

    />

  )

}



function ContextMenuSeparator({

  className,

  ...props

}: React.ComponentPropsWithoutRef<typeof ContextMenuPrimitive.Separator>) {

  return (

    <ContextMenuPrimitive.Separator

      data-slot="context-menu-separator"

      className={cn("-mx-1 my-1 h-0.5 bg-border", className)}

      {...props}

    />

  )

}



const ContextMenuShortcut = ({

  className,

  ...props

}: React.HTMLAttributes<HTMLSpanElement>) => {

  return (

    <span

      className={cn(

        "ml-auto text-xs font-base tracking-widest text-main-foreground",

        className,

      )}

      {...props}

    />

  )

}

ContextMenuShortcut.displayName = "ContextMenuShortcut"



export {

  ContextMenu,

  ContextMenuTrigger,

  ContextMenuContent,

  ContextMenuItem,

  ContextMenuCheckboxItem,

  ContextMenuRadioItem,

  ContextMenuLabel,

  ContextMenuSeparator,

  ContextMenuShortcut,

  ContextMenuGroup,

  ContextMenuPortal,

  ContextMenuSub,

  ContextMenuSubContent,

  ContextMenuSubTrigger,

  ContextMenuRadioGroup,

}
```

---

### Data Table <a name="data-table"></a>

Powerful table and datagrids built using TanStack Table.

- **Registry Key**: `data-table`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/data-table.json
```

#### How to Use

```tsx
<DataTableDemo />
```

---

### Date Picker <a name="date-picker"></a>

A date picker component with calendar popup.

- **Registry Key**: `date-picker`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/date-picker.json
```

#### How to Use

##### Example 1

```tsx
const [date, setDate] = React.useState<Date>()
```

##### Example 2

```tsx
<Popover>
  <PopoverTrigger asChild>
    <Button
      variant="noShadow"
      className="w-[280px] justify-start text-left font-base"
    >
      <CalendarIcon />
      {date ? format(date, "PPP") : <span>Pick a date</span>}
    </Button>
  </PopoverTrigger>
  <PopoverContent className="w-auto border-0! p-0">
    <Calendar
      mode="single"
      selected={date}
      onSelect={setDate}
      initialFocus
    />
  </PopoverContent>
</Popover>
```

---

### Dialog <a name="dialog"></a>

A window overlaid on either the primary window or another dialog window.

- **Registry Key**: `dialog`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/dialog.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-dialog`

#### How to Use

```tsx
<Dialog>
  <form>
    <DialogTrigger asChild>
      <Button>Edit Profile</Button>
    </DialogTrigger>
    <DialogContent className="sm:max-w-[425px]">
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
          <Button variant="neutral">Cancel</Button>
        </DialogClose>
        <Button type="submit">Save changes</Button>
      </DialogFooter>
    </DialogContent>
  </form>
</Dialog>
```

#### Component Source Code

##### File: `src/components/ui/dialog.tsx`

```tsx
"use client"



import * as DialogPrimitive from "@radix-ui/react-dialog"

import { X } from "lucide-react"



import * as React from "react"



import { cn } from "@/lib/utils"



function Dialog({

  ...props

}: React.ComponentProps<typeof DialogPrimitive.Root>) {

  return <DialogPrimitive.Root data-slot="dialog" {...props} />

}



function DialogTrigger({

  ...props

}: React.ComponentProps<typeof DialogPrimitive.Trigger>) {

  return <DialogPrimitive.Trigger data-slot="dialog-trigger" {...props} />

}



function DialogPortal({

  ...props

}: React.ComponentProps<typeof DialogPrimitive.Portal>) {

  return <DialogPrimitive.Portal data-slot="dialog-portal" {...props} />

}



function DialogClose({

  ...props

}: React.ComponentProps<typeof DialogPrimitive.Close>) {

  return <DialogPrimitive.Close data-slot="dialog-close" {...props} />

}



function DialogOverlay({

  className,

  ...props

}: React.ComponentProps<typeof DialogPrimitive.Overlay>) {

  return (

    <DialogPrimitive.Overlay

      data-slot="dialog-overlay"

      className={cn(

        "fixed inset-0 z-50 bg-overlay data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0",

        className,

      )}

      {...props}

    />

  )

}



function DialogContent({

  className,

  children,

  ...props

}: React.ComponentProps<typeof DialogPrimitive.Content>) {

  return (

    <DialogPortal>

      <DialogOverlay />

      <DialogPrimitive.Content

        data-slot="dialog-content"

        className={cn(

          "bg-background data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 fixed top-[50%] left-[50%] z-50 grid w-full max-w-[calc(100%-2rem)] translate-x-[-50%] translate-y-[-50%] gap-4 rounded-base border-2 border-border p-6 shadow-shadow duration-200 sm:max-w-lg",

          className,

        )}

        {...props}

      >

        {children}

        <DialogPrimitive.Close className="absolute right-4 top-4 rounded-base opacity-100 ring-offset-white focus:outline-hidden focus:ring-2 focus:ring-black focus:ring-offset-2 disabled:pointer-events-none [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4">

          <X />

          <span className="sr-only">Close</span>

        </DialogPrimitive.Close>

      </DialogPrimitive.Content>

    </DialogPortal>

  )

}



function DialogHeader({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="dialog-header"

      className={cn("flex flex-col gap-2 text-center sm:text-left", className)}

      {...props}

    />

  )

}



function DialogFooter({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="dialog-footer"

      className={cn(

        "flex flex-col-reverse gap-3 sm:flex-row sm:justify-end",

        className,

      )}

      {...props}

    />

  )

}



function DialogTitle({

  className,

  ...props

}: React.ComponentProps<typeof DialogPrimitive.Title>) {

  return (

    <DialogPrimitive.Title

      data-slot="dialog-title"

      className={cn(

        "text-lg font-heading leading-none tracking-tight",

        className,

      )}

      {...props}

    />

  )

}



function DialogDescription({

  className,

  ...props

}: React.ComponentProps<typeof DialogPrimitive.Description>) {

  return (

    <DialogPrimitive.Description

      data-slot="dialog-description"

      className={cn("text-sm font-base text-foreground", className)}

      {...props}

    />

  )

}



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

### Drawer <a name="drawer"></a>

A drawer component that slides out from the edge of the screen.

- **Registry Key**: `drawer`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/drawer.json
```

#### Dependencies

- **NPM Packages**:
  - `vaul`
  - `@radix-ui/react-dialog`

#### How to Use

```tsx
<Drawer>
  <DrawerTrigger asChild>
    <Button>Open</Button>
  </DrawerTrigger>
  <DrawerContent>
    <div className="mx-auto w-[300px]">
      <DrawerHeader>
        <DrawerTitle>Are you absolutely sure?</DrawerTitle>
        <DrawerDescription>This action cannot be undone.</DrawerDescription>
      </DrawerHeader>
      <DrawerFooter className="grid grid-cols-2">
        <Button example="noShadow">Submit</Button>
        <DrawerClose asChild>
          <Button className="bg-secondary-background text-foreground" example="noShadow">
            Cancel
          </Button>
        </DrawerClose>
      </DrawerFooter>
    </div>
  </DrawerContent>
</Drawer>
```

#### Component Source Code

##### File: `src/components/ui/drawer.tsx`

```tsx
"use client"



import { Drawer as DrawerPrimitive } from "vaul"



import * as React from "react"



import { cn } from "@/lib/utils"



function Drawer({

  shouldScaleBackground = true,

  ...props

}: React.ComponentProps<typeof DrawerPrimitive.Root>) {

  return (

    <DrawerPrimitive.Root

      data-slot="drawer"

      shouldScaleBackground={shouldScaleBackground}

      {...props}

    />

  )

}



function DrawerTrigger({

  ...props

}: React.ComponentProps<typeof DrawerPrimitive.Trigger>) {

  return <DrawerPrimitive.Trigger data-slot="drawer-trigger" {...props} />

}



function DrawerPortal({

  ...props

}: React.ComponentProps<typeof DrawerPrimitive.Portal>) {

  return <DrawerPrimitive.Portal data-slot="drawer-portal" {...props} />

}



function DrawerClose({

  ...props

}: React.ComponentProps<typeof DrawerPrimitive.Close>) {

  return <DrawerPrimitive.Close data-slot="drawer-close" {...props} />

}



function DrawerOverlay({

  className,

  ...props

}: React.ComponentProps<typeof DrawerPrimitive.Overlay>) {

  return (

    <DrawerPrimitive.Overlay

      data-slot="drawer-overlay"

      className={cn(

        "data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 fixed inset-0 z-50 bg-overlay",

        className,

      )}

      {...props}

    />

  )

}



function DrawerContent({

  className,

  children,

  ...props

}: React.ComponentProps<typeof DrawerPrimitive.Content>) {

  return (

    <DrawerPortal>

      <DrawerOverlay />

      <DrawerPrimitive.Content

        data-slot="drawer-content"

        className={cn(

          "bg-background group/drawer-content fixed z-50 flex h-auto flex-col",

          "data-[vaul-drawer-direction=top]:inset-x-0 data-[vaul-drawer-direction=top]:top-0 data-[vaul-drawer-direction=top]:mb-24 data-[vaul-drawer-direction=top]:max-h-[80vh] data-[vaul-drawer-direction=top]:rounded-b-base border-t-2 border-t-border",

          "data-[vaul-drawer-direction=bottom]:inset-x-0 data-[vaul-drawer-direction=bottom]:bottom-0 data-[vaul-drawer-direction=bottom]:mt-24 data-[vaul-drawer-direction=bottom]:max-h-[80vh] data-[vaul-drawer-direction=bottom]:rounded-t-base border-b-2 border-b-border",

          "data-[vaul-drawer-direction=right]:inset-y-0 data-[vaul-drawer-direction=right]:right-0 data-[vaul-drawer-direction=right]:w-3/4 data-[vaul-drawer-direction=right]:sm:max-w-sm border-r-2 border-r-border",

          "data-[vaul-drawer-direction=left]:inset-y-0 data-[vaul-drawer-direction=left]:left-0 data-[vaul-drawer-direction=left]:w-3/4 data-[vaul-drawer-direction=left]:sm:max-w-sm border-l-2 border-l-border",

          className,

        )}

        {...props}

      >

        <div className="mx-auto mt-4 hidden h-2 w-[100px] shrink-0 rounded-full group-data-[vaul-drawer-direction=bottom]/drawer-content:block bg-current" />

        {children}

      </DrawerPrimitive.Content>

    </DrawerPortal>

  )

}



function DrawerHeader({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="drawer-header"

      className={cn("grid gap-1.5 p-4 text-center sm:text-left", className)}

      {...props}

    />

  )

}



function DrawerFooter({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="drawer-footer"

      className={cn("mt-auto flex flex-col gap-3 p-4", className)}

      {...props}

    />

  )

}



function DrawerTitle({

  className,

  ...props

}: React.ComponentProps<typeof DrawerPrimitive.Title>) {

  return (

    <DrawerPrimitive.Title

      data-slot="drawer-title"

      className={cn(

        "text-lg font-heading leading-none tracking-tight",

        className,

      )}

      {...props}

    />

  )

}



function DrawerDescription({

  className,

  ...props

}: React.ComponentProps<typeof DrawerPrimitive.Description>) {

  return (

    <DrawerPrimitive.Description

      data-slot="drawer-description"

      className={cn("text-sm font-base text-foreground", className)}

      {...props}

    />

  )

}



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

Displays a menu to the user — such as a set of actions or functions.

- **Registry Key**: `dropdown-menu`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/dropdown-menu.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-dropdown-menu`

#### How to Use

```tsx
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button example={'noShadow'}>Open</Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent className="w-56">
    <DropdownMenuLabel>My Account</DropdownMenuLabel>
    <DropdownMenuSeparator />
    <DropdownMenuGroup>
      <DropdownMenuItem>
        <User />
        <span>Profile</span>
        <DropdownMenuShortcut>⇧⌘P</DropdownMenuShortcut>
      </DropdownMenuItem>
      <DropdownMenuItem>
        <CreditCard />
        <span>Billing</span>
        <DropdownMenuShortcut>⌘B</DropdownMenuShortcut>
      </DropdownMenuItem>
      <DropdownMenuItem>
        <Settings />
        <span>Settings</span>
        <DropdownMenuShortcut>⌘S</DropdownMenuShortcut>
      </DropdownMenuItem>
      <DropdownMenuItem>
        <Keyboard />
        <span>Keyboard shortcuts</span>
        <DropdownMenuShortcut>⌘K</DropdownMenuShortcut>
      </DropdownMenuItem>
    </DropdownMenuGroup>
    <DropdownMenuSeparator />
    <DropdownMenuGroup>
      <DropdownMenuItem>
        <Users />
        <span>Team</span>
      </DropdownMenuItem>
      <DropdownMenuSub>
        <DropdownMenuSubTrigger>
          <UserPlus />
          <span>Invite users</span>
        </DropdownMenuSubTrigger>
        <DropdownMenuPortal>
          <DropdownMenuSubContent>
            <DropdownMenuItem>
              <Mail />
              <span>Email</span>
            </DropdownMenuItem>
            <DropdownMenuItem>
              <MessageSquare />
              <span>Message</span>
            </DropdownMenuItem>
            <DropdownMenuSeparator />
            <DropdownMenuItem>
              <PlusCircle />
              <span>More...</span>
            </DropdownMenuItem>
          </DropdownMenuSubContent>
        </DropdownMenuPortal>
      </DropdownMenuSub>
      <DropdownMenuItem>
        <Plus />
        <span>New Team</span>
        <DropdownMenuShortcut>⌘+T</DropdownMenuShortcut>
      </DropdownMenuItem>
    </DropdownMenuGroup>
    <DropdownMenuSeparator />
    <DropdownMenuItem>
      <Github />
      <span>GitHub</span>
    </DropdownMenuItem>
    <DropdownMenuItem>
      <LifeBuoy />
      <span>Support</span>
    </DropdownMenuItem>
    <DropdownMenuItem disabled>
      <Cloud />
      <span>API</span>
    </DropdownMenuItem>
    <DropdownMenuSeparator />
    <DropdownMenuItem>
      <LogOut />
      <span>Log out</span>
      <DropdownMenuShortcut>⇧⌘Q</DropdownMenuShortcut>
    </DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

#### Component Source Code

##### File: `src/components/ui/dropdown-menu.tsx`

```tsx
"use client"



import * as DropdownMenuPrimitive from "@radix-ui/react-dropdown-menu"

import { Check, ChevronRight, Circle } from "lucide-react"



import * as React from "react"



import { cn } from "@/lib/utils"



function DropdownMenu({

  ...props

}: React.ComponentProps<typeof DropdownMenuPrimitive.Root>) {

  return <DropdownMenuPrimitive.Root data-slot="dropdown-menu" {...props} />

}



function DropdownMenuTrigger({

  ...props

}: React.ComponentProps<typeof DropdownMenuPrimitive.Trigger>) {

  return (

    <DropdownMenuPrimitive.Trigger

      data-slot="dropdown-menu-trigger"

      {...props}

    />

  )

}



function DropdownMenuGroup({

  ...props

}: React.ComponentProps<typeof DropdownMenuPrimitive.Group>) {

  return <DropdownMenuPrimitive.Group {...props} />

}



function DropdownMenuPortal({

  ...props

}: React.ComponentProps<typeof DropdownMenuPrimitive.Portal>) {

  return <DropdownMenuPrimitive.Portal {...props} />

}



function DropdownMenuSub({

  ...props

}: React.ComponentProps<typeof DropdownMenuPrimitive.Sub>) {

  return <DropdownMenuPrimitive.Sub {...props} />

}



function DropdownMenuRadioGroup({

  ...props

}: React.ComponentProps<typeof DropdownMenuPrimitive.RadioGroup>) {

  return <DropdownMenuPrimitive.RadioGroup {...props} />

}



function DropdownMenuSubTrigger({

  className,

  inset,

  children,

  ...props

}: React.ComponentProps<typeof DropdownMenuPrimitive.SubTrigger> & {

  inset?: boolean

}) {

  return (

    <DropdownMenuPrimitive.SubTrigger

      data-slot="dropdown-menu-sub-trigger"

      data-inset={inset}

      className={cn(

        "flex cursor-default select-none items-center rounded-base border-2 border-transparent bg-main px-2 py-1.5 text-sm font-base outline-hidden focus:border-border gap-2 data-[inset=true]:pl-8 [&_svg]:pointer-events-none [&_svg]:w-4 [&_svg]:h-4 [&_svg]:shrink-0",

        className,

      )}

      {...props}

    >

      {children}

      <ChevronRight className="ml-auto" />

    </DropdownMenuPrimitive.SubTrigger>

  )

}



function DropdownMenuSubContent({

  className,

  ...props

}: React.ComponentProps<typeof DropdownMenuPrimitive.SubContent>) {

  return (

    <DropdownMenuPrimitive.SubContent

      className={cn(

        "z-50 min-w-[8rem] overflow-hidden rounded-base border-2 border-border bg-main p-1 font-base text-main-foreground data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 origin-(--radix-dropdown-menu-content-transform-origin)",

        className,

      )}

      {...props}

    />

  )

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

          "z-50 min-w-[8rem] overflow-hidden rounded-base border-2 border-border bg-main p-1 font-base text-main-foreground data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 origin-(--radix-dropdown-menu-content-transform-origin)",

          className,

        )}

        {...props}

      />

    </DropdownMenuPrimitive.Portal>

  )

}



function DropdownMenuItem({

  className,

  inset,

  ...props

}: React.ComponentProps<typeof DropdownMenuPrimitive.Item> & {

  inset?: boolean

}) {

  return (

    <DropdownMenuPrimitive.Item

      data-slot="dropdown-menu-item"

      data-inset={inset}

      className={cn(

        "relative gap-2 [&_svg]:pointer-events-none [&_svg]:w-4 [&_svg]:h-4 [&_svg]:shrink-0 flex cursor-default select-none items-center rounded-base border-2 border-transparent data-[inset=true]:pl-8 bg-main px-2 py-1.5 text-sm font-base outline-hidden transition-colors focus:border-border data-disabled:pointer-events-none data-disabled:opacity-50",

        className,

      )}

      {...props}

    />

  )

}



function DropdownMenuCheckboxItem({

  className,

  children,

  checked,

  ...props

}: React.ComponentProps<typeof DropdownMenuPrimitive.CheckboxItem>) {

  return (

    <DropdownMenuPrimitive.CheckboxItem

      className={cn(

        "relative flex cursor-default select-none items-center rounded-base border-2 border-transparent gap-2 py-1.5 pl-8 pr-2 text-sm font-base text-main-foreground outline-hidden transition-colors focus:border-border data-disabled:pointer-events-none data-disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",

        className,

      )}

      checked={checked}

      {...props}

    >

      <span className="absolute left-2 flex size-3.5 items-center justify-center">

        <DropdownMenuPrimitive.ItemIndicator>

          <Check />

        </DropdownMenuPrimitive.ItemIndicator>

      </span>

      {children}

    </DropdownMenuPrimitive.CheckboxItem>

  )

}



function DropdownMenuRadioItem({

  className,

  children,

  ...props

}: React.ComponentProps<typeof DropdownMenuPrimitive.RadioItem>) {

  return (

    <DropdownMenuPrimitive.RadioItem

      className={cn(

        "relative flex cursor-default select-none items-center rounded-base border-2 border-transparent gap-2 py-1.5 pl-8 pr-2 text-sm font-base outline-hidden transition-colors focus:border-border data-disabled:pointer-events-none data-disabled:opacity-50",

        className,

      )}

      {...props}

    >

      <span className="absolute left-2 flex size-3.5 items-center justify-center">

        <DropdownMenuPrimitive.ItemIndicator>

          <Circle className="size-2 fill-current" />

        </DropdownMenuPrimitive.ItemIndicator>

      </span>

      {children}

    </DropdownMenuPrimitive.RadioItem>

  )

}



function DropdownMenuLabel({

  className,

  inset,

  ...props

}: React.ComponentProps<typeof DropdownMenuPrimitive.Label> & {

  inset?: boolean

}) {

  return (

    <DropdownMenuPrimitive.Label

      data-slot="dropdown-menu-label"

      data-inset={inset}

      className={cn(

        "px-2 py-1.5 text-sm font-heading data-[inset]:pl-8",

        className,

      )}

      {...props}

    />

  )

}



function DropdownMenuSeparator({

  className,

  ...props

}: React.ComponentProps<typeof DropdownMenuPrimitive.Separator>) {

  return (

    <DropdownMenuPrimitive.Separator

      className={cn("-mx-1 my-1 h-0.5 bg-border", className)}

      {...props}

    />

  )

}



function DropdownMenuShortcut({

  className,

  ...props

}: React.HTMLAttributes<HTMLSpanElement>) {

  return (

    <span

      data-slot="dropdown-menu-shortcut"

      className={cn("ml-auto text-xs font-base tracking-widest", className)}

      {...props}

    />

  )

}



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

### Form <a name="form"></a>

Building forms with React Hook Form and Zod.

- **Registry Key**: `form`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/form.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-label`
  - `@radix-ui/react-slot`
  - `@hookform/resolvers`
  - `zod`
  - `react-hook-form`
- **Local Registry Dependencies**:
  - `https://neobrutalism.dev/r/nbutton.json`
  - `https://neobrutalism.dev/r/nlabel.json`

#### How to Use

```tsx
<Form {...form}>
  <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-8">
    <FormField
      control={form.control}
      name="username"
      render={({ field }) => (
        <FormItem>
          <FormLabel>Username</FormLabel>
          <FormControl>
            <Input placeholder="ekmas" {...field} />
          </FormControl>
          <FormDescription>This is your public display name.</FormDescription>
          <FormMessage />
        </FormItem>
      )}
    />
    <Button type="submit">Submit</Button>
  </form>
</Form>
```

#### Component Source Code

##### File: `src/components/ui/form.tsx`

```tsx
"use client"



import * as LabelPrimitive from "@radix-ui/react-label"

import { Slot } from "@radix-ui/react-slot"

import {

  Controller,

  FormProvider,

  useFormContext,

  useFormState,

  type ControllerProps,

  type FieldPath,

  type FieldValues,

} from "react-hook-form"



import * as React from "react"



import { Label } from "@/components/ui/label"



import { cn } from "@/lib/utils"



const Form = FormProvider



type FormFieldContextValue<

  TFieldValues extends FieldValues = FieldValues,

  TName extends FieldPath<TFieldValues> = FieldPath<TFieldValues>,

> = {

  name: TName

}



const FormFieldContext = React.createContext<FormFieldContextValue>(

  {} as FormFieldContextValue,

)



function FormField<

  TFieldValues extends FieldValues = FieldValues,

  TName extends FieldPath<TFieldValues> = FieldPath<TFieldValues>,

>({ ...props }: ControllerProps<TFieldValues, TName>) {

  return (

    <FormFieldContext.Provider value={{ name: props.name }}>

      <Controller {...props} />

    </FormFieldContext.Provider>

  )

}



const useFormField = () => {

  const fieldContext = React.useContext(FormFieldContext)

  const itemContext = React.useContext(FormItemContext)

  const { getFieldState } = useFormContext()

  const formState = useFormState({ name: fieldContext.name })

  const fieldState = getFieldState(fieldContext.name, formState)



  if (!fieldContext) {

    throw new Error("useFormField should be used within <FormField>")

  }



  const { id } = itemContext



  return {

    id,

    name: fieldContext.name,

    formItemId: `${id}-form-item`,

    formDescriptionId: `${id}-form-item-description`,

    formMessageId: `${id}-form-item-message`,

    ...fieldState,

  }

}



type FormItemContextValue = {

  id: string

}



const FormItemContext = React.createContext<FormItemContextValue>(

  {} as FormItemContextValue,

)



function FormItem({ className, ...props }: React.ComponentProps<"div">) {

  const id = React.useId()



  return (

    <FormItemContext.Provider value={{ id }}>

      <div

        data-slot="form-item"

        className={cn("grid gap-2", className)}

        {...props}

      />

    </FormItemContext.Provider>

  )

}



function FormLabel({

  className,

  ...props

}: React.ComponentProps<typeof LabelPrimitive.Root>) {

  const { error, formItemId } = useFormField()



  return (

    <Label

      data-slot="form-label"

      data-error={!!error}

      className={cn("font-heading", className)}

      htmlFor={formItemId}

      {...props}

    />

  )

}



function FormControl({ ...props }: React.ComponentProps<typeof Slot>) {

  const { error, formItemId, formDescriptionId, formMessageId } = useFormField()



  return (

    <Slot

      data-slot="form-control"

      id={formItemId}

      aria-describedby={

        !error

          ? `${formDescriptionId}`

          : `${formDescriptionId} ${formMessageId}`

      }

      aria-invalid={!!error}

      {...props}

    />

  )

}



function FormDescription({ className, ...props }: React.ComponentProps<"p">) {

  const { formDescriptionId } = useFormField()



  return (

    <p

      data-slot="form-description"

      id={formDescriptionId}

      className={cn("text-sm font-base text-foreground", className)}

      {...props}

    />

  )

}



function FormMessage({ className, ...props }: React.ComponentProps<"p">) {

  const { error, formMessageId } = useFormField()

  const body = error ? String(error?.message ?? "") : props.children



  if (!body) {

    return null

  }



  return (

    <p

      data-slot="form-message"

      id={formMessageId}

      className={cn("text-sm font-base text-red-500", className)}

      {...props}

    >

      {body}

    </p>

  )

}



export {

  useFormField,

  Form,

  FormItem,

  FormLabel,

  FormControl,

  FormDescription,

  FormMessage,

  FormField,

}
```

---

### Hover Card <a name="hover-card"></a>

For sighted users to preview content available behind a link.

- **Registry Key**: `hover-card`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/hover-card.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-hover-card`

#### How to Use

```tsx
<HoverCard>
  <HoverCardTrigger asChild>
    <Button example="noShadow">Hover</Button>
  </HoverCardTrigger>
  <HoverCardContent>
    The React Framework – created and maintained by @vercel.
  </HoverCardContent>
</HoverCard>
```

#### Component Source Code

##### File: `src/components/ui/hover-card.tsx`

```tsx
"use client"



import * as HoverCardPrimitive from "@radix-ui/react-hover-card"



import * as React from "react"



import { cn } from "@/lib/utils"



function HoverCard({

  ...props

}: React.ComponentProps<typeof HoverCardPrimitive.Root>) {

  return <HoverCardPrimitive.Root data-slot="hover-card" {...props} />

}



function HoverCardTrigger({

  ...props

}: React.ComponentProps<typeof HoverCardPrimitive.Trigger>) {

  return (

    <HoverCardPrimitive.Trigger data-slot="hover-card-trigger" {...props} />

  )

}



function HoverCardContent({

  className,

  align = "center",

  sideOffset = 4,

  ...props

}: React.ComponentProps<typeof HoverCardPrimitive.Content>) {

  return (

    <HoverCardPrimitive.Content

      data-slot="hover-card-content"

      align={align}

      sideOffset={sideOffset}

      className={cn(

        "z-50 w-64 rounded-base border-2 border-border bg-main p-4 font-base text-main-foreground data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 origin-(--radix-hover-card-content-transform-origin)",

        className,

      )}

      {...props}

    />

  )

}



export { HoverCard, HoverCardTrigger, HoverCardContent }
```

---

### Image Card <a name="image-card"></a>

A card component optimized for displaying images with captions.

- **Registry Key**: `image-card`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/image-card.json
```

#### How to Use

```tsx
<ImageCard
  caption="Image"
  imageUrl="https://hips.hearstapps.com/hmg-prod/images/flowers-trees-and-bushes-reach-their-peak-of-full-bloom-in-news-photo-1678292967.jpg?resize=300:*"
></ImageCard>
```

#### Component Source Code

##### File: `src/components/ui/image-card.tsx`

```tsx
import { cn } from "@/lib/utils"



type Props = {

  imageUrl: string

  caption: string

  className?: string

}



export default function ImageCard({ imageUrl, caption, className }: Props) {

  return (

    <figure

      className={cn(

        "w-[250px] overflow-hidden rounded-base border-2 border-border bg-main font-base shadow-shadow",

        className,

      )}

    >

      <img className="w-full aspect-4/3" src={imageUrl} alt="image" />

      <figcaption className="border-t-2 text-main-foreground border-border p-4">

        {caption}

      </figcaption>

    </figure>

  )

}
```

---

### Input OTP <a name="input-otp"></a>

Accessible one-time password input for authentication.

- **Registry Key**: `input-otp`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/input-otp.json
```

#### Dependencies

- **NPM Packages**:
  - `input-otp`

#### How to Use

```tsx
<InputOTP maxLength={6}>
  <InputOTPGroup>
    <InputOTPSlot index={0} />
    <InputOTPSlot index={1} />
    <InputOTPSlot index={2} />
  </InputOTPGroup>
  <InputOTPSeparator />
  <InputOTPGroup>
    <InputOTPSlot index={3} />
    <InputOTPSlot index={4} />
    <InputOTPSlot index={5} />
  </InputOTPGroup>
</InputOTP>
```

#### Component Source Code

##### File: `src/components/ui/input-otp.tsx`

```tsx
"use client"



import { OTPInput, OTPInputContext } from "input-otp"

import { Dot } from "lucide-react"



import * as React from "react"



import { cn } from "@/lib/utils"



function InputOTP({

  className,

  containerClassName,

  ...props

}: React.ComponentProps<typeof OTPInput> & {

  containerClassName?: string

}) {

  return (

    <OTPInput

      data-slot="input-otp"

      containerClassName={cn(

        "flex items-center gap-2 has-disabled:opacity-50",

        containerClassName,

      )}

      className={cn("disabled:cursor-not-allowed", className)}

      {...props}

    />

  )

}



function InputOTPGroup({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="input-otp-group"

      className={cn("flex items-center", className)}

      {...props}

    />

  )

}



function InputOTPSlot({

  index,

  className,

  ...props

}: React.ComponentProps<"div"> & { index: number }) {

  const inputOTPContext = React.useContext(OTPInputContext)

  const { char, hasFakeCaret, isActive } = inputOTPContext?.slots[index] ?? {}



  return (

    <div

      data-slot="input-otp-slot"

      data-active={isActive}

      className={cn(

        "relative flex size-10 items-center justify-center border-y-2 border-r-2 border-border bg-secondary-background text-sm font-base text-foreground first:rounded-l-base first:border-l-2 last:rounded-r-base transition-all",

        isActive && "z-10 ring-1 ring-ring",

        className,

      )}

      {...props}

    >

      {char}

      {hasFakeCaret && (

        <div className="pointer-events-none absolute inset-0 flex items-center justify-center">

          <div className="h-4 w-px animate-caret-blink bg-current duration-1000" />

        </div>

      )}

    </div>

  )

}



function InputOTPSeparator({ ...props }: React.ComponentProps<"div">) {

  return (

    <div data-slot="input-otp-separator" role="separator" {...props}>

      <Dot className="size-4" />

    </div>

  )

}



export { InputOTP, InputOTPGroup, InputOTPSlot, InputOTPSeparator }
```

---

### Input <a name="input"></a>

Displays a form input field or a component that looks like an input field.

- **Registry Key**: `input`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/input.json
```

#### How to Use

```tsx
<Input className="w-[200px]" type="email" placeholder="Email" />
```

#### Component Source Code

##### File: `src/components/ui/input.tsx`

```tsx
import * as React from "react"



import { cn } from "@/lib/utils"



function Input({ className, type, ...props }: React.ComponentProps<"input">) {

  return (

    <input

      type={type}

      data-slot="input"

      className={cn(

        "flex h-10 w-full rounded-base border-2 border-border bg-secondary-background selection:bg-main selection:text-main-foreground px-3 py-2 text-sm font-base text-foreground file:border-0 file:bg-transparent file:text-sm file:font-heading placeholder:text-foreground/50 focus-visible:outline-hidden focus-visible:ring-2 focus-visible:ring-black focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50",

        className,

      )}

      {...props}

    />

  )

}



export { Input }
```

---

### Label <a name="label"></a>

Renders an accessible label associated with controls.

- **Registry Key**: `label`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/label.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-label`

#### How to Use

```tsx
<div>
  <div className="flex items-center space-x-2">
    <Checkbox id="terms" />
    <Label htmlFor="terms">Accept terms and conditions</Label>
  </div>
</div>
```

#### Component Source Code

##### File: `src/components/ui/label.tsx`

```tsx
"use client"



import * as LabelPrimitive from "@radix-ui/react-label"



import * as React from "react"



import { cn } from "@/lib/utils"



function Label({

  className,

  ...props

}: React.ComponentProps<typeof LabelPrimitive.Root>) {

  return (

    <LabelPrimitive.Root

      data-slot="label"

      className={cn(

        "text-sm font-heading leading-none peer-disabled:cursor-not-allowed peer-disabled:opacity-70",

        className,

      )}

      {...props}

    />

  )

}



export { Label }
```

---

### Marquee <a name="marquee"></a>

A scrolling text component that animates horizontally.

- **Registry Key**: `marquee`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/marquee.json
```

#### How to Use

```tsx
<Marquee items={items} />
```

#### Component Source Code

##### File: `src/components/ui/marquee.tsx`

```tsx
export default function Marquee({ items }: { items: string[] }) {

  return (

    <div className="relative flex w-full overflow-x-hidden border-b-2 border-t-2 border-border bg-secondary-background text-foreground font-base">

      <div className="animate-marquee whitespace-nowrap py-12">

        {items.map((item) => {

          return (

            <span key={item} className="mx-4 text-4xl">

              {item}

            </span>

          )

        })}

      </div>



      <div className="absolute top-0 animate-marquee2 whitespace-nowrap py-12">

        {items.map((item) => {

          return (

            <span key={item} className="mx-4 text-4xl">

              {item}

            </span>

          )

        })}

      </div>



      {/* must have both of these in order to work */}

    </div>

  )

}
```

---

### Menubar <a name="menubar"></a>

A horizontal navigation bar with clickable menu items.

- **Registry Key**: `menubar`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/menubar.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-menubar`

#### How to Use

```tsx
<Menubar>
  <MenubarMenu>
    <MenubarTrigger>File</MenubarTrigger>
    <MenubarContent>
      <MenubarItem>
        New Tab <MenubarShortcut>⌘T</MenubarShortcut>
      </MenubarItem>
      <MenubarItem>
        New Window <MenubarShortcut>⌘N</MenubarShortcut>
      </MenubarItem>
      <MenubarItem disabled>New Incognito Window</MenubarItem>
      <MenubarSeparator />
      <MenubarSub>
        <MenubarSubTrigger>Share</MenubarSubTrigger>
        <MenubarSubContent>
          <MenubarItem>Email link</MenubarItem>
          <MenubarItem>Messages</MenubarItem>
          <MenubarItem>Notes</MenubarItem>
        </MenubarSubContent>
      </MenubarSub>
      <MenubarSeparator />
      <MenubarItem>
        Print... <MenubarShortcut>⌘P</MenubarShortcut>
      </MenubarItem>
    </MenubarContent>
  </MenubarMenu>
  <MenubarMenu>
    <MenubarTrigger>Edit</MenubarTrigger>
    <MenubarContent>
      <MenubarItem>
        Undo <MenubarShortcut>⌘Z</MenubarShortcut>
      </MenubarItem>
      <MenubarItem>
        Redo <MenubarShortcut>⇧⌘Z</MenubarShortcut>
      </MenubarItem>
      <MenubarSeparator />
      <MenubarSub>
        <MenubarSubTrigger>Find</MenubarSubTrigger>
        <MenubarSubContent>
          <MenubarItem>Search the web</MenubarItem>
          <MenubarSeparator />
          <MenubarItem>Find...</MenubarItem>
          <MenubarItem>Find Next</MenubarItem>
          <MenubarItem>Find Previous</MenubarItem>
        </MenubarSubContent>
      </MenubarSub>
      <MenubarSeparator />
      <MenubarItem>Cut</MenubarItem>
      <MenubarItem>Copy</MenubarItem>
      <MenubarItem>Paste</MenubarItem>
    </MenubarContent>
  </MenubarMenu>
  <MenubarMenu>
    <MenubarTrigger>View</MenubarTrigger>
    <MenubarContent>
      <MenubarCheckboxItem>Always Show Bookmarks Bar</MenubarCheckboxItem>
      <MenubarCheckboxItem checked>Always Show Full URLs</MenubarCheckboxItem>
      <MenubarSeparator />
      <MenubarItem inset>
        Reload <MenubarShortcut>⌘R</MenubarShortcut>
      </MenubarItem>
      <MenubarItem disabled inset>
        Force Reload <MenubarShortcut>⇧⌘R</MenubarShortcut>
      </MenubarItem>
      <MenubarSeparator />
      <MenubarItem inset>Toggle Fullscreen</MenubarItem>
      <MenubarSeparator />
      <MenubarItem inset>Hide Sidebar</MenubarItem>
    </MenubarContent>
  </MenubarMenu>
  <MenubarMenu>
    <MenubarTrigger>Profiles</MenubarTrigger>
    <MenubarContent>
      <MenubarRadioGroup value="benoit">
        <MenubarRadioItem value="andy">Andy</MenubarRadioItem>
        <MenubarRadioItem value="benoit">Benoit</MenubarRadioItem>
        <MenubarRadioItem value="Luis">Luis</MenubarRadioItem>
      </MenubarRadioGroup>
      <MenubarSeparator />
      <MenubarItem inset>Edit...</MenubarItem>
      <MenubarSeparator />
      <MenubarItem inset>Add Profile...</MenubarItem>
    </MenubarContent>
  </MenubarMenu>
</Menubar>
```

#### Component Source Code

##### File: `src/components/ui/menubar.tsx`

```tsx
"use client"



import * as MenubarPrimitive from "@radix-ui/react-menubar"

import { Check, ChevronRight, Circle } from "lucide-react"



import * as React from "react"



import { cn } from "@/lib/utils"



function Menubar({

  className,

  ...props

}: React.ComponentProps<typeof MenubarPrimitive.Root>) {

  return (

    <MenubarPrimitive.Root

      data-slot="menubar"

      className={cn(

        "flex h-11 items-center space-x-1 rounded-base border-2 border-border bg-main p-1 font-base",

        className,

      )}

      {...props}

    />

  )

}



function MenubarMenu({

  ...props

}: React.ComponentProps<typeof MenubarPrimitive.Menu>) {

  return <MenubarPrimitive.Menu data-slot="menubar-menu" {...props} />

}



function MenubarTrigger({

  className,

  ...props

}: React.ComponentProps<typeof MenubarPrimitive.Trigger>) {

  return (

    <MenubarPrimitive.Trigger

      data-slot="menubar-trigger"

      className={cn(

        "flex cursor-default select-none items-center text-main-foreground rounded-base px-3 py-1.5 text-sm border-2 border-transparent font-heading outline-none focus:border-border data-[state=open]:border-border",

        className,

      )}

      {...props}

    />

  )

}



function MenubarContent({

  className,

  align = "start",

  alignOffset = -4,

  sideOffset = 8,

  ...props

}: React.ComponentProps<typeof MenubarPrimitive.Content>) {

  return (

    <MenubarPrimitive.Portal>

      <MenubarPrimitive.Content

        data-slot="menubar-content"

        align={align}

        alignOffset={alignOffset}

        sideOffset={sideOffset}

        className={cn(

          "z-50 min-w-[12rem] overflow-hidden rounded-base border-2 border-border bg-main p-1 text-main-foreground data-[state=open]:animate-in data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 origin-(--radix-menubar-content-transform-origin)",

          className,

        )}

        {...props}

      />

    </MenubarPrimitive.Portal>

  )

}



function MenubarGroup({

  ...props

}: React.ComponentProps<typeof MenubarPrimitive.Group>) {

  return <MenubarPrimitive.Group data-slot="menubar-group" {...props} />

}



function MenubarPortal({

  ...props

}: React.ComponentProps<typeof MenubarPrimitive.Portal>) {

  return <MenubarPrimitive.Portal data-slot="menubar-portal" {...props} />

}



function MenubarSub({

  ...props

}: React.ComponentProps<typeof MenubarPrimitive.Sub>) {

  return <MenubarPrimitive.Sub data-slot="menubar-sub" {...props} />

}



function MenubarRadioGroup({

  ...props

}: React.ComponentProps<typeof MenubarPrimitive.RadioGroup>) {

  return (

    <MenubarPrimitive.RadioGroup data-slot="menubar-radio-group" {...props} />

  )

}



function MenubarItem({

  className,

  inset,

  ...props

}: React.ComponentPropsWithoutRef<typeof MenubarPrimitive.Item> & {

  inset?: boolean

}) {

  return (

    <MenubarPrimitive.Item

      data-slot="menubar-item"

      data-inset={inset}

      className={cn(

        "relative flex cursor-default select-none items-center rounded-base border-2 border-transparent px-2 py-1.5 text-sm font-base outline-hidden focus:border-border data-disabled:pointer-events-none data-disabled:opacity-50 data-[inset]:pl-8 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",

        className,

      )}

      {...props}

    />

  )

}



function MenubarCheckboxItem({

  className,

  children,

  checked,

  ...props

}: React.ComponentPropsWithoutRef<typeof MenubarPrimitive.CheckboxItem>) {

  return (

    <MenubarPrimitive.CheckboxItem

      data-slot="menubar-checkbox-item"

      className={cn(

        "relative flex cursor-default select-none items-center rounded-base border-2 border-transparent py-1.5 pl-8 pr-2 text-sm font-base outline-hidden focus:border-border data-disabled:pointer-events-none data-disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",

        className,

      )}

      checked={checked}

      {...props}

    >

      <span className="pointer-events-none absolute left-2 flex size-3.5 items-center justify-center">

        <MenubarPrimitive.ItemIndicator>

          <Check className="size-4" />

        </MenubarPrimitive.ItemIndicator>

      </span>

      {children}

    </MenubarPrimitive.CheckboxItem>

  )

}



function MenubarRadioItem({

  className,

  children,

  ...props

}: React.ComponentPropsWithoutRef<typeof MenubarPrimitive.RadioItem>) {

  return (

    <MenubarPrimitive.RadioItem

      data-slot="menubar-radio-item"

      className={cn(

        "relative flex cursor-default select-none items-center rounded-base border-2 border-transparent py-1.5 pl-8 pr-2 text-sm font-base outline-hidden focus:border-border data-disabled:pointer-events-none data-disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",

        className,

      )}

      {...props}

    >

      <span className="pointer-events-none absolute left-2 flex size-3.5 items-center justify-center">

        <MenubarPrimitive.ItemIndicator>

          <Circle className="size-2 fill-current" />

        </MenubarPrimitive.ItemIndicator>

      </span>

      {children}

    </MenubarPrimitive.RadioItem>

  )

}



function MenubarLabel({

  className,

  inset,

  ...props

}: React.ComponentPropsWithoutRef<typeof MenubarPrimitive.Label> & {

  inset?: boolean

}) {

  return (

    <MenubarPrimitive.Label

      data-slot="menubar-label"

      data-inset={inset}

      className={cn(

        "px-2 py-1.5 text-sm font-heading data-[inset]:pl-8",

        className,

      )}

      {...props}

    />

  )

}



function MenubarSeparator({

  className,

  ...props

}: React.ComponentPropsWithoutRef<typeof MenubarPrimitive.Separator>) {

  return (

    <MenubarPrimitive.Separator

      data-slot="menubar-separator"

      className={cn("-mx-1 my-1 h-0.5 bg-border", className)}

      {...props}

    />

  )

}



function MenubarShortcut({

  className,

  ...props

}: React.HTMLAttributes<HTMLSpanElement>) {

  return (

    <span

      data-slot="menubar-shortcut"

      className={cn(

        "ml-auto text-xs tracking-widest text-main-foreground",

        className,

      )}

      {...props}

    />

  )

}



function MenubarSubTrigger({

  className,

  inset,

  children,

  ...props

}: React.ComponentPropsWithoutRef<typeof MenubarPrimitive.SubTrigger> & {

  inset?: boolean

}) {

  return (

    <MenubarPrimitive.SubTrigger

      data-slot="menubar-sub-trigger"

      className={cn(

        "flex cursor-default select-none items-center rounded-base border-2 border-transparent px-3 py-1.5 text-sm font-base outline-hidden focus:border-border data-[state=open]:border-border data-[inset]:pl-8",

        className,

      )}

      {...props}

    >

      {children}

      <ChevronRight className="ml-auto size-4" />

    </MenubarPrimitive.SubTrigger>

  )

}



function MenubarSubContent({

  className,

  ...props

}: React.ComponentPropsWithoutRef<typeof MenubarPrimitive.SubContent>) {

  return (

    <MenubarPrimitive.SubContent

      data-slot="menubar-sub-content"

      className={cn(

        "z-50 min-w-[8rem] overflow-hidden rounded-base border-2 border-border bg-main p-1 font-base text-main-foreground data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 origin-(--radix-menubar-content-transform-origin)",

        className,

      )}

      {...props}

    />

  )

}



export {

  Menubar,

  MenubarMenu,

  MenubarTrigger,

  MenubarContent,

  MenubarItem,

  MenubarSeparator,

  MenubarLabel,

  MenubarCheckboxItem,

  MenubarRadioGroup,

  MenubarRadioItem,

  MenubarPortal,

  MenubarSubContent,

  MenubarSubTrigger,

  MenubarGroup,

  MenubarSub,

  MenubarShortcut,

}
```

---

### Navigation Menu <a name="navigation-menu"></a>

A collection of links for site navigation.

- **Registry Key**: `navigation-menu`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/navigation-menu.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-navigation-menu`

#### How to Use

```tsx
"use client"

import * as React from "react"
import Link from "next/link"

import {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuItem,
  NavigationMenuLink,
  NavigationMenuList,
  NavigationMenuTrigger,
  navigationMenuTriggerStyle,
} from "@/components/ui/navigation-menu"

import { cn } from "@/lib/utils"

const components: { title: string; href: string; description: string }[] = [
  {
    title: "Alert Dialog",
    href: "https://ui.shadcn.com/docs/primitives/alert-dialog",
    description:
      "A modal dialog that interrupts the user with important content and expects a response.",
  },
  {
    title: "Hover Card",
    href: "https://ui.shadcn.com/docs/primitives/hover-card",
    description:
      "For sighted users to preview content available behind a link.",
  },
  {
    title: "Progress",
    href: "https://ui.shadcn.com/docs/primitives/progress",
    description:
      "Displays an indicator showing the completion progress of a task, typically displayed as a progress bar.",
  },
  {
    title: "Scroll-area",
    href: "https://ui.shadcn.com/docs/primitives/scroll-area",
    description: "Visually or semantically separates content.",
  },
  {
    title: "Tabs",
    href: "https://ui.shadcn.com/docs/primitives/tabs",
    description:
      "A set of layered sections of content—known as tab panels—that are displayed one at a time.",
  },
  {
    title: "Tooltip",
    href: "https://ui.shadcn.com/docs/primitives/tooltip",
    description:
      "A popup that displays information related to an element when the element receives keyboard focus or the mouse hovers over it.",
  },
]

export default function NavigationMenuDemo() {
  return (
    <NavigationMenu className="z-5 ">
      <NavigationMenuList>
        <NavigationMenuItem>
          <NavigationMenuTrigger>Getting started</NavigationMenuTrigger>
          <NavigationMenuContent>
            <ul className="grid w-[500px] gap-3 p-2 lg:grid-cols-[.75fr_1fr]">
              <li className="row-span-3">
                <NavigationMenuLink asChild>
                  <a
                    className="flex h-full w-full select-none flex-col justify-end rounded-base p-6 no-underline outline-hidden"
                    href="https://ui.shadcn.com"
                  >
                    <div className="mb-2 mt-4 text-lg font-heading">
                      shadcn/ui
                    </div>
                    <p className="text-sm font-base leading-tight">
                      Beautifully designed components that you can copy and
                      paste into your apps. Accessible. Customizable. Open
                      Source.
                    </p>
                  </a>
                </NavigationMenuLink>
              </li>
              <ListItem href="https://ui.shadcn.com/docs" title="Introduction">
                Re-usable components built using Radix UI and Tailwind CSS.
              </ListItem>
              <ListItem
                href="https://ui.shadcn.com/docs/installation"
                title="Installation"
              >
                How to install dependencies and structure your app.
              </ListItem>
              <ListItem
                href="https://ui.shadcn.com/docs/primitives/typography"
                title="Typography"
              >
                Styles for headings, paragraphs, lists...etc
              </ListItem>
            </ul>
          </NavigationMenuContent>
        </NavigationMenuItem>
        <NavigationMenuItem>
          <NavigationMenuTrigger>Components</NavigationMenuTrigger>
          <NavigationMenuContent>
            <ul className="grid w-[400px] gap-3 p-2 md:w-[500px] md:grid-cols-2 lg:w-[600px] ">
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
          </NavigationMenuContent>
        </NavigationMenuItem>
        <NavigationMenuItem>
          <Link href="https://ui.shadcn.com/docs" legacyBehavior passHref>
            <NavigationMenuLink className={navigationMenuTriggerStyle()}>
              Documentation
            </NavigationMenuLink>
          </Link>
        </NavigationMenuItem>
      </NavigationMenuList>
    </NavigationMenu>
  )
}

function ListItem({
  className,
  title,
  children,
  ...props
}: React.ComponentProps<"a">) {
  return (
    <li>
      <NavigationMenuLink asChild>
        <a
          className={cn(
            "hover:bg-accent block text-main-foreground select-none space-y-1 rounded-base border-2 border-transparent p-3 leading-none no-underline outline-hidden transition-colors hover:border-border",
            className,
          )}
          {...props}
        >
          <div className="text-base font-heading leading-none">{title}</div>
          <p className="font-base line-clamp-2 text-sm leading-snug">
            {children}
          </p>
        </a>
      </NavigationMenuLink>
    </li>
  )
}
ListItem.displayName = "ListItem"
```

#### Component Source Code

##### File: `src/components/ui/navigation-menu.tsx`

```tsx
"use client"



import * as NavigationMenuPrimitive from "@radix-ui/react-navigation-menu"

import { cva } from "class-variance-authority"

import { ChevronDown } from "lucide-react"



import * as React from "react"



import { cn } from "@/lib/utils"



function NavigationMenu({

  className,

  children,

  viewport = true,

  ...props

}: React.ComponentProps<typeof NavigationMenuPrimitive.Root> & {

  viewport?: boolean

}) {

  return (

    <NavigationMenuPrimitive.Root

      data-slot="navigation-menu"

      data-viewport={viewport}

      className={cn(

        "relative z-10 flex max-w-max rounded-base font-heading border-border border-2 p-1 bg-main flex-1 items-center justify-center",

        className,

      )}

      {...props}

    >

      {children}

      {viewport && <NavigationMenuViewport />}

    </NavigationMenuPrimitive.Root>

  )

}



function NavigationMenuList({

  className,

  ...props

}: React.ComponentProps<typeof NavigationMenuPrimitive.List>) {

  return (

    <NavigationMenuPrimitive.List

      data-slot="navigation-menu-list"

      className={cn(

        "group flex flex-1 list-none items-center font-heading justify-center space-x-1",

        className,

      )}

      {...props}

    />

  )

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

  )

}



const navigationMenuTriggerStyle = cva(

  "group inline-flex h-10 w-max items-center justify-center text-main-foreground rounded-base bg-main px-4 py-2 text-sm font-heading transition-colors focus:outline-none disabled:pointer-events-none disabled:opacity-50",

)



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

      <ChevronDown

        className="relative top-[1px] ml-2 size-4 font-heading transition duration-200 group-data-[state=open]:rotate-180"

        aria-hidden="true"

      />

    </NavigationMenuPrimitive.Trigger>

  )

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

        "group-data-[viewport=false]/navigation-menu:bg-main group-data-[viewport=false]/navigation-menu:text-main-foreground group-data-[viewport=false]/navigation-menu:data-[state=open]:animate-in group-data-[viewport=false]/navigation-menu:data-[state=closed]:animate-out group-data-[viewport=false]/navigation-menu:data-[state=closed]:zoom-out-95 group-data-[viewport=false]/navigation-menu:data-[state=open]:zoom-in-95 group-data-[viewport=false]/navigation-menu:data-[state=open]:fade-in-0 group-data-[viewport=false]/navigation-menu:data-[state=closed]:fade-out-0 group-data-[viewport=false]/navigation-menu:top-full group-data-[viewport=false]/navigation-menu:mt-1.5 group-data-[viewport=false]/navigation-menu:overflow-hidden group-data-[viewport=false]/navigation-menu:duration-200 **:data-[slot=navigation-menu-link]:focus:ring-0 **:data-[slot=navigation-menu-link]:focus:outline-none",

        className,

      )}

      {...props}

    />

  )

}



function NavigationMenuLink({

  className,

  ...props

}: React.ComponentProps<typeof NavigationMenuPrimitive.Link>) {

  return (

    <NavigationMenuPrimitive.Link

      data-slot="navigation-menu-link"

      className={cn(

        "block select-none space-y-1 rounded-base p-2 leading-none no-underline outline-none transition-colors focus-visible:ring-4 focus-visible:outline-1 [&_svg:not([class*='size-'])]:size-4",

        className,

      )}

      {...props}

    />

  )

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

          "origin-top-center relative mt-1.5 h-[var(--radix-navigation-menu-viewport-height)] w-full overflow-hidden rounded-base border-2 border-border bg-main text-main-foreground data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-90 md:w-[var(--radix-navigation-menu-viewport-width)]",

          className,

        )}

        {...props}

      />

    </div>

  )

}



function NavigationMenuIndicator({

  className,

  ...props

}: React.ComponentProps<typeof NavigationMenuPrimitive.Indicator>) {

  return (

    <NavigationMenuPrimitive.Indicator

      data-slot="navigation-menu-indicator"

      className={cn(

        "top-full z-[1] flex h-1.5 items-end font-heading justify-center overflow-hidden data-[state=visible]:animate-in data-[state=hidden]:animate-out data-[state=hidden]:fade-out data-[state=visible]:fade-in",

        className,

      )}

      {...props}

    >

      <div className="relative top-[60%] h-2 w-2 rotate-45 rounded-tl-sm bg-white" />

    </NavigationMenuPrimitive.Indicator>

  )

}



export {

  navigationMenuTriggerStyle,

  NavigationMenu,

  NavigationMenuList,

  NavigationMenuItem,

  NavigationMenuContent,

  NavigationMenuTrigger,

  NavigationMenuLink,

  NavigationMenuIndicator,

  NavigationMenuViewport,

}
```

---

### Pagination <a name="pagination"></a>

Displays page numbers and controls for navigating paginated data.

- **Registry Key**: `pagination`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/pagination.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `https://neobrutalism.dev/r/nbutton.json`

#### How to Use

```tsx
<Pagination>
  <PaginationContent>
    <PaginationItem>
      <PaginationPrevious href="#" />
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#">1</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#" isActive>
        2
      </PaginationLink>
    </PaginationItem>
    <div className="items-center md:flex hidden">
      <PaginationItem>
        <PaginationLink href="#">3</PaginationLink>
      </PaginationItem>
      <PaginationItem>
        <PaginationEllipsis />
      </PaginationItem>
    </div>
    <PaginationItem>
      <PaginationNext href="#" />
    </PaginationItem>
  </PaginationContent>
</Pagination>
```

#### Component Source Code

##### File: `src/components/ui/pagination.tsx`

```tsx
import { ChevronLeft, ChevronRight, MoreHorizontal } from "lucide-react"



import * as React from "react"



import { buttonVariants } from "@/components/ui/button"



import { cn } from "@/lib/utils"



function Pagination({ className, ...props }: React.ComponentProps<"nav">) {

  return (

    <nav

      data-slot="pagination"

      role="navigation"

      aria-label="pagination"

      className={cn("mx-auto flex w-full justify-center", className)}

      {...props}

    />

  )

}



function PaginationContent({

  className,

  ...props

}: React.ComponentProps<"ul">) {

  return (

    <ul

      data-slot="pagination-content"

      className={cn("flex flex-row items-center gap-1", className)}

      {...props}

    />

  )

}



function PaginationItem({ className, ...props }: React.ComponentProps<"li">) {

  return (

    <li data-slot="pagination-item" className={cn("", className)} {...props} />

  )

}



function PaginationLink({

  className,

  isActive,

  size = "icon",

  ...props

}: React.ComponentProps<"a"> & {

  isActive?: boolean

  size?: "default" | "sm" | "lg" | "icon"

}) {

  return (

    <a

      data-slot="pagination-link"

      aria-current={isActive ? "page" : undefined}

      className={cn(

        buttonVariants({

          variant: "noShadow",

          size,

        }),

        className,

        isActive && "bg-black text-white",

      )}

      {...props}

    />

  )

}



function PaginationPrevious({

  className,

  ...props

}: React.ComponentProps<typeof PaginationLink>) {

  return (

    <PaginationLink

      data-slot="pagination-previous"

      aria-label="Go to previous page"

      size="default"

      className={cn("gap-1 pl-2.5", className)}

      {...props}

    >

      <ChevronLeft className="size-4" />

      <span>Previous</span>

    </PaginationLink>

  )

}



function PaginationNext({

  className,

  ...props

}: React.ComponentProps<typeof PaginationLink>) {

  return (

    <PaginationLink

      data-slot="pagination-next"

      aria-label="Go to next page"

      size="default"

      className={cn("gap-1 pr-2.5", className)}

      {...props}

    >

      <span>Next</span>

      <ChevronRight className="size-4" />

    </PaginationLink>

  )

}



function PaginationEllipsis({

  className,

  ...props

}: React.ComponentProps<"span">) {

  return (

    <span

      data-slot="pagination-ellipsis"

      aria-hidden

      className={cn("flex size-9 items-center justify-center", className)}

      {...props}

    >

      <MoreHorizontal className="size-4" />

      <span className="sr-only">More pages</span>

    </span>

  )

}



export {

  Pagination,

  PaginationContent,

  PaginationEllipsis,

  PaginationItem,

  PaginationLink,

  PaginationNext,

  PaginationPrevious,

}
```

---

### Popover <a name="popover"></a>

Displays floating content in relation to a target element.

- **Registry Key**: `popover`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/popover.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-popover`

#### How to Use

```tsx
<Popover>
  <PopoverTrigger asChild>
    <Button example="noShadow">Open popover</Button>
  </PopoverTrigger>
  <PopoverContent className="w-80 text-main-foreground">
    <div className="grid gap-4">
      <div className="space-y-2">
        <h4 className="font-heading leading-none">Dimensions</h4>
        <p className="text-sm">Set the dimensions for the layer.</p>
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
```

#### Component Source Code

##### File: `src/components/ui/popover.tsx`

```tsx
"use client"



import * as PopoverPrimitive from "@radix-ui/react-popover"



import * as React from "react"



import { cn } from "@/lib/utils"



function Popover({

  ...props

}: React.ComponentProps<typeof PopoverPrimitive.Root>) {

  return <PopoverPrimitive.Root data-slot="popover" {...props} />

}



function PopoverTrigger({

  ...props

}: React.ComponentProps<typeof PopoverPrimitive.Trigger>) {

  return <PopoverPrimitive.Trigger data-slot="popover-trigger" {...props} />

}



function PopoverContent({

  className,

  align = "center",

  sideOffset = 4,

  ...props

}: React.ComponentProps<typeof PopoverPrimitive.Content>) {

  return (

    <PopoverPrimitive.Portal>

      <PopoverPrimitive.Content

        data-slot="popover-content"

        align={align}

        sideOffset={sideOffset}

        className={cn(

          "z-50 w-72 rounded-base border-2 border-border bg-main p-4 text-foreground outline-none data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 origin-(--radix-popover-content-transform-origin)",

          className,

        )}

        {...props}

      />

    </PopoverPrimitive.Portal>

  )

}



export { Popover, PopoverTrigger, PopoverContent }
```

---

### Progress <a name="progress"></a>

Displays an indicator showing the completion progress of a task.

- **Registry Key**: `progress`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/progress.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-progress`

#### How to Use

##### Example 1

```tsx
const [progress, setProgress] = React.useState(13);

React.useEffect(() => {
  const timer = setTimeout(() => setProgress(66), 500);
  return () => clearTimeout(timer);
}, [])
```

##### Example 2

```tsx
<Progress value={progress} className="w-[60%]" />
```

#### Component Source Code

##### File: `src/components/ui/progress.tsx`

```tsx
"use client"



import * as ProgressPrimitive from "@radix-ui/react-progress"



import * as React from "react"



import { cn } from "@/lib/utils"



function Progress({

  className,

  value,

  ...props

}: React.ComponentProps<typeof ProgressPrimitive.Root> & {

  value?: number

}) {

  return (

    <ProgressPrimitive.Root

      data-slot="progress"

      className={cn(

        "relative h-4 w-full overflow-hidden rounded-base border-2 border-border bg-secondary-background",

        className,

      )}

      {...props}

    >

      <ProgressPrimitive.Indicator

        data-slot="progress-indicator"

        className="h-full w-full flex-1 border-r-2 border-border bg-main transition-all"

        style={{ transform: `translateX(-${100 - (value || 0)}%)` }}

      />

    </ProgressPrimitive.Root>

  )

}



export { Progress }
```

---

### Radio Group <a name="radio-group"></a>

A set of checkable buttons where only one can be checked at a time.

- **Registry Key**: `radio-group`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/radio-group.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-radio-group`

#### How to Use

```tsx
<RadioGroup defaultValue="comfortable">
  <div className="flex items-center space-x-2">
    <RadioGroupItem value="default" id="r1" />
    <Label htmlFor="r1">Default</Label>
  </div>
  <div className="flex items-center space-x-2">
    <RadioGroupItem value="comfortable" id="r2" />
    <Label htmlFor="r2">Comfortable</Label>
  </div>
  <div className="flex items-center space-x-2">
    <RadioGroupItem value="compact" id="r3" />
    <Label htmlFor="r3">Compact</Label>
  </div>
</RadioGroup>
```

#### Component Source Code

##### File: `src/components/ui/radio-group.tsx`

```tsx
"use client"



import * as RadioGroupPrimitive from "@radix-ui/react-radio-group"

import { Circle } from "lucide-react"



import * as React from "react"



import { cn } from "@/lib/utils"



function RadioGroup({

  className,

  ...props

}: React.ComponentProps<typeof RadioGroupPrimitive.Root>) {

  return (

    <RadioGroupPrimitive.Root

      data-slot="radio-group"

      className={cn("grid gap-2", className)}

      {...props}

    />

  )

}



function RadioGroupItem({

  className,

  ...props

}: React.ComponentProps<typeof RadioGroupPrimitive.Item>) {

  return (

    <RadioGroupPrimitive.Item

      data-slot="radio-group-item"

      className={cn(

        "aspect-square size-4 rounded-full border-2 border-border text-black dark:text-white focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50",

        className,

      )}

      {...props}

    >

      <RadioGroupPrimitive.Indicator className="flex items-center justify-center">

        <Circle className="size-2 fill-current text-current" />

      </RadioGroupPrimitive.Indicator>

    </RadioGroupPrimitive.Item>

  )

}



export { RadioGroup, RadioGroupItem }
```

---

### Resizable <a name="resizable"></a>

Accessible resizable panel groups with keyboard support.

- **Registry Key**: `resizable`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/resizable.json
```

#### Dependencies

- **NPM Packages**:
  - `react-resizable-panels`

#### How to Use

```tsx
<ResizablePanelGroup
  direction="horizontal"
  className="rounded-base max-w-md border-2 border-border text-main-foreground shadow-shadow"
>
  <ResizablePanel defaultSize={50}>
    <div className="flex h-[200px] items-center justify-center bg-main p-6">
      <span className="font-base">One</span>
    </div>
  </ResizablePanel>
  <ResizableHandle />
  <ResizablePanel defaultSize={50}>
    <ResizablePanelGroup direction="vertical">
      <ResizablePanel defaultSize={25}>
        <div className="flex h-full items-center justify-center bg-main p-6">
          <span className="font-base">Two</span>
        </div>
      </ResizablePanel>
      <ResizableHandle />
      <ResizablePanel defaultSize={75}>
        <div className="flex h-full items-center justify-center bg-main p-6">
          <span className="font-base">Three</span>
        </div>
      </ResizablePanel>
    </ResizablePanelGroup>
  </ResizablePanel>
</ResizablePanelGroup>
```

#### Component Source Code

##### File: `src/components/ui/resizable.tsx`

```tsx
"use client"



import { GripVertical } from "lucide-react"

import * as ResizablePrimitive from "react-resizable-panels"



import * as React from "react"



import { cn } from "@/lib/utils"



function ResizablePanelGroup({

  className,

  ...props

}: React.ComponentProps<typeof ResizablePrimitive.PanelGroup>) {

  return (

    <ResizablePrimitive.PanelGroup

      data-slot="resizable-panel-group"

      className={cn(

        "flex h-full w-full font-base data-[panel-group-direction=vertical]:flex-col",

        className,

      )}

      {...props}

    />

  )

}



function ResizablePanel({

  className,

  ...props

}: React.ComponentProps<typeof ResizablePrimitive.Panel>) {

  return (

    <ResizablePrimitive.Panel

      data-slot="resizable-panel"

      className={cn(className)}

      {...props}

    />

  )

}



function ResizableHandle({

  withHandle,

  className,

  ...props

}: React.ComponentProps<typeof ResizablePrimitive.PanelResizeHandle> & {

  withHandle?: boolean

}) {

  return (

    <ResizablePrimitive.PanelResizeHandle

      data-slot="resizable-handle"

      className={cn(

        "relative flex w-0.5 items-center justify-center bg-border after:absolute after:inset-y-0 after:left-1/2 after:w-1 after:-translate-x-1/2 focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-black focus-visible:ring-offset-1 data-[panel-group-direction=vertical]:h-0.5 data-[panel-group-direction=vertical]:w-full data-[panel-group-direction=vertical]:after:left-0 data-[panel-group-direction=vertical]:after:h-1 data-[panel-group-direction=vertical]:after:w-full data-[panel-group-direction=vertical]:after:-translate-y-1/2 data-[panel-group-direction=vertical]:after:translate-x-0 [&[data-panel-group-direction=vertical]>div]:rotate-90",

        className,

      )}

      {...props}

    >

      {withHandle && (

        <div className="z-10 flex h-4 w-3 items-center justify-center rounded-base border bg-border">

          <GripVertical className="size-2.5" />

        </div>

      )}

    </ResizablePrimitive.PanelResizeHandle>

  )

}



export { ResizablePanelGroup, ResizablePanel, ResizableHandle }
```

---

### Scroll Area <a name="scroll-area"></a>

Augments native scroll functionality for custom, cross-browser styling.

- **Registry Key**: `scroll-area`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/scroll-area.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-scroll-area`

#### How to Use

```tsx
<ScrollArea className="rounded-base h-[200px] w-[350px] text-main-foreground border-2 border-border bg-main p-4 shadow-shadow">
  Jokester began sneaking into the castle in the middle of the night and
  leaving jokes all over the place: under the king&apos;s pillow, in his
  soup, even in the royal toilet. The king was furious, but he couldn&apos;t
  seem to stop Jokester. And then, one day, the people of the kingdom
  discovered that the jokes left by Jokester were so funny that they
  couldn&apos;t help but laugh. And once they started laughing, they
  couldn&apos;t stop.
</ScrollArea>
```

#### Component Source Code

##### File: `src/components/ui/scroll-area.tsx`

```tsx
"use client"



import * as ScrollAreaPrimitive from "@radix-ui/react-scroll-area"



import * as React from "react"



import { cn } from "@/lib/utils"



function ScrollArea({

  className,

  children,

  ...props

}: React.ComponentProps<typeof ScrollAreaPrimitive.Root>) {

  return (

    <ScrollAreaPrimitive.Root

      data-slot="scroll-area"

      className={cn("relative overflow-hidden", className)}

      {...props}

    >

      <ScrollAreaPrimitive.Viewport className="h-full w-full font-base">

        {children}

      </ScrollAreaPrimitive.Viewport>

      <ScrollBar />

      <ScrollAreaPrimitive.Corner />

    </ScrollAreaPrimitive.Root>

  )

}



function ScrollBar({

  className,

  orientation = "vertical",

  ...props

}: React.ComponentProps<typeof ScrollAreaPrimitive.ScrollAreaScrollbar>) {

  return (

    <ScrollAreaPrimitive.ScrollAreaScrollbar

      data-slot="scroll-area-scrollbar"

      orientation={orientation}

      className={cn(

        "flex touch-none select-none transition-colors",

        orientation === "vertical" &&

          "h-full w-2.5 border-l border-l-transparent p-[1px]",

        orientation === "horizontal" &&

          "h-2.5 flex-col border-t border-t-transparent p-[1px]",

        className,

      )}

      {...props}

    >

      <ScrollAreaPrimitive.ScrollAreaThumb className="relative flex-1 rounded-full bg-border" />

    </ScrollAreaPrimitive.ScrollAreaScrollbar>

  )

}



export { ScrollArea, ScrollBar }
```

---

### Select <a name="select"></a>

Displays a list of options for the user to pick from.

- **Registry Key**: `select`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/select.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-select`

#### How to Use

```tsx
<Select>
  <SelectTrigger className="w-[180px]">
    <SelectValue placeholder="Select a fruit" />
  </SelectTrigger>
  <SelectContent>
    <SelectGroup>
      <SelectLabel>Fruits</SelectLabel>
      <SelectItem value="apple">Apple</SelectItem>
      <SelectItem value="banana">Banana</SelectItem>
      <SelectItem value="blueberry">Blueberry</SelectItem>
      <SelectItem value="grapes">Grapes</SelectItem>
      <SelectItem value="pineapple">Pineapple</SelectItem>
    </SelectGroup>
  </SelectContent>
</Select>
```

#### Component Source Code

##### File: `src/components/ui/select.tsx`

```tsx
"use client"



import * as SelectPrimitive from "@radix-ui/react-select"

import { Check, ChevronDown, ChevronUp } from "lucide-react"



import * as React from "react"



import { cn } from "@/lib/utils"



function Select({

  ...props

}: React.ComponentProps<typeof SelectPrimitive.Root>) {

  return <SelectPrimitive.Root data-slot="select" {...props} />

}



function SelectGroup({

  ...props

}: React.ComponentProps<typeof SelectPrimitive.Group>) {

  return <SelectPrimitive.Group data-slot="select-group" {...props} />

}



function SelectValue({

  ...props

}: React.ComponentProps<typeof SelectPrimitive.Value>) {

  return <SelectPrimitive.Value data-slot="select-value" {...props} />

}



function SelectTrigger({

  className,

  children,

  ...props

}: React.ComponentProps<typeof SelectPrimitive.Trigger>) {

  return (

    <SelectPrimitive.Trigger

      data-slot="select-trigger"

      className={cn(

        "flex h-10 w-full items-center justify-between rounded-base border-2 border-border bg-main gap-2 px-3 py-2 text-sm font-base text-main-foreground ring-offset-white placeholder:text-foreground/50 focus-visible:outline-hidden focus-visible:ring-2 focus-visible:ring-black focus-visible:ring-offset-2 focus:outline-hidden focus:ring-2 focus:ring-black focus:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 [&>span]:line-clamp-1 *:data-[slot=select-value]:line-clamp-1 *:data-[slot=select-value]:flex *:data-[slot=select-value]:items-center *:data-[slot=select-value]:gap-2 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",

        className,

      )}

      {...props}

    >

      {children}

      <SelectPrimitive.Icon asChild>

        <ChevronDown className="size-4" />

      </SelectPrimitive.Icon>

    </SelectPrimitive.Trigger>

  )

}



function SelectScrollUpButton({

  className,

  ...props

}: React.ComponentProps<typeof SelectPrimitive.ScrollUpButton>) {

  return (

    <SelectPrimitive.ScrollUpButton

      data-slot="select-scroll-up"

      className={cn(

        "flex cursor-default text-main-foreground font-base items-center justify-center py-1",

        className,

      )}

      {...props}

    >

      <ChevronUp className="size-4" />

    </SelectPrimitive.ScrollUpButton>

  )

}



function SelectScrollDownButton({

  className,

  ...props

}: React.ComponentProps<typeof SelectPrimitive.ScrollDownButton>) {

  return (

    <SelectPrimitive.ScrollDownButton

      data-slot="select-scroll-down"

      className={cn(

        "flex cursor-default text-main-foreground font-base items-center justify-center py-1",

        className,

      )}

      {...props}

    >

      <ChevronDown className="size-4" />

    </SelectPrimitive.ScrollDownButton>

  )

}



function SelectContent({

  className,

  children,

  position = "popper",

  ...props

}: React.ComponentProps<typeof SelectPrimitive.Content>) {

  return (

    <SelectPrimitive.Portal>

      <SelectPrimitive.Content

        data-slot="select-content"

        className={cn(

          "relative z-50 max-h-96 min-w-[8rem] overflow-hidden rounded-base border-2 border-border bg-main text-main-foreground data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 origin-(--radix-select-content-transform-origin)",

          position === "popper" &&

            "data-[side=bottom]:translate-y-1 data-[side=left]:-translate-x-1 data-[side=right]:translate-x-1 data-[side=top]:-translate-y-1",

          className,

        )}

        position={position}

        {...props}

      >

        <SelectScrollUpButton />

        <SelectPrimitive.Viewport

          className={cn(

            "p-1",

            position === "popper" &&

              "h-[var(--radix-select-trigger-height)] w-full min-w-[var(--radix-select-trigger-width)]",

          )}

        >

          {children}

        </SelectPrimitive.Viewport>

        <SelectScrollDownButton />

      </SelectPrimitive.Content>

    </SelectPrimitive.Portal>

  )

}



function SelectLabel({

  className,

  ...props

}: React.ComponentProps<typeof SelectPrimitive.Label>) {

  return (

    <SelectPrimitive.Label

      data-slot="select-label"

      className={cn(

        "border-2 border-transparent py-1.5 pr-8 pl-2 text-sm font-base text-main-foreground/80",

        className,

      )}

      {...props}

    />

  )

}



function SelectItem({

  className,

  children,

  ...props

}: React.ComponentProps<typeof SelectPrimitive.Item>) {

  return (

    <SelectPrimitive.Item

      data-slot="select-item"

      className={cn(

        "relative flex w-full cursor-default select-none items-center gap-2 rounded-base py-1.5 pr-8 pl-2 text-sm border-2 border-transparent font-base outline-none focus:border-border data-[disabled]:pointer-events-none data-[disabled]:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 *:[span]:last:flex *:[span]:last:items-center *:[span]:last:gap-2",

        className,

      )}

      {...props}

    >

      <span className="absolute right-2 flex size-3.5 items-center justify-center">

        <SelectPrimitive.ItemIndicator>

          <Check className="size-4" />

        </SelectPrimitive.ItemIndicator>

      </span>

      <SelectPrimitive.ItemText>{children}</SelectPrimitive.ItemText>

    </SelectPrimitive.Item>

  )

}



function SelectSeparator({

  className,

  ...props

}: React.ComponentProps<typeof SelectPrimitive.Separator>) {

  return (

    <SelectPrimitive.Separator

      data-slot="select-separator"

      className={cn("-mx-1 my-1 h-px bg-border", className)}

      {...props}

    />

  )

}



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

### Sheet <a name="sheet"></a>

Extends the Dialog component to display content that complements the main content of the screen.

- **Registry Key**: `sheet`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/sheet.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-dialog`

#### How to Use

```tsx
<Sheet>
  <SheetTrigger asChild>
    <Button>Open</Button>
  </SheetTrigger>
  <SheetContent>
    <SheetHeader>
      <SheetTitle>Edit profile</SheetTitle>
      <SheetDescription>
        Make changes to your profile here. Click save when you&apos;re
        done.
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
        <Button variant="neutral">Close</Button>
      </SheetClose>
    </SheetFooter>
  </SheetContent>
</Sheet>
```

#### Component Source Code

##### File: `src/components/ui/sheet.tsx`

```tsx
"use client"



import * as SheetPrimitive from "@radix-ui/react-dialog"

import { X } from "lucide-react"



import * as React from "react"



import { cn } from "@/lib/utils"



function Sheet({ ...props }: React.ComponentProps<typeof SheetPrimitive.Root>) {

  return <SheetPrimitive.Root data-slot="sheet" {...props} />

}



function SheetTrigger({

  ...props

}: React.ComponentProps<typeof SheetPrimitive.Trigger>) {

  return <SheetPrimitive.Trigger data-slot="sheet-trigger" {...props} />

}



function SheetClose({

  ...props

}: React.ComponentProps<typeof SheetPrimitive.Close>) {

  return <SheetPrimitive.Close data-slot="sheet-close" {...props} />

}



function SheetPortal({

  ...props

}: React.ComponentProps<typeof SheetPrimitive.Portal>) {

  return <SheetPrimitive.Portal data-slot="sheet-portal" {...props} />

}



function SheetOverlay({

  className,

  ...props

}: React.ComponentProps<typeof SheetPrimitive.Overlay>) {

  return (

    <SheetPrimitive.Overlay

      data-slot="sheet-overlay"

      className={cn(

        "data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 fixed inset-0 z-50 bg-overlay",

        className,

      )}

      {...props}

    />

  )

}



function SheetContent({

  className,

  children,

  side = "right",

  ...props

}: React.ComponentProps<typeof SheetPrimitive.Content> & {

  side?: "top" | "bottom" | "left" | "right"

}) {

  return (

    <SheetPortal>

      <SheetOverlay />

      <SheetPrimitive.Content

        data-slot="sheet-content"

        className={cn(

          "bg-background data-[state=open]:animate-in data-[state=closed]:animate-out fixed z-50 flex flex-col gap-4 border-2 border-border transition ease-in-out data-[state=closed]:duration-300 data-[state=open]:duration-500",

          side === "right" &&

            "data-[state=closed]:slide-out-to-right data-[state=open]:slide-in-from-right inset-y-0 right-0 h-full w-3/4 border-l sm:max-w-sm",

          side === "left" &&

            "data-[state=closed]:slide-out-to-left data-[state=open]:slide-in-from-left inset-y-0 left-0 h-full w-3/4 border-r sm:max-w-sm",

          side === "top" &&

            "data-[state=closed]:slide-out-to-top data-[state=open]:slide-in-from-top inset-x-0 top-0 h-auto border-b",

          side === "bottom" &&

            "data-[state=closed]:slide-out-to-bottom data-[state=open]:slide-in-from-bottom inset-x-0 bottom-0 h-auto border-t",

          className,

        )}

        {...props}

      >

        {children}

        <SheetPrimitive.Close className="absolute right-4 top-4 rounded-base ring-offset-white focus:outline-none focus:ring-2 focus:ring-ring focus:ring-offset-2 disabled:pointer-events-none">

          <X className="h-4 w-4" />

          <span className="sr-only">Close</span>

        </SheetPrimitive.Close>

      </SheetPrimitive.Content>

    </SheetPortal>

  )

}



function SheetHeader({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="sheet-header"

      className={cn("flex flex-col gap-1.5 p-4", className)}

      {...props}

    />

  )

}



function SheetFooter({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="sheet-footer"

      className={cn("mt-auto flex flex-col gap-3 p-4", className)}

      {...props}

    />

  )

}



function SheetTitle({

  className,

  ...props

}: React.ComponentProps<typeof SheetPrimitive.Title>) {

  return (

    <SheetPrimitive.Title

      data-slot="sheet-title"

      className={cn("text-foreground font-heading", className)}

      {...props}

    />

  )

}



function SheetDescription({

  className,

  ...props

}: React.ComponentProps<typeof SheetPrimitive.Description>) {

  return (

    <SheetPrimitive.Description

      data-slot="sheet-description"

      className={cn("text-sm text-foreground font-base", className)}

      {...props}

    />

  )

}



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

A composable, themeable and customizable sidebar component.

- **Registry Key**: `sidebar`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/sidebar.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `https://neobrutalism.dev/r/nbutton.json`
  - `https://neobrutalism.dev/r/nsheet.json`
  - `https://neobrutalism.dev/r/ntooltip.json`
  - `https://neobrutalism.dev/r/ninput.json`
  - `https://neobrutalism.dev/r/nskeleton.json`

#### How to Use

```tsx
"use client"

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
} from "lucide-react"

import * as React from "react"

import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar"
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/components/ui/collapsible"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuShortcut,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu"
import {
  Sidebar,
  SidebarContent,
  SidebarFooter,
  SidebarGroup,
  SidebarGroupAction,
  SidebarGroupContent,
  SidebarGroupLabel,
  SidebarHeader,
  SidebarMenu,
  SidebarMenuAction,
  SidebarMenuButton,
  SidebarMenuItem,
  SidebarMenuSub,
  SidebarMenuSubButton,
  SidebarMenuSubItem,
  SidebarRail,
  useSidebar,
} from "@/components/ui/sidebar"

// This is sample data.
const data = {
  user: {
    name: "shadcn",
    email: "m@example.com",
    avatar: "/avatars/shadcn.jpg",
  },
  teams: [
    {
      name: "Acme Inc",
      logo: GalleryVerticalEnd,
      plan: "Enterprise",
    },
    {
      name: "Acme Corp.",
      logo: AudioWaveform,
      plan: "Startup",
    },
    {
      name: "Evil Corp.",
      logo: Command,
      plan: "Free",
    },
  ],
  navMain: [
    {
      title: "Playground",
      url: "#",
      icon: SquareTerminal,
      isActive: true,
      items: [
        {
          title: "History",
          url: "#",
        },
        {
          title: "Starred",
          url: "#",
        },
        {
          title: "Settings",
          url: "#",
        },
      ],
    },
    {
      title: "Models",
      url: "#",
      icon: Bot,
      items: [
        {
          title: "Genesis",
          url: "#",
        },
        {
          title: "Explorer",
          url: "#",
        },
        {
          title: "Quantum",
          url: "#",
        },
      ],
    },
    {
      title: "Documentation",
      url: "#",
      icon: BookOpen,
      items: [
        {
          title: "Introduction",
          url: "#",
        },
        {
          title: "Get Started",
          url: "#",
        },
        {
          title: "Tutorials",
          url: "#",
        },
        {
          title: "Changelog",
          url: "#",
        },
      ],
    },
    {
      title: "Settings",
      url: "#",
      icon: Settings2,
      items: [
        {
          title: "General",
          url: "#",
        },
        {
          title: "Team",
          url: "#",
        },
        {
          title: "Billing",
          url: "#",
        },
        {
          title: "Limits",
          url: "#",
        },
      ],
    },
  ],
  projects: [
    {
      name: "Design Engineering",
      url: "#",
      icon: Frame,
    },
    {
      name: "Sales & Marketing",
      url: "#",
      icon: PieChart,
    },
    {
      name: "Travel",
      url: "#",
      icon: Map,
    },
  ],
}

export function AppSidebar({ ...props }: React.ComponentProps<typeof Sidebar>) {
  const { isMobile } = useSidebar()
  const [activeTeam, setActiveTeam] = React.useState(data.teams[0])

  if (!activeTeam) {
    return null
  }

  return (
    <Sidebar collapsible="icon" {...props}>
      <SidebarHeader>
        <SidebarMenu>
          <SidebarMenuItem>
            <DropdownMenu>
              <DropdownMenuTrigger className="focus-visible:ring-0" asChild>
                <SidebarMenuButton
                  size="lg"
                  className="data-[state=open]:bg-main data-[state=open]:text-main-foreground data-[state=open]:outline-border data-[state=open]:outline-2"
                >
                  <div className="flex aspect-square size-8 items-center justify-center rounded-base">
                    <activeTeam.logo className="size-4" />
                  </div>
                  <div className="grid flex-1 text-left text-sm leading-tight">
                    <span className="truncate font-heading">
                      {activeTeam.name}
                    </span>
                    <span className="truncate text-xs">{activeTeam.plan}</span>
                  </div>
                  <ChevronsUpDown className="ml-auto" />
                </SidebarMenuButton>
              </DropdownMenuTrigger>
              <DropdownMenuContent
                className="w-[--radix-dropdown-menu-trigger-width] min-w-56 rounded-base"
                align="start"
                side={isMobile ? "bottom" : "right"}
                sideOffset={4}
              >
                <DropdownMenuLabel className="text-sm font-heading">
                  Teams
                </DropdownMenuLabel>
                {data.teams.map((team, index) => (
                  <DropdownMenuItem
                    key={team.name}
                    onClick={() => setActiveTeam(team)}
                    className="gap-2 p-1.5"
                  >
                    <div className="flex size-6 items-center justify-center">
                      <team.logo className="size-4 shrink-0" />
                    </div>
                    {team.name}
                    <DropdownMenuShortcut>⌘{index + 1}</DropdownMenuShortcut>
                  </DropdownMenuItem>
                ))}
                <DropdownMenuSeparator />
                <DropdownMenuItem className="gap-2 p-1.5">
                  <div className="flex size-6 items-center justify-center">
                    <Plus className="size-4" />
                  </div>
                  <div className="font-base">Add team</div>
                </DropdownMenuItem>
              </DropdownMenuContent>
            </DropdownMenu>
          </SidebarMenuItem>
        </SidebarMenu>
      </SidebarHeader>
      <SidebarContent>
        <SidebarGroup>
          <SidebarGroupLabel>Platform</SidebarGroupLabel>
          <SidebarMenu>
            {data.navMain.map((item) => (
              <Collapsible
                key={item.title}
                asChild
                defaultOpen={item.isActive}
                className="group/collapsible"
              >
                <SidebarMenuItem>
                  <CollapsibleTrigger asChild>
                    <SidebarMenuButton
                      className="data-[state=open]:bg-main data-[state=open]:outline-border data-[state=open]:text-main-foreground"
                      tooltip={item.title}
                    >
                      {item.icon && <item.icon />}
                      <span>{item.title}</span>
                      <ChevronRight className="ml-auto transition-transform duration-200 group-data-[state=open]/collapsible:rotate-90" />
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
        <SidebarGroup className="group-data-[collapsible=icon]:hidden">
          <SidebarGroupLabel>Projects</SidebarGroupLabel>
          <SidebarMenu>
            {data.projects.map((item) => (
              <SidebarMenuItem key={item.name}>
                <SidebarMenuButton asChild>
                  <a href={item.url}>
                    <item.icon />
                    <span>{item.name}</span>
                  </a>
                </SidebarMenuButton>
                <DropdownMenu>
                  <DropdownMenuTrigger asChild>
                    <SidebarMenuAction>
                      <MoreHorizontal className="group-hover/menu-item:text-main-foreground" />
                      <span className="sr-only">More</span>
                    </SidebarMenuAction>
                  </DropdownMenuTrigger>
                  <DropdownMenuContent
                    className="w-48"
                    side={isMobile ? "bottom" : "right"}
                    align={isMobile ? "end" : "start"}
                  >
                    <DropdownMenuItem>
                      <Folder />
                      <span>View Project</span>
                    </DropdownMenuItem>
                    <DropdownMenuItem>
                      <Forward />
                      <span>Share Project</span>
                    </DropdownMenuItem>
                    <DropdownMenuSeparator />
                    <DropdownMenuItem>
                      <Trash2 />
                      <span>Delete Project</span>
                    </DropdownMenuItem>
                  </DropdownMenuContent>
                </DropdownMenu>
              </SidebarMenuItem>
            ))}
            <SidebarMenuItem>
              <SidebarMenuButton>
                <MoreHorizontal />
                <span>More</span>
              </SidebarMenuButton>
            </SidebarMenuItem>
          </SidebarMenu>
        </SidebarGroup>
      </SidebarContent>
      <SidebarFooter>
        <SidebarMenu>
          <SidebarMenuItem>
            <DropdownMenu>
              <DropdownMenuTrigger asChild>
                <SidebarMenuButton
                  className="group-data-[state=collapsed]:hover:outline-0 group-data-[state=collapsed]:hover:bg-transparent overflow-visible"
                  size="lg"
                >
                  <Avatar className="h-8 w-8">
                    <AvatarImage
                      src="https://github.com/shadcn.png?size=40"
                      alt="CN"
                    />
                    <AvatarFallback>CN</AvatarFallback>
                  </Avatar>
                  <div className="grid flex-1 text-left text-sm leading-tight">
                    <span className="truncate font-heading">
                      {data.user.name}
                    </span>
                    <span className="truncate text-xs">{data.user.email}</span>
                  </div>
                  <ChevronsUpDown className="ml-auto size-4" />
                </SidebarMenuButton>
              </DropdownMenuTrigger>
              <DropdownMenuContent
                className="w-[--radix-dropdown-menu-trigger-width] min-w-56"
                side={isMobile ? "bottom" : "right"}
                align="end"
                sideOffset={4}
              >
                <DropdownMenuLabel className="p-0 font-base">
                  <div className="flex items-center gap-2 px-1 py-1.5 text-left text-sm">
                    <Avatar className="h-8 w-8">
                      <AvatarImage
                        src="https://github.com/shadcn.png?size=40"
                        alt="CN"
                      />
                      <AvatarFallback>CN</AvatarFallback>
                    </Avatar>
                    <div className="grid flex-1 text-left text-sm leading-tight">
                      <span className="truncate font-heading">
                        {data.user.name}
                      </span>
                      <span className="truncate text-xs">
                        {data.user.email}
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
      </SidebarFooter>
      <SidebarRail />
    </Sidebar>
  )
}
```

#### Component Source Code

##### File: `src/components/ui/sidebar.tsx`

```tsx
"use client"



import { useIsMobile } from "@/hooks/use-mobile"

import { Slot } from "@radix-ui/react-slot"

import { cva, VariantProps } from "class-variance-authority"

import { PanelLeftIcon } from "lucide-react"



import * as React from "react"



import { Button } from "@/components/ui/button"

import { Input } from "@/components/ui/input"

import {

  Sheet,

  SheetContent,

  SheetDescription,

  SheetHeader,

  SheetTitle,

} from "@/components/ui/sheet"

import { Skeleton } from "@/components/ui/skeleton"

import {

  Tooltip,

  TooltipContent,

  TooltipProvider,

  TooltipTrigger,

} from "@/components/ui/tooltip"



import { cn } from "@/lib/utils"



const SIDEBAR_COOKIE_NAME = "sidebar_state"

const SIDEBAR_COOKIE_MAX_AGE = 60 * 60 * 24 * 7

const SIDEBAR_WIDTH = "16rem"

const SIDEBAR_WIDTH_MOBILE = "18rem"

const SIDEBAR_WIDTH_ICON = "3rem"

const SIDEBAR_KEYBOARD_SHORTCUT = "b"



type SidebarContextProps = {

  state: "expanded" | "collapsed"

  open: boolean

  setOpen: (open: boolean) => void

  openMobile: boolean

  setOpenMobile: (open: boolean) => void

  isMobile: boolean

  toggleSidebar: () => void

}



const SidebarContext = React.createContext<SidebarContextProps | null>(null)



function useSidebar() {

  const context = React.useContext(SidebarContext)

  if (!context) {

    throw new Error("useSidebar must be used within a SidebarProvider.")

  }



  return context

}



function SidebarProvider({

  defaultOpen = true,

  open: openProp,

  onOpenChange: setOpenProp,

  className,

  style,

  children,

  ...props

}: React.ComponentProps<"div"> & {

  defaultOpen?: boolean

  open?: boolean

  onOpenChange?: (open: boolean) => void

}) {

  const isMobile = useIsMobile()

  const [openMobile, setOpenMobile] = React.useState(false)



  // This is the internal state of the sidebar.

  // We use openProp and setOpenProp for control from outside the component.

  const [_open, _setOpen] = React.useState(defaultOpen)

  const open = openProp ?? _open

  const setOpen = React.useCallback(

    (value: boolean | ((value: boolean) => boolean)) => {

      const openState = typeof value === "function" ? value(open) : value

      if (setOpenProp) {

        setOpenProp(openState)

      } else {

        _setOpen(openState)

      }



      // This sets the cookie to keep the sidebar state.

      document.cookie = `${SIDEBAR_COOKIE_NAME}=${openState}; path=/; max-age=${SIDEBAR_COOKIE_MAX_AGE}`

    },

    [setOpenProp, open],

  )



  // Helper to toggle the sidebar.

  const toggleSidebar = React.useCallback(() => {

    return isMobile ? setOpenMobile((open) => !open) : setOpen((open) => !open)

  }, [isMobile, setOpen, setOpenMobile])



  // Adds a keyboard shortcut to toggle the sidebar.

  React.useEffect(() => {

    const handleKeyDown = (event: KeyboardEvent) => {

      if (

        event.key === SIDEBAR_KEYBOARD_SHORTCUT &&

        (event.metaKey || event.ctrlKey)

      ) {

        event.preventDefault()

        toggleSidebar()

      }

    }



    window.addEventListener("keydown", handleKeyDown)

    return () => window.removeEventListener("keydown", handleKeyDown)

  }, [toggleSidebar])



  // We add a state so that we can do data-state="expanded" or "collapsed".

  // This makes it easier to style the sidebar with Tailwind classes.

  const state = open ? "expanded" : "collapsed"



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

  )



  return (

    <SidebarContext.Provider value={contextValue}>

      <TooltipProvider delayDuration={0}>

        <div

          data-slot="sidebar-wrapper"

          style={

            {

              "--sidebar-width": SIDEBAR_WIDTH,

              "--sidebar-width-icon": SIDEBAR_WIDTH_ICON,

              ...style,

            } as React.CSSProperties

          }

          className={cn(

            "group/sidebar-wrapper has-data-[variant=inset]:bg-sidebar flex min-h-svh w-full",

            className,

          )}

          {...props}

        >

          {children}

        </div>

      </TooltipProvider>

    </SidebarContext.Provider>

  )

}



function Sidebar({

  side = "left",

  variant = "sidebar",

  collapsible = "offcanvas",

  className,

  children,

  ...props

}: React.ComponentProps<"div"> & {

  side?: "left" | "right"

  variant?: "sidebar" | "floating" | "inset"

  collapsible?: "offcanvas" | "icon" | "none"

}) {

  const { isMobile, state, openMobile, setOpenMobile } = useSidebar()



  if (collapsible === "none") {

    return (

      <div

        data-slot="sidebar"

        className={cn(

          "bg-secondary-background text-foreground flex h-full w-(--sidebar-width) flex-col",

          className,

        )}

        {...props}

      >

        {children}

      </div>

    )

  }



  if (isMobile) {

    return (

      <Sheet open={openMobile} onOpenChange={setOpenMobile} {...props}>

        <SheetContent

          data-sidebar="sidebar"

          data-slot="sidebar"

          data-mobile="true"

          className="bg-secondary-background text-foreground w-(--sidebar-width) p-0 [&>button]:hidden"

          style={

            {

              "--sidebar-width": SIDEBAR_WIDTH_MOBILE,

            } as React.CSSProperties

          }

          side={side}

        >

          <SheetHeader className="sr-only">

            <SheetTitle>Sidebar</SheetTitle>

            <SheetDescription>Displays the mobile sidebar.</SheetDescription>

          </SheetHeader>

          <div className="flex h-full w-full flex-col">{children}</div>

        </SheetContent>

      </Sheet>

    )

  }



  return (

    <div

      className="group peer hidden md:block"

      data-state={state}

      data-collapsible={state === "collapsed" ? collapsible : ""}

      data-variant={variant}

      data-side={side}

      data-slot="sidebar"

    >

      {/* This is what handles the sidebar gap on desktop */}

      <div

        data-slot="sidebar-gap"

        className={cn(

          "relative w-(--sidebar-width) bg-transparent transition-[width] duration-200 ease-linear",

          "group-data-[collapsible=offcanvas]:w-0",

          "group-data-[side=right]:rotate-180",

          variant === "floating" || variant === "inset"

            ? "group-data-[collapsible=icon]:w-[calc(var(--sidebar-width-icon)+(--spacing(4)))]"

            : "group-data-[collapsible=icon]:w-(--sidebar-width-icon)",

        )}

      />

      <div

        data-slot="sidebar-container"

        className={cn(

          "fixed inset-y-0 z-10 hidden h-svh w-(--sidebar-width) transition-[left,right,width] duration-200 ease-linear md:flex",

          side === "left"

            ? "left-0 group-data-[collapsible=offcanvas]:left-[calc(var(--sidebar-width)*-1)]"

            : "right-0 group-data-[collapsible=offcanvas]:right-[calc(var(--sidebar-width)*-1)]",

          // Adjust the padding for floating and inset variants.

          variant === "floating" || variant === "inset"

            ? "p-2 group-data-[collapsible=icon]:w-[calc(var(--sidebar-width-icon)+(--spacing(4))+2px)]"

            : "group-data-[collapsible=icon]:w-(--sidebar-width-icon) group-data-[side=left]:border-r-2 border-r-border group-data-[side=right]:border-l-2 border-l-border",

          className,

        )}

        {...props}

      >

        <div

          data-sidebar="sidebar"

          data-slot="sidebar-inner"

          className="bg-secondary-background flex h-full w-full flex-col"

        >

          {children}

        </div>

      </div>

    </div>

  )

}



function SidebarTrigger({

  className,

  onClick,

  ...props

}: React.ComponentProps<typeof Button>) {

  const { toggleSidebar } = useSidebar()



  return (

    <Button

      data-sidebar="trigger"

      data-slot="sidebar-trigger"

      variant="noShadow"

      size="icon"

      className={cn("size-7", className)}

      onClick={(event) => {

        onClick?.(event)

        toggleSidebar()

      }}

      {...props}

    >

      <PanelLeftIcon />

      <span className="sr-only">Toggle Sidebar</span>

    </Button>

  )

}



function SidebarRail({ className, ...props }: React.ComponentProps<"button">) {

  const { toggleSidebar } = useSidebar()



  return (

    <button

      data-sidebar="rail"

      data-slot="sidebar-rail"

      aria-label="Toggle Sidebar"

      tabIndex={-1}

      onClick={toggleSidebar}

      title="Toggle Sidebar"

      className={cn(

        "absolute inset-y-0 z-20 hidden w-4 -translate-x-1/2 transition-all ease-linear group-data-[side=left]:-right-4 group-data-[side=right]:left-0 after:absolute after:inset-y-0 after:left-1/2 after:w-[2px] sm:flex",

        "in-data-[side=left]:cursor-w-resize in-data-[side=right]:cursor-e-resize",

        "[[data-side=left][data-state=collapsed]_&]:cursor-e-resize [[data-side=right][data-state=collapsed]_&]:cursor-w-resize",

        "hover:group-data-[collapsible=offcanvas]:bg-sidebar group-data-[collapsible=offcanvas]:translate-x-0 group-data-[collapsible=offcanvas]:after:left-full",

        "[[data-side=left][data-collapsible=offcanvas]_&]:-right-2",

        "[[data-side=right][data-collapsible=offcanvas]_&]:-left-2",

        className,

      )}

      {...props}

    />

  )

}



function SidebarInset({ className, ...props }: React.ComponentProps<"main">) {

  return (

    <main

      data-slot="sidebar-inset"

      className={cn(

        "bg-secondary-background relative flex w-full flex-1 flex-col",

        "md:peer-data-[variant=inset]:m-2 md:peer-data-[variant=inset]:ml-0 md:peer-data-[variant=inset]:rounded-base md:peer-data-[variant=inset]:shadow-sm md:peer-data-[variant=inset]:peer-data-[state=collapsed]:ml-2",

        className,

      )}

      {...props}

    />

  )

}



function SidebarInput({

  className,

  ...props

}: React.ComponentProps<typeof Input>) {

  return (

    <Input

      data-slot="sidebar-input"

      data-sidebar="input"

      className={cn(

        "bg-secondary-background h-8 w-full shadow-none",

        className,

      )}

      {...props}

    />

  )

}



function SidebarHeader({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="sidebar-header"

      data-sidebar="header"

      className={cn(

        "flex flex-col gap-2 p-2 border-b-2 border-b-border",

        className,

      )}

      {...props}

    />

  )

}



function SidebarFooter({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="sidebar-footer"

      data-sidebar="footer"

      className={cn(

        "flex flex-col gap-2 p-2 border-t-2 border-t-border",

        className,

      )}

      {...props}

    />

  )

}



function SidebarContent({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="sidebar-content"

      data-sidebar="content"

      className={cn(

        "flex min-h-0 flex-1 flex-col overflow-auto group-data-[collapsible=icon]:overflow-hidden",

        className,

      )}

      {...props}

    />

  )

}



function SidebarGroup({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="sidebar-group"

      data-sidebar="group"

      className={cn(

        "relative flex w-full min-w-0 flex-col p-2 border-b-2 border-b-border last:border-b-0",

        className,

      )}

      {...props}

    />

  )

}



function SidebarGroupLabel({

  className,

  asChild = false,

  ...props

}: React.ComponentProps<"div"> & { asChild?: boolean }) {

  const Comp = asChild ? Slot : "div"



  return (

    <Comp

      data-slot="sidebar-group-label"

      data-sidebar="group-label"

      className={cn(

        "text-foreground ring-ring flex h-8 shrink-0 items-center rounded-base px-2 text-sm font-heading outline-hidden transition-[margin,opacity] duration-200 ease-linear focus-visible:ring-2 [&>svg]:size-4 [&>svg]:shrink-0",

        "group-data-[collapsible=icon]:-mt-8 group-data-[collapsible=icon]:opacity-0",

        className,

      )}

      {...props}

    />

  )

}



function SidebarGroupAction({

  className,

  asChild = false,

  ...props

}: React.ComponentProps<"button"> & { asChild?: boolean }) {

  const Comp = asChild ? Slot : "button"



  return (

    <Comp

      data-slot="sidebar-group-action"

      data-sidebar="group-action"

      className={cn(

        "absolute top-3.5 right-3 flex aspect-square w-5 items-center justify-center rounded-base p-0 outline-hidden transition-transform focus-visible:ring-2 [&>svg]:size-4 [&>svg]:shrink-0",

        // Increases the hit area of the button on mobile.

        "after:absolute after:-inset-2 md:after:hidden",

        "group-data-[collapsible=icon]:hidden",

        className,

      )}

      {...props}

    />

  )

}



function SidebarGroupContent({

  className,

  ...props

}: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="sidebar-group-content"

      data-sidebar="group-content"

      className={cn("w-full text-sm", className)}

      {...props}

    />

  )

}



function SidebarMenu({ className, ...props }: React.ComponentProps<"ul">) {

  return (

    <ul

      data-slot="sidebar-menu"

      data-sidebar="menu"

      className={cn("flex w-full min-w-0 flex-col gap-1", className)}

      {...props}

    />

  )

}



function SidebarMenuItem({ className, ...props }: React.ComponentProps<"li">) {

  return (

    <li

      data-slot="sidebar-menu-item"

      data-sidebar="menu-item"

      className={cn("group/menu-item relative font-base", className)}

      {...props}

    />

  )

}



const sidebarMenuButtonVariants = cva(

  "peer/menu-button flex w-full items-center gap-2 overflow-hidden outline-2 outline-transparent rounded-base p-2 text-left text-sm ring-ring transition-[width,height,padding] hover:bg-main hover:text-main-foreground hover:outline-border focus-visible:outline-border focus-visible:text-main-foreground focus-visible:bg-main disabled:pointer-events-none disabled:opacity-50 group-has-data-[sidebar=menu-action]/menu-item:pr-8 aria-disabled:pointer-events-none aria-disabled:opacity-50 group-data-[collapsible=icon]:size-8! group-data-[collapsible=icon]:p-2! [&>span:last-child]:truncate [&>svg]:size-4 [&>svg]:shrink-0",

  {

    variants: {

      size: {

        default: "h-8 text-sm",

        sm: "h-7 text-xs",

        lg: "h-12 text-sm group-data-[collapsible=icon]:p-0!",

      },

    },

    defaultVariants: {

      size: "default",

    },

  },

)



function SidebarMenuButton({

  asChild = false,

  isActive = false,

  size = "default",

  tooltip,

  className,

  ...props

}: React.ComponentProps<"button"> & {

  asChild?: boolean

  isActive?: boolean

  tooltip?: string | React.ComponentProps<typeof TooltipContent>

} & VariantProps<typeof sidebarMenuButtonVariants>) {

  const Comp = asChild ? Slot : "button"

  const { isMobile, state } = useSidebar()



  const button = (

    <Comp

      data-slot="sidebar-menu-button"

      data-sidebar="menu-button"

      data-size={size}

      data-active={isActive}

      className={cn(sidebarMenuButtonVariants({ size }), className)}

      {...props}

    />

  )



  if (!tooltip) {

    return button

  }



  if (typeof tooltip === "string") {

    tooltip = {

      children: tooltip,

    }

  }



  return (

    <Tooltip>

      <TooltipTrigger asChild>{button}</TooltipTrigger>

      <TooltipContent

        side="right"

        align="center"

        hidden={state !== "collapsed" || isMobile}

        {...tooltip}

      />

    </Tooltip>

  )

}



function SidebarMenuAction({

  className,

  asChild = false,

  showOnHover = false,

  ...props

}: React.ComponentProps<"button"> & {

  asChild?: boolean

  showOnHover?: boolean

}) {

  const Comp = asChild ? Slot : "button"



  return (

    <Comp

      data-slot="sidebar-menu-action"

      data-sidebar="menu-action"

      className={cn(

        "[&_svg]:text-foreground hover:[&_svg]:text-main-foreground text-main-foreground hover:bg-main hover:outline-border outline-transparent outline-2 absolute top-1.5 right-1 flex aspect-square w-5 items-center justify-center rounded-base p-0 transition-transform focus-visible:ring-2 [&>svg]:size-4 [&>svg]:shrink-0",

        // Increases the hit area of the button on mobile.

        "after:absolute after:-inset-2 md:after:hidden",

        "peer-data-[size=sm]/menu-button:top-1",

        "peer-data-[size=default]/menu-button:top-1.5",

        "peer-data-[size=lg]/menu-button:top-2.5",

        "group-data-[collapsible=icon]:hidden",

        className,

      )}

      {...props}

    />

  )

}



function SidebarMenuBadge({

  className,

  ...props

}: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="sidebar-menu-badge"

      data-sidebar="menu-badge"

      className={cn(

        "text-foreground pointer-events-none absolute right-1 flex h-5 min-w-5 items-center justify-center rounded-base px-1 text-xs font-base tabular-nums select-none",

        "peer-hover/menu-button:text-main-foreground",

        "peer-data-[size=sm]/menu-button:top-1",

        "peer-data-[size=default]/menu-button:top-1.5",

        "peer-data-[size=lg]/menu-button:top-2.5",

        "group-data-[collapsible=icon]:hidden",

        className,

      )}

      {...props}

    />

  )

}



function SidebarMenuSkeleton({

  className,

  showIcon = false,

  ...props

}: React.ComponentProps<"div"> & {

  showIcon?: boolean

}) {

  // Random width between 50 to 90%.

  const width = React.useMemo(() => {

    return `${Math.floor(Math.random() * 40) + 50}%`

  }, [])



  return (

    <div

      data-slot="sidebar-menu-skeleton"

      data-sidebar="menu-skeleton"

      className={cn("flex h-8 items-center gap-2 rounded-base px-2", className)}

      {...props}

    >

      {showIcon && (

        <Skeleton

          className="size-4 rounded-base"

          data-sidebar="menu-skeleton-icon"

        />

      )}

      <Skeleton

        className="h-4 max-w-(--skeleton-width) flex-1"

        data-sidebar="menu-skeleton-text"

        style={

          {

            "--skeleton-width": width,

          } as React.CSSProperties

        }

      />

    </div>

  )

}



function SidebarMenuSub({ className, ...props }: React.ComponentProps<"ul">) {

  return (

    <ul

      data-slot="sidebar-menu-sub"

      data-sidebar="menu-sub"

      className={cn(

        "border-l-foreground/50 mx-3.5 flex min-w-0 translate-x-px flex-col gap-1 border-l-2 px-2.5 py-0.5",

        "group-data-[collapsible=icon]:hidden",

        className,

      )}

      {...props}

    />

  )

}



function SidebarMenuSubItem({

  className,

  ...props

}: React.ComponentProps<"li">) {

  return (

    <li

      data-slot="sidebar-menu-sub-item"

      data-sidebar="menu-sub-item"

      className={cn("group/menu-sub-item relative", className)}

      {...props}

    />

  )

}



function SidebarMenuSubButton({

  asChild = false,

  size = "md",

  isActive = false,

  className,

  ...props

}: React.ComponentProps<"a"> & {

  asChild?: boolean

  size?: "sm" | "md"

  isActive?: boolean

}) {

  const Comp = asChild ? Slot : "a"



  return (

    <Comp

      data-slot="sidebar-menu-sub-button"

      data-sidebar="menu-sub-button"

      data-size={size}

      data-active={isActive}

      className={cn(

        "text-foreground hover:bg-main hover:outline-border hover:text-main-foreground  active:bg-main outline-transparent outline-2 [&>svg]:text-main-foreground flex h-7 min-w-0 -translate-x-px items-center gap-2 overflow-hidden rounded-base px-2 focus-visible:ring-2 disabled:pointer-events-none disabled:opacity-50 aria-disabled:pointer-events-none aria-disabled:opacity-50 [&>span:last-child]:truncate [&>svg]:size-4 [&>svg]:shrink-0",

        "data-[active=true]:bg-main data-[active=true]:outline-border",

        size === "sm" && "text-xs",

        size === "md" && "text-sm",

        "group-data-[collapsible=icon]:hidden",

        className,

      )}

      {...props}

    />

  )

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

  SidebarTrigger,

  useSidebar,

}
```

##### File: `src/hooks/use-mobile.ts`

```tsx
import * as React from "react"



const MOBILE_BREAKPOINT = 768



export function useIsMobile() {

  const [isMobile, setIsMobile] = React.useState<boolean | undefined>(undefined)



  React.useEffect(() => {

    const mql = window.matchMedia(`(max-width: ${MOBILE_BREAKPOINT - 1}px)`)

    const onChange = () => {

      setIsMobile(window.innerWidth < MOBILE_BREAKPOINT)

    }

    mql.addEventListener("change", onChange)

    setIsMobile(window.innerWidth < MOBILE_BREAKPOINT)

    return () => mql.removeEventListener("change", onChange)

  }, [])



  return !!isMobile

}
```

---

### Skeleton <a name="skeleton"></a>

Use to show a placeholder while content is loading.

- **Registry Key**: `skeleton`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/skeleton.json
```

#### How to Use

```tsx
<div className="flex items-center space-x-4">
  <Skeleton className="h-12 w-12 rounded-full" />
  <div className="space-y-2">
    <Skeleton className="h-4 sm:w-[250px] w-[100px]" />
    <Skeleton className="h-4 sm:w-[200px] w-[100px]" />
  </div>
</div>
```

#### Component Source Code

##### File: `src/components/ui/skeleton.tsx`

```tsx
import { cn } from "@/lib/utils"



function Skeleton({ className, ...props }: React.ComponentProps<"div">) {

  return (

    <div

      data-slot="skeleton"

      className={cn(

        "animate-pulse rounded-base bg-secondary-background border-2 border-border",

        className,

      )}

      {...props}

    />

  )

}



export { Skeleton }
```

---

### Slider <a name="slider"></a>

An input where the user selects a value from within a given range.

- **Registry Key**: `slider`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/slider.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-slider`

#### How to Use

```tsx
<Slider defaultValue={[33]} max={100} step={1} />
```

#### Component Source Code

##### File: `src/components/ui/slider.tsx`

```tsx
"use client"



import * as SliderPrimitive from "@radix-ui/react-slider"



import * as React from "react"



import { cn } from "@/lib/utils"



function Slider({

  className,

  defaultValue,

  value,

  min = 0,

  max = 100,

  ...props

}: React.ComponentProps<typeof SliderPrimitive.Root>) {

  const _values = React.useMemo(

    () =>

      Array.isArray(value)

        ? value

        : Array.isArray(defaultValue)

          ? defaultValue

          : [min, max],

    [value, defaultValue, min, max],

  )



  return (

    <SliderPrimitive.Root

      data-slot="slider"

      defaultValue={defaultValue}

      value={value}

      min={min}

      max={max}

      className={cn(

        "relative flex w-full touch-none select-none items-center data-[disabled]:opacity-50 data-[orientation=vertical]:h-full data-[orientation=vertical]:min-h-44 data-[orientation=vertical]:w-auto data-[orientation=vertical]:flex-col",

        className,

      )}

      {...props}

    >

      <SliderPrimitive.Track

        data-slot="slider-track"

        className="relative w-full grow overflow-hidden rounded-base bg-secondary-background border-2 border-border data-[orientation=horizontal]:h-3 data-[orientation=horizontal]:w-full data-[orientation=vertical]:h-full data-[orientation=vertical]:w-3"

      >

        <SliderPrimitive.Range

          data-slot="slider-range"

          className="absolute bg-main data-[orientation=horizontal]:h-full data-[orientation=vertical]:w-full"

        />

      </SliderPrimitive.Track>

      {Array.from({ length: _values.length }, (_, index) => (

        <SliderPrimitive.Thumb

          data-slot="slider-thumb"

          key={index}

          className="block h-5 w-5 rounded-full border-2 border-border bg-white ring-offset-white transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50"

        />

      ))}

    </SliderPrimitive.Root>

  )

}



export { Slider }
```

---

### Sonner <a name="sonner"></a>

An opinionated toast component for React.

- **Registry Key**: `sonner`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/sonner.json
```

#### Dependencies

- **NPM Packages**:
  - `sonner`
  - `next-themes`

#### How to Use

```tsx
<Button
  onClick={() =>
    toast("Event has been created", {
      description: "Sunday, December 03, 2023 at 9:00 AM",
      action: {
        label: "Undo",
        onClick: () => console.log("Undo"),
      },
    })
  }
>
  Show Toast
</Button>
```

#### Component Source Code

##### File: `src/components/ui/sonner.tsx`

```tsx
"use client"



import { useTheme } from "next-themes"

import { Toaster as Sonner, ToasterProps } from "sonner"



const Toaster = ({ ...props }: ToasterProps) => {

  const { theme = "system" } = useTheme()



  return (

    <Sonner

      theme={theme as ToasterProps["theme"]}

      style={{ fontFamily: "inherit", overflowWrap: "anywhere" }}

      toastOptions={{

        unstyled: true,

        classNames: {

          toast:

            "bg-background text-foreground border-border border-2 font-heading shadow-shadow rounded-base text-[13px] flex items-center gap-2.5 p-4 w-[356px] [&:has(button)]:justify-between",

          description: "font-base",

          actionButton:

            "font-base border-2 text-[12px] h-6 px-2 bg-main text-main-foreground border-border rounded-base shrink-0",

          cancelButton:

            "font-base border-2 text-[12px] h-6 px-2 bg-secondary-background text-foreground border-border rounded-base shrink-0",

          error: "bg-black text-white",

          loading:

            "[&[data-sonner-toast]_[data-icon]]:flex [&[data-sonner-toast]_[data-icon]]:size-4 [&[data-sonner-toast]_[data-icon]]:relative [&[data-sonner-toast]_[data-icon]]:justify-start [&[data-sonner-toast]_[data-icon]]:items-center [&[data-sonner-toast]_[data-icon]]:flex-shrink-0",

        },

      }}

      {...props}

    />

  )

}



export { Toaster }
```

---

### Switch <a name="switch"></a>

A toggle switch alternative to a checkbox.

- **Registry Key**: `switch`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/switch.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-switch`

#### How to Use

```tsx
<div className="flex items-center space-x-2">
  <Switch id="airplane-mode" />
  <Label htmlFor="airplane-mode">Airplane Mode</Label>
</div>
```

#### Component Source Code

##### File: `src/components/ui/switch.tsx`

```tsx
"use client"



import * as SwitchPrimitive from "@radix-ui/react-switch"



import * as React from "react"



import { cn } from "@/lib/utils"



function Switch({

  className,

  ...props

}: React.ComponentProps<typeof SwitchPrimitive.Root>) {

  return (

    <SwitchPrimitive.Root

      data-slot="switch"

      className={cn(

        "peer inline-flex h-6 w-12 shrink-0 cursor-pointer items-center rounded-full border-2 border-border bg-secondary-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 data-[state=checked]:bg-main data-[state=unchecked]:bg-secondary-background",

        className,

      )}

      {...props}

    >

      <SwitchPrimitive.Thumb

        data-slot="switch-thumb"

        className={cn(

          "pointer-events-none block h-4 w-4 rounded-full bg-white border-2 border-border ring-0 transition-transform data-[state=checked]:translate-x-6 data-[state=unchecked]:translate-x-1",

        )}

      />

    </SwitchPrimitive.Root>

  )

}



export { Switch }
```

---

### Table <a name="table"></a>

A responsive table component with sorting and filtering.

- **Registry Key**: `table`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/table.json
```

#### How to Use

```tsx
<Table>
  <TableCaption className="text-foreground">
    A list of your recent invoices.
  </TableCaption>
  <TableHeader>
    <TableRow>
      <TableHead className="w-[100px]">Invoice</TableHead>
      <TableHead>Status</TableHead>
      <TableHead>Method</TableHead>
      <TableHead className="text-right">Amount</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    {invoices.map((invoice) => (
      <TableRow key={invoice.invoice}>
        <TableCell className="font-base">{invoice.invoice}</TableCell>
        <TableCell>{invoice.paymentStatus}</TableCell>
        <TableCell>{invoice.paymentMethod}</TableCell>
        <TableCell className="text-right">{invoice.totalAmount}</TableCell>
      </TableRow>
    ))}
  </TableBody>
  <TableFooter>
    <TableRow>
      <TableCell colSpan={3}>Total</TableCell>
      <TableCell className="text-right">$2,500.00</TableCell>
    </TableRow>
  </TableFooter>
</Table>
```

#### Component Source Code

##### File: `src/components/ui/table.tsx`

```tsx
import * as React from "react"



import { cn } from "@/lib/utils"



function Table({ className, ...props }: React.ComponentProps<"table">) {

  return (

    <div className="relative w-full overflow-auto">

      <table

        data-slot="table"

        className={cn(

          "w-full caption-bottom border-2 border-border text-sm",

          className,

        )}

        {...props}

      />

    </div>

  )

}



function TableHeader({ className, ...props }: React.ComponentProps<"thead">) {

  return (

    <thead

      data-slot="table-header"

      className={cn("[&_tr]:border-b-2 [&_tr]:border-border", className)}

      {...props}

    />

  )

}



function TableBody({ className, ...props }: React.ComponentProps<"tbody">) {

  return (

    <tbody

      data-slot="table-body"

      className={cn("[&_tr:last-child]:border-0", className)}

      {...props}

    />

  )

}



function TableFooter({ className, ...props }: React.ComponentProps<"tfoot">) {

  return (

    <tfoot

      data-slot="table-footer"

      className={cn(

        "border-t-2 border-border bg-main font-base text-main-foreground last:[&>tr]:border-b-0",

        className,

      )}

      {...props}

    />

  )

}



function TableRow({ className, ...props }: React.ComponentProps<"tr">) {

  return (

    <tr

      data-slot="table-row"

      className={cn(

        "border-b-2 border-border transition-colors text-main-foreground bg-main font-base data-[state=selected]:bg-secondary-background data-[state=selected]:text-main-foreground",

        className,

      )}

      {...props}

    />

  )

}



function TableHead({ className, ...props }: React.ComponentProps<"th">) {

  return (

    <th

      data-slot="table-head"

      className={cn(

        "h-12 px-4 text-left align-middle font-heading text-main-foreground [&:has([role=checkbox])]:pr-0",

        className,

      )}

      {...props}

    />

  )

}



function TableCell({ className, ...props }: React.ComponentProps<"td">) {

  return (

    <td

      data-slot="table-cell"

      className={cn(

        "p-4 align-middle [&:has([role=checkbox])]:pr-0",

        className,

      )}

      {...props}

    />

  )

}



function TableCaption({

  className,

  ...props

}: React.ComponentProps<"caption">) {

  return (

    <caption

      data-slot="table-caption"

      className={cn("mt-4 text-sm text-foreground font-base", className)}

      {...props}

    />

  )

}



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

A set of layered sections of content that display one panel at a time.

- **Registry Key**: `tabs`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/tabs.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-tabs`

#### How to Use

```tsx
<Tabs defaultValue="account" className="max-w-[400px]">
  <TabsList className="grid w-full grid-cols-2">
    <TabsTrigger value="account">Account</TabsTrigger>
    <TabsTrigger value="password">Password</TabsTrigger>
  </TabsList>
  <TabsContent value="account">
    <Card>
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
        <div className="grid gap-3">
          <Label htmlFor="tabs-demo-username">Username</Label>
          <Input id="tabs-demo-username" defaultValue="@peduarte" />
        </div>
      </CardContent>
      <CardFooter>
        <Button className="w-full" variant="neutral">
          Save changes
        </Button>
      </CardFooter>
    </Card>
  </TabsContent>
  <TabsContent value="password">
    <Card>
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
        <Button className="w-full" variant="neutral">
          Save password
        </Button>
      </CardFooter>
    </Card>
  </TabsContent>
</Tabs>
```

#### Component Source Code

##### File: `src/components/ui/tabs.tsx`

```tsx
"use client"



import * as TabsPrimitive from "@radix-ui/react-tabs"



import * as React from "react"



import { cn } from "@/lib/utils"



function Tabs({

  className,

  ...props

}: React.ComponentProps<typeof TabsPrimitive.Root>) {

  return (

    <TabsPrimitive.Root

      data-slot="tabs"

      className={cn("w-full", className)}

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

      data-slot="tabs-list"

      className={cn(

        "inline-flex h-12 items-center justify-center rounded-base border-2 border-border bg-background p-1 text-foreground",

        className,

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

      data-slot="tabs-trigger"

      className={cn(

        "inline-flex items-center justify-center whitespace-nowrap rounded-base border-2 border-transparent px-2 py-1 gap-1.5 text-sm font-heading ring-offset-white transition-all focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50 data-[state=active]:bg-main data-[state=active]:text-main-foreground data-[state=active]:border-border",

        className,

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

      data-slot="tabs-content"

      className={cn(

        "mt-2 ring-offset-white focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2",

        className,

      )}

      {...props}

    />

  )

}



export { Tabs, TabsList, TabsTrigger, TabsContent }
```

---

### Textarea <a name="textarea"></a>

Displays a form textarea or a component that looks like a textarea.

- **Registry Key**: `textarea`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/textarea.json
```

#### How to Use

```tsx
<Textarea />
```

#### Component Source Code

##### File: `src/components/ui/textarea.tsx`

```tsx
import * as React from "react"



import { cn } from "@/lib/utils"



function Textarea({ className, ...props }: React.ComponentProps<"textarea">) {

  return (

    <textarea

      data-slot="textarea"

      className={cn(

        "flex min-h-[80px] w-full rounded-base border-2 border-border bg-secondary-background selection:bg-main selection:text-main-foreground px-3 py-2 text-sm font-base text-foreground placeholder:text-foreground/50 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-black focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50",

        className,

      )}

      {...props}

    />

  )

}



export { Textarea }
```

---

### Tooltip <a name="tooltip"></a>

A popup that displays information related to an element when it receives keyboard focus or mouse hover.

- **Registry Key**: `tooltip`

#### Installation

```bash
npx shadcn@latest add https://www.neobrutalism.dev/r/tooltip.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-tooltip`

#### How to Use

```tsx
<TooltipProvider>
  <Tooltip>
    <TooltipTrigger asChild>
      <Button example="noShadow">Hover</Button>
    </TooltipTrigger>
    <TooltipContent>
      <p>Add to library</p>
    </TooltipContent>
  </Tooltip>
</TooltipProvider>
```

#### Component Source Code

##### File: `src/components/ui/tooltip.tsx`

```tsx
"use client"



import * as TooltipPrimitive from "@radix-ui/react-tooltip"



import * as React from "react"



import { cn } from "@/lib/utils"



function TooltipProvider({

  delayDuration = 0,

  ...props

}: React.ComponentProps<typeof TooltipPrimitive.Provider>) {

  return (

    <TooltipPrimitive.Provider

      data-slot="tooltip-provider"

      delayDuration={delayDuration}

      {...props}

    />

  )

}



function Tooltip({

  ...props

}: React.ComponentProps<typeof TooltipPrimitive.Root>) {

  return <TooltipPrimitive.Root data-slot="tooltip" {...props} />

}



function TooltipTrigger({

  ...props

}: React.ComponentProps<typeof TooltipPrimitive.Trigger>) {

  return <TooltipPrimitive.Trigger data-slot="tooltip-trigger" {...props} />

}



function TooltipContent({

  className,

  sideOffset = 4,

  ...props

}: React.ComponentProps<typeof TooltipPrimitive.Content>) {

  return (

    <TooltipPrimitive.Content

      data-slot="tooltip-content"

      sideOffset={sideOffset}

      className={cn(

        "z-50 overflow-hidden rounded-base border-2 border-border bg-main px-3 py-1.5 text-sm font-base text-main-foreground animate-in fade-in-0 zoom-in-95 data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 origin-(--radix-tooltip-content-transform-origin)",

        className,

      )}

      {...props}

    />

  )

}



export { Tooltip, TooltipTrigger, TooltipContent, TooltipProvider }
```

---
