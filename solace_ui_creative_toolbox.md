# Solace UI Creative Developer Reference Guide

A comprehensive developer guide for **Solace UI** (solaceui.com) — a library of clean, responsive component blocks built using Radix UI, Framer Motion, Tailwind CSS, and TypeScript.

## Table of Contents
- [Bento Grid 1](#bento-grid-1)
- [Bento Grid 2](#bento-grid-2)
- [Chat Messages 1](#chat-messages-1)
- [Chat Messages 2](#chat-messages-2)
- [Footer Section 1](#footer-section-1)
- [Footer Section 2](#footer-section-2)
- [Footer Section 3](#footer-section-3)
- [Footer Section 4](#footer-section-4)
- [Footer Section 5](#footer-section-5)
- [Footer Section 6](#footer-section-6)
- [Hero Section 1](#hero-section-1)
- [Hero Section 2](#hero-section-2)
- [Hero Section 3](#hero-section-3)
- [Hero Section 4](#hero-section-4)
- [Hero Section 5](#hero-section-5)
- [Hero Section 6](#hero-section-6)
- [Hero Section 7](#hero-section-7)
- [Hero Section 8](#hero-section-8)
- [Hero Video 1](#hero-video-1)
- [Hero Video 2](#hero-video-2)
- [Phone Mockups 1](#phone-mockups-1)
- [Team Section 1](#team-section-1)
- [Team Section 2](#team-section-2)
- [Team Section 3](#team-section-3)
- [Team Section 4](#team-section-4)
- [Team Section 5](#team-section-5)
- [Team Section 6](#team-section-6)
- [Testimonial Section 1](#testimonial-section-1)
- [Testimonial Section 2](#testimonial-section-2)
- [Testimonial Section 3](#testimonial-section-3)
- [Testimonial Section 4](#testimonial-section-4)

---

## Bento Grid 1 <a name="bento-grid-1"></a>

Solace UI bento grid block variation 1.

- **Registry Key**: `bento-grid-1`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/bento-grid-1.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/bento-grid/one/index.tsx`
```tsx
import {
    BentoGrid,
    BentoItem,
    ChatMessaging,
    CompanyLogos,
} from "@/registry/blocks/bento-grid/one/bento-grid-template-one";


export default function BentoGridOne() {

    const items: BentoItem[] = [

        {

            id: "1",

            type: "feature",

            title: "World-Class Information Design",

            description:

                "Transform complex data into crisp visuals that quickly tell your story.",

            image:

                "https://res.cloudinary.com/harshitproject/image/upload/v1746774246/hero-video.jpg",

        },

        {

            id: "2",

            type: "chat",

            content: <ChatMessaging />,

        },

        {

            id: "3",

            type: "partners",

            title: "Connected Everywhere",

            description:

                "Embed your work seamlessly across your favorite platforms for instant sharing.",

            content: <CompanyLogos />,

        },

    ];



    return (

        <main className="flex min-h-screen flex-col items-center justify-center p-4 md:p-24 ">

            <BentoGrid items={items} />

        </main>

    );

}
```
##### File: `src/registry/blocks/bento-grid/one/bento-grid-template-one.tsx`
```tsx
"use client";



import type { ReactNode } from "react";

import { motion } from "motion/react";

import Image from "next/image";

import { cn } from "@/lib/utils";



// Types for our bento grid items

export type BentoItem = {

    id: string;

    type: "feature" | "chat" | "partners";

    title?: string;

    description?: string;

    image?: string;

    className?: string;

    content?: ReactNode;

};



// Props for our base BentoGrid component

interface BentoGridProps {

    items: BentoItem[];

    className?: string;

}



// Props for each bento item

interface BentoItemProps {

    item: BentoItem;

    className?: string;

}



// Base BentoGrid component

export function BentoGrid({ items, className }: BentoGridProps) {

    return (

        <div

            className={cn(

                "grid grid-cols-1 md:grid-cols-2 gap-4 max-w-7xl mx-auto",

                className

            )}

        >

            {items.map((item) => {

                return (

                    <div

                        key={item.id}

                        className={cn(

                            "row-span-1",

                            item.type === "feature" && "col-span-1 md:col-span-2",

                            item.className

                        )}

                    >

                        <BentoGridItem item={item} />

                    </div>

                );

            })}

        </div>

    );

}



// Individual BentoGridItem component

function BentoGridItem({ item, className }: BentoItemProps) {

    const { type, title, description, image, content } = item;



    // Different layouts based on item type

    switch (type) {

        case "feature":

            return (

                <motion.div

                    initial={{ opacity: 0, y: 20 }}

                    animate={{ opacity: 1, y: 0 }}

                    transition={{ duration: 0.5 }}

                    className={cn(

                        "group relative overflow-hidden rounded-xl bg-[#19ffc2ec] dark:bg-neutral-900 p-8 h-[400px] flex flex-col md:flex-row shadow-md",

                        className

                    )}

                >

                    <div className="flex flex-col justify-center z-10 md:w-1/2">

                        <span className="text-xs uppercase  dark:text-[#1DCD9F] text-black font-medium mb-2">

                            Visualise Info

                        </span>

                        <h2 className="text-3xl md:text-4xl font-bold  dark:text-white text-black mb-4 text-balance">

                            {title}

                        </h2>

                        <p className="text-sm text-neutral-600 max-w-md text-balance">

                            {description}

                        </p>

                    </div>



                    {image && (

                        <div className=" md:w-1/2 h-full right-0 top-0 flex items-center justify-center">

                            <div className="relative w-full h-full">

                                <motion.div

                                    initial={{ rotate: 0 }}

                                    animate={{ scale: 1.1 }}

                                    transition={{ duration: 0.7 }}

                                    className="absolute inset-0 z-0"

                                >

                                    <div className="relative w-full h-full top-10 left-10">

                                        <Image

                                            src={image || "/placeholder.svg"}

                                            alt="visualise"

                                            fill

                                            className="object-cover rounded-md shadow-xl dark:shadow-[#1DCD9F] shadow-white"

                                        />

                                    </div>

                                </motion.div>

                            </div>

                        </div>

                    )}

                    <div className="absolute bottom-0 z-50 inset-x-0 h-[2rem] bg-gradient-to-t dark:from-neutral-900 from-[#1DCD9F] to-transparent w-full pointer-events-none" />

                </motion.div>

            );



        case "chat":

            return (

                <motion.div

                    initial={{ opacity: 0, y: 20 }}

                    animate={{ opacity: 1, y: 0 }}

                    transition={{ duration: 0.5, delay: 0.1 }}

                    className={cn(

                        "group relative overflow-hidden rounded-xl bg-[#222222] dark:bg-neutral-900 p-8 h-[400px] flex flex-col shadow-md",

                        className

                    )}

                >

                    <div className="z-10">

                        <span className="text-xs uppercase text-[#1DCD9F] font-medium mb-2 flex items-center justify-center p-2">

                            Customise

                        </span>

                    </div>



                    <div className="flex-1 relative">{content}</div>

                    <div className="absolute bottom-0 z-50 inset-x-0 h-[2rem] bg-gradient-to-t from-neutral-900 to-transparent w-full pointer-events-none" />

                </motion.div>

            );



        case "partners":

            return (

                <motion.div

                    initial={{ opacity: 0, y: 20 }}

                    animate={{ opacity: 1, y: 0 }}

                    transition={{ duration: 0.5, delay: 0.2 }}

                    className={cn(

                        "group relative overflow-hidden rounded-xl bg-neutral-900 p-8 h-[400px] flex flex-col shadow-md",

                        className

                    )}

                >

                    <div className="flex-1 relative flex items-center justify-center">

                        {content}

                        <div className="absolute left-0 z-[100] inset-y-0 w-10 bg-gradient-to-r from-neutral-900 to-transparent  h-full pointer-events-none" />

                        <div className="absolute right-0 z-[100] inset-y-0 w-10 bg-gradient-to-l from-neutral-900  to-transparent h-full pointer-events-none" />

                    </div>



                    <div className="mt-4 z-10">

                        <span className="text-xs uppercase text-[#1DCD9F] font-medium ">

                            Embed

                        </span>

                        <h3 className="text-2xl font-bold text-white mb-2 text-balance">

                            {title}

                        </h3>

                        <p className="text-sm dark:text-neutral-600 text-neutral-700 text-balance">

                            {description}

                        </p>

                    </div>

                </motion.div>

            );



        default:

            return null;

    }

}



// Company logos component for the partners section

export function CompanyLogos() {

    const logos = [

        {

            name: "Spotify",

            url: "https://res.cloudinary.com/harshitproject/image/upload/v1746774292/Spotify.png",

        },

        {

            name: "Notion",

            url: "https://res.cloudinary.com/harshitproject/image/upload/v1746774291/Notion.png",

        },

        {

            name: "Slack",

            url: "https://res.cloudinary.com/harshitproject/image/upload/v1746774292/Slack.png",

        },

        {

            name: "Twitter",

            url: "https://res.cloudinary.com/harshitproject/image/upload/v1746774292/Twitter.png",

        },

        {

            name: "Apple",

            url: "https://res.cloudinary.com/harshitproject/image/upload/v1746774291/Apple.png",

        },

    ];



    return (

        <div className="w-full overflow-hidden">

            <motion.div

                className="flex gap-8 items-center justify-center"

                animate={{ x: [0, -1000] }}

                transition={{

                    x: {

                        repeat: Number.POSITIVE_INFINITY,

                        repeatType: "loop",

                        duration: 20,

                        ease: "linear",

                    },

                }}

            >

                {/* First set of logos */}

                {logos.map((logo) => (

                    <div

                        key={logo.name}

                        className="flex-shrink-0 w-16 h-16 bg-neutral-800 rounded-lg flex items-center justify-center p-2"

                    >

                        <Image

                            src={logo.url || "/placeholder.svg"}

                            alt={logo.name}

                            width={40}

                            height={40}

                            className="w-10 h-10 object-contain"

                        />

                    </div>

                ))}



                {/* Duplicate set for seamless looping */}

                {logos.map((logo) => (

                    <div

                        key={`${logo.name}-dup`}

                        className="flex-shrink-0 w-16 h-16 bg-neutral-800 rounded-lg flex items-center justify-center p-2"

                    >

                        <Image

                            src={logo.url || "/placeholder.svg"}

                            alt={logo.name}

                            width={40}

                            height={40}

                            className="w-10 h-10 object-contain"

                        />

                    </div>

                ))}

            </motion.div>

        </div>

    );

}



// Chat messaging component with iPhone mockup

export function ChatMessaging() {

    return (

        <div className="relative w-full h-full flex items-center justify-center">

            <div className="relative w-[280px] h-[500px] bg-neutral-800 rounded-[36px] p-3 border-4 border-neutral-700 overflow-hidden">

                <div className="absolute top-0 left-1/2 transform -translate-x-1/2 w-1/3 h-6 bg-neutral-700 rounded-b-xl z-10" />

                <div className="w-full h-full bg-neutral-900 rounded-[28px] p-4 overflow-hidden">

                    <div className="flex items-center mb-4">

                        <div className="w-8 h-8 rounded-full bg-[#1DCD9F] mr-3" />

                        <div>

                            <div className="text-white text-xs font-medium">Assistant</div>

                            <div className="text-neutral-400 text-[10px]">Online</div>

                        </div>

                    </div>



                    <div className="space-y-3">

                        <motion.div

                            initial={{ opacity: 0, x: -20 }}

                            animate={{ opacity: 1, x: 0 }}

                            transition={{ duration: 0.5 }}

                            className="bg-neutral-800 p-2 rounded-lg rounded-tl-none max-w-[80%] text-[10px] text-white"

                        >

                            How can I help you today?

                        </motion.div>



                        <motion.div

                            initial={{ opacity: 0, x: 20 }}

                            animate={{ opacity: 1, x: 0 }}

                            transition={{ duration: 0.5, delay: 0.3 }}

                            className="bg-[#1DCD9F] p-2 rounded-lg rounded-tr-none max-w-[80%] ml-auto text-[10px] text-white"

                        >

                            I need help with my project

                        </motion.div>



                        <motion.div

                            initial={{ opacity: 0, x: -20 }}

                            animate={{ opacity: 1, x: 0 }}

                            transition={{ duration: 0.5, delay: 0.6 }}

                            className="bg-neutral-800 p-2 rounded-lg rounded-tl-none max-w-[80%] text-[10px] text-white"

                        >

                            I&apos;d be happy to help! What kind of project are you working

                            on?

                        </motion.div>



                        <motion.div

                            initial={{ opacity: 0, x: 20 }}

                            animate={{ opacity: 1, x: 0 }}

                            transition={{ duration: 0.5, delay: 0.9 }}

                            className="bg-[#1DCD9F] p-2 rounded-lg rounded-tr-none max-w-[80%] ml-auto text-[10px] text-white"

                        >

                            I&apos;m building a component library with Next.js and Tailwind

                        </motion.div>

                    </div>

                </div>

            </div>

        </div>

    );

}
```
---

## Bento Grid 2 <a name="bento-grid-2"></a>

Solace UI bento grid block variation 2.

- **Registry Key**: `bento-grid-2`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/bento-grid-2.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### Component Source Code
##### File: `src/registry/blocks/bento-grid/two/index.tsx`
```tsx
"use client";



import { useState } from "react";

import {
    BentoGridTemplateTwo,
    BentoItem,
} from "@/registry/blocks/bento-grid/two/bento-grid-template-two";


const sampleBentoData: BentoItem[] = [

    {

        id: "1",

        title: "Visual Design System",

        description:

            "A beautiful and comprehensive design system built for modern applications.",

        image: "https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?q=80&w=2564&auto=format&fit=crop",

        size: "large",

        priority: 1,

        tag: "Featured",

        variant: "glass", //Semi‑transparent frosted panel with content overlay

        accentColor: "#FFFFFF",

        link: "#design",

    },

    {

        id: "2",

        title: "AI Studio",

        description: "Create, train, and deploy AI models with intuitive tools.",

        variant: "highlight", // Vibrant accent color with crisp white text

        tag: "New",

        accentColor: "#FFFFFF",

        link: "#ai",

    },

    {

        id: "3",

        title: "Analytics",

        description: "Real-time insights and beautiful data visualizations.",

        variant: "solid", // Fully colored background with custom text color

        color: "#18181b",

        accentColor: "#FFFFFF",

        link: "#analytics",

    },

    {

        id: "4",

        title: "Cloud Platform",

        description: "Scalable infrastructure for modern applications.",

        image: "https://images.unsplash.com/photo-1451187580459-43490279c0fa?q=80&w=2672&auto=format&fit=crop",

        variant: "default", // Clean card with light/dark adaptive backgrounds

        size: "wide",

        accentColor: "#FFFFFF",

        link: "#cloud",

    },

    {

        id: "5",

        title: "Security Suite",

        description: "Enterprise-grade security for your applications.",

        variant: "solid",

        color: "#0f172a",

        accentColor: "#FFFFFF",

        tag: "Premium",

        link: "#security",

    },

    {

        id: "6",

        title: "Developer Hub",

        description: "Comprehensive documentation and resources.",

        image: "https://images.unsplash.com/photo-1555066931-4365d14bab8c?q=80&w=2670&auto=format&fit=crop",

        variant: "glass",

        accentColor: "#FFFFFF",

        link: "#docs",

    },

    {

        id: "7",

        title: "Collaboration",

        description: "Tools for teams to work better together.",

        variant: "solid",

        accentColor: "#FFFFFF",

        color: "#020617",

        link: "#collaboration",

    },

    {

        id: "8",

        title: "Community",

        description: "Join thousands of developers building amazing things.",

        image: "https://images.unsplash.com/photo-1522071820081-009f0129c71c?q=80&w=2670&auto=format&fit=crop",

        variant: "default",

        accentColor: "#FFFFFF",

        link: "#community",

    },

];



export default function BentoGridTwo() {

    const [items] = useState<BentoItem[]>(sampleBentoData);



    return (

        <div className="container mx-auto px-6 py-16 md:py-24">

            <BentoGridTemplateTwo items={items} gap={6} animate={true} />

        </div>

    );

}
```
##### File: `src/registry/blocks/bento-grid/two/bento-grid-template-two.tsx`
```tsx
"use client";



import React from "react";

import { motion } from "motion/react";

import { cn } from "@/lib/utils";

import Link from "next/link";

import { ArrowUpRight } from "lucide-react";

import Image from "next/image";



export type BentoItem = {

    id: string;

    title: string;

    description?: string;

    image?: string;

    link?: string;

    size?: "small" | "medium" | "large" | "wide" | "tall";

    color?: string; // background for solid variants

    accentColor?: string; // text color override

    variant?: "default" | "highlight" | "glass" | "solid";

    tag?: string;

    priority?: number;

};



export type BentoGridProps = {

    items: BentoItem[];

    className?: string;

    animate?: boolean;

    gap?: number;

};



export function BentoGridTemplateTwo({

    items,

    className,

    animate = true,

    gap = 4,

}: BentoGridProps) {

    const cols = 4;

    const leftover = items.length % cols;



    return (

        <div

            className={cn(

                "grid grid-cols-1 md:grid-cols-4 auto-rows-[minmax(240px,auto)]",

                className

            )}

            style={{ gap: `${gap * 0.25}rem` }}

        >

            {items.map((item, i) => {

                const isLastSingle = leftover === 1 && i === items.length - 1;

                return (

                    <motion.div

                        key={item.id}

                        initial={animate ? { opacity: 0, y: 20 } : undefined}

                        animate={animate ? { opacity: 1, y: 0 } : undefined}

                        transition={{

                            duration: 0.5,

                            delay: animate ? i * 0.05 : 0,

                            ease: [0.21, 0.58, 0.36, 1] as [number, number, number, number],
                        }}

                        className={cn(

                            "group/bento relative",

                            "col-span-1",

                            item.size === "large" && "md:col-span-2 md:row-span-2",

                            item.size === "wide" && "md:col-span-2",

                            item.size === "tall" &&

                            item.variant !== "glass" &&

                            "md:row-span-2",

                            item.priority === 1 && "md:col-span-2 md:row-span-2",

                            isLastSingle && "md:col-span-4"

                        )}

                    >

                        <BentoGridItem item={item} />

                    </motion.div>

                );

            })}

        </div>

    );

}



function BentoGridItem({ item }: { item: BentoItem }) {

    const {

        title,

        description,

        image,

        link,

        color,

        accentColor,

        variant = "default",

        tag,

    } = item;



    const hasImage = Boolean(image);



    // Base classes

    const classes = [

        "group relative overflow-hidden rounded-[2rem] transition-all duration-500 h-full flex flex-col",

        "shadow-sm hover:shadow-xl hover:shadow-black/5 dark:hover:shadow-white/5",

    ];



    let contentWrapperClasses = "relative z-20 flex flex-col h-full p-8";

    let textClasses = "";

    let descriptionClasses = "";

    let tagClasses = "inline-flex items-center rounded-full px-3 py-1 text-[11px] uppercase tracking-wider font-semibold backdrop-blur-md transition-colors";



    // Variants

    if (variant === "default") {

        if (hasImage) {

            classes.push("bg-zinc-950", "text-zinc-50");

            textClasses = "text-zinc-50";

            descriptionClasses = "text-zinc-300";

            tagClasses = cn(tagClasses, "bg-white/10 text-white border border-white/20");

            contentWrapperClasses = cn(contentWrapperClasses, "justify-end");

        } else {

            classes.push(

                "bg-zinc-50 dark:bg-zinc-900/40",

                "border border-zinc-200/60 dark:border-zinc-800/60"

            );

            textClasses = "text-zinc-950 dark:text-zinc-50";

            descriptionClasses = "text-zinc-600 dark:text-zinc-400";

            tagClasses = cn(tagClasses, "bg-zinc-200/50 dark:bg-zinc-800/50 text-zinc-700 dark:text-zinc-300 border border-zinc-300/50 dark:border-zinc-700/50");

            contentWrapperClasses = cn(contentWrapperClasses, "justify-between");

        }

    } else if (variant === "highlight") {

        classes.push(

            "bg-zinc-900 dark:bg-zinc-100",

            "text-zinc-50 dark:text-zinc-900"

        );

        textClasses = "text-zinc-50 dark:text-zinc-900";

        descriptionClasses = "text-zinc-400 dark:text-zinc-600";

        tagClasses = cn(tagClasses, "bg-white/10 dark:bg-black/10 text-white dark:text-black border border-white/20 dark:border-black/20");

        contentWrapperClasses = cn(contentWrapperClasses, "justify-between");

    } else if (variant === "glass") {

        classes.push(

            hasImage ? "bg-zinc-950" : "bg-white/40 dark:bg-zinc-900/40 backdrop-blur-2xl",

            "border border-zinc-200/50 dark:border-zinc-800/50"

        );

        textClasses = hasImage ? "text-zinc-50" : "text-zinc-900 dark:text-zinc-50";

        descriptionClasses = hasImage ? "text-zinc-300" : "text-zinc-600 dark:text-zinc-400";

        tagClasses = cn(tagClasses, hasImage ? "bg-white/10 text-white border border-white/20" : "bg-zinc-900/5 dark:bg-white/10 text-zinc-900 dark:text-white border border-zinc-900/10 dark:border-white/20");

        contentWrapperClasses = cn(contentWrapperClasses, hasImage ? "justify-end" : "justify-between");

    } else if (variant === "solid") {

        classes.push("text-white");

        textClasses = "text-white";

        descriptionClasses = "text-white/80";

        tagClasses = cn(tagClasses, "bg-white/20 text-white border border-white/20");

        contentWrapperClasses = cn(contentWrapperClasses, "justify-between");

    }



    const ContentCore = (

        <div

            className={cn(...classes)}

            style={

                variant === "solid" && color ? { backgroundColor: color } : undefined

            }

        >

            {/* Subtle inner ring for premium finish */}

            <div className="absolute inset-0 z-20 pointer-events-none rounded-[2rem] ring-1 ring-inset ring-black/5 dark:ring-white/10" />



            {image && (

                <div className="absolute inset-0 z-0 overflow-hidden">

                    <Image

                        src={image}

                        alt={title}

                        fill

                        className="object-cover transition-transform duration-1000 ease-out group-hover:scale-105"

                    />

                    {/* Elegant gradient overlay specifically optimized for typography */}

                    <div className="absolute inset-0 bg-gradient-to-t from-zinc-950/90 via-zinc-950/30 to-transparent z-10 transition-opacity duration-500 group-hover:opacity-80" />

                </div>

            )}



            <div className={contentWrapperClasses}>

                <div className={cn("space-y-6 flex flex-col items-start", hasImage && "mt-auto")}>

                    {tag && (

                        <span className={tagClasses}>

                            {tag}

                        </span>

                    )}



                    <div className="space-y-3">

                        <h3 className={cn("text-2xl font-semibold tracking-tight", textClasses)}>

                            {title}

                        </h3>



                        {description && (

                            <p className={cn("text-base leading-relaxed max-w-sm", descriptionClasses)}>

                                {description}

                            </p>

                        )}

                    </div>

                </div>



                {link && (

                    <div className={cn("mt-8 flex items-center", hasImage ? "justify-end" : "justify-start")}>

                        <div 

                            className={cn(

                                "flex items-center gap-2 text-sm font-medium transition-all duration-300 ease-out",

                                textClasses,

                                "opacity-60 group-hover:opacity-100"

                            )}

                        >

                            <span>Explore</span>

                            <ArrowUpRight className="h-4 w-4 transition-transform duration-500 ease-out group-hover:translate-x-1 group-hover:-translate-y-1" />

                        </div>

                    </div>

                )}

            </div>

            

            {/* Subtle glow effect on non-image cards */}

            {!hasImage && variant !== "highlight" && variant !== "solid" && (

                <div className="absolute inset-0 z-0 bg-gradient-to-br from-white/0 to-white/0 transition-colors duration-500 group-hover:from-white/40 group-hover:to-white/0 dark:group-hover:from-white/5 dark:group-hover:to-white/0 pointer-events-none rounded-[2rem]" />

            )}

        </div>

    );



    if (link) {

        return (

            <Link href={link} className="block h-full w-full">

                {ContentCore}

            </Link>

        );

    }



    return ContentCore;

}
```
---

## Chat Messages 1 <a name="chat-messages-1"></a>

Solace UI chat messages component variation 1.

- **Registry Key**: `chat-messages-1`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/chat-messages-1.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### Component Source Code
##### File: `src/registry/components/chat-messages/one/index.tsx`
```tsx
"use client";



import { useState } from "react";

import Chat, { type User } from "@/registry/components/chat-messages/chat";

import type { Message } from "@/registry/components/chat-messages/chat-message";

import IphoneFrame from "@/registry/components/chat-messages/iphone-frame";



export default function ChatMessagesDemo() {

    // Define users with their avatars

    const users: User[] = [

        {

            name: "User1",

            avatar:

                "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png", // Replace with actual avatar URL

        },

        {

            name: "User2",

            avatar:

                "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-four.png", // Replace with actual avatar URL

        },

    ];



    const [messages] = useState<Message[]>([

        {

            id: "m1",

            name: "User1",

            message: "Hey, why'd you call it Solace UI ?",

        },

        {

            id: "m2",

            name: "User2",

            message: "Because it is meant to be chill and stress free.",

        },

        {

            id: "m3",

            name: "User1",

            message: "So is it like a digital chill zone?",

        },

        {

            id: "m4",

            name: "User2",

            message: "Exactly, simple and calm design.",

        },

    ]);



    return (

        <main className="relative flex flex-col items-center justify-center p-4 ">

            {/* Clipping Wrapper */}

            <div className="mx-auto overflow-hidden w-[300px] h-[360px] sm:w-[350px] sm:h-[400px]">

                <IphoneFrame>

                    <Chat

                        messages={messages}

                        currentUser="User2"

                        users={users} // Pass the users array with avatars

                    />

                </IphoneFrame>





            </div>

            <div

                className="absolute bottom-0 left-0 right-0 z-50 h-24 sm:h-32 bg-gradient-to-t from-background to-transparent pointer-events-none"

                aria-hidden="true"

            ></div>

        </main>

    );

}
```
##### File: `src/registry/components/chat-messages/chat.tsx`
```tsx
"use client";



import { type FC, useRef, useEffect, useState, useLayoutEffect } from "react";

import { motion, AnimatePresence } from "motion/react";

import ChatMessage, { type Message } from "@/registry/components/chat-messages/chat-message";


export interface User {

    name: string;

    avatar: string;

}



interface ChatProps {

    messages: Message[];

    currentUser: string;

    users: User[];

}



const Chat: FC<ChatProps> = ({ messages, currentUser, users }) => {

    const containerRef = useRef<HTMLDivElement>(null);

    const [visibleMessages, setVisibleMessages] = useState<Message[]>([]);



    useEffect(() => {

        if (messages.length === 0) return;



        setVisibleMessages([]);

        let cancelled = false;



        (async () => {

            for (const msg of messages) {

                if (cancelled) return; // guard before delay

                await new Promise((r) => setTimeout(r, 700));

                if (cancelled) return; // guard after delay

                setVisibleMessages((prev) => [...prev, msg]);

            }

        })();



        return () => {

            cancelled = true;

        };

    }, [messages]);



    useLayoutEffect(() => {

        const el = containerRef.current;

        if (el) el.scrollTop = el.scrollHeight;

    }, [visibleMessages]);



    const getUserAvatar = (name: string) =>

        users.find((u) => u.name === name)?.avatar ??

        "/placeholder.svg?height=40&width=40";



    return (

        <div

            ref={containerRef}

            className="w-full h-full overflow-y-auto p-2 bg-white dark:bg-[#212121] flex flex-col py-[2rem]"

        >

            <AnimatePresence initial={false} mode="popLayout">

                {visibleMessages.map((message) => (

                    <motion.div

                        key={message.id}

                        layout

                        initial={{ opacity: 0, y: 20 }}

                        animate={{ opacity: 1, y: 0 }}

                        exit={{ opacity: 0, y: 20 }}

                        transition={{ duration: 0.4 }}

                        className={`flex mb-4 ${message.name === currentUser ? "justify-end" : "justify-start"

                            } items-end`}

                    >

                        <ChatMessage

                            message={message}

                            isCurrentUser={message.name === currentUser}

                            avatar={getUserAvatar(message.name)}

                        />

                    </motion.div>

                ))}

            </AnimatePresence>



            {visibleMessages.length > 0 &&

                visibleMessages[visibleMessages.length - 1].name === currentUser && (

                    <div className="text-xs text-right mt-1 mr-2 dark:text-white text-black">

                        Delivered

                    </div>

                )}

        </div>

    );

};



export default Chat;
```
##### File: `src/registry/components/chat-messages/chat-message.tsx`
```tsx
"use client";



import { motion } from "motion/react";

import type { FC } from "react";

import Image from "next/image";



export interface Message {

    id: string;

    name: string;

    message: string;

    timestamp?: string;

}



interface ChatMessageProps {

    message: Message;

    isCurrentUser: boolean;

    avatar: string;

}



const ChatMessage: FC<ChatMessageProps> = ({

    message,

    isCurrentUser,

    avatar,

}) => {

    return (

        <motion.div

            layout

            initial={{ opacity: 0, y: 20 }}

            animate={{ opacity: 1, y: 0 }}

            exit={{ opacity: 0, y: 20 }}

            transition={{ duration: 0.4 }}

            className={`flex mb-4 ${isCurrentUser ? "justify-end" : "justify-start"

                } items-end`}

        >

            {!isCurrentUser && (

                <div className="flex-shrink-0 mr-2">

                    <div className="w-8 h-8 rounded-full overflow-hidden">

                        <Image

                            src={avatar || "/placeholder.svg"}

                            alt={`${message.name}'s avatar`}

                            width={32}

                            height={32}

                            className="object-cover"

                        />

                    </div>

                </div>

            )}



            <div

                className={`max-w-[70%] px-2 py-1 rounded-2xl ${isCurrentUser

                        ? "bg-blue-500 text-white rounded-br-none"

                        : "bg-gray-200 text-black rounded-bl-none"

                    }`}

            >

                <p className="text-sm sm:text-base">{message.message}</p>

                {message.timestamp && (

                    <p

                        className={`text-xs mt-1 ${isCurrentUser ? "text-blue-100" : "text-gray-500"

                            }`}

                    >

                        {message.timestamp}

                    </p>

                )}

            </div>



            {isCurrentUser && <div className="w-8 "></div>}

        </motion.div>

    );

};



export default ChatMessage;
```
##### File: `src/registry/components/chat-messages/blur-fade.tsx`
```tsx
// Component From Motion-Primitives



"use client";



import {

    AnimatePresence,

    motion,

    useInView,

    UseInViewOptions,

    Variants,

    MotionProps,

} from "motion/react";

import { useRef } from "react";



type MarginType = UseInViewOptions["margin"];



interface BlurFadeProps extends MotionProps {

    children: React.ReactNode;

    className?: string;

    variant?: {

        hidden: { y: number };

        visible: { y: number };

    };

    duration?: number;

    delay?: number;

    offset?: number;

    direction?: "up" | "down" | "left" | "right";

    inView?: boolean;

    inViewMargin?: MarginType;

    blur?: string;

}



export function BlurFade({

    children,

    className,

    variant,

    duration = 0.4,

    delay = 0,

    offset = 6,

    direction = "down",

    inView = false,

    inViewMargin = "-50px",

    blur = "6px",

    ...props

}: BlurFadeProps) {

    const ref = useRef(null);

    const inViewResult = useInView(ref, { once: true, margin: inViewMargin });

    const isInView = !inView || inViewResult;

    const defaultVariants: Variants = {

        hidden: {

            [direction === "left" || direction === "right" ? "x" : "y"]:

                direction === "right" || direction === "down" ? -offset : offset,

            opacity: 0,

            filter: `blur(${blur})`,

        },

        visible: {

            [direction === "left" || direction === "right" ? "x" : "y"]: 0,

            opacity: 1,

            filter: `blur(0px)`,

        },

    };

    const combinedVariants = variant || defaultVariants;

    return (

        <AnimatePresence>

            <motion.div

                ref={ref}

                initial="hidden"

                animate={isInView ? "visible" : "hidden"}

                exit="hidden"

                variants={combinedVariants}

                transition={{

                    delay: 0.04 + delay,

                    duration,

                    ease: "easeOut",

                }}

                className={className}

                {...props}

            >

                {children}

            </motion.div>

        </AnimatePresence>

    );

}
```
##### File: `src/registry/components/chat-messages/iphone-frame.tsx`
```tsx
import type { FC, ReactNode } from "react";

import {

    WifiIcon as LuWifi,

    BatteryMediumIcon as LuBatteryMedium,

    SignalIcon as LuSignal,

} from "lucide-react";



interface IphoneFrameProps {

    children: ReactNode;

    className?: string;

}



const IphoneFrame: FC<IphoneFrameProps> = ({ children, className = "" }) => {

    // Get current time in format HH:MM

    const getCurrentTime = () => {

        const now = new Date();

        const hours = now.getHours().toString().padStart(2, "0");

        const minutes = now.getMinutes().toString().padStart(2, "0");

        return `${hours}:${minutes}`;

    };



    return (

        // The main phone body

        <div

            className={`relative mx-auto border-[10px] border-gray-900 bg-[#212121] dark:border-gray-200 dark:bg-gray-100 rounded-[44px] shadow-xl overflow-hidden w-[300px] h-[600px] sm:w-[350px] sm:h-[700px] ${className}`}

        >

            {/* Status Bar */}

            <div className="absolute top-0 left-0 right-0 h-8 z-20 px-6 flex justify-between items-center text-black dark:text-white dark:bg-[#212121] bg-white">

                {/* Left side - Time */}

                <div className="text-xs font-semibold flex items-center">

                    <LuSignal className="w-3.5 h-3.5 mr-1" />

                    {getCurrentTime()}

                </div>



                {/* Notch (center) */}

                <div

                    className="absolute top-0 left-1/2 -translate-x-1/2 w-[45%] h-[24px] bg-black dark:bg-gray-100 rounded-b-xl z-30 flex justify-center items-center space-x-2 p-1"

                >

                    {/* Notch Details (Speaker/Camera) */}

                    <div

                        className="w-10 h-2 rounded-full bg-white dark:bg-black"></div>

                    <div

                        className="w-2 h-2 rounded-full bg-white dark:bg-black"></div>

                </div>



                {/* Right side - WiFi and Battery */}

                <div className="text-xs font-semibold flex items-center">

                    <LuWifi className="w-4 h-4 mr-1" />

                    <LuBatteryMedium className="w-5 h-5" />

                </div>

            </div>



            {/* The screen area */}

            <div

                className="w-full h-full (bg-white dark:bg-black) rounded-[34px] overflow-hidden pt-8"

            >

                {children}

            </div>



            {/* Side Buttons */}

            {/* Volume Up */}

            <div

                className="absolute -left-[12px] top-[100px] w-[4px] h-[50px] rounded-l-md bg-gray-800 dark:bg-gray-300"></div>

            {/* Volume Down */}

            <div

                className="absolute -left-[12px] top-[160px] w-[4px] h-[50px] rounded-l-md bg-gray-800 dark:bg-gray-300"></div>

            {/* Power/Side Button */}

            <div

                className="absolute -right-[12px] top-[130px] w-[4px] h-[80px] rounded-r-md bg-gray-800 dark:bg-gray-300"></div>

        </div>

    );

};



export default IphoneFrame;
```
---

## Chat Messages 2 <a name="chat-messages-2"></a>

Solace UI chat messages component variation 2.

- **Registry Key**: `chat-messages-2`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/chat-messages-2.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### Component Source Code
##### File: `src/registry/components/chat-messages/two/index.tsx`
```tsx
"use client";



import { useState } from "react";

import Chat, { type User } from "@/registry/components/chat-messages/chat";

import type { Message } from "@/registry/components/chat-messages/chat-message";

import IphoneFrame from "@/registry/components/chat-messages/iphone-frame";



export default function ChatMessagesReverse() {

    const users: User[] = [

        {

            name: "User1",

            avatar:

                "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png", // Replace with actual avatar URL

        },

        {

            name: "User2",

            avatar:

                "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-four.png", // Replace with actual avatar URL

        },

    ];



    const [messages] = useState<Message[]>([

        {

            id: "n1",

            name: "User1",

            message: "Hey, what is the best component library?",

        },

        {

            id: "n2",

            name: "User2",

            message: "I think Solace UI is awesome it is simple and easy to use.",

        },

        {

            id: "n3",

            name: "User1",

            message: "So, it is reliable and has a clean design?",

        },

        {

            id: "n4",

            name: "User2",

            message: "Yes, exactly. It makes development a breeze.",

        },

    ]);



    return (

        <main className="relative flex flex-col items-center justify-center p-4 ">

            {/* Clipping Wrapper */}

            <div className="mx-auto overflow-hidden w-[300px] h-[360px] sm:w-[350px] sm:h-[420px]">

                <IphoneFrame>

                    <Chat

                        messages={messages}

                        currentUser="User1"

                        users={users} // Pass the users array with avatars

                    />

                </IphoneFrame>





            </div>

            <div

                className="absolute bottom-0 left-0 right-0 z-50 h-24 sm:h-32 bg-linear-to-t from-background to-transparent pointer-events-none"

                aria-hidden="true"></div>

        </main>

    );

}
```
##### File: `src/registry/components/chat-messages/chat.tsx`
```tsx
"use client";



import { type FC, useRef, useEffect, useState, useLayoutEffect } from "react";

import { motion, AnimatePresence } from "motion/react";

import ChatMessage, { type Message } from "@/registry/components/chat-messages/chat-message";


export interface User {

    name: string;

    avatar: string;

}



interface ChatProps {

    messages: Message[];

    currentUser: string;

    users: User[];

}



const Chat: FC<ChatProps> = ({ messages, currentUser, users }) => {

    const containerRef = useRef<HTMLDivElement>(null);

    const [visibleMessages, setVisibleMessages] = useState<Message[]>([]);



    useEffect(() => {

        if (messages.length === 0) return;



        setVisibleMessages([]);

        let cancelled = false;



        (async () => {

            for (const msg of messages) {

                if (cancelled) return; // guard before delay

                await new Promise((r) => setTimeout(r, 700));

                if (cancelled) return; // guard after delay

                setVisibleMessages((prev) => [...prev, msg]);

            }

        })();



        return () => {

            cancelled = true;

        };

    }, [messages]);



    useLayoutEffect(() => {

        const el = containerRef.current;

        if (el) el.scrollTop = el.scrollHeight;

    }, [visibleMessages]);



    const getUserAvatar = (name: string) =>

        users.find((u) => u.name === name)?.avatar ??

        "/placeholder.svg?height=40&width=40";



    return (

        <div

            ref={containerRef}

            className="w-full h-full overflow-y-auto p-2 bg-white dark:bg-[#212121] flex flex-col py-[2rem]"

        >

            <AnimatePresence initial={false} mode="popLayout">

                {visibleMessages.map((message) => (

                    <motion.div

                        key={message.id}

                        layout

                        initial={{ opacity: 0, y: 20 }}

                        animate={{ opacity: 1, y: 0 }}

                        exit={{ opacity: 0, y: 20 }}

                        transition={{ duration: 0.4 }}

                        className={`flex mb-4 ${message.name === currentUser ? "justify-end" : "justify-start"

                            } items-end`}

                    >

                        <ChatMessage

                            message={message}

                            isCurrentUser={message.name === currentUser}

                            avatar={getUserAvatar(message.name)}

                        />

                    </motion.div>

                ))}

            </AnimatePresence>



            {visibleMessages.length > 0 &&

                visibleMessages[visibleMessages.length - 1].name === currentUser && (

                    <div className="text-xs text-right mt-1 mr-2 dark:text-white text-black">

                        Delivered

                    </div>

                )}

        </div>

    );

};



export default Chat;
```
##### File: `src/registry/components/chat-messages/chat-message.tsx`
```tsx
"use client";



import { motion } from "motion/react";

import type { FC } from "react";

import Image from "next/image";



export interface Message {

    id: string;

    name: string;

    message: string;

    timestamp?: string;

}



interface ChatMessageProps {

    message: Message;

    isCurrentUser: boolean;

    avatar: string;

}



const ChatMessage: FC<ChatMessageProps> = ({

    message,

    isCurrentUser,

    avatar,

}) => {

    return (

        <motion.div

            layout

            initial={{ opacity: 0, y: 20 }}

            animate={{ opacity: 1, y: 0 }}

            exit={{ opacity: 0, y: 20 }}

            transition={{ duration: 0.4 }}

            className={`flex mb-4 ${isCurrentUser ? "justify-end" : "justify-start"

                } items-end`}

        >

            {!isCurrentUser && (

                <div className="flex-shrink-0 mr-2">

                    <div className="w-8 h-8 rounded-full overflow-hidden">

                        <Image

                            src={avatar || "/placeholder.svg"}

                            alt={`${message.name}'s avatar`}

                            width={32}

                            height={32}

                            className="object-cover"

                        />

                    </div>

                </div>

            )}



            <div

                className={`max-w-[70%] px-2 py-1 rounded-2xl ${isCurrentUser

                        ? "bg-blue-500 text-white rounded-br-none"

                        : "bg-gray-200 text-black rounded-bl-none"

                    }`}

            >

                <p className="text-sm sm:text-base">{message.message}</p>

                {message.timestamp && (

                    <p

                        className={`text-xs mt-1 ${isCurrentUser ? "text-blue-100" : "text-gray-500"

                            }`}

                    >

                        {message.timestamp}

                    </p>

                )}

            </div>



            {isCurrentUser && <div className="w-8 "></div>}

        </motion.div>

    );

};



export default ChatMessage;
```
##### File: `src/registry/components/chat-messages/blur-fade.tsx`
```tsx
// Component From Motion-Primitives



"use client";



import {

    AnimatePresence,

    motion,

    useInView,

    UseInViewOptions,

    Variants,

    MotionProps,

} from "motion/react";

import { useRef } from "react";



type MarginType = UseInViewOptions["margin"];



interface BlurFadeProps extends MotionProps {

    children: React.ReactNode;

    className?: string;

    variant?: {

        hidden: { y: number };

        visible: { y: number };

    };

    duration?: number;

    delay?: number;

    offset?: number;

    direction?: "up" | "down" | "left" | "right";

    inView?: boolean;

    inViewMargin?: MarginType;

    blur?: string;

}



export function BlurFade({

    children,

    className,

    variant,

    duration = 0.4,

    delay = 0,

    offset = 6,

    direction = "down",

    inView = false,

    inViewMargin = "-50px",

    blur = "6px",

    ...props

}: BlurFadeProps) {

    const ref = useRef(null);

    const inViewResult = useInView(ref, { once: true, margin: inViewMargin });

    const isInView = !inView || inViewResult;

    const defaultVariants: Variants = {

        hidden: {

            [direction === "left" || direction === "right" ? "x" : "y"]:

                direction === "right" || direction === "down" ? -offset : offset,

            opacity: 0,

            filter: `blur(${blur})`,

        },

        visible: {

            [direction === "left" || direction === "right" ? "x" : "y"]: 0,

            opacity: 1,

            filter: `blur(0px)`,

        },

    };

    const combinedVariants = variant || defaultVariants;

    return (

        <AnimatePresence>

            <motion.div

                ref={ref}

                initial="hidden"

                animate={isInView ? "visible" : "hidden"}

                exit="hidden"

                variants={combinedVariants}

                transition={{

                    delay: 0.04 + delay,

                    duration,

                    ease: "easeOut",

                }}

                className={className}

                {...props}

            >

                {children}

            </motion.div>

        </AnimatePresence>

    );

}
```
##### File: `src/registry/components/chat-messages/iphone-frame.tsx`
```tsx
import type { FC, ReactNode } from "react";

import {

    WifiIcon as LuWifi,

    BatteryMediumIcon as LuBatteryMedium,

    SignalIcon as LuSignal,

} from "lucide-react";



interface IphoneFrameProps {

    children: ReactNode;

    className?: string;

}



const IphoneFrame: FC<IphoneFrameProps> = ({ children, className = "" }) => {

    // Get current time in format HH:MM

    const getCurrentTime = () => {

        const now = new Date();

        const hours = now.getHours().toString().padStart(2, "0");

        const minutes = now.getMinutes().toString().padStart(2, "0");

        return `${hours}:${minutes}`;

    };



    return (

        // The main phone body

        <div

            className={`relative mx-auto border-[10px] border-gray-900 bg-[#212121] dark:border-gray-200 dark:bg-gray-100 rounded-[44px] shadow-xl overflow-hidden w-[300px] h-[600px] sm:w-[350px] sm:h-[700px] ${className}`}

        >

            {/* Status Bar */}

            <div className="absolute top-0 left-0 right-0 h-8 z-20 px-6 flex justify-between items-center text-black dark:text-white dark:bg-[#212121] bg-white">

                {/* Left side - Time */}

                <div className="text-xs font-semibold flex items-center">

                    <LuSignal className="w-3.5 h-3.5 mr-1" />

                    {getCurrentTime()}

                </div>



                {/* Notch (center) */}

                <div

                    className="absolute top-0 left-1/2 -translate-x-1/2 w-[45%] h-[24px] bg-black dark:bg-gray-100 rounded-b-xl z-30 flex justify-center items-center space-x-2 p-1"

                >

                    {/* Notch Details (Speaker/Camera) */}

                    <div

                        className="w-10 h-2 rounded-full bg-white dark:bg-black"></div>

                    <div

                        className="w-2 h-2 rounded-full bg-white dark:bg-black"></div>

                </div>



                {/* Right side - WiFi and Battery */}

                <div className="text-xs font-semibold flex items-center">

                    <LuWifi className="w-4 h-4 mr-1" />

                    <LuBatteryMedium className="w-5 h-5" />

                </div>

            </div>



            {/* The screen area */}

            <div

                className="w-full h-full (bg-white dark:bg-black) rounded-[34px] overflow-hidden pt-8"

            >

                {children}

            </div>



            {/* Side Buttons */}

            {/* Volume Up */}

            <div

                className="absolute -left-[12px] top-[100px] w-[4px] h-[50px] rounded-l-md bg-gray-800 dark:bg-gray-300"></div>

            {/* Volume Down */}

            <div

                className="absolute -left-[12px] top-[160px] w-[4px] h-[50px] rounded-l-md bg-gray-800 dark:bg-gray-300"></div>

            {/* Power/Side Button */}

            <div

                className="absolute -right-[12px] top-[130px] w-[4px] h-[80px] rounded-r-md bg-gray-800 dark:bg-gray-300"></div>

        </div>

    );

};



export default IphoneFrame;
```
---

## Footer Section 1 <a name="footer-section-1"></a>

Solace UI footer section block variation 1.

- **Registry Key**: `footer-section-1`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/footer-section-1.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/footer-section/one/index.tsx`
```tsx
"use client";

import React from "react";

import { SocialCloud } from "@/registry/blocks/footer-section/social-cloud";

import { motion, Variants } from "motion/react";



const SolaceUILogo = ({ className }: { className?: string }) => {

  return (

    <svg className={className} width="64" height="38" viewBox="0 0 64 38" fill="none" xmlns="http://www.w3.org/2000/svg">

      <path d="M0 20.1032L39.8387 20.1032C44.7808 20.1032 48.7871 24.1095 48.7871 29.0516C48.7871 33.9937 44.7808 38 39.8387 38L1.56459e-06 38L0 20.1032Z" fill="currentColor" />

      <path d="M63.4968 17.8968L23.6581 17.8968C18.716 17.8968 14.7097 13.8904 14.7097 8.94839C14.7097 4.00633 18.716 0 23.6581 0L63.4968 0V17.8968Z" fill="currentColor" />

    </svg>

  )

}





export default function Footer1() {

  const containerVariants = {

    hidden: { opacity: 0 },

    visible: {

      opacity: 1,

      transition: {

        staggerChildren: 0.1,

        delayChildren: 0.1,

      },

    },

  };



  const itemVariants: Variants = {

    hidden: { opacity: 0, y: 20 },

    visible: {

      opacity: 1,

      y: 0,

      transition: {

        type: "spring",

        stiffness: 260,

        damping: 20,

      },

    },

  };



  const dividerVariants: Variants = {

    hidden: { opacity: 0 },

    visible: { opacity: 1, transition: { duration: 0.5, delay: 0.5 } }, // Appears after a delay

  };



  return (

    <footer className="w-full py-12 bg-(--color-background) dark:bg-(--color-background) text-black dark:text-white overflow-hidden">

      <motion.div

        initial="hidden"

        whileInView="visible"

        viewport={{ once: true, margin: "0px 0px -100px 0px" }}

        variants={containerVariants}

        className="container mx-auto px-4 flex flex-col items-center gap-10 mb-12"

      >

        {/* Logo */}

        <motion.div variants={itemVariants} className="flex justify-center">

          <SolaceUILogo className="h-10 w-auto" />

        </motion.div>



        {/* Navigation Links */}

        <motion.nav

          variants={itemVariants}

          className="flex flex-wrap justify-center gap-x-8 gap-y-4 text-base font-medium relative z-10"

        >

          {["Products", "Solution", "Company", "About", "Blog", "Terms"].map(

            (item) => (

              <motion.a

                key={item}

                href="#"

                className="relative px-2 py-1 group"

                whileHover={{ scale: 1.05 }}

                whileTap={{ scale: 0.95 }}

              >

                <span className="relative z-10 group-hover:text-black dark:group-hover:text-white transition-colors duration-300">

                  {item}

                </span>

                <motion.span

                  className="absolute inset-0 bg-(--color-background) dark:bg-(--color-background) rounded-md -z-0 origin-center"

                  initial={{ scale: 0, opacity: 0 }}

                  whileHover={{ scale: 1, opacity: 1 }}

                  transition={{ type: "spring", stiffness: 300, damping: 20 }}

                />

              </motion.a>

            ),

          )}

        </motion.nav>



        {/* Social Media Icons */}

        <motion.div variants={itemVariants}>

          <SocialCloud className="text-black dark:text-white" />

        </motion.div>

      </motion.div>



      {/* Divider */}

      <motion.div

        className="w-full h-12 border-y border-black dark:border-white opacity-10 bg-[repeating-linear-gradient(315deg,currentColor_0,currentColor_1px,transparent_0,transparent_50%)]"

        style={{ backgroundSize: "10px 10px" }}

        initial={{ backgroundPositionX: "0%" }}

        whileInView={{ backgroundPositionX: "100%" }}

        viewport={{ once: true }}

        transition={{

          ease: "linear",

          duration: 20,

        }}

      />



      {/* Copyright */}

      <motion.div

        className="container mx-auto px-4 mt-8 text-center text-sm text-neutral-400 dark:text-neutral-600"

        initial="hidden"

        whileInView="visible"

        viewport={{ once: true }}

        variants={itemVariants} // Re-trigger for copyright or just use simple fade

      >

        <p>&copy; {new Date().getFullYear()} SolaceUI, All rights reserved</p>

      </motion.div>

    </footer>

  );

}
```
##### File: `src/registry/blocks/footer-section/social-cloud.tsx`
```tsx
"use client";

import React from "react";

import { cn } from "@/lib/utils";

import { motion, MotionProps } from "motion/react";



interface SocialIconProps extends React.SVGProps<SVGSVGElement> {}



export const XIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="23"

    height="20"

    viewBox="0 0 23 20"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M9.46617 12.6219L14.625 19.5H22.2083L13.6955 8.14883L20.7783 0H17.9075L12.3641 6.3765L7.58333 0H0L8.13583 10.8496L0.6175 19.5H3.48833L9.46617 12.6219ZM15.7083 17.3333L4.33333 2.16667H6.5L17.875 17.3333H15.7083Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const LinkedInIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="23"

    height="23"

    viewBox="0 0 23 23"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M20 0C20.663 0 21.2989 0.263392 21.7678 0.732233C22.2366 1.20107 22.5 1.83696 22.5 2.5V20C22.5 20.663 22.2366 21.2989 21.7678 21.7678C21.2989 22.2366 20.663 22.5 20 22.5H2.5C1.83696 22.5 1.20107 22.2366 0.732233 21.7678C0.263392 21.2989 0 20.663 0 20V2.5C0 1.83696 0.263392 1.20107 0.732233 0.732233C1.20107 0.263392 1.83696 0 2.5 0H20ZM19.375 19.375V12.75C19.375 11.6692 18.9457 10.6328 18.1815 9.86854C17.4172 9.10433 16.3808 8.675 15.3 8.675C14.2375 8.675 13 9.325 12.4 10.3V8.9125H8.9125V19.375H12.4V13.2125C12.4 12.25 13.175 11.4625 14.1375 11.4625C14.6016 11.4625 15.0467 11.6469 15.3749 11.9751C15.7031 12.3033 15.8875 12.7484 15.8875 13.2125V19.375H19.375ZM4.85 6.95C5.40695 6.95 5.9411 6.72875 6.33492 6.33492C6.72875 5.9411 6.95 5.40695 6.95 4.85C6.95 3.6875 6.0125 2.7375 4.85 2.7375C4.28973 2.7375 3.75241 2.96007 3.35624 3.35624C2.96007 3.75241 2.7375 4.28973 2.7375 4.85C2.7375 6.0125 3.6875 6.95 4.85 6.95ZM6.5875 19.375V8.9125H3.125V19.375H6.5875Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const FacebookIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="29"

    height="29"

    viewBox="0 0 29 29"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M28.3333 14.1667C28.3333 6.34667 21.9867 0 14.1667 0C6.34667 0 0 6.34667 0 14.1667C0 21.0233 4.87333 26.7325 11.3333 28.05V18.4167H8.5V14.1667H11.3333V10.625C11.3333 7.89083 13.5575 5.66667 16.2917 5.66667H19.8333V9.91667H17C16.2208 9.91667 15.5833 10.5542 15.5833 11.3333V14.1667H19.8333V18.4167H15.5833V28.2625C22.7375 27.5542 28.3333 21.5192 28.3333 14.1667Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const ThreadsIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="18"

    height="22"

    viewBox="0 0 18 22"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M17.4167 6.92196C15.5768 0.0771293 9.25 0.505296 9.25 0.505296C9.25 0.505296 0.5 -0.0780373 0.5 10.9995C0.5 22.077 9.25 21.4948 9.25 21.4948C9.25 21.4948 14.451 21.8401 16.8333 16.9238C17.6115 14.7561 17.4167 10.422 9.83333 10.422C9.83333 10.422 6.33333 10.422 6.33333 13.3386C6.33333 14.4773 7.5 15.672 9.25 15.672C11 15.672 12.9495 14.4738 13.3333 12.172C14.5 5.17196 8.08333 4.58863 6.33333 7.5053"

      stroke="currentColor"

      strokeLinecap="round"

      strokeLinejoin="round"

    />

  </motion.svg>

);



export const InstagramIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="24"

    height="24"

    viewBox="0 0 24 24"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M6.76667 0H16.5667C20.3 0 23.3333 3.03333 23.3333 6.76667V16.5667C23.3333 18.3613 22.6204 20.0824 21.3514 21.3514C20.0824 22.6204 18.3613 23.3333 16.5667 23.3333H6.76667C3.03333 23.3333 0 20.3 0 16.5667V6.76667C0 4.97204 0.712915 3.25091 1.98191 1.98191C3.25091 0.712915 4.97204 0 6.76667 0ZM6.53333 2.33333C5.41942 2.33333 4.35114 2.77583 3.56349 3.56349C2.77583 4.35114 2.33333 5.41942 2.33333 6.53333V16.8C2.33333 19.1217 4.21167 21 6.53333 21H16.8C17.9139 21 18.9822 20.5575 19.7699 19.7699C20.5575 18.9822 21 17.9139 21 16.8V6.53333C21 4.21167 19.1217 2.33333 16.8 2.33333H6.53333ZM17.7917 4.08333C18.1784 4.08333 18.5494 4.23698 18.8229 4.51047C19.0964 4.78396 19.25 5.15489 19.25 5.54167C19.25 5.92844 19.0964 6.29937 18.8229 6.57286C18.5494 6.84635 18.1784 7 17.7917 7C17.4049 7 17.034 6.84635 16.7605 6.57286C16.487 6.29937 16.3333 5.92844 16.3333 5.54167C16.3333 5.15489 16.487 4.78396 16.7605 4.51047C17.034 4.23698 17.4049 4.08333 17.7917 4.08333ZM11.6667 5.83333C13.2138 5.83333 14.6975 6.44792 15.7915 7.54188C16.8854 8.63584 17.5 10.1196 17.5 11.6667C17.5 13.2138 16.8854 14.6975 15.7915 15.7915C14.6975 16.8854 13.2138 17.5 11.6667 17.5C10.1196 17.5 8.63584 16.8854 7.54188 15.7915C6.44792 14.6975 5.83333 13.2138 5.83333 11.6667C5.83333 10.1196 6.44792 8.63584 7.54188 7.54188C8.63584 6.44792 10.1196 5.83333 11.6667 5.83333ZM11.6667 8.16667C10.7384 8.16667 9.84817 8.53542 9.19179 9.19179C8.53542 9.84817 8.16667 10.7384 8.16667 11.6667C8.16667 12.5949 8.53542 13.4852 9.19179 14.1415C9.84817 14.7979 10.7384 15.1667 11.6667 15.1667C12.5949 15.1667 13.4852 14.7979 14.1415 14.1415C14.7979 13.4852 15.1667 12.5949 15.1667 11.6667C15.1667 10.7384 14.7979 9.84817 14.1415 9.19179C13.4852 8.53542 12.5949 8.16667 11.6667 8.16667Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const TikTokIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="19"

    height="21"

    viewBox="0 0 19 21"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M14.5133 3.29C13.716 2.37945 13.2765 1.2103 13.2767 0H9.67167V14.4667C9.64444 15.2497 9.31411 15.9916 8.75036 16.5357C8.18661 17.0799 7.43353 17.3838 6.65 17.3833C4.99333 17.3833 3.61667 16.03 3.61667 14.35C3.61667 12.3433 5.55333 10.8383 7.54833 11.4567V7.77C3.52333 7.23333 0 10.36 0 14.35C0 18.235 3.22 21 6.63833 21C10.3017 21 13.2767 18.025 13.2767 14.35V7.01167C14.7385 8.06149 16.4936 8.62475 18.2933 8.62167V5.01667C18.2933 5.01667 16.1 5.12167 14.5133 3.29Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const YouTubeIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="29"

    height="21"

    viewBox="0 0 29 21"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M28.0501 3.13163C27.7206 1.8991 26.7497 0.928222 25.5172 0.59876C23.2832 -1.51787e-07 14.3244 0 14.3244 0C14.3244 0 5.36561 -1.51787e-07 3.13164 0.59876C1.8991 0.928222 0.928222 1.8991 0.59876 3.13163C0 5.36561 0 10.0271 0 10.0271C0 10.0271 0 14.6886 0.59876 16.9225C0.928222 18.1551 1.8991 19.126 3.13164 19.4554C5.36561 20.0542 14.3244 20.0542 14.3244 20.0542C14.3244 20.0542 23.2832 20.0542 25.5172 19.4554C26.7497 19.126 27.7206 18.1551 28.0501 16.9225C28.6488 14.6886 28.6488 10.0271 28.6488 10.0271C28.6488 10.0271 28.6488 5.36561 28.0501 3.13163ZM11.4595 14.3244V5.72977L18.9025 10.0271L11.4595 14.3244Z"

      fill="currentColor"

    />

  </motion.svg>

);



interface SocialCloudProps

  extends

    Omit<React.HTMLAttributes<HTMLDivElement>, keyof MotionProps>,

    MotionProps {}



export function SocialCloud({ className, ...props }: SocialCloudProps) {

  return (

    <motion.div

      className={cn(

        "flex flex-wrap items-center gap-6 cursor-pointer",

        className,

      )}

      {...props}

    >

      <XIcon />

      <LinkedInIcon />

      <FacebookIcon />

      <ThreadsIcon />

      <InstagramIcon />

      <TikTokIcon />

      <YouTubeIcon />

    </motion.div>

  );

}
```
---

## Footer Section 2 <a name="footer-section-2"></a>

Solace UI footer section block variation 2.

- **Registry Key**: `footer-section-2`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/footer-section-2.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/footer-section/two/index.tsx`
```tsx
"use client";

import React from "react";

import Link from "next/link";

import { SocialCloud } from "@/registry/blocks/footer-section/social-cloud";
import Image from "next/image";

import { motion } from "motion/react";



const downloadGoogleDark = () => (

  <svg

    width="120"

    height="40"

    viewBox="0 0 120 40"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

  >

    <rect x="0.5" y="0.5" width="119" height="39" rx="5.5" fill="black" />

    <rect x="0.5" y="0.5" width="119" height="39" rx="5.5" stroke="#A6A6A6" />

    <path

      d="M17.8048 19.4617L8.0896 30.0059C8.09051 30.0078 8.09051 30.0106 8.09142 30.0125C8.38981 31.1574 9.41179 32 10.6254 32C11.1108 32 11.5662 31.8656 11.9567 31.6305L11.9877 31.6118L22.9229 25.1593L17.8048 19.4617Z"

      fill="#EA4335"

    />

    <path

      d="M27.6331 17.6662L27.624 17.6597L22.9028 14.8612L17.5839 19.7013L22.9219 25.1582L27.6176 22.3878C28.4406 21.9324 29 21.045 29 20.0223C29 19.0052 28.4489 18.1225 27.6331 17.6662Z"

      fill="#FBBC04"

    />

    <path

      d="M8.08942 9.99331C8.03102 10.2135 8 10.4449 8 10.6838V29.3163C8 29.5552 8.03102 29.7866 8.09034 30.0059L18.1386 19.7313L8.08942 9.99331Z"

      fill="#4285F4"

    />

    <path

      d="M17.8766 19.9999L22.9044 14.8594L11.9819 8.38351C11.585 8.13996 11.1214 8 10.626 8C9.41237 8 8.38856 8.84447 8.09018 9.99034C8.09018 9.99127 8.08926 9.9922 8.08926 9.99314L17.8766 19.9999Z"

      fill="#34A853"

    />

    <path

      d="M43.61 11.71C43.61 12.71 43.3133 13.5067 42.72 14.1C42.0533 14.8067 41.1767 15.16 40.09 15.16C39.05 15.16 38.17 14.8 37.45 14.08C36.73 13.36 36.37 12.4733 36.37 11.42C36.37 10.3667 36.73 9.48 37.45 8.76C38.17 8.04 39.05 7.68 40.09 7.68C40.6167 7.68 41.1133 7.77333 41.58 7.96C42.0467 8.14667 42.43 8.41 42.73 8.75L42.07 9.41C41.85 9.14333 41.5633 8.93667 41.21 8.79C40.8633 8.63667 40.49 8.56 40.09 8.56C39.31 8.56 38.65 8.83 38.11 9.37C37.5767 9.91667 37.31 10.6 37.31 11.42C37.31 12.24 37.5767 12.9233 38.11 13.47C38.65 14.01 39.31 14.28 40.09 14.28C40.8033 14.28 41.3967 14.08 41.87 13.68C42.3433 13.28 42.6167 12.73 42.69 12.03H40.09V11.17H43.56C43.5933 11.3567 43.61 11.5367 43.61 11.71ZM48.9078 7.84V8.72H45.6478V10.99H48.5878V11.85H45.6478V14.12H48.9078V15H44.7278V7.84H48.9078ZM52.5877 8.72V15H51.6677V8.72H49.6677V7.84H54.5877V8.72H52.5877ZM58.6654 15H57.7454V7.84H58.6654V15ZM62.5487 8.72V15H61.6287V8.72H59.6287V7.84H64.5487V8.72H62.5487ZM74.521 11.42C74.521 12.48 74.1677 13.3667 73.461 14.08C72.7477 14.8 71.8743 15.16 70.841 15.16C69.801 15.16 68.9277 14.8 68.221 14.08C67.5143 13.3667 67.161 12.48 67.161 11.42C67.161 10.36 67.5143 9.47333 68.221 8.76C68.9277 8.04 69.801 7.68 70.841 7.68C71.881 7.68 72.7543 8.04333 73.461 8.77C74.1677 9.48333 74.521 10.3667 74.521 11.42ZM68.101 11.42C68.101 12.2467 68.361 12.93 68.881 13.47C69.4077 14.01 70.061 14.28 70.841 14.28C71.621 14.28 72.271 14.01 72.791 13.47C73.3177 12.9367 73.581 12.2533 73.581 11.42C73.581 10.5867 73.3177 9.90333 72.791 9.37C72.271 8.83 71.621 8.56 70.841 8.56C70.061 8.56 69.4077 8.83 68.881 9.37C68.361 9.91 68.101 10.5933 68.101 11.42ZM76.5267 15H75.6067V7.84H76.7267L80.2067 13.41H80.2467L80.2067 12.03V7.84H81.1267V15H80.1667L76.5267 9.16H76.4867L76.5267 10.54V15Z"

      fill="white"

    />

    <path

      d="M93.5181 31.4097H95.1469V20.3981H93.5181V31.4097ZM108.189 24.3646L106.322 29.1388H106.266L104.328 24.3646H102.573L105.479 31.0371L103.823 34.749H105.521L110 24.3646H108.189ZM98.9519 30.1588C98.4176 30.1588 97.6739 29.8902 97.6739 29.2234C97.6739 28.3742 98.6001 28.0483 99.4005 28.0483C100.116 28.0483 100.454 28.2042 100.889 28.4165C100.762 29.4365 99.892 30.1588 98.9519 30.1588ZM99.1483 24.1241C97.969 24.1241 96.7469 24.6482 96.2424 25.8101L97.6879 26.4188C97.9969 25.8101 98.5721 25.611 99.1762 25.611C100.019 25.611 100.875 26.121 100.889 27.0283V27.1411C100.594 26.971 99.9627 26.7165 99.1902 26.7165C97.632 26.7165 96.0451 27.5806 96.0451 29.1952C96.0451 30.6689 97.323 31.6184 98.7546 31.6184C99.8501 31.6184 100.454 31.1225 100.833 30.5411H100.889V31.3912H102.461V27.1692C102.461 25.2146 101.015 24.1241 99.1483 24.1241ZM89.0821 25.7053H86.7655V21.9308H89.0821C90.2998 21.9308 90.9911 22.9482 90.9911 23.8176C90.9911 24.6711 90.2998 25.7053 89.0821 25.7053ZM89.0402 20.3981H85.1375V31.4097H86.7655V27.2379H89.0402C90.8453 27.2379 92.6199 25.9184 92.6199 23.8176C92.6199 21.7168 90.8453 20.3981 89.0402 20.3981ZM67.7583 30.1606C66.6332 30.1606 65.6913 29.2102 65.6913 27.9047C65.6913 26.5852 66.6332 25.6198 67.7583 25.6198C68.8695 25.6198 69.7406 26.5852 69.7406 27.9047C69.7406 29.2102 68.8695 30.1606 67.7583 30.1606ZM69.6289 24.9812H69.5722C69.2064 24.5417 68.5038 24.1444 67.6178 24.1444C65.7611 24.1444 64.0599 25.7898 64.0599 27.9047C64.0599 30.0047 65.7611 31.6369 67.6178 31.6369C68.5038 31.6369 69.2064 31.2396 69.5722 30.7851H69.6289V31.3251C69.6289 32.7582 68.8695 33.5246 67.6457 33.5246C66.6471 33.5246 66.0282 32.8005 65.7751 32.1901L64.3549 32.7864C64.7626 33.78 65.8458 35 67.6457 35C69.5582 35 71.1757 33.8646 71.1757 31.0978V24.3708H69.6289V24.9812ZM72.3008 31.4097H73.9323V20.3973H72.3008V31.4097ZM76.3362 27.777C76.2943 26.3298 77.4474 25.5916 78.2766 25.5916C78.9243 25.5916 79.4725 25.9176 79.6549 26.3862L76.3362 27.777ZM81.3989 26.528C81.0899 25.6912 80.1472 24.1444 78.2208 24.1444C76.3083 24.1444 74.7196 25.6621 74.7196 27.8907C74.7196 29.9906 76.2943 31.6369 78.4032 31.6369C80.1053 31.6369 81.0899 30.5869 81.4976 29.9765L80.2319 29.1247C79.8103 29.7493 79.2333 30.1606 78.4032 30.1606C77.5739 30.1606 76.983 29.7774 76.6033 29.0261L81.5674 26.9534L81.3989 26.528ZM41.8501 25.2939V26.883H45.6184C45.5058 27.777 45.2107 28.4297 44.7612 28.8834C44.2121 29.4374 43.3541 30.0479 41.8501 30.0479C39.5291 30.0479 37.7152 28.1602 37.7152 25.8189C37.7152 23.4767 39.5291 21.5899 41.8501 21.5899C43.1018 21.5899 44.0157 22.0867 44.6905 22.7254L45.8017 21.604C44.8589 20.6959 43.6081 20 41.8501 20C38.6719 20 36 22.6117 36 25.8189C36 29.0261 38.6719 31.6369 41.8501 31.6369C43.5653 31.6369 44.8589 31.0688 45.8715 30.0047C46.9129 28.9547 47.2358 27.4793 47.2358 26.2866C47.2358 25.9176 47.2079 25.5775 47.1512 25.2939H41.8501ZM51.5208 30.1606C50.3957 30.1606 49.425 29.2243 49.425 27.8907C49.425 26.5421 50.3957 25.6198 51.5208 25.6198C52.6451 25.6198 53.6158 26.5421 53.6158 27.8907C53.6158 29.2243 52.6451 30.1606 51.5208 30.1606ZM51.5208 24.1444C49.4669 24.1444 47.7936 25.7194 47.7936 27.8907C47.7936 30.0479 49.4669 31.6369 51.5208 31.6369C53.5739 31.6369 55.2472 30.0479 55.2472 27.8907C55.2472 25.7194 53.5739 24.1444 51.5208 24.1444ZM59.65 30.1606C58.5249 30.1606 57.5542 29.2243 57.5542 27.8907C57.5542 26.5421 58.5249 25.6198 59.65 25.6198C60.7752 25.6198 61.745 26.5421 61.745 27.8907C61.745 29.2243 60.7752 30.1606 59.65 30.1606ZM59.65 24.1444C57.597 24.1444 55.9237 25.7194 55.9237 27.8907C55.9237 30.0479 57.597 31.6369 59.65 31.6369C61.7031 31.6369 63.3764 30.0479 63.3764 27.8907C63.3764 25.7194 61.7031 24.1444 59.65 24.1444Z"

      fill="white"

    />

  </svg>

);

const downloadAppleDark = () => (

  <svg

    width="120"

    height="40"

    viewBox="0 0 120 40"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

  >

    <rect x="0.5" y="0.5" width="119" height="39" rx="5.5" fill="black" />

    <rect x="0.5" y="0.5" width="119" height="39" rx="5.5" stroke="#A6A6A6" />

    <path

      d="M24.7045 20.7631C24.7166 19.8432 24.9669 18.9413 25.4321 18.1412C25.8972 17.3411 26.5621 16.6688 27.3648 16.187C26.8548 15.476 26.1821 14.8908 25.4 14.478C24.6178 14.0652 23.7479 13.8361 22.8592 13.809C20.9635 13.6147 19.1258 14.9164 18.1598 14.9164C17.1751 14.9164 15.6878 13.8283 14.0862 13.8604C13.0502 13.8931 12.0406 14.1872 11.1557 14.7141C10.2708 15.241 9.54075 15.9827 9.03674 16.8669C6.85352 20.5573 8.48201 25.9809 10.5734 28.964C11.6197 30.4247 12.8426 32.0564 14.4428 31.9985C16.0086 31.9351 16.5934 31.0237 18.4835 31.0237C20.3561 31.0237 20.9048 31.9985 22.5374 31.9617C24.2176 31.9351 25.2762 30.4945 26.2859 29.02C27.0377 27.9792 27.6162 26.8288 28 25.6116C27.0238 25.2085 26.1908 24.5338 25.6048 23.6716C25.0187 22.8094 24.7056 21.7979 24.7045 20.7631Z"

      fill="white"

    />

    <path

      d="M21.6208 11.8471C22.5369 10.7734 22.9883 9.39335 22.879 8C21.4794 8.14352 20.1865 8.7966 19.258 9.82911C18.804 10.3335 18.4563 10.9203 18.2348 11.556C18.0132 12.1917 17.9222 12.8638 17.9669 13.5338C18.6669 13.5408 19.3595 13.3927 19.9924 13.1005C20.6254 12.8084 21.1821 12.3798 21.6208 11.8471Z"

      fill="white"

    />

    <path

      d="M36.791 8.50146H38.8608C40.6494 8.50146 41.5195 9.56494 41.5195 11.4941C41.5195 13.4233 40.6406 14.5 38.8608 14.5H36.791V8.50146ZM37.7886 9.34082V13.6606H38.751C39.9375 13.6606 40.4956 12.9443 40.4956 11.5073C40.4956 10.0615 39.9331 9.34082 38.751 9.34082H37.7886ZM44.6748 9.77588C45.8877 9.77588 46.7358 10.5625 46.7358 11.8677V12.4697C46.7358 13.8188 45.8877 14.5791 44.6748 14.5791C43.4443 14.5791 42.605 13.8276 42.605 12.4741V11.8721C42.605 10.6021 43.4575 9.77588 44.6748 9.77588ZM44.6792 10.5625C43.9849 10.5625 43.5894 11.1426 43.5894 11.9204V12.439C43.5894 13.2168 43.9585 13.7925 44.6792 13.7925C45.3911 13.7925 45.7559 13.2212 45.7559 12.439V11.9204C45.7559 11.1426 45.3735 10.5625 44.6792 10.5625ZM53.8989 9.85498L52.6772 14.5H51.6841L50.7129 11.0723H50.6865L49.7329 14.5H48.7354L47.4609 9.85498H48.5112L49.2583 13.397H49.3022L50.2383 9.85498H51.1567L52.1191 13.397H52.1631L52.9233 9.85498H53.8989ZM54.8657 14.5V9.85498H55.8237V10.6899H55.8721C55.9907 10.3252 56.3291 9.78467 57.2695 9.78467C58.2056 9.78467 58.834 10.3032 58.834 11.3623V14.5H57.8584V11.6479C57.8584 10.9404 57.4893 10.6152 56.9399 10.6152C56.2192 10.6152 55.8413 11.1689 55.8413 11.9204V14.5H54.8657ZM60.3105 14.5V8.18506H61.2861V14.5H60.3105ZM64.6348 9.77588C65.8477 9.77588 66.6958 10.5625 66.6958 11.8677V12.4697C66.6958 13.8188 65.8477 14.5791 64.6348 14.5791C63.4043 14.5791 62.5649 13.8276 62.5649 12.4741V11.8721C62.5649 10.6021 63.4175 9.77588 64.6348 9.77588ZM64.6392 10.5625C63.9448 10.5625 63.5493 11.1426 63.5493 11.9204V12.439C63.5493 13.2168 63.9185 13.7925 64.6392 13.7925C65.3511 13.7925 65.7158 13.2212 65.7158 12.439V11.9204C65.7158 11.1426 65.3335 10.5625 64.6392 10.5625ZM69.2227 14.5703C68.3218 14.5703 67.7021 14.0166 67.7021 13.1509C67.7021 12.3291 68.2734 11.7754 69.3457 11.7754H70.519V11.3403C70.519 10.8086 70.1807 10.5581 69.6445 10.5581C69.1172 10.5581 68.8799 10.7778 68.8052 11.0854H67.8779C67.9351 10.3076 68.5195 9.78467 69.6753 9.78467C70.6685 9.78467 71.4902 10.1978 71.4902 11.3535V14.5H70.563V13.8979H70.519C70.3125 14.2539 69.9082 14.5703 69.2227 14.5703ZM69.5259 13.8145C70.0796 13.8145 70.519 13.4365 70.519 12.9312V12.4302H69.4995C68.9326 12.4302 68.6821 12.7158 68.6821 13.1025C68.6821 13.5859 69.0864 13.8145 69.5259 13.8145ZM74.4961 9.79346C75.1509 9.79346 75.6519 10.0835 75.832 10.5537H75.8804V8.18506H76.856V14.5H75.9067V13.7573H75.8584C75.7178 14.2275 75.1597 14.5615 74.4829 14.5615C73.415 14.5615 72.7207 13.8013 72.7207 12.5752V11.7798C72.7207 10.5537 73.4282 9.79346 74.4961 9.79346ZM74.7686 10.5933C74.1182 10.5933 73.7139 11.0767 73.7139 11.9204V12.4302C73.7139 13.2783 74.1226 13.7617 74.7905 13.7617C75.4497 13.7617 75.8804 13.2827 75.8804 12.4917V11.7886C75.8804 11.0723 75.4102 10.5933 74.7686 10.5933ZM82.2129 9.77588C83.4258 9.77588 84.2739 10.5625 84.2739 11.8677V12.4697C84.2739 13.8188 83.4258 14.5791 82.2129 14.5791C80.9824 14.5791 80.1431 13.8276 80.1431 12.4741V11.8721C80.1431 10.6021 80.9956 9.77588 82.2129 9.77588ZM82.2173 10.5625C81.5229 10.5625 81.1274 11.1426 81.1274 11.9204V12.439C81.1274 13.2168 81.4966 13.7925 82.2173 13.7925C82.9292 13.7925 83.2939 13.2212 83.2939 12.439V11.9204C83.2939 11.1426 82.9116 10.5625 82.2173 10.5625ZM85.5308 14.5V9.85498H86.4888V10.6899H86.5371C86.6558 10.3252 86.9941 9.78467 87.9346 9.78467C88.8706 9.78467 89.499 10.3032 89.499 11.3623V14.5H88.5234V11.6479C88.5234 10.9404 88.1543 10.6152 87.605 10.6152C86.8843 10.6152 86.5063 11.1689 86.5063 11.9204V14.5H85.5308ZM93.2739 9.88574V8.72559H94.2275V9.88574H95.269V10.6504H94.2275V13.1157C94.2275 13.6211 94.4165 13.7617 94.9395 13.7617C95.0713 13.7617 95.2471 13.7529 95.3218 13.7441V14.4912C95.2427 14.5044 94.9175 14.5308 94.6978 14.5308C93.5684 14.5308 93.2607 14.1265 93.2607 13.1948V10.6504H92.5532V9.88574H93.2739ZM96.4995 14.5V8.18506H97.4707V10.6899H97.519C97.6201 10.3604 97.998 9.78467 98.9297 9.78467C99.835 9.78467 100.481 10.3076 100.481 11.3667V14.5H99.5098V11.6523C99.5098 10.9448 99.1187 10.6152 98.5649 10.6152C97.8662 10.6152 97.4707 11.1646 97.4707 11.9204V14.5H96.4995ZM103.755 14.5791C102.489 14.5791 101.703 13.8013 101.703 12.4917V11.8633C101.703 10.5449 102.564 9.77588 103.698 9.77588C104.862 9.77588 105.684 10.5845 105.684 11.8633V12.4082H102.669V12.6367C102.669 13.3047 103.065 13.7969 103.75 13.7969C104.26 13.7969 104.612 13.5552 104.678 13.2651H105.631C105.574 13.8013 105.007 14.5791 103.755 14.5791ZM102.669 11.771H104.73V11.7095C104.73 11.0107 104.322 10.5449 103.702 10.5449C103.083 10.5449 102.669 11.0107 102.669 11.7095V11.771Z"

      fill="white"

    />

    <path

      d="M38.2061 30.5H36.1758L40.3066 18.5029H42.5391L46.6611 30.5H44.5518L43.4883 27.1777H39.2783L38.2061 30.5ZM41.4316 20.5771H41.3525L39.7266 25.6484H43.04L41.4316 20.5771ZM52.2644 30.6318C51.0603 30.6318 50.1462 30.0605 49.654 29.208H49.5837V33.585H47.6325V21.21H49.531V22.5723H49.6013C50.1111 21.6846 51.0603 21.0869 52.3083 21.0869C54.3913 21.0869 55.8767 22.6602 55.8767 25.4375V26.2637C55.8767 29.0234 54.4089 30.6318 52.2644 30.6318ZM51.8161 29.0234C53.0554 29.0234 53.8991 28.0303 53.8991 26.1582V25.5078C53.8991 23.7061 53.1081 22.6777 51.781 22.6777C50.4187 22.6777 49.5661 23.7852 49.5661 25.499V26.1582C49.5661 27.916 50.4275 29.0234 51.8161 29.0234ZM62.183 30.6318C60.9789 30.6318 60.0649 30.0605 59.5727 29.208H59.5024V33.585H57.5512V21.21H59.4496V22.5723H59.52C60.0297 21.6846 60.9789 21.0869 62.227 21.0869C64.31 21.0869 65.7954 22.6602 65.7954 25.4375V26.2637C65.7954 29.0234 64.3276 30.6318 62.183 30.6318ZM61.7348 29.0234C62.9741 29.0234 63.8178 28.0303 63.8178 26.1582V25.5078C63.8178 23.7061 63.0268 22.6777 61.6996 22.6777C60.3373 22.6777 59.4848 23.7852 59.4848 25.499V26.1582C59.4848 27.916 60.3461 29.0234 61.7348 29.0234ZM69.8387 27.1689H71.7899C71.8778 28.2061 72.7919 29.0938 74.4882 29.0938C76.0438 29.0938 76.9667 28.3643 76.9667 27.2305C76.9667 26.3164 76.3514 25.8242 75.0682 25.5166L73.0995 25.0244C71.5526 24.6641 70.1639 23.7412 70.1639 21.79C70.1639 19.4961 72.1679 18.2393 74.497 18.2393C76.8261 18.2393 78.7684 19.4961 78.8124 21.7373H76.8964C76.8085 20.7178 76.0262 19.874 74.4706 19.874C73.0995 19.874 72.1679 20.5244 72.1679 21.6406C72.1679 22.4229 72.7128 22.9854 73.829 23.2402L75.7889 23.7236C77.5907 24.1631 78.9618 25.0156 78.9618 27.0547C78.9618 29.4102 77.0546 30.7373 74.3387 30.7373C70.9989 30.7373 69.8827 28.7861 69.8387 27.1689ZM81.3395 21.21V18.9512H83.2555V21.21H85.066V22.7744H83.2555V27.7314C83.2555 28.7422 83.6334 29.0234 84.6793 29.0234C84.8463 29.0234 85.0045 29.0234 85.1188 29.0059V30.5C84.9605 30.5264 84.5914 30.5615 84.1959 30.5615C81.9371 30.5615 81.3131 29.7529 81.3131 27.8896V22.7744H80.0299V21.21H81.3395ZM90.3353 21.0518C93.0071 21.0518 94.4573 22.9326 94.4573 25.4639V26.2109C94.4573 28.8301 93.0159 30.6582 90.3353 30.6582C87.6546 30.6582 86.1956 28.8301 86.1956 26.2109V25.4639C86.1956 22.9414 87.6634 21.0518 90.3353 21.0518ZM90.3353 22.6162C88.8851 22.6162 88.1644 23.8027 88.1644 25.4902V26.2021C88.1644 27.8633 88.8763 29.085 90.3353 29.085C91.7943 29.085 92.4974 27.8721 92.4974 26.2021V25.4902C92.4974 23.7939 91.7855 22.6162 90.3353 22.6162ZM96.1055 30.5V21.21H98.0567V22.4316H98.127C98.3643 21.8516 99.0586 21.0781 100.351 21.0781C100.606 21.0781 100.825 21.0957 101.01 21.1309V22.8535C100.843 22.8096 100.5 22.7832 100.175 22.7832C98.6104 22.7832 98.083 23.75 98.083 24.998V30.5H96.1055ZM105.743 30.6582C103.256 30.6582 101.674 29.0146 101.674 26.2637V25.3232C101.674 22.7305 103.22 21.0518 105.664 21.0518C108.142 21.0518 109.636 22.792 109.636 25.4111V26.2988H103.598V26.5186C103.598 28.083 104.442 29.1201 105.769 29.1201C106.762 29.1201 107.439 28.6279 107.677 27.8281H109.531C109.25 29.3311 108.037 30.6582 105.743 30.6582ZM103.598 24.9365H107.729V24.9189C107.729 23.6006 106.912 22.5635 105.673 22.5635C104.416 22.5635 103.598 23.6006 103.598 24.9189V24.9365Z"

      fill="white"

    />

  </svg>

);

const downloadGoogleLight = () => (

  <svg

    width="120"

    height="40"

    viewBox="0 0 120 40"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

  >

    <rect width="120" height="40" rx="6" fill="white" />

    <rect x="0.5" y="0.5" width="119" height="39" rx="5.5" stroke="black" />

    <path

      d="M17.8048 19.4617L8.0896 30.0059C8.09051 30.0078 8.09051 30.0106 8.09142 30.0125C8.38981 31.1574 9.41179 32 10.6254 32C11.1108 32 11.5662 31.8656 11.9567 31.6305L11.9877 31.6118L22.9229 25.1593L17.8048 19.4617Z"

      fill="#EA4335"

    />

    <path

      d="M27.6331 17.6662L27.624 17.6597L22.9028 14.8612L17.5839 19.7013L22.9219 25.1582L27.6176 22.3878C28.4406 21.9324 29 21.045 29 20.0223C29 19.0052 28.4489 18.1225 27.6331 17.6662Z"

      fill="#FBBC04"

    />

    <path

      d="M8.08942 9.99331C8.03102 10.2135 8 10.4449 8 10.6838V29.3163C8 29.5552 8.03102 29.7866 8.09034 30.0059L18.1386 19.7313L8.08942 9.99331Z"

      fill="#4285F4"

    />

    <path

      d="M17.8766 19.9999L22.9044 14.8594L11.9819 8.38351C11.585 8.13996 11.1214 8 10.626 8C9.41237 8 8.38856 8.84447 8.09018 9.99034C8.09018 9.99127 8.08926 9.9922 8.08926 9.99314L17.8766 19.9999Z"

      fill="#34A853"

    />

    <path

      d="M43.61 11.71C43.61 12.71 43.3133 13.5067 42.72 14.1C42.0533 14.8067 41.1767 15.16 40.09 15.16C39.05 15.16 38.17 14.8 37.45 14.08C36.73 13.36 36.37 12.4733 36.37 11.42C36.37 10.3667 36.73 9.48 37.45 8.76C38.17 8.04 39.05 7.68 40.09 7.68C40.6167 7.68 41.1133 7.77333 41.58 7.96C42.0467 8.14667 42.43 8.41 42.73 8.75L42.07 9.41C41.85 9.14333 41.5633 8.93667 41.21 8.79C40.8633 8.63667 40.49 8.56 40.09 8.56C39.31 8.56 38.65 8.83 38.11 9.37C37.5767 9.91667 37.31 10.6 37.31 11.42C37.31 12.24 37.5767 12.9233 38.11 13.47C38.65 14.01 39.31 14.28 40.09 14.28C40.8033 14.28 41.3967 14.08 41.87 13.68C42.3433 13.28 42.6167 12.73 42.69 12.03H40.09V11.17H43.56C43.5933 11.3567 43.61 11.5367 43.61 11.71ZM48.9078 7.84V8.72H45.6478V10.99H48.5878V11.85H45.6478V14.12H48.9078V15H44.7278V7.84H48.9078ZM52.5877 8.72V15H51.6677V8.72H49.6677V7.84H54.5877V8.72H52.5877ZM58.6654 15H57.7454V7.84H58.6654V15ZM62.5487 8.72V15H61.6287V8.72H59.6287V7.84H64.5487V8.72H62.5487ZM74.521 11.42C74.521 12.48 74.1677 13.3667 73.461 14.08C72.7477 14.8 71.8743 15.16 70.841 15.16C69.801 15.16 68.9277 14.8 68.221 14.08C67.5143 13.3667 67.161 12.48 67.161 11.42C67.161 10.36 67.5143 9.47333 68.221 8.76C68.9277 8.04 69.801 7.68 70.841 7.68C71.881 7.68 72.7543 8.04333 73.461 8.77C74.1677 9.48333 74.521 10.3667 74.521 11.42ZM68.101 11.42C68.101 12.2467 68.361 12.93 68.881 13.47C69.4077 14.01 70.061 14.28 70.841 14.28C71.621 14.28 72.271 14.01 72.791 13.47C73.3177 12.9367 73.581 12.2533 73.581 11.42C73.581 10.5867 73.3177 9.90333 72.791 9.37C72.271 8.83 71.621 8.56 70.841 8.56C70.061 8.56 69.4077 8.83 68.881 9.37C68.361 9.91 68.101 10.5933 68.101 11.42ZM76.5267 15H75.6067V7.84H76.7267L80.2067 13.41H80.2467L80.2067 12.03V7.84H81.1267V15H80.1667L76.5267 9.16H76.4867L76.5267 10.54V15Z"

      fill="black"

    />

    <path

      d="M93.5181 31.4097H95.1469V20.3981H93.5181V31.4097ZM108.189 24.3646L106.322 29.1388H106.266L104.328 24.3646H102.573L105.479 31.0371L103.823 34.749H105.521L110 24.3646H108.189ZM98.9519 30.1588C98.4176 30.1588 97.6739 29.8902 97.6739 29.2234C97.6739 28.3742 98.6001 28.0483 99.4005 28.0483C100.116 28.0483 100.454 28.2042 100.889 28.4165C100.762 29.4365 99.892 30.1588 98.9519 30.1588ZM99.1483 24.1241C97.969 24.1241 96.7469 24.6482 96.2424 25.8101L97.6879 26.4188C97.9969 25.8101 98.5721 25.611 99.1762 25.611C100.019 25.611 100.875 26.121 100.889 27.0283V27.1411C100.594 26.971 99.9627 26.7165 99.1902 26.7165C97.632 26.7165 96.0451 27.5806 96.0451 29.1952C96.0451 30.6689 97.323 31.6184 98.7546 31.6184C99.8501 31.6184 100.454 31.1225 100.833 30.5411H100.889V31.3912H102.461V27.1692C102.461 25.2146 101.015 24.1241 99.1483 24.1241ZM89.0821 25.7053H86.7655V21.9308H89.0821C90.2998 21.9308 90.9911 22.9482 90.9911 23.8176C90.9911 24.6711 90.2998 25.7053 89.0821 25.7053ZM89.0402 20.3981H85.1375V31.4097H86.7655V27.2379H89.0402C90.8453 27.2379 92.6199 25.9184 92.6199 23.8176C92.6199 21.7168 90.8453 20.3981 89.0402 20.3981ZM67.7583 30.1606C66.6332 30.1606 65.6913 29.2102 65.6913 27.9047C65.6913 26.5852 66.6332 25.6198 67.7583 25.6198C68.8695 25.6198 69.7406 26.5852 69.7406 27.9047C69.7406 29.2102 68.8695 30.1606 67.7583 30.1606ZM69.6289 24.9812H69.5722C69.2064 24.5417 68.5038 24.1444 67.6178 24.1444C65.7611 24.1444 64.0599 25.7898 64.0599 27.9047C64.0599 30.0047 65.7611 31.6369 67.6178 31.6369C68.5038 31.6369 69.2064 31.2396 69.5722 30.7851H69.6289V31.3251C69.6289 32.7582 68.8695 33.5246 67.6457 33.5246C66.6471 33.5246 66.0282 32.8005 65.7751 32.1901L64.3549 32.7864C64.7626 33.78 65.8458 35 67.6457 35C69.5582 35 71.1757 33.8646 71.1757 31.0978V24.3708H69.6289V24.9812ZM72.3008 31.4097H73.9323V20.3973H72.3008V31.4097ZM76.3362 27.777C76.2943 26.3298 77.4474 25.5916 78.2766 25.5916C78.9243 25.5916 79.4725 25.9176 79.6549 26.3862L76.3362 27.777ZM81.3989 26.528C81.0899 25.6912 80.1472 24.1444 78.2208 24.1444C76.3083 24.1444 74.7196 25.6621 74.7196 27.8907C74.7196 29.9906 76.2943 31.6369 78.4032 31.6369C80.1053 31.6369 81.0899 30.5869 81.4976 29.9765L80.2319 29.1247C79.8103 29.7493 79.2333 30.1606 78.4032 30.1606C77.5739 30.1606 76.983 29.7774 76.6033 29.0261L81.5674 26.9534L81.3989 26.528ZM41.8501 25.2939V26.883H45.6184C45.5058 27.777 45.2107 28.4297 44.7612 28.8834C44.2121 29.4374 43.3541 30.0479 41.8501 30.0479C39.5291 30.0479 37.7152 28.1602 37.7152 25.8189C37.7152 23.4767 39.5291 21.5899 41.8501 21.5899C43.1018 21.5899 44.0157 22.0867 44.6905 22.7254L45.8017 21.604C44.8589 20.6959 43.6081 20 41.8501 20C38.6719 20 36 22.6117 36 25.8189C36 29.0261 38.6719 31.6369 41.8501 31.6369C43.5653 31.6369 44.8589 31.0688 45.8715 30.0047C46.9129 28.9547 47.2358 27.4793 47.2358 26.2866C47.2358 25.9176 47.2079 25.5775 47.1512 25.2939H41.8501ZM51.5208 30.1606C50.3957 30.1606 49.425 29.2243 49.425 27.8907C49.425 26.5421 50.3957 25.6198 51.5208 25.6198C52.6451 25.6198 53.6158 26.5421 53.6158 27.8907C53.6158 29.2243 52.6451 30.1606 51.5208 30.1606ZM51.5208 24.1444C49.4669 24.1444 47.7936 25.7194 47.7936 27.8907C47.7936 30.0479 49.4669 31.6369 51.5208 31.6369C53.5739 31.6369 55.2472 30.0479 55.2472 27.8907C55.2472 25.7194 53.5739 24.1444 51.5208 24.1444ZM59.65 30.1606C58.5249 30.1606 57.5542 29.2243 57.5542 27.8907C57.5542 26.5421 58.5249 25.6198 59.65 25.6198C60.7752 25.6198 61.745 26.5421 61.745 27.8907C61.745 29.2243 60.7752 30.1606 59.65 30.1606ZM59.65 24.1444C57.597 24.1444 55.9237 25.7194 55.9237 27.8907C55.9237 30.0479 57.597 31.6369 59.65 31.6369C61.7031 31.6369 63.3764 30.0479 63.3764 27.8907C63.3764 25.7194 61.7031 24.1444 59.65 24.1444Z"

      fill="black"

    />

  </svg>

);

const downloadAppleLight = () => (

  <svg

    width="120"

    height="40"

    viewBox="0 0 120 40"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

  >

    <rect width="120" height="40" rx="6" fill="white" />

    <rect x="0.5" y="0.5" width="119" height="39" rx="5.5" stroke="black" />

    <path

      d="M24.7045 20.7631C24.7166 19.8432 24.9669 18.9413 25.4321 18.1412C25.8972 17.3411 26.5621 16.6688 27.3648 16.187C26.8548 15.476 26.1821 14.8908 25.4 14.478C24.6178 14.0652 23.7479 13.8361 22.8592 13.809C20.9635 13.6147 19.1258 14.9164 18.1598 14.9164C17.1751 14.9164 15.6878 13.8283 14.0862 13.8604C13.0502 13.8931 12.0406 14.1872 11.1557 14.7141C10.2708 15.241 9.54075 15.9827 9.03674 16.8669C6.85352 20.5573 8.48201 25.9809 10.5734 28.964C11.6197 30.4247 12.8426 32.0564 14.4428 31.9985C16.0086 31.9351 16.5934 31.0237 18.4835 31.0237C20.3561 31.0237 20.9048 31.9985 22.5374 31.9617C24.2176 31.9351 25.2762 30.4945 26.2859 29.02C27.0377 27.9792 27.6162 26.8288 28 25.6116C27.0238 25.2085 26.1908 24.5338 25.6048 23.6716C25.0187 22.8094 24.7056 21.7979 24.7045 20.7631Z"

      fill="black"

    />

    <path

      d="M21.6208 11.8471C22.5369 10.7734 22.9883 9.39335 22.879 8C21.4793 8.14352 20.1865 8.7966 19.258 9.82911C18.804 10.3335 18.4563 10.9203 18.2348 11.556C18.0132 12.1917 17.9222 12.8638 17.9669 13.5338C18.6669 13.5408 19.3595 13.3927 19.9924 13.1005C20.6253 12.8084 21.1821 12.3798 21.6208 11.8471Z"

      fill="black"

    />

    <path

      d="M36.791 8.50146H38.8608C40.6494 8.50146 41.5195 9.56494 41.5195 11.4941C41.5195 13.4233 40.6406 14.5 38.8608 14.5H36.791V8.50146ZM37.7886 9.34082V13.6606H38.751C39.9375 13.6606 40.4956 12.9443 40.4956 11.5073C40.4956 10.0615 39.9331 9.34082 38.751 9.34082H37.7886ZM44.6748 9.77588C45.8877 9.77588 46.7358 10.5625 46.7358 11.8677V12.4697C46.7358 13.8188 45.8877 14.5791 44.6748 14.5791C43.4443 14.5791 42.605 13.8276 42.605 12.4741V11.8721C42.605 10.6021 43.4575 9.77588 44.6748 9.77588ZM44.6792 10.5625C43.9849 10.5625 43.5894 11.1426 43.5894 11.9204V12.439C43.5894 13.2168 43.9585 13.7925 44.6792 13.7925C45.3911 13.7925 45.7559 13.2212 45.7559 12.439V11.9204C45.7559 11.1426 45.3735 10.5625 44.6792 10.5625ZM53.8989 9.85498L52.6772 14.5H51.6841L50.7129 11.0723H50.6865L49.7329 14.5H48.7354L47.4609 9.85498H48.5112L49.2583 13.397H49.3022L50.2383 9.85498H51.1567L52.1191 13.397H52.1631L52.9233 9.85498H53.8989ZM54.8657 14.5V9.85498H55.8237V10.6899H55.8721C55.9907 10.3252 56.3291 9.78467 57.2695 9.78467C58.2056 9.78467 58.834 10.3032 58.834 11.3623V14.5H57.8584V11.6479C57.8584 10.9404 57.4893 10.6152 56.9399 10.6152C56.2192 10.6152 55.8413 11.1689 55.8413 11.9204V14.5H54.8657ZM60.3105 14.5V8.18506H61.2861V14.5H60.3105ZM64.6348 9.77588C65.8477 9.77588 66.6958 10.5625 66.6958 11.8677V12.4697C66.6958 13.8188 65.8477 14.5791 64.6348 14.5791C63.4043 14.5791 62.5649 13.8276 62.5649 12.4741V11.8721C62.5649 10.6021 63.4175 9.77588 64.6348 9.77588ZM64.6392 10.5625C63.9448 10.5625 63.5493 11.1426 63.5493 11.9204V12.439C63.5493 13.2168 63.9185 13.7925 64.6392 13.7925C65.3511 13.7925 65.7158 13.2212 65.7158 12.439V11.9204C65.7158 11.1426 65.3335 10.5625 64.6392 10.5625ZM69.2227 14.5703C68.3218 14.5703 67.7021 14.0166 67.7021 13.1509C67.7021 12.3291 68.2734 11.7754 69.3457 11.7754H70.519V11.3403C70.519 10.8086 70.1807 10.5581 69.6445 10.5581C69.1172 10.5581 68.8799 10.7778 68.8052 11.0854H67.8779C67.9351 10.3076 68.5195 9.78467 69.6753 9.78467C70.6685 9.78467 71.4902 10.1978 71.4902 11.3535V14.5H70.563V13.8979H70.519C70.3125 14.2539 69.9082 14.5703 69.2227 14.5703ZM69.5259 13.8145C70.0796 13.8145 70.519 13.4365 70.519 12.9312V12.4302H69.4995C68.9326 12.4302 68.6821 12.7158 68.6821 13.1025C68.6821 13.5859 69.0864 13.8145 69.5259 13.8145ZM74.4961 9.79346C75.1509 9.79346 75.6519 10.0835 75.832 10.5537H75.8804V8.18506H76.856V14.5H75.9067V13.7573H75.8584C75.7178 14.2275 75.1597 14.5615 74.4829 14.5615C73.415 14.5615 72.7207 13.8013 72.7207 12.5752V11.7798C72.7207 10.5537 73.4282 9.79346 74.4961 9.79346ZM74.7686 10.5933C74.1182 10.5933 73.7139 11.0767 73.7139 11.9204V12.4302C73.7139 13.2783 74.1226 13.7617 74.7905 13.7617C75.4497 13.7617 75.8804 13.2827 75.8804 12.4917V11.7886C75.8804 11.0723 75.4102 10.5933 74.7686 10.5933ZM82.2129 9.77588C83.4258 9.77588 84.2739 10.5625 84.2739 11.8677V12.4697C84.2739 13.8188 83.4258 14.5791 82.2129 14.5791C80.9824 14.5791 80.1431 13.8276 80.1431 12.4741V11.8721C80.1431 10.6021 80.9956 9.77588 82.2129 9.77588ZM82.2173 10.5625C81.5229 10.5625 81.1274 11.1426 81.1274 11.9204V12.439C81.1274 13.2168 81.4966 13.7925 82.2173 13.7925C82.9292 13.7925 83.2939 13.2212 83.2939 12.439V11.9204C83.2939 11.1426 82.9116 10.5625 82.2173 10.5625ZM85.5308 14.5V9.85498H86.4888V10.6899H86.5371C86.6558 10.3252 86.9941 9.78467 87.9346 9.78467C88.8706 9.78467 89.499 10.3032 89.499 11.3623V14.5H88.5234V11.6479C88.5234 10.9404 88.1543 10.6152 87.605 10.6152C86.8843 10.6152 86.5063 11.1689 86.5063 11.9204V14.5H85.5308ZM93.2739 9.88574V8.72559H94.2275V9.88574H95.269V10.6504H94.2275V13.1157C94.2275 13.6211 94.4165 13.7617 94.9395 13.7617C95.0713 13.7617 95.2471 13.7529 95.3218 13.7441V14.4912C95.2427 14.5044 94.9175 14.5308 94.6978 14.5308C93.5684 14.5308 93.2607 14.1265 93.2607 13.1948V10.6504H92.5532V9.88574H93.2739ZM96.4995 14.5V8.18506H97.4707V10.6899H97.519C97.6201 10.3604 97.998 9.78467 98.9297 9.78467C99.835 9.78467 100.481 10.3076 100.481 11.3667V14.5H99.5098V11.6523C99.5098 10.9448 99.1187 10.6152 98.5649 10.6152C97.8662 10.6152 97.4707 11.1646 97.4707 11.9204V14.5H96.4995ZM103.755 14.5791C102.489 14.5791 101.703 13.8013 101.703 12.4917V11.8633C101.703 10.5449 102.564 9.77588 103.698 9.77588C104.862 9.77588 105.684 10.5845 105.684 11.8633V12.4082H102.669V12.6367C102.669 13.3047 103.065 13.7969 103.75 13.7969C104.26 13.7969 104.612 13.5552 104.678 13.2651H105.631C105.574 13.8013 105.007 14.5791 103.755 14.5791ZM102.669 11.771H104.73V11.7095C104.73 11.0107 104.322 10.5449 103.702 10.5449C103.083 10.5449 102.669 11.0107 102.669 11.7095V11.771Z"

      fill="black"

    />

    <path

      d="M38.2061 30.5H36.1758L40.3066 18.5029H42.5391L46.6611 30.5H44.5518L43.4883 27.1777H39.2783L38.2061 30.5ZM41.4316 20.5771H41.3525L39.7266 25.6484H43.04L41.4316 20.5771ZM52.2644 30.6318C51.0603 30.6318 50.1462 30.0605 49.654 29.208H49.5837V33.585H47.6325V21.21H49.531V22.5723H49.6013C50.1111 21.6846 51.0603 21.0869 52.3083 21.0869C54.3913 21.0869 55.8767 22.6602 55.8767 25.4375V26.2637C55.8767 29.0234 54.4089 30.6318 52.2644 30.6318ZM51.8161 29.0234C53.0554 29.0234 53.8991 28.0303 53.8991 26.1582V25.5078C53.8991 23.7061 53.1081 22.6777 51.781 22.6777C50.4187 22.6777 49.5661 23.7852 49.5661 25.499V26.1582C49.5661 27.916 50.4275 29.0234 51.8161 29.0234ZM62.183 30.6318C60.9789 30.6318 60.0649 30.0605 59.5727 29.208H59.5024V33.585H57.5512V21.21H59.4496V22.5723H59.52C60.0297 21.6846 60.9789 21.0869 62.227 21.0869C64.31 21.0869 65.7954 22.6602 65.7954 25.4375V26.2637C65.7954 29.0234 64.3276 30.6318 62.183 30.6318ZM61.7348 29.0234C62.9741 29.0234 63.8178 28.0303 63.8178 26.1582V25.5078C63.8178 23.7061 63.0268 22.6777 61.6996 22.6777C60.3373 22.6777 59.4848 23.7852 59.4848 25.499V26.1582C59.4848 27.916 60.3461 29.0234 61.7348 29.0234ZM69.8387 27.1689H71.7899C71.8778 28.2061 72.7919 29.0938 74.4882 29.0938C76.0438 29.0938 76.9667 28.3643 76.9667 27.2305C76.9667 26.3164 76.3514 25.8242 75.0682 25.5166L73.0995 25.0244C71.5526 24.6641 70.1639 23.7412 70.1639 21.79C70.1639 19.4961 72.1679 18.2393 74.497 18.2393C76.8261 18.2393 78.7684 19.4961 78.8124 21.7373H76.8964C76.8085 20.7178 76.0262 19.874 74.4706 19.874C73.0995 19.874 72.1679 20.5244 72.1679 21.6406C72.1679 22.4229 72.7128 22.9854 73.829 23.2402L75.7889 23.7236C77.5907 24.1631 78.9618 25.0156 78.9618 27.0547C78.9618 29.4102 77.0546 30.7373 74.3387 30.7373C70.9989 30.7373 69.8827 28.7861 69.8387 27.1689ZM81.3395 21.21V18.9512H83.2555V21.21H85.066V22.7744H83.2555V27.7314C83.2555 28.7422 83.6334 29.0234 84.6793 29.0234C84.8463 29.0234 85.0045 29.0234 85.1188 29.0059V30.5C84.9605 30.5264 84.5914 30.5615 84.1959 30.5615C81.9371 30.5615 81.3131 29.7529 81.3131 27.8896V22.7744H80.0299V21.21H81.3395ZM90.3353 21.0518C93.0071 21.0518 94.4573 22.9326 94.4573 25.4639V26.2109C94.4573 28.8301 93.0159 30.6582 90.3353 30.6582C87.6546 30.6582 86.1956 28.8301 86.1956 26.2109V25.4639C86.1956 22.9414 87.6634 21.0518 90.3353 21.0518ZM90.3353 22.6162C88.8851 22.6162 88.1644 23.8027 88.1644 25.4902V26.2021C88.1644 27.8633 88.8763 29.085 90.3353 29.085C91.7943 29.085 92.4974 27.8721 92.4974 26.2021V25.4902C92.4974 23.7939 91.7855 22.6162 90.3353 22.6162ZM96.1055 30.5V21.21H98.0567V22.4316H98.127C98.3643 21.8516 99.0586 21.0781 100.351 21.0781C100.606 21.0781 100.825 21.0957 101.01 21.1309V22.8535C100.843 22.8096 100.5 22.7832 100.175 22.7832C98.6104 22.7832 98.083 23.75 98.083 24.998V30.5H96.1055ZM105.743 30.6582C103.256 30.6582 101.674 29.0146 101.674 26.2637V25.3232C101.674 22.7305 103.22 21.0518 105.664 21.0518C108.142 21.0518 109.636 22.792 109.636 25.4111V26.2988H103.598V26.5186C103.598 28.083 104.442 29.1201 105.769 29.1201C106.762 29.1201 107.439 28.6279 107.677 27.8281H109.531C109.25 29.3311 108.037 30.6582 105.743 30.6582ZM103.598 24.9365H107.729V24.9189C107.729 23.6006 106.912 22.5635 105.673 22.5635C104.416 22.5635 103.598 23.6006 103.598 24.9189V24.9365Z"

      fill="black"

    />

  </svg>

);



export default function Footer2() {

  return (

    <section className="py-8 px-4 [--color-primary:#003AF9]">

      <div className="container mx-auto">

        <motion.div

          initial={{ opacity: 0, y: 20 }}

          whileInView={{ opacity: 1, y: 0 }}

          viewport={{ once: true }}

          transition={{ duration: 0.5 }}

          className="bg-(--color-primary) rounded-md p-8 md:px-12 md:py-10 overflow-hidden flex flex-col md:flex-row items-center md:items-stretch justify-between relative max-w-7xl mx-auto"

        >

          {/* Left Section */}

          <div className="flex flex-col justify-between z-10 w-full md:w-1/2 space-y-6 md:space-y-0">

            <div className="space-y-6">

              <h2 className="text-3xl md:text-3xl lg:text-4xl font-bold text-white leading-tight">

                The AI Workspace

                <br />

                That Works For You

              </h2>



              <div className="flex flex-wrap gap-4">

                <Link

                  href=""

                  className="dark:hidden shadow-lg transition-transform hover:scale-105"

                >

                  {downloadAppleLight()}

                </Link>

                <Link

                  href=""

                  className="dark:hidden shadow-lg transition-transform hover:scale-105"

                >

                  {downloadGoogleLight()}

                </Link>

                {/* dark-mode buttons */}

                <Link

                  href=""

                  className="hidden dark:block shadow-lg transition-transform hover:scale-105"

                >

                  {downloadAppleDark()}

                </Link>

                <Link

                  href=""

                  className="hidden dark:block shadow-lg transition-transform hover:scale-105"

                >

                  {downloadGoogleDark()}

                </Link>

              </div>

            </div>



            <div className="flex flex-col space-y-3 md:space-y-6 md:mt-16">

              {/* Footer Items */}

              <nav className="flex flex-wrap gap-x-6 gap-y- text-white/90 font-medium">

                {["Products", "Solution", "Company", "About"].map((item) => (

                  <Link

                    key={item}

                    href="#"

                    className="hover:text-white transition-colors"

                  >

                    {item}

                  </Link>

                ))}

              </nav>



              {/* Social Cloud - adjust color to white for blue bg */}

              <div>

                <SocialCloud className="text-white/60 gap-5" />

              </div>

            </div>

          </div>



          {/* Right Section */}

          <div className="w-full flex flex-col justify-end items-center md:items-end md:absolute md:bottom-0 md:right-0 md:pr-12 md:pb-0 lg:pr-16 pointer-events-none">

            {/* Image Container */}

            <div

              className="relative w-auto pointer-events-auto rounded-t-md translate-y-20 md:translate-y-45 lg:translate-y-40"

              style={{

                boxShadow: "2px -5px 20px 8px rgba(255, 255, 255, 0.47)",

                marginBottom: "-1px", // Ensure it flushes with the bottom

              }}

            >

              <Image

                src="https://res.cloudinary.com/harshitproject/image/upload/v1746774805/Notion-screen.png"

                alt="App Screenshot"

                width={800}

                height={1000}

                className="h-[450px] md:h-[500px] w-auto rounded-t-md block object-cover object-top"

              />

            </div>

          </div>



          {/* Mobile Bottom Fade */}

          <div className="absolute bottom-0 left-0 w-full h-32 md:h-20 bg-gradient-to-t from-[#003AF9] to-transparent pointer-events-none z-20" />

        </motion.div>

      </div>

    </section>

  );

}
```
##### File: `src/registry/blocks/footer-section/social-cloud.tsx`
```tsx
"use client";

import React from "react";

import { cn } from "@/lib/utils";

import { motion, MotionProps } from "motion/react";



interface SocialIconProps extends React.SVGProps<SVGSVGElement> {}



export const XIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="23"

    height="20"

    viewBox="0 0 23 20"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M9.46617 12.6219L14.625 19.5H22.2083L13.6955 8.14883L20.7783 0H17.9075L12.3641 6.3765L7.58333 0H0L8.13583 10.8496L0.6175 19.5H3.48833L9.46617 12.6219ZM15.7083 17.3333L4.33333 2.16667H6.5L17.875 17.3333H15.7083Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const LinkedInIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="23"

    height="23"

    viewBox="0 0 23 23"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M20 0C20.663 0 21.2989 0.263392 21.7678 0.732233C22.2366 1.20107 22.5 1.83696 22.5 2.5V20C22.5 20.663 22.2366 21.2989 21.7678 21.7678C21.2989 22.2366 20.663 22.5 20 22.5H2.5C1.83696 22.5 1.20107 22.2366 0.732233 21.7678C0.263392 21.2989 0 20.663 0 20V2.5C0 1.83696 0.263392 1.20107 0.732233 0.732233C1.20107 0.263392 1.83696 0 2.5 0H20ZM19.375 19.375V12.75C19.375 11.6692 18.9457 10.6328 18.1815 9.86854C17.4172 9.10433 16.3808 8.675 15.3 8.675C14.2375 8.675 13 9.325 12.4 10.3V8.9125H8.9125V19.375H12.4V13.2125C12.4 12.25 13.175 11.4625 14.1375 11.4625C14.6016 11.4625 15.0467 11.6469 15.3749 11.9751C15.7031 12.3033 15.8875 12.7484 15.8875 13.2125V19.375H19.375ZM4.85 6.95C5.40695 6.95 5.9411 6.72875 6.33492 6.33492C6.72875 5.9411 6.95 5.40695 6.95 4.85C6.95 3.6875 6.0125 2.7375 4.85 2.7375C4.28973 2.7375 3.75241 2.96007 3.35624 3.35624C2.96007 3.75241 2.7375 4.28973 2.7375 4.85C2.7375 6.0125 3.6875 6.95 4.85 6.95ZM6.5875 19.375V8.9125H3.125V19.375H6.5875Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const FacebookIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="29"

    height="29"

    viewBox="0 0 29 29"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M28.3333 14.1667C28.3333 6.34667 21.9867 0 14.1667 0C6.34667 0 0 6.34667 0 14.1667C0 21.0233 4.87333 26.7325 11.3333 28.05V18.4167H8.5V14.1667H11.3333V10.625C11.3333 7.89083 13.5575 5.66667 16.2917 5.66667H19.8333V9.91667H17C16.2208 9.91667 15.5833 10.5542 15.5833 11.3333V14.1667H19.8333V18.4167H15.5833V28.2625C22.7375 27.5542 28.3333 21.5192 28.3333 14.1667Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const ThreadsIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="18"

    height="22"

    viewBox="0 0 18 22"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M17.4167 6.92196C15.5768 0.0771293 9.25 0.505296 9.25 0.505296C9.25 0.505296 0.5 -0.0780373 0.5 10.9995C0.5 22.077 9.25 21.4948 9.25 21.4948C9.25 21.4948 14.451 21.8401 16.8333 16.9238C17.6115 14.7561 17.4167 10.422 9.83333 10.422C9.83333 10.422 6.33333 10.422 6.33333 13.3386C6.33333 14.4773 7.5 15.672 9.25 15.672C11 15.672 12.9495 14.4738 13.3333 12.172C14.5 5.17196 8.08333 4.58863 6.33333 7.5053"

      stroke="currentColor"

      strokeLinecap="round"

      strokeLinejoin="round"

    />

  </motion.svg>

);



export const InstagramIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="24"

    height="24"

    viewBox="0 0 24 24"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M6.76667 0H16.5667C20.3 0 23.3333 3.03333 23.3333 6.76667V16.5667C23.3333 18.3613 22.6204 20.0824 21.3514 21.3514C20.0824 22.6204 18.3613 23.3333 16.5667 23.3333H6.76667C3.03333 23.3333 0 20.3 0 16.5667V6.76667C0 4.97204 0.712915 3.25091 1.98191 1.98191C3.25091 0.712915 4.97204 0 6.76667 0ZM6.53333 2.33333C5.41942 2.33333 4.35114 2.77583 3.56349 3.56349C2.77583 4.35114 2.33333 5.41942 2.33333 6.53333V16.8C2.33333 19.1217 4.21167 21 6.53333 21H16.8C17.9139 21 18.9822 20.5575 19.7699 19.7699C20.5575 18.9822 21 17.9139 21 16.8V6.53333C21 4.21167 19.1217 2.33333 16.8 2.33333H6.53333ZM17.7917 4.08333C18.1784 4.08333 18.5494 4.23698 18.8229 4.51047C19.0964 4.78396 19.25 5.15489 19.25 5.54167C19.25 5.92844 19.0964 6.29937 18.8229 6.57286C18.5494 6.84635 18.1784 7 17.7917 7C17.4049 7 17.034 6.84635 16.7605 6.57286C16.487 6.29937 16.3333 5.92844 16.3333 5.54167C16.3333 5.15489 16.487 4.78396 16.7605 4.51047C17.034 4.23698 17.4049 4.08333 17.7917 4.08333ZM11.6667 5.83333C13.2138 5.83333 14.6975 6.44792 15.7915 7.54188C16.8854 8.63584 17.5 10.1196 17.5 11.6667C17.5 13.2138 16.8854 14.6975 15.7915 15.7915C14.6975 16.8854 13.2138 17.5 11.6667 17.5C10.1196 17.5 8.63584 16.8854 7.54188 15.7915C6.44792 14.6975 5.83333 13.2138 5.83333 11.6667C5.83333 10.1196 6.44792 8.63584 7.54188 7.54188C8.63584 6.44792 10.1196 5.83333 11.6667 5.83333ZM11.6667 8.16667C10.7384 8.16667 9.84817 8.53542 9.19179 9.19179C8.53542 9.84817 8.16667 10.7384 8.16667 11.6667C8.16667 12.5949 8.53542 13.4852 9.19179 14.1415C9.84817 14.7979 10.7384 15.1667 11.6667 15.1667C12.5949 15.1667 13.4852 14.7979 14.1415 14.1415C14.7979 13.4852 15.1667 12.5949 15.1667 11.6667C15.1667 10.7384 14.7979 9.84817 14.1415 9.19179C13.4852 8.53542 12.5949 8.16667 11.6667 8.16667Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const TikTokIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="19"

    height="21"

    viewBox="0 0 19 21"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M14.5133 3.29C13.716 2.37945 13.2765 1.2103 13.2767 0H9.67167V14.4667C9.64444 15.2497 9.31411 15.9916 8.75036 16.5357C8.18661 17.0799 7.43353 17.3838 6.65 17.3833C4.99333 17.3833 3.61667 16.03 3.61667 14.35C3.61667 12.3433 5.55333 10.8383 7.54833 11.4567V7.77C3.52333 7.23333 0 10.36 0 14.35C0 18.235 3.22 21 6.63833 21C10.3017 21 13.2767 18.025 13.2767 14.35V7.01167C14.7385 8.06149 16.4936 8.62475 18.2933 8.62167V5.01667C18.2933 5.01667 16.1 5.12167 14.5133 3.29Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const YouTubeIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="29"

    height="21"

    viewBox="0 0 29 21"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M28.0501 3.13163C27.7206 1.8991 26.7497 0.928222 25.5172 0.59876C23.2832 -1.51787e-07 14.3244 0 14.3244 0C14.3244 0 5.36561 -1.51787e-07 3.13164 0.59876C1.8991 0.928222 0.928222 1.8991 0.59876 3.13163C0 5.36561 0 10.0271 0 10.0271C0 10.0271 0 14.6886 0.59876 16.9225C0.928222 18.1551 1.8991 19.126 3.13164 19.4554C5.36561 20.0542 14.3244 20.0542 14.3244 20.0542C14.3244 20.0542 23.2832 20.0542 25.5172 19.4554C26.7497 19.126 27.7206 18.1551 28.0501 16.9225C28.6488 14.6886 28.6488 10.0271 28.6488 10.0271C28.6488 10.0271 28.6488 5.36561 28.0501 3.13163ZM11.4595 14.3244V5.72977L18.9025 10.0271L11.4595 14.3244Z"

      fill="currentColor"

    />

  </motion.svg>

);



interface SocialCloudProps

  extends

    Omit<React.HTMLAttributes<HTMLDivElement>, keyof MotionProps>,

    MotionProps {}



export function SocialCloud({ className, ...props }: SocialCloudProps) {

  return (

    <motion.div

      className={cn(

        "flex flex-wrap items-center gap-6 cursor-pointer",

        className,

      )}

      {...props}

    >

      <XIcon />

      <LinkedInIcon />

      <FacebookIcon />

      <ThreadsIcon />

      <InstagramIcon />

      <TikTokIcon />

      <YouTubeIcon />

    </motion.div>

  );

}
```
---

## Footer Section 3 <a name="footer-section-3"></a>

Solace UI footer section block variation 3.

- **Registry Key**: `footer-section-3`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/footer-section-3.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/footer-section/three/index.tsx`
```tsx
"use client";

import React, { useRef } from "react";

import Link from "next/link";

import { motion, useInView } from "motion/react";

import { SocialCloud } from "@/registry/blocks/footer-section/social-cloud";
import { AnimatedGroup } from "@/components/ui/animated-group"; //from motion-primitives



export default function Footer3({

  title = "Build Tastefully Crafted UIs",

}: {

  title?: string;

}) {

  const containerVariants = {

    // ... existing variants ...

    hidden: { opacity: 0 },

    visible: {

      opacity: 1,

      transition: {

        staggerChildren: 0.1,

        delayChildren: 0.1,

      },

    },

  };



  const itemVariants = {

    hidden: { opacity: 0, y: 20 },

    visible: {

      opacity: 1,

      y: 0,

      transition: {

        duration: 0.5,

      },

    },

  };



  const titleRef = useRef(null);

  const isTitleInView = useInView(titleRef, { once: true, margin: "-100px" });



  return (

    <section className="py-12 px-4 [--color-primary:#003AF9]">

      <motion.div

        className="container mx-auto max-w-7xl"

        variants={containerVariants}

        initial="hidden"

        whileInView="visible"

        viewport={{ once: true }}

      >

        {/* 1. Upper div with noise and text */}

        <motion.div

          className="relative w-full overflow-hidden rounded-xl bg-(--color-primary) h-[100px] md:h-[300px]"

          variants={itemVariants}

        >

          {/* SVG Noise Overlay */}

          <svg

            className="absolute inset-0 w-full h-full opacity-90 pointer-events-none mix-blend-multiply"

            xmlns="http://www.w3.org/2000/svg"

          >

            <filter id="noiseFilter">

              <feTurbulence

                type="fractalNoise"

                baseFrequency="0.65"

                numOctaves="4"

                stitchTiles="stitch"

              />

            </filter>

            <rect width="100%" height="100%" filter="url(#noiseFilter)" />

          </svg>



          {/* Centered Text */}

          <div

            ref={titleRef}

            className="absolute inset-0 flex items-center justify-center pointer-events-none"

          >

            {isTitleInView && (

              <AnimatedGroup

                className="text-2xl md:text-4xl lg:text-5xl font-bold text-white text-center flex flex-wrap justify-center gap-x-3"

                preset="slide"

              >

                {title.split(" ").map((word) => (

                  <span key={word} className="inline-block">

                    {word}

                  </span>

                ))}

              </AnimatedGroup>

            )}

          </div>

        </motion.div>



        {/* 2 & 3. Bottom Section */}

        <div className="mt-8 md:mt-16 flex flex-col md:flex-row justify-between gap-12 lg:gap-8">

          {/* 2. Below Left Section */}

          <motion.div

            className="flex flex-col justify-between space-y-12 lg:w-1/3"

            variants={itemVariants}

          >

            <div className="space-y-4">

              <h3 className="text-xl font-bold text-black dark:text-white">

                Newsletter

              </h3>

              <div className="flex items-center gap-4 max-w-md">

                <input

                  type="email"

                  placeholder="Enter Your Email"

                  className="flex-1 rounded-lg border border-neutral-300 dark:border-neutral-700 px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-black dark:focus:ring-white bg-transparent dark:bg-neutral-900 text-black dark:text-white"

                />

                <button className="rounded-lg bg-black text-white dark:bg-white dark:text-black px-6 py-2 text-sm font-medium hover:opacity-90 transition-opacity">

                  Submit

                </button>

              </div>

            </div>



            <div className="space-y-4 md:space-y-6">

              <SocialCloud className="text-neutral-500 dark:text-neutral-400 gap-4" />

              <p className="text-sm text-neutral-400 dark:text-neutral-600">

                &copy; {new Date().getFullYear()} SolaceUI, All rights reserved

              </p>

            </div>

          </motion.div>



          {/* 3. Below Right Section */}

          <motion.div

            className="flex flex-wrap gap-8 lg:gap-24 lg:w-1/2 lg:justify-end"

            variants={itemVariants}

          >

            {/* Column 1: Product */}

            <div className="flex flex-col space-y-4">

              <h4 className="text-lg font-bold text-black dark:text-white">

                Product

              </h4>

              <ul className="flex flex-col space-y-2 text-neutral-600 dark:text-neutral-400">

                {[

                  "Features",

                  "Solution",

                  "Customers",

                  "Pricing",

                  "Help",

                  "Terms",

                ].map((item) => (

                  <li key={item}>

                    <Link

                      href="#"

                      className="hover:text-black dark:hover:text-white transition-colors"

                    >

                      {item}

                    </Link>

                  </li>

                ))}

              </ul>

            </div>



            {/* Column 2: Company */}

            <div className="flex flex-col space-y-4">

              <h4 className="text-lg font-bold text-black dark:text-white">

                Company

              </h4>

              <ul className="flex flex-col space-y-2 text-neutral-600 dark:text-neutral-400">

                {[

                  "About",

                  "Careers",

                  "Blogs",

                  "Press",

                  "Contact",

                  "Privacy",

                ].map((item) => (

                  <li key={item}>

                    <Link

                      href="#"

                      className="hover:text-black dark:hover:text-white transition-colors"

                    >

                      {item}

                    </Link>

                  </li>

                ))}

              </ul>

            </div>



            {/* Column 3: Social */}

            <div className="flex flex-col space-y-4">

              <h4 className="text-lg font-bold text-black dark:text-white">

                Social

              </h4>

              <ul className="flex flex-col space-y-2 text-neutral-600 dark:text-neutral-400">

                {[

                  "X",

                  "LinkedIn",

                  "Facebook",

                  "Threads",

                  "Instagram",

                  "Youtube",

                ].map((item) => (

                  <li key={item}>

                    <Link

                      href="#"

                      className="hover:text-black dark:hover:text-white transition-colors"

                    >

                      {item}

                    </Link>

                  </li>

                ))}

              </ul>

            </div>

          </motion.div>

        </div>

      </motion.div>

    </section>

  );

}
```
##### File: `src/registry/blocks/footer-section/social-cloud.tsx`
```tsx
"use client";

import React from "react";

import { cn } from "@/lib/utils";

import { motion, MotionProps } from "motion/react";



interface SocialIconProps extends React.SVGProps<SVGSVGElement> {}



export const XIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="23"

    height="20"

    viewBox="0 0 23 20"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M9.46617 12.6219L14.625 19.5H22.2083L13.6955 8.14883L20.7783 0H17.9075L12.3641 6.3765L7.58333 0H0L8.13583 10.8496L0.6175 19.5H3.48833L9.46617 12.6219ZM15.7083 17.3333L4.33333 2.16667H6.5L17.875 17.3333H15.7083Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const LinkedInIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="23"

    height="23"

    viewBox="0 0 23 23"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M20 0C20.663 0 21.2989 0.263392 21.7678 0.732233C22.2366 1.20107 22.5 1.83696 22.5 2.5V20C22.5 20.663 22.2366 21.2989 21.7678 21.7678C21.2989 22.2366 20.663 22.5 20 22.5H2.5C1.83696 22.5 1.20107 22.2366 0.732233 21.7678C0.263392 21.2989 0 20.663 0 20V2.5C0 1.83696 0.263392 1.20107 0.732233 0.732233C1.20107 0.263392 1.83696 0 2.5 0H20ZM19.375 19.375V12.75C19.375 11.6692 18.9457 10.6328 18.1815 9.86854C17.4172 9.10433 16.3808 8.675 15.3 8.675C14.2375 8.675 13 9.325 12.4 10.3V8.9125H8.9125V19.375H12.4V13.2125C12.4 12.25 13.175 11.4625 14.1375 11.4625C14.6016 11.4625 15.0467 11.6469 15.3749 11.9751C15.7031 12.3033 15.8875 12.7484 15.8875 13.2125V19.375H19.375ZM4.85 6.95C5.40695 6.95 5.9411 6.72875 6.33492 6.33492C6.72875 5.9411 6.95 5.40695 6.95 4.85C6.95 3.6875 6.0125 2.7375 4.85 2.7375C4.28973 2.7375 3.75241 2.96007 3.35624 3.35624C2.96007 3.75241 2.7375 4.28973 2.7375 4.85C2.7375 6.0125 3.6875 6.95 4.85 6.95ZM6.5875 19.375V8.9125H3.125V19.375H6.5875Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const FacebookIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="29"

    height="29"

    viewBox="0 0 29 29"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M28.3333 14.1667C28.3333 6.34667 21.9867 0 14.1667 0C6.34667 0 0 6.34667 0 14.1667C0 21.0233 4.87333 26.7325 11.3333 28.05V18.4167H8.5V14.1667H11.3333V10.625C11.3333 7.89083 13.5575 5.66667 16.2917 5.66667H19.8333V9.91667H17C16.2208 9.91667 15.5833 10.5542 15.5833 11.3333V14.1667H19.8333V18.4167H15.5833V28.2625C22.7375 27.5542 28.3333 21.5192 28.3333 14.1667Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const ThreadsIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="18"

    height="22"

    viewBox="0 0 18 22"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M17.4167 6.92196C15.5768 0.0771293 9.25 0.505296 9.25 0.505296C9.25 0.505296 0.5 -0.0780373 0.5 10.9995C0.5 22.077 9.25 21.4948 9.25 21.4948C9.25 21.4948 14.451 21.8401 16.8333 16.9238C17.6115 14.7561 17.4167 10.422 9.83333 10.422C9.83333 10.422 6.33333 10.422 6.33333 13.3386C6.33333 14.4773 7.5 15.672 9.25 15.672C11 15.672 12.9495 14.4738 13.3333 12.172C14.5 5.17196 8.08333 4.58863 6.33333 7.5053"

      stroke="currentColor"

      strokeLinecap="round"

      strokeLinejoin="round"

    />

  </motion.svg>

);



export const InstagramIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="24"

    height="24"

    viewBox="0 0 24 24"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M6.76667 0H16.5667C20.3 0 23.3333 3.03333 23.3333 6.76667V16.5667C23.3333 18.3613 22.6204 20.0824 21.3514 21.3514C20.0824 22.6204 18.3613 23.3333 16.5667 23.3333H6.76667C3.03333 23.3333 0 20.3 0 16.5667V6.76667C0 4.97204 0.712915 3.25091 1.98191 1.98191C3.25091 0.712915 4.97204 0 6.76667 0ZM6.53333 2.33333C5.41942 2.33333 4.35114 2.77583 3.56349 3.56349C2.77583 4.35114 2.33333 5.41942 2.33333 6.53333V16.8C2.33333 19.1217 4.21167 21 6.53333 21H16.8C17.9139 21 18.9822 20.5575 19.7699 19.7699C20.5575 18.9822 21 17.9139 21 16.8V6.53333C21 4.21167 19.1217 2.33333 16.8 2.33333H6.53333ZM17.7917 4.08333C18.1784 4.08333 18.5494 4.23698 18.8229 4.51047C19.0964 4.78396 19.25 5.15489 19.25 5.54167C19.25 5.92844 19.0964 6.29937 18.8229 6.57286C18.5494 6.84635 18.1784 7 17.7917 7C17.4049 7 17.034 6.84635 16.7605 6.57286C16.487 6.29937 16.3333 5.92844 16.3333 5.54167C16.3333 5.15489 16.487 4.78396 16.7605 4.51047C17.034 4.23698 17.4049 4.08333 17.7917 4.08333ZM11.6667 5.83333C13.2138 5.83333 14.6975 6.44792 15.7915 7.54188C16.8854 8.63584 17.5 10.1196 17.5 11.6667C17.5 13.2138 16.8854 14.6975 15.7915 15.7915C14.6975 16.8854 13.2138 17.5 11.6667 17.5C10.1196 17.5 8.63584 16.8854 7.54188 15.7915C6.44792 14.6975 5.83333 13.2138 5.83333 11.6667C5.83333 10.1196 6.44792 8.63584 7.54188 7.54188C8.63584 6.44792 10.1196 5.83333 11.6667 5.83333ZM11.6667 8.16667C10.7384 8.16667 9.84817 8.53542 9.19179 9.19179C8.53542 9.84817 8.16667 10.7384 8.16667 11.6667C8.16667 12.5949 8.53542 13.4852 9.19179 14.1415C9.84817 14.7979 10.7384 15.1667 11.6667 15.1667C12.5949 15.1667 13.4852 14.7979 14.1415 14.1415C14.7979 13.4852 15.1667 12.5949 15.1667 11.6667C15.1667 10.7384 14.7979 9.84817 14.1415 9.19179C13.4852 8.53542 12.5949 8.16667 11.6667 8.16667Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const TikTokIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="19"

    height="21"

    viewBox="0 0 19 21"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M14.5133 3.29C13.716 2.37945 13.2765 1.2103 13.2767 0H9.67167V14.4667C9.64444 15.2497 9.31411 15.9916 8.75036 16.5357C8.18661 17.0799 7.43353 17.3838 6.65 17.3833C4.99333 17.3833 3.61667 16.03 3.61667 14.35C3.61667 12.3433 5.55333 10.8383 7.54833 11.4567V7.77C3.52333 7.23333 0 10.36 0 14.35C0 18.235 3.22 21 6.63833 21C10.3017 21 13.2767 18.025 13.2767 14.35V7.01167C14.7385 8.06149 16.4936 8.62475 18.2933 8.62167V5.01667C18.2933 5.01667 16.1 5.12167 14.5133 3.29Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const YouTubeIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="29"

    height="21"

    viewBox="0 0 29 21"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M28.0501 3.13163C27.7206 1.8991 26.7497 0.928222 25.5172 0.59876C23.2832 -1.51787e-07 14.3244 0 14.3244 0C14.3244 0 5.36561 -1.51787e-07 3.13164 0.59876C1.8991 0.928222 0.928222 1.8991 0.59876 3.13163C0 5.36561 0 10.0271 0 10.0271C0 10.0271 0 14.6886 0.59876 16.9225C0.928222 18.1551 1.8991 19.126 3.13164 19.4554C5.36561 20.0542 14.3244 20.0542 14.3244 20.0542C14.3244 20.0542 23.2832 20.0542 25.5172 19.4554C26.7497 19.126 27.7206 18.1551 28.0501 16.9225C28.6488 14.6886 28.6488 10.0271 28.6488 10.0271C28.6488 10.0271 28.6488 5.36561 28.0501 3.13163ZM11.4595 14.3244V5.72977L18.9025 10.0271L11.4595 14.3244Z"

      fill="currentColor"

    />

  </motion.svg>

);



interface SocialCloudProps

  extends

    Omit<React.HTMLAttributes<HTMLDivElement>, keyof MotionProps>,

    MotionProps {}



export function SocialCloud({ className, ...props }: SocialCloudProps) {

  return (

    <motion.div

      className={cn(

        "flex flex-wrap items-center gap-6 cursor-pointer",

        className,

      )}

      {...props}

    >

      <XIcon />

      <LinkedInIcon />

      <FacebookIcon />

      <ThreadsIcon />

      <InstagramIcon />

      <TikTokIcon />

      <YouTubeIcon />

    </motion.div>

  );

}
```
##### File: `src/components/ui/animated-group.tsx`
```tsx
// Component From Motion Primitives



"use client";

import { ReactNode } from "react";

import { motion, Variants, HTMLMotionProps } from "motion/react";

import React from "react";



export type PresetType =

  | "fade"

  | "slide"

  | "scale"

  | "blur"

  | "blur-slide"

  | "zoom"

  | "flip"

  | "bounce"

  | "rotate"

  | "swing";



export type AnimatedGroupProps = {

  children: ReactNode;

  className?: string;

  variants?: {

    container?: Variants;

    item?: Variants;

  };

  preset?: PresetType;

  as?: React.ElementType;

  asChild?: React.ElementType;

};



const defaultContainerVariants: Variants = {

  visible: {

    transition: {

      staggerChildren: 0.1,

    },

  },

};



const defaultItemVariants: Variants = {

  hidden: { opacity: 0 },

  visible: { opacity: 1 },

};



const presetVariants: Record<PresetType, Variants> = {

  fade: {},

  slide: {

    hidden: { y: 20 },

    visible: { y: 0 },

  },

  scale: {

    hidden: { scale: 0.8 },

    visible: { scale: 1 },

  },

  blur: {

    hidden: { filter: "blur(4px)" },

    visible: { filter: "blur(0px)" },

  },

  "blur-slide": {

    hidden: { filter: "blur(4px)", y: 20 },

    visible: { filter: "blur(0px)", y: 0 },

  },

  zoom: {

    hidden: { scale: 0.5 },

    visible: {

      scale: 1,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  flip: {

    hidden: { rotateX: -90 },

    visible: {

      rotateX: 0,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  bounce: {

    hidden: { y: -50 },

    visible: {

      y: 0,

      transition: { type: "spring", stiffness: 400, damping: 10 },

    },

  },

  rotate: {

    hidden: { rotate: -180 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 200, damping: 15 },

    },

  },

  swing: {

    hidden: { rotate: -10 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 300, damping: 8 },

    },

  },

};



const mergeVariants = (

  defaultVariants: Variants,

  customVariants: Partial<Variants> = {},

): Variants => {

  return {

    hidden: {

      ...defaultVariants.hidden,

      ...customVariants.hidden,

    },

    visible: {

      ...defaultVariants.visible,

      ...customVariants.visible,

    },

  };

};



function AnimatedGroup({

  children,

  className,

  variants,

  preset = "fade",

  as = "div",

  asChild = "div",

}: AnimatedGroupProps) {

  const MotionContainer = motion.div;

  const MotionChild = motion.div;



  const presetVariant = preset ? presetVariants[preset] : {};

  const itemVariants = variants?.item

    ? mergeVariants(defaultItemVariants, variants.item)

    : mergeVariants(defaultItemVariants, presetVariant);



  const containerVariants = variants?.container || defaultContainerVariants;



  return (

    <MotionContainer

      initial="hidden"

      animate="visible"

      variants={containerVariants}

      className={className}

    >

      {React.Children.map(children, (child, index) => (

        <MotionChild key={index} variants={itemVariants}>

          {child}

        </MotionChild>

      ))}

    </MotionContainer>

  );

}



export { AnimatedGroup };
```
---

## Footer Section 4 <a name="footer-section-4"></a>

Solace UI footer section block variation 4.

- **Registry Key**: `footer-section-4`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/footer-section-4.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/footer-section/four/index.tsx`
```tsx
"use client";

import React from "react";

import Link from "next/link";

import Image from "next/image";

import { motion, Variants } from "motion/react";

import { SocialCloud } from "@/registry/blocks/footer-section/social-cloud";



const FOOTER_TITLE = "Build Tastefully Crafted UIs";



const SolaceUILogo = ({ className }: { className?: string }) => {

  return (

    <svg className={className} width="64" height="38" viewBox="0 0 64 38" fill="none" xmlns="http://www.w3.org/2000/svg">

      <path d="M0 20.1032L39.8387 20.1032C44.7808 20.1032 48.7871 24.1095 48.7871 29.0516C48.7871 33.9937 44.7808 38 39.8387 38L1.56459e-06 38L0 20.1032Z" fill="currentColor" />

      <path d="M63.4968 17.8968L23.6581 17.8968C18.716 17.8968 14.7097 13.8904 14.7097 8.94839C14.7097 4.00633 18.716 0 23.6581 0L63.4968 0V17.8968Z" fill="currentColor" />

    </svg>

  )

}



export default function Footer4() {

  const footerLinks = [

    {

      title: "Product",

      links: [

        { label: "Features", href: "#" },

        { label: "Solution", href: "#" },

        { label: "Customers", href: "#" },

        { label: "Pricing", href: "#" },

        { label: "Help", href: "#" },

        { label: "Terms", href: "#" },

      ],

    },

    {

      title: "Resources",

      links: [

        { label: "Help", href: "#" },

        { label: "Tutorials", href: "#" },

        { label: "API Reference", href: "#" },

        { label: "Status", href: "#" },

        { label: "Docs", href: "#" },

        { label: "Templates", href: "#" },

      ],

    },

    {

      title: "Company",

      links: [

        { label: "About", href: "#" },

        { label: "Careers", href: "#" },

        { label: "Team", href: "#" },

        { label: "Press", href: "#" },

        { label: "Contact", href: "#" },

        { label: "Privacy", href: "#" },

      ],

    },



    {

      title: "Socials",

      links: [

        { label: "X", href: "#" },

        { label: "LinkedIn", href: "#" },

        { label: "Facebook", href: "#" },

        { label: "Threads", href: "#" },

        { label: "Instagram", href: "#" },

        { label: "Youtube", href: "#" },

      ],

    },

  ];



  const containerVariants: Variants = {

    hidden: { opacity: 0 },

    visible: {

      opacity: 1,

      transition: {

        staggerChildren: 0.2,

        delayChildren: 0.1,

      },

    },

  };



  const itemVariants: Variants = {

    hidden: { opacity: 0, y: 30 },

    visible: {

      opacity: 1,

      y: 0,

      transition: {

        duration: 0.6,

        ease: "easeOut",

      },

    },

  };



  return (

    <section className="py-12 px-4 [--color-primary:#003AF9]">

      <motion.div

        className="container mx-auto max-w-7xl"

        initial="hidden"

        whileInView="visible"

        viewport={{ once: true, margin: "-100px" }}

        variants={containerVariants}

      >

        <div className="flex flex-col md:flex-row gap-4 h-full">

          {/* Blue Card (Left in image, described as Right by user potentially) */}

          <motion.div

            className="relative w-full md:w-1/3 min-h-[300px] md:min-h-[600px] overflow-hidden rounded-2xl bg-(--color-primary) flex flex-col justify-between p-8 md:p-10"

            variants={itemVariants}

          >

            {/* SVG Noise Overlay */}

            <svg

              className="absolute inset-0 w-full h-full opacity-90 pointer-events-none mix-blend-multiply z-0"

              xmlns="http://www.w3.org/2000/svg"

            >

              <filter id="noiseFilter2">

                <feTurbulence

                  type="fractalNoise"

                  baseFrequency="0.65"

                  numOctaves="4"

                  stitchTiles="stitch"

                />

              </filter>

              <rect width="100%" height="100%" filter="url(#noiseFilter2)" />

            </svg>



            {/* Top Logo */}

            <div className="relative z-10">

              <div className="flex items-center gap-2 text-white">

                <SolaceUILogo className="h-7 w-auto" />

                <span className="text-xl font-bold tracking-tight">

                  SolaceUI

                </span>

              </div>

            </div>



            {/* Bottom Content */}

            <div className="relative z-10 space-y-6">

              <h3 className="text-lg font-bold text-white capitalize">

                {FOOTER_TITLE}

              </h3>

              <SocialCloud className="text-white/80 gap-4" />

              <p className="text-xs text-white/60">

                &copy; {new Date().getFullYear()} SolaceUI, All rights reserved

              </p>

            </div>

          </motion.div>



          {/* White Card (Right in image) */}

          <motion.div

            className="w-full md:w-2/3 rounded-2xl bg-white dark:bg-black border border-neutral-300 dark:border-neutral-700 p-8 md:p-12 flex flex-col justify-between min-h-[500px] md:min-h-[600px]"

            variants={itemVariants}

          >

            {/* Top Categories Grid */}

            <div className="grid grid-cols-2 md:grid-cols-4 gap-8 md:gap-10">

              {footerLinks.map((section, idx) => (

                <div key={idx} className="flex flex-col space-y-6">

                  <h4 className="text-lg font-bold text-black dark:text-white">

                    {section.title}

                  </h4>

                  <ul className="flex flex-col space-y-3 text-neutral-600 dark:text-neutral-400 font-medium">

                    {section.links.map((link, linkIdx) => (

                      <li key={linkIdx}>

                        <Link

                          href={link.href}

                          className="hover:text-black dark:hover:text-white transition-colors"

                        >

                          {link.label}

                        </Link>

                      </li>

                    ))}

                  </ul>

                </div>

              ))}

            </div>



            {/* Bottom Newsletter */}

            <div className="space-y-4 mt-12 md:mt-0">

              <h4 className="text-lg font-bold text-black dark:text-white">

                Newsletter

              </h4>

              <div className="flex flex-col sm:flex-row gap-4 max-w-md w-full">

                <input

                  type="email"

                  placeholder="Enter Your Email"

                  className="flex-1 rounded-md px-4 py-3 text-sm focus:outline-none focus:ring-1 focus:ring-black dark:focus:ring-white bg-transparent text-black dark:text-white border border-neutral-300 dark:border-neutral-700"

                />

                <button className="rounded-md bg-black text-white dark:bg-white dark:text-black px-8 py-3 text-sm font-medium hover:opacity-90 transition-opacity">

                  Submit

                </button>

              </div>

            </div>

          </motion.div>

        </div>

      </motion.div>

    </section>

  );

}
```
##### File: `src/registry/blocks/footer-section/social-cloud.tsx`
```tsx
"use client";

import React from "react";

import { cn } from "@/lib/utils";

import { motion, MotionProps } from "motion/react";



interface SocialIconProps extends React.SVGProps<SVGSVGElement> {}



export const XIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="23"

    height="20"

    viewBox="0 0 23 20"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M9.46617 12.6219L14.625 19.5H22.2083L13.6955 8.14883L20.7783 0H17.9075L12.3641 6.3765L7.58333 0H0L8.13583 10.8496L0.6175 19.5H3.48833L9.46617 12.6219ZM15.7083 17.3333L4.33333 2.16667H6.5L17.875 17.3333H15.7083Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const LinkedInIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="23"

    height="23"

    viewBox="0 0 23 23"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M20 0C20.663 0 21.2989 0.263392 21.7678 0.732233C22.2366 1.20107 22.5 1.83696 22.5 2.5V20C22.5 20.663 22.2366 21.2989 21.7678 21.7678C21.2989 22.2366 20.663 22.5 20 22.5H2.5C1.83696 22.5 1.20107 22.2366 0.732233 21.7678C0.263392 21.2989 0 20.663 0 20V2.5C0 1.83696 0.263392 1.20107 0.732233 0.732233C1.20107 0.263392 1.83696 0 2.5 0H20ZM19.375 19.375V12.75C19.375 11.6692 18.9457 10.6328 18.1815 9.86854C17.4172 9.10433 16.3808 8.675 15.3 8.675C14.2375 8.675 13 9.325 12.4 10.3V8.9125H8.9125V19.375H12.4V13.2125C12.4 12.25 13.175 11.4625 14.1375 11.4625C14.6016 11.4625 15.0467 11.6469 15.3749 11.9751C15.7031 12.3033 15.8875 12.7484 15.8875 13.2125V19.375H19.375ZM4.85 6.95C5.40695 6.95 5.9411 6.72875 6.33492 6.33492C6.72875 5.9411 6.95 5.40695 6.95 4.85C6.95 3.6875 6.0125 2.7375 4.85 2.7375C4.28973 2.7375 3.75241 2.96007 3.35624 3.35624C2.96007 3.75241 2.7375 4.28973 2.7375 4.85C2.7375 6.0125 3.6875 6.95 4.85 6.95ZM6.5875 19.375V8.9125H3.125V19.375H6.5875Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const FacebookIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="29"

    height="29"

    viewBox="0 0 29 29"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M28.3333 14.1667C28.3333 6.34667 21.9867 0 14.1667 0C6.34667 0 0 6.34667 0 14.1667C0 21.0233 4.87333 26.7325 11.3333 28.05V18.4167H8.5V14.1667H11.3333V10.625C11.3333 7.89083 13.5575 5.66667 16.2917 5.66667H19.8333V9.91667H17C16.2208 9.91667 15.5833 10.5542 15.5833 11.3333V14.1667H19.8333V18.4167H15.5833V28.2625C22.7375 27.5542 28.3333 21.5192 28.3333 14.1667Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const ThreadsIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="18"

    height="22"

    viewBox="0 0 18 22"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M17.4167 6.92196C15.5768 0.0771293 9.25 0.505296 9.25 0.505296C9.25 0.505296 0.5 -0.0780373 0.5 10.9995C0.5 22.077 9.25 21.4948 9.25 21.4948C9.25 21.4948 14.451 21.8401 16.8333 16.9238C17.6115 14.7561 17.4167 10.422 9.83333 10.422C9.83333 10.422 6.33333 10.422 6.33333 13.3386C6.33333 14.4773 7.5 15.672 9.25 15.672C11 15.672 12.9495 14.4738 13.3333 12.172C14.5 5.17196 8.08333 4.58863 6.33333 7.5053"

      stroke="currentColor"

      strokeLinecap="round"

      strokeLinejoin="round"

    />

  </motion.svg>

);



export const InstagramIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="24"

    height="24"

    viewBox="0 0 24 24"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M6.76667 0H16.5667C20.3 0 23.3333 3.03333 23.3333 6.76667V16.5667C23.3333 18.3613 22.6204 20.0824 21.3514 21.3514C20.0824 22.6204 18.3613 23.3333 16.5667 23.3333H6.76667C3.03333 23.3333 0 20.3 0 16.5667V6.76667C0 4.97204 0.712915 3.25091 1.98191 1.98191C3.25091 0.712915 4.97204 0 6.76667 0ZM6.53333 2.33333C5.41942 2.33333 4.35114 2.77583 3.56349 3.56349C2.77583 4.35114 2.33333 5.41942 2.33333 6.53333V16.8C2.33333 19.1217 4.21167 21 6.53333 21H16.8C17.9139 21 18.9822 20.5575 19.7699 19.7699C20.5575 18.9822 21 17.9139 21 16.8V6.53333C21 4.21167 19.1217 2.33333 16.8 2.33333H6.53333ZM17.7917 4.08333C18.1784 4.08333 18.5494 4.23698 18.8229 4.51047C19.0964 4.78396 19.25 5.15489 19.25 5.54167C19.25 5.92844 19.0964 6.29937 18.8229 6.57286C18.5494 6.84635 18.1784 7 17.7917 7C17.4049 7 17.034 6.84635 16.7605 6.57286C16.487 6.29937 16.3333 5.92844 16.3333 5.54167C16.3333 5.15489 16.487 4.78396 16.7605 4.51047C17.034 4.23698 17.4049 4.08333 17.7917 4.08333ZM11.6667 5.83333C13.2138 5.83333 14.6975 6.44792 15.7915 7.54188C16.8854 8.63584 17.5 10.1196 17.5 11.6667C17.5 13.2138 16.8854 14.6975 15.7915 15.7915C14.6975 16.8854 13.2138 17.5 11.6667 17.5C10.1196 17.5 8.63584 16.8854 7.54188 15.7915C6.44792 14.6975 5.83333 13.2138 5.83333 11.6667C5.83333 10.1196 6.44792 8.63584 7.54188 7.54188C8.63584 6.44792 10.1196 5.83333 11.6667 5.83333ZM11.6667 8.16667C10.7384 8.16667 9.84817 8.53542 9.19179 9.19179C8.53542 9.84817 8.16667 10.7384 8.16667 11.6667C8.16667 12.5949 8.53542 13.4852 9.19179 14.1415C9.84817 14.7979 10.7384 15.1667 11.6667 15.1667C12.5949 15.1667 13.4852 14.7979 14.1415 14.1415C14.7979 13.4852 15.1667 12.5949 15.1667 11.6667C15.1667 10.7384 14.7979 9.84817 14.1415 9.19179C13.4852 8.53542 12.5949 8.16667 11.6667 8.16667Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const TikTokIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="19"

    height="21"

    viewBox="0 0 19 21"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M14.5133 3.29C13.716 2.37945 13.2765 1.2103 13.2767 0H9.67167V14.4667C9.64444 15.2497 9.31411 15.9916 8.75036 16.5357C8.18661 17.0799 7.43353 17.3838 6.65 17.3833C4.99333 17.3833 3.61667 16.03 3.61667 14.35C3.61667 12.3433 5.55333 10.8383 7.54833 11.4567V7.77C3.52333 7.23333 0 10.36 0 14.35C0 18.235 3.22 21 6.63833 21C10.3017 21 13.2767 18.025 13.2767 14.35V7.01167C14.7385 8.06149 16.4936 8.62475 18.2933 8.62167V5.01667C18.2933 5.01667 16.1 5.12167 14.5133 3.29Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const YouTubeIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="29"

    height="21"

    viewBox="0 0 29 21"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M28.0501 3.13163C27.7206 1.8991 26.7497 0.928222 25.5172 0.59876C23.2832 -1.51787e-07 14.3244 0 14.3244 0C14.3244 0 5.36561 -1.51787e-07 3.13164 0.59876C1.8991 0.928222 0.928222 1.8991 0.59876 3.13163C0 5.36561 0 10.0271 0 10.0271C0 10.0271 0 14.6886 0.59876 16.9225C0.928222 18.1551 1.8991 19.126 3.13164 19.4554C5.36561 20.0542 14.3244 20.0542 14.3244 20.0542C14.3244 20.0542 23.2832 20.0542 25.5172 19.4554C26.7497 19.126 27.7206 18.1551 28.0501 16.9225C28.6488 14.6886 28.6488 10.0271 28.6488 10.0271C28.6488 10.0271 28.6488 5.36561 28.0501 3.13163ZM11.4595 14.3244V5.72977L18.9025 10.0271L11.4595 14.3244Z"

      fill="currentColor"

    />

  </motion.svg>

);



interface SocialCloudProps

  extends

    Omit<React.HTMLAttributes<HTMLDivElement>, keyof MotionProps>,

    MotionProps {}



export function SocialCloud({ className, ...props }: SocialCloudProps) {

  return (

    <motion.div

      className={cn(

        "flex flex-wrap items-center gap-6 cursor-pointer",

        className,

      )}

      {...props}

    >

      <XIcon />

      <LinkedInIcon />

      <FacebookIcon />

      <ThreadsIcon />

      <InstagramIcon />

      <TikTokIcon />

      <YouTubeIcon />

    </motion.div>

  );

}
```
---

## Footer Section 5 <a name="footer-section-5"></a>

Solace UI footer section block variation 5.

- **Registry Key**: `footer-section-5`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/footer-section-5.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `@paper-design/shaders-react`

#### Component Source Code
##### File: `src/registry/blocks/footer-section/five/index.tsx`
```tsx
"use client";
import React from "react";
import { FlutedGlass } from "@paper-design/shaders-react";
import Link from "next/link";

const companyName = "SolaceUI";


const SolaceUILogo = ({ className }: { className?: string }) => {
  return (
    <svg className={className} width="64" height="38" viewBox="0 0 64 38" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M0 20.1032L39.8387 20.1032C44.7808 20.1032 48.7871 24.1095 48.7871 29.0516C48.7871 33.9937 44.7808 38 39.8387 38L1.56459e-06 38L0 20.1032Z" fill="currentColor" />
      <path d="M63.4968 17.8968L23.6581 17.8968C18.716 17.8968 14.7097 13.8904 14.7097 8.94839C14.7097 4.00633 18.716 0 23.6581 0L63.4968 0V17.8968Z" fill="currentColor" />
    </svg>
  )
}

const footerLinks = [
  {
    title: "Product",
    links: [
      { name: "Features", href: "#" },
      { name: "Solution", href: "#" },
      { name: "Customers", href: "#" },
      { name: "Pricing", href: "#" },
      { name: "Help", href: "#" },
      { name: "Terms", href: "#" },
    ],
  },
  {
    title: "Company",
    links: [
      { name: "About", href: "#" },
      { name: "Careers", href: "#" },
      { name: "Blogs", href: "#" },
      { name: "Pricing", href: "#" },
      { name: "Contact", href: "#" },
      { name: "Privacy", href: "#" },
    ],
  },
  {
    title: "Social",
    links: [
      { name: "X", href: "#" },
      { name: "LinkedIn", href: "#" },
      { name: "Facebook", href: "#" },
      { name: "Threads", href: "#" },
      { name: "Instagram", href: "#" },
      { name: "Youtube", href: "#" },
    ],
  },
];

export default function FooterSection5() {
  return (
    <footer className="w-full bg-white relative overflow-hidden antialiased [font-synthesis:none]">
      {/* Large Stroke Text Section */}
      <div className="relative w-full flex justify-center items-end pt-24 md:pt-32 pb-0 z-0">
        <h1 className="text-[120px] sm:text-[160px] md:text-[210px] font-semibold text-transparent [-webkit-text-stroke:1px_rgba(0,0,0,0.4)] leading-[0.75] select-none -mb-4 md:-mb-6 opacity-50">
          {companyName}
        </h1>
      </div>

      {/* Blue Panel Section */}
      <div className="relative w-full [--color-primary:#1C76F8] bg-(--color-primary) z-10 min-h-[400px]">
        {/* Background Shader */}
        <div className="absolute inset-0 z-0 pointer-events-none">
          <FlutedGlass
            size={0.89}
            shape="lines"
            angle={0}
            distortionShape="prism"
            distortion={0.5}
            shift={0}
            blur={0}
            edges={0.25}
            stretch={0}
            scale={1.11}
            fit="cover"
            highlights={0.1}
            shadows={0.2}
            grainMixer={0.1}
            grainOverlay={0.1}
            colorBack="#00000000"
            colorHighlight="#FFFFFF"
            colorShadow="#000000"
            className="w-full h-full bg-transparent"
          />
        </div>

        {/* Content */}
        <div className="relative z-10 max-w-7xl mx-auto px-6 md:px-12 lg:px-24 py-16 md:py-24 flex flex-col lg:flex-row justify-between gap-16 lg:gap-8">

          {/* Left Side */}
          <div className="flex flex-col justify-between max-w-sm w-full">
            <div className="flex flex-col">
              {/* Logo SVG */}
              <SolaceUILogo className="text-white w-9 h-auto shrink-0 mb-2" />
              <h2 className="text-white text-xl md:text-[22px] font-medium leading-tight">
                Ship Tastefully Crafted<br />Marketing Pages
              </h2>
            </div>

            <div className="flex flex-col gap-3 mt-12 lg:mt-auto pt-8">
              {/* Social Icons SVG from reference */}
              <svg width="312" height="34" viewBox="0 0 312 34" fill="none" xmlns="http://www.w3.org/2000/svg" className="shrink-0 w-[180px] md:w-[200px] h-auto">
                <path d="M12.673 20.703L18.427 28.375H26.885L17.39 15.714L25.29 6.625H22.088L15.905 13.737L10.573 6.625H2.115L11.189 18.727L2.803 28.375H6.005L12.673 20.703ZM19.635 25.958L6.948 9.042H9.364L22.052 25.958H19.635Z" fill="#FFFFFF" />
                <path d="M68.75 5.75C69.413 5.75 70.049 6.013 70.518 6.482C70.987 6.951 71.25 7.587 71.25 8.25V25.75C71.25 26.413 70.987 27.049 70.518 27.518C70.049 27.987 69.413 28.25 68.75 28.25H51.25C50.587 28.25 49.951 27.987 49.482 27.518C49.013 27.049 48.75 26.413 48.75 25.75V8.25C48.75 7.587 49.013 6.951 49.482 6.482C49.951 6.013 50.587 5.75 51.25 5.75H68.75ZM68.125 25.125V18.5C68.125 17.419 67.696 16.383 66.931 15.618C66.167 14.854 65.131 14.425 64.05 14.425C62.987 14.425 61.75 15.075 61.15 16.05V14.662H57.663V25.125H61.15V18.962C61.15 18 61.925 17.212 62.888 17.212C63.352 17.212 63.797 17.397 64.125 17.725C64.453 18.053 64.638 18.498 64.638 18.962V25.125H68.125ZM53.6 12.7C54.157 12.7 54.691 12.479 55.085 12.085C55.479 11.691 55.7 11.157 55.7 10.6C55.7 9.438 54.763 8.488 53.6 8.488C53.04 8.488 52.502 8.71 52.106 9.106C51.71 9.502 51.487 10.04 51.487 10.6C51.487 11.762 52.438 12.7 53.6 12.7ZM55.337 25.125V14.662H51.875V25.125H55.337Z" fill="#FFFFFF" />
                <path d="M124.167 17C124.167 9.18 117.82 2.833 110 2.833C102.18 2.833 95.833 9.18 95.833 17C95.833 23.857 100.707 29.566 107.167 30.883V21.25H104.333V17H107.167V13.458C107.167 10.724 109.391 8.5 112.125 8.5H115.667V12.75H112.833C112.054 12.75 111.417 13.387 111.417 14.167V17H115.667V21.25H111.417V31.096C118.571 30.387 124.167 24.352 124.167 17Z" fill="#FFFFFF" />
                <path d="M167.458 12.922C165.619 6.078 159.292 6.506 159.292 6.506C159.292 6.506 150.542 5.923 150.542 17C150.542 28.078 159.292 27.495 159.292 27.495C159.292 27.495 164.493 27.841 166.875 22.924C167.653 20.757 167.458 16.422 159.875 16.422C159.875 16.422 156.375 16.422 156.375 19.339C156.375 20.478 157.542 21.672 159.292 21.672C161.042 21.672 162.991 20.474 163.375 18.172C164.542 11.172 158.125 10.589 156.375 13.506" stroke="#FFFFFF" strokeLinecap="round" strokeLinejoin="round" />
                <path d="M200.1 6.333H209.9C213.633 6.333 216.667 9.367 216.667 13.1V22.9C216.667 24.695 215.954 26.416 214.685 27.685C213.416 28.954 211.695 29.667 209.9 29.667H200.1C196.367 29.667 193.333 26.633 193.333 22.9V13.1C193.333 11.305 194.046 9.584 195.315 8.315C196.584 7.046 198.305 6.333 200.1 6.333ZM199.867 8.667C198.753 8.667 197.684 9.109 196.897 9.897C196.109 10.684 195.667 11.753 195.667 12.867V23.133C195.667 25.455 197.545 27.333 199.867 27.333H210.133C211.247 27.333 212.315 26.891 213.103 26.103C213.891 25.316 214.333 24.247 214.333 23.133V12.867C214.333 10.545 212.455 8.667 210.133 8.667H199.867ZM211.125 10.417C211.512 10.417 211.883 10.57 212.156 10.844C212.43 11.117 212.583 11.488 212.583 11.875C212.583 12.262 212.43 12.633 212.156 12.906C211.883 13.18 211.512 13.333 211.125 13.333C210.738 13.333 210.367 13.18 210.094 12.906C209.82 12.633 209.667 12.262 209.667 11.875C209.667 11.488 209.82 11.117 210.094 10.844C210.367 10.57 210.738 10.417 211.125 10.417ZM205 12.167C206.547 12.167 208.031 12.781 209.125 13.875C210.219 14.969 210.833 16.453 210.833 18C210.833 19.547 210.219 21.031 209.125 22.125C208.031 23.219 206.547 23.833 205 23.833C203.453 23.833 201.969 23.219 200.875 22.125C199.781 21.031 199.167 19.547 199.167 18C199.167 16.453 199.781 14.969 200.875 13.875C201.969 12.781 203.453 12.167 205 12.167ZM205 14.5C204.072 14.5 203.181 14.869 202.525 15.525C201.869 16.181 201.5 17.072 201.5 18C201.5 18.928 201.869 19.819 202.525 20.475C203.181 21.131 204.072 21.5 205 21.5C205.928 21.5 206.818 21.131 207.475 20.475C208.131 19.819 208.5 18.928 208.5 18C208.5 17.072 208.131 16.181 207.475 15.525C206.818 14.869 205.928 14.5 205 14.5Z" fill="#FFFFFF" />
                <path d="M256.367 9.79C255.569 8.879 255.13 7.71 255.13 6.5H251.525V20.967C251.498 21.75 251.167 22.492 250.604 23.036C250.04 23.58 249.287 23.884 248.503 23.883C246.847 23.883 245.47 22.53 245.47 20.85C245.47 18.843 247.407 17.338 249.402 17.957V14.27C245.377 13.733 241.853 16.86 241.853 20.85C241.853 24.735 245.073 27.5 248.492 27.5C252.155 27.5 255.13 24.525 255.13 20.85V13.512C256.592 14.562 258.347 15.125 260.147 15.122V11.517C260.147 11.517 257.953 11.622 256.367 9.79Z" fill="#FFFFFF" />
                <path d="M311.209 8.588C310.88 7.356 309.909 6.385 308.677 6.055C306.443 5.457 297.484 5.457 297.484 5.457C297.484 5.457 288.525 5.457 286.291 6.055C285.059 6.385 284.088 7.356 283.758 8.588C283.159 10.822 283.159 15.484 283.159 15.484C283.159 15.484 283.159 20.145 283.758 22.379C284.088 23.612 285.059 24.583 286.291 24.912C288.525 25.511 297.484 25.511 297.484 25.511C297.484 25.511 306.443 25.511 308.677 24.912C309.909 24.583 310.88 23.612 311.209 22.379C311.808 20.145 311.808 15.484 311.808 15.484C311.808 15.484 311.808 10.822 311.209 8.588ZM294.619 19.781V11.186L302.062 15.484L294.619 19.781Z" fill="#FFFFFF" />
              </svg>
              <p className="font-light text-white/80 text-xs md:text-[13px] mt-1">
                © 2026 {companyName}, All rights reserved
              </p>
            </div>
          </div>

          {/* Right Side - Links */}
          <div className="flex gap-12 md:gap-24 flex-wrap lg:flex-nowrap">
            {footerLinks.map((section) => (
              <div key={section.title} className="flex flex-col gap-5">
                <h3 className="text-white font-semibold text-lg md:text-xl">
                  {section.title}
                </h3>
                <ul className="flex flex-col gap-3 md:gap-4">
                  {section.links.map((link) => (
                    <li key={link.name}>
                      <Link
                        href={link.href}
                        className="text-white/70 hover:text-white transition-colors text-sm md:text-[15px] font-medium"
                      >
                        {link.name}
                      </Link>
                    </li>
                  ))}
                </ul>
              </div>
            ))}
          </div>

        </div>
      </div>
    </footer>
  );
}
```
##### File: `src/registry/blocks/footer-section/social-cloud.tsx`
```tsx
"use client";

import React from "react";

import { cn } from "@/lib/utils";

import { motion, MotionProps } from "motion/react";



interface SocialIconProps extends React.SVGProps<SVGSVGElement> {}



export const XIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="23"

    height="20"

    viewBox="0 0 23 20"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M9.46617 12.6219L14.625 19.5H22.2083L13.6955 8.14883L20.7783 0H17.9075L12.3641 6.3765L7.58333 0H0L8.13583 10.8496L0.6175 19.5H3.48833L9.46617 12.6219ZM15.7083 17.3333L4.33333 2.16667H6.5L17.875 17.3333H15.7083Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const LinkedInIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="23"

    height="23"

    viewBox="0 0 23 23"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M20 0C20.663 0 21.2989 0.263392 21.7678 0.732233C22.2366 1.20107 22.5 1.83696 22.5 2.5V20C22.5 20.663 22.2366 21.2989 21.7678 21.7678C21.2989 22.2366 20.663 22.5 20 22.5H2.5C1.83696 22.5 1.20107 22.2366 0.732233 21.7678C0.263392 21.2989 0 20.663 0 20V2.5C0 1.83696 0.263392 1.20107 0.732233 0.732233C1.20107 0.263392 1.83696 0 2.5 0H20ZM19.375 19.375V12.75C19.375 11.6692 18.9457 10.6328 18.1815 9.86854C17.4172 9.10433 16.3808 8.675 15.3 8.675C14.2375 8.675 13 9.325 12.4 10.3V8.9125H8.9125V19.375H12.4V13.2125C12.4 12.25 13.175 11.4625 14.1375 11.4625C14.6016 11.4625 15.0467 11.6469 15.3749 11.9751C15.7031 12.3033 15.8875 12.7484 15.8875 13.2125V19.375H19.375ZM4.85 6.95C5.40695 6.95 5.9411 6.72875 6.33492 6.33492C6.72875 5.9411 6.95 5.40695 6.95 4.85C6.95 3.6875 6.0125 2.7375 4.85 2.7375C4.28973 2.7375 3.75241 2.96007 3.35624 3.35624C2.96007 3.75241 2.7375 4.28973 2.7375 4.85C2.7375 6.0125 3.6875 6.95 4.85 6.95ZM6.5875 19.375V8.9125H3.125V19.375H6.5875Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const FacebookIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="29"

    height="29"

    viewBox="0 0 29 29"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M28.3333 14.1667C28.3333 6.34667 21.9867 0 14.1667 0C6.34667 0 0 6.34667 0 14.1667C0 21.0233 4.87333 26.7325 11.3333 28.05V18.4167H8.5V14.1667H11.3333V10.625C11.3333 7.89083 13.5575 5.66667 16.2917 5.66667H19.8333V9.91667H17C16.2208 9.91667 15.5833 10.5542 15.5833 11.3333V14.1667H19.8333V18.4167H15.5833V28.2625C22.7375 27.5542 28.3333 21.5192 28.3333 14.1667Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const ThreadsIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="18"

    height="22"

    viewBox="0 0 18 22"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M17.4167 6.92196C15.5768 0.0771293 9.25 0.505296 9.25 0.505296C9.25 0.505296 0.5 -0.0780373 0.5 10.9995C0.5 22.077 9.25 21.4948 9.25 21.4948C9.25 21.4948 14.451 21.8401 16.8333 16.9238C17.6115 14.7561 17.4167 10.422 9.83333 10.422C9.83333 10.422 6.33333 10.422 6.33333 13.3386C6.33333 14.4773 7.5 15.672 9.25 15.672C11 15.672 12.9495 14.4738 13.3333 12.172C14.5 5.17196 8.08333 4.58863 6.33333 7.5053"

      stroke="currentColor"

      strokeLinecap="round"

      strokeLinejoin="round"

    />

  </motion.svg>

);



export const InstagramIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="24"

    height="24"

    viewBox="0 0 24 24"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M6.76667 0H16.5667C20.3 0 23.3333 3.03333 23.3333 6.76667V16.5667C23.3333 18.3613 22.6204 20.0824 21.3514 21.3514C20.0824 22.6204 18.3613 23.3333 16.5667 23.3333H6.76667C3.03333 23.3333 0 20.3 0 16.5667V6.76667C0 4.97204 0.712915 3.25091 1.98191 1.98191C3.25091 0.712915 4.97204 0 6.76667 0ZM6.53333 2.33333C5.41942 2.33333 4.35114 2.77583 3.56349 3.56349C2.77583 4.35114 2.33333 5.41942 2.33333 6.53333V16.8C2.33333 19.1217 4.21167 21 6.53333 21H16.8C17.9139 21 18.9822 20.5575 19.7699 19.7699C20.5575 18.9822 21 17.9139 21 16.8V6.53333C21 4.21167 19.1217 2.33333 16.8 2.33333H6.53333ZM17.7917 4.08333C18.1784 4.08333 18.5494 4.23698 18.8229 4.51047C19.0964 4.78396 19.25 5.15489 19.25 5.54167C19.25 5.92844 19.0964 6.29937 18.8229 6.57286C18.5494 6.84635 18.1784 7 17.7917 7C17.4049 7 17.034 6.84635 16.7605 6.57286C16.487 6.29937 16.3333 5.92844 16.3333 5.54167C16.3333 5.15489 16.487 4.78396 16.7605 4.51047C17.034 4.23698 17.4049 4.08333 17.7917 4.08333ZM11.6667 5.83333C13.2138 5.83333 14.6975 6.44792 15.7915 7.54188C16.8854 8.63584 17.5 10.1196 17.5 11.6667C17.5 13.2138 16.8854 14.6975 15.7915 15.7915C14.6975 16.8854 13.2138 17.5 11.6667 17.5C10.1196 17.5 8.63584 16.8854 7.54188 15.7915C6.44792 14.6975 5.83333 13.2138 5.83333 11.6667C5.83333 10.1196 6.44792 8.63584 7.54188 7.54188C8.63584 6.44792 10.1196 5.83333 11.6667 5.83333ZM11.6667 8.16667C10.7384 8.16667 9.84817 8.53542 9.19179 9.19179C8.53542 9.84817 8.16667 10.7384 8.16667 11.6667C8.16667 12.5949 8.53542 13.4852 9.19179 14.1415C9.84817 14.7979 10.7384 15.1667 11.6667 15.1667C12.5949 15.1667 13.4852 14.7979 14.1415 14.1415C14.7979 13.4852 15.1667 12.5949 15.1667 11.6667C15.1667 10.7384 14.7979 9.84817 14.1415 9.19179C13.4852 8.53542 12.5949 8.16667 11.6667 8.16667Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const TikTokIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="19"

    height="21"

    viewBox="0 0 19 21"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M14.5133 3.29C13.716 2.37945 13.2765 1.2103 13.2767 0H9.67167V14.4667C9.64444 15.2497 9.31411 15.9916 8.75036 16.5357C8.18661 17.0799 7.43353 17.3838 6.65 17.3833C4.99333 17.3833 3.61667 16.03 3.61667 14.35C3.61667 12.3433 5.55333 10.8383 7.54833 11.4567V7.77C3.52333 7.23333 0 10.36 0 14.35C0 18.235 3.22 21 6.63833 21C10.3017 21 13.2767 18.025 13.2767 14.35V7.01167C14.7385 8.06149 16.4936 8.62475 18.2933 8.62167V5.01667C18.2933 5.01667 16.1 5.12167 14.5133 3.29Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const YouTubeIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="29"

    height="21"

    viewBox="0 0 29 21"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M28.0501 3.13163C27.7206 1.8991 26.7497 0.928222 25.5172 0.59876C23.2832 -1.51787e-07 14.3244 0 14.3244 0C14.3244 0 5.36561 -1.51787e-07 3.13164 0.59876C1.8991 0.928222 0.928222 1.8991 0.59876 3.13163C0 5.36561 0 10.0271 0 10.0271C0 10.0271 0 14.6886 0.59876 16.9225C0.928222 18.1551 1.8991 19.126 3.13164 19.4554C5.36561 20.0542 14.3244 20.0542 14.3244 20.0542C14.3244 20.0542 23.2832 20.0542 25.5172 19.4554C26.7497 19.126 27.7206 18.1551 28.0501 16.9225C28.6488 14.6886 28.6488 10.0271 28.6488 10.0271C28.6488 10.0271 28.6488 5.36561 28.0501 3.13163ZM11.4595 14.3244V5.72977L18.9025 10.0271L11.4595 14.3244Z"

      fill="currentColor"

    />

  </motion.svg>

);



interface SocialCloudProps

  extends

    Omit<React.HTMLAttributes<HTMLDivElement>, keyof MotionProps>,

    MotionProps {}



export function SocialCloud({ className, ...props }: SocialCloudProps) {

  return (

    <motion.div

      className={cn(

        "flex flex-wrap items-center gap-6 cursor-pointer",

        className,

      )}

      {...props}

    >

      <XIcon />

      <LinkedInIcon />

      <FacebookIcon />

      <ThreadsIcon />

      <InstagramIcon />

      <TikTokIcon />

      <YouTubeIcon />

    </motion.div>

  );

}
```
---

## Footer Section 6 <a name="footer-section-6"></a>

Solace UI footer section block variation 6.

- **Registry Key**: `footer-section-6`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/footer-section-6.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `@paper-design/shaders-react`

#### Component Source Code
##### File: `src/registry/blocks/footer-section/six/index.tsx`
```tsx
"use client";

import React from "react";

import { HalftoneDots } from "@paper-design/shaders-react";

import Link from "next/link";

import { SocialCloud } from "@/registry/blocks/footer-section/social-cloud";



const companyName = "SolaceUI";



const SolaceUILogo = ({ className }: { className?: string }) => {

  return (

    <svg className={className} width="64" height="38" viewBox="0 0 64 38" fill="none" xmlns="http://www.w3.org/2000/svg">

      <path d="M0 20.1032L39.8387 20.1032C44.7808 20.1032 48.7871 24.1095 48.7871 29.0516C48.7871 33.9937 44.7808 38 39.8387 38L1.56459e-06 38L0 20.1032Z" fill="currentColor" />

      <path d="M63.4968 17.8968L23.6581 17.8968C18.716 17.8968 14.7097 13.8904 14.7097 8.94839C14.7097 4.00633 18.716 0 23.6581 0L63.4968 0V17.8968Z" fill="currentColor" />

    </svg>

  )

}



const footerLinks = [

  {

    title: "Product",

    links: [

      { name: "Features", href: "#" },

      { name: "Solution", href: "#" },

      { name: "Customers", href: "#" },

      { name: "Pricing", href: "#" },

      { name: "Help", href: "#" },

      { name: "Terms", href: "#" },

    ],

  },

  {

    title: "Company",

    links: [

      { name: "About", href: "#" },

      { name: "Careers", href: "#" },

      { name: "Blogs", href: "#" },

      { name: "Pricing", href: "#" },

      { name: "Contact", href: "#" },

      { name: "Privacy", href: "#" },

    ],

  },

  {

    title: "Social",

    links: [

      { name: "X", href: "#" },

      { name: "LinkedIn", href: "#" },

      { name: "Facebook", href: "#" },

      { name: "Threads", href: "#" },

      { name: "Instagram", href: "#" },

      { name: "Youtube", href: "#" },

    ],

  },

];



export default function FooterSection6() {

  return (

    <footer className="w-full relative overflow-hidden antialiased [font-synthesis:none] bg-white text-black">

      {/* Background Shader */}

      <div className="absolute inset-0 z-0 pointer-events-none opacity-40">

        <HalftoneDots

          contrast={0.01}

          originalColors

          inverted={false}

          grid="hex"

          radius={2}

          size={0.16}

          scale={1}

          type="classic"

          fit="cover"

          image="https://res.cloudinary.com/harshitproject/image/upload/v1778511966/renaissance.webp"

          colorFront="#FFFFFF"

          colorBack="#00000000"

          className="w-full h-full bg-white"

        />

      </div>



      {/* White Fade at Bottom */}

      <div className="absolute bottom-0 left-0 right-0 h-48 bg-gradient-to-t from-white via-white/80 to-transparent z-0 pointer-events-none" />



      {/* Content */}

      <div className="relative z-10 max-w-7xl mx-auto px-6 md:px-12 lg:px-24 pt-24 pb-12 flex flex-col min-h-[400px]">

        {/* Top Section */}

        <div className="flex flex-col lg:flex-row justify-between gap-16 lg:gap-8 flex-1">

          {/* Left Side */}

          <div className="flex flex-col justify-between max-w-sm w-full">

            <div className="flex flex-col gap-4 lg:-mt-1.5">

              {/* Logo and Name inline */}

              <div className="flex items-center gap-2.5">

                <SolaceUILogo className="w-9 h-auto shrink-0 text-black" />

                <span className="font-semibold text-black text-2xl md:text-[26px] tracking-tight">{companyName}</span>

              </div>

              <h2 className="text-black/80 text-lg md:text-[20px] font-medium leading-snug">

                Ship Tastefully Crafted<br />Marketing Pages

              </h2>

            </div>



            <div className="mt-12 lg:mt-auto pb-1">

              <SocialCloud className="text-black/70 gap-4" />

            </div>

          </div>



          {/* Right Side - Links */}

          <div className="flex gap-12 md:gap-24 flex-wrap lg:flex-nowrap">

            {footerLinks.map((section) => (

              <div key={section.title} className="flex flex-col gap-5">

                <h3 className="text-black font-medium text-[20px] md:text-[22px]">

                  {section.title}

                </h3>

                <ul className="flex flex-col gap-3 md:gap-4">

                  {section.links.map((link) => (

                    <li key={link.name}>

                      <Link

                        href={link.href}

                        className="text-black/70 hover:text-black transition-colors text-[16px] md:text-[17px]"

                      >

                        {link.name}

                      </Link>

                    </li>

                  ))}

                </ul>

              </div>

            ))}

          </div>

        </div>



        {/* Bottom Centered Copyright */}

        <div className="mt-20 md:mt-24 w-full flex justify-center relative z-10">

          <p className="font-light text-black/70 text-sm md:text-[15px]">

            © 2026 {companyName}, All rights reserved

          </p>

        </div>

      </div>

    </footer>

  );

}
```
##### File: `src/registry/blocks/footer-section/social-cloud.tsx`
```tsx
"use client";

import React from "react";

import { cn } from "@/lib/utils";

import { motion, MotionProps } from "motion/react";



interface SocialIconProps extends React.SVGProps<SVGSVGElement> {}



export const XIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="23"

    height="20"

    viewBox="0 0 23 20"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M9.46617 12.6219L14.625 19.5H22.2083L13.6955 8.14883L20.7783 0H17.9075L12.3641 6.3765L7.58333 0H0L8.13583 10.8496L0.6175 19.5H3.48833L9.46617 12.6219ZM15.7083 17.3333L4.33333 2.16667H6.5L17.875 17.3333H15.7083Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const LinkedInIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="23"

    height="23"

    viewBox="0 0 23 23"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M20 0C20.663 0 21.2989 0.263392 21.7678 0.732233C22.2366 1.20107 22.5 1.83696 22.5 2.5V20C22.5 20.663 22.2366 21.2989 21.7678 21.7678C21.2989 22.2366 20.663 22.5 20 22.5H2.5C1.83696 22.5 1.20107 22.2366 0.732233 21.7678C0.263392 21.2989 0 20.663 0 20V2.5C0 1.83696 0.263392 1.20107 0.732233 0.732233C1.20107 0.263392 1.83696 0 2.5 0H20ZM19.375 19.375V12.75C19.375 11.6692 18.9457 10.6328 18.1815 9.86854C17.4172 9.10433 16.3808 8.675 15.3 8.675C14.2375 8.675 13 9.325 12.4 10.3V8.9125H8.9125V19.375H12.4V13.2125C12.4 12.25 13.175 11.4625 14.1375 11.4625C14.6016 11.4625 15.0467 11.6469 15.3749 11.9751C15.7031 12.3033 15.8875 12.7484 15.8875 13.2125V19.375H19.375ZM4.85 6.95C5.40695 6.95 5.9411 6.72875 6.33492 6.33492C6.72875 5.9411 6.95 5.40695 6.95 4.85C6.95 3.6875 6.0125 2.7375 4.85 2.7375C4.28973 2.7375 3.75241 2.96007 3.35624 3.35624C2.96007 3.75241 2.7375 4.28973 2.7375 4.85C2.7375 6.0125 3.6875 6.95 4.85 6.95ZM6.5875 19.375V8.9125H3.125V19.375H6.5875Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const FacebookIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="29"

    height="29"

    viewBox="0 0 29 29"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M28.3333 14.1667C28.3333 6.34667 21.9867 0 14.1667 0C6.34667 0 0 6.34667 0 14.1667C0 21.0233 4.87333 26.7325 11.3333 28.05V18.4167H8.5V14.1667H11.3333V10.625C11.3333 7.89083 13.5575 5.66667 16.2917 5.66667H19.8333V9.91667H17C16.2208 9.91667 15.5833 10.5542 15.5833 11.3333V14.1667H19.8333V18.4167H15.5833V28.2625C22.7375 27.5542 28.3333 21.5192 28.3333 14.1667Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const ThreadsIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="18"

    height="22"

    viewBox="0 0 18 22"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M17.4167 6.92196C15.5768 0.0771293 9.25 0.505296 9.25 0.505296C9.25 0.505296 0.5 -0.0780373 0.5 10.9995C0.5 22.077 9.25 21.4948 9.25 21.4948C9.25 21.4948 14.451 21.8401 16.8333 16.9238C17.6115 14.7561 17.4167 10.422 9.83333 10.422C9.83333 10.422 6.33333 10.422 6.33333 13.3386C6.33333 14.4773 7.5 15.672 9.25 15.672C11 15.672 12.9495 14.4738 13.3333 12.172C14.5 5.17196 8.08333 4.58863 6.33333 7.5053"

      stroke="currentColor"

      strokeLinecap="round"

      strokeLinejoin="round"

    />

  </motion.svg>

);



export const InstagramIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="24"

    height="24"

    viewBox="0 0 24 24"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M6.76667 0H16.5667C20.3 0 23.3333 3.03333 23.3333 6.76667V16.5667C23.3333 18.3613 22.6204 20.0824 21.3514 21.3514C20.0824 22.6204 18.3613 23.3333 16.5667 23.3333H6.76667C3.03333 23.3333 0 20.3 0 16.5667V6.76667C0 4.97204 0.712915 3.25091 1.98191 1.98191C3.25091 0.712915 4.97204 0 6.76667 0ZM6.53333 2.33333C5.41942 2.33333 4.35114 2.77583 3.56349 3.56349C2.77583 4.35114 2.33333 5.41942 2.33333 6.53333V16.8C2.33333 19.1217 4.21167 21 6.53333 21H16.8C17.9139 21 18.9822 20.5575 19.7699 19.7699C20.5575 18.9822 21 17.9139 21 16.8V6.53333C21 4.21167 19.1217 2.33333 16.8 2.33333H6.53333ZM17.7917 4.08333C18.1784 4.08333 18.5494 4.23698 18.8229 4.51047C19.0964 4.78396 19.25 5.15489 19.25 5.54167C19.25 5.92844 19.0964 6.29937 18.8229 6.57286C18.5494 6.84635 18.1784 7 17.7917 7C17.4049 7 17.034 6.84635 16.7605 6.57286C16.487 6.29937 16.3333 5.92844 16.3333 5.54167C16.3333 5.15489 16.487 4.78396 16.7605 4.51047C17.034 4.23698 17.4049 4.08333 17.7917 4.08333ZM11.6667 5.83333C13.2138 5.83333 14.6975 6.44792 15.7915 7.54188C16.8854 8.63584 17.5 10.1196 17.5 11.6667C17.5 13.2138 16.8854 14.6975 15.7915 15.7915C14.6975 16.8854 13.2138 17.5 11.6667 17.5C10.1196 17.5 8.63584 16.8854 7.54188 15.7915C6.44792 14.6975 5.83333 13.2138 5.83333 11.6667C5.83333 10.1196 6.44792 8.63584 7.54188 7.54188C8.63584 6.44792 10.1196 5.83333 11.6667 5.83333ZM11.6667 8.16667C10.7384 8.16667 9.84817 8.53542 9.19179 9.19179C8.53542 9.84817 8.16667 10.7384 8.16667 11.6667C8.16667 12.5949 8.53542 13.4852 9.19179 14.1415C9.84817 14.7979 10.7384 15.1667 11.6667 15.1667C12.5949 15.1667 13.4852 14.7979 14.1415 14.1415C14.7979 13.4852 15.1667 12.5949 15.1667 11.6667C15.1667 10.7384 14.7979 9.84817 14.1415 9.19179C13.4852 8.53542 12.5949 8.16667 11.6667 8.16667Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const TikTokIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="19"

    height="21"

    viewBox="0 0 19 21"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: -5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M14.5133 3.29C13.716 2.37945 13.2765 1.2103 13.2767 0H9.67167V14.4667C9.64444 15.2497 9.31411 15.9916 8.75036 16.5357C8.18661 17.0799 7.43353 17.3838 6.65 17.3833C4.99333 17.3833 3.61667 16.03 3.61667 14.35C3.61667 12.3433 5.55333 10.8383 7.54833 11.4567V7.77C3.52333 7.23333 0 10.36 0 14.35C0 18.235 3.22 21 6.63833 21C10.3017 21 13.2767 18.025 13.2767 14.35V7.01167C14.7385 8.06149 16.4936 8.62475 18.2933 8.62167V5.01667C18.2933 5.01667 16.1 5.12167 14.5133 3.29Z"

      fill="currentColor"

    />

  </motion.svg>

);



export const YouTubeIcon = ({ className, ...props }: SocialIconProps) => (

  <motion.svg

    className={cn("h-5 w-5", className)}

    width="29"

    height="21"

    viewBox="0 0 29 21"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    whileHover={{ scale: 1.2, rotate: 5 }}

    whileTap={{ scale: 0.9 }}

    transition={{ type: "spring", stiffness: 400, damping: 17 }}

  >

    <path

      d="M28.0501 3.13163C27.7206 1.8991 26.7497 0.928222 25.5172 0.59876C23.2832 -1.51787e-07 14.3244 0 14.3244 0C14.3244 0 5.36561 -1.51787e-07 3.13164 0.59876C1.8991 0.928222 0.928222 1.8991 0.59876 3.13163C0 5.36561 0 10.0271 0 10.0271C0 10.0271 0 14.6886 0.59876 16.9225C0.928222 18.1551 1.8991 19.126 3.13164 19.4554C5.36561 20.0542 14.3244 20.0542 14.3244 20.0542C14.3244 20.0542 23.2832 20.0542 25.5172 19.4554C26.7497 19.126 27.7206 18.1551 28.0501 16.9225C28.6488 14.6886 28.6488 10.0271 28.6488 10.0271C28.6488 10.0271 28.6488 5.36561 28.0501 3.13163ZM11.4595 14.3244V5.72977L18.9025 10.0271L11.4595 14.3244Z"

      fill="currentColor"

    />

  </motion.svg>

);



interface SocialCloudProps

  extends

    Omit<React.HTMLAttributes<HTMLDivElement>, keyof MotionProps>,

    MotionProps {}



export function SocialCloud({ className, ...props }: SocialCloudProps) {

  return (

    <motion.div

      className={cn(

        "flex flex-wrap items-center gap-6 cursor-pointer",

        className,

      )}

      {...props}

    >

      <XIcon />

      <LinkedInIcon />

      <FacebookIcon />

      <ThreadsIcon />

      <InstagramIcon />

      <TikTokIcon />

      <YouTubeIcon />

    </motion.div>

  );

}
```
---

## Hero Section 1 <a name="hero-section-1"></a>

Solace UI hero section block variation 1.

- **Registry Key**: `hero-section-1`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/hero-section-1.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`
- **Registry Dependencies**:
  - `button`

#### Component Source Code
##### File: `src/registry/blocks/hero-section/one/index.tsx`
```tsx
"use client";



import React from "react";

import Link from "next/link";

import { ArrowRight } from "lucide-react";

import { Button } from "@/components/ui/button"; // Adjust to your path

import { TextEffect } from "@/registry/blocks/hero-section/text-effect";
import { AnimatedGroup } from "@/components/ui/animated-group"; // Adjust to your path

import CircularGallery from "@/registry/blocks/hero-section/one/circular-gallery";
import { Variants } from "motion/react";



const transitionVariants: { item: Variants } = {

  item: {

    hidden: {

      opacity: 0,

      filter: "blur(12px)",

      y: 12,

    },

    visible: {

      opacity: 1,

      filter: "blur(0px)",

      y: 0,

      transition: {

        type: "spring",

        bounce: 0.3,

        duration: 1.5,

      },

    },

  },

};



export default function HeroSection1() {

  const defaultItems = [

    {

      image: `https://res.cloudinary.com/harshitproject/image/upload/v1746774805/Behance-screen.png`,

      text: "Behance",

    },

    {

      image: `https://res.cloudinary.com/harshitproject/image/upload/v1746774805/Notion-screen.png`,

      text: "Notion",

    },

    {

      image: `https://res.cloudinary.com/harshitproject/image/upload/v1746774806/One-screen.png`,

      text: "One",

    },

    {

      image: `https://res.cloudinary.com/harshitproject/image/upload/v1746774807/Reddit-nj7hwh.png`,

      text: "Reddit",

    },

    {

      image: `https://res.cloudinary.com/harshitproject/image/upload/v1746774805/Behance-screen.png`,

      text: "Behance",

    },

    {

      image: `https://res.cloudinary.com/harshitproject/image/upload/v1746774805/Notion-screen.png`,

      text: "Notion",

    },

  ];



  return (

    <>

      <main className="z-10">

        <section>

          <div className="relative overflow-hidden pt-2 md:pt-4">

            {/* Constrained content for the hero text */}

            <div className="mx-auto max-w-7xl px-6">

              <div className="text-center sm:mx-auto lg:mr-auto lg:mt-0">

                <AnimatedGroup variants={transitionVariants}>

                  <Link

                    href="#link"

                    className="hover:bg-background dark:hover:border-t-border bg-muted group mx-auto flex w-fit items-center gap-4 rounded-full border p-1 pl-4 shadow-md shadow-zinc-950/5 transition-colors duration-300 dark:border-t-white/5 dark:shadow-zinc-950"

                  >

                    <span className="text-foreground text-sm">

                      Introducing Exclusive Curated UI Designs

                    </span>

                    <span className="dark:border-background block h-4 w-0.5 border-l bg-white dark:bg-zinc-700"></span>

                    <div className="bg-background group-hover:bg-muted size-6 overflow-hidden rounded-full duration-500">

                      <div className="flex w-12 -translate-x-1/2 duration-500 ease-in-out group-hover:translate-x-0">

                        <span className="flex size-6">

                          <ArrowRight className="m-auto size-3 dark:text-white text-black" />

                        </span>

                        <span className="flex size-6">

                          <ArrowRight className="m-auto size-3 dark:text-white text-black" />

                        </span>

                      </div>

                    </div>

                  </Link>

                </AnimatedGroup>



                <TextEffect

                  preset="fade-in-blur"

                  speedSegment={0.3}

                  as="h1"

                  className="mt-8 text-balance text-6xl dark:text-white text-black md:text-7xl lg:mt-16 xl:text-[5.25rem] "

                >

                  The Ultimate Library of Curated UI Designs

                </TextEffect>

                <TextEffect

                  per="line"

                  preset="fade-in-blur"

                  speedSegment={0.3}

                  delay={0.5}

                  as="p"

                  className="mx-auto mt-8 max-w-2xl text-balance text-lg dark:text-white/60 text-black"

                >

                  Explore hundreds of beautifully crafted user interfaces from

                  top designers around the world. Choose our handpicked gallery

                  of modern UI inspiration.

                </TextEffect>



                <AnimatedGroup

                  variants={{

                    container: {

                      visible: {

                        transition: {

                          staggerChildren: 0.05,

                          delayChildren: 0.75,

                        },

                      },

                    },

                    ...transitionVariants,

                  }}

                  className="mt-12 flex flex-col items-center justify-center gap-2 md:flex-row"

                >

                  <div key={1} className="">

                    <Button

                      asChild

                      size="lg"

                      className="rounded-md px-5 text-base dark:bg-white bg-black"

                    >

                      <Link href="#link">

                        <span className="text-nowra dark:text-black text-white">

                          Browse Designs

                        </span>

                      </Link>

                    </Button>

                  </div>

                  <Button

                    key={2}

                    asChild

                    size="lg"

                    variant="ghost"

                    className="rounded-md px-5 bg-foreground/10"

                  >

                    <Link href="#link">

                      <span className="text-nowrap dark:text-white text-black">

                        Submit Your Work

                      </span>

                    </Link>

                  </Button>

                </AnimatedGroup>

              </div>

            </div>



            <div className="mt-12 w-screen relative left-1/2 right-1/2 ml-[-50vw] mr-[-50vw]">

              <CircularGallery

                items={defaultItems}

                bend={-9}

                textColor="#ffffff"

                borderRadius={0.03}

              />

            </div>

          </div>

        </section>

      </main>

    </>

  );

}
```
##### File: `src/registry/blocks/hero-section/one/circular-gallery.tsx`
```tsx
// Component from React Bits



// Install the dependency

// npm install ogl



"use client";

import { useRef, useEffect } from "react";

import {

    Renderer,

    Camera,

    Transform,

    Plane,

    Mesh,

    Program,

    Texture,

} from "ogl";



type GL = Renderer["gl"];



/**

 * Simple utility: debounce

 */

function debounce<T extends (...args: any[]) => void>(func: T, wait: number) {

    let timeout: number;

    return function (this: any, ...args: Parameters<T>) {

        window.clearTimeout(timeout);

        timeout = window.setTimeout(() => func.apply(this, args), wait);

    };

}



/**

 * Linear interpolation

 */

function lerp(p1: number, p2: number, t: number): number {

    return p1 + (p2 - p1) * t;

}



/**

 * Auto-bind all class methods

 */

function autoBind(instance: any): void {

    const proto = Object.getPrototypeOf(instance);

    Object.getOwnPropertyNames(proto).forEach((key) => {

        if (key !== "constructor" && typeof instance[key] === "function") {

            instance[key] = instance[key].bind(instance);

        }

    });

}



/**

 * Extract a numeric px font size from a string like "bold 30px sans-serif"

 */

function getFontSize(font: string): number {

    const match = font.match(/(\d+)px/);

    return match ? parseInt(match[1], 10) : 30;

}



/**

 * Create a texture from text (to display image titles, etc.)

 */

function createTextTexture(

    gl: GL,

    text: string,

    font: string = "bold 30px monospace",

    color: string = "black"

): { texture: Texture; width: number; height: number } {

    const canvas = document.createElement("canvas");

    const context = canvas.getContext("2d");

    if (!context) throw new Error("Could not get 2d context");



    context.font = font;

    const metrics = context.measureText(text);

    const textWidth = Math.ceil(metrics.width);

    const fontSize = getFontSize(font);

    const textHeight = Math.ceil(fontSize * 1.2);



    canvas.width = textWidth + 20;

    canvas.height = textHeight + 20;



    context.font = font;

    context.fillStyle = color;

    context.textBaseline = "middle";

    context.textAlign = "center";

    context.clearRect(0, 0, canvas.width, canvas.height);

    context.fillText(text, canvas.width / 2, canvas.height / 2);



    const texture = new Texture(gl, { generateMipmaps: false });

    texture.image = canvas;

    return { texture, width: canvas.width, height: canvas.height };

}



interface TitleProps {

    gl: GL;

    plane: Mesh;

    renderer: Renderer;

    text: string;

    textColor?: string;

    font?: string;

}



/**

 * Displays a text label beneath each image

 */

class Title {

    gl: GL;

    plane: Mesh;

    renderer: Renderer;

    text: string;

    textColor: string;

    font: string;

    mesh!: Mesh;



    constructor({

        gl,

        plane,

        renderer,

        text,

        textColor = "#545050",

        font = "30px sans-serif",

    }: TitleProps) {

        autoBind(this);

        this.gl = gl;

        this.plane = plane;

        this.renderer = renderer;

        this.text = text;

        this.textColor = textColor;

        this.font = font;

        this.createMesh();

    }



    createMesh() {

        const { texture, width, height } = createTextTexture(

            this.gl,

            this.text,

            this.font,

            this.textColor

        );

        const geometry = new Plane(this.gl);

        const program = new Program(this.gl, {

            vertex: `

        attribute vec3 position;

        attribute vec2 uv;

        uniform mat4 modelViewMatrix;

        uniform mat4 projectionMatrix;

        varying vec2 vUv;

        void main() {

          vUv = uv;

          gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);

        }

      `,

            fragment: `

        precision highp float;

        uniform sampler2D tMap;

        varying vec2 vUv;

        void main() {

          vec4 color = texture2D(tMap, vUv);

          // Discard very transparent fragments

          if (color.a < 0.1) discard;

          gl_FragColor = color;

        }

      `,

            uniforms: { tMap: { value: texture } },

            transparent: true,

        });

        this.mesh = new Mesh(this.gl, { geometry, program });



        const aspect = width / height;

        // Make text ~15% of plane height

        const textHeightScaled = this.plane.scale.y * 0.15;

        const textWidthScaled = textHeightScaled * aspect;

        this.mesh.scale.set(textWidthScaled, textHeightScaled, 1);



        // Position the text slightly below the plane

        this.mesh.position.y =

            -this.plane.scale.y * 0.5 - textHeightScaled * 0.5 - 0.05;



        this.mesh.setParent(this.plane);

    }

}



interface ScreenSize {

    width: number;

    height: number;

}



interface Viewport {

    width: number;

    height: number;

}



interface MediaProps {

    geometry: Plane;

    gl: GL;

    image: string;

    index: number;

    length: number;

    renderer: Renderer;

    scene: Transform;

    screen: ScreenSize;

    text: string;

    viewport: Viewport;

    bend: number;

    textColor: string;

    borderRadius?: number;

    font?: string;

}



/**

 * Represents a single image + text in the circular gallery

 */

class Media {

    extra: number = 0;

    geometry: Plane;

    gl: GL;

    image: string;

    index: number;

    length: number;

    renderer: Renderer;

    scene: Transform;

    screen: ScreenSize;

    text: string;

    viewport: Viewport;

    bend: number;

    textColor: string;

    borderRadius: number;

    font?: string;

    program!: Program;

    plane!: Mesh;

    title!: Title;

    scale!: number;

    padding!: number;

    width!: number;

    widthTotal!: number;

    x!: number;

    speed: number = 0;

    isBefore: boolean = false;

    isAfter: boolean = false;



    constructor({

        geometry,

        gl,

        image,

        index,

        length,

        renderer,

        scene,

        screen,

        text,

        viewport,

        bend,

        textColor,

        borderRadius = 0,

        font,

    }: MediaProps) {

        this.geometry = geometry;

        this.gl = gl;

        this.image = image;

        this.index = index;

        this.length = length;

        this.renderer = renderer;

        this.scene = scene;

        this.screen = screen;

        this.text = text;

        this.viewport = viewport;

        this.bend = bend;

        this.textColor = textColor;

        this.borderRadius = borderRadius;

        this.font = font;



        this.createShader();

        this.createMesh();

        this.createTitle();

        this.onResize();

    }



    createShader() {

        const texture = new Texture(this.gl, { generateMipmaps: false });

        this.program = new Program(this.gl, {

            depthTest: false,

            depthWrite: false,

            vertex: `

        precision highp float;

        attribute vec3 position;

        attribute vec2 uv;

        uniform mat4 modelViewMatrix;

        uniform mat4 projectionMatrix;

        uniform float uTime;

        uniform float uSpeed;

        varying vec2 vUv;

        void main() {

          vUv = uv;

          vec3 p = position;

          // Subtle wave

          p.z = (sin(p.x * 4.0 + uTime) * 1.5 + cos(p.y * 2.0 + uTime) * 1.5) * (0.1 + uSpeed * 0.5);

          gl_Position = projectionMatrix * modelViewMatrix * vec4(p, 1.0);

        }

      `,

            fragment: `

        precision highp float;

        uniform vec2 uImageSizes;

        uniform vec2 uPlaneSizes;

        uniform sampler2D tMap;

        uniform float uBorderRadius;

        varying vec2 vUv;

        

        // Signed Distance Function for a rounded box

        float roundedBoxSDF(vec2 p, vec2 b, float r) {

          vec2 d = abs(p) - b;

          return length(max(d, vec2(0.0))) + min(max(d.x, d.y), 0.0) - r;

        }

        

        void main() {

          // Fit the image while preserving aspect ratio

          vec2 ratio = vec2(

            min((uPlaneSizes.x / uPlaneSizes.y) / (uImageSizes.x / uImageSizes.y), 1.0),

            min((uPlaneSizes.y / uPlaneSizes.x) / (uImageSizes.y / uImageSizes.x), 1.0)

          );

          vec2 uv = vec2(

            vUv.x * ratio.x + (1.0 - ratio.x) * 0.5,

            vUv.y * ratio.y + (1.0 - ratio.y) * 0.5

          );

          

          vec4 color = texture2D(tMap, uv);

          

          // Rounded corners in UV space

          float d = roundedBoxSDF(vUv - 0.5, vec2(0.5 - uBorderRadius), uBorderRadius);

          if(d > 0.0) {

            discard;

          }

          

          gl_FragColor = vec4(color.rgb, 1.0);

        }

      `,

            uniforms: {

                tMap: { value: texture },

                uPlaneSizes: { value: [0, 0] },

                uImageSizes: { value: [0, 0] },

                uSpeed: { value: 0 },

                uTime: { value: 100 * Math.random() },

                uBorderRadius: { value: this.borderRadius },

            },

            transparent: true,

        });



        const img = new Image();

        img.crossOrigin = "anonymous";

        img.src = this.image;

        img.onload = () => {

            texture.image = img;

            this.program.uniforms.uImageSizes.value = [

                img.naturalWidth,

                img.naturalHeight,

            ];

        };

    }



    createMesh() {

        this.plane = new Mesh(this.gl, {

            geometry: this.geometry,

            program: this.program,

        });

        this.plane.setParent(this.scene);

    }



    createTitle() {

        this.title = new Title({

            gl: this.gl,

            plane: this.plane,

            renderer: this.renderer,

            text: this.text,

            textColor: this.textColor,

            font: this.font,

        });

    }



    update(

        scroll: { current: number; last: number },

        direction: "right" | "left"

    ) {

        // Move horizontally

        this.plane.position.x = this.x - scroll.current - this.extra;



        // Bend the carousel

        const x = this.plane.position.x;

        const H = this.viewport.width / 2;

        if (this.bend === 0) {

            this.plane.position.y = 0;

            this.plane.rotation.z = 0;

        } else {

            const B_abs = Math.abs(this.bend);

            const R = (H * H + B_abs * B_abs) / (2 * B_abs);

            const effectiveX = Math.min(Math.abs(x), H);

            const arc = R - Math.sqrt(R * R - effectiveX * effectiveX);



            if (this.bend > 0) {

                this.plane.position.y = -arc;

                this.plane.rotation.z = -Math.sign(x) * Math.asin(effectiveX / R);

            } else {

                this.plane.position.y = arc;

                this.plane.rotation.z = Math.sign(x) * Math.asin(effectiveX / R);

            }

        }



        // Animate wave

        this.speed = scroll.current - scroll.last;

        this.program.uniforms.uTime.value += 0.04;

        this.program.uniforms.uSpeed.value = this.speed;



        // Looping logic

        const planeOffset = this.plane.scale.x / 2;

        const viewportOffset = this.viewport.width / 2;

        this.isBefore = this.plane.position.x + planeOffset < -viewportOffset;

        this.isAfter = this.plane.position.x - planeOffset > viewportOffset;



        if (direction === "right" && this.isBefore) {

            this.extra -= this.widthTotal;

            this.isBefore = this.isAfter = false;

        }

        if (direction === "left" && this.isAfter) {

            this.extra += this.widthTotal;

            this.isBefore = this.isAfter = false;

        }

    }



    onResize({

        screen,

        viewport,

    }: { screen?: ScreenSize; viewport?: Viewport } = {}) {

        if (screen) this.screen = screen;

        if (viewport) {

            this.viewport = viewport;

        }

        // Scale the images based on screen size

        this.scale = this.screen.height / 1500;

        this.plane.scale.y =

            (this.viewport.height * (900 * this.scale)) / this.screen.height;

        this.plane.scale.x =

            (this.viewport.width * (700 * this.scale)) / this.screen.width;



        this.program.uniforms.uPlaneSizes.value = [

            this.plane.scale.x,

            this.plane.scale.y,

        ];



        this.padding = 2;

        this.width = this.plane.scale.x + this.padding;

        this.widthTotal = this.width * this.length;

        this.x = this.width * this.index;

    }

}



interface AppConfig {

    items?: { image: string; text: string }[];

    bend?: number;

    textColor?: string;

    borderRadius?: number;

    font?: string;

}



/**

 * Main application controlling the scene, camera, and a set of Media items

 */

class App {

    container: HTMLElement;

    scroll: {

        ease: number;

        current: number;

        target: number;

        last: number;

        position?: number;

    };

    onCheckDebounce: (...args: any[]) => void;

    renderer!: Renderer;

    gl!: GL;

    camera!: Camera;

    scene!: Transform;

    planeGeometry!: Plane;

    medias: Media[] = [];

    mediasImages: { image: string; text: string }[] = [];

    screen!: { width: number; height: number };

    viewport!: { width: number; height: number };

    raf: number = 0;



    boundOnResize!: () => void;

    boundOnWheel!: () => void;

    boundOnTouchDown!: (e: MouseEvent | TouchEvent) => void;

    boundOnTouchMove!: (e: MouseEvent | TouchEvent) => void;

    boundOnTouchUp!: () => void;



    isDown: boolean = false;

    start: number = 0;



    constructor(

        container: HTMLElement,

        {

            items,

            bend = 1,

            textColor = "#ffffff",

            borderRadius = 0,

            font = "bold 30px DM Sans",

        }: AppConfig

    ) {

        document.documentElement.classList.remove("no-js");

        this.container = container;

        this.scroll = { ease: 0.05, current: 0, target: 0, last: 0 };

        this.onCheckDebounce = debounce(this.onCheck.bind(this), 200);



        this.createRenderer();

        this.createCamera();

        this.createScene();

        this.onResize();

        this.createGeometry();

        this.createMedias(items, bend, textColor, borderRadius, font);

        this.update();

        this.addEventListeners();

    }



    createRenderer() {

        this.renderer = new Renderer({ alpha: true });

        this.gl = this.renderer.gl;

        this.gl.clearColor(0, 0, 0, 0);

        this.container.appendChild(this.renderer.gl.canvas as HTMLCanvasElement);

    }



    createCamera() {

        this.camera = new Camera(this.gl);

        this.camera.fov = 45;

        this.camera.position.z = 20;

    }



    createScene() {

        this.scene = new Transform();

    }



    createGeometry() {

        // Increase segments so the wave looks smoother

        this.planeGeometry = new Plane(this.gl, {

            heightSegments: 50,

            widthSegments: 100,

        });

    }



    createMedias(

        items: { image: string; text: string }[] | undefined,

        bend: number = 1,

        textColor: string,

        borderRadius: number,

        font: string

    ) {

        // If no items provided, fallback to placeholders

        const defaultItems = [

            {

                image: `https://picsum.photos/seed/5/800/600?grayscale`,

                text: "Deep Diving",

            },

            {

                image: `https://picsum.photos/seed/16/800/600?grayscale`,

                text: "Train Track",

            },

            {

                image: `https://picsum.photos/seed/17/800/600?grayscale`,

                text: "Santorini",

            },

            {

                image: `https://picsum.photos/seed/8/800/600?grayscale`,

                text: "Blurry Lights",

            },

            {

                image: `https://picsum.photos/seed/9/800/600?grayscale`,

                text: "New York",

            },

            {

                image: `https://picsum.photos/seed/10/800/600?grayscale`,

                text: "Good Boy",

            },

            {

                image: `https://picsum.photos/seed/21/800/600?grayscale`,

                text: "Coastline",

            },

            {

                image: `https://picsum.photos/seed/12/800/600?grayscale`,

                text: "Palm Trees",

            },

        ];



        const galleryItems = items && items.length ? items : defaultItems;

        // Duplicate items for infinite loop effect

        this.mediasImages = galleryItems.concat(galleryItems);



        this.medias = this.mediasImages.map((data, index) => {

            return new Media({

                geometry: this.planeGeometry,

                gl: this.gl,

                image: data.image,

                index,

                length: this.mediasImages.length,

                renderer: this.renderer,

                scene: this.scene,

                screen: this.screen,

                text: data.text,

                viewport: this.viewport,

                bend,

                textColor,

                borderRadius,

                font,

            });

        });

    }



    onTouchDown(e: MouseEvent | TouchEvent) {

        this.isDown = true;

        this.scroll.position = this.scroll.current;

        this.start = "touches" in e ? e.touches[0].clientX : e.clientX;

    }



    onTouchMove(e: MouseEvent | TouchEvent) {

        if (!this.isDown) return;

        const x = "touches" in e ? e.touches[0].clientX : e.clientX;

        const distance = (this.start - x) * 0.05;

        this.scroll.target = (this.scroll.position ?? 0) + distance;

    }



    onTouchUp() {

        this.isDown = false;

        this.onCheck();

    }



    onWheel() {

        // Scroll horizontally

        this.scroll.target += 2;

        this.onCheckDebounce();

    }



    onCheck() {

        // Snap to nearest image

        if (!this.medias.length) return;

        const width = this.medias[0].width;

        const itemIndex = Math.round(Math.abs(this.scroll.target) / width);

        const item = width * itemIndex;

        this.scroll.target = this.scroll.target < 0 ? -item : item;

    }



    onResize() {

        this.screen = {

            width: this.container.clientWidth,

            height: this.container.clientHeight,

        };

        this.renderer.setSize(this.screen.width, this.screen.height);



        // Update camera

        this.camera.perspective({

            aspect: this.screen.width / this.screen.height,

        });



        // Convert FOV to radians

        const fov = (this.camera.fov * Math.PI) / 180;

        const height = 2 * Math.tan(fov / 2) * this.camera.position.z;

        const width = height * this.camera.aspect;

        this.viewport = { width, height };



        // Resize each Media

        this.medias.forEach((media) =>

            media.onResize({ screen: this.screen, viewport: this.viewport })

        );

    }



    update() {

        this.scroll.current = lerp(

            this.scroll.current,

            this.scroll.target,

            this.scroll.ease

        );



        const direction = this.scroll.current > this.scroll.last ? "right" : "left";

        this.medias.forEach((media) => media.update(this.scroll, direction));



        this.renderer.render({ scene: this.scene, camera: this.camera });

        this.scroll.last = this.scroll.current;

        this.raf = window.requestAnimationFrame(this.update.bind(this));

    }



    addEventListeners() {

        this.boundOnResize = this.onResize.bind(this);

        this.boundOnWheel = this.onWheel.bind(this);

        this.boundOnTouchDown = this.onTouchDown.bind(this);

        this.boundOnTouchMove = this.onTouchMove.bind(this);

        this.boundOnTouchUp = this.onTouchUp.bind(this);



        window.addEventListener("resize", this.boundOnResize);

        window.addEventListener("mousewheel", this.boundOnWheel);

        window.addEventListener("wheel", this.boundOnWheel);

        window.addEventListener("mousedown", this.boundOnTouchDown);

        window.addEventListener("mousemove", this.boundOnTouchMove);

        window.addEventListener("mouseup", this.boundOnTouchUp);

        window.addEventListener("touchstart", this.boundOnTouchDown);

        window.addEventListener("touchmove", this.boundOnTouchMove);

        window.addEventListener("touchend", this.boundOnTouchUp);

    }



    destroy() {

        window.cancelAnimationFrame(this.raf);

        window.removeEventListener("resize", this.boundOnResize);

        window.removeEventListener("mousewheel", this.boundOnWheel);

        window.removeEventListener("wheel", this.boundOnWheel);

        window.removeEventListener("mousedown", this.boundOnTouchDown);

        window.removeEventListener("mousemove", this.boundOnTouchMove);

        window.removeEventListener("mouseup", this.boundOnTouchUp);

        window.removeEventListener("touchstart", this.boundOnTouchDown);

        window.removeEventListener("touchmove", this.boundOnTouchMove);

        window.removeEventListener("touchend", this.boundOnTouchUp);



        if (this.renderer?.gl?.canvas?.parentNode) {

            this.renderer.gl.canvas.parentNode.removeChild(

                this.renderer.gl.canvas as HTMLCanvasElement

            );

        }

    }

}



interface CircularGalleryProps {

    items?: { image: string; text: string }[];

    bend?: number;

    textColor?: string;

    borderRadius?: number;

    font?: string;

}



/**

 * Public React component for the circular gallery

 */

export default function CircularGallery({

    items,

    bend = 3,

    textColor = "#ffffff",

    borderRadius = 0.05,

    font = "bold 30px DM Sans",

}: CircularGalleryProps) {

    const containerRef = useRef<HTMLDivElement>(null);



    useEffect(() => {

        if (!containerRef.current) return;

        const app = new App(containerRef.current, {

            items,

            bend,

            textColor,

            borderRadius,

            font,

        });



        return () => {

            app.destroy();

        };

    }, [items, bend, textColor, borderRadius, font]);



    return (

        <div

            className="w-full h-[400px] overflow-hidden cursor-grab active:cursor-grabbing"

            ref={containerRef}

        />

    );

}
```
##### File: `src/registry/blocks/hero-section/text-effect.tsx`
```tsx
// Component From Motion Primitives



"use client";

import { cn } from "@/lib/utils";

import {

    AnimatePresence,

    motion,

    TargetAndTransition,

    Transition,

    Variant,

    Variants,

} from "motion/react";

import React from "react";



export type PresetType = "blur" | "fade-in-blur" | "scale" | "fade" | "slide";



export type PerType = "word" | "char" | "line";



export type TextEffectProps = {

    children: string;

    per?: PerType;

    as?: keyof React.JSX.IntrinsicElements;

    variants?: {

        container?: Variants;

        item?: Variants;

    };

    className?: string;

    preset?: PresetType;

    delay?: number;

    speedReveal?: number;

    speedSegment?: number;

    trigger?: boolean;

    onAnimationComplete?: () => void;

    onAnimationStart?: () => void;

    segmentWrapperClassName?: string;

    containerTransition?: Transition;

    segmentTransition?: Transition;

    style?: React.CSSProperties;

};



const defaultStaggerTimes: Record<PerType, number> = {

    char: 0.03,

    word: 0.05,

    line: 0.1,

};



const defaultContainerVariants: Variants = {

    hidden: { opacity: 0 },

    visible: {

        opacity: 1,

        transition: {

            staggerChildren: 0.05,

        },

    },

    exit: {

        transition: { staggerChildren: 0.05, staggerDirection: -1 },

    },

};



const defaultItemVariants: Variants = {

    hidden: { opacity: 0 },

    visible: {

        opacity: 1,

    },

    exit: { opacity: 0 },

};



const presetVariants: Record<

    PresetType,

    { container: Variants; item: Variants }

> = {

    blur: {

        container: defaultContainerVariants,

        item: {

            hidden: { opacity: 0, filter: "blur(12px)" },

            visible: { opacity: 1, filter: "blur(0px)" },

            exit: { opacity: 0, filter: "blur(12px)" },

        },

    },

    "fade-in-blur": {

        container: defaultContainerVariants,

        item: {

            hidden: { opacity: 0, y: 20, filter: "blur(12px)" },

            visible: { opacity: 1, y: 0, filter: "blur(0px)" },

            exit: { opacity: 0, y: 20, filter: "blur(12px)" },

        },

    },

    scale: {

        container: defaultContainerVariants,

        item: {

            hidden: { opacity: 0, scale: 0 },

            visible: { opacity: 1, scale: 1 },

            exit: { opacity: 0, scale: 0 },

        },

    },

    fade: {

        container: defaultContainerVariants,

        item: {

            hidden: { opacity: 0 },

            visible: { opacity: 1 },

            exit: { opacity: 0 },

        },

    },

    slide: {

        container: defaultContainerVariants,

        item: {

            hidden: { opacity: 0, y: 20 },

            visible: { opacity: 1, y: 0 },

            exit: { opacity: 0, y: 20 },

        },

    },

};



const AnimationComponent: React.FC<{

    segment: string;

    variants: Variants;

    per: "line" | "word" | "char";

    segmentWrapperClassName?: string;

}> = React.memo(({ segment, variants, per, segmentWrapperClassName }) => {

    const content =

        per === "line" ? (

            <motion.span variants={variants} className="block">

                {segment}

            </motion.span>

        ) : per === "word" ? (

            <motion.span

                aria-hidden="true"

                variants={variants}

                className="inline-block whitespace-pre"

            >

                {segment}

            </motion.span>

        ) : (

            <motion.span className="inline-block whitespace-pre">

                {segment.split("").map((char, charIndex) => (

                    <motion.span

                        key={`char-${charIndex}`}

                        aria-hidden="true"

                        variants={variants}

                        className="inline-block whitespace-pre"

                    >

                        {char}

                    </motion.span>

                ))}

            </motion.span>

        );



    if (!segmentWrapperClassName) {

        return content;

    }



    const defaultWrapperClassName = per === "line" ? "block" : "inline-block";



    return (

        <span className={cn(defaultWrapperClassName, segmentWrapperClassName)}>

            {content}

        </span>

    );

});



AnimationComponent.displayName = "AnimationComponent";



const splitText = (text: string, per: "line" | "word" | "char") => {

    if (per === "line") return text.split("\n");

    return text.split(/(\s+)/);

};



const hasTransition = (

    variant: Variant

): variant is TargetAndTransition & { transition?: Transition } => {

    return (

        typeof variant === "object" && variant !== null && "transition" in variant

    );

};



const createVariantsWithTransition = (

    baseVariants: Variants,

    transition?: Transition & { exit?: Transition }

): Variants => {

    if (!transition) return baseVariants;



    const { exit: _, ...mainTransition } = transition;



    return {

        ...baseVariants,

        visible: {

            ...baseVariants.visible,

            transition: {

                ...(hasTransition(baseVariants.visible)

                    ? baseVariants.visible.transition

                    : {}),

                ...mainTransition,

            },

        },

        exit: {

            ...baseVariants.exit,

            transition: {

                ...(hasTransition(baseVariants.exit)

                    ? baseVariants.exit.transition

                    : {}),

                ...mainTransition,

                staggerDirection: -1,

            },

        },

    };

};



export function TextEffect({

    children,

    per = "word",

    as = "p",

    variants,

    className,

    preset = "fade",

    delay = 0,

    speedReveal = 1,

    speedSegment = 1,

    trigger = true,

    onAnimationComplete,

    onAnimationStart,

    segmentWrapperClassName,

    containerTransition,

    segmentTransition,

    style,

}: TextEffectProps) {

    const segments = splitText(children, per);

    const MotionTag = motion[as as keyof typeof motion] as typeof motion.div;



    const baseVariants = preset

        ? presetVariants[preset]

        : { container: defaultContainerVariants, item: defaultItemVariants };



    const stagger = defaultStaggerTimes[per] / speedReveal;



    const baseDuration = 0.3 / speedSegment;



    const customStagger = hasTransition(variants?.container?.visible ?? {})

        ? (variants?.container?.visible as TargetAndTransition).transition

            ?.staggerChildren

        : undefined;



    const customDelay = hasTransition(variants?.container?.visible ?? {})

        ? (variants?.container?.visible as TargetAndTransition).transition

            ?.delayChildren

        : undefined;



    const computedVariants = {

        container: createVariantsWithTransition(

            variants?.container || baseVariants.container,

            {

                staggerChildren: customStagger ?? stagger,

                delayChildren: customDelay ?? delay,

                ...containerTransition,

                exit: {

                    staggerChildren: customStagger ?? stagger,

                    staggerDirection: -1,

                },

            }

        ),

        item: createVariantsWithTransition(variants?.item || baseVariants.item, {

            duration: baseDuration,

            ...segmentTransition,

        }),

    };



    return (

        <AnimatePresence mode="popLayout">

            {trigger && (

                <MotionTag

                    initial="hidden"

                    animate="visible"

                    exit="exit"

                    variants={computedVariants.container}

                    className={className}

                    onAnimationComplete={onAnimationComplete}

                    onAnimationStart={onAnimationStart}

                    style={style}

                >

                    {per !== "line" ? <span className="sr-only">{children}</span> : null}

                    {segments.map((segment, index) => (

                        <AnimationComponent

                            key={`${per}-${index}-${segment}`}

                            segment={segment}

                            variants={computedVariants.item}

                            per={per}

                            segmentWrapperClassName={segmentWrapperClassName}

                        />

                    ))}

                </MotionTag>

            )}

        </AnimatePresence>

    );

}
```
##### File: `src/components/ui/animated-group.tsx`
```tsx
// Component From Motion Primitives



"use client";

import { ReactNode } from "react";

import { motion, Variants, HTMLMotionProps } from "motion/react";

import React from "react";



export type PresetType =

  | "fade"

  | "slide"

  | "scale"

  | "blur"

  | "blur-slide"

  | "zoom"

  | "flip"

  | "bounce"

  | "rotate"

  | "swing";



export type AnimatedGroupProps = {

  children: ReactNode;

  className?: string;

  variants?: {

    container?: Variants;

    item?: Variants;

  };

  preset?: PresetType;

  as?: React.ElementType;

  asChild?: React.ElementType;

};



const defaultContainerVariants: Variants = {

  visible: {

    transition: {

      staggerChildren: 0.1,

    },

  },

};



const defaultItemVariants: Variants = {

  hidden: { opacity: 0 },

  visible: { opacity: 1 },

};



const presetVariants: Record<PresetType, Variants> = {

  fade: {},

  slide: {

    hidden: { y: 20 },

    visible: { y: 0 },

  },

  scale: {

    hidden: { scale: 0.8 },

    visible: { scale: 1 },

  },

  blur: {

    hidden: { filter: "blur(4px)" },

    visible: { filter: "blur(0px)" },

  },

  "blur-slide": {

    hidden: { filter: "blur(4px)", y: 20 },

    visible: { filter: "blur(0px)", y: 0 },

  },

  zoom: {

    hidden: { scale: 0.5 },

    visible: {

      scale: 1,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  flip: {

    hidden: { rotateX: -90 },

    visible: {

      rotateX: 0,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  bounce: {

    hidden: { y: -50 },

    visible: {

      y: 0,

      transition: { type: "spring", stiffness: 400, damping: 10 },

    },

  },

  rotate: {

    hidden: { rotate: -180 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 200, damping: 15 },

    },

  },

  swing: {

    hidden: { rotate: -10 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 300, damping: 8 },

    },

  },

};



const mergeVariants = (

  defaultVariants: Variants,

  customVariants: Partial<Variants> = {},

): Variants => {

  return {

    hidden: {

      ...defaultVariants.hidden,

      ...customVariants.hidden,

    },

    visible: {

      ...defaultVariants.visible,

      ...customVariants.visible,

    },

  };

};



function AnimatedGroup({

  children,

  className,

  variants,

  preset = "fade",

  as = "div",

  asChild = "div",

}: AnimatedGroupProps) {

  const MotionContainer = motion.div;

  const MotionChild = motion.div;



  const presetVariant = preset ? presetVariants[preset] : {};

  const itemVariants = variants?.item

    ? mergeVariants(defaultItemVariants, variants.item)

    : mergeVariants(defaultItemVariants, presetVariant);



  const containerVariants = variants?.container || defaultContainerVariants;



  return (

    <MotionContainer

      initial="hidden"

      animate="visible"

      variants={containerVariants}

      className={className}

    >

      {React.Children.map(children, (child, index) => (

        <MotionChild key={index} variants={itemVariants}>

          {child}

        </MotionChild>

      ))}

    </MotionContainer>

  );

}



export { AnimatedGroup };
```
---

## Hero Section 2 <a name="hero-section-2"></a>

Solace UI hero section block variation 2.

- **Registry Key**: `hero-section-2`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/hero-section-2.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/hero-section/two/index.tsx`
```tsx
"use client";



import { useEffect, useState } from "react";

import type { Variants } from "motion/react";



import { Instrument_Serif } from "next/font/google";

import { MoveDown } from "lucide-react";

import Link from "next/link";

import { AnimatedGroup } from "@/components/ui/animated-group";

import TriplePhoneHero from "@/registry/blocks/hero-section/two/triple-phone";


const instrumentSerif = Instrument_Serif({

  subsets: ["latin"],

  weight: ["400"],

});



export function HeroSection2() {

  const [isVisible, setIsVisible] = useState(false);



  useEffect(() => {

    setIsVisible(true);

  }, []);



  const downloadGoogleDark = () => (

    <svg

      width="120"

      height="40"

      viewBox="0 0 120 40"

      fill="none"

      xmlns="http://www.w3.org/2000/svg"

    >

      <rect x="0.5" y="0.5" width="119" height="39" rx="5.5" fill="black" />

      <rect x="0.5" y="0.5" width="119" height="39" rx="5.5" stroke="#A6A6A6" />

      <path

        d="M17.8048 19.4617L8.0896 30.0059C8.09051 30.0078 8.09051 30.0106 8.09142 30.0125C8.38981 31.1574 9.41179 32 10.6254 32C11.1108 32 11.5662 31.8656 11.9567 31.6305L11.9877 31.6118L22.9229 25.1593L17.8048 19.4617Z"

        fill="#EA4335"

      />

      <path

        d="M27.6331 17.6662L27.624 17.6597L22.9028 14.8612L17.5839 19.7013L22.9219 25.1582L27.6176 22.3878C28.4406 21.9324 29 21.045 29 20.0223C29 19.0052 28.4489 18.1225 27.6331 17.6662Z"

        fill="#FBBC04"

      />

      <path

        d="M8.08942 9.99331C8.03102 10.2135 8 10.4449 8 10.6838V29.3163C8 29.5552 8.03102 29.7866 8.09034 30.0059L18.1386 19.7313L8.08942 9.99331Z"

        fill="#4285F4"

      />

      <path

        d="M17.8766 19.9999L22.9044 14.8594L11.9819 8.38351C11.585 8.13996 11.1214 8 10.626 8C9.41237 8 8.38856 8.84447 8.09018 9.99034C8.09018 9.99127 8.08926 9.9922 8.08926 9.99314L17.8766 19.9999Z"

        fill="#34A853"

      />

      <path

        d="M43.61 11.71C43.61 12.71 43.3133 13.5067 42.72 14.1C42.0533 14.8067 41.1767 15.16 40.09 15.16C39.05 15.16 38.17 14.8 37.45 14.08C36.73 13.36 36.37 12.4733 36.37 11.42C36.37 10.3667 36.73 9.48 37.45 8.76C38.17 8.04 39.05 7.68 40.09 7.68C40.6167 7.68 41.1133 7.77333 41.58 7.96C42.0467 8.14667 42.43 8.41 42.73 8.75L42.07 9.41C41.85 9.14333 41.5633 8.93667 41.21 8.79C40.8633 8.63667 40.49 8.56 40.09 8.56C39.31 8.56 38.65 8.83 38.11 9.37C37.5767 9.91667 37.31 10.6 37.31 11.42C37.31 12.24 37.5767 12.9233 38.11 13.47C38.65 14.01 39.31 14.28 40.09 14.28C40.8033 14.28 41.3967 14.08 41.87 13.68C42.3433 13.28 42.6167 12.73 42.69 12.03H40.09V11.17H43.56C43.5933 11.3567 43.61 11.5367 43.61 11.71ZM48.9078 7.84V8.72H45.6478V10.99H48.5878V11.85H45.6478V14.12H48.9078V15H44.7278V7.84H48.9078ZM52.5877 8.72V15H51.6677V8.72H49.6677V7.84H54.5877V8.72H52.5877ZM58.6654 15H57.7454V7.84H58.6654V15ZM62.5487 8.72V15H61.6287V8.72H59.6287V7.84H64.5487V8.72H62.5487ZM74.521 11.42C74.521 12.48 74.1677 13.3667 73.461 14.08C72.7477 14.8 71.8743 15.16 70.841 15.16C69.801 15.16 68.9277 14.8 68.221 14.08C67.5143 13.3667 67.161 12.48 67.161 11.42C67.161 10.36 67.5143 9.47333 68.221 8.76C68.9277 8.04 69.801 7.68 70.841 7.68C71.881 7.68 72.7543 8.04333 73.461 8.77C74.1677 9.48333 74.521 10.3667 74.521 11.42ZM68.101 11.42C68.101 12.2467 68.361 12.93 68.881 13.47C69.4077 14.01 70.061 14.28 70.841 14.28C71.621 14.28 72.271 14.01 72.791 13.47C73.3177 12.9367 73.581 12.2533 73.581 11.42C73.581 10.5867 73.3177 9.90333 72.791 9.37C72.271 8.83 71.621 8.56 70.841 8.56C70.061 8.56 69.4077 8.83 68.881 9.37C68.361 9.91 68.101 10.5933 68.101 11.42ZM76.5267 15H75.6067V7.84H76.7267L80.2067 13.41H80.2467L80.2067 12.03V7.84H81.1267V15H80.1667L76.5267 9.16H76.4867L76.5267 10.54V15Z"

        fill="white"

      />

      <path

        d="M93.5181 31.4097H95.1469V20.3981H93.5181V31.4097ZM108.189 24.3646L106.322 29.1388H106.266L104.328 24.3646H102.573L105.479 31.0371L103.823 34.749H105.521L110 24.3646H108.189ZM98.9519 30.1588C98.4176 30.1588 97.6739 29.8902 97.6739 29.2234C97.6739 28.3742 98.6001 28.0483 99.4005 28.0483C100.116 28.0483 100.454 28.2042 100.889 28.4165C100.762 29.4365 99.892 30.1588 98.9519 30.1588ZM99.1483 24.1241C97.969 24.1241 96.7469 24.6482 96.2424 25.8101L97.6879 26.4188C97.9969 25.8101 98.5721 25.611 99.1762 25.611C100.019 25.611 100.875 26.121 100.889 27.0283V27.1411C100.594 26.971 99.9627 26.7165 99.1902 26.7165C97.632 26.7165 96.0451 27.5806 96.0451 29.1952C96.0451 30.6689 97.323 31.6184 98.7546 31.6184C99.8501 31.6184 100.454 31.1225 100.833 30.5411H100.889V31.3912H102.461V27.1692C102.461 25.2146 101.015 24.1241 99.1483 24.1241ZM89.0821 25.7053H86.7655V21.9308H89.0821C90.2998 21.9308 90.9911 22.9482 90.9911 23.8176C90.9911 24.6711 90.2998 25.7053 89.0821 25.7053ZM89.0402 20.3981H85.1375V31.4097H86.7655V27.2379H89.0402C90.8453 27.2379 92.6199 25.9184 92.6199 23.8176C92.6199 21.7168 90.8453 20.3981 89.0402 20.3981ZM67.7583 30.1606C66.6332 30.1606 65.6913 29.2102 65.6913 27.9047C65.6913 26.5852 66.6332 25.6198 67.7583 25.6198C68.8695 25.6198 69.7406 26.5852 69.7406 27.9047C69.7406 29.2102 68.8695 30.1606 67.7583 30.1606ZM69.6289 24.9812H69.5722C69.2064 24.5417 68.5038 24.1444 67.6178 24.1444C65.7611 24.1444 64.0599 25.7898 64.0599 27.9047C64.0599 30.0047 65.7611 31.6369 67.6178 31.6369C68.5038 31.6369 69.2064 31.2396 69.5722 30.7851H69.6289V31.3251C69.6289 32.7582 68.8695 33.5246 67.6457 33.5246C66.6471 33.5246 66.0282 32.8005 65.7751 32.1901L64.3549 32.7864C64.7626 33.78 65.8458 35 67.6457 35C69.5582 35 71.1757 33.8646 71.1757 31.0978V24.3708H69.6289V24.9812ZM72.3008 31.4097H73.9323V20.3973H72.3008V31.4097ZM76.3362 27.777C76.2943 26.3298 77.4474 25.5916 78.2766 25.5916C78.9243 25.5916 79.4725 25.9176 79.6549 26.3862L76.3362 27.777ZM81.3989 26.528C81.0899 25.6912 80.1472 24.1444 78.2208 24.1444C76.3083 24.1444 74.7196 25.6621 74.7196 27.8907C74.7196 29.9906 76.2943 31.6369 78.4032 31.6369C80.1053 31.6369 81.0899 30.5869 81.4976 29.9765L80.2319 29.1247C79.8103 29.7493 79.2333 30.1606 78.4032 30.1606C77.5739 30.1606 76.983 29.7774 76.6033 29.0261L81.5674 26.9534L81.3989 26.528ZM41.8501 25.2939V26.883H45.6184C45.5058 27.777 45.2107 28.4297 44.7612 28.8834C44.2121 29.4374 43.3541 30.0479 41.8501 30.0479C39.5291 30.0479 37.7152 28.1602 37.7152 25.8189C37.7152 23.4767 39.5291 21.5899 41.8501 21.5899C43.1018 21.5899 44.0157 22.0867 44.6905 22.7254L45.8017 21.604C44.8589 20.6959 43.6081 20 41.8501 20C38.6719 20 36 22.6117 36 25.8189C36 29.0261 38.6719 31.6369 41.8501 31.6369C43.5653 31.6369 44.8589 31.0688 45.8715 30.0047C46.9129 28.9547 47.2358 27.4793 47.2358 26.2866C47.2358 25.9176 47.2079 25.5775 47.1512 25.2939H41.8501ZM51.5208 30.1606C50.3957 30.1606 49.425 29.2243 49.425 27.8907C49.425 26.5421 50.3957 25.6198 51.5208 25.6198C52.6451 25.6198 53.6158 26.5421 53.6158 27.8907C53.6158 29.2243 52.6451 30.1606 51.5208 30.1606ZM51.5208 24.1444C49.4669 24.1444 47.7936 25.7194 47.7936 27.8907C47.7936 30.0479 49.4669 31.6369 51.5208 31.6369C53.5739 31.6369 55.2472 30.0479 55.2472 27.8907C55.2472 25.7194 53.5739 24.1444 51.5208 24.1444ZM59.65 30.1606C58.5249 30.1606 57.5542 29.2243 57.5542 27.8907C57.5542 26.5421 58.5249 25.6198 59.65 25.6198C60.7752 25.6198 61.745 26.5421 61.745 27.8907C61.745 29.2243 60.7752 30.1606 59.65 30.1606ZM59.65 24.1444C57.597 24.1444 55.9237 25.7194 55.9237 27.8907C55.9237 30.0479 57.597 31.6369 59.65 31.6369C61.7031 31.6369 63.3764 30.0479 63.3764 27.8907C63.3764 25.7194 61.7031 24.1444 59.65 24.1444Z"

        fill="white"

      />

    </svg>

  );

  const downloadAppleDark = () => (

    <svg

      width="120"

      height="40"

      viewBox="0 0 120 40"

      fill="none"

      xmlns="http://www.w3.org/2000/svg"

    >

      <rect x="0.5" y="0.5" width="119" height="39" rx="5.5" fill="black" />

      <rect x="0.5" y="0.5" width="119" height="39" rx="5.5" stroke="#A6A6A6" />

      <path

        d="M24.7045 20.7631C24.7166 19.8432 24.9669 18.9413 25.4321 18.1412C25.8972 17.3411 26.5621 16.6688 27.3648 16.187C26.8548 15.476 26.1821 14.8908 25.4 14.478C24.6178 14.0652 23.7479 13.8361 22.8592 13.809C20.9635 13.6147 19.1258 14.9164 18.1598 14.9164C17.1751 14.9164 15.6878 13.8283 14.0862 13.8604C13.0502 13.8931 12.0406 14.1872 11.1557 14.7141C10.2708 15.241 9.54075 15.9827 9.03674 16.8669C6.85352 20.5573 8.48201 25.9809 10.5734 28.964C11.6197 30.4247 12.8426 32.0564 14.4428 31.9985C16.0086 31.9351 16.5934 31.0237 18.4835 31.0237C20.3561 31.0237 20.9048 31.9985 22.5374 31.9617C24.2176 31.9351 25.2762 30.4945 26.2859 29.02C27.0377 27.9792 27.6162 26.8288 28 25.6116C27.0238 25.2085 26.1908 24.5338 25.6048 23.6716C25.0187 22.8094 24.7056 21.7979 24.7045 20.7631Z"

        fill="white"

      />

      <path

        d="M21.6208 11.8471C22.5369 10.7734 22.9883 9.39335 22.879 8C21.4794 8.14352 20.1865 8.7966 19.258 9.82911C18.804 10.3335 18.4563 10.9203 18.2348 11.556C18.0132 12.1917 17.9222 12.8638 17.9669 13.5338C18.6669 13.5408 19.3595 13.3927 19.9924 13.1005C20.6254 12.8084 21.1821 12.3798 21.6208 11.8471Z"

        fill="white"

      />

      <path

        d="M36.791 8.50146H38.8608C40.6494 8.50146 41.5195 9.56494 41.5195 11.4941C41.5195 13.4233 40.6406 14.5 38.8608 14.5H36.791V8.50146ZM37.7886 9.34082V13.6606H38.751C39.9375 13.6606 40.4956 12.9443 40.4956 11.5073C40.4956 10.0615 39.9331 9.34082 38.751 9.34082H37.7886ZM44.6748 9.77588C45.8877 9.77588 46.7358 10.5625 46.7358 11.8677V12.4697C46.7358 13.8188 45.8877 14.5791 44.6748 14.5791C43.4443 14.5791 42.605 13.8276 42.605 12.4741V11.8721C42.605 10.6021 43.4575 9.77588 44.6748 9.77588ZM44.6792 10.5625C43.9849 10.5625 43.5894 11.1426 43.5894 11.9204V12.439C43.5894 13.2168 43.9585 13.7925 44.6792 13.7925C45.3911 13.7925 45.7559 13.2212 45.7559 12.439V11.9204C45.7559 11.1426 45.3735 10.5625 44.6792 10.5625ZM53.8989 9.85498L52.6772 14.5H51.6841L50.7129 11.0723H50.6865L49.7329 14.5H48.7354L47.4609 9.85498H48.5112L49.2583 13.397H49.3022L50.2383 9.85498H51.1567L52.1191 13.397H52.1631L52.9233 9.85498H53.8989ZM54.8657 14.5V9.85498H55.8237V10.6899H55.8721C55.9907 10.3252 56.3291 9.78467 57.2695 9.78467C58.2056 9.78467 58.834 10.3032 58.834 11.3623V14.5H57.8584V11.6479C57.8584 10.9404 57.4893 10.6152 56.9399 10.6152C56.2192 10.6152 55.8413 11.1689 55.8413 11.9204V14.5H54.8657ZM60.3105 14.5V8.18506H61.2861V14.5H60.3105ZM64.6348 9.77588C65.8477 9.77588 66.6958 10.5625 66.6958 11.8677V12.4697C66.6958 13.8188 65.8477 14.5791 64.6348 14.5791C63.4043 14.5791 62.5649 13.8276 62.5649 12.4741V11.8721C62.5649 10.6021 63.4175 9.77588 64.6348 9.77588ZM64.6392 10.5625C63.9448 10.5625 63.5493 11.1426 63.5493 11.9204V12.439C63.5493 13.2168 63.9185 13.7925 64.6392 13.7925C65.3511 13.7925 65.7158 13.2212 65.7158 12.439V11.9204C65.7158 11.1426 65.3335 10.5625 64.6392 10.5625ZM69.2227 14.5703C68.3218 14.5703 67.7021 14.0166 67.7021 13.1509C67.7021 12.3291 68.2734 11.7754 69.3457 11.7754H70.519V11.3403C70.519 10.8086 70.1807 10.5581 69.6445 10.5581C69.1172 10.5581 68.8799 10.7778 68.8052 11.0854H67.8779C67.9351 10.3076 68.5195 9.78467 69.6753 9.78467C70.6685 9.78467 71.4902 10.1978 71.4902 11.3535V14.5H70.563V13.8979H70.519C70.3125 14.2539 69.9082 14.5703 69.2227 14.5703ZM69.5259 13.8145C70.0796 13.8145 70.519 13.4365 70.519 12.9312V12.4302H69.4995C68.9326 12.4302 68.6821 12.7158 68.6821 13.1025C68.6821 13.5859 69.0864 13.8145 69.5259 13.8145ZM74.4961 9.79346C75.1509 9.79346 75.6519 10.0835 75.832 10.5537H75.8804V8.18506H76.856V14.5H75.9067V13.7573H75.8584C75.7178 14.2275 75.1597 14.5615 74.4829 14.5615C73.415 14.5615 72.7207 13.8013 72.7207 12.5752V11.7798C72.7207 10.5537 73.4282 9.79346 74.4961 9.79346ZM74.7686 10.5933C74.1182 10.5933 73.7139 11.0767 73.7139 11.9204V12.4302C73.7139 13.2783 74.1226 13.7617 74.7905 13.7617C75.4497 13.7617 75.8804 13.2827 75.8804 12.4917V11.7886C75.8804 11.0723 75.4102 10.5933 74.7686 10.5933ZM82.2129 9.77588C83.4258 9.77588 84.2739 10.5625 84.2739 11.8677V12.4697C84.2739 13.8188 83.4258 14.5791 82.2129 14.5791C80.9824 14.5791 80.1431 13.8276 80.1431 12.4741V11.8721C80.1431 10.6021 80.9956 9.77588 82.2129 9.77588ZM82.2173 10.5625C81.5229 10.5625 81.1274 11.1426 81.1274 11.9204V12.439C81.1274 13.2168 81.4966 13.7925 82.2173 13.7925C82.9292 13.7925 83.2939 13.2212 83.2939 12.439V11.9204C83.2939 11.1426 82.9116 10.5625 82.2173 10.5625ZM85.5308 14.5V9.85498H86.4888V10.6899H86.5371C86.6558 10.3252 86.9941 9.78467 87.9346 9.78467C88.8706 9.78467 89.499 10.3032 89.499 11.3623V14.5H88.5234V11.6479C88.5234 10.9404 88.1543 10.6152 87.605 10.6152C86.8843 10.6152 86.5063 11.1689 86.5063 11.9204V14.5H85.5308ZM93.2739 9.88574V8.72559H94.2275V9.88574H95.269V10.6504H94.2275V13.1157C94.2275 13.6211 94.4165 13.7617 94.9395 13.7617C95.0713 13.7617 95.2471 13.7529 95.3218 13.7441V14.4912C95.2427 14.5044 94.9175 14.5308 94.6978 14.5308C93.5684 14.5308 93.2607 14.1265 93.2607 13.1948V10.6504H92.5532V9.88574H93.2739ZM96.4995 14.5V8.18506H97.4707V10.6899H97.519C97.6201 10.3604 97.998 9.78467 98.9297 9.78467C99.835 9.78467 100.481 10.3076 100.481 11.3667V14.5H99.5098V11.6523C99.5098 10.9448 99.1187 10.6152 98.5649 10.6152C97.8662 10.6152 97.4707 11.1646 97.4707 11.9204V14.5H96.4995ZM103.755 14.5791C102.489 14.5791 101.703 13.8013 101.703 12.4917V11.8633C101.703 10.5449 102.564 9.77588 103.698 9.77588C104.862 9.77588 105.684 10.5845 105.684 11.8633V12.4082H102.669V12.6367C102.669 13.3047 103.065 13.7969 103.75 13.7969C104.26 13.7969 104.612 13.5552 104.678 13.2651H105.631C105.574 13.8013 105.007 14.5791 103.755 14.5791ZM102.669 11.771H104.73V11.7095C104.73 11.0107 104.322 10.5449 103.702 10.5449C103.083 10.5449 102.669 11.0107 102.669 11.7095V11.771Z"

        fill="white"

      />

      <path

        d="M38.2061 30.5H36.1758L40.3066 18.5029H42.5391L46.6611 30.5H44.5518L43.4883 27.1777H39.2783L38.2061 30.5ZM41.4316 20.5771H41.3525L39.7266 25.6484H43.04L41.4316 20.5771ZM52.2644 30.6318C51.0603 30.6318 50.1462 30.0605 49.654 29.208H49.5837V33.585H47.6325V21.21H49.531V22.5723H49.6013C50.1111 21.6846 51.0603 21.0869 52.3083 21.0869C54.3913 21.0869 55.8767 22.6602 55.8767 25.4375V26.2637C55.8767 29.0234 54.4089 30.6318 52.2644 30.6318ZM51.8161 29.0234C53.0554 29.0234 53.8991 28.0303 53.8991 26.1582V25.5078C53.8991 23.7061 53.1081 22.6777 51.781 22.6777C50.4187 22.6777 49.5661 23.7852 49.5661 25.499V26.1582C49.5661 27.916 50.4275 29.0234 51.8161 29.0234ZM62.183 30.6318C60.9789 30.6318 60.0649 30.0605 59.5727 29.208H59.5024V33.585H57.5512V21.21H59.4496V22.5723H59.52C60.0297 21.6846 60.9789 21.0869 62.227 21.0869C64.31 21.0869 65.7954 22.6602 65.7954 25.4375V26.2637C65.7954 29.0234 64.3276 30.6318 62.183 30.6318ZM61.7348 29.0234C62.9741 29.0234 63.8178 28.0303 63.8178 26.1582V25.5078C63.8178 23.7061 63.0268 22.6777 61.6996 22.6777C60.3373 22.6777 59.4848 23.7852 59.4848 25.499V26.1582C59.4848 27.916 60.3461 29.0234 61.7348 29.0234ZM69.8387 27.1689H71.7899C71.8778 28.2061 72.7919 29.0938 74.4882 29.0938C76.0438 29.0938 76.9667 28.3643 76.9667 27.2305C76.9667 26.3164 76.3514 25.8242 75.0682 25.5166L73.0995 25.0244C71.5526 24.6641 70.1639 23.7412 70.1639 21.79C70.1639 19.4961 72.1679 18.2393 74.497 18.2393C76.8261 18.2393 78.7684 19.4961 78.8124 21.7373H76.8964C76.8085 20.7178 76.0262 19.874 74.4706 19.874C73.0995 19.874 72.1679 20.5244 72.1679 21.6406C72.1679 22.4229 72.7128 22.9854 73.829 23.2402L75.7889 23.7236C77.5907 24.1631 78.9618 25.0156 78.9618 27.0547C78.9618 29.4102 77.0546 30.7373 74.3387 30.7373C70.9989 30.7373 69.8827 28.7861 69.8387 27.1689ZM81.3395 21.21V18.9512H83.2555V21.21H85.066V22.7744H83.2555V27.7314C83.2555 28.7422 83.6334 29.0234 84.6793 29.0234C84.8463 29.0234 85.0045 29.0234 85.1188 29.0059V30.5C84.9605 30.5264 84.5914 30.5615 84.1959 30.5615C81.9371 30.5615 81.3131 29.7529 81.3131 27.8896V22.7744H80.0299V21.21H81.3395ZM90.3353 21.0518C93.0071 21.0518 94.4573 22.9326 94.4573 25.4639V26.2109C94.4573 28.8301 93.0159 30.6582 90.3353 30.6582C87.6546 30.6582 86.1956 28.8301 86.1956 26.2109V25.4639C86.1956 22.9414 87.6634 21.0518 90.3353 21.0518ZM90.3353 22.6162C88.8851 22.6162 88.1644 23.8027 88.1644 25.4902V26.2021C88.1644 27.8633 88.8763 29.085 90.3353 29.085C91.7943 29.085 92.4974 27.8721 92.4974 26.2021V25.4902C92.4974 23.7939 91.7855 22.6162 90.3353 22.6162ZM96.1055 30.5V21.21H98.0567V22.4316H98.127C98.3643 21.8516 99.0586 21.0781 100.351 21.0781C100.606 21.0781 100.825 21.0957 101.01 21.1309V22.8535C100.843 22.8096 100.5 22.7832 100.175 22.7832C98.6104 22.7832 98.083 23.75 98.083 24.998V30.5H96.1055ZM105.743 30.6582C103.256 30.6582 101.674 29.0146 101.674 26.2637V25.3232C101.674 22.7305 103.22 21.0518 105.664 21.0518C108.142 21.0518 109.636 22.792 109.636 25.4111V26.2988H103.598V26.5186C103.598 28.083 104.442 29.1201 105.769 29.1201C106.762 29.1201 107.439 28.6279 107.677 27.8281H109.531C109.25 29.3311 108.037 30.6582 105.743 30.6582ZM103.598 24.9365H107.729V24.9189C107.729 23.6006 106.912 22.5635 105.673 22.5635C104.416 22.5635 103.598 23.6006 103.598 24.9189V24.9365Z"

        fill="white"

      />

    </svg>

  );

  const downloadGoogleLight = () => (

    <svg

      width="120"

      height="40"

      viewBox="0 0 120 40"

      fill="none"

      xmlns="http://www.w3.org/2000/svg"

    >

      <rect width="120" height="40" rx="6" fill="white" />

      <rect x="0.5" y="0.5" width="119" height="39" rx="5.5" stroke="black" />

      <path

        d="M17.8048 19.4617L8.0896 30.0059C8.09051 30.0078 8.09051 30.0106 8.09142 30.0125C8.38981 31.1574 9.41179 32 10.6254 32C11.1108 32 11.5662 31.8656 11.9567 31.6305L11.9877 31.6118L22.9229 25.1593L17.8048 19.4617Z"

        fill="#EA4335"

      />

      <path

        d="M27.6331 17.6662L27.624 17.6597L22.9028 14.8612L17.5839 19.7013L22.9219 25.1582L27.6176 22.3878C28.4406 21.9324 29 21.045 29 20.0223C29 19.0052 28.4489 18.1225 27.6331 17.6662Z"

        fill="#FBBC04"

      />

      <path

        d="M8.08942 9.99331C8.03102 10.2135 8 10.4449 8 10.6838V29.3163C8 29.5552 8.03102 29.7866 8.09034 30.0059L18.1386 19.7313L8.08942 9.99331Z"

        fill="#4285F4"

      />

      <path

        d="M17.8766 19.9999L22.9044 14.8594L11.9819 8.38351C11.585 8.13996 11.1214 8 10.626 8C9.41237 8 8.38856 8.84447 8.09018 9.99034C8.09018 9.99127 8.08926 9.9922 8.08926 9.99314L17.8766 19.9999Z"

        fill="#34A853"

      />

      <path

        d="M43.61 11.71C43.61 12.71 43.3133 13.5067 42.72 14.1C42.0533 14.8067 41.1767 15.16 40.09 15.16C39.05 15.16 38.17 14.8 37.45 14.08C36.73 13.36 36.37 12.4733 36.37 11.42C36.37 10.3667 36.73 9.48 37.45 8.76C38.17 8.04 39.05 7.68 40.09 7.68C40.6167 7.68 41.1133 7.77333 41.58 7.96C42.0467 8.14667 42.43 8.41 42.73 8.75L42.07 9.41C41.85 9.14333 41.5633 8.93667 41.21 8.79C40.8633 8.63667 40.49 8.56 40.09 8.56C39.31 8.56 38.65 8.83 38.11 9.37C37.5767 9.91667 37.31 10.6 37.31 11.42C37.31 12.24 37.5767 12.9233 38.11 13.47C38.65 14.01 39.31 14.28 40.09 14.28C40.8033 14.28 41.3967 14.08 41.87 13.68C42.3433 13.28 42.6167 12.73 42.69 12.03H40.09V11.17H43.56C43.5933 11.3567 43.61 11.5367 43.61 11.71ZM48.9078 7.84V8.72H45.6478V10.99H48.5878V11.85H45.6478V14.12H48.9078V15H44.7278V7.84H48.9078ZM52.5877 8.72V15H51.6677V8.72H49.6677V7.84H54.5877V8.72H52.5877ZM58.6654 15H57.7454V7.84H58.6654V15ZM62.5487 8.72V15H61.6287V8.72H59.6287V7.84H64.5487V8.72H62.5487ZM74.521 11.42C74.521 12.48 74.1677 13.3667 73.461 14.08C72.7477 14.8 71.8743 15.16 70.841 15.16C69.801 15.16 68.9277 14.8 68.221 14.08C67.5143 13.3667 67.161 12.48 67.161 11.42C67.161 10.36 67.5143 9.47333 68.221 8.76C68.9277 8.04 69.801 7.68 70.841 7.68C71.881 7.68 72.7543 8.04333 73.461 8.77C74.1677 9.48333 74.521 10.3667 74.521 11.42ZM68.101 11.42C68.101 12.2467 68.361 12.93 68.881 13.47C69.4077 14.01 70.061 14.28 70.841 14.28C71.621 14.28 72.271 14.01 72.791 13.47C73.3177 12.9367 73.581 12.2533 73.581 11.42C73.581 10.5867 73.3177 9.90333 72.791 9.37C72.271 8.83 71.621 8.56 70.841 8.56C70.061 8.56 69.4077 8.83 68.881 9.37C68.361 9.91 68.101 10.5933 68.101 11.42ZM76.5267 15H75.6067V7.84H76.7267L80.2067 13.41H80.2467L80.2067 12.03V7.84H81.1267V15H80.1667L76.5267 9.16H76.4867L76.5267 10.54V15Z"

        fill="black"

      />

      <path

        d="M93.5181 31.4097H95.1469V20.3981H93.5181V31.4097ZM108.189 24.3646L106.322 29.1388H106.266L104.328 24.3646H102.573L105.479 31.0371L103.823 34.749H105.521L110 24.3646H108.189ZM98.9519 30.1588C98.4176 30.1588 97.6739 29.8902 97.6739 29.2234C97.6739 28.3742 98.6001 28.0483 99.4005 28.0483C100.116 28.0483 100.454 28.2042 100.889 28.4165C100.762 29.4365 99.892 30.1588 98.9519 30.1588ZM99.1483 24.1241C97.969 24.1241 96.7469 24.6482 96.2424 25.8101L97.6879 26.4188C97.9969 25.8101 98.5721 25.611 99.1762 25.611C100.019 25.611 100.875 26.121 100.889 27.0283V27.1411C100.594 26.971 99.9627 26.7165 99.1902 26.7165C97.632 26.7165 96.0451 27.5806 96.0451 29.1952C96.0451 30.6689 97.323 31.6184 98.7546 31.6184C99.8501 31.6184 100.454 31.1225 100.833 30.5411H100.889V31.3912H102.461V27.1692C102.461 25.2146 101.015 24.1241 99.1483 24.1241ZM89.0821 25.7053H86.7655V21.9308H89.0821C90.2998 21.9308 90.9911 22.9482 90.9911 23.8176C90.9911 24.6711 90.2998 25.7053 89.0821 25.7053ZM89.0402 20.3981H85.1375V31.4097H86.7655V27.2379H89.0402C90.8453 27.2379 92.6199 25.9184 92.6199 23.8176C92.6199 21.7168 90.8453 20.3981 89.0402 20.3981ZM67.7583 30.1606C66.6332 30.1606 65.6913 29.2102 65.6913 27.9047C65.6913 26.5852 66.6332 25.6198 67.7583 25.6198C68.8695 25.6198 69.7406 26.5852 69.7406 27.9047C69.7406 29.2102 68.8695 30.1606 67.7583 30.1606ZM69.6289 24.9812H69.5722C69.2064 24.5417 68.5038 24.1444 67.6178 24.1444C65.7611 24.1444 64.0599 25.7898 64.0599 27.9047C64.0599 30.0047 65.7611 31.6369 67.6178 31.6369C68.5038 31.6369 69.2064 31.2396 69.5722 30.7851H69.6289V31.3251C69.6289 32.7582 68.8695 33.5246 67.6457 33.5246C66.6471 33.5246 66.0282 32.8005 65.7751 32.1901L64.3549 32.7864C64.7626 33.78 65.8458 35 67.6457 35C69.5582 35 71.1757 33.8646 71.1757 31.0978V24.3708H69.6289V24.9812ZM72.3008 31.4097H73.9323V20.3973H72.3008V31.4097ZM76.3362 27.777C76.2943 26.3298 77.4474 25.5916 78.2766 25.5916C78.9243 25.5916 79.4725 25.9176 79.6549 26.3862L76.3362 27.777ZM81.3989 26.528C81.0899 25.6912 80.1472 24.1444 78.2208 24.1444C76.3083 24.1444 74.7196 25.6621 74.7196 27.8907C74.7196 29.9906 76.2943 31.6369 78.4032 31.6369C80.1053 31.6369 81.0899 30.5869 81.4976 29.9765L80.2319 29.1247C79.8103 29.7493 79.2333 30.1606 78.4032 30.1606C77.5739 30.1606 76.983 29.7774 76.6033 29.0261L81.5674 26.9534L81.3989 26.528ZM41.8501 25.2939V26.883H45.6184C45.5058 27.777 45.2107 28.4297 44.7612 28.8834C44.2121 29.4374 43.3541 30.0479 41.8501 30.0479C39.5291 30.0479 37.7152 28.1602 37.7152 25.8189C37.7152 23.4767 39.5291 21.5899 41.8501 21.5899C43.1018 21.5899 44.0157 22.0867 44.6905 22.7254L45.8017 21.604C44.8589 20.6959 43.6081 20 41.8501 20C38.6719 20 36 22.6117 36 25.8189C36 29.0261 38.6719 31.6369 41.8501 31.6369C43.5653 31.6369 44.8589 31.0688 45.8715 30.0047C46.9129 28.9547 47.2358 27.4793 47.2358 26.2866C47.2358 25.9176 47.2079 25.5775 47.1512 25.2939H41.8501ZM51.5208 30.1606C50.3957 30.1606 49.425 29.2243 49.425 27.8907C49.425 26.5421 50.3957 25.6198 51.5208 25.6198C52.6451 25.6198 53.6158 26.5421 53.6158 27.8907C53.6158 29.2243 52.6451 30.1606 51.5208 30.1606ZM51.5208 24.1444C49.4669 24.1444 47.7936 25.7194 47.7936 27.8907C47.7936 30.0479 49.4669 31.6369 51.5208 31.6369C53.5739 31.6369 55.2472 30.0479 55.2472 27.8907C55.2472 25.7194 53.5739 24.1444 51.5208 24.1444ZM59.65 30.1606C58.5249 30.1606 57.5542 29.2243 57.5542 27.8907C57.5542 26.5421 58.5249 25.6198 59.65 25.6198C60.7752 25.6198 61.745 26.5421 61.745 27.8907C61.745 29.2243 60.7752 30.1606 59.65 30.1606ZM59.65 24.1444C57.597 24.1444 55.9237 25.7194 55.9237 27.8907C55.9237 30.0479 57.597 31.6369 59.65 31.6369C61.7031 31.6369 63.3764 30.0479 63.3764 27.8907C63.3764 25.7194 61.7031 24.1444 59.65 24.1444Z"

        fill="black"

      />

    </svg>

  );

  const downloadAppleLight = () => (

    <svg

      width="120"

      height="40"

      viewBox="0 0 120 40"

      fill="none"

      xmlns="http://www.w3.org/2000/svg"

    >

      <rect width="120" height="40" rx="6" fill="white" />

      <rect x="0.5" y="0.5" width="119" height="39" rx="5.5" stroke="black" />

      <path

        d="M24.7045 20.7631C24.7166 19.8432 24.9669 18.9413 25.4321 18.1412C25.8972 17.3411 26.5621 16.6688 27.3648 16.187C26.8548 15.476 26.1821 14.8908 25.4 14.478C24.6178 14.0652 23.7479 13.8361 22.8592 13.809C20.9635 13.6147 19.1258 14.9164 18.1598 14.9164C17.1751 14.9164 15.6878 13.8283 14.0862 13.8604C13.0502 13.8931 12.0406 14.1872 11.1557 14.7141C10.2708 15.241 9.54075 15.9827 9.03674 16.8669C6.85352 20.5573 8.48201 25.9809 10.5734 28.964C11.6197 30.4247 12.8426 32.0564 14.4428 31.9985C16.0086 31.9351 16.5934 31.0237 18.4835 31.0237C20.3561 31.0237 20.9048 31.9985 22.5374 31.9617C24.2176 31.9351 25.2762 30.4945 26.2859 29.02C27.0377 27.9792 27.6162 26.8288 28 25.6116C27.0238 25.2085 26.1908 24.5338 25.6048 23.6716C25.0187 22.8094 24.7056 21.7979 24.7045 20.7631Z"

        fill="black"

      />

      <path

        d="M21.6208 11.8471C22.5369 10.7734 22.9883 9.39335 22.879 8C21.4793 8.14352 20.1865 8.7966 19.258 9.82911C18.804 10.3335 18.4563 10.9203 18.2348 11.556C18.0132 12.1917 17.9222 12.8638 17.9669 13.5338C18.6669 13.5408 19.3595 13.3927 19.9924 13.1005C20.6253 12.8084 21.1821 12.3798 21.6208 11.8471Z"

        fill="black"

      />

      <path

        d="M36.791 8.50146H38.8608C40.6494 8.50146 41.5195 9.56494 41.5195 11.4941C41.5195 13.4233 40.6406 14.5 38.8608 14.5H36.791V8.50146ZM37.7886 9.34082V13.6606H38.751C39.9375 13.6606 40.4956 12.9443 40.4956 11.5073C40.4956 10.0615 39.9331 9.34082 38.751 9.34082H37.7886ZM44.6748 9.77588C45.8877 9.77588 46.7358 10.5625 46.7358 11.8677V12.4697C46.7358 13.8188 45.8877 14.5791 44.6748 14.5791C43.4443 14.5791 42.605 13.8276 42.605 12.4741V11.8721C42.605 10.6021 43.4575 9.77588 44.6748 9.77588ZM44.6792 10.5625C43.9849 10.5625 43.5894 11.1426 43.5894 11.9204V12.439C43.5894 13.2168 43.9585 13.7925 44.6792 13.7925C45.3911 13.7925 45.7559 13.2212 45.7559 12.439V11.9204C45.7559 11.1426 45.3735 10.5625 44.6792 10.5625ZM53.8989 9.85498L52.6772 14.5H51.6841L50.7129 11.0723H50.6865L49.7329 14.5H48.7354L47.4609 9.85498H48.5112L49.2583 13.397H49.3022L50.2383 9.85498H51.1567L52.1191 13.397H52.1631L52.9233 9.85498H53.8989ZM54.8657 14.5V9.85498H55.8237V10.6899H55.8721C55.9907 10.3252 56.3291 9.78467 57.2695 9.78467C58.2056 9.78467 58.834 10.3032 58.834 11.3623V14.5H57.8584V11.6479C57.8584 10.9404 57.4893 10.6152 56.9399 10.6152C56.2192 10.6152 55.8413 11.1689 55.8413 11.9204V14.5H54.8657ZM60.3105 14.5V8.18506H61.2861V14.5H60.3105ZM64.6348 9.77588C65.8477 9.77588 66.6958 10.5625 66.6958 11.8677V12.4697C66.6958 13.8188 65.8477 14.5791 64.6348 14.5791C63.4043 14.5791 62.5649 13.8276 62.5649 12.4741V11.8721C62.5649 10.6021 63.4175 9.77588 64.6348 9.77588ZM64.6392 10.5625C63.9448 10.5625 63.5493 11.1426 63.5493 11.9204V12.439C63.5493 13.2168 63.9185 13.7925 64.6392 13.7925C65.3511 13.7925 65.7158 13.2212 65.7158 12.439V11.9204C65.7158 11.1426 65.3335 10.5625 64.6392 10.5625ZM69.2227 14.5703C68.3218 14.5703 67.7021 14.0166 67.7021 13.1509C67.7021 12.3291 68.2734 11.7754 69.3457 11.7754H70.519V11.3403C70.519 10.8086 70.1807 10.5581 69.6445 10.5581C69.1172 10.5581 68.8799 10.7778 68.8052 11.0854H67.8779C67.9351 10.3076 68.5195 9.78467 69.6753 9.78467C70.6685 9.78467 71.4902 10.1978 71.4902 11.3535V14.5H70.563V13.8979H70.519C70.3125 14.2539 69.9082 14.5703 69.2227 14.5703ZM69.5259 13.8145C70.0796 13.8145 70.519 13.4365 70.519 12.9312V12.4302H69.4995C68.9326 12.4302 68.6821 12.7158 68.6821 13.1025C68.6821 13.5859 69.0864 13.8145 69.5259 13.8145ZM74.4961 9.79346C75.1509 9.79346 75.6519 10.0835 75.832 10.5537H75.8804V8.18506H76.856V14.5H75.9067V13.7573H75.8584C75.7178 14.2275 75.1597 14.5615 74.4829 14.5615C73.415 14.5615 72.7207 13.8013 72.7207 12.5752V11.7798C72.7207 10.5537 73.4282 9.79346 74.4961 9.79346ZM74.7686 10.5933C74.1182 10.5933 73.7139 11.0767 73.7139 11.9204V12.4302C73.7139 13.2783 74.1226 13.7617 74.7905 13.7617C75.4497 13.7617 75.8804 13.2827 75.8804 12.4917V11.7886C75.8804 11.0723 75.4102 10.5933 74.7686 10.5933ZM82.2129 9.77588C83.4258 9.77588 84.2739 10.5625 84.2739 11.8677V12.4697C84.2739 13.8188 83.4258 14.5791 82.2129 14.5791C80.9824 14.5791 80.1431 13.8276 80.1431 12.4741V11.8721C80.1431 10.6021 80.9956 9.77588 82.2129 9.77588ZM82.2173 10.5625C81.5229 10.5625 81.1274 11.1426 81.1274 11.9204V12.439C81.1274 13.2168 81.4966 13.7925 82.2173 13.7925C82.9292 13.7925 83.2939 13.2212 83.2939 12.439V11.9204C83.2939 11.1426 82.9116 10.5625 82.2173 10.5625ZM85.5308 14.5V9.85498H86.4888V10.6899H86.5371C86.6558 10.3252 86.9941 9.78467 87.9346 9.78467C88.8706 9.78467 89.499 10.3032 89.499 11.3623V14.5H88.5234V11.6479C88.5234 10.9404 88.1543 10.6152 87.605 10.6152C86.8843 10.6152 86.5063 11.1689 86.5063 11.9204V14.5H85.5308ZM93.2739 9.88574V8.72559H94.2275V9.88574H95.269V10.6504H94.2275V13.1157C94.2275 13.6211 94.4165 13.7617 94.9395 13.7617C95.0713 13.7617 95.2471 13.7529 95.3218 13.7441V14.4912C95.2427 14.5044 94.9175 14.5308 94.6978 14.5308C93.5684 14.5308 93.2607 14.1265 93.2607 13.1948V10.6504H92.5532V9.88574H93.2739ZM96.4995 14.5V8.18506H97.4707V10.6899H97.519C97.6201 10.3604 97.998 9.78467 98.9297 9.78467C99.835 9.78467 100.481 10.3076 100.481 11.3667V14.5H99.5098V11.6523C99.5098 10.9448 99.1187 10.6152 98.5649 10.6152C97.8662 10.6152 97.4707 11.1646 97.4707 11.9204V14.5H96.4995ZM103.755 14.5791C102.489 14.5791 101.703 13.8013 101.703 12.4917V11.8633C101.703 10.5449 102.564 9.77588 103.698 9.77588C104.862 9.77588 105.684 10.5845 105.684 11.8633V12.4082H102.669V12.6367C102.669 13.3047 103.065 13.7969 103.75 13.7969C104.26 13.7969 104.612 13.5552 104.678 13.2651H105.631C105.574 13.8013 105.007 14.5791 103.755 14.5791ZM102.669 11.771H104.73V11.7095C104.73 11.0107 104.322 10.5449 103.702 10.5449C103.083 10.5449 102.669 11.0107 102.669 11.7095V11.771Z"

        fill="black"

      />

      <path

        d="M38.2061 30.5H36.1758L40.3066 18.5029H42.5391L46.6611 30.5H44.5518L43.4883 27.1777H39.2783L38.2061 30.5ZM41.4316 20.5771H41.3525L39.7266 25.6484H43.04L41.4316 20.5771ZM52.2644 30.6318C51.0603 30.6318 50.1462 30.0605 49.654 29.208H49.5837V33.585H47.6325V21.21H49.531V22.5723H49.6013C50.1111 21.6846 51.0603 21.0869 52.3083 21.0869C54.3913 21.0869 55.8767 22.6602 55.8767 25.4375V26.2637C55.8767 29.0234 54.4089 30.6318 52.2644 30.6318ZM51.8161 29.0234C53.0554 29.0234 53.8991 28.0303 53.8991 26.1582V25.5078C53.8991 23.7061 53.1081 22.6777 51.781 22.6777C50.4187 22.6777 49.5661 23.7852 49.5661 25.499V26.1582C49.5661 27.916 50.4275 29.0234 51.8161 29.0234ZM62.183 30.6318C60.9789 30.6318 60.0649 30.0605 59.5727 29.208H59.5024V33.585H57.5512V21.21H59.4496V22.5723H59.52C60.0297 21.6846 60.9789 21.0869 62.227 21.0869C64.31 21.0869 65.7954 22.6602 65.7954 25.4375V26.2637C65.7954 29.0234 64.3276 30.6318 62.183 30.6318ZM61.7348 29.0234C62.9741 29.0234 63.8178 28.0303 63.8178 26.1582V25.5078C63.8178 23.7061 63.0268 22.6777 61.6996 22.6777C60.3373 22.6777 59.4848 23.7852 59.4848 25.499V26.1582C59.4848 27.916 60.3461 29.0234 61.7348 29.0234ZM69.8387 27.1689H71.7899C71.8778 28.2061 72.7919 29.0938 74.4882 29.0938C76.0438 29.0938 76.9667 28.3643 76.9667 27.2305C76.9667 26.3164 76.3514 25.8242 75.0682 25.5166L73.0995 25.0244C71.5526 24.6641 70.1639 23.7412 70.1639 21.79C70.1639 19.4961 72.1679 18.2393 74.497 18.2393C76.8261 18.2393 78.7684 19.4961 78.8124 21.7373H76.8964C76.8085 20.7178 76.0262 19.874 74.4706 19.874C73.0995 19.874 72.1679 20.5244 72.1679 21.6406C72.1679 22.4229 72.7128 22.9854 73.829 23.2402L75.7889 23.7236C77.5907 24.1631 78.9618 25.0156 78.9618 27.0547C78.9618 29.4102 77.0546 30.7373 74.3387 30.7373C70.9989 30.7373 69.8827 28.7861 69.8387 27.1689ZM81.3395 21.21V18.9512H83.2555V21.21H85.066V22.7744H83.2555V27.7314C83.2555 28.7422 83.6334 29.0234 84.6793 29.0234C84.8463 29.0234 85.0045 29.0234 85.1188 29.0059V30.5C84.9605 30.5264 84.5914 30.5615 84.1959 30.5615C81.9371 30.5615 81.3131 29.7529 81.3131 27.8896V22.7744H80.0299V21.21H81.3395ZM90.3353 21.0518C93.0071 21.0518 94.4573 22.9326 94.4573 25.4639V26.2109C94.4573 28.8301 93.0159 30.6582 90.3353 30.6582C87.6546 30.6582 86.1956 28.8301 86.1956 26.2109V25.4639C86.1956 22.9414 87.6634 21.0518 90.3353 21.0518ZM90.3353 22.6162C88.8851 22.6162 88.1644 23.8027 88.1644 25.4902V26.2021C88.1644 27.8633 88.8763 29.085 90.3353 29.085C91.7943 29.085 92.4974 27.8721 92.4974 26.2021V25.4902C92.4974 23.7939 91.7855 22.6162 90.3353 22.6162ZM96.1055 30.5V21.21H98.0567V22.4316H98.127C98.3643 21.8516 99.0586 21.0781 100.351 21.0781C100.606 21.0781 100.825 21.0957 101.01 21.1309V22.8535C100.843 22.8096 100.5 22.7832 100.175 22.7832C98.6104 22.7832 98.083 23.75 98.083 24.998V30.5H96.1055ZM105.743 30.6582C103.256 30.6582 101.674 29.0146 101.674 26.2637V25.3232C101.674 22.7305 103.22 21.0518 105.664 21.0518C108.142 21.0518 109.636 22.792 109.636 25.4111V26.2988H103.598V26.5186C103.598 28.083 104.442 29.1201 105.769 29.1201C106.762 29.1201 107.439 28.6279 107.677 27.8281H109.531C109.25 29.3311 108.037 30.6582 105.743 30.6582ZM103.598 24.9365H107.729V24.9189C107.729 23.6006 106.912 22.5635 105.673 22.5635C104.416 22.5635 103.598 23.6006 103.598 24.9189V24.9365Z"

        fill="black"

      />

    </svg>

  );



  const transitionVariants: { item: Variants; container: Variants } = {

    item: {

      hidden: { opacity: 0, filter: "blur(15px)", y: 20 },

      visible: {

        opacity: 1,

        filter: "blur(0px)",

        y: 0,

        transition: {

          type: "spring" as const,

          bounce: 0.2,

          duration: 1.5,

          staggerChildren: 0.1,

          delayChildren: 0.1,

        },

      },

    },

    container: {

      hidden: { opacity: 0 },

      visible: {

        opacity: 1,

        transition: { staggerChildren: 0.12, delayChildren: 0.2 },

      },

    },

  };



  return (

    <section

      id="hero"

      className={`relative flex flex-col items-center justify-center overflow-y-hidden overflow-x-visible pt-20 pb-10 ${instrumentSerif.className}`}

    >

      <AnimatedGroup variants={transitionVariants} className="max-w-4xl">

        <div

          className={`text-center mb-8 ${

            isVisible ? "opacity-100" : "opacity-0"

          } transition-opacity duration-1000`}

        >

          <h1 className="text-6xl md:text-8xl font-medium mb-4 text-balance dark:text-white text-black">

            Make any song you can <span className="text-blue-500">imagine</span>

          </h1>

          <span className="text-2xl mb-8 mx-auto dark:text-white/70 text-black/70 text-balance">

            Start with a simple prompt, your next track is just a step away.

          </span>



          <div className="flex flex-col items-center justify-center sm:flex-row sm:space-x-4 space-y-4 sm:space-y-0 pt-5">

            {/* light-mode buttons */}

            <Link href="" className="dark:hidden shadow-lg">

              {downloadAppleLight()}

            </Link>

            <Link href="" className="dark:hidden shadow-lg">

              {downloadGoogleLight()}

            </Link>

            {/* dark-mode buttons */}

            <Link href="" className="hidden dark:block shadow-lg">

              {downloadAppleDark()}

            </Link>

            <Link href="" className="hidden dark:block shadow-lg">

              {downloadGoogleDark()}

            </Link>

          </div>

        </div>

      </AnimatedGroup>



      <TriplePhoneHero

        imageLeftSrc="https://res.cloudinary.com/harshitproject/image/upload/v1746774677/suno-left.png"

        imageCenterSrc="https://res.cloudinary.com/harshitproject/image/upload/v1746774677/suno-center.png"

        imageRightSrc="https://res.cloudinary.com/harshitproject/image/upload/v1746774678/suno-right.png"

      />



      <div className="pointer-events-none absolute inset-x-0 bottom-0 h-40 bg-gradient-to-t from-white dark:from-[#212121] to-transparent z-40" />



      <div className="absolute bottom-6 left-1/2 -translate-x-1/2 z-50 flex items-center text-sm">

        <span className="tracking-wide dark:text-white/60 text-black/60">

          Learn More

        </span>

        <MoveDown className="w-3 h-3 dark:text-white/60 text-black/60" />

      </div>

    </section>

  );

}



export default HeroSection2;
```
##### File: `src/registry/blocks/hero-section/two/triple-phone.tsx`
```tsx
"use client";



import React, { useRef } from "react";

import { motion, useInView } from "motion/react";

import Image from "next/image";

import { cn } from "@/lib/utils";



// Interface for the Iphone15Pro component props

interface Iphone15ProProps extends React.SVGProps<SVGSVGElement> {

    width?: string | number;

    height?: string | number;

    src?: string;

    alt?: string;

}



const Iphone15Pro: React.FC<Iphone15ProProps> = ({

    width = "100%",

    height = "auto",

    src,

    alt = "iPhone screen content",

    className,

    ...props

}) => {

    return (

        <div className={cn("relative", className)}>

            <svg

                width={width}

                height={height}

                viewBox="0 0 433 882"

                preserveAspectRatio="xMidYMid meet"

                fill="none"

                xmlns="http://www.w3.org/2000/svg"

                className="transition-all duration-500 ease-in-out"

                {...props}

            >

                {/* Outer frame */}

                <path

                    d="M2 73C2 32.6832 34.6832 0 75 0H357C397.317 0 430 32.6832 430 73V809C430 849.317 397.317 882 357 882H75C34.6832 882 2 849.317 2 809V73Z"

                    className="dark:fill-[#DADADA] fill-[#404040]"

                />

                {/* Side buttons */}

                <path

                    d="M0 171C0 170.448 0.447715 170 1 170H3V204H1C0.447715 204 0 203.552 0 203V171Z"

                    className="dark:fill-[#DADADA] fill-[#404040]"

                />

                <path

                    d="M1 234C1 233.448 1.44772 233 2 233H3.5V300H2C1.44772 300 1 299.552 1 299V234Z"

                    className="dark:fill-[#DADADA] fill-[#404040]"

                />

                <path

                    d="M1 319C1 318.448 1.44772 318 2 318H3.5V385H2C1.44772 385 1 384.552 1 384V319Z"

                    className="dark:fill-[#DADADA] fill-[#404040]"

                />

                <path

                    d="M430 279H432C432.552 279 433 279.448 433 280V384C433 384.552 432.552 385 432 385H430V279Z"

                    className="dark:fill-[#DADADA] fill-[#404040]"

                />



                {/* Inner body */}

                <path

                    d="M6 74C6 35.3401 37.3401 4 76 4H356C394.66 4 426 35.3401 426 74V808C426 846.66 394.66 878 356 878H76C37.3401 878 6 846.66 6 808V74Z"

                    className="fill-[#262626] dark:fill-black" // Simplified dark mode fill

                />



                {/* Top speaker grille */}

                <path

                    opacity="0.5"

                    d="M174 5H258V5.5C258 6.60457 257.105 7.5 256 7.5H176C174.895 7.5 174 6.60457 174 5.5V5Z"

                    className="dark:fill-[#DADADA] fill-[#404040]"

                />



                {/* Screen area */}

                <path

                    d="M21.25 75C21.25 44.2101 46.2101 19.25 77 19.25H355C385.79 19.25 410.75 44.2101 410.75 75V807C410.75 837.79 385.79 862.75 355 862.75H77C46.2101 862.75 21.25 837.79 21.25 807V75Z"

                    className="fill-[#111] dark:fill-[#F5F5F5]" // Screen background

                />



                {/* Screen Content Area */}

                {src && (

                    <foreignObject

                        x="21.25"

                        y="19.25"

                        width="389.5"

                        height="843.5"

                        clipPath="url(#roundedCorners)"

                    >

                        <div

                            style={{

                                width: "100%",

                                height: "100%",

                                borderRadius: "55.75px",

                                overflow: "hidden",

                                position: "relative",

                                backgroundColor: "#111",

                            }}

                            className="dark:bg-[#F5F5F5]"

                        >

                            <Image

                                src={src}

                                alt={alt}

                                fill

                                style={{ objectFit: "cover" }}

                                sizes="(max-width: 768px) 80vw, (max-width: 1200px) 50vw, 33vw"

                                priority

                            />

                        </div>

                    </foreignObject>

                )}



                {/* Notch area */}

                <path

                    d="M154 48.5C154 38.2827 162.283 30 172.5 30H259.5C269.717 30 278 38.2827 278 48.5C278 58.7173 269.717 67 259.5 67H172.5C162.283 67 154 58.7173 154 48.5Z"

                    className="fill-[#262626] dark:fill-[#F0F0F0]"

                />

                {/* Inner Notch Elements */}

                <path

                    d="M249 48.5C249 42.701 253.701 38 259.5 38C265.299 38 270 42.701 270 48.5C270 54.299 265.299 59 259.5 59C253.701 59 249 54.299 249 48.5Z"

                    className="fill-[#111] dark:fill-[#D1D1D1]" // Slightly darker for contrast

                />

                <path

                    d="M254 48.5C254 45.4624 256.462 43 259.5 43C262.538 43 265 45.4624 265 48.5C265 51.5376 262.538 54 259.5 54C256.462 54 254 51.5376 254 48.5Z"

                    className="fill-white/30 dark:fill-black" // Even darker for the lens appearance

                />



                <defs>

                    <clipPath id="roundedCorners">

                        <rect

                            x="21.25"

                            y="19.25"

                            width="389.5"

                            height="843.5"

                            rx="55.75"

                            ry="55.75"

                        />

                    </clipPath>

                </defs>

            </svg>

        </div>

    );

};



interface TriplePhoneHeroProps {

    imageLeftSrc: string;

    imageLeftAlt?: string;

    imageCenterSrc: string;

    imageCenterAlt?: string;

    imageRightSrc: string;

    imageRightAlt?: string;

}



export const TriplePhoneHero: React.FC<TriplePhoneHeroProps> = ({

    imageLeftSrc,

    imageLeftAlt = "Left phone screen content",

    imageCenterSrc,

    imageCenterAlt = "Center phone screen content",

    imageRightSrc,

    imageRightAlt = "Right phone screen content",

}) => {

    const containerRef = useRef<HTMLDivElement>(null);

    const isInView = useInView(containerRef, {

        once: true,

        margin: "-30% 0px -30% 0px",

    });



    const common = { duration: 1.2, ease: [0.4, 0, 0.2, 1] as [number, number, number, number] };



    const centerVariant = {

        hidden: { opacity: 0, y: "60%", scale: 0.85 },

        visible: {

            opacity: 1,

            y: "15%",

            scale: 1,

            transition: { ...common },

        },

    };

    const side = (dir: "left" | "right") => ({

        hidden: { opacity: 0, y: "65%", x: "0%", rotate: 0, scale: 0.85 },

        visible: {

            opacity: 0.8,

            y: "25%",

            x: dir === "left" ? "-50%" : "50%",

            rotate: dir === "left" ? -10 : 10,

            scale: 1,

            transition: { ...common, delay: 0.15 },

        },

    });



    return (

        <div

            ref={containerRef}

            className="relative flex min-h-[600px] w-full items-center justify-center"

        >

            <div className="relative flex h-full w-full max-w-4xl items-center justify-center">

                {/* left phone */}

                <motion.div

                    variants={side("left")}

                    initial="hidden"

                    animate={isInView ? "visible" : "hidden"}

                    className="absolute w-[280px] md:w-[320px] lg:w-[360px] z-10"

                >

                    <Iphone15Pro src={imageLeftSrc} alt={imageLeftAlt} />

                </motion.div>



                {/* center phone */}

                <motion.div

                    variants={centerVariant}

                    initial="hidden"

                    animate={isInView ? "visible" : "hidden"}

                    className="relative w-[300px] md:w-[340px] lg:w-[380px] z-20"

                >

                    <Iphone15Pro src={imageCenterSrc} alt={imageCenterAlt} />

                </motion.div>



                {/* right phone */}

                <motion.div

                    variants={side("right")}

                    initial="hidden"

                    animate={isInView ? "visible" : "hidden"}

                    className="absolute w-[280px] md:w-[320px] lg:w-[360px] z-10"

                >

                    <Iphone15Pro src={imageRightSrc} alt={imageRightAlt} />

                </motion.div>

            </div>

        </div>

    );

};



export default TriplePhoneHero;
```
##### File: `src/components/ui/animated-group.tsx`
```tsx
// Component From Motion Primitives



"use client";

import { ReactNode } from "react";

import { motion, Variants, HTMLMotionProps } from "motion/react";

import React from "react";



export type PresetType =

  | "fade"

  | "slide"

  | "scale"

  | "blur"

  | "blur-slide"

  | "zoom"

  | "flip"

  | "bounce"

  | "rotate"

  | "swing";



export type AnimatedGroupProps = {

  children: ReactNode;

  className?: string;

  variants?: {

    container?: Variants;

    item?: Variants;

  };

  preset?: PresetType;

  as?: React.ElementType;

  asChild?: React.ElementType;

};



const defaultContainerVariants: Variants = {

  visible: {

    transition: {

      staggerChildren: 0.1,

    },

  },

};



const defaultItemVariants: Variants = {

  hidden: { opacity: 0 },

  visible: { opacity: 1 },

};



const presetVariants: Record<PresetType, Variants> = {

  fade: {},

  slide: {

    hidden: { y: 20 },

    visible: { y: 0 },

  },

  scale: {

    hidden: { scale: 0.8 },

    visible: { scale: 1 },

  },

  blur: {

    hidden: { filter: "blur(4px)" },

    visible: { filter: "blur(0px)" },

  },

  "blur-slide": {

    hidden: { filter: "blur(4px)", y: 20 },

    visible: { filter: "blur(0px)", y: 0 },

  },

  zoom: {

    hidden: { scale: 0.5 },

    visible: {

      scale: 1,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  flip: {

    hidden: { rotateX: -90 },

    visible: {

      rotateX: 0,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  bounce: {

    hidden: { y: -50 },

    visible: {

      y: 0,

      transition: { type: "spring", stiffness: 400, damping: 10 },

    },

  },

  rotate: {

    hidden: { rotate: -180 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 200, damping: 15 },

    },

  },

  swing: {

    hidden: { rotate: -10 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 300, damping: 8 },

    },

  },

};



const mergeVariants = (

  defaultVariants: Variants,

  customVariants: Partial<Variants> = {},

): Variants => {

  return {

    hidden: {

      ...defaultVariants.hidden,

      ...customVariants.hidden,

    },

    visible: {

      ...defaultVariants.visible,

      ...customVariants.visible,

    },

  };

};



function AnimatedGroup({

  children,

  className,

  variants,

  preset = "fade",

  as = "div",

  asChild = "div",

}: AnimatedGroupProps) {

  const MotionContainer = motion.div;

  const MotionChild = motion.div;



  const presetVariant = preset ? presetVariants[preset] : {};

  const itemVariants = variants?.item

    ? mergeVariants(defaultItemVariants, variants.item)

    : mergeVariants(defaultItemVariants, presetVariant);



  const containerVariants = variants?.container || defaultContainerVariants;



  return (

    <MotionContainer

      initial="hidden"

      animate="visible"

      variants={containerVariants}

      className={className}

    >

      {React.Children.map(children, (child, index) => (

        <MotionChild key={index} variants={itemVariants}>

          {child}

        </MotionChild>

      ))}

    </MotionContainer>

  );

}



export { AnimatedGroup };
```
---

## Hero Section 3 <a name="hero-section-3"></a>

Solace UI hero section block variation 3.

- **Registry Key**: `hero-section-3`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/hero-section-3.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/hero-section/three/index.tsx`
```tsx
"use client";

import Image from "next/image";

import { AnimatedGroup } from "@/components/ui/animated-group";

import { motion, Variants } from "motion/react";

import LogoCloud from "@/registry/blocks/hero-section/logo-cloud";



export default function HeroSection3() {

  const transitionVariants: { container?: Variants; item?: Variants } = {

    item: {

      hidden: {

        opacity: 0,

        filter: "blur(12px)",

        y: 12,

      },

      visible: {

        opacity: 1,

        filter: "blur(0px)",

        y: 0,

        transition: {

          type: "spring",

          bounce: 0.3,

          duration: 1.5,

        },

      },

    },

  };



  const imageVariants: Variants = {

    hidden: {

      opacity: 0,

      y: 40,

      filter: "blur(10px)",

      scale: 0.98,

    },

    visible: {

      opacity: 1,

      y: 0,

      filter: "blur(0px)",

      scale: 1,

      transition: {

        type: "spring",

        bounce: 0.3,

        duration: 1.5,

        delay: 0.4,

      },

    },

  };



  return (

    <div className="min-h-screen max-w-7xl mx-auto [--color-primary:#003AF9] z-40">

      <div className="py-6 md:py-10 bg-linear-to-b from-(--color-primary) to-white min-h-[calc(100vh-8rem)] w-full rounded-lg">

        <AnimatedGroup

          className="flex flex-col gap-4 md:gap-2 px-4 md:px-12"

          variants={transitionVariants}

        >

          <h1 className="text-3xl sm:text-4xl md:text-5xl lg:text-6xl font-bold text-white leading-tight max-w-180">

            AI Agents That Code Like Your Best Engineer

          </h1>

          <p className="text-white/60 font-light text-lg md:text-xl tracking-tight max-w-xl pt-2 md:pt-1">

            Autonomous agents that debug, refactor, and ship features while you

            focus on architecture and strategy

          </p>

          <div className="flex flex-col sm:flex-row gap-3 md:gap-2 pt-3 w-full sm:w-auto">

            <button className="border border-white text-white px-5 py-2 rounded-md cursor-pointer text-sm md:text-base w-full sm:w-auto">

              Try for free

            </button>

            <button className="bg-black text-white px-5 py-2 rounded-md shadow-[-1px_3px_10px_0px_rgba(0,0,0,0.26)] cursor-pointer text-sm md:text-base w-full sm:w-auto">

              Book a demo

            </button>

          </div>

        </AnimatedGroup>



        <div className="px-4 md:px-10 mt-8 md:mt-10">

          <motion.div

            className="relative p-1.5 md:p-2 rounded-2xl bg-white/10 border border-white/20 shadow-2xl backdrop-blur-sm"

            initial="hidden"

            animate="visible"

            variants={imageVariants}

          >

            <Image

              src="https://res.cloudinary.com/harshitproject/image/upload/v1774017120/hero-light.png"

              alt="Dashboard Preview"

              className="rounded-xl w-full h-auto shadow-sm border border-black/5"

              width={1000}

              height={2000}

            />

            <div className="hidden md:block absolute bottom-0 left-0 w-full h-20 md:h-60 bg-linear-to-t from-white to-transparent rounded-lg">

              <div className="absolute bottom-5 md:bottom-10 left-0 w-full no-scrollbar">

                <LogoCloud className="mask-linear-to-r from-transparent via-black to-transparent pb-3 w-full" />

              </div>

            </div>

          </motion.div>

          <div className="md:hidden pt-12 pb-6  no-scrollbar">

            <LogoCloud className="mask-linear-to-r from-transparent via-black to-transparent pb-3 w-full" />

          </div>

        </div>

      </div>

    </div>

  );

}
```
##### File: `src/registry/blocks/hero-section/logo-cloud.tsx`
```tsx
import { cn } from "@/lib/utils";

import { AnimatedGroup } from "@/components/ui/animated-group";



interface LogoCloudProps extends React.HTMLAttributes<HTMLDivElement> {}



export default function LogoCloud({ className, ...props }: LogoCloudProps) {

  const logos = [

    {

      name: "nvidia",

      logo: (

        <svg

          width="85"

          height="16"

          viewBox="0 0 85 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_11_36)">

            <mask

              id="mask0_11_36"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="85"

              height="16"

            >

              <path d="M84.2105 0H0V16H84.2105V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_11_36)">

              <path

                d="M51.448 3.0305V13.5301H54.411V3.0305H51.448ZM28.1353 3.0127V13.5212H31.125V5.36177L33.4562 5.37067C34.2215 5.37067 34.7554 5.55752 35.1202 5.94904C35.5918 6.44732 35.7786 7.25704 35.7786 8.72521V13.5212H38.6794V7.71973C38.6794 3.57327 36.0367 3.0127 33.4562 3.0127H28.1353ZM56.2262 3.0305V13.5301H61.0311C63.5937 13.5301 64.4301 13.103 65.3288 12.1509C65.9695 11.4836 66.3788 10.0065 66.3788 8.39598C66.3788 6.91892 66.0318 5.60201 65.4178 4.7834C64.3323 3.31523 62.7484 3.0305 60.3815 3.0305H56.2262ZM59.1625 5.30838H60.4349C62.2857 5.30838 63.4781 6.13589 63.4781 8.2892C63.4781 10.4425 62.2857 11.2789 60.4349 11.2789H59.1625V5.30838ZM47.1769 3.0305L44.7033 11.3501L42.3364 3.0305H39.1332L42.5144 13.5301H46.7854L50.2023 3.0305H47.1769ZM67.7669 13.5301H70.7299V3.0305H67.7669V13.5301ZM76.0776 3.0305L71.94 13.5212H74.8586L75.517 11.6615H80.4109L81.0338 13.5123H84.2104L80.0372 3.0305H76.0776ZM77.9996 4.94356L79.797 9.85525H76.1488L77.9996 4.94356Z"

                fill="#1E1E1E"

              />

              <path

                d="M9.01366 4.77448V3.333C9.15603 3.32411 9.29839 3.31521 9.44076 3.31521C13.3915 3.19064 15.9808 6.71424 15.9808 6.71424C15.9808 6.71424 13.1868 10.5938 10.1882 10.5938C9.78778 10.5938 9.39627 10.5315 9.02256 10.4069V6.02909C10.5619 6.21595 10.8733 6.8922 11.7898 8.43155L13.8453 6.70534C13.8453 6.70534 12.3415 4.73889 9.81448 4.73889C9.54754 4.72999 9.2806 4.74778 9.01366 4.77448ZM9.01366 0.00515747V2.15847L9.44076 2.13178C14.9308 1.94492 18.5167 6.63416 18.5167 6.63416C18.5167 6.63416 14.4058 11.6348 10.1259 11.6348C9.75219 11.6348 9.38737 11.5992 9.02256 11.5369V12.8716C9.32509 12.9072 9.63652 12.9339 9.93905 12.9339C13.9253 12.9339 16.8083 10.8963 19.6023 8.49384C20.065 8.86755 21.9602 9.76625 22.3517 10.1578C19.7001 12.3823 13.516 14.1707 10.0102 14.1707C9.67211 14.1707 9.35178 14.153 9.03145 14.1174V15.9948H24.1758V0.00515747H9.01366ZM9.01366 10.4069V11.5458C5.32989 10.8874 4.30662 7.05236 4.30662 7.05236C4.30662 7.05236 6.07732 5.0948 9.01366 4.77448V6.0202H9.00476C7.46541 5.83334 6.25528 7.27481 6.25528 7.27481C6.25528 7.27481 6.94043 9.70396 9.01366 10.4069ZM2.47364 6.8922C2.47364 6.8922 4.65365 3.67113 9.02256 3.333V2.15847C4.18205 2.54998 0 6.64305 0 6.64305C0 6.64305 2.36686 13.4945 9.01366 14.1174V12.8716C4.13756 12.2666 2.47364 6.8922 2.47364 6.8922Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_11_36">

              <rect width="85" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

    {

      name: "column",

      logo: (

        <svg

          width="73"

          height="16"

          viewBox="0 0 73 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_12_44)">

            <mask

              id="mask0_12_44"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="73"

              height="16"

            >

              <path d="M72.8389 0H0V16H72.8389V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_12_44)">

              <path

                d="M8.46958 12.0453C8.20458 12.4776 7.84015 12.8243 7.38648 13.0756C6.93235 13.3271 6.41831 13.4546 5.85858 13.4546C5.28463 13.4546 4.75307 13.3129 4.27855 13.0335C3.80349 12.7542 3.42826 12.3648 3.16322 11.876C2.8972 11.3862 2.7623 10.8245 2.7623 10.2065C2.7623 9.60343 2.90075 9.04553 3.17384 8.54824C3.44689 8.0514 3.82265 7.65431 4.29073 7.36789C4.75845 7.08182 5.29325 6.93674 5.88027 6.93674C6.38275 6.93674 6.85363 7.04991 7.27984 7.27321C7.7065 7.49714 8.07195 7.80943 8.36597 8.20128L8.42367 8.27809L10.4527 6.57565L10.3955 6.50604C9.85453 5.84821 9.18386 5.32577 8.40206 4.95312C7.61991 4.58029 6.76416 4.3913 5.85858 4.3913C4.79219 4.3913 3.7994 4.6507 2.90782 5.16229C2.01589 5.67416 1.30068 6.38559 0.782104 7.27695C0.263131 8.16857 0 9.15416 0 10.2065C0 11.2591 0.263176 12.2411 0.782238 13.1255C1.30072 14.0097 2.01589 14.7174 2.90782 15.229C3.7994 15.7406 4.79219 16 5.85858 16C6.83679 16 7.75544 15.7814 8.58901 15.3502C9.42147 14.9196 10.1179 14.3231 10.659 13.5773L10.7133 13.5026L8.52021 11.9627L8.46958 12.0453Z"

                fill="#1E1E1E"

              />

              <path

                d="M20.3982 5.16212C19.4993 4.65061 18.4957 4.3913 17.4152 4.3913C16.3488 4.3913 15.356 4.6507 14.4645 5.16229C13.5725 5.67416 12.8573 6.38559 12.3387 7.27695C11.8198 8.16857 11.5566 9.15416 11.5566 10.2065C11.5566 11.2591 11.8198 12.2411 12.3389 13.1255C12.8574 14.0097 13.5725 14.7174 14.4645 15.229C15.356 15.7406 16.3488 16 17.4152 16C18.4957 16 19.4993 15.7407 20.3982 15.2292C21.297 14.7178 22.0159 14.01 22.5349 13.1255C23.054 12.2406 23.3171 11.2585 23.3171 10.2065C23.3171 9.15478 23.054 8.1691 22.5351 7.27695C22.0159 6.38532 21.297 5.6738 20.3982 5.16212ZM20.1436 11.8752C19.8712 12.3642 19.4921 12.7539 19.0169 13.0335C18.5421 13.3129 18.0032 13.4546 17.4152 13.4546C16.8413 13.4546 16.3097 13.3129 15.8352 13.0335C15.3601 12.7542 14.9849 12.3648 14.7199 11.876C14.4538 11.3862 14.3189 10.8245 14.3189 10.2065C14.3189 9.60343 14.4574 9.04553 14.7305 8.54824C15.0035 8.0514 15.3793 7.65431 15.8474 7.36789C16.3151 7.08182 16.8499 6.93674 17.4369 6.93674C18.0236 6.93674 18.5584 7.08182 19.0264 7.3678C19.4947 7.65457 19.8705 8.05176 20.1433 8.54824C20.4164 9.04553 20.5549 9.60343 20.5549 10.2065C20.5549 10.824 20.4165 11.3853 20.1436 11.8752Z"

                fill="#1E1E1E"

              />

              <path

                d="M37.9218 10.4884C37.9218 11.1201 37.7979 11.6671 37.5535 12.1143C37.3099 12.5597 36.9862 12.8978 36.5914 13.1192C36.1953 13.3418 35.7543 13.4546 35.2808 13.4546C34.8075 13.4546 34.3925 13.353 34.0472 13.1524C33.7023 12.9527 33.4258 12.6619 33.2253 12.2883C33.0239 11.9135 32.9218 11.4684 32.9218 10.9654V4.65149H30.1812V11.5074C30.1812 12.3577 30.3673 13.131 30.7342 13.8059C31.1019 14.4829 31.6277 15.0237 32.297 15.413C32.966 15.8025 33.7437 16 34.6087 16C35.5172 16 36.3574 15.7694 37.1061 15.3146C37.4519 15.1044 37.7692 14.8491 38.0519 14.5538V15.7398H40.6624V4.65149H37.9218V10.4884Z"

                fill="#1E1E1E"

              />

              <path

                d="M57.7883 4.94486C57.127 4.57754 56.3826 4.3913 55.576 4.3913C54.7397 4.3913 53.9657 4.5923 53.2756 4.98877C52.7464 5.2927 52.2965 5.689 51.9367 6.16788C51.6119 5.66242 51.1895 5.24835 50.6799 4.93597C50.0895 4.57452 49.4316 4.3913 48.7243 4.3913C47.9583 4.3913 47.2558 4.59692 46.6362 5.00246C46.2824 5.2343 45.9661 5.52134 45.6931 5.8579V4.6515H43.1909V15.7398H45.9532V9.25247C45.9532 8.82151 46.0482 8.4237 46.2353 8.06998C46.4219 7.71778 46.6808 7.43767 47.005 7.23756C47.3285 7.03799 47.6881 6.93674 48.0737 6.93674C48.4731 6.93674 48.8362 7.03782 49.1528 7.23712C49.4703 7.43749 49.7258 7.71769 49.9122 8.06998C50.0995 8.42361 50.1944 8.82142 50.1944 9.25247V15.7398H52.9349V9.25247C52.9349 8.82151 53.0333 8.42405 53.2274 8.07114C53.421 7.71822 53.6804 7.43758 53.9983 7.23712C54.3146 7.03782 54.6776 6.93674 55.0773 6.93674C55.477 6.93674 55.8438 7.03799 56.1677 7.23756C56.4914 7.43749 56.7465 7.71716 56.9259 8.06865C57.1063 8.42317 57.1978 8.82151 57.1978 9.25247V15.7398H59.9602V8.55864C59.9602 7.79396 59.7623 7.08493 59.372 6.45093C58.9821 5.81879 58.4494 5.31208 57.7883 4.94486Z"

                fill="#1E1E1E"

              />

              <path

                d="M72.2858 6.60712C71.9178 5.93062 71.3884 5.38622 70.7123 4.98921C70.0363 4.59247 69.2621 4.3913 68.4112 4.3913C67.4877 4.3913 66.64 4.62563 65.8917 5.08789C65.5549 5.2959 65.245 5.54526 64.968 5.83097V4.6515H62.3359V15.7398H65.0982V9.90292C65.0982 9.28465 65.2186 8.74461 65.4561 8.29773C65.6925 7.85272 66.0162 7.51119 66.4182 7.28255C66.8213 7.05311 67.2657 6.93674 67.7391 6.93674C68.1975 6.93674 68.6088 7.0419 68.9614 7.24948C69.3136 7.4566 69.5939 7.75111 69.7946 8.12465C69.9961 8.50006 70.0982 8.93787 70.0982 9.42591V15.7398H72.8388V8.90551C72.8388 8.05629 72.6527 7.28299 72.2858 6.60712Z"

                fill="#1E1E1E"

              />

              <path

                d="M23.7266 0V1.84061H24.1911C24.6972 1.84061 25.1075 2.25088 25.1075 2.75697V15.7398H27.8702V0H23.7266Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_12_44">

              <rect width="73" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

    {

      name: "github",

      logo: (

        <svg

          width="60"

          height="16"

          viewBox="0 0 60 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_12_56)">

            <mask

              id="mask0_12_56"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="60"

              height="16"

            >

              <path d="M59.1716 0H0V16H59.1716V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_12_56)">

              <path

                d="M11.3991 6.84818H6.42744C6.36582 6.84822 6.30674 6.87271 6.26318 6.91627C6.21961 6.95984 6.19512 7.01892 6.19509 7.08053V9.5113C6.19512 9.57295 6.2196 9.63206 6.26316 9.67569C6.30671 9.71931 6.36579 9.74389 6.42744 9.74402H8.3669V12.7639C8.3669 12.7639 7.93141 12.9125 6.72742 12.9125C5.30697 12.9125 3.32267 12.3933 3.32267 8.03C3.32267 3.6658 5.3889 3.09147 7.32873 3.09147C9.00789 3.09147 9.73129 3.38726 10.1915 3.52955C10.3362 3.5739 10.47 3.43001 10.47 3.30151L11.0246 0.95304C11.0246 0.893044 11.0043 0.820605 10.9358 0.771451C10.7489 0.638153 9.60846 0 6.72742 0C3.40841 0 0.00390625 1.41219 0.00390625 8.20013C0.00390625 14.9886 3.90181 16 7.18644 16C9.9061 16 11.5559 14.8378 11.5559 14.8378C11.6239 14.8001 11.6313 14.7051 11.6313 14.6615V7.08041C11.6313 6.95229 11.5274 6.84818 11.3991 6.84818ZM37.0212 0.813337C37.0214 0.782828 37.0157 0.752569 37.0042 0.724297C36.9927 0.696026 36.9758 0.6703 36.9543 0.648595C36.9329 0.626891 36.9074 0.609637 36.8792 0.597824C36.8511 0.58601 36.8209 0.579871 36.7904 0.579759H33.9909C33.9604 0.579872 33.9301 0.586003 33.9019 0.597803C33.8737 0.609603 33.8481 0.626838 33.8266 0.648528C33.8051 0.670218 33.788 0.695937 33.7765 0.724215C33.7649 0.752492 33.759 0.782776 33.7591 0.813337L33.7598 6.22321H29.3964V0.813337C29.3966 0.782817 29.3908 0.752551 29.3793 0.724279C29.3678 0.696006 29.3508 0.670282 29.3294 0.64858C29.3079 0.626879 29.2824 0.609628 29.2542 0.597818C29.2261 0.586008 29.1959 0.579871 29.1654 0.579759H26.3661C26.3045 0.580084 26.2455 0.604871 26.2021 0.648672C26.1588 0.692474 26.1346 0.751703 26.1349 0.813337V15.4618C26.1349 15.5909 26.2387 15.696 26.3661 15.696H29.1654C29.2934 15.696 29.3964 15.5907 29.3964 15.4618V9.19604H33.7598L33.7522 15.4618C33.7522 15.5909 33.8561 15.696 33.9841 15.696H36.7902C36.9184 15.696 37.0209 15.5907 37.0212 15.4618V0.813337ZM16.6816 2.73568C16.6816 1.72757 15.8734 0.913002 14.8764 0.913002C13.8804 0.913002 13.0716 1.72757 13.0716 2.73568C13.0716 3.74255 13.8804 4.55922 14.8764 4.55922C15.8734 4.55922 16.6816 3.74255 16.6816 2.73568ZM16.4814 12.3718V5.61007C16.4816 5.54843 16.4573 5.48923 16.4139 5.44546C16.3704 5.4017 16.3114 5.37694 16.2498 5.37662H13.4593C13.3313 5.37662 13.2167 5.50868 13.2167 5.63718V15.3245C13.2167 15.6093 13.3941 15.6939 13.6238 15.6939H16.1379C16.4138 15.6939 16.4814 15.5583 16.4814 15.3201V12.3718ZM47.6598 5.39867H44.8819C44.7545 5.39867 44.6508 5.50363 44.6508 5.63286V12.8154C44.6508 12.8154 43.945 13.3318 42.9434 13.3318C41.9418 13.3318 41.6761 12.8774 41.6761 11.8966V5.63286C41.6761 5.50363 41.5725 5.39867 41.4451 5.39867H38.6257C38.4985 5.39867 38.3942 5.50363 38.3942 5.63286V12.3708C38.3942 15.2839 40.0178 15.9966 42.2514 15.9966C44.0837 15.9966 45.5609 14.9844 45.5609 14.9844C45.5609 14.9844 45.6313 15.5177 45.6631 15.581C45.695 15.6441 45.7779 15.7078 45.8675 15.7078L47.6612 15.6999C47.7883 15.6999 47.8927 15.5947 47.8927 15.4661L47.8917 5.63299C47.8917 5.50363 47.7878 5.39867 47.6598 5.39867ZM54.1565 13.323C53.193 13.2936 52.5395 12.8563 52.5395 12.8563V8.21763C52.5395 8.21763 53.1841 7.82241 53.9752 7.7517C54.9755 7.66214 55.9394 7.96434 55.9394 10.3505C55.9395 12.8669 55.5045 13.3635 54.1565 13.323ZM55.2522 5.06974C53.6745 5.06974 52.6013 5.77367 52.6013 5.77367V0.813459C52.6013 0.684104 52.498 0.579759 52.3703 0.579759H49.5631C49.5325 0.579904 49.5023 0.586065 49.4742 0.597889C49.446 0.609714 49.4205 0.626971 49.399 0.648674C49.3775 0.670377 49.3605 0.6961 49.3489 0.724374C49.3374 0.752649 49.3315 0.78292 49.3317 0.813459V15.4618C49.3317 15.5909 49.4354 15.696 49.5634 15.696H51.5113C51.5989 15.696 51.6653 15.6507 51.7143 15.5716C51.7627 15.4929 51.8326 14.8963 51.8326 14.8963C51.8326 14.8963 52.9805 15.9841 55.1535 15.9841C57.7047 15.9841 59.1677 14.6901 59.1677 10.175C59.1677 5.65972 56.8311 5.06974 55.2522 5.06974ZM24.5269 5.37539H22.4271L22.4239 2.60115C22.4239 2.49618 22.3698 2.4437 22.2484 2.4437H19.3868C19.2756 2.4437 19.2159 2.49274 19.2159 2.59955V5.46643C19.2159 5.46643 17.7819 5.81248 17.6849 5.84057C17.6365 5.85459 17.594 5.88393 17.5638 5.92419C17.5336 5.96445 17.5172 6.01345 17.5172 6.0638V7.86529C17.5172 7.99489 17.6207 8.09911 17.7487 8.09911H19.2159V12.4329C19.2159 15.652 21.4738 15.9682 22.9975 15.9682C23.6936 15.9682 24.5264 15.7447 24.6639 15.6937C24.7471 15.6632 24.7954 15.5771 24.7954 15.4837L24.7977 13.502C24.7977 13.3726 24.6886 13.2683 24.5656 13.2683C24.4433 13.2683 24.1302 13.318 23.808 13.318C22.7766 13.318 22.4271 12.8384 22.4271 12.2177L22.4269 8.09899H24.5269C24.6549 8.09899 24.7585 7.99464 24.7585 7.86516V5.60859C24.7587 5.57806 24.7528 5.5478 24.7412 5.51955C24.7297 5.49129 24.7126 5.46559 24.6911 5.44393C24.6696 5.42227 24.644 5.40506 24.6159 5.3933C24.5877 5.38154 24.5575 5.37545 24.5269 5.37539Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_12_56">

              <rect width="60" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

    {

      name: "nike",

      logo: (

        <svg

          width="45"

          height="16"

          viewBox="0 0 45 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_12_63)">

            <mask

              id="mask0_12_63"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="45"

              height="16"

            >

              <path d="M44.4444 0H0V16H44.4444V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_12_63)">

              <path

                d="M5.69321 0.711516C2.80128 4.10777 0.0280227 8.31985 0.000226217 11.468C-0.0106178 12.6526 0.367757 13.6867 1.27509 14.4704C2.58028 15.5976 4.01767 15.996 5.44901 15.9978C7.54095 16.0006 9.61786 15.1566 11.2445 14.5065C13.9834 13.4109 44.2572 0.264234 44.2572 0.264234C44.5493 0.118114 44.4942 -0.0645871 44.1289 0.026695C43.9817 0.0638256 11.1708 8.95533 11.1708 8.95533C10.5375 9.13336 9.89081 9.22546 9.26104 9.22875C6.7398 9.24352 4.49604 7.84415 4.51416 4.89444C4.52109 3.74016 4.87524 2.34883 5.69321 0.711516Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_12_63">

              <rect width="45" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

    {

      name: "lemonSquezy",

      logo: (

        <svg

          width="122"

          height="16"

          viewBox="0 0 122 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_12_70)">

            <mask

              id="mask0_12_70"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="122"

              height="16"

            >

              <path d="M121.143 0H0V16H121.143V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_12_70)">

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M24.3337 7.91658H27.9357C27.6958 7.00149 27.0387 6.63046 26.2383 6.63046C25.3583 6.63046 24.6531 7.11012 24.3337 7.91658ZM30.049 9.48155H24.2059C24.4458 10.6602 25.4379 11.0945 26.3022 11.0945C27.3908 11.0945 27.8562 10.4432 27.8562 10.4432H29.8729C29.2642 11.993 27.8079 12.845 26.2384 12.845C24.0769 12.845 22.188 11.2485 22.188 8.83149C22.188 6.42858 24.0612 4.94058 26.1744 4.94058C28.2238 4.94058 30.3058 6.31989 30.049 9.48155Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M49.8991 8.89326C49.8991 7.68418 49.1625 6.73898 47.9461 6.73898C46.7287 6.73898 45.9125 7.68418 45.9125 8.89326C45.9125 10.1023 46.7287 11.0476 47.9461 11.0476C49.1625 11.0476 49.8991 10.1023 49.8991 8.89326ZM43.8149 8.90835C43.8149 6.42858 45.8004 4.94058 47.9461 4.94058C50.1076 4.94058 51.9965 6.44366 51.9965 8.87686C51.9965 11.3417 50.0424 12.8448 47.8811 12.8448C45.7039 12.8448 43.8149 11.3417 43.8149 8.90835Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M60.5665 8.56783V12.7679H58.4362V9.03223C58.4362 8.76852 58.6293 6.81595 57.092 6.73915C56.3386 6.69241 54.9944 7.09486 54.9944 9.12566V12.7679H52.8813V5.01758H54.8211L54.8275 6.09349C54.8275 6.09349 55.7021 4.94058 57.3333 4.94058C59.3985 4.94058 60.5665 6.42858 60.5665 8.56783Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M68.3526 6.58378C67.6801 6.58378 67.3921 6.90938 67.3921 7.23503C67.3921 7.76138 68.1126 7.91658 68.5926 8.01001C70.0189 8.30395 71.4595 8.72303 71.4595 10.3649C71.4595 11.9615 70.0983 12.845 68.4492 12.845C66.6086 12.845 65.1835 11.7608 65.0869 10.1176H67.0555C67.1041 10.582 67.4246 11.1865 68.4012 11.1865C69.2172 11.1865 69.4103 10.7689 69.4103 10.4432C69.4103 9.86898 68.8492 9.69852 68.3046 9.57481C67.3606 9.37292 65.3269 9.00195 65.3269 7.23503C65.3269 5.71555 66.8326 4.94058 68.3852 4.94058C70.1778 4.94058 71.3629 5.99446 71.4595 7.29681H69.4892C69.4258 7.03309 69.1703 6.58378 68.3526 6.58378Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M77.9322 8.89326C77.9322 7.60601 77.2111 6.84743 76.1711 6.84743C75.1945 6.84743 74.2168 7.52898 74.2168 8.89326C74.2168 10.2564 75.1945 10.9391 76.1711 10.9391C77.2111 10.9391 77.9322 10.1794 77.9322 8.89326ZM79.9334 5.01758V15.8675H77.9322V12.0081C77.4202 12.5659 76.6991 12.8448 75.8659 12.8448C73.8339 12.8448 72.1362 11.2333 72.1362 8.89326C72.1362 6.55212 73.8339 4.94058 75.8659 4.94058C77.4637 4.94058 78.0619 5.97858 78.0619 5.97858L78.0585 5.01758H79.9334Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M91.3737 7.91658H94.976C94.736 7.00149 94.0789 6.63046 93.2789 6.63046C92.3989 6.63046 91.6932 7.11012 91.3737 7.91658ZM97.0892 9.48155H91.2452C91.4863 10.6602 92.4783 11.0945 93.3423 11.0945C94.4309 11.0945 94.8966 10.4432 94.8966 10.4432H96.9132C96.3046 11.993 94.848 12.845 93.2789 12.845C91.1172 12.845 89.228 11.2485 89.228 8.83149C89.228 6.42858 91.1017 4.94058 93.2149 4.94058C95.264 4.94058 97.3463 6.31989 97.0892 9.48155Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M99.8801 7.91658H103.482C103.242 7.00149 102.585 6.63046 101.785 6.63046C100.905 6.63046 100.2 7.11012 99.8801 7.91658ZM105.596 9.48155H99.7527C99.9921 10.6602 100.984 11.0945 101.849 11.0945C102.937 11.0945 103.402 10.4432 103.402 10.4432H105.42C104.81 11.993 103.354 12.845 101.785 12.845C99.6235 12.845 97.7344 11.2485 97.7344 8.83149C97.7344 6.42858 99.6075 4.94058 101.721 4.94058C103.77 4.94058 105.852 6.31989 105.596 9.48155Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M112.724 11.0325V12.7678H105.92V11.7444L109.779 6.75417H106.08V5.01758H112.564V6.04097L108.706 11.0325H112.724Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M121.053 5.01758V11.8378V11.993C121.053 14.0855 119.948 15.9459 117.211 15.9459C114.649 15.9459 113.256 14.3177 113.256 13.0936H115.257C115.257 13.0936 115.561 14.1322 117.147 14.1322C118.492 14.1322 119.052 13.3875 119.052 12.3034V11.9778C118.699 12.3653 118.028 12.845 116.827 12.845C114.729 12.845 113.417 11.3734 113.417 9.21892L113.4 5.01758H115.481V8.75332C115.481 9.80703 115.867 11.0478 117.195 11.0478C117.883 11.0478 119.02 10.7221 119.02 8.65989V5.01758H121.053Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M20.6641 10.4413C20.6641 10.9246 20.8545 11.124 21.2078 11.124C21.4567 11.124 21.6184 11.0951 21.8543 11.0243L21.9861 12.6171C21.5453 12.7584 21.1192 12.8442 20.5755 12.8442C19.3279 12.8442 18.564 12.4467 18.564 10.7253V1.91797H20.6641V10.4413Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M42.9113 8.56783V12.7679H40.7811V9.03223C40.7811 7.96326 40.8934 6.61423 39.4201 6.73915C39.0369 6.76943 37.9796 6.93966 37.9796 9.12566V12.7679H35.8664V9.03223C35.8664 7.96326 35.9785 6.61423 34.5054 6.73915C34.1208 6.76943 33.0649 6.93966 33.0649 9.12566V12.7679H30.9517V5.01758H32.8917L32.8934 6.09349C32.8934 6.09349 33.6348 4.94057 35.0177 4.94057C36.684 4.94057 37.3381 6.19566 37.3381 6.19566C37.3381 6.19566 38.0555 4.92551 39.8855 4.92551C41.9662 4.92551 42.9113 6.41349 42.9113 8.56783Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M80.8315 9.21772V5.01758H82.9618V8.75332C82.9618 9.01697 82.7687 10.9695 84.3058 11.0464C85.0595 11.0931 86.4035 10.6906 86.4035 8.65989V5.01758H88.5167V12.7679H86.5795L86.5704 11.6921C86.5704 11.6921 85.6955 12.845 84.0647 12.845C81.9995 12.845 80.8315 11.357 80.8315 9.21772Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M3.95953 9.82034L8.25169 11.8047C8.78369 12.0508 9.15918 12.4638 9.36198 12.9375C9.87489 14.1371 9.17387 15.3639 8.07341 15.8052C6.97272 16.2462 5.79969 15.9624 5.26631 14.7149L3.39836 10.3352C3.25361 9.99571 3.61724 9.66211 3.95953 9.82034Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M4.2166 8.53576L8.64726 6.8609C10.1198 6.30427 11.7283 7.35747 11.7066 8.88776C11.7062 8.90776 11.7059 8.9277 11.7054 8.94787C11.6735 10.4381 10.1098 11.4397 8.6696 10.9125L4.2208 9.28416C3.86592 9.15433 3.8633 8.6693 4.2166 8.53576Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M3.96857 7.95566L8.32406 6.10497C9.77137 5.48992 10.1387 3.64397 9.00514 2.57739C8.99029 2.56334 8.97543 2.54946 8.9604 2.53559C7.84903 1.50404 6.01189 1.86724 5.37919 3.22594L3.4247 7.42371C3.26876 7.75846 3.6212 8.1032 3.96857 7.95566Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M2.84771 7.22434L4.43123 2.88237C4.62755 2.344 4.59119 1.79497 4.38822 1.32126C3.87425 0.12216 2.48234 -0.264902 1.38202 0.176995C0.281877 0.619063 -0.339783 1.62302 0.194641 2.87002L2.07483 7.24497C2.22063 7.584 2.72149 7.57063 2.84771 7.22434Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_12_70">

              <rect width="122" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

  ];



  return (

    <AnimatedGroup

      className={cn(

        "flex items-center justify-center  md:gap-10 lg:gap-20 overflow-x-auto md:px-4",

        className,

      )}

      variants={{

        container: {

          hidden: { opacity: 0 },

          visible: {

            opacity: 1,

            transition: {

              staggerChildren: 0.05,

              delayChildren: 1.5,

            },

          },

        },

        item: {

          hidden: {

            opacity: 0,

            filter: "blur(12px)",

            y: -60,

            rotateX: 90,

          },

          visible: {

            opacity: 1,

            filter: "blur(0px)",

            y: 0,

            rotateX: 0,

            transition: {

              type: "spring",

              bounce: 0.3,

              duration: 1,

            },

          },

        },

      }}

      {...props}

    >

      {logos.map((item) => (

        <div

          key={item.name}

          className="shrink-0 scale-60 md:scale-100 dark:invert"

        >

          {item.logo}

        </div>

      ))}

    </AnimatedGroup>

  );

}
```
##### File: `src/components/ui/animated-group.tsx`
```tsx
// Component From Motion Primitives



"use client";

import { ReactNode } from "react";

import { motion, Variants, HTMLMotionProps } from "motion/react";

import React from "react";



export type PresetType =

  | "fade"

  | "slide"

  | "scale"

  | "blur"

  | "blur-slide"

  | "zoom"

  | "flip"

  | "bounce"

  | "rotate"

  | "swing";



export type AnimatedGroupProps = {

  children: ReactNode;

  className?: string;

  variants?: {

    container?: Variants;

    item?: Variants;

  };

  preset?: PresetType;

  as?: React.ElementType;

  asChild?: React.ElementType;

};



const defaultContainerVariants: Variants = {

  visible: {

    transition: {

      staggerChildren: 0.1,

    },

  },

};



const defaultItemVariants: Variants = {

  hidden: { opacity: 0 },

  visible: { opacity: 1 },

};



const presetVariants: Record<PresetType, Variants> = {

  fade: {},

  slide: {

    hidden: { y: 20 },

    visible: { y: 0 },

  },

  scale: {

    hidden: { scale: 0.8 },

    visible: { scale: 1 },

  },

  blur: {

    hidden: { filter: "blur(4px)" },

    visible: { filter: "blur(0px)" },

  },

  "blur-slide": {

    hidden: { filter: "blur(4px)", y: 20 },

    visible: { filter: "blur(0px)", y: 0 },

  },

  zoom: {

    hidden: { scale: 0.5 },

    visible: {

      scale: 1,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  flip: {

    hidden: { rotateX: -90 },

    visible: {

      rotateX: 0,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  bounce: {

    hidden: { y: -50 },

    visible: {

      y: 0,

      transition: { type: "spring", stiffness: 400, damping: 10 },

    },

  },

  rotate: {

    hidden: { rotate: -180 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 200, damping: 15 },

    },

  },

  swing: {

    hidden: { rotate: -10 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 300, damping: 8 },

    },

  },

};



const mergeVariants = (

  defaultVariants: Variants,

  customVariants: Partial<Variants> = {},

): Variants => {

  return {

    hidden: {

      ...defaultVariants.hidden,

      ...customVariants.hidden,

    },

    visible: {

      ...defaultVariants.visible,

      ...customVariants.visible,

    },

  };

};



function AnimatedGroup({

  children,

  className,

  variants,

  preset = "fade",

  as = "div",

  asChild = "div",

}: AnimatedGroupProps) {

  const MotionContainer = motion.div;

  const MotionChild = motion.div;



  const presetVariant = preset ? presetVariants[preset] : {};

  const itemVariants = variants?.item

    ? mergeVariants(defaultItemVariants, variants.item)

    : mergeVariants(defaultItemVariants, presetVariant);



  const containerVariants = variants?.container || defaultContainerVariants;



  return (

    <MotionContainer

      initial="hidden"

      animate="visible"

      variants={containerVariants}

      className={className}

    >

      {React.Children.map(children, (child, index) => (

        <MotionChild key={index} variants={itemVariants}>

          {child}

        </MotionChild>

      ))}

    </MotionContainer>

  );

}



export { AnimatedGroup };
```
---

## Hero Section 4 <a name="hero-section-4"></a>

Solace UI hero section block variation 4.

- **Registry Key**: `hero-section-4`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/hero-section-4.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/hero-section/four/index.tsx`
```tsx
"use client";



import Image from "next/image";

import { AnimatedGroup } from "@/components/ui/animated-group";

import { motion } from "motion/react";

import LogoCloud from "@/registry/blocks/hero-section/logo-cloud";



export default function HeroSection4() {

  return (

    <div className="relative md:h-screen h-[500px] w-full overflow-hidden bg-white dark:bg-(--color-background) [--color-primary:#003AF9] flex flex-col items-center justify-start pt-12 md:pt-10">

      {/* Text Content */}

      <div className="container mx-auto px-4 flex flex-col items-center z-10 shrink-0">

        <AnimatedGroup

          className="flex flex-col items-center text-center max-w-4xl mx-auto gap-3"

          preset="fade"

        >

          <h1 className="text-4xl md:text-6xl font-bold tracking-tighter text-black dark:text-white leading-tight max-w-3xl">

            AI Agents That Code Like Your Best Engineer

          </h1>

          <p className="text-lg md:text-xl text-neutral-600 dark:text-neutral-400 max-w-lg">

            Autonomous agents that debug, refactor, and ship features while you

            focus on architecture and strategy

          </p>



          <div className="flex flex-row items-center gap-4 mt-4 w-auto">

            <button className="px-4 py-2 rounded-md bg-(--color-primary) text-white dark:text-white font-medium hover:opacity-90 transition-opacity cursor-pointer text-sm md:text-base whitespace-nowrap">

              Book a demo

            </button>

            <button className="border border-neutral-300 text-black dark:text-white px-4 py-2 rounded-md dark:hover:border-(--color-primary) transition-colors cursor-pointer text-sm md:text-base whitespace-nowrap">

              Try for free

            </button>

          </div>

        </AnimatedGroup>

      </div>



      {/* Hero Image Section - No grow, just flows naturally and gets clipped */}

      <div

        className="relative w-full flex justify-center mt-12"

        style={{ perspective: "1000px" }}

      >

        <div className="relative w-full max-w-7xl mx-auto">

          <motion.div

            initial={{ opacity: 0, y: 60 }}

            animate={{ opacity: 1, y: 0 }}

            transition={{

              duration: 1.2,

              delay: 0.3,

              type: "spring",

              bounce: 0.3,

            }}

            className="w-full"

          >

            {/* 3D Rotation Wrapper */}

            <div

              style={{

                transformStyle: "preserve-3d",

                transform: "rotateX(-12deg) rotateY(-30deg) rotateZ(-5deg)",

                transformOrigin: "center bottom",

              }}

              className="mx-auto w-full flex justify-center ml-[30px]"

            >

              {/* Tilted Outer Container - Blue Frame */}

              <div className="relative py-2 px-4 md:py-5 md:px-6 rounded-xl bg-(--color-primary) shadow-[-9px_9px_0px_rgba(4,16,255,0.25)] dark:shadow-[-9px_9px_0px_rgba(4,16,255,0.5)] w-[90%] md:w-full max-w-7xl ml-4">

                <div>

                  <div className="absolute top-2 left-6 size-2 rounded-full bg-white/20"></div>

                  <div className="absolute top-2 left-10 size-2 rounded-full bg-white/20"></div>

                  <div className="absolute top-2 left-14 size-2 rounded-full bg-white/20"></div>

                </div>

                {/* Inner Image */}

                <div className="relative overflow-hidden rounded-lg bg-white shadow-lg">

                  <Image

                    src="https://res.cloudinary.com/harshitproject/image/upload/v1774017120/hero-light.png"

                    alt="Dashboard Preview"

                    width={1000}

                    height={1000}

                    className="w-full h-auto object-cover"

                    priority

                  />

                </div>

              </div>

            </div>

          </motion.div>

        </div>

      </div>



      {/* Fade at the bottom - always at viewport bottom */}

      <div className="absolute bottom-0 left-0 w-full h-32 md:h-96 bg-linear-to-t from-white dark:from-(--color-background) to-transparent pointer-events-none z-20"></div>



      {/* Logo Cloud - positioned at bottom of viewport */}

      <div className="absolute bottom-4 md:bottom-28 left-0 w-full z-30">

        <LogoCloud className="mask-linear-to-r from-transparent via-black to-transparent pb-3 w-full" />

      </div>

    </div>

  );

}
```
##### File: `src/registry/blocks/hero-section/logo-cloud.tsx`
```tsx
import { cn } from "@/lib/utils";

import { AnimatedGroup } from "@/components/ui/animated-group";



interface LogoCloudProps extends React.HTMLAttributes<HTMLDivElement> {}



export default function LogoCloud({ className, ...props }: LogoCloudProps) {

  const logos = [

    {

      name: "nvidia",

      logo: (

        <svg

          width="85"

          height="16"

          viewBox="0 0 85 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_11_36)">

            <mask

              id="mask0_11_36"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="85"

              height="16"

            >

              <path d="M84.2105 0H0V16H84.2105V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_11_36)">

              <path

                d="M51.448 3.0305V13.5301H54.411V3.0305H51.448ZM28.1353 3.0127V13.5212H31.125V5.36177L33.4562 5.37067C34.2215 5.37067 34.7554 5.55752 35.1202 5.94904C35.5918 6.44732 35.7786 7.25704 35.7786 8.72521V13.5212H38.6794V7.71973C38.6794 3.57327 36.0367 3.0127 33.4562 3.0127H28.1353ZM56.2262 3.0305V13.5301H61.0311C63.5937 13.5301 64.4301 13.103 65.3288 12.1509C65.9695 11.4836 66.3788 10.0065 66.3788 8.39598C66.3788 6.91892 66.0318 5.60201 65.4178 4.7834C64.3323 3.31523 62.7484 3.0305 60.3815 3.0305H56.2262ZM59.1625 5.30838H60.4349C62.2857 5.30838 63.4781 6.13589 63.4781 8.2892C63.4781 10.4425 62.2857 11.2789 60.4349 11.2789H59.1625V5.30838ZM47.1769 3.0305L44.7033 11.3501L42.3364 3.0305H39.1332L42.5144 13.5301H46.7854L50.2023 3.0305H47.1769ZM67.7669 13.5301H70.7299V3.0305H67.7669V13.5301ZM76.0776 3.0305L71.94 13.5212H74.8586L75.517 11.6615H80.4109L81.0338 13.5123H84.2104L80.0372 3.0305H76.0776ZM77.9996 4.94356L79.797 9.85525H76.1488L77.9996 4.94356Z"

                fill="#1E1E1E"

              />

              <path

                d="M9.01366 4.77448V3.333C9.15603 3.32411 9.29839 3.31521 9.44076 3.31521C13.3915 3.19064 15.9808 6.71424 15.9808 6.71424C15.9808 6.71424 13.1868 10.5938 10.1882 10.5938C9.78778 10.5938 9.39627 10.5315 9.02256 10.4069V6.02909C10.5619 6.21595 10.8733 6.8922 11.7898 8.43155L13.8453 6.70534C13.8453 6.70534 12.3415 4.73889 9.81448 4.73889C9.54754 4.72999 9.2806 4.74778 9.01366 4.77448ZM9.01366 0.00515747V2.15847L9.44076 2.13178C14.9308 1.94492 18.5167 6.63416 18.5167 6.63416C18.5167 6.63416 14.4058 11.6348 10.1259 11.6348C9.75219 11.6348 9.38737 11.5992 9.02256 11.5369V12.8716C9.32509 12.9072 9.63652 12.9339 9.93905 12.9339C13.9253 12.9339 16.8083 10.8963 19.6023 8.49384C20.065 8.86755 21.9602 9.76625 22.3517 10.1578C19.7001 12.3823 13.516 14.1707 10.0102 14.1707C9.67211 14.1707 9.35178 14.153 9.03145 14.1174V15.9948H24.1758V0.00515747H9.01366ZM9.01366 10.4069V11.5458C5.32989 10.8874 4.30662 7.05236 4.30662 7.05236C4.30662 7.05236 6.07732 5.0948 9.01366 4.77448V6.0202H9.00476C7.46541 5.83334 6.25528 7.27481 6.25528 7.27481C6.25528 7.27481 6.94043 9.70396 9.01366 10.4069ZM2.47364 6.8922C2.47364 6.8922 4.65365 3.67113 9.02256 3.333V2.15847C4.18205 2.54998 0 6.64305 0 6.64305C0 6.64305 2.36686 13.4945 9.01366 14.1174V12.8716C4.13756 12.2666 2.47364 6.8922 2.47364 6.8922Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_11_36">

              <rect width="85" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

    {

      name: "column",

      logo: (

        <svg

          width="73"

          height="16"

          viewBox="0 0 73 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_12_44)">

            <mask

              id="mask0_12_44"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="73"

              height="16"

            >

              <path d="M72.8389 0H0V16H72.8389V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_12_44)">

              <path

                d="M8.46958 12.0453C8.20458 12.4776 7.84015 12.8243 7.38648 13.0756C6.93235 13.3271 6.41831 13.4546 5.85858 13.4546C5.28463 13.4546 4.75307 13.3129 4.27855 13.0335C3.80349 12.7542 3.42826 12.3648 3.16322 11.876C2.8972 11.3862 2.7623 10.8245 2.7623 10.2065C2.7623 9.60343 2.90075 9.04553 3.17384 8.54824C3.44689 8.0514 3.82265 7.65431 4.29073 7.36789C4.75845 7.08182 5.29325 6.93674 5.88027 6.93674C6.38275 6.93674 6.85363 7.04991 7.27984 7.27321C7.7065 7.49714 8.07195 7.80943 8.36597 8.20128L8.42367 8.27809L10.4527 6.57565L10.3955 6.50604C9.85453 5.84821 9.18386 5.32577 8.40206 4.95312C7.61991 4.58029 6.76416 4.3913 5.85858 4.3913C4.79219 4.3913 3.7994 4.6507 2.90782 5.16229C2.01589 5.67416 1.30068 6.38559 0.782104 7.27695C0.263131 8.16857 0 9.15416 0 10.2065C0 11.2591 0.263176 12.2411 0.782238 13.1255C1.30072 14.0097 2.01589 14.7174 2.90782 15.229C3.7994 15.7406 4.79219 16 5.85858 16C6.83679 16 7.75544 15.7814 8.58901 15.3502C9.42147 14.9196 10.1179 14.3231 10.659 13.5773L10.7133 13.5026L8.52021 11.9627L8.46958 12.0453Z"

                fill="#1E1E1E"

              />

              <path

                d="M20.3982 5.16212C19.4993 4.65061 18.4957 4.3913 17.4152 4.3913C16.3488 4.3913 15.356 4.6507 14.4645 5.16229C13.5725 5.67416 12.8573 6.38559 12.3387 7.27695C11.8198 8.16857 11.5566 9.15416 11.5566 10.2065C11.5566 11.2591 11.8198 12.2411 12.3389 13.1255C12.8574 14.0097 13.5725 14.7174 14.4645 15.229C15.356 15.7406 16.3488 16 17.4152 16C18.4957 16 19.4993 15.7407 20.3982 15.2292C21.297 14.7178 22.0159 14.01 22.5349 13.1255C23.054 12.2406 23.3171 11.2585 23.3171 10.2065C23.3171 9.15478 23.054 8.1691 22.5351 7.27695C22.0159 6.38532 21.297 5.6738 20.3982 5.16212ZM20.1436 11.8752C19.8712 12.3642 19.4921 12.7539 19.0169 13.0335C18.5421 13.3129 18.0032 13.4546 17.4152 13.4546C16.8413 13.4546 16.3097 13.3129 15.8352 13.0335C15.3601 12.7542 14.9849 12.3648 14.7199 11.876C14.4538 11.3862 14.3189 10.8245 14.3189 10.2065C14.3189 9.60343 14.4574 9.04553 14.7305 8.54824C15.0035 8.0514 15.3793 7.65431 15.8474 7.36789C16.3151 7.08182 16.8499 6.93674 17.4369 6.93674C18.0236 6.93674 18.5584 7.08182 19.0264 7.3678C19.4947 7.65457 19.8705 8.05176 20.1433 8.54824C20.4164 9.04553 20.5549 9.60343 20.5549 10.2065C20.5549 10.824 20.4165 11.3853 20.1436 11.8752Z"

                fill="#1E1E1E"

              />

              <path

                d="M37.9218 10.4884C37.9218 11.1201 37.7979 11.6671 37.5535 12.1143C37.3099 12.5597 36.9862 12.8978 36.5914 13.1192C36.1953 13.3418 35.7543 13.4546 35.2808 13.4546C34.8075 13.4546 34.3925 13.353 34.0472 13.1524C33.7023 12.9527 33.4258 12.6619 33.2253 12.2883C33.0239 11.9135 32.9218 11.4684 32.9218 10.9654V4.65149H30.1812V11.5074C30.1812 12.3577 30.3673 13.131 30.7342 13.8059C31.1019 14.4829 31.6277 15.0237 32.297 15.413C32.966 15.8025 33.7437 16 34.6087 16C35.5172 16 36.3574 15.7694 37.1061 15.3146C37.4519 15.1044 37.7692 14.8491 38.0519 14.5538V15.7398H40.6624V4.65149H37.9218V10.4884Z"

                fill="#1E1E1E"

              />

              <path

                d="M57.7883 4.94486C57.127 4.57754 56.3826 4.3913 55.576 4.3913C54.7397 4.3913 53.9657 4.5923 53.2756 4.98877C52.7464 5.2927 52.2965 5.689 51.9367 6.16788C51.6119 5.66242 51.1895 5.24835 50.6799 4.93597C50.0895 4.57452 49.4316 4.3913 48.7243 4.3913C47.9583 4.3913 47.2558 4.59692 46.6362 5.00246C46.2824 5.2343 45.9661 5.52134 45.6931 5.8579V4.6515H43.1909V15.7398H45.9532V9.25247C45.9532 8.82151 46.0482 8.4237 46.2353 8.06998C46.4219 7.71778 46.6808 7.43767 47.005 7.23756C47.3285 7.03799 47.6881 6.93674 48.0737 6.93674C48.4731 6.93674 48.8362 7.03782 49.1528 7.23712C49.4703 7.43749 49.7258 7.71769 49.9122 8.06998C50.0995 8.42361 50.1944 8.82142 50.1944 9.25247V15.7398H52.9349V9.25247C52.9349 8.82151 53.0333 8.42405 53.2274 8.07114C53.421 7.71822 53.6804 7.43758 53.9983 7.23712C54.3146 7.03782 54.6776 6.93674 55.0773 6.93674C55.477 6.93674 55.8438 7.03799 56.1677 7.23756C56.4914 7.43749 56.7465 7.71716 56.9259 8.06865C57.1063 8.42317 57.1978 8.82151 57.1978 9.25247V15.7398H59.9602V8.55864C59.9602 7.79396 59.7623 7.08493 59.372 6.45093C58.9821 5.81879 58.4494 5.31208 57.7883 4.94486Z"

                fill="#1E1E1E"

              />

              <path

                d="M72.2858 6.60712C71.9178 5.93062 71.3884 5.38622 70.7123 4.98921C70.0363 4.59247 69.2621 4.3913 68.4112 4.3913C67.4877 4.3913 66.64 4.62563 65.8917 5.08789C65.5549 5.2959 65.245 5.54526 64.968 5.83097V4.6515H62.3359V15.7398H65.0982V9.90292C65.0982 9.28465 65.2186 8.74461 65.4561 8.29773C65.6925 7.85272 66.0162 7.51119 66.4182 7.28255C66.8213 7.05311 67.2657 6.93674 67.7391 6.93674C68.1975 6.93674 68.6088 7.0419 68.9614 7.24948C69.3136 7.4566 69.5939 7.75111 69.7946 8.12465C69.9961 8.50006 70.0982 8.93787 70.0982 9.42591V15.7398H72.8388V8.90551C72.8388 8.05629 72.6527 7.28299 72.2858 6.60712Z"

                fill="#1E1E1E"

              />

              <path

                d="M23.7266 0V1.84061H24.1911C24.6972 1.84061 25.1075 2.25088 25.1075 2.75697V15.7398H27.8702V0H23.7266Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_12_44">

              <rect width="73" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

    {

      name: "github",

      logo: (

        <svg

          width="60"

          height="16"

          viewBox="0 0 60 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_12_56)">

            <mask

              id="mask0_12_56"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="60"

              height="16"

            >

              <path d="M59.1716 0H0V16H59.1716V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_12_56)">

              <path

                d="M11.3991 6.84818H6.42744C6.36582 6.84822 6.30674 6.87271 6.26318 6.91627C6.21961 6.95984 6.19512 7.01892 6.19509 7.08053V9.5113C6.19512 9.57295 6.2196 9.63206 6.26316 9.67569C6.30671 9.71931 6.36579 9.74389 6.42744 9.74402H8.3669V12.7639C8.3669 12.7639 7.93141 12.9125 6.72742 12.9125C5.30697 12.9125 3.32267 12.3933 3.32267 8.03C3.32267 3.6658 5.3889 3.09147 7.32873 3.09147C9.00789 3.09147 9.73129 3.38726 10.1915 3.52955C10.3362 3.5739 10.47 3.43001 10.47 3.30151L11.0246 0.95304C11.0246 0.893044 11.0043 0.820605 10.9358 0.771451C10.7489 0.638153 9.60846 0 6.72742 0C3.40841 0 0.00390625 1.41219 0.00390625 8.20013C0.00390625 14.9886 3.90181 16 7.18644 16C9.9061 16 11.5559 14.8378 11.5559 14.8378C11.6239 14.8001 11.6313 14.7051 11.6313 14.6615V7.08041C11.6313 6.95229 11.5274 6.84818 11.3991 6.84818ZM37.0212 0.813337C37.0214 0.782828 37.0157 0.752569 37.0042 0.724297C36.9927 0.696026 36.9758 0.6703 36.9543 0.648595C36.9329 0.626891 36.9074 0.609637 36.8792 0.597824C36.8511 0.58601 36.8209 0.579871 36.7904 0.579759H33.9909C33.9604 0.579872 33.9301 0.586003 33.9019 0.597803C33.8737 0.609603 33.8481 0.626838 33.8266 0.648528C33.8051 0.670218 33.788 0.695937 33.7765 0.724215C33.7649 0.752492 33.759 0.782776 33.7591 0.813337L33.7598 6.22321H29.3964V0.813337C29.3966 0.782817 29.3908 0.752551 29.3793 0.724279C29.3678 0.696006 29.3508 0.670282 29.3294 0.64858C29.3079 0.626879 29.2824 0.609628 29.2542 0.597818C29.2261 0.586008 29.1959 0.579871 29.1654 0.579759H26.3661C26.3045 0.580084 26.2455 0.604871 26.2021 0.648672C26.1588 0.692474 26.1346 0.751703 26.1349 0.813337V15.4618C26.1349 15.5909 26.2387 15.696 26.3661 15.696H29.1654C29.2934 15.696 29.3964 15.5907 29.3964 15.4618V9.19604H33.7598L33.7522 15.4618C33.7522 15.5909 33.8561 15.696 33.9841 15.696H36.7902C36.9184 15.696 37.0209 15.5907 37.0212 15.4618V0.813337ZM16.6816 2.73568C16.6816 1.72757 15.8734 0.913002 14.8764 0.913002C13.8804 0.913002 13.0716 1.72757 13.0716 2.73568C13.0716 3.74255 13.8804 4.55922 14.8764 4.55922C15.8734 4.55922 16.6816 3.74255 16.6816 2.73568ZM16.4814 12.3718V5.61007C16.4816 5.54843 16.4573 5.48923 16.4139 5.44546C16.3704 5.4017 16.3114 5.37694 16.2498 5.37662H13.4593C13.3313 5.37662 13.2167 5.50868 13.2167 5.63718V15.3245C13.2167 15.6093 13.3941 15.6939 13.6238 15.6939H16.1379C16.4138 15.6939 16.4814 15.5583 16.4814 15.3201V12.3718ZM47.6598 5.39867H44.8819C44.7545 5.39867 44.6508 5.50363 44.6508 5.63286V12.8154C44.6508 12.8154 43.945 13.3318 42.9434 13.3318C41.9418 13.3318 41.6761 12.8774 41.6761 11.8966V5.63286C41.6761 5.50363 41.5725 5.39867 41.4451 5.39867H38.6257C38.4985 5.39867 38.3942 5.50363 38.3942 5.63286V12.3708C38.3942 15.2839 40.0178 15.9966 42.2514 15.9966C44.0837 15.9966 45.5609 14.9844 45.5609 14.9844C45.5609 14.9844 45.6313 15.5177 45.6631 15.581C45.695 15.6441 45.7779 15.7078 45.8675 15.7078L47.6612 15.6999C47.7883 15.6999 47.8927 15.5947 47.8927 15.4661L47.8917 5.63299C47.8917 5.50363 47.7878 5.39867 47.6598 5.39867ZM54.1565 13.323C53.193 13.2936 52.5395 12.8563 52.5395 12.8563V8.21763C52.5395 8.21763 53.1841 7.82241 53.9752 7.7517C54.9755 7.66214 55.9394 7.96434 55.9394 10.3505C55.9395 12.8669 55.5045 13.3635 54.1565 13.323ZM55.2522 5.06974C53.6745 5.06974 52.6013 5.77367 52.6013 5.77367V0.813459C52.6013 0.684104 52.498 0.579759 52.3703 0.579759H49.5631C49.5325 0.579904 49.5023 0.586065 49.4742 0.597889C49.446 0.609714 49.4205 0.626971 49.399 0.648674C49.3775 0.670377 49.3605 0.6961 49.3489 0.724374C49.3374 0.752649 49.3315 0.78292 49.3317 0.813459V15.4618C49.3317 15.5909 49.4354 15.696 49.5634 15.696H51.5113C51.5989 15.696 51.6653 15.6507 51.7143 15.5716C51.7627 15.4929 51.8326 14.8963 51.8326 14.8963C51.8326 14.8963 52.9805 15.9841 55.1535 15.9841C57.7047 15.9841 59.1677 14.6901 59.1677 10.175C59.1677 5.65972 56.8311 5.06974 55.2522 5.06974ZM24.5269 5.37539H22.4271L22.4239 2.60115C22.4239 2.49618 22.3698 2.4437 22.2484 2.4437H19.3868C19.2756 2.4437 19.2159 2.49274 19.2159 2.59955V5.46643C19.2159 5.46643 17.7819 5.81248 17.6849 5.84057C17.6365 5.85459 17.594 5.88393 17.5638 5.92419C17.5336 5.96445 17.5172 6.01345 17.5172 6.0638V7.86529C17.5172 7.99489 17.6207 8.09911 17.7487 8.09911H19.2159V12.4329C19.2159 15.652 21.4738 15.9682 22.9975 15.9682C23.6936 15.9682 24.5264 15.7447 24.6639 15.6937C24.7471 15.6632 24.7954 15.5771 24.7954 15.4837L24.7977 13.502C24.7977 13.3726 24.6886 13.2683 24.5656 13.2683C24.4433 13.2683 24.1302 13.318 23.808 13.318C22.7766 13.318 22.4271 12.8384 22.4271 12.2177L22.4269 8.09899H24.5269C24.6549 8.09899 24.7585 7.99464 24.7585 7.86516V5.60859C24.7587 5.57806 24.7528 5.5478 24.7412 5.51955C24.7297 5.49129 24.7126 5.46559 24.6911 5.44393C24.6696 5.42227 24.644 5.40506 24.6159 5.3933C24.5877 5.38154 24.5575 5.37545 24.5269 5.37539Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_12_56">

              <rect width="60" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

    {

      name: "nike",

      logo: (

        <svg

          width="45"

          height="16"

          viewBox="0 0 45 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_12_63)">

            <mask

              id="mask0_12_63"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="45"

              height="16"

            >

              <path d="M44.4444 0H0V16H44.4444V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_12_63)">

              <path

                d="M5.69321 0.711516C2.80128 4.10777 0.0280227 8.31985 0.000226217 11.468C-0.0106178 12.6526 0.367757 13.6867 1.27509 14.4704C2.58028 15.5976 4.01767 15.996 5.44901 15.9978C7.54095 16.0006 9.61786 15.1566 11.2445 14.5065C13.9834 13.4109 44.2572 0.264234 44.2572 0.264234C44.5493 0.118114 44.4942 -0.0645871 44.1289 0.026695C43.9817 0.0638256 11.1708 8.95533 11.1708 8.95533C10.5375 9.13336 9.89081 9.22546 9.26104 9.22875C6.7398 9.24352 4.49604 7.84415 4.51416 4.89444C4.52109 3.74016 4.87524 2.34883 5.69321 0.711516Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_12_63">

              <rect width="45" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

    {

      name: "lemonSquezy",

      logo: (

        <svg

          width="122"

          height="16"

          viewBox="0 0 122 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_12_70)">

            <mask

              id="mask0_12_70"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="122"

              height="16"

            >

              <path d="M121.143 0H0V16H121.143V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_12_70)">

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M24.3337 7.91658H27.9357C27.6958 7.00149 27.0387 6.63046 26.2383 6.63046C25.3583 6.63046 24.6531 7.11012 24.3337 7.91658ZM30.049 9.48155H24.2059C24.4458 10.6602 25.4379 11.0945 26.3022 11.0945C27.3908 11.0945 27.8562 10.4432 27.8562 10.4432H29.8729C29.2642 11.993 27.8079 12.845 26.2384 12.845C24.0769 12.845 22.188 11.2485 22.188 8.83149C22.188 6.42858 24.0612 4.94058 26.1744 4.94058C28.2238 4.94058 30.3058 6.31989 30.049 9.48155Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M49.8991 8.89326C49.8991 7.68418 49.1625 6.73898 47.9461 6.73898C46.7287 6.73898 45.9125 7.68418 45.9125 8.89326C45.9125 10.1023 46.7287 11.0476 47.9461 11.0476C49.1625 11.0476 49.8991 10.1023 49.8991 8.89326ZM43.8149 8.90835C43.8149 6.42858 45.8004 4.94058 47.9461 4.94058C50.1076 4.94058 51.9965 6.44366 51.9965 8.87686C51.9965 11.3417 50.0424 12.8448 47.8811 12.8448C45.7039 12.8448 43.8149 11.3417 43.8149 8.90835Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M60.5665 8.56783V12.7679H58.4362V9.03223C58.4362 8.76852 58.6293 6.81595 57.092 6.73915C56.3386 6.69241 54.9944 7.09486 54.9944 9.12566V12.7679H52.8813V5.01758H54.8211L54.8275 6.09349C54.8275 6.09349 55.7021 4.94058 57.3333 4.94058C59.3985 4.94058 60.5665 6.42858 60.5665 8.56783Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M68.3526 6.58378C67.6801 6.58378 67.3921 6.90938 67.3921 7.23503C67.3921 7.76138 68.1126 7.91658 68.5926 8.01001C70.0189 8.30395 71.4595 8.72303 71.4595 10.3649C71.4595 11.9615 70.0983 12.845 68.4492 12.845C66.6086 12.845 65.1835 11.7608 65.0869 10.1176H67.0555C67.1041 10.582 67.4246 11.1865 68.4012 11.1865C69.2172 11.1865 69.4103 10.7689 69.4103 10.4432C69.4103 9.86898 68.8492 9.69852 68.3046 9.57481C67.3606 9.37292 65.3269 9.00195 65.3269 7.23503C65.3269 5.71555 66.8326 4.94058 68.3852 4.94058C70.1778 4.94058 71.3629 5.99446 71.4595 7.29681H69.4892C69.4258 7.03309 69.1703 6.58378 68.3526 6.58378Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M77.9322 8.89326C77.9322 7.60601 77.2111 6.84743 76.1711 6.84743C75.1945 6.84743 74.2168 7.52898 74.2168 8.89326C74.2168 10.2564 75.1945 10.9391 76.1711 10.9391C77.2111 10.9391 77.9322 10.1794 77.9322 8.89326ZM79.9334 5.01758V15.8675H77.9322V12.0081C77.4202 12.5659 76.6991 12.8448 75.8659 12.8448C73.8339 12.8448 72.1362 11.2333 72.1362 8.89326C72.1362 6.55212 73.8339 4.94058 75.8659 4.94058C77.4637 4.94058 78.0619 5.97858 78.0619 5.97858L78.0585 5.01758H79.9334Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M91.3737 7.91658H94.976C94.736 7.00149 94.0789 6.63046 93.2789 6.63046C92.3989 6.63046 91.6932 7.11012 91.3737 7.91658ZM97.0892 9.48155H91.2452C91.4863 10.6602 92.4783 11.0945 93.3423 11.0945C94.4309 11.0945 94.8966 10.4432 94.8966 10.4432H96.9132C96.3046 11.993 94.848 12.845 93.2789 12.845C91.1172 12.845 89.228 11.2485 89.228 8.83149C89.228 6.42858 91.1017 4.94058 93.2149 4.94058C95.264 4.94058 97.3463 6.31989 97.0892 9.48155Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M99.8801 7.91658H103.482C103.242 7.00149 102.585 6.63046 101.785 6.63046C100.905 6.63046 100.2 7.11012 99.8801 7.91658ZM105.596 9.48155H99.7527C99.9921 10.6602 100.984 11.0945 101.849 11.0945C102.937 11.0945 103.402 10.4432 103.402 10.4432H105.42C104.81 11.993 103.354 12.845 101.785 12.845C99.6235 12.845 97.7344 11.2485 97.7344 8.83149C97.7344 6.42858 99.6075 4.94058 101.721 4.94058C103.77 4.94058 105.852 6.31989 105.596 9.48155Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M112.724 11.0325V12.7678H105.92V11.7444L109.779 6.75417H106.08V5.01758H112.564V6.04097L108.706 11.0325H112.724Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M121.053 5.01758V11.8378V11.993C121.053 14.0855 119.948 15.9459 117.211 15.9459C114.649 15.9459 113.256 14.3177 113.256 13.0936H115.257C115.257 13.0936 115.561 14.1322 117.147 14.1322C118.492 14.1322 119.052 13.3875 119.052 12.3034V11.9778C118.699 12.3653 118.028 12.845 116.827 12.845C114.729 12.845 113.417 11.3734 113.417 9.21892L113.4 5.01758H115.481V8.75332C115.481 9.80703 115.867 11.0478 117.195 11.0478C117.883 11.0478 119.02 10.7221 119.02 8.65989V5.01758H121.053Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M20.6641 10.4413C20.6641 10.9246 20.8545 11.124 21.2078 11.124C21.4567 11.124 21.6184 11.0951 21.8543 11.0243L21.9861 12.6171C21.5453 12.7584 21.1192 12.8442 20.5755 12.8442C19.3279 12.8442 18.564 12.4467 18.564 10.7253V1.91797H20.6641V10.4413Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M42.9113 8.56783V12.7679H40.7811V9.03223C40.7811 7.96326 40.8934 6.61423 39.4201 6.73915C39.0369 6.76943 37.9796 6.93966 37.9796 9.12566V12.7679H35.8664V9.03223C35.8664 7.96326 35.9785 6.61423 34.5054 6.73915C34.1208 6.76943 33.0649 6.93966 33.0649 9.12566V12.7679H30.9517V5.01758H32.8917L32.8934 6.09349C32.8934 6.09349 33.6348 4.94057 35.0177 4.94057C36.684 4.94057 37.3381 6.19566 37.3381 6.19566C37.3381 6.19566 38.0555 4.92551 39.8855 4.92551C41.9662 4.92551 42.9113 6.41349 42.9113 8.56783Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M80.8315 9.21772V5.01758H82.9618V8.75332C82.9618 9.01697 82.7687 10.9695 84.3058 11.0464C85.0595 11.0931 86.4035 10.6906 86.4035 8.65989V5.01758H88.5167V12.7679H86.5795L86.5704 11.6921C86.5704 11.6921 85.6955 12.845 84.0647 12.845C81.9995 12.845 80.8315 11.357 80.8315 9.21772Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M3.95953 9.82034L8.25169 11.8047C8.78369 12.0508 9.15918 12.4638 9.36198 12.9375C9.87489 14.1371 9.17387 15.3639 8.07341 15.8052C6.97272 16.2462 5.79969 15.9624 5.26631 14.7149L3.39836 10.3352C3.25361 9.99571 3.61724 9.66211 3.95953 9.82034Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M4.2166 8.53576L8.64726 6.8609C10.1198 6.30427 11.7283 7.35747 11.7066 8.88776C11.7062 8.90776 11.7059 8.9277 11.7054 8.94787C11.6735 10.4381 10.1098 11.4397 8.6696 10.9125L4.2208 9.28416C3.86592 9.15433 3.8633 8.6693 4.2166 8.53576Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M3.96857 7.95566L8.32406 6.10497C9.77137 5.48992 10.1387 3.64397 9.00514 2.57739C8.99029 2.56334 8.97543 2.54946 8.9604 2.53559C7.84903 1.50404 6.01189 1.86724 5.37919 3.22594L3.4247 7.42371C3.26876 7.75846 3.6212 8.1032 3.96857 7.95566Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M2.84771 7.22434L4.43123 2.88237C4.62755 2.344 4.59119 1.79497 4.38822 1.32126C3.87425 0.12216 2.48234 -0.264902 1.38202 0.176995C0.281877 0.619063 -0.339783 1.62302 0.194641 2.87002L2.07483 7.24497C2.22063 7.584 2.72149 7.57063 2.84771 7.22434Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_12_70">

              <rect width="122" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

  ];



  return (

    <AnimatedGroup

      className={cn(

        "flex items-center justify-center  md:gap-10 lg:gap-20 overflow-x-auto md:px-4",

        className,

      )}

      variants={{

        container: {

          hidden: { opacity: 0 },

          visible: {

            opacity: 1,

            transition: {

              staggerChildren: 0.05,

              delayChildren: 1.5,

            },

          },

        },

        item: {

          hidden: {

            opacity: 0,

            filter: "blur(12px)",

            y: -60,

            rotateX: 90,

          },

          visible: {

            opacity: 1,

            filter: "blur(0px)",

            y: 0,

            rotateX: 0,

            transition: {

              type: "spring",

              bounce: 0.3,

              duration: 1,

            },

          },

        },

      }}

      {...props}

    >

      {logos.map((item) => (

        <div

          key={item.name}

          className="shrink-0 scale-60 md:scale-100 dark:invert"

        >

          {item.logo}

        </div>

      ))}

    </AnimatedGroup>

  );

}
```
##### File: `src/components/ui/animated-group.tsx`
```tsx
// Component From Motion Primitives



"use client";

import { ReactNode } from "react";

import { motion, Variants, HTMLMotionProps } from "motion/react";

import React from "react";



export type PresetType =

  | "fade"

  | "slide"

  | "scale"

  | "blur"

  | "blur-slide"

  | "zoom"

  | "flip"

  | "bounce"

  | "rotate"

  | "swing";



export type AnimatedGroupProps = {

  children: ReactNode;

  className?: string;

  variants?: {

    container?: Variants;

    item?: Variants;

  };

  preset?: PresetType;

  as?: React.ElementType;

  asChild?: React.ElementType;

};



const defaultContainerVariants: Variants = {

  visible: {

    transition: {

      staggerChildren: 0.1,

    },

  },

};



const defaultItemVariants: Variants = {

  hidden: { opacity: 0 },

  visible: { opacity: 1 },

};



const presetVariants: Record<PresetType, Variants> = {

  fade: {},

  slide: {

    hidden: { y: 20 },

    visible: { y: 0 },

  },

  scale: {

    hidden: { scale: 0.8 },

    visible: { scale: 1 },

  },

  blur: {

    hidden: { filter: "blur(4px)" },

    visible: { filter: "blur(0px)" },

  },

  "blur-slide": {

    hidden: { filter: "blur(4px)", y: 20 },

    visible: { filter: "blur(0px)", y: 0 },

  },

  zoom: {

    hidden: { scale: 0.5 },

    visible: {

      scale: 1,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  flip: {

    hidden: { rotateX: -90 },

    visible: {

      rotateX: 0,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  bounce: {

    hidden: { y: -50 },

    visible: {

      y: 0,

      transition: { type: "spring", stiffness: 400, damping: 10 },

    },

  },

  rotate: {

    hidden: { rotate: -180 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 200, damping: 15 },

    },

  },

  swing: {

    hidden: { rotate: -10 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 300, damping: 8 },

    },

  },

};



const mergeVariants = (

  defaultVariants: Variants,

  customVariants: Partial<Variants> = {},

): Variants => {

  return {

    hidden: {

      ...defaultVariants.hidden,

      ...customVariants.hidden,

    },

    visible: {

      ...defaultVariants.visible,

      ...customVariants.visible,

    },

  };

};



function AnimatedGroup({

  children,

  className,

  variants,

  preset = "fade",

  as = "div",

  asChild = "div",

}: AnimatedGroupProps) {

  const MotionContainer = motion.div;

  const MotionChild = motion.div;



  const presetVariant = preset ? presetVariants[preset] : {};

  const itemVariants = variants?.item

    ? mergeVariants(defaultItemVariants, variants.item)

    : mergeVariants(defaultItemVariants, presetVariant);



  const containerVariants = variants?.container || defaultContainerVariants;



  return (

    <MotionContainer

      initial="hidden"

      animate="visible"

      variants={containerVariants}

      className={className}

    >

      {React.Children.map(children, (child, index) => (

        <MotionChild key={index} variants={itemVariants}>

          {child}

        </MotionChild>

      ))}

    </MotionContainer>

  );

}



export { AnimatedGroup };
```
---

## Hero Section 5 <a name="hero-section-5"></a>

Solace UI hero section block variation 5.

- **Registry Key**: `hero-section-5`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/hero-section-5.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/hero-section/five/index.tsx`
```tsx
"use client";

import React from "react";

import Image from "next/image";

import Link from "next/link";

import LogoCloud from "@/registry/blocks/hero-section/logo-cloud";

import { ArrowRight } from "lucide-react";

import { AnimatedGroup } from "@/components/ui/animated-group";

import { motion, Variants } from "motion/react";



export default function HeroSection5() {

  const navItems = [

    { name: "Products", href: "#" },

    { name: "Customers", href: "#" },

    { name: "Solution", href: "#" },

    { name: "Pricing", href: "#" },

    { name: "Company", href: "#" },

  ];



  const transitionVariants: { container?: Variants; item?: Variants } = {

    item: {

      hidden: {

        opacity: 0,

        filter: "blur(12px)",

        y: 12,

      },

      visible: {

        opacity: 1,

        filter: "blur(0px)",

        y: 0,

        transition: {

          type: "spring",

          bounce: 0.3,

          duration: 1,

        },

      },

    },

  };



  const imageVariants: Variants = {

    hidden: {

      opacity: 0,

      y: 40,

      filter: "blur(10px)",

      scale: 0.98,

    },

    visible: {

      opacity: 1,

      y: 0,

      filter: "blur(0px)",

      scale: 1,

      transition: {

        type: "spring",

        bounce: 0.3,

        duration: 1,

        delay: 0.2,

      },

    },

  };



  return (

    <div className="w-full min-h-screen bg-white dark:bg-[#212121] [--color-primary:#003AF9]">

      <div className="max-w-3xl mx-auto border-x border-neutral-200 dark:border-white/20 min-h-screen flex flex-col relative">

        {/* 1. Navbar */}

        <nav className="w-full flex justify-between items-center py-4 px-6 border-b border-black/10 dark:border-white/20">

          <div className="font-bold text-md tracking-tight">SolaceUI</div>



          <div className="items-center gap-4 hidden md:flex">

            {navItems.map((item) => (

              <Link href={item.href} key={item.name}>

                <span className="text-sm md:text-[1rem] text-neutral-400 hover:text-black transition-colors  dark:text-white/70">

                  {item.name}

                </span>

              </Link>

            ))}

          </div>



          <div className="flex items-center gap-4">

            <Link href="#">

              <button className="px-3 py-1 text-sm font-medium border border-neutral-200 text-black dark:text-white hover:bg-neutral-200 transition-colors rounded-none">

                Log in

              </button>

            </Link>

            <Link href="#">

              <button className="px-3 py-1 text-sm font-medium bg-(--color-primary) text-white hover:bg-black/90 transition-colors rounded-none">

                Sign Up

              </button>

            </Link>

          </div>

        </nav>



        {/* 2. Upper Hero Section */}

        <AnimatedGroup

          className="flex flex-col items-center text-center mt-8 md:mt-12 px-6 relative z-10 will-change-transform"

          variants={transitionVariants}

        >

          {/* Announcement Chip */}

          <Link

            href="#"

            className="inline-flex items-center gap-2 px-2 py-1 md:px-3 md:py-1 border border-neutral-200 dark:border-white/20 rounded-none mb-8 cursor-pointer group"

          >

            <span className="md:px-2 px-2 py-0 rounded-none bg-neutral-300 dark:bg-white/10 text-neutral-400 dark:text-white text-sm group-hover:bg-neutral-900 group-hover:text-white transition-colors">

              Hiring

            </span>

            <span className="text-sm text-neutral-400  dark:text-white/20 group-hover:text-black transition-colors dark:group-hover:text-white">

              Apply for Design Engineers

            </span>



            <ArrowRight className="size-3 text-neutral-400  dark:text-white/60 group-hover:translate-x-1 transition-colors group-hover:text-black dark:group-hover:text-white" />

          </Link>



          {/* Hero Text */}

          <h1 className="text-3xl md:text-5xl font-semibold tracking-tighter text-black dark:text-white  mb-3 md:mb-5">

            AI Agents That Code Like <br className="hidden md:block" />

            Your Best Engineer

          </h1>



          {/* Sub Heading */}

          <p className="text-sm md:text-lg text-black/60 dark:text-white/80 max-w-[400px] md:max-w-lg text-center md:mb-5 mb-4 md:leading-relaxed">

            Autonomous agents that debug, refactor, and ship features while you

            focus on architecture and strategy

          </p>



          {/* Two CTA Buttons */}

          <div className="flex flex-row gap-4 mb-10">

            <button className="px-2 py-1  md:px-3 md:py-1 bg-(--color-primary) text-white text-sm md:text-lg font-medium hover:bg-blue-700 transition-colors rounded-non min-w-[100px] md:min-w-[200px]">

              Book free demo

            </button>

            <button className="px-2 py-1 md:px-3 md:py-1 border border-neutral-300 text-black dark:text-white text-sm md:text-lg font-medium hover:bg-gray-50 transition-colors rounded-none min-w-[100px] md:min-w-[200px]">

              Try it now

            </button>

          </div>

        </AnimatedGroup>



        {/* 3. Divider Shaded Div */}

        <div className="w-full h-12 border-y border-black dark:border-white opacity-10 bg-[repeating-linear-gradient(315deg,currentColor_0,currentColor_1px,transparent_0,transparent_50%)] bg-[length:10px_10px]"></div>



        {/* 4. Hero Image Container */}

        <div className="w-full relative bg-gray-50 dark:bg-[#212121] border-b border-black/10 dark:border-none">

          <motion.div

            className="max-w-5xl mx-auto overflow-hidden"

            initial="hidden"

            animate="visible"

            variants={imageVariants}

            style={{ willChange: "transform, opacity, filter" }}

          >

            <div className="relative">

              <Image

                src="https://res.cloudinary.com/harshitproject/image/upload/v1774017120/hero-light.png"

                alt="Dashboard Preview"

                width={1200}

                height={800}

                className="object-cover object-top w-full h-auto"

                priority

                sizes="(max-width: 768px) 100vw, (max-width: 1200px) 100vw, 1200px"

              />



              {/* Fade Effect - Always visible on image bottom */}

              <div className="absolute bottom-0 left-0 right-0 h-48 bg-linear-to-t from-white dark:from-black via-white/80 dark:via-[#212121]/20 to-transparent pointer-events-none" />



              {/* Overlay Logo Cloud (Visible on md to lg/xl) */}

              <div className="absolute bottom-0 left-0 right-0 h-48 hidden md:flex xl:hidden flex-col justify-end items-center pb-8 overflow-hidden z-10">

                <div className="w-full max-w-4xl px-4 opacity-80 scale-100">

                  <LogoCloud className="gap-8 lg:gap-16 pb-0 overflow-hidden" />

                </div>

              </div>

            </div>



            {/* Bottom Logo Cloud (Visible on mobile and xl+) */}

            <div className="flex md:hidden xl:flex flex-col justify-end items-center pb-8 pt-6 overflow-hidden bg-white dark:bg-[#212121] border-y dark:border-white/20 border-neutral-200">

              <div className="w-full max-w-4xl px-4 opacity-80 scale-90 xl:scale-80">

                <LogoCloud className="gap-2 pb-0 overflow-hidden" />

              </div>

            </div>

          </motion.div>

        </div>

      </div>

    </div>

  );

}
```
##### File: `src/registry/blocks/hero-section/logo-cloud.tsx`
```tsx
import { cn } from "@/lib/utils";

import { AnimatedGroup } from "@/components/ui/animated-group";



interface LogoCloudProps extends React.HTMLAttributes<HTMLDivElement> {}



export default function LogoCloud({ className, ...props }: LogoCloudProps) {

  const logos = [

    {

      name: "nvidia",

      logo: (

        <svg

          width="85"

          height="16"

          viewBox="0 0 85 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_11_36)">

            <mask

              id="mask0_11_36"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="85"

              height="16"

            >

              <path d="M84.2105 0H0V16H84.2105V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_11_36)">

              <path

                d="M51.448 3.0305V13.5301H54.411V3.0305H51.448ZM28.1353 3.0127V13.5212H31.125V5.36177L33.4562 5.37067C34.2215 5.37067 34.7554 5.55752 35.1202 5.94904C35.5918 6.44732 35.7786 7.25704 35.7786 8.72521V13.5212H38.6794V7.71973C38.6794 3.57327 36.0367 3.0127 33.4562 3.0127H28.1353ZM56.2262 3.0305V13.5301H61.0311C63.5937 13.5301 64.4301 13.103 65.3288 12.1509C65.9695 11.4836 66.3788 10.0065 66.3788 8.39598C66.3788 6.91892 66.0318 5.60201 65.4178 4.7834C64.3323 3.31523 62.7484 3.0305 60.3815 3.0305H56.2262ZM59.1625 5.30838H60.4349C62.2857 5.30838 63.4781 6.13589 63.4781 8.2892C63.4781 10.4425 62.2857 11.2789 60.4349 11.2789H59.1625V5.30838ZM47.1769 3.0305L44.7033 11.3501L42.3364 3.0305H39.1332L42.5144 13.5301H46.7854L50.2023 3.0305H47.1769ZM67.7669 13.5301H70.7299V3.0305H67.7669V13.5301ZM76.0776 3.0305L71.94 13.5212H74.8586L75.517 11.6615H80.4109L81.0338 13.5123H84.2104L80.0372 3.0305H76.0776ZM77.9996 4.94356L79.797 9.85525H76.1488L77.9996 4.94356Z"

                fill="#1E1E1E"

              />

              <path

                d="M9.01366 4.77448V3.333C9.15603 3.32411 9.29839 3.31521 9.44076 3.31521C13.3915 3.19064 15.9808 6.71424 15.9808 6.71424C15.9808 6.71424 13.1868 10.5938 10.1882 10.5938C9.78778 10.5938 9.39627 10.5315 9.02256 10.4069V6.02909C10.5619 6.21595 10.8733 6.8922 11.7898 8.43155L13.8453 6.70534C13.8453 6.70534 12.3415 4.73889 9.81448 4.73889C9.54754 4.72999 9.2806 4.74778 9.01366 4.77448ZM9.01366 0.00515747V2.15847L9.44076 2.13178C14.9308 1.94492 18.5167 6.63416 18.5167 6.63416C18.5167 6.63416 14.4058 11.6348 10.1259 11.6348C9.75219 11.6348 9.38737 11.5992 9.02256 11.5369V12.8716C9.32509 12.9072 9.63652 12.9339 9.93905 12.9339C13.9253 12.9339 16.8083 10.8963 19.6023 8.49384C20.065 8.86755 21.9602 9.76625 22.3517 10.1578C19.7001 12.3823 13.516 14.1707 10.0102 14.1707C9.67211 14.1707 9.35178 14.153 9.03145 14.1174V15.9948H24.1758V0.00515747H9.01366ZM9.01366 10.4069V11.5458C5.32989 10.8874 4.30662 7.05236 4.30662 7.05236C4.30662 7.05236 6.07732 5.0948 9.01366 4.77448V6.0202H9.00476C7.46541 5.83334 6.25528 7.27481 6.25528 7.27481C6.25528 7.27481 6.94043 9.70396 9.01366 10.4069ZM2.47364 6.8922C2.47364 6.8922 4.65365 3.67113 9.02256 3.333V2.15847C4.18205 2.54998 0 6.64305 0 6.64305C0 6.64305 2.36686 13.4945 9.01366 14.1174V12.8716C4.13756 12.2666 2.47364 6.8922 2.47364 6.8922Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_11_36">

              <rect width="85" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

    {

      name: "column",

      logo: (

        <svg

          width="73"

          height="16"

          viewBox="0 0 73 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_12_44)">

            <mask

              id="mask0_12_44"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="73"

              height="16"

            >

              <path d="M72.8389 0H0V16H72.8389V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_12_44)">

              <path

                d="M8.46958 12.0453C8.20458 12.4776 7.84015 12.8243 7.38648 13.0756C6.93235 13.3271 6.41831 13.4546 5.85858 13.4546C5.28463 13.4546 4.75307 13.3129 4.27855 13.0335C3.80349 12.7542 3.42826 12.3648 3.16322 11.876C2.8972 11.3862 2.7623 10.8245 2.7623 10.2065C2.7623 9.60343 2.90075 9.04553 3.17384 8.54824C3.44689 8.0514 3.82265 7.65431 4.29073 7.36789C4.75845 7.08182 5.29325 6.93674 5.88027 6.93674C6.38275 6.93674 6.85363 7.04991 7.27984 7.27321C7.7065 7.49714 8.07195 7.80943 8.36597 8.20128L8.42367 8.27809L10.4527 6.57565L10.3955 6.50604C9.85453 5.84821 9.18386 5.32577 8.40206 4.95312C7.61991 4.58029 6.76416 4.3913 5.85858 4.3913C4.79219 4.3913 3.7994 4.6507 2.90782 5.16229C2.01589 5.67416 1.30068 6.38559 0.782104 7.27695C0.263131 8.16857 0 9.15416 0 10.2065C0 11.2591 0.263176 12.2411 0.782238 13.1255C1.30072 14.0097 2.01589 14.7174 2.90782 15.229C3.7994 15.7406 4.79219 16 5.85858 16C6.83679 16 7.75544 15.7814 8.58901 15.3502C9.42147 14.9196 10.1179 14.3231 10.659 13.5773L10.7133 13.5026L8.52021 11.9627L8.46958 12.0453Z"

                fill="#1E1E1E"

              />

              <path

                d="M20.3982 5.16212C19.4993 4.65061 18.4957 4.3913 17.4152 4.3913C16.3488 4.3913 15.356 4.6507 14.4645 5.16229C13.5725 5.67416 12.8573 6.38559 12.3387 7.27695C11.8198 8.16857 11.5566 9.15416 11.5566 10.2065C11.5566 11.2591 11.8198 12.2411 12.3389 13.1255C12.8574 14.0097 13.5725 14.7174 14.4645 15.229C15.356 15.7406 16.3488 16 17.4152 16C18.4957 16 19.4993 15.7407 20.3982 15.2292C21.297 14.7178 22.0159 14.01 22.5349 13.1255C23.054 12.2406 23.3171 11.2585 23.3171 10.2065C23.3171 9.15478 23.054 8.1691 22.5351 7.27695C22.0159 6.38532 21.297 5.6738 20.3982 5.16212ZM20.1436 11.8752C19.8712 12.3642 19.4921 12.7539 19.0169 13.0335C18.5421 13.3129 18.0032 13.4546 17.4152 13.4546C16.8413 13.4546 16.3097 13.3129 15.8352 13.0335C15.3601 12.7542 14.9849 12.3648 14.7199 11.876C14.4538 11.3862 14.3189 10.8245 14.3189 10.2065C14.3189 9.60343 14.4574 9.04553 14.7305 8.54824C15.0035 8.0514 15.3793 7.65431 15.8474 7.36789C16.3151 7.08182 16.8499 6.93674 17.4369 6.93674C18.0236 6.93674 18.5584 7.08182 19.0264 7.3678C19.4947 7.65457 19.8705 8.05176 20.1433 8.54824C20.4164 9.04553 20.5549 9.60343 20.5549 10.2065C20.5549 10.824 20.4165 11.3853 20.1436 11.8752Z"

                fill="#1E1E1E"

              />

              <path

                d="M37.9218 10.4884C37.9218 11.1201 37.7979 11.6671 37.5535 12.1143C37.3099 12.5597 36.9862 12.8978 36.5914 13.1192C36.1953 13.3418 35.7543 13.4546 35.2808 13.4546C34.8075 13.4546 34.3925 13.353 34.0472 13.1524C33.7023 12.9527 33.4258 12.6619 33.2253 12.2883C33.0239 11.9135 32.9218 11.4684 32.9218 10.9654V4.65149H30.1812V11.5074C30.1812 12.3577 30.3673 13.131 30.7342 13.8059C31.1019 14.4829 31.6277 15.0237 32.297 15.413C32.966 15.8025 33.7437 16 34.6087 16C35.5172 16 36.3574 15.7694 37.1061 15.3146C37.4519 15.1044 37.7692 14.8491 38.0519 14.5538V15.7398H40.6624V4.65149H37.9218V10.4884Z"

                fill="#1E1E1E"

              />

              <path

                d="M57.7883 4.94486C57.127 4.57754 56.3826 4.3913 55.576 4.3913C54.7397 4.3913 53.9657 4.5923 53.2756 4.98877C52.7464 5.2927 52.2965 5.689 51.9367 6.16788C51.6119 5.66242 51.1895 5.24835 50.6799 4.93597C50.0895 4.57452 49.4316 4.3913 48.7243 4.3913C47.9583 4.3913 47.2558 4.59692 46.6362 5.00246C46.2824 5.2343 45.9661 5.52134 45.6931 5.8579V4.6515H43.1909V15.7398H45.9532V9.25247C45.9532 8.82151 46.0482 8.4237 46.2353 8.06998C46.4219 7.71778 46.6808 7.43767 47.005 7.23756C47.3285 7.03799 47.6881 6.93674 48.0737 6.93674C48.4731 6.93674 48.8362 7.03782 49.1528 7.23712C49.4703 7.43749 49.7258 7.71769 49.9122 8.06998C50.0995 8.42361 50.1944 8.82142 50.1944 9.25247V15.7398H52.9349V9.25247C52.9349 8.82151 53.0333 8.42405 53.2274 8.07114C53.421 7.71822 53.6804 7.43758 53.9983 7.23712C54.3146 7.03782 54.6776 6.93674 55.0773 6.93674C55.477 6.93674 55.8438 7.03799 56.1677 7.23756C56.4914 7.43749 56.7465 7.71716 56.9259 8.06865C57.1063 8.42317 57.1978 8.82151 57.1978 9.25247V15.7398H59.9602V8.55864C59.9602 7.79396 59.7623 7.08493 59.372 6.45093C58.9821 5.81879 58.4494 5.31208 57.7883 4.94486Z"

                fill="#1E1E1E"

              />

              <path

                d="M72.2858 6.60712C71.9178 5.93062 71.3884 5.38622 70.7123 4.98921C70.0363 4.59247 69.2621 4.3913 68.4112 4.3913C67.4877 4.3913 66.64 4.62563 65.8917 5.08789C65.5549 5.2959 65.245 5.54526 64.968 5.83097V4.6515H62.3359V15.7398H65.0982V9.90292C65.0982 9.28465 65.2186 8.74461 65.4561 8.29773C65.6925 7.85272 66.0162 7.51119 66.4182 7.28255C66.8213 7.05311 67.2657 6.93674 67.7391 6.93674C68.1975 6.93674 68.6088 7.0419 68.9614 7.24948C69.3136 7.4566 69.5939 7.75111 69.7946 8.12465C69.9961 8.50006 70.0982 8.93787 70.0982 9.42591V15.7398H72.8388V8.90551C72.8388 8.05629 72.6527 7.28299 72.2858 6.60712Z"

                fill="#1E1E1E"

              />

              <path

                d="M23.7266 0V1.84061H24.1911C24.6972 1.84061 25.1075 2.25088 25.1075 2.75697V15.7398H27.8702V0H23.7266Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_12_44">

              <rect width="73" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

    {

      name: "github",

      logo: (

        <svg

          width="60"

          height="16"

          viewBox="0 0 60 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_12_56)">

            <mask

              id="mask0_12_56"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="60"

              height="16"

            >

              <path d="M59.1716 0H0V16H59.1716V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_12_56)">

              <path

                d="M11.3991 6.84818H6.42744C6.36582 6.84822 6.30674 6.87271 6.26318 6.91627C6.21961 6.95984 6.19512 7.01892 6.19509 7.08053V9.5113C6.19512 9.57295 6.2196 9.63206 6.26316 9.67569C6.30671 9.71931 6.36579 9.74389 6.42744 9.74402H8.3669V12.7639C8.3669 12.7639 7.93141 12.9125 6.72742 12.9125C5.30697 12.9125 3.32267 12.3933 3.32267 8.03C3.32267 3.6658 5.3889 3.09147 7.32873 3.09147C9.00789 3.09147 9.73129 3.38726 10.1915 3.52955C10.3362 3.5739 10.47 3.43001 10.47 3.30151L11.0246 0.95304C11.0246 0.893044 11.0043 0.820605 10.9358 0.771451C10.7489 0.638153 9.60846 0 6.72742 0C3.40841 0 0.00390625 1.41219 0.00390625 8.20013C0.00390625 14.9886 3.90181 16 7.18644 16C9.9061 16 11.5559 14.8378 11.5559 14.8378C11.6239 14.8001 11.6313 14.7051 11.6313 14.6615V7.08041C11.6313 6.95229 11.5274 6.84818 11.3991 6.84818ZM37.0212 0.813337C37.0214 0.782828 37.0157 0.752569 37.0042 0.724297C36.9927 0.696026 36.9758 0.6703 36.9543 0.648595C36.9329 0.626891 36.9074 0.609637 36.8792 0.597824C36.8511 0.58601 36.8209 0.579871 36.7904 0.579759H33.9909C33.9604 0.579872 33.9301 0.586003 33.9019 0.597803C33.8737 0.609603 33.8481 0.626838 33.8266 0.648528C33.8051 0.670218 33.788 0.695937 33.7765 0.724215C33.7649 0.752492 33.759 0.782776 33.7591 0.813337L33.7598 6.22321H29.3964V0.813337C29.3966 0.782817 29.3908 0.752551 29.3793 0.724279C29.3678 0.696006 29.3508 0.670282 29.3294 0.64858C29.3079 0.626879 29.2824 0.609628 29.2542 0.597818C29.2261 0.586008 29.1959 0.579871 29.1654 0.579759H26.3661C26.3045 0.580084 26.2455 0.604871 26.2021 0.648672C26.1588 0.692474 26.1346 0.751703 26.1349 0.813337V15.4618C26.1349 15.5909 26.2387 15.696 26.3661 15.696H29.1654C29.2934 15.696 29.3964 15.5907 29.3964 15.4618V9.19604H33.7598L33.7522 15.4618C33.7522 15.5909 33.8561 15.696 33.9841 15.696H36.7902C36.9184 15.696 37.0209 15.5907 37.0212 15.4618V0.813337ZM16.6816 2.73568C16.6816 1.72757 15.8734 0.913002 14.8764 0.913002C13.8804 0.913002 13.0716 1.72757 13.0716 2.73568C13.0716 3.74255 13.8804 4.55922 14.8764 4.55922C15.8734 4.55922 16.6816 3.74255 16.6816 2.73568ZM16.4814 12.3718V5.61007C16.4816 5.54843 16.4573 5.48923 16.4139 5.44546C16.3704 5.4017 16.3114 5.37694 16.2498 5.37662H13.4593C13.3313 5.37662 13.2167 5.50868 13.2167 5.63718V15.3245C13.2167 15.6093 13.3941 15.6939 13.6238 15.6939H16.1379C16.4138 15.6939 16.4814 15.5583 16.4814 15.3201V12.3718ZM47.6598 5.39867H44.8819C44.7545 5.39867 44.6508 5.50363 44.6508 5.63286V12.8154C44.6508 12.8154 43.945 13.3318 42.9434 13.3318C41.9418 13.3318 41.6761 12.8774 41.6761 11.8966V5.63286C41.6761 5.50363 41.5725 5.39867 41.4451 5.39867H38.6257C38.4985 5.39867 38.3942 5.50363 38.3942 5.63286V12.3708C38.3942 15.2839 40.0178 15.9966 42.2514 15.9966C44.0837 15.9966 45.5609 14.9844 45.5609 14.9844C45.5609 14.9844 45.6313 15.5177 45.6631 15.581C45.695 15.6441 45.7779 15.7078 45.8675 15.7078L47.6612 15.6999C47.7883 15.6999 47.8927 15.5947 47.8927 15.4661L47.8917 5.63299C47.8917 5.50363 47.7878 5.39867 47.6598 5.39867ZM54.1565 13.323C53.193 13.2936 52.5395 12.8563 52.5395 12.8563V8.21763C52.5395 8.21763 53.1841 7.82241 53.9752 7.7517C54.9755 7.66214 55.9394 7.96434 55.9394 10.3505C55.9395 12.8669 55.5045 13.3635 54.1565 13.323ZM55.2522 5.06974C53.6745 5.06974 52.6013 5.77367 52.6013 5.77367V0.813459C52.6013 0.684104 52.498 0.579759 52.3703 0.579759H49.5631C49.5325 0.579904 49.5023 0.586065 49.4742 0.597889C49.446 0.609714 49.4205 0.626971 49.399 0.648674C49.3775 0.670377 49.3605 0.6961 49.3489 0.724374C49.3374 0.752649 49.3315 0.78292 49.3317 0.813459V15.4618C49.3317 15.5909 49.4354 15.696 49.5634 15.696H51.5113C51.5989 15.696 51.6653 15.6507 51.7143 15.5716C51.7627 15.4929 51.8326 14.8963 51.8326 14.8963C51.8326 14.8963 52.9805 15.9841 55.1535 15.9841C57.7047 15.9841 59.1677 14.6901 59.1677 10.175C59.1677 5.65972 56.8311 5.06974 55.2522 5.06974ZM24.5269 5.37539H22.4271L22.4239 2.60115C22.4239 2.49618 22.3698 2.4437 22.2484 2.4437H19.3868C19.2756 2.4437 19.2159 2.49274 19.2159 2.59955V5.46643C19.2159 5.46643 17.7819 5.81248 17.6849 5.84057C17.6365 5.85459 17.594 5.88393 17.5638 5.92419C17.5336 5.96445 17.5172 6.01345 17.5172 6.0638V7.86529C17.5172 7.99489 17.6207 8.09911 17.7487 8.09911H19.2159V12.4329C19.2159 15.652 21.4738 15.9682 22.9975 15.9682C23.6936 15.9682 24.5264 15.7447 24.6639 15.6937C24.7471 15.6632 24.7954 15.5771 24.7954 15.4837L24.7977 13.502C24.7977 13.3726 24.6886 13.2683 24.5656 13.2683C24.4433 13.2683 24.1302 13.318 23.808 13.318C22.7766 13.318 22.4271 12.8384 22.4271 12.2177L22.4269 8.09899H24.5269C24.6549 8.09899 24.7585 7.99464 24.7585 7.86516V5.60859C24.7587 5.57806 24.7528 5.5478 24.7412 5.51955C24.7297 5.49129 24.7126 5.46559 24.6911 5.44393C24.6696 5.42227 24.644 5.40506 24.6159 5.3933C24.5877 5.38154 24.5575 5.37545 24.5269 5.37539Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_12_56">

              <rect width="60" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

    {

      name: "nike",

      logo: (

        <svg

          width="45"

          height="16"

          viewBox="0 0 45 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_12_63)">

            <mask

              id="mask0_12_63"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="45"

              height="16"

            >

              <path d="M44.4444 0H0V16H44.4444V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_12_63)">

              <path

                d="M5.69321 0.711516C2.80128 4.10777 0.0280227 8.31985 0.000226217 11.468C-0.0106178 12.6526 0.367757 13.6867 1.27509 14.4704C2.58028 15.5976 4.01767 15.996 5.44901 15.9978C7.54095 16.0006 9.61786 15.1566 11.2445 14.5065C13.9834 13.4109 44.2572 0.264234 44.2572 0.264234C44.5493 0.118114 44.4942 -0.0645871 44.1289 0.026695C43.9817 0.0638256 11.1708 8.95533 11.1708 8.95533C10.5375 9.13336 9.89081 9.22546 9.26104 9.22875C6.7398 9.24352 4.49604 7.84415 4.51416 4.89444C4.52109 3.74016 4.87524 2.34883 5.69321 0.711516Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_12_63">

              <rect width="45" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

    {

      name: "lemonSquezy",

      logo: (

        <svg

          width="122"

          height="16"

          viewBox="0 0 122 16"

          fill="none"

          xmlns="http://www.w3.org/2000/svg"

        >

          <g clipPath="url(#clip0_12_70)">

            <mask

              id="mask0_12_70"

              style={{ maskType: "luminance" }}

              maskUnits="userSpaceOnUse"

              x="0"

              y="0"

              width="122"

              height="16"

            >

              <path d="M121.143 0H0V16H121.143V0Z" fill="white" />

            </mask>

            <g mask="url(#mask0_12_70)">

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M24.3337 7.91658H27.9357C27.6958 7.00149 27.0387 6.63046 26.2383 6.63046C25.3583 6.63046 24.6531 7.11012 24.3337 7.91658ZM30.049 9.48155H24.2059C24.4458 10.6602 25.4379 11.0945 26.3022 11.0945C27.3908 11.0945 27.8562 10.4432 27.8562 10.4432H29.8729C29.2642 11.993 27.8079 12.845 26.2384 12.845C24.0769 12.845 22.188 11.2485 22.188 8.83149C22.188 6.42858 24.0612 4.94058 26.1744 4.94058C28.2238 4.94058 30.3058 6.31989 30.049 9.48155Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M49.8991 8.89326C49.8991 7.68418 49.1625 6.73898 47.9461 6.73898C46.7287 6.73898 45.9125 7.68418 45.9125 8.89326C45.9125 10.1023 46.7287 11.0476 47.9461 11.0476C49.1625 11.0476 49.8991 10.1023 49.8991 8.89326ZM43.8149 8.90835C43.8149 6.42858 45.8004 4.94058 47.9461 4.94058C50.1076 4.94058 51.9965 6.44366 51.9965 8.87686C51.9965 11.3417 50.0424 12.8448 47.8811 12.8448C45.7039 12.8448 43.8149 11.3417 43.8149 8.90835Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M60.5665 8.56783V12.7679H58.4362V9.03223C58.4362 8.76852 58.6293 6.81595 57.092 6.73915C56.3386 6.69241 54.9944 7.09486 54.9944 9.12566V12.7679H52.8813V5.01758H54.8211L54.8275 6.09349C54.8275 6.09349 55.7021 4.94058 57.3333 4.94058C59.3985 4.94058 60.5665 6.42858 60.5665 8.56783Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M68.3526 6.58378C67.6801 6.58378 67.3921 6.90938 67.3921 7.23503C67.3921 7.76138 68.1126 7.91658 68.5926 8.01001C70.0189 8.30395 71.4595 8.72303 71.4595 10.3649C71.4595 11.9615 70.0983 12.845 68.4492 12.845C66.6086 12.845 65.1835 11.7608 65.0869 10.1176H67.0555C67.1041 10.582 67.4246 11.1865 68.4012 11.1865C69.2172 11.1865 69.4103 10.7689 69.4103 10.4432C69.4103 9.86898 68.8492 9.69852 68.3046 9.57481C67.3606 9.37292 65.3269 9.00195 65.3269 7.23503C65.3269 5.71555 66.8326 4.94058 68.3852 4.94058C70.1778 4.94058 71.3629 5.99446 71.4595 7.29681H69.4892C69.4258 7.03309 69.1703 6.58378 68.3526 6.58378Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M77.9322 8.89326C77.9322 7.60601 77.2111 6.84743 76.1711 6.84743C75.1945 6.84743 74.2168 7.52898 74.2168 8.89326C74.2168 10.2564 75.1945 10.9391 76.1711 10.9391C77.2111 10.9391 77.9322 10.1794 77.9322 8.89326ZM79.9334 5.01758V15.8675H77.9322V12.0081C77.4202 12.5659 76.6991 12.8448 75.8659 12.8448C73.8339 12.8448 72.1362 11.2333 72.1362 8.89326C72.1362 6.55212 73.8339 4.94058 75.8659 4.94058C77.4637 4.94058 78.0619 5.97858 78.0619 5.97858L78.0585 5.01758H79.9334Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M91.3737 7.91658H94.976C94.736 7.00149 94.0789 6.63046 93.2789 6.63046C92.3989 6.63046 91.6932 7.11012 91.3737 7.91658ZM97.0892 9.48155H91.2452C91.4863 10.6602 92.4783 11.0945 93.3423 11.0945C94.4309 11.0945 94.8966 10.4432 94.8966 10.4432H96.9132C96.3046 11.993 94.848 12.845 93.2789 12.845C91.1172 12.845 89.228 11.2485 89.228 8.83149C89.228 6.42858 91.1017 4.94058 93.2149 4.94058C95.264 4.94058 97.3463 6.31989 97.0892 9.48155Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M99.8801 7.91658H103.482C103.242 7.00149 102.585 6.63046 101.785 6.63046C100.905 6.63046 100.2 7.11012 99.8801 7.91658ZM105.596 9.48155H99.7527C99.9921 10.6602 100.984 11.0945 101.849 11.0945C102.937 11.0945 103.402 10.4432 103.402 10.4432H105.42C104.81 11.993 103.354 12.845 101.785 12.845C99.6235 12.845 97.7344 11.2485 97.7344 8.83149C97.7344 6.42858 99.6075 4.94058 101.721 4.94058C103.77 4.94058 105.852 6.31989 105.596 9.48155Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M112.724 11.0325V12.7678H105.92V11.7444L109.779 6.75417H106.08V5.01758H112.564V6.04097L108.706 11.0325H112.724Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M121.053 5.01758V11.8378V11.993C121.053 14.0855 119.948 15.9459 117.211 15.9459C114.649 15.9459 113.256 14.3177 113.256 13.0936H115.257C115.257 13.0936 115.561 14.1322 117.147 14.1322C118.492 14.1322 119.052 13.3875 119.052 12.3034V11.9778C118.699 12.3653 118.028 12.845 116.827 12.845C114.729 12.845 113.417 11.3734 113.417 9.21892L113.4 5.01758H115.481V8.75332C115.481 9.80703 115.867 11.0478 117.195 11.0478C117.883 11.0478 119.02 10.7221 119.02 8.65989V5.01758H121.053Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M20.6641 10.4413C20.6641 10.9246 20.8545 11.124 21.2078 11.124C21.4567 11.124 21.6184 11.0951 21.8543 11.0243L21.9861 12.6171C21.5453 12.7584 21.1192 12.8442 20.5755 12.8442C19.3279 12.8442 18.564 12.4467 18.564 10.7253V1.91797H20.6641V10.4413Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M42.9113 8.56783V12.7679H40.7811V9.03223C40.7811 7.96326 40.8934 6.61423 39.4201 6.73915C39.0369 6.76943 37.9796 6.93966 37.9796 9.12566V12.7679H35.8664V9.03223C35.8664 7.96326 35.9785 6.61423 34.5054 6.73915C34.1208 6.76943 33.0649 6.93966 33.0649 9.12566V12.7679H30.9517V5.01758H32.8917L32.8934 6.09349C32.8934 6.09349 33.6348 4.94057 35.0177 4.94057C36.684 4.94057 37.3381 6.19566 37.3381 6.19566C37.3381 6.19566 38.0555 4.92551 39.8855 4.92551C41.9662 4.92551 42.9113 6.41349 42.9113 8.56783Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M80.8315 9.21772V5.01758H82.9618V8.75332C82.9618 9.01697 82.7687 10.9695 84.3058 11.0464C85.0595 11.0931 86.4035 10.6906 86.4035 8.65989V5.01758H88.5167V12.7679H86.5795L86.5704 11.6921C86.5704 11.6921 85.6955 12.845 84.0647 12.845C81.9995 12.845 80.8315 11.357 80.8315 9.21772Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M3.95953 9.82034L8.25169 11.8047C8.78369 12.0508 9.15918 12.4638 9.36198 12.9375C9.87489 14.1371 9.17387 15.3639 8.07341 15.8052C6.97272 16.2462 5.79969 15.9624 5.26631 14.7149L3.39836 10.3352C3.25361 9.99571 3.61724 9.66211 3.95953 9.82034Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M4.2166 8.53576L8.64726 6.8609C10.1198 6.30427 11.7283 7.35747 11.7066 8.88776C11.7062 8.90776 11.7059 8.9277 11.7054 8.94787C11.6735 10.4381 10.1098 11.4397 8.6696 10.9125L4.2208 9.28416C3.86592 9.15433 3.8633 8.6693 4.2166 8.53576Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M3.96857 7.95566L8.32406 6.10497C9.77137 5.48992 10.1387 3.64397 9.00514 2.57739C8.99029 2.56334 8.97543 2.54946 8.9604 2.53559C7.84903 1.50404 6.01189 1.86724 5.37919 3.22594L3.4247 7.42371C3.26876 7.75846 3.6212 8.1032 3.96857 7.95566Z"

                fill="#1E1E1E"

              />

              <path

                fillRule="evenodd"

                clipRule="evenodd"

                d="M2.84771 7.22434L4.43123 2.88237C4.62755 2.344 4.59119 1.79497 4.38822 1.32126C3.87425 0.12216 2.48234 -0.264902 1.38202 0.176995C0.281877 0.619063 -0.339783 1.62302 0.194641 2.87002L2.07483 7.24497C2.22063 7.584 2.72149 7.57063 2.84771 7.22434Z"

                fill="#1E1E1E"

              />

            </g>

          </g>

          <defs>

            <clipPath id="clip0_12_70">

              <rect width="122" height="16" fill="white" />

            </clipPath>

          </defs>

        </svg>

      ),

    },

  ];



  return (

    <AnimatedGroup

      className={cn(

        "flex items-center justify-center  md:gap-10 lg:gap-20 overflow-x-auto md:px-4",

        className,

      )}

      variants={{

        container: {

          hidden: { opacity: 0 },

          visible: {

            opacity: 1,

            transition: {

              staggerChildren: 0.05,

              delayChildren: 1.5,

            },

          },

        },

        item: {

          hidden: {

            opacity: 0,

            filter: "blur(12px)",

            y: -60,

            rotateX: 90,

          },

          visible: {

            opacity: 1,

            filter: "blur(0px)",

            y: 0,

            rotateX: 0,

            transition: {

              type: "spring",

              bounce: 0.3,

              duration: 1,

            },

          },

        },

      }}

      {...props}

    >

      {logos.map((item) => (

        <div

          key={item.name}

          className="shrink-0 scale-60 md:scale-100 dark:invert"

        >

          {item.logo}

        </div>

      ))}

    </AnimatedGroup>

  );

}
```
##### File: `src/components/ui/animated-group.tsx`
```tsx
// Component From Motion Primitives



"use client";

import { ReactNode } from "react";

import { motion, Variants, HTMLMotionProps } from "motion/react";

import React from "react";



export type PresetType =

  | "fade"

  | "slide"

  | "scale"

  | "blur"

  | "blur-slide"

  | "zoom"

  | "flip"

  | "bounce"

  | "rotate"

  | "swing";



export type AnimatedGroupProps = {

  children: ReactNode;

  className?: string;

  variants?: {

    container?: Variants;

    item?: Variants;

  };

  preset?: PresetType;

  as?: React.ElementType;

  asChild?: React.ElementType;

};



const defaultContainerVariants: Variants = {

  visible: {

    transition: {

      staggerChildren: 0.1,

    },

  },

};



const defaultItemVariants: Variants = {

  hidden: { opacity: 0 },

  visible: { opacity: 1 },

};



const presetVariants: Record<PresetType, Variants> = {

  fade: {},

  slide: {

    hidden: { y: 20 },

    visible: { y: 0 },

  },

  scale: {

    hidden: { scale: 0.8 },

    visible: { scale: 1 },

  },

  blur: {

    hidden: { filter: "blur(4px)" },

    visible: { filter: "blur(0px)" },

  },

  "blur-slide": {

    hidden: { filter: "blur(4px)", y: 20 },

    visible: { filter: "blur(0px)", y: 0 },

  },

  zoom: {

    hidden: { scale: 0.5 },

    visible: {

      scale: 1,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  flip: {

    hidden: { rotateX: -90 },

    visible: {

      rotateX: 0,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  bounce: {

    hidden: { y: -50 },

    visible: {

      y: 0,

      transition: { type: "spring", stiffness: 400, damping: 10 },

    },

  },

  rotate: {

    hidden: { rotate: -180 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 200, damping: 15 },

    },

  },

  swing: {

    hidden: { rotate: -10 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 300, damping: 8 },

    },

  },

};



const mergeVariants = (

  defaultVariants: Variants,

  customVariants: Partial<Variants> = {},

): Variants => {

  return {

    hidden: {

      ...defaultVariants.hidden,

      ...customVariants.hidden,

    },

    visible: {

      ...defaultVariants.visible,

      ...customVariants.visible,

    },

  };

};



function AnimatedGroup({

  children,

  className,

  variants,

  preset = "fade",

  as = "div",

  asChild = "div",

}: AnimatedGroupProps) {

  const MotionContainer = motion.div;

  const MotionChild = motion.div;



  const presetVariant = preset ? presetVariants[preset] : {};

  const itemVariants = variants?.item

    ? mergeVariants(defaultItemVariants, variants.item)

    : mergeVariants(defaultItemVariants, presetVariant);



  const containerVariants = variants?.container || defaultContainerVariants;



  return (

    <MotionContainer

      initial="hidden"

      animate="visible"

      variants={containerVariants}

      className={className}

    >

      {React.Children.map(children, (child, index) => (

        <MotionChild key={index} variants={itemVariants}>

          {child}

        </MotionChild>

      ))}

    </MotionContainer>

  );

}



export { AnimatedGroup };
```
---

## Hero Section 6 <a name="hero-section-6"></a>

Solace UI hero section block variation 6.

- **Registry Key**: `hero-section-6`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/hero-section-6.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/hero-section/six/index.tsx`
```tsx
"use client";

import React from "react";

import Image from "next/image";

import { cn } from "@/lib/utils";

import Link from "next/link";

import { motion, Variants } from "motion/react";

import { AnimatedGroup } from "@/components/ui/animated-group";



interface Iphone15ProProps extends React.SVGProps<SVGSVGElement> {

  width?: string | number;

  height?: string | number;

  src?: string;

  alt?: string;

}



const Iphone15Pro: React.FC<Iphone15ProProps> = ({

  width = "100%",

  height = "auto",

  src,

  alt = "iPhone screen content",

  className,

  ...props

}) => {

  return (

    <div className={cn("relative", className)}>

      <svg

        width={width}

        height={height}

        viewBox="0 0 433 882"

        preserveAspectRatio="xMidYMid meet"

        fill="none"

        xmlns="http://www.w3.org/2000/svg"

        className="transition-all duration-500 ease-in-out"

        {...props}

      >

        {/* Outer frame */}

        <path

          d="M2 73C2 32.6832 34.6832 0 75 0H357C397.317 0 430 32.6832 430 73V809C430 849.317 397.317 882 357 882H75C34.6832 882 2 849.317 2 809V73Z"

          className="dark:fill-[#DADADA] fill-[#404040]"

        />

        {/* Side buttons */}

        <path

          d="M0 171C0 170.448 0.447715 170 1 170H3V204H1C0.447715 204 0 203.552 0 203V171Z"

          className="dark:fill-[#DADADA] fill-[#404040]"

        />

        <path

          d="M1 234C1 233.448 1.44772 233 2 233H3.5V300H2C1.44772 300 1 299.552 1 299V234Z"

          className="dark:fill-[#DADADA] fill-[#404040]"

        />

        <path

          d="M1 319C1 318.448 1.44772 318 2 318H3.5V385H2C1.44772 385 1 384.552 1 384V319Z"

          className="dark:fill-[#DADADA] fill-[#404040]"

        />

        <path

          d="M430 279H432C432.552 279 433 279.448 433 280V384C433 384.552 432.552 385 432 385H430V279Z"

          className="dark:fill-[#DADADA] fill-[#404040]"

        />



        {/* Inner body */}

        <path

          d="M6 74C6 35.3401 37.3401 4 76 4H356C394.66 4 426 35.3401 426 74V808C426 846.66 394.66 878 356 878H76C37.3401 878 6 846.66 6 808V74Z"

          className="fill-[#262626] dark:fill-black" // Simplified dark mode fill

        />



        {/* Top speaker grille */}

        <path

          opacity="0.5"

          d="M174 5H258V5.5C258 6.60457 257.105 7.5 256 7.5H176C174.895 7.5 174 6.60457 174 5.5V5Z"

          className="dark:fill-[#DADADA] fill-[#404040]"

        />



        {/* Screen area */}

        <path

          d="M21.25 75C21.25 44.2101 46.2101 19.25 77 19.25H355C385.79 19.25 410.75 44.2101 410.75 75V807C410.75 837.79 385.79 862.75 355 862.75H77C46.2101 862.75 21.25 837.79 21.25 807V75Z"

          className="fill-[#111] dark:fill-[#F5F5F5]" // Screen background

        />



        {/* Screen Content Area */}

        {src && (

          <foreignObject

            x="21.25"

            y="19.25"

            width="389.5"

            height="843.5"

            clipPath="url(#roundedCorners)"

          >

            <div

              style={{

                width: "100%",

                height: "100%",

                borderRadius: "55.75px",

                overflow: "hidden",

                position: "relative",

                backgroundColor: "#111",

              }}

              className="dark:bg-[#F5F5F5]"

            >

              <Image

                src={src}

                alt={alt}

                fill

                style={{ objectFit: "cover" }}

                sizes="(max-width: 768px) 80vw, (max-width: 1200px) 50vw, 33vw"

                priority

              />

            </div>

          </foreignObject>

        )}



        {/* Notch area */}

        <path

          d="M154 48.5C154 38.2827 162.283 30 172.5 30H259.5C269.717 30 278 38.2827 278 48.5C278 58.7173 269.717 67 259.5 67H172.5C162.283 67 154 58.7173 154 48.5Z"

          className="fill-[#262626] dark:fill-[#F0F0F0]"

        />

        {/* Inner Notch Elements */}

        <path

          d="M249 48.5C249 42.701 253.701 38 259.5 38C265.299 38 270 42.701 270 48.5C270 54.299 265.299 59 259.5 59C253.701 59 249 54.299 249 48.5Z"

          className="fill-[#111] dark:fill-[#D1D1D1]" // Slightly darker for contrast

        />

        <path

          d="M254 48.5C254 45.4624 256.462 43 259.5 43C262.538 43 265 45.4624 265 48.5C265 51.5376 262.538 54 259.5 54C256.462 54 254 51.5376 254 48.5Z"

          className="fill-white/30 dark:fill-black" // Even darker for the lens appearance

        />



        <defs>

          <clipPath id="roundedCorners">

            <rect

              x="21.25"

              y="19.25"

              width="389.5"

              height="843.5"

              rx="55.75"

              ry="55.75"

            />

          </clipPath>

        </defs>

      </svg>

    </div>

  );

};



const navItems = [

  { name: "Product", href: "#" },

  { name: "Customer", href: "#" },

  { name: "Solution", href: "#" },

  { name: "Pricing", href: "#" },

  { name: "Company", href: "#" },

];



export default function HeroSection6() {

  const textVariants: Variants = {

    hidden: { opacity: 0, filter: "blur(10px)", y: 20 },

    visible: {

      opacity: 1,

      filter: "blur(0px)",

      y: 0,

      transition: {

        type: "spring",

        bounce: 0.2,

        duration: 1,

      },

    },

  };



  return (

    <div className="relative w-full h-screen [--color-primary:#003AF9] overflow-hidden">

      {/* Radial Gradient Background */}

      <div className="absolute inset-0 z-0 bg-[radial-gradient(125%_125%_at_50%_10%,#fff_40%,var(--color-primary)_100%)] dark:bg-[radial-gradient(125%_125%_at_50%_10%,#000_40%,var(--color-primary)_100%)]" />



      {/* Navbar */}

      <nav className="w-full flex justify-between items-center py-4 px-4 sm:px-6 border-b border-black/10 dark:border-white/20 relative z-10">

        <div className="font-bold text-md tracking-tight">SolaceUI</div>



        <div className="items-center gap-4 hidden md:flex">

          {navItems.map((item) => (

            <Link href={item.href} key={item.name}>

              <span className="text-sm md:text-[1rem] text-neutral-400 hover:text-black transition-colors  dark:text-white/70">

                {item.name}

              </span>

            </Link>

          ))}

        </div>



        <div className="flex items-center gap-4">

          <Link href="#">

            <button className="px-3 py-1 text-sm font-medium border border-neutral-200 text-black dark:text-white hover:bg-neutral-200 transition-colors rounded-sm">

              Log in

            </button>

          </Link>

          <Link href="#">

            <button className="px-3 py-1 text-sm font-medium bg-(--color-primary) text-white hover:bg-black/90 transition-colors rounded-sm">

              Sign Up

            </button>

          </Link>

        </div>

      </nav>



      {/* Hero Content */}

      <div className="flex flex-col items-center justify-start text-center pt-20 md:pt-10 px-4 pb-0 max-w-7xl mx-auto z-10 relative rounded-none">

        <AnimatedGroup

          className="max-w-4xl mx-auto text-center flex flex-col items-center"

          variants={{

            container: {

              visible: {

                transition: {

                  staggerChildren: 0.1,

                },

              },

            },

            item: textVariants,

          }}

        >

          <h1 className="text-3xl md:text-4xl lg:text-6xl font-bold tracking-tight text-gray-900 dark:text-white mb-5 leading-[1.1] px-6 md:px-0">

            AI Agents That Code

            <br />

            Like Your Best Engineer

          </h1>

          <p className="text-sm sm:text-sm md:text-lg lg:text-xl text-neutral-600 dark:text-neutral-300 max-w-[350px]  md:max-w-lg  mx-auto mb-6 md:mb-5">

            Autonomous agents that debug, refactor, and ship features while you

            focus on architecture and strategy

          </p>

          <div className="flex flex-row sm:flex-row items-center justify-center gap-4 w-[300px] md:w-full mb-16 mx-auto">

            <button className="px-1 py-1 md:px-4 md:py-2 text-lg rounded-md bg-(--color-primary) hover:bg-(--color-primary)/90 text-white w-full sm:w-auto shadow-lg shadow-(--color-primary)/20 transition-all hover:shadow-(--color-primary)/40 rounded-sm cursor-pointer">

              Book a demo

            </button>



            <button className="px-1 py-1 md:px-4 md:py-2 text-lg rounded-md w-full sm:w-auto border border-neutral-300 dark:border-neutral-700 hover:bg-neutral-300 dark:hover:bg-neutral-800 transition-colors rounded-sm cursor-pointer">

              Try for free

            </button>

          </div>

        </AnimatedGroup>



        {/* Hero Images Section */}

        <div className="relative w-full mx-auto z-20">

          <div className="relative">

            {/* Desktop Screenshot */}

            <motion.div

              initial={{ opacity: 0, y: 30, scale: 0.98 }}

              animate={{ opacity: 1, y: 0, scale: 1 }}

              transition={{ duration: 0.8, delay: 0.4, ease: "easeOut" }}

              className="relative w-full rounded-md overflow-hidden border border-gray-200 dark:border-gray-800 shadow-xl dark:bg-gray-900"

            >

              <Image

                src="https://res.cloudinary.com/harshitproject/image/upload/v1774017120/hero-light.png"

                alt="Desktop Dashboard"

                width={800}

                height={800}

                className="object-cover object-left w-full h-auto"

                priority

              />

            </motion.div>



            {/* iPhone Frame */}

            <div className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-[55%] md:-translate-y-[45%] lg:-translate-y-[55%] w-[150px] sm:w-[220px] md:w-[260px] lg:w-[320px] xl:w-[380px]">

              <motion.div

                initial={{ opacity: 0, scale: 0.8, y: 20 }}

                animate={{ opacity: 1, scale: 1, y: 0 }}

                transition={{ duration: 0.8, delay: 0.7, ease: "easeOut" }}

              >

                <Iphone15Pro

                  src="https://res.cloudinary.com/harshitproject/image/upload/v1746774805/Notion-screen.png"

                  className="w-full h-[240px] md:h-[420px] lg:h-[480px] xl:h-[540px]"

                />

              </motion.div>

            </div>

          </div>



          {/* Fade Overlay for both images */}

          {/* Fade Overlay for both images */}

          <motion.div

            initial={{ opacity: 0 }}

            animate={{ opacity: 1 }}

            transition={{ duration: 0.8, delay: 0.4, ease: "easeOut" }}

            className="absolute -bottom-2 left-0 right-0 h-50 md:h-60 lg:h-80 bg-gradient-to-t from-white via-white/80 to-transparent  z-30 pointer-events-none rounded-md"

          />

        </div>

      </div>

    </div>

  );

}
```
##### File: `src/components/ui/animated-group.tsx`
```tsx
// Component From Motion Primitives



"use client";

import { ReactNode } from "react";

import { motion, Variants, HTMLMotionProps } from "motion/react";

import React from "react";



export type PresetType =

  | "fade"

  | "slide"

  | "scale"

  | "blur"

  | "blur-slide"

  | "zoom"

  | "flip"

  | "bounce"

  | "rotate"

  | "swing";



export type AnimatedGroupProps = {

  children: ReactNode;

  className?: string;

  variants?: {

    container?: Variants;

    item?: Variants;

  };

  preset?: PresetType;

  as?: React.ElementType;

  asChild?: React.ElementType;

};



const defaultContainerVariants: Variants = {

  visible: {

    transition: {

      staggerChildren: 0.1,

    },

  },

};



const defaultItemVariants: Variants = {

  hidden: { opacity: 0 },

  visible: { opacity: 1 },

};



const presetVariants: Record<PresetType, Variants> = {

  fade: {},

  slide: {

    hidden: { y: 20 },

    visible: { y: 0 },

  },

  scale: {

    hidden: { scale: 0.8 },

    visible: { scale: 1 },

  },

  blur: {

    hidden: { filter: "blur(4px)" },

    visible: { filter: "blur(0px)" },

  },

  "blur-slide": {

    hidden: { filter: "blur(4px)", y: 20 },

    visible: { filter: "blur(0px)", y: 0 },

  },

  zoom: {

    hidden: { scale: 0.5 },

    visible: {

      scale: 1,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  flip: {

    hidden: { rotateX: -90 },

    visible: {

      rotateX: 0,

      transition: { type: "spring", stiffness: 300, damping: 20 },

    },

  },

  bounce: {

    hidden: { y: -50 },

    visible: {

      y: 0,

      transition: { type: "spring", stiffness: 400, damping: 10 },

    },

  },

  rotate: {

    hidden: { rotate: -180 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 200, damping: 15 },

    },

  },

  swing: {

    hidden: { rotate: -10 },

    visible: {

      rotate: 0,

      transition: { type: "spring", stiffness: 300, damping: 8 },

    },

  },

};



const mergeVariants = (

  defaultVariants: Variants,

  customVariants: Partial<Variants> = {},

): Variants => {

  return {

    hidden: {

      ...defaultVariants.hidden,

      ...customVariants.hidden,

    },

    visible: {

      ...defaultVariants.visible,

      ...customVariants.visible,

    },

  };

};



function AnimatedGroup({

  children,

  className,

  variants,

  preset = "fade",

  as = "div",

  asChild = "div",

}: AnimatedGroupProps) {

  const MotionContainer = motion.div;

  const MotionChild = motion.div;



  const presetVariant = preset ? presetVariants[preset] : {};

  const itemVariants = variants?.item

    ? mergeVariants(defaultItemVariants, variants.item)

    : mergeVariants(defaultItemVariants, presetVariant);



  const containerVariants = variants?.container || defaultContainerVariants;



  return (

    <MotionContainer

      initial="hidden"

      animate="visible"

      variants={containerVariants}

      className={className}

    >

      {React.Children.map(children, (child, index) => (

        <MotionChild key={index} variants={itemVariants}>

          {child}

        </MotionChild>

      ))}

    </MotionContainer>

  );

}



export { AnimatedGroup };
```
---

## Hero Section 7 <a name="hero-section-7"></a>

Solace UI hero section block variation 7.

- **Registry Key**: `hero-section-7`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/hero-section-7.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `@paper-design/shaders-react`

#### Component Source Code
##### File: `src/registry/blocks/hero-section/seven/index.tsx`
```tsx
"use client";
import React from "react";
import Image from "next/image";
import { FlutedGlass } from "@paper-design/shaders-react";
import { motion } from "motion/react";
import Link from "next/link";

const navItems = [
  { name: "Product", href: "#" },
  { name: "Customer", href: "#" },
  { name: "Solution", href: "#" },
  { name: "Pricing", href: "#" },
  { name: "Company", href: "#" },
];

export default function HeroSection7() {
  return (
    <div className="relative w-full min-h-screen overflow-hidden antialiased [font-synthesis:none] [--color-primary:#0097AE] bg-linear-to-b from-(--color-primary) to-white">
      {/* Background Shader */}
      <div className="absolute inset-0 z-0 pointer-events-none">
        <FlutedGlass
          size={0.89}
          shape="lines"
          angle={0}
          distortionShape="prism"
          distortion={0.5}
          shift={0}
          blur={0}
          edges={0.25}
          stretch={0}
          scale={1.11}
          fit="cover"
          highlights={0.1}
          shadows={0.2}
          grainMixer={0.1}
          grainOverlay={0.1}
          colorBack="#00000000"
          colorHighlight="#FFFFFF"
          colorShadow="#000000"
          className="w-full h-full bg-transparent"
        />
      </div>

      {/* Navbar */}
      <nav className="max-w-5xl mx-auto w-full flex justify-between items-center py-5 px-4 sm:px-6 relative z-10">
        <div className="font-bold text-md tracking-tight text-white">SolaceUI</div>

        <div className="items-center gap-4 hidden md:flex">
          {navItems.map((item) => (
            <Link href={item.href} key={item.name}>
              <span className="text-sm md:text-[1rem] text-white/70 hover:text-white transition-colors">
                {item.name}
              </span>
            </Link>
          ))}
        </div>

        <div className="flex items-center gap-4">
          <Link href="#">
            <button className="px-3 py-1 text-sm font-medium border border-white/20 text-white hover:bg-white/10 transition-colors rounded-sm cursor-pointer">
              Log in
            </button>
          </Link>
          <Link href="#">
            <button className="px-3 py-1 text-sm font-medium bg-black text-white hover:bg-black/80 transition-colors rounded-sm cursor-pointer">
              Sign Up
            </button>
          </Link>
        </div>
      </nav>

      {/* Hero Content */}
      <div className="relative z-10 flex flex-col items-center justify-start text-center pt-20 md:pt-28 px-4 pb-0 max-w-5xl mx-auto">
        <motion.div
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.8, ease: "easeOut" }}
          className="max-w-3xl mx-auto flex flex-col items-center"
        >
          <h1 className="text-3xl md:text-5xl lg:text-6xl font-semibold text-white  leading-[1.1] tracking-tight mb-5">
            AI Agents That Code
            <br className="hidden md:block" /> Like Your Best Engineer
          </h1>

          <p className="text-base md:text-lg lg:text-xl text-[#FFFFFF80] font-light  max-w-lg mx-auto mb-8 leading-relaxed">
            Autonomous agents that debug, refactor, and ship features while you
            focus on architecture and strategy
          </p>

          <div className="flex flex-col sm:flex-row items-center justify-center gap-4 w-full md:w-auto">
            <button
              className="w-full sm:w-[180px] h-[48px] rounded-xl bg-black text-white   font-light text-lg md:text-xl transition-transform hover:scale-105 active:scale-95 cursor-pointer shadow-[inset_3px_3px_3px_rgba(242,242,242,0.3),inset_-3px_-3px_3px_rgba(242,242,242,0.3)]"
            >
              Book a demo
            </button>

            <button className="w-full sm:w-[180px] h-[48px] rounded-xl border-[1.5px] border-black text-black   font-light text-lg md:text-xl transition-all hover:scale-105 active:scale-95 hover:bg-black/5 cursor-pointer">
              Try for free
            </button>
          </div>
        </motion.div>

        {/* Hero Image Section */}
        <motion.div
          initial={{ opacity: 0, y: 40 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 1, delay: 0.3, ease: "easeOut" }}
          className="relative w-full max-w-5xl mx-auto mt-16 z-20 flex justify-center px-4 md:px-0"
        >
          <div className="w-full p-2 md:p-4 bg-[#FFFFFF40] rounded-[14px] shadow-2xl">
            <div className="relative w-full rounded-[10px] overflow-hidden border border-white/20">
              <Image
                src="https://res.cloudinary.com/harshitproject/image/upload/v1774017120/hero-light.png"
                alt="Dashboard App"
                width={1200}
                height={719}
                className="w-full h-auto object-cover object-top"
                priority
              />
            </div>
          </div>
        </motion.div>
      </div>

      {/* Bottom Fade Gradient to mask image overflow as per original design */}
      <div className="absolute bottom-0 left-0 w-full h-32 md:h-48 z-30 pointer-events-none bg-linear-to-t from-white to-transparent" />
    </div>
  );
}
```
---

## Hero Section 8 <a name="hero-section-8"></a>

Solace UI hero section block variation 8.

- **Registry Key**: `hero-section-8`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/hero-section-8.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `@paper-design/shaders-react`

#### Component Source Code
##### File: `src/registry/blocks/hero-section/eight/index.tsx`
```tsx
"use client";
import React from "react";
import Image from "next/image";
import { FlutedGlass } from "@paper-design/shaders-react";
import { motion } from "motion/react";
import Link from "next/link";

const navItems = [
  { name: "Product", href: "#" },
  { name: "Customer", href: "#" },
  { name: "Solution", href: "#" },
  { name: "Pricing", href: "#" },
  { name: "Company", href: "#" },
];

export default function HeroSection8() {
  return (
    <div className="relative w-full min-h-screen overflow-hidden antialiased [font-synthesis:none] [--color-primary:#9E73EA] bg-linear-to-b from-(--color-primary) to-white">
      {/* Background Shader */}
      <div className="absolute inset-0 z-0 pointer-events-none">
        <FlutedGlass
          size={0.89}
          shape="lines"
          angle={0}
          distortionShape="prism"
          distortion={0.5}
          shift={0}
          blur={0}
          edges={0.25}
          stretch={0}
          scale={1.11}
          fit="cover"
          highlights={0.1}
          shadows={0.2}
          grainMixer={0.1}
          grainOverlay={0.1}
          colorBack="#00000000"
          colorHighlight="#FFFFFF"
          colorShadow="#000000"
          className="w-full h-full bg-transparent"
        />
      </div>

      {/* Navbar */}
      <nav className="max-w-7xl mx-auto w-full flex justify-between items-center py-5 px-4 sm:px-6 relative z-10">
        <div className="font-bold text-md tracking-tight text-white">SolaceUI</div>

        <div className="items-center gap-4 hidden md:flex">
          {navItems.map((item) => (
            <Link href={item.href} key={item.name}>
              <span className="text-sm md:text-[1rem] text-white/70 hover:text-white transition-colors">
                {item.name}
              </span>
            </Link>
          ))}
        </div>

        <div className="flex items-center gap-4">
          <Link href="#">
            <button className="px-3 py-1 text-sm font-medium border border-white/20 text-white hover:bg-white/10 transition-colors rounded-sm cursor-pointer">
              Log in
            </button>
          </Link>
          <Link href="#">
            <button className="px-3 py-1 text-sm font-medium bg-black text-white hover:bg-black/80 transition-colors rounded-sm cursor-pointer">
              Sign Up
            </button>
          </Link>
        </div>
      </nav>

      {/* Hero Content */}
      <div className="relative z-10 flex flex-col lg:flex-row items-center lg:items-start justify-between pt-12 md:pt-20 lg:pt-32 px-4 sm:px-6 max-w-7xl mx-auto gap-12 lg:gap-2">

        {/* Left Column - Text & Actions */}
        <motion.div
          initial={{ opacity: 0, x: -20 }}
          animate={{ opacity: 1, x: 0 }}
          transition={{ duration: 0.8, ease: "easeOut" }}
          className="w-full lg:w-1/2 flex flex-col items-center lg:items-start text-center lg:text-left shrink-0 z-20"
        >
          <h1 className="text-4xl md:text-5xl lg:text-6xl font-semibold text-white leading-[1.1] tracking-tight mb-5 max-w-[600px]">
            AI Agents That Code Like Your Best Engineer
          </h1>

          <p className="text-base md:text-lg lg:text-xl text-[#FFFFFF80] font-light max-w-lg mb-8 leading-relaxed">
            Autonomous agents that debug, refactor, and ship features while you
            focus on architecture and strategy
          </p>

          <div className="flex flex-col sm:flex-row items-center justify-center lg:justify-start gap-4 w-full md:w-auto mb-10">
            <button className="w-full sm:w-[180px] h-[48px] rounded-xl bg-black text-white font-light text-lg transition-transform hover:scale-105 active:scale-95 cursor-pointer shadow-[inset_3px_3px_3px_rgba(242,242,242,0.3),inset_-3px_-3px_3px_rgba(242,242,242,0.3)]">
              Book a demo
            </button>

            <button className="w-full sm:w-[180px] h-[48px] rounded-xl border-[1.5px] border-black text-black font-light text-lg transition-all hover:scale-105 active:scale-95 hover:bg-black/5 cursor-pointer bg-transparent">
              Try for free
            </button>
          </div>

          {/* Testimonial Section */}
          <div className="flex items-center gap-4">
            <div className="flex -space-x-3">
              {['one', 'two', 'three', 'four', 'five'].map((name, index) => (
                <div key={index} className="w-10 h-10 rounded-full overflow-hidden shrink-0 relative">
                  <Image
                    src={`https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-${name}.png`}
                    alt={`Team member ${index + 1}`}
                    fill
                    className="object-cover object-top"
                  />
                </div>
              ))}
            </div>
            <p className="text-sm md:text-base text-black/30 font-light max-w-[200px] leading-tight text-left">
              Trusted by the best people in 200+ companies
            </p>
          </div>
        </motion.div>

        {/* Right Column - Image Section */}
        <motion.div
          initial={{ opacity: 0, x: 40 }}
          animate={{ opacity: 1, x: 0 }}
          transition={{ duration: 1, delay: 0.3, ease: "easeOut" }}
          className="w-full lg:w-1/2 relative flex justify-start lg:ml-8 xl:ml-16 pointer-events-none"
        >
          {/* Bounding Image Container - Force absolute width on large screens to break out of container correctly */}
          <div className="w-full lg:w-[900px] xl:w-[1200px] p-3 lg:p-5 xl:p-8 bg-[#FFFFFF4A] rounded-[14px] shadow-2xl shrink-0">
            <div className="relative w-full rounded-[10px] overflow-hidden border border-white/20 aspect-[1200/719]">
              <Image
                src="https://res.cloudinary.com/harshitproject/image/upload/v1774017120/hero-light.png"
                alt="Dashboard App"
                fill
                className="object-cover object-top"
                priority
              />
            </div>
          </div>
        </motion.div>
      </div>

      {/* Bottom Fade Gradient */}
      <div className="absolute bottom-0 left-0 w-full h-32 md:h-48 z-30 pointer-events-none bg-linear-to-t from-white to-transparent" />
    </div>
  );
}
```
---

## Hero Video 1 <a name="hero-video-1"></a>

Solace UI hero video component variation 1.

- **Registry Key**: `hero-video-1`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/hero-video-1.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### Component Source Code
##### File: `src/registry/components/hero-video/one/index.tsx`
```tsx
import { HeroVideoDefault } from "@/registry/components/hero-video/hero-video-default";
import React from "react";



export default function HeroVideoOne() {

    return (

        <div className="flex flex-col items-center justify-center py-12">

            <HeroVideoDefault

                className="dark:hidden block"

                videoSrc="https://www.youtube.com/embed/cjZjqP3EqdE?si=wXqy8Rpcnwll8Grr&amp;start=360"

                thumbnailSrc="https://startup-template-sage.vercel.app/hero-light.png"

                thumbnailAlt="Hero Video"

                enableSound={false}

            />

            <HeroVideoDefault

                className="hidden dark:block"

                videoSrc="https://www.youtube.com/embed/cjZjqP3EqdE?si=wXqy8Rpcnwll8Grr&amp;start=360"

                thumbnailSrc="https://startup-template-sage.vercel.app/hero-dark.png"

                thumbnailAlt="Hero Video"

                enableSound={false}

            />

        </div>

    );

}
```
##### File: `src/registry/components/hero-video/hero-video-default.tsx`
```tsx
"use client";

import { useState, useEffect, useCallback } from "react";

import { AnimatePresence, motion } from "motion/react";

import { Play, XIcon } from "lucide-react";

import useSound from "use-sound";

import { cn } from "@/lib/utils";



interface HeroVideoProps {

    videoSrc: string;

    thumbnailSrc: string;

    thumbnailAlt?: string;

    className?: string;

    soundSrc?: string;

    enableSound?: boolean;

}



export function HeroVideoDefault({

    videoSrc,

    thumbnailSrc,

    thumbnailAlt = "Video thumbnail",

    className,

    soundSrc = "/sounds/whoosh-motion.mp3",

    enableSound = true,

}: HeroVideoProps) {

    const [isVideoOpen, setIsVideoOpen] = useState<boolean>(false);

    const [playSound, { sound, stop }] = useSound(soundSrc, {

        soundEnabled: enableSound,

        volume: 1,

    });



    useEffect(() => {

        console.log("Sound enabled:", enableSound);

        console.log("Sound source:", soundSrc);

        return () => {

            if (sound) {

                stop();

            }

        };

    }, [enableSound, soundSrc, sound, stop]);



    const handlePlayClick = useCallback(() => {

        console.log("Play button clicked");

        if (enableSound) {

            try {

                playSound();

                console.log("Attempting to play sound");

            } catch (error) {

                console.error("Error playing sound:", error);

            }

        }

        setIsVideoOpen(true);

    }, [enableSound, playSound]);



    return (

        <div className={cn("relative", className)}>

            <div className="group relative cursor-pointer" onClick={handlePlayClick}>

                <img

                    src={thumbnailSrc}

                    alt={thumbnailAlt}

                    width={1920}

                    height={1080}

                    className="w-[20rem] md:w-[60rem] rounded-md border shadow-lg transition-all duration-200 ease-out group-hover:brightness-[0.8]"

                />

                <div className="absolute inset-0 flex scale-[0.9] items-center justify-center rounded-2xl transition-all duration-200 ease-out group-hover:scale-100">

                    <div className="flex size-28 items-center justify-center rounded-full bg-primary/10 backdrop-blur-md dark:bg-[#212121]">

                        <div

                            className={`relative flex size-20 scale-100 items-center justify-center rounded-full bg-gradient-to-b from-primary/30 to-primary shadow-md transition-all duration-200 ease-out group-hover:scale-[1.2] dark:from-white/10 dark:to-white/30 dark:shadow-gray-900`}

                        >

                            <Play

                                className="size-8 scale-100 fill-white text-white transition-transform duration-200 ease-out group-hover:scale-105 dark:fill-gray-300 dark:text-gray-300"

                                style={{

                                    filter:

                                        "drop-shadow(0 4px 3px rgb(0 0 0 / 0.07)) drop-shadow(0 2px 2px rgb(0 0 0 / 0.06))",

                                }}

                            />

                        </div>

                    </div>

                </div>

            </div>

            <AnimatePresence>

                {isVideoOpen && (

                    <motion.div

                        initial={{ opacity: 0 }}

                        animate={{ opacity: 1 }}

                        onClick={() => setIsVideoOpen(false)}

                        exit={{ opacity: 0 }}

                        className="fixed inset-0 z-50 flex items-center justify-center bg-black/20 backdrop-blur-md"

                    >

                        <motion.div

                            initial={{ scale: 0, rotate: "180deg" }}

                            animate={{

                                scale: 1,

                                rotate: "0deg",

                                transition: {

                                    type: "spring",

                                    bounce: 0.25,

                                },

                            }}

                            exit={{ scale: 0, rotate: "180deg" }}

                            className="relative mx-4 aspect-video w-full max-w-4xl md:mx-0"

                            onClick={(e) => e.stopPropagation()}

                        >

                            <motion.button

                                className="absolute -top-16 right-0 rounded-full bg-neutral-900/50 p-2 text-xl text-white ring-1 backdrop-blur-md dark:bg-neutral-100/50 dark:text-black"

                                onClick={() => setIsVideoOpen(false)}

                            >

                                <XIcon className="size-5" />

                            </motion.button>

                            <div className="relative isolate z-[1] size-full overflow-hidden rounded-2xl border-2 border-white">

                                <iframe

                                    src={videoSrc}

                                    className="size-full rounded-2xl"

                                    allowFullScreen

                                    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"

                                ></iframe>

                            </div>

                        </motion.div>

                    </motion.div>

                )}

            </AnimatePresence>

        </div>

    );

}
```
---

## Hero Video 2 <a name="hero-video-2"></a>

Solace UI hero video component variation 2.

- **Registry Key**: `hero-video-2`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/hero-video-2.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### Component Source Code
##### File: `src/registry/components/hero-video/two/index.tsx`
```tsx
import { HeroVideoDefault } from "@/registry/components/hero-video/hero-video-default";
import React from "react";



export default function HeroVideoSound() {

    return (

        <div className="flex flex-col items-center justify-center py-12">

            <HeroVideoDefault

                className="dark:hidden block"

                videoSrc="https://www.youtube.com/embed/cjZjqP3EqdE?si=wXqy8Rpcnwll8Grr&amp;start=360"

                thumbnailSrc="https://startup-template-sage.vercel.app/hero-light.png"

                thumbnailAlt="Hero Video"

                soundSrc="/sounds/whoosh-motion.mp3"

                enableSound={true}

            />

            <HeroVideoDefault

                className="hidden dark:block"

                videoSrc="https://www.youtube.com/embed/cjZjqP3EqdE?si=wXqy8Rpcnwll8Grr&amp;start=360"

                thumbnailSrc="https://startup-template-sage.vercel.app/hero-dark.png"

                thumbnailAlt="Hero Video"

                soundSrc="/sounds/whoosh-motion.mp3"

                enableSound={true}

            />

        </div>

    );

}
```
##### File: `src/registry/components/hero-video/hero-video-default.tsx`
```tsx
"use client";

import { useState, useEffect, useCallback } from "react";

import { AnimatePresence, motion } from "motion/react";

import { Play, XIcon } from "lucide-react";

import useSound from "use-sound";

import { cn } from "@/lib/utils";



interface HeroVideoProps {

    videoSrc: string;

    thumbnailSrc: string;

    thumbnailAlt?: string;

    className?: string;

    soundSrc?: string;

    enableSound?: boolean;

}



export function HeroVideoDefault({

    videoSrc,

    thumbnailSrc,

    thumbnailAlt = "Video thumbnail",

    className,

    soundSrc = "/sounds/whoosh-motion.mp3",

    enableSound = true,

}: HeroVideoProps) {

    const [isVideoOpen, setIsVideoOpen] = useState<boolean>(false);

    const [playSound, { sound, stop }] = useSound(soundSrc, {

        soundEnabled: enableSound,

        volume: 1,

    });



    useEffect(() => {

        console.log("Sound enabled:", enableSound);

        console.log("Sound source:", soundSrc);

        return () => {

            if (sound) {

                stop();

            }

        };

    }, [enableSound, soundSrc, sound, stop]);



    const handlePlayClick = useCallback(() => {

        console.log("Play button clicked");

        if (enableSound) {

            try {

                playSound();

                console.log("Attempting to play sound");

            } catch (error) {

                console.error("Error playing sound:", error);

            }

        }

        setIsVideoOpen(true);

    }, [enableSound, playSound]);



    return (

        <div className={cn("relative", className)}>

            <div className="group relative cursor-pointer" onClick={handlePlayClick}>

                <img

                    src={thumbnailSrc}

                    alt={thumbnailAlt}

                    width={1920}

                    height={1080}

                    className="w-[20rem] md:w-[60rem] rounded-md border shadow-lg transition-all duration-200 ease-out group-hover:brightness-[0.8]"

                />

                <div className="absolute inset-0 flex scale-[0.9] items-center justify-center rounded-2xl transition-all duration-200 ease-out group-hover:scale-100">

                    <div className="flex size-28 items-center justify-center rounded-full bg-primary/10 backdrop-blur-md dark:bg-[#212121]">

                        <div

                            className={`relative flex size-20 scale-100 items-center justify-center rounded-full bg-gradient-to-b from-primary/30 to-primary shadow-md transition-all duration-200 ease-out group-hover:scale-[1.2] dark:from-white/10 dark:to-white/30 dark:shadow-gray-900`}

                        >

                            <Play

                                className="size-8 scale-100 fill-white text-white transition-transform duration-200 ease-out group-hover:scale-105 dark:fill-gray-300 dark:text-gray-300"

                                style={{

                                    filter:

                                        "drop-shadow(0 4px 3px rgb(0 0 0 / 0.07)) drop-shadow(0 2px 2px rgb(0 0 0 / 0.06))",

                                }}

                            />

                        </div>

                    </div>

                </div>

            </div>

            <AnimatePresence>

                {isVideoOpen && (

                    <motion.div

                        initial={{ opacity: 0 }}

                        animate={{ opacity: 1 }}

                        onClick={() => setIsVideoOpen(false)}

                        exit={{ opacity: 0 }}

                        className="fixed inset-0 z-50 flex items-center justify-center bg-black/20 backdrop-blur-md"

                    >

                        <motion.div

                            initial={{ scale: 0, rotate: "180deg" }}

                            animate={{

                                scale: 1,

                                rotate: "0deg",

                                transition: {

                                    type: "spring",

                                    bounce: 0.25,

                                },

                            }}

                            exit={{ scale: 0, rotate: "180deg" }}

                            className="relative mx-4 aspect-video w-full max-w-4xl md:mx-0"

                            onClick={(e) => e.stopPropagation()}

                        >

                            <motion.button

                                className="absolute -top-16 right-0 rounded-full bg-neutral-900/50 p-2 text-xl text-white ring-1 backdrop-blur-md dark:bg-neutral-100/50 dark:text-black"

                                onClick={() => setIsVideoOpen(false)}

                            >

                                <XIcon className="size-5" />

                            </motion.button>

                            <div className="relative isolate z-[1] size-full overflow-hidden rounded-2xl border-2 border-white">

                                <iframe

                                    src={videoSrc}

                                    className="size-full rounded-2xl"

                                    allowFullScreen

                                    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"

                                ></iframe>

                            </div>

                        </motion.div>

                    </motion.div>

                )}

            </AnimatePresence>

        </div>

    );

}
```
---

## Phone Mockups 1 <a name="phone-mockups-1"></a>

Solace UI phone mockups component variation 1.

- **Registry Key**: `phone-mockups-1`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/phone-mockups-1.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`
- **Registry Dependencies**:
  - `button`

#### Component Source Code
##### File: `src/registry/components/phone-mockups/one/index.tsx`
```tsx
import React from "react";

import {
    ImageItem,
    PhoneCarousel,
} from "@/registry/components/phone-mockups/one/phone-carousel";


const exampleImages: ImageItem[] = [

    {

        src: "https://res.cloudinary.com/harshitproject/image/upload/v1746774805/Behance-screen.png",

        alt: "Behance app on iPhone",

    },

    {

        src: "https://res.cloudinary.com/harshitproject/image/upload/v1746774805/Notion-screen.png",

        alt: "Notion app on iPhone",

    },

    {

        src: "https://res.cloudinary.com/harshitproject/image/upload/v1746774806/One-screen.png",

        alt: "One app on iPhone",

    },

    {

        src: "https://res.cloudinary.com/harshitproject/image/upload/v1746774807/Reddit-nj7hwh.png",

        alt: "Reddit app on iPhone",

    },

];



export default function PhoneMockupBasic() {

    return <PhoneCarousel images={exampleImages} />;

}
```
##### File: `src/registry/components/phone-mockups/one/phone-carousel.tsx`
```tsx
"use client";

import type React from "react";

import { useEffect, useState, useRef } from "react";

import Image from "next/image";

import { ChevronLeft, ChevronRight, Pause, Play } from "lucide-react";

import { Button } from "@/components/ui/button";

import { cn } from "@/lib/utils";

import { useIsMobile } from "@/registry/components/phone-mockups/one/use-mobile";


interface Iphone15ProProps extends React.SVGProps<SVGSVGElement> {

    width?: string | number;

    height?: string | number;

    src?: string;

    alt?: string;

}



const Iphone15Pro: React.FC<Iphone15ProProps> = ({

    width = "100%",

    height = "auto",

    src,

    alt = "iPhone screen content",

    className,

    ...props

}) => {

    return (

        <div className={cn("relative", className)}>

            <svg

                width={width}

                height={height}

                viewBox="0 0 433 882"

                preserveAspectRatio="xMidYMid meet"

                fill="none"

                xmlns="http://www.w3.org/2000/svg"

                className="transition-all duration-500 ease-in-out"

                {...props}

            >

                {/* Outer frame */}

                <path

                    d="M2 73C2 32.6832 34.6832 0 75 0H357C397.317 0 430 32.6832 430 73V809C430 849.317 397.317 882 357 882H75C34.6832 882 2 849.317 2 809V73Z"

                    className="dark:fill-[#DADADA] fill-[#404040]"

                />

                {/* side nubs */}

                <path

                    d="M0 171C0 170.448 0.447715 170 1 170H3V204H1C0.447715 204 0 203.552 0 203V171Z"

                    className="dark:fill-[#DADADA] fill-[#404040]"

                />

                <path

                    d="M1 234C1 233.448 1.44772 233 2 233H3.5V300H2C1.44772 300 1 299.552 1 299V234Z"

                    className="dark:fill-[#DADADA] fill-[#404040]"

                />

                <path

                    d="M1 319C1 318.448 1.44772 318 2 318H3.5V385H2C1.44772 385 1 384.552 1 384V319Z"

                    className="dark:fill-[#DADADA] fill-[#404040]"

                />

                <path

                    d="M430 279H432C432.552 279 433 279.448 433 280V384C433 384.552 432.552 385 432 385H430V279Z"

                    className="dark:fill-[#DADADA] fill-[#404040]"

                />

                {/* inner body */}

                <path

                    d="M6 74C6 35.3401 37.3401 4 76 4H356C394.66 4 426 35.3401 426 74V808C426 846.66 394.66 878 356 878H76C37.3401 878 6 846.66 6 808V74Z"

                    className="fill-[#262626] dark:fill-gradient-to-b dark:from-white dark:to-[#F0F0F0]"

                />

                <path

                    opacity="0.5"

                    d="M174 5H258V5.5C258 6.60457 257.105 7.5 256 7.5H176C174.895 7.5 174 6.60457 174 5.5V5Z"

                    className="dark:fill-[#DADADA] fill-[#404040]"

                />

                {/* screen area */}

                <path

                    d="M21.25 75C21.25 44.2101 46.2101 19.25 77 19.25H355C385.79 19.25 410.75 44.2101 410.75 75V807C410.75 837.79 385.79 862.75 355 862.75H77C46.2101 862.75 21.25 837.79 21.25 807V75Z"

                    className="dark:fill-[#F5F5F5] fill-[#404040] dark:stroke-[#E0E0E0] stroke-[#404040] stroke-[0.5]"

                    filter="drop-shadow(0px 2px 4px rgba(0, 0, 0, 0.1))"

                />

                {src && (

                    <foreignObject

                        x="21.25"

                        y="19.25"

                        width="389.5"

                        height="843.5"

                        clipPath="url(#roundedCorners)"

                    >

                        <div

                            style={{ width: "100%", height: "100%", position: "relative" }}

                        >

                            <Image

                                src={src || "/placeholder.svg"}

                                alt={alt}

                                fill

                                style={{ objectFit: "cover" }}

                                sizes="(max-width: 768px) 80vw, (max-width: 1200px) 50vw, 33vw"

                                priority

                            />

                        </div>

                    </foreignObject>

                )}

                {/* notch area */}

                <path

                    d="M154 48.5C154 38.2827 162.283 30 172.5 30H259.5C269.717 30 278 38.2827 278 48.5C278 58.7173 269.717 67 259.5 67H172.5C162.283 67 154 58.7173 154 48.5Z"

                    className="fill-[#262626] dark:fill-[#F0F0F0] dark:drop-shadow-sm"

                />

                <path

                    d="M249 48.5C249 42.701 253.701 38 259.5 38C265.299 38 270 42.701 270 48.5C270 54.299 265.299 59 259.5 59C253.701 59 249 54.299 249 48.5Z"

                    className="fill-[#262626] dark:fill-[#F0F0F0]"

                />

                <path

                    d="M254 48.5C254 45.4624 256.462 43 259.5 43C262.538 43 265 45.4624 265 48.5C265 51.5376 262.538 54 259.5 54C256.462 54 254 51.5376 254 48.5Z"

                    className="fill-[#262626] dark:fill-[#E0E0E0]"

                />

                {/* highlight */}

                <path

                    d="M76 4C37.3401 4 6 35.3401 6 74V808C6 846.66 37.3401 878 76 878H356C394.66 878 426 846.66 426 808V74C426 35.3401 394.66 4 356 4H76Z"

                    className="fill-transparent dark:stroke-white/20 stroke-[0.5] stroke-transparent"

                />

                <defs>

                    <clipPath id="roundedCorners">

                        <rect

                            x="21.25"

                            y="19.25"

                            width="389.5"

                            height="843.5"

                            rx="55.75"

                            ry="55.75"

                        />

                    </clipPath>

                </defs>

            </svg>

        </div>

    );

};



export interface ImageItem {

    src: string;

    alt: string;

}



interface PhoneCarouselProps {

    images: ImageItem[];

    className?: string;

    featureMode?: boolean;

    featuresData?: { images: ImageItem[] }[];

    activeFeatureIndex?: number;

}



export const PhoneCarousel: React.FC<PhoneCarouselProps> = ({

    images,

    className,

    featureMode,

    featuresData,

    activeFeatureIndex = 0,

}) => {

    const [isClient, setIsClient] = useState<boolean>(false);

    const [currentIndex, setCurrentIndex] = useState<number>(0);

    const [isPaused, setIsPaused] = useState<boolean>(false);

    const [isHovering, setIsHovering] = useState<boolean>(false);

    const carouselRef = useRef<HTMLDivElement>(null);

    const isMobile = useIsMobile();



    useEffect(() => {

        setIsClient(true);

    }, []);



    useEffect(() => {

        if (featureMode) return;



        let interval: NodeJS.Timeout;

        if (!isPaused && !isHovering) {

            interval = setInterval(() => {

                setCurrentIndex((prevIndex) => (prevIndex + 1) % images.length);

            }, 3000);

        }

        return () => clearInterval(interval);

    }, [isPaused, isHovering, images.length, featureMode]);



    if (!isClient) {

        return (

            <div className="w-full h-[400px] flex items-center justify-center">

                <div className="animate-pulse w-64 h-96 rounded-3xl"></div>

            </div>

        );

    }



    // FEATURE MODE

    if (featureMode && featuresData) {

        const total = featuresData.length;

        const active = activeFeatureIndex;

        const prev = (active - 1 + total) % total;

        const next = (active + 1) % total;



        // Each feature has a single image

        const prevImage = featuresData[prev].images[0];

        const activeImage = featuresData[active].images[0];

        const nextImage = featuresData[next].images[0];



        return (

            <section

                className={cn(

                    "relative w-full py-6 md:py-10 overflow-visible",

                    className

                )}

                aria-label="iPhone product showcase in feature mode"

            >

                <div className="relative h-[600px] sm:h-[650px] lg:h-[700px] w-full">

                    {/* Center the phone stack */}

                    <div className="absolute top-0 left-1/2 transform -translate-x-1/2">

                        {/* 1) Back phone (prev) */}

                        <div

                            className="absolute opacity-60"

                            style={{

                                transform: "translateY(-20px) scale(0.92)",

                                zIndex: 10,

                            }}

                        >

                            <Iphone15Pro

                                width={isMobile ? 280 : 350}

                                height="auto"

                                src={prevImage.src}

                                alt={prevImage.alt}

                            />

                        </div>



                        {/* 2) Middle phone (next) */}

                        <div

                            className="absolute opacity-80"

                            style={{

                                transform: "translateY(25px) scale(0.96)",

                                zIndex: 20,

                            }}

                        >

                            <Iphone15Pro

                                width={isMobile ? 280 : 350}

                                height="auto"

                                src={nextImage.src}

                                alt={nextImage.alt}

                            />

                        </div>



                        {/* 3) Front phone (active) */}

                        <div

                            className="relative"

                            style={{

                                transform: "translateY(70px) scale(1)",

                                zIndex: 30,

                            }}

                        >

                            <Iphone15Pro

                                width={isMobile ? 280 : 350}

                                height="auto"

                                src={activeImage.src}

                                alt={activeImage.alt}

                            />

                        </div>

                    </div>

                </div>

            </section>

        );

    }



    // NORMAL MODE

    const handlePrevious = () => {

        setCurrentIndex((prevIndex) =>

            prevIndex === 0 ? images.length - 1 : prevIndex - 1

        );

    };



    const handleNext = () => {

        setCurrentIndex((prevIndex) => (prevIndex + 1) % images.length);

    };



    const togglePause = () => {

        setIsPaused((prev) => !prev);

    };



    return (

        <section

            className="relative w-full py-6 md:py-10 overflow-hidden"

            aria-label="iPhone product showcase"

        >

            <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">

                <div className="relative">

                    {/* Main carousel container */}

                    <div

                        ref={carouselRef}

                        className="flex justify-center items-start h-[410px] md:h-[510px] lg:h-[520px]"

                        onMouseEnter={() => setIsHovering(true)}

                        onMouseLeave={() => setIsHovering(false)}

                    >

                        <div className="relative flex justify-center w-full">

                            {images.map((image, index) => {

                                const isActive = index === currentIndex;

                                const isPrevious =

                                    index === currentIndex - 1 ||

                                    (currentIndex === 0 && index === images.length - 1);

                                const isNext =

                                    index === currentIndex + 1 ||

                                    (currentIndex === images.length - 1 && index === 0);



                                return (

                                    <div

                                        key={index}

                                        className={cn(

                                            "absolute transition-all duration-700 ease-in-out transform",

                                            isActive ? "z-20 scale-100" : "opacity-0 scale-90",

                                            isPrevious ? "-translate-x-[10%] opacity-30 z-10" : "",

                                            isNext ? "translate-x-[10%] opacity-30 z-10" : "",

                                            !isActive && !isPrevious && !isNext ? "opacity-0" : ""

                                        )}

                                        style={{

                                            top: "0",

                                            transform: `translateY(0px) ${isPrevious

                                                    ? "translateX(-60%)"

                                                    : isNext

                                                        ? "translateX(60%)"

                                                        : "translateX(0)"

                                                } ${isActive ? "scale(1)" : "scale(0.9)"}`,

                                        }}

                                        aria-hidden={!isActive}

                                    >

                                        <div className="group">

                                            <Iphone15Pro

                                                width={isMobile ? 280 : 350}

                                                height="auto"

                                                src={image.src}

                                                alt={image.alt}

                                                className="transition-all duration-100 hover:scale-105 hover:-rotate-6"

                                            />

                                        </div>

                                    </div>

                                );

                            })}

                        </div>

                    </div>



                    {/* Controls */}

                    <div className="absolute bottom-8 left-0 right-0 flex justify-center items-center gap-4 z-30">

                        <Button

                            variant="outline"

                            size="icon"

                            onClick={handlePrevious}

                            className="rounded-full bg-black/60 backdrop-blur-sm border-white/20 hover:bg-black/80 shadow-md"

                            aria-label="Previous image"

                        >

                            <ChevronLeft className="h-5 w-5 text-white" />

                        </Button>

                        <Button

                            variant="outline"

                            size="icon"

                            onClick={togglePause}

                            className="rounded-full bg-black/60 backdrop-blur-sm border-white/20 hover:bg-black/80 shadow-md"

                            aria-label={isPaused ? "Play slideshow" : "Pause slideshow"}

                        >

                            {isPaused ? (

                                <Play className="h-5 w-5 text-white" />

                            ) : (

                                <Pause className="h-5 w-5 text-white" />

                            )}

                        </Button>

                        <Button

                            variant="outline"

                            size="icon"

                            onClick={handleNext}

                            className="rounded-full bg-black/60 backdrop-blur-sm border-white/20 hover:bg-black/80 shadow-md"

                            aria-label="Next image"

                        >

                            <ChevronRight className="h-5 w-5 text-white" />

                        </Button>

                    </div>

                </div>

            </div>

        </section>

    );

};
```
##### File: `src/registry/components/phone-mockups/one/use-mobile.tsx`
```tsx
import * as React from "react";



const MOBILE_BREAKPOINT = 768;



export function useIsMobile() {

    const [isMobile, setIsMobile] = React.useState<boolean | undefined>(

        undefined

    );



    React.useEffect(() => {

        const mql = window.matchMedia(`(max-width: ${MOBILE_BREAKPOINT - 1}px)`);

        const onChange = () => {

            setIsMobile(window.innerWidth < MOBILE_BREAKPOINT);

        };

        mql.addEventListener("change", onChange);

        setIsMobile(window.innerWidth < MOBILE_BREAKPOINT);

        return () => mql.removeEventListener("change", onChange);

    }, []);



    return !!isMobile;

}
```
---

## Team Section 1 <a name="team-section-1"></a>

Solace UI team section block variation 1.

- **Registry Key**: `team-section-1`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/team-section-1.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### Component Source Code
##### File: `src/registry/blocks/team-section/one/index.tsx`
```tsx
"use client";



import { motion } from "motion/react";

import TeamMemberCard, { type TeamMember } from "@/registry/blocks/team-section/one/team-card-one";


const teamMembers: TeamMember[] = [

  {

    id: 1,

    name: "James",

    role: "Founder",

    image:

      "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-four.png",

    cardType: "white", // white, yellow, pink and white-alt

    social: "https://twitter.com/harshitlog",

  },

  {

    id: 2,

    name: "Charlotte ",

    role: "Design Engineer",

    image:

      "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png",

    cardType: "white",

    social: "https://twitter.com/harshitlog",

  },

  {

    id: 3,

    name: "Alexander",

    role: "Social Media Manager",

    image:

      "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-one.png",

    cardType: "white",

    social: "https://twitter.com/harshitlog",

  },

  {

    id: 4,

    name: "Olivia",

    role: "Product Manager",

    image:

      "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-three.png",

    cardType: "white",

    social: "https://twitter.com/harshitlog",

  },

];



export default function Team1() {

  return (

    <section className="py-16 flex flex-col items-center justify-center px-9">

      <motion.div

        initial={{ opacity: 0, y: 20 }}

        animate={{ opacity: 1, y: 0 }}

        transition={{ duration: 0.6 }}

        className="container mx-auto px-4"

      >

        <h2 className="text-4xl font-bold dark:text-white text-black text-center mb-12">

          Meet Our Team

        </h2>



        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-12 justify-items-center">

          {teamMembers.map((member) => (

            <TeamMemberCard key={member.id} member={member} />

          ))}

        </div>

      </motion.div>

    </section>

  );

}
```
##### File: `src/registry/blocks/team-section/one/team-card-one.tsx`
```tsx
"use client";



import { motion } from "motion/react";

import Image from "next/image";

import Link from "next/link";

import { useState } from "react";

import { MoveUpRight } from "lucide-react";



export type TeamMember = {

  id: number;

  name: string;

  role: string;

  image: string;

  cardType: "yellow" | "white" | "pink" | "white-alt";

  social?: string;

};



export default function TeamMemberCard({ member }: { member: TeamMember }) {

  const [isHovered, setIsHovered] = useState(false);



  // Card border colors based on type

  const cardBorder = {

    yellow: "border-yellow-300",

    white: "border-white",

    pink: "border-pink-500",

    "white-alt": "border-white",

  };



  return (

    <div

      className={`relative p-2 transition-all duration-300 transform hover:scale-105 ${

        isHovered ? "z-50" : "z-0"

      }`}

    >

      {/* Lanyard clip */}

      <div className="absolute left-1/2 -top-8 transform -translate-x-1/2 w-8 h-8 dark:bg-white bg-black rounded-full flex items-center justify-center z-10">

        <div className="w-4 h-4 dark:bg-[#212121] bg-white rounded-full"></div>

      </div>



      {/* Lanyard */}

      <div className="absolute left-1/2 -top-2 transform -translate-x-1/2 w-1 h-6 dark:bg-gray-300 bg-black"></div>



      {/* ID Card */}

      <motion.div

        className={`relative w-64 h-96 rounded-xl overflow-hidden shadow-lg border-2 gap-4 ${

          cardBorder[member.cardType]

        }`}

        initial={{ y: 10, opacity: 0 }}

        animate={{ y: 0, opacity: 1 }}

        transition={{ duration: 0.5 }}

        onMouseEnter={() => setIsHovered(true)}

        onMouseLeave={() => setIsHovered(false)}

      >

        {/* Background Image (Black & White to Color on Hover) */}

        <div className="absolute inset-0">

          <Image

            src={member.image || "/placeholder.svg?height=400&width=300"}

            alt={member.name}

            fill

            className={`object-cover filter transition duration-300 ${

              isHovered ? "grayscale-0" : "grayscale"

            }`}

          />

        </div>



        <div className="absolute bottom-2 left-0 right-0 z-10 text-center">

          <div className="text-xl font-bold text-white drop-shadow-md">

            Logo

          </div>

        </div>



        {/* Hover overlay with name and designation */}

        <motion.div

          initial={{ opacity: 0 }}

          animate={{ opacity: isHovered ? 1 : 0 }}

          transition={{ duration: 0.3 }}

          className="absolute inset-0 bg-black/70 flex flex-col items-center justify-center p-6 z-10"

        >

          <div className="text-center">

            {member.social ? (

              <Link

                href={member.social}

                target="_blank"

                className="inline-flex items-center gap-1"

              >

                <span className="text-2xl font-bold text-white leading-none">

                  {member.name}

                </span>

                {/* Adjust top offset if needed for perfect alignment */}

                <MoveUpRight className="w-4 h-4 text-white relative top-[1px]" />

              </Link>

            ) : (

              <span className="text-2xl font-bold text-white leading-none">

                {member.name}

              </span>

            )}

            <p className="text-lg text-white mt-1">{member.role}</p>

          </div>

        </motion.div>

      </motion.div>

    </div>

  );

}
```
---

## Team Section 2 <a name="team-section-2"></a>

Solace UI team section block variation 2.

- **Registry Key**: `team-section-2`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/team-section-2.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### Component Source Code
##### File: `src/registry/blocks/team-section/two/index.tsx`
```tsx
"use client";



import { motion } from "motion/react";

import { TeamCard2, TeamMember } from "@/registry/blocks/team-section/two/team-card-two";


const teamMembers: TeamMember[] = [

  {

    id: "1",

    name: "James",

    role: "CEO & Founder",

    bio: "Passionate about creating innovative solutions and leading teams to success.",

    imageUrl:

      "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-four.png",

    socialLinks: [

      { platform: "linkedin", url: "https://linkedin.com", icon: "linkedin" },

      { platform: "twitter", url: "https://twitter.com", icon: "twitter" },

    ],

  },

  {

    id: "2",

    name: "Charlotte",

    role: "CTO",

    bio: "Tech enthusiast with over 15 years of experience in software architecture.",

    imageUrl:

      "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png",

    socialLinks: [

      { platform: "github", url: "https://github.com", icon: "github" },

      { platform: "linkedin", url: "https://linkedin.com", icon: "linkedin" },

    ],

  },

  {

    id: "3",

    name: "Alexander",

    role: "Lead Designer",

    bio: "Creating beautiful and intuitive user experiences through thoughtful design.",

    imageUrl:

      "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-one.png",

    socialLinks: [

      { platform: "twitter", url: "https://twitter.com", icon: "twitter" },

      { platform: "linkedin", url: "https://linkedin.com", icon: "linkedin" },

    ],

  },

  {

    id: "4",

    name: "Olivia",

    role: "CMO",

    bio: "Driving brand growth and market presence with strategic campaigns.",

    imageUrl:

      "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-three.png",

    socialLinks: [

      { platform: "linkedin", url: "https://linkedin.com", icon: "linkedin" },

      { platform: "twitter", url: "https://twitter.com", icon: "twitter" },

    ],

  },

];



export default function Team2() {

  return (

    <section className="relative overflow-hidden py-16 sm:py-24">

      <div className="absolute inset-0 -z-10" />

      <div className="mx-auto max-w-7xl px-6 lg:px-8">

        <motion.div

          initial={{ opacity: 0, y: 20 }}

          whileInView={{ opacity: 1, y: 0 }}

          viewport={{ once: true }}

          transition={{ duration: 0.6 }}

          className="mx-auto max-w-2xl text-center"

        >

          <h2 className="text-3xl font-bold tracking-tight sm:text-4xl">

            Meet Our Team

          </h2>

        </motion.div>

        <ul

          role="list"

          className="mx-auto mt-16 grid max-w-2xl grid-cols-1 gap-x-8 gap-y-16 sm:grid-cols-2 lg:mx-0 lg:max-w-none lg:grid-cols-4"

        >

          {teamMembers.map((member, index) => (

            <TeamCard2 key={member.id} member={member} index={index} />

          ))}

        </ul>

      </div>

    </section>

  );

}
```
##### File: `src/registry/blocks/team-section/two/team-card-two.tsx`
```tsx
"use client";



import { motion } from "motion/react";

import Image from "next/image";

import {

  Github,

  Linkedin,

  Twitter,

  DivideIcon as LucideIcon,

} from "lucide-react";

import Link from "next/link";



export interface SocialLink {

  platform: string;

  url: string;

  icon: string;

}



export interface TeamMember {

  id: string;

  name: string;

  role: string;

  bio: string;

  imageUrl: string;

  socialLinks: SocialLink[];

}



interface TeamMemberCardProps {

  member: TeamMember;

  index: number;

}



const socialIcons: Record<string, typeof LucideIcon> = {

  github: Github,

  linkedin: Linkedin,

  twitter: Twitter,

};



export function TeamCard2({ member, index }: TeamMemberCardProps) {

  return (

    <motion.li

      initial={{ opacity: 0, y: 30 }}

      whileInView={{ opacity: 1, y: 0 }}

      viewport={{ once: true, margin: "-10%" }}

      transition={{ duration: 0.7, delay: index * 0.15, ease: [0.21, 0.47, 0.32, 0.98] as [number, number, number, number] }}
      className="group flex flex-col gap-6"

    >

      <div className="relative p-1">

        <div className="relative aspect-4/5 w-full overflow-hidden rounded-none bg-muted/30 shadow-[0_8px_30px_rgb(0,0,0,0.04)] ring-1 ring-black/5 transition-all duration-500 hover:shadow-[0_20px_40px_rgb(0,0,0,0.08)] dark:shadow-[0_8px_30px_rgb(255,255,255,0.02)] dark:ring-white/10 dark:hover:shadow-[0_20px_40px_rgb(255,255,255,0.06)]">

          <Image

            src={member.imageUrl}

            alt={member.name}

            fill

            className="object-cover object-top filter grayscale transition-all duration-700 ease-out group-hover:scale-[1.03] group-hover:grayscale-0 dark:brightness-90 dark:group-hover:brightness-100"

            sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 25vw"

            loading="lazy"

          />

          <div className="absolute inset-0 bg-linear-to-t from-black/20 via-transparent to-transparent opacity-0 transition-opacity duration-500 group-hover:opacity-100 dark:from-black/40" />

        </div>



        {/* Hover Focus Corners - positioned outside the image */}

        <div className="pointer-events-none absolute -left-2 -top-2 h-4 w-4 -translate-x-2 -translate-y-2 border-l-2 border-t-2 border-foreground/15 opacity-0 transition-all duration-500 ease-out group-hover:translate-x-0 group-hover:translate-y-0 group-hover:opacity-100 dark:border-foreground/50" />

        <div className="pointer-events-none absolute -right-2 -top-2 h-4 w-4 translate-x-2 -translate-y-2 border-r-2 border-t-2 border-foreground/15 opacity-0 transition-all duration-500 ease-out group-hover:translate-x-0 group-hover:translate-y-0 group-hover:opacity-100 dark:border-foreground/50" />

        <div className="pointer-events-none absolute -bottom-2 -left-2 h-4 w-4 -translate-x-2 translate-y-2 border-b-2 border-l-2 border-foreground/15 opacity-0 transition-all duration-500 ease-out group-hover:translate-x-0 group-hover:translate-y-0 group-hover:opacity-100 dark:border-foreground/50" />

        <div className="pointer-events-none absolute -bottom-2 -right-2 h-4 w-4 translate-x-2 translate-y-2 border-b-2 border-r-2 border-foreground/15 opacity-0 transition-all duration-500 ease-out group-hover:translate-x-0 group-hover:translate-y-0 group-hover:opacity-100 dark:border-foreground/50" />

      </div>



      <div className="flex flex-col px-2">

        <div className="flex items-start justify-between gap-4">

          <div className="flex flex-col">

            <h3 className="text-xl font-medium tracking-tight text-foreground/90 transition-colors group-hover:text-foreground">

              {member.name}

            </h3>

            <p className="mt-1 text-sm font-medium tracking-wide text-primary/70 uppercase">

              {member.role}

            </p>

          </div>

          <div className="flex flex-wrap gap-1.5 opacity-0 -translate-x-2 transition-all duration-500 ease-out group-hover:opacity-100 group-hover:translate-x-0">

            {member.socialLinks.map((link, i) => {

              const Icon = socialIcons[link.icon];

              return (

                <Link

                  key={link.platform}

                  href={link.url}

                  target="_blank"

                  rel="noopener noreferrer"

                  style={{ transitionDelay: `${i * 50}ms` }}

                  className="flex h-8 w-8 items-center justify-center rounded-md bg-secondary/50 text-muted-foreground ring-1 ring-border/50 transition-all duration-300 hover:scale-110 hover:bg-primary hover:text-primary-foreground hover:shadow-sm focus:outline-none focus:ring-2 focus:ring-ring focus:ring-offset-2"

                >

                  <Icon className="h-4 w-4" />

                  <span className="sr-only">{link.platform}</span>

                </Link>

              );

            })}

          </div>

        </div>

        <p className="mt-4 text-sm leading-tight text-muted-foreground/40 line-clamp-2 transition-colors duration-300 group-hover:text-muted-foreground">

          {member.bio}

        </p>

      </div>

    </motion.li>

  );

}
```
---

## Team Section 3 <a name="team-section-3"></a>

Solace UI team section block variation 3.

- **Registry Key**: `team-section-3`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/team-section-3.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/team-section/three/index.tsx`
```tsx
"use client";

import { motion } from "motion/react";

import Image from "next/image";

import Link from "next/link";



interface TeamMember {

  id: string;

  name: string;

  role: string;

  imageUrl: string;

  socialLinks: string;

}



const teamMembers: TeamMember[] = [

  {

    id: "1",

    name: "James",

    role: "CEO & Founder",

    imageUrl:

      "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-four.png",

    socialLinks: "https://twitter.com/harshitlog",

  },

  {

    id: "2",

    name: "Charlotte",

    role: "CTO",

    imageUrl:

      "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png",

    socialLinks: "https://twitter.com/harshitlog",

  },

  {

    id: "3",

    name: "Alexander",

    role: "Lead Designer",

    imageUrl:

      "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-one.png",

    socialLinks: "https://twitter.com/harshitlog",

  },

  {

    id: "4",

    name: "Olivia",

    role: "CEO & Founder",

    imageUrl:

      "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-three.png",

    socialLinks: "https://twitter.com/harshitlog",

  },

  {

    id: "5",

    name: "Clark",

    role: "Visual Designer",

    imageUrl:

      "https://res.cloudinary.com/harshitproject/image/upload/v1746774431/member-two.png",

    socialLinks: "https://twitter.com/harshitlog",

  },

  {

    id: "6",

    name: "Pallavi",

    role: "Growth Engineer",

    imageUrl: "https://res.cloudinary.com/harshitproject/image/upload/v1774017708/member-six.png",

    socialLinks: "https://twitter.com/harshitlog",

  },

];



export default function Team3() {

  return (

    <section className="relative overflow-hidden py-24 sm:py-32 ">

      <div className="mx-auto max-w-6xl px-6 lg:px-8">

        <motion.div

          initial={{ opacity: 0, y: 20 }}

          whileInView={{ opacity: 1, y: 0 }}

          viewport={{ once: true }}

          transition={{ duration: 0.6 }}

          className="mx-auto max-w-2xl text-center"

        >

          <h2 className="text-3xl font-bold tracking-tight dark:text-white text-black sm:text-4xl">

            Meet Our Team

          </h2>

          <p className="mt-4 text-lg leading-8 text-gray-400">

            We&apos;re a dynamic group of individuals who are passionate about

            what we do.

          </p>

        </motion.div>



        <div className="relative mt-16 sm:mt-20">

          <motion.div

            initial={{ opacity: 0 }}

            whileInView={{ opacity: 1 }}

            viewport={{ once: true }}

            transition={{ duration: 0.6, delay: 0.2 }}

            className="flex flex-wrap justify-center items-center gap-6 lg:max-w-[850px] mx-auto"

          >

            {teamMembers.map((member, index) => (

              <motion.div

                key={member.id}

                initial={{ scale: 0 }}

                whileInView={{ scale: 1 }}

                viewport={{ once: true }}

                transition={{

                  duration: 0.6,

                  delay: 0.3 + index * 0.1,

                }}

                className="relative shrink-0 w-40 h-40 md:w-48 md:h-48 rounded-full overflow-hidden hover:scale-110 md:hover:scale-125 transition-transform duration-300 hover:z-10"

              >

                <div className="relative w-full h-full ">

                  <Image

                    src={member.imageUrl}

                    alt={member.name}

                    fill

                    className="object-cover object-top"

                  />

                  <div className="absolute inset-0 bg-black/40 opacity-0 hover:opacity-100 transition-opacity duration-300 flex items-center justify-center">

                    <Link

                      href={member.socialLinks}

                      className="flex flex-col text-center p-4"

                    >

                      <span className="text-white font-semibold text-lg underline">

                        {member.name}

                      </span>

                      <span className="text-white/80 text-sm">

                        {member.role}

                      </span>

                    </Link>

                  </div>

                </div>

              </motion.div>

            ))}

          </motion.div>

        </div>

      </div>

    </section>

  );

}
```
---

## Team Section 4 <a name="team-section-4"></a>

Solace UI team section block variation 4.

- **Registry Key**: `team-section-4`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/team-section-4.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### Component Source Code
##### File: `src/registry/blocks/team-section/four/index.tsx`
```tsx
"use client";

import { useState } from "react";
import { motion, AnimatePresence } from "motion/react";
import Image from "next/image";
import { ArrowLeft, ArrowRight } from "lucide-react";
import { cn } from "@/lib/utils";

interface TeamMember {
  id: string;
  name: string;
  role: string;
  image: string;
}

const teamMembers: TeamMember[] = [
  {
    id: "1",
    name: "James Mitchell",
    role: "Product Manager",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-four.png",
  },
  {
    id: "2",
    name: "Sarah Jenkins",
    role: "Lead Designer",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png",
  },
  {
    id: "3",
    name: "David Kim",
    role: "Founder and CEO",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-one.png",
  },
  {
    id: "4",
    name: "Alex Rivera",
    role: "Software Engineer",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774431/member-two.png",
  },
  {
    id: "5",
    name: "Dr. Helen Cho",
    role: "Research Director",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-three.png",
  },
];

export default function Team4() {
  const [activeIndex, setActiveIndex] = useState<number>(2);

  const handlePrev = () => {
    setActiveIndex((prev) => (prev - 1 + teamMembers.length) % teamMembers.length);
  };

  const handleNext = () => {
    setActiveIndex((prev) => (prev + 1) % teamMembers.length);
  };

  return (
    <section className="relative w-full py-24 bg-background overflow-hidden flex flex-col items-center justify-center">
      <div className="w-full max-w-6xl px-4 md:px-8">

        {/* Main Images & Floating text container */}
        <div className="relative w-full h-[400px] md:h-[550px] lg:h-[600px] mt-12">

          {/* Top Fade Overlay */}
          <div className="absolute top-0 left-0 right-0 h-28 bg-gradient-to-b from-background via-background/60 to-transparent pointer-events-none z-10" />

          {/* Bottom Fade Overlay */}
          <div className="absolute bottom-0 left-0 right-0 h-28 bg-gradient-to-t from-background via-background/60 to-transparent pointer-events-none z-10" />

          {/* Cards Flex Row */}
          <div className="flex w-full h-full gap-2 sm:gap-4 md:gap-1 items-stretch overflow-visible">
            {teamMembers.map((member, index) => {
              const isActive = index === activeIndex;

              return (
                <div
                  key={member.id}
                  onClick={() => setActiveIndex(index)}
                  className={cn(
                    "relative cursor-pointer transition-all duration-700 ease-out flex-1",
                    isActive ? "flex-[2] md:flex-[2.5]" : "flex-[0.8] md:flex-[1]"
                  )}
                >
                  {/* Floating active member text positioned above upper fade */}
                  <AnimatePresence mode="wait">
                    {isActive && (
                      <motion.div
                        layoutId="activeTeamText"
                        initial={{ opacity: 0, y: 10 }}
                        animate={{ opacity: 1, y: 0 }}
                        exit={{ opacity: 0, y: -10 }}
                        transition={{ duration: 0.5, ease: "easeOut" }}
                        className={cn(
                          "absolute bottom-[calc(100%+16px)] z-20 whitespace-nowrap pointer-events-none",
                          index >= 3 ? "right-0 text-right" : "left-0 text-left"
                        )}
                      >
                        <span className="font-sans font-semibold text-foreground text-md md:text-xl">
                          {member.name},
                        </span>
                        <span className="font-sans text-muted-foreground/60 dark:text-zinc-500 text-md md:text-xl ml-1.5">
                          {member.role}
                        </span>
                      </motion.div>
                    )}
                  </AnimatePresence>

                  {/* Individual member image card */}
                  <div className="w-full h-full rounded-lg overflow-hidden relative shadow-sm">
                    <Image
                      src={member.image}
                      alt={member.name}
                      fill
                      className={cn(
                        "object-cover object-center transition-all duration-700 ease-out",
                        isActive
                          ? "grayscale-0 scale-100 contrast-[1.05]"
                          : "grayscale contrast-[0.95] brightness-90 md:hover:brightness-95 md:hover:scale-[1.01]"
                      )}
                      sizes="(max-width: 768px) 30vw, 25vw"
                      priority={index === 2}
                    />
                  </div>
                </div>
              );
            })}
          </div>
        </div>

        {/* Navigation buttons at the bottom center */}
        <div className="flex justify-center items-center gap-2.5 mt-10">
          <button
            onClick={handlePrev}
            className="w-7 h-7 bg-zinc-950 dark:bg-zinc-50 hover:bg-zinc-800 dark:hover:bg-zinc-200 text-zinc-50 dark:text-zinc-950 flex items-center justify-center transition-colors focus:outline-none cursor-pointer"
            aria-label="Previous team member"
          >
            <ArrowLeft className="w-3.5 h-3.5" />
          </button>
          <button
            onClick={handleNext}
            className="w-7 h-7 bg-zinc-950 dark:bg-zinc-50 hover:bg-zinc-800 dark:hover:bg-zinc-200 text-zinc-50 dark:text-zinc-950 flex items-center justify-center transition-colors focus:outline-none cursor-pointer"
            aria-label="Next team member"
          >
            <ArrowRight className="w-3.5 h-3.5" />
          </button>
        </div>

      </div>
    </section>
  );
}
```
---

## Team Section 5 <a name="team-section-5"></a>

Solace UI team section block variation 5.

- **Registry Key**: `team-section-5`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/team-section-5.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### Component Source Code
##### File: `src/registry/blocks/team-section/five/index.tsx`
```tsx
"use client";

import { useState } from "react";
import Image from "next/image";
import { motion, AnimatePresence } from "motion/react";
import { X } from "lucide-react";
import { cn } from "@/lib/utils";

interface TeamMember {
  id: string;
  name: string;
  role: string;
  image: string;
  xUrl?: string;
  linkedinUrl?: string;
}

const teamMembers: TeamMember[] = [
  {
    id: "member-1",
    name: "Sarah Chen",
    role: "Head of Design",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png",
    xUrl: "https://x.com",
    linkedinUrl: "https://linkedin.com",
  },
  {
    id: "member-2",
    name: "Marcus Rodriguez",
    role: "VP of Engineering",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-one.png",
    xUrl: "https://x.com",
    linkedinUrl: "https://linkedin.com",
  },
  {
    id: "member-3",
    name: "James Mitchell",
    role: "Product Manager",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-four.png",
    xUrl: "https://x.com",
    linkedinUrl: "https://linkedin.com",
  },
  {
    id: "member-4",
    name: "David Kim",
    role: "Founder & CEO",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-two.png",
    xUrl: "https://x.com",
    linkedinUrl: "https://linkedin.com",
  },
  {
    id: "member-5",
    name: "Olivia Koe",
    role: "Lead Designer",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1774017708/member-six.png",
    xUrl: "https://x.com",
    linkedinUrl: "https://linkedin.com",
  },
  {
    id: "member-6",
    name: "Amara Okonkwo",
    role: "CTO",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-three.png",
    xUrl: "https://x.com",
    linkedinUrl: "https://linkedin.com",
  },
  {
    id: "member-7",
    name: "Sarah Chen",
    role: "Head of Design",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png",
    xUrl: "https://x.com",
    linkedinUrl: "https://linkedin.com",
  },
];

// Layout coordinates normalized to percentages from design coordinates:
// Container aspect ratio: 100/76.1
// Card dimensions: width = 19.65cqw, height = 18.5cqw (Polaroid aspect ratio ~17/16)
const cardLayouts = [
  { left: "58.5cqw", top: "6cqw" },     // Card 1: Sarah Chen
  { left: "80.35cqw", top: "6cqw" },    // Card 2: Marcus Rodriguez
  { left: "0cqw", top: "31.8cqw" },     // Card 3: James Mitchell
  { left: "21.85cqw", top: "31.8cqw" }, // Card 4: David Kim
  { left: "80.35cqw", top: "31.8cqw" }, // Card 5: Olivia Koe
  { left: "58.5cqw", top: "57.6cqw" },    // Card 6: Amara Okonkwo
  { left: "80.35cqw", top: "57.6cqw" },   // Card 7: Sarah Chen (repeated)
];

const TwitterIcon = () => (
  <svg
    width="20"
    height="18"
    viewBox="0 0 25 22"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    className="w-5 h-auto text-white/50 hover:text-white transition-colors"
  >
    <path
      d="M10.558 14.078L16.313 21.75H24.771L15.276 9.089L23.176 0H19.974L13.791 7.112L8.458 0H0L9.075 12.101L0.689 21.75H3.891L10.558 14.078ZM17.521 19.333L4.833 2.417H7.25L19.938 19.333H17.521Z"
      fill="currentColor"
    />
  </svg>
);

const LinkedInIcon = () => (
  <svg
    width="18"
    height="18"
    viewBox="0 0 23 23"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    className="w-4.5 h-auto text-white/50 hover:text-white transition-colors"
  >
    <path
      d="M20 0C20.663 0 21.299 0.263 21.768 0.732C22.237 1.201 22.5 1.837 22.5 2.5V20C22.5 20.663 22.237 21.299 21.768 21.768C21.299 22.237 20.663 22.5 20 22.5H2.5C1.837 22.5 1.201 22.237 0.732 21.768C0.263 21.299 0 20.663 0 20V2.5C0 1.837 0.263 1.201 0.732 0.732C1.201 0.263 1.837 0 2.5 0H20ZM19.375 19.375V12.75C19.375 11.669 18.946 10.633 18.181 9.869C17.417 9.104 16.381 8.675 15.3 8.675C14.238 8.675 13 9.325 12.4 10.3V8.912H8.912V19.375H12.4V13.213C12.4 12.25 13.175 11.463 14.137 11.463C14.602 11.463 15.047 11.647 15.375 11.975C15.703 12.303 15.887 12.748 15.887 13.213V19.375H19.375ZM4.85 6.95C5.407 6.95 5.941 6.729 6.335 6.335C6.729 5.941 6.95 5.407 6.95 4.85C6.95 3.688 6.013 2.737 4.85 2.737C4.29 2.737 3.752 2.96 3.356 3.356C2.96 3.752 2.737 4.29 2.737 4.85C2.737 6.013 3.688 6.95 4.85 6.95ZM6.588 19.375V8.912H3.125V19.375H6.588Z"
      fill="currentColor"
    />
  </svg>
);

export default function Team5() {
  const [selectedMember, setSelectedMember] = useState<TeamMember | null>(null);

  return (
    <section className="relative w-full py-20 md:py-28 bg-white dark:bg-background overflow-hidden flex flex-col items-center justify-center">
      {/* Dashed background pattern spanning end-to-end (full viewport width) */}
      <div className="absolute inset-x-0 top-1/2 -translate-y-1/2 h-[82%] z-0 opacity-10 bg-[repeating-linear-gradient(315deg,currentColor_0,currentColor_1px,transparent_0,transparent_50%)] bg-[length:10px_10px] border-y border-black dark:border-white pointer-events-none"></div>

      <div className="w-full max-w-5xl px-4 md:px-8 z-10 flex flex-col items-center justify-center">

        {/* Main Grid Wrapper utilizing Container Queries for flawless scaling */}
        <div
          className={cn(
            "@container relative w-full aspect-[100/76.1] z-10 transition-all duration-500 ease-in-out",
            selectedMember ? "opacity-15 pointer-events-none blur-[1.5px]" : "opacity-100"
          )}
        >
          {/* Background Text "Meet" - Styled to sit at same top and height as Row 1 Cards */}
          <div
            className="absolute left-0 font-sans font-semibold text-black dark:text-white select-none tracking-tight flex items-center leading-none"
            style={{
              top: "6cqw",
              height: "18.5cqw",
              fontSize: "25cqw",
            }}
          >
            Meet
          </div>

          {/* Background Text "The" - Styled to sit at same top and height as Row 2 Cards */}
          <div
            className="absolute font-sans font-semibold text-black dark:text-white select-none tracking-tight flex items-center leading-none"
            style={{
              left: "43.7cqw",
              top: "31.8cqw",
              height: "18.5cqw",
              fontSize: "25cqw",
            }}
          >
            The
          </div>

          {/* Background Text "Team" - Styled to sit at same top and height as Row 3 Cards */}
          <div
            className="absolute left-0 font-sans font-semibold text-black dark:text-white select-none tracking-tight flex items-center leading-none"
            style={{
              top: "57.6cqw",
              height: "18.5cqw",
              fontSize: "25cqw",
            }}
          >
            Team
          </div>

          {/* Polaroid Cards - Positioned on the exact same levels and heights as the text boxes */}
          {teamMembers.map((member, index) => {
            const layout = cardLayouts[index];
            return (
              <motion.div
                key={`${member.id}-${index}`}
                layoutId={`card-${member.id}-${index}`}
                onClick={() => setSelectedMember(member)}
                className="absolute p-[1%] bg-white/95 dark:bg-zinc-900/95 border border-neutral-200/50 dark:border-zinc-800/50 shadow-md shadow-black/5 hover:shadow-xl dark:shadow-black/30 rounded-[4%] cursor-pointer flex flex-col justify-between"
                style={{
                  left: layout.left,
                  top: layout.top,
                  width: "19.65cqw",
                  height: "18.5cqw",
                }}
                whileHover={{
                  scale: 1.04,
                  rotate: index % 2 === 0 ? 1 : -1,
                }}
                transition={{
                  type: "spring",
                  stiffness: 300,
                  damping: 20,
                }}
              >
                <div className="relative w-full h-[85%] rounded-[3%] overflow-hidden bg-neutral-100 dark:bg-zinc-800">
                  <Image
                    src={member.image}
                    alt={member.name}
                    fill
                    className={cn(
                      "object-cover object-top transition-all duration-500",
                      "grayscale hover:grayscale-0"
                    )}
                    sizes="(max-width: 768px) 15vw, 10vw"
                  />
                </div>
                {/* Subtle placeholder line to balance the Polaroid look */}
                <div className="w-1/3 h-1 bg-neutral-200 dark:bg-zinc-700 rounded-full mx-auto mt-auto opacity-40" />
              </motion.div>
            );
          })}
        </div>
      </div>

      {/* Expanded Modal Card */}
      <AnimatePresence>
        {selectedMember && (
          <div className="fixed inset-0 z-50 flex items-center justify-center p-4">
            {/* Backdrop */}
            <motion.div
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              exit={{ opacity: 0 }}
              onClick={() => setSelectedMember(null)}
              className="absolute inset-0 bg-white/20 dark:bg-black/40 backdrop-blur-md cursor-pointer"
            />

            {/* Modal Detail Card */}
            {(() => {
              // Find the index of the selected member to get its original slot for layout animation
              const index = teamMembers.findIndex((m) => m.id === selectedMember.id);
              return (
                <motion.div
                  layoutId={`card-${selectedMember.id}-${index}`}
                  className="relative w-full max-w-[340px] sm:max-w-[400px] aspect-[134/193.75] bg-black text-white p-3.5 pb-6 rounded-xl border-4 border-black shadow-2xl z-50 flex flex-col justify-between"
                  transition={{
                    type: "spring",
                    stiffness: 260,
                    damping: 25,
                  }}
                >
                  {/* Close button on top-right of image */}
                  <button
                    onClick={() => setSelectedMember(null)}
                    className="absolute top-6 right-6 z-50 p-1.5 rounded-full bg-black/60 hover:bg-black/90 text-white/80 hover:text-white border border-white/10 transition-colors cursor-pointer"
                    aria-label="Close profile details"
                  >
                    <X className="w-4 h-4" />
                  </button>

                  {/* Polaroid Image Frame - Styled to be larger with thin padding around the edges */}
                  <div className="relative w-full h-[78%] rounded-lg overflow-hidden bg-zinc-900 border border-zinc-800/55">
                    <Image
                      src={selectedMember.image}
                      alt={selectedMember.name}
                      fill
                      className="object-cover object-top"
                      sizes="(max-width: 768px) 80vw, 40vw"
                      priority
                    />
                  </div>

                  {/* Member Info & Socials Section */}
                  <div className="mt-4 flex items-end justify-between px-2">
                    <div className="flex flex-col text-left">
                      <h3 className="font-sans font-semibold text-white text-xl sm:text-2xl tracking-tight leading-none">
                        {selectedMember.name}
                      </h3>
                      <p className="font-sans font-light text-neutral-400 text-sm sm:text-base mt-2.5 leading-none">
                        {selectedMember.role}
                      </p>
                    </div>
                    <div className="flex items-center gap-3 pb-0.5">
                      {selectedMember.xUrl && (
                        <a
                          href={selectedMember.xUrl}
                          target="_blank"
                          rel="noopener noreferrer"
                          className="hover:text-white transition-colors"
                          aria-label="Twitter profile"
                        >
                          <TwitterIcon />
                        </a>
                      )}
                      {selectedMember.linkedinUrl && (
                        <a
                          href={selectedMember.linkedinUrl}
                          target="_blank"
                          rel="noopener noreferrer"
                          className="hover:text-white transition-colors"
                          aria-label="LinkedIn profile"
                        >
                          <LinkedInIcon />
                        </a>
                      )}
                    </div>
                  </div>
                </motion.div>
              );
            })()}
          </div>
        )}
      </AnimatePresence>
    </section>
  );
}
```
---

## Team Section 6 <a name="team-section-6"></a>

Solace UI team section block variation 6.

- **Registry Key**: `team-section-6`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/team-section-6.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/team-section/six/index.tsx`
```tsx
"use client";

import { useState } from "react";
import Image from "next/image";
import { motion, AnimatePresence } from "motion/react";
import { cn } from "@/lib/utils";

interface TeamMember {
  id: string;
  name: string;
  role: string;
  image: string;
  categories: string[];
}

const teamMembers: TeamMember[] = [
  {
    id: "member-1",
    name: "David Kim",
    role: "Founder & CEO",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-two.png",
    categories: ["Product", "GTM"],
  },
  {
    id: "member-2",
    name: "Sarah Chen",
    role: "Head of Design",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png",
    categories: ["Design"],
  },
  {
    id: "member-3",
    name: "James Mitchell",
    role: "Product Manager",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-four.png",
    categories: ["Product", "GTM"],
  },
  {
    id: "member-4",
    name: "Marcus Rodriguez",
    role: "VP of Engineering",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-one.png",
    categories: ["Security", "Product"],
  },
  {
    id: "member-5",
    name: "Olivia Koe",
    role: "Lead Designer",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1774017708/member-six.png",
    categories: ["Design"],
  },
  {
    id: "member-6",
    name: "Amara Okonkwo",
    role: "CTO",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-three.png",
    categories: ["Security", "Product"],
  },
  {
    id: "member-7",
    name: "Sofia Weber",
    role: "Sales Director",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1774017708/member-six.png",
    categories: ["Sales", "GTM"],
  },
  {
    id: "member-8",
    name: "Elena Rodriguez",
    role: "Head of Operations",
    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png",
    categories: ["Operations"],
  },
];

const categories = ["All", "Product", "Design", "Sales", "Security", "GTM", "Operations"];

export default function Team6() {
  const [activeCategory, setActiveCategory] = useState<string>("All");

  const filteredMembers =
    activeCategory === "All"
      ? teamMembers
      : teamMembers.filter((member) => member.categories.includes(activeCategory));

  return (
    <section className="relative w-full py-18 md:py-20 bg-white dark:bg-background overflow-hidden flex flex-col items-center justify-center">
      <div className="w-full max-w-6xl px-4 md:px-8 z-10 flex flex-col items-start text-left">

        {/* Title */}
        <h2 className="text-4xl md:text-5xl lg:text-6xl font-sans font-semibold tracking-tight text-neutral-900 dark:text-white capitalize max-w-4xl mb-10 leading-tight">
          Meet The Incredible Team Behind Solace UI
        </h2>

        {/* Category Filters */}
        <div className="flex flex-wrap gap-2 md:gap-3 mb-14 w-full">
          {categories.map((category) => (
            <button
              key={category}
              onClick={() => setActiveCategory(category)}
              className={cn(
                "px-4 py-1 text-sm md:text-base font-sans  border transition-all duration-300 cursor-pointer focus:outline-none",
                activeCategory === category
                  ? "border-black dark:border-white text-black dark:text-white bg-neutral-50 dark:bg-zinc-900 font-medium shadow-sm"
                  : "border-neutral-200/60 dark:border-zinc-800/80 text-neutral-400 dark:text-zinc-500 hover:text-neutral-900 dark:hover:text-zinc-200 hover:border-neutral-300 dark:hover:border-zinc-700"
              )}
            >
              {category}
            </button>
          ))}
        </div>

        {/* Grid Container */}
        <div className="relative w-full overflow-visible">

          <motion.div
            layout
            className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-x-1 gap-y-1 w-full"
          >
            <AnimatePresence mode="popLayout">
              {filteredMembers.map((member) => (
                <motion.div
                  layout
                  initial={{ opacity: 0, scale: 0.95 }}
                  animate={{ opacity: 1, scale: 1 }}
                  exit={{ opacity: 0, scale: 0.95 }}
                  transition={{ duration: 0.4, ease: "easeInOut" }}
                  key={member.id}
                  className="group relative aspect-[3/4.2] w-full rounded-md overflow-hidden bg-neutral-50 dark:bg-zinc-900 border border-neutral-200/50 dark:border-zinc-800/50 shadow-sm hover:shadow-xl transition-all duration-500 cursor-pointer"
                >
                  {/* Image */}
                  <Image
                    src={member.image}
                    alt={member.name}
                    fill
                    className="object-cover object-top transition-all duration-500 grayscale contrast-[1.05] group-hover:grayscale-0 group-hover:scale-[1.02]"
                    sizes="(max-width: 768px) 100vw, (max-width: 1024px) 50vw, 33vw"
                  />

                  {/* Gradient Overlay for Text Readability */}
                  <div className="absolute inset-0 bg-gradient-to-t from-black/70 via-black/25 to-transparent z-10" />

                  {/* Name overlay at the bottom left */}
                  <div className="absolute bottom-6 left-6 z-20 flex flex-col text-left">
                    <h3 className="font-sans font-bold text-white text-2xl md:text-3xl leading-tight">
                      {member.name}
                    </h3>
                    <p className="font-sans text-white/70 text-sm md:text-base mt-2 font-light">
                      {member.role}
                    </p>
                  </div>
                </motion.div>
              ))}
            </AnimatePresence>
          </motion.div>
        </div>
      </div>

      {/* Bottom fade overlay spanning end-to-end */}
      <div className="absolute bottom-0 left-0 right-0 h-44 bg-gradient-to-t from-white via-white/80 to-transparent dark:from-background dark:via-background/80 pointer-events-none z-20" />
    </section>
  );
}
```
---

## Testimonial Section 1 <a name="testimonial-section-1"></a>

Solace UI testimonial section block variation 1.

- **Registry Key**: `testimonial-section-1`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/testimonial-section-1.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/testimonial-section/one/index.tsx`
```tsx
"use client";



import React from "react";

import Image from "next/image";

import { cn } from "@/lib/utils";

import { motion, Variants } from "motion/react";



interface Testimonial {

  name: string;

  role: string;

  image: string;

  quote: string;

  className: string;

  imageBorderColor: string;

}



const testimonials: Testimonial[] = [

  {

    name: "Sarah Chen",

    role: "CEO of DataFlow Technologies",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png",

    quote:

      "SolaceUI transformed our design workflow. What used to take weeks now takes days, and our product consistency has never been better.",

    className:

      "lg:col-span-1 lg:row-span-2 bg-(--color-primary) text-white border-transparent",

    imageBorderColor: "border-black",

  },

  {

    name: "Marcus Rodriguez",

    role: "CEO of Quantum Labs",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-one.png",

    quote:

      "We've tried every UI framework out there. SolaceUI is the first that actually delivers. Our time-to-market improved by 40%",

    className:

      "lg:col-span-1 bg-white dark:bg-black border border-neutral-300 dark:border-neutral-600 text-black dark:text-white",

    imageBorderColor: "border-(--color-primary)",

  },

  {

    name: "Oilivia Koe",

    role: "CEO of Nexus Digital",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1774017708/member-six.png",

    quote:

      "SolaceUI has become non-negotiable in our tech stack. It's elegant, powerful, and scaled beautifully with us from seed to Series B",

    className:

      "lg:col-span-1 lg:row-span-2 bg-black dark:bg-white text-white dark:text-black border-transparent ",

    imageBorderColor: "border-(--color-primary)",

  },

  {

    name: "David Kim",

    role: "CEO of Streamline Software",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-two.png",

    quote:

      "Perfect balance between flexibility and structure. Our design system went from scattered mess to cohesive asset. ROI was evident in Q1.",

    className:

      "lg:col-span-1 bg-white dark:bg-black border border-neutral-300 dark:border-neutral-600 text-black dark:text-white",

    imageBorderColor: "border-(--color-primary)",

  },

  {

    name: "James Mitchell",

    role: "CEO of Visionary Apps",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-four.png",

    quote:

      "The accessibility features are built-in, not bolted-on. It's rare to find a tool that makes both designers and developers genuinely happy",

    className:

      "lg:col-span-1 bg-black  dark:bg-white text-white dark:text-black border-neutral-800",

    imageBorderColor: "border-(--color-primary)",

  },

  {

    name: "Amara Okonkwo",

    role: "CEO of Horizon Platforms",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-three.png",

    quote:

      "We migrated our entire product in under three months. The performance improvements alone justified the switch. Highly recommend",

    className:

      "lg:col-span-2 bg-(--color-primary) text-white border-transparent",

    imageBorderColor: "border-black",

  },

];



export default function Testimonial1() {

  const containerVariants = {

    hidden: { opacity: 0 },

    visible: {

      opacity: 1,

      transition: {

        staggerChildren: 0.1,

      },

    },

  };



  const itemVariants: Variants = {

    hidden: { opacity: 0, y: 20, filter: "blur(4px)" },

    visible: {

      opacity: 1,

      y: 0,

      filter: "blur(0px)",

      transition: {

        duration: 0.4,

        ease: "easeOut" as const,

      },

    },

  };



  return (

    <div className="w-full py-10 [--color-primary:#003AF9] bg-white dark:bg-black text-neutral-900 dark:text-neutral-100">

      <div className="max-w-7xl mx-auto px-4 md:px-8">

        <h2 className="text-2xl md:text-3xl font-bold text-center mb-10 tracking-tight">

          Trusted By The Best People

        </h2>

        <motion.div

          className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 auto-rows-auto"

          variants={containerVariants}

          initial="hidden"

          whileInView="visible"

          viewport={{ once: true, margin: "-100px" }}

        >

          {testimonials.map((testimonial, index) => (

            <motion.div

              key={index}

              variants={itemVariants}

              whileHover={{ scale: 1.02 }}

              className={cn(

                "p-8 rounded-lg flex flex-col justify-between",

                testimonial.className,

              )}

            >

              <p

                className={cn(

                  "font-medium leading-relaxed mb-8",

                  testimonial.className.includes("lg:row-span-2") ||

                    testimonial.className.includes("col-span-2")

                    ? "text-xl md:text-2xl lg:text-3xl"

                    : "text-base",

                )}

              >

                {testimonial.quote}

              </p>

              <div className="flex items-center gap-4">

                <div

                  className={cn(

                    "relative w-12 h-12 rounded-full overflow-hidden border-[1.5px]",

                    testimonial.imageBorderColor,

                  )}

                >

                  <Image

                    src={testimonial.image}

                    alt={testimonial.name}

                    fill

                    className="object-cover object-top"

                    sizes="48px"

                    loading="lazy"

                  />

                </div>

                <div>

                  <h4 className="font-bold text-base">{testimonial.name}</h4>

                  <p className="text-sm opacity-80">{testimonial.role}</p>

                </div>

              </div>

            </motion.div>

          ))}

        </motion.div>

      </div>

    </div>

  );

}
```
---

## Testimonial Section 2 <a name="testimonial-section-2"></a>

Solace UI testimonial section block variation 2.

- **Registry Key**: `testimonial-section-2`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/testimonial-section-2.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/testimonial-section/two/index.tsx`
```tsx
"use client";



import React, { useState } from "react";

import Image from "next/image";

import { motion, AnimatePresence } from "motion/react";

import { X } from "lucide-react";



interface Testimonial {

  id: string;

  name: string;

  role: string;

  image: string;

  quote: string;

}



const testimonials: Testimonial[] = [

  {

    id: "1",

    name: "Sarah Chen",

    role: "CEO of DataFlow",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png",

    quote:

      "SolaceUI transformed our design workflow. What used to take weeks now takes days.",

  },

  {

    id: "2",

    name: "Marcus Rodriguez",

    role: "Product Lead",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-one.png",

    quote:

      "The best investment we've made for our frontend architecture in years.",

  },

  {

    id: "3",

    name: "Olivia Koe",

    role: "Design Director",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1774017708/member-six.png",

    quote:

      "Simply beautiful components that are easy to customize and integrate.",

  },

  {

    id: "4",

    name: "David Kim",

    role: "Founder",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-two.png",

    quote: "Our development velocity has doubled since adopting SolaceUI.",

  },

  {

    id: "5",

    name: "Amara Okonkwo",

    role: "CTO",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-three.png",

    quote:

      "Accessibility and performance out of the box. Truly impressive work.",

  },

  {

    id: "6",

    name: "James Mitchell",

    role: "Frontend Dev",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-four.png",

    quote: "The documentation is clear and the components just work. Love it.",

  },

  {

    id: "7",

    name: "Elena Rodriguez",

    role: "Product Manager",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png",

    quote:

      "It looks premium and feels premium. Our users noticed the difference immediately.",

  },

  {

    id: "8",

    name: "Michael Chang",

    role: "Tech Lead",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-one.png",

    quote:

      "Clean abstractions and great TypeScript support. A joy to work with.",

  },

  {

    id: "9",

    name: "Sofia Weber",

    role: "Designer",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1774017708/member-six.png",

    quote:

      "Finally a library that respects design constraints while offering flexibility.",

  },

];



export default function Testimonial2() {

  const [selected, setSelected] = useState<Testimonial | null>(null);



  // Split testimonials into 3 rows for visual variance

  const row1 = testimonials.slice(0, 3);

  const row2 = testimonials.slice(3, 6);

  const row3 = testimonials.slice(6, 9);



  return (

    <div className="relative w-full py-20 overflow-hidden [--color-primary:#003AF9] bg-white dark:bg-background text-neutral-900 dark:text-white">

      <div className="max-w-7xl mx-auto px-4 text-center mb-12">

        <h2 className="text-3xl md:text-5xl font-bold tracking-tight">

          Trusted By The Best People

        </h2>

      </div>



      {/* Main Container acting as the viewport for background and fades */}

      <div className="relative w-full">

        {/* Shaded Background - Matches the height of this container exactly */}

        <div className="absolute inset-0 z-0 opacity-10 bg-[repeating-linear-gradient(315deg,currentColor_0,currentColor_1px,transparent_0,transparent_50%)] bg-[length:10px_10px] border-y border-black dark:border-white pointer-events-none"></div>



        {/* Fades - Match the height of this container exactly */}

        <div className="absolute left-0 top-0 bottom-0 w-40 bg-gradient-to-r from-white dark:from-black to-transparent z-20 pointer-events-none"></div>

        <div className="absolute right-0 top-0 bottom-0 w-40 bg-gradient-to-l from-white dark:from-black to-transparent z-20 pointer-events-none"></div>



        {/* Content Rows */}

        <div className="relative z-10 flex flex-col gap-8 py-12 items-center justify-center overflow-hidden">

          {[row1, row2, row3].map((row, rowIndex) => (

            <motion.div

              key={rowIndex}

              className="flex items-center gap-6 min-w-max"

              animate={{

                x: rowIndex % 2 === 0 ? ["0%", "-25%"] : ["-25%", "0%"],

              }}

              transition={{

                duration: 40,

                repeat: Infinity,

                ease: "linear",

              }}

            >

              {[...row, ...row, ...row, ...row].map((testimonial, i) => (

                <Capsule

                  key={`${testimonial.id}-${i}`}

                  testimonial={testimonial}

                  onClick={() => setSelected(testimonial)}

                />

              ))}

            </motion.div>

          ))}

        </div>

      </div>



      {/* Modal */}

      <AnimatePresence>

        {selected && (

          <div className="fixed inset-0 z-50 flex items-center justify-center p-4">

            {/* Backdrop */}

            <motion.div

              initial={{ opacity: 0 }}

              animate={{ opacity: 1 }}

              exit={{ opacity: 0 }}

              onClick={() => setSelected(null)}

              className="absolute inset-0 bg-white/10 dark:bg-black/40 backdrop-blur-md"

            />



            {/* Modal Card */}

            <motion.div

              initial={{ opacity: 0, scale: 0.9, y: 20 }}

              animate={{ opacity: 1, scale: 1, y: 0 }}

              exit={{

                opacity: 0,

                scale: 0.95,

                y: 10,

                transition: { duration: 0.15 },

              }}

              transition={{ duration: 0.2, ease: "easeOut" }}

              className="relative w-full max-w-lg bg-black dark:bg-white text-white dark:text-black p-8 md:p-12 rounded-2xl border-2 border-(--color-primary) shadow-2xl z-50"

            >

              <button

                onClick={() => setSelected(null)}

                className="absolute top-4 right-4 p-2 text-neutral-500 hover:text-white dark:hover:text-black transition-colors"

              >

                <X size={20} />

              </button>



              <div className="flex flex-col items-center text-center">

                <p className="text-xl md:text-2xl font-medium leading-relaxed mb-8">

                  &ldquo;{selected.quote}&rdquo;

                </p>



                <div className="flex items-center gap-4">

                  <div className="relative w-12 h-12 rounded-full overflow-hidden border-2 border-(--color-primary)">

                    <Image

                      src={selected.image}

                      alt={selected.name}

                      fill

                      className="object-cover object-top"

                    />

                  </div>

                  <div className="text-left">

                    <h4 className="font-bold text-base text-white dark:text-black">

                      {selected.name}

                    </h4>

                    <p className="text-sm text-neutral-400 dark:text-neutral-600">

                      {selected.role}

                    </p>

                  </div>

                </div>

              </div>

            </motion.div>

          </div>

        )}

      </AnimatePresence>

    </div>

  );

}



function Capsule({

  testimonial,

  onClick,

}: {

  testimonial: Testimonial;

  onClick: () => void;

}) {

  return (

    <motion.div

      whileHover={{ scale: 1.05 }}

      whileTap={{ scale: 0.95 }}

      onClick={onClick}

      className="group flex items-center gap-4 p-2 pr-8 rounded-full bg-white dark:bg-black/90 border border-neutral-300 hover:border-(--color-primary) hover:border-dashed dark:hover:border-(--color-primary) dark:border-neutral-800 cursor-pointer transition-all shadow-sm hover:shadow-md group"

    >

      <div className="relative w-14 h-14 rounded-full overflow-hidden border border-black group-hover:border-(--color-primary) dark:group-hover:border-(--color-primary)  dark:border-white  transition-colors">

        <Image

          src={testimonial.image}

          alt={testimonial.name}

          fill

          className="object-cover object-top"

          sizes="48px"

        />

      </div>

      <div className="flex flex-col items-start leading-tight">

        <span className="text-sm font-bold text-neutral-900 dark:text-white">

          {testimonial.name}

        </span>

        <span className="text-xs text-neutral-500 dark:text-neutral-400">

          {testimonial.role}

        </span>

      </div>

    </motion.div>

  );

}
```
---

## Testimonial Section 3 <a name="testimonial-section-3"></a>

Solace UI testimonial section block variation 3.

- **Registry Key**: `testimonial-section-3`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/testimonial-section-3.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### Component Source Code
##### File: `src/registry/blocks/testimonial-section/three/index.tsx`
```tsx
"use client";



import React, { useState, useEffect, useCallback, useMemo } from "react";

import { ArrowLeft, ArrowRight } from "lucide-react";

import { motion } from "motion/react";

import Image from "next/image";

import { cn } from "@/lib/utils";



const testimonials = [

  {

    id: "1",

    name: "Marcus Rodriguez",

    role: "Product Lead",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-one.png",

    quote:

      "The best investment we've made for our frontend architecture in years.",

  },

  {

    id: "2",

    name: "Sarah Chen",

    role: "CEO of DataFlow",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png",

    quote:

      "SolaceUI transformed our design workflow. What used to take weeks now takes days.",

  },



  {

    id: "3",

    name: "Olivia Koe",

    role: "Design Director",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1774017708/member-six.png",

    quote:

      "Simply beautiful components that are easy to customize and integrate.",

  },

  {

    id: "4",

    name: "David Kim",

    role: "Founder",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-two.png",

    quote: "Our development velocity has doubled since adopting SolaceUI.",

  },

  {

    id: "5",

    name: "Amara Okonkwo",

    role: "CTO",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-three.png",

    quote:

      "Accessibility and performance out of the box. Truly impressive work.",

  },

];



export default function Testimonial3() {

  const [currentIndex, setCurrentIndex] = useState(1); // Start with index 1 so 0 is left, 1 is center, 2 is right (conceptually)



  const handleNext = useCallback(() => {

    setCurrentIndex((prev) => (prev + 1) % testimonials.length);

  }, []);



  const handlePrev = useCallback(() => {

    setCurrentIndex(

      (prev) => (prev - 1 + testimonials.length) % testimonials.length,

    );

  }, []);



  useEffect(() => {

    const handleKeyDown = (e: KeyboardEvent) => {

      if (e.key === "ArrowLeft") {

        handlePrev();

      } else if (e.key === "ArrowRight") {

        handleNext();

      }

    };



    window.addEventListener("keydown", handleKeyDown);

    return () => window.removeEventListener("keydown", handleKeyDown);

  }, [handleNext, handlePrev]);



  const visibleItems = useMemo(() => {

    const total = testimonials.length;

    const leftIndex = (currentIndex - 1 + total) % total;

    const centerIndex = currentIndex;

    const rightIndex = (currentIndex + 1) % total;



    return [

      { ...testimonials[leftIndex], position: "left" },

      { ...testimonials[centerIndex], position: "center" },

      { ...testimonials[rightIndex], position: "right" },

    ];

  }, [currentIndex]);



  return (

    <section

      className="w-full py-20 bg-background flex flex-col items-center justify-center overflow-hidden"

      style={{ "--color-primary": "#003AF9" } as React.CSSProperties}

    >

      <div className="text-center mb-12 space-y-2">

        <h2 className="text-4xl md:text-5xl font-bold tracking-tight text-neutral-900 dark:text-white">

          Trusted By The

          <br />

          Best People

        </h2>

      </div>



      <div className="relative w-full max-w-7xl px-4 flex items-stretch justify-center">

        <div className="flex flex-row items-stretch justify-center w-full">

          {visibleItems.map((item, index) => {

            const isCenter = item.position === "center";



            return (

              <React.Fragment key={item.id}>

                {/* Render Gap before center and after center (so between left-center and center-right) */}

                {index > 0 && (

                  <div className="hidden md:block w-[10px] sm:w-[20px] relative shrink-0">

                    <div className="absolute inset-0 bg-[repeating-linear-gradient(315deg,currentColor_0,currentColor_1px,transparent_0,transparent_50%)] bg-[length:10px_10px] text-neutral-300 dark:text-neutral-700 opacity-50 h-full w-full" />

                  </div>

                )}



                <motion.div

                  layout

                  initial={{ opacity: 0, scale: 0.9 }}

                  animate={{ opacity: 1, scale: 1 }}

                  exit={{ opacity: 0, scale: 0.9 }}

                  transition={{ duration: 0.5, type: "spring" }}

                  style={{ willChange: "transform, opacity" }}

                  className={cn(

                    "relative flex flex-col justify-between border p-8 w-full md:w-[300px] shrink-0 rounded-none overflow-hidden",

                    isCenter

                      ? "bg-(--color-primary) border-(--color-primary) text-white z-20"

                      : "hidden md:flex bg-white dark:bg-neutral-950 border-neutral-300 dark:border-neutral-800 text-neutral-600 dark:text-neutral-300 z-0",

                  )}

                >

                  {/* Overlay for side divs */}

                  {!isCenter && item.position === "left" && (

                    <div className="absolute inset-0 bg-gradient-to-r from-white/90 via-white/40 to-transparent dark:from-black/90 dark:via-black/40 dark:to-transparent z-10 pointer-events-none transition-all duration-300" />

                  )}

                  {!isCenter && item.position === "right" && (

                    <div className="absolute inset-0 bg-gradient-to-l from-white/90 via-white/40 to-transparent dark:from-black/90 dark:via-black/40 dark:to-transparent z-10 pointer-events-none transition-all duration-300" />

                  )}



                  <div

                    className={cn(

                      "text-xl md:text-[1.6rem] font-medium mb-8",

                      !isCenter && "blur-[1px] opacity-70",

                    )}

                  >

                    "{item.quote}"

                  </div>



                  <div className="flex items-center gap-4 mt-auto">

                    <div

                      className={cn(

                        "relative w-12 h-12 overflow-hidden",

                        isCenter

                          ? "border-2 border-black"

                          : "border-2 border-(--color-primary)",

                      )}

                    >

                      <Image

                        src={item.image}

                        alt={item.name}

                        width={48}

                        height={48}

                        loading="lazy"

                        sizes="48px"

                        className="object-cover object-top"

                      />

                    </div>

                    <div className="flex flex-col text-left">

                      <span

                        className={cn(

                          "font-bold text-lg",

                          isCenter

                            ? "text-white"

                            : "text-neutral-900 dark:text-white",

                        )}

                      >

                        {item.name}

                      </span>

                      <span

                        className={cn(

                          "text-sm",

                          isCenter ? "text-blue-100" : "text-neutral-500",

                        )}

                      >

                        {item.role}

                      </span>

                    </div>

                  </div>

                </motion.div>

              </React.Fragment>

            );

          })}

        </div>

      </div>



      {/* Navigation Actions */}

      <div className="flex items-center gap-6 mt-12 bg-transparent">

        <button

          onClick={handlePrev}

          className="group p-2 rounded-full hover:bg-neutral-100 dark:hover:bg-neutral-800 transition-colors"

          aria-label="Previous testimonial"

        >

          <ArrowLeft className="w-6 h-6 text-neutral-400 dark:text-neutral-400 group-hover:text-(--color-primary) transition-colors" />

        </button>

        <button

          onClick={handleNext}

          className="group p-2 rounded-full hover:bg-neutral-100 dark:hover:bg-neutral-800 transition-colors"

          aria-label="Next testimonial"

        >

          <ArrowRight className="w-6 h-6 text-neutral-400 dark:text-neutral-400 group-hover:text-(--color-primary) transition-colors" />

        </button>

      </div>

    </section>

  );

}
```
---

## Testimonial Section 4 <a name="testimonial-section-4"></a>

Solace UI testimonial section block variation 4.

- **Registry Key**: `testimonial-section-4`
#### Installation
```bash
npx shadcn@latest add https://www.solaceui.com/r/testimonial-section-4.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### Component Source Code
##### File: `src/registry/blocks/testimonial-section/four/index.tsx`
```tsx
"use client";



import React, { useState, useEffect, useCallback, useMemo } from "react";

import { ArrowLeft, ArrowRight } from "lucide-react";

import { motion, AnimatePresence } from "motion/react";

import Image from "next/image";

import { cn } from "@/lib/utils";





const NvidiaLogo = ({ className }: { className?: string }) => (

  <svg

    className={className}

    viewBox="0 0 85 16"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

  >

    <g clipPath="url(#clip0_11_36)">

      <mask

        id="mask0_11_36"

        style={{ maskType: "luminance" }}

        maskUnits="userSpaceOnUse"

        x="0"

        y="0"

        width="85"

        height="16"

      >

        <path d="M84.2105 0H0V16H84.2105V0Z" fill="white" />

      </mask>

      <g mask="url(#mask0_11_36)">

        <path

          d="M51.448 3.0305V13.5301H54.411V3.0305H51.448ZM28.1353 3.0127V13.5212H31.125V5.36177L33.4562 5.37067C34.2215 5.37067 34.7554 5.55752 35.1202 5.94904C35.5918 6.44732 35.7786 7.25704 35.7786 8.72521V13.5212H38.6794V7.71973C38.6794 3.57327 36.0367 3.0127 33.4562 3.0127H28.1353ZM56.2262 3.0305V13.5301H61.0311C63.5937 13.5301 64.4301 13.103 65.3288 12.1509C65.9695 11.4836 66.3788 10.0065 66.3788 8.39598C66.3788 6.91892 66.0318 5.60201 65.4178 4.7834C64.3323 3.31523 62.7484 3.0305 60.3815 3.0305H56.2262ZM59.1625 5.30838H60.4349C62.2857 5.30838 63.4781 6.13589 63.4781 8.2892C63.4781 10.4425 62.2857 11.2789 60.4349 11.2789H59.1625V5.30838ZM47.1769 3.0305L44.7033 11.3501L42.3364 3.0305H39.1332L42.5144 13.5301H46.7854L50.2023 3.0305H47.1769ZM67.7669 13.5301H70.7299V3.0305H67.7669V13.5301ZM76.0776 3.0305L71.94 13.5212H74.8586L75.517 11.6615H80.4109L81.0338 13.5123H84.2104L80.0372 3.0305H76.0776ZM77.9996 4.94356L79.797 9.85525H76.1488L77.9996 4.94356Z"

          fill="currentColor"

        />

        <path

          d="M9.01366 4.77448V3.333C9.15603 3.32411 9.29839 3.31521 9.44076 3.31521C13.3915 3.19064 15.9808 6.71424 15.9808 6.71424C15.9808 6.71424 13.1868 10.5938 10.1882 10.5938C9.78778 10.5938 9.39627 10.5315 9.02256 10.4069V6.02909C10.5619 6.21595 10.8733 6.8922 11.7898 8.43155L13.8453 6.70534C13.8453 6.70534 12.3415 4.73889 9.81448 4.73889C9.54754 4.72999 9.2806 4.74778 9.01366 4.77448ZM9.01366 0.00515747V2.15847L9.44076 2.13178C14.9308 1.94492 18.5167 6.63416 18.5167 6.63416C18.5167 6.63416 14.4058 11.6348 10.1259 11.6348C9.75219 11.6348 9.38737 11.5992 9.02256 11.5369V12.8716C9.32509 12.9072 9.63652 12.9339 9.93905 12.9339C13.9253 12.9339 16.8083 10.8963 19.6023 8.49384C20.065 8.86755 21.9602 9.76625 22.3517 10.1578C19.7001 12.3823 13.516 14.1707 10.0102 14.1707C9.67211 14.1707 9.35178 14.153 9.03145 14.1174V15.9948H24.1758V0.00515747H9.01366ZM9.01366 10.4069V11.5458C5.32989 10.8874 4.30662 7.05236 4.30662 7.05236C4.30662 7.05236 6.07732 5.0948 9.01366 4.77448V6.0202H9.00476C7.46541 5.83334 6.25528 7.27481 6.25528 7.27481C6.25528 7.27481 6.94043 9.70396 9.01366 10.4069ZM2.47364 6.8922C2.47364 6.8922 4.65365 3.67113 9.02256 3.333V2.15847C4.18205 2.54998 0 6.64305 0 6.64305C0 6.64305 2.36686 13.4945 9.01366 14.1174V12.8716C4.13756 12.2666 2.47364 6.8922 2.47364 6.8922Z"

          fill="currentColor"

        />

      </g>

    </g>

    <defs>

      <clipPath id="clip0_11_36">

        <rect width="85" height="16" fill="white" />

      </clipPath>

    </defs>

  </svg>

);



const ColumnLogo = ({ className }: { className?: string }) => (

  <svg

    viewBox="0 0 73 16"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    className={className}

  >

    <g clipPath="url(#clip0_12_44)">

      <mask

        id="mask0_12_44"

        style={{ maskType: "luminance" }}

        maskUnits="userSpaceOnUse"

        x="0"

        y="0"

        width="73"

        height="16"

      >

        <path d="M72.8389 0H0V16H72.8389V0Z" fill="currentColor" />

      </mask>

      <g mask="url(#mask0_12_44)">

        <path

          d="M8.46958 12.0453C8.20458 12.4776 7.84015 12.8243 7.38648 13.0756C6.93235 13.3271 6.41831 13.4546 5.85858 13.4546C5.28463 13.4546 4.75307 13.3129 4.27855 13.0335C3.80349 12.7542 3.42826 12.3648 3.16322 11.876C2.8972 11.3862 2.7623 10.8245 2.7623 10.2065C2.7623 9.60343 2.90075 9.04553 3.17384 8.54824C3.44689 8.0514 3.82265 7.65431 4.29073 7.36789C4.75845 7.08182 5.29325 6.93674 5.88027 6.93674C6.38275 6.93674 6.85363 7.04991 7.27984 7.27321C7.7065 7.49714 8.07195 7.80943 8.36597 8.20128L8.42367 8.27809L10.4527 6.57565L10.3955 6.50604C9.85453 5.84821 9.18386 5.32577 8.40206 4.95312C7.61991 4.58029 6.76416 4.3913 5.85858 4.3913C4.79219 4.3913 3.7994 4.6507 2.90782 5.16229C2.01589 5.67416 1.30068 6.38559 0.782104 7.27695C0.263131 8.16857 0 9.15416 0 10.2065C0 11.2591 0.263176 12.2411 0.782238 13.1255C1.30072 14.0097 2.01589 14.7174 2.90782 15.229C3.7994 15.7406 4.79219 16 5.85858 16C6.83679 16 7.75544 15.7814 8.58901 15.3502C9.42147 14.9196 10.1179 14.3231 10.659 13.5773L10.7133 13.5026L8.52021 11.9627L8.46958 12.0453Z"

          fill="currentColor"

        />

        <path

          d="M20.3982 5.16212C19.4993 4.65061 18.4957 4.3913 17.4152 4.3913C16.3488 4.3913 15.356 4.6507 14.4645 5.16229C13.5725 5.67416 12.8573 6.38559 12.3387 7.27695C11.8198 8.16857 11.5566 9.15416 11.5566 10.2065C11.5566 11.2591 11.8198 12.2411 12.3389 13.1255C12.8574 14.0097 13.5725 14.7174 14.4645 15.229C15.356 15.7406 16.3488 16 17.4152 16C18.4957 16 19.4993 15.7407 20.3982 15.2292C21.297 14.7178 22.0159 14.01 22.5349 13.1255C23.054 12.2406 23.3171 11.2585 23.3171 10.2065C23.3171 9.15478 23.054 8.1691 22.5351 7.27695C22.0159 6.38532 21.297 5.6738 20.3982 5.16212ZM20.1436 11.8752C19.8712 12.3642 19.4921 12.7539 19.0169 13.0335C18.5421 13.3129 18.0032 13.4546 17.4152 13.4546C16.8413 13.4546 16.3097 13.3129 15.8352 13.0335C15.3601 12.7542 14.9849 12.3648 14.7199 11.876C14.4538 11.3862 14.3189 10.8245 14.3189 10.2065C14.3189 9.60343 14.4574 9.04553 14.7305 8.54824C15.0035 8.0514 15.3793 7.65431 15.8474 7.36789C16.3151 7.08182 16.8499 6.93674 17.4369 6.93674C18.0236 6.93674 18.5584 7.08182 19.0264 7.3678C19.4947 7.65457 19.8705 8.05176 20.1433 8.54824C20.4164 9.04553 20.5549 9.60343 20.5549 10.2065C20.5549 10.824 20.4165 11.3853 20.1436 11.8752Z"

          fill="currentColor"

        />

        <path

          d="M37.9218 10.4884C37.9218 11.1201 37.7979 11.6671 37.5535 12.1143C37.3099 12.5597 36.9862 12.8978 36.5914 13.1192C36.1953 13.3418 35.7543 13.4546 35.2808 13.4546C34.8075 13.4546 34.3925 13.353 34.0472 13.1524C33.7023 12.9527 33.4258 12.6619 33.2253 12.2883C33.0239 11.9135 32.9218 11.4684 32.9218 10.9654V4.65149H30.1812V11.5074C30.1812 12.3577 30.3673 13.131 30.7342 13.8059C31.1019 14.4829 31.6277 15.0237 32.297 15.413C32.966 15.8025 33.7437 16 34.6087 16C35.5172 16 36.3574 15.7694 37.1061 15.3146C37.4519 15.1044 37.7692 14.8491 38.0519 14.5538V15.7398H40.6624V4.65149H37.9218V10.4884Z"

          fill="currentColor"

        />

        <path

          d="M57.7883 4.94486C57.127 4.57754 56.3826 4.3913 55.576 4.3913C54.7397 4.3913 53.9657 4.5923 53.2756 4.98877C52.7464 5.2927 52.2965 5.689 51.9367 6.16788C51.6119 5.66242 51.1895 5.24835 50.6799 4.93597C50.0895 4.57452 49.4316 4.3913 48.7243 4.3913C47.9583 4.3913 47.2558 4.59692 46.6362 5.00246C46.2824 5.2343 45.9661 5.52134 45.6931 5.8579V4.6515H43.1909V15.7398H45.9532V9.25247C45.9532 8.82151 46.0482 8.4237 46.2353 8.06998C46.4219 7.71778 46.6808 7.43767 47.005 7.23756C47.3285 7.03799 47.6881 6.93674 48.0737 6.93674C48.4731 6.93674 48.8362 7.03782 49.1528 7.23712C49.4703 7.43749 49.7258 7.71769 49.9122 8.06998C50.0995 8.42361 50.1944 8.82142 50.1944 9.25247V15.7398H52.9349V9.25247C52.9349 8.82151 53.0333 8.42405 53.2274 8.07114C53.421 7.71822 53.6804 7.43758 53.9983 7.23712C54.3146 7.03782 54.6776 6.93674 55.0773 6.93674C55.477 6.93674 55.8438 7.03799 56.1677 7.23756C56.4914 7.43749 56.7465 7.71716 56.9259 8.06865C57.1063 8.42317 57.1978 8.82151 57.1978 9.25247V15.7398H59.9602V8.55864C59.9602 7.79396 59.7623 7.08493 59.372 6.45093C58.9821 5.81879 58.4494 5.31208 57.7883 4.94486Z"

          fill="currentColor"

        />

        <path

          d="M72.2858 6.60712C71.9178 5.93062 71.3884 5.38622 70.7123 4.98921C70.0363 4.59247 69.2621 4.3913 68.4112 4.3913C67.4877 4.3913 66.64 4.62563 65.8917 5.08789C65.5549 5.2959 65.245 5.54526 64.968 5.83097V4.6515H62.3359V15.7398H65.0982V9.90292C65.0982 9.28465 65.2186 8.74461 65.4561 8.29773C65.6925 7.85272 66.0162 7.51119 66.4182 7.28255C66.8213 7.05311 67.2657 6.93674 67.7391 6.93674C68.1975 6.93674 68.6088 7.0419 68.9614 7.24948C69.3136 7.4566 69.5939 7.75111 69.7946 8.12465C69.9961 8.50006 70.0982 8.93787 70.0982 9.42591V15.7398H72.8388V8.90551C72.8388 8.05629 72.6527 7.28299 72.2858 6.60712Z"

          fill="currentColor"

        />

        <path

          d="M23.7266 0V1.84061H24.1911C24.6972 1.84061 25.1075 2.25088 25.1075 2.75697V15.7398H27.8702V0H23.7266Z"

          fill="currentColor"

        />

      </g>

    </g>

    <defs>

      <clipPath id="clip0_12_44">

        <rect width="73" height="16" fill="white" />

      </clipPath>

    </defs>

  </svg>

);



const GithubLogo = ({ className }: { className?: string }) => (

  <svg

    className={className}

    width="60"

    height="16"

    viewBox="0 0 60 16"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

  >

    <g clipPath="url(#clip0_12_56)">

      <mask

        id="mask0_12_56"

        style={{ maskType: "luminance" }}

        maskUnits="userSpaceOnUse"

        x="0"

        y="0"

        width="60"

        height="16"

      >

        <path d="M59.1716 0H0V16H59.1716V0Z" fill="currentColor" />

      </mask>

      <g mask="url(#mask0_12_56)">

        <path

          d="M11.3991 6.84818H6.42744C6.36582 6.84822 6.30674 6.87271 6.26318 6.91627C6.21961 6.95984 6.19512 7.01892 6.19509 7.08053V9.5113C6.19512 9.57295 6.2196 9.63206 6.26316 9.67569C6.30671 9.71931 6.36579 9.74389 6.42744 9.74402H8.3669V12.7639C8.3669 12.7639 7.93141 12.9125 6.72742 12.9125C5.30697 12.9125 3.32267 12.3933 3.32267 8.03C3.32267 3.6658 5.3889 3.09147 7.32873 3.09147C9.00789 3.09147 9.73129 3.38726 10.1915 3.52955C10.3362 3.5739 10.47 3.43001 10.47 3.30151L11.0246 0.95304C11.0246 0.893044 11.0043 0.820605 10.9358 0.771451C10.7489 0.638153 9.60846 0 6.72742 0C3.40841 0 0.00390625 1.41219 0.00390625 8.20013C0.00390625 14.9886 3.90181 16 7.18644 16C9.9061 16 11.5559 14.8378 11.5559 14.8378C11.6239 14.8001 11.6313 14.7051 11.6313 14.6615V7.08041C11.6313 6.95229 11.5274 6.84818 11.3991 6.84818ZM37.0212 0.813337C37.0214 0.782828 37.0157 0.752569 37.0042 0.724297C36.9927 0.696026 36.9758 0.6703 36.9543 0.648595C36.9329 0.626891 36.9074 0.609637 36.8792 0.597824C36.8511 0.58601 36.8209 0.579871 36.7904 0.579759H33.9909C33.9604 0.579872 33.9301 0.586003 33.9019 0.597803C33.8737 0.609603 33.8481 0.626838 33.8266 0.648528C33.8051 0.670218 33.788 0.695937 33.7765 0.724215C33.7649 0.752492 33.759 0.782776 33.7591 0.813337L33.7598 6.22321H29.3964V0.813337C29.3966 0.782817 29.3908 0.752551 29.3793 0.724279C29.3678 0.696006 29.3508 0.670282 29.3294 0.64858C29.3079 0.626879 29.2824 0.609628 29.2542 0.597818C29.2261 0.586008 29.1959 0.579871 29.1654 0.579759H26.3661C26.3045 0.580084 26.2455 0.604871 26.2021 0.648672C26.1588 0.692474 26.1346 0.751703 26.1349 0.813337V15.4618C26.1349 15.5909 26.2387 15.696 26.3661 15.696H29.1654C29.2934 15.696 29.3964 15.5907 29.3964 15.4618V9.19604H33.7598L33.7522 15.4618C33.7522 15.5909 33.8561 15.696 33.9841 15.696H36.7902C36.9184 15.696 37.0209 15.5907 37.0212 15.4618V0.813337ZM16.6816 2.73568C16.6816 1.72757 15.8734 0.913002 14.8764 0.913002C13.8804 0.913002 13.0716 1.72757 13.0716 2.73568C13.0716 3.74255 13.8804 4.55922 14.8764 4.55922C15.8734 4.55922 16.6816 3.74255 16.6816 2.73568ZM16.4814 12.3718V5.61007C16.4816 5.54843 16.4573 5.48923 16.4139 5.44546C16.3704 5.4017 16.3114 5.37694 16.2498 5.37662H13.4593C13.3313 5.37662 13.2167 5.50868 13.2167 5.63718V15.3245C13.2167 15.6093 13.3941 15.6939 13.6238 15.6939H16.1379C16.4138 15.6939 16.4814 15.5583 16.4814 15.3201V12.3718ZM47.6598 5.39867H44.8819C44.7545 5.39867 44.6508 5.50363 44.6508 5.63286V12.8154C44.6508 12.8154 43.945 13.3318 42.9434 13.3318C41.9418 13.3318 41.6761 12.8774 41.6761 11.8966V5.63286C41.6761 5.50363 41.5725 5.39867 41.4451 5.39867H38.6257C38.4985 5.39867 38.3942 5.50363 38.3942 5.63286V12.3708C38.3942 15.2839 40.0178 15.9966 42.2514 15.9966C44.0837 15.9966 45.5609 14.9844 45.5609 14.9844C45.5609 14.9844 45.6313 15.5177 45.6631 15.581C45.695 15.6441 45.7779 15.7078 45.8675 15.7078L47.6612 15.6999C47.7883 15.6999 47.8927 15.5947 47.8927 15.4661L47.8917 5.63299C47.8917 5.50363 47.7878 5.39867 47.6598 5.39867ZM54.1565 13.323C53.193 13.2936 52.5395 12.8563 52.5395 12.8563V8.21763C52.5395 8.21763 53.1841 7.82241 53.9752 7.7517C54.9755 7.66214 55.9394 7.96434 55.9394 10.3505C55.9395 12.8669 55.5045 13.3635 54.1565 13.323ZM55.2522 5.06974C53.6745 5.06974 52.6013 5.77367 52.6013 5.77367V0.813459C52.6013 0.684104 52.498 0.579759 52.3703 0.579759H49.5631C49.5325 0.579904 49.5023 0.586065 49.4742 0.597889C49.446 0.609714 49.4205 0.626971 49.399 0.648674C49.3775 0.670377 49.3605 0.6961 49.3489 0.724374C49.3374 0.752649 49.3315 0.78292 49.3317 0.813459V15.4618C49.3317 15.5909 49.4354 15.696 49.5634 15.696H51.5113C51.5989 15.696 51.6653 15.6507 51.7143 15.5716C51.7627 15.4929 51.8326 14.8963 51.8326 14.8963C51.8326 14.8963 52.9805 15.9841 55.1535 15.9841C57.7047 15.9841 59.1677 14.6901 59.1677 10.175C59.1677 5.65972 56.8311 5.06974 55.2522 5.06974ZM24.5269 5.37539H22.4271L22.4239 2.60115C22.4239 2.49618 22.3698 2.4437 22.2484 2.4437H19.3868C19.2756 2.4437 19.2159 2.49274 19.2159 2.59955V5.46643C19.2159 5.46643 17.7819 5.81248 17.6849 5.84057C17.6365 5.85459 17.594 5.88393 17.5638 5.92419C17.5336 5.96445 17.5172 6.01345 17.5172 6.0638V7.86529C17.5172 7.99489 17.6207 8.09911 17.7487 8.09911H19.2159V12.4329C19.2159 15.652 21.4738 15.9682 22.9975 15.9682C23.6936 15.9682 24.5264 15.7447 24.6639 15.6937C24.7471 15.6632 24.7954 15.5771 24.7954 15.4837L24.7977 13.502C24.7977 13.3726 24.6886 13.2683 24.5656 13.2683C24.4433 13.2683 24.1302 13.318 23.808 13.318C22.7766 13.318 22.4271 12.8384 22.4271 12.2177L22.4269 8.09899H24.5269C24.6549 8.09899 24.7585 7.99464 24.7585 7.86516V5.60859C24.7587 5.57806 24.7528 5.5478 24.7412 5.51955C24.7297 5.49129 24.7126 5.46559 24.6911 5.44393C24.6696 5.42227 24.644 5.40506 24.6159 5.3933C24.5877 5.38154 24.5575 5.37545 24.5269 5.37539Z"

          fill="currentColor"

        />

      </g>

    </g>

    <defs>

      <clipPath id="clip0_12_56">

        <rect width="60" height="16" fill="white" />

      </clipPath>

    </defs>

  </svg>

);



const NikeLogo = ({ className }: { className?: string }) => (

  <svg

    className={className}

    viewBox="0 0 45 16"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

  >

    <g clipPath="url(#clip0_12_63)">

      <mask

        id="mask0_12_63"

        style={{ maskType: "luminance" }}

        maskUnits="userSpaceOnUse"

        x="0"

        y="0"

        width="45"

        height="16"

      >

        <path d="M44.4444 0H0V16H44.4444V0Z" fill="white" />

      </mask>

      <g mask="url(#mask0_12_63)">

        <path

          d="M5.69321 0.711516C2.80128 4.10777 0.0280227 8.31985 0.000226217 11.468C-0.0106178 12.6526 0.367757 13.6867 1.27509 14.4704C2.58028 15.5976 4.01767 15.996 5.44901 15.9978C7.54095 16.0006 9.61786 15.1566 11.2445 14.5065C13.9834 13.4109 44.2572 0.264234 44.2572 0.264234C44.5493 0.118114 44.4942 -0.0645871 44.1289 0.026695C43.9817 0.0638256 11.1708 8.95533 11.1708 8.95533C10.5375 9.13336 9.89081 9.22546 9.26104 9.22875C6.7398 9.24352 4.49604 7.84415 4.51416 4.89444C4.52109 3.74016 4.87524 2.34883 5.69321 0.711516Z"

          fill="currentColor"

        />

      </g>

    </g>

    <defs>

      <clipPath id="clip0_12_63">

        <rect width="45" height="16" fill="white" />

      </clipPath>

    </defs>

  </svg>

);



const LemonSquezyLogo = ({ className }: { className?: string }) => (

  <svg

    viewBox="0 0 122 16"

    fill="none"

    xmlns="http://www.w3.org/2000/svg"

    className={className}

  >

    <g clipPath="url(#clip0_12_70)">

      <mask

        id="mask0_12_70"

        style={{ maskType: "luminance" }}

        maskUnits="userSpaceOnUse"

        x="0"

        y="0"

        width="122"

        height="16"

      >

        <path d="M121.143 0H0V16H121.143V0Z" fill="white" />

      </mask>

      <g mask="url(#mask0_12_70)">

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M24.3337 7.91658H27.9357C27.6958 7.00149 27.0387 6.63046 26.2383 6.63046C25.3583 6.63046 24.6531 7.11012 24.3337 7.91658ZM30.049 9.48155H24.2059C24.4458 10.6602 25.4379 11.0945 26.3022 11.0945C27.3908 11.0945 27.8562 10.4432 27.8562 10.4432H29.8729C29.2642 11.993 27.8079 12.845 26.2384 12.845C24.0769 12.845 22.188 11.2485 22.188 8.83149C22.188 6.42858 24.0612 4.94058 26.1744 4.94058C28.2238 4.94058 30.3058 6.31989 30.049 9.48155Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M49.8991 8.89326C49.8991 7.68418 49.1625 6.73898 47.9461 6.73898C46.7287 6.73898 45.9125 7.68418 45.9125 8.89326C45.9125 10.1023 46.7287 11.0476 47.9461 11.0476C49.1625 11.0476 49.8991 10.1023 49.8991 8.89326ZM43.8149 8.90835C43.8149 6.42858 45.8004 4.94058 47.9461 4.94058C50.1076 4.94058 51.9965 6.44366 51.9965 8.87686C51.9965 11.3417 50.0424 12.8448 47.8811 12.8448C45.7039 12.8448 43.8149 11.3417 43.8149 8.90835Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M60.5665 8.56783V12.7679H58.4362V9.03223C58.4362 8.76852 58.6293 6.81595 57.092 6.73915C56.3386 6.69241 54.9944 7.09486 54.9944 9.12566V12.7679H52.8813V5.01758H54.8211L54.8275 6.09349C54.8275 6.09349 55.7021 4.94058 57.3333 4.94058C59.3985 4.94058 60.5665 6.42858 60.5665 8.56783Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M68.3526 6.58378C67.6801 6.58378 67.3921 6.90938 67.3921 7.23503C67.3921 7.76138 68.1126 7.91658 68.5926 8.01001C70.0189 8.30395 71.4595 8.72303 71.4595 10.3649C71.4595 11.9615 70.0983 12.845 68.4492 12.845C66.6086 12.845 65.1835 11.7608 65.0869 10.1176H67.0555C67.1041 10.582 67.4246 11.1865 68.4012 11.1865C69.2172 11.1865 69.4103 10.7689 69.4103 10.4432C69.4103 9.86898 68.8492 9.69852 68.3046 9.57481C67.3606 9.37292 65.3269 9.00195 65.3269 7.23503C65.3269 5.71555 66.8326 4.94058 68.3852 4.94058C70.1778 4.94058 71.3629 5.99446 71.4595 7.29681H69.4892C69.4258 7.03309 69.1703 6.58378 68.3526 6.58378Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M77.9322 8.89326C77.9322 7.60601 77.2111 6.84743 76.1711 6.84743C75.1945 6.84743 74.2168 7.52898 74.2168 8.89326C74.2168 10.2564 75.1945 10.9391 76.1711 10.9391C77.2111 10.9391 77.9322 10.1794 77.9322 8.89326ZM79.9334 5.01758V15.8675H77.9322V12.0081C77.4202 12.5659 76.6991 12.8448 75.8659 12.8448C73.8339 12.8448 72.1362 11.2333 72.1362 8.89326C72.1362 6.55212 73.8339 4.94058 75.8659 4.94058C77.4637 4.94058 78.0619 5.97858 78.0619 5.97858L78.0585 5.01758H79.9334Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M91.3737 7.91658H94.976C94.736 7.00149 94.0789 6.63046 93.2789 6.63046C92.3989 6.63046 91.6932 7.11012 91.3737 7.91658ZM97.0892 9.48155H91.2452C91.4863 10.6602 92.4783 11.0945 93.3423 11.0945C94.4309 11.0945 94.8966 10.4432 94.8966 10.4432H96.9132C96.3046 11.993 94.848 12.845 93.2789 12.845C91.1172 12.845 89.228 11.2485 89.228 8.83149C89.228 6.42858 91.1017 4.94058 93.2149 4.94058C95.264 4.94058 97.3463 6.31989 97.0892 9.48155Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M99.8801 7.91658H103.482C103.242 7.00149 102.585 6.63046 101.785 6.63046C100.905 6.63046 100.2 7.11012 99.8801 7.91658ZM105.596 9.48155H99.7527C99.9921 10.6602 100.984 11.0945 101.849 11.0945C102.937 11.0945 103.402 10.4432 103.402 10.4432H105.42C104.81 11.993 103.354 12.845 101.785 12.845C99.6235 12.845 97.7344 11.2485 97.7344 8.83149C97.7344 6.42858 99.6075 4.94058 101.721 4.94058C103.77 4.94058 105.852 6.31989 105.596 9.48155Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M112.724 11.0325V12.7678H105.92V11.7444L109.779 6.75417H106.08V5.01758H112.564V6.04097L108.706 11.0325H112.724Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M121.053 5.01758V11.8378V11.993C121.053 14.0855 119.948 15.9459 117.211 15.9459C114.649 15.9459 113.256 14.3177 113.256 13.0936H115.257C115.257 13.0936 115.561 14.1322 117.147 14.1322C118.492 14.1322 119.052 13.3875 119.052 12.3034V11.9778C118.699 12.3653 118.028 12.845 116.827 12.845C114.729 12.845 113.417 11.3734 113.417 9.21892L113.4 5.01758H115.481V8.75332C115.481 9.80703 115.867 11.0478 117.195 11.0478C117.883 11.0478 119.02 10.7221 119.02 8.65989V5.01758H121.053Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M20.6641 10.4413C20.6641 10.9246 20.8545 11.124 21.2078 11.124C21.4567 11.124 21.6184 11.0951 21.8543 11.0243L21.9861 12.6171C21.5453 12.7584 21.1192 12.8442 20.5755 12.8442C19.3279 12.8442 18.564 12.4467 18.564 10.7253V1.91797H20.6641V10.4413Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M42.9113 8.56783V12.7679H40.7811V9.03223C40.7811 7.96326 40.8934 6.61423 39.4201 6.73915C39.0369 6.76943 37.9796 6.93966 37.9796 9.12566V12.7679H35.8664V9.03223C35.8664 7.96326 35.9785 6.61423 34.5054 6.73915C34.1208 6.76943 33.0649 6.93966 33.0649 9.12566V12.7679H30.9517V5.01758H32.8917L32.8934 6.09349C32.8934 6.09349 33.6348 4.94057 35.0177 4.94057C36.684 4.94057 37.3381 6.19566 37.3381 6.19566C37.3381 6.19566 38.0555 4.92551 39.8855 4.92551C41.9662 4.92551 42.9113 6.41349 42.9113 8.56783Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M80.8315 9.21772V5.01758H82.9618V8.75332C82.9618 9.01697 82.7687 10.9695 84.3058 11.0464C85.0595 11.0931 86.4035 10.6906 86.4035 8.65989V5.01758H88.5167V12.7679H86.5795L86.5704 11.6921C86.5704 11.6921 85.6955 12.845 84.0647 12.845C81.9995 12.845 80.8315 11.357 80.8315 9.21772Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M3.95953 9.82034L8.25169 11.8047C8.78369 12.0508 9.15918 12.4638 9.36198 12.9375C9.87489 14.1371 9.17387 15.3639 8.07341 15.8052C6.97272 16.2462 5.79969 15.9624 5.26631 14.7149L3.39836 10.3352C3.25361 9.99571 3.61724 9.66211 3.95953 9.82034Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M4.2166 8.53576L8.64726 6.8609C10.1198 6.30427 11.7283 7.35747 11.7066 8.88776C11.7062 8.90776 11.7059 8.9277 11.7054 8.94787C11.6735 10.4381 10.1098 11.4397 8.6696 10.9125L4.2208 9.28416C3.86592 9.15433 3.8633 8.6693 4.2166 8.53576Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M3.96857 7.95566L8.32406 6.10497C9.77137 5.48992 10.1387 3.64397 9.00514 2.57739C8.99029 2.56334 8.97543 2.54946 8.9604 2.53559C7.84903 1.50404 6.01189 1.86724 5.37919 3.22594L3.4247 7.42371C3.26876 7.75846 3.6212 8.1032 3.96857 7.95566Z"

          fill="currentColor"

        />

        <path

          fillRule="evenodd"

          clipRule="evenodd"

          d="M2.84771 7.22434L4.43123 2.88237C4.62755 2.344 4.59119 1.79497 4.38822 1.32126C3.87425 0.12216 2.48234 -0.264902 1.38202 0.176995C0.281877 0.619063 -0.339783 1.62302 0.194641 2.87002L2.07483 7.24497C2.22063 7.584 2.72149 7.57063 2.84771 7.22434Z"

          fill="currentColor"

        />

      </g>

    </g>

    <defs>

      <clipPath id="clip0_12_70">

        <rect width="122" height="16" fill="white" />

      </clipPath>

    </defs>

  </svg>

);



const testimonials = [

  {

    id: "1",

    name: "Sarah Chen",

    role: "Enineer Coulmn",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-five.png",

    quote:

      "Perfect balance between flexibility and structure. Our design system went from scattered mess to cohesive asset. ROI was evident in Q1.",

    logo: (

      <ColumnLogo className="scale-80 text-white dark:text-neutral-600 opacity-60" />

    ),

  },

  {

    id: "2",

    name: "Olivia Koe",

    role: "CTO Github",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1774017708/member-six.png", // Using similar image/name for demo as per reference or variance

    quote:

      "SolaceUI transformed our design workflow. What used to take weeks now takes days, and our product consistency has never been better.",

    logo: (

      <GithubLogo className="scale-80 text-white/ dark:text-neutral-600 opacity-60" />

    ),

  },

  {

    id: "3",

    name: "Amara Okonkwo",

    role: "CTO LemonSquezy",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-three.png",

    quote:

      "We migrated our entire product in under three months. The performance improvements alone justified the switch. Highly recommend.",

    logo: (

      <LemonSquezyLogo className="scale-120 text-white dark:text-neutral-600 opacity-60" />

    ),

  },

  {

    id: "4",

    name: "David Kim",

    role: "Founder Nike",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-two.png",

    quote: "Our development velocity has doubled since adopting SolaceUI.",

    logo: (

      <NikeLogo className="scale-80 text-white dark:text-neutral-600 opacity-60" />

    ),

  },

  {

    id: "5",

    name: "James Mitchell",

    role: "Design, Nvidia",

    image: "https://res.cloudinary.com/harshitproject/image/upload/v1746774430/member-four.png",

    quote:

      "Accessibility and performance out of the box. Truly impressive work.",

    logo: (

      <NvidiaLogo className="scale-80 text-white dark:text-neutral-600 opacity-60" />

    ),

  },

];



export default function Testimonial4() {

  const [currentIndex, setCurrentIndex] = useState(1);



  const handleNext = useCallback(() => {

    setCurrentIndex((prev) => (prev + 1) % testimonials.length);

  }, []);



  const handlePrev = useCallback(() => {

    setCurrentIndex(

      (prev) => (prev - 1 + testimonials.length) % testimonials.length,

    );

  }, []);



  useEffect(() => {

    const handleKeyDown = (e: KeyboardEvent) => {

      if (e.key === "ArrowLeft") handlePrev();

      if (e.key === "ArrowRight") handleNext();

    };

    window.addEventListener("keydown", handleKeyDown);

    return () => window.removeEventListener("keydown", handleKeyDown);

  }, [handleNext, handlePrev]);



  const visibleItems = useMemo(() => {

    const total = testimonials.length;

    const leftIndex = (currentIndex - 1 + total) % total;

    const centerIndex = currentIndex;

    const rightIndex = (currentIndex + 1) % total;



    return [

      { item: testimonials[leftIndex], position: "left" },

      { item: testimonials[centerIndex], position: "center" },

      { item: testimonials[rightIndex], position: "right" },

    ];

  }, [currentIndex]);



  return (

    <section className="w-full py-20 bg-white dark:bg-background text-black dark:text-white overflow-hidden">

      <div className="max-w-7xl mx-auto px-4 relative">

        {/* Header & Actions */}

        <div className="flex flex-col md:flex-row justify-between items-end mb-12 gap-8">

          <div className="space-y-4 w-full text-center">

            <h2 className="text-4xl md:text-4xl font-bold tracking-tight">

              Trusted By The <br /> Best People

            </h2>

          </div>

        </div>



        {/* Main Carousel Area */}

        <div className="relative w-full mt-16">

          {/* Navigation Buttons (Desktop) */}

          <div className="absolute -top-12 right-0 z-40 hidden md:flex items-center gap-2">

            <button

              onClick={handlePrev}

              className="bg-black dark:bg-neutral-800 text-white p-1 hover:opacity-80 transition-opacity cursor-pointer"

              aria-label="Previous"

            >

              <ArrowLeft size={16} />

            </button>

            <button

              onClick={handleNext}

              className="bg-black dark:bg-neutral-800 text-white p-1 hover:opacity-80 transition-opacity cursor-pointer"

              aria-label="Next"

            >

              <ArrowRight size={16} />

            </button>

          </div>



          {/* Shaded Strip Background */}

          <div className="absolute -inset-y-9 md:-inset-y-12 w-full z-0 pointer-events-none">

            {/* Pattern */}

            <div className="absolute inset-0 bg-[repeating-linear-gradient(315deg,currentColor_0,currentColor_1px,transparent_0,transparent_50%)] bg-[length:10px_10px] text-neutral-200 dark:text-neutral-800 opacity-70" />

          </div>



          {/* Fades */}

          <div className="absolute left-0 -inset-y-12 w-40 bg-linear-to-r from-white via-white/50 to-transparent dark:from-background dark:via-background/50 dark:to-transparent z-30 pointer-events-none hidden md:block" />

          <div className="absolute right-0 -inset-y-12 w-40 bg-linear-to-l from-white via-white/50 to-transparent dark:from-background dark:via-background/50 dark:to-transparent z-30 pointer-events-none hidden md:block" />



          {/* Cards Container */}

          <div className="relative z-10 flex flex-col md:flex-row items-center md:items-stretch justify-center gap-6 md:gap-4 lg:gap-4 min-h-0 md:min-h-[500px]">

            <AnimatePresence mode="popLayout">

              {visibleItems.map(({ item, position }) => {

                const isCenter = position === "center";



                return (

                  <motion.div

                    key={item.id}

                    layout

                    initial={{ opacity: 0, scale: 0.9 }}

                    animate={{

                      opacity: 1,

                      scale: 1,

                    }}

                    exit={{ opacity: 0, scale: 0.9 }}

                    transition={{ duration: 0.5, type: "spring" }}

                    style={{ willChange: "transform, opacity" }}

                    className={cn(

                      "flex flex-col w-full md:flex-1 lg:flex-none lg:w-[320px] xl:w-[350px] relative bg-white dark:bg-black border border-neutral-200 dark:border-neutral-800",

                      !isCenter && "hidden md:flex",

                      isCenter

                        ? "z-20 dark:shadow-[1px_1px_50px_18px_rgba(255,255,255,0.39)] ring-1 ring-neutral-900/5"

                        : "z-10 opacity-100 md:opacity-70 md:grayscale",

                    )}

                  >

                    {/* Image Region (Top Half) */}

                    <div

                      className={cn(

                        "relative w-full aspect-[4/3] overflow-hidden bg-neutral-100 dark:bg-neutral-900",

                        !isCenter ? "md:grayscale" : "",

                      )}

                    >

                      <Image

                        src={item.image}

                        alt={item.name}

                        fill

                        sizes="(max-width: 768px) 100vw, 350px"

                        className="object-cover object-top p-1 bg-neutral-900 dark:bg-white"

                        priority={isCenter}

                      />

                    </div>



                    {/* Content Region (Bottom Half) */}

                    <div className="p-6 md:p-8 flex flex-col justify-between flex-1 bg-black text-white dark:bg-white dark:text-black">

                      <p className="text-base md:text-lg font-normal leading-relaxed mb-6 opacity-90">

                        {item.quote}

                      </p>



                      <div className="flex items-end justify-between mt-auto">

                        <div className="flex flex-col">

                          <span className="font-bold text-lg text-white dark:text-black">

                            {item.name},

                          </span>

                          <span className="text-sm text-neutral-400 dark:text-neutral-600">

                            {item.role}

                          </span>

                        </div>

                        <div className="w-1/4  md:hidden flex lg:flex items-center justify-center mb-2">

                          {item.logo}

                        </div>

                      </div>

                    </div>

                  </motion.div>

                );

              })}

            </AnimatePresence>

          </div>

        </div>



        {/* Mobile Navigation (Visible only on mobile) */}

        <div className="flex md:hidden justify-center items-start gap-4 mt-10 z-10">

          <button

            onClick={handlePrev}

            className="bg-black dark:bg-neutral-800 text-white p-1 rounded-none hover:opacity-80 transition-opacity"

            aria-label="Previous"

          >

            <ArrowLeft size={20} />

          </button>

          <button

            onClick={handleNext}

            className="bg-black dark:bg-neutral-800 text-white p-1 rounded-none hover:opacity-80 transition-opacity"

            aria-label="Next"

          >

            <ArrowRight size={20} />

          </button>

        </div>

      </div>

    </section>

  );

}
```
---
