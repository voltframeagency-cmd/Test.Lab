# Ali Imam UI Creative Developer Reference Guide

A complete, structured reference guide for **Ali Imam UI** (aliimam.in) — a high-performance components and blocks library containing custom WebGL/Canvas shader backgrounds (`pixelgrid-shader`, `dithered-swirl`, `ripple-shader`), attraction forces, and 3D flipping book elements.

## Table of Contents

- [Blocks](#blocks)
  - [About 01](#about-01)
  - [Bento Grid 01](#bento-grid-01)
  - [Bento Grid 02](#bento-grid-02)
  - [Bento Grid 03](#bento-grid-03)
  - [Bento Grid 04](#bento-grid-04)
  - [Bento Grid 05](#bento-grid-05)
  - [Blogs 01](#blogs-01)
  - [Book Demo 01](#book-demo-01)
  - [Book Demo 02](#book-demo-02)
  - [Book Demo 03](#book-demo-03)
  - [Book Demo 04](#book-demo-04)
  - [Contact Us 01](#contact-us-01)
  - [Cta 01](#cta-01)
  - [Cta 02](#cta-02)
  - [Download 01](#download-01)
  - [Download 02](#download-02)
  - [Faq 01](#faq-01)
  - [Faq 02](#faq-02)
  - [Feature 01](#feature-01)
  - [Feature 02](#feature-02)
  - [Feature 03](#feature-03)
  - [Footer 01](#footer-01)
  - [Footer 02](#footer-02)
  - [Footer 03](#footer-03)
  - [Footer 04](#footer-04)
  - [Footer 05](#footer-05)
  - [Footer 06](#footer-06)
  - [Header 01](#header-01)
  - [Header 02](#header-02)
  - [Header 03](#header-03)
  - [Header 04](#header-04)
  - [Header 05](#header-05)
  - [Header 06](#header-06)
  - [Hero 01](#hero-01)
  - [Hero 02](#hero-02)
  - [Hero 03](#hero-03)
  - [Hero 04](#hero-04)
  - [Hero 05](#hero-05)
  - [Hero 06](#hero-06)
  - [Hero 07](#hero-07)
  - [Hero 08](#hero-08)
  - [Hero 09](#hero-09)
  - [Hero 10](#hero-10)
  - [Hero 11](#hero-11)
  - [Hero 12](#hero-12)
  - [Home 01](#home-01)
  - [Landing 01](#landing-01)
  - [Login 01](#login-01)
  - [Login 02](#login-02)
  - [Logos 01](#logos-01)
  - [Logos 02](#logos-02)
  - [Logos 03](#logos-03)
  - [Logos 04](#logos-04)
  - [Logos 05](#logos-05)
  - [Pricing 01](#pricing-01)
  - [Pricing 02](#pricing-02)
  - [Pricing 03](#pricing-03)
  - [Pricing 04](#pricing-04)
  - [Stats 01](#stats-01)
  - [Stats 02](#stats-02)
  - [Stats 03](#stats-03)
  - [Stats 04](#stats-04)
  - [Stats 05](#stats-05)
  - [Testimonials 01](#testimonials-01)
  - [Testimonials 02](#testimonials-02)
  - [Testimonials 03](#testimonials-03)
  - [Testimonials 04](#testimonials-04)
  - [Testimonials 05](#testimonials-05)
- [UI Components](#ui-components)
  - [Attraction](#attraction)
  - [Bento](#bento)
  - [Book](#book)
  - [Border Glow](#border-glow)
  - [Carousel](#carousel)
  - [Counter Number](#counter-number)
  - [Device](#device)
  - [Dithered Swirl](#dithered-swirl)
  - [Dot Pattern](#dot-pattern)
  - [Gauge](#gauge)
  - [Gradient Bars](#gradient-bars)
  - [Grid Pattern](#grid-pattern)
  - [Marquee](#marquee)
  - [PixelGrid Shader](#pixelgrid-shader)
  - [Render Canvas](#render-canvas)
  - [Ripple Shader](#ripple-shader)
  - [Scroll Progress](#scroll-progress)
  - [Shine Border](#shine-border)
  - [Typewriter](#typewriter)

---

## Blocks <a name="blocks"></a>

### About 01 <a name="about-01"></a>

A simple about page.

- **Registry Name**: `aliimam/about-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/about-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@paper-design/shaders-react`
- **Registry Dependencies**:
  - `button`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
##### File: `connect.tsx`

```tsx
import Link from "next/link"
import {
  SectionHeader,
  SectionHeaderDescription,
  SectionHeaderHeading,
} from "@/src/components/layout/page-header"
import {
  Figma,
  Github,
  Instagram,
  LinkedIn,
  Pinterest,
  Threads,
  X,
  YouTube,
} from "@aliimam/logos"

export function Connect() {
  return (
    <section className="container">
      <div className="flex justify-center border-x">
        <div className="max-w-5xl pb-20">
          <SectionHeader>
            <SectionHeaderHeading>Connect</SectionHeaderHeading>
            <SectionHeaderDescription>
              You can find me on everywhere with handle @aliimam.in Also see all
              social links in here
            </SectionHeaderDescription>
          </SectionHeader>
          <div className="[mask-image:radial-gradient(ellipse_100%_100%_at_50%_0%,#000_70%,transparent_100%)]">
            <div className="bg-background dark:bg-muted/50 grid gap-x-6 border px-6 pt-6 pb-10 md:grid-cols-2">
              <Integration
                icon={<X />}
                name="X"
                links="https://x.com/aliimam_in"
                description="Follow me for design insights, tech updates, and creative content."
              />
              <Integration
                icon={<LinkedIn />}
                name="LinkedIn"
                links="https://www.linkedin.com/in/aliimam-in/"
                description="Connect with me professionally and explore my career journey."
              />
              <Integration
                icon={<Instagram />}
                name="Instagram"
                links="https://www.instagram.com/aliimam.in/"
                description="Visual stories, behind-the-scenes, and creative inspiration."
              />
              <Integration
                icon={<Github />}
                name="Github"
                links="https://github.com/aliimam-in"
                description="Explore my open-source projects and code repositories."
              />
              <Integration
                icon={<YouTube />}
                name="Youtube"
                links="https://www.youtube.com/@aliimam_in"
                description="Watch tutorials, design processes, and creative content."
              />
              <Integration
                icon={<Figma />}
                name="Figma"
                links="https://www.figma.com/@aliimam_in"
                description="Check out my design files, UI kits, and design resources."
              />
              <Integration
                icon={<Threads />}
                name="Threads"
                links="https://www.threads.com/@aliimam.in"
                description="Join conversations about design, tech, and creativity."
              />
              <Integration
                icon={<Pinterest />}
                name="Pinterest"
                links="https://in.pinterest.com/aliimam_in/"
                description="Discover curated design inspiration and creative ideas."
              />
            </div>
          </div>
          <p className="text-muted-foreground md:text-md mx-auto max-w-lg text-center text-sm font-light">
            For partnerships, collaborations, sponsorships, commissions, events,
            you can reach out to me at{" "}
            <Link
              className="text-primary font-semibold hover:underline"
              href={""}
            >
              contact@aliimam.in
            </Link>
          </p>
        </div>
      </div>
    </section>
  )
}

const Integration = ({
  icon,
  name,
  links,
  description,
}: {
  icon: React.ReactNode
  name: string
  links: string
  description: string
}) => {
  return (
    <Link
      target="_blank"
      href={links}
      className="hover:bg-secondary grid grid-cols-[auto_1fr_auto] items-center gap-3 border-b border-dashed p-3 last:border-b-0"
    >
      <div className="bg-muted border-foreground/5 flex size-12 items-center justify-center border">
        {icon}
      </div>
      <div className="space-y-0.5">
        <h3 className="text-sm font-medium">{name}</h3>
        <p className="text-muted-foreground line-clamp-1 text-sm">
          {description}
        </p>
      </div>
    </Link>
  )
}
```
##### File: `create.tsx`

```tsx
import Link from "next/link"
import {
  SectionActions,
  SectionHeader,
  SectionHeaderDescription,
  SectionHeaderHeading,
} from "@/src/components/layout/page-header"

import { Button } from "@/registry/aliimam/ui/button"

export function Craete() {
  return (
    <section className="container">
      <div className="border-x border-b px-4 py-10">
        <SectionHeader>
          <SectionHeaderHeading>Let’s Create Something</SectionHeaderHeading>
          <SectionHeaderDescription>
            Ready to elevate your brand with bold, innovative design? Whether
            you need a stunning website, a cohesive brand identity, or engaging
            digital content, I’m here to make it happen. Let’s collaborate to
            turn your ideas into reality.
          </SectionHeaderDescription>
          <SectionActions>
            <Button asChild size={"xl"} variant={"outline"}>
              <Link target="_blank" download={true} href="/cv.pdf">
                Download CV
              </Link>
            </Button>
            <Button asChild size={"xl"}>
              <Link target="_blank" href="https://cal.com/aliimam/30min">
                Book a Call
              </Link>
            </Button>
          </SectionActions>
        </SectionHeader>
      </div>
    </section>
  )
}
```
##### File: `journey.tsx`

```tsx
import {
  SectionHeader,
  SectionHeaderDescription,
  SectionHeaderHeading,
} from "@/src/components/layout/page-header"

export function Journey() {
  return (
    <section className="relative container">
      <div className="border-x border-b pb-20">
        <SectionHeader>
          <SectionHeaderHeading>My Creative Journey</SectionHeaderHeading>
        </SectionHeader>
        <div className="flex flex-col items-center justify-center space-y-6">
          <SectionHeaderDescription className="text-center">
            My love for design began years ago, evolving through diverse roles
            across industries, from sales to animation, before finding my true
            calling in graphic design and development. Each step of my career
            has shaped my unique perspective, blending artistic vision with
            strategic thinking. Today, at Here, I channel this experience into
            creating digital and visual experiences that are both stunning and
            functional. My journey spans roles at renowned companies like
            IDCUBE, Steadfast Nutrition, Garage Productions, and Yellow Straw
            Advertising, where I honed my skills in UI/UX design, branding,
            motion graphics, and more. From crafting pixel-perfect interfaces to
            leading creative campaigns, I’ve built a reputation for delivering
            results that exceed expectations.
          </SectionHeaderDescription>

          <SectionHeaderDescription className="text-center">
            At Here, you work directly with me—founder, designer, and creative
            strategist. This means personalized attention, seamless
            collaboration, and a commitment to bringing your vision to life.
            I’ve been dedicated to providing a complete platform for design,
            offering end-to-end creative solutions tailored to your needs.
          </SectionHeaderDescription>

          <SectionHeaderDescription className="text-center">
            Currently, I work from my studio as an independent contractor,
            helping brands develop effective visuals and design systems. In my
            design studio, I spend most of my time building design tools and
            resources, with a focus on creativity and experience, while
            experimenting with new tools and trends.
          </SectionHeaderDescription>
        </div>
      </div>
    </section>
  )
}
```
##### File: `page-header.tsx`

```tsx
import { cn } from "@/src/lib/utils"

function PageHeader({
  className,
  children,
  ...props
}: React.ComponentProps<"section">) {
  return (
    <section className={cn("border-grid", className)} {...props}>
      <div className="container-wrapper">
        <div className="container flex flex-col items-center gap-2 py-8 text-center md:py-16 lg:py-20 xl:gap-4">
          {children}
        </div>
      </div>
    </section>
  )
}

function PageHeaderHeading({
  className,
  ...props
}: React.ComponentProps<"h1">) {
  return (
    <h1
      className={cn(
        "font-bold tracking-tighter uppercase text-6xl xl:text-8xl",
        className
      )}
      {...props}
    />
  )
}

function PageHeaderDescription({
  className,
  ...props
}: React.ComponentProps<"p">) {
  return (
    <p
      className={cn(
        "text-muted-foreground max-w-4xl text-sm xl:text-lg",
        className
      )}
      {...props}
    />
  )
}

function PageActions({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      className={cn(
        "flex w-full items-center justify-center gap-2 pt-2 **:data-[slot=button]:shadow-none",
        className
      )}
      {...props}
    />
  )
}

export { PageActions, PageHeader, PageHeaderDescription, PageHeaderHeading }

function SectionHeader({
  className,
  children,
  ...props
}: React.ComponentProps<"section">) {
  return (
    <section className={cn("border-grid", className)} {...props}>
      <div className="container-wrapper">
        <div className="container flex flex-col items-center gap-2 py-8 text-center md:py-16 lg:py-20 xl:gap-4">
          {children}
        </div>
      </div>
    </section>
  )
}

function SectionHeaderHeading({
  className,
  ...props
}: React.ComponentProps<"h1">) {
  return (
    <h1
      className={cn(
        "text-3xl font-medium tracking-tighter uppercase xl:text-5xl",
        className
      )}
      {...props}
    />
  )
}

function SectionHeaderDescription({
  className,
  ...props
}: React.ComponentProps<"p">) {
  return (
    <p
      className={cn(
        "text-muted-foreground xl:text-md max-w-3xl text-sm",
        className
      )}
      {...props}
    />
  )
}

function SectionActions({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      className={cn(
        "flex w-full items-center justify-center gap-2 pt-2 **:data-[slot=button]:shadow-none",
        className
      )}
      {...props}
    />
  )
}

export {
  SectionActions,
  SectionHeader,
  SectionHeaderDescription,
  SectionHeaderHeading,
}
```
##### File: `me.tsx`

```tsx
import Link from "next/link"
import { Apple, Vercel } from "@aliimam/logos"
import { Plus } from "lucide-react"

import {
  SectionHeader,
  SectionHeaderDescription,
  SectionHeaderHeading,
} from "@/registry/aliimam/pages/about/about-01/components/page-header"

export function Me() {
  return (
    <div className="relative border-b pb-20">
      <SectionHeader>
        <SectionHeaderHeading>About AI</SectionHeaderHeading>
        <SectionHeaderDescription>
          I’m a passionate in Design and Code based in Bokaro Steel City, India.
          Also, It’s not that hard to find my contact just search Ali Imam
          designer.
        </SectionHeaderDescription>
      </SectionHeader>
      <div>
        <h1 className="text-center">Founder</h1>
        <div className="container grid items-center justify-center lg:grid-cols-3">
          <h1 className="mt-6 text-center">
            I&apos;m Ali, Creative Design Engineer.
          </h1>

          <div className="border-primary/10 relative mx-auto my-6 flex h-[336px] max-w-[250px] flex-col items-start rounded-2xl border p-4 md:h-[28rem] md:max-w-sm">
            <Plus className="text-primary absolute -top-4 -left-4" />
            <Plus className="text-primary absolute -bottom-4 -left-4" />
            <Plus className="text-primary absolute -top-4 -right-4" />
            <Plus className="text-primary absolute -right-4 -bottom-4" />

            <img
              src="/ai.jpg"
              alt="Your Image"
              height={700}
              width={700}
              className="h-[304px] rounded-sm object-cover md:h-[354px]"
            />

            <div className="-mt-17 w-full rounded-b-md bg-gradient-to-b from-black/0 to-black text-white md:-mt-24">
              <h1 className="z-20 w-full items-center text-center text-[40px] font-black tracking-tighter uppercase md:text-[60px]">
                Ali Imam
              </h1>
            </div>
          </div>
          <h1 className="mt-6 text-center">Design That Gives.</h1>
        </div>
      </div>
      <div className="relative">
        <div className="flex w-full flex-wrap justify-center gap-3 p-3 pt-10">
          <Link
            href={"https://21st.dev/community/aliimam"}
            target="_blank"
            className="hover:bg-muted w-fit cursor-pointer rounded-sm border p-1"
          >
            <div className="flex items-center justify-center pl-3">
              <h1 className="text-xl font-semibold md:text-2xl">
                Build and deploy on the AI Cloud.
              </h1>
              <Vercel className="size-12 p-4 md:size-14" />
            </div>
          </Link>
          <Link
            href={"https://ui.shadcn.com/docs/directory"}
            target="_blank"
            className="hover:bg-muted w-fit cursor-pointer rounded-sm border p-1"
          >
            <div className="flex items-center justify-center pl-3">
              <h1 className="text-xl font-semibold md:text-2xl">
                Deisgn inspiration in
              </h1>
              <Apple className="size-12 p-4 md:size-14" />
            </div>
          </Link>
        </div>
      </div>
    </div>
  )
}
```
---

### Bento Grid 01 <a name="bento-grid-01"></a>

A simple bento section.

- **Registry Name**: `aliimam/bento-grid-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/bento-grid-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`
  - `recharts`
- **Registry Dependencies**:
  - `chart`
  - `checkbox`
  - `field`
  - `calendar`
  - `button`
  - `bento`
  - `dot-pattern`
  - `marquee`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `bento.tsx`

```tsx
"use client"

import Link from "next/link"

import { Marquee } from "@/registry/aliimam/components/marquee"

type BentoBlock = {
  name: string
  category: string
  href: string
  light: string
  dark: string
}

function ThemeImage({
  light,
  dark,
  alt,
}: {
  light: string
  dark: string
  alt: string
}) {
  return (
    <>
      <img
        src={light}
        alt={alt}
        className={`border-muted/10 hidden h-full w-full border object-contain dark:block`}
      />
      <img
        src={dark}
        alt={alt}
        className={`border-muted/20 block h-full w-full border object-contain dark:hidden`}
      />
    </>
  )
}

export function BentoLanding({ blocks = [] }: { blocks?: BentoBlock[] }) {
  if (!blocks.length) {
    return (
      <section className="text-muted-foreground container py-24 text-center">
        No blocks found
      </section>
    )
  }

  return (
    <section className="border-b">
      <div className="container">
        <div className="bg-foreground">
          <Marquee
            pauseOnHover
            reverse
            speed="slow"
            className="[mask-image:linear-gradient(to_right,transparent,black_8%,black_92%,transparent)] py-1"
          >
            <div className="flex gap-1">
              {blocks.map((block) => (
                <Link
                  key={block.name}
                  href={block.href}
                  className="relative h-40 shrink-0 overflow-hidden md:h-60"
                >
                  <ThemeImage
                    light={block.light}
                    dark={block.dark}
                    alt={block.name}
                  />
                </Link>
              ))}
            </div>
          </Marquee>
        </div>
      </div>
    </section>
  )
}
```
##### File: `empty-avatar.tsx`

```tsx
import { Plus } from "@aliimam/icons"

import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/registry/aliimam/ui/avatar"
import { Button } from "@/registry/aliimam/ui/button"
import {
  Empty,
  EmptyContent,
  EmptyDescription,
  EmptyHeader,
  EmptyMedia,
  EmptyTitle,
} from "@/registry/aliimam/ui/empty"

export function EmptyAvatarGroup() {
  return (
    <Empty>
      <EmptyHeader>
        <EmptyMedia>
          <div className="*:data-[slot=avatar]:ring-background flex -space-x-2 *:data-[slot=avatar]:size-12 *:data-[slot=avatar]:ring-2 *:data-[slot=avatar]:grayscale">
            <Avatar>
              <AvatarImage src="https://github.com/shadcn.png" alt="@shadcn" />
              <AvatarFallback>CN</AvatarFallback>
            </Avatar>
            <Avatar>
              <AvatarImage
                src="https://github.com/maxleiter.png"
                alt="@maxleiter"
              />
              <AvatarFallback>LR</AvatarFallback>
            </Avatar>
            <Avatar>
              <AvatarImage
                src="https://github.com/evilrabbit.png"
                alt="@evilrabbit"
              />
              <AvatarFallback>ER</AvatarFallback>
            </Avatar>
          </div>
        </EmptyMedia>
        <EmptyTitle>No Team Members</EmptyTitle>
        <EmptyDescription>Invite your team.</EmptyDescription>
      </EmptyHeader>
      <EmptyContent>
        <Button size="sm">
          <Plus />
          Invite Members
        </Button>
      </EmptyContent>
    </Empty>
  )
}
```
##### File: `hear.tsx`

```tsx
import { Checkbox } from "@/registry/aliimam/ui/checkbox"
import {
  Field,
  FieldDescription,
  FieldGroup,
  FieldLabel,
  FieldLegend,
  FieldSet,
  FieldTitle,
} from "@/registry/aliimam/ui/field"

const options = [
  {
    label: "Social Media",
    value: "social-media",
  },

  {
    label: "Search Engine",
    value: "search-engine",
  },
  {
    label: "Referral",
    value: "referral",
  },
  {
    label: "Other",
    value: "other",
  },
]

export function FieldHear() {
  return (
    <div className="w-full">
      <div className="">
        <form>
          <FieldGroup>
            <FieldSet className="gap-4">
              <FieldLegend>How did you hear about us?</FieldLegend>
              <FieldDescription className="line-clamp-1">
                Select the option that best describes how you heard about us.
              </FieldDescription>
              <FieldGroup className="flex flex-row flex-wrap gap-2 [--radius:9999rem]">
                {options.map((option) => (
                  <FieldLabel
                    htmlFor={option.value}
                    key={option.value}
                    className="!w-fit"
                  >
                    <Field
                      orientation="horizontal"
                      className="gap-1.5 overflow-hidden px-3! py-1.5! transition-all duration-100 ease-linear group-has-data-[state=checked]/field-label:px-2!"
                    >
                      <Checkbox
                        value={option.value}
                        id={option.value}
                        defaultChecked={option.value === "social-media"}
                        className="-ml-6 -translate-x-1 rounded-full transition-all duration-100 ease-linear data-[state=checked]:ml-0 data-[state=checked]:translate-x-0"
                      />
                      <FieldTitle>{option.label}</FieldTitle>
                    </Field>
                  </FieldLabel>
                ))}
              </FieldGroup>
            </FieldSet>
          </FieldGroup>
        </form>
      </div>
    </div>
  )
}
```
---

### Bento Grid 02 <a name="bento-grid-02"></a>

A simple bento section.

- **Registry Name**: `aliimam/bento-grid-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/bento-grid-02.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`
  - `@aliimam/icon`
  - `recharts`
- **Registry Dependencies**:
  - `chart`
  - `checkbox`
  - `table`
  - `badge`
  - `card`
  - `button`
  - `bento`
  - `device`
  - `gradient-bars`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Props & API Reference

```typescript
export interface ChartAreaProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}
```

#### Component Source Code

##### File: `bento.tsx`

```tsx
"use client"

import Link from "next/link"

import { Marquee } from "@/registry/aliimam/components/marquee"

type BentoBlock = {
  name: string
  category: string
  href: string
  light: string
  dark: string
}

function ThemeImage({
  light,
  dark,
  alt,
}: {
  light: string
  dark: string
  alt: string
}) {
  return (
    <>
      <img
        src={light}
        alt={alt}
        className={`border-muted/10 hidden h-full w-full border object-contain dark:block`}
      />
      <img
        src={dark}
        alt={alt}
        className={`border-muted/20 block h-full w-full border object-contain dark:hidden`}
      />
    </>
  )
}

export function BentoLanding({ blocks = [] }: { blocks?: BentoBlock[] }) {
  if (!blocks.length) {
    return (
      <section className="text-muted-foreground container py-24 text-center">
        No blocks found
      </section>
    )
  }

  return (
    <section className="border-b">
      <div className="container">
        <div className="bg-foreground">
          <Marquee
            pauseOnHover
            reverse
            speed="slow"
            className="[mask-image:linear-gradient(to_right,transparent,black_8%,black_92%,transparent)] py-1"
          >
            <div className="flex gap-1">
              {blocks.map((block) => (
                <Link
                  key={block.name}
                  href={block.href}
                  className="relative h-40 shrink-0 overflow-hidden md:h-60"
                >
                  <ThemeImage
                    light={block.light}
                    dark={block.dark}
                    alt={block.name}
                  />
                </Link>
              ))}
            </div>
          </Marquee>
        </div>
      </div>
    </section>
  )
}
```
##### File: `chart-area.tsx`

```tsx
/** Auto-generated - Do not edit */
'use client';
import React from 'react';

export interface ChartAreaProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}

export const ChartArea = React.forwardRef<SVGSVGElement, ChartAreaProps>(
  ({ size = 24, className = '', strokeWidth = 1, ...props }, ref) => (
    <svg 
      ref={ref}
      width={size}
      height={size} 
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      className={className}
      xmlns="http://www.w3.org/2000/svg"
      {...(strokeWidth !== undefined ? { strokeWidth } : {})}
      {...props}
    >
      <path d="M3 3v16a2 2 0 0 0 2 2h16" />
  <path d="M7 11.207a.5.5 0 0 1 .146-.353l2-2a.5.5 0 0 1 .708 0l3.292 3.292a.5.5 0 0 0 .708 0l4.292-4.292a.5.5 0 0 1 .854.353V16a1 1 0 0 1-1 1H8a1 1 0 0 1-1-1z" />
    </svg>
  )
);
ChartArea.displayName = "ChartArea";
export const ChartAreaMetadata = { 
  id: "chart-area", 
  baseId: "chart-area", 
  variant: "default", 
  name: "Chart Area", 
  category: "chart", 
  tags: [], 
  viewBox: "0 0 24 24" 
} as const;

export default ChartArea;
```
##### File: `checkbox-table.tsx`

```tsx
"use client"

import * as React from "react"

import { Checkbox } from "@/registry/aliimam/ui/checkbox"
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/registry/aliimam/ui/table"

const tableData = [
  {
    id: "1",
    name: "Sarah Chen",
    email: "sarah.chen@example.com",
    role: "Admin",
  },
  {
    id: "2",
    name: "Marcus Rodriguez",
    email: "marcus.rodriguez@example.com",
    role: "User",
  },
  {
    id: "3",
    name: "Priya Patel",
    email: "priya.patel@example.com",
    role: "User",
  },
  {
    id: "4",
    name: "David Kim",
    email: "david.kim@example.com",
    role: "Editor",
  },
]

export function CheckboxInTable() {
  const [selectedRows, setSelectedRows] = React.useState<Set<string>>(
    new Set(["1"])
  )

  const selectAll = selectedRows.size === tableData.length

  const handleSelectAll = (checked: boolean) => {
    if (checked) {
      setSelectedRows(new Set(tableData.map((row) => row.id)))
    } else {
      setSelectedRows(new Set())
    }
  }

  const handleSelectRow = (id: string, checked: boolean) => {
    const newSelected = new Set(selectedRows)
    if (checked) {
      newSelected.add(id)
    } else {
      newSelected.delete(id)
    }
    setSelectedRows(newSelected)
  }

  return (
    <Table>
      <TableHeader>
        <TableRow>
          <TableHead className="w-8">
            <Checkbox
              id="select-all-checkbox"
              name="select-all-checkbox"
              checked={selectAll}
              onCheckedChange={handleSelectAll}
            />
          </TableHead>
          <TableHead>Name</TableHead>
          <TableHead>Email</TableHead>
          <TableHead>Role</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        {tableData.map((row) => (
          <TableRow
            key={row.id}
            data-state={selectedRows.has(row.id) ? "selected" : undefined}
          >
            <TableCell>
              <Checkbox
                id={`row-${row.id}-checkbox`}
                name={`row-${row.id}-checkbox`}
                checked={selectedRows.has(row.id)}
                onCheckedChange={(checked) =>
                  handleSelectRow(row.id, checked === true)
                }
              />
            </TableCell>
            <TableCell className="font-medium">{row.name}</TableCell>
            <TableCell>{row.email}</TableCell>
            <TableCell>{row.role}</TableCell>
          </TableRow>
        ))}
      </TableBody>
    </Table>
  )
}
```
---

### Bento Grid 03 <a name="bento-grid-03"></a>

A simple bento section.

- **Registry Name**: `aliimam/bento-grid-03`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/bento-grid-03.json
```

#### Dependencies

- **Registry Dependencies**:
  - `grid-pattern`
  - `button`
  - `bento`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `bento.tsx`

```tsx
"use client"

import Link from "next/link"

import { Marquee } from "@/registry/aliimam/components/marquee"

type BentoBlock = {
  name: string
  category: string
  href: string
  light: string
  dark: string
}

function ThemeImage({
  light,
  dark,
  alt,
}: {
  light: string
  dark: string
  alt: string
}) {
  return (
    <>
      <img
        src={light}
        alt={alt}
        className={`border-muted/10 hidden h-full w-full border object-contain dark:block`}
      />
      <img
        src={dark}
        alt={alt}
        className={`border-muted/20 block h-full w-full border object-contain dark:hidden`}
      />
    </>
  )
}

export function BentoLanding({ blocks = [] }: { blocks?: BentoBlock[] }) {
  if (!blocks.length) {
    return (
      <section className="text-muted-foreground container py-24 text-center">
        No blocks found
      </section>
    )
  }

  return (
    <section className="border-b">
      <div className="container">
        <div className="bg-foreground">
          <Marquee
            pauseOnHover
            reverse
            speed="slow"
            className="[mask-image:linear-gradient(to_right,transparent,black_8%,black_92%,transparent)] py-1"
          >
            <div className="flex gap-1">
              {blocks.map((block) => (
                <Link
                  key={block.name}
                  href={block.href}
                  className="relative h-40 shrink-0 overflow-hidden md:h-60"
                >
                  <ThemeImage
                    light={block.light}
                    dark={block.dark}
                    alt={block.name}
                  />
                </Link>
              ))}
            </div>
          </Marquee>
        </div>
      </div>
    </section>
  )
}
```
---

### Bento Grid 04 <a name="bento-grid-04"></a>

A simple bento section.

- **Registry Name**: `aliimam/bento-grid-04`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/bento-grid-04.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `bento`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `bento.tsx`

```tsx
"use client"

import Link from "next/link"

import { Marquee } from "@/registry/aliimam/components/marquee"

type BentoBlock = {
  name: string
  category: string
  href: string
  light: string
  dark: string
}

function ThemeImage({
  light,
  dark,
  alt,
}: {
  light: string
  dark: string
  alt: string
}) {
  return (
    <>
      <img
        src={light}
        alt={alt}
        className={`border-muted/10 hidden h-full w-full border object-contain dark:block`}
      />
      <img
        src={dark}
        alt={alt}
        className={`border-muted/20 block h-full w-full border object-contain dark:hidden`}
      />
    </>
  )
}

export function BentoLanding({ blocks = [] }: { blocks?: BentoBlock[] }) {
  if (!blocks.length) {
    return (
      <section className="text-muted-foreground container py-24 text-center">
        No blocks found
      </section>
    )
  }

  return (
    <section className="border-b">
      <div className="container">
        <div className="bg-foreground">
          <Marquee
            pauseOnHover
            reverse
            speed="slow"
            className="[mask-image:linear-gradient(to_right,transparent,black_8%,black_92%,transparent)] py-1"
          >
            <div className="flex gap-1">
              {blocks.map((block) => (
                <Link
                  key={block.name}
                  href={block.href}
                  className="relative h-40 shrink-0 overflow-hidden md:h-60"
                >
                  <ThemeImage
                    light={block.light}
                    dark={block.dark}
                    alt={block.name}
                  />
                </Link>
              ))}
            </div>
          </Marquee>
        </div>
      </div>
    </section>
  )
}
```
---

### Bento Grid 05 <a name="bento-grid-05"></a>

A simple bento section.

- **Registry Name**: `aliimam/bento-grid-05`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/bento-grid-05.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`
  - `@aliimam/icons`
  - `recharts`
- **Registry Dependencies**:
  - `button`
  - `bento`
  - `orb`
  - `card`
  - `chart`
  - `bar-visualizer`
  - `avatar`
  - `field`
  - `toggle-group`
  - `message`
  - `response`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `bento.tsx`

```tsx
"use client"

import Link from "next/link"

import { Marquee } from "@/registry/aliimam/components/marquee"

type BentoBlock = {
  name: string
  category: string
  href: string
  light: string
  dark: string
}

function ThemeImage({
  light,
  dark,
  alt,
}: {
  light: string
  dark: string
  alt: string
}) {
  return (
    <>
      <img
        src={light}
        alt={alt}
        className={`border-muted/10 hidden h-full w-full border object-contain dark:block`}
      />
      <img
        src={dark}
        alt={alt}
        className={`border-muted/20 block h-full w-full border object-contain dark:hidden`}
      />
    </>
  )
}

export function BentoLanding({ blocks = [] }: { blocks?: BentoBlock[] }) {
  if (!blocks.length) {
    return (
      <section className="text-muted-foreground container py-24 text-center">
        No blocks found
      </section>
    )
  }

  return (
    <section className="border-b">
      <div className="container">
        <div className="bg-foreground">
          <Marquee
            pauseOnHover
            reverse
            speed="slow"
            className="[mask-image:linear-gradient(to_right,transparent,black_8%,black_92%,transparent)] py-1"
          >
            <div className="flex gap-1">
              {blocks.map((block) => (
                <Link
                  key={block.name}
                  href={block.href}
                  className="relative h-40 shrink-0 overflow-hidden md:h-60"
                >
                  <ThemeImage
                    light={block.light}
                    dark={block.dark}
                    alt={block.name}
                  />
                </Link>
              ))}
            </div>
          </Marquee>
        </div>
      </div>
    </section>
  )
}
```
##### File: `analytics.tsx`

```tsx
"use client"

import { Analytics as VercelAnalytics } from "@vercel/analytics/react"

export function Analytics() {
  return <VercelAnalytics />
}
```
##### File: `bar-visual.tsx`

```tsx
"use client"

import { useState } from "react"

import {
  BarVisualizer,
  type AgentState,
} from "@/registry/aliimam/ui/bar-visualizer"
import { Button } from "@/registry/aliimam/ui/button"
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/registry/aliimam/ui/card"

export function BarVisualizerDemo() {
  const [state, setState] = useState<AgentState>("listening")

  return (
    <Card className="border-none shadow-none">
      <CardHeader>
        <CardTitle>Audio Frequency Visualizer</CardTitle>
        <CardDescription>
          Real-time frequency band visualization with animated state transitions
        </CardDescription>
      </CardHeader>
      <CardContent>
        <div className="space-y-4">
          <BarVisualizer
            state={state}
            demo={true}
            barCount={20}
            minHeight={15}
            maxHeight={90}
            className="h-40 max-w-full"
          />

          <div className="flex flex-wrap gap-2">
            <Button
              size="sm"
              variant={state === "connecting" ? "default" : "outline"}
              onClick={() => setState("connecting")}
            >
              Connecting
            </Button>
            <Button
              size="sm"
              variant={state === "initializing" ? "default" : "outline"}
              onClick={() => setState("initializing")}
            >
              Initializing
            </Button>
            <Button
              size="sm"
              variant={state === "listening" ? "default" : "outline"}
              onClick={() => setState("listening")}
            >
              Listening
            </Button>
            <Button
              size="sm"
              variant={state === "speaking" ? "default" : "outline"}
              onClick={() => setState("speaking")}
            >
              Speaking
            </Button>
            <Button
              size="sm"
              variant={state === "thinking" ? "default" : "outline"}
              onClick={() => setState("thinking")}
            >
              Thinking
            </Button>
          </div>
        </div>
      </CardContent>
    </Card>
  )
}
```
##### File: `contributers.tsx`

```tsx
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/registry/aliimam/ui/avatar"

const usernames = [
  "shadcn",
  "vercel",
  "nextjs",
  "tailwindlabs",
  "typescript-lang",
  "eslint",
  "prettier",
  "babel",
  "webpack",
  "rollup",
  "vite",
  "react",
]

export function Contributors() {
  return (
    <div className="flex h-full w-full flex-col items-center justify-center">
      <h1>Contributors</h1>
      <div className="w-full p-6">
        <div className="flex flex-wrap justify-center gap-2">
          {usernames.map((username) => (
            <Avatar key={username}>
              <AvatarImage
                src={`https://github.com/${username}.png`}
                alt={username}
              />
              <AvatarFallback>{username.charAt(0)}</AvatarFallback>
            </Avatar>
          ))}
        </div>
      </div>
      <a
        href="#"
        className="text-muted-foreground text-sm underline underline-offset-4"
      >
        + 810 contributors
      </a>
    </div>
  )
}
```
##### File: `font-weight.tsx`

```tsx
"use client"

import * as React from "react"

import {
  Field,
  FieldDescription,
  FieldLabel,
} from "@/registry/aliimam/ui/field"
import {
  ToggleGroup,
  ToggleGroupItem,
} from "@/registry/aliimam/ui/toggle-group"

export function ToggleGroupFontWeight() {
  const [fontWeight, setFontWeight] = React.useState("normal")
  return (
    <Field className="flex h-full flex-col items-center justify-center">
      <FieldLabel>Font Weight</FieldLabel>
      <ToggleGroup
        type="single"
        value={fontWeight}
        onValueChange={(value) => setFontWeight(value)}
        variant="outline"
        spacing={2}
        size="lg"
      >
        <ToggleGroupItem
          value="light"
          aria-label="Light"
          className="flex size-16 flex-col items-center justify-center rounded-xl"
        >
          <span className="text-2xl leading-none font-light">Aa</span>
          <span className="text-muted-foreground text-xs">Light</span>
        </ToggleGroupItem>
        <ToggleGroupItem
          value="normal"
          aria-label="Normal"
          className="flex size-16 flex-col items-center justify-center rounded-xl"
        >
          <span className="text-2xl leading-none font-normal">Aa</span>
          <span className="text-muted-foreground text-xs">Normal</span>
        </ToggleGroupItem>
        <ToggleGroupItem
          value="medium"
          aria-label="Medium"
          className="flex size-16 flex-col items-center justify-center rounded-xl"
        >
          <span className="text-2xl leading-none font-medium">Aa</span>
          <span className="text-muted-foreground text-xs">Medium</span>
        </ToggleGroupItem>
        <ToggleGroupItem
          value="bold"
          aria-label="Bold"
          className="flex size-16 flex-col items-center justify-center rounded-xl"
        >
          <span className="text-2xl leading-none font-bold">Aa</span>
          <span className="text-muted-foreground text-xs">Bold</span>
        </ToggleGroupItem>
      </ToggleGroup>
      <FieldDescription>
        Use{" "}
        <code className="bg-muted rounded-md px-1 py-0.5 font-mono">
          font-{fontWeight}
        </code>{" "}
        to set the font weight.
      </FieldDescription>
    </Field>
  )
}
```
##### File: `message.tsx`

```tsx
import type { ComponentProps, HTMLAttributes } from "react"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/registry/aliimam/lib/utils"
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/registry/aliimam/ui/avatar"

export type MessageProps = HTMLAttributes<HTMLDivElement> & {
  from: "user" | "assistant"
}

export const Message = ({ className, from, ...props }: MessageProps) => (
  <div
    className={cn(
      "group flex w-full items-end justify-end gap-2 py-4",
      from === "user" ? "is-user" : "is-assistant flex-row-reverse justify-end",
      className
    )}
    {...props}
  />
)

const messageContentVariants = cva(
  "is-user:dark flex flex-col gap-2 overflow-hidden rounded-lg text-sm",
  {
    variants: {
      variant: {
        contained: [
          "max-w-[80%] px-4 py-3",
          "group-[.is-user]:bg-primary group-[.is-user]:text-primary-foreground",
          "group-[.is-assistant]:bg-secondary group-[.is-assistant]:text-foreground",
        ],
        flat: [
          "group-[.is-user]:max-w-[80%] group-[.is-user]:bg-secondary group-[.is-user]:px-4 group-[.is-user]:py-3 group-[.is-user]:text-foreground",
          "group-[.is-assistant]:text-foreground",
        ],
      },
    },
    defaultVariants: {
      variant: "contained",
    },
  }
)

export type MessageContentProps = HTMLAttributes<HTMLDivElement> &
  VariantProps<typeof messageContentVariants>

export const MessageContent = ({
  children,
  className,
  variant,
  ...props
}: MessageContentProps) => (
  <div
    className={cn(messageContentVariants({ variant, className }))}
    {...props}
  >
    {children}
  </div>
)

export type MessageAvatarProps = ComponentProps<typeof Avatar> & {
  src: string
  name?: string
}

export const MessageAvatar = ({
  src,
  name,
  className,
  ...props
}: MessageAvatarProps) => (
  <Avatar className={cn("ring-border size-8 ring-1", className)} {...props}>
    <AvatarImage alt="" className="mt-0 mb-0" src={src} />
    <AvatarFallback>{name?.slice(0, 2) || "ME"}</AvatarFallback>
  </Avatar>
)
```
---

### Blogs 01 <a name="blogs-01"></a>

A simple blogs page.

- **Registry Name**: `aliimam/blogs-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/blogs-01.json
```

#### Dependencies

- **NPM Packages**:
  - `motion/react`
- **Registry Dependencies**:
  - `aspect-ratio`
  - `badge`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Props & API Reference

```typescript
type LazyImageProps = {
  alt: string
  src: string
  className?: string
  containerClassName?: string
  /** URL of the fallback image. default: undefined */
  fallback?: string
  /** The ratio of the image. */
  ratio: number
  /** Whether the image should only load when it is in view. default: false */
  inView?: boolean
}
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
##### File: `blog-page.tsx`

```tsx
/* eslint-disable @typescript-eslint/no-unused-vars */
import { ArrowRight, Clock, User } from "lucide-react"

import { cn } from "@/registry/aliimam/lib/utils"
import { Badge } from "@/registry/aliimam/ui/badge"

import { LazyImage } from "./lazy-image"

type BlogType = {
  title: string
  href: string
  description: string
  author: string
  createdAt: string
  readTime: string
  image: string
  category: string
}

const blogs: BlogType[] = [
  {
    title: "Design Systems That Scale",
    href: "#",
    description:
      "Learn how to build and maintain scalable design systems that empower teams to move faster while staying consistent.",
    image: "/r/hero-01-light.png",
    createdAt: "2025-08-25",
    author: "Ava Mitchell",
    readTime: "7 min",
    category: "Design",
  },
  {
    title: "The Psychology of Color in UI",
    href: "#",
    description:
      "Explore how different colors influence user perception, emotion, and conversion in digital product design.",
    image: "/r/hero-02-light.png",
    createdAt: "2025-07-14",
    author: "Liam Carter",
    readTime: "5 min",
    category: "Design",
  },
  {
    title: "Microinteractions That Delight",
    href: "#",
    description:
      "Discover how subtle animations and interactions can enhance usability and bring joy to your users.",
    image: "/r/hero-03-light.png",
    createdAt: "2025-06-30",
    author: "Sophia Kim",
    readTime: "6 min",
    category: "Animation",
  },
  {
    title: "Accessibility Beyond Compliance",
    href: "#",
    description:
      "Practical steps to make your UI accessible, not just legally compliant, but genuinely inclusive for everyone.",
    image: "/r/hero-04-light.png",
    createdAt: "2025-06-18",
    author: "Ethan Rodriguez",
    readTime: "8 min",
    category: "Accessibility",
  },
  {
    title: "Dark Mode Done Right",
    href: "#",
    description:
      "Tips and tricks to design beautiful and functional dark mode experiences that users will love.",
    image: "/r/hero-05-light.png",
    createdAt: "2025-05-20",
    author: "Maya Chen",
    readTime: "4 min",
    category: "Design",
  },
  {
    title: "Typography That Speaks",
    href: "#",
    description:
      "How to select and pair typefaces that enhance readability, hierarchy, and brand personality.",
    image: "/r/hero-06-light.png",
    createdAt: "2025-05-02",
    author: "Noah Patel",
    readTime: "9 min",
    category: "Typography",
  },
  {
    title: "The Future of UI Animation",
    href: "#",
    description:
      "From motion guidelines to advanced prototyping—discover where UI animation is headed in 2025.",
    image: "/r/hero-07-light.png",
    createdAt: "2025-04-15",
    author: "Chloe Ramirez",
    readTime: "10 min",
    category: "Animation",
  },
  {
    title: "Minimalism vs Maximalism",
    href: "#",
    description:
      "A deep dive into two opposing design philosophies and how to decide which fits your product.",
    image: "/r/hero-08-light.png",
    createdAt: "2025-04-01",
    author: "Benjamin Scott",
    readTime: "6 min",
    category: "Design",
  },
  {
    title: "Designing for Mobile-First",
    href: "#",
    description:
      "Best practices for mobile-first design, from layout decisions to performance optimization.",
    image: "/r/hero-10-light.png",
    createdAt: "2025-03-22",
    author: "Isabella White",
    readTime: "7 min",
    category: "Responsive",
  },
  {
    title: "Figma Hacks for Power Users",
    href: "#",
    description:
      "Hidden features, shortcuts, and workflows in Figma that can dramatically speed up your design process.",
    image: "/r/hero-11-light.png",
    createdAt: "2025-03-09",
    author: "James Walker",
    readTime: "5 min",
    category: "Tools",
  },
  {
    title: "Designing With AI Tools",
    href: "#",
    description:
      "A practical look at how AI tools are shaping UI/UX workflows—from ideation to final delivery.",
    image: "/r/hero-12-light.png",
    createdAt: "2025-02-28",
    author: "Olivia Brooks",
    readTime: "8 min",
    category: "Tools",
  },
  {
    title: "The Art of Prototyping",
    href: "#",
    description:
      "How to create prototypes that effectively communicate your ideas and speed up stakeholder feedback.",
    image: "/r/feature-01-light.png",
    createdAt: "2025-02-14",
    author: "Daniel Green",
    readTime: "6 min",
    category: "Design",
  },
]

const categories = [
  "All",
  "Design",
  "Animation",
  "Accessibility",
  "Typography",
  "Responsive",
  "Tools",
]

export function BlogPage() {
  const featuredBlog = blogs[0]
  const recentBlogs = blogs.slice(1, 7)
  const allBlogs = blogs.slice(1)

  return (
    <div className="min-h-screen">
      <section className="border-border/40 relative overflow-hidden border-b">
        <div
          className="absolute inset-0 z-0"
          style={{
            background:
              "linear-gradient(to right, var(--background) 0%, var(--background) 50%, rgba(255,255,255,0) 100%), radial-gradient(ellipse at 110% 50%, var(--primary) 0%, var(--background) 70%)",
            opacity: 0.7,
          }}
        >
          <div
            style={{
              WebkitMaskImage:
                "linear-gradient(to right, rgba(0,0,0,0) 0%, rgba(0,0,0,0.45) 100%)",
              backgroundImage:
                "repeating-linear-gradient(0deg, var(--primary) 0px, var(--primary) 1px, transparent 1px, transparent 20px)",
              height: "100%",
              left: "0",
              maskImage:
                "linear-gradient(to right, rgba(0,0,0,0) 0%, rgba(0,0,0,0.45) 100%)",
              pointerEvents: "none",
              position: "absolute",
              top: "0",
              width: "100%",
            }}
          />
        </div>
        <div className="relative z-10 mx-auto max-w-7xl px-4 py-16 sm:px-6 lg:px-8">
          <div className="space-y-4">
            <Badge variant="secondary" className="w-fit">
              Latest Articles
            </Badge>
            <h1 className="text-foreground text-4xl font-bold tracking-tight sm:text-5xl md:text-6xl">
              Design & Development <br className="hidden sm:block" />
              Insights
            </h1>
            <p className="text-muted-foreground max-w-2xl text-lg">
              Explore thought-provoking articles on design systems, UI/UX
              principles, and modern development practices. Learn from industry
              experts and stay ahead of the curve.
            </p>
          </div>
        </div>
      </section>

      <section className="border-border/40 border-b">
        <div className="mx-auto max-w-7xl px-4 py-12 sm:px-6 lg:px-8">
          <a
            href={featuredBlog.href}
            className="group grid items-stretch gap-8 md:grid-cols-2 md:gap-12"
          >
            <div className="bg-muted overflow-hidden rounded-md">
              <LazyImage
                alt={featuredBlog.title}
                className="transition-transform duration-500 group-hover:scale-110"
                containerClassName="rounded-md"
                fallback="/placeholder.svg"
                inView={true}
                ratio={16 / 9}
                src={featuredBlog.image}
              />
            </div>

            <div className="flex flex-col justify-center space-y-6 py-4">
              <div className="space-y-3">
                <Badge variant="outline">{featuredBlog.category}</Badge>
                <h2 className="text-foreground group-hover:text-primary text-3xl font-bold transition-colors sm:text-4xl">
                  {featuredBlog.title}
                </h2>
                <p className="text-muted-foreground text-base leading-relaxed">
                  {featuredBlog.description}
                </p>
              </div>

              <div className="border-border/40 flex flex-wrap gap-6 border-t pt-4">
                <div className="text-muted-foreground flex items-center gap-2 text-sm">
                  <User className="h-4 w-4" />
                  <span>{featuredBlog.author}</span>
                </div>
                <div className="text-muted-foreground flex items-center gap-2 text-sm">
                  <Clock className="h-4 w-4" />
                  <span>{featuredBlog.readTime} read</span>
                </div>
                <div className="text-muted-foreground flex items-center gap-2 text-sm">
                  <span>{featuredBlog.createdAt}</span>
                </div>
              </div>

              <button className="group/btn text-primary mt-2 flex w-fit items-center gap-2 font-semibold transition-all hover:gap-3">
                Read Article
                <ArrowRight className="h-4 w-4 transition-transform group-hover/btn:translate-x-1" />
              </button>
            </div>
          </a>
        </div>
      </section>

      <section className="border-border/40 bg-muted/50 border-b">
        <div className="mx-auto max-w-7xl px-4 py-8 sm:px-6 lg:px-8">
          <div className="flex flex-wrap gap-2">
            {categories.map((category) => (
              <button
                key={category}
                className={cn(
                  "rounded-full px-4 py-2 text-sm font-medium transition-colors",
                  category === "All"
                    ? "bg-primary text-primary-foreground"
                    : "bg-background border-border/60 text-foreground hover:border-primary/40 hover:bg-muted border"
                )}
              >
                {category}
              </button>
            ))}
          </div>
        </div>
      </section>

      {/* Recent Articles Grid */}
      <section className="border-border/40 border-b">
        <div className="mx-auto max-w-7xl px-4 py-16 sm:px-6 lg:px-8">
          <div className="space-y-8">
            <h2 className="text-foreground text-3xl font-bold">
              Recent Articles
            </h2>
            <div className="grid gap-8 md:grid-cols-2 lg:grid-cols-3">
              {recentBlogs.map((blog) => (
                <BlogCard key={blog.title} {...blog} />
              ))}
            </div>
          </div>
        </div>
      </section>

      <section className="border-border/40 border-b">
        <div className="mx-auto max-w-7xl px-4 py-16 sm:px-6 lg:px-8">
          <div className="space-y-8">
            <h2 className="text-foreground text-3xl font-bold">All Articles</h2>
            <div className="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
              {allBlogs.map((blog) => (
                <BlogCardCompact key={blog.title} {...blog} />
              ))}
            </div>
          </div>
        </div>
      </section>

      {/* Newsletter CTA */}
      <section className="border-border/40 relative overflow-hidden border-b">
        <div
          className="absolute inset-0 z-0"
          style={{
            background:
              "linear-gradient(to bottom, var(--background) 30%, var(--primary) 65%, var(--background) 100%)",
            opacity: 0.5,
          }}
        >
          <div
            style={{
              WebkitMaskImage:
                "linear-gradient(to bottom, rgba(0,0,0,0) 25%, rgba(0,0,0,0.4) 50%, rgba(0,0,0,0) 75%)",
              backgroundImage:
                "repeating-linear-gradient(90deg, var(--primary) 0px, var(--primary) 1px, transparent 1px, transparent 45px)",
              height: "100%",
              left: "0",
              maskImage:
                "linear-gradient(to bottom, rgba(0,0,0,0) 25%, rgba(0,0,0,0.4) 50%, rgba(0,0,0,0) 75%)",
              pointerEvents: "none",
              position: "absolute",
              top: "0",
              width: "100%",
            }}
          />
        </div>
        <div className="mx-auto max-w-7xl px-4 py-20 sm:px-6 lg:px-8">
          <div className="border-primary/20 bg-background/80 relative z-10 space-y-4 rounded-md border p-8 text-center shadow-2xl md:p-12">
            <h2 className="text-foreground text-3xl font-bold">Stay Updated</h2>
            <p className="text-muted-foreground mx-auto max-w-2xl">
              Subscribe to our newsletter to receive the latest articles, design
              trends, and development insights delivered to your inbox.
            </p>
            <div className="mx-auto flex max-w-sm flex-col gap-2 pt-4 sm:flex-row">
              <input
                type="email"
                placeholder="Enter your email"
                className="bg-background border-border/40 text-foreground placeholder:text-muted-foreground focus:ring-primary/50 flex-1 rounded-lg border px-4 py-3 focus:ring-2 focus:outline-none"
              />
              <button className="bg-primary text-primary-foreground hover:bg-primary/90 rounded-lg px-6 py-3 font-semibold whitespace-nowrap transition-colors">
                Subscribe
              </button>
            </div>
          </div>
        </div>
      </section>

      {/* Footer */}
      <footer className="bg-muted/50">
        <div className="mx-auto max-w-7xl px-4 py-12 sm:px-6 lg:px-8">
          <div className="text-muted-foreground flex flex-col items-center justify-between gap-8 text-sm md:flex-row">
            <p>© 2025 Design Blog. All rights reserved.</p>
            <div className="flex gap-6">
              <a href="#" className="hover:text-foreground transition-colors">
                About
              </a>
              <a href="#" className="hover:text-foreground transition-colors">
                Contact
              </a>
              <a href="#" className="hover:text-foreground transition-colors">
                Privacy
              </a>
            </div>
          </div>
        </div>
      </footer>
    </div>
  )
}

function BlogCard({
  title,
  description,
  // eslint-disable-next-line @typescript-eslint/no-unused-vars
  createdAt,
  readTime,
  image,
  author,
  category,
  className,
  ...props
}: React.ComponentProps<"a"> & BlogType) {
  return (
    <a
      className={cn(
        "group border-border/60 hover:border-primary/40 flex flex-col gap-4 overflow-hidden rounded-xl border transition-all hover:shadow-lg",
        className
      )}
      {...props}
    >
      <div className="bg-muted h-48 overflow-hidden">
        <LazyImage
          alt={title}
          className="transition-transform duration-500 group-hover:scale-110"
          containerClassName="w-full h-full"
          fallback="https://placehold.co/640x360?text=fallback-image"
          inView={false}
          ratio={16 / 9}
          src={image}
        />
      </div>

      <div className="flex flex-1 flex-col gap-3 px-4 pb-4">
        <Badge variant="secondary" className="w-fit">
          {category}
        </Badge>
        <h3 className="text-foreground group-hover:text-primary line-clamp-2 text-lg font-bold transition-colors">
          {title}
        </h3>
        <p className="text-muted-foreground line-clamp-3 flex-1 text-sm">
          {description}
        </p>

        <div className="text-muted-foreground border-border/40 flex items-center gap-2 border-t pt-2 text-xs">
          <span className="truncate">{author}</span>
          <span>•</span>
          <span className="whitespace-nowrap">{readTime} read</span>
        </div>
      </div>
    </a>
  )
}

function BlogCardCompact({
  title,
  description,
  createdAt,
  readTime,
  image,
  author,
  category,
  className,
  ...props
}: React.ComponentProps<"a"> & BlogType) {
  return (
    <a
      className={cn(
        "group hover:border-border/60 hover:bg-muted/40 flex flex-col gap-3 rounded-lg border border-transparent p-3 transition-all",
        className
      )}
      {...props}
    >
      <div className="flex items-start gap-3">
        <div className="bg-muted flex h-20 w-20 shrink-0 overflow-hidden rounded-lg">
          <LazyImage
            alt={title}
            className="transition-transform duration-500 group-hover:scale-110"
            containerClassName="w-full h-full"
            fallback="/placeholder.svg"
            inView={false}
            ratio={1}
            src={image}
          />
        </div>

        <div className="min-w-0 flex-1">
          <Badge variant="outline" className="mb-2 text-xs">
            {category}
          </Badge>
          <h4 className="text-foreground group-hover:text-primary line-clamp-2 text-sm font-semibold transition-colors">
            {title}
          </h4>
          <p className="text-muted-foreground mt-1 text-xs">{readTime} read</p>
        </div>
      </div>
    </a>
  )
}
```
##### File: `lazy-image.tsx`

```tsx
"use client"

import React from "react"
import { useInView } from "motion/react"

import { cn } from "@/registry/aliimam/lib/utils"
import { AspectRatio } from "@/registry/aliimam/ui/aspect-ratio"

type LazyImageProps = {
  alt: string
  src: string
  className?: string
  containerClassName?: string
  /** URL of the fallback image. default: undefined */
  fallback?: string
  /** The ratio of the image. */
  ratio: number
  /** Whether the image should only load when it is in view. default: false */
  inView?: boolean
}

export function LazyImage({
  alt,
  src,
  ratio,
  fallback,
  inView = false,
  className,
  containerClassName,
}: LazyImageProps) {
  const ref = React.useRef<HTMLDivElement | null>(null)
  const imgRef = React.useRef<HTMLImageElement | null>(null)
  const isInView = useInView(ref, { once: true })

  const [imgSrc, setImgSrc] = React.useState<string | undefined>(
    inView ? undefined : src
  )
  const [isLoading, setIsLoading] = React.useState(true)

  const handleError = () => {
    if (fallback) {
      setImgSrc(fallback)
    }
    setIsLoading(false)
  }

  const handleLoad = React.useCallback(() => {
    setIsLoading(false)
  }, [])

  // Load image only when inView
  React.useEffect(() => {
    if (inView && isInView && !imgSrc) {
      setImgSrc(src)
    }
  }, [inView, isInView, src, imgSrc])

  // Handle cached images instantly
  React.useEffect(() => {
    if (imgRef.current?.complete) {
      handleLoad()
    }
  }, [handleLoad])

  return (
    <AspectRatio
      className={cn(
        "bg-accent/30 relative size-full overflow-hidden border",
        containerClassName
      )}
      ratio={ratio}
      ref={ref}
    >
      {imgSrc && (
        // biome-ignore lint/correctness/useImageSize: dynamic image size
        <img
          alt={alt}
          className={cn(
            "aspect-video size-full object-cover transition-opacity duration-500",
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
  )
}
```
---

### Book Demo 01 <a name="book-demo-01"></a>

A simple book-demo section.

- **Registry Name**: `aliimam/book-demo-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/book-demo-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `input`
  - `label`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `book-demo.tsx`

```tsx
"use client"

import { ArrowRight } from "@aliimam/icons"
import {
  Apple,
  ClaudeAI,
  Cursor,
  Github,
  GoogleGemini,
  OpenAI,
  Replicate,
} from "@aliimam/logos"

import { Button } from "@/registry/aliimam/ui/button"
import { Checkbox } from "@/registry/aliimam/ui/checkbox"
import { Input } from "@/registry/aliimam/ui/input"
import { Label } from "@/registry/aliimam/ui/label"
import { Separator } from "@/registry/aliimam/ui/separator"
import { Textarea } from "@/registry/aliimam/ui/textarea"

const Bookademo4 = () => {
  return (
    <section className="py-6">
      <div className="mx-6">
        <div className="w-full">
          <div className="space-y-6">
            <h1 className="text-xl font-light text-green-500">
              Connect with a Design Expert
            </h1>
            <h2 className="w-full text-5xl font-semibold tracking-tighter md:text-7xl">
              Let’s craft your <br className="hidden md:block" /> vision into
              reality
            </h2>
          </div>
          <div className="mt-6 grid w-full border lg:grid-cols-2">
            <form className="space-y-8 p-10 lg:border-r">
              <div className="grid grid-cols-2 gap-4">
                <div>
                  <Label htmlFor="firstName">Full Name</Label>
                  <Input
                    type="text"
                    placeholder="Your Name"
                    className="mt-2"
                    required
                  />
                </div>
                <div>
                  <Label htmlFor="organization">Design Studio</Label>
                  <Input
                    type="text"
                    placeholder="Your Studio"
                    className="mt-2"
                    required
                  />
                </div>
                <div>
                  <Label htmlFor="position">Role</Label>
                  <Input
                    type="text"
                    placeholder="Creative Director"
                    className="mt-2"
                    required
                  />
                </div>
                <div>
                  <Label htmlFor="businessEmail">Work Email</Label>
                  <Input
                    type="email"
                    placeholder="name@designstudio.com"
                    className="mt-2"
                    required
                  />
                </div>
              </div>
              <div>
                <Label>Project Budget (select one)</Label>
                <div className="mt-2 flex justify-between">
                  <div className="space-y-2">
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>{"<$50k"}</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$50k – $250k</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$250k – $1M</Label>
                    </div>
                  </div>
                  <div className="space-y-2">
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$1M – $5M</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$5M – $10M</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>{">$10M"}</Label>
                    </div>
                  </div>
                </div>
                <Separator className="my-6 w-full" />
                <div>
                  <Label>
                    Which design field do you represent? (select all that apply)
                  </Label>
                  <div className="mt-2 space-y-2">
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>Graphic Design</Label>
                    </div>
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>UI/UX Design</Label>
                    </div>
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>Product Design</Label>
                    </div>
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>Other</Label>
                    </div>
                  </div>
                </div>
              </div>
              <Separator className="my-6 w-full" />
              <div>
                <Label>Tell us about your design needs</Label>
                <Textarea
                  placeholder="Describe your project vision..."
                  className="mt-4 h-30"
                />
              </div>
              <Separator className="my-6 w-full" />
              <div className="flex space-x-2">
                <Checkbox />
                <p className="text-muted-foreground text-xs">
                  By checking this box, you agree to receive design insights,
                  updates, and inspiration from our team. See our Privacy Policy
                  and Terms & Conditions. You can unsubscribe anytime.
                </p>
              </div>
              <Button>
                <ArrowRight />
                Start Your Design Journey
              </Button>
            </form>
            <div className="mt-6 flex h-full w-full flex-col space-y-6 border-t p-10 lg:border-t-0">
              <div className="">
                <Apple size={80} />
                <p className="mt-10 italic">
                  “Collaborating with our design partners elevates Apple’s
                  vision for innovative and sustainable design, creating lasting
                  impact for our brand and community.”
                </p>
                <p className="mt-10 font-semibold">Ali Imam // Apple</p>
                <p>Chief Design Officer</p>
              </div>
              <Separator className="my-10 w-full" />
              <p>Trusted by global leaders in design and innovation</p>
              <div className="flex justify-start">
                <div className="grid grid-cols-2 border p-2 md:grid-cols-3">
                  {[
                    <OpenAI key="openai" size={40} />,
                    <ClaudeAI key="claude" size={40} />,
                    <Replicate key="replicate" size={40} />,
                    <Cursor key="cursor" size={40} />,
                    <GoogleGemini key="gemini" size={40} />,
                    <Github key="github" size={40} />,
                  ].map((Logo, i) => (
                    <div
                      key={i}
                      className="flex h-30 w-30 items-center justify-center border md:h-40 md:w-40"
                    >
                      {Logo}
                    </div>
                  ))}
                </div>
              </div>
            </div>
          </div>
          <div className="w-full border border-t-0 p-20 text-center">
            <h3 className="text-3xl font-semibold md:text-5xl">
              Ready to elevate <br /> your design impact?
            </h3>
            <div className="mt-6 flex flex-wrap justify-center gap-3">
              <Button variant="outline">Get in Touch</Button>
              <Button>
                <ArrowRight />
                Explore Our Design Services
              </Button>
            </div>
          </div>
        </div>
      </div>
    </section>
  )
}

export default Bookademo4
```
---

### Book Demo 02 <a name="book-demo-02"></a>

A simple book-demo section.

- **Registry Name**: `aliimam/book-demo-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/book-demo-02.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `input`
  - `label`
  - `select`
  - `separator`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `book-demo.tsx`

```tsx
"use client"

import { ArrowRight } from "@aliimam/icons"
import {
  Apple,
  ClaudeAI,
  Cursor,
  Github,
  GoogleGemini,
  OpenAI,
  Replicate,
} from "@aliimam/logos"

import { Button } from "@/registry/aliimam/ui/button"
import { Checkbox } from "@/registry/aliimam/ui/checkbox"
import { Input } from "@/registry/aliimam/ui/input"
import { Label } from "@/registry/aliimam/ui/label"
import { Separator } from "@/registry/aliimam/ui/separator"
import { Textarea } from "@/registry/aliimam/ui/textarea"

const Bookademo4 = () => {
  return (
    <section className="py-6">
      <div className="mx-6">
        <div className="w-full">
          <div className="space-y-6">
            <h1 className="text-xl font-light text-green-500">
              Connect with a Design Expert
            </h1>
            <h2 className="w-full text-5xl font-semibold tracking-tighter md:text-7xl">
              Let’s craft your <br className="hidden md:block" /> vision into
              reality
            </h2>
          </div>
          <div className="mt-6 grid w-full border lg:grid-cols-2">
            <form className="space-y-8 p-10 lg:border-r">
              <div className="grid grid-cols-2 gap-4">
                <div>
                  <Label htmlFor="firstName">Full Name</Label>
                  <Input
                    type="text"
                    placeholder="Your Name"
                    className="mt-2"
                    required
                  />
                </div>
                <div>
                  <Label htmlFor="organization">Design Studio</Label>
                  <Input
                    type="text"
                    placeholder="Your Studio"
                    className="mt-2"
                    required
                  />
                </div>
                <div>
                  <Label htmlFor="position">Role</Label>
                  <Input
                    type="text"
                    placeholder="Creative Director"
                    className="mt-2"
                    required
                  />
                </div>
                <div>
                  <Label htmlFor="businessEmail">Work Email</Label>
                  <Input
                    type="email"
                    placeholder="name@designstudio.com"
                    className="mt-2"
                    required
                  />
                </div>
              </div>
              <div>
                <Label>Project Budget (select one)</Label>
                <div className="mt-2 flex justify-between">
                  <div className="space-y-2">
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>{"<$50k"}</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$50k – $250k</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$250k – $1M</Label>
                    </div>
                  </div>
                  <div className="space-y-2">
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$1M – $5M</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$5M – $10M</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>{">$10M"}</Label>
                    </div>
                  </div>
                </div>
                <Separator className="my-6 w-full" />
                <div>
                  <Label>
                    Which design field do you represent? (select all that apply)
                  </Label>
                  <div className="mt-2 space-y-2">
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>Graphic Design</Label>
                    </div>
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>UI/UX Design</Label>
                    </div>
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>Product Design</Label>
                    </div>
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>Other</Label>
                    </div>
                  </div>
                </div>
              </div>
              <Separator className="my-6 w-full" />
              <div>
                <Label>Tell us about your design needs</Label>
                <Textarea
                  placeholder="Describe your project vision..."
                  className="mt-4 h-30"
                />
              </div>
              <Separator className="my-6 w-full" />
              <div className="flex space-x-2">
                <Checkbox />
                <p className="text-muted-foreground text-xs">
                  By checking this box, you agree to receive design insights,
                  updates, and inspiration from our team. See our Privacy Policy
                  and Terms & Conditions. You can unsubscribe anytime.
                </p>
              </div>
              <Button>
                <ArrowRight />
                Start Your Design Journey
              </Button>
            </form>
            <div className="mt-6 flex h-full w-full flex-col space-y-6 border-t p-10 lg:border-t-0">
              <div className="">
                <Apple size={80} />
                <p className="mt-10 italic">
                  “Collaborating with our design partners elevates Apple’s
                  vision for innovative and sustainable design, creating lasting
                  impact for our brand and community.”
                </p>
                <p className="mt-10 font-semibold">Ali Imam // Apple</p>
                <p>Chief Design Officer</p>
              </div>
              <Separator className="my-10 w-full" />
              <p>Trusted by global leaders in design and innovation</p>
              <div className="flex justify-start">
                <div className="grid grid-cols-2 border p-2 md:grid-cols-3">
                  {[
                    <OpenAI key="openai" size={40} />,
                    <ClaudeAI key="claude" size={40} />,
                    <Replicate key="replicate" size={40} />,
                    <Cursor key="cursor" size={40} />,
                    <GoogleGemini key="gemini" size={40} />,
                    <Github key="github" size={40} />,
                  ].map((Logo, i) => (
                    <div
                      key={i}
                      className="flex h-30 w-30 items-center justify-center border md:h-40 md:w-40"
                    >
                      {Logo}
                    </div>
                  ))}
                </div>
              </div>
            </div>
          </div>
          <div className="w-full border border-t-0 p-20 text-center">
            <h3 className="text-3xl font-semibold md:text-5xl">
              Ready to elevate <br /> your design impact?
            </h3>
            <div className="mt-6 flex flex-wrap justify-center gap-3">
              <Button variant="outline">Get in Touch</Button>
              <Button>
                <ArrowRight />
                Explore Our Design Services
              </Button>
            </div>
          </div>
        </div>
      </div>
    </section>
  )
}

export default Bookademo4
```
---

### Book Demo 03 <a name="book-demo-03"></a>

A simple book-demo section.

- **Registry Name**: `aliimam/book-demo-03`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/book-demo-03.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
  - `@aliimam/logos`
- **Registry Dependencies**:
  - `button`
  - `input`
  - `label`
  - `select`
  - `textarea`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `book-demo.tsx`

```tsx
"use client"

import { ArrowRight } from "@aliimam/icons"
import {
  Apple,
  ClaudeAI,
  Cursor,
  Github,
  GoogleGemini,
  OpenAI,
  Replicate,
} from "@aliimam/logos"

import { Button } from "@/registry/aliimam/ui/button"
import { Checkbox } from "@/registry/aliimam/ui/checkbox"
import { Input } from "@/registry/aliimam/ui/input"
import { Label } from "@/registry/aliimam/ui/label"
import { Separator } from "@/registry/aliimam/ui/separator"
import { Textarea } from "@/registry/aliimam/ui/textarea"

const Bookademo4 = () => {
  return (
    <section className="py-6">
      <div className="mx-6">
        <div className="w-full">
          <div className="space-y-6">
            <h1 className="text-xl font-light text-green-500">
              Connect with a Design Expert
            </h1>
            <h2 className="w-full text-5xl font-semibold tracking-tighter md:text-7xl">
              Let’s craft your <br className="hidden md:block" /> vision into
              reality
            </h2>
          </div>
          <div className="mt-6 grid w-full border lg:grid-cols-2">
            <form className="space-y-8 p-10 lg:border-r">
              <div className="grid grid-cols-2 gap-4">
                <div>
                  <Label htmlFor="firstName">Full Name</Label>
                  <Input
                    type="text"
                    placeholder="Your Name"
                    className="mt-2"
                    required
                  />
                </div>
                <div>
                  <Label htmlFor="organization">Design Studio</Label>
                  <Input
                    type="text"
                    placeholder="Your Studio"
                    className="mt-2"
                    required
                  />
                </div>
                <div>
                  <Label htmlFor="position">Role</Label>
                  <Input
                    type="text"
                    placeholder="Creative Director"
                    className="mt-2"
                    required
                  />
                </div>
                <div>
                  <Label htmlFor="businessEmail">Work Email</Label>
                  <Input
                    type="email"
                    placeholder="name@designstudio.com"
                    className="mt-2"
                    required
                  />
                </div>
              </div>
              <div>
                <Label>Project Budget (select one)</Label>
                <div className="mt-2 flex justify-between">
                  <div className="space-y-2">
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>{"<$50k"}</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$50k – $250k</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$250k – $1M</Label>
                    </div>
                  </div>
                  <div className="space-y-2">
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$1M – $5M</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$5M – $10M</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>{">$10M"}</Label>
                    </div>
                  </div>
                </div>
                <Separator className="my-6 w-full" />
                <div>
                  <Label>
                    Which design field do you represent? (select all that apply)
                  </Label>
                  <div className="mt-2 space-y-2">
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>Graphic Design</Label>
                    </div>
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>UI/UX Design</Label>
                    </div>
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>Product Design</Label>
                    </div>
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>Other</Label>
                    </div>
                  </div>
                </div>
              </div>
              <Separator className="my-6 w-full" />
              <div>
                <Label>Tell us about your design needs</Label>
                <Textarea
                  placeholder="Describe your project vision..."
                  className="mt-4 h-30"
                />
              </div>
              <Separator className="my-6 w-full" />
              <div className="flex space-x-2">
                <Checkbox />
                <p className="text-muted-foreground text-xs">
                  By checking this box, you agree to receive design insights,
                  updates, and inspiration from our team. See our Privacy Policy
                  and Terms & Conditions. You can unsubscribe anytime.
                </p>
              </div>
              <Button>
                <ArrowRight />
                Start Your Design Journey
              </Button>
            </form>
            <div className="mt-6 flex h-full w-full flex-col space-y-6 border-t p-10 lg:border-t-0">
              <div className="">
                <Apple size={80} />
                <p className="mt-10 italic">
                  “Collaborating with our design partners elevates Apple’s
                  vision for innovative and sustainable design, creating lasting
                  impact for our brand and community.”
                </p>
                <p className="mt-10 font-semibold">Ali Imam // Apple</p>
                <p>Chief Design Officer</p>
              </div>
              <Separator className="my-10 w-full" />
              <p>Trusted by global leaders in design and innovation</p>
              <div className="flex justify-start">
                <div className="grid grid-cols-2 border p-2 md:grid-cols-3">
                  {[
                    <OpenAI key="openai" size={40} />,
                    <ClaudeAI key="claude" size={40} />,
                    <Replicate key="replicate" size={40} />,
                    <Cursor key="cursor" size={40} />,
                    <GoogleGemini key="gemini" size={40} />,
                    <Github key="github" size={40} />,
                  ].map((Logo, i) => (
                    <div
                      key={i}
                      className="flex h-30 w-30 items-center justify-center border md:h-40 md:w-40"
                    >
                      {Logo}
                    </div>
                  ))}
                </div>
              </div>
            </div>
          </div>
          <div className="w-full border border-t-0 p-20 text-center">
            <h3 className="text-3xl font-semibold md:text-5xl">
              Ready to elevate <br /> your design impact?
            </h3>
            <div className="mt-6 flex flex-wrap justify-center gap-3">
              <Button variant="outline">Get in Touch</Button>
              <Button>
                <ArrowRight />
                Explore Our Design Services
              </Button>
            </div>
          </div>
        </div>
      </div>
    </section>
  )
}

export default Bookademo4
```
---

### Book Demo 04 <a name="book-demo-04"></a>

A simple book-demo section.

- **Registry Name**: `aliimam/book-demo-04`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/book-demo-04.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
  - `@aliimam/logos`
- **Registry Dependencies**:
  - `button`
  - `input`
  - `label`
  - `checkbox`
  - `textarea`
  - `separator`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `book-demo.tsx`

```tsx
"use client"

import { ArrowRight } from "@aliimam/icons"
import {
  Apple,
  ClaudeAI,
  Cursor,
  Github,
  GoogleGemini,
  OpenAI,
  Replicate,
} from "@aliimam/logos"

import { Button } from "@/registry/aliimam/ui/button"
import { Checkbox } from "@/registry/aliimam/ui/checkbox"
import { Input } from "@/registry/aliimam/ui/input"
import { Label } from "@/registry/aliimam/ui/label"
import { Separator } from "@/registry/aliimam/ui/separator"
import { Textarea } from "@/registry/aliimam/ui/textarea"

const Bookademo4 = () => {
  return (
    <section className="py-6">
      <div className="mx-6">
        <div className="w-full">
          <div className="space-y-6">
            <h1 className="text-xl font-light text-green-500">
              Connect with a Design Expert
            </h1>
            <h2 className="w-full text-5xl font-semibold tracking-tighter md:text-7xl">
              Let’s craft your <br className="hidden md:block" /> vision into
              reality
            </h2>
          </div>
          <div className="mt-6 grid w-full border lg:grid-cols-2">
            <form className="space-y-8 p-10 lg:border-r">
              <div className="grid grid-cols-2 gap-4">
                <div>
                  <Label htmlFor="firstName">Full Name</Label>
                  <Input
                    type="text"
                    placeholder="Your Name"
                    className="mt-2"
                    required
                  />
                </div>
                <div>
                  <Label htmlFor="organization">Design Studio</Label>
                  <Input
                    type="text"
                    placeholder="Your Studio"
                    className="mt-2"
                    required
                  />
                </div>
                <div>
                  <Label htmlFor="position">Role</Label>
                  <Input
                    type="text"
                    placeholder="Creative Director"
                    className="mt-2"
                    required
                  />
                </div>
                <div>
                  <Label htmlFor="businessEmail">Work Email</Label>
                  <Input
                    type="email"
                    placeholder="name@designstudio.com"
                    className="mt-2"
                    required
                  />
                </div>
              </div>
              <div>
                <Label>Project Budget (select one)</Label>
                <div className="mt-2 flex justify-between">
                  <div className="space-y-2">
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>{"<$50k"}</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$50k – $250k</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$250k – $1M</Label>
                    </div>
                  </div>
                  <div className="space-y-2">
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$1M – $5M</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>$5M – $10M</Label>
                    </div>
                    <div className="flex items-center gap-2">
                      <Checkbox className="h-6 w-6 rounded-full" />
                      <Label>{">$10M"}</Label>
                    </div>
                  </div>
                </div>
                <Separator className="my-6 w-full" />
                <div>
                  <Label>
                    Which design field do you represent? (select all that apply)
                  </Label>
                  <div className="mt-2 space-y-2">
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>Graphic Design</Label>
                    </div>
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>UI/UX Design</Label>
                    </div>
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>Product Design</Label>
                    </div>
                    <div className="flex items-center space-x-2">
                      <Checkbox className="mt-2 h-6 w-6" />
                      <Label>Other</Label>
                    </div>
                  </div>
                </div>
              </div>
              <Separator className="my-6 w-full" />
              <div>
                <Label>Tell us about your design needs</Label>
                <Textarea
                  placeholder="Describe your project vision..."
                  className="mt-4 h-30"
                />
              </div>
              <Separator className="my-6 w-full" />
              <div className="flex space-x-2">
                <Checkbox />
                <p className="text-muted-foreground text-xs">
                  By checking this box, you agree to receive design insights,
                  updates, and inspiration from our team. See our Privacy Policy
                  and Terms & Conditions. You can unsubscribe anytime.
                </p>
              </div>
              <Button>
                <ArrowRight />
                Start Your Design Journey
              </Button>
            </form>
            <div className="mt-6 flex h-full w-full flex-col space-y-6 border-t p-10 lg:border-t-0">
              <div className="">
                <Apple size={80} />
                <p className="mt-10 italic">
                  “Collaborating with our design partners elevates Apple’s
                  vision for innovative and sustainable design, creating lasting
                  impact for our brand and community.”
                </p>
                <p className="mt-10 font-semibold">Ali Imam // Apple</p>
                <p>Chief Design Officer</p>
              </div>
              <Separator className="my-10 w-full" />
              <p>Trusted by global leaders in design and innovation</p>
              <div className="flex justify-start">
                <div className="grid grid-cols-2 border p-2 md:grid-cols-3">
                  {[
                    <OpenAI key="openai" size={40} />,
                    <ClaudeAI key="claude" size={40} />,
                    <Replicate key="replicate" size={40} />,
                    <Cursor key="cursor" size={40} />,
                    <GoogleGemini key="gemini" size={40} />,
                    <Github key="github" size={40} />,
                  ].map((Logo, i) => (
                    <div
                      key={i}
                      className="flex h-30 w-30 items-center justify-center border md:h-40 md:w-40"
                    >
                      {Logo}
                    </div>
                  ))}
                </div>
              </div>
            </div>
          </div>
          <div className="w-full border border-t-0 p-20 text-center">
            <h3 className="text-3xl font-semibold md:text-5xl">
              Ready to elevate <br /> your design impact?
            </h3>
            <div className="mt-6 flex flex-wrap justify-center gap-3">
              <Button variant="outline">Get in Touch</Button>
              <Button>
                <ArrowRight />
                Explore Our Design Services
              </Button>
            </div>
          </div>
        </div>
      </div>
    </section>
  )
}

export default Bookademo4
```
---

### Contact Us 01 <a name="contact-us-01"></a>

A simple contact-us page.

- **Registry Name**: `aliimam/contact-us-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/contact-us-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `input`
  - `textarea`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
---

### Cta 01 <a name="cta-01"></a>

A simple cta section.

- **Registry Name**: `aliimam/cta-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/cta-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
  - `@aliimam/logos`
- **Registry Dependencies**:
  - `button`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `cta.tsx`

```tsx
import Link from "next/link"
import {
  SectionHeader,
  SectionHeaderDescription,
  SectionHeaderHeading,
} from "@/src/components/layout/page-header"
import { WhatsApp } from "@aliimam/logos"

import { Button } from "@/registry/aliimam/ui/button"

import { WhatsAppJoin } from "../../icons"

export function CallToAction() {
  return (
    <section className="relative container overflow-hidden">
      <div className="relative overflow-hidden border-x pb-20">
        {/* Content */}
        <div className="relative z-10 space-y-3 text-center">
          <SectionHeader>
            <SectionHeaderHeading>Start Your Design Today</SectionHeaderHeading>
            <SectionHeaderDescription>
              Bring your brand and ideas to life with professional design
              services. Whether it’s branding, UI/UX, or marketing materials,
              our team is ready to help.
            </SectionHeaderDescription>
          </SectionHeader>

          <div className="h-full">
            <div className="flex flex-col items-center justify-center gap-3">
              <WhatsAppJoin className="h-40 w-full" />
            </div>
          </div>

          <div className="flex flex-wrap justify-center gap-3 pt-6">
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              <Button className="h-12 w-40 cursor-pointer">Book a Call</Button>
            </Link>
            <Link href="/blocks">
              <Button variant={"outline"} className="h-12 w-40 cursor-pointer">
                <WhatsApp /> Start Chat
              </Button>
            </Link>
          </div>
        </div><h1
          className="w-full text-center tracking-tighter border-y mt-10 font-black 
             text-[clamp(1rem,18vw,20rem)] leading-none"
        >
          HIRE ME
        </h1>
      </div>
    </section>
  )
}
```
---

### Cta 02 <a name="cta-02"></a>

A simple cta section.

- **Registry Name**: `aliimam/cta-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/cta-02.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`
- **Registry Dependencies**:
  - `button`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `cta.tsx`

```tsx
import Link from "next/link"
import {
  SectionHeader,
  SectionHeaderDescription,
  SectionHeaderHeading,
} from "@/src/components/layout/page-header"
import { WhatsApp } from "@aliimam/logos"

import { Button } from "@/registry/aliimam/ui/button"

import { WhatsAppJoin } from "../../icons"

export function CallToAction() {
  return (
    <section className="relative container overflow-hidden">
      <div className="relative overflow-hidden border-x pb-20">
        {/* Content */}
        <div className="relative z-10 space-y-3 text-center">
          <SectionHeader>
            <SectionHeaderHeading>Start Your Design Today</SectionHeaderHeading>
            <SectionHeaderDescription>
              Bring your brand and ideas to life with professional design
              services. Whether it’s branding, UI/UX, or marketing materials,
              our team is ready to help.
            </SectionHeaderDescription>
          </SectionHeader>

          <div className="h-full">
            <div className="flex flex-col items-center justify-center gap-3">
              <WhatsAppJoin className="h-40 w-full" />
            </div>
          </div>

          <div className="flex flex-wrap justify-center gap-3 pt-6">
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              <Button className="h-12 w-40 cursor-pointer">Book a Call</Button>
            </Link>
            <Link href="/blocks">
              <Button variant={"outline"} className="h-12 w-40 cursor-pointer">
                <WhatsApp /> Start Chat
              </Button>
            </Link>
          </div>
        </div><h1
          className="w-full text-center tracking-tighter border-y mt-10 font-black 
             text-[clamp(1rem,18vw,20rem)] leading-none"
        >
          HIRE ME
        </h1>
      </div>
    </section>
  )
}
```
---

### Download 01 <a name="download-01"></a>

A simple download section.

- **Registry Name**: `aliimam/download-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/download-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
  - `@aliimam/logos`
- **Registry Dependencies**:
  - `button`
  - `separator`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Props & API Reference

```typescript
export interface DownloadProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}
```

#### Component Source Code

##### File: `download.tsx`

```tsx
/** Auto-generated - Do not edit */
'use client';
import React from 'react';

export interface DownloadProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}

export const Download = React.forwardRef<SVGSVGElement, DownloadProps>(
  ({ size = 24, className = '', strokeWidth = 1, ...props }, ref) => (
    <svg 
      ref={ref}
      width={size}
      height={size} 
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      className={className}
      xmlns="http://www.w3.org/2000/svg"
      {...(strokeWidth !== undefined ? { strokeWidth } : {})}
      {...props}
    >
      <path d="M12 15V3" />
  <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4" />
  <path d="m7 10 5 5 5-5" />
    </svg>
  )
);
Download.displayName = "Download";
export const DownloadMetadata = { 
  id: "download", 
  baseId: "download", 
  variant: "default", 
  name: "Download", 
  category: "others", 
  tags: [], 
  viewBox: "0 0 24 24" 
} as const;

export default Download;
```
---

### Download 02 <a name="download-02"></a>

A simple download section.

- **Registry Name**: `aliimam/download-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/download-02.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
  - `@aliimam/logos`
- **Registry Dependencies**:
  - `button`
  - `input`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Props & API Reference

```typescript
export interface DownloadProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}
```

#### Component Source Code

##### File: `download.tsx`

```tsx
/** Auto-generated - Do not edit */
'use client';
import React from 'react';

export interface DownloadProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}

export const Download = React.forwardRef<SVGSVGElement, DownloadProps>(
  ({ size = 24, className = '', strokeWidth = 1, ...props }, ref) => (
    <svg 
      ref={ref}
      width={size}
      height={size} 
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      className={className}
      xmlns="http://www.w3.org/2000/svg"
      {...(strokeWidth !== undefined ? { strokeWidth } : {})}
      {...props}
    >
      <path d="M12 15V3" />
  <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4" />
  <path d="m7 10 5 5 5-5" />
    </svg>
  )
);
Download.displayName = "Download";
export const DownloadMetadata = { 
  id: "download", 
  baseId: "download", 
  variant: "default", 
  name: "Download", 
  category: "others", 
  tags: [], 
  viewBox: "0 0 24 24" 
} as const;

export default Download;
```
---

### Faq 01 <a name="faq-01"></a>

A simple faq section.

- **Registry Name**: `aliimam/faq-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/faq-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `accordion`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `faq.tsx`

```tsx
"use client"

import {
  SectionHeader,
  SectionHeaderDescription,
  SectionHeaderHeading,
} from "@/src/components/layout/page-header"

import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/registry/aliimam/ui/accordion"

export function FAQs() {
  const faqItems = [
    {
      id: "item-1",
      question: "How fast will I receive my designs?",
      answer:
        "Most design requests are completed within 24–48 hours. Larger or more complex requests may take longer, but I provide daily progress updates so you always know what’s happening.",
    },
    {
      id: "item-2",
      question: "Who will be working on my designs?",
      answer:
        "You’ll work directly with me, Ali — the founder and sole designer. I don’t outsource or use a team, so every design is personally crafted with full attention.",
    },
    {
      id: "item-3",
      question: "What type of design work do you offer?",
      answer:
        "I work across all kinds of design, including branding, UI/UX, social media creatives, marketing designs, and website visuals.",
    },
    {
      id: "item-4",
      question: "Is there a limit to how many requests I can make?",
      answer:
        "There’s no limit on total requests. Depending on your plan, I work on a fixed number of active requests at a time. Once one is done, the next begins.",
    },
    {
      id: "item-5",
      question: "Do you offer unlimited revisions?",
      answer:
        "Yes. Unlimited revisions are included. I’ll keep refining the design until you’re fully satisfied.",
    },
    {
      id: "item-6",
      question: "How do you handle large or complex projects?",
      answer:
        "Large projects are broken into smaller tasks or phases to ensure quality, clarity, and timely delivery.",
    },
    {
      id: "item-7",
      question: "How will I receive updates on my work?",
      answer:
        "I share daily updates on what was completed that day, so you’re always aware of progress.",
    },
    {
      id: "item-8",
      question: "How do we communicate?",
      answer:
        "For smooth and fast communication, I’m available on WhatsApp. Other tools can also be used if preferred.",
    },
    {
      id: "item-9",
      question: "How do I submit design requests?",
      answer:
        "You can submit requests through WhatsApp, Figma, Google Docs, Notion, Trello, or Loom — whatever is easiest for you.",
    },
    {
      id: "item-10",
      question: "What if I don’t like the design?",
      answer:
        "That’s completely fine. We’ll revise it until it matches your vision. Feedback is a core part of the process.",
    },
    {
      id: "item-11",
      question: "Are there any design requests you don’t support?",
      answer:
        "I focus on digital and brand-related design. If something is outside this scope, we can discuss it before starting.",
    },
    {
      id: "item-12",
      question: "How does the subscription work?",
      answer:
        "To get started, simply book a call. We’ll discuss your needs, choose the right plan, and begin right away.",
    },
    {
      id: "item-13",
      question: "Can I subscribe for just one month?",
      answer:
        "Yes. The subscription is flexible — you can subscribe monthly, pause, or cancel anytime.",
    },
  ]

  return (
    <section className="relative container">
      <div className="border">
        <div className="mx-auto max-w-5xl lg:border-x">
          <SectionHeader>
            <SectionHeaderHeading>
              Frequently Asked Questions
            </SectionHeaderHeading>
            <SectionHeaderDescription>
              Discover quick and comprehensive answers to common questions about
              our platform, services, and features.
            </SectionHeaderDescription>
          </SectionHeader>
          <Accordion
            type="single"
            collapsible
            className="-mb-1"
            defaultValue="item-1"
          >
            {faqItems.map((item) => (
              <AccordionItem
                key={item.id}
                value={item.id}
                className="space-y-1 border-none"
              >
                <AccordionTrigger className="group flex w-full justify-end py-0 hover:no-underline [&_svg]:hidden">
                  <div className="bg-primary text-primary-foreground max-w-[80%] cursor-pointer px-4 py-3 text-left text-base transition">
                    {item.question}
                  </div>
                </AccordionTrigger>

                <AccordionContent className="flex justify-start">
                  <div className="bg-muted text-muted-foreground max-w-[80%] px-4 py-3 text-base">
                    {item.answer}
                  </div>
                </AccordionContent>
              </AccordionItem>
            ))}
          </Accordion>
        </div>
      </div>
    </section>
  )
}
```
---

### Faq 02 <a name="faq-02"></a>

A simple faq section.

- **Registry Name**: `aliimam/faq-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/faq-02.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `accordion`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `faq.tsx`

```tsx
"use client"

import {
  SectionHeader,
  SectionHeaderDescription,
  SectionHeaderHeading,
} from "@/src/components/layout/page-header"

import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/registry/aliimam/ui/accordion"

export function FAQs() {
  const faqItems = [
    {
      id: "item-1",
      question: "How fast will I receive my designs?",
      answer:
        "Most design requests are completed within 24–48 hours. Larger or more complex requests may take longer, but I provide daily progress updates so you always know what’s happening.",
    },
    {
      id: "item-2",
      question: "Who will be working on my designs?",
      answer:
        "You’ll work directly with me, Ali — the founder and sole designer. I don’t outsource or use a team, so every design is personally crafted with full attention.",
    },
    {
      id: "item-3",
      question: "What type of design work do you offer?",
      answer:
        "I work across all kinds of design, including branding, UI/UX, social media creatives, marketing designs, and website visuals.",
    },
    {
      id: "item-4",
      question: "Is there a limit to how many requests I can make?",
      answer:
        "There’s no limit on total requests. Depending on your plan, I work on a fixed number of active requests at a time. Once one is done, the next begins.",
    },
    {
      id: "item-5",
      question: "Do you offer unlimited revisions?",
      answer:
        "Yes. Unlimited revisions are included. I’ll keep refining the design until you’re fully satisfied.",
    },
    {
      id: "item-6",
      question: "How do you handle large or complex projects?",
      answer:
        "Large projects are broken into smaller tasks or phases to ensure quality, clarity, and timely delivery.",
    },
    {
      id: "item-7",
      question: "How will I receive updates on my work?",
      answer:
        "I share daily updates on what was completed that day, so you’re always aware of progress.",
    },
    {
      id: "item-8",
      question: "How do we communicate?",
      answer:
        "For smooth and fast communication, I’m available on WhatsApp. Other tools can also be used if preferred.",
    },
    {
      id: "item-9",
      question: "How do I submit design requests?",
      answer:
        "You can submit requests through WhatsApp, Figma, Google Docs, Notion, Trello, or Loom — whatever is easiest for you.",
    },
    {
      id: "item-10",
      question: "What if I don’t like the design?",
      answer:
        "That’s completely fine. We’ll revise it until it matches your vision. Feedback is a core part of the process.",
    },
    {
      id: "item-11",
      question: "Are there any design requests you don’t support?",
      answer:
        "I focus on digital and brand-related design. If something is outside this scope, we can discuss it before starting.",
    },
    {
      id: "item-12",
      question: "How does the subscription work?",
      answer:
        "To get started, simply book a call. We’ll discuss your needs, choose the right plan, and begin right away.",
    },
    {
      id: "item-13",
      question: "Can I subscribe for just one month?",
      answer:
        "Yes. The subscription is flexible — you can subscribe monthly, pause, or cancel anytime.",
    },
  ]

  return (
    <section className="relative container">
      <div className="border">
        <div className="mx-auto max-w-5xl lg:border-x">
          <SectionHeader>
            <SectionHeaderHeading>
              Frequently Asked Questions
            </SectionHeaderHeading>
            <SectionHeaderDescription>
              Discover quick and comprehensive answers to common questions about
              our platform, services, and features.
            </SectionHeaderDescription>
          </SectionHeader>
          <Accordion
            type="single"
            collapsible
            className="-mb-1"
            defaultValue="item-1"
          >
            {faqItems.map((item) => (
              <AccordionItem
                key={item.id}
                value={item.id}
                className="space-y-1 border-none"
              >
                <AccordionTrigger className="group flex w-full justify-end py-0 hover:no-underline [&_svg]:hidden">
                  <div className="bg-primary text-primary-foreground max-w-[80%] cursor-pointer px-4 py-3 text-left text-base transition">
                    {item.question}
                  </div>
                </AccordionTrigger>

                <AccordionContent className="flex justify-start">
                  <div className="bg-muted text-muted-foreground max-w-[80%] px-4 py-3 text-base">
                    {item.answer}
                  </div>
                </AccordionContent>
              </AccordionItem>
            ))}
          </Accordion>
        </div>
      </div>
    </section>
  )
}
```
---

### Feature 01 <a name="feature-01"></a>

A simple feature demo section.

- **Registry Name**: `aliimam/feature-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/feature-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `separator`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
---

### Feature 02 <a name="feature-02"></a>

A simple feature demo section.

- **Registry Name**: `aliimam/feature-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/feature-02.json
```

#### Dependencies

- **Registry Dependencies**:
  - `dot-pattern`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
##### File: `timeline.tsx`

```tsx
"use client"

import type { ReactNode } from "react"
import * as LucideIcons from "lucide-react"

import { cn } from "@/registry/aliimam/lib/utils"

export function TimelineContainer({ children }: { children: ReactNode }) {
  return (
    <div className="mx-auto flex max-w-md flex-col justify-center gap-3 md:order-2">
      {children}
    </div>
  )
}

export function TimelineEvent({
  label,
  message,
  icon,
  isLast = false,
}: Event & { isLast?: boolean }) {
  const Icon = LucideIcons[
    icon.name as keyof typeof LucideIcons
  ] as LucideIcons.LucideIcon // ✅ cast to LucideIcon

  return (
    <div className="group relative -m-2 flex gap-4 border border-transparent p-2">
      <div className="relative">
        <div
          className={cn(
            "bg-background rounded-full border p-2",
            icon.borderColor
          )}
        >
          <Icon className={cn("h-4 w-4", icon.textColor)} />
        </div>
        {!isLast ? (
          <div className="bg-muted absolute inset-x-0 mx-auto h-full w-[2px]" />
        ) : null}
      </div>
      <div className="mt-1 flex flex-1 flex-col gap-1">
        <div className="flex items-center justify-between gap-4">
          <p className="text-lg font-semibold">{label}</p>
        </div>
        <p className="text-muted-foreground text-xs">{message}</p>
      </div>
    </div>
  )
}

export function Timeline() {
  return (
    <div className="">
      <TimelineContainer>
        {timeline.map((event, i) => (
          <TimelineEvent
            key={event.message}
            isLast={i === timeline.length - 1}
            {...event}
          />
        ))}
      </TimelineContainer>
    </div>
  )
}

interface Event {
  label: string
  message: string
  icon: {
    name: keyof typeof LucideIcons // ✅ type-safe icon names
    textColor: string
    borderColor: string
  }
}

const timeline: Event[] = [
  {
    label: "Choose Your Design",
    message:
      "Browse and select a design that fits your needs, then access your personalized dashboard.",
    icon: {
      name: "Shapes",
      textColor: "text-orange-500",
      borderColor: "border-orange-500/40",
    },
  },
  {
    label: "Provide Your Brief",
    message: "Share your design preferences and requirements with us.",
    icon: {
      name: "Send",
      textColor: "text-amber-500",
      borderColor: "border-amber-500/40",
    },
  },
  {
    label: "Receive Your Designs",
    message: "Get your initial designs within 48 hours.",
    icon: {
      name: "Check",
      textColor: "text-blue-500",
      borderColor: "border-blue-500/40",
    },
  },
  {
    label: "Request Revisions",
    message:
      "We’re committed to perfection—request as many revisions as needed until you’re satisfied.",
    icon: {
      name: "Repeat",
      textColor: "text-green-500",
      borderColor: "border-green-500/40",
    },
  },
  {
    label: "Get Final Files",
    message: "Once approved, we’ll deliver the final files to you.",
    icon: {
      name: "Download",
      textColor: "text-green-500",
      borderColor: "border-green-500/40",
    },
  },
]
```
---

### Feature 03 <a name="feature-03"></a>

A simple feature demo section.

- **Registry Name**: `aliimam/feature-03`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/feature-03.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `shine-border`
  - `button`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
---

### Footer 01 <a name="footer-01"></a>

A simple footer.

- **Registry Name**: `aliimam/footer-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/footer-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
  - `@aliimam/logos`
  - `next-themes`
- **Registry Dependencies**:
  - `button`
  - `dropdown-menu`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Props & API Reference

```typescript
export type ThemeSwitcherProps = {
  value?: "light" | "dark" | "system"
  onChange?: (theme: "light" | "dark" | "system") => void
  defaultValue?: "light" | "dark" | "system"
  className?: string
}
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
##### File: `footer.tsx`

```tsx
import Link from "next/link"
import { ChevronDown, SquareArrowOutUpRight } from "@aliimam/icons"
import { Github, LinkedIn, Vercel, X, YouTube } from "@aliimam/logos"

import { ThemeSwitcher } from "@/registry/aliimam/pages/home/home-01/layout/theme"
import { Button } from "@/registry/aliimam/ui/button"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/registry/aliimam/ui/dropdown-menu"

const linksPro = [
  {
    group: "Product",
    items: [
      {
        title: "AI",
        href: "#",
      },
      {
        title: "Enterprise",
        href: "#",
      },
      {
        title: "Fluid Compute",
        href: "#",
      },
      {
        title: "Next.js",
        href: "#",
      },
      {
        title: "Observability",
        href: "#",
      },
      {
        title: "Previews",
        href: "#",
      },
      {
        title: "Rendering",
        href: "#",
      },
      {
        title: "Security",
        href: "#",
      },
      {
        title: "Turbo",
        href: "#",
      },
      {
        title: "Domains",
        href: "#",
      },
    ],
  },
]

const linksRes = [
  {
    group: "Resources",
    items: [
      {
        title: "Docs",
        href: "#",
      },
      {
        title: "Guides",
        href: "#",
      },
      {
        title: "Academy",
        href: "#",
      },
      {
        title: "Help",
        href: "#",
      },
      {
        title: "Integrations",
        href: "#",
      },
      {
        title: "Pricing",
        href: "#",
      },
      {
        title: "Resources",
        href: "#",
      },
      {
        title: "Solution Partners",
        href: "#",
      },
      {
        title: "Startups",
        href: "#",
      },
      {
        title: "Templates",
        href: "#",
      },
    ],
  },
]

const linksCom = [
  {
    group: "Company",
    items: [
      {
        title: "About",
        href: "#",
      },
      {
        title: "Blog",
        href: "#",
      },
      {
        title: "Careers",
        href: "#",
      },
      {
        title: "Changelog",
        href: "#",
      },
      {
        title: "Contact Us",
        href: "#",
      },
      {
        title: "Customers",
        href: "#",
      },
      {
        title: "Events",
        href: "#",
      },
      {
        title: "Partners",
        href: "#",
      },
      {
        title: "Shipped",
        href: "#",
      },
      {
        title: "Privacy Policy",
        href: "#",
      },
    ],
  },
]

export function Footer() {
  return (
    <footer className="py-16">
      <div className="mx-auto max-w-6xl px-6">
        <div className="">
          <div className="grid grid-cols-2 gap-14 md:grid-cols-3 lg:grid-cols-5">
            <div className="grid gap-3">
              {linksPro.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>
                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <Link
                    href={"#"}
                    className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm duration-150"
                  >
                    v0
                    <SquareArrowOutUpRight strokeWidth={2} size={16} />
                  </Link>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              {linksRes.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>
                  <Link
                    href={"#"}
                    className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm duration-150"
                  >
                    Community
                    <SquareArrowOutUpRight strokeWidth={2} size={16} />
                  </Link>
                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <DropdownMenu>
                    <DropdownMenuTrigger className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm">
                      SDKs by Vercel <ChevronDown strokeWidth={2} size={16} />
                    </DropdownMenuTrigger>
                    <DropdownMenuContent
                      side="top"
                      align="end"
                      className="w-60 p-1"
                    >
                      <DropdownMenuItem className="h-10 px-4">
                        AI SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Flags SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Chat SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Streamdown AI{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                    </DropdownMenuContent>
                  </DropdownMenu>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              {linksCom.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>

                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <DropdownMenu>
                    <DropdownMenuTrigger className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm">
                      Legal <ChevronDown strokeWidth={2} size={16} />
                    </DropdownMenuTrigger>
                    <DropdownMenuContent
                      side="top"
                      align="end"
                      className="w-60 p-1"
                    >
                      <DropdownMenuItem className="h-10 px-4">
                        Cookie Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Cookie Preferences
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DMCA Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DORA Addendum
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DPA
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-16 px-4">
                        Domain Name Registration and Services Terms
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Event Code of Conduct
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Event Terms and Conditions
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Inactivity Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Job Applicant Privacy Notice
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Privacy Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        SLA
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Sub-processors
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Support Terms
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Terms of Service
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Trademark Policy
                      </DropdownMenuItem>
                    </DropdownMenuContent>
                  </DropdownMenu>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              <div className="space-y-3 text-sm">
                <span className="block font-medium">Social</span>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <Github size={14} />
                    Github
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <LinkedIn className="grayscale" size={14} />
                    LinkedIn
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <X size={14} />
                    Twitter
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <YouTube className="grayscale" size={14} />
                    YouTube
                  </span>
                </Link>
              </div>
            </div>
            <div className="flex justify-end">
              <Vercel size={18} />
            </div>
          </div>
        </div>
        <div className="mt-10 flex flex-wrap items-end justify-between gap-6 py-6">
          <Button
            className="cursor-pointer text-blue-500 hover:text-blue-500"
            variant={"ghost"}
          >
            <span className="border-background block size-3 rounded-full border bg-blue-500" />
            All systems normal.
          </Button>
          <ThemeSwitcher />
        </div>
      </div>
    </footer>
  )
}
```
##### File: `theme.tsx`

```tsx
"use client"

import { useCallback, useEffect, useState } from "react"
import { Monitor, Moon, Sun } from "@aliimam/icons"
import { useTheme } from "next-themes"

import { cn } from "@/registry/aliimam/lib/utils"

const themes = [
  {
    key: "system",
    icon: Monitor,
    label: "System theme",
  },
  {
    key: "light",
    icon: Sun,
    label: "Light theme",
  },
  {
    key: "dark",
    icon: Moon,
    label: "Dark theme",
  },
]

export type ThemeSwitcherProps = {
  value?: "light" | "dark" | "system"
  onChange?: (theme: "light" | "dark" | "system") => void
  defaultValue?: "light" | "dark" | "system"
  className?: string
}

export const ThemeSwitcher = ({ className }: ThemeSwitcherProps) => {
  const { theme, setTheme } = useTheme()
  const [mounted, setMounted] = useState(false)

  const handleThemeClick = useCallback(
    (themeKey: "light" | "dark" | "system") => {
      setTheme(themeKey)
    },
    [setTheme]
  )

  // Prevent hydration mismatch
  useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) {
    return null
  }

  return (
    <div
      className={cn(
        "bg-background ring-border relative isolate flex h-7 rounded-full p-1 ring-1",
        className
      )}
    >
      {themes.map(({ key, icon: Icon, label }) => {
        const isActive = theme === key

        return (
          <button
            aria-label={label}
            className="relative h-5 w-6 rounded-full"
            key={key}
            onClick={() => handleThemeClick(key as "light" | "dark" | "system")}
            type="button"
          >
            {isActive && (
              <div className="bg-secondary absolute inset-0 rounded-full" />
            )}
            <Icon
              className={cn(
                "relative z-10 m-auto h-3.5 w-3.5",
                isActive ? "text-foreground" : "text-muted-foreground"
              )}
            />
          </button>
        )
      })}
    </div>
  )
}
```
---

### Footer 02 <a name="footer-02"></a>

A simple footer.

- **Registry Name**: `aliimam/footer-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/footer-02.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
  - `@aliimam/logos`
  - `next-themes`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Props & API Reference

```typescript
export type ThemeSwitcherProps = {
  value?: "light" | "dark" | "system"
  onChange?: (theme: "light" | "dark" | "system") => void
  defaultValue?: "light" | "dark" | "system"
  className?: string
}
```

#### Component Source Code

##### File: `demo.tsx`

```tsx
"use client"

import Link from "next/link"
import { Behance } from "@/src/components/logos"
import { Facebook, Heart, Mail } from "@aliimam/icons"
import {
  Apple,
  Instagram,
  LinkedIn,
  Threads,
  WhatsApp,
  X,
  YouTube,
} from "@aliimam/logos"

import { Theme } from "@/registry/aliimam/blocks/footer/footer-02/theme"

const navigation = {
  categories: [
    {
      id: "women",
      name: "Women",

      sections: [
        {
          id: "about",
          name: "About",
          items: [
            { name: "About", href: "/about" },
            { name: "Works", href: "/agency/works" },
            { name: "Pricing", href: "/pricing" },
          ],
        },
        {
          id: "features",
          name: "Features",
          items: [
            { name: "Products", href: "/products" },
            { name: "Agency", href: "/agency" },
            { name: "Dashboard", href: "/dashboard" },
          ],
        },
        {
          id: "products",
          name: "Products",
          items: [
            { name: "DIcons", href: "/products/dicons" },
            { name: "DShapes", href: "/products/dshapes" },
            { name: "Graaadients", href: "/products/graaadients" },
          ],
        },
        {
          id: "designs",
          name: "Designs",
          items: [
            { name: "Design", href: "/designs" },
            { name: "Components", href: "/components" },
            { name: "Blogs", href: "/blogs" },
          ],
        },
        {
          id: "other",
          name: "Others",
          items: [
            { name: "Graphic", href: "/graphic" },
            { name: "3D Icons", href: "/products/3dicons" },
            { name: "Colors", href: "/products/colors/generate" },
          ],
        },
        {
          id: "company",
          name: "Company",
          items: [
            { name: "Contact", href: "/contact" },
            { name: "Terms", href: "/terms" },
            { name: "Privacy", href: "/privacy" },
          ],
        },
      ],
    },
  ],
}

const Underline = `hover:-translate-y-1 border border-dotted rounded-xl p-2.5 transition-transform `

export default function Footer() {
  return (
    <footer className="mx-auto mt-20 flex h-full w-full flex-col items-center justify-center">
      <div className="relative mx-auto grid max-w-7xl items-center justify-center gap-6 p-10 pb-0 md:flex">
        <Link href="/">
          <p className="flex items-center justify-center rounded-full">
            <Apple className="text-primary w-8" />
          </p>
        </Link>
        <p className="text-muted-foreground text-center text-xs leading-4 md:text-left">
          Welcome to Designali, where creativity meets strategy to bring your
          vision to life. I am passionate about transforming ideas into
          compelling visual experiences. I specialize in crafting unique brand
          identities, immersive digital experiences, and engaging content that
          resonates with your audience. My mission is to empower businesses and
          brands to stand out in a crowded market. I believe in the power of
          design to tell stories, evoke emotions, and drive meaningful
          connections. I believe in quality, not quantity. Designali is actually
          an agency of one. This means you&apos;ll work directly with me,
          founder of Designali.
        </p>
      </div>

      <div className="mx-auto w-full max-w-7xl px-6 py-10">
        <div className="border-b border-dotted"> </div>
        <div className="py-10">
          {navigation.categories.map((category) => (
            <div
              key={category.name}
              className="grid grid-cols-3 flex-row justify-between gap-6 leading-6 md:flex"
            >
              {category.sections.map((section) => (
                <div key={section.name}>
                  <ul
                    role="list"
                    aria-labelledby={`${category.id}-${section.id}-heading-mobile`}
                    className="flex flex-col space-y-2"
                  >
                    {section.items.map((item) => (
                      <li key={item.name} className="flow-root">
                        <Link
                          href={item.href}
                          className="text-sm text-slate-600 hover:text-black md:text-xs dark:text-slate-400 hover:dark:text-white"
                        >
                          {item.name}
                        </Link>
                      </li>
                    ))}
                  </ul>
                </div>
              ))}
            </div>
          ))}
        </div>
        <div className="border-b border-dotted"> </div>
      </div>

      <div className="flex flex-wrap justify-center gap-y-6">
        <div className="flex flex-wrap items-center justify-center gap-6 gap-y-4 px-6">
          <Link
            aria-label="Logo"
            href="mailto:contact@designali.in"
            rel="noreferrer"
            target="_blank"
            className={Underline}
          >
            <Mail strokeWidth={1.5} className="h-5 w-5" />
          </Link>
          <Link
            aria-label="Logo"
            href="https://x.com/designali_in"
            rel="noreferrer"
            target="_blank"
            className={Underline}
          >
            <X className="h-5 w-5" />
          </Link>
          <Link
            aria-label="Logo"
            href="https://www.instagram.com/designali.in/"
            rel="noreferrer"
            target="_blank"
            className={Underline}
          >
            <Instagram className="h-5 w-5" />
          </Link>
          <Link
            aria-label="Logo"
            href="https://www.threads.net/designali.in"
            rel="noreferrer"
            target="_blank"
            className={Underline}
          >
            <Threads className="h-5 w-5" />
          </Link>
          <Link
            aria-label="Logo"
            href="https://chat.whatsapp.com/LWsNPcz5BlWDVOha41vzuh"
            rel="noreferrer"
            target="_blank"
            className={Underline}
          >
            <WhatsApp className="h-5 w-5" />
          </Link>
          <Link
            aria-label="Logo"
            href="https://www.behance.net/designali-in"
            rel="noreferrer"
            target="_blank"
            className={Underline}
          >
            <Behance className="h-5 w-5" />
          </Link>
          <Link
            aria-label="Logo"
            href="https://www.facebook.com/designali.agency"
            rel="noreferrer"
            target="_blank"
            className={Underline}
          >
            <Facebook className="h-5 w-5" />
          </Link>
          <Link
            aria-label="Logo"
            href="https://www.linkedin.com/company/designali"
            rel="noreferrer"
            target="_blank"
            className={Underline}
          >
            <LinkedIn className="h-5 w-5" />
          </Link>
          <Link
            aria-label="Logo"
            href="https://www.youtube.com/@designali-in"
            rel="noreferrer"
            target="_blank"
            className={Underline}
          >
            <YouTube className="h-5 w-5" />
          </Link>
        </div>
        <Theme />
      </div>

      <div className="mx-auto mt-10 mb-10 flex flex-col justify-between text-center text-xs md:max-w-7xl">
        <div className="flex flex-row items-center justify-center gap-1 text-slate-600 dark:text-slate-400">
          <span> © </span>
          <span>{new Date().getFullYear()}</span>
          <span>Made with</span>
          <Heart className="mx-1 h-4 w-4 animate-pulse text-red-600" />
          <span> by </span>
          <span className="hover:text-ali dark:hover:text-ali cursor-pointer text-black dark:text-white">
            <Link
              aria-label="Logo"
              className="font-bold"
              href="https://www.instagram.com/aliimam.in/"
              target="_blank"
            >
              Ali Imam {""}
            </Link>
          </span>
          -
          <span className="hover:text-ali cursor-pointer text-slate-600 dark:text-slate-400 dark:hover:text-red-600">
            <Link aria-label="Logo" className="" href="/">
              Designali
            </Link>
          </span>
        </div>
      </div>
    </footer>
  )
}
```
##### File: `theme.tsx`

```tsx
"use client"

import { useCallback, useEffect, useState } from "react"
import { Monitor, Moon, Sun } from "@aliimam/icons"
import { useTheme } from "next-themes"

import { cn } from "@/registry/aliimam/lib/utils"

const themes = [
  {
    key: "system",
    icon: Monitor,
    label: "System theme",
  },
  {
    key: "light",
    icon: Sun,
    label: "Light theme",
  },
  {
    key: "dark",
    icon: Moon,
    label: "Dark theme",
  },
]

export type ThemeSwitcherProps = {
  value?: "light" | "dark" | "system"
  onChange?: (theme: "light" | "dark" | "system") => void
  defaultValue?: "light" | "dark" | "system"
  className?: string
}

export const ThemeSwitcher = ({ className }: ThemeSwitcherProps) => {
  const { theme, setTheme } = useTheme()
  const [mounted, setMounted] = useState(false)

  const handleThemeClick = useCallback(
    (themeKey: "light" | "dark" | "system") => {
      setTheme(themeKey)
    },
    [setTheme]
  )

  // Prevent hydration mismatch
  useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) {
    return null
  }

  return (
    <div
      className={cn(
        "bg-background ring-border relative isolate flex h-7 rounded-full p-1 ring-1",
        className
      )}
    >
      {themes.map(({ key, icon: Icon, label }) => {
        const isActive = theme === key

        return (
          <button
            aria-label={label}
            className="relative h-5 w-6 rounded-full"
            key={key}
            onClick={() => handleThemeClick(key as "light" | "dark" | "system")}
            type="button"
          >
            {isActive && (
              <div className="bg-secondary absolute inset-0 rounded-full" />
            )}
            <Icon
              className={cn(
                "relative z-10 m-auto h-3.5 w-3.5",
                isActive ? "text-foreground" : "text-muted-foreground"
              )}
            />
          </button>
        )
      })}
    </div>
  )
}
```
---

### Footer 03 <a name="footer-03"></a>

A simple footer.

- **Registry Name**: `aliimam/footer-03`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/footer-03.json
```

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `footer.tsx`

```tsx
import Link from "next/link"
import { ChevronDown, SquareArrowOutUpRight } from "@aliimam/icons"
import { Github, LinkedIn, Vercel, X, YouTube } from "@aliimam/logos"

import { ThemeSwitcher } from "@/registry/aliimam/pages/home/home-01/layout/theme"
import { Button } from "@/registry/aliimam/ui/button"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/registry/aliimam/ui/dropdown-menu"

const linksPro = [
  {
    group: "Product",
    items: [
      {
        title: "AI",
        href: "#",
      },
      {
        title: "Enterprise",
        href: "#",
      },
      {
        title: "Fluid Compute",
        href: "#",
      },
      {
        title: "Next.js",
        href: "#",
      },
      {
        title: "Observability",
        href: "#",
      },
      {
        title: "Previews",
        href: "#",
      },
      {
        title: "Rendering",
        href: "#",
      },
      {
        title: "Security",
        href: "#",
      },
      {
        title: "Turbo",
        href: "#",
      },
      {
        title: "Domains",
        href: "#",
      },
    ],
  },
]

const linksRes = [
  {
    group: "Resources",
    items: [
      {
        title: "Docs",
        href: "#",
      },
      {
        title: "Guides",
        href: "#",
      },
      {
        title: "Academy",
        href: "#",
      },
      {
        title: "Help",
        href: "#",
      },
      {
        title: "Integrations",
        href: "#",
      },
      {
        title: "Pricing",
        href: "#",
      },
      {
        title: "Resources",
        href: "#",
      },
      {
        title: "Solution Partners",
        href: "#",
      },
      {
        title: "Startups",
        href: "#",
      },
      {
        title: "Templates",
        href: "#",
      },
    ],
  },
]

const linksCom = [
  {
    group: "Company",
    items: [
      {
        title: "About",
        href: "#",
      },
      {
        title: "Blog",
        href: "#",
      },
      {
        title: "Careers",
        href: "#",
      },
      {
        title: "Changelog",
        href: "#",
      },
      {
        title: "Contact Us",
        href: "#",
      },
      {
        title: "Customers",
        href: "#",
      },
      {
        title: "Events",
        href: "#",
      },
      {
        title: "Partners",
        href: "#",
      },
      {
        title: "Shipped",
        href: "#",
      },
      {
        title: "Privacy Policy",
        href: "#",
      },
    ],
  },
]

export function Footer() {
  return (
    <footer className="py-16">
      <div className="mx-auto max-w-6xl px-6">
        <div className="">
          <div className="grid grid-cols-2 gap-14 md:grid-cols-3 lg:grid-cols-5">
            <div className="grid gap-3">
              {linksPro.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>
                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <Link
                    href={"#"}
                    className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm duration-150"
                  >
                    v0
                    <SquareArrowOutUpRight strokeWidth={2} size={16} />
                  </Link>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              {linksRes.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>
                  <Link
                    href={"#"}
                    className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm duration-150"
                  >
                    Community
                    <SquareArrowOutUpRight strokeWidth={2} size={16} />
                  </Link>
                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <DropdownMenu>
                    <DropdownMenuTrigger className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm">
                      SDKs by Vercel <ChevronDown strokeWidth={2} size={16} />
                    </DropdownMenuTrigger>
                    <DropdownMenuContent
                      side="top"
                      align="end"
                      className="w-60 p-1"
                    >
                      <DropdownMenuItem className="h-10 px-4">
                        AI SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Flags SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Chat SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Streamdown AI{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                    </DropdownMenuContent>
                  </DropdownMenu>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              {linksCom.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>

                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <DropdownMenu>
                    <DropdownMenuTrigger className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm">
                      Legal <ChevronDown strokeWidth={2} size={16} />
                    </DropdownMenuTrigger>
                    <DropdownMenuContent
                      side="top"
                      align="end"
                      className="w-60 p-1"
                    >
                      <DropdownMenuItem className="h-10 px-4">
                        Cookie Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Cookie Preferences
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DMCA Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DORA Addendum
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DPA
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-16 px-4">
                        Domain Name Registration and Services Terms
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Event Code of Conduct
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Event Terms and Conditions
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Inactivity Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Job Applicant Privacy Notice
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Privacy Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        SLA
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Sub-processors
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Support Terms
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Terms of Service
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Trademark Policy
                      </DropdownMenuItem>
                    </DropdownMenuContent>
                  </DropdownMenu>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              <div className="space-y-3 text-sm">
                <span className="block font-medium">Social</span>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <Github size={14} />
                    Github
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <LinkedIn className="grayscale" size={14} />
                    LinkedIn
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <X size={14} />
                    Twitter
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <YouTube className="grayscale" size={14} />
                    YouTube
                  </span>
                </Link>
              </div>
            </div>
            <div className="flex justify-end">
              <Vercel size={18} />
            </div>
          </div>
        </div>
        <div className="mt-10 flex flex-wrap items-end justify-between gap-6 py-6">
          <Button
            className="cursor-pointer text-blue-500 hover:text-blue-500"
            variant={"ghost"}
          >
            <span className="border-background block size-3 rounded-full border bg-blue-500" />
            All systems normal.
          </Button>
          <ThemeSwitcher />
        </div>
      </div>
    </footer>
  )
}
```
---

### Footer 04 <a name="footer-04"></a>

A simple footer.

- **Registry Name**: `aliimam/footer-04`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/footer-04.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
  - `@aliimam/logos`
  - `next-themes`
- **Registry Dependencies**:
  - `button`
  - `switch`
  - `select`
  - `input`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Props & API Reference

```typescript
export type ThemeSwitcherProps = {
  value?: "light" | "dark" | "system"
  onChange?: (theme: "light" | "dark" | "system") => void
  defaultValue?: "light" | "dark" | "system"
  className?: string
}
```

#### Component Source Code

##### File: `footer.tsx`

```tsx
import Link from "next/link"
import { ChevronDown, SquareArrowOutUpRight } from "@aliimam/icons"
import { Github, LinkedIn, Vercel, X, YouTube } from "@aliimam/logos"

import { ThemeSwitcher } from "@/registry/aliimam/pages/home/home-01/layout/theme"
import { Button } from "@/registry/aliimam/ui/button"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/registry/aliimam/ui/dropdown-menu"

const linksPro = [
  {
    group: "Product",
    items: [
      {
        title: "AI",
        href: "#",
      },
      {
        title: "Enterprise",
        href: "#",
      },
      {
        title: "Fluid Compute",
        href: "#",
      },
      {
        title: "Next.js",
        href: "#",
      },
      {
        title: "Observability",
        href: "#",
      },
      {
        title: "Previews",
        href: "#",
      },
      {
        title: "Rendering",
        href: "#",
      },
      {
        title: "Security",
        href: "#",
      },
      {
        title: "Turbo",
        href: "#",
      },
      {
        title: "Domains",
        href: "#",
      },
    ],
  },
]

const linksRes = [
  {
    group: "Resources",
    items: [
      {
        title: "Docs",
        href: "#",
      },
      {
        title: "Guides",
        href: "#",
      },
      {
        title: "Academy",
        href: "#",
      },
      {
        title: "Help",
        href: "#",
      },
      {
        title: "Integrations",
        href: "#",
      },
      {
        title: "Pricing",
        href: "#",
      },
      {
        title: "Resources",
        href: "#",
      },
      {
        title: "Solution Partners",
        href: "#",
      },
      {
        title: "Startups",
        href: "#",
      },
      {
        title: "Templates",
        href: "#",
      },
    ],
  },
]

const linksCom = [
  {
    group: "Company",
    items: [
      {
        title: "About",
        href: "#",
      },
      {
        title: "Blog",
        href: "#",
      },
      {
        title: "Careers",
        href: "#",
      },
      {
        title: "Changelog",
        href: "#",
      },
      {
        title: "Contact Us",
        href: "#",
      },
      {
        title: "Customers",
        href: "#",
      },
      {
        title: "Events",
        href: "#",
      },
      {
        title: "Partners",
        href: "#",
      },
      {
        title: "Shipped",
        href: "#",
      },
      {
        title: "Privacy Policy",
        href: "#",
      },
    ],
  },
]

export function Footer() {
  return (
    <footer className="py-16">
      <div className="mx-auto max-w-6xl px-6">
        <div className="">
          <div className="grid grid-cols-2 gap-14 md:grid-cols-3 lg:grid-cols-5">
            <div className="grid gap-3">
              {linksPro.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>
                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <Link
                    href={"#"}
                    className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm duration-150"
                  >
                    v0
                    <SquareArrowOutUpRight strokeWidth={2} size={16} />
                  </Link>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              {linksRes.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>
                  <Link
                    href={"#"}
                    className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm duration-150"
                  >
                    Community
                    <SquareArrowOutUpRight strokeWidth={2} size={16} />
                  </Link>
                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <DropdownMenu>
                    <DropdownMenuTrigger className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm">
                      SDKs by Vercel <ChevronDown strokeWidth={2} size={16} />
                    </DropdownMenuTrigger>
                    <DropdownMenuContent
                      side="top"
                      align="end"
                      className="w-60 p-1"
                    >
                      <DropdownMenuItem className="h-10 px-4">
                        AI SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Flags SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Chat SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Streamdown AI{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                    </DropdownMenuContent>
                  </DropdownMenu>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              {linksCom.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>

                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <DropdownMenu>
                    <DropdownMenuTrigger className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm">
                      Legal <ChevronDown strokeWidth={2} size={16} />
                    </DropdownMenuTrigger>
                    <DropdownMenuContent
                      side="top"
                      align="end"
                      className="w-60 p-1"
                    >
                      <DropdownMenuItem className="h-10 px-4">
                        Cookie Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Cookie Preferences
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DMCA Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DORA Addendum
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DPA
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-16 px-4">
                        Domain Name Registration and Services Terms
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Event Code of Conduct
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Event Terms and Conditions
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Inactivity Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Job Applicant Privacy Notice
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Privacy Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        SLA
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Sub-processors
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Support Terms
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Terms of Service
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Trademark Policy
                      </DropdownMenuItem>
                    </DropdownMenuContent>
                  </DropdownMenu>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              <div className="space-y-3 text-sm">
                <span className="block font-medium">Social</span>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <Github size={14} />
                    Github
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <LinkedIn className="grayscale" size={14} />
                    LinkedIn
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <X size={14} />
                    Twitter
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <YouTube className="grayscale" size={14} />
                    YouTube
                  </span>
                </Link>
              </div>
            </div>
            <div className="flex justify-end">
              <Vercel size={18} />
            </div>
          </div>
        </div>
        <div className="mt-10 flex flex-wrap items-end justify-between gap-6 py-6">
          <Button
            className="cursor-pointer text-blue-500 hover:text-blue-500"
            variant={"ghost"}
          >
            <span className="border-background block size-3 rounded-full border bg-blue-500" />
            All systems normal.
          </Button>
          <ThemeSwitcher />
        </div>
      </div>
    </footer>
  )
}
```
##### File: `theme.tsx`

```tsx
"use client"

import { useCallback, useEffect, useState } from "react"
import { Monitor, Moon, Sun } from "@aliimam/icons"
import { useTheme } from "next-themes"

import { cn } from "@/registry/aliimam/lib/utils"

const themes = [
  {
    key: "system",
    icon: Monitor,
    label: "System theme",
  },
  {
    key: "light",
    icon: Sun,
    label: "Light theme",
  },
  {
    key: "dark",
    icon: Moon,
    label: "Dark theme",
  },
]

export type ThemeSwitcherProps = {
  value?: "light" | "dark" | "system"
  onChange?: (theme: "light" | "dark" | "system") => void
  defaultValue?: "light" | "dark" | "system"
  className?: string
}

export const ThemeSwitcher = ({ className }: ThemeSwitcherProps) => {
  const { theme, setTheme } = useTheme()
  const [mounted, setMounted] = useState(false)

  const handleThemeClick = useCallback(
    (themeKey: "light" | "dark" | "system") => {
      setTheme(themeKey)
    },
    [setTheme]
  )

  // Prevent hydration mismatch
  useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) {
    return null
  }

  return (
    <div
      className={cn(
        "bg-background ring-border relative isolate flex h-7 rounded-full p-1 ring-1",
        className
      )}
    >
      {themes.map(({ key, icon: Icon, label }) => {
        const isActive = theme === key

        return (
          <button
            aria-label={label}
            className="relative h-5 w-6 rounded-full"
            key={key}
            onClick={() => handleThemeClick(key as "light" | "dark" | "system")}
            type="button"
          >
            {isActive && (
              <div className="bg-secondary absolute inset-0 rounded-full" />
            )}
            <Icon
              className={cn(
                "relative z-10 m-auto h-3.5 w-3.5",
                isActive ? "text-foreground" : "text-muted-foreground"
              )}
            />
          </button>
        )
      })}
    </div>
  )
}
```
---

### Footer 05 <a name="footer-05"></a>

A simple footer.

- **Registry Name**: `aliimam/footer-05`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/footer-05.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `footer.tsx`

```tsx
import Link from "next/link"
import { ChevronDown, SquareArrowOutUpRight } from "@aliimam/icons"
import { Github, LinkedIn, Vercel, X, YouTube } from "@aliimam/logos"

import { ThemeSwitcher } from "@/registry/aliimam/pages/home/home-01/layout/theme"
import { Button } from "@/registry/aliimam/ui/button"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/registry/aliimam/ui/dropdown-menu"

const linksPro = [
  {
    group: "Product",
    items: [
      {
        title: "AI",
        href: "#",
      },
      {
        title: "Enterprise",
        href: "#",
      },
      {
        title: "Fluid Compute",
        href: "#",
      },
      {
        title: "Next.js",
        href: "#",
      },
      {
        title: "Observability",
        href: "#",
      },
      {
        title: "Previews",
        href: "#",
      },
      {
        title: "Rendering",
        href: "#",
      },
      {
        title: "Security",
        href: "#",
      },
      {
        title: "Turbo",
        href: "#",
      },
      {
        title: "Domains",
        href: "#",
      },
    ],
  },
]

const linksRes = [
  {
    group: "Resources",
    items: [
      {
        title: "Docs",
        href: "#",
      },
      {
        title: "Guides",
        href: "#",
      },
      {
        title: "Academy",
        href: "#",
      },
      {
        title: "Help",
        href: "#",
      },
      {
        title: "Integrations",
        href: "#",
      },
      {
        title: "Pricing",
        href: "#",
      },
      {
        title: "Resources",
        href: "#",
      },
      {
        title: "Solution Partners",
        href: "#",
      },
      {
        title: "Startups",
        href: "#",
      },
      {
        title: "Templates",
        href: "#",
      },
    ],
  },
]

const linksCom = [
  {
    group: "Company",
    items: [
      {
        title: "About",
        href: "#",
      },
      {
        title: "Blog",
        href: "#",
      },
      {
        title: "Careers",
        href: "#",
      },
      {
        title: "Changelog",
        href: "#",
      },
      {
        title: "Contact Us",
        href: "#",
      },
      {
        title: "Customers",
        href: "#",
      },
      {
        title: "Events",
        href: "#",
      },
      {
        title: "Partners",
        href: "#",
      },
      {
        title: "Shipped",
        href: "#",
      },
      {
        title: "Privacy Policy",
        href: "#",
      },
    ],
  },
]

export function Footer() {
  return (
    <footer className="py-16">
      <div className="mx-auto max-w-6xl px-6">
        <div className="">
          <div className="grid grid-cols-2 gap-14 md:grid-cols-3 lg:grid-cols-5">
            <div className="grid gap-3">
              {linksPro.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>
                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <Link
                    href={"#"}
                    className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm duration-150"
                  >
                    v0
                    <SquareArrowOutUpRight strokeWidth={2} size={16} />
                  </Link>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              {linksRes.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>
                  <Link
                    href={"#"}
                    className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm duration-150"
                  >
                    Community
                    <SquareArrowOutUpRight strokeWidth={2} size={16} />
                  </Link>
                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <DropdownMenu>
                    <DropdownMenuTrigger className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm">
                      SDKs by Vercel <ChevronDown strokeWidth={2} size={16} />
                    </DropdownMenuTrigger>
                    <DropdownMenuContent
                      side="top"
                      align="end"
                      className="w-60 p-1"
                    >
                      <DropdownMenuItem className="h-10 px-4">
                        AI SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Flags SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Chat SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Streamdown AI{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                    </DropdownMenuContent>
                  </DropdownMenu>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              {linksCom.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>

                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <DropdownMenu>
                    <DropdownMenuTrigger className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm">
                      Legal <ChevronDown strokeWidth={2} size={16} />
                    </DropdownMenuTrigger>
                    <DropdownMenuContent
                      side="top"
                      align="end"
                      className="w-60 p-1"
                    >
                      <DropdownMenuItem className="h-10 px-4">
                        Cookie Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Cookie Preferences
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DMCA Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DORA Addendum
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DPA
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-16 px-4">
                        Domain Name Registration and Services Terms
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Event Code of Conduct
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Event Terms and Conditions
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Inactivity Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Job Applicant Privacy Notice
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Privacy Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        SLA
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Sub-processors
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Support Terms
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Terms of Service
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Trademark Policy
                      </DropdownMenuItem>
                    </DropdownMenuContent>
                  </DropdownMenu>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              <div className="space-y-3 text-sm">
                <span className="block font-medium">Social</span>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <Github size={14} />
                    Github
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <LinkedIn className="grayscale" size={14} />
                    LinkedIn
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <X size={14} />
                    Twitter
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <YouTube className="grayscale" size={14} />
                    YouTube
                  </span>
                </Link>
              </div>
            </div>
            <div className="flex justify-end">
              <Vercel size={18} />
            </div>
          </div>
        </div>
        <div className="mt-10 flex flex-wrap items-end justify-between gap-6 py-6">
          <Button
            className="cursor-pointer text-blue-500 hover:text-blue-500"
            variant={"ghost"}
          >
            <span className="border-background block size-3 rounded-full border bg-blue-500" />
            All systems normal.
          </Button>
          <ThemeSwitcher />
        </div>
      </div>
    </footer>
  )
}
```
---

### Footer 06 <a name="footer-06"></a>

A simple footer.

- **Registry Name**: `aliimam/footer-06`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/footer-06.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `footer.tsx`

```tsx
import Link from "next/link"
import { ChevronDown, SquareArrowOutUpRight } from "@aliimam/icons"
import { Github, LinkedIn, Vercel, X, YouTube } from "@aliimam/logos"

import { ThemeSwitcher } from "@/registry/aliimam/pages/home/home-01/layout/theme"
import { Button } from "@/registry/aliimam/ui/button"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/registry/aliimam/ui/dropdown-menu"

const linksPro = [
  {
    group: "Product",
    items: [
      {
        title: "AI",
        href: "#",
      },
      {
        title: "Enterprise",
        href: "#",
      },
      {
        title: "Fluid Compute",
        href: "#",
      },
      {
        title: "Next.js",
        href: "#",
      },
      {
        title: "Observability",
        href: "#",
      },
      {
        title: "Previews",
        href: "#",
      },
      {
        title: "Rendering",
        href: "#",
      },
      {
        title: "Security",
        href: "#",
      },
      {
        title: "Turbo",
        href: "#",
      },
      {
        title: "Domains",
        href: "#",
      },
    ],
  },
]

const linksRes = [
  {
    group: "Resources",
    items: [
      {
        title: "Docs",
        href: "#",
      },
      {
        title: "Guides",
        href: "#",
      },
      {
        title: "Academy",
        href: "#",
      },
      {
        title: "Help",
        href: "#",
      },
      {
        title: "Integrations",
        href: "#",
      },
      {
        title: "Pricing",
        href: "#",
      },
      {
        title: "Resources",
        href: "#",
      },
      {
        title: "Solution Partners",
        href: "#",
      },
      {
        title: "Startups",
        href: "#",
      },
      {
        title: "Templates",
        href: "#",
      },
    ],
  },
]

const linksCom = [
  {
    group: "Company",
    items: [
      {
        title: "About",
        href: "#",
      },
      {
        title: "Blog",
        href: "#",
      },
      {
        title: "Careers",
        href: "#",
      },
      {
        title: "Changelog",
        href: "#",
      },
      {
        title: "Contact Us",
        href: "#",
      },
      {
        title: "Customers",
        href: "#",
      },
      {
        title: "Events",
        href: "#",
      },
      {
        title: "Partners",
        href: "#",
      },
      {
        title: "Shipped",
        href: "#",
      },
      {
        title: "Privacy Policy",
        href: "#",
      },
    ],
  },
]

export function Footer() {
  return (
    <footer className="py-16">
      <div className="mx-auto max-w-6xl px-6">
        <div className="">
          <div className="grid grid-cols-2 gap-14 md:grid-cols-3 lg:grid-cols-5">
            <div className="grid gap-3">
              {linksPro.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>
                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <Link
                    href={"#"}
                    className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm duration-150"
                  >
                    v0
                    <SquareArrowOutUpRight strokeWidth={2} size={16} />
                  </Link>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              {linksRes.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>
                  <Link
                    href={"#"}
                    className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm duration-150"
                  >
                    Community
                    <SquareArrowOutUpRight strokeWidth={2} size={16} />
                  </Link>
                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <DropdownMenu>
                    <DropdownMenuTrigger className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm">
                      SDKs by Vercel <ChevronDown strokeWidth={2} size={16} />
                    </DropdownMenuTrigger>
                    <DropdownMenuContent
                      side="top"
                      align="end"
                      className="w-60 p-1"
                    >
                      <DropdownMenuItem className="h-10 px-4">
                        AI SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Flags SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Chat SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Streamdown AI{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                    </DropdownMenuContent>
                  </DropdownMenu>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              {linksCom.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>

                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <DropdownMenu>
                    <DropdownMenuTrigger className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm">
                      Legal <ChevronDown strokeWidth={2} size={16} />
                    </DropdownMenuTrigger>
                    <DropdownMenuContent
                      side="top"
                      align="end"
                      className="w-60 p-1"
                    >
                      <DropdownMenuItem className="h-10 px-4">
                        Cookie Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Cookie Preferences
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DMCA Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DORA Addendum
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DPA
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-16 px-4">
                        Domain Name Registration and Services Terms
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Event Code of Conduct
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Event Terms and Conditions
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Inactivity Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Job Applicant Privacy Notice
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Privacy Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        SLA
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Sub-processors
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Support Terms
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Terms of Service
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Trademark Policy
                      </DropdownMenuItem>
                    </DropdownMenuContent>
                  </DropdownMenu>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              <div className="space-y-3 text-sm">
                <span className="block font-medium">Social</span>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <Github size={14} />
                    Github
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <LinkedIn className="grayscale" size={14} />
                    LinkedIn
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <X size={14} />
                    Twitter
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <YouTube className="grayscale" size={14} />
                    YouTube
                  </span>
                </Link>
              </div>
            </div>
            <div className="flex justify-end">
              <Vercel size={18} />
            </div>
          </div>
        </div>
        <div className="mt-10 flex flex-wrap items-end justify-between gap-6 py-6">
          <Button
            className="cursor-pointer text-blue-500 hover:text-blue-500"
            variant={"ghost"}
          >
            <span className="border-background block size-3 rounded-full border bg-blue-500" />
            All systems normal.
          </Button>
          <ThemeSwitcher />
        </div>
      </div>
    </footer>
  )
}
```
---

### Header 01 <a name="header-01"></a>

A simple header section.

- **Registry Name**: `aliimam/header-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/header-01.json
```

#### Dependencies

- **NPM Packages**:
  - `next-themes`
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `navigation-menu`
  - `toggle`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
##### File: `header.tsx`

```tsx
import { Button } from "@/registry/aliimam/ui/button"

export function Header() {
  return (
    <div className="absolute top-0 left-0 z-20 flex h-12 w-full items-center justify-center px-6 sm:h-14 sm:px-8 md:h-16 md:px-12 lg:px-0">
      <div className="border-muted absolute top-6 left-0 h-0 w-full border-t sm:top-7 md:top-8"></div>

      <div className="bg-muted relative z-30 flex h-10 w-full max-w-[calc(100%-32px)] items-center justify-between overflow-hidden rounded-md border px-3 py-1.5 pr-2 backdrop-blur-sm sm:h-11 sm:max-w-[calc(100%-48px)] sm:px-4 sm:py-2 sm:pr-3 md:h-12 md:max-w-[calc(100%-64px)] md:px-2 lg:w-[700px] lg:max-w-[700px]">
        <div className="flex items-center justify-center">
          <div className="flex items-center justify-start">
            <div className="flex flex-col justify-center pl-2 text-sm leading-5 font-medium sm:text-base md:text-lg lg:text-xl">
              AI
            </div>
          </div>
          <div className="flex flex-row items-start justify-start gap-2 pl-3 sm:flex sm:gap-3 sm:pl-4 md:gap-4 md:pl-5 lg:gap-4 lg:pl-5">
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Designs
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Pricing
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Docs
              </div>
            </div>
          </div>
        </div>
        <Button size={"sm"}>Log in</Button>
      </div>
    </div>
  )
}
```
##### File: `theme-switch.tsx`

```tsx
"use client"

import * as React from "react"
import { useEffect, useState } from "react"
import { Moon, Sun } from "@aliimam/icons"
import { useTheme } from "next-themes"

import { Toggle } from "@/registry/aliimam/ui/toggle"

export function ModeToggle() {
  const { theme, setTheme } = useTheme()
  const [mounted, setMounted] = useState(false)

  useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) return null

  return (
    <div className="flex flex-col justify-center">
      <div>
        <Toggle
          className="group bg-secondary dark:bg-secondary data-[state=on]:hover:bg-muted size-9 cursor-pointer data-[state=on]:bg-transparent"
          pressed={theme === "dark"}
          onPressedChange={() => setTheme(theme === "dark" ? "light" : "dark")}
          aria-label={`Switch to ${theme === "dark" ? "light" : "dark"} mode`}
        >
          <Moon
            size={16}
            className="shrink-0 scale-0 opacity-0 transition-all group-data-[state=on]:scale-100 group-data-[state=on]:opacity-100"
            aria-hidden="true"
          />
          <Sun
            size={16}
            className="absolute shrink-0 scale-100 opacity-100 transition-all group-data-[state=on]:scale-0 group-data-[state=on]:opacity-0"
            aria-hidden="true"
          />
        </Toggle>
      </div>
    </div>
  )
}
```
##### File: `menus.tsx`

```tsx
"use client"

import * as React from "react"
import { useState } from "react"

import {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuItem,
  NavigationMenuList,
  NavigationMenuTrigger,
} from "@/registry/aliimam/blocks/header/header-05/components/navigation"

export function Menus() {
  type MenuName =
    | "design"
    | "dev"
    | "learning"
    | "community"
    | "resources"
    | "tools"
    | "services"
    | "company"
    | null

  const [activeMenu, setActiveMenu] = useState<MenuName>(null)

  const handleMouseEnter = (menuName: MenuName) => setActiveMenu(menuName)
  const handleMouseLeave = () => setActiveMenu(null)

  return (
    <div className="hidden md:block">
      <NavigationMenu>
        <NavigationMenuList>
          {/* 1. Design */}
          <NavigationMenuItem
            onMouseEnter={() => handleMouseEnter("design")}
            onMouseLeave={handleMouseLeave}
          >
            <NavigationMenuTrigger isActive={activeMenu === "design"}>
              Design
            </NavigationMenuTrigger>
            <NavigationMenuContent isOpen={activeMenu === "design"}>
              <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                <li>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      UI/UX
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Branding
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Prototypes
                    </div>
                  </a>
                </li>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          {/* 2. Development */}
          <NavigationMenuItem
            onMouseEnter={() => handleMouseEnter("dev")}
            onMouseLeave={handleMouseLeave}
          >
            <NavigationMenuTrigger isActive={activeMenu === "dev"}>
              Development
            </NavigationMenuTrigger>
            <NavigationMenuContent isOpen={activeMenu === "dev"}>
              <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                <li>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Frontend
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Backend
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      DevOps
                    </div>
                  </a>
                </li>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          {/* 3. Learning */}
          <NavigationMenuItem
            onMouseEnter={() => handleMouseEnter("learning")}
            onMouseLeave={handleMouseLeave}
          >
            <NavigationMenuTrigger isActive={activeMenu === "learning"}>
              Learning
            </NavigationMenuTrigger>
            <NavigationMenuContent isOpen={activeMenu === "learning"}>
              <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                <li>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Tutorials
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Courses
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Blogs
                    </div>
                  </a>
                </li>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          {/* 4. Community */}
          <NavigationMenuItem
            onMouseEnter={() => handleMouseEnter("community")}
            onMouseLeave={handleMouseLeave}
          >
            <NavigationMenuTrigger isActive={activeMenu === "community"}>
              Community
            </NavigationMenuTrigger>
            <NavigationMenuContent isOpen={activeMenu === "community"}>
              <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                <li>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Forums
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Discord
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      GitHub
                    </div>
                  </a>
                </li>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          {/* 5. Resources */}
          <NavigationMenuItem
            onMouseEnter={() => handleMouseEnter("resources")}
            onMouseLeave={handleMouseLeave}
          >
            <NavigationMenuTrigger isActive={activeMenu === "resources"}>
              Resources
            </NavigationMenuTrigger>
            <NavigationMenuContent isOpen={activeMenu === "resources"}>
              <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                <li>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Documentation
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      API Reference
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Changelog
                    </div>
                  </a>
                </li>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          {/* 6. Tools */}
          <NavigationMenuItem
            onMouseEnter={() => handleMouseEnter("tools")}
            onMouseLeave={handleMouseLeave}
          >
            <NavigationMenuTrigger isActive={activeMenu === "tools"}>
              Tools
            </NavigationMenuTrigger>
            <NavigationMenuContent isOpen={activeMenu === "tools"}>
              <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                <li>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Figma
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Sketch
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Photoshop
                    </div>
                  </a>
                </li>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          <div className="hidden lg:flex">
            <NavigationMenuItem
              onMouseEnter={() => handleMouseEnter("services")}
              onMouseLeave={handleMouseLeave}
            >
              <NavigationMenuTrigger isActive={activeMenu === "services"}>
                Services
              </NavigationMenuTrigger>
              <NavigationMenuContent isOpen={activeMenu === "services"}>
                <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                  <li>
                    <a href="#" className="group block">
                      <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                        Consulting
                      </div>
                    </a>
                    <a href="#" className="group block">
                      <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                        Workshops
                      </div>
                    </a>
                    <a href="#" className="group block">
                      <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                        Support
                      </div>
                    </a>
                  </li>
                </ul>
              </NavigationMenuContent>
            </NavigationMenuItem>

            <NavigationMenuItem
              onMouseEnter={() => handleMouseEnter("company")}
              onMouseLeave={handleMouseLeave}
            >
              <NavigationMenuTrigger isActive={activeMenu === "company"}>
                Company
              </NavigationMenuTrigger>
              <NavigationMenuContent isOpen={activeMenu === "company"}>
                <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                  <li>
                    <a href="#" className="group block">
                      <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                        About Us
                      </div>
                    </a>
                    <a href="#" className="group block">
                      <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                        Careers
                      </div>
                    </a>
                    <a href="#" className="group block">
                      <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                        Contact
                      </div>
                    </a>
                  </li>
                </ul>
              </NavigationMenuContent>
            </NavigationMenuItem>
          </div>
        </NavigationMenuList>
      </NavigationMenu>
    </div>
  )
}
```
---

### Header 02 <a name="header-02"></a>

A simple header section.

- **Registry Name**: `aliimam/header-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/header-02.json
```

#### Dependencies

- **NPM Packages**:
  - `next-themes`
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `command`
  - `navigation-menu`
  - `toggle`
  - `accordion`
  - `sheet`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Props & API Reference

```typescript
export interface SearchProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
##### File: `header.tsx`

```tsx
import { Button } from "@/registry/aliimam/ui/button"

export function Header() {
  return (
    <div className="absolute top-0 left-0 z-20 flex h-12 w-full items-center justify-center px-6 sm:h-14 sm:px-8 md:h-16 md:px-12 lg:px-0">
      <div className="border-muted absolute top-6 left-0 h-0 w-full border-t sm:top-7 md:top-8"></div>

      <div className="bg-muted relative z-30 flex h-10 w-full max-w-[calc(100%-32px)] items-center justify-between overflow-hidden rounded-md border px-3 py-1.5 pr-2 backdrop-blur-sm sm:h-11 sm:max-w-[calc(100%-48px)] sm:px-4 sm:py-2 sm:pr-3 md:h-12 md:max-w-[calc(100%-64px)] md:px-2 lg:w-[700px] lg:max-w-[700px]">
        <div className="flex items-center justify-center">
          <div className="flex items-center justify-start">
            <div className="flex flex-col justify-center pl-2 text-sm leading-5 font-medium sm:text-base md:text-lg lg:text-xl">
              AI
            </div>
          </div>
          <div className="flex flex-row items-start justify-start gap-2 pl-3 sm:flex sm:gap-3 sm:pl-4 md:gap-4 md:pl-5 lg:gap-4 lg:pl-5">
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Designs
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Pricing
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Docs
              </div>
            </div>
          </div>
        </div>
        <Button size={"sm"}>Log in</Button>
      </div>
    </div>
  )
}
```
##### File: `theme-switch.tsx`

```tsx
"use client"

import * as React from "react"
import { useEffect, useState } from "react"
import { Moon, Sun } from "@aliimam/icons"
import { useTheme } from "next-themes"

import { Toggle } from "@/registry/aliimam/ui/toggle"

export function ModeToggle() {
  const { theme, setTheme } = useTheme()
  const [mounted, setMounted] = useState(false)

  useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) return null

  return (
    <div className="flex flex-col justify-center">
      <div>
        <Toggle
          className="group bg-secondary dark:bg-secondary data-[state=on]:hover:bg-muted size-9 cursor-pointer data-[state=on]:bg-transparent"
          pressed={theme === "dark"}
          onPressedChange={() => setTheme(theme === "dark" ? "light" : "dark")}
          aria-label={`Switch to ${theme === "dark" ? "light" : "dark"} mode`}
        >
          <Moon
            size={16}
            className="shrink-0 scale-0 opacity-0 transition-all group-data-[state=on]:scale-100 group-data-[state=on]:opacity-100"
            aria-hidden="true"
          />
          <Sun
            size={16}
            className="absolute shrink-0 scale-100 opacity-100 transition-all group-data-[state=on]:scale-0 group-data-[state=on]:opacity-0"
            aria-hidden="true"
          />
        </Toggle>
      </div>
    </div>
  )
}
```
##### File: `search.tsx`

```tsx
/** Auto-generated - Do not edit */
'use client';
import React from 'react';

export interface SearchProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}

export const Search = React.forwardRef<SVGSVGElement, SearchProps>(
  ({ size = 24, className = '', strokeWidth = 1, ...props }, ref) => (
    <svg 
      ref={ref}
      width={size}
      height={size} 
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      className={className}
      xmlns="http://www.w3.org/2000/svg"
      {...(strokeWidth !== undefined ? { strokeWidth } : {})}
      {...props}
    >
      <path d="m21 21-4.34-4.34" />
  <circle cx="11" cy="11" r="8" />
    </svg>
  )
);
Search.displayName = "Search";
export const SearchMetadata = { 
  id: "search", 
  baseId: "search", 
  variant: "default", 
  name: "Search", 
  category: "others", 
  tags: [], 
  viewBox: "0 0 24 24" 
} as const;

export default Search;
```
---

### Header 03 <a name="header-03"></a>

A simple header section.

- **Registry Name**: `aliimam/header-03`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/header-03.json
```

#### Dependencies

- **NPM Packages**:
  - `next-themes`
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `navigation-menu`
  - `toggle`
  - `accordion`
  - `sheet`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
##### File: `header.tsx`

```tsx
import { Button } from "@/registry/aliimam/ui/button"

export function Header() {
  return (
    <div className="absolute top-0 left-0 z-20 flex h-12 w-full items-center justify-center px-6 sm:h-14 sm:px-8 md:h-16 md:px-12 lg:px-0">
      <div className="border-muted absolute top-6 left-0 h-0 w-full border-t sm:top-7 md:top-8"></div>

      <div className="bg-muted relative z-30 flex h-10 w-full max-w-[calc(100%-32px)] items-center justify-between overflow-hidden rounded-md border px-3 py-1.5 pr-2 backdrop-blur-sm sm:h-11 sm:max-w-[calc(100%-48px)] sm:px-4 sm:py-2 sm:pr-3 md:h-12 md:max-w-[calc(100%-64px)] md:px-2 lg:w-[700px] lg:max-w-[700px]">
        <div className="flex items-center justify-center">
          <div className="flex items-center justify-start">
            <div className="flex flex-col justify-center pl-2 text-sm leading-5 font-medium sm:text-base md:text-lg lg:text-xl">
              AI
            </div>
          </div>
          <div className="flex flex-row items-start justify-start gap-2 pl-3 sm:flex sm:gap-3 sm:pl-4 md:gap-4 md:pl-5 lg:gap-4 lg:pl-5">
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Designs
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Pricing
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Docs
              </div>
            </div>
          </div>
        </div>
        <Button size={"sm"}>Log in</Button>
      </div>
    </div>
  )
}
```
##### File: `theme-switch.tsx`

```tsx
"use client"

import * as React from "react"
import { useEffect, useState } from "react"
import { Moon, Sun } from "@aliimam/icons"
import { useTheme } from "next-themes"

import { Toggle } from "@/registry/aliimam/ui/toggle"

export function ModeToggle() {
  const { theme, setTheme } = useTheme()
  const [mounted, setMounted] = useState(false)

  useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) return null

  return (
    <div className="flex flex-col justify-center">
      <div>
        <Toggle
          className="group bg-secondary dark:bg-secondary data-[state=on]:hover:bg-muted size-9 cursor-pointer data-[state=on]:bg-transparent"
          pressed={theme === "dark"}
          onPressedChange={() => setTheme(theme === "dark" ? "light" : "dark")}
          aria-label={`Switch to ${theme === "dark" ? "light" : "dark"} mode`}
        >
          <Moon
            size={16}
            className="shrink-0 scale-0 opacity-0 transition-all group-data-[state=on]:scale-100 group-data-[state=on]:opacity-100"
            aria-hidden="true"
          />
          <Sun
            size={16}
            className="absolute shrink-0 scale-100 opacity-100 transition-all group-data-[state=on]:scale-0 group-data-[state=on]:opacity-0"
            aria-hidden="true"
          />
        </Toggle>
      </div>
    </div>
  )
}
```
---

### Header 04 <a name="header-04"></a>

A simple header section.

- **Registry Name**: `aliimam/header-04`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/header-04.json
```

#### Dependencies

- **NPM Packages**:
  - `next-themes`
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `command`
  - `toggle`
  - `separator`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Props & API Reference

```typescript
export interface SearchProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
##### File: `header.tsx`

```tsx
import { Button } from "@/registry/aliimam/ui/button"

export function Header() {
  return (
    <div className="absolute top-0 left-0 z-20 flex h-12 w-full items-center justify-center px-6 sm:h-14 sm:px-8 md:h-16 md:px-12 lg:px-0">
      <div className="border-muted absolute top-6 left-0 h-0 w-full border-t sm:top-7 md:top-8"></div>

      <div className="bg-muted relative z-30 flex h-10 w-full max-w-[calc(100%-32px)] items-center justify-between overflow-hidden rounded-md border px-3 py-1.5 pr-2 backdrop-blur-sm sm:h-11 sm:max-w-[calc(100%-48px)] sm:px-4 sm:py-2 sm:pr-3 md:h-12 md:max-w-[calc(100%-64px)] md:px-2 lg:w-[700px] lg:max-w-[700px]">
        <div className="flex items-center justify-center">
          <div className="flex items-center justify-start">
            <div className="flex flex-col justify-center pl-2 text-sm leading-5 font-medium sm:text-base md:text-lg lg:text-xl">
              AI
            </div>
          </div>
          <div className="flex flex-row items-start justify-start gap-2 pl-3 sm:flex sm:gap-3 sm:pl-4 md:gap-4 md:pl-5 lg:gap-4 lg:pl-5">
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Designs
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Pricing
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Docs
              </div>
            </div>
          </div>
        </div>
        <Button size={"sm"}>Log in</Button>
      </div>
    </div>
  )
}
```
##### File: `theme-switch.tsx`

```tsx
"use client"

import * as React from "react"
import { useEffect, useState } from "react"
import { Moon, Sun } from "@aliimam/icons"
import { useTheme } from "next-themes"

import { Toggle } from "@/registry/aliimam/ui/toggle"

export function ModeToggle() {
  const { theme, setTheme } = useTheme()
  const [mounted, setMounted] = useState(false)

  useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) return null

  return (
    <div className="flex flex-col justify-center">
      <div>
        <Toggle
          className="group bg-secondary dark:bg-secondary data-[state=on]:hover:bg-muted size-9 cursor-pointer data-[state=on]:bg-transparent"
          pressed={theme === "dark"}
          onPressedChange={() => setTheme(theme === "dark" ? "light" : "dark")}
          aria-label={`Switch to ${theme === "dark" ? "light" : "dark"} mode`}
        >
          <Moon
            size={16}
            className="shrink-0 scale-0 opacity-0 transition-all group-data-[state=on]:scale-100 group-data-[state=on]:opacity-100"
            aria-hidden="true"
          />
          <Sun
            size={16}
            className="absolute shrink-0 scale-100 opacity-100 transition-all group-data-[state=on]:scale-0 group-data-[state=on]:opacity-0"
            aria-hidden="true"
          />
        </Toggle>
      </div>
    </div>
  )
}
```
##### File: `search.tsx`

```tsx
/** Auto-generated - Do not edit */
'use client';
import React from 'react';

export interface SearchProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}

export const Search = React.forwardRef<SVGSVGElement, SearchProps>(
  ({ size = 24, className = '', strokeWidth = 1, ...props }, ref) => (
    <svg 
      ref={ref}
      width={size}
      height={size} 
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      className={className}
      xmlns="http://www.w3.org/2000/svg"
      {...(strokeWidth !== undefined ? { strokeWidth } : {})}
      {...props}
    >
      <path d="m21 21-4.34-4.34" />
  <circle cx="11" cy="11" r="8" />
    </svg>
  )
);
Search.displayName = "Search";
export const SearchMetadata = { 
  id: "search", 
  baseId: "search", 
  variant: "default", 
  name: "Search", 
  category: "others", 
  tags: [], 
  viewBox: "0 0 24 24" 
} as const;

export default Search;
```
---

### Header 05 <a name="header-05"></a>

A simple header section.

- **Registry Name**: `aliimam/header-05`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/header-05.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`
- **Registry Dependencies**:
  - `command`
  - `sheet`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Props & API Reference

```typescript
export interface NavigationProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}
```

```typescript
export interface SearchProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
##### File: `header.tsx`

```tsx
import { Button } from "@/registry/aliimam/ui/button"

export function Header() {
  return (
    <div className="absolute top-0 left-0 z-20 flex h-12 w-full items-center justify-center px-6 sm:h-14 sm:px-8 md:h-16 md:px-12 lg:px-0">
      <div className="border-muted absolute top-6 left-0 h-0 w-full border-t sm:top-7 md:top-8"></div>

      <div className="bg-muted relative z-30 flex h-10 w-full max-w-[calc(100%-32px)] items-center justify-between overflow-hidden rounded-md border px-3 py-1.5 pr-2 backdrop-blur-sm sm:h-11 sm:max-w-[calc(100%-48px)] sm:px-4 sm:py-2 sm:pr-3 md:h-12 md:max-w-[calc(100%-64px)] md:px-2 lg:w-[700px] lg:max-w-[700px]">
        <div className="flex items-center justify-center">
          <div className="flex items-center justify-start">
            <div className="flex flex-col justify-center pl-2 text-sm leading-5 font-medium sm:text-base md:text-lg lg:text-xl">
              AI
            </div>
          </div>
          <div className="flex flex-row items-start justify-start gap-2 pl-3 sm:flex sm:gap-3 sm:pl-4 md:gap-4 md:pl-5 lg:gap-4 lg:pl-5">
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Designs
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Pricing
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Docs
              </div>
            </div>
          </div>
        </div>
        <Button size={"sm"}>Log in</Button>
      </div>
    </div>
  )
}
```
##### File: `menus.tsx`

```tsx
"use client"

import * as React from "react"
import { useState } from "react"

import {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuItem,
  NavigationMenuList,
  NavigationMenuTrigger,
} from "@/registry/aliimam/blocks/header/header-05/components/navigation"

export function Menus() {
  type MenuName =
    | "design"
    | "dev"
    | "learning"
    | "community"
    | "resources"
    | "tools"
    | "services"
    | "company"
    | null

  const [activeMenu, setActiveMenu] = useState<MenuName>(null)

  const handleMouseEnter = (menuName: MenuName) => setActiveMenu(menuName)
  const handleMouseLeave = () => setActiveMenu(null)

  return (
    <div className="hidden md:block">
      <NavigationMenu>
        <NavigationMenuList>
          {/* 1. Design */}
          <NavigationMenuItem
            onMouseEnter={() => handleMouseEnter("design")}
            onMouseLeave={handleMouseLeave}
          >
            <NavigationMenuTrigger isActive={activeMenu === "design"}>
              Design
            </NavigationMenuTrigger>
            <NavigationMenuContent isOpen={activeMenu === "design"}>
              <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                <li>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      UI/UX
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Branding
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Prototypes
                    </div>
                  </a>
                </li>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          {/* 2. Development */}
          <NavigationMenuItem
            onMouseEnter={() => handleMouseEnter("dev")}
            onMouseLeave={handleMouseLeave}
          >
            <NavigationMenuTrigger isActive={activeMenu === "dev"}>
              Development
            </NavigationMenuTrigger>
            <NavigationMenuContent isOpen={activeMenu === "dev"}>
              <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                <li>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Frontend
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Backend
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      DevOps
                    </div>
                  </a>
                </li>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          {/* 3. Learning */}
          <NavigationMenuItem
            onMouseEnter={() => handleMouseEnter("learning")}
            onMouseLeave={handleMouseLeave}
          >
            <NavigationMenuTrigger isActive={activeMenu === "learning"}>
              Learning
            </NavigationMenuTrigger>
            <NavigationMenuContent isOpen={activeMenu === "learning"}>
              <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                <li>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Tutorials
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Courses
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Blogs
                    </div>
                  </a>
                </li>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          {/* 4. Community */}
          <NavigationMenuItem
            onMouseEnter={() => handleMouseEnter("community")}
            onMouseLeave={handleMouseLeave}
          >
            <NavigationMenuTrigger isActive={activeMenu === "community"}>
              Community
            </NavigationMenuTrigger>
            <NavigationMenuContent isOpen={activeMenu === "community"}>
              <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                <li>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Forums
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Discord
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      GitHub
                    </div>
                  </a>
                </li>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          {/* 5. Resources */}
          <NavigationMenuItem
            onMouseEnter={() => handleMouseEnter("resources")}
            onMouseLeave={handleMouseLeave}
          >
            <NavigationMenuTrigger isActive={activeMenu === "resources"}>
              Resources
            </NavigationMenuTrigger>
            <NavigationMenuContent isOpen={activeMenu === "resources"}>
              <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                <li>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Documentation
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      API Reference
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Changelog
                    </div>
                  </a>
                </li>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          {/* 6. Tools */}
          <NavigationMenuItem
            onMouseEnter={() => handleMouseEnter("tools")}
            onMouseLeave={handleMouseLeave}
          >
            <NavigationMenuTrigger isActive={activeMenu === "tools"}>
              Tools
            </NavigationMenuTrigger>
            <NavigationMenuContent isOpen={activeMenu === "tools"}>
              <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                <li>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Figma
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Sketch
                    </div>
                  </a>
                  <a href="#" className="group block">
                    <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                      Photoshop
                    </div>
                  </a>
                </li>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          <div className="hidden lg:flex">
            <NavigationMenuItem
              onMouseEnter={() => handleMouseEnter("services")}
              onMouseLeave={handleMouseLeave}
            >
              <NavigationMenuTrigger isActive={activeMenu === "services"}>
                Services
              </NavigationMenuTrigger>
              <NavigationMenuContent isOpen={activeMenu === "services"}>
                <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                  <li>
                    <a href="#" className="group block">
                      <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                        Consulting
                      </div>
                    </a>
                    <a href="#" className="group block">
                      <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                        Workshops
                      </div>
                    </a>
                    <a href="#" className="group block">
                      <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                        Support
                      </div>
                    </a>
                  </li>
                </ul>
              </NavigationMenuContent>
            </NavigationMenuItem>

            <NavigationMenuItem
              onMouseEnter={() => handleMouseEnter("company")}
              onMouseLeave={handleMouseLeave}
            >
              <NavigationMenuTrigger isActive={activeMenu === "company"}>
                Company
              </NavigationMenuTrigger>
              <NavigationMenuContent isOpen={activeMenu === "company"}>
                <ul className="grid w-full grid-cols-1 gap-6 md:grid-cols-5">
                  <li>
                    <a href="#" className="group block">
                      <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                        About Us
                      </div>
                    </a>
                    <a href="#" className="group block">
                      <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                        Careers
                      </div>
                    </a>
                    <a href="#" className="group block">
                      <div className="hover:text-background text-background/80 mb-3 text-xl font-semibold">
                        Contact
                      </div>
                    </a>
                  </li>
                </ul>
              </NavigationMenuContent>
            </NavigationMenuItem>
          </div>
        </NavigationMenuList>
      </NavigationMenu>
    </div>
  )
}
```
##### File: `phone-menus.tsx`

```tsx
const PhoneMenu = () => {
  const mainNav = [
    {
      title: "Products",
      items: [
        { label: "New Arrivals", href: "#" },
        { label: "Best Sellers", href: "#" },
        { label: "Trending Now", href: "#" },
        { label: "Gift Ideas", href: "#" },
      ],
    },
    {
      title: "Services",
      items: [
        { label: "Consulting", href: "#" },
        { label: "Custom Solutions", href: "#" },
        { label: "Training & Support", href: "#" },
        { label: "Maintenance Plans", href: "#" },
      ],
    },
    {
      title: "Company",
      items: [
        { label: "About Us", href: "#" },
        { label: "Careers", href: "#" },
        { label: "Press", href: "#" },
        { label: "Partners", href: "#" },
      ],
    },
    {
      title: "Resources",
      items: [
        { label: "Blog", href: "#" },
        { label: "Guides & Tutorials", href: "#" },
        { label: "Webinars", href: "#" },
        { label: "Documentation", href: "#" },
      ],
    },
    {
      title: "Support",
      items: [
        { label: "Help Center", href: "#" },
        { label: "Contact Support", href: "#" },
        { label: "FAQ", href: "#" },
        { label: "Community Forums", href: "#" },
      ],
    },
    {
      title: "Legal",
      items: [
        { label: "Privacy Policy", href: "#" },
        { label: "Terms of Service", href: "#" },
        { label: "Cookie Policy", href: "#" },
      ],
    },
    {
      title: "Locations",
      items: [
        { label: "Find a Store", href: "#" },
        { label: "International Offices", href: "#" },
        { label: "Events", href: "#" },
      ],
    },
    {
      title: "Contact",
      items: [
        { label: "Email Us", href: "#" },
        { label: "Request a Call", href: "#" },
        { label: "Live Chat", href: "#" },
      ],
    },
    {
      title: "Community",
      items: [
        { label: "Forums", href: "#" },
        { label: "Ambassadors", href: "#" },
        { label: "User Stories", href: "#" },
      ],
    },
    {
      title: "Extras",
      items: [
        { label: "Gift Cards", href: "#" },
        { label: "Promotions", href: "#" },
        { label: "Newsletter", href: "#" },
      ],
    },
  ]

  return (
    <nav className="flex flex-col py-6">
      {mainNav.map((item) => (
        <div key={item.title} className="border-b border-white/10">
          <button className="w-full px-6 py-3 text-left text-sm text-white transition-colors hover:bg-white/5">
            {item.title}
          </button>
        </div>
      ))}
    </nav>
  )
}

export { PhoneMenu }
```
##### File: `navigation.tsx`

```tsx
/** Auto-generated - Do not edit */
'use client';
import React from 'react';

export interface NavigationProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}

export const Navigation = React.forwardRef<SVGSVGElement, NavigationProps>(
  ({ size = 24, className = '', strokeWidth = 1, ...props }, ref) => (
    <svg 
      ref={ref}
      width={size}
      height={size} 
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      className={className}
      xmlns="http://www.w3.org/2000/svg"
      {...(strokeWidth !== undefined ? { strokeWidth } : {})}
      {...props}
    >
      <polygon points="3 11 22 2 13 21 11 13 3 11" />
    </svg>
  )
);
Navigation.displayName = "Navigation";
export const NavigationMetadata = { 
  id: "navigation", 
  baseId: "navigation", 
  variant: "default", 
  name: "Navigation", 
  category: "navigation", 
  tags: [], 
  viewBox: "0 0 24 24" 
} as const;

export default Navigation;
```
##### File: `search.tsx`

```tsx
/** Auto-generated - Do not edit */
'use client';
import React from 'react';

export interface SearchProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}

export const Search = React.forwardRef<SVGSVGElement, SearchProps>(
  ({ size = 24, className = '', strokeWidth = 1, ...props }, ref) => (
    <svg 
      ref={ref}
      width={size}
      height={size} 
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      className={className}
      xmlns="http://www.w3.org/2000/svg"
      {...(strokeWidth !== undefined ? { strokeWidth } : {})}
      {...props}
    >
      <path d="m21 21-4.34-4.34" />
  <circle cx="11" cy="11" r="8" />
    </svg>
  )
);
Search.displayName = "Search";
export const SearchMetadata = { 
  id: "search", 
  baseId: "search", 
  variant: "default", 
  name: "Search", 
  category: "others", 
  tags: [], 
  viewBox: "0 0 24 24" 
} as const;

export default Search;
```
---

### Header 06 <a name="header-06"></a>

A simple header section.

- **Registry Name**: `aliimam/header-06`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/header-06.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`
  - `@aliimam/icons`
  - `next-themes`
- **Registry Dependencies**:
  - `navigation-menu`
  - `avatar`
  - `dropdown-menu`
  - `button`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Props & API Reference

```typescript
export type ThemeSwitcherProps = {
  value?: "light" | "dark" | "system"
  onChange?: (theme: "light" | "dark" | "system") => void
  defaultValue?: "light" | "dark" | "system"
  className?: string
}
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
##### File: `header.tsx`

```tsx
import { Button } from "@/registry/aliimam/ui/button"

export function Header() {
  return (
    <div className="absolute top-0 left-0 z-20 flex h-12 w-full items-center justify-center px-6 sm:h-14 sm:px-8 md:h-16 md:px-12 lg:px-0">
      <div className="border-muted absolute top-6 left-0 h-0 w-full border-t sm:top-7 md:top-8"></div>

      <div className="bg-muted relative z-30 flex h-10 w-full max-w-[calc(100%-32px)] items-center justify-between overflow-hidden rounded-md border px-3 py-1.5 pr-2 backdrop-blur-sm sm:h-11 sm:max-w-[calc(100%-48px)] sm:px-4 sm:py-2 sm:pr-3 md:h-12 md:max-w-[calc(100%-64px)] md:px-2 lg:w-[700px] lg:max-w-[700px]">
        <div className="flex items-center justify-center">
          <div className="flex items-center justify-start">
            <div className="flex flex-col justify-center pl-2 text-sm leading-5 font-medium sm:text-base md:text-lg lg:text-xl">
              AI
            </div>
          </div>
          <div className="flex flex-row items-start justify-start gap-2 pl-3 sm:flex sm:gap-3 sm:pl-4 md:gap-4 md:pl-5 lg:gap-4 lg:pl-5">
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Designs
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Pricing
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Docs
              </div>
            </div>
          </div>
        </div>
        <Button size={"sm"}>Log in</Button>
      </div>
    </div>
  )
}
```
##### File: `theme.tsx`

```tsx
"use client"

import { useCallback, useEffect, useState } from "react"
import { Monitor, Moon, Sun } from "@aliimam/icons"
import { useTheme } from "next-themes"

import { cn } from "@/registry/aliimam/lib/utils"

const themes = [
  {
    key: "system",
    icon: Monitor,
    label: "System theme",
  },
  {
    key: "light",
    icon: Sun,
    label: "Light theme",
  },
  {
    key: "dark",
    icon: Moon,
    label: "Dark theme",
  },
]

export type ThemeSwitcherProps = {
  value?: "light" | "dark" | "system"
  onChange?: (theme: "light" | "dark" | "system") => void
  defaultValue?: "light" | "dark" | "system"
  className?: string
}

export const ThemeSwitcher = ({ className }: ThemeSwitcherProps) => {
  const { theme, setTheme } = useTheme()
  const [mounted, setMounted] = useState(false)

  const handleThemeClick = useCallback(
    (themeKey: "light" | "dark" | "system") => {
      setTheme(themeKey)
    },
    [setTheme]
  )

  // Prevent hydration mismatch
  useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) {
    return null
  }

  return (
    <div
      className={cn(
        "bg-background ring-border relative isolate flex h-7 rounded-full p-1 ring-1",
        className
      )}
    >
      {themes.map(({ key, icon: Icon, label }) => {
        const isActive = theme === key

        return (
          <button
            aria-label={label}
            className="relative h-5 w-6 rounded-full"
            key={key}
            onClick={() => handleThemeClick(key as "light" | "dark" | "system")}
            type="button"
          >
            {isActive && (
              <div className="bg-secondary absolute inset-0 rounded-full" />
            )}
            <Icon
              className={cn(
                "relative z-10 m-auto h-3.5 w-3.5",
                isActive ? "text-foreground" : "text-muted-foreground"
              )}
            />
          </button>
        )
      })}
    </div>
  )
}
```
---

### Hero 01 <a name="hero-01"></a>

A simple hero section.

- **Registry Name**: `aliimam/hero-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/hero-01.json
```

#### Dependencies

- **Registry Dependencies**:
  - `button`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `hero.tsx`

```tsx
"use client"

import Link from "next/link"
import {
  PageActions,
  PageHeader,
  PageHeaderDescription,
  PageHeaderHeading,
} from "@/src/components/layout/page-header"
import { Button } from "@/registry/aliimam/ui/button"
import { PixelGridShader } from "@/registry/aliimam/components/shaders/pixelgrid-shader"   
 
  
  
export function Hero() {
  return (
    <div className="relative flex flex-col h-[calc(100vh-var(--header-height)-var(--footer-height)+5rem)] items-center justify-center overflow-hidden">
   
      <PixelGridShader 
        amplitude={0.15}   
        shape="plasma" 
        pxSize={2}   
        colorFg="#ff0000" 
        className="hidden" />

      <PageHeader className="relative z-10">
        <PageHeaderHeading className="max-w-100 md:max-w-full">Design without limits</PageHeaderHeading>
        <PageHeaderDescription className="max-w-100 md:max-w-full">
          I create digital experiences that connect and inspire. I build apps,
          websites, brands, and products end-to-end.
        </PageHeaderDescription>
        <PageActions>
          <Button size={"xl"} variant={"outline"} asChild>
            <Link href="/docs">Get Started</Link>
          </Button>
          <Button asChild size={"xl"}>
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              Book a Call
            </Link>
          </Button>
        </PageActions>
      </PageHeader>
    </div>
  )
}
```
---

### Hero 02 <a name="hero-02"></a>

A simple hero section.

- **Registry Name**: `aliimam/hero-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/hero-02.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`
- **Registry Dependencies**:
  - `button`
  - `marquee`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `hero.tsx`

```tsx
"use client"

import Link from "next/link"
import {
  PageActions,
  PageHeader,
  PageHeaderDescription,
  PageHeaderHeading,
} from "@/src/components/layout/page-header"
import { Button } from "@/registry/aliimam/ui/button"
import { PixelGridShader } from "@/registry/aliimam/components/shaders/pixelgrid-shader"   
 
  
  
export function Hero() {
  return (
    <div className="relative flex flex-col h-[calc(100vh-var(--header-height)-var(--footer-height)+5rem)] items-center justify-center overflow-hidden">
   
      <PixelGridShader 
        amplitude={0.15}   
        shape="plasma" 
        pxSize={2}   
        colorFg="#ff0000" 
        className="hidden" />

      <PageHeader className="relative z-10">
        <PageHeaderHeading className="max-w-100 md:max-w-full">Design without limits</PageHeaderHeading>
        <PageHeaderDescription className="max-w-100 md:max-w-full">
          I create digital experiences that connect and inspire. I build apps,
          websites, brands, and products end-to-end.
        </PageHeaderDescription>
        <PageActions>
          <Button size={"xl"} variant={"outline"} asChild>
            <Link href="/docs">Get Started</Link>
          </Button>
          <Button asChild size={"xl"}>
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              Book a Call
            </Link>
          </Button>
        </PageActions>
      </PageHeader>
    </div>
  )
}
```
---

### Hero 03 <a name="hero-03"></a>

A simple hero section.

- **Registry Name**: `aliimam/hero-03`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/hero-03.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `separator`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `hero.tsx`

```tsx
"use client"

import Link from "next/link"
import {
  PageActions,
  PageHeader,
  PageHeaderDescription,
  PageHeaderHeading,
} from "@/src/components/layout/page-header"
import { Button } from "@/registry/aliimam/ui/button"
import { PixelGridShader } from "@/registry/aliimam/components/shaders/pixelgrid-shader"   
 
  
  
export function Hero() {
  return (
    <div className="relative flex flex-col h-[calc(100vh-var(--header-height)-var(--footer-height)+5rem)] items-center justify-center overflow-hidden">
   
      <PixelGridShader 
        amplitude={0.15}   
        shape="plasma" 
        pxSize={2}   
        colorFg="#ff0000" 
        className="hidden" />

      <PageHeader className="relative z-10">
        <PageHeaderHeading className="max-w-100 md:max-w-full">Design without limits</PageHeaderHeading>
        <PageHeaderDescription className="max-w-100 md:max-w-full">
          I create digital experiences that connect and inspire. I build apps,
          websites, brands, and products end-to-end.
        </PageHeaderDescription>
        <PageActions>
          <Button size={"xl"} variant={"outline"} asChild>
            <Link href="/docs">Get Started</Link>
          </Button>
          <Button asChild size={"xl"}>
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              Book a Call
            </Link>
          </Button>
        </PageActions>
      </PageHeader>
    </div>
  )
}
```
---

### Hero 04 <a name="hero-04"></a>

A simple hero section.

- **Registry Name**: `aliimam/hero-04`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/hero-04.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `hero.tsx`

```tsx
"use client"

import Link from "next/link"
import {
  PageActions,
  PageHeader,
  PageHeaderDescription,
  PageHeaderHeading,
} from "@/src/components/layout/page-header"
import { Button } from "@/registry/aliimam/ui/button"
import { PixelGridShader } from "@/registry/aliimam/components/shaders/pixelgrid-shader"   
 
  
  
export function Hero() {
  return (
    <div className="relative flex flex-col h-[calc(100vh-var(--header-height)-var(--footer-height)+5rem)] items-center justify-center overflow-hidden">
   
      <PixelGridShader 
        amplitude={0.15}   
        shape="plasma" 
        pxSize={2}   
        colorFg="#ff0000" 
        className="hidden" />

      <PageHeader className="relative z-10">
        <PageHeaderHeading className="max-w-100 md:max-w-full">Design without limits</PageHeaderHeading>
        <PageHeaderDescription className="max-w-100 md:max-w-full">
          I create digital experiences that connect and inspire. I build apps,
          websites, brands, and products end-to-end.
        </PageHeaderDescription>
        <PageActions>
          <Button size={"xl"} variant={"outline"} asChild>
            <Link href="/docs">Get Started</Link>
          </Button>
          <Button asChild size={"xl"}>
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              Book a Call
            </Link>
          </Button>
        </PageActions>
      </PageHeader>
    </div>
  )
}
```
---

### Hero 05 <a name="hero-05"></a>

A simple hero section.

- **Registry Name**: `aliimam/hero-05`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/hero-05.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `marquee`
  - `button`
  - `badge`
  - `card`
  - `marquee`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `hero.tsx`

```tsx
"use client"

import Link from "next/link"
import {
  PageActions,
  PageHeader,
  PageHeaderDescription,
  PageHeaderHeading,
} from "@/src/components/layout/page-header"
import { Button } from "@/registry/aliimam/ui/button"
import { PixelGridShader } from "@/registry/aliimam/components/shaders/pixelgrid-shader"   
 
  
  
export function Hero() {
  return (
    <div className="relative flex flex-col h-[calc(100vh-var(--header-height)-var(--footer-height)+5rem)] items-center justify-center overflow-hidden">
   
      <PixelGridShader 
        amplitude={0.15}   
        shape="plasma" 
        pxSize={2}   
        colorFg="#ff0000" 
        className="hidden" />

      <PageHeader className="relative z-10">
        <PageHeaderHeading className="max-w-100 md:max-w-full">Design without limits</PageHeaderHeading>
        <PageHeaderDescription className="max-w-100 md:max-w-full">
          I create digital experiences that connect and inspire. I build apps,
          websites, brands, and products end-to-end.
        </PageHeaderDescription>
        <PageActions>
          <Button size={"xl"} variant={"outline"} asChild>
            <Link href="/docs">Get Started</Link>
          </Button>
          <Button asChild size={"xl"}>
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              Book a Call
            </Link>
          </Button>
        </PageActions>
      </PageHeader>
    </div>
  )
}
```
---

### Hero 06 <a name="hero-06"></a>

A simple hero section.

- **Registry Name**: `aliimam/hero-06`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/hero-06.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `dot-pattern`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `hero.tsx`

```tsx
"use client"

import Link from "next/link"
import {
  PageActions,
  PageHeader,
  PageHeaderDescription,
  PageHeaderHeading,
} from "@/src/components/layout/page-header"
import { Button } from "@/registry/aliimam/ui/button"
import { PixelGridShader } from "@/registry/aliimam/components/shaders/pixelgrid-shader"   
 
  
  
export function Hero() {
  return (
    <div className="relative flex flex-col h-[calc(100vh-var(--header-height)-var(--footer-height)+5rem)] items-center justify-center overflow-hidden">
   
      <PixelGridShader 
        amplitude={0.15}   
        shape="plasma" 
        pxSize={2}   
        colorFg="#ff0000" 
        className="hidden" />

      <PageHeader className="relative z-10">
        <PageHeaderHeading className="max-w-100 md:max-w-full">Design without limits</PageHeaderHeading>
        <PageHeaderDescription className="max-w-100 md:max-w-full">
          I create digital experiences that connect and inspire. I build apps,
          websites, brands, and products end-to-end.
        </PageHeaderDescription>
        <PageActions>
          <Button size={"xl"} variant={"outline"} asChild>
            <Link href="/docs">Get Started</Link>
          </Button>
          <Button asChild size={"xl"}>
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              Book a Call
            </Link>
          </Button>
        </PageActions>
      </PageHeader>
    </div>
  )
}
```
---

### Hero 07 <a name="hero-07"></a>

A simple hero section.

- **Registry Name**: `aliimam/hero-07`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/hero-07.json
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
- **Registry Dependencies**:
  - `button`
  - `grid-pattern`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `hero.tsx`

```tsx
"use client"

import Link from "next/link"
import {
  PageActions,
  PageHeader,
  PageHeaderDescription,
  PageHeaderHeading,
} from "@/src/components/layout/page-header"
import { Button } from "@/registry/aliimam/ui/button"
import { PixelGridShader } from "@/registry/aliimam/components/shaders/pixelgrid-shader"   
 
  
  
export function Hero() {
  return (
    <div className="relative flex flex-col h-[calc(100vh-var(--header-height)-var(--footer-height)+5rem)] items-center justify-center overflow-hidden">
   
      <PixelGridShader 
        amplitude={0.15}   
        shape="plasma" 
        pxSize={2}   
        colorFg="#ff0000" 
        className="hidden" />

      <PageHeader className="relative z-10">
        <PageHeaderHeading className="max-w-100 md:max-w-full">Design without limits</PageHeaderHeading>
        <PageHeaderDescription className="max-w-100 md:max-w-full">
          I create digital experiences that connect and inspire. I build apps,
          websites, brands, and products end-to-end.
        </PageHeaderDescription>
        <PageActions>
          <Button size={"xl"} variant={"outline"} asChild>
            <Link href="/docs">Get Started</Link>
          </Button>
          <Button asChild size={"xl"}>
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              Book a Call
            </Link>
          </Button>
        </PageActions>
      </PageHeader>
    </div>
  )
}
```
##### File: `gallery.tsx`

```tsx
"use client";

import { PhotoProvider, PhotoView } from "react-photo-view";

import "react-photo-view/dist/react-photo-view.css";

import { CldImage } from "next-cloudinary";
 

type ImageType = {
  secure_url: string;
  id?: string;
};

export { CldImage }


export function Grid2({ images }: { images: ImageType[] }) {
  return (
    <>
      <PhotoProvider>
        <div className="grid items-stretch gap-1 md:gap-2 md:grid-cols-2">
          {images.map((image: ImageType) => (
            <PhotoView key={image.secure_url}  src={image.secure_url}>
              <CldImage
                src={image.secure_url}
                alt={image.secure_url}
                loading="lazy"
                width={700}
                height={600}
                className="rounded-xl object-cover hover:cursor-zoom-in hover:saturate-0"
              />
            </PhotoView>
          ))}
        </div>
      </PhotoProvider>
    </>
  );
}

export function Grid4({ images }: { images: ImageType[] }) {
  return (
    <>
      <PhotoProvider>
        <div className="grid grid-cols-2 items-stretch gap-1 md:gap-2 md:grid-cols-4">
          {images.map((image: ImageType) => (
            <PhotoView key={image.secure_url} src={image.secure_url}>
              <CldImage
                src={image.secure_url}
                alt={image.secure_url}
                loading="lazy"
                width={500}
                height={500}
                className="rounded object-cover hover:cursor-zoom-in hover:saturate-0"
              />
            </PhotoView>
          ))}
        </div>
      </PhotoProvider>
    </>
  );
} 

export function Grid3({ images }: { images: ImageType[] }) {
  return (
    <>
      <PhotoProvider>
        <div className="grid grid-cols-2 items-stretch gap-1 md:gap-2 md:grid-cols-3">
          {images.map((image: ImageType) => (
            <PhotoView key={image.secure_url} src={image.secure_url}>
              <CldImage
                src={image.secure_url}
                alt={image.secure_url}
                loading="lazy"
                width={500}
                height={500}
                className="rounded object-cover hover:cursor-zoom-in hover:saturate-0"
              />
            </PhotoView>
          ))}
        </div>
      </PhotoProvider>
    </>
  );
} 

export function Grid5({ images }: { images: ImageType[] }) {
  return (
    <>
      <PhotoProvider>
        <div className="grid grid-cols-3 items-stretch gap-1 md:gap-2 md:grid-cols-4 lg:grid-cols-5">
          {images.map((image: ImageType) => (
            <PhotoView key={image.secure_url} src={image.secure_url}>
              <CldImage
                src={image.secure_url}
                alt={image.secure_url}
                loading="lazy"
                width={500}
                height={500}
                className="rounded object-cover hover:cursor-zoom-in hover:saturate-0"
              />
            </PhotoView>
          ))}
        </div>
      </PhotoProvider>
    </>
  );
}
```
---

### Hero 08 <a name="hero-08"></a>

A simple hero section.

- **Registry Name**: `aliimam/hero-08`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/hero-08.json
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
- **Registry Dependencies**:
  - `button`
  - `gradient-bars`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `hero.tsx`

```tsx
"use client"

import Link from "next/link"
import {
  PageActions,
  PageHeader,
  PageHeaderDescription,
  PageHeaderHeading,
} from "@/src/components/layout/page-header"
import { Button } from "@/registry/aliimam/ui/button"
import { PixelGridShader } from "@/registry/aliimam/components/shaders/pixelgrid-shader"   
 
  
  
export function Hero() {
  return (
    <div className="relative flex flex-col h-[calc(100vh-var(--header-height)-var(--footer-height)+5rem)] items-center justify-center overflow-hidden">
   
      <PixelGridShader 
        amplitude={0.15}   
        shape="plasma" 
        pxSize={2}   
        colorFg="#ff0000" 
        className="hidden" />

      <PageHeader className="relative z-10">
        <PageHeaderHeading className="max-w-100 md:max-w-full">Design without limits</PageHeaderHeading>
        <PageHeaderDescription className="max-w-100 md:max-w-full">
          I create digital experiences that connect and inspire. I build apps,
          websites, brands, and products end-to-end.
        </PageHeaderDescription>
        <PageActions>
          <Button size={"xl"} variant={"outline"} asChild>
            <Link href="/docs">Get Started</Link>
          </Button>
          <Button asChild size={"xl"}>
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              Book a Call
            </Link>
          </Button>
        </PageActions>
      </PageHeader>
    </div>
  )
}
```
##### File: `button-rotate.tsx`

```tsx
import { Button } from "@/registry/aliimam/ui/button"

export const ButtonRotate = () => {
  const text = "I LOVE YOUR DESIGN"

  return (
    <div className="border-primary rounded-full border border-dotted p-1">
      <Button className="bg-primary relative grid h-[100px] w-[100px] place-content-center overflow-hidden rounded-full p-0">
        <p
          className="absolute inset-0"
          style={{
            animation: "text-rotation 8s linear infinite",
            position: "absolute",
            inset: 0,
          }}
        >
          {Array.from(text).map((char, i) => (
            <span
              key={i}
              style={{
                position: "absolute",
                inset: "6px",
                transform: `rotate(${19 * i}deg)`,
                transformOrigin: "50% 50%",
                userSelect: "none",
                display: "inline-block",
              }}
            >
              {char === " " ? "\u00A0" : char}
            </span>
          ))}
        </p>

        <div className="text-primary bg-background relative flex h-[40px] w-[40px] items-center justify-center overflow-hidden rounded-full">
          <svg
            viewBox="0 0 14 15"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
            className="absolute h-4 w-4 transition-transform duration-300 ease-in-out"
            style={{ transform: "translate(0, 0)" }}
          >
            <path
              d="M13.376 11.552l-.264-10.44-10.44-.24.024 2.28 6.96-.048L.2 12.56l1.488 1.488 9.432-9.432-.048 6.912 2.304.024z"
              fill="currentColor"
            />
          </svg>
          <svg
            viewBox="0 0 14 15"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
            className="absolute h-4 w-4 transition-transform duration-300 ease-in-out"
            style={{ transform: "translate(-150%, 150%)" }}
          >
            <path
              d="M13.376 11.552l-.264-10.44-10.44-.24.024 2.28 6.96-.048L.2 12.56l1.488 1.488 9.432-9.432-.048 6.912 2.304.024z"
              fill="currentColor"
            />
          </svg>
        </div>

        <style jsx>{`
          @keyframes text-rotation {
            to {
              rotate: 360deg;
            }
          }
          p {
            animation: text-rotation 8s linear infinite;
          }
          span {
            user-select: none;
          }
          button:hover svg:first-child {
            transform: translate(150%, -150%);
            color: black;
          }
          button:hover svg:last-child {
            transform: translate(0);
            color: black;
            transition-delay: 0.1s;
          }
        `}</style>
      </Button>
    </div>
  )
}
```
---

### Hero 09 <a name="hero-09"></a>

A simple hero section.

- **Registry Name**: `aliimam/hero-09`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/hero-09.json
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
- **Registry Dependencies**:
  - `button`
  - `attraction`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `hero.tsx`

```tsx
"use client"

import Link from "next/link"
import {
  PageActions,
  PageHeader,
  PageHeaderDescription,
  PageHeaderHeading,
} from "@/src/components/layout/page-header"
import { Button } from "@/registry/aliimam/ui/button"
import { PixelGridShader } from "@/registry/aliimam/components/shaders/pixelgrid-shader"   
 
  
  
export function Hero() {
  return (
    <div className="relative flex flex-col h-[calc(100vh-var(--header-height)-var(--footer-height)+5rem)] items-center justify-center overflow-hidden">
   
      <PixelGridShader 
        amplitude={0.15}   
        shape="plasma" 
        pxSize={2}   
        colorFg="#ff0000" 
        className="hidden" />

      <PageHeader className="relative z-10">
        <PageHeaderHeading className="max-w-100 md:max-w-full">Design without limits</PageHeaderHeading>
        <PageHeaderDescription className="max-w-100 md:max-w-full">
          I create digital experiences that connect and inspire. I build apps,
          websites, brands, and products end-to-end.
        </PageHeaderDescription>
        <PageActions>
          <Button size={"xl"} variant={"outline"} asChild>
            <Link href="/docs">Get Started</Link>
          </Button>
          <Button asChild size={"xl"}>
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              Book a Call
            </Link>
          </Button>
        </PageActions>
      </PageHeader>
    </div>
  )
}
```
##### File: `skills.tsx`

```tsx
"use client"

import * as React from "react"

import { Attraction } from "@/registry/aliimam/components/attraction"

const designServices = {
  graphicDesign: [
    "Logos",
    "Branding",
    "Social Media",
    "Print",
    "Packaging",
    "Marketing",
    "Infographics",
    "Motion",
    "Illustration",
    "Presentations",
    "Newsletters",
    "Thumbnails",
    "Covers",
    "Signage",
    "Mascots",
  ],
  webDigitalDesign: [
    "Websites",
    "UI/UX",
    "Landing Pages",
    "Ads",
    "Emails",
    "Apps",
    "No-Code",
  ],
  productPackagingDesign: ["Mockups", "3D", "Prototypes", "Products"],
  otherTools: [
    "Cursor",
    "Photoshop",
    "Illustrator",
    "After Effects",
    "Premiere Pro",
    "Figma",
    "Framer",
    "VS Code",
    "React",
    "Typescript",
    "Vercel",
    "Github",
    "Framer",
    "ChatGPT",
    "V0",
    "Grok",
    "Shadcn/UI",
  ],
}

const colors = [
  "bg-red-500",
  "bg-purple-500",
  "bg-blue-500",
  "bg-green-500",
  "bg-orange-500",
  "bg-pink-500",
  "bg-teal-500",
  "bg-indigo-500",
  "bg-yellow-500",
  "bg-cyan-500",
  "bg-rose-500",
  "bg-emerald-500",
]

const getRandom = (min: number, max: number) =>
  Math.random() * (max - min) + min

const Skills = () => {
  const servicesWithProps = React.useMemo(() => {
    const allServices = Object.values(designServices).flat()

    return allServices.map((service, index) => ({
      service,
      x: getRandom(0, 600),
      y: getRandom(0, 400),
      angle: getRandom(0, 360),
      color: colors[index % colors.length],
    }))
  }, [])

  return (
    <div className="flex h-[600px] w-full items-center justify-center overflow-hidden">
      <Attraction gravity={{ x: 0, y: 1 }}>
        {servicesWithProps.map(({ service, x, y, color, angle }, index) => (
          <div
            key={index}
            data-x={x}
            data-y={y}
            data-angle={angle}
            className={`${color} cursor-grab rounded-md px-4 py-2 text-sm text-white uppercase select-none active:cursor-grabbing`}
          >
            {service}
          </div>
        ))}
      </Attraction>
    </div>
  )
}

export { Skills }
```
---

### Hero 10 <a name="hero-10"></a>

A simple hero section.

- **Registry Name**: `aliimam/hero-10`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/hero-10.json
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
- **Registry Dependencies**:
  - `button`
  - `border-glow`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `hero.tsx`

```tsx
"use client"

import Link from "next/link"
import {
  PageActions,
  PageHeader,
  PageHeaderDescription,
  PageHeaderHeading,
} from "@/src/components/layout/page-header"
import { Button } from "@/registry/aliimam/ui/button"
import { PixelGridShader } from "@/registry/aliimam/components/shaders/pixelgrid-shader"   
 
  
  
export function Hero() {
  return (
    <div className="relative flex flex-col h-[calc(100vh-var(--header-height)-var(--footer-height)+5rem)] items-center justify-center overflow-hidden">
   
      <PixelGridShader 
        amplitude={0.15}   
        shape="plasma" 
        pxSize={2}   
        colorFg="#ff0000" 
        className="hidden" />

      <PageHeader className="relative z-10">
        <PageHeaderHeading className="max-w-100 md:max-w-full">Design without limits</PageHeaderHeading>
        <PageHeaderDescription className="max-w-100 md:max-w-full">
          I create digital experiences that connect and inspire. I build apps,
          websites, brands, and products end-to-end.
        </PageHeaderDescription>
        <PageActions>
          <Button size={"xl"} variant={"outline"} asChild>
            <Link href="/docs">Get Started</Link>
          </Button>
          <Button asChild size={"xl"}>
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              Book a Call
            </Link>
          </Button>
        </PageActions>
      </PageHeader>
    </div>
  )
}
```
---

### Hero 11 <a name="hero-11"></a>

A simple hero section.

- **Registry Name**: `aliimam/hero-11`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/hero-11.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `hero.tsx`

```tsx
"use client"

import Link from "next/link"
import {
  PageActions,
  PageHeader,
  PageHeaderDescription,
  PageHeaderHeading,
} from "@/src/components/layout/page-header"
import { Button } from "@/registry/aliimam/ui/button"
import { PixelGridShader } from "@/registry/aliimam/components/shaders/pixelgrid-shader"   
 
  
  
export function Hero() {
  return (
    <div className="relative flex flex-col h-[calc(100vh-var(--header-height)-var(--footer-height)+5rem)] items-center justify-center overflow-hidden">
   
      <PixelGridShader 
        amplitude={0.15}   
        shape="plasma" 
        pxSize={2}   
        colorFg="#ff0000" 
        className="hidden" />

      <PageHeader className="relative z-10">
        <PageHeaderHeading className="max-w-100 md:max-w-full">Design without limits</PageHeaderHeading>
        <PageHeaderDescription className="max-w-100 md:max-w-full">
          I create digital experiences that connect and inspire. I build apps,
          websites, brands, and products end-to-end.
        </PageHeaderDescription>
        <PageActions>
          <Button size={"xl"} variant={"outline"} asChild>
            <Link href="/docs">Get Started</Link>
          </Button>
          <Button asChild size={"xl"}>
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              Book a Call
            </Link>
          </Button>
        </PageActions>
      </PageHeader>
    </div>
  )
}
```
---

### Hero 12 <a name="hero-12"></a>

A simple hero section.

- **Registry Name**: `aliimam/hero-12`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/hero-12.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `hero.tsx`

```tsx
"use client"

import Link from "next/link"
import {
  PageActions,
  PageHeader,
  PageHeaderDescription,
  PageHeaderHeading,
} from "@/src/components/layout/page-header"
import { Button } from "@/registry/aliimam/ui/button"
import { PixelGridShader } from "@/registry/aliimam/components/shaders/pixelgrid-shader"   
 
  
  
export function Hero() {
  return (
    <div className="relative flex flex-col h-[calc(100vh-var(--header-height)-var(--footer-height)+5rem)] items-center justify-center overflow-hidden">
   
      <PixelGridShader 
        amplitude={0.15}   
        shape="plasma" 
        pxSize={2}   
        colorFg="#ff0000" 
        className="hidden" />

      <PageHeader className="relative z-10">
        <PageHeaderHeading className="max-w-100 md:max-w-full">Design without limits</PageHeaderHeading>
        <PageHeaderDescription className="max-w-100 md:max-w-full">
          I create digital experiences that connect and inspire. I build apps,
          websites, brands, and products end-to-end.
        </PageHeaderDescription>
        <PageActions>
          <Button size={"xl"} variant={"outline"} asChild>
            <Link href="/docs">Get Started</Link>
          </Button>
          <Button asChild size={"xl"}>
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              Book a Call
            </Link>
          </Button>
        </PageActions>
      </PageHeader>
    </div>
  )
}
```
---

### Home 01 <a name="home-01"></a>

A simple home page.

- **Registry Name**: `aliimam/home-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/home-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Props & API Reference

```typescript
export type ThemeSwitcherProps = {
  value?: "light" | "dark" | "system"
  onChange?: (theme: "light" | "dark" | "system") => void
  defaultValue?: "light" | "dark" | "system"
  className?: string
}
```

```typescript
export interface ScaleProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
##### File: `header.tsx`

```tsx
import { Button } from "@/registry/aliimam/ui/button"

export function Header() {
  return (
    <div className="absolute top-0 left-0 z-20 flex h-12 w-full items-center justify-center px-6 sm:h-14 sm:px-8 md:h-16 md:px-12 lg:px-0">
      <div className="border-muted absolute top-6 left-0 h-0 w-full border-t sm:top-7 md:top-8"></div>

      <div className="bg-muted relative z-30 flex h-10 w-full max-w-[calc(100%-32px)] items-center justify-between overflow-hidden rounded-md border px-3 py-1.5 pr-2 backdrop-blur-sm sm:h-11 sm:max-w-[calc(100%-48px)] sm:px-4 sm:py-2 sm:pr-3 md:h-12 md:max-w-[calc(100%-64px)] md:px-2 lg:w-[700px] lg:max-w-[700px]">
        <div className="flex items-center justify-center">
          <div className="flex items-center justify-start">
            <div className="flex flex-col justify-center pl-2 text-sm leading-5 font-medium sm:text-base md:text-lg lg:text-xl">
              AI
            </div>
          </div>
          <div className="flex flex-row items-start justify-start gap-2 pl-3 sm:flex sm:gap-3 sm:pl-4 md:gap-4 md:pl-5 lg:gap-4 lg:pl-5">
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Designs
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Pricing
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Docs
              </div>
            </div>
          </div>
        </div>
        <Button size={"sm"}>Log in</Button>
      </div>
    </div>
  )
}
```
##### File: `footer.tsx`

```tsx
import Link from "next/link"
import { ChevronDown, SquareArrowOutUpRight } from "@aliimam/icons"
import { Github, LinkedIn, Vercel, X, YouTube } from "@aliimam/logos"

import { ThemeSwitcher } from "@/registry/aliimam/pages/home/home-01/layout/theme"
import { Button } from "@/registry/aliimam/ui/button"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/registry/aliimam/ui/dropdown-menu"

const linksPro = [
  {
    group: "Product",
    items: [
      {
        title: "AI",
        href: "#",
      },
      {
        title: "Enterprise",
        href: "#",
      },
      {
        title: "Fluid Compute",
        href: "#",
      },
      {
        title: "Next.js",
        href: "#",
      },
      {
        title: "Observability",
        href: "#",
      },
      {
        title: "Previews",
        href: "#",
      },
      {
        title: "Rendering",
        href: "#",
      },
      {
        title: "Security",
        href: "#",
      },
      {
        title: "Turbo",
        href: "#",
      },
      {
        title: "Domains",
        href: "#",
      },
    ],
  },
]

const linksRes = [
  {
    group: "Resources",
    items: [
      {
        title: "Docs",
        href: "#",
      },
      {
        title: "Guides",
        href: "#",
      },
      {
        title: "Academy",
        href: "#",
      },
      {
        title: "Help",
        href: "#",
      },
      {
        title: "Integrations",
        href: "#",
      },
      {
        title: "Pricing",
        href: "#",
      },
      {
        title: "Resources",
        href: "#",
      },
      {
        title: "Solution Partners",
        href: "#",
      },
      {
        title: "Startups",
        href: "#",
      },
      {
        title: "Templates",
        href: "#",
      },
    ],
  },
]

const linksCom = [
  {
    group: "Company",
    items: [
      {
        title: "About",
        href: "#",
      },
      {
        title: "Blog",
        href: "#",
      },
      {
        title: "Careers",
        href: "#",
      },
      {
        title: "Changelog",
        href: "#",
      },
      {
        title: "Contact Us",
        href: "#",
      },
      {
        title: "Customers",
        href: "#",
      },
      {
        title: "Events",
        href: "#",
      },
      {
        title: "Partners",
        href: "#",
      },
      {
        title: "Shipped",
        href: "#",
      },
      {
        title: "Privacy Policy",
        href: "#",
      },
    ],
  },
]

export function Footer() {
  return (
    <footer className="py-16">
      <div className="mx-auto max-w-6xl px-6">
        <div className="">
          <div className="grid grid-cols-2 gap-14 md:grid-cols-3 lg:grid-cols-5">
            <div className="grid gap-3">
              {linksPro.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>
                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <Link
                    href={"#"}
                    className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm duration-150"
                  >
                    v0
                    <SquareArrowOutUpRight strokeWidth={2} size={16} />
                  </Link>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              {linksRes.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>
                  <Link
                    href={"#"}
                    className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm duration-150"
                  >
                    Community
                    <SquareArrowOutUpRight strokeWidth={2} size={16} />
                  </Link>
                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <DropdownMenu>
                    <DropdownMenuTrigger className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm">
                      SDKs by Vercel <ChevronDown strokeWidth={2} size={16} />
                    </DropdownMenuTrigger>
                    <DropdownMenuContent
                      side="top"
                      align="end"
                      className="w-60 p-1"
                    >
                      <DropdownMenuItem className="h-10 px-4">
                        AI SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Flags SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Chat SDK{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Streamdown AI{" "}
                        <SquareArrowOutUpRight strokeWidth={2} size={16} />
                      </DropdownMenuItem>
                    </DropdownMenuContent>
                  </DropdownMenu>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              {linksCom.map((link, index) => (
                <div key={index} className="space-y-3 text-sm">
                  <span className="block font-medium">{link.group}</span>

                  {link.items.map((item, index) => (
                    <Link
                      key={index}
                      href={item.href}
                      className="text-muted-foreground hover:text-primary block duration-150"
                    >
                      <span>{item.title}</span>
                    </Link>
                  ))}
                  <DropdownMenu>
                    <DropdownMenuTrigger className="text-muted-foreground hover:text-primary flex items-center gap-1 text-sm">
                      Legal <ChevronDown strokeWidth={2} size={16} />
                    </DropdownMenuTrigger>
                    <DropdownMenuContent
                      side="top"
                      align="end"
                      className="w-60 p-1"
                    >
                      <DropdownMenuItem className="h-10 px-4">
                        Cookie Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Cookie Preferences
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DMCA Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DORA Addendum
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        DPA
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-16 px-4">
                        Domain Name Registration and Services Terms
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Event Code of Conduct
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Event Terms and Conditions
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Inactivity Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Job Applicant Privacy Notice
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Privacy Policy
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        SLA
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Sub-processors
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Support Terms
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Terms of Service
                      </DropdownMenuItem>
                      <DropdownMenuItem className="h-10 px-4">
                        Trademark Policy
                      </DropdownMenuItem>
                    </DropdownMenuContent>
                  </DropdownMenu>
                </div>
              ))}
            </div>
            <div className="grid gap-3">
              <div className="space-y-3 text-sm">
                <span className="block font-medium">Social</span>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <Github size={14} />
                    Github
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <LinkedIn className="grayscale" size={14} />
                    LinkedIn
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <X size={14} />
                    Twitter
                  </span>
                </Link>
                <Link
                  href="#"
                  className="text-muted-foreground hover:text-primary block duration-150"
                >
                  <span className="flex items-center gap-2">
                    <YouTube className="grayscale" size={14} />
                    YouTube
                  </span>
                </Link>
              </div>
            </div>
            <div className="flex justify-end">
              <Vercel size={18} />
            </div>
          </div>
        </div>
        <div className="mt-10 flex flex-wrap items-end justify-between gap-6 py-6">
          <Button
            className="cursor-pointer text-blue-500 hover:text-blue-500"
            variant={"ghost"}
          >
            <span className="border-background block size-3 rounded-full border bg-blue-500" />
            All systems normal.
          </Button>
          <ThemeSwitcher />
        </div>
      </div>
    </footer>
  )
}
```
##### File: `theme.tsx`

```tsx
"use client"

import { useCallback, useEffect, useState } from "react"
import { Monitor, Moon, Sun } from "@aliimam/icons"
import { useTheme } from "next-themes"

import { cn } from "@/registry/aliimam/lib/utils"

const themes = [
  {
    key: "system",
    icon: Monitor,
    label: "System theme",
  },
  {
    key: "light",
    icon: Sun,
    label: "Light theme",
  },
  {
    key: "dark",
    icon: Moon,
    label: "Dark theme",
  },
]

export type ThemeSwitcherProps = {
  value?: "light" | "dark" | "system"
  onChange?: (theme: "light" | "dark" | "system") => void
  defaultValue?: "light" | "dark" | "system"
  className?: string
}

export const ThemeSwitcher = ({ className }: ThemeSwitcherProps) => {
  const { theme, setTheme } = useTheme()
  const [mounted, setMounted] = useState(false)

  const handleThemeClick = useCallback(
    (themeKey: "light" | "dark" | "system") => {
      setTheme(themeKey)
    },
    [setTheme]
  )

  // Prevent hydration mismatch
  useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) {
    return null
  }

  return (
    <div
      className={cn(
        "bg-background ring-border relative isolate flex h-7 rounded-full p-1 ring-1",
        className
      )}
    >
      {themes.map(({ key, icon: Icon, label }) => {
        const isActive = theme === key

        return (
          <button
            aria-label={label}
            className="relative h-5 w-6 rounded-full"
            key={key}
            onClick={() => handleThemeClick(key as "light" | "dark" | "system")}
            type="button"
          >
            {isActive && (
              <div className="bg-secondary absolute inset-0 rounded-full" />
            )}
            <Icon
              className={cn(
                "relative z-10 m-auto h-3.5 w-3.5",
                isActive ? "text-foreground" : "text-muted-foreground"
              )}
            />
          </button>
        )
      })}
    </div>
  )
}
```
##### File: `hero.tsx`

```tsx
"use client"

import Link from "next/link"
import {
  PageActions,
  PageHeader,
  PageHeaderDescription,
  PageHeaderHeading,
} from "@/src/components/layout/page-header"
import { Button } from "@/registry/aliimam/ui/button"
import { PixelGridShader } from "@/registry/aliimam/components/shaders/pixelgrid-shader"   
 
  
  
export function Hero() {
  return (
    <div className="relative flex flex-col h-[calc(100vh-var(--header-height)-var(--footer-height)+5rem)] items-center justify-center overflow-hidden">
   
      <PixelGridShader 
        amplitude={0.15}   
        shape="plasma" 
        pxSize={2}   
        colorFg="#ff0000" 
        className="hidden" />

      <PageHeader className="relative z-10">
        <PageHeaderHeading className="max-w-100 md:max-w-full">Design without limits</PageHeaderHeading>
        <PageHeaderDescription className="max-w-100 md:max-w-full">
          I create digital experiences that connect and inspire. I build apps,
          websites, brands, and products end-to-end.
        </PageHeaderDescription>
        <PageActions>
          <Button size={"xl"} variant={"outline"} asChild>
            <Link href="/docs">Get Started</Link>
          </Button>
          <Button asChild size={"xl"}>
            <Link target="_blank" href="https://cal.com/aliimam/30min">
              Book a Call
            </Link>
          </Button>
        </PageActions>
      </PageHeader>
    </div>
  )
}
```
##### File: `deploy.tsx`

```tsx
"use client"

import { GitMerge, Globe, Terminal } from "@aliimam/icons"

export function Deploy() {
  return (
    <div className="mx-auto flex max-w-6xl flex-col items-center justify-center px-4 lg:px-0">
      <div className="w-full space-y-3 border-x border-b py-20">
        <h1 className="flex w-full items-center justify-center gap-2 text-center text-3xl leading-none font-semibold tracking-tight lg:text-4xl">
          Develop with your favorite tools
          <Terminal className="mt-2" strokeWidth={2.5} size={34} />
        </h1>
        <h1 className="flex w-full items-center justify-center gap-2 text-center text-3xl leading-none font-semibold tracking-tight lg:text-4xl">
          Launch globally, instantly
          <Globe strokeWidth={2.5} size={30} />
          Keep pushing
          <GitMerge strokeWidth={2.5} size={30} />
        </h1>
      </div>
      <div className="w-full space-y-3 border-x border-b py-2" />
    </div>
  )
}
```
##### File: `gateway.tsx`

```tsx
"use client"

import { GitBranch, Sparkles } from "@aliimam/icons"
import { RotateCcw } from "lucide-react"

export function Gateway() {
  return (
    <div className="mx-auto flex max-w-6xl flex-col items-center justify-center px-4 lg:px-0">
      <div className="w-full border-x border-b">
        <div className="grid md:grid-cols-2">
          <div className="p-10">
            <h1 className="text-muted-foreground lg:text-md flex w-full gap-1 text-sm">
              <Sparkles size={18} />
              Vercel AI Gateway
            </h1>
            <h1 className="w-full pt-4 text-sm font-semibold tracking-tight lg:text-2xl">
              Deploy AI in seconds.
            </h1>
            <h1 className="text-muted-foreground w-full text-sm font-medium tracking-tight lg:text-2xl">
              Access all major models through a single, unified interface and
              shared AI credit wallet.
            </h1>
            <div className="pt-20">
              <img
                className="size-full dark:hidden"
                alt={`Ali's avatar`}
                src={"/pages/ai-gateway-full-light.svg"}
                fetchPriority="high"
              />
              <img
                className="hidden size-full dark:block"
                alt={`Ali's avatar`}
                src={"/pages/ai-gateway-full-dark.svg"}
                fetchPriority="high"
              />
            </div>
          </div>
          <div className="border-t p-10 md:border-t-0 md:border-l">
            <h1 className="text-muted-foreground lg:text-md flex w-full gap-1 text-sm">
              <RotateCcw size={18} />
              Instant Rollbacks
            </h1>
            <h1 className="w-full pt-4 text-sm font-semibold tracking-tight lg:text-2xl">
              Go ahead, deploy on Friday.
            </h1>
            <h1 className="text-muted-foreground w-full text-sm font-medium tracking-tight lg:text-2xl">
              Safely manage releases with automated deployments and instant
              rollbacks.
            </h1>
            <div className="mt-10 flex flex-col space-y-4">
              <div className="flex w-fit gap-6 self-start rounded-xl border p-4 shadow-sm">
                <div className="space-y-3">
                  <div className="flex items-center justify-between text-sm">
                    <div>
                      <span className="font-bold"> vercel-site/</span>
                      <span className="self-end font-bold">aliimam</span>
                    </div>
                    <span className="text-muted-foreground text-xs">
                      1d ago
                    </span>
                  </div>
                  <p className="flex items-center gap-2 text-sm">
                    <GitBranch strokeWidth={2} size={14} />
                    ba5f55f <span className="">Update bento box design</span>
                  </p>
                </div>
                <div className="flex items-center justify-center">
                  <span className="absolute text-xs">90</span>
                  <svg
                    aria-hidden="true"
                    fill="none"
                    height="32"
                    strokeWidth="2"
                    viewBox="0 0 100 100"
                    width="32"
                  >
                    <circle
                      cx="50"
                      cy="50"
                      r="45"
                      strokeWidth="10"
                      strokeDashoffset="0"
                      strokeLinecap="round"
                      strokeLinejoin="round"
                      stroke="green"
                    ></circle>
                  </svg>
                </div>
              </div>

              <div className="relative flex justify-center">
                <div className="flex items-center justify-center">
                  <svg
                    data-size="large"
                    fill="none"
                    height="152"
                    viewBox="0 0 117 152"
                    width="117"
                  >
                    <path
                      d="M3.99999 4L3.99999 60C3.99999 66.6274 9.37258 72 16 72L104 72C110.627 72 116 77.3726 116 84L116 152"
                      stroke="url(#paint0_linear_1364_100888)"
                      strokeWidth="2"
                    />
                    <path
                      d="M3.99999 4L3.99999 60C3.99999 66.6274 9.37258 72 16 72L104 72C110.627 72 116 77.3726 116 84L116 152"
                      stroke="url(#paint1_linear_1364_100888)"
                      strokeWidth="2"
                    />
                    <g clipPath="url(#clip0_1364_100888)">
                      <path
                        clipRule="evenodd"
                        d="M4 0.5L8 7.5H0L4 0.5Z"
                        fill="#45DEC4"
                        fillRule="evenodd"
                      />
                    </g>
                    <defs>
                      <linearGradient
                        gradientUnits="userSpaceOnUse"
                        id="paint0_linear_1364_100888"
                        x1="116"
                        x2="4"
                        y1="72"
                        y2="72"
                      >
                        <stop stopColor="#E5484D" />
                        <stop offset="0.5" stopColor="#FFC634" />
                        <stop offset="1" stopColor="#45DEC4" />
                      </linearGradient>
                      <linearGradient
                        gradientUnits="userSpaceOnUse"
                        id="paint1_linear_1364_100888"
                        x1="116"
                        x2="116"
                        y1="152"
                        y2="1.56712e-05"
                      >
                        <stop stopColor="var(--ds-background-200)" />
                        <stop
                          offset="0.322368"
                          stopColor="var(--ds-background-200)"
                          stopOpacity="0"
                        />
                      </linearGradient>
                      <clipPath id="clip0_1364_100888">
                        <rect fill="white" height="8" width="8" />
                      </clipPath>
                    </defs>
                  </svg>
                </div>

                <RotateCcw
                  size={35}
                  className="bg-background absolute top-9 translate-y-1/2 rounded-full border p-2"
                />
              </div>

              <div className="flex w-fit gap-6 self-end rounded-xl border border-dashed p-4 shadow-sm">
                <div className="space-y-3">
                  <div className="flex items-center justify-between text-sm">
                    <div>
                      <span className="font-bold"> vercel-site/</span>
                      <span className="self-end font-bold">ai</span>
                    </div>
                    <span className="text-muted-foreground text-xs">
                      1d ago
                    </span>
                  </div>
                  <p className="flex items-center gap-2 text-sm">
                    <GitBranch strokeWidth={2} size={14} />
                    ba5f55f <span className="">Update bento box design</span>
                  </p>
                </div>
                <div className="flex items-center justify-center">
                  <span className="absolute text-xs">55</span>
                  <svg
                    aria-hidden="true"
                    fill="none"
                    height="32"
                    strokeWidth="2"
                    viewBox="0 0 100 100"
                    width="32"
                  >
                    <circle
                      cx="50"
                      cy="50"
                      r="45"
                      strokeWidth="10"
                      strokeDashoffset="0"
                      strokeLinecap="round"
                      strokeLinejoin="round"
                      stroke="red"
                    ></circle>
                  </svg>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  )
}
```
##### File: `connected.tsx`

```tsx
"use client"

import { CircleArrowUp, Lock, MessageSquare, Terminal } from "@aliimam/icons"

export function Connected() {
  return (
    <div className="mx-auto flex max-w-6xl flex-col items-center justify-center px-4 lg:px-0">
      <div className="w-full border-x border-b">
        <div className="grid md:grid-cols-2">
          <div className="p-10">
            <h1 className="text-muted-foreground lg:text-md flex w-full gap-1 text-sm">
              <Terminal size={18} />
              Git-connected Deploys
            </h1>
            <h1 className="w-full pt-4 text-sm font-semibold tracking-tight lg:text-2xl">
              From localhost to https, in seconds.
            </h1>
            <h1 className="text-muted-foreground w-full text-sm font-medium tracking-tight lg:text-2xl">
              Deploy from Git or your CLI.
            </h1>
            <div className="mt-10 w-60 rounded-xl border p-4 shadow-sm lg:w-80">
              <div className="mb-3 flex gap-1">
                <div className="bg-primary/30 h-2 w-2 rounded-full" />
                <div className="bg-primary/30 h-2 w-2 rounded-full" />
                <div className="bg-primary/30 h-2 w-2 rounded-full" />
              </div>
              <p className="font-mono text-[10px]">
                ▲ ~ vercel-site/ git push Enumerating objects: 1, done. Counting
                objects: 100% (1/1), done. Writing objects: 100% (1/1), 72
                bytes, done. Total 1 (delta 0), reused 0 (delta 0). To
                github.com:vercel/ vercel-site.git 21326a9..81663c3 main - main
              </p>
            </div>
            <div className="bg-background relative z-10 -mt-10 ml-30 w-fit rounded-xl border p-4 shadow-sm lg:w-90">
              <div className="mb-3 grid grid-cols-3 items-center gap-1">
                <div className="flex gap-1">
                  <div className="h-2 w-2 rounded-full bg-red-500" />
                  <div className="h-2 w-2 rounded-full bg-blue-500" />
                  <div className="h-2 w-2 rounded-full bg-green-500" />
                </div>
                <div className="flex items-center gap-2">
                  <Lock size={12} />
                  <p className="font-mono text-[10px]">vercel.com</p>
                </div>
              </div>
              <div className="flex flex-col items-center justify-center border p-6 pb-0">
                <span className="text-muted-foreground text-center text-3xl font-bold">
                  What will you ship?
                </span>
                <div className="h-0 w-0 border-r-28 border-b-48 border-l-28 border-r-transparent border-l-transparent pt-6"></div>
              </div>
            </div>
          </div>
          <div className="border-t p-10 md:border-t-0 md:border-l">
            <h1 className="text-muted-foreground lg:text-md flex w-full gap-1 text-sm">
              <MessageSquare size={18} />
              Collaborative Pre-production
            </h1>
            <h1 className="w-full pt-4 text-sm font-semibold tracking-tight lg:text-2xl">
              Every deploy is remarkable.
            </h1>
            <h1 className="text-muted-foreground w-full text-sm font-medium tracking-tight lg:text-2xl">
              Chat with your team on real, production-grade UI, not just
              designs.
            </h1>
            <div className="mt-10 flex flex-col space-y-4">
              <div className="bg-background w-60 self-start rounded-xl border p-4 shadow-sm">
                <p className="text-sm">
                  Swapped out the{" "}
                  <span className="bg-secondary rounded-md p-1 px-2">
                    button
                  </span>{" "}
                  for some variant we need
                </p>
              </div>
              <div className="self-end">
                <div className="bg-background flex items-center gap-4">
                  <div id="pointer" className="relative">
                    <svg
                      width="16.8"
                      height="18.2"
                      viewBox="0 0 12 13"
                      className="fill-blue-500"
                      stroke="white"
                      strokeWidth="1"
                      xmlns="http://www.w3.org/2000/svg"
                      transform="scale(-1,1) translate(-12,0)"
                    >
                      <path
                        fillRule="evenodd"
                        clipRule="evenodd"
                        d="M12 5.50676L0 0L2.83818 13L6.30623 7.86537L12 5.50676V5.50676Z"
                      />
                    </svg>

                    <span className="text-primary-foreground relative -top-1 right-3 rounded-3xl bg-blue-500 px-2 py-1 text-xs">
                      AI
                    </span>
                  </div>
                  <p className="w-fit rounded-xl border p-4 text-sm shadow-sm">
                    How about this instead?
                  </p>
                </div>
              </div>
              <div className="self-start">
                <div className="bg-background flex items-center gap-4">
                  <p className="w-60 rounded-xl border p-4 text-sm shadow-sm">
                    I like it. Does this work with the brand tweaks{" "}
                    <strong>@aliimam-in</strong>
                  </p>
                  <div id="pointer" className="relative">
                    <svg
                      width="16.8"
                      height="18.2"
                      viewBox="0 0 12 13"
                      className="fill-rose-500"
                      stroke="white"
                      strokeWidth="1"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        fillRule="evenodd"
                        clipRule="evenodd"
                        d="M12 5.50676L0 0L2.83818 13L6.30623 7.86537L12 5.50676V5.50676Z"
                      />
                    </svg>

                    <span className="text-primary-foreground relative -top-1 left-3 rounded-3xl bg-rose-500 px-2 py-1 text-xs">
                      Ali Imam
                    </span>
                  </div>
                </div>
              </div>
              <div className="self-end">
                <div className="bg-background flex items-end gap-4">
                  <p className="flex w-fit items-center gap-3 rounded-xl border p-4 text-sm shadow-sm">
                    This Looks Graet!
                    <CircleArrowUp />
                  </p>
                  <div id="pointer" className="relative">
                    <svg
                      width="16.8"
                      height="18.2"
                      viewBox="0 0 12 13"
                      className="fill-green-500"
                      stroke="white"
                      strokeWidth="1"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        fillRule="evenodd"
                        clipRule="evenodd"
                        d="M12 5.50676L0 0L2.83818 13L6.30623 7.86537L12 5.50676V5.50676Z"
                      />
                    </svg>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  )
}
```
##### File: `ovservality.tsx`

```tsx
"use client"

import { ChartArea, Plus } from "@aliimam/icons"

export function Ovservality() {
  return (
    <div className="mx-auto flex max-w-6xl flex-col items-center justify-center px-4 lg:px-0">
      <div className="bg-background relative w-full border-x border-b">
        <Plus
          size={30}
          strokeWidth={0.8}
          className="absolute -bottom-4 -left-4"
        />
        <Plus
          size={30}
          strokeWidth={0.8}
          className="absolute -top-4 -right-4"
        />
        <div className="grid">
          <div className="p-10">
            <h1 className="text-muted-foreground lg:text-md flex w-full gap-1 text-sm">
              <ChartArea size={18} />
              Observability
            </h1>
            <h1 className="w-full pt-4 text-sm font-semibold tracking-tight lg:text-2xl">
              Route-aware observability.
            </h1>
            <h1 className="text-muted-foreground w-full max-w-sm text-sm font-medium tracking-tight lg:text-2xl">
              Monitor and analyze the performance and traffic of your projects.
            </h1>
            <div className="lg:-mt-32">
              <img
                className="size-full dark:hidden"
                alt={`Ali's avatar`}
                src={"/pages/analytics-large-light.avif"}
                fetchPriority="high"
              />
              <img
                className="hidden size-full dark:block"
                alt={`Ali's avatar`}
                src={"/pages/analytics-large-dark.avif"}
                fetchPriority="high"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  )
}
```
##### File: `ready.tsx`

```tsx
"use client"

import { Plus } from "@aliimam/icons"

import { Button } from "@/registry/aliimam/ui/button"

export function Ready() {
  return (
    <div className="mx-auto flex max-w-6xl flex-col items-center justify-center px-4 lg:px-0">
      <div className="relative grid w-full border-x border-b md:grid-cols-3">
        <Plus size={30} strokeWidth={0.8} className="absolute -top-4 -left-4" />
        <Plus
          size={30}
          strokeWidth={0.8}
          className="absolute -right-4 -bottom-4"
        />
        <div className="col-span-2 flex max-w-2xl flex-col p-10">
          <h1 className="w-full pt-4 text-sm font-semibold tracking-tight lg:text-2xl">
            Ready to deploy?{" "}
            <span className="text-muted-foreground">
              Start building with a free account. Speak to an expert for your
            </span>{" "}
            <span className="text-blue-500">Pro</span>{" "}
            <span className="text-muted-foreground">or</span>{" "}
            <span className="text-violet-500">Enterprise</span>{" "}
            <span className="text-muted-foreground">needs.</span>
          </h1>
          <div className="mt-6 flex flex-wrap gap-3">
            <Button className="h-10 w-fit rounded-full px-6">
              Start Deploying
            </Button>

            <Button
              variant={"outline"}
              className="h-10 w-fit rounded-full px-6"
            >
              Talk to an expert
            </Button>
          </div>
        </div>
        <div className="col-span-1 flex flex-col border-t p-10 md:max-w-xl md:border-t-0 md:border-l">
          <h1 className="w-full pt-4 text-sm font-semibold tracking-tight lg:text-lg">
            Explore Vercel Enterprise{" "}
            <span className="text-muted-foreground">
              with an interactive product tour, trial, or a personalized demo.
            </span>
          </h1>
          <div className="mt-6 flex gap-3">
            <Button
              variant={"outline"}
              className="h-10 w-fit rounded-full px-6"
            >
              Explore Enterprise
            </Button>
          </div>
        </div>
      </div>
    </div>
  )
}
```
##### File: `scale.tsx`

```tsx
/** Auto-generated - Do not edit */
'use client';
import React from 'react';

export interface ScaleProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}

export const Scale = React.forwardRef<SVGSVGElement, ScaleProps>(
  ({ size = 24, className = '', strokeWidth = 1, ...props }, ref) => (
    <svg 
      ref={ref}
      width={size}
      height={size} 
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      className={className}
      xmlns="http://www.w3.org/2000/svg"
      {...(strokeWidth !== undefined ? { strokeWidth } : {})}
      {...props}
    >
      <path d="M12 3v18" />
  <path d="m19 8 3 8a5 5 0 0 1-6 0zV7" />
  <path d="M3 7h1a17 17 0 0 0 8-2 17 17 0 0 0 8 2h1" />
  <path d="m5 8 3 8a5 5 0 0 1-6 0zV7" />
  <path d="M7 21h10" />
    </svg>
  )
);
Scale.displayName = "Scale";
export const ScaleMetadata = { 
  id: "scale", 
  baseId: "scale", 
  variant: "default", 
  name: "Scale", 
  category: "others", 
  tags: [], 
  viewBox: "0 0 24 24" 
} as const;

export default Scale;
```
---

### Landing 01 <a name="landing-01"></a>

A simple landing page.

- **Registry Name**: `aliimam/landing-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/landing-01.json
```

#### Dependencies

- **NPM Packages**:
  - ``
- **Registry Dependencies**:
  - `button`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
##### File: `bento-section.tsx`

```tsx
import { BentoGrid, BentoGridItem } from "@/registry/aliimam/components/bento"
import { GridPattern } from "@/registry/aliimam/components/grid-pattern"
import { Badge } from "@/registry/aliimam/ui/badge"
import { Button } from "@/registry/aliimam/ui/button"

export function BentoSection() {
  return (
    <div className="flex w-full flex-col items-center justify-center">
      <div className="flex items-center justify-center gap-6 self-stretch px-4 py-8 sm:px-6 md:px-24 md:py-16">
        <div className="flex w-full max-w-4xl flex-col items-center justify-start gap-3 overflow-hidden">
          <Badge variant={"secondary"}>Creative Studio</Badge>
          <div className="flex w-full max-w-xl flex-col justify-center text-center text-xl leading-tight font-semibold tracking-tight sm:text-2xl md:text-3xl lg:text-5xl">
            Design that defines modern brands
          </div>
          <div className="text-muted-foreground self-stretch text-center text-sm leading-6">
            We craft thoughtful digital products
            <br className="hidden sm:block" />
            that blend strategy, clarity, and visual impact.
          </div>
        </div>
      </div>

      <div className="border-y p-4">
        <BentoGrid
          cols={{ base: 2, md: 3, lg: 4 }}
          rowHeight={{ base: "180px", md: "220px", lg: "240px" }}
          className=""
        >
          <BentoGridItem colSpan={2} rowSpan={2} className="rounded-none p-0">
            <div className="relative flex h-full flex-col justify-between p-8">
              <GridPattern
                width={80}
                height={80}
                squares={[
                  [6, 2],
                  [2, 3],
                  [4, 4],
                ]}
              />
              <div>
                <h1 className="text-3xl font-semibold tracking-tight md:text-5xl lg:text-6xl">
                  Crafted for <br /> Ambitious Ideas.
                </h1>
                <p className="text-muted-foreground mt-4 max-w-md text-sm md:text-base">
                  From concept to launch, we design experiences that feel
                  intuitive, refined, and built to scale.
                </p>
              </div>
              <div className="relative z-10 flex gap-3">
                <Button size="lg">Explore Projects</Button>
                <Button variant="outline" size="lg">
                  Start a Project
                </Button>
              </div>
            </div>
          </BentoGridItem>

          <BentoGridItem className="rounded-none">
            <div className="flex h-full flex-col justify-between">
              <span className="text-muted-foreground text-sm">
                Capabilities
              </span>
              <div className="space-y-2 text-sm font-medium">
                <p>Product Design</p>
                <p>Visual Identity</p>
                <p>Web Experiences</p>
                <p>Motion & Interaction</p>
              </div>
            </div>
          </BentoGridItem>

          <BentoGridItem className="rounded-none">
            <div className="flex h-full flex-col justify-between">
              <span className="text-muted-foreground text-sm">Approach</span>
              <p className="text-sm font-medium">
                Research → Strategy → Design → Launch
              </p>
              <p className="text-muted-foreground text-xs">
                Intentional design, measurable impact.
              </p>
            </div>
          </BentoGridItem>

          <BentoGridItem colSpan={2} className="rounded-none">
            <div className="flex h-full flex-col justify-between">
              <span className="text-muted-foreground text-sm">
                Highlighted Work
              </span>
              <div>
                <h2 className="text-2xl font-semibold tracking-tight">
                  AI Productivity Platform
                </h2>
                <p className="text-muted-foreground mt-2 text-sm">
                  Redesigned core workflows to improve usability and boost
                  retention across enterprise teams.
                </p>
              </div>
            </div>
          </BentoGridItem>

          <BentoGridItem className="rounded-none">
            <div className="flex h-full flex-col justify-between">
              <span className="text-muted-foreground text-sm">
                Client Words
              </span>
              <p className="text-sm italic">
                “A seamless blend of creativity and strategic thinking.”
              </p>
              <p className="text-muted-foreground text-xs">
                — Head of Product, Tech Company
              </p>
            </div>
          </BentoGridItem>

          <BentoGridItem className="flex items-center justify-center rounded-none">
            <div className="text-center">
              <h2 className="text-5xl font-bold tracking-tight">150+</h2>
              <p className="text-muted-foreground text-sm">
                Successful Launches
              </p>
            </div>
          </BentoGridItem>

          <BentoGridItem colSpan={2} className="rounded-none">
            <div className="flex h-full flex-col justify-between">
              <span className="text-muted-foreground text-sm">Our Toolkit</span>
              <p className="text-sm font-medium">
                Figma · Framer · Next.js · React · Tailwind
              </p>
              <p className="text-muted-foreground text-xs">
                Built for speed, clarity, and long-term growth.
              </p>
            </div>
          </BentoGridItem>
        </BentoGrid>
      </div>

      <div className="relative h-12 self-stretch overflow-hidden border-b">
        <div className="absolute inset-0 h-full w-full overflow-hidden">
          <div className="relative h-full w-full">
            {Array.from({ length: 300 }).map((_, i) => (
              <div
                key={i}
                className="outline-primary/40 absolute h-4 w-full origin-top-left -rotate-45 outline-[0.5px] outline-offset-[-0.25px]"
                style={{
                  top: `${i * 16 - 120}px`,
                  left: "-100%",
                  width: "300%",
                }}
              ></div>
            ))}
          </div>
        </div>
      </div>
    </div>
  )
}
```
##### File: `cta-section.tsx`

```tsx
"use client"

import { Button } from "@/registry/aliimam/ui/button"

export default function CTASection() {
  return (
    <div className="relative flex w-full flex-col items-center justify-center gap-2 overflow-hidden">
      <div className="relative z-10 flex items-center justify-center gap-6 self-stretch border-t border-b px-6 py-12 md:px-24 md:py-12">
        <div className="absolute inset-0 h-full w-full overflow-hidden">
          <div
            className="relative h-full w-full overflow-hidden"
            style={{
              background:
                "linear-gradient(to bottom, var(--background) 0%, var(--background) 40%, rgba(255,255,255,0) 100%), radial-gradient(ellipse at 50% 120%, var(--primary) 0%, var(--background) 70%)",
            }}
          >
            <div
              style={{
                WebkitMaskImage:
                  "linear-gradient(to bottom, rgba(0,0,0,0) 0%, rgba(0,0,0,0.6) 60%)",
                backgroundImage:
                  "repeating-conic-gradient(from 0deg at 50% 100%, var(--code) 0deg, var(--code) 2deg, transparent 2deg, transparent 10deg)",
                bottom: "-20%",
                height: "100%",
                left: "50%",
                maskImage:
                  "linear-gradient(to bottom, rgba(0,0,0,0) 0%, rgba(0,0,0,0.6) 100%)",
                pointerEvents: "none",
                position: "absolute",
                transform: "translateX(-50%)",
                width: "200%",
              }}
            />
          </div>
        </div>

        <div className="relative z-20 flex w-full max-w-3xl flex-col items-center justify-start gap-6 overflow-hidden px-6 py-5 md:py-8">
          <div className="flex flex-col items-start justify-start gap-3 self-stretch">
            <div className="flex flex-col justify-center self-stretch text-center text-3xl leading-tight font-semibold tracking-tight md:text-5xl">
              Ready to elevate your digital presence?
            </div>
            <div className="text-muted-foreground self-stretch text-center text-base leading-7 font-medium">
              Let’s design experiences that captivate users,
              <br />
              strengthen your brand, and drive meaningful growth.
            </div>
          </div>
          <Button size={"lg"}>Start Your Project</Button>
        </div>
      </div>
    </div>
  )
}
```
##### File: `documentation-section.tsx`

```tsx
"use client"

import { Carousel } from "@/registry/aliimam/components/carousel"
import { Badge } from "@/registry/aliimam/ui/badge"

export default function DocumentationSection() {
  const slides = [
    <div
      key={"1"}
      className="bg-card text-card-foreground relative h-full w-full overflow-hidden rounded-md border"
    >
      <div className="h-full w-full overflow-hidden">
        <img
          src="/templates/ai-hero-black.jpg"
          className="h-full w-full object-cover"
        />
      </div>
    </div>,
    <div
      key={"2"}
      className="bg-card text-card-foreground relative h-full w-full overflow-hidden rounded-md border"
    >
      <div className="h-full w-full overflow-hidden">
        <img
          src="/templates/ai-icons.jpg"
          className="h-full w-full object-cover"
        />
      </div>
    </div>,
    <div
      key={"3"}
      className="bg-card text-card-foreground relative h-full w-full overflow-hidden rounded-md border"
    >
      <div className="h-full w-full overflow-hidden">
        <img
          src="/templates/ai-icons-1.jpg"
          className="h-full w-full object-cover"
        />
      </div>
    </div>,
    <div
      key={"4"}
      className="bg-card text-card-foreground relative h-full w-full overflow-hidden rounded-md border"
    >
      <div className="h-full w-full">
        <img
          src="/templates/ai-logos.jpg"
          className="h-full w-full object-cover"
        />
      </div>
    </div>,
    <div
      key={"5"}
      className="bg-card text-card-foreground relative h-full w-full overflow-hidden rounded-md border"
    >
      <div className="h-full w-full overflow-hidden">
        <img
          src="/templates/ai-logos-1.jpg"
          className="h-full w-full object-cover"
        />
      </div>
    </div>,
  ]

  return (
    <div className="relative flex w-full flex-col items-center justify-center">
      <div
        className="absolute inset-0 -z-10"
        style={{
          background:
            "linear-gradient(to bottom, var(--background) 0%, var(--background) 20%, rgba(255,255,255,0) 100%), radial-gradient(ellipse at 50% 120%, var(--primary) 0%, var(--background) 60%)",
        }}
      >
        <div
          style={{
            WebkitMaskImage:
              "linear-gradient(to bottom, rgba(0,0,0,0) 0%, rgba(0,0,0,0.8) 100%)",
            backgroundImage:
              "repeating-linear-gradient(90deg, var(--primary) 0px, var(--primary) 1px, transparent 1px, transparent 12px)",
            height: "100%",
            left: "0",
            maskImage:
              "linear-gradient(to bottom, rgba(0,0,0,0) 0%, rgba(0,0,0,0.8) 100%)",
            opacity: "0.25",
            pointerEvents: "none",
            position: "absolute",
            top: "0",
            width: "100%",
          }}
        />
      </div>
      <div className="flex items-center justify-center gap-6 self-stretch px-4 py-8 sm:px-6 md:px-24 md:py-16">
        <div className="flex w-full max-w-4xl flex-col items-center justify-start gap-3 overflow-hidden">
          <Badge variant={"secondary"}>Selected Work</Badge>
          <div className="flex w-full max-w-xl flex-col justify-center text-center text-xl leading-tight font-semibold tracking-tight sm:text-2xl md:text-3xl lg:text-5xl">
            A closer look at our design craft
          </div>
          <div className="text-muted-foreground self-stretch text-center text-sm leading-6">
            Explore interfaces, branding systems, and product experiences
            <br className="hidden sm:block" />
            thoughtfully designed to balance beauty and performance.
          </div>
        </div>
      </div>

      <div className="mx-auto flex h-full w-full items-center justify-center border-b pb-20">
        <Carousel slides={slides} />
      </div>
    </div>
  )
}
```
##### File: `faq-section.tsx`

```tsx
"use client"

import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/registry/aliimam/ui/accordion"

interface FAQItem {
  question: string
  answer: string
}

const faqData: FAQItem[] = [
  {
    question: "What’s included in each pricing plan?",
    answer:
      "Each plan includes access to core features, regular updates, and dedicated support. Higher tiers unlock advanced automation, analytics, and priority assistance.",
  },
  {
    question: "Can I upgrade or downgrade anytime?",
    answer:
      "Yes. You can switch plans at any time. Upgrades take effect immediately, and downgrades apply at the start of your next billing cycle.",
  },
  {
    question: "Is there a free trial available?",
    answer:
      "Absolutely. We offer a free trial so you can explore all essential features before committing to a paid plan.",
  },
  {
    question: "Do you offer discounts for annual billing?",
    answer:
      "Yes. Annual plans come with a discounted rate compared to monthly billing, helping you save more as you scale.",
  },
  {
    question: "Is my data secure?",
    answer:
      "Security is a top priority. We use industry-standard encryption and best practices to ensure your data remains protected at all times.",
  },
  {
    question: "How do I get started?",
    answer:
      "Simply choose a plan that fits your needs, create your account, and you’ll be up and running in minutes.",
  },
]

export default function FAQSection() {
  return (
    <div className="flex w-full items-start justify-center">
      <div className="flex flex-1 flex-col gap-6 px-4 py-16 md:px-12 md:py-20 lg:flex-row lg:gap-12">
        <div className="flex w-full flex-col gap-4 lg:flex-1 lg:py-5">
          <h2 className="text-4xl leading-tight font-semibold tracking-tight">
            Frequently Asked Questions
          </h2>
          <p className="text-muted-foreground text-base leading-7">
            Everything you need to know about our pricing,
            <br className="hidden md:block" />
            plans, and billing.
          </p>
        </div>

        <div className="w-full lg:flex-1">
          <Accordion type="single" className="w-full">
            {faqData.map((item, index) => (
              <AccordionItem
                key={index}
                value={`item-${index}`}
                className="border-b"
              >
                <AccordionTrigger className="p-5 text-left text-base font-medium hover:no-underline">
                  {item.question}
                </AccordionTrigger>

                <AccordionContent className="p-5 text-sm leading-6">
                  {item.answer}
                </AccordionContent>
              </AccordionItem>
            ))}
          </Accordion>
        </div>
      </div>
    </div>
  )
}
```
##### File: `footer-section.tsx`

```tsx
import { Apple, Github, LinkedIn, X } from "@aliimam/logos"

export default function FooterSection() {
  return (
    <div className="flex w-full flex-col items-start justify-start pt-10">
      <div className="flex h-auto flex-col items-stretch justify-between self-stretch pt-0 pr-0 pb-8 md:flex-row">
        <div className="flex h-auto flex-col items-start justify-start gap-8 p-4 md:p-8">
          <div className="flex items-center justify-start gap-3 self-stretch">
            <div className="text-center text-xl leading-4 font-semibold">
              <Apple className="text-primary" />
            </div>
          </div>
          <div className="text-sm font-medium">
            <h1 className="text-lg font-medium">Design without limits</h1>
            <p className="text-muted-foreground max-w-md">
              I create digital experiences that connect and inspire. I build
              apps, websites, brands, and products end-to-end.
            </p>
          </div>
          <div className="flex items-start justify-start gap-6">
            <LinkedIn className="w-6" />
            <X className="w-6" />
            <Github className="w-6" />
          </div>
        </div>

        <div className="flex flex-col flex-wrap items-start justify-start gap-6 self-stretch p-4 sm:flex-row sm:justify-between md:gap-8 md:p-8">
          <div className="flex min-w-40 flex-1 flex-col items-start justify-start gap-3">
            <div className="self-stretch text-sm leading-5 font-medium">
              Product
            </div>
            <div className="flex flex-col items-start justify-end gap-2">
              <div className="text-muted-foreground hover:text-primary cursor-pointer text-sm leading-5 font-normal transition-colors">
                Features
              </div>
              <div className="text-muted-foreground hover:text-primary cursor-pointer text-sm leading-5 font-normal transition-colors">
                Pricing
              </div>
              <div className="text-muted-foreground hover:text-primary cursor-pointer text-sm leading-5 font-normal transition-colors">
                Live Previews
              </div>
              <div className="text-muted-foreground hover:text-primary cursor-pointer text-sm leading-5 font-normal transition-colors">
                Real-time Previews
              </div>
              <div className="text-muted-foreground hover:text-primary cursor-pointer text-sm leading-5 font-normal transition-colors">
                AI Agents
              </div>
            </div>
          </div>

          <div className="flex min-w-40 flex-1 flex-col items-start justify-start gap-3">
            <div className="text-sm leading-5 font-medium">Company</div>
            <div className="flex flex-col items-start justify-center gap-2">
              <div className="text-muted-foreground hover:text-primary cursor-pointer text-sm leading-5 font-normal transition-colors">
                About us
              </div>
              <div className="text-muted-foreground hover:text-primary cursor-pointer text-sm leading-5 font-normal transition-colors">
                Our team
              </div>
              <div className="text-muted-foreground hover:text-primary cursor-pointer text-sm leading-5 font-normal transition-colors">
                Careers
              </div>
              <div className="text-muted-foreground hover:text-primary cursor-pointer text-sm leading-5 font-normal transition-colors">
                Brand
              </div>
              <div className="text-muted-foreground hover:text-primary cursor-pointer text-sm leading-5 font-normal transition-colors">
                Contact
              </div>
            </div>
          </div>

          <div className="flex min-w-40 flex-1 flex-col items-start justify-start gap-3">
            <div className="text-sm leading-5 font-medium">Resources</div>
            <div className="flex flex-col items-center justify-center gap-2">
              <div className="text-muted-foreground hover:text-primary cursor-pointer self-stretch text-sm leading-5 font-normal transition-colors">
                Terms of use
              </div>
              <div className="text-muted-foreground hover:text-primary cursor-pointer self-stretch text-sm leading-5 font-normal transition-colors">
                API Reference
              </div>
              <div className="text-muted-foreground hover:text-primary cursor-pointer self-stretch text-sm leading-5 font-normal transition-colors">
                Documentation
              </div>
              <div className="text-muted-foreground hover:text-primary cursor-pointer self-stretch text-sm leading-5 font-normal transition-colors">
                Community
              </div>
              <div className="text-muted-foreground hover:text-primary cursor-pointer self-stretch text-sm leading-5 font-normal transition-colors">
                Support
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  )
}
```
##### File: `feature-cards.tsx`

```tsx
"use client"

import { useEffect, useRef, useState } from "react"

export function FeatureCards() {
  const [activeCard, setActiveCard] = useState(0)
  const [progress, setProgress] = useState(0)
  const mountedRef = useRef(true)

  useEffect(() => {
    const progressInterval = setInterval(() => {
      if (!mountedRef.current) return

      setProgress((prev) => {
        if (prev >= 100) {
          if (mountedRef.current) {
            setActiveCard((current) => (current + 1) % 3)
          }
          return 0
        }
        return prev + 2 // 2% every 100ms = 5 seconds total
      })
    }, 100)

    return () => {
      clearInterval(progressInterval)
      mountedRef.current = false
    }
  }, [])

  useEffect(() => {
    return () => {
      mountedRef.current = false
    }
  }, [])

  const handleCardClick = (index: number) => {
    if (!mountedRef.current) return
    setActiveCard(index)
    setProgress(0)
  }

  return (
    <div>
      <div className="pointer-events-none absolute top-60 left-1/2 -z-10 -translate-x-1/2 transform">
        <img
          src="/ai-logo.png"
          alt="AI Logo"
          className="h-auto w-120 opacity-30"
        />
      </div>
      <div className="relative z-5 my-8 flex w-full flex-col items-center justify-center gap-2">
        <div className="flex h-[520px] w-full max-w-5xl flex-col items-start justify-start overflow-hidden rounded-md border shadow-2xl">
          <div className="flex flex-1 items-start justify-start self-stretch">
            <div className="flex h-full w-full items-center justify-center">
              <div className="relative h-full w-full overflow-hidden">
                <div
                  className={`absolute inset-0 transition-all duration-500 ease-in-out ${
                    activeCard === 0
                      ? "blur-0 scale-100 opacity-100"
                      : "scale-95 opacity-0 blur-sm"
                  }`}
                >
                  <img
                    src="/templates/ai-hero-black.jpg"
                    alt="Bento Grid Dashboard"
                    className="aspect-video h-full w-full object-cover"
                  />
                </div>

                <div
                  className={`absolute inset-0 transition-all duration-500 ease-in-out ${
                    activeCard === 1
                      ? "blur-0 scale-100 opacity-100"
                      : "scale-95 opacity-0 blur-sm"
                  }`}
                >
                  <img
                    src="/templates/ai-icons.jpg"
                    alt="Bento Grid Dashboard"
                    className="aspect-video h-full w-full object-cover"
                  />
                </div>

                <div
                  className={`absolute inset-0 transition-all duration-500 ease-in-out ${
                    activeCard === 2
                      ? "blur-0 scale-100 opacity-100"
                      : "scale-95 opacity-0 blur-sm"
                  }`}
                >
                  <img
                    src="/templates/ai-icons-1.jpg"
                    alt="Bento Grid Dashboard"
                    className="aspect-video h-full w-full object-cover"
                  />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div className="mt-10 flex items-start justify-center self-stretch border-y">
        <div className="relative w-4 self-stretch overflow-hidden sm:w-6 md:w-8 lg:w-12">
          <div className="absolute -top-30 -left-4 flex w-40 flex-col items-start justify-start">
            {Array.from({ length: 50 }).map((_, i) => (
              <div
                key={i}
                className="outline-primary/40 h-4 origin-top-left -rotate-45 self-stretch outline-[0.5px] outline-offset-[-0.25px]"
              ></div>
            ))}
          </div>
        </div>

        <div className="flex flex-1 flex-col items-stretch justify-center gap-0 px-0 md:flex-row">
          <FeatureCard
            title="Build scalable design systems"
            description="Create consistent, reusable components that keep your brand unified across every platform."
            isActive={activeCard === 0}
            progress={activeCard === 0 ? progress : 0}
            onClick={() => handleCardClick(0)}
          />
          <FeatureCard
            title="Prototype with precision"
            description="Turn ideas into interactive prototypes quickly and refine user flows with clarity."
            isActive={activeCard === 1}
            progress={activeCard === 1 ? progress : 0}
            onClick={() => handleCardClick(1)}
          />
          <FeatureCard
            title="Collaborate with creatives"
            description="Work seamlessly with designers and developers using shared assets and real-time feedback."
            isActive={activeCard === 2}
            progress={activeCard === 2 ? progress : 0}
            onClick={() => handleCardClick(2)}
          />
        </div>

        <div className="relative w-4 self-stretch overflow-hidden sm:w-6 md:w-8 lg:w-12">
          <div className="absolute -top-30 -left-4 flex w-40 flex-col items-start justify-start">
            {Array.from({ length: 50 }).map((_, i) => (
              <div
                key={i}
                className="outline-primary/40 h-4 origin-top-left -rotate-45 self-stretch outline-[0.5px] outline-offset-[-0.25px]"
              ></div>
            ))}
          </div>
        </div>
      </div>
    </div>
  )
}

function FeatureCard({
  title,
  description,
  isActive,
  progress,
  onClick,
}: {
  title: string
  description: string
  isActive: boolean
  progress: number
  onClick: () => void
}) {
  return (
    <div
      className={`relative flex w-full cursor-pointer flex-col items-start justify-start gap-2 self-stretch overflow-hidden px-6 py-5 md:flex-1 ${
        isActive ? "bg-code border" : "border-r-0 border-l-0 md:border"
      }`}
      onClick={onClick}
    >
      {isActive && (
        <div className="absolute top-0 left-0 h-1 w-full">
          <div
            className="bg-primary h-full transition-all duration-100 ease-linear"
            style={{ width: `${progress}%` }}
          />
        </div>
      )}

      <div className="flex flex-col justify-center self-stretch text-sm font-semibold md:text-lg">
        {title}
      </div>
      <div className="text-muted-foreground self-stretch text-sm">
        {description}
      </div>
    </div>
  )
}
```
##### File: `header.tsx`

```tsx
import { Button } from "@/registry/aliimam/ui/button"

export function Header() {
  return (
    <div className="absolute top-0 left-0 z-20 flex h-12 w-full items-center justify-center px-6 sm:h-14 sm:px-8 md:h-16 md:px-12 lg:px-0">
      <div className="border-muted absolute top-6 left-0 h-0 w-full border-t sm:top-7 md:top-8"></div>

      <div className="bg-muted relative z-30 flex h-10 w-full max-w-[calc(100%-32px)] items-center justify-between overflow-hidden rounded-md border px-3 py-1.5 pr-2 backdrop-blur-sm sm:h-11 sm:max-w-[calc(100%-48px)] sm:px-4 sm:py-2 sm:pr-3 md:h-12 md:max-w-[calc(100%-64px)] md:px-2 lg:w-[700px] lg:max-w-[700px]">
        <div className="flex items-center justify-center">
          <div className="flex items-center justify-start">
            <div className="flex flex-col justify-center pl-2 text-sm leading-5 font-medium sm:text-base md:text-lg lg:text-xl">
              AI
            </div>
          </div>
          <div className="flex flex-row items-start justify-start gap-2 pl-3 sm:flex sm:gap-3 sm:pl-4 md:gap-4 md:pl-5 lg:gap-4 lg:pl-5">
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Designs
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Pricing
              </div>
            </div>
            <div className="flex items-center justify-start">
              <div className="text-muted-foreground flex flex-col justify-center text-xs font-medium md:text-[13px]">
                Docs
              </div>
            </div>
          </div>
        </div>
        <Button size={"sm"}>Log in</Button>
      </div>
    </div>
  )
}
```
##### File: `hero-section.tsx`

```tsx
import { Button } from "@/registry/aliimam/ui/button"

export function HeroSection() {
  return (
    <section className="relative pt-40 pb-16">
      <div className="mx-auto max-w-4xl px-4">
        <div className="flex flex-col items-center gap-12">
          <div className="flex flex-col items-center gap-3">
            <div className="flex flex-col items-center gap-6">
              <h1 className="max-w-4xl text-center text-5xl leading-tight font-medium md:text-7xl">
                Design experiences that people remember
              </h1>
              <p className="text-muted-foreground max-w-xl text-center text-lg leading-7 font-medium">
                From brand identity to polished interfaces, build bold,
                user-focused designs that elevate your product and stand out.
              </p>
            </div>
          </div>

          <div className="flex justify-center">
            <Button size={"lg"}>Explore our work</Button>
          </div>
        </div>
      </div>
    </section>
  )
}
```
##### File: `logo-section.tsx`

```tsx
import {
  ClaudeAIWordmark,
  CursorWordmark,
  GithubWordmark,
  GoogleGeminiWordmark,
  GoogleWordmark,
  GrokWordmark,
  OpenAIWordmark,
  PerplexityAIWordmark,
  ReplicateWordmark,
  ResendWordmark,
  SunoWordmark,
  YouTubeWordmark,
} from "@aliimam/logos"

import { Badge } from "@/registry/aliimam/ui/badge"

export function LogoSection() {
  return (
    <div className="flex w-full flex-col items-center justify-center">
      <div className="flex items-center justify-center gap-6 self-stretch px-4 py-8 sm:px-6 md:px-24 md:py-16">
        <div className="flex w-full max-w-4xl flex-col items-center justify-start gap-3 overflow-hidden">
          <Badge variant={"secondary"}>Trusted by Creative Teams</Badge>
          <div className="flex w-full max-w-xl flex-col justify-center text-center text-xl leading-tight font-semibold tracking-tight sm:text-2xl md:text-3xl lg:text-5xl">
            Powering world-class design
          </div>
          <div className="text-muted-foreground self-stretch text-center text-sm leading-6">
            Designers and product teams rely on our tools
            <br className="hidden sm:block" />
            to craft consistent, scalable, and beautiful experiences.
          </div>
        </div>
      </div>

      <div className="flex items-start justify-center self-stretch border-y">
        <div className="relative w-4 self-stretch overflow-hidden sm:w-6 md:w-8 lg:w-12">
          <div className="absolute -top-30 -left-10 flex w-40 flex-col items-start justify-start">
            {Array.from({ length: 50 }).map((_, i) => (
              <div
                key={i}
                className="outline-primary/40 h-4 origin-top-left -rotate-45 self-stretch outline outline-offset-[-0.25px]"
              />
            ))}
          </div>
        </div>

        <div className="w-full">
          <div className="mx-auto grid w-full grid-cols-2 sm:grid-cols-4 lg:grid-cols-6">
            {[
              <OpenAIWordmark key="openai" size={80} />,
              <ClaudeAIWordmark key="claude" size={80} />,
              <ReplicateWordmark key="replicate" size={80} />,
              <CursorWordmark key="cursor" size={80} />,
              <GoogleGeminiWordmark key="gemini" size={80} />,
              <GithubWordmark key="github" size={80} />,
              <GrokWordmark key="grok" size={80} />,
              <GoogleWordmark key="google" size={80} />,
              <SunoWordmark key="suno" size={80} />,
              <ResendWordmark key="resend" size={80} />,
              <YouTubeWordmark key="youtube" size={80} />,
              <PerplexityAIWordmark key="perp" size={80} />,
            ].map((Logo, i) => (
              <div
                key={i}
                className="flex h-20 w-full items-center justify-center border md:h-24"
              >
                {Logo}
              </div>
            ))}
          </div>
        </div>

        <div className="relative w-4 self-stretch overflow-hidden sm:w-6 md:w-8 lg:w-12">
          <div className="absolute -top-30 -left-10 flex w-40 flex-col items-start justify-start">
            {Array.from({ length: 50 }).map((_, i) => (
              <div
                key={i}
                className="outline-primary/40 h-4 origin-top-left -rotate-45 self-stretch outline outline-offset-[-0.25px]"
              />
            ))}
          </div>
        </div>
      </div>
    </div>
  )
}
```
##### File: `pricing-section.tsx`

```tsx
"use client"

import { useState } from "react"
import { Check } from "@aliimam/icons"

import { Badge } from "@/registry/aliimam/ui/badge"
import { Button } from "@/registry/aliimam/ui/button"
import { Tabs, TabsList, TabsTrigger } from "@/registry/aliimam/ui/tabs"

export default function PricingSection() {
  const [billingPeriod, setBillingPeriod] = useState<"monthly" | "annually">(
    "annually"
  )

  const pricing = {
    starter: {
      monthly: 0,
      annually: 0,
    },
    professional: {
      monthly: 50,
      annually: 25, // 20% discount for annual
    },
    enterprise: {
      monthly: 500,
      annually: 250, // 20% discount for annual
    },
  }

  return (
    <div className="flex w-full flex-col items-center justify-center gap-2">
      <div className="flex items-center justify-center gap-6 self-stretch px-4 py-8">
        <div className="flex w-full max-w-4xl flex-col items-center justify-start gap-3 overflow-hidden">
          <Badge variant={"secondary"}>Pricing</Badge>
          <div className="flex w-full max-w-xl flex-col justify-center text-center text-xl leading-tight font-semibold tracking-tight sm:text-2xl md:text-3xl lg:text-5xl">
            Simple, transparent pricing
          </div>
          <div className="text-muted-foreground self-stretch text-center text-sm leading-6">
            Choose a plan that fits your needs
            <br className="hidden sm:block" />
            with no hidden fees and flexible upgrades anytime.
          </div>
        </div>
      </div>

      <div className="flex items-center justify-center self-stretch px-6 py-9 md:px-16">
        <Tabs
          value={billingPeriod}
          onValueChange={(value) =>
            setBillingPeriod(value as "monthly" | "annually")
          }
          className="w-auto"
        >
          <TabsList className="grid w-65 grid-cols-2 rounded-full">
            <TabsTrigger
              value="annually"
              className="data-[state=active]:bg-background rounded-full"
            >
              Annually
            </TabsTrigger>
            <TabsTrigger
              value="monthly"
              className="data-[state=active]:bg-background rounded-full"
            >
              Monthly
            </TabsTrigger>
          </TabsList>
        </Tabs>
      </div>

      <div className="flex items-center justify-center self-stretch border-y">
        <div className="flex w-full items-start justify-center">
          <div className="relative w-4 self-stretch overflow-hidden sm:w-6 md:w-8 lg:w-12">
            <div className="absolute -top-30 -left-10 flex w-40 flex-col items-start justify-start">
              {Array.from({ length: 50 }).map((_, i) => (
                <div
                  key={i}
                  className="outline-primary/40 h-4 origin-top-left -rotate-45 self-stretch outline outline-offset-[-0.25px]"
                />
              ))}
            </div>
          </div>

          <div className="flex flex-1 flex-col items-center justify-center md:flex-row md:gap-6">
            <div className="flex max-w-full flex-1 flex-col items-start justify-start gap-12 self-stretch overflow-hidden border-x px-6 py-5 md:max-w-none">
              <div className="flex flex-col items-center justify-start gap-9 self-stretch">
                <div className="flex flex-col items-start justify-start gap-2 self-stretch">
                  <div className="text-lg leading-7 font-medium">Creator</div>
                  <div className="text-muted-foreground w-full max-w-80 text-sm leading-5 font-normal">
                    Ideal for freelance designers and creative beginners.
                  </div>
                </div>

                <div className="flex flex-col items-start justify-start gap-2 self-stretch">
                  <div className="flex flex-col items-start justify-start gap-1">
                    <div className="relative flex h-15 items-center text-5xl font-medium">
                      <span className="invisible">
                        ${pricing.starter[billingPeriod]}
                      </span>
                      <span
                        className="absolute inset-0 flex items-center transition-all duration-500"
                        style={{
                          opacity: billingPeriod === "annually" ? 1 : 0,
                          transform: `scale(${billingPeriod === "annually" ? 1 : 0.8})`,
                          filter: `blur(${billingPeriod === "annually" ? 0 : 4}px)`,
                        }}
                        aria-hidden={billingPeriod !== "annually"}
                      >
                        ${pricing.starter.annually}
                      </span>
                      <span
                        className="absolute inset-0 flex items-center transition-all duration-500"
                        style={{
                          opacity: billingPeriod === "monthly" ? 1 : 0,
                          transform: `scale(${billingPeriod === "monthly" ? 1 : 0.8})`,
                          filter: `blur(${billingPeriod === "monthly" ? 0 : 4}px)`,
                        }}
                        aria-hidden={billingPeriod !== "monthly"}
                      >
                        ${pricing.starter.monthly}
                      </span>
                    </div>
                    <div className="text-sm font-medium">
                      per {billingPeriod === "monthly" ? "month" : "year"}, per
                      user.
                    </div>
                  </div>
                </div>

                <Button size={"lg"} className="w-full">
                  Start for free
                </Button>
              </div>

              <div className="flex flex-col items-start justify-start gap-2 self-stretch">
                {[
                  "Up to 5 design projects",
                  "Basic brand kit tools",
                  "Community feedback access",
                  "Starter UI components",
                  "Export in PNG & JPG",
                ].map((feature, index) => (
                  <div
                    key={index}
                    className="flex items-center justify-start gap-3 self-stretch"
                  >
                    <div className="relative flex h-4 w-4 items-center justify-center">
                      <Check />
                    </div>
                    <div className="flex-1 text-[12.5px] font-normal">
                      {feature}
                    </div>
                  </div>
                ))}
              </div>
            </div>

            <div className="bg-foreground flex max-w-full flex-1 flex-col items-start justify-start gap-12 self-stretch overflow-hidden border-x px-6 py-5 md:max-w-none">
              <div className="flex flex-col items-center justify-start gap-9 self-stretch">
                <div className="flex flex-col items-start justify-start gap-2 self-stretch">
                  <div className="text-background text-lg leading-7 font-medium">
                    Studio
                  </div>
                  <div className="text-muted w-full max-w-80 text-sm leading-5 font-normal">
                    Advanced toolkit for agencies and growing creative teams.
                  </div>
                </div>

                <div className="flex flex-col items-start justify-start gap-2 self-stretch">
                  <div className="flex flex-col items-start justify-start gap-1">
                    <div className="text-background relative flex h-15 items-center text-5xl font-medium">
                      <span className="invisible">
                        ${pricing.professional[billingPeriod]}
                      </span>
                      <span
                        className="absolute inset-0 flex items-center transition-all duration-500"
                        style={{
                          opacity: billingPeriod === "annually" ? 1 : 0,
                          transform: `scale(${billingPeriod === "annually" ? 1 : 0.8})`,
                          filter: `blur(${billingPeriod === "annually" ? 0 : 4}px)`,
                        }}
                        aria-hidden={billingPeriod !== "annually"}
                      >
                        ${pricing.professional.annually}
                      </span>
                      <span
                        className="absolute inset-0 flex items-center transition-all duration-500"
                        style={{
                          opacity: billingPeriod === "monthly" ? 1 : 0,
                          transform: `scale(${billingPeriod === "monthly" ? 1 : 0.8})`,
                          filter: `blur(${billingPeriod === "monthly" ? 0 : 4}px)`,
                        }}
                        aria-hidden={billingPeriod !== "monthly"}
                      >
                        ${pricing.professional.monthly}
                      </span>
                    </div>
                    <div className="text-background text-sm font-medium">
                      per {billingPeriod === "monthly" ? "month" : "year"}, per
                      user.
                    </div>
                  </div>
                </div>
                <Button size={"lg"} className="w-full">
                  Start Creating
                </Button>
              </div>

              <div className="flex flex-col items-start justify-start gap-2 self-stretch">
                {[
                  "Unlimited design projects",
                  "Complete brand management",
                  "Advanced UI component library",
                  "Figma & Adobe integration",
                  "Vector & SVG export",
                  "Team collaboration workspace",
                  "Priority creative support",
                  "Version history & backups",
                ].map((feature, index) => (
                  <div
                    key={index}
                    className="flex items-center justify-start gap-3 self-stretch"
                  >
                    <div className="relative flex h-4 w-4 items-center justify-center">
                      <Check className="text-background" />
                    </div>
                    <div className="text-background flex-1 text-[12.5px] leading-5 font-normal">
                      {feature}
                    </div>
                  </div>
                ))}
              </div>
            </div>

            <div className="flex max-w-full flex-1 flex-col items-start justify-start gap-12 self-stretch overflow-hidden border-x px-6 py-5 md:max-w-none">
              <div className="flex flex-col items-center justify-start gap-9 self-stretch">
                <div className="flex flex-col items-start justify-start gap-2 self-stretch">
                  <div className="text-lg leading-7 font-medium">
                    Agency Pro
                  </div>
                  <div className="w-full max-w-80 text-sm leading-5 font-normal">
                    Full-scale creative infrastructure for large design teams.
                  </div>
                </div>

                <div className="flex flex-col items-start justify-start gap-2 self-stretch">
                  <div className="flex flex-col items-start justify-start gap-1">
                    <div className="relative flex h-15 items-center text-5xl font-medium">
                      <span className="invisible">
                        ${pricing.enterprise[billingPeriod]}
                      </span>
                      <span
                        className="absolute inset-0 flex items-center transition-all duration-500"
                        style={{
                          opacity: billingPeriod === "annually" ? 1 : 0,
                          transform: `scale(${billingPeriod === "annually" ? 1 : 0.8})`,
                          filter: `blur(${billingPeriod === "annually" ? 0 : 4}px)`,
                        }}
                        aria-hidden={billingPeriod !== "annually"}
                      >
                        ${pricing.enterprise.annually}
                      </span>
                      <span
                        className="absolute inset-0 flex items-center transition-all duration-500"
                        style={{
                          opacity: billingPeriod === "monthly" ? 1 : 0,
                          transform: `scale(${billingPeriod === "monthly" ? 1 : 0.8})`,
                          filter: `blur(${billingPeriod === "monthly" ? 0 : 4}px)`,
                        }}
                        aria-hidden={billingPeriod !== "monthly"}
                      >
                        ${pricing.enterprise.monthly}
                      </span>
                    </div>
                    <div className="text-sm font-medium text-[#847971]">
                      per {billingPeriod === "monthly" ? "month" : "year"}, per
                      user.
                    </div>
                  </div>
                </div>
                <Button size={"lg"} className="w-full">
                  Upgrade to Studio
                </Button>
              </div>

              <div className="flex flex-col items-start justify-start gap-2 self-stretch">
                {[
                  "Everything in Studio",
                  "Dedicated creative strategist",
                  "White-label design system",
                  "Custom component development",
                  "Advanced asset management",
                  "SSO & enterprise security",
                  "Custom contracts & billing",
                  "24/7 premium support",
                ].map((feature, index) => (
                  <div
                    key={index}
                    className="flex items-center justify-start gap-3 self-stretch"
                  >
                    <div className="relative flex h-4 w-4 items-center justify-center">
                      <Check />
                    </div>
                    <div className="flex-1 text-[12.5px] font-normal">
                      {feature}
                    </div>
                  </div>
                ))}
              </div>
            </div>
          </div>

          <div className="relative w-4 self-stretch overflow-hidden sm:w-6 md:w-8 lg:w-12">
            <div className="absolute -top-30 -left-10 flex w-40 flex-col items-start justify-start">
              {Array.from({ length: 50 }).map((_, i) => (
                <div
                  key={i}
                  className="outline-primary/40 h-4 origin-top-left -rotate-45 self-stretch outline outline-offset-[-0.25px]"
                />
              ))}
            </div>
          </div>
        </div>
      </div>
    </div>
  )
}
```
##### File: `testimonials-section.tsx`

```tsx
"use client"

import { useEffect, useState } from "react"
import { ChevronLeft, ChevronRight } from "@aliimam/icons"

import { Badge } from "@/registry/aliimam/ui/badge"
import { Button } from "@/registry/aliimam/ui/button"

export default function TestimonialsSection() {
  const [activeTestimonial, setActiveTestimonial] = useState(0)
  const [isTransitioning, setIsTransitioning] = useState(false)

  const testimonials = [
    {
      quote:
        "Working with this team elevated our entire product experience. Every interaction now feels intentional and beautifully crafted.",
      name: "Aarav Mehta",
      company: "Founder, Nova Labs",
      image: "https://github.com/shadcn.png",
    },
    {
      quote:
        "Their design thinking completely reshaped our platform. The clarity, usability, and visual polish exceeded our expectations.",
      name: "Emily Carter",
      company: "Head of Product, Lumina",
      image: "https://github.com/vercel.png",
    },
    {
      quote:
        "From branding to interface design, the attention to detail was exceptional. Our launch received incredible feedback from users.",
      name: "Daniel Kim",
      company: "CEO, Horizon Tech",
      image: "https://github.com/claude.png",
    },
  ]

  useEffect(() => {
    const interval = setInterval(() => {
      setIsTransitioning(true)
      setTimeout(() => {
        setActiveTestimonial((prev) => (prev + 1) % testimonials.length)
        setTimeout(() => {
          setIsTransitioning(false)
        }, 100)
      }, 300)
    }, 12000)

    return () => clearInterval(interval)
  }, [testimonials.length])

  const handleNavigationClick = (index: number) => {
    setIsTransitioning(true)
    setTimeout(() => {
      setActiveTestimonial(index)
      setTimeout(() => {
        setIsTransitioning(false)
      }, 100)
    }, 300)
  }

  return (
    <div className="flex w-full flex-col items-center justify-center">
      <div className="flex items-center justify-center gap-6 self-stretch px-4 py-8 sm:px-6 md:px-24 md:py-16">
        <div className="flex w-full max-w-4xl flex-col items-center justify-start gap-3 overflow-hidden">
          <Badge variant={"secondary"}>Client Stories</Badge>
          <div className="flex w-full max-w-xl flex-col justify-center text-center text-xl leading-tight font-semibold tracking-tight sm:text-2xl md:text-3xl lg:text-5xl">
            Trusted by forward-thinking brands
          </div>
          <div className="text-muted-foreground self-stretch text-center text-sm leading-6">
            We partner with ambitious teams
            <br className="hidden sm:block" />
            to design meaningful digital experiences that deliver impact.
          </div>
        </div>
      </div>

      <div className="bg-background flex items-center justify-start self-stretch overflow-hidden border border-t-0 border-r-0 border-b border-l-0 px-2">
        <div className="flex flex-1 flex-col items-end justify-center gap-6 py-16 md:flex-row md:py-17">
          <div className="flex flex-col items-start justify-center gap-4 self-stretch px-3 md:flex-row md:px-12">
            <img
              className="h-50 w-48 rounded-lg object-cover transition-all duration-700 ease-in-out md:h-50 md:w-48"
              style={{
                opacity: isTransitioning ? 0.6 : 1,
                transform: isTransitioning ? "scale(0.95)" : "scale(1)",
                transition:
                  "opacity 0.7s ease-in-out, transform 0.7s ease-in-out",
              }}
              src={testimonials[activeTestimonial].image || "/placeholder.svg"}
              alt={testimonials[activeTestimonial].name}
            />
            <div className="flex flex-1 flex-col items-start justify-start gap-3 overflow-hidden px-6">
              <div
                className="line-clamp-5 flex h-40 flex-col justify-start self-stretch overflow-hidden text-4xl font-medium tracking-tight transition-all duration-700 ease-in-out"
                style={{
                  filter: isTransitioning ? "blur(4px)" : "blur(0px)",
                  transition: "filter 0.7s ease-in-out",
                }}
              >
                {testimonials[activeTestimonial].quote}
              </div>
              <div
                className="flex items-start justify-start gap-1 self-stretch transition-all duration-700 ease-in-out"
                style={{
                  filter: isTransitioning ? "blur(4px)" : "blur(0px)",
                  transition: "filter 0.7s ease-in-out",
                }}
              >
                <div className="flex flex-col justify-center self-stretch text-lg font-medium">
                  {testimonials[activeTestimonial].name} -
                </div>
                <div className="text-muted-foreground flex flex-col justify-center self-stretch text-lg font-medium">
                  {testimonials[activeTestimonial].company}
                </div>
              </div>
            </div>
          </div>

          <div className="flex items-start justify-start gap-3 pr-6">
            <Button
              size={"icon"}
              variant={"outline"}
              onClick={() =>
                handleNavigationClick(
                  (activeTestimonial - 1 + testimonials.length) %
                    testimonials.length
                )
              }
            >
              <ChevronLeft />
            </Button>
            <Button
              size={"icon"}
              variant={"outline"}
              onClick={() =>
                handleNavigationClick(
                  (activeTestimonial + 1) % testimonials.length
                )
              }
            >
              <ChevronRight />
            </Button>
          </div>
        </div>
      </div>

      <div className="relative h-12 self-stretch overflow-hidden border-b">
        <div className="absolute inset-0 h-full w-full overflow-hidden">
          <div className="relative h-full w-full">
            {Array.from({ length: 300 }).map((_, i) => (
              <div
                key={i}
                className="outline-primary/40 absolute h-4 w-full origin-top-left -rotate-45 outline-[0.5px] outline-offset-[-0.25px]"
                style={{
                  top: `${i * 16 - 120}px`,
                  left: "-100%",
                  width: "300%",
                }}
              ></div>
            ))}
          </div>
        </div>
      </div>
    </div>
  )
}
```
---

### Login 01 <a name="login-01"></a>

A simple login page.

- **Registry Name**: `aliimam/login-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/login-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `input`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
---

### Login 02 <a name="login-02"></a>

A simple login page.

- **Registry Name**: `aliimam/login-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/login-02.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `input`
  - `field`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
##### File: `login-form.tsx`

```tsx
import { cn } from "@/registry/aliimam/lib/utils"
import { Button } from "@/registry/aliimam/ui/button"
import {
  Field,
  FieldDescription,
  FieldGroup,
  FieldLabel,
  FieldSeparator,
} from "@/registry/aliimam/ui/field"
import { Input } from "@/registry/aliimam/ui/input"

export function LoginForm({
  className,
  ...props
}: React.ComponentProps<"form">) {
  return (
    <form className={cn("flex flex-col gap-6", className)} {...props}>
      <FieldGroup>
        <div className="flex flex-col items-center gap-1 text-center">
          <h1 className="text-2xl font-bold">Login to your account</h1>
          <p className="text-muted-foreground text-sm text-balance">
            Enter your email below to login to your account
          </p>
        </div>
        <Field>
          <FieldLabel htmlFor="email">Email</FieldLabel>
          <Input id="email" type="email" placeholder="m@example.com" required />
        </Field>
        <Field>
          <div className="flex items-center">
            <FieldLabel htmlFor="password">Password</FieldLabel>
            <a
              href="#"
              className="ml-auto text-sm underline-offset-4 hover:underline"
            >
              Forgot your password?
            </a>
          </div>
          <Input id="password" type="password" required />
        </Field>
        <Field>
          <Button type="submit">Login</Button>
        </Field>
        <FieldSeparator>Or continue with</FieldSeparator>
        <Field>
          <Button variant="outline" type="button">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
              <path
                d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"
                fill="currentColor"
              />
            </svg>
            Login with GitHub
          </Button>
          <FieldDescription className="text-center">
            Don&apos;t have an account?{" "}
            <a href="#" className="underline underline-offset-4">
              Sign up
            </a>
          </FieldDescription>
        </Field>
      </FieldGroup>
    </form>
  )
}
```
---

### Logos 01 <a name="logos-01"></a>

A simple logos section.

- **Registry Name**: `aliimam/logos-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/logos-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
---

### Logos 02 <a name="logos-02"></a>

A simple logos section.

- **Registry Name**: `aliimam/logos-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/logos-02.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
---

### Logos 03 <a name="logos-03"></a>

A simple logos section.

- **Registry Name**: `aliimam/logos-03`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/logos-03.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
---

### Logos 04 <a name="logos-04"></a>

A simple logos section.

- **Registry Name**: `aliimam/logos-04`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/logos-04.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
---

### Logos 05 <a name="logos-05"></a>

A simple logos section.

- **Registry Name**: `aliimam/logos-05`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/logos-05.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
---

### Pricing 01 <a name="pricing-01"></a>

A simple pricing section.

- **Registry Name**: `aliimam/pricing-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/pricing-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `separator`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `pricing.tsx`

```tsx
import Link from "next/link"
import {
  SectionHeader,
  SectionHeaderDescription,
  SectionHeaderHeading,
} from "@/src/components/layout/page-header"
import { Check } from "lucide-react"

import { Button } from "@/registry/aliimam/ui/button"
import { Separator } from "@/registry/aliimam/ui/separator"

export function Pricing() {
  return (
    <div className="border-t">
      <section className="relative container">
        <div className="pb-20 lg:border-x">
          <SectionHeader>
            <SectionHeaderHeading>
              Pricing that Power your Design.
            </SectionHeaderHeading>
            <SectionHeaderDescription>
              Whether you’re building your brand, launching a product, or
              crafting visuals, our plans give you the tools and support to
              create stunning designs at any scale.
            </SectionHeaderDescription>
          </SectionHeader>

          <div className="flex justify-center">
            <div className="grid max-w-5xl items-center gap-6 md:grid-cols-5 md:gap-0 md:px-12">
              <div className="flex h-min flex-col justify-between space-y-8 border-y p-6 md:col-span-2 md:my-2 md:border md:border-r-0 lg:p-10">
                <div className="space-y-4">
                  <div>
                    <h2 className="text-md font-thin">Free Forever</h2>
                    <span className="my-3 block text-3xl font-semibold md:text-7xl">
                      $0
                    </span>
                    <p className="text-muted-foreground text-sm">
                      Perfect for individual designers and engineers getting
                      started with design.
                    </p>
                  </div>

                  <Button asChild size={"xl"} variant={"outline"}>
                    <Link href="/blocks">Get Started</Link>
                  </Button>

                  <Separator className="my-6" />

                  <div>
                    <h3 className="text-muted-foreground mb-2 text-xs">
                      Included
                    </h3>
                    <ul className="list-outside space-y-2 text-sm">
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Access to free all designs
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Community forum access
                      </li>
                    </ul>

                    <p className="text-muted-foreground mt-3 text-xs">
                      *Free plan features are limited but enough to get you
                      started with your first designs.
                    </p>
                  </div>
                </div>
              </div>

              <div className="bg-muted rounded-md border p-6 shadow-2xl md:col-span-3 lg:p-10">
                <div className="space-y-4">
                  <div>
                    <h2 className="text-xl font-thin">Business Plan</h2>
                    <div className="flex items-end gap-1">
                      <span className="my-3 block text-7xl font-semibold tracking-tighter">
                        $1995
                      </span>
                      <span className="mb-4 block text-xl font-thin">
                        / Month
                      </span>
                    </div>
                    <p className="text-muted-foreground text-sm">
                      Designed for brands and teams managing multiple clients
                      and high-volume projects.
                    </p>
                  </div>
                  <div className="flex gap-2">
                    <Button asChild size={"xl"}>
                      <Link
                        href="https://cal.com/aliimam/30min"
                        target="_blank"
                      >
                        Join Today
                      </Link>
                    </Button>
                    <Button asChild variant={"outline"} size={"xl"}>
                      <Link
                        href="https://cal.com/aliimam/30min"
                        target="_blank"
                      >
                        Book a Call
                      </Link>
                    </Button>
                  </div>

                  <Separator className="my-6" />

                  <div>
                    <h3 className="text-muted-foreground mb-2 text-xs font-light">
                      What’s included:
                    </h3>
                    <ul className="list-outside space-y-2 text-sm">
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />I personally
                        handle all design work
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Unlimited design revisions
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Daily updates on your requests
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        One active request at a time for better quality
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Pause or cancel anytime
                      </li>
                    </ul>

                    <p className="text-muted-foreground mt-3 text-xs">
                      *Pro plan is ideal for professionals and brands who need
                      full access to all design creatives of my work.
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  )
}
```
---

### Pricing 02 <a name="pricing-02"></a>

A simple pricing section.

- **Registry Name**: `aliimam/pricing-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/pricing-02.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `page.tsx`

```tsx
import * as React from "react"
import { Metadata } from "next"
import { notFound } from "next/navigation"
import { ThemeListener } from "@/src/components/themes/theme-listener"
import { siteConfig } from "@/src/lib/config"
import { getRegistryComponent, getRegistryItem } from "@/src/lib/registry"
import { absoluteUrl, cn } from "@/src/lib/utils"
import { registryItemSchema } from "shadcn/schema"
import { z } from "zod"

export const revalidate = false
export const dynamic = "force-static"
export const dynamicParams = false

const getCachedRegistryItem = React.cache(async (name: string) => {
  return await getRegistryItem(name)
})

export async function generateMetadata({
  params,
}: {
  params: Promise<{
    name: string
  }>
}): Promise<Metadata> {
  const { name } = await params
  const item = await getCachedRegistryItem(name)

  if (!item) {
    return {}
  }

  const title = item.name
  const description = item.description

  return {
    title: item.description,
    description,
    openGraph: {
      title,
      description,
      type: "article",
      url: absoluteUrl(`/view/${item.name}`),
      images: [
        {
          url: siteConfig.ogImage,
          width: 1200,
          height: 630,
          alt: siteConfig.name,
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      title,
      description,
      images: [siteConfig.ogImage],
      creator: "@aliimamio",
    },
  }
}

export async function generateStaticParams() {
  const { Index } = await import("@/registry/__index__")
  const index = z.record(registryItemSchema).parse(Index)

  return Object.values(index)
    .filter((block) =>
      [
        "registry:block",
        "registry:component",
        "registry:example",
        "registry:internal",
      ].includes(block.type)
    )
    .map((block) => ({
      name: block.name,
    }))
}

export default async function BlockPage({
  params,
}: {
  params: Promise<{
    name: string
  }>
}) {
  const { name } = await params
  const item = await getCachedRegistryItem(name)
  const Component = getRegistryComponent(name)

  if (!item || !Component) {
    return notFound()
  }

  return (
    <>
      <ThemeListener />
      <div
        className={cn("bg-background theme-container", item.meta?.container)}
      >
        <Component />
      </div>
    </>
  )
}
```
---

### Pricing 03 <a name="pricing-03"></a>

A simple pricing section.

- **Registry Name**: `aliimam/pricing-03`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/pricing-03.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `tabs`
  - `badge`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `pricing.tsx`

```tsx
import Link from "next/link"
import {
  SectionHeader,
  SectionHeaderDescription,
  SectionHeaderHeading,
} from "@/src/components/layout/page-header"
import { Check } from "lucide-react"

import { Button } from "@/registry/aliimam/ui/button"
import { Separator } from "@/registry/aliimam/ui/separator"

export function Pricing() {
  return (
    <div className="border-t">
      <section className="relative container">
        <div className="pb-20 lg:border-x">
          <SectionHeader>
            <SectionHeaderHeading>
              Pricing that Power your Design.
            </SectionHeaderHeading>
            <SectionHeaderDescription>
              Whether you’re building your brand, launching a product, or
              crafting visuals, our plans give you the tools and support to
              create stunning designs at any scale.
            </SectionHeaderDescription>
          </SectionHeader>

          <div className="flex justify-center">
            <div className="grid max-w-5xl items-center gap-6 md:grid-cols-5 md:gap-0 md:px-12">
              <div className="flex h-min flex-col justify-between space-y-8 border-y p-6 md:col-span-2 md:my-2 md:border md:border-r-0 lg:p-10">
                <div className="space-y-4">
                  <div>
                    <h2 className="text-md font-thin">Free Forever</h2>
                    <span className="my-3 block text-3xl font-semibold md:text-7xl">
                      $0
                    </span>
                    <p className="text-muted-foreground text-sm">
                      Perfect for individual designers and engineers getting
                      started with design.
                    </p>
                  </div>

                  <Button asChild size={"xl"} variant={"outline"}>
                    <Link href="/blocks">Get Started</Link>
                  </Button>

                  <Separator className="my-6" />

                  <div>
                    <h3 className="text-muted-foreground mb-2 text-xs">
                      Included
                    </h3>
                    <ul className="list-outside space-y-2 text-sm">
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Access to free all designs
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Community forum access
                      </li>
                    </ul>

                    <p className="text-muted-foreground mt-3 text-xs">
                      *Free plan features are limited but enough to get you
                      started with your first designs.
                    </p>
                  </div>
                </div>
              </div>

              <div className="bg-muted rounded-md border p-6 shadow-2xl md:col-span-3 lg:p-10">
                <div className="space-y-4">
                  <div>
                    <h2 className="text-xl font-thin">Business Plan</h2>
                    <div className="flex items-end gap-1">
                      <span className="my-3 block text-7xl font-semibold tracking-tighter">
                        $1995
                      </span>
                      <span className="mb-4 block text-xl font-thin">
                        / Month
                      </span>
                    </div>
                    <p className="text-muted-foreground text-sm">
                      Designed for brands and teams managing multiple clients
                      and high-volume projects.
                    </p>
                  </div>
                  <div className="flex gap-2">
                    <Button asChild size={"xl"}>
                      <Link
                        href="https://cal.com/aliimam/30min"
                        target="_blank"
                      >
                        Join Today
                      </Link>
                    </Button>
                    <Button asChild variant={"outline"} size={"xl"}>
                      <Link
                        href="https://cal.com/aliimam/30min"
                        target="_blank"
                      >
                        Book a Call
                      </Link>
                    </Button>
                  </div>

                  <Separator className="my-6" />

                  <div>
                    <h3 className="text-muted-foreground mb-2 text-xs font-light">
                      What’s included:
                    </h3>
                    <ul className="list-outside space-y-2 text-sm">
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />I personally
                        handle all design work
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Unlimited design revisions
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Daily updates on your requests
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        One active request at a time for better quality
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Pause or cancel anytime
                      </li>
                    </ul>

                    <p className="text-muted-foreground mt-3 text-xs">
                      *Pro plan is ideal for professionals and brands who need
                      full access to all design creatives of my work.
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  )
}
```
---

### Pricing 04 <a name="pricing-04"></a>

A simple pricing section.

- **Registry Name**: `aliimam/pricing-04`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/pricing-04.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`
  - `card`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `pricing.tsx`

```tsx
import Link from "next/link"
import {
  SectionHeader,
  SectionHeaderDescription,
  SectionHeaderHeading,
} from "@/src/components/layout/page-header"
import { Check } from "lucide-react"

import { Button } from "@/registry/aliimam/ui/button"
import { Separator } from "@/registry/aliimam/ui/separator"

export function Pricing() {
  return (
    <div className="border-t">
      <section className="relative container">
        <div className="pb-20 lg:border-x">
          <SectionHeader>
            <SectionHeaderHeading>
              Pricing that Power your Design.
            </SectionHeaderHeading>
            <SectionHeaderDescription>
              Whether you’re building your brand, launching a product, or
              crafting visuals, our plans give you the tools and support to
              create stunning designs at any scale.
            </SectionHeaderDescription>
          </SectionHeader>

          <div className="flex justify-center">
            <div className="grid max-w-5xl items-center gap-6 md:grid-cols-5 md:gap-0 md:px-12">
              <div className="flex h-min flex-col justify-between space-y-8 border-y p-6 md:col-span-2 md:my-2 md:border md:border-r-0 lg:p-10">
                <div className="space-y-4">
                  <div>
                    <h2 className="text-md font-thin">Free Forever</h2>
                    <span className="my-3 block text-3xl font-semibold md:text-7xl">
                      $0
                    </span>
                    <p className="text-muted-foreground text-sm">
                      Perfect for individual designers and engineers getting
                      started with design.
                    </p>
                  </div>

                  <Button asChild size={"xl"} variant={"outline"}>
                    <Link href="/blocks">Get Started</Link>
                  </Button>

                  <Separator className="my-6" />

                  <div>
                    <h3 className="text-muted-foreground mb-2 text-xs">
                      Included
                    </h3>
                    <ul className="list-outside space-y-2 text-sm">
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Access to free all designs
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Community forum access
                      </li>
                    </ul>

                    <p className="text-muted-foreground mt-3 text-xs">
                      *Free plan features are limited but enough to get you
                      started with your first designs.
                    </p>
                  </div>
                </div>
              </div>

              <div className="bg-muted rounded-md border p-6 shadow-2xl md:col-span-3 lg:p-10">
                <div className="space-y-4">
                  <div>
                    <h2 className="text-xl font-thin">Business Plan</h2>
                    <div className="flex items-end gap-1">
                      <span className="my-3 block text-7xl font-semibold tracking-tighter">
                        $1995
                      </span>
                      <span className="mb-4 block text-xl font-thin">
                        / Month
                      </span>
                    </div>
                    <p className="text-muted-foreground text-sm">
                      Designed for brands and teams managing multiple clients
                      and high-volume projects.
                    </p>
                  </div>
                  <div className="flex gap-2">
                    <Button asChild size={"xl"}>
                      <Link
                        href="https://cal.com/aliimam/30min"
                        target="_blank"
                      >
                        Join Today
                      </Link>
                    </Button>
                    <Button asChild variant={"outline"} size={"xl"}>
                      <Link
                        href="https://cal.com/aliimam/30min"
                        target="_blank"
                      >
                        Book a Call
                      </Link>
                    </Button>
                  </div>

                  <Separator className="my-6" />

                  <div>
                    <h3 className="text-muted-foreground mb-2 text-xs font-light">
                      What’s included:
                    </h3>
                    <ul className="list-outside space-y-2 text-sm">
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />I personally
                        handle all design work
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Unlimited design revisions
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Daily updates on your requests
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        One active request at a time for better quality
                      </li>
                      <li className="flex items-center gap-2">
                        <Check className="size-3 text-green-500" />
                        Pause or cancel anytime
                      </li>
                    </ul>

                    <p className="text-muted-foreground mt-3 text-xs">
                      *Pro plan is ideal for professionals and brands who need
                      full access to all design creatives of my work.
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  )
}
```
---

### Stats 01 <a name="stats-01"></a>

A simple stats section.

- **Registry Name**: `aliimam/stats-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/stats-01.json
```

#### Dependencies

- **Registry Dependencies**:
  - `counter-number`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `stats.tsx`

```tsx
"use client"

import { ArrowRight } from "@aliimam/icons"

import { Button } from "@/registry/aliimam/ui/button"

const Stats1 = () => {
  return (
    <section className="py-32">
      <div className="container">
        <div className="grid gap-8 md:grid-cols-3">
          <div className="grid gap-4 md:col-span-3 lg:grid-cols-3">
            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Streamlined design workflows
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">82%</h3>
                <p className="text-foreground/80 text-base">
                  of manual design tasks eliminated
                </p>
              </div>
            </div>

            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Design precision
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">99.9%</h3>
                <p className="text-foreground/80 text-base">
                  design consistency achieved
                </p>
              </div>
            </div>

            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Scalable design projects
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">520K+</h3>
                <p className="text-foreground/80 text-base">
                  total design assets created
                </p>
              </div>
            </div>
          </div>
        </div>
        <div className="flex flex-col justify-center p-6 py-20 text-center">
          <div>
            <h2 className="mb-4 text-3xl font-medium md:text-5xl">
              Built for creativity, loved for efficiency
            </h2>
            <p className="text-muted-foreground mb-6 text-lg">
              The design platform modern creative teams rely on
            </p>
          </div>
          <div className="flex flex-col items-center gap-4">
            <Button className="mt-4 h-14 px-10">Start designing free</Button>
            <p className="text-muted-foreground text-xs">
              10-project trial, no credit card required
            </p>
            <a className="flex items-center gap-2 text-sm text-blue-500 hover:text-blue-600">
              <span>★ ★ ★ ★ ★</span>
              <span className="text-primary text-md">Google reviews</span>
              <ArrowRight className="h-4 w-4" />
            </a>
          </div>
        </div>
      </div>
    </section>
  )
}

export default Stats1
```
---

### Stats 02 <a name="stats-02"></a>

A simple stats section.

- **Registry Name**: `aliimam/stats-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/stats-02.json
```

#### Dependencies

- **Registry Dependencies**:
  - `gauge`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `stats.tsx`

```tsx
"use client"

import { ArrowRight } from "@aliimam/icons"

import { Button } from "@/registry/aliimam/ui/button"

const Stats1 = () => {
  return (
    <section className="py-32">
      <div className="container">
        <div className="grid gap-8 md:grid-cols-3">
          <div className="grid gap-4 md:col-span-3 lg:grid-cols-3">
            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Streamlined design workflows
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">82%</h3>
                <p className="text-foreground/80 text-base">
                  of manual design tasks eliminated
                </p>
              </div>
            </div>

            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Design precision
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">99.9%</h3>
                <p className="text-foreground/80 text-base">
                  design consistency achieved
                </p>
              </div>
            </div>

            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Scalable design projects
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">520K+</h3>
                <p className="text-foreground/80 text-base">
                  total design assets created
                </p>
              </div>
            </div>
          </div>
        </div>
        <div className="flex flex-col justify-center p-6 py-20 text-center">
          <div>
            <h2 className="mb-4 text-3xl font-medium md:text-5xl">
              Built for creativity, loved for efficiency
            </h2>
            <p className="text-muted-foreground mb-6 text-lg">
              The design platform modern creative teams rely on
            </p>
          </div>
          <div className="flex flex-col items-center gap-4">
            <Button className="mt-4 h-14 px-10">Start designing free</Button>
            <p className="text-muted-foreground text-xs">
              10-project trial, no credit card required
            </p>
            <a className="flex items-center gap-2 text-sm text-blue-500 hover:text-blue-600">
              <span>★ ★ ★ ★ ★</span>
              <span className="text-primary text-md">Google reviews</span>
              <ArrowRight className="h-4 w-4" />
            </a>
          </div>
        </div>
      </div>
    </section>
  )
}

export default Stats1
```
---

### Stats 03 <a name="stats-03"></a>

A simple stats section.

- **Registry Name**: `aliimam/stats-03`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/stats-03.json
```

#### Dependencies

- **Registry Dependencies**:
  - `counter-number`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `stats.tsx`

```tsx
"use client"

import { ArrowRight } from "@aliimam/icons"

import { Button } from "@/registry/aliimam/ui/button"

const Stats1 = () => {
  return (
    <section className="py-32">
      <div className="container">
        <div className="grid gap-8 md:grid-cols-3">
          <div className="grid gap-4 md:col-span-3 lg:grid-cols-3">
            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Streamlined design workflows
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">82%</h3>
                <p className="text-foreground/80 text-base">
                  of manual design tasks eliminated
                </p>
              </div>
            </div>

            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Design precision
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">99.9%</h3>
                <p className="text-foreground/80 text-base">
                  design consistency achieved
                </p>
              </div>
            </div>

            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Scalable design projects
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">520K+</h3>
                <p className="text-foreground/80 text-base">
                  total design assets created
                </p>
              </div>
            </div>
          </div>
        </div>
        <div className="flex flex-col justify-center p-6 py-20 text-center">
          <div>
            <h2 className="mb-4 text-3xl font-medium md:text-5xl">
              Built for creativity, loved for efficiency
            </h2>
            <p className="text-muted-foreground mb-6 text-lg">
              The design platform modern creative teams rely on
            </p>
          </div>
          <div className="flex flex-col items-center gap-4">
            <Button className="mt-4 h-14 px-10">Start designing free</Button>
            <p className="text-muted-foreground text-xs">
              10-project trial, no credit card required
            </p>
            <a className="flex items-center gap-2 text-sm text-blue-500 hover:text-blue-600">
              <span>★ ★ ★ ★ ★</span>
              <span className="text-primary text-md">Google reviews</span>
              <ArrowRight className="h-4 w-4" />
            </a>
          </div>
        </div>
      </div>
    </section>
  )
}

export default Stats1
```
---

### Stats 04 <a name="stats-04"></a>

A simple stats section.

- **Registry Name**: `aliimam/stats-04`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/stats-04.json
```

#### Dependencies

- **Registry Dependencies**:
  - `counter-number`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `stats.tsx`

```tsx
"use client"

import { ArrowRight } from "@aliimam/icons"

import { Button } from "@/registry/aliimam/ui/button"

const Stats1 = () => {
  return (
    <section className="py-32">
      <div className="container">
        <div className="grid gap-8 md:grid-cols-3">
          <div className="grid gap-4 md:col-span-3 lg:grid-cols-3">
            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Streamlined design workflows
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">82%</h3>
                <p className="text-foreground/80 text-base">
                  of manual design tasks eliminated
                </p>
              </div>
            </div>

            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Design precision
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">99.9%</h3>
                <p className="text-foreground/80 text-base">
                  design consistency achieved
                </p>
              </div>
            </div>

            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Scalable design projects
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">520K+</h3>
                <p className="text-foreground/80 text-base">
                  total design assets created
                </p>
              </div>
            </div>
          </div>
        </div>
        <div className="flex flex-col justify-center p-6 py-20 text-center">
          <div>
            <h2 className="mb-4 text-3xl font-medium md:text-5xl">
              Built for creativity, loved for efficiency
            </h2>
            <p className="text-muted-foreground mb-6 text-lg">
              The design platform modern creative teams rely on
            </p>
          </div>
          <div className="flex flex-col items-center gap-4">
            <Button className="mt-4 h-14 px-10">Start designing free</Button>
            <p className="text-muted-foreground text-xs">
              10-project trial, no credit card required
            </p>
            <a className="flex items-center gap-2 text-sm text-blue-500 hover:text-blue-600">
              <span>★ ★ ★ ★ ★</span>
              <span className="text-primary text-md">Google reviews</span>
              <ArrowRight className="h-4 w-4" />
            </a>
          </div>
        </div>
      </div>
    </section>
  )
}

export default Stats1
```
---

### Stats 05 <a name="stats-05"></a>

A simple stats section.

- **Registry Name**: `aliimam/stats-05`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/stats-05.json
```

#### Dependencies

- **Registry Dependencies**:
  - `gauge`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `stats.tsx`

```tsx
"use client"

import { ArrowRight } from "@aliimam/icons"

import { Button } from "@/registry/aliimam/ui/button"

const Stats1 = () => {
  return (
    <section className="py-32">
      <div className="container">
        <div className="grid gap-8 md:grid-cols-3">
          <div className="grid gap-4 md:col-span-3 lg:grid-cols-3">
            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Streamlined design workflows
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">82%</h3>
                <p className="text-foreground/80 text-base">
                  of manual design tasks eliminated
                </p>
              </div>
            </div>

            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Design precision
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">99.9%</h3>
                <p className="text-foreground/80 text-base">
                  design consistency achieved
                </p>
              </div>
            </div>

            <div className="bg-secondary flex h-60 flex-col justify-between rounded-lg p-6">
              <div className="mb-4">
                <p className="text-muted-foreground text-sm">
                  Scalable design projects
                </p>
              </div>
              <div>
                <h3 className="text-6xl font-semibold">520K+</h3>
                <p className="text-foreground/80 text-base">
                  total design assets created
                </p>
              </div>
            </div>
          </div>
        </div>
        <div className="flex flex-col justify-center p-6 py-20 text-center">
          <div>
            <h2 className="mb-4 text-3xl font-medium md:text-5xl">
              Built for creativity, loved for efficiency
            </h2>
            <p className="text-muted-foreground mb-6 text-lg">
              The design platform modern creative teams rely on
            </p>
          </div>
          <div className="flex flex-col items-center gap-4">
            <Button className="mt-4 h-14 px-10">Start designing free</Button>
            <p className="text-muted-foreground text-xs">
              10-project trial, no credit card required
            </p>
            <a className="flex items-center gap-2 text-sm text-blue-500 hover:text-blue-600">
              <span>★ ★ ★ ★ ★</span>
              <span className="text-primary text-md">Google reviews</span>
              <ArrowRight className="h-4 w-4" />
            </a>
          </div>
        </div>
      </div>
    </section>
  )
}

export default Stats1
```
---

### Testimonials 01 <a name="testimonials-01"></a>

A simple testimonials section.

- **Registry Name**: `aliimam/testimonials-01`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/testimonials-01.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`
- **Registry Dependencies**:
  - `button`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `testimonial.tsx`

```tsx
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/registry/aliimam/ui/avatar"

export default function TestimonialDemo() {
  return (
    <section className="relative flex min-h-svh w-full flex-col items-center justify-center overflow-hidden py-10">
      <div className="mx-auto max-w-5xl">
        <div className="mx-auto max-w-2xl text-center">
          <blockquote>
            <p className="text-lg font-medium sm:text-xl md:text-3xl">
              Adopting this design system transformed the way we build products.
              It brings clarity, consistency, and a premium feel to every
              interface we ship.
            </p>

            <div className="mt-12 flex items-center justify-center gap-6">
              <Avatar className="size-12">
                <AvatarImage
                  src="/ali.jpg"
                  alt="Ali Imam"
                  height="400"
                  width="400"
                  loading="lazy"
                />
                <AvatarFallback>AI</AvatarFallback>
              </Avatar>

              <div className="space-y-1 border-l pl-6">
                <cite className="font-medium">Ali Imam</cite>
                <span className="text-muted-foreground block text-sm">
                  Creative Design Engineer
                </span>
              </div>
            </div>
          </blockquote>
        </div>
      </div>
    </section>
  )
}
```
---

### Testimonials 02 <a name="testimonials-02"></a>

A simple testimonials section.

- **Registry Name**: `aliimam/testimonials-02`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/testimonials-02.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/icons`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `testimonial.tsx`

```tsx
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/registry/aliimam/ui/avatar"

export default function TestimonialDemo() {
  return (
    <section className="relative flex min-h-svh w-full flex-col items-center justify-center overflow-hidden py-10">
      <div className="mx-auto max-w-5xl">
        <div className="mx-auto max-w-2xl text-center">
          <blockquote>
            <p className="text-lg font-medium sm:text-xl md:text-3xl">
              Adopting this design system transformed the way we build products.
              It brings clarity, consistency, and a premium feel to every
              interface we ship.
            </p>

            <div className="mt-12 flex items-center justify-center gap-6">
              <Avatar className="size-12">
                <AvatarImage
                  src="/ali.jpg"
                  alt="Ali Imam"
                  height="400"
                  width="400"
                  loading="lazy"
                />
                <AvatarFallback>AI</AvatarFallback>
              </Avatar>

              <div className="space-y-1 border-l pl-6">
                <cite className="font-medium">Ali Imam</cite>
                <span className="text-muted-foreground block text-sm">
                  Creative Design Engineer
                </span>
              </div>
            </div>
          </blockquote>
        </div>
      </div>
    </section>
  )
}
```
---

### Testimonials 03 <a name="testimonials-03"></a>

A simple testimonials section.

- **Registry Name**: `aliimam/testimonials-03`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/testimonials-03.json
```

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `testimonial.tsx`

```tsx
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/registry/aliimam/ui/avatar"

export default function TestimonialDemo() {
  return (
    <section className="relative flex min-h-svh w-full flex-col items-center justify-center overflow-hidden py-10">
      <div className="mx-auto max-w-5xl">
        <div className="mx-auto max-w-2xl text-center">
          <blockquote>
            <p className="text-lg font-medium sm:text-xl md:text-3xl">
              Adopting this design system transformed the way we build products.
              It brings clarity, consistency, and a premium feel to every
              interface we ship.
            </p>

            <div className="mt-12 flex items-center justify-center gap-6">
              <Avatar className="size-12">
                <AvatarImage
                  src="/ali.jpg"
                  alt="Ali Imam"
                  height="400"
                  width="400"
                  loading="lazy"
                />
                <AvatarFallback>AI</AvatarFallback>
              </Avatar>

              <div className="space-y-1 border-l pl-6">
                <cite className="font-medium">Ali Imam</cite>
                <span className="text-muted-foreground block text-sm">
                  Creative Design Engineer
                </span>
              </div>
            </div>
          </blockquote>
        </div>
      </div>
    </section>
  )
}
```
---

### Testimonials 04 <a name="testimonials-04"></a>

A simple testimonials section.

- **Registry Name**: `aliimam/testimonials-04`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/testimonials-04.json
```

#### Dependencies

- **NPM Packages**:
  - `@aliimam/logos`
- **Registry Dependencies**:
  - `separator`
  - `marquee`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `testimonial.tsx`

```tsx
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/registry/aliimam/ui/avatar"

export default function TestimonialDemo() {
  return (
    <section className="relative flex min-h-svh w-full flex-col items-center justify-center overflow-hidden py-10">
      <div className="mx-auto max-w-5xl">
        <div className="mx-auto max-w-2xl text-center">
          <blockquote>
            <p className="text-lg font-medium sm:text-xl md:text-3xl">
              Adopting this design system transformed the way we build products.
              It brings clarity, consistency, and a premium feel to every
              interface we ship.
            </p>

            <div className="mt-12 flex items-center justify-center gap-6">
              <Avatar className="size-12">
                <AvatarImage
                  src="/ali.jpg"
                  alt="Ali Imam"
                  height="400"
                  width="400"
                  loading="lazy"
                />
                <AvatarFallback>AI</AvatarFallback>
              </Avatar>

              <div className="space-y-1 border-l pl-6">
                <cite className="font-medium">Ali Imam</cite>
                <span className="text-muted-foreground block text-sm">
                  Creative Design Engineer
                </span>
              </div>
            </div>
          </blockquote>
        </div>
      </div>
    </section>
  )
}
```
---

### Testimonials 05 <a name="testimonials-05"></a>

A simple testimonials section.

- **Registry Name**: `aliimam/testimonials-05`
- **Type**: `registry:block`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/testimonials-05.json
```

#### Dependencies

- **Registry Dependencies**:
  - `avatar`

#### How to Use

```tsx
// See Component Source Code below for usage details
```

#### Component Source Code

##### File: `testimonial.tsx`

```tsx
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/registry/aliimam/ui/avatar"

export default function TestimonialDemo() {
  return (
    <section className="relative flex min-h-svh w-full flex-col items-center justify-center overflow-hidden py-10">
      <div className="mx-auto max-w-5xl">
        <div className="mx-auto max-w-2xl text-center">
          <blockquote>
            <p className="text-lg font-medium sm:text-xl md:text-3xl">
              Adopting this design system transformed the way we build products.
              It brings clarity, consistency, and a premium feel to every
              interface we ship.
            </p>

            <div className="mt-12 flex items-center justify-center gap-6">
              <Avatar className="size-12">
                <AvatarImage
                  src="/ali.jpg"
                  alt="Ali Imam"
                  height="400"
                  width="400"
                  loading="lazy"
                />
                <AvatarFallback>AI</AvatarFallback>
              </Avatar>

              <div className="space-y-1 border-l pl-6">
                <cite className="font-medium">Ali Imam</cite>
                <span className="text-muted-foreground block text-sm">
                  Creative Design Engineer
                </span>
              </div>
            </div>
          </blockquote>
        </div>
      </div>
    </section>
  )
}
```
---

## UI Components <a name="ui-components"></a>

### Attraction <a name="attraction"></a>

The component is a very flexible wrapper for applying Matter.js physics to React children.

- **Registry Name**: `aliimam/attraction`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/attraction.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/attraction.json
```
```tsx
import { Attraction } from "@/components/ui/attraction"
```
```tsx
<Attraction>
  <div className="h-16 w-16 bg-red-500" />
  <div className="h-16 w-16 bg-blue-500" />
  <div className="h-16 w-16 bg-green-500" />
</Attraction>
```
#### Props & API Reference

| Prop          | Type                     | Default            | Description                    |
| ------------- | ------------------------ | ------------------ | ------------------------------ |
| `gravity`     | `{x: number, y: number}` | `{x:0,y:1}`        | Physics world gravity          |
| `className`   | `string`                 | `""`               | Custom class for container     |
| `paused`      | `boolean`                | `false`            | Pause physics without clearing |
| `walls`       | `boolean`                | `true`             | Toggle world walls             |
| `friction`    | `number`                 | `0.2`              | Default friction for bodies    |
| `restitution` | `number`                 | `0.5`              | Default bounciness             |
| `stiffness`   | `number`                 | `0.2`              | Mouse drag stiffness           |
| `background`  | `string`                 | `"transparent"`    | Canvas background              |
| `width`       | `number`                 | `container width`  | Force canvas width             |
| `height`      | `number`                 | `container height` | Force canvas height            |

#### Component Source Code

##### File: `attraction.tsx`

```tsx
//@ts-nocheck
"use client"

import React, {
  forwardRef,
  useEffect,
  useImperativeHandle,
  useRef,
} from "react"
import { debounce } from "lodash"
import {
  Bodies,
  Body,
  Engine,
  Events,
  Mouse,
  MouseConstraint,
  Render,
  Runner,
  World,
} from "matter-js"

import { cn } from "@/registry/aliimam/lib/utils"

// ─── Types ────────────────────────────────────────────────────────────────────

export type AttractionRef = {
  /** Resume the runner. */
  start: () => void
  /** Pause the runner. */
  stop: () => void
  /** Tear down and re-initialise everything from scratch. */
  reset: () => void
  /** Apply an impulse (force) to every non-static body. */
  shake: (force?: number) => void
}

export type CollisionEvent = {
  /** The two DOM elements whose physics bodies collided. */
  elements: [HTMLElement, HTMLElement]
  /** Raw Matter.js collision pair. */
  pair: Matter.ICollisionPair
}

type AttractionProps = {
  children: React.ReactNode

  // ── Gravity ──────────────────────────────────────────────────────────────
  /** Gravity vector applied to all bodies. Default: `{ x: 0, y: 1 }` */
  gravity?: { x: number; y: number }
  /**
   * Multiplier applied on top of `gravity`.
   * Useful for a quick "moon mode" (`0.1`) or "heavy" feel (`2`).
   * Default: `1`
   */
  gravityScale?: number

  // ── Engine ────────────────────────────────────────────────────────────────
  /**
   * Speed multiplier for the physics simulation.
   * `1` = real-time, `0.5` = half speed, `2` = double speed.
   * Default: `1`
   */
  timeScale?: number
  /**
   * Allow bodies to enter a sleep state when they stop moving.
   * Reduces CPU usage for complex scenes.
   * Default: `false`
   */
  enableSleep?: boolean
  /**
   * Whether to start the simulation immediately on mount.
   * Set to `false` if you want to call `ref.start()` manually.
   * Default: `true`
   */
  autoStart?: boolean

  // ── Default body properties (overridable per-item via data-* attributes) ──
  /**
   * Sliding friction applied to all bodies (0 = ice, 1 = no slide).
   * Can be overridden per item with `data-friction`.
   * Default: `0.2`
   */
  friction?: number
  /**
   * Air resistance / drag applied to all bodies (0 = none, 1 = heavy).
   * Can be overridden per item with `data-friction-air`.
   * Default: `0.01`
   */
  frictionAir?: number
  /**
   * Bounciness of all bodies (0 = no bounce, 1 = perfectly elastic).
   * Can be overridden per item with `data-restitution`.
   * Default: `0.5`
   */
  restitution?: number

  // ── Interaction ───────────────────────────────────────────────────────────
  /**
   * Allow the user to drag bodies with the mouse / touch.
   * On mobile this is always disabled to avoid fighting with scroll.
   * Default: `true`
   */
  draggable?: boolean
  /**
   * Stiffness of the mouse spring when dragging (0–1).
   * Lower values feel floatier; higher values feel snappier.
   * Default: `0.2`
   */
  dragStiffness?: number

  // ── Callbacks ─────────────────────────────────────────────────────────────
  /** Called once the engine and all bodies are ready. */
  onReady?: () => void
  /** Called on every collision between two tracked elements. */
  onCollision?: (event: CollisionEvent) => void

  className?: string
}

// ─── Component ────────────────────────────────────────────────────────────────

const Attraction = forwardRef<PhysicsRef, PhysicsProps>(
  (
    {
      children,
      gravity = { x: 0, y: 1 },
      gravityScale = 1,
      timeScale = 1,
      enableSleep = false,
      autoStart = true,
      friction = 1,
      frictionAir = 0.01,
      restitution = 0.5,
      draggable = true,
      dragStiffness = 0.5,
      onReady,
      onCollision,
      className,
    },
    ref
  ) => {
    const containerRef = useRef<HTMLDivElement>(null)
    const engine = useRef(Engine.create())
    const runner = useRef<Runner | null>(null)
    const render = useRef<Render | null>(null)
    const bodies = useRef<{ el: HTMLElement; body: Matter.Body }[]>([])
    const animationFrame = useRef<number | null>(null)

    // Keep a ref to latest callbacks so the stable `init` closure can read them
    const onReadyRef = useRef(onReady)
    const onCollisionRef = useRef(onCollision)
    useEffect(() => {
      onReadyRef.current = onReady
    }, [onReady])
    useEffect(() => {
      onCollisionRef.current = onCollision
    }, [onCollision])

    // ── Helpers ──────────────────────────────────────────────────────────────

    /** Read a data-* attribute and fall back to a default value. */
    const dataNum = (
      el: HTMLElement,
      key: string,
      fallback: number
    ): number => {
      const v = (el as HTMLElement).dataset[key]
      return v !== undefined ? parseFloat(v) : fallback
    }

    /** Parse a position string like "50%" or "120" relative to a container size. */
    const parsePos = (value: string | undefined, size: number): number => {
      if (!value) return Math.random() * size
      if (value.includes("%")) return (parseFloat(value) / 100) * size
      return parseFloat(value)
    }

    // ── Init / Clear ─────────────────────────────────────────────────────────

    const init = () => {
      if (!containerRef.current) return

      const width = containerRef.current.offsetWidth
      const height = containerRef.current.offsetHeight

      // ── Engine settings ───────────────────────────────────────────────────
      engine.current.gravity.x = gravity.x * gravityScale
      engine.current.gravity.y = gravity.y * gravityScale
      engine.current.timing.timeScale = timeScale
      engine.current.enableSleeping = enableSleep

      // ── Renderer (invisible – only used for the mouse constraint hook) ─────
      render.current = Render.create({
        element: containerRef.current,
        engine: engine.current,
        options: {
          width,
          height,
          wireframes: false,
          background: "transparent",
        },
      })
      // Canvas should never block scroll / pointer events on child elements
      render.current.canvas.style.pointerEvents = "none"
      render.current.canvas.style.display = "none"

      // ── Mouse constraint ──────────────────────────────────────────────────
      const isMobile = window.innerWidth < 768
      const mouse = Mouse.create(containerRef.current)
      const mouseConstraint = MouseConstraint.create(engine.current, {
        mouse,
        constraint: {
          // Disable drag on mobile (or when `draggable` is false)
          stiffness: !draggable || isMobile ? 0 : dragStiffness,
          render: { visible: false },
        },
      })
      World.add(engine.current.world, mouseConstraint)

      // ── Walls ─────────────────────────────────────────────────────────────
      const walls = [
        Bodies.rectangle(width / 2, height + 20, width, 40, { isStatic: true }),
        Bodies.rectangle(width + 20, height / 2, 40, height, {
          isStatic: true,
        }),
        Bodies.rectangle(-20, height / 2, 40, height, { isStatic: true }),
        Bodies.rectangle(width / 2, -20, width, 40, { isStatic: true }),
      ]
      World.add(engine.current.world, walls)

      // ── Bodies ────────────────────────────────────────────────────────────
      const items = containerRef.current.querySelectorAll<HTMLElement>(
        "[data-physics-item]"
      )
      const containerRect = containerRef.current.getBoundingClientRect()

      items.forEach((el) => {
        const rect = el.getBoundingClientRect()

        const x = parsePos(el.dataset.x, containerRect.width)
        const y = parsePos(el.dataset.y, containerRect.height)
        const angle = el.dataset.angle
          ? (parseFloat(el.dataset.angle) * Math.PI) / 180
          : 0

        // Per-item property overrides via data-* attributes
        const bodyFriction = dataNum(el, "friction", friction)
        const bodyFrictionAir = dataNum(el, "frictionAir", frictionAir)
        const bodyRestitution = dataNum(el, "restitution", restitution)
        const bodyMass = el.dataset.mass
          ? parseFloat(el.dataset.mass)
          : undefined
        const isStatic = el.dataset.static === "true"

        // Shape: "circle" uses radius from the smaller dimension; default is rectangle
        const shape = el.dataset.shape ?? "rectangle"

        let body: Matter.Body

        if (shape === "circle") {
          const radius = Math.min(rect.width, rect.height) / 2
          body = Bodies.circle(x, y, radius, {
            friction: bodyFriction,
            frictionAir: bodyFrictionAir,
            restitution: bodyRestitution,
            isStatic,
            angle,
          })
        } else {
          body = Bodies.rectangle(x, y, rect.width, rect.height, {
            friction: bodyFriction,
            frictionAir: bodyFrictionAir,
            restitution: bodyRestitution,
            isStatic,
            angle,
          })
        }

        if (bodyMass !== undefined) Body.setMass(body, bodyMass)

        World.add(engine.current.world, body)
        bodies.current.push({ el, body })
      })

      // ── Collision events ──────────────────────────────────────────────────
      if (onCollisionRef.current) {
        Events.on(engine.current, "collisionStart", (event) => {
          if (!onCollisionRef.current) return
          event.pairs.forEach((pair) => {
            const a = bodies.current.find((b) => b.body === pair.bodyA)
            const b = bodies.current.find((b) => b.body === pair.bodyB)
            if (a && b) {
              onCollisionRef.current({
                elements: [a.el, b.el],
                pair,
              })
            }
          })
        })
      }

      // ── Runner + render loop ──────────────────────────────────────────────
      runner.current = Runner.create()
      if (autoStart) {
        Runner.run(runner.current, engine.current)
        Render.run(render.current)
      }

      const update = () => {
        bodies.current.forEach(({ el, body }) => {
          const { x, y } = body.position
          const rotation = body.angle * (180 / Math.PI)
          el.style.transform = `translate(${x - el.offsetWidth / 2}px, ${
            y - el.offsetHeight / 2
          }px) rotate(${rotation}deg)`
        })
        animationFrame.current = requestAnimationFrame(update)
      }
      update()

      onReadyRef.current?.()
    }

    const clear = () => {
      if (animationFrame.current) cancelAnimationFrame(animationFrame.current)
      if (runner.current) Runner.stop(runner.current)
      if (render.current) {
        Render.stop(render.current)
        render.current.canvas.remove()
      }
      Events.off(engine.current, "collisionStart")
      World.clear(engine.current.world, false)
      Engine.clear(engine.current)
      bodies.current = []
    }

    // ── Imperative handle ────────────────────────────────────────────────────

    useImperativeHandle(ref, () => ({
      start: () => {
        if (runner.current) Runner.run(runner.current, engine.current)
        if (render.current) Render.run(render.current)
      },
      stop: () => {
        if (runner.current) Runner.stop(runner.current)
      },
      reset: () => {
        clear()
        init()
      },
      shake: (force = 0.05) => {
        bodies.current.forEach(({ body }) => {
          if (body.isStatic) return
          Body.applyForce(body, body.position, {
            x: (Math.random() - 0.5) * force,
            y: (Math.random() - 0.5) * force,
          })
        })
      },
    }))

    // ── Lifecycle ────────────────────────────────────────────────────────────

    useEffect(() => {
      init()

      const handleResize = debounce(() => {
        clear()
        init()
      }, 400)

      window.addEventListener("resize", handleResize)
      return () => {
        window.removeEventListener("resize", handleResize)
        clear()
      }
      // eslint-disable-next-line react-hooks/exhaustive-deps
    }, [])

    // ── Live prop sync (no full re-init needed) ───────────────────────────────

    useEffect(() => {
      engine.current.gravity.x = gravity.x * gravityScale
      engine.current.gravity.y = gravity.y * gravityScale
    }, [gravity.x, gravity.y, gravityScale])

    useEffect(() => {
      engine.current.timing.timeScale = timeScale
    }, [timeScale])

    useEffect(() => {
      engine.current.enableSleeping = enableSleep
    }, [enableSleep])

    // ── Render ───────────────────────────────────────────────────────────────

    return (
      <div
        ref={containerRef}
        className={cn(
          "pointer-events-none absolute inset-0 select-none",
          className
        )}
      >
        {React.Children.map(children, (child) =>
          React.isValidElement(child) ? (
            <div
              data-physics-item
              className="pointer-events-auto absolute cursor-grab"
            >
              {child}
            </div>
          ) : (
            child
          )
        )}
      </div>
    )
  }
)

Attraction.displayName = "Attraction"
export { Attraction }
export type { AttractionProps }
```
---

### Bento <a name="bento"></a>

A fully responsive, Tailwind JIT-safe Bento Grid layout system with responsive props support.

- **Registry Name**: `aliimam/bento`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/bento.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/bento.json
```
```tsx
import { BentoGrid, BentoGridItem } from "@/components/ui/bento"
```
```tsx
<BentoGrid>
  <BentoGridItem>Item 1</BentoGridItem>
  <BentoGridItem>Item 2</BentoGridItem>
  <BentoGridItem>Item 3</BentoGridItem>
</BentoGrid>
```
#### Component Source Code

##### File: `bento.tsx`

```tsx
"use client"

import Link from "next/link"

import { Marquee } from "@/registry/aliimam/components/marquee"

type BentoBlock = {
  name: string
  category: string
  href: string
  light: string
  dark: string
}

function ThemeImage({
  light,
  dark,
  alt,
}: {
  light: string
  dark: string
  alt: string
}) {
  return (
    <>
      <img
        src={light}
        alt={alt}
        className={`border-muted/10 hidden h-full w-full border object-contain dark:block`}
      />
      <img
        src={dark}
        alt={alt}
        className={`border-muted/20 block h-full w-full border object-contain dark:hidden`}
      />
    </>
  )
}

export function BentoLanding({ blocks = [] }: { blocks?: BentoBlock[] }) {
  if (!blocks.length) {
    return (
      <section className="text-muted-foreground container py-24 text-center">
        No blocks found
      </section>
    )
  }

  return (
    <section className="border-b">
      <div className="container">
        <div className="bg-foreground">
          <Marquee
            pauseOnHover
            reverse
            speed="slow"
            className="[mask-image:linear-gradient(to_right,transparent,black_8%,black_92%,transparent)] py-1"
          >
            <div className="flex gap-1">
              {blocks.map((block) => (
                <Link
                  key={block.name}
                  href={block.href}
                  className="relative h-40 shrink-0 overflow-hidden md:h-60"
                >
                  <ThemeImage
                    light={block.light}
                    dark={block.dark}
                    alt={block.name}
                  />
                </Link>
              ))}
            </div>
          </Marquee>
        </div>
      </div>
    </section>
  )
}
```
---

### Book <a name="book"></a>

A 3D styled, interactive book component with multiple variants and props.

- **Registry Name**: `aliimam/book`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/book.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/book.json
```
```tsx
import { Book } from "@/components/ui/book"
```
```tsx
<Book
  title="The Great Adventure"
  author="John Doe"
  spineText="Adventure"
  bookmark
  variant="hardcover"
  animation="hover"
>
  <p>This is some content inside the book.</p>
</Book>
```
#### Props & API Reference

```typescript
export interface BookProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}
```

#### Component Source Code

##### File: `book.tsx`

```tsx
/** Auto-generated - Do not edit */
'use client';
import React from 'react';

export interface BookProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}

export const Book = React.forwardRef<SVGSVGElement, BookProps>(
  ({ size = 24, className = '', strokeWidth = 1, ...props }, ref) => (
    <svg 
      ref={ref}
      width={size}
      height={size} 
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      className={className}
      xmlns="http://www.w3.org/2000/svg"
      {...(strokeWidth !== undefined ? { strokeWidth } : {})}
      {...props}
    >
      <path d="M4 19.5v-15A2.5 2.5 0 0 1 6.5 2H19a1 1 0 0 1 1 1v18a1 1 0 0 1-1 1H6.5a1 1 0 0 1 0-5H20" />
    </svg>
  )
);
Book.displayName = "Book";
export const BookMetadata = { 
  id: "book", 
  baseId: "book", 
  variant: "default", 
  name: "Book", 
  category: "book", 
  tags: [], 
  viewBox: "0 0 24 24" 
} as const;

export default Book;
```
---

### Border Glow <a name="border-glow"></a>

Animated gradient border wrapper with glow effects and multiple presets.

- **Registry Name**: `aliimam/border-glow`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/border-glow.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/border-glow.json
```
```tsx
import { border-glowGrid, border-glowGridItem } from "@/components/ui/border-glow"
```
```tsx
<border-glowGrid>
  <border-glowGridItem>Item 1</border-glowGridItem>
  <border-glowGridItem>Item 2</border-glowGridItem>
  <border-glowGridItem>Item 3</border-glowGridItem>
</border-glowGrid>
```
```tsx
@property --glow-angle {
  syntax: '<angle>';
  initial-value: 0deg;
  inherits: false;
}

@keyframes glow-angle-rotate {
  to {
    --glow-angle: 1turn;
  }
}

.glow-border {
  border-image: var(--glow-gradient) var(--glow-spread, 1);
  animation: glow-angle-rotate var(--glow-animation-duration, 4s)
    linear infinite;
  animation-direction: var(--glow-direction, normal);
}

.glow-border[data-gradient-type='conic'] {
  --glow-gradient: conic-gradient(
    from var(--glow-angle),
    var(--glow-color-1),
    var(--glow-color-2),
    var(--glow-color-3),
    var(--glow-color-4),
    var(--glow-color-5),
    var(--glow-color-6),
    var(--glow-color-7),
    var(--glow-color-8),
    var(--glow-color-9),
    var(--glow-color-10),
    var(--glow-color-1)
  );
}

.glow-border[data-gradient-type='radial'] {
  --glow-gradient: radial-gradient(
    circle,
    var(--glow-color-1),
    var(--glow-color-5),
    var(--glow-color-9)
  );
}

.glow-border[data-gradient-type='linear'] {
  --glow-gradient: linear-gradient(
    90deg,
    var(--glow-color-1),
    var(--glow-color-2),
    var(--glow-color-3),
    var(--glow-color-4),
    var(--glow-color-5),
    var(--glow-color-6),
    var(--glow-color-7),
    var(--glow-color-8),
    var(--glow-color-9),
    var(--glow-color-10)
  );
}

@media (prefers-reduced-motion: reduce) {
  .glow-border {
    animation: none;
  }
}
```
#### Props & API Reference

| Prop                | Type                              | Default     | Description                               |
| ------------------- | --------------------------------- | ----------- | ----------------------------------------- |
| `children`          | `React.ReactNode`                 | —           | Content inside the glow wrapper           |
| `width`             | `string`                          | `"100%"`    | CSS width value                           |
| `height`            | `string`                          | `undefined` | CSS height value                          |
| `aspectRatio`       | `string`                          | `undefined` | CSS aspect-ratio value                    |
| `animationDuration` | `number`                          | `4`         | Duration of rotation animation in seconds |
| `rotationDirection` | `"normal" \| "reverse"`           | `"normal"`  | Rotation direction                        |
| `paused`            | `boolean`                         | `false`     | Pauses animation                          |
| `hoverOnly`         | `boolean`                         | `false`     | Animates only on hover                    |
| `gradientType`      | `"conic" \| "radial" \| "linear"` | `"conic"`   | Type of gradient used                     |
| `gradientColors`    | `string[]`                        | `undefined` | Custom color array (overrides preset)     |
| `colorPreset`       | Preset name                       | `"custom"`  | Built-in gradient preset                  |
| `glowIntensity`     | `number`                          | `1`         | Controls glow opacity                     |
| `borderWidth`       | `string`                          | `"1.25em"`  | Border thickness                          |
| `blurAmount`        | `string`                          | `"0.75em"`  | Blur intensity                            |
| `inset`             | `string`                          | `"-1em"`    | Position offset of glow                   |
| `glowSpread`        | `string`                          | `"1"`       | Border-image slice value                  |
| `innerGlow`         | `boolean`                         | `false`     | Makes glow render inside container        |
| `backgroundColor`   | `string`                          | `undefined` | Custom background color                   |

#### Component Source Code

##### File: `border-glow.tsx`

```tsx
"use client"

import React from "react"

import { cn } from "@/registry/aliimam/lib/utils"

export interface BorderGlowProps extends React.HTMLAttributes<HTMLDivElement> {
  children?: React.ReactNode

  width?: string
  height?: string
  aspectRatio?: string

  animationDuration?: number
  rotationDirection?: "normal" | "reverse"
  paused?: boolean
  hoverOnly?: boolean

  gradientType?: "conic" | "radial" | "linear"
  gradientColors?: string[]
  colorPreset?:
    | "nature"
    | "ocean"
    | "sunset"
    | "aurora"
    | "neon"
    | "fire"
    | "ice"
    | "royal"
    | "cyberpunk"
    | "pastel"
    | "custom"

  glowIntensity?: number
  borderWidth?: string
  blurAmount?: string
  inset?: string
  glowSpread?: string
  innerGlow?: boolean

  backgroundColor?: string
}

const colorPresets: Record<string, string[]> = {
  nature: [
    "#669900",
    "#88bb22",
    "#99cc33",
    "#aaddaa",
    "#ccee66",
    "#006699",
    "#228888",
    "#3399cc",
    "#55aacc",
    "#669900",
  ],
  ocean: [
    "#006699",
    "#1177aa",
    "#2288bb",
    "#3399cc",
    "#44aadd",
    "#55bbee",
    "#66ccff",
    "#44bbee",
    "#2299cc",
    "#006699",
  ],
  sunset: [
    "#ff6600",
    "#ff7711",
    "#ff8822",
    "#ff9900",
    "#ffaa22",
    "#ffbb44",
    "#ffcc00",
    "#ff9933",
    "#ff7722",
    "#ff6600",
  ],
  aurora: [
    "#00ff87",
    "#22ffaa",
    "#44ffcc",
    "#60efff",
    "#88ddff",
    "#bb99ff",
    "#dd77ee",
    "#ff68f0",
    "#ff55cc",
    "#00ff87",
  ],
  neon: [
    "#ff00ff",
    "#00ffff",
    "#ff00aa",
    "#00ffaa",
    "#ff0066",
    "#00ffcc",
    "#ff00ff",
  ],
  fire: [
    "#ff0000",
    "#ff3300",
    "#ff6600",
    "#ff9900",
    "#ffcc00",
    "#ff3300",
    "#ff0000",
  ],
  ice: [
    "#e0f7ff",
    "#b3ecff",
    "#80e1ff",
    "#4dd6ff",
    "#1acbff",
    "#00bfff",
    "#0099cc",
  ],
  royal: [
    "#2b1055",
    "#4b0082",
    "#6a0dad",
    "#8a2be2",
    "#9932cc",
    "#ba55d3",
    "#2b1055",
  ],
  cyberpunk: ["#ff0080", "#ff00ff", "#7928ca", "#2afadf", "#00ffcc", "#ff0080"],
  pastel: ["#ffd1dc", "#ffe4b5", "#e0bbff", "#c1f0f6", "#fef9c3", "#ffd1dc"],
  custom: [
    "#669900",
    "#99cc33",
    "#ccee66",
    "#006699",
    "#3399cc",
    "#990066",
    "#cc3399",
    "#ff6600",
    "#ff9900",
    "#ffcc00",
  ],
}

export const BorderGlow = React.forwardRef<HTMLDivElement, BorderGlowProps>(
  (
    {
      children,
      className,

      width = "100%",
      height,
      aspectRatio,

      animationDuration = 5,
      rotationDirection = "normal",
      paused = false,
      hoverOnly = false,

      gradientType = "conic",
      gradientColors,
      colorPreset = "custom",

      glowIntensity = 1,
      borderWidth = "1.5em",
      blurAmount = "1.5rem",
      inset = "-1em",
      glowSpread = "1",
      innerGlow = false,

      backgroundColor,

      style,
      ...props
    },
    ref
  ) => {
    const colors =
      gradientColors || colorPresets[colorPreset] || colorPresets.custom

    const colorVars: Record<string, string> = {}
    for (let i = 0; i < 10; i++) {
      colorVars[`--glow-color-${i + 1}`] = colors[i % colors.length]
    }

    return (
      <div
        ref={ref}
        className={cn(
          "group relative isolate overflow-hidden rounded-md",
          className
        )}
        style={
          {
            width,
            height,
            aspectRatio,
            background: backgroundColor,

            "--glow-animation-duration": `${animationDuration}s`,
            "--glow-direction": rotationDirection,
            "--glow-spread": glowSpread,

            ...colorVars,
            ...style,
          } as React.CSSProperties
        }
        {...props}
      >
        <div
          className={cn(
            "glow-border absolute -z-10 rounded-[inherit] border-solid",
            paused && "[animation-play-state:paused]",
            hoverOnly &&
              "opacity-0 transition-opacity duration-300 group-hover:opacity-100"
          )}
          style={{
            inset: innerGlow ? "0" : inset,
            borderWidth,
            filter: `blur(${blurAmount})`,
            opacity: glowIntensity,
            animationPlayState: paused ? "paused" : "running",
          }}
          data-gradient-type={gradientType}
        />

        <div className="relative z-10 h-full w-full p-4">{children}</div>
      </div>
    )
  }
)

BorderGlow.displayName = "BorderGlow"

export default BorderGlow
```
---

### Carousel <a name="carousel"></a>

A dynamic animated tile grid that reconstructs an image using animated opacity effects.

- **Registry Name**: `aliimam/carousel`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/carousel.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/carousel.json
```
```tsx
import { Carousel } from "@/components/ui/carousel"
```
```tsx
<Carousel slides={[<img src="/1.jpg" />, <img src="/2.jpg" />]} />
```
#### Props & API Reference

```typescript
type CarouselProps = {
  opts?: CarouselOptions
  plugins?: CarouselPlugin
  orientation?: "horizontal" | "vertical"
  setApi?: (api: CarouselApi) => void
}
```

```typescript
type CarouselContextProps = {
  carouselRef: ReturnType<typeof useEmblaCarousel>[0]
  api: ReturnType<typeof useEmblaCarousel>[1]
  scrollPrev: () => void
  scrollNext: () => void
  canScrollPrev: boolean
  canScrollNext: boolean
}
```

#### Component Source Code

##### File: `carousel.tsx`

```tsx
"use client"

import * as React from "react"
import useEmblaCarousel, {
  type UseEmblaCarouselType,
} from "embla-carousel-react"
import { ArrowLeft, ArrowRight } from "lucide-react"

import { cn } from "@/registry/aliimam/lib/utils"
import { Button } from "@/registry/aliimam/ui/button"

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
        "absolute size-8 rounded-full",
        orientation === "horizontal"
          ? "top-1/2 -left-12 -translate-y-1/2"
          : "-top-12 left-1/2 -translate-x-1/2 rotate-90",
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
        "absolute size-8 rounded-full",
        orientation === "horizontal"
          ? "top-1/2 -right-12 -translate-y-1/2"
          : "-bottom-12 left-1/2 -translate-x-1/2 rotate-90",
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

### Counter Number <a name="counter-number"></a>

An animated numeric display component with smooth counting, formatting, sizes, and semantic color variants.

- **Registry Name**: `aliimam/counter-number`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/counter-number.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/counter-number.json
```
```tsx
import { CounterNumber } from "@/components/ui/counter-number"
```
```tsx
<div className="flex flex-wrap items-end gap-8">
  {/* Default */}
  <CounterNumber value={12345} />

  {/* With prefix/suffix */}
  <CounterNumber value={86} suffix="%" size="xl" color="success" />

  {/* Decimals */}
  <CounterNumber value={3.14159} decimalPlaces={3} size="lg" />

  {/* Currency */}
  <CounterNumber
    value={1299.99}
    currency="USD"
    decimalPlaces={2}
    size="xl"
    color="primary"
  />

  {/* Locale + separator */}
  <CounterNumber value={1000000} locale="de-DE" separator="." size="lg" />

  {/* Mono spacing for layout stability */}
  <CounterNumber value={2025} preserveAspectRatio className="font-medium" />
</div>
```
#### Props & API Reference

```typescript
interface CounterNumberProps extends ComponentPropsWithoutRef<"span"> {
  value: number
  startValue?: number
  duration?: number // new: animation duration in ms

  decimalPlaces?: number
  prefix?: string
  suffix?: string
  separator?: string
  currency?: string
  locale?: string

  size?: SizeVariant
  color?: ColorVariant
  preserveAspectRatio?: boolean
}
```

#### Component Source Code

##### File: `counter-number.tsx`

```tsx
"use client"

import {
  useEffect,
  useRef,
  useState,
  type ComponentPropsWithoutRef,
} from "react"

import { cn } from "@/registry/aliimam/lib/utils"

type SizeVariant = "sm" | "md" | "lg" | "xl" | "2xl"
type ColorVariant =
  | "default"
  | "primary"
  | "secondary"
  | "success"
  | "warning"
  | "error"

interface CounterNumberProps extends ComponentPropsWithoutRef<"span"> {
  value: number
  startValue?: number
  duration?: number // new: animation duration in ms

  decimalPlaces?: number
  prefix?: string
  suffix?: string
  separator?: string
  currency?: string
  locale?: string

  size?: SizeVariant
  color?: ColorVariant
  preserveAspectRatio?: boolean
}

const sizeClasses: Record<SizeVariant, string> = {
  sm: "text-md",
  md: "text-xl",
  lg: "text-3xl",
  xl: "text-5xl",
  "2xl": "text-7xl",
}

const colorClasses: Record<ColorVariant, string> = {
  default: "text-foreground",
  primary: "text-blue-600 dark:text-blue-400",
  secondary: "text-gray-600 dark:text-gray-400",
  success: "text-green-600 dark:text-green-400",
  warning: "text-yellow-600 dark:text-yellow-400",
  error: "text-red-600 dark:text-red-400",
}

export function CounterNumber({
  value,
  startValue = 0,
  duration = 1000,
  decimalPlaces = 0,
  prefix = "",
  suffix = "",
  separator = ",",
  currency,
  locale = "en-US",
  size = "md",
  color = "default",
  preserveAspectRatio = false,
  className,
  ...props
}: CounterNumberProps) {
  const ref = useRef<HTMLSpanElement>(null)
  const [displayValue, setDisplayValue] = useState(startValue)

  useEffect(() => {
    let startTime: number | null = null
    const start = displayValue
    const end = value
    const diff = end - start

    const step = (timestamp: number) => {
      if (!startTime) startTime = timestamp
      const progress = Math.min((timestamp - startTime) / duration, 1)
      setDisplayValue(start + diff * progress)
      if (progress < 1) {
        requestAnimationFrame(step)
      }
    }

    requestAnimationFrame(step)
  }, [value, duration]) // animate whenever value or duration changes

  const formatNumber = (numValue: number): string => {
    let formattedValue: string

    if (currency) {
      formattedValue = new Intl.NumberFormat(locale, {
        style: "currency",
        currency: currency,
        minimumFractionDigits: decimalPlaces,
        maximumFractionDigits: decimalPlaces,
      }).format(numValue)
    } else {
      formattedValue = new Intl.NumberFormat(locale, {
        minimumFractionDigits: decimalPlaces,
        maximumFractionDigits: decimalPlaces,
      }).format(numValue)

      if (separator !== ",") {
        formattedValue = formattedValue.replace(/,/g, separator)
      }
    }

    return `${prefix}${formattedValue}${suffix}`
  }

  const combinedClassName = cn(
    "inline-block tabular-nums tracking-wider transition-all",
    sizeClasses[size],
    colorClasses[color],
    preserveAspectRatio && "font-mono",
    className
  )

  return (
    <span ref={ref} className={combinedClassName} {...props}>
      {formatNumber(displayValue)}
    </span>
  )
}
```
---

### Device <a name="device"></a>

A responsive device frame component for showcasing images or UI previews inside MacBook, iMac, iPhone, or iPad mockups.

- **Registry Name**: `aliimam/device`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/device.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/device.json
```
```tsx
import { Device } from "@/components/ui/device"
```
```tsx
{
  /* Macbook */
}
;<Device variant="macbook" src="/screenshot.png" />
{
  /* iMac */
}
;<Device variant="imac" src="/screenshot.png" />
{
  /* iPhone */
}
;<Device variant="iphone" src="/screenshot.png" />
{
  /* iPad */
}
;<Device variant="ipad" src="/screenshot.png" />
```
#### Props & API Reference

```typescript
export interface DeviceProps extends SVGProps<SVGSVGElement> {
  variant?: "macbook" | "imac" | "iphone" | "ipad"
  src?: string
}
```

#### Component Source Code

##### File: `device.tsx`

```tsx
import type { SVGProps } from "react"

export interface DeviceProps extends SVGProps<SVGSVGElement> {
  variant?: "macbook" | "imac" | "iphone" | "ipad"
  src?: string
}

export function Device({ variant = "macbook", src, ...props }: DeviceProps) {
  if (variant === "macbook") {
    return (
      <svg
        width={650}
        height={400}
        viewBox="0 0 650 400"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        {...props}
      >
        <path
          fill="#a4a5a7"
          d="M79.56,13.18h491.32c7.23,0,13.1,5.87,13.1,13.1v336.61H66.46V26.28c0-7.23,5.87-13.1,13.1-13.1Z"
        />

        <path
          fill="#222"
          d="M79.96,14.24h490.45c6.83,0,12.37,5.54,12.37,12.37v336.28H67.59V26.6c0-6.83,5.54-12.37,12.37-12.37Z"
        />

        <path
          fill="#000"
          d="M570.25,15.74H80.34c-6.12,0-11.08,4.96-11.08,11.08v336.07h512.08V26.82c0-6.12-4.96-11.08-11.08-11.08ZM575.74,345.17H74.52V27.31c0-3.31,2.68-5.99,5.99-5.99h489.24c3.31,0,5.99,2.68,5.99,5.99v317.86Z"
        />
        <rect
          fill="currentColor"
          x="74.5"
          y="21.5"
          width="501"
          height="323"
          rx="5"
          ry="5"
        />
        {src && (
          <image
            href={src}
            x="74.5"
            y="21.5"
            width="501"
            height="323"
            preserveAspectRatio="xMidYMid slice"
            clipPath="url(#roundedCornersMacbook)"
          />
        )}
        <rect
          fill="#1d1d1d"
          x="69.09"
          y="350.51"
          width="512.11"
          height="12.48"
        />

        <path
          fill="#000"
          d="M298.14,21.02h54.07v6.5c0,1.56-1.27,2.82-2.82,2.82h-48.42c-1.56,0-2.82-1.27-2.82-2.82v-6.5h0Z"
        />
        <path
          fill="#acadaf"
          d="M19.04,362.77h611.92v10.39c0,5.95-4.83,10.79-10.79,10.79H29.83c-5.95,0-10.79-4.83-10.79-10.79v-10.39h0Z"
        />

        <path
          fill="#080d4c"
          d="M325.11,25.14c-1.99.03-1.99-3.09,0-3.06,1.99-.03,1.99,3.09,0,3.06Z"
        />

        <polygon
          fill="#b9b9bb"
          points="600.06 385.39 567.29 385.39 565.84 383.95 601.82 383.95 600.06 385.39"
        />
        <polygon
          fill="#292929"
          points="598.73 386.82 568.64 386.82 567.32 385.39 600.35 385.39 598.73 386.82"
        />
        <polygon
          fill="#b9b9bb"
          points="82.64 385.39 49.87 385.39 48.43 383.95 84.41 383.95 82.64 385.39"
        />
        <polygon
          fill="#292929"
          points="81.31 386.82 51.23 386.82 49.9 385.39 82.93 385.39 81.31 386.82"
        />
        <path
          fill="#8f9091"
          d="M278.11,362.6h94.05c0,3.63-2.95,6.58-6.58,6.58h-80.89c-3.63,0-6.58-2.95-6.58-6.58h0Z"
        />

        <defs>
          <clipPath id="roundedCornersMacbook">
            <rect
              fill="#ffffff"
              x="74.5"
              y="21.5"
              width="501"
              height="323"
              rx="5"
              ry="5"
            />
          </clipPath>
        </defs>
      </svg>
    )
  }

  if (variant === "imac") {
    return (
      <svg
        width={600}
        height={500}
        viewBox="0 0 600 500"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        {...props}
      >
        <rect
          fill="url(#linear-gradient)"
          x="232.4"
          y="401.32"
          width="135.19"
          height="83.37"
        />
        <rect
          fill="#dedfe2"
          x="234.32"
          y="489.39"
          width="17.21"
          height="1.9"
          rx=".15"
          ry=".15"
        />
        <rect
          fill="#dedfe2"
          x="348.45"
          y="489.39"
          width="17.21"
          height="1.9"
          rx=".15"
          ry=".15"
        />
        <rect
          fill="#dedfe1"
          x="232.4"
          y="484.69"
          width="135.19"
          height="5.61"
        />
        <path
          fill="#eeeeef"
          d="M23.83,10.99h552.03c4.92,0,8.91,3.99,8.91,8.91v324.18H14.92V19.9c0-4.92,3.99-8.91,8.91-8.91Z"
        />
        <path
          fill="#d9d9db"
          d="M23.83,343.94h552.03c4.92,0,8.91,3.99,8.91,8.91v48.47H14.92v-48.47c0-4.92,3.99-8.91,8.91-8.91Z"
          transform="translate(599.69 745.26) rotate(180)"
        />
        <path
          fill="#231f20"
          d="M570.43,330.43H29.57c-.44,0-.79-.36-.79-.79V25.47c0-.44.36-.79.79-.79h540.87c.44,0,.79.36.79.79v304.17c0,.44-.36.79-.79.79ZM29.57,25.37c-.05,0-.1.04-.1.09v304.17c0,.05.04.1.1.1h540.87c.05,0,.09-.04.09-.1V25.47c0-.05-.04-.09-.09-.09H29.57Z"
        />
        <rect
          fill="#fff"
          x="29.12"
          y="25.02"
          width="541.76"
          height="305.06"
          rx=".44"
          ry=".44"
        />
        <circle fill="#414042" cx="300" cy="17.7" r="2.11" />
        <circle fill="#262262" cx="300" cy="17.7" r=".85" />
        <rect
          fill="currentColor"
          x="29.12"
          y="25.02"
          width="541.76"
          height="305.06"
          rx=".44"
          ry=".44"
        />
        {src && (
          <image
            href={src}
            x="29.12"
            y="25.02"
            width="543.76"
            height="305.06"
            preserveAspectRatio="xMidYMid slice"
            clipPath="url(#roundedCornersiMac)"
          />
        )}

        <defs>
          <clipPath id="roundedCornersiMac">
            <rect
              fill="#ffffff"
              x="29.12"
              y="25.02"
              width="541.76"
              height="305.06"
              rx=".44"
              ry=".44"
            />
          </clipPath>
        </defs>

        <linearGradient
          id="linear-gradient"
          x1="300"
          y1="484.69"
          x2="300"
          y2="401.32"
          gradientUnits="userSpaceOnUse"
        >
          <stop offset="0" stopColor="#a7a9ac" />
          <stop offset=".1" stopColor="#d1d3d4" />
          <stop offset=".41" stopColor="#e6e7e8" />
          <stop offset=".73" stopColor="#e6e7e8" />
          <stop offset="1" stopColor="#d1d3d4" />
        </linearGradient>
      </svg>
    )
  }

  if (variant === "iphone") {
    return (
      <svg
        width={200}
        height={400}
        viewBox="0 0 200 400"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        {...props}
      >
        <path
          fill="#303333"
          d="M194.85,127.85c0-.25-.2-.45-.44-.45-.11.04-.37.03-.68,0V36.12c0-17.91-14.29-32.42-31.91-32.42H38.17C20.55,3.7,6.26,18.21,6.26,36.12v49.16c-.3.02-.55.03-.65-.02-.24,0-.44.2-.44.45,0,0,0,17.36,0,17.36-.03.41.5.49,1.09.48v13.68c-.6,0-1.13.08-1.09.49,0,0,0,28.64,0,28.64-.03.42.5.49,1.09.48v7.98c-.6,0-1.13.08-1.09.49,0,0,0,28.64,0,28.64-.03.42.5.49,1.09.48v179.5c0,17.91,14.29,32.42,31.91,32.42h123.65c17.62,0,31.91-14.52,31.91-32.42v-189.55c.31-.02.57-.03.68.04,1.25.1.03-46.11.44-46.55ZM187.32,363.23c0,13.61-13.25,26.65-26.64,26.65H39.32c-13.39,0-26.62-13.04-26.62-26.65V36.8c0-13.61,13.22-26.69,26.62-26.69h121.36c13.39,0,26.64,13.08,26.64,26.69v326.43Z"
        />
        <path
          fill="#000000"
          d="M161.38,5.89H38.78c-16.54,0-29.95,13.5-29.95,30.15v327.79c0,16.65,13.41,30.15,29.95,30.15h122.6c16.54,0,29.95-13.5,29.95-30.15V36.05c0-16.65-13.41-30.15-29.95-30.15ZM187.32,363.65c0,13.69-12.28,26.24-25.87,26.24H38.7c-13.6,0-26-12.55-26-26.24V36.24c0-13.69,11.42-26.13,25.02-26.13h122.75c13.6,0,26.85,12.44,26.85,26.13v327.4Z"
        />

        <rect
          fill="currentColor"
          x="12.7"
          y="10.11"
          width="174.62"
          height="379.78"
          rx="26.97"
          ry="26.97"
        />

        {src && (
          <image
            href={src}
            x="12.7"
            y="10.11"
            width="174.62"
            height="379.78"
            preserveAspectRatio="xMidYMid slice"
            clipPath="url(#roundedCornersiPhone)"
          />
        )}
        <path
          fill="#000000"
          d="M119.61,32.33h-38.93c-10.48-.18-10.5-15.78,0-15.96,0,0,38.93,0,38.93,0,4.41,0,7.98,3.57,7.98,7.98,0,4.41-3.57,7.98-7.98,7.98Z"
        />
        <path
          fill="#080d4c"
          d="M118.78,27.68c-4.32.06-4.32-6.73,0-6.66,4.32-.06,4.32,6.73,0,6.66Z"
        />

        <defs>
          <clipPath id="roundedCornersiPhone">
            <rect
              fill="#ffffff"
              x="12.7"
              y="10.11"
              width="174.62"
              height="379.78"
              rx="26.97"
              ry="26.97"
            />
          </clipPath>
        </defs>
      </svg>
    )
  }

  if (variant === "ipad") {
    return (
      <svg
        width={520}
        height={400}
        viewBox="0 0 520 400"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        {...props}
      >
        <path
          fill="#aaabac"
          d="M479.04,14.14H88.14v-.59c0-.16-.13-.3-.3-.3h-16.7c-.16,0-.3.13-.3.3v.59h-3.46v-.59c0-.16-.13-.3-.3-.3h-16.7c-.16,0-.3.13-.3.3v.59h-9.13c-13.4,0-24.27,10.78-24.45,24.14h-.48c-.16,0-.3.13-.3.3v20.07c0,.16.13.3.3.3h.47v303.38c0,13.51,10.95,24.45,24.45,24.45h438.08c13.51,0,24.45-10.95,24.45-24.45V38.6c0-13.51-10.95-24.45-24.45-24.45Z"
        />

        <rect
          fill="#000"
          x="18.58"
          y="15.94"
          width="482.84"
          height="368.91"
          rx="23.29"
          ry="23.29"
        />

        <rect
          fill="currentColor"
          x="31.37"
          y="28.47"
          width="457.25"
          height="342.87"
          rx="9.61"
          ry="9.61"
        />
        {src && (
          <image
            href={src}
            x="31.37"
            y="28.47"
            width="457.25"
            height="342.87"
            preserveAspectRatio="xMidYMid slice"
            clipPath="url(#roundedCorners)"
          />
        )}
        <circle fill="#0a1054" cx="245.1" cy="22.23" r="2.44" />
        <circle fill="#333" cx="274.98" cy="22.23" r=".88" />

        <defs>
          <clipPath id="roundedCorners">
            <rect
              fill="#ffffff"
              x="31.37"
              y="28.47"
              width="457.25"
              height="342.87"
              rx="9.61"
              ry="9.61"
            />
          </clipPath>
        </defs>
      </svg>
    )
  }

  return null
}
```
---

### Dithered Swirl <a name="dithered-swirl"></a>

Animated dithering swirl canvas background.

- **Registry Name**: `aliimam/dithered-swirl`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/dithered-swirl.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/dithered-swirl.json
```
```tsx
import { DitheredSwirl } from "@/components/ui/dithered-swirl"
```
```tsx
<DitheredSwirl />
```
#### Props & API Reference

| Prop        | Type                             | Default       | Description              |
| ----------- | -------------------------------- | ------------- | ------------------------ |
| `width`     | number                           | `1000`        | Canvas width             |
| `height`    | number                           | `1000`        | Canvas height            |
| `fg`        | string                           | `#ff0000`     | Foreground color         |
| `bg`        | string                           | `transparent` | Background color         |
| `ac`        | string                           | `#00ff00`     | Accent color             |
| `pixelSize` | number                           | `2`           | Dithering pixel size     |
| `threshold` | number                           | `0.51`        | Dither threshold         |
| `spread`    | number                           | `0.5`         | Pattern spread           |
| `acMix`     | number                           | `0`           | Accent blending strength |
| `acMode`    | `"blend" \| "hard" \| "pattern"` | `"blend"`     | Accent rendering mode    |
| `speed`     | number                           | `1`           | Animation speed          |
| `twist`     | number                           | `0`           | Swirl twist intensity    |
| `scale`     | number                           | `10`          | Pattern frequency        |
| `className` | string                           | —             | Additional CSS classes   |
| `style`     | React.CSSProperties              | —             | Inline styles            |

#### Component Source Code

##### File: `dithered-swirl.tsx`

```tsx
"use client"
"use client"

import { useEffect, useRef } from "react"

type RGBA = [number, number, number, number]
type PatternType = "bayer4" | "bayer8"
type AcMode = "blend" | "hard" | "pattern"

interface DitheredSwirlProps {
  width?: number
  height?: number
  fg?: string
  bg?: string
  ac?: string
  pixelSize?: number
  threshold?: number
  spread?: number
  acMix?: number
  acMode?: AcMode
  patternType?: PatternType
  speed?: number
  intensity?: number
  scale?: number
  className?: string
  style?: React.CSSProperties
}

// Returns [r, g, b, a] — a=0 for "transparent", a=255 for hex
function parseColor(hex: string): RGBA {
  if (!hex || hex === "transparent") return [0, 0, 0, 0]
  const h = hex.replace("#", "")
  const full =
    h.length === 3
      ? h
          .split("")
          .map((c) => c + c)
          .join("")
      : h
  const n = parseInt(full, 16)
  return [(n >> 16) & 255, (n >> 8) & 255, n & 255, 255]
}

function makeBayer4() {
  const B = [
    [0, 8, 2, 10],
    [12, 4, 14, 6],
    [3, 11, 1, 9],
    [15, 7, 13, 5],
  ].map((r) => r.map((v) => v / 16))
  return (x: number, y: number) => B[y & 3][x & 3]
}

function makeBayer8() {
  const m: number[][] = Array.from({ length: 8 }, () => new Array(8))
  for (let y = 0; y < 8; y++) {
    for (let x = 0; x < 8; x++) {
      let v = 0,
        xc = x ^ y,
        yc = y
      for (let bit = 0; bit < 6; bit++) {
        v |= (yc & 1) << (5 - bit)
        bit++
        if (bit < 6) v |= (xc & 1) << (5 - bit)
        yc >>= 1
        xc >>= 1
      }
      m[y][x] = v / 64
    }
  }
  return (x: number, y: number) => m[y & 7][x & 7]
}

export function DitheredSwirl({
  width = 800,
  height = 800,
  fg = "#ff00ff",
  bg = "transparent",
  ac = "#00ffaa",
  pixelSize = 2,
  threshold = 150 / 250,
  spread = 0.5,
  acMix = 0,
  acMode = "blend",
  patternType = "bayer4",
  speed = 1,
  intensity = 0,
  scale = 2,
  className,
  style,
}: DitheredSwirlProps) {
  const canvasRef = useRef<HTMLCanvasElement>(null)
  const rafRef = useRef<number>(0)

  const propsRef = useRef({
    fg: parseColor(fg),
    bg: parseColor(bg),
    ac: parseColor(ac),
    pixelSize,
    threshold,
    spread,
    acMix,
    acMode,
    patternType,
    speed,
    intensity,
    scale,
  })

  useEffect(() => {
    propsRef.current = {
      fg: parseColor(fg),
      bg: parseColor(bg),
      ac: parseColor(ac),
      pixelSize,
      threshold,
      spread,
      acMix,
      acMode,
      patternType,
      speed,
      intensity,
      scale,
    }
  })

  useEffect(() => {
    const canvas = canvasRef.current
    if (!canvas) return
    const ctx = canvas.getContext("2d")
    if (!ctx) return

    const W = width,
      H = height
    const clamp01 = (v: number) => (v < 0 ? 0 : v > 1 ? 1 : v)
    const buf = new Float32Array(W * H)
    let t = 0

    const draw = () => {
      const p = propsRef.current
      t += 0.016 * p.speed

      const pattern = p.patternType === "bayer8" ? makeBayer8() : makeBayer4()

      const cx = W / 2,
        cy = H / 2
      const maxR = Math.sqrt(cx * cx + cy * cy)
      const twist = (p.intensity / 50) * 4

      for (let y = 0; y < H; y++) {
        for (let x = 0; x < W; x++) {
          const dx = x - cx,
            dy = y - cy
          const dist = Math.sqrt(dx * dx + dy * dy) / maxR
          const angle = Math.atan2(dy, dx) + dist * twist * Math.sin(t * 2)
          const v =
            (Math.sin(angle * p.scale + t * 3) * 0.5 + 0.5) * (1 - dist * 0.3)
          buf[y * W + x] = clamp01(v)
        }
      }

      const img = ctx.createImageData(W, H)
      const d = img.data
      const halfZone = p.acMix * 0.45
      const acLo = 0.5 - halfZone
      const acHi = 0.5 + halfZone

      for (let y = 0; y < H; y++) {
        for (let x = 0; x < W; x++) {
          const idx = y * W + x
          const gray = buf[idx]
          const qx = Math.floor(x / p.pixelSize)
          const qy = Math.floor(y / p.pixelSize)
          const pv = pattern(qx, qy)
          const dithered = gray + (pv - 0.5) * p.spread > p.threshold ? 1 : 0

          let r: number, g: number, b: number, a: number

          if (gray < 0) {
            ;[r, g, b, a] = p.ac
          } else if (p.acMix > 0.01 && gray >= acLo && gray <= acHi) {
            if (p.acMode === "hard") {
              ;[r, g, b, a] = p.ac
            } else if (p.acMode === "pattern") {
              ;[r, g, b, a] = dithered ? p.ac : p.bg
            } else {
              const dist2 = Math.abs(gray - 0.5)
              const ef = halfZone > 0.01 ? 1 - dist2 / halfZone : 1
              const bl = ef * ef
              const src = dithered ? p.fg : p.bg
              r = src[0] + (p.ac[0] - src[0]) * bl
              g = src[1] + (p.ac[1] - src[1]) * bl
              b = src[2] + (p.ac[2] - src[2]) * bl
              a = src[3] + (p.ac[3] - src[3]) * bl
            }
          } else {
            ;[r, g, b, a] = dithered ? p.fg : p.bg
          }

          const px4 = idx * 4
          d[px4] = r
          d[px4 + 1] = g
          d[px4 + 2] = b
          d[px4 + 3] = a
        }
      }

      ctx.putImageData(img, 0, 0)
      rafRef.current = requestAnimationFrame(draw)
    }

    rafRef.current = requestAnimationFrame(draw)
    return () => cancelAnimationFrame(rafRef.current)
  }, [width, height])

  return (
    <canvas
      ref={canvasRef}
      width={width}
      height={height}
      className={className}
      style={{
        width: "100%",
        height: "100%",
        display: "block",
        imageRendering: "pixelated",
        background: "transparent",
        ...style,
      }}
    />
  )
}
```
---

### Dot Pattern <a name="dot-pattern"></a>

A background utility component that renders a repeated dot pattern using SVG `<pattern>`. Useful for decorative backgrounds or UI sections.

- **Registry Name**: `aliimam/dot-pattern`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/dot-pattern.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/dot-pattern.json
```
```tsx
import { DotPattern } from "@/components/ui/dot-pattern"
```
```tsx
<DotPattern />
```
#### Props & API Reference

```typescript
interface DotPatternProps {
  width?: number
  height?: number
  x?: number
  y?: number
  cx?: number
  cy?: number
  dotSize?: number
  className?: string
}
```

#### Component Source Code

##### File: `dot-pattern.tsx`

```tsx
"use client"

import { useId } from "react"

import { cn } from "@/registry/aliimam/lib/utils"

interface DotPatternProps {
  width?: number
  height?: number
  x?: number
  y?: number
  cx?: number
  cy?: number
  dotSize?: number
  className?: string
}

function DotPattern({
  width = 20,
  height = 20,
  x = 0,
  y = 0,
  cx = 1,
  cy = 1,
  dotSize = 0.7,
  className,
  ...props
}: DotPatternProps) {
  const id = useId()

  return (
    <svg
      aria-hidden="true"
      className={cn(
        "pointer-events-none absolute inset-0 h-full w-full rounded-md p-1",
        className
      )}
      {...props}
    >
      <defs>
        <pattern
          id={id}
          width={width}
          height={height}
          patternUnits="userSpaceOnUse"
          patternContentUnits="userSpaceOnUse"
          x={x}
          y={y}
        >
          <circle
            id="pattern-circle"
            cx={cx}
            cy={cy}
            r={dotSize}
            fill="currentColor"
            opacity={0.5}
          />
        </pattern>
      </defs>
      <rect width="100%" height="100%" strokeWidth={0} fill={`url(#${id})`} />
    </svg>
  )
}

export { DotPattern }
```
---

### Gauge <a name="gauge"></a>

A flexible, animated gauge component with support for full, half, and quarter circle types, gradients, multi-rings, thresholds, tick marks, and glow effects.

- **Registry Name**: `aliimam/gauge`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/gauge.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/gauge.json
```
```tsx
import { Gauge } from "@/components/gauge"
```
```tsx
<Gauge value={63} />
```
#### Props & API Reference

```typescript
export interface GaugeProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}
```

#### Component Source Code

##### File: `gauge.tsx`

```tsx
/** Auto-generated - Do not edit */
'use client';
import React from 'react';

export interface GaugeProps extends React.SVGProps<SVGSVGElement> {
  size?: number | string;
  strokeWidth?: number;
}

export const Gauge = React.forwardRef<SVGSVGElement, GaugeProps>(
  ({ size = 24, className = '', strokeWidth = 1, ...props }, ref) => (
    <svg 
      ref={ref}
      width={size}
      height={size} 
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      className={className}
      xmlns="http://www.w3.org/2000/svg"
      {...(strokeWidth !== undefined ? { strokeWidth } : {})}
      {...props}
    >
      <path d="m12 14 4-4" />
  <path d="M3.34 19a10 10 0 1 1 17.32 0" />
    </svg>
  )
);
Gauge.displayName = "Gauge";
export const GaugeMetadata = { 
  id: "gauge", 
  baseId: "gauge", 
  variant: "default", 
  name: "Gauge", 
  category: "others", 
  tags: [], 
  viewBox: "0 0 24 24" 
} as const;

export default Gauge;
```
---

### Gradient Bars <a name="gradient-bars"></a>

A dynamic animated canvas component that renders trails following the cursor with configurable physics and color cycling.

- **Registry Name**: `aliimam/gradient-bars`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/gradient-bars.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/gradient-bars.json
```
```tsx
import { GradientBars } from "@/components/ui/gradient-bars"
```
```tsx
<GradientBars />
```
#### Props & API Reference

```typescript
interface GradientBarsProps {
  className?: string
  numBars?: number

  colors?: string[]
  orientation?: Orientation

  minScale?: number
  maxScale?: number

  animation?: AnimationVariant
  duration?: number
  delayStep?: number
  easing?: string

  ariaHidden?: boolean
}
```

#### Component Source Code

##### File: `gradient-bars.tsx`

```tsx
"use client"

import React, { CSSProperties, useMemo } from "react"

import { cn } from "@/registry/aliimam/lib/utils"

type Orientation = "vertical" | "horizontal"
type AnimationVariant = "pulse" | "wave" | "none"

interface GradientBarsProps {
  className?: string
  numBars?: number

  colors?: string[]
  orientation?: Orientation

  minScale?: number
  maxScale?: number

  animation?: AnimationVariant
  duration?: number
  delayStep?: number
  easing?: string

  ariaHidden?: boolean
}

export function GradientBars({
  className,
  numBars = 15,

  colors = ["#000fff", "transparent"],
  orientation = "vertical",

  minScale = 0.2,
  maxScale = 1,

  animation = "pulse",
  duration = 2,
  delayStep = 0.08,
  easing = "ease-in-out",

  ariaHidden = true,
}: GradientBarsProps) {
  const bars = useMemo(() => Array.from({ length: numBars }), [numBars])

  const gradient = `linear-gradient(${
    orientation === "vertical" ? "to top" : "to right"
  }, ${colors.join(", ")})`

  const getScale = (index: number) => {
    const position = index / (numBars - 1 || 1)
    const distance = Math.abs(position - 0.5)
    const curve = Math.pow(distance * 2, 2)
    return minScale + (maxScale - minScale) * curve
  }

  return (
    <div
      aria-hidden={ariaHidden}
      className={cn("absolute inset-0 overflow-hidden", className)}
    >
      {/* Inline animations */}
      <style>{`
        @keyframes gradient-bar-pulse {
          from { opacity: 0.6; }
          to { opacity: 1; }
        }

        @keyframes gradient-bar-wave {
          0% { opacity: 0.4; }
          50% { opacity: 1; }
          100% { opacity: 0.4; }
        }

        @media (prefers-reduced-motion: reduce) {
          .gradient-bar {
            animation: none !important;
            transition: none !important;
          }
        }
      `}</style>

      <div
        className={cn(
          "flex h-full w-full",
          orientation === "horizontal" && "flex-col"
        )}
      >
        {bars.map((_, index) => {
          const scale = getScale(index)

          const style: CSSProperties = {
            flex: `1 0 ${100 / numBars}%`,
            maxWidth: orientation === "vertical" ? `${100 / numBars}%` : "100%",
            maxHeight:
              orientation === "horizontal" ? `${100 / numBars}%` : "100%",
            background: gradient,
            transform:
              orientation === "vertical"
                ? `scaleY(${scale})`
                : `scaleX(${scale})`,
            transformOrigin: "bottom left",
            transition:
              animation === "none"
                ? undefined
                : `transform ${duration}s ${easing}`,
            animation:
              animation === "pulse"
                ? `gradient-bar-pulse ${duration}s ${easing} infinite alternate`
                : animation === "wave"
                  ? `gradient-bar-wave ${duration}s ${easing} infinite`
                  : undefined,
            animationDelay:
              animation !== "none" ? `${index * delayStep}s` : undefined,
          }

          return <div key={index} className="gradient-bar" style={style} />
        })}
      </div>
    </div>
  )
}
```
---

### Grid Pattern <a name="grid-pattern"></a>

A flexible SVG-based grid background utility with customizable spacing, dash patterns, and square highlights. Useful for charts, diagrams, or decorative UI sections.

- **Registry Name**: `aliimam/grid-pattern`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/grid-pattern.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/grid-pattern.json
```
```tsx
import { GridPattern } from "@/components/ui/grid-pattern"
```
```tsx
<GridPattern />
```
#### Props & API Reference

```typescript
interface GridPatternProps {
  width?: number
  height?: number
  x?: number
  y?: number
  squares?: Array<[x: number, y: number]>
  strokeDasharray?: string
  className?: string
  variant?: "default" | "small" | "large"
  [key: string]: unknown
}
```

#### Component Source Code

##### File: `grid-pattern.tsx`

```tsx
"use client"

import { useId } from "react"

import { cn } from "@/registry/aliimam/lib/utils"

interface GridPatternProps {
  width?: number
  height?: number
  x?: number
  y?: number
  squares?: Array<[x: number, y: number]>
  strokeDasharray?: string
  className?: string
  variant?: "default" | "small" | "large"
  [key: string]: unknown
}

const variantStyles = {
  default: {
    width: 30,
    height: 30,
    className: "stroke-gray-500/20",
  },
  small: {
    width: 10,
    height: 10,
    className: "stroke-gray-500/30",
  },
  large: {
    width: 60,
    height: 60,
    className: "fill-gray-500/20 stroke-gray-500/25",
  },
}

// <CHANGE> Added variant-specific path generators for different grid patterns
const getPatternPath = (variant: string, width: number, height: number) => {
  switch (variant) {
    default:
      return `M.5 ${height}V.5H${width}`
  }
}

export function GridPattern({
  width,
  height,
  x = -1,
  y = -1,
  strokeDasharray = "0",
  squares,
  className,
  variant = "default",
  ...props
}: GridPatternProps) {
  const id = useId()

  // <CHANGE> Apply variant styles with user overrides
  const variantConfig = variantStyles[variant] || variantStyles.default
  const finalWidth = width ?? variantConfig.width
  const finalHeight = height ?? variantConfig.height
  const finalClassName = cn(
    "pointer-events-none absolute inset-0 h-full w-full",
    variantConfig.className,
    className
  )

  return (
    <svg aria-hidden="true" className={finalClassName} {...props}>
      <defs>
        <pattern
          id={id}
          width={finalWidth}
          height={finalHeight}
          patternUnits="userSpaceOnUse"
          x={x}
          y={y}
        >
          <path
            d={getPatternPath(variant, finalWidth, finalHeight)}
            fill="none"
            strokeDasharray={strokeDasharray}
          />
        </pattern>
      </defs>
      <rect width="100%" height="100%" strokeWidth={0} fill={`url(#${id})`} />
      {squares && (
        <svg x={x} y={y} className="overflow-visible">
          {squares.map(([x, y]) => (
            <rect
              className="fill-primary/20"
              strokeWidth="0"
              key={`${x}-${y}`}
              width={finalWidth - 1}
              height={finalHeight - 1}
              x={x * finalWidth + 1}
              y={y * finalHeight + 1}
            />
          ))}
        </svg>
      )}
    </svg>
  )
}
```
---

### Marquee <a name="marquee"></a>

Renders a looping marquee animation with horizontal or vertical scrolling. Supports adjustable speed, direction, repeat count, and pause on hover.

- **Registry Name**: `aliimam/marquee`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/marquee.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/marquee.json
```
```tsx
import { Marquee } from "@/components/marquee"
```
```tsx
// Basic
<Marquee>
  <div>Item 1</div>
  <div>Item 2</div>
</Marquee>

// With fade and custom speed
<Marquee speed={25} fade={true} gap="20px">
  <img src="logo1.png" />
  <img src="logo2.png" />
</Marquee>

// Vertical with delay
<Marquee vertical delay={2} pauseOnHover>
  <Card>Content</Card>
</Marquee>

// Auto-fill responsive
<Marquee autoFill speed="slow" reverse>
  <Badge>Tag</Badge>
</Marquee>
```
```tsx
@import "tailwindcss";
@import "tw-animate-css";

@theme inline {
  --animate-marquee: marquee var(--duration) infinite linear;
  --animate-marquee-vertical: marquee-vertical var(--duration) linear infinite;
}


@keyframes marquee {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(calc(-100% - var(--gap)));
  }
}

@keyframes marquee-vertical {
  from {
    transform: translateY(0);
  }
  to {
    transform: translateY(calc(-100% - var(--gap)));
  }
}
```
#### Props & API Reference

```typescript
interface MarqueeProps extends ComponentPropsWithoutRef<"div"> {
  /**
   * Optional CSS class name to apply custom styles
   */
  className?: string
  /**
   * Whether to reverse the animation direction
   * @default false
   */
  reverse?: boolean
  /**
   * Whether to pause the animation on hover
   * @default false
   */
  pauseOnHover?: boolean
  /**
   * Content to be displayed in the marquee
   */
  children: React.ReactNode
  /**
   * Whether to animate vertically instead of horizontally
   * @default false
   */
  vertical?: boolean
  /**
   * Number of times to repeat the content
   * @default 5
   */
  repeat?: number
  /**
   * Animation speed variant or custom duration in seconds
   * @default "normal"
   */
  speed?: "slow" | "normal" | "fast" | number
  /**
   * Gap between repeated items (in pixels or any CSS unit)
   * @default "6px"
   */
  gap?: string | number
  /**
   * Apply fade effect at edges for smoother visual experience
   * @default true
   */
  fade?: boolean
  /**
   * Delay before animation starts (in seconds)
   * @default 0
   */
  delay?: number
  /**
   * Whether to apply auto-fill to calculate optimal repeat count
   * @default false
   */
  autoFill?: boolean
}
```

#### Component Source Code

##### File: `marquee.tsx`

```tsx
import { ComponentPropsWithoutRef } from "react"

import { cn } from "@/registry/aliimam/lib/utils"

interface MarqueeProps extends ComponentPropsWithoutRef<"div"> {
  /**
   * Optional CSS class name to apply custom styles
   */
  className?: string
  /**
   * Whether to reverse the animation direction
   * @default false
   */
  reverse?: boolean
  /**
   * Whether to pause the animation on hover
   * @default false
   */
  pauseOnHover?: boolean
  /**
   * Content to be displayed in the marquee
   */
  children: React.ReactNode
  /**
   * Whether to animate vertically instead of horizontally
   * @default false
   */
  vertical?: boolean
  /**
   * Number of times to repeat the content
   * @default 5
   */
  repeat?: number
  /**
   * Animation speed variant or custom duration in seconds
   * @default "normal"
   */
  speed?: "slow" | "normal" | "fast" | number
  /**
   * Gap between repeated items (in pixels or any CSS unit)
   * @default "6px"
   */
  gap?: string | number
  /**
   * Apply fade effect at edges for smoother visual experience
   * @default true
   */
  fade?: boolean
  /**
   * Delay before animation starts (in seconds)
   * @default 0
   */
  delay?: number
  /**
   * Whether to apply auto-fill to calculate optimal repeat count
   * @default false
   */
  autoFill?: boolean
}

export function Marquee({
  className,
  reverse = false,
  pauseOnHover = false,
  children,
  vertical = false,
  repeat = 5,
  speed = "normal",
  gap = "6px",
  fade = false,
  delay = 0,
  autoFill = false,
  ...props
}: MarqueeProps) {
  const speedVariants = {
    slow: "[--duration:120s]",
    normal: "[--duration:40s]",
    fast: "[--duration:10s]",
  }

  const gapValue = typeof gap === "number" ? `${gap}px` : gap
  const duration = typeof speed === "number" ? `${speed}s` : undefined
  const repeatCount = autoFill ? 10 : repeat

  return (
    <div
      {...props}
      className={cn(
        "group relative flex overflow-hidden p-1",
        typeof speed === "string" ? speedVariants[speed] : "",
        {
          "flex-row": !vertical,
          "flex-col": vertical,
        },
        className
      )}
      style={
        {
          "--gap": gapValue,
          ...(duration && { "--duration": duration }),
          ...(delay && { "--delay": `${delay}s` }),
        } as React.CSSProperties
      }
    >
      {fade && (
        <>
          <div
            className={cn(
              "pointer-events-none absolute z-10",
              vertical
                ? "from-background inset-x-0 top-0 h-1/6 bg-gradient-to-b"
                : "from-background inset-y-0 left-0 w-1/6 bg-gradient-to-r"
            )}
          />
          <div
            className={cn(
              "pointer-events-none absolute z-10",
              vertical
                ? "from-background inset-x-0 bottom-0 h-1/6 bg-gradient-to-t"
                : "from-background inset-y-0 right-0 w-1/6 bg-gradient-to-l"
            )}
          />
        </>
      )}
      {Array(repeatCount)
        .fill(0)
        .map((_, i) => (
          <div
            key={i}
            className={cn("flex shrink-0 justify-around [gap:var(--gap)]", {
              "animate-marquee flex-row": !vertical,
              "animate-marquee-vertical flex-col": vertical,
              "group-hover:[animation-play-state:paused]": pauseOnHover,
              "[animation-direction:reverse]": reverse,
              "[animation-delay:var(--delay)]": delay > 0,
            })}
          >
            {children}
          </div>
        ))}
    </div>
  )
}
```
---

### PixelGrid Shader <a name="pixelgrid-shader"></a>

Animated WebGL2 pixel-grid dithering shader with multiple shapes and interactive cursor ripple.

- **Registry Name**: `aliimam/pixelgrid-shader`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/pixelgrid-shader.json
```

#### Dependencies

- **NPM Packages**:
  - `three`

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/pixelgrid-shader.json
```
```tsx
import { PixelGridShader } from "@/components/ui/pixelgrid-shader"
```
```tsx
<PixelGridShader />
```
#### Props & API Reference

```typescript
export interface PixelGridShaderProps {
  shape?: Shape
  matrix?: Matrix
  pxSize?: number
  amplitude?: number
  frequency?: number
  speed?: number
  rings?: number
  colorFg?: string // HEX color
  cursorMode?: CursorMode
  cursorSize?: number
  cursorScale?: number
  className?: string
}
```

#### Component Source Code

##### File: `pixelgrid-shader.tsx`

```tsx
"use client"

import { useCallback, useEffect, useRef } from "react"

// ── Types ──────────────────────────────────────────────────────────────────
export type Shape = "wave" | "noise" | "ripple" | "swirl" | "plasma" | "scan"
export type Matrix = "random" | "bayer2" | "bayer4" | "bayer8"
export type CursorMode = "ripple" | "erase" | "burn"

interface CursorState {
  x: number
  y: number
  active: boolean
  mode: CursorMode
}

interface AppState {
  shape: Shape
  matrix: Matrix
  pxSize: number
  amplitude: number
  frequency: number
  speed: number
  rings: number
  colorFg: string
  cursorSize: number
  cursorScale: number
  cursor: CursorState
}

export interface PixelGridShaderProps {
  shape?: Shape
  matrix?: Matrix
  pxSize?: number
  amplitude?: number
  frequency?: number
  speed?: number
  rings?: number
  colorFg?: string // HEX color
  cursorMode?: CursorMode
  cursorSize?: number
  cursorScale?: number
  className?: string
}

// ── HEX → RGB ──────────────────────────────────────────────────────────────
function hexToRgb(hex: string): [number, number, number] {
  let cleaned = hex.replace("#", "")

  if (cleaned.length === 3) {
    cleaned =
      cleaned[0] +
      cleaned[0] +
      cleaned[1] +
      cleaned[1] +
      cleaned[2] +
      cleaned[2]
  }

  const bigint = parseInt(cleaned, 16)
  return [(bigint >> 16) & 255, (bigint >> 8) & 255, bigint & 255]
}

// ── Bayer matrices ─────────────────────────────────────────────────────────
const B2 = [0, 2, 3, 1]
const B4 = [0, 8, 2, 10, 12, 4, 14, 6, 3, 11, 1, 9, 15, 7, 13, 5]
const B8 = [
  0, 32, 8, 40, 2, 34, 10, 42, 48, 16, 56, 24, 50, 18, 58, 26, 12, 44, 4, 36,
  14, 46, 6, 38, 60, 28, 52, 20, 62, 30, 54, 22, 3, 35, 11, 43, 1, 33, 9, 41,
  51, 19, 59, 27, 49, 17, 57, 25, 15, 47, 7, 39, 13, 45, 5, 37, 63, 31, 55, 23,
  61, 29, 53, 21,
]

function getBayer(ix: number, iy: number, matrix: Matrix): number {
  if (matrix === "random") return Math.random()
  if (matrix === "bayer2") return B2[(iy % 2) * 2 + (ix % 2)] / 4
  if (matrix === "bayer4") return B4[(iy % 4) * 4 + (ix % 4)] / 16
  return B8[(iy % 8) * 8 + (ix % 8)] / 64
}

// ── Noise helpers ──────────────────────────────────────────────────────────
function hash(x: number, y: number): number {
  const n = Math.sin(x * 127.1 + y * 311.7) * 43758.5453
  return n - Math.floor(n)
}

function smoothNoise(x: number, y: number): number {
  const ix = Math.floor(x),
    iy = Math.floor(y)
  const fx = x - ix,
    fy = y - iy
  const ux = fx * fx * (3 - 2 * fx)
  const uy = fy * fy * (3 - 2 * fy)

  return (
    hash(ix, iy) * (1 - ux) * (1 - uy) +
    hash(ix + 1, iy) * ux * (1 - uy) +
    hash(ix, iy + 1) * (1 - ux) * uy +
    hash(ix + 1, iy + 1) * ux * uy
  )
}

function fbm(x: number, y: number): number {
  return (
    0.5 * smoothNoise(x, y) +
    0.35 * smoothNoise(2 * x, 2 * y) +
    0.15 * smoothNoise(4 * x, 4 * y)
  )
}

const smoothstep = (a: number, b: number, x: number) => {
  const t = Math.max(0, Math.min(1, (x - a) / (b - a)))
  return t * t * (3 - 2 * t)
}

const fract = (x: number) => x - Math.floor(x)

// ── Pattern evaluator ──────────────────────────────────────────────────────
function evalShape(nx: number, ny: number, t: number, s: AppState): number {
  const { shape, amplitude, frequency, rings, cursor } = s
  const PI = Math.PI
  const TWO_PI = 2 * PI
  let v = 0

  if (shape === "wave") {
    const wave =
      Math.cos(frequency * 2.5 * nx * 4 - 2 * t) *
      Math.sin(frequency * 1.5 * nx * 4 + t) *
      amplitude
    const boundary = ny * 2 + wave
    v = 1 - (Math.tanh(boundary * 4) * 0.5 + 0.5)
  } else if (shape === "noise") {
    const nuv_x = nx * 0.1 * frequency * 80
    const nuv_y = ny * 0.1 * frequency * 80
    v = fbm(nuv_x - 1 * t, nuv_y + 1 * t)
    v = 0.5 + amplitude * (v - 0.5) * 10
    v = smoothstep(0.5, 2, v)
  } else if (shape === "ripple") {
    const dist = Math.sqrt(nx * nx + ny * ny)
    const r =
      Math.sin(
        Math.pow(dist * 2, 2 + amplitude * 0.7) * rings * PI - frequency * t
      ) *
        0.5 +
      0.2
    v = r * smoothstep(0.5, 2, dist)
  } else if (shape === "swirl") {
    const l = Math.sqrt(nx * nx + ny * ny)
    const angle = rings * Math.atan2(ny, nx) + frequency * 1 * t
    const twist = Math.max(amplitude * 1, 0.5)
    const offset = Math.pow(Math.max(l, 1e-6), -twist) + angle / TWO_PI
    v = fract(offset) * smoothstep(1.5, 0.1, l)
  } else if (shape === "plasma") {
    const x = nx * frequency * 50
    const y = ny * frequency * 50
    v =
      (Math.sin(x + t) +
        Math.sin(y + t * 0.07) +
        Math.sin((x + y) * 0.5 + t * 0.5) +
        Math.sin(Math.sqrt(x * x + y * y + 1) * 1.5 + t * 0.9)) *
        1 +
      0.05
    v = 0.5 + (v - 6) * amplitude
  } else if (shape === "scan") {
    const scanline = Math.sin(ny * rings * PI * 3 - frequency * t) * 0.5 + 0.5
    const fade = 1 - Math.abs(nx) * 0.5
    v = scanline * Math.max(0, fade) * amplitude
  }

  if (cursor.active) {
    const dx = nx - cursor.x
    const dy = ny - cursor.y
    const dist = Math.sqrt(dx * dx + dy * dy)
    const scaledDist = dist / s.cursorSize

    const ripple =
      Math.sin(scaledDist * 30 * s.cursorScale - t * 6 * s.cursorScale) * 0.5 +
      0.5

    const falloff = smoothstep(1.0, 0.0, scaledDist)

    if (cursor.mode === "ripple") v = Math.min(1, v + ripple * falloff * 0.8)
    else if (cursor.mode === "erase") v *= 1 - falloff * 0.95
    else if (cursor.mode === "burn") v = Math.min(1, v + falloff * 0.9)
  }

  return Math.max(0, Math.min(1, v))
}

// ── Component ──────────────────────────────────────────────────────────────
export function PixelGridShader({
  shape = "wave",
  matrix = "bayer8",
  pxSize = 2,
  amplitude = 0.15,
  frequency = 0.7,
  speed = 0.3,
  rings = 3,
  colorFg = "#0000ff",
  cursorMode = "burn",
  cursorSize = 0.3,
  cursorScale = 0.2,
  className,
}: PixelGridShaderProps) {
  const containerRef = useRef<HTMLDivElement>(null)
  const canvasRef = useRef<HTMLCanvasElement>(null)

  const stateRef = useRef<AppState>({
    shape,
    matrix,
    pxSize,
    amplitude,
    frequency,
    speed,
    rings,
    colorFg,
    cursorSize,
    cursorScale,
    cursor: { x: -1, y: -1, active: false, mode: cursorMode },
  })

  const rafRef = useRef<number>(0)
  const startTimeRef = useRef(Date.now())

  useEffect(() => {
    stateRef.current = {
      ...stateRef.current,
      shape,
      matrix,
      pxSize,
      amplitude,
      frequency,
      speed,
      rings,
      colorFg,
      cursorSize,
      cursorScale,
      cursor: { ...stateRef.current.cursor, mode: cursorMode },
    }
  }, [
    shape,
    matrix,
    pxSize,
    amplitude,
    frequency,
    speed,
    rings,
    colorFg,
    cursorMode,
    cursorSize,
    cursorScale,
  ])

  useEffect(() => {
    const canvas = canvasRef.current!
    const container = containerRef.current!
    const ctx = canvas.getContext("2d")!

    let dpr = 1

    const resize = () => {
      dpr = window.devicePixelRatio || 1
      const W = container.clientWidth
      const H = container.clientHeight
      canvas.width = Math.round(W * dpr)
      canvas.height = Math.round(H * dpr)
      canvas.style.width = W + "px"
      canvas.style.height = H + "px"
    }

    resize()
    const ro = new ResizeObserver(resize)
    ro.observe(container)

    const render = () => {
      const s = stateRef.current
      const elapsed = (Date.now() - startTimeRef.current) * 0.001 * s.speed

      const ps = Math.max(1, Math.floor(s.pxSize * dpr))
      const cols = Math.ceil(canvas.width / ps)
      const rows = Math.ceil(canvas.height / ps)

      const [r, g, b] = hexToRgb(s.colorFg)

      ctx.clearRect(0, 0, canvas.width, canvas.height)
      ctx.fillStyle = `rgb(${r}, ${g}, ${b})`

      const aspect = canvas.width / canvas.height

      for (let row = 0; row < rows; row++) {
        for (let col = 0; col < cols; col++) {
          let nx = col / cols - 0.5
          let ny = row / rows - 0.5

          if (aspect > 1) nx *= aspect
          else ny /= aspect

          const signal = evalShape(nx, ny, elapsed, s)
          const threshold = getBayer(col, row, s.matrix)

          if (signal > threshold) {
            ctx.fillRect(col * ps, row * ps, ps, ps)
          }
        }
      }

      rafRef.current = requestAnimationFrame(render)
    }

    rafRef.current = requestAnimationFrame(render)

    return () => {
      cancelAnimationFrame(rafRef.current)
      ro.disconnect()
    }
  }, [])

  const updateCursor = (x: number, y: number) => {
    const canvas = canvasRef.current!
    const aspect = canvas.width / canvas.height

    let cx = x
    let cy = y

    if (aspect > 1) cx *= aspect
    else cy /= aspect

    stateRef.current.cursor.x = cx
    stateRef.current.cursor.y = cy
    stateRef.current.cursor.active = true
  }

  const handleMouseMove = useCallback(
    (e: React.MouseEvent<HTMLCanvasElement>) => {
      const r = e.currentTarget.getBoundingClientRect()
      updateCursor(
        (e.clientX - r.left) / r.width - 0.5,
        (e.clientY - r.top) / r.height - 0.5
      )
    },
    []
  )

  return (
    <div
      ref={containerRef}
      className={className}
      style={{ position: "absolute", inset: 0, overflow: "hidden" }}
    >
      <canvas
        ref={canvasRef}
        style={{
          display: "block",
          background: "transparent",
          width: "100%",
          height: "100%",
        }}
        onMouseMove={handleMouseMove}
        onMouseLeave={() => (stateRef.current.cursor.active = false)}
      />
    </div>
  )
}
```
---

### Render Canvas <a name="render-canvas"></a>

A dynamic animated canvas component that renders trails following the cursor with configurable physics and color cycling.

- **Registry Name**: `aliimam/render-canvas`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/render-canvas.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/render-canvas.json
```
```tsx
import { RenderCanvas } from "@/components/ui/render-canvas"
```
```tsx
export default function Page() {
  return (
    <div>
      <RenderCanvas />
    </div>
  )
}
```
#### Props & API Reference

| Prop                  | Type    | Default  | Description                                             |
| --------------------- | ------- | -------- | ------------------------------------------------------- |
| `trails`              | number  | `80`     | Number of trail lines rendered.                         |
| `size`                | number  | `50`     | Number of nodes per trail line.                         |
| `friction`            | number  | `0.5`    | Friction applied to line movement.                      |
| `dampening`           | number  | `0.025`  | Dampening factor for velocity transfer.                 |
| `tension`             | number  | `0.99`   | Spring tension applied along line nodes.                |
| `lineWidth`           | number  | `10`     | Width of the line strokes.                              |
| `colorHue`            | number  | `285`    | Base hue for HSLA stroke color.                         |
| `colorSaturation`     | number  | `100`    | Saturation of HSLA stroke color.                        |
| `colorLightness`      | number  | `50`     | Lightness of HSLA stroke color.                         |
| `opacity`             | number  | `0.025`  | Opacity of strokes.                                     |
| `enableColorCycle`    | boolean | `true`   | Enable dynamic color cycling for trails.                |
| `colorCycleSpeed`     | number  | `0.0015` | Speed of hue wave for color cycling.                    |
| `colorCycleAmplitude` | number  | `85`     | Amplitude of hue wave for color cycling.                |
| `width`               | number  | `700`    | Canvas width in pixels.                                 |
| `height`              | number  | `650`    | Canvas height in pixels.                                |
| `className`           | string  | `""`     | Additional Tailwind/CSS classes applied to wrapper div. |

#### Component Source Code

##### File: `render-canvas.tsx`

```tsx
/* eslint-disable @typescript-eslint/no-explicit-any */
/* eslint-disable no-var */
/* eslint-disable @typescript-eslint/no-unused-expressions */
/* eslint-disable @typescript-eslint/ban-ts-comment */
// @ts-nocheck

"use client"

import { useEffect, useRef } from "react"

import { cn } from "@/registry/aliimam/lib/utils"

interface RenderCanvasProps {
  // Animation configuration
  trails?: number
  size?: number
  friction?: number
  dampening?: number
  tension?: number

  // Visual properties
  lineWidth?: number
  colorHue?: number
  colorSaturation?: number
  colorLightness?: number
  opacity?: number

  // Wave animation for color cycling
  enableColorCycle?: boolean
  colorCycleSpeed?: number
  colorCycleAmplitude?: number

  // Canvas dimensions
  width?: number
  height?: number

  // Styling
  className?: string
}

export function RenderCanvas({
  trails = 80,
  size = 50,
  friction = 0.5,
  dampening = 0.025,
  tension = 0.99,
  lineWidth = 10,
  colorHue = 285,
  colorSaturation = 100,
  colorLightness = 50,
  opacity = 0.025,
  enableColorCycle = true,
  colorCycleSpeed = 0.0015,
  colorCycleAmplitude = 85,
  width = 1400,
  height = 900,
  className = "",
}: RenderCanvasProps) {
  const canvasRef = useRef<HTMLCanvasElement>(null)
  const animationRef = useRef<{
    ctx:
      | (CanvasRenderingContext2D & { running?: boolean; frame?: number })
      | null
    cleanup: () => void
  }>({ ctx: null, cleanup: () => {} })

  useEffect(() => {
    const canvas = canvasRef.current
    if (!canvas) return

    const ctx = canvas.getContext("2d") as CanvasRenderingContext2D & {
      running?: boolean
      frame?: number
    }
    if (!ctx) return

    ctx.running = true
    ctx.frame = 1
    animationRef.current.ctx = ctx

    // Wave function for color cycling
    function WaveFunction(config: any) {
      this.phase = config.phase || 0
      this.offset = config.offset || 0
      this.frequency = config.frequency || 0.001
      this.amplitude = config.amplitude || 1
    }

    WaveFunction.prototype.update = function () {
      this.phase += this.frequency
      return this.offset + Math.sin(this.phase) * this.amplitude
    }

    // Node class for trail points
    function Node() {
      this.x = 0
      this.y = 0
      this.vy = 0
      this.vx = 0
    }

    // Line class for trail effects
    function Line(config: any) {
      this.spring = config.spring + 0.1 * Math.random() - 0.05
      this.friction = friction + 0.01 * Math.random() - 0.005
      this.nodes = []
      for (let i = 0; i < size; i++) {
        const node = new Node()
        node.x = pos.x
        node.y = pos.y
        this.nodes.push(node)
      }
    }

    Line.prototype.update = function () {
      let spring = this.spring
      let node = this.nodes[0]
      node.vx += (pos.x - node.x) * spring
      node.vy += (pos.y - node.y) * spring

      for (let i = 0; i < this.nodes.length; i++) {
        node = this.nodes[i]
        if (i > 0) {
          const prevNode = this.nodes[i - 1]
          node.vx += (prevNode.x - node.x) * spring
          node.vy += (prevNode.y - node.y) * spring
          node.vx += prevNode.vx * dampening
          node.vy += prevNode.vy * dampening
        }
        node.vx *= this.friction
        node.vy *= this.friction
        node.x += node.vx
        node.y += node.vy
        spring *= tension
      }
    }

    Line.prototype.draw = function () {
      if (this.nodes.length < 2) return

      let x = this.nodes[0].x
      let y = this.nodes[0].y
      ctx.beginPath()
      ctx.moveTo(x, y)

      for (let i = 1; i < this.nodes.length - 2; i++) {
        const node = this.nodes[i]
        const nextNode = this.nodes[i + 1]
        x = 0.5 * (node.x + nextNode.x)
        y = 0.5 * (node.y + nextNode.y)
        ctx.quadraticCurveTo(node.x, node.y, x, y)
      }

      if (this.nodes.length >= 2) {
        const secondLast = this.nodes[this.nodes.length - 2]
        const last = this.nodes[this.nodes.length - 1]
        ctx.quadraticCurveTo(secondLast.x, secondLast.y, last.x, last.y)
      }

      ctx.stroke()
      ctx.closePath()
    }

    // Initialize variables
    let colorWave: any = null
    const pos = { x: canvas.width / 2, y: canvas.height / 2 } // ✅ start centered
    let lines: any[] = []

    if (enableColorCycle) {
      colorWave = new WaveFunction({
        phase: Math.random() * 2 * Math.PI,
        amplitude: colorCycleAmplitude,
        frequency: colorCycleSpeed,
        offset: colorHue,
      })
    }

    function createLines() {
      lines = []
      for (let i = 0; i < trails; i++) {
        lines.push(new Line({ spring: 0.45 + (i / trails) * 0.025 }))
      }
    }

    // ✅ Fix cursor offset using bounding rect
    function handleMouseMove(e: MouseEvent | TouchEvent) {
      const rect = canvas.getBoundingClientRect()
      if ("touches" in e && e.touches) {
        pos.x = e.touches[0].clientX - rect.left
        pos.y = e.touches[0].clientY - rect.top
      } else {
        const mouseEvent = e as MouseEvent
        pos.x = mouseEvent.clientX - rect.left
        pos.y = mouseEvent.clientY - rect.top
      }
      e.preventDefault()
    }

    function handleTouchStart(e: TouchEvent) {
      if (e.touches.length === 1) {
        const rect = canvas.getBoundingClientRect()
        pos.x = e.touches[0].clientX - rect.left
        pos.y = e.touches[0].clientY - rect.top
      }
    }

    function render() {
      if (!ctx.running) return

      ctx.globalCompositeOperation = "source-over"
      ctx.clearRect(0, 0, ctx.canvas.width, ctx.canvas.height)
      ctx.globalCompositeOperation = "lighter"

      if (enableColorCycle && colorWave) {
        ctx.strokeStyle = `hsla(${Math.round(colorWave.update())},${colorSaturation}%,${colorLightness}%,${opacity})`
      } else {
        ctx.strokeStyle = `hsla(${colorHue},${colorSaturation}%,${colorLightness}%,${opacity})`
      }

      ctx.lineWidth = lineWidth

      for (let i = 0; i < lines.length; i++) {
        const line = lines[i]
        line.update()
        line.draw()
      }

      ctx.frame++
      requestAnimationFrame(render)
    }

    function resizeCanvas() {
      const rect = canvas.getBoundingClientRect()
      canvas.width = width || rect.width
      canvas.height = height || rect.height
    }

    function initializeAnimation(e: MouseEvent | TouchEvent) {
      document.removeEventListener("mousemove", initializeAnimation)
      document.removeEventListener("touchstart", initializeAnimation)

      document.addEventListener("mousemove", handleMouseMove)
      document.addEventListener("touchmove", handleMouseMove)
      document.addEventListener("touchstart", handleTouchStart)

      handleMouseMove(e)
      createLines()
      render()
    }

    // Set up event listeners
    document.addEventListener("mousemove", initializeAnimation)
    document.addEventListener("touchstart", initializeAnimation)
    window.addEventListener("resize", resizeCanvas)

    const handleFocus = () => {
      if (!ctx.running) {
        ctx.running = true
        render()
      }
    }

    const handleBlur = () => {
      ctx.running = false
    }

    window.addEventListener("focus", handleFocus)
    window.addEventListener("blur", handleBlur)

    resizeCanvas()

    // Cleanup function
    animationRef.current.cleanup = () => {
      ctx.running = false
      document.removeEventListener("mousemove", initializeAnimation)
      document.removeEventListener("touchstart", initializeAnimation)
      document.removeEventListener("mousemove", handleMouseMove)
      document.removeEventListener("touchmove", handleMouseMove)
      document.removeEventListener("touchstart", handleTouchStart)
      window.removeEventListener("resize", resizeCanvas)
      window.removeEventListener("focus", handleFocus)
      window.removeEventListener("blur", handleBlur)
    }

    return () => {
      animationRef.current.cleanup()
    }
  }, [
    trails,
    size,
    friction,
    dampening,
    tension,
    lineWidth,
    colorHue,
    colorSaturation,
    colorLightness,
    opacity,
    enableColorCycle,
    colorCycleSpeed,
    colorCycleAmplitude,
    width,
    height,
  ])

  return (
    <div
      className={cn(
        "relative flex flex-col items-center justify-center overflow-hidden",
        className
      )}
      style={{ height, width }}
    >
      <canvas
        ref={canvasRef}
        id="canvas"
        className="absolute inset-0 h-full w-full cursor-default"
      />
    </div>
  )
}
```
---

### Ripple Shader <a name="ripple-shader"></a>

Animated interactive ripple grid shader using Three.js and GLSL.

- **Registry Name**: `aliimam/ripple-shader`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/ripple-shader.json
```

#### Dependencies

- **NPM Packages**:
  - `three`

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/ripple-shader.json
```
```tsx
import { RippleShader } from "@/components/ui/ripple-shader"
```
```tsx
<RippleShader />
```
#### Props & API Reference

```typescript
interface RippleShaderProps {
  /** Size of each pixel block. Default: 4 */
  pixelSize?: number
  /** Animation speed multiplier. Default: 1 */
  speed?: number
  /** Ripple wave frequency. Default: 5 */
  waveFrequency?: number
  /** Threshold for pixel on/off (0–1). Default: 0.5 */
  threshold?: number
  /** Amount of random sparkle/grain (0–1). Default: 0.3 */
  grainAmount?: number
  /** Opacity of grid lines (0 = none, 1 = full). Default: 0.5 */
  gridOpacity?: number
  /** Radius of cursor influence (normalized). Default: 0.25 */
  cursorRadius?: number
  /** How far pixels get pushed away from cursor. Default: 0.08 */
  cursorPush?: number
  /** CSS color override (any valid CSS color string) */
  color?: string
  /** CSS class for container */
  className?: string
}
```

#### Component Source Code

##### File: `ripple-shader.tsx`

```tsx
"use client"

import { useEffect, useRef } from "react"
import * as THREE from "three"

interface RippleShaderProps {
  /** Size of each pixel block. Default: 4 */
  pixelSize?: number
  /** Animation speed multiplier. Default: 1 */
  speed?: number
  /** Ripple wave frequency. Default: 5 */
  waveFrequency?: number
  /** Threshold for pixel on/off (0–1). Default: 0.5 */
  threshold?: number
  /** Amount of random sparkle/grain (0–1). Default: 0.3 */
  grainAmount?: number
  /** Opacity of grid lines (0 = none, 1 = full). Default: 0.5 */
  gridOpacity?: number
  /** Radius of cursor influence (normalized). Default: 0.25 */
  cursorRadius?: number
  /** How far pixels get pushed away from cursor. Default: 0.08 */
  cursorPush?: number
  /** CSS color override (any valid CSS color string) */
  color?: string
  /** CSS class for container */
  className?: string
}

export function RippleShader({
  pixelSize = 2,
  speed = 0.5,
  waveFrequency = 5,
  threshold = 0.8,
  grainAmount = 0.04,
  gridOpacity = 0.2,
  cursorRadius = 0.5,
  cursorPush = 0.5,
  color = "#00ff00",
  className = "w-full absolute h-full inset-0",
}: RippleShaderProps) {
  const containerRef = useRef<HTMLDivElement>(null)
  const sceneRef = useRef<{
    renderer: THREE.WebGLRenderer
    uniforms: any
    animationId: number
  } | null>(null)

  useEffect(() => {
    if (!containerRef.current) return
    const container = containerRef.current

    const parseCSSColor = (varName: string): [number, number, number] => {
      const raw = getComputedStyle(document.documentElement)
        .getPropertyValue(varName)
        .trim()
      const hslMatch = raw.match(/^(\d+)\s+([\d.]+)%\s+([\d.]+)%$/)
      if (hslMatch) {
        const h = parseFloat(hslMatch[1]) / 360
        const s = parseFloat(hslMatch[2]) / 100
        const l = parseFloat(hslMatch[3]) / 100
        const q = l < 0.5 ? l * (1 + s) : l + s - l * s
        const p = 2 * l - q
        const hue2rgb = (t: number) => {
          if (t < 0) t += 1
          if (t > 1) t -= 1
          if (t < 1 / 6) return p + (q - p) * 6 * t
          if (t < 1 / 2) return q
          if (t < 2 / 3) return p + (q - p) * (2 / 3 - t) * 6
          return p
        }
        return [hue2rgb(h + 1 / 3), hue2rgb(h), hue2rgb(h - 1 / 3)]
      }
      const hexMatch = raw.match(/^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i)
      if (hexMatch)
        return [
          parseInt(hexMatch[1], 16) / 255,
          parseInt(hexMatch[2], 16) / 255,
          parseInt(hexMatch[3], 16) / 255,
        ]
      const nums = raw.match(/[\d.]+/g)
      if (nums && nums.length >= 3) {
        const scale = parseFloat(nums[0]) > 1 ? 255 : 1
        return [
          parseFloat(nums[0]) / scale,
          parseFloat(nums[1]) / scale,
          parseFloat(nums[2]) / scale,
        ]
      }
      return [0.23, 0.51, 0.96]
    }

    const parseCSSColorFromString = (
      input: string
    ): [number, number, number] => {
      const temp = document.createElement("div")
      temp.style.color = input
      document.body.appendChild(temp)
      const computed = getComputedStyle(temp).color
      document.body.removeChild(temp)
      const nums = computed.match(/[\d.]+/g)
      if (nums && nums.length >= 3)
        return [
          parseFloat(nums[0]) / 255,
          parseFloat(nums[1]) / 255,
          parseFloat(nums[2]) / 255,
        ]
      return [0.23, 0.51, 0.96]
    }

    const primary = color
      ? parseCSSColorFromString(color)
      : parseCSSColor("--primary")

    const vertexShader = `void main() { gl_Position = vec4(position, 1.0); }`

    const fragmentShader = `
      precision highp float;
      uniform vec2  resolution;
      uniform float time;
      uniform float uPixelSize;
      uniform float uSpeed;
      uniform float uWaveFrequency;
      uniform float uThreshold;
      uniform float uGrainAmount;
      uniform float uGridOpacity;
      uniform vec3  uPrimary;
      uniform vec2  uMouse;       // 0..1, y-flipped
      uniform float uMouseActive;
      uniform float uCursorRadius;
      uniform float uCursorPush;

      float hash(vec2 p) {
        return fract(sin(dot(p, vec2(127.1, 311.7))) * 43758.5453123);
      }

      void main(void) {
        // ── pixel cell center ──────────────────────────────────────
        vec2 pixelCoord = floor(gl_FragCoord.xy / uPixelSize);
        vec2 cellCenter = (pixelCoord + 0.5) * uPixelSize;

        // ── mouse in pixel space ───────────────────────────────────
        vec2 mousePixel = uMouse * resolution;

        // ── displacement: push cell center away from cursor ────────
        vec2  delta     = cellCenter - mousePixel;
        float rawDist   = length(delta);
        // use pixel-space radius: cursorRadius is fraction of min(res)
        float radiusPx  = uCursorRadius * min(resolution.x, resolution.y);
        float proximity = 1.0 - smoothstep(0.0, radiusPx, rawDist);
        // push direction + magnitude (cells closest = pushed most)
        vec2  pushDir   = rawDist > 0.001 ? normalize(delta) : vec2(1.0, 0.0);
        float pushAmt   = proximity * uCursorPush * min(resolution.x, resolution.y) * uMouseActive;
        vec2  displaced = cellCenter + pushDir * pushAmt;

        // ── remap displaced position back to UV ────────────────────
        vec2 pixelUV = (displaced / uPixelSize) / floor(resolution / uPixelSize);
        vec2 uv      = pixelUV * 2.0 - 1.0;
        uv.x        *= resolution.x / resolution.y;

        float t = time * 0.3 * uSpeed;

        // ── base waves (same as before) ────────────────────────────
        float dist     = length(uv);
        float wave     = sin(dist * uWaveFrequency - t * 2.5) * 0.5 + 0.5;
        float diagWave = sin((uv.x + uv.y) * 5.0 - t * 1.8) * 0.5 + 0.5;
        float n        = hash(pixelCoord + floor(t * 3.0));

        float brightness = wave * (1.0 - uGrainAmount - 0.3)
                         + diagWave * 0.3
                         + n * uGrainAmount;

        // ── cursor: lower threshold near cursor = more pixels ON ───
        float thr  = uThreshold + 0.1 * sin(t * 0.7 + dist * 2.0)
                   - proximity * 0.3 * uMouseActive;
        float cell = step(thr, brightness);

        // ── color ──────────────────────────────────────────────────
        float colorT    = wave * 0.7 + diagWave * 0.3;
        vec3  baseColor = mix(uPrimary * 0.4, uPrimary, colorT);
        // glow: boosted brightness + slight white mix near cursor
        vec3  glowColor = mix(uPrimary, vec3(1.0), proximity * 0.5);
        vec3  finalColor = mix(baseColor, glowColor, proximity * uMouseActive * 0.8);

        float alpha = cell * (0.5 + colorT * 0.5 + proximity * uMouseActive * 0.3);

        // ── grid lines ─────────────────────────────────────────────
        vec2  f        = fract(gl_FragCoord.xy / uPixelSize);
        float gridLine = 1.0 - step(0.06, min(f.x, f.y));
        alpha = mix(alpha, 0.0, gridLine * uGridOpacity * 0.5);

        gl_FragColor = vec4(finalColor, clamp(alpha, 0.0, 1.0));
      }
    `

    const camera = new THREE.Camera()
    camera.position.z = 1
    const scene = new THREE.Scene()
    const geometry = new THREE.PlaneGeometry(2, 2)

    const uniforms = {
      time: { value: 0.0 },
      resolution: { value: new THREE.Vector2() },
      uPixelSize: { value: pixelSize },
      uSpeed: { value: speed },
      uWaveFrequency: { value: waveFrequency },
      uThreshold: { value: threshold },
      uGrainAmount: { value: grainAmount },
      uGridOpacity: { value: gridOpacity },
      uPrimary: { value: new THREE.Vector3(...primary) },
      uMouse: { value: new THREE.Vector2(-999, -999) },
      uMouseActive: { value: 0.0 },
      uCursorRadius: { value: cursorRadius },
      uCursorPush: { value: cursorPush },
    }

    const material = new THREE.ShaderMaterial({
      uniforms,
      vertexShader,
      fragmentShader,
      transparent: true,
    })
    scene.add(new THREE.Mesh(geometry, material))

    const renderer = new THREE.WebGLRenderer({ antialias: false, alpha: true })
    renderer.setClearColor(0x000000, 0)
    renderer.setPixelRatio(window.devicePixelRatio)
    container.appendChild(renderer.domElement)

    const onResize = () => {
      const w = container.clientWidth
      const h = container.clientHeight
      renderer.setSize(w, h)
      uniforms.resolution.value.set(
        renderer.domElement.width,
        renderer.domElement.height
      )
    }
    onResize()
    window.addEventListener("resize", onResize)

    // Smooth mouse tracking with lerp target
    const mouseTarget = new THREE.Vector2(-999, -999)
    const mouseCurrent = new THREE.Vector2(-999, -999)

    const onMouseMove = (e: MouseEvent) => {
      const rect = container.getBoundingClientRect()
      const dpr = renderer.getPixelRatio()
      mouseTarget.set(
        (e.clientX - rect.left) / rect.width,
        1.0 - (e.clientY - rect.top) / rect.height
      )
      uniforms.uMouseActive.value = 1.0
    }
    const onMouseLeave = () => {
      uniforms.uMouseActive.value = 0.0
    }
    container.addEventListener("mousemove", onMouseMove)
    container.addEventListener("mouseleave", onMouseLeave)

    let animationId = 0
    const animate = () => {
      animationId = requestAnimationFrame(animate)
      uniforms.time.value += 0.016
      // Smooth mouse follow
      mouseCurrent.lerp(mouseTarget, 0.12)
      uniforms.uMouse.value.copy(mouseCurrent)
      renderer.render(scene, camera)
      if (sceneRef.current) sceneRef.current.animationId = animationId
    }

    sceneRef.current = { renderer, uniforms, animationId: 0 }
    animate()

    return () => {
      window.removeEventListener("resize", onResize)
      container.removeEventListener("mousemove", onMouseMove)
      container.removeEventListener("mouseleave", onMouseLeave)
      cancelAnimationFrame(sceneRef.current?.animationId ?? 0)
      if (container.contains(renderer.domElement))
        container.removeChild(renderer.domElement)
      renderer.dispose()
      geometry.dispose()
      material.dispose()
    }
  }, [
    pixelSize,
    speed,
    waveFrequency,
    threshold,
    grainAmount,
    gridOpacity,
    cursorRadius,
    cursorPush,
  ])

  return (
    <div
      ref={containerRef}
      className={className}
      style={{ overflow: "hidden" }}
    />
  )
}
```
---

### Scroll Progress <a name="scroll-progress"></a>

A customizable scroll progress bar with gradient variants, sizes, and optional percentage display.

- **Registry Name**: `aliimam/scroll-progress`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/scroll-progress.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/scroll-progress.json
```
```tsx
import { ScrollProgress } from "@/components/ui/scroll-progress"
```
```tsx
Basic
<ScrollProgress />

---

Bottom Positioned Progress Bar
<ScrollProgress position="bottom" />

---

Custom Size & Rounded Corners
<ScrollProgress size="lg" rounded="full" />

---

Gradient Variants
<ScrollProgress variant="rainbow" />
<ScrollProgress variant="ocean" />
<ScrollProgress variant="sunset" />

---

With Glow Effect
<ScrollProgress variant="neon" glow="lg" />

---

Show Scroll Percentage
<ScrollProgress showPercentage />

---

Centered Percentage Indicator
<ScrollProgress
  showPercentage
  percentagePosition="center"
/>

---

Custom Gradient
<ScrollProgress
  variant="custom"
  customGradient="bg-gradient-to-r from-fuchsia-500 via-violet-500 to-cyan-500"
/>

---

Custom Spring Animation
<ScrollProgress
  springConfig={{
    stiffness: 120,
    damping: 20,
  }}
/>

---

Scroll Tracking Inside a Container
const ref = useRef<HTMLDivElement>(null)

return (
  <div ref={ref} className="h-[400px] overflow-y-auto">
    <ScrollProgress container={ref} />
    {/* scrollable content */}
  </div>
)
```
#### Props & API Reference

```typescript
interface ScrollProgressProps extends VariantProps<
  typeof scrollProgressVariants
> {
  className?: string
  customGradient?: string
  springConfig?: {
    stiffness?: number
    damping?: number
    restDelta?: number
  }
  showPercentage?: boolean
  percentagePosition?: "left" | "right" | "center"
  container?: React.RefObject<HTMLElement>
}
```

#### Component Source Code

##### File: `scroll-progress.tsx`

```tsx
"use client"

import { useState } from "react"
import { cva, type VariantProps } from "class-variance-authority"
import {
  motion,
  useMotionValueEvent,
  useScroll,
  useSpring,
} from "framer-motion"

import { cn } from "@/registry/aliimam/lib/utils"

const scrollProgressVariants = cva("fixed z-30 origin-left", {
  variants: {
    variant: {
      default: "bg-gradient-to-r from-blue-500 via-purple-500 to-pink-500",
      rainbow:
        "bg-gradient-to-r from-red-500 via-yellow-500 via-green-500 via-blue-500 to-purple-500",
      ocean: "bg-gradient-to-r from-cyan-400 via-blue-500 to-blue-600",
      sunset: "bg-gradient-to-r from-orange-400 via-red-500 to-pink-500",
      forest: "bg-gradient-to-r from-green-400 via-emerald-500 to-teal-500",
      monochrome: "bg-gradient-to-r from-gray-600 via-gray-800 to-black",
      neon: "bg-gradient-to-r from-pink-500 via-purple-500 to-indigo-500",
      fire: "bg-gradient-to-r from-yellow-400 via-orange-500 to-red-600",
      ice: "bg-gradient-to-r from-blue-200 via-cyan-300 to-blue-400",
      gold: "bg-gradient-to-r from-yellow-300 via-yellow-500 to-yellow-600",
      solid: "bg-blue-500",
      custom: "", // For custom gradients
    },
    size: {
      xs: "h-0.5",
      sm: "h-1",
      default: "h-1.5",
      lg: "h-2",
      xl: "h-3",
      "2xl": "h-4",
    },
    position: {
      top: "inset-x-0 top-0",
      bottom: "inset-x-0 bottom-0",
    },
    rounded: {
      none: "",
      sm: "rounded-sm",
      default: "rounded",
      lg: "rounded-lg",
      xl: "rounded-xl",
      full: "rounded-full",
    },
    glow: {
      none: "",
      sm: "shadow-sm",
      default: "shadow-md",
      lg: "shadow-lg drop-shadow-lg",
      xl: "shadow-xl drop-shadow-xl",
    },
  },
  defaultVariants: {
    variant: "default",
    size: "default",
    position: "top",
    rounded: "none",
    glow: "none",
  },
})

interface ScrollProgressProps extends VariantProps<
  typeof scrollProgressVariants
> {
  className?: string
  customGradient?: string
  springConfig?: {
    stiffness?: number
    damping?: number
    restDelta?: number
  }
  showPercentage?: boolean
  percentagePosition?: "left" | "right" | "center"
  container?: React.RefObject<HTMLElement>
}

export function ScrollProgress({
  className,
  variant,
  size,
  position,
  rounded,
  glow,
  customGradient,
  springConfig = {
    stiffness: 200,
    damping: 50,
    restDelta: 0.001,
  },
  showPercentage = false,
  percentagePosition = "right",
  container,
}: ScrollProgressProps) {
  const { scrollYProgress } = useScroll(container ? { container } : undefined)
  const scaleX = useSpring(scrollYProgress, springConfig)

  const [percentage, setPercentage] = useState(0)
  useMotionValueEvent(scrollYProgress, "change", (latest) => {
    setPercentage(Math.round(latest * 100))
  })

  const progressBarClasses = cn(
    scrollProgressVariants({ variant, size, position, rounded, glow }),
    variant === "custom" && customGradient,
    className
  )

  const percentageClasses = cn(
    "fixed z-40 text-xs font-medium text-foreground bg-background/80 backdrop-blur-sm px-2 py-1 rounded",
    position === "top" ? "top-2" : "bottom-2",
    percentagePosition === "left" && "left-4",
    percentagePosition === "right" && "right-4",
    percentagePosition === "center" && "left-1/2 -translate-x-1/2"
  )

  return (
    <>
      <motion.div
        className={progressBarClasses}
        style={{
          scaleX,
        }}
      />
      {showPercentage && (
        <motion.div
          className={percentageClasses}
          style={{
            opacity: scrollYProgress,
          }}
        >
          <motion.span>{percentage}%</motion.span>
        </motion.div>
      )}
    </>
  )
}
```
---

### Shine Border <a name="shine-border"></a>

Renders an animated neon gradient border around any child content. The border is drawn using a masked overlay with CSS animations — no Tailwind or external CSS required. Supports customization of radius, width, speed, and colors.

- **Registry Name**: `aliimam/shine-border`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/shine-border.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/shine-border.json
```
```tsx
import { ShineBorder } from "@/components/ui/shine-border"
```
```tsx
<div className="relative h-screen w-full overflow-hidden">
  <ShineBorder borderRadius={12} borderWidth={3} duration={10}>
    <span style={{ color: "white", fontSize: 20 }}>Neon Glow ✨</span>
  </ShineBorder>
</div>
```
#### Props & API Reference

```typescript
interface ShineBorderProps {
  borderRadius?: number
  borderWidth?: number
  duration?: number
  color?: TColorProp
  className?: string
  children: React.ReactNode
}
```

#### Component Source Code

##### File: `shine-border.tsx`

```tsx
/* eslint-disable @typescript-eslint/no-explicit-any */
/* eslint-disable @typescript-eslint/ban-ts-comment */
/* eslint-disable react/no-unknown-property */
"use client"

import type React from "react"

type TColorProp = string | string[]

interface ShineBorderProps {
  borderRadius?: number
  borderWidth?: number
  duration?: number
  color?: TColorProp
  className?: string
  children: React.ReactNode
}

/**
 * ShineBorder
 * - Self-contained, no Tailwind, no external CSS.
 * - Uses an absolutely-positioned overlay with CSS masking to render an animated border.
 * - Customizable via props: borderRadius, borderWidth, duration, color (string or array).
 */
export function ShineBorder({
  borderRadius = 8,
  borderWidth = 1,
  duration = 14,
  color = [
    "#ff00ff", // neon pink
    "#00ffff", // neon cyan
    "#ff3131", // neon red
    "#00ff00", // neon green
    "#ffea00", // neon yellow
    "#ff6ec7", // hot magenta
    "#39ff14", // electric lime
    "#ff8300", // neon orange
    "#7df9ff", // electric blue
    "#fe019a", // neon fuchsia
  ],

  className,
  children,
}: ShineBorderProps) {
  const colorString =
    Array.isArray(color) && color.length > 0 ? color.join(",") : String(color)

  const rootStyle: React.CSSProperties = {
    // Expose variables for internal CSS
    // Note: These are referenced by the <style> block below
    // for animation, masking, and layout.
    // Using px ensures predictable rendering.
    // Consumers can still style the wrapper via className or style if desired.
    //@ts-ignore: CSS variables allowed at runtime
    "--sb-border-radius": `${borderRadius}px`,
    //@ts-ignore
    "--sb-border-width": `${borderWidth}px`,
    //@ts-ignore
    "--sb-duration": `${duration}s`,
  }

  const overlayStyle: React.CSSProperties = {
    position: "absolute",
    inset: 0,
    // padding defines the visible border thickness via mask
    padding: `var(--sb-border-width)`,
    borderRadius: `var(--sb-border-radius)`,
    backgroundImage: `radial-gradient(transparent, transparent, ${colorString}, transparent, transparent)`,
    backgroundSize: "300% 300%",
    animation: "sb-shine-pulse var(--sb-duration) linear infinite",
    // Show only the border region using masks (content-box vs border-box)
    // WebKit/Safari
    WebkitMask:
      "linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0)",
    WebkitMaskComposite: "xor" as any,
    // Standard (Chromium/Firefox)
    mask: "linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0)",
    maskComposite: "exclude" as any,
    pointerEvents: "none",
  }

  const containerStyle: React.CSSProperties = {
    position: "relative",
    display: "grid",
    placeItems: "center",
    width: "100%",
    height: "100%",
    borderRadius: `var(--sb-border-radius)`,
  }

  return (
    <div style={rootStyle} className={className}>
      {/* Local styles for the component (no external CSS or Tailwind needed) */}
      <style>{`
        @keyframes sb-shine-pulse {
          0%   { background-position: 0% 0%; }
          50%  { background-position: 100% 100%; }
          100% { background-position: 0% 0%; }
        }
      `}</style>

      <div style={containerStyle}>
        <div aria-hidden="true" style={overlayStyle} />
        {children}
      </div>
    </div>
  )
}
```
---

### Typewriter <a name="typewriter"></a>

Use the Typewriter component inside headings to create animated subheadings for hero sections, landing pages, or portfolio intros.

- **Registry Name**: `aliimam/typewriter`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://aliimam.in/r/typewriter.json
```

#### How to Use

```bash
npx shadcn@latest add https://aliimam.in/r/typewriter.json
```
```tsx
import { Typewriter } from "@/components/typewriter"
```
```tsx
<Typewriter words={["Developer", "Designer", "Freelancer"]} />
```
#### Props & API Reference

```typescript
interface TypewriterProps {
  words?: string[]
  speed?: number
  delayBetweenWords?: number
  cursor?: boolean
  cursorChar?: string
  className?: string
}
```

#### Component Source Code

##### File: `typewriter.tsx`

```tsx
"use client"

import { useEffect, useState } from "react"

interface TypewriterProps {
  words?: string[]
  speed?: number
  delayBetweenWords?: number
  cursor?: boolean
  cursorChar?: string
  className?: string
}

export function Typewriter({
  words = [],
  speed = 100,
  delayBetweenWords = 2000,
  cursor = true,
  cursorChar = "|",
  className = "",
}: TypewriterProps) {
  const [displayText, setDisplayText] = useState("")
  const [isDeleting, setIsDeleting] = useState(false)
  const [wordIndex, setWordIndex] = useState(0)
  const [charIndex, setCharIndex] = useState(0)
  const [showCursor, setShowCursor] = useState(true)

  const currentWord = words[wordIndex]

  useEffect(() => {
    const timeout = setTimeout(
      () => {
        // Typing logic
        if (!isDeleting) {
          if (charIndex < currentWord.length) {
            setDisplayText(currentWord.substring(0, charIndex + 1))
            setCharIndex(charIndex + 1)
          } else {
            // Word is complete, wait before deleting
            setTimeout(() => {
              setIsDeleting(true)
            }, delayBetweenWords)
          }
        } else {
          // Deleting logic
          if (charIndex > 0) {
            setDisplayText(currentWord.substring(0, charIndex - 1))
            setCharIndex(charIndex - 1)
          } else {
            // Word is deleted, move to next word
            setIsDeleting(false)
            setWordIndex((prev) => (prev + 1) % words.length)
          }
        }
      },
      isDeleting ? speed / 2 : speed
    )

    return () => clearTimeout(timeout)
  }, [
    charIndex,
    currentWord,
    isDeleting,
    speed,
    delayBetweenWords,
    wordIndex,
    words,
  ])

  // Cursor blinking effect
  useEffect(() => {
    if (!cursor) return

    const cursorInterval = setInterval(() => {
      setShowCursor((prev) => !prev)
    }, 500)

    return () => clearInterval(cursorInterval)
  }, [cursor])

  return (
    <div className="inline-block">
      <span className={className}>
        {displayText}
        {cursor && (
          <span
            className="ml-1 transition-opacity duration-75"
            style={{ opacity: showCursor ? 1 : 0 }}
          >
            {cursorChar}
          </span>
        )}
      </span>
    </div>
  )
}
```
---
